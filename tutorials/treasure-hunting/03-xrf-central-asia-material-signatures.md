---
layout: tutorial
title: "How X-Ray Fluorescence Reveals Material Signatures in Central Asia"
description: "A math-first tutorial on XRF, elemental fingerprints, and how material signatures reveal unseen differences in Central Asia, with 30 exercises and solutions."
difficulty: intermediate
estimated_time: "2-3 hours"
series: treasure-hunting
order: 4
---

## Why this tutorial matters

X-ray fluorescence (XRF) does not show hidden geometry the way GPR can, and it does not map landscape texture the way SAR can. What it does reveal is **composition**. It tells you which elements are present strongly enough to be measured, and that makes it a powerful tool for identifying pigments, metal residues, slag, ceramic chemistry, ore-related materials, and geochemical contrasts in soil.

For Central Asia, this matters across archaeology, mining landscapes, metallurgy, and trade studies. A visually ordinary object can contain an extraordinary elemental signature.

<div class="info-box tip">
  <strong>Key idea</strong>
  XRF reveals material difference, not magical provenance by itself. The strength of the method comes from patterns, calibration, and careful comparison.
</div>

<div class="learning-objectives">
  <div class="learning-objectives-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#1e4f8a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <h3>What you will learn</h3>
  </div>
  <ul>
    <li>Understand X-ray fluorescence physics: how incident X-rays eject inner-shell electrons, triggering characteristic photon emission that identifies elements</li>
    <li>Apply Moseley’s law relationship between atomic number and emission frequency to predict characteristic X-ray energies for different elements</li>
    <li>Interpret XRF spectra using counting statistics (σ ≈ √N) and element intensity ratios to compare samples independent of measurement geometry</li>
    <li>Use portable XRF to identify metallurgical residues, ceramic glaze recipes, and soil chemistry anomalies at Central Asian archaeological and craft sites</li>
    <li>Integrate XRF compositional data with SAR, EM, and GPR spatial data to build multi-method evidence chains for material interpretation</li>
  </ul>
</div>

<div class="prerequisites">
  <div class="prerequisites-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#8b5e00" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
    <h3>Prerequisites</h3>
  </div>
  <ul>
    <li>Basic understanding of atomic structure (electron shells, X-ray photon energy)</li>
    <li>Comfort with simple statistics (counting, square-root uncertainty, ratios)</li>
    <li>Awareness of archaeological or geochemical sampling concepts (helpful but not required)</li>
  </ul>
</div>

<div class="concept-diagram">
  <svg viewBox="0 0 620 320" xmlns="http://www.w3.org/2000/svg" style="max-width: 580px;">
    <rect x="0" y="0" width="620" height="320" fill="#f9f8f6" rx="8"/>
    <text x="310" y="22" text-anchor="middle" font-family="Inter, sans-serif" font-size="13" font-weight="bold" fill="#111">X-Ray Fluorescence: From Atomic Emission to Elemental Spectrum</text>
    <defs>
      <marker id="arrXB" markerWidth="8" markerHeight="6" refX="8" refY="3" orient="auto">
        <path d="M0,0 L8,3 L0,6" fill="#1e4f8a"/>
      </marker>
      <marker id="arrXR" markerWidth="8" markerHeight="6" refX="8" refY="3" orient="auto">
        <path d="M0,0 L8,3 L0,6" fill="#d92b1f"/>
      </marker>
      <marker id="arrXG" markerWidth="8" markerHeight="6" refX="8" refY="3" orient="auto">
        <path d="M0,0 L8,3 L0,6" fill="#68625b"/>
      </marker>
    </defs>
    <!-- Left: Atomic process -->
    <rect x="15" y="38" width="215" height="185" fill="#fff" stroke="#68625b" stroke-width="1" rx="4"/>
    <text x="122" y="55" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" font-weight="bold" fill="#1e4f8a">Atomic Process</text>
    <!-- Atom nucleus -->
    <circle cx="122" cy="140" r="10" fill="#1e4f8a"/>
    <text x="122" y="143" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#fff">Z</text>
    <!-- Inner shell (K) -->
    <ellipse cx="122" cy="140" rx="35" ry="35" fill="none" stroke="#68625b" stroke-width="1"/>
    <text x="90" y="115" font-family="Inter, sans-serif" font-size="7" fill="#68625b">K shell</text>
    <!-- Outer shell (L) -->
    <ellipse cx="122" cy="140" rx="60" ry="55" fill="none" stroke="#68625b" stroke-width="0.8" stroke-dasharray="3,2"/>
    <text x="72" y="95" font-family="Inter, sans-serif" font-size="7" fill="#68625b">L shell</text>
    <!-- Electron on K shell -->
    <circle cx="152" cy="120" r="4" fill="#165d34"/>
    <text x="160" y="118" font-family="Inter, sans-serif" font-size="7" fill="#165d34">e⁻</text>
    <!-- Step 1: Incident X-ray (arrow from left) -->
    <line x1="25" y1="130" x2="70" y2="130" stroke="#1e4f8a" stroke-width="2" marker-end="url(#arrXB)"/>
    <text x="48" y="124" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#1e4f8a">Incident</text>
    <text x="48" y="143" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#1e4f8a">X-ray</text>
    <!-- Step 2: Ejected electron -->
    <line x1="156" y1="120" x2="200" y2="85" stroke="#165d34" stroke-width="1.5" stroke-dasharray="3,2"/>
    <circle cx="205" cy="80" r="4" fill="#165d34"/>
    <text x="210" y="75" font-family="Inter, sans-serif" font-size="7" fill="#165d34">ejected e⁻</text>
    <!-- Step 3: Outer electron drops down -->
    <path d="M 100 90 Q 115 100 130 115" fill="none" stroke="#8b5e00" stroke-width="1.5" marker-end="url(#arrXG)"/>
    <text x="88" y="88" font-family="Inter, sans-serif" font-size="7" fill="#8b5e00">L→K</text>
    <text x="88" y="97" font-family="Inter, sans-serif" font-size="7" fill="#8b5e00">transition</text>
    <!-- Step 4: Characteristic X-ray emitted (arrow going out) -->
    <line x1="155" y1="155" x2="205" y2="185" stroke="#d92b1f" stroke-width="2" marker-end="url(#arrXR)"/>
    <text x="200" y="205" font-family="Inter, sans-serif" font-size="8" fill="#d92b1f">Characteristic</text>
    <text x="200" y="215" font-family="Inter, sans-serif" font-size="8" fill="#d92b1f">X-ray out</text>
    <!-- Step labels -->
    <circle cx="35" cy="65" r="8" fill="#1e4f8a"/>
    <text x="35" y="68" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#fff">1</text>
    <text x="48" y="68" font-family="Inter, sans-serif" font-size="8" fill="#111">X-ray hits atom</text>
    <circle cx="35" cy="80" r="8" fill="#165d34"/>
    <text x="35" y="83" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#fff">2</text>
    <text x="48" y="83" font-family="Inter, sans-serif" font-size="8" fill="#111">Inner e⁻ ejected</text>
    <circle cx="135" cy="65" r="8" fill="#8b5e00"/>
    <text x="135" y="68" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#fff">3</text>
    <text x="148" y="68" font-family="Inter, sans-serif" font-size="8" fill="#111">Outer e⁻ fills gap</text>
    <circle cx="135" cy="80" r="8" fill="#d92b1f"/>
    <text x="135" y="83" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#fff">4</text>
    <text x="148" y="83" font-family="Inter, sans-serif" font-size="8" fill="#111">X-ray emitted</text>
    <!-- Right: Energy spectrum -->
    <rect x="245" y="38" width="360" height="185" fill="#fff" stroke="#68625b" stroke-width="1" rx="4"/>
    <text x="425" y="55" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" font-weight="bold" fill="#111">Energy Spectrum with Element Peaks</text>
    <!-- Axes -->
    <line x1="275" y1="65" x2="275" y2="200" stroke="#111" stroke-width="1"/>
    <line x1="275" y1="200" x2="590" y2="200" stroke="#111" stroke-width="1"/>
    <text x="268" y="75" text-anchor="end" font-family="Inter, sans-serif" font-size="8" fill="#68625b">Counts</text>
    <text x="430" y="213" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#68625b">Energy (keV)</text>
    <!-- Background curve -->
    <polyline points="280,185 300,182 320,180 340,178 360,176 380,174 400,173 420,172 440,174 460,176 480,178 500,180 520,183 540,186 560,188 580,190" fill="none" stroke="#68625b" stroke-width="1" stroke-dasharray="2,2"/>
    <!-- Fe Kα peak -->
    <polyline points="335,178 340,170 345,130 350,80 355,130 360,170 365,178" fill="none" stroke="#1e4f8a" stroke-width="2"/>
    <text x="350" y="72" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" font-weight="bold" fill="#1e4f8a">Fe</text>
    <text x="350" y="215" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#68625b">6.4</text>
    <!-- Cu Kα peak -->
    <polyline points="395,174 400,165 405,120 410,95 415,120 420,165 425,174" fill="none" stroke="#d92b1f" stroke-width="2"/>
    <text x="410" y="87" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" font-weight="bold" fill="#d92b1f">Cu</text>
    <text x="410" y="215" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#68625b">8.0</text>
    <!-- Pb Lα peak -->
    <polyline points="460,176 465,168 470,140 475,110 480,140 485,168 490,176" fill="none" stroke="#8b5e00" stroke-width="2"/>
    <text x="475" y="103" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" font-weight="bold" fill="#8b5e00">Pb</text>
    <text x="475" y="215" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#68625b">10.5</text>
    <!-- Sn peak (smaller) -->
    <polyline points="525,183 530,175 535,155 540,145 545,155 550,175 555,183" fill="none" stroke="#165d34" stroke-width="2"/>
    <text x="540" y="138" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" font-weight="bold" fill="#165d34">Sn</text>
    <text x="540" y="215" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#68625b">25.3</text>
    <!-- Counting uncertainty label -->
    <text x="425" y="190" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#68625b">σ ≈ √N (counting uncertainty)</text>
    <!-- Bottom: Applications -->
    <rect x="15" y="240" width="590" height="65" fill="#fff" stroke="#8b5e00" stroke-width="1.5" rx="4"/>
    <text x="310" y="258" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" font-weight="bold" fill="#8b5e00">Central Asia Material Signatures</text>
    <!-- Application boxes -->
    <rect x="30" y="265" width="130" height="28" fill="#f0e8d8" rx="3"/>
    <text x="95" y="278" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#111">Metallurgical slag</text>
    <text x="95" y="289" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#68625b">↑Fe, Cu, As, Zn</text>
    <rect x="172" y="265" width="130" height="28" fill="#f0e8d8" rx="3"/>
    <text x="237" y="278" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#111">Ceramic glazes</text>
    <text x="237" y="289" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#68625b">Pb, Cu, Mn, Sn</text>
    <rect x="314" y="265" width="130" height="28" fill="#f0e8d8" rx="3"/>
    <text x="379" y="278" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#111">Soil chemistry</text>
    <text x="379" y="289" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#68625b">P, Ca, Fe anomalies</text>
    <rect x="456" y="265" width="135" height="28" fill="#f0e8d8" rx="3"/>
    <text x="523" y="278" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#111">Trade comparisons</text>
    <text x="523" y="289" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#68625b">Cu/Fe, Pb/Sn ratios</text>
  </svg>
  <p class="diagram-caption">Figure: X-ray fluorescence principle — an incident X-ray ejects an inner-shell electron, an outer electron fills the vacancy emitting a characteristic X-ray, and the resulting energy spectrum reveals element-specific peaks for material identification.</p>
</div>

## What XRF can reveal in Central Asia

### 1. Metallurgical activity

Copper, lead, iron, arsenic, zinc, and other elements can indicate slag, ore processing, alloying, or workshop residues.

### 2. Pigments and glazes

Ceramic surfaces and decorative materials may show elevated Fe, Cu, Mn, Pb, or Sn depending on recipes and firing practices.

### 3. Soil chemistry around occupation zones

Phosphorus, calcium, iron, and trace metals can differ between activity areas, dumps, and natural background.

### 4. Material selection in trade and craft

Elemental ratios help compare artifacts, ore sources, or ceramic groups even when shapes are similar.

## The physics of XRF

An incoming X-ray ejects an inner-shell electron. When an outer electron drops down to fill the vacancy, the atom emits a characteristic X-ray photon. The photon energy depends on the element.

A simplified relation inspired by Moseley's law is

$$
\sqrt{\nu} = a(Z-b)
$$

where $Z$ is atomic number and $\nu$ is emitted frequency. The exact constants depend on the transition, but the core idea is that each element has a characteristic spectral pattern.

## Reading XRF mathematically

### Intensity and concentration

A first-pass intuition is that line intensity often increases with concentration, though matrix effects and geometry matter:

$$
I \propto C
$$

This is useful as a beginner model, but in real field work you must remember that absorption, enhancement, and surface condition also influence counts.

### Counting statistics

If you detect $N$ counts, the approximate counting uncertainty is often modeled as

$$
\sigma_N \approx \sqrt{N}
$$

So if a peak contains $400$ counts, the approximate noise scale is $20$ counts.

### Ratios

Element ratios can be more stable for comparison than raw counts alone:

$$
R = \frac{I_{Cu}}{I_{Fe}}
$$

If two samples have different exposure or geometry but similar relative composition, ratios can help preserve interpretive meaning.

## A practical workflow

### 1. Ask a composition question

Examples:

- Is this soil patch enriched in metallurgical residue?
- Do two glaze fragments likely belong to the same recipe family?
- Does a suspected workshop area differ chemically from local background?

### 2. Control surface effects

Roughness, corrosion, coatings, and contamination can distort field readings. Clean, stable measurement conditions matter.

### 3. Compare against background, not intuition

A number becomes meaningful only when compared with local baseline or relevant reference samples.

### 4. Use XRF as part of a chain of evidence

XRF is strongest when paired with visual classification, context, and other sensing methods.

## Central Asia examples

### Example A: Slag-rich workshop margin

A dark scatter near an old settlement yields elevated Fe and Cu relative to nearby soil. That does not automatically prove smelting, but it strongly motivates targeted sampling.

### Example B: Ceramic glaze comparison

Two visually similar fragments differ sharply in Pb and Cu peaks. That could indicate different glaze recipes or production sources.

### Example C: Ore-zone screening

In a mineralized landscape, XRF can help distinguish rock pieces or soils enriched in economically or archaeologically relevant elements.

## How XRF connects to SAR, EM, and GPR

- SAR highlights landscape-scale patterns and moisture-sensitive anomalies.
- EM highlights conductivity contrasts and buried disturbance.
- GPR highlights shallow geometry.
- XRF helps explain what sampled material is made of.

## A compact checklist

<ul class="checklist">
  <li><strong>Element:</strong> Which peaks matter for the question you are asking?</li>
  <li><strong>Background:</strong> What is the local baseline composition?</li>
  <li><strong>Counts:</strong> Are the peaks strong relative to counting noise?</li>
  <li><strong>Surface condition:</strong> Could corrosion, dust, or coating distort the reading?</li>
  <li><strong>Comparison:</strong> Are you using ratios or repeated measurements where useful?</li>
</ul>

## Exercises

<div class="exercises-section">
  <h2>Easy exercises</h2>

  <div class="exercise"><h4>Easy 1 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What does XRF primarily reveal: geometry or elemental composition?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Elemental composition.</p></div></div>
  <div class="exercise"><h4>Easy 2 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>If a peak has 100 counts, what is the approximate counting uncertainty scale?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$\sqrt{100}=10$ counts.</p></div></div>
  <div class="exercise"><h4>Easy 3 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Why is a local background sample useful in XRF?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Because numbers become meaningful only when compared with a baseline.</p></div></div>
  <div class="exercise"><h4>Easy 4 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Which is more likely to affect a portable XRF reading: surface corrosion or moonlight?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Surface corrosion.</p></div></div>
  <div class="exercise"><h4>Easy 5 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What kind of photon is emitted in fluorescence: characteristic X-ray or radio wave?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>A characteristic X-ray.</p></div></div>
  <div class="exercise"><h4>Easy 6 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Would XRF alone prove the exact source mine of an artifact?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>No. It can support comparison, but provenance claims require broader evidence.</p></div></div>
  <div class="exercise"><h4>Easy 7 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>If Fe counts are much higher than Cu counts, which raw intensity is larger?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Fe intensity is larger.</p></div></div>
  <div class="exercise"><h4>Easy 8 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Why can element ratios be useful?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>They can help compare samples more robustly than raw counts alone.</p></div></div>
  <div class="exercise"><h4>Easy 9 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What does an incoming X-ray first do in the simple fluorescence model?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>It ejects an inner-shell electron.</p></div></div>
  <div class="exercise"><h4>Easy 10 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Which method is better for wall geometry: GPR or XRF?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>GPR.</p></div></div>

  <h2>Medium exercises</h2>

  <div class="exercise"><h4>Medium 1 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>If a peak has 400 counts, what is the approximate counting uncertainty?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$\sqrt{400}=20$ counts.</p></div></div>
  <div class="exercise"><h4>Medium 2 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>If $I_{Cu}=60$ and $I_{Fe}=120$, what is the ratio $R=I_{Cu}/I_{Fe}$?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$R = 60/120 = 0.5$.</p></div></div>
  <div class="exercise"><h4>Medium 3 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>Why is “high iron” alone a weak archaeological claim?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Because many natural soils and rocks are iron-rich, so comparison with context is essential.</p></div></div>
  <div class="exercise"><h4>Medium 4 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>If counts rise from 100 to 400, by what factor does the counting uncertainty scale increase?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>It increases from 10 to 20, so by a factor of 2.</p></div></div>
  <div class="exercise"><h4>Medium 5 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>Two samples have similar color, but one has strong Pb and the other does not. What does that suggest?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>They may have different compositions or manufacturing recipes despite similar appearance.</p></div></div>
  <div class="exercise"><h4>Medium 6 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>If $I \propto C$ in a simple model, and concentration doubles, what happens to intensity?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>It doubles in the simple proportional model.</p></div></div>
  <div class="exercise"><h4>Medium 7 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>Why should dusty or coated surfaces be treated carefully in field XRF?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Because the surface layer may distort the signal and hide the composition of the underlying material.</p></div></div>
  <div class="exercise"><h4>Medium 8 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>If one sample has Cu/Fe ratio 0.2 and another has 0.8, what does that imply qualitatively?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>The second sample is much richer in Cu relative to Fe.</p></div></div>
  <div class="exercise"><h4>Medium 9 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>What kind of Central Asian context might benefit from XRF: workshop debris or slope shadow in SAR?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Workshop debris.</p></div></div>
  <div class="exercise"><h4>Medium 10 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>Name two interpretive supports that strengthen an XRF reading.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Background comparison and repeated measurements are two strong supports.</p></div></div>

  <h2>Hard exercises</h2>

  <div class="exercise"><h4>Hard 1 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>If a peak rises from 225 to 400 counts, how does the uncertainty change?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>It rises from $\sqrt{225}=15$ to $\sqrt{400}=20$ counts.</p></div></div>
  <div class="exercise"><h4>Hard 2 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Why is “portable XRF gives exact composition” too strong a statement?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Because matrix effects, surface condition, calibration limits, and measurement geometry all affect the result.</p></div></div>
  <div class="exercise"><h4>Hard 3 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>If two samples have counts $(Cu,Fe)=(30,150)$ and $(90,180)$, which has the larger Cu/Fe ratio?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Sample 1 ratio is 0.2. Sample 2 ratio is 0.5. Sample 2 has the larger Cu/Fe ratio.</p></div></div>
  <div class="exercise"><h4>Hard 4 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Design a three-step evidence plan for testing whether a dark scatter is metallurgical slag.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>One good plan: collect repeated XRF readings against local background, inspect texture and magnetism visually, and compare with laboratory or contextual evidence from workshop features.</p></div></div>
  <div class="exercise"><h4>Hard 5 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Why can two visually similar ceramics belong to different production traditions?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Because different raw materials and glaze recipes can produce similar appearance while preserving distinct elemental signatures.</p></div></div>
  <div class="exercise"><h4>Hard 6 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>If intensity is proportional to concentration in a simple model, and sample A has three times the Cu intensity of sample B, what is the concentration ratio in that simple model?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Three to one.</p></div></div>
  <div class="exercise"><h4>Hard 7 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Why does XRF pair well with SAR and EM in archaeological prospecting?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Because SAR and EM help locate interesting patterns, while XRF helps explain the material character of targeted samples from those patterns.</p></div></div>
  <div class="exercise"><h4>Hard 8 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>A soil sample has elevated Fe, Mn, and P relative to background. Why should you still hesitate before assigning a specific activity?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Because several processes could create that chemistry; context, spatial pattern, and comparison data are still needed.</p></div></div>
  <div class="exercise"><h4>Hard 9 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Explain why ratios can sometimes outperform raw counts in comparison.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Ratios can reduce the influence of absolute count level and help highlight relative composition differences between samples.</p></div></div>
  <div class="exercise"><h4>Hard 10 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Give a short scientific reason why XRF is valuable in Central Asia.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>The region contains rich craft, metallurgical, and mineral histories, so fast elemental screening is valuable for distinguishing materials, residues, and geochemical anomalies.</p></div></div>
</div>

## Final takeaway

XRF reveals hidden difference in composition. In Central Asia, that makes it a strong partner to SAR, EM, and GPR: once a place or object becomes interesting, XRF helps explain what the material actually is.
