---
layout: tutorial
title: "How GNSS Surveying Enables Precise Georeferencing in Central Asia"
description: "A math-first tutorial on GPS/GLONASS positioning, coordinate systems, differential correction, and survey-grade georeferencing for Central Asia, with 30 exercises and solutions."
difficulty: intermediate
estimated_time: "2-3 hours"
series: treasure-hunting
order: 6
---

## Why this tutorial matters for field work

Every measurement you make in the field—whether from EM surveys, GPR transects, metal detection grids, or XRF sampling—needs to be tied to a coordinate system. Without accurate georeferencing, your data cannot be integrated with satellite imagery, historical maps, or regional databases.

In Central Asia, GNSS surveying faces unique challenges: vast distances between reference stations, legacy Soviet coordinate systems, and terrain ranging from desert plains to mountain valleys. Understanding the mathematics of satellite positioning lets you estimate realistic accuracy and choose appropriate methods for your project.

<div class="info-box tip">
  <strong>Key idea</strong>
  GNSS does not measure position directly. It measures distance to satellites. Position emerges from solving a system of equations.
</div>

## GNSS fundamentals: the four constellations

### GPS (United States)

The Global Positioning System uses 31 satellites in six orbital planes at approximately 20,200 km altitude. Each satellite transmits on L1 (1575.42 MHz) and L2 (1227.60 MHz) frequencies.

### GLONASS (Russia)

Russia's GLONASS constellation has 24 satellites at approximately 19,100 km altitude. GLONASS uses frequency division multiple access (FDMA), meaning each satellite transmits on a slightly different frequency.

<div class="info-box tip">
  <strong>Central Asia advantage</strong>
  GLONASS satellites have higher orbital inclination (64.8° vs 55° for GPS), providing better coverage at northern latitudes common in Kazakhstan and Kyrgyzstan.
</div>

### Galileo (European Union)

The European system operates 24 satellites at 23,222 km altitude. Galileo offers improved accuracy and integrity monitoring useful for professional surveying.

### BeiDou (China)

China's BeiDou system includes geostationary and inclined geosynchronous satellites, providing enhanced coverage across Asia. For Central Asian fieldwork, BeiDou often contributes 8-12 visible satellites.

## Mathematical foundations

### Trilateration: the core geometry

Position is determined by measuring distances to multiple satellites. If you know the exact distance $\rho_i$ to satellite $i$ at known position $(X_i, Y_i, Z_i)$, your position $(x, y, z)$ satisfies:

$$
\rho_i = \sqrt{(X_i - x)^2 + (Y_i - y)^2 + (Z_i - z)^2}
$$

With three satellites, you have three equations and three unknowns. But GNSS receivers have imperfect clocks, adding a fourth unknown: receiver clock bias $b$.

### Pseudorange measurement

The receiver measures signal travel time $\Delta t_i$ and multiplies by the speed of light $c$ to get pseudorange:

$$
P_i = c \cdot \Delta t_i = \rho_i + c \cdot b + \epsilon_i
$$

where $\epsilon_i$ represents errors from atmospheric delays, multipath, and noise. The term "pseudo" indicates the measurement includes clock error.

### The navigation equations

With four or more satellites, we solve:

$$
P_i = \sqrt{(X_i - x)^2 + (Y_i - y)^2 + (Z_i - z)^2} + c \cdot b
$$

This is a system of nonlinear equations typically solved by linearization. Define the geometric range:

$$
\rho_i^0 = \sqrt{(X_i - x_0)^2 + (Y_i - y_0)^2 + (Z_i - z_0)^2}
$$

where $(x_0, y_0, z_0)$ is an initial position estimate. The linearized system becomes:

$$
\Delta P_i = a_{ix} \Delta x + a_{iy} \Delta y + a_{iz} \Delta z + c \cdot \Delta b
$$

where the direction cosines are:

$$
a_{ix} = \frac{x_0 - X_i}{\rho_i^0}, \quad a_{iy} = \frac{y_0 - Y_i}{\rho_i^0}, \quad a_{iz} = \frac{z_0 - Z_i}{\rho_i^0}
$$

### Carrier phase measurement

For centimeter-level accuracy, receivers measure the phase of the carrier wave. The carrier phase observable is:

$$
\Phi_i = \rho_i + c \cdot b - c \cdot b_i^s + \lambda N_i + \epsilon_i
$$

where:
- $b_i^s$ is satellite clock bias
- $\lambda$ is carrier wavelength (19.0 cm for GPS L1)
- $N_i$ is the integer ambiguity (unknown number of whole wavelengths)

<div class="info-box tip">
  <strong>Key idea</strong>
  Carrier phase is more precise than pseudorange by a factor of 100 or more, but requires resolving integer ambiguities.
</div>

### Dilution of Precision (DOP)

Satellite geometry affects position accuracy. The DOP factor multiplies measurement error to give position error:

$$
\sigma_{position} = \text{DOP} \times \sigma_{measurement}
$$

The geometry matrix $G$ and covariance matrix $Q$ are related by:

$$
Q = (G^T G)^{-1}
$$

Different DOP values describe different components:
- GDOP: Geometric (3D position + time)
- PDOP: Position (3D only)
- HDOP: Horizontal (2D)
- VDOP: Vertical

For Central Asian surveys, aim for PDOP < 3.0.

## Coordinate systems for Central Asia

### WGS84: the global reference

WGS84 (World Geodetic System 1984) is the native reference frame for GPS. Coordinates are expressed as:
- Latitude $\phi$ (degrees north/south of equator)
- Longitude $\lambda$ (degrees east/west of prime meridian)
- Ellipsoidal height $h$ (meters above WGS84 ellipsoid)

The WGS84 ellipsoid parameters:
- Semi-major axis: $a = 6{,}378{,}137$ m
- Flattening: $f = 1/298.257223563$

### UTM zones for Central Asia

Universal Transverse Mercator (UTM) provides planar coordinates suitable for mapping. Central Asia spans multiple UTM zones:

| Country | Primary UTM Zones |
|---------|-------------------|
| Kazakhstan | 40N, 41N, 42N, 43N, 44N |
| Uzbekistan | 41N, 42N |
| Kyrgyzstan | 42N, 43N |
| Tajikistan | 42N, 43N |
| Turkmenistan | 40N, 41N |

The conversion from geodetic to UTM coordinates uses:

$$
E = E_0 + k_0 \cdot N \cdot (A + A_3 + A_5)
$$

$$
N_{UTM} = N_0 + k_0 \cdot (M + N \cdot \tan\phi \cdot (A_2 + A_4 + A_6))
$$

where $k_0 = 0.9996$ is the scale factor and $E_0 = 500{,}000$ m is the false easting.

### Soviet legacy: Pulkovo 1942 and SK-42

Many Central Asian maps use the Pulkovo 1942 datum with Krasovsky ellipsoid parameters:
- Semi-major axis: $a = 6{,}378{,}245$ m
- Flattening: $f = 1/298.3$

The transformation from WGS84 to Pulkovo 1942 requires a 7-parameter Helmert transformation:

$$
\begin{pmatrix} X_{P42} \\ Y_{P42} \\ Z_{P42} \end{pmatrix} = 
\begin{pmatrix} \Delta X \\ \Delta Y \\ \Delta Z \end{pmatrix} + 
(1 + s) \cdot R \cdot 
\begin{pmatrix} X_{WGS} \\ Y_{WGS} \\ Z_{WGS} \end{pmatrix}
$$

where $R$ is a rotation matrix and $s$ is scale factor. Typical parameters for Central Asia:
- $\Delta X \approx +25$ m
- $\Delta Y \approx -141$ m
- $\Delta Z \approx -79$ m

<div class="info-box tip">
  <strong>Warning</strong>
  Using wrong datum parameters can introduce errors of 100+ meters. Always verify transformation parameters against local control points.
</div>

## Error sources and accuracy levels

### Atmospheric errors

**Ionospheric delay** affects signal propagation. The delay is frequency-dependent:

$$
I = \frac{40.3 \cdot TEC}{f^2}
$$

where TEC is total electron content and $f$ is frequency in Hz. Dual-frequency receivers eliminate most ionospheric error.

**Tropospheric delay** depends on atmospheric pressure, temperature, and humidity. Models like Saastamoinen estimate:

$$
T = 0.002277 \cdot \left( P + \frac{1255}{T} + 0.05 \right) \cdot e \cdot \frac{1}{\cos z}
$$

### Multipath error

Reflected signals from nearby surfaces cause position errors of 0.5-5 m. Common multipath sources in Central Asia:
- Vehicle bodies during mobile surveys
- Rocky outcrops in mountain regions
- Metal structures at archaeological sites

### Typical accuracy levels

| Method | Horizontal Accuracy | Vertical Accuracy |
|--------|---------------------|-------------------|
| Single point (autonomous) | 3-5 m | 5-8 m |
| SBAS-aided (EGNOS/SDCM) | 1-2 m | 2-3 m |
| Post-processed differential | 0.5-1 m | 1-2 m |
| Real-time kinematic (RTK) | 1-3 cm | 2-5 cm |
| Static carrier phase | 2-5 mm | 5-10 mm |

## Differential GNSS and RTK concepts

### The differential principle

Errors are correlated over short baselines. A reference station at known position $(X_r, Y_r, Z_r)$ computes pseudorange corrections:

$$
\Delta P_i = P_i^{measured} - P_i^{computed}
$$

The rover applies these corrections:

$$
P_i^{corrected} = P_i^{rover} - \Delta P_i
$$

Atmospheric and orbital errors largely cancel over baselines less than 20 km.

### Real-Time Kinematic (RTK)

RTK uses carrier phase measurements with real-time data link. The rover resolves integer ambiguities using:

$$
\nabla \Delta \Phi_{12}^{jk} = \nabla \Delta \rho_{12}^{jk} + \lambda \nabla \Delta N_{12}^{jk}
$$

where $\nabla \Delta$ denotes double-differencing between receivers (1,2) and satellites (j,k).

### Network RTK in Central Asia

Several Central Asian countries are developing CORS (Continuously Operating Reference Station) networks:
- Kazakhstan: KazGEO network
- Uzbekistan: Emerging infrastructure
- Kyrgyzstan: Limited coverage, mountainous terrain

For remote sites, bring your own base station and establish local control.

## Integration with other survey methods

### Georeferencing EM survey grids

For electromagnetic conductivity surveys, establish a local grid with GNSS-surveyed corners:

1. Survey grid corners with RTK or static GNSS
2. Compute grid azimuth from corner coordinates
3. Record EM station positions relative to grid origin
4. Transform to geographic coordinates:

$$
\begin{pmatrix} E \\ N \end{pmatrix} = 
\begin{pmatrix} E_0 \\ N_0 \end{pmatrix} + 
\begin{pmatrix} \cos\theta & -\sin\theta \\ \sin\theta & \cos\theta \end{pmatrix}
\begin{pmatrix} x \\ y \end{pmatrix}
$$

### GPR transect positioning

GPR data requires continuous position logging. Options include:
- **Wheel odometer**: Relative positioning along transect
- **GNSS logging**: 1 Hz or higher position updates
- **Total station**: For high-precision indoor or obstructed areas

The GPR trace position is interpolated:

$$
(E_i, N_i) = (E_1, N_1) + \frac{d_i}{D} \cdot ((E_2, N_2) - (E_1, N_1))
$$

where $d_i$ is distance to trace $i$ and $D$ is total transect length.

### Metal detection find spots

Record each find with GNSS coordinates. For sub-meter accuracy in difficult conditions:

1. Mark find location with flag
2. Wait for satellite geometry to improve (PDOP < 2.5)
3. Average 30+ position fixes
4. Record in field database with timestamp and DOP values

## Practical field procedures

### Pre-survey planning

1. **Check satellite availability**: Use planning software to identify optimal observation windows
2. **Verify transformation parameters**: Test against known benchmarks
3. **Prepare equipment**: Charge batteries, format memory cards, update firmware
4. **Review site conditions**: Identify potential multipath sources and obstructions

### Base station setup

For local RTK operations:

1. Position tripod over control point or establish new point
2. Level precisely (bubble within 1mm of center)
3. Measure antenna height to phase center
4. Configure base to broadcast corrections (radio or cellular)
5. Log raw data for post-processing backup

### Rover operations

1. Initialize RTK and verify fixed solution
2. Check coordinate system and transformation
3. Measure known point to verify setup
4. Begin survey with systematic coverage
5. Re-check known point periodically

<div class="info-box tip">
  <strong>Field tip</strong>
  In Central Asia's continental climate, battery performance drops significantly below -10°C. Carry spare batteries inside your coat.
</div>

## Data post-processing and coordinate transformation

### Processing workflow

1. **Download raw data** from base and rover
2. **Import into processing software** (e.g., Trimble Business Center, Leica Infinity)
3. **Apply precise ephemeris** if available
4. **Process baselines** with appropriate settings
5. **Adjust network** if multiple base stations used
6. **Transform coordinates** to project datum
7. **Export** in required format

### Quality indicators

- **RMS values**: Should be < 10 mm horizontal, < 15 mm vertical for RTK
- **Ambiguity resolution**: Fixed integer solutions required for cm accuracy
- **Baseline length**: Accuracy degrades approximately 1 ppm with distance

### Coordinate transformation in practice

For transforming between WGS84 and local systems:

$$
\begin{pmatrix} x' \\ y' \\ z' \end{pmatrix} = 
\begin{pmatrix} 1 & r_z & -r_y \\ -r_z & 1 & r_x \\ r_y & -r_x & 1 \end{pmatrix}
\begin{pmatrix} x \\ y \\ z \end{pmatrix}
\cdot (1 + s) +
\begin{pmatrix} t_x \\ t_y \\ t_z \end{pmatrix}
$$

Determine parameters by measuring common points in both systems and solving by least squares.

## Field checklist

<ul class="checklist">
  <li>GNSS receiver charged and firmware updated</li>
  <li>Base station tripod, tribrach, and antenna</li>
  <li>Radio or cellular data link equipment</li>
  <li>Rover pole with bipod for stability</li>
  <li>Spare batteries (especially for cold weather)</li>
  <li>Coordinate transformation parameters verified</li>
  <li>Control point coordinates for checking</li>
  <li>Field notebook and waterproof markers</li>
  <li>Satellite availability checked for survey window</li>
  <li>Memory cards formatted and labeled</li>
  <li>Processing software license current</li>
  <li>Site access permissions confirmed</li>
</ul>

## Exercises

<div class="exercise" id="exercise-1">
  <h4>Exercise 1: Pseudorange calculation</h4>
  <p>A GNSS receiver measures signal travel time of 0.072 seconds from a satellite. Calculate the pseudorange.</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>Using $P = c \cdot \Delta t$ where $c = 299{,}792{,}458$ m/s:</p>
      <p>$$P = 299{,}792{,}458 \times 0.072 = 21{,}585{,}057\ \text{m}$$</p>
      <p>The pseudorange is approximately 21,585 km.</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-2">
  <h4>Exercise 2: Position error from DOP</h4>
  <p>If HDOP = 1.2 and the pseudorange measurement error is 2.5 m, what is the expected horizontal position error?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>$$\sigma_H = \text{HDOP} \times \sigma_{measurement} = 1.2 \times 2.5 = 3.0\ \text{m}$$</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-3">
  <h4>Exercise 3: Ionospheric delay</h4>
  <p>Calculate the ionospheric delay for GPS L1 (1575.42 MHz) when TEC = $50 \times 10^{16}$ electrons/m².</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>$$I = \frac{40.3 \times TEC}{f^2} = \frac{40.3 \times 50 \times 10^{16}}{(1575.42 \times 10^6)^2}$$</p>
      <p>$$I = \frac{2.015 \times 10^{18}}{2.48 \times 10^{18}} = 8.1\ \text{m}$$</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-4">
  <h4>Exercise 4: UTM zone determination</h4>
  <p>A site in Kyrgyzstan has longitude 74.5°E. Which UTM zone contains this location?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>UTM zone = floor((longitude + 180) / 6) + 1</p>
      <p>Zone = floor((74.5 + 180) / 6) + 1 = floor(42.4) + 1 = 42 + 1 = 43</p>
      <p>The site is in UTM Zone 43N.</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-5">
  <h4>Exercise 5: Baseline accuracy</h4>
  <p>An RTK baseline is 15 km long with base accuracy of 5 mm + 1 ppm. What is the total horizontal uncertainty?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>$$\sigma = 5\ \text{mm} + 1\ \text{ppm} \times 15{,}000\ \text{m}$$</p>
      <p>$$\sigma = 5\ \text{mm} + 15\ \text{mm} = 20\ \text{mm}$$</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-6">
  <h4>Exercise 6: Carrier wavelength</h4>
  <p>Calculate the wavelength of GPS L1 carrier (frequency 1575.42 MHz).</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>$$\lambda = \frac{c}{f} = \frac{299{,}792{,}458}{1{,}575{,}420{,}000} = 0.1903\ \text{m} = 19.03\ \text{cm}$$</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-7">
  <h4>Exercise 7: Number of ambiguity cycles</h4>
  <p>If the true range to a satellite is 22,500,000 m and wavelength is 19 cm, how many complete wavelengths fit in the path?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>$$N = \frac{\rho}{\lambda} = \frac{22{,}500{,}000}{0.19} = 118{,}421{,}052\ \text{cycles}$$</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-8">
  <h4>Exercise 8: Geometric range</h4>
  <p>Calculate the geometric range from receiver at (4,000,000, 3,000,000, 4,500,000) m to satellite at (15,000,000, 12,000,000, 18,000,000) m in ECEF coordinates.</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>$$\rho = \sqrt{(15-4)^2 + (12-3)^2 + (18-4.5)^2} \times 10^6$$</p>
      <p>$$\rho = \sqrt{121 + 81 + 182.25} \times 10^6 = \sqrt{384.25} \times 10^6$$</p>
      <p>$$\rho = 19.60 \times 10^6\ \text{m} = 19{,}600\ \text{km}$$</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-9">
  <h4>Exercise 9: Clock bias effect</h4>
  <p>A receiver clock is fast by 1 microsecond. What pseudorange error does this introduce?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>$$\Delta P = c \times \Delta t = 299{,}792{,}458 \times 10^{-6} = 299.8\ \text{m}$$</p>
      <p>A 1 μs clock error causes approximately 300 m range error.</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-10">
  <h4>Exercise 10: GLONASS frequency</h4>
  <p>GLONASS L1 center frequency is 1602 MHz with channel spacing of 0.5625 MHz. What is the frequency for channel k = 3?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>$$f_k = 1602 + k \times 0.5625 = 1602 + 3 \times 0.5625 = 1603.6875\ \text{MHz}$$</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-11">
  <h4>Exercise 11: Grid rotation</h4>
  <p>A local survey grid has azimuth 15° from north. Convert local coordinates (100, 50) to grid north-aligned coordinates.</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>Using rotation matrix with $\theta = 15°$:</p>
      <p>$$E = 100 \cos(15°) - 50 \sin(15°) = 96.6 - 12.9 = 83.7\ \text{m}$$</p>
      <p>$$N = 100 \sin(15°) + 50 \cos(15°) = 25.9 + 48.3 = 74.2\ \text{m}$$</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-12">
  <h4>Exercise 12: Differential correction</h4>
  <p>Base station computes pseudorange correction of -3.5 m. Rover measures pseudorange of 21,500,000 m. What is the corrected pseudorange?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>$$P_{corrected} = P_{rover} + \Delta P = 21{,}500{,}000 + (-3.5) = 21{,}499{,}996.5\ \text{m}$$</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-13">
  <h4>Exercise 13: Ellipsoidal height vs orthometric</h4>
  <p>At a site in Uzbekistan, the geoid undulation is N = -35 m. If the GNSS-measured ellipsoidal height is 450 m, what is the orthometric height?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>$$H = h - N = 450 - (-35) = 485\ \text{m}$$</p>
      <p>The orthometric (sea level) height is 485 m.</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-14">
  <h4>Exercise 14: Observation averaging</h4>
  <p>Five position fixes give eastings: 500001.2, 500003.1, 499999.8, 500002.5, 500001.4 m. Calculate the mean and standard deviation.</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>Mean = (1.2 + 3.1 - 0.2 + 2.5 + 1.4)/5 + 500000 = 500001.6 m</p>
      <p>Variance = [(−0.4)² + (1.5)² + (−1.8)² + (0.9)² + (−0.2)²]/4 = 1.65</p>
      <p>Standard deviation = √1.65 = 1.28 m</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-15">
  <h4>Exercise 15: Minimum satellites</h4>
  <p>Why are four satellites required for 3D positioning, not three?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>Three unknowns (x, y, z) define position, but receiver clock bias adds a fourth unknown. Four equations (from four satellites) are needed to solve for four unknowns.</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-16">
  <h4>Exercise 16: Double differencing</h4>
  <p>Explain why double differencing eliminates satellite clock errors.</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>Single difference between receivers eliminates satellite clock error (same satellite, both receivers). Second difference between satellites eliminates receiver clock error. The double difference contains only geometric terms plus integer ambiguity and noise.</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-17">
  <h4>Exercise 17: Tropospheric model</h4>
  <p>At elevation angle 30°, how much longer is the signal path through the troposphere compared to zenith?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>Mapping function ≈ 1/sin(elevation)</p>
      <p>$$m = \frac{1}{\sin(30°)} = \frac{1}{0.5} = 2$$</p>
      <p>The path is twice as long as at zenith.</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-18">
  <h4>Exercise 18: Datum shift magnitude</h4>
  <p>If Pulkovo 1942 differs from WGS84 by approximately 100 m in your area, and you use the wrong datum, what happens to your excavation grid?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>The entire grid shifts by ~100 m from the intended location. Features identified in satellite imagery will not align with ground positions. Integration with other data sources will fail.</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-19">
  <h4>Exercise 19: RTK initialization time</h4>
  <p>Why does RTK require an initialization period, and what happens during it?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>During initialization, the receiver resolves integer ambiguities—determining the exact number of carrier wavelengths between receiver and each satellite. This requires observing satellites for 30-120 seconds to solve the ambiguity equations reliably.</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-20">
  <h4>Exercise 20: PDOP interpretation</h4>
  <p>PDOP values are 1.5 at 10:00, 2.8 at 14:00, and 5.2 at 18:00. Which time offers best positioning?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>10:00 with PDOP = 1.5 offers best positioning. Lower DOP means better satellite geometry. PDOP > 4 typically indicates poor geometry; avoid surveying at 18:00 if possible.</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-21">
  <h4>Exercise 21: Multipath identification</h4>
  <p>How can you identify multipath in your GNSS data?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>Signs of multipath include: (1) High residuals on specific satellites, (2) Signal-to-noise ratio variations, (3) Position scatter that repeats daily (satellite geometry repeats), (4) Large differences between code and carrier measurements. Move away from reflective surfaces.</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-22">
  <h4>Exercise 22: Central meridian</h4>
  <p>What is the central meridian for UTM Zone 42N, and why does it matter?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>Central meridian = 6° × 42 - 183° = 69°E</p>
      <p>Scale factor is exactly 0.9996 along the central meridian and increases toward zone edges. Points far from the central meridian have larger scale distortion.</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-23">
  <h4>Exercise 23: Antenna height error</h4>
  <p>If you record antenna height as 2.00 m but it's actually 2.15 m, what error appears in your vertical coordinates?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>All points will have 0.15 m systematic error in height. Horizontal positions are unaffected. This is why careful antenna height measurement and recording is critical.</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-24">
  <h4>Exercise 24: Epoch weighting</h4>
  <p>You have 100 epochs with PDOP 1.5 and 50 epochs with PDOP 4.0. If you weight by 1/PDOP², what is the weighted contribution of each group?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>Weight₁ = 1/(1.5)² = 0.444 per epoch × 100 = 44.4 total</p>
      <p>Weight₂ = 1/(4.0)² = 0.0625 per epoch × 50 = 3.125 total</p>
      <p>Good-geometry epochs contribute 44.4/(44.4+3.125) = 93% of solution weight.</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-25">
  <h4>Exercise 25: Position repeatability</h4>
  <p>You survey the same control point on three different days, getting coordinates differing by up to 8 mm. Is this acceptable for archaeological grid control?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>Yes. For most archaeological applications, 1-2 cm repeatability is excellent. 8 mm variation is within typical RTK performance and far exceeds requirements for grid layout and feature mapping.</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-26">
  <h4>Exercise 26: Satellite visibility</h4>
  <p>A mountain blocks all satellites below 25° elevation to the north. How does this affect positioning?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>Satellites visible from the south create an asymmetric geometry, degrading north-south positioning accuracy. DOP values increase. Solution: observe during times when more satellites are in the southern sky, or extend observation time to capture different satellite positions.</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-27">
  <h4>Exercise 27: GPR transect georeferencing</h4>
  <p>A 50 m GPR transect has start coordinates (500100, 4500200) and end coordinates (500145, 4500225). What are the coordinates at trace 200 of 400 total traces?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>Fraction along line = 200/400 = 0.5</p>
      <p>$$E = 500100 + 0.5 × (500145 - 500100) = 500122.5$$</p>
      <p>$$N = 4500200 + 0.5 × (4500225 - 4500200) = 4500212.5$$</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-28">
  <h4>Exercise 28: Survey-to-excavation transformation</h4>
  <p>An excavation grid uses local coordinates with origin at a GNSS-surveyed point. Grid north is rotated 12° from true north. How do you transform local (5, 10) to UTM?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>Apply rotation then translation:</p>
      <p>$$\Delta E = 5 \cos(12°) - 10 \sin(12°) = 4.89 - 2.08 = 2.81\ \text{m}$$</p>
      <p>$$\Delta N = 5 \sin(12°) + 10 \cos(12°) = 1.04 + 9.78 = 10.82\ \text{m}$$</p>
      <p>Add to origin UTM coordinates for final position.</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-29">
  <h4>Exercise 29: Base station occupation</h4>
  <p>You need 5 mm + 0.5 ppm accuracy over a 30 km baseline. The receiver specification requires 1 hour per 10 km for this accuracy. How long should you occupy the base?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>For 30 km at 1 hour per 10 km: 3 hours minimum occupation.</p>
      <p>Expected accuracy: 5 mm + 0.5 ppm × 30,000 m = 5 mm + 15 mm = 20 mm</p>
      <p>Longer occupation improves results; consider 4-6 hours for critical control.</p>
    </div>
  </details>
</div>

<div class="exercise" id="exercise-30">
  <h4>Exercise 30: Multi-constellation benefit</h4>
  <p>With GPS only, you see 6 satellites. Adding GLONASS brings 5 more. How does this affect PDOP, assuming uniform sky distribution?</p>
  <details>
    <summary>Show solution</summary>
    <div class="solution">
      <p>More satellites improve geometry. With redundant observations, DOP typically improves by factor of $\sqrt{n_1/n_2}$ where n is satellite count.</p>
      <p>Approximate improvement: $\sqrt{6/11} \approx 0.74$</p>
      <p>If original PDOP was 2.5, new PDOP ≈ 1.85. Multi-constellation also provides resilience if some satellites are blocked.</p>
    </div>
  </details>
</div>

## Final takeaway

GNSS surveying transforms your field measurements into precisely located data that integrates with global mapping systems. In Central Asia, success depends on understanding the mathematics of positioning, choosing appropriate methods for your accuracy requirements, and correctly handling coordinate transformations between WGS84 and legacy datums. Master these fundamentals, and your geophysical surveys become spatially precise contributions to regional research.
