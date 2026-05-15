---
layout: stage
title: "Agent in AL"
stage_num: 7
description: "Convert your agent to AL code so it can be packaged and distributed as an extension."
prev_stage: /stages/06-export-import/
prev_title: "Export & Import"
next_stage: /stages/08-setup-page/
next_title: "Customizing the Setup Page"
---

## Overview

<div class="callout callout-warning">
  <div class="callout-title">&#128104;&#8205;&#128187; Developer track starts here</div>
  From this stage onwards the workshop shifts to writing AL code. If you're not a developer — or you'd simply like more time to experiment with your agent in the Business Central UI — <strong>that's completely fine!</strong> Head back to <a href="{{ '/stages/05-publishing/' | relative_url }}">Stage 5</a> and keep iterating on your agent's instructions and profile. There's no obligation to continue beyond this point.
</div>

A UI-built agent only lives in the environment where it was created. Moving it to AL means you can version control it, deploy to any environment, and eventually distribute it via AppSource. Use the XML from Stage 6 as your reference — you'll recreate the same agent as a proper AL extension.

<div class="callout callout-tip">
  <div class="callout-title">&#128214; Docs reference</div>
  <a href="https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/ai/ai-agent-sdk-overview" target="_blank">Coding agents in AL — Overview</a><br/>
  <a href="https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/ai/ai-agent-sdk-convert-agent" target="_blank">Convert an exported agent to AL</a>
</div>

---

## Part 1 — Create the AL Project

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">1</div>
    <h3>Create a new AL project using the Agent template</h3>
  </div>
  <p>In VS Code, hit <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> and run <strong>AL: New Project</strong>. When prompted for a template, pick <strong>Agent</strong>.</p>
  <p>Name it something like <code>MyWorkshopAgent</code> and choose a folder.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">2</div>
    <h3>Set the runtime version in app.json</h3>
  </div>
  <p>Open <code>app.json</code> and set <code>"runtime"</code> to <code>"17.0"</code> — you need this for the Agent SDK interfaces to be available.</p>
<pre><code>{
  "id": "...",
  "name": "MyWorkshopAgent",
  "runtime": "17.0",
  ...
}</code></pre>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">3</div>
    <h3>Configure launch.json to point at your environment</h3>
  </div>
  <p>Open <code>.vscode/launch.json</code> and update it to target your environment — grab the environment name and tenant ID from your handout.</p>
<pre><code>{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "al",
      "request": "launch",
      "name": "My Workshop Environment",
      "environmentType": "Sandbox",
      "environmentName": "&lt;your-environment-name&gt;",
      "tenant": "&lt;your-tenant-id&gt;",
      "authentication": "UserPassword"
    }
  ]
}</code></pre>
  <p>Replace the placeholder values with what's on your handout (same environment name as the URL).</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">4</div>
    <h3>Download symbols</h3>
  </div>
  <p>Run <strong>AL: Download Symbols</strong> from the command palette. VS Code will connect to your environment and pull down the symbols needed to compile. You'll be asked to sign in if needed.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">5</div>
    <h3>Explore the generated structure</h3>
  </div>
  <p>The template gives you a full working skeleton. Key files:</p>
  <ul>
    <li><strong>AgentMetadataProvider enum extension</strong> — registers your agent type</li>
    <li><strong>AgentFactory codeunit</strong> — implements <code>IAgentFactory</code> (default profile, permissions, setup page)</li>
    <li><strong>AgentMetadata codeunit</strong> — implements <code>IAgentMetadata</code> (display name, initials, summary)</li>
    <li><strong>AgentTaskExecution codeunit</strong> — implements <code>IAgentTaskExecution</code></li>
    <li><strong>Instructions .txt resource file</strong> — the plain-text instructions for your agent</li>
    <li><strong>Setup page</strong> — the configuration dialog shown when creating an instance</li>
    <li><strong>Install codeunit</strong> — registers the Copilot capability on installation</li>
  </ul>
</div>

---

## Part 2 — Adapt the Template to Match Your Exported Agent

Keep the XML from Stage 6 open alongside VS Code — you'll use it as a reference for the next few steps.

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">6</div>
    <h3>Copy the instructions into the resource file</h3>
  </div>
  <p>Find the instructions text in the XML and paste it into the <strong>instructions .txt resource file</strong> generated by the template, replacing the placeholder. The <code>IAgentFactory</code> loads this on agent creation.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">7</div>
    <h3>Set the default profile</h3>
  </div>
  <p>In the <strong>AgentFactory</strong> codeunit, find the <code>GetDefaultProfile</code> method. Update the <code>Profile ID</code> to match the profile from your exported agent (e.g. <code>'BUSINESS MANAGER'</code>).</p>
<pre><code>procedure GetDefaultProfile(var TempAllProfile: Record "All Profile" temporary)
begin
    TempAllProfile."Profile ID" := 'BUSINESS MANAGER';
    TempAllProfile."App ID" := SystemApplicationAppId;
    TempAllProfile.Insert();
end;</code></pre>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">8</div>
    <h3>Set the default permissions</h3>
  </div>
  <p>In <strong>AgentFactory</strong>, update <code>GetDefaultAccessControls</code> with the permission sets from the XML. The template has <code>D365 BASIC</code> as a placeholder — swap it out. We used SUPER for the workshop, but you'd normally go least-privilege in production.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">9</div>
    <h3>Update the display name and initials</h3>
  </div>
  <p>In the <strong>Setup page</strong>, update the display name and initials to match your agent.</p>
</div>

---

## Part 3 — Publish and Verify

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">10</div>
    <h3>Publish the extension</h3>
  </div>
  <p>Press <kbd>F5</kbd> (or run <strong>AL: Publish</strong>) to compile and deploy the extension to Business Central.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">11</div>
    <h3>Create an instance of your AL agent</h3>
  </div>
  <p>Go to the <strong>Agents (preview)</strong> page — your agent type should be listed. Select it and run through the setup dialog. It'll apply the profile and permissions from your AL code automatically.</p>
</div>

<div class="callout callout-info">
  <div class="callout-title">&#8505; AL agents and the task designer</div>
  Unlike UI-built agents, AL agents can't be triggered from the task designer in the UI. Programmatic task creation is covered in Stage 9 — that's how you'll run tasks against this agent.
</div>

<div class="callout callout-tip">
  <div class="callout-title">💡 Why this matters</div>
  Once it's in AL you can commit to source control, run it through CI/CD, promote it with a <code>.app</code> file, and eventually submit to AppSource — no BC UI needed.
</div>

<div class="callout callout-tip">
  <div class="callout-title">&#10003; Stage complete when…</div>
  Your AL extension is published and an agent instance is created from it on the Agents (preview) page.
</div>
