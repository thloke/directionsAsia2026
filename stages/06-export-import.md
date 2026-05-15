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

Agents can be exported as XML and imported back in — handy for backups, sharing configs, or promoting between environments.

---

## Part 1 — Export the Agent

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">1</div>
    <h3>Open your agent card</h3>
  </div>
  <p>Go to the <strong>Agents (preview)</strong> page and open the agent you configured in the earlier stages.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">2</div>
    <h3>Export the agent definition</h3>
  </div>
  <p>From the action bar, choose <strong>Export agent definition</strong>. BC generates and downloads an XML file with the full agent config — name, instructions, profile, permissions. Save it somewhere you can find it.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">3</div>
    <h3>Inspect the XML</h3>
  </div>
  <p>Open it in a text editor (Notepad is fine) and have a look at the structure — can you spot the instructions you wrote? The profile name? This is the portable representation of your agent.</p>
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
  <p>Select the XML file you exported in Part 1 — BC will read it and create a new agent from it.</p>
</div>

<div class="task-card">
  <div class="task-card-header">
    <div class="task-number">6</div>
    <h3>Verify the imported agent</h3>
  </div>
  <p>Open the new agent and check that the instructions, profile and permissions match the original. Run a quick task to confirm it behaves the same way.</p>
</div>

<div class="callout callout-tip">
  <div class="callout-title">💡 What this enables</div>
  This is how you move an agent between environments — sandbox to production — without recreating it by hand. The XML is readable enough to store in source control alongside your AL code too.
</div>

<div class="callout callout-tip">
  <div class="callout-title">&#10003; Stage complete when…</div>
  You have an exported XML file and a successfully imported agent that matches the original.
</div>
