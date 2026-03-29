---
layout: tutorial
title: "How Magnetometry Reveals Buried Archaeological Features in Central Asia"
description: "A math-first tutorial on magnetic surveying, flux density, susceptibility contrast, and archaeological feature detection for Central Asia, with 30 exercises and solutions."
difficulty: intermediate
estimated_time: "2-3 hours"
series: treasure-hunting
order: 5
---

## Why this tutorial matters

Magnetometry is one of the most powerful non-invasive techniques for detecting buried archaeological features. Unlike methods that require active transmission, magnetometry passively measures subtle variations in Earth's magnetic field caused by human activity that altered soil properties centuries or millennia ago.

In Central Asia, magnetometry excels at revealing kurgans, fired structures (kilns, hearths), defensive walls, irrigation canals, and occupation layers. The technique can survey large areas rapidly, making it ideal for the vast landscapes of the steppes and piedmont zones.

<div class="info-box tip">
  <strong>Key idea</strong>
  Magnetometry does not detect "objects" directly. It detects contrasts in magnetic properties between archaeological features and surrounding soil. A fired hearth, a filled ditch, or a collapsed mudbrick wall each create characteristic magnetic signatures.
</div>

## Key concepts in archaeological magnetometry

### 1. Earth's magnetic field

Earth acts as a giant dipole magnet. At any location, the ambient magnetic field has a specific intensity, inclination (dip angle), and declination (deviation from true north). In Central Asia, the total field intensity is typically 50,000–55,000 nanotesla (nT), with inclination around 55°–65°.

### 2. Magnetic flux density

The magnetic flux density $\mathbf{B}$ describes the strength and direction of the magnetic field. It is measured in tesla (T), though archaeological work uses nanotesla (nT) because anomalies are typically 1–100 nT against a 50,000 nT background.

The flux through a surface is

$$
\Phi_B = \int \mathbf{B} \cdot d\mathbf{A}
$$

### 3. Magnetic susceptibility

Magnetic susceptibility $\chi$ measures how easily a material becomes magnetized in an external field. The induced magnetization is

$$
\mathbf{M} = \chi \mathbf{H}
$$

where $\mathbf{H}$ is the magnetic field intensity. For archaeological purposes, the key quantity is the susceptibility contrast $\Delta\chi$ between a feature and its matrix.

### 4. Types of magnetism in soils

**Diamagnetic** materials (quartz, calcite) have $\chi < 0$ and slightly repel magnetic fields. Their effect is negligible.

**Paramagnetic** materials have weak positive $\chi$ that disappears when the external field is removed.

**Ferromagnetic and ferrimagnetic** materials (magnetite, maghemite, hematite) have strong $\chi$ and can retain magnetization. These drive most archaeological signals.

### 5. Remnant magnetism

Remnant magnetization $\mathbf{M}_r$ is magnetization that persists without an external field. Two types matter for archaeology:

**Thermoremanent magnetization (TRM)**: Acquired when materials cool through the Curie temperature in Earth's field. Kilns, hearths, and burned structures exhibit strong TRM.

**Viscous remanent magnetization (VRM)**: Slowly acquired over time as magnetic grains align with Earth's field.

<div class="info-box tip">
  <strong>Key idea</strong>
  Fired features like kilns can produce anomalies of 20–200 nT or more because of TRM. This makes Central Asian ceramic production sites particularly detectable.
</div>

## Mathematical foundations

### The dipole model

A small magnetic anomaly source can be modeled as a magnetic dipole. The field at distance $r$ along the dipole axis is

$$
B_z = \frac{\mu_0}{4\pi} \cdot \frac{2m}{r^3}
$$

where $m$ is the magnetic moment and $\mu_0 = 4\pi \times 10^{-7}$ T·m/A is the permeability of free space.

### Anomaly amplitude and depth

For a buried sphere of radius $a$ with susceptibility contrast $\Delta\chi$ at depth $z$, the maximum vertical field anomaly at the surface is approximately

$$
\Delta B_{max} \approx \frac{4\pi}{3} \cdot \frac{a^3 \Delta\chi B_0}{z^3}
$$

where $B_0$ is the ambient field. This cubic depth dependence explains why deeper features produce progressively weaker signals.

### The half-width rule

For a dipole source, the horizontal distance from peak anomaly to half-maximum ($x_{1/2}$) relates to depth by

$$
z \approx 0.77 \cdot x_{1/2}
$$

This provides a quick depth estimate from profile data.

### Gradient measurements

A vertical gradiometer measures the difference between two sensors separated by distance $\Delta z$:

$$
\frac{\partial B}{\partial z} \approx \frac{B_{upper} - B_{lower}}{\Delta z}
$$

Gradient measurements enhance shallow features and suppress regional variations, which is valuable in areas with magnetic geology.

## Instrumentation

### Fluxgate magnetometers

Fluxgate sensors use a magnetically permeable core driven into saturation by an AC signal. The external field creates an asymmetry in saturation that produces a measurable output. Key characteristics:

- Sensitivity: 0.1–1 nT
- Sampling rate: 10–100 Hz
- Orientation-dependent (must be aligned)
- Commonly used as vertical gradiometers with 0.5–1 m sensor separation

### Proton precession magnetometers

These measure the precession frequency of protons in a hydrogen-rich fluid (water or kerosene) after polarization. The Larmor frequency is

$$
f = \gamma_p B
$$

where $\gamma_p = 42.58$ MHz/T is the proton gyromagnetic ratio. Characteristics:

- Absolute measurements (no drift)
- Sensitivity: ~1 nT
- Lower sampling rate (1–3 readings per second)
- Omnidirectional (no alignment needed)

### Optically pumped (cesium/potassium) magnetometers

These exploit quantum transitions in alkali metal vapors. Characteristics:

- Sensitivity: 0.01–0.1 nT
- High sampling rate: 10–1000 Hz
- Excellent for high-resolution surveys
- More expensive and complex

### Gradiometer configurations

**Vertical gradiometer**: Two sensors stacked vertically; measures $\partial B/\partial z$. Most common for archaeology.

**Horizontal gradiometer**: Two sensors side by side; measures $\partial B/\partial x$. Less common but useful for linear features.

<div class="info-box tip">
  <strong>Key idea</strong>
  Gradiometers are preferred in Central Asia because they suppress diurnal variation and regional geology, emphasizing near-surface archaeological features.
</div>

## Survey design and data collection

### Grid layout

Surveys typically use a regular grid system. Common parameters:

- **Traverse spacing**: 0.5–1 m (closer for high resolution)
- **Sample interval**: 0.125–0.25 m along traverses
- **Grid size**: 20 × 20 m or 30 × 30 m squares for management

### Walking patterns

**Parallel (unidirectional)**: All traverses walked in same direction. Avoids heading errors but slower.

**Zigzag (bidirectional)**: Alternating directions. Faster but requires heading correction.

The heading error in zigzag surveys appears as a "striping" artifact and can be corrected by:

$$
B_{corrected} = B_{raw} - h \cdot (-1)^n
$$

where $h$ is the heading offset and $n$ is the traverse number.

### Diurnal correction

Earth's field varies by 10–50 nT over a day due to ionospheric currents. A base station magnetometer records these changes:

$$
B_{corrected}(t) = B_{survey}(t) - [B_{base}(t) - B_{base,mean}]
$$

### Survey speed

Typical walking speed is 0.5–1 m/s. At 10 Hz sampling and 0.5 m/s, data points are spaced at 0.05 m—often resampled to 0.125 or 0.25 m for processing.

## Data processing and visualization

### Standard processing workflow

1. **Download and format** raw data
2. **Apply diurnal correction** using base station
3. **Remove spikes** (iron debris, fences)
4. **Apply heading correction** if zigzag survey
5. **Grid data** using interpolation (kriging or inverse distance)
6. **Apply filters** (low-pass, high-pass, directional)
7. **Generate visualizations** (grayscale, color maps, 3D surfaces)

### Despiking

Isolated extreme values from surface iron (nails, wire) are identified and removed:

$$
\text{If } |B_i - \bar{B}_{local}| > k\sigma, \text{ then replace } B_i
$$

where $k$ is typically 2–3 standard deviations.

### High-pass filtering

A high-pass filter removes long-wavelength trends:

$$
B_{filtered} = B - B_{smoothed}
$$

where $B_{smoothed}$ is computed using a moving window average.

### Visualization conventions

- **Grayscale**: White = positive anomaly, Black = negative (or reversed)
- **Color scales**: Often bipolar (red-white-blue or similar)
- **Dynamic range**: Typically ±10 to ±50 nT, clipped to enhance subtle features

<div class="info-box tip">
  <strong>Key idea</strong>
  Processing should enhance real features without creating artifacts. Always compare processed images to raw data to ensure anomalies are genuine.
</div>

## Archaeological applications in Central Asia

### 1. Kurgans and burial mounds

Kurgans produce complex magnetic signatures. The mound fill often has higher susceptibility than surrounding soil due to organic enrichment and disturbance. Internal chambers, robber trenches, and stone features create additional contrast.

Typical signatures:
- Central high or dipolar anomaly over chamber
- Ring anomaly at mound edge
- Linear anomalies from robber trenches

### 2. Settlements and occupation layers

Occupation debris enriches soil with magnetically enhanced material: ash, charcoal, fired daub, pottery fragments. Settlement sites often appear as broad positive anomalies (5–20 nT) with superimposed point sources.

### 3. Kilns and hearths

Fired structures are the strongest magnetic sources on archaeological sites. A pottery kiln can produce anomalies of 50–200+ nT due to thermoremanent magnetization. The signature is typically a strong dipole with:

$$
\text{Anomaly shape} \propto \text{firing temperature} \times \text{clay iron content}
$$

### 4. Fortifications and walls

Mudbrick walls may have weak susceptibility contrast when intact, but collapsed and burned walls produce strong anomalies. Ditches filled with magnetically enhanced sediment appear as linear positive features.

### 5. Irrigation systems

Canals and qanats filled with silted sediment often have different susceptibility than surrounding soil. Linear anomalies, sometimes subtle (2–5 nT), can trace ancient water systems across kilometers.

### 6. Metallurgical sites

Slag heaps, furnace remains, and ore processing areas produce strong, often chaotic magnetic signatures. Iron slag is intensely magnetic, while copper smelting debris is more variable.

## Practical interpretation workflow

### Step 1: Regional context

Before interpreting anomalies, understand:
- Expected geology and natural magnetic variation
- Known archaeological site types in the region
- Modern interference sources (pipelines, fences, debris)

### Step 2: Anomaly classification

Classify anomalies by:

**Shape**: Point (pit, hearth), linear (wall, ditch), areal (occupation layer)

**Amplitude**: Weak (<5 nT), moderate (5–20 nT), strong (>20 nT)

**Polarity**: Positive, negative, dipolar

### Step 3: Pattern recognition

Look for:
- Geometric arrangements suggesting structures
- Repeated elements indicating planning
- Alignments with topography or other features

### Step 4: Depth estimation

Use the half-width rule for isolated anomalies:

$$
z \approx 0.77 \cdot x_{1/2}
$$

Apply cautiously—real features are rarely perfect dipoles.

### Step 5: Integration

Compare magnetometry with:
- Satellite/aerial imagery
- Surface collection data
- GPR and EM survey results
- Historical maps and accounts

### Step 6: Targeted verification

Select representative anomalies for ground-truthing through:
- Auger/core sampling
- Test excavation
- Additional geophysical methods

## Central Asia survey considerations

### Soil conditions

Loess soils of Central Asia often have naturally elevated susceptibility from windblown magnetite. Background values of $\chi = 20-100 \times 10^{-5}$ SI are common. This increases noise but also means anthropogenic enhancement stands out.

### Modern interference

Soviet-era infrastructure (irrigation pipes, power lines, buried cables) creates strong interference. Always document modern features before interpretation.

### Seasonal timing

Surveys are best conducted when:
- Ground is dry (wet soil conducts, creates temperature gradients)
- Vegetation is low (better walking, less sensor interference)
- Temperature is moderate (instrument stability)

Spring (April–May) and autumn (September–October) are typically optimal.

### Altitude considerations

At high altitudes in the Tien Shan or Pamirs, total field is slightly lower and inclination steeper. Adjust expectations for anomaly amplitude and shape.

## A compact checklist

<ul class="checklist">
  <li><strong>Instrument calibration:</strong> Check sensor alignment and noise levels before survey</li>
  <li><strong>Base station:</strong> Deploy diurnal reference if surveying for more than 30 minutes</li>
  <li><strong>Grid documentation:</strong> Record corner coordinates, traverse direction, and sample interval</li>
  <li><strong>Walking speed:</strong> Maintain consistent pace for even sampling</li>
  <li><strong>Interference log:</strong> Note all visible iron, fences, and modern features</li>
  <li><strong>Depth estimation:</strong> Apply half-width rule to isolated anomalies</li>
  <li><strong>Multi-method comparison:</strong> Integrate with GPR, EM, and surface data</li>
  <li><strong>Ground truth:</strong> Verify key anomaly interpretations before publication</li>
</ul>

## Exercises

<div class="exercises-section">
  <h2>Easy exercises</h2>

  <div class="exercise"><h4>Easy 1 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What does magnetometry measure: active transmitted signals or passive variations in Earth's magnetic field?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Passive variations in Earth's magnetic field.</p></div></div>

  <div class="exercise"><h4>Easy 2 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What is the approximate total magnetic field intensity in Central Asia in nanotesla?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>50,000–55,000 nT.</p></div></div>

  <div class="exercise"><h4>Easy 3 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What Greek letter represents magnetic susceptibility?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Chi ($\chi$).</p></div></div>

  <div class="exercise"><h4>Easy 4 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What type of magnetization do kilns and hearths acquire when cooling from high temperature?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Thermoremanent magnetization (TRM).</p></div></div>

  <div class="exercise"><h4>Easy 5 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Which type of magnetometer measures proton precession frequency?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Proton precession magnetometer.</p></div></div>

  <div class="exercise"><h4>Easy 6 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What is the advantage of a gradiometer over a single-sensor magnetometer?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>It suppresses regional variations and diurnal changes while enhancing near-surface anomalies.</p></div></div>

  <div class="exercise"><h4>Easy 7 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What artifact appears in zigzag survey data if heading correction is not applied?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Striping pattern parallel to traverse direction.</p></div></div>

  <div class="exercise"><h4>Easy 8 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What causes diurnal variation in magnetic measurements?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Ionospheric currents that vary with solar activity and time of day.</p></div></div>

  <div class="exercise"><h4>Easy 9 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>In grayscale magnetic maps, what does white typically represent?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Positive magnetic anomaly (higher than background).</p></div></div>

  <div class="exercise"><h4>Easy 10 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What seasonal conditions are best for magnetometry surveys in Central Asia?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Dry ground, low vegetation, moderate temperatures—typically spring or autumn.</p></div></div>

  <h2>Medium exercises</h2>

  <div class="exercise"><h4>Medium 1 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>If a dipole source anomaly has half-width $x_{1/2} = 2.6$ m, estimate the depth using the half-width rule.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$z \approx 0.77 \times 2.6 = 2.0$ m.</p></div></div>

  <div class="exercise"><h4>Medium 2 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>If the depth to a spherical anomaly source doubles, by what factor does the surface anomaly amplitude change?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>It decreases by a factor of $2^3 = 8$ (due to the $1/z^3$ dependence).</p></div></div>

  <div class="exercise"><h4>Medium 3 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>A vertical gradiometer has sensors at 0.5 m and 1.0 m above ground. If the lower sensor reads 50,012.3 nT and the upper reads 50,008.7 nT, what is the vertical gradient?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Gradient $= (50,008.7 - 50,012.3) / 0.5 = -7.2$ nT/m.</p></div></div>

  <div class="exercise"><h4>Medium 4 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>Why do fired structures produce stronger magnetic anomalies than unfired mudbrick walls?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Firing converts iron minerals to strongly magnetic forms (magnetite, maghemite) and imparts thermoremanent magnetization.</p></div></div>

  <div class="exercise"><h4>Medium 5 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>What is the Larmor frequency of protons in a 52,000 nT field, given $\gamma_p = 42.58$ MHz/T?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$f = 42.58 \times 52,000 \times 10^{-9} = 2,214$ Hz (about 2.2 kHz).</p></div></div>

  <div class="exercise"><h4>Medium 6 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>In the equation $\mathbf{M} = \chi \mathbf{H}$, what does increasing $\chi$ indicate about a material's magnetic behavior?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Higher susceptibility means the material magnetizes more strongly in response to an external field.</p></div></div>

  <div class="exercise"><h4>Medium 7 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>Why might a robber trench through a kurgan appear as a linear anomaly?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>The trench fill has different susceptibility than surrounding mound material, creating magnetic contrast along its length.</p></div></div>

  <div class="exercise"><h4>Medium 8 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>If survey sampling is at 10 Hz and walking speed is 0.6 m/s, what is the along-track sample spacing?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$0.6 / 10 = 0.06$ m (6 cm).</p></div></div>

  <div class="exercise"><h4>Medium 9 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>Why is despiking necessary before gridding magnetic data?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Isolated extreme values from surface iron contamination would distort interpolation and mask subtle archaeological features.</p></div></div>

  <div class="exercise"><h4>Medium 10 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>What signature would you expect from a circular ditch surrounding a kurgan?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>A ring-shaped positive anomaly if the ditch fill has higher susceptibility than surrounding soil.</p></div></div>

  <h2>Hard exercises</h2>

  <div class="exercise"><h4>Hard 1 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>A buried kiln produces a 120 nT anomaly at 1 m depth. If identical kilns exist at 2 m depth elsewhere, what anomaly amplitude would you expect, and why might actual values differ?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Expected amplitude: $120 / 8 = 15$ nT (due to $1/z^3$ dependence). Actual values may differ due to different firing temperatures, clay composition, preservation state, or non-spherical geometry.</p></div></div>

  <div class="exercise"><h4>Hard 2 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Explain why the half-width depth rule ($z \approx 0.77 \cdot x_{1/2}$) may give inaccurate results for elongated features like walls.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>The rule assumes a compact dipole source. Elongated features have different field geometry—the anomaly profile is wider relative to depth, leading to depth overestimation.</p></div></div>

  <div class="exercise"><h4>Hard 3 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Design a survey strategy to distinguish ancient irrigation canals from Soviet-era pipelines in a Ferghana Valley site.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Strategy: (1) Map modern infrastructure from Soviet-era maps and satellite imagery; (2) Note that iron pipes produce very strong (>100 nT), sharp bipolar anomalies while canal fills produce weaker, broader anomalies; (3) Use historical alignment analysis—ancient canals follow terrain, Soviet pipes follow straight lines; (4) Confirm with GPR to distinguish iron from sediment fill.</p></div></div>

  <div class="exercise"><h4>Hard 4 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>If a proton magnetometer has 1 nT sensitivity and a feature produces a 3 nT anomaly, what signal-to-noise considerations affect detectability?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>With 1 nT instrument noise and 3 nT signal, SNR ≈ 3. Detection is possible but marginal. Factors affecting actual detectability include: diurnal variation (10-50 nT without correction), operator-induced noise, geological noise, and whether multiple measurements can be averaged to improve SNR.</p></div></div>

  <div class="exercise"><h4>Hard 5 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Why might a settlement site appear as a broad positive anomaly superimposed with multiple point anomalies?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>The broad anomaly reflects generally enhanced susceptibility of occupation soil (ash, organics, debris accumulation). Point anomalies represent individual features with stronger magnetization: hearths, kilns, pits with concentrated burned material, or iron objects.</p></div></div>

  <div class="exercise"><h4>Hard 6 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Calculate the magnetic moment $m$ of a dipole that produces a 50 nT anomaly at 2 m distance along its axis. Use $\mu_0 = 4\pi \times 10^{-7}$ T·m/A.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>From $B_z = \frac{\mu_0}{4\pi} \cdot \frac{2m}{r^3}$, solving for $m$: $m = \frac{B_z \cdot 4\pi \cdot r^3}{2\mu_0} = \frac{50 \times 10^{-9} \cdot 4\pi \cdot 8}{2 \cdot 4\pi \times 10^{-7}} = \frac{50 \times 8 \times 10^{-9}}{2 \times 10^{-7}} = 2$ A·m².</p></div></div>

  <div class="exercise"><h4>Hard 7 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>A site shows strong negative anomalies in a region where positive anomalies are expected. What geological or archaeological explanations might account for this?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Possible explanations: (1) Features have lower susceptibility than surrounding magnetically enhanced natural soil; (2) Stone-lined features with diamagnetic limestone; (3) Induced magnetization opposes remnant magnetization at the site's latitude/inclination; (4) Reversed thermoremanent magnetization from specific firing conditions; (5) The features are cuts filled with sterile low-susceptibility material.</p></div></div>

  <div class="exercise"><h4>Hard 8 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Explain how to use the gradient tensor to determine the position and orientation of an elongated magnetic source.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>The gradient tensor contains all first spatial derivatives of the field ($\partial B_x/\partial x$, $\partial B_x/\partial y$, etc.). Principal eigenvectors point toward the source, while eigenvalue ratios indicate source elongation. The strike of a linear feature aligns with the eigenvector corresponding to the smallest eigenvalue magnitude. Full tensor measurement requires multiple sensor configurations or multiple survey lines.</p></div></div>

  <div class="exercise"><h4>Hard 9 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>A magnetometry survey detects a regular grid of anomalies at 3 m spacing. Propose three hypotheses and methods to test each.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Hypotheses: (1) Agricultural features (planting pits, orchard holes)—test with historical agricultural records and GPR for pit shape; (2) Foundation posts or pier bases—test with targeted excavation and comparison to regional architectural typology; (3) Modern interference (buried grid, drainage)—test with utility records, metal detector, and anomaly amplitude analysis (modern features often have stronger, sharper signatures). Compare spacing to known local architectural modules.</p></div></div>

  <div class="exercise"><h4>Hard 10 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Why is multi-method integration (magnetometry + GPR + EM) particularly important for complex sites like Silk Road caravanserais?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Caravanserais contain diverse features: fired structures (kitchens, baths) best seen by magnetometry; stone/brick walls visible in GPR; water systems and organic-rich deposits detectable by EM conductivity. No single method captures all feature types. Integration allows: (1) cross-validation of interpretations; (2) detection of features invisible to any single method; (3) better depth constraints from combining techniques; (4) distinguishing construction materials from fill characteristics.</p></div></div>
</div>

## Final takeaway

Magnetometry transforms invisible magnetic contrasts into maps of human activity. In Central Asia's landscapes—from steppe kurgan fields to oasis settlements—this passive technique reveals fired structures, occupation layers, and buried architecture without excavation. Mathematical understanding of field behavior, susceptibility contrast, and depth relationships enables rigorous interpretation. Combined with complementary methods, magnetometry is indispensable for modern archaeological prospection.
