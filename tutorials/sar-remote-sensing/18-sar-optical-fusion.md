---
layout: tutorial
title: "How SAR-Optical Fusion Enhances Land Cover Analysis in Central Asia"
description: "A practical tutorial on combining Sentinel-1 SAR and Sentinel-2 optical data for improved classification, gap-filling, and time-series analysis, with 30 exercises and solutions."
difficulty: intermediate
estimated_time: "3-4 hours"
series: sar-remote-sensing
order: 18
---

## Why fusion matters

Central Asia loses 30–60% of Sentinel-2 acquisitions to cloud cover during the growing season. Optical sensors alone cannot deliver continuous monitoring for irrigation management, crop mapping, or environmental reporting. SAR penetrates clouds but lacks spectral richness. Fusion combines both strengths—structural and moisture sensitivity from SAR, spectral discrimination from optical—consistently improving classification accuracy by 5–15 percentage points.

<div class="info-box tip">
  <strong>Key idea</strong>
  SAR and optical data are complementary, not redundant. SAR responds to structure, roughness, and moisture. Optical responds to pigments, minerals, and reflectance. Combining them captures information that neither sensor provides alone.
</div>

<div class="learning-objectives">
  <div class="learning-objectives-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#1e4f8a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <h3>What you will learn</h3>
  </div>
  <ul>
    <li>Explain why SAR (structure, moisture) and optical (spectral, pigments) data are complementary rather than redundant</li>
    <li>Co-register Sentinel-1 and Sentinel-2 imagery with proper preprocessing — radiometric terrain correction and cloud masking</li>
    <li>Implement feature-level fusion by stacking SAR and optical bands and decision-level fusion using ensemble classifiers</li>
    <li>Fill cloud gaps in optical time series using SAR-derived predictions and temporal harmonization techniques</li>
    <li>Evaluate fusion accuracy gains (5–15 pp improvement) by comparing SAR-only, optical-only, and fused classification results</li>
  </ul>
</div>

<div class="prerequisites">
  <div class="prerequisites-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#8b5e00" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
    <h3>Prerequisites</h3>
  </div>
  <ul>
    <li>Understanding of SAR backscatter (VV/VH polarization) and optical multispectral imagery (Sentinel-2 bands, NDVI)</li>
    <li>Familiarity with supervised classification concepts (training data, Random Forest, accuracy metrics)</li>
    <li>Python experience with geospatial libraries or Google Earth Engine JavaScript/Python API</li>
    <li>Basic knowledge of image co-registration and coordinate reference systems</li>
  </ul>
</div>

<div class="concept-diagram">
  <svg viewBox="0 0 620 320" xmlns="http://www.w3.org/2000/svg" style="max-width: 580px;">
    <defs>
      <marker id="fusArrow" markerWidth="8" markerHeight="6" refX="8" refY="3" orient="auto"><path d="M0,0 L8,3 L0,6" fill="#165d34"/></marker>
      <marker id="fusArrowG" markerWidth="8" markerHeight="6" refX="8" refY="3" orient="auto"><path d="M0,0 L8,3 L0,6" fill="#68625b"/></marker>
    </defs>
    <text x="310" y="18" font-family="Inter, sans-serif" font-size="13" fill="#111" text-anchor="middle" font-weight="bold">SAR-Optical Data Fusion for Land Cover Classification</text>
    <!-- SAR input (left) -->
    <rect x="20" y="40" width="170" height="120" rx="6" fill="#e8f0fa" stroke="#1e4f8a" stroke-width="1.5"/>
    <text x="105" y="60" font-family="Inter, sans-serif" font-size="12" fill="#1e4f8a" text-anchor="middle" font-weight="bold">Sentinel-1 SAR</text>
    <!-- SAR grayscale image representation -->
    <rect x="38" y="70" width="55" height="55" rx="2" fill="#444" stroke="#68625b" stroke-width="0.5"/>
    <rect x="42" y="74" width="10" height="10" fill="#999"/><rect x="54" y="74" width="10" height="10" fill="#555"/><rect x="66" y="74" width="10" height="10" fill="#bbb"/>
    <rect x="42" y="86" width="10" height="10" fill="#666"/><rect x="54" y="86" width="10" height="10" fill="#aaa"/><rect x="66" y="86" width="10" height="10" fill="#777"/>
    <rect x="42" y="98" width="10" height="10" fill="#888"/><rect x="54" y="98" width="10" height="10" fill="#555"/><rect x="66" y="98" width="10" height="10" fill="#ccc"/>
    <text x="65" y="135" font-family="Inter, sans-serif" font-size="8" fill="#68625b" text-anchor="middle">VV + VH bands</text>
    <!-- SAR info bullets -->
    <text x="105" y="78" font-family="Inter, sans-serif" font-size="9" fill="#111" text-anchor="start">✓ All-weather</text>
    <text x="105" y="92" font-family="Inter, sans-serif" font-size="9" fill="#111" text-anchor="start">✓ Structure</text>
    <text x="105" y="106" font-family="Inter, sans-serif" font-size="9" fill="#111" text-anchor="start">✓ Moisture</text>
    <text x="105" y="120" font-family="Inter, sans-serif" font-size="9" fill="#111" text-anchor="start">✓ Roughness</text>
    <text x="105" y="148" font-family="Inter, sans-serif" font-size="9" fill="#d92b1f" text-anchor="middle">✗ Limited spectral info</text>
    <!-- Optical input (right) -->
    <rect x="430" y="40" width="170" height="120" rx="6" fill="#fdf6e3" stroke="#8b5e00" stroke-width="1.5"/>
    <text x="515" y="60" font-family="Inter, sans-serif" font-size="12" fill="#8b5e00" text-anchor="middle" font-weight="bold">Sentinel-2 Optical</text>
    <!-- Optical color image representation -->
    <rect x="448" y="70" width="55" height="55" rx="2" fill="#eee" stroke="#68625b" stroke-width="0.5"/>
    <rect x="452" y="74" width="10" height="10" fill="#165d34"/><rect x="464" y="74" width="10" height="10" fill="#6aa843"/><rect x="476" y="74" width="10" height="10" fill="#165d34"/>
    <rect x="452" y="86" width="10" height="10" fill="#8b5e00"/><rect x="464" y="86" width="10" height="10" fill="#c4a56e"/><rect x="476" y="86" width="10" height="10" fill="#6aa843"/>
    <rect x="452" y="98" width="10" height="10" fill="#1e4f8a"/><rect x="464" y="98" width="10" height="10" fill="#165d34"/><rect x="476" y="98" width="10" height="10" fill="#8b5e00"/>
    <text x="475" y="135" font-family="Inter, sans-serif" font-size="8" fill="#68625b" text-anchor="middle">13 spectral bands</text>
    <!-- Optical info bullets -->
    <text x="515" y="78" font-family="Inter, sans-serif" font-size="9" fill="#111" text-anchor="start">✓ Spectral</text>
    <text x="515" y="92" font-family="Inter, sans-serif" font-size="9" fill="#111" text-anchor="start">✓ Vegetation</text>
    <text x="515" y="106" font-family="Inter, sans-serif" font-size="9" fill="#111" text-anchor="start">✓ Minerals</text>
    <text x="515" y="120" font-family="Inter, sans-serif" font-size="9" fill="#111" text-anchor="start">✓ Color</text>
    <text x="515" y="148" font-family="Inter, sans-serif" font-size="9" fill="#d92b1f" text-anchor="middle">✗ Blocked by clouds</text>
    <!-- Fusion arrows converging -->
    <line x1="190" y1="110" x2="245" y2="200" stroke="#1e4f8a" stroke-width="2" marker-end="url(#fusArrow)"/>
    <line x1="430" y1="110" x2="375" y2="200" stroke="#8b5e00" stroke-width="2" marker-end="url(#fusArrow)"/>
    <!-- Plus symbol -->
    <circle cx="310" cy="165" r="16" fill="#fff" stroke="#165d34" stroke-width="2"/>
    <line x1="302" y1="165" x2="318" y2="165" stroke="#165d34" stroke-width="2.5"/>
    <line x1="310" y1="157" x2="310" y2="173" stroke="#165d34" stroke-width="2.5"/>
    <!-- Fused product (center bottom) -->
    <rect x="210" y="200" width="200" height="70" rx="6" fill="#f0f7f2" stroke="#165d34" stroke-width="2"/>
    <text x="310" y="220" font-family="Inter, sans-serif" font-size="12" fill="#165d34" text-anchor="middle" font-weight="bold">Fused Classification</text>
    <!-- Mini classified map -->
    <rect x="240" y="230" width="12" height="10" fill="#165d34"/><rect x="254" y="230" width="12" height="10" fill="#6aa843"/>
    <rect x="268" y="230" width="12" height="10" fill="#8b5e00"/><rect x="282" y="230" width="12" height="10" fill="#1e4f8a"/>
    <rect x="296" y="230" width="12" height="10" fill="#165d34"/><rect x="310" y="230" width="12" height="10" fill="#c4a56e"/>
    <rect x="324" y="230" width="12" height="10" fill="#6aa843"/><rect x="338" y="230" width="12" height="10" fill="#68625b"/>
    <rect x="352" y="230" width="12" height="10" fill="#165d34"/><rect x="366" y="230" width="12" height="10" fill="#8b5e00"/>
    <text x="310" y="257" font-family="Inter, sans-serif" font-size="9" fill="#111" text-anchor="middle">Crop · Water · Urban · Desert · Forest</text>
    <!-- Accuracy comparison -->
    <rect x="100" y="285" width="420" height="28" rx="4" fill="#f5f5f0" stroke="#68625b" stroke-width="0.5"/>
    <text x="310" y="303" font-family="Inter, sans-serif" font-size="10" fill="#111" text-anchor="middle">Typical accuracy: SAR-only ~72% · Optical-only ~82% · <tspan font-weight="bold" fill="#165d34">Fused ~90%</tspan></text>
  </svg>
  <p class="diagram-caption">SAR-optical fusion combines Sentinel-1 structural/moisture information with Sentinel-2 spectral data, producing classified maps with 5–15 percentage-point accuracy improvement over either sensor alone.</p>
</div>

## SAR versus optical: what each sensor sees

Sentinel-1 transmits C-band microwaves (5.405 GHz, ~5.6 cm wavelength). Backscatter intensity depends on surface roughness, dielectric constant (moisture), geometry, and vegetation structure. The backscatter coefficient:

$$
\sigma^0_{dB} = 10 \log_{10}(\sigma^0)
$$

Sentinel-2 measures reflected sunlight across 13 bands from visible through SWIR. Chlorophyll absorption, vegetation indices, and mineral signatures enable species-level discrimination.

| Property | SAR strength | Optical strength |
|----------|-------------|-----------------|
| Cloud independence | ✓ All-weather | ✗ Cloud-blocked |
| Vegetation structure | ✓ Volume scattering | ✗ Top-of-canopy only |
| Soil moisture | ✓ Dielectric sensitivity | ✗ Limited |
| Species discrimination | ✗ Limited spectral info | ✓ Narrow bands |
| Urban mapping | ✓ Double-bounce | ✓ Spectral + spatial |

## Co-registration and geometric alignment

Before fusion, pixels from both sensors must represent the same ground location.

### Preprocessing Sentinel-1

```python
import ee
ee.Initialize()

def preprocess_s1(image):
    """Preprocess Sentinel-1 GRD with angle correction."""
    angle = image.select('angle')
    sigma0 = image.select(['VV', 'VH'])
    sigma0_nat = ee.Image(10).pow(sigma0.divide(10))
    corrected = sigma0_nat.multiply(angle.multiply(3.14159 / 180).cos())
    corrected_db = ee.Image(10).multiply(corrected.log10())
    return corrected_db.rename(['VV', 'VH']).copyProperties(image, ['system:time_start'])

s1 = (ee.ImageCollection('COPERNICUS/S1_GRD')
      .filterBounds(ee.Geometry.Point(71.0, 40.5))
      .filterDate('2023-04-01', '2023-10-01')
      .filter(ee.Filter.eq('instrumentMode', 'IW'))
      .filter(ee.Filter.listContains('transmitterReceiverPolarisation', 'VV'))
      .filter(ee.Filter.listContains('transmitterReceiverPolarisation', 'VH'))
      .filter(ee.Filter.eq('orbitProperties_pass', 'ASCENDING'))
      .map(preprocess_s1))
```

### Preprocessing Sentinel-2

```python
def preprocess_s2(image):
    """Cloud mask and scale Sentinel-2 SR."""
    qa = image.select('QA60')
    mask = qa.bitwiseAnd(1 << 10).eq(0).And(qa.bitwiseAnd(1 << 11).eq(0))
    return image.updateMask(mask).divide(10000).copyProperties(image, ['system:time_start'])

s2 = (ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
      .filterBounds(ee.Geometry.Point(71.0, 40.5))
      .filterDate('2023-04-01', '2023-10-01')
      .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 30))
      .map(preprocess_s2))
```

Resample to a common 10 m grid. Match temporal windows within ±5 days or use monthly composites.

## Feature-level fusion: stacking SAR and optical bands

Concatenate SAR and optical features into one multi-band image:

```python
def build_fused_stack(s1_col, s2_col):
    """Build fused SAR-optical feature stack."""
    opt = s2_col.median().select(['B2','B3','B4','B5','B6','B7','B8','B11','B12'])
    ndvi = s2_col.median().normalizedDifference(['B8', 'B4']).rename('NDVI')
    ndwi = s2_col.median().normalizedDifference(['B3', 'B8']).rename('NDWI')

    vv_mean = s1_col.select('VV').mean().rename('VV_mean')
    vh_mean = s1_col.select('VH').mean().rename('VH_mean')
    vv_std = s1_col.select('VV').reduce(ee.Reducer.stdDev()).rename('VV_std')
    vh_std = s1_col.select('VH').reduce(ee.Reducer.stdDev()).rename('VH_std')
    cr = vh_mean.subtract(vv_mean).rename('VH_VV_ratio')

    return ee.Image.cat([opt, ndvi, ndwi, vv_mean, vh_mean, vv_std, vh_std, cr])
```

Normalize features before classification using z-score:

$$
x_{\text{norm}} = \frac{x - \mu}{\sigma}
$$

<div class="info-box tip">
  <strong>Key idea</strong>
  Feature-level fusion is the simplest approach: stack normalized SAR and optical bands into one image, then feed the combined feature vector to any classifier. The algorithm discovers cross-sensor relationships automatically.
</div>

## Decision-level fusion: ensemble classifiers

Train separate classifiers on SAR and optical data, then combine predictions:

$$
\hat{y} = \arg\max_k \left[ w_S \cdot P_S(y=k|\mathbf{x}) + w_O \cdot P_O(y=k|\mathbf{x}) \right]
$$

where weights $w_S + w_O = 1$ are proportional to validation accuracy.

```python
from sklearn.ensemble import RandomForestClassifier
import numpy as np

def decision_level_fusion(X_sar, X_opt, y, w_sar=0.4, w_opt=0.6):
    """Fuse SAR and optical classifiers at decision level."""
    rf_sar = RandomForestClassifier(n_estimators=200, random_state=42)
    rf_opt = RandomForestClassifier(n_estimators=200, random_state=42)
    rf_sar.fit(X_sar, y)
    rf_opt.fit(X_opt, y)

    prob_fused = w_sar * rf_sar.predict_proba(X_sar) + w_opt * rf_opt.predict_proba(X_opt)
    return np.argmax(prob_fused, axis=1)
```

Decision-level fusion suits scenarios where SAR and optical have different acquisition dates or when you want to evaluate each sensor's independent contribution.

## Cloud gap-filling with SAR

SAR backscatter correlates with vegetation properties, enabling NDVI prediction for cloud-masked dates:

$$
\widehat{\text{NDVI}}_t = \beta_0 + \beta_1 \cdot \sigma^0_{VV,t} + \beta_2 \cdot \sigma^0_{VH,t} + \beta_3 \cdot \frac{\sigma^0_{VH,t}}{\sigma^0_{VV,t}}
$$

```python
from sklearn.ensemble import GradientBoostingRegressor
import numpy as np

def sar_optical_gapfill(sar_ts, ndvi_ts, cloud_mask):
    """Fill NDVI gaps using SAR regression."""
    vv, vh = sar_ts[:, 0], sar_ts[:, 1]
    X = np.column_stack([vv, vh, vh - vv])

    model = GradientBoostingRegressor(n_estimators=100, max_depth=4, random_state=42)
    model.fit(X[cloud_mask], ndvi_ts[cloud_mask])

    ndvi_filled = ndvi_ts.copy()
    gaps = ~cloud_mask
    ndvi_filled[gaps] = model.predict(X[gaps])
    print(f'R²: {model.score(X[cloud_mask], ndvi_ts[cloud_mask]):.3f}, filled {gaps.sum()} dates')
    return ndvi_filled
```

<div class="info-box tip">
  <strong>Key idea</strong>
  VH polarization is especially useful for gap-filling because volume scattering in crop canopies correlates with leaf area index and biomass—the same quantities that drive NDVI.
</div>

## Time-series harmonization

Create monthly composites combining both sensors:

```python
def monthly_fused_composites(s1_col, s2_col, year, aoi):
    """Create monthly SAR-optical composites for growing season."""
    composites = []
    for month in range(4, 11):
        start = ee.Date.fromYMD(year, month, 1)
        end = start.advance(1, 'month')
        sar = s1_col.filterDate(start, end).median().select(['VV', 'VH'])
        opt = s2_col.filterDate(start, end).median().select(['B4', 'B8', 'B11'])
        ndvi = opt.normalizedDifference(['B8', 'B4']).rename('NDVI')
        composites.append(sar.addBands(opt).addBands(ndvi).set('month', month))
    return ee.ImageCollection(composites)
```

Extract temporal statistics—seasonal mean, std, max, percentiles—across the composite series for classification.

## Random Forest classification with fused features

### Comparison workflow

```python
from sklearn.model_selection import cross_val_score, StratifiedKFold

def compare_fusion_approaches(X_sar, X_opt, X_fused, y):
    """Compare SAR-only, optical-only, and fused classification."""
    cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
    for name, X in [('SAR-only', X_sar), ('Optical-only', X_opt), ('Fused', X_fused)]:
        rf = RandomForestClassifier(n_estimators=300, max_depth=20, random_state=42)
        scores = cross_val_score(rf, X, y, cv=cv, scoring='accuracy')
        print(f'{name}: {scores.mean():.3f} ± {scores.std():.3f}')

# Typical Central Asia results:
# SAR-only:     0.72 ± 0.03
# Optical-only: 0.81 ± 0.02
# Fused:        0.89 ± 0.02
```

Feature importance analysis typically shows both SAR and optical features in the top 10, confirming complementary contributions. SAR features rank highest for water/wetland and urban/bare soil separation.

## Central Asia applications

**Agriculture (Fergana Valley)**: Cotton peaks August (high NDVI + high VH), wheat peaks May (high NDVI + low VH). Rice paddies show unique flood-to-canopy SAR signature. Fusion enables crop mapping through cloudy growing season.

**Wetlands (Amu Darya delta)**: SAR detects open water year-round. High VH + moderate NDVI identifies emergent marsh. Combined monitoring tracks Aral Sea desiccation—retreating shoreline (SAR) and salt crust formation (SWIR).

**Urban (Tashkent, Astana)**: Double-bounce in SAR detects buildings regardless of material; NDBI from optical separates built-up from bare soil that SAR confuses. New construction appears as bright SAR targets before optical signatures stabilize.

## Accuracy comparison: SAR-only vs optical-only vs fused

| Land cover class | SAR-only | Optical-only | Fused |
|-----------------|----------|-------------|-------|
| Water | 95% | 97% | 98% |
| Irrigated crop | 68% | 82% | 90% |
| Rainfed crop | 55% | 74% | 83% |
| Grassland | 60% | 71% | 80% |
| Bare soil | 72% | 78% | 86% |
| Urban | 80% | 75% | 91% |
| Wetland | 78% | 70% | 88% |
| **Overall** | **72%** | **78%** | **88%** |

Fusion consistently outperforms either sensor alone. SAR contributes most to urban and wetland classes; optical contributes most to crop discrimination.

<div class="info-box tip">
  <strong>Key idea</strong>
  The largest fusion benefit occurs for classes with ambiguous signatures in one sensor. Urban areas confuse optical (similar to bare soil) but are distinct in SAR. Wetlands confuse SAR (variable water levels) but have distinctive spectral profiles.
</div>

## Checklist

<ul class="checklist">
  <li>Preprocess SAR (calibration, speckle filter, terrain correction) and optical (cloud masking, atmospheric correction) independently before fusion</li>
  <li>Verify co-registration by overlaying SAR and optical imagery and checking feature alignment</li>
  <li>Resample all bands to a common 10 m grid</li>
  <li>Align temporal windows: use composites from the same month or ±5-day matched pairs</li>
  <li>Normalize feature ranges (z-score or min-max) before classification</li>
  <li>Include SAR temporal statistics alongside optical indices</li>
  <li>Compare SAR-only, optical-only, and fused results to quantify fusion benefit</li>
  <li>Use spatial cross-validation to avoid inflated accuracy</li>
  <li>Analyze feature importance to confirm both sensor types contribute</li>
  <li>Validate gap-filled values against withheld cloud-free observations</li>
  <li>Document sensor combinations, temporal windows, and preprocessing for reproducibility</li>
</ul>

## Exercises

<details>
<summary><strong>Exercise 1: Complementary information</strong></summary>

Name three properties SAR measures better than optical, and three that optical measures better.

<strong>Solution:</strong>
SAR excels at: (1) surface roughness, (2) soil moisture, (3) vegetation structure/biomass. Optical excels at: (1) chlorophyll content, (2) mineral composition, (3) crop species discrimination.
</details>

<details>
<summary><strong>Exercise 2: Cloud impact</strong></summary>

With 24 Sentinel-2 acquisitions per year and 40% cloud contamination, how many usable optical images remain? How does Sentinel-1 compare?

<strong>Solution:</strong>
Usable optical: $24 \times 0.60 = 14$ images. Sentinel-1 provides ~30 images/year (ascending + descending), all usable regardless of clouds—more than double the temporal density.
</details>

<details>
<summary><strong>Exercise 3: Cross-ratio calculation</strong></summary>

A pixel has $\sigma^0_{VV} = -8$ dB and $\sigma^0_{VH} = -15$ dB. Calculate the VH/VV cross-ratio and suggest a land cover class.

<strong>Solution:</strong>
Cross-ratio: $-15 - (-8) = -7$ dB. This moderate value suggests cropland or grassland. Dense forest typically shows −6 to −4 dB; bare soil is below −10 dB.
</details>

<details>
<summary><strong>Exercise 4: VH-NDVI correlation</strong></summary>

Explain physically why VH backscatter correlates positively with NDVI over agricultural fields.

<strong>Solution:</strong>
VH increases with volume scattering in dense canopies. As crops grow, more multiple-scattering events depolarize the signal, raising VH. NDVI also increases with canopy density. Both respond to the same variable—vegetation density—creating positive correlation.
</details>

<details>
<summary><strong>Exercise 5: Resolution matching</strong></summary>

Sentinel-2 Band 11 (SWIR) is 20 m; Sentinel-1 VV is 10 m. Describe two approaches to create a 10 m fused product.

<strong>Solution:</strong>
(1) Bilinear resampling of Band 11 to 10 m—simple but adds no real detail. (2) Pan-sharpening using 10 m Sentinel-2 bands (B2, B3, B4, B8) to sharpen 20 m bands before fusion, preserving spectral information at higher resolution.
</details>

<details>
<summary><strong>Exercise 6: Feature vector dimension</strong></summary>

A fused stack has 9 optical bands, 2 indices, VV/VH means, VV/VH standard deviations, and a cross-ratio. How many features?

<strong>Solution:</strong>
$9 + 2 + 2 + 2 + 1 = 16$ features per pixel.
</details>

<details>
<summary><strong>Exercise 7: Z-score normalization</strong></summary>

VV band: mean = −10.5 dB, std = 3.2 dB. A pixel has VV = −6.1 dB. Calculate its z-score.

<strong>Solution:</strong>
$z = \frac{-6.1 - (-10.5)}{3.2} = \frac{4.4}{3.2} = 1.375$. The pixel is 1.375 standard deviations above the mean—likely a bright target (urban or rough surface).
</details>

<details>
<summary><strong>Exercise 8: Temporal alignment</strong></summary>

You have Sentinel-2 from June 5, Sentinel-1 from June 1 and June 13. Which SAR image to pair?

<strong>Solution:</strong>
June 1 (4-day gap) over June 13 (8-day gap). Closer temporal proximity ensures more similar surface conditions, especially during rapid crop growth.
</details>

<details>
<summary><strong>Exercise 9: Speckle filtering importance</strong></summary>

Why is speckle filtering essential before SAR-optical fusion?

<strong>Solution:</strong>
Speckle is multiplicative coherent noise creating random intensity fluctuations. Without filtering, pixel-level SAR values don't reliably correspond to surface properties, degrading classifier accuracy because consistent SAR-optical relationships cannot be established.
</details>

<details>
<summary><strong>Exercise 10: Orbit direction</strong></summary>

Why use a single orbit pass direction (ascending or descending) for SAR composites?

<strong>Solution:</strong>
Ascending and descending passes have different incidence angles and look directions. Mixing them creates inconsistent viewing geometry—shadows differ, backscatter values change for the same target. Single-pass composites ensure temporal statistics are meaningful.
</details>

<details>
<summary><strong>Exercise 11: Feature-level vs decision-level</strong></summary>

SAR images arrive every 12 days; optical only every 30 days after cloud removal. Which fusion approach and why?

<strong>Solution:</strong>
Decision-level fusion. Temporal mismatch means feature stacking wastes SAR data or introduces compositing artifacts. Decision-level fusion trains separate classifiers exploiting each sensor's full temporal density, then combines predictions.
</details>

<details>
<summary><strong>Exercise 12: Weighted voting</strong></summary>

SAR classifier: 75% accuracy. Optical classifier: 85%. Calculate fusion weights.

<strong>Solution:</strong>
$w_{SAR} = \frac{0.75}{0.75 + 0.85} = 0.469$, $w_{opt} = \frac{0.85}{1.60} = 0.531$. Optical gets higher weight but SAR still contributes meaningfully.
</details>

<details>
<summary><strong>Exercise 13: Gap-filling prediction</strong></summary>

Model: $\widehat{\text{NDVI}} = 0.35 + 0.02 \cdot \sigma^0_{VH}$. For $\sigma^0_{VH} = -12$ dB, what is predicted NDVI?

<strong>Solution:</strong>
$\widehat{\text{NDVI}} = 0.35 + 0.02 \times (-12) = 0.11$. Low NDVI consistent with sparse vegetation—physically reasonable given the low VH (minimal volume scattering).
</details>

<details>
<summary><strong>Exercise 14: Gap-fill validation</strong></summary>

Gap-fill RMSE = 0.08, NDVI range is 0.05–0.85. Calculate normalized RMSE and assess quality.

<strong>Solution:</strong>
NRMSE = $\frac{0.08}{0.85 - 0.05} = 10\%$. This is acceptable; below 15% is generally considered good for SAR-based NDVI prediction.
</details>

<details>
<summary><strong>Exercise 15: Multi-temporal feature count</strong></summary>

7-month growing season, 16 features per monthly composite. Total features in a multi-temporal stack?

<strong>Solution:</strong>
$7 \times 16 = 112$ features. Consider PCA or feature selection to avoid the curse of dimensionality—aim for 10–30 training samples per feature per class.
</details>

<details>
<summary><strong>Exercise 16: Rice detection</strong></summary>

Describe the SAR-optical temporal signature of rice paddies (April–September).

<strong>Solution:</strong>
April–May: flooded—very low VV (specular reflection), near-zero NDVI. June–July: growth—VV/VH rise as stems emerge, NDVI increases. August: peak canopy—high VH (volume scattering), NDVI ~0.8. September: senescence—both decline. This flood-to-canopy trajectory is unique to rice.
</details>

<details>
<summary><strong>Exercise 17: Urban double-bounce</strong></summary>

Why does SAR detect buildings more reliably than optical in cities?

<strong>Solution:</strong>
SAR double-bounce (ground-wall reflection) creates bright returns regardless of building material. Optical detection depends on roof reflectance, which varies and can resemble bare soil. Fusion adds NDBI to separate built-up from natural bright surfaces.
</details>

<details>
<summary><strong>Exercise 18: Spatial cross-validation</strong></summary>

Why is random pixel-level CV problematic for fusion classification?

<strong>Solution:</strong>
Spatial autocorrelation means nearby pixels share similar values. Random splits place correlated pixels in both train and test sets, inflating accuracy. Use spatial block CV—assign entire geographic blocks (e.g., 5 km × 5 km) to folds.
</details>

<details>
<summary><strong>Exercise 19: Class confusion</strong></summary>

Grassland and rainfed crop are frequently confused. Propose two features to separate them.

<strong>Solution:</strong>
(1) VV temporal standard deviation—cropland shows higher variation from plowing/planting/harvest cycles vs stable grassland. (2) NDVI green-up rate in spring—crops show rapid green-up after planting; grassland greens gradually.
</details>

<details>
<summary><strong>Exercise 20: Aral Sea monitoring</strong></summary>

How would fusion help monitor the Aral Sea region (2015–2024)?

<strong>Solution:</strong>
SAR provides year-round water extent (low backscatter). Optical NDVI tracks vegetation colonization on exposed lakebed. SWIR detects salt crusts. Combined: consistent shoreline mapping (SAR), vegetation succession (optical), dust source identification from roughness (SAR) plus mineralogy (SWIR).
</details>

<details>
<summary><strong>Exercise 21: PCA on fused stack</strong></summary>

PCA on 112 features: first 15 components explain 95% variance. Tradeoffs?

<strong>Solution:</strong>
Benefits: 87% dimensionality reduction, less overfitting, faster training. Tradeoffs: components lack physical meaning (can't identify which sensor contributed most), and rare but informative features may be compressed into discarded minor components.
</details>

<details>
<summary><strong>Exercise 22: Seasonal strategy for wheat</strong></summary>

Which months to prioritize for winter wheat mapping in Kazakhstan?

<strong>Solution:</strong>
(1) November–December: SAR detects planted fields through clouds. (2) March–April: SAR captures green-up; optical confirms growth. (3) June: both detect harvest. These three windows capture key phenological transitions that separate wheat from other crops.
</details>

<details>
<summary><strong>Exercise 23: Training sample adequacy</strong></summary>

100 samples per class, 7 classes, 16 fused features. Sufficient?

<strong>Solution:</strong>
Samples-per-feature ratio: $100/16 = 6.25$, below the recommended 10–30. Options: reduce features via selection/PCA, collect more samples (≥160–480 per class), or use a simpler classifier.
</details>

<details>
<summary><strong>Exercise 24: SAR texture features</strong></summary>

Write Python code to compute GLCM texture from SAR for a fused stack.

<strong>Solution:</strong>

```python
from skimage.feature import grayscale_coprops, grayscale_comatrix
import numpy as np

def compute_sar_texture(sar_band, window_size=7):
    """Compute GLCM contrast and homogeneity from SAR."""
    lo, hi = np.nanpercentile(sar_band, [2, 98])
    quantized = np.clip((sar_band - lo) / (hi - lo) * 63, 0, 63).astype(np.uint8)
    pad = window_size // 2
    rows, cols = sar_band.shape
    contrast = np.zeros_like(sar_band)

    for i in range(pad, rows - pad):
        for j in range(pad, cols - pad):
            win = quantized[i-pad:i+pad+1, j-pad:j+pad+1]
            glcm = grayscale_comatrix(win, [1], [0], 64, True, True)
            contrast[i, j] = grayscale_coprops(glcm, 'contrast')[0, 0]
    return contrast
```
</details>

<details>
<summary><strong>Exercise 25: Wetland classification scheme</strong></summary>

Design a fusion-based wetland classification for the Amu Darya delta.

<strong>Solution:</strong>
(1) Open water: low VV + low NDVI. (2) Emergent marsh: high VH + moderate NDVI. (3) Wet meadow: moderate VV + high NDVI. (4) Salt flat: moderate VV + very low NDVI + high SWIR. (5) Reed bed: very high VH + high NDVI. (6) Riparian forest: high VH + high NDVI + red-edge profile. SAR is critical for classes 1, 2, 5; optical for 3, 4, 6.
</details>

<details>
<summary><strong>Exercise 26: Accuracy upper bound</strong></summary>

SAR-only OA = 72%, optical-only = 81%. Estimate fused accuracy using $OA_{fused} \approx 1 - (1 - OA_S)(1 - OA_O)$.

<strong>Solution:</strong>
$1 - (0.28)(0.19) = 1 - 0.053 = 94.7\%$. This assumes independent errors—when SAR misclassifies, optical gets it right. In practice, errors are partially correlated, so actual fused accuracy is lower (typically 85–90%). The formula gives an upper bound.
</details>

<details>
<summary><strong>Exercise 27: Time-series interpolation</strong></summary>

Write code to create a daily harmonized NDVI from optical and SAR-predicted values.

<strong>Solution:</strong>

```python
from scipy.interpolate import interp1d
import numpy as np

def harmonize_timeseries(dates_opt, ndvi_opt, dates_sar, ndvi_sar_pred):
    """Merge optical and SAR-predicted NDVI into daily series."""
    all_dates = np.concatenate([dates_opt, dates_sar])
    all_ndvi = np.concatenate([ndvi_opt, ndvi_sar_pred])
    order = np.argsort(all_dates)
    ud, idx = np.unique(all_dates[order], return_index=True)
    f = interp1d(ud, all_ndvi[order][idx], kind='linear', fill_value='extrapolate')
    daily = np.arange(ud[0], ud[-1] + 1)
    return daily, np.clip(f(daily), -0.1, 1.0)
```
</details>

<details>
<summary><strong>Exercise 28: Change detection</strong></summary>

Detect illegal construction in a protected wetland using two-date SAR-optical fusion.

<strong>Solution:</strong>
Compute $\Delta VV = VV_{after} - VV_{before}$ (construction creates double-bounce: positive) and $\Delta NDVI = NDVI_{after} - NDVI_{before}$ (vegetation removal: negative). Pixels with positive $\Delta VV$ AND negative $\Delta NDVI$ are high-confidence construction. This cross-sensor logic reduces false alarms compared to either sensor alone.
</details>

<details>
<summary><strong>Exercise 29: Regional scaling</strong></summary>

Describe a GEE workflow to classify all irrigated cropland across Central Asia using fused data.

<strong>Solution:</strong>

```python
def classify_region(aoi, year):
    composites = monthly_fused_composites(s1, s2, year, aoi)
    features = extract_temporal_features(composites)
    training = features.sampleRegions(reference_points, ['class'], 10)
    classifier = ee.Classifier.smileRandomForest(300).train(
        training, 'class', features.bandNames())
    classified = features.classify(classifier)
    ee.batch.Export.image.toDrive(
        image=classified, region=aoi, scale=10, maxPixels=1e13).start()
```

Split Central Asia into tiles if the region exceeds GEE memory limits. GEE handles parallel computation.
</details>

<details>
<summary><strong>Exercise 30: Full pipeline design</strong></summary>

Design a complete fusion pipeline for land degradation mapping in the Aral Sea basin.

<strong>Solution:</strong>
**Sensors**: Sentinel-1 VV/VH ascending 12-day; Sentinel-2 10 bands cloud-masked. **Preprocessing**: SAR—calibration, Lee filter 5×5, SRTM terrain correction. Optical—atmospheric correction, QA60 cloud mask. **Features** (~40): monthly VV/VH mean/std, NDVI/NDWI/BSI monthly, annual NDVI trend, VV seasonal amplitude. **Classifier**: Random Forest (300 trees) with spatial block CV. **Training**: 150+ samples across 5 degradation levels. **Validation**: 30% spatial holdout; report OA, kappa, per-class F1. **Outputs**: annual degradation maps 2017–2024, change trajectories, area statistics per administrative unit.
</details>
