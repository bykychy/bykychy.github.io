---
layout: tutorial
title: "How Thermal Infrared Remote Sensing Reveals Temperature Patterns in Central Asia"
description: "A math-first tutorial on thermal radiation, blackbody physics, land surface temperature, emissivity, and thermal anomaly detection for Central Asia, with 30 exercises and solutions."
difficulty: intermediate
estimated_time: "2-3 hours"
series: sar-remote-sensing
order: 8
---

## Why this tutorial matters

Optical sensors measure reflected sunlight—they reveal what surfaces look like. SAR measures geometric structure and moisture through microwave backscatter. But neither directly measures temperature. Thermal infrared remote sensing fills this critical gap: it captures the energy that surfaces emit because they are warm.

For Central Asia, thermal imaging is indispensable. The region experiences extreme continental climate with summer temperatures exceeding 45°C in lowland deserts and winter temperatures dropping below -30°C in mountain valleys. Urban heat islands in cities like Tashkent and Almaty pose public health risks. Geothermal areas in Kyrgyzstan and Tajikistan indicate subsurface energy resources. Agricultural water stress manifests as elevated canopy temperatures days before visible wilting.

<div class="info-box tip">
  <strong>Key idea</strong>
  Thermal infrared measures what surfaces emit, not what they reflect. This reveals temperature—a fundamental physical property invisible to optical and SAR sensors. Temperature patterns indicate energy exchange, moisture status, and subsurface processes.
</div>

## Thermal radiation physics

### Blackbody radiation and Planck's law

All objects with temperature above absolute zero emit electromagnetic radiation. A perfect emitter—a blackbody—emits radiation according to Planck's law:

$$
B_\lambda(T) = \frac{2hc^2}{\lambda^5} \cdot \frac{1}{e^{hc/\lambda k_B T} - 1}
$$

where $B_\lambda$ is spectral radiance (W·m⁻²·sr⁻¹·μm⁻¹), $\lambda$ is wavelength, $T$ is absolute temperature (K), $h = 6.626 \times 10^{-34}$ J·s is Planck's constant, $c = 3 \times 10^8$ m/s is the speed of light, and $k_B = 1.381 \times 10^{-23}$ J/K is Boltzmann's constant.

For practical calculations, we often use simplified constants. At thermal infrared wavelengths (8-14 μm):

$$
B_\lambda(T) = \frac{C_1}{\lambda^5 (e^{C_2/\lambda T} - 1)}
$$

where $C_1 = 1.191 \times 10^8$ W·μm⁴·m⁻²·sr⁻¹ and $C_2 = 14388$ μm·K.

### Wien's displacement law

The wavelength of peak emission shifts with temperature:

$$
\lambda_{max} = \frac{b}{T}
$$

where $b = 2898$ μm·K. At 300 K (27°C), Earth surfaces emit most strongly near 9.7 μm—the thermal infrared region.

### Stefan-Boltzmann law

Total radiant power per unit area integrates over all wavelengths:

$$
M = \sigma T^4
$$

where $\sigma = 5.67 \times 10^{-8}$ W·m⁻²·K⁻⁴. This fourth-power relationship means small temperature differences produce measurable radiance differences.

<div class="info-box tip">
  <strong>Key idea</strong>
  The $T^4$ relationship amplifies thermal contrasts. A 10 K temperature difference between adjacent surfaces produces a radiance difference detectable from space—the physical basis for thermal remote sensing.
</div>

## Emissivity concepts

### Real surfaces versus blackbodies

Real surfaces are not perfect blackbodies. Emissivity $\varepsilon$ describes how efficiently a surface emits compared to a blackbody at the same temperature:

$$
L_\lambda = \varepsilon_\lambda \cdot B_\lambda(T)
$$

where $L_\lambda$ is the actual spectral radiance and $0 \leq \varepsilon \leq 1$.

Emissivity varies with:
- **Material composition**: Metals have low emissivity (0.1-0.3); vegetation and water have high emissivity (0.95-0.99)
- **Surface roughness**: Rough surfaces emit more efficiently than smooth ones
- **Wavelength**: Emissivity varies across the thermal spectrum
- **Viewing angle**: Emissivity typically decreases at oblique angles

### Kirchhoff's law

For opaque materials at thermal equilibrium, emissivity equals absorptivity:

$$
\varepsilon_\lambda = \alpha_\lambda = 1 - \rho_\lambda
$$

where $\rho_\lambda$ is reflectivity. Materials that reflect thermal radiation poorly absorb and emit it efficiently.

### Common emissivity values for Central Asia surfaces

| Surface Type | Emissivity (10.5-12.5 μm) |
|-------------|---------------------------|
| Water bodies | 0.98-0.99 |
| Dense vegetation | 0.96-0.98 |
| Agricultural crops | 0.95-0.97 |
| Bare soil (wet) | 0.92-0.96 |
| Bare soil (dry) | 0.88-0.94 |
| Sand/desert | 0.84-0.92 |
| Concrete/urban | 0.90-0.94 |
| Asphalt | 0.93-0.96 |
| Rock outcrops | 0.85-0.93 |

## Satellite thermal sensors

### Landsat 8/9 TIRS

The Thermal Infrared Sensor provides two bands:
- **Band 10**: 10.6-11.2 μm (100 m resolution, resampled to 30 m)
- **Band 11**: 11.5-12.5 μm (100 m resolution, resampled to 30 m)

The split-window capability enables atmospheric correction. Conversion from digital number to radiance:

$$
L_\lambda = M_L \cdot Q_{cal} + A_L
$$

where $M_L$ is the multiplicative scaling factor, $A_L$ is the additive offset, and $Q_{cal}$ is the quantized calibrated pixel value.

Conversion to brightness temperature:

$$
T_B = \frac{K_2}{\ln(K_1/L_\lambda + 1)}
$$

where $K_1 = 774.89$ W·m⁻²·sr⁻¹·μm⁻¹ and $K_2 = 1321.08$ K for Band 10.

### MODIS

The Moderate Resolution Imaging Spectroradiometer provides:
- **Band 31**: 10.78-11.28 μm (1 km resolution)
- **Band 32**: 11.77-12.27 μm (1 km resolution)

MODIS offers daily coverage—essential for monitoring temporal dynamics. The MOD11 product provides Land Surface Temperature at 1 km resolution.

### ASTER

The Advanced Spaceborne Thermal Emission and Reflection Radiometer provides five thermal bands (8.125-11.65 μm) at 90 m resolution—the highest spatial resolution among standard thermal sensors. Multi-band capability enables Temperature-Emissivity Separation.

### Sentinel-3 SLSTR

The Sea and Land Surface Temperature Radiometer provides:
- **S7**: 3.74 μm (mid-infrared)
- **S8**: 10.85 μm
- **S9**: 12.02 μm

At 1 km resolution with daily revisit, SLSTR complements Landsat for operational monitoring.

## Land Surface Temperature retrieval

### Single-channel method

When only one thermal band is available, LST retrieval requires atmospheric parameters:

$$
T_s = \frac{1}{\varepsilon} \left[ \frac{T_B - L_{atm}^\uparrow - \tau(1-\varepsilon)L_{atm}^\downarrow}{\tau} \right]
$$

where $\tau$ is atmospheric transmissivity, $L_{atm}^\uparrow$ is upwelling atmospheric radiance, and $L_{atm}^\downarrow$ is downwelling atmospheric radiance.

A simplified approach using water vapor content $w$ (g/cm²):

$$
T_s = \gamma \left[ \frac{1}{\varepsilon}(\psi_1 L_\lambda + \psi_2) + \psi_3 \right] + \delta
$$

where $\gamma$ and $\delta$ are Planck function parameters, and $\psi$ coefficients depend on atmospheric conditions.

### Split-window algorithm

The split-window method uses two thermal bands to correct for atmospheric effects:

$$
T_s = T_{B,i} + C_1(T_{B,i} - T_{B,j}) + C_2(T_{B,i} - T_{B,j})^2 + C_0 + (C_3 + C_4 w)(1 - \varepsilon) + (C_5 + C_6 w)\Delta\varepsilon
$$

where subscripts $i$ and $j$ denote the two thermal bands, $\varepsilon = (\varepsilon_i + \varepsilon_j)/2$ is mean emissivity, $\Delta\varepsilon = \varepsilon_i - \varepsilon_j$ is emissivity difference, and $C$ coefficients are empirically derived.

<div class="info-box tip">
  <strong>Key idea</strong>
  Split-window algorithms exploit differential atmospheric absorption between adjacent thermal bands. The atmosphere affects each band differently, enabling separation of surface temperature from atmospheric effects.
</div>

### Temperature-Emissivity Separation (TES)

With multiple thermal bands (like ASTER's five), temperature and emissivity can be separated simultaneously. The TES algorithm iterates:

1. Assume initial emissivity spectrum
2. Calculate temperature from radiance
3. Refine emissivity using empirical relationships
4. Iterate until convergence

The Normalized Emissivity Method uses:

$$
\beta = \frac{\varepsilon_\lambda}{\bar{\varepsilon}}
$$

where $\bar{\varepsilon}$ is the average emissivity across bands.

## Atmospheric correction

### Atmospheric effects on thermal radiation

The atmosphere affects thermal measurements through:

$$
L_{sensor} = \tau \varepsilon B(T_s) + L_{atm}^\uparrow + \tau(1-\varepsilon)L_{atm}^\downarrow
$$

The first term is the surface emission attenuated by the atmosphere. The second is upwelling atmospheric emission. The third is downwelling atmospheric radiance reflected by the surface.

### Water vapor absorption

Water vapor is the dominant absorber in the thermal infrared. The 8-14 μm atmospheric window has variable transmissivity:

$$
\tau \approx e^{-k \cdot w / \cos\theta}
$$

where $k$ is an absorption coefficient, $w$ is columnar water vapor, and $\theta$ is view zenith angle.

### Correction approaches

**Radiative transfer models** (MODTRAN, 6S) calculate atmospheric parameters from:
- Atmospheric profiles (temperature, humidity vs. altitude)
- Aerosol optical depth
- Surface elevation
- Sensor and sun geometry

**Empirical methods** use nearby water bodies as reference points, assuming water temperature is known or uniform.

## Central Asia applications

### Urban heat islands

Cities absorb and store solar energy in buildings, roads, and infrastructure. The Urban Heat Island (UHI) intensity:

$$
\Delta T_{UHI} = T_{urban} - T_{rural}
$$

In Tashkent, summer UHI intensity reaches 6-8°C. Thermal imagery identifies hotspots—industrial zones, dense commercial areas, poorly vegetated neighborhoods—guiding urban planning and heat mitigation.

The Surface Urban Heat Island (SUHI) differs from air temperature UHI measured by weather stations. LST captures radiative surface temperature, which can exceed air temperature by 10-20°C for sun-heated surfaces.

### Geothermal exploration

Central Asia contains geothermal resources, particularly along tectonic zones in Kyrgyzstan and Tajikistan. Thermal anomalies indicate subsurface heat:

$$
\Delta T_{anomaly} = T_{observed} - T_{background}
$$

Night-time imagery is preferable—solar heating masks geothermal signals during daytime. Persistent anomalies across seasons suggest genuine geothermal sources rather than surface effects.

### Agricultural water stress

Water-stressed crops close stomata, reducing transpiration and raising leaf temperature. The Crop Water Stress Index:

$$
CWSI = \frac{(T_c - T_a) - (T_c - T_a)_{ll}}{(T_c - T_a)_{ul} - (T_c - T_a)_{ll}}
$$

where $T_c$ is canopy temperature, $T_a$ is air temperature, and subscripts $ll$ and $ul$ denote lower and upper limits under well-watered and non-transpiring conditions.

In the Fergana Valley, thermal monitoring detects irrigation deficits before visible wilting, enabling proactive water management.

### Archaeological prospection

Buried structures have different thermal properties than surrounding soil. Subsurface walls retain heat differently, creating thermal contrasts:

$$
\Delta T \propto \frac{\partial T}{\partial t} \cdot \left(\frac{1}{\alpha_{structure}} - \frac{1}{\alpha_{soil}}\right)
$$

where $\alpha$ is thermal diffusivity. Pre-dawn imaging maximizes contrasts when surface temperature gradients are steepest.

Ancient irrigation systems (karez/qanat) in Uzbekistan and Turkmenistan appear as linear thermal anomalies—cooler than surroundings due to subsurface water.

## Diurnal and seasonal patterns

### Thermal inertia

Thermal inertia describes resistance to temperature change:

$$
P = \sqrt{k \rho c}
$$

where $k$ is thermal conductivity, $\rho$ is density, and $c$ is specific heat capacity. High thermal inertia materials (water, wet soil) change temperature slowly; low thermal inertia materials (dry sand, rock) change rapidly.

The diurnal temperature range relates to thermal inertia:

$$
\Delta T_{diurnal} \propto \frac{1}{P}
$$

### Day-night temperature differences

Apparent Thermal Inertia (ATI) uses day-night pairs:

$$
ATI = \frac{1 - \alpha}{T_{day} - T_{night}}
$$

where $\alpha$ is surface albedo. ATI correlates with soil moisture and material properties—useful for soil mapping and moisture estimation in Central Asia's arid landscapes.

### Seasonal patterns

Summer thermal imagery captures peak heating and evaporative demand. Winter imagery reveals persistent thermal sources (geothermal, industrial) against cold backgrounds. Seasonal analysis distinguishes permanent features from transient phenomena.

<div class="info-box tip">
  <strong>Key idea</strong>
  Time matters in thermal sensing. Day imagery shows solar heating patterns; night imagery reveals thermal inertia and subsurface influences. Multi-temporal analysis separates surface effects from persistent thermal sources.
</div>

## Integration with optical and SAR data

### Thermal-optical fusion

Combining thermal with optical data enables:

$$
ET = 1.26 \cdot \frac{\Delta}{\Delta + \gamma} \cdot \frac{(R_n - G)}{L_v} \cdot \left(\frac{T_{cold} - T_s}{T_{cold} - T_{hot}}\right)
$$

This SEBAL-type equation estimates evapotranspiration using thermal temperature $T_s$ with optical-derived net radiation $R_n$ and cold/hot reference pixels.

NDVI from optical data estimates emissivity for LST retrieval:

$$
\varepsilon = 0.004 \cdot P_v + 0.986
$$

where $P_v$ is vegetation proportion derived from NDVI.

### Thermal-SAR fusion

SAR provides moisture information independent of thermal state. Combining:
- **Thermal LST** indicates surface energy balance
- **SAR backscatter** indicates soil moisture and roughness
- **SAR interferometry** indicates surface deformation

Joint analysis detects irrigation patterns (thermal cooling + SAR moisture), permafrost dynamics (thermal + deformation), and infrastructure monitoring.

## Practical workflow

### Step 1: Data acquisition

Download Landsat Collection 2 Level-2 products, which include surface temperature. For MODIS, use MOD11A1 (daily) or MOD11A2 (8-day composite) products.

### Step 2: Quality assessment

Check quality flags for clouds, cloud shadows, and fill values. Thermal bands are highly sensitive to clouds—even thin cirrus contaminates measurements.

### Step 3: Unit conversion

Convert to physical units. For Landsat Collection 2 Surface Temperature:

$$
T_s = ST_{B10} \cdot 0.00341802 + 149.0 - 273.15
$$

This converts scaled DN to Celsius.

### Step 4: Emissivity correction

Apply land cover-specific emissivity. Use NDVI-based methods for vegetated areas:

$$
\varepsilon = \begin{cases}
0.973 - 0.047 \cdot \rho_{red} & \text{if NDVI} < 0.2 \\
0.004 \cdot P_v + 0.986 & \text{if } 0.2 \leq \text{NDVI} \leq 0.5 \\
0.99 & \text{if NDVI} > 0.5
\end{cases}
$$

### Step 5: Analysis

Calculate statistics, detect anomalies, and extract temporal patterns. Compare with reference data and validate against ground measurements where available.

## Checklist for thermal analysis

<ul class="checklist">
  <li>Identified appropriate thermal sensor for spatial and temporal requirements</li>
  <li>Downloaded thermal data with quality flags and metadata</li>
  <li>Screened for clouds and atmospheric contamination</li>
  <li>Converted digital numbers to brightness temperature</li>
  <li>Applied atmospheric correction (split-window or radiative transfer)</li>
  <li>Estimated surface emissivity from land cover or spectral methods</li>
  <li>Calculated Land Surface Temperature with uncertainty</li>
  <li>Considered acquisition time (day vs. night, season)</li>
  <li>Validated against ground measurements or reference surfaces</li>
  <li>Documented assumptions and limitations</li>
</ul>

## Exercises

<div class="exercises">

<h3>Exercise 1</h3>
<p>Using Wien's displacement law, calculate the wavelength of peak emission for a surface at 25°C. Express your answer in micrometers.</p>
<details>
<summary>Solution</summary>
Convert temperature to Kelvin: $T = 25 + 273.15 = 298.15$ K.

Apply Wien's law:

$$\lambda_{max} = \frac{2898}{298.15} = 9.72\ \mu m$$

Peak emission occurs at 9.72 μm, within the 8-14 μm atmospheric window measured by thermal sensors.
</details>

<h3>Exercise 2</h3>
<p>Calculate the total radiant exitance for surfaces at 20°C and 40°C using the Stefan-Boltzmann law. What is the percentage increase in emission?</p>
<details>
<summary>Solution</summary>
At 20°C (293.15 K): $M_1 = 5.67 \times 10^{-8} \times (293.15)^4 = 418.8$ W/m²

At 40°C (313.15 K): $M_2 = 5.67 \times 10^{-8} \times (313.15)^4 = 545.0$ W/m²

Percentage increase: $\frac{545.0 - 418.8}{418.8} \times 100 = 30.1\%$

A 20°C temperature increase produces a 30% increase in radiant emission.
</details>

<h3>Exercise 3</h3>
<p>A thermal sensor measures radiance of 9.5 W·m⁻²·sr⁻¹·μm⁻¹ at 11 μm. If the surface emissivity is 0.96, what is the blackbody-equivalent radiance?</p>
<details>
<summary>Solution</summary>
The measured radiance is: $L_\lambda = \varepsilon \cdot B_\lambda(T)$

Solving for blackbody radiance:

$$B_\lambda(T) = \frac{L_\lambda}{\varepsilon} = \frac{9.5}{0.96} = 9.90\ \text{W·m}^{-2}\text{·sr}^{-1}\text{·μm}^{-1}$$

The equivalent blackbody radiance is 9.90 W·m⁻²·sr⁻¹·μm⁻¹.
</details>

<h3>Exercise 4</h3>
<p>Using Landsat 8 TIRS calibration constants ($K_1 = 774.89$, $K_2 = 1321.08$), calculate brightness temperature for a radiance of 8.2 W·m⁻²·sr⁻¹·μm⁻¹.</p>
<details>
<summary>Solution</summary>
Apply the inverse Planck equation:

$$T_B = \frac{K_2}{\ln(K_1/L_\lambda + 1)} = \frac{1321.08}{\ln(774.89/8.2 + 1)}$$

$$T_B = \frac{1321.08}{\ln(95.47)} = \frac{1321.08}{4.559} = 289.8\ K$$

Converting to Celsius: $T_B = 289.8 - 273.15 = 16.6°C$
</details>

<h3>Exercise 5</h3>
<p>Dry sand has emissivity 0.90 and wet sand has emissivity 0.95. If both are at 35°C, what brightness temperature would a sensor measure for each (assuming no atmosphere)?</p>
<details>
<summary>Solution</summary>
For a surface with emissivity $\varepsilon$ at temperature $T$:

Dry sand: $T_{B,dry} = T \cdot \varepsilon^{0.25} = 308.15 \times 0.90^{0.25} = 308.15 \times 0.974 = 300.1$ K = 26.9°C

Wet sand: $T_{B,wet} = 308.15 \times 0.95^{0.25} = 308.15 \times 0.987 = 304.2$ K = 31.0°C

The sensor would measure 26.9°C for dry sand and 31.0°C for wet sand—a 4.1°C apparent difference despite identical actual temperatures.
</details>

<h3>Exercise 6</h3>
<p>Calculate the thermal inertia for soil with thermal conductivity 1.5 W·m⁻¹·K⁻¹, density 1600 kg/m³, and specific heat 850 J·kg⁻¹·K⁻¹.</p>
<details>
<summary>Solution</summary>
Thermal inertia:

$$P = \sqrt{k \rho c} = \sqrt{1.5 \times 1600 \times 850}$$

$$P = \sqrt{2,040,000} = 1429\ \text{J·m}^{-2}\text{·K}^{-1}\text{·s}^{-0.5}$$

This is a moderate thermal inertia typical of moist agricultural soil.
</details>

<h3>Exercise 7</h3>
<p>An urban area shows daytime LST of 42°C and nighttime LST of 24°C. Adjacent rural areas show 35°C and 22°C. Calculate daytime and nighttime UHI intensity.</p>
<details>
<summary>Solution</summary>
Daytime UHI intensity:
$$\Delta T_{UHI,day} = 42 - 35 = 7°C$$

Nighttime UHI intensity:
$$\Delta T_{UHI,night} = 24 - 22 = 2°C$$

The urban area is 7°C warmer during daytime but only 2°C warmer at night, indicating that solar heating amplifies the UHI effect.
</details>

<h3>Exercise 8</h3>
<p>Atmospheric transmissivity is 0.85 and upwelling atmospheric radiance is 1.2 W·m⁻²·sr⁻¹·μm⁻¹. If sensor-measured radiance is 8.0 W·m⁻²·sr⁻¹·μm⁻¹, what is the surface-leaving radiance?</p>
<details>
<summary>Solution</summary>
Simplified atmospheric equation (ignoring reflected downwelling):

$$L_{sensor} = \tau L_{surface} + L_{atm}^\uparrow$$

Solving for surface radiance:

$$L_{surface} = \frac{L_{sensor} - L_{atm}^\uparrow}{\tau} = \frac{8.0 - 1.2}{0.85} = \frac{6.8}{0.85} = 8.0\ \text{W·m}^{-2}\text{·sr}^{-1}\text{·μm}^{-1}$$
</details>

<h3>Exercise 9</h3>
<p>A geothermal area has background temperature of 18°C. A thermal anomaly shows 26°C. If the anomaly covers 4 pixels at 100 m resolution, calculate the anomaly area and temperature excess.</p>
<details>
<summary>Solution</summary>
Temperature excess: $\Delta T = 26 - 18 = 8°C$

Pixel area: $100 \times 100 = 10,000$ m² = 1 hectare

Anomaly area: $4 \times 10,000 = 40,000$ m² = 4 hectares

The geothermal anomaly covers 4 hectares with 8°C excess temperature.
</details>

<h3>Exercise 10</h3>
<p>Using NDVI-based emissivity estimation, calculate emissivity for pixels with NDVI values of 0.1, 0.35, and 0.7.</p>
<details>
<summary>Solution</summary>
For NDVI = 0.1 (< 0.2): $\varepsilon = 0.973 - 0.047 \times 0.1 = 0.968$

For NDVI = 0.35 (between 0.2 and 0.5):
$P_v = [(0.35 - 0.2)/(0.5 - 0.2)]^2 = (0.5)^2 = 0.25$
$\varepsilon = 0.004 \times 0.25 + 0.986 = 0.987$

For NDVI = 0.7 (> 0.5): $\varepsilon = 0.99$

Higher vegetation cover corresponds to higher emissivity.
</details>

<h3>Exercise 11</h3>
<p>Calculate Apparent Thermal Inertia (ATI) for a surface with albedo 0.25, daytime temperature 38°C, and nighttime temperature 22°C.</p>
<details>
<summary>Solution</summary>
$$ATI = \frac{1 - \alpha}{T_{day} - T_{night}} = \frac{1 - 0.25}{38 - 22} = \frac{0.75}{16} = 0.047°C^{-1}$$

Low ATI indicates low thermal inertia—likely dry, low-density material.
</details>

<h3>Exercise 12</h3>
<p>A cotton field shows canopy temperature of 32°C when air temperature is 35°C. The well-watered baseline is -2°C and stressed baseline is +5°C. Calculate CWSI.</p>
<details>
<summary>Solution</summary>
$$T_c - T_a = 32 - 35 = -3°C$$

$$CWSI = \frac{(T_c - T_a) - (T_c - T_a)_{ll}}{(T_c - T_a)_{ul} - (T_c - T_a)_{ll}}$$

$$CWSI = \frac{-3 - (-2)}{5 - (-2)} = \frac{-1}{7} = -0.14$$

Negative CWSI indicates the crop is transpiring at maximum rate—well-watered conditions.
</details>

<h3>Exercise 13</h3>
<p>Landsat has ±1.5 K uncertainty. What percentage error at 300 K?</p>
<details>
<summary>Solution</summary>
$(1.5/300) \times 100 = 0.50\%$
</details>

<h3>Exercise 14</h3>
<p>Two bands: 305 K and 303 K. Split-window coefficient 2.5. Estimate surface temperature.</p>
<details>
<summary>Solution</summary>
$T_s = 305 + 2.5 \times (305 - 303) = 310$ K
</details>

<h3>Exercise 15</h3>
<p>A lake (20°C) covers 0.4 of a pixel; land (35°C) covers 0.6. What pixel temperature results?</p>
<details>
<summary>Solution</summary>
$T_{pixel} = 0.4 \times 20 + 0.6 \times 35 = 29°C$
</details>

<h3>Exercise 16</h3>
<p>For a 500 m² geothermal feature, is MODIS (1 km) or Landsat (100 m) better?</p>
<details>
<summary>Solution</summary>
Feature: ~22×22 m. MODIS: 0.05% of pixel—undetectable. Landsat: ~5% of pixel—marginally detectable. Landsat is better, but higher resolution (ASTER/airborne) preferred.
</details>

<h3>Exercise 17</h3>
<p>Calculate radiance sensitivity at 11 μm, 300 K.</p>
<details>
<summary>Solution</summary>
$\partial B/\partial T \approx 0.09$ W·m⁻²·sr⁻¹·μm⁻¹·K⁻¹

This enables 1 K discrimination.
</details>

<h3>Exercise 18</h3>
<p>A 2×2 km area with 100 m pixels. How many pixels?</p>
<details>
<summary>Solution</summary>
$2000/100 \times 2000/100 = 400$ pixels
</details>

<h3>Exercise 19</h3>
<p>Water vapor increases from 1.5 to 3.0 g/cm². If $\tau = e^{-0.1w}$, calculate transmissivity change.</p>
<details>
<summary>Solution</summary>
At w=1.5: $\tau_1 = e^{-0.15} = 0.861$
At w=3.0: $\tau_2 = e^{-0.30} = 0.741$

Transmissivity decreases by 12 percentage points.
</details>

<h3>Exercise 20</h3>
<p>Temperatures: 25°C, 38°C, 27°C, 36°C (morning, afternoon, morning, afternoon). Calculate mean diurnal range.</p>
<details>
<summary>Solution</summary>
Day 1: 38-25=13°C; Day 2: 36-27=9°C

Mean DTR = 11°C
</details>

<h3>Exercise 21</h3>
<p>Concrete (ε=0.92) and vegetation (ε=0.98) are both at 35°C. What apparent temperature difference would a sensor measure?</p>
<details>
<summary>Solution</summary>
Concrete: $T_B = 308.15 \times 0.92^{0.25} = 301.7$ K = 28.5°C
Vegetation: $T_B = 308.15 \times 0.98^{0.25} = 306.6$ K = 33.4°C

Apparent difference: 4.9°C despite identical actual temperatures.
</details>

<h3>Exercise 22</h3>
<p>Cooling pond limit: 45°C. Measured: 42°C center, 38°C edges. Is facility in compliance?</p>
<details>
<summary>Solution</summary>
Maximum: 42°C; Limit: 45°C; Margin: 3°C

In compliance, but sensor uncertainty (±1.5°C) means continued monitoring recommended.
</details>

<h3>Exercise 23</h3>
<p>MODIS: 10:30 AM shows 28°C, 1:30 PM shows 36°C. Estimate noon temperature.</p>
<details>
<summary>Solution</summary>
Rate: 8°C/3hr = 2.67°C/hr
Noon (1.5 hr after 10:30): $T = 28 + 4.0 = 32°C$
</details>

<h3>Exercise 24</h3>
<p>Calculate vegetation fraction for NDVI=0.42, given NDVI_soil=0.15, NDVI_veg=0.65.</p>
<details>
<summary>Solution</summary>
$$P_v = \left(\frac{0.42 - 0.15}{0.65 - 0.15}\right)^2 = 0.29$$

29% vegetation cover.
</details>

<h3>Exercise 25</h3>
<p>A dust storm reduces transmissivity from 0.90 to 0.70. How does this affect brightness temperature if surface is 310 K and atmosphere is 280 K?</p>
<details>
<summary>Solution</summary>
Clear (τ=0.90): $T_B = 0.90 \times 310 + 0.10 \times 280 = 307$ K

Dusty (τ=0.70): $T_B = 0.70 \times 310 + 0.30 \times 280 = 301$ K

The dust reduces measured temperature by 6 K.
</details>

<h3>Exercise 26</h3>
<p>Sensor noise is 0.3 K. How many observations needed to detect 2°C change at 95% confidence?</p>
<details>
<summary>Solution</summary>
Required: $\frac{2 \times 0.3}{\sqrt{n}} \leq 0.5$, giving $n \geq 2$.

Minimum 2 observations; 4-5 recommended for robustness.
</details>

<h3>Exercise 27</h3>
<p>A 10 m wide canal (22°C) crosses a 100 m pixel of fields (34°C). What pixel temperature results?</p>
<details>
<summary>Solution</summary>
Water fraction: $10/100 = 0.10$

$T_{pixel} = 0.10 \times 22 + 0.90 \times 34 = 32.8°C$

The canal reduces temperature by only 1.2°C.
</details>

<h3>Exercise 28</h3>
<p>Pre-dawn acquisition at 5:00 AM, afternoon at 2:00 PM, sunrise at 6:00 AM. How many hours of solar heating affect the afternoon image?</p>
<details>
<summary>Solution</summary>
Solar heating: 2:00 PM - 6:00 AM = 8 hours

This maximizes thermal contrast between materials.
</details>

<h3>Exercise 29</h3>
<p>Hot reference: 45°C, cold reference: 22°C. A crop pixel at 30°C has what evaporative fraction?</p>
<details>
<summary>Solution</summary>
$$EF = \frac{45 - 30}{45 - 22} = \frac{15}{23} = 0.65$$

65% of available energy goes to evapotranspiration.
</details>

<h3>Exercise 30</h3>
<p>Detecting 0.5°C/year warming with annual variation ±8°C and sensor uncertainty 1.0 K. How many years needed?</p>
<details>
<summary>Solution</summary>
Annual uncertainty: $\sqrt{(8/\sqrt{12})^2 + 1^2} = 2.5°C$

For 95% confidence: $n \geq (2 \times 2.5/0.5)^2/12 \approx 8$ years

8-10 years of monthly data needed.
</details>

</div>
