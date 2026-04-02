---
layout: default
title: "SAR & Remote Sensing"
description: "Museum-style, math-first tutorials on Sentinel SAR, Sentinel-2 multispectral, drone photogrammetry, and remote sensing for Central Asia."
---

# SAR & Remote Sensing

<div class="hero-pattern" style="margin-bottom: 1.5rem;">
  <div style="display: flex; align-items: center; gap: 1rem; margin-bottom: 1rem;">
    <svg width="48" height="48" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg">
      <circle cx="24" cy="24" r="22" stroke="#1e4f8a" stroke-width="2" fill="#f0f4fa"/>
      <path d="M14 34 L24 10 L34 34" stroke="#1e4f8a" stroke-width="2" fill="none"/>
      <ellipse cx="24" cy="10" rx="4" ry="3" stroke="#d92b1f" stroke-width="1.5" fill="none"/>
      <path d="M10 38 L38 38" stroke="#8b5e00" stroke-width="2"/>
      <path d="M18 38 L18 30 M24 38 L24 26 M30 38 L30 32" stroke="#165d34" stroke-width="1.5" stroke-dasharray="2 2"/>
      <circle cx="24" cy="10" r="1.5" fill="#d92b1f"/>
    </svg>
    <div>
      <p style="margin: 0; font-size: 1.1rem; color: var(--ink);">This gallery is about seeing landscape structure through radar logic, optical spectral analysis, and high-resolution aerial survey: backscatter, slope, roughness, moisture, vegetation indices, and temporal change. The tutorials cover satellite and drone-based methods for Central Asia.</p>
    </div>
  </div>
</div>

<div class="stats-bar">
  <div class="stat-item">
    <span class="stat-number">22</span>
    <span class="stat-label">Tutorials</span>
  </div>
  <div class="stat-item">
    <span class="stat-number">660</span>
    <span class="stat-label">Exercises</span>
  </div>
  <div class="stat-item">
    <span class="stat-number">8</span>
    <span class="stat-label">Sensor Types</span>
  </div>
  <div class="stat-item">
    <span class="stat-number">10m</span>
    <span class="stat-label">Resolution</span>
  </div>
</div>

<div class="path-grid">
  <div class="path-card">
    <div class="topic-icon topic-icon-radar"><svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M5.636 18.364a9 9 0 0 1 0-12.728"/><path d="M8.464 15.536a5 5 0 0 1 0-7.072"/><circle cx="12" cy="12" r="1"/><path d="M15.536 8.464a5 5 0 0 1 0 7.072"/><path d="M18.364 5.636a9 9 0 0 1 0 12.728"/></svg></div>
    <div class="eyebrow">Suggested route</div>
    <h2>Core remote sensing sequence</h2>
    <p>Begin with SAR fundamentals, then process data, learn interferometry and time-series methods, add optical and thermal analysis.</p>
    <ol class="path-steps">
      <li><span class="path-step-num">01</span><div><a class="path-step-link" href="{{ '/tutorials/sar-remote-sensing/01-sar-fundamentals' | relative_url }}">SAR Fundamentals</a><span class="desc">Geometry, radar equation, resolution, and speckle.</span></div></li>
      <li><span class="path-step-num">02</span><div><a class="path-step-link" href="{{ '/tutorials/sar-remote-sensing/02-sentinel1-data-processing' | relative_url }}">Sentinel-1 Data Processing</a><span class="desc">Calibration, terrain correction, filtering, and units.</span></div></li>
      <li><span class="path-step-num">03</span><div><a class="path-step-link" href="{{ '/tutorials/sar-remote-sensing/03-insar-change-detection' | relative_url }}">InSAR & Change Detection</a><span class="desc">Phase, coherence, wrapping, and line-of-sight deformation.</span></div></li>
      <li><span class="path-step-num">04</span><div><a class="path-step-link" href="{{ '/tutorials/sar-remote-sensing/04-time-series-change-detection' | relative_url }}">Time-Series Analysis</a><span class="desc">Temporal statistics, seasonal decomposition, and anomaly detection.</span></div></li>
      <li><span class="path-step-num">05</span><div><a class="path-step-link" href="{{ '/tutorials/sar-remote-sensing/06-sentinel2-multispectral-analysis' | relative_url }}">Sentinel-2 Multispectral</a><span class="desc">Optical bands, spectral indices, and vegetation mapping.</span></div></li>
      <li><span class="path-step-num">06</span><div><a class="path-step-link" href="{{ '/tutorials/sar-remote-sensing/08-thermal-infrared-remote-sensing' | relative_url }}">Thermal Infrared</a><span class="desc">LST, emissivity, and thermal anomaly detection.</span></div></li>
    </ol>
    <a class="cta-button" href="{{ '/tutorials/sar-remote-sensing/01-sar-fundamentals' | relative_url }}"><svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="vertical-align: middle; margin-right: 0.4rem;"><polygon points="5 3 19 12 5 21 5 3"/></svg>Start this route</a>
  </div>
  <div class="path-card">
    <div class="topic-icon topic-icon-nature"><svg width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><rect x="4" y="4" width="16" height="16" rx="2"/><line x1="4" y1="10" x2="20" y2="10"/><line x1="10" y1="4" x2="10" y2="20"/><circle cx="7" cy="7" r="1" fill="white"/><circle cx="14" cy="14" r="1" fill="white"/></svg></div>
    <div class="eyebrow">Advanced track</div>
    <h2>Analysis, cloud computing & coding</h2>
    <p>Cloud platforms, machine learning, terrain analysis, and Python geospatial workflows.</p>
    <ol class="path-steps">
      <li><span class="path-step-num">09</span><div><a class="path-step-link" href="{{ '/tutorials/sar-remote-sensing/09-google-earth-engine-python' | relative_url }}">Google Earth Engine</a><span class="desc">Cloud-based analysis at planetary scale.</span></div></li>
      <li><span class="path-step-num">10</span><div><a class="path-step-link" href="{{ '/tutorials/sar-remote-sensing/10-machine-learning-classification' | relative_url }}">ML Classification</a><span class="desc">Random Forest, SVM, deep learning for land cover.</span></div></li>
      <li><span class="path-step-num">11</span><div><a class="path-step-link" href="{{ '/tutorials/sar-remote-sensing/11-dem-terrain-analysis' | relative_url }}">DEM & Terrain</a><span class="desc">Slope, aspect, drainage, and landform analysis.</span></div></li>
      <li><span class="path-step-num">12</span><div><a class="path-step-link" href="{{ '/tutorials/sar-remote-sensing/12-sar-polarimetry' | relative_url }}">SAR Polarimetry</a><span class="desc">Scattering matrices and decomposition theorems.</span></div></li>
      <li><span class="path-step-num">13</span><div><a class="path-step-link" href="{{ '/tutorials/sar-remote-sensing/13-water-resources-monitoring' | relative_url }}">Water Resources</a><span class="desc">Lakes, snow, soil moisture, and irrigation.</span></div></li>
      <li><span class="path-step-num">15</span><div><a class="path-step-link" href="{{ '/tutorials/sar-remote-sensing/15-python-geospatial-analysis' | relative_url }}">Python Geospatial</a><span class="desc">Rasterio, geopandas, and xarray workflows.</span></div></li>
    </ol>
    <a class="cta-button" href="{{ '/tutorials/sar-remote-sensing/09-google-earth-engine-python' | relative_url }}"><svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" style="vertical-align: middle; margin-right: 0.4rem;"><polygon points="5 3 19 12 5 21 5 3"/></svg>Start advanced track</a>
  </div>
</div>

<div class="info-box note">
  <strong>About the sensors</strong>
  Sentinel-1 (SAR) and Sentinel-2 (optical) are ESA Copernicus missions. This gallery now also covers drone-based photogrammetry and LiDAR for high-resolution local surveys that complement satellite data.
</div>

<div class="section-card featured-card">
  <div class="eyebrow">Now on display</div>
  <h2>Twenty-two live remote sensing tutorials</h2>
  <ul class="tutorial-list">
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/01-sar-fundamentals' | relative_url }}">How SAR Fundamentals Help Reveal Hidden Structure</a>
      <span class="desc">Radar equation, geometry, resolution, polarization, and speckle.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/02-sentinel1-data-processing' | relative_url }}">How Sentinel-1 Data Processing Reveals Cleaner Signals</a>
      <span class="desc">Calibration, filtering, terrain correction, and interpretation-ready backscatter.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/03-insar-change-detection' | relative_url }}">How InSAR and Change Detection Reveal Motion and Stability</a>
      <span class="desc">Phase, coherence, LOS displacement, and deformation reasoning.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/04-time-series-change-detection' | relative_url }}">How Time-Series Analysis Reveals Landscape Dynamics</a>
      <span class="desc">Temporal statistics, seasonal decomposition, and anomaly detection.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/05-sentinel1-central-asia-unseen-patterns' | relative_url }}">How Sentinel SAR Data Reveals Patterns in Central Asia</a>
      <span class="desc">Backscatter, moisture, geometry, and 30 regional exercises.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/06-sentinel2-multispectral-analysis' | relative_url }}">How Sentinel-2 Multispectral Data Reveals Landscape Patterns</a>
      <span class="desc">Spectral indices, band combinations, vegetation, and land cover.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/07-drone-photogrammetry-lidar' | relative_url }}">How Drone Photogrammetry and LiDAR Reveal Surface Detail</a>
      <span class="desc">UAV platforms, SfM processing, point clouds, and terrain models.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/08-thermal-infrared-remote-sensing' | relative_url }}">How Thermal Infrared Reveals Temperature Patterns</a>
      <span class="desc">Land surface temperature, emissivity, and thermal anomaly detection.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/09-google-earth-engine-python' | relative_url }}">How Google Earth Engine Enables Cloud-Scale Analysis</a>
      <span class="desc">Python API, image collections, reducer operations, and exports.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/10-machine-learning-classification' | relative_url }}">How Machine Learning Improves Land Cover Classification</a>
      <span class="desc">Random Forest, SVM, CNNs, training data, and accuracy assessment.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/11-dem-terrain-analysis' | relative_url }}">How DEM Analysis Reveals Terrain Structure</a>
      <span class="desc">Slope, aspect, curvature, drainage networks, and landform mapping.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/12-sar-polarimetry' | relative_url }}">How SAR Polarimetry Reveals Scattering Mechanisms</a>
      <span class="desc">Polarization, covariance matrices, decomposition, and H-Alpha.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/13-water-resources-monitoring' | relative_url }}">How Remote Sensing Monitors Water Resources</a>
      <span class="desc">Surface water, wetlands, snow cover, and soil moisture mapping.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/14-archaeological-remote-sensing' | relative_url }}">How Remote Sensing Reveals Archaeological Landscapes</a>
      <span class="desc">Crop marks, kurgans, qanats, and Silk Road site prospection.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/15-python-geospatial-analysis' | relative_url }}">How Python Enables Geospatial Analysis</a>
      <span class="desc">Rasterio, geopandas, xarray, and reproducible workflows.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/16-persistent-scatterer-insar' | relative_url }}">How PS-InSAR Measures Millimetre Ground Deformation</a>
      <span class="desc">Persistent scatterers, SBAS, atmospheric correction, and subsidence monitoring.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/17-disaster-monitoring-remote-sensing' | relative_url }}">How Remote Sensing Monitors Natural Disasters</a>
      <span class="desc">Earthquake damage, flood mapping, landslides, and wildfire assessment.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/18-sar-optical-fusion' | relative_url }}">How SAR-Optical Fusion Enhances Land Cover Analysis</a>
      <span class="desc">Combining Sentinel-1 and Sentinel-2, cloud gap-filling, and ML classification.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/19-accuracy-assessment-validation' | relative_url }}">How Accuracy Assessment Validates Results</a>
      <span class="desc">Confusion matrices, kappa, sampling design, and validation protocols.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/20-land-degradation-monitoring' | relative_url }}">How Remote Sensing Monitors Land Degradation</a>
      <span class="desc">Desertification, salinization, erosion, and vegetation trends.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/21-field-spectroscopy-hyperspectral' | relative_url }}">How Field Spectroscopy Enables Hyperspectral Analysis</a>
      <span class="desc">Spectroradiometers, spectral libraries, and mineral identification.</span>
    </li>
    <li>
      <a href="{{ '/tutorials/sar-remote-sensing/22-urban-remote-sensing' | relative_url }}">How Remote Sensing Monitors Urban Growth</a>
      <span class="desc">Urban expansion, heat islands, and infrastructure monitoring.</span>
    </li>
  </ul>
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
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/06-sentinel2-multispectral-analysis' | relative_url }}">Sentinel-2 Multispectral Analysis</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">Spectral bands, NDVI, NDWI, band combinations, and optical-SAR fusion.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/07-drone-photogrammetry-lidar' | relative_url }}">Drone Photogrammetry & LiDAR</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">UAV platforms, structure-from-motion, point clouds, DSM/DTM, and accuracy.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/08-thermal-infrared-remote-sensing' | relative_url }}">Thermal Infrared Remote Sensing</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">Land surface temperature, emissivity, thermal anomalies, and Landsat/MODIS thermal.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/09-google-earth-engine-python' | relative_url }}">Google Earth Engine with Python</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">Cloud computing, image collections, filter/map/reduce, and export workflows.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/10-machine-learning-classification' | relative_url }}">Machine Learning Classification</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">Random Forest, SVM, CNN, training data collection, and accuracy assessment.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/11-dem-terrain-analysis' | relative_url }}">DEM & Terrain Analysis</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">Slope, aspect, curvature, hydrological analysis, and landform classification.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/12-sar-polarimetry' | relative_url }}">SAR Polarimetry</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">Scattering matrices, covariance, Pauli, Freeman-Durden, and H-Alpha classification.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/13-water-resources-monitoring' | relative_url }}">Water Resources Monitoring</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">Surface water mapping, wetlands, snow cover, soil moisture, and Aral Sea case study.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/14-archaeological-remote-sensing' | relative_url }}">Archaeological Remote Sensing</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">Crop marks, soil marks, kurgan detection, qanats, and Silk Road prospection.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/15-python-geospatial-analysis' | relative_url }}">Python Geospatial Analysis</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">Rasterio, geopandas, xarray, visualization, and batch processing workflows.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/16-persistent-scatterer-insar' | relative_url }}">Persistent Scatterer InSAR</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">PS-InSAR, SBAS, atmospheric phase screens, and millimetre-scale deformation.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/17-disaster-monitoring-remote-sensing' | relative_url }}">Natural Disaster Monitoring</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">Earthquake damage, flood mapping, landslide detection, and wildfire monitoring.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/18-sar-optical-fusion' | relative_url }}">SAR-Optical Data Fusion</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">Combining Sentinel-1 SAR and Sentinel-2 optical for improved classification.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/19-accuracy-assessment-validation' | relative_url }}">Accuracy Assessment & Validation</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">Confusion matrices, kappa, F1 score, sampling design, and reporting standards.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/20-land-degradation-monitoring' | relative_url }}">Land Degradation Monitoring</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">Desertification, salinization, erosion, and multi-decadal vegetation trends.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/21-field-spectroscopy-hyperspectral' | relative_url }}">Field Spectroscopy & Hyperspectral</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">Spectroradiometers, spectral libraries, mineral mapping, and unmixing.</span>
  </li>
  <li>
    <a href="{{ '/tutorials/sar-remote-sensing/22-urban-remote-sensing' | relative_url }}">Urban Remote Sensing</a>
    <span class="status-pill status-live">Live</span>
    <span class="desc">Urban expansion, built-up indices, heat islands, and infrastructure monitoring.</span>
  </li>
</ul>
