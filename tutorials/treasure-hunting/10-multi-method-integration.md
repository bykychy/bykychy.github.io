---
layout: tutorial
title: "How Multi-Method Integration Strengthens Archaeological Survey in Central Asia"
description: "A practical tutorial on combining geophysical methods, remote sensing, and field survey for robust site investigation in Central Asia, with 30 exercises and solutions."
difficulty: advanced
estimated_time: "2-3 hours"
series: treasure-hunting
order: 10
---

## Why this tutorial matters

No single prospection method sees everything. Magnetometry misses non-magnetic features. GPR struggles in saline soils. Remote sensing cannot penetrate dense vegetation. Yet buried sites leave multiple signatures—thermal, magnetic, chemical, electromagnetic—that different methods detect with varying sensitivity.

**Multi-method integration** is not about collecting more data; it is about combining complementary observations to build robust interpretations that no single technique could achieve alone. In Central Asia, where sites span millennia and environments range from alpine meadows to desert basins, integration is essential for reliable prospection.

This tutorial synthesizes concepts from previous modules—magnetometry, GPR, remote sensing, XRF, geochemical sampling, and GNSS surveying—into a unified workflow for combining evidence across scales and measurement types.

<div class="info-box tip">
  <strong>Key idea</strong>
  Integration strength comes from independence: if two methods with different physical bases both indicate the same feature, confidence increases multiplicatively. When they disagree, the discrepancy itself becomes diagnostic information.
</div>

<div class="learning-objectives">
  <div class="learning-objectives-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#1e4f8a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <h3>What you will learn</h3>
  </div>
  <ul>
    <li>How to evaluate method complementarity using detection-set overlap to choose optimal technique combinations for Central Asian sites</li>
    <li>A hierarchical survey strategy from satellite reconnaissance through geophysical mapping to targeted excavation with decision criteria at each stage</li>
    <li>Data integration approaches—GIS overlay, joint inversion, Bayesian fusion—for combining measurements on different physical scales</li>
    <li>Scale-matching and coordinate-system harmonization techniques for merging datasets from magnetometry, GPR, ERT, geochemistry, and remote sensing</li>
    <li>Interpretation workflows and reporting standards that synthesize multi-method evidence into defensible archaeological or engineering conclusions</li>
  </ul>
</div>

<div class="prerequisites">
  <div class="prerequisites-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#8b5e00" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
    <h3>Prerequisites</h3>
  </div>
  <ul>
    <li>Completion of at least three earlier tutorials in this series (magnetometry, GPR, EM, ERT, or geochemistry) to understand individual method strengths</li>
    <li>Working knowledge of GIS software for layering and comparing georeferenced datasets from different survey methods</li>
    <li>Familiarity with basic probability and set theory (union, intersection) used in complementarity and Bayesian confidence calculations</li>
  </ul>
</div>

<div class="concept-diagram">
  <svg viewBox="0 0 620 320" xmlns="http://www.w3.org/2000/svg" style="max-width: 580px;">
    <!-- Background -->
    <rect x="0" y="0" width="620" height="320" fill="#f8f7f4" rx="6"/>

    <!-- Title -->
    <text x="310" y="22" text-anchor="middle" font-family="Inter, sans-serif" font-size="13" font-weight="600" fill="#111">Multi-Method Integration — Converging on a Target</text>

    <!-- Central target area -->
    <ellipse cx="310" cy="190" rx="60" ry="45" fill="#d92b1f" opacity="0.12" stroke="#d92b1f" stroke-width="1.5" stroke-dasharray="5,3"/>
    <text x="310" y="185" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" font-weight="600" fill="#d92b1f">Target</text>
    <text x="310" y="200" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#d92b1f">Archaeological site</text>

    <!-- METHOD 1: Magnetometer (top-left) -->
    <g transform="translate(75,55)">
      <rect x="-30" y="-15" width="60" height="50" fill="#fff" stroke="#1e4f8a" stroke-width="1.5" rx="5"/>
      <line x1="0" y1="-15" x2="0" y2="-28" stroke="#1e4f8a" stroke-width="1.5"/>
      <circle cx="0" cy="-31" r="4" fill="#1e4f8a"/>
      <text x="0" y="5" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="600" fill="#1e4f8a">Magneto-</text>
      <text x="0" y="16" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="600" fill="#1e4f8a">meter</text>
      <text x="0" y="28" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#68625b">Δχ, TRM</text>
    </g>
    <!-- Arrow to target -->
    <line x1="115" y1="85" x2="260" y2="170" stroke="#1e4f8a" stroke-width="1.5" marker-end="url(#arrowBlue10)"/>

    <!-- METHOD 2: GPR antenna (top-right) -->
    <g transform="translate(540,55)">
      <rect x="-28" y="-10" width="56" height="44" fill="#fff" stroke="#165d34" stroke-width="1.5" rx="5"/>
      <rect x="-18" y="-5" width="36" height="12" fill="#165d34" opacity="0.3" rx="2"/>
      <text x="0" y="18" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="600" fill="#165d34">GPR</text>
      <text x="0" y="28" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#68625b">ε contrast</text>
    </g>
    <line x1="505" y1="80" x2="365" y2="170" stroke="#165d34" stroke-width="1.5" marker-end="url(#arrowGreen10)"/>

    <!-- METHOD 3: ERT array (bottom-left) -->
    <g transform="translate(70,275)">
      <rect x="-35" y="-18" width="70" height="44" fill="#fff" stroke="#8b5e00" stroke-width="1.5" rx="5"/>
      <!-- Tiny electrode symbols -->
      <line x1="-18" y1="-5" x2="-18" y2="5" stroke="#8b5e00" stroke-width="1.5"/>
      <line x1="-6" y1="-5" x2="-6" y2="5" stroke="#8b5e00" stroke-width="1.5"/>
      <line x1="6" y1="-5" x2="6" y2="5" stroke="#8b5e00" stroke-width="1.5"/>
      <line x1="18" y1="-5" x2="18" y2="5" stroke="#8b5e00" stroke-width="1.5"/>
      <text x="0" y="18" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="600" fill="#8b5e00">ERT</text>
    </g>
    <text x="70" y="307" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#68625b">ρ imaging</text>
    <line x1="120" y1="270" x2="260" y2="215" stroke="#8b5e00" stroke-width="1.5" marker-end="url(#arrowEarth10)"/>

    <!-- METHOD 4: Soil sampler (bottom-right) -->
    <g transform="translate(545,275)">
      <rect x="-32" y="-18" width="64" height="44" fill="#fff" stroke="#68625b" stroke-width="1.5" rx="5"/>
      <circle cx="0" cy="-2" r="8" fill="none" stroke="#68625b" stroke-width="1.2"/>
      <line x1="0" y1="6" x2="0" y2="14" stroke="#68625b" stroke-width="1.2"/>
      <text x="0" y="18" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="600" fill="#68625b">Geochem</text>
    </g>
    <text x="545" y="307" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#68625b">P, Cu, Pb, Zn</text>
    <line x1="500" y1="270" x2="365" y2="215" stroke="#68625b" stroke-width="1.5" marker-end="url(#arrowGray10)"/>

    <!-- Integration box at center-bottom -->
    <rect x="220" y="248" width="180" height="55" fill="#fff" stroke="#111" stroke-width="1.5" rx="6"/>
    <text x="310" y="268" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" font-weight="600" fill="#111">GIS Integration Layer</text>
    <text x="310" y="282" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">Overlay · Joint inversion</text>
    <text x="310" y="294" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">Bayesian fusion · Confidence map</text>

    <!-- Connection from target to integration -->
    <line x1="310" y1="235" x2="310" y2="248" stroke="#111" stroke-width="1" stroke-dasharray="3,2"/>

    <!-- Satellite (top-center) -->
    <g transform="translate(310,55)">
      <rect x="-25" y="-15" width="50" height="36" fill="#fff" stroke="#d92b1f" stroke-width="1.5" rx="5"/>
      <line x1="-25" y1="0" x2="-35" y2="0" stroke="#d92b1f" stroke-width="1.5"/>
      <line x1="25" y1="0" x2="35" y2="0" stroke="#d92b1f" stroke-width="1.5"/>
      <text x="0" y="3" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="600" fill="#d92b1f">Remote</text>
      <text x="0" y="14" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="600" fill="#d92b1f">sensing</text>
    </g>
    <text x="310" y="86" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#68625b">SAR, optical, thermal</text>
    <line x1="310" y1="94" x2="310" y2="145" stroke="#d92b1f" stroke-width="1.5" marker-end="url(#arrowRed10)"/>

    <!-- Confidence label -->
    <text x="310" y="155" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111" font-style="italic">Confidence ↑ as methods converge</text>

    <!-- Arrow markers -->
    <defs>
      <marker id="arrowBlue10" markerWidth="8" markerHeight="6" refX="8" refY="3" orient="auto"><polygon points="0,0 8,3 0,6" fill="#1e4f8a"/></marker>
      <marker id="arrowGreen10" markerWidth="8" markerHeight="6" refX="8" refY="3" orient="auto"><polygon points="0,0 8,3 0,6" fill="#165d34"/></marker>
      <marker id="arrowEarth10" markerWidth="8" markerHeight="6" refX="8" refY="3" orient="auto"><polygon points="0,0 8,3 0,6" fill="#8b5e00"/></marker>
      <marker id="arrowGray10" markerWidth="8" markerHeight="6" refX="8" refY="3" orient="auto"><polygon points="0,0 8,3 0,6" fill="#68625b"/></marker>
      <marker id="arrowRed10" markerWidth="8" markerHeight="6" refX="8" refY="3" orient="auto"><polygon points="0,0 8,3 0,6" fill="#d92b1f"/></marker>
    </defs>
  </svg>
  <p class="diagram-caption">Multi-method integration: five complementary techniques—magnetometry, GPR, ERT, geochemical sampling, and remote sensing—each sensitive to different physical properties, converge on the same target. A GIS integration layer combines evidence through overlay, joint inversion, and Bayesian fusion to build a high-confidence site interpretation.</p>
</div>

## Method complementarity

### What each technique reveals

Understanding what each method actually measures is prerequisite to meaningful integration:

| Method | Physical basis | Detects well | Misses or struggles with |
|--------|---------------|--------------|--------------------------|
| Magnetometry | Magnetic susceptibility contrast | Fired materials, hearths, kilns, ditches | Non-magnetic stone, areas with magnetic geology |
| GPR | Dielectric permittivity contrast | Walls, voids, stratigraphic interfaces | High-conductivity soils, deep targets |
| EMI | Electrical conductivity | Metal, wet fills, clay structures | Deep features, magnetically quiet sites |
| Satellite SAR | Surface roughness, moisture | Large-scale patterns, buried channels | Fine details, vegetated areas (C-band) |
| Optical imagery | Reflectance variations | Crop marks, soil marks, shadow patterns | Buried features with no surface expression |
| XRF | Elemental composition | Metallurgical residue, mineral processing | Organic remains, low-concentration signals |
| Geochemical sampling | Element distribution | Activity zones, occupation intensity | Point-specific features, rapid assessment |

### Complementarity matrix

Methods complement each other when one detects what another misses. The **complementarity score** can be estimated as:

$$
C_{ij} = 1 - \frac{|S_i \cap S_j|}{|S_i \cup S_j|}
$$

where $S_i$ and $S_j$ are the sets of feature types each method detects. High complementarity (approaching 1) means methods reveal different information; low complementarity (approaching 0) means redundancy.

**Practical complementarity pairs for Central Asia:**

- **Magnetometry + GPR**: Mag detects thermoremanent features; GPR detects structural boundaries
- **SAR + Magnetometry**: SAR reveals landscape context; mag reveals activity hotspots
- **Geochemistry + EMI**: Chemistry shows activity type; EMI shows extent and intensity
- **Optical + Thermal IR**: Optical shows surface marks; thermal reveals subsurface moisture

<div class="info-box tip">
  <strong>Key idea</strong>
  Choose method combinations that maximize complementarity for your target feature types, not methods that measure the same thing in different ways.
</div>

## Survey design strategy

### Reconnaissance to detailed investigation

Effective survey follows a hierarchical workflow from broad reconnaissance to targeted investigation:

**Stage 1: Regional reconnaissance**
- Satellite imagery analysis (optical + SAR)
- Historical map review and archive research
- Scale: 10-1000 km²
- Output: Candidate site locations, landscape context

**Stage 2: Site identification**
- Drone survey (RGB, multispectral, thermal)
- Rapid field walkover with GPS
- Scale: 1-100 hectares
- Output: Site boundaries, surface features, priority zones

**Stage 3: Geophysical mapping**
- Magnetometry and/or EMI full coverage
- GPR transects at priority locations
- Scale: 0.1-10 hectares
- Output: Anomaly maps, feature hypotheses

**Stage 4: Targeted investigation**
- High-resolution GPR grids
- Geochemical sampling at anomalies
- XRF spot measurements
- Scale: 100-1000 m²
- Output: Feature characterization, excavation targets

### Resource allocation model

Survey efficiency requires balancing coverage against resolution. The **coverage-resolution tradeoff** follows:

$$
T_{total} = A \cdot \frac{1}{v \cdot s} + n_{targets} \cdot t_{detailed}
$$

where:
- $A$ = survey area
- $v$ = traverse speed
- $s$ = line spacing
- $n_{targets}$ = number of detailed investigation targets
- $t_{detailed}$ = time per detailed target

Optimizing this equation guides decisions about how much area to survey at what resolution.

## Data integration approaches

### Visual overlay

The simplest integration approach places georeferenced datasets as layers for visual comparison:

1. Export each dataset as a georeferenced raster (GeoTIFF)
2. Load into GIS software with transparency controls
3. Toggle layers on/off to compare feature expressions
4. Create composite maps with multiple data types

**Advantages**: Rapid, intuitive, requires minimal processing
**Limitations**: Subjective interpretation, no quantitative combination

### GIS-based integration

Geographic Information Systems enable systematic spatial analysis:

**Raster algebra**: Combine datasets mathematically

$$
I_{combined} = w_1 \cdot R_1 + w_2 \cdot R_2 + ... + w_n \cdot R_n
$$

where $R_i$ are normalized raster layers and $w_i$ are weights.

**Boolean overlay**: Define criteria for feature identification

$$
Feature = (Mag > threshold_1) \cap (GPR > threshold_2) \cap (Chem > threshold_3)
$$

<div class="info-box tip">
  <strong>Key idea</strong>
  GIS-based approaches provide excellent results with accessible tools. Joint inversion produces more rigorous integration but requires specialized software.
</div>

## Scale matching

### The scale hierarchy

Different methods operate at different spatial scales. Effective integration requires matching scales appropriately:

| Platform | Typical resolution | Coverage per day | Best for |
|----------|-------------------|------------------|----------|
| Satellite | 0.3-30 m | 100-10,000 km² | Regional patterns |
| Drone | 2-10 cm | 10-100 ha | Site mapping |
| Ground geophysics | 10-100 cm | 0.5-5 ha | Feature detection |
| Point sampling | cm-scale | 50-200 points | Characterization |

### Registration accuracy requirements

For meaningful overlay, datasets must be registered to common coordinates within accuracy appropriate to the target scale:

$$
\epsilon_{registration} < \frac{r_{finest}}{3}
$$

where $r_{finest}$ is the resolution of the finest dataset. For 25 cm GPR data combined with 50 cm magnetometry, registration should be accurate to ~8 cm.

## Coordinate system harmonization

### Coordinate reference systems

Central Asia spans multiple UTM zones and uses various national coordinate systems:

- **WGS84**: Global standard, use for data exchange
- **UTM zones 41-45N**: Projected coordinates for local work
- **Pulkovo 1942**: Legacy Soviet system, still common in archives
- **National systems**: Kyrgyzstan, Kazakhstan, etc. have local variants

### Transformation workflow

1. **Identify source CRS** for each dataset
2. **Define target CRS** (typically UTM for local work, WGS84 for reporting)
3. **Apply transformation** using appropriate parameters
4. **Validate** by checking known control points

The transformation between two projected systems follows:

$$
\begin{bmatrix} E' \\ N' \end{bmatrix} = \mathbf{R} \cdot \begin{bmatrix} E \\ N \end{bmatrix} + \begin{bmatrix} \Delta E \\ \Delta N \end{bmatrix}
$$

where $\mathbf{R}$ is a rotation matrix and $\Delta E, \Delta N$ are translation offsets.

### Ground control points

For legacy datasets without metadata, establish transformation parameters using ground control points (GCPs):

1. Identify recognizable features in the legacy dataset
2. Measure the same features with modern GNSS
3. Calculate transformation parameters by least squares
4. Apply transformation and check residuals

Minimum GCPs required: 3 for affine transformation, 6+ for polynomial correction.

<div class="info-box tip">
  <strong>Key idea</strong>
  Never assume coordinate systems match. Always verify by checking feature positions against independent references (satellite imagery, GNSS measurements) before combining datasets.
</div>

## Interpretation workflow

### Hypothesis testing with multiple evidence lines

Integration supports rigorous interpretation through hypothesis testing:

**Step 1**: Generate hypotheses from initial data
- "This magnetic dipole represents a kiln"
- "This radar reflection is a wall foundation"

**Step 2**: Predict what other methods should show
- If kiln: elevated phosphorus, fired clay, ash deposits
- If wall: linear continuation, resistivity contrast

**Step 3**: Collect additional data to test predictions

**Step 4**: Evaluate consistency
- Consistent evidence strengthens hypothesis
- Contradictory evidence requires revised interpretation

### Evidence weighting

Weight evidence by **independence** (truly different measurements count more), **specificity** (unique signatures count more than ambiguous ones), and **quality** (high-SNR data counts more than noisy data).

### Anomaly classification

Classify integrated anomalies systematically:

| Class | Criteria | Interpretation confidence |
|-------|----------|--------------------------|
| A | 3+ methods agree, high amplitude | Strong candidate |
| B | 2 methods agree, moderate amplitude | Probable feature |
| C | 1 method, high amplitude | Possible feature |
| D | 1 method, low amplitude | Uncertain |

## Central Asia case studies

### Case 1: Caravanserai identification (Kazakhstan)

**Methods**: SAR + drone multispectral + magnetometry + geochemistry

SAR coherence maps identified rectangular low-coherence zones along trade routes. Drone survey confirmed vegetation anomalies; magnetometry revealed room blocks and courtyard structures; phosphorus enrichment confirmed intensive occupation. **Result**: Three previously unknown Silk Road caravanserais documented.

### Case 2: Bronze Age metallurgical complex (Kyrgyzstan)

**Methods**: Thermal IR + EMI + XRF + magnetometry

ASTER thermal anomalies identified slag deposits. EMI mapped conductive zones across 15 hectares. XRF transects showed Cu-Pb-As enrichment; magnetometry revealed 12 furnace locations. **Result**: Copper smelting operation spanning 200+ years reconstructed.

### Case 3: Irrigation system reconstruction (Uzbekistan)

**Methods**: Corona imagery + Sentinel-2 + GPR + geochemistry

Corona revealed canal networks invisible in modern cultivation. Sentinel-2 NDVI showed differential crop vigor; GPR documented canal profiles; geochemistry confirmed fill sequences. **Result**: 45 km of irrigation infrastructure mapped.

<div class="info-box tip">
  <strong>Key idea</strong>
  Method selection should follow research questions. Different site types require different combinations based on their expected signatures.
</div>

## Reporting and documentation standards

### Data documentation requirements

Each integrated dataset requires:

1. **Metadata**: CRS, resolution, acquisition date, instrument parameters
2. **Processing history**: All transformations, filters, and corrections applied
3. **Quality indicators**: Noise levels, coverage gaps, registration accuracy
4. **Uncertainty estimates**: Position error, measurement precision

### Integration documentation

For combined interpretations:

1. **Source datasets**: List all inputs with references
2. **Integration method**: Document how datasets were combined
3. **Weighting decisions**: Explain any subjective choices
4. **Alternative interpretations**: Note what else the evidence might support

### Archive formats

For long-term preservation:

- **Raster data**: GeoTIFF with embedded metadata
- **Vector data**: GeoPackage or shapefile with .prj file
- **Point data**: CSV with coordinate columns and attribute definitions
- **Documentation**: PDF/A with embedded images

## Multi-method survey checklist

<ul class="checklist">
  <li>Define research questions and target feature types</li>
  <li>Assess site conditions (soil type, vegetation, terrain)</li>
  <li>Select complementary methods based on expected signatures</li>
  <li>Plan hierarchical survey from reconnaissance to detailed</li>
  <li>Establish common coordinate reference system</li>
  <li>Deploy GNSS control points across site</li>
  <li>Collect regional reconnaissance data (satellite, drone)</li>
  <li>Conduct geophysical survey with systematic coverage</li>
  <li>Target detailed investigation at anomaly locations</li>
  <li>Register all datasets to common grid</li>
  <li>Normalize datasets to comparable scales</li>
  <li>Perform visual overlay comparison</li>
  <li>Apply quantitative integration (raster algebra, classification)</li>
  <li>Generate hypotheses from combined evidence</li>
  <li>Test hypotheses against additional data lines</li>
  <li>Classify anomalies by confidence level</li>
  <li>Document integration methods and decisions</li>
  <li>Prepare georeferenced archive with metadata</li>
  <li>Report findings with uncertainty estimates</li>
</ul>

## Exercises

<div class="exercise" id="exercise-1" data-difficulty="intermediate">
<h4>Exercise 1: Complementarity calculation</h4>
<p>Method A detects feature types {hearths, kilns, pits}. Method B detects {walls, floors, pits}. Calculate the complementarity score between methods A and B.</p>
<details>
<summary>Show solution</summary>
<p>Intersection: {pits}, size = 1<br>
Union: {hearths, kilns, pits, walls, floors}, size = 5<br>
C_AB = 1 - (1/5) = 1 - 0.2 = <strong>0.8</strong><br>
High complementarity indicates these methods reveal mostly different feature types.</p>
</details>
</div>

<div class="exercise" id="exercise-2" data-difficulty="intermediate">
<h4>Exercise 2: Survey time estimation</h4>
<p>A 5-hectare site requires magnetometry at 0.5 m line spacing. Walking speed is 1.5 m/s. Additionally, 8 detailed GPR grids will take 45 minutes each. Calculate total survey time.</p>
<details>
<summary>Show solution</summary>
<p>Area = 50,000 m². Line length for full coverage ≈ 50,000/0.5 = 100,000 m traverse<br>
Time for mag = 100,000/1.5 = 66,667 seconds = 18.5 hours<br>
GPR time = 8 × 45 = 360 minutes = 6 hours<br>
Total = <strong>24.5 hours</strong> (approximately 3 field days)</p>
</details>
</div>

<div class="exercise" id="exercise-3" data-difficulty="beginner">
<h4>Exercise 3: Registration accuracy</h4>
<p>You are combining 20 cm GPR data with 50 cm magnetometry. What registration accuracy is required?</p>
<details>
<summary>Show solution</summary>
<p>Finest resolution = 20 cm<br>
Required accuracy < 20/3 = <strong>6.7 cm</strong><br>
RTK GNSS easily achieves this; standard GPS does not.</p>
</details>
</div>

<div class="exercise" id="exercise-4" data-difficulty="intermediate">
<h4>Exercise 4: Raster algebra</h4>
<p>Magnetometry layer ranges 0-100 nT. GPR amplitude ranges 0-255. To combine with equal weights, what normalization should you apply?</p>
<details>
<summary>Show solution</summary>
<p>Normalize each to 0-1 range:<br>
Mag_norm = Mag / 100<br>
GPR_norm = GPR / 255<br>
Combined = 0.5 × Mag_norm + 0.5 × GPR_norm<br>
Result ranges <strong>0-1</strong> with equal contribution from each method.</p>
</details>
</div>

<div class="exercise" id="exercise-5" data-difficulty="advanced">
<h4>Exercise 5: Boolean overlay</h4>
<p>Define a Boolean expression for "probable kiln" given: Mag > 50 nT, GPR shows hyperbolic reflection, Phosphorus EF > 3.</p>
<details>
<summary>Show solution</summary>
<p>Probable_kiln = (Mag > 50) AND (GPR_hyperbola = TRUE) AND (P_EF > 3)<br>
All three conditions must be met. This identifies locations where magnetic, structural, and chemical evidence converge on kiln interpretation.</p>
</details>
</div>

<div class="exercise" id="exercise-6" data-difficulty="intermediate">
<h4>Exercise 6: UTM zone selection</h4>
<p>A survey site is located at 72.5°E, 40.2°N. Which UTM zone should be used?</p>
<details>
<summary>Show solution</summary>
<p>UTM zone = floor((longitude + 180) / 6) + 1<br>
Zone = floor((72.5 + 180) / 6) + 1 = floor(252.5/6) + 1 = 42 + 1 = <strong>43N</strong><br>
EPSG code: 32643</p>
</details>
</div>

<div class="exercise" id="exercise-7" data-difficulty="beginner">
<h4>Exercise 7: Scale matching</h4>
<p>Satellite imagery has 10 m resolution. Magnetometry has 0.5 m resolution. By what factor must you upsample the satellite data to match?</p>
<details>
<summary>Show solution</summary>
<p>Factor = 10 / 0.5 = <strong>20×</strong><br>
Each satellite pixel becomes a 20×20 grid of interpolated values. Note: this does not add real information, only allows overlay.</p>
</details>
</div>

<div class="exercise" id="exercise-8" data-difficulty="advanced">
<h4>Exercise 8: Confidence calculation</h4>
<p>Three independent methods give P(null) = 0.3, 0.4, 0.5 with equal weights. Calculate combined feature confidence.</p>
<details>
<summary>Show solution</summary>
<p>C = (1-0.3)^(1/3) × (1-0.4)^(1/3) × (1-0.5)^(1/3)<br>
C = 0.7^0.33 × 0.6^0.33 × 0.5^0.33<br>
C = 0.888 × 0.843 × 0.794 = <strong>0.59</strong><br>
Moderate-high confidence from combined evidence.</p>
</details>
</div>

<div class="exercise" id="exercise-9" data-difficulty="intermediate">
<h4>Exercise 9: GCP requirements</h4>
<p>A legacy Soviet map needs georeferencing. You can identify 8 features that still exist. Is this sufficient for polynomial correction? What transformation order can you use?</p>
<details>
<summary>Show solution</summary>
<p>Polynomial order n requires minimum (n+1)(n+2)/2 GCPs:<br>
1st order: 3 GCPs (affine)<br>
2nd order: 6 GCPs<br>
3rd order: 10 GCPs<br>
With 8 GCPs: <strong>2nd order polynomial</strong> is possible with 2 redundant points for error checking.</p>
</details>
</div>

<div class="exercise" id="exercise-10" data-difficulty="beginner">
<h4>Exercise 10: Method selection</h4>
<p>You need to survey a site with highly saline soils. Rank GPR, magnetometry, and EMI by expected effectiveness.</p>
<details>
<summary>Show solution</summary>
<p>1. <strong>Magnetometry</strong> - unaffected by salinity<br>
2. <strong>EMI</strong> - affected but still functional<br>
3. <strong>GPR</strong> - severely attenuated in conductive soils<br>
GPR may be nearly useless; magnetometry is the primary method.</p>
</details>
</div>

<div class="exercise" id="exercise-11" data-difficulty="intermediate">
<h4>Exercise 11: Hierarchical survey design</h4>
<p>A 200 km² region needs survey. You have 10 field days. Design a hierarchical approach specifying area coverage at each stage.</p>
<details>
<summary>Show solution</summary>
<p>Stage 1 (remote): Full 200 km² satellite analysis (pre-field)<br>
Stage 2 (2 days): Drone survey of 20 candidate sites, ~5 km² total<br>
Stage 3 (6 days): Magnetometry of 10 priority sites, ~30 ha total<br>
Stage 4 (2 days): Detailed GPR at 5 best anomaly clusters, ~2 ha total<br>
Progressive focusing from <strong>regional to site-specific</strong>.</p>
</details>
</div>

<div class="exercise" id="exercise-12" data-difficulty="advanced">
<h4>Exercise 12: Fuzzy classification</h4>
<p>Assign fuzzy membership values (0-1) for "settlement" given: Mag anomaly = 35 nT (threshold 50), Phosphorus EF = 4 (threshold 3), Surface pottery = present.</p>
<details>
<summary>Show solution</summary>
<p>μ_mag = 35/50 = 0.7 (below threshold, partial membership)<br>
μ_P = min(4/3, 1) = 1.0 (exceeds threshold)<br>
μ_pottery = 1.0 (binary presence)<br>
μ_settlement = min(0.7, 1.0, 1.0) = <strong>0.7</strong><br>
Probable settlement, limited by magnetic evidence.</p>
</details>
</div>

<div class="exercise" id="exercise-13" data-difficulty="beginner">
<h4>Exercise 13: Data format selection</h4>
<p>You need to archive magnetometry data for a national repository. What format and metadata should you include?</p>
<details>
<summary>Show solution</summary>
<p>Format: <strong>GeoTIFF</strong> with embedded CRS<br>
Metadata: Instrument type, survey date, line spacing, sensor height, CRS (EPSG code), processing steps, operator, project ID<br>
Additional: PDF documentation with survey parameters and quality notes.</p>
</details>
</div>

<div class="exercise" id="exercise-14" data-difficulty="intermediate">
<h4>Exercise 14: Anomaly classification</h4>
<p>An anomaly shows: strong magnetic dipole (80 nT), clear GPR hyperbola, elevated Cu but not P. Classify and interpret.</p>
<details>
<summary>Show solution</summary>
<p>Classification: <strong>Class A</strong> (3 methods agree)<br>
Interpretation: Metalworking feature (not habitation)<br>
Reasoning: High mag + GPR structure + Cu without P suggests smithing location rather than domestic occupation. The lack of phosphorus rules out food processing or waste disposal.</p>
</details>
</div>

<div class="exercise" id="exercise-15" data-difficulty="advanced">
<h4>Exercise 15: Contradiction resolution</h4>
<p>Magnetometry shows strong linear anomaly (wall?), but GPR shows nothing at the same location. List three possible explanations.</p>
<details>
<summary>Show solution</summary>
<p>1. <strong>Magnetic enhancement without structure</strong>: burned surface or magnetically enriched soil, not a constructed feature<br>
2. <strong>GPR attenuation</strong>: high clay content absorbing radar signal<br>
3. <strong>Depth mismatch</strong>: feature is deep enough for mag but below GPR range<br>
Additional investigation (EMI, excavation) needed to resolve.</p>
</details>
</div>

<div class="exercise" id="exercise-16" data-difficulty="intermediate">
<h4>Exercise 16: Coverage optimization</h4>
<p>Budget allows 20 person-days. Compare: (A) 8 ha magnetometry at 0.5 m spacing, or (B) 4 ha mag + 2 ha detailed GPR. Which reveals more?</p>
<details>
<summary>Show solution</summary>
<p>Option A: More coverage, single method<br>
Option B: Less coverage, complementary methods<br>
<strong>Option B is preferred</strong> if research questions require feature characterization. Option A if reconnaissance/detection is the primary goal. Integration value outweighs coverage for most archaeological questions.</p>
</details>
</div>

<div class="exercise" id="exercise-17" data-difficulty="beginner">
<h4>Exercise 17: Coordinate transformation</h4>
<p>A point has coordinates E=500000, N=4500000 in UTM 43N. After transformation to Pulkovo 1942, E shifts by +120 m, N by -85 m. What are the new coordinates?</p>
<details>
<summary>Show solution</summary>
<p>E' = 500000 + 120 = <strong>500120</strong><br>
N' = 4500000 - 85 = <strong>4499915</strong><br>
Note: Real transformations are more complex with rotation and scale factors; this is a simplified translation example.</p>
</details>
</div>

<div class="exercise" id="exercise-18" data-difficulty="advanced">
<h4>Exercise 18: Joint interpretation</h4>
<p>Design a hypothesis test: "This circular anomaly is a burial mound." What would magnetometry, GPR, geochemistry, and surface survey each predict?</p>
<details>
<summary>Show solution</summary>
<p><strong>Mag prediction</strong>: Ring of enhanced susceptibility (ditch), central quiet zone or dipoles (chamber)<br>
<strong>GPR prediction</strong>: Hyperbolic reflections from chamber walls, layered fill<br>
<strong>Geochemistry prediction</strong>: Elevated Ca and P from bone decomposition<br>
<strong>Surface prediction</strong>: Slight topographic rise, possible stone scatter<br>
If all four predictions match observations, hypothesis is strongly supported.</p>
</details>
</div>

<div class="exercise" id="exercise-19" data-difficulty="intermediate">
<h4>Exercise 19: Temporal integration</h4>
<p>You have 1970s Corona imagery showing a mound, 2020 Sentinel showing agricultural field, and 2023 drone showing slight crop mark. How do you integrate temporally?</p>
<details>
<summary>Show solution</summary>
<p>1. <strong>Corona</strong>: Baseline—mound existed pre-agriculture<br>
2. <strong>Sentinel</strong>: Confirms mound is now leveled by plowing<br>
3. <strong>Drone</strong>: Crop mark indicates subsurface remains persist<br>
Integration: Site is <strong>disturbed but not destroyed</strong>. Target geophysics at crop mark location; expect truncated features.</p>
</details>
</div>

<div class="exercise" id="exercise-20" data-difficulty="beginner">
<h4>Exercise 20: Layer ordering</h4>
<p>In GIS, you have: satellite base map, magnetometry, GPR, geochemical points. What layer order (bottom to top) allows best visualization?</p>
<details>
<summary>Show solution</summary>
<p>Bottom to top:<br>
1. Satellite base map (opaque)<br>
2. Magnetometry (50% transparent)<br>
3. GPR (50% transparent, different color ramp)<br>
4. Geochemical points (solid symbols on top)<br>
Base map provides context; rasters overlay with transparency; points always on top for visibility.</p>
</details>
</div>

<div class="exercise" id="exercise-21" data-difficulty="intermediate">
<h4>Exercise 21: Enrichment factor integration</h4>
<p>Three soil samples show: (1) P EF=5, Cu EF=2; (2) P EF=2, Cu EF=8; (3) P EF=6, Cu EF=7. Classify each by activity type.</p>
<details>
<summary>Show solution</summary>
<p>Sample 1: <strong>Habitation/organic waste</strong> (high P, low Cu)<br>
Sample 2: <strong>Metallurgical activity</strong> (low P, high Cu)<br>
Sample 3: <strong>Mixed activity zone</strong> (high both)—possible workshop within settlement<br>
Ratio P/Cu discriminates activity types effectively.</p>
</details>
</div>

<div class="exercise" id="exercise-22" data-difficulty="advanced">
<h4>Exercise 22: Uncertainty propagation</h4>
<p>Magnetometry has ±5 nT uncertainty. GPR depth has ±10% uncertainty. A feature is detected at 45±5 nT and 1.2±0.12 m depth. Express combined uncertainty.</p>
<details>
<summary>Show solution</summary>
<p>For independent measurements, uncertainties remain separate:<br>
<strong>Magnetic intensity</strong>: 45 ± 5 nT (11% relative uncertainty)<br>
<strong>Depth</strong>: 1.2 ± 0.12 m (10% relative uncertainty)<br>
Combined interpretation uncertainty should reflect the larger contributor. Feature characterization confidence is limited by the ~10% positional/dimensional uncertainty.</p>
</details>
</div>

<div class="exercise" id="exercise-23" data-difficulty="beginner">
<h4>Exercise 23: Method independence</h4>
<p>Are magnetometry and magnetic susceptibility measurements independent evidence? Explain.</p>
<details>
<summary>Show solution</summary>
<p><strong>No</strong>, they are not independent—both measure magnetic properties of soil/features. Agreement between them does not increase confidence multiplicatively because they respond to the same physical property.<br>
Contrast with mag + GPR, which measure different properties (magnetic vs. dielectric) and are truly independent.</p>
</details>
</div>

<div class="exercise" id="exercise-24" data-difficulty="intermediate">
<h4>Exercise 24: Sampling strategy</h4>
<p>A magnetometry survey reveals 15 anomalies across 3 hectares. You can collect 30 soil samples. Design an efficient sampling strategy.</p>
<details>
<summary>Show solution</summary>
<p>Strategy:<br>
- 15 samples at anomaly centers (1 per anomaly)<br>
- 10 samples at anomaly edges (characterize extent)<br>
- 5 samples in "quiet" zones (establish background)<br>
This provides <strong>anomaly characterization + background baseline</strong> for meaningful enrichment factor calculation.</p>
</details>
</div>

<div class="exercise" id="exercise-25" data-difficulty="advanced">
<h4>Exercise 25: SAR + optical fusion</h4>
<p>SAR shows high backscatter in a linear pattern. Optical shows no surface expression. What additional method would help interpret this? Why?</p>
<details>
<summary>Show solution</summary>
<p><strong>GPR or EMI</strong> would help most.<br>
High SAR backscatter without optical expression suggests subsurface feature affecting surface roughness/moisture (not visible soil mark). GPR would confirm structural cause; EMI would detect if the feature is conductive.<br>
The combination would distinguish: buried wall (GPR+), filled ditch (EMI+), or modern disturbance.</p>
</details>
</div>

<div class="exercise" id="exercise-26" data-difficulty="intermediate">
<h4>Exercise 26: Reporting completeness</h4>
<p>List five essential elements that must be included when reporting an integrated multi-method survey.</p>
<details>
<summary>Show solution</summary>
<p>1. <strong>Methods used</strong> with instrument specifications and parameters<br>
2. <strong>Coverage maps</strong> showing where each method was applied<br>
3. <strong>Integration approach</strong> describing how data were combined<br>
4. <strong>Coordinate reference system</strong> and registration accuracy achieved<br>
5. <strong>Confidence classifications</strong> for interpreted features with supporting evidence summary</p>
</details>
</div>

<div class="exercise" id="exercise-27" data-difficulty="beginner">
<h4>Exercise 27: Resolution limits</h4>
<p>Can you detect a 30 cm wide wall using 10 m satellite imagery? What about with 50 cm drone imagery? 25 cm GPR?</p>
<details>
<summary>Show solution</summary>
<p>10 m satellite: <strong>No</strong>—wall is 1/33 of pixel, invisible<br>
50 cm drone: <strong>Unlikely</strong>—wall is 0.6 pixels, may appear as subtle linear tone<br>
25 cm GPR: <strong>Yes</strong>—wall is 1.2 resolution cells, clearly detectable as reflection<br>
Rule: Feature should span at least 2-3 resolution cells for reliable detection.</p>
</details>
</div>

<div class="exercise" id="exercise-28" data-difficulty="advanced">
<h4>Exercise 28: Cost-benefit analysis</h4>
<p>Adding a third geophysical method costs 40% more but increases feature detection by only 15%. Under what circumstances is this justified?</p>
<details>
<summary>Show solution</summary>
<p>Justified when:<br>
1. <strong>High-value research</strong>: 15% more features significantly changes interpretation<br>
2. <strong>Excavation planned</strong>: Confident targeting saves more than 40% cost<br>
3. <strong>Characterization needed</strong>: Third method provides different information type<br>
4. <strong>Regulatory requirement</strong>: Compliance mandates multiple methods<br>
Not justified for pure reconnaissance where coverage matters more than completeness.</p>
</details>
</div>

<div class="exercise" id="exercise-29" data-difficulty="intermediate">
<h4>Exercise 29: Seasonal considerations</h4>
<p>You can survey in spring (wet soil, growing crops) or autumn (dry soil, harvested fields). Which season favors which methods?</p>
<details>
<summary>Show solution</summary>
<p><strong>Spring advantages</strong>:<br>
- Crop marks visible in growing vegetation<br>
- EMI conductivity contrasts enhanced by moisture<br>
- Thermal contrasts stronger in wet conditions<br>
<strong>Autumn advantages</strong>:<br>
- GPR penetration better in dry soil<br>
- Surface access easier after harvest<br>
- Magnetometry unaffected by season<br>
<strong>Optimal</strong>: Multi-season survey if resources allow.</p>
</details>
</div>

<div class="exercise" id="exercise-30" data-difficulty="advanced">
<h4>Exercise 30: Integration synthesis</h4>
<p>Design a complete multi-method survey for a suspected medieval fortress in Tajikistan highlands. Specify methods, sequence, integration approach, and expected deliverables.</p>
<details>
<summary>Show solution</summary>
<p><strong>Phase 1 - Remote</strong>: Corona + Sentinel-2 analysis, DEM extraction<br>
<strong>Phase 2 - Drone</strong>: RGB orthomosaic + thermal survey, 5 cm resolution<br>
<strong>Phase 3 - Geophysics</strong>: Magnetometry full coverage, GPR at structural anomalies<br>
<strong>Phase 4 - Ground truth</strong>: Geochemical sampling at activity zones, XRF at metallic anomalies<br>
<strong>Integration</strong>: GIS overlay with Boolean classification for structure/activity zones<br>
<strong>Deliverables</strong>: Georeferenced maps (CRS: UTM 42N), anomaly catalog with confidence ratings, interpreted site plan with phase hypothesis, archived datasets in GeoTIFF/GeoPackage format.</p>
</details>
</div>
