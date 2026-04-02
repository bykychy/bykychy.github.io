---
layout: tutorial
title: "How Proper Documentation Preserves Survey Value in Central Asia"
description: "A practical tutorial on field notes, metadata standards, data management, and professional reporting for geophysical and remote sensing surveys, with 30 exercises and solutions."
difficulty: beginner
estimated_time: "2 hours"
series: treasure-hunting
order: 11
---

## Why this tutorial matters

Data without documentation loses its value. A magnetometry dataset without coordinates is meaningless. GPR profiles without depth calibration cannot be interpreted. XRF readings without sample provenance are scientifically worthless. In Central Asia's challenging field conditions—remote sites, limited infrastructure, harsh weather—the discipline of systematic documentation separates professional surveys from amateur excursions.

This tutorial addresses the often-neglected foundation of all survey work: recording what you did, how you did it, and why. Whether you are conducting electromagnetic surveys in the Fergana Valley, collecting soil samples near Samarkand, or running GPR transects across Kazakh steppe sites, proper documentation ensures your data remains usable for decades.

<div class="info-box tip">
  <strong>Key idea</strong>
  The most sophisticated instrument produces worthless data if you cannot prove when, where, and how measurements were taken. Documentation is not bureaucracy—it is the bridge between raw readings and scientific knowledge.
</div>

<div class="learning-objectives">
  <div class="learning-objectives-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#1e4f8a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <h3>What you will learn</h3>
  </div>
  <ul>
    <li>Maintain systematic field notebooks with waterproof media, permanent ink, and legally defensible recording practices</li>
    <li>Apply metadata standards (who, what, when, where, how, why) to every geophysical and remote-sensing dataset</li>
    <li>Implement consistent file-naming conventions and GPS waypoint/track logging for Central Asian survey projects</li>
    <li>Structure professional survey reports including executive summaries, methodology sections, and appendices</li>
    <li>Archive and share datasets using open formats, Creative Commons licensing, and long-term preservation strategies</li>
  </ul>
</div>

<div class="prerequisites">
  <div class="prerequisites-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#8b5e00" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
    <h3>Prerequisites</h3>
  </div>
  <ul>
    <li>Basic familiarity with at least one geophysical survey method (e.g., magnetometry, GPR, or EM)</li>
    <li>Ability to read and record GPS coordinates in latitude/longitude and UTM formats</li>
    <li>Access to a text editor or spreadsheet for creating metadata records</li>
  </ul>
</div>

<div class="concept-diagram">
  <svg viewBox="0 0 620 320" xmlns="http://www.w3.org/2000/svg" style="max-width: 580px;">
    <!-- Background -->
    <rect x="0" y="0" width="620" height="320" fill="#f9f8f6" rx="8"/>
    <text x="310" y="28" text-anchor="middle" font-family="Inter, sans-serif" font-size="13" font-weight="bold" fill="#111">Field Documentation Workflow</text>

    <!-- Step 1: Field Notebook -->
    <rect x="20" y="55" width="100" height="80" rx="6" fill="#fff" stroke="#1e4f8a" stroke-width="1.5"/>
    <rect x="35" y="65" width="70" height="50" rx="3" fill="#e8eef5" stroke="#1e4f8a" stroke-width="0.8"/>
    <line x1="42" y1="76" x2="98" y2="76" stroke="#1e4f8a" stroke-width="0.6"/>
    <line x1="42" y1="84" x2="98" y2="84" stroke="#1e4f8a" stroke-width="0.6"/>
    <line x1="42" y1="92" x2="85" y2="92" stroke="#1e4f8a" stroke-width="0.6"/>
    <line x1="42" y1="100" x2="95" y2="100" stroke="#1e4f8a" stroke-width="0.6"/>
    <text x="70" y="148" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">Field Notebook</text>
    <text x="70" y="160" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">Waterproof, ink</text>

    <!-- Arrow 1 -->
    <line x1="125" y1="95" x2="148" y2="95" stroke="#68625b" stroke-width="1.5" marker-end="url(#arrowGray11)"/>

    <!-- Step 2: Digital Entry -->
    <rect x="152" y="55" width="100" height="80" rx="6" fill="#fff" stroke="#165d34" stroke-width="1.5"/>
    <rect x="167" y="67" width="70" height="45" rx="2" fill="#eaf5ec" stroke="#165d34" stroke-width="0.8"/>
    <text x="202" y="82" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#165d34">CSV / XLSX</text>
    <rect x="175" y="88" width="54" height="6" rx="1" fill="#165d34" opacity="0.2"/>
    <rect x="175" y="97" width="42" height="6" rx="1" fill="#165d34" opacity="0.2"/>
    <text x="202" y="148" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">Digital Entry</text>
    <text x="202" y="160" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">Same-day transfer</text>

    <!-- Arrow 2 -->
    <line x1="257" y1="95" x2="280" y2="95" stroke="#68625b" stroke-width="1.5" marker-end="url(#arrowGray11)"/>

    <!-- Step 3: Metadata Standard -->
    <rect x="284" y="55" width="100" height="80" rx="6" fill="#fff" stroke="#8b5e00" stroke-width="1.5"/>
    <text x="334" y="78" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#8b5e00">WHO</text>
    <text x="334" y="90" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#8b5e00">WHAT · WHEN</text>
    <text x="334" y="102" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#8b5e00">WHERE · HOW</text>
    <text x="334" y="114" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" font-weight="bold" fill="#8b5e00">WHY</text>
    <text x="334" y="148" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">Metadata Standard</text>
    <text x="334" y="160" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">Six essential Qs</text>

    <!-- Arrow 3 -->
    <line x1="389" y1="95" x2="412" y2="95" stroke="#68625b" stroke-width="1.5" marker-end="url(#arrowGray11)"/>

    <!-- Step 4: GIS Database -->
    <rect x="416" y="55" width="100" height="80" rx="6" fill="#fff" stroke="#d92b1f" stroke-width="1.5"/>
    <circle cx="466" cy="82" r="16" fill="#fdeaea" stroke="#d92b1f" stroke-width="0.8"/>
    <circle cx="466" cy="82" r="8" fill="none" stroke="#d92b1f" stroke-width="0.6"/>
    <line x1="458" y1="82" x2="474" y2="82" stroke="#d92b1f" stroke-width="0.6"/>
    <line x1="466" y1="74" x2="466" y2="90" stroke="#d92b1f" stroke-width="0.6"/>
    <text x="466" y="115" text-anchor="middle" font-family="Inter, sans-serif" font-size="8" fill="#d92b1f">Georeferenced</text>
    <text x="466" y="148" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">GIS Database</text>
    <text x="466" y="160" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">Spatial archive</text>

    <!-- Arrow 4 -->
    <line x1="521" y1="95" x2="544" y2="95" stroke="#68625b" stroke-width="1.5" marker-end="url(#arrowGray11)"/>

    <!-- Step 5: Report -->
    <rect x="548" y="55" width="56" height="80" rx="6" fill="#fff" stroke="#111" stroke-width="1.5"/>
    <rect x="556" y="64" width="40" height="52" rx="2" fill="#f2f2f2" stroke="#111" stroke-width="0.6"/>
    <line x1="561" y1="74" x2="591" y2="74" stroke="#111" stroke-width="0.5"/>
    <line x1="561" y1="82" x2="591" y2="82" stroke="#111" stroke-width="0.5"/>
    <line x1="561" y1="90" x2="586" y2="90" stroke="#111" stroke-width="0.5"/>
    <line x1="561" y1="98" x2="589" y2="98" stroke="#111" stroke-width="0.5"/>
    <line x1="561" y1="106" x2="580" y2="106" stroke="#111" stroke-width="0.5"/>
    <text x="576" y="148" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">Report</text>
    <text x="576" y="160" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#68625b">Final output</text>

    <!-- Quality Control feedback loop -->
    <path d="M 310 170 L 310 195 L 70 195 L 70 170" fill="none" stroke="#d92b1f" stroke-width="1" stroke-dasharray="4,3"/>
    <text x="190" y="210" text-anchor="middle" font-family="Inter, sans-serif" font-size="9" fill="#d92b1f">← Quality Control Checks at Every Stage →</text>

    <!-- Legend -->
    <rect x="20" y="235" width="580" height="70" rx="6" fill="#fff" stroke="#68625b" stroke-width="0.5"/>
    <text x="30" y="253" font-family="Inter, sans-serif" font-size="10" font-weight="bold" fill="#111">Key Principles</text>
    <circle cx="38" cy="268" r="3" fill="#1e4f8a"/>
    <text x="48" y="272" font-family="Inter, sans-serif" font-size="9" fill="#111">Record immediately in the field — never from memory</text>
    <circle cx="38" cy="285" r="3" fill="#165d34"/>
    <text x="48" y="289" font-family="Inter, sans-serif" font-size="9" fill="#111">Transfer to digital same day; backup to 3 separate locations</text>
    <circle cx="330" cy="268" r="3" fill="#8b5e00"/>
    <text x="340" y="272" font-family="Inter, sans-serif" font-size="9" fill="#111">Every dataset must answer: who, what, when, where, how, why</text>
    <circle cx="330" cy="285" r="3" fill="#d92b1f"/>
    <text x="340" y="289" font-family="Inter, sans-serif" font-size="9" fill="#111">Use open formats (CSV, GeoTIFF) for long-term access</text>

    <defs>
      <marker id="arrowGray11" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="6" markerHeight="6" orient="auto-start-reverse">
        <path d="M 0 0 L 10 5 L 0 10 z" fill="#68625b"/>
      </marker>
    </defs>
  </svg>
  <p class="diagram-caption">Figure — Documentation workflow from field notebook to final report, with quality control at every stage.</p>
</div>

## Field notebook essentials

### What to record

Every field session requires systematic recording of conditions, decisions, and observations. The notebook is your primary legal and scientific record.

**Mandatory entries for each survey day:**

| Category | Details to record |
|----------|-------------------|
| Personnel | Names, roles, arrival/departure times |
| Weather | Temperature, wind, precipitation, cloud cover |
| Equipment | Serial numbers, calibration status, battery levels |
| Site conditions | Soil moisture, vegetation, recent disturbance |
| Deviations | Any changes from planned methodology |
| Problems | Equipment issues, access restrictions, safety concerns |

### When to record

Record information **immediately**, not from memory hours later. The three critical recording moments are:

1. **Start of session**: Conditions, equipment setup, calibration values
2. **During work**: Anomalies, decisions, deviations from plan
3. **End of session**: Summary, equipment status, next-day preparations

<div class="info-box tip">
  <strong>Key idea</strong>
  Write in the field, not in the hotel. Memory degrades rapidly, and reconstructed notes lack the precision and legal standing of contemporaneous records.
</div>

### How to record

Use waterproof notebooks with numbered pages. Write in permanent ink—pencil smudges and fades. Each entry must include:

- Date and time (24-hour format, local timezone noted)
- Location (site name, grid reference, coordinates)
- Author initials
- Sequential entry numbers

Never erase mistakes. Draw a single line through errors and initial the correction. Blank spaces should be struck through to prevent later additions.

## Metadata standards

### The six essential questions

Every dataset must answer six fundamental questions. These form the core of any metadata record:

**Who** — Surveyor names, organization, contact information, qualifications

**What** — Survey type, instruments used, parameters measured, units

**When** — Date range, time of day, duration, timezone

**Where** — Coordinates (datum specified), site boundaries, country, region

**How** — Methodology, instrument settings, sampling intervals, processing steps

**Why** — Research objectives, client requirements, permit conditions

### Coordinate reference systems

Central Asia spans multiple coordinate systems. Always record:

- **Datum**: WGS84 is standard for GPS; Soviet-era maps use Pulkovo 1942
- **Projection**: UTM zones 40-45 cover most of Central Asia
- **Coordinate format**: Decimal degrees preferred (e.g., 41.31125°N, 69.27917°E)

<div class="info-box tip">
  <strong>Key idea</strong>
  A coordinate without its datum is ambiguous. The difference between WGS84 and Pulkovo 1942 can exceed 100 meters—enough to invalidate precise surveys.
</div>

### Instrument metadata

For each instrument, record:

| Field | Example |
|-------|---------|
| Manufacturer | Geometrics |
| Model | G-858 MagMapper |
| Serial number | M858-2019-0847 |
| Firmware version | 3.2.1 |
| Calibration date | 2024-03-15 |
| Calibration certificate | CAL-2024-0312 |
| Sensor height | 0.30 m above ground |
| Sampling rate | 10 Hz |

## Photographic documentation

### What to photograph

Photographs serve as visual verification and contextual record. Essential shots include:

1. **Overview shots**: Site from multiple directions, surrounding landscape
2. **Setup shots**: Equipment configuration, reference stations
3. **Detail shots**: Anomaly locations, soil conditions, excavation faces
4. **Scale shots**: Include scale bar or known object for size reference
5. **Problem documentation**: Equipment failures, access issues, disturbances

### Technical requirements

| Parameter | Recommendation |
|-----------|----------------|
| Resolution | Minimum 12 megapixels |
| Format | RAW + JPEG for archival; JPEG for field notes |
| Geotagging | Enable GPS in camera or use external GPS log |
| Naming | Site_Date_Sequence (e.g., SAMARKAND_20240615_001.jpg) |

### Photo log maintenance

Each photograph requires a log entry:

```
Photo ID: AFRAS_20240615_023
Time: 14:35 local
Direction: NW (315°)
Subject: GPR grid corner stake, grid A
Photographer: KA
Notes: Reference stake at grid origin (0,0)
```

<div class="info-box tip">
  <strong>Key idea</strong>
  An unlabeled photograph is nearly worthless months later. Maintain the photo log in real-time, not as an afterthought.
</div>

## GPS waypoint and track logging

### Waypoint protocols

Mark waypoints for all significant locations:

- Grid corners and control points
- Sample collection points
- Anomaly centers
- Reference stations
- Access routes and landmarks

**Waypoint naming convention:**

```
[SiteCode]-[Type]-[Number]
Examples:
SAM-GP-001  (Samarkand, Grid Point 1)
FER-SS-015  (Fergana, Soil Sample 15)
KAZ-AN-003  (Kazakhstan, Anomaly 3)
```

### Track logging

Enable continuous track logging during all survey activities. Set track recording interval based on activity:

| Activity | Track interval |
|----------|----------------|
| Walking survey | 5 seconds |
| Vehicle reconnaissance | 2 seconds |
| Detailed grid work | 1 second |
| Static measurements | Manual waypoints only |

Export tracks daily in GPX format. Include tracks in the project archive even if they seem redundant—they document actual coverage versus planned coverage.

### Accuracy documentation

Record GPS accuracy at each critical point:

- Horizontal accuracy (HDOP or estimated position error)
- Number of satellites
- Fix type (autonomous, DGPS, RTK)
- Antenna height above ground

## Data file naming conventions

### Naming structure

Adopt a consistent naming convention across all project files:

```
[Project]_[Site]_[Date]_[Type]_[Sequence].[ext]

Examples:
SILK2024_SAM01_20240615_MAG_001.dat
SILK2024_SAM01_20240615_GPR_A01.dzt
SILK2024_SAM01_20240615_XRF_042.csv
```

Never use spaces or special characters. Use underscores or hyphens. Keep names under 50 characters for compatibility.

### Version control

When reprocessing data, maintain version history:

```
SILK2024_SAM01_MAG_001_v1.dat    (original)
SILK2024_SAM01_MAG_001_v2.dat    (despike applied)
SILK2024_SAM01_MAG_001_v3.dat    (drift corrected)
```

Document processing steps in an accompanying README or log file.

### Folder structure

Organize project data hierarchically:

```
PROJECT_NAME/
├── 01_Admin/
│   ├── permits/
│   ├── contracts/
│   └── correspondence/
├── 02_Planning/
│   ├── site_maps/
│   └── survey_designs/
├── 03_Field_Data/
│   ├── raw/
│   ├── field_notes/
│   └── photos/
├── 04_Processing/
│   ├── intermediate/
│   └── final/
├── 05_Reports/
│   ├── drafts/
│   └── final/
└── 06_Archive/
    └── backups/
```

<div class="info-box tip">
  <strong>Key idea</strong>
  A colleague should be able to navigate your project folder without asking you questions. Clear structure is a form of documentation.
</div>

## Quality control checklists

### Daily field checklist

Complete before leaving the site each day:

<ul class="checklist">
  <li>All data files transferred to backup device</li>
  <li>Field notes complete with daily summary</li>
  <li>Photo log current and photos backed up</li>
  <li>GPS tracks exported and saved</li>
  <li>Equipment batteries charging</li>
  <li>Calibration records updated</li>
  <li>Tomorrow's plan confirmed</li>
</ul>

### Data quality checklist

Review after each processing stage:

<ul class="checklist">
  <li>File counts match expected totals</li>
  <li>Coordinate ranges are plausible</li>
  <li>Value ranges are within instrument specifications</li>
  <li>No unexpected gaps in coverage</li>
  <li>Processing parameters documented</li>
  <li>Version numbers incremented correctly</li>
  <li>Checksums computed for archival files</li>
</ul>

### Pre-submission checklist

Before delivering data to clients or archives:

<ul class="checklist">
  <li>All metadata fields complete</li>
  <li>Coordinate reference system documented</li>
  <li>Units specified for all measurements</li>
  <li>Processing workflow described</li>
  <li>Known limitations stated</li>
  <li>File formats are open or widely supported</li>
  <li>Readme file included</li>
  <li>License terms clear</li>
</ul>

## Survey report structure

### Standard report sections

Professional survey reports follow a consistent structure:

**1. Executive Summary** (1 page)
- Key findings in non-technical language
- Main recommendations
- Survey limitations

**2. Introduction** (1-2 pages)
- Project background and objectives
- Site location and context
- Previous work summary

**3. Methodology** (2-4 pages)
- Survey techniques employed
- Instrument specifications
- Field procedures
- Processing workflow
- Quality control measures

**4. Results** (variable length)
- Data presentation with maps and figures
- Anomaly descriptions
- Statistical summaries
- Interpretation of findings

**5. Discussion** (2-3 pages)
- Significance of results
- Comparison with expectations
- Integration with other evidence
- Confidence assessment

**6. Conclusions and Recommendations** (1-2 pages)
- Summary of key findings
- Recommended follow-up actions
- Priorities for further investigation

**7. Appendices**
- Full metadata records
- Instrument calibration certificates
- Processing parameter logs
- Complete data tables

### Writing guidelines

Reports should be:

- **Precise**: Use specific values, not vague qualifiers
- **Reproducible**: Another professional could replicate your work
- **Honest**: State limitations and uncertainties clearly
- **Accessible**: Technical content with clear explanations

<div class="info-box tip">
  <strong>Key idea</strong>
  Write the report you would want to receive. Include everything needed to evaluate and build upon your work.
</div>

## Data archiving and preservation

### Archive principles

Long-term preservation requires attention to format, redundancy, and documentation:

**Format longevity**: Choose open formats over proprietary ones. ASCII text outlasts binary formats. CSV outlasts Excel. GeoTIFF outlasts manufacturer-specific raster formats.

**Redundancy**: Maintain at least three copies on at least two different media types in at least two physical locations.

**Documentation**: Archive documentation *with* the data. A dataset without its metadata is an orphan.

### Recommended archive formats

| Data type | Archive format | Notes |
|-----------|---------------|-------|
| Tabular data | CSV with header row | UTF-8 encoding |
| Geospatial vector | GeoPackage or Shapefile | Include .prj file |
| Geospatial raster | GeoTIFF | Uncompressed or LZW |
| Images | TIFF or JPEG | Highest quality available |
| Documents | PDF/A | Archival PDF standard |
| Metadata | XML or JSON | ISO 19115 schema |

### Checksum verification

Compute checksums for all archive files to detect corruption:

```
sha256sum SILK2024_SAM01_MAG_001.csv > SILK2024_SAM01_MAG_001.csv.sha256
```

Verify checksums when retrieving archived data.

## Sharing data: formats, licensing, repositories

### Open data principles

Scientific data increasingly requires open sharing. Consider:

- **Funder requirements**: Many grants mandate open access
- **Journal policies**: Data availability statements are standard
- **Reuse potential**: Your data may answer questions you never asked

### Licensing options

| License | Conditions | Use case |
|---------|------------|----------|
| CC0 (Public Domain) | No restrictions | Maximum reuse |
| CC-BY | Attribution required | Standard academic |
| CC-BY-NC | Attribution, non-commercial | Restricted commercial use |
| CC-BY-SA | Attribution, share-alike | Viral open access |

### Repository selection

Choose repositories based on data type and community standards:

- **Zenodo**: General-purpose, DOI-issuing, free
- **Figshare**: Research data, visualization features
- **OpenContext**: Archaeological data specialty
- **PANGAEA**: Earth science data
- **Institutional repositories**: University-hosted options

<div class="info-box tip">
  <strong>Key idea</strong>
  Depositing data in a repository creates a citable, persistent record. Your survey data becomes part of the permanent scientific literature.
</div>

### Data citation format

When sharing data, provide a recommended citation:

```
Author(s). (Year). Dataset Title. Repository Name. 
https://doi.org/10.xxxx/xxxxx
```

Example:
```
Arzymatov, K. (2024). Magnetometry Survey of Afrasiab Sector B. 
Zenodo. https://doi.org/10.5281/zenodo.12345678
```

## Documentation checklist summary

Use this master checklist to verify documentation completeness:

<ul class="checklist">
  <li>Field notebook entries complete for all survey days</li>
  <li>Weather and site conditions recorded</li>
  <li>Equipment serial numbers and calibration data logged</li>
  <li>All coordinates include datum and projection</li>
  <li>Photo log matches photograph files</li>
  <li>GPS waypoints exported with descriptive names</li>
  <li>GPS tracks exported in GPX format</li>
  <li>File naming convention followed consistently</li>
  <li>Folder structure organized and logical</li>
  <li>Processing steps documented with parameters</li>
  <li>Data quality checks performed and recorded</li>
  <li>Report includes all standard sections</li>
  <li>Metadata complete for all datasets</li>
  <li>Archive copies made in open formats</li>
  <li>Checksums computed for archive files</li>
  <li>License terms specified for shared data</li>
</ul>

## Exercises

The following exercises reinforce documentation principles through practical scenarios. Work through each problem before revealing the solution.

<details>
<summary><strong>Exercise 1: Field notebook entry</strong></summary>

You arrive at a survey site at 08:30 local time on June 15, 2024. Temperature is 24°C, light wind from the west, clear sky. You are conducting magnetometry with a G-858 magnetometer (serial #M858-2019-0847). The soil is dry and vegetation is sparse grass. Write the opening field notebook entry.

**Solution:**

```
Date: 2024-06-15
Time: 08:30 (UTC+5)
Location: Afrasiab Sector B (39.6750°N, 66.9875°E)
Personnel: K. Arzymatov (lead), A. Sultanov (assistant)

Weather: Clear, 24°C, light W wind (~5 km/h), no precipitation
Site conditions: Dry soil, sparse grass cover, no recent disturbance

Equipment setup:
- Magnetometer: Geometrics G-858 (S/N: M858-2019-0847)
- Firmware: v3.2.1, Calibrated: 2024-03-15
- Sensor height: 0.30 m
- Sampling rate: 10 Hz
- GPS: Trimble R2 (S/N: TR2-2022-1156), RTK mode

Planned work: Complete grids A1-A4 (40m × 40m each)
Entry #: AFR-2024-001
Initials: KA
```
</details>

<details>
<summary><strong>Exercise 2: Coordinate datum conversion issue</strong></summary>

A colleague provides coordinates from Soviet-era maps in Pulkovo 1942 datum. Your GPS records WGS84. The Soviet coordinates are 41°18'45"N, 69°16'30"E. Estimate the approximate offset and explain why this matters for survey documentation.

**Solution:**

The transformation from Pulkovo 1942 to WGS84 in Central Asia typically shifts points by 25-150 meters, depending on location. For Uzbekistan, the offset is approximately:

- ΔLatitude: ~+2-3 arc-seconds (~60-90 m northward)
- ΔLongitude: ~-5-6 arc-seconds (~120-150 m westward)

This matters because a 100-meter offset would place survey points in completely different locations on modern base maps. Documentation must specify which datum was used for all coordinates. If mixing datasets from different sources, explicit transformation parameters must be recorded.

The documentation should state: "Soviet map coordinates (Pulkovo 1942) were transformed to WGS84 using [specific transformation method and parameters]."
</details>

<details>
<summary><strong>Exercise 3: Photo log entry</strong></summary>

You photograph a potential kiln anomaly at grid coordinates (35.5, 22.0). The time is 11:42, you are facing northeast. Write the photo log entry for image number 047.

**Solution:**

```
Photo ID: AFR_20240615_047
Time: 11:42 local (UTC+5)
Grid coordinates: (35.5, 22.0) in Grid A2
Direction: NE (045°)
Subject: Magnetic anomaly MAG-012, potential kiln feature
Scale: 30 cm ruler included in frame
Photographer: KA
Camera: Nikon D7500, 35mm lens
GPS: 39.67523°N, 66.98812°E (WGS84)
Notes: Circular concentration of fired clay fragments 
visible on surface. Anomaly amplitude ~45 nT.
```
</details>

<details>
<summary><strong>Exercise 4: File naming correction</strong></summary>

Identify the problems with these file names and provide corrected versions:

1. `Samarkand June 15 magnetometry data (final).dat`
2. `GPR_line#1_v.2.dzt`
3. `xrf_sample_42.csv`

**Solution:**

**Problem 1:** `Samarkand June 15 magnetometry data (final).dat`
- Contains spaces (compatibility issues)
- Contains parentheses (special characters)
- No project code, date format inconsistent
- **Corrected:** `SILK2024_SAM01_20240615_MAG_FINAL.dat`

**Problem 2:** `GPR_line#1_v.2.dzt`
- Contains # symbol (special character)
- Version notation uses period in middle of name
- No project, site, or date information
- **Corrected:** `SILK2024_SAM01_20240615_GPR_L01_v2.dzt`

**Problem 3:** `xrf_sample_42.csv`
- No project or site identifier
- No date
- Lowercase inconsistent with convention
- **Corrected:** `SILK2024_SAM01_20240615_XRF_042.csv`
</details>

<details>
<summary><strong>Exercise 5: Metadata completeness check</strong></summary>

Review this metadata record and identify what is missing:

```
Survey: Magnetometry
Location: Near Samarkand
Date: June 2024
Instrument: Gradiometer
Data format: CSV
```

**Solution:**

**Missing required metadata:**

- **Who:** Surveyor name, organization, contact information
- **Precise location:** Exact coordinates with datum, site boundaries
- **Specific dates:** Start and end dates, not just month
- **Instrument details:** Manufacturer, model, serial number, calibration date
- **Methodology:** Sensor height, sampling interval, traverse spacing, grid dimensions
- **Processing:** Any corrections applied, software used
- **Coordinate reference system:** Datum, projection, coordinate format
- **Units:** Measurement units (nT, nT/m, etc.)
- **Data quality:** Accuracy estimates, known issues
- **Why:** Survey objectives, research context

The minimal record provides almost no useful context for interpreting or reusing the data.
</details>

<details>
<summary><strong>Exercise 6: Waypoint naming scheme</strong></summary>

Design a waypoint naming convention for a project with three sites (Afrasiab, Pendzhikent, Varakhsha), collecting magnetometry grid points, soil samples, and marking anomalies. Provide example waypoints.

**Solution:**

**Convention:** `[SITE]-[TYPE]-[NUMBER]`

**Site codes:**
- AFR = Afrasiab
- PEN = Pendzhikent
- VAR = Varakhsha

**Type codes:**
- GP = Grid Point
- SS = Soil Sample
- AN = Anomaly
- CP = Control Point
- RP = Reference Point

**Examples:**
- `AFR-GP-001` = Afrasiab Grid Point 1
- `AFR-SS-015` = Afrasiab Soil Sample 15
- `AFR-AN-003` = Afrasiab Anomaly 3
- `PEN-GP-001` = Pendzhikent Grid Point 1
- `VAR-CP-001` = Varakhsha Control Point 1

**Numbering:** Three-digit zero-padded for sorting (001-999).
</details>

<details>
<summary><strong>Exercise 7: Daily backup verification</strong></summary>

At the end of a field day, you collected 8 GPR profiles, took 45 photographs, recorded 12 GPS waypoints, and logged 3.2 km of GPS tracks. Describe the backup verification process.

**Solution:**

**Backup verification checklist:**

1. **File counts:**
   - GPR files: Verify 8 .dzt files present
   - Photos: Count 45 .jpg files in photo folder
   - GPS waypoints: Check waypoint file contains 12 entries
   - GPS tracks: Confirm track file exists with reasonable size

2. **Spot-check files:**
   - Open one GPR file to verify data loads correctly
   - View several photos to confirm they are not corrupted
   - Check waypoint coordinates are within expected range
   - Verify track length is approximately 3.2 km

3. **Copy verification:**
   - Copy to backup drive, verify copy completed
   - Compare file sizes between original and backup
   - Optionally compute checksums on critical files

4. **Documentation:**
   - Field notes entry confirming backup complete
   - Record backup location and media used

5. **Secondary backup:**
   - Cloud upload if connectivity allows
   - Second local backup on different device
</details>

<details>
<summary><strong>Exercise 8: Report executive summary</strong></summary>

Write a one-paragraph executive summary for a magnetometry survey that covered 2 hectares, identified 15 anomalies, and found one probable kiln site. The client is a cultural heritage ministry.

**Solution:**

A magnetometry survey of 2.0 hectares at Afrasiab Sector B was completed between June 15-20, 2024. The survey identified 15 magnetic anomalies of potential archaeological significance, ranging from 20 to 85 nT in amplitude. The most significant finding is Anomaly MAG-012 (45 nT, 3 m diameter), which displays characteristics consistent with a ceramic kiln based on its dipolar signature and association with surface fired clay fragments. We recommend targeted excavation at MAG-012 to confirm the kiln interpretation, followed by geophysical investigation of adjacent Sector C. The survey data has been archived according to ministry standards and is available for integration with existing site records.
</details>

<details>
<summary><strong>Exercise 9: Processing documentation</strong></summary>

You applied three processing steps to magnetometry data: (1) despiking with 3σ threshold, (2) heading correction of -0.8 nT, (3) interpolation to 0.25 m grid. Document these steps for the project archive.

**Solution:**

**Processing Log: SILK2024_SAM01_MAG**

| Step | Operation | Parameters | Software | Date |
|------|-----------|------------|----------|------|
| 1 | Despike | Threshold: 3σ, Window: 5 samples, Replace: interpolate | Geosoft Oasis | 2024-06-21 |
| 2 | Heading correction | Offset: -0.8 nT (E-W lines), Applied to all traverses | Geosoft Oasis | 2024-06-21 |
| 3 | Gridding | Cell size: 0.25 m, Method: bi-directional line interpolation, Search radius: 1.0 m | Geosoft Oasis | 2024-06-21 |

**Input file:** SILK2024_SAM01_MAG_RAW.csv
**Output file:** SILK2024_SAM01_MAG_v3.grd

**Notes:** Heading error determined from tie-line analysis. Original data preserved in 03_Field_Data/raw/. Processor: K. Arzymatov.
</details>

<details>
<summary><strong>Exercise 10: License selection</strong></summary>

You are depositing survey data in a public repository. A mining company funded the survey and wants acknowledgment. An academic collaborator wants to use the data in a commercial textbook. Which Creative Commons license would you recommend and why?

**Solution:**

**Recommended license: CC-BY (Attribution)**

**Rationale:**

- Requires attribution, satisfying the mining company's acknowledgment requirement
- Permits commercial use, allowing the academic textbook
- Maximizes reuse potential while maintaining credit
- Standard for academic data sharing

**Alternatives considered:**

- CC-BY-NC: Would block the commercial textbook—not appropriate
- CC0: No attribution required—fails the funder's requirement
- CC-BY-SA: Share-alike requirement might complicate textbook licensing

**Implementation:** Include the funder in the recommended citation: "Data collection funded by [Mining Company]. Please cite: Arzymatov, K. (2024). [Dataset title]. Repository. DOI."
</details>

<details>
<summary><strong>Exercise 11: Weather documentation impact</strong></summary>

Explain why weather documentation matters for: (a) magnetometry, (b) soil sampling, (c) GPR surveys. What specific conditions should be recorded?

**Solution:**

**(a) Magnetometry:**
- **Magnetic storms** affect diurnal variation correction
- **Temperature** affects sensor drift
- Record: solar activity index, temperature, storm warnings
- Impact: Surveys during magnetic storms may require discarding data

**(b) Soil sampling:**
- **Recent rainfall** affects soil chemistry and moisture
- **Temperature** affects biological activity
- Record: precipitation (last 48 hours), soil moisture estimate, temperature
- Impact: Wet samples require different handling; recent rain may leach mobile elements

**(c) GPR:**
- **Soil moisture** dramatically affects radar velocity and penetration
- **Surface water** can block signal
- Record: recent precipitation, standing water, soil moisture estimate
- Impact: Velocity calibration varies with moisture; depth calculations may be incorrect if moisture conditions change
</details>

<details>
<summary><strong>Exercise 12: Calibration record</strong></summary>

Your XRF analyzer was factory-calibrated 14 months ago. The manufacturer recommends annual calibration. You are about to start a 3-week field campaign. What should you document and what actions should you take?

**Solution:**

**Documentation required:**
- Last calibration date: [14 months ago]
- Manufacturer recommendation: 12-month interval
- Calibration status: OVERDUE

**Actions:**
1. **Before fieldwork:** Run certified reference standards (CRM), record values
2. **Document deviation:** If CRM values within acceptable range (typically ±10%), note "calibration due but CRM check passed" in equipment log
3. **If significant deviation:** Postpone survey or use correction factors
4. **During fieldwork:** Run CRM daily, document results
5. **After fieldwork:** Schedule factory calibration immediately
6. **In report:** State "Instrument calibration was overdue. Daily CRM checks showed [acceptable/unacceptable] deviation of [X%]."

**Risk mitigation:** Apply empirical correction if CRM shows consistent offset. Document correction factor and rationale.
</details>

<details>
<summary><strong>Exercise 13: Folder organization</strong></summary>

You receive a project folder with these files in a single directory. Reorganize them into the standard folder structure.

```
magnetometry_raw.csv
permit_scan.pdf
site_photo_001.jpg
final_report_v2.docx
processed_grid.grd
field_notes_day1.pdf
gps_waypoints.gpx
```

**Solution:**

```
PROJECT_NAME/
├── 01_Admin/
│   └── permits/
│       └── permit_scan.pdf
├── 02_Planning/
│   (empty - no planning files provided)
├── 03_Field_Data/
│   ├── raw/
│   │   └── magnetometry_raw.csv
│   ├── field_notes/
│   │   └── field_notes_day1.pdf
│   ├── photos/
│   │   └── site_photo_001.jpg
│   └── gps/
│       └── gps_waypoints.gpx
├── 04_Processing/
│   └── final/
│       └── processed_grid.grd
├── 05_Reports/
│   └── final/
│       └── final_report_v2.docx
└── README.txt
```

**Also create:** README.txt explaining folder contents and naming conventions.
</details>

<details>
<summary><strong>Exercise 14: Checksum verification</strong></summary>

You compute SHA256 checksums for archive files. Later, one checksum fails verification. What are the possible causes and what should you do?

**Solution:**

**Possible causes:**
1. **Storage media degradation** (bit rot)
2. **Incomplete file transfer** during backup
3. **Accidental modification** of file
4. **Malicious tampering** (rare but possible)
5. **Checksum file corruption** (the checksum itself is wrong)

**Response procedure:**
1. **Check other copies:** Verify checksum against different backup
2. **If match found:** Replace corrupted file from good backup
3. **Recalculate original:** If original still exists, recompute checksum
4. **If no good copy:** Document data loss, assess impact
5. **Investigate cause:** Check storage media health, review handling procedures

**Prevention:**
- Store checksums separately from data files
- Verify checksums periodically, not just on retrieval
- Maintain multiple backup copies on different media
</details>

<details>
<summary><strong>Exercise 15: Missing documentation scenario</strong></summary>

You inherit a dataset from a retired colleague. The files are named `grid1.csv` through `grid12.csv`. There is no other documentation. List the questions you need to answer before this data can be used.

**Solution:**

**Critical unknown information:**

1. **Where?** Site name, coordinates, country, region
2. **When?** Survey date(s), year at minimum
3. **What?** Instrument type (magnetometry? GPR? resistivity?)
4. **Units?** What do the values represent? nT? Ohm-m? ns?
5. **Grid geometry?** Grid dimensions, line spacing, sample interval
6. **Coordinate system?** Local grid or georeferenced? If georeferenced, what datum?
7. **Orientation?** Grid north direction, which corner is origin
8. **Processing?** Raw or processed? What corrections applied?
9. **Quality?** Any known problems, gaps, or issues?
10. **Who?** Surveyor, organization, contact for questions
11. **Why?** Survey objectives, research context
12. **Ownership?** Can this data be used/published?

**Without this information, the data has minimal scientific value.** Attempt to locate field notebooks, contact former colleagues, check institutional records.
</details>

<details>
<summary><strong>Exercise 16: Report methodology section</strong></summary>

Write the equipment specification paragraph for a methodology section. The survey used a Sensors & Software pulseEKKO PRO GPR with 500 MHz antenna, 0.02 m trace spacing, 50 ns time window, collecting parallel profiles at 0.5 m line spacing.

**Solution:**

Ground-penetrating radar data were collected using a Sensors & Software pulseEKKO PRO system equipped with a 500 MHz center-frequency antenna. The system was configured with a 50 ns time window and 0.02 m trace spacing, achieved using an odometer wheel trigger. Parallel survey lines were collected at 0.5 m intervals across the grid. Based on a calibrated average radar velocity of 0.09 m/ns (determined from hyperbola fitting to point reflector responses), the theoretical vertical resolution is approximately 0.045 m and maximum penetration depth is approximately 2.25 m, though actual penetration was limited to approximately 1.5 m due to clay-rich soil conditions. All GPR profiles were collected with consistent antenna orientation (antenna axis perpendicular to survey direction) to maintain polarization consistency.
</details>

<details>
<summary><strong>Exercise 17: Time zone documentation</strong></summary>

Your survey spans two weeks in Uzbekistan (UTC+5, no daylight saving). Your GPS records UTC. Your camera is set to local time. Your instrument logs are in local time. Document the time synchronization procedure.

**Solution:**

**Time Reference Documentation:**

| Device | Time setting | Offset from UTC |
|--------|--------------|-----------------|
| GPS receiver | UTC | 0 hours |
| Camera | Local (Uzbekistan) | +5 hours |
| Magnetometer logger | Local (Uzbekistan) | +5 hours |
| Field notebook | Local (Uzbekistan) | +5 hours |

**Synchronization procedure:**
1. At start of each day, photograph GPS screen showing UTC time
2. Record local time in notebook at same moment
3. Verify 5-hour offset is consistent
4. Note any device clock drift (>1 minute)

**Archive convention:**
- All timestamps in metadata records converted to UTC
- Local time retained in field notes with "(UTC+5)" notation
- Photo EXIF times remain in local time; noted in photo log

**Example conversion:**
- Field note entry "11:30 local" = 06:30 UTC
- GPS waypoint "06:30:00Z" = 11:30 local
</details>

<details>
<summary><strong>Exercise 18: Anomaly documentation</strong></summary>

You identify a strong magnetic anomaly (78 nT amplitude) at grid coordinates (45.2, 33.8). Document this anomaly for the survey report.

**Solution:**

**Anomaly Record: MAG-017**

| Field | Value |
|-------|-------|
| Anomaly ID | MAG-017 |
| Grid coordinates | (45.2, 33.8) in Grid B2 |
| Geographic coordinates | 39.67845°N, 66.99023°E (WGS84) |
| Amplitude | +78 nT (positive monopolar) |
| Dimensions | 4.5 m × 3.2 m (approximate extent at half-amplitude) |
| Shape | Elongated NE-SW |
| Associated anomalies | Adjacent to MAG-016 (12 nT, possible related feature) |
| Surface observations | No visible surface features; grass cover continuous |
| Interpretation | Probable buried fired structure (kiln or hearth) based on amplitude and monopolar response |
| Confidence | Moderate—requires ground-truthing |
| Photos | AFR_20240617_078, AFR_20240617_079 |
| Recommendation | Priority 2 for excavation testing |
</details>

<details>
<summary><strong>Exercise 19: Data citation creation</strong></summary>

Create a proper data citation for a GPR survey dataset being deposited in Zenodo. The survey was conducted by K. Arzymatov and T. Bekov in May 2024 at Varakhsha, Uzbekistan. The assigned DOI is 10.5281/zenodo.87654321.

**Solution:**

**Recommended citation format:**

Arzymatov, K., & Bekov, T. (2024). Ground-Penetrating Radar Survey of Varakhsha Archaeological Site, Uzbekistan (May 2024) [Dataset]. Zenodo. https://doi.org/10.5281/zenodo.87654321

**Alternative formats:**

**APA 7:**
Arzymatov, K., & Bekov, T. (2024). *Ground-Penetrating Radar Survey of Varakhsha Archaeological Site, Uzbekistan (May 2024)* [Dataset]. Zenodo. https://doi.org/10.5281/zenodo.87654321

**DataCite:**
Arzymatov, K.; Bekov, T. (2024): Ground-Penetrating Radar Survey of Varakhsha Archaeological Site, Uzbekistan (May 2024). Zenodo. Dataset. https://doi.org/10.5281/zenodo.87654321

**Include in repository metadata:** keywords, geographic coordinates, time period covered, related publications, funding acknowledgments.
</details>

<details>
<summary><strong>Exercise 20: Quality control failure</strong></summary>

During quality review, you notice that GPS coordinates for 15 waypoints are all identical (39.670000, 66.990000). Diagnose the problem and document the issue.

**Solution:**

**Diagnosis:**

The identical coordinates with many trailing zeros suggest:
1. **GPS malfunction:** Unit recording default/fallback coordinates
2. **No satellite fix:** Indoor recording or heavy obstruction
3. **Averaging error:** Coordinates rounded or truncated
4. **Copy-paste error:** Manual entry mistake during data transfer
5. **Single-point recording:** Same location recorded repeatedly by mistake

**Investigation steps:**
1. Check GPS PDOP/accuracy values for these points
2. Review GPS track log for the survey period
3. Check field notebook for manual coordinate records
4. Verify other waypoints from same session are valid

**Documentation:**

```
QUALITY ISSUE: QC-2024-003
Date identified: 2024-06-22
Affected data: Waypoints SS-015 through SS-029
Issue: All 15 waypoints record identical coordinates
Impact: Sample locations cannot be mapped
Investigation: [Findings]
Resolution: [e.g., "Coordinates recovered from field notebook 
sketches and handheld GPS backup"]
Status: RESOLVED / UNRESOLVED
Reviewer: KA
```
</details>

<details>
<summary><strong>Exercise 21: Archive format selection</strong></summary>

You need to archive magnetometry data originally in a proprietary .mag format, GPR profiles in .dzt format, and a GIS project in .mxd format. Recommend archive formats for each and explain your reasoning.

**Solution:**

| Original | Archive format | Reasoning |
|----------|----------------|-----------|
| .mag (proprietary) | CSV + metadata | ASCII text maximizes longevity; proprietary formats may become unreadable |
| .dzt (Sensors & Software) | SEG-Y + original .dzt | SEG-Y is industry standard; retain original for full fidelity |
| .mxd (ArcGIS project) | GeoPackage + PDF map | .mxd requires specific software version; GeoPackage is open standard |

**Additional archive items:**

- README.txt explaining formats and data structure
- Processing scripts (if any) to enable reproduction
- Software version information for proprietary formats
- Sample visualization images (PNG/PDF) for quick reference

**Principle:** Archive in open formats for longevity, but retain proprietary originals when significant information would be lost in conversion.
</details>

<details>
<summary><strong>Exercise 22: Field safety documentation</strong></summary>

During a survey, a team member suffers heat exhaustion and work stops for the day. What documentation is required?

**Solution:**

**Required documentation:**

**1. Incident report:**
- Date, time, location
- Personnel involved
- Description of incident
- Environmental conditions (temperature, humidity)
- Response actions taken
- Outcome (recovery, medical treatment)
- Witnesses

**2. Field notebook entry:**
- Time work stopped
- Reason for stoppage
- Condition of affected person
- Supervisor notification

**3. Survey log:**
- Document incomplete coverage
- Note which planned work was not completed
- Record any data quality concerns

**4. Equipment record:**
- Equipment status when work stopped
- Proper shutdown procedures followed
- Storage conditions

**5. Next-day planning:**
- Modified schedule accounting for missed work
- Additional safety measures (earlier start, more breaks)

**Reporting:** File incident report per organizational policy; notify project manager.
</details>

<details>
<summary><strong>Exercise 23: Coordinate precision</strong></summary>

A GPS displays coordinates as 39.6750°N, 66.9875°E. How many decimal places are meaningful given a reported accuracy of ±3 meters? What is the appropriate precision for documentation?

**Solution:**

**Coordinate precision analysis:**

| Decimal places | Approximate precision |
|----------------|----------------------|
| 1 (39.7°) | ~11 km |
| 2 (39.67°) | ~1.1 km |
| 3 (39.675°) | ~110 m |
| 4 (39.6750°) | ~11 m |
| 5 (39.67500°) | ~1.1 m |
| 6 (39.675000°) | ~0.11 m |

**Given ±3 m accuracy:**
- 4 decimal places (11 m precision) is appropriate
- 5 decimal places implies precision better than actual accuracy
- Recording more than 5 places is false precision

**Documentation recommendation:**
- Record as displayed: 39.6750°N, 66.9875°E
- Document accuracy: "±3 m horizontal (autonomous GPS)"
- Do not add trailing zeros (39.675000°) that imply false precision
- If averaging multiple readings, report number of readings and standard deviation
</details>

<details>
<summary><strong>Exercise 24: Multi-instrument synchronization</strong></summary>

You are conducting a combined magnetometry and GPR survey. The magnetometer records at 10 Hz, the GPR at approximately 50 traces/m, and photos are taken periodically. Describe the time synchronization protocol.

**Solution:**

**Synchronization protocol:**

**At session start:**
1. Display GPS UTC time on screen
2. Photograph GPS display with camera
3. Start magnetometer recording, note start time
4. Start GPR recording, note start time
5. Record all times in field notebook

**Reference table:**
| Device | Time source | Sync method | Check frequency |
|--------|-------------|-------------|-----------------|
| GPS | Satellite UTC | Self-correcting | N/A |
| Camera | Internal clock | Photo of GPS time | Daily |
| Magnetometer | Internal clock | Record GPS time at start/end | Each session |
| GPR | Internal clock | Record GPS time at start/end | Each session |

**During survey:**
- Mark common waypoints on all systems
- Photograph GPS at waypoints with time visible
- Log synchronization events in field notes

**End of session:**
- Record end times from all devices
- Calculate drift: (End_device - End_GPS) - (Start_device - Start_GPS)
- Apply drift correction if >1 second per hour

**Documentation:** "All instruments synchronized to GPS UTC at session start. Maximum observed drift: X seconds over Y hours."
</details>

<details>
<summary><strong>Exercise 25: Report limitation statement</strong></summary>

Write a limitations paragraph for a survey report where: (1) rain prevented work for 2 of 7 planned days, (2) one grid could not be surveyed due to modern construction, and (3) the magnetometer showed increased noise near a power line.

**Solution:**

**Limitations:**

This survey achieved coverage of approximately 71% of the planned survey area due to field constraints. Adverse weather conditions (rain on June 18-19) prevented data collection for two scheduled survey days, resulting in incomplete coverage of the northern sector. Grid D4 (0.25 ha) could not be surveyed due to recent construction activity that introduced significant ground disturbance and metallic debris. Additionally, data quality along the western edge of Grids A1-A4 (within approximately 15 m of the high-voltage power line on the site boundary) is compromised by electromagnetic interference; magnetic anomalies in this zone should be interpreted with caution and may represent noise rather than archaeological features. Survey results outside these affected areas are considered reliable. Future survey campaigns should schedule weather contingency days and may consider alternative methods (such as GPR) for the power line margin zone.
</details>

<details>
<summary><strong>Exercise 26: Version control scenario</strong></summary>

You have processed a dataset three times, making corrections each time. The files are named data_final.csv, data_final2.csv, and data_FINAL_FINAL.csv. Restructure this naming to follow proper version control conventions.

**Solution:**

**Proper naming structure:**

```
SILK2024_SAM01_MAG_v1.0.csv  (original processing)
SILK2024_SAM01_MAG_v1.1.csv  (minor corrections)
SILK2024_SAM01_MAG_v2.0.csv  (major reprocessing)
```

**Accompanying changelog:**

```
SILK2024_SAM01_MAG_CHANGELOG.txt

v1.0 (2024-06-20)
- Initial processing with standard despike and drift correction

v1.1 (2024-06-21)
- Corrected heading error in lines 15-18
- Fixed coordinate offset in grid B

v2.0 (2024-06-23)
- Complete reprocessing with updated base station data
- Applied improved interpolation algorithm
- This is the authoritative version
```

**Version numbering convention:**
- Major.Minor format
- Major: Significant reprocessing or methodology change
- Minor: Bug fixes, small corrections
- Always document changes in changelog file
</details>

<details>
<summary><strong>Exercise 27: Repository selection</strong></summary>

You have completed surveys at three sites for a European Union-funded project. The grant requires open data publication. Your datasets include magnetometry grids, XRF measurements, and georeferenced site maps. Recommend appropriate repositories.

**Solution:**

**Repository recommendations:**

| Data type | Repository | Reasoning |
|-----------|------------|-----------|
| Magnetometry/XRF data | Zenodo | General-purpose, EU-affiliated, DOI-issuing, meets Open Access requirements |
| Site maps (GIS) | Zenodo | Same as above; accepts GeoPackage |
| Archaeological interpretation | OpenContext | Archaeology-specific, linked data capability, peer review option |

**EU compliance considerations:**
- Zenodo is hosted by CERN, aligns with EU Open Science Policy
- Check specific grant requirements (Horizon Europe has FAIR data mandate)
- Use CC-BY license unless grant specifies otherwise

**Implementation:**
1. Create Zenodo upload for raw and processed data
2. Link datasets using related identifiers
3. Create OpenContext project for interpretive content
4. Cross-reference DOIs between repositories
5. Include data availability statement in publications

**Metadata requirements:**
- Funding acknowledgment (grant number)
- Project name and dates
- Geographic coordinates
- Keywords for discoverability
</details>

<details>
<summary><strong>Exercise 28: Field notebook recovery</strong></summary>

Your field notebook is damaged by water, making several pages illegible. Describe recovery strategies and documentation procedures.

**Solution:**

**Immediate actions:**
1. Do not force pages apart—allow to dry naturally
2. Place absorbent paper between wet pages
3. Photograph all legible portions immediately

**Recovery strategies:**
1. **Cross-reference:** Reconstruct entries from other records (GPS logs, photo timestamps, instrument files, daily emails/messages)
2. **Colleague memory:** Interview team members while memory is fresh
3. **Digital backup:** Check if any pages were photographed previously
4. **Instrument logs:** Extract times and values from data files

**Documentation:**

```
RECORD DAMAGE REPORT: RDR-2024-001
Date: 2024-06-20
Affected: Field notebook pages 23-31 (June 16-17 entries)
Cause: Water damage from rain exposure
Recovery attempt: Partial—approximately 60% legible

Reconstructed information:
- Grid coverage: Confirmed from GPS tracks
- Waypoint coordinates: Recovered from GPS export
- Weather: Reconstructed from meteorological station data
- Equipment settings: Recovered from instrument log files

Unrecoverable information:
- Detailed observational notes for anomalies MAG-008 to MAG-011
- Soil condition descriptions
- Several sketch diagrams

Signed: KA
Date: 2024-06-21
```

**Prevention:** Photograph critical pages daily; use waterproof notebooks.
</details>

<details>
<summary><strong>Exercise 29: Metadata schema selection</strong></summary>

You are creating formal metadata for a geophysical dataset. Compare ISO 19115 and Dublin Core, and recommend which to use for archiving a magnetometry survey.

**Solution:**

**Comparison:**

| Feature | ISO 19115 | Dublin Core |
|---------|-----------|-------------|
| Complexity | High (400+ elements) | Low (15 elements) |
| Geospatial support | Comprehensive | Limited |
| Coordinate systems | Detailed specification | Not defined |
| Data quality | Extensive quality elements | No quality fields |
| Adoption | GIS/Earth science standard | General web standard |
| Implementation effort | High | Low |

**Recommendation: ISO 19115 (or profile like ISO 19115-2)**

**Reasoning:**
- Magnetometry is geospatial data—needs coordinate reference system documentation
- Data quality metadata (accuracy, completeness) is essential
- ISO 19115 is the standard for geoscientific data archives
- Many repositories (e.g., PANGAEA) use ISO 19115

**Practical approach:**
- Use ISO 19115 core elements for repository submission
- Generate Dublin Core automatically for broad discoverability
- Many tools convert ISO 19115 → Dublin Core automatically

**Minimum ISO 19115 elements for magnetometry:**
- Title, abstract, dates, contacts
- Geographic extent (bounding box)
- Coordinate reference system
- Lineage (processing history)
- Data quality report
</details>

<details>
<summary><strong>Exercise 30: Complete documentation audit</strong></summary>

Review this documentation package and identify at least five deficiencies:

```
Files provided:
- mag_data.csv (no header row)
- report.docx (dated 2024)
- photos/ (47 JPG files, named IMG_0001.jpg etc.)
- readme.txt: "Magnetometry survey of archaeological site. 
  Contact: email@example.com"
```

**Solution:**

**Documentation deficiencies identified:**

1. **No coordinate information**
   - Site location not specified
   - Coordinates absent from data file
   - No datum or projection defined

2. **Missing metadata**
   - No instrument specification
   - No survey dates
   - No surveyor/organization name
   - No methodology description

3. **Data file problems**
   - No header row (columns undefined)
   - No units specified
   - File name non-descriptive

4. **Photo documentation absent**
   - Photos not renamed (generic camera names)
   - No photo log linking images to locations
   - No EXIF data mentioned/preserved

5. **Incomplete readme**
   - No data structure explanation
   - No processing description
   - No license information
   - Contact email insufficient (name, organization missing)

6. **Version control missing**
   - No version numbers
   - "2024" is insufficient date precision
   - No changelog

7. **No quality documentation**
   - No accuracy statement
   - No limitations described
   - No calibration information

**Corrective actions:** Create complete metadata record, rename files to convention, produce photo log, expand readme, add version control, document limitations.
</details>

## Summary

Proper documentation transforms raw measurements into scientific evidence. The investment in systematic recording, standardized metadata, and professional reporting pays dividends throughout a project's lifecycle and beyond. Your future self—and colleagues who inherit your work—will thank you for clear, complete, and consistent documentation.

The principles covered in this tutorial apply across all survey methods. Whether you are conducting electromagnetic surveys, GPR profiling, magnetometry mapping, or geochemical sampling, the documentation framework remains consistent: record everything, organize systematically, archive for longevity, and share appropriately.

<div class="info-box tip">
  <strong>Key idea</strong>
  Excellence in documentation is the hallmark of professional survey practice. It is what distinguishes scientific data from mere numbers.
</div>
