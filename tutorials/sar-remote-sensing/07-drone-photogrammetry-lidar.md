---
layout: tutorial
title: "How Drone Photogrammetry and LiDAR Reveal Surface Detail in Central Asia"
description: "A math-first tutorial on UAV survey, structure-from-motion photogrammetry, LiDAR point clouds, and high-resolution terrain analysis for Central Asia, with 30 exercises and solutions."
difficulty: advanced
estimated_time: "3-4 hours"
series: sar-remote-sensing
order: 7
---

## Why this tutorial matters

Satellite imagery covers vast areas but resolves features at meter to tens-of-meter scales. Ground surveys are precise but slow and limited in extent. UAV-based photogrammetry and LiDAR occupy the critical middle ground: centimeter-level resolution over hectare-scale areas, captured in hours rather than weeks.

For Central Asia, this capability is transformative. The region contains archaeological sites threatened by erosion, irrigation networks requiring precise monitoring, mountain glaciers with complex surface texture, and agricultural zones needing crop health assessment at sub-field resolution.

<div class="info-box tip">
  <strong>Key idea</strong>
  Drones bridge the resolution gap. Satellites see patterns across landscapes; ground instruments see details at points. UAVs capture the intermediate scale where many management decisions are made: individual field parcels, building footprints, erosion gullies, and archaeological features.
</div>

## UAV platforms and sensors

### Multirotor versus fixed-wing

**Multirotor UAVs** (quadcopters, hexacopters) offer vertical takeoff and landing, precise hovering, and the ability to operate in confined spaces. Flight times typically range from 20 to 45 minutes. They are ideal for small sites requiring high overlap and flexibility.

**Fixed-wing UAVs** cover larger areas more efficiently. With flight times exceeding 60 minutes and higher cruise speeds, they can survey hundreds of hectares per flight. However, they require open areas for launch and recovery and cannot hover over specific targets.

The choice depends on site characteristics. For a 2-hectare archaeological excavation in Tajikistan, a multirotor provides flexibility. For mapping 500 hectares of irrigated land in Uzbekistan, a fixed-wing is more practical.

### Sensor types

**RGB cameras** capture standard color imagery. When processed through photogrammetry software, overlapping RGB images produce orthomosaics and digital surface models (DSMs). Entry-level systems use consumer cameras (20-24 MP); professional systems use metric cameras with calibrated optics.

**Multispectral sensors** capture discrete spectral bands (typically 4-10 bands covering visible through near-infrared). These enable vegetation indices like NDVI:

$$
\text{NDVI} = \frac{\rho_{NIR} - \rho_{Red}}{\rho_{NIR} + \rho_{Red}}
$$

where $\rho$ represents reflectance in each band.

**LiDAR sensors** emit laser pulses and measure return times to calculate distances. Modern UAV LiDAR systems achieve point densities exceeding 100 points per square meter, with vertical accuracy better than 5 cm. Unlike photogrammetry, LiDAR penetrates vegetation canopy, making it essential for terrain mapping in forested or vegetated areas.

## Photogrammetry principles

### Structure from Motion (SfM)

Structure from Motion reconstructs 3D geometry from 2D images taken from different viewpoints. The fundamental principle is triangulation: if the same point appears in multiple images with known camera positions, its 3D coordinates can be calculated.

The collinearity equations relate image coordinates $(x, y)$ to ground coordinates $(X, Y, Z)$:

$$
x = x_0 - f \frac{r_{11}(X - X_c) + r_{12}(Y - Y_c) + r_{13}(Z - Z_c)}{r_{31}(X - X_c) + r_{32}(Y - Y_c) + r_{33}(Z - Z_c)}
$$

$$
y = y_0 - f \frac{r_{21}(X - X_c) + r_{22}(Y - Y_c) + r_{23}(Z - Z_c)}{r_{31}(X - X_c) + r_{32}(Y - Y_c) + r_{33}(Z - Z_c)}
$$

where $f$ is focal length, $(x_0, y_0)$ is the principal point, $(X_c, Y_c, Z_c)$ is the camera position, and $r_{ij}$ are elements of the rotation matrix.

### Bundle adjustment

Bundle adjustment optimizes camera positions and 3D point coordinates by minimizing reprojection error:

$$
\min_{\mathbf{c}, \mathbf{p}} \sum_{i=1}^{n} \sum_{j=1}^{m} v_{ij} \| \mathbf{x}_{ij} - \pi(\mathbf{c}_i, \mathbf{p}_j) \|^2
$$

where $\mathbf{c}_i$ are camera parameters, $\mathbf{p}_j$ are point coordinates, and $v_{ij}$ indicates visibility.

### Ground Control Points (GCPs)

GCPs are surveyed targets with known coordinates that anchor the model to an absolute reference frame. A minimum of 5 well-distributed GCPs is recommended, placed at corners and center of the survey area.

<div class="info-box tip">
  <strong>Key idea</strong>
  GCPs transform relative accuracy into absolute accuracy. SfM produces internally consistent models, but GCPs anchor those models to real-world coordinates with surveyed precision.
</div>

## LiDAR fundamentals

### Pulse returns and waveform

A LiDAR pulse travels to the ground, reflects, and returns to the sensor. The time-of-flight $\Delta t$ determines range:

$$
R = \frac{c \cdot \Delta t}{2}
$$

where $c \approx 3 \times 10^8$ m/s is the speed of light.

In vegetated areas, a single pulse may produce multiple returns: first return from canopy top, intermediate returns from branches, and last return from ground. This multi-return capability enables terrain extraction beneath vegetation.

### Point density and coverage

Point density depends on pulse repetition frequency (PRF), scan rate, flight altitude, and speed. For a swath width $W$, ground speed $v$, and PRF $f$:

$$
\rho \approx \frac{f}{v \cdot W}
$$

Typical UAV LiDAR achieves 50-400 points/m². Higher density resolves finer features but increases data volume and processing time.

### Point classification

Raw point clouds are classified into ground, vegetation, buildings, and noise. Ground classification enables Digital Terrain Model extraction.

## Flight planning

### Overlap requirements

Standard recommendations: forward overlap 75-85%, side overlap 65-75%. For challenging terrain, increase to 85% forward and 80% side.

### Ground Sample Distance (GSD)

GSD is the ground dimension represented by one pixel:

$$
\text{GSD} = \frac{H \cdot S_w}{f \cdot I_w}
$$

where $H$ is flight altitude above ground, $S_w$ is sensor width, $f$ is focal length, and $I_w$ is image width in pixels.

For a camera with 35mm sensor width, 35mm focal length, and 6000 pixels across, flying at 100m:

$$
\text{GSD} = \frac{100 \times 0.035}{0.035 \times 6000} = \frac{3.5}{210} \approx 0.0167\ \text{m} = 1.67\ \text{cm}
$$

### Flight line spacing

Given side overlap percentage $p_{side}$, image width $W_{img}$, and GSD:

$$
\text{Spacing} = W_{img} \times \text{GSD} \times (1 - p_{side})
$$

For 6000-pixel width, 1.67 cm GSD, and 70% overlap:

$$
\text{Spacing} = 6000 \times 0.0167 \times 0.30 = 30.1\ \text{m}
$$

### Area coverage calculation

For a rectangular area $A$, flight line spacing $s$, ground speed $v$, and turn time $t_{turn}$:

$$
T_{flight} = \frac{A}{s \cdot v} + n_{lines} \cdot t_{turn}
$$

This helps estimate whether a site can be covered in a single battery or requires multiple flights.

## Processing workflows

### Image alignment and sparse cloud

Feature matching across images and bundle adjustment produces camera positions, sparse point cloud, and calibration refinement. Target reprojection error: < 1 pixel.

### Dense point cloud and mesh

Multi-view stereo densifies the sparse cloud (100-1000 pts/m²). Triangulated meshes with texture mapping create photorealistic 3D models.

### DSM and DTM

The DSM represents the top visible surface. The DTM represents bare-earth terrain:

$$
\text{Canopy Height Model} = \text{DSM} - \text{DTM}
$$

### Orthomosaic generation

Orthomosaics are geometrically corrected, seamlessly mosaicked aerial images. Each pixel is orthorectified using the DSM:

$$
X = X_c + (Z - Z_c) \cdot \frac{x - x_0}{f}
$$

The result is a map-accurate image where measurements can be made directly.

## Central Asia applications

### Archaeological site mapping

Central Asia's Silk Road heritage includes cities, caravanserais, and burial mounds. UAV survey enables micro-topographic mapping, detection of subsurface architecture through vegetation marks, and 3D recording of threatened sites. For kurgan documentation in Kazakhstan, 2-3 cm GSD reveals construction details invisible in satellite imagery.

### Erosion monitoring

Loess deposits across Kazakhstan, Tajikistan, and Kyrgyzstan are highly erodible. Repeat UAV surveys quantify erosion rates:

$$
\text{Volume change} = \int \int (Z_{t2} - Z_{t1})\ dx\ dy
$$

Multi-temporal DSM differencing at centimeter precision detects gully headcut advance, sheet erosion, and mass wasting.

### Agricultural monitoring

Cotton, wheat, and rice cultivation in Uzbekistan and Turkmenistan benefits from precision agriculture. UAV multispectral data provides:

- Within-field variability mapping
- Irrigation uniformity assessment
- Crop stress detection before visible symptoms
- Yield prediction models

The coefficient of variation (CV) of NDVI within a field indicates management zone potential:

$$
\text{CV} = \frac{\sigma_{NDVI}}{\mu_{NDVI}} \times 100\%
$$

### Glacier and permafrost monitoring

Tien Shan and Pamir glaciers are retreating. UAV surveys map surface velocity using feature tracking between epochs:

$$
v = \frac{\sqrt{\Delta x^2 + \Delta y^2}}{\Delta t}
$$

where $\Delta x$, $\Delta y$ are feature displacements and $\Delta t$ is the time interval.

LiDAR excels for debris-covered glaciers where photogrammetry struggles with texture-poor ice.

### Infrastructure inspection

Pipeline corridors and road networks require inspection. UAV surveys provide right-of-way monitoring, subsidence detection, and damage assessment after seismic events.

## Accuracy assessment

### Horizontal and vertical accuracy metrics

Root Mean Square Error (RMSE) quantifies accuracy against independent check points:

$$
\text{RMSE}_z = \sqrt{\frac{\sum_{i=1}^{n}(Z_{UAV,i} - Z_{check,i})^2}{n}}
$$

Similar formulas apply for horizontal coordinates. Typical UAV photogrammetry achieves:

- Horizontal RMSE: 1-3 × GSD
- Vertical RMSE: 1-2 × GSD (with GCPs)

### Point cloud quality metrics

For LiDAR, key metrics include:

- Point density (pts/m²)
- Vertical accuracy (RMSE against surveyed points)
- Completeness (coverage percentage)
- Ground classification accuracy

### Validation strategies

Reserve 20-30% of surveyed points as independent check points. Compare UAV products against RTK GNSS surveys or total station measurements.

<div class="info-box tip">
  <strong>Key idea</strong>
  Accuracy without validation is assumption. Always reserve independent check points and report quantitative metrics.
</div>

## Integration with satellite data

### Sentinel-2 fusion

Sentinel-2 provides 10-20m multispectral imagery every 5 days. UAV data calibrates Sentinel-derived indices and validates land cover classifications. For vegetation monitoring:

$$
\text{LAI} = a \cdot \text{NDVI}_{S2}^b + c
$$

### Sentinel-1 SAR integration

SAR complements optical UAV data by providing temporal continuity during cloudy periods and detecting moisture patterns invisible to optical sensors.

### Scale bridging approach

A hierarchical framework uses each data source appropriately:

| Scale | Source | Resolution | Temporal frequency |
|-------|--------|------------|-------------------|
| Regional | Sentinel-1/2 | 10-20 m | 5-12 days |
| Local | UAV RGB/MS | 1-5 cm | Campaign-based |
| Point | Ground survey | mm-cm | As needed |

## Checklist for UAV survey planning

<ul class="checklist">
  <li>Define survey objectives and required accuracy</li>
  <li>Select appropriate platform (multirotor vs fixed-wing)</li>
  <li>Choose sensor based on deliverables (RGB, multispectral, LiDAR)</li>
  <li>Calculate required GSD and flight altitude</li>
  <li>Plan flight lines with adequate overlap (75-85% forward, 65-75% side)</li>
  <li>Design GCP network (minimum 5 points, distributed across site)</li>
  <li>Survey GCP positions with RTK GNSS</li>
  <li>Check weather conditions (wind < 10 m/s, no precipitation)</li>
  <li>Verify airspace regulations and obtain permits</li>
  <li>Conduct pre-flight system checks</li>
  <li>Fly mission and verify image capture</li>
  <li>Download and backup raw data immediately</li>
  <li>Process through photogrammetry/LiDAR software</li>
  <li>Validate products against check points</li>
  <li>Document metadata and accuracy metrics</li>
</ul>

## Exercises

<div class="exercise-container">
  <h3>Exercise 1</h3>
  <p>A quadcopter carries a camera with 24mm focal length and a sensor measuring 23.5mm × 15.6mm at 6000 × 4000 pixels. Calculate the GSD when flying at 80m altitude.</p>
  <details>
    <summary>Solution</summary>
    <p>GSD = (H × S_w) / (f × I_w) = (80 × 0.0235) / (0.024 × 6000) = 1.88 / 144 = 0.013 m = <strong>1.3 cm</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 2</h3>
  <p>For the camera in Exercise 1, what flight altitude achieves 2.5 cm GSD?</p>
  <details>
    <summary>Solution</summary>
    <p>H = (GSD × f × I_w) / S_w = (0.025 × 0.024 × 6000) / 0.0235 = 3.6 / 0.0235 = <strong>153 m</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 3</h3>
  <p>Given 80% forward overlap and 6000-pixel image width at 1.5 cm GSD, calculate the distance between consecutive image centers.</p>
  <details>
    <summary>Solution</summary>
    <p>Image ground width = 6000 × 0.015 = 90 m. Distance = 90 × (1 - 0.80) = 90 × 0.20 = <strong>18 m</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 4</h3>
  <p>A UAV flies at 8 m/s with 18m spacing between image centers. What is the required photo interval?</p>
  <details>
    <summary>Solution</summary>
    <p>Interval = Distance / Speed = 18 / 8 = <strong>2.25 seconds</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 5</h3>
  <p>A LiDAR pulse takes 0.67 microseconds for round-trip travel. What is the range to the target?</p>
  <details>
    <summary>Solution</summary>
    <p>R = (c × Δt) / 2 = (3×10⁸ × 0.67×10⁻⁶) / 2 = 201 / 2 = <strong>100.5 m</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 6</h3>
  <p>A LiDAR system has PRF of 300 kHz, scan angle ±30°, and flies at 50m altitude at 5 m/s. Estimate the point density.</p>
  <details>
    <summary>Solution</summary>
    <p>Swath width W = 2 × 50 × tan(30°) = 2 × 50 × 0.577 = 57.7 m. Density ρ ≈ PRF / (v × W) = 300,000 / (5 × 57.7) = 300,000 / 288.5 ≈ <strong>1040 pts/m²</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 7</h3>
  <p>Five check points have vertical errors of +3, -2, +5, -1, +4 cm. Calculate the RMSE.</p>
  <details>
    <summary>Solution</summary>
    <p>RMSE = √[(9 + 4 + 25 + 1 + 16) / 5] = √[55/5] = √11 = <strong>3.32 cm</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 8</h3>
  <p>A 10-hectare site needs coverage at 70% side overlap with 35m swath width. How many flight lines are required?</p>
  <details>
    <summary>Solution</summary>
    <p>For a square 10 ha site: √100,000 ≈ 316 m side. Line spacing = 35 × (1-0.70) = 10.5 m. Lines = 316/10.5 + 1 ≈ <strong>31 lines</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 9</h3>
  <p>DSM elevation at a point is 1523.4 m; DTM elevation is 1518.7 m. What is the canopy height?</p>
  <details>
    <summary>Solution</summary>
    <p>Canopy Height = DSM - DTM = 1523.4 - 1518.7 = <strong>4.7 m</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 10</h3>
  <p>A UAV battery allows 25 minutes of flight. With 10% reserve and 2-minute takeoff/landing, how much survey time is available?</p>
  <details>
    <summary>Solution</summary>
    <p>Reserve = 25 × 0.10 = 2.5 min. Survey time = 25 - 2.5 - 2 = <strong>20.5 minutes</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 11</h3>
  <p>An orthomosaic covers 15 hectares at 2 cm GSD. How many pixels does it contain (assuming square extent)?</p>
  <details>
    <summary>Solution</summary>
    <p>Area = 150,000 m². Pixels per m² = (1/0.02)² = 2500. Total = 150,000 × 2500 = <strong>375 million pixels</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 12</h3>
  <p>NDVI values in a field range from 0.45 to 0.78 with mean 0.62 and standard deviation 0.08. Calculate the coefficient of variation.</p>
  <details>
    <summary>Solution</summary>
    <p>CV = (σ / μ) × 100% = (0.08 / 0.62) × 100% = <strong>12.9%</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 13</h3>
  <p>A glacier feature moves 4.2 m east and 1.8 m south over 30 days. What is the velocity in m/year?</p>
  <details>
    <summary>Solution</summary>
    <p>Displacement = √(4.2² + 1.8²) = √(17.64 + 3.24) = √20.88 = 4.57 m in 30 days. Annual = 4.57 × (365/30) = <strong>55.6 m/year</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 14</h3>
  <p>Two DTMs covering 500 m² show mean elevation change of -0.12 m. What volume of material was lost?</p>
  <details>
    <summary>Solution</summary>
    <p>Volume = Area × Mean change = 500 × 0.12 = <strong>60 m³</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 15</h3>
  <p>A photogrammetric solution reports 0.8 pixel reprojection error with 3 cm GSD. What is the ground-level error?</p>
  <details>
    <summary>Solution</summary>
    <p>Ground error = 0.8 × 3 = <strong>2.4 cm</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 16</h3>
  <p>A multispectral camera records NIR reflectance of 0.42 and Red reflectance of 0.08. Calculate NDVI.</p>
  <details>
    <summary>Solution</summary>
    <p>NDVI = (0.42 - 0.08) / (0.42 + 0.08) = 0.34 / 0.50 = <strong>0.68</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 17</h3>
  <p>Bundle adjustment across 450 images with 125,000 tie points. How many observation equations?</p>
  <details>
    <summary>Solution</summary>
    <p>~3 images per point × 2 equations: 125,000 × 3 × 2 = <strong>750,000 equations</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 18</h3>
  <p>GCPs have ±2 cm accuracy. With 1.5× GSD vertical accuracy and 2.5 cm GSD, what is expected RMSE?</p>
  <details>
    <summary>Solution</summary>
    <p>√(3.75² + 2²) ≈ <strong>4.25 cm</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 19</h3>
  <p>Fixed-wing covers 120 ha in 45 min at 12 m/s. What is effective swath width?</p>
  <details>
    <summary>Solution</summary>
    <p>Distance = 32,400 m. Swath = 1,200,000/32,400 = <strong>37 m</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 20</h3>
  <p>Point cloud: 85 pts/m² over 2 ha. Total point count?</p>
  <details>
    <summary>Solution</summary>
    <p>85 × 20,000 = <strong>1.7 million points</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 21</h3>
  <p>Classification: 9,450 correct ground points of 10,000; 320 vegetation misclassified. Calculate precision and recall.</p>
  <details>
    <summary>Solution</summary>
    <p>Recall = <strong>94.5%</strong>. Precision = 9450/9770 = <strong>96.7%</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 22</h3>
  <p>LAI = 2.8 × NDVI^1.4 - 0.3. For NDVI = 0.55, predict LAI.</p>
  <details>
    <summary>Solution</summary>
    <p>2.8 × 0.435 - 0.3 = <strong>0.92</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 23</h3>
  <p>Wind 7 m/s, quadcopter max 15 m/s. Ground speed into vs with wind?</p>
  <details>
    <summary>Solution</summary>
    <p>Into: <strong>8 m/s</strong>. With: <strong>22 m/s</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 24</h3>
  <p>Mound rises 1.2 m, RMSE = 3 cm. Is it detectable?</p>
  <details>
    <summary>Solution</summary>
    <p>SNR = 1.2/0.03 = 40. <strong>Easily detectable</strong>.</p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 25</h3>
  <p>Processing 800 images takes 6 hours. Estimate time for 1200 images.</p>
  <details>
    <summary>Solution</summary>
    <p>Scaling ~1.5×: <strong>~9 hours</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 26</h3>
  <p>5 million triangles, 4 cm² average area. Ground coverage?</p>
  <details>
    <summary>Solution</summary>
    <p>5M × 4 = 20M cm² = <strong>0.2 hectares</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 27</h3>
  <p>RTK: 1.5 cm horizontal accuracy. How many GCPs for 3 cm orthomosaic RMSE?</p>
  <details>
    <summary>Solution</summary>
    <p>With margin available, <strong>5 GCPs</strong> minimum for distribution.</p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 28</h3>
  <p>NDVI: healthy 0.72, stressed 0.51. Percentage difference?</p>
  <details>
    <summary>Solution</summary>
    <p>(0.72-0.51)/0.72 = <strong>29.2% reduction</strong></p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 29</h3>
  <p>A kurgan (burial mound) has diameter 25 m. What minimum GSD resolves features at 1/10 of the mound diameter?</p>
  <details>
    <summary>Solution</summary>
    <p>Target resolution = 25/10 = 2.5 m. To resolve, GSD should be ≤ 2.5/3 ≈ <strong>0.8 m</strong> (Nyquist), but for clear mapping, <strong>< 25 cm</strong> recommended.</p>
  </details>
</div>

<div class="exercise-container">
  <h3>Exercise 30</h3>
  <p>Integrating Sentinel-1 backscatter (σ⁰ = -12 dB) with UAV-derived surface roughness (RMS height = 2.3 cm), estimate if the surface appears rough or smooth to C-band radar (λ = 5.6 cm).</p>
  <details>
    <summary>Solution</summary>
    <p>Rayleigh criterion: h > λ/(8 cos θ). For θ = 35°: threshold = 5.6/(8 × 0.82) = 0.85 cm. Since 2.3 > 0.85, surface is <strong>rough</strong> to C-band.</p>
  </details>
</div>

## Further reading

- Colomina, I., & Molina, P. (2014). Unmanned aerial systems for photogrammetry and remote sensing. *ISPRS JPRS*, 92, 79-97.
- Westoby, M. J., et al. (2012). Structure-from-Motion photogrammetry. *Geomorphology*, 179, 300-314.
