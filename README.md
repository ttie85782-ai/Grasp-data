# Robotic Grasping Dataset (Privacy-Preserved)

## Overview

This dataset contains **9,126 records** of robotic grasping experiments in a logistics/warehouse environment. The data includes object dimensions, grasp poses, suction cup configurations, and success labels for grasp quality prediction research.

Due to **customer privacy and business confidentiality**, all absolute physical measurements have been obfuscated while preserving statistical relationships and research usability.

---
<!-- 

## Dataset Statistics

| Property | Value |
|----------|-------|
| Total samples | 9,126 |
| Features | 19 columns |
| Success rate | 78.3% (as provided) |
| Arm types | 18 suction cups / 20 suction cups |

---

## Column Description

| Column | Description | Data Type | Privacy Handling |
|--------|-------------|-----------|------------------|
| `Length` | Object length (normalized) | Float | Standardized → [0,10] + noise |
| `Width` | Object width (normalized) | Float | Standardized → [0,10] + noise |
| `Height` | Object height (normalized) | Float | Standardized → [0,10] + noise |
| `Detection_score` | Detection confidence | Float (0-1) | Added tiny noise (±0.01) |
| `Normal_x` | Grasp normal vector X | Float | 3% noise + renormalized |
| `Normal_y` | Grasp normal vector Y | Float | 3% noise + renormalized |
| `Normal_z` | Grasp normal vector Z | Float | 3% noise + renormalized |
| `Normal_d` | Plane distance parameter | Float | Standardized → [0,5] + noise |
| `Center_x` | Object center X (normalized) | Float | MinMax [0,100] + translation + noise |
| `Center_y` | Object center Y (normalized) | Float | MinMax [0,100] + translation + noise |
| `Center_z` | Object center Z (normalized) | Float | MinMax [0,100] + translation + noise |
| `Grasp_x` | Grasp point X (normalized) | Float | MinMax [0,100] + translation + noise |
| `Grasp_y` | Grasp point Y (normalized) | Float | MinMax [0,100] + translation + noise |
| `Grasp_z` | Grasp point Z (normalized) | Float | MinMax [0,100] + translation + noise |
| `Cup_state` | Suction cup state (0/1/2) | Integer | Preserved (no privacy risk) |
| `Weight` | Object weight (normalized) | Float | Standardized → [0,10] + noise |
| `Type_arm` | Suction cup count (18 or 20) | Integer | Preserved (hardware config) |
| `Grasp_time` | Relative grasp duration | Float | Standardized (relative time) |
| `Success` | Grasp success label (0/1) | Integer | Preserved (target variable) |

---
-->

## Privacy Preservation Methods

### 1. Geometric Dimensions & Weight (`Length`, `Width`, `Height`, `Weight`)

- **Original units**: Meters (length) / Kilograms (weight)
- **Applied method**: Z-score standardization → min-max scaling to [0,10] → added Gaussian noise (σ = 2% of scaled std)
- **Effect**: Absolute physical units are irrecoverable. Relative size/weight relationships are preserved.

### 2. Spatial Coordinates (`Center_*`, `Grasp_*`)

- **Original units**: Meters in global coordinate system
- **Applied method**: Min-max normalization to [0,100] → random translation (-20 to +20) → added Gaussian noise (σ = 0.5)
- **Effect**: Global origin and absolute positions are lost. **Relative distances and spatial patterns** (e.g., grasp point offset from center) are preserved.

### 3. Normal Vectors (`Normal_x`, `Normal_y`, `Normal_z`)

- **Applied method**: Added isotropic Gaussian noise (σ = 0.03) → re-normalized to unit length
- **Effect**: Average angular deviation < 3°. Directional information is preserved for grasp orientation analysis.

### 4. Plane Distance (`Normal_d`)

- **Applied method**: Standardization → scaling to [0,5] → added tiny noise
- **Effect**: Absolute distance values are hidden; relative comparisons remain valid.

### 5. Detection Score

- **Applied method**: Added Gaussian noise (σ = 0.01) → clipped to [0,1]
- **Effect**: Individual detection confidence values are obfuscated; distribution shape is preserved.

### 6. Temporal Information (`Grasp_time`)

- **Original**: Absolute timestamps or raw durations
- **Applied method**: Converted to relative seconds from first record → standardized
- **Effect**: Absolute time information is removed; relative ordering and timing patterns are preserved.

### 7. Preserved Columns (No Privacy Risk)

- `Cup_state`: Suction cup state code (0/1/2)
- `Type_arm`: Hardware configuration (18 or 20 suction cups)
- `Success`: Research target variable (0 = fail, 1 = success)

---

## Usage Examples

### Load dataset
```python
import numpy as np
data = np.load('npy_output/grasp_data_20250721.npy')
print(f"Data shape: {data.shape}")
