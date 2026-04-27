# Data

The M5 Walmart Hierarchical Sales dataset is **not included** in this repository.

Kaggle requires each user to accept the competition rules before downloading. Please follow the steps below to obtain the data.

---

## Download Instructions

1. Create a Kaggle account at [kaggle.com](https://www.kaggle.com)
2. Accept the competition rules at: [kaggle.com/competitions/m5-forecasting-accuracy/data](https://www.kaggle.com/competitions/m5-forecasting-accuracy/data)
3. Install the Kaggle API:
   ```bash
   pip install kaggle
   ```
4. Download your API token from [kaggle.com/settings](https://www.kaggle.com/settings) under API → Create New Token. This downloads a `kaggle.json` file.
5. Place the token file and set permissions:
   ```bash
   mv kaggle.json ~/.kaggle/kaggle.json
   chmod 600 ~/.kaggle/kaggle.json
   ```
6. Download and unzip the data:
   ```bash
   kaggle competitions download -c m5-forecasting-accuracy -p data/
   cd data && unzip m5-forecasting-accuracy.zip
   ```

---

## Files Required

| File | Size | Description |
|---|---|---|
| `sales_train_validation.csv` | ~115 MB | Daily unit sales for 30,490 SKU-store combinations across 10 stores and 1,913 days |
| `calendar.csv` | ~100 KB | Maps day indices (d_1 to d_1913) to actual dates; includes event and SNAP flags |
| `sell_prices.csv` | ~194 MB | Weekly sell prices per item per store |

The files `sales_train_evaluation.csv` and `sample_submission.csv` are not used in this analysis.

---

## Data Path Configuration

All notebooks expect the data at the path specified by `DATA_DIR` in Block 1. Update this to match your local setup before running:

```python
DATA_DIR = r"your/local/path/to/data"
```

---

## Dataset Reference

Makridakis, S., Spiliotis, E., & Assimakopoulos, V. (2022). M5 accuracy competition: Results, findings, and conclusions. *International Journal of Forecasting*, 38(4), 1346-1364.
