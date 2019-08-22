---
title: 'Azure Service Fabric: Implementación de un nuevo clúster de Azure Service Fabric con compatibilidad con identidad administrada | Microsoft Docs'
description: En este artículo se muestra cómo crear un nuevo clúster de Service Fabric con la identidad administrada habilitada.
services: service-fabric
author: athinanthny
ms.service: service-fabric
ms.topic: article
ms.date: 07/25/2019
ms.author: atsenthi
ms.openlocfilehash: 24fcdaf612a26109194524733e1fb9069dc664e0
ms.sourcegitcommit: 5d6c8231eba03b78277328619b027d6852d57520
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68965569"
---
# <a name="create-a-new-azure-service-fabric-cluster-with-managed-identity-support"></a>Creación de un nuevo clúster de Azure Service Fabric con compatibilidad con la identidad administrada

Para acceder a la característica de identidad administrada para las aplicaciones de Azure Service Fabric, primero debe habilitar el servicio de token de identidad administrada en el clúster. Este servicio es responsable de la autenticación de aplicaciones de Service Fabric que usan sus identidades administradas y de la obtención de los tokens de acceso en su nombre. Una vez habilitado el servicio, puede verlo en Service Fabric Explorer, bajo la sección **Sistema** del panel izquierdo. Se ejecuta con el nombre **fabric:/System/ManagedIdentityTokenService** junto a otros servicios del sistema.

> [!NOTE]
> Se requiere un entorno de ejecución de Service Fabric versión 6.5.658.9590 o posterior para habilitar el **servicio de token de identidad administrada**.  

## <a name="enable-the-managed-identity-token-service"></a>Habilitación del servicio de token de identidad administrada 
Para habilitar el servicio de token de identidad administrada durante la creación del clúster, puede usar el siguiente fragmento de código en una plantilla de Azure Resource Manager:

```json
"fabricSettings": [
    {
        "name": "ManagedIdentityTokenService",
        "parameters": [
            {
                "name": "IsEnabled",
                "value": "true"
            }
        ]
    }
]
```

## <a name="errors"></a>Errors

Si se produce un error en la implementación con este mensaje, significa que el clúster no está en la versión de Service Fabric necesaria (el entorno de ejecución mínimo admitido es 6.5 CU2):



```json
{
    "code": "ParameterNotAllowed",
    "message": "Section 'ManagedIdentityTokenService' and Parameter 'IsEnabled' is not allowed."
}
```

## <a name="next-steps"></a>Pasos siguientes
* [Implementación de una aplicación de Azure Service Fabric con una identidad administrada asignada por el sistema](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)
* [Implementación de una aplicación de Azure Service Fabric con una identidad administrada asignada por el usuario](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
* [Aprovechamiento de la identidad administrada de una aplicación de Service Fabric desde el código de servicio](./how-to-managed-identity-service-fabric-app-code.md)
* [Concesión de acceso a otros recursos de Azure para una aplicación de Azure Service Fabric](./how-to-grant-access-other-resources.md)

## <a name="related-articles"></a>Artículos relacionados
* Revisión de la [compatibilidad con la identidad administrada](./concepts-managed-identity.md) en Azure Service Fabric

* [Habilitación de la compatibilidad con la identidad administrada en un clúster de Azure Service Fabric existente](./configure-existing-cluster-enable-managed-identity-token-service.md)
