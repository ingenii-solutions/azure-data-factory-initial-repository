# Ingenii Azure Data Factory Initial Repository

This repository is the starting point for any Data Factory meant to pull data into the Ingenii Azure Data Platform.

Provided are initial linked services and data sets for interacting with the Credential Store Key Vault and the Data Lake that the data should end up in.

## Resource Names

One thing you'll notice in the linked services and data sets is that the name of the Data Lake or Key Vault is a parameter to be passed. Our recommended approach here is to use global variables to host these names, which are specific to the specific Data Factory instance in the Dev/Test/Prod environment. These variables can then be passed in pipelines so that the same data set and pipeline definitions will work in any Data Factory.

Unfortunately, we cannot add these names as part of the platform deployment as they must be added once the Data Factory has been joined to the Azure DevOps repository. [See this guide](https://docs.microsoft.com/en-us/azure/data-factory/author-global-parameters) for more information and how to create these variables.
