---
layout: default
title: "SAR & Remote Sensing"
description: "Museum-style, math-first tutorials on Sentinel SAR and remote sensing for Central Asia."
---

# SAR & Remote Sensing

This gallery is about seeing landscape structure through radar logic: backscatter, slope, roughness, moisture, and temporal change. The current anchor tutorial is a full Sentinel-1 lesson focused on Central Asia.

<div class="path-grid">
  <div class="path-card">
    <div class="eyebrow">Suggested route</div>
    <h2>Read the SAR sequence in this order</h2>
    <p>Begin with image formation, then process data, then learn phase-based change, and only then move to regional interpretation.</p>
    <ol class="path-steps">
      <li><span class="path-step-num">01</span><div><a class="path-step-link" href="{{ '/tutorials/sar-remote-sensing/01-sar-fundamentals' | relative_url }}">SAR Fundamentals</a><span class="desc">Geometry, radar equation, resolution, and speckle.</span></div></li>
      <li><span class="path-step-num">02</span><div><a class="path-step-link" href="{{ '/tutorials/sar-remote-sensing/02-sentinel1-data-processing' | relative_url }}">Sentinel-1 Data Processing</a><span class="desc">Calibration, terrain correction, filtering, and units.</span></div></li>
      <li><span class="path-step-num">03</span><div><a class="path-step-link" href="{{ '/tutorials/sar-remote-sensing/03-insar-change-detection' | relative_url }}">InSAR & Change Detection</a><span class="desc">Phase, coherence, wrapping, and line-of-sight deformation.</span></div></li>
      <li><span class="path-step-num">04</span><div><a class="path-step-link" href="{{ '/tutorials/sar-remote-sensing/04-time-series-change-detection' | relative_url }}">Time-Series Analysis</a><span class="desc">Temporal statistics, seasonal decomposition, and anomaly detection.</span></div></li>
      <li><span class="path-step-num">05</span><div><a class="path-step-link" href="{{ '/tutorials/sar-remote-sensing/05-sentinel1-central-asia-unseen-patterns' | relative_url }}">Sentinel-1 Central Asia Interpretation</a><span class="desc">Use the full toolkit on regional landscape questions.</span></div></li>
    </ol>
    <a class="cta-button" href="{{ '/tutorials/sar-remote-sensing/01-sar-fundamentals' | relative_url }}">Start this route</a>
  </div>
</div>

<div class="info-box note">
  <strong>About Sentinel-1</strong>
  Sentinel-1 is an ESA Copernicus mission. It is not a NASA satellite, but NASA research and the broader radar community contribute many important methods used to interpret SAR scientifically.
</div>

<div class="section-card featured-card">
  <div class="eyebrow">Anchor lesson</div>
  <h2>Start with the Sentinel-1 interpretation tutorial</h2>
  <p><a class="featured-link" href="{{ '/tutorials/sar-remote-sensing/05-sentinel1-central-asia-unseen-patterns' | relative_url }}">How Sentinel SAR Data Can Reveal Interesting Patterns in Central Asia</a></p>
  <p>It explains what SAR can reveal indirectly, how to compare seasons and geometries, and how to think mathematically about backscatter and change. The full SAR sequence is now live below.</p>
</div>

<ul class="roadmap-list">
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/01-sar-fundamentals' | relative_url }}">SAR Fundamentals</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">Synthetic aperture, radar equation, geometry, resolution, polarization, and speckle.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/02-sentinel1-data-processing' | relative_url }}">Sentinel-1 Data Processing</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">Calibration, filtering, terrain correction, dB conversion, and export workflow.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/03-insar-change-detection' | relative_url }}">InSAR & Change Detection</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">Phase, coherence, line-of-sight displacement, and deformation logic.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/04-time-series-change-detection' | relative_url }}">Time-Series Analysis & Change Detection</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">Temporal statistics, seasonal decomposition, harmonic analysis, and anomaly detection.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/05-sentinel1-central-asia-unseen-patterns' | relative_url }}">Sentinel-1 Central Asia interpretation</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">Backscatter, moisture, geometry, temporal statistics, and 30 exercises.</span>
  </li>
</ul>
