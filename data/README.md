
# Data

Here we share notebooks to work with the [ClimateNet dataset](https://portal.nersc.gov/project/ClimateNet/climatenet_new/) dataset:

- download_climatenet.ipynb: download ClimateNet (~30GB) into a local GDrive (for use with Colab) or S3 Bucket (for use with AWS).
- data_exploration_climatenet.ipynb: plot examples of visualizations of the 16 channels and compute statistics (mean, std, class prevalence) for training.
- feature_engineering.ipynb: generate new engineered features (velocity and votricity).
- train_history.ipynb: train a new model based on custom parameters, save weights and config, save training history, and plot precision/recall for our various experiments.
- visualize_predictions.ipynb: produce visualizations of the segmentation maps predicted by the different models we trained.