---
layout: tutorial
title: "How Seismic Methods Reveal Near-Surface Structure in Central Asia"
description: "A math-first tutorial on seismic refraction, reflection, MASW, and subsurface velocity imaging for engineering and archaeology in Central Asia, with 30 exercises and solutions."
difficulty: advanced
estimated_time: "3-4 hours"
series: treasure-hunting
order: 9
---

## Why this tutorial matters

Seismic methods probe the subsurface by analyzing how elastic waves propagate through earth materials. Unlike electromagnetic methods that respond to conductivity and permittivity, or magnetic methods sensitive to susceptibility, seismic techniques directly measure mechanical properties—velocity, density, and stiffness—that reveal geological structure, material strength, and buried interfaces.

In Central Asia, seismic methods are essential for engineering site characterization, water table mapping, bedrock depth determination, and archaeological prospection of buried walls, voids, and compacted surfaces. The region's diverse geology—loess plateaus, alluvial fans, mountain piedmonts, and desert basins—creates complex velocity structures ideally suited to seismic investigation.

<div class="info-box tip">
  <strong>Key idea</strong>
  Seismic methods measure travel times of elastic waves through the subsurface. Different materials have different velocities, and velocity contrasts create reflections and refractions that reveal layer boundaries, structures, and anomalies.
</div>

<div class="learning-objectives">
  <div class="learning-objectives-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#1e4f8a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <h3>What you will learn</h3>
  </div>
  <ul>
    <li>How P-wave, S-wave, and surface-wave velocities relate to elastic moduli and reveal material properties of Central Asian subsurface soils</li>
    <li>Seismic refraction theory—Snell's law, critical angle, head waves—and the intercept-time method for determining layer depths and velocities</li>
    <li>Seismic reflection principles including normal-moveout correction, stacking, and how to image buried interfaces in layered sediments</li>
    <li>MASW (Multichannel Analysis of Surface Waves) for building shear-wave velocity profiles from Rayleigh-wave dispersion curves</li>
    <li>Practical survey design: geophone spacing, source selection (hammer, weight drop), and field procedures for loess, alluvial, and bedrock terrains</li>
  </ul>
</div>

<div class="prerequisites">
  <div class="prerequisites-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#8b5e00" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
    <h3>Prerequisites</h3>
  </div>
  <ul>
    <li>Understanding of wave propagation (frequency, wavelength, velocity) and Snell's law at the introductory physics level</li>
    <li>Familiarity with trigonometric functions and the ability to interpret travel-time versus offset graphs</li>
    <li>Completion of Tutorial 08 (ERT) or equivalent exposure to geophysical imaging and cross-section interpretation</li>
    <li>Basic comfort with spectral analysis concepts (Fourier transforms, dispersion) for the MASW section</li>
  </ul>
</div>

<div class="concept-diagram">
  <svg viewBox="0 0 620 320" xmlns="http://www.w3.org/2000/svg" style="max-width: 580px;">
    <!-- Background -->
    <rect x="0" y="0" width="620" height="320" fill="#f8f7f4" rx="6"/>

    <!-- Title -->
    <text x="310" y="22" text-anchor="middle" font-family="Inter, sans-serif" font-size="13" font-weight="600" fill="#111">Seismic Refraction &amp; Reflection — Wave Paths and Travel-Time Graph</text>

    <!-- LEFT PANEL: Cross-section -->
    <text x="195" y="42" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" font-weight="600" fill="#111">Cross-section</text>

    <!-- Ground surface -->
    <line x1="20" y1="80" x2="380" y2="80" stroke="#8b5e00" stroke-width="2"/>

    <!-- Layer 1 -->
    <rect x="20" y="80" width="360" height="70" fill="#e8dcc8" rx="0"/>
    <text x="30" y="118" font-family="Inter, sans-serif" font-size="10" fill="#68625b">Layer 1: V₁ = 400 m/s</text>

    <!-- Layer boundary -->
    <line x1="20" y1="150" x2="380" y2="150" stroke="#111" stroke-width="1.5" stroke-dasharray="6,3"/>

    <!-- Layer 2 -->
    <rect x="20" y="150" width="360" height="80" fill="#d9ccb0" rx="0"/>
    <text x="30" y="195" font-family="Inter, sans-serif" font-size="10" fill="#68625b">Layer 2: V₂ = 1800 m/s</text>

    <!-- Source (hammer) -->
    <g transform="translate(55,55)">
      <line x1="0" y1="25" x2="0" y2="10" stroke="#d92b1f" stroke-width="2.5"/>
      <line x1="-6" y1="10" x2="6" y2="10" stroke="#d92b1f" stroke-width="2.5"/>
      <polygon points="0,25 -5,30 5,30" fill="#d92b1f"/>
    </g>
    <text x="55" y="50" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" font-weight="600" fill="#d92b1f">Source</text>

    <!-- Geophones -->
    <g fill="#1e4f8a" stroke="#1e4f8a" stroke-width="1">
      <polygon points="115,80 110,70 120,70"/>
      <polygon points="165,80 160,70 170,70"/>
      <polygon points="215,80 210,70 220,70"/>
      <polygon points="265,80 260,70 270,70"/>
      <polygon points="315,80 310,70 320,70"/>
      <polygon points="355,80 350,70 360,70"/>
    </g>
    <text x="355" y="65" font-family="Inter, sans-serif" font-size="9" fill="#1e4f8a">Geophones</text>

    <!-- Direct wave (straight along surface) -->
    <line x1="55" y1="82" x2="315" y2="82" stroke="#165d34" stroke-width="1.5"/>
    <text x="190" y="96" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#165d34">Direct wave (V₁)</text>

    <!-- Refracted wave path -->
    <polyline points="55,82 100,150 280,150 315,82" fill="none" stroke="#d92b1f" stroke-width="1.5"/>
    <text x="190" y="163" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#d92b1f">Head wave along interface (V₂)</text>

    <!-- Critical angle arc -->
    <path d="M 85 82 Q 85 110 100 130" fill="none" stroke="#8b5e00" stroke-width="1" stroke-dasharray="2,2"/>
    <text x="72" y="115" font-family="Inter, sans-serif" font-size="9" fill="#8b5e00">θc</text>

    <!-- Reflected wave path -->
    <polyline points="55,82 170,150 265,82" fill="none" stroke="#1e4f8a" stroke-width="1.5" stroke-dasharray="5,3"/>
    <text x="140" y="140" font-family="Inter, sans-serif" font-size="9" fill="#1e4f8a">Reflected</text>

    <!-- Divider -->
    <line x1="395" y1="35" x2="395" y2="310" stroke="#68625b" stroke-width="0.5" stroke-dasharray="3,3"/>

    <!-- RIGHT PANEL: Travel-time graph -->
    <text x="505" y="42" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" font-weight="600" fill="#111">Travel-time graph</text>

    <!-- Axes -->
    <line x1="420" y1="240" x2="595" y2="240" stroke="#111" stroke-width="1.2"/>
    <line x1="420" y1="240" x2="420" y2="60" stroke="#111" stroke-width="1.2"/>
    <text x="510" y="260" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">Offset (m)</text>
    <text x="412" y="145" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111" transform="rotate(-90,412,145)">Time (ms)</text>

    <!-- Offset tick marks -->
    <g font-family="Inter, sans-serif" font-size="8" fill="#68625b" text-anchor="middle">
      <line x1="445" y1="240" x2="445" y2="244" stroke="#68625b"/><text x="445" y="253">5</text>
      <line x1="480" y1="240" x2="480" y2="244" stroke="#68625b"/><text x="480" y="253">15</text>
      <line x1="515" y1="240" x2="515" y2="244" stroke="#68625b"/><text x="515" y="253">25</text>
      <line x1="550" y1="240" x2="550" y2="244" stroke="#68625b"/><text x="550" y="253">35</text>
      <line x1="585" y1="240" x2="585" y2="244" stroke="#68625b"/><text x="585" y="253">45</text>
    </g>

    <!-- Direct-wave line (steep slope = slow) -->
    <line x1="420" y1="240" x2="575" y2="90" stroke="#165d34" stroke-width="2"/>
    <text x="480" y="140" font-family="Inter, sans-serif" font-size="10" fill="#165d34" transform="rotate(-44,480,140)">slope = 1/V₁</text>

    <!-- Refracted-wave line (shallower slope, intercept) -->
    <line x1="500" y1="192" x2="585" y2="150" stroke="#d92b1f" stroke-width="2"/>
    <text x="555" y="140" font-family="Inter, sans-serif" font-size="10" fill="#d92b1f">1/V₂</text>

    <!-- Crossover point -->
    <circle cx="510" cy="185" r="4" fill="none" stroke="#111" stroke-width="1.5"/>
    <text x="520" y="200" font-family="Inter, sans-serif" font-size="9" fill="#111">x_cross</text>

    <!-- Intercept time -->
    <line x1="418" y1="212" x2="425" y2="212" stroke="#d92b1f" stroke-width="1"/>
    <text x="435" y="216" font-family="Inter, sans-serif" font-size="9" fill="#d92b1f">tᵢ</text>

    <!-- Reflected-wave hyperbola -->
    <path d="M 420 210 Q 470 175 520 165 Q 560 158 590 155" fill="none" stroke="#1e4f8a" stroke-width="1.5" stroke-dasharray="5,3"/>
    <text x="590" y="148" font-family="Inter, sans-serif" font-size="9" fill="#1e4f8a">Reflected</text>

    <!-- Legend -->
    <line x1="420" y1="285" x2="445" y2="285" stroke="#165d34" stroke-width="2"/>
    <text x="450" y="289" font-family="Inter, sans-serif" font-size="9" fill="#111">Direct wave</text>
    <line x1="420" y1="300" x2="445" y2="300" stroke="#d92b1f" stroke-width="2"/>
    <text x="450" y="304" font-family="Inter, sans-serif" font-size="9" fill="#111">Refracted wave</text>
    <line x1="530" y1="285" x2="555" y2="285" stroke="#1e4f8a" stroke-width="1.5" stroke-dasharray="5,3"/>
    <text x="560" y="289" font-family="Inter, sans-serif" font-size="9" fill="#111">Reflected wave</text>
  </svg>
  <p class="diagram-caption">Left: seismic wave paths from a hammer source to a geophone array — the direct wave travels along the surface at V₁, the refracted head wave travels along the faster layer boundary at V₂, and the reflected wave bounces off the interface. Right: corresponding travel-time graph showing how slope inversions reveal layer velocities and crossover distance indicates layer depth.</p>
</div>

## Seismic wave fundamentals

### Types of seismic waves

When energy is released at the surface (hammer blow, weight drop, explosive), it generates several wave types that propagate through the subsurface.

**P-waves (Primary waves)** are compressional waves where particle motion is parallel to wave propagation. They travel fastest and arrive first. P-wave velocity $V_P$ depends on elastic moduli:

$$
V_P = \sqrt{\frac{K + \frac{4}{3}G}{\rho}}
$$

where $K$ is bulk modulus, $G$ is shear modulus, and $\rho$ is density.

**S-waves (Secondary waves)** are shear waves where particle motion is perpendicular to propagation. They cannot travel through fluids. S-wave velocity is:

$$
V_S = \sqrt{\frac{G}{\rho}}
$$

**Surface waves** travel along the free surface and include Rayleigh waves (retrograde elliptical motion) and Love waves (horizontal shear). They attenuate more slowly with distance than body waves but penetrate to limited depth.

### Velocity relationships

The ratio of P-wave to S-wave velocity relates to Poisson's ratio $\nu$:

$$
\frac{V_P}{V_S} = \sqrt{\frac{2(1-\nu)}{1-2\nu}}
$$

For most rocks, $\nu$ ranges from 0.2 to 0.35, giving $V_P/V_S$ ratios of 1.6 to 2.1. Water-saturated sediments have higher ratios (approaching infinity as $\nu \rightarrow 0.5$).

<div class="info-box tip">
  <strong>Key idea</strong>
  P-wave velocity increases dramatically across the water table (from ~500 m/s to ~1500 m/s in unconsolidated sediments) because water's incompressibility dominates. S-wave velocity is less affected since shear modulus of water is zero.
</div>

### Typical velocities in Central Asian materials

| Material | $V_P$ (m/s) | $V_S$ (m/s) |
|----------|-------------|-------------|
| Air | 330 | — |
| Water | 1500 | — |
| Dry loess | 200–400 | 100–200 |
| Saturated loess | 1500–1800 | 150–300 |
| Alluvial gravel | 400–700 | 200–400 |
| Saturated gravel | 1600–2000 | 250–500 |
| Weathered bedrock | 1500–3000 | 700–1500 |
| Intact limestone | 3000–6000 | 1500–3500 |
| Granite | 4500–6500 | 2500–3500 |

## Seismic refraction

### The critical angle and head waves

When a seismic wave traveling in a slower layer encounters an interface with a faster layer, refraction occurs according to Snell's law:

$$
\frac{\sin \theta_1}{V_1} = \frac{\sin \theta_2}{V_2}
$$

At the critical angle $\theta_c$, the refracted wave travels parallel to the interface:

$$
\theta_c = \arcsin\left(\frac{V_1}{V_2}\right)
$$

This critically refracted wave, called a head wave, travels along the interface at velocity $V_2$ and continuously radiates energy back to the surface at angle $\theta_c$. At sufficient distance, head waves arrive before direct waves.

### Travel-time curves

For a two-layer model with layer 1 thickness $h$ and velocities $V_1$ and $V_2$ ($V_2 > V_1$), the travel times are:

**Direct wave:**
$$
t_d = \frac{x}{V_1}
$$

**Refracted wave:**
$$
t_r = \frac{x}{V_2} + \frac{2h\cos\theta_c}{V_1} = \frac{x}{V_2} + t_i
$$

where $t_i$ is the intercept time. The crossover distance where refracted and direct arrivals coincide is:

$$
x_c = 2h\sqrt{\frac{V_2 + V_1}{V_2 - V_1}}
$$

### Depth calculation from travel-time data

From the intercept time $t_i$ and measured velocities:

$$
h = \frac{t_i V_1 V_2}{2\sqrt{V_2^2 - V_1^2}}
$$

Alternatively, using the crossover distance:

$$
h = \frac{x_c}{2}\sqrt{\frac{V_2 - V_1}{V_2 + V_1}}
$$

<div class="info-box tip">
  <strong>Key idea</strong>
  Refraction requires velocity to increase with depth. It cannot detect low-velocity layers (velocity inversions) because no critical refraction occurs at their upper boundaries—these "hidden layers" are invisible to refraction.
</div>

### Dipping layer refraction

For a dipping interface, travel times differ between updip and downdip shooting directions. The apparent velocities are:

$$
V_{2d} = \frac{V_1}{\sin(\theta_c + \alpha)} \quad \text{(downdip)}
$$

$$
V_{2u} = \frac{V_1}{\sin(\theta_c - \alpha)} \quad \text{(updip)}
$$

where $\alpha$ is the dip angle. The true velocity is:

$$
V_2 = \frac{V_1}{\sin\theta_c}
$$

where $\theta_c = \frac{1}{2}\left(\arcsin\frac{V_1}{V_{2d}} + \arcsin\frac{V_1}{V_{2u}}\right)$.

## Seismic reflection

### Two-way travel time

In reflection surveys, energy travels down to a reflector and back up to receivers. The two-way travel time (TWT) for a horizontal reflector at depth $z$ is:

$$
t_0 = \frac{2z}{V}
$$

For a source-receiver offset $x$, the travel time follows a hyperbolic relationship:

$$
t^2 = t_0^2 + \frac{x^2}{V^2}
$$

This is the normal moveout (NMO) equation. The NMO correction removes offset dependence before stacking traces.

### Impedance contrast and reflection coefficient

Reflections occur where acoustic impedance $Z = \rho V$ changes. The reflection coefficient at normal incidence is:

$$
R = \frac{Z_2 - Z_1}{Z_2 + Z_1} = \frac{\rho_2 V_2 - \rho_1 V_1}{\rho_2 V_2 + \rho_1 V_1}
$$

Strong reflections occur at:
- Water table (large velocity increase)
- Bedrock surface (velocity and density increase)
- Buried pavements or compacted layers
- Void spaces (dramatic impedance decrease)

### Vertical resolution

The vertical resolution—ability to distinguish closely spaced reflectors—is approximately one-quarter wavelength:

$$
\Delta z_{min} \approx \frac{\lambda}{4} = \frac{V}{4f}
$$

where $f$ is the dominant frequency. Higher frequencies improve resolution but attenuate more rapidly with depth.

<div class="info-box tip">
  <strong>Key idea</strong>
  Near-surface reflection surveys typically use frequencies of 50–200 Hz. At 100 Hz in material with $V = 400$ m/s, vertical resolution is about $\frac{400}{4 \times 100} = 1$ m. Achieving sub-meter resolution requires higher frequencies and shallow targets.
</div>

### Horizontal resolution and Fresnel zone

The first Fresnel zone defines horizontal resolution—the area contributing to a reflection. Its radius is:

$$
r_F = \sqrt{\frac{\lambda z}{2}} = \sqrt{\frac{Vz}{2f}}
$$

At 2 m depth with $V = 400$ m/s and $f = 100$ Hz, $r_F \approx 2$ m. Deeper targets have larger Fresnel zones and poorer lateral resolution.

## MASW: Multichannel Analysis of Surface Waves

### Surface wave dispersion

Surface waves in layered media are dispersive—different frequencies travel at different velocities because they sample different depths. Higher frequencies concentrate near the surface; lower frequencies penetrate deeper.

The phase velocity $c$ of Rayleigh waves varies with frequency $f$, creating a dispersion curve $c(f)$. Inverting this curve yields the S-wave velocity profile $V_S(z)$.

### The dispersion equation

For a homogeneous halfspace, the Rayleigh wave velocity is approximately:

$$
V_R \approx \frac{0.87 + 1.12\nu}{1 + \nu} V_S
$$

For layered media, the dispersion relationship is complex and solved numerically. The fundamental mode dominates at low frequencies; higher modes appear at higher frequencies.

### MASW field procedure

MASW surveys use a linear array of low-frequency geophones (4.5 Hz typical) with an active source (sledgehammer, weight drop) offset from the spread. Key parameters include:

- **Spread length $L$**: Determines maximum investigation depth (~$L/3$ to $L/2$)
- **Geophone spacing $dx$**: Controls maximum frequency and aliasing ($f_{max} < V_{R,min}/2dx$)
- **Source offset**: Typically $0.5L$ to $1.0L$ from nearest geophone to develop surface waves

### Dispersion curve extraction

From multichannel records, dispersion is extracted using frequency-wavenumber ($f$-$k$) or phase-shift transforms. The dispersion image shows energy concentration along the fundamental mode (and sometimes higher modes).

Phase velocity at each frequency is:

$$
c(f) = \frac{2\pi f}{k}
$$

where $k$ is the wavenumber at maximum amplitude.

### Inversion for $V_S$ profile

The measured dispersion curve is inverted to obtain $V_S(z)$ by iteratively adjusting a layered earth model until theoretical dispersion matches observations. The inverse problem is non-unique; constraints from refraction or borehole data improve solutions.

<div class="info-box tip">
  <strong>Key idea</strong>
  MASW provides $V_S$ profiles without drilling. Shear-wave velocity directly relates to material stiffness, making MASW essential for engineering site characterization, liquefaction assessment, and archaeological detection of compacted or disturbed zones.
</div>

## Equipment and sources

### Geophones

Geophones are velocity-sensitive electromechanical transducers. A coil suspended in a magnetic field generates voltage proportional to ground velocity. Key specifications:

- **Natural frequency**: 4.5 Hz (MASW), 10–14 Hz (refraction), 40–100 Hz (high-resolution reflection)
- **Sensitivity**: Typically 28–32 V/(m/s) for standard units
- **Damping**: 0.6–0.7 critical for flat response above natural frequency

Geophone planting affects coupling. Firm contact with compacted soil is essential; loose sand or thick vegetation degrades signal quality.

### Seismographs and recording systems

Modern seismographs digitize signals at 24-bit resolution with sample intervals of 0.125–2 ms. Key parameters:

- **Sample interval $\Delta t$**: Determines maximum frequency ($f_{max} = 1/2\Delta t$ by Nyquist)
- **Record length**: Must capture all arrivals of interest (typically 200–1000 ms for shallow surveys)
- **Channel count**: 24–96 channels common for near-surface work

### Seismic sources

**Sledgehammer** (4–10 kg): Lightweight, portable, low cost. Produces frequencies up to 200 Hz. Suitable for shallow (< 20 m) refraction and reflection.

**Weight drop** (50–200 kg): Greater energy for deeper penetration. Accelerated weight drop systems provide consistent, repeatable impacts.

**Explosive charges**: Maximum energy but require permits and safety protocols. Used for deep investigations or poor coupling conditions.

**Vibroseis**: Controlled frequency sweep, excellent for reflection surveys. Miniature vibrators exist for shallow engineering work.

<div class="info-box tip">
  <strong>Key idea</strong>
  Source selection balances depth penetration, resolution, portability, and logistics. In Central Asia's remote archaeological sites, sledgehammer surveys are often most practical. Engineering projects may justify weight-drop or mini-vibrator systems.
</div>

## Survey design and field procedures

### Refraction survey geometry

Standard refraction arrays use inline geometry with:

- **Geophone spacing**: 1–5 m depending on depth of investigation
- **Spread length**: 5–10× target depth
- **Shot points**: At both ends (forward/reverse) plus center shots

For a 50 m spread targeting 15 m depth:
- 24 geophones at 2 m spacing
- Shots at 0, 25, and 50 m (and offset shots at -5 m and 55 m)

### Reflection survey geometry

Near-surface reflection surveys use:

- **Common midpoint (CMP) method**: Multiple source-receiver pairs share reflection points
- **Fold**: Number of traces contributing to each CMP (typically 6–24)
- **Near offset**: Minimum source-receiver distance (limits shallow imaging)
- **Far offset**: Maximum offset (should not exceed ~2× target depth)

### MASW survey geometry

Roll-along MASW surveys move the array incrementally to create pseudo-2D sections:

- **24 geophones at 1–2 m spacing**
- **Source offset 10–30 m from nearest geophone**
- **Array moves half-spread between records**

### Field procedures checklist

1. **Site reconnaissance**: Identify noise sources, obstacles, optimal spread locations
2. **Equipment test**: Verify all geophones, cables, and recording system
3. **Geophone planting**: Firm coupling, vertical orientation, consistent depth
4. **Noise assessment**: Record ambient noise before shooting
5. **Timing verification**: Check trigger synchronization
6. **Multiple stacks**: Average repeated impacts to improve signal-to-noise
7. **Field notes**: Record geometry, source details, ground conditions

## Data processing and interpretation

### Refraction processing

1. **First-break picking**: Identify first arrivals on each trace
2. **Travel-time plotting**: Create time-distance curves
3. **Velocity determination**: Measure slopes of linear segments
4. **Layer thickness calculation**: Use intercept time or crossover methods
5. **Tomographic inversion**: For complex structures, iteratively adjust velocity model

### Reflection processing

1. **Geometry assignment**: Define source and receiver coordinates
2. **Trace editing**: Remove bad traces, mute noise
3. **Filtering**: Bandpass filter to enhance reflection frequencies
4. **Velocity analysis**: Determine stacking velocities from moveout
5. **NMO correction**: Flatten hyperbolic reflections
6. **CMP stacking**: Sum traces at common midpoints to enhance reflections
7. **Migration**: Collapse diffraction patterns, position dipping reflectors correctly

### MASW processing

1. **Dispersion imaging**: Apply frequency-wavenumber or phase-shift transform
2. **Dispersion curve picking**: Extract phase velocity versus frequency
3. **Initial model**: Estimate layer thicknesses and velocities
4. **Inversion**: Iterate model until theoretical dispersion matches picked curve
5. **Pseudo-2D section**: Interpolate 1D profiles along survey line

<div class="info-box tip">
  <strong>Key idea</strong>
  Processing converts raw field records into interpretable velocity models. Each method requires different workflows, but all involve noise suppression, travel-time analysis, and model construction from the processed data.
</div>

## Central Asia applications

### Bedrock depth mapping

Determining depth to competent bedrock is essential for foundation design in Central Asia's piedmont zones. Seismic refraction excels at this task because weathered rock overlying fresh bedrock creates strong velocity contrast:

- Alluvium/colluvium: 300–800 m/s
- Weathered bedrock: 1500–2500 m/s
- Competent bedrock: 3000–6000 m/s

A typical survey deploys 24 geophones at 3 m spacing with hammer source at both ends. Travel-time analysis reveals bedrock depth variation along the profile.

### Water table detection

The dramatic P-wave velocity increase at the water table (from ~300 m/s unsaturated to ~1500 m/s saturated) makes refraction ideal for water table mapping. The refraction arrival from the saturated zone appears as a distinct velocity segment in travel-time data.

However, caution is needed: if the water table coincides with a lithologic boundary, velocity contrasts may be ambiguous.

### Archaeological applications

Seismic methods detect buried archaeological features through velocity contrasts:

**Buried walls and foundations**: Compact masonry has higher velocity than surrounding fill or soil. Reflection surveys image wall tops; MASW may detect shear-wave velocity anomalies.

**Compacted surfaces**: Ancient roads, floors, and courtyards have elevated velocity from trampling and compaction. MASW detects these as thin high-velocity layers.

**Voids and cavities**: Kurgans, tombs, and underground chambers create velocity anomalies. Reflection surveys may image void roofs; tomographic inversions reveal low-velocity zones.

**Ditch fills**: Refilled ditches often have lower velocity than surrounding undisturbed soil, creating detectable anomalies.

### Engineering site characterization

Seismic methods inform foundation design, slope stability, and seismic hazard assessment:

- **$V_{S30}$**: Average S-wave velocity in upper 30 m, used for seismic site classification
- **Rippability**: Rock excavation difficulty correlates with P-wave velocity
- **Liquefaction potential**: Low $V_S$ in saturated granular soils indicates susceptibility
- **Slope stability**: Velocity anomalies reveal weak zones and groundwater

<div class="info-box tip">
  <strong>Key idea</strong>
  Central Asia's seismic hazard makes $V_S$ profiling especially important. The 1948 Ashgabat and 1966 Tashkent earthquakes demonstrated how local site conditions amplify ground motion. MASW surveys provide essential data for modern building codes.
</div>

## Integration with other methods

### Seismic + ERT

Electrical resistivity tomography (ERT) and seismic refraction complement each other:

- **ERT** resolves water content, salinity, clay content
- **Seismic** resolves stiffness, compaction, lithologic boundaries
- **Joint interpretation** distinguishes saturated sand (low resistivity, high velocity) from clay (low resistivity, low-moderate velocity)

### Seismic + GPR

Ground-penetrating radar provides high resolution in resistive materials where seismic may lack contrast:

- **GPR** images stratigraphy, voids, and buried objects with centimeter resolution
- **Seismic** penetrates deeper and works in conductive soils where GPR fails
- **Combined** surveys characterize both shallow structure (GPR) and deeper contacts (seismic)

### Seismic + magnetometry

For archaeological sites:

- **Magnetometry** detects burned features, iron, magnetic soil enhancement
- **Seismic** detects structural elements, compaction, voids
- **Integration** reveals site complexity: magnetic anomalies from kilns, seismic anomalies from walls

### Multi-method workflow

1. **Regional reconnaissance**: Satellite imagery, historical records
2. **Rapid coverage**: Magnetometry or EM for large-area screening
3. **Detailed profiles**: Seismic refraction/reflection across key anomalies
4. **Material characterization**: MASW for engineering parameters
5. **Target investigation**: GPR for shallow high-resolution imaging
6. **Ground truth**: Trenching or coring at selected locations

## Field checklist for seismic surveys

<ul class="checklist">
  <li><strong>Equipment inventory:</strong> Seismograph, cables, geophones (plus spares), source (hammer, plate), trigger, batteries</li>
  <li><strong>Pre-survey calibration:</strong> Test all channels, verify trigger timing, check recording parameters</li>
  <li><strong>Site documentation:</strong> GPS coordinates, photographs, sketch maps, access notes</li>
  <li><strong>Spread layout:</strong> Mark geophone positions, measure and record spacing, ensure straight line</li>
  <li><strong>Geophone planting:</strong> Remove loose material, plant firmly vertical, check coupling</li>
  <li><strong>Noise assessment:</strong> Record ambient noise, identify and mitigate noise sources</li>
  <li><strong>Source preparation:</strong> Clear strike plate, verify trigger connection, establish stacking protocol</li>
  <li><strong>Quality control:</strong> Review each record in field, re-shoot if noisy or clipped</li>
  <li><strong>Field notes:</strong> Document shot locations, stack counts, unusual observations</li>
  <li><strong>Geometry recording:</strong> Note any deviations from planned layout</li>
  <li><strong>Equipment care:</strong> Coil cables properly, secure equipment during transport</li>
  <li><strong>Data backup:</strong> Copy files to separate media before leaving site</li>
</ul>

## Exercises

<div class="exercises-section">
  <h2>Easy exercises</h2>

  <div class="exercise"><h4>Easy 1 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What type of seismic wave arrives first at a geophone: P-wave or S-wave?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>P-wave (primary wave) arrives first because it travels faster than S-wave.</p></div></div>

  <div class="exercise"><h4>Easy 2 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What is the approximate P-wave velocity of water?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Approximately 1500 m/s.</p></div></div>

  <div class="exercise"><h4>Easy 3 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Why can't S-waves travel through water?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>S-waves are shear waves requiring a medium with shear strength. Water has zero shear modulus and cannot support shear stress.</p></div></div>

  <div class="exercise"><h4>Easy 4 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What does MASW stand for?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Multichannel Analysis of Surface Waves.</p></div></div>

  <div class="exercise"><h4>Easy 5 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What is the critical angle in seismic refraction?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>The angle of incidence at which the refracted wave travels parallel to the interface between layers.</p></div></div>

  <div class="exercise"><h4>Easy 6 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What seismic source is most commonly used for shallow surveys in remote areas?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Sledgehammer striking a metal plate.</p></div></div>

  <div class="exercise"><h4>Easy 7 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What does TWT stand for in reflection seismology?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Two-Way Travel time—the time for a wave to travel from source to reflector and back to receiver.</p></div></div>

  <div class="exercise"><h4>Easy 8 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What physical property does acoustic impedance combine?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Density ($\rho$) and velocity ($V$): $Z = \rho V$.</p></div></div>

  <div class="exercise"><h4>Easy 9 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>What type of geophone is typically used for MASW surveys?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Low-frequency geophones, typically 4.5 Hz natural frequency.</p></div></div>

  <div class="exercise"><h4>Easy 10 <span class="difficulty-tag difficulty-easy">Easy</span></h4><p>Why is seismic refraction unable to detect a low-velocity layer beneath a high-velocity layer?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>No critical refraction occurs when velocity decreases—the wave refracts away from the normal rather than along the interface, so no head wave is generated.</p></div></div>

  <h2>Medium exercises</h2>

  <div class="exercise"><h4>Medium 1 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>A P-wave travels through a layer at 500 m/s and encounters an interface where velocity increases to 2000 m/s. Calculate the critical angle.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$\theta_c = \arcsin(V_1/V_2) = \arcsin(500/2000) = \arcsin(0.25) = 14.5°$.</p></div></div>

  <div class="exercise"><h4>Medium 2 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>A refraction survey measures $V_1 = 400$ m/s, $V_2 = 1600$ m/s, and intercept time $t_i = 0.025$ s. Calculate the layer thickness.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$h = \frac{t_i V_1 V_2}{2\sqrt{V_2^2 - V_1^2}} = \frac{0.025 \times 400 \times 1600}{2\sqrt{1600^2 - 400^2}} = \frac{16000}{2\sqrt{2400000}} = \frac{16000}{3098} = 5.2$ m.</p></div></div>

  <div class="exercise"><h4>Medium 3 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>If a reflection occurs at 50 m depth in material with velocity 1000 m/s, what is the two-way travel time?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$t_0 = 2z/V = 2 \times 50/1000 = 0.1$ s = 100 ms.</p></div></div>

  <div class="exercise"><h4>Medium 4 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>Calculate the reflection coefficient at an interface where $\rho_1 = 1800$ kg/m³, $V_1 = 500$ m/s, $\rho_2 = 2200$ kg/m³, $V_2 = 2500$ m/s.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$Z_1 = 1800 \times 500 = 900,000$; $Z_2 = 2200 \times 2500 = 5,500,000$. $R = (Z_2 - Z_1)/(Z_2 + Z_1) = (5,500,000 - 900,000)/(5,500,000 + 900,000) = 4,600,000/6,400,000 = 0.72$.</p></div></div>

  <div class="exercise"><h4>Medium 5 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>What vertical resolution is achievable at 80 Hz dominant frequency in material with $V = 600$ m/s?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$\Delta z_{min} = V/(4f) = 600/(4 \times 80) = 600/320 = 1.9$ m.</p></div></div>

  <div class="exercise"><h4>Medium 6 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>If $V_P/V_S = 1.73$, what is Poisson's ratio?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>From $V_P/V_S = \sqrt{(2-2\nu)/(1-2\nu)}$, squaring: $3 = (2-2\nu)/(1-2\nu)$. Solving: $3(1-2\nu) = 2-2\nu$; $3-6\nu = 2-2\nu$; $1 = 4\nu$; $\nu = 0.25$.</p></div></div>

  <div class="exercise"><h4>Medium 7 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>A refraction spread has crossover distance $x_c = 30$ m with $V_1 = 350$ m/s and $V_2 = 1750$ m/s. What is the layer thickness?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$h = (x_c/2)\sqrt{(V_2-V_1)/(V_2+V_1)} = (30/2)\sqrt{(1750-350)/(1750+350)} = 15\sqrt{1400/2100} = 15\sqrt{0.667} = 15 \times 0.816 = 12.2$ m.</p></div></div>

  <div class="exercise"><h4>Medium 8 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>What is the Fresnel zone radius for a reflector at 10 m depth, frequency 100 Hz, and velocity 800 m/s?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$r_F = \sqrt{Vz/(2f)} = \sqrt{800 \times 10/(2 \times 100)} = \sqrt{8000/200} = \sqrt{40} = 6.3$ m.</p></div></div>

  <div class="exercise"><h4>Medium 9 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>Explain why MASW penetration depth is approximately one-third to one-half of the spread length.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Surface waves sample depth proportional to wavelength. The longest wavelengths (lowest frequencies) that can be reliably measured require array lengths of at least 2–3 wavelengths. Therefore, maximum wavelength ≈ $L/2$ to $L/3$, and since penetration depth ≈ wavelength/2 to wavelength, investigation depth is roughly $L/3$ to $L/2$.</p></div></div>

  <div class="exercise"><h4>Medium 10 <span class="difficulty-tag difficulty-medium">Medium</span></h4><p>A 24-channel system with 2 m geophone spacing uses hammer source at both ends. How many shot points are there, and what is the spread length?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Spread length = (24-1) × 2 = 46 m. With shots at both ends, there are 2 shot points (plus optional center shot makes 3). Offset shots outside the spread may add 2 more.</p></div></div>

  <h2>Hard exercises</h2>

  <div class="exercise"><h4>Hard 1 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Derive the intercept time formula for a two-layer model. Starting from the refracted ray path geometry, show that $t_i = 2h\cos\theta_c/V_1$.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>The refracted ray travels: (1) down through layer 1 at angle $\theta_c$ to depth $h$, path length = $h/\cos\theta_c$; (2) along the interface; (3) up through layer 1 at angle $\theta_c$, path length = $h/\cos\theta_c$. Total time in layer 1 = $2(h/\cos\theta_c)/V_1 = 2h/(V_1\cos\theta_c)$. The horizontal distance traveled while in layer 1 = $2h\tan\theta_c$. Time along interface for distance $x$ = $(x - 2h\tan\theta_c)/V_2$. Total time = $2h/(V_1\cos\theta_c) + (x - 2h\tan\theta_c)/V_2$. Rearranging: $t = x/V_2 + 2h/(V_1\cos\theta_c) - 2h\tan\theta_c/V_2$. Using $\sin\theta_c = V_1/V_2$: $t = x/V_2 + 2h(1/V_1\cos\theta_c - \sin\theta_c/(V_2\cos\theta_c)) = x/V_2 + 2h(V_2 - V_1\sin\theta_c)/(V_1 V_2\cos\theta_c)$. Since $\sin\theta_c = V_1/V_2$: $= x/V_2 + 2h(V_2 - V_1^2/V_2)/(V_1 V_2\cos\theta_c) = x/V_2 + 2h(V_2^2 - V_1^2)/(V_1 V_2^2\cos\theta_c)$. This simplifies to $t = x/V_2 + 2h\cos\theta_c/V_1 = x/V_2 + t_i$.</p></div></div>

  <div class="exercise"><h4>Hard 2 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>A dipping interface gives apparent velocities of 1200 m/s (downdip) and 2400 m/s (updip), with $V_1 = 500$ m/s. Calculate the true refractor velocity and dip angle.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>From apparent velocities: $\sin(\theta_c + \alpha) = V_1/V_{2d} = 500/1200 = 0.417$; $\sin(\theta_c - \alpha) = V_1/V_{2u} = 500/2400 = 0.208$. Therefore: $\theta_c + \alpha = 24.6°$ and $\theta_c - \alpha = 12.0°$. Adding: $2\theta_c = 36.6°$, so $\theta_c = 18.3°$. Subtracting: $2\alpha = 12.6°$, so $\alpha = 6.3°$. True velocity: $V_2 = V_1/\sin\theta_c = 500/\sin(18.3°) = 500/0.314 = 1592$ m/s.</p></div></div>

  <div class="exercise"><h4>Hard 3 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Design an MASW survey to determine $V_{S30}$ at a site where bedrock is expected at 25 m. Specify geophone spacing, spread length, source offset, and frequency range needed.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>For 30 m penetration, spread length should be 60–90 m. Using 24 geophones at 3 m spacing gives 69 m spread. Source offset: 30–50 m from nearest geophone. Frequency range: Low frequencies (2–5 Hz) penetrate to 30 m; high frequencies (30–50 Hz) resolve shallow layers. Minimum frequency ≈ $V_{R,min}/(L/2) = 150/(34.5) = 4.3$ Hz. Maximum frequency limited by spatial aliasing: $f_{max} < V_{R,min}/(2dx) = 150/(2×3) = 25$ Hz. Use 4.5 Hz geophones, record 2 s, sample at 1 ms.</p></div></div>

  <div class="exercise"><h4>Hard 4 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>A buried void (air-filled) creates a strong negative impedance contrast. If surrounding soil has $\rho = 1900$ kg/m³ and $V_P = 800$ m/s, and the void is air ($\rho = 1.2$ kg/m³, $V_P = 330$ m/s), calculate the reflection coefficient. What does this imply for void detection?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>$Z_{soil} = 1900 × 800 = 1,520,000$; $Z_{air} = 1.2 × 330 = 396$. $R = (Z_{air} - Z_{soil})/(Z_{air} + Z_{soil}) = (396 - 1,520,000)/(396 + 1,520,000) = -1,519,604/1,520,396 ≈ -0.9995$. The reflection coefficient approaches -1, meaning nearly total reflection with phase reversal. This strong reflection makes voids highly detectable by reflection methods, appearing as bright reversed-polarity events. The challenge is that the void roof must be resolved (quarter-wavelength criterion) and multiples from strong reflectors may complicate interpretation.</p></div></div>

  <div class="exercise"><h4>Hard 5 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Explain how joint inversion of P-wave refraction and MASW data can resolve ambiguities in determining water table depth in heterogeneous alluvium.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>In heterogeneous alluvium, velocity changes occur at both lithologic boundaries and the water table. P-wave refraction sees dramatic velocity increase at saturation (300→1500 m/s) but also at gravel/bedrock contacts. MASW measures $V_S$, which changes little at the water table (since $G$ is matrix-controlled) but responds to lithology. Joint inversion: (1) MASW identifies lithologic layering through $V_S$ structure; (2) P-wave refraction identifies all velocity interfaces; (3) Where $V_P$ jumps sharply but $V_S$ changes minimally, the interface is the water table; (4) Where both $V_P$ and $V_S$ change, the interface is lithologic. The ratio $V_P/V_S$ is diagnostic: it approaches 4–5 for saturated unconsolidated sediments, versus 1.7–2.5 for dry or lithologic contrasts.</p></div></div>

  <div class="exercise"><h4>Hard 6 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>A seismic reflection survey for archaeological walls uses 100 Hz source and expects targets at 2 m depth in loess ($V = 300$ m/s). Calculate vertical resolution, Fresnel zone radius, and minimum detectable wall thickness. What challenges do you anticipate?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Vertical resolution: $\lambda/4 = V/(4f) = 300/(4×100) = 0.75$ m. Fresnel zone: $r_F = \sqrt{Vz/(2f)} = \sqrt{300×2/(2×100)} = \sqrt{3} = 1.7$ m. Minimum wall thickness ≈ 0.75 m (λ/4 criterion). Challenges: (1) Archaeological walls are often thinner than 0.75 m—they may not produce distinct reflections; (2) Near-surface strong multiples from ground surface complicate shallow imaging; (3) Fresnel zone of 1.7 m means lateral resolution is poor—narrow walls appear smeared; (4) Source coupling in loose loess is difficult; (5) Cultural noise may be significant. Higher frequencies would improve resolution but attenuate rapidly in loess.</p></div></div>

  <div class="exercise"><h4>Hard 7 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>A refraction tomography survey reveals a low-velocity zone at 5–8 m depth beneath a proposed building site in the Ferghana Valley. Propose three geological hypotheses and additional investigations to distinguish them.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>Hypotheses: (1) **Paleochannel fill**: Loose sandy/silty channel deposits have lower velocity than surrounding floodplain clays. Test with borehole logs, grain size analysis, historical channel mapping. (2) **Collapsible loess**: Metastable loess structure creates low velocity; collapse upon wetting is an engineering hazard. Test with consolidation tests on samples, identify loess fabric microscopically. (3) **Archaeological fill**: Disturbed or organic-rich cultural deposits. Test with magnetometry (enhanced susceptibility), coring for artifacts/charcoal, radiocarbon dating. Additional investigations: (a) MASW for $V_S$ profile—very low $V_S$ suggests liquefaction risk; (b) ERT for resistivity—high resistivity suggests dry granular fill, low suggests clay or saturated conditions; (c) Drilling with SPT for engineering parameters.</p></div></div>

  <div class="exercise"><h4>Hard 8 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Derive the NMO equation showing that $t^2 = t_0^2 + x^2/V^2$ for a horizontal reflector at depth $z$ with uniform velocity $V$.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>For a source at position 0 and receiver at offset $x$, the reflection point is at horizontal position $x/2$ (for a horizontal reflector). The ray path from source to reflection point has length $\sqrt{z^2 + (x/2)^2}$. By symmetry, the path from reflection point to receiver has the same length. Total path length: $2\sqrt{z^2 + (x/2)^2} = 2\sqrt{z^2 + x^2/4}$. Travel time: $t = 2\sqrt{z^2 + x^2/4}/V$. Squaring: $t^2 = 4(z^2 + x^2/4)/V^2 = (4z^2 + x^2)/V^2 = 4z^2/V^2 + x^2/V^2$. Since $t_0 = 2z/V$, we have $t_0^2 = 4z^2/V^2$. Therefore: $t^2 = t_0^2 + x^2/V^2$. QED.</p></div></div>

  <div class="exercise"><h4>Hard 9 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>A three-layer refraction model has $V_1 = 400$ m/s, $V_2 = 1200$ m/s, $V_3 = 3000$ m/s. If layer 1 is 3 m thick and layer 2 is 8 m thick, calculate the intercept times for the two refracted arrivals.</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>For refraction from layer 2: $\theta_{c1} = \arcsin(400/1200) = 19.5°$. $t_{i1} = 2h_1\cos\theta_{c1}/V_1 = 2×3×\cos(19.5°)/400 = 6×0.943/400 = 0.014$ s = 14 ms. For refraction from layer 3, we need critical angles at both interfaces. $\theta_{c2} = \arcsin(1200/3000) = 23.6°$. The ray in layer 1 must also satisfy Snell's law: $\sin\theta_1/V_1 = \sin\theta_{c2}/V_2$, so $\sin\theta_1 = 400×\sin(23.6°)/1200 = 0.133$, $\theta_1 = 7.7°$. $t_{i2} = 2h_1\cos\theta_1/V_1 + 2h_2\cos\theta_{c2}/V_2 = 2×3×\cos(7.7°)/400 + 2×8×\cos(23.6°)/1200 = 5.95/400 + 14.65/1200 = 0.0149 + 0.0122 = 0.027$ s = 27 ms.</p></div></div>

  <div class="exercise"><h4>Hard 10 <span class="difficulty-tag difficulty-hard">Hard</span></h4><p>Compare the strengths and limitations of seismic refraction, seismic reflection, and MASW for investigating a suspected kurgan burial chamber at 4 m depth in a Central Asian steppe site. Which method(s) would you prioritize and why?</p><button class="solution-toggle">Show solution</button><div class="solution-block"><p>**Seismic refraction**: Can detect velocity decrease if chamber is air-filled or debris-filled, but hidden layer problem may cause it to be missed if sandwiched between higher-velocity layers. Strength: simple acquisition, robust depth estimates. Limitation: poor lateral resolution, may miss small features. **Seismic reflection**: Can image chamber roof as a reflector if impedance contrast is sufficient. At 4 m depth with ~100 Hz source, vertical resolution ~1 m may resolve chamber top. Strength: good depth positioning. Limitation: near-surface noise, requires careful processing, small chambers may not produce coherent reflections. **MASW**: Detects low-$V_S$ zone from disturbed/loose fill or void. Strength: sensitive to material stiffness changes. Limitation: 1D assumption may smear lateral boundaries, inversion non-uniqueness. **Recommended approach**: Start with MASW for rapid 2D $V_S$ section identifying anomalous zones. Follow with targeted reflection profiles across anomalies for depth confirmation. Use refraction to establish regional velocity structure. Combine with GPR for shallow high-resolution imaging of chamber architecture.</p></div></div>
</div>

## Final takeaway

Seismic methods reveal subsurface structure through the physics of elastic wave propagation. In Central Asia, these techniques address critical questions—Where is bedrock? How deep is the water table? What are the site's engineering properties? Where might buried archaeological features exist?

Mastering seismic interpretation requires understanding wave types, velocity relationships, and the distinct capabilities of refraction, reflection, and MASW. Each method has strengths: refraction for layer thickness, reflection for imaging interfaces, MASW for shear-wave velocity profiling. Combined with complementary geophysical methods and geological knowledge, seismic surveys provide essential subsurface information for engineering, archaeology, and resource exploration across Central Asia's diverse landscapes.
