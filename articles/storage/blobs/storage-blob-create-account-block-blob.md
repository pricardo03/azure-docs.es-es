---
title: 'Crear una cuenta de almacenamiento de blob en bloques: Azure Storage| Microsoft Docs'
description: Aquí se muestra cómo crear una cuenta de almacenamiento de blob en bloques de Azure con las características de rendimiento Premium.
author: tamram
services: storage
ms.service: storage
ms.topic: conceptual
ms.date: 03/23/2019
ms.author: tamram
ms.subservice: blobs
ms.openlocfilehash: 9d8fb8f5f470dc47088efb30b7f823a0b8c624c8
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65141015"
---
# <a name="create-a-block-blob-storage-account"></a>Creación de una cuenta de almacenamiento de blobs en bloques

El tipo de cuenta de almacenamiento de blob en bloques le permite crear blobs en bloques con características de rendimiento Premium. Este tipo de cuenta de almacenamiento está optimizado para cargas de trabajo con altas tasas de transacciones o que requieren tiempos de acceso muy rápidos. En este artículo se muestra cómo crear una cuenta de almacenamiento de blob en bloques mediante Azure Portal, la CLI de Azure o Azure PowerShell.

Para obtener más información sobre las cuentas de almacenamiento de blob en bloques, consulte [Introducción a las cuentas de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-account-overview).

## <a name="create-account-in-the-azure-portal"></a>Crear una cuenta en Azure Portal

Para crear una cuenta de almacenamiento de blob en bloques en Azure Portal, siga estos pasos:

1. En Azure Portal, seleccione **Todos los servicios** > la categoría **Almacenamiento** > **Cuentas de almacenamiento**.

1. En **Cuentas de almacenamiento**, seleccione **Agregar**.

1. Seleccione la suscripción en la que se va a crear la cuenta de almacenamiento en el campo **Suscripción**.

1. En el campo del **Grupo de recursos**, seleccione un grupo de recursos existente o **Crear nuevo** y escriba un nombre para el grupo de recursos.

1. En el campo **Nombre de la cuenta de almacenamiento**, escriba un nombre para la cuenta. Tenga en cuenta estas directrices:

   - El nombre debe ser único en Azure.
   - El nombre debe tener entre tres y 24 caracteres.
   - El nombre solo puede contener números y letras minúsculas.

1. En el campo **Ubicación**, seleccione una ubicación para la cuenta de almacenamiento o utilice la ubicación predeterminada.

1. Defina la configuración de los siguientes valores:

   |Campo     |Valor  |
   |---------|---------|
   |**Rendimiento**    |  Seleccione **Premium**.   |
   |**Tipo de cuenta**    | Seleccione **BlockBlobStorage**.      |
   |**Replicación**    |  Deje el valor predeterminado de **Almacenamiento con redundancia local (LRS)** .      |

   ![Se muestra la interfaz de usuario del portal para crear una cuenta de almacenamiento de blob en bloques](media/storage-blob-create-account-block-blob/create-block-blob-storage-account.png)

1. Seleccione **Revisar y crear** para revisar la configuración de la cuenta de almacenamiento.

1. Seleccione **Crear**.

## <a name="create-account-using-azure-powershell"></a>Creación de una cuenta con Azure PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

1. Abra una sesión de Windows PowerShell con privilegios elevados (ejecución como administrador).

1. Ejecute el siguiente comando para asegurarse de que la versión más reciente del módulo de PowerShell `Az` esté instalada.

   ```powershell
   Install-Module -Name Az -AllowClobber
   ```

1. Abra una nueva consola de PowerShell e inicie sesión con su cuenta de Azure.

   ```powershell
   Connect-AzAccount -SubscriptionId <SubscriptionID>
   ```

1. Si es necesario, cree un grupo de recursos. Reemplace los valores por expresiones de código delimitadas y ejecute el siguiente comando.

   ```powershell
   $resourcegroup = "new_resource_group_name"
   $location = "region_name"
   New-AzResourceGroup -Name $resourceGroup -Location $location
   ```

1. Cree una cuenta de almacenamiento de blob en bloques. Reemplace los valores por expresiones de código delimitadas y ejecute el siguiente comando.

   ```powershell
   $resourcegroup = "resource_group_name"
   $storageaccount = "new_storage_account_name"
   $location = "region_name"

   New-AzStorageAccount -ResourceGroupName $resourcegroup -Name $storageaccount -Location $location -Kind "BlockBlobStorage" -SkuName "Premium_LRS"
   ```

## <a name="create-account-using-azure-cli"></a>Crear una cuenta con la CLI de Azure

Para crear una cuenta de blob en bloques con la CLI de Azure, debe instalar primero la CLI de Azure en su versión 2.0.46 o posterior. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

1. Inicie sesión en la suscripción de Azure.

   ```azurecli
   az login
   ```

1. Si es necesario, cree un grupo de recursos. Reemplace los valores entre corchetes (incluidos los corchetes) y ejecute el siguiente comando.

   ```azurecli
   az group create \
    --name "<new_resource_group_name>" \
    --location "<location>"
   ```

1. Cree una cuenta de almacenamiento de blob en bloques. Reemplace los valores entre corchetes (incluidos los corchetes) y ejecute el siguiente comando.

   ```azurecli
   az storage account create \
    --location "<location>" \
    --name "<new_storage_account_name>" \
    --resource-group "<resource_group_name>" \
    --kind "BlockBlobStorage" \
    --sku "Premium_LRS"
   ```

## <a name="next-steps"></a>Pasos siguientes

- Para más información sobre las cuentas de almacenamiento, vea [Introducción a las cuentas de Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-account-overview).

- Para más información sobre los grupos de recursos, consulte [Información general de Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview).
