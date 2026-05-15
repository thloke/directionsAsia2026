---
layout: stage
title: "Your First Agent"
stage_num: 2
description: "Create a UI-driven custom agent and trigger its first task — no code required."
prev_stage: /stages/01-introduction/
prev_title: "Introduction & Setup"
next_stage: /stages/03-tools-actions/
next_title: "Tools & Actions"
---

## Overview

In this stage you will create a custom agent entirely through the Business Central UI — no AL code needed. By the end you will have a live, active agent and have triggered it to run its first task.

<div class="callout callout-info">
  <div class="callout-title">&#8505; Before You Start</div>
  Your environment has already been set up with the <strong>Custom Agent</strong> capability enabled and the <strong>AGENT - ADMIN</strong> permission set assigned to your user. If the agent icon is missing from the navigation bar, let your facilitator know.
</div>

---

## Part 1 — Create the Agent

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">1</div>
    <h3>Open the agent designer</h3>
  </div>
  <p>In the top-right navigation bar of your Role Centre, look for the <strong>+</strong> (plus) icon. Select it, then choose <strong>Agent</strong> &gt; <strong>Create</strong>.</p>
  <p><img src="{{ '/assets/images/create_icon.png' | relative_url }}" alt="The plus icon used to create a new agent" style="max-width:100%; border:1px solid var(--border); border-radius:var(--radius-lg); margin-top:0.75rem;" /></p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">2</div>
    <h3>Choose a starting point</h3>
  </div>
  <p>In the <strong>Create agent</strong> wizard, select <strong>Create agent from scratch</strong>, then choose <strong>Next</strong>.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">3</div>
    <h3>Give your agent an identity</h3>
  </div>
  <p>Fill in the following fields:</p>
  <ul>
    <li><strong>Name</strong> — a short internal name, e.g. <code>WorkshopAgent01</code></li>
    <li><strong>Display Name</strong> — what users will see, e.g. <code>My First Agent</code></li>
    <li><strong>Initials</strong> — suggested automatically, but feel free to change it</li>
    <li><strong>Description</strong> — optional; briefly describe what the agent does</li>
  </ul>
  <p>Select the arrow to continue to the next page.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">4</div>
    <h3>Set permissions</h3>
  </div>
  <p>Under <strong>Permissions</strong>, select <strong>Manage permissions</strong> and add the <code>SUPER</code> permission set. This gives the agent access to everything in the environment for the purposes of this workshop.</p>
  <div class="callout callout-warning" style="margin-top:0.75rem; margin-bottom:0;">
    <div class="callout-title">&#9888; Workshop Only</div>
    Using SUPER is fine for a sandbox workshop. In production you would assign only the minimum permissions the agent needs.
  </div>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">5</div>
    <h3>Write the agent's instructions</h3>
  </div>
  <p>Under <strong>Instructions for the agent</strong>, select <strong>Edit instructions</strong> and enter the following:</p>
  <pre><code>You are a helpful Business Central assistant.
When given a task, look up the requested information
and provide a clear, concise summary to the user.</code></pre>
  <p>Keep instructions short and specific — you will refine them in later stages.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">6</div>
    <h3>Activate and save</h3>
  </div>
  <p>Turn on the <strong>Active</strong> toggle, then select <strong>Update</strong> to complete the setup.</p>
  <p>The agent icon in the navigation bar will change appearance to show the agent is now active.</p>
</div>

<div class="callout callout-tip">
  <div class="callout-title">&#10003; You're here when…</div>
  The agent icon in the top navigation changes and your agent appears in the <strong>Agents (preview)</strong> page with a status of <strong>Active</strong>.
</div>

---

## Part 2 — Trigger Your First Task

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">7</div>
    <h3>Open Agent Tasks</h3>
  </div>
  <p>Use the search (<strong>Tell me</strong>) to open the <strong>Agent Tasks</strong> page, or navigate via the agent icon in the navigation bar and select your agent.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">8</div>
    <h3>Run a task</h3>
  </div>
  <p>Select the <strong>Run task</strong> action. In the dialog, enter a simple task for the agent — for example:</p>
  <pre><code>List the top 5 customers by name.</code></pre>
  <p>Confirm and watch the agent pick up and process the task.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">9</div>
    <h3>Review the result</h3>
  </div>
  <p>Once the task completes, open the task entry to see the agent's output and the steps it took to get there. This task log is your main tool for understanding and improving your agent's behaviour.</p>
</div>

<div class="callout callout-tip">
  <div class="callout-title">&#10003; Stage complete when…</div>
  Your agent has processed a task and you can see its output in the task log.
</div>
