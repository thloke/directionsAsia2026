---
layout: stage
title: "Make It Your Own"
stage_num: 5
description: "Adapt the agent for your own product and explore what it can do."
prev_stage: /stages/04-testing/
prev_title: "Inspecting Task Logs"
next_stage: /stages/06-export-import/
next_title: "Export & Import"
---

## Overview

This is your freeform time. Take what you've built and make it relevant to **your** product. There's no single right answer here — experiment, break things, and see how far you can push the agent.

---

## Part 1 — Rewrite the Agent Instructions

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">1</div>
    <h3>Open your agent in the Designer</h3>
  </div>
  <p>Navigate to <strong>Copilot & AI Capabilities</strong> and open your agent from Stage 2 in the Agent Designer.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">2</div>
    <h3>Rewrite the instructions for your product</h3>
  </div>
  <p>Replace the placeholder instructions with something meaningful to <strong>your</strong> scenario. Think about:</p>
  <ul>
    <li>What business process does your product support?</li>
    <li>What should the agent be <em>good</em> at? What should it avoid?</li>
    <li>What tone or persona fits your product?</li>
  </ul>
  <p>Keep instructions clear and specific — the more context you give the agent, the better it performs.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">3</div>
    <h3>Run a task that makes sense for your scenario</h3>
  </div>
  <p>Give the agent a task that reflects a real workflow from your product domain. Watch the task pane and see how it reasons through the steps.</p>
</div>

---

## Part 2 — Install Your App and Extend the Agent (Optional)

<div class="callout callout-tip">
  <div class="callout-title">&#128276; This environment has AppSource access</div>
  If you have an app published to AppSource (or a per-tenant extension available), you can install it here and let the agent interact with it.
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">4</div>
    <h3>Install your app from AppSource</h3>
  </div>
  <p>Go to <strong>Extension Management</strong> and install your app, or search for it in AppSource via <strong>Microsoft AppSource</strong> in the Business Central marketplace.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">5</div>
    <h3>Update the agent instructions to reference your app</h3>
  </div>
  <p>Add context to the instructions so the agent knows your app exists and what it does. For example:</p>
  <blockquote><em>"This company uses [Your App Name] for [purpose]. When a user asks about [topic], navigate to [page] in [Your App Name] to find the answer."</em></blockquote>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">6</div>
    <h3>Give the agent a task that touches your app</h3>
  </div>
  <p>Ask the agent to do something that requires it to use a page or feature from your installed app. Check the task log entries (Stage 4) to see how it navigated there.</p>
</div>

---

<div class="callout callout-tip">
  <div class="callout-title">💡 Ideas to try</div>
  <ul>
    <li>Ask the agent to summarise data from a custom list page in your app</li>
    <li>Ask it to create a record using your app's data entry page</li>
    <li>Ask it something it <em>can't</em> do — then use the profile customisation from Stage 3 to restrict it further</li>
    <li>Compare log entries between a task that succeeded and one that failed</li>
  </ul>
</div>

<div class="callout callout-tip">
  <div class="callout-title">&#10003; Stage complete when…</div>
  You've run at least one task using your own product-specific instructions and have a feel for the agent's strengths and limitations in your domain.
</div>
