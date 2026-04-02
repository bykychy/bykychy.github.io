---
layout: tutorial
title: "How Sentinel SAR Data Can Reveal Interesting Patterns in Central Asia"
description: "A math-first tutorial on how Sentinel-1 SAR reveals roughness, moisture, geometry, and repeating structure across Central Asia, with 30 exercises and solutions."
difficulty: intermediate
estimated_time: "2-3 hours"
series: sar-remote-sensing
order: 5
---

## Why this tutorial matters

Sentinel-1 SAR does **not** behave like an X-ray scanner for the ground. It cannot directly show every buried object. What it can do is reveal patterns that optical imagery may miss: moisture contrasts, roughness changes, floodplain texture, irrigation geometry, dune alignment, slope-facing differences, and repeated engineered structures.

That is exactly why SAR is valuable for Central Asia. Much of the region is dry, expansive, mountainous, and seasonally extreme. Radar works day and night, penetrates clouds, and often highlights subtle structure that becomes clearer when you compare seasons, polarizations, or incidence geometries.

<div class="info-box tip">
  <strong>Key idea</strong>
  SAR is best used as a clue generator. It helps you decide where the landscape behaves differently from its surroundings. Then you combine that clue with field knowledge, topography, geology, archaeology, and if available, ground-based electromagnetic or X-ray methods.
</div>

<div class="learning-objectives">
  <div class="learning-objectives-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#1e4f8a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <h3>What you will learn</h3>
  </div>
  <ul>
    <li>How radar backscatter responds differently to smooth water, rough soil, vegetation, and urban structures</li>
    <li>How abandoned irrigation geometry and paleochannels create detectable surface contrasts in SAR imagery</li>
    <li>How alluvial fans, fault-controlled landforms, and tectonic structures appear under different radar look angles</li>
    <li>How seasonal moisture and flood dynamics in Central Asian deltas and lakeshores alter radar returns</li>
    <li>How to use SAR as a clue generator for archaeological, hydrological, and geological investigation</li>
  </ul>
</div>

<div class="prerequisites">
  <div class="prerequisites-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#8b5e00" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
    <h3>Prerequisites</h3>
  </div>
  <ul>
    <li>Completed Tutorial 02 on Sentinel-1 data processing (calibration and terrain correction)</li>
    <li>Basic understanding of radar scattering mechanisms (specular, diffuse, corner reflection)</li>
    <li>Familiarity with Central Asian geography: river basins, mountain fronts, steppe, and desert landscapes</li>
  </ul>
</div>

<div class="concept-diagram">
  <svg viewBox="0 0 620 320" xmlns="http://www.w3.org/2000/svg" style="max-width: 580px;">
    <!-- Background -->
    <rect x="0" y="0" width="620" height="320" fill="#f8f9fa" rx="8"/>
    <text x="310" y="22" text-anchor="middle" font-family="Inter, sans-serif" font-size="13" font-weight="bold" fill="#1e4f8a">Radar Scattering Mechanisms across Central Asian Surfaces</text>

    <!-- Incoming radar wave (top center) -->
    <text x="310" y="48" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#68625b">Incoming radar pulse</text>

    <!-- ===== SCENE 1: Water (Specular) ===== -->
    <rect x="15" y="72" width="140" height="230" rx="6" fill="#fff" stroke="#1e4f8a" stroke-width="1.5"/>
    <text x="85" y="90" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" font-weight="bold" fill="#1e4f8a">Smooth Water</text>
    <text x="85" y="103" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">Specular reflection</text>

    <!-- Water surface -->
    <rect x="30" y="200" width="110" height="25" rx="2" fill="#c5d5ea" stroke="#1e4f8a" stroke-width="1"/>
    <path d="M 35 210 Q 50 205 65 210 Q 80 215 95 210 Q 110 205 125 210" stroke="#1e4f8a" stroke-width="1" fill="none" opacity="0.5"/>

    <!-- Incoming ray -->
    <line x1="60" y1="120" x2="85" y2="198" stroke="#d92b1f" stroke-width="1.5"/>
    <polygon points="82,192 85,198 90,194" fill="#d92b1f"/>
    <!-- Reflected away from sensor -->
    <line x1="85" y1="198" x2="130" y2="130" stroke="#d92b1f" stroke-width="1.5" stroke-dasharray="4,3"/>
    <polygon points="128,136 130,130 124,132" fill="#d92b1f"/>
    <text x="125" y="125" font-family="Inter, sans-serif" font-size="8" fill="#d92b1f">away</text>

    <!-- Backscatter indicator -->
    <rect x="35" y="240" width="90" height="18" rx="3" fill="#e8e6e3"/>
    <rect x="35" y="240" width="14" height="18" rx="3" fill="#1e4f8a" opacity="0.4"/>
    <text x="80" y="253" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#111">Very low &sigma;&sup0;</text>
    <text x="85" y="278" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#1e4f8a">Dark in SAR image</text>

    <!-- ===== SCENE 2: Rough soil (Diffuse) ===== -->
    <rect x="165" y="72" width="140" height="230" rx="6" fill="#fff" stroke="#8b5e00" stroke-width="1.5"/>
    <text x="235" y="90" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" font-weight="bold" fill="#8b5e00">Rough Soil</text>
    <text x="235" y="103" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">Diffuse scattering</text>

    <!-- Rough surface -->
    <path d="M 180 200 L 190 195 L 200 202 L 210 193 L 220 200 L 230 196 L 240 203 L 250 194 L 260 200 L 270 196 L 280 202 L 290 198" stroke="#8b5e00" stroke-width="2" fill="none"/>
    <rect x="180" y="203" width="110" height="22" fill="#f0e8d8" stroke="none"/>

    <!-- Incoming ray -->
    <line x1="210" y1="120" x2="235" y2="197" stroke="#d92b1f" stroke-width="1.5"/>
    <polygon points="232,191 235,197 240,193" fill="#d92b1f"/>
    <!-- Scattered in multiple directions -->
    <line x1="235" y1="197" x2="215" y2="140" stroke="#165d34" stroke-width="1.2"/>
    <polygon points="216,146 215,140 211,144" fill="#165d34"/>
    <line x1="235" y1="197" x2="255" y2="145" stroke="#165d34" stroke-width="1.2"/>
    <polygon points="254,151 255,145 250,148" fill="#165d34"/>
    <line x1="235" y1="197" x2="235" y2="135" stroke="#165d34" stroke-width="1.2"/>
    <polygon points="231,141 235,135 239,141" fill="#165d34"/>
    <text x="225" y="130" font-family="Inter, sans-serif" font-size="8" fill="#165d34">scattered</text>

    <!-- Backscatter indicator -->
    <rect x="185" y="240" width="90" height="18" rx="3" fill="#e8e6e3"/>
    <rect x="185" y="240" width="45" height="18" rx="3" fill="#8b5e00" opacity="0.5"/>
    <text x="235" y="253" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#111">Medium &sigma;&sup0;</text>
    <text x="235" y="278" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#8b5e00">Gray in SAR image</text>

    <!-- ===== SCENE 3: Vegetation (Volume) ===== -->
    <rect x="315" y="72" width="140" height="230" rx="6" fill="#fff" stroke="#165d34" stroke-width="1.5"/>
    <text x="385" y="90" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" font-weight="bold" fill="#165d34">Vegetation</text>
    <text x="385" y="103" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">Volume scattering</text>

    <!-- Vegetation canopy -->
    <ellipse cx="365" cy="180" rx="18" ry="22" fill="#165d34" opacity="0.4"/>
    <ellipse cx="385" cy="175" rx="20" ry="25" fill="#165d34" opacity="0.5"/>
    <ellipse cx="405" cy="180" rx="18" ry="22" fill="#165d34" opacity="0.4"/>
    <!-- Trunks -->
    <line x1="370" y1="200" x2="370" y2="215" stroke="#8b5e00" stroke-width="2"/>
    <line x1="385" y1="198" x2="385" y2="215" stroke="#8b5e00" stroke-width="2"/>
    <line x1="400" y1="200" x2="400" y2="215" stroke="#8b5e00" stroke-width="2"/>
    <!-- Ground -->
    <line x1="330" y1="215" x2="440" y2="215" stroke="#8b5e00" stroke-width="1.5"/>

    <!-- Incoming ray -->
    <line x1="360" y1="120" x2="385" y2="170" stroke="#d92b1f" stroke-width="1.5"/>
    <polygon points="382,164 385,170 390,166" fill="#d92b1f"/>
    <!-- Multiple internal scattering -->
    <line x1="385" y1="170" x2="370" y2="140" stroke="#165d34" stroke-width="1"/>
    <line x1="385" y1="170" x2="395" y2="142" stroke="#165d34" stroke-width="1"/>
    <line x1="385" y1="170" x2="380" y2="138" stroke="#165d34" stroke-width="1"/>

    <!-- Backscatter indicator -->
    <rect x="335" y="240" width="90" height="18" rx="3" fill="#e8e6e3"/>
    <rect x="335" y="240" width="55" height="18" rx="3" fill="#165d34" opacity="0.5"/>
    <text x="385" y="253" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#111">Med-High &sigma;&sup0;</text>
    <text x="385" y="278" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#165d34">Textured in SAR</text>

    <!-- ===== SCENE 4: Urban (Corner) ===== -->
    <rect x="465" y="72" width="140" height="230" rx="6" fill="#fff" stroke="#d92b1f" stroke-width="1.5"/>
    <text x="535" y="90" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" font-weight="bold" fill="#d92b1f">Urban / Structure</text>
    <text x="535" y="103" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">Corner reflection</text>

    <!-- Building with corner reflector -->
    <rect x="510" y="175" width="40" height="40" fill="#68625b" stroke="#111" stroke-width="1"/>
    <line x1="480" y1="215" x2="590" y2="215" stroke="#8b5e00" stroke-width="1.5"/>

    <!-- Double bounce path -->
    <line x1="500" y1="125" x2="520" y2="215" stroke="#d92b1f" stroke-width="1.5"/>
    <polygon points="518,209 520,215 524,210" fill="#d92b1f"/>
    <line x1="520" y1="215" x2="510" y2="195" stroke="#d92b1f" stroke-width="1.5"/>
    <line x1="510" y1="195" x2="490" y2="130" stroke="#d92b1f" stroke-width="1.5"/>
    <polygon points="492,136 490,130 486,135" fill="#d92b1f"/>
    <text x="488" y="126" font-family="Inter, sans-serif" font-size="8" fill="#d92b1f" font-weight="bold">back to</text>
    <text x="488" y="135" font-family="Inter, sans-serif" font-size="8" fill="#d92b1f" font-weight="bold">sensor</text>

    <!-- Backscatter indicator -->
    <rect x="485" y="240" width="90" height="18" rx="3" fill="#e8e6e3"/>
    <rect x="485" y="240" width="80" height="18" rx="3" fill="#d92b1f" opacity="0.5"/>
    <text x="535" y="253" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#111">Very high &sigma;&sup0;</text>
    <text x="535" y="278" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#d92b1f">Bright in SAR image</text>
  </svg>
  <p class="diagram-caption">Figure: Four dominant radar scattering mechanisms encountered in Central Asia. Smooth water reflects energy away (dark pixels), rough soil scatters diffusely, vegetation causes volume scattering, and buildings create strong corner-reflector returns (bright pixels).</p>
</div>


## What Sentinel-1 can reveal in Central Asia

### 1. Abandoned irrigation geometry

In river oases and agricultural margins, old canals and field boundaries may keep different soil moisture or micro-topography from surrounding terrain. Even when they are faint in optical imagery, they can alter radar backscatter enough to stand out in the right season.

### 2. Paleochannels and buried drainage traces

Drylands in Kazakhstan, Uzbekistan, and Turkmenistan contain ancient channels whose sediments differ from surrounding material. A paleochannel may affect:

- surface roughness,
- retained moisture after precipitation,
- vegetation response,
- local drainage,
- and the orientation of fine sediment patterns.

SAR may not image the buried channel itself, but it can reveal the surface expression of that hidden structure.

### 3. Alluvial fans and fault-controlled landforms

In Kyrgyzstan, Tajikistan, and eastern Kazakhstan, mountain fronts produce fan systems, debris cones, and tectonically controlled drainage. Radar is very sensitive to slope orientation relative to the sensor look direction, so fan lobes, scarps, and aligned structures can become visually strong.

### 4. Wetness changes in deltas, lakeshores, and river corridors

The Amu Darya and Syr Darya systems, delta plains, reservoir margins, and ephemeral lakes all show strong seasonal change. A wet soil patch, shallow inundation, or emergent vegetation can change radar returns dramatically.

### 5. Human-made linear patterns

Road beds, pipelines, embankments, canal walls, and rectangular field geometry often appear more regular than natural terrain. A useful rule is simple: when radar brightness organizes itself into repeated, directional, human-scale geometry, you have a strong reason to investigate further.

## The physics behind the image

Sentinel-1 is a **C-band SAR** mission with wavelength around $5.6\ \text{cm}$. That wavelength sets the scale of roughness to which the instrument is sensitive.

If the surface height variation is small relative to the wavelength, the surface behaves more smoothly. If the variation is comparable to the wavelength, scattering becomes stronger and more diffuse.

### A simplified radar equation

The received power is often summarized conceptually as

$$
P_r \propto \frac{P_t G^2 \lambda^2 \sigma}{R^4}
$$

where:

- $P_t$ is transmitted power,
- $G$ is antenna gain,
- $\lambda$ is wavelength,
- $\sigma$ is radar cross section,
- $R$ is range from sensor to target.

For image interpretation, the most important term is the scattering behavior hidden inside $\sigma$. That term changes with roughness, dielectric properties, vegetation structure, orientation, and local geometry.

### Why moisture matters

Soil moisture changes the dielectric constant of the ground. Higher dielectric contrast usually increases backscatter, especially when roughness is also present. This is why freshly wetted channels, irrigated fields, and damp sediment belts often become brighter than nearby dry ground.

### Why slope matters

SAR is side-looking. A slope facing the radar can become bright because it compresses energy into a shorter ground distance. A slope facing away can be dimmer, or in extreme terrain can even become shadowed.

This means that some “interesting bright lines” are geological or topographic rather than archaeological. Interpretation must always check terrain.

## Reading a SAR image mathematically

### Linear scale and decibels

Sentinel products are often analyzed in linear power or in decibels:

$$
\sigma^0_{dB} = 10 \log_{10}(\sigma^0)
$$

If $\sigma^0 = 0.01$, then

$$
10 \log_{10}(0.01) = -20\ \text{dB}
$$

If one target is twice as bright in linear power as another, the dB difference is:

$$
10 \log_{10}(2) \approx 3.01\ \text{dB}
$$

That small formula matters a lot. Many useful changes in SAR interpretation are only a few dB, but that can still represent meaningful physical contrast.

### Ratios for interpretation

A simple change ratio between two dates can be written as

$$
\rho = \frac{\sigma^0_{t_2}}{\sigma^0_{t_1}}
$$

and in decibels:

$$
\Delta_{dB} = 10\log_{10}(\sigma^0_{t_2}) - 10\log_{10}(\sigma^0_{t_1})
$$

If a dry river trace becomes wet after precipitation, $\rho > 1$ and $\Delta_{dB}$ is positive.

### Temporal statistics

For a pixel measured over $n$ acquisition dates, the temporal mean is

$$
\mu = \frac{1}{n}\sum_{i=1}^{n} x_i
$$

and the temporal variance is

$$
\mathrm{Var}(x) = \frac{1}{n}\sum_{i=1}^{n}(x_i-\mu)^2
$$

These simple statistics are surprisingly powerful:

- low mean + low variance may indicate consistently smooth dry surfaces,
- moderate mean + high variance may indicate seasonal agriculture or wetness change,
- high mean + low variance may indicate persistent rough or built structure.

## A practical workflow for Central Asia

### 1. Start with a physical question

Do not begin with “find treasure.” Begin with a measurable hypothesis:

- Where are moisture-retaining linear traces?
- Where do engineered rectangular patterns survive in dry terrain?
- Which parts of an alluvial fan change strongly after rainfall?
- Where are persistent bright returns aligned with known infrastructure?

### 2. Choose the comparison that makes the phenomenon visible

Useful comparisons include:

- dry season vs wet season,
- ascending vs descending passes,
- VV vs VH polarization,
- current scene vs temporal median,
- radar image vs slope or hillshade.

### 3. Check whether the pattern is geometric, seasonal, or topographic

Ask three questions:

1. Is the pattern aligned with terrain?
2. Does it repeat with season or moisture?
3. Does it form a human-like geometry such as rectangles, straight segments, or parallel spacing?

When all three answers are considered, the interpretation becomes much stronger.

### 4. Use SAR as part of a sensor stack

SAR becomes more convincing when paired with:

- DEM and slope maps,
- optical imagery,
- old maps,
- field electromagnetic survey,
- portable XRF or laboratory analysis where material questions matter.

<div class="info-box warning">
  <strong>Important limit</strong>
  Sentinel-1 C-band is excellent for landscape interpretation, but it is not a guaranteed detector of buried artifacts. If you present SAR results as direct proof of hidden objects, you will over-interpret the data.
</div>

## Central Asia interpretation examples

### Example A: Paleochannel hunting in an arid basin

Imagine a dry basin in Uzbekistan. Optical imagery shows only faint tonal variation. After a rainfall event, a curving band becomes brighter in SAR than adjacent sediment.

Possible explanation:

- finer sediment retained more moisture,
- the paleochannel surface is rougher,
- shallow vegetation responded first along the former drainage path.

A smart next step is not excavation. It is checking whether the bright curve:

- matches topographic subtle lows,
- repeats across multiple wet dates,
- and aligns with regional drainage logic.

### Example B: Fan lobes below a mountain front

In Kyrgyzstan, a fan surface facing the radar may appear bright while an adjacent lobe appears dim. That contrast alone does not prove a material change. It may come from look geometry.

To test the hypothesis, compare:

- opposite orbit directions,
- slope aspect maps,
- and wet versus dry season scenes.

If the contrast flips with orbit direction, geometry is likely dominant. If it persists across directions and varies with season, moisture or roughness differences may be more important.

### Example C: Human-made linear traces

Suppose a site in southern Kazakhstan contains faint, repeated linear traces. In SAR they appear:

- regularly spaced,
- partially rectangular,
- stronger after irrigation,
- and aligned differently from natural drainage.

That combination is much more persuasive than brightness alone. It suggests surface engineering or anthropogenic land organization worth comparing against historical imagery and cadastral patterns.

## How this connects to treasure hunting and “showing the unseen”

The unseen is often revealed indirectly:

- SAR reveals **surface behavior** that hints at hidden structure.
- Electromagnetic survey reveals **conductivity and induced current response**.
- GPR reveals **subsurface reflectors and geometry**.
- XRF reveals **elemental composition**.

Each sensor sees a different projection of reality. A serious investigator learns the transfer function of each sensor instead of expecting one instrument to answer everything.

## A compact mathematical checklist

<ul class="checklist">
  <li><strong>Wavelength:</strong> Is the target scale comparable to the radar wavelength or much smaller?</li>
  <li><strong>Units:</strong> Are you comparing linear values or dB values?</li>
  <li><strong>Geometry:</strong> Could slope or layover explain the contrast?</li>
  <li><strong>Seasonality:</strong> Does the pattern persist or only appear after moisture change?</li>
  <li><strong>Regularity:</strong> Is the pattern natural, engineered, or ambiguous?</li>
  <li><strong>Cross-validation:</strong> What does terrain, optical imagery, or field data say?</li>
</ul>

## Exercises

<div class="exercises-section">
  <h2>Easy exercises</h2>
  <p>These build fluency with units, dB conversion, wavelength thinking, and basic interpretation.</p>

  <div class="exercise">
    <h4>Easy 1 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
    <p>If $\sigma^0 = 0.1$, what is the value in dB?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>$10\log_{10}(0.1) = -10\ \text{dB}$.</p></div>
  </div>

  <div class="exercise">
    <h4>Easy 2 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
    <p>If one pixel has linear power 0.02 and another has 0.04, which is brighter and by how many dB?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>The second is brighter. The ratio is $0.04/0.02 = 2$, so the difference is about $3.01\ \text{dB}$.</p></div>
  </div>

  <div class="exercise">
    <h4>Easy 3 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
    <p>Why can SAR acquire useful imagery at night?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Because SAR is an active sensor: it transmits its own microwave energy and measures the return.</p></div>
  </div>

  <div class="exercise">
    <h4>Easy 4 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
    <p>What physical property often increases when soil gets wetter and therefore can increase backscatter?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>The dielectric constant usually increases with moisture, which can increase the radar return.</p></div>
  </div>

  <div class="exercise">
    <h4>Easy 5 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
    <p>Sentinel-1 uses C-band with wavelength about $5.6$ cm. Would a perfectly smooth water surface usually appear bright or dark?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Usually dark, because smooth water reflects energy away from the radar in a specular manner.</p></div>
  </div>

  <div class="exercise">
    <h4>Easy 6 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
    <p>A band is visible in SAR after rainfall but barely visible in dry conditions. Name one plausible interpretation.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>The band may retain more moisture than surrounding ground, perhaps due to a paleochannel, canal trace, or sediment contrast.</p></div>
  </div>

  <div class="exercise">
    <h4>Easy 7 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
    <p>What does a positive $\Delta_{dB}$ between two dates mean?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>It means the target is brighter in the second date than in the first date.</p></div>
  </div>

  <div class="exercise">
    <h4>Easy 8 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
    <p>Why should a radar-bright slope be compared with a DEM or slope map?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Because topography can create brightness changes that have nothing to do with materials or archaeology.</p></div>
  </div>

  <div class="exercise">
    <h4>Easy 9 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
    <p>Write the formula converting linear backscatter $\sigma^0$ to decibels.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>$\sigma^0_{dB} = 10\log_{10}(\sigma^0)$.</p></div>
  </div>

  <div class="exercise">
    <h4>Easy 10 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
    <p>Which is a stronger clue of human activity: one isolated bright pixel, or many regularly spaced linear features?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Many regularly spaced linear features, because repeated geometry is more diagnostic than a single bright point.</p></div>
  </div>

  <h2>Medium exercises</h2>
  <p>These focus on calculations, comparisons, and structured interpretation.</p>

  <div class="exercise">
    <h4>Medium 1 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
    <p>Convert $\sigma^0 = 0.005$ to dB.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>$10\log_{10}(0.005) \approx -23.01\ \text{dB}$.</p></div>
  </div>

  <div class="exercise">
    <h4>Medium 2 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
    <p>A pixel changes from $-15$ dB to $-9$ dB. By what factor did the linear power increase?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>The change is $6$ dB, so the factor is $10^{6/10} \approx 3.98$. The power is about four times larger.</p></div>
  </div>

  <div class="exercise">
    <h4>Medium 3 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
    <p>Given temporal values $[-14,-13,-15,-14]$ dB, what is the mean in dB?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>The arithmetic mean is $(-14-13-15-14)/4 = -14$ dB.</p></div>
  </div>

  <div class="exercise">
    <h4>Medium 4 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
    <p>Why is a repeated seasonal brightening along a curved trace more persuasive than a single-date brightening?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Because repeatability supports a stable physical cause such as retained moisture or persistent structure, rather than noise or one-off imaging conditions.</p></div>
  </div>

  <div class="exercise">
    <h4>Medium 5 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
    <p>A target has values 0.03, 0.06, and 0.03 in linear units across three dates. Compute the temporal mean.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>$\mu = (0.03 + 0.06 + 0.03)/3 = 0.04$.</p></div>
  </div>

  <div class="exercise">
    <h4>Medium 6 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
    <p>Using the values in Medium 5, compute the variance with the formula in the tutorial.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>The deviations are $-0.01, 0.02, -0.01$. Squares are $0.0001, 0.0004, 0.0001$. Variance is $(0.0006)/3 = 0.0002$.</p></div>
  </div>

  <div class="exercise">
    <h4>Medium 7 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
    <p>If a bright feature disappears when you view the same area from the opposite orbit direction, what is a likely explanation?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Look geometry or slope orientation is likely dominating the signal rather than a stable material contrast.</p></div>
  </div>

  <div class="exercise">
    <h4>Medium 8 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
    <p>Why might a rectangular pattern that becomes brighter after irrigation be especially interesting?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Because the geometry suggests human organization, while the wetting response suggests a distinct soil or surface structure that stores moisture differently.</p></div>
  </div>

  <div class="exercise">
    <h4>Medium 9 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
    <p>A pixel changes from linear power 0.02 to 0.01. What is $\Delta_{dB}$?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>$10\log_{10}(0.01)-10\log_{10}(0.02)=10\log_{10}(0.5)\approx -3.01$ dB.</p></div>
  </div>

  <div class="exercise">
    <h4>Medium 10 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
    <p>Give two reasons why SAR should be combined with optical imagery or field survey before making archaeological claims.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Topography can mimic targets, and SAR alone does not identify composition or depth reliably. Optical or field data helps validate the interpretation.</p></div>
  </div>

  <h2>Hard exercises</h2>
  <p>These ask you to reason from formulas, ambiguity, and competing interpretations.</p>

  <div class="exercise">
    <h4>Hard 1 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
    <p>A feature changes from $-18$ dB in dry season to $-11$ dB after rainfall. What is the linear change factor, and what might that suggest physically?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>The increase is $7$ dB, so the factor is $10^{7/10} \approx 5.01$. The return is about five times stronger, suggesting much higher wetness, roughness activation, vegetation response, or a combination.</p></div>
  </div>

  <div class="exercise">
    <h4>Hard 2 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
    <p>Suppose two fan surfaces have the same material, but one faces the radar and one faces away. Why can their backscatter still differ strongly?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Because local incidence angle changes with slope orientation. The facing slope compresses the signal geometry and can appear brighter even without material change.</p></div>
  </div>

  <div class="exercise">
    <h4>Hard 3 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
    <p>You have linear values $[0.01, 0.01, 0.04, 0.04]$. Compute the mean and variance.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Mean is $(0.01+0.01+0.04+0.04)/4 = 0.025$. Deviations are $-0.015,-0.015,0.015,0.015$; squares sum to $4 \times 0.000225 = 0.0009$. Variance is $0.0009/4 = 0.000225$.</p></div>
  </div>

  <div class="exercise">
    <h4>Hard 4 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
    <p>Why is “SAR can see buried treasure directly” a poor scientific claim for Sentinel-1 C-band data?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Because Sentinel-1 primarily measures microwave interaction with the surface and near-surface expression of terrain, roughness, moisture, and vegetation. Buried objects influence SAR only indirectly, if at all, through surface effects.</p></div>
  </div>

  <div class="exercise">
    <h4>Hard 5 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
    <p>A candidate lineament is bright in VV, weak in VH, stable over time, and aligned with slope. Is the better first hypothesis geological, hydrological, or anthropogenic?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Geological or topographic is the better first hypothesis, because alignment with slope and temporal stability are consistent with terrain-controlled scattering.</p></div>
  </div>

  <div class="exercise">
    <h4>Hard 6 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
    <p>Propose a three-layer evidence test for a suspected anthropogenic rectangular pattern in an arid basin.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>One good answer: compare wet and dry SAR scenes, compare with DEM/slope to remove terrain explanations, and compare with optical or historical imagery to test for persistent human geometry.</p></div>
  </div>

  <div class="exercise">
    <h4>Hard 7 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
    <p>A target has dB values $[-12,-12,-12,-6]$. What does that temporal pattern suggest?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>It suggests a mostly stable surface with one strong event-driven change, perhaps wetting, flooding, disturbance, or geometry change in one acquisition.</p></div>
  </div>

  <div class="exercise">
    <h4>Hard 8 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
    <p>If a pattern is bright only after rainfall, curvilinear, and follows subtle topographic lows, what hidden structure could be consistent with the evidence?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>A paleochannel or former drainage path is a strong candidate because it can retain moisture and follow low-relief curving topography.</p></div>
  </div>

  <div class="exercise">
    <h4>Hard 9 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
    <p>Why is regular spacing mathematically useful in interpretation, not just visually attractive?</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>Because repeated spacing increases the probability of an organized generating process. Inference becomes stronger when the scene departs from random natural texture and shows structured periodicity.</p></div>
  </div>

  <div class="exercise">
    <h4>Hard 10 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
    <p>Design a short argument for why Sentinel-1 is especially useful in Central Asia even though it does not directly image buried objects.</p>
    <button class="solution-toggle">▶ Show Solution</button>
    <div class="solution-block"><p>One strong argument is that Central Asia has large cloud-prone and seasonally variable areas, strong relief, many dryland surfaces, and widespread irrigation and floodplain systems. Sentinel-1 gives repeat, all-weather, day-night observations that make surface expressions of hidden structure easier to compare over time.</p></div>
  </div>
</div>

## Final takeaway

Sentinel-1 helps you detect the **behavior** of landscapes, not magic underground truth. In Central Asia, that is often enough to reveal paleochannels, wetness patterns, engineered surfaces, slope-controlled landforms, and other subtle clues that deserve deeper investigation.

If you want to master this topic, practice three habits:

1. convert between linear and dB values comfortably,
2. always test geometry versus material explanations,
3. treat SAR as one layer in a larger evidence stack.
