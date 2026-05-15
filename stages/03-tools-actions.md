---
layout: stage
title: "Customizing the Agent's Profile"
stage_num: 3
description: "Use profile customization to control what your agent can see and access."
prev_stage: /stages/02-first-agent/
prev_title: "Your First Agent"
next_stage: /stages/04-testing/
next_title: "Testing & Debugging"
---

## Overview

An agent can only interact with pages and actions that its profile exposes. In this stage you will use the **Customize profile** tool to hide an action from the agent's Role Centre, then re-run the original task to observe how the restriction affects the agent's behaviour.

---

## Part 1 — Customize the Profile

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">1</div>
    <h3>Open the agent's design page</h3>
  </div>
  <p>Navigate to the <strong>Agents (preview)</strong> page and select your agent from Stage 2.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">2</div>
    <h3>Open profile customization</h3>
  </div>
  <p>In the agent page, select <strong>Design</strong> &gt; <strong>Customize profile</strong>. This opens the Business Central personalization experience in a new window, scoped to your agent's profile.</p>
  <div class="callout callout-info" style="margin-top:0.75rem; margin-bottom:0;">
    <div class="callout-title">&#8505; What you're seeing</div>
    The <strong>Customizing</strong> banner at the top of the new window confirms you are editing the agent's profile — not your own personal view. Changes here apply to all users (and agents) on this profile.
  </div>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">3</div>
    <h3>Hide the Customer List action</h3>
  </div>
  <p>On the Role Centre, locate the <strong>Customers</strong> action (the link that navigates to the Customer List).</p>
  <ol>
    <li>Point to the action — it will highlight with an arrowhead</li>
    <li>Select the arrowhead, then choose <strong>Hide</strong></li>
    <li>The action will appear greyed out with italic text, confirming it is hidden</li>
  </ol>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">4</div>
    <h3>Save the customization</h3>
  </div>
  <p>Select <strong>Done</strong> in the <strong>Customizing</strong> banner to save and close the personalization window.</p>
</div>

<div class="callout callout-tip">
  <div class="callout-title">&#10003; You're here when…</div>
  The customization window closes and you're back on the agent page.
</div>

---

## Part 2 — Test the Restriction

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">5</div>
    <h3>Re-run the original task</h3>
  </div>
  <p>Run a new task with the same prompt as Stage 2:</p>
  <pre><code>List the top 5 customers by name.</code></pre>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">6</div>
    <h3>Observe the result</h3>
  </div>
  <p>Watch the task pane on the right. This time the agent should be unable to navigate to the Customer List and will report that it cannot complete the task.</p>
  <p>This demonstrates how profile customization acts as a guardrail — the agent is bounded by exactly what its profile exposes.</p>
</div>

<div class="callout callout-tip">
  <div class="callout-title">&#10003; Stage complete when…</div>
  The agent fails (or returns a "cannot access" response) on the same task it completed successfully in Stage 2.
</div>
