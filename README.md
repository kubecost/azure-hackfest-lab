# azure-hackfest-lab
Kubecost workshop for the Azure Hackfest event 

## Pre-Requisites

Kubecost has the ability to ingest the Azure Cost Export to ensure that the costs we use are reflective of any Reserved Instances or enterprise discounts you have.

Due to the scheduled nature of the Cost Exports being published, they should be configured ahead of time.

Steps via [Azure Portal](https://portal.azure.com):

1. Login to [Azure Portal](https://portal.azure.com).
1. Select "Cost Management" from the home page or search box.
1. Select the Scope you want to create the export for.
    1. It is recommended to choose the subscription where your test AKS cluster as the billing scope.
1. Select "Exports" from the menu.
1. Select "Add" from the resource menu
1. Provide a meaningful but simple name (Example: amortizeddailym2d).
1. For "Metric", select "Amortized cost (Usage and Purchases)".
1. For "Export Type", select "Daily export of month-to-date costs".
1. For "Start Date", select todays date.
1. Leave "File Partitioning" off.
1. For the "Storage" section, choose "Use existing" or "Create new" based on your preference and populate the remaining details.
    1. It is recommended to create a new storage account so it can be easily identified.
    1. Take note of the Storage Account and Container Name.
1. Tag the storage account with the following name and value: `kubernetes_namespace:kubecost`

Steps via az cli:

```shell
location=
subscriptionId=
resourceGroupName=
storageAccountName=
storageContainerName=
storageDirectoryName=

az resource group create

az storage account create \
  --name \
  --resource-group \
  --location \
  --sku Standard_LRS \
  --kind StorageV2

az storage container create \
  --name "costexports" \
  --account-name 

az costmanagement export create \
  --name "kcamortizeddailym2d" \
  --scope "/subscriptions/{subscriptionId}/" \ --storage-account-id "kcazhhdemo" \
  --storage-container "costexports" \
  --timeframe MonthToDate \
  --recurrence Daily \
  --storage-directory "kubecost"\
  --type AmortizedCost

```
