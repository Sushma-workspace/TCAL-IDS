# TCAL-IDS: Code to Paper Map

Paper: *Temporal Convolution and Attention-Guided LightGBM Framework for Multiclass Intrusion Detection in Industrial Internet of Things Networks*. There is one Kaggle notebook per dataset, and all three share the same pipeline.

| Notebook | Dataset | Classes |
|---|---|---|
| `sushma-bot-iot.ipynb` | NF-BoT-IoT-v2 | 5 |
| `sushma-iotid20.ipynb` | IoTID20 | 5 |
| `sushma-ton-iot.ipynb` | NF-ToN-IoT-v2 | 10 |

## Methodology

Cell numbers count from 0.

| Paper section | BoT / IoTID20 / ToN cell | What the code does |
|---|---|---|
| Preprocessing: ingestion and cleaning | 4-7 / 2-8 / 19-26 | Reads in chunks, removes bad rows, encodes categories. IoTID20 is sorted by Timestamp; the other two keep file order. |
| Preprocessing: folds | 11 / 12 / 44 | `stratified_block_edges`: 5 contiguous blocks. |
| Eq. 1, outlier clipping, Feature Selection | 12 / 13 / 45 | `fit_fold`: 1st-99th percentile clipping, min-max scaling, variance, correlation and SHAP selection, all fitted on the training folds only. |
| Temporal sequencing, class balancing | 13 / 14 / 46 | `block_starts`: W=10, S=5, 9-record embargo. `balance`: SMOTE-Tomek on training windows only. |
| Table I | 16 / 17 / 49 | `TABLE_I`: the model and training settings. |
| TCN, Eq. 2 | 17 / 18 / 50 | `TCNBlock` |
| Multi-Head Self-Attention, Eqs. 5-6 | 18 / 19 / 51 | `TCN_MHSA`: attention and pooling. |
| Eq. 7 | 19 / 20 / 52 | `FocalLoss` |
| Algorithm 1, Stages 1 and 2 | 20 / 21 / 53 | `train_stage1`, `fit_lgbm`, `run_tcal` |

## Results

| Paper item | BoT / IoTID20 / ToN cell | Output |
|---|---|---|
| Tables III and V: 20 seeds, 5 folds, mean, SD, 95% CI (Table II metrics) | 22-23 / 23-24 / 55-56 | `runs/`, `summary/` |
| Table IV: LightGBM only; TCN + MHSA + Softmax; full model | 20, 22 / 21, 23 / 53, 55 | `runs/`, `config` and `head` columns |
| Table VII: time, GFLOPs, parameters, latency | 20, 26 / 21, 27 / 53, 59 | `timing/`, `profile/` |
| Table VIII: paired t-test, Holm, Cohen's dz | 24 / 25 / 57 | `stats/` |
| Figs. 2-4: training curves | 28 / 29 / 61 | `training_curves.png` in the dataset folder |
| Figs. 5-7: confusion matrices | 27 / 28 / 60 | Shown in the notebook, from the last fold only |

Added for the reviewer response:

| Response item | BoT / IoTID20 / ToN cell | Output |
|---|---|---|
| Table X-J: W/S sensitivity | 25 / 26 / 58 | `sensitivity/` |
| Table X-L: SHAP rankings per fold | 12 / 13 / 45 | `shap/`, `features/` |
| 30-trial random search | 21 / 22 / 54 | `search/` |


- **Tables III and VIII, comparison rows:**
- **Table IV:** the no-attention, no-SMOTE-Tomek, no-focal-loss and no-SHAP variants.
- **Figs. 5-7:** the pooled confusion matrices the paper reports across folds.
- **Table VI:** the k=3 to k=7 sweep, because k is fixed at 5.
- **Table VII:** memory usage.
- **Table IX and Fig. 1:** literature values and a drawn diagram.
