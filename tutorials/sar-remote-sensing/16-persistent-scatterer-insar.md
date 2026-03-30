---
layout: tutorial
title: "How Persistent Scatterer InSAR Reveals mm-Scale Ground Motion in Central Asia"
description: "An advanced tutorial on PS-InSAR, SBAS, and multi-temporal InSAR for subsidence, uplift, and infrastructure monitoring in Central Asia, with 30 exercises and solutions."
difficulty: advanced
estimated_time: "4-5 hours"
series: sar-remote-sensing
order: 16
---

## Why PS-InSAR matters

Central Asia is a region of slow, relentless ground deformation. Aquifers beneath the Fergana Valley compact as irrigation wells draw water faster than recharge replaces it. Open-pit mines at Muruntau and Kumtor reshape tonnes of overburden daily, triggering slope creep. The Tien Shan front thrusts upward at a few millimetres per year, loading faults that periodically rupture. Across the Aral Sea basin, decades of desiccation have caused regional subsidence that alters drainage and accelerates desertification.

Conventional Differential InSAR (DInSAR) measures surface displacement between two SAR acquisitions, but atmospheric delay, temporal decorrelation, and the difficulty of separating signal from noise limit its accuracy. Persistent Scatterer InSAR (PS-InSAR) overcomes these limitations by identifying pixels that maintain coherent radar phase over months to years and inverting their phase history to extract displacement time series with millimetre-level precision.

<div class="info-box tip">
  <strong>Key idea</strong>
  PS-InSAR converts a stack of tens to hundreds of SAR images into a displacement time series at each stable scatterer, achieving sub-millimetre velocity precision by exploiting temporal redundancy and spatial filtering of atmospheric signals.
</div>

Subsidence rates of 5–30 mm yr⁻¹ from groundwater extraction, tectonic loading of 1–4 mm yr⁻¹ along the Tien Shan front, and mining-induced settlement of 10–100 mm yr⁻¹ all fall within the detection range of modern multi-temporal InSAR.

## PS-InSAR vs DInSAR vs SBAS fundamentals

### Conventional DInSAR

A single differential interferogram records:

$$
\phi_{\text{diff}} = \phi_{\text{defo}} + \phi_{\text{topo}} + \phi_{\text{atmo}} + \phi_{\text{orbit}} + \phi_{\text{noise}}
$$

where $\phi_{\text{defo}}$ is the deformation phase, $\phi_{\text{topo}}$ the residual topographic phase, $\phi_{\text{atmo}}$ the atmospheric delay, $\phi_{\text{orbit}}$ the orbital error, and $\phi_{\text{noise}}$ thermal and decorrelation noise. In a single interferogram these terms are inseparable, limiting accuracy to 5–10 mm.

### Persistent Scatterer InSAR

PS-InSAR uses $N$ SAR images, forming $N - 1$ interferograms relative to a single master. Pixels with stable backscatter amplitude — persistent scatterers such as building corners, rock outcrops, and metal structures — preserve phase coherence across the stack. The phase at PS pixel $p$ in interferogram $i$ is:

$$
\phi_p^{(i)} = \frac{4\pi}{\lambda} \left( v_p \, t_i + \frac{B_{\perp}^{(i)} \, \Delta h_p}{R \sin\theta} \right) + \phi_{\text{atmo},p}^{(i)} + \phi_{\text{res},p}^{(i)}
$$

where $v_p$ is the mean LOS velocity, $t_i$ the temporal baseline, $B_{\perp}^{(i)}$ the perpendicular baseline, $\Delta h_p$ the DEM error, $R$ the slant range, $\theta$ the incidence angle, and $\phi_{\text{res}}$ absorbs higher-order deformation and residual noise.

### Small Baseline Subset (SBAS)

SBAS selects pairs with small perpendicular and temporal baselines, working on distributed scatterers by applying spatial multilooking. The network of interferograms is inverted via SVD to recover displacement time series. SBAS suits rural Central Asia — agricultural fields, desert plains, and mountain slopes — where man-made PS are sparse.

| Feature | DInSAR | PS-InSAR | SBAS |
|---------|--------|----------|------|
| Input images | 2 | 20–200+ | 20–200+ |
| Scatterer type | Any coherent pixel | Point scatterers | Distributed scatterers |
| Spatial resolution | Full | Full (point targets) | Multilooked (reduced) |
| Atmospheric separation | Not possible | Phase filtering | Spatio-temporal filtering |
| Typical accuracy | 5–10 mm per pair | 0.5–1 mm yr⁻¹ velocity | 1–2 mm yr⁻¹ velocity |
| Best terrain | Urban, rocky | Urban, infrastructure | Rural, natural surfaces |

## Persistent scatterer selection

### Amplitude dispersion index

The amplitude dispersion index $D_A$ is:

$$
D_A = \frac{\sigma_A}{\bar{A}}
$$

where $\sigma_A$ is the amplitude standard deviation and $\bar{A}$ the mean. A typical threshold is $D_A < 0.4$ for initial candidates, tightened to $D_A < 0.25$ in final selection. For strong scatterers the relationship $\sigma_\phi \approx D_A$ holds when $D_A < 0.25$.

### Coherence-based selection

Temporal coherence $\gamma_t$ provides the definitive test:

$$
\gamma_t = \frac{1}{N} \left| \sum_{i=1}^{N} e^{j(\phi_i - \hat{\phi}_i)} \right|
$$

where $\phi_i$ is observed and $\hat{\phi}_i$ modelled phase after fitting velocity and DEM error. A threshold of $\gamma_t > 0.7$ is standard.

<div class="info-box tip">
  <strong>Key idea</strong>
  Amplitude dispersion is a quick, model-free pre-filter reducing millions of pixels to tens of thousands. Temporal coherence is the definitive test after model fitting. Together they form a two-stage selection funnel.
</div>

### PS density across Central Asian terrain

| Land cover | Typical PS density (PS km⁻²) | Examples |
|------------|-------------------------------|----------|
| Dense urban | 500–3000 | Tashkent centre, Almaty CBD |
| Industrial | 200–1000 | Navoi mining complex |
| Rocky outcrop | 10–100 | Tien Shan ridgelines |
| Agricultural | 0–5 | Fergana Valley irrigated fields |
| Vegetated slopes | 0–2 | Walnut forests of Jalal-Abad |

## Atmospheric phase screen estimation and removal

The tropospheric delay difference between acquisitions can produce 10–30 mm equivalent phase — far exceeding deformation signals. The atmospheric phase screen (APS) is spatially correlated but temporally uncorrelated, while deformation is temporally smooth and often spatially localised.

### Spatio-temporal filtering

1. **Temporal high-pass filter**: Isolates the temporally random APS from temporally smooth deformation
2. **Spatial low-pass filter**: Smooths the high-passed residuals to estimate the spatially correlated APS

$$
\hat{\phi}_{\text{atmo},p}^{(i)} = \text{LP}_{\text{space}} \left[ \text{HP}_{\text{time}} \left( \phi_{\text{res},p}^{(i)} \right) \right]
$$

<div class="info-box warning">
  <strong>Caution</strong>
  If deformation has a spatial wavelength comparable to the APS (e.g. broad subsidence bowls of 5–10 km), the spatial filter may absorb deformation. Use external corrections from ERA5 or GACOS in such cases.
</div>

### Stratified tropospheric delay

In mountainous Central Asia, topography-correlated delay is critical. Elevation differences of 2000–4000 m along the Tien Shan produce differential delays of 20–40 mm per pair:

$$
\phi_{\text{strat}} = K \cdot h_p
$$

where $K$ varies with weather and $h_p$ is pixel elevation. This is removed by fitting a phase-elevation relationship at each epoch.

### External corrections

- **ERA5 / GACOS**: Global atmospheric model providing tropospheric corrections at ~90 m resolution
- **GNSS zenith total delay**: Point measurements from stations at Bishkek, Tashkent, and Kitab

Central Asia's sparse GNSS network makes ERA5/GACOS the practical choice for most studies.

## Network inversion and velocity estimation

### Phase model

Given $M$ interferograms at PS pixel $p$, the observed unwrapped phases relate to unknowns by:

$$
\mathbf{\phi}_p = \mathbf{A} \begin{pmatrix} v_p \\ \Delta h_p \end{pmatrix} + \mathbf{\phi}_{\text{atmo}} + \mathbf{e}_p
$$

After atmospheric removal, parameters are estimated by weighted least squares:

$$
\begin{pmatrix} \hat{v}_p \\ \widehat{\Delta h}_p \end{pmatrix} = \left( \mathbf{A}^T \mathbf{W} \mathbf{A} \right)^{-1} \mathbf{A}^T \mathbf{W} \, \tilde{\mathbf{\phi}}_p
$$

### Phase unwrapping in time

The maximum unambiguous velocity for C-band ($\lambda = 5.6$ cm) with 12-day repeat:

$$
|v_{\max}| = \frac{\lambda}{4 \Delta t} = \frac{0.056}{4 \times 12/365} \approx 42.7 \text{ cm yr}^{-1}
$$

This exceeds virtually all Central Asian deformation rates.

### SBAS inversion

SBAS connects pairs through an incidence matrix $\mathbf{B}$. The minimum-norm SVD solution bridges disconnected subsets:

$$
\hat{\mathbf{d}} = \mathbf{B}^+ \, \mathbf{\phi}
$$

<div class="info-box tip">
  <strong>Key idea</strong>
  PS-InSAR and SBAS can be combined: StaMPS implements a merged approach where PS and distributed scatterers are jointly inverted, maximising spatial coverage across urban and rural terrain.
</div>

## SBAS for distributed scatterers

Distributed scatterers (DS) from natural surfaces have coherence enhanced by multilooking over $L$ pixels:

$$
\hat{\gamma} = \frac{\left| \sum_{k=1}^{L} s_1^{(k)} \, s_2^{(k)*} \right|}{\sqrt{\sum_{k=1}^{L} |s_1^{(k)}|^2 \cdot \sum_{k=1}^{L} |s_2^{(k)}|^2}}
$$

Advanced processing uses statistically homogeneous pixel (SHP) selection — only neighbours with similar scattering statistics are included, preserving edges while maximising estimation windows. The Phase Linking approach finds the maximum-likelihood phase estimates from the full $N \times N$ coherence matrix:

$$
\hat{\boldsymbol{\psi}} = \arg\max_{\boldsymbol{\psi}} \, \boldsymbol{\psi}^H \, \hat{\mathbf{\Gamma}} \, \boldsymbol{\psi}
$$

This is particularly useful for loess soils and alluvial fans of Central Asia that decorrelate within weeks.

## Error sources in multi-temporal InSAR

### DEM errors

A DEM error $\Delta h$ produces phase scaling with perpendicular baseline:

$$
\phi_{\text{DEM}} = \frac{4\pi}{\lambda} \cdot \frac{B_\perp \, \Delta h}{R \sin\theta}
$$

For Sentinel-1 with $B_\perp < 150$ m, a 10 m DEM error produces ~2–3 mm apparent displacement. This is jointly estimated with velocity in the PS inversion.

### Atmospheric residuals

Velocity uncertainty decreases as $1/\sqrt{N}$:

$$
\sigma_v \approx \frac{\sigma_{\text{atmo}}}{\Delta T \sqrt{N/2}}
$$

For a 5-year stack with 150 images and $\sigma_{\text{atmo}} = 8$ mm: $\sigma_v \approx 0.15$ mm yr⁻¹.

### Phase unwrapping errors

Unwrapping errors introduce $2\pi$ jumps (~28 mm for C-band). Closure phases detect these:

$$
\phi_{12} + \phi_{23} + \phi_{31} = 0 \quad (\text{ideally})
$$

<div class="info-box danger">
  <strong>Critical check</strong>
  Always inspect closure phases and temporal coherence maps before interpreting velocity results. A single unwrapping error can bias the entire time series by centimetres.
</div>

### Thermal expansion

A steel structure ($\alpha \approx 12 \times 10^{-6}$ °C⁻¹, height 50 m) with 40°C seasonal range produces:

$$
\Delta d_{\text{thermal}} = \alpha \, H \, \Delta T \, \cos\theta \approx 12 \times 10^{-6} \times 50 \times 40 \times 0.73 \approx 17.5 \text{ mm}
$$

This periodic signal must not be mistaken for structural settlement — common in continental Central Asia.

## Software tools

### StaMPS

The most widely used open-source PS-InSAR processor: PS selection by amplitude dispersion, 3D phase unwrapping, APS filtering, and combined PS/SBAS processing. Uses ISCE2 or SNAP for co-registration, then MATLAB/Octave for PS analysis.

### SNAP

ESA's graphical tool for Sentinel-1 TOPS co-registration, interferogram formation, and StaMPS export.

### ISCE2

JPL's Python framework for precise co-registration with enhanced spectral diversity (ESD) and stack processing via `topsStack`.

### MintPy

Modern Python time-series package: network inversion, ERA5/GACOS tropospheric correction, velocity and seasonal estimation, and publication-quality visualisation.

<div class="info-box tip">
  <strong>Recommended workflow</strong>
  Start with ISCE2 topsStack for co-registration, then MintPy for time-series inversion. This Python-only pipeline avoids MATLAB licensing and has excellent Jupyter notebook tutorials.
</div>

### Typical processing chain

```
SAR SLC stack (Sentinel-1 IW)
        │
        ▼
  Co-registration & interferograms (ISCE2 / SNAP)
        │
        ▼
  PS candidate selection (StaMPS) or coherence selection (MintPy)
        │
        ▼
  Phase unwrapping (3D / network-based)
        │
        ▼
  Atmospheric correction (filter / ERA5 / GACOS)
        │
        ▼
  Velocity estimation & time-series inversion
        │
        ▼
  Geocoding & visualisation (QGIS, matplotlib)
```

## Central Asia applications

### Mining subsidence and slope stability

The Muruntau gold mine (Uzbekistan, world's largest open-pit by area) and Kumtor mine (Kyrgyzstan, 4000 m elevation) produce measurable deformation: pit wall creep at 10–100 mm yr⁻¹, tailings dam settlement revealing accelerating consolidation, and waste dump differential movement of 20–50 mm yr⁻¹.

### Groundwater-induced subsidence

Intensive irrigation across the Fergana Valley and around Tashkent depletes confined aquifers, compacting clay-rich aquitards. SBAS reveals subsidence bowls of 10–25 mm yr⁻¹ near major pumping stations, with seasonal signals diagnostic of aquifer storage properties.

### Tectonic deformation

The India-Eurasia collision drives 20–25 mm yr⁻¹ of shortening distributed across Central Asia:

- **Tien Shan frontal thrusts**: 1–4 mm yr⁻¹ strain accumulation
- **Talas-Fergana fault**: ~10 mm yr⁻¹ right-lateral slip
- **Northern Pamir boundary**: 10–15 mm yr⁻¹ convergence

PS-InSAR provides spatially continuous velocity fields between sparse GNSS stations.

### Infrastructure monitoring

Sentinel-1 PS-InSAR enables monitoring of Nurek Dam (Tajikistan, 300 m tall), railway subsidence on loess terrain, and gas pipelines from Turkmenistan to China crossing hundreds of kilometres of desert.

## Visualization and interpretation

### Velocity maps and time series

Velocity maps use blue (uplift/toward satellite) through white (stable) to red (subsidence/away). All velocities are relative to a chosen reference point. Time-series at individual PS reveal:

$$
d_p(t) = v_p \, t + a_p \sin(2\pi t + \phi_p) + \sum_k s_k \, H(t - t_k) + r_p(t)
$$

where $v_p$ is velocity, $a_p$ seasonal amplitude, $s_k$ step displacements at known epochs, and $r_p(t)$ the residual.

### LOS decomposition

Combining ascending and descending data separates vertical ($d_v$) and east-west ($d_e$) components:

$$
d_v = \frac{d_{\text{LOS}}^{\text{asc}} \sin\theta_d + d_{\text{LOS}}^{\text{desc}} \sin\theta_a}{\sin(\theta_a + \theta_d)}
$$

$$
d_e = \frac{d_{\text{LOS}}^{\text{asc}} \cos\theta_d - d_{\text{LOS}}^{\text{desc}} \cos\theta_a}{\sin(\theta_a + \theta_d)}
$$

<div class="info-box note">
  <strong>North-south limitation</strong>
  Near-polar SAR orbits are insensitive to north-south displacement. This component must be constrained by GNSS.
</div>

### Cross-validation

Validate PS-InSAR against GNSS time series, levelling surveys, artificial corner reflectors, and ascending-descending consistency. Agreement within 1–2 mm yr⁻¹ is expected.

## Practical workflow checklist

<ul class="checklist">
  <li>Download Sentinel-1 SLC stack (minimum 30 images, 2+ years)</li>
  <li>Verify precise orbit files (available 20 days after acquisition)</li>
  <li>Select master image minimising total baseline spread</li>
  <li>Co-register all images using enhanced spectral diversity</li>
  <li>Form interferograms: single-master for PS, small-baseline for SBAS</li>
  <li>Remove flat-earth and topographic phase using Copernicus DEM</li>
  <li>Apply amplitude dispersion threshold (D_A &lt; 0.4) for PS candidates</li>
  <li>Estimate velocity and DEM error by periodogram or network inversion</li>
  <li>Perform 3D phase unwrapping</li>
  <li>Estimate and remove atmospheric phase screen</li>
  <li>Evaluate temporal coherence; reject PS with γ_t &lt; 0.7</li>
  <li>Check closure phases for unwrapping errors</li>
  <li>Geocode results and select stable reference point</li>
  <li>Generate velocity map and displacement time series</li>
  <li>Decompose LOS using ascending and descending data</li>
  <li>Validate against GNSS or levelling</li>
  <li>Interpret time series: linear, seasonal, and transient components</li>
  <li>Document processing parameters and uncertainty estimates</li>
</ul>

## Exercises

**Exercise 1**: A pixel has mean amplitude $\bar{A} = 200$ and standard deviation $\sigma_A = 50$. Calculate $D_A$. Would it be selected as a PS with threshold $D_A < 0.4$?

<details>
<summary>Show Solution</summary>
$D_A = \sigma_A / \bar{A} = 50 / 200 = 0.25$. Since $0.25 < 0.4$, the pixel passes the initial PS selection threshold.
</details>

**Exercise 2**: Name three types of objects in Central Asian cities that act as persistent scatterers.

<details>
<summary>Show Solution</summary>
(1) Building corners and facades — especially Soviet-era concrete blocks in Bishkek and Tashkent. (2) Metal structures: lamp posts, satellite dishes, fences. (3) Rock outcrops on hillsides near cities. All maintain stable radar backscatter.
</details>

**Exercise 3**: Why does irrigated farmland in the Fergana Valley have near-zero PS density?

<details>
<summary>Show Solution</summary>
Agricultural surfaces change as crops grow, are harvested, and fields are ploughed. This temporal change causes decorrelation — phase becomes random between acquisitions. Without a stable dominant scatterer, amplitude fluctuates strongly, giving high $D_A$ values that fail PS selection.
</details>

**Exercise 4**: Explain the difference between single-master and small-baseline interferometric networks.

<details>
<summary>Show Solution</summary>
Single-master forms all interferograms relative to one reference image (star graph), maximising temporal baseline diversity. Small-baseline connects pairs with small temporal and perpendicular baselines (complex graph), avoiding decorrelation but potentially splitting into disconnected subsets requiring SVD.
</details>

**Exercise 5**: What does temporal coherence $\gamma_t$ measure?

<details>
<summary>Show Solution</summary>
It measures how well the fitted phase model (velocity + DEM error) explains the observed phase. Values near 1 indicate a good model fit and reliable PS; values near 0 indicate the pixel is dominated by noise or non-linear deformation unsuitable for PS analysis.
</details>

**Exercise 6**: A stack has 100 images over 4 years with single-epoch atmospheric noise $\sigma_{\text{atmo}} = 8$ mm. Estimate velocity uncertainty.

<details>
<summary>Show Solution</summary>
$\sigma_v = \sigma_{\text{atmo}} / (\Delta T \sqrt{N/2}) = 8 / (4 \times \sqrt{50}) = 8 / 28.3 \approx 0.28$ mm yr⁻¹. Sub-millimetre precision illustrates the power of long time series.
</details>

**Exercise 7**: Why is the Aral Sea region challenging for PS-InSAR but suitable for SBAS?

<details>
<summary>Show Solution</summary>
No man-made structures exist on the exposed seabed, so PS density is near zero. However, bare soil and salt flats maintain coherence over short baselines, making them ideal for SBAS with multilooking and small temporal baseline pairs.
</details>

**Exercise 8**: A PS shows $-12$ mm yr⁻¹ LOS velocity with incidence angle $39°$. What is the vertical rate assuming pure vertical motion?

<details>
<summary>Show Solution</summary>
$d_v = d_{\text{LOS}} / \cos\theta = -12 / \cos(39°) = -12 / 0.777 = -15.4$ mm yr⁻¹. Negative sign indicates subsidence.
</details>

**Exercise 9**: Why is Sentinel-1's 12-day revisit better for PS-InSAR than ERS's 35-day repeat?

<details>
<summary>Show Solution</summary>
Shorter revisit means: (1) smaller inter-acquisition phase increments, improving temporal unwrapping; (2) ~30 images per year vs ~10, improving atmospheric averaging; (3) less decorrelation per pair; (4) higher maximum unambiguous velocity.
</details>

**Exercise 10**: Give two advantages and two disadvantages of ERA5 atmospheric corrections for a Tien Shan study.

<details>
<summary>Show Solution</summary>
Advantages: (1) captures topography-correlated stratified delay critical for extreme elevation ranges; (2) does not risk absorbing long-wavelength deformation as spatial filtering can. Disadvantages: (1) limited spatial resolution (~30 km) misses local atmospheric variations; (2) cannot capture fine-scale turbulent tropospheric delays.
</details>

**Exercise 11**: Calculate the maximum unambiguous LOS velocity for Sentinel-1 C-band ($\lambda = 5.6$ cm, $\Delta t = 12$ days).

<details>
<summary>Show Solution</summary>
$v_{\max} = \lambda / (4\Delta t) = 0.056 / (4 \times 12/365) = 0.056 / 0.1315 = 42.6$ cm yr⁻¹. This far exceeds typical Central Asian deformation rates.
</details>

**Exercise 12**: A 10 m DEM error exists with $B_\perp = 120$ m, $R = 850$ km, $\theta = 39°$, $\lambda = 5.6$ cm. Calculate the phase error and equivalent displacement.

<details>
<summary>Show Solution</summary>
$\phi_{\text{DEM}} = (4\pi/\lambda)(B_\perp \Delta h)/(R\sin\theta) = (224.4)(1200)/(534{,}650) = 0.504$ rad. Displacement: $d = \phi\lambda/(4\pi) = 0.504 \times 0.056/(4\pi) = 2.25$ mm.
</details>

**Exercise 13**: A PS near Tashkent shows $d(t) = -8t + 5\sin(2\pi t)$ mm. What is the velocity? What causes the seasonal component?

<details>
<summary>Show Solution</summary>
Linear velocity is $-8$ mm yr⁻¹ (subsidence) with 5 mm seasonal amplitude. The seasonal signal likely reflects groundwater fluctuation: increased summer irrigation pumping lowers the water table causing compaction, with partial winter rebound.
</details>

**Exercise 14**: Ascending ($\theta_a = 39°$) LOS velocity is $-10$ mm yr⁻¹; descending ($\theta_d = 39°$) is $-8$ mm yr⁻¹. Calculate vertical and east-west components.

<details>
<summary>Show Solution</summary>
$d_v = ((-10)(0.629) + (-8)(0.629))/\sin(78°) = -11.32/0.978 = -11.6$ mm yr⁻¹ (subsidence). $d_e = ((-10)(0.777) - (-8)(0.777))/0.978 = -1.55/0.978 = -1.6$ mm yr⁻¹ (slight westward).
</details>

**Exercise 15**: Why can't InSAR detect north-south motion on the Talas-Fergana fault?

<details>
<summary>Show Solution</summary>
SAR satellites orbit nearly north-south, so the LOS lies approximately in the east-west/vertical plane. North-south motion is perpendicular to both ascending and descending LOS directions, producing negligible phase change. The fault-parallel component must be constrained by GNSS.
</details>

**Exercise 16**: Describe the two-stage PS selection funnel and why both stages are needed.

<details>
<summary>Show Solution</summary>
Stage 1: Amplitude dispersion ($D_A < 0.4$) is a model-free filter on amplitude stability, quickly reducing millions of pixels to thousands. Stage 2: Temporal coherence ($\gamma_t > 0.7$) after phase model fitting assesses whether the model explains the data. Both are needed because amplitude stability is necessary but not sufficient — a pixel may have stable amplitude yet non-linear phase behaviour making it unsuitable as a PS.
</details>

**Exercise 17**: A closure triangle has $\phi_{12} = 2.1$, $\phi_{23} = 1.4$, $\phi_{13} = 3.8$ rad. Calculate closure phase and comment.

<details>
<summary>Show Solution</summary>
Closure phase: $\phi_{12} + \phi_{23} - \phi_{13} = 2.1 + 1.4 - 3.8 = -0.3$ rad. Non-zero but small. Possible causes: multilooking bias, processing errors, or decorrelation. Closure phases consistently exceeding $\pm 1$ rad would indicate systematic unwrapping errors.
</details>

**Exercise 18**: A 200 m east-west steel bridge in Bishkek ($\alpha = 12 \times 10^{-6}$ °C⁻¹) has 50°C annual temperature range. Estimate seasonal LOS displacement on ascending pass ($\theta = 39°$, heading $348°$).

<details>
<summary>Show Solution</summary>
Expansion: $\Delta L = 12 \times 10^{-6} \times 200 \times 50 = 120$ mm along east-west axis. Ascending LOS sensitivity to east-west: $\sin(39°) \times \cos(78°) = 0.629 \times 0.208 = 0.131$. LOS displacement: $120 \times 0.131 \approx 15.7$ mm seasonal amplitude — a clear periodic signal in the time series.
</details>

**Exercise 19**: Why is SVD needed in SBAS when the interferometric network is disconnected?

<details>
<summary>Show Solution</summary>
Temporal gaps (e.g. winter snow reducing coherence in the Tien Shan) can split the network into disconnected subsets, making the design matrix rank-deficient. SVD provides the minimum-norm solution bridging gaps by finding the smoothest time series consistent with data within each subset.
</details>

**Exercise 20**: With 60 images, how many interferograms does single-master PS-InSAR produce? How many could full SBAS produce?

<details>
<summary>Show Solution</summary>
Single-master: $N - 1 = 59$ interferograms. Full network: $N(N-1)/2 = 60 \times 59/2 = 1770$. In practice SBAS selects 200–400 small-baseline pairs from this pool.
</details>

**Exercise 21**: A mining subsidence bowl has $-45$ mm yr⁻¹ at centre and 2 km radius. Estimate edge strain.

<details>
<summary>Show Solution</summary>
Tilt: $d_{\max}/r = 45 \times 10^{-3}/2000 = 22.5\,\mu$rad. Strain: $\approx d_{\max}/(2r) = 45 \times 10^{-3}/4000 = 11.3\,\mu\epsilon$. This exceeds damage thresholds for sensitive structures (~$5\,\mu\epsilon$), indicating risk of cracking.
</details>

**Exercise 22**: Design a PS-InSAR strategy for monitoring Nurek Dam (Tajikistan). Specify data, processing, and validation.

<details>
<summary>Show Solution</summary>
Data: Sentinel-1 ascending and descending SLC stacks, minimum 3 years (ideally 5+). Processing: PS-InSAR via StaMPS or MintPy — the dam body provides excellent PS. Apply ERA5 atmospheric correction for mountainous topography. Include seasonal analysis tied to reservoir level. Validation: Compare against dam instrumentation (pendulums, extensometers), cross-validate ascending vs descending decomposed velocities, and monitor for time-series acceleration indicating structural concern.
</details>

**Exercise 23**: Temporal coherence at a PS drops from 0.85 to 0.55 when the velocity search range widens from $\pm 20$ to $\pm 50$ mm yr⁻¹. Explain.

<details>
<summary>Show Solution</summary>
The wider search range introduces more sidelobes and ambiguities, causing convergence to a local minimum rather than the global best-fit velocity. Lower coherence indicates poorer model fit. The pixel may also exhibit non-linear deformation poorly described by constant velocity. Solution: use network-based inversion rather than single-pixel periodogram.
</details>

**Exercise 24**: How does SHP selection improve distributed scatterer processing over fixed rectangular multilooking?

<details>
<summary>Show Solution</summary>
Fixed windows mix dissimilar pixels at boundaries, degrading coherence and blurring resolution. SHP tests each neighbour's amplitude distribution against the central pixel using statistical tests (Anderson-Darling or Kolmogorov-Smirnov), averaging only similar pixels. This preserves sharp boundaries, adapts window size to local homogeneity, and produces more accurate phase estimates.
</details>

**Exercise 25**: A PS in the Kyzylkum Desert shows 3 mm yr⁻¹ apparent uplift far from known deformation. List three non-tectonic explanations.

<details>
<summary>Show Solution</summary>
(1) Residual topography-correlated atmospheric delay not fully removed. (2) Orbital ramp residual mimicking a velocity gradient. (3) Reference point instability — if the reference PS is subsiding, all others appear to uplift. Additionally, seasonal soil moisture in clay-rich soils could bias the mean velocity with asymmetric temporal sampling.
</details>

**Exercise 26**: PS density is only 3 km⁻² in the Fergana Valley. Propose three strategies to increase it.

<details>
<summary>Show Solution</summary>
(1) SBAS with distributed scatterer analysis and multilooking, potentially yielding 50–200 points km⁻². (2) Install artificial corner reflectors at critical locations (pumping stations, fault traces). (3) Apply SqueeSAR or phase-linking combining PS and DS processing with adaptive SHP filtering. Additionally, L-band data (ALOS-2) would increase coherence over vegetation.
</details>

**Exercise 27**: Derive the maximum temporal baseline $\Delta t$ for unambiguous unwrapping given velocity $v_{\max}$.

<details>
<summary>Show Solution</summary>
Phase increment: $\Delta\phi = (4\pi/\lambda) v_{\max} \Delta t < \pi$. Therefore $\Delta t < \lambda/(4 v_{\max})$. For C-band and $v_{\max} = 100$ mm yr⁻¹: $\Delta t < 0.056/(4 \times 0.1) = 0.14$ yr $= 51$ days. Sentinel-1's 12 days is well within this. For $v_{\max} = 500$ mm yr⁻¹: $\Delta t < 10.2$ days — Sentinel-1 just barely fails.
</details>

**Exercise 28**: Two adjacent PS in Almaty (50 m apart) show $-2$ and $-25$ mm yr⁻¹. Is this physically plausible?

<details>
<summary>Show Solution</summary>
Velocity difference of 23 mm yr⁻¹ over 50 m implies strain rate of $460\,\mu\epsilon$ yr⁻¹ — far exceeding tectonic rates (0.01–1 $\mu\epsilon$ yr⁻¹). Plausible if one PS is on a settling building on soft soil and the other on bedrock. Could also indicate thermal dilation or an unwrapping error ($\lambda/2 \approx 28$ mm). Check temporal coherence, time series for jumps, and neighbouring PS.
</details>

**Exercise 29**: Design a MintPy workflow for groundwater subsidence monitoring around Tashkent.

<details>
<summary>Show Solution</summary>
Step 1: Process Sentinel-1 ascending SLC stack with ISCE2 topsStack (60+ images, 3+ years, small-baseline network with 90-day temporal and 150 m perpendicular thresholds). Step 2: Load into MintPy with `smallbaselineApp.py`, coherence-based network with minimum coherence 0.5. Step 3: Network inversion with coherence weighting. Step 4: ERA5 tropospheric correction (`mintpy.troposphericDelay.method = pyaps`). Step 5: Velocity estimation with linear + seasonal model. Step 6: Visualise with `view.py velocity.h5` and `tsview.py`. Validate against Tashkent GNSS stations; identify subsidence bowls correlated with pumping centres.
</details>

**Exercise 30**: A colleague claims 0.3 mm yr⁻¹ fault creep near Bishkek from 2 years of Sentinel-1 (50 images). Critically evaluate.

<details>
<summary>Show Solution</summary>
Velocity precision: $\sigma_v = 8/(2\times\sqrt{25}) = 0.8$ mm yr⁻¹. The claimed 0.3 mm yr⁻¹ is below the 1-sigma noise — not statistically significant. Two years is too short; incomplete seasonal sampling can bias results. Near the Tien Shan, topography-correlated atmospheric residuals of 1–3 mm yr⁻¹ can mimic tectonic signals. Must compare with GNSS from Bishkek IGS station. Reliably detecting 0.3 mm yr⁻¹ requires 5+ years of data or independent corroboration from multiple geometries.
</details>

## Summary

Persistent Scatterer InSAR and SBAS transform Central Asia's vast, sparsely instrumented terrain into a continuously monitored deformation field. By exploiting phase stability of permanent reflectors and temporal redundancy of multi-year SAR stacks, these methods achieve velocity precision better than 1 mm yr⁻¹ — sufficient to track groundwater depletion beneath the Fergana Valley, tectonic strain along the Tien Shan, slope instability at Kumtor and Muruntau, and settlement of critical infrastructure like Nurek Dam. As Sentinel-1 archives deepen beyond a decade and tools like MintPy mature, multi-temporal InSAR is becoming indispensable for geohazard monitoring across the region.
