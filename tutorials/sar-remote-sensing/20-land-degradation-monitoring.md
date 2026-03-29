---
layout: tutorial
title: "How Remote Sensing Monitors Land Degradation in Central Asia"
description: "A practical tutorial on desertification, salinization, erosion, and vegetation degradation assessment using satellite data for Central Asia, with 30 exercises and solutions."
difficulty: intermediate
estimated_time: "3 hours"
series: sar-remote-sensing
order: 20
---

## Why this tutorial matters: The Aral Sea crisis and regional desertification

Central Asia is experiencing one of humanity's greatest environmental catastrophes. The **Aral Sea**, once the world's fourth-largest inland water body, has lost over **90% of its volume** since 1960. What remains is a toxic landscape of salt flats, dust storms, and abandoned fishing communities.

The consequences extend far beyond the Aral basin:

- **Toxic dust storms** carry 75 million tons of salt and pesticide residue annually across the region
- **15 million hectares** of agricultural land show signs of degradation
- **Desertification** threatens 66% of Kazakhstan's territory
- **Salinization** affects 50% of irrigated lands in Uzbekistan and Turkmenistan

Remote sensing provides the only practical means to monitor land degradation across this vast, transboundary region where ground-based monitoring networks are sparse.

<div class="info-box tip">
  <strong>Key idea</strong>
  Land degradation is a reduction in land productivity caused by human activities or climate change. Remote sensing detects degradation through changes in vegetation cover, soil properties, surface albedo, and land use patterns over time.
</div>

## Desertification indicators from remote sensing

Desertification is the degradation of drylands resulting from climate variability and human activities. Remote sensing captures three primary indicators.

### Vegetation loss quantification

The most direct indicator is declining vegetation. The Normalized Difference Vegetation Index (NDVI) provides a robust measure:

$$
\text{NDVI} = \frac{\rho_{\text{NIR}} - \rho_{\text{Red}}}{\rho_{\text{NIR}} + \rho_{\text{Red}}}
$$

For desertification monitoring, we calculate the **Vegetation Condition Index (VCI)**:

$$
\text{VCI} = \frac{\text{NDVI} - \text{NDVI}_{\min}}{\text{NDVI}_{\max} - \text{NDVI}_{\min}} \times 100
$$

Where NDVI_min and NDVI_max represent the historical range for each pixel. VCI values below 35% indicate vegetation stress.

The **Rain Use Efficiency (RUE)** isolates human-induced degradation from climate variability:

$$
\text{RUE} = \frac{\text{NPP}}{\text{Precipitation}}
$$

Declining RUE over time indicates land degradation rather than drought.

### Albedo change detection

Surface albedo—the fraction of solar radiation reflected—increases with desertification as dark vegetation is replaced by bright bare soil:

$$
\alpha = \frac{R_{\uparrow}}{R_{\downarrow}}
$$

For Landsat:

$$
\alpha_{\text{surface}} = 0.356 \cdot \rho_1 + 0.130 \cdot \rho_3 + 0.373 \cdot \rho_4 + 0.085 \cdot \rho_5 + 0.072 \cdot \rho_7 - 0.0018
$$

<div class="info-box tip">
  <strong>Key idea</strong>
  Desertification creates a positive feedback loop: vegetation loss increases albedo, which reduces surface temperature, suppresses convective rainfall, and further reduces vegetation—accelerating degradation.
</div>

### Sand encroachment mapping

Active sand dunes and encroaching sand sheets can be detected using spectral indices. The **Normalized Multi-band Drought Index (NMDI)** distinguishes bare sand from other surfaces:

$$
\text{NMDI} = \frac{\rho_{\text{NIR}} - (\rho_{\text{SWIR1}} - \rho_{\text{SWIR2}})}{\rho_{\text{NIR}} + (\rho_{\text{SWIR1}} - \rho_{\text{SWIR2}})}
$$

Sand dune migration rates can be quantified using multi-temporal imagery:

$$
v = \frac{\Delta x}{\Delta t}
$$

In the Karakum Desert, dune migration rates reach 5-15 meters per year, threatening agricultural oases and infrastructure.

## Salinization mapping

Soil salinization is Central Asia's most pressing agricultural threat.

### Spectral indices for salinity

**Salinity Index (SI)**:
$$
\text{SI} = \sqrt{\rho_{\text{Blue}} \times \rho_{\text{Red}}}
$$

**Normalized Difference Salinity Index (NDSI)**:
$$
\text{NDSI} = \frac{\rho_{\text{Red}} - \rho_{\text{NIR}}}{\rho_{\text{Red}} + \rho_{\text{NIR}}}
$$

### SAR sensitivity to soil salinity

SAR backscatter responds to soil salinity through dielectric properties. Saline soils have higher dielectric constants:

$$
\sigma^0 = A + B \cdot \ln(\text{EC})
$$

Where EC is electrical conductivity (dS/m). Studies show correlation coefficients of 0.7-0.85 between Sentinel-1 backscatter and field EC measurements.

<div class="info-box tip">
  <strong>Key idea</strong>
  SAR is valuable for salinity mapping because saline areas are often waterlogged during irrigation seasons, making optical imagery ineffective due to standing water or cloud cover.
</div>

## Soil erosion assessment using RUSLE factors

The Revised Universal Soil Loss Equation (RUSLE) estimates annual soil loss:

$$
A = R \times K \times LS \times C \times P
$$

Where:
- A = annual soil loss (t/ha/yr)
- R = rainfall erosivity factor
- K = soil erodibility factor
- LS = slope length and steepness factor
- C = cover management factor
- P = support practice factor

### Rainfall erosivity (R-factor)

The R-factor is derived from precipitation data:

$$
R = \sum_{i=1}^{n} (EI_{30})_i
$$

Where EI_30 is the product of storm energy and 30-minute maximum intensity. Satellite precipitation products (TRMM, GPM, CHIRPS) provide regional coverage:

$$
R = 0.0483 \times P^{1.61}
$$

### Slope length and steepness (LS-factor)

Digital Elevation Models from SRTM or ASTER provide topographic inputs:

$$
LS = \left(\frac{\lambda}{22.13}\right)^m \times (65.41 \sin^2 \theta + 4.56 \sin \theta + 0.065)
$$

Where λ is slope length, θ is slope angle, and m depends on slope steepness.

### Cover management (C-factor)

The C-factor is derived from NDVI:

$$
C = \exp\left(-\alpha \times \frac{\text{NDVI}}{\beta - \text{NDVI}}\right)
$$

Typical values: α = 2, β = 1. Bare soil has C ≈ 1; dense vegetation has C ≈ 0.001.

<div class="info-box tip">
  <strong>Key idea</strong>
  The C-factor is the most dynamic RUSLE parameter and the primary target for degradation monitoring. Remote sensing captures seasonal and inter-annual vegetation changes that directly influence erosion vulnerability.
</div>

## Vegetation degradation trends

### NDVI time-series analysis

Long-term vegetation trends reveal degradation patterns. For multi-decadal archives:

$$
\text{NDVI}(t) = \beta_0 + \beta_1 \cdot t + \epsilon
$$

A negative slope (β₁ < 0) with p < 0.05 indicates significant vegetation decline. The **Mann-Kendall test** provides robust trend detection for non-parametric data.

### RESTREND method

RESTREND removes rainfall effects to isolate human-induced degradation:

1. Establish NDVI-rainfall relationship for each pixel
2. Calculate expected NDVI based on rainfall
3. Compute residuals: $R = \text{NDVI}_{\text{observed}} - \text{NDVI}_{\text{predicted}}$
4. Analyze residual trends over time

Declining residuals indicate degradation independent of rainfall variability.

### Phenology shifts

Vegetation phenology shifts with climate and degradation:

$$
\text{SOS} = \text{day when NDVI reaches 20% of amplitude}
$$

Earlier senescence and shortened growing seasons indicate degradation stress.

## Pasture degradation in rangelands

Central Asia's rangelands support traditional pastoral systems. Overgrazing and climate change have accelerated degradation.

### Grazing intensity indicators

Biomass estimation from NDVI:

$$
\text{Biomass} = a \times \text{NDVI}^b
$$

Typical coefficients for Central Asian rangelands: a = 2500, b = 1.5 (biomass in kg/ha).

<div class="info-box tip">
  <strong>Key idea</strong>
  Pasture degradation often begins with palatable species replacement—vegetation cover may appear stable while productivity declines. Multi-temporal productivity analysis is essential.
</div>

### Carrying capacity estimation

Remote sensing enables carrying capacity calculation:

$$
\text{CC} = \frac{\text{Available biomass} \times \text{Proper use factor}}{\text{Daily consumption} \times \text{Grazing days}}
$$

## Irrigated land abandonment detection

Soviet-era irrigation schemes expanded agriculture across 8 million hectares in Central Asia. Post-Soviet economic collapse and water scarcity have led to widespread abandonment.

### Abandonment indicators

Abandoned irrigated land shows distinct signatures:

1. **Vegetation decline**: NDVI drops from seasonal agricultural patterns to low constant values
2. **Increased salinity**: Salt accumulation without leaching
3. **Surface roughness**: SAR coherence changes as fields are no longer tilled

| Stage | Years | NDVI Pattern | SAR Pattern |
|-------|-------|--------------|-------------|
| Active farming | - | Seasonal cycles, peak 0.6-0.8 | Variable |
| Initial decline | 0-2 | Reduced amplitude, peak 0.4-0.5 | Increasing coherence |
| Abandonment | 2-5 | Low constant, 0.1-0.2 | High coherence |
| Secondary succession | 5+ | Recovery to 0.3-0.4 | Variable |

## Multi-decadal change analysis

### Landsat archive exploitation

The Landsat archive (1972-present) provides unprecedented temporal depth. For Central Asia, key epochs include:

- **1972-1991**: Soviet irrigation expansion, Aral Sea decline
- **1991-2000**: Post-Soviet abandonment, infrastructure decay
- **2000-2015**: Recovery in some areas, continued decline in others
- **2015-present**: Climate change impacts intensifying

### BFAST for breakpoint detection

**BFAST (Breaks For Additive Seasonal and Trend)** detects abrupt changes in time-series, separating seasonal patterns from trend breaks.

<div class="info-box tip">
  <strong>Key idea</strong>
  Multi-decadal analysis reveals that some "degraded" areas have stabilized or begun recovery, while apparently stable areas may be on degradation trajectories. Current condition alone is insufficient for assessment.
</div>

## Policy-relevant indicators

### UNCCD framework

The United Nations Convention to Combat Desertification (UNCCD) defines three indicators for land degradation neutrality:

1. **Land cover change**: Transitions between land cover classes
2. **Land productivity dynamics**: Vegetation productivity trends
3. **Soil organic carbon**: Carbon stock changes

The integration follows:

$$
\text{LDN} = \text{Gains} - \text{Losses}
$$

Land Degradation Neutrality is achieved when gains equal or exceed losses.

### SDG 15.3.1: Proportion of degraded land

Sustainable Development Goal indicator 15.3.1 requires monitoring the proportion of degraded land:

$$
\text{SDG 15.3.1} = \frac{\text{Area}_{\text{degraded}}}{\text{Total area}} \times 100\%
$$

Land is classified as degraded if any of the three sub-indicators shows negative trends.

### Land Productivity Dynamics (LPD)

The UNCCD LPD classification uses five classes:

| Class | Description | Trend |
|-------|-------------|-------|
| 1 | Declining | Significant negative trend |
| 2 | Early signs of decline | Non-significant negative |
| 3 | Stable but stressed | Below historical mean |
| 4 | Stable | Within normal variability |
| 5 | Increasing | Significant positive trend |

## Central Asia hotspots and case studies

### The Aral Sea Basin

The most severe degradation hotspot:
- Lake area: 68,000 km² (1960) → 6,800 km² (2020)
- Exposed lakebed: 54,000 km² of salt flats
- Dust storm frequency: Increased 300% since 1960

### Fergana Valley salinization

The densely populated Fergana Valley shows extensive salinization:
- 60% of irrigated land affected
- EC values reaching 8-16 dS/m in severe areas

### Kazakh Steppe degradation

Northern Kazakhstan's wheat belt shows:
- 30% decline in soil organic carbon since 1960
- Wind erosion: 20-40 t/ha/year in vulnerable areas

<div class="info-box tip">
  <strong>Key idea</strong>
  Each Central Asian country faces distinct degradation challenges: Kazakhstan—wind erosion; Uzbekistan and Turkmenistan—salinization; Kyrgyzstan and Tajikistan—mountain pasture degradation. Monitoring approaches must be tailored accordingly.
</div>

## Land degradation monitoring checklist

<ul class="checklist">
  <li>Define study area boundaries and coordinate reference system</li>
  <li>Compile multi-decadal satellite archive (Landsat, MODIS, Sentinel)</li>
  <li>Apply atmospheric correction and cloud masking</li>
  <li>Calculate vegetation indices (NDVI, EVI, SAVI)</li>
  <li>Derive land cover classification for baseline and current periods</li>
  <li>Compute land cover transition matrix</li>
  <li>Generate NDVI time-series for each pixel</li>
  <li>Apply trend analysis (linear regression, Mann-Kendall)</li>
  <li>Calculate RESTREND to separate climate and human effects</li>
  <li>Extract phenology metrics (SOS, EOS, LOS)</li>
  <li>Map salinity indices for agricultural areas</li>
  <li>Calculate RUSLE factors from remote sensing</li>
  <li>Estimate annual soil loss rates</li>
  <li>Process SAR data for soil moisture and salinity</li>
  <li>Generate DEM-derived terrain parameters</li>
  <li>Identify abandoned irrigation areas</li>
  <li>Detect sand encroachment and dune migration</li>
  <li>Calculate albedo change maps</li>
  <li>Classify Land Productivity Dynamics categories</li>
  <li>Compute SDG 15.3.1 sub-indicators</li>
  <li>Integrate indicators for final degradation assessment</li>
  <li>Validate with field data and high-resolution imagery</li>
  <li>Generate uncertainty estimates</li>
  <li>Create policy-relevant maps and statistics</li>
  <li>Document methodology for reproducibility</li>
</ul>

## Exercises

### Exercise 1: NDVI trend calculation
Calculate the annual NDVI trend for a pixel with the following 10-year record: NDVI values = [0.45, 0.43, 0.42, 0.40, 0.38, 0.37, 0.35, 0.33, 0.32, 0.30].

<details>
<summary>Show Solution</summary>

Using linear regression:

Years (x): [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
NDVI (y): [0.45, 0.43, 0.42, 0.40, 0.38, 0.37, 0.35, 0.33, 0.32, 0.30]

Mean x = 4.5, Mean y = 0.375

Slope = Σ(x - x̄)(y - ȳ) / Σ(x - x̄)²
     = (-0.1375) / 82.5
     = -0.0167 NDVI units/year

The pixel shows significant vegetation decline at 1.67% NDVI per year. Over 10 years, this represents a 37% decline in vegetation vigor.
</details>

### Exercise 2: Vegetation Condition Index
Calculate VCI for a current NDVI of 0.32, where historical NDVI_min = 0.15 and NDVI_max = 0.55.

<details>
<summary>Show Solution</summary>

VCI = (NDVI - NDVI_min) / (NDVI_max - NDVI_min) × 100
VCI = (0.32 - 0.15) / (0.55 - 0.15) × 100
VCI = 0.17 / 0.40 × 100
VCI = 42.5%

This indicates moderate vegetation stress (between 35-50%). Values below 35% indicate severe stress; above 50% indicates normal conditions.
</details>

### Exercise 3: Salinity Index calculation
Calculate the Salinity Index for a pixel with Blue reflectance = 0.15 and Red reflectance = 0.25.

<details>
<summary>Show Solution</summary>

SI = √(ρ_Blue × ρ_Red)
SI = √(0.15 × 0.25)
SI = √0.0375
SI = 0.194

This relatively high SI value suggests possible salt accumulation. Non-saline vegetated surfaces typically have SI < 0.10.
</details>

### Exercise 4: Rain Use Efficiency
Calculate RUE for an area with NPP = 250 g C/m²/year and precipitation = 320 mm/year. Interpret the result.

<details>
<summary>Show Solution</summary>

RUE = NPP / Precipitation
RUE = 250 / 320
RUE = 0.78 g C/m²/mm

For semi-arid rangelands, RUE typically ranges from 0.4-1.2 g C/m²/mm. This value is within normal range. If RUE declines over time while precipitation remains stable, it indicates land degradation.
</details>

### Exercise 5: RUSLE soil loss estimation
Calculate annual soil loss given: R = 800, K = 0.035, LS = 3.5, C = 0.25, P = 0.8.

<details>
<summary>Show Solution</summary>

A = R × K × LS × C × P
A = 800 × 0.035 × 3.5 × 0.25 × 0.8
A = 19.6 t/ha/year

This exceeds typical soil formation rates (0.5-2 t/ha/year), indicating unsustainable erosion. Intervention is needed to reduce the C-factor through improved vegetation cover.
</details>

### Exercise 6: C-factor from NDVI
Calculate the C-factor for NDVI = 0.35 using α = 2 and β = 1.

<details>
<summary>Show Solution</summary>

C = exp(-α × NDVI / (β - NDVI))
C = exp(-2 × 0.35 / (1 - 0.35))
C = exp(-0.70 / 0.65)
C = exp(-1.077)
C = 0.34

This moderate C-factor indicates partial vegetation cover providing some erosion protection. Dense vegetation (NDVI > 0.7) would yield C < 0.05.
</details>

### Exercise 7: Albedo change interpretation
An area showed albedo increase from 0.18 to 0.28 over 20 years. Calculate the energy balance impact.

<details>
<summary>Show Solution</summary>

Δα = 0.28 - 0.18 = 0.10

With solar radiation ≈ 250 W/m², the absorbed radiation change:
ΔR_absorbed = -250 × 0.10 = -25 W/m²

This reduction in absorbed energy cools the surface by approximately 2-3°C, reducing convective activity and potentially suppressing local precipitation—a desertification feedback mechanism.
</details>

### Exercise 8: Sand dune migration rate
A dune crest moved 45 meters between images taken 3 years apart. Calculate annual migration rate.

<details>
<summary>Show Solution</summary>

v = Δx / Δt
v = 45 m / 3 years
v = 15 m/year

This is a fast-moving dune requiring immediate attention. At this rate, infrastructure or agricultural land 100m away would be impacted within 7 years.
</details>

### Exercise 9: Mann-Kendall test
For data series [2.1, 2.3, 2.0, 2.4, 2.6, 2.5], calculate the S statistic.

<details>
<summary>Show Solution</summary>

Compare each pair (j > i):
Sign(2.3-2.1)=+1, Sign(2.0-2.1)=-1, Sign(2.4-2.1)=+1, Sign(2.6-2.1)=+1, Sign(2.5-2.1)=+1
Sign(2.0-2.3)=-1, Sign(2.4-2.3)=+1, Sign(2.6-2.3)=+1, Sign(2.5-2.3)=+1
Sign(2.4-2.0)=+1, Sign(2.6-2.0)=+1, Sign(2.5-2.0)=+1
Sign(2.6-2.4)=+1, Sign(2.5-2.4)=+1
Sign(2.5-2.6)=-1

S = 5 + (-1+1+1+1) + (1+1+1) + (1+1) + (-1) = 5 + 2 + 3 + 2 - 1 = 11

Positive S indicates an increasing trend.
</details>

### Exercise 10: Phenology shift detection
Calculate length of season change if SOS shifted from day 95 to day 85 and EOS shifted from day 275 to day 260.

<details>
<summary>Show Solution</summary>

Original LOS = 275 - 95 = 180 days
New LOS = 260 - 85 = 175 days
ΔLOS = 175 - 180 = -5 days

The growing season shortened by 5 days. Earlier green-up (10 days) suggests warming, but earlier senescence (15 days) indicates increased water or heat stress ending the season prematurely—a degradation signal.
</details>

### Exercise 11: Biomass estimation
Estimate rangeland biomass for NDVI = 0.42 using Biomass = 2500 × NDVI^1.5.

<details>
<summary>Show Solution</summary>

Biomass = 2500 × (0.42)^1.5
Biomass = 2500 × 0.272
Biomass = 680 kg/ha

For Central Asian rangelands, this represents moderate productivity. Carrying capacity would be approximately 0.3-0.4 sheep/ha assuming 50% utilization rate.
</details>

### Exercise 12: Dielectric constant effect
SAR backscatter increased from -12 dB to -8 dB. Estimate the change in soil EC if the relationship is σ⁰ = -15 + 2.5×ln(EC).

<details>
<summary>Show Solution</summary>

Initial: -12 = -15 + 2.5×ln(EC₁) → ln(EC₁) = 1.2 → EC₁ = 3.32 dS/m
Final: -8 = -15 + 2.5×ln(EC₂) → ln(EC₂) = 2.8 → EC₂ = 16.44 dS/m

The EC increased approximately 5-fold, indicating severe salinization. Soils above 8 dS/m are unsuitable for most crops.
</details>

### Exercise 13: Land cover transition
A 1000 km² area showed: Forest→Cropland: 50 km², Cropland→Bare: 80 km², Grassland→Cropland: 30 km². Calculate net degradation.

<details>
<summary>Show Solution</summary>

Using UNCCD degradation matrix:
- Forest→Cropland: Degradation (50 km²)
- Cropland→Bare: Degradation (80 km²)
- Grassland→Cropland: Stable (0 km² counted as degradation)

Net degradation = 50 + 80 = 130 km²
Degradation proportion = 130/1000 = 13%

This exceeds typical thresholds for significant concern (>5%).
</details>

### Exercise 14: RESTREND residual
Observed NDVI = 0.38, rainfall = 280 mm. If the pixel's NDVI-rainfall regression is NDVI = 0.12 + 0.001×P, calculate the residual.

<details>
<summary>Show Solution</summary>

NDVI_predicted = 0.12 + 0.001 × 280 = 0.40
Residual = NDVI_observed - NDVI_predicted
Residual = 0.38 - 0.40 = -0.02

The negative residual indicates the vegetation is underperforming relative to rainfall expectations, suggesting human-induced degradation or other non-climatic stress factors.
</details>

### Exercise 15: LS-factor calculation
Calculate LS for slope length = 100 m, slope angle = 15°, with m = 0.5.

<details>
<summary>Show Solution</summary>

LS = (λ/22.13)^m × (65.41×sin²θ + 4.56×sinθ + 0.065)
LS = (100/22.13)^0.5 × (65.41×sin²(15°) + 4.56×sin(15°) + 0.065)
LS = 2.126 × (65.41×0.067 + 4.56×0.259 + 0.065)
LS = 2.126 × (4.38 + 1.18 + 0.065)
LS = 2.126 × 5.63
LS = 11.97

This high LS-factor indicates steep slopes requiring soil conservation measures.
</details>

### Exercise 16: Abandonment probability
Calculate abandonment probability with logistic model coefficients: β₀ = -2.5, β₁(NDVI_trend) = -15, β₂(distance_km) = 0.3. For NDVI_trend = -0.015/yr and distance = 8 km.

<details>
<summary>Show Solution</summary>

z = β₀ + β₁×X₁ + β₂×X₂
z = -2.5 + (-15)×(-0.015) + 0.3×8
z = -2.5 + 0.225 + 2.4
z = 0.125

P = 1 / (1 + e^(-z))
P = 1 / (1 + e^(-0.125))
P = 1 / (1 + 0.882)
P = 0.53 or 53%

This field has a moderate-high probability of abandonment.
</details>

### Exercise 17: Aral Sea area change
The Aral Sea area was 68,000 km² in 1960 and 6,800 km² in 2020. Calculate average annual loss rate and percentage remaining.

<details>
<summary>Show Solution</summary>

Total loss = 68,000 - 6,800 = 61,200 km²
Time period = 60 years
Annual loss rate = 61,200 / 60 = 1,020 km²/year

Percentage remaining = (6,800 / 68,000) × 100 = 10%

The Aral Sea lost 90% of its area at an average rate exceeding 1,000 km²/year—one of the fastest large-scale environmental changes in recorded history.
</details>

### Exercise 18: SDG 15.3.1 calculation
A country has: LPD degraded = 45,000 km², LC degraded = 38,000 km², SOC degraded = 52,000 km². Total area = 450,000 km². Calculate SDG 15.3.1 using one-out-all-out principle.

<details>
<summary>Show Solution</summary>

Union of degraded areas (one-out-all-out):
Maximum extent ≈ 52,000 km² (if all areas overlap)
Minimum extent could be higher if areas don't overlap

Assuming 60% overlap, estimated degraded area ≈ 65,000 km²

SDG 15.3.1 = (65,000 / 450,000) × 100 = 14.4%

This indicates significant land degradation requiring national action.
</details>

### Exercise 19: Carrying capacity
Calculate carrying capacity for rangeland with biomass = 800 kg/ha, proper use = 50%, daily consumption = 2 kg/animal, grazing period = 180 days.

<details>
<summary>Show Solution</summary>

CC = (Biomass × Proper use) / (Daily consumption × Grazing days)
CC = (800 × 0.50) / (2 × 180)
CC = 400 / 360
CC = 1.11 sheep/ha

If current stocking rate exceeds 1.11 sheep/ha, the pasture is overgrazed and degradation is likely.
</details>

### Exercise 20: Spectral mixture analysis
A pixel has reflectance = 0.25. Endmember reflectances: green vegetation = 0.45, dry grass = 0.35, bare soil = 0.15. Estimate fractions if only two endmembers are present (dry grass and bare soil).

<details>
<summary>Show Solution</summary>

0.25 = f_dry × 0.35 + f_bare × 0.15
f_dry + f_bare = 1

Substituting: 0.25 = f_dry × 0.35 + (1 - f_dry) × 0.15
0.25 = 0.35f_dry + 0.15 - 0.15f_dry
0.10 = 0.20f_dry
f_dry = 0.50

f_bare = 0.50

The pixel is 50% dry grass and 50% bare soil, indicating degraded pasture conditions.
</details>

### Exercise 21: Coherence-based change detection
InSAR coherence dropped from 0.8 to 0.3 over irrigated land. Interpret this change.

<details>
<summary>Show Solution</summary>

High coherence (0.8) indicates stable surface—actively farmed land maintains similar roughness. Low coherence (0.3) indicates temporal decorrelation from vegetation growth, soil moisture changes, or flooding. Context from optical imagery distinguishes active farming from abandonment.
</details>

### Exercise 22: BFAST breakpoint detection
A time series shows NDVI stable at 0.45 for years 1-5, then declining to 0.25 by year 10. Assess breakpoint significance.

<details>
<summary>Show Solution</summary>

Pre-break mean: 0.45, Post-break trend: -0.04/year
Change magnitude = 0.20, typical NDVI SD ≈ 0.03
Signal-to-noise ratio = 6.67 (>3)

The breakpoint at year 5 can be detected at p < 0.01 (99% confidence).
</details>

### Exercise 23: Dust source identification
Calculate dust emission potential (DEP) for exposed Aral lakebed with NDVI = 0.02 and soil moisture = 3%.

<details>
<summary>Show Solution</summary>

DEP = 1 if (NDVI < 0.15) AND (SM < 5%)

NDVI = 0.02 < 0.15 ✓
SM = 3% < 5% ✓

DEP = 1 (high emission potential)

The exposed Aral lakebed (54,000 km²) is the region's primary dust source, generating 75 million tons of salt-laden dust annually.
</details>

### Exercise 24: Irrigation efficiency
Calculate efficiency if ET (from remote sensing) = 650 mm/season and water applied = 1200 mm.

<details>
<summary>Show Solution</summary>

Efficiency = ET / Water applied × 100 = 650/1200 × 100 = 54%

This is typical for Central Asian flood irrigation. Improving to 75% efficiency would save 250 mm/season per hectare.
</details>

### Exercise 25: Carbon stock change
SOC declined from 45 to 38 t/ha over 20 years. Calculate annual CO2 emissions.

<details>
<summary>Show Solution</summary>

Annual C loss = (45-38)/20 = 0.35 t C/ha/year
CO2 equivalent = 0.35 × 3.67 = 1.28 t CO2/ha/year

For 1 million affected hectares: 1.28 million t CO2/year emissions.
</details>

### Exercise 26: Sensor harmonization
Convert Landsat 8 NIR (0.35) to Sentinel-2 using: ρ_S2 = 0.98 × ρ_L8 + 0.0055.

<details>
<summary>Show Solution</summary>

ρ_S2 = 0.98 × 0.35 + 0.0055 = 0.349

The 0.3% difference is within calibration uncertainty, enabling consistent cross-sensor analysis.
</details>

### Exercise 27: LPD classification
Classify a pixel with NDVI trend = -0.008/year and p = 0.03.

<details>
<summary>Show Solution</summary>

Negative trend with p < 0.05 (significant) = **LPD Class 1 (Declining)**

This indicates confirmed vegetation degradation requiring investigation and intervention.
</details>

### Exercise 28: Salt accumulation
Calculate annual salt input if ET = 1000 mm, groundwater EC = 3 dS/m, leaching fraction = 0.15.

<details>
<summary>Show Solution</summary>

Salt = ET × EC × (1-LF) × 0.64
Salt = 1000 × 3 × 0.85 × 0.64 = 1,632 kg/ha/year

Without adequate leaching, this reaches crop-damaging levels within 3-5 years.
</details>

### Exercise 29: Multi-criteria index
Calculate DI = 0.3×NDVI + 0.3×LST + 0.2×Albedo + 0.2×SM for scores: NDVI=0.7, LST=0.6, Albedo=0.8, SM=0.5.

<details>
<summary>Show Solution</summary>

DI = 0.3×0.7 + 0.3×0.6 + 0.2×0.8 + 0.2×0.5 = 0.65

Classification: **High degradation** (0.6-0.8 range), with vegetation loss and albedo increase as primary drivers.
</details>

### Exercise 30: LDN scenario
Current degradation = 1.5%/year, restoration = 0.8%/year. What's needed for Land Degradation Neutrality?

<details>
<summary>Show Solution</summary>

Net degradation = 1.5% - 0.8% = 0.7%/year (degradation exceeds restoration)

For LDN: Must either increase restoration to 1.5%/year OR reduce degradation to 0.8%/year.

Policy recommendation: Combine degradation prevention with active restoration to achieve neutrality.
</details>

## Summary

Land degradation monitoring with remote sensing integrates multiple data sources and indicators to assess complex environmental changes across Central Asia. The Aral Sea crisis demonstrates both the severity of degradation and the power of satellite observations to document change.

Key takeaways:

1. **Multi-indicator approach**: No single index captures all degradation processes. Combine vegetation, soil, and surface property indicators.

2. **Temporal depth matters**: Multi-decadal archives reveal trends invisible in snapshot assessments. Landsat's 50-year record is invaluable.

3. **Separate climate from humans**: RESTREND and similar methods distinguish human-induced degradation from climate variability.

4. **Policy relevance**: SDG 15.3.1 and UNCCD indicators translate scientific measurements into actionable metrics.

5. **Regional specificity**: Each Central Asian country faces distinct degradation challenges requiring tailored monitoring approaches.

Remote sensing cannot replace field validation, but it provides the spatial coverage and temporal frequency essential for regional degradation assessment and policy support.
