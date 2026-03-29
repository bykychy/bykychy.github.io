---
layout: tutorial
title: "How Python Enables Geospatial Analysis for Central Asia Research"
description: "A practical tutorial on rasterio, geopandas, xarray, and geospatial workflows in Python for remote sensing and GIS analysis, with 30 exercises and solutions."
difficulty: intermediate
estimated_time: "3-4 hours"
series: sar-remote-sensing
order: 15
---

## Why this tutorial matters

Traditional GIS workflows rely on graphical interfaces where each click represents an undocumented step. When you process imagery through menus and dialogs, the exact sequence of operations exists only in your memory. Months later, when a colleague asks how you produced a figure, or when you need to update an analysis with new data, you face the tedious task of reconstructing your workflow from scratch.

Python transforms geospatial analysis into reproducible, scalable science. Every step becomes code that documents itself. When new Sentinel-2 imagery arrives for the Fergana Valley, the same script that processed last year's data processes this year's data. When a reviewer questions your methodology, you share your code. When you need to apply your analysis to Tajikistan after developing it for Kyrgyzstan, you change a file path.

<div class="info-box tip">
  <strong>Key idea</strong>
  Code is documentation. A Python script that processes satellite imagery is simultaneously an analysis tool and a complete record of methodology. This dual nature makes computational approaches essential for scientific research.
</div>

For Central Asia specifically, Python-based workflows address critical challenges: processing decades of Landsat and Sentinel archives, integrating data from multiple sources (satellite imagery, climate records, administrative boundaries), and scaling analyses across the five countries of the region. The ecosystem of geospatial Python libraries has matured to the point where complex analyses that once required expensive proprietary software now run on any laptop.

## Python geospatial ecosystem overview

The Python geospatial stack consists of interconnected libraries that handle different data types and operations. Understanding this ecosystem helps you choose the right tool for each task.

### Core libraries

**GDAL/OGR** forms the foundation. The Geospatial Data Abstraction Library reads and writes hundreds of raster and vector formats. Most higher-level Python libraries wrap GDAL functionality with more Pythonic interfaces.

**Rasterio** provides elegant access to raster data. Built on GDAL, it offers clean syntax for reading, writing, and manipulating gridded data like satellite imagery and digital elevation models.

**Fiona** does for vector data what Rasterio does for rasters. It reads and writes shapefiles, GeoJSON, GeoPackage, and other vector formats with a simple Python interface.

**Shapely** handles geometric operations: buffering, intersection, union, and spatial predicates. It implements the OGC Simple Features specification.

**Geopandas** combines Pandas DataFrames with geometric capabilities. It extends the familiar Pandas syntax to spatial data, enabling operations like spatial joins and overlay analysis.

**Pyproj** manages coordinate reference systems and transformations. It wraps the PROJ library to handle the complex mathematics of projecting Earth's curved surface onto flat maps.

### Scientific computing stack

**NumPy** underlies all numerical operations. Raster data in Python is fundamentally NumPy arrays, making array operations directly applicable to imagery.

**Xarray** extends NumPy arrays with labeled dimensions and coordinates. For multidimensional data—time series of satellite imagery, climate model output, hyperspectral cubes—xarray provides intuitive access and powerful computation.

**Rioxarray** bridges rasterio and xarray, adding geospatial awareness to xarray's multidimensional arrays. It handles CRS information, spatial coordinates, and geospatial I/O within the xarray framework.

**Dask** enables parallel and out-of-core computation. When datasets exceed memory, Dask arrays process data in chunks, scaling from laptops to clusters.

```python
# Standard imports for geospatial work
import numpy as np
import pandas as pd
import geopandas as gpd
import rasterio
from rasterio.plot import show
import xarray as xr
import rioxarray
from shapely.geometry import Point, Polygon, box
import matplotlib.pyplot as plt
from pyproj import CRS, Transformer

# Set up plotting defaults
plt.rcParams['figure.figsize'] = (10, 8)
plt.rcParams['figure.dpi'] = 100
```

<div class="info-box tip">
  <strong>Key idea</strong>
  The geospatial Python ecosystem is modular. Rather than one monolithic application, you combine specialized libraries. This design allows each component to excel at its task while enabling flexible workflows.
</div>

## Rasterio: working with gridded data

Rasterio is the primary library for raster operations in Python. It provides clean access to satellite imagery, digital elevation models, and any gridded geospatial data.

### Reading raster data

Opening a raster file with rasterio creates a dataset object containing metadata and data access methods:

```python
import rasterio
from rasterio.plot import show

# Open a GeoTIFF file
with rasterio.open('sentinel2_b4.tif') as src:
    # Access metadata
    print(f"Width: {src.width}, Height: {src.height}")
    print(f"Number of bands: {src.count}")
    print(f"Data type: {src.dtypes[0]}")
    print(f"CRS: {src.crs}")
    print(f"Bounds: {src.bounds}")
    print(f"Transform: {src.transform}")
    
    # Read the first band as a NumPy array
    band1 = src.read(1)
    print(f"Array shape: {band1.shape}")
    print(f"Min: {band1.min()}, Max: {band1.max()}")
```

The `transform` attribute is crucial—it defines the relationship between pixel coordinates and geographic coordinates:

```python
with rasterio.open('dem.tif') as src:
    transform = src.transform
    
    # Convert pixel coordinates to geographic coordinates
    # row 0, col 0 corresponds to upper-left corner
    x, y = rasterio.transform.xy(transform, 0, 0)
    print(f"Upper-left corner: ({x}, {y})")
    
    # Convert geographic coordinates to pixel coordinates
    row, col = rasterio.transform.rowcol(transform, x, y)
    print(f"Pixel coordinates: row={row}, col={col}")
```

### Reading multiple bands

Satellite imagery typically contains multiple spectral bands. Rasterio can read them individually or together:

```python
with rasterio.open('sentinel2_multispectral.tif') as src:
    # Read all bands at once: shape is (bands, height, width)
    all_bands = src.read()
    print(f"Shape: {all_bands.shape}")
    
    # Read specific bands (1-indexed)
    red = src.read(4)    # Band 4 is red in Sentinel-2
    nir = src.read(8)    # Band 8 is NIR
    
    # Calculate NDVI
    ndvi = (nir.astype(float) - red.astype(float)) / (nir + red + 1e-10)
    
    # Read bands as a list
    rgb_bands = src.read([4, 3, 2])  # Red, Green, Blue
```

### Windowed reading

Large rasters may not fit in memory. Windowed reading loads only portions of the data:

```python
from rasterio.windows import Window

with rasterio.open('large_mosaic.tif') as src:
    # Define a window: Window(col_off, row_off, width, height)
    window = Window(1000, 2000, 512, 512)
    
    # Read only the window
    subset = src.read(1, window=window)
    print(f"Subset shape: {subset.shape}")
    
    # Get the transform for the window
    window_transform = src.window_transform(window)
```

For processing large files in tiles:

```python
from rasterio.windows import Window
import numpy as np

def process_in_tiles(input_path, output_path, tile_size=1024):
    """Process a large raster in tiles to manage memory."""
    with rasterio.open(input_path) as src:
        profile = src.profile.copy()
        
        with rasterio.open(output_path, 'w', **profile) as dst:
            for row_off in range(0, src.height, tile_size):
                for col_off in range(0, src.width, tile_size):
                    # Calculate actual window size (may be smaller at edges)
                    win_height = min(tile_size, src.height - row_off)
                    win_width = min(tile_size, src.width - col_off)
                    
                    window = Window(col_off, row_off, win_width, win_height)
                    
                    # Read, process, write
                    data = src.read(window=window)
                    processed = your_processing_function(data)
                    dst.write(processed, window=window)
```

### Writing raster data

Creating new rasters requires specifying a profile (metadata dictionary):

```python
import numpy as np
import rasterio
from rasterio.transform import from_bounds

# Create synthetic data
data = np.random.rand(100, 100).astype(np.float32)

# Define the profile
profile = {
    'driver': 'GTiff',
    'dtype': 'float32',
    'width': 100,
    'height': 100,
    'count': 1,  # number of bands
    'crs': 'EPSG:32643',  # UTM zone 43N
    'transform': from_bounds(70.0, 40.0, 71.0, 41.0, 100, 100),
    'compress': 'lzw'
}

with rasterio.open('output.tif', 'w', **profile) as dst:
    dst.write(data, 1)  # Write to band 1
```

When deriving outputs from existing rasters, copy and modify the profile:

```python
with rasterio.open('input.tif') as src:
    profile = src.profile.copy()
    data = src.read(1)
    
    # Modify profile for different output type
    profile.update(dtype='float32', count=1)
    
    # Process data
    result = data.astype(np.float32) / 10000.0  # Scale reflectance
    
    with rasterio.open('output.tif', 'w', **profile) as dst:
        dst.write(result, 1)
```

### Reprojection

Reprojecting rasters to different coordinate systems is a common requirement:

```python
from rasterio.warp import calculate_default_transform, reproject, Resampling

def reproject_raster(input_path, output_path, dst_crs):
    """Reproject a raster to a new CRS."""
    with rasterio.open(input_path) as src:
        # Calculate the transform for the new CRS
        transform, width, height = calculate_default_transform(
            src.crs, dst_crs, src.width, src.height, *src.bounds
        )
        
        # Update the profile
        profile = src.profile.copy()
        profile.update(
            crs=dst_crs,
            transform=transform,
            width=width,
            height=height
        )
        
        with rasterio.open(output_path, 'w', **profile) as dst:
            for i in range(1, src.count + 1):
                reproject(
                    source=rasterio.band(src, i),
                    destination=rasterio.band(dst, i),
                    src_transform=src.transform,
                    src_crs=src.crs,
                    dst_transform=transform,
                    dst_crs=dst_crs,
                    resampling=Resampling.bilinear
                )

# Example: reproject from WGS84 to UTM
reproject_raster('wgs84_image.tif', 'utm_image.tif', 'EPSG:32643')
```

<div class="info-box tip">
  <strong>Key idea</strong>
  Resampling method matters. Use `nearest` for categorical data (land cover classes), `bilinear` or `cubic` for continuous data (reflectance, elevation), and `average` when downsampling to coarser resolution.
</div>

## Geopandas: vector data analysis

Geopandas extends Pandas to handle spatial data. It stores geometry in a special column and provides spatial operations alongside standard DataFrame functionality.

### Reading and exploring vector data

```python
import geopandas as gpd

# Read a shapefile
regions = gpd.read_file('central_asia_regions.shp')

# Basic exploration
print(f"Number of features: {len(regions)}")
print(f"Columns: {regions.columns.tolist()}")
print(f"CRS: {regions.crs}")
print(f"Bounds: {regions.total_bounds}")

# View first few rows
print(regions.head())

# Access geometry column
print(regions.geometry.head())
```

### Creating GeoDataFrames

You can create GeoDataFrames from coordinates or existing geometries:

```python
from shapely.geometry import Point, Polygon
import pandas as pd

# From a DataFrame with coordinates
df = pd.DataFrame({
    'name': ['Bishkek', 'Dushanbe', 'Tashkent', 'Almaty', 'Ashgabat'],
    'lat': [42.87, 38.56, 41.31, 43.24, 37.95],
    'lon': [74.59, 68.77, 69.28, 76.95, 58.38]
})

# Create geometry column
geometry = [Point(xy) for xy in zip(df.lon, df.lat)]

# Create GeoDataFrame
capitals = gpd.GeoDataFrame(df, geometry=geometry, crs='EPSG:4326')
print(capitals)

# Create from scratch with polygons
polygons = [
    Polygon([(70, 40), (71, 40), (71, 41), (70, 41)]),
    Polygon([(71, 40), (72, 40), (72, 41), (71, 41)])
]
gdf = gpd.GeoDataFrame({'id': [1, 2]}, geometry=polygons, crs='EPSG:4326')
```

### Geometric operations

Geopandas provides access to Shapely's geometric operations:

```python
# Buffer points (distance in CRS units)
# First reproject to a projected CRS for meaningful distances
capitals_utm = capitals.to_crs('EPSG:32643')
capitals_utm['buffer_10km'] = capitals_utm.buffer(10000)

# Calculate areas (requires projected CRS)
regions_utm = regions.to_crs('EPSG:32643')
regions_utm['area_km2'] = regions_utm.geometry.area / 1e6

# Centroid
regions_utm['centroid'] = regions_utm.geometry.centroid

# Simplify geometries (for visualization)
regions_simplified = regions_utm.copy()
regions_simplified['geometry'] = regions_utm.simplify(1000)

# Convex hull
regions_utm['convex_hull'] = regions_utm.geometry.convex_hull

# Bounding box
regions_utm['bbox'] = regions_utm.geometry.envelope
```

### Spatial relationships and joins

Spatial joins combine datasets based on geographic relationships:

```python
# Spatial join: find which region each capital is in
capitals_with_region = gpd.sjoin(
    capitals, 
    regions, 
    how='left', 
    predicate='within'
)
print(capitals_with_region[['name', 'region_name']])

# Different predicates: 'intersects', 'contains', 'within', 'touches', 'crosses'

# Spatial filter: select features intersecting a geometry
from shapely.geometry import box
bbox = box(68, 38, 72, 42)
subset = regions[regions.intersects(bbox)]

# Distance-based selection
# Find all features within 100km of a point
point = Point(69, 41)
capitals_utm = capitals.to_crs('EPSG:32643')
point_utm = gpd.GeoSeries([point], crs='EPSG:4326').to_crs('EPSG:32643')[0]
nearby = capitals_utm[capitals_utm.distance(point_utm) < 100000]
```

### Overlay operations

Overlay operations combine geometries from two layers:

```python
# Intersection: areas that exist in both layers
intersection = gpd.overlay(layer1, layer2, how='intersection')

# Union: combined geometries from both layers
union = gpd.overlay(layer1, layer2, how='union')

# Difference: areas in layer1 but not in layer2
difference = gpd.overlay(layer1, layer2, how='difference')

# Symmetric difference: areas in either but not both
sym_diff = gpd.overlay(layer1, layer2, how='symmetric_difference')

# Example: clip regions by a study area boundary
study_area = gpd.read_file('study_boundary.shp')
clipped = gpd.clip(regions, study_area)
```

### Dissolve and aggregate

Aggregate features based on attributes:

```python
# Dissolve regions by country
countries = regions.dissolve(by='country_name', aggfunc='sum')

# Dissolve with multiple aggregation functions
countries = regions.dissolve(
    by='country_name',
    aggfunc={
        'population': 'sum',
        'area_km2': 'sum',
        'num_districts': 'count'
    }
)
```

<div class="info-box tip">
  <strong>Key idea</strong>
  Always check your CRS before spatial operations. Distance calculations and area measurements require projected coordinate systems (meters), while spatial relationships work in any CRS but may give unexpected results if layers don't match.
</div>

## Xarray: multidimensional geospatial data

Xarray handles labeled, multidimensional arrays—essential for time series of satellite imagery, climate data, and hyperspectral cubes.

### Core concepts

Xarray introduces two main data structures:

- **DataArray**: a labeled N-dimensional array with named dimensions
- **Dataset**: a collection of DataArrays sharing dimensions

```python
import xarray as xr
import numpy as np

# Create a DataArray
time = pd.date_range('2020-01-01', periods=12, freq='MS')
lat = np.linspace(40, 42, 20)
lon = np.linspace(70, 72, 25)

# Random temperature data: (time, lat, lon)
data = np.random.rand(12, 20, 25) * 30 - 10

temp = xr.DataArray(
    data,
    dims=['time', 'lat', 'lon'],
    coords={
        'time': time,
        'lat': lat,
        'lon': lon
    },
    attrs={'units': 'degrees_C', 'long_name': 'Temperature'}
)

print(temp)
```

### Selecting data

Xarray provides intuitive selection by labels or indices:

```python
# Select by label
jan_data = temp.sel(time='2020-01')
region = temp.sel(lat=slice(40.5, 41.5), lon=slice(70.5, 71.5))

# Select nearest value
point = temp.sel(lat=41.3, lon=71.2, method='nearest')

# Select by index
first_timestep = temp.isel(time=0)
subset = temp.isel(time=slice(0, 6), lat=slice(5, 15))

# Boolean selection
warm = temp.where(temp > 20)
```

### Computation

Xarray operations preserve labels and handle missing data:

```python
# Statistics along dimensions
monthly_mean = temp.mean(dim='time')
spatial_mean = temp.mean(dim=['lat', 'lon'])

# Anomalies from climatology
climatology = temp.groupby('time.month').mean()
anomalies = temp.groupby('time.month') - climatology

# Rolling windows
rolling_mean = temp.rolling(time=3, center=True).mean()

# Resampling in time
quarterly = temp.resample(time='QS').mean()
annual = temp.resample(time='YS').sum()
```

### Reading NetCDF and other formats

```python
# Open a single NetCDF file
ds = xr.open_dataset('climate_data.nc')
print(ds)

# Access variables
temperature = ds['temperature']
precipitation = ds['precipitation']

# Open multiple files as a single dataset
ds = xr.open_mfdataset('data_*.nc', combine='by_coords')

# Lazy loading with Dask (for large datasets)
ds = xr.open_dataset('large_file.nc', chunks={'time': 10})
```

### Working with Datasets

```python
# Create a Dataset
ds = xr.Dataset({
    'temperature': temp,
    'precipitation': xr.DataArray(
        np.random.rand(12, 20, 25) * 100,
        dims=['time', 'lat', 'lon'],
        coords={'time': time, 'lat': lat, 'lon': lon}
    )
})

# Add variables
ds['ndvi'] = xr.DataArray(
    np.random.rand(12, 20, 25),
    dims=['time', 'lat', 'lon']
)

# Save to NetCDF
ds.to_netcdf('output.nc')

# Subset and save
ds.sel(time='2020-06').to_netcdf('june_2020.nc')
```

## Rioxarray: bridging rasterio and xarray

Rioxarray adds geospatial capabilities to xarray, handling CRS information, spatial coordinates, and geospatial I/O.

### Reading geospatial rasters

```python
import rioxarray
import xarray as xr

# Open a GeoTIFF with rioxarray
da = xr.open_dataarray('sentinel2.tif', engine='rasterio')

# Or use rioxarray directly
da = rioxarray.open_rasterio('sentinel2.tif')

# Access geospatial information
print(f"CRS: {da.rio.crs}")
print(f"Resolution: {da.rio.resolution()}")
print(f"Bounds: {da.rio.bounds()}")
print(f"NoData: {da.rio.nodata}")
```

### Opening time series of images

```python
from pathlib import Path
import xarray as xr

# List all GeoTIFFs
files = sorted(Path('imagery').glob('sentinel2_*.tif'))

# Open each file and stack along time dimension
datasets = []
for f in files:
    # Extract date from filename
    date = pd.to_datetime(f.stem.split('_')[1])
    
    da = rioxarray.open_rasterio(f)
    da = da.expand_dims(time=[date])
    datasets.append(da)

# Concatenate along time
time_series = xr.concat(datasets, dim='time')
print(time_series)
```

### Clipping and masking

```python
import geopandas as gpd

# Load a boundary
boundary = gpd.read_file('study_area.shp')

# Clip raster to boundary
clipped = da.rio.clip(boundary.geometry, boundary.crs)

# Clip with all_touched=True to include all pixels touching the boundary
clipped = da.rio.clip(boundary.geometry, boundary.crs, all_touched=True)

# Clip to bounding box
from shapely.geometry import box
bbox = box(70.5, 40.5, 71.5, 41.5)
clipped = da.rio.clip([bbox], 'EPSG:4326')
```

### Reprojection

```python
# Reproject to a new CRS
da_utm = da.rio.reproject('EPSG:32643')

# Reproject with specific resolution
da_resampled = da.rio.reproject(
    'EPSG:32643',
    resolution=30,
    resampling=rasterio.enums.Resampling.bilinear
)

# Match another dataset's grid
reference = rioxarray.open_rasterio('reference.tif')
da_matched = da.rio.reproject_match(reference)
```

### Writing geospatial data

```python
# Write CRS and spatial information
da.rio.write_crs('EPSG:4326', inplace=True)
da.rio.write_nodata(-9999, inplace=True)

# Save to GeoTIFF
da.rio.to_raster('output.tif', driver='GTiff', compress='LZW')

# Save specific bands
da.sel(band=[1, 2, 3]).rio.to_raster('rgb.tif')
```

<div class="info-box tip">
  <strong>Key idea</strong>
  Rioxarray lets you use xarray's powerful computation (groupby, rolling, resampling) while maintaining geospatial metadata. This is essential for time series analysis of satellite imagery.
</div>

## Coordinate reference systems and transformations

Understanding coordinate reference systems (CRS) is fundamental to geospatial work. Errors in CRS handling cause misaligned data and incorrect measurements.

### CRS fundamentals

A CRS defines how coordinates map to locations on Earth. Two main types exist:

- **Geographic CRS**: coordinates are latitude/longitude angles (e.g., EPSG:4326/WGS84)
- **Projected CRS**: coordinates are distances on a flat surface (e.g., UTM zones)

```python
from pyproj import CRS

# Create CRS objects
wgs84 = CRS.from_epsg(4326)
utm43n = CRS.from_epsg(32643)  # UTM zone 43N, covers much of Central Asia

# Inspect CRS properties
print(f"WGS84 is geographic: {wgs84.is_geographic}")
print(f"UTM43N is projected: {utm43n.is_projected}")
print(f"UTM43N units: {utm43n.axis_info[0].unit_name}")

# Get CRS from various formats
crs_from_wkt = CRS.from_wkt(wgs84.to_wkt())
crs_from_proj4 = CRS.from_proj4('+proj=longlat +datum=WGS84')
```

### Transformations

Transform coordinates between CRS using pyproj:

```python
from pyproj import Transformer

# Create a transformer
transformer = Transformer.from_crs('EPSG:4326', 'EPSG:32643', always_xy=True)

# Transform a single point (lon, lat) -> (x, y)
x, y = transformer.transform(74.59, 42.87)  # Bishkek
print(f"Bishkek in UTM: ({x:.2f}, {y:.2f})")

# Transform arrays of coordinates
lons = [74.59, 68.77, 69.28]
lats = [42.87, 38.56, 41.31]
xs, ys = transformer.transform(lons, lats)

# Transform bounds
bounds_wgs84 = (70, 40, 72, 42)  # minx, miny, maxx, maxy
bounds_utm = transformer.transform_bounds(*bounds_wgs84)
```

### Choosing appropriate CRS

Different analyses require different CRS:

```python
def choose_utm_zone(lon):
    """Determine UTM zone from longitude."""
    zone = int((lon + 180) / 6) + 1
    return zone

# Central Asia spans UTM zones 40-45
cities = {
    'Ashgabat': (58.38, 37.95),   # UTM 40N
    'Tashkent': (69.28, 41.31),   # UTM 42N
    'Bishkek': (74.59, 42.87),    # UTM 43N
    'Almaty': (76.95, 43.24)      # UTM 43N
}

for city, (lon, lat) in cities.items():
    zone = choose_utm_zone(lon)
    epsg = 32600 + zone  # Northern hemisphere UTM
    print(f"{city}: UTM zone {zone}N (EPSG:{epsg})")
```

For regional analyses, consider:

```python
# Equal-area projection for area calculations
# Asia North Albers Equal Area Conic
albers = CRS.from_proj4(
    '+proj=aea +lat_1=15 +lat_2=65 +lat_0=30 +lon_0=95 '
    '+x_0=0 +y_0=0 +datum=WGS84 +units=m +no_defs'
)

# Equidistant projection for distance measurements
# Azimuthal Equidistant centered on Central Asia
aeqd = CRS.from_proj4(
    '+proj=aeqd +lat_0=41 +lon_0=70 +x_0=0 +y_0=0 '
    '+datum=WGS84 +units=m +no_defs'
)
```

<div class="info-box tip">
  <strong>Key idea</strong>
  Use geographic CRS (lat/lon) for data storage and exchange. Use projected CRS for measurements. UTM provides good local accuracy; equal-area projections preserve area across regions; equidistant projections preserve distances from a center point.
</div>

## Zonal statistics and raster-vector interaction

Combining raster and vector data extracts meaningful statistics: average NDVI per field, maximum elevation per watershed, precipitation totals per province.

### Zonal statistics with rasterstats

```python
from rasterstats import zonal_stats
import geopandas as gpd

# Load zones (polygons)
zones = gpd.read_file('districts.shp')

# Calculate zonal statistics
stats = zonal_stats(
    zones,
    'ndvi.tif',
    stats=['min', 'max', 'mean', 'std', 'count']
)

# Result is a list of dictionaries
for i, s in enumerate(stats):
    print(f"Zone {i}: mean={s['mean']:.3f}, std={s['std']:.3f}")

# Add statistics to GeoDataFrame
zones['ndvi_mean'] = [s['mean'] for s in stats]
zones['ndvi_std'] = [s['std'] for s in stats]
```

### Custom statistics and categorical rasters

```python
# Percentiles and custom functions
stats = zonal_stats(
    zones,
    'elevation.tif',
    stats=['percentile_10', 'percentile_50', 'percentile_90'],
    add_stats={'range': lambda x: x.max() - x.min()}
)

# For categorical rasters (land cover)
cat_stats = zonal_stats(
    zones,
    'landcover.tif',
    categorical=True,
    category_map={1: 'forest', 2: 'cropland', 3: 'urban', 4: 'water'}
)

# Get pixel counts per category
for i, s in enumerate(cat_stats):
    print(f"Zone {i}: {s}")
```

### Point sampling from rasters

Extract raster values at point locations:

```python
from rasterstats import point_query
import geopandas as gpd

# Sample points
samples = gpd.read_file('sample_points.shp')

# Extract values
values = point_query(samples, 'dem.tif')
samples['elevation'] = values

# For multiband rasters, sample each band
with rasterio.open('sentinel2.tif') as src:
    # Convert points to (x, y) coordinates
    coords = [(p.x, p.y) for p in samples.geometry]
    
    # Sample all bands
    samples['band_values'] = [list(src.sample([coord]))[0] for coord in coords]
```

### Rasterizing vector data

Convert vector features to raster format:

```python
from rasterio.features import rasterize
import rasterio

# Load vector data
zones = gpd.read_file('zones.shp')

# Create shapes for rasterization: (geometry, value) pairs
shapes = ((geom, value) for geom, value in zip(zones.geometry, zones['zone_id']))

# Open a reference raster for extent and resolution
with rasterio.open('reference.tif') as src:
    out_shape = (src.height, src.width)
    transform = src.transform
    
    # Rasterize
    rasterized = rasterize(
        shapes,
        out_shape=out_shape,
        transform=transform,
        fill=0,
        dtype='int32'
    )

# Save the result
profile = src.profile.copy()
profile.update(dtype='int32', count=1)
with rasterio.open('zones_raster.tif', 'w', **profile) as dst:
    dst.write(rasterized, 1)
```

### Extracting raster values along lines

For transects or profiles:

```python
from shapely.geometry import LineString
import numpy as np

def extract_profile(raster_path, line, num_points=100):
    """Extract values along a line at regular intervals."""
    # Generate points along the line
    distances = np.linspace(0, line.length, num_points)
    points = [line.interpolate(d) for d in distances]
    
    with rasterio.open(raster_path) as src:
        values = []
        for point in points:
            # Sample the raster
            row, col = rasterio.transform.rowcol(src.transform, point.x, point.y)
            if 0 <= row < src.height and 0 <= col < src.width:
                values.append(src.read(1)[row, col])
            else:
                values.append(np.nan)
    
    return distances, values

# Example usage
line = LineString([(70, 40), (72, 42)])
distances, elevations = extract_profile('dem.tif', line)
```

## Visualization: matplotlib, contextily, and folium

Effective visualization communicates spatial patterns and supports analysis.

### Basic matplotlib maps

```python
import matplotlib.pyplot as plt
import geopandas as gpd

# Simple vector map
fig, ax = plt.subplots(figsize=(10, 8))
regions.plot(ax=ax, column='population', cmap='YlOrRd', 
             legend=True, edgecolor='black', linewidth=0.5)
ax.set_title('Population by Region')
ax.set_xlabel('Longitude')
ax.set_ylabel('Latitude')
plt.tight_layout()
plt.savefig('population_map.png', dpi=150)
```

### Raster visualization

```python
import rasterio
from rasterio.plot import show

# Simple raster display
with rasterio.open('dem.tif') as src:
    fig, ax = plt.subplots(figsize=(10, 8))
    show(src, ax=ax, cmap='terrain')
    ax.set_title('Digital Elevation Model')
    plt.colorbar(ax.images[0], ax=ax, label='Elevation (m)')
    plt.tight_layout()
    plt.savefig('dem_map.png', dpi=150)

# RGB composite
with rasterio.open('sentinel2.tif') as src:
    fig, ax = plt.subplots(figsize=(10, 8))
    # Read RGB bands and normalize
    rgb = src.read([4, 3, 2])  # Red, Green, Blue bands
    rgb_norm = (rgb - rgb.min()) / (rgb.max() - rgb.min())
    show(rgb_norm, ax=ax, transform=src.transform)
    ax.set_title('Sentinel-2 True Color')
```

### Adding basemaps with contextily

```python
import contextily as ctx

# Vector data with basemap
fig, ax = plt.subplots(figsize=(12, 10))

# Plot must be in Web Mercator for contextily
regions_wm = regions.to_crs(epsg=3857)
regions_wm.plot(ax=ax, alpha=0.5, edgecolor='black', facecolor='none', linewidth=2)

# Add basemap
ctx.add_basemap(ax, source=ctx.providers.OpenTopoMap)
ax.set_title('Central Asia Regions')
ax.set_axis_off()
plt.tight_layout()
plt.savefig('map_with_basemap.png', dpi=150)
```

### Interactive maps with Folium

```python
import folium

# Create a base map centered on Central Asia
m = folium.Map(
    location=[41, 70],
    zoom_start=5,
    tiles='OpenStreetMap'
)

# Add vector data
folium.GeoJson(
    regions.to_crs(epsg=4326).__geo_interface__,
    style_function=lambda x: {
        'fillColor': '#3186cc',
        'color': 'black',
        'weight': 1,
        'fillOpacity': 0.3
    },
    tooltip=folium.GeoJsonTooltip(fields=['name', 'population'])
).add_to(m)

# Add markers
for idx, row in capitals.iterrows():
    folium.Marker(
        location=[row.geometry.y, row.geometry.x],
        popup=row['name'],
        icon=folium.Icon(color='red', icon='star')
    ).add_to(m)

# Save to HTML
m.save('interactive_map.html')
```

### Multi-panel figures

```python
fig, axes = plt.subplots(2, 2, figsize=(14, 12))

# Panel 1: Original image
with rasterio.open('sentinel2.tif') as src:
    show(src.read([4, 3, 2]), ax=axes[0, 0], transform=src.transform)
axes[0, 0].set_title('True Color')

# Panel 2: NDVI
with rasterio.open('ndvi.tif') as src:
    im = show(src, ax=axes[0, 1], cmap='RdYlGn', vmin=-1, vmax=1)
axes[0, 1].set_title('NDVI')
plt.colorbar(axes[0, 1].images[0], ax=axes[0, 1])

# Panel 3: Land cover
with rasterio.open('landcover.tif') as src:
    show(src, ax=axes[1, 0], cmap='tab10')
axes[1, 0].set_title('Land Cover')

# Panel 4: Elevation
with rasterio.open('dem.tif') as src:
    show(src, ax=axes[1, 1], cmap='terrain')
axes[1, 1].set_title('Elevation')
plt.colorbar(axes[1, 1].images[0], ax=axes[1, 1])

plt.tight_layout()
plt.savefig('multi_panel.png', dpi=150)
```

<div class="info-box tip">
  <strong>Key idea</strong>
  Choose visualization tools based on audience. Matplotlib produces publication-quality static figures. Contextily adds geographic context with basemaps. Folium creates interactive web maps for exploration and sharing.
</div>

## Automation and batch processing

Real-world geospatial projects involve processing many files. Automation ensures consistency and saves time.

### Processing multiple files

```python
from pathlib import Path
import geopandas as gpd
import rasterio
from rasterstats import zonal_stats
import pandas as pd

def process_ndvi_timeseries(image_dir, zones_path, output_csv):
    """Calculate zonal NDVI statistics for a time series of images."""
    
    # Load zones
    zones = gpd.read_file(zones_path)
    
    # Find all NDVI images
    image_files = sorted(Path(image_dir).glob('ndvi_*.tif'))
    
    results = []
    for img_path in image_files:
        # Extract date from filename
        date_str = img_path.stem.split('_')[1]
        
        # Calculate zonal statistics
        stats = zonal_stats(zones, str(img_path), stats=['mean', 'std'])
        
        # Store results
        for i, s in enumerate(stats):
            results.append({
                'zone_id': zones.iloc[i]['id'],
                'date': date_str,
                'ndvi_mean': s['mean'],
                'ndvi_std': s['std']
            })
    
    # Create DataFrame and save
    df = pd.DataFrame(results)
    df.to_csv(output_csv, index=False)
    return df

# Usage
df = process_ndvi_timeseries('imagery/', 'zones.shp', 'ndvi_timeseries.csv')
```

### Parallel processing

```python
from concurrent.futures import ProcessPoolExecutor
from pathlib import Path
import rasterio
from rasterio.warp import calculate_default_transform, reproject, Resampling

def reproject_single_file(args):
    """Reproject a single file to UTM."""
    input_path, output_dir, dst_crs = args
    output_path = Path(output_dir) / f"{input_path.stem}_utm.tif"
    
    with rasterio.open(input_path) as src:
        transform, width, height = calculate_default_transform(
            src.crs, dst_crs, src.width, src.height, *src.bounds
        )
        
        profile = src.profile.copy()
        profile.update(crs=dst_crs, transform=transform, width=width, height=height)
        
        with rasterio.open(output_path, 'w', **profile) as dst:
            for i in range(1, src.count + 1):
                reproject(
                    source=rasterio.band(src, i),
                    destination=rasterio.band(dst, i),
                    src_transform=src.transform,
                    src_crs=src.crs,
                    dst_transform=transform,
                    dst_crs=dst_crs,
                    resampling=Resampling.bilinear
                )
    
    return output_path

def batch_reproject(input_dir, output_dir, dst_crs, max_workers=4):
    """Reproject multiple files in parallel."""
    input_files = list(Path(input_dir).glob('*.tif'))
    args_list = [(f, output_dir, dst_crs) for f in input_files]
    
    with ProcessPoolExecutor(max_workers=max_workers) as executor:
        results = list(executor.map(reproject_single_file, args_list))
    
    return results

# Usage
results = batch_reproject('wgs84_images/', 'utm_images/', 'EPSG:32643')
```

### Command-line interface

```python
#!/usr/bin/env python
"""Process satellite imagery for Central Asia analysis."""

import argparse
from pathlib import Path
import geopandas as gpd
import rasterio
from rasterstats import zonal_stats
import pandas as pd

def main():
    parser = argparse.ArgumentParser(description='Calculate zonal statistics')
    parser.add_argument('raster', help='Input raster file')
    parser.add_argument('zones', help='Zones shapefile')
    parser.add_argument('output', help='Output CSV file')
    parser.add_argument('--stats', nargs='+', default=['mean', 'std'],
                        help='Statistics to calculate')
    
    args = parser.parse_args()
    
    # Load zones
    zones = gpd.read_file(args.zones)
    
    # Calculate statistics
    stats = zonal_stats(zones, args.raster, stats=args.stats)
    
    # Build output dataframe
    df = zones.drop(columns='geometry').copy()
    for stat in args.stats:
        df[stat] = [s[stat] for s in stats]
    
    # Save
    df.to_csv(args.output, index=False)
    print(f"Saved results to {args.output}")

if __name__ == '__main__':
    main()
```

### Logging and error handling

```python
import logging
from pathlib import Path
import rasterio

# Configure logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler('processing.log'),
        logging.StreamHandler()
    ]
)
logger = logging.getLogger(__name__)

def safe_process_file(input_path, output_path, process_func):
    """Process a file with error handling and logging."""
    logger.info(f"Processing: {input_path}")
    
    try:
        result = process_func(input_path, output_path)
        logger.info(f"Success: {output_path}")
        return result
    except rasterio.errors.RasterioIOError as e:
        logger.error(f"IO Error processing {input_path}: {e}")
    except ValueError as e:
        logger.error(f"Value Error processing {input_path}: {e}")
    except Exception as e:
        logger.exception(f"Unexpected error processing {input_path}: {e}")
    
    return None

def batch_process_with_logging(input_dir, output_dir, process_func):
    """Process multiple files with comprehensive logging."""
    input_files = list(Path(input_dir).glob('*.tif'))
    logger.info(f"Found {len(input_files)} files to process")
    
    successful = 0
    failed = 0
    
    for input_path in input_files:
        output_path = Path(output_dir) / f"{input_path.stem}_processed.tif"
        result = safe_process_file(input_path, output_path, process_func)
        
        if result is not None:
            successful += 1
        else:
            failed += 1
    
    logger.info(f"Processing complete: {successful} successful, {failed} failed")
    return successful, failed
```

## Checklist

Before starting a geospatial Python project, ensure you have:

<ul class="checklist">
  <li>Installed core libraries: numpy, pandas, geopandas, rasterio, shapely, pyproj</li>
  <li>Installed xarray and rioxarray for multidimensional data</li>
  <li>Installed visualization libraries: matplotlib, contextily, folium</li>
  <li>Installed rasterstats for zonal statistics</li>
  <li>Verified GDAL installation and version compatibility</li>
  <li>Set up a consistent project directory structure</li>
  <li>Documented the CRS for all input datasets</li>
  <li>Checked that vector and raster data align spatially</li>
  <li>Created a requirements.txt or environment.yml for reproducibility</li>
  <li>Set up logging for batch processing workflows</li>
  <li>Tested code on a small subset before processing full datasets</li>
  <li>Verified output files have correct CRS and metadata</li>
</ul>

## Exercises

Work through these exercises to build proficiency with Python geospatial libraries. Each exercise targets specific skills from the tutorial.

<div class="exercises">
  <h3>Exercise 1: Read and inspect a GeoTIFF</h3>
  <p>Open a GeoTIFF file with rasterio and print its width, height, CRS, bounds, and data type.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">import rasterio

with rasterio.open('image.tif') as src:
    print(f"Width: {src.width}")
    print(f"Height: {src.height}")
    print(f"CRS: {src.crs}")
    print(f"Bounds: {src.bounds}")
    print(f"Data type: {src.dtypes[0]}")
    print(f"Number of bands: {src.count}")</code></pre>
  </details>

  <h3>Exercise 2: Calculate NDVI from Sentinel-2 bands</h3>
  <p>Given a multi-band Sentinel-2 image, calculate NDVI using bands 4 (Red) and 8 (NIR), and save the result.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">import rasterio
import numpy as np

with rasterio.open('sentinel2.tif') as src:
    red = src.read(4).astype(np.float32)
    nir = src.read(8).astype(np.float32)
    
    # Avoid division by zero
    ndvi = (nir - red) / (nir + red + 1e-10)
    ndvi = np.clip(ndvi, -1, 1)
    
    profile = src.profile.copy()
    profile.update(dtype='float32', count=1)
    
    with rasterio.open('ndvi.tif', 'w', **profile) as dst:
        dst.write(ndvi, 1)</code></pre>
  </details>

  <h3>Exercise 3: Window reading for large files</h3>
  <p>Read a 512x512 pixel window from a large raster starting at row 1000, column 2000.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">from rasterio.windows import Window
import rasterio

with rasterio.open('large_image.tif') as src:
    window = Window(col_off=2000, row_off=1000, width=512, height=512)
    data = src.read(1, window=window)
    window_transform = src.window_transform(window)
    print(f"Window shape: {data.shape}")
    print(f"Window transform: {window_transform}")</code></pre>
  </details>

  <h3>Exercise 4: Reproject a raster to UTM</h3>
  <p>Reproject a WGS84 raster to UTM Zone 43N (EPSG:32643) using bilinear resampling.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">import rasterio
from rasterio.warp import calculate_default_transform, reproject, Resampling

with rasterio.open('wgs84_image.tif') as src:
    dst_crs = 'EPSG:32643'
    transform, width, height = calculate_default_transform(
        src.crs, dst_crs, src.width, src.height, *src.bounds
    )
    
    profile = src.profile.copy()
    profile.update(crs=dst_crs, transform=transform, width=width, height=height)
    
    with rasterio.open('utm_image.tif', 'w', **profile) as dst:
        for i in range(1, src.count + 1):
            reproject(
                source=rasterio.band(src, i),
                destination=rasterio.band(dst, i),
                src_transform=src.transform,
                src_crs=src.crs,
                dst_transform=transform,
                dst_crs=dst_crs,
                resampling=Resampling.bilinear
            )</code></pre>
  </details>

  <h3>Exercise 5: Create a GeoDataFrame from coordinates</h3>
  <p>Create a GeoDataFrame containing five Central Asian capitals with their names and coordinates.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">import geopandas as gpd
import pandas as pd
from shapely.geometry import Point

data = {
    'name': ['Bishkek', 'Dushanbe', 'Tashkent', 'Almaty', 'Ashgabat'],
    'country': ['Kyrgyzstan', 'Tajikistan', 'Uzbekistan', 'Kazakhstan', 'Turkmenistan'],
    'lat': [42.87, 38.56, 41.31, 43.24, 37.95],
    'lon': [74.59, 68.77, 69.28, 76.95, 58.38]
}

df = pd.DataFrame(data)
geometry = [Point(lon, lat) for lon, lat in zip(df['lon'], df['lat'])]
gdf = gpd.GeoDataFrame(df, geometry=geometry, crs='EPSG:4326')
print(gdf)</code></pre>
  </details>

  <h3>Exercise 6: Perform a spatial join</h3>
  <p>Join point data to polygon data to find which region each point falls within.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">import geopandas as gpd

points = gpd.read_file('sample_points.shp')
regions = gpd.read_file('regions.shp')

# Ensure same CRS
points = points.to_crs(regions.crs)

# Spatial join
joined = gpd.sjoin(points, regions, how='left', predicate='within')
print(joined[['point_id', 'region_name']])</code></pre>
  </details>

  <h3>Exercise 7: Buffer and dissolve</h3>
  <p>Create 10km buffers around points and dissolve overlapping buffers into single polygons.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">import geopandas as gpd

points = gpd.read_file('points.shp')

# Reproject to UTM for accurate distances
points_utm = points.to_crs('EPSG:32643')

# Buffer 10km
buffered = points_utm.copy()
buffered['geometry'] = points_utm.buffer(10000)

# Dissolve all overlapping buffers
dissolved = buffered.dissolve()
print(f"Original features: {len(points)}")
print(f"After dissolve: {len(dissolved)}")</code></pre>
  </details>

  <h3>Exercise 8: Calculate area in square kilometers</h3>
  <p>Calculate the area of each polygon in a GeoDataFrame in square kilometers.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">import geopandas as gpd

regions = gpd.read_file('regions.shp')

# Must use projected CRS for accurate area
regions_utm = regions.to_crs('EPSG:32643')

# Calculate area in km²
regions_utm['area_km2'] = regions_utm.geometry.area / 1e6

print(regions_utm[['name', 'area_km2']])</code></pre>
  </details>

  <h3>Exercise 9: Clip vectors to a bounding box</h3>
  <p>Clip a vector dataset to a rectangular bounding box defined by coordinates.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">import geopandas as gpd
from shapely.geometry import box

regions = gpd.read_file('regions.shp')

# Define bounding box (minx, miny, maxx, maxy)
bbox = box(68, 38, 72, 42)
bbox_gdf = gpd.GeoDataFrame(geometry=[bbox], crs='EPSG:4326')

# Clip
clipped = gpd.clip(regions, bbox_gdf)
print(f"Original features: {len(regions)}")
print(f"After clipping: {len(clipped)}")</code></pre>
  </details>

  <h3>Exercise 10: Create an xarray DataArray</h3>
  <p>Create a 3D xarray DataArray with dimensions (time, lat, lon) representing monthly temperature data.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">import xarray as xr
import numpy as np
import pandas as pd

# Create coordinates
time = pd.date_range('2020-01-01', periods=12, freq='MS')
lat = np.linspace(40, 42, 20)
lon = np.linspace(70, 72, 25)

# Create random temperature data
data = np.random.rand(12, 20, 25) * 30 - 10

# Create DataArray
temp = xr.DataArray(
    data,
    dims=['time', 'lat', 'lon'],
    coords={'time': time, 'lat': lat, 'lon': lon},
    attrs={'units': 'degrees_C', 'long_name': 'Monthly Temperature'}
)

print(temp)</code></pre>
  </details>

  <h3>Exercise 11: Select data by label in xarray</h3>
  <p>From a time series DataArray, select data for June 2020 and a spatial subset between lat 40.5-41.5 and lon 70.5-71.5.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">import xarray as xr

# Assuming 'temp' DataArray exists from Exercise 10
june_data = temp.sel(time='2020-06')
print(f"June data shape: {june_data.shape}")

spatial_subset = temp.sel(
    lat=slice(40.5, 41.5),
    lon=slice(70.5, 71.5)
)
print(f"Spatial subset shape: {spatial_subset.shape}")</code></pre>
  </details>

  <h3>Exercise 12: Calculate monthly climatology</h3>
  <p>Calculate the monthly mean climatology from a multi-year temperature dataset using groupby.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">import xarray as xr
import numpy as np
import pandas as pd

# Create 3-year dataset
time = pd.date_range('2018-01-01', periods=36, freq='MS')
data = np.random.rand(36, 20, 25) * 30 - 10

temp = xr.DataArray(data, dims=['time', 'lat', 'lon'],
                    coords={'time': time})

# Calculate monthly climatology
climatology = temp.groupby('time.month').mean(dim='time')
print(f"Climatology shape: {climatology.shape}")
print(climatology)</code></pre>
  </details>

  <h3>Exercise 13: Open a GeoTIFF with rioxarray</h3>
  <p>Open a GeoTIFF using rioxarray and print its CRS, resolution, and bounds.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">import rioxarray

da = rioxarray.open_rasterio('image.tif')

print(f"CRS: {da.rio.crs}")
print(f"Resolution: {da.rio.resolution()}")
print(f"Bounds: {da.rio.bounds()}")
print(f"NoData: {da.rio.nodata}")
print(f"Shape: {da.shape}")</code></pre>
  </details>

  <h3>Exercise 14: Clip a raster to a polygon</h3>
  <p>Use rioxarray to clip a raster to the boundary of a polygon from a shapefile.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">import rioxarray
import geopandas as gpd

# Load raster and vector
da = rioxarray.open_rasterio('image.tif')
boundary = gpd.read_file('boundary.shp')

# Clip
clipped = da.rio.clip(boundary.geometry, boundary.crs)

# Save result
clipped.rio.to_raster('clipped.tif')</code></pre>
  </details>

  <h3>Exercise 15: Reproject with rioxarray</h3>
  <p>Reproject a raster to a new CRS and resample to 30m resolution using rioxarray.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">import rioxarray
from rasterio.enums import Resampling

da = rioxarray.open_rasterio('image.tif')

# Reproject to UTM with 30m resolution
da_reprojected = da.rio.reproject(
    'EPSG:32643',
    resolution=30,
    resampling=Resampling.bilinear
)

print(f"New CRS: {da_reprojected.rio.crs}")
print(f"New resolution: {da_reprojected.rio.resolution()}")

da_reprojected.rio.to_raster('reprojected.tif')</code></pre>
  </details>

  <h3>Exercise 16: Transform coordinates with pyproj</h3>
  <p>Transform a list of lat/lon coordinates to UTM Zone 43N coordinates.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">from pyproj import Transformer

# Create transformer (always_xy ensures lon, lat order)
transformer = Transformer.from_crs('EPSG:4326', 'EPSG:32643', always_xy=True)

# Coordinates (lon, lat)
lons = [74.59, 68.77, 69.28]
lats = [42.87, 38.56, 41.31]

# Transform
xs, ys = transformer.transform(lons, lats)

for lon, lat, x, y in zip(lons, lats, xs, ys):
    print(f"({lon}, {lat}) -> ({x:.2f}, {y:.2f})")</code></pre>
  </details>

  <h3>Exercise 17: Determine UTM zone from longitude</h3>
  <p>Write a function that returns the appropriate UTM EPSG code for a given longitude in the Northern Hemisphere.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">def get_utm_epsg(lon, northern_hemisphere=True):
    """Get UTM EPSG code for a longitude."""
    zone = int((lon + 180) / 6) + 1
    if northern_hemisphere:
        epsg = 32600 + zone
    else:
        epsg = 32700 + zone
    return epsg, zone

# Test with Central Asian cities
cities = [('Ashgabat', 58.38), ('Tashkent', 69.28), ('Bishkek', 74.59)]
for name, lon in cities:
    epsg, zone = get_utm_epsg(lon)
    print(f"{name}: UTM Zone {zone}N (EPSG:{epsg})")</code></pre>
  </details>

  <h3>Exercise 18: Calculate zonal statistics</h3>
  <p>Calculate mean, min, max, and standard deviation of a raster for each polygon in a zones shapefile.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">from rasterstats import zonal_stats
import geopandas as gpd

zones = gpd.read_file('zones.shp')

stats = zonal_stats(
    zones,
    'elevation.tif',
    stats=['min', 'max', 'mean', 'std', 'count']
)

# Add to GeoDataFrame
for stat_name in ['min', 'max', 'mean', 'std', 'count']:
    zones[f'elev_{stat_name}'] = [s[stat_name] for s in stats]

print(zones[['zone_id', 'elev_mean', 'elev_std']])</code></pre>
  </details>

  <h3>Exercise 19: Categorical zonal statistics</h3>
  <p>Calculate the pixel count of each land cover class within each zone.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">from rasterstats import zonal_stats
import geopandas as gpd
import pandas as pd

zones = gpd.read_file('zones.shp')

# Categorical statistics
cat_stats = zonal_stats(
    zones,
    'landcover.tif',
    categorical=True
)

# Convert to DataFrame
cat_df = pd.DataFrame(cat_stats)
cat_df.columns = [f'class_{c}' for c in cat_df.columns]

# Join back to zones
zones_with_lc = zones.join(cat_df)
print(zones_with_lc.head())</code></pre>
  </details>

  <h3>Exercise 20: Point sampling from raster</h3>
  <p>Extract raster values at a set of sample point locations.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">from rasterstats import point_query
import geopandas as gpd

points = gpd.read_file('sample_points.shp')

# Sample values
values = point_query(points, 'dem.tif')

# Add to GeoDataFrame
points['elevation'] = values

print(points[['point_id', 'elevation']])</code></pre>
  </details>

  <h3>Exercise 21: Rasterize vector data</h3>
  <p>Convert a polygon shapefile to a raster with the same extent and resolution as a reference raster.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">from rasterio.features import rasterize
import rasterio
import geopandas as gpd

zones = gpd.read_file('zones.shp')

# Create (geometry, value) pairs
shapes = ((geom, val) for geom, val in zip(zones.geometry, zones['zone_id']))

# Use reference raster for extent
with rasterio.open('reference.tif') as src:
    rasterized = rasterize(
        shapes,
        out_shape=(src.height, src.width),
        transform=src.transform,
        fill=0,
        dtype='int32'
    )
    
    profile = src.profile.copy()
    profile.update(dtype='int32', count=1)

# Save
with rasterio.open('zones_raster.tif', 'w', **profile) as dst:
    dst.write(rasterized, 1)</code></pre>
  </details>

  <h3>Exercise 22: Create a simple matplotlib map</h3>
  <p>Create a choropleth map showing population by region using matplotlib.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">import geopandas as gpd
import matplotlib.pyplot as plt

regions = gpd.read_file('regions.shp')

fig, ax = plt.subplots(figsize=(10, 8))

regions.plot(
    ax=ax,
    column='population',
    cmap='YlOrRd',
    legend=True,
    legend_kwds={'label': 'Population'},
    edgecolor='black',
    linewidth=0.5
)

ax.set_title('Population by Region')
ax.set_xlabel('Longitude')
ax.set_ylabel('Latitude')

plt.tight_layout()
plt.savefig('population_map.png', dpi=150)</code></pre>
  </details>

  <h3>Exercise 23: Add a basemap with contextily</h3>
  <p>Create a map with vector data overlaid on an OpenStreetMap basemap.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">import geopandas as gpd
import matplotlib.pyplot as plt
import contextily as ctx

regions = gpd.read_file('regions.shp')

# Convert to Web Mercator for basemap
regions_wm = regions.to_crs(epsg=3857)

fig, ax = plt.subplots(figsize=(12, 10))

regions_wm.plot(
    ax=ax,
    alpha=0.5,
    edgecolor='blue',
    linewidth=2,
    facecolor='none'
)

ctx.add_basemap(ax, source=ctx.providers.OpenStreetMap.Mapnik)

ax.set_title('Regions with Basemap')
ax.set_axis_off()

plt.tight_layout()
plt.savefig('basemap.png', dpi=150)</code></pre>
  </details>

  <h3>Exercise 24: Create an interactive Folium map</h3>
  <p>Create an interactive web map with polygon boundaries and popup information.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">import folium
import geopandas as gpd

regions = gpd.read_file('regions.shp')
regions = regions.to_crs(epsg=4326)

# Create map centered on data
center = [regions.geometry.centroid.y.mean(), regions.geometry.centroid.x.mean()]
m = folium.Map(location=center, zoom_start=6)

# Add GeoJSON layer
folium.GeoJson(
    regions.__geo_interface__,
    style_function=lambda x: {
        'fillColor': '#3186cc',
        'color': 'black',
        'weight': 1,
        'fillOpacity': 0.4
    },
    tooltip=folium.GeoJsonTooltip(
        fields=['name', 'population'],
        aliases=['Region:', 'Population:']
    )
).add_to(m)

m.save('interactive_map.html')</code></pre>
  </details>

  <h3>Exercise 25: Create a multi-panel figure</h3>
  <p>Create a 2x2 figure showing an RGB image, NDVI, elevation, and land cover.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">import matplotlib.pyplot as plt
import rasterio
from rasterio.plot import show
import numpy as np

fig, axes = plt.subplots(2, 2, figsize=(12, 10))

# Panel 1: RGB
with rasterio.open('sentinel2.tif') as src:
    rgb = src.read([4, 3, 2])
    rgb = np.clip(rgb / 3000, 0, 1)  # Normalize
    show(rgb, ax=axes[0, 0], transform=src.transform)
axes[0, 0].set_title('True Color')

# Panel 2: NDVI
with rasterio.open('ndvi.tif') as src:
    show(src, ax=axes[0, 1], cmap='RdYlGn', vmin=-1, vmax=1)
axes[0, 1].set_title('NDVI')

# Panel 3: Elevation
with rasterio.open('dem.tif') as src:
    show(src, ax=axes[1, 0], cmap='terrain')
axes[1, 0].set_title('Elevation')

# Panel 4: Land Cover
with rasterio.open('landcover.tif') as src:
    show(src, ax=axes[1, 1], cmap='tab10')
axes[1, 1].set_title('Land Cover')

plt.tight_layout()
plt.savefig('multi_panel.png', dpi=150)</code></pre>
  </details>

  <h3>Exercise 26: Batch process files in a directory</h3>
  <p>Write a function to calculate NDVI for all Sentinel-2 images in a directory.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">from pathlib import Path
import rasterio
import numpy as np

def batch_calculate_ndvi(input_dir, output_dir):
    """Calculate NDVI for all images in a directory."""
    input_path = Path(input_dir)
    output_path = Path(output_dir)
    output_path.mkdir(exist_ok=True)
    
    processed = []
    for img_file in input_path.glob('*.tif'):
        with rasterio.open(img_file) as src:
            red = src.read(4).astype(np.float32)
            nir = src.read(8).astype(np.float32)
            
            ndvi = (nir - red) / (nir + red + 1e-10)
            
            profile = src.profile.copy()
            profile.update(dtype='float32', count=1)
            
            out_file = output_path / f"{img_file.stem}_ndvi.tif"
            with rasterio.open(out_file, 'w', **profile) as dst:
                dst.write(ndvi, 1)
            
            processed.append(out_file)
            print(f"Processed: {img_file.name}")
    
    return processed

# Usage
results = batch_calculate_ndvi('sentinel2_images/', 'ndvi_output/')</code></pre>
  </details>

  <h3>Exercise 27: Parallel processing with ProcessPoolExecutor</h3>
  <p>Modify the batch processing function to run in parallel using multiple CPU cores.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">from concurrent.futures import ProcessPoolExecutor
from pathlib import Path
import rasterio
import numpy as np

def calculate_ndvi_single(args):
    """Calculate NDVI for a single file."""
    input_file, output_file = args
    
    with rasterio.open(input_file) as src:
        red = src.read(4).astype(np.float32)
        nir = src.read(8).astype(np.float32)
        ndvi = (nir - red) / (nir + red + 1e-10)
        
        profile = src.profile.copy()
        profile.update(dtype='float32', count=1)
        
        with rasterio.open(output_file, 'w', **profile) as dst:
            dst.write(ndvi, 1)
    
    return output_file

def batch_ndvi_parallel(input_dir, output_dir, max_workers=4):
    """Calculate NDVI in parallel."""
    input_path = Path(input_dir)
    output_path = Path(output_dir)
    output_path.mkdir(exist_ok=True)
    
    # Prepare argument pairs
    args_list = [
        (f, output_path / f"{f.stem}_ndvi.tif")
        for f in input_path.glob('*.tif')
    ]
    
    with ProcessPoolExecutor(max_workers=max_workers) as executor:
        results = list(executor.map(calculate_ndvi_single, args_list))
    
    return results</code></pre>
  </details>

  <h3>Exercise 28: Create a command-line tool</h3>
  <p>Create a CLI tool using argparse that clips a raster to a shapefile boundary.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">#!/usr/bin/env python
"""Clip a raster to a shapefile boundary."""

import argparse
import rioxarray
import geopandas as gpd

def main():
    parser = argparse.ArgumentParser(description='Clip raster to boundary')
    parser.add_argument('raster', help='Input raster file')
    parser.add_argument('boundary', help='Boundary shapefile')
    parser.add_argument('output', help='Output raster file')
    parser.add_argument('--all-touched', action='store_true',
                        help='Include all pixels touching boundary')
    
    args = parser.parse_args()
    
    # Load data
    da = rioxarray.open_rasterio(args.raster)
    boundary = gpd.read_file(args.boundary)
    
    # Clip
    clipped = da.rio.clip(
        boundary.geometry,
        boundary.crs,
        all_touched=args.all_touched
    )
    
    # Save
    clipped.rio.to_raster(args.output)
    print(f"Saved clipped raster to {args.output}")

if __name__ == '__main__':
    main()</code></pre>
  </details>

  <h3>Exercise 29: Add logging to a processing script</h3>
  <p>Add comprehensive logging to a batch processing script that logs to both file and console.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">import logging
from pathlib import Path
import rasterio

# Configure logging
logging.basicConfig(
    level=logging.INFO,
    format='%(asctime)s - %(levelname)s - %(message)s',
    handlers=[
        logging.FileHandler('processing.log'),
        logging.StreamHandler()
    ]
)
logger = logging.getLogger(__name__)

def process_with_logging(input_dir, output_dir, process_func):
    """Batch process with logging."""
    input_path = Path(input_dir)
    output_path = Path(output_dir)
    
    files = list(input_path.glob('*.tif'))
    logger.info(f"Found {len(files)} files to process")
    
    success, failed = 0, 0
    
    for f in files:
        logger.info(f"Processing: {f.name}")
        try:
            out_file = output_path / f"{f.stem}_processed.tif"
            process_func(f, out_file)
            logger.info(f"Success: {out_file.name}")
            success += 1
        except Exception as e:
            logger.error(f"Failed: {f.name} - {e}")
            failed += 1
    
    logger.info(f"Complete: {success} success, {failed} failed")
    return success, failed</code></pre>
  </details>

  <h3>Exercise 30: Build a complete analysis pipeline</h3>
  <p>Create a function that takes a Sentinel-2 image and zones shapefile, calculates NDVI, computes zonal statistics, and saves results to CSV.</p>
  
  <details>
    <summary>Show solution</summary>
    <pre><code class="language-python">import rasterio
import geopandas as gpd
from rasterstats import zonal_stats
import numpy as np
import pandas as pd
from pathlib import Path

def ndvi_zonal_analysis(image_path, zones_path, output_csv, output_ndvi=None):
    """
    Complete NDVI zonal analysis pipeline.
    
    Parameters:
        image_path: Path to Sentinel-2 image
        zones_path: Path to zones shapefile
        output_csv: Path for output statistics CSV
        output_ndvi: Optional path to save NDVI raster
    """
    # Step 1: Calculate NDVI
    with rasterio.open(image_path) as src:
        red = src.read(4).astype(np.float32)
        nir = src.read(8).astype(np.float32)
        ndvi = (nir - red) / (nir + red + 1e-10)
        ndvi = np.clip(ndvi, -1, 1)
        
        profile = src.profile.copy()
        profile.update(dtype='float32', count=1)
        
        # Save NDVI if path provided
        if output_ndvi:
            with rasterio.open(output_ndvi, 'w', **profile) as dst:
                dst.write(ndvi, 1)
            ndvi_path = output_ndvi
        else:
            # Use temporary approach: write and read back
            ndvi_path = 'temp_ndvi.tif'
            with rasterio.open(ndvi_path, 'w', **profile) as dst:
                dst.write(ndvi, 1)
    
    # Step 2: Load zones
    zones = gpd.read_file(zones_path)
    
    # Step 3: Calculate zonal statistics
    stats = zonal_stats(
        zones,
        ndvi_path,
        stats=['min', 'max', 'mean', 'std', 'median', 'count']
    )
    
    # Step 4: Build results DataFrame
    results = zones.drop(columns='geometry').copy()
    for stat in ['min', 'max', 'mean', 'std', 'median', 'count']:
        results[f'ndvi_{stat}'] = [s[stat] for s in stats]
    
    # Step 5: Save to CSV
    results.to_csv(output_csv, index=False)
    
    # Clean up temp file if created
    if not output_ndvi:
        Path(ndvi_path).unlink()
    
    return results

# Usage
results = ndvi_zonal_analysis(
    'sentinel2_20200601.tif',
    'agricultural_zones.shp',
    'ndvi_stats.csv',
    output_ndvi='ndvi_20200601.tif'
)
print(results.head())</code></pre>
  </details>
</div>

## Summary

This tutorial covered the essential Python libraries for geospatial analysis:

- **Rasterio** for reading, writing, and manipulating raster data with clean, Pythonic syntax
- **Geopandas** for vector operations combining Pandas functionality with spatial awareness
- **Xarray** for labeled multidimensional arrays, essential for time series and climate data
- **Rioxarray** for bridging xarray's computation power with geospatial metadata
- **Pyproj** for coordinate transformations and CRS management
- **Rasterstats** for extracting statistics from rasters using vector zones
- **Matplotlib, Contextily, and Folium** for static and interactive visualization

The Python geospatial ecosystem enables reproducible, scalable analysis that would be tedious or impossible in traditional GIS interfaces. For Central Asia research—whether analyzing decades of satellite archives, processing imagery across five countries, or automating monitoring systems—these tools provide the foundation for rigorous scientific work.

<div class="info-box tip">
  <strong>Key idea</strong>
  Mastery comes through practice. Work through the exercises, adapt them to your own data, and gradually build complexity. Each project teaches new patterns and reveals new capabilities in these libraries.
</div>
