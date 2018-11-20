---
title: Deploy complete environmnent
published: false
description: 
tags: #devops #azure #dotnetcore
---

# Introduction

I hope that everyone knows about CI/CD processes. Continuous Integration helps teams work in parallel on the same project. Most common scenario is to use some git server and integrate work as a pull request from a feature branch to development one. Many tools provide easy way to build an artifact (installation package) from a merged code base. Continuous deployment is a way to maintain application state with newest code base on the desired environment.

Imagine that before pushing new changes to production, you want to make all of the tests on a dedicated environment. Or maybe on every newly created environment you want to seed some data? Sometimes on this specific environment data stored/missing can introduce unecessary bugs or unwanted situations which require you attention, before it can be considered as tested/usable.

# Idea

Now I came with an idea to create a brand new environment, just for testing purpose. In that way, you can always be sure, that there is no old data, but only data that is required to system work in the right way. Nothing will interfere with test cases, no blocker on a fresh start. 

I am using Microsoft Azure Cloud, as it is most familiar for me. In the simplest case, I want to create a storage (azure table storage), pass its connection string to the key vault, seed data there and finally deploy application which will consume the data.

# Solution

### 1. Create storage account
There are few ways to create a storage account on Microsoft Azure Cloud. For a start, there is an Azure Portal - the web interface for managing your cloud resources. If you are more familiar with devops work, you would use [Azure Powershell](https://docs.microsoft.com/en-us/powershell/azure/overview?view=azurermps-6.12.0) (cross platform right now) or [Azure CLI](https://docs.microsoft.com/en-us/cli/azure/?view=azure-cli-latest).

In my scenario, I want to integrate storage account creation process with continuous deployment process. I could write some powershell script and add it as a step for my CD definition. I can also write some custom extension for my CI/CD tool, which will give me a nice view for creating new storage account :) I am working with [Azure DevOps](https://azure.microsoft.com/en-us/services/devops/) (which is a successor of team services).

After some fights with diving into an azure devops extension development, I end up with something like described on one of my previous posts - [here](https://dev.to/meanin/create-azure-storage-account-on-release-pipeline-4kn4). It looks like below. Nice right? Instead of fighting with some powershell you can consume well designed ui for creating resource you need.
![img](https://raw.githubusercontent.com/meanin/vsts-tasks/master/screenshots/createstorageaccount.png)

### 2. Pass connection string to Key Vault
Using a different database / storage on each environment could be difficult. One way is to use dedicated configuration file for each environment, but then credentials are stored in a version control system. Second way is to configure dedicated hosting environment to have a credentails stored there, but it create a problem on configuring new environment. It is needed to remember for all of the credentials that application will use, before it even is deployed. 

Happily, there is another way, which is my favorite. Store all of the neccessary credentials outside of source control and fetch real values (override configuration file) on CD process. It can be done in a dedicated centralized storage for secrets. The Microsoft Azure Cloud provides a component that is called [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-overview). The Azure Devops portal  provides an easy way to fetch data from a key vault, you can read more about it  on [Microsoft docs](https://docs.microsoft.com/en-us/azure/devops/pipelines/library/variable-groups?view=vsts&tabs=yaml).

So I came up with an idea to push a connection string to my newly created Storage Account to the selected Key Vault during CD process, just after creation is completed. I didn't found any existing way to do that, besides inline powershell/cli script. As previous, I believe that developers who work on a code base, shouldn't care about configuring deployment scripts. I decided to create another task for the Azure Devops, also described in a dedicated [post](https://dev.to/meanin/pass-storage-account-connection-string-to-key-vault-on-release-pipeline-1pkl). It looks like below:
![img](https://raw.githubusercontent.com/meanin/vsts-tasks/master/screenshots/connectionstringtokeyvault.png)

### 3. Seed Table Storage
Have you ever store data without which product won't work on a database side? Data that is not permanent, that mutate during application lifetime, so there is need to be stored in a db, but without which application will not start. I imagine some user configuration values, defaults that are handled by admin, etc. For that case, usually missing part is seed functionality. Again I cannot find anything that works out of the box. 

The more I get into developing the Azure Devops extension, the more idea for new tasks I find. Now I created one that helps seeding data into the Azure Table Storage. See [post](https://dev.to/meanin/seed-table-storage-10an). It needs an input json file, with predefined two fields which are ATS restriction - PartitionKey and RowKey. Any other field can names as needed by an application. The task configuration needs an Azure subscription, Storage Account Name, table name and of course path to seed file. In this case it is a local json file.
![img](https://raw.githubusercontent.com/meanin/vsts-tasks/master/screenshots/seedtablestorage.png)

### 4. Application deployment
A default way to host an application in an Azure Cloud is to deploy it to an App Service. It supports multiple languages like c#/.net, java, php, static web sites (Angular/React/others), even node.js or python. It is able to use Windows OS, Linux, or docker container. Review the Microsoft documentation to learn [more](https://docs.microsoft.com/en-us/azure/app-service/app-service-web-overview). Configuration is pretty straightforward. There are few fields that needs your attention, like Azure subscription, App type and App Service Name. For more information, please visit this [page](https://docs.microsoft.com/en-us/azure/devops/pipelines/targets/webapp?toc=/azure/devops/deploy-azure/toc.json&bc=/azure/devops/deploy-azure/breadcrumb/toc.json&view=vsts).

# Summary
With some effort, now I am able to deploy a complete environment through the Azure Devops release pipeline. A Storage Account/table is create as a first step. The connetion string is passed to the Key Vault as secret. Next desired table is filled with seed data. Last, the application is deployed with credentials fetched from a Key Vault, configuration values are overwritten. From developers perspective, it is transparent to which database, which table application is writing/reading from on this specific environment. They can focus on delivering functionality, instead of fighting with deployment process, connection strings validation, credentials, etc. 

For the end, I prepared short asp.net core demo project, which read from the Azure Table Storage table all data, and present it on a web site. A code base can be found on my [github](https://github.com/meanin/asp-net-core-azure-storage-account). As you can see in the config file, there is only connection string to local emulator, instead of real ATS instance.
```C#
{
  "Logging": {
    "LogLevel": {
      "Default": "Warning"
    }
  },
  "AllowedHosts": "*",
  "ConnectionString": "UseDevelopmentStorage=true",
  "DemoTableName": "MyTable"
}
```
There is also included seed.json file, which could be possibly fetched from somewhere else, but for simplified view it is stored within a code base.
```
[
  {
    "PartitionKey": "partitionKey1",
    "RowKey": "1",
    "OtherColumn": "value1",
    "FourthColumn": "123"
  }    ...
]
```
A few minutes after triggering release, I saw this, brand new web application, consuming a table, which source code base does not know anything about. Voila!
![img](img/2018-mm-dd-deploy-complete-environment/deployed-application.png)

# P.S.
No, not powershell ;) If you have any needs for extend the Azure Devops, with some shinny new deployment task, you can contact with me :)