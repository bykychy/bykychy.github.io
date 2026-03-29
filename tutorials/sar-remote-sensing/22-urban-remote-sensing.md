---
layout: tutorial
title: "How Remote Sensing Monitors Urban Growth in Central Asia"
description: "A practical tutorial on urban expansion mapping, building extraction, urban heat islands, and infrastructure monitoring for Central Asian cities, with 30 exercises and solutions."
difficulty: intermediate
estimated_time: "2-3 hours"
series: sar-remote-sensing
order: 22
---

## Why urban remote sensing matters

Central Asia is experiencing rapid urbanization. Tashkent has grown from 2.1 million to over 3 million residents since independence. Almaty's built-up area has expanded by 40% in two decades. Bishkek, Dushanbe, and Ashgabat follow similar trajectories. This transformation creates urgent needs: monitoring informal settlements, tracking infrastructure development, managing urban heat stress, and preserving green spaces.

Ground surveys cannot keep pace with change. Remote sensing provides systematic, repeatable measurements across entire metropolitan regions. Satellite imagery reveals what is changing, where, and how fast—information essential for urban planning and sustainable development.

<div class="info-box tip">
  <strong>Key idea</strong>
  Urban areas have distinctive spectral and geometric signatures. Impervious surfaces reflect differently than vegetation. Buildings create characteristic SAR backscatter patterns. Thermal sensors reveal heat islands. Combining these data sources enables comprehensive urban monitoring.
</div>

## Urban spectral signatures

### Impervious surfaces

Urban areas are dominated by impervious surfaces—concrete, asphalt, rooftops, and paved areas that prevent water infiltration. These materials have characteristic spectral properties:

| Surface Type | Visible | NIR | SWIR | Key Characteristics |
|--------------|---------|-----|------|---------------------|
| Concrete | High (0.3-0.4) | Medium (0.25-0.35) | Medium (0.2-0.3) | Relatively flat spectrum |
| Asphalt | Low (0.05-0.15) | Low (0.1-0.2) | Low (0.1-0.15) | Dark, absorptive |
| Metal roofs | Variable | High (0.3-0.6) | High (0.4-0.7) | Highly reflective, specular |
| Clay tiles | Medium (0.2-0.3) | Medium (0.2-0.35) | Low (0.15-0.25) | Reddish color signature |

The key distinction from vegetation is in the near-infrared (NIR). Healthy vegetation strongly reflects NIR (0.4-0.6) while absorbing visible red—the basis for vegetation indices. Impervious surfaces lack this NIR reflectance jump.

### Spectral mixture in urban pixels

At 10-30 meter resolution, most urban pixels contain mixtures of materials. A single Sentinel-2 pixel might include:
- Partial building rooftop
- Adjacent street surface
- Yard vegetation
- Shadows from structures

This spectral mixing complicates classification. Pure endmember spectra rarely match actual pixel values. Sub-pixel analysis or higher resolution data addresses this challenge.

<div class="info-box tip">
  <strong>Key idea</strong>
  Urban remote sensing at moderate resolution (10-30m) fundamentally involves spectral unmixing. Understanding that pixels are mixtures—not pure classes—improves both analysis accuracy and interpretation.
</div>

## Built-up indices

### Normalized Difference Built-up Index (NDBI)

NDBI exploits the spectral difference between SWIR and NIR that characterizes built-up areas:

$$
\text{NDBI} = \frac{\rho_{SWIR} - \rho_{NIR}}{\rho_{SWIR} + \rho_{NIR}}
$$

For Sentinel-2: SWIR = Band 11 (1610 nm), NIR = Band 8 (842 nm).

Built-up areas typically show NDBI > 0, while vegetation shows NDBI < 0. However, bare soil also produces positive NDBI values—a significant limitation in arid Central Asian cities.

### Urban Index (UI)

UI uses the thermal band to distinguish built-up from bare soil:

$$
\text{UI} = \frac{\rho_{SWIR2} - \rho_{NIR}}{\rho_{SWIR2} + \rho_{NIR}}
$$

where SWIR2 is the longer wavelength SWIR band (Landsat Band 7, 2200 nm).

### Combined Built-up Index (BU)

The BU index combines NDBI with NDVI to suppress bare soil confusion:

$$
\text{BU} = \text{NDBI} - \text{NDVI}
$$

where:

$$
\text{NDVI} = \frac{\rho_{NIR} - \rho_{Red}}{\rho_{NIR} + \rho_{Red}}
$$

This subtraction reduces false positives from bare soil while preserving built-up detection.

<div class="info-box tip">
  <strong>Key idea</strong>
  No single index perfectly separates built-up areas from all other land covers. Combining indices—especially NDBI with NDVI—significantly improves urban mapping accuracy in arid regions.
</div>

## Urban expansion time-series analysis

### Detecting change over time

Urban expansion monitoring requires consistent, calibrated imagery across multiple dates. Key considerations:

1. **Radiometric consistency**: Use surface reflectance products, not raw DN values
2. **Seasonal timing**: Acquire imagery in similar seasons to minimize phenological effects
3. **Cloud-free composites**: Create annual composites from multiple acquisitions

### Change detection approaches

**Post-classification comparison** classifies each date independently, then identifies pixels that changed class:

$$
\text{Change}_{t1 \to t2} = \text{Class}_{t2} - \text{Class}_{t1}
$$

This is intuitive but propagates classification errors from both dates.

**Image differencing** computes spectral change directly:

$$
\Delta \text{Index} = \text{Index}_{t2} - \text{Index}_{t1}
$$

Pixels exceeding a threshold indicate change. This avoids classification errors but cannot identify change type without additional analysis.

**Trajectory analysis** examines full time-series behavior, fitting trends to multi-year index values and identifying pixels with sustained increases in built-up characteristics.

### Central Asian urban growth patterns

Analysis of Landsat archives reveals distinct expansion patterns:

| City | 2000 Built-up (km²) | 2020 Built-up (km²) | Annual Growth Rate |
|------|---------------------|---------------------|-------------------|
| Tashkent | 285 | 412 | 1.9% |
| Almaty | 198 | 312 | 2.3% |
| Bishkek | 127 | 189 | 2.0% |
| Dushanbe | 58 | 98 | 2.7% |
| Ashgabat | 95 | 168 | 2.9% |

Growth concentrates along transportation corridors and in peri-urban areas where agricultural land converts to residential use.

## Building footprint extraction

### From pixels to objects

Individual building extraction requires different approaches than urban area mapping. At 10m Sentinel-2 resolution, most buildings occupy 1-5 pixels—insufficient for footprint delineation.

High-resolution imagery (< 1m) enables building detection through:
- **Edge detection**: Buildings have sharp geometric edges
- **Shadow analysis**: Building heights create predictable shadows
- **Texture analysis**: Rooftops have distinct textural patterns
- **Shape analysis**: Buildings are typically rectangular

### Deep learning for building extraction

Convolutional neural networks achieve state-of-the-art building extraction. U-Net architecture with encoder-decoder structure and skip connections is particularly effective, enabling pixel-wise segmentation that delineates building footprints from high-resolution imagery.

### Training data sources

Building footprint training data for Central Asian cities:
- **OpenStreetMap**: Variable coverage, good in city centers
- **Microsoft Building Footprints**: AI-derived, available for some regions
- **Manual digitization**: Highest quality but labor-intensive

## Urban heat island mapping

### The physics of urban heating

Urban heat islands (UHI) occur because cities modify the surface energy balance:

$$
R_n = H + LE + G
$$

where $R_n$ is net radiation, $H$ is sensible heat flux, $LE$ is latent heat flux, and $G$ is ground heat flux.

Cities increase $H$ and decrease $LE$ compared to vegetated surroundings because:
- Impervious surfaces store more heat (higher thermal inertia)
- Less evapotranspiration (reduced $LE$)
- Building geometry traps radiation
- Anthropogenic heat release (vehicles, air conditioning)

### Land surface temperature retrieval

Thermal sensors measure at-sensor radiance $L_{sensor}$. Converting to land surface temperature (LST) requires atmospheric correction and emissivity estimation:

$$
L_{sensor} = \tau \cdot \varepsilon \cdot B(T_s) + L_{atm}^{\uparrow} + \tau(1-\varepsilon)L_{atm}^{\downarrow}
$$

where $\tau$ is atmospheric transmissivity, $\varepsilon$ is surface emissivity, $B(T_s)$ is Planck function at surface temperature, and $L_{atm}$ terms are atmospheric radiance contributions.

The single-channel algorithm estimates LST as:

$$
T_s = \frac{K_2}{\ln\left(\frac{K_1 \cdot \varepsilon}{L_{sensor}} + 1\right)}
$$

where $K_1$ and $K_2$ are sensor-specific calibration constants.

### Urban-rural temperature gradients

UHI intensity is typically measured as the temperature difference between urban and rural reference areas:

$$
\text{UHI intensity} = T_{urban} - T_{rural}
$$

For Central Asian cities, summer daytime UHI reaches:
- Tashkent: 4-8°C
- Almaty: 3-6°C  
- Bishkek: 2-5°C

<div class="info-box tip">
  <strong>Key idea</strong>
  Urban heat islands create public health risks during Central Asian summers when temperatures exceed 40°C. Thermal remote sensing enables systematic UHI mapping—identifying hotspots where vulnerable populations face heat stress.
</div>

## SAR for urban areas

### Double-bounce scattering

Synthetic Aperture Radar responds distinctively to urban structures. The dominant mechanism is double-bounce scattering, where radar waves reflect first from horizontal ground surfaces, then from vertical building walls (or vice versa), returning strongly to the sensor.

The double-bounce backscatter coefficient depends on building orientation relative to the radar look direction:

$$
\sigma^0_{double} \propto \cos^4(\theta - \phi)
$$

where $\theta$ is the radar incidence angle and $\phi$ is building orientation. Buildings aligned perpendicular to the radar look direction produce maximum return.

### Coherence for urban monitoring

InSAR coherence—the correlation between two SAR acquisitions—remains high over stable urban structures and low over vegetated or changing surfaces:

$$
\gamma = \frac{|\langle s_1 \cdot s_2^* \rangle|}{\sqrt{\langle|s_1|^2\rangle \langle|s_2|^2\rangle}}
$$

where $s_1$ and $s_2$ are complex SAR signals and $\langle \rangle$ denotes spatial averaging.

High coherence (γ > 0.6) typically indicates:
- Permanent structures (buildings, bridges)
- Paved surfaces
- Rocky terrain

Low coherence (γ < 0.3) indicates:
- Vegetation
- Water surfaces
- Construction activity (surface change)

### SAR-based urban mapping

SAR-based urban classification combines backscatter intensity with coherence. Urban areas exhibit high backscatter (strong double-bounce returns) combined with high temporal coherence (stable structures). A weighted combination of VV backscatter, VH backscatter, and coherence produces effective urban probability maps.

## Infrastructure monitoring

### Road network mapping

Roads create linear features detectable through multiple approaches:

**Spectral methods**: Asphalt and concrete have distinct spectral signatures, though similar to other impervious surfaces.

**Geometric methods**: Road detection algorithms exploit linearity using edge detection and Hough transforms to identify long, narrow, connected features.

**SAR methods**: Roads appear as dark linear features (specular reflection away from sensor) bordered by bright edges (double-bounce from roadside structures).

### Bridge and structure monitoring

Bridges are critical infrastructure requiring regular monitoring. InSAR techniques detect millimeter-scale deformation:

1. **Persistent Scatterer InSAR (PSI)**: Identifies stable reflectors on bridge structures
2. **Deformation time series**: Tracks structural movement over months/years
3. **Thermal dilation monitoring**: Correlates movement with temperature cycles

For Central Asian infrastructure, this enables:
- Dam stability monitoring (Nurek, Rogun)
- Bridge condition assessment
- Building settlement detection in seismically active zones

## Green space and urban vegetation

### Urban vegetation indices

Urban vegetation mapping uses standard indices with modified thresholds:

$$
\text{NDVI}_{urban} > 0.2 \text{ typically indicates vegetated pixels}
$$

Additional indices help distinguish vegetation types:

**Enhanced Vegetation Index (EVI)** reduces atmospheric and soil background effects:

$$
\text{EVI} = 2.5 \cdot \frac{\rho_{NIR} - \rho_{Red}}{\rho_{NIR} + 6\rho_{Red} - 7.5\rho_{Blue} + 1}
$$

**Soil Adjusted Vegetation Index (SAVI)** minimizes soil brightness influences:

$$
\text{SAVI} = \frac{\rho_{NIR} - \rho_{Red}}{\rho_{NIR} + \rho_{Red} + L} \cdot (1 + L)
$$

where $L = 0.5$ is the soil adjustment factor.

### Green space per capita analysis

Urban green space is a key sustainability indicator. Remote sensing enables systematic measurement by identifying vegetated pixels (NDVI > threshold), calculating total green area, and dividing by population. WHO recommends minimum 9 m² green space per capita. Many Central Asian urban areas fall below this threshold.

## Case studies: Central Asian capitals

### Tashkent, Uzbekistan

**Urban context**: Population 2.9 million, continental climate, extensive Soviet-era planning with recent rapid development.

**Key findings from remote sensing**:
- Urban expansion concentrated in southern and eastern sectors
- UHI intensity reaches 6-8°C in summer afternoons
- Green space declined 12% between 2010-2020 in central districts
- New construction visible in SAR coherence time series

**Analysis approach**: Landsat time series (1990-2024) combined with Sentinel-2 for recent high-resolution mapping. MODIS LST for thermal analysis.

### Almaty, Kazakhstan

**Urban context**: Population 2.0 million, foothills location with elevation gradient (600-1000m), seismic risk zone.

**Key findings**:
- Sprawl extends along mountain valleys
- Temperature inversions trap urban pollution (visible in thermal imagery)
- Building density highest in historical center
- SAR coherence enables building deformation monitoring for earthquake preparedness

**Analysis approach**: Multi-sensor fusion integrating optical, thermal, and SAR data. DEM incorporation for terrain-aware analysis.

### Bishkek, Kyrgyzstan

**Urban context**: Population 1.1 million, grid layout, rapid informal settlement growth on periphery.

**Key findings**:
- Informal settlements expand faster than planned development
- Irrigation canal network visible in high-resolution imagery
- UHI weaker than Tashkent due to lower density and elevation
- Soviet-era microrayon structures dominate SAR backscatter signature

**Analysis approach**: Change detection focused on peri-urban conversion of agricultural land. Building footprint extraction for settlement classification.

<div class="info-box tip">
  <strong>Key idea</strong>
  Each Central Asian city presents unique challenges. Tashkent's density creates strong UHI. Almaty's topography influences expansion patterns. Bishkek's informal settlements require high-resolution monitoring. Analysis approaches must adapt to local context.
</div>

## Practical workflow checklist

<ul class="checklist">
  <li>Define study area boundary and time period for analysis</li>
  <li>Acquire cloud-free optical imagery (Sentinel-2 or Landsat)</li>
  <li>Apply atmospheric correction to obtain surface reflectance</li>
  <li>Calculate built-up indices (NDBI, NDVI, BU)</li>
  <li>Acquire SAR data (Sentinel-1 GRD or SLC)</li>
  <li>Process SAR for backscatter and coherence</li>
  <li>Collect training data for urban/non-urban classes</li>
  <li>Train classifier and validate with independent samples</li>
  <li>Generate urban extent maps for multiple dates</li>
  <li>Perform change detection to identify expansion</li>
  <li>Acquire thermal data for UHI analysis</li>
  <li>Retrieve land surface temperature</li>
  <li>Calculate UHI intensity relative to rural reference</li>
  <li>Extract vegetation indices for green space mapping</li>
  <li>Integrate with population data for per-capita metrics</li>
  <li>Validate results with ground truth or high-resolution imagery</li>
  <li>Document methodology and accuracy assessment</li>
  <li>Create visualizations and maps for stakeholders</li>
</ul>

## Exercises

**Exercise 1**: Calculate NDBI for a Sentinel-2 image. If Band 8 (NIR) = 0.25 and Band 11 (SWIR) = 0.32, what is NDBI?

<details>
<summary>Show Solution</summary>
NDBI = (SWIR - NIR) / (SWIR + NIR) = (0.32 - 0.25) / (0.32 + 0.25) = 0.07 / 0.57 = 0.123. This positive value suggests built-up or bare soil.
</details>

**Exercise 2**: What BU value would you expect for dense vegetation with NDVI = 0.7 and NDBI = -0.2?

<details>
<summary>Show Solution</summary>
BU = NDBI - NDVI = -0.2 - 0.7 = -0.9. Strongly negative BU indicates vegetation, not built-up area.
</details>

**Exercise 3**: A pixel has NDVI = 0.1 and NDBI = 0.15. Is it more likely built-up or bare soil? What additional data would help?

<details>
<summary>Show Solution</summary>
Both values are consistent with either built-up or bare soil. Thermal data (built-up is warmer), SAR coherence (built-up has high coherence), or texture analysis would help distinguish them.
</details>

**Exercise 4**: Calculate the annual urban growth rate if a city expanded from 150 km² to 210 km² over 15 years.

<details>
<summary>Show Solution</summary>
Using compound growth: Rate = (210/150)^(1/15) - 1 = (1.4)^0.0667 - 1 = 0.0226 = 2.26% annual growth rate.
</details>

**Exercise 5**: Why might NDBI produce false positives in Almaty compared to European cities?

<details>
<summary>Show Solution</summary>
Central Asia has extensive bare soil and sparse vegetation surrounding cities. Bare soil has similar SWIR-NIR spectral characteristics to built-up surfaces, causing false positives. European cities typically have vegetated surroundings.
</details>

**Exercise 6**: An urban area has LST = 312 K. Nearby rural fields have LST = 304 K. What is the UHI intensity in °C?

<details>
<summary>Show Solution</summary>
UHI intensity = 312 - 304 = 8 K = 8°C. This is a strong UHI effect typical of Central Asian cities in summer.
</details>

**Exercise 7**: Explain why SAR coherence is useful for distinguishing construction sites from completed buildings.

<details>
<summary>Show Solution</summary>
Construction sites change between SAR acquisitions (earthmoving, new structures), producing low coherence. Completed buildings remain stable, maintaining high coherence (γ > 0.6).
</details>

**Exercise 8**: A Sentinel-1 image shows a bright linear feature with dark center. What urban infrastructure might this represent?

<details>
<summary>Show Solution</summary>
This pattern suggests a road or canal: the dark center is specular reflection away from sensor (smooth surface), bright edges are double-bounce from adjacent structures or embankments.
</details>

**Exercise 9**: Calculate emissivity for an urban pixel that is 60% concrete (ε=0.92) and 40% vegetation (ε=0.98).

<details>
<summary>Show Solution</summary>
Mixed emissivity = 0.60 × 0.92 + 0.40 × 0.98 = 0.552 + 0.392 = 0.944. The pixel emissivity is weighted by fractional cover.
</details>

**Exercise 10**: Why do buildings aligned perpendicular to SAR look direction appear brightest?

<details>
<summary>Show Solution</summary>
Double-bounce scattering is maximized when building walls are perpendicular to radar look direction. The ground-wall-sensor path efficiently returns energy. Aligned or oblique buildings scatter energy away from the sensor.
</details>

**Exercise 11**: A city has 850,000 residents and 7.2 km² of green space. Does it meet WHO recommendations?

<details>
<summary>Show Solution</summary>
Green space per capita = 7,200,000 m² / 850,000 = 8.47 m² per person. This falls below the WHO recommendation of 9 m² per capita.
</details>

**Exercise 12**: Why is seasonal timing important when comparing urban imagery from different years?

<details>
<summary>Show Solution</summary>
Vegetation phenology changes seasonally. Comparing summer and winter images would confuse vegetation change with urban expansion. Consistent seasonal timing isolates actual land cover change.
</details>

**Exercise 13**: A thermal image shows a cool linear feature crossing a hot urban area. What might this be?

<details>
<summary>Show Solution</summary>
Likely a river, canal, or tree-lined street. Water and vegetation have lower LST than surrounding impervious surfaces due to evaporative cooling.
</details>

**Exercise 14**: Calculate SAVI for a pixel with NIR = 0.35, Red = 0.08, using L = 0.5.

<details>
<summary>Show Solution</summary>
SAVI = [(0.35 - 0.08) / (0.35 + 0.08 + 0.5)] × 1.5 = (0.27 / 0.93) × 1.5 = 0.29 × 1.5 = 0.435. This indicates moderate vegetation.
</details>

**Exercise 15**: Why might building extraction models trained on European cities perform poorly in Central Asian cities?

<details>
<summary>Show Solution</summary>
Architectural differences: Central Asian buildings have different roof materials (clay tiles, flat roofs), courtyard layouts, and construction styles. Training data must represent local building characteristics.
</details>

**Exercise 16**: An analyst detects 15% urban expansion but ground validation shows only 10%. What might cause this overestimate?

<details>
<summary>Show Solution</summary>
Possible causes: bare soil misclassified as built-up, greenhouse/polytunnel structures, temporary construction areas, or seasonal surface moisture changes affecting spectral signatures.
</details>

**Exercise 17**: How would you use multi-temporal SAR to detect a new building constructed in 2023?

<details>
<summary>Show Solution</summary>
Compare coherence maps before and after construction. The new building location shows low coherence during construction (change), then high coherence after completion (stable structure). Backscatter intensity also increases.
</details>

**Exercise 18**: Tashkent's UHI is 7°C at 14:00 local time. Would you expect it higher or lower at 22:00? Why?

<details>
<summary>Show Solution</summary>
Typically higher at night. Urban materials (concrete, asphalt) have high thermal inertia—they release stored heat slowly. Rural areas cool faster. Nighttime UHI often exceeds daytime UHI.
</details>

**Exercise 19**: Calculate the total impervious surface area for a city where classification shows 4,200 urban pixels at 10m resolution.

<details>
<summary>Show Solution</summary>
Area = 4,200 pixels × (10m × 10m) = 4,200 × 100 m² = 420,000 m² = 0.42 km² of impervious surface.
</details>

**Exercise 20**: Why might VH polarization be more useful than VV for distinguishing urban vegetation from buildings?

<details>
<summary>Show Solution</summary>
VH polarization is sensitive to volume scattering in vegetation canopies. Buildings produce primarily surface and double-bounce scattering in VV. The VH/VV ratio helps separate these mechanisms.
</details>

**Exercise 21**: An infrared image shows metal rooftops. Would they appear hot or cold compared to surrounding concrete? Why?

<details>
<summary>Show Solution</summary>
Metal roofs appear cool (low radiance) because they have low emissivity (0.1-0.3). They may actually be hot but emit less thermal radiation. This is a common source of LST retrieval errors.
</details>

**Exercise 22**: Design a change detection approach to identify informal settlements appearing on agricultural land.

<details>
<summary>Show Solution</summary>
(1) Create annual NDVI composites to identify vegetated agricultural areas, (2) Calculate BU index for each year, (3) Flag pixels where BU increases significantly while NDVI decreases, (4) Apply minimum patch size filter to remove noise, (5) Validate with high-resolution imagery.
</details>

**Exercise 23**: Why is 12-day Sentinel-1 revisit time better than 24-day for urban coherence analysis?

<details>
<summary>Show Solution</summary>
Shorter temporal baselines maintain higher coherence. Over 24 days, more surface changes occur (vegetation growth, construction activity), decorrelating the signal. 12-day pairs preserve coherence for change detection.
</details>

**Exercise 24**: A park shows NDVI = 0.6 in May and NDVI = 0.3 in August. Is this urban change or natural variation?

<details>
<summary>Show Solution</summary>
Likely natural seasonal variation. Central Asian summers are hot and dry—vegetation stress reduces NDVI. True urban change would show consistent low NDVI across seasons, not seasonal fluctuation.
</details>

**Exercise 25**: Calculate EVI for: Blue = 0.05, Red = 0.06, NIR = 0.45.

<details>
<summary>Show Solution</summary>
EVI = 2.5 × (0.45 - 0.06) / (0.45 + 6×0.06 - 7.5×0.05 + 1) = 2.5 × 0.39 / (0.45 + 0.36 - 0.375 + 1) = 2.5 × 0.39 / 1.435 = 0.679. This indicates healthy vegetation.
</details>

**Exercise 26**: How would dam construction appear in a SAR coherence time series?

<details>
<summary>Show Solution</summary>
Initially high coherence (natural terrain), then low coherence during construction (surface disturbance), returning to high coherence after completion (stable concrete structure). Water behind dam shows persistently low coherence.
</details>

**Exercise 27**: Why do Soviet-era microrayon blocks produce distinctive SAR signatures?

<details>
<summary>Show Solution</summary>
Standardized building orientations, regular spacing, and similar construction materials create repeating backscatter patterns. Building alignment relative to radar look direction produces predictable bright/dark patterns.
</details>

**Exercise 28**: Calculate population density if 45,000 people live in an area classified as 3.8 km² urban extent.

<details>
<summary>Show Solution</summary>
Population density = 45,000 / 3.8 = 11,842 people per km². This is moderate density typical of Central Asian residential areas.
</details>

**Exercise 29**: An analyst wants to map building heights from single optical image. What limitations exist?

<details>
<summary>Show Solution</summary>
Shadow length indicates height but requires known sun angle and flat terrain. Occlusion prevents seeing all buildings. Stereo imagery or LiDAR provides more reliable height estimates than monocular analysis.
</details>

**Exercise 30**: Design a comprehensive urban monitoring system integrating optical, thermal, and SAR data for Tashkent.

<details>
<summary>Show Solution</summary>
System components: (1) Monthly Sentinel-2 composites for land cover classification and green space monitoring, (2) Summer Landsat-8/9 thermal acquisitions for UHI mapping, (3) Sentinel-1 12-day coherence stacks for construction monitoring and building stability, (4) Annual change detection for expansion tracking, (5) Integration with census data for socioeconomic analysis. Processing in Google Earth Engine for scalability.
</details>

## Summary

Urban remote sensing enables systematic monitoring of Central Asian cities during rapid transformation. Key techniques include:

- **Spectral indices** (NDBI, BU) identify built-up areas but require bare soil correction
- **Time-series analysis** quantifies expansion rates and patterns
- **Thermal imaging** reveals urban heat islands affecting public health
- **SAR coherence** distinguishes stable structures from active construction
- **Building extraction** requires high-resolution data and deep learning approaches
- **Green space mapping** supports urban sustainability assessment

The combination of optical, thermal, and SAR data provides comprehensive urban intelligence—essential for planning sustainable cities across Central Asia.
