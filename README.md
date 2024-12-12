# Electricity Generation Forecasting for AICUP2024

This project is developed for the [AICUP2024 Electricity Generation Forecasting Competition](https://tbrain.trendmicro.com.tw/Competitions/Details/36). The objective is to predict electricity generation from solar panels based on microclimate data and weather station inputs. Our approach leverages LSTM-based sequential models to process time-series data effectively.

## Competition Overview
Competition Name: AICUP 2024 - Electricity Generation Forecasting  
Leaderboard Score: 537581.4  
**Rank: 16 / 934 (Top 1.7%)**

The competition aims to utilize microclimate data to predict solar panel power output at various locations.

## Repository Structure
Below is the structure of the repository:
```
AICUP2024_electricity_prediction/
├── 36_TrainingData/                  # Primary training datasets
├── 36_TrainingData_Additional_V2/   # Additional datasets
├── cwbdata/                          # External data from Central Weather Bureau (CWB)
├── code/                             # Code for training and predicting
├── models/                           # Model checkpoints
├── scalar/                           # Scalers for normalization, used during both training and prediction
├── submits/                          # Submission files
├── requirements.txt                  # List of dependencies
└── README.md                         # Project documentation
```

### relationship between folders explained
* During training, the main Jupyter Notebook file (cwb2loc___.ipynb) is located in the `code/` folder. This notebook is responsible for data preprocessing, training, and merging datasets. During the training process, scalar files are generated to record the parameters for data normalization, and model checkpoints are saved to preserve the best-performing models. These files are stored in the `scalar/` and `models/` folders, respectively.
* For generating the submission files, another Jupyter Notebook (predict___.ipynb) in the `code/` folder is used. This notebook reads the saved models and scalars, applies the normalization, and outputs the submission file.

# Method
## Data Description
1. Primary Data:
    * Meteorological data from the Central Weather Bureau, including temperature, rainfall, sunshine duration, and UV index.
2. Supplementary Data:
    * Data from additional datasets provided in the competition.
3. Features Used:
    * Hourly meteorological readings for [temperature, rainfall, sunlight duration, etc.].
    * Solar panel location encoded as one-hot or geographical coordinates.

## Model Architecture
The model employs a Bi-directional LSTM for sequential data processing. 
The architecture includes:
* Input: Time-series data from meteorological readings and solar panel location encoding.
* Hidden Layers: Two LSTM layers followed by two fully connected layers with ReLU and residual connections.
* Output: Solar panel power generation for each time unit.

## Training Methodology
### Preprocessing
* Data Interpolation:
    * Tested methods: linear, cubic spline, and no interpolation. The final method retained was "no interpolation" due to stability.
* Missing Data Handling: Used neighboring values to fill gaps. Records with excessive gaps were discarded.
* Feature Engineering: Explored encoding methods, including one-hot and raw numerical location identifiers.
### Hyperparameters
* Batch Size: 16
* Optimizer: AdamW (LR: 1e-3, Weight Decay: 1e-2)
* Scheduler: ReduceLROnPlateau
* Early Stopping: Monitored validation loss with patience of 50 epochs.
### Tools
* Wandb: For logging and visualizing metrics.
* Auto Checkpoints: Saved the best model based on validation loss.

## Results
* Best Validation Loss: Achieved after ~300 epochs using "no interpolation" for preprocessing.
* Leaderboard Score: 537581.4, securing rank 16.

## Future Work
* Experiment with alternative models like XGBoost or diffusion models.
* Incorporate additional features like air pollution indices.
* Leverage pre-training on external datasets to enhance feature representation.

# Usage
## Environment Setup
**Installation Steps**
1. Clone the repository:
```
git clone https://github.com/davidteng00000/AICUP2024_electricity_prediction.git
cd AICUP2024_electricity_prediction
```
2. (optional) Set up a virtual environment (using venv for example):
```
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate
```
3. Install dependencies:
```
pip install -r requirements.txt
```

## Contact
* Author: Po-Yuan Teng
* Email: davidteng@g.ncu.edu.tw  
For detailed questions, refer to the competition report and code repository.
