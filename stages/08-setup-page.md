---
layout: stage
title: "Customizing the Setup Page"
stage_num: 8
description: "Add per-environment configuration to your agent using a custom setup page."
prev_stage: /stages/07-al-agent/
prev_title: "Agent in AL"
next_stage: /stages/09-programmatic-tasks/
next_title: "Programmatic Tasks"
---

## Overview

Once your agent is deployed as an extension, different environments will likely need different configuration — company name, tone, maybe an API endpoint. Hardcoding those means a recompile each time. The setup page solves that — admins can change values without touching code.

Here we'll add a custom field and inject it into the instructions dynamically.

<div class="callout callout-tip">
  <div class="callout-title">&#128214; Docs reference</div>
  <a href="https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/ai/ai-agent-sdk-setup-page" target="_blank">Agent setup pages (preview)</a>
</div>

---

## Part 1 — Add a Custom Field to the Setup Table

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">1</div>
    <h3>Add a field to your setup table</h3>
  </div>
  <p>Open your agent's setup table (the one with <code>User Security ID</code> as the primary key) and add a field for the per-environment value. For example, a description of what product the agent is supporting.</p>
<pre><code>field(20; "Product Description"; Text[250])
{
    Caption = 'Product Description';
    DataClassification = CustomerContent;
}</code></pre>
</div>

---

## Part 2 — Expose the Field on the Setup Page

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">2</div>
    <h3>Add the field to the setup page layout</h3>
  </div>
  <p>Open the setup page and add a group for your new field below the standard <code>Agent Setup Part</code>. Setting <code>IsUpdated := true</code> on validate activates the Update button when the value changes.</p>
<pre><code>group(AdditionalConfiguration)
{
    Caption = 'Additional Configuration';

    field(ProductDescription; Rec."Product Description")
    {
        ApplicationArea = All;
        ToolTip = 'Describe the product this agent supports. This is injected into the agent instructions.';

        trigger OnValidate()
        begin
            IsUpdated := true;
        end;
    }
}</code></pre>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">3</div>
    <h3>Save the custom field in SaveCustomProperties</h3>
  </div>
  <p>In <code>SaveCustomProperties</code>, make sure the new field gets written to the database alongside the existing ones.</p>
<pre><code>MyAgentSetupRecord."Product Description" := TempMyAgentSetup."Product Description";</code></pre>
</div>

---

## Part 3 — Inject the Value into the Instructions

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">4</div>
    <h3>Switch from static to dynamic instructions</h3>
  </div>
  <p>In <code>SaveSetupRecord</code>, replace the static instructions with a dynamic version that reads the custom field and merges it in:</p>
<pre><code>local procedure GetInstructions(ProductDescription: Text): Text
var
    InstructionsTxt: Label 'You are an agent for a Business Central customer.\\Product context: %1\\Your role is to help users navigate and operate the system efficiently.', Comment = '%1 = product description';
begin
    exit(StrSubstNo(InstructionsTxt, ProductDescription));
end;</code></pre>
  <p>Then pass the value when calling <code>Agent.SetInstructions</code>:</p>
<pre><code>if IsNewAgent then
    Agent.SetInstructions(TempMyAgentSetup."User Security ID",
        GetInstructions(TempMyAgentSetup."Product Description"));</code></pre>
</div>

---

## Part 4 — Test the Setup Page End-to-End

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">5</div>
    <h3>Publish and delete the existing agent instance</h3>
  </div>
  <p>Publish (<kbd>F5</kbd>), then delete the agent instance from Stage 7 on the <strong>Agents (preview)</strong> page so you can create a fresh one through the setup dialog.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">6</div>
    <h3>Create a new instance and fill in the custom field</h3>
  </div>
  <p>Create a new instance. When the setup dialog opens you should see your <strong>Product Description</strong> field — fill it in and click <strong>Update</strong>.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">7</div>
    <h3>Verify the instructions reflect the configured value</h3>
  </div>
  <p>Open the agent card and check that your description appears in the <strong>Instructions</strong> field. Run a task to make sure it's behaving accordingly.</p>
</div>

<div class="callout callout-tip">
  <div class="callout-title">💡 Why this pattern matters</div>
  The setup page is the only place an admin touches your agent's config. Keep the instructions template in AL, the variable parts in the setup table, and you ship one <code>.app</code> that works everywhere — no per-customer forks.
</div>

<div class="callout callout-tip">
  <div class="callout-title">&#10003; Stage complete when…</div>
  Your setup page shows the custom field, a new agent instance picks up the value, and the instructions stored on the agent contain the text you entered during setup.
</div>
