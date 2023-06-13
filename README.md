# CS231 Semantic Segmentation of Extreme Climate Events

Tropical cyclones (TCs), also known as hurricanes, typhoons or tropical storms, are the most destructive type of extreme weather events and have caused $1.1 trillion in damage and 6,697 deaths since 1980 in the US alone.

In this project, we apply the light-weight CGNet context guided computer vision architecture to semantic segmentation for the identification of tropical cyclones in climate simulations data.
<!--
https://user-images.githubusercontent.com/3156495/207959086-e1a2e246-b863-4420-8275-b76c4877ba6d.mp4 -->

## Getting started

**1. Data**
- Run the `download_climatenet.ipynb` notebook in the `data/` folder to download ClimateNet.
- Optionally: split the train set in train (1996-2007) and val (2008-2010).

Directions for new IBTrACS/ERA5 data coming soon

**2. Model:**
- We provide `train_eval.py`, a script that trains a model on the chosen parameters (in `config.json`).  Different branches will add in temporal segmentation along with LSTM
Syntax:
```
$ train_eval.py --model_path <model_path> -data_path <data_path>
$ train_eval.py -m <model_path> -d <data_path>

<model_path>: path to folder containing config.json describing the model
<data_path>: path folder containing dataset with corresponding layers

Example: train_eval.py -m '/model/1. Baseline' -d /data/ClimateNet/
```

## Directory structure

```
\ train_eval.py -> main script: train a model on ClimateNet and evaluate it
\ README.MD -> this document
│
├─ climatenet/ -> updated CGNet implementation (pyTorch model definition)
│    └─ utils/ -> utils for CGNet implementation (loss functions, data loading)
│
├─ data/
│    └─ data_exploration_climatenet.ipynb -> explore ClimateNet dataset
│    └─ download_climatenet.ipynb -> download the ClimateNet dataset
│    └─ feature_engineering.ipynb -> generate new engineered features
│    └─ train_history.ipynb -> train model, save history, display metrics
│    └─ visualize_predictions.ipynb -> load trained models and plot predicted segmaps
│
├─ experiments/
│    └─ abalation/ -> data ablation experiments
│    └─ data_augmentation/ -> data augmentation experiments
│    └─ baseline/ -> original published baseline
│
├─ models/
│    └─ 1. Baseline -> baseline CGNet model (Jaccard loss, 15 epochs)
│    └─ 2. Baseline with LRS -> baseline trained with learning rate scheduler
│    └─ 3. Feature engineering -> model 2 trained on engineered features (velocity/vorticity)
│    └─ 4. Cross-entropy -> model 3 + cross-entropy loss
│    └─ 5. Weighted CE -> model 3 + weighted cross-entropy loss
│    └─ 6. Focal Tversky -> model 3 + focal Tversky loss
│    └─ 7. Weighted Jaccard -> model 3 + weighted Jaccard loss [best performing]
```

## ClimateNet Dataset

ClimateNet is an open, community-sourced, human expert-labeled data set, mapping the outputs of Community Atmospheric Model (CAM5.1) climate simulation runs, for 459 time steps from 1996 to 2013.

![](<climatenet.png>)

Each example is a netCDF file containing an array (1152, 768) for one time step, with each pixel mapping to a (latitude, longitude) point, with 16 channels for key atmospheric variables and one class label.

| Channel | Description                                               | Units  |
|---------|-----------------------------------------------------------|--------|
| TMQ     | Total (vertically integrated) precipitable water          | kg/m^2 |
| U850    | Zonal wind at 850 mbar pressure surface                   | m/s    |
| V850    | Meridional wind at 850 mbar pressure surface              | m/s    |
| UBOT    | Lowest level zonal wind                                   | m/s    |
| VBOT    | Lowest model level meridional wind                        | m/s    |
| QREFHT  | Reference height humidity                                 | kg/kg  |
| PS      | Surface pressure                                          | Pa     |
| PSL     | Sea level pressure                                        | Pa     |  
| T200    | Temperature at 200 mbar pressure surface                  | K      |
| T500    | Temperature at 500 mbar pressure surface                  | K      |
| PRECT   | Total (convective and large-scale) precipitation rate     | m/s    |  
| TS      | Surface temperature (radiative)                           | K      |
| TREFHT  | Reference height temperature                              | K      |
| Z1000   | Geopotential Z at 1000 mbar pressure surface              | m      |
| Z200    | Geopotential Z at 200 mbar pressure surface               | m      |
| ZBOT    | Lowest modal level height                                 | m      |
| LABELS  | 0: Background, 1: Tropical Cyclone, 2: Atmospheric river  | -      |  


The data set is split in a training set of 398 (map, labels) pairs spanning years 1996 to 2010 in the CAM5.1 climate simulation, and a test set of 61 (map, labels) pairs spanning 2011 to 2013.

For learning-rate scheduling purposes, we further split the train set into a training (1996-2007) and valiation (2008-2010) set as well.

You can find the data at [https://portal.nersc.gov/project/ClimateNet/](https://portal.nersc.gov/project/ClimateNet/) and we provide a notebook to download the data automatically in the `data/` folder.


## ClimateNet CGNet implementation

We build on ClimateNet, a Python library for deep learning-based Climate Science. It provides tools to train pyTorch models on ClimateNet data for semantic segmentation of extreme weather events, and is used as the implementation of our baseline model.

ClimateNet library repository: [https://github.com/andregraubner/ClimateNet](https://github.com/andregraubner/ClimateNet).

The library is itself building on the orginal CGNet implementation proposed here: [https://github.com/wutianyiRosun/CGNet](https://github.com/wutianyiRosun/CGNet).


## Baseline

We use the library and the published implementation of the CGNet network as our baseline, and assess baseline performance on the latest published weights trained over the ClimateNet training set for 15 epochs with the Jaccard loss.

Pre-trained weights available at [https://portal.nersc.gov/project/ClimateNet/climatenet_new/model/](https://portal.nersc.gov/project/ClimateNet/climatenet_new/model/).


## Results

Results can be seen in the chart below

![](<results.png>)

Select experiments we performed and models we trained are reported in the `experiments/` and `models/` directories.

## References

Methods: _Lukas Kapp-Schwoerer, Andre Graubner, Sol Kim, and Karthik Kashinath. Spatio-temporal segmentation and tracking of weather patterns with light-weight neural networks. AI for Earth Sciences Workshop at NeurIPS 2020. [https://ai4earthscience.github.io/neurips-2020-workshop/papers/ai4earth_neurips_2020_55.pdf](https://ai4earthscience.github.io/neurips-2020-workshop/papers/ai4earth_neurips_2020_55.pdf)._

Human-labled ClimateNet dataset: _Prabhat, K. Kashinath, M. Mudigonda, S. Kim, L. Kapp-Schwoerer, A. Graubner, E. Karaismailoglu, L. von Kleist, T. Kurth, A. Greiner, A. Mahesh, K. Yang, C. Lewis, J. Chen, A. Lou, S. Chandran, B. Toms, W. Chapman, K. Dagon, C. A. Shields, T. O’Brien, M. Wehner, and W. Collins. Climatenet: an expert-labeled open dataset and deep learning architecture for enabling high-precision analyses of extreme weather, 2021. [https: //gmd.copernicus.org/articles/14/107/2021/](https: //gmd.copernicus.org/articles/14/107/2021/)._

Origial CGNet paper: _Tianyi Wu, Sheng Tang, Rui Zhang, Juan Cao, and Yongdong Zhang. CGNet: A light-weight context guided network for semantic segmentation. IEEE Transactions on Image Processing, 30:1169–1179, 2021. doi: 10.1109/TIP.2020.3042065. [https://github.com/wutianyiRosun/CGNet](https://github.com/wutianyiRosun/CGNet)._
