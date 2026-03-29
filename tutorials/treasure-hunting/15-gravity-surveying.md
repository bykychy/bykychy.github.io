---
layout: tutorial
title: "How Gravity Surveying Reveals Subsurface Density Contrasts in Central Asia"
description: "A practical tutorial on microgravity surveys, Bouguer anomalies, terrain corrections, and applications for void detection and geological mapping, with 30 exercises and solutions."
difficulty: advanced
estimated_time: "3-4 hours"
series: treasure-hunting
order: 15
---

## Why this tutorial matters

Gravity surveying measures minute variations in Earth's gravitational field caused by lateral density contrasts in the subsurface. Unlike seismic methods that probe velocity structure or electromagnetic methods sensitive to conductivity, gravity responds directly to mass distribution—revealing basins, faults, voids, ore bodies, and buried structures through their density signatures.

In Central Asia, gravity methods address questions spanning scales from regional tectonics to archaeological prospection: What is the depth to basement beneath sedimentary basins? Where do fault zones displace dense bedrock? Are there voids, tunnels, or burial chambers beneath the surface? The technique requires no active source, operates passively, and provides depth-integrated information about subsurface density.

<div class="info-box tip">
  <strong>Key idea</strong>
  Gravity anomalies arise from density contrasts, not absolute densities. A buried void (air vs. rock) and a salt dome (salt vs. sediment) both create anomalies because they differ from surrounding material. The sign and magnitude of the anomaly depend on whether the feature is denser or less dense than its host.
</div>

## Why gravity matters: density contrasts reveal hidden structures

Every mass exerts gravitational attraction. When subsurface materials have different densities, they cause spatial variations in the gravitational field that can be measured at the surface. These variations are small—typically microGals (µGal) to milliGals (mGal)—but modern instruments detect them with remarkable precision.

### What gravity can detect

**Geological structures**: Sedimentary basins (low-density fill over dense basement), salt domes (low-density salt rising through denser sediments), faults displacing rocks of different density, intrusive bodies (dense mafic intrusions vs. lighter country rock).

**Voids and cavities**: Caves, mine workings, tunnels, and burial chambers create negative anomalies because air or partially filled voids are less dense than surrounding rock or soil.

**Archaeological features**: Large structures—tombs, collapsed chambers, dense stone foundations, compacted floors—can produce detectable anomalies when density contrast and size are sufficient.

### Density contrasts in Central Asian settings

| Feature | Typical density (kg/m³) | Contrast with host |
|---------|------------------------|-------------------|
| Air (void) | 0 | −1800 to −2600 |
| Loess | 1400–1700 | reference |
| Alluvial sand/gravel | 1800–2100 | +200 to +500 |
| Saturated sediment | 2000–2200 | +300 to +600 |
| Limestone | 2400–2700 | +700 to +1000 |
| Granite | 2650–2750 | +900 to +1100 |
| Salt | 2100–2200 | −300 to −500 (vs. limestone) |
| Basalt | 2800–3000 | +1100 to +1300 |

## Gravity fundamentals: Newton's law and gravitational acceleration

### Newton's law of universal gravitation

The gravitational force between two masses is:

$$
F = G\frac{m_1 m_2}{r^2}
$$

where $G = 6.674 \times 10^{-11}$ N·m²/kg² is the gravitational constant, $m_1$ and $m_2$ are the masses, and $r$ is the distance between their centers.

### Gravitational acceleration

The gravitational acceleration (gravity) at Earth's surface is the force per unit mass:

$$
g = \frac{F}{m} = G\frac{M_E}{R_E^2}
$$

For Earth, this gives approximately 9.81 m/s² or 981,000 mGal (where 1 mGal = 10⁻⁵ m/s² = 10⁻³ cm/s² in CGS units).

### Units in gravity surveying

The **Gal** (after Galileo) equals 1 cm/s². Gravity surveys measure anomalies in:
- **milliGal (mGal)**: 10⁻³ Gal = 10⁻⁵ m/s²
- **microGal (µGal)**: 10⁻⁶ Gal = 10⁻⁸ m/s²

Modern gravimeters achieve precision of 1–10 µGal. To put this in perspective, a 1 µGal anomaly represents a change of about one part in 10⁹ of Earth's gravity.

<div class="info-box tip">
  <strong>Key idea</strong>
  Archaeological and engineering applications require microgravity surveys with precision better than 10 µGal. Regional geological mapping may accept 0.1–1 mGal precision. The required precision determines instrument choice and survey procedures.
</div>

### Gravity of a buried sphere

A sphere of radius $a$, density contrast $\Delta\rho$, and center depth $z$ produces a vertical gravity anomaly at the surface directly above it:

$$
\Delta g_z = \frac{4}{3}\pi G \Delta\rho a^3 \cdot \frac{z}{(x^2 + z^2)^{3/2}}
$$

At the point directly above ($x = 0$):

$$
\Delta g_{max} = \frac{4}{3}\pi G \Delta\rho \frac{a^3}{z^2}
$$

The anomaly decreases with the square of depth—doubling the depth reduces the anomaly by a factor of four.

### The half-width rule for depth estimation

For a spherical source, the horizontal distance from the maximum anomaly to the half-maximum value ($x_{1/2}$) relates to depth:

$$
z = 1.305 \cdot x_{1/2}
$$

This provides a quick field estimate of source depth from profile measurements.

## Gravimeters: instruments for measuring gravity

### Relative vs. absolute gravimeters

**Absolute gravimeters** measure the actual value of $g$ by timing a freely falling mass or atom interferometry. They are large, expensive, and typically used for establishing base stations. Precision: 1–2 µGal.

**Relative gravimeters** measure the difference in gravity between stations. They use a mass on a spring; small gravity changes cause small displacements measured with extreme precision. Most field surveys use relative gravimeters.

### Spring gravimeter principles

A mass $m$ suspended on a spring with constant $k$ reaches equilibrium where:

$$
mg = k \cdot \delta
$$

where $\delta$ is the spring extension. A change in gravity $\Delta g$ causes:

$$
\Delta g = \frac{k}{m} \cdot \Delta\delta
$$

Modern gravimeters use astatic or zero-length springs that amplify small displacements. The Scintrex CG-5 and CG-6 are common field instruments with 1–5 µGal repeatability.

### Instrument drift

All spring gravimeters exhibit drift—a slow change in reading over time due to spring relaxation, temperature effects, and other factors. Drift rates are typically 10–500 µGal/day for well-behaved instruments.

**Drift correction** requires repeated measurements at a base station throughout the survey. If drift is linear:

$$
\Delta g_{drift}(t) = \frac{g_{base}(t_{end}) - g_{base}(t_{start})}{t_{end} - t_{start}} \cdot (t - t_{start})
$$

The corrected reading at any station is:

$$
g_{corrected} = g_{observed} - \Delta g_{drift}(t)
$$

<div class="info-box tip">
  <strong>Key idea</strong>
  Always return to the base station multiple times during a survey. For microgravity work, reoccupy the base every 1–2 hours. Plot drift curves to verify linear behavior; non-linear drift may indicate instrument problems.
</div>

### Earth tides

The gravitational attraction of the Moon and Sun causes Earth tides—periodic variations in gravity up to ±300 µGal with periods of ~12 and ~24 hours. Earth tide corrections are computed from astronomical formulas and applied to all readings.

The tidal gravity variation is approximately:

$$
\Delta g_{tide} = 1.16 \times (\text{theoretical tide}) \cdot \delta
$$

where $\delta$ ≈ 1.16 is the gravimetric factor accounting for Earth's elastic response.

## Survey design: stations, base stations, and loops

### Station spacing

Station spacing depends on target size and depth. For a feature of diameter $D$ at depth $z$, use spacing $\leq D/2$ to adequately sample the anomaly.

| Application | Target size | Depth | Suggested spacing |
|------------|-------------|-------|-------------------|
| Regional geology | km-scale basins | 1–10 km | 1–5 km |
| Detailed structure | 100s of meters | 100–1000 m | 50–200 m |
| Engineering/cavities | 10–50 m | 5–30 m | 5–15 m |
| Archaeology (tombs) | 2–10 m | 1–5 m | 0.5–2 m |

### Base station requirements

The base station serves as the reference for all relative measurements. Requirements:

- Stable ground (no subsidence, traffic vibration)
- Protected from weather extremes
- Accessible throughout the survey
- Known elevation (surveyed to ±1 cm for microgravity)
- Ideally tied to a regional gravity network

### Loop closures

A survey loop begins and ends at the base station, allowing drift estimation and quality control. The loop misclosure is:

$$
\text{Misclosure} = g_{base}(t_{end}) - g_{base}(t_{start}) - \Delta g_{drift}
$$

Acceptable misclosure for microgravity: < 10 µGal. Larger misclosures indicate instrument problems, reading errors, or unmodeled drift.

### Survey procedures for microgravity

1. Level the instrument carefully at each station (< 10 arc-seconds)
2. Allow thermal equilibration (2–5 minutes at each station)
3. Take multiple readings (3–5) and average
4. Record time, reading, and any environmental notes
5. Reoccupy base station every 1–2 hours
6. Survey in calm weather (avoid wind, temperature extremes)

## Data reduction: from observed gravity to anomalies

Raw gravimeter readings must be corrected for several effects before interpretation. Each correction removes a known contribution to isolate the anomaly of interest.

### Latitude correction

Gravity varies with latitude due to Earth's rotation and equatorial bulge. The International Gravity Formula gives normal gravity:

$$
g_0(\phi) = 978032.67714\left(1 + 0.00193185138639 \sin^2\phi - 0.00000228 \sin^2(2\phi)\right) \text{ mGal}
$$

where $\phi$ is latitude. The latitude correction for north-south distance $\Delta y$ is:

$$
\Delta g_{lat} = 0.812 \sin(2\phi) \cdot \Delta y \text{ mGal/km}
$$

At latitude 42°N (typical for Central Asia), this is approximately 0.78 mGal/km.

### Free-air correction

Gravity decreases with elevation above the reference ellipsoid. The free-air correction accounts for the station being at height $h$ above sea level:

$$
\Delta g_{FA} = -0.3086 \cdot h \text{ mGal}
$$

where $h$ is in meters. This assumes no mass between the station and the reference surface.

### Bouguer correction

The Bouguer correction accounts for the gravitational attraction of rock between the station and sea level. For an infinite horizontal slab of thickness $h$ and density $\rho$:

$$
\Delta g_{Bouguer} = 2\pi G \rho h = 0.04192 \rho h \text{ mGal}
$$

where $\rho$ is in g/cm³ and $h$ in meters. A standard density of 2.67 g/cm³ gives:

$$
\Delta g_{Bouguer} = 0.1119 \cdot h \text{ mGal}
$$

The combined free-air and Bouguer correction is:

$$
\Delta g_{combined} = (-0.3086 + 0.1119) \cdot h = -0.1967 \cdot h \text{ mGal}
$$

<div class="info-box tip">
  <strong>Key idea</strong>
  Elevation must be known precisely for accurate Bouguer anomalies. A 1 cm elevation error causes approximately 2 µGal error—critical for microgravity surveys. Use differential GNSS or precise leveling to achieve centimeter accuracy.
</div>

### Terrain correction

The Bouguer correction assumes flat terrain, but real topography deviates from an infinite slab. Hills above station level exert upward attraction (reducing observed gravity), while valleys represent missing mass (also reducing gravity). Both require positive terrain corrections.

The terrain correction is computed by dividing surrounding terrain into compartments and summing their gravitational effects:

$$
\Delta g_{terrain} = G\rho \int \int \int \frac{z - z_0}{r^3} dV
$$

For a cylindrical ring segment (Hammer chart method):

$$
\Delta g_i = G\rho \cdot 2\pi (r_2 - r_1) \left[\sqrt{r_2^2 + \bar{z}^2} - \sqrt{r_1^2 + \bar{z}^2} - (r_2 - r_1)\right] / n
$$

where $n$ is the number of compartments in the ring.

Modern surveys use digital elevation models (DEMs) and compute terrain corrections numerically within radii of 100 m to 166.7 km (the Hayford outer limit).

### The complete Bouguer anomaly

Combining all corrections:

$$
\Delta g_{CBA} = g_{obs} - g_0(\phi) + 0.3086h - 2\pi G\rho h + \Delta g_{terrain} + \Delta g_{tide} + \Delta g_{drift}
$$

Or more compactly:

$$
\Delta g_{CBA} = g_{obs} - g_0 + \Delta g_{FA} - \Delta g_{Bouguer} + \Delta g_{TC}
$$

The **complete Bouguer anomaly** (CBA) reflects subsurface density variations after removing all predictable contributions from latitude, elevation, and topography.

## Bouguer anomaly interpretation

### Qualitative interpretation

**Positive anomalies** indicate excess mass: dense intrusions, uplifted basement, metallic ore bodies.

**Negative anomalies** indicate mass deficit: sedimentary basins, salt domes, voids, low-density fill.

**Gradients** mark lateral density boundaries: faults, basin edges, intrusion contacts.

### Quantitative interpretation: forward modeling

Forward modeling computes the gravity effect of an assumed density structure and compares it to observed anomalies. Iteratively adjust the model until computed and observed anomalies match.

For a 2D horizontal cylinder (approximating tunnels, buried walls):

$$
\Delta g = 2\pi G \Delta\rho R^2 \cdot \frac{z}{x^2 + z^2}
$$

where $R$ is the cylinder radius and $z$ is the depth to the axis.

For a 2D vertical fault with throw $t$ and density contrast $\Delta\rho$:

$$
\Delta g(x) = 2G\Delta\rho t \cdot \arctan\left(\frac{x}{z}\right)
$$

### Inverse modeling

Inversion algorithms find density distributions that explain observed anomalies. However, gravity inversion is inherently non-unique—many different mass distributions can produce the same surface anomaly.

Constraints from geology, drilling, or other geophysical methods are essential to reduce ambiguity.

<div class="info-box tip">
  <strong>Key idea</strong>
  Gravity inversion is non-unique: a shallow small body can produce the same anomaly as a deeper larger body. Always constrain interpretations with independent data—boreholes, seismic sections, geological mapping.
</div>

## Regional-residual separation

Observed Bouguer anomalies contain contributions from sources at all depths. Regional-residual separation isolates shallow targets (residual) from deep, broad features (regional).

### Graphical methods

Draw a smooth regional trend through the data by eye or fitting, then subtract it from observed values. Simple but subjective.

### Polynomial surfaces

Fit a low-order polynomial to station values:

$$
g_{regional}(x,y) = a_0 + a_1 x + a_2 y + a_3 x^2 + a_4 xy + a_5 y^2 + \cdots
$$

The residual is:

$$
g_{residual} = g_{CBA} - g_{regional}
$$

Higher-order polynomials capture more of the anomaly field; choose the order based on the scale of interest.

### Wavelength filtering

Upward continuation attenuates short-wavelength anomalies from shallow sources:

$$
F(k, h) = e^{-2\pi k h}
$$

where $k$ is wavenumber and $h$ is continuation height. The regional is approximated by upward-continued data; the residual is the difference from observed.

### Matched filtering

Design filters based on expected source depths. If a target is at depth $z$, its anomaly spectrum peaks at wavenumber $k = 1/(2\pi z)$. Bandpass filtering around this wavenumber enhances targets at that depth.

## Microgravity for archaeology: tomb detection and tunnel mapping

### Why microgravity works for archaeology

Archaeological features create density contrasts with surrounding soil:
- **Voids** (tombs, chambers, tunnels): $\Delta\rho = -1500$ to $-2000$ kg/m³
- **Stone walls/foundations**: $\Delta\rho = +500$ to $+1000$ kg/m³
- **Compacted floors**: $\Delta\rho = +200$ to $+400$ kg/m³
- **Rubble fill**: $\Delta\rho = -200$ to $-500$ kg/m³

### Detectability criteria

The minimum detectable anomaly depends on survey precision. For a spherical void of radius $a$ at depth $z$:

$$
\Delta g = \frac{4}{3}\pi G \Delta\rho \frac{a^3}{z^2}
$$

With $G = 6.67 \times 10^{-11}$, $\Delta\rho = -1800$ kg/m³ (air vs. soil), $a = 2$ m, and $z = 4$ m:

$$
\Delta g = \frac{4}{3}\pi \times 6.67 \times 10^{-11} \times (-1800) \times \frac{8}{16} = -63 \text{ µGal}
$$

This is readily detectable with modern microgravity surveys (precision 5–10 µGal).

### Survey design for archaeological microgravity

1. **Station spacing**: 0.5–2 m depending on expected target size
2. **Grid coverage**: Ensure full coverage of suspected area plus buffer
3. **Elevation control**: Survey all stations to ±1 cm using GNSS or total station
4. **Base station**: Establish stable reference outside the survey area
5. **Reading protocol**: Multiple readings per station, reoccupy base frequently

### Case study: Kurgan burial chambers

Central Asian kurgans (burial mounds) often contain stone-lined chambers at depth. A typical chamber (3 m × 2 m × 2 m) at 3 m depth produces a negative anomaly of approximately 50–100 µGal. Microgravity surveys can locate chambers without excavation, guiding archaeological investigation.

<div class="info-box tip">
  <strong>Key idea</strong>
  Microgravity is ideal for detecting voids and cavities because the density contrast is large (air vs. rock/soil). A 2-meter chamber at 4-meter depth produces an anomaly 10× larger than instrumental precision.
</div>

### Tunnel and qanat detection

Central Asia's extensive qanat (karez) irrigation systems consist of underground tunnels at depths of 2–20 m. A circular tunnel of diameter $D$ at depth $z$ produces:

$$
\Delta g = 2\pi G \Delta\rho \left(\frac{D}{2}\right)^2 \cdot \frac{z}{z^2 + x^2}
$$

A 1.5 m diameter qanat at 5 m depth with $\Delta\rho = -1600$ kg/m³ (air-filled) creates approximately 15 µGal anomaly—detectable but requiring careful surveys.

## Integration with seismic and magnetic data

### Gravity-seismic integration

Seismic methods provide velocity structure; gravity provides density. The two are related through empirical relationships:

**Gardner's relation**:
$$
\rho = 1.741 \cdot V_P^{0.25}
$$

where $\rho$ is in g/cm³ and $V_P$ in km/s. This allows seismic velocity models to constrain gravity interpretation, and vice versa.

**Joint inversion**: Simultaneously invert gravity and seismic data, imposing consistency between velocity and density models. This reduces non-uniqueness in both methods.

### Gravity-magnetic integration

Magnetic anomalies arise from susceptibility contrasts; gravity from density contrasts. Many geological units have characteristic density-susceptibility relationships:

| Rock type | Density (g/cm³) | Susceptibility (SI) |
|-----------|-----------------|---------------------|
| Sediments | 1.8–2.4 | 0.0001–0.001 |
| Limestone | 2.5–2.7 | 0.0001–0.0003 |
| Granite | 2.6–2.7 | 0.001–0.05 |
| Basalt | 2.8–3.0 | 0.01–0.1 |
| Magnetite ore | 4.0–5.0 | 0.5–1.0 |

Combining gravity (mass) and magnetic (magnetization) data allows lithological discrimination.

**Poisson's relation** links gravity and magnetic potential fields for uniformly magnetized bodies:

$$
M = \frac{J}{G\rho} \nabla V
$$

where $J$ is magnetization intensity. This allows computing one potential field from the other if the magnetization-density ratio is known.

### Multi-method workflows

1. **Regional reconnaissance**: Airborne magnetics and gravity define basin geometry, major structures
2. **Detailed gravity**: Refine depth to basement, map density variations
3. **Seismic**: Velocity structure, layer geometry, fault locations
4. **Microgravity**: Locate voids, cavities, near-surface density anomalies
5. **Integration**: Joint interpretation constrained by all datasets

## Central Asia applications: basins, faults, and voids

### Sedimentary basin studies

Central Asian basins (Ferghana, Tarim, Junggar, Afghan-Tajik) contain thick sedimentary sequences over crystalline basement. Gravity mapping reveals:

- **Basin depth**: Negative Bouguer anomalies correlate with sediment thickness
- **Basin geometry**: Gravity gradients mark basin margins and internal structures
- **Basement highs**: Positive residual anomalies indicate shallow basement

The gravity effect of a sedimentary basin can be modeled as:

$$
\Delta g = 2\pi G \Delta\rho \left[z_2 - z_1 + \sqrt{z_1^2 + a^2} - \sqrt{z_2^2 + a^2}\right]
$$

for a 2D basin with half-width $a$ and depth range $z_1$ to $z_2$.

### Fault detection

Faults juxtaposing rocks of different density produce gravity gradients. The maximum horizontal gradient occurs directly over a vertical fault:

$$
\left(\frac{dg}{dx}\right)_{max} = 2G\Delta\rho \frac{t}{z}
$$

where $t$ is fault throw and $z$ is depth to the faulted horizon. Horizontal gradient maps effectively delineate fault traces.

### Void detection in karst terrain

Central Asian limestones and gypsum formations host karst voids that pose engineering hazards. Microgravity surveys detect cavities through their negative anomalies. For a spherical cave:

$$
\text{Detectability threshold:} \quad \frac{a^3}{z^2} > \frac{\Delta g_{min}}{(4/3)\pi G |\Delta\rho|}
$$

A 5 m radius cavity in limestone ($\Delta\rho = -2500$ kg/m³) at 10 m depth produces approximately 260 µGal—easily detectable.

### Salt tectonics

Salt structures in Central Asian basins (particularly Afghan-Tajik basin) produce distinctive gravity signatures. Salt ($\rho$ ≈ 2.15 g/cm³) is less dense than surrounding sediments ($\rho$ ≈ 2.4–2.5 g/cm³), creating negative anomalies over salt domes and positive edge effects.

<div class="info-box tip">
  <strong>Key idea</strong>
  In Central Asian tectonic settings, gravity effectively maps: (1) sedimentary basin depth, (2) fault displacement, (3) basement topography, and (4) salt structures. These complement seismic interpretation and reduce exploration risk.
</div>

## Field survey checklist

<ul class="checklist">
  <li>Verify gravimeter calibration and drift characteristics before fieldwork</li>
  <li>Establish base station on stable ground with known coordinates and elevation</li>
  <li>Tie base station to regional gravity network if available</li>
  <li>Survey all station positions with GNSS or total station (±1 cm elevation for microgravity)</li>
  <li>Plan survey loops returning to base every 1–2 hours</li>
  <li>Allow instrument thermal equilibration at each station (2–5 minutes)</li>
  <li>Level gravimeter carefully (< 10 arc-seconds)</li>
  <li>Record multiple readings per station and compute average</li>
  <li>Note environmental conditions (wind, temperature, ground vibration)</li>
  <li>Apply real-time earth tide corrections or record precise times</li>
  <li>Monitor drift by plotting base station readings throughout day</li>
  <li>Verify loop closures meet precision requirements (< 10 µGal for microgravity)</li>
  <li>Obtain DEM for terrain corrections (resolution appropriate to survey)</li>
  <li>Select Bouguer density appropriate for local geology</li>
  <li>Apply all corrections systematically: drift, tide, latitude, free-air, Bouguer, terrain</li>
  <li>Perform regional-residual separation appropriate to target depth</li>
  <li>Interpret anomalies with geological constraints and complementary data</li>
  <li>Document all procedures, corrections, and uncertainties in survey report</li>
</ul>

## Exercises

<div class="exercises">
  <h2>Easy exercises</h2>

  <div class="exercise"><h4>Easy 1 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Calculate the gravitational acceleration at Earth's surface using $G = 6.674 \times 10^{-11}$ N·m²/kg², $M_E = 5.972 \times 10^{24}$ kg, and $R_E = 6.371 \times 10^{6}$ m.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$g = GM_E/R_E^2 = (6.674 \times 10^{-11})(5.972 \times 10^{24})/(6.371 \times 10^6)^2 = 3.986 \times 10^{14}/4.059 \times 10^{13} = 9.82$ m/s² = 982,000 mGal.</p></div></div>

  <div class="exercise"><h4>Easy 2 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Convert 25 µGal to mGal and to m/s².</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>25 µGal = 0.025 mGal = $25 \times 10^{-6}$ Gal = $25 \times 10^{-6} \times 10^{-2}$ m/s² = $2.5 \times 10^{-7}$ m/s².</p></div></div>

  <div class="exercise"><h4>Easy 3 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>A station is at 1200 m elevation. Calculate the free-air correction using $\Delta g_{FA} = -0.3086h$ mGal.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$\Delta g_{FA} = -0.3086 \times 1200 = -370.3$ mGal. This large negative value is added to observed gravity to correct for elevation above the reference ellipsoid.</p></div></div>

  <div class="exercise"><h4>Easy 4 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Calculate the Bouguer slab correction for 1200 m elevation using density 2.67 g/cm³.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$\Delta g_{Bouguer} = 0.04192 \times 2.67 \times 1200 = 134.3$ mGal. This positive value is subtracted from observed gravity to remove the effect of rock between the station and sea level.</p></div></div>

  <div class="exercise"><h4>Easy 5 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What is the combined elevation correction (free-air + Bouguer) for a station at 500 m with $\rho = 2.5$ g/cm³?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Free-air: $-0.3086 \times 500 = -154.3$ mGal. Bouguer: $0.04192 \times 2.5 \times 500 = 52.4$ mGal. Combined: $-154.3 + 52.4 = -101.9$ mGal (add to observed gravity). Alternatively: $(-0.3086 + 0.04192 \times 2.5) \times 500 = -0.2038 \times 500 = -101.9$ mGal.</p></div></div>

  <div class="exercise"><h4>Easy 6 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>A gravimeter shows drift of 50 µGal over a 5-hour survey. What is the drift rate?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Drift rate = 50 µGal / 5 hours = 10 µGal/hour. This is typical for well-behaved instruments and easily corrected with regular base station reoccupations.</p></div></div>

  <div class="exercise"><h4>Easy 7 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Why do both hills above a station and valleys below a station require positive terrain corrections?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Hills above the station exert upward gravitational attraction, reducing observed gravity. Valleys represent missing mass compared to the infinite slab assumption, also reducing observed gravity. Both effects decrease the measured value, so positive corrections are needed to compensate.</p></div></div>

  <div class="exercise"><h4>Easy 8 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>A buried feature produces a 40 µGal anomaly with half-width $x_{1/2} = 3.5$ m. Estimate the depth using $z = 1.305 \cdot x_{1/2}$.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$z = 1.305 \times 3.5 = 4.6$ m. This half-width rule assumes a spherical source and provides a quick field estimate of depth.</p></div></div>

  <div class="exercise"><h4>Easy 9 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What density contrast would you expect between an air-filled void and surrounding limestone ($\rho = 2600$ kg/m³)?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$\Delta\rho = \rho_{air} - \rho_{limestone} = 0 - 2600 = -2600$ kg/m³. The large negative contrast explains why voids produce strong negative anomalies that are relatively easy to detect.</p></div></div>

  <div class="exercise"><h4>Easy 10 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>For a microgravity survey with 5 µGal precision, how accurately must elevation be known if the combined correction factor is 0.2 mGal/m?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Elevation error = Gravity precision / Correction factor = 5 µGal / 0.2 mGal/m = 0.005 mGal / 0.2 mGal/m = 0.025 m = 2.5 cm. For 5 µGal precision, elevation must be known to better than 2.5 cm.</p></div></div>

  <h2>Medium exercises</h2>

  <div class="exercise"><h4>Medium 1 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>Calculate the gravity anomaly directly above a spherical void of radius 2.5 m at depth 5 m in soil with density 1900 kg/m³.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$\Delta\rho = 0 - 1900 = -1900$ kg/m³. $\Delta g = (4/3)\pi G \Delta\rho (a^3/z^2) = (4/3)\pi \times 6.67 \times 10^{-11} \times (-1900) \times (2.5^3/5^2)$. Volume factor: $2.5^3/25 = 15.625/25 = 0.625$ m. $\Delta g = 4.19 \times 6.67 \times 10^{-11} \times (-1900) \times 0.625 = -3.32 \times 10^{-7}$ m/s² = -33.2 µGal.</p></div></div>

  <div class="exercise"><h4>Medium 2 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>A survey loop starts at base (reading 5234.56 mGal) at 08:00, occupies 12 stations, and returns to base at 12:00 (reading 5234.61 mGal). Calculate the drift correction for a station occupied at 10:00.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Total drift = 5234.61 - 5234.56 = 0.05 mGal = 50 µGal over 4 hours. Drift rate = 50/4 = 12.5 µGal/hour. At 10:00 (2 hours after start): Drift correction = $12.5 \times 2 = 25$ µGal. Subtract 25 µGal from the 10:00 reading to correct for drift.</p></div></div>

  <div class="exercise"><h4>Medium 3 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>What minimum void radius is detectable at 6 m depth if survey precision is 8 µGal and $\Delta\rho = -2000$ kg/m³?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>From $\Delta g = (4/3)\pi G |\Delta\rho| (a^3/z^2)$, solve for $a$: $a^3 = \Delta g \cdot z^2 / [(4/3)\pi G |\Delta\rho|]$. $a^3 = (8 \times 10^{-8}) \times 36 / [(4.19) \times (6.67 \times 10^{-11}) \times 2000]$. $a^3 = 2.88 \times 10^{-6} / (5.59 \times 10^{-7}) = 5.15$ m³. $a = 1.73$ m. Minimum detectable radius is approximately 1.7 m.</p></div></div>

  <div class="exercise"><h4>Medium 4 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>A horizontal tunnel (approximated as a 2D cylinder) has radius 1 m, runs perpendicular to the survey profile, and is at 4 m depth. Calculate the maximum anomaly if $\Delta\rho = -1800$ kg/m³.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>For a 2D horizontal cylinder: $\Delta g_{max} = 2\pi G \Delta\rho R^2 / z$. $\Delta g_{max} = 2\pi \times (6.67 \times 10^{-11}) \times (-1800) \times 1^2 / 4$. $= 2\pi \times 6.67 \times 10^{-11} \times (-1800) / 4 = -1.89 \times 10^{-7}$ m/s² = -18.9 µGal. The anomaly is detectable with good microgravity technique.</p></div></div>

  <div class="exercise"><h4>Medium 5 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>Explain why the latitude correction is necessary and calculate it for a station 500 m north of the base at latitude 40°N.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Gravity increases from equator to poles due to Earth's rotation (centrifugal force) and equatorial bulge. The latitude correction removes this effect. $\Delta g_{lat} = 0.812 \sin(2\phi) \cdot \Delta y$ mGal/km. At $\phi = 40°$: $\sin(80°) = 0.985$. $\Delta g_{lat} = 0.812 \times 0.985 \times 0.5 = 0.40$ mGal. Station 500 m north has 0.40 mGal higher normal gravity than base.</p></div></div>

  <div class="exercise"><h4>Medium 6 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>A gravity profile across a fault shows a gradient of 2 mGal over 200 m. If the faulted layer is at 100 m depth with $\Delta\rho = 400$ kg/m³, estimate the fault throw.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>The maximum gradient over a vertical fault is $(dg/dx)_{max} = 2G\Delta\rho(t/z)$. Observed gradient ≈ 2 mGal/200 m = 0.01 mGal/m = $10^{-7}$ m/s²/m. $t = (dg/dx) \cdot z / (2G\Delta\rho) = (10^{-7}) \times 100 / (2 \times 6.67 \times 10^{-11} \times 400)$. $= 10^{-5} / (5.34 \times 10^{-8}) = 187$ m. Estimated fault throw is approximately 190 m.</p></div></div>

  <div class="exercise"><h4>Medium 7 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>What is the Bouguer anomaly at a station with: observed gravity 979,432.5 mGal, latitude 42°N ($g_0 = 980,318.2$ mGal), elevation 850 m, terrain correction 0.3 mGal, using $\rho = 2.67$ g/cm³?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Free-air correction: $0.3086 \times 850 = 262.3$ mGal. Bouguer correction: $0.04192 \times 2.67 \times 850 = 95.1$ mGal. $\Delta g_{CBA} = g_{obs} - g_0 + \Delta g_{FA} - \Delta g_{Bouguer} + \Delta g_{TC}$. $= 979432.5 - 980318.2 + 262.3 - 95.1 + 0.3 = -718.2$ mGal. The negative Bouguer anomaly suggests low-density material (sedimentary basin) beneath the station.</p></div></div>

  <div class="exercise"><h4>Medium 8 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>A second-order polynomial regional trend is fitted to Bouguer data: $g_{reg} = -45.2 + 0.05x - 0.02y + 0.001x^2$ mGal. Calculate the residual at a station with $g_{CBA} = -48.5$ mGal at coordinates (100, 50) m.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$g_{reg} = -45.2 + 0.05(100) - 0.02(50) + 0.001(100)^2$. $= -45.2 + 5 - 1 + 10 = -31.2$ mGal. Residual = $g_{CBA} - g_{reg} = -48.5 - (-31.2) = -17.3$ mGal. The negative residual indicates local mass deficit (possible void or low-density fill) at this location.</p></div></div>

  <div class="exercise"><h4>Medium 9 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>Using Gardner's relation $\rho = 1.741 V_P^{0.25}$ (with $V_P$ in km/s), calculate density for sediments with $V_P = 2.5$ km/s and basement with $V_P = 5.5$ km/s. What is the density contrast?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Sediments: $\rho = 1.741 \times 2.5^{0.25} = 1.741 \times 1.257 = 2.19$ g/cm³. Basement: $\rho = 1.741 \times 5.5^{0.25} = 1.741 \times 1.532 = 2.67$ g/cm³. Density contrast: $\Delta\rho = 2.67 - 2.19 = 0.48$ g/cm³ = 480 kg/m³. This contrast drives basin gravity anomalies.</p></div></div>

  <div class="exercise"><h4>Medium 10 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>How does doubling the depth of a spherical source affect its gravity anomaly? What about doubling the radius?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>From $\Delta g \propto a^3/z^2$: Doubling depth ($z \rightarrow 2z$): Anomaly decreases by factor of $2^2 = 4$ (quarter of original). Doubling radius ($a \rightarrow 2a$): Anomaly increases by factor of $2^3 = 8$ (eight times original). The cubic dependence on radius and inverse-square dependence on depth explain why large shallow features dominate while small deep features become undetectable.</p></div></div>

  <h2>Hard exercises</h2>

  <div class="exercise"><h4>Hard 1 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Derive the gravity effect of an infinite horizontal slab of thickness $h$ and density $\rho$, showing that $\Delta g = 2\pi G\rho h$.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Consider a horizontal slab extending infinitely in $x$ and $y$, from $z = 0$ to $z = h$. Divide into thin horizontal sheets of thickness $dz$. Each sheet contributes: $dg = 2\pi G\rho \cdot dz \cdot z / (r^2 + z^2)^{3/2}$ integrated over $r$ from 0 to $\infty$. However, for an infinite sheet at height $z$, the vertical component integrates to $2\pi G\rho \cdot dz$, independent of $z$. Total: $\Delta g = \int_0^h 2\pi G\rho \cdot dz = 2\pi G\rho h$. This result—the Bouguer slab formula—is remarkable because it depends only on thickness, not depth.</p></div></div>

  <div class="exercise"><h4>Hard 2 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>A sedimentary basin is modeled as a 2D feature with parabolic cross-section: depth $z = z_0(1 - x^2/a^2)$ where $z_0 = 3$ km is maximum depth and $a = 10$ km is half-width. If $\Delta\rho = -500$ kg/m³, calculate the maximum Bouguer anomaly.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>For a 2D basin, integrate horizontal strips: $\Delta g = 2G\Delta\rho \int_{-a}^{a} z(x) \cdot \text{contribution factor} \, dx$. For a parabolic basin with small $z_0/a$, the anomaly at $x=0$ is approximately: $\Delta g_{max} \approx 2\pi G\Delta\rho \cdot A / (\pi a)$ where $A$ is cross-sectional area. $A = \int_{-a}^{a} z_0(1 - x^2/a^2) dx = z_0[x - x^3/(3a^2)]_{-a}^{a} = z_0 \cdot 4a/3 = 3000 \times 40000/3 = 40 \times 10^6$ m². $\Delta g_{max} \approx 2G|\Delta\rho| \cdot (2A/\pi a) = 2 \times 6.67 \times 10^{-11} \times 500 \times 2 \times 40 \times 10^6 / (\pi \times 10^4)$. $= 1.33 \times 10^{-7} \times 80 \times 10^6 / (3.14 \times 10^4) = -0.034$ m/s² = -34 mGal (negative because $\Delta\rho < 0$).</p></div></div>

  <div class="exercise"><h4>Hard 3 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Design a microgravity survey to detect a burial chamber 3 m × 2 m × 2 m at 4 m depth beneath a kurgan. Specify station spacing, required precision, and expected anomaly magnitude.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Chamber volume ≈ 12 m³. Equivalent sphere radius: $a = (3V/4\pi)^{1/3} = (3 \times 12/4\pi)^{1/3} = (2.86)^{1/3} = 1.42$ m. With $\Delta\rho = -1800$ kg/m³ (air vs. soil) and $z = 4$ m: $\Delta g = (4/3)\pi G|\Delta\rho|(a^3/z^2) = 4.19 \times 6.67 \times 10^{-11} \times 1800 \times (2.86/16) = 4.19 \times 6.67 \times 10^{-11} \times 1800 \times 0.179 = 9.0 \times 10^{-8}$ m/s² ≈ 9 µGal anomaly. **Survey design**: Precision needed: 3–5 µGal (≥3× signal-to-noise). Station spacing: 1 m (half the smallest dimension). Elevation precision: ±1 cm. Base reoccupation: every hour. Grid: 20 m × 20 m centered on kurgan summit.</p></div></div>

  <div class="exercise"><h4>Hard 4 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Explain the non-uniqueness problem in gravity interpretation. Give a specific example showing how two different mass distributions can produce identical surface anomalies.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Gravity non-uniqueness arises because many mass distributions can produce the same potential field. **Depth-size tradeoff**: A spherical body with mass $M$ at depth $z$ produces $\Delta g_{max} = GM/z^2$. The same anomaly results from mass $4M$ at depth $2z$, or mass $M/4$ at depth $z/2$ (subject to geometric constraints). **Example**: A 2 m radius sphere at 4 m depth with $\Delta\rho = -2000$ kg/m³ produces anomaly ≈ 14 µGal. Identical anomaly from: (a) 1.26 m radius sphere at 2.5 m depth with same density, or (b) 2.5 m radius sphere at 5 m depth with $\Delta\rho = -1280$ kg/m³. **Resolution**: Constrain interpretations with independent data—drilling confirms depth, seismic provides geometry, geology limits plausible densities.</p></div></div>

  <div class="exercise"><h4>Hard 5 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>A qanat (underground irrigation channel) runs through a survey area. It is approximately circular with 1.2 m diameter at varying depth (3–8 m). Design a survey strategy to map its path and explain how anomaly character changes with depth.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>**Survey design**: Profile lines perpendicular to suspected qanat trend, spaced 5–10 m along strike. Station spacing 0.5–1 m along profiles. Grid survey over suspected nodes/junctions at 1 m spacing. Required precision: 3–5 µGal. **Anomaly variation**: For 1.2 m diameter horizontal cylinder (R = 0.6 m), $\Delta\rho = -1700$ kg/m³: At $z = 3$ m: $\Delta g_{max} = 2\pi G|\Delta\rho|R^2/z = 2\pi \times 6.67 \times 10^{-11} \times 1700 \times 0.36/3 = 8.5$ µGal. At $z = 8$ m: $\Delta g_{max} = 2\pi \times 6.67 \times 10^{-11} \times 1700 \times 0.36/8 = 3.2$ µGal. **Character change**: Shallow sections produce sharper, higher-amplitude anomalies; deep sections produce broader, lower-amplitude anomalies. Half-width increases with depth, aiding depth estimation. Sections deeper than ~10 m may be undetectable at 3 µGal precision.</p></div></div>

  <div class="exercise"><h4>Hard 6 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Derive the relationship between the gravity anomaly half-width and source depth for a point mass, showing that $z = x_{1/2} / 0.766$ or equivalently $z = 1.305 \cdot x_{1/2}$.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>For a point mass $M$ at depth $z$, the vertical gravity anomaly at horizontal distance $x$ is: $\Delta g(x) = GM z / (x^2 + z^2)^{3/2}$. Maximum at $x = 0$: $\Delta g_{max} = GM/z^2$. At half-maximum: $\Delta g(x_{1/2}) = \Delta g_{max}/2$, so: $GMz/(x_{1/2}^2 + z^2)^{3/2} = (GM/z^2)/2$. Simplify: $z/(x_{1/2}^2 + z^2)^{3/2} = 1/(2z^2)$. $2z^3 = (x_{1/2}^2 + z^2)^{3/2}$. Let $u = x_{1/2}/z$: $2z^3 = z^3(u^2 + 1)^{3/2}$. $2 = (u^2 + 1)^{3/2}$. $(u^2 + 1) = 2^{2/3} = 1.587$. $u^2 = 0.587$, so $u = 0.766$. Therefore $x_{1/2} = 0.766z$, or $z = x_{1/2}/0.766 = 1.305 \cdot x_{1/2}$. QED.</p></div></div>

  <div class="exercise"><h4>Hard 7 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>A gravity survey over a suspected salt dome reveals a circular negative anomaly of -5 mGal with half-width 2 km. Assuming a vertical cylinder model and salt-sediment density contrast of -350 kg/m³, estimate the dome radius and depth to top.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>For a vertical cylinder, the anomaly shape depends on radius-to-depth ratio. Using the half-width rule for an approximate point mass: $z \approx 1.305 \times 2 = 2.6$ km to center of mass. For a vertical cylinder extending deep, the top is shallower. **Forward modeling approach**: For a vertical cylinder of radius $R$ extending from depth $z_1$ to $z_2 \rightarrow \infty$: $\Delta g_0 = 2\pi G\Delta\rho R[\sqrt{R^2 + z_1^2} - z_1]$. With $\Delta g = -5$ mGal = $-5 \times 10^{-5}$ m/s² and $|\Delta\rho| = 350$ kg/m³: Trial with $R = 2$ km, $z_1 = 1$ km: $\Delta g = 2\pi \times 6.67 \times 10^{-11} \times 350 \times 2000 \times [\sqrt{5 \times 10^6} - 1000]$ $= 2.93 \times 10^{-4} \times [2236 - 1000] = 2.93 \times 10^{-4} \times 1236 = 0.036$ m/s² (too large). Refine: smaller dome or deeper. With $R = 1.5$ km, $z_1 = 1.5$ km: $\Delta g \approx 5$ mGal—reasonable fit. **Estimate**: Radius ~1.5 km, depth to top ~1.5 km.</p></div></div>

  <div class="exercise"><h4>Hard 8 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Explain how joint interpretation of gravity and magnetic data can distinguish between: (a) a mafic intrusion, (b) a sedimentary basin, and (c) a salt dome. What signatures would each produce?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>**Mafic intrusion**: High density (~2900 kg/m³) produces positive gravity anomaly. High susceptibility (0.01–0.1 SI) produces strong positive magnetic anomaly. **Signature**: Coincident gravity and magnetic highs. **Sedimentary basin**: Low density (~2100–2300 kg/m³) produces negative gravity anomaly. Low susceptibility produces negligible magnetic signature unless basement is magnetic. **Signature**: Gravity low with weak/no magnetic signature; possible magnetic high at basin edges where basement shallows. **Salt dome**: Low density (~2150 kg/m³) produces negative gravity anomaly. Very low susceptibility—salt is diamagnetic. **Signature**: Gravity low with no magnetic anomaly (or slight low). **Discrimination**: Mafic intrusion: $+g$, $+M$ (correlated). Basin: $-g$, $M \approx 0$ (gravity-dominated). Salt: $-g$, $M \approx 0$ (similar to basin but typically sharper gradient, circular shape, and may show rim syncline in gravity).</p></div></div>

  <div class="exercise"><h4>Hard 9 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Calculate the terrain correction for a survey station surrounded by a circular mesa. The mesa rim is 100 m from the station, rises 15 m above station level, and extends outward to 500 m. Use $\rho = 2400$ kg/m³.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>The terrain correction for a cylindrical ring (inner radius $r_1$, outer radius $r_2$, height $h$) above station level is: $\Delta g_{TC} = 2\pi G\rho[\sqrt{r_2^2 + h^2} - r_2 - \sqrt{r_1^2 + h^2} + r_1]$. With $r_1 = 100$ m, $r_2 = 500$ m, $h = 15$ m: $\sqrt{r_2^2 + h^2} = \sqrt{250000 + 225} = \sqrt{250225} = 500.22$ m. $\sqrt{r_1^2 + h^2} = \sqrt{10000 + 225} = \sqrt{10225} = 101.12$ m. $\Delta g_{TC} = 2\pi \times 6.67 \times 10^{-11} \times 2400 \times [500.22 - 500 - 101.12 + 100]$. $= 1.006 \times 10^{-6} \times [-0.90] = -9.1 \times 10^{-7}$ m/s² = -91 µGal. Wait—this is negative, but terrain corrections should be positive. **Correction**: The mesa exerts upward pull, reducing observed gravity. The terrain correction formula accounts for this; the positive correction to add is +91 µGal.</p></div></div>

  <div class="exercise"><h4>Hard 10 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>A combined gravity-seismic interpretation project aims to map the basement beneath 2 km of sediment in the Ferghana Basin. Design an integrated workflow specifying: (1) gravity survey parameters, (2) seismic constraints, and (3) joint interpretation strategy.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>**Gravity survey parameters**: Station spacing: 500 m (resolves structures >1 km). Precision: 0.1 mGal (adequate for regional basement mapping). Coverage: Basin extent plus 10 km margins for regional field. Elevation: GNSS to ±10 cm. Bouguer density: 2.4 g/cm³ (average sediment). **Seismic constraints**: Reflection seismics: Image basement reflector, constrain depth at seismic lines. Refraction/MASW: Velocity-depth profiles for density estimation via Gardner relation. Velocity model: Convert to density using $\rho = 1.741V_P^{0.25}$ for forward gravity modeling. **Joint interpretation**: (1) Process gravity to Bouguer anomaly; compute terrain corrections from SRTM DEM. (2) Perform regional-residual separation using upward continuation (5 km) to isolate basement signal. (3) Forward model gravity using seismic-derived basement depths along 2D profiles. (4) Interpolate basement surface between seismic control using gravity inversion constrained by seismic depths. (5) Iterate: adjust sediment density or basement density to minimize misfit. (6) Produce basement depth map with uncertainty estimates based on gravity-seismic consistency.</p></div></div>
</div>

## Final takeaway

Gravity surveying reveals subsurface density variations that other methods cannot directly measure. In Central Asia, applications span from regional basin and fault mapping to archaeological detection of burial chambers and ancient tunnels. The method's strengths—passive measurement, depth penetration, sensitivity to voids—complement seismic and electromagnetic techniques.

Successful gravity interpretation requires rigorous data reduction (latitude, elevation, terrain corrections), appropriate survey design (station spacing matched to target size), and integration with geological constraints to address inherent non-uniqueness. Modern microgravity surveys achieve precision sufficient to detect meter-scale voids at depths of several meters, opening applications from archaeology to engineering hazard assessment.

Whether investigating the deep structure of Central Asian sedimentary basins or searching for intact kurgan burial chambers, gravity surveying provides unique information about mass distribution that no other geophysical method can replicate.
