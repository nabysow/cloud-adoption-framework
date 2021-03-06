---
title: Moodle migration tasks, architecture, and template
description: Learn about the task and architecture, and template involved in a Moodle migration.
author: BrianBlanchard
ms.author: brblanch
ms.date: 11/06/2020
ms.topic: conceptual
ms.service: cloud-adoption-framework
ms.subservice: plan
---

# Moodle migration tasks, architecture, and template

## Moodle migration task outline

Moodle migrations include the following tasks:

- Deploy Azure infrastructure with Azure Resource Manager templates.
- Download and install AzCopy.
- Copy over the backup archive to the Controller Virtual Machine instance from the Azure Resource Manager deployment.
- Migration of Moodle application and configuration.
- Set up Moodle controller instance and worker nodes.
- Configuring PHP and the web server.

## Deploy Azure infrastructure with Azure Resource Manager templates

- When using an Azure Resource Manager template to deploy infrastructure on Azure, a couple of options are available to you. The following diagram provides an overview of infrastructure resources.

![Azure infrastructure resources.](images/architecture.png)

A fully configurable deployment gives more flexibility and choices for deployments. A predefined deployment size uses one of four predefined Moodle sizes. The four predefined templates options are minimal, short-to-mid, large, and maximal, and they're available at the [Moodle GitHub repository](https://github.com/Azure/Moodle).

- [Minimal](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FMoodle%2Fmaster%2Fazuredeploy-minimal.json): This deployment will use NFS, MySQL, and smaller auto scale web front-end virtual machine sku (one vCore) that will give faster deployment time (less than 30 minutes) and currently requires only two virtual machines will fit into an Azure free trial subscription.

- [Small-to-mid](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FMoodle%2Fmaster%2Fazuredeploy-small2mid-noha.json): Supports up to 1,000 concurrent users. This deployment will use NFS (no high availability) and MySQL (eight vCores) without other options like elastic search or Redis cache.

- [Large (high-availability)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FMoodle%2Fmaster%2Fazuredeploy-large-ha.json): Supports more than 2,000 concurrent users. This deployment will use Azure Files, MySQL (16 vCores), and Redis cache without other options like elastic search.

- [Maximum](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2FMoodle%2Fmaster%2Fazuredeploy-maximal.json): This maximal deployment will use Azure Files, MySQL with the highest SKU, Redis cache, elastic search (three virtual machines), and large storage sizes (both data disks and databases).

Select **Launch** to deploy any of the predefined templates. This will direct you to the Azure portal, where you'll need to complete mandatory fields such as **Subscription**, **Resource Group**, [**SSH key**](https://docs.github.com/en/free-pro-team@latest/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent), and **Region**.

![Custom deployment: Deploy from a custom template.](images/custom-deployment.png)

The preceding predefined templates will deploy the default versions.

```bash
Ubuntu: 18.04-LTS
PHP: 7.4
Moodle: 3.8
```

If the PHP and Moodle versions are lagging with on-premises, then update the versions with following steps:

- Select **Edit Template** on the **Custom deployment** page.

![Edit template: Edit your Azure Resource Manager template.](images/edit-template.png)

- In the **Resources** section, add the Moodle and PHT versions to the **Parameters** block.

    ```json
    "phpVersion":       { "value": "7.2" },
    "moodleVersion":    { "value": "MOODLE_38_STABLE"}
    ```

- For Moodle 3.9, the value should be `MOODLE_39_STABLE`.

- Select **Save** to save your changes.

## Next steps

Continue to [Moodle migration resources](./migration-resources.md) for more information about the Moodle migration process.
