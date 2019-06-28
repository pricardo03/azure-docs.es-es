---
title: Migración de las herramientas de administración de Azure desde Azure Alemania a Azure global
description: En este artículo se proporciona información sobre cómo migrar las herramientas de administración de Azure desde Azure Alemania a Azure global.
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 08/15/2018
ms.topic: article
ms.custom: bfmigrate
ms.openlocfilehash: bc237d0f78c77a6fbc89db4061ca38403c3d777b
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033660"
---
# <a name="migrate-management-tool-resources-to-global-azure"></a>Migración de los recursos de las herramientas de administración en Azure global

En este artículo se proporciona información que puede ayudarle a migrar las herramientas de administración de Azure desde Azure Alemania a Azure global.

## <a name="traffic-manager"></a>Traffic Manager

Azure Traffic Manager puede ayudarle a realizar una migración perfecta. Sin embargo, no puede migrar los perfiles de Traffic Manager que cree en Azure Alemania en Azure global. (Durante una migración, se migran los puntos de conexión de Traffic Manager al entorno de destino, por lo que se necesita actualizar el perfil de Traffic Manager de todas formas).

Puede definir puntos de conexión adicionales en el entorno de destino mediante Traffic Manager mientras se ejecuta en el entorno de origen. Cuando Traffic Manager se ejecuta en el nuevo entorno, todavía puede definir los puntos de conexión que aún no han migrado en el entorno de origen. Este escenario se conoce como [escenario Blue-Green](https://azure.microsoft.com/blog/blue-green-deployments-using-azure-traffic-manager/). Este escenario implica los pasos siguientes:

1. Cree un nuevo perfil de Traffic Manager en Azure global.
1. Defina los puntos de conexión en Azure Alemania.
1. Cambie el registro CNAME de DNS en el nuevo perfil de Traffic Manager.
1. Desactive el perfil de Traffic Manager antiguo.
1. Migre y configure los puntos de conexión. Para cada punto de conexión en Azure Alemania:
   1. Migre el punto de conexión a Azure global.
   1. Cambie el perfil de Traffic Manager para usar el nuevo punto de conexión.

Para obtener más información:

- Actualice sus conocimientos completando los [tutoriales de Traffic Manager](https://docs.microsoft.com/azure/traffic-manager/#step-by-step-tutorials).
- Revise la [introducción a Traffic Manager](../traffic-manager/traffic-manager-overview.md).
- Aprenda a [crear un perfil de Traffic Manager](../traffic-manager/traffic-manager-create-profile.md).
- Obtenga información sobre el [escenario Blue-Green](https://azure.microsoft.com/blog/blue-green-deployments-using-azure-traffic-manager/).

## <a name="backup"></a>Copia de seguridad

No puede migrar los trabajos de Azure Backup y las instantáneas desde Azure Alemania a Azure global.

Para obtener más información: 

- Actualice sus conocimientos completando los [tutoriales de Backup](https://docs.microsoft.com/azure/backup/#step-by-step-tutorials).
- Revise la [introducción a Azure Backup](../backup/backup-introduction-to-azure-backup.md).

## <a name="scheduler"></a>Scheduler

Azure Scheduler está en desuso. Utilice Azure Logic Apps para crear trabajos de programación.

Para obtener más información:

- Familiarícese con las características de Azure Logic Apps completando los [tutoriales de Logic Apps](https://docs.microsoft.com/azure/logic-apps/#step-by-step-tutorials).
- Revise la [introducción a Logic Apps](../logic-apps/logic-apps-overview.md).

## <a name="network-watcher"></a>Network Watcher

En este momento no se admite la migración de una instancia de Azure Network Watcher desde Azure Alemania a Azure global. Se recomienda crear y configurar una nueva instancia de Network Watcher en Azure global. A continuación, compare los resultados entre los entornos antiguo y nuevo. 

Para obtener más información:

- Actualice sus conocimientos completando los [tutoriales de Network Watcher](https://docs.microsoft.com/azure/network-watcher/#step-by-step-tutorials).
- Revise la [introducción a Network Watcher](../network-watcher/network-watcher-monitoring-overview.md).
- Más información sobre los [registros de flujo de los grupos de seguridad de red](../network-watcher/network-watcher-nsg-flow-logging-portal.md).
- Obtenga información sobre el [monitor de conexión](../network-watcher/connection-monitor.md).

## <a name="site-recovery"></a>Site Recovery

No se puede migrar la configuración actual de Azure Site Recovery en Azure global. Debe configurar una nueva solución de Site Recovery en Azure global.

Para obtener más información sobre Site Recovery y cómo aprender a migrar las máquinas virtuales de Azure Alemania a Global Azure, consulte [Uso de Site Recovery](./germany-migration-compute.md#compute-iaas).

Actualice sus conocimientos completando estos tutoriales paso a paso:

- [Recuperación ante desastres de Azure a Azure](https://docs.microsoft.com/azure/site-recovery/#azure-to-azure)
- [Recuperación ante desastres de VMware en Azure](https://docs.microsoft.com/azure/site-recovery/#vmware)
- [Recuperación ante desastres de Hyper en Azure](https://docs.microsoft.com/azure/site-recovery/#hyper-v)

## <a name="azure-policies"></a>Directivas de Azure

No puede migrar directamente directivas de Azure Alemania a Azure global. Durante una migración, normalmente cambia el ámbito de las directivas asignadas. Esto se aplica especialmente cuando la suscripción es diferente en el entorno de destino, como en este escenario. Sin embargo, puede conservar las definiciones de directiva y volver a usarlas en Azure global.

En la CLI de Azure, ejecute el siguiente comando para enumerar todas las directivas en el entorno actual.

> [!NOTE]
> No olvide cambiar el entorno AzureGermanCloud en la CLI de Azure antes de ejecutar los comandos siguientes.


```azurecli
az policy definition list --query '[].{Type:policyType,Name:name}' --output table
```

Exporte solo las directivas que tengan **personalizado** como valor de **PolicyType**. Exporte **policyRule** a un archivo. En el ejemplo siguiente se exporta la directiva personalizada "Solo permitir Centro de Alemania" (versión abreviada: `allowgconly`) a un archivo en la carpeta actual: 

```azurecli
az policy definition show --name allowgconly --output json --query policyRule > policy.json
```

El archivo de exportación será similar al ejemplo siguiente:

```json
{
  "if": {
    "not": {
      "equals": "germanycentral",
      "field": "location"
    }
  },
  "then": {
    "effect": "Deny"
  }
}
```

A continuación, cambie el entorno de Azure global. Modifique la regla de directiva mediante la edición del archivo. Por ejemplo, cambie `germanycentral` a `westeurope`.

```json
{
  "if": {
    "not": {
      "equals": "westeurope",
      "field": "location"
    }
  },
  "then": {
    "effect": "Deny"
  }
}
```

Creación de una nueva directiva:

```azurecli
cat policy.json |az policy definition create --name "allowweonly" --rules @-
```

Ahora tiene una nueva directiva denominada `allowweonly`. La directiva solo permite Oeste de Europa como región.

Asigne la directiva a los ámbitos en el nuevo entorno según corresponda. Para documentar las asignaciones antiguas de Azure Alemania, ejecute el comando siguiente:

```azurecli
az policy assignment list
```

Para obtener más información:

- Actualice sus conocimientos completando el [tutorial de las directivas de Azure](../governance/policy/tutorials/create-and-manage.md).
- Obtenga información sobre cómo [ver las directivas mediante la CLI de Azure](../governance/policy/tutorials/create-and-manage.md#view-policy-definitions-with-azure-cli) o [ver las directivas mediante el uso de PowerShell](../governance/policy/tutorials/create-and-manage.md#view-policy-definitions-with-powershell).
- Obtenga información sobre cómo [crear una definición de directiva mediante la CLI de Azure](../governance/policy/tutorials/create-and-manage.md#create-a-policy-definition-with-azure-cli) o cómo [crear una definición de directiva mediante PowerShell](../governance/policy/tutorials/create-and-manage.md#create-a-policy-definition-with-powershell).

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre herramientas, técnicas y recomendaciones para migrar los recursos en las siguientes categorías de servicio:

- [Proceso](./germany-migration-compute.md)
- [Redes](./germany-migration-networking.md)
- [Storage](./germany-migration-storage.md)
- [Web](./germany-migration-web.md)
- [Bases de datos](./germany-migration-databases.md)
- [Analytics](./germany-migration-analytics.md)
- [IoT](./germany-migration-iot.md)
- [Integración](./germany-migration-integration.md)
- [Identidad](./germany-migration-identity.md)
- [Seguridad](./germany-migration-security.md)
- [Elementos multimedia](./germany-migration-media.md)
