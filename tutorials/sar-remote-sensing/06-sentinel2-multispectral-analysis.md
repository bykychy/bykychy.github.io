---
layout: tutorial
title: "How Sentinel-2 Multispectral Data Reveals Landscape Patterns in Central Asia"
description: "A math-first tutorial on optical remote sensing, spectral indices, band combinations, and vegetation/soil analysis for Central Asia, with 30 exercises and solutions."
difficulty: intermediate
estimated_time: "2-3 hours"
series: sar-remote-sensing
order: 6
---

## Why this tutorial matters

Sentinel-2 is a **multispectral optical sensor** that captures sunlight reflected from Earth's surface across 13 spectral bands. Unlike Sentinel-1 SAR, which actively sends microwave pulses, Sentinel-2 passively measures reflected solar radiation in the visible, near-infrared (NIR), and shortwave infrared (SWIR) regions.

This fundamental difference makes the two sensors complementary:

| Property | Sentinel-1 SAR | Sentinel-2 Optical |
|----------|----------------|-------------------|
| Energy source | Active (microwave) | Passive (sunlight) |
| Cloud penetration | Yes | No |
| Day/night operation | Yes | Daytime only |
| Sensitivity | Roughness, moisture, structure | Pigments, minerals, water content |
| Spatial resolution | 10-20 m | 10-60 m depending on band |

<div class="info-box tip">
  <strong>Key idea</strong>
  SAR tells you about surface structure and moisture. Optical tells you about chemical composition and biological state. Together, they form a powerful diagnostic toolkit for understanding Central Asian landscapes.
</div>

For Central Asia, Sentinel-2 excels at monitoring:

- vegetation health in irrigated croplands,
- water body extent in shrinking lakes like the Aral Sea,
- bare soil composition in deserts and steppes,
- snow cover dynamics in the Tien Shan and Pamir mountains,
- land cover change across rapidly developing regions.

<div class="learning-objectives">
  <div class="learning-objectives-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#1e4f8a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <h3>What you will learn</h3>
  </div>
  <ul>
    <li>How Sentinel-2 captures reflected solar radiation across 13 spectral bands from visible to shortwave infrared</li>
    <li>How to compute and interpret spectral indices including NDVI, NDWI, and NDBI for vegetation, water, and built-up areas</li>
    <li>How different surface materials (vegetation, soil, water, snow) produce distinctive spectral signatures</li>
    <li>How to combine Sentinel-1 SAR and Sentinel-2 optical data for more robust landscape analysis</li>
    <li>How to apply multispectral analysis to Central Asian challenges: irrigation monitoring, Aral Sea change, and land cover mapping</li>
  </ul>
</div>

<div class="prerequisites">
  <div class="prerequisites-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#8b5e00" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
    <h3>Prerequisites</h3>
  </div>
  <ul>
    <li>Basic understanding of electromagnetic radiation, wavelength, and photon energy</li>
    <li>Familiarity with raster image band math and normalized difference indices</li>
    <li>Completed SAR tutorials (01-05) recommended for understanding SAR-optical synergy</li>
  </ul>
</div>

<div class="concept-diagram">
  <svg viewBox="0 0 620 320" xmlns="http://www.w3.org/2000/svg" style="max-width: 580px;">
    <!-- Background -->
    <rect x="0" y="0" width="620" height="320" fill="#f8f9fa" rx="8"/>
    <text x="310" y="22" text-anchor="middle" font-family="Inter, sans-serif" font-size="13" font-weight="bold" fill="#1e4f8a">Sentinel-2 Spectral Bands and Surface Spectral Signatures</text>

    <!-- Spectrum bar area -->
    <rect x="40" y="40" width="540" height="50" rx="4" fill="#fff" stroke="#e0ddd8" stroke-width="1"/>

    <!-- Spectrum gradient bands -->
    <!-- Blue ~443-490nm -->
    <rect x="65" y="48" width="50" height="34" rx="2" fill="#2255cc" opacity="0.7"/>
    <text x="90" y="68" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#fff">Blue</text>
    <text x="90" y="78" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#c5d5ea">490 nm</text>

    <!-- Green ~560nm -->
    <rect x="120" y="48" width="50" height="34" rx="2" fill="#22aa44" opacity="0.7"/>
    <text x="145" y="68" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#fff">Green</text>
    <text x="145" y="78" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#eeffee">560 nm</text>

    <!-- Red ~665nm -->
    <rect x="175" y="48" width="50" height="34" rx="2" fill="#cc2222" opacity="0.7"/>
    <text x="200" y="68" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#fff">Red</text>
    <text x="200" y="78" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#fdd">665 nm</text>

    <!-- Red Edge ~705-783nm -->
    <rect x="230" y="48" width="65" height="34" rx="2" fill="#991111" opacity="0.6"/>
    <text x="262" y="68" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#fff">Red Edge</text>
    <text x="262" y="78" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#fdd">705-783</text>

    <!-- NIR ~842nm -->
    <rect x="300" y="48" width="70" height="34" rx="2" fill="#882222" opacity="0.5"/>
    <text x="335" y="68" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#fff">NIR</text>
    <text x="335" y="78" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#fdd">842 nm</text>

    <!-- SWIR1 ~1610nm -->
    <rect x="395" y="48" width="75" height="34" rx="2" fill="#554433" opacity="0.6"/>
    <text x="432" y="68" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#fff">SWIR1</text>
    <text x="432" y="78" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#e8e0d0">1610 nm</text>

    <!-- SWIR2 ~2190nm -->
    <rect x="475" y="48" width="75" height="34" rx="2" fill="#332211" opacity="0.6"/>
    <text x="512" y="68" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#fff">SWIR2</text>
    <text x="512" y="78" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#e8e0d0">2190 nm</text>

    <!-- Wavelength axis label -->
    <text x="310" y="100" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">Wavelength &rarr;</text>

    <!-- Spectral response chart -->
    <rect x="40" y="110" width="540" height="165" rx="4" fill="#fff" stroke="#e0ddd8" stroke-width="1"/>

    <!-- Y-axis for reflectance -->
    <line x1="65" y1="125" x2="65" y2="265" stroke="#111" stroke-width="1"/>
    <text x="50" y="195" font-family="Inter, sans-serif" font-size="8" fill="#111" transform="rotate(-90, 50, 195)">Reflectance</text>
    <text x="62" y="132" text-anchor="end" font-family="Inter, sans-serif" font-size="7" fill="#68625b">High</text>
    <text x="62" y="262" text-anchor="end" font-family="Inter, sans-serif" font-size="7" fill="#68625b">Low</text>

    <!-- X-axis bands -->
    <line x1="65" y1="265" x2="560" y2="265" stroke="#111" stroke-width="1"/>
    <text x="90" y="275" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#68625b">B</text>
    <text x="145" y="275" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#68625b">G</text>
    <text x="200" y="275" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#68625b">R</text>
    <text x="262" y="275" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#68625b">RE</text>
    <text x="335" y="275" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#68625b">NIR</text>
    <text x="432" y="275" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#68625b">SWIR1</text>
    <text x="512" y="275" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#68625b">SWIR2</text>

    <!-- Vegetation spectral curve (green) - low in blue/red, high in NIR, dip in SWIR -->
    <polyline points="90,230 145,215 200,235 262,175 335,140 432,195 512,210" stroke="#165d34" stroke-width="2.5" fill="none"/>
    <circle cx="90" cy="230" r="3" fill="#165d34"/>
    <circle cx="145" cy="215" r="3" fill="#165d34"/>
    <circle cx="200" cy="235" r="3" fill="#165d34"/>
    <circle cx="262" cy="175" r="3" fill="#165d34"/>
    <circle cx="335" cy="140" r="3" fill="#165d34"/>
    <circle cx="432" cy="195" r="3" fill="#165d34"/>
    <circle cx="512" cy="210" r="3" fill="#165d34"/>

    <!-- Bare soil spectral curve (brown) - gradually increasing -->
    <polyline points="90,245 145,235 200,225 262,218 335,210 432,200 512,195" stroke="#8b5e00" stroke-width="2.5" fill="none" stroke-dasharray="6,3"/>
    <circle cx="90" cy="245" r="3" fill="#8b5e00"/>
    <circle cx="145" cy="235" r="3" fill="#8b5e00"/>
    <circle cx="200" cy="225" r="3" fill="#8b5e00"/>
    <circle cx="262" cy="218" r="3" fill="#8b5e00"/>
    <circle cx="335" cy="210" r="3" fill="#8b5e00"/>
    <circle cx="432" cy="200" r="3" fill="#8b5e00"/>
    <circle cx="512" cy="195" r="3" fill="#8b5e00"/>

    <!-- Water spectral curve (blue) - high in blue/green, drops off -->
    <polyline points="90,195 145,210 200,240 262,250 335,255 432,258 512,258" stroke="#1e4f8a" stroke-width="2.5" fill="none" stroke-dasharray="2,3"/>
    <circle cx="90" cy="195" r="3" fill="#1e4f8a"/>
    <circle cx="145" cy="210" r="3" fill="#1e4f8a"/>
    <circle cx="200" cy="240" r="3" fill="#1e4f8a"/>
    <circle cx="262" cy="250" r="3" fill="#1e4f8a"/>
    <circle cx="335" cy="255" r="3" fill="#1e4f8a"/>
    <circle cx="432" cy="258" r="3" fill="#1e4f8a"/>
    <circle cx="512" cy="258" r="3" fill="#1e4f8a"/>

    <!-- Red edge annotation -->
    <path d="M 230 175 L 230 145 L 270 145" stroke="#d92b1f" stroke-width="1" fill="none" stroke-dasharray="3,2"/>
    <text x="275" y="148" font-family="Inter, sans-serif" font-size="8" fill="#d92b1f">Red edge &mdash; key for</text>
    <text x="275" y="157" font-family="Inter, sans-serif" font-size="8" fill="#d92b1f">vegetation detection</text>

    <!-- Legend -->
    <rect x="400" y="120" width="155" height="55" rx="4" fill="#fff" stroke="#e0ddd8" stroke-width="1"/>
    <line x1="410" y1="133" x2="430" y2="133" stroke="#165d34" stroke-width="2.5"/>
    <text x="435" y="137" font-family="Inter, sans-serif" font-size="9" fill="#111">Vegetation</text>
    <line x1="410" y1="148" x2="430" y2="148" stroke="#8b5e00" stroke-width="2.5" stroke-dasharray="6,3"/>
    <text x="435" y="152" font-family="Inter, sans-serif" font-size="9" fill="#111">Bare soil</text>
    <line x1="410" y1="163" x2="430" y2="163" stroke="#1e4f8a" stroke-width="2.5" stroke-dasharray="2,3"/>
    <text x="435" y="167" font-family="Inter, sans-serif" font-size="9" fill="#111">Water</text>

    <!-- NDVI annotation -->
    <rect x="40" y="288" width="540" height="24" rx="4" fill="#eef2f7" stroke="#1e4f8a" stroke-width="1"/>
    <text x="310" y="304" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#1e4f8a">Key index: NDVI = (NIR &minus; Red) / (NIR + Red)  |  Vegetation: NDVI &gt; 0.3  |  Water: NDVI &lt; 0  |  Soil: NDVI &asymp; 0.1&ndash;0.2</text>
  </svg>
  <p class="diagram-caption">Figure: Sentinel-2 spectral bands spanning visible to shortwave infrared, with characteristic spectral signatures for vegetation (high NIR reflectance), bare soil (gradually increasing), and water (rapid decrease beyond visible). The red edge region is critical for vegetation health assessment.</p>
</div>


## The electromagnetic spectrum and Sentinel-2 bands

### Electromagnetic radiation basics

All remote sensing depends on electromagnetic radiation. The energy carried by a photon is:

$$
E = h \nu = \frac{hc}{\lambda}
$$

where $h \approx 6.626 \times 10^{-34}\ \text{J·s}$ is Planck's constant, $c \approx 3 \times 10^8\ \text{m/s}$ is the speed of light, $\nu$ is frequency, and $\lambda$ is wavelength.

Shorter wavelengths carry more energy. Blue light ($\lambda \approx 450\ \text{nm}$) is more energetic than red light ($\lambda \approx 650\ \text{nm}$), which is why blue scatters more in the atmosphere (Rayleigh scattering).

### Sentinel-2 band specifications

Sentinel-2 carries the Multi-Spectral Instrument (MSI) with 13 bands:

| Band | Central wavelength (nm) | Resolution (m) | Primary use |
|------|------------------------|----------------|-------------|
| B1 | 443 | 60 | Aerosol detection |
| B2 | 490 | 10 | Blue (water, aerosol) |
| B3 | 560 | 10 | Green (vegetation peak) |
| B4 | 665 | 10 | Red (chlorophyll absorption) |
| B5 | 705 | 20 | Red edge 1 |
| B6 | 740 | 20 | Red edge 2 |
| B7 | 783 | 20 | Red edge 3 |
| B8 | 842 | 10 | NIR (vegetation structure) |
| B8A | 865 | 20 | NIR narrow |
| B9 | 945 | 60 | Water vapor |
| B10 | 1375 | 60 | Cirrus detection |
| B11 | 1610 | 20 | SWIR 1 (moisture, minerals) |
| B12 | 2190 | 20 | SWIR 2 (geology, soil) |

The **red edge** bands (B5, B6, B7) are positioned at the steep transition between red absorption and NIR reflection in vegetation. This region is extremely sensitive to chlorophyll content and plant stress.

## Mathematical foundations

### Radiance and reflectance

When sunlight hits a surface, some energy is absorbed, some transmitted, and some reflected. We measure what returns to the sensor.

**Radiance** ($L$) is the power per unit area per unit solid angle per unit wavelength:

$$
L_\lambda = \frac{d\Phi}{dA \cdot d\Omega \cdot d\lambda} \quad [\text{W·m}^{-2}\text{·sr}^{-1}\text{·nm}^{-1}]
$$

**Reflectance** ($\rho$) normalizes the measurement by incoming solar irradiance:

$$
\rho_\lambda = \frac{\pi L_\lambda}{E_\lambda \cos\theta_s}
$$

where $E_\lambda$ is solar irradiance at the top of atmosphere and $\theta_s$ is the solar zenith angle.

<div class="info-box tip">
  <strong>Key idea</strong>
  Reflectance is dimensionless (0 to 1) and comparable across dates and locations. Always convert to reflectance before computing indices or comparing scenes.
</div>

### Atmospheric correction

The atmosphere scatters and absorbs light. The at-sensor radiance includes:

$$
L_{\text{sensor}} = L_{\text{path}} + T \cdot L_{\text{surface}}
$$

where $L_{\text{path}}$ is atmospheric path radiance (light scattered without reaching the surface) and $T$ is atmospheric transmittance.

Sentinel-2 Level-2A products are atmospherically corrected using the Sen2Cor processor, providing **Bottom of Atmosphere (BOA) reflectance**. For quantitative analysis, always use Level-2A data.

### The Beer-Lambert law

Absorption in vegetation canopies and water follows exponential decay:

$$
I = I_0 e^{-\alpha d}
$$

where $I_0$ is incident intensity, $\alpha$ is the absorption coefficient, and $d$ is path length. This explains why water appears darker at longer wavelengths and why dense vegetation absorbs more red light.

## Spectral indices

Spectral indices combine bands to highlight specific surface properties while reducing noise from illumination geometry and atmospheric effects.

### Normalized Difference Vegetation Index (NDVI)

The most widely used vegetation index exploits the contrast between chlorophyll absorption in red and strong NIR reflection:

$$
\text{NDVI} = \frac{\rho_{\text{NIR}} - \rho_{\text{Red}}}{\rho_{\text{NIR}} + \rho_{\text{Red}}} = \frac{B8 - B4}{B8 + B4}
$$

NDVI ranges from $-1$ to $+1$:
- $< 0$: water, snow, clouds
- $0$ to $0.2$: bare soil, rock, urban
- $0.2$ to $0.4$: sparse vegetation, grassland
- $0.4$ to $0.6$: moderate vegetation
- $> 0.6$: dense healthy vegetation

### Soil Adjusted Vegetation Index (SAVI)

In arid regions like Central Asia, bare soil significantly influences NDVI. SAVI adds a soil brightness correction factor $L$:

$$
\text{SAVI} = \frac{(B8 - B4)(1 + L)}{B8 + B4 + L}
$$

The standard value $L = 0.5$ works well for intermediate vegetation cover. For sparse vegetation, use $L = 1$; for dense vegetation, $L = 0.25$.

### Normalized Difference Water Index (NDWI)

NDWI detects water bodies and vegetation water content using green and NIR:

$$
\text{NDWI} = \frac{B3 - B8}{B3 + B8}
$$

Water has positive NDWI (reflects green, absorbs NIR), while vegetation has negative NDWI. An alternative formulation for water body detection uses SWIR:

$$
\text{MNDWI} = \frac{B3 - B11}{B3 + B11}
$$

### Normalized Difference Built-up Index (NDBI)

Urban and built-up areas reflect strongly in SWIR relative to NIR:

$$
\text{NDBI} = \frac{B11 - B8}{B11 + B8}
$$

Built-up areas typically show positive NDBI, while vegetation shows negative values.

### Bare Soil Index (BSI)

For dryland regions, identifying bare soil is critical:

$$
\text{BSI} = \frac{(B11 + B4) - (B8 + B2)}{(B11 + B4) + (B8 + B2)}
$$

Higher BSI values indicate exposed soil. This is particularly useful for mapping agricultural bare periods and desert surfaces in Central Asia.

## Band combinations for visualization

### True color (natural color)

Assign RGB channels to visible bands:
- R = B4 (Red, 665 nm)
- G = B3 (Green, 560 nm)
- B = B2 (Blue, 490 nm)

This shows the scene as human eyes would perceive it.

### False color infrared

A classic combination for vegetation analysis:
- R = B8 (NIR, 842 nm)
- G = B4 (Red, 665 nm)
- B = B3 (Green, 560 nm)

Healthy vegetation appears bright red because it reflects strongly in NIR. Water appears dark blue-black. Urban areas appear cyan or gray.

### Agriculture composite

Optimized for crop monitoring:
- R = B11 (SWIR, 1610 nm)
- G = B8 (NIR, 842 nm)
- B = B2 (Blue, 490 nm)

This combination separates crop types by their moisture and structural properties. Irrigated crops appear bright green, bare soil appears brown-orange.

### Geology composite

For mineral and lithological mapping:
- R = B12 (SWIR, 2190 nm)
- G = B11 (SWIR, 1610 nm)
- B = B2 (Blue, 490 nm)

Different rock types and soil mineralogies create distinct color signatures.

## Cloud masking and scene selection

### The cloud problem

Unlike SAR, optical sensors cannot penetrate clouds. In Central Asia, cloud cover varies seasonally:
- Summer: convective clouds in mountains
- Winter: frontal systems across plains
- Spring/Fall: generally clearer conditions

### Cloud detection

Sentinel-2 Level-2A includes a Scene Classification Layer (SCL) with classes:
- 0: No data
- 3: Cloud shadows
- 8: Cloud medium probability
- 9: Cloud high probability
- 10: Thin cirrus

A simple cloud mask excludes pixels where $\text{SCL} \in \{3, 8, 9, 10\}$.

### Composite strategies

For cloud-free analysis, create temporal composites:

**Median composite**: For each pixel, take the median reflectance across multiple dates:

$$
\rho_{\text{median}} = \text{median}(\rho_1, \rho_2, \ldots, \rho_n)
$$

**Maximum NDVI composite**: Select the observation with highest NDVI (peak vegetation):

$$
\rho_{\text{composite}} = \rho_{t^*} \quad \text{where} \quad t^* = \arg\max_t \text{NDVI}_t
$$

<div class="info-box tip">
  <strong>Key idea</strong>
  For Central Asia, June-August provides the best cloud-free coverage for lowlands. For mountain regions, September-October after monsoon retreat often gives clearest views.
</div>

## Central Asia applications

### Vegetation mapping in agricultural regions

The Fergana Valley, Chu Valley, and lowland Uzbekistan contain extensive irrigated agriculture. Sentinel-2 enables:

1. **Crop type mapping**: Different crops have distinct spectral signatures. Cotton reflects differently from wheat, rice, or orchards.

2. **Phenology tracking**: Time series of NDVI reveal planting, growth, and harvest cycles:
$$
\text{Growing season length} = t_{\text{senescence}} - t_{\text{green-up}}
$$

3. **Irrigation monitoring**: NDWI and moisture-sensitive bands detect irrigation timing and water stress.

### Water body mapping

Central Asia's water resources are under pressure. Sentinel-2 tracks:

- **Aral Sea**: The dramatic shrinkage can be quantified by thresholding MNDWI.
- **Reservoirs**: Seasonal filling patterns for Toktogul, Nurek, and other reservoirs.
- **Glacial lakes**: Expansion of moraine-dammed lakes in Tien Shan and Pamir.

A simple water mask uses:
$$
\text{Water} = \begin{cases} 1 & \text{if MNDWI} > 0 \\ 0 & \text{otherwise} \end{cases}
$$

### Soil and land degradation analysis

Desertification, salinization, and erosion are major concerns. BSI combined with soil salinity indices helps identify:

- Expanding desert margins in Karakum and Kyzylkum
- Salt accumulation in irrigated areas
- Dust source regions

### Land cover classification

Combine multiple indices and bands for supervised classification. A typical feature vector might include:

$$
\mathbf{x} = [B2, B3, B4, B8, B11, B12, \text{NDVI}, \text{NDWI}, \text{BSI}]
$$

Common classifiers include Random Forest, Support Vector Machines, and neural networks.

## Combining Sentinel-2 with Sentinel-1 SAR

### Complementary information

| Feature | S1 SAR strength | S2 Optical strength |
|---------|-----------------|---------------------|
| Structure | High | Moderate |
| Moisture | High | Moderate (SWIR) |
| Pigments | None | High |
| Minerals | None | High |
| Cloud independence | Yes | No |
| Texture | High | Moderate |

### Fusion approaches

**Simple stacking**: Combine bands from both sensors as input to classification:
$$
\mathbf{x}_{\text{fused}} = [B2, B3, B4, B8, \ldots, \text{VV}, \text{VH}]
$$

**Index fusion**: Create combined indices:
$$
\text{SAR-Optical Index} = \frac{\text{VH}}{\text{VV}} \times \text{NDVI}
$$

**Decision fusion**: Classify separately, then combine predictions.

<div class="info-box tip">
  <strong>Key idea</strong>
  In Central Asia, SAR provides consistent temporal sampling through cloudy periods, while optical provides richer spectral information during clear conditions. Fusing both maximizes information extraction.
</div>

## A practical workflow

### Step 1: Define your question

What landscape property do you want to map? Examples:
- Where is irrigated agriculture expanding?
- How has the Aral Sea shoreline changed?
- What areas show vegetation stress?

### Step 2: Select appropriate imagery

- Use Level-2A (atmospherically corrected) products
- Choose dates matching your phenological question
- Check cloud cover percentage before downloading
- Consider seasonal composites for stability

### Step 3: Preprocess

1. Load bands at consistent resolution (resample 20m bands to 10m if needed)
2. Apply cloud mask using SCL
3. Clip to area of interest
4. Convert to reflectance if using Level-1C

### Step 4: Compute indices

Select indices relevant to your question:
- Vegetation: NDVI, SAVI, EVI
- Water: NDWI, MNDWI
- Built-up: NDBI
- Soil: BSI

### Step 5: Analyze and interpret

- Threshold indices for simple mapping
- Use supervised classification for land cover
- Compare dates for change detection
- Validate with ground truth or high-resolution imagery

### Step 6: Integrate with SAR

If temporal gaps exist due to clouds, fill with SAR-derived estimates. If structural information is needed, add SAR backscatter as features.

## Checklist for Sentinel-2 analysis in Central Asia

<ul class="checklist">
  <li>Downloaded Level-2A (BOA reflectance) products, not Level-1C (TOA radiance)</li>
  <li>Checked cloud cover percentage and inspected SCL layer</li>
  <li>Selected dates appropriate for the target phenomenon (growing season, dry season, etc.)</li>
  <li>Resampled all bands to consistent spatial resolution</li>
  <li>Applied cloud and shadow masking before analysis</li>
  <li>Computed indices using reflectance values, not raw DN</li>
  <li>Used SAVI instead of NDVI for sparse vegetation areas</li>
  <li>Considered MNDWI for water body detection in turbid conditions</li>
  <li>Created temporal composites to reduce cloud impact</li>
  <li>Validated results against known reference data</li>
  <li>Documented processing steps for reproducibility</li>
  <li>Considered SAR integration for cloudy periods or structural analysis</li>
</ul>

<div class="exercises-container">
  <h2>Exercises</h2>

  <h2>Easy exercises</h2>
  <p>These test basic understanding of concepts and simple calculations.</p>

  <div class="exercise">
    <h4>Easy 1 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
    <p>Which Sentinel-2 band has the highest spatial resolution, and what is that resolution?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Bands B2, B3, B4, and B8 all have 10 m resolution, which is the highest available on Sentinel-2.</p></div>
  </div>

  <div class="exercise">
    <h4>Easy 2 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
    <p>Calculate NDVI for a pixel with B4 = 0.05 and B8 = 0.45.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>NDVI = (0.45 - 0.05) / (0.45 + 0.05) = 0.40 / 0.50 = 0.80. This indicates dense, healthy vegetation.</p></div>
  </div>

  <div class="exercise">
    <h4>Easy 3 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
    <p>What RGB band assignment creates a true color image?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>R = B4 (Red), G = B3 (Green), B = B2 (Blue). This matches human color perception.</p></div>
  </div>

  <div class="exercise">
    <h4>Easy 4 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
    <p>Why does water appear dark in the NIR band (B8)?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Water strongly absorbs near-infrared radiation, reflecting very little back to the sensor. This is why NIR-based indices like NDWI are effective for water detection.</p></div>
  </div>

  <div class="exercise">
    <h4>Easy 5 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
    <p>What is the difference between Level-1C and Level-2A Sentinel-2 products?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Level-1C provides Top of Atmosphere (TOA) reflectance, while Level-2A provides Bottom of Atmosphere (BOA) reflectance after atmospheric correction. Level-2A is preferred for quantitative analysis.</p></div>
  </div>

  <div class="exercise">
    <h4>Easy 6 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
    <p>Which spectral region do the red edge bands (B5, B6, B7) cover, and why are they useful?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>They cover 705-783 nm, the transition zone between red absorption and NIR reflection. This region is highly sensitive to chlorophyll content and vegetation stress.</p></div>
  </div>

  <div class="exercise">
    <h4>Easy 7 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
    <p>A pixel has NDVI = -0.2. What type of surface is this likely to be?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Negative NDVI typically indicates water, snow, or clouds, since these absorb more NIR than red light.</p></div>
  </div>

  <div class="exercise">
    <h4>Easy 8 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
    <p>What is the primary advantage of SAR over optical sensors for Central Asian monitoring?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>SAR can image through clouds and operate day or night, providing consistent temporal coverage regardless of weather or solar illumination.</p></div>
  </div>

  <div class="exercise">
    <h4>Easy 9 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
    <p>Calculate NDWI for a water pixel with B3 = 0.12 and B8 = 0.02.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>NDWI = (0.12 - 0.02) / (0.12 + 0.02) = 0.10 / 0.14 ≈ 0.71. Positive NDWI confirms this is water.</p></div>
  </div>

  <div class="exercise">
    <h4>Easy 10 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
    <p>Why is June-August generally the best period for Sentinel-2 analysis in Central Asian lowlands?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>This period typically has the lowest cloud cover, longest daylight hours, and peak vegetation activity, providing the clearest images with maximum information content.</p></div>
  </div>

  <h2>Medium exercises</h2>
  <p>These require applying formulas and reasoning about physical processes.</p>

  <div class="exercise">
    <h4>Medium 1 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
    <p>Calculate SAVI with L = 0.5 for a sparse vegetation pixel with B4 = 0.15 and B8 = 0.30.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>SAVI = (0.30 - 0.15)(1 + 0.5) / (0.30 + 0.15 + 0.5) = (0.15)(1.5) / 0.95 = 0.225 / 0.95 ≈ 0.24. This is lower than the equivalent NDVI of 0.33, showing the soil adjustment effect.</p></div>
  </div>

  <div class="exercise">
    <h4>Medium 2 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
    <p>Why does healthy vegetation appear red in a false color infrared composite?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>In false color infrared, NIR (B8) is assigned to the red channel. Healthy vegetation reflects strongly in NIR due to leaf cell structure, so it appears bright red. The combination R=B8, G=B4, B=B3 makes vegetation dominant in red.</p></div>
  </div>

  <div class="exercise">
    <h4>Medium 3 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
    <p>A scene has 30% cloud cover. If you have 5 dates available, what is the probability that at least one date is cloud-free over a given pixel, assuming clouds are randomly distributed?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>P(cloudy on one date) = 0.30. P(cloudy on all 5 dates) = 0.30^5 = 0.00243. P(at least one clear) = 1 - 0.00243 ≈ 0.998 or 99.8%.</p></div>
  </div>

  <div class="exercise">
    <h4>Medium 4 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
    <p>Calculate BSI for a bare soil pixel with B2 = 0.10, B4 = 0.20, B8 = 0.25, B11 = 0.35.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>BSI = ((0.35 + 0.20) - (0.25 + 0.10)) / ((0.35 + 0.20) + (0.25 + 0.10)) = (0.55 - 0.35) / (0.55 + 0.35) = 0.20 / 0.90 ≈ 0.22. Positive value indicates bare soil.</p></div>
  </div>

  <div class="exercise">
    <h4>Medium 5 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
    <p>Explain why MNDWI (using SWIR) is often better than NDWI (using NIR) for detecting water in turbid conditions.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Turbid water contains suspended sediments that scatter NIR light back to the sensor, reducing the NIR absorption signal. SWIR penetrates less into water and shows stronger absorption contrast regardless of turbidity, making MNDWI more robust.</p></div>
  </div>

  <div class="exercise">
    <h4>Medium 6 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
    <p>You have NDVI values [0.35, 0.42, 0.38, 0.45, 0.40] across 5 dates. Calculate the temporal mean and identify which date shows the peak.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Mean = (0.35 + 0.42 + 0.38 + 0.45 + 0.40) / 5 = 2.00 / 5 = 0.40. The peak is 0.45 on date 4, which would be selected in a maximum NDVI composite.</p></div>
  </div>

  <div class="exercise">
    <h4>Medium 7 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
    <p>Why do different crop types have different spectral signatures in Sentinel-2 imagery?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Crops differ in leaf structure, chlorophyll content, canopy architecture, water content, and growth stage. These affect how they absorb and reflect light across different wavelengths. Cotton has different cell structure than wheat; rice paddies have standing water; orchards have gaps between trees.</p></div>
  </div>

  <div class="exercise">
    <h4>Medium 8 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
    <p>Calculate the photon energy for blue light at 490 nm. Use h = 6.626 × 10⁻³⁴ J·s and c = 3 × 10⁸ m/s.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>E = hc/λ = (6.626 × 10⁻³⁴)(3 × 10⁸) / (490 × 10⁻⁹) = (1.988 × 10⁻²⁵) / (4.9 × 10⁻⁷) ≈ 4.06 × 10⁻¹⁹ J. Blue photons carry more energy than red photons.</p></div>
  </div>

  <div class="exercise">
    <h4>Medium 9 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
    <p>A reservoir's MNDWI changes from 0.65 in April to 0.35 in September. What does this suggest?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>The decrease in MNDWI suggests the water area has shrunk. Pixels that were water (high MNDWI) in April may now be exposed shoreline or mudflats (lower MNDWI) in September after summer drawdown for irrigation.</p></div>
  </div>

  <div class="exercise">
    <h4>Medium 10 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
    <p>Why is atmospheric correction important before comparing NDVI values from different dates?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Without atmospheric correction, different atmospheric conditions (aerosols, water vapor, sun angle) on different dates affect measured radiance differently. This introduces artificial variation that masks true surface changes. BOA reflectance removes these effects for valid comparisons.</p></div>
  </div>

  <h2>Hard exercises</h2>
  <p>These require multi-step reasoning, synthesis across concepts, or handling ambiguity.</p>

  <div class="exercise">
    <h4>Hard 1 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
    <p>Design a decision tree to distinguish water, vegetation, bare soil, and built-up areas using only NDVI, NDWI, and NDBI.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>1. If NDWI > 0 → Water. 2. Else if NDVI > 0.3 → Vegetation. 3. Else if NDBI > 0 → Built-up. 4. Else → Bare soil. Thresholds may need adjustment for specific regions, and mixed pixels may require fuzzy logic or more bands.</p></div>
  </div>

  <div class="exercise">
    <h4>Hard 2 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
    <p>A cotton field shows NDVI = 0.65 in July but NDVI = 0.25 in August with no change in precipitation. Propose two hypotheses and how you would test them.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>H1: Harvest occurred between observations. Test: Check if surrounding fields show similar drops and match known harvest timing. H2: Irrigation was cut off causing stress. Test: Examine NDWI/SWIR for moisture changes; check SAR backscatter for soil moisture proxy. Field visit or farmer records would confirm.</p></div>
  </div>

  <div class="exercise">
    <h4>Hard 3 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
    <p>Explain mathematically why the normalized difference form (A-B)/(A+B) reduces illumination effects compared to a simple ratio A/B.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>If illumination scales both bands by factor k, then: Simple ratio: (kA)/(kB) = A/B (unchanged). Normalized difference: (kA - kB)/(kA + kB) = k(A-B)/k(A+B) = (A-B)/(A+B) (unchanged). Both are illumination-invariant, but the normalized form bounds values to [-1,1] and reduces sensitivity to very small denominators that could cause ratio instability.</p></div>
  </div>

  <div class="exercise">
    <h4>Hard 4 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
    <p>You want to map salinization in irrigated fields. Which bands and indices would you prioritize, and why?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>SWIR bands (B11, B12) are sensitive to salt crusts which have distinct mineral absorption features. Low NDVI indicates vegetation stress from salinity. A salinity index could be: SI = √(B2 × B4). High BSI with low vegetation indicates salt-affected bare soil. Time series showing declining NDVI over years suggests progressive salinization.</p></div>
  </div>

  <div class="exercise">
    <h4>Hard 5 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
    <p>Derive the relationship between NDVI values 0.2 and 0.4 in terms of the ratio B8/B4.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Let r = B8/B4. Then NDVI = (B8-B4)/(B8+B4) = (r-1)/(r+1). For NDVI=0.2: 0.2 = (r-1)/(r+1), solving: 0.2r + 0.2 = r - 1, so r = 1.5. For NDVI=0.4: 0.4r + 0.4 = r - 1, so r = 2.33. The NIR/Red ratio increases from 1.5 to 2.33 as NDVI goes from 0.2 to 0.4.</p></div>
  </div>

  <div class="exercise">
    <h4>Hard 6 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
    <p>A pixel is classified as water by MNDWI but shows positive NDVI. What could explain this, and how would you resolve it?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>This could indicate: (1) Floating vegetation or algae bloom—water present but with chlorophyll. (2) Mixed pixel with water and vegetation. (3) Shallow water with submerged vegetation. Resolution: Check B8 absolute value (true water should be very low), examine spatial context, compare with SAR (vegetation has volume scattering), or use higher resolution imagery.</p></div>
  </div>

  <div class="exercise">
    <h4>Hard 7 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
    <p>Design a workflow to estimate the area of the Aral Sea from Sentinel-2 imagery, accounting for uncertainty.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>1. Download cloud-free scenes covering the sea. 2. Compute MNDWI. 3. Apply threshold (e.g., 0) to create water mask. 4. Calculate area = pixel_count × pixel_area (100 m²). 5. Estimate uncertainty by testing threshold sensitivity (try 0.0, ±0.05, ±0.1) and reporting area range. 6. Cross-validate with SAR-based water detection for cloud-affected dates. 7. Report as mean ± standard deviation across threshold tests.</p></div>
  </div>

  <div class="exercise">
    <h4>Hard 8 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
    <p>Why might a SAR-optical fusion approach outperform either sensor alone for land cover classification in Central Asia?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>SAR captures structural information (roughness, geometry, moisture) that optical cannot directly measure. Optical captures compositional information (pigments, minerals) that SAR cannot detect. Together they reduce class confusion: e.g., wet bare soil and irrigated crops may look similar in optical but differ in SAR; different minerals may look similar in SAR but differ in optical. Fusion also provides temporal gap-filling when clouds block optical.</p></div>
  </div>

  <div class="exercise">
    <h4>Hard 9 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
    <p>Calculate the variance of NDVI for values [0.30, 0.35, 0.40, 0.45, 0.50]. What does high vs. low temporal variance indicate ecologically?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Mean = (0.30+0.35+0.40+0.45+0.50)/5 = 0.40. Deviations: [-0.10, -0.05, 0.00, 0.05, 0.10]. Squared: [0.01, 0.0025, 0, 0.0025, 0.01]. Sum = 0.025. Variance = 0.025/5 = 0.005. This is moderate variance. High variance indicates dynamic systems (agriculture, seasonal wetlands). Low variance indicates stable systems (desert, evergreen forest, urban).</p></div>
  </div>

  <div class="exercise">
    <h4>Hard 10 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
    <p>Propose a method to detect illegal irrigation using Sentinel-2 time series in a region where irrigation is officially prohibited during drought restrictions.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>1. Establish baseline: Identify known legal irrigation patterns (NDVI phenology, NDWI spikes) from pre-restriction years. 2. During restriction: Monitor for anomalous green-up (high NDVI) or moisture signatures (high NDWI/SWIR absorption) inconsistent with natural rainfall. 3. Compare suspect fields to neighbors—natural vegetation should respond uniformly to rainfall, while irrigated fields show independent greening. 4. Validate with SAR moisture detection. 5. Report fields with phenology inconsistent with precipitation data.</p></div>
  </div>
</div>

## Final takeaway

Sentinel-2 multispectral data reveals the chemical and biological state of landscapes through careful analysis of reflected sunlight across 13 spectral bands. For Central Asia, this means tracking vegetation health in irrigated systems, monitoring shrinking water bodies, mapping soil conditions in drylands, and detecting land cover change across this dynamic region.

Master three habits for effective Sentinel-2 analysis:

1. Always work with atmospherically corrected (Level-2A) data and apply cloud masks.
2. Choose indices appropriate to your question—SAVI for sparse vegetation, MNDWI for turbid water, BSI for soil exposure.
3. Integrate with SAR to fill temporal gaps and add structural information that optical sensors cannot capture.
