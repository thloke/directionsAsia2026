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

A UI-built agent lives only in the environment where it was created.Moving it to AL lets you version-control it, deploy it to any environment, and eventually distribute it via AppSource. In this stage you'll use the exported XML from Stage 6 as the source of truth and recreate the same agent as a proper AL extension.

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
  <p>In Visual Studio Code, press <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> and run <strong>AL: New Project</strong>. When prompted to choose a template, select <strong>Agent</strong>.</p>
  <p>Give the project a name (e.g. <code>MyWorkshopAgent</code>) and choose a folder for it.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">2</div>
    <h3>Set the runtime version in app.json</h3>
  </div>
  <p>Open <code>app.json</code> in the project root. Find the <code>"runtime"</code> field and set it to <code>"17.0"</code> — this is required for the Agent SDK interfaces to be available.</p>
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
  <p>Open <code>.vscode/launch.json</code>. Update the configuration to target your workshop environment — you'll need the environment name from your printed handout.</p>
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
  <p>Replace <code>&lt;your-environment-name&gt;</code> with the environment name from your handout (the same one used in the URL: <code>businesscentral.dynamics.com/&lt;environmentname&gt;</code>).</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">4</div>
    <h3>Download symbols</h3>
  </div>
  <p>Press <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> and run <strong>AL: Download Symbols</strong>. VS Code will connect to your environment and pull down the symbol packages needed to compile against it. You'll be prompted to sign in if you haven't already.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">5</div>
    <h3>Explore the generated structure</h3>
  </div>
  <p>The template generates the skeleton of a working agent. The key files are:</p>
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

Open the XML file you exported in Stage 6 alongside VS Code — it is your reference for the values in the following steps.

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">6</div>
    <h3>Copy the instructions into the resource file</h3>
  </div>
  <p>Find the instructions text inside the exported XML and paste it into the <strong>instructions .txt resource file</strong> generated by the template, replacing the placeholder text.</p>
  <p>The <code>IAgentFactory</code> implementation loads this file and applies it when a new agent instance is created.</p>
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
  <p>In the <strong>AgentFactory</strong> codeunit, find the <code>GetDefaultAccessControls</code> method. Add the permission sets listed in the XML (the template uses <code>D365 BASIC</code> as a placeholder — replace or extend it to match).</p>
  <p>For the workshop agent you set up SUPER in Stage 2 — for production use a least-privilege set instead.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">9</div>
    <h3>Update the display name and initials</h3>
  </div>
  <p>In the <strong>Setup page</strong>, update the label constants for display name and initials to match your agent. These are the values shown in the BC UI when the agent is active.</p>
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
  <p>In Business Central, navigate to <strong>Copilot & AI Capabilities</strong>. You should see your new agent type listed. Select it and follow the setup dialog to create an instance — it will apply the profile and permissions you defined in AL automatically.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">12</div>
    <h3>Run the same task and compare</h3>
  </div>
  <p>Give your AL agent the same task you ran against the UI-built agent in Stage 2. Watch the task pane and check the log entries. The behaviour should be identical — the agent is simply defined in code now rather than the UI.</p>
</div>

<div class="callout callout-tip">
  <div class="callout-title">💡 Why this matters</div>
  Once your agent is in AL you can commit it to source control, run it through a CI/CD pipeline, promote it across environments with a <code>.app</code> file, and eventually submit it to AppSource — all without ever touching the Business Central UI.
</div>

<div class="callout callout-tip">
  <div class="callout-title">&#10003; Stage complete when…</div>
  Your AL extension is published, an agent instance is created from it, and you've successfully run a task that matches the behaviour of the UI-built agent from Stage 2.
</div>
