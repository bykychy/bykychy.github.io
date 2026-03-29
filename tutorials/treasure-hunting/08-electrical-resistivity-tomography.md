---
layout: tutorial
title: "How Electrical Resistivity Tomography Reveals Subsurface Structure in Central Asia"
description: "A math-first tutorial on ERT survey design, apparent resistivity, inversion, and subsurface imaging for archaeology and hydrogeology in Central Asia, with 30 exercises and solutions."
difficulty: advanced
estimated_time: "3-4 hours"
series: treasure-hunting
order: 8
---

## Why this tutorial matters

Electrical Resistivity Tomography (ERT) sees what other methods cannot. While **GPR** excels in dry soils and **EM methods** detect conductors efficiently, ERT provides quantitative subsurface imaging across Central Asia's challenging terrain—from saline valleys to frozen permafrost zones.

<div class="info-box tip">
  <strong>Key idea</strong>
  ERT is the only near-surface method providing true quantitative resistivity values at depth. Unlike EM apparent conductivity or GPR amplitudes, inverted ERT models give actual resistivity in Ω·m comparable across sites and seasons.
</div>

### Comparison with EM and GPR

| Property | ERT | EM | GPR |
|----------|-----|-----|-----|
| Output | True resistivity (Ω·m) | Apparent conductivity | Reflection amplitude |
| Depth control | Excellent | Limited | Poor |
| Saline soils | Works well | Works well | Fails |

## Electrical resistivity fundamentals

### Ohm's law in the ground

For a cylindrical conductor: $R = \rho \frac{L}{A}$ where $\rho$ is resistivity (Ω·m).

For a point electrode on a homogeneous half-space, the potential at distance $r$ is:

$$V(r) = \frac{I \rho}{2\pi r}$$

### The four-electrode measurement

Using four electrodes (A, B inject current; M, N measure voltage):

$$\Delta V = V_M - V_N = \frac{I\rho}{2\pi}\left(\frac{1}{AM} - \frac{1}{BM} - \frac{1}{AN} + \frac{1}{BN}\right)$$

### The geometric factor

The **geometric factor** $k$ simplifies this:

$$\rho_a = k \frac{\Delta V}{I}$$

where $k = \frac{2\pi}{\frac{1}{AM} - \frac{1}{BM} - \frac{1}{AN} + \frac{1}{BN}}$

**Apparent resistivity** $\rho_a$ represents the resistivity of a homogeneous half-space producing the same measurement.

## Array configurations

### Wenner array

Equal spacing $a$ between electrodes (A-M-N-B): $k_{Wenner} = 2\pi a$

**Advantages:** Strong signal, simple. **Disadvantages:** Poor horizontal resolution.

### Schlumberger array

For $L = AB/2$ and $l = MN/2$: $k_{Schlumberger} = \frac{\pi L^2}{l}$

### Dipole-dipole array

Current and potential dipoles separated by $na$: $k_{DD} = \pi n(n+1)(n+2)a$

<div class="info-box tip">
  <strong>Key idea</strong>
  Dipole-dipole is the workhorse of modern 2D ERT—excellent horizontal resolution for walls, ditches, and faults.
</div>

### Depth of investigation

| Array | Median depth |
|-------|-------------|
| Wenner | 0.17 × spread |
| Dipole-dipole (n=6) | 0.50 × spread |

## 2D and 3D survey design

For $N$ electrodes at spacing $a$:
- **Line length:** $(N-1) \times a$
- **Max depth:** ~$0.2 \times$ line length

**Survey planning:** $a = \min\left(\frac{z_{target}}{0.2}, \frac{w_{target}}{2}\right)$

## Data processing and inversion

### Quality control

**Reciprocal error:** $\epsilon_{rec} = \frac{|\rho_{normal} - \rho_{reciprocal}|}{mean} \times 100\%$. Remove data with $\epsilon > 5\%$.

### Inversion

Minimize: $\Phi = \|\mathbf{W}_d(\mathbf{d}_{obs} - \mathbf{d}_{calc})\|^2 + \lambda\|\mathbf{W}_m\mathbf{m}\|^2$

Target: $RMS = \sqrt{\frac{1}{N}\sum\left(\frac{d_{obs} - d_{calc}}{\epsilon}\right)^2} \approx 1$

## Central Asia applications

### Groundwater

**Fresh:** $> 50$ Ω·m | **Brackish:** $10-50$ Ω·m | **Saline:** $< 10$ Ω·m

### Archaeological features

| Feature | Resistivity |
|---------|-------------|
| Mud brick | 30-100 Ω·m |
| Stone walls | 200-1000+ Ω·m |
| Voids | > 1000 Ω·m |

### Permafrost

Frozen ground: $\rho_{frozen} = \rho_{unfrozen} \times (1 + \alpha \cdot S_i)$ where $\alpha > 100$.

## Integration with other methods

- **ERT + GPR:** ERT identifies where GPR will succeed ($\rho > 50$ Ω·m)
- **ERT + EM:** ERT calibrates EM and provides depth discrimination
- **ERT + Magnetics:** Combined interpretation distinguishes fired vs. stone structures

## Practical workflow

1. **Design:** Calculate spacing from depth/resolution requirements
2. **Setup:** Install electrodes, check contact resistance (< 5 kΩ)
3. **Acquire:** Run automated protocol, monitor quality
4. **Process:** Remove bad data, run inversion, validate RMS

## Field checklist

<ul class="checklist">
  <li>Resistivity meter with sufficient channels</li>
  <li>Multi-core cables (test continuity)</li>
  <li>Stainless steel electrodes (30+ cm)</li>
  <li>Electrode hammer and extraction tool</li>
  <li>Measuring tapes and GPS</li>
  <li>Water/bentonite for contact improvement</li>
  <li>Laptop with inversion software</li>
  <li>Field notebook and camera</li>
  <li>Permission documents</li>
</ul>

## Exercises

### Easy exercises (1-10)

<div class="exercise" id="ex1">
<strong>Exercise 1:</strong> A Wenner array with electrode spacing $a = 2$ m measures a voltage of 45 mV when injecting 100 mA current. Calculate the apparent resistivity.
<button class="solution-toggle" onclick="toggleSolution('sol1')">Show Solution</button>
<div class="solution" id="sol1" style="display:none;">
<strong>Solution:</strong><br>
For Wenner array: $k = 2\pi a = 2\pi \times 2 = 12.57$ m<br><br>
$\rho_a = k \frac{\Delta V}{I} = 12.57 \times \frac{0.045}{0.100} = 12.57 \times 0.45 = 5.66$ Ω·m
</div>
</div>

<div class="exercise" id="ex2">
<strong>Exercise 2:</strong> What is the geometric factor for a dipole-dipole array with $a = 1$ m and $n = 3$?
<button class="solution-toggle" onclick="toggleSolution('sol2')">Show Solution</button>
<div class="solution" id="sol2" style="display:none;">
<strong>Solution:</strong><br>
$k_{DD} = \pi n(n+1)(n+2)a = \pi \times 3 \times 4 \times 5 \times 1 = 60\pi = 188.5$ m
</div>
</div>

<div class="exercise" id="ex3">
<strong>Exercise 3:</strong> If ground resistivity is 100 Ω·m and a single electrode injects 500 mA, what is the potential at 5 m distance?
<button class="solution-toggle" onclick="toggleSolution('sol3')">Show Solution</button>
<div class="solution" id="sol3" style="display:none;">
<strong>Solution:</strong><br>
$V(r) = \frac{I\rho}{2\pi r} = \frac{0.500 \times 100}{2\pi \times 5} = \frac{50}{31.42} = 1.59$ V
</div>
</div>

<div class="exercise" id="ex4">
<strong>Exercise 4:</strong> A survey requires imaging to 20 m depth. Using a Wenner array, what minimum electrode spread is needed?
<button class="solution-toggle" onclick="toggleSolution('sol4')">Show Solution</button>
<div class="solution" id="sol4" style="display:none;">
<strong>Solution:</strong><br>
For Wenner: $z_e \approx 0.17 \times$ electrode spread (which is $3a$)<br><br>
$20 = 0.17 \times 3a$<br>
$a = \frac{20}{0.51} = 39.2$ m<br><br>
Minimum electrode spacing $a \approx 40$ m, giving spread of 120 m.
</div>
</div>

<div class="exercise" id="ex5">
<strong>Exercise 5:</strong> Convert apparent conductivity of 25 mS/m to apparent resistivity in Ω·m.
<button class="solution-toggle" onclick="toggleSolution('sol5')">Show Solution</button>
<div class="solution" id="sol5" style="display:none;">
<strong>Solution:</strong><br>
$\rho_a = \frac{1}{\sigma_a} = \frac{1}{0.025 \text{ S/m}} = 40$ Ω·m
</div>
</div>

<div class="exercise" id="ex6">
<strong>Exercise 6:</strong> A 48-electrode array with 2 m spacing is deployed. What is the maximum profile length and approximate maximum imaging depth?
<button class="solution-toggle" onclick="toggleSolution('sol6')">Show Solution</button>
<div class="solution" id="sol6" style="display:none;">
<strong>Solution:</strong><br>
Profile length: $(N-1) \times a = 47 \times 2 = 94$ m<br><br>
Maximum depth: $\approx 0.2 \times 94 = 18.8$ m
</div>
</div>

<div class="exercise" id="ex7">
<strong>Exercise 7:</strong> The reciprocal measurements for a data point are 85.2 Ω·m and 82.1 Ω·m. Calculate the reciprocal error and determine if the data should be kept.
<button class="solution-toggle" onclick="toggleSolution('sol7')">Show Solution</button>
<div class="solution" id="sol7" style="display:none;">
<strong>Solution:</strong><br>
$\epsilon_{rec} = \frac{|85.2 - 82.1|}{(85.2 + 82.1)/2} \times 100\% = \frac{3.1}{83.65} \times 100\% = 3.7\%$<br><br>
Since $\epsilon_{rec} < 5\%$, the data should be kept.
</div>
</div>

<div class="exercise" id="ex8">
<strong>Exercise 8:</strong> An aquifer has water resistivity of 20 Ω·m. If the formation factor is 4, what is the bulk rock resistivity?
<button class="solution-toggle" onclick="toggleSolution('sol8')">Show Solution</button>
<div class="solution" id="sol8" style="display:none;">
<strong>Solution:</strong><br>
$F = \frac{\rho_{rock}}{\rho_{water}}$<br><br>
$\rho_{rock} = F \times \rho_{water} = 4 \times 20 = 80$ Ω·m
</div>
</div>

<div class="exercise" id="ex9">
<strong>Exercise 9:</strong> A mud brick wall has resistivity 60 Ω·m in surrounding loess at 35 Ω·m. Calculate the resistivity contrast ratio.
<button class="solution-toggle" onclick="toggleSolution('sol9')">Show Solution</button>
<div class="solution" id="sol9" style="display:none;">
<strong>Solution:</strong><br>
Contrast ratio $= \frac{\rho_{wall}}{\rho_{background}} = \frac{60}{35} = 1.71$<br><br>
This moderate contrast should be detectable with ERT.
</div>
</div>

<div class="exercise" id="ex10">
<strong>Exercise 10:</strong> A Schlumberger sounding uses AB/2 = 50 m and MN/2 = 0.5 m. Calculate the geometric factor.
<button class="solution-toggle" onclick="toggleSolution('sol10')">Show Solution</button>
<div class="solution" id="sol10" style="display:none;">
<strong>Solution:</strong><br>
$k_{Schlumberger} = \frac{\pi L^2}{l} = \frac{\pi \times 50^2}{0.5} = \frac{2500\pi}{0.5} = 5000\pi = 15,708$ m
</div>
</div>

### Medium exercises (11-20)

<div class="exercise" id="ex11">
<strong>Exercise 11:</strong> For a pole-dipole array with $a = 3$ m and $n = 4$, calculate (a) the geometric factor and (b) the expected voltage if resistivity is 150 Ω·m and current is 200 mA.
<button class="solution-toggle" onclick="toggleSolution('sol11')">Show Solution</button>
<div class="solution" id="sol11" style="display:none;">
<strong>Solution:</strong><br>
(a) $k_{PD} = 2\pi n(n+1)a = 2\pi \times 4 \times 5 \times 3 = 120\pi = 377$ m<br><br>
(b) $\rho_a = k\frac{\Delta V}{I}$, so $\Delta V = \frac{\rho_a \times I}{k} = \frac{150 \times 0.200}{377} = 0.0796$ V = 79.6 mV
</div>
</div>

<div class="exercise" id="ex12">
<strong>Exercise 12:</strong> An ERT profile has 64 electrodes at 1.5 m spacing using dipole-dipole with maximum $n = 8$. Estimate the number of measurements and the approximate maximum imaging depth.
<button class="solution-toggle" onclick="toggleSolution('sol12')">Show Solution</button>
<div class="solution" id="sol12" style="display:none;">
<strong>Solution:</strong><br>
For dipole-dipole with $N$ electrodes and max $n$:<br>
Number of measurements $\approx (N-3) \times n_{max} - \frac{n_{max}(n_{max}-1)}{2}$<br>
$\approx 61 \times 8 - 28 = 488 - 28 = 460$ measurements (approximate)<br><br>
Profile length: $63 \times 1.5 = 94.5$ m<br>
Maximum depth: $\approx 0.2 \times 94.5 = 19$ m
</div>
</div>

<div class="exercise" id="ex13">
<strong>Exercise 13:</strong> Frozen permafrost has resistivity 5000 Ω·m while the unfrozen active layer shows 200 Ω·m. If the active layer is 2 m thick, sketch the expected apparent resistivity curve for a Wenner expanding spread centered over this structure.
<button class="solution-toggle" onclick="toggleSolution('sol13')">Show Solution</button>
<div class="solution" id="sol13" style="display:none;">
<strong>Solution:</strong><br>
At small $a$ (shallow penetration): $\rho_a \approx 200$ Ω·m (active layer dominates)<br>
At large $a$ (deep penetration): $\rho_a$ approaches 5000 Ω·m (permafrost dominates)<br><br>
The curve rises monotonically from ~200 Ω·m at small spacings to asymptotically approach 5000 Ω·m at large spacings. The transition occurs around $a \approx 2/0.17 \approx 12$ m where the median depth equals the layer boundary.
</div>
</div>

<div class="exercise" id="ex14">
<strong>Exercise 14:</strong> Calculate the formation factor for a sand with porosity 35% using Archie's equation with $a = 1$ and $m = 2$. If pore water has resistivity 8 Ω·m, what is the bulk sand resistivity?
<button class="solution-toggle" onclick="toggleSolution('sol14')">Show Solution</button>
<div class="solution" id="sol14" style="display:none;">
<strong>Solution:</strong><br>
$F = a\phi^{-m} = 1 \times (0.35)^{-2} = 1 \times 8.16 = 8.16$<br><br>
$\rho_{rock} = F \times \rho_{water} = 8.16 \times 8 = 65.3$ Ω·m
</div>
</div>

<div class="exercise" id="ex15">
<strong>Exercise 15:</strong> An inversion converges with RMS = 2.3 after 5 iterations. The data errors are estimated at 3%. Is this fit acceptable? What might cause elevated RMS?
<button class="solution-toggle" onclick="toggleSolution('sol15')">Show Solution</button>
<div class="solution" id="sol15" style="display:none;">
<strong>Solution:</strong><br>
With 3% data errors, target RMS should be close to 1.0. RMS = 2.3 indicates the model doesn't fit the data within errors.<br><br>
Possible causes:<br>
1. 3D effects not captured by 2D inversion<br>
2. Underestimated data errors<br>
3. Incorrect topography<br>
4. Model mesh too coarse<br>
5. Extreme resistivity contrasts causing non-uniqueness<br><br>
Consider increasing data error estimates, refining mesh, or using robust (L1) inversion.
</div>
</div>

<div class="exercise" id="ex16">
<strong>Exercise 16:</strong> Design a survey to image buried walls at 1-3 m depth with wall width ~0.8 m in an area 50 m × 50 m. Specify array type, electrode spacing, and line configuration.
<button class="solution-toggle" onclick="toggleSolution('sol16')">Show Solution</button>
<div class="solution" id="sol16" style="display:none;">
<strong>Solution:</strong><br>
<strong>Depth requirement:</strong> $a_{min} = z_{target}/0.2 = 3/0.2 = 15$ m spread needed (manageable with any spacing)<br><br>
<strong>Resolution requirement:</strong> $a_{max} = w_{target}/2 = 0.8/2 = 0.4$ m<br><br>
<strong>Recommended design:</strong><br>
- Array: Dipole-dipole (best horizontal resolution)<br>
- Electrode spacing: 0.5 m (resolves 1 m features)<br>
- Lines: Parallel lines at 2 m spacing, perpendicular to expected wall orientation<br>
- Electrodes per line: 64 (31.5 m length)<br>
- Number of lines: 25 to cover 50 m width<br>
- Use roll-along to extend coverage to 50 m length
</div>
</div>

<div class="exercise" id="ex17">
<strong>Exercise 17:</strong> A qanat (underground irrigation tunnel) appears as a 500 Ω·m anomaly in 80 Ω·m sediment at 8 m depth. If the tunnel diameter is 1.5 m, estimate whether it will be resolved with 2 m electrode spacing.
<button class="solution-toggle" onclick="toggleSolution('sol17')">Show Solution</button>
<div class="solution" id="sol17" style="display:none;">
<strong>Solution:</strong><br>
Maximum resolution: $w_{min} \approx 2a = 4$ m (conservative estimate)<br><br>
Tunnel diameter (1.5 m) < 4 m, so individual tunnel geometry won't be resolved. However:<br>
- Resistivity contrast: $500/80 = 6.25$ (strong)<br>
- Depth: 8 m requires spread of ~40 m (achievable)<br><br>
The tunnel will appear as a diffuse high-resistivity anomaly, detectable but not resolved in detail. For better resolution, use 0.5-1 m spacing, which would require more electrodes.
</div>
</div>

<div class="exercise" id="ex18">
<strong>Exercise 18:</strong> Compare signal strength for Wenner and dipole-dipole (n=4) arrays with same total spread of 30 m in 100 Ω·m ground with 100 mA current.
<button class="solution-toggle" onclick="toggleSolution('sol18')">Show Solution</button>
<div class="solution" id="sol18" style="display:none;">
<strong>Solution:</strong><br>
<strong>Wenner:</strong> Spread = 3a = 30 m, so $a = 10$ m<br>
$k_W = 2\pi a = 62.8$ m<br>
$\Delta V_W = \frac{\rho I}{k} = \frac{100 \times 0.1}{62.8} = 0.159$ V = 159 mV<br><br>
<strong>Dipole-dipole (n=4):</strong> Spread = $(n+2)a = 6a = 30$ m, so $a = 5$ m<br>
$k_{DD} = \pi n(n+1)(n+2)a = \pi \times 4 \times 5 \times 6 \times 5 = 600\pi = 1885$ m<br>
$\Delta V_{DD} = \frac{100 \times 0.1}{1885} = 0.0053$ V = 5.3 mV<br><br>
Wenner signal is 30× stronger, but dipole-dipole provides better horizontal resolution.
</div>
</div>

<div class="exercise" id="ex19">
<strong>Exercise 19:</strong> An irrigation canal has leaked, creating a plume of saline water (5 Ω·m) in otherwise fresh-water saturated sediment (60 Ω·m). Design an ERT monitoring survey to track plume migration over time.
<button class="solution-toggle" onclick="toggleSolution('sol19')">Show Solution</button>
<div class="solution" id="sol19" style="display:none;">
<strong>Solution:</strong><br>
<strong>Design considerations:</strong><br>
- Strong contrast (60/5 = 12×) ensures detectability<br>
- Monitoring requires consistent geometry for time-lapse comparison<br><br>
<strong>Recommended approach:</strong><br>
1. Install permanent electrodes to ensure exact repeatability<br>
2. Use dipole-dipole for lateral resolution of plume edges<br>
3. Electrode spacing: 2-5 m depending on expected plume size<br>
4. Multiple parallel lines perpendicular to canal<br>
5. Measurement frequency: weekly initially, monthly for long-term<br>
6. Process using time-lapse inversion with baseline constraint<br><br>
<strong>Time-lapse approach:</strong> Invert ratio of resistivities or percent change to highlight temporal changes while suppressing static features.
</div>
</div>

<div class="exercise" id="ex20">
<strong>Exercise 20:</strong> An archaeological site shows both magnetic anomalies (possible kilns) and EM conductivity highs (possible clay-lined pits). How would you design an ERT survey to investigate, and what would distinguish these feature types?
<button class="solution-toggle" onclick="toggleSolution('sol20')">Show Solution</button>
<div class="solution" id="sol20" style="display:none;">
<strong>Solution:</strong><br>
<strong>Survey design:</strong><br>
1. Run ERT lines across both anomaly types<br>
2. Use 0.5-1 m electrode spacing for structural detail<br>
3. Collect dense measurements (n up to 6-8) for depth resolution<br><br>
<strong>Distinguishing features:</strong><br>
<strong>Kilns (fired structures):</strong><br>
- Higher resistivity than surroundings (fired clay is less porous)<br>
- Often 100-300 Ω·m in 50 Ω·m background<br>
- Magnetic + resistive = kiln signature<br><br>
<strong>Clay-lined pits:</strong><br>
- Lower resistivity (clay + moisture retention)<br>
- Often 10-30 Ω·m<br>
- Conductive anomaly in both EM and ERT<br><br>
The combination of magnetics (thermal alteration) and ERT (physical properties) provides diagnostic capability that neither method alone achieves.
</div>
</div>

### Hard exercises (21-30)

<div class="exercise" id="ex21">
<strong>Exercise 21:</strong> Derive the geometric factor for a general four-electrode array where electrodes are at positions $x_A$, $x_B$, $x_M$, $x_N$ along a line. Show that the Wenner formula is a special case.
<button class="solution-toggle" onclick="toggleSolution('sol21')">Show Solution</button>
<div class="solution" id="sol21" style="display:none;">
<strong>Solution:</strong><br>
For electrodes on a line, distances are:<br>
$AM = |x_M - x_A|$, $BM = |x_M - x_B|$, $AN = |x_N - x_A|$, $BN = |x_N - x_B|$<br><br>
General geometric factor:<br>
$$k = \frac{2\pi}{\frac{1}{|x_M - x_A|} - \frac{1}{|x_M - x_B|} - \frac{1}{|x_N - x_A|} + \frac{1}{|x_N - x_B|}}$$<br><br>
<strong>Wenner case:</strong> $x_A = 0$, $x_M = a$, $x_N = 2a$, $x_B = 3a$<br>
$AM = a$, $BM = 2a$, $AN = 2a$, $BN = a$<br><br>
$$k = \frac{2\pi}{\frac{1}{a} - \frac{1}{2a} - \frac{1}{2a} + \frac{1}{a}} = \frac{2\pi}{\frac{2}{a} - \frac{1}{a}} = \frac{2\pi}{\frac{1}{a}} = 2\pi a$$<br><br>
This confirms the Wenner formula $k = 2\pi a$.
</div>
</div>

<div class="exercise" id="ex22">
<strong>Exercise 22:</strong> A two-layer earth has $\rho_1 = 50$ Ω·m (0-5 m depth) over $\rho_2 = 500$ Ω·m (below 5 m). Using the reflection coefficient method, estimate the apparent resistivity for a Wenner array with $a = 20$ m.
<button class="solution-toggle" onclick="toggleSolution('sol22')">Show Solution</button>
<div class="solution" id="sol22" style="display:none;">
<strong>Solution:</strong><br>
Reflection coefficient: $K = \frac{\rho_2 - \rho_1}{\rho_2 + \rho_1} = \frac{500 - 50}{500 + 50} = \frac{450}{550} = 0.818$<br><br>
For Wenner array, apparent resistivity involves image series. First-order approximation:<br><br>
The characteristic function for Wenner array at large spacings approaches:<br>
$$\rho_a \approx \rho_1 \left[1 + 4K\sum_{n=1}^{\infty}\frac{K^{n-1}}{\sqrt{1 + (2nh/a)^2}}\right]$$<br><br>
For $h = 5$ m, $a = 20$ m, $(h/a) = 0.25$:<br><br>
First term ($n=1$): $\frac{1}{\sqrt{1 + 0.25}} = 0.894$<br><br>
$\rho_a \approx 50 \times [1 + 4 \times 0.818 \times 0.894] \approx 50 \times [1 + 2.93] \approx 50 \times 3.93 = 197$ Ω·m<br><br>
This is between $\rho_1$ and $\rho_2$, as expected, weighted toward the second layer at this large spacing.
</div>
</div>

<div class="exercise" id="ex23">
<strong>Exercise 23:</strong> Set up the Jacobian matrix structure for a simple 2D ERT problem with 4 model cells and 3 measurements. Explain what each element represents physically.
<button class="solution-toggle" onclick="toggleSolution('sol23')">Show Solution</button>
<div class="solution" id="sol23" style="display:none;">
<strong>Solution:</strong><br>
Jacobian matrix $\mathbf{J}$ has dimensions $N_{data} \times N_{model}$ = 3 × 4:<br><br>
$$\mathbf{J} = \begin{pmatrix} J_{11} & J_{12} & J_{13} & J_{14} \\ J_{21} & J_{22} & J_{23} & J_{24} \\ J_{31} & J_{32} & J_{33} & J_{34} \end{pmatrix}$$<br><br>
Each element $J_{ij} = \frac{\partial d_i}{\partial m_j} = \frac{\partial \log\rho_{a,i}}{\partial \log\rho_j}$<br><br>
<strong>Physical meaning:</strong><br>
$J_{ij}$ represents how much measurement $i$ changes when the resistivity of cell $j$ changes by a small amount. It is the sensitivity of measurement $i$ to model cell $j$.<br><br>
For log-parameterization, $J_{ij}$ represents the fractional change in apparent resistivity per fractional change in model resistivity.<br><br>
Cells near the electrodes have large sensitivities; deep or distant cells have small sensitivities. The Jacobian encodes the resolution capability of the survey.
</div>
</div>

<div class="exercise" id="ex24">
<strong>Exercise 24:</strong> A survey collects 500 measurements with average reciprocal error 3.5%. After inversion, RMS = 1.8 with regularization parameter λ = 10. Explain how you would adjust λ and why, referencing the bias-variance tradeoff.
<button class="solution-toggle" onclick="toggleSolution('sol24')">Show Solution</button>
<div class="solution" id="sol24" style="display:none;">
<strong>Solution:</strong><br>
With 3.5% reciprocal error, target RMS ≈ 1.0-1.5. Current RMS = 1.8 indicates underfitting.<br><br>
<strong>Bias-variance tradeoff:</strong><br>
- High λ → smooth model (high bias, low variance) → underfits data<br>
- Low λ → rough model (low bias, high variance) → overfits noise<br><br>
<strong>Adjustment strategy:</strong><br>
1. Decrease λ to allow more model structure<br>
2. Try λ = 5, then λ = 2, monitoring RMS and model roughness<br>
3. Target RMS ≈ 1.0-1.2<br>
4. Use L-curve analysis: plot $\|\mathbf{d}_{obs} - \mathbf{d}_{calc}\|$ vs $\|\nabla\mathbf{m}\|$ for various λ<br>
5. Choose λ at the L-curve corner<br><br>
<strong>Caution:</strong> If reducing λ creates geologically unreasonable features (extreme values, oscillations), the data may have unidentified errors or the model discretization may be inadequate.
</div>
</div>

<div class="exercise" id="ex25">
<strong>Exercise 25:</strong> Develop a sensitivity analysis for detecting a 2 m × 2 m void at 6 m depth. Calculate the expected anomaly magnitude using dipole-dipole with $a = 2$ m and compare $n = 2$ vs $n = 6$.
<button class="solution-toggle" onclick="toggleSolution('sol25')">Show Solution</button>
<div class="solution" id="sol25" style="display:none;">
<strong>Solution:</strong><br>
Void: effectively infinite resistivity anomaly in background $\rho_b = 100$ Ω·m.<br><br>
<strong>Sensitivity depth:</strong><br>
For $n = 2$: median depth ≈ $0.25 \times 3a = 1.5$ m (too shallow)<br>
For $n = 6$: median depth ≈ $0.25 \times 7a = 3.5$ m (closer but still shallow)<br><br>
<strong>Anomaly estimation (simplified):</strong><br>
The void produces a positive resistivity anomaly. Using a sphere approximation for a compact target:<br><br>
$$\frac{\Delta\rho_a}{\rho_b} \approx \left(\frac{r}{z}\right)^3 \times f(n)$$<br><br>
where $r$ is equivalent radius (~1 m), $z$ is depth (6 m).<br><br>
For $n = 2$ (depth sensitivity ~1.5 m): $(1/6)^3 \times$ sensitivity factor → very weak anomaly (~1-2%)<br><br>
For $n = 6$ (depth sensitivity ~3.5 m): Better coupling to target depth → moderate anomaly (~3-5%)<br><br>
<strong>Conclusion:</strong> Neither configuration optimally samples 6 m depth. Recommend larger $a$ (3-4 m) or Wenner array with $a = 15-20$ m for better depth coupling. Expected anomaly ~5-10% with appropriate configuration.
</div>
</div>

<div class="exercise" id="ex26">
<strong>Exercise 26:</strong> Design a time-lapse ERT monitoring system for permafrost degradation in the Tian Shan. Address electrode installation in frozen ground, seasonal measurement timing, and inversion approach for detecting active layer deepening.
<button class="solution-toggle" onclick="toggleSolution('sol26')">Show Solution</button>
<div class="solution" id="sol26" style="display:none;">
<strong>Solution:</strong><br>
<strong>Electrode installation:</strong><br>
1. Install during late summer when active layer is deepest<br>
2. Use stainless steel electrodes (1 m length) with thermal insulation at surface<br>
3. Drill pilot holes if necessary; backfill with bentonite<br>
4. Install in bedrock or stable ground below maximum active layer depth for permanence<br>
5. Use 2-3 m spacing for active layer resolution (typically 1-4 m thick)<br><br>
<strong>Measurement timing:</strong><br>
1. Monthly measurements during thaw season (May-September)<br>
2. Quarterly during frozen season<br>
3. Synchronized with temperature monitoring for calibration<br><br>
<strong>Inversion approach:</strong><br>
1. Use time-lapse inversion with reference model from frozen period<br>
2. Parameterize as percent change from baseline<br>
3. Apply temporal smoothness constraint between consecutive surveys<br>
4. Focus on frozen/unfrozen boundary (1000+ Ω·m to <200 Ω·m transition)<br><br>
<strong>Detection criteria:</strong><br>
Active layer deepening appears as downward migration of the resistivity transition zone. Track the depth where $\rho = 500$ Ω·m isosurface over time.
</div>
</div>

<div class="exercise" id="ex27">
<strong>Exercise 27:</strong> An ERT survey over an archaeological site shows a high-resistivity anomaly at 2 m depth. GPR data over the same feature shows no clear reflection. Explain possible causes for this discrepancy and how you would investigate further.
<button class="solution-toggle" onclick="toggleSolution('sol27')">Show Solution</button>
<div class="solution" id="sol27" style="display:none;">
<strong>Solution:</strong><br>
<strong>Possible causes for ERT anomaly without GPR reflection:</strong><br><br>
1. <strong>Gradual resistivity transition:</strong> ERT detects bulk resistivity changes; GPR requires sharp dielectric contrasts. A gradual interface produces ERT anomaly but weak/no GPR reflection.<br><br>
2. <strong>Conductive overburden:</strong> If surface soil is conductive ($\rho < 30$ Ω·m), GPR energy attenuates before reaching 2 m. ERT penetrates regardless of surface conductivity.<br><br>
3. <strong>Similar dielectric but different resistivity:</strong> Materials can have similar dielectric constants (no GPR contrast) but different resistivities (ERT contrast). Example: dry rubble vs. dry sand.<br><br>
4. <strong>Geometric effects:</strong> Small targets may be below GPR resolution but accumulate as distributed ERT anomaly.<br><br>
<strong>Investigation approach:</strong><br>
1. Check GPR depth of penetration (is signal reaching 2 m?)<br>
2. Measure surface soil resistivity—if < 30 Ω·m, GPR is attenuated<br>
3. Excavate test pit to verify feature<br>
4. Try lower-frequency GPR antenna for deeper penetration<br>
5. Use resistivity data to estimate dielectric contrast—if $< 2:1$, GPR reflection would be weak
</div>
</div>

<div class="exercise" id="ex28">
<strong>Exercise 28:</strong> Derive the relationship between ERT-measured resistivity and hydraulic conductivity for a sandy aquifer. Use the Kozeny-Carman equation and Archie's law to establish this link.
<button class="solution-toggle" onclick="toggleSolution('sol28')">Show Solution</button>
<div class="solution" id="sol28" style="display:none;">
<strong>Solution:</strong><br>
<strong>Archie's law (electrical):</strong><br>
$$\rho_{bulk} = \rho_w \cdot a\phi^{-m}$$<br>
where $\phi$ is porosity, $a \approx 1$, $m \approx 2$.<br><br>
<strong>Kozeny-Carman (hydraulic):</strong><br>
$$K = \frac{\rho_f g}{\mu} \cdot \frac{\phi^3}{(1-\phi)^2} \cdot \frac{d^2}{180}$$<br><br>
<strong>Connecting equations:</strong><br>
From Archie's law: $\phi = \left(\frac{\rho_w}{\rho_{bulk}}\right)^{1/m}$<br><br>
Substituting into Kozeny-Carman:<br>
$$K \propto \phi^3 \propto \left(\frac{\rho_w}{\rho_{bulk}}\right)^{3/m}$$<br><br>
For $m = 2$:<br>
$$K \propto \rho_{bulk}^{-1.5}$$<br><br>
<strong>Empirical relationship:</strong><br>
$$\log K = A - B\log\rho_{bulk}$$<br><br>
where $A$ and $B$ are site-specific constants determined by calibration with pumping tests. Typically $B \approx 1.3-1.8$ for sandy aquifers.<br><br>
<strong>Caution:</strong> This relationship assumes constant grain size and water chemistry. Clay content and salinity variations require additional corrections.
</div>
</div>

<div class="exercise" id="ex29">
<strong>Exercise 29:</strong> A 3D ERT survey uses a 10×10 electrode grid at 2 m spacing. Calculate (a) the minimum number of measurements for full 3D coverage using dipole-dipole configurations in both x and y directions, and (b) the expected survey time if each measurement takes 2 seconds.
<button class="solution-toggle" onclick="toggleSolution('sol29')">Show Solution</button>
<div class="solution" id="sol29" style="display:none;">
<strong>Solution:</strong><br>
<strong>(a) Number of measurements:</strong><br><br>
<strong>X-direction lines (10 lines of 10 electrodes):</strong><br>
Per line with dipole-dipole, max $n = 6$:<br>
Measurements ≈ $(10-3) \times 6 = 42$ per line<br>
Total x-direction: $10 \times 42 = 420$<br><br>
<strong>Y-direction lines (10 lines of 10 electrodes):</strong><br>
Same calculation: $10 \times 42 = 420$<br><br>
<strong>Cross-line measurements (diagonal dipoles):</strong><br>
For true 3D sensitivity, add equatorial dipole-dipole:<br>
Approximately 200-400 additional measurements<br><br>
<strong>Total: ~1000-1200 measurements minimum</strong><br><br>
<strong>(b) Survey time:</strong><br>
$1100 \times 2$ s = 2200 s = 37 minutes of measurement time<br><br>
Adding setup, electrode testing, and data checks:<br>
Realistic field time: 2-3 hours for the complete 3D survey<br><br>
<strong>Note:</strong> Modern multi-channel systems can measure multiple configurations simultaneously, potentially reducing this by a factor of 4-8.
</div>
</div>

<div class="exercise" id="ex30">
<strong>Exercise 30:</strong> You're planning an ERT survey across a suspected ancient qanat system in southern Kazakhstan. The qanats may be air-filled (very high resistivity) or water-filled (moderate resistivity) depending on season. Design a comprehensive survey strategy including array selection, line orientation, depth requirements, and seasonal considerations.
<button class="solution-toggle" onclick="toggleSolution('sol30')">Show Solution</button>
<div class="solution" id="sol30" style="display:none;">
<strong>Solution:</strong><br>
<strong>Target characteristics:</strong><br>
- Qanat tunnels: 1-2 m diameter, 5-20 m depth<br>
- Air-filled: $\rho > 1000$ Ω·m<br>
- Water-filled: $\rho = 20-50$ Ω·m<br>
- Background (loess/alluvium): $\rho = 50-150$ Ω·m<br><br>
<strong>Survey design:</strong><br><br>
<strong>1. Array selection:</strong> Dipole-dipole for horizontal resolution of linear features, supplemented by Wenner-Schlumberger for depth verification.<br><br>
<strong>2. Electrode spacing:</strong> 2 m primary (resolves 4 m features; qanat signature will be detectible though not resolved in detail). Add 1 m infill lines over anomalies.<br><br>
<strong>3. Line orientation:</strong> Perpendicular to suspected qanat alignment (typically these run downslope from mountains). Use topographic analysis and historical maps to estimate orientation. Run at least 3 orientations (0°, 45°, 90°) if orientation unknown.<br><br>
<strong>4. Depth requirements:</strong><br>
- Maximum depth 20 m → line length ~100 m minimum<br>
- Use 48+ electrodes at 2 m = 94 m line<br><br>
<strong>5. Seasonal strategy:</strong><br>
- Survey in both wet (spring) and dry (late summer) seasons<br>
- Water-filled qanats appear as low-resistivity anomalies in wet season<br>
- Same qanats appear as high-resistivity in dry season<br>
- Temporal change confirms qanat vs. geological feature<br><br>
<strong>6. Integration:</strong><br>
- Precede with EM reconnaissance to identify conductive zones efficiently<br>
- Follow up anomalies with targeted excavation or camera inspection<br><br>
<strong>Expected signatures:</strong><br>
- Active qanat (wet): Linear low-resistivity anomaly<br>
- Abandoned qanat (dry): Linear high-resistivity anomaly<br>
- Collapsed qanat: Irregular high-resistivity with low-resistivity halo (rubble + infiltration)
</div>
</div>

<script>
function toggleSolution(id) {
  var sol = document.getElementById(id);
  if (sol.style.display === "none") {
    sol.style.display = "block";
  } else {
    sol.style.display = "none";
  }
}
</script>

## Summary

Electrical Resistivity Tomography provides quantitative subsurface imaging essential for Central Asia's challenging geological and archaeological environments. Unlike GPR, it works in conductive and saline soils. Unlike EM methods, it provides true resistivity values with controlled depth resolution.

The key principles:

1. **Apparent resistivity** transforms voltage measurements into interpretable subsurface properties using the **geometric factor**
2. **Array selection** balances resolution, signal strength, and survey efficiency
3. **Inversion** transforms apparent resistivity pseudosections into true subsurface models through iterative optimization
4. **Integration** with other methods provides robust multi-parameter interpretation

Central Asia applications span groundwater exploration in arid zones, archaeological prospection along the Silk Road, permafrost monitoring in mountain regions, and irrigation system assessment—all benefiting from ERT's unique combination of depth penetration, quantitative output, and versatility in difficult terrain.

