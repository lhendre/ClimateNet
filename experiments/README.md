
# Experiments

Here we report code used for some of the experiments we conducted.

## Ablation

We ran ablation studies for the core ClimateNet channels on which the baseline model was trained on.

Syntax:
```
$ python3 example-ablation-XYZ.py

<WYZ>: channel to be removed during training.
```


## Data augmentation

We experimented with data augmentation by translating each frame in the dataset east-west by a random longitude increment (between -180ºW and +180ºW).

Reproduction: 
```
Replace `/climatenet/utils/data.py` with the `data.py` file in this directory.
```

The constructor for the ClimateDataLabeled class now shifts each sample of the dataset by a random amount at load time.


## Original Baseline

Original weights for the baseline model published along the dataset and paper, used to guide our experimentations.

Source: https://portal.nersc.gov/project/ClimateNet/climatenet_new/


