# Operationalizing-Machine-Learning 
This project is part of the Udacity Azure ML Nanodegree. In this project, we use Azure to configure a cloud-based machine learning production model, deploy it, and consume it. We also create, publish, and consume a pipeline.
# Architectural Diagram
![image](https://user-images.githubusercontent.com/59172649/147090347-7cd96251-7484-4026-8592-cba3c4c4c5a9.png)

   - Authentication Step : we create a Security Principal (SP) to interact with the Azure Workspace.
   - Automated ML Experiment Step : we create an experiment using Automated ML, configure a compute cluster, and use that cluster to run the experiment.
   - Deploy the best model  Step:  when we deploy the Best Model it will allow us to interact with the HTTP API service and interact with the model by sending data over POST requests.
   - Enable logging Step : Logging helps monitor our deployed model. It helps us know the number of requests it gets, the time each request takes, etc.
   - Swagger Documentation Step : In this step, we consume the deployed model using Swagger.
   - Consume model endpoints Step : We interact with the endpoint using some test data to get inference.
   - Create and publish a pipeline Step : In this step, we automate this workflow by creating a pipeline with the Python SDK.
# Authentication

I used the lab Udacity provided to us, so I skipped this step since I'm not authorized to create a security principal.
# Automated ML Experiment

In this step, I created an AutoML experiment to run using the Bank Marketing Dataset which was loaded in the Azure Workspace, choosing 'y' as the target column.
![image](https://user-images.githubusercontent.com/59172649/147095309-9c010dc8-d32e-43fa-9500-1566388ca7fe.png)
I uploaded the dataset into Azure ML Studio in the Registered Dataset Section.

For the compute cluster, I used the Standard_DS12_v2 for the Virtual Machine and 1 as the minimum number of nodes.

I ran the experiment using classification. The run took some time to test various models and found the best model for the task.
![image](https://user-images.githubusercontent.com/59172649/147095272-79074098-1690-4c93-9100-7cf88333f79b.png)
Since it's a classification problem, I specified the task type without enabling deep learning.
![image](https://user-images.githubusercontent.com/59172649/147095578-70401e6f-a13e-4eb7-a339-55ffec6a166e.png)
I reduced the Exit Criterion to 1 hour and Concurrency to 5 concurrent iterations max.

we ran The experiment for about 32 minute , and many models are being trained on the Bank Marketing Dataset to find the best one.
![image](https://user-images.githubusercontent.com/59172649/147096063-63c3d748-d870-46d4-aa5a-631842fb8bc6.png)
![image](https://user-images.githubusercontent.com/59172649/147096093-d963d3b3-1f73-4dec-aaba-b2ff7103cf63.png)
The best model for this classification problem was a Voting Ensemble model with 0.969 Accuracy.
In this section, I can see all of the model's metrics, such as Accuracy.
![image](https://user-images.githubusercontent.com/59172649/147096323-46fb6b6b-a847-44ba-951a-421784201e13.png)
I can see some graphs such as Recall and ROC .There are many other details that I can access on the side by checking any value I'm interested in.
# Deploy the best model
To interact with the best chosen model for our task, we need to deploy it. This can be easily done in the Azure Machine Learning Studio, which provides us with an URL to send our test data to.

In this step, we deployed our trained Voting Ensemble model using Azure Container Instance (ACI), with authentication enabled.
![image](https://user-images.githubusercontent.com/59172649/147096563-006f8637-6aed-45b5-9e82-f1bb364998a5.png)
I deployed the best model using Azure Container Instance (ACI) with Authentication enabled.
![image](https://user-images.githubusercontent.com/59172649/147096663-a4a68a92-d717-4eb9-846b-d3835d89b468.png)
The model is successfully deployed, and we can access the model endpoint in the Endpoints section of Azure ML Studio.
![image](https://user-images.githubusercontent.com/59172649/147096716-54e0360c-df67-44ba-9546-4dbba2f21de9.png)
# Enable logging
Enabling Application Insights and Logs could have been done at the time of deployment, but for this project we achieved it using Azure Python SDK.
![image](https://user-images.githubusercontent.com/59172649/147096804-b52a8097-887b-4053-b18e-f8ad884bccd4.png)
Running the logs.py script requires interactive authentication, after successfully logging in, we can see the logs in the screenshots above.
![image](https://user-images.githubusercontent.com/59172649/147096896-d5e2e792-b199-44e5-bc1e-3c6ed582b17a.png)
By running the logs.py script, we enabled Application Insight
# Swagger Documentation
To consume our best AutoML model using Swagger, we first need to download the swagger.json file provided to us in the Endpoints section of Azure Machine Learning Studio.
This is the default Swagger documentation before running the swagger.sh script.
![image](https://user-images.githubusercontent.com/59172649/147097356-9885beff-2647-45b8-859f-0ad9a5bd23ec.png)
Then we run the swagger.sh and serve.py files to be able to interact with the swagger instance running with the documentation for the HTTP API of the model.
![image](https://user-images.githubusercontent.com/59172649/147097035-393e1e9b-4561-42eb-ba43-715c095556af.png)
After running the script, we can find our best model's documentation instead of the default Swagger page.
![image](https://user-images.githubusercontent.com/59172649/147097275-9b4653bc-d50e-4691-b70e-031206704139.png)
This is the content of the API, diplaying the methods used to interact with the model.
![image](https://user-images.githubusercontent.com/59172649/147097291-4d53d1cd-e8a2-4924-a116-3abfe88ffcba.png)
And this is the input for the /score POST method that returns our deployed model's preditions.
# Consume model endpoints
Finally, it's time to interact with the model and feed some test data to it. We do this by providing the scoring_uri and the key to the endpoint.py script and running it.
![image](https://user-images.githubusercontent.com/59172649/147097532-d9f82b0d-318a-4b04-8caf-636be92d981f.png)
After modifying both the scoring_uri and the key to match the key for my service and the URI that was generated after deployment, I ran the endpoint.py script to get inference from the deployed model.
# Create and publish a pipeline
For this step, I used the aml-pipelines-with-automated-machine-learning-step Jupyter Notebook to create a Pipeline

I created, consumed and published the best model for the bank marketing dataset using AutoML with Python SDK.
![image](https://user-images.githubusercontent.com/59172649/147097638-45b82af8-ccd0-4ae6-99bf-641aa577cfdd.png)
After updating the notebook to have the same keys, URI, dataset, cluster, and model names already created, I run through the cells to create a pipeline.
![image](https://user-images.githubusercontent.com/59172649/147097693-3f757cc0-f9e2-434d-9dd9-6e62d8f63960.png)
This is the pipeline created in the Pipelines section of Azure ML Studio.
![image](https://user-images.githubusercontent.com/59172649/147097719-9cba5f20-7d54-41cb-9908-f8da4a107ce8.png)
This is the Pipeline Overview in the Azure ML Studio.
![image](https://user-images.githubusercontent.com/59172649/147097829-ff0d9170-963c-415e-8452-d21a80112337.png)
![image](https://user-images.githubusercontent.com/59172649/147097882-aad696f8-dbce-426f-a525-160432810b5d.png)
![image](https://user-images.githubusercontent.com/59172649/147097926-9cb25313-53fc-4bf0-b0b6-91b00174d76d.png)
This is the REST endpoint in Azure ML Studio, with a status of ACTIVE.







![image](https://user-images.githubusercontent.com/59172649/145849546-7ca4d931-fe8a-4ef0-82b7-7a3b70c8eb22.png)
![image](https://user-images.githubusercontent.com/59172649/145860448-e7fc971a-2979-4f96-8f96-69908cd2d077.png)
![image](https://user-images.githubusercontent.com/59172649/145860747-35c512c7-eb05-4bca-830a-2e2a1284437a.png)
![image](https://user-images.githubusercontent.com/59172649/145861656-1d3ce6dd-7586-4706-aec0-bd03ffd34e92.png)
![image](https://user-images.githubusercontent.com/59172649/145864541-548bf455-de0f-4156-a955-886d881190e5.png)
![image](https://user-images.githubusercontent.com/59172649/145864786-da782ba4-9d99-4ca8-94b4-d1caa5c1a8ae.png)


![image](https://user-images.githubusercontent.com/59172649/145987643-adaeef40-1f37-48e1-9f1b-21cdabf44920.png)
![image](https://user-images.githubusercontent.com/59172649/145987984-3a35cabe-e106-4bc8-b442-2bb37ac19fe8.png)
![image](https://user-images.githubusercontent.com/59172649/145988103-705c7fed-dcca-48cf-81ce-45a51bc30656.png)
![image](https://user-images.githubusercontent.com/59172649/145988451-27bdf798-c7b6-41f2-9335-708f0694295c.png)
![image](https://user-images.githubusercontent.com/59172649/145988855-8661e4a2-32aa-420c-9d31-8de3e1e78498.png)
![image](https://user-images.githubusercontent.com/59172649/145988965-9d6bcb99-c4cf-42ea-a97d-82b8a75766eb.png)





![image](https://user-images.githubusercontent.com/59172649/145992087-6a2fdcd7-6cf7-4f13-9762-77323afcf3df.png)
![image](https://user-images.githubusercontent.com/59172649/145992522-ef49b54f-7b04-4dbf-8f27-450eaa545951.png)
![image](https://user-images.githubusercontent.com/59172649/145997394-4b8b8c18-75e6-4b2b-b304-2a1d624fb16e.png)
![image](https://user-images.githubusercontent.com/59172649/145997567-7f0164d9-cfa0-4c3f-9c9f-a9afcaaf638d.png)
![image](https://user-images.githubusercontent.com/59172649/145997685-580266f5-1123-47b9-8824-992987e81b9a.png)
![image](https://user-images.githubusercontent.com/59172649/145997845-82234fa0-a1f1-4609-b99e-71848d811f83.png)
![image](https://user-images.githubusercontent.com/59172649/145998076-d21dacb0-bb5d-41df-8b99-227d24bcf1ef.png)
![image](https://user-images.githubusercontent.com/59172649/146001297-a54618d6-837e-428f-b900-6af8cac15d9f.png)
![image](https://user-images.githubusercontent.com/59172649/146001403-8aacde2a-b41c-47f1-886c-a5df047fee4d.png)
![image](https://user-images.githubusercontent.com/59172649/146002041-dcd14d27-a655-4701-8d39-4d7e59fd1788.png)
![image](https://user-images.githubusercontent.com/59172649/146002999-2a938bf7-7630-4fae-8854-eae43a47b863.png)
![image](https://user-images.githubusercontent.com/59172649/146085879-df2734da-3d5a-4462-9c1d-36ddadc49c2a.png)
![image](https://user-images.githubusercontent.com/59172649/146085975-1b5299bc-761e-4dba-8332-ce49c7958897.png)
![image](https://user-images.githubusercontent.com/59172649/146086123-9ee23d1c-f6b6-46a2-ab21-2e8a952c01cd.png)
![image](https://user-images.githubusercontent.com/59172649/146086213-45a75dab-c2d0-4d94-9bb4-53264fd3032b.png)
![image](https://user-images.githubusercontent.com/59172649/146091893-acc11314-5912-4f0f-bfdb-d13d06e3e53c.png)
![image](https://user-images.githubusercontent.com/59172649/146091988-4350b9f6-6b19-4ed5-93b0-c8c52b13c735.png)
![image](https://user-images.githubusercontent.com/59172649/146092238-94d69212-04b8-4560-af39-365ecd5ea656.png)

![image](https://user-images.githubusercontent.com/59172649/146089289-c88bc82f-95af-423e-b199-1c28e1b25192.png)
![image](https://user-images.githubusercontent.com/59172649/146092611-a7fab383-d4e0-457d-a151-5f825fd4497e.png)
![image](https://user-images.githubusercontent.com/59172649/146092887-f8ad7112-274c-4d72-af78-3819262758a6.png)
![image](https://user-images.githubusercontent.com/59172649/146093282-68b64b16-6847-48cb-b35d-68f5e3a885ff.png)
![image](https://user-images.githubusercontent.com/59172649/146093502-80be8cc6-5e67-4a8e-9bea-4aaa5e98abde.png)
![image](https://user-images.githubusercontent.com/59172649/146093546-71fe5837-df5c-46ea-a2b7-ed8f4b966ce7.png)









