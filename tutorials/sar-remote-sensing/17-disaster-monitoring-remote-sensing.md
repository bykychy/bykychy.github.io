---
layout: tutorial
title: "How Remote Sensing Monitors Natural Disasters in Central Asia"
description: "A practical tutorial on earthquake damage assessment, flood mapping, landslide detection, and wildfire monitoring using SAR and optical data, with 30 exercises and solutions."
difficulty: intermediate
estimated_time: "3-4 hours"
series: sar-remote-sensing
order: 17
---

## Why this tutorial matters

Central Asia sits at the intersection of multiple natural hazard zones. The Tien Shan and Pamir mountains generate earthquakes, landslides, and glacial lake outburst floods. The Aral Sea basin experiences dust storms. River systems flood annually, while summers bring wildfires.

When disaster strikes, hours matter. Emergency responders need damage assessments before roads are cleared. Remote sensing provides critical information when ground access is impossible.

<div class="info-box tip">
  <strong>Key idea</strong>
  The value of disaster remote sensing is measured in response time. A damage map delivered 6 hours after an earthquake saves more lives than a perfect map delivered 6 days later.
</div>

<div class="learning-objectives">
  <div class="learning-objectives-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#1e4f8a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <h3>What you will learn</h3>
  </div>
  <ul>
    <li>Map earthquake building damage using SAR coherence loss and damage proxy map classification (moderate/severe thresholds)</li>
    <li>Detect flood extent from SAR backscatter thresholds and change detection between pre-event and flood-event imagery</li>
    <li>Identify landslide-affected slopes through coherence loss, amplitude change, and DEM-derived susceptibility indicators</li>
    <li>Monitor glacial lake outburst floods (GLOFs) and wildfire burn areas using multi-temporal SAR and optical indices</li>
    <li>Design rapid-response mapping workflows with pre-event baselines and integration into emergency response systems</li>
  </ul>
</div>

<div class="prerequisites">
  <div class="prerequisites-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#8b5e00" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
    <h3>Prerequisites</h3>
  </div>
  <ul>
    <li>Understanding of SAR backscatter fundamentals and coherence computation from earlier tutorials in this series</li>
    <li>Basic Python programming for image processing (NumPy array operations, thresholding, filtering)</li>
    <li>Familiarity with change detection concepts — comparing pre-event and post-event imagery</li>
    <li>Awareness of Central Asia's natural hazard context (seismic zones, river systems, mountain terrain)</li>
  </ul>
</div>

<div class="concept-diagram">
  <svg viewBox="0 0 620 320" xmlns="http://www.w3.org/2000/svg" style="max-width: 580px;">
    <text x="310" y="18" font-family="Inter, sans-serif" font-size="13" fill="#111" text-anchor="middle" font-weight="bold">Multi-Hazard Disaster Monitoring with Remote Sensing</text>
    <!-- Earthquake panel -->
    <rect x="15" y="35" width="185" height="130" rx="5" fill="#fce8e7" stroke="#d92b1f" stroke-width="1.2"/>
    <text x="107" y="52" font-family="Inter, sans-serif" font-size="11" fill="#d92b1f" text-anchor="middle" font-weight="bold">Earthquake Damage</text>
    <!-- Before coherence (high) -->
    <rect x="28" y="62" width="70" height="45" rx="2" fill="#fff" stroke="#68625b" stroke-width="0.5"/>
    <text x="63" y="73" font-family="Inter, sans-serif" font-size="8" fill="#111" text-anchor="middle">Pre-event</text>
    <rect x="35" y="77" width="12" height="12" fill="#1e4f8a" opacity="0.8"/><rect x="49" y="77" width="12" height="12" fill="#1e4f8a" opacity="0.7"/><rect x="63" y="77" width="12" height="12" fill="#1e4f8a" opacity="0.9"/>
    <rect x="35" y="91" width="12" height="12" fill="#1e4f8a" opacity="0.75"/><rect x="49" y="91" width="12" height="12" fill="#1e4f8a" opacity="0.85"/><rect x="63" y="91" width="12" height="12" fill="#1e4f8a" opacity="0.8"/>
    <!-- After coherence (low) -->
    <rect x="112" y="62" width="70" height="45" rx="2" fill="#fff" stroke="#68625b" stroke-width="0.5"/>
    <text x="147" y="73" font-family="Inter, sans-serif" font-size="8" fill="#111" text-anchor="middle">Post-event</text>
    <rect x="119" y="77" width="12" height="12" fill="#1e4f8a" opacity="0.7"/><rect x="133" y="77" width="12" height="12" fill="#d92b1f" opacity="0.7"/><rect x="147" y="77" width="12" height="12" fill="#d92b1f" opacity="0.9"/>
    <rect x="119" y="91" width="12" height="12" fill="#1e4f8a" opacity="0.6"/><rect x="133" y="91" width="12" height="12" fill="#d92b1f" opacity="0.8"/><rect x="147" y="91" width="12" height="12" fill="#d92b1f" opacity="0.6"/>
    <text x="63" y="118" font-family="Inter, sans-serif" font-size="8" fill="#1e4f8a" text-anchor="middle">High coherence</text>
    <text x="147" y="118" font-family="Inter, sans-serif" font-size="8" fill="#d92b1f" text-anchor="middle">Coherence loss</text>
    <!-- Arrow between -->
    <text x="102" y="92" font-family="Inter, sans-serif" font-size="12" fill="#68625b" text-anchor="middle">→</text>
    <text x="107" y="148" font-family="Inter, sans-serif" font-size="9" fill="#68625b" text-anchor="middle" font-style="italic">Damage proxy map</text>
    <!-- Flood panel -->
    <rect x="215" y="35" width="185" height="130" rx="5" fill="#e8f0fa" stroke="#1e4f8a" stroke-width="1.2"/>
    <text x="307" y="52" font-family="Inter, sans-serif" font-size="11" fill="#1e4f8a" text-anchor="middle" font-weight="bold">Flood Mapping</text>
    <!-- SAR image with water (dark) -->
    <rect x="228" y="62" width="70" height="55" rx="2" fill="#fff" stroke="#68625b" stroke-width="0.5"/>
    <text x="263" y="73" font-family="Inter, sans-serif" font-size="8" fill="#111" text-anchor="middle">Pre-flood</text>
    <rect x="235" y="77" width="50" height="35" fill="#68625b" opacity="0.3"/>
    <path d="M240,95 Q255,90 270,95" fill="none" stroke="#1e4f8a" stroke-width="1"/>
    <rect x="312" y="62" width="70" height="55" rx="2" fill="#fff" stroke="#68625b" stroke-width="0.5"/>
    <text x="347" y="73" font-family="Inter, sans-serif" font-size="8" fill="#111" text-anchor="middle">During flood</text>
    <rect x="319" y="77" width="50" height="35" fill="#68625b" opacity="0.3"/>
    <rect x="319" y="87" width="50" height="25" fill="#1e4f8a" opacity="0.5"/>
    <text x="263" y="130" font-family="Inter, sans-serif" font-size="8" fill="#68625b" text-anchor="middle">Normal</text>
    <text x="347" y="130" font-family="Inter, sans-serif" font-size="8" fill="#1e4f8a" text-anchor="middle">Dark = water</text>
    <!-- Arrow -->
    <text x="300" y="97" font-family="Inter, sans-serif" font-size="12" fill="#68625b" text-anchor="middle">→</text>
    <text x="307" y="148" font-family="Inter, sans-serif" font-size="9" fill="#68625b" text-anchor="middle" font-style="italic">Flood extent map</text>
    <!-- Landslide panel -->
    <rect x="415" y="35" width="185" height="130" rx="5" fill="#f0f7f2" stroke="#165d34" stroke-width="1.2"/>
    <text x="507" y="52" font-family="Inter, sans-serif" font-size="11" fill="#165d34" text-anchor="middle" font-weight="bold">Landslide Detection</text>
    <!-- Slope with slide -->
    <polygon points="430,115 510,62 510,115" fill="#165d34" opacity="0.2" stroke="#165d34" stroke-width="0.5"/>
    <polygon points="510,62 590,115 510,115" fill="#165d34" opacity="0.15" stroke="#165d34" stroke-width="0.5"/>
    <!-- Slide scar -->
    <path d="M475,82 Q490,78 500,85 Q505,90 495,95 Q485,92 475,82Z" fill="#8b5e00" opacity="0.6" stroke="#8b5e00" stroke-width="0.5"/>
    <text x="487" y="108" font-family="Inter, sans-serif" font-size="8" fill="#8b5e00" text-anchor="middle">Slide scar</text>
    <!-- Coherence bars -->
    <rect x="530" y="70" width="8" height="35" fill="#165d34" opacity="0.6"/>
    <rect x="545" y="85" width="8" height="20" fill="#d92b1f" opacity="0.6"/>
    <text x="534" y="115" font-family="Inter, sans-serif" font-size="7" fill="#165d34" text-anchor="middle">Pre</text>
    <text x="549" y="115" font-family="Inter, sans-serif" font-size="7" fill="#d92b1f" text-anchor="middle">Post</text>
    <text x="542" y="130" font-family="Inter, sans-serif" font-size="8" fill="#68625b" text-anchor="middle">Coherence</text>
    <text x="507" y="148" font-family="Inter, sans-serif" font-size="9" fill="#68625b" text-anchor="middle" font-style="italic">Slope failure map</text>
    <!-- Bottom: Rapid Response Pipeline -->
    <rect x="30" y="180" width="560" height="55" rx="5" fill="#f5f5f0" stroke="#68625b" stroke-width="1"/>
    <text x="310" y="198" font-family="Inter, sans-serif" font-size="11" fill="#111" text-anchor="middle" font-weight="bold">Rapid Response Pipeline</text>
    <!-- Pipeline steps -->
    <rect x="48" y="206" width="90" height="22" rx="3" fill="#1e4f8a" opacity="0.15" stroke="#1e4f8a" stroke-width="0.5"/>
    <text x="93" y="220" font-family="Inter, sans-serif" font-size="9" fill="#1e4f8a" text-anchor="middle">Pre-event baseline</text>
    <text x="148" y="220" font-family="Inter, sans-serif" font-size="11" fill="#68625b" text-anchor="middle">→</text>
    <rect x="158" y="206" width="90" height="22" rx="3" fill="#d92b1f" opacity="0.15" stroke="#d92b1f" stroke-width="0.5"/>
    <text x="203" y="220" font-family="Inter, sans-serif" font-size="9" fill="#d92b1f" text-anchor="middle">Event detection</text>
    <text x="258" y="220" font-family="Inter, sans-serif" font-size="11" fill="#68625b" text-anchor="middle">→</text>
    <rect x="268" y="206" width="90" height="22" rx="3" fill="#8b5e00" opacity="0.15" stroke="#8b5e00" stroke-width="0.5"/>
    <text x="313" y="220" font-family="Inter, sans-serif" font-size="9" fill="#8b5e00" text-anchor="middle">Change analysis</text>
    <text x="368" y="220" font-family="Inter, sans-serif" font-size="11" fill="#68625b" text-anchor="middle">→</text>
    <rect x="378" y="206" width="90" height="22" rx="3" fill="#165d34" opacity="0.15" stroke="#165d34" stroke-width="0.5"/>
    <text x="423" y="220" font-family="Inter, sans-serif" font-size="9" fill="#165d34" text-anchor="middle">Damage map</text>
    <text x="478" y="220" font-family="Inter, sans-serif" font-size="11" fill="#68625b" text-anchor="middle">→</text>
    <rect x="488" y="206" width="90" height="22" rx="3" fill="#68625b" opacity="0.15" stroke="#68625b" stroke-width="0.5"/>
    <text x="533" y="220" font-family="Inter, sans-serif" font-size="9" fill="#68625b" text-anchor="middle">Emergency alert</text>
    <!-- Timeline -->
    <text x="310" y="260" font-family="Inter, sans-serif" font-size="10" fill="#d92b1f" text-anchor="middle" font-style="italic">Target: actionable damage assessment within 6–12 hours of event</text>
    <!-- Central Asia context -->
    <rect x="80" y="275" width="460" height="35" rx="4" fill="#fdf6e3" stroke="#8b5e00" stroke-width="0.5"/>
    <text x="310" y="290" font-family="Inter, sans-serif" font-size="9" fill="#8b5e00" text-anchor="middle">Central Asia hazards: Tien Shan earthquakes · Syr Darya floods · Pamir landslides · Aral dust storms · Glacial lake outbursts</text>
    <text x="310" y="303" font-family="Inter, sans-serif" font-size="9" fill="#8b5e00" text-anchor="middle">SAR advantage: all-weather, day/night imaging when optical sensors are blocked by clouds or darkness</text>
  </svg>
  <p class="diagram-caption">Multi-hazard disaster monitoring framework: SAR coherence loss detects earthquake damage, backscatter thresholds map flood extent, and slope coherence analysis identifies landslides — all feeding a rapid-response pipeline.</p>
</div>

## Earthquake damage mapping

### Coherence loss detection

SAR coherence measures signal similarity between acquisitions. Buildings maintain high coherence because their structure is stable. When earthquakes damage buildings, coherence drops dramatically.

```python
import numpy as np
from scipy.ndimage import uniform_filter

def compute_coherence(master, slave, window=5):
    cross = master * np.conj(slave)
    cross_mean = uniform_filter(cross.real, window) + 1j * uniform_filter(cross.imag, window)
    master_power = uniform_filter(np.abs(master)**2, window)
    slave_power = uniform_filter(np.abs(slave)**2, window)
    coherence = np.abs(cross_mean) / np.sqrt(master_power * slave_power + 1e-10)
    return np.clip(coherence, 0, 1)

def detect_damage(pre_coh, post_coh, threshold=0.3):
    stable = pre_coh > 0.6
    drop = pre_coh - post_coh
    return stable & (drop > threshold), drop
```

### Building damage proxy mapping

The damage proxy map methodology classifies severity based on coherence change statistics.

```python
def create_damage_proxy_map(pre_coh, co_coh, urban_mask):
    delta = pre_coh - co_coh
    dpm = np.zeros_like(delta, dtype=np.uint8)
    valid = urban_mask & (pre_coh > 0.5)
    dpm[valid & (delta > 0.20)] = 1  # Moderate
    dpm[valid & (delta > 0.35)] = 2  # Severe
    return dpm
```

<div class="info-box tip">
  <strong>Key idea</strong>
  Coherence loss detection requires pre-event baselines. Urban areas and bare rock maintain high coherence, making damage detection most reliable there.
</div>

## Flood mapping with SAR

Water surfaces appear dark in SAR imagery because smooth water reflects radar energy away from the sensor.

### SAR water detection

```python
def detect_water(sar_db, threshold=-15, permanent_mask=None):
    water = sar_db < threshold
    if permanent_mask is not None:
        return water & ~permanent_mask
    return water

def flood_change_detection(pre_db, flood_db, change_thresh=-3, abs_thresh=-12):
    change = flood_db - pre_db
    flood = (change < change_thresh) & (flood_db < abs_thresh)
    return flood, change
```

### Flood depth estimation

```python
def estimate_depth(flood_mask, dem):
    from scipy.ndimage import binary_dilation, binary_erosion
    boundary = binary_dilation(flood_mask) & ~binary_erosion(flood_mask)
    water_surface = np.median(dem[boundary])
    depth = np.zeros_like(dem)
    depth[flood_mask] = np.maximum(water_surface - dem[flood_mask], 0)
    return depth, water_surface
```

<div class="info-box tip">
  <strong>Key idea</strong>
  Wind roughens water surfaces, increasing backscatter. Calm conditions immediately after flooding provide the best mapping opportunity.
</div>

## Landslide detection

The steep terrain of the Tien Shan and Pamir ranges generates frequent landslides. Earthquakes trigger widespread slope failures. Intense rainfall saturates hillsides. Glacial retreat destabilizes moraines. Remote sensing detects both sudden and gradual slope movements through multiple techniques.

### DInSAR for slope deformation

Differential InSAR measures surface displacement between acquisitions. Slow-moving landslides show systematic phase patterns indicating centimeters to meters of annual movement. The line-of-sight displacement captures the component of motion toward or away from the satellite.

<div class="info-box tip">
  <strong>Key idea</strong>
  InSAR measures displacement only in the line-of-sight direction. A landslide moving horizontally across the radar look direction may show minimal signal. Combining ascending and descending passes helps recover full 3D motion.
</div>

```python
def dinsar_displacement(master, slave, wavelength=0.055):
    interferogram = master * np.conj(slave)
    phase = np.angle(interferogram)
    displacement = (phase * wavelength) / (4 * np.pi)
    return displacement

def detect_rapid_landslides(pre_coh, post_coh, slope, slope_thresh=15):
    coh_loss = pre_coh - post_coh
    steep = slope > slope_thresh
    return steep & (coh_loss > 0.4) & (pre_coh > 0.5)
```

### Optical change detection

```python
def ndvi_change_landslide(pre_nir, pre_red, post_nir, post_red, thresh=-0.3):
    pre_ndvi = (pre_nir - pre_red) / (pre_nir + pre_red + 1e-10)
    post_ndvi = (post_nir - post_red) / (post_nir + post_red + 1e-10)
    change = post_ndvi - pre_ndvi
    return (pre_ndvi > 0.3) & (change < thresh), change
```

## Wildfire monitoring

### Burn severity indices

```python
def calculate_nbr(nir, swir2):
    return (nir - swir2) / (nir + swir2 + 1e-10)

def classify_burn_severity(dnbr):
    severity = np.zeros_like(dnbr, dtype=np.uint8)
    severity[(dnbr >= 0.1) & (dnbr < 0.27)] = 1   # Low
    severity[(dnbr >= 0.27) & (dnbr < 0.44)] = 2  # Moderate-low
    severity[(dnbr >= 0.44) & (dnbr < 0.66)] = 3  # Moderate-high
    severity[dnbr >= 0.66] = 4                     # High
    return severity
```

<div class="info-box tip">
  <strong>Key idea</strong>
  Post-fire imagery should be acquired soon after fire but after smoke clears. Waiting too long allows vegetation regrowth.
</div>

## Glacial lake outburst floods (GLOFs)

Central Asia's glaciers create lakes dammed by ice or moraine. Remote sensing monitors lake growth and dam stability.

```python
def monitor_lake_area(masks, dates, pixel_size_m=10):
    import pandas as pd
    areas = [{'date': d, 'area_m2': np.sum(m) * pixel_size_m**2} 
             for m, d in zip(masks, dates)]
    df = pd.DataFrame(areas)
    df['expansion_rate'] = df['area_m2'].diff() / df['date'].diff().dt.days
    return df
```

## Dust storms and atmospheric hazards

The Aral Sea disaster created massive dust sources. Remote sensing tracks dust transport.

```python
def detect_dust(blue, red, swir):
    bright = (blue + red) / 2 > 0.3
    ratio = (blue - red) / (blue + red + 1e-10)
    dust_sig = (ratio > 0.05) & (ratio < 0.4)
    not_cloud = swir < 0.4
    return bright & dust_sig & not_cloud
```

## Pre-event baseline data preparation

Effective disaster response requires pre-existing baseline data.

```python
def create_baseline_stack(slc_archive, output_dir):
    seasonal_pairs = {'winter': [], 'spring': [], 'summer': [], 'fall': []}
    for slc in slc_archive:
        season = get_season(extract_date(slc))
        seasonal_pairs[season].append(slc)
    # Process each season's coherence baselines
    return output_dir
```

## Rapid mapping workflows

```python
RESPONSE_TIMELINE_HOURS = {
    'international_charter': {'first_map': 24, 'detailed': 72},
    'copernicus_ems': {'rush': 8, 'standard': 24}
}

def assess_timeline(event_time, target='copernicus_ems'):
    from datetime import datetime
    elapsed = (datetime.now() - event_time).total_seconds() / 3600
    targets = RESPONSE_TIMELINE_HOURS[target]
    return {k: v - elapsed for k, v in targets.items()}
```

## Integration with emergency response systems

```python
def export_for_responders(damage_map, stats):
    return {
        'geojson': export_geojson(damage_map),
        'pdf_map': create_pdf_map(damage_map),
        'kml': export_kml(damage_map),
        'statistics': stats
    }
```

## Disaster monitoring checklist

<ul class="checklist">
  <li>Maintain pre-event baseline imagery for all monitored regions</li>
  <li>Pre-compute seasonal coherence baselines for earthquake damage detection</li>
  <li>Establish permanent water body masks for flood change detection</li>
  <li>Create urban area masks from OpenStreetMap or building databases</li>
  <li>Configure automated alerts for new Sentinel-1 acquisitions</li>
  <li>Test processing workflows before disaster events occur</li>
  <li>Document all processing parameters for reproducibility</li>
  <li>Validate detection thresholds using historical events</li>
  <li>Establish communication protocols with emergency responders</li>
  <li>Practice rapid mapping exercises to maintain readiness</li>
  <li>Archive all disaster response products with metadata</li>
  <li>Monitor glacial lakes quarterly during melt season</li>
</ul>

## Exercises

### Exercise 1: Coherence calculation
Compute coherence between two complex SAR images.

<details>
<summary>Show solution</summary>

```python
from scipy.ndimage import uniform_filter
master = np.random.randn(500, 500) + 1j * np.random.randn(500, 500)
slave = master * (0.8 + 0.2 * np.random.randn(500, 500))
window = 5
cross = uniform_filter((master * np.conj(slave)).real, window) + \
        1j * uniform_filter((master * np.conj(slave)).imag, window)
mp = uniform_filter(np.abs(master)**2, window)
sp = uniform_filter(np.abs(slave)**2, window)
coh = np.abs(cross) / np.sqrt(mp * sp + 1e-10)
print(f"Mean coherence: {coh.mean():.3f}")
```
</details>

### Exercise 2: Damage proxy map
Create a three-class damage proxy map from coherence change.

<details>
<summary>Show solution</summary>

```python
def classify_damage(coh_change, urban):
    dpm = np.zeros_like(coh_change, dtype=np.uint8)
    valid = urban & ~np.isnan(coh_change)
    dpm[valid & (coh_change > 0.2) & (coh_change <= 0.35)] = 1
    dpm[valid & (coh_change > 0.35)] = 2
    return dpm
```
</details>

### Exercise 3: Water detection threshold
Determine optimal SAR water detection threshold.

<details>
<summary>Show solution</summary>

```python
from skimage.filters import threshold_otsu
land = np.random.normal(-8, 2, 5000)
water = np.random.normal(-18, 3, 2000)
threshold = threshold_otsu(np.concatenate([land, water]))
print(f"Optimal threshold: {threshold:.2f} dB")
```
</details>

### Exercise 4: Flood change detection
Detect flooding by comparing pre-flood and during-flood SAR.

<details>
<summary>Show solution</summary>

```python
pre = np.random.normal(-10, 3, (500, 500))
during = pre.copy()
during[100:200, 150:300] = np.random.normal(-20, 2, (100, 150))
change = during - pre
flood = (change < -4) & (during < -14)
print(f"Flooded pixels: {np.sum(flood)}")
```
</details>

### Exercise 5: Flood depth estimation
Estimate flood depth using DEM and flood boundary.

<details>
<summary>Show solution</summary>

```python
from scipy.ndimage import binary_dilation, binary_erosion
dem = np.random.rand(500, 500) * 20 + 100
flood = dem < 108
boundary = binary_dilation(flood) & ~binary_erosion(flood)
ws = np.median(dem[boundary])
depth = np.maximum(ws - dem, 0) * flood
print(f"Max depth: {depth.max():.1f}m")
```
</details>

### Exercise 6: NDVI landslide detection
Detect landslide scars from NDVI change.

<details>
<summary>Show solution</summary>

```python
pre_nir, pre_red = np.random.rand(500, 500) * 0.4 + 0.3, np.random.rand(500, 500) * 0.1
post_nir, post_red = pre_nir.copy(), pre_red.copy()
post_nir[200:250, 300:350] = 0.15
pre_ndvi = (pre_nir - pre_red) / (pre_nir + pre_red)
post_ndvi = (post_nir - post_red) / (post_nir + post_red)
landslide = (pre_ndvi > 0.3) & ((post_ndvi - pre_ndvi) < -0.3)
print(f"Landslide pixels: {np.sum(landslide)}")
```
</details>

### Exercise 7: Slope filtering
Filter landslide candidates to steep slopes only.

<details>
<summary>Show solution</summary>

```python
detection = np.random.rand(500, 500) > 0.9
slope = np.random.rand(500, 500) * 70
filtered = detection & (slope >= 15) & (slope <= 60)
print(f"After filtering: {np.sum(filtered)}")
```
</details>

### Exercise 8: NBR calculation
Calculate Normalized Burn Ratio.

<details>
<summary>Show solution</summary>

```python
nir = np.random.rand(500, 500) * 0.4 + 0.2
swir2 = np.random.rand(500, 500) * 0.2 + 0.1
nbr = (nir - swir2) / (nir + swir2 + 1e-10)
print(f"NBR range: {nbr.min():.3f} to {nbr.max():.3f}")
```
</details>

### Exercise 9: Burn severity classification
Classify burn severity from dNBR.

<details>
<summary>Show solution</summary>

```python
dnbr = np.random.rand(500, 500) * 0.8
severity = np.zeros_like(dnbr, dtype=np.uint8)
severity[(dnbr >= 0.1) & (dnbr < 0.27)] = 1
severity[(dnbr >= 0.27) & (dnbr < 0.44)] = 2
severity[(dnbr >= 0.44) & (dnbr < 0.66)] = 3
severity[dnbr >= 0.66] = 4
print(f"High severity: {np.sum(severity == 4)} pixels")
```
</details>

### Exercise 10: Active fire detection
Detect active fires from thermal anomalies.

<details>
<summary>Show solution</summary>

```python
thermal = np.random.normal(295, 5, (500, 500))
thermal[200:210, 300:310] = 400
fires = thermal > 330
print(f"Fire pixels: {np.sum(fires)}")
```
</details>

### Exercise 11: Glacial lake tracking
Calculate lake area change over time.

<details>
<summary>Show solution</summary>

```python
import pandas as pd
from datetime import datetime, timedelta
dates = [datetime(2023, 1, 1) + timedelta(days=30*i) for i in range(12)]
masks = [np.random.rand(100, 100) > (0.7 - i*0.02) for i in range(12)]
areas = [{'date': d, 'area_ha': np.sum(m) * 100 / 10000} for m, d in zip(masks, dates)]
print(pd.DataFrame(areas))
```
</details>

### Exercise 12: Dust storm detection
Detect dust using spectral bands.

<details>
<summary>Show solution</summary>

```python
blue = np.random.rand(500, 500) * 0.3 + 0.2
red = np.random.rand(500, 500) * 0.25 + 0.15
swir = np.random.rand(500, 500) * 0.2
bright = (blue + red) / 2 > 0.3
ratio = (blue - red) / (blue + red)
dust = bright & (ratio > 0.05) & (ratio < 0.4) & (swir < 0.4)
print(f"Dust pixels: {np.sum(dust)}")
```
</details>

### Exercise 13: Multi-temporal flood probability
Calculate flood probability from time series.

<details>
<summary>Show solution</summary>

```python
stack = np.random.normal(-10, 2, (20, 500, 500))
stack[15, 100:200, 100:200] = -22
baseline = stack[:15]
mean_b, std_b = baseline.mean(axis=0), baseline.std(axis=0) + 0.1
z = (stack[15] - mean_b) / std_b
prob = 1 / (1 + np.exp(z + 2))
print(f"High probability pixels: {np.sum(prob > 0.9)}")
```
</details>

### Exercise 14: Displacement velocity
Calculate annual velocity from InSAR stack.

<details>
<summary>Show solution</summary>

```python
from datetime import datetime, timedelta
dates = [datetime(2023, 1, 1) + timedelta(days=12*i) for i in range(30)]
disp = np.zeros((30, 500, 500))
for i in range(30):
    disp[i, 200:250, 200:250] = -i * 0.002
days = np.array([(d - dates[0]).days for d in dates])
t = days - days.mean()
slope = np.sum(t[:, None, None] * disp, axis=0) / np.sum(t**2)
vel = slope * 365
print(f"Max velocity: {vel.min():.1f} mm/year")
```
</details>

### Exercise 15: Building damage statistics
Calculate damage by administrative unit.

<details>
<summary>Show solution</summary>

```python
damage = np.random.randint(0, 3, (500, 500))
stats = {'total': damage.size, 'severe': np.sum(damage == 2), 
         'moderate': np.sum(damage == 1)}
stats['pct_severe'] = stats['severe'] / stats['total'] * 100
print(stats)
```
</details>

### Exercise 16: Baseline selection
Select optimal pre-event image pair.

<details>
<summary>Show solution</summary>

```python
from datetime import datetime, timedelta
event = datetime(2024, 3, 15)
available = [datetime(2024, 1, 1) + timedelta(days=12*i) for i in range(10)]
pre = [d for d in available if d < event]
if len(pre) >= 2:
    print(f"Baseline: {pre[-2].date()} to {pre[-1].date()}")
```
</details>

### Exercise 17: Response timeline
Check if mapping timeline is achievable.

<details>
<summary>Show solution</summary>

```python
from datetime import datetime
event = datetime(2024, 3, 15, 6, 30)
now = datetime(2024, 3, 15, 14, 0)
elapsed = (now - event).total_seconds() / 3600
remaining = 24 - elapsed
print(f"Elapsed: {elapsed:.1f}h, Remaining: {remaining:.1f}h")
```
</details>

### Exercise 18: Burn area metrics
Calculate burned area and perimeter.

<details>
<summary>Show solution</summary>

```python
from scipy.ndimage import binary_erosion
burn = np.zeros((500, 500), dtype=bool)
burn[100:300, 150:350] = True
area_ha = np.sum(burn) * 100 / 10000
boundary = burn & ~binary_erosion(burn)
perim_km = np.sum(boundary) * 10 / 1000
print(f"Area: {area_ha:.1f} ha, Perimeter: {perim_km:.2f} km")
```
</details>

### Exercise 19: Population exposure
Estimate affected population.

<details>
<summary>Show solution</summary>

```python
hazard = np.random.rand(500, 500) > 0.8
pop = np.random.exponential(100, (500, 500))
affected = int(np.sum(pop[hazard]))
print(f"Affected population: {affected:,}")
```
</details>

### Exercise 20: Multi-hazard zones
Identify areas affected by multiple hazards.

<details>
<summary>Show solution</summary>

```python
flood = np.random.rand(500, 500) > 0.9
landslide = np.random.rand(500, 500) > 0.95
fire = np.random.rand(500, 500) > 0.92
count = flood.astype(int) + landslide.astype(int) + fire.astype(int)
print(f"Multi-hazard: {np.sum(count >= 2)} pixels")
```
</details>

### Exercise 21: Validation sampling
Generate stratified validation points.

<details>
<summary>Show solution</summary>

```python
damage = np.random.randint(0, 3, (500, 500))
points = []
for cls in [0, 1, 2]:
    coords = np.argwhere(damage == cls)
    idx = np.random.choice(len(coords), min(30, len(coords)), replace=False)
    for r, c in coords[idx]:
        points.append({'row': r, 'col': c, 'class': cls})
print(f"Generated {len(points)} points")
```
</details>

### Exercise 22: Baseline composite
Create cloud-free composite.

<details>
<summary>Show solution</summary>

```python
stack = np.random.rand(10, 500, 500)
clouds = np.random.rand(10, 500, 500) > 0.8
masked = np.ma.array(stack, mask=clouds)
composite = np.ma.median(masked, axis=0).filled(np.nan)
print(f"Coverage: {np.sum(~np.isnan(composite)) / composite.size * 100:.1f}%")
```
</details>

### Exercise 23: GLOF breach model
Simple dam breach parameters.

<details>
<summary>Show solution</summary>

```python
volume, height, slope = 5e6, 30, 0.05
peak_q = 0.65 * np.sqrt(9.81) * height**1.5 * (volume / 1e6)**0.4
print(f"Peak discharge: {peak_q:.0f} m³/s")
```
</details>

### Exercise 24: Intensity correlation
Compute local intensity correlation.

<details>
<summary>Show solution</summary>

```python
from scipy.ndimage import uniform_filter
pre = np.random.rand(500, 500)
post = pre.copy()
post[200:300, 200:300] = np.random.rand(100, 100)
pre_m, post_m = uniform_filter(pre, 15), uniform_filter(post, 15)
cross = uniform_filter((pre - pre_m) * (post - post_m), 15)
pre_v = uniform_filter((pre - pre_m)**2, 15)
post_v = uniform_filter((post - post_m)**2, 15)
corr = cross / (np.sqrt(pre_v * post_v) + 1e-10)
print(f"Low correlation: {np.sum(corr < 0.5)} pixels")
```
</details>

### Exercise 25: Flood recurrence
Analyze flood frequency from multi-year data.

<details>
<summary>Show solution</summary>

```python
masks = [np.random.rand(500, 500) > 0.85 for _ in range(20)]
count = np.sum(np.stack(masks), axis=0)
recurrence = np.zeros_like(count, dtype=float)
recurrence[count > 0] = 20 / count[count > 0]
print(f"Annual flood zone: {np.sum(recurrence == 1)} pixels")
```
</details>

### Exercise 26: Alert generation
Generate CAP-format alert.

<details>
<summary>Show solution</summary>

```python
from datetime import datetime
alert = {
    'id': f"ALERT-{datetime.now().strftime('%Y%m%d%H%M%S')}",
    'event': 'Flood', 'severity': 'Severe',
    'area_km2': 150.5
}
print(f"Alert: {alert['id']}, {alert['event']}")
```
</details>

### Exercise 27: Threshold sensitivity
Analyze detection sensitivity.

<details>
<summary>Show solution</summary>

```python
image = np.random.normal(-10, 5, (500, 500))
truth = image < -15
results = []
for t in range(-20, -5):
    pred = image < t
    tp = np.sum(pred & truth)
    fp, fn = np.sum(pred & ~truth), np.sum(~pred & truth)
    f1 = 2 * tp / (2 * tp + fp + fn) if (tp + fp + fn) > 0 else 0
    results.append((t, f1))
best = max(results, key=lambda x: x[1])
print(f"Best threshold: {best[0]} (F1={best[1]:.3f})")
```
</details>

### Exercise 28: GeoJSON export
Export detection to GeoJSON.

<details>
<summary>Show solution</summary>

```python
import json
geojson = {
    'type': 'FeatureCollection',
    'features': [{
        'type': 'Feature',
        'geometry': {'type': 'Point', 'coordinates': [74.5, 42.8]},
        'properties': {'severity': 'high'}
    }]
}
print(json.dumps(geojson, indent=2))
```
</details>

### Exercise 29: Quality metrics
Calculate detection metrics.

<details>
<summary>Show solution</summary>

```python
pred = np.random.rand(500, 500) > 0.8
truth = np.random.rand(500, 500) > 0.85
tp = np.sum(pred & truth)
fp, fn = np.sum(pred & ~truth), np.sum(~pred & truth)
precision = tp / (tp + fp)
recall = tp / (tp + fn)
f1 = 2 * precision * recall / (precision + recall)
print(f"Precision: {precision:.3f}, Recall: {recall:.3f}, F1: {f1:.3f}")
```
</details>

### Exercise 30: Complete workflow
Implement end-to-end rapid mapping.

<details>
<summary>Show solution</summary>

```python
class DisasterWorkflow:
    def __init__(self, event_type):
        self.event_type = event_type
        
    def execute(self, pre, post):
        if self.event_type == 'flood':
            change = post - pre
            detection = (change < -3) & (post < -14)
        else:
            detection = np.abs(post - pre) > 0.3
        stats = {'affected_pixels': int(np.sum(detection)),
                 'area_km2': np.sum(detection) * 0.01}
        return {'detection': detection, 'stats': stats}

wf = DisasterWorkflow('flood')
pre = np.random.normal(-10, 2, (500, 500))
post = pre.copy()
post[100:200, 150:300] = np.random.normal(-18, 2, (100, 150))
result = wf.execute(pre, post)
print(f"Results: {result['stats']}")
```
</details>

## Summary

This tutorial covered disaster monitoring techniques for Central Asia:

- **Earthquakes**: Coherence loss detection and damage proxy mapping
- **Floods**: SAR water detection, change detection, depth estimation
- **Landslides**: DInSAR displacement and optical NDVI analysis
- **Wildfires**: NBR/dNBR indices and active fire detection
- **GLOFs**: Lake monitoring and dam stability assessment
- **Dust storms**: Spectral detection and plume tracking

These skills support operational disaster response. Maintain baselines, practice workflows, and be ready to contribute when needed.
