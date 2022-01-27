# Ingenii Azure Data Factory Initial Repository

This repository is the starting point for any Data Factory meant to pull data into the Ingenii Azure Data Platform. As part of the Platform deployment, a set of Data Factories will have been created related to this repository, one for each environment (e.g. Dev, Test, and Production). Following the structure [recommended by Azure](https://docs.microsoft.com/en-us/azure/data-factory/continuous-integration-delivery), only the Dev Data Factory should be integrated with this repository, which has been done already as part of the platform deployment.

Provided in this repository are:
1. Initial linked services and data sets for interacting with the Credential Store Key Vault and the Data Lake that the data files must be uploaded to.
2. CI/CD pipelines for deploying changes made in your Dev environment to Test and Production. Please see the CI/CD section below for more details.
3. Instructions for using the [Ingenii Azure Data Factory Generator](https://github.com/ingenii-solutions/azure-data-factory-generator).
4. An example `Ingenii Azure Data Factory Generator` config file in the `adfg_configs` folder.

## Resource Names

For the linked services, we have set example names for the Data Lake / Key Vault for these to connect to. Since we do not know the name of these reosurces before they are deployed, you will need to manually update these in your Dev Data Factory to point to the respective resources in your Dev environment. This can be done through the [Data Factory management hub](https://docs.microsoft.com/en-us/azure/data-factory/author-management-hub#linked-services).

## CI/CD

This repository comes with CI/CD pipelines in order to deploy changes made to the Dev Data Factory into your Test and Production environment Data Factories. Following the structure [recommended by Azure](https://docs.microsoft.com/en-us/azure/data-factory/continuous-integration-delivery), these are held on the `adf_publish` branch of this repository; please see the `README.md` file on that branch for full details of the CI/CD process.

## Pipeline Generation

To ease pulling data into your platform, Ingenii has created the [Ingenii Azure Data Factory Generator](https://github.com/ingenii-solutions/azure-data-factory-generator) which can take a small configuration file and generate Data Factory pipelines and their dependent resources. Please see the  [Pipeline_Generation.md](docs/user/Pipeline_Generation.md) file for full details of how to use this package in this repository, or the [usage details in the  source repository](https://github.com/ingenii-solutions/azure-data-factory-generator/blob/main/docs/user/Usage.md) for general instructions.

## Reference

- [Azure Data Factory CI/CD](https://docs.microsoft.com/en-us/azure/data-factory/continuous-integration-delivery)
- [Azure Data Factory Automated Publishing](https://docs.microsoft.com/en-us/azure/data-factory/continuous-integration-delivery-improvements)
- [Azure Data Factory Linked Services](https://docs.microsoft.com/en-us/azure/data-factory/concepts-linked-services)
