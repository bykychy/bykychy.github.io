---
layout: tutorial
title: "How Terrestrial Laser Scanning Documents Archaeological Sites in Central Asia"
description: "A practical tutorial on TLS survey design, point cloud processing, mesh generation, and heritage documentation for Central Asia sites, with 30 exercises and solutions."
difficulty: intermediate
estimated_time: "3 hours"
series: treasure-hunting
order: 14
---

## Why this tutorial matters

Archaeological sites vanish. Erosion degrades mud-brick walls in Uzbekistan. Looters damage petroglyphs in Kazakhstan. Development threatens burial mounds across the steppe. Terrestrial Laser Scanning (TLS) captures millimeter-resolution 3D documentation before destruction occurs, preserving detailed geometric records for future generations.

<div class="info-box tip">
  <strong>Key idea</strong>
  TLS captures what exists today with millimeter precision. Unlike photographs, point clouds contain true 3D geometry—enabling accurate measurements and virtual sections decades after fieldwork ends.
</div>

## TLS principles: how scanners measure distance

### Time-of-flight scanners

Time-of-flight (TOF) scanners measure distance by timing laser pulse travel. The scanner emits a pulse, records when the reflection returns, and calculates distance using: **Distance = (Speed of light × Time) ÷ 2**

TOF scanners excel at long ranges—up to 6 kilometers. They work well for landscapes and large monuments but offer slower measurement rates and lower precision at distance.

| Characteristic | Typical values |
|----------------|----------------|
| Range | 2-6,000 m |
| Precision | ±2-10 mm at 100 m |
| Measurement rate | 50,000-1,000,000 pts/sec |

### Phase-shift scanners

Phase-shift scanners measure distance by comparing the phase of emitted and returned continuous laser waves. They achieve higher precision and faster rates than TOF—ideal for architectural and artifact documentation.

| Characteristic | Typical values |
|----------------|----------------|
| Range | 0.6-350 m |
| Precision | ±1-3 mm at 50 m |
| Measurement rate | 500,000-2,000,000 pts/sec |

<div class="info-box tip">
  <strong>Key idea</strong>
  Phase-shift for precision at moderate ranges; time-of-flight for long-range coverage. Most archaeological projects use phase-shift scanners for their combination of speed and precision.
</div>

## Survey planning: designing effective coverage

### Station placement strategy

Effective TLS surveys require thoughtful station placement. Each position must capture target surfaces while maintaining sufficient overlap with neighboring scans.

**Key placement principles:**
- **Line-of-sight coverage**: Every surface must be visible from at least one station
- **Overlap**: Adjacent scans should share 30-50% coverage
- **Incidence angle**: Avoid grazing angles below 20°
- **Range**: Stay within optimal range for required precision

### Resolution settings

| Resolution | Point spacing at 10 m | Typical use |
|------------|----------------------|-------------|
| Low | 12-25 mm | Large-scale documentation |
| Medium | 6-12 mm | Architectural recording |
| High | 3-6 mm | Detailed features |
| Ultra-high | 1-3 mm | Fine carvings, inscriptions |

### Control points and targets

Establish control using GNSS or total station. Place minimum 4 points visible from multiple stations, distributed throughout the site. Registration targets (spheres, checkerboards) enable scan alignment—place so each scan captures 3-4 targets visible from adjacent stations.

## Registration: aligning multiple scans

### Target-based registration

Target-based registration uses artificial targets with known geometry. The process identifies corresponding targets across scans and calculates optimal alignment. Expect residuals of 1-3 mm for well-designed surveys.

### Cloud-to-cloud registration

Cloud-to-cloud (C2C) registration aligns scans by matching overlapping geometry without targets. Iterative Closest Point (ICP) algorithms find optimal alignment by minimizing point distances.

<div class="info-box tip">
  <strong>Key idea</strong>
  Combine target-based and cloud-to-cloud registration. Use targets for initial alignment; refine with ICP for final millimeter precision.
</div>

### SLAM-based registration

SLAM (Simultaneous Localization and Mapping) enables registration during data collection with mobile scanners. Precision reaches 10-30 mm—useful for rapid documentation where centimeter accuracy suffices.

## Point cloud processing

### Noise removal

Raw point clouds contain noise from atmospheric interference, multi-path reflections, and edge effects. Apply statistical outlier removal, then manually inspect edges and reflective surfaces.

### Decimation strategies

| Method | Description |
|--------|-------------|
| Uniform | Keep every Nth point |
| Spatial | Grid-based sampling |
| Curvature-based | Preserve detail in curved areas |

<div class="info-box tip">
  <strong>Key idea</strong>
  Preserve original data. Always keep full-resolution archives; create decimated versions for specific applications.
</div>

### Classification

Assign semantic labels to points—ground, architecture, artifacts, vegetation. This enables selective analysis and visualization. Manual refinement remains essential for archaeological applications.

## Mesh generation and surface reconstruction

### Point cloud to mesh conversion

Meshes convert discrete points into continuous surfaces. Common algorithms include Poisson (smooth, watertight), ball-pivoting (preserves edges), and Screened Poisson (balanced).

### Optimization

Raw meshes require: hole filling, smoothing, decimation, texture mapping. Balance optimization against detail loss—don't over-smooth weathered stonework.

| Application | Triangle count |
|-------------|----------------|
| Web visualization | 50,000-500,000 |
| Desktop viewing | 500,000-5,000,000 |
| 3D printing | 1,000,000-20,000,000 |

## Orthoimages and section generation

### Orthoimage creation

Orthoimages are geometrically corrected images with uniform scale. Project point clouds onto defined planes, assign colors, and export at 1-5 mm/pixel resolution for archaeological documentation.

### Section extraction

TLS enables unlimited section extraction—vertical profiles, horizontal plans, arbitrary cuts. Export as CAD files (DXF/DWG) or georeferenced rasters.

## Integration with photogrammetry

| Aspect | TLS | Photogrammetry |
|--------|-----|----------------|
| Geometry precision | ±1-5 mm | ±5-20 mm |
| Color quality | Moderate | Excellent |
| Dark conditions | Works well | Requires lighting |

<div class="info-box tip">
  <strong>Key idea</strong>
  TLS provides geometric truth; photogrammetry provides visual richness. Combined, they document both measurable form and visual appearance.
</div>

## Heritage documentation standards

Follow ICOMOS and CIPA guidelines. Document comprehensive metadata: project information, scanner specifications, registration accuracy, coordinate systems. Archive in open formats—E57 for point clouds, OBJ/PLY for meshes, TIFF for images.

## Central Asia applications

### Petroglyphs

Central Asia's rock art benefits from TLS millimeter detail. Capture subtle pecking patterns, quantify erosion between surveys, enable study without site visits.

### Architecture

Silk Road monuments—caravanserais, madrasas, mausoleums—require multiple interior stations for domed structures. Mud-brick surfaces may absorb laser energy; test settings on representative materials.

### Excavations

Document daily excavation progress, capture trench sections, record complex features in situ. Establish permanent control outside trenches.

## Field workflow checklist

<ul class="checklist">
  <li>Verify scanner calibration before deployment</li>
  <li>Charge all batteries (scanner, targets, GNSS)</li>
  <li>Establish control point network</li>
  <li>Plan station positions for complete coverage</li>
  <li>Set appropriate resolution for documentation goals</li>
  <li>Place targets visible from multiple stations</li>
  <li>Scan with 30-50% overlap between stations</li>
  <li>Check data quality after each scan</li>
  <li>Register scans in field software for quality check</li>
  <li>Back up data to multiple storage devices</li>
</ul>

## Exercises

### Exercise 1: Scanner selection
A project requires documenting a caravanserai courtyard (40×40 m) with 3 mm precision. Which scanner type is most appropriate?

<details>
<summary>Show solution</summary>
Phase-shift scanner. The 40 m courtyard is within phase-shift range, and 3 mm precision matches phase-shift capabilities. TOF would achieve only 5-10 mm at these ranges.
</details>

### Exercise 2: Station count
Estimate minimum stations for a rectangular building interior (20×10 m) with four rooms and central corridor.

<details>
<summary>Show solution</summary>
8-12 stations: one per room (4), corridor positions (2-3), plus corners/doorways (2-5). Each room needs complete wall coverage with appropriate incidence angles.
</details>

### Exercise 3: Resolution calculation
At 6 mm resolution, approximately how many points on a 10×10 m wall at 15 m distance?

<details>
<summary>Show solution</summary>
Approximately 1-2 million points. Resolution scales with distance (6 mm becomes ~9 mm at 15 m). Points per m² ≈ 12,000. Total ≈ 1.2 million, varying with incidence angle.
</details>

### Exercise 4: Target placement
Design target strategy for a cylindrical minaret (8 m diameter, 25 m height).

<details>
<summary>Show solution</summary>
Three height levels with 4 targets at 90° intervals each = 12 minimum. Use spheres for visibility from multiple angles. Add ground targets linking to site control.
</details>

### Exercise 5: Overlap problem
Two scans show only 15% overlap. What actions should be taken?

<details>
<summary>Show solution</summary>
15% is insufficient. Add intermediate station if possible. Check target visibility. Attempt C2C if geometry is distinctive. Minimum recommended: 30%.
</details>

### Exercise 6: Residual analysis
Residuals of 2, 3, 15, 2, 4 mm for five targets. Interpret these results.

<details>
<summary>Show solution</summary>
The 15 mm outlier indicates problem—movement, misidentification, or damage. Remove and reprocess. Remaining 2-4 mm residuals are acceptable.
</details>

### Exercise 7: Processing order
Order these steps: mesh generation, noise removal, georeferencing, registration, decimation, classification.

<details>
<summary>Show solution</summary>
Registration → Georeferencing → Noise removal → Classification → Decimation → Mesh generation. This ensures cleaning happens in unified coordinate space.
</details>

### Exercise 8: Decimation ratio
500M points reduced to 50M. Original spacing 5 mm. Calculate new spacing.

<details>
<summary>Show solution</summary>
10:1 ratio. Spacing increases by √10 ≈ 3.16× for 2D spatial decimation. New spacing ≈ 15-16 mm.
</details>

### Exercise 9: Classification categories
Define categories for an excavation trench with architecture and stratigraphy.

<details>
<summary>Show solution</summary>
Modern surface, trench walls (stratigraphy), excavated floor, architecture, finds in situ, natural features, modern intrusions, section markers.
</details>

### Exercise 10: Coordinate transformation
Local point (10, 5, 2.5), scanner at UTM (500000, 4500000, 850), oriented 45° from north. Calculate UTM coordinates.

<details>
<summary>Show solution</summary>
Rotate then translate. E = 500000 + 10×cos45° - 5×sin45° = 500003.54 m. N = 4500000 + 10×sin45° + 5×cos45° = 4500010.61 m. Z = 852.5 m.
</details>

### Exercise 11: Mesh file size
100 MB limit, 50 bytes per triangle. Maximum triangle count?

<details>
<summary>Show solution</summary>
104,857,600 / 50 ≈ 2.1 million triangles. Reserve 20% for headers: ~1.7 million usable triangles.
</details>

### Exercise 12: Orthoimage resolution
Print at 1:50, 300 DPI. Required point density?

<details>
<summary>Show solution</summary>
Pixel = 25.4/300 mm × 50 = 4.2 mm real-world. Need 2-3 mm point spacing for adequate coverage.
</details>

### Exercise 13: Serial sections
5 m trench, 10 cm intervals, 2 m depth. Total section length?

<details>
<summary>Show solution</summary>
51 sections × 2 m = 102 m of section drawings—demonstrating TLS efficiency.
</details>

### Exercise 14: Integration offset
TLS and photogrammetry show 15 mm systematic offset. Diagnose causes.

<details>
<summary>Show solution</summary>
Different datums, scaling error, registration error, or timing difference. Re-process photogrammetry using TLS control.
</details>

### Exercise 15: Range limitation
Scanner precise to 3 mm within 50 m; maximum range 130 m. How many stations for 200 m site?

<details>
<summary>Show solution</summary>
Minimum 4 stations linearly, but overlap needs likely require 6-8. Position critical features within 50 m precision zone.
</details>

### Exercise 16: Data volume
10 stations, 30M points each, 17 bytes per point. Total volume?

<details>
<summary>Show solution</summary>
30M × 17 = 510 MB per scan. 10 scans = 5.1 GB. Add imagery (~2 GB) = 7+ GB total.
</details>

### Exercise 17: Edge effects
Inscription shows points extending 10-15 mm beyond carved edges. Explain.

<details>
<summary>Show solution</summary>
Mixed pixel effect—laser spans depth discontinuity. Scan from multiple angles, apply edge filtering, or scan closer.
</details>

### Exercise 18: Incidence angle
At what angle does footprint elongate 3×?

<details>
<summary>Show solution</summary>
cos(angle) = 1/3, angle ≈ 70.5° from perpendicular (19.5° grazing). Keep surfaces within 60° incidence.
</details>

### Exercise 19: Control accuracy
GNSS control ±20 mm. Maximum achievable absolute accuracy?

<details>
<summary>Show solution</summary>
±20-25 mm absolute. Relative accuracy within model remains at scanner precision (±2-5 mm).
</details>

### Exercise 20: Multi-campaign registration
Three campaigns over two years. Design registration strategy.

<details>
<summary>Show solution</summary>
Establish permanent control markers. Include in every session. Register each campaign internally, then to control. Use unchanged features as check points.
</details>

### Exercise 21: Erosion quantification
Two surveys 5 years apart. How to quantify erosion?

<details>
<summary>Show solution</summary>
Register to common reference using stable areas. Compute C2C distances. Map as colorized cloud. Distinguish erosion patterns from random registration error.
</details>

### Exercise 22: Dome scanning
15 m diameter dome interior. Design strategy.

<details>
<summary>Show solution</summary>
Center position for apex. Ring of 6-8 positions near walls for drum and lower dome. Additional positions for niches/corners.
</details>

### Exercise 23: Mud-brick challenges
Poor scan quality on mud-brick. Solutions?

<details>
<summary>Show solution</summary>
Scan closer, increase power/integration time, scan in diffuse light, tune filtering for weak returns, supplement with photogrammetry.
</details>

### Exercise 24: Dynamic objects
Tourists walk through scans. How to handle?

<details>
<summary>Show solution</summary>
Moving objects appear as scattered noise. Remove with statistical filtering. Overlapping scans provide clean coverage. Manually delete persistent artifacts.
</details>

### Exercise 25: Archive formats
50 GB, 50-year preservation. Format recommendations?

<details>
<summary>Show solution</summary>
E57 for point clouds (open standard), PLY for meshes, TIFF for images, XML/JSON for metadata. Plan migration every 10-15 years.
</details>

### Exercise 26: Quality control
Design QC checklist for TLS processing.

<details>
<summary>Show solution</summary>
Verify stations captured, check noise levels, review registration residuals, validate control points, inspect gaps, verify mesh quality, check metadata completeness.
</details>

### Exercise 27: Cost-benefit
TLS vs total station for 500 m² excavation?

<details>
<summary>Show solution</summary>
Total station: 2-3 days, 2,000-5,000 points. TLS: 0.5-1 day, 100M+ points, unlimited products. TLS captures more data faster; total station useful for attributed control points.
</details>

### Exercise 28: Report components
Essential TLS technical report components?

<details>
<summary>Show solution</summary>
Summary, objectives, site description, equipment specs, methodology, control network, registration accuracy, processing workflow, products, quality metrics, recommendations.
</details>

### Exercise 29: Emergency documentation
Earthquake damages monument. Design rapid protocol.

<details>
<summary>Show solution</summary>
Safety first, minimal GNSS control, perimeter scan from safe distance, prioritize collapsed areas, medium resolution, field registration check, multiple backups. Target: 1-2 days.
</details>

### Exercise 30: Complete workflow
Design workflow for 60×80 m caravanserai with courtyard, rooms, portal.

<details>
<summary>Show solution</summary>
Field (3-4 days): control network, exterior (15 stations), courtyard (8), rooms (25), portal detail (5 ultra-high). Processing (5-7 days): registration, cleaning, mesh generation, orthoimages, sections. Deliverables: E57 cloud, textured mesh, orthoimages, CAD sections, technical report.
</details>
