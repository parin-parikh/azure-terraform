# Azure Terraform README

This README file provides an overview of my experience using Terraform to manage Azure resources. I'll walk through the steps for setting up Terraform, creating an Azure resource group and storage account, and managing these resources using various Terraform commands.

## Prerequisites

Before you begin, ensure you have the following:

1. An Azure account
2. Terraform installed on your local machine
3. Azure CLI installed and configured

## Step-by-Step Experience

### Step 1: Setting Up Your Terraform Configuration

1. **Create a new directory** for your Terraform configuration files.
   
   ```sh
   mkdir terraform-azure
   cd terraform-azure
   ```

2. **Create a file named `main.tf`** and add the following configuration to define a resource group and a storage account:

   ```hcl
   terraform {
     required_providers {
       azurerm = {
         source  = "hashicorp/azurerm"
         version = "3.105.0"
       }
     }
   }

   provider "azurerm" {
     features {
       resource_group {
         prevent_deletion_if_contains_resources = false
       }
     }
   }

   resource "azurerm_resource_group" "example" {
     name     = "my-terraform-rg"
     location = "West US"
   }

   resource "azurerm_storage_account" "example" {
     name                     = "terraformstorage324pp"
     resource_group_name      = azurerm_resource_group.example.name
     location                 = azurerm_resource_group.example.location
     account_tier             = "Standard"
     account_replication_type = "GRS"

     tags = {
       environment = "staging"
     }
   }
   ```

### Step 2: Initializing Terraform

I ran the `terraform init` command to initialize the directory containing the Terraform configuration files. This command downloads the Azure provider and sets up the environment.

```sh
terraform init
```

**Explanation**: `terraform init` initializes the Terraform working directory, downloading necessary provider plugins and setting up the backend for storing the state file.

### Step 3: Planning the Deployment

I ran the `terraform plan` command to create an execution plan. This plan shows what actions Terraform will take to achieve the desired state defined in the configuration files.

```sh
terraform plan
```

**Explanation**: `terraform plan` allows you to preview the changes that will be made to your infrastructure. It compares the current state with the desired state and shows a detailed list of actions that will be performed.

### Step 4: Applying the Configuration

I ran the `terraform apply` command to apply the changes required to reach the desired state of the configuration. This command prompts for confirmation before making any changes.

```sh
terraform apply
```

**Explanation**: `terraform apply` executes the actions proposed in the `terraform plan` step, creating or updating infrastructure resources as defined in the configuration files. 

### Step 5: Verifying the Deployment

After applying the configuration, I verified the resources in the Azure Portal to ensure they were created as expected.

1. Open the [Azure Portal](https://portal.azure.com/).
2. Navigate to "Resource groups" and find the resource group named `my-terraform-rg`.
3. Inside the resource group, verify the presence of the storage account named `terraformstorage324pp`.

### Step 6: Destroying the Resources

When I no longer needed the resources, I ran the `terraform destroy` command to remove all the resources defined in the Terraform configuration.

```sh
terraform destroy
```

**Explanation**: `terraform destroy` removes all the resources that are defined in the Terraform configuration files. This command is useful for cleaning up your environment and ensuring that you are not incurring any unexpected costs.

## Summary

In this experience, I learned how to use Terraform to manage Azure resources. I set up a Terraform configuration, initialized Terraform, planned and applied the configuration, verified the deployment in the Azure Portal, and destroyed the resources when they were no longer needed.
