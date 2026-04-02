---
layout: tutorial
title: "How Google Earth Engine Enables Large-Scale Analysis of Central Asia"
description: "A practical tutorial on Google Earth Engine Python API, cloud computing, image collections, and scalable geospatial analysis for Central Asia, with 30 exercises and solutions."
difficulty: intermediate
estimated_time: "3-4 hours"
series: sar-remote-sensing
order: 9
---

## Why this tutorial matters

Central Asia spans over 4 million square kilometers across Kazakhstan, Uzbekistan, Turkmenistan, Kyrgyzstan, and Tajikistan. Analyzing environmental change across this vast region—from the shrinking Aral Sea to expanding irrigated agriculture to retreating glaciers—requires processing petabytes of satellite imagery. No local workstation can handle this scale.

Google Earth Engine (GEE) solves this problem by bringing computation to the data. Instead of downloading terabytes of imagery to your computer, you write code that executes on Google's servers where the data already resides. A time-series analysis that might take weeks on a local machine completes in minutes on GEE.

<div class="info-box tip">
  <strong>Key idea</strong>
  GEE inverts the traditional workflow. Rather than "download data, then process locally," you "upload code to where data lives." This paradigm shift enables analysis at continental and global scales with modest computing resources.
</div>

The platform hosts over 70 petabytes of geospatial data, including complete archives of Landsat (1972–present), Sentinel-1 and Sentinel-2 (2014–present), MODIS, climate datasets, terrain models, and land cover products. All are analysis-ready: preprocessed, georeferenced, and organized into queryable collections.

<div class="learning-objectives">
  <div class="learning-objectives-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#1e4f8a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <h3>What you will learn</h3>
  </div>
  <ul>
    <li>Authenticate and initialise the GEE Python API (ee.Initialize) for cloud-based geospatial analysis</li>
    <li>Work with core data structures: ee.Image, ee.ImageCollection, ee.Geometry, ee.Feature, ee.FeatureCollection</li>
    <li>Filter, composite, and mosaic satellite image collections over Central Asia</li>
    <li>Perform server-side map/reduce operations and understand lazy evaluation</li>
    <li>Export results (GeoTIFF, CSV, assets) and visualise with geemap and folium</li>
  </ul>
</div>

<div class="prerequisites">
  <div class="prerequisites-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#8b5e00" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
    <h3>Prerequisites</h3>
  </div>
  <ul>
    <li>Working Python 3 environment with pip/conda package management</li>
    <li>A registered Google Earth Engine account (sign up at earthengine.google.com)</li>
    <li>Basic Python programming: variables, loops, functions, list comprehensions</li>
    <li>Familiarity with raster/vector geospatial concepts (coordinate systems, bands, pixels)</li>
  </ul>
</div>

<div class="concept-diagram">
  <svg viewBox="0 0 620 320" xmlns="http://www.w3.org/2000/svg" style="max-width: 580px;">
    <!-- LOCAL MACHINE -->
    <rect x="10" y="90" width="120" height="140" rx="8" fill="#fff" stroke="#1e4f8a" stroke-width="2"/>
    <text x="70" y="115" text-anchor="middle" font-family="Inter, sans-serif" font-size="12" fill="#1e4f8a" font-weight="700">Local Machine</text>
    <rect x="30" y="128" width="80" height="28" rx="4" fill="#1e4f8a" opacity="0.1"/>
    <text x="70" y="146" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">Python script</text>
    <rect x="30" y="164" width="80" height="28" rx="4" fill="#1e4f8a" opacity="0.1"/>
    <text x="70" y="182" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">ee library</text>
    <text x="70" y="218" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">Defines operations</text>
    <!-- Arrow Local to API -->
    <line x1="130" y1="160" x2="195" y2="160" stroke="#1e4f8a" stroke-width="2"/>
    <polygon points="195,155 205,160 195,165" fill="#1e4f8a"/>
    <text x="167" y="150" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#1e4f8a">REST API</text>
    <!-- GEE API gateway -->
    <rect x="205" y="120" width="90" height="80" rx="8" fill="#fff" stroke="#165d34" stroke-width="2"/>
    <text x="250" y="148" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" fill="#165d34" font-weight="700">GEE API</text>
    <text x="250" y="165" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">Computation</text>
    <text x="250" y="178" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">graph builder</text>
    <text x="250" y="192" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">Lazy evaluation</text>
    <!-- Arrow API to Cloud -->
    <line x1="295" y1="160" x2="360" y2="160" stroke="#165d34" stroke-width="2"/>
    <polygon points="360,155 370,160 360,165" fill="#165d34"/>
    <!-- CLOUD box -->
    <rect x="370" y="40" width="240" height="240" rx="12" fill="#f7f7f5" stroke="#68625b" stroke-width="1.5" stroke-dasharray="6 3"/>
    <text x="490" y="65" text-anchor="middle" font-family="Inter, sans-serif" font-size="13" fill="#68625b" font-weight="700">Google Cloud</text>
    <!-- Data catalog -->
    <rect x="390" y="80" width="100" height="55" rx="6" fill="#fff" stroke="#8b5e00" stroke-width="1.5"/>
    <text x="440" y="100" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#8b5e00" font-weight="600">Data Catalog</text>
    <text x="440" y="114" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#111">Landsat, Sentinel</text>
    <text x="440" y="126" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#111">MODIS, DEMs…</text>
    <!-- Compute cluster -->
    <rect x="510" y="80" width="90" height="55" rx="6" fill="#fff" stroke="#d92b1f" stroke-width="1.5"/>
    <text x="555" y="100" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#d92b1f" font-weight="600">Compute</text>
    <text x="555" y="114" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#111">Map / Reduce</text>
    <text x="555" y="126" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#111">Parallel tiles</text>
    <!-- Arrow catalog to compute -->
    <line x1="490" y1="107" x2="510" y2="107" stroke="#68625b" stroke-width="1.2"/>
    <polygon points="507,103 515,107 507,111" fill="#68625b"/>
    <!-- Results box -->
    <rect x="430" y="155" width="130" height="50" rx="6" fill="#fff" stroke="#165d34" stroke-width="1.5"/>
    <text x="495" y="175" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#165d34" font-weight="600">Results</text>
    <text x="495" y="190" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#111">Tiles, stats, exports</text>
    <!-- Arrow compute to results -->
    <line x1="555" y1="135" x2="520" y2="155" stroke="#68625b" stroke-width="1.2"/>
    <polygon points="523,151 518,158 527,155" fill="#68625b"/>
    <!-- Export paths -->
    <rect x="400" y="222" width="85" height="38" rx="5" fill="#fff" stroke="#1e4f8a" stroke-width="1.2"/>
    <text x="442" y="240" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#1e4f8a" font-weight="600">Google Drive</text>
    <text x="442" y="252" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#111">GeoTIFF / CSV</text>
    <rect x="505" y="222" width="90" height="38" rx="5" fill="#fff" stroke="#1e4f8a" stroke-width="1.2"/>
    <text x="550" y="240" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#1e4f8a" font-weight="600">GEE Assets</text>
    <text x="550" y="252" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#111">Reusable layers</text>
    <line x1="470" y1="205" x2="450" y2="222" stroke="#68625b" stroke-width="1"/>
    <line x1="520" y1="205" x2="540" y2="222" stroke="#68625b" stroke-width="1"/>
    <!-- Return arrow -->
    <path d="M430,180 L250,180 L250,200 L130,200 L130,190" fill="none" stroke="#165d34" stroke-width="1.5" stroke-dasharray="5 3"/>
    <polygon points="125,190 130,200 135,190" fill="#165d34"/>
    <text x="250" y="215" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#165d34">getInfo() / map tiles</text>
  </svg>
  <p class="diagram-caption">Figure: GEE cloud computing architecture. Python code on your local machine defines operations; the GEE API builds a computation graph executed on Google Cloud where petabytes of satellite data already reside. Only results travel back to your machine.</p>
</div>

## GEE core data structures

Understanding GEE requires mastering five fundamental data types that represent spatial information at different levels of complexity.

### Image

An `ee.Image` is a raster with one or more bands, each containing pixel values plus metadata. Every band has a name, data type, scale (pixel size), and projection.

```python
import ee
ee.Initialize()

# Load a single Sentinel-2 image
image = ee.Image('COPERNICUS/S2_SR/20230715T054639_20230715T055732_T43TGK')

# Get band names
print(image.bandNames().getInfo())
# ['B1', 'B2', 'B3', 'B4', 'B5', 'B6', 'B7', 'B8', 'B8A', 'B9', 'B11', 'B12', ...]

# Access specific bands
red = image.select('B4')
nir = image.select('B8')
```

### ImageCollection

An `ee.ImageCollection` is a stack of images, typically representing a time series or spatial mosaic. Most satellite datasets in GEE are organized as ImageCollections.

```python
# Access the Sentinel-2 Surface Reflectance collection
s2 = ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')

# The collection contains millions of images globally
# Filtering narrows it to your region and time period
```

### Geometry

An `ee.Geometry` represents vector shapes: points, lines, polygons, or multi-geometries. Geometries define regions of interest for analysis.

```python
# Define a point (e.g., Bishkek, Kyrgyzstan)
point = ee.Geometry.Point([74.5698, 42.8746])

# Define a polygon (e.g., Ferghana Valley bounding box)
ferghana = ee.Geometry.Rectangle([69.5, 40.0, 72.5, 41.5])

# Define a complex polygon
polygon = ee.Geometry.Polygon([
    [[71.0, 40.5], [72.0, 40.5], [72.0, 41.0], [71.0, 41.0], [71.0, 40.5]]
])
```

### Feature and FeatureCollection

An `ee.Feature` combines a geometry with properties (attributes). An `ee.FeatureCollection` is a set of features—equivalent to a shapefile or GeoJSON.

```python
# Create a feature with properties
city = ee.Feature(
    ee.Geometry.Point([71.4491, 51.1801]),
    {'name': 'Astana', 'population': 1350000, 'country': 'Kazakhstan'}
)

# Load an existing FeatureCollection (country boundaries)
countries = ee.FeatureCollection('USDOS/LSIB_SIMPLE/2017')
central_asia = countries.filter(ee.Filter.inList('country_na', 
    ['Kazakhstan', 'Uzbekistan', 'Turkmenistan', 'Kyrgyzstan', 'Tajikistan']))
```

<div class="info-box tip">
  <strong>Key idea</strong>
  GEE operations are lazy: they define computations without executing them. Actual computation happens only when you request results (via <code>getInfo()</code>, <code>Export</code>, or visualization). This allows GEE to optimize execution across its server infrastructure.
</div>

## Python API setup and authentication

### Installation

Install the Earth Engine Python API using pip:

```python
pip install earthengine-api
```

For visualization support with folium or geemap:

```python
pip install geemap folium
```

### Authentication

Before using GEE, you must authenticate your Google account:

```python
import ee

# First-time authentication (opens browser)
ee.Authenticate()

# Initialize the API (required at start of every script)
ee.Initialize(project='your-cloud-project-id')
```

For cloud projects, you need a Google Cloud project with the Earth Engine API enabled. The free tier provides substantial computational quota for research and education.

### Verifying setup

```python
import ee
ee.Initialize()

# Test by loading a DEM
dem = ee.Image('USGS/SRTMGL1_003')
elevation_bishkek = dem.sample(ee.Geometry.Point([74.5698, 42.8746]), 30)
print(elevation_bishkek.first().get('elevation').getInfo())
# Should print elevation in meters (~800)
```

## Working with satellite collections

### Sentinel-1 SAR data

Sentinel-1 provides C-band SAR imagery regardless of cloud cover or daylight conditions—essential for monitoring Central Asia's often cloudy mountain regions.

```python
# Load Sentinel-1 GRD collection
s1 = ee.ImageCollection('COPERNICUS/S1_GRD')

# Filter for Central Asia region and parameters
kyrgyzstan = ee.Geometry.Rectangle([69.0, 39.0, 80.0, 43.5])

s1_filtered = (s1
    .filterBounds(kyrgyzstan)
    .filterDate('2023-01-01', '2023-12-31')
    .filter(ee.Filter.eq('instrumentMode', 'IW'))
    .filter(ee.Filter.listContains('transmitterReceiverPolarisation', 'VV'))
    .filter(ee.Filter.listContains('transmitterReceiverPolarisation', 'VH'))
    .filter(ee.Filter.eq('orbitProperties_pass', 'DESCENDING'))
)

print(f"Found {s1_filtered.size().getInfo()} images")
```

### Sentinel-2 multispectral data

Sentinel-2 provides 10m resolution optical imagery with 13 spectral bands. The `S2_SR_HARMONIZED` collection contains atmospherically corrected surface reflectance.

```python
# Load Sentinel-2 SR collection with cloud masking
s2 = ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')

def mask_clouds(image):
    """Mask clouds using the QA60 band"""
    qa = image.select('QA60')
    cloud_bit = 1 << 10
    cirrus_bit = 1 << 11
    mask = qa.bitwiseAnd(cloud_bit).eq(0).And(qa.bitwiseAnd(cirrus_bit).eq(0))
    return image.updateMask(mask)

# Filter and apply cloud mask
ferghana = ee.Geometry.Rectangle([69.5, 40.0, 72.5, 41.5])

s2_clean = (s2
    .filterBounds(ferghana)
    .filterDate('2023-05-01', '2023-09-30')
    .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 20))
    .map(mask_clouds)
)
```

### Landsat collections

For long-term analysis, Landsat provides continuous coverage since 1984 (Landsat 5 onward):

```python
# Landsat 8/9 Collection 2, Level 2 (Surface Reflectance)
l8 = ee.ImageCollection('LANDSAT/LC08/C02/T1_L2')
l9 = ee.ImageCollection('LANDSAT/LC09/C02/T1_L2')

# Combine for maximum temporal density
landsat_89 = l8.merge(l9)

# Scale factors for Collection 2
def apply_scale_factors(image):
    optical = image.select('SR_B.').multiply(0.0000275).add(-0.2)
    thermal = image.select('ST_B.*').multiply(0.00341802).add(149.0)
    return image.addBands(optical, overwrite=True).addBands(thermal, overwrite=True)

landsat_scaled = landsat_89.map(apply_scale_factors)
```

## Filtering, mapping, and reducing

### Filtering collections

GEE provides powerful filtering operations to narrow collections to relevant images:

```python
# Spatial filter
collection = collection.filterBounds(geometry)

# Temporal filter
collection = collection.filterDate('2020-01-01', '2020-12-31')

# Metadata filter
collection = collection.filter(ee.Filter.lt('CLOUD_COVER', 10))

# Multiple conditions
collection = collection.filter(
    ee.Filter.And(
        ee.Filter.gte('system:time_start', start_date.millis()),
        ee.Filter.lte('system:time_start', end_date.millis()),
        ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 30)
    )
)
```

### Mapping functions over collections

The `map()` function applies a transformation to every image in a collection:

```python
def add_ndvi(image):
    """Add NDVI band to Sentinel-2 image"""
    ndvi = image.normalizedDifference(['B8', 'B4']).rename('NDVI')
    return image.addBands(ndvi)

# Apply to entire collection
s2_with_ndvi = s2_clean.map(add_ndvi)
```

<div class="info-box tip">
  <strong>Key idea</strong>
  Functions passed to <code>map()</code> must use only Earth Engine operations (not Python operations on computed values). This allows GEE to distribute the function across its servers.
</div>

### Reducing collections

Reducers collapse collections into single images by applying statistical operations pixel-by-pixel:

```python
# Temporal median composite (reduces noise and clouds)
median_composite = s2_with_ndvi.median()

# Maximum NDVI (peak growing season)
max_ndvi = s2_with_ndvi.select('NDVI').max()

# Standard deviation (variability)
ndvi_stddev = s2_with_ndvi.select('NDVI').reduce(ee.Reducer.stdDev())

# Percentile composite (e.g., 25th percentile for shadow-free)
p25_composite = s2_with_ndvi.reduce(ee.Reducer.percentile([25]))
```

## Computing indices and band math

### Vegetation indices

Vegetation indices quantify plant health, density, and photosynthetic activity. The most common is NDVI:

$$
\text{NDVI} = \frac{\rho_{NIR} - \rho_{Red}}{\rho_{NIR} + \rho_{Red}}
$$

```python
def compute_indices(image):
    """Compute multiple vegetation indices for Sentinel-2"""
    
    # NDVI: Normalized Difference Vegetation Index
    ndvi = image.normalizedDifference(['B8', 'B4']).rename('NDVI')
    
    # EVI: Enhanced Vegetation Index (less saturated for dense vegetation)
    # EVI = 2.5 * (NIR - Red) / (NIR + 6*Red - 7.5*Blue + 1)
    evi = image.expression(
        '2.5 * (NIR - RED) / (NIR + 6 * RED - 7.5 * BLUE + 1)',
        {
            'NIR': image.select('B8'),
            'RED': image.select('B4'),
            'BLUE': image.select('B2')
        }
    ).rename('EVI')
    
    # SAVI: Soil-Adjusted Vegetation Index
    # SAVI = ((NIR - Red) / (NIR + Red + L)) * (1 + L), L=0.5
    savi = image.expression(
        '((NIR - RED) / (NIR + RED + 0.5)) * 1.5',
        {
            'NIR': image.select('B8'),
            'RED': image.select('B4')
        }
    ).rename('SAVI')
    
    # NDWI: Normalized Difference Water Index (Gao, 1996)
    ndwi = image.normalizedDifference(['B8', 'B11']).rename('NDWI')
    
    return image.addBands([ndvi, evi, savi, ndwi])
```

### Water and snow indices

For Central Asia's water resources and cryosphere:

```python
def compute_water_snow_indices(image):
    """Compute water and snow indices"""
    
    # MNDWI: Modified NDWI for water body mapping
    # MNDWI = (Green - SWIR1) / (Green + SWIR1)
    mndwi = image.normalizedDifference(['B3', 'B11']).rename('MNDWI')
    
    # NDSI: Normalized Difference Snow Index
    # NDSI = (Green - SWIR1) / (Green + SWIR1)
    ndsi = image.normalizedDifference(['B3', 'B11']).rename('NDSI')
    
    # SWI: Sentinel Water Index
    # SWI = 1 / sqrt(Blue - SWIR2)
    swi = image.expression(
        '1.0 / sqrt(BLUE - SWIR2)',
        {
            'BLUE': image.select('B2'),
            'SWIR2': image.select('B12')
        }
    ).rename('SWI')
    
    return image.addBands([mndwi, ndsi, swi])
```

### Built-up and bare soil indices

```python
def compute_urban_indices(image):
    """Indices for built-up areas and bare soil"""
    
    # NDBI: Normalized Difference Built-up Index
    ndbi = image.normalizedDifference(['B11', 'B8']).rename('NDBI')
    
    # BSI: Bare Soil Index
    # BSI = ((SWIR1 + Red) - (NIR + Blue)) / ((SWIR1 + Red) + (NIR + Blue))
    bsi = image.expression(
        '((SWIR1 + RED) - (NIR + BLUE)) / ((SWIR1 + RED) + (NIR + BLUE))',
        {
            'SWIR1': image.select('B11'),
            'RED': image.select('B4'),
            'NIR': image.select('B8'),
            'BLUE': image.select('B2')
        }
    ).rename('BSI')
    
    return image.addBands([ndbi, bsi])
```

## Time-series analysis and compositing

### Monthly composites

Creating consistent monthly composites enables change detection and phenological analysis:

```python
def monthly_composite(year, month, collection, aoi):
    """Create a monthly median composite"""
    start = ee.Date.fromYMD(year, month, 1)
    end = start.advance(1, 'month')
    
    monthly = collection.filterDate(start, end).filterBounds(aoi).median()
    
    return monthly.set({
        'year': year,
        'month': month,
        'system:time_start': start.millis()
    })

# Create composites for growing season 2023
aoi = ee.Geometry.Rectangle([69.5, 40.0, 72.5, 41.5])
months = ee.List.sequence(4, 9)  # April to September

def make_monthly(month):
    return monthly_composite(2023, month, s2_clean, aoi)

monthly_composites = ee.ImageCollection(months.map(make_monthly))
```

### Seasonal statistics

```python
def seasonal_stats(collection, band_name):
    """Compute seasonal statistics for a band"""
    band = collection.select(band_name)
    
    return ee.Image.cat([
        band.mean().rename(f'{band_name}_mean'),
        band.max().rename(f'{band_name}_max'),
        band.min().rename(f'{band_name}_min'),
        band.reduce(ee.Reducer.stdDev()).rename(f'{band_name}_stddev'),
        band.reduce(ee.Reducer.percentile([10, 50, 90])).rename(
            [f'{band_name}_p10', f'{band_name}_p50', f'{band_name}_p90']
        )
    ])

# Growing season NDVI statistics
ndvi_stats = seasonal_stats(s2_with_ndvi.filterDate('2023-04-01', '2023-09-30'), 'NDVI')
```

### Trend analysis

Linear regression reveals long-term trends in vegetation or water extent:

```python
def add_time(image):
    """Add time band for regression"""
    date = ee.Date(image.get('system:time_start'))
    years = date.difference(ee.Date('2017-01-01'), 'year')
    return image.addBands(ee.Image(years).float().rename('t'))

# Prepare collection with time band
collection_with_time = s2_with_ndvi.map(add_time)

# Linear regression: NDVI = a + b*t
trend = collection_with_time.select(['t', 'NDVI']).reduce(
    ee.Reducer.linearFit()
)

# 'scale' band contains slope (change per year)
# 'offset' band contains intercept
ndvi_slope = trend.select('scale')  # NDVI change per year
```

<div class="info-box tip">
  <strong>Key idea</strong>
  The slope from linear regression indicates the rate of change. Positive slopes show "greening" (increasing vegetation); negative slopes show "browning" (vegetation decline). For Central Asia, this reveals irrigation expansion, desertification, or climate-driven shifts.
</div>

## Export workflows

### Export to Google Drive

The most common export destination for personal use:

```python
# Export image to Drive
task = ee.batch.Export.image.toDrive(
    image=median_composite.select(['B4', 'B3', 'B2', 'NDVI']),
    description='Ferghana_Composite_2023',
    folder='GEE_Exports',
    fileNamePrefix='ferghana_s2_composite',
    region=ferghana.getInfo()['coordinates'],
    scale=10,
    crs='EPSG:32642',  # UTM Zone 42N
    maxPixels=1e13,
    fileFormat='GeoTIFF'
)
task.start()

# Check status
print(task.status())
```

### Export to Cloud Storage

For large exports or integration with cloud workflows:

```python
task = ee.batch.Export.image.toCloudStorage(
    image=ndvi_stats,
    description='NDVI_Stats_Export',
    bucket='your-gcs-bucket',
    fileNamePrefix='central_asia/ndvi_stats_2023',
    region=aoi.getInfo()['coordinates'],
    scale=10,
    crs='EPSG:32642',
    maxPixels=1e13,
    fileFormat='GeoTIFF'
)
task.start()
```

### Export to Earth Engine Assets

Save processed data back to GEE for future analysis:

```python
task = ee.batch.Export.image.toAsset(
    image=ndvi_stats,
    description='NDVI_Stats_Asset',
    assetId='users/your_username/central_asia/ndvi_stats_2023',
    region=aoi.getInfo()['coordinates'],
    scale=10,
    crs='EPSG:32642',
    maxPixels=1e13
)
task.start()
```

### Exporting FeatureCollections

Export vector data (classification results, sample points):

```python
task = ee.batch.Export.table.toDrive(
    collection=sample_points,
    description='Sample_Points_Export',
    folder='GEE_Exports',
    fileNamePrefix='classification_samples',
    fileFormat='GeoJSON'  # or 'SHP', 'CSV', 'KML'
)
task.start()
```

## Central Asia applications

### Irrigation mapping

The Ferghana Valley contains over 2 million hectares of irrigated land. GEE enables tracking irrigation extent and intensity:

```python
def map_irrigation(year, aoi):
    """Map irrigated areas using growing season NDVI threshold"""
    
    # Load and filter Sentinel-2
    s2 = (ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
        .filterBounds(aoi)
        .filterDate(f'{year}-05-01', f'{year}-09-30')
        .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 20))
        .map(mask_clouds))
    
    # Compute max NDVI (captures peak growing season)
    max_ndvi = s2.map(add_ndvi).select('NDVI').max()
    
    # Classify: irrigated if max NDVI > 0.5
    irrigated = max_ndvi.gt(0.5).selfMask()
    
    # Calculate area
    area = irrigated.multiply(ee.Image.pixelArea()).reduceRegion(
        reducer=ee.Reducer.sum(),
        geometry=aoi,
        scale=10,
        maxPixels=1e13
    )
    
    return irrigated.set('irrigated_area_ha', 
                         ee.Number(area.get('NDVI')).divide(10000))

# Map irrigation in Ferghana Valley
ferghana = ee.Geometry.Rectangle([69.5, 40.0, 72.5, 41.5])
irrigation_2023 = map_irrigation(2023, ferghana)
print(f"Irrigated area: {irrigation_2023.get('irrigated_area_ha').getInfo():.0f} ha")
```

### Land cover change detection

Detecting urban expansion, agricultural change, and desertification:

```python
def compute_change(year1, year2, aoi, index='NDVI'):
    """Compute change between two years"""
    
    def get_annual_composite(year):
        s2 = (ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
            .filterBounds(aoi)
            .filterDate(f'{year}-05-01', f'{year}-09-30')
            .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 20))
            .map(mask_clouds)
            .map(add_ndvi))
        return s2.select(index).median()
    
    img1 = get_annual_composite(year1)
    img2 = get_annual_composite(year2)
    
    # Compute difference
    diff = img2.subtract(img1).rename(f'{index}_change')
    
    # Classify significant changes
    gain = diff.gt(0.15).selfMask().rename('vegetation_gain')
    loss = diff.lt(-0.15).selfMask().rename('vegetation_loss')
    
    return ee.Image.cat([diff, gain, loss])

# Detect changes 2018-2023
change = compute_change(2018, 2023, ferghana)
```

### Snow cover monitoring

Central Asia depends on snowmelt for 70-80% of water resources. Monitoring snow cover is critical:

```python
def monthly_snow_cover(year, month, aoi):
    """Compute monthly snow cover fraction"""
    
    start = ee.Date.fromYMD(year, month, 1)
    end = start.advance(1, 'month')
    
    # Load MODIS snow cover product
    snow = (ee.ImageCollection('MODIS/061/MOD10A1')
        .filterDate(start, end)
        .filterBounds(aoi)
        .select('NDSI_Snow_Cover'))
    
    # Binary snow mask (NDSI > 40 is snow)
    snow_binary = snow.map(lambda img: img.gt(40))
    
    # Monthly mean snow cover
    mean_cover = snow_binary.mean()
    
    # Calculate snow-covered area
    snow_area = mean_cover.multiply(ee.Image.pixelArea()).reduceRegion(
        reducer=ee.Reducer.sum(),
        geometry=aoi,
        scale=500,
        maxPixels=1e13
    )
    
    return mean_cover.set({
        'year': year,
        'month': month,
        'snow_area_km2': ee.Number(snow_area.get('NDSI_Snow_Cover')).divide(1e6)
    })

# Tien Shan mountains
tien_shan = ee.Geometry.Rectangle([74.0, 41.0, 80.0, 43.5])

# Winter snow cover
feb_snow = monthly_snow_cover(2023, 2, tien_shan)
print(f"Snow cover: {feb_snow.get('snow_area_km2').getInfo():.0f} km²")
```

### Glacier monitoring

```python
def glacier_extent(year, glacier_outline):
    """Map glacier extent using Sentinel-2 NDSI"""
    
    # Summer images for snow-free glacier mapping
    s2 = (ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
        .filterBounds(glacier_outline)
        .filterDate(f'{year}-07-01', f'{year}-09-15')
        .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 10))
        .map(mask_clouds))
    
    # Median composite
    composite = s2.median()
    
    # NDSI for snow/ice
    ndsi = composite.normalizedDifference(['B3', 'B11'])
    
    # Classify ice (NDSI > 0.4 and NIR > threshold for debris-free)
    ice = ndsi.gt(0.4).And(composite.select('B8').gt(1500))
    
    # Clip to glacier outline
    glacier_ice = ice.clip(glacier_outline).selfMask()
    
    # Calculate area
    area = glacier_ice.multiply(ee.Image.pixelArea()).reduceRegion(
        reducer=ee.Reducer.sum(),
        geometry=glacier_outline,
        scale=10,
        maxPixels=1e13
    )
    
    return glacier_ice.set('glacier_area_km2', 
                           ee.Number(area.get('nd')).divide(1e6))
```

## Best practices for efficient code

### Minimize getInfo() calls

Every `getInfo()` call blocks execution and transfers data from server to client. Batch operations where possible:

```python
# Inefficient: multiple getInfo calls
for i in range(10):
    result = collection.filter(...).size().getInfo()  # 10 round trips

# Efficient: single getInfo call
results = collection.filter(...).aggregate_array('property').getInfo()  # 1 round trip
```

### Use appropriate scales

Processing at higher resolution than necessary wastes computation:

```python
# For regional analysis, 30m or 100m is often sufficient
stats = image.reduceRegion(
    reducer=ee.Reducer.mean(),
    geometry=large_aoi,
    scale=100,  # Not 10m when analyzing millions of hectares
    maxPixels=1e13
)
```

### Leverage server-side operations

Keep computation on GEE servers rather than fetching data for local processing:

```python
# Server-side: good
mean_ndvi = collection.select('NDVI').mean()

# Client-side: bad (fetches all data)
values = collection.select('NDVI').getInfo()  # Avoid!
local_mean = sum(values) / len(values)
```

### Use filterBounds early

Spatial filtering dramatically reduces data volume:

```python
# Good: filter spatially first
collection = (ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
    .filterBounds(aoi)  # First!
    .filterDate('2023-01-01', '2023-12-31')
    .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 20)))

# Less efficient: date filter first
collection = (ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
    .filterDate('2023-01-01', '2023-12-31')
    .filterBounds(aoi)  # Later
    .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 20)))
```

### Cache intermediate results

For repeated analysis, export intermediate products:

```python
# If you'll use this composite multiple times, export it
task = ee.batch.Export.image.toAsset(
    image=expensive_composite,
    assetId='users/you/cached_composite',
    ...
)

# Then load from asset in future analyses
cached = ee.Image('users/you/cached_composite')
```

## Tutorial checklist

<ul class="checklist">
  <li>Understand the five core data types: Image, ImageCollection, Geometry, Feature, FeatureCollection</li>
  <li>Install earthengine-api and authenticate with <code>ee.Authenticate()</code></li>
  <li>Successfully run <code>ee.Initialize()</code> with a cloud project</li>
  <li>Filter Sentinel-1 collection by orbit pass, polarization, and instrument mode</li>
  <li>Filter Sentinel-2 collection by cloud percentage and apply QA60 cloud mask</li>
  <li>Apply scale factors to Landsat Collection 2 data</li>
  <li>Write a function that can be mapped over an ImageCollection</li>
  <li>Compute NDVI, EVI, NDWI, and NDSI using normalizedDifference and expression</li>
  <li>Create monthly composites using median reduction</li>
  <li>Perform linear regression for trend analysis</li>
  <li>Export images to Google Drive in GeoTIFF format</li>
  <li>Export FeatureCollections to GeoJSON or Shapefile</li>
  <li>Map irrigated areas using NDVI thresholding</li>
  <li>Detect land cover change between two time periods</li>
  <li>Monitor snow cover using MODIS or Sentinel-2 NDSI</li>
  <li>Apply best practices: filterBounds first, minimize getInfo, use appropriate scale</li>
  <li>Complete all 30 exercises with working solutions</li>
</ul>

## Exercises

<details>
<summary><strong>Exercise 1:</strong> Create a Point geometry for Tashkent (69.2401°E, 41.2995°N) and extract the SRTM elevation.</summary>

```python
import ee
ee.Initialize()

# Create point geometry
tashkent = ee.Geometry.Point([69.2401, 41.2995])

# Load SRTM DEM
dem = ee.Image('USGS/SRTMGL1_003')

# Extract elevation
elevation = dem.sample(tashkent, 30).first().get('elevation')
print(f"Tashkent elevation: {elevation.getInfo()} m")
# Expected output: approximately 455 m
```
</details>

<details>
<summary><strong>Exercise 2:</strong> Count how many Sentinel-2 images cover the Issyk-Kul lake region (76.5-78.5°E, 42.0-42.8°N) in 2023.</summary>

```python
# Define region
issyk_kul = ee.Geometry.Rectangle([76.5, 42.0, 78.5, 42.8])

# Filter collection
s2 = (ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
    .filterBounds(issyk_kul)
    .filterDate('2023-01-01', '2023-12-31'))

count = s2.size().getInfo()
print(f"Sentinel-2 images over Issyk-Kul in 2023: {count}")
# Expected: several hundred images
```
</details>

<details>
<summary><strong>Exercise 3:</strong> Filter Sentinel-1 for VV polarization, IW mode, descending orbit over Almaty region for summer 2023.</summary>

```python
# Almaty region
almaty = ee.Geometry.Rectangle([76.5, 42.8, 77.5, 43.5])

s1 = (ee.ImageCollection('COPERNICUS/S1_GRD')
    .filterBounds(almaty)
    .filterDate('2023-06-01', '2023-08-31')
    .filter(ee.Filter.eq('instrumentMode', 'IW'))
    .filter(ee.Filter.listContains('transmitterReceiverPolarisation', 'VV'))
    .filter(ee.Filter.eq('orbitProperties_pass', 'DESCENDING')))

print(f"S1 images: {s1.size().getInfo()}")
print(f"First image: {s1.first().get('system:index').getInfo()}")
```
</details>

<details>
<summary><strong>Exercise 4:</strong> Write a cloud masking function for Sentinel-2 using the SCL band (Scene Classification Layer).</summary>

```python
def mask_clouds_scl(image):
    """
    Mask clouds using Scene Classification Layer (SCL).
    SCL values: 3=cloud shadow, 8=medium prob cloud, 
                9=high prob cloud, 10=thin cirrus
    """
    scl = image.select('SCL')
    mask = scl.neq(3).And(scl.neq(8)).And(scl.neq(9)).And(scl.neq(10))
    return image.updateMask(mask)

# Apply to collection
s2_masked = s2.map(mask_clouds_scl)
print(f"Collection size: {s2_masked.size().getInfo()}")
```
</details>

<details>
<summary><strong>Exercise 5:</strong> Compute NDVI for a Sentinel-2 image and find the mean NDVI over a polygon.</summary>

```python
# Sample polygon (agricultural area near Samarkand)
farm = ee.Geometry.Rectangle([66.8, 39.6, 67.0, 39.8])

# Get cloud-free image
image = (ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
    .filterBounds(farm)
    .filterDate('2023-07-01', '2023-07-31')
    .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 5))
    .first())

# Compute NDVI
ndvi = image.normalizedDifference(['B8', 'B4']).rename('NDVI')

# Calculate mean
mean_ndvi = ndvi.reduceRegion(
    reducer=ee.Reducer.mean(),
    geometry=farm,
    scale=10
)
print(f"Mean NDVI: {mean_ndvi.get('NDVI').getInfo():.3f}")
```
</details>

<details>
<summary><strong>Exercise 6:</strong> Calculate MNDWI (Modified Normalized Difference Water Index) for water body detection.</summary>

```python
def add_mndwi(image):
    """
    MNDWI = (Green - SWIR1) / (Green + SWIR1)
    For Sentinel-2: (B3 - B11) / (B3 + B11)
    """
    mndwi = image.normalizedDifference(['B3', 'B11']).rename('MNDWI')
    return image.addBands(mndwi)

# Apply to image
image_mndwi = add_mndwi(image)

# Water where MNDWI > 0.3
water = image_mndwi.select('MNDWI').gt(0.3)
print("Water mask computed")
```
</details>

<details>
<summary><strong>Exercise 7:</strong> Create a median composite of Sentinel-2 for summer 2023 over the Ferghana Valley.</summary>

```python
ferghana = ee.Geometry.Rectangle([69.5, 40.0, 72.5, 41.5])

s2 = (ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
    .filterBounds(ferghana)
    .filterDate('2023-06-01', '2023-08-31')
    .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 20))
    .map(mask_clouds_scl))

median = s2.median()
print(f"Band names: {median.bandNames().getInfo()}")
```
</details>

<details>
<summary><strong>Exercise 8:</strong> Compute the maximum NDVI composite for the growing season (April-September 2023).</summary>

```python
def add_ndvi(image):
    ndvi = image.normalizedDifference(['B8', 'B4']).rename('NDVI')
    return image.addBands(ndvi)

s2_ndvi = (ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
    .filterBounds(ferghana)
    .filterDate('2023-04-01', '2023-09-30')
    .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 20))
    .map(mask_clouds_scl)
    .map(add_ndvi))

max_ndvi = s2_ndvi.select('NDVI').max()
print("Max NDVI composite created")
```
</details>

<details>
<summary><strong>Exercise 9:</strong> Calculate standard deviation of NDVI over the growing season.</summary>

```python
ndvi_stddev = s2_ndvi.select('NDVI').reduce(ee.Reducer.stdDev())

# Get mean std dev over region
mean_std = ndvi_stddev.reduceRegion(
    reducer=ee.Reducer.mean(),
    geometry=ferghana,
    scale=100
)
print(f"Mean NDVI std dev: {mean_std.get('NDVI_stdDev').getInfo():.4f}")
```
</details>

<details>
<summary><strong>Exercise 10:</strong> Map irrigated areas using NDVI > 0.5 threshold during peak growing season.</summary>

```python
# July composite for peak irrigation
july_s2 = (ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
    .filterBounds(ferghana)
    .filterDate('2023-07-01', '2023-07-31')
    .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 10))
    .map(mask_clouds_scl)
    .map(add_ndvi)
    .median())

# Irrigated mask
irrigated = july_s2.select('NDVI').gt(0.5).selfMask()

# Calculate area
area = irrigated.multiply(ee.Image.pixelArea()).reduceRegion(
    reducer=ee.Reducer.sum(),
    geometry=ferghana,
    scale=10,
    maxPixels=1e13
)
print(f"Irrigated area: {area.get('NDVI').getInfo() / 1e6:.0f} km²")
```
</details>

<details>
<summary><strong>Exercise 11:</strong> Extract Sentinel-1 VV and VH backscatter values at a specific point.</summary>

```python
point = ee.Geometry.Point([71.0, 40.5])

s1 = (ee.ImageCollection('COPERNICUS/S1_GRD')
    .filterBounds(point)
    .filterDate('2023-07-01', '2023-07-31')
    .filter(ee.Filter.eq('instrumentMode', 'IW'))
    .first())

values = s1.select(['VV', 'VH']).reduceRegion(
    reducer=ee.Reducer.first(),
    geometry=point,
    scale=10
)
print(f"VV: {values.get('VV').getInfo():.2f} dB")
print(f"VH: {values.get('VH').getInfo():.2f} dB")
```
</details>

<details>
<summary><strong>Exercise 12:</strong> Compute the VH/VV ratio for Sentinel-1 (useful for crop classification).</summary>

```python
def add_vh_vv_ratio(image):
    """Add VH/VV ratio band"""
    ratio = image.select('VH').divide(image.select('VV')).rename('VH_VV')
    return image.addBands(ratio)

s1_ratio = (ee.ImageCollection('COPERNICUS/S1_GRD')
    .filterBounds(ferghana)
    .filterDate('2023-07-01', '2023-07-31')
    .filter(ee.Filter.eq('instrumentMode', 'IW'))
    .map(add_vh_vv_ratio)
    .median())

print(f"Bands: {s1_ratio.bandNames().getInfo()}")
```
</details>

<details>
<summary><strong>Exercise 13:</strong> Create monthly NDVI time series for 2023 (12 monthly values).</summary>

```python
def monthly_ndvi(month):
    month = ee.Number(month)
    start = ee.Date.fromYMD(2023, month, 1)
    end = start.advance(1, 'month')
    
    monthly = (ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
        .filterBounds(ferghana)
        .filterDate(start, end)
        .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 30))
        .map(add_ndvi)
        .select('NDVI')
        .median())
    
    mean_ndvi = monthly.reduceRegion(
        reducer=ee.Reducer.mean(),
        geometry=ferghana,
        scale=100
    )
    
    return ee.Feature(None, {
        'month': month,
        'ndvi': mean_ndvi.get('NDVI')
    })

months = ee.List.sequence(1, 12)
time_series = ee.FeatureCollection(months.map(monthly_ndvi))
print(time_series.getInfo())
```
</details>

<details>
<summary><strong>Exercise 14:</strong> Perform linear trend analysis on NDVI from 2018-2023.</summary>

```python
def annual_ndvi(year):
    year = ee.Number(year).int()
    start = ee.Date.fromYMD(year, 6, 1)
    end = ee.Date.fromYMD(year, 8, 31)
    
    ndvi = (ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
        .filterBounds(ferghana)
        .filterDate(start, end)
        .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 20))
        .map(add_ndvi)
        .select('NDVI')
        .median())
    
    return ndvi.set('year', year).set('system:time_start', start.millis())

years = ee.List.sequence(2018, 2023)
annual_collection = ee.ImageCollection(years.map(annual_ndvi))

# Add time band and compute trend
def add_time(image):
    t = ee.Number(image.get('year')).subtract(2018)
    return image.addBands(ee.Image.constant(t).float().rename('t'))

with_time = annual_collection.map(add_time)
trend = with_time.select(['t', 'NDVI']).reduce(ee.Reducer.linearFit())

print("Trend analysis complete")
print(f"Bands: {trend.bandNames().getInfo()}")
```
</details>

<details>
<summary><strong>Exercise 15:</strong> Detect snow cover using Sentinel-2 NDSI threshold.</summary>

```python
# Winter image
s2_winter = (ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
    .filterBounds(tien_shan)
    .filterDate('2023-02-01', '2023-02-28')
    .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 20))
    .median())

# NDSI
ndsi = s2_winter.normalizedDifference(['B3', 'B11']).rename('NDSI')

# Snow mask (NDSI > 0.4)
snow = ndsi.gt(0.4).selfMask()

# Snow area
snow_area = snow.multiply(ee.Image.pixelArea()).reduceRegion(
    reducer=ee.Reducer.sum(),
    geometry=tien_shan,
    scale=100,
    maxPixels=1e13
)
print(f"Snow area: {snow_area.get('NDSI').getInfo() / 1e6:.0f} km²")
```
</details>

<details>
<summary><strong>Exercise 16:</strong> Use MODIS to track snow cover change between January and March 2023.</summary>

```python
def monthly_snow(month):
    start = ee.Date.fromYMD(2023, month, 1)
    end = start.advance(1, 'month')
    
    snow = (ee.ImageCollection('MODIS/061/MOD10A1')
        .filterDate(start, end)
        .filterBounds(tien_shan)
        .select('NDSI_Snow_Cover')
        .mean())
    
    # Snow where NDSI > 40
    snow_binary = snow.gt(40)
    
    area = snow_binary.multiply(ee.Image.pixelArea()).reduceRegion(
        reducer=ee.Reducer.sum(),
        geometry=tien_shan,
        scale=500,
        maxPixels=1e13
    )
    
    return ee.Feature(None, {
        'month': month,
        'snow_km2': ee.Number(area.get('NDSI_Snow_Cover')).divide(1e6)
    })

jan = monthly_snow(1)
mar = monthly_snow(3)
print(f"January snow: {jan.get('snow_km2').getInfo():.0f} km²")
print(f"March snow: {mar.get('snow_km2').getInfo():.0f} km²")
```
</details>

<details>
<summary><strong>Exercise 17:</strong> Compute land surface temperature from Landsat 8 thermal band.</summary>

```python
# Load Landsat 8 with scaling
l8 = ee.ImageCollection('LANDSAT/LC08/C02/T1_L2')

def apply_scale(image):
    thermal = image.select('ST_B10').multiply(0.00341802).add(149.0)
    return image.addBands(thermal.rename('LST_K'), overwrite=True)

lst = (l8
    .filterBounds(ferghana)
    .filterDate('2023-07-01', '2023-07-31')
    .filter(ee.Filter.lt('CLOUD_COVER', 10))
    .map(apply_scale)
    .select('LST_K')
    .median())

# Convert to Celsius
lst_c = lst.subtract(273.15).rename('LST_C')

# Mean temperature
mean_lst = lst_c.reduceRegion(
    reducer=ee.Reducer.mean(),
    geometry=ferghana,
    scale=30
)
print(f"Mean LST: {mean_lst.get('LST_C').getInfo():.1f} °C")
```
</details>

<details>
<summary><strong>Exercise 18:</strong> Create a FeatureCollection of major Central Asian cities with populations.</summary>

```python
cities = ee.FeatureCollection([
    ee.Feature(ee.Geometry.Point([71.4491, 51.1801]), 
               {'name': 'Astana', 'country': 'Kazakhstan', 'pop': 1350000}),
    ee.Feature(ee.Geometry.Point([76.9286, 43.2220]), 
               {'name': 'Almaty', 'country': 'Kazakhstan', 'pop': 2000000}),
    ee.Feature(ee.Geometry.Point([69.2401, 41.2995]), 
               {'name': 'Tashkent', 'country': 'Uzbekistan', 'pop': 2500000}),
    ee.Feature(ee.Geometry.Point([74.5698, 42.8746]), 
               {'name': 'Bishkek', 'country': 'Kyrgyzstan', 'pop': 1100000}),
    ee.Feature(ee.Geometry.Point([68.7864, 38.5598]), 
               {'name': 'Dushanbe', 'country': 'Tajikistan', 'pop': 860000})
])

print(f"Cities: {cities.size().getInfo()}")
print(cities.first().getInfo())
```
</details>

<details>
<summary><strong>Exercise 19:</strong> Extract NDVI values at each city location for summer 2023.</summary>

```python
# Summer composite
s2_summer = (ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
    .filterDate('2023-06-01', '2023-08-31')
    .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 20))
    .map(add_ndvi)
    .select('NDVI')
    .median())

# Sample at city locations
city_ndvi = s2_summer.sampleRegions(
    collection=cities,
    scale=10,
    geometries=True
)

print(city_ndvi.getInfo())
```
</details>

<details>
<summary><strong>Exercise 20:</strong> Filter the cities FeatureCollection to only Kazakhstan.</summary>

```python
kazakhstan_cities = cities.filter(ee.Filter.eq('country', 'Kazakhstan'))
print(f"Kazakhstan cities: {kazakhstan_cities.size().getInfo()}")
print([f.get('properties').get('name') for f in kazakhstan_cities.getInfo()['features']])
```
</details>

<details>
<summary><strong>Exercise 21:</strong> Calculate the total population of cities with population > 1 million.</summary>

```python
large_cities = cities.filter(ee.Filter.gt('pop', 1000000))
total_pop = large_cities.aggregate_sum('pop')
print(f"Total population of large cities: {total_pop.getInfo():,}")
```
</details>

<details>
<summary><strong>Exercise 22:</strong> Create an RGB composite visualization parameters dictionary.</summary>

```python
# Sentinel-2 true color visualization
vis_true_color = {
    'bands': ['B4', 'B3', 'B2'],
    'min': 0,
    'max': 3000,
    'gamma': 1.2
}

# Sentinel-2 false color (vegetation)
vis_false_color = {
    'bands': ['B8', 'B4', 'B3'],
    'min': 0,
    'max': 5000,
    'gamma': 1.1
}

# NDVI visualization
vis_ndvi = {
    'min': -0.2,
    'max': 0.8,
    'palette': ['brown', 'yellow', 'lightgreen', 'green', 'darkgreen']
}

print("Visualization parameters defined")
```
</details>

<details>
<summary><strong>Exercise 23:</strong> Use geemap to display an interactive map with Sentinel-2 imagery.</summary>

```python
import geemap

# Create map centered on Ferghana Valley
Map = geemap.Map(center=[40.5, 71.0], zoom=8)

# Add median composite
Map.addLayer(median, vis_true_color, 'Sentinel-2 True Color')
Map.addLayer(max_ndvi, vis_ndvi, 'Max NDVI 2023')

# Add layer control
Map.addLayerControl()

# Display (in Jupyter notebook)
Map
```
</details>

<details>
<summary><strong>Exercise 24:</strong> Export a GeoTIFF of NDVI to Google Drive.</summary>

```python
task = ee.batch.Export.image.toDrive(
    image=max_ndvi,
    description='Ferghana_MaxNDVI_2023',
    folder='GEE_Exports',
    fileNamePrefix='ferghana_max_ndvi_2023',
    region=ferghana.getInfo()['coordinates'],
    scale=10,
    crs='EPSG:32642',
    maxPixels=1e13,
    fileFormat='GeoTIFF'
)

task.start()
print(f"Export started. Task ID: {task.id}")
```
</details>

<details>
<summary><strong>Exercise 25:</strong> Export sample points to a GeoJSON file.</summary>

```python
# Create sample points
sample_points = max_ndvi.sample(
    region=ferghana,
    scale=1000,
    numPixels=100,
    geometries=True
)

task = ee.batch.Export.table.toDrive(
    collection=sample_points,
    description='NDVI_Sample_Points',
    folder='GEE_Exports',
    fileNamePrefix='ndvi_samples',
    fileFormat='GeoJSON'
)

task.start()
print("Export to GeoJSON started")
```
</details>

<details>
<summary><strong>Exercise 26:</strong> Detect change in water extent of Aral Sea between 2000 and 2023.</summary>

```python
aral_sea = ee.Geometry.Rectangle([58.0, 43.5, 61.5, 46.5])

def water_extent(year):
    """Map water using MNDWI from Landsat"""
    collection = 'LANDSAT/LC08/C02/T1_L2' if year >= 2013 else 'LANDSAT/LE07/C02/T1_L2'
    
    img = (ee.ImageCollection(collection)
        .filterBounds(aral_sea)
        .filterDate(f'{year}-06-01', f'{year}-09-30')
        .filter(ee.Filter.lt('CLOUD_COVER', 20))
        .median())
    
    # Scale factors
    green = img.select('SR_B3' if year >= 2013 else 'SR_B2').multiply(0.0000275).add(-0.2)
    swir = img.select('SR_B6' if year >= 2013 else 'SR_B5').multiply(0.0000275).add(-0.2)
    
    mndwi = green.subtract(swir).divide(green.add(swir))
    water = mndwi.gt(0.2).selfMask()
    
    area = water.multiply(ee.Image.pixelArea()).reduceRegion(
        reducer=ee.Reducer.sum(),
        geometry=aral_sea,
        scale=30,
        maxPixels=1e13
    )
    
    return ee.Number(area.values().get(0)).divide(1e6)

water_2000 = water_extent(2007)  # Using L7 for proxy
water_2023 = water_extent(2023)
print(f"Water 2007: {water_2000.getInfo():.0f} km²")
print(f"Water 2023: {water_2023.getInfo():.0f} km²")
```
</details>

<details>
<summary><strong>Exercise 27:</strong> Compute zonal statistics of elevation for each Central Asian country.</summary>

```python
# Load country boundaries
countries = ee.FeatureCollection('USDOS/LSIB_SIMPLE/2017').filter(
    ee.Filter.inList('country_na', 
        ['Kazakhstan', 'Uzbekistan', 'Turkmenistan', 'Kyrgyzstan', 'Tajikistan'])
)

# Load DEM
dem = ee.Image('USGS/SRTMGL1_003')

# Compute zonal statistics
def add_elevation_stats(feature):
    stats = dem.reduceRegion(
        reducer=ee.Reducer.mean().combine(
            ee.Reducer.minMax(), sharedInputs=True
        ),
        geometry=feature.geometry(),
        scale=1000,
        maxPixels=1e13
    )
    return feature.set(stats)

country_elevation = countries.map(add_elevation_stats)
print(country_elevation.getInfo())
```
</details>

<details>
<summary><strong>Exercise 28:</strong> Create a land cover classification using simple thresholds.</summary>

```python
def simple_classification(image):
    """
    Simple rule-based classification:
    1 = Water (MNDWI > 0.3)
    2 = Snow/Ice (NDSI > 0.4)
    3 = Vegetation (NDVI > 0.4)
    4 = Urban/Bare (NDBI > 0 and NDVI < 0.2)
    5 = Other
    """
    ndvi = image.normalizedDifference(['B8', 'B4'])
    mndwi = image.normalizedDifference(['B3', 'B11'])
    ndsi = image.normalizedDifference(['B3', 'B11'])
    ndbi = image.normalizedDifference(['B11', 'B8'])
    
    water = mndwi.gt(0.3)
    snow = ndsi.gt(0.4).And(water.Not())
    veg = ndvi.gt(0.4).And(water.Not()).And(snow.Not())
    urban = ndbi.gt(0).And(ndvi.lt(0.2)).And(water.Not())
    
    classification = (ee.Image(5)
        .where(water, 1)
        .where(snow, 2)
        .where(veg, 3)
        .where(urban, 4))
    
    return classification.rename('landcover')

# Apply classification
composite = (ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
    .filterBounds(ferghana)
    .filterDate('2023-06-01', '2023-08-31')
    .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 10))
    .median())

landcover = simple_classification(composite)
print("Classification complete")
```
</details>

<details>
<summary><strong>Exercise 29:</strong> Calculate area statistics for each land cover class.</summary>

```python
# Calculate pixel areas
pixel_area = ee.Image.pixelArea()

# Multiply classification by area
class_areas = landcover.addBands(pixel_area)

# Reduce by class
stats = class_areas.reduceRegion(
    reducer=ee.Reducer.sum().group(
        groupField=0,
        groupName='class'
    ),
    geometry=ferghana,
    scale=10,
    maxPixels=1e13
)

print("Area by class:")
for group in stats.get('groups').getInfo():
    class_id = group['class']
    area_km2 = group['sum'] / 1e6
    class_names = {1: 'Water', 2: 'Snow', 3: 'Vegetation', 4: 'Urban', 5: 'Other'}
    print(f"  {class_names.get(class_id, 'Unknown')}: {area_km2:.0f} km²")
```
</details>

<details>
<summary><strong>Exercise 30:</strong> Build a complete workflow: filter S2, compute NDVI, create composite, detect irrigation, export results.</summary>

```python
# Complete irrigation mapping workflow for Central Asia

import ee
ee.Initialize()

# 1. Define study area
ferghana = ee.Geometry.Rectangle([69.5, 40.0, 72.5, 41.5])

# 2. Cloud masking function
def mask_clouds(image):
    qa = image.select('QA60')
    mask = qa.bitwiseAnd(1 << 10).eq(0).And(qa.bitwiseAnd(1 << 11).eq(0))
    return image.updateMask(mask)

# 3. Add indices function
def add_indices(image):
    ndvi = image.normalizedDifference(['B8', 'B4']).rename('NDVI')
    ndwi = image.normalizedDifference(['B8', 'B11']).rename('NDWI')
    return image.addBands([ndvi, ndwi])

# 4. Filter and process collection
s2 = (ee.ImageCollection('COPERNICUS/S2_SR_HARMONIZED')
    .filterBounds(ferghana)
    .filterDate('2023-06-01', '2023-08-31')
    .filter(ee.Filter.lt('CLOUDY_PIXEL_PERCENTAGE', 15))
    .map(mask_clouds)
    .map(add_indices))

# 5. Create composites
median_composite = s2.median()
max_ndvi = s2.select('NDVI').max()

# 6. Detect irrigation (NDVI > 0.5)
irrigated = max_ndvi.gt(0.5).selfMask().rename('irrigated')

# 7. Calculate irrigated area
irrigated_area = irrigated.multiply(ee.Image.pixelArea()).reduceRegion(
    reducer=ee.Reducer.sum(),
    geometry=ferghana,
    scale=10,
    maxPixels=1e13
)

print(f"Irrigated area: {irrigated_area.get('irrigated').getInfo() / 1e6:.0f} km²")

# 8. Export results
export_image = median_composite.select(['B4', 'B3', 'B2', 'B8']).addBands(max_ndvi).addBands(irrigated)

task = ee.batch.Export.image.toDrive(
    image=export_image,
    description='Ferghana_Irrigation_2023',
    folder='GEE_Exports',
    fileNamePrefix='ferghana_irrigation_analysis_2023',
    region=ferghana.getInfo()['coordinates'],
    scale=10,
    crs='EPSG:32642',
    maxPixels=1e13,
    fileFormat='GeoTIFF'
)

task.start()
print(f"Export started. Task status: {task.status()}")
print("Workflow complete!")
```
</details>

## Summary

Google Earth Engine transforms the scale of possible geospatial analysis. By executing computation on Google's infrastructure where petabytes of satellite data already reside, GEE enables analysis that would be impractical on local machines. For Central Asia—a region spanning millions of square kilometers with complex environmental dynamics—this capability is transformative.

The key concepts to remember:
- **Five data types**: Image, ImageCollection, Geometry, Feature, FeatureCollection form the foundation
- **Lazy evaluation**: Operations define computations; execution happens only when results are requested
- **Filter-Map-Reduce**: The paradigm for efficient processing of large collections
- **Server-side operations**: Keep computation on GEE servers; minimize data transfer to client
- **Export workflows**: Three destinations (Drive, Cloud Storage, Assets) serve different use cases

Central Asia's environmental challenges—water scarcity, glacier retreat, desertification, agricultural intensification—demand analysis at regional scales over multi-decadal timespans. Google Earth Engine provides the platform to perform this analysis efficiently, opening new possibilities for research, monitoring, and decision support across the region.
