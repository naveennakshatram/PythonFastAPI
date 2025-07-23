# Python Fast-API with Harness AI

## Step 1

Create a python Fast-API application with required api endpoints

## Step 2

Create a Dockerfile and Create a Image to Docker file 
Better to Test locally (The Docker Image)

## Step 3

Setting up Google Cloud & Harness Connectors

Harness will use this service account to authenticate with GKE and GCR/GAR.

Go to the Google Cloud Console: console.cloud.google.com

Navigate to IAM & Admin > Service Accounts.

Click + Create Service Account.

Service account name: harness-gke-deployer

Click Done.

Click the three dots (⋮) under "Actions" for your new service account and select Manage keys.

Click Add Key > Create new key.

Select JSON as the key type and click Create. This will download a JSON file to your computer. Keep this file secure! (e.g., harness-gke-deployer-xxxxxxxxxxxx.json). You'll need its content for Harness.


## Step 4

Grant Permissions to the Service Account:

This service account needs roles to interact with GCR/GAR and GKE.

Go back to IAM & Admin > IAM.

Click + Grant Access.

New principals: Enter the email of your harness-gke-deployer service account.

Select a role: Add the following roles:

Storage Admin (or Storage Object Admin) - For pushing/pulling Docker images.

Kubernetes Engine Developer - For deploying applications to GKE.

Kubernetes Engine Service Agent (or Kubernetes Engine Admin if Developer isn't enough, but start with Developer)

Workload Identity User (if you plan to use Workload Identity, but Kubernetes Engine Developer is often enough for basic deployments).

Click Save.


## Step 5

Configure Harness GCP Connector:

This tells Harness how to use the service account to talk to GCP.

In Harness, navigate to your Project.

In the left-hand navigation, go to Project Settings (or Account Settings if you want it available across projects).

Click on Connectors.

Click + New Connector.

Search for and select Google Cloud Platform.

Connector Name: MyGCPConnector

Description: (Optional) Connects to my GCP project for GKE and GCR

Click Continue.

Credentials: Select Specify credentials in Harness.

GCP Credentials Type: Select Google Cloud Key (Service Account Key).


## Step 6

Building and Pushing Your Docker Image (CI Pipeline)

We need a CI pipeline to take your Dockerfile and code, build the Docker image, and push it to GCR.

Create a New CI Pipeline:

    Go to your Project > Continuous Integration (CI).

    Click + New Pipeline.

    Pipeline Name: FastAPIDockerBuild

    Click Start.

