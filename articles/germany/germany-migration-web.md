---
title: Migración de recursos web de Azure desde Azure Alemania a Azure global
description: En este artículo se proporciona información sobre cómo migrar los recursos web de Azure desde Azure Alemania a Azure global.
author: gitralf
services: germany
cloud: Azure Germany
ms.author: ralfwi
ms.service: germany
ms.date: 08/15/2018
ms.topic: article
ms.custom: bfmigrate
ms.openlocfilehash: 4e607d821d0a587b25c6cb6929ac6765d500b6fb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033656"
---
# <a name="migrate-web-resources-to-global-azure"></a>Migración de recursos web a Azure global

En este artículo se proporciona información que puede ayudarle a migrar los recursos web de Azure desde Azure Alemania a Azure global.

## <a name="web-apps"></a>Web Apps

En este momento, no se admite la migración de aplicaciones que ha creado con la característica Web Apps de Azure App Service de Azure Alemania a Azure global. Se recomienda exportar la aplicación web como una plantilla de Azure Resource Manager. Después, vuelva a realizar la implementación después de cambiar la propiedad de ubicación a la nueva región de destino.

> [!IMPORTANT]
> Cambie la ubicación, los secretos de Azure Key Vault, los certificados y otros GUID para que sea coherente con la nueva región.

Para obtener más información:

- Actualice sus conocimientos completando los [tutoriales de App Service](https://docs.microsoft.com/azure/app-service/#step-by-step-tutorials).
- Obtenga información acerca de cómo [exportar las plantillas de Azure Resource Manager](../azure-resource-manager/manage-resource-groups-portal.md#export-resource-groups-to-templates).
- Revise la [introducción a Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
- Revise la [introducción a App Service](../app-service/overview.md).
- Consulte una [introducción a las ubicaciones de Azure](https://azure.microsoft.com/global-infrastructure/locations/).
- Obtenga información sobre cómo [volver a implementar una plantilla](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="notification-hubs"></a>Notification Hubs

Para migrar la configuración de una instancia de Azure Notification Hubs a otra, exporte e importe todos los tokens de registro con sus etiquetas:

1. [Exporte los registros existentes del Centro de notificaciones](/previous-versions/azure/azure-services/dn790624(v=azure.100)) a un contenedor de almacenamiento de blobs de Azure.
1. Cree un nuevo Centro de notificaciones en el entorno de destino.
1. [Importe los tokens de registro](/previous-versions/azure/azure-services/dn790624(v=azure.100)) desde el almacenamiento de blobs al nuevo Centro de notificaciones.

Para obtener más información:

- Actualice sus conocimientos completando los [tutoriales de Notification Hubs](https://docs.microsoft.com/azure/notification-hubs/#step-by-step-tutorials).
- Revise la [introducción a Azure Notification Hubs](../notification-hubs/notification-hubs-push-notification-overview.md).

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre herramientas, técnicas y recomendaciones para migrar los recursos en las siguientes categorías de servicio:

- [Proceso](./germany-migration-compute.md)
- [Redes](./germany-migration-networking.md)
- [Storage](./germany-migration-storage.md)
- [Bases de datos](./germany-migration-databases.md)
- [Analytics](./germany-migration-analytics.md)
- [IoT](./germany-migration-iot.md)
- [Integración](./germany-migration-integration.md)
- [Identidad](./germany-migration-identity.md)
- [Seguridad](./germany-migration-security.md)
- [Herramientas de administración](./germany-migration-management-tools.md)
- [Elementos multimedia](./germany-migration-media.md)
