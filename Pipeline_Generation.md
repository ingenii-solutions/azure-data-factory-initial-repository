# Pipeline Generation

All files you want to ingest into the platform need to be uploaded to the `raw` container in your data lake. If you already have a system to do this then you should be able to skip this section; if not we have built the [Ingenii Azure Data Factory Generator](https://github.com/ingenii-solutions/azure-data-factory-generator) to easily create and manage Data Factory pipelines to get data and write it to files in your data lake.

- [Adding or updating a data source](#adding-or-updating-a-data-source) - adding a new pipeline and the objects to support it
- [Deployment](#deployment) - deploying your objects to the Data Factory in different environments

## Adding or updating a data source

In this repository you can create the configurations that the [Ingenii Azure Data Factory Generator](https://github.com/ingenii-solutions/azure-data-factory-generator) package will read to create the pipelines and other Data Factory objects to pull data in. Every `.json` file in the `pipeline_generation` folder corresponds to an individual data source; several tables can be defined in one `.json` file. The steps to follow are:

1. Check the [Azure Data Factory Generator documentation](https://github.com/ingenii-solutions/azure-data-factory-generator/blob/main/docs/user/Usage.md) to see if it supports the type of connector you need to use, such as SFTP or a type of API call. If it's not there, please [raise an issue on the GitHib page](https://github.com/ingenii-solutions/azure-data-factory-generator/issues) so we know to add it in.
2. Create any external required resources, such as adding a password to the relevant Azure Key Vault so that the pipelines can access it. What each pipeline requires is detailed in the [Azure Data Factory Generator documentation](https://github.com/ingenii-solutions/azure-data-factory-generator/blob/main/docs/user/Usage.md).
3. For the next steps you should clone this repository, and for best practices open a new branch based on `main`.
4. In the `pipeline_generation` folder, if you want to add a new data source then create a new file, or if you want to update an existing data source by adding tables or changing the configuration then edit the respective file.
5. The generator is a Python package, and so can be installed with `pip install azure_data_factory_generator`. We've also provided a `requirements.txt` file, so instead you can run `pip install -r requirements.txt`.
6. Generate the Data Factory resources by running `make create-data-factory-objects` from the root of the repository, which will create the required `.json` files in the relevant subfolders.
7. Add the new and changed files to a commit, and push to update this branch.
8. Since the Development Data is also integrated with this repository, this is enough for the Data Factory to use these new objects. In the Development Data Factory check that your functionality is working as it should.

From this point this follows the normal CI/CD process just like any other change; please see the `README.md` file on the `adf_publish` branch for full details of the CI/CD process.
