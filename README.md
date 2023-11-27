# Azure SDK Sample3 StorageAccount Operations

**NOTE:**
For general info about Azure SDK for .NET navigate to the following github repo: https://github.com/Azure/azure-sdk-for-net

```csharp
﻿using System;
using System.Threading.Tasks;
using Azure;
using Azure.Core;
using Azure.Identity;
using Azure.ResourceManager;
using Azure.ResourceManager.Resources;
using Azure.ResourceManager.Storage;
using Azure.ResourceManager.Storage.Models;

ArmClient armClient = new ArmClient(new DefaultAzureCredential());
SubscriptionResource subscription = await armClient.GetDefaultSubscriptionAsync();

string resourceId = "/subscriptions/846901e6-da09-45c8-98ca-7cca2353ff0e/resourceGroups/myRgNameLUISCOCO/providers/Microsoft.Storage/storageAccounts/myaccountluiscoco";
StorageAccountResource storageAccount = armClient.GetStorageAccountResource(new ResourceIdentifier(resourceId));

//Get keys on a storage account
await foreach (StorageAccountKey key in storageAccount.GetKeysAsync())
{
    Console.WriteLine(key.Value);
}

//Get a storage account
Console.WriteLine(storageAccount.Id.Name);

//Add a tag to the storage account
string resourceGroupName = "myRgNameLUISCOCO";
string StorageAccountName = "myaccountluiscoco";

ResourceGroupResource resourceGroup =
    armClient.GetDefaultSubscription().GetResourceGroup(resourceGroupName);
StorageAccountCollection accountCollection3 = resourceGroup.GetStorageAccounts();
StorageAccountResource storageAccount3 = await accountCollection3.GetAsync(StorageAccountName);

// add a tag on this storage account
await storageAccount3.AddTagAsync("key", "value");

//Delete a storage account
StorageAccountCollection accountCollection2 = resourceGroup.GetStorageAccounts();
StorageAccountResource storageAccount2 = await accountCollection2.GetAsync(StorageAccountName);
await storageAccount2.DeleteAsync(WaitUntil.Completed);
```
