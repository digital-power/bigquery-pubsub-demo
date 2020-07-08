# BigQuery Demo

Demo project to setup a realtime stream of sales data to Google BigQuery using Pub/Sub and DataFlow. 

Do not forget to perform the final step of this readme to delete the project after finishing. Otherwise, this might be a costly excercise!

## Prerequisites

* [python 3](https://python.org)
* [pip](https://pip.pypa.io/en/stable/installing/)
* [jupyter](https://jupyter.org/install.html)

Alternatively, you can set up a Python 3 (with pip) Jupyter environment with Docker. The [Jupyter Docker Stacks](https://jupyter-docker-stacks.readthedocs.io/en/latest/index.html#) is a good starting point for this.

Besides the technical prerequisites, you need a Google account with access to Cloud Console. You can set this up with your private Gmail account. Read more bout Google Cloud over here: [Google Cloud](https://cloud.google.com/).

## Steps

### 1. Create project

1. Go to https://console.cloud.google.com
2. Click on select a project in the top bar and click on “New Project”
3. Fill in a name for the project and click “Create”
4. Note down the project ID in the notebook

### 2. Create service account

1. Navigate to IAM & Admin
2. Within IAM, navigate to service accounts and click on “Create service account”
3. Fill in a name for the service account and optionally a description and click on “Create”
4. Click on “Select a role” and select "Pub/Sub Publisher" and click on “Continue”
5. On the next step you can simply click on “Done”, this will redirect you to the service accounts overview
6. From here, click on your newly created service account and click on “Add key” -> “Create new key”
7. Select JSON and click on “Create”, this will download a JSON file to your computer. 
8. Copy this file to the root of the GIT repository and rename it to `service_account.json`

### 3. Create BigQuery dataset and table

1. Navigate to BigQuery
2. Select the BigQuery project on the left
3. Click on “Create dataset” and give the dataset the following ID: `bigquery_demo`
4. Select “European Union (EU)” from the dropdown for your data location
5. Leave the rest of the fields as is and click on “Create dataset”
6. Select your newly created dataset and click on “Create table”
7. Fill in a table name
8. Enable “Edit as text” for the schema and copy the JSON schema from the repository
9. Click on “Create table” and note the table ID in the notebook

### 4. Create Pub/Sub topic

1. Navigate to Pub/Sub, this will enable Pub/Sub for your project
2. Click on “Create Topic”
3. Fill in the topic ID, this should be “streaming_demo”
4. Leave encryption to Google-managed key and click on “Create topic”
5. Note down your topic ID and topic name in the notebook

### 5. Create GCS bucket

1. Navigate to Storage Browser
2. Click on “Create bucket” and fill in the fields
3. Note the name of your bucket down in the notebook

### 6. Setup DataFlow pipeline

1. Navigate to DataFlow in GCP
2. Click on “Create”
3. Fill in a job name and select “Europe-west1” as the regional endpoint
4. From the template dropdown, select “Pub/Sub Topic to BigQuery”
5. Fill in the required parameters:
	1. Input Pub/Sub topic: topic name (projects/bigquery-demo-282010/topics/streaming_demo)
	2. BigQuery output table: table ID
	3. Temporary location: gs://bigquery-demo/temporary
	4. Encryption: Google-managed key

### 7. Test message streaming to Pub/Sub

1. Open the notebook in the repo with Jupyter
2. Update the project_id and topic_id fields in the 3rd cell
3. Run all cells from top to bottom, the last cell wil start sending messages to your Pub/Sub topic
4. Validate that the messages are coming in through the graph in your topic overview

### 8. IMPORTANT: delete project after completing

1. Navigate to IAM & Admin / Settings
2. Click on "Shutdown"
3. Enter your project ID and click "Shut down"
