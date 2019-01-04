---
title: Use Azure Key Vault con Managed Applications | Microsoft Docs
description: Muestra cómo usar los secretos de acceso de Azure Key Vault al implementar Managed Applications
services: managed-applications
author: tfitzmac
manager: timlt
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.date: 07/11/2018
ms.author: tomfitz
ms.openlocfilehash: dcfbb7f3b1d110d4c1fdf22863d795c85152ec35
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2018
ms.locfileid: "52725019"
---
# <a name="access-key-vault-secret-when-deploying-azure-managed-applications"></a>Acceso al secreto de Key Vault al implementar Azure Managed Applications

Cuando tiene que pasar un valor seguro (por ejemplo, una contraseña) como un parámetro durante la implementación, puede recuperar el valor de [Azure Key Vault](../key-vault/key-vault-whatis.md). Para acceder a Key Vault cuando se implementa Managed Applications, debe conceder acceso a la entidad de servicio del **proveedor de recursos de dispositivos**. En este artículo se describe cómo configurar Key Vault para que funcione con Managed Applications.

## <a name="enable-template-deployment"></a>Habilitar la implementación de plantillas

1. En el portal, seleccione su instancia de Key Vault.

1. Seleccione **Directivas de acceso**.   

   ![Seleccionar directivas de acceso](./media/key-vault-access/select-access-policies.png)

1. Seleccione **Haga clic para mostrar las directivas de acceso avanzado**.

   ![Mostrar directivas de acceso avanzado](./media/key-vault-access/advanced.png)

1. Seleccione **Habilitar el acceso a Azure Resource Manager para la implementación de plantillas**. Después, seleccione **Guardar**.

   ![Habilitar la implementación de plantillas](./media/key-vault-access/enable-template.png)

## <a name="add-service-as-contributor"></a>Agregar servicio como colaborador

1. Seleccione **Access Control (IAM)**.

   ![Selección del control de acceso](./media/key-vault-access/access-control.png)

1. Seleccione **Agregar asignación de roles**.

   ![Seleccione Agregar](./media/key-vault-access/add-access-control.png)

1. Seleccione **Colaborador** para el rol. Busque el **proveedor de recursos de dispositivos** y selecciónelo entre las opciones disponibles.

   ![Búsqueda de proveedor](./media/key-vault-access/search-provider.png)

1. Seleccione **Guardar**.

## <a name="reference-key-vault-secret"></a>Referencia a secreto de Key Vault

Para pasar un secreto desde Key Vault a una plantilla de la aplicación administrada, debe usar una [plantilla vinculada](../azure-resource-manager/resource-group-linked-templates.md) y hacer referencia a Key Vault en los parámetros de la plantilla vinculada. Proporcione el identificador de recurso de Key Vault y el nombre del secreto.

```json
"resources": [{
  "apiVersion": "2015-01-01",
  "name": "linkedTemplate",
  "type": "Microsoft.Resources/deployments",
  "properties": {
    "mode": "incremental",
    "templateLink": {
      "uri": "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/azure-resource-manager/keyvaultparameter/sqlserver.json",
      "contentVersion": "1.0.0.0"
    },
    "parameters": {
      "adminPassword": {
        "reference": {
          "keyVault": {
            "id": "/subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.KeyVault/vaults/<key-vault-name>"
          },
          "secretName": "<secret-name>"
        }
      },
      "adminLogin": { "value": "[parameters('adminLogin')]" },
      "sqlServerName": {"value": "[parameters('sqlServerName')]"}
    }
  }
}],
```

## <a name="next-steps"></a>Pasos siguientes

Ha configurado Key Vault para que esté accesible durante la implementación de una aplicación administrada.

* Para obtener información acerca de cómo pasar un valor de Key Vault como parámetro de plantilla, consulte [Uso de Azure Key Vault para pasar el valor de parámetro seguro durante la implementación](../azure-resource-manager/resource-manager-keyvault-parameter.md).
* Para ver ejemplos de aplicaciones administradas, consulte [Sample projects for Azure managed applications](sample-projects.md) (Ejemplos de proyectos para aplicaciones administradas de Azure).
* Para aprender a crear un archivo de definición de interfaz de usuario para una aplicación administrada, consulte [Introducción a CreateUiDefinition](create-uidefinition-overview.md).