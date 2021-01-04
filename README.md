# Operationalizing Machine Learning using Azure ML

In this project we are provided a dataset: <b> [Bank Marketing dataset](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) </b> <br>

First an AutoML experiment was created and run to find the best model. 
The problem is to predict whether the clients end up subscribing to the product being advertised by the bank. 
This model is then deployed as a REST endpoint so that others can query it and get a prediction. 
For Debugging and Monitoring purposes the logging was enabled. 
Using OpenAPI Docs the Swagger files was displayed which provides information on how to consume the endpoints.

## Architectural Diagram

Here is an architectural view of the project:
![project architecture](./screenshots/architecture.png)

## Key Steps

### Creating a service principal

I skipped this step because I was using the Lab provided by Udacity. I am not authorized to create a security principal. 

### Getting the data

The data to be used from this project is at the link [Bank Marketing dataset link](https://automlsamplenotebookdata.blob.core.windows.net/automl-sample-notebook-data/bankmarketing_train.csv) and uploaded in the Azure Datasets.
![data set](./screenshots/dataset.png)
![data set_details](./screenshots/banking_dataset.png)

### Setting up and running the AutoML Experiment

For this Compute cluster is created. After the creation of the cluster, an AutoML Experiment is created. The experiment is run using Classification with a primary metric of ``AUC_weighted`` on the dataset.

![AutoML](./screenshots/AutoML.png)

### Selecting the best performing model

Once the automated ML step is completed, the best performing model is selected (``VotingEnsemble``).

![Best performing model](./screenshots/best_model.png)

Here are the list of the models:

![List of Model](./screenshots/model_list.png)

### Deploying the best performing model

Now the best performing model is deployed. While deploying the model ``ACI (azure container instance)`` is selected and ``Authentication`` is enabled for the deployment.
![model deployment](./screenshots/model_endpoint.png)

### Setup logging

I have enabled logs using logs.py script in which I have used "service.update(enable_app_insights=True)" to enable the logs in the deployed model endpoint.
Then the script is run producing the output as shown in the screenshot below:
![script log run](./screenshots/logging.png)

This enables ``Application Insights`` as you can see in the screenshot below

![app insights enabled](./screenshots/application_insights.png)

### Endpoint documentation using OpenAPI(Swagger)

Documenting the endpoint is very important. 
Documentation provide information on the type of requests that can be made on the endpoints available for consumption, the type of input or data expected with a sample.
First The Swagger shell script was run to download the Swagger Container and to run my Swagger File.

![docker_swagger](./screenshots/docker_swagger.png)

This Shell script runs a sample Swagger page as shown below

![swagger](./screenshots/swagger_sample.png)

Now the Swagger.json file is downloaded, and the serve.py script is run, so we can access the Swagger page for the endpoint. 
![swagger](./screenshots/swagger.png)
![swagger_get](./screenshots/swagger_get.png)
![swagger_post](./screenshots/swagger_post.png)

### Consume the endpoint

The script found in ``endpoint.py`` is modified to consume the endpoint to make prediction. 
The score url and primary key is pasted in the script.
The endpoint script is run which makes post requests to the endpoint with some data to make prediction.
![consume endpoint](./screenshots/endpoint.png)

### Benchmark the endpoint

The benchmark shell script is run. The result is as shown below.
![benchmark](./screenshots/benchmark.png)

### Create, Publish and Consume a Pipeline
Using the Jupyter notebook provided in the starter files a pipeline is created. It uses the same dataset as before. 
After the execution of all the cells a pipeline is created and deployed for consumption.

Here is a screenshot showing that the pipeline run has been completed with the dataset and the auto_ml module:
![completed pipeline run](./screenshots/pipeline_run.png)
![completed pipeline run_2](./screenshots/pipeline_run_2.png)

Here's a screenshot of the pipeline published
![publish pipeline](./screenshots/pipeline_endpoint.png)
![publish pipeline_2](./screenshots/pipeline_endpoint_2.png)

## Screen Recording

[Here](https://youtu.be/iKsjQpxv7_0) is a screencast showing the project in action.

## Standout Suggestions

### Improve on the project

Some ways to improve this project are:
- Handle Class Imbalance as shown in the image below
  ![class balance](./screenshots/AreasofImp.png)

- The data contains many outliers. The outliers affect the model performance.

- Remove unnecessary or irrelevant columns that aren't contributing a lot to the model.

- An automated CI/CD pipeline can be set up so that with every new trained model it can be deployed automatically and used by the user.