# Ingenii Azure Data Factory Initial Repository - adf_publish Branch

This is the branch that Azure Data Factory uses in order to deploy to the Test and Production environments. For general usage of this repository, please see the `README` file on the `main` branch. Each repository should only be connected to one Dev Data Factory, and deploy into one Data Factory per Test and Production environment.

Please note that these are the pipelines that we provide, but this does not mean these are set in stone. If you wish to make improvements please do so.

## CI/CD

Ingenii has provided Azure DevOps Pipelines to easily publish changes from the Dev environment to Test and Production. After clicking the `Publish` button in your Dev Data Factory, Azure will automatically update this branch with the new factory definition, where these pipelines will take over.

The order of operations are:
1. When you want to make a change, in your Dev Data Factory create a new branch to safely make and test the changes.
2. Once you are happy with your changes, open a pull request to merge your branch into the `main` branch. Usually someone else will need to approve your pull request before it can be completed and the changes merged in.
3. In your Dev Data Factory, on the `main` branch, click the `Publish` button. Azure will update this `adf_publish` branch with the new changes.
4. The Test pipeline will start automatically, deploying to the Test Data Factory. Test the changes there.
5. Once you are happy, run the Production Pipeline in Azure DevOps to deploy the changes to Production.

### Pipelines

The `CI/CD` folder holds `.yml` files that define the Azure DevOps Pipelines.

As part of the Ingenii Azure Data Platform deployment, the names of the resources that the Data Factory will connect to, such as the Data Lake and the credentials store Key Vault, are made available to the pipeline. These are used to keep the Data Factory definition the same in each environment but connecting to the relevant resources.

The two environments (Test and Production) have their own files (`test.yml` and `prod.yml`) that use the steps from the same file (`template.yml`) to guarantee that the same actions are taken for both.

The steps the pipelines takes in `template.yml` are:
1. First, take the Dev factory definition that Azure provides, and make a copy in a separate folder
2. Replace the Dev resource names, depending on the environment it is deploying to. The pipeline will write to the logs the changes it is making.
3. Any custom replacements are applied (see the next section)
4. The updated definition is written to the pipeline logs so they can be reviewed if needed
5. The updated definition is deployed to the target Data Factory

### Custom replacements

The pipelines will update the names of several resources, including the Data Lake, credentials Key Vault, and Engineering Databricks hostname and cluster ID. In case there are more replacements you want to make, we've provided this option through the `custom_replacements` folder. In here, there are `JSON` files for each target environment where you can add the strings you want to replace.

For this example `JSON`;
```json
{
    "string1": "string2",
    "string3": "string4"
}
```
any occurence of `string1` in the factory definition will be replaced by `string2`, and `string3` with `string4`. Please review the step of the pipeline which writes out the updated definition to logs in order to check that things are being updated correctly.

## Data Factory 'Key' Name

In `CICD/template.yml`, one of the steps is called `'Set the Data Factory key name'`. This 'key' name is used in the case you have several groups Data Factories joined to separate repositories and is used to set them apart. If you have kept with the default deployment and not added more Data Factories to your platform, you can safely ignore this section.

This name is the key when the Data Factory is defined in your platform `.yml` file, or another way to determine this is from the repository name: the default name for this repository created by the Ingenii Azure Data Platform is `Data Factory - <key>`, so for `Data Factory - data` the key is `data`. `data` is the default value, so if this is incorrect you will need to update the `template.yml` file manually.

To be certain this is set correctly, check the Configuration Key Vault in your shared environment. This is in the resource group `<platform code>-shr-<location code>-rg-security`, and called `<platform code>-shr-<location code>-kv-conf-<uniqueness code>`. Check that the secret called `data-factory-name-<key>-dev` has the value of the name of the dev Data Factory this repository is connected to.

## Reference

- [Azure Data Factory CI/CD](https://docs.microsoft.com/en-us/azure/data-factory/continuous-integration-delivery)
- [Azure Data Factory Automated Publishing](https://docs.microsoft.com/en-us/azure/data-factory/continuous-integration-delivery-improvements)
