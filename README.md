# ML Pipeline with DVC and Remote Storage

A comprehensive machine learning pipeline project demonstrating best practices for data versioning, experiment tracking, and remote storage integration using DVC (Data Version Control).

## 📋 Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [Project Structure](#project-structure)
- [Installation](#installation)
- [Usage](#usage)
- [DVC Pipeline Setup](#dvc-pipeline-setup)
- [Experiments](#experiments)
- [Contributing](#contributing)
- [License](#license)

## 🎯 Project Overview

This project demonstrates a production-ready ML pipeline built with DVC for data versioning and experiment tracking. It includes components for data ingestion, preprocessing, and model training with comprehensive logging and parameter management.

## ✨ Features

- **Data Ingestion**: Load and split datasets with proper logging
- **Data Preprocessing**: Text transformation with NLTK (tokenization, stemming, stopword removal)
- **DVC Integration**: Version control for data, models, and pipeline stages
- **Experiment Tracking**: Track ML experiments with DVCLive
- **Parameter Management**: YAML-based configuration for reproducible pipelines
- **Comprehensive Logging**: File and console logging for all operations
- **Remote Storage Support**: Setup for cloud storage integration (S3, GCS, etc.)

## 📂 Project Structure

```
ML_pipeline_with_DVC_with_remote_storage_practice/
├── src/
│   ├── data_ingestion.py          # Data loading and train/test splitting
│   └── data_preprocessing.py       # Text preprocessing and transformation
├── data/
│   ├── raw/                        # Original raw data
│   │   ├── train.csv
│   │   └── test.csv
│   ├── interim/                    # Intermediate processed data
│   └── processed/                  # Final processed data
├── experiments/
│   ├── mynotebook.ipynb            # Jupyter notebook for experiments
│   └── spam.csv                    # Sample experiment data
├── logs/                           # Application logs
│   ├── data_ingestion.log
│   └── data_preprocessing.log
├── dvc.yaml                        # DVC pipeline configuration
├── params.yaml                     # Pipeline parameters
├── .gitignore                      # Git ignore rules
├── README.md                       # This file
└── LICENSE                         # Project license
```

## 🚀 Installation

### Prerequisites

- Python 3.8 or higher
- Git
- DVC

### Setup Steps

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd ML_pipeline_with_DVC_with_remote_storage_practice
   ```

2. **Create a virtual environment**
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**
   ```bash
   pip install pandas scikit-learn nltk pyyaml dvclive
   ```

4. **Initialize DVC**
   ```bash
   dvc init
   ```

5. **Configure remote storage** (optional)
   ```bash
   dvc remote add -d myremote s3://my-bucket/path
   # or for Google Cloud Storage
   dvc remote add -d myremote gs://my-bucket/path
   ```

## 📖 Usage

### Run Data Ingestion

```bash
python src/data_ingestion.py
```

This script:
- Loads data from `data/raw/` directory
- Splits into train and test sets based on parameters
- Saves split data with logging to `logs/data_ingestion.log`

### Run Data Preprocessing

```bash
python src/data_preprocessing.py
```

This script:
- Reads raw training and test data
- Applies text preprocessing (lowercase, tokenization, stopword removal, stemming)
- Encodes target column
- Removes duplicates
- Saves processed data to `data/interim/` with logging to `logs/data_preprocessing.log`

## 🔄 DVC Pipeline Setup

### 1. Initialize DVC Pipeline
```bash
dvc init
```

### 2. Create dvc.yaml
Define pipeline stages in `dvc.yaml`:
```yaml
stages:
  data_ingestion:
    cmd: python src/data_ingestion.py
    deps:
      - src/data_ingestion.py
      - data/raw/train.csv
      - data/raw/test.csv
    outs:
      - data/processed/train.csv
      - data/processed/test.csv

  data_preprocessing:
    cmd: python src/data_preprocessing.py
    deps:
      - src/data_preprocessing.py
      - data/raw/train.csv
    outs:
      - data/interim/train_processed.csv
```

### 3. Create params.yaml
Define parameters for reproducibility:
```yaml
data_ingestion:
  test_size: 0.2
  random_state: 42

preprocessing:
  text_column: "text"
  target_column: "target"
```

### 4. Run Pipeline
```bash
# Reproduce the entire pipeline
dvc repro

# Visualize the pipeline DAG
dvc dag
```

## 🧪 Experiments

### Track Experiments with DVCLive

```bash
# Install DVCLive
pip install dvclive

# Run an experiment
dvc exp run

# View all experiments
dvc exp show

# Apply a previous experiment
dvc exp apply <exp-name>

# Remove an experiment
dvc exp remove <exp-name>
```

### Using VS Code Extension

1. Install the DVC extension in VS Code
2. Navigate to the DVC panel to:
   - View all experiments
   - Compare experiment results
   - Switch between experiments

## 📝 Logs

Application logs are saved to the `logs/` directory:
- `data_ingestion.log`: Logs from data ingestion stage
- `data_preprocessing.log`: Logs from preprocessing stage

Log format: `TIMESTAMP - LOGGER_NAME - LOG_LEVEL - MESSAGE`

## 🔧 Configuration

### Parameters File (params.yaml)

All pipeline parameters are managed through `params.yaml`. Modify this file to adjust:
- Train/test split ratio
- Random seeds for reproducibility
- Text preprocessing columns
- Other model hyperparameters

## 🐛 Troubleshooting

### NLTK Resource Not Found
If you encounter `Resource punkt_tab not found`:
```python
import nltk
nltk.download('punkt_tab')
nltk.download('stopwords')
```

### DVC Remote Configuration
To setup remote storage:
```bash
# Add remote
dvc remote add -d myremote <remote-path>

# Push data to remote
dvc push

# Pull data from remote
dvc pull
```

## 📊 Best Practices

1. **Always use parameters.yaml** for configuration changes
2. **Commit parameters and .dvc files** to Git (not actual data)
3. **Run `dvc repro`** to ensure pipeline consistency
4. **Use `dvc exp run`** for experiment tracking
5. **Add meaningful commit messages** when pushing to Git/DVC

## 🤝 Contributing

1. Create a feature branch: `git checkout -b feature/your-feature`
2. Make your changes and test thoroughly
3. Run the pipeline: `dvc repro`
4. Commit: `git commit -am 'Add feature'`
5. Push: `git push origin feature/your-feature`
6. Create a Pull Request

## 📄 License

This project is licensed under the LICENSE file - see the file for details.