---
title: Pass Storage Account connection string to Key Vault on release pipeline
published: true
description: 
tags: #devops #azure #microservices
---

# Introduction

I wrote a few sentence about an Azure Cloud [here](https://dev.to/meanin/create-azure-storage-account-on-release-pipeline-4kn4). Most of release pipelines changes somehow app configuration to align it with new environment. For a database connection, usually, we inject a connection string into mentioned configuration. Where do you get from a connection string? From a database, you say. But how to achieve the same, when database is created on the same release pipeline?

This was a missing part for me, to create a fully operational environment on release pipeline from a tasks available on an Azure DevOps. Sure there is [ARM template deployment](https://docs.microsoft.com/en-us/azure/devops/pipelines/apps/cd/azure/deploy-provision-azure-vm?view=vsts), but I am not a big fan of it.

I decided to create my own task which will take a connection string and pass it to a [Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-overview) - Safe store for you secrets, keys and certificates. 

# Install extension

To install this extension, you need an organization on Azure DevOps portal. You can start [here](https://azure.microsoft.com/en-us/services/devops/?nav=min). On this portal, you have to have rights to install extensions. Then navigate [here](https://marketplace.visualstudio.com/items?itemName=meanin.storage-account-managment).

# Configure task

![img](https://raw.githubusercontent.com/meanin/vsts-tasks/master/screenshots/connectionstringtokeyvault.png)

To use this task you have to have a [configured](https://docs.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints?view=vsts#sep-azure-rm) Azure Resource Manager connection with a Service Principal. Set a storage account name which connection string you want to pass into a Key Vault. Set a Key Vault name (if it does not exists, it will be created now). Set a Key name. If your Key Vault does not exists yet, you can set a location for it, or it will inherit location from a Resource Group (scoped by a Service Principal). And voila', after task execution is completed, you key is securely stored in a Key Vault.

This is first published version of this task. I will appreciate all feedback :)