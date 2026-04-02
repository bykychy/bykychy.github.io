---
layout: tutorial
title: "How Induced Polarization Reveals Mineral and Archaeological Targets in Central Asia"
description: "A practical tutorial on time-domain and frequency-domain IP, chargeability, phase angle, and applications for mining and archaeology in Central Asia, with 30 exercises and solutions."
difficulty: advanced
estimated_time: "3-4 hours"
series: treasure-hunting
order: 13
---

## Why This Tutorial Matters

Electrical resistivity tomography maps conductivity contrasts, but many targets — disseminated sulfides, ancient slag, buried metalwork — have resistivities identical to host rock. **Induced Polarization (IP)** detects these by measuring subsurface charge storage, invisible to resistivity alone.

Central Asia's Tien Shan and Altai belts host porphyry Cu–Au systems with fine-grained chalcopyrite in silicified rock. Along the Silk Road, slag and metal-rich soils lack resistivity contrast. IP exploits **electrochemical polarization** to find them.

| Target | Resistivity Contrast | IP Contrast |
|--------|---------------------|-------------|
| Disseminated sulfides in silicified host | Low | **High** |
| Massive sulfide lens | High | High |
| Clay alteration zone | Moderate | **High** |
| Slag deposit in alluvium | Low | **High** |
| Bronze artifact cluster | Very Low | **Moderate** |
| Graphite-bearing shear zone | High | **High** |

---

<div class="learning-objectives">
  <div class="learning-objectives-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#1e4f8a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <h3>What you will learn</h3>
  </div>
  <ul>
    <li>Distinguish electrode polarization (conductive grains) from membrane polarization (clay pore-throat effects) and predict which targets each mechanism detects</li>
    <li>Calculate chargeability from time-domain IP decay curves and interpret phase angle / percent frequency effect in frequency-domain IP</li>
    <li>Design IP survey arrays (dipole-dipole, gradient) with appropriate electrode spacings for mineral exploration and archaeological prospection</li>
    <li>Process IP data through contact-resistance checks, decay-curve editing, and 2D/3D inversion to produce chargeability cross-sections</li>
    <li>Apply IP methods to Central Asian targets: porphyry Cu–Au sulfide detection, Silk Road slag deposits, and clay-alteration zone mapping</li>
  </ul>
</div>

<div class="prerequisites">
  <div class="prerequisites-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#8b5e00" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
    <h3>Prerequisites</h3>
  </div>
  <ul>
    <li>Working knowledge of electrical resistivity theory and tomography (ERT) survey design</li>
    <li>Understanding of basic electrochemistry: ions, current flow in electrolytes, and Ohm's law</li>
    <li>Familiarity with geophysical inversion concepts (forward modelling, data misfit, regularization)</li>
    <li>Experience with resistivity data acquisition hardware (multi-electrode systems, cables, stainless-steel electrodes)</li>
  </ul>
</div>

<div class="concept-diagram">
  <svg viewBox="0 0 620 320" xmlns="http://www.w3.org/2000/svg" style="max-width: 580px;">
    <rect x="0" y="0" width="620" height="320" fill="#f9f8f6" rx="8"/>
    <text x="310" y="22" text-anchor="middle" font-family="Inter, sans-serif" font-size="13" font-weight="bold" fill="#111">Induced Polarization Measurement Principle</text>

    <!-- Ground surface -->
    <rect x="20" y="120" width="360" height="3" fill="#8b5e00"/>
    <text x="200" y="116" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#8b5e00">Ground Surface</text>

    <!-- Subsurface -->
    <rect x="20" y="123" width="360" height="90" fill="#f0ead6" opacity="0.5"/>

    <!-- Current electrodes C1 C2 -->
    <rect x="55" y="105" width="6" height="22" fill="#d92b1f" rx="1"/>
    <text x="58" y="100" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" font-weight="bold" fill="#d92b1f">C₁</text>
    <rect x="335" y="105" width="6" height="22" fill="#d92b1f" rx="1"/>
    <text x="338" y="100" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" font-weight="bold" fill="#d92b1f">C₂</text>

    <!-- Potential electrodes P1 P2 -->
    <rect x="155" y="105" width="6" height="22" fill="#1e4f8a" rx="1"/>
    <text x="158" y="100" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" font-weight="bold" fill="#1e4f8a">P₁</text>
    <rect x="235" y="105" width="6" height="22" fill="#1e4f8a" rx="1"/>
    <text x="238" y="100" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" font-weight="bold" fill="#1e4f8a">P₂</text>

    <!-- Transmitter box -->
    <rect x="30" y="60" width="60" height="28" rx="4" fill="#fdeaea" stroke="#d92b1f" stroke-width="1"/>
    <text x="60" y="78" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#d92b1f">Tx (I)</text>
    <line x1="58" y1="88" x2="58" y2="100" stroke="#d92b1f" stroke-width="1"/>

    <!-- Receiver box -->
    <rect x="170" y="60" width="60" height="28" rx="4" fill="#e8eef5" stroke="#1e4f8a" stroke-width="1"/>
    <text x="200" y="78" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#1e4f8a">Rx (V)</text>
    <line x1="158" y1="88" x2="158" y2="100" stroke="#1e4f8a" stroke-width="0.8"/>
    <line x1="238" y1="88" x2="238" y2="100" stroke="#1e4f8a" stroke-width="0.8"/>

    <!-- Wire connections -->
    <line x1="58" y1="88" x2="338" y2="88" stroke="#d92b1f" stroke-width="0.6" stroke-dasharray="3,2" opacity="0.4"/>

    <!-- Current flow arrows in subsurface -->
    <path d="M 58,130 Q 120,180 200,190 Q 280,180 338,130" fill="none" stroke="#d92b1f" stroke-width="1" stroke-dasharray="4,3" opacity="0.6"/>
    <text x="200" y="200" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#d92b1f">current flow lines</text>

    <!-- Polarizable body -->
    <ellipse cx="200" cy="165" rx="35" ry="18" fill="#8b5e00" opacity="0.3" stroke="#8b5e00" stroke-width="1"/>
    <text x="200" y="168" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" font-weight="bold" fill="#8b5e00">Sulfide / Slag</text>

    <!-- Grain-scale inset -->
    <rect x="20" y="220" width="160" height="90" rx="5" fill="#fff" stroke="#8b5e00" stroke-width="0.8"/>
    <text x="100" y="236" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#8b5e00">Grain-Scale Polarization</text>
    <!-- Pore space -->
    <rect x="35" y="245" width="130" height="40" rx="3" fill="#e8eef5" opacity="0.5"/>
    <!-- Conductive grain -->
    <ellipse cx="75" cy="265" rx="18" ry="12" fill="#68625b" opacity="0.6" stroke="#111" stroke-width="0.8"/>
    <text x="75" y="268" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#fff">grain</text>
    <!-- Ion accumulation symbols -->
    <text x="53" y="260" font-family="Inter, sans-serif" font-size="9" fill="#d92b1f">+ +</text>
    <text x="91" y="260" font-family="Inter, sans-serif" font-size="9" fill="#1e4f8a">− −</text>
    <!-- Arrow showing current -->
    <line x1="35" y1="265" x2="50" y2="265" stroke="#d92b1f" stroke-width="1" marker-end="url(#arr13)"/>
    <line x1="100" y1="265" x2="155" y2="265" stroke="#d92b1f" stroke-width="1" marker-end="url(#arr13)"/>
    <text x="100" y="295" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#68625b">Double-layer capacitance at grain–fluid interface</text>

    <!-- Decay curve (right side) -->
    <rect x="400" y="40" width="205" height="270" rx="5" fill="#fff" stroke="#1e4f8a" stroke-width="0.8"/>
    <text x="502" y="58" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" font-weight="bold" fill="#111">Time-Domain IP Response</text>

    <!-- Axes -->
    <line x1="430" y1="75" x2="430" y2="280" stroke="#111" stroke-width="1"/>
    <line x1="430" y1="280" x2="590" y2="280" stroke="#111" stroke-width="1"/>
    <text x="420" y="178" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#111" transform="rotate(-90,420,178)">Voltage</text>
    <text x="510" y="296" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#111">Time</text>

    <!-- Primary voltage (on period) -->
    <line x1="430" y1="100" x2="500" y2="100" stroke="#d92b1f" stroke-width="2"/>
    <text x="465" y="94" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#d92b1f">Vp (current ON)</text>

    <!-- Current shutoff line -->
    <line x1="500" y1="75" x2="500" y2="285" stroke="#68625b" stroke-width="0.8" stroke-dasharray="3,2"/>
    <text x="504" y="85" font-family="Inter, sans-serif" font-size="7" fill="#68625b">off</text>

    <!-- Decay curve -->
    <path d="M 500,100 L 500,120 Q 510,200 530,240 Q 550,260 570,270 L 585,273" fill="none" stroke="#1e4f8a" stroke-width="2"/>

    <!-- Vs label -->
    <line x1="500" y1="120" x2="508" y2="120" stroke="#1e4f8a" stroke-width="0.8"/>
    <text x="516" y="124" font-family="Inter, sans-serif" font-size="8" fill="#1e4f8a">Vs</text>

    <!-- Chargeability shaded area -->
    <path d="M 510,192 Q 520,220 535,242 Q 545,254 555,260 L 555,280 L 510,280 Z" fill="#1e4f8a" opacity="0.15"/>
    <text x="536" y="254" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#1e4f8a">M (mV/V)</text>

    <!-- Integration window labels -->
    <line x1="510" y1="275" x2="510" y2="285" stroke="#111" stroke-width="0.8"/>
    <text x="510" y="296" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#111">t₁</text>
    <line x1="555" y1="275" x2="555" y2="285" stroke="#111" stroke-width="0.8"/>
    <text x="555" y="296" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#111">t₂</text>

    <!-- Zero baseline -->
    <line x1="430" y1="273" x2="590" y2="273" stroke="#68625b" stroke-width="0.5" stroke-dasharray="2,2"/>
    <text x="588" y="270" text-anchor="end" font-family="Inter, sans-serif" font-size="7" fill="#68625b">0 V</text>

    <defs>
      <marker id="arr13" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="5" markerHeight="5" orient="auto-start-reverse">
        <path d="M 0 0 L 10 5 L 0 10 z" fill="#d92b1f"/>
      </marker>
    </defs>
  </svg>
  <p class="diagram-caption">Figure — IP measurement setup with dipole-dipole electrodes (left), grain-scale electrode polarization mechanism (bottom-left), and time-domain chargeability decay curve (right).</p>
</div>

## The IP Phenomenon

### Electrode Polarization

When electrical current passes through rock containing conductive grains (sulfides, graphite, metals), ions accumulate at grain–fluid interfaces, creating **double-layer capacitance**. After current stops, stored charge dissipates, producing a measurable **decay voltage**.

The overvoltage:

$$V_s = \frac{RT}{nF} \ln\left(\frac{i}{i_0}\right)$$

where $R = 8.314$ J mol⁻¹ K⁻¹, $T$ is temperature (K), $n$ is electron count, $F = 96\,485$ C mol⁻¹, $i$ is current density, and $i_0$ is exchange current density.

<div class="info-box tip">
<strong>Key Concept — Why Grain Size Matters</strong><br>
5 % fine pyrite (0.1 mm) produces much larger IP than the same fraction as coarse cubes (5 mm) because total grain-surface area — and double-layer capacitance — is dramatically larger.
</div>

### Membrane Polarization

Even without metallic minerals, rocks show IP effects. When pore throats narrow to ~1–100 nm, ion transport is selectively impeded, creating **ion concentration gradients**. This **membrane polarization** dominates in clay-rich sediments.

The relaxation time $\tau$ depends on diffusion path $l$ and coefficient $D$:

$$\tau = \frac{l^2}{2D}$$

For path lengths 0.01–1 mm and $D \approx 10^{-9}$ m²/s, $\tau$ falls in 0.05–500 ms — the standard IP measurement window.

<div class="info-box tip">
<strong>Key Concept — Two Polarization Mechanisms</strong><br>
<em>Electrode polarization</em>: conductive grains (sulfides, graphite, metals) → strong, long-duration IP. <em>Membrane polarization</em>: clay-rich pore throats → weaker, shorter-duration IP.
</div>

---

## Time-Domain IP

In TDIP, a square-wave current is injected. After shut-off at $t = 0$, the **secondary voltage** $V_s(t)$ decay is recorded.

### Chargeability

The Seigel chargeability is:

$$M = \frac{1}{V_p} \frac{1}{t_2 - t_1} \int_{t_1}^{t_2} V_s(t) \, dt$$

where $V_p$ is the primary voltage and $[t_1, t_2]$ is the measurement window after shut-off. Units are mV/V.

| Material | $M$ (mV/V) |
|----------|-----------|
| Barren sandstone | 1–5 |
| Clay-rich alluvium | 10–30 |
| 1 % disseminated pyrite | 20–60 |
| 5 % disseminated pyrite | 50–150 |
| Massive sulfides | 100–500 |
| Graphite schist | 50–200 |
| Ancient slag | 30–80 |

### Decay Curves

The decay voltage follows a Cole–Cole relaxation. In practice, it is sampled in logarithmically spaced **time gates** (20 gates from 20 ms to 2000 ms):

- **Fast decays** ($\tau$ < 100 ms): clay, membrane polarization
- **Slow decays** ($\tau$ > 100 ms): sulfide mineralization, graphite
- **Very slow decays** ($\tau$ > 1 s): massive sulfides, large metallic objects

<div class="info-box tip">
<strong>Key Concept — Multi-Gate Analysis</strong><br>
Recording many time gates separates targets by relaxation time. Sulfides decay slowly; clays decay fast. This spectral information is lost with a single chargeability window.
</div>

### Acquisition Protocol

A standard TDIP waveform uses **50 % duty cycle**: on $T$ s, off $T$ s, reversed $T$ s, off $T$ s. Common $T$: 1–8 s. Stacking $N$ cycles improves SNR by $\sqrt{N}$.

---

## Frequency-Domain IP

In FDIP, the transmitter injects sinusoidal or multi-frequency current; the receiver measures **amplitude** and **phase** of voltage relative to current.

### Percent Frequency Effect

The **percent frequency effect** (PFE):

$$\text{PFE} = \frac{\rho_{DC} - \rho_{AC}}{\rho_{AC}} \times 100\%$$

where $\rho_{DC}$ is apparent resistivity at low frequency (0.1 Hz) and $\rho_{AC}$ at higher frequency (10 Hz).

### Phase Angle

At frequency $\omega$, complex impedance is $Z(\omega) = |Z| \, e^{i\phi}$ where $\phi$ is **phase angle** (mrad). Phase relates to chargeability:

$$\phi \approx -M \cdot \frac{\pi}{4} \cdot c \quad \text{(small } M \text{)}$$

### Cole–Cole Model in Frequency Domain

The full Cole–Cole impedance is:

$$Z(\omega) = R_0 \left[1 - M\left(1 - \frac{1}{1 + (i\omega\tau)^c}\right)\right]$$

where $R_0$ is DC resistance, $M$ is chargeability, $\tau$ is relaxation time, $c$ is frequency exponent. Fitting multi-frequency data yields four parameters per voxel.

<div class="info-box tip">
<strong>Key Concept — Phase vs. Chargeability</strong><br>
Phase (FDIP) and chargeability (TDIP) measure the same property — polarisability — from complementary perspectives, related through Fourier transform theory.
</div>

---

## IP vs. Resistivity: Complementary Information

Resistivity $\rho$ and chargeability $M$ probe different properties. Resistivity: **porosity**, **salinity**, **connectivity**. Chargeability: **polarisable minerals**, **grain size**, **pore geometry**.

| $\rho$ | $M$ | Interpretation |
|--------|-----|----------------|
| Low | Low | Saline groundwater, clean sand |
| Low | High | Clay-rich zone, massive sulfides |
| High | Low | Dry limestone, unfractured granite |
| High | High | Disseminated sulfides in silicified rock |

The **metal factor** combines both: $\text{MF} = \frac{2\pi \times M \times 1000}{\rho}$.

High MF values (>100) strongly indicate metallic mineralization.

<div class="info-box tip">
<strong>Key Concept — Cross-Plotting</strong><br>
Always cross-plot $\rho$ against $M$. High-$M$, high-$\rho$ clusters in a porphyry system indicate disseminated sulfides in silicified rock — invisible on resistivity alone.
</div>

---

## Survey Design

### Array Selection

All standard resistivity arrays work for IP:

| Array | IP Suitability | Key Property |
|-------|---------------|-------------|
| Dipole–dipole | **Excellent** | Low EM coupling, good lateral resolution |
| Pole–dipole | Good | Stronger signal, moderate EM coupling |
| Wenner | Moderate | Strong signal but high EM coupling |
| Gradient | **Excellent** | Fast areal coverage |

The geometric factor for a general four-electrode array:

$$k = 2\pi \left(\frac{1}{r_1} - \frac{1}{r_2} - \frac{1}{r_3} + \frac{1}{r_4}\right)^{-1}$$

where $r_1, r_2, r_3, r_4$ are current-to-potential electrode distances.

### Current Injection and Noise Reduction

IP signals are small (1–10 mV on primaries of 100–1000 mV). Key noise sources:

- **EM coupling**: Mutual inductance between cables mimics IP decay. Minimised with dipole–dipole arrays.
- **Telluric currents**: Removed by stacking alternating-polarity cycles.
- **Electrode polarization noise**: Use non-polarising Cu/CuSO₄ potential electrodes.
- **Power-line harmonics**: 50 Hz in Central Asia. Use notch filters or synchronised timing.

Transmitter current should be maximised (0.5–5 A shallow, up to 20 A deep) because SNR scales linearly with $I$.

<div class="info-box tip">
<strong>Key Concept — EM Coupling</strong><br>
EM coupling is the largest systematic error source in IP. It produces negative apparent chargeability increasing with dipole separation and frequency. Use dipole–dipole arrays and keep Tx/Rx cables separated.
</div>

### Survey Layout for Central Asia Conditions

- **Arid soils**: Water electrodes with saline solution; use bentonite paste
- **Temperature swings**: Allow thermal equilibration above 40 °C
- **Remote logistics**: Battery-powered transmitters where fuel is scarce
- **Steep terrain**: 2D profiles along contours; 3D grids on gentle slopes

---

## Data Processing and Inversion

### Quality Control

Before inversion, inspect for:

1. **Negative apparent chargeabilities** — usually EM coupling; remove or flag
2. **Repeat errors** > 5 % — flag for removal
3. **Reciprocal errors** > 5 % — indicates electrode problems
4. **Low voltage** ($V_p < 1$ mV) or high contact resistance (> 10 kΩ) — reject

### Forward Modelling

The apparent chargeability $M_a$ uses the Seigel linearisation:

$$M_a = M \left[1 - \frac{\rho_a(\rho_1, \rho_2, \ldots)}{\rho_a(\rho_1(1 - M_1), \rho_2(1 - M_2), \ldots)}\right]$$

This computes $\rho_a$ with and without the IP effect. Valid when $M \ll 1$.

### 2D and 3D Inversion

Modern IP inversion simultaneously recovers $\rho(\mathbf{r})$ and $M(\mathbf{r})$. The objective function:

$$\Phi = \Phi_d + \beta \Phi_m = \| \mathbf{W}_d (\mathbf{d}^{\text{obs}} - \mathbf{d}^{\text{pred}}) \|^2 + \beta \| \mathbf{W}_m (\mathbf{m} - \mathbf{m}_{\text{ref}}) \|^2$$

where $\mathbf{W}_d$ is data weighting, $\mathbf{W}_m$ is regularisation, and $\beta$ is the trade-off parameter. Inversion proceeds sequentially: first resistivity, then chargeability with $\rho$ held fixed.

<div class="info-box tip">
<strong>Key Concept — Depth of Investigation</strong><br>
IP depth of investigation is shallower than resistivity. For dipole–dipole with dipole length $a$ and separation $n$: $z_{\text{med}} \approx 0.2 \times a \times (n + 1)$.
</div>

---

## Mineral Exploration Applications

### Disseminated Sulfides

Porphyry copper systems in the Tien Shan contain disseminated chalcopyrite, bornite, and pyrite. IP chargeabilities of 20–80 mV/V delineate the sulfide shell. The relationship:

$$M \approx \eta \cdot \phi_s^{0.8}$$

where $\eta$ is a scaling constant (400–600 mV/V for pyrite).

### Clay and Alteration Mapping

Argillic alteration zones produce **membrane polarization** mapping the alteration halo. The IP–resistivity cross-plot separates clay (low-$\rho$, moderate-$M$) from silicified core (high-$\rho$, high-$M$).

### Graphite Discrimination

Graphite schists in the Altai and Karatau produce very high IP ($M$ > 100 mV/V). Discrimination:

- **Resistivity**: graphite ($\rho$ < 10 Ω·m); sulfides: moderate-high $\rho$
- **Decay shape**: graphite $\tau$ > 1 s, $c \approx 0.5$; sulfides $c \approx 0.3$
- **Context**: graphite follows stratigraphy; sulfides cluster around intrusions

<div class="info-box tip">
<strong>Key Concept — The Graphite Problem</strong><br>
Graphite is the most common IP false positive in Central Asian greenstone belts. Graphite: low-$\rho$, high-$M$. Disseminated sulfides: high-$\rho$, high-$M$. Always examine both models together.
</div>

---

## Archaeological Applications

### Slag Deposits and Metallurgical Sites

Ancient smelting sites along the Silk Road left slag fields. Slag — glassy silicate with metallic prills — produces 30–80 mV/V against background of 2–10 mV/V.

### Metal Concentrations

Coin hoards, bronze caches, and iron deposits produce localised anomalies (15–60 mV/V at 0.5–2 m spacings). Minimum detectable mass at depth $z$:

$$m_{\min} \approx \frac{k \cdot z^3 \cdot N_{\min}}{\sigma_s \cdot I}$$

where $k$ is a geometric constant, $N_{\min}$ is noise floor, $\sigma_s$ is surface conductivity, $I$ is current. A 2 kg bronze hoard is detectable at 1 m.

### Integration with Other Archaeological Methods

IP is most powerful combined with **magnetometry** (fired features), **GPR** (structural detail), **ERT** (simultaneous), and **geochemistry** (validation).

---

## Central Asia Mining and Archaeological Sites

### Key Geological Provinces

1. **Tien Shan Copper–Gold Belt** (Kyrgyzstan, Kazakhstan): Porphyry and skarn deposits. IP targets: disseminated chalcopyrite–pyrite in quartz–sericite alteration.

2. **Altai Polymetallic Belt** (Kazakhstan, Russia): VMS deposits. IP distinguishes economic sulfide from barren pyrite halos.

3. **Karamazar–Chatkal District** (Uzbekistan, Tajikistan): Epithermal Au–Ag veins. IP maps silicified vein corridors.

4. **Muruntau Gold Province** (Uzbekistan): Giant gold deposit in black shale. IP maps the graphite–sulfide redox front.

### Key Archaeological Regions

1. **Fergana Valley**: Bronze Age to medieval metallurgy. IP maps buried slag and smelting debris.

2. **Zeravshan Corridor**: Multi-period urban sites with metal-working quarters.

3. **Semirech'ye** (SE Kazakhstan): Saka–Wusun kurgan fields with iron/bronze grave goods.

4. **Otrar Oasis** (S Kazakhstan): Medieval craft production — buried kilns and metalworking areas.

<div class="info-box tip">
<strong>Key Concept — Dual-Purpose Surveys</strong><br>
Many Central Asian archaeological sites sit on mineralised formations. A single IP survey can simultaneously assess mineral potential and map cultural features. Report both to stakeholders.
</div>

---

## Field Checklist

<ul class="checklist">
  <li>IP-capable resistivity meter with multi-gate recording</li>
  <li>High-power transmitter (≥ 250 W mineral; 50 W archaeology)</li>
  <li>Non-polarising Cu/CuSO₄ potential electrodes</li>
  <li>Stainless-steel current electrodes (≥ 40 cm)</li>
  <li>Multi-conductor cables with station markers</li>
  <li>Cable reels and connectors (test continuity before deployment)</li>
  <li>Saline solution and bentonite for dry-soil electrode contact</li>
  <li>12 V batteries or portable generator</li>
  <li>GPS/GNSS for electrode georeferencing</li>
  <li>Field laptop with QC and inversion software</li>
  <li>Contact resistance tester, electrode hammer, tapes</li>
  <li>Sun shelter for instruments</li>
  <li>Field notebook, camera, site permissions</li>
  <li>Safety: insulated gloves, high-voltage warning signs</li>
</ul>

---

## Exercises

### Fundamentals (Exercises 1–10)

**Exercise 1.** A TDIP survey records $V_p = 480$ mV and $\int_{t_1}^{t_2} V_s(t) \, dt = 72$ mV·s over a 2 s window. Calculate chargeability in mV/V.

<details><summary>Show Solution</summary>

$$M = \frac{1}{V_p} \frac{1}{t_2 - t_1} \int_{t_1}^{t_2} V_s(t) \, dt = \frac{1}{0.480} \times \frac{0.072}{2} = \frac{0.036}{0.480} = 0.075 \text{ V/V} = 75 \text{ mV/V}$$

A strong IP response, consistent with significant sulfide mineralization or slag.
</details>

**Exercise 2.** FDIP: $\rho = 250$ Ω·m at 0.1 Hz, $\rho = 210$ Ω·m at 10 Hz. Calculate PFE.

<details><summary>Show Solution</summary>

$$\text{PFE} = \frac{\rho_{DC} - \rho_{AC}}{\rho_{AC}} \times 100\% = \frac{250 - 210}{210} \times 100\% = 19.0\%$$

PFE of 19 % indicates strong polarisation — likely sulfide or graphite.
</details>

**Exercise 3.** Calculate MF for $M = 45$ mV/V and $\rho = 120$ Ω·m.

<details><summary>Show Solution</summary>

$$\text{MF} = \frac{2\pi \times M \times 1000}{\rho} = \frac{2\pi \times 45 \times 1000}{120} = 2356$$

MF = 2356 ≫ 100, strongly indicating metallic mineralization.
</details>

**Exercise 4.** Cole–Cole: $M = 0.08$, $\tau = 0.5$ s, $c = 0.35$. At $\omega = 2\pi$ rad/s, calculate $|(i\omega\tau)^c|$.

<details><summary>Show Solution</summary>

$$|( i\omega\tau)^c| = |\omega\tau|^c = (2\pi \times 0.5)^{0.35} = \pi^{0.35} = e^{0.35 \times 1.1447} = 1.493$$

A significant magnitude that would produce a measurable phase shift.
</details>

**Exercise 5.** Estimate $\tau$ for membrane polarization with $l = 0.2$ mm and $D = 1.5 \times 10^{-9}$ m²/s.

<details><summary>Show Solution</summary>

$$\tau = \frac{l^2}{2D} = \frac{(0.2 \times 10^{-3})^2}{2 \times 1.5 \times 10^{-9}} = \frac{4 \times 10^{-8}}{3 \times 10^{-9}} = 13.3 \text{ ms}$$

Short $\tau$ characteristic of clay membrane polarization — best captured in early time gates.
</details>

**Exercise 6.** A rock has 3 % pyrite. Using $M \approx 500 \cdot \phi_s^{0.8}$ mV/V, estimate chargeability.

<details><summary>Show Solution</summary>

$M = 500 \times (0.03)^{0.8}$. Compute $(0.03)^{0.8} = e^{0.8 \times \ln(0.03)} = e^{0.8 \times (-3.507)} = e^{-2.806} = 0.0605$.

$$M = 500 \times 0.0605 = 30.3 \text{ mV/V}$$

Clearly anomalous above typical background of 1–5 mV/V.
</details>

**Exercise 7.** Dipole–dipole array: $a = 25$ m, $n = 4$. Calculate geometric factor $k$.

<details><summary>Show Solution</summary>

For dipole–dipole: $k = \pi \cdot n(n+1)(n+2) \cdot a = \pi \times 4 \times 5 \times 6 \times 25 = 9425$ m.

The large geometric factor means measured voltage will be small, requiring high current.
</details>

**Exercise 8.** A survey uses $T = 4$ s on-time, 16 stacks. By what factor does stacking improve SNR?

<details><summary>Show Solution</summary>

SNR improvement $= \sqrt{N} = \sqrt{16} = 4$ (12 dB improvement).

Total time per station: $16 \times 4T = 256$ s ≈ 4.3 minutes.
</details>

**Exercise 9.** Phase angle $\phi = -22$ mrad at 1 Hz, $c = 0.4$. Estimate intrinsic chargeability $M$.

<details><summary>Show Solution</summary>

Using $\phi \approx -M \cdot \frac{\pi}{4} \cdot c$:

$$M = \frac{|\phi|}{(\pi/4) \cdot c} = \frac{0.022}{0.7854 \times 0.4} = \frac{0.022}{0.3142} = 0.070 = 70 \text{ mV/V}$$

High intrinsic chargeability — significant sulfide or graphite content.
</details>

**Exercise 10.** Butler–Volmer overvoltage at 25 °C: pyrite grain, $n = 2$, $i = 0.5$ A/m², $i_0 = 0.1$ A/m². Calculate $V_s$.

<details><summary>Show Solution</summary>

$$V_s = \frac{RT}{nF} \ln\left(\frac{i}{i_0}\right) = \frac{8.314 \times 298}{2 \times 96{,}485} \times \ln(5) = 0.01284 \times 1.6094 = 20.7 \text{ mV}$$

A measurable electrochemical potential contributing to the IP response.
</details>

### Survey Design and Processing (Exercises 11–20)

**Exercise 11.** You need to detect a sulfide body at 50 m depth using dipole–dipole with $n_{\max} = 6$. Using $z_{\text{med}} \approx 0.2 \times a \times (n+1)$, find minimum dipole length $a$.

<details><summary>Show Solution</summary>

$z_{\text{med}} = 0.2 \times a \times (n+1)$. With $z = 50$ m and $n = 6$:

$$a = \frac{50}{0.2 \times 7} = 35.7 \text{ m} \rightarrow \text{use } a = 40 \text{ m}$$

Total spread: $a \times (n + 2) = 40 \times 8 = 320$ m.
</details>

**Exercise 12.** A 64-electrode line at 10 m spacing uses dipole–dipole with $n = 1$ to $6$. How many unique data points?

<details><summary>Show Solution</summary>

For dipole–dipole with unit dipole ($a$ = electrode spacing):
$n=1$: $64-3=61$; $n=2$: $60$; $n=3$: $59$; $n=4$: $58$; $n=5$: $57$; $n=6$: $56$.

Total = $61+60+59+58+57+56 = 351$ data points (702 including both $\rho$ and $M$).
</details>

**Exercise 13.** Contact resistance at one electrode reads 8.5 kΩ. The transmitter delivers 2 A max at 800 V compliance. Can you inject sufficient current if both electrodes have similar resistance?

<details><summary>Show Solution</summary>

Total contact resistance: $R = 2 \times 8.5 = 17$ kΩ. Required voltage for 2 A: $V = 2 \times 17{,}000 = 34{,}000$ V — far above the 800 V limit.

Maximum current at 800 V: $I = 800/17{,}000 = 47$ mA — insufficient.

**Action**: Reduce contact resistance below 2 kΩ per electrode using saline solution or bentonite.
</details>

**Exercise 14.** A TDIP dataset has 351 points. After QC, 12 show negative chargeability and 8 have repeat errors > 10 %. What is the rejection rate, and is it acceptable?

<details><summary>Show Solution</summary>

Rejected: $12 + 8 = 20$ points. Rate: $20/351 \times 100\% = 5.7\%$.

Guidelines: < 5 % excellent; 5–10 % acceptable; 10–20 % marginal; > 20 % poor. The 5.7 % is acceptable for reliable inversion.
</details>

**Exercise 15.** An IP inversion reports $\chi^2 = 2.8$ with 300 data points. Is the fit acceptable?

<details><summary>Show Solution</summary>

Target $\chi^2 \approx 1.0$. A value of 2.8 means the model under-fits the data — noise estimates may be too optimistic, discretisation too coarse, or systematic EM coupling errors remain.

**Action**: Increase data uncertainties by $\sqrt{2.8} \approx 1.67\times$ and re-invert, or remove outliers. Target range: $\chi^2 = 0.8$–$1.2$.
</details>

**Exercise 16.** Design an IP survey for an archaeological site in the Fergana Valley with slag targets at 0.5–2 m depth. Recommend dipole length, array, and on-time.

<details><summary>Show Solution</summary>

**Dipole length**: $a = 1$ m. Median depth for $n=1$: $0.2 \times 1 \times 2 = 0.4$ m; for $n=6$: $1.4$ m. Covers target range.

**Array**: Dipole–dipole — minimal EM coupling, good lateral resolution.

**On-time**: $T = 2$ s (slag $\tau$ = 0.1–1 s). Use 4–8 stacks. Non-polarising electrodes essential at small spacings.
</details>

**Exercise 17.** A gradient-array IP survey covers 400 × 200 m. Transmitter electrodes are 600 m apart. Receiver dipoles: 20 m on a 20 × 20 m grid. How many receiver positions?

<details><summary>Show Solution</summary>

Receivers along 400 m: $400/20 + 1 = 21$. Along 200 m: $200/20 + 1 = 11$.

Total: $21 \times 11 = 231$ receiver positions, each yielding one $\rho$ and one $M$ value. The gradient array is efficient: transmitter stays fixed while receivers move.
</details>

**Exercise 18.** Convert a TDIP chargeability of 55 mV/V to an approximate phase angle at 1 Hz, assuming $c = 0.35$.

<details><summary>Show Solution</summary>

$$\phi = -M \cdot \frac{\pi}{4} \cdot c = -0.055 \times 0.7854 \times 0.35 = -15.1 \text{ mrad}$$

Moderately strong phase anomaly, consistent with sulfide mineralization or slag.
</details>

**Exercise 19.** A 2D inversion uses 5 m × 2.5 m cells with $a = 10$ m dipole–dipole. Is the cell size appropriate?

<details><summary>Show Solution</summary>

Horizontal cell (5 m) = $a/2$ — appropriate. Cells much smaller than $a/2$ are under-determined; larger than $a$ lose resolution. Vertical cell (2.5 m) = $a/4$ — good near surface. Deeper cells should increase logarithmically (e.g., 2.5, 3.5, 5, 7, 10 m) to avoid over-parameterisation.
</details>

**Exercise 20.** Why does EM coupling produce negative apparent chargeabilities? How do you recognise this in a pseudosection?

<details><summary>Show Solution</summary>

**Mechanism**: EM coupling from mutual inductance produces voltage with **opposite** polarity to IP decay, giving negative chargeability. Increases with cable length ($n$), frequency, and ground conductivity.

**Recognition**: Negative values at large $n$-separations forming a "curtain" along the pseudosection bottom; systematic increase with $n$; oscillating positive/negative values.

**Correction**: Remove negative values or apply EM-decoupling algorithms.
</details>

### Applications (Exercises 21–30)

**Exercise 21.** A porphyry prospect in the Tien Shan: $\rho = 800$ Ω·m, $M = 65$ mV/V. Calculate MF and interpret.

<details><summary>Show Solution</summary>

$$\text{MF} = \frac{2\pi \times 65 \times 1000}{800} = 510$$

MF = 510 ≫ 100, indicating metallic mineralization. High $\rho$ (800 Ω·m) rules out clay/graphite — consistent with disseminated sulfides in silicified porphyry rock. Warrants follow-up drilling.
</details>

**Exercise 22.** Two IP anomalies at a Silk Road site: A has $M = 52$ mV/V, $\tau \approx 50$ ms; B has $M = 38$ mV/V, $\tau \approx 800$ ms. Which is slag, which is buried metal?

<details><summary>Show Solution</summary>

**Anomaly 1** ($M=52$, $\tau=50$ ms): Fast decay → fine-grained metallic particles → **slag** (dispersed prills in glassy matrix, large surface area).

**Anomaly 2** ($M=38$, $\tau=800$ ms): Slow decay → large coherent metallic object → **buried metal** (e.g., bronze vessel). Multi-gate analysis is essential for this discrimination.
</details>

**Exercise 23.** At a VMS prospect in the Altai: Target A has $\rho = 5$ Ω·m, $M = 180$ mV/V; Target B has $\rho = 350$ Ω·m, $M = 90$ mV/V. Which is more likely economic?

<details><summary>Show Solution</summary>

**Target A** ($\rho=5$ Ω·m, $M=180$ mV/V): Very low $\rho$ + very high $M$ → likely **graphite** or massive pyrite (both extremely conductive and polarisable).

**Target B** ($\rho=350$ Ω·m, $M=90$ mV/V): Moderate-high $\rho$ + high $M$ → **disseminated sulfides in competent rock** — more likely economic in a VMS context.

MF_A = $2\pi \times 180 \times 1000 / 5 = 226{,}195$; MF_B = $2\pi \times 90 \times 1000 / 350 = 1617$. Both high, but Target B is more likely economic; Target A needs graphite assessment.
</details>

**Exercise 24.** An archaeological IP line: $a = 1$ m, dipole–dipole, $n = 1$–$4$, 51 electrodes over 50 m. How many data points?

<details><summary>Show Solution</summary>

$n=1$: $51-3=48$; $n=2$: $47$; $n=3$: $46$; $n=4$: $45$. Total = $186$ data points.

At 8 stacks with $T=2$ s (16 s/reading): $186 \times 16 = 2976$ s ≈ 50 minutes per line.
</details>

**Exercise 25.** A chargeability anomaly of 35 mV/V is found at 1.5 m depth; background is 5 mV/V. Assess detectability.

<details><summary>Show Solution</summary>

Anomaly-to-background ratio: $35/5 = 7.0$ (excellent; threshold is ~3).

Anomaly-to-noise ratio (assuming 0.5 mV/V noise): $(35 - 5)/0.5 = 60$. Unambiguously detectable.
</details>

**Exercise 26.** An IP survey has noise floor 2 mV/V and background $M = 4$ mV/V. Using $M = 500 \cdot \phi_s^{0.8}$, find the minimum detectable sulfide volume fraction.

<details><summary>Show Solution</summary>

$M_{\min} = M_{\text{bg}} + 3\sigma = 4 + 6 = 10$ mV/V. Setting $500 \cdot \phi_s^{0.8} = 10$:

$$\phi_s = (0.02)^{1.25} = e^{1.25 \times \ln(0.02)} = e^{-4.890} = 0.0075 = 0.75\%$$

IP can detect sulfide fractions as low as 0.75 % — remarkable sensitivity for disseminated mineralization.
</details>

**Exercise 27.** A deep IP survey in the Karamazar uses $I = 10$ A, $a = 100$ m dipole–dipole, $n = 6$. Calculate $k$ and expected voltage for $\rho = 200$ Ω·m.

<details><summary>Show Solution</summary>

$k = \pi \times 6 \times 7 \times 8 \times 100 = 105{,}558$ m.

$$\Delta V = \frac{\rho \times I}{k} = \frac{200 \times 10}{105{,}558} = 18.9 \text{ mV}$$

Secondary voltage at first gate (for $M = 40$ mV/V): $V_s \approx 0.040 \times 18.9 \times 0.5 = 0.38$ mV — near noise floor. High current and stacking essential.
</details>

**Exercise 28.** Three kurgans in Semirech'ye show IP anomalies: A = 12, B = 35, C = 8 mV/V. Background = 5 mV/V. Rank by likelihood of metal grave goods.

<details><summary>Show Solution</summary>

Anomalies above background: A: $12-5=7$ mV/V (ratio 2.4); B: $35-5=30$ mV/V (ratio 7.0); C: $8-5=3$ mV/V (ratio 1.6).

**Ranking**: 1. **Kurgan B** — strong anomaly, likely significant metal goods. 2. **Kurgan A** — moderate, possible smaller objects. 3. **Kurgan C** — marginal, may be geological noise.

Always integrate with magnetometry and GPR before excavation recommendations.
</details>

**Exercise 29.** Cole–Cole inversion yields: deep anomaly $M=0.12$, $\tau=2.5$ s, $c=0.28$; shallow anomaly $M=0.09$, $\tau=0.08$ s, $c=0.45$. Interpret both.

<details><summary>Show Solution</summary>

**Deep** ($M=120$ mV/V, $\tau=2.5$ s, $c=0.28$): High $M$, very long $\tau$, low $c$ → large conductive grains with broad size distribution → **massive/semi-massive sulfide**.

**Shallow** ($M=90$ mV/V, $\tau=0.08$ s, $c=0.45$): High $M$, short $\tau$, higher $c$ → fine-grained polarisable material → **clay alteration** or fine disseminated sulfide.

Combined pattern: clay cap over sulfide body — classic porphyry system. High-priority drill target.
</details>

**Exercise 30.** Design a complete IP survey for a 100 × 60 m ancient copper smelting site in the Zeravshan corridor. Specify all parameters and estimate survey duration.

<details><summary>Show Solution</summary>

- **Array**: Dipole–dipole, $a = 2$ m, $n = 1$–$6$
- **Line spacing**: 4 m, 16 lines across 60 m
- **Line length**: 100 m, 51 electrodes/line
- **Data points/line**: $48+47+46+45+44+43 = 273$; **Total**: $273 \times 16 = 4368$
- **On-time**: $T = 2$ s, 4 stacks → 32 s/reading
- **Raw time**: $4368 \times 32 / 3600 \approx 39$ h; with multi-channel system (~4× speedup): ~10 field hours, 2 days including setup

**Expected**: Slag → high-$M$ (30–80), moderate-$\rho$ (50–200 Ω·m). Furnaces → high-$\rho$, moderate-$M$. Metal dumps → moderate-$M$, localised geometry.
</details>

---

## Summary

Induced Polarization extends electrical methods beyond what resistivity alone achieves. By measuring charge storage and release — through electrode polarization at metallic surfaces and membrane polarization in clay-lined pores — IP reveals disseminated sulfides, graphite, slag, and buried metalwork otherwise invisible.

**Key takeaways**:

1. **TDIP** measures chargeability from voltage decay; **FDIP** measures phase shift. Both probe the same property.
2. **Multi-gate/multi-frequency** acquisition enables Cole–Cole spectral decomposition, discriminating targets by mineralogy and grain size.
3. **Cross-plot** $\rho$ vs $M$: high-$\rho$/high-$M$ = disseminated sulfides; low-$\rho$/high-$M$ = graphite or clay.
4. **EM coupling** is the primary artefact. Use dipole–dipole arrays and inspect for negative chargeabilities.
5. In **Central Asia**, IP is essential for porphyry Cu exploration (Tien Shan), VMS prospects (Altai), and Silk Road archaeological prospection.
6. **Archaeological IP** at sub-metre spacings maps slag, furnaces, and metal concentrations invisible to other methods.
