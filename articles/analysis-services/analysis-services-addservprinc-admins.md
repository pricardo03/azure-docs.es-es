---
title: Incorporación de una entidad de servicio al rol de administrador de Azure Analysis Services | Microsoft Docs
description: Aprenda cómo agregar una entidad de servicio de automatización al rol de administrador del servidor de Azure Analysis Services
author: minewiskan
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 10/29/2019
ms.author: owend
ms.reviewer: minewiskan
ms.custom: fasttrack-edit
ms.openlocfilehash: b75740e9bff714ad68c93bea7e387e60da2f1c59
ms.sourcegitcommit: 0eb0673e7dd9ca21525001a1cab6ad1c54f2e929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2020
ms.locfileid: "77212494"
---
# <a name="add-a-service-principal-to-the-server-administrator-role"></a>Incorporación de una entidad de servicio al rol de administrador del servidor 

 Para automatizar las tareas de PowerShell desatendidas, una entidad de servicio debe tener privilegios de **administrador del servidor** en el servidor de Analysis Services que se está administrando. En este artículo se describe cómo agregar una entidad de servicio al rol de administrador de servidor en un servidor de Azure AS. Puede hacerlo mediante SQL Server Management Studio o una plantilla de Resource Manager.
 
> [!NOTE]
> En las operaciones de servidor que usan cmdlets de Azure PowerShell, la entidad de servicio también debe pertenecer al rol **Propietario** del recurso en el [control de acceso basado en rol (RBAC) de Azure](../role-based-access-control/overview.md). 

## <a name="before-you-begin"></a>Antes de empezar
Antes de completar esta tarea, debe tener registrada una entidad de servicio en Azure Active Directory.

[Creación de una entidad de servicio: Azure Portal](../active-directory/develop/howto-create-service-principal-portal.md)   
[Creación de una entidad de servicio: PowerShell](../active-directory/develop/howto-authenticate-service-principal-powershell.md)

## <a name="using-sql-server-management-studio"></a>Uso de SQL Server Management Studio

Puede configurar administradores de servidor mediante SQL Server Management Studio (SSMS). Para completar esta tarea, debe tener permisos de [administrador del servidor](analysis-services-server-admins.md) en el servidor Azure AS. 

1. En SSMS, conéctese a su servidor Azure AS.
2. En **Propiedades del servidor** > **Seguridad**, haga clic en **Agregar**.
3. En **Seleccione un usuario o grupo**, busque la aplicación registrada por su nombre, selecciónela y, a continuación, haga clic en **Agregar**.

    ![Búsqueda de la cuenta de la entidad de servicio](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-picker.png)

4. Compruebe el identificador de la cuenta de la entidad de servicio y, a continuación, haga clic en **Aceptar**.
    
    ![Búsqueda de la cuenta de la entidad de servicio](./media/analysis-services-addservprinc-admins/aas-add-sp-ssms-add.png)

## <a name="using-a-resource-manager-template"></a>Uso de una plantilla de Resource Manager

También puede configurar administradores de servidor si implementa el servidor de Analysis Services mediante una plantilla de Azure Resource Manager. La identidad que ejecuta la implementación debe pertenecer al rol **Colaborador** del recurso en el [control de acceso basado en rol (RBAC) de Azure](../role-based-access-control/overview.md).

> [!IMPORTANT]
> La entidad de servicio se debe agregar con el formato `app:{service-principal-client-id}@{azure-ad-tenant-id}`.

La siguiente plantilla de Resource Manager implementa un servidor de Analysis Services con una entidad de servicio especificada agregada al rol Administrador de Analysis Services:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "analysisServicesServerName": {
            "type": "string"
        },
        "location": {
            "type": "string"
        },
        "analysisServicesSkuName": {
            "type": "string"
        },
        "analysisServicesCapacity": {
            "type": "int"
        },
        "servicePrincipalClientId": {
            "type": "string"
        },
        "servicePrincipalTenantId": {
            "type": "string"
        }
    },
    "resources": [
        {
            "name": "[parameters('analysisServicesServerName')]",
            "type": "Microsoft.AnalysisServices/servers",
            "apiVersion": "2017-08-01",
            "location": "[parameters('location')]",
            "sku": {
                "name": "[parameters('analysisServicesSkuName')]",
                "capacity": "[parameters('analysisServicesCapacity')]"
            },
            "properties": {
                "asAdministrators": {
                    "members": [
                        "[concat('app:', parameters('servicePrincipalClientId'), '@', parameters('servicePrincipalTenantId'))]"
                    ]
                }
            }
        }
    ]
}
```

## <a name="related-information"></a>Información relacionada

* [Descarga del módulo de PowerShell de SQL Server](https://docs.microsoft.com/sql/ssms/download-sql-server-ps-module)   
* [Descarga de SSMS](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms)   


