# RAF622M - Final Project

This repository contains all Jupyter notebooks needed to run Amanda Christianson's and Karl Prokop's final project in the course RAF622M at the University of Iceland. The project consists of four notebooks and one config.yaml file. The notebooks are numbered according to the order in which they should be run. Any file paths are refering to the JURECA HPC system used during the course.

### Project Aim
The aim of the project is to compare the effect of using different normalization methods in the pre-prosessing stage. No normalization, min-max normalization, percentile normalization and standardization (z-score normalization) are compared. The test set performance of the models is compared using the metrics overall accuracy, balanced accuracy and F1-score.

### Notebook `1_data_selection`
Extracting four Sentinel-2 aquisitions from a tile in Dalarna in the north of Sweden. This data is saved to /p/scratch/training2600/team1/data/ in the format `.SAFE.zip`.

### Notebook `2_corine_data_preparation`:
Aligns CORINE land-cover data with the Sentinel-2 acquisitions. Some CORINE land-cover classes are combined using `CORINE_REDUCE_CLASSES` to better match what a machine learning model can distinguish and to improve performance. The notebook normalizes the image data using the four methods specified in the project aim. Patches for training are extracted. Class distribution is calculated and visualized.

- The training data is saved in sub-directories named after the normalization type, in the parent directory /p/scratch/training2600/team1/data/training_data
- The visualizations are saved in the directory /p/project1/TRAINING2600/team1/results

### Notebook `3_training_pipeline`:
Training pipeline from split into training/validation/test to predictions on the test set. Hyperparameters are set in the `config.yaml` file. The notebook should be run once for each normalization method, setting the `NORMALIZATION_TYPE` variable to the desired method. After training, the model is used to predict against the test set. These results are saved as .npz files in the directory /p/scratch/training2600/team1/data/evaluation/NORMALIZATION_TYPE.

### Notebook `4_model_evaluation`:
Calculation and comparison of performance metrics on the test set. The notebook assumes that data is avaliable for all normalization types. The saved evaluation results are stored in /p/project1/training2600/team1/evaluation_results. There you can find:

- `class_distribution_NORMALIZATION_TYPE.CSV`: Comparisons of true vs predicted classes. One file per normalization type.
- `confusions_matrix_NORMALIZATION_TYPE.png`: Confusion matrix normalized by true class. One file per normalization type.
- `confusion_matrices_all.png`: Simplified confusion matricies for all four normalization types. Used for the report.
- `majority_baseline_comparison.csv`: Majority baseline comparison using balanced accuracy as metric.
- `per_class_NORMALIZATION_TYPE.csv`: Per-class precision, recall, F1-score and support. One file per normalization type.
- `summary_metrics.csv`: Overall accuracy, balanced accuracy, macro-F1 score and gap for all normalization types.

## Rerun the project with your own data
To run the project with your own data, you need to change the file paths in the notebooks. Every notebook contains a some path variables at the top. If you want to rerun the project on JURECA, you only need to change `user` to your own username or teamname. If you want to run it on a different system, you might need to adjust to the existing file system structure there. 

The first notebook `1_data_selection` is downloading the dataset and needs internet access for that. It also needs login data for the Copernicus Open Access Hub, which should be stored in a .env file. To select different data, you only need to change the coordinates, dates and tileselection.