---
layout: default
title: "Field Atlas"
description: "A curated field atlas for Central Asia that organizes tutorials by landscape, hazard, survey method, and interpretation task."
permalink: /field-atlas/
---

<div class="hero">
  <div class="eyebrow">Field atlas</div>
  <h1>Central Asia Through Hidden Signals</h1>
  <p class="lead">Use this atlas to enter the collection by landscape problem instead of by instrument. Each route connects terrain, sensing logic, and the tutorials most useful for that question.</p>
  <div class="hero-actions">
    <a class="cta-button" href="{{ '/tutorials/sar-remote-sensing/' | relative_url }}">Browse radar sequence</a>
    <a class="cta-button cta-secondary" href="{{ '/tutorials/treasure-hunting/' | relative_url }}">Browse field methods</a>
  </div>
</div>

<div class="atlas-grid">
  <div class="atlas-card">
    <div class="eyebrow">Landscape case</div>
    <h2>Dry channels, irrigation, and water memory</h2>
    <p>Start here if you want to read former canals, wetness retention, delta structure, or paleochannel traces across plains and oases.</p>
    <div class="atlas-tags">
      <span class="atlas-tag">Hydrology</span>
      <span class="atlas-tag">Irrigation</span>
      <span class="atlas-tag">Drylands</span>
    </div>
    <ul class="atlas-links">
      <li>
        <a href="{{ '/tutorials/sar-remote-sensing/05-sentinel1-central-asia-unseen-patterns' | relative_url }}">Sentinel-1 Central Asia Interpretation</a>
        <span class="desc">Best regional starting point for wetness and subtle landscape structure.</span>
      </li>
      <li>
        <a href="{{ '/tutorials/treasure-hunting/01-electromagnetic-survey-central-asia-unseen-conductivity' | relative_url }}">Electromagnetic Survey Methods</a>
        <span class="desc">Useful when conductivity contrast from water or salts matters on the ground.</span>
      </li>
      <li>
        <a href="{{ '/tutorials/sar-remote-sensing/02-sentinel1-data-processing' | relative_url }}">Sentinel-1 Data Processing</a>
        <span class="desc">Calibrate and terrain-correct repeated scenes before comparing seasonal behavior.</span>
      </li>
    </ul>
  </div>

  <div class="atlas-card">
    <div class="eyebrow">Landscape case</div>
    <h2>Mountain fronts, slope risk, and deformation</h2>
    <p>Use this route for landslides, unstable roads, fault-controlled landforms, glacier margins, and motion in high-relief terrain.</p>
    <div class="atlas-tags">
      <span class="atlas-tag">Mountains</span>
      <span class="atlas-tag">Hazards</span>
      <span class="atlas-tag">Deformation</span>
    </div>
    <ul class="atlas-links">
      <li>
        <a href="{{ '/tutorials/sar-remote-sensing/01-sar-fundamentals' | relative_url }}">SAR Fundamentals</a>
        <span class="desc">Learn layover, shadow, incidence angle, and why terrain dominates interpretation.</span>
      </li>
      <li>
        <a href="{{ '/tutorials/sar-remote-sensing/03-insar-change-detection' | relative_url }}">InSAR & Change Detection</a>
        <span class="desc">Best entry point for motion, coherence loss, and line-of-sight displacement.</span>
      </li>
      <li>
        <a href="{{ '/tutorials/sar-remote-sensing/05-sentinel1-central-asia-unseen-patterns' | relative_url }}">Sentinel-1 Central Asia Interpretation</a>
        <span class="desc">Use the interpretation workflow to test geometry versus physical change.</span>
      </li>
    </ul>
  </div>

  <div class="atlas-card">
    <div class="eyebrow">Site case</div>
    <h2>Buried geometry and archaeological prospection</h2>
    <p>Enter here if the question is whether hidden walls, ditches, mound structure, or engineered surfaces may exist beneath the ground.</p>
    <div class="atlas-tags">
      <span class="atlas-tag">Archaeology</span>
      <span class="atlas-tag">Geometry</span>
      <span class="atlas-tag">Survey design</span>
    </div>
    <ul class="atlas-links">
      <li>
        <a href="{{ '/tutorials/treasure-hunting/02-ground-penetrating-radar-central-asia' | relative_url }}">Ground-Penetrating Radar</a>
        <span class="desc">Best for shallow geometry, hyperbolas, and depth estimation.</span>
      </li>
      <li>
        <a href="{{ '/tutorials/treasure-hunting/01-electromagnetic-survey-central-asia-unseen-conductivity' | relative_url }}">Electromagnetic Survey Methods</a>
        <span class="desc">Use when buried fills or moisture-retaining features create conductivity contrast.</span>
      </li>
      <li>
        <a href="{{ '/tutorials/treasure-hunting/04-metal-detection-fundamentals-central-asia' | relative_url }}">Metal Detection Fundamentals</a>
        <span class="desc">Useful for localized conductive target patterns and close-range field screening.</span>
      </li>
    </ul>
  </div>

  <div class="atlas-card">
    <div class="eyebrow">Material case</div>
    <h2>Craft, metallurgy, and material signatures</h2>
    <p>Follow this route when the central question is what a sample, fragment, slag scatter, or soil enrichment is made of.</p>
    <div class="atlas-tags">
      <span class="atlas-tag">Materials</span>
      <span class="atlas-tag">Metallurgy</span>
      <span class="atlas-tag">Composition</span>
    </div>
    <ul class="atlas-links">
      <li>
        <a href="{{ '/tutorials/treasure-hunting/03-xrf-central-asia-material-signatures' | relative_url }}">X-Ray Fluorescence Field Analysis</a>
        <span class="desc">Best for elemental fingerprints and composition-based comparison.</span>
      </li>
      <li>
        <a href="{{ '/tutorials/treasure-hunting/04-metal-detection-fundamentals-central-asia' | relative_url }}">Metal Detection Fundamentals</a>
        <span class="desc">Useful for finding conductive targets before closer composition work.</span>
      </li>
      <li>
        <a href="{{ '/tutorials/treasure-hunting/01-electromagnetic-survey-central-asia-unseen-conductivity' | relative_url }}">Electromagnetic Survey Methods</a>
        <span class="desc">Helpful when site-scale conductive residues or saline process zones matter.</span>
      </li>
    </ul>
  </div>
</div>

<div class="section-card featured-card">
  <div class="eyebrow">How to use this atlas</div>
  <h2>Choose the question first, then the instrument</h2>
  <ul class="tutorial-list">
    <li><strong>Landscape behavior</strong><span class="desc">Start with SAR when the question is broad, repeated, or regional.</span></li>
    <li><strong>Near-surface contrast</strong><span class="desc">Use EM or metal detection when conductivity and target response matter most.</span></li>
    <li><strong>Subsurface shape</strong><span class="desc">Use GPR when buried geometry is the main unknown.</span></li>
    <li><strong>Material identity</strong><span class="desc">Use XRF when composition is the key evidence layer.</span></li>
  </ul>
</div>
