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

When your agent is deployed as an AL extension, every environment it lands in might need a slightly different configuration — a different company name in the instructions, a different API endpoint, a different tone. Hardcoding these values in AL means a recompile every time. A custom setup page lets administrators configure the agent at install time without touching code.

In this stage you'll add a custom field to your agent's setup page and inject its value dynamically into the agent's instructions.

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
  <p>Open your agent's setup table (the one with <code>User Security ID</code> as the primary key). Add a new field to hold the per-environment value — for example, a short description of what product this agent is supporting.</p>

```al
field(20; "Product Description"; Text[250])
{
    Caption = 'Product Description';
    DataClassification = CustomerContent;
}
```
</div>

---

## Part 2 — Expose the Field on the Setup Page

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">2</div>
    <h3>Add the field to the setup page layout</h3>
  </div>
  <p>Open your setup page. Below the standard <code>Agent Setup Part</code>, add a group with your new field. Setting <code>IsUpdated := true</code> on validate ensures the <strong>Update</strong> button becomes active when the user changes the value.</p>

```al
group(AdditionalConfiguration)
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
}
```
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">3</div>
    <h3>Save the custom field in SaveCustomProperties</h3>
  </div>
  <p>In your <code>SaveCustomProperties</code> procedure, make sure the new field is written to the database alongside the existing ones.</p>

```al
MyAgentSetupRecord."Product Description" := TempMyAgentSetup."Product Description";
```
</div>

---

## Part 3 — Inject the Value into the Instructions

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">4</div>
    <h3>Switch from static to dynamic instructions</h3>
  </div>
  <p>In your <code>SaveSetupRecord</code> procedure, replace the static instructions call with one that reads the custom field and merges it in. For example:</p>

```al
local procedure GetInstructions(ProductDescription: Text): Text
var
    InstructionsTxt: Label 'You are an agent for a Business Central customer.\\Product context: %1\\Your role is to help users navigate and operate the system efficiently.', Comment = '%1 = product description';
begin
    exit(StrSubstNo(InstructionsTxt, ProductDescription));
end;
```

  <p>Then pass the value when calling <code>Agent.SetInstructions</code>:</p>

```al
if IsNewAgent then
    Agent.SetInstructions(TempMyAgentSetup."User Security ID",
        GetInstructions(TempMyAgentSetup."Product Description"));
```
</div>

---

## Part 4 — Test the Setup Page End-to-End

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">5</div>
    <h3>Publish and delete the existing agent instance</h3>
  </div>
  <p>Publish the updated extension (<kbd>F5</kbd>). In <strong>Copilot & AI Capabilities</strong>, delete the agent instance you created in Stage 7 so you can create a fresh one through the setup dialog.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">6</div>
    <h3>Create a new instance and fill in the custom field</h3>
  </div>
  <p>Create a new instance of your agent. When the setup dialog opens you should see your new <strong>Product Description</strong> field. Enter something descriptive, then click <strong>Update</strong>.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">7</div>
    <h3>Verify the instructions reflect the configured value</h3>
  </div>
  <p>Open the agent card and check the <strong>Instructions</strong> field — your product description should appear in the text. Run a task and confirm the agent behaves accordingly.</p>
</div>

<div class="callout callout-tip">
  <div class="callout-title">💡 Why this pattern matters</div>
  The setup page is the only place a customer-facing admin interacts with your agent's internals. Keeping the instructions template in AL and the variable parts in the setup table means you ship one <code>.app</code> file that works correctly in every environment — no per-customer forks required.
</div>

<div class="callout callout-tip">
  <div class="callout-title">&#10003; Stage complete when…</div>
  Your setup page shows the custom field, a new agent instance picks up the value, and the instructions stored on the agent contain the text you entered during setup.
</div>
