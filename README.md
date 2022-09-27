---
name: Highly available zone-redundant web application
page_type: sample
languages:
- bicep
products:
- azure
- azure-app-service
- azure-functions
- azure-front-door
- azure-cache-redis
- azure-cognitive-search
- azure-search
- azure-cosmos-db
- azure-key-vault
- azure-blob-storage
- azure-private-link
- azure-service-bus
- azure-sql-database
---

# Highly available zone-redundant web application

This Azure Sample contains a Bicep template to deploy a working example of the Azure architecture center reference architecture: [Highly available zone-redundant web application][hazrwebapp]. The considerations section of the reference architecture document should be reviewed carefully, and the template modified to meet business requirements, before deploying to production environments.

## Prerequisites

* Azure Subscription with Contributor access
* [AZ CLI][azcli], either installed locally or using [Azure Cloud Shell][cloudshell].

## Deployment Steps

### 1. Clone this repo.

```
git clone https://github.com/Azure-Samples/highly-available-zone-redundant-webapp.git
cd highly-available-zone-redundant-webapp
```

### 2. Create a resource group

```
az group create -n zr-ha-webapp-rg -l westus2
```

### 3. Create a deployment

Deploy the solution using the bicep template provided by creating a deployment.

```
az deployment group create -g zr-ha-webapp-rg --template-file ./bicep/main.bicep
```

## Bicep parameters

| param | Description | Default value |
| -- | -- | -- |
| `applicationName` | Optional. A name that will be prepended to all deployed resources. | An alphanumeric id that is unique to the resource group. |
| `location` | Optional. The Azure region (location) to deploy to. Must be a region that supports availability zones. | Resource group location. |
| `staticWebAppsLocation` | Optional. The Azure region (location) to deploy Static Web Apps to. Even though Static Web Apps is a non-regional resource, a location must be chosen from a limited subset of region. | The value of the `location` parameter, or the resource group location. |
| `sqlAdminPassword` | Optional. A password for the Azure SQL server admin user. | Defaults to a new GUID. |

## Instructions

View the deployed resources in the Azure Portal observing:

* App Service web apps and Function Apps created and ready for app deployment.
* Private endpoints configured and auto-approved for Azure Service Bus, Cosmos DB, Azure Cache for Redis, Functions, Key Vault, Storage, Azure Search and SQL DB.
* Managed system identities enabled and role assignments created (where available).
* Azure Front Door routing rules configured.

Configure continuous deployment pipelines to deploy applications into Azure App Service Web Apps and Function Apps.

## Cleanup

Delete the resource group to cleanup all resources:

```
az group delete -n zr-ha-webapp-rg 
```

## Azure Policies

This implementation doesn't provide any workload-level Azure Policies to govern the architecture. Consider adding policies where it makes sense to enforce specific topology decisions, functional, and/ or non-functional requirements. Also, this implementation assumes no interference with Azure Policies that you may have applied to your subscription or your subscription's management group(s).  If you have a policy related error during deployment, please check into what Azure Policies are being enforced at your scope and adjust the template accordingly.


## Resources

* [Highly available zone-redundant web application][hazrwebapp] reference architecture in the [Azure Architecture Center][aac].
* [Contributing guidelines](./CONTRIBUTING.md).

<!-- Links -->
[hazrwebapp]:https://learn.microsoft.com/azure/architecture/reference-architectures/app-service-web-app/zone-redundant
[aac]:https://learn.microsoft.com/azure/architecture/
[azcli]:https://learn.microsoft.com/cli/azure/install-azure-cli
[cloudshell]:https://learn.microsoft.com/azure/cloud-shell/overview