---
layout: tutorial
title: "How Remote Sensing Reveals Archaeological Landscapes in Central Asia"
description: "A practical tutorial on detecting archaeological sites, ancient irrigation, Silk Road features, and burial mounds using satellite imagery for Central Asia, with 30 exercises and solutions."
difficulty: intermediate
estimated_time: "2-3 hours"
series: sar-remote-sensing
order: 14
---

## Why this tutorial matters

Central Asia preserves one of Earth's richest archaeological landscapes—from Neolithic settlements to medieval Silk Road cities, from Bronze Age irrigation to kurgan burial mounds spanning millennia. Yet the region's 4 million square kilometers present a fundamental challenge: traditional field survey cannot systematically cover such vastness.

Remote sensing transforms this equation. Satellites provide systematic coverage independent of ground access, detect features invisible to the naked eye, and enable temporal analysis spanning decades.

<div class="info-box tip">
  <strong>Key idea</strong>
  Remote sensing does not replace fieldwork—it makes fieldwork dramatically more efficient. Instead of searching blindly, archaeologists target specific anomalies, shifting from discovery mode to verification mode.
</div>

<div class="learning-objectives">
  <div class="learning-objectives-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#1e4f8a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <h3>What you will learn</h3>
  </div>
  <ul>
    <li>Identify crop marks, soil marks, shadow marks, and moisture anomalies as indicators of buried archaeological features</li>
    <li>Apply NDVI and SAVI vegetation indices to multispectral imagery for detecting subsurface walls and ditches</li>
    <li>Use SAR penetration depth and backscatter contrast to locate archaeological structures beneath arid Central Asian soils</li>
    <li>Detect kurgans, mounds, and linear features (qanats, canals, ancient roads) using DEM analysis and edge-detection techniques</li>
    <li>Design a multi-sensor prospection workflow for Silk Road site discovery combining optical, SAR, and historical CORONA imagery</li>
  </ul>
</div>

<div class="prerequisites">
  <div class="prerequisites-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#8b5e00" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
    <h3>Prerequisites</h3>
  </div>
  <ul>
    <li>Familiarity with satellite imagery (optical bands, resolution concepts) from earlier tutorials in this series</li>
    <li>Basic understanding of SAR backscatter and interferometric coherence</li>
    <li>Experience with a GIS platform (QGIS, Google Earth Engine, or similar) for visualizing raster data</li>
    <li>General knowledge of Central Asian geography and archaeological site types (helpful but not required)</li>
  </ul>
</div>

<div class="concept-diagram">
  <svg viewBox="0 0 620 320" xmlns="http://www.w3.org/2000/svg" style="max-width: 580px;">
    <!-- Sky background -->
    <rect x="0" y="0" width="620" height="100" fill="#d6e8f7"/>
    <!-- Satellite -->
    <rect x="280" y="12" width="30" height="20" rx="3" fill="#68625b" stroke="#111" stroke-width="1"/>
    <line x1="270" y1="22" x2="280" y2="22" stroke="#1e4f8a" stroke-width="2"/>
    <line x1="310" y1="22" x2="320" y2="22" stroke="#1e4f8a" stroke-width="2"/>
    <rect x="260" y="18" width="10" height="8" fill="#1e4f8a"/>
    <rect x="320" y="18" width="10" height="8" fill="#1e4f8a"/>
    <text x="295" y="50" font-family="Inter, sans-serif" font-size="10" fill="#111" text-anchor="middle">Satellite sensor</text>
    <!-- Radar beam -->
    <polygon points="295,32 180,100 410,100" fill="#1e4f8a" opacity="0.08" stroke="#1e4f8a" stroke-width="0.5" stroke-dasharray="4,3"/>
    <!-- Ground surface -->
    <rect x="0" y="100" width="620" height="10" fill="#c4a56e"/>
    <!-- Soil layer -->
    <rect x="0" y="110" width="620" height="130" fill="#ddd0b5"/>
    <!-- Buried wall (subsurface) -->
    <rect x="170" y="135" width="12" height="50" fill="#8b5e00" stroke="#68625b" stroke-width="1"/>
    <rect x="270" y="135" width="12" height="50" fill="#8b5e00" stroke="#68625b" stroke-width="1"/>
    <rect x="370" y="135" width="12" height="50" fill="#8b5e00" stroke="#68625b" stroke-width="1"/>
    <text x="275" y="210" font-family="Inter, sans-serif" font-size="10" fill="#8b5e00" text-anchor="middle">Buried walls</text>
    <!-- Buried ditch -->
    <path d="M440,135 Q460,185 480,135" fill="#68625b" stroke="#68625b" stroke-width="1"/>
    <text x="460" y="210" font-family="Inter, sans-serif" font-size="10" fill="#68625b" text-anchor="middle">Ditch</text>
    <!-- Vegetation on surface: stressed over walls, vigorous over ditch -->
    <!-- Stressed crops over wall 1 -->
    <line x1="165" y1="100" x2="165" y2="88" stroke="#a8c060" stroke-width="2"/>
    <line x1="170" y1="100" x2="170" y2="90" stroke="#a8c060" stroke-width="2"/>
    <line x1="176" y1="100" x2="176" y2="87" stroke="#a8c060" stroke-width="2"/>
    <line x1="182" y1="100" x2="182" y2="89" stroke="#a8c060" stroke-width="2"/>
    <!-- Stressed crops over wall 2 -->
    <line x1="265" y1="100" x2="265" y2="88" stroke="#a8c060" stroke-width="2"/>
    <line x1="270" y1="100" x2="270" y2="91" stroke="#a8c060" stroke-width="2"/>
    <line x1="276" y1="100" x2="276" y2="87" stroke="#a8c060" stroke-width="2"/>
    <line x1="282" y1="100" x2="282" y2="90" stroke="#a8c060" stroke-width="2"/>
    <!-- Stressed crops over wall 3 -->
    <line x1="365" y1="100" x2="365" y2="89" stroke="#a8c060" stroke-width="2"/>
    <line x1="370" y1="100" x2="370" y2="91" stroke="#a8c060" stroke-width="2"/>
    <line x1="376" y1="100" x2="376" y2="88" stroke="#a8c060" stroke-width="2"/>
    <line x1="382" y1="100" x2="382" y2="90" stroke="#a8c060" stroke-width="2"/>
    <!-- Vigorous crops over ditch -->
    <line x1="440" y1="100" x2="440" y2="78" stroke="#165d34" stroke-width="2"/>
    <line x1="448" y1="100" x2="448" y2="75" stroke="#165d34" stroke-width="2"/>
    <line x1="455" y1="100" x2="455" y2="72" stroke="#165d34" stroke-width="2"/>
    <line x1="462" y1="100" x2="462" y2="74" stroke="#165d34" stroke-width="2"/>
    <line x1="470" y1="100" x2="470" y2="76" stroke="#165d34" stroke-width="2"/>
    <line x1="478" y1="100" x2="478" y2="78" stroke="#165d34" stroke-width="2"/>
    <!-- Normal crops elsewhere -->
    <line x1="100" y1="100" x2="100" y2="84" stroke="#6aa843" stroke-width="2"/>
    <line x1="110" y1="100" x2="110" y2="82" stroke="#6aa843" stroke-width="2"/>
    <line x1="120" y1="100" x2="120" y2="85" stroke="#6aa843" stroke-width="2"/>
    <line x1="130" y1="100" x2="130" y2="83" stroke="#6aa843" stroke-width="2"/>
    <line x1="220" y1="100" x2="220" y2="83" stroke="#6aa843" stroke-width="2"/>
    <line x1="230" y1="100" x2="230" y2="85" stroke="#6aa843" stroke-width="2"/>
    <line x1="320" y1="100" x2="320" y2="84" stroke="#6aa843" stroke-width="2"/>
    <line x1="330" y1="100" x2="330" y2="82" stroke="#6aa843" stroke-width="2"/>
    <line x1="520" y1="100" x2="520" y2="83" stroke="#6aa843" stroke-width="2"/>
    <line x1="530" y1="100" x2="530" y2="85" stroke="#6aa843" stroke-width="2"/>
    <!-- Labels -->
    <text x="176" y="75" font-family="Inter, sans-serif" font-size="9" fill="#d92b1f" text-anchor="middle">Stressed</text>
    <text x="455" y="65" font-family="Inter, sans-serif" font-size="9" fill="#165d34" text-anchor="middle">Vigorous</text>
    <!-- NDVI profile at bottom -->
    <rect x="30" y="245" width="560" height="65" rx="4" fill="#f5f5f0" stroke="#68625b" stroke-width="0.5"/>
    <text x="45" y="260" font-family="Inter, sans-serif" font-size="10" fill="#111" font-weight="bold">NDVI profile</text>
    <line x1="60" y1="265" x2="60" y2="300" stroke="#111" stroke-width="1"/>
    <line x1="60" y1="300" x2="570" y2="300" stroke="#111" stroke-width="1"/>
    <text x="55" y="275" font-family="Inter, sans-serif" font-size="8" fill="#111" text-anchor="end">High</text>
    <text x="55" y="298" font-family="Inter, sans-serif" font-size="8" fill="#111" text-anchor="end">Low</text>
    <!-- NDVI curve: dips over walls, peaks over ditch -->
    <polyline points="70,280 130,280 160,293 190,280 250,280 260,293 290,280 340,280 360,293 390,280 430,280 445,270 460,268 475,270 490,280 550,280" fill="none" stroke="#165d34" stroke-width="2"/>
    <!-- Dip labels -->
    <text x="176" y="307" font-family="Inter, sans-serif" font-size="8" fill="#d92b1f" text-anchor="middle">wall</text>
    <text x="276" y="307" font-family="Inter, sans-serif" font-size="8" fill="#d92b1f" text-anchor="middle">wall</text>
    <text x="376" y="307" font-family="Inter, sans-serif" font-size="8" fill="#d92b1f" text-anchor="middle">wall</text>
    <text x="460" y="307" font-family="Inter, sans-serif" font-size="8" fill="#165d34" text-anchor="middle">ditch</text>
  </svg>
  <p class="diagram-caption">Cross-section showing how buried archaeological features affect surface vegetation, creating detectable crop marks. Walls impede root growth (lower NDVI), while ditches retain moisture (higher NDVI).</p>
</div>

## Principles: crop marks, soil marks, shadow marks, moisture anomalies

### Crop marks

Buried features affect overlying vegetation through differential moisture. A buried wall impedes roots and reduces moisture, causing crop stress (lighter vegetation). A buried ditch retains moisture, promoting vigorous growth (darker vegetation). Crop marks peak during late spring moisture stress.

### Soil marks

Where archaeology lies near surface, color and texture contrasts appear. Mudbrick weathers lighter; occupation debris introduces darker organic matter. Best observed on freshly plowed fields.

### Shadow marks

Subtle topographic variations cast shadows at low sun angles. Eroded walls appear as shadow lines; filled ditches create depressions. Winter morning/evening imagery provides optimal enhancement.

### Moisture anomalies

Archaeological features create persistent moisture differences detectable in thermal infrared and SAR. Compacted floors shed water; middens retain it.

## Optical imagery analysis

### High-resolution imagery

VHR sensors (WorldView, Pléiades, Planet SkySat) at 30-50 cm resolution reveal individual architectural features: walls, rooms, streets, fortifications. For Central Asian sites, VHR imagery shows rectangular foundations, street grids, irrigation networks, and kurgan rings.

### Multispectral analysis

Beyond visible light, NIR and SWIR bands reveal vegetation stress invisible to the eye.

**NDVI** highlights crop mark variations:

$$
\text{NDVI} = \frac{\rho_{NIR} - \rho_{Red}}{\rho_{NIR} + \rho_{Red}}
$$

**SAVI** reduces soil background influence in sparse vegetation:

$$
\text{SAVI} = \frac{(\rho_{NIR} - \rho_{Red}) \cdot (1 + L)}{\rho_{NIR} + \rho_{Red} + L}
$$

### Historical imagery

Declassified CORONA photography (1960-1972) at ~2m resolution documents landscapes before modern development—sites now beneath reservoirs, pre-irrigation patterns, intact kurgan fields.

## SAR for archaeology

### Penetration depth

Radar penetration depends on wavelength and soil moisture. C-band (Sentinel-1) penetrates 1-2 meters in dry sand; L-band reaches 2-5 meters. Central Asia's arid environments provide favorable conditions for subsurface detection.

### Moisture contrast

Archaeological features with different moisture retention create backscatter contrasts. Compacted walls drain rapidly (darker); ditches accumulate moisture (brighter). Time-series analysis distinguishes persistent archaeological anomalies from transient variations.

### Geometric feature detection

Corner reflectors (wall-ground interfaces) produce strong returns. Linear features create distinctive patterns depending on orientation relative to radar look direction.

<div class="info-box tip">
  <strong>Key idea</strong>
  Combine ascending and descending Sentinel-1 orbits. Linear features visible in one geometry may be invisible in the other.
</div>

## Detecting linear features: qanats, canals, roads, walls

### Qanats and karez

Underground aqueducts with surface manifestations: chains of excavation shafts at 20-50m intervals, spoil mounds, linear vegetation anomalies following tunnel alignment.

### Ancient canals

Abandoned canals appear as linear vegetation anomalies, slight depressions, or soil color contrasts. Multitemporal NDVI reveals seasonal moisture retention patterns.

### Roads and routes

Ancient roads leave compacted surfaces with different drainage, roadside ditches, and cut-and-fill terracing visible in slope analysis. Silk Road routes often underlie modern alignments.

### Defensive walls

Long walls create shadow lines, soil contrasts, vegetation boundaries, and SAR corner reflector effects.

## Mound and kurgan detection strategies

### Morphometric analysis

Kurgans exhibit characteristic morphology: circular planform, convex profile, raised elevation. DEMs enable systematic screening using circularity index (perimeter²/area ≈ 12.57 for circles) and height-to-diameter ratios (typically 0.1-0.3).

### Ring features

Many kurgans include surrounding ditches or stone rings visible as circular crop marks or moisture anomalies in SAR.

<div class="info-box tip">
  <strong>Key idea</strong>
  Not every circular feature is a kurgan. Multi-source verification—optical, SAR, and topographic data—reduces false positives.
</div>

## Ancient irrigation and paleochannel mapping

### Paleochannel detection

Abandoned channels preserve relict sediments detectable through spectral analysis, vegetation anomalies, topographic analysis, and SAR moisture contrasts. The Amu Darya and Syr Darya deltas preserve complex networks documenting millennia of river migration.

### Irrigation network mapping

Ancient systems create hierarchical features: main canals, secondary distributaries, field-scale ditches. Mapping reveals water sources, distribution hierarchies, and abandonment patterns.

## Silk Road site prospection

### Site types

**Caravanserais**: Rectangular compounds (50-100m) at water sources, passes, and one-day travel intervals. Detectable as soil marks, wall lines, and corner reflector effects.

**Urban sites**: Street grids, citadel mounds, extensive agricultural zones, multiple occupation phases.

**Watchtowers**: Intervisible stations identifiable through viewshed analysis.

### Route reconstruction

Least-cost modeling constrained by water, terrain, and inter-station distances. Known sites anchor models; predicted paths identify prospection targets.

## Integration with ground geophysics and field survey

### Ground-penetrating radar

GPR provides subsurface imaging at centimeter resolution—wall foundations, burial chambers, stratigraphic sequences. Works best in dry, sandy sediments.

### Magnetometry

Detects fired materials and soil disturbances. Rapid and effective for targeting excavation within site boundaries.

### Field verification protocols

1. Navigate to anomaly coordinates
2. Document surface conditions
3. Collect diagnostic artifacts
4. Photograph with scale
5. Record interpretation confidence
6. Update database with results

## Legal and ethical considerations

### Legal frameworks

Archaeological research requires permits from national heritage agencies. Satellite imagery may have restricted distribution. Precise coordinates may require protection.

### Ethical considerations

- Community engagement in heritage research
- Looting risk from published locations
- Balancing open data with protection needs
- Building local expertise

<div class="info-box tip">
  <strong>Key idea</strong>
  Archaeological remote sensing is not just discovery—equally important is using these tools to document, monitor, and protect sites from ongoing threats.
</div>

## Remote sensing workflow checklist

<ul class="checklist">
  <li>Define research questions and target feature types</li>
  <li>Review archaeological databases and literature</li>
  <li>Acquire appropriate imagery (optical, SAR, historical)</li>
  <li>Preprocess imagery (geometric, radiometric)</li>
  <li>Generate derived products (indices, DEMs)</li>
  <li>Systematic visual interpretation</li>
  <li>Apply automated detection algorithms</li>
  <li>Cross-reference anomalies across sources</li>
  <li>Classify by confidence and feature type</li>
  <li>Prioritize for ground verification</li>
  <li>Conduct systematic field verification</li>
  <li>Update interpretations from field results</li>
  <li>Integrate with regional databases</li>
  <li>Assess protection needs</li>
  <li>Archive with metadata</li>
  <li>Publish following ethical guidelines</li>
</ul>

## Exercises

<div class="exercise" id="exercise-1">
  <h3>Exercise 1: Crop mark timing</h3>
  <p>A wheat field overlies buried mudbrick walls. Irrigation occurs in April; harvest in July. When would crop marks be most visible?</p>
  <details>
    <summary>Show solution</summary>
    <p>May-June, during grain filling when moisture stress differentiates vegetation over buried features. Walls impede moisture, causing earlier stress and yellower vegetation.</p>
  </details>
</div>

<div class="exercise" id="exercise-2">
  <h3>Exercise 2: Shadow geometry</h3>
  <p>An east-west wall creates shadow marks. At what sun position are shadows maximized?</p>
  <details>
    <summary>Show solution</summary>
    <p>Sun due south (azimuth 180°) at low elevation (10-20°). The wall casts long shadows to the north, perpendicular to the wall alignment.</p>
  </details>
</div>

<div class="exercise" id="exercise-3">
  <h3>Exercise 3: SAR penetration</h3>
  <p>Sentinel-1 C-band (5.6 cm wavelength) images dry sandy soil. What penetration depth is expected?</p>
  <details>
    <summary>Show solution</summary>
    <p>C-band penetrates approximately 1-2 meters in ideal dry sand conditions. Field observations confirm detection capability at these depths.</p>
  </details>
</div>

<div class="exercise" id="exercise-4">
  <h3>Exercise 4: NDVI crop marks</h3>
  <p>Healthy wheat: NDVI = 0.75. Stressed wheat over walls: NDVI = 0.55. Is this detectable?</p>
  <details>
    <summary>Show solution</summary>
    <p>Yes—26.7% reduction. NDVI differences exceeding 0.1 are significant. The 0.20 difference produces clear patterns.</p>
  </details>
</div>

<div class="exercise" id="exercise-5">
  <h3>Exercise 5: Qanat detection</h3>
  <p>Qanat shafts appear at 35m intervals. What surface features indicate qanat presence?</p>
  <details>
    <summary>Show solution</summary>
    <p>Chains of circular depressions (shaft openings), spoil mounds creating shadow chains, and linear darker vegetation following tunnel alignment.</p>
  </details>
</div>

<div class="exercise" id="exercise-6">
  <h3>Exercise 6: Kurgan circularity</h3>
  <p>Feature has perimeter 95m and area 700m². Calculate circularity and assess origin.</p>
  <details>
    <summary>Show solution</summary>
    <p>Circularity = 95²/700 = 12.89. Near perfect circle (4π ≈ 12.57), strongly suggesting constructed kurgan. Natural features exceed 15-20.</p>
  </details>
</div>

<div class="exercise" id="exercise-7">
  <h3>Exercise 7: CORONA resolution</h3>
  <p>CORONA KH-4B has ~2m resolution. What minimum kurgan diameter is detectable?</p>
  <details>
    <summary>Show solution</summary>
    <p>Features need 2-3 pixels for reliability. Minimum: 4-6m diameter. Smaller kurgans may be visible but not reliably distinguished.</p>
  </details>
</div>

<div class="exercise" id="exercise-8">
  <h3>Exercise 8: SAR look direction</h3>
  <p>Compare backscatter from N-S road versus E-W road in Sentinel-1 descending orbit (eastward look).</p>
  <details>
    <summary>Show solution</summary>
    <p>N-S road is perpendicular to look direction—acts as corner reflector, strong returns. E-W road is parallel—weaker returns. N-S appears brighter.</p>
  </details>
</div>

<div class="exercise" id="exercise-9">
  <h3>Exercise 9: Caravanserai spacing</h3>
  <p>Caravans averaged 30 km/day. Caravanserai at 39.5°N, 66.5°E. Where search for next station?</p>
  <details>
    <summary>Show solution</summary>
    <p>~30 km east/west. At this latitude, 30 km ≈ 0.35° longitude. Search centers: 66.15°E and 66.85°E, with 5-10 km radius.</p>
  </details>
</div>

<div class="exercise" id="exercise-10">
  <h3>Exercise 10: Looting detection</h3>
  <p>What distinguishes looting pits from archaeological excavations in imagery?</p>
  <details>
    <summary>Show solution</summary>
    <p>Looting: irregular shapes, random distribution, asymmetric spoil, fresh soil, progressive increase over time. Excavations: rectangular, systematic grid, organized spoil.</p>
  </details>
</div>

<div class="exercise" id="exercise-11">
  <h3>Exercise 11: Paleochannel vegetation</h3>
  <p>What vegetation characteristics indicate subsurface moisture from an abandoned channel?</p>
  <details>
    <summary>Show solution</summary>
    <p>Denser coverage, greener color persisting into dry season, taller growth, higher NDVI, moisture-tolerant species composition.</p>
  </details>
</div>

<div class="exercise" id="exercise-12">
  <h3>Exercise 12: Wall detection strategy</h3>
  <p>Design multi-sensor approach to find a 50 km defensive wall across steppe.</p>
  <details>
    <summary>Show solution</summary>
    <p>Sentinel-2 NDVI for vegetation anomalies; CORONA for baseline; Sentinel-1 ascending/descending for orientation-independent detection; VHR for verification.</p>
  </details>
</div>

<div class="exercise" id="exercise-13">
  <h3>Exercise 13: Site loss assessment</h3>
  <p>CORONA (1968) shows 15 kurgans; modern imagery shows 9. How investigate the loss?</p>
  <details>
    <summary>Show solution</summary>
    <p>Check for infrastructure at lost locations; examine Landsat archive to date disappearance; assess agricultural intensification; field visit survivors.</p>
  </details>
</div>

<div class="exercise" id="exercise-14">
  <h3>Exercise 14: Change detection</h3>
  <p>Using Sentinel-2 from 2019 and 2023, how detect new looting?</p>
  <details>
    <summary>Show solution</summary>
    <p>Normalize images; calculate difference; threshold significant changes; focus on site boundaries; filter by size (2-10m); examine irregular pit shapes.</p>
  </details>
</div>

<div class="exercise" id="exercise-15">
  <h3>Exercise 15: Verification priority</h3>
  <p>45 potential kurgans identified; time for 15 field visits. How prioritize?</p>
  <details>
    <summary>Show solution</summary>
    <p>Rank by: detection confidence, spatial distribution, size range representation, landscape diversity, accessibility, threat status, multi-source confirmation.</p>
  </details>
</div>

<div class="exercise" id="exercise-16">
  <h3>Exercise 16: Irrigation hierarchy</h3>
  <p>Canals show widths of 2m, 5m, and 12m. What does this indicate?</p>
  <details>
    <summary>Show solution</summary>
    <p>Three-tier hierarchy: 12m primary canals from rivers; 5m secondary distributaries; 2m tertiary field ditches. Indicates centralized water management.</p>
  </details>
</div>

<div class="exercise" id="exercise-17">
  <h3>Exercise 17: Thermal anomaly</h3>
  <p>Rectangular cool anomaly (2°C below surroundings) appears at a site. Possible causes?</p>
  <details>
    <summary>Show solution</summary>
    <p>Higher soil moisture; denser vegetation cooling; stone foundations with high thermal inertia; subsurface voids. Rectangular shape suggests architecture.</p>
  </details>
</div>

<div class="exercise" id="exercise-18">
  <h3>Exercise 18: Predictive model</h3>
  <p>Bronze Age sites cluster within 500m of water. Design a predictive model.</p>
  <details>
    <summary>Show solution</summary>
    <p>Map water sources; buffer 500m; exclude steep slopes and modern areas; prioritize south-facing slopes; validate that known sites fall within predicted zones.</p>
  </details>
</div>

<div class="exercise" id="exercise-19">
  <h3>Exercise 19: Archive comparison</h3>
  <p>Compare utility of CORONA, Landsat, and Sentinel for different archaeological questions.</p>
  <details>
    <summary>Show solution</summary>
    <p>CORONA: pre-development baseline, lost sites. Landsat: long-term change, seasonal patterns. Sentinel: current conditions, high frequency, SAR capability.</p>
  </details>
</div>

<div class="exercise" id="exercise-20">
  <h3>Exercise 20: False positive reduction</h3>
  <p>Automated kurgan detection: 200 candidates, 60% false positives. How refine?</p>
  <details>
    <summary>Show solution</summary>
    <p>Analyze false positive characteristics; add contextual filters; require multi-source confirmation; tighten circularity thresholds; add texture filters.</p>
  </details>
</div>

<div class="exercise" id="exercise-21">
  <h3>Exercise 21: Route modeling</h3>
  <p>Two caravanserais 45 km apart with mountain ridge between. Model the route.</p>
  <details>
    <summary>Show solution</summary>
    <p>Generate cost surface from DEM; calculate least-cost path; identify passes; search for intermediate features at rest points; apply viewshed for watchtowers.</p>
  </details>
</div>

<div class="exercise" id="exercise-22">
  <h3>Exercise 22: Reservoir survey</h3>
  <p>2,000 ha will flood. Design documentation survey.</p>
  <details>
    <summary>Show solution</summary>
    <p>Acquire CORONA baseline; process Sentinel archive; obtain VHR; generate DEM; systematic interpretation; automated detection; field verification; archive with agencies.</p>
  </details>
</div>

<div class="exercise" id="exercise-23">
  <h3>Exercise 23: SAR timing</h3>
  <p>When are SAR moisture contrasts maximized in continental climate?</p>
  <details>
    <summary>Show solution</summary>
    <p>Moisture transition periods: spring snowmelt (March-April) when differential drainage appears; early summer drought onset (June) when features dry at different rates.</p>
  </details>
</div>

<div class="exercise" id="exercise-24">
  <h3>Exercise 24: Urban mapping</h3>
  <p>VHR shows 500×400m area with complex soil marks. What features to map?</p>
  <details>
    <summary>Show solution</summary>
    <p>Street alignments, building footprints, fortifications, open spaces, water features, mounded areas. Grid reveals planning; density indicates population.</p>
  </details>
</div>

<div class="exercise" id="exercise-25">
  <h3>Exercise 25: Geophysics targeting</h3>
  <p>3 days for magnetometer at 5 ha site. How target survey?</p>
  <details>
    <summary>Show solution</summary>
    <p>Prioritize strongest anomalies; sample different feature types; include control area; avoid metal interference; cross features perpendicularly.</p>
  </details>
</div>

<div class="exercise" id="exercise-26">
  <h3>Exercise 26: Publication ethics</h3>
  <p>50 new kurgan sites identified. What to publish, what to protect?</p>
  <details>
    <summary>Show solution</summary>
    <p>Publish: methodology, classifications, statistics, representative examples. Protect: precise coordinates, detailed maps, access routes. Deposit full data with heritage agencies.</p>
  </details>
</div>

<div class="exercise" id="exercise-27">
  <h3>Exercise 27: Temporal resolution</h3>
  <p>What temporal resolution captures crop mark windows?</p>
  <details>
    <summary>Show solution</summary>
    <p>Windows last 1-2 weeks; need 5-10 day revisit. Sentinel-2 (5-day), Planet (daily), or Landsat+Sentinel combined (~3-day) work well.</p>
  </details>
</div>

<div class="exercise" id="exercise-28">
  <h3>Exercise 28: Scale effects</h3>
  <p>Why are 5m kurgans visible in 50cm imagery but not 10m Sentinel-2?</p>
  <details>
    <summary>Show solution</summary>
    <p>At 50cm: 5m spans 10 pixels—sufficient. At 10m: spans 0.5 pixels—below threshold. Feature size must match sensor resolution.</p>
  </details>
</div>

<div class="exercise" id="exercise-29">
  <h3>Exercise 29: Integration workflow</h3>
  <p>Design workflow integrating CORONA, Sentinel-1, Sentinel-2, VHR for 100 km² survey.</p>
  <details>
    <summary>Show solution</summary>
    <p>CORONA baseline; Sentinel-2 vegetation time series; Sentinel-1 moisture/geometry; combine anomalies; VHR for priority candidates; field verification; iterate.</p>
  </details>
</div>

<div class="exercise" id="exercise-30">
  <h3>Exercise 30: Monitoring program</h3>
  <p>Design program to monitor 200 sites for looting and development threats.</p>
  <details>
    <summary>Show solution</summary>
    <p>Establish baselines; define boundaries; monthly Sentinel-2 download; automated change detection; threshold flagging; visual verification; quarterly VHR; annual field sample; alert system.</p>
  </details>
</div>
