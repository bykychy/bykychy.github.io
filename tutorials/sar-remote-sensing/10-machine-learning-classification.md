---
layout: tutorial
title: "How Machine Learning Classifies Landscapes and Detects Change in Central Asia"
description: "A math-first tutorial on supervised classification, random forests, neural networks, and accuracy assessment for remote sensing in Central Asia, with 30 exercises and solutions."
difficulty: advanced
estimated_time: "3-4 hours"
series: sar-remote-sensing
order: 10
---

## Why this tutorial matters

For decades, analysts manually interpreted satellite imagery—drawing polygons, visually identifying land cover types, comparing images by eye. This was slow, subjective, and impossible to scale across Central Asia's 4 million square kilometers.

Machine learning transforms this. Algorithms learn patterns from labeled examples and classify millions of pixels automatically. A model trained on 500 samples can classify 40 million pixels in seconds.

<div class="info-box tip">
  <strong>Key idea</strong>
  Machine learning amplifies human expertise. Domain knowledge guides feature selection and training data collection. The algorithm handles repetitive classification at scale.
</div>

<div class="learning-objectives">
  <div class="learning-objectives-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#1e4f8a" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M22 11.08V12a10 10 0 1 1-5.93-9.14"/><polyline points="22 4 12 14.01 9 11.01"/></svg>
    <h3>What you will learn</h3>
  </div>
  <ul>
    <li>Formulate remote sensing classification as a supervised machine learning problem (features, labels, training/test split)</li>
    <li>Implement Random Forest, SVM, and k-NN classifiers for multi-class land cover mapping</li>
    <li>Engineer spectral, textural, and temporal features from Sentinel-2 and SAR imagery</li>
    <li>Evaluate classifier accuracy with confusion matrices, overall accuracy, kappa, and F1-score</li>
    <li>Apply spatial cross-validation to avoid inflated accuracy from spatial autocorrelation</li>
  </ul>
</div>

<div class="prerequisites">
  <div class="prerequisites-header">
    <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#8b5e00" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M4 19.5A2.5 2.5 0 0 1 6.5 17H20"/><path d="M6.5 2H20v20H6.5A2.5 2.5 0 0 1 4 19.5v-15A2.5 2.5 0 0 1 6.5 2z"/></svg>
    <h3>Prerequisites</h3>
  </div>
  <ul>
    <li>Intermediate Python with NumPy and familiarity with scikit-learn API</li>
    <li>Basic statistics: mean, variance, probability distributions, Bayes’ theorem</li>
    <li>Understanding of multispectral imagery and vegetation indices (NDVI, NDWI)</li>
    <li>Completion of the GEE Python tutorial (Tutorial 09) for cloud-based workflows</li>
  </ul>
</div>

<div class="concept-diagram">
  <svg viewBox="0 0 620 320" xmlns="http://www.w3.org/2000/svg" style="max-width: 580px;">
    <!-- Stage 1: Training data -->
    <rect x="10" y="50" width="105" height="110" rx="7" fill="#fff" stroke="#8b5e00" stroke-width="1.8"/>
    <text x="62" y="72" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" fill="#8b5e00" font-weight="700">Training Data</text>
    <rect x="22" y="84" width="14" height="14" rx="2" fill="#165d34"/><text x="42" y="95" font-family="Inter, sans-serif" font-size="9" fill="#111">Forest</text>
    <rect x="22" y="102" width="14" height="14" rx="2" fill="#d92b1f"/><text x="42" y="113" font-family="Inter, sans-serif" font-size="9" fill="#111">Urban</text>
    <rect x="22" y="120" width="14" height="14" rx="2" fill="#1e4f8a"/><text x="42" y="131" font-family="Inter, sans-serif" font-size="9" fill="#111">Water</text>
    <rect x="22" y="138" width="14" height="14" rx="2" fill="#8b5e00"/><text x="42" y="149" font-family="Inter, sans-serif" font-size="9" fill="#111">Cropland</text>
    <!-- Arrow 1-2 -->
    <line x1="115" y1="105" x2="150" y2="105" stroke="#111" stroke-width="1.5"/>
    <polygon points="150,100 160,105 150,110" fill="#111"/>
    <!-- Stage 2: Feature extraction -->
    <rect x="162" y="40" width="110" height="130" rx="7" fill="#fff" stroke="#1e4f8a" stroke-width="1.8"/>
    <text x="217" y="62" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" fill="#1e4f8a" font-weight="700">Feature</text>
    <text x="217" y="76" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" fill="#1e4f8a" font-weight="700">Extraction</text>
    <text x="217" y="98" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">Spectral bands</text>
    <text x="217" y="114" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">NDVI, NDWI</text>
    <text x="217" y="130" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">SAR backscatter</text>
    <text x="217" y="146" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">GLCM texture</text>
    <text x="217" y="162" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">Temporal stats</text>
    <!-- Arrow 2-3 -->
    <line x1="272" y1="105" x2="307" y2="105" stroke="#111" stroke-width="1.5"/>
    <polygon points="307,100 317,105 307,110" fill="#111"/>
    <!-- Stage 3: Classifier -->
    <rect x="319" y="45" width="110" height="120" rx="7" fill="#fff" stroke="#d92b1f" stroke-width="1.8"/>
    <text x="374" y="67" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" fill="#d92b1f" font-weight="700">Classifier</text>
    <line x1="374" y1="82" x2="374" y2="105" stroke="#d92b1f" stroke-width="1.5"/>
    <line x1="374" y1="82" x2="350" y2="100" stroke="#d92b1f" stroke-width="1.2"/>
    <line x1="374" y1="82" x2="398" y2="100" stroke="#d92b1f" stroke-width="1.2"/>
    <circle cx="350" cy="103" r="5" fill="#d92b1f" opacity="0.25"/>
    <circle cx="398" cy="103" r="5" fill="#d92b1f" opacity="0.25"/>
    <line x1="350" y1="108" x2="340" y2="120" stroke="#d92b1f" stroke-width="1"/>
    <line x1="350" y1="108" x2="360" y2="120" stroke="#d92b1f" stroke-width="1"/>
    <line x1="398" y1="108" x2="388" y2="120" stroke="#d92b1f" stroke-width="1"/>
    <line x1="398" y1="108" x2="408" y2="120" stroke="#d92b1f" stroke-width="1"/>
    <text x="374" y="145" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">Random Forest</text>
    <text x="374" y="158" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">SVM / k-NN</text>
    <!-- Arrow 3-4 -->
    <line x1="429" y1="105" x2="464" y2="105" stroke="#111" stroke-width="1.5"/>
    <polygon points="464,100 474,105 464,110" fill="#111"/>
    <!-- Stage 4: Classified map -->
    <rect x="476" y="50" width="130" height="110" rx="7" fill="#fff" stroke="#165d34" stroke-width="1.8"/>
    <text x="541" y="72" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" fill="#165d34" font-weight="700">Classified Map</text>
    <rect x="494" y="82" width="16" height="16" fill="#165d34"/><rect x="510" y="82" width="16" height="16" fill="#165d34"/><rect x="526" y="82" width="16" height="16" fill="#8b5e00"/><rect x="542" y="82" width="16" height="16" fill="#8b5e00"/><rect x="558" y="82" width="16" height="16" fill="#d92b1f"/>
    <rect x="494" y="98" width="16" height="16" fill="#165d34"/><rect x="510" y="98" width="16" height="16" fill="#8b5e00"/><rect x="526" y="98" width="16" height="16" fill="#8b5e00"/><rect x="542" y="98" width="16" height="16" fill="#1e4f8a"/><rect x="558" y="98" width="16" height="16" fill="#d92b1f"/>
    <rect x="494" y="114" width="16" height="16" fill="#8b5e00"/><rect x="510" y="114" width="16" height="16" fill="#8b5e00"/><rect x="526" y="114" width="16" height="16" fill="#1e4f8a"/><rect x="542" y="114" width="16" height="16" fill="#1e4f8a"/><rect x="558" y="114" width="16" height="16" fill="#8b5e00"/>
    <rect x="494" y="130" width="16" height="16" fill="#8b5e00"/><rect x="510" y="130" width="16" height="16" fill="#1e4f8a"/><rect x="526" y="130" width="16" height="16" fill="#1e4f8a"/><rect x="542" y="130" width="16" height="16" fill="#1e4f8a"/><rect x="558" y="130" width="16" height="16" fill="#8b5e00"/>
    <!-- Accuracy assessment -->
    <rect x="100" y="210" width="420" height="90" rx="7" fill="#fff" stroke="#68625b" stroke-width="1.5"/>
    <text x="310" y="233" text-anchor="middle" font-family="Inter, sans-serif" font-size="11" fill="#68625b" font-weight="700">Accuracy Assessment</text>
    <text x="170" y="258" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">Confusion Matrix</text>
    <text x="310" y="258" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">OA / Kappa / F1</text>
    <text x="440" y="258" text-anchor="middle" font-family="Inter, sans-serif" font-size="10" fill="#111">Spatial CV</text>
    <rect x="142" y="265" width="12" height="12" fill="#165d34" opacity="0.7"/><rect x="154" y="265" width="12" height="12" fill="#eee"/><rect x="166" y="265" width="12" height="12" fill="#eee"/>
    <rect x="142" y="277" width="12" height="12" fill="#eee"/><rect x="154" y="277" width="12" height="12" fill="#1e4f8a" opacity="0.7"/><rect x="166" y="277" width="12" height="12" fill="#eee"/>
    <rect x="142" y="289" width="12" height="12" fill="#eee"/><rect x="154" y="289" width="12" height="12" fill="#eee"/><rect x="166" y="289" width="12" height="12" fill="#d92b1f" opacity="0.7"/>
    <line x1="541" y1="160" x2="541" y2="210" stroke="#68625b" stroke-width="1.2" stroke-dasharray="4 3"/>
    <polygon points="537,207 541,215 545,207" fill="#68625b"/>
  </svg>
  <p class="diagram-caption">Figure: Supervised classification pipeline. Labeled training pixels are transformed into feature vectors, fed into a classifier (Random Forest, SVM), producing a land-cover map validated through accuracy assessment with spatial cross-validation.</p>
</div>

## Machine learning fundamentals

### Features, labels, and learning

In supervised ML, we have $n$ samples with $p$ features and associated labels:

- **Feature matrix** $\mathbf{X} \in \mathbb{R}^{n \times p}$: rows are samples, columns are features
- **Label vector** $\mathbf{y} \in \{1, 2, ..., K\}^n$ for $K$ classes

The goal is learning a function $f: \mathbb{R}^p \rightarrow \{1, ..., K\}$ that generalizes to new samples:

$$
\hat{y} = f(\mathbf{x})
$$

### Training, validation, and testing

Data splitting prevents overfitting:

- **Training set** (70%): fit model parameters
- **Validation set** (15%): tune hyperparameters
- **Test set** (15%): final accuracy assessment

<div class="info-box tip">
  <strong>Key idea</strong>
  Use spatial blocking for remote sensing. Assign entire geographic blocks to train/test sets to avoid inflated accuracy from spatial autocorrelation.
</div>

### The bias-variance tradeoff

Model error decomposes into:

$$
\mathbb{E}[(y - \hat{f})^2] = \text{Bias}^2 + \text{Variance} + \sigma^2
$$

Simple models have high bias; complex models have high variance. Finding the right complexity is key.

## Supervised versus unsupervised classification

**Supervised classification** requires labeled training data—you provide examples and the algorithm learns to distinguish them. Higher accuracy but requires expensive data collection.

**Unsupervised classification** (clustering) groups pixels by spectral similarity without labels. K-means minimizes within-cluster variance:

$$
\min_{\{C_k\}} \sum_{k=1}^{K} \sum_{\mathbf{x}_i \in C_k} \|\mathbf{x}_i - \boldsymbol{\mu}_k\|^2
$$

No labeled data needed, but clusters may not correspond to meaningful classes. Hybrid approaches work well: cluster first, then collect targeted training samples within each stratum.

## Common classification algorithms

### K-Nearest Neighbors (k-NN)

K-NN classifies by majority vote among $k$ nearest neighbors:

$$
\hat{y} = \text{mode}\{y_i : \mathbf{x}_i \in N_k(\mathbf{x})\}
$$

Feature scaling is critical since features with larger ranges dominate distance calculations.

```python
from sklearn.neighbors import KNeighborsClassifier
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
knn = KNeighborsClassifier(n_neighbors=5)
knn.fit(X_train_scaled, y_train)
```

### Support Vector Machines (SVM)

SVMs find the hyperplane maximizing margin between classes:

$$
\min_{\mathbf{w}, b} \frac{1}{2}\|\mathbf{w}\|^2 \quad \text{s.t.} \quad y_i(\mathbf{w}^T \mathbf{x}_i + b) \geq 1
$$

The RBF kernel handles non-linear boundaries:

$$
K(\mathbf{x}, \mathbf{x}') = \exp\left(-\gamma \|\mathbf{x} - \mathbf{x}'\|^2\right)
$$

```python
from sklearn.svm import SVC
svm = SVC(kernel='rbf', C=1.0, gamma='scale')
svm.fit(X_train_scaled, y_train)
```

### Random Forest

Random Forest builds an ensemble of decision trees, each trained on a bootstrap sample with a random subset of features. Final predictions aggregate individual tree votes.

For classification:
$$
\hat{y} = \text{mode}\{h_t(\mathbf{x}) : t = 1, ..., T\}
$$

where $h_t$ is the $t$-th tree and $T$ is the total number of trees.

<div class="info-box tip">
  <strong>Key idea</strong>
  Random Forest is often the best starting point for remote sensing classification. It handles high-dimensional data well, provides feature importance rankings, requires minimal hyperparameter tuning, and is robust to noise.
</div>

Key hyperparameters:
- **n_estimators**: number of trees (more is generally better, with diminishing returns)
- **max_depth**: maximum tree depth (controls complexity)
- **max_features**: features considered at each split (typically $\sqrt{p}$ for classification)

```python
from sklearn.ensemble import RandomForestClassifier

# Train Random Forest
rf = RandomForestClassifier(
    n_estimators=200,
    max_depth=20,
    max_features='sqrt',
    random_state=42,
    n_jobs=-1  # Use all CPU cores
)
rf.fit(X_train, y_train)

# Feature importance
importance = rf.feature_importances_
for name, imp in zip(feature_names, importance):
    print(f"{name}: {imp:.4f}")
```

### Gradient Boosting

Gradient boosting builds trees sequentially, each correcting errors of the previous ensemble:

$$
F_m(\mathbf{x}) = F_{m-1}(\mathbf{x}) + \eta h_m(\mathbf{x})
$$

where $\eta$ is the learning rate and $h_m$ is fitted to the negative gradient of the loss function.

XGBoost and LightGBM are efficient implementations widely used in competitions and production.

```python
from sklearn.ensemble import GradientBoostingClassifier

gb = GradientBoostingClassifier(
    n_estimators=100,
    learning_rate=0.1,
    max_depth=5
)
gb.fit(X_train, y_train)
```

## Feature engineering for remote sensing

### Spectral features

Raw spectral bands are the foundation. For Sentinel-2, 10 bands are commonly used:

| Band | Wavelength (nm) | Resolution | Use |
|------|-----------------|------------|-----|
| B2 | 490 (Blue) | 10m | Water, atmosphere |
| B3 | 560 (Green) | 10m | Vegetation vigor |
| B4 | 665 (Red) | 10m | Chlorophyll absorption |
| B5 | 705 (Red Edge 1) | 20m | Vegetation stress |
| B6 | 740 (Red Edge 2) | 20m | Canopy structure |
| B7 | 783 (Red Edge 3) | 20m | Leaf area index |
| B8 | 842 (NIR) | 10m | Vegetation, water |
| B8A | 865 (NIR narrow) | 20m | Vegetation monitoring |
| B11 | 1610 (SWIR 1) | 20m | Moisture, geology |
| B12 | 2190 (SWIR 2) | 20m | Minerals, moisture |

**Vegetation indices** enhance discrimination:

$$
\text{NDVI} = \frac{B8 - B4}{B8 + B4}
$$

$$
\text{EVI} = 2.5 \times \frac{B8 - B4}{B8 + 6 \times B4 - 7.5 \times B2 + 1}
$$

$$
\text{NDWI} = \frac{B3 - B8}{B3 + B8}
$$

$$
\text{NDBI} = \frac{B11 - B8}{B11 + B8}
$$

### Textural features

Texture captures spatial patterns. The Gray-Level Co-occurrence Matrix (GLCM) measures how often pixel pairs with specific values occur at a given distance and direction.

Common GLCM statistics:

**Contrast** measures local variation:
$$
\text{Contrast} = \sum_{i,j} (i-j)^2 P(i,j)
$$

**Homogeneity** measures local similarity:
$$
\text{Homogeneity} = \sum_{i,j} \frac{P(i,j)}{1 + |i-j|}
$$

**Entropy** measures randomness:
$$
\text{Entropy} = -\sum_{i,j} P(i,j) \log P(i,j)
$$

where $P(i,j)$ is the probability of co-occurrence.

```python
from skimage.feature import graycomatrix, graycoprops

def compute_glcm_features(image, distances=[1], angles=[0]):
    """Compute GLCM texture features."""
    # Quantize to 64 levels
    image_quantized = (image / 4).astype(np.uint8)
    
    glcm = graycomatrix(image_quantized, distances, angles, 
                        levels=64, symmetric=True, normed=True)
    
    features = {
        'contrast': graycoprops(glcm, 'contrast')[0, 0],
        'homogeneity': graycoprops(glcm, 'homogeneity')[0, 0],
        'energy': graycoprops(glcm, 'energy')[0, 0],
        'correlation': graycoprops(glcm, 'correlation')[0, 0]
    }
    return features
```

### Temporal features

Time series capture phenological patterns critical for crop identification. Key temporal features:

- **Maximum NDVI**: peak vegetation greenness
- **Amplitude**: difference between max and min NDVI
- **Green-up date**: day of year when NDVI rises above threshold
- **Season length**: duration above threshold

For Central Asia, cotton shows a distinctive late-season green-up (June-July), while winter wheat peaks in May before rapid senescence.

```python
def extract_temporal_features(ndvi_timeseries, dates):
    """Extract phenological features from NDVI time series."""
    features = {
        'max_ndvi': np.max(ndvi_timeseries),
        'min_ndvi': np.min(ndvi_timeseries),
        'amplitude': np.max(ndvi_timeseries) - np.min(ndvi_timeseries),
        'mean_ndvi': np.mean(ndvi_timeseries),
        'std_ndvi': np.std(ndvi_timeseries)
    }
    
    # Date of maximum (as day of year)
    max_idx = np.argmax(ndvi_timeseries)
    features['date_max'] = dates[max_idx].timetuple().tm_yday
    
    return features
```

## Training data collection strategies

### Reference data sources

Quality training data is the foundation of accurate classification. Sources include:

1. **Field surveys**: GPS points with direct observation
2. **Very high resolution imagery**: Google Earth, Maxar, Planet
3. **Existing maps**: land cover databases, agricultural statistics
4. **Crowdsourced data**: OpenStreetMap, Geo-Wiki

For Central Asia, field access can be challenging. High-resolution imagery interpretation supplemented with local expert knowledge often provides the best balance.

### Sample design considerations

**Class balance**: Ensure sufficient samples for minority classes. Imbalanced training leads to models that ignore rare classes.

**Spatial distribution**: Distribute samples across the study area to capture within-class variability.

**Temporal consistency**: Collect samples that match the imagery acquisition dates.

**Sample size**: Rules of thumb suggest 50-100 samples per class minimum, but more is better. Complex landscapes require more samples.

<div class="info-box tip">
  <strong>Key idea</strong>
  Iterative refinement improves training data. Run an initial classification, identify areas of confusion, collect additional samples in those areas, and retrain. This active learning approach focuses effort where it matters most.
</div>

### Handling mixed pixels

At 10-30m resolution, many pixels contain multiple land cover types. Strategies include:

- **Pure pixel selection**: only label pixels clearly dominated by one class
- **Fuzzy training**: assign fractional class memberships
- **Sub-pixel classification**: model class proportions rather than hard labels

## Accuracy assessment

### The confusion matrix

The confusion matrix tabulates predicted versus actual classes:

|  | Predicted A | Predicted B | Predicted C |
|--|-------------|-------------|-------------|
| **Actual A** | $n_{AA}$ | $n_{AB}$ | $n_{AC}$ |
| **Actual B** | $n_{BA}$ | $n_{BB}$ | $n_{BC}$ |
| **Actual C** | $n_{CA}$ | $n_{CB}$ | $n_{CC}$ |

Diagonal elements are correct classifications; off-diagonal elements are errors.

### Accuracy metrics

**Overall accuracy (OA)**:
$$
\text{OA} = \frac{\sum_{i} n_{ii}}{n}
$$

**Producer's accuracy** (recall) for class $i$:
$$
\text{PA}_i = \frac{n_{ii}}{\sum_{j} n_{ij}}
$$

**User's accuracy** (precision) for class $i$:
$$
\text{UA}_i = \frac{n_{ii}}{\sum_{j} n_{ji}}
$$

**F1-score** balances precision and recall:
$$
F1_i = 2 \times \frac{\text{PA}_i \times \text{UA}_i}{\text{PA}_i + \text{UA}_i}
$$

### Cohen's Kappa

Kappa adjusts for chance agreement:

$$
\kappa = \frac{p_o - p_e}{1 - p_e}
$$

where $p_o$ is observed agreement and $p_e$ is expected agreement by chance:

$$
p_e = \sum_{i} \frac{n_{i+} \times n_{+i}}{n^2}
$$

Interpretation: $\kappa < 0.4$ poor, $0.4-0.6$ moderate, $0.6-0.8$ substantial, $> 0.8$ excellent.

```python
from sklearn.metrics import confusion_matrix, classification_report, cohen_kappa_score

# Compute metrics
cm = confusion_matrix(y_test, predictions)
print("Confusion Matrix:")
print(cm)

print("\nClassification Report:")
print(classification_report(y_test, predictions, target_names=class_names))

kappa = cohen_kappa_score(y_test, predictions)
print(f"\nCohen's Kappa: {kappa:.4f}")
```

### Cross-validation

K-fold cross-validation provides robust accuracy estimates by rotating through different train/test splits:

```python
from sklearn.model_selection import cross_val_score, StratifiedKFold

# Stratified K-fold maintains class proportions
cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)

# Cross-validation scores
scores = cross_val_score(rf, X, y, cv=cv, scoring='accuracy')
print(f"Accuracy: {scores.mean():.3f} ± {scores.std():.3f}")
```

For spatial data, use spatial cross-validation that respects geographic structure.

## Central Asia applications

### Land cover classification

Central Asia's landscapes include irrigated cropland, rainfed agriculture, rangelands, deserts, mountain forests, and wetlands. A typical classification scheme:

1. Water (permanent lakes, rivers, reservoirs)
2. Irrigated cropland
3. Rainfed cropland
4. Grassland/rangeland
5. Shrubland
6. Bare soil/desert
7. Urban/built-up
8. Wetland
9. Forest
10. Snow/ice

The Aral Sea region exemplifies dramatic land cover change. Former seabed now appears as bare soil or salt flats, while former fishing villages are abandoned.

### Crop type mapping

Distinguishing crop types requires temporal features capturing phenological differences:

- **Cotton**: late green-up (June), peak in August-September
- **Winter wheat**: green through winter, harvest in June
- **Rice**: flooded fields in spring, high NDVI in summer
- **Alfalfa**: multiple harvest cycles, sustained greenness

```python
# Feature set for crop classification
feature_columns = [
    # Spectral (single date)
    'B2', 'B3', 'B4', 'B5', 'B6', 'B7', 'B8', 'B11', 'B12',
    # Indices
    'NDVI', 'EVI', 'NDWI',
    # Temporal statistics
    'NDVI_max', 'NDVI_mean', 'NDVI_std', 'NDVI_amplitude',
    'date_greenup', 'date_peak', 'season_length'
]

# Train classifier
rf_crop = RandomForestClassifier(n_estimators=300, max_depth=25)
rf_crop.fit(X_train[feature_columns], y_train)
```

### Wetland mapping

Central Asia's wetlands—deltas of the Amu Darya and Syr Darya, Lake Balkhash margins, seasonal lakes—are ecologically critical and highly dynamic. Classification challenges include:

- Seasonal flooding patterns
- Gradients between open water, emergent vegetation, and upland
- Salt-affected areas that confuse spectral signatures

Multi-temporal Sentinel-1 SAR improves wetland detection by capturing surface water extent independent of cloud cover.

## Deep learning introduction

### Why neural networks for remote sensing?

Traditional ML requires hand-crafted features. Deep learning learns features automatically from raw data. For image classification, this is transformative: convolutional neural networks (CNNs) learn spatial patterns—edges, textures, shapes—that humans would struggle to define explicitly.

### Neural network basics

A neural network transforms inputs through layers of neurons. Each neuron computes:

$$
z = \mathbf{w}^T \mathbf{x} + b
$$
$$
a = \sigma(z)
$$

where $\sigma$ is an activation function (ReLU, sigmoid, tanh).

**ReLU** is standard for hidden layers:
$$
\text{ReLU}(z) = \max(0, z)
$$

**Softmax** converts outputs to class probabilities:
$$
\text{softmax}(z_i) = \frac{e^{z_i}}{\sum_j e^{z_j}}
$$

Training minimizes cross-entropy loss via backpropagation and gradient descent:

$$
L = -\sum_{i} y_i \log(\hat{y}_i)
$$

### Convolutional Neural Networks (CNNs)

CNNs apply learnable filters that slide across images, detecting local patterns:

$$
(f * g)(i, j) = \sum_{m} \sum_{n} f(m, n) \cdot g(i-m, j-n)
$$

Key components:
- **Convolutional layers**: learn spatial filters
- **Pooling layers**: reduce spatial dimensions
- **Fully connected layers**: combine features for classification

```python
import tensorflow as tf
from tensorflow.keras import layers, models

def build_cnn(input_shape, num_classes):
    """Build a simple CNN for image classification."""
    model = models.Sequential([
        # First conv block
        layers.Conv2D(32, (3, 3), activation='relu', 
                      input_shape=input_shape),
        layers.MaxPooling2D((2, 2)),
        
        # Second conv block
        layers.Conv2D(64, (3, 3), activation='relu'),
        layers.MaxPooling2D((2, 2)),
        
        # Third conv block
        layers.Conv2D(128, (3, 3), activation='relu'),
        layers.MaxPooling2D((2, 2)),
        
        # Classification head
        layers.Flatten(),
        layers.Dense(128, activation='relu'),
        layers.Dropout(0.5),
        layers.Dense(num_classes, activation='softmax')
    ])
    
    model.compile(
        optimizer='adam',
        loss='sparse_categorical_crossentropy',
        metrics=['accuracy']
    )
    return model

# Example: 64x64 patches with 10 spectral bands, 5 classes
model = build_cnn((64, 64, 10), 5)
model.summary()
```

<div class="info-box tip">
  <strong>Key idea</strong>
  Deep learning excels when you have large labeled datasets and want to leverage spatial context. For smaller datasets typical in remote sensing, traditional ML with engineered features often performs equally well with less computational cost.
</div>

## Practical workflow

### Complete classification pipeline

```python
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split, GridSearchCV
from sklearn.ensemble import RandomForestClassifier
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import classification_report, confusion_matrix
import rasterio
from rasterio.mask import mask

def load_training_data(raster_path, shapefile_path):
    """Extract pixel values at training point locations."""
    # Load shapefile with training points
    import geopandas as gpd
    points = gpd.read_file(shapefile_path)
    
    # Extract values from raster
    with rasterio.open(raster_path) as src:
        # Sample raster at point locations
        coords = [(p.x, p.y) for p in points.geometry]
        values = list(src.sample(coords))
    
    X = np.array(values)
    y = points['class_id'].values
    
    return X, y

def train_classifier(X, y, feature_names=None):
    """Train and evaluate Random Forest classifier."""
    # Split data
    X_train, X_test, y_train, y_test = train_test_split(
        X, y, test_size=0.3, stratify=y, random_state=42
    )
    
    # Hyperparameter tuning
    param_grid = {
        'n_estimators': [100, 200, 300],
        'max_depth': [10, 20, 30, None],
        'min_samples_split': [2, 5, 10]
    }
    
    rf = RandomForestClassifier(random_state=42, n_jobs=-1)
    grid_search = GridSearchCV(rf, param_grid, cv=5, scoring='f1_macro')
    grid_search.fit(X_train, y_train)
    
    # Best model
    best_rf = grid_search.best_estimator_
    
    # Evaluate
    y_pred = best_rf.predict(X_test)
    print("Best parameters:", grid_search.best_params_)
    print("\nClassification Report:")
    print(classification_report(y_test, y_pred))
    
    # Feature importance
    if feature_names:
        importance = pd.DataFrame({
            'feature': feature_names,
            'importance': best_rf.feature_importances_
        }).sort_values('importance', ascending=False)
        print("\nTop 10 features:")
        print(importance.head(10))
    
    return best_rf

def classify_image(model, raster_path, output_path):
    """Apply trained model to entire image."""
    with rasterio.open(raster_path) as src:
        # Read all bands
        img = src.read()
        profile = src.profile
        
        # Reshape for prediction: (bands, rows, cols) -> (pixels, bands)
        n_bands, rows, cols = img.shape
        img_flat = img.reshape(n_bands, -1).T
        
        # Handle nodata
        valid_mask = ~np.any(np.isnan(img_flat), axis=1)
        
        # Predict
        predictions = np.zeros(img_flat.shape[0], dtype=np.uint8)
        predictions[valid_mask] = model.predict(img_flat[valid_mask])
        
        # Reshape back to image
        classified = predictions.reshape(rows, cols)
    
    # Write output
    profile.update(dtype=rasterio.uint8, count=1)
    with rasterio.open(output_path, 'w', **profile) as dst:
        dst.write(classified, 1)
    
    print(f"Classification saved to {output_path}")
```

### Handling large images

For images too large to fit in memory, process in tiles:

```python
def classify_in_tiles(model, raster_path, output_path, tile_size=1024):
    """Classify large raster in tiles."""
    with rasterio.open(raster_path) as src:
        profile = src.profile.copy()
        profile.update(dtype=rasterio.uint8, count=1)
        
        with rasterio.open(output_path, 'w', **profile) as dst:
            # Process in tiles
            for window in generate_windows(src.height, src.width, tile_size):
                # Read tile
                tile = src.read(window=window)
                
                # Classify
                n_bands = tile.shape[0]
                tile_flat = tile.reshape(n_bands, -1).T
                pred = model.predict(tile_flat)
                pred_tile = pred.reshape(window.height, window.width)
                
                # Write tile
                dst.write(pred_tile.astype(np.uint8), 1, window=window)
```

## Checklist

<ul class="checklist">
  <li>Define clear, mutually exclusive class definitions before collecting training data</li>
  <li>Collect sufficient training samples (50-100+ per class minimum)</li>
  <li>Ensure training data is spatially distributed across the study area</li>
  <li>Match training data dates to imagery acquisition dates</li>
  <li>Use spatial blocking for train/test splits to avoid inflated accuracy</li>
  <li>Scale features when using distance-based algorithms (k-NN, SVM)</li>
  <li>Tune hyperparameters using cross-validation</li>
  <li>Examine confusion matrix to identify problematic class pairs</li>
  <li>Report both overall accuracy and per-class metrics</li>
  <li>Document the classification workflow for reproducibility</li>
  <li>Validate results against independent reference data</li>
  <li>Consider class imbalance and apply appropriate strategies</li>
</ul>

## Exercises

<details>
<summary><strong>Exercise 1: Feature matrix construction</strong></summary>

You have 500 training pixels, each with 10 spectral bands. Write the dimensions of the feature matrix $\mathbf{X}$ and label vector $\mathbf{y}$ for a 5-class classification problem.

<strong>Solution:</strong>
The feature matrix $\mathbf{X}$ has dimensions $500 \times 10$ (500 samples, 10 features). The label vector $\mathbf{y}$ has dimensions $500 \times 1$ with values from the set $\{1, 2, 3, 4, 5\}$.
</details>

<details>
<summary><strong>Exercise 2: Bias-variance interpretation</strong></summary>

A Random Forest with 5 trees achieves 95% training accuracy but only 70% test accuracy. A Random Forest with 500 trees achieves 88% training accuracy and 85% test accuracy. Explain this in terms of bias and variance.

<strong>Solution:</strong>
The 5-tree model exhibits high variance (overfitting): it memorizes training data (95% training accuracy) but fails to generalize (70% test accuracy). The 500-tree model has lower variance due to ensemble averaging, reducing the gap between training and test accuracy. The slight decrease in training accuracy with more trees reflects regularization that improves generalization.
</details>

<details>
<summary><strong>Exercise 3: K-means objective</strong></summary>

For k-means clustering with $K=3$ clusters and 2D data, describe what the algorithm optimizes and how it converges.

<strong>Solution:</strong>
K-means minimizes total within-cluster variance: $\sum_{k=1}^{3} \sum_{\mathbf{x}_i \in C_k} \|\mathbf{x}_i - \boldsymbol{\mu}_k\|^2$. It alternates between assigning points to nearest centroids and updating centroids to cluster means. Convergence occurs when assignments stabilize (guaranteed in finite steps, though may find local minimum).
</details>

<details>
<summary><strong>Exercise 4: SVM margin calculation</strong></summary>

An SVM finds weight vector $\mathbf{w} = [3, 4]^T$. Calculate the margin width.

<strong>Solution:</strong>
Margin width is $\frac{2}{\|\mathbf{w}\|} = \frac{2}{\sqrt{3^2 + 4^2}} = \frac{2}{\sqrt{25}} = \frac{2}{5} = 0.4$ units.
</details>

<details>
<summary><strong>Exercise 5: RBF kernel behavior</strong></summary>

For the RBF kernel $K(\mathbf{x}, \mathbf{x}') = \exp(-\gamma\|\mathbf{x} - \mathbf{x}'\|^2)$, what happens as $\gamma \rightarrow \infty$? As $\gamma \rightarrow 0$?

<strong>Solution:</strong>
As $\gamma \rightarrow \infty$: kernel approaches 0 for any $\mathbf{x} \neq \mathbf{x}'$ and 1 for $\mathbf{x} = \mathbf{x}'$. Each point becomes its own support vector (extreme overfitting). As $\gamma \rightarrow 0$: kernel approaches 1 for all pairs, making all points equally similar (extreme underfitting, essentially linear).
</details>

<details>
<summary><strong>Exercise 6: Random Forest ensemble size</strong></summary>

A Random Forest with 100 trees achieves 82% accuracy. Estimate the accuracy with 400 trees.

<strong>Solution:</strong>
Accuracy typically improves with more trees but with diminishing returns. With 400 trees, expect modest improvement, perhaps 83-84%. The benefit of additional trees is variance reduction through averaging; bias remains unchanged. Beyond a certain point (often 200-500 trees), gains become negligible while computation increases.
</details>

<details>
<summary><strong>Exercise 7: NDVI calculation</strong></summary>

A pixel has reflectance values: Red = 0.08, NIR = 0.45. Calculate NDVI and interpret the value.

<strong>Solution:</strong>
$\text{NDVI} = \frac{0.45 - 0.08}{0.45 + 0.08} = \frac{0.37}{0.53} \approx 0.70$. This indicates healthy, dense vegetation. NDVI values above 0.6 typically correspond to vigorous green vegetation such as agricultural crops at peak growth or dense forest.
</details>

<details>
<summary><strong>Exercise 8: GLCM contrast interpretation</strong></summary>

Image A has GLCM contrast = 2.5. Image B has GLCM contrast = 45.3. Describe the visual difference.

<strong>Solution:</strong>
Image A (low contrast) has smooth texture with similar neighboring pixel values—perhaps a uniform crop field or calm water. Image B (high contrast) has rough texture with large differences between neighbors—perhaps urban areas with buildings and shadows, or heterogeneous forest with gaps.
</details>

<details>
<summary><strong>Exercise 9: Temporal feature design</strong></summary>

Design three temporal features that would help distinguish cotton from winter wheat in Uzbekistan.

<strong>Solution:</strong>
1. **Date of NDVI peak**: Wheat peaks in May; cotton peaks in August-September.
2. **NDVI in June**: Wheat is senescing (low NDVI); cotton is growing rapidly (rising NDVI).
3. **Winter NDVI (December-February)**: Wheat shows moderate green; cotton fields are bare soil.
</details>

<details>
<summary><strong>Exercise 10: Sample size estimation</strong></summary>

For a 7-class land cover classification with 200 samples per class, you use 70% for training. How many training samples per class?

<strong>Solution:</strong>
Training samples per class: $200 \times 0.70 = 140$ samples. Total training samples: $140 \times 7 = 980$ samples. This is a reasonable sample size for Random Forest classification.
</details>

<details>
<summary><strong>Exercise 11: Confusion matrix construction</strong></summary>

From 100 test samples of class Water, 85 are correctly classified, 10 are misclassified as Wetland, and 5 as Vegetation. Construct the Water row of the confusion matrix.

<strong>Solution:</strong>
| Actual | Pred Water | Pred Wetland | Pred Vegetation |
|--------|-----------|--------------|-----------------|
| Water | 85 | 10 | 5 |

Producer's accuracy for Water = 85/100 = 85%.
</details>

<details>
<summary><strong>Exercise 12: Producer's vs User's accuracy</strong></summary>

Class "Forest" has Producer's accuracy = 90% and User's accuracy = 60%. Interpret these values for a forest conservation project.

<strong>Solution:</strong>
Producer's accuracy (90%): 90% of actual forest pixels are correctly identified—good for detecting existing forest. User's accuracy (60%): only 60% of pixels classified as forest are actually forest—40% commission error. This means many non-forest pixels are incorrectly labeled as forest. For conservation, this could lead to overestimating forest area and including non-forest areas in protection zones.
</details>

<details>
<summary><strong>Exercise 13: F1-score calculation</strong></summary>

Class "Wetland" has precision (User's accuracy) = 0.75 and recall (Producer's accuracy) = 0.60. Calculate the F1-score.

<strong>Solution:</strong>
$F1 = 2 \times \frac{0.75 \times 0.60}{0.75 + 0.60} = 2 \times \frac{0.45}{1.35} = 2 \times 0.333 = 0.667$

The F1-score of 0.67 indicates moderate performance, balancing the trade-off between correctly identifying wetlands and avoiding false positives.
</details>

<details>
<summary><strong>Exercise 14: Kappa interpretation</strong></summary>

Classification A achieves overall accuracy = 85% and Kappa = 0.45. Classification B achieves overall accuracy = 75% and Kappa = 0.65. Which is better?

<strong>Solution:</strong>
Classification B is likely better despite lower overall accuracy. The higher Kappa (0.65 vs 0.45) indicates better agreement beyond chance. Classification A's high overall accuracy but low Kappa suggests it may be dominated by a large majority class that is easy to classify, while performing poorly on minority classes. Kappa accounts for class imbalance and is often more informative.
</details>

<details>
<summary><strong>Exercise 15: Cross-validation design</strong></summary>

Design a 5-fold cross-validation scheme for 1000 samples with 10 classes.

<strong>Solution:</strong>
Using stratified 5-fold CV: divide 1000 samples into 5 folds of 200 samples each, maintaining class proportions in each fold. Each iteration uses 4 folds (800 samples) for training and 1 fold (200 samples) for validation. Rotate through all 5 folds. Final accuracy is the mean across all 5 iterations, with standard deviation indicating stability.
</details>

<details>
<summary><strong>Exercise 16: Spatial autocorrelation problem</strong></summary>

Explain why random train/test splitting can inflate accuracy estimates for satellite imagery classification.

<strong>Solution:</strong>
Spatial autocorrelation means nearby pixels are similar. Random splitting places spatially adjacent pixels in both training and test sets. The model can then "memorize" local patterns and appear to generalize well, when actually it's just interpolating between nearby training samples. True generalization requires classifying new locations—spatial blocking ensures test areas are geographically separated from training areas.
</details>

<details>
<summary><strong>Exercise 17: Feature scaling necessity</strong></summary>

NDVI ranges from -1 to 1. Elevation ranges from 0 to 5000 meters. Explain why scaling matters for k-NN classification.

<strong>Solution:</strong>
K-NN uses Euclidean distance. Without scaling, elevation dominates: a 100m difference contributes 100 to the distance, while a 0.1 NDVI difference contributes only 0.1. The classifier would essentially ignore NDVI. After standardization (zero mean, unit variance), both features contribute equally to distance calculations, allowing the algorithm to properly weight both spectral and topographic information.
</details>

<details>
<summary><strong>Exercise 18: Class imbalance handling</strong></summary>

Your dataset has 5000 cropland samples but only 50 wetland samples. Describe two strategies to address this imbalance.

<strong>Solution:</strong>
1. **Class weighting**: assign higher weights to minority class samples in the loss function. In sklearn: `RandomForestClassifier(class_weight='balanced')`.
2. **Resampling**: either oversample wetland (duplicate minority samples) or undersample cropland (randomly remove majority samples). SMOTE generates synthetic minority samples by interpolating between existing ones.
</details>

<details>
<summary><strong>Exercise 19: Neural network architecture</strong></summary>

Design a neural network for pixel-based classification with 12 spectral features and 6 output classes. Specify layers and activation functions.

<strong>Solution:</strong>
```
Input: 12 features
Hidden Layer 1: 64 neurons, ReLU activation
Hidden Layer 2: 32 neurons, ReLU activation
Output Layer: 6 neurons, Softmax activation
```
This gives approximately $12 \times 64 + 64 \times 32 + 32 \times 6 = 768 + 2048 + 192 = 3008$ parameters (plus biases). Softmax ensures outputs sum to 1 for probability interpretation.
</details>

<details>
<summary><strong>Exercise 20: CNN filter interpretation</strong></summary>

A 3×3 convolutional filter produces high activations on edges between irrigated fields and desert. What might the filter weights look like?

<strong>Solution:</strong>
The filter likely learned an edge detector, with positive weights on one side and negative on the other, such as:
```
[-1 -1 -1]
[ 0  0  0]
[ 1  1  1]
```
This detects horizontal edges where pixel values transition from low (desert) to high (vegetation) or vice versa. The filter responds strongly where irrigated field boundaries meet desert.
</details>

<details>
<summary><strong>Exercise 21: Central Asia crop phenology</strong></summary>

Describe the expected NDVI time series for rice paddies in the Fergana Valley from March to October.

<strong>Solution:</strong>
March-April: Low NDVI (bare soil, field preparation). May: Very low NDVI with standing water (flooding for transplanting). June-July: Rapidly rising NDVI as rice grows. August: Peak NDVI (0.7-0.8) at maximum tillering/heading. September: Declining NDVI as rice matures and yellows. October: Low NDVI after harvest. The distinctive spring flooding signature distinguishes rice from other crops.
</details>

<details>
<summary><strong>Exercise 22: Multi-temporal composite</strong></summary>

You have monthly Sentinel-2 images from April to September (6 dates). Design a feature stack for crop classification.

<strong>Solution:</strong>
For each date, include: B2, B3, B4, B8, B11, B12, NDVI, EVI (8 features × 6 dates = 48 features). Add temporal statistics across dates: max NDVI, mean NDVI, std NDVI, date of max NDVI (4 features). Total: 52 features. This captures both instantaneous spectral signatures and seasonal dynamics critical for crop type discrimination.
</details>

<details>
<summary><strong>Exercise 23: Accuracy vs area estimation</strong></summary>

Your classification has 90% overall accuracy. Does this mean area estimates are accurate to within 10%? Explain.

<strong>Solution:</strong>
No. Area estimates depend on the balance of omission and commission errors, not just overall accuracy. If a class has 80% Producer's accuracy (20% omission) and 95% User's accuracy (5% commission), the net effect on area is: estimated area ≈ true area × (commission rate / omission rate). Errors can partially cancel or compound. Proper area estimation requires adjusting for the confusion matrix structure.
</details>

<details>
<summary><strong>Exercise 24: Hyperparameter tuning</strong></summary>

For Random Forest, max_depth=5 gives 75% accuracy; max_depth=50 gives 82% training but 72% test accuracy. Suggest an optimal value.

<strong>Solution:</strong>
max_depth=5 underfits (high bias); max_depth=50 overfits (high variance). The optimal is somewhere between. Try max_depth values of 10, 15, 20, 25, 30 using cross-validation. Select the depth that maximizes validation accuracy. Given the patterns described, max_depth around 15-25 would likely work well, balancing model complexity with generalization.
</details>

<details>
<summary><strong>Exercise 25: Transfer learning concept</strong></summary>

You have a CNN trained on 100,000 labeled samples from Kazakhstan. How could you use it for a new project in Tajikistan with only 500 labeled samples?

<strong>Solution:</strong>
Transfer learning: use the Kazakhstan-trained model's convolutional layers (which learned general features like edges, textures, vegetation patterns) as feature extractors. Remove the final classification layers and add new layers for Tajikistan classes. Train only the new layers using the 500 Tajikistan samples, keeping transferred layers frozen or fine-tuning with a low learning rate. This leverages learned features while adapting to new geography.
</details>

<details>
<summary><strong>Exercise 26: Mixed pixel problem</strong></summary>

A 30m Landsat pixel covers 900 m². The pixel contains 400 m² cotton, 300 m² wheat, and 200 m² bare soil. What will hard classification produce? What would be better?

<strong>Solution:</strong>
Hard classification assigns one label. With cotton as plurality (44%), the pixel likely classifies as cotton, ignoring wheat and bare soil. This introduces error in area estimation. Better approaches: (1) Sub-pixel classification estimates fractions: 0.44 cotton, 0.33 wheat, 0.22 bare soil. (2) Use higher resolution imagery (Sentinel-2 at 10m or Planet at 3m) where pixels are more likely to be pure.
</details>

<details>
<summary><strong>Exercise 27: Ensemble combination</strong></summary>

Random Forest achieves 82% accuracy; SVM achieves 80%; neural network achieves 81%. Design an ensemble combining all three.

<strong>Solution:</strong>
Simple voting ensemble: for each pixel, collect predictions from all three models and use majority vote. If all three disagree, use the prediction from the most confident model (highest class probability). Alternative: weighted voting based on per-class performance—if SVM performs best for Water class, weight its Water predictions higher. Soft voting averages predicted probabilities before taking argmax.
</details>

<details>
<summary><strong>Exercise 28: Computational efficiency</strong></summary>

You need to classify a 10,000 × 10,000 pixel image with 10 bands using Random Forest. Estimate memory requirements and suggest optimization strategies.

<strong>Solution:</strong>
Image size: 10,000 × 10,000 × 10 bands × 4 bytes (float32) = 4 GB. For classification, must reshape to (100 million pixels × 10 features). Strategies: (1) Process in tiles (1024×1024) to fit in memory. (2) Use float16 instead of float32 (halves memory). (3) Use memory-mapped files. (4) Classify on GPU if available. (5) Parallelize across CPU cores for prediction.
</details>

<details>
<summary><strong>Exercise 29: Error analysis workflow</strong></summary>

Your classification confuses "grassland" and "sparse shrubland" frequently. Design a workflow to improve discrimination.

<strong>Solution:</strong>
1. Examine confused samples: view imagery at training locations to verify labels are correct.
2. Add discriminative features: texture (shrubs create rougher texture), seasonal patterns (shrubs may stay greener in dry season), SWIR bands (woody vs herbaceous).
3. Consider merging classes if truly inseparable spectrally.
4. Collect additional training samples in transition zones.
5. Use ancillary data: elevation, slope (shrubs may prefer certain topographic positions).
6. Retrain and evaluate improvement in the specific grassland-shrubland confusion.
</details>

<details>
<summary><strong>Exercise 30: Complete pipeline design</strong></summary>

Design an end-to-end machine learning pipeline for mapping irrigated agriculture in the Amu Darya basin using Sentinel-2 imagery.

<strong>Solution:</strong>
1. **Data collection**: Download cloud-free Sentinel-2 images for key phenological dates (April, June, August).
2. **Preprocessing**: Atmospheric correction, cloud masking, resampling 20m bands to 10m.
3. **Feature engineering**: Spectral bands, NDVI, EVI, NDWI, NDBI for each date; temporal statistics; GLCM texture from B8.
4. **Training data**: Digitize 100+ samples per class from Google Earth: irrigated crops, rainfed, water, desert, urban. Use spatial stratification.
5. **Model training**: Random Forest with 300 trees, spatial cross-validation, hyperparameter tuning via grid search.
6. **Classification**: Apply to full basin, process in tiles.
7. **Accuracy assessment**: Independent validation samples, confusion matrix, per-class F1-scores, Kappa.
8. **Post-processing**: Majority filter to remove salt-and-pepper noise, mask known water bodies.
9. **Output**: GeoTIFF classification map, accuracy report, area statistics by administrative unit.
</details>
