---
layout: tutorial
title: "How Remote Sensing Monitors Water Resources in Central Asia"
description: "A practical tutorial on mapping surface water, wetlands, snow cover, soil moisture, and irrigation using satellite data for Central Asia, with 30 exercises and solutions."
difficulty: intermediate
estimated_time: "2-3 hours"
series: sar-remote-sensing
order: 13
---

## Why this tutorial matters

Central Asia faces one of the world's most severe water crises. The region depends almost entirely on two river systems: the **Amu Darya** and **Syr Darya**, fed by glacial and snow melt from the Tien Shan and Pamir mountains.

The statistics are stark:

- The **Aral Sea** lost over 90% of its volume between 1960 and 2010
- Agriculture consumes 85-90% of available freshwater
- Glaciers have lost 20-30% of their mass since 1960

Remote sensing provides the only practical means to monitor water resources across this vast, transboundary region.

<div class="info-box tip">
  <strong>Key idea</strong>
  Water exists in multiple forms: surface water, groundwater, snow, glaciers, soil moisture, and vapor. Remote sensing can detect most of these, each requiring different sensors and techniques.
</div>

<div class="learning-objectives">
  <div class="learning-objectives-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#1e4f8a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <h3>What you will learn</h3>
  </div>
  <ul>
    <li>Map surface water extent using NDWI, MNDWI, and Automated Water Extraction Index (AWEI)</li>
    <li>Detect water bodies from SAR imagery using backscatter thresholding and Otsu’s method</li>
    <li>Monitor snow cover and glacier change with NDSI and optical time-series analysis</li>
    <li>Estimate soil moisture from Sentinel-1 SAR backscatter and interpret SMAP/SMOS products</li>
    <li>Track the Aral Sea recession and irrigation-driven water use across Central Asia’s river basins</li>
  </ul>
</div>

<div class="prerequisites">
  <div class="prerequisites-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#8b5e00" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
    <h3>Prerequisites</h3>
  </div>
  <ul>
    <li>Familiarity with spectral indices (NDVI, NDWI) and multispectral image interpretation</li>
    <li>Basic SAR concepts: backscatter, speckle, and image geometry (Tutorials 01–07)</li>
    <li>Experience with Google Earth Engine or Python geospatial libraries for time-series analysis</li>
  </ul>
</div>

<div class="concept-diagram">
  <svg viewBox="0 0 620 320" xmlns="http://www.w3.org/2000/svg" style="max-width: 580px;">
    <text x="310" y="20" text-anchor="middle" font-family="Inter, sans-serif" font-size="13" fill="#111" font-weight="700">Satellite Monitoring of Water Resources</text>
    <!-- Satellite icon -->
    <rect x="280" y="32" width="60" height="20" rx="4" fill="#68625b"/>
    <rect x="255" y="36" width="25" height="12" rx="2" fill="#1e4f8a" opacity="0.4"/>
    <rect x="340" y="36" width="25" height="12" rx="2" fill="#1e4f8a" opacity="0.4"/>
    <text x="310" y="47" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#fff">SAT</text>
    <!-- Downward beams -->
    <line x1="290" y1="52" x2="120" y2="115" stroke="#1e4f8a" stroke-width="1" stroke-dasharray="4 2" opacity="0.5"/>
    <line x1="310" y1="52" x2="310" y2="115" stroke="#1e4f8a" stroke-width="1" stroke-dasharray="4 2" opacity="0.5"/>
    <line x1="330" y1="52" x2="500" y2="115" stroke="#1e4f8a" stroke-width="1" stroke-dasharray="4 2" opacity="0.5"/>
    <!-- Mountains / snow cover (left) -->
    <polygon points="30,220 80,130 130,220" fill="#68625b" opacity="0.15" stroke="#68625b" stroke-width="1"/>
    <polygon points="60,220 100,140 140,220" fill="#68625b" opacity="0.1"/>
    <polygon points="70,155 80,130 90,150 85,148 80,135 75,148" fill="#fff" stroke="#1e4f8a" stroke-width="1"/>
    <text x="80" y="125" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#1e4f8a" font-weight="600">Snow / Glacier</text>
    <text x="80" y="240" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">NDSI &gt; 0.4</text>
    <!-- Soil moisture (center-left) -->
    <rect x="150" y="175" width="100" height="45" rx="5" fill="#8b5e00" opacity="0.12" stroke="#8b5e00" stroke-width="1.2"/>
    <circle cx="170" cy="192" r="4" fill="#8b5e00" opacity="0.7"/>
    <circle cx="185" cy="192" r="4" fill="#8b5e00" opacity="0.5"/>
    <circle cx="200" cy="192" r="4" fill="#8b5e00" opacity="0.3"/>
    <circle cx="215" cy="192" r="4" fill="#8b5e00" opacity="0.15"/>
    <circle cx="230" cy="192" r="4" fill="#8b5e00" opacity="0.08"/>
    <text x="200" y="165" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#8b5e00" font-weight="600">Soil Moisture</text>
    <text x="200" y="234" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">SAR σ° / SMAP</text>
    <!-- Surface water body (center) -->
    <ellipse cx="340" cy="190" rx="65" ry="30" fill="#1e4f8a" opacity="0.18" stroke="#1e4f8a" stroke-width="1.5"/>
    <ellipse cx="340" cy="190" rx="85" ry="40" fill="none" stroke="#d92b1f" stroke-width="1.2" stroke-dasharray="5 3"/>
    <text x="340" y="175" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#1e4f8a" font-weight="600">Surface Water</text>
    <text x="340" y="195" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#1e4f8a">Current extent</text>
    <!-- Recession arrows -->
    <line x1="270" y1="180" x2="285" y2="185" stroke="#d92b1f" stroke-width="1.3"/>
    <polygon points="283,181 289,186 281,188" fill="#d92b1f"/>
    <line x1="410" y1="180" x2="395" y2="185" stroke="#d92b1f" stroke-width="1.3"/>
    <polygon points="397,181 391,186 399,188" fill="#d92b1f"/>
    <text x="340" y="244" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#d92b1f">Historical extent (recession)</text>
    <text x="340" y="258" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">NDWI / MNDWI / AWEI</text>
    <!-- Irrigated agriculture (right) -->
    <rect x="460" y="155" width="90" height="65" rx="4" fill="#165d34" opacity="0.1" stroke="#165d34" stroke-width="1.2"/>
    <line x1="475" y1="170" x2="475" y2="210" stroke="#165d34" stroke-width="1" opacity="0.5"/>
    <line x1="490" y1="170" x2="490" y2="210" stroke="#165d34" stroke-width="1" opacity="0.5"/>
    <line x1="505" y1="170" x2="505" y2="210" stroke="#165d34" stroke-width="1" opacity="0.5"/>
    <line x1="520" y1="170" x2="520" y2="210" stroke="#165d34" stroke-width="1" opacity="0.5"/>
    <line x1="535" y1="170" x2="535" y2="210" stroke="#165d34" stroke-width="1" opacity="0.5"/>
    <text x="505" y="150" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#165d34" font-weight="600">Irrigation</text>
    <text x="505" y="234" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">NDVI time-series</text>
    <!-- Seasonal timeline -->
    <line x1="60" y1="290" x2="560" y2="290" stroke="#111" stroke-width="1.5"/>
    <text x="60" y="306" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#111">Winter</text>
    <text x="185" y="306" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#111">Spring melt</text>
    <text x="310" y="306" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#111">Summer peak</text>
    <text x="435" y="306" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#111">Autumn</text>
    <text x="560" y="306" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#111">Winter</text>
    <line x1="60" y1="286" x2="60" y2="294" stroke="#111" stroke-width="1.2"/>
    <line x1="185" y1="286" x2="185" y2="294" stroke="#111" stroke-width="1.2"/>
    <line x1="310" y1="286" x2="310" y2="294" stroke="#111" stroke-width="1.2"/>
    <line x1="435" y1="286" x2="435" y2="294" stroke="#111" stroke-width="1.2"/>
    <line x1="560" y1="286" x2="560" y2="294" stroke="#111" stroke-width="1.2"/>
    <path d="M80,272 C120,265 160,265 180,272" fill="none" stroke="#1e4f8a" stroke-width="1.2"/>
    <polygon points="177,269 183,274 176,275" fill="#1e4f8a"/>
    <text x="130" y="265" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#1e4f8a">snowmelt → runoff</text>
  </svg>
  <p class="diagram-caption">Figure: Satellite monitoring of water resources in Central Asia. Different sensors and indices track snow/glacier cover, soil moisture, surface water extent and recession, and irrigation patterns across seasonal cycles.</p>
</div>

## Surface water mapping

### The physics of water detection

Water has distinct spectral properties:

1. **Low reflectance in NIR and SWIR**: Water strongly absorbs these wavelengths
2. **Smooth surface for SAR**: Calm water acts as a specular reflector, appearing dark

The spectral reflectance of water follows:

$$
\rho_{\text{water}}(\lambda) \approx \begin{cases}
0.02-0.10 & \text{visible (400-700 nm)} \\
< 0.01 & \text{NIR (700-1000 nm)} \\
\approx 0 & \text{SWIR (1000-2500 nm)}
\end{cases}
$$

### Normalized Difference Water Index (NDWI)

The classic water index exploits the NIR absorption:

$$
\text{NDWI} = \frac{\rho_{\text{Green}} - \rho_{\text{NIR}}}{\rho_{\text{Green}} + \rho_{\text{NIR}}}
$$

For Sentinel-2: $\text{NDWI} = \frac{B3 - B8}{B3 + B8}$

Water typically yields NDWI > 0, while vegetation yields NDWI < 0 due to high NIR reflectance.

### Modified NDWI (MNDWI)

SWIR provides even better water-land contrast:

$$
\text{MNDWI} = \frac{\rho_{\text{Green}} - \rho_{\text{SWIR}}}{\rho_{\text{Green}} + \rho_{\text{SWIR}}}
$$

For Sentinel-2: $\text{MNDWI} = \frac{B3 - B11}{B3 + B11}$

<div class="info-box tip">
  <strong>Key idea</strong>
  MNDWI outperforms NDWI in urban areas and for turbid water because built-up surfaces have high NIR reflectance (confusing NDWI) but moderate SWIR reflectance.
</div>

### Automated Water Extraction Index (AWEI)

For challenging environments with shadows:

$$
\text{AWEI}_{\text{nsh}} = 4(\rho_{\text{Green}} - \rho_{\text{SWIR1}}) - (0.25\rho_{\text{NIR}} + 2.75\rho_{\text{SWIR2}})
$$

### SAR-based water detection

SAR provides cloud-independent water mapping. Over calm water, the radar signal reflects away from the sensor (specular reflection), producing very low backscatter:

$$
\sigma^0_{\text{water}} \ll \sigma^0_{\text{land}}
$$

The backscatter coefficient for water at C-band (Sentinel-1) typically ranges:

$$
\sigma^0_{\text{water}} \approx -20 \text{ to } -25 \text{ dB}
$$

A simple threshold approach classifies pixels as water when:

$$
\sigma^0_{\text{VV}} < T_{\text{water}}
$$

where $T_{\text{water}} \approx -15$ to $-18$ dB depending on wind conditions.

## Wetland delineation and monitoring

### Wetland spectral signatures

Wetlands contain mixtures of water, vegetation, and saturated soil. The spectral signature varies with water depth, vegetation density, and phenology.

The Normalized Difference Vegetation Index helps distinguish vegetated wetlands:

$$
\text{NDVI} = \frac{\rho_{\text{NIR}} - \rho_{\text{Red}}}{\rho_{\text{NIR}} + \rho_{\text{Red}}}
$$

### SAR for wetland mapping

SAR penetrates vegetation and interacts with underlying water through **double-bounce scattering**:

$$
\sigma^0_{\text{flooded vegetation}} > \sigma^0_{\text{dry vegetation}}
$$

<div class="info-box tip">
  <strong>Key idea</strong>
  Counterintuitively, flooded forests appear BRIGHTER in SAR than dry forests due to double-bounce. This is opposite to open water, which appears dark.
</div>

### Wetland change detection

The coefficient of variation (CV) over a time series indicates hydrological variability:

$$
\text{CV} = \frac{\sigma_{\sigma^0}}{\mu_{\sigma^0}}
$$

High CV indicates areas with fluctuating water levels.

## Snow cover mapping

### Snow spectral properties

Snow has distinctive spectral characteristics:

$$
\rho_{\text{snow}}(\lambda) \approx \begin{cases}
0.80-0.95 & \text{visible} \\
0.40-0.70 & \text{NIR} \\
0.05-0.20 & \text{SWIR}
\end{cases}
$$

The dramatic drop in SWIR reflectance distinguishes snow from clouds, which remain bright across all bands.

### Normalized Difference Snow Index (NDSI)

The standard snow detection algorithm:

$$
\text{NDSI} = \frac{\rho_{\text{Green}} - \rho_{\text{SWIR}}}{\rho_{\text{Green}} + \rho_{\text{SWIR}}}
$$

For Sentinel-2: $\text{NDSI} = \frac{B3 - B11}{B3 + B11}$

Snow typically yields NDSI > 0.4. Combined with a brightness threshold ($\rho_{\text{Green}} > 0.1$), this effectively separates snow from water (which also has high NDSI but low visible reflectance).

### Fractional snow cover

Real pixels often contain mixed snow and bare ground. The linear spectral unmixing model:

$$
\rho_{\text{pixel}} = f_{\text{snow}} \cdot \rho_{\text{snow}} + (1 - f_{\text{snow}}) \cdot \rho_{\text{bare}}
$$

Solving for snow fraction:

$$
f_{\text{snow}} = \frac{\rho_{\text{pixel}} - \rho_{\text{bare}}}{\rho_{\text{snow}} - \rho_{\text{bare}}}
$$

The MODIS Snow Cover product uses a similar approach to estimate fractional coverage at 500m resolution.

### Snow Water Equivalent (SWE)

The water content of snowpack:

$$
\text{SWE} = \rho_{\text{snow}} \cdot d
$$

where $\rho_{\text{snow}}$ is snow density and $d$ is snow depth. Passive microwave sensors estimate SWE using brightness temperature gradients.

## Soil moisture from SAR

### Dielectric constant and moisture

The dielectric constant $\varepsilon$ controls radar backscatter:

$$
\varepsilon_{\text{wet soil}} \approx 15-40 \quad \text{vs.} \quad \varepsilon_{\text{dry soil}} \approx 3-5
$$

Higher dielectric constant increases backscatter: $\sigma^0 \propto \sqrt{\varepsilon}$

### The water cloud model

Vegetation complicates retrieval. The Water Cloud Model separates contributions:

$$
\sigma^0_{\text{total}} = \sigma^0_{\text{veg}} + \gamma^2 \sigma^0_{\text{soil}}
$$

where $\gamma^2 = \exp(-2\tau / \cos\theta)$ is the two-way vegetation transmissivity.

<div class="info-box tip">
  <strong>Key idea</strong>
  Soil moisture retrieval accuracy decreases with vegetation density. Over dense crops, SAR may not penetrate to soil.
</div>

### Change detection approach

When vegetation is stable, temporal changes relate directly to moisture:

$$
\Delta\sigma^0_{dB} \approx S_m \cdot \Delta m_v
$$

where $S_m$ is the moisture sensitivity (0.1-0.3 dB per %).

## Irrigation monitoring

### Crop water stress detection

Thermal remote sensing detects plant water stress through canopy temperature. The Crop Water Stress Index:

$$
\text{CWSI} = \frac{(T_c - T_a) - (T_c - T_a)_{\text{ll}}}{(T_c - T_a)_{\text{ul}} - (T_c - T_a)_{\text{ll}}}
$$

where $T_c$ is canopy temperature, $T_a$ is air temperature, "ll" is the lower limit (well-watered), and "ul" is the upper limit (non-transpiring).

### Evapotranspiration estimation

The energy balance approach:

$$
\text{ET} = R_n - G - H
$$

where $R_n$ is net radiation, $G$ is soil heat flux, and $H$ is sensible heat flux.

<div class="info-box tip">
  <strong>Key idea</strong>
  Landsat thermal bands at 100m resolution enable field-scale irrigation monitoring—critical for Central Asia's fragmented agricultural landscapes.
</div>

### Irrigation canal mapping

SAR maps canals as linear dark features bordered by bright levees:

$$
\nabla I = \sqrt{\left(\frac{\partial I}{\partial x}\right)^2 + \left(\frac{\partial I}{\partial y}\right)^2}
$$

## Lake and reservoir monitoring

### Water level estimation

Satellite altimetry measures surface water elevation:

$$
h = H - R - \sum \Delta R
$$

where $h$ is water surface height, $H$ is satellite altitude, $R$ is measured range.

### Surface area mapping

Multi-temporal NDWI tracks extent. For the Aral Sea:

| Year | Area (km²) | % of 1960 |
|------|------------|-----------|
| 1960 | 68,000 | 100% |
| 2000 | 24,000 | 35% |
| 2020 | 8,300 | 12% |

### Volume estimation

Combining area and level: $V = a \cdot h^b$

### The Aral Sea case study

Key observations:
1. **Separation** (1989): Split into North and South basins
2. **Eastern lobe loss** (2009-2014): Dried completely
3. **Partial recovery** (2005+): Kok-Aral dam stabilized North Aral

## Glacial lake outburst flood monitoring

### GLOF hazard assessment

Glacial lakes form behind moraine dams. Remote sensing enables lake inventory, growth monitoring, and early warning.

### Lake detection

Glacial lakes criteria: $\text{NDWI} > 0.3$, elevation > 3000m, area < 1 km²

Track expansion: $\frac{dA}{dt} = \text{annual growth rate}$

### InSAR for dam stability

Phase differences reveal displacement: $\Delta\phi = \frac{4\pi}{\lambda} \cdot d_{LOS}$

<div class="info-box tip">
  <strong>Key idea</strong>
  Glacial lakes in the Tien Shan and Pamirs pose significant flood risk. Satellite monitoring provides the most practical early warning for remote locations.
</div>

## Time-series analysis for water trends

### Harmonic analysis

Water resources exhibit seasonal patterns:

$$
y(t) = a_0 + \sum_{k=1}^{n} \left[ a_k \cos\left(\frac{2\pi k t}{T}\right) + b_k \sin\left(\frac{2\pi k t}{T}\right) \right]
$$

### Trend detection

Linear regression: $y(t) = \beta_0 + \beta_1 t + \epsilon$

The Mann-Kendall test provides non-parametric trend significance.

## Integration of optical, SAR, and thermal data

### Sensor fusion strategies

| Sensor | Strength | Limitation |
|--------|----------|------------|
| Optical | Spectral detail | Cloud sensitivity |
| SAR | All-weather | Speckle noise |
| Thermal | Temperature | Low resolution |

### Decision-level fusion

Union: $\text{Water}_{\text{fused}} = \text{Water}_{\text{optical}} \cup \text{Water}_{\text{SAR}}$

Intersection: $\text{Water}_{\text{fused}} = \text{Water}_{\text{optical}} \cap \text{Water}_{\text{SAR}}$

### Feature-level fusion

Stack indices: $\mathbf{x} = [\text{MNDWI}, \sigma^0_{\text{VV}}, \sigma^0_{\text{VH}}, \text{LST}]^T$

<div class="info-box tip">
  <strong>Key idea</strong>
  Multi-sensor fusion improves accuracy where cloud cover affects optical imagery and wind roughens water surfaces for SAR.
</div>

## Tutorial checklist

<ul class="checklist">
  <li>Understand why Central Asia faces critical water scarcity</li>
  <li>Calculate NDWI and MNDWI for surface water mapping</li>
  <li>Apply SAR thresholds for cloud-free water detection</li>
  <li>Recognize double-bounce scattering in flooded wetlands</li>
  <li>Compute NDSI for snow cover mapping</li>
  <li>Estimate fractional snow cover using spectral unmixing</li>
  <li>Understand dielectric constant relationship to soil moisture</li>
  <li>Apply the Water Cloud Model for vegetated surfaces</li>
  <li>Calculate Crop Water Stress Index from thermal data</li>
  <li>Map irrigation infrastructure using SAR edge detection</li>
  <li>Track lake area changes using multi-temporal analysis</li>
  <li>Estimate reservoir volume from area-elevation relationships</li>
  <li>Identify glacial lakes and assess GLOF hazard</li>
  <li>Apply harmonic analysis for seasonal water patterns</li>
  <li>Detect trends using Mann-Kendall statistics</li>
  <li>Implement multi-sensor fusion for robust water mapping</li>
</ul>

## Exercises

<div class="exercise">
<strong>Exercise 1: Calculate NDWI</strong><br>
A pixel has Green reflectance = 0.12 and NIR reflectance = 0.03. Calculate NDWI and determine if this is water.

<details>
<summary>Solution</summary>
$$\text{NDWI} = \frac{0.12 - 0.03}{0.12 + 0.03} = \frac{0.09}{0.15} = 0.60$$
NDWI = 0.60 > 0, indicating water.
</details>
</div>

<div class="exercise">
<strong>Exercise 2: MNDWI calculation</strong><br>
The same pixel has SWIR reflectance = 0.01. Calculate MNDWI and compare to NDWI.

<details>
<summary>Solution</summary>
$$\text{MNDWI} = \frac{0.12 - 0.01}{0.12 + 0.01} = \frac{0.11}{0.13} = 0.85$$
MNDWI (0.85) > NDWI (0.60). MNDWI provides stronger water signal due to near-zero SWIR reflectance.
</details>
</div>

<div class="exercise">
<strong>Exercise 3: SAR water threshold</strong><br>
A Sentinel-1 VV image shows backscatter values of -22 dB over a lake and -8 dB over adjacent land. What threshold would separate water from land?

<details>
<summary>Solution</summary>
A threshold of $T = -15$ dB would work. Pixels with $\sigma^0 < -15$ dB are classified as water, pixels with $\sigma^0 > -15$ dB as land. This provides equal margin from both classes.
</details>
</div>

<div class="exercise">
<strong>Exercise 4: NDSI calculation</strong><br>
A mountain pixel has Green = 0.85 and SWIR = 0.15. Calculate NDSI and determine if this is snow.

<details>
<summary>Solution</summary>
$$\text{NDSI} = \frac{0.85 - 0.15}{0.85 + 0.15} = \frac{0.70}{1.00} = 0.70$$
NDSI = 0.70 > 0.4, indicating snow. The high Green reflectance confirms this is snow, not water.
</details>
</div>

<div class="exercise">
<strong>Exercise 5: Fractional snow cover</strong><br>
A 500m pixel has measured reflectance of 0.55 at Green wavelength. Pure snow reflectance = 0.90, bare rock = 0.20. Calculate snow fraction.

<details>
<summary>Solution</summary>
$$f_{\text{snow}} = \frac{0.55 - 0.20}{0.90 - 0.20} = \frac{0.35}{0.70} = 0.50$$
The pixel is 50% snow-covered.
</details>
</div>

<div class="exercise">
<strong>Exercise 6: Snow water equivalent</strong><br>
A snowpack is 80 cm deep with density 350 kg/m³. Calculate SWE in mm.

<details>
<summary>Solution</summary>
$$\text{SWE} = \rho \cdot d = 350 \text{ kg/m}^3 \times 0.80 \text{ m} = 280 \text{ kg/m}^2 = 280 \text{ mm}$$
</details>
</div>

<div class="exercise">
<strong>Exercise 7: Dielectric constant effect</strong><br>
If backscatter is proportional to $\sqrt{\varepsilon}$, by what factor does backscatter increase when soil moisture raises $\varepsilon$ from 4 to 25?

<details>
<summary>Solution</summary>
$$\frac{\sigma^0_{\text{wet}}}{\sigma^0_{\text{dry}}} = \frac{\sqrt{25}}{\sqrt{4}} = \frac{5}{2} = 2.5$$
Backscatter increases by factor of 2.5 (about 4 dB).
</details>
</div>

<div class="exercise">
<strong>Exercise 8: Vegetation transmissivity</strong><br>
Calculate two-way vegetation transmissivity $\gamma^2$ for optical depth $\tau = 0.3$ at incidence angle $\theta = 40°$.

<details>
<summary>Solution</summary>
$$\gamma^2 = \exp\left(\frac{-2 \times 0.3}{\cos 40°}\right) = \exp\left(\frac{-0.6}{0.766}\right) = \exp(-0.783) = 0.457$$
About 46% of the soil signal penetrates through vegetation.
</details>
</div>

<div class="exercise">
<strong>Exercise 9: Soil moisture sensitivity</strong><br>
If moisture sensitivity is 0.2 dB/% and backscatter increased by 1.6 dB, estimate the soil moisture change.

<details>
<summary>Solution</summary>
$$\Delta m_v = \frac{\Delta\sigma^0}{S_m} = \frac{1.6 \text{ dB}}{0.2 \text{ dB/\%}} = 8\%$$
Volumetric soil moisture increased by 8%.
</details>
</div>

<div class="exercise">
<strong>Exercise 10: CWSI interpretation</strong><br>
A field has CWSI = 0.7. What does this indicate about irrigation needs?

<details>
<summary>Solution</summary>
CWSI ranges from 0 (well-watered) to 1 (severe stress). A value of 0.7 indicates significant water stress—the canopy temperature is 70% of the way from well-watered to non-transpiring. Irrigation is urgently needed.
</details>
</div>

<div class="exercise">
<strong>Exercise 11: Energy balance</strong><br>
A field has Rn = 500 W/m², G = 50 W/m², H = 150 W/m². Calculate latent heat flux and equivalent ET rate.

<details>
<summary>Solution</summary>
$$LE = R_n - G - H = 500 - 50 - 150 = 300 \text{ W/m}^2$$
With latent heat of vaporization $\lambda = 2.45$ MJ/kg:
$$\text{ET} = \frac{300 \times 3600}{2.45 \times 10^6} = 0.44 \text{ mm/hour}$$
</details>
</div>

<div class="exercise">
<strong>Exercise 12: Lake area change</strong><br>
A lake measured 450 km² in 2000 and 380 km² in 2020. Calculate the average annual rate of change.

<details>
<summary>Solution</summary>
$$\frac{dA}{dt} = \frac{380 - 450}{2020 - 2000} = \frac{-70}{20} = -3.5 \text{ km}^2\text{/year}$$
The lake is shrinking at 3.5 km² per year.
</details>
</div>

<div class="exercise">
<strong>Exercise 13: Volume estimation</strong><br>
A reservoir has area-height relationship $V = 0.5h^{2.3}$ (V in km³, h in m). Calculate volume at h = 25 m.

<details>
<summary>Solution</summary>
$$V = 0.5 \times 25^{2.3} = 0.5 \times 1445.4 = 722.7 \text{ km}^3$$
</details>
</div>

<div class="exercise">
<strong>Exercise 14: Altimetry calculation</strong><br>
Satellite altitude H = 800 km, measured range R = 799.650 km, total corrections = 2.5 m. Calculate water surface height.

<details>
<summary>Solution</summary>
$$h = H - R - \sum\Delta R = 800,000 - 799,650 - 2.5 = 347.5 \text{ m}$$
</details>
</div>

<div class="exercise">
<strong>Exercise 15: InSAR displacement</strong><br>
A Sentinel-1 interferogram shows phase difference of 2.5 radians over a moraine dam. Wavelength = 5.6 cm. Calculate LOS displacement.

<details>
<summary>Solution</summary>
$$d_{LOS} = \frac{\Delta\phi \cdot \lambda}{4\pi} = \frac{2.5 \times 5.6}{4\pi} = \frac{14}{12.57} = 1.11 \text{ cm}$$
</details>
</div>

<div class="exercise">
<strong>Exercise 16: Glacial lake growth rate</strong><br>
A glacial lake grew from 0.15 km² in 2015 to 0.24 km² in 2023. Calculate growth rate and project area in 2030.

<details>
<summary>Solution</summary>
$$\frac{dA}{dt} = \frac{0.24 - 0.15}{2023 - 2015} = \frac{0.09}{8} = 0.01125 \text{ km}^2\text{/year}$$
Projected 2030 area: $0.24 + 7 \times 0.01125 = 0.32$ km²
</details>
</div>

<div class="exercise">
<strong>Exercise 17: Harmonic amplitude</strong><br>
A water body has harmonic coefficients $a_1 = 0.15$ and $b_1 = 0.08$ for NDWI. Calculate the amplitude of seasonal variation.

<details>
<summary>Solution</summary>
$$A_1 = \sqrt{a_1^2 + b_1^2} = \sqrt{0.15^2 + 0.08^2} = \sqrt{0.0289} = 0.17$$
</details>
</div>

<div class="exercise">
<strong>Exercise 18: Trend magnitude</strong><br>
Linear regression of lake area gives: Area = 520 - 2.8t, where t is years since 2000. What is the predicted area in 2035?

<details>
<summary>Solution</summary>
$$\text{Area}_{2035} = 520 - 2.8 \times 35 = 520 - 98 = 422 \text{ km}^2$$
</details>
</div>

<div class="exercise">
<strong>Exercise 19: Double-bounce interpretation</strong><br>
VH backscatter over a wetland increased from -18 dB to -12 dB between dry and wet seasons. What does this indicate?

<details>
<summary>Solution</summary>
The 6 dB increase indicates flooding beneath vegetation. Double-bounce scattering (water-stem-water) enhanced backscatter. The wetland is now flooded but vegetated—typical of seasonal marshlands.
</details>
</div>

<div class="exercise">
<strong>Exercise 20: Sensor fusion logic</strong><br>
Optical classification marks a pixel as water (MNDWI = 0.45). SAR marks it as land (σ⁰ = -10 dB). Using intersection rule, what is the fused classification?

<details>
<summary>Solution</summary>
Using intersection: Water requires BOTH sensors to agree. Since SAR says land, the fused result is NOT water. This might be a wind-roughened water surface appearing bright in SAR, or a false positive in the optical classification.
</details>
</div>

<div class="exercise">
<strong>Exercise 21: Cloud masking strategy</strong><br>
You need weekly water extent maps for 6 months. Cloud cover averages 40%. How should you combine optical and SAR data?

<details>
<summary>Solution</summary>
Use SAR (Sentinel-1, 6-day revisit) as the baseline for consistent temporal coverage. Gap-fill with optical when cloud-free. This ensures no gaps due to clouds while benefiting from optical's spectral precision when available.
</details>
</div>

<div class="exercise">
<strong>Exercise 22: AWEI calculation</strong><br>
Calculate AWEI_nsh for: Green = 0.10, NIR = 0.02, SWIR1 = 0.01, SWIR2 = 0.005.

<details>
<summary>Solution</summary>
$$\text{AWEI}_{\text{nsh}} = 4(0.10 - 0.01) - (0.25 \times 0.02 + 2.75 \times 0.005)$$
$$= 4(0.09) - (0.005 + 0.01375) = 0.36 - 0.01875 = 0.341$$
Positive value indicates water.
</details>
</div>

<div class="exercise">
<strong>Exercise 23: Photon energy</strong><br>
Calculate the energy of a SWIR photon at 1610 nm wavelength.

<details>
<summary>Solution</summary>
$$E = \frac{hc}{\lambda} = \frac{6.626 \times 10^{-34} \times 3 \times 10^8}{1610 \times 10^{-9}}$$
$$= \frac{1.988 \times 10^{-25}}{1.61 \times 10^{-6}} = 1.23 \times 10^{-19} \text{ J} = 0.77 \text{ eV}$$
</details>
</div>

<div class="exercise">
<strong>Exercise 24: Aral Sea decline</strong><br>
The Aral Sea lost 90% of its 1960 volume by 2010. If initial volume was 1,090 km³, calculate 2010 volume and average annual loss.

<details>
<summary>Solution</summary>
2010 volume: $1090 \times 0.10 = 109$ km³<br>
Volume lost: $1090 - 109 = 981$ km³<br>
Annual loss: $\frac{981}{50} = 19.6$ km³/year
</details>
</div>

<div class="exercise">
<strong>Exercise 25: Coefficient of variation</strong><br>
A wetland pixel has mean backscatter -15 dB (linear: 0.0316) with standard deviation 0.012 in linear units. Calculate CV.

<details>
<summary>Solution</summary>
$$\text{CV} = \frac{\sigma}{\mu} = \frac{0.012}{0.0316} = 0.38$$
High CV (38%) indicates fluctuating water levels—characteristic of wetland margins.
</details>
</div>

<div class="exercise">
<strong>Exercise 26: Snow vs. cloud discrimination</strong><br>
Two pixels both have Green = 0.85. Pixel A has SWIR = 0.12, Pixel B has SWIR = 0.78. Which is snow?

<details>
<summary>Solution</summary>
Pixel A: NDSI = $(0.85 - 0.12)/(0.85 + 0.12) = 0.75$ → Snow<br>
Pixel B: NDSI = $(0.85 - 0.78)/(0.85 + 0.78) = 0.04$ → Cloud<br>
Snow absorbs SWIR; clouds reflect it.
</details>
</div>

<div class="exercise">
<strong>Exercise 27: Irrigation efficiency</strong><br>
A 100-hectare field receives 800 mm irrigation annually. ET is estimated at 520 mm. Calculate irrigation efficiency.

<details>
<summary>Solution</summary>
$$\text{Efficiency} = \frac{\text{ET}}{\text{Applied water}} = \frac{520}{800} = 0.65 = 65\%$$
35% of applied water is lost to runoff, deep percolation, or evaporation from soil.
</details>
</div>

<div class="exercise">
<strong>Exercise 28: Multi-temporal composite</strong><br>
You have 24 Sentinel-1 images over 6 months. How would you create a water frequency map?

<details>
<summary>Solution</summary>
1. Apply water threshold to each image<br>
2. Create binary water/non-water masks<br>
3. Sum masks: $\text{Frequency} = \frac{\sum_{i=1}^{24} W_i}{24}$<br>
4. Values near 1.0 = permanent water; 0.3-0.7 = seasonal flooding; near 0 = permanent land
</details>
</div>

<div class="exercise">
<strong>Exercise 29: Reservoir storage change</strong><br>
A reservoir's water level dropped from 245m to 238m. Area decreased from 85 km² to 72 km². Estimate volume change using trapezoidal rule.

<details>
<summary>Solution</summary>
$$\Delta V = \frac{(A_1 + A_2)}{2} \times \Delta h = \frac{(85 + 72)}{2} \times 7 = 78.5 \times 7 = 549.5 \text{ million m}^3$$
The reservoir lost approximately 0.55 km³.
</details>
</div>

<div class="exercise">
<strong>Exercise 30: Central Asia water balance</strong><br>
The Amu Darya contributes 78 km³/year at its source. Irrigation withdrawals are 45 km³, evaporation losses 18 km³. How much reaches the Aral Sea?

<details>
<summary>Solution</summary>
$$\text{Flow to Aral} = 78 - 45 - 18 = 15 \text{ km}^3\text{/year}$$
Only 19% of the original flow reaches the Aral Sea. Historically, 55+ km³/year was needed to maintain the sea level.
</details>
</div>
