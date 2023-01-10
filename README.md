# Application Integration logging to Cloud Logging

This sample repository demonstrates how to log from [Application Integration](https://cloud.google.com/application-integration/docs/overview) to GCP Cloud Logging.

# Description

Application Integration sends logs (also known as execution logs) to the `executions` entity. You can try the API [here](https://cloud.google.com/application-integration/docs/reference/rest/v1/projects.locations.integrations.executions/list). But some customers may want to send this information to Cloud Logging. This sample shows how to send integration information to Cloud Logging.

## Using the Integration

This integration can be used as-is to try the feature without any changes for demo/experimental purposes. However, it is not meant to be used as a separate integration. The recommendation is to import the JavaScript task and the REST task into your own integration. Then edit the JavaScript task to add the names for the variables you want to be sent to cloud logging.

```javascript
function executeScript(event) {

    //enter the list of all the variables to log
    var logVariables = ["sample1", "sample2", "sample3"];
    ...
```

# Prerequisites

* The integration version JSON is downloaded [here](./src/executeworkflows.json)
* This repo uses a custom cloud builder called `integrationcli-builder`. , based on [integrationcli](https://github.com/srinandan/integrationcli) and gcloud

# Configuring Cloud Build

In the settings page of Cloud Build enable the following service account permissions:
* Secret Manager (Secret Manager Accessor)
* Service Accounts (Service Account User)
* Cloud Build (Cloud Build WorkerPool User)

Grant the Application Integration Admin role to the Cloud Build Service Agent

```
    gcloud projects add-iam-policy-binding PROJECT_ID \
        --member="serviceAccount:service-PROJECT_NUMBER@gcp-sa-cloudbuild.iam.gserviceaccount.com" \
        --role="roles/integrations.integrationAdmin"
```

## Steps

1. Modify  the [overrides file](./overrides/overrides.json) to set the appropriate endpoint for Workflows. Replace the following string

```
"stringValue": "https://workflowexecutions.googleapis.com/v1/projects/<project-id>/locations/<region>/workflows/<workflow-name>/executions"
```

2. Modify the [authconfig file](./authconfig/authconfig.json) to set the appropriate Service Account with permissions to execute a Workflow. Replace the following string

```
"serviceAccount": "<sa-name>@<project-id>.iam.gserviceaccount.com",
```
Ensure this service account has LogWriter permission.

3. Trigger the build manually

```sh

gcloud builds submit --config=cloudbuild.yaml --region=<region-name> --project=<project-name>
```

The integration is labeled with the `SHORT_SHA`, the first seven characters of the commit id
___

## Support

This is not an officially supported Google product
