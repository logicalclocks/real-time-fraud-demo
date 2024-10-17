# Hopsworks real-time credit card fraud detection tutorial

## Introduction
In this guide we will demonstrate how to build a real-time machine learning system detecting credit card fraud.

The guide is divided into the following sections:
- **Data Generation**: Generate a sample of credit card user profiles and transactions
- **Feature Pipeline**: Compute the features and create the feature groups
- **Training Pipeline**: Assemble the training dataset, train and validate the model
- **Inference Pipeline**: Deploy the model and make requests

### Data Generation

Run the `data_generation/generate_historical_data.ipynb` notebook to generate 2 CSV files:
- `historical_transactions.csv`: Containing historical transactions
- `kyc.csv`: Containing informations about the different accounts

You can save the above csv files in Hopsworks or S3. 

<span style="background-color: #FFFF00">Please note that you will have to adjust the location of the CSV files in the feature pipelines notebooks</span>

### Feature Pipeline

The feature pipeline directory contains the code to compute the different feature groups and populate the feature store.
While the first run is static, you can use the data generator above to generate new data and update the feature groups (in real-time)

I suggest you execute the notebooks in the following order:

#### Profiles
Uses the data in the `kyc.csv` file to create the necessary profiles features.

#### Transactions
Uses the data in the `historical_transactions.csv` file to create the necessary transactions features.

#### Profiles Last Transactions
Uses the data in the `historical_transactions.csv` file to compute the time and location of the last transaction for each account

### Training Pipeline
The training pipeline contains the code to join the different features together, generate a training dataset, train a model and registered with the feature store.
- `fraud_model_fv.ipynb`: This notebook joins the necessary features together and register the feature view. It also generates a training dataset split by time.
- `model_training,ipynb`: This notebook trains the model on the training dataset and registers the model with the Hopsworks model registry.

### Inference Pipeline

This creates a KServe based deployment on Hopsworks. It deploys the model and provide and endpoint where you can send predictions. While you run your own KServe deployment, you can extract the code to fetch the data from the online feature store and compute the on-demand features.