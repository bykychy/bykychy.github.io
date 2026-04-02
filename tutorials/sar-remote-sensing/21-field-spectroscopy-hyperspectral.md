---
layout: tutorial
title: "How Field Spectroscopy Enables Hyperspectral Analysis in Central Asia"
description: "A practical tutorial on spectroradiometers, spectral libraries, mineral identification, and hyperspectral image analysis for geology and vegetation, with 30 exercises and solutions."
difficulty: advanced
estimated_time: "3-4 hours"
series: sar-remote-sensing
order: 21
---

## Why spectroscopy matters

Every material on Earth's surface interacts with light in a characteristic way. When sunlight strikes a copper ore deposit, specific wavelengths are absorbed while others reflect back toward a sensor. This absorption pattern—the spectral signature—identifies the material as uniquely as a fingerprint identifies a person.

<div class="info-box tip">
  <strong>Key idea</strong>
  Spectroscopy identifies materials by measuring how they absorb and reflect electromagnetic radiation at specific wavelengths. The absorption features in a spectrum reveal molecular bonds and crystal structures, providing diagnostic information that no photograph or broadband image can capture.
</div>

For Central Asia, this capability transforms how we explore for minerals, monitor crop health, and assess land degradation. Hyperspectral sensors with hundreds of narrow bands detect specific absorption features that distinguish chalcopyrite from hematite, enabling targeted exploration that reduces costs and environmental impact.

<div class="learning-objectives">
  <div class="learning-objectives-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#1e4f8a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <h3>What you will learn</h3>
  </div>
  <ul>
    <li>Operate a field spectroradiometer, calibrate with white reference panels, and compute reflectance factors from raw detector counts</li>
    <li>Identify diagnostic absorption features for minerals (iron oxides, clays, carbonates) and vegetation (chlorophyll, water bands, red edge)</li>
    <li>Build spectral libraries with proper metadata and apply continuum removal for quantitative feature comparison</li>
    <li>Perform spectral unmixing using endmember extraction algorithms (PPI, N-FINDR, VCA) and constrained abundance estimation</li>
    <li>Apply hyperspectral analysis to Central Asian mineral exploration, crop stress detection, and salinization mapping using PRISMA and EnMAP data</li>
  </ul>
</div>

<div class="prerequisites">
  <div class="prerequisites-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#8b5e00" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
    <h3>Prerequisites</h3>
  </div>
  <ul>
    <li>Understanding of the electromagnetic spectrum (visible, NIR, SWIR wavelength ranges)</li>
    <li>Familiarity with remote sensing fundamentals (reflectance, radiance, atmospheric correction)</li>
    <li>Basic linear algebra (matrix operations, least-squares optimization) for spectral unmixing</li>
    <li>Prior exposure to multispectral satellite imagery analysis (e.g., Landsat, Sentinel-2)</li>
  </ul>
</div>

<div class="concept-diagram">
  <svg viewBox="0 0 620 320" xmlns="http://www.w3.org/2000/svg" style="max-width: 580px;">
    <rect x="0" y="0" width="620" height="320" fill="#f9f8f6" rx="8"/>
    <text x="310" y="22" text-anchor="middle" font-family="Inter, sans-serif" font-size="13" font-weight="bold" fill="#111">Field Spectroscopy: From Measurement to Spectral Identification</text>
    <defs>
      <marker id="arrBlue" markerWidth="8" markerHeight="6" refX="8" refY="3" orient="auto">
        <path d="M0,0 L8,3 L0,6" fill="#1e4f8a"/>
      </marker>
      <marker id="arrGray2" markerWidth="8" markerHeight="6" refX="8" refY="3" orient="auto">
        <path d="M0,0 L8,3 L0,6" fill="#68625b"/>
      </marker>
    </defs>
    <!-- Sun -->
    <circle cx="55" cy="55" r="18" fill="#f5d442" stroke="#8b5e00" stroke-width="1.5"/>
    <line x1="55" y1="75" x2="55" y2="90" stroke="#8b5e00" stroke-width="1" stroke-dasharray="3,2"/>
    <line x1="55" y1="75" x2="90" y2="105" stroke="#8b5e00" stroke-width="1" stroke-dasharray="3,2"/>
    <line x1="55" y1="75" x2="20" y2="105" stroke="#8b5e00" stroke-width="1" stroke-dasharray="3,2"/>
    <text x="55" y="55" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#8b5e00" dy="3">☀</text>
    <!-- Ground surface with rock sample -->
    <rect x="15" y="110" width="120" height="40" fill="#d4c4a0" stroke="#8b5e00" stroke-width="1" rx="3"/>
    <rect x="40" y="115" width="35" height="20" fill="#a08060" stroke="#8b5e00" stroke-width="1" rx="2"/>
    <text x="57" y="129" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#fff">Rock</text>
    <text x="75" y="163" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#111">Target surface</text>
    <!-- Reflected light arrow -->
    <line x1="75" y1="110" x2="120" y2="80" stroke="#1e4f8a" stroke-width="1.5" marker-end="url(#arrBlue)"/>
    <text x="108" y="88" font-family="Inter, sans-serif" font-size="8" fill="#1e4f8a">Reflected</text>
    <!-- Spectroradiometer -->
    <rect x="120" y="55" width="80" height="35" fill="#e8e6e2" stroke="#1e4f8a" stroke-width="1.5" rx="4"/>
    <text x="160" y="72" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#1e4f8a">Spectro-</text>
    <text x="160" y="83" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#1e4f8a">radiometer</text>
    <!-- White reference -->
    <rect x="15" y="175" width="60" height="25" fill="#fff" stroke="#68625b" stroke-width="1" rx="2"/>
    <text x="45" y="191" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#68625b">White ref.</text>
    <text x="45" y="214" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#68625b">ρ = DN_t/DN_r</text>
    <!-- Arrow from spectroradiometer to spectral curve -->
    <line x1="200" y1="72" x2="230" y2="72" stroke="#68625b" stroke-width="1.5" marker-end="url(#arrGray2)"/>
    <!-- Spectral curve panel -->
    <rect x="235" y="38" width="200" height="140" fill="#fff" stroke="#68625b" stroke-width="1" rx="4"/>
    <text x="335" y="55" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" font-weight="bold" fill="#111">Spectral Curve</text>
    <!-- Axes -->
    <line x1="260" y1="65" x2="260" y2="160" stroke="#111" stroke-width="1"/>
    <line x1="260" y1="160" x2="420" y2="160" stroke="#111" stroke-width="1"/>
    <text x="255" y="72" text-anchor="end" font-family="Inter, sans-serif" font-size="8" fill="#68625b">R</text>
    <text x="340" y="172" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#68625b">Wavelength (nm)</text>
    <text x="270" y="172" font-family="Inter, sans-serif" font-size="7" fill="#68625b">400</text>
    <text x="335" y="172" font-family="Inter, sans-serif" font-size="7" fill="#68625b">1400</text>
    <text x="405" y="172" font-family="Inter, sans-serif" font-size="7" fill="#68625b">2500</text>
    <!-- Spectral reflectance curve with absorption dips -->
    <polyline points="270,120 278,115 286,108 294,100 302,90 308,92 312,98 316,95 320,88 328,85 336,100 340,105 344,96 348,90 356,88 364,92 372,110 378,98 384,90 392,95 400,105 408,115" fill="none" stroke="#1e4f8a" stroke-width="2"/>
    <!-- Absorption feature labels with arrows -->
    <!-- Fe3+ at ~900nm -->
    <line x1="308" y1="98" x2="308" y2="110" stroke="#d92b1f" stroke-width="0.8"/>
    <text x="308" y="118" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#d92b1f">Fe³⁺</text>
    <!-- OH at ~1400nm -->
    <line x1="336" y1="105" x2="336" y2="116" stroke="#d92b1f" stroke-width="0.8"/>
    <text x="336" y="124" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#d92b1f">OH</text>
    <!-- H2O at ~1900nm -->
    <line x1="372" y1="110" x2="372" y2="122" stroke="#d92b1f" stroke-width="0.8"/>
    <text x="372" y="130" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#d92b1f">H₂O</text>
    <!-- Al-OH at ~2200nm -->
    <line x1="400" y1="105" x2="400" y2="118" stroke="#d92b1f" stroke-width="0.8"/>
    <text x="400" y="126" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#d92b1f">Al-OH</text>
    <!-- Arrow to identification -->
    <line x1="435" y1="108" x2="452" y2="108" stroke="#68625b" stroke-width="1.5" marker-end="url(#arrGray2)"/>
    <!-- Identification panel -->
    <rect x="457" y="38" width="150" height="140" fill="#fff" stroke="#165d34" stroke-width="1.5" rx="4"/>
    <text x="532" y="55" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" font-weight="bold" fill="#165d34">Identification</text>
    <!-- Mineral list -->
    <rect x="470" y="64" width="124" height="18" fill="#f0e8d8" rx="2"/>
    <text x="532" y="77" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#8b5e00">🔬 Kaolinite (2205 nm)</text>
    <rect x="470" y="86" width="124" height="18" fill="#f0e8d8" rx="2"/>
    <text x="532" y="99" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#8b5e00">🔬 Goethite (900 nm)</text>
    <rect x="470" y="108" width="124" height="18" fill="#dbecd0" rx="2"/>
    <text x="532" y="121" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#165d34">🌿 Chlorophyll (680 nm)</text>
    <rect x="470" y="130" width="124" height="18" fill="#d8e8f0" rx="2"/>
    <text x="532" y="143" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#1e4f8a">💧 Water (1450 nm)</text>
    <!-- Bottom: Unmixing workflow -->
    <rect x="50" y="195" width="520" height="55" fill="#fff" stroke="#1e4f8a" stroke-width="1.5" rx="4"/>
    <text x="310" y="212" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" font-weight="bold" fill="#1e4f8a">Spectral Unmixing Workflow</text>
    <!-- Steps -->
    <rect x="65" y="220" width="90" height="20" fill="#e8e6e2" rx="3"/>
    <text x="110" y="234" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#111">Endmembers</text>
    <line x1="158" y1="230" x2="172" y2="230" stroke="#68625b" stroke-width="1" marker-end="url(#arrGray2)"/>
    <rect x="175" y="220" width="90" height="20" fill="#e8e6e2" rx="3"/>
    <text x="220" y="234" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#111">PPI / N-FINDR</text>
    <line x1="268" y1="230" x2="282" y2="230" stroke="#68625b" stroke-width="1" marker-end="url(#arrGray2)"/>
    <rect x="285" y="220" width="90" height="20" fill="#e8e6e2" rx="3"/>
    <text x="330" y="234" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#111">R = Σ fᵢRᵢ + ε</text>
    <line x1="378" y1="230" x2="392" y2="230" stroke="#68625b" stroke-width="1" marker-end="url(#arrGray2)"/>
    <rect x="395" y="220" width="90" height="20" fill="#dbecd0" rx="3"/>
    <text x="440" y="234" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#165d34">Abundance map</text>
    <!-- Caption area -->
    <text x="310" y="275" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">Sensors: ASD FieldSpec (field) · PRISMA / EnMAP (space) · AVIRIS (airborne)</text>
    <text x="310" y="292" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">Coverage: 350–2500 nm | 3–10 nm spectral resolution | 230–285 bands</text>
  </svg>
  <p class="diagram-caption">Figure: Field spectroscopy workflow — sunlight reflects from a target surface, a spectroradiometer records the spectral curve, absorption features identify minerals and vegetation, and unmixing maps sub-pixel abundance.</p>
</div>

## Electromagnetic spectrum and spectral signatures

The electromagnetic spectrum for remote sensing spans 350 to 2500 nanometers:

**Visible range (400-700 nm)**: Chlorophyll absorption creates the green appearance of vegetation. Iron oxides produce red and yellow colors in soils.

**Near-infrared (700-1300 nm)**: Vegetation exhibits high reflectance due to cell structure scattering. The "red edge" around 700 nm is the most sensitive indicator of plant health.

**Shortwave infrared (1300-2500 nm)**: Clay minerals show features near 2200 nm. Carbonates absorb near 2300 nm. This region contains the richest information for geological mapping.

### Physics of absorption features

The absorption depth relates to material abundance and grain size:

$$D = 1 - \frac{R_b}{R_c}$$

where $$R_b$$ is reflectance at the absorption band center and $$R_c$$ is continuum reflectance.

<div class="info-box tip">
  <strong>Key idea</strong>
  The wavelength of an absorption feature identifies what material is present. The depth indicates how much of that material exists.
</div>

Continuum removal normalizes spectra for comparison:

$$R_{cr}(\lambda) = \frac{R(\lambda)}{C(\lambda)}$$

## Spectroradiometer operation and calibration

Field spectroradiometers measure reflected solar radiation. Full-range instruments (ASD FieldSpec, Spectral Evolution PSR+) cover 350-2500 nm with 3-10 nm resolution.

### Field measurement protocols

The reflectance factor calculation uses:

$$\rho(\lambda) = \frac{DN_{target}(\lambda)}{DN_{reference}(\lambda)} \times \rho_{reference}(\lambda)$$

<div class="info-box tip">
  <strong>Key idea</strong>
  The white reference measurement transforms raw detector counts into reflectance values that can be compared across instruments, locations, and dates.
</div>

Measure reference panels every 5-10 minutes. Collect 10-30 spectra per target and average to reduce noise.

## Building spectral libraries

A spectral library contains reference spectra with metadata: material identification, location, measurement conditions, and sample preparation. The USGS Spectral Library and ECOSTRESS provide community reference collections.

For mineral mapping, measure fresh rock surfaces—weathering alters spectral signatures. For vegetation, measure leaves at different canopy positions. For soils, measure at consistent moisture content.

## Absorption features: minerals, vegetation, water

### Mineral absorption features

**Iron-bearing minerals**: Fe²⁺ produces absorptions near 1000-1100 nm. Fe³⁺ produces features near 850-900 nm.

**Hydroxyl-bearing minerals**: Clay minerals show absorptions at 1400 nm (OH overtone) and 2200-2300 nm. The exact position distinguishes clay types:
- Kaolinite: doublet at 2160 and 2205 nm
- Montmorillonite: feature at 2205 nm
- Illite: feature at 2195 nm

**Carbonate minerals**: CO₃ produces absorptions at 2300-2350 nm.

<div class="info-box tip">
  <strong>Key idea</strong>
  Kaolinite always absorbs at 2205 nm because Al-OH bonds always have the same vibrational energy. This physical basis makes spectral identification reliable across different conditions.
</div>

### Vegetation spectral features

**Red edge**: The sharp reflectance increase from 680-750 nm. The red edge position (REP) indicates plant health:

$$REP = 700 + 40 \times \frac{R_{700} - R_{670}}{R_{740} - R_{700}}$$

**Water absorption bands**: Liquid water absorbs at 970, 1200, 1450, and 1940 nm.

The Normalized Difference Water Index:

$$NDWI = \frac{R_{860} - R_{1240}}{R_{860} + R_{1240}}$$

## Hyperspectral sensors

### Spaceborne sensors

**PRISMA (2019-present)**: 240 bands, 400-2500 nm, 30 m resolution. Data free for research.

**EnMAP (2022-present)**: 230 bands, 30 m resolution, excellent radiometric quality.

**EMIT (2022-present)**: 285 bands, 60 m resolution, targets mineral dust sources.

<div class="info-box tip">
  <strong>Key idea</strong>
  Spaceborne sensors provide global coverage but trade resolution for orbital viewing. Airborne sensors provide higher quality data for targeted areas. Field spectroscopy provides ground truth enabling both.
</div>

## Spectral unmixing and endmember extraction

At 30 m resolution, few pixels contain pure materials. The linear mixing model:

$$R_{mixed}(\lambda) = \sum_{i=1}^{n} f_i \times R_i(\lambda) + \epsilon(\lambda)$$

where $$f_i$$ is the fraction of endmember $$i$$.

### Endmember extraction algorithms

**Pixel Purity Index (PPI)**: Projects data onto random vectors, counting spectral extremes.

**N-FINDR**: Finds pixels maximizing simplex volume in spectral space.

**VCA**: Iteratively projects data orthogonal to identified endmembers.

### Abundance estimation

$$\min_f \|R_{mixed} - \sum f_i R_i\|^2$$

subject to $$\sum f_i = 1$$ and $$f_i \geq 0$$.

## Mineral mapping for exploration

Hydrothermal systems produce characteristic mineral assemblages indicating proximity to ore deposits.

**Phyllic alteration**: Quartz-sericite-pyrite shows 2200 nm absorption from sericite.

**Argillic alteration**: Clay minerals produce features in the 2160-2220 nm range.

The iron feature depth index:

$$IFD = 1 - \frac{2 \times R_{900}}{R_{750} + R_{1050}}$$

## Vegetation biochemistry

**Red edge chlorophyll index**:

$$CI_{red edge} = \frac{R_{750}}{R_{710}} - 1$$

**Water Index**:

$$WI = \frac{R_{900}}{R_{970}}$$

**Moisture Stress Index**:

$$MSI = \frac{R_{1600}}{R_{820}}$$

## Central Asia applications

**Mining**: Mapping sericite-pyrite alteration in Tien Shan gold prospects. EnMAP reveals clay mineral zonation in Kazakhstan copper provinces.

**Agriculture**: Detecting cotton stress in Fergana Valley 7-10 days before visible symptoms. Enabling 15-20% fertilizer savings in wheat fields.

**Salinization**: Mapping salt mineral assemblages. Tracking salinization expansion in the Aral Sea region.

<div class="info-box tip">
  <strong>Key idea</strong>
  Central Asia combines mineral resources, intensive agriculture, and environmental challenges that hyperspectral analysis addresses. The open terrain and clear atmospheric conditions make the region particularly suitable for these techniques.
</div>

## Checklist

<ul class="checklist">
  <li>Understand how absorption features identify materials</li>
  <li>Know spectral ranges (VNIR, SWIR) and characteristic absorbers</li>
  <li>Calibrate spectroradiometers using white reference panels</li>
  <li>Build spectral libraries with proper metadata</li>
  <li>Identify diagnostic absorption features for major mineral groups</li>
  <li>Recognize vegetation features (chlorophyll, red edge, water bands)</li>
  <li>Distinguish between linear and nonlinear mixing</li>
  <li>Extract endmembers using PPI, N-FINDR, or VCA</li>
  <li>Perform constrained spectral unmixing</li>
  <li>Map hydrothermal alteration minerals</li>
  <li>Calculate vegetation biochemistry indices</li>
  <li>Apply techniques to Central Asia applications</li>
  <li>Design field campaigns for image validation</li>
  <li>Perform atmospheric correction</li>
  <li>Interpret unmixing residuals</li>
</ul>

## Exercises

### Exercise 1: Absorption feature identification
**Task:** A spectrum shows features at 900 nm, 1400 nm, 2200 nm, and 2350 nm. What minerals might be present?

<details>
<summary>Show Solution</summary>
The 900 nm feature indicates iron oxide (goethite). The 1400 nm is OH overtone in clays. The 2200 nm indicates Al-OH bonds (kaolinite or montmorillonite). The 2350 nm suggests carbonate. This represents weathered carbonate rock with clay alteration and iron oxide staining.
</details>

### Exercise 2: Calculate reflectance
**Task:** Target reads 2500 counts, reference panel (98% reflectance) reads 5000 counts at 550 nm. What is target reflectance?

<details>
<summary>Show Solution</summary>
ρ = (2500/5000) × 0.98 = 0.49 or 49% reflectance.
</details>

### Exercise 3: Continuum removal
**Task:** Reflectances: 0.45 at 2100 nm, 0.30 at 2200 nm, 0.50 at 2300 nm. Calculate band depth at 2200 nm.

<details>
<summary>Show Solution</summary>
Continuum at 2200 nm: C = 0.45 + 0.05 × 0.5 = 0.475. Continuum-removed: 0.30/0.475 = 0.632. Band depth: D = 1 - 0.632 = 0.368 (36.8%).
</details>

### Exercise 4: Red edge position
**Task:** Calculate REP for R_670=0.05, R_700=0.15, R_740=0.45.

<details>
<summary>Show Solution</summary>
REP = 700 + 40 × (0.15-0.05)/(0.45-0.15) = 700 + 40 × 0.333 = 713.3 nm. Typical healthy vegetation.
</details>

### Exercise 5: Clay mineral identification
**Task:** Two samples show 2200 nm absorptions at 2195 nm and 2215 nm. Which clays?

<details>
<summary>Show Solution</summary>
2195 nm indicates illite. 2215 nm suggests kaolinite-smectite mixture or well-crystallized kaolinite.
</details>

### Exercise 6: Water index calculation
**Task:** R_900=0.42, R_970=0.35. Calculate Water Index.

<details>
<summary>Show Solution</summary>
WI = 0.42/0.35 = 1.20. Values above 1.0 indicate water absorption present—moderate content typical of non-stressed vegetation.
</details>

### Exercise 7: Linear unmixing setup
**Task:** Mixed pixel R_750=0.32 with vegetation (0.45), soil (0.25), shadow (0.05). Set up equation.

<details>
<summary>Show Solution</summary>
0.32 = f_veg × 0.45 + f_soil × 0.25 + f_shadow × 0.05, with f_veg + f_soil + f_shadow = 1.0. Need additional wavelengths to solve.
</details>

### Exercise 8: Verify unmixing
**Task:** Verify f_veg=0.4, f_soil=0.5, f_shadow=0.1 for Exercise 7.

<details>
<summary>Show Solution</summary>
0.4 × 0.45 + 0.5 × 0.25 + 0.1 × 0.05 = 0.31. Close to 0.32 (residual 0.01). Sum = 1.0 ✓
</details>

### Exercise 9: SNR calculation
**Task:** Mean reflectance 0.35, standard deviation 0.007. Calculate SNR.

<details>
<summary>Show Solution</summary>
SNR = 0.35/0.007 = 50:1. Acceptable for field measurements.
</details>

### Exercise 10: Iron feature depth
**Task:** Calculate IFD for R_750=0.35, R_900=0.15, R_1050=0.30.

<details>
<summary>Show Solution</summary>
IFD = 1 - (2 × 0.15)/(0.35 + 0.30) = 1 - 0.462 = 0.538. Strong iron absorption.
</details>

### Exercise 11: Chlorophyll index
**Task:** Calculate CI_red_edge for R_710=0.12, R_750=0.40.

<details>
<summary>Show Solution</summary>
CI = (0.40/0.12) - 1 = 2.33. Moderate to high chlorophyll content.
</details>

### Exercise 12: Atmospheric windows
**Task:** Spectrum shows noise at 1380-1420 nm and 1800-1950 nm. Cause and handling?

<details>
<summary>Show Solution</summary>
Atmospheric water vapor absorption bands. Exclude these wavelengths from analysis or interpolate across them.
</details>

### Exercise 13: Carbonate vs chlorite
**Task:** Both show features near 2330 nm. How to distinguish?

<details>
<summary>Show Solution</summary>
Chlorite shows additional 2250 nm feature. Carbonates have sharp, symmetric features; chlorite broader. Chlorite may show iron features in VNIR.
</details>

### Exercise 14: Endmember number
**Task:** PCA eigenvalues: 85%, 10%, 3%, 1%... How many endmembers?

<details>
<summary>Show Solution</summary>
3-4 endmembers. PC1-3 explain 98%. Start with 3, add 4th if residuals show patterns.
</details>

### Exercise 15: Reference timing
**Task:** Targets at 10:00-10:45, references at 9:55 and 10:50. Adequate?

<details>
<summary>Show Solution</summary>
No. 55-minute gap is too long. Reference every 5-10 minutes required.
</details>

### Exercise 16: NDWI calculation
**Task:** R_860=0.38, R_1240=0.22. Calculate NDWI.

<details>
<summary>Show Solution</summary>
NDWI = (0.38-0.22)/(0.38+0.22) = 0.267. Moderate water content, suggests stress.
</details>

### Exercise 17: Gypsum identification
**Task:** Features at 1450, 1490, 1540, 2215 nm. Is gypsum present?

<details>
<summary>Show Solution</summary>
Yes. Characteristic gypsum triplet (1450, 1490, 1535 nm) from structural water, plus 2215 nm SO4-H2O combination.
</details>

### Exercise 18: Vegetation stress
**Task:** Baseline REP=720 nm, current REP=710 nm. Interpret.

<details>
<summary>Show Solution</summary>
10 nm blue-shift indicates moderate stress. Chlorophyll decreasing. Possible water stress, nitrogen deficiency, or disease.
</details>

### Exercise 19: Unmixing RMSE
**Task:** RMSE ranges 0.01-0.15. Which pixels need examination?

<details>
<summary>Show Solution</summary>
RMSE > 0.05-0.08. High values suggest missing endmembers, nonlinear mixing, or calibration errors.
</details>

### Exercise 20: Alteration zoning
**Task:** Sericite increases from 5% (periphery) to 40% (center). Interpretation?

<details>
<summary>Show Solution</summary>
Indicates proximity to porphyry center. Phyllic alteration intensifies toward hydrothermal source. Center may overlie mineralization.
</details>

### Exercise 21: Sensor resolution
**Task:** EnMAP shows 3-pixel anomaly (90 m), AVIRIS shows 20+ pixels (80 m). Implications?

<details>
<summary>Show Solution</summary>
Consistent extent. AVIRIS reveals internal structure—possible core/halo pattern. Use AVIRIS for detailed mapping, EnMAP for reconnaissance.
</details>

### Exercise 22: Field strategy
**Task:** 4 hours, 10 km², 5 rock types. Design sampling.

<details>
<summary>Show Solution</summary>
30 min setup, 3 hours sampling (30 min/type), 30 min wrap-up. 3 locations per type, 15 spectra each. Reference every 5 min. GPS and photos.
</details>

### Exercise 23: Moisture effects
**Task:** Wet soil shows 1450/1900 nm features absent when dry. Effect on mineral ID?

<details>
<summary>Show Solution</summary>
Water masks mineral features. Measure soils air-dried. Use 2200 nm clay feature—less affected by moisture.
</details>

### Exercise 24: Kaolinite crystallinity
**Task:** Sample A: 15%/18% doublet depth. Sample B: 25%/28%. Interpretation?

<details>
<summary>Show Solution</summary>
Sample B has higher abundance and/or better crystallinity. Deeper features indicate finer grain size, higher purity, or better-ordered structure.
</details>

### Exercise 25: MSI drought monitoring
**Task:** Weekly MSI: 0.8, 0.9, 1.1, 1.4. Interpret.

<details>
<summary>Show Solution</summary>
Progressive water stress. MSI increases as water decreases. Week 4 likely shows visible wilting. Should irrigate by Week 3.
</details>

### Exercise 26: Mixed minerals
**Task:** Spectrum shows carbonate (2340 nm) and clay (2200 nm). Estimate abundance?

<details>
<summary>Show Solution</summary>
Band depth ratio, spectral unmixing, or spectral angle. Calibrate with known mixtures—absorption strength differs between minerals.
</details>

### Exercise 27: Shadow handling
**Task:** Mountain terrain has many shadowed pixels. How to handle?

<details>
<summary>Show Solution</summary>
Include shade as endmember (most common). Or: topographic correction with DEM, mask low-brightness pixels, or BRDF modeling.
</details>

### Exercise 28: Cross-calibration
**Task:** Field granite: 25% at 550 nm. USGS library: 30%. Cause?

<details>
<summary>Show Solution</summary>
Sample differences, grain size, weathering, moisture, or calibration offset. 5% within natural variation. Verify with uniform reference material.
</details>

### Exercise 29: Derivative analysis
**Task:** First derivative peaks at 722 nm. Information?

<details>
<summary>Show Solution</summary>
Peak is red edge position—inflection point. 722 nm indicates healthy vegetation (715-725 nm range). Derivative emphasizes shape over brightness.
</details>

### Exercise 30: Workflow design
**Task:** Design study for 500 km² salinization mapping in southern Kazakhstan.

<details>
<summary>Show Solution</summary>
**Preparation:** Obtain salinity maps, identify salt minerals, acquire PRISMA/EnMAP (late summer). **Field:** 50 sites, EC measurements, spectroradiometer, build local library. **Processing:** Atmospheric correction, endmember extraction, constrained unmixing. **Validation:** Compare with field EC, generate hazard maps, accuracy assessment. **Deliverables:** Salt distribution maps, severity classification, remediation priorities.
</details>

---

This tutorial introduced field spectroscopy and hyperspectral analysis fundamentals. The ability to identify materials through spectral signatures enables mineral exploration, agricultural monitoring, and environmental assessment across Central Asia.
