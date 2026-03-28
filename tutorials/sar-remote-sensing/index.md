---
layout: default
title: "SAR & Remote Sensing"
---

# 🛰️ SAR & Remote Sensing

Deep-dive into Synthetic Aperture Radar (SAR) technology and learn to leverage free Sentinel-1 satellite data for geospatial analysis. These tutorials cover the physics of microwave remote sensing, data processing pipelines, interferometric techniques, and creative applications for Kyrgyzstan.

<div class="info-box note">
  <strong>🛰️ About Sentinel-1</strong>
  Sentinel-1 is a two-satellite constellation operated by ESA as part of the Copernicus programme. It provides free, open-access C-band SAR data with a 6-12 day repeat cycle, making it ideal for monitoring changes across Central Asia's vast and remote landscapes.
</div>

<ul class="tutorial-list">
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/01-sar-fundamentals' | relative_url }}">1. SAR Fundamentals</a>
    <span class="desc">The synthetic aperture concept, radar equation derivation, range & azimuth resolution, speckle noise theory, polarimetric decomposition, and SAR imaging geometry.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/02-sentinel1-data-processing' | relative_url }}">2. Sentinel-1 Data Processing</a>
    <span class="desc">ESA SNAP toolbox walkthrough, radiometric calibration (σ⁰, β⁰, γ⁰), multilook processing, speckle filtering algorithms, Range-Doppler terrain correction, and GeoTIFF export.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/03-insar-change-detection' | relative_url }}">3. InSAR & Change Detection</a>
    <span class="desc">Interferometric SAR theory, phase unwrapping mathematics, coherence estimation, DEM generation from SAR pairs, differential InSAR for displacement mapping, and time-series analysis (SBAS, PSI).</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/04-kyrgyzstan-analysis-ideas' | relative_url }}">4. SAR Analysis Ideas for Kyrgyzstan</a>
    <span class="desc">Brainstorm session: detecting buried archaeological structures, mineral deposit signatures, glacier monitoring on Tien Shan, landslide susceptibility mapping, soil moisture estimation, and urban growth tracking.</span>
  </li>
</ul>
