---
layout: stage
title: "Export & Import"
stage_num: 6
description: "Export your agent definition to XML and import it into a new agent."
prev_stage: /stages/05-publishing/
prev_title: "Make It Your Own"
next_stage: /stages/07-al-agent/
next_title: "Agent in AL"
---

## Overview

Business Central agents can be exported as an XML definition file and imported into the same — or a different — environment. This is useful for backing up a working agent, sharing configurations, or promoting an agent between environments.

---

## Part 1 — Export the Agent

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">1</div>
    <h3>Open your agent card</h3>
  </div>
  <p>Navigate to <strong>Copilot & AI Capabilities</strong> and open the agent you configured in the earlier stages.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">2</div>
    <h3>Export the agent definition</h3>
  </div>
  <p>From the action bar, choose <strong>Export agent definition</strong>. Business Central will generate and download an <strong>XML file</strong> containing the full agent configuration — name, instructions, profile, permissions, and capabilities.</p>
  <p>Save the file somewhere you can find it in the next part.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">3</div>
    <h3>Inspect the XML</h3>
  </div>
  <p>Open the downloaded XML file in a text editor (Notepad is fine). Have a look at the structure — can you spot the instructions you wrote? The profile name? This is the portable representation of your agent.</p>
</div>

---

## Part 2 — Import the Agent

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">4</div>
    <h3>Start a new import</h3>
  </div>
  <p>Back in <strong>Copilot & AI Capabilities</strong>, choose the <strong>Import agent definition</strong> action (you'll find it in the same area as the create button).</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">5</div>
    <h3>Select the XML file</h3>
  </div>
  <p>When prompted, select the XML file you exported in Part 1. Business Central will read the definition and create a new agent from it.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">6</div>
    <h3>Verify the imported agent</h3>
  </div>
  <p>Open the newly imported agent and confirm that the instructions, profile, and permissions all match the original. Run a quick task to make sure it behaves the same way.</p>
</div>

<div class="callout callout-tip">
  <div class="callout-title">💡 What this enables</div>
  The export/import flow is how you can move an agent between environments — for example from a sandbox to production — without having to manually recreate the configuration. The XML file is also human-readable, so it can be stored in source control alongside your AL code.
</div>

<div class="callout callout-tip">
  <div class="callout-title">&#10003; Stage complete when…</div>
  You have an exported XML file and a successfully imported agent that matches the original.
</div>
