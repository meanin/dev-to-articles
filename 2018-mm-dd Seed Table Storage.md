---
title: Seed Table Storage
published: true
description: 
tags: #devops #azure #microservices
---

# Introduction

I wrote a few sentence about an Azure Cloud [here](https://dev.to/meanin/create-azure-storage-account-on-release-pipeline-4kn4). When you are deploying an application, sometimes you need some basics data already be placed in a database. Some prerequisites, without which an application will work in a wrong manner. Seeding and migrating SQL databases in a .net world is a well described topic. If you know Entity Framework or Hibernate, you know where to start. What to do with this, when you want to store data in an Azure Table Storage?

# Install extension

To install this extension, you need an organization on Azure DevOps portal. You can start [here](https://azure.microsoft.com/en-us/services/devops/?nav=min). On this portal, you have to have rights to install extensions. Then navigate [here](https://marketplace.visualstudio.com/items?itemName=meanin.storage-account-managment).

# Configure task

![img](https://raw.githubusercontent.com/meanin/vsts-tasks/master/screenshots/seedtablestorage.png)

To use this task you have to have a [configured](https://docs.microsoft.com/en-us/azure/devops/pipelines/library/service-endpoints?view=vsts#sep-azure-rm) Azure Resource Manager connection with a Service Principal. Set a storage account name and a table name which you want to seed. Select a json file location. File have to be a json list of flat object, which everyone contains at least two properties: PrimaryKey and RowKey.
Here is a little example:
```
[
    {
        "partitionKey": "partitionKey1",
        "rowKey": "1",
        "otherColumn": "value1",
        "fourthColumn": "123"
    },
    {
        "partitionKey": "partitionKey1",
        "rowKey": "2",
        "otherColumn": "value2",
        "fourthColumn": "123"
    },
    {
        "partitionKey": "partitionKey2",
        "rowKey": "1",
        "otherColumn": "value1"
    },
    {
        "partitionKey": "partitionKey2",
        "rowKey": "2",
        "otherColumn": "value1"
    }
]
```
After release, table with data looks like below:
![img](https://raw.githubusercontent.com/meanin/dev-to-articles/master/img/2018-mm-dd-seed-table-storage/seed-table.png)

This is first published version of this task. I will appreciate all feedback :)