---
layout: tutorial
title: "How InSAR and Change Detection Reveal Motion and Stability in Central Asia"
description: "A math-first tutorial on interferometric SAR, coherence, phase, and line-of-sight displacement in Central Asia, with 30 exercises and solutions."
difficulty: advanced
estimated_time: "2-3 hours"
series: sar-remote-sensing
order: 3
---

## Why this tutorial matters

InSAR is one of the most elegant ideas in remote sensing. Instead of comparing brightness alone, it compares the phase of two SAR observations. That phase difference can reveal surface motion, topography, or loss of coherence.

In Central Asia, InSAR is valuable for landslides, glacier motion, subsidence, uplift, reservoir-related deformation, and stability monitoring near infrastructure.

<div class="info-box tip">
  <strong>Key idea</strong>
  InSAR measures phase difference, not motion directly. Displacement is inferred only after you interpret the phase carefully.
</div>

<div class="learning-objectives">
  <div class="learning-objectives-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#1e4f8a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <h3>What you will learn</h3>
  </div>
  <ul>
    <li>How interferometric phase difference between two SAR acquisitions encodes surface displacement</li>
    <li>How to compute line-of-sight displacement from the formula d = &lambda;&Delta;&phi; / 4&pi;</li>
    <li>How to interpret coherence as a measure of scattering stability between two dates</li>
    <li>Why phase unwrapping is necessary and where it can fail</li>
    <li>How to apply InSAR for landslide, glacier, and subsidence monitoring in Central Asia</li>
  </ul>
</div>

<div class="prerequisites">
  <div class="prerequisites-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#8b5e00" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
    <h3>Prerequisites</h3>
  </div>
  <ul>
    <li>Completed Tutorial 02 on Sentinel-1 data processing and calibration</li>
    <li>Understanding of complex numbers, phase, and trigonometric functions</li>
    <li>Familiarity with SAR imaging geometry (slant range, look angle, line of sight)</li>
  </ul>
</div>

<div class="concept-diagram">
  <svg viewBox="0 0 620 320" xmlns="http://www.w3.org/2000/svg" style="max-width: 580px;">
    <!-- Background -->
    <rect x="0" y="0" width="620" height="320" fill="#f8f9fa" rx="8"/>
    <text x="310" y="22" text-anchor="middle" font-family="Inter, sans-serif" font-size="13" font-weight="bold" fill="#1e4f8a">InSAR: Measuring Surface Displacement from Phase Difference</text>

    <!-- Satellite Pass 1 -->
    <rect x="90" y="38" width="60" height="30" rx="4" fill="#1e4f8a"/>
    <text x="120" y="57" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#fff">SAT t&#x2081;</text>
    <!-- Solar panels pass 1 -->
    <rect x="72" y="43" width="18" height="20" rx="2" fill="#1e4f8a" stroke="#c5d5ea" stroke-width="1"/>
    <rect x="150" y="43" width="18" height="20" rx="2" fill="#1e4f8a" stroke="#c5d5ea" stroke-width="1"/>

    <!-- Satellite Pass 2 -->
    <rect x="230" y="38" width="60" height="30" rx="4" fill="#d92b1f"/>
    <text x="260" y="57" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#fff">SAT t&#x2082;</text>
    <!-- Solar panels pass 2 -->
    <rect x="212" y="43" width="18" height="20" rx="2" fill="#d92b1f" stroke="#fdd" stroke-width="1"/>
    <rect x="290" y="43" width="18" height="20" rx="2" fill="#d92b1f" stroke="#fdd" stroke-width="1"/>

    <!-- Radar beams -->
    <line x1="120" y1="68" x2="180" y2="175" stroke="#1e4f8a" stroke-width="1.5" stroke-dasharray="6,3"/>
    <line x1="260" y1="68" x2="200" y2="175" stroke="#d92b1f" stroke-width="1.5" stroke-dasharray="6,3"/>

    <!-- Ground surface -->
    <path d="M 30 200 Q 100 180 180 195 Q 220 202 280 190 Q 360 175 440 195 Q 520 210 600 200" stroke="#8b5e00" stroke-width="2.5" fill="none"/>
    <!-- Ground fill -->
    <path d="M 30 200 Q 100 180 180 195 Q 220 202 280 190 Q 360 175 440 195 Q 520 210 600 200 L 600 230 L 30 230 Z" fill="#f0e8d8" stroke="none"/>

    <!-- Target point -->
    <circle cx="190" cy="195" r="5" fill="#d92b1f" stroke="#111" stroke-width="1.5"/>
    <text x="190" y="215" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#111">Target</text>

    <!-- Deformation arrow -->
    <line x1="190" y1="192" x2="190" y2="178" stroke="#d92b1f" stroke-width="2"/>
    <polygon points="185,180 190,172 195,180" fill="#d92b1f"/>
    <text x="210" y="177" font-family="Inter, sans-serif" font-size="8" fill="#d92b1f" font-weight="bold">d&#x2097;&#x2092;&#x209b;</text>

    <!-- Phase labels: range distances -->
    <text x="125" y="130" font-family="Inter, sans-serif" font-size="9" fill="#1e4f8a" transform="rotate(-50, 125, 130)">R&#x2081;</text>
    <text x="245" y="130" font-family="Inter, sans-serif" font-size="9" fill="#d92b1f" transform="rotate(50, 245, 130)">R&#x2082;</text>

    <!-- Interferogram panel -->
    <rect x="370" y="38" width="230" height="110" rx="6" fill="#fff" stroke="#1e4f8a" stroke-width="1.5"/>
    <text x="485" y="56" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" font-weight="bold" fill="#1e4f8a">Interferogram</text>

    <!-- Phase fringe visualization -->
    <rect x="385" y="65" width="200" height="30" rx="3" fill="none" stroke="#e0ddd8" stroke-width="1"/>
    <!-- Color fringe bands -->
    <rect x="385" y="65" width="25" height="30" rx="3" fill="#1e4f8a" opacity="0.3"/>
    <rect x="410" y="65" width="25" height="30" fill="#1e4f8a" opacity="0.6"/>
    <rect x="435" y="65" width="25" height="30" fill="#1e4f8a" opacity="0.9"/>
    <rect x="460" y="65" width="25" height="30" fill="#d92b1f" opacity="0.3"/>
    <rect x="485" y="65" width="25" height="30" fill="#d92b1f" opacity="0.6"/>
    <rect x="510" y="65" width="25" height="30" fill="#d92b1f" opacity="0.9"/>
    <rect x="535" y="65" width="25" height="30" fill="#1e4f8a" opacity="0.3"/>
    <rect x="560" y="65" width="25" height="30" rx="3" fill="#1e4f8a" opacity="0.6"/>
    <text x="485" y="110" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#68625b">&larr; one fringe = &lambda;/2 &asymp; 2.8 cm LOS displacement &rarr;</text>

    <!-- Formula -->
    <text x="485" y="133" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">&Delta;&phi; = &phi;&#x2082; &minus; &phi;&#x2081;  &rarr;  d = &lambda;&Delta;&phi; / 4&pi;</text>

    <!-- Coherence panel -->
    <rect x="370" y="158" width="230" height="72" rx="6" fill="#fff" stroke="#165d34" stroke-width="1.5"/>
    <text x="485" y="176" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" font-weight="bold" fill="#165d34">Coherence (&gamma;)</text>

    <!-- Coherence bar -->
    <rect x="390" y="186" width="190" height="14" rx="3" fill="none" stroke="#e0ddd8" stroke-width="1"/>
    <defs>
      <linearGradient id="cohGrad" x1="0%" y1="0%" x2="100%" y2="0%">
        <stop offset="0%" style="stop-color:#d92b1f;stop-opacity:0.8"/>
        <stop offset="50%" style="stop-color:#8b5e00;stop-opacity:0.7"/>
        <stop offset="100%" style="stop-color:#165d34;stop-opacity:0.9"/>
      </linearGradient>
    </defs>
    <rect x="390" y="186" width="190" height="14" rx="3" fill="url(#cohGrad)"/>
    <text x="395" y="214" font-family="Inter, sans-serif" font-size="8" fill="#d92b1f">0 (decorrelated)</text>
    <text x="575" y="214" text-anchor="end" font-family="Inter, sans-serif" font-size="8" fill="#165d34">1 (stable)</text>

    <!-- Bottom annotation -->
    <rect x="30" y="245" width="560" height="60" rx="5" fill="#eef2f7" stroke="#1e4f8a" stroke-width="1"/>
    <text x="310" y="264" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" font-weight="bold" fill="#1e4f8a">Phase contributions:</text>
    <text x="310" y="282" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">&Delta;&phi; = &phi;&#x1D41F;&#x1D425;&#x1D41E;&#x1D42D; + &phi;&#x1D42D;&#x1D428;&#x1D429;&#x1D428; + &phi;&#x1D41D;&#x1D422;&#x1D42C;&#x1D429; + &phi;&#x1D41E;&#x1D42D;&#x1D426; + &phi;&#x1D427;&#x1D428;&#x1D422;&#x1D42C;&#x1D41E;</text>
    <text x="310" y="297" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">Only after removing flat-earth, topographic, and atmospheric contributions can displacement be isolated</text>
  </svg>
  <p class="diagram-caption">Figure: InSAR principle — two satellite passes observe the same target. The phase difference between acquisitions encodes surface displacement, while coherence indicates measurement reliability.</p>
</div>


## The interferometric phase idea

If two SAR images observe the same target, a simplified interferometric phase difference can be written as

$$
\Delta \phi = \phi_2 - \phi_1
$$

That phase contains contributions from geometry, topography, atmosphere, noise, and possible ground displacement.

A conceptual decomposition is

$$
\Delta\phi = \phi_{flat} + \phi_{topo} + \phi_{disp} + \phi_{atm} + \phi_{noise}
$$

## Displacement along line of sight

For simple line-of-sight displacement,

$$
d_{LOS} = \frac{\lambda \Delta\phi}{4\pi}
$$

This formula is fundamental. A full $2\pi$ phase cycle corresponds to a displacement of $\lambda/2$ along the radar line of sight.

### Sentinel-1 scale

For C-band with wavelength about $5.6$ cm, one full wrapped cycle corresponds to about $2.8$ cm of LOS displacement.

## Coherence

Coherence measures how stable the scattering is between the two dates. A common conceptual form is

$$
\gamma = \frac{|\langle s_1 s_2^* \rangle|}{\sqrt{\langle |s_1|^2 \rangle \langle |s_2|^2 \rangle}}
$$

where $s_1$ and $s_2$ are complex signals from the two acquisitions.

Values near 1 mean the target stayed phase-stable; lower values suggest temporal change, vegetation, noise, or geometric decorrelation.

## Wrapped phase and unwrapping

Interferometric phase is measured modulo $2\pi$, so the observed value is wrapped. To estimate cumulative displacement or topographic phase, the data often must be unwrapped.

Unwrapping is difficult because errors can propagate when coherence is low.

## What InSAR can reveal in Central Asia

### 1. Landslide motion

Mountain slopes in Kyrgyzstan, Tajikistan, and Kazakhstan can show slow deformation before or after failure.

### 2. Glacier and permafrost change

High mountain cryospheric systems produce deformation and coherence changes that can be scientifically important.

### 3. Subsidence or uplift in sedimentary basins

Groundwater extraction, compaction, or tectonic motion may create measurable LOS change.

### 4. Infrastructure stability

Road corridors, dams, and urbanized areas can be monitored for subtle deformation if coherence is sufficient.

## A practical workflow

### 1. Start with a stability question

Ask:

- Is the ground moving?
- Is coherence dropping in a meaningful way?
- Is this change seasonal, sudden, or persistent?

### 2. Choose image pairs carefully

Longer time gaps may increase decorrelation. Large baselines may hurt coherence. Pair selection is not trivial.

### 3. Check coherence before trusting phase

Low coherence means the interferometric phase is less trustworthy.

### 4. Separate motion from atmosphere and topography when possible

A single interferogram can be suggestive. Time series and multiple pairs are often much more robust.

## Central Asia examples

### Example A: Slow-moving slope above a valley road

A mountain slope shows a repeated fringe pattern and moderate coherence over several pairs. That is a strong reason to investigate deformation risk.

### Example B: Delta plain subsidence

A sedimentary area shows gradual LOS change over time. This could indicate compaction, water-related subsidence, or infrastructure loading.

### Example C: Glacier surface

A glacier may decorrelate rapidly, so coherence itself becomes informative even where displacement extraction is difficult.

## Change detection beyond phase

Even when full InSAR is difficult, SAR still supports change detection through:

- amplitude ratios,
- dB differences,
- coherence loss,
- repeated temporal statistics.

## A compact checklist

<ul class="checklist">
  <li><strong>Coherence:</strong> Is the phase stable enough to trust?</li>
  <li><strong>Pair choice:</strong> Are baseline and time gap reasonable?</li>
  <li><strong>Wrapping:</strong> Could phase ambiguity be misleading?</li>
  <li><strong>Atmosphere:</strong> Could atmospheric delay mimic deformation?</li>
  <li><strong>Geometry:</strong> Remember displacement is along the radar LOS, not full 3D motion.</li>
</ul>

## Exercises

<div class="exercises-section">
  <h2>Easy exercises</h2>

  <div class="exercise"><h4>Easy 1 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What does InSAR compare: brightness only or phase between two SAR images?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Phase between two SAR images.</p></div></div>
  <div class="exercise"><h4>Easy 2 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Write the LOS displacement formula.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$d_{LOS}=\lambda\Delta\phi/(4\pi)$.</p></div></div>
  <div class="exercise"><h4>Easy 3 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What does high coherence usually indicate?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>That the scattering stayed stable between the two acquisitions.</p></div></div>
  <div class="exercise"><h4>Easy 4 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What does low coherence often indicate?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Change, noise, vegetation effects, or geometric decorrelation.</p></div></div>
  <div class="exercise"><h4>Easy 5 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Does a full $2\pi$ phase cycle correspond to $\lambda/2$ or $2\lambda$ of LOS displacement?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$\lambda/2$.</p></div></div>
  <div class="exercise"><h4>Easy 6 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Is wrapped phase measured modulo $2\pi$?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Yes.</p></div></div>
  <div class="exercise"><h4>Easy 7 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Why can glaciers be difficult for InSAR?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Because they often decorrelate quickly.</p></div></div>
  <div class="exercise"><h4>Easy 8 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Does InSAR measure full 3D displacement directly?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>No, it measures displacement projected onto the radar line of sight.</p></div></div>
  <div class="exercise"><h4>Easy 9 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Name one process in Central Asia that InSAR can help study.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Landslides, glacier change, subsidence, uplift, or infrastructure deformation.</p></div></div>
  <div class="exercise"><h4>Easy 10 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Why should coherence be checked before trusting phase?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Because unstable phase can lead to misleading displacement interpretation.</p></div></div>

  <h2>Medium exercises</h2>

  <div class="exercise"><h4>Medium 1 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>If $\Delta\phi = 0$, what is LOS displacement in the simple formula?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Zero.</p></div></div>
  <div class="exercise"><h4>Medium 2 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>For Sentinel-1 with $\lambda\approx 5.6$ cm, what LOS displacement corresponds to one full phase cycle?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$\lambda/2 \approx 2.8$ cm.</p></div></div>
  <div class="exercise"><h4>Medium 3 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>If coherence drops from 0.9 to 0.3, what qualitative change has happened?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>The scene has become much less phase-stable and the interferometric interpretation is less trustworthy.</p></div></div>
  <div class="exercise"><h4>Medium 4 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>Why can atmospheric delay mimic deformation in a single interferogram?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Because atmospheric effects also change phase, even if the ground has not moved.</p></div></div>
  <div class="exercise"><h4>Medium 5 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>If $\Delta\phi=\pi$, what fraction of a full wrapped cycle is that?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Half a cycle.</p></div></div>
  <div class="exercise"><h4>Medium 6 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>Why does long time separation often reduce coherence?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Because the scattering properties of the surface have more time to change.</p></div></div>
  <div class="exercise"><h4>Medium 7 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>What is one reason a road corridor can be a useful InSAR target?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Built surfaces can remain coherent enough to monitor subtle deformation.</p></div></div>
  <div class="exercise"><h4>Medium 8 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>Why are multiple interferograms often stronger evidence than one?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Because they help distinguish persistent motion from one-off atmospheric or noise effects.</p></div></div>
  <div class="exercise"><h4>Medium 9 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>What does the star in $s_2^*$ indicate in the coherence formula?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Complex conjugation.</p></div></div>
  <div class="exercise"><h4>Medium 10 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>Why is amplitude change detection still useful when full InSAR fails?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Because brightness and coherence changes can still reveal physical change even when phase is too unstable for reliable displacement.</p></div></div>

  <h2>Hard exercises</h2>

  <div class="exercise"><h4>Hard 1 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>If $\Delta\phi = 2\pi$, what LOS displacement does the simple formula give?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$d_{LOS}=\lambda(2\pi)/(4\pi)=\lambda/2$.</p></div></div>
  <div class="exercise"><h4>Hard 2 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Why is “phase fringe = deformation” too strong a statement?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Because phase also contains topographic residuals, atmospheric effects, and noise, not just displacement.</p></div></div>
  <div class="exercise"><h4>Hard 3 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Design a three-step reasoning chain for a suspected slow landslide detected by InSAR in Kyrgyzstan.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Check coherence stability, compare multiple interferograms or time-series behavior, and compare with slope, geology, and ground evidence before calling it active deformation.</p></div></div>
  <div class="exercise"><h4>Hard 4 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Why is LOS displacement not the same as vertical displacement?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Because the radar sees motion projected onto its viewing direction, which mixes vertical and horizontal components.</p></div></div>
  <div class="exercise"><h4>Hard 5 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>If a target keeps high coherence but shows no systematic phase trend, what does that suggest?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>It suggests the target is stable and phase-consistent, with little or no detectable displacement trend.</p></div></div>
  <div class="exercise"><h4>Hard 6 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Why can unwrapping errors be dangerous?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Because they can create false cumulative displacement patterns from incorrect cycle counting.</p></div></div>
  <div class="exercise"><h4>Hard 7 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Give a scientific reason InSAR is valuable in Central Asia.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>The region contains active slopes, glaciers, tectonic structures, and infrastructure in mountainous terrain, all of which benefit from repeated deformation monitoring.</p></div></div>
  <div class="exercise"><h4>Hard 8 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>What is one reason time series methods such as PSI or SBAS can outperform a single interferogram?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>They use multiple observations to reduce ambiguity and separate persistent signals from noise or atmosphere more effectively.</p></div></div>
  <div class="exercise"><h4>Hard 9 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>If phase is wrapped, why does seeing only the wrapped map not tell you total displacement directly?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Because the total number of full $2\pi$ cycles is ambiguous until unwrapping or further constraints are applied.</p></div></div>
  <div class="exercise"><h4>Hard 10 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Why should coherence loss itself sometimes be treated as a signal rather than a failure?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Because rapid decorrelation can indicate real physical change such as vegetation growth, snow change, water presence, or unstable surface conditions.</p></div></div>
</div>

## Final takeaway

InSAR reveals whether the ground is stable, shifting, or losing coherence. In Central Asia, that makes it one of the most valuable radar tools for watching motion through time rather than only reading a single image.
