# Results tables (auto-generated)

Run: `full_64px`. Values are mean ± 95% CI over seeds, on the TEST set.


## pneumoniamnist

Dumb baseline (always predict class 1): accuracy 0.625, AUC 0.500, QWK 0.000


### All scores per setting

| setting | seeds | accuracy | macro_f1 | qwk | auc |
|---|---|---|---|---|---|
| pretrained+aug+clahe | 5 | 0.915 ± 0.018 | 0.905 ± 0.022 | 0.811 ± 0.043 | 0.985 ± 0.007 |
| pretrained+aug | 5 | 0.913 ± 0.016 | 0.903 ± 0.019 | 0.807 ± 0.037 | 0.984 ± 0.006 |
| pretrained+clahe | 5 | 0.891 ± 0.023 | 0.875 ± 0.028 | 0.753 ± 0.055 | 0.977 ± 0.006 |
| scratch+aug+clahe | 5 | 0.891 ± 0.020 | 0.876 ± 0.025 | 0.754 ± 0.048 | 0.972 ± 0.007 |
| pretrained | 5 | 0.878 ± 0.030 | 0.858 ± 0.040 | 0.722 ± 0.074 | 0.970 ± 0.014 |
| scratch+aug | 5 | 0.879 ± 0.020 | 0.861 ± 0.025 | 0.725 ± 0.047 | 0.965 ± 0.009 |
| scratch+clahe | 5 | 0.866 ± 0.010 | 0.844 ± 0.014 | 0.694 ± 0.025 | 0.961 ± 0.008 |
| scratch | 5 | 0.856 ± 0.017 | 0.831 ± 0.023 | 0.669 ± 0.043 | 0.951 ± 0.007 |


### Effect of each ingredient on test AUC (ON minus OFF)

| ingredient | effect | ci95_half_width | ci_low | ci_high | seeds_used | verdict |
|---|---|---|---|---|---|---|
| pretrained | 0.016 | 0.006 | 0.011 | 0.022 | 5 | clear improvement |
| augment | 0.012 | 0.006 | 0.005 | 0.018 | 5 | clear improvement |
| clahe | 0.006 | 0.003 | 0.003 | 0.009 | 5 | clear improvement |



## retinamnist

Dumb baseline (always predict class 0): accuracy 0.435, AUC 0.500, QWK 0.000


### All scores per setting

| setting | seeds | accuracy | macro_f1 | qwk | auc |
|---|---|---|---|---|---|
| pretrained+aug+clahe | 5 | 0.553 ± 0.014 | 0.404 ± 0.036 | 0.579 ± 0.054 | 0.790 ± 0.011 |
| pretrained+aug | 5 | 0.527 ± 0.025 | 0.386 ± 0.030 | 0.552 ± 0.077 | 0.759 ± 0.020 |
| pretrained+clahe | 5 | 0.525 ± 0.029 | 0.399 ± 0.017 | 0.537 ± 0.044 | 0.765 ± 0.021 |
| scratch+aug | 5 | 0.531 ± 0.020 | 0.325 ± 0.031 | 0.519 ± 0.063 | 0.736 ± 0.008 |
| pretrained | 5 | 0.501 ± 0.027 | 0.344 ± 0.045 | 0.498 ± 0.063 | 0.747 ± 0.007 |
| scratch+aug+clahe | 5 | 0.509 ± 0.006 | 0.306 ± 0.037 | 0.429 ± 0.079 | 0.745 ± 0.019 |
| scratch | 5 | 0.488 ± 0.053 | 0.265 ± 0.122 | 0.303 ± 0.246 | 0.715 ± 0.011 |
| scratch+clahe | 5 | 0.481 ± 0.031 | 0.243 ± 0.076 | 0.201 ± 0.128 | 0.728 ± 0.021 |


### Effect of each ingredient on test QWK (ON minus OFF)

| ingredient | effect | ci95_half_width | ci_low | ci_high | seeds_used | verdict |
|---|---|---|---|---|---|---|
| pretrained | 0.179 | 0.124 | 0.055 | 0.302 | 5 | clear improvement |
| augment | 0.135 | 0.092 | 0.043 | 0.227 | 5 | clear improvement |
| clahe | -0.031 | 0.053 | -0.084 | 0.021 | 5 | cannot tell (CI includes 0) |

