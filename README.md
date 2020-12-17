# Example MLflow project

Example MLFlow project for testing purposes

It's recommended you run all of this in a virtual environment

## Setup

Ask Peak Engineering about the environment variables you can set to run this in the Peak ecosystem.

First install mlflow - `pip install mlflow`

Then install the dependencies - `pip install -r requirements.txt`


## Experiment Creation

You can create a new experiment using the following cli command:

`mlflow experiments create -n test-experiment`

## Run the experiment

`mlflow run . --no-conda --experiment-name test-experiment -P alpha=0.5`

You can run the same script multiple times and change the value of `alpha` to any number between 0 and 1, and can also pass `l1_ratio` as a second parameter (also between 0 and 1)

These values `alpha` and `l1_ratio` are parameters specific to this example script, so don't worry about what they mean too much.

