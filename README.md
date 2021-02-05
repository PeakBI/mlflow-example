# Example MLFlow project

Example of how to run a Python MLFlow project in the Peak ecosystem.

## Project Structure

A quick overview of the important files in this project, and what they do:

- **`run.py`** - this is the model training code. It trains a linear regression model using SKLearn. The training script takes two training parameters, `alpha` and `l1_ratio`. The training code uses the [MLFlow Python library](https://www.mlflow.org/docs/latest/python_api/index.html) to track the training performance. Take a look at the code to get a feel for how to do this, particularly the [`mlflow.log_param`](https://www.mlflow.org/docs/latest/python_api/mlflow.tracking.html#mlflow.tracking.MlflowClient.log_param), [`mlflow.log_metric`](https://www.mlflow.org/docs/latest/python_api/mlflow.tracking.html#mlflow.tracking.MlflowClient.log_metric), [`mlflow.log_artifact`](https://www.mlflow.org/docs/latest/python_api/mlflow.tracking.html#mlflow.tracking.MlflowClient.log_artifact) and [`mlflow.sklearn.log_model`](https://www.mlflow.org/docs/latest/python_api/mlflow.sklearn.html#mlflow.sklearn.log_model).
- **`MLProject`** - this file uses the [MLFlow Projects](https://www.mlflow.org/docs/latest/projects.html) format to help the mlflow cli to run the experiment more easily and reproducibly. In particular this file defines an **entrypoint** for the project, which tells the MLFlow CLI how to run the training script and defines training parameters, allowing you to set parameter types and default values. 
- **`requirements.txt`** - defines the PyPI packages required to run the training code. MLFlow prefers to use Conda as a package manager, but in Peak's ecosystem it's preferred to use PyPI dependencies than Conda.

## Setup

Refer to the Peak documentation for instructions about setting up your workspace or workflow environment to work with Peak's MLFlow integration.

**N.B** it is very important to ensure that the `MLFLOW_TRACKING_URI` and `MLFLOW_TRACKING_TOKEN` environment variables are set correctly, otherwise you may end up logging your MLFlow runs locally, and not into the Peak ecosystem.

Once you've set up your environment, you should clone this repository and install the dependencies:

`pip install -r requirements.txt`

It's recommended you run all of this in a virtual environment.


## Experiment Creation

You can create a new experiment using the [following cli command](https://www.mlflow.org/docs/latest/cli.html#mlflow-run):

`mlflow experiments create -n my-experiment`

## Run the experiment

You can run the experiment code either using the MLFlow CLI or by invoking the `train.py` script directly.

### Using the MLFlow CLI

This option makes use of the [MLFlow Projects](https://www.mlflow.org/docs/latest/projects.html) format and the `MLProject` file.

To run an experiment, use the [`mlflow run`](https://www.mlflow.org/docs/latest/cli.html#mlflow-run)

`mlflow run . --no-conda --experiment-id 1 -P alpha=0.5`

You can also pass the experiment name rather than the id:

`mlflow run . --no-conda --experiment-name my-experiment -P alpha=0.5`

Or you can set the experiment id or name in an environment variable - this is a better option in automated workflows:

```bash
export MLFLOW_EXPERIMENT_ID=1
# OR MLFLOW_EXPERIMENT_NAME=my-experiment
mlflow run . --no-conda -P alpha=0.5
```

You can run the same script multiple times and change the value of the `alpha` training parameter to any number between 0 and 1, and can also pass `l1_ratio` as a second parameter (also between 0 and 1)

In this example, the parameters `alpha` and `l1_ratio` are specific to this example script, so they will probably change in your own code.

**N.B - `--no-conda`** - MLFLow likes to use the Anaconda package manager, which is not supported by Peak out-of-the-box. In this example dependencies are installed using `pip` and the `--no-conda` flag tells MLFlow to not use Conda. Of course, if you do want to use Conda, it's possible to install it into your environment yourself.

### Running the script directly

You can also run the script directly using the `python` executable.

In this case, you will have to set the experiment ID or name using an environment variable:

```bash
MLFLOW_EXPERIMENT_ID=1 python train.py 0.3 0.7
```

This will run the `train.py` script with `alpha` as `0.3` and `l1_ratio` as `0.7`
