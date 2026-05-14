---
layout: default
title: "Workshop Overview"
description: "Building Agents in Business Central — Directions Asia 2026"
---

<div class="hero">
  <h1>Building Agents in<br>Business Central</h1>
  <p>A hands-on workshop for building AI-powered agents inside Microsoft Dynamics 365 Business Central.</p>
  <a href="{{ '/stages/01-introduction/' | relative_url }}" class="btn btn-primary" style="font-size: 1rem; padding: 0.7rem 1.75rem;">
    Start Workshop &#8594;
  </a>
</div>

## Workshop Stages

Work through these stages in order — each one builds on the last.

<div class="stages-grid">
{% for stage in site.stages %}
<a href="{{ stage.url | relative_url }}" class="stage-card">
  <div class="stage-card-num">Stage {{ stage.num }}</div>
  <h3>{{ stage.icon }} {{ stage.title }}</h3>
  <p>{{ stage.description }}</p>
</a>
{% endfor %}
</div>

## About This Workshop

This workshop was presented at **Directions Asia 2026**. You will learn how to build intelligent agents that automate processes and assist users directly inside Business Central — from initial setup through to publishing.
