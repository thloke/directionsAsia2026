---
layout: stage
title: "Programmatic Tasks"
stage_num: 9
description: "Trigger agent tasks from page actions and business events using AL code."
prev_stage: /stages/08-setup-page/
prev_title: "Customizing the Setup Page"
next_stage: /stages/10-bring-your-own-app/
next_title: "Bring Your Own App"
---

## Overview

So far tasks have been triggered manually. In practice you'll want the agent to kick off automatically — a button click, a document posting, a status change. Here we'll wire that up using the `Agent Task Builder` API.

<div class="callout callout-tip">
  <div class="callout-title">&#128214; Docs reference</div>
  <a href="https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/ai/ai-agent-sdk-tasks" target="_blank">Managing agent tasks programmatically (preview)</a>
</div>

---

## Part 1 — Trigger a Task from a Page Action

The simplest integration: a button on a page that sends a task to your agent.

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">1</div>
    <h3>Add a page extension with an agent action</h3>
  </div>
  <p>Create a page extension with a <strong>Send to Agent</strong> action. <code>AgentUserSecurityId</code> is the <code>User Security ID</code> from your setup table — retrieve it before calling <code>Initialize</code>.</p>
<pre><code>pageextension 50200 "Sales Order Agent Action" extends "Sales Order"
{
    actions
    {
        addlast(Processing)
        {
            action(SendToAgent)
            {
                Caption = 'Send to Agent';
                Image = Task;
                ApplicationArea = All;
                ToolTip = 'Ask the agent to review this sales order.';

                trigger OnAction()
                var
                    AgentTaskBuilder: Codeunit "Agent Task Builder";
                    AgentTask: Record "Agent Task";
                    MyAgentSetup: Record "My Agent Setup";
                begin
                    MyAgentSetup.FindFirst();
                    AgentTask := AgentTaskBuilder
                        .Initialize(MyAgentSetup."User Security ID", 'Review Sales Order')
                        .AddTaskMessage(
                            'Sales Team',
                            'Please review sales order ' + Rec."No." +
                            ' for customer ' + Rec."Sell-to Customer Name")
                        .Create();

                    Message('Task created: %1', AgentTask.ID);
                end;
            }
        }
    }
}</code></pre>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">2</div>
    <h3>Publish and test the action</h3>
  </div>
  <p>Publish, open a sales order, and try the <strong>Send to Agent</strong> action. Go to the <strong>Agents (preview)</strong> page and check a task appeared. Approve and run it, then look at the log.</p>
</div>

---

## Part 2 — Skip the Approval Step

By default a new task waits for approval before the agent starts. For internal, trusted triggers you can skip that.

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">3</div>
    <h3>Set RequiresReview to false</h3>
  </div>
  <p>Update the action to use <code>Agent Task Message Builder</code> directly and call <code>SetRequiresReview(false)</code> — the agent will start as soon as the task is created.</p>
<pre><code>var
    AgentTaskBuilder: Codeunit "Agent Task Builder";
    AgentTaskMessageBuilder: Codeunit "Agent Task Message Builder";
    AgentTask: Record "Agent Task";
    MyAgentSetup: Record "My Agent Setup";
begin
    MyAgentSetup.FindFirst();

    AgentTaskMessageBuilder
        .Initialize('Sales Team',
            'Please review sales order ' + Rec."No." +
            ' for customer ' + Rec."Sell-to Customer Name")
        .SetRequiresReview(false);

    AgentTask := AgentTaskBuilder
        .Initialize(MyAgentSetup."User Security ID", 'Review Sales Order')
        .AddTaskMessage(AgentTaskMessageBuilder)
        .Create();
end;</code></pre>

<div class="callout callout-warning" style="margin-top:0.75rem;">
  <div class="callout-title">&#9888;&#65039; Only skip review for trusted sources</div>
  Never set <code>SetRequiresReview(false)</code> for messages that originate from external or user-controlled input — always validate inputs first.
</div>
</div>

---

## Part 3 — Trigger a Task from a Business Event

Instead of a button, let the system create the task automatically when something happens.

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">4</div>
    <h3>Subscribe to a posting event</h3>
  </div>
  <p>Add an event subscriber that fires after a sales invoice posts. No user action needed — the agent kicks off automatically.</p>
<pre><code>[EventSubscriber(ObjectType::Codeunit, Codeunit::"Sales-Post",
    'OnAfterSalesInvHeaderInsert', '', false, false)]
local procedure OnAfterSalesInvoicePost(var SalesInvHeader: Record "Sales Invoice Header")
var
    AgentTaskBuilder: Codeunit "Agent Task Builder";
    AgentTaskMessageBuilder: Codeunit "Agent Task Message Builder";
    MyAgentSetup: Record "My Agent Setup";
begin
    if not MyAgentSetup.FindFirst() then
        exit;

    AgentTaskMessageBuilder
        .Initialize(
            'System',
            'New invoice posted: ' + SalesInvHeader."No." +
            ' for ' + SalesInvHeader."Sell-to Customer Name" +
            '. Amount: ' + Format(SalesInvHeader.Amount))
        .SetRequiresReview(false);

    AgentTaskBuilder
        .Initialize(MyAgentSetup."User Security ID", 'Review Posted Invoice')
        .AddTaskMessage(AgentTaskMessageBuilder)
        .Create();
end;</code></pre>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">5</div>
    <h3>Post an invoice and observe the task</h3>
  </div>
  <p>Publish, then post a sales invoice. Switch to the <strong>Agents (preview)</strong> page and watch the task appear automatically. Check the log to see how it processed the details.</p>
</div>

---

## Part 4 — Track Task Lifecycle

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">6</div>
    <h3>Store the task ID alongside your record</h3>
  </div>
  <p>For proper tracking, add an <code>Agent Task ID</code> field (BigInteger) to the relevant table and save the ID on task creation:</p>
<pre><code>AgentTask := AgentTaskBuilder
    .Initialize(MyAgentSetup."User Security ID", 'Review Sales Order')
    .AddTaskMessage(AgentTaskMessageBuilder)
    .Create();

Rec."Agent Task ID" := AgentTask.ID;
Rec.Modify();</code></pre>
  <p>Later you can check its status:</p>
<pre><code>var
    AgentTaskCU: Codeunit "Agent Task";
    AgentTaskRec: Record "Agent Task";
begin
    AgentTaskRec.Get(Rec."Agent Task ID");

    if AgentTaskCU.IsTaskRunning(AgentTaskRec) then
        Message('Still running...')
    else if AgentTaskCU.IsTaskCompleted(AgentTaskRec) then
        Message('Done!');
end;</code></pre>
</div>

<div class="callout callout-tip">
  <div class="callout-title">💡 External ID vs table-based tracking</div>
  Use <strong>SetExternalId</strong> on the task builder for simple one-to-one correlations (e.g. an email thread ID). For anything more complex — multiple records per task, audit trails, querying by business entity — store the task ID in your own table as shown above.
</div>

<div class="callout callout-tip">
  <div class="callout-title">&#10003; Stage complete when…</div>
  You can trigger an agent task from a page action <em>and</em> from a business event, and you can look up the task record in AL to check whether it completed.
</div>
