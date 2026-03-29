---
layout: tutorial
title: "How Advanced GPR Processing Reveals Subsurface Detail in Central Asia"
description: "An advanced tutorial on GPR signal processing, migration, attribute analysis, and 3D visualization for archaeological and geological applications, with 30 exercises and solutions."
difficulty: advanced
estimated_time: "3-4 hours"
series: treasure-hunting
order: 12
---

## Why advanced processing matters

Raw GPR data rarely tells the full story. Subsurface reflections are contaminated by system artifacts, geometric distortions, and noise that obscure archaeological and geological features. Advanced processing transforms noisy radargrams into interpretable images where buried walls, voids, kurgans, and ancient irrigation channels become visible.

In Central Asia, where dry loess soils and arid conditions often provide excellent radar penetration, the difference between raw and processed data can mean the difference between missing a qanat entirely and mapping its complete subsurface geometry.

<div class="info-box tip">
  <strong>Processing philosophy</strong>
  Each processing step should have a clear physical justification. Blind application of filters can create artifacts that look like real features. Always compare processed data against raw data to verify that apparent features are genuine.
</div>

This tutorial builds on fundamental GPR concepts and introduces the mathematical and practical tools needed for professional-grade interpretation. We focus on techniques particularly relevant to Central Asian archaeology: kurgan detection, qanat mapping, and ancient irrigation system reconstruction.

## Signal processing fundamentals

### Dewow filtering

GPR systems often exhibit low-frequency "wow" caused by inductive coupling between transmitter and receiver or by system electronics. This manifests as a slowly varying DC offset that shifts trace baselines.

The dewow filter removes this artifact by subtracting a running mean or applying a high-pass filter. For a trace $A(t)$, the dewowed signal is

$$
A_{\text{dewow}}(t) = A(t) - \frac{1}{W} \int_{t-W/2}^{t+W/2} A(\tau) \, d\tau
$$

where $W$ is the filter window width. In discrete form:

$$
A_{\text{dewow}}[n] = A[n] - \frac{1}{2M+1} \sum_{k=n-M}^{n+M} A[k]
$$

The window length should be longer than the dominant period of the low-frequency artifact but shorter than the features of interest. Typically, $W$ corresponds to 2-5 times the dominant signal period.

<div class="info-box tip">
  <strong>Choosing dewow window</strong>
  For a 400 MHz antenna with center period around 2.5 ns, a dewow window of 5-10 ns effectively removes wow while preserving shallow reflections.
</div>

### Time-zero correction

The first arrival in a GPR trace does not always correspond to the ground surface. System delays, cable lengths, and antenna geometry create offsets that must be corrected for accurate depth conversion.

Time-zero correction aligns all traces so that $t = 0$ corresponds to the air-ground interface. The corrected time is

$$
t_{\text{corrected}} = t_{\text{raw}} - t_0
$$

where $t_0$ is the static time shift, determined by:

1. **Direct wave method**: Identifying the first break of the direct ground wave
2. **Surface reflection method**: Using a metal plate to create a known reference
3. **Antenna calibration**: Using manufacturer-provided offsets

For Central Asian surveys, where ground surfaces may be irregular, trace-by-trace time-zero picking may be necessary. The first positive or negative peak of the ground wave often serves as a consistent reference.

### Gain functions

Radar signals attenuate with depth due to geometric spreading and material absorption. Gain functions compensate for this attenuation to make deep reflections visible.

#### Spherical and exponential compensation (SEC)

The SEC gain combines geometric spreading correction with exponential attenuation compensation:

$$
G(t) = \left( 1 + \alpha \cdot v \cdot t \right) \cdot e^{\beta t}
$$

where $\alpha$ controls the linear (spreading) component and $\beta$ controls exponential attenuation. For a signal $A(t)$, the gained signal is

$$
A_{\text{gained}}(t) = A(t) \cdot G(t)
$$

#### Automatic gain control (AGC)

AGC normalizes signal amplitude using a sliding window:

$$
A_{\text{AGC}}(t) = \frac{A(t)}{\text{RMS}_{W}(t)}
$$

where

$$
\text{RMS}_{W}(t) = \sqrt{\frac{1}{W} \int_{t-W/2}^{t+W/2} A^2(\tau) \, d\tau}
$$

AGC equalizes amplitudes throughout the trace, making weak deep reflections visible. However, it destroys true amplitude information and can create artifacts at boundaries between high and low amplitude zones.

<div class="info-box tip">
  <strong>Gain selection</strong>
  Use SEC for quantitative amplitude analysis. Use AGC for visual interpretation when relative amplitudes are less important than identifying reflection geometry.
</div>

#### Energy decay compensation

In attenuating media, signal energy decays exponentially:

$$
E(t) = E_0 \cdot e^{-2\alpha_{\text{att}} \cdot d(t)}
$$

where $\alpha_{\text{att}}$ is the attenuation coefficient (Np/m) and $d(t)$ is the depth corresponding to time $t$. The attenuation coefficient relates to conductivity by

$$
\alpha_{\text{att}} = \frac{\sigma}{2} \sqrt{\frac{\mu}{\varepsilon}}
$$

For dry Central Asian loess with conductivity $\sigma \approx 1-5$ mS/m, attenuation is typically 0.1-0.5 dB/m at 400 MHz.

## Filtering techniques

### Bandpass filtering

GPR data contains useful signal within a frequency band centered on the antenna frequency. Noise outside this band can be removed with a bandpass filter.

For an antenna with center frequency $f_c$, the passband typically spans

$$
f_{\text{low}} = 0.25 \cdot f_c \quad \text{to} \quad f_{\text{high}} = 2.5 \cdot f_c
$$

The Butterworth bandpass filter has transfer function

$$
|H(f)|^2 = \frac{1}{1 + \left( \frac{f_{\text{low}}}{f} \right)^{2n}} \cdot \frac{1}{1 + \left( \frac{f}{f_{\text{high}}} \right)^{2n}}
$$

where $n$ is the filter order. Higher orders provide sharper cutoffs but may introduce ringing artifacts.

<div class="info-box tip">
  <strong>Filter order selection</strong>
  For archaeological GPR, a 2nd or 4th order Butterworth filter typically provides good noise rejection without excessive ringing. Zero-phase filtering (forward-backward application) prevents phase distortion.
</div>

### Background removal

Horizontal banding from system ringing, antenna coupling, or planar interfaces can mask point targets and dipping reflectors. Background removal subtracts the average trace from each individual trace:

$$
A_{\text{BGremoved}}(x, t) = A(x, t) - \bar{A}(t)
$$

where

$$
\bar{A}(t) = \frac{1}{N} \sum_{i=1}^{N} A(x_i, t)
$$

This removes flat-lying reflections and horizontal noise but preserves hyperbolic diffractions and dipping events.

For spatially varying backgrounds, a moving-window average can be used:

$$
\bar{A}(x, t) = \frac{1}{2L+1} \sum_{k=x-L}^{x+L} A(k, t)
$$

where $L$ is the window half-width in traces.

### Frequency-wavenumber (F-K) filtering

F-K filtering operates in the two-dimensional Fourier domain to separate signals based on their apparent velocity. The 2D Fourier transform of a radargram $A(x, t)$ is

$$
\tilde{A}(k_x, f) = \iint A(x, t) \cdot e^{-i(k_x x + 2\pi f t)} \, dx \, dt
$$

where $k_x$ is horizontal wavenumber and $f$ is temporal frequency.

Events with different apparent velocities map to different regions in F-K space. The apparent velocity relates wavenumber to frequency:

$$
v_{\text{app}} = \frac{2\pi f}{k_x}
$$

Dipping reflectors, diffractions, and noise can be isolated and removed by designing appropriate F-K filter masks. A fan filter passing only events with apparent velocities between $v_1$ and $v_2$ has the mask

$$
M(k_x, f) = \begin{cases} 1 & \text{if } v_1 < \frac{2\pi f}{k_x} < v_2 \\ 0 & \text{otherwise} \end{cases}
$$

<div class="info-box tip">
  <strong>F-K filtering for kurgan surveys</strong>
  When surveying burial mounds, F-K filtering can separate shallow near-horizontal stratigraphy from deeper diffraction patterns generated by burial chambers or stone structures.
</div>

## Migration techniques

Migration is the most important processing step for accurate subsurface imaging. It collapses diffraction hyperbolas to their point sources and repositions dipping reflectors to their true subsurface locations.

### The migration problem

A point scatterer at depth $z$ and horizontal position $x_0$ generates a hyperbolic diffraction pattern:

$$
t(x) = \frac{2}{v} \sqrt{z^2 + (x - x_0)^2}
$$

Migration inverts this relationship, transforming the hyperbola back to a focused point at $(x_0, z)$.

For a dipping reflector with true dip $\theta$, the unmigrated apparent dip $\phi$ satisfies

$$
\sin \phi = \sin \theta \cdot \cos \theta = \frac{\sin 2\theta}{2}
$$

Migration corrects this geometric distortion, revealing true reflector geometry.

### Kirchhoff migration

Kirchhoff migration sums trace amplitudes along diffraction curves and places the result at the apex:

$$
I(x_0, z) = \sum_{x} A\left( x, t = \frac{2}{v}\sqrt{z^2 + (x-x_0)^2} \right) \cdot w(x, x_0, z)
$$

where $w(x, x_0, z)$ is a weighting function that accounts for geometric spreading and obliquity:

$$
w(x, x_0, z) = \frac{z}{\left[ z^2 + (x-x_0)^2 \right]^{3/4}} \cdot \cos \theta
$$

where $\theta$ is the angle from vertical.

Kirchhoff migration is flexible, handles irregular geometries, and allows spatially varying velocities. For archaeological GPR in Central Asia, where velocity may change laterally (e.g., from intact loess to disturbed fill), this flexibility is valuable.

### Stolt F-K migration

For constant velocity media, Stolt migration provides efficient migration in the frequency-wavenumber domain. The algorithm maps the data from $(k_x, f)$ to $(k_x, k_z)$ using the dispersion relation:

$$
k_z = \sqrt{\left( \frac{2\pi f}{v} \right)^2 - k_x^2}
$$

The migrated image is obtained by inverse Fourier transform:

$$
I(x, z) = \iint \tilde{A}(k_x, k_z) \cdot e^{i(k_x x + k_z z)} \, dk_x \, dk_z
$$

Stolt migration is computationally efficient (FFT-based) but assumes constant velocity, which may not hold in heterogeneous archaeological sites.

### Phase-shift migration

Phase-shift migration propagates the wavefield downward through depth slices:

$$
\tilde{A}(k_x, z + \Delta z, f) = \tilde{A}(k_x, z, f) \cdot e^{i k_z \Delta z}
$$

where

$$
k_z = \sqrt{\left( \frac{2\pi f}{v(z)} \right)^2 - k_x^2}
$$

The imaging condition extracts the zero-time component at each depth:

$$
I(x, z) = \int \tilde{A}(k_x, z, t=0) \cdot e^{i k_x x} \, dk_x
$$

Phase-shift migration naturally handles vertical velocity variations, making it suitable for layered sites where velocity increases with depth due to compaction.

<div class="info-box tip">
  <strong>Migration velocity</strong>
  Migration velocity errors cause defocusing. Undermigration (velocity too low) leaves residual hyperbolas; overmigration (velocity too high) creates "smiles" that curve upward.
</div>

## Velocity analysis and depth conversion

Accurate depth conversion requires reliable velocity estimates. Several methods are available for Central Asian survey conditions.

### Hyperbola fitting

The most common method fits hyperbolas to point diffractions. For a diffraction with apex at $(x_0, t_0)$, the travel time curve is

$$
t(x) = t_0 \sqrt{1 + \frac{(x - x_0)^2}{(v \cdot t_0 / 2)^2}}
$$

Rearranging:

$$
t^2 = t_0^2 + \frac{4(x - x_0)^2}{v^2}
$$

Plotting $t^2$ versus $(x - x_0)^2$ yields a line with slope $4/v^2$.

For field estimation, measuring the hyperbola width $\Delta x$ at a time $\Delta t$ below the apex gives

$$
v = \frac{2 \Delta x}{\sqrt{(\Delta t + t_0)^2 - t_0^2}}
$$

### Common midpoint (CMP) analysis

CMP surveys with variable antenna separation provide velocity-depth profiles. For a horizontal reflector at depth $z$, the travel time with offset $x$ is

$$
t(x) = \frac{2}{v} \sqrt{z^2 + \frac{x^2}{4}}
$$

The normal moveout (NMO) is

$$
\Delta t_{\text{NMO}} = t(x) - t(0) \approx \frac{x^2}{2 v^2 t_0}
$$

for small offsets. Velocity is estimated by finding the value that flattens reflections in CMP gathers:

$$
v_{\text{NMO}} = \frac{x}{\sqrt{t(x)^2 - t(0)^2}}
$$

### Dielectric estimation from soil properties

When field measurements are unavailable, velocity can be estimated from soil properties using mixing models. For a soil with volumetric water content $\theta_w$, the complex refractive index method gives

$$
\sqrt{\varepsilon_r} \approx (1 - \phi)\sqrt{\varepsilon_s} + \phi(1 - S_w)\sqrt{\varepsilon_a} + \phi S_w \sqrt{\varepsilon_w}
$$

where $\phi$ is porosity, $S_w$ is water saturation, and $\varepsilon_s$, $\varepsilon_a$, $\varepsilon_w$ are dielectric constants of solid, air, and water.

For dry Central Asian loess ($\theta_w < 5\%$), typical values are $\varepsilon_r \approx 4-6$, giving velocities of $0.12-0.15$ m/ns.

### Depth conversion

Once velocity is determined, depth conversion follows from

$$
z(t) = \frac{1}{2} \int_0^t v(\tau) \, d\tau
$$

For constant velocity: $z = vt/2$

For linear velocity increase $v(z) = v_0 + kz$:

$$
z(t) = \frac{v_0}{k} \left( e^{kt/2} - 1 \right)
$$

<div class="info-box tip">
  <strong>Central Asian velocities</strong>
  Typical GPR velocities in Central Asian contexts: Dry loess 0.12-0.15 m/ns; Moist agricultural soil 0.06-0.08 m/ns; Sandy desert 0.13-0.16 m/ns; Clay-rich alluvium 0.05-0.07 m/ns.
</div>

## Attribute analysis

Beyond reflection amplitude, GPR traces contain information encoded in phase, frequency, and other attributes that can reveal subsurface properties.

### Instantaneous amplitude (envelope)

The instantaneous amplitude or envelope is the magnitude of the analytic signal:

$$
E(t) = |A(t) + i \mathcal{H}[A(t)]| = \sqrt{A^2(t) + \mathcal{H}^2[A(t)]}
$$

where $\mathcal{H}$ denotes the Hilbert transform:

$$
\mathcal{H}[A(t)] = \frac{1}{\pi} \text{P.V.} \int_{-\infty}^{\infty} \frac{A(\tau)}{t - \tau} \, d\tau
$$

Instantaneous amplitude highlights strong reflectors regardless of polarity and is useful for mapping reflection strength variations that may indicate material property changes.

### Instantaneous phase

The instantaneous phase tracks wavelet polarity independent of amplitude:

$$
\phi(t) = \arctan \frac{\mathcal{H}[A(t)]}{A(t)}
$$

Phase sections enhance weak reflections and continuous interfaces. In Central Asian irrigation systems, phase displays can reveal subtle canal boundaries that are difficult to see in amplitude data.

### Instantaneous frequency

The instantaneous frequency is the time derivative of phase:

$$
f_{\text{inst}}(t) = \frac{1}{2\pi} \frac{d\phi}{dt}
$$

Frequency decreases with attenuation, so low instantaneous frequency can indicate high-loss materials (clay, moist soil). Conversely, high frequencies suggest low-loss materials (dry sand, air voids).

For a propagating wave with attenuation:

$$
f_{\text{observed}} = f_0 \cdot e^{-\alpha_f \cdot z}
$$

where $\alpha_f$ is the frequency-dependent attenuation coefficient.

### Reflection strength and polarity

Reflection polarity indicates the sign of the dielectric contrast:

$$
R = \frac{\sqrt{\varepsilon_2} - \sqrt{\varepsilon_1}}{\sqrt{\varepsilon_2} + \sqrt{\varepsilon_1}}
$$

Positive reflection (same polarity as transmitted pulse): $\varepsilon_2 > \varepsilon_1$ (e.g., dry soil over moist)

Negative reflection (inverted polarity): $\varepsilon_2 < \varepsilon_1$ (e.g., soil over air void)

This polarity information is crucial for identifying voids in kurgans or air-filled qanat tunnels.

## 3D GPR data acquisition and visualization

### Data acquisition geometry

3D GPR surveys collect multiple parallel profiles with controlled spacing. The Nyquist criterion requires

$$
\Delta y \leq \frac{\lambda}{4} = \frac{v}{4 f_c}
$$

For a 400 MHz antenna in dry soil ($v = 0.12$ m/ns), this gives $\Delta y \leq 0.075$ m. Practical surveys often use 0.25-0.50 m spacing, accepting some spatial aliasing.

The 3D data volume $A(x, y, t)$ contains information about subsurface geometry in all three dimensions.

### Spatial interpolation

Irregularly spaced profiles require interpolation to create regular grids. Common methods include:

**Inverse distance weighting:**

$$
A(x_0, y_0, t) = \frac{\sum_i w_i \cdot A(x_i, y_i, t)}{\sum_i w_i}
$$

where $w_i = 1/d_i^p$ and $d_i$ is the distance to the $i$-th data point.

**Kriging:** Provides optimal interpolation based on spatial autocorrelation:

$$
\hat{A}(x_0, y_0) = \sum_i \lambda_i \cdot A(x_i, y_i)
$$

where weights $\lambda_i$ are determined by solving the kriging system based on the semivariogram.

### Depth slices and time slices

Time slices extract horizontal sections at constant two-way travel time:

$$
S(x, y) = A(x, y, t = t_{\text{slice}})
$$

After depth conversion, these become true depth slices showing plan-view maps at specific depths. Time slice movies (animated sequences through depth) are powerful visualization tools for identifying buried structures.

### Isosurface rendering

3D visualization can render surfaces of constant amplitude:

$$
\{ (x, y, z) : E(x, y, z) = E_{\text{threshold}} \}
$$

where $E$ is the envelope amplitude. This creates 3D models of strong reflectors, useful for visualizing kurgan chambers or qanat tunnel networks.

<div class="info-box tip">
  <strong>3D survey planning</strong>
  For burial mounds and other finite targets, a radial survey pattern centered on the feature can provide efficient coverage with lines converging at the target center.
</div>

## Hyperbola fitting and point target analysis

### Automated hyperbola detection

Hyperbolas can be detected automatically using the Hough transform or matched filtering. The Hough transform parameterizes hyperbolas by apex position $(x_0, t_0)$ and velocity $v$:

$$
\mathcal{H}(x_0, t_0, v) = \sum_x A\left( x, \frac{2}{v}\sqrt{z_0^2 + (x-x_0)^2} \right)
$$

where $z_0 = vt_0/2$. Peaks in Hough space correspond to hyperbola parameters.

### Target depth and size estimation

For a point target generating a hyperbola, the depth is

$$
z = \frac{v \cdot t_{\text{apex}}}{2}
$$

The apparent size of the target relates to the Fresnel zone radius at that depth:

$$
r_F = \sqrt{\frac{\lambda z}{2}} = \sqrt{\frac{v z}{2 f_c}}
$$

Targets smaller than $r_F$ appear as point diffractors. Larger targets produce reflection segments rather than hyperbolas.

### Multiple target resolution

Two point targets at the same depth but separated horizontally by $\Delta x$ can be resolved if

$$
\Delta x > r_F
$$

Two targets at the same horizontal position but different depths can be resolved if

$$
\Delta z > \frac{\lambda}{4} = \frac{v}{4 f_c}
$$

For a 400 MHz antenna in dry loess ($v = 0.12$ m/ns), vertical resolution is approximately 0.075 m.

## Stratigraphic interpretation

### Reflection configuration analysis

GPR reflections can be classified by their configuration:

- **Parallel**: Uniform deposition, undisturbed stratigraphy
- **Divergent**: Variable deposition rates, tilting during deposition
- **Chaotic**: Disturbed material, mass movement, bioturbation
- **Sigmoidal**: Prograding deposits, channel fill
- **Hyperbolic**: Point sources, voids, pipes, boulders

### Reflection terminations

Termination patterns indicate geological relationships:

- **Onlap**: Younger beds lap onto older surface (burial mound construction)
- **Downlap**: Beds dip into underlying surface (channel fill)
- **Toplap**: Beds terminate at upper surface (erosion)
- **Truncation**: Reflections cut by later surface (excavation, disturbance)

### Facies analysis

GPR facies combine reflection amplitude, continuity, and configuration to characterize depositional environments. In Central Asian contexts:

- **Loess facies**: Parallel, continuous, moderate amplitude
- **Alluvial channel facies**: Inclined, discontinuous, variable amplitude
- **Kurgan fill facies**: Chaotic to subparallel, disturbed continuity
- **Qanat tunnel facies**: Strong hyperbolic diffraction, void signature

<div class="info-box tip">
  <strong>Disturbance detection</strong>
  Archaeological excavations, looting, and burial chamber collapse create distinctive disruption patterns in otherwise continuous stratigraphy. Look for truncation of natural layering and chaotic fill signatures.
</div>

## Central Asia case studies

### Kurgan burial mounds

Kurgans present distinctive GPR signatures depending on construction and preservation:

**Construction layers**: Alternating soil and organic layers in mound construction create subparallel reflections dipping away from the center:

$$
\theta_{\text{apparent}}(r) = \arctan \frac{h_{\text{mound}}}{r_{\text{base}}}
$$

**Chamber voids**: Air-filled chambers produce strong negative-polarity reflections (dielectric decrease) and characteristic diffraction patterns:

$$
R_{\text{void}} = \frac{\sqrt{1} - \sqrt{5}}{\sqrt{1} + \sqrt{5}} \approx -0.38
$$

**Dromos passages**: Entry passages appear as linear diffraction sequences trending toward the chamber.

**Ring ditches**: Encircling ditches filled with different material create concentric amplitude anomalies in time slices.

### Qanat irrigation systems

Qanats (underground aqueducts) present linear subsurface features with characteristic signatures:

**Tunnel detection**: Main tunnels produce strong diffractions from the void-soil interface. The tunnel diameter $D$ can be estimated from diffraction width:

$$
D \approx 2 \sqrt{W \cdot z - z^2}
$$

where $W$ is the measured diffraction half-width at the apex depth $z$.

**Vertical shaft detection**: Access shafts appear as vertical diffraction sequences with regular spacing (typically 20-50 m).

**Water detection**: Active or recently active qanats may show attenuated returns below the water table due to increased conductivity.

### Ancient irrigation canals

Surface irrigation systems leave subsurface traces even when completely filled:

**Canal cross-sections**: U-shaped or V-shaped reflection patterns indicate former channel geometry. Channel depth can be measured from the basal reflection:

$$
d_{\text{channel}} = z_{\text{base}} - z_{\text{shoulder}}
$$

**Fill stratigraphy**: Multiple filling episodes create internal layering within channel fills.

**Levee structures**: Raised canal banks may show internal stratification from construction layers.

### Caravanserai and urban sites

Built environments produce complex GPR signatures:

**Wall foundations**: Linear high-amplitude reflections from mudbrick or stone foundations. Wall thickness relates to reflection duration:

$$
T_{\text{wall}} \approx \frac{2w}{v_{\text{wall}}}
$$

**Floor surfaces**: Horizontal high-amplitude reflections from compacted or plastered floors.

**Room voids**: Collapsed rooms may preserve air pockets generating void signatures.

## Processing workflow checklist

Follow this systematic workflow for processing Central Asian GPR data:

<ul class="checklist">
  <li>Import raw data and verify file integrity</li>
  <li>Apply time-zero correction using direct wave or known reference</li>
  <li>Remove DC bias with dewow filter (window 2-5× dominant period)</li>
  <li>Apply bandpass filter (0.25-2.5× center frequency)</li>
  <li>Perform background removal if horizontal banding present</li>
  <li>Estimate velocity from hyperbola fitting or CMP analysis</li>
  <li>Apply appropriate gain function (SEC for quantitative, AGC for visualization)</li>
  <li>Perform migration using estimated velocity model</li>
  <li>Convert to depth using velocity function</li>
  <li>Calculate attribute volumes (envelope, phase, frequency)</li>
  <li>Generate time/depth slices for plan-view interpretation</li>
  <li>Compare processed results against raw data to verify features</li>
  <li>Integrate with other survey methods (magnetometry, resistivity)</li>
  <li>Document processing parameters for reproducibility</li>
  <li>Export georeferenced results for GIS integration</li>
</ul>

## Exercises

### Signal processing exercises

**Exercise 1**: A GPR trace shows a DC offset that varies from +50 mV at the start to -30 mV at 100 ns. Design a dewow filter using a 20 ns window. If the original trace value at t=50 ns is 120 mV, what is the dewowed value assuming linear offset variation?

<details>
<summary>Show solution</summary>
The DC offset varies linearly from +50 mV at t=0 to -30 mV at t=100 ns. The offset function is:

$$\text{offset}(t) = 50 - 0.8t$$

At t=50 ns, the offset is $50 - 0.8(50) = 10$ mV.

For a 20 ns window centered at t=50 ns (from 40 to 60 ns), the average offset is:
$$\bar{\text{offset}} = \frac{1}{20}\int_{40}^{60} (50 - 0.8t) \, dt = 50 - 0.8(50) = 10 \text{ mV}$$

Dewowed value: $120 - 10 = 110$ mV.
</details>

**Exercise 2**: A 400 MHz antenna produces a pulse with dominant period 2.5 ns. The wow artifact has a period of approximately 25 ns. What dewow filter window length would you recommend, and why?

<details>
<summary>Show solution</summary>
The dewow window should be longer than the wow period (25 ns) but not so long that it removes legitimate signal variations. A window of 30-50 ns would capture the full wow cycle.

However, we must also consider that the window should be short enough to preserve shallow reflection patterns. A window of 30-40 ns (12-16× the signal dominant period) provides good wow removal while preserving reflection information.

Recommended: 35 ns window, representing about 1.4× the wow period and 14× the signal period.
</details>

**Exercise 3**: Time-zero analysis shows the ground surface reflection arrives 5.2 ns earlier than the system-recorded zero time. If uncorrected, what depth error would occur for a feature at 50 ns two-way time in soil with v = 0.10 m/ns?

<details>
<summary>Show solution</summary>
Without correction, the apparent depth is:
$$z_{\text{apparent}} = \frac{v \cdot t}{2} = \frac{0.10 \times 50}{2} = 2.50 \text{ m}$$

With proper time-zero correction (subtracting 5.2 ns from the apparent arrival):
$$z_{\text{true}} = \frac{0.10 \times (50 - 5.2)}{2} = \frac{0.10 \times 44.8}{2} = 2.24 \text{ m}$$

Depth error: $2.50 - 2.24 = 0.26$ m (10.4% overestimate).
</details>

**Exercise 4**: An SEC gain function uses $\alpha = 2$ and $\beta = 0.03$ ns⁻¹. For soil velocity 0.12 m/ns, calculate the gain applied at t = 40 ns. If the original amplitude is 15 μV, what is the gained amplitude?

<details>
<summary>Show solution</summary>
The SEC gain function is:
$$G(t) = (1 + \alpha \cdot v \cdot t) \cdot e^{\beta t}$$

Substituting values:
$$G(40) = (1 + 2 \times 0.12 \times 40) \cdot e^{0.03 \times 40}$$
$$G(40) = (1 + 9.6) \cdot e^{1.2} = 10.6 \times 3.32 = 35.2$$

Gained amplitude: $15 \times 35.2 = 528$ μV.
</details>

**Exercise 5**: An AGC filter with 30 ns window is applied to a trace. Within the window centered at t=60 ns, the trace values are {-20, 15, 30, 25, -10, -25, 20} mV (sampled at 5 ns intervals). Calculate the RMS amplitude and the AGC-normalized value for the center sample (30 mV).

<details>
<summary>Show solution</summary>
RMS amplitude:
$$\text{RMS} = \sqrt{\frac{(-20)^2 + 15^2 + 30^2 + 25^2 + (-10)^2 + (-25)^2 + 20^2}{7}}$$
$$= \sqrt{\frac{400 + 225 + 900 + 625 + 100 + 625 + 400}{7}} = \sqrt{\frac{3275}{7}} = \sqrt{467.9} = 21.6 \text{ mV}$$

AGC-normalized value for center sample:
$$A_{\text{AGC}} = \frac{30}{21.6} = 1.39$$ (dimensionless, normalized units)
</details>

### Filtering exercises

**Exercise 6**: Design a Butterworth bandpass filter for a 250 MHz antenna. Specify the corner frequencies and explain your reasoning.

<details>
<summary>Show solution</summary>
For a 250 MHz center frequency antenna:

Low-cut frequency: $f_{\text{low}} = 0.25 \times 250 = 62.5$ MHz
High-cut frequency: $f_{\text{high}} = 2.5 \times 250 = 625$ MHz

This range captures the full signal bandwidth while rejecting:
- Low-frequency noise (60 Hz interference, wow residuals)
- High-frequency noise (electronic noise, aliasing artifacts)

A 4th-order Butterworth provides -24 dB/octave rolloff, giving good noise rejection without excessive ringing. Apply as zero-phase filter to preserve reflection timing.
</details>

**Exercise 7**: A radargram has 200 traces. Background removal subtracts the mean trace. If trace #100 has amplitude 45 mV at t=30 ns, and the mean of all traces at t=30 ns is 38 mV, what is the background-removed amplitude?

<details>
<summary>Show solution</summary>
Background-removed amplitude:
$$A_{\text{BGremoved}} = A_{\text{original}} - \bar{A} = 45 - 38 = 7 \text{ mV}$$

This removal suppresses the 38 mV horizontal event (which appears on all traces) and preserves the 7 mV anomaly specific to this location.
</details>

**Exercise 8**: In F-K space, a reflection event appears at f = 300 MHz and k_x = 5 rad/m. Calculate its apparent velocity. Is this a realistic subsurface reflection or likely noise?

<details>
<summary>Show solution</summary>
Apparent velocity:
$$v_{\text{app}} = \frac{2\pi f}{k_x} = \frac{2\pi \times 300 \times 10^6}{5} = 3.77 \times 10^8 \text{ m/s}$$

This exceeds the speed of light (3 × 10⁸ m/s), which is physically impossible for a subsurface reflection. This event is likely:
- Direct air wave (travels at c)
- System noise/ringing
- Aliased energy

A valid subsurface reflection in soil (v ≈ 0.1 m/ns) would have $v_{\text{app}} < 0.1 \times 10^9 = 10^8$ m/s.
</details>

### Migration exercises

**Exercise 9**: A point diffractor at depth 1.5 m generates a hyperbola in soil with velocity 0.11 m/ns. Calculate the two-way travel time at the apex and at a horizontal offset of 2 m.

<details>
<summary>Show solution</summary>
At the apex (x = 0):
$$t_0 = \frac{2z}{v} = \frac{2 \times 1.5}{0.11} = 27.3 \text{ ns}$$

At offset x = 2 m:
$$t(2) = \frac{2}{v}\sqrt{z^2 + x^2} = \frac{2}{0.11}\sqrt{1.5^2 + 2^2} = \frac{2}{0.11}\sqrt{6.25} = \frac{2 \times 2.5}{0.11} = 45.5 \text{ ns}$$
</details>

**Exercise 10**: Migration with velocity 0.10 m/ns collapses a hyperbola to a point at depth 2.0 m. If the true velocity is 0.12 m/ns, where would the migrated point actually appear, and what artifact might you observe?

<details>
<summary>Show solution</summary>
True apex time: $t_0 = \frac{2 \times 2.0}{0.12} = 33.3$ ns

Migration with wrong velocity (0.10 m/ns):
- The migration algorithm assumes depth $z = \frac{0.10 \times 33.3}{2} = 1.67$ m
- The hyperbola curvature expected for v=0.10 at this depth doesn't match the actual data

Since we're using too low a velocity (undermigration), the hyperbola won't fully collapse. Residual unfocused energy will remain, appearing as a partial hyperbola or "frown" below the apparent focus point.

The point will appear at z ≈ 1.67 m (too shallow by 0.33 m) with residual diffraction tails.
</details>

**Exercise 11**: For Stolt migration, derive the mapping from frequency f = 400 MHz and horizontal wavenumber k_x = 8 rad/m to vertical wavenumber k_z, assuming velocity v = 0.10 m/ns.

<details>
<summary>Show solution</summary>
The dispersion relation for Stolt migration:
$$k_z = \sqrt{\left(\frac{2\pi f}{v}\right)^2 - k_x^2}$$

Substituting values:
$$\frac{2\pi f}{v} = \frac{2\pi \times 400 \times 10^6}{0.10 \times 10^9} = \frac{2\pi \times 400}{100} = 25.1 \text{ rad/m}$$

$$k_z = \sqrt{25.1^2 - 8^2} = \sqrt{630.5 - 64} = \sqrt{566.5} = 23.8 \text{ rad/m}$$
</details>

**Exercise 12**: A dipping reflector has true dip θ = 30°. Calculate the apparent (unmigrated) dip φ using the dip relationship formula.

<details>
<summary>Show solution</summary>
The relationship between true dip θ and apparent dip φ:
$$\sin \phi = \frac{\sin 2\theta}{2}$$

For θ = 30°:
$$\sin \phi = \frac{\sin 60°}{2} = \frac{0.866}{2} = 0.433$$
$$\phi = \arcsin(0.433) = 25.7°$$

The unmigrated reflector appears with a gentler dip (25.7°) than its true dip (30°). Migration steepens dipping reflectors to their correct geometry.
</details>

### Velocity analysis exercises

**Exercise 13**: A hyperbola has apex at t₀ = 35 ns. At horizontal offset x = 1.5 m, the travel time is t = 48 ns. Calculate the subsurface velocity and the target depth.

<details>
<summary>Show solution</summary>
Using the hyperbola equation in squared form:
$$t^2 = t_0^2 + \frac{4x^2}{v^2}$$

$$48^2 = 35^2 + \frac{4(1.5)^2}{v^2}$$
$$2304 = 1225 + \frac{9}{v^2}$$
$$\frac{9}{v^2} = 1079$$
$$v^2 = \frac{9}{1079} = 0.00834$$
$$v = 0.091 \text{ m/ns}$$

Depth: $z = \frac{v \cdot t_0}{2} = \frac{0.091 \times 35}{2} = 1.59$ m
</details>

**Exercise 14**: A CMP gather shows a reflection at t(0) = 40 ns for zero offset. With antenna separation x = 2.0 m, the same reflection arrives at t(2.0) = 44 ns. Calculate the NMO velocity.

<details>
<summary>Show solution</summary>
NMO velocity formula:
$$v_{\text{NMO}} = \frac{x}{\sqrt{t(x)^2 - t(0)^2}}$$

$$v_{\text{NMO}} = \frac{2.0}{\sqrt{44^2 - 40^2}} = \frac{2.0}{\sqrt{1936 - 1600}} = \frac{2.0}{\sqrt{336}} = \frac{2.0}{18.3} = 0.109 \text{ m/ns}$$
</details>

**Exercise 15**: Dry loess has porosity φ = 0.45, water saturation Sw = 0.08, and mineral dielectric constant εs = 5.5. Using the CRIM formula, estimate the bulk dielectric constant and GPR velocity.

<details>
<summary>Show solution</summary>
Complex Refractive Index Method (CRIM):
$$\sqrt{\varepsilon_r} = (1-\phi)\sqrt{\varepsilon_s} + \phi(1-S_w)\sqrt{\varepsilon_a} + \phi S_w\sqrt{\varepsilon_w}$$

With εs = 5.5, εa = 1, εw = 81:
$$\sqrt{\varepsilon_r} = (1-0.45)\sqrt{5.5} + 0.45(1-0.08)\sqrt{1} + 0.45(0.08)\sqrt{81}$$
$$= 0.55 \times 2.35 + 0.414 \times 1 + 0.036 \times 9$$
$$= 1.29 + 0.414 + 0.324 = 2.03$$

$$\varepsilon_r = 2.03^2 = 4.12$$

Velocity: $v = \frac{c}{\sqrt{\varepsilon_r}} = \frac{0.30}{2.03} = 0.148$ m/ns
</details>

### Attribute analysis exercises

**Exercise 16**: A GPR trace has amplitude A(t) = 50 sin(2πft) mV with f = 400 MHz. Calculate the Hilbert transform and instantaneous amplitude at t = 1 ns.

<details>
<summary>Show solution</summary>
For a sinusoidal signal A(t) = A₀ sin(2πft), the Hilbert transform is:
$$\mathcal{H}[A(t)] = -A_0 \cos(2\pi ft)$$

At t = 1 ns with f = 400 MHz:
$$A(1) = 50 \sin(2\pi \times 0.4 \times 1) = 50 \sin(2.51) = 50 \times 0.588 = 29.4 \text{ mV}$$
$$\mathcal{H}[A(1)] = -50 \cos(2.51) = -50 \times (-0.809) = 40.4 \text{ mV}$$

Instantaneous amplitude (envelope):
$$E(t) = \sqrt{A^2 + \mathcal{H}^2} = \sqrt{29.4^2 + 40.4^2} = \sqrt{864 + 1632} = \sqrt{2496} = 50 \text{ mV}$$

The envelope equals the original amplitude A₀ = 50 mV, as expected for a pure sinusoid.
</details>

**Exercise 17**: A reflection from a soil-void interface has negative polarity. If the soil dielectric constant is εr = 6, confirm this polarity using the reflection coefficient formula.

<details>
<summary>Show solution</summary>
Reflection coefficient:
$$R = \frac{\sqrt{\varepsilon_2} - \sqrt{\varepsilon_1}}{\sqrt{\varepsilon_2} + \sqrt{\varepsilon_1}}$$

For soil (ε₁ = 6) overlying air void (ε₂ = 1):
$$R = \frac{\sqrt{1} - \sqrt{6}}{\sqrt{1} + \sqrt{6}} = \frac{1 - 2.45}{1 + 2.45} = \frac{-1.45}{3.45} = -0.42$$

The negative reflection coefficient confirms the observed negative polarity. Energy reflects with inverted phase when entering a lower-dielectric medium (void).
</details>

**Exercise 18**: Instantaneous frequency analysis shows f_inst = 320 MHz at 2 m depth in material where surface frequency was 400 MHz. Estimate the frequency-dependent attenuation coefficient αf.

<details>
<summary>Show solution</summary>
Using the frequency decay relationship:
$$f_{\text{observed}} = f_0 \cdot e^{-\alpha_f z}$$

$$320 = 400 \cdot e^{-\alpha_f \times 2}$$
$$\frac{320}{400} = e^{-2\alpha_f}$$
$$0.8 = e^{-2\alpha_f}$$
$$\ln(0.8) = -2\alpha_f$$
$$\alpha_f = \frac{-\ln(0.8)}{2} = \frac{0.223}{2} = 0.111 \text{ m}^{-1}$$

This indicates moderate frequency-dependent attenuation, typical of slightly moist or clay-bearing soils.
</details>

### 3D GPR exercises

**Exercise 19**: A 3D survey uses a 400 MHz antenna in soil with velocity 0.12 m/ns. Calculate the Nyquist line spacing and comment on whether 0.25 m spacing would be adequate.

<details>
<summary>Show solution</summary>
Nyquist line spacing:
$$\Delta y \leq \frac{\lambda}{4} = \frac{v}{4f_c} = \frac{0.12}{4 \times 0.4} = \frac{0.12}{1.6} = 0.075 \text{ m}$$

A 0.25 m spacing is 3.3× the Nyquist limit. This will cause spatial aliasing for features with high spatial frequencies (small lateral extent).

For practical archaeological surveys, 0.25 m spacing is often acceptable because:
1. Archaeological targets (walls, chambers) are typically larger than 0.5 m
2. Migration and interpolation can partially compensate
3. Survey efficiency improves dramatically

However, for detailed mapping of small features (pottery, individual stones), tighter spacing would be needed.
</details>

**Exercise 20**: A time slice at 45 ns shows a circular anomaly with diameter 3.5 m. If the velocity is 0.10 m/ns, at what depth does this feature occur? What archaeological feature might produce this signature in a kurgan context?

<details>
<summary>Show solution</summary>
Depth calculation:
$$z = \frac{v \cdot t}{2} = \frac{0.10 \times 45}{2} = 2.25 \text{ m}$$

A circular anomaly 3.5 m in diameter at 2.25 m depth in a kurgan context could be:

1. **Burial chamber**: Central chambers in Scythian and Saka kurgans are often circular or oval, 2-5 m diameter
2. **Ring ditch intersection**: Concentric ritual ditches may appear circular in time slice
3. **Collapsed stone cairn**: Stone covering that has settled into circular form
4. **Wooden chamber remains**: Collapsed timber structure with circular footprint

The depth (2.25 m) is consistent with a primary burial chamber in a medium-sized kurgan.
</details>

**Exercise 21**: Inverse distance weighting with p = 2 is used to interpolate a value at location (5, 5). Four data points are: (4, 5) with value 100; (6, 5) with value 80; (5, 4) with value 90; (5, 6) with value 110. Calculate the interpolated value.

<details>
<summary>Show solution</summary>
Distances from (5, 5) to each point:
- d₁ = 1 (to point at (4,5))
- d₂ = 1 (to point at (6,5))  
- d₃ = 1 (to point at (5,4))
- d₄ = 1 (to point at (5,6))

Weights with p = 2: $w_i = 1/d_i^2$
- w₁ = w₂ = w₃ = w₄ = 1/1² = 1

Since all weights are equal (equidistant points):
$$A(5,5) = \frac{1 \times 100 + 1 \times 80 + 1 \times 90 + 1 \times 110}{1 + 1 + 1 + 1} = \frac{380}{4} = 95$$
</details>

### Interpretation exercises

**Exercise 22**: A radargram shows a reflection that terminates abruptly against a dipping surface. Younger sediments above the termination show continuous, parallel reflections. Identify this reflection termination pattern and its geological significance.

<details>
<summary>Show solution</summary>
This is an **onlap** termination pattern. Younger, continuous parallel reflections lap onto an older dipping surface.

Geological significance in Central Asian context:
- The dipping surface could be the original ground surface or earlier construction phase of a burial mound
- Younger sediments represent later deposition or construction
- In kurgan context: this might indicate a secondary burial phase constructed over an earlier mound
- In natural settings: represents transgressive deposition filling topographic lows

This pattern is diagnostic of time-sequential deposition where later deposits progressively cover an existing slope.
</details>

**Exercise 23**: A GPR profile across a suspected qanat shows a strong diffraction hyperbola at depth 4 m with apex amplitude 3× background. The hyperbola half-width at the apex depth is 1.8 m. Estimate the tunnel diameter.

<details>
<summary>Show solution</summary>
Using the tunnel diameter formula:
$$D \approx 2\sqrt{W \cdot z - z^2}$$

where W = 1.8 m (half-width) and z = 4 m (depth):

$$D \approx 2\sqrt{1.8 \times 4 - 4^2}$$

Wait - this gives a negative value under the square root, indicating the formula needs clarification. The correct interpretation:

The half-width W of the diffraction hyperbola at the apex level relates to the Fresnel zone. For a point diffractor:
$$r_F = \sqrt{\frac{\lambda z}{2}}$$

For a tunnel with finite diameter, the apparent diffraction is broadened. A more practical approach:

If the hyperbola half-width at depth z is W, and assuming the tunnel diameter D adds to the point-source diffraction width:
$$W_{\text{observed}} = W_{\text{point}} + D/2$$

For a 400 MHz antenna with v = 0.1 m/ns at z = 4 m:
$$W_{\text{point}} = r_F = \sqrt{\frac{0.25 \times 4}{2}} = 0.71 \text{ m}$$

$$D = 2(W_{\text{observed}} - W_{\text{point}}) = 2(1.8 - 0.71) = 2.18 \text{ m}$$

This is a large qanat tunnel, consistent with main trunk lines in major irrigation systems.
</details>

**Exercise 24**: Parallel GPR profiles across a kurgan show chaotic reflections in the center transitioning to continuous dipping reflections on the flanks. Interpret this reflection pattern.

<details>
<summary>Show solution</summary>
This classic kurgan signature indicates:

**Central chaotic zone**: 
- Disturbed fill material from chamber construction
- Possible chamber collapse
- Backfill from ancient or modern disturbance
- Stone cairn or jumbled construction materials

**Flanking dipping reflections**:
- Intact mound construction layers
- Original kurgan stratigraphy preserved
- Layers dip away from center following mound geometry

**Archaeological interpretation**:
- The kurgan has a central burial feature (chamber or pit)
- Construction method involved layered mound building around a central structure
- The chaotic zone may mark the primary burial location
- Width of chaotic zone indicates approximate chamber/pit size
- Preservation state can be assessed from chaos intensity

This pattern guides excavation strategy: the chaotic zone is the target for burial investigation.
</details>

**Exercise 25**: A canal cross-section shows a U-shaped reflection with base at 1.2 m depth and shoulders at 0.6 m depth. The channel width at the shoulder level is 3.5 m. Calculate the channel cross-sectional area assuming parabolic geometry.

<details>
<summary>Show solution</summary>
For a parabolic channel cross-section:

Channel depth: $d = 1.2 - 0.6 = 0.6$ m
Channel half-width at top: $w = 3.5/2 = 1.75$ m

Parabolic equation: $y = \frac{d}{w^2}x^2$ where y is depth below shoulder and x is horizontal distance from center.

Cross-sectional area for a parabola:
$$A = \frac{2}{3} \times \text{width} \times \text{depth} = \frac{2}{3} \times 3.5 \times 0.6 = 1.4 \text{ m}^2$$

This is a moderate-sized irrigation canal. For context, Achaemenid and later Central Asian canals ranged from 0.5 m² (small distributary) to 10+ m² (main trunk canal).
</details>

### Advanced exercises

**Exercise 26**: Design a complete processing workflow for a GPR survey over a suspected Zoroastrian dakhma (tower of silence) in eastern Iran. Specify antenna frequency, key processing steps, and interpretation targets.

<details>
<summary>Show solution</summary>
**Survey design**:
- Primary antenna: 250-400 MHz for 2-4 m penetration
- Secondary antenna: 100 MHz for deeper reconnaissance
- Line spacing: 0.5 m over main platform, 1.0 m in surrounding area
- Orientation: Grid pattern aligned to cardinal directions (Zoroastrian orientation)

**Processing workflow**:
1. Time-zero correction (surface irregularity expected)
2. Dewow with 40 ns window (250 MHz antenna)
3. Bandpass filter: 60-600 MHz
4. Background removal (remove platform surface multiples)
5. Velocity analysis from hyperbolas (expect v ≈ 0.10-0.12 m/ns in dry limestone/earth)
6. SEC gain with moderate β for visualization
7. Kirchhoff migration (variable surface topography)
8. Depth conversion
9. 3D volume generation with 0.5 m slice interval

**Interpretation targets**:
- Circular platform structure and walls
- Central pit or ossuary features
- Drainage channels (ritual washing areas)
- Stairs or access ramps
- Disturbance patterns from later use or vandalism
- Stone/mudbrick differentiation from reflection strength
</details>

**Exercise 27**: A 3D GPR volume shows a linear diffraction sequence trending NW-SE at depths 3-5 m, with regularly spaced vertical diffraction columns every 30 m. Identify the feature and estimate the qanat gradient if the depth increases from 3.0 m at the SE end to 5.0 m at the NW end over 800 m horizontal distance.

<details>
<summary>Show solution</summary>
**Feature identification**: This is a **qanat** (karez) underground aqueduct system.

The linear diffraction sequence is the main tunnel, and the regularly spaced (30 m) vertical diffractions are the **access/ventilation shafts** used for construction and maintenance.

**Gradient calculation**:
Depth change: $\Delta z = 5.0 - 3.0 = 2.0$ m
Horizontal distance: $\Delta x = 800$ m

Gradient: 
$$\text{slope} = \frac{\Delta z}{\Delta x} = \frac{2.0}{800} = 0.0025 = 0.25\%$$

This is equivalent to 2.5 m per kilometer, or 1:400.

**Engineering assessment**: This gradient is typical for qanats:
- Steep enough to maintain water flow
- Gentle enough to prevent erosion
- Consistent with Persian/Central Asian qanat engineering (typically 0.1-0.5%)

The shaft spacing of 30 m indicates construction in moderately stable ground (shaft spacing increases in more stable materials).
</details>

**Exercise 28**: Velocity analysis across a site shows v = 0.14 m/ns in undisturbed loess and v = 0.09 m/ns in an archaeological mound. If a reflector appears at t = 50 ns in both areas, calculate the actual depth difference between the two areas.

<details>
<summary>Show solution</summary>
**Undisturbed loess**:
$$z_{\text{loess}} = \frac{v_{\text{loess}} \cdot t}{2} = \frac{0.14 \times 50}{2} = 3.5 \text{ m}$$

**Archaeological mound**:
$$z_{\text{mound}} = \frac{v_{\text{mound}} \cdot t}{2} = \frac{0.09 \times 50}{2} = 2.25 \text{ m}$$

**Depth difference**: $3.5 - 2.25 = 1.25$ m

**Interpretation**: The same travel time corresponds to a 1.25 m shallower depth in the mound material. This velocity difference suggests:
- Higher moisture content in mound (disturbed, less compacted)
- Different soil composition (mixed cultural material)
- Possible clay content increase

Without proper velocity correction, features in the mound would be interpreted 1.25 m deeper than their actual position when using the loess velocity.
</details>

**Exercise 29**: Compare the advantages of Kirchhoff migration versus Stolt migration for processing GPR data from a kurgan with significant surface topography and laterally varying fill materials.

<details>
<summary>Show solution</summary>
**Kirchhoff migration advantages for this scenario**:

1. **Topographic flexibility**: Kirchhoff naturally handles irregular datum surfaces. The kurgan's domed surface can be incorporated directly.

2. **Lateral velocity variation**: The algorithm allows different velocities for intact mound vs. disturbed fill:
   - Apply v = 0.12 m/ns in intact construction layers
   - Apply v = 0.09 m/ns in central disturbed zone

3. **Aperture control**: Migration aperture can be adjusted to prevent artifacts from steep topography.

4. **True-amplitude option**: Can preserve relative amplitudes for quantitative interpretation.

**Stolt migration limitations**:

1. **Flat datum assumption**: Requires data redatuming to flat surface first, introducing additional processing steps and potential errors.

2. **Constant velocity requirement**: Cannot accommodate the lateral velocity change between disturbed and undisturbed zones without domain splitting.

3. **Efficiency vs. accuracy tradeoff**: While computationally faster, accuracy suffers in complex geometries.

**Recommendation**: Use Kirchhoff migration for kurgan data, accepting the computational cost for improved accuracy in the complex velocity-topography environment.
</details>

**Exercise 30**: A research project proposes using GPR to map the complete underground network of a Silk Road caravanserai. Design the survey parameters, processing workflow, and quality control measures for this project.

<details>
<summary>Show solution</summary>
**Survey parameters**:

*Antenna selection*:
- Primary: 400 MHz for wall/floor detail (resolution ~0.1 m, penetration 2-3 m)
- Secondary: 200 MHz for deeper foundations (resolution ~0.2 m, penetration 4-5 m)

*Grid design*:
- Interior: 0.25 m line spacing, orthogonal grid
- Exterior: 0.50 m spacing extending 20 m beyond visible walls
- Profile orientation: N-S and E-W for wall detection regardless of orientation

*Data acquisition*:
- Trace interval: 0.02 m (wheel encoder)
- Time window: 80 ns (400 MHz) / 150 ns (200 MHz)
- Samples per trace: 512 minimum
- Stacking: 4-16 stacks in noisy areas

**Processing workflow**:
1. Data quality review (check positioning, remove bad traces)
2. Time-zero correction (metal plate calibration on-site)
3. Dewow filter (10 ns window for 400 MHz)
4. DC offset removal
5. Bandpass: 100-800 MHz (400 MHz data)
6. Velocity analysis at multiple locations (expect spatial variation)
7. Background removal (moving window, 50 traces)
8. Migration (Kirchhoff with spatially varying velocity)
9. Depth conversion
10. 3D volume construction
11. Depth slice generation (0.1 m intervals)
12. Attribute calculation (envelope, phase)

**Quality control**:
- Daily calibration over known metal plate
- Cross-line ties at grid intersections (amplitude match within 15%)
- Repeat lines (10% of total) for reproducibility
- Ground truth at exposed wall sections
- Independent velocity verification via hyperbola fitting
- Processing parameter documentation
- Archive raw data with metadata

**Deliverables**:
- Migrated depth slices at 0.1 m intervals
- Interpreted wall/floor plan overlay
- 3D visualization of major structures
- Attribute maps highlighting construction phases
- Uncertainty assessment for depth and position
</details>

## Summary

Advanced GPR processing transforms raw data into interpretable subsurface images. The key principles are:

1. **Each processing step must have physical justification** - blind filtering creates artifacts
2. **Velocity is fundamental** - accurate depth conversion and migration require good velocity models
3. **Migration is essential** - unmigrated data mispositions features and blurs point targets
4. **Attributes extend interpretation** - phase, frequency, and envelope reveal properties beyond amplitude
5. **3D visualization integrates information** - time slices and volumes reveal spatial relationships

For Central Asian archaeology, these techniques enable detailed mapping of kurgans, qanats, irrigation systems, and urban sites that would be invisible in raw data. The combination of dry soil conditions (good penetration) and complex buried architecture (clear targets) makes this region ideal for advanced GPR applications.

Continue to the next tutorial to learn about multi-method geophysical integration, combining GPR with magnetometry, resistivity, and other techniques for comprehensive site characterization.
