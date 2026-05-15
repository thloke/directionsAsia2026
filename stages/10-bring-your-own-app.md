---
layout: stage
title: "Bring Your Own App"
stage_num: 10
description: "Integrate everything you've built with your own AL extension."
prev_stage: /stages/09-programmatic-tasks/
prev_title: "Programmatic Tasks"
---

## Overview

Your sandbox. Agent in AL, setup page for config, programmatic task creation — now point it at your own app.

No set path here — find a meaningful integration point and go.

---

## Getting Started

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">1</div>
    <h3>Add your app as a dependency</h3>
  </div>
  <p>In <code>app.json</code>, add your app as a dependency:</p>

```json
"dependencies": [
  {
    "id": "<your-app-id>",
    "name": "<Your App Name>",
    "publisher": "<Your Publisher>",
    "version": "1.0.0.0"
  }
]
```

  <p>Then run <strong>AL: Download Symbols</strong> to pull in your app's symbols so you can reference its objects directly.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">2</div>
    <h3>Pick an integration point</h3>
  </div>
  <p>Think about where the agent adds most value in your workflow. Some ideas:</p>
  <ul>
    <li><strong>Action on a key page</strong> — add a "Send to Agent" button on your app's main list or card page, passing relevant record context in the task message</li>
    <li><strong>Business event trigger</strong> — subscribe to an event your app already raises (e.g. a custom document being posted or a status change) and auto-create a task</li>
    <li><strong>Setup page field</strong> — expose a config value from your app in the agent setup page so the agent knows about your app's data structure</li>
    <li><strong>Profile customization</strong> — use the agent's profile to hide or show pages from your app, scoping what the agent can see</li>
  </ul>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">3</div>
    <h3>Write the task message with useful context</h3>
  </div>
  <p>The richer the task message, the better the agent performs. Include record IDs, relevant field values, and a clear instruction:</p>

```al
AgentTaskMessageBuilder
    .Initialize(
        'System',
        'A new ' + Rec.TableCaption() + ' has been created: ' + Rec."No." +
        '. Description: ' + Rec.Description +
        '. Please review and take appropriate action.')
    .SetRequiresReview(false);
```
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">4</div>
    <h3>Update the agent instructions to know about your app</h3>
  </div>
  <p>Add context about your app's pages and data to the agent instructions — via the setup page field from Stage 8, or directly in the resource file. The more it understands your app's structure, the more accurately it navigates.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">5</div>
    <h3>Publish, trigger, and inspect</h3>
  </div>
  <p>Publish both extensions, trigger your integration point, and watch it work. Use the task log from Stage 4 to see what pages it hit and what decisions it made. Iterate on the instructions and message until it handles the workflow reliably.</p>
</div>

---

<div class="callout callout-tip">
  <div class="callout-title">💡 Things worth experimenting with</div>
  <ul>
    <li>Give the agent an ambiguous task and see how it interprets it — then tighten the instructions</li>
    <li>Intentionally restrict the agent's profile (Stage 3 approach) so it can't access a page — observe the failure in the task log</li>
    <li>Add a second custom setup field to hold something app-specific (an account code, a series number) and inject it into the task message dynamically</li>
    <li>Store the task ID on your record and surface its status back in your app's UI</li>
  </ul>
</div>

<div class="callout callout-tip">
  <div class="callout-title">&#10003; Stage complete when…</div>
  You've triggered at least one agent task that touches a page or record from your own app, and the task log shows the agent successfully navigating your extension's UI.
</div>
