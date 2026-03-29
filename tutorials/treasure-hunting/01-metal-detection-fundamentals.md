---
layout: tutorial
title: "Metal Detection Fundamentals"
difficulty: beginner
estimated_time: "2-3 hours"
category: treasure-hunting
order: 1
---

## Introduction to Metal Detection in Central Asia

Central Asia sits at the crossroads of ancient civilizations. The Silk Road — the vast network of trade routes connecting China to the Mediterranean — passed directly through modern-day Kyrgyzstan, Kazakhstan, Uzbekistan, and Tajikistan. Scythian and Saka warrior cultures left behind elaborate burial mounds (kurgans) filled with gold and bronze artifacts. Turkic stone monuments, Chinese coins, Greek-influenced Bactrian metalwork, and Islamic-era treasures all lie buried beneath Central Asian soil.

Metal detection technology allows us to locate these buried metallic objects without excavation. Understanding the physics behind this technology — electromagnetic induction, eddy currents, and signal processing — is essential for anyone serious about archaeological prospection or treasure hunting in this region.

<div class="info-box warning">
<strong>⚖️ Legal Notice</strong>
Archaeological sites in Kyrgyzstan and neighboring countries are protected by law. Unauthorized excavation of cultural heritage sites is a criminal offense. Always obtain proper permits from the relevant Ministry of Culture and work with licensed archaeologists. This tutorial is for educational purposes.
</div>

## Physics of Electromagnetic Induction

Every metal detector relies on **Faraday's Law of Electromagnetic Induction**:

$$\mathcal{E} = -\frac{d\Phi_B}{dt}$$

where $\mathcal{E}$ is the induced electromotive force (EMF) in volts and $\Phi_B = \int \mathbf{B} \cdot d\mathbf{A}$ is the magnetic flux through a surface.

### How a Metal Detector Works — Step by Step

1. **Transmit coil** carries alternating current, generating a time-varying primary magnetic field $\mathbf{B}_p(t)$
2. This field penetrates the soil and reaches a **buried metallic target**
3. The changing flux through the target induces **eddy currents** (circular currents within the metal)
4. These eddy currents create their own **secondary magnetic field** $\mathbf{B}_s(t)$
5. The **receive coil** detects the secondary field, which differs in amplitude and phase from the primary field
6. Signal processing extracts target information (depth, size, conductivity, ferromagnetism)

### Skin Depth

Eddy currents don't penetrate uniformly into a conductor. They concentrate near the surface, decaying exponentially with depth. The **skin depth** $\delta$ is the depth at which the current density falls to $1/e$ (≈37%) of its surface value:

$$\delta = \sqrt{\frac{2}{\omega \mu \sigma}}$$

where:
- $\omega = 2\pi f$ is the angular frequency of the transmitted field (rad/s)
- $\mu = \mu_0 \mu_r$ is the magnetic permeability of the target ($\mu_0 = 4\pi \times 10^{-7}$ H/m)
- $\sigma$ is the electrical conductivity of the target (S/m)

| Material | Conductivity $\sigma$ (MS/m) | Skin depth at 10 kHz |
|----------|------------------------------|---------------------|
| Copper | 59.6 | 0.65 mm |
| Gold | 41.0 | 0.78 mm |
| Silver | 63.0 | 0.63 mm |
| Bronze | 7.0 | 1.90 mm |
| Iron | 10.0 (but $\mu_r \approx 200$) | 0.11 mm |

## VLF (Very Low Frequency) Metal Detectors

VLF detectors are the most common type, operating with a **continuous sinusoidal signal** typically between 3–30 kHz.

### Operating Principle

The transmit coil produces a magnetic field:

$$B_{tx}(t) = B_0 \sin(\omega t)$$

When this field encounters a buried target, the induced eddy currents produce a secondary field that is shifted in both **amplitude** and **phase** relative to the primary:

$$B_{rx}(t) = B_s \sin(\omega t + \phi)$$

The phase shift $\phi$ contains critical information:
- **Highly conductive targets** (silver, copper): Large phase shift (close to 90°)
- **Ferrous targets** (iron, steel): Small phase shift (close to 0°)
- **Medium conductivity** (gold, bronze): Intermediate phase shift

### Phase Angle Discrimination

By decomposing the received signal into two components:

$$B_{rx}(t) = \underbrace{B_s \cos\phi}_{R\text{ (resistive/in-phase)}} \sin(\omega t) + \underbrace{B_s \sin\phi}_{X\text{ (reactive/quadrature)}} \cos(\omega t)$$

The detector maps each target on a **conductivity-ferromagnetism plane** using the ratio $X/R$, enabling discrimination between junk (iron nails) and valuables (coins, jewelry).

### Ground Balance

Central Asian soils — particularly the **loess deposits** of the Fergana Valley and volcanic soils near Kyrgyzstan's Tien Shan — contain iron oxide minerals (magnetite, maghemite) that produce a ground signal. Ground balancing subtracts this mineral response:

$$B_{ground}(t) = B_g \sin(\omega t + \phi_g)$$

The detector's ground balance circuit adjusts to cancel $B_{ground}$, leaving only target signals. Manual ground balance is preferred in highly variable Central Asian terrain.

## Pulse Induction (PI) Metal Detectors

PI detectors use a fundamentally different approach — they transmit short, powerful **pulses** of current and analyze the **decay characteristics** of the induced response.

### Operating Principle

1. A brief current pulse (typically 10–40 μs) energizes the transmit coil
2. The pulse is abruptly terminated
3. Eddy currents induced in nearby targets continue to flow and decay
4. The detector measures the **exponential decay voltage**:

$$V(t) = V_0 \, e^{-t/\tau}$$

where $\tau$ is the **time constant** that depends on target properties.

### Target Time Constant

For a metallic sphere of radius $a$, conductivity $\sigma$, and permeability $\mu_0$:

$$\tau = \frac{\mu_0 \sigma a^2}{\pi^2}$$

This means:
- **Larger targets** → longer time constant → detected at later times
- **More conductive targets** → longer time constant
- **Small iron fragments** → very short time constant (can be filtered out)

### Why PI Excels in Central Asia

PI detectors are largely **immune to ground mineralization** because the ground mineral response decays much faster than metallic targets. This makes PI ideal for:
- Heavily mineralized volcanic soils near Issyk-Kul
- Salt-encrusted terrain in the Kazakh steppe
- "Hot rocks" common in mountain valleys

## Target Discrimination and Identification

### The Conductivity-Ferromagnetism Plane

Every metallic target occupies a position on a 2D plane defined by:
- **X-axis**: Electrical conductivity ($\sigma$)
- **Y-axis**: Magnetic susceptibility ($\chi_m$)

| Target | Conductivity | Ferromagnetism | Typical Target ID |
|--------|-------------|----------------|-------------------|
| Iron nail | Medium | High | 15-25 |
| Gold ring | Medium-High | None | 50-65 |
| Copper coin | High | None | 75-85 |
| Silver coin | Very High | None | 85-95 |
| Bronze artifact | Medium | Low | 55-70 |
| Aluminum foil | Medium | None | 65-75 |

### Central Asian Archaeological Targets

Common finds along the Silk Road include:
- **Kushan and Greco-Bactrian coins** (bronze, copper, silver): Target ID 60–95
- **Scythian gold** (high purity): Target ID 50–65
- **Iron arrowheads and tools**: Target ID 15–30
- **Bronze belt buckles and ornaments**: Target ID 55–70
- **Chinese copper coins** (square hole type): Target ID 75–85

## Mathematical Foundations

### Magnetic Dipole Approximation

At distances much greater than the coil radius, the transmit coil acts as a magnetic dipole. The field from a magnetic dipole with moment $m$ at distance $r$ along the axis:

$$B = \frac{\mu_0}{4\pi} \cdot \frac{2m}{r^3}$$

Off-axis, the full dipole field is:

$$\mathbf{B} = \frac{\mu_0}{4\pi r^3}\left[3(\mathbf{m} \cdot \hat{r})\hat{r} - \mathbf{m}\right]$$

### Signal vs. Depth: The Inverse Power Law

The signal strength $S$ from a buried target at depth $d$ follows approximately:

$$S \propto \frac{1}{d^6}$$

This arises because:
- Primary field at target: $B_p \propto 1/d^3$ (dipole field)
- Secondary field at coil: $B_s \propto 1/d^3$ (target also acts as dipole)
- Combined: $S \propto B_p \times B_s \propto 1/d^6$

For a coin-sized target ($\sim$25mm diameter), typical maximum detection depths:
- VLF at 10 kHz: ~25–30 cm
- PI detector: ~35–45 cm

### Detection Depth Estimation

The maximum detection depth $d_{max}$ for a target of radius $a$ can be estimated:

$$d_{max} \approx k \cdot a^{2/3} \cdot D^{1/3}$$

where $D$ is the coil diameter and $k$ is a detector-specific constant (typically 5–10 for modern detectors). This shows that **coil size matters** — a larger coil detects deeper but with less sensitivity to small targets.

## Practical Field Techniques for Central Asia

### Survey Grid Methods

For systematic coverage of an archaeological site:
1. **Establish a grid** using stakes and string (typically 20m × 20m squares)
2. **Walk parallel lines** with 50–75 cm spacing (overlapping detector swing)
3. **Record GPS coordinates** of each find with a handheld GPS/GNSS unit
4. **Mark signal strength** and target ID for each detection

### Altitude and Environmental Effects

Kyrgyzstan's terrain ranges from 500m (Fergana Valley) to 7,439m (Jengish Chokusu). At high altitude:
- **Air pressure** doesn't affect EM detection (the fields propagate through soil, not air)
- **Temperature** matters: cold temperatures reduce battery efficiency by 30–50%
- **Soil moisture** changes dramatically with season and altitude:
  - Wet soil: higher conductivity → more ground signal, reduced depth
  - Frozen soil: lower conductivity → reduced ground signal, better penetration
  - Dry loess: excellent penetration conditions (ideal for detection)

### Seasonal Considerations

| Season | Conditions | Detectability |
|--------|-----------|---------------|
| Spring (Apr-May) | Snowmelt, wet soil | Moderate — high ground signal |
| Summer (Jun-Aug) | Dry, hot | Excellent — low ground noise |
| Autumn (Sep-Oct) | Cooling, occasional rain | Good — moderate conditions |
| Winter (Nov-Mar) | Frozen ground, snow | Variable — frozen soil reduces mineralization but limits access |

<div class="info-box tip">
<strong>💡 Best Practice</strong>
The ideal time for metal detection surveys in Kyrgyzstan's valleys is late summer (July-August) when soils are dry and ground mineralization response is minimized. For high-altitude sites (above 3000m), the window is typically June-September.
</div>

---

<div class="exercises-section">
<h2>Exercises</h2>

<h3>Easy (Conceptual &amp; Basic Calculations)</h3>

<div class="exercise">
<h4>Exercise 1 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
<p>Calculate the skin depth $\delta$ in a copper target ($\sigma = 59.6$ MS/m, $\mu_r = 1$) at an operating frequency of $f = 8$ kHz.</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>Using $\delta = \sqrt{\frac{2}{\omega \mu \sigma}}$:</p>
<p>$\omega = 2\pi \times 8000 = 50{,}265$ rad/s</p>
<p>$\mu = \mu_0 = 4\pi \times 10^{-7} = 1.257 \times 10^{-6}$ H/m</p>
<p>$\sigma = 59.6 \times 10^6$ S/m</p>
<p>$$\delta = \sqrt{\frac{2}{50265 \times 1.257 \times 10^{-6} \times 59.6 \times 10^6}} = \sqrt{\frac{2}{3.766}} = \sqrt{0.5311} = 0.729 \text{ mm}$$</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 2 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
<p>A VLF detector operates at 12 kHz. What is the angular frequency $\omega$ and the period $T$ of the transmitted signal?</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>$\omega = 2\pi f = 2\pi \times 12{,}000 = 75{,}398$ rad/s</p>
<p>$T = 1/f = 1/12{,}000 = 83.3$ μs</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 3 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
<p>Why do PI detectors perform better than VLF detectors in the mineralized volcanic soils found near Issyk-Kul in Kyrgyzstan?</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>PI detectors measure the decay of eddy currents after the transmit pulse ends. Mineralized soil produces a response that decays very quickly (short time constant), while metallic targets produce responses with longer time constants. By sampling at later times after the pulse, PI detectors can effectively ignore the ground mineral signal. VLF detectors, in contrast, must continuously balance against the ground signal which is always present during transmission, making them more susceptible to ground noise in mineralized soils.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 4 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
<p>The magnetic field of a dipole decreases as $1/r^3$. If a detector measures a signal of 100 mV from a coin at 10 cm depth, what signal would the same coin produce at 20 cm depth?</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>Signal follows the inverse sixth power law: $S \propto 1/d^6$</p>
<p>$$\frac{S_2}{S_1} = \left(\frac{d_1}{d_2}\right)^6 = \left(\frac{10}{20}\right)^6 = \left(\frac{1}{2}\right)^6 = \frac{1}{64}$$</p>
<p>$S_2 = 100 \times \frac{1}{64} = 1.56$ mV</p>
<p>The signal drops by a factor of 64 when depth doubles! This illustrates why detection depth is so limited.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 5 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
<p>Calculate the time constant $\tau$ for a solid bronze sphere ($\sigma = 7.0$ MS/m) with radius $a = 15$ mm.</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>$\tau = \frac{\mu_0 \sigma a^2}{\pi^2}$</p>
<p>$= \frac{4\pi \times 10^{-7} \times 7.0 \times 10^6 \times (0.015)^2}{\pi^2}$</p>
<p>$= \frac{4\pi \times 10^{-7} \times 7.0 \times 10^6 \times 2.25 \times 10^{-4}}{\pi^2}$</p>
<p>$= \frac{4 \times 7.0 \times 2.25 \times 10^{-5}}{\pi} = \frac{6.3 \times 10^{-4}}{3.1416} = 2.0 \times 10^{-4}$ s $= 200$ μs</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 6 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
<p>A detector's receive signal has in-phase component $R = 3.2$ mV and quadrature component $X = 7.8$ mV. Calculate the phase angle $\phi = \arctan(X/R)$. Would this likely be a ferrous or non-ferrous target?</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>$\phi = \arctan\left(\frac{7.8}{3.2}\right) = \arctan(2.4375) = 67.7°$</p>
<p>A large phase angle (closer to 90°) indicates a <strong>non-ferrous, highly conductive target</strong> — likely copper or silver. Ferrous targets have small phase angles (closer to 0°) due to their high permeability dominating over conductivity.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 7 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
<p>Convert the following conductivities to resistivity: (a) Copper: $\sigma = 59.6$ MS/m, (b) Wet soil: $\sigma = 0.05$ S/m, (c) Dry sand: $\sigma = 0.001$ S/m.</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>Resistivity $\rho = 1/\sigma$:</p>
<p>(a) $\rho_{Cu} = 1/(59.6 \times 10^6) = 1.68 \times 10^{-8}$ Ω·m = 16.8 nΩ·m</p>
<p>(b) $\rho_{wet} = 1/0.05 = 20$ Ω·m</p>
<p>(c) $\rho_{dry} = 1/0.001 = 1000$ Ω·m</p>
<p>Note the enormous range — over 10 orders of magnitude between metal and dry ground.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 8 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
<p>What is the wavelength of the electromagnetic field transmitted by a VLF detector at 15 kHz? Can this wavelength resolve targets at typical archaeological depths (0.1–1 m)?</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>$\lambda = c/f = (3 \times 10^8)/(15 \times 10^3) = 20{,}000$ m = 20 km</p>
<p>The wavelength is enormously larger than the target depth. This means VLF metal detectors do <strong>not</strong> work by wave reflection (like radar). Instead, they operate in the <strong>near-field</strong> regime where the electromagnetic coupling is inductive (transformer-like), not radiative. The detection mechanism is electromagnetic induction, not wave propagation.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 9 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
<p>List three advantages and three disadvantages of VLF detectors compared to PI detectors for surveying a Scythian kurgan site in Kyrgyzstan.</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p><strong>VLF Advantages:</strong> (1) Better target discrimination — can distinguish gold from iron using phase analysis. (2) Lighter and generally cheaper. (3) Better sensitivity to small, shallow targets like coins.</p>
<p><strong>VLF Disadvantages:</strong> (1) Poor performance in mineralized soils common in Kyrgyz mountain terrain. (2) Shallower maximum detection depth (~25-30 cm for coins). (3) More ground noise in variable terrain requiring constant rebalancing.</p>
<p><strong>PI Advantages:</strong> (1) Excellent depth penetration (35-45+ cm). (2) Superior performance in mineralized/volcanic soils. (3) Less affected by ground variation.</p>
<p><strong>PI Disadvantages:</strong> (1) Poor discrimination — cannot easily distinguish iron from gold. (2) Heavier, more power-hungry. (3) Slower recovery time between pulses.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 10 <span class="difficulty-tag difficulty-easy">Easy</span></h4>
<p>Explain physically why the signal strength from a buried target decreases as $1/d^6$ rather than $1/d^3$.</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>The $1/d^6$ relationship comes from two separate $1/d^3$ factors:</p>
<p>1. The <strong>primary field</strong> from the transmit coil decays as $B_p \propto 1/d^3$ (magnetic dipole field). This determines how strongly the target is excited.</p>
<p>2. The <strong>secondary field</strong> from the target's eddy currents also decays as $B_s \propto 1/d^3$ on its way back to the receive coil (the target also acts as a magnetic dipole).</p>
<p>The received signal is proportional to the product: $S \propto B_p \times B_s \propto \frac{1}{d^3} \times \frac{1}{d^3} = \frac{1}{d^6}$</p>
<p>This is why doubling the depth reduces the signal by $2^6 = 64$ times.</p>
</div>
</div>

<h3>Medium (Multi-step Calculations)</h3>

<div class="exercise">
<h4>Exercise 11 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
<p>A gold ring ($\sigma = 41.0$ MS/m, $\mu_r = 1$) and an iron ring ($\sigma = 10.0$ MS/m, $\mu_r = 200$) have the same radius $a = 10$ mm. Compare their skin depths at $f = 10$ kHz and their PI time constants.</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p><strong>Skin depths</strong> ($\omega = 2\pi \times 10^4 = 62{,}832$ rad/s):</p>
<p>Gold: $\delta_{Au} = \sqrt{\frac{2}{62832 \times 4\pi\times10^{-7} \times 41\times10^6}} = \sqrt{\frac{2}{3.237}} = 0.786$ mm</p>
<p>Iron: $\delta_{Fe} = \sqrt{\frac{2}{62832 \times 200 \times 4\pi\times10^{-7} \times 10\times10^6}} = \sqrt{\frac{2}{157.9}} = 0.113$ mm</p>
<p>Iron's skin depth is ~7× smaller due to high permeability — eddy currents are confined to a very thin surface layer.</p>
<p><strong>PI time constants</strong> ($\tau = \mu_0 \sigma a^2/\pi^2$, using $\mu_0$ for non-ferrous):</p>
<p>Gold: $\tau_{Au} = \frac{4\pi\times10^{-7} \times 41\times10^6 \times 10^{-4}}{\pi^2} = \frac{5.152}{9.87} = 0.522$ ms = 522 μs</p>
<p>Iron: $\tau_{Fe} = \frac{4\pi\times10^{-7} \times 10\times10^6 \times 10^{-4}}{\pi^2} = 127$ μs</p>
<p>Gold has a longer time constant, so a PI detector sampling at later delay times (>300 μs) could preferentially detect gold over iron.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 12 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
<p>A PI detector can measure signals down to $V_{min} = 0.5$ μV. The initial response from a Kushan bronze coin at 10 cm depth is $V_0 = 2.0$ mV with $\tau = 180$ μs. At what sampling delay time $t$ does the signal drop below the noise floor?</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>$V(t) = V_0 e^{-t/\tau} = V_{min}$</p>
<p>$e^{-t/\tau} = \frac{V_{min}}{V_0} = \frac{0.5 \times 10^{-6}}{2.0 \times 10^{-3}} = 2.5 \times 10^{-4}$</p>
<p>$-t/\tau = \ln(2.5 \times 10^{-4}) = -8.294$</p>
<p>$t = 8.294 \times 180 = 1{,}493$ μs $\approx 1.5$ ms</p>
<p>The coin's signal is detectable for about 1.5 ms after the pulse. This gives the detector a window of roughly 8.3 time constants to sample the decay curve.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 13 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
<p>A VLF detector at 18 kHz measures a target with phase angle $\phi = 45°$ and signal amplitude 5.2 mV. Decompose into resistive (R) and reactive (X) components. Where would this target fall on a discrimination plot, and what might it be in a Silk Road context?</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>$R = A\cos\phi = 5.2\cos(45°) = 5.2 \times 0.707 = 3.68$ mV</p>
<p>$X = A\sin\phi = 5.2\sin(45°) = 5.2 \times 0.707 = 3.68$ mV</p>
<p>Equal R and X components with a 45° phase angle indicates a <strong>medium-conductivity, non-ferrous target</strong>. On a discrimination plot, this falls in the range of gold, lead, or large bronze objects.</p>
<p>In a Silk Road context, likely candidates: (1) a gold coin or jewelry piece, (2) a bronze ornament, or (3) a lead seal or weight used in trade.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 14 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
<p>Compare the maximum detection depth for the same detector coil ($D = 28$ cm) detecting: (a) a small gold earring ($a = 5$ mm) and (b) a bronze helmet ($a = 100$ mm). Use $d_{max} = k \cdot a^{2/3} \cdot D^{1/3}$ with $k = 8$.</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>$D^{1/3} = (0.28)^{1/3} = 0.654$ m$^{1/3}$</p>
<p>(a) Earring: $d_{max} = 8 \times (0.005)^{2/3} \times 0.654 = 8 \times 0.0292 \times 0.654 = 0.153$ m $= 15.3$ cm</p>
<p>(b) Helmet: $d_{max} = 8 \times (0.100)^{2/3} \times 0.654 = 8 \times 0.2154 \times 0.654 = 1.127$ m $= 113$ cm</p>
<p>The large bronze helmet can be detected at over 1 meter depth — 7× deeper than the tiny earring. This highlights why large Scythian kurgan offerings (helmets, cauldrons, horse tack) are detectable at much greater depths than small items.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 15 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
<p>An archaeological survey near Burana Tower in Kyrgyzstan covers a 100m × 100m area. If the detector sweep width is 0.6m and the walking speed is 1.5 km/h, how long will a complete survey take? How many parallel transect lines are needed?</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p><strong>Transect lines:</strong> $N = 100/0.6 = 167$ lines</p>
<p><strong>Total distance:</strong> $167 \times 100 = 16{,}700$ m = 16.7 km</p>
<p><strong>Walking time:</strong> $16.7/1.5 = 11.1$ hours of walking</p>
<p>Adding ~20% for turns, battery changes, target investigation: $\approx 13-14$ hours</p>
<p>This means approximately <strong>2 full days</strong> of surveying for a single 1-hectare area. For large sites, systematic metal detection is very time-intensive, which is why EM survey methods (Tutorial 2) or GPR (Tutorial 3) are often used for initial reconnaissance.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 16 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
<p>A VLF detector operates at two frequencies: $f_1 = 5$ kHz and $f_2 = 15$ kHz. Calculate the skin depth in bronze ($\sigma = 7.0$ MS/m) at both frequencies. How does this dual-frequency capability help with target identification?</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>At $f_1 = 5$ kHz: $\omega_1 = 31{,}416$ rad/s</p>
<p>$\delta_1 = \sqrt{\frac{2}{31416 \times 4\pi\times10^{-7} \times 7\times10^6}} = \sqrt{\frac{2}{27.7}} = 0.269$ mm = 2.69 mm</p>
<p>At $f_2 = 15$ kHz: $\omega_2 = 94{,}248$ rad/s</p>
<p>$\delta_2 = \sqrt{\frac{2}{94248 \times 4\pi\times10^{-7} \times 7\times10^6}} = \sqrt{\frac{2}{83.1}} = 0.155$ mm = 1.55 mm</p>
<p><strong>Dual-frequency benefit:</strong> At the lower frequency, eddy currents penetrate deeper into the target. The ratio of responses at two frequencies depends on the target's size relative to the skin depth. A thin foil (much thinner than $\delta$) responds differently than a solid coin (much thicker than $\delta$). Comparing the two responses helps distinguish thin junk (aluminum foil: ~0.02mm thick) from solid artifacts (coins: ~2mm thick).</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 17 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
<p>The PI decay curve from an unknown target gives $V_0 = 1.5$ mV and $V(500\text{ μs}) = 0.15$ mV. Determine the time constant $\tau$. Using $\tau = \mu_0 \sigma a^2/\pi^2$, if the target is copper ($\sigma = 59.6$ MS/m), estimate its radius.</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>From $V(t) = V_0 e^{-t/\tau}$:</p>
<p>$0.15 = 1.5 \times e^{-500/\tau}$</p>
<p>$e^{-500/\tau} = 0.1$</p>
<p>$-500/\tau = \ln(0.1) = -2.303$</p>
<p>$\tau = 500/2.303 = 217$ μs</p>
<p>Now solving for radius: $a = \sqrt{\frac{\tau \pi^2}{\mu_0 \sigma}} = \sqrt{\frac{217 \times 10^{-6} \times 9.87}{4\pi\times10^{-7} \times 59.6\times10^6}}$</p>
<p>$= \sqrt{\frac{2.142 \times 10^{-3}}{74.89}} = \sqrt{2.86 \times 10^{-5}} = 5.35 \times 10^{-3}$ m $= 5.4$ mm</p>
<p>This is consistent with a small copper coin or bead — common Silk Road artifacts.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 18 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
<p>Ground mineralization at a site in the Kazakh steppe produces a phase angle of $\phi_g = 88°$. A buried target produces a combined (ground + target) phase of $\phi_{total} = 72°$. The ground signal amplitude is 4 mV and total signal is 5.5 mV. Estimate the target's isolated signal amplitude and phase.</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>Decompose ground signal: $R_g = 4\cos(88°) = 0.140$ mV, $X_g = 4\sin(88°) = 3.998$ mV</p>
<p>Decompose total: $R_t = 5.5\cos(72°) = 1.700$ mV, $X_t = 5.5\sin(72°) = 5.231$ mV</p>
<p>Target only: $R_{target} = 1.700 - 0.140 = 1.560$ mV, $X_{target} = 5.231 - 3.998 = 1.233$ mV</p>
<p>Target amplitude: $A = \sqrt{1.560^2 + 1.233^2} = \sqrt{3.955} = 1.99$ mV</p>
<p>Target phase: $\phi = \arctan(1.233/1.560) = 38.3°$</p>
<p>A phase angle of ~38° suggests a medium-conductivity target like a bronze object or gold artifact.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 19 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
<p>Frozen ground in winter has conductivity approximately 10× lower than the same soil in summer. How does this change the skin depth at 10 kHz? If summer soil has $\sigma = 0.05$ S/m, what are the skin depths in both seasons?</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>$\omega = 2\pi \times 10^4 = 62{,}832$ rad/s, $\mu = \mu_0$</p>
<p><strong>Summer:</strong> $\delta_s = \sqrt{\frac{2}{62832 \times 4\pi\times10^{-7} \times 0.05}} = \sqrt{\frac{2}{3.948\times10^{-3}}} = \sqrt{506.6} = 22.5$ m</p>
<p><strong>Winter:</strong> $\sigma_w = 0.005$ S/m, $\delta_w = \sqrt{\frac{2}{62832 \times 4\pi\times10^{-7} \times 0.005}} = \sqrt{5066} = 71.2$ m</p>
<p>The EM field penetrates much deeper than typical target depths (both seasons). The skin depth in <em>soil</em> at these frequencies is large — the relevant attenuation is negligible for archaeology. However, the <strong>ground response amplitude</strong> (which is proportional to $\sigma$) is 10× weaker in winter, meaning less ground noise and potentially clearer target signals. This is one reason why frozen-ground detection can be effective despite challenging conditions.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 20 <span class="difficulty-tag difficulty-medium">Medium</span></h4>
<p>A detector's target ID system maps phase angles to a 0-99 scale where ID $= 99 \times \phi/90°$. Two targets are found: Target A has ID = 42, Target B has ID = 78. Calculate their phase angles and classify them (ferrous/non-ferrous, likely material).</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p><strong>Target A:</strong> $\phi_A = 42 \times 90/99 = 38.2°$ — Medium phase angle, non-ferrous. Likely gold, lead, or large bronze. In a Central Asian context: possibly a gold ornament or bronze buckle.</p>
<p><strong>Target B:</strong> $\phi_B = 78 \times 90/99 = 70.9°$ — High phase angle, highly conductive non-ferrous. Likely copper or silver. In Silk Road context: possibly a silver or copper trade coin.</p>
<p>Both targets are worth investigating. The ID system effectively linearizes the phase response to make field identification faster.</p>
</div>
</div>

<h3>Hard (Derivations &amp; Complex Problems)</h3>

<div class="exercise">
<h4>Exercise 21 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
<p>Derive the expression for the mutual inductance $M$ between a circular transmit coil of radius $R_c$ and a small circular target (conducting loop) of radius $a$ at depth $d$ on the coil axis. Start from the Biot-Savart law and the dipole approximation.</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p><strong>Step 1:</strong> The axial magnetic field from a circular coil of radius $R_c$ carrying current $I$ at distance $d$ on axis:</p>
<p>$$B_z(d) = \frac{\mu_0 I R_c^2}{2(R_c^2 + d^2)^{3/2}}$$</p>
<p><strong>Step 2:</strong> The flux through the small target loop (radius $a \ll d$, so $B$ is approximately uniform):</p>
<p>$$\Phi = B_z \cdot \pi a^2 = \frac{\mu_0 I R_c^2 \pi a^2}{2(R_c^2 + d^2)^{3/2}}$$</p>
<p><strong>Step 3:</strong> Mutual inductance $M = \Phi/I$:</p>
<p>$$M = \frac{\mu_0 \pi R_c^2 a^2}{2(R_c^2 + d^2)^{3/2}}$$</p>
<p><strong>Step 4:</strong> For $d \gg R_c$ (deep targets), $(R_c^2 + d^2)^{3/2} \approx d^3$:</p>
<p>$$M \approx \frac{\mu_0 \pi R_c^2 a^2}{2d^3}$$</p>
<p>This shows $M \propto 1/d^3$, confirming the dipole behavior. The received signal, proportional to $M^2$, gives $S \propto 1/d^6$.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 22 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
<p>A buried conducting sphere of radius $a$ and conductivity $\sigma$ is excited by a step function of magnetic field. Show that the decay of the induced magnetic moment follows $m(t) = m_0 \sum_{n=1}^{\infty} A_n e^{-n^2\pi^2 t/(\mu_0\sigma a^2)}$, and explain why the late-time behavior is dominated by the $n=1$ term with $\tau_1 = \mu_0\sigma a^2/\pi^2$.</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>The diffusion of the magnetic field into a conducting sphere is governed by:</p>
<p>$$\frac{\partial \mathbf{B}}{\partial t} = \frac{1}{\mu_0 \sigma}\nabla^2\mathbf{B}$$</p>
<p>In spherical coordinates with azimuthal symmetry, separation of variables gives eigenmodes with time constants:</p>
<p>$$\tau_n = \frac{\mu_0 \sigma a^2}{n^2 \pi^2}, \quad n = 1, 2, 3, \ldots$$</p>
<p>The general solution for the induced magnetic moment is a sum of exponentials:</p>
<p>$$m(t) = m_0 \sum_{n=1}^{\infty} A_n \exp\left(-\frac{t}{\tau_n}\right) = m_0 \sum_{n=1}^{\infty} A_n \exp\left(-\frac{n^2\pi^2 t}{\mu_0\sigma a^2}\right)$$</p>
<p><strong>Late-time dominance of $n=1$:</strong> Higher-order terms decay faster since $\tau_n = \tau_1/n^2$. For $n=2$, $\tau_2 = \tau_1/4$; for $n=3$, $\tau_3 = \tau_1/9$. After a few multiples of $\tau_2$, only the $n=1$ term survives:</p>
<p>$$m(t) \xrightarrow{t \gg \tau_2} m_0 A_1 e^{-t/\tau_1}$$</p>
<p>This is exactly the single-exponential decay $V(t) = V_0 e^{-t/\tau}$ that PI detectors measure. The fundamental time constant $\tau_1 = \mu_0\sigma a^2/\pi^2$ encodes both the size and conductivity of the target.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 23 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
<p>Design a complete metal detection survey for a 50m × 50m area at the Ak-Beshim archaeological site (ancient Suyab) near Tokmok, Kyrgyzstan. The soil is loess with moderate mineralization. Expected targets include Tang Dynasty Chinese coins (bronze, ~25mm diameter) at 10-30 cm depth. Specify: detector type, frequency, coil size, grid spacing, estimated time, and data recording protocol.</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p><strong>Detector choice:</strong> VLF multi-frequency detector (e.g., Minelab Equinox series). VLF is preferred because: (1) Tang coins are bronze — discrimination is essential to avoid iron nails from later periods. (2) Moderate mineralization is manageable with VLF ground balance. (3) Coins at 10-30 cm are within VLF range.</p>
<p><strong>Frequency:</strong> Multi-frequency (5/10/15/20 kHz simultaneous) or single frequency at 10 kHz — good compromise between depth and sensitivity for 25mm bronze coins.</p>
<p><strong>Coil:</strong> 11" DD (double-D) coil. DD provides better ground rejection than concentric in mineralized loess. Size gives effective sweep width of ~30 cm per pass.</p>
<p><strong>Grid spacing:</strong> 0.5m line spacing (provides overlap with 30cm effective sweep width). Lines parallel to site grid.</p>
<p><strong>Transect lines:</strong> $50/0.5 = 100$ lines × 50m = 5,000m total</p>
<p><strong>Estimated time:</strong> At 1.2 km/h careful walking speed: 5000/1200 = 4.2 hours walking. Add 50% for target investigation, turns, recording: ~6.5 hours = 1 full day.</p>
<p><strong>Protocol:</strong> (1) Set up 50m × 50m grid with corner stakes, GPS-referenced. (2) Mark transect lines with tape. (3) Ground balance at each transect start. (4) Record each detection: GPS position (±1m), target ID number, signal strength, estimated depth. (5) Flag each detection with numbered pin flags. (6) Photo-document flag positions. (7) Repeat in perpendicular direction for cross-validation if time permits.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 24 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
<p>Derive the relationship between the phase angle $\phi$ of the received signal and the induction parameter $\alpha = \omega\mu\sigma a^2$ for a small conducting sphere. Show that $\phi \to 0$ for $\alpha \to 0$ (small targets) and $\phi \to 90°$ for $\alpha \to \infty$ (large targets).</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>The complex magnetic polarizability of a conducting sphere in a uniform alternating field can be written as:</p>
<p>$$\chi(\alpha) = \frac{3}{2}\left[\frac{2(\sin\beta - \beta\cos\beta)}{(\beta^2 - 1)\sin\beta + \beta\cos\beta} - 1\right]$$</p>
<p>where $\beta = (1+i)\sqrt{\alpha/2} = (1+i)a/\delta$.</p>
<p>For the limiting cases:</p>
<p><strong>Small $\alpha$ ($a \ll \delta$, "Rayleigh" limit):</strong> The sphere is much smaller than the skin depth. Expanding for small $\beta$:</p>
<p>$$\chi \approx -\frac{i\alpha}{5} + O(\alpha^2)$$</p>
<p>This is purely imaginary (quadrature), but with magnitude $\propto \alpha$. The phase is $\phi = \arctan(\text{Im}/\text{Re}) \to 90°$ actually... Let's be more careful. The in-phase component goes as $\alpha^2$ while quadrature goes as $\alpha$, so $\phi = \arctan(\alpha/\alpha^2) = \arctan(1/\alpha) \to 90°$ for small $\alpha$.</p>
<p><strong>Large $\alpha$ ($a \gg \delta$, "skin depth" limit):</strong> Eddy currents are confined to a thin shell. The response approaches that of a perfectly conducting sphere:</p>
<p>$$\chi \to \frac{3}{2}\left[\frac{2}{1} - 1\right] = \frac{3}{2}$$</p>
<p>This is purely real (in-phase), so $\phi \to 0°$.</p>
<p>The physical interpretation: small targets have eddy currents that lag the applied field by 90° (inductive behavior), while large targets have surface currents that mirror the applied field with no lag (reflective behavior). Real targets fall between these limits, with $\phi$ decreasing as the induction parameter increases.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 25 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
<p>Two targets are buried at the same location but at different depths: a gold coin ($\sigma = 41$ MS/m, $a = 12$ mm) at $d_1 = 15$ cm and an iron nail ($\sigma = 10$ MS/m, $\mu_r = 200$, $a = 3$ mm) at $d_2 = 5$ cm. Calculate the relative signal amplitudes from each target and discuss how a PI detector could separate them using time-domain analysis.</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p><strong>Time constants:</strong></p>
<p>Gold coin: $\tau_{Au} = \frac{4\pi\times10^{-7} \times 41\times10^6 \times (0.012)^2}{\pi^2} = \frac{4\pi\times10^{-7} \times 41\times10^6 \times 1.44\times10^{-4}}{\pi^2}$</p>
<p>$= \frac{7.42 \times 10^{-4}}{9.87} = 75.1$ μs</p>
<p>Iron nail: $\tau_{Fe} = \frac{4\pi\times10^{-7} \times 10\times10^6 \times (0.003)^2}{\pi^2} = \frac{1.131\times10^{-4}}{9.87} = 11.5$ μs</p>
<p><strong>Initial signal amplitudes</strong> (proportional to $a^3/d^3$ for the mutual inductance factor, times $a^3/d^3$ again for the return path):</p>
<p>$S_{Au} \propto \frac{(0.012)^3}{(0.15)^6} = \frac{1.728\times10^{-6}}{1.139\times10^{-5}} = 0.152$</p>
<p>$S_{Fe} \propto \frac{(0.003)^3}{(0.05)^6} = \frac{2.7\times10^{-8}}{1.563\times10^{-8}} = 1.728$</p>
<p>The iron nail is initially ~11× stronger despite being smaller, due to being much shallower.</p>
<p><strong>Time-domain separation:</strong> At $t = 50$ μs, the iron nail's signal has decayed to $e^{-50/11.5} = e^{-4.35} = 1.3\%$ of its initial value, while the gold coin's signal is $e^{-50/75.1} = e^{-0.67} = 51\%$ of its initial value.</p>
<p>So at $t = 50$ μs: $S_{Fe}/S_{Au} \approx (11.4 \times 0.013)/(1 \times 0.51) = 0.29$</p>
<p>The gold coin's signal now dominates! By sampling at later delay times, a PI detector can effectively discriminate based on time constant — a powerful technique for separating overlapping targets.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 26 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
<p>The magnetic susceptibility of Central Asian loess varies between $\chi = 50 \times 10^{-5}$ SI (unburned) and $\chi = 500 \times 10^{-5}$ SI (thermally enhanced by ancient hearths/kilns). A VLF detector at 10 kHz has a circular coil of radius 14 cm at height 5 cm above ground. Estimate the change in ground response (in-phase component) when passing from natural loess over a buried hearth at 20 cm depth. Model the hearth as a horizontal disk of radius 50 cm.</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>The magnetic response of the ground is dominated by the in-phase component and is proportional to the magnetic susceptibility. To first order:</p>
<p>$$R_{ground} \propto \chi \int_V \frac{\partial B_z}{\partial z} \, dV$$</p>
<p>For the change over the hearth versus normal loess, we need:</p>
<p>$\Delta\chi = (500 - 50) \times 10^{-5} = 450 \times 10^{-5}$ SI</p>
<p>The vertical magnetic field from the coil at height $h$ above ground at depth $d$ below surface (total distance $h + d = 25$ cm = 0.25 m) along the axis:</p>
<p>$B_z = \frac{\mu_0 I R_c^2}{2(R_c^2 + z^2)^{3/2}}$ where $z = 0.25$ m, $R_c = 0.14$ m</p>
<p>$(R_c^2 + z^2)^{3/2} = (0.0196 + 0.0625)^{3/2} = (0.0821)^{3/2} = 0.02353$ m³</p>
<p>The anomalous response is approximately $\Delta R / R \propto \Delta\chi / \chi_{background} = 450/50 = 9\times$ the normal ground signal at the hearth location.</p>
<p>This 9× increase would saturate most detectors' ground balance, producing a strong false "target" signal. This is actually <strong>useful</strong> — magnetically enhanced soils from ancient burning are a key archaeological indicator. However, it means a metal detector survey near hearths will need frequent rebalancing, and the magnetic anomaly itself should be mapped as an archaeological feature.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 27 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
<p>A detector manufacturer claims their new VLF detector can detect a 25mm copper coin at 40 cm depth. Given the $1/d^6$ signal law, the minimum detectable signal is $V_{min} = 0.1$ μV, and a reference measurement gives $V = 5$ mV at $d = 5$ cm, is this claim physically plausible?</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>Using $V(d) = V_{ref} \times (d_{ref}/d)^6$:</p>
<p>$$V(40) = 5 \times 10^{-3} \times \left(\frac{5}{40}\right)^6 = 5 \times 10^{-3} \times \left(\frac{1}{8}\right)^6 = 5 \times 10^{-3} \times \frac{1}{262{,}144}$$</p>
<p>$$= 1.91 \times 10^{-8} \text{ V} = 19.1 \text{ nV}$$</p>
<p>Compare with $V_{min} = 0.1$ μV = 100 nV.</p>
<p>The predicted signal (19.1 nV) is <strong>5.2 times below</strong> the minimum detectable signal. The claim is <strong>not plausible</strong> under these assumptions.</p>
<p>However, the strict $1/d^6$ law applies to point dipole sources. At $d = 5$ cm (only 2× the coin diameter), the near-field effects make the actual signal stronger than dipole theory predicts. The $1/d^6$ law becomes more accurate at larger distances. So the reference signal at 5 cm is likely <em>higher</em> than the $1/d^6$ extrapolation would suggest, making the actual signal at 40 cm even weaker relative to the 5 cm measurement. The claim is physically implausible.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 28 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
<p>An inverse problem: A PI detector measures the following decay data from an unknown buried target. Determine the number of targets, their time constants, and estimate their sizes assuming copper ($\sigma = 59.6$ MS/m).</p>
<p>Data: $V(50\text{ μs}) = 3.2$ mV, $V(100\text{ μs}) = 2.1$ mV, $V(200\text{ μs}) = 1.05$ mV, $V(500\text{ μs}) = 0.20$ mV, $V(1000\text{ μs}) = 0.018$ mV.</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p>If there's a single exponential, $\ln V$ vs $t$ should be linear. Let's check:</p>
<p>$\ln(3.2) = 1.163$, $\ln(2.1) = 0.742$, $\ln(1.05) = 0.049$, $\ln(0.20) = -1.609$, $\ln(0.018) = -4.017$</p>
<p>Slope from 50→100 μs: $(0.742-1.163)/50 = -8.42 \times 10^{-3}$/μs → $\tau \approx 119$ μs</p>
<p>Slope from 500→1000 μs: $(-4.017-(-1.609))/500 = -4.82 \times 10^{-3}$/μs → $\tau \approx 208$ μs</p>
<p>The slope changes significantly — this is NOT a single exponential. The early-time steep decay plus late-time shallow decay indicates <strong>two targets</strong>: a small target (short $\tau$, dominates early) and a larger target (long $\tau$, dominates late).</p>
<p><strong>Late-time fit</strong> (500-1000 μs, dominated by slow component):</p>
<p>$\tau_2 = 208$ μs, $V_2(0) = 0.20 \times e^{500/208} = 0.20 \times 11.1 = 2.22$ mV</p>
<p><strong>Subtract slow component from early data:</strong></p>
<p>$V_1(50) = 3.2 - 2.22\times e^{-50/208} = 3.2 - 1.75 = 1.45$ mV</p>
<p>$V_1(100) = 2.1 - 2.22\times e^{-100/208} = 2.1 - 1.37 = 0.73$ mV</p>
<p>$\tau_1 = -50/\ln(0.73/1.45) = -50/(-0.686) = 73$ μs</p>
<p><strong>Size estimates</strong> from $a = \sqrt{\tau\pi^2/(\mu_0\sigma)}$:</p>
<p>Target 1: $a_1 = \sqrt{73\times10^{-6} \times 9.87/(4\pi\times10^{-7} \times 59.6\times10^6)} = \sqrt{9.62\times10^{-6}} = 3.1$ mm</p>
<p>Target 2: $a_2 = \sqrt{208\times10^{-6} \times 9.87/74.89} = \sqrt{2.74\times10^{-5}} = 5.2$ mm</p>
<p>Two copper targets: a ~6mm diameter bead/fragment and a ~10mm diameter coin or ornament piece.</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 29 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
<p>Derive an expression for the minimum detectable target radius $a_{min}$ as a function of depth $d$, coil radius $R_c$, operating frequency $\omega$, target conductivity $\sigma$, and detector sensitivity $V_{min}$. Assume the coil carries current $I_0$ and has $N$ turns.</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p><strong>Step 1 — Primary field at target:</strong> On-axis field at distance $d$:</p>
<p>$$B_p = \frac{\mu_0 N I_0 R_c^2}{2(R_c^2 + d^2)^{3/2}}$$</p>
<p><strong>Step 2 — Induced EMF in target:</strong> For a small sphere, the induced dipole moment is:</p>
<p>$$m_s = \frac{2\pi a^3}{\mu_0} \chi(\alpha) B_p$$</p>
<p>where $\chi(\alpha)$ is the response function and $\alpha = \omega\mu_0\sigma a^2$. In the Rayleigh limit ($a \ll \delta$): $\chi \approx -i\alpha/5 = -i\omega\mu_0\sigma a^2/5$.</p>
<p><strong>Step 3 — Secondary field at coil:</strong></p>
<p>$$B_s = \frac{\mu_0}{4\pi}\frac{2m_s}{(R_c^2+d^2)^{3/2}}$$</p>
<p><strong>Step 4 — Received voltage:</strong></p>
<p>$$V = N\omega\pi R_c^2 B_s = N\omega\pi R_c^2 \cdot \frac{\mu_0 m_s}{2\pi(R_c^2+d^2)^{3/2}}$$</p>
<p><strong>Step 5 — Combine and solve for $a_{min}$:</strong></p>
<p>Setting $V = V_{min}$ and combining, we get (in the small-target Rayleigh limit):</p>
<p>$$a_{min}^5 = \frac{5 V_{min} (R_c^2 + d^2)^3}{\mu_0^2 \omega^2 \sigma N^2 I_0 R_c^4 \pi}$$</p>
<p>$$\boxed{a_{min} = \left[\frac{5 V_{min} (R_c^2 + d^2)^3}{\mu_0^2 \omega^2 \sigma N^2 I_0 R_c^4 \pi}\right]^{1/5}}$$</p>
<p>Key insights: $a_{min} \propto d^{6/5}$ (for $d \gg R_c$), $a_{min} \propto 1/\omega^{2/5}$ (higher frequency helps), $a_{min} \propto 1/\sigma^{1/5}$ (more conductive targets are easier to detect).</p>
</div>
</div>

<div class="exercise">
<h4>Exercise 30 <span class="difficulty-tag difficulty-hard">Hard</span></h4>
<p>A team is planning a multi-technology survey of a suspected Sogdian merchant settlement along the Silk Road near Osh, Kyrgyzstan. The site is 200m × 300m on gently sloping terrain (5° grade) with mixed loess and alluvial soils. Expected features include stone building foundations (30-80 cm depth), metal artifacts (coins, tools, ornaments at 10-50 cm), and possible kiln/furnace remains. Design a phased survey strategy integrating metal detection with other geophysical methods, specifying instrument choices, survey parameters, and estimated timeframes for each phase.</p>
<button class="solution-toggle" onclick="this.nextElementSibling.style.display = this.nextElementSibling.style.display === 'none' ? 'block' : 'none'; this.textContent = this.nextElementSibling.style.display === 'none' ? '▶ Show Solution' : '▼ Hide Solution';">▶ Show Solution</button>
<div class="solution-block" style="display:none;">
<p><strong>Solution:</strong></p>
<p><strong>Phase 1 — Reconnaissance EM Survey (Days 1-3):</strong></p>
<p>Instrument: EM31 (Geonics) for rapid conductivity mapping. Line spacing: 2m, walking speed 3 km/h. Total lines: 150 × 200m = 30 km. Time: ~10 hours walking = 2 days with setup.</p>
<p>Purpose: Map large-scale features (building foundations create conductivity anomalies, kilns create magnetic susceptibility anomalies). Identify priority areas for detailed survey.</p>
<p><strong>Phase 2 — Magnetic Gradiometry (Days 3-5):</strong></p>
<p>Instrument: Bartington Grad-601 fluxgate gradiometer. Grid: 20m × 20m blocks across the most promising 2-hectare area identified in Phase 1. Line spacing: 0.5m, 8 readings/m.</p>
<p>Purpose: Detect kiln/furnace remains (strong magnetic anomalies), iron concentrations, and burnt features. This will map the settlement layout.</p>
<p><strong>Phase 3 — Targeted Metal Detection (Days 6-10):</strong></p>
<p>Instrument: Multi-frequency VLF (Minelab Equinox 900) for discrimination. Areas: Focus on 3-5 "hotspot" areas (each ~30m × 30m) identified by Phases 1-2 — doorways, middens, market areas.</p>
<p>Survey: 0.5m line spacing, systematic sweep, all targets logged with GPS, target ID, and depth estimate. ~5 days for all hotspot areas.</p>
<p><strong>Phase 4 — GPR Verification (Days 11-14):</strong></p>
<p>Instrument: GSSI SIR-4000 with 400 MHz antenna. Targeted profiles over key anomalies from all previous phases. 0.25m line spacing in priority blocks.</p>
<p>Purpose: Image building foundations, determine wall thickness, verify depth of features before excavation.</p>
<p><strong>Phase 5 — XRF Soil Survey (Days 15-16):</strong></p>
<p>Instrument: Portable XRF (Olympus Vanta). Grid: 5m spacing across full site (40 × 60 = 2,400 points). 30-second measurements.</p>
<p>Purpose: Map phosphorus (human activity), lead/copper (metallurgy areas), calcium (mortar/plaster) to reconstruct site functional zones.</p>
<p><strong>Total estimated time: 16 field days + 5 days data processing = 3 weeks.</strong></p>
<p>This phased approach moves from coarse reconnaissance to fine-scale targeting, minimizing time and maximizing information at each stage.</p>
</div>
</div>

</div>
