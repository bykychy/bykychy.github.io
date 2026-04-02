---
layout: tutorial
title: "How Geochemical Soil Sampling Reveals Activity Signatures in Central Asia"
description: "A math-first tutorial on soil chemistry, element ratios, sampling strategies, and activity pattern interpretation for Central Asia, with 30 exercises and solutions."
difficulty: advanced
estimated_time: "2-3 hours"
series: treasure-hunting
order: 7
---

## Why this tutorial matters

Soil remembers. Long after surface features erode and structures collapse, the chemistry of soil horizons preserves evidence of human activity. Geochemical soil sampling bridges the gap between **XRF point measurements** and **landscape-scale prospection**, allowing systematic detection of settlements, workshops, agricultural zones, and burial grounds across Central Asia's diverse terrain.

This tutorial builds on concepts from XRF analysis (elemental signatures) and integrates with remote sensing approaches. Where XRF tells you *what elements are present* in a sample, geochemical soil sampling tells you *how those elements vary spatially* and what that variation means for detecting past human activity.

<div class="info-box tip">
  <strong>Key idea</strong>
  Geochemical prospection works because human activities concentrate, disperse, and transform elements in predictable ways. The mathematics of anomaly detection lets us distinguish anthropogenic signals from natural background variation.
</div>

<div class="learning-objectives">
  <div class="learning-objectives-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#1e4f8a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <h3>What you will learn</h3>
  </div>
  <ul>
    <li>How element mobility classes (P, Cu, Pb, Zn, Ca, Fe) determine which soil chemistry signatures persist over archaeological timescales</li>
    <li>The enrichment-factor formula for distinguishing anthropogenic anomalies from natural geochemical background variation</li>
    <li>Optimal sampling grid design, depth selection, and preparation procedures for Central Asian steppe and desert soils</li>
    <li>Statistical methods—including kriging interpolation, principal component analysis, and multi-element ratios—for mapping activity zones</li>
    <li>Interpretation of geochemical signatures for settlement, metallurgy, animal husbandry, and burial contexts in Central Asian archaeology</li>
  </ul>
</div>

<div class="prerequisites">
  <div class="prerequisites-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#8b5e00" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
    <h3>Prerequisites</h3>
  </div>
  <ul>
    <li>Understanding of basic descriptive statistics (mean, standard deviation, percentiles) and hypothesis testing concepts</li>
    <li>Familiarity with XRF elemental analysis from the earlier tutorial in this series (Tutorial 04) or equivalent experience</li>
    <li>Working knowledge of sampling theory and grid-based spatial data collection from field survey or GIS coursework</li>
    <li>Comfort with ratio and logarithmic calculations used in enrichment factors and geochemical normalization</li>
  </ul>
</div>

<div class="concept-diagram">
  <svg viewBox="0 0 620 320" xmlns="http://www.w3.org/2000/svg" style="max-width: 580px;">
    <!-- Background -->
    <rect x="0" y="0" width="620" height="320" fill="#f8f7f4" rx="6"/>

    <!-- Title -->
    <text x="310" y="22" text-anchor="middle" font-family="Inter, sans-serif" font-size="13" font-weight="600" fill="#111">Geochemical Soil Sampling — Element Migration from Buried Feature</text>

    <!-- LEFT PANEL: Plan-view sampling grid -->
    <text x="150" y="42" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" font-weight="600" fill="#111">Plan view — Sampling grid</text>

    <!-- Grid dots -->
    <g fill="#68625b">
      <circle cx="70" cy="65" r="3"/><circle cx="110" cy="65" r="3"/><circle cx="150" cy="65" r="3"/><circle cx="190" cy="65" r="3"/><circle cx="230" cy="65" r="3"/>
      <circle cx="70" cy="100" r="3"/><circle cx="110" cy="100" r="3"/><circle cx="150" cy="100" r="3"/><circle cx="190" cy="100" r="3"/><circle cx="230" cy="100" r="3"/>
      <circle cx="70" cy="135" r="3"/><circle cx="110" cy="135" r="3"/><circle cx="150" cy="135" r="3"/><circle cx="190" cy="135" r="3"/><circle cx="230" cy="135" r="3"/>
      <circle cx="70" cy="170" r="3"/><circle cx="110" cy="170" r="3"/><circle cx="150" cy="170" r="3"/><circle cx="190" cy="170" r="3"/><circle cx="230" cy="170" r="3"/>
      <circle cx="70" cy="205" r="3"/><circle cx="110" cy="205" r="3"/><circle cx="150" cy="205" r="3"/><circle cx="190" cy="205" r="3"/><circle cx="230" cy="205" r="3"/>
    </g>

    <!-- Anomalous zone (hot colours) -->
    <ellipse cx="155" cy="135" rx="55" ry="40" fill="#d92b1f" opacity="0.15" stroke="#d92b1f" stroke-width="1" stroke-dasharray="4,3"/>
    <text x="155" y="232" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#d92b1f">Elevated P, Cu, Zn</text>

    <!-- Grid spacing annotation -->
    <line x1="70" y1="58" x2="110" y2="58" stroke="#68625b" stroke-width="0.8"/>
    <text x="90" y="55" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">10 m</text>

    <!-- Divider -->
    <line x1="285" y1="35" x2="285" y2="310" stroke="#68625b" stroke-width="0.5" stroke-dasharray="3,3"/>

    <!-- RIGHT PANEL: Cross-section -->
    <text x="450" y="42" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" font-weight="600" fill="#111">Cross-section — Upward element migration</text>

    <!-- Surface -->
    <line x1="310" y1="72" x2="590" y2="72" stroke="#8b5e00" stroke-width="2"/>
    <text x="315" y="67" font-family="Inter, sans-serif" font-size="10" fill="#8b5e00">Surface</text>

    <!-- Soil column layers -->
    <rect x="310" y="72" width="280" height="35" fill="#e8dcc8" rx="0"/>
    <text x="315" y="92" font-family="Inter, sans-serif" font-size="10" fill="#68625b">Modern A-horizon (0–20 cm)</text>

    <rect x="310" y="107" width="280" height="50" fill="#d9ccb0" rx="0"/>
    <text x="315" y="130" font-family="Inter, sans-serif" font-size="10" fill="#68625b">Anthropogenic layer (20–50 cm)</text>

    <rect x="310" y="157" width="280" height="60" fill="#c4b698" rx="0"/>
    <text x="315" y="185" font-family="Inter, sans-serif" font-size="10" fill="#68625b">Subsoil (50–100 cm)</text>

    <rect x="310" y="217" width="280" height="55" fill="#b0a486" rx="0"/>

    <!-- Buried feature -->
    <rect x="400" y="225" width="90" height="40" fill="#d92b1f" opacity="0.4" stroke="#d92b1f" stroke-width="1.5" rx="4"/>
    <text x="445" y="242" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" font-weight="600" fill="#d92b1f">Buried</text>
    <text x="445" y="255" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#d92b1f">workshop</text>

    <!-- Upward migration arrows -->
    <g stroke="#d92b1f" stroke-width="1.2" fill="#d92b1f" opacity="0.7">
      <line x1="430" y1="222" x2="430" y2="115"/><polygon points="430,115 427,122 433,122"/>
      <line x1="445" y1="222" x2="445" y2="85"/><polygon points="445,85 442,92 448,92"/>
      <line x1="460" y1="222" x2="460" y2="115"/><polygon points="460,115 457,122 463,122"/>
    </g>
    <text x="488" y="160" font-family="Inter, sans-serif" font-size="10" fill="#d92b1f" transform="rotate(-90,488,160)">Element diffusion</text>

    <!-- Sampling depth indicator -->
    <line x1="540" y1="72" x2="540" y2="107" stroke="#1e4f8a" stroke-width="2"/>
    <line x1="535" y1="72" x2="545" y2="72" stroke="#1e4f8a" stroke-width="2"/>
    <line x1="535" y1="107" x2="545" y2="107" stroke="#1e4f8a" stroke-width="2"/>
    <text x="555" y="93" font-family="Inter, sans-serif" font-size="10" fill="#1e4f8a">Sample</text>
    <text x="555" y="103" font-family="Inter, sans-serif" font-size="10" fill="#1e4f8a">15–30 cm</text>

    <!-- Concentration profile at far right -->
    <text x="450" y="290" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">Concentration →</text>
    <line x1="350" y1="280" x2="390" y2="280" stroke="#165d34" stroke-width="2"/>
    <text x="395" y="284" font-family="Inter, sans-serif" font-size="9" fill="#165d34">Background</text>
    <line x1="350" y1="296" x2="390" y2="296" stroke="#d92b1f" stroke-width="2"/>
    <text x="395" y="300" font-family="Inter, sans-serif" font-size="9" fill="#d92b1f">Anomaly (EF &gt; 2)</text>
  </svg>
  <p class="diagram-caption">Left: plan-view sampling grid over a site, with elevated concentrations (red ellipse) marking a buried activity area. Right: cross-section showing how elements migrate upward from a buried workshop through the soil column, reaching the optimal sampling depth of 15–30 cm.</p>
</div>

## Geochemical principles

### Element mobility in soils

Elements move through soil at different rates depending on their chemical properties. Understanding mobility is essential for interpreting what a soil sample actually represents.

The **mobility index** for an element can be approximated as:

$$
M_i = \frac{C_{soluble}}{C_{total}}
$$

where $C_{soluble}$ is the concentration extractable by weak acids or water, and $C_{total}$ is the total concentration. High mobility means rapid redistribution; low mobility means signals persist near their source.

**Mobility classes for key elements:**

| Element | Mobility | Archaeological implication |
|---------|----------|---------------------------|
| P (Phosphorus) | Low-Medium | Excellent activity marker |
| Cu (Copper) | Low | Metallurgical residue persists |
| Pb (Lead) | Very Low | Smelting signatures last millennia |
| Zn (Zinc) | Medium | Disperses but concentrates in activity zones |
| Ca (Calcium) | High | Indicates ash, lime, bone processing |
| Fe (Iron) | Low | Background normalizer, smithing indicator |
| Mn (Manganese) | Medium-High | Redox conditions, processing waste |

### Soil horizons and sampling depth

Central Asian soils vary from chernozems to desert soils. The critical sampling depth depends on soil type and target activity:

$$
d_{optimal} = d_{A} + \Delta_{disturbance}
$$

where $d_{A}$ is the A-horizon thickness and $\Delta_{disturbance}$ accounts for post-depositional mixing. For most archaeological prospection in Central Asia, sampling at **15-30 cm depth** captures anthropogenic enrichment while avoiding modern contamination.

<div class="info-box tip">
  <strong>Key idea</strong>
  Sampling too shallow captures modern pollution; sampling too deep misses anthropogenic layers. The A-horizon in Central Asian steppe soils is typically 20-40 cm thick.
</div>

### Anthropogenic enrichment factors

The **enrichment factor** quantifies how much an element is elevated above natural background:

$$
EF_i = \frac{C_i / C_{ref}}{C_{i,bg} / C_{ref,bg}}
$$

where:
- $C_i$ is concentration of element $i$ in the sample
- $C_{ref}$ is concentration of a reference element (typically Fe or Al)
- $C_{i,bg}$ and $C_{ref,bg}$ are background concentrations

An $EF > 2$ suggests moderate enrichment; $EF > 5$ indicates strong anthropogenic influence; $EF > 10$ points to intensive activity like metallurgy or waste disposal.

## Key elements for prospection work

### Phosphorus (P)

Phosphorus is the cornerstone of settlement detection. Human and animal waste, food processing, and bone disposal all concentrate P in occupation zones.

The **P accumulation rate** in settlement soils can be modeled as:

$$
\frac{dP}{dt} = k_{input} \cdot \rho_{occupation} - k_{loss} \cdot P
$$

where $k_{input}$ reflects activity intensity, $\rho_{occupation}$ is occupation density, and $k_{loss}$ is the slow leaching rate. At equilibrium, settlement P levels can be 3-10× background.

### Copper (Cu) and Zinc (Zn)

These elements mark metallurgical activity. The Cu/Zn ratio helps distinguish bronze working from brass processing:

$$
R_{CuZn} = \frac{C_{Cu}}{C_{Zn}}
$$

For bronze (Cu-Sn), expect $R_{CuZn} > 10$. For brass (Cu-Zn), expect $R_{CuZn} < 3$.

### Lead (Pb)

Lead persists in soils for millennia. Its immobility makes it excellent for detecting:
- Glazed ceramic production
- Silver-lead ore processing
- Smelting operations

Background Pb in Central Asian soils is typically 15-30 ppm; smelting sites can exceed 500 ppm.

### Calcium (Ca)

Elevated Ca indicates:
- Hearth ash concentrations
- Lime-plastered surfaces
- Bone processing areas
- Mortar and construction debris

The Ca/P ratio helps distinguish activity types:

$$
R_{CaP} = \frac{C_{Ca}}{C_{P}}
$$

High $R_{CaP}$ (>50) suggests ash or lime; low $R_{CaP}$ (<10) suggests organic waste or bone.

### Iron (Fe) and Manganese (Mn)

Iron serves two roles: as an activity marker (smithing, forging) and as a reference element for normalization. The Fe/Mn ratio responds to redox conditions:

$$
R_{FeMn} = \frac{C_{Fe}}{C_{Mn}}
$$

Disturbed soils and waterlogged deposits show characteristic Fe/Mn signatures.

## Sampling design

### Grid spacing

The sampling interval determines what features you can detect. The **Nyquist criterion** adapted for geochemistry suggests:

$$
\Delta x \leq \frac{D_{feature}}{2}
$$

where $D_{feature}$ is the minimum feature diameter you want to resolve. For settlement detection, 20-50 m grids are typical; for intra-site analysis, 2-5 m grids may be needed.

<div class="info-box tip">
  <strong>Key idea</strong>
  A 25 m grid can reliably detect features ≥50 m across. For courtyards, houses, or workshops, reduce spacing to 5-10 m.
</div>

### Sample size considerations

The number of samples needed depends on the area and expected variability:

$$
n \geq \frac{z^2 \cdot s^2}{\epsilon^2}
$$

where $z$ is the confidence coefficient (1.96 for 95%), $s$ is the estimated standard deviation, and $\epsilon$ is the acceptable error margin. For reconnaissance, aim for 3-5 samples per hectare; for detailed mapping, 25-100 samples per hectare.

### Composite sampling

Composite samples reduce cost while maintaining spatial resolution. Each composite combines 3-5 subsamples from a defined area:

$$
C_{composite} = \frac{1}{n} \sum_{j=1}^{n} C_j
$$

This averages out micro-scale variability and provides a robust estimate of local chemistry.

### Depth protocol for Central Asia

Based on regional soil characteristics, use this depth protocol:

1. **Surface scrape (0-2 cm)**: Discard—modern contamination
2. **Upper A-horizon (5-15 cm)**: Secondary sample
3. **Lower A-horizon (15-30 cm)**: Primary sample
4. **B-horizon (if accessible)**: Background reference

## Sample preparation and analysis

### Field collection

Collect 200-500 g of soil per sample using stainless steel or plastic tools (avoid galvanized metal, which contaminates Zn readings). Record:

- GPS coordinates (accuracy <3 m)
- Depth interval
- Soil color and texture
- Visible inclusions (charcoal, ceramics, bone)

### Laboratory preparation

Standard preparation involves:

1. Air-dry at <40°C
2. Disaggregate and sieve to <2 mm
3. Homogenize
4. Subsample for analysis (typically <180 μm for XRF)

The **mass attenuation** for XRF analysis depends on particle size:

$$
I = I_0 \cdot e^{-\mu \rho x}
$$

where $\mu$ is the mass attenuation coefficient, $\rho$ is density, and $x$ is path length. Consistent particle size ensures comparable measurements.

### Analytical methods

**Portable XRF (pXRF)**: Field-deployable, 30-120 s per reading, detection limits of 10-100 ppm for most elements. Ideal for reconnaissance and rapid screening.

**Laboratory XRF**: Better precision (±1-5%), lower detection limits, requires sample transport. Use for confirmation and detailed studies.

**ICP-AES/ICP-MS**: Highest sensitivity (sub-ppm detection), requires acid digestion. Reserve for trace elements and high-precision work.

The analytical precision should be verified using:

$$
RSD = \frac{\sigma}{\bar{x}} \times 100\%
$$

Aim for $RSD < 10\%$ for major elements, $RSD < 20\%$ for trace elements.

## Statistical analysis

### Background estimation

Establishing local background is critical. Use one of these approaches:

**Method 1: Median + MAD**

$$
C_{background} = \text{median}(C_i)
$$

$$
\text{MAD} = \text{median}(|C_i - \text{median}(C_i)|)
$$

Anomalies are values where $C > C_{background} + k \cdot \text{MAD}$, typically with $k = 3$.

**Method 2: Iterative 2σ clipping**

Remove values beyond 2σ from the mean, recalculate, and repeat until convergence:

$$
\bar{C}_{n+1} = \text{mean}(C_i : |C_i - \bar{C}_n| < 2\sigma_n)
$$

### Anomaly detection

The **anomaly index** for a sample combines multiple elements:

$$
AI = \sum_{i=1}^{k} w_i \cdot \frac{C_i - \bar{C}_i}{\sigma_i}
$$

where $w_i$ are weights reflecting each element's importance for the target activity type. High AI values indicate probable activity zones.

<div class="info-box tip">
  <strong>Key idea</strong>
  Multi-element anomaly indices outperform single-element thresholds because anthropogenic activities affect multiple elements simultaneously.
</div>

### Element ratios for activity classification

Use ratio spaces to classify activity types:

| Activity | P | Cu | Pb | Ca | Signature |
|----------|---|----|----|----|-----------| 
| Domestic occupation | High | Low | Low | Medium | P↑, Ca↑ |
| Bronze metallurgy | Low | Very High | Low | Low | Cu↑, Cu/Zn↑ |
| Lead-silver processing | Low | Medium | Very High | Low | Pb↑↑ |
| Ceramic production | Low | Low | Medium | Medium | Variable |
| Agricultural fields | Medium | Low | Low | Low | P moderate |

### Principal component analysis

For complex datasets, PCA reduces dimensionality while preserving variance:

$$
\mathbf{X}_{reduced} = \mathbf{X} \cdot \mathbf{W}
$$

where $\mathbf{W}$ contains the principal component loadings. Plot samples in PC1-PC2 space to identify clusters corresponding to different activity types.

## Central Asia applications

### Settlement detection

Yurt camps, caravanserais, and permanent settlements leave distinct P and Ca signatures. In the Kazakh steppe, settlement soils show:

- P: 2-5× background (800-2000 ppm vs. 400 ppm background)
- Ca: 1.5-3× background (variable with soil type)
- Cu, Pb: Near background unless craft activities present

The **settlement probability index** can be calculated as:

$$
SPI = 0.5 \cdot \frac{P - P_{bg}}{P_{bg}} + 0.3 \cdot \frac{Ca - Ca_{bg}}{Ca_{bg}} + 0.2 \cdot \frac{K - K_{bg}}{K_{bg}}
$$

Values of $SPI > 1$ suggest probable settlement; $SPI > 2$ indicates high confidence.

### Metallurgical site identification

Central Asia's rich metallurgical heritage—from Sintashta bronze to medieval Islamic brass—leaves distinctive soil signatures. Look for:

- Cu > 200 ppm (vs. 20-50 ppm background)
- Pb > 100 ppm near smelting areas
- As elevated near arsenical bronze production
- Slag fragments in sample residues

The **metallurgical intensity index**:

$$
MII = \log_{10}\left(\frac{Cu \cdot Pb}{Fe}\right) - \log_{10}\left(\frac{Cu_{bg} \cdot Pb_{bg}}{Fe_{bg}}\right)
$$

### Agricultural history

Irrigated fields and manured zones show moderate P enrichment and altered Ca/Mg ratios. The **cultivation index**:

$$
CI = \frac{P}{P_{bg}} \cdot \frac{(Ca/Mg)}{(Ca/Mg)_{bg}}
$$

Values of $CI > 1.5$ suggest historical cultivation; $CI > 2.5$ indicates intensive agriculture or manuring.

### Burial detection

Burial sites concentrate P (from bone decomposition) and sometimes Cu or Au (from grave goods):

$$
P_{burial} \approx P_{bg} + m_{bone} \cdot f_P \cdot (1 - e^{-\lambda t})
$$

where $m_{bone}$ is bone mass, $f_P$ is the P fraction in bone (~10%), and $\lambda$ is the decomposition rate.

## Integration with remote sensing and geophysics

### Combining with satellite imagery

Use NDVI anomalies to guide soil sampling—areas with anomalous vegetation often reflect subsurface chemistry differences:

$$
NDVI = \frac{NIR - Red}{NIR + Red}
$$

Sample both NDVI-high and NDVI-low zones within your study area to calibrate the vegetation-chemistry relationship.

### Integration with magnetometry

Magnetic surveys detect fired features and some metallurgical residues. Combine with geochemistry:

- High magnetism + High Cu/Pb = Probable smithing area
- High magnetism + High P = Hearth or cooking area
- Low magnetism + High P = Organic waste or latrine

### GPR integration

Ground-penetrating radar reveals buried structures. Target soil sampling at:

- Anomaly edges (activity boundaries)
- Strong reflector locations (floors, surfaces)
- Hyperbola clusters (pit features)

## A practical workflow

### Phase 1: Reconnaissance (1-3 days)

1. Define study area using satellite imagery and historical sources
2. Establish 50-100 m sample grid across area
3. Collect composite surface samples (15-25 cm depth)
4. Analyze with pXRF in field or camp laboratory
5. Map preliminary element distributions

### Phase 2: Targeted investigation (2-5 days)

1. Identify anomaly clusters from Phase 1
2. Reduce grid spacing to 10-25 m in anomalous zones
3. Collect individual samples at controlled depths
4. Include background samples from geologically similar non-anomalous areas
5. Analyze full element suite

### Phase 3: Interpretation and integration

1. Calculate enrichment factors and anomaly indices
2. Classify activity types using ratio analysis
3. Overlay with remote sensing and geophysical data
4. Generate probability maps for different activity types
5. Prioritize targets for excavation or further investigation

<div class="info-box tip">
  <strong>Key idea</strong>
  Geochemical prospection is most powerful when integrated with other methods. Use soil chemistry to guide where to deploy more expensive techniques like excavation or detailed geophysics.
</div>

## Field equipment checklist

<ul class="checklist">
  <li>GPS unit (sub-3 m accuracy, coordinate system configured)</li>
  <li>Stainless steel trowels and augers (avoid galvanized tools)</li>
  <li>Sample bags (kraft paper or cloth, pre-labeled)</li>
  <li>Permanent markers and field notebook</li>
  <li>Soil color chart (Munsell)</li>
  <li>Tape measure and depth gauge</li>
  <li>Portable XRF analyzer (if available)</li>
  <li>Calibration standards (certified reference materials)</li>
  <li>Protective gloves (nitrile, powder-free)</li>
  <li>Sieve set (2 mm mesh minimum)</li>
  <li>Drying racks or trays</li>
  <li>Camera for documentation</li>
  <li>Site maps and imagery printouts</li>
  <li>First aid kit and sun protection</li>
  <li>Water and field rations</li>
</ul>

## Exercises and solutions

<details>
<summary><strong>Exercise 1: Enrichment factor calculation</strong></summary>

A soil sample shows Cu = 180 ppm and Fe = 3.2%. Background values are Cu = 35 ppm and Fe = 4.0%. Calculate the enrichment factor for Cu.

**Solution:**

$$EF_{Cu} = \frac{Cu_{sample}/Fe_{sample}}{Cu_{bg}/Fe_{bg}} = \frac{180/32000}{35/40000} = \frac{0.005625}{0.000875} = 6.43$$

The Cu enrichment factor of 6.43 indicates strong anthropogenic enrichment, likely from metallurgical activity.
</details>

<details>
<summary><strong>Exercise 2: Grid spacing determination</strong></summary>

You want to detect settlement features at least 40 m in diameter. What maximum grid spacing should you use?

**Solution:**

Using the Nyquist criterion:

$$\Delta x \leq \frac{D_{feature}}{2} = \frac{40}{2} = 20 \text{ m}$$

Use a grid spacing of 20 m or less to reliably detect 40 m features.
</details>

<details>
<summary><strong>Exercise 3: Sample size calculation</strong></summary>

Preliminary sampling suggests σ = 120 ppm for P with mean 450 ppm. How many samples are needed to estimate the mean within ±30 ppm at 95% confidence?

**Solution:**

$$n \geq \frac{z^2 \cdot s^2}{\epsilon^2} = \frac{1.96^2 \cdot 120^2}{30^2} = \frac{3.84 \cdot 14400}{900} = 61.4$$

You need at least 62 samples to achieve the desired precision.
</details>

<details>
<summary><strong>Exercise 4: Composite sample interpretation</strong></summary>

A composite sample from 4 subsamples has P values: 520, 480, 890, 510 ppm. What is the composite value, and what does the high outlier suggest?

**Solution:**

$$C_{composite} = \frac{520 + 480 + 890 + 510}{4} = \frac{2400}{4} = 600 \text{ ppm}$$

The outlier (890 ppm) suggests localized P enrichment within the sampling area. Consider collecting individual samples to pinpoint the anomaly source.
</details>

<details>
<summary><strong>Exercise 5: Cu/Zn ratio interpretation</strong></summary>

Sample A: Cu = 450 ppm, Zn = 180 ppm. Sample B: Cu = 380 ppm, Zn = 290 ppm. Classify the likely metallurgical activity at each location.

**Solution:**

Sample A: $R_{CuZn} = 450/180 = 2.5$
Sample B: $R_{CuZn} = 380/290 = 1.31$

Both ratios are below 3, suggesting brass-related activity (Cu-Zn alloy). Sample B with the lower ratio indicates more Zn-rich processing, possibly brass production or zinc ore processing.
</details>

<details>
<summary><strong>Exercise 6: Background estimation using MAD</strong></summary>

P values (ppm): 380, 410, 395, 420, 405, 890, 415, 400, 385, 950. Calculate the background using median and MAD, then identify anomalies.

**Solution:**

Sorted: 380, 385, 395, 400, 405, 410, 415, 420, 890, 950

Median = (405 + 410)/2 = 407.5 ppm

Deviations: 27.5, 22.5, 12.5, 7.5, 2.5, 2.5, 7.5, 12.5, 482.5, 542.5

MAD = (7.5 + 7.5)/2 = 7.5 ppm

Anomaly threshold = 407.5 + 3 × 7.5 = 430 ppm

Anomalies: 890 ppm and 950 ppm are both above threshold.
</details>

<details>
<summary><strong>Exercise 7: Settlement probability index</strong></summary>

Sample values: P = 920 ppm, Ca = 1.8%, K = 2.1%. Background: P = 400 ppm, Ca = 1.2%, K = 1.8%. Calculate SPI.

**Solution:**

$$SPI = 0.5 \cdot \frac{920-400}{400} + 0.3 \cdot \frac{18000-12000}{12000} + 0.2 \cdot \frac{21000-18000}{18000}$$

$$SPI = 0.5 \cdot 1.3 + 0.3 \cdot 0.5 + 0.2 \cdot 0.167 = 0.65 + 0.15 + 0.033 = 0.833$$

SPI = 0.83 suggests possible settlement activity but below the high-confidence threshold of 1.0.
</details>

<details>
<summary><strong>Exercise 8: Analytical precision assessment</strong></summary>

Five replicate measurements of a standard give Cu values: 142, 138, 145, 141, 139 ppm. Calculate RSD and assess if precision is acceptable.

**Solution:**

Mean = (142 + 138 + 145 + 141 + 139)/5 = 141 ppm

Variance = [(1 + 9 + 16 + 0 + 4)]/4 = 7.5

σ = √7.5 = 2.74 ppm

$$RSD = \frac{2.74}{141} \times 100\% = 1.94\%$$

RSD of 1.94% is well below the 10% threshold—excellent precision.
</details>

<details>
<summary><strong>Exercise 9: Depth selection</strong></summary>

A soil profile shows: modern debris 0-8 cm, A-horizon 8-35 cm, B-horizon below 35 cm. What depth interval should you sample for archaeological prospection?

**Solution:**

Avoid modern contamination (0-8 cm) and deep B-horizon. Sample the lower A-horizon:

Primary sample: 20-30 cm (captures anthropogenic enrichment, avoids modern material)
Secondary sample: 10-20 cm (upper A-horizon comparison)

This depth protocol matches the standard 15-30 cm guidance for Central Asian steppe soils.
</details>

<details>
<summary><strong>Exercise 10: Multi-element anomaly index</strong></summary>

Calculate the anomaly index for a sample with: P = 750 ppm (bg: 420, σ: 85), Cu = 95 ppm (bg: 40, σ: 15), Pb = 55 ppm (bg: 25, σ: 8). Use equal weights.

**Solution:**

$$AI = \frac{1}{3}\left[\frac{750-420}{85} + \frac{95-40}{15} + \frac{55-25}{8}\right]$$

$$AI = \frac{1}{3}\left[3.88 + 3.67 + 3.75\right] = \frac{11.30}{3} = 3.77$$

An AI of 3.77 indicates a strong multi-element anomaly consistent with combined settlement and craft activity.
</details>

<details>
<summary><strong>Exercise 11: Ca/P ratio classification</strong></summary>

Two samples: A has Ca = 2.5%, P = 1200 ppm; B has Ca = 4.2%, P = 650 ppm. Classify the likely activity type for each.

**Solution:**

Sample A: $R_{CaP} = 25000/1200 = 20.8$
Sample B: $R_{CaP} = 42000/650 = 64.6$

Sample A (R = 20.8): Lower ratio suggests organic waste, bone processing, or latrine area (P-dominated).

Sample B (R = 64.6): Higher ratio suggests ash deposits, lime processing, or hearth area (Ca-dominated).
</details>

<details>
<summary><strong>Exercise 12: Metallurgical intensity index</strong></summary>

Sample: Cu = 320 ppm, Pb = 180 ppm, Fe = 3.8%. Background: Cu = 38 ppm, Pb = 22 ppm, Fe = 4.1%. Calculate MII.

**Solution:**

$$MII = \log_{10}\left(\frac{320 \times 180}{38000}\right) - \log_{10}\left(\frac{38 \times 22}{41000}\right)$$

$$MII = \log_{10}(1.516) - \log_{10}(0.0204) = 0.181 - (-1.690) = 1.87$$

MII of 1.87 indicates significant metallurgical activity—about 74× more intense than background.
</details>

<details>
<summary><strong>Exercise 13: Sampling density calculation</strong></summary>

A 15-hectare survey area needs detailed mapping. How many samples are required at 50 samples/hectare? What grid spacing does this imply?

**Solution:**

Total samples = 15 × 50 = 750 samples

For a regular grid: $n = \frac{A}{\Delta x^2}$

$$\Delta x = \sqrt{\frac{A}{n}} = \sqrt{\frac{150000 \text{ m}^2}{750}} = \sqrt{200} = 14.1 \text{ m}$$

Use a 14 m grid spacing, collecting 750 samples across the area.
</details>

<details>
<summary><strong>Exercise 14: Iterative background calculation</strong></summary>

Initial P values (ppm): 420, 385, 410, 395, 880, 405, 400, 440, 920, 415. Apply one iteration of 2σ clipping.

**Solution:**

Initial mean = 507 ppm, σ = 198 ppm

Threshold: 507 ± 2(198) = 111 to 903 ppm

Values outside range: 880 (keep, <903), 920 (remove, >903)

Remaining: 420, 385, 410, 395, 880, 405, 400, 440, 415

New mean = 461 ppm (would continue iterating until convergence)
</details>

<details>
<summary><strong>Exercise 15: Detection limit assessment</strong></summary>

Your pXRF has detection limits: P = 100 ppm, Cu = 15 ppm, Pb = 8 ppm. Background values are P = 400 ppm, Cu = 35 ppm, Pb = 22 ppm. Can you reliably detect 50% enrichment for each element?

**Solution:**

50% enrichment levels: P = 600 ppm, Cu = 52.5 ppm, Pb = 33 ppm

P: 600 ppm >> 100 ppm detection limit ✓
Cu: 52.5 ppm >> 15 ppm detection limit ✓
Pb: 33 ppm >> 8 ppm detection limit ✓

All elements can be reliably measured at 50% enrichment levels—well above detection limits.
</details>

<details>
<summary><strong>Exercise 16: Spatial interpolation</strong></summary>

Four samples at corners of a 20×20 m square show P values: NW=520, NE=480, SW=650, SE=580 ppm. Estimate P at the center using inverse distance weighting.

**Solution:**

All corners are equidistant (d = 14.14 m) from center. With equal distances, IDW reduces to simple average:

$$P_{center} = \frac{520 + 480 + 650 + 580}{4} = 557.5 \text{ ppm}$$

The center is estimated at 558 ppm, suggesting moderate enrichment across the area with higher values in the southern portion.
</details>

<details>
<summary><strong>Exercise 17: Cultivation index calculation</strong></summary>

Field sample: P = 680 ppm, Ca = 1.9%, Mg = 0.45%. Background: P = 380 ppm, Ca = 1.3%, Mg = 0.52%. Calculate CI.

**Solution:**

$$CI = \frac{P}{P_{bg}} \times \frac{(Ca/Mg)}{(Ca/Mg)_{bg}}$$

$$CI = \frac{680}{380} \times \frac{(1.9/0.45)}{(1.3/0.52)} = 1.79 \times \frac{4.22}{2.50} = 1.79 \times 1.69 = 3.02$$

CI of 3.02 exceeds 2.5, indicating intensive historical agriculture with probable manuring.
</details>

<details>
<summary><strong>Exercise 18: Error propagation in ratios</strong></summary>

Cu = 85 ± 8 ppm, Zn = 42 ± 5 ppm. Calculate the Cu/Zn ratio and its uncertainty.

**Solution:**

$$R = \frac{Cu}{Zn} = \frac{85}{42} = 2.02$$

For ratio uncertainty:

$$\frac{\sigma_R}{R} = \sqrt{\left(\frac{\sigma_{Cu}}{Cu}\right)^2 + \left(\frac{\sigma_{Zn}}{Zn}\right)^2}$$

$$\frac{\sigma_R}{2.02} = \sqrt{\left(\frac{8}{85}\right)^2 + \left(\frac{5}{42}\right)^2} = \sqrt{0.0089 + 0.0142} = 0.152$$

$$\sigma_R = 2.02 \times 0.152 = 0.31$$

Cu/Zn = 2.02 ± 0.31
</details>

<details>
<summary><strong>Exercise 19: Sample mass calculation</strong></summary>

You need 50 g of <180 μm material for XRF analysis. If the <180 μm fraction is typically 35% of bulk soil, how much bulk sample should you collect?

**Solution:**

$$m_{bulk} = \frac{m_{required}}{f_{fraction}} = \frac{50}{0.35} = 143 \text{ g}$$

Collect at least 143 g; recommend 200-250 g to account for losses during processing.
</details>

<details>
<summary><strong>Exercise 20: Multi-site comparison</strong></summary>

Site A (settlement): P = 850 ppm, Cu = 45 ppm, Pb = 28 ppm
Site B (metallurgical): P = 420 ppm, Cu = 380 ppm, Pb = 195 ppm
Background: P = 400 ppm, Cu = 38 ppm, Pb = 24 ppm

Calculate enrichment factors for each element at each site.

**Solution:**

Using Fe as implicit reference (assuming similar Fe at all sites):

Site A: EF(P) = 850/400 = 2.13, EF(Cu) = 45/38 = 1.18, EF(Pb) = 28/24 = 1.17

Site B: EF(P) = 420/400 = 1.05, EF(Cu) = 380/38 = 10.0, EF(Pb) = 195/24 = 8.13

Site A shows P enrichment (settlement signature). Site B shows Cu and Pb enrichment (metallurgical signature).
</details>

<details>
<summary><strong>Exercise 21: Temporal decay modeling</strong></summary>

A hearth was abandoned 800 years ago. Initial Ca enrichment was 15× background. If the decay half-life for Ca in this soil is 500 years, what enrichment factor remains today?

**Solution:**

Using exponential decay:

$$EF(t) = EF_0 \cdot e^{-\lambda t}$$

where $\lambda = \ln(2)/t_{1/2} = 0.693/500 = 0.00139$ yr⁻¹

$$EF(800) = 15 \cdot e^{-0.00139 \times 800} = 15 \cdot e^{-1.11} = 15 \cdot 0.33 = 4.95$$

About 5× enrichment remains—still detectable but reduced from original levels.
</details>

<details>
<summary><strong>Exercise 22: Transect analysis</strong></summary>

A 200 m transect with 10 m spacing shows P values (ppm): 380, 395, 420, 580, 720, 890, 850, 640, 450, 410, 385, 390, 375, 395, 400, 385, 410, 395, 380, 405, 390. Identify the anomaly zone boundaries.

**Solution:**

Background (excluding obvious anomalies): ~395 ppm
MAD ≈ 15 ppm
Threshold = 395 + 3(15) = 440 ppm

Anomalous values (>440 ppm): positions 4-9 (580, 720, 890, 850, 640, 450 ppm)

Anomaly zone: 30-90 m along transect (60 m wide feature, centered at ~55 m)
</details>

<details>
<summary><strong>Exercise 23: NDVI-geochemistry correlation</strong></summary>

Plot shows: NDVI = 0.45 where P = 380 ppm, NDVI = 0.62 where P = 720 ppm, NDVI = 0.38 where P = 340 ppm. Calculate the correlation coefficient.

**Solution:**

Data: (380, 0.45), (720, 0.62), (340, 0.38)

Means: P̄ = 480, NDVĪ = 0.483

Covariance = [(380-480)(0.45-0.483) + (720-480)(0.62-0.483) + (340-480)(0.38-0.483)]/2
= [(-100)(-0.033) + (240)(0.137) + (-140)(-0.103)]/2 = [3.3 + 32.88 + 14.42]/2 = 25.3

σ_P = 161.2, σ_NDVI = 0.099

r = 25.3/(161.2 × 0.099) = 1.59 → r ≈ 0.99

Strong positive correlation between P enrichment and NDVI.
</details>

<details>
<summary><strong>Exercise 24: Quality control assessment</strong></summary>

Standard reference material with certified Cu = 125 ppm gives readings: 118, 131, 122, 128, 126, 119, 133, 124, 120, 129 ppm. Assess accuracy and precision.

**Solution:**

Mean = 125 ppm
σ = 5.0 ppm
RSD = 5.0/125 × 100% = 4.0%

Accuracy: Mean equals certified value—excellent accuracy.
Precision: RSD = 4.0% < 10% threshold—good precision.

Both accuracy and precision are acceptable for archaeological prospection work.
</details>

<details>
<summary><strong>Exercise 25: Sampling strategy optimization</strong></summary>

Budget allows 200 samples. Area has: Zone A (5 ha, suspected settlement), Zone B (12 ha, background), Zone C (3 ha, suspected workshop). Allocate samples proportionally to both area and archaeological priority.

**Solution:**

Priority weights: Zone A = 3, Zone C = 3, Zone B = 1

Weighted area: A = 5×3 = 15, B = 12×1 = 12, C = 3×3 = 9
Total = 36

Sample allocation:
Zone A: 200 × (15/36) = 83 samples → 17 samples/ha
Zone B: 200 × (12/36) = 67 samples → 6 samples/ha
Zone C: 200 × (9/36) = 50 samples → 17 samples/ha

Higher density in priority zones while maintaining coverage of background area.
</details>

<details>
<summary><strong>Exercise 26: Contamination assessment</strong></summary>

Surface (0-5 cm): Pb = 185 ppm. Depth 15-25 cm: Pb = 42 ppm. Background Pb = 25 ppm. Is the surface contamination modern or archaeological?

**Solution:**

Surface enrichment factor: 185/25 = 7.4
Subsurface enrichment factor: 42/25 = 1.68

The sharp decrease with depth (185 → 42 ppm) and subsurface values near background suggest modern contamination at surface (possibly vehicle emissions, leaded gasoline legacy, or recent industrial activity).

Archaeological Pb enrichment would typically show a buried maximum, not a surface maximum. Collect samples from 15-25 cm for archaeological interpretation.
</details>

<details>
<summary><strong>Exercise 27: Element ratio stability</strong></summary>

Two samples from same feature measured 6 months apart:
Time 1: Cu = 245 ppm, Fe = 3.6%
Time 2: Cu = 228 ppm, Fe = 3.4%

Did absolute concentrations or Cu/Fe ratio change more?

**Solution:**

Absolute Cu change: (245-228)/245 = 6.9% decrease

Cu/Fe ratios:
Time 1: 245/36000 = 0.00681
Time 2: 228/34000 = 0.00671

Ratio change: (0.00681-0.00671)/0.00681 = 1.5% decrease

The Cu/Fe ratio changed only 1.5% compared to 6.9% for absolute Cu. Ratios are more stable across measurement conditions—supporting their use for site comparison.
</details>

<details>
<summary><strong>Exercise 28: Feature size estimation</strong></summary>

A circular P anomaly shows values >2× background within a radius of approximately 12 m from center. Estimate the original feature size if P has dispersed 3 m beyond original boundaries over 1500 years.

**Solution:**

Current anomaly radius: 12 m
Dispersal distance: 3 m

Original feature radius = 12 - 3 = 9 m
Original feature diameter = 18 m
Original feature area = π × 9² = 254 m²

This corresponds to a medium-sized structure or activity area (approximately 16 × 16 m equivalent square).
</details>

<details>
<summary><strong>Exercise 29: Detection probability</strong></summary>

Your survey grid is 30 m. A workshop feature is 25 m in diameter. What is the probability that at least one sample falls within the feature?

**Solution:**

For a regular grid with spacing Δx over a circular feature of diameter D:

The feature will always be detected if D ≥ Δx√2 = 30 × 1.414 = 42.4 m

For D = 25 m < 42.4 m, some grid positions may miss the feature.

Approximate detection probability:
$$P_{detect} \approx \frac{\pi (D/2)^2}{(\Delta x)^2} = \frac{\pi (12.5)^2}{30^2} = \frac{491}{900} = 0.55$$

Only ~55% detection probability. Reduce grid spacing to 20 m for reliable detection of 25 m features.
</details>

<details>
<summary><strong>Exercise 30: Integrated interpretation</strong></summary>

A site shows: High P anomaly (zone A), high Cu+Pb anomaly (zone B, 40 m east), high Ca anomaly (zone C, overlapping zones A and B). Magnetometry shows high values in zone B only. Interpret the site organization.

**Solution:**

Zone A (high P, low metals): Domestic occupation area—organic waste, food processing, possible animal keeping.

Zone B (high Cu+Pb, high magnetism): Metallurgical workshop—the magnetic signal confirms fired features (furnaces, hearths), while Cu+Pb indicate metal processing.

Zone C (high Ca, overlapping): Ash disposal and/or lime processing—Ca enrichment spanning both areas suggests communal waste disposal or construction activity.

Site interpretation: A settlement with separated domestic (A) and industrial (B) zones, with shared infrastructure or waste management (C). The 40 m separation between domestic and metallurgical areas is typical for health/safety or wind direction considerations.
</details>

## Summary

Geochemical soil sampling provides a cost-effective, non-destructive method for landscape-scale prospection in Central Asia. The key principles are:

1. **Element selection matters**: P for occupation, Cu/Pb for metallurgy, Ca for combustion features
2. **Background estimation is critical**: Use robust statistics (median, MAD) to define anomaly thresholds
3. **Ratios outperform absolute values**: Element ratios are more stable and diagnostic
4. **Integration multiplies value**: Combine with remote sensing and geophysics for confident interpretation
5. **Sampling design determines resolution**: Match grid spacing to target feature size

The mathematics of enrichment factors, anomaly indices, and spatial statistics transforms raw concentration data into actionable prospection intelligence.
