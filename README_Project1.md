# A Multi-Seed Ablation Study of Pretraining, Augmentation and CLAHE for Small Medical Image Classifiers

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/YOUR_GITHUB_USERNAME/YOUR_REPO_NAME/blob/main/Project1_Ablation_Study.ipynb)

> A small, reproducible benchmark study on public low-resolution datasets. **Not validated for clinical use.**

## 1. Research question

Do ImageNet pretraining, data augmentation and CLAHE contrast enhancement give a *real* improvement on small medical image classifiers, or are the differences within the noise caused by random seeds?

## 2. Data

Both datasets come from [MedMNIST](https://medmnist.com/). Images are the **64×64** versions with the official train/validation/test split (I did not re-split the data).

| Dataset | Modality | Task | Classes | Train / Val / Test images |
|---|---|---|---|---|
| PneumoniaMNIST | Chest X-ray | Normal vs pneumonia | 2 | 4,708 / 524 / 624 |
| RetinaMNIST | Retinal fundus photo | Diabetic retinopathy severity | 5 (ordinal, 0 to 4) | 1,080 / 120 / 400 |

Data checks: [result of notebook Step 3, for example "0 identical images between splits"].

## 3. Method

- **Model:** ResNet-18 (torchvision) with the last layer replaced. Same hyperparameters for every run: AdamW, learning rate 3e-4, weight decay 1e-4, cosine schedule, batch size 64, 15 epochs (PneumoniaMNIST) and 30 epochs (RetinaMNIST).
- **Three on/off ingredients, giving 8 settings:**
  1. ImageNet-pretrained weights vs random initialisation
  2. Augmentation (random flip, rotation up to 10°, shift up to 5%, brightness and contrast up to 20%) vs none
  3. CLAHE (clip limit 2.0, 8×8 tiles; lightness channel for colour images) vs none
- **Repetitions:** every setting was trained with 5 different random seeds on each dataset (80 runs in total).
- **No test-set leakage:** the epoch with the lowest validation loss is kept, and the test set is used once per run. The reference model for later projects is chosen by validation score: the setting with the best mean validation main score was `pretrained+aug` for PneumoniaMNIST and `pretrained+aug+clahe` for RetinaMNIST (these are the models used in Projects 2 and 3).
- **Scores:** accuracy, macro-F1, quadratic weighted kappa (QWK) and AUC. Main score: AUC for PneumoniaMNIST, QWK for RetinaMNIST. A "dumb" majority-class baseline is reported for reference.
- **Statistics:** mean ± 95% confidence interval across seeds (t-distribution). The effect of an ingredient is computed per seed as (mean score of the 4 settings with it ON) minus (mean score of the 4 settings with it OFF); then I report the mean and 95% CI of these differences across seeds.

## 4. Results

All numbers are on the **test set**: mean ± 95% confidence interval over the 5 seeds.

### PneumoniaMNIST

Majority-class baseline (always predicts class 1): accuracy 0.625, AUC 0.500, QWK 0.000.

**All scores per setting** (sorted by the main score, AUC)

| setting | seeds | accuracy | macro-F1 | QWK | AUC |
|---|---|---|---|---|---|
| pretrained+aug+clahe | 5 | 0.915 ± 0.018 | 0.905 ± 0.022 | 0.811 ± 0.043 | 0.985 ± 0.007 |
| pretrained+aug | 5 | 0.913 ± 0.016 | 0.903 ± 0.019 | 0.807 ± 0.037 | 0.984 ± 0.006 |
| pretrained+clahe | 5 | 0.891 ± 0.023 | 0.875 ± 0.028 | 0.753 ± 0.055 | 0.977 ± 0.006 |
| scratch+aug+clahe | 5 | 0.891 ± 0.020 | 0.876 ± 0.025 | 0.754 ± 0.048 | 0.972 ± 0.007 |
| pretrained | 5 | 0.878 ± 0.030 | 0.858 ± 0.040 | 0.722 ± 0.074 | 0.970 ± 0.014 |
| scratch+aug | 5 | 0.879 ± 0.020 | 0.861 ± 0.025 | 0.725 ± 0.047 | 0.965 ± 0.009 |
| scratch+clahe | 5 | 0.866 ± 0.010 | 0.844 ± 0.014 | 0.694 ± 0.025 | 0.961 ± 0.008 |
| scratch | 5 | 0.856 ± 0.017 | 0.831 ± 0.023 | 0.669 ± 0.043 | 0.951 ± 0.007 |

**Effect of each ingredient on test AUC** (ON minus OFF, mean and 95% CI over the 5 seeds)

| ingredient | effect | 95% CI half-width | CI low | CI high | seeds used | verdict |
|---|---|---|---|---|---|---|
| pretrained | 0.016 | 0.006 | 0.011 | 0.022 | 5 | clear improvement |
| augment | 0.012 | 0.006 | 0.005 | 0.018 | 5 | clear improvement |
| clahe | 0.006 | 0.003 | 0.003 | 0.009 | 5 | clear improvement |

### RetinaMNIST

Majority-class baseline (always predicts class 0): accuracy 0.435, AUC 0.500, QWK 0.000.

**All scores per setting** (sorted by the main score, QWK)

| setting | seeds | accuracy | macro-F1 | QWK | AUC |
|---|---|---|---|---|---|
| pretrained+aug+clahe | 5 | 0.553 ± 0.014 | 0.404 ± 0.036 | 0.579 ± 0.054 | 0.790 ± 0.011 |
| pretrained+aug | 5 | 0.527 ± 0.025 | 0.386 ± 0.030 | 0.552 ± 0.077 | 0.759 ± 0.020 |
| pretrained+clahe | 5 | 0.525 ± 0.029 | 0.399 ± 0.017 | 0.537 ± 0.044 | 0.765 ± 0.021 |
| scratch+aug | 5 | 0.531 ± 0.020 | 0.325 ± 0.031 | 0.519 ± 0.063 | 0.736 ± 0.008 |
| pretrained | 5 | 0.501 ± 0.027 | 0.344 ± 0.045 | 0.498 ± 0.063 | 0.747 ± 0.007 |
| scratch+aug+clahe | 5 | 0.509 ± 0.006 | 0.306 ± 0.037 | 0.429 ± 0.079 | 0.745 ± 0.019 |
| scratch | 5 | 0.488 ± 0.053 | 0.265 ± 0.122 | 0.303 ± 0.246 | 0.715 ± 0.011 |
| scratch+clahe | 5 | 0.481 ± 0.031 | 0.243 ± 0.076 | 0.201 ± 0.128 | 0.728 ± 0.021 |

**Effect of each ingredient on test QWK** (ON minus OFF, mean and 95% CI over the 5 seeds)

| ingredient | effect | 95% CI half-width | CI low | CI high | seeds used | verdict |
|---|---|---|---|---|---|---|
| pretrained | 0.179 | 0.124 | 0.055 | 0.302 | 5 | clear improvement |
| augment | 0.135 | 0.092 | 0.043 | 0.227 | 5 | clear improvement |
| clahe | -0.031 | 0.053 | -0.084 | 0.021 | 5 | cannot tell (CI includes 0) |

![PneumoniaMNIST: every setting, every seed](figures/pneumoniamnist_config_scores.png)
![PneumoniaMNIST: effect of each ingredient](figures/pneumoniamnist_effects.png)
![RetinaMNIST: every setting, every seed](figures/retinamnist_config_scores.png)
![RetinaMNIST: effect of each ingredient](figures/retinamnist_effects.png)

## 5. Key findings

- On RetinaMNIST, pretraining changed test QWK by +0.179 (95% CI 0.055 to 0.302) and augmentation by +0.135 (0.043 to 0.227), the two clear effects, while CLAHE did not help (−0.031, CI −0.084 to +0.021, which includes 0).
- On PneumoniaMNIST all three ingredients gave clear but small gains in test AUC (pretraining +0.016, augmentation +0.012, CLAHE +0.006; all CIs above 0), and the best setting, `pretrained+aug+clahe`, reached AUC 0.985 ± 0.007 against 0.951 ± 0.007 for plain `scratch` (no pretraining, augmentation or CLAHE).
- Averaged over the 8 settings, the best and worst seed of the same setting differed by 0.016 AUC on PneumoniaMNIST and by 0.195 QWK on RetinaMNIST, so single-run differences smaller than this are not reliable.
- The noisiest case was RetinaMNIST trained from scratch, where test QWK ranged from 0.008 to 0.502 over the 5 seeds.

## 6. Limitations

- Low-resolution images: fine details such as small retinal lesions are largely lost, so results may differ at full resolution.
- The test sets are small (624 and 400 images), so scores are uncertain even for a fixed model.
- Only exact duplicate images were checked. Near-duplicates and patient-level separation could not be verified (MedMNIST provides no patient IDs).
- The same hyperparameters were used for every setting; each setting might do better with its own tuning.
- One architecture (ResNet-18) and 5 seeds only; interactions between ingredients were not analysed in depth.
- The epoch was chosen by validation loss. On RetinaMNIST this selected very early epochs (median best epoch 2 of 30; 36 of the 40 runs chose epoch 5 or earlier). On PneumoniaMNIST the validation scores were much higher than the test scores (mean accuracy 0.979 on validation against 0.886 on test over the 40 runs), so the validation set appeared easier than the test set.
- Not clinically validated and not suitable for medical decisions.

## 7. How to reproduce

1. Click the **Open in Colab** badge above.
2. Runtime, Change runtime type, choose a GPU.
3. In the settings cell set `QUICK_TEST = False`, then Runtime, Run all (in my run the 80 training runs took about 31 minutes on a Tesla T4 GPU, plus time for the data download and evaluation).

The exact library versions and settings of my run are in `environment.json`. To run locally: `pip install -r requirements.txt`.

## 8. Repository contents

| File | What it is |
|---|---|
| `Project1_Ablation_Study.ipynb` | Full code and explanations, with the outputs of my run |
| `results.csv` | One row per training run (validation and test scores) |
| `results_tables.md`, `summary_*.csv`, `effects_*.csv` | Summary tables |
| `figures/` | Plots used in this README |
| `environment.json` | Software versions and settings |
| `requirements.txt` | Python packages |

## 9. References and acknowledgements

- J. Yang et al., *MedMNIST v2: A large-scale lightweight benchmark for 2D and 3D biomedical image classification*, Scientific Data, 2023.
- D. Kermany et al., *Identifying Medical Diagnoses and Treatable Diseases by Image-Based Deep Learning*, Cell, 2018 (source of PneumoniaMNIST).
- R. Liu et al., *DeepDRiD: Diabetic Retinopathy, Grading and Image Quality Estimation Challenge*, Patterns, 2022 (source of RetinaMNIST).
- K. He et al., *Deep Residual Learning for Image Recognition*, CVPR, 2016 (ResNet).
- K. Zuiderveld, *Contrast Limited Adaptive Histogram Equalization*, Graphics Gems IV, 1994 (CLAHE).
- Please also check the license terms of each dataset on the MedMNIST website.

## 10. Author

Nisha Pandey · [your email] · [link to your CV or LinkedIn]
