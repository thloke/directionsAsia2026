---
layout: default
title: "Workshop Overview"
description: "Building Agents in Business Central — Directions Asia 2026"
---

<div class="hero">
  <div style="display:flex; align-items:center; justify-content:center; gap:2.5rem; flex-wrap:wrap;">
    <div>
      <h1>Building Agents in<br>Business Central</h1>
      <p>A hands-on workshop for building AI-powered agents inside Microsoft Dynamics 365 Business Central.</p>
      <a href="{{ '/stages/01-introduction/' | relative_url }}" class="btn btn-primary" style="font-size: 1rem; padding: 0.7rem 1.75rem;">
        Start Workshop &#8594;
      </a>
    </div>
    <div style="text-align:center; flex-shrink:0;">
      <img src="{{ '/assets/images/workshop_qr.png' | relative_url }}" alt="QR code linking to workshop instructions" style="width:150px; height:150px; background:#fff; border-radius:8px; padding:6px;" />
      <div style="font-size:0.75rem; margin-top:0.4rem; opacity:0.8;">Scan for workshop instructions</div>
      <div style="font-size:0.85rem; margin-top:0.5rem; font-family:monospace; word-break:break-all; opacity:0.9;">thloke.github.io/directionsAsia2026</div>
    </div>
  </div>
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
