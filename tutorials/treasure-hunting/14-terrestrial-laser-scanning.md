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

TLS revolutionizes heritage documentation by recording surface geometry with unprecedented precision. A single scan station captures millions of 3D points in minutes. For Central Asia's archaeological landscape, TLS provides the geometric foundation for conservation and research.

<div class="info-box tip">
  <strong>Key idea</strong>
  TLS captures what exists today with millimeter precision. Unlike photographs, point clouds contain true 3D geometry—enabling accurate measurements and virtual sections decades after fieldwork ends.
</div>

<div class="learning-objectives">
  <div class="learning-objectives-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#1e4f8a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <h3>What you will learn</h3>
  </div>
  <ul>
    <li>Compare time-of-flight and phase-shift scanner principles to select the right instrument for archaeological site documentation</li>
    <li>Design TLS survey plans with optimal station placement, scan overlap (30–50%), and resolution settings for Central Asian heritage sites</li>
    <li>Register multiple scans using target-based and cloud-to-cloud methods, achieving sub-centimeter alignment accuracy</li>
    <li>Process point clouds through noise filtering, decimation, meshing, and orthoimage generation for heritage recording</li>
    <li>Integrate TLS data with photogrammetry and GIS to produce comprehensive 3D documentation meeting international heritage standards</li>
  </ul>
</div>

<div class="prerequisites">
  <div class="prerequisites-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#8b5e00" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
    <h3>Prerequisites</h3>
  </div>
  <ul>
    <li>Basic understanding of 3D coordinate systems, transformations, and georeferencing with GNSS</li>
    <li>Familiarity with archaeological site documentation methods (plans, sections, photography)</li>
    <li>Experience with at least one point-cloud viewer or 3D modelling application (e.g., CloudCompare, MeshLab)</li>
  </ul>
</div>

<div class="concept-diagram">
  <svg viewBox="0 0 620 320" xmlns="http://www.w3.org/2000/svg" style="max-width: 580px;">
    <rect x="0" y="0" width="620" height="320" fill="#f9f8f6" rx="8"/>
    <text x="310" y="22" text-anchor="middle" font-family="Inter, sans-serif" font-size="13" font-weight="bold" fill="#111">Terrestrial Laser Scanning Setup</text>

    <!-- Ground -->
    <rect x="20" y="240" width="580" height="4" fill="#8b5e00" rx="1"/>
    <rect x="20" y="244" width="580" height="56" fill="#f0ead6" opacity="0.3" rx="0"/>

    <!-- Scanner on tripod -->
    <!-- Tripod legs -->
    <line x1="120" y1="200" x2="95" y2="240" stroke="#68625b" stroke-width="2"/>
    <line x1="120" y1="200" x2="120" y2="240" stroke="#68625b" stroke-width="2"/>
    <line x1="120" y1="200" x2="145" y2="240" stroke="#68625b" stroke-width="2"/>
    <!-- Scanner body -->
    <rect x="105" y="170" width="30" height="30" rx="4" fill="#1e4f8a" stroke="#111" stroke-width="1"/>
    <circle cx="120" cy="185" r="6" fill="#e8eef5" stroke="#1e4f8a" stroke-width="1"/>
    <circle cx="120" cy="185" r="2" fill="#d92b1f"/>
    <text x="120" y="260" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#111">Scanner</text>

    <!-- Laser beam fan -->
    <line x1="135" y1="180" x2="310" y2="80" stroke="#d92b1f" stroke-width="0.6" opacity="0.5"/>
    <line x1="135" y1="183" x2="340" y2="100" stroke="#d92b1f" stroke-width="0.6" opacity="0.5"/>
    <line x1="135" y1="185" x2="360" y2="130" stroke="#d92b1f" stroke-width="0.8" opacity="0.7"/>
    <line x1="135" y1="187" x2="370" y2="160" stroke="#d92b1f" stroke-width="0.6" opacity="0.5"/>
    <line x1="135" y1="190" x2="380" y2="190" stroke="#d92b1f" stroke-width="0.6" opacity="0.5"/>
    <line x1="135" y1="192" x2="370" y2="220" stroke="#d92b1f" stroke-width="0.6" opacity="0.5"/>
    <line x1="135" y1="194" x2="340" y2="238" stroke="#d92b1f" stroke-width="0.6" opacity="0.5"/>
    <text x="200" y="155" font-family="Inter, sans-serif" font-size="8" fill="#d92b1f" transform="rotate(-25,200,155)">laser pulses</text>

    <!-- Archaeological structure (wall/building) -->
    <!-- Back wall -->
    <rect x="300" y="80" width="120" height="160" fill="#e8dcc8" stroke="#8b5e00" stroke-width="1.5" rx="2"/>
    <!-- Side wall (perspective) -->
    <polygon points="420,80 500,110 500,240 420,240" fill="#d9ccb4" stroke="#8b5e00" stroke-width="1.5"/>
    <!-- Door opening -->
    <rect x="340" y="160" width="40" height="80" fill="#f9f8f6" stroke="#8b5e00" stroke-width="1"/>
    <!-- Window -->
    <rect x="320" y="100" width="25" height="20" fill="#f9f8f6" stroke="#8b5e00" stroke-width="0.8"/>
    <!-- Top edge -->
    <line x1="300" y1="80" x2="420" y2="80" stroke="#8b5e00" stroke-width="1.5"/>
    <text x="380" y="268" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#8b5e00">Archaeological Structure</text>

    <!-- Point cloud dots on structure -->
    <g fill="#1e4f8a" opacity="0.7">
      <circle cx="310" cy="95" r="1.2"/><circle cx="325" cy="92" r="1.2"/><circle cx="350" cy="88" r="1.2"/>
      <circle cx="375" cy="90" r="1.2"/><circle cx="395" cy="93" r="1.2"/><circle cx="410" cy="87" r="1.2"/>
      <circle cx="305" cy="115" r="1.2"/><circle cx="315" cy="110" r="1.2"/><circle cx="390" cy="108" r="1.2"/>
      <circle cx="400" cy="115" r="1.2"/><circle cx="412" cy="105" r="1.2"/>
      <circle cx="308" cy="135" r="1.2"/><circle cx="318" cy="130" r="1.2"/><circle cx="388" cy="128" r="1.2"/>
      <circle cx="398" cy="135" r="1.2"/><circle cx="408" cy="132" r="1.2"/>
      <circle cx="310" cy="150" r="1.2"/><circle cx="395" cy="148" r="1.2"/><circle cx="415" cy="155" r="1.2"/>
      <circle cx="305" cy="170" r="1.2"/><circle cx="315" cy="175" r="1.2"/><circle cx="390" cy="168" r="1.2"/>
      <circle cx="310" cy="195" r="1.2"/><circle cx="395" cy="190" r="1.2"/><circle cx="405" cy="200" r="1.2"/>
      <circle cx="308" cy="215" r="1.2"/><circle cx="400" cy="210" r="1.2"/><circle cx="415" cy="220" r="1.2"/>
      <!-- Side wall points -->
      <circle cx="430" cy="100" r="1.2"/><circle cx="445" cy="118" r="1.2"/><circle cx="460" cy="130" r="1.2"/>
      <circle cx="475" cy="145" r="1.2"/><circle cx="490" cy="158" r="1.2"/>
      <circle cx="435" cy="160" r="1.2"/><circle cx="450" cy="170" r="1.2"/><circle cx="470" cy="185" r="1.2"/>
      <circle cx="485" cy="198" r="1.2"/><circle cx="495" cy="215" r="1.2"/>
      <circle cx="440" cy="200" r="1.2"/><circle cx="460" cy="215" r="1.2"/><circle cx="480" cy="228" r="1.2"/>
    </g>

    <!-- Registration targets -->
    <!-- Target 1 -->
    <g transform="translate(265,200)">
      <rect x="-8" y="-8" width="16" height="16" fill="#fff" stroke="#111" stroke-width="0.8"/>
      <rect x="-8" y="-8" width="8" height="8" fill="#111"/>
      <rect x="0" y="0" width="8" height="8" fill="#111"/>
    </g>
    <text x="265" y="222" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#111">target</text>

    <!-- Target 2 -->
    <g transform="translate(530,170)">
      <rect x="-8" y="-8" width="16" height="16" fill="#fff" stroke="#111" stroke-width="0.8"/>
      <rect x="-8" y="-8" width="8" height="8" fill="#111"/>
      <rect x="0" y="0" width="8" height="8" fill="#111"/>
    </g>
    <text x="530" y="192" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#111">target</text>

    <!-- Target 3 -->
    <g transform="translate(350,48)">
      <rect x="-8" y="-8" width="16" height="16" fill="#fff" stroke="#111" stroke-width="0.8"/>
      <rect x="-8" y="-8" width="8" height="8" fill="#111"/>
      <rect x="0" y="0" width="8" height="8" fill="#111"/>
    </g>
    <text x="350" y="38" text-anchor="middle" font-family="Inter, sans-serif" font-size="7" fill="#111">target</text>

    <!-- Legend -->
    <rect x="20" y="280" width="260" height="30" rx="4" fill="#fff" stroke="#68625b" stroke-width="0.5"/>
    <circle cx="35" cy="295" r="3" fill="#d92b1f"/>
    <text x="44" y="298" font-family="Inter, sans-serif" font-size="8" fill="#111">Laser beams</text>
    <circle cx="115" cy="295" r="3" fill="#1e4f8a"/>
    <text x="124" y="298" font-family="Inter, sans-serif" font-size="8" fill="#111">Point cloud</text>
    <rect x="195" y="291" width="8" height="8" fill="#fff" stroke="#111" stroke-width="0.6"/>
    <rect x="195" y="291" width="4" height="4" fill="#111"/>
    <rect x="199" y="295" width="4" height="4" fill="#111"/>
    <text x="210" y="298" font-family="Inter, sans-serif" font-size="8" fill="#111">Registration target</text>
  </svg>
  <p class="diagram-caption">Figure — TLS survey: scanner on tripod emitting laser pulses, generating a point cloud on the archaeological structure, with checkerboard registration targets for scan alignment.</p>
</div>

## TLS principles: how scanners measure distance

### Time-of-flight scanners

Time-of-flight (TOF) scanners measure distance by timing laser pulse travel. The scanner emits a pulse, records when the reflection returns, and calculates distance using the speed of light: **Distance = (Speed of light × Time) ÷ 2**

TOF scanners excel at long ranges—up to 6 kilometers for some models. They work well for landscape documentation, large monuments, and survey control. However, measurement rates are slower and precision decreases with distance.

| Characteristic | Typical values |
|----------------|----------------|
| Range | 2-6,000 m |
| Precision | ±2-10 mm at 100 m |
| Measurement rate | 50,000-1,000,000 pts/sec |
| Best applications | Large sites, landscapes |

### Phase-shift scanners

Phase-shift scanners measure distance by comparing the phase of emitted and returned continuous laser waves, achieving higher precision and faster rates than TOF.

| Characteristic | Typical values |
|----------------|----------------|
| Range | 0.6-350 m |
| Precision | ±1-3 mm at 50 m |
| Measurement rate | 500,000-2,000,000 pts/sec |

<div class="info-box tip">
  <strong>Key idea</strong>
  Phase-shift for precision and speed at moderate ranges; time-of-flight for long-range coverage. Most archaeological projects use phase-shift scanners for their combination of speed and precision.
</div>

## Survey planning: designing effective coverage

### Station placement strategy

Effective TLS surveys require thoughtful station placement. Each position must capture target surfaces while maintaining sufficient overlap with neighboring scans for registration.

**Key placement principles:**
1. **Line-of-sight coverage**: Every surface must be visible from at least one station
2. **Overlap**: Adjacent scans should share 30-50% coverage
3. **Incidence angle**: Avoid grazing angles below 20°
4. **Range**: Stay within optimal range for required precision

### Scan resolution settings

| Resolution | Point spacing at 10 m | Typical use |
|------------|----------------------|-------------|
| Low | 12-25 mm | Large-scale documentation |
| Medium | 6-12 mm | Architectural recording |
| High | 3-6 mm | Detailed features |
| Ultra-high | 1-3 mm | Fine carvings |

<div class="info-box tip">
  <strong>Key idea</strong>
  Match resolution to requirements. Over-scanning wastes time; under-scanning misses critical details.
</div>

### Control point networks

Establish control using GNSS or total station before scanning.

**Control point requirements:**
- Minimum 4 points visible from multiple stations
- Distribution throughout site
- Stable, well-defined positions (survey markers, architectural corners)
- Documented with photographs and coordinates

### Target types and placement

Registration targets provide identifiable points for aligning multiple scans:

| Target type | Size | Precision | Best use |
|-------------|------|-----------|----------|
| Spheres | 145 mm diameter | ±1-2 mm | Outdoor, multiple angles |
| Checkerboard | 150×150 mm | ±1 mm | Indoor, known positions |
| Natural targets | Variable | ±3-10 mm | When artificial targets prohibited |

Place targets so each scan captures at least 3-4 targets visible from adjacent stations.

## Registration: aligning multiple scans

### Target-based registration

Target-based registration uses artificial targets to compute transformations between scans.

**Workflow:**
1. Detect targets automatically in each scan
2. Match corresponding targets between scan pairs
3. Compute rigid transformation
4. Apply transformation to align scans

Expect residuals of 1-3 mm for well-designed surveys.

### Cloud-to-cloud registration

Cloud-to-cloud (C2C) registration aligns scans by matching overlapping geometry without targets using ICP algorithms.

**Advantages:** No targets required, works with existing geometry
**Limitations:** Requires geometric variation, may fail in symmetric areas

<div class="info-box tip">
  <strong>Key idea</strong>
  Combine target-based and cloud-to-cloud registration. Use targets for initial alignment; refine with ICP for final precision.
</div>

### SLAM-based registration

Simultaneous Localization and Mapping (SLAM) enables registration during data collection using mobile scanners. The system tracks scanner movement while building the 3D model incrementally.

SLAM excels for rapid documentation of complex spaces—narrow corridors, multi-room structures. However, precision typically reaches 10-30 mm, below static TLS standards.

### Registration quality metrics

| Metric | Good value | Concern threshold |
|--------|------------|-------------------|
| Target residuals | <3 mm | >5 mm |
| Cloud-to-cloud RMS | <5 mm | >10 mm |
| Control point residuals | <10 mm | >20 mm |

## Point cloud processing

### Noise removal

Raw point clouds contain noise from atmospheric interference, multi-path reflections, edge effects, and sensor limitations.

**Common noise types:**
- **Outliers**: Isolated points far from surfaces
- **Mixed pixels**: Points at depth discontinuities with averaged positions
- **Multi-path reflections**: Ghost points from indirect laser paths
- **Moving objects**: People, vehicles, vegetation

Apply statistical outlier removal first, then manually inspect and clean remaining noise at edges and reflective surfaces.

### Decimation strategies

Full-resolution datasets may contain billions of points. Decimation reduces point count while preserving important geometry.

| Method | Description | Best use |
|--------|-------------|----------|
| Uniform | Keep every Nth point | Simple reduction |
| Spatial | Grid-based sampling | Uniform density output |
| Curvature-based | Preserve detail in curved areas | Adaptive preservation |

For heritage documentation, curvature-based decimation preserves detail where it matters—carved surfaces, architectural moldings—while reducing points on flat areas.

<div class="info-box tip">
  <strong>Key idea</strong>
  Preserve original data. Always keep full-resolution archives; create decimated versions for specific applications. Storage is cheap; recapturing lost detail is impossible.
</div>

### Point cloud classification

Classification assigns semantic labels to points—ground, vegetation, buildings, artifacts. This enables selective analysis and visualization.

**Archaeological classification categories:**
- Ground surface (natural terrain)
- Architectural elements (walls, floors)
- Excavation surfaces (trench walls, stratigraphy)
- Artifacts and inscriptions
- Modern elements (scaffolding, equipment)
- Vegetation

## Mesh generation and surface reconstruction

### Point cloud to mesh conversion

Meshes convert discrete points into continuous surfaces—enabling visualization, measurement, and 3D printing.

| Algorithm | Characteristics | Best use |
|-----------|----------------|----------|
| Poisson | Watertight, smooth | Complete objects |
| Ball-pivoting | Preserves sharp features | Architectural detail |
| Screened Poisson | Balanced smoothness/detail | General heritage |

### Mesh optimization

Raw meshes require optimization:
- **Hole filling**: Close gaps from scanning shadows
- **Smoothing**: Reduce noise while preserving features
- **Decimation**: Reduce triangle count for performance
- **Texture mapping**: Apply colors from photographs

Balance optimization against detail loss. Archaeological meshes must preserve authentic surface character.

| Application | Triangle count |
|-------------|----------------|
| Web visualization | 50,000-500,000 |
| Desktop viewing | 500,000-5,000,000 |
| 3D printing | 1,000,000-20,000,000 |

## Orthoimages and section generation

### Orthoimage creation

Orthoimages are geometrically corrected images where every pixel represents true position. Unlike photographs, orthoimages have uniform scale—enabling direct measurement.

**TLS orthoimage workflow:**
1. Define projection plane (vertical for elevations, horizontal for plans)
2. Project point cloud onto plane
3. Assign color from scanner imagery
4. Export at appropriate resolution (typically 1-5 mm/pixel)

### Section generation

TLS enables extraction of unlimited sections through point clouds—profiles that would require extensive manual measurement.

**Section types:**
- Vertical sections: Wall profiles, architectural elevations
- Horizontal sections: Floor plans at specified heights
- Serial sections: Multiple parallel slices for analysis

Export sections as 2D CAD files (DXF/DWG) for integration with architectural drawings.

## Integration with photogrammetry

### Complementary strengths

| Aspect | TLS | Photogrammetry |
|--------|-----|----------------|
| Geometry precision | ±1-5 mm | ±5-20 mm |
| Color quality | Moderate | Excellent |
| Dark conditions | Works well | Requires lighting |
| Equipment cost | High | Low-moderate |

### Fusion workflows

1. **TLS for geometry control**: Use scan data to scale photogrammetric models
2. **Photogrammetry for texture**: Apply photographs to TLS meshes
3. **Validation**: Cross-check measurements between methods

<div class="info-box tip">
  <strong>Key idea</strong>
  TLS provides geometric truth; photogrammetry provides visual richness. Combined, they document both measurable form and visual appearance.
</div>

## Heritage documentation standards

### International guidelines

Heritage documentation follows established standards: ICOMOS principles for recording monuments, CIPA heritage documentation protocols, and ISO 19650 for data management.

### Metadata requirements

**Project metadata:** Site identification, dates, personnel, permits
**Technical metadata:** Scanner specs, registration accuracy, coordinate systems
**Contextual metadata:** Archaeological interpretation, conservation condition

### Archive formats

| Data type | Archive format |
|-----------|----------------|
| Point clouds | E57, LAS/LAZ |
| Meshes | OBJ, PLY |
| Images | TIFF, PNG |
| Metadata | XML, JSON |

## Central Asia applications

### Petroglyph documentation

Central Asia's rock art—from Tamgaly in Kazakhstan to Saimaly-Tash in Kyrgyzstan—benefits enormously from TLS:
- Millimeter detail captures subtle pecking patterns
- Weathering analysis quantifies erosion between campaigns
- Virtual access enables study without site visits

Scan petroglyphs at ultra-high resolution (1-2 mm) with multiple angles.

### Architectural documentation

Silk Road monuments present complex challenges:
- Domed structures require multiple interior stations
- Decorated surfaces need high resolution for tile work
- Structural deformation visible in deviation maps

<div class="info-box tip">
  <strong>Key idea</strong>
  Central Asia's mud-brick surfaces absorb laser energy and may provide weak returns. Test scanner settings on representative surfaces; consider supplementary photogrammetry.
</div>

### Excavation documentation

TLS revolutionizes excavation recording:
- Daily surface models document progress
- Section recording captures trench walls before collapse
- Volume calculation quantifies excavated material

Establish permanent control points outside trenches for multi-session consistency.

## Field workflow checklist

<ul class="checklist">
  <li>Verify scanner calibration before deployment</li>
  <li>Charge all batteries (scanner, targets, GNSS)</li>
  <li>Pack targets, tripods, and mounting hardware</li>
  <li>Establish control point network with GNSS</li>
  <li>Plan station positions for complete coverage</li>
  <li>Set appropriate resolution for documentation goals</li>
  <li>Place targets visible from multiple stations</li>
  <li>Document target positions with photographs</li>
  <li>Scan with 30-50% overlap between stations</li>
  <li>Check data quality after each scan</li>
  <li>Register scans in field software for quality check</li>
  <li>Back up data to multiple storage devices</li>
</ul>

## Exercises

### Exercise 1: Scanner selection
A project requires documenting a caravanserai courtyard (40×40 m) with 3 mm precision on architectural details. Which scanner type is most appropriate?

<details>
<summary>Show solution</summary>
A phase-shift scanner is most appropriate. The 40 m courtyard is well within phase-shift range (typically 100+ m), and the 3 mm precision requirement matches phase-shift capabilities. Time-of-flight scanners would work for coverage but typically achieve only 5-10 mm precision at these ranges.
</details>

### Exercise 2: Station count estimation
Estimate the minimum number of scan stations needed to document a rectangular building interior (20×10 m) with four rooms and a central corridor.

<details>
<summary>Show solution</summary>
Minimum 8-12 stations: at least one station per room (4) plus corridor positions (2-3) plus additional stations for corners and doorways (2-5). Each room needs complete wall coverage with appropriate incidence angles. Plan for 30-50% overlap between adjacent stations.
</details>

### Exercise 3: Resolution calculation
At 6 mm resolution setting, approximately how many points will be captured on a 10×10 m wall face at 15 m distance?

<details>
<summary>Show solution</summary>
Approximately 1-2 million points. At 6 mm resolution at 15 m range, point spacing scales to approximately 9 mm. Points per m² ≈ (1000/9)² ≈ 12,346. Total ≈ 100 × 12,346 = 1.2 million. Actual count varies with incidence angle.
</details>

### Exercise 4: Target placement
A site requires scanning a cylindrical minaret (8 m diameter, 25 m height). Design a target placement strategy.

<details>
<summary>Show solution</summary>
Place targets at three height levels (ground, ~10 m, ~20 m) distributed around the circumference. At each level, position 4 targets at 90° intervals. Total: 12 targets minimum. Use spherical targets for visibility from multiple angles. Add ground-level targets linking to site control.
</details>

### Exercise 5: Overlap verification
Two scans show only 15% overlap in preliminary registration. What actions should be taken?

<details>
<summary>Show solution</summary>
15% overlap is insufficient for reliable registration. Review station placement—may need intermediate station. Check target visibility in both scans. Attempt cloud-to-cloud registration if geometry is distinctive. If possible, return to field and add bridging scan. Minimum recommended overlap is 30%.
</details>

### Exercise 6: Registration residuals
Target-based registration shows residuals of 2, 3, 15, 2, 4 mm for five targets. Interpret these results.

<details>
<summary>Show solution</summary>
The 15 mm residual indicates a problem—likely movement, misidentification, or damage. Remove this target and reprocess. Remaining targets (2-4 mm) show acceptable residuals. Investigate the problematic target to prevent similar issues.
</details>

### Exercise 7: Processing order
Arrange these steps in correct order: mesh generation, noise removal, georeferencing, registration, decimation, classification.

<details>
<summary>Show solution</summary>
Correct order: (1) Registration—align scans; (2) Georeferencing—transform to project coordinates; (3) Noise removal—clean erroneous points; (4) Classification—assign labels; (5) Decimation—reduce for applications; (6) Mesh generation—create surfaces.
</details>

### Exercise 8: Decimation ratio
A 500 million point cloud must be reduced to 50 million points. Calculate the decimation ratio and new point spacing if original was 5 mm.

<details>
<summary>Show solution</summary>
Decimation ratio = 10:1. For spatial decimation, spacing increases by √10 ≈ 3.16×. Original 5 mm becomes approximately 15-16 mm. This remains adequate for web visualization.
</details>

### Exercise 9: Classification categories
Define appropriate point cloud classification categories for an excavation trench with architecture and stratigraphy.

<details>
<summary>Show solution</summary>
Categories: Modern surface, trench walls (stratigraphy), excavated floor, architecture, finds in situ, natural features, modern intrusions (equipment, sandbags), section markers. Each enables selective visualization.
</details>

### Exercise 10: Coordinate transformation
A point has local scanner coordinates (10.000, 5.000, 2.500). Scanner position in UTM is (500000.000, 4500000.000, 850.000) with 45° orientation from north. Calculate UTM coordinates.

<details>
<summary>Show solution</summary>
Apply rotation then translation. E = 500000 + 10×cos(45°) - 5×sin(45°) = 500003.54 m. N = 4500000 + 10×sin(45°) + 5×cos(45°) = 4500010.61 m. Z = 850 + 2.5 = 852.5 m.
</details>

### Exercise 11: Mesh triangle count
A heritage mesh must fit within 100 MB file size. If each triangle requires 50 bytes, how many triangles can the mesh contain?

<details>
<summary>Show solution</summary>
100 MB = 104,857,600 bytes. Maximum triangles = 104,857,600 / 50 ≈ 2.1 million. Reserve 20% for headers: usable triangles ≈ 1.7 million.
</details>

### Exercise 12: Orthoimage resolution
An orthoimage will be printed at 1:50 scale with 300 DPI resolution. What point cloud density is required?

<details>
<summary>Show solution</summary>
Pixel on paper = 25.4/300 = 0.085 mm. Real-world pixel = 0.085 × 50 = 4.2 mm. Need 2-3 mm point spacing for adequate coverage. This requires high-resolution scanning.
</details>

### Exercise 13: Section extraction
An excavation trench is 5 m long. How many serial sections at 10 cm intervals, and total drawing length at 2 m depth?

<details>
<summary>Show solution</summary>
51 sections (0 to 5 m at 10 cm intervals). Total length = 51 × 2 m = 102 m of section drawings—demonstrating TLS efficiency versus manual methods.
</details>

### Exercise 14: Photogrammetry integration
TLS data shows systematic 15 mm offset from photogrammetric model. Diagnose possible causes and solutions.

<details>
<summary>Show solution</summary>
Possible causes: Different control datums, scaling error in photogrammetry, registration error, timing difference. Solutions: Re-process photogrammetry using TLS control points, apply rigid transformation, use TLS as geometric reference.
</details>

### Exercise 15: Scanner range limitation
Scanner achieves 3 mm precision within 50 m; maximum range 130 m. How does this affect survey design for a 200 m long site?

<details>
<summary>Show solution</summary>
Multiple overlapping stations needed to keep targets within 50 m. Minimum 4 stations linearly, but overlap requirements likely need 6-8 stations. Position critical features within precision zone; use outer range for context.
</details>

### Exercise 16: Data volume estimation
10-station survey, 30 million points per scan, 17 bytes per point (XYZ + RGB + intensity). Calculate total raw data volume.

<details>
<summary>Show solution</summary>
Per scan: 30M × 17 = 510 MB. Total: 5.1 GB. With scanner imagery (~210 MB per station), add 2.1 GB. Total project: approximately 7.2 GB.
</details>

### Exercise 17: Edge effect identification
A stone inscription shows anomalous points extending 10-15 mm beyond carved surface edges. Explain and address.

<details>
<summary>Show solution</summary>
Mixed pixel effect—laser footprint spans depth discontinuity, returning averaged distance. Address by scanning from multiple angles, applying edge-aware filtering, manual cleaning, or closer range scanning.
</details>

### Exercise 18: Incidence angle effects
At what incidence angle does laser footprint elongation exceed 3× the perpendicular size?

<details>
<summary>Show solution</summary>
Elongation = 1/cos(angle). For 3×: cos(angle) = 1/3, angle ≈ 70.5° from perpendicular (19.5° grazing). Plan stations to keep surfaces within 60° incidence.
</details>

### Exercise 19: Control point accuracy
GNSS control points have ±20 mm accuracy. What is maximum achievable absolute accuracy?

<details>
<summary>Show solution</summary>
Absolute accuracy limited to ±20-25 mm (control accuracy plus measurement errors). Relative accuracy within model remains at scanner precision (±2-5 mm).
</details>

### Exercise 20: Multi-session registration
A monument requires three campaigns over two years. Design registration strategy.

<details>
<summary>Show solution</summary>
Establish permanent control monuments. Document with GNSS. Include in every session. Register each campaign internally, then to control. Use unchanged features as check points. Expect 10-20 mm consistency.
</details>

### Exercise 21: Weathering quantification
Two TLS surveys 5 years apart. How to quantify surface erosion?

<details>
<summary>Show solution</summary>
Register both to common reference using stable areas. Compute cloud-to-cloud distances. Map as colorized point cloud. Distinguish erosion patterns from random registration error. Detection threshold: 2-3× registration accuracy.
</details>

### Exercise 22: Dome scanning strategy
A 15 m diameter dome requires interior documentation. Design scanning strategy.

<details>
<summary>Show solution</summary>
Center position captures apex with near-perpendicular angles. Ring of 6-8 positions near walls captures drum and lower dome. Additional positions for corners/niches. Use leveled scanner for consistent vertical reference.
</details>

### Exercise 23: Mud-brick challenges
A mud-brick fortress shows poor scan quality. Diagnose causes and solutions.

<details>
<summary>Show solution</summary>
Mud-brick absorbs laser energy; porous surface scatters returns. Solutions: Scan closer, increase power/integration time, scan in diffuse lighting, tune filtering for weak returns, supplement with photogrammetry.
</details>

### Exercise 24: Dynamic scene handling
Tourists occasionally walk through scans. How should these be handled?

<details>
<summary>Show solution</summary>
Moving objects appear as scattered noise—remove with statistical filtering. Overlapping scans provide clean data. Manually delete persistent artifacts. Complete removal requires at least one clean scan of each surface.
</details>

### Exercise 25: Archive format selection
A project must archive 50 GB for 50-year preservation. Evaluate format options.

<details>
<summary>Show solution</summary>
E57 format—open standard, widely supported. Store point clouds in E57, meshes in PLY, images in TIFF, metadata in XML/JSON. Plan format migration every 10-15 years.
</details>

### Exercise 26: Quality control workflow
Design systematic QC checklist for TLS processing.

<details>
<summary>Show solution</summary>
Verify all stations captured; check noise levels; review registration residuals (<3 mm targets, <5 mm C2C); verify control points; inspect gaps and artifacts; validate mesh quality; verify metadata completeness.
</details>

### Exercise 27: Cost-benefit analysis
Compare TLS versus total station for 500 m² excavation with complex stratigraphy.

<details>
<summary>Show solution</summary>
Total station: 2-3 days, 2,000-5,000 points, manual drawings. TLS: 0.5-1 day, 100+ million points, orthoimages, sections, 3D visualization. TLS captures more data faster. Combine both for optimal results.
</details>

### Exercise 28: Report requirements
List essential TLS technical report components.

<details>
<summary>Show solution</summary>
Executive summary, project background, site description, equipment specs, survey methodology, control network, registration accuracy, processing workflow, products delivered, quality metrics, recommendations.
</details>

### Exercise 29: Disaster documentation
An earthquake damages a historic monument. Design rapid TLS documentation protocol.

<details>
<summary>Show solution</summary>
Safety assessment first. Minimal GNSS control (4-6 points). Perimeter scan from safe distance. Prioritize collapsed areas. Medium resolution. Field registration check. Multiple backups. Target: 1-2 days before further damage.
</details>

### Exercise 30: Project workflow design
Design complete TLS workflow for a medieval caravanserai (60×80 m) with courtyard, rooms, and decorative portal.

<details>
<summary>Show solution</summary>
Field (3-4 days): Control network (12-15 points), exterior (15 stations), courtyard (8 stations), rooms (20-25 stations), portal detail (5 ultra-high resolution). Processing (5-7 days): Registration, noise removal, mesh generation, orthoimages, sections. Deliverables: E57 cloud, textured mesh, orthoimages, CAD sections, technical report.
</details>
