---
layout: tutorial
title: "How Digital Elevation Models Reveal Terrain Structure in Central Asia"
description: "A math-first tutorial on DEM sources, terrain derivatives, hydrological analysis, and geomorphometry for Central Asia, with 30 exercises and solutions."
difficulty: intermediate
estimated_time: "2-3 hours"
series: sar-remote-sensing
order: 11
---

## Why this tutorial matters

Every process on Earth's surface responds to gravity, and gravity follows terrain. Water flows downhill, sediments accumulate in valleys, and human settlements cluster on accessible slopes. Central Asia presents terrain complexity at every scale—the Tian Shan exceeds 7,000 meters while the Aral Sea depression drops below 50 meters.

<div class="info-box tip">
  <strong>Key idea</strong>
  Terrain is the template. Before analyzing vegetation indices or mapping land cover, understand the surface that shapes all patterns. A DEM is your first layer, not an afterthought.
</div>

Digital Elevation Models encode terrain numerically, enabling quantitative analysis. This tutorial develops terrain analysis mathematics for Central Asia's diverse landscapes.

## DEM types: DSM vs DTM vs DEM

A **Digital Elevation Model (DEM)** is a gridded elevation representation. More specific terms:

- **Digital Surface Model (DSM)**: Elevation including buildings and vegetation canopy
- **Digital Terrain Model (DTM)**: Bare earth surface, vegetation removed

The relationship: $\text{DSM} = \text{DTM} + h_{features}$

**Raster DEMs** store elevation as regular grids: $Z = f(i, j)$. **TINs** use triangular facets: $z(x, y) = ax + by + c$ within each triangle. TINs adapt to complexity; rasters dominate for computational simplicity.

<div class="info-box tip">
  <strong>Key idea</strong>
  Choose DSM for surface features (urban, canopy). Choose DTM for hydrology and landslides. Most global DEMs are DSMs.
</div>

## DEM sources for Central Asia

**SRTM**: The 2000 mission produced near-global DEMs via C-band InSAR. SRTM1 offers ~30 m resolution; SRTM3 ~90 m. Vertical accuracy: 16 m absolute, <10 m relative.

**ASTER GDEM**: Optical stereo from NASA's Terra satellite. Version 3 provides 1 arc-second coverage with ~8.5 m RMSE.

**Copernicus DEM**: From TanDEM-X interferometry. GLO-30 (30 m) offers <4 m accuracy—the current gold standard for free global DEMs.

**ALOS World 3D**: Japanese optical stereo DEM. Free 30 m version available; ~5 m RMSE.

**Local surveys**: Central Asian national datasets from aerial surveys may offer superior local accuracy but often have restricted access.

## Resolution, accuracy, and vertical datums

Resolution determines the minimum feature size detectable. Features smaller than $3\Delta x$ cannot be reliably resolved—at 30 m resolution, gullies narrower than 90 m disappear.

**Vertical accuracy** has two components: absolute (closeness to true elevation) and relative (accuracy of elevation differences). For derivatives like slope, relative accuracy matters more.

**Vertical datums**: Elevations reference either geoid models (EGM96/EGM2008) or the WGS84 ellipsoid. The conversion: $h_{orthometric} = h_{ellipsoidal} - N$ where $N$ is geoid undulation (0 to -45 m in Central Asia).

<div class="info-box tip">
  <strong>Key idea</strong>
  Always verify vertical datum before combining DEMs. A 30 m offset appears as a step boundary in mosaicked data.
</div>

## Terrain derivatives

### Slope

Slope uses finite differences:

$$
\frac{\partial Z}{\partial x} = \frac{Z_{i+1,j} - Z_{i-1,j}}{2\Delta x}, \quad \frac{\partial Z}{\partial y} = \frac{Z_{i,j+1} - Z_{i,j-1}}{2\Delta y}
$$

$$
\text{slope} = \arctan\left(\sqrt{\left(\frac{\partial Z}{\partial x}\right)^2 + \left(\frac{\partial Z}{\partial y}\right)^2}\right)
$$

### Aspect

Downslope direction, clockwise from north: $\text{aspect} = \arctan2\left(-\frac{\partial Z}{\partial x}, \frac{\partial Z}{\partial y}\right)$

### Curvature

**Profile curvature** (steepest descent): $\kappa_p = \frac{-f_{xx}f_x^2 - 2f_{xy}f_x f_y - f_{yy}f_y^2}{(f_x^2 + f_y^2)\sqrt{(1 + f_x^2 + f_y^2)^3}}$

**Plan curvature**: $\kappa_c = \frac{f_{xx}f_y^2 - 2f_{xy}f_x f_y + f_{yy}f_x^2}{(f_x^2 + f_y^2)^{3/2}}$

Positive plan curvature = convergent terrain (valleys); negative = divergent (ridges).

### Hillshade

Illumination simulation: $I = \cos(\theta_z) \cos(\text{slope}) + \sin(\theta_z) \sin(\text{slope}) \cos(\theta_a - \text{aspect})$

### TPI and TRI

**Topographic Position Index**: $\text{TPI} = Z_0 - \bar{Z}_{neighborhood}$ (positive = ridges, negative = valleys)

**Terrain Ruggedness Index**: $\text{TRI} = \frac{1}{8}\sum_{k=1}^{8}|Z_0 - Z_k|$

## Hydrological analysis

### Flow direction

D8 algorithm assigns flow to steepest neighbor:

$$
\text{flow direction} = \arg\max_k \frac{Z_0 - Z_k}{d_k}
$$

### Flow accumulation

Counts upstream cells: $A_i = 1 + \sum_{j \in \text{upslope}(i)} A_j$

High values identify streams. Drainage density: $\approx \frac{1}{\sqrt{T \cdot \Delta x^2}}$

### Watershed and stream extraction

Watersheds propagate upstream from pour points. Streams form where $A > T$ (constant threshold) or $A \cdot S^k > T$ (slope-area).

<div class="info-box tip">
  <strong>Key idea</strong>
  Fill sinks before hydrological analysis—local minima trap flow artificially.
</div>

## Viewshed and visibility analysis

Viewshed identifies visible cells from an observer. Target $(x_t, y_t, z_t)$ is visible if line-of-sight clears intervening terrain:

$$
\frac{z_o + h - z_i}{d_{o,i}} > \frac{z_o + h - z_t}{d_{o,t}} \quad \forall i \text{ between } o \text{ and } t
$$

**Cumulative viewshed** counts observer points seeing each cell. **Intervisibility** tests mutual visibility for communication planning.

## Central Asia applications

### Landslide susceptibility

$P(\text{landslide}) = f(\text{slope}, \text{curvature}, \text{aspect}, \text{TWI}, \text{geology})$

Topographic Wetness Index: $\text{TWI} = \ln\left(\frac{A}{\tan(\text{slope})}\right)$

Central Asia's loess plateaus show slope thresholds around 25-35° for triggering.

### Irrigation planning

Traditional gravity irrigation follows contours at 0.1-0.5% gradient. DEM identifies command areas, optimal canal routes, and drainage problem zones.

### Archaeological detection

Tells rise 2-20 m above plains; collapsed qanats appear as linear depressions. Multi-angle hillshade reveals subtle signatures.

### Glacier monitoring

Glaciers occupy elevations above ~4,000 m ELA, slopes <45°, and north-facing aspects in northern Tian Shan.

## DEM preprocessing

### Void filling

DEMs contain voids (no-data cells) from radar shadow, water bodies, or processing failures. Filling approaches include:

**Interpolation**: Fill small voids from neighboring valid cells
**Auxiliary DEM**: Substitute values from another DEM source
**Morphological**: Extend surrounding terrain patterns into voids

### Smoothing

Noise filtering reduces spurious terrain detail. Common filters:

**Mean filter**: $Z_{smoothed} = \frac{1}{n}\sum Z_i$ (simple, blurs edges)

**Gaussian filter**: Weight by distance with $\sigma$ controlling smoothness

**Edge-preserving**: Retain steep slopes while smoothing flat areas

### Mosaicking

Combining multiple DEM tiles requires attention to:

- Edge matching at tile boundaries
- Datum consistency
- Resolution resampling if mixing sources

$$
Z_{mosaic}(x,y) = w_1(x,y)Z_1 + w_2(x,y)Z_2
$$

where weights $w$ blend seamlessly across overlap zones.

<div class="info-box tip">
  <strong>Key idea</strong>
  Preprocessing choices propagate to all derivatives. Aggressive smoothing eliminates real terrain features; insufficient smoothing creates spurious drainage patterns. Validate against independent data when possible.
</div>

## Integration with SAR and optical data

### Terrain correction for SAR

SAR geometry distorts terrain. Geocoding requires a DEM to project slant-range observations to ground coordinates. The transformation uses:

$$
R = \sqrt{(X_s - X_t)^2 + (Y_s - Y_t)^2 + (Z_s - Z_t)^2}
$$

where $(X_s, Y_s, Z_s)$ is satellite position and $(X_t, Y_t, Z_t)$ is the ground target from the DEM.

### Radiometric terrain correction

Terrain slope and aspect affect radar brightness. Local incidence angle $\theta_{loc}$ differs from scene incidence angle:

$$
\cos(\theta_{loc}) = \cos(\theta_i)\cos(\text{slope}) + \sin(\theta_i)\sin(\text{slope})\cos(\phi - \text{aspect})
$$

where $\phi$ is the satellite look direction azimuth. Radiometric terrain correction normalizes brightness variations caused by topography.

### Optical terrain normalization

In optical imagery, terrain affects both illumination (shade) and viewing geometry. Topographic correction removes illumination effects:

$$
\rho_{corrected} = \rho_{observed} \cdot \frac{\cos(\theta_z)}{\cos(\theta_i)}
$$

where $\theta_z$ is solar zenith and $\theta_i$ is local illumination angle from DEM.

### Multi-temporal DEM differencing

Subtracting DEMs from different epochs reveals surface change:

$$
\Delta h = Z_{t2} - Z_{t1}
$$

Applications include glacier mass balance, landslide volume, and subsidence monitoring. Co-registration accuracy must exceed expected change magnitudes.

## Checklist

<ul class="checklist">
  <li>Identified appropriate DEM source for analysis requirements</li>
  <li>Verified vertical datum and documented coordinate reference system</li>
  <li>Checked for voids and applied appropriate filling method</li>
  <li>Assessed resolution adequacy for target features</li>
  <li>Computed slope and aspect for terrain characterization</li>
  <li>Generated hillshade for visual interpretation</li>
  <li>Filled sinks before hydrological analysis</li>
  <li>Calculated flow direction and accumulation grids</li>
  <li>Extracted stream network with justified threshold</li>
  <li>Delineated watersheds for study area</li>
  <li>Applied terrain correction to SAR or optical data</li>
  <li>Documented preprocessing steps for reproducibility</li>
</ul>

## Exercises

<details class="exercise" id="exercise-1">
  <summary>Exercise 1: Slope from elevation grid</summary>
  <div class="exercise-content">
    <p>Given a 3×3 DEM (meters) with 10 m cell size:</p>
    <pre>
    120  125  130
    118  123  128
    115  120  126
    </pre>
    <p>Calculate the slope at the center cell in degrees.</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>Using finite differences:
      $$\frac{\partial Z}{\partial x} = \frac{128 - 118}{2 \times 10} = 0.5$$
      $$\frac{\partial Z}{\partial y} = \frac{125 - 120}{2 \times 10} = 0.25$$
      $$\text{slope} = \arctan(\sqrt{0.5^2 + 0.25^2}) = \arctan(0.559) = 29.2°$$</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-2">
  <summary>Exercise 2: Aspect calculation</summary>
  <div class="exercise-content">
    <p>Using the same DEM from Exercise 1, calculate the aspect at the center cell.</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>$$\text{aspect} = \arctan2(-0.5, 0.25) = \arctan2(-0.5, 0.25)$$
      This gives approximately 296.6° (west-northwest), indicating flow toward the west-northwest.</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-3">
  <summary>Exercise 3: Resolution and feature detection</summary>
  <div class="exercise-content">
    <p>An irrigation canal is 15 m wide. What DEM resolution is needed to reliably detect it?</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>Using the 3× rule: features need ~3 cells to be reliably resolved.
      $$\Delta x \leq \frac{15}{3} = 5 \text{ m}$$
      A 5 m or finer resolution DEM is required.</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-4">
  <summary>Exercise 4: Vertical datum conversion</summary>
  <div class="exercise-content">
    <p>A point has ellipsoidal height 3,450 m (WGS84). The local geoid undulation is -32 m. What is the orthometric height?</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>$$h_{orthometric} = h_{ellipsoidal} - N = 3450 - (-32) = 3482 \text{ m}$$</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-5">
  <summary>Exercise 5: TPI interpretation</summary>
  <div class="exercise-content">
    <p>A cell has elevation 2,340 m. The mean elevation of cells within 500 m radius is 2,285 m. Calculate and interpret the TPI.</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>$$\text{TPI} = 2340 - 2285 = +55 \text{ m}$$
      Positive TPI indicates the cell is elevated relative to surroundings—likely a ridge or hilltop.</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-6">
  <summary>Exercise 6: Flow accumulation threshold</summary>
  <div class="exercise-content">
    <p>A 30 m DEM covers a watershed. You want streams with approximately 1 km² contributing area. What flow accumulation threshold should you use?</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>Each cell represents $30 \times 30 = 900$ m².
      $$T = \frac{1,000,000 \text{ m}^2}{900 \text{ m}^2/\text{cell}} \approx 1111 \text{ cells}$$
      Use a threshold of approximately 1,100 cells.</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-7">
  <summary>Exercise 7: Hillshade calculation</summary>
  <div class="exercise-content">
    <p>Calculate hillshade intensity for a cell with slope 25° and aspect 180° (south-facing), illuminated from azimuth 315° (northwest) at zenith 45°.</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>$$I = \cos(45°)\cos(25°) + \sin(45°)\sin(25°)\cos(315° - 180°)$$
      $$I = 0.707 \times 0.906 + 0.707 \times 0.423 \times \cos(135°)$$
      $$I = 0.641 + 0.299 \times (-0.707) = 0.641 - 0.211 = 0.430$$</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-8">
  <summary>Exercise 8: TWI calculation</summary>
  <div class="exercise-content">
    <p>A cell has flow accumulation of 500 cells (30 m resolution) and slope of 8°. Calculate the Topographic Wetness Index.</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>Contributing area: $A = 500 \times 900 = 450,000$ m²
      $$\text{TWI} = \ln\left(\frac{450000}{\tan(8°)}\right) = \ln\left(\frac{450000}{0.141}\right) = \ln(3,191,489) = 14.98$$</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-9">
  <summary>Exercise 9: Landslide slope threshold</summary>
  <div class="exercise-content">
    <p>In a study area, 80% of historical landslides occurred on slopes between 25° and 45°. If a slope map shows 15% of the area in this range, what is the landslide density ratio?</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>Density ratio = (% of landslides in class) / (% of area in class)
      $$\text{Density ratio} = \frac{80\%}{15\%} = 5.33$$
      Slopes 25-45° are 5.3× more susceptible than average.</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-10">
  <summary>Exercise 10: Viewshed line-of-sight</summary>
  <div class="exercise-content">
    <p>An observer at elevation 1,200 m with 2 m eye height looks toward a point 3 km away at elevation 1,180 m. Between them, a ridge at 1.5 km distance has elevation 1,210 m. Is the target visible?</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>Observer effective elevation: 1,202 m
      Angle to ridge: $\arctan((1210-1202)/1500) = 0.306°$
      Angle to target: $\arctan((1180-1202)/3000) = -0.42°$
      The ridge angle (0.306°) exceeds the target angle (-0.42°), so the ridge blocks the view. Target is NOT visible.</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-11">
  <summary>Exercise 11: DEM accuracy assessment</summary>
  <div class="exercise-content">
    <p>Five ground control points show DEM errors of: +3.2, -1.8, +2.5, -0.9, +1.4 meters. Calculate the RMSE.</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>$$\text{RMSE} = \sqrt{\frac{3.2^2 + 1.8^2 + 2.5^2 + 0.9^2 + 1.4^2}{5}}$$
      $$= \sqrt{\frac{10.24 + 3.24 + 6.25 + 0.81 + 1.96}{5}} = \sqrt{\frac{22.5}{5}} = \sqrt{4.5} = 2.12 \text{ m}$$</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-12">
  <summary>Exercise 12: D8 flow direction</summary>
  <div class="exercise-content">
    <p>For the center cell of this DEM (10 m cells), determine the D8 flow direction:</p>
    <pre>
    145  142  140
    148  144  139
    150  147  143
    </pre>
    <details class="solution">
      <summary>Solution</summary>
      <p>Calculate slopes to each neighbor:
      - NE: (144-140)/(10×√2) = 0.283
      - E: (144-139)/10 = 0.500
      - SE: (144-143)/(10×√2) = 0.071
      Maximum slope is toward E (0.500). Flow direction = East (90°).</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-13">
  <summary>Exercise 13: Curvature interpretation</summary>
  <div class="exercise-content">
    <p>A cell has profile curvature of -0.02 m⁻¹ and plan curvature of +0.03 m⁻¹. Describe the landform.</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>Negative profile curvature: convex slope (flow accelerates)
      Positive plan curvature: convergent terrain (flow concentrates)
      This describes a convex hillslope draining into a valley—typical of a headwall or upper valley slope where water gathers while flowing down an increasingly steep surface.</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-14">
  <summary>Exercise 14: Terrain Ruggedness Index</summary>
  <div class="exercise-content">
    <p>Calculate TRI for the center cell of this 3×3 neighborhood:</p>
    <pre>
    1250  1248  1253
    1245  1249  1256
    1242  1247  1251
    </pre>
    <details class="solution">
      <summary>Solution</summary>
      <p>$$\text{TRI} = \frac{|1249-1250|+|1249-1248|+|1249-1253|+|1249-1245|+|1249-1256|+|1249-1242|+|1249-1247|+|1249-1251|}{8}$$
      $$= \frac{1+1+4+4+7+7+2+2}{8} = \frac{28}{8} = 3.5 \text{ m}$$</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-15">
  <summary>Exercise 15: Glacier elevation threshold</summary>
  <div class="exercise-content">
    <p>The equilibrium line altitude (ELA) in the Tian Shan is 4,100 m. A DEM shows 12% of a catchment above this elevation. If the catchment is 850 km², what area potentially supports glacier accumulation?</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>$$A_{accumulation} = 0.12 \times 850 = 102 \text{ km}^2$$
      Note: Actual glacier extent depends also on slope, aspect, and precipitation patterns.</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-16">
  <summary>Exercise 16: Canal gradient design</summary>
  <div class="exercise-content">
    <p>An irrigation canal must drop 15 m over 5 km. What gradient is this in percent, and is it suitable for gravity flow?</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>$$\text{Gradient} = \frac{15 \text{ m}}{5000 \text{ m}} \times 100 = 0.3\%$$
      This is within the typical range (0.1-0.5%) for gravity-fed irrigation canals—suitable for efficient flow without excessive erosion.</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-17">
  <summary>Exercise 17: DEM differencing accuracy</summary>
  <div class="exercise-content">
    <p>Two DEMs have individual accuracies of 4 m and 5 m RMSE. What is the minimum detectable surface change?</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>Combined error (assuming independence):
      $$\sigma_{diff} = \sqrt{4^2 + 5^2} = \sqrt{41} = 6.4 \text{ m}$$
      Minimum detectable change ≈ 2σ = 12.8 m for 95% confidence.</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-18">
  <summary>Exercise 18: SAR terrain correction</summary>
  <div class="exercise-content">
    <p>A Sentinel-1 scene has 35° incidence angle. A slope faces the satellite at 20° inclination. Calculate the local incidence angle.</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>For a slope facing the satellite (aspect aligned with look direction):
      $$\theta_{local} = \theta_{incidence} - \text{slope} = 35° - 20° = 15°$$
      The effective incidence angle decreases, causing brighter radar returns (foreshortening effect).</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-19">
  <summary>Exercise 19: Watershed area calculation</summary>
  <div class="exercise-content">
    <p>A watershed delineation identifies 45,000 cells as draining to a pour point. The DEM has 30 m resolution. What is the watershed area in km²?</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>$$A = 45000 \times 30 \times 30 = 40,500,000 \text{ m}^2 = 40.5 \text{ km}^2$$</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-20">
  <summary>Exercise 20: Multi-scale TPI</summary>
  <div class="exercise-content">
    <p>A location has TPI = +25 m at 100 m radius and TPI = -150 m at 2 km radius. Interpret the landform position.</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>Positive local TPI: elevated relative to immediate surroundings (local ridge or hilltop)
      Negative regional TPI: depressed relative to broader landscape (within a valley)
      Interpretation: This is a local ridge or terrace within a larger valley system—common in Central Asian mountain valleys.</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-21">
  <summary>Exercise 21: Void filling decision</summary>
  <div class="exercise-content">
    <p>An SRTM DEM has a void over a steep mountain ridge. ASTER GDEM shows data but with high noise. What filling approach would you recommend?</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>Recommended approach: Use ASTER GDEM values with smoothing. Apply a Gaussian filter to reduce noise while preserving the ridge structure. Alternatively, use delta-surface fill: compute the difference between SRTM and ASTER in surrounding valid areas, then apply this correction to ASTER values in the void.</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-22">
  <summary>Exercise 22: Archaeological feature detection</summary>
  <div class="exercise-content">
    <p>A tell (ancient settlement mound) rises 8 m above the surrounding plain. Using a 30 m DEM with 3 m vertical accuracy, can you reliably detect it?</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>Signal-to-noise ratio: $8/3 = 2.67$
      This is marginally detectable but not highly reliable. The feature is less than 3σ above the noise floor. Detection would require multiple adjacent cells showing elevated values, or use of a higher-resolution DEM.</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-23">
  <summary>Exercise 23: Drainage density</summary>
  <div class="exercise-content">
    <p>A stream network extracted with threshold 500 cells (30 m DEM) totals 45 km in a 60 km² basin. Calculate the drainage density.</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>$$D_d = \frac{L}{A} = \frac{45 \text{ km}}{60 \text{ km}^2} = 0.75 \text{ km/km}^2$$
      This moderate density is typical of semi-arid regions with moderate slopes.</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-24">
  <summary>Exercise 24: Solar radiation and aspect</summary>
  <div class="exercise-content">
    <p>In the northern Tian Shan (42°N), a south-facing slope receives direct solar radiation for approximately 10 hours on summer solstice. If a north-facing slope of equal steepness receives only 3 hours, what is the ratio of daily insolation?</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>Assuming equal intensity during direct illumination:
      $$\text{Ratio} = \frac{10}{3} = 3.33$$
      South-facing slopes receive more than 3× the direct solar energy, explaining dramatic vegetation and snow pattern differences.</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-25">
  <summary>Exercise 25: Sink depth analysis</summary>
  <div class="exercise-content">
    <p>A DEM contains a sink with minimum elevation 1,845 m. The pour point (lowest outlet) is at 1,849 m. Is this likely a real feature or an artifact?</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>Sink depth: $1849 - 1845 = 4$ m
      For a DEM with typical 5-10 m accuracy, a 4 m sink is within the error range and likely an artifact. Fill this sink before hydrological analysis. Real closed basins (e.g., lakes) typically show much larger depths or independent confirmation.</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-26">
  <summary>Exercise 26: Command area delineation</summary>
  <div class="exercise-content">
    <p>An irrigation intake is at 1,100 m elevation. The canal grade is 0.2%. What elevation range can gravity irrigation serve within 10 km of the intake?</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>Elevation drop over 10 km: $10,000 \times 0.002 = 20$ m
      Minimum serviceable elevation: $1100 - 20 = 1080$ m
      Command area includes all terrain between 1,100 m (intake) and 1,080 m (10 km downstream at canal grade), constrained by topography allowing canal routing.</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-27">
  <summary>Exercise 27: Relief ratio</summary>
  <div class="exercise-content">
    <p>A watershed has maximum elevation 4,200 m, minimum 1,800 m, and basin length 35 km. Calculate the relief ratio and interpret.</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>$$R_r = \frac{H_{max} - H_{min}}{L} = \frac{4200 - 1800}{35000} = 0.069$$
      A relief ratio of 0.069 indicates a moderately steep basin. Values above 0.05 suggest significant erosion potential and rapid runoff response.</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-28">
  <summary>Exercise 28: DEM resampling</summary>
  <div class="exercise-content">
    <p>A 30 m SRTM DEM will be combined with 90 m Copernicus DEM. What resampling approach is appropriate, and what accuracy considerations apply?</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>Resample the 90 m DEM to 30 m using bilinear or cubic interpolation. However, this does not create true 30 m information—the effective resolution remains 90 m. For terrain derivatives, the coarser source will show smoother slopes. Weight decisions toward the higher-accuracy Copernicus DEM where both have valid data.</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-29">
  <summary>Exercise 29: Permafrost elevation modeling</summary>
  <div class="exercise-content">
    <p>Permafrost lower limit in the Tian Shan is approximately 3,400 m on north slopes and 3,800 m on south slopes. For a DEM-based map, how would you implement this aspect-dependent threshold?</p>
    <details class="solution">
      <summary>Solution</summary>
      <p>Create an aspect-adjusted threshold surface:
      $$Z_{threshold} = 3400 + 400 \times \frac{1 + \cos(\text{aspect})}{2}$$
      This varies linearly from 3,400 m (north, aspect=0°) to 3,800 m (south, aspect=180°). Classify as potential permafrost where DEM elevation exceeds this threshold.</p>
    </details>
  </div>
</details>

<details class="exercise" id="exercise-30">
  <summary>Exercise 30: Integrated terrain analysis</summary>
  <div class="exercise-content">
    <p>Design a landslide susceptibility model for a Kyrgyz mountain valley using only DEM-derived variables. List the variables, justify each, and describe how you would combine them.</p>
    <details class="solution">
      <summary>Solution</summary>
      <p><strong>Variables:</strong>
      1. **Slope**: Primary control on gravitational stress (threshold typically 25-45°)
      2. **TWI**: Indicates soil saturation potential (trigger for rainfall-induced slides)
      3. **Plan curvature**: Convergent areas concentrate water
      4. **Profile curvature**: Convex slopes indicate tension zones
      5. **TRI**: Rugged terrain indicates past instability
      6. **Aspect**: North slopes may have more snowmelt infiltration
      
      <strong>Combination:</strong> Use logistic regression or weights-of-evidence calibrated against historical landslide inventory:
      $$P = \frac{1}{1 + e^{-(\beta_0 + \beta_1 S + \beta_2 TWI + \beta_3 \kappa_c + ...)}}$$
      Validate with independent landslide data and express results as susceptibility classes.</p>
    </details>
  </div>
</details>
