---
layout: tutorial
title: "How Borehole Logging Reveals Subsurface Stratigraphy in Central Asia"
description: "A practical tutorial on gamma-ray, resistivity, sonic, and other downhole logging methods for geological characterization, with 30 exercises and solutions."
difficulty: advanced
estimated_time: "3-4 hours"
series: treasure-hunting
order: 16
---

## Why borehole logging matters

Surface geophysics provides spatial coverage but inherent ambiguity. Borehole geophysical logging delivers **ground truth**—direct, continuous measurements of formation properties at depth. In Central Asia's complex sedimentary basins, from the Fergana Valley to the Caspian Depression, borehole logs are essential for calibrating surface surveys, correlating stratigraphy between wells, and characterizing aquifers, hydrocarbon reservoirs, and mineral deposits.

<div class="info-box tip">
  <strong>Key idea</strong>
  Borehole logs transform surface geophysical interpretations from "possible" to "probable" by providing vertical resolution (centimeter-scale) that no surface method can match.
</div>

### The logging principle

A **logging tool (sonde)** is lowered into a borehole on a wireline cable. As it moves upward at controlled speed (typically 300–600 m/hr), sensors continuously record physical properties of the surrounding formation. The result is a **log curve**—a continuous trace of measurement versus depth.

### Why Central Asia needs borehole logging

| Challenge | How logging helps |
|-----------|-------------------|
| Complex alluvial sequences | Distinguish sand, silt, clay layers |
| Saline aquifers | Separate formation water from fresh water |
| Loess-paleosol sequences | Identify buried soils for archaeology |
| Hydrocarbon exploration | Evaluate porosity, saturation |
| Mineral deposits | Detect ore-bearing zones |

### Depth reference

All logs reference a **datum**—typically ground level (GL) or kelly bushing (KB). Ensure consistent datum when correlating wells:

$$z_{true} = z_{measured} + (KB - GL)$$

<div class="learning-objectives">
  <div class="learning-objectives-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#1e4f8a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <h3>What you will learn</h3>
  </div>
  <ul>
    <li>Interpret gamma-ray logs to estimate shale volume and distinguish clean sands, shales, and evaporites in Central Asian sedimentary sequences</li>
    <li>Analyse resistivity logs (normal, lateral, induction) to identify formation fluids and calculate water saturation using Archie's equation</li>
    <li>Use sonic (acoustic) logs to derive porosity, compute synthetic seismograms, and calibrate surface seismic data</li>
    <li>Combine density, neutron, and caliper logs for lithology identification and cross-plot analysis of mineralogy and pore fluids</li>
    <li>Correlate multiple borehole logs across wells and integrate them with surface geophysics to build 3D stratigraphic models</li>
  </ul>
</div>

<div class="prerequisites">
  <div class="prerequisites-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#8b5e00" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
    <h3>Prerequisites</h3>
  </div>
  <ul>
    <li>Foundational knowledge of sedimentary geology: stratigraphy, lithology, and depositional environments</li>
    <li>Understanding of basic petrophysics: porosity, permeability, fluid saturation, and rock-physics relationships</li>
    <li>Familiarity with at least one surface geophysical method (seismic, resistivity, or magnetics) for integration context</li>
    <li>Ability to read well-log displays (depth tracks, linear/logarithmic scales, multi-curve overlays)</li>
  </ul>
</div>

<div class="concept-diagram">
  <svg viewBox="0 0 620 320" xmlns="http://www.w3.org/2000/svg" style="max-width: 580px;">
    <rect x="0" y="0" width="620" height="320" fill="#f9f8f6" rx="8"/>
    <text x="310" y="22" text-anchor="middle" font-family="Inter, sans-serif" font-size="13" font-weight="bold" fill="#111">Borehole Geophysical Logging</text>

    <!-- Ground surface -->
    <rect x="20" y="42" width="600" height="4" fill="#8b5e00"/>
    <text x="45" y="39" font-family="Inter, sans-serif" font-size="8" fill="#8b5e00">Ground Surface</text>

    <!-- Borehole shaft -->
    <rect x="68" y="46" width="24" height="260" fill="#c8dced" stroke="#1e4f8a" stroke-width="1" rx="2"/>

    <!-- Wireline cable -->
    <line x1="80" y1="35" x2="80" y2="200" stroke="#111" stroke-width="1.5"/>
    <!-- Winch at top -->
    <circle cx="80" cy="35" r="6" fill="#68625b" stroke="#111" stroke-width="1"/>
    <text x="80" y="28" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#111">winch</text>

    <!-- Logging sonde -->
    <rect x="72" y="200" width="16" height="60" rx="3" fill="#1e4f8a" stroke="#111" stroke-width="1"/>
    <circle cx="80" cy="210" r="3" fill="#d92b1f"/>
    <circle cx="80" cy="225" r="3" fill="#165d34"/>
    <circle cx="80" cy="240" r="3" fill="#8b5e00"/>
    <text x="80" y="272" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#1e4f8a">sonde</text>

    <!-- Lithological layers -->
    <!-- Layer 1: Sand -->
    <rect x="20" y="46" width="48" height="45" fill="#f5e6a3" opacity="0.6"/>
    <rect x="92" y="46" width="28" height="45" fill="#f5e6a3" opacity="0.6"/>
    <text x="34" y="72" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#8b5e00">Sand</text>

    <!-- Layer 2: Shale -->
    <rect x="20" y="91" width="48" height="40" fill="#a0a090" opacity="0.4"/>
    <rect x="92" y="91" width="28" height="40" fill="#a0a090" opacity="0.4"/>
    <line x1="20" y1="100" x2="48" y2="100" stroke="#68625b" stroke-width="0.4"/>
    <line x1="25" y1="108" x2="45" y2="108" stroke="#68625b" stroke-width="0.4"/>
    <line x1="20" y1="116" x2="48" y2="116" stroke="#68625b" stroke-width="0.4"/>
    <text x="34" y="110" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#111">Shale</text>

    <!-- Layer 3: Limestone -->
    <rect x="20" y="131" width="48" height="50" fill="#d4cdb8" opacity="0.5"/>
    <rect x="92" y="131" width="28" height="50" fill="#d4cdb8" opacity="0.5"/>
    <text x="34" y="160" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#8b5e00">Limestone</text>

    <!-- Layer 4: Sand (reservoir) -->
    <rect x="20" y="181" width="48" height="40" fill="#f5e6a3" opacity="0.7"/>
    <rect x="92" y="181" width="28" height="40" fill="#f5e6a3" opacity="0.7"/>
    <text x="34" y="202" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#8b5e00">Sand</text>
    <text x="34" y="212" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#8b5e00">(reservoir)</text>

    <!-- Layer 5: Shale -->
    <rect x="20" y="221" width="48" height="35" fill="#a0a090" opacity="0.4"/>
    <rect x="92" y="221" width="28" height="35" fill="#a0a090" opacity="0.4"/>
    <text x="34" y="242" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#111">Shale</text>

    <!-- Layer 6: Basement -->
    <rect x="20" y="256" width="48" height="50" fill="#8a8078" opacity="0.4"/>
    <rect x="92" y="256" width="28" height="50" fill="#8a8078" opacity="0.4"/>
    <text x="34" y="280" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#111">Basement</text>

    <!-- Layer boundaries -->
    <line x1="20" y1="91" x2="120" y2="91" stroke="#8b5e00" stroke-width="0.5" stroke-dasharray="2,2"/>
    <line x1="20" y1="131" x2="120" y2="131" stroke="#8b5e00" stroke-width="0.5" stroke-dasharray="2,2"/>
    <line x1="20" y1="181" x2="120" y2="181" stroke="#8b5e00" stroke-width="0.5" stroke-dasharray="2,2"/>
    <line x1="20" y1="221" x2="120" y2="221" stroke="#8b5e00" stroke-width="0.5" stroke-dasharray="2,2"/>
    <line x1="20" y1="256" x2="120" y2="256" stroke="#8b5e00" stroke-width="0.5" stroke-dasharray="2,2"/>

    <!-- Depth scale -->
    <text x="12" y="50" text-anchor="end" font-family="Inter, sans-serif" font-size="7" fill="#68625b">0 m</text>
    <text x="12" y="159" text-anchor="end" font-family="Inter, sans-serif" font-size="7" fill="#68625b">50 m</text>
    <text x="12" y="270" text-anchor="end" font-family="Inter, sans-serif" font-size="7" fill="#68625b">100 m</text>

    <!-- LOG TRACK 1: Gamma-ray -->
    <rect x="140" y="35" width="120" height="275" fill="#fff" stroke="#68625b" stroke-width="0.8" rx="3"/>
    <text x="200" y="50" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#165d34">Gamma-Ray (API)</text>
    <text x="148" y="62" font-family="Inter, sans-serif" font-size="7" fill="#68625b">0</text>
    <text x="248" y="62" text-anchor="end" font-family="Inter, sans-serif" font-size="7" fill="#68625b">150</text>
    <line x1="145" y1="65" x2="255" y2="65" stroke="#68625b" stroke-width="0.3"/>

    <!-- GR curve: low in sand, high in shale, low in limestone -->
    <polyline points="170,68 165,75 168,80 172,85 175,88
      225,95 235,100 230,108 240,115 238,120 235,125 228,128
      180,134 175,142 172,150 178,158 175,165 170,172 172,178
      168,184 165,192 170,198 168,204 172,210 168,215 170,218
      230,224 235,232 228,240 232,248 225,252
      195,258 200,265 198,272 205,280 210,288 215,296 220,302" fill="none" stroke="#165d34" stroke-width="1.5"/>

    <!-- Shale line -->
    <line x1="220" y1="65" x2="220" y2="310" stroke="#165d34" stroke-width="0.4" stroke-dasharray="2,2" opacity="0.5"/>

    <!-- LOG TRACK 2: Resistivity -->
    <rect x="270" y="35" width="120" height="275" fill="#fff" stroke="#68625b" stroke-width="0.8" rx="3"/>
    <text x="330" y="50" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#d92b1f">Resistivity (Ω·m)</text>
    <text x="278" y="62" font-family="Inter, sans-serif" font-size="7" fill="#68625b">0.2</text>
    <text x="378" y="62" text-anchor="end" font-family="Inter, sans-serif" font-size="7" fill="#68625b">2000</text>
    <line x1="275" y1="65" x2="385" y2="65" stroke="#68625b" stroke-width="0.3"/>

    <!-- Resistivity curve: high in sand (if hydrocarbon), low in shale, high in limestone -->
    <polyline points="340,68 345,75 350,80 348,85 355,88
      295,95 290,100 292,108 288,115 290,120 293,125 295,128
      350,134 355,142 360,150 358,158 362,165 365,172 360,178
      370,184 375,192 372,198 368,204 370,210 375,215 372,218
      295,224 290,232 293,240 288,248 292,252
      310,258 315,265 320,272 318,280 322,288 320,296 315,302" fill="none" stroke="#d92b1f" stroke-width="1.5"/>

    <!-- LOG TRACK 3: Sonic -->
    <rect x="400" y="35" width="120" height="275" fill="#fff" stroke="#68625b" stroke-width="0.8" rx="3"/>
    <text x="460" y="50" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#1e4f8a">Sonic (µs/ft)</text>
    <text x="408" y="62" font-family="Inter, sans-serif" font-size="7" fill="#68625b">140</text>
    <text x="508" y="62" text-anchor="end" font-family="Inter, sans-serif" font-size="7" fill="#68625b">40</text>
    <line x1="405" y1="65" x2="515" y2="65" stroke="#68625b" stroke-width="0.3"/>

    <!-- Sonic curve: slow in shale/sand, fast in limestone/basement -->
    <polyline points="460,68 455,75 450,80 458,85 452,88
      430,95 425,100 428,108 422,115 425,120 428,125 432,128
      490,134 495,142 492,150 488,158 494,165 496,172 492,178
      455,184 448,192 452,198 458,204 450,210 448,215 452,218
      428,224 425,232 430,240 422,248 428,252
      500,258 505,265 502,272 508,280 510,288 505,296 508,302" fill="none" stroke="#1e4f8a" stroke-width="1.5"/>

    <!-- Interpretation labels (right side) -->
    <rect x="530" y="46" width="80" height="45" rx="3" fill="#f5e6a3" opacity="0.5" stroke="#8b5e00" stroke-width="0.5"/>
    <text x="570" y="64" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#8b5e00">Low GR</text>
    <text x="570" y="76" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#8b5e00">High Res → Sand</text>

    <rect x="530" y="95" width="80" height="32" rx="3" fill="#a0a090" opacity="0.3" stroke="#68625b" stroke-width="0.5"/>
    <text x="570" y="110" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#111">High GR</text>
    <text x="570" y="122" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#111">Low Res → Shale</text>

    <rect x="530" y="135" width="80" height="40" rx="3" fill="#d4cdb8" opacity="0.4" stroke="#8b5e00" stroke-width="0.5"/>
    <text x="570" y="153" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#8b5e00">Low GR, Fast Δt</text>
    <text x="570" y="165" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#8b5e00">→ Limestone</text>

    <rect x="530" y="260" width="80" height="30" rx="3" fill="#8a8078" opacity="0.3" stroke="#68625b" stroke-width="0.5"/>
    <text x="570" y="278" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#111">Very fast Δt</text>
    <text x="570" y="290" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#111">→ Basement</text>
  </svg>
  <p class="diagram-caption">Figure — Borehole logging cross-section: sonde descends the well recording gamma-ray, resistivity, and sonic logs; log responses distinguish sand, shale, limestone, and basement lithologies.</p>
</div>

<div class="learning-objectives">
  <div class="learning-objectives-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#1e4f8a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <h3>What you will learn</h3>
  </div>
  <ul>
    <li>Interpret gamma-ray logs to estimate shale volume and distinguish clean sands, shales, and evaporites in Central Asian sedimentary sequences</li>
    <li>Analyse resistivity logs (normal, lateral, induction) to identify formation fluids and calculate water saturation using Archie's equation</li>
    <li>Use sonic (acoustic) logs to derive porosity, compute synthetic seismograms, and calibrate surface seismic data</li>
    <li>Combine density, neutron, and caliper logs for lithology identification and cross-plot analysis of mineralogy and pore fluids</li>
    <li>Correlate multiple borehole logs across wells and integrate them with surface geophysics to build 3D stratigraphic models</li>
  </ul>
</div>

<div class="prerequisites">
  <div class="prerequisites-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#8b5e00" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
    <h3>Prerequisites</h3>
  </div>
  <ul>
    <li>Foundational knowledge of sedimentary geology: stratigraphy, lithology, and depositional environments</li>
    <li>Understanding of basic petrophysics: porosity, permeability, fluid saturation, and rock-physics relationships</li>
    <li>Familiarity with at least one surface geophysical method (seismic, resistivity, or magnetics) for integration context</li>
    <li>Ability to read well-log displays (depth tracks, linear/logarithmic scales, multi-curve overlays)</li>
  </ul>
</div>

<div class="concept-diagram">
  <svg viewBox="0 0 620 320" xmlns="http://www.w3.org/2000/svg" style="max-width: 580px;">
    <rect x="0" y="0" width="620" height="320" fill="#f9f8f6" rx="8"/>
    <text x="310" y="22" text-anchor="middle" font-family="Inter, sans-serif" font-size="13" font-weight="bold" fill="#111">Borehole Geophysical Logging</text>

    <!-- Ground surface -->
    <rect x="20" y="42" width="600" height="4" fill="#8b5e00"/>
    <text x="45" y="39" font-family="Inter, sans-serif" font-size="8" fill="#8b5e00">Ground Surface</text>

    <!-- Borehole shaft -->
    <rect x="68" y="46" width="24" height="260" fill="#c8dced" stroke="#1e4f8a" stroke-width="1" rx="2"/>

    <!-- Wireline cable -->
    <line x1="80" y1="35" x2="80" y2="200" stroke="#111" stroke-width="1.5"/>
    <!-- Winch at top -->
    <circle cx="80" cy="35" r="6" fill="#68625b" stroke="#111" stroke-width="1"/>
    <text x="80" y="28" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#111">winch</text>

    <!-- Logging sonde -->
    <rect x="72" y="200" width="16" height="60" rx="3" fill="#1e4f8a" stroke="#111" stroke-width="1"/>
    <circle cx="80" cy="210" r="3" fill="#d92b1f"/>
    <circle cx="80" cy="225" r="3" fill="#165d34"/>
    <circle cx="80" cy="240" r="3" fill="#8b5e00"/>
    <text x="80" y="272" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#1e4f8a">sonde</text>

    <!-- Lithological layers -->
    <!-- Layer 1: Sand -->
    <rect x="20" y="46" width="48" height="45" fill="#f5e6a3" opacity="0.6"/>
    <rect x="92" y="46" width="28" height="45" fill="#f5e6a3" opacity="0.6"/>
    <text x="34" y="72" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#8b5e00">Sand</text>

    <!-- Layer 2: Shale -->
    <rect x="20" y="91" width="48" height="40" fill="#a0a090" opacity="0.4"/>
    <rect x="92" y="91" width="28" height="40" fill="#a0a090" opacity="0.4"/>
    <line x1="20" y1="100" x2="48" y2="100" stroke="#68625b" stroke-width="0.4"/>
    <line x1="25" y1="108" x2="45" y2="108" stroke="#68625b" stroke-width="0.4"/>
    <line x1="20" y1="116" x2="48" y2="116" stroke="#68625b" stroke-width="0.4"/>
    <text x="34" y="110" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#111">Shale</text>

    <!-- Layer 3: Limestone -->
    <rect x="20" y="131" width="48" height="50" fill="#d4cdb8" opacity="0.5"/>
    <rect x="92" y="131" width="28" height="50" fill="#d4cdb8" opacity="0.5"/>
    <text x="34" y="160" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#8b5e00">Limestone</text>

    <!-- Layer 4: Sand (reservoir) -->
    <rect x="20" y="181" width="48" height="40" fill="#f5e6a3" opacity="0.7"/>
    <rect x="92" y="181" width="28" height="40" fill="#f5e6a3" opacity="0.7"/>
    <text x="34" y="202" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#8b5e00">Sand</text>
    <text x="34" y="212" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#8b5e00">(reservoir)</text>

    <!-- Layer 5: Shale -->
    <rect x="20" y="221" width="48" height="35" fill="#a0a090" opacity="0.4"/>
    <rect x="92" y="221" width="28" height="35" fill="#a0a090" opacity="0.4"/>
    <text x="34" y="242" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#111">Shale</text>

    <!-- Layer 6: Basement -->
    <rect x="20" y="256" width="48" height="50" fill="#8a8078" opacity="0.4"/>
    <rect x="92" y="256" width="28" height="50" fill="#8a8078" opacity="0.4"/>
    <text x="34" y="280" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#111">Basement</text>

    <!-- Layer boundaries -->
    <line x1="20" y1="91" x2="120" y2="91" stroke="#8b5e00" stroke-width="0.5" stroke-dasharray="2,2"/>
    <line x1="20" y1="131" x2="120" y2="131" stroke="#8b5e00" stroke-width="0.5" stroke-dasharray="2,2"/>
    <line x1="20" y1="181" x2="120" y2="181" stroke="#8b5e00" stroke-width="0.5" stroke-dasharray="2,2"/>
    <line x1="20" y1="221" x2="120" y2="221" stroke="#8b5e00" stroke-width="0.5" stroke-dasharray="2,2"/>
    <line x1="20" y1="256" x2="120" y2="256" stroke="#8b5e00" stroke-width="0.5" stroke-dasharray="2,2"/>

    <!-- Depth scale -->
    <text x="12" y="50" text-anchor="end" font-family="Inter, sans-serif" font-size="7" fill="#68625b">0 m</text>
    <text x="12" y="159" text-anchor="end" font-family="Inter, sans-serif" font-size="7" fill="#68625b">50 m</text>
    <text x="12" y="270" text-anchor="end" font-family="Inter, sans-serif" font-size="7" fill="#68625b">100 m</text>

    <!-- LOG TRACK 1: Gamma-ray -->
    <rect x="140" y="35" width="120" height="275" fill="#fff" stroke="#68625b" stroke-width="0.8" rx="3"/>
    <text x="200" y="50" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#165d34">Gamma-Ray (API)</text>
    <text x="148" y="62" font-family="Inter, sans-serif" font-size="7" fill="#68625b">0</text>
    <text x="248" y="62" text-anchor="end" font-family="Inter, sans-serif" font-size="7" fill="#68625b">150</text>
    <line x1="145" y1="65" x2="255" y2="65" stroke="#68625b" stroke-width="0.3"/>

    <!-- GR curve: low in sand, high in shale, low in limestone -->
    <polyline points="170,68 165,75 168,80 172,85 175,88
      225,95 235,100 230,108 240,115 238,120 235,125 228,128
      180,134 175,142 172,150 178,158 175,165 170,172 172,178
      168,184 165,192 170,198 168,204 172,210 168,215 170,218
      230,224 235,232 228,240 232,248 225,252
      195,258 200,265 198,272 205,280 210,288 215,296 220,302" fill="none" stroke="#165d34" stroke-width="1.5"/>

    <!-- Shale line -->
    <line x1="220" y1="65" x2="220" y2="310" stroke="#165d34" stroke-width="0.4" stroke-dasharray="2,2" opacity="0.5"/>

    <!-- LOG TRACK 2: Resistivity -->
    <rect x="270" y="35" width="120" height="275" fill="#fff" stroke="#68625b" stroke-width="0.8" rx="3"/>
    <text x="330" y="50" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#d92b1f">Resistivity (Ω·m)</text>
    <text x="278" y="62" font-family="Inter, sans-serif" font-size="7" fill="#68625b">0.2</text>
    <text x="378" y="62" text-anchor="end" font-family="Inter, sans-serif" font-size="7" fill="#68625b">2000</text>
    <line x1="275" y1="65" x2="385" y2="65" stroke="#68625b" stroke-width="0.3"/>

    <!-- Resistivity curve: high in sand (if hydrocarbon), low in shale, high in limestone -->
    <polyline points="340,68 345,75 350,80 348,85 355,88
      295,95 290,100 292,108 288,115 290,120 293,125 295,128
      350,134 355,142 360,150 358,158 362,165 365,172 360,178
      370,184 375,192 372,198 368,204 370,210 375,215 372,218
      295,224 290,232 293,240 288,248 292,252
      310,258 315,265 320,272 318,280 322,288 320,296 315,302" fill="none" stroke="#d92b1f" stroke-width="1.5"/>

    <!-- LOG TRACK 3: Sonic -->
    <rect x="400" y="35" width="120" height="275" fill="#fff" stroke="#68625b" stroke-width="0.8" rx="3"/>
    <text x="460" y="50" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#1e4f8a">Sonic (µs/ft)</text>
    <text x="408" y="62" font-family="Inter, sans-serif" font-size="7" fill="#68625b">140</text>
    <text x="508" y="62" text-anchor="end" font-family="Inter, sans-serif" font-size="7" fill="#68625b">40</text>
    <line x1="405" y1="65" x2="515" y2="65" stroke="#68625b" stroke-width="0.3"/>

    <!-- Sonic curve: slow in shale/sand, fast in limestone/basement -->
    <polyline points="460,68 455,75 450,80 458,85 452,88
      430,95 425,100 428,108 422,115 425,120 428,125 432,128
      490,134 495,142 492,150 488,158 494,165 496,172 492,178
      455,184 448,192 452,198 458,204 450,210 448,215 452,218
      428,224 425,232 430,240 422,248 428,252
      500,258 505,265 502,272 508,280 510,288 505,296 508,302" fill="none" stroke="#1e4f8a" stroke-width="1.5"/>

    <!-- Interpretation labels (right side) -->
    <rect x="530" y="46" width="80" height="45" rx="3" fill="#f5e6a3" opacity="0.5" stroke="#8b5e00" stroke-width="0.5"/>
    <text x="570" y="64" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#8b5e00">Low GR</text>
    <text x="570" y="76" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#8b5e00">High Res → Sand</text>

    <rect x="530" y="95" width="80" height="32" rx="3" fill="#a0a090" opacity="0.3" stroke="#68625b" stroke-width="0.5"/>
    <text x="570" y="110" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#111">High GR</text>
    <text x="570" y="122" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#111">Low Res → Shale</text>

    <rect x="530" y="135" width="80" height="40" rx="3" fill="#d4cdb8" opacity="0.4" stroke="#8b5e00" stroke-width="0.5"/>
    <text x="570" y="153" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#8b5e00">Low GR, Fast Δt</text>
    <text x="570" y="165" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#8b5e00">→ Limestone</text>

    <rect x="530" y="260" width="80" height="30" rx="3" fill="#8a8078" opacity="0.3" stroke="#68625b" stroke-width="0.5"/>
    <text x="570" y="278" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#111">Very fast Δt</text>
    <text x="570" y="290" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#111">→ Basement</text>
  </svg>
  <p class="diagram-caption">Figure — Borehole logging cross-section: sonde descends the well recording gamma-ray, resistivity, and sonic logs; log responses distinguish sand, shale, limestone, and basement lithologies.</p>
</div>

## Gamma-ray logging: natural radioactivity

### Physical basis

Gamma-ray (GR) logging measures natural radioactivity from potassium-40 ($^{40}$K), uranium-238 ($^{238}$U), and thorium-232 ($^{232}$Th) decay series. Clay minerals concentrate these elements, so GR response correlates with **shale content**.

The GR tool contains a scintillation detector (typically NaI crystal) that counts gamma photons. Output is in **API units** (American Petroleum Institute), calibrated against a standard test pit.

<div class="info-box tip">
  <strong>Key idea</strong>
  High GR = shale/clay; Low GR = clean sand, limestone, or evaporite. This simple relationship makes GR the most widely used log for lithology identification.
</div>

### Shale volume calculation

The **gamma-ray index** $I_{GR}$ normalizes the log between clean and shale endpoints:

$$I_{GR} = \frac{GR_{log} - GR_{clean}}{GR_{shale} - GR_{clean}}$$

For linear approximation: $V_{sh} = I_{GR}$

For older consolidated rocks, use nonlinear transforms:

**Larionov (Tertiary):** $V_{sh} = 0.083 \times (2^{3.7 \times I_{GR}} - 1)$

**Larionov (older rocks):** $V_{sh} = 0.33 \times (2^{2 \times I_{GR}} - 1)$

### Spectral gamma-ray logging

Spectral GR tools measure K, U, Th concentrations separately. This enables:

- **Clay typing:** Illite (high K), kaolinite (low K)
- **Organic matter:** High U often correlates with organic-rich shales
- **Provenance studies:** Th/K ratio indicates source terrain

Typical values in Central Asian formations:

| Lithology | GR (API) | K (%) | U (ppm) | Th (ppm) |
|-----------|----------|-------|---------|----------|
| Clean sand | 20-40 | <1 | <2 | <5 |
| Shale | 80-150 | 2-4 | 3-8 | 10-20 |
| Volcanic ash | 100-200 | 4-8 | 2-5 | 15-30 |
| Evaporite | 10-30 | 0-0.5 | <1 | <2 |

## Resistivity logging: formation conductivity

### Why resistivity matters

Formation resistivity depends on:
1. **Porosity** (more pore space = lower resistivity)
2. **Fluid content** (water vs. hydrocarbon)
3. **Water salinity** (saline = conductive)
4. **Clay content** (surface conduction)

Archie's equation relates these:

$$R_t = \frac{a \cdot R_w}{\phi^m \cdot S_w^n}$$

where $R_t$ = true formation resistivity, $R_w$ = water resistivity, $\phi$ = porosity, $S_w$ = water saturation, and $a$, $m$, $n$ are empirical constants.

### Normal resistivity logs

A **normal tool** has one current electrode (A) and one measure electrode (M) at fixed spacing. The **apparent resistivity** $R_a$ is:

$$R_a = 4\pi \cdot AM \cdot \frac{V}{I}$$

Common spacings:
- **Short normal (SN):** AM = 16 inches (0.4 m) — shallow investigation
- **Long normal (LN):** AM = 64 inches (1.6 m) — deeper investigation

<div class="info-box tip">
  <strong>Key idea</strong>
  Comparing short and long normal logs reveals invasion profile: if drilling fluid invaded the formation, SN reads the invaded zone while LN approaches true formation resistivity.
</div>

### Lateral resistivity logs

The **lateral tool** places the measure electrode pair far from the current electrode, providing deeper investigation. Spacing typically 18 feet 8 inches (5.7 m). Good for thin bed resolution but complex bed boundary effects.

### Induction logging

For low-resistivity formations or oil-based mud (non-conductive), **induction tools** use electromagnetic induction:

A transmitter coil creates a time-varying magnetic field, inducing eddy currents in the formation. A receiver coil measures the secondary magnetic field proportional to formation conductivity.

$$\sigma_a \propto \frac{V_R}{V_T}$$

Output in **conductivity** (mS/m) or inverted to resistivity. Modern array induction tools (AIT) provide multiple depths of investigation.

### Laterolog (focused resistivity)

**Laterologs** use guard electrodes to focus current into the formation, minimizing borehole and shoulder bed effects. Essential in high-resistivity formations and thin beds.

$$R_a = K \cdot \frac{V}{I}$$

where $K$ is the tool constant determined by electrode geometry.

| Tool | Investigation depth | Best for |
|------|---------------------|----------|
| Short normal | 0.3-0.5 m | Invaded zone |
| Long normal | 0.8-1.5 m | Moderate invasion |
| Lateral | 1-3 m | Deep resistivity |
| Induction | 0.5-2.5 m | Conductive formations |
| Laterolog | 0.3-1.5 m | High resistivity, thin beds |

## Sonic/acoustic logging: P-wave velocity

### Physical basis

Sonic logging measures the travel time of compressional (P-wave) acoustic pulses through the formation. A transmitter emits a pulse; receivers at known spacing detect arrivals.

**Interval transit time** $\Delta t$ (μs/ft or μs/m):

$$\Delta t = \frac{t_2 - t_1}{d}$$

where $t_1$, $t_2$ are arrival times at receivers separated by distance $d$.

**Velocity:** $V_p = \frac{1}{\Delta t} = \frac{10^6}{\Delta t_{μs/m}}$ m/s

### Porosity from sonic log

The **Wyllie time-average equation** assumes wave travels through matrix and fluid:

$$\Delta t = \phi \cdot \Delta t_f + (1-\phi) \cdot \Delta t_{ma}$$

Solving for porosity:

$$\phi_{sonic} = \frac{\Delta t_{log} - \Delta t_{ma}}{\Delta t_f - \Delta t_{ma}}$$

Typical values:

| Material | Δt (μs/ft) | Vp (m/s) |
|----------|------------|----------|
| Sandstone matrix | 55.5 | 5500 |
| Limestone matrix | 47.5 | 6400 |
| Dolomite matrix | 43.5 | 7000 |
| Water | 189 | 1600 |
| Oil | 238 | 1280 |
| Shale | 60-170 | 1800-5000 |

<div class="info-box tip">
  <strong>Key idea</strong>
  Sonic porosity assumes primary porosity. In fractured or vuggy carbonates, sonic "sees" only matrix porosity while density/neutron logs see total porosity.
</div>

### Compaction correction

In unconsolidated formations, apply compaction correction:

$$\phi_{corrected} = \phi_{sonic} \cdot \frac{1}{C_p}$$

where $C_p = \frac{\Delta t_{sh}}{\Delta t_{sh,compacted}}$ (ratio of observed shale transit time to compacted shale transit time, typically 100 μs/ft).

### Full waveform sonic logging

Modern **dipole sonic tools** record full waveforms, enabling:
- **Shear wave velocity** $V_s$ from flexural modes
- **Stoneley wave** analysis for permeability
- **Anisotropy** detection from fast/slow shear

Dynamic elastic moduli:

$$G = \rho V_s^2$$ (shear modulus)

$$E = 2G(1 + \nu)$$ (Young's modulus)

$$\nu = \frac{V_p^2 - 2V_s^2}{2(V_p^2 - V_s^2)}$$ (Poisson's ratio)

## Density logging: formation bulk density

### Physical basis

Density logging uses a gamma-ray source (typically $^{137}$Cs) and detectors at fixed spacing. Gamma rays undergo **Compton scattering** in the formation; the count rate at detectors depends on electron density, which correlates with bulk density.

$$\rho_e = 2 \cdot \rho_b \cdot \frac{Z}{A}$$

For most formation minerals, $Z/A \approx 0.5$, so $\rho_e \approx \rho_b$.

### Porosity from density

$$\phi_D = \frac{\rho_{ma} - \rho_b}{\rho_{ma} - \rho_f}$$

where $\rho_{ma}$ = matrix density, $\rho_b$ = bulk density from log, $\rho_f$ = fluid density.

Matrix densities:

| Mineral | ρma (g/cm³) |
|---------|-------------|
| Quartz | 2.65 |
| Calcite | 2.71 |
| Dolomite | 2.87 |
| Anhydrite | 2.98 |
| Clay | 2.20-2.65 |

### Photoelectric factor (Pe)

Modern density tools also measure the **photoelectric absorption index** $P_e$, sensitive to atomic number (lithology):

| Mineral | Pe (barns/electron) |
|---------|---------------------|
| Quartz | 1.81 |
| Calcite | 5.08 |
| Dolomite | 3.14 |
| Anhydrite | 5.05 |
| Barite | 267 |

<div class="info-box tip">
  <strong>Key idea</strong>
  Pe distinguishes lithologies that look similar on other logs. Sand (Pe ≈ 1.8) vs. limestone (Pe ≈ 5.1) is immediately clear even when porosities are similar.
</div>

### Quality control

The **density correction curve** $\Delta\rho$ indicates borehole rugosity effects. Accept data where $|\Delta\rho| < 0.15$ g/cm³.

## Neutron logging: hydrogen index and porosity

### Physical basis

Neutron tools emit high-energy neutrons (from AmBe or $^{252}$Cf sources). Neutrons slow down through collisions; hydrogen (in water, hydrocarbons, clay) is most effective at slowing neutrons due to similar mass.

The tool measures either:
- **Thermal neutron count** (CNL - Compensated Neutron Log)
- **Epithermal neutron count** (better in saline environments)

Output is **neutron porosity** $\phi_N$ calibrated to limestone matrix.

### Hydrogen index

$$HI = \frac{H_{formation}}{H_{fresh water}}$$

Pure water: HI = 1.0; Oil: HI ≈ 0.9-1.0; Gas: HI ≈ 0.4-0.6

### Lithology effect

Neutron porosity requires matrix correction:

| Matrix | Correction |
|--------|------------|
| Limestone | 0 (reference) |
| Sandstone | +4 p.u. |
| Dolomite | -2 to -7 p.u. |

### Gas effect

Gas has low hydrogen index, causing neutron porosity to read **too low**. Combined with density (which reads **too high** in gas), this creates the characteristic **crossover** pattern.

$$\phi_{gas} \approx \sqrt{\frac{\phi_D^2 + \phi_N^2}{2}}$$

<div class="info-box tip">
  <strong>Key idea</strong>
  Density-neutron crossover (ρb reading lower than expected, φN reading lower than expected) is the classic gas indicator on logs.
</div>

## Caliper and temperature logs

### Caliper logging

The caliper log measures **borehole diameter** using mechanical arms. Essential for:

1. **Quality control:** Density and neutron logs degrade in washed-out zones
2. **Cement volume calculations:** $V_{cement} = \frac{\pi}{4}(d_{caliper}^2 - d_{casing}^2) \cdot h$
3. **Lithology indication:** Shales cave; hard limestone maintains gauge

**Bit size** is the reference. Hole enlargement > 2 inches suggests unstable formation.

### Temperature logging

Temperature logs measure borehole fluid temperature. Uses:

1. **Geothermal gradient:** $\nabla T = \frac{dT}{dz}$ (typically 25-30°C/km)
2. **Fluid entry detection:** Influx zones show temperature anomalies
3. **Cement top location:** Heat of hydration causes temperature increase

Heat flow: $Q = k \cdot \nabla T$

where $k$ = thermal conductivity (W/m·K).

| Lithology | Thermal conductivity (W/m·K) |
|-----------|------------------------------|
| Sandstone | 2.5-4.0 |
| Shale | 1.0-2.5 |
| Limestone | 2.5-3.5 |
| Halite | 5.5-6.5 |

## Log interpretation: lithology and fluid content

### Crossplot analysis

**Neutron-density crossplot:** Plot $\phi_N$ (x-axis) vs. $\rho_b$ (y-axis). Points fall on lithology lines; gas shifts points up and left.

**M-N crossplot:** Eliminate porosity effects:

$$M = \frac{\Delta t_f - \Delta t}{\rho_b - \rho_f} \times 0.01$$

$$N = \frac{\phi_{Nf} - \phi_N}{\rho_b - \rho_f}$$

### Shaly sand analysis

In shaly sands, clay contributes to conductivity. The **Simandoux equation**:

$$\frac{1}{\sqrt{R_t}} = \frac{V_{sh}}{R_{sh}} + \frac{\phi^m S_w^n}{a R_w}$$

Or the **dual-water model** for more rigorous treatment.

### Quick-look interpretation

| Log response | Interpretation |
|--------------|----------------|
| High GR, low resistivity | Shale |
| Low GR, high resistivity, Δt ≈ 55 | Clean sand |
| Low GR, Pe ≈ 5, Δt ≈ 47 | Limestone |
| Very low GR, very high resistivity | Evaporite |
| Density-neutron crossover | Gas-bearing |
| SP deflection negative | Permeable sand |

<div class="info-box tip">
  <strong>Key idea</strong>
  No single log defines lithology—use multiple logs together. GR + resistivity + sonic + density/neutron provides robust interpretation.
</div>

### Water saturation calculation

From Archie's equation:

$$S_w = \left(\frac{a \cdot R_w}{\phi^m \cdot R_t}\right)^{1/n}$$

Typical parameters for clean sandstone: $a = 0.81$, $m = 2$, $n = 2$ (Humble formula)

For Central Asian carbonates: $a = 1$, $m = 2$, $n = 2$

**Bulk volume water:** $BVW = \phi \cdot S_w$

At irreducible water saturation, BVW is constant regardless of porosity.

## Correlation between boreholes

### Stratigraphic correlation

**Marker beds** are distinctive log signatures correlatable across a field:
- Coal seams (very high density contrast)
- Volcanic ash (high GR spike)
- Maximum flooding surfaces (high GR shale)
- Carbonate hardgrounds (density/sonic kicks)

### Correlation workflow

1. **Flatten on datum:** Choose a regional marker and set to zero
2. **Identify markers:** Work outward from known correlations
3. **Check consistency:** Isopach maps should be geologically reasonable
4. **Integrate cores:** Core descriptions confirm log picks

### Facies analysis

**Electrofacies** are log-based facies units defined by cluster analysis or neural networks:

$$d_{ij} = \sqrt{\sum_k w_k (x_{ik} - x_{jk})^2}$$

where $d_{ij}$ = distance between samples $i$ and $j$, $x$ = normalized log values, $w$ = weights.

### Sequence stratigraphy from logs

| Systems tract | GR pattern | Interpretation |
|--------------|------------|----------------|
| Lowstand | Blocky, low GR | Channel/fan sands |
| Transgressive | Fining-upward | Flooding |
| Highstand | Coarsening-upward | Progradation |

## Integration with surface geophysics and remote sensing

### Sonic log to seismic

**Synthetic seismogram:** Convolve reflectivity with wavelet:

$$R_i = \frac{\rho_{i+1}V_{i+1} - \rho_i V_i}{\rho_{i+1}V_{i+1} + \rho_i V_i}$$

$$Synthetic = R * Wavelet$$

This ties wells to seismic sections for horizon interpretation.

### Resistivity log to ERT/EM

Borehole resistivity calibrates surface ERT inversions:
1. Extract 1D resistivity profile at well location from ERT model
2. Compare with deep resistivity log (blocking to ERT resolution)
3. Adjust ERT inversion constraints if mismatch

### Density log to gravity

Borehole density constrains gravity modeling:

$$g = 2\pi G \rho h$$

For a slab of thickness $h$ and density contrast $\Delta\rho$.

### GR log to radiometric surveys

Airborne/ground gamma spectrometry detects K, U, Th at surface. Borehole spectral GR provides depth extension and calibration.

<div class="info-box tip">
  <strong>Key idea</strong>
  The power of borehole logging lies in integration. Logs calibrate surface geophysics; surface geophysics extends log information spatially.
</div>

### Central Asian case studies

**Fergana Valley aquifer characterization:**
- Surface ERT delineates aquifer geometry
- Borehole resistivity identifies fresh/saline interface
- Neutron-density defines porosity for storage estimates

**Tien Shan mineral exploration:**
- Surface magnetics locates intrusions
- Borehole magnetic susceptibility confirms ore-bearing zones
- GR logs map alteration halos

## Borehole logging checklist

<ul class="checklist">
  <li>Define logging objectives (lithology, porosity, saturation, correlation)</li>
  <li>Review borehole conditions (depth, diameter, fluid, stability)</li>
  <li>Select appropriate tool suite (GR mandatory; resistivity/sonic/density/neutron as needed)</li>
  <li>Verify tool calibrations (before and after log)</li>
  <li>Record accurate depth reference (KB, GL, water table)</li>
  <li>Specify logging speed (300-600 m/hr for most tools)</li>
  <li>Run caliper first to assess hole condition</li>
  <li>Log up (standard practice for depth accuracy)</li>
  <li>Obtain repeat section (minimum 60 m overlap)</li>
  <li>Check log quality (noise, spikes, shifts at casing shoe)</li>
  <li>Perform environmental corrections (borehole size, mud weight, temperature)</li>
  <li>Normalize logs between wells before correlation</li>
  <li>Document all header information (well name, date, tools, scales)</li>
  <li>Archive digital LAS files with paper prints</li>
  <li>Integrate with core and cuttings descriptions</li>
</ul>

## Exercises

### Exercise 1: Gamma-ray index calculation
A gamma-ray log reads 85 API. The clean sand baseline is 25 API and the shale baseline is 130 API. Calculate the gamma-ray index.

<details>
<summary>Show solution</summary>

$$I_{GR} = \frac{GR_{log} - GR_{clean}}{GR_{shale} - GR_{clean}} = \frac{85 - 25}{130 - 25} = \frac{60}{105} = 0.571$$

The gamma-ray index is **0.57** (or 57%).

</details>

### Exercise 2: Shale volume (Larionov)
Using the IGR from Exercise 1, calculate shale volume using the Larionov equation for Tertiary rocks.

<details>
<summary>Show solution</summary>

$$V_{sh} = 0.083 \times (2^{3.7 \times I_{GR}} - 1)$$
$$V_{sh} = 0.083 \times (2^{3.7 \times 0.571} - 1)$$
$$V_{sh} = 0.083 \times (2^{2.11} - 1)$$
$$V_{sh} = 0.083 \times (4.32 - 1) = 0.083 \times 3.32 = 0.276$$

Shale volume is **27.6%** using the nonlinear Larionov transform (compared to 57% linear).

</details>

### Exercise 3: Apparent resistivity (short normal)
A short normal tool (AM = 0.4 m) measures 25 mV with 100 mA current. Calculate apparent resistivity.

<details>
<summary>Show solution</summary>

$$R_a = 4\pi \cdot AM \cdot \frac{V}{I}$$
$$R_a = 4\pi \times 0.4 \times \frac{0.025}{0.1}$$
$$R_a = 5.03 \times 0.25 = 1.26 \text{ Ω·m}$$

Apparent resistivity is **1.26 Ω·m**.

</details>

### Exercise 4: Water saturation (Archie)
A clean sandstone has: φ = 0.22, Rt = 15 Ω·m, Rw = 0.05 Ω·m. Using Humble formula (a=0.81, m=2, n=2), calculate water saturation.

<details>
<summary>Show solution</summary>

$$S_w = \left(\frac{a \cdot R_w}{\phi^m \cdot R_t}\right)^{1/n}$$
$$S_w = \left(\frac{0.81 \times 0.05}{0.22^2 \times 15}\right)^{0.5}$$
$$S_w = \left(\frac{0.0405}{0.0484 \times 15}\right)^{0.5}$$
$$S_w = \left(\frac{0.0405}{0.726}\right)^{0.5} = (0.0558)^{0.5} = 0.236$$

Water saturation is **23.6%**, indicating hydrocarbon saturation of 76.4%.

</details>

### Exercise 5: Sonic porosity
A sonic log reads Δt = 95 μs/ft in a sandstone. Matrix transit time is 55.5 μs/ft, fluid transit time is 189 μs/ft. Calculate sonic porosity.

<details>
<summary>Show solution</summary>

$$\phi_{sonic} = \frac{\Delta t_{log} - \Delta t_{ma}}{\Delta t_f - \Delta t_{ma}}$$
$$\phi_{sonic} = \frac{95 - 55.5}{189 - 55.5} = \frac{39.5}{133.5} = 0.296$$

Sonic porosity is **29.6%**.

</details>

### Exercise 6: Density porosity
A density log reads ρb = 2.32 g/cm³. Assuming sandstone matrix (ρma = 2.65 g/cm³) and fresh water (ρf = 1.0 g/cm³), calculate density porosity.

<details>
<summary>Show solution</summary>

$$\phi_D = \frac{\rho_{ma} - \rho_b}{\rho_{ma} - \rho_f} = \frac{2.65 - 2.32}{2.65 - 1.0} = \frac{0.33}{1.65} = 0.20$$

Density porosity is **20%**.

</details>

### Exercise 7: Porosity comparison
Compare sonic porosity (29.6%) and density porosity (20%) from Exercises 5-6. What might explain the difference?

<details>
<summary>Show solution</summary>

The sonic porosity (29.6%) is significantly higher than density porosity (20%). Possible explanations:

1. **Unconsolidated formation:** Sonic log responds to poor grain contact, reading too slow
2. **Shale content:** Clay-bound water increases Δt but may not affect ρb proportionally
3. **Gas effect:** Gas decreases density (increasing φD) but data shows opposite

Most likely: the formation is **unconsolidated or shaly**. Apply compaction correction to sonic or use density porosity as more reliable.

</details>

### Exercise 8: Gas identification
A zone shows: φN = 12% (limestone), ρb = 2.25 g/cm³ (limestone φD = 28%). Is this a gas indicator?

<details>
<summary>Show solution</summary>

**Yes, classic gas indicator.** 

The neutron porosity (12%) is much lower than density porosity (28%). This crossover pattern occurs because:
- Gas has low hydrogen index → neutron reads low
- Gas has low density → bulk density reads low → density porosity reads high

The density-neutron separation (28% - 12% = 16 p.u.) strongly suggests gas.

Estimated true porosity: $\phi \approx \sqrt{\frac{\phi_D^2 + \phi_N^2}{2}} = \sqrt{\frac{0.28^2 + 0.12^2}{2}} = \sqrt{\frac{0.0784 + 0.0144}{2}} = \sqrt{0.0464} = 0.215$ or **21.5%**

</details>

### Exercise 9: Cement volume calculation
Caliper reads 10.5 inches; 7-inch casing will be run. Calculate cement volume per meter of hole.

<details>
<summary>Show solution</summary>

Convert to meters: 10.5 in = 0.267 m, 7 in = 0.178 m

$$V_{cement} = \frac{\pi}{4}(d_{caliper}^2 - d_{casing}^2) \times h$$
$$V_{cement} = \frac{\pi}{4}(0.267^2 - 0.178^2) \times 1$$
$$V_{cement} = 0.785 \times (0.0713 - 0.0317) = 0.785 \times 0.0396 = 0.0311 \text{ m}^3/\text{m}$$

Cement volume is **31.1 liters per meter** of hole.

</details>

### Exercise 10: Geothermal gradient
Temperature log shows 45°C at 500 m and 75°C at 1500 m. Surface temperature is 15°C. Calculate geothermal gradient.

<details>
<summary>Show solution</summary>

$$\nabla T = \frac{\Delta T}{\Delta z} = \frac{75 - 45}{1500 - 500} = \frac{30}{1000} = 0.030 \text{ °C/m} = 30 \text{ °C/km}$$

Alternatively, from surface: $\frac{75 - 15}{1500} = \frac{60}{1500} = 0.040$ °C/m (40 °C/km)

The interval gradient is **30°C/km**; average gradient from surface is **40°C/km**. The difference suggests higher conductivity rocks in the shallow section.

</details>

### Exercise 11: P-wave velocity
A sonic log reads Δt = 72 μs/ft. Convert to m/s.

<details>
<summary>Show solution</summary>

First convert to SI: 72 μs/ft × 3.281 ft/m = 236.2 μs/m

$$V_p = \frac{10^6}{\Delta t_{μs/m}} = \frac{10^6}{236.2} = 4234 \text{ m/s}$$

Or directly: $V_p = \frac{304800}{72} = 4233$ m/s

P-wave velocity is **4,234 m/s**.

</details>

### Exercise 12: Poisson's ratio
Dipole sonic log gives Vp = 4200 m/s and Vs = 2400 m/s. Calculate Poisson's ratio.

<details>
<summary>Show solution</summary>

$$\nu = \frac{V_p^2 - 2V_s^2}{2(V_p^2 - V_s^2)}$$
$$\nu = \frac{4200^2 - 2(2400)^2}{2(4200^2 - 2400^2)}$$
$$\nu = \frac{17,640,000 - 11,520,000}{2(17,640,000 - 5,760,000)}$$
$$\nu = \frac{6,120,000}{2 \times 11,880,000} = \frac{6,120,000}{23,760,000} = 0.258$$

Poisson's ratio is **0.26**, typical for consolidated sandstone.

</details>

### Exercise 13: Shear modulus
Using Vp = 4200 m/s, Vs = 2400 m/s, and ρb = 2.35 g/cm³, calculate shear modulus.

<details>
<summary>Show solution</summary>

$$G = \rho V_s^2$$

Convert density: 2.35 g/cm³ = 2350 kg/m³

$$G = 2350 \times 2400^2 = 2350 \times 5,760,000 = 13.54 \times 10^9 \text{ Pa} = 13.54 \text{ GPa}$$

Shear modulus is **13.5 GPa**.

</details>

### Exercise 14: Reflectivity calculation
Layer 1: ρ = 2.40 g/cm³, Vp = 3800 m/s. Layer 2: ρ = 2.55 g/cm³, Vp = 4500 m/s. Calculate reflection coefficient.

<details>
<summary>Show solution</summary>

Acoustic impedance Z = ρV:
- $Z_1 = 2.40 \times 3800 = 9120$ kg/(m²·s)
- $Z_2 = 2.55 \times 4500 = 11475$ kg/(m²·s)

$$R = \frac{Z_2 - Z_1}{Z_2 + Z_1} = \frac{11475 - 9120}{11475 + 9120} = \frac{2355}{20595} = 0.114$$

Reflection coefficient is **+0.114** (positive = harder layer below).

</details>

### Exercise 15: Neutron porosity correction
A neutron log reads φN = 18% (limestone calibration) in a sandstone. Apply matrix correction.

<details>
<summary>Show solution</summary>

For sandstone, add approximately 4 porosity units to limestone-calibrated neutron:

$$\phi_N^{corrected} = 18\% + 4\% = 22\%$$

Corrected neutron porosity is **22%**.

</details>

### Exercise 16: Bulk volume water
Zone A: φ = 25%, Sw = 40%. Zone B: φ = 18%, Sw = 55%. Compare BVW values.

<details>
<summary>Show solution</summary>

$$BVW = \phi \times S_w$$

Zone A: $BVW = 0.25 \times 0.40 = 0.10$ (10%)
Zone B: $BVW = 0.18 \times 0.55 = 0.099$ (9.9%)

Both zones have nearly identical BVW ≈ **10%**, suggesting both are at irreducible water saturation. This is expected in a uniform reservoir—higher porosity rock has lower Sw at irreducible.

</details>

### Exercise 17: Formation water resistivity
At reservoir temperature of 85°C, NaCl concentration is 35,000 ppm. Estimate Rw.

<details>
<summary>Show solution</summary>

Using the approximation: $R_w \approx \frac{3.3}{C^{0.85}}$ at 25°C, where C is in kppm:

$$R_{w,25°C} \approx \frac{3.3}{35^{0.85}} = \frac{3.3}{20.8} = 0.159 \text{ Ω·m}$$

Temperature correction (Arps formula):
$$R_{w,T} = R_{w,25} \times \frac{25 + 21.5}{T + 21.5} = 0.159 \times \frac{46.5}{106.5} = 0.069 \text{ Ω·m}$$

Formation water resistivity at 85°C is approximately **0.07 Ω·m**.

</details>

### Exercise 18: Depth correction
Well has KB = 12.5 m above GL. A marker is picked at 1847 m measured depth. What is true vertical depth subsea (TVDSS)?

<details>
<summary>Show solution</summary>

Assuming vertical well and GL = 0 datum:

$$TVDSS = MD - KB = 1847 - 12.5 = 1834.5 \text{ m}$$

True vertical depth subsea is **1834.5 m** below ground level.

Note: If sea level reference is needed, add ground elevation.

</details>

### Exercise 19: Invasion assessment
Short normal reads 5 Ω·m, long normal reads 25 Ω·m, deep lateral reads 45 Ω·m. What invasion profile exists?

<details>
<summary>Show solution</summary>

The resistivity increases with investigation depth:
- Rxo (flushed zone) ≈ 5 Ω·m
- Ri (transition zone) ≈ 25 Ω·m  
- Rt (uninvaded) ≈ 45 Ω·m

This is a **"resistivity increasing" profile**, typical of:
- Water-based mud invading hydrocarbon-bearing zone
- Fresh mud filtrate displacing saline formation water

The zone is likely **oil or gas bearing** with saline formation water.

</details>

### Exercise 20: Log normalization
Well A: GR range 20-140 API. Well B: GR range 35-125 API. Normalize Well B to Well A scale.

<details>
<summary>Show solution</summary>

Linear normalization:

$$GR_{norm} = (GR_B - GR_{B,min}) \times \frac{GR_{A,max} - GR_{A,min}}{GR_{B,max} - GR_{B,min}} + GR_{A,min}$$

For a reading of 80 API in Well B:

$$GR_{norm} = (80 - 35) \times \frac{140 - 20}{125 - 35} + 20$$
$$GR_{norm} = 45 \times \frac{120}{90} + 20 = 45 \times 1.33 + 20 = 60 + 20 = 80 \text{ API}$$

Coincidentally the same in this case. For GR_B = 100:

$$GR_{norm} = (100 - 35) \times 1.33 + 20 = 65 \times 1.33 + 20 = 86.5 + 20 = 106.5 \text{ API}$$

</details>

### Exercise 21: Spectral GR interpretation
A formation shows: K = 1.2%, U = 8 ppm, Th = 6 ppm. What might this indicate?

<details>
<summary>Show solution</summary>

Examining the ratios:
- Th/K = 6/1.2 = 5 (relatively low)
- U is elevated relative to Th and K

This pattern suggests:
- **Organic-rich shale** (high U associated with organic matter)
- Or **uranium mineralization zone**

The low Th/K rules out heavy mineral concentrations. The elevated U with moderate K suggests either:
1. Marine organic-rich source rock
2. Reducing environment concentrating uranium

Likely interpretation: **Organic-rich marine shale** or potential source rock.

</details>

### Exercise 22: Caliper interpretation
Caliper shows: 8.5" bit size, 12" reading in shale, 8.5" in sand, 8" in limestone. Interpret.

<details>
<summary>Show solution</summary>

| Interval | Caliper | Interpretation |
|----------|---------|----------------|
| Shale | 12" (+3.5") | **Washout/cave** - unstable shale |
| Sand | 8.5" (bit size) | **In gauge** - competent sand |
| Limestone | 8" (-0.5") | **Undergauge** - mudcake buildup on permeable zone |

The limestone is permeable (mudcake forms), sand is cemented and stable, shale is reactive and caving. Density/neutron logs in the shale washout zone will be **unreliable**.

</details>

### Exercise 23: M-N plot calculation
Given: Δt = 85 μs/ft, ρb = 2.45 g/cm³, φN = 15%. Calculate M and N values. Assume Δtf = 189, ρf = 1.0, φNf = 1.0.

<details>
<summary>Show solution</summary>

$$M = \frac{\Delta t_f - \Delta t}{\rho_b - \rho_f} \times 0.01$$
$$M = \frac{189 - 85}{2.45 - 1.0} \times 0.01 = \frac{104}{1.45} \times 0.01 = 71.7 \times 0.01 = 0.717$$

$$N = \frac{\phi_{Nf} - \phi_N}{\rho_b - \rho_f}$$
$$N = \frac{1.0 - 0.15}{2.45 - 1.0} = \frac{0.85}{1.45} = 0.586$$

**M = 0.717, N = 0.586**

These values can be plotted on an M-N crossplot to determine lithology independent of porosity.

</details>

### Exercise 24: Heat flow calculation
Geothermal gradient is 28°C/km. Average thermal conductivity is 2.5 W/(m·K). Calculate heat flow.

<details>
<summary>Show solution</summary>

$$Q = k \times \nabla T$$

Convert gradient to SI: 28°C/km = 0.028°C/m

$$Q = 2.5 \times 0.028 = 0.070 \text{ W/m}^2 = 70 \text{ mW/m}^2$$

Heat flow is **70 mW/m²**, typical for continental crust.

</details>

### Exercise 25: Simandoux equation
Shaly sand: Vsh = 0.25, Rsh = 2 Ω·m, φ = 0.18, Rt = 8 Ω·m, Rw = 0.1 Ω·m. Calculate Sw using Simandoux.

<details>
<summary>Show solution</summary>

$$\frac{1}{\sqrt{R_t}} = \frac{V_{sh}}{\sqrt{R_{sh}}} + \frac{\phi^m S_w^n}{a\sqrt{R_w}}$$

Using a=1, m=2, n=2:

$$\frac{1}{\sqrt{8}} = \frac{0.25}{\sqrt{2}} + \frac{0.18^2 \times S_w^2}{1 \times \sqrt{0.1}}$$

$$0.354 = 0.177 + \frac{0.0324 \times S_w^2}{0.316}$$

$$0.354 - 0.177 = 0.1025 \times S_w^2$$

$$0.177 = 0.1025 \times S_w^2$$

$$S_w^2 = 1.73$$

$$S_w = 1.31$$

This exceeds 1.0, indicating **100% water saturation**. The shale effect dominates; this is a wet zone.

</details>

### Exercise 26: Synthetic seismogram sampling
Logs sampled at 0.5 ft. Seismic data bandwidth is 10-60 Hz. What is the minimum resolvable bed thickness?

<details>
<summary>Show solution</summary>

Dominant frequency ≈ (10 + 60)/2 = 35 Hz

Assuming average velocity ≈ 3500 m/s:

Wavelength: $\lambda = \frac{V}{f} = \frac{3500}{35} = 100$ m

Vertical resolution (tuning thickness) = λ/4 = **25 m**

Beds thinner than 25 m will not be individually resolved on seismic but will be captured in log data at 0.5 ft (0.15 m) sampling.

</details>

### Exercise 27: Correlation exercise
Three wells show a distinctive high-GR spike at 845 m (Well A), 892 m (Well B), and 821 m (Well C). Well B is 3 km east and Well C is 2 km north of Well A. Calculate formation dip direction.

<details>
<summary>Show solution</summary>

Depth differences from Well A:
- Well B: 892 - 845 = +47 m deeper (3 km E)
- Well C: 821 - 845 = -24 m shallower (2 km N)

Gradient to east: 47 m / 3000 m = 0.0157 (dipping east)
Gradient to north: -24 m / 2000 m = -0.012 (rising north)

Dip azimuth: $\theta = \arctan\left(\frac{0.0157}{-0.012}\right) + 180° ≈ 127°$ (SE)

Dip magnitude: $\sqrt{0.0157^2 + 0.012^2} = 0.0198$ = 1.13°

Formation dips gently **1.1° toward SE (azimuth ~127°)**.

</details>

### Exercise 28: Fluid entry detection
Temperature log shows: steady 65°C above 1200 m, drops to 58°C at 1200-1210 m, returns to gradient below. Interpret.

<details>
<summary>Show solution</summary>

The **negative temperature anomaly** (cooling) at 1200-1210 m indicates:

**Fluid influx from a cooler zone** — likely water entry from a shallower aquifer through casing leak or from a cooler adjacent formation.

Alternative: Gas expansion (Joule-Thomson cooling) at a gas entry point.

The return to normal gradient below confirms this is a localized influx, not a regional thermal anomaly. Recommend: **flow log** to confirm entry location and rate.

</details>

### Exercise 29: Crossplot analysis
Zone plots at φN = 20%, ρb = 2.40 g/cm³. Limestone line passes through (0%, 2.71). Where does this point fall?

<details>
<summary>Show solution</summary>

On limestone line, expected ρb at φN = 20%:
$$\rho_b = \phi_N \times \rho_f + (1-\phi_N) \times \rho_{ma}$$
$$\rho_b = 0.20 \times 1.0 + 0.80 \times 2.71 = 0.20 + 2.17 = 2.37 \text{ g/cm}^3$$

Observed: ρb = 2.40 g/cm³ (slightly higher than limestone line)

The point falls **slightly above the limestone line** (toward dolomite). This could indicate:
1. Dolomitic limestone
2. Calcite with minor dolomite cement
3. Limestone with heavy mineral

Most likely: **dolomitic limestone** (partial dolomitization).

</details>

### Exercise 30: Integrated interpretation
A 10-m thick zone shows: GR = 35 API, Rt = 150 Ω·m, Δt = 60 μs/ft, ρb = 2.52 g/cm³, φN = 8% (limestone), Pe = 5.1. Full interpretation?

<details>
<summary>Show solution</summary>

**Systematic analysis:**

1. **GR = 35 API** → Clean (low shale content), not radioactive
2. **Pe = 5.1** → Limestone (calcite Pe = 5.08)
3. **Rt = 150 Ω·m** → Very high resistivity → tight or hydrocarbon-bearing
4. **Sonic porosity:** φs = (60-47.5)/(189-47.5) = 8.8%
5. **Density porosity:** φD = (2.71-2.52)/(2.71-1.0) = 11.1%
6. **Neutron porosity:** 8% (limestone calibration, no correction needed)

**Porosity crosscheck:** φD (11%) > φN (8%) — slight separation suggests **gas effect**

**Interpretation:** 
- Lithology: **Clean limestone**
- Porosity: ~10% (average)
- Fluid: Likely **gas-bearing** (density-neutron separation, very high resistivity)
- Quality: Moderate porosity, excellent sealing potential above/below

**Water saturation:** (assuming Rw = 0.1 Ω·m)
$$S_w = \sqrt{\frac{1 \times 0.1}{0.10^2 \times 150}} = \sqrt{\frac{0.1}{1.5}} = 0.26$$

**Sw ≈ 26%**, confirming hydrocarbon saturation of 74%.

**Final interpretation: Gas-bearing clean limestone, φ ≈ 10%, Sw ≈ 26%**

</details>

---

## Summary

Borehole geophysical logging provides irreplaceable ground truth for Central Asian subsurface investigations. The combination of gamma-ray (lithology), resistivity (fluid content), sonic (velocity/porosity), density (porosity/lithology), and neutron (porosity/gas detection) logs enables comprehensive formation evaluation. Integration with surface geophysics—calibrating ERT inversions, tying seismic horizons, validating EM conductivity models—maximizes the value of both borehole and regional datasets.

<div class="info-box tip">
  <strong>Key takeaway</strong>
  Master log interpretation as a multi-log exercise. Single logs are ambiguous; combined analysis resolves lithology, porosity, and fluid content with confidence.
</div>
