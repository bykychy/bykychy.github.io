---
layout: tutorial
title: "How Time-Series Analysis Reveals Landscape Dynamics in Central Asia"
description: "A math-first tutorial on SAR time-series methods, seasonal decomposition, anomaly detection, and multi-temporal change analysis for Central Asia, with 30 exercises and solutions."
difficulty: advanced
estimated_time: "3-4 hours"
series: sar-remote-sensing
order: 4
---

## Why this tutorial matters

Single SAR images capture a moment. Time-series analysis reveals process: seasonal irrigation cycles, gradual land subsidence, vegetation phenology, construction activity, and slow landscape change that no single acquisition can show.

Central Asia's dramatic seasonal contrasts—from frozen winter steppe to summer irrigation, from spring floods to autumn dryness—make time-series methods particularly powerful here.

<div class="info-box tip">
  <strong>Key idea</strong>
  Time-series analysis transforms SAR from a snapshot tool into a process-monitoring instrument. The mathematics of temporal statistics separates persistent structure from transient change.
</div>

<div class="learning-objectives">
  <div class="learning-objectives-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#1e4f8a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <h3>What you will learn</h3>
  </div>
  <ul>
    <li>How to build a SAR time-series from co-registered Sentinel-1 acquisitions and compute temporal statistics</li>
    <li>How to decompose backscatter into seasonal patterns and long-term trends using harmonic models</li>
    <li>How to detect anomalies by comparing observed backscatter against expected seasonal behavior</li>
    <li>How temporal coefficient of variation separates stable from dynamic landscape elements</li>
    <li>How to apply CUSUM and z-score methods for abrupt change detection in Central Asian landscapes</li>
  </ul>
</div>

<div class="prerequisites">
  <div class="prerequisites-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#8b5e00" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
    <h3>Prerequisites</h3>
  </div>
  <ul>
    <li>Completed Tutorials 02-03 on Sentinel-1 processing and InSAR fundamentals</li>
    <li>Understanding of basic statistics (mean, standard deviation, z-scores) and trigonometric functions</li>
    <li>Experience with multi-temporal raster data stacking and pixel-wise analysis</li>
  </ul>
</div>

<div class="concept-diagram">
  <svg viewBox="0 0 620 320" xmlns="http://www.w3.org/2000/svg" style="max-width: 580px;">
    <!-- Background -->
    <rect x="0" y="0" width="620" height="320" fill="#f8f9fa" rx="8"/>
    <text x="310" y="22" text-anchor="middle" font-family="Inter, sans-serif" font-size="13" font-weight="bold" fill="#1e4f8a">SAR Time-Series Analysis: From Acquisitions to Anomaly Detection</text>

    <!-- Timeline axis -->
    <line x1="40" y1="270" x2="580" y2="270" stroke="#111" stroke-width="1.5"/>
    <polygon points="580,266 590,270 580,274" fill="#111"/>
    <text x="598" y="274" font-family="Inter, sans-serif" font-size="9" fill="#111">time</text>

    <!-- Y-axis -->
    <line x1="40" y1="55" x2="40" y2="270" stroke="#111" stroke-width="1.5"/>
    <polygon points="36,55 40,45 44,55" fill="#111"/>
    <text x="22" y="160" font-family="Inter, sans-serif" font-size="9" fill="#111" transform="rotate(-90, 22, 160)">&sigma;&sup0; (dB)</text>

    <!-- Season labels -->
    <text x="115" y="288" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#68625b">Winter</text>
    <text x="235" y="288" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#68625b">Spring</text>
    <text x="350" y="288" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#68625b">Summer</text>
    <text x="465" y="288" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#68625b">Autumn</text>

    <!-- Season dividers -->
    <line x1="175" y1="60" x2="175" y2="270" stroke="#e0ddd8" stroke-width="1" stroke-dasharray="4,3"/>
    <line x1="295" y1="60" x2="295" y2="270" stroke="#e0ddd8" stroke-width="1" stroke-dasharray="4,3"/>
    <line x1="410" y1="60" x2="410" y2="270" stroke="#e0ddd8" stroke-width="1" stroke-dasharray="4,3"/>
    <line x1="530" y1="60" x2="530" y2="270" stroke="#e0ddd8" stroke-width="1" stroke-dasharray="4,3"/>

    <!-- Seasonal model curve (sinusoidal) -->
    <path d="M 60 190 Q 95 220 130 210 Q 165 200 200 160 Q 235 120 270 110 Q 305 100 340 120 Q 375 140 410 170 Q 445 200 480 210 Q 515 215 550 195" stroke="#1e4f8a" stroke-width="2.5" fill="none"/>
    <text x="560" y="195" font-family="Inter, sans-serif" font-size="9" fill="#1e4f8a" font-weight="bold">model</text>

    <!-- Individual SAR acquisition points -->
    <circle cx="70" cy="195" r="4" fill="#68625b" stroke="#111" stroke-width="1"/>
    <circle cx="100" cy="215" r="4" fill="#68625b" stroke="#111" stroke-width="1"/>
    <circle cx="130" cy="205" r="4" fill="#68625b" stroke="#111" stroke-width="1"/>
    <circle cx="160" cy="195" r="4" fill="#68625b" stroke="#111" stroke-width="1"/>
    <circle cx="190" cy="170" r="4" fill="#68625b" stroke="#111" stroke-width="1"/>
    <circle cx="220" cy="145" r="4" fill="#68625b" stroke="#111" stroke-width="1"/>
    <circle cx="250" cy="115" r="4" fill="#68625b" stroke="#111" stroke-width="1"/>
    <circle cx="280" cy="108" r="4" fill="#68625b" stroke="#111" stroke-width="1"/>
    <circle cx="310" cy="105" r="4" fill="#68625b" stroke="#111" stroke-width="1"/>
    <circle cx="340" cy="125" r="4" fill="#68625b" stroke="#111" stroke-width="1"/>

    <!-- Anomaly point -->
    <circle cx="370" cy="80" r="6" fill="#d92b1f" stroke="#d92b1f" stroke-width="2"/>
    <line x1="370" y1="86" x2="370" y2="145" stroke="#d92b1f" stroke-width="1.5" stroke-dasharray="3,2"/>
    <text x="378" y="75" font-family="Inter, sans-serif" font-size="9" fill="#d92b1f" font-weight="bold">Anomaly!</text>
    <text x="378" y="86" font-family="Inter, sans-serif" font-size="8" fill="#d92b1f">z-score &gt; threshold</text>

    <!-- Normal points continued -->
    <circle cx="400" cy="165" r="4" fill="#68625b" stroke="#111" stroke-width="1"/>
    <circle cx="430" cy="195" r="4" fill="#68625b" stroke="#111" stroke-width="1"/>
    <circle cx="460" cy="208" r="4" fill="#68625b" stroke="#111" stroke-width="1"/>
    <circle cx="490" cy="215" r="4" fill="#68625b" stroke="#111" stroke-width="1"/>
    <circle cx="520" cy="205" r="4" fill="#68625b" stroke="#111" stroke-width="1"/>
    <circle cx="550" cy="190" r="4" fill="#68625b" stroke="#111" stroke-width="1"/>

    <!-- Mean line -->
    <line x1="60" y1="165" x2="550" y2="165" stroke="#8b5e00" stroke-width="1" stroke-dasharray="8,4"/>
    <text x="560" y="165" font-family="Inter, sans-serif" font-size="8" fill="#8b5e00">mean</text>

    <!-- Legend box -->
    <rect x="60" y="38" width="240" height="46" rx="4" fill="#fff" stroke="#e0ddd8" stroke-width="1"/>
    <circle cx="75" cy="52" r="4" fill="#68625b" stroke="#111" stroke-width="1"/>
    <text x="85" y="56" font-family="Inter, sans-serif" font-size="9" fill="#111">SAR acquisitions (6-12 day repeat)</text>
    <circle cx="75" cy="72" r="5" fill="#d92b1f" stroke="#d92b1f" stroke-width="1.5"/>
    <text x="85" y="76" font-family="Inter, sans-serif" font-size="9" fill="#111">Detected anomaly (deviation from model)</text>

    <!-- Bottom annotation -->
    <rect x="40" y="298" width="540" height="18" rx="3" fill="#eef2f7"/>
    <text x="310" y="311" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#1e4f8a">Harmonic model: &sigma;&sup0;(t) = a&#x2080; + a&#x2081;cos(2&pi;t/T) + b&#x2081;sin(2&pi;t/T) &mdash; residuals reveal anomalous change</text>
  </svg>
  <p class="diagram-caption">Figure: SAR time-series analysis detects anomalies by comparing each acquisition against a seasonal model. Points deviating significantly from expected backscatter indicate landscape change events such as flooding, construction, or land use shifts.</p>
</div>


## The time-series concept in SAR

A SAR time-series is a sequence of co-registered images over the same area, typically acquired at regular intervals (e.g., Sentinel-1's 6-12 day repeat cycle).

For each pixel location $(x, y)$, we have a temporal sequence:

$$
\sigma^0(x, y, t_1), \sigma^0(x, y, t_2), \ldots, \sigma^0(x, y, t_n)
$$

where $\sigma^0$ is backscatter coefficient and $t_i$ are acquisition times.

## Temporal statistics

### Mean backscatter

The temporal mean provides a stable baseline:

$$
\bar{\sigma}^0(x, y) = \frac{1}{n} \sum_{i=1}^{n} \sigma^0(x, y, t_i)
$$

High mean values indicate persistently rough or wet surfaces. Low mean values suggest smooth or dry conditions.

### Temporal standard deviation

Variability over time reveals dynamic behavior:

$$
s(x, y) = \sqrt{\frac{1}{n-1} \sum_{i=1}^{n} \left( \sigma^0(x, y, t_i) - \bar{\sigma}^0(x, y) \right)^2}
$$

High standard deviation indicates seasonal change, irrigation activity, or variable moisture. Low values suggest stable surfaces like bare rock or urban areas.

### Coefficient of variation

The ratio normalizes variability by mean:

$$
CV(x, y) = \frac{s(x, y)}{\bar{\sigma}^0(x, y)}
$$

This helps compare variability across regions with different baseline backscatter.

## Seasonal decomposition

### The seasonal model

Many Central Asian landscapes follow predictable annual cycles. We can model backscatter as:

$$
\sigma^0(t) = \mu + A \cos\left(\frac{2\pi(t - \phi)}{T}\right) + \epsilon(t)
$$

where:
- $\mu$ is the annual mean,
- $A$ is the seasonal amplitude,
- $\phi$ is the phase (timing of peak),
- $T$ is the period (typically 365 days),
- $\epsilon(t)$ is residual variation.

### Fitting seasonal curves

Least-squares fitting finds optimal parameters. For agricultural areas, $A$ tracks vegetation growth. For wetlands, $\phi$ reveals flooding timing.

### Harmonic analysis

More complex patterns use multiple harmonics:

$$
\sigma^0(t) = \mu + \sum_{k=1}^{K} A_k \cos\left(\frac{2\pi k(t - \phi_k)}{T}\right) + \epsilon(t)
$$

The first harmonic captures the main annual cycle. Higher harmonics capture sub-annual patterns like double cropping.

## Anomaly detection

### Z-score method

An observation is anomalous when it deviates significantly from the expected seasonal value:

$$
z(t) = \frac{\sigma^0(t) - \hat{\sigma}^0(t)}{s_{res}}
$$

where $\hat{\sigma}^0(t)$ is the predicted value and $s_{res}$ is residual standard deviation.

Values beyond $|z| > 2$ or $|z| > 3$ flag potential anomalies: sudden flooding, fire scars, construction, or agricultural clearing.

### Cumulative sum (CUSUM) detection

CUSUM tracks accumulated deviations from a baseline:

$$
S_t = \max(0, S_{t-1} + (x_t - \mu_0) - k)
$$

where $\mu_0$ is the baseline mean and $k$ is a slack parameter. A threshold $h$ triggers change detection when $S_t > h$.

CUSUM is powerful for detecting gradual trends or persistent shifts.

## Multi-temporal composites

### RGB time composites

Assign different time periods to color channels:
- **Red**: early period mean backscatter
- **Green**: middle period mean backscatter  
- **Blue**: late period mean backscatter

Color indicates timing of change:
- Yellow (R+G): early-to-mid activity
- Cyan (G+B): mid-to-late activity
- White: persistent brightness
- Black: persistent darkness

### Seasonal color composites

For annual cycles:
- **Red**: summer (June-August) mean
- **Green**: spring (March-May) mean
- **Blue**: winter (December-February) mean

Agricultural land appears green-red (spring-summer growth). Irrigated areas show strong seasonal color variation.

## Coherence time-series

### What coherence measures

Coherence quantifies phase stability between SAR acquisitions:

$$
\gamma = \frac{|\langle s_1 s_2^* \rangle|}{\sqrt{\langle |s_1|^2 \rangle \langle |s_2|^2 \rangle}}
$$

where $s_1$ and $s_2$ are complex SAR signals from two dates.

### Coherence loss mechanisms

- **Temporal decorrelation**: surface change (vegetation growth, soil disturbance)
- **Geometric decorrelation**: baseline effects
- **Volume decorrelation**: penetration into vegetation canopy

### Coherence as change indicator

Low coherence between dates indicates surface disturbance. Time-series of coherence images reveals:
- Agricultural activity (seasonal coherence loss)
- Construction (sudden coherence loss)
- Stable urban areas (persistent high coherence)
- Vegetated slopes (persistent low coherence)

## Central Asia applications

### 1. Irrigation monitoring

The Ferghana Valley, Amu Darya delta, and Syr Darya basin show strong seasonal backscatter cycles tied to irrigation.

**Workflow:**
1. Build multi-year time-series
2. Fit seasonal model to each pixel
3. Map amplitude (irrigation intensity)
4. Map phase (irrigation timing)
5. Detect anomalies (failed irrigation, new fields)

### 2. Glacial lake outburst flood (GLOF) monitoring

Tien Shan and Pamir glacial lakes require monitoring for sudden drainage.

**Workflow:**
1. Build dense time-series over lake area
2. Track lake extent using backscatter thresholds
3. Detect sudden area changes
4. Correlate with precipitation and temperature

### 3. Land subsidence detection

Mining regions, groundwater extraction zones, and soft sediment areas show gradual subsidence.

**Workflow:**
1. Use InSAR time-series (PSI or SBAS methods)
2. Estimate linear velocity at persistent scatterer locations
3. Map subsidence bowls and gradients
4. Correlate with extraction or loading history

### 4. Desertification and vegetation change

Aral Sea basin and steppe margins show long-term drying trends.

**Workflow:**
1. Build multi-year VH/VV time-series
2. Track vegetation proxy (VH-VV ratio or cross-pol)
3. Compute linear trends
4. Map areas of significant decline or recovery

## A practical workflow

### Step 1: Build the time-series stack

1. Download all available Sentinel-1 scenes over your area
2. Apply orbit files and calibration
3. Coregister all scenes to a common geometry
4. Apply terrain correction
5. Export as a multi-band stack (one band per date)

### Step 2: Compute temporal statistics

1. Calculate mean, standard deviation, coefficient of variation
2. Generate temporal composites
3. Visualize as false-color images

### Step 3: Fit seasonal models

1. Apply harmonic regression to each pixel
2. Extract amplitude, phase, and goodness-of-fit
3. Map seasonal parameters

### Step 4: Detect anomalies and trends

1. Compute residuals from seasonal model
2. Apply Z-score or CUSUM detection
3. Flag and investigate anomalies
4. Compute linear trends on residuals

### Step 5: Interpret in context

1. Cross-reference with optical imagery
2. Check against land use maps
3. Validate with field knowledge or ancillary data

## A compact checklist

<ul class="checklist">
  <li><strong>Stack quality:</strong> Are all images properly coregistered and calibrated?</li>
  <li><strong>Temporal sampling:</strong> Is the repeat interval sufficient for your process of interest?</li>
  <li><strong>Seasonal model:</strong> Does a harmonic model capture the dominant pattern?</li>
  <li><strong>Anomaly thresholds:</strong> Are detection thresholds appropriate for noise level?</li>
  <li><strong>Physical interpretation:</strong> Can detected changes be explained by known processes?</li>
</ul>

## Exercises

<div class="exercises-section">
  <h2>Easy exercises</h2>

  <div class="exercise"><h4>Easy 1 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What is the Sentinel-1 repeat cycle over Central Asia?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>6-12 days depending on acquisition mode and location.</p></div></div>
  <div class="exercise"><h4>Easy 2 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Write the formula for temporal mean backscatter.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$\bar{\sigma}^0 = \frac{1}{n} \sum_{i=1}^{n} \sigma^0(t_i)$</p></div></div>
  <div class="exercise"><h4>Easy 3 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What does high temporal standard deviation indicate?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Dynamic behavior such as seasonal change, irrigation activity, or variable moisture.</p></div></div>
  <div class="exercise"><h4>Easy 4 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What does low coherence between two SAR dates indicate?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Surface change or disturbance between the acquisitions.</p></div></div>
  <div class="exercise"><h4>Easy 5 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>In an RGB time composite, what color indicates activity only in the early period?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Red (if early period is assigned to the red channel).</p></div></div>
  <div class="exercise"><h4>Easy 6 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What is the coefficient of variation?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>The ratio of standard deviation to mean: $CV = s/\bar{\sigma}^0$.</p></div></div>
  <div class="exercise"><h4>Easy 7 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Why is coregistration important for time-series analysis?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>So that the same pixel location corresponds to the same ground area across all dates.</p></div></div>
  <div class="exercise"><h4>Easy 8 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Name one Central Asian application of SAR time-series.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Irrigation monitoring, GLOF detection, subsidence mapping, or desertification tracking.</p></div></div>
  <div class="exercise"><h4>Easy 9 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What does the phase parameter $\phi$ represent in a seasonal model?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>The timing of the seasonal peak.</p></div></div>
  <div class="exercise"><h4>Easy 10 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Why are urban areas often high-coherence over time?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Because built structures are stable and do not change between acquisitions.</p></div></div>

  <h2>Medium exercises</h2>

  <div class="exercise"><h4>Medium 1 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>If temporal standard deviation is 2 dB and mean is -10 dB, what is the coefficient of variation?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>CV = 2/10 = 0.2 (note: strictly this should use linear values, but the ratio gives a sense of relative variability).</p></div></div>
  <div class="exercise"><h4>Medium 2 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>Why might an irrigated field show high amplitude in a seasonal model?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Because crop growth and irrigation create strong backscatter differences between wet/vegetated and dry/bare seasons.</p></div></div>
  <div class="exercise"><h4>Medium 3 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>What advantage does CUSUM have over simple threshold detection?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>CUSUM accumulates small persistent deviations, making it sensitive to gradual trends that single-date thresholds would miss.</p></div></div>
  <div class="exercise"><h4>Medium 4 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>In a seasonal RGB composite, what color would a summer-irrigated field appear?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Red-dominant (if summer is assigned to red), because backscatter peaks during summer irrigation and vegetation.</p></div></div>
  <div class="exercise"><h4>Medium 5 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>Why is residual analysis important after fitting a seasonal model?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Residuals reveal deviations from the expected seasonal pattern—anomalies, trends, or events not explained by the regular cycle.</p></div></div>
  <div class="exercise"><h4>Medium 6 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>How does double cropping appear in harmonic analysis?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>As significant second-harmonic amplitude (6-month period) in addition to the first harmonic.</p></div></div>
  <div class="exercise"><h4>Medium 7 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>What causes temporal decorrelation in vegetated areas?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Plant growth, leaf movement, and moisture changes alter scattering geometry between acquisitions.</p></div></div>
  <div class="exercise"><h4>Medium 8 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>Why is terrain correction especially important for time-series in mountainous areas?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Without terrain correction, geometric distortions (layover, shadow) create false temporal patterns when comparing different orbits.</p></div></div>
  <div class="exercise"><h4>Medium 9 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>What Z-score value is commonly used as an anomaly threshold?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>|z| > 2 or |z| > 3, depending on desired sensitivity versus false alarm rate.</p></div></div>
  <div class="exercise"><h4>Medium 10 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>How can you distinguish seasonal variation from a permanent change?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Seasonal variation repeats annually; permanent change shows a shift in the baseline that persists through multiple seasons.</p></div></div>

  <h2>Hard exercises</h2>

  <div class="exercise"><h4>Hard 1 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Design a workflow to detect new construction in a city using SAR time-series.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>1) Build dense backscatter time-series. 2) Compute coherence between consecutive pairs. 3) Identify pixels with sudden coherence drop followed by high coherence recovery. 4) Cross-check with amplitude increase indicating new structures. 5) Validate with optical imagery.</p></div></div>
  <div class="exercise"><h4>Hard 2 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Why might a two-harmonic model fit better than a one-harmonic model for rice paddies?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Rice paddies often show two distinct cycles: flooding/planting and harvest, creating sub-annual backscatter patterns captured by the second harmonic.</p></div></div>
  <div class="exercise"><h4>Hard 3 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Explain how PSI (Persistent Scatterer Interferometry) differs from standard time-series backscatter analysis.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>PSI uses phase information from interferometric pairs to measure millimeter-scale displacement, while backscatter analysis uses amplitude changes to detect surface property changes. PSI requires stable scatterers and measures motion; backscatter analysis works everywhere but doesn't measure displacement.</p></div></div>
  <div class="exercise"><h4>Hard 4 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>If a glacial lake drains over 3 days, what temporal sampling is needed to detect it reliably?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Sampling faster than the event duration—ideally 1-2 day repeat. Sentinel-1's 6-12 day cycle might miss rapid events, requiring constellation data or alternative sensors.</p></div></div>
  <div class="exercise"><h4>Hard 5 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>How can you separate irrigation timing from rainfall effects in a backscatter time-series?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Compare with meteorological data; irrigation shows localized patterns matching field boundaries, while rainfall affects broader regions more uniformly. Irrigation often occurs in dry season when rainfall is minimal.</p></div></div>
  <div class="exercise"><h4>Hard 6 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Why does SBAS (Small Baseline Subset) use a network of interferometric pairs rather than a single master?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>SBAS minimizes temporal and spatial decorrelation by selecting pairs with small baselines. The network approach provides redundancy and allows inversion for displacement time-series even when not all pairs are coherent.</p></div></div>
  <div class="exercise"><h4>Hard 7 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Design a change detection algorithm that distinguishes fire scars from seasonal vegetation changes.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>1) Fit seasonal model to establish expected pattern. 2) Detect sudden negative anomalies (backscatter drop). 3) Check if anomaly persists across multiple acquisitions (fire scars persist; seasonal changes recover). 4) Cross-reference with thermal anomaly data. 5) Check spatial pattern (fire scars have irregular boundaries unlike agricultural patterns).</p></div></div>
  <div class="exercise"><h4>Hard 8 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>What are the limitations of using a linear trend model on a 3-year time-series?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Three years may not distinguish true linear trends from multi-year climate cycles. Interannual variability (drought years vs. wet years) can dominate. Statistical significance requires accounting for autocorrelation in the time-series.</p></div></div>
  <div class="exercise"><h4>Hard 9 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>How would you validate time-series anomaly detection results in a remote area of Central Asia?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>1) Cross-reference with optical time-series (Landsat, Sentinel-2). 2) Check historical reports or news for known events. 3) Use very high resolution imagery (commercial satellites) for spot checks. 4) If possible, conduct field validation. 5) Compare multiple independent detection methods.</p></div></div>
  <div class="exercise"><h4>Hard 10 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Why is the choice of reference period critical in anomaly detection, and how would you select it?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>The reference period defines "normal" behavior. It should span multiple complete seasonal cycles (2+ years minimum), exclude known anomalous periods, and represent the baseline condition you want to detect deviations from. Choice affects both sensitivity and false alarm rate.</p></div></div>
</div>

## Final takeaway

Time-series analysis transforms SAR from static snapshots into a dynamic monitoring system. The mathematics of temporal statistics, seasonal decomposition, and anomaly detection provide rigorous frameworks for separating signal from noise and process from artifact. In Central Asia, where seasonal contrasts are dramatic and remote monitoring is essential, these methods are indispensable.
