---
title: Introducción a la identidad administrada de Service Fabric | Microsoft Docs
description: Este artículo es una introducción a la identidad administrada.
services: service-fabric
author: athinanthny
ms.service: service-fabric
ms.topic: conceptual
ms.date: 07/25/2019
ms.author: atsenthi
ms.openlocfilehash: edce98e6babb676ee72f1d254b929e557332dd75
ms.sourcegitcommit: 1d0b37e2e32aad35cc012ba36200389e65b75c21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333135"
---
# <a name="managed-identity-for-service-fabric-application-preview"></a>Identidad administrada para la aplicación de Service Fabric (versión preliminar)

Un desafío común al compilar aplicaciones en la nube consiste en el modo de administrar las credenciales del código para autenticar los servicios en la nube. Mantener las credenciales seguras es una tarea importante, ya que nunca aparecen en las estaciones de trabajo de los desarrolladores y no se controlan en el código fuente. La característica Identidad administrada para recursos de Azure en Azure Active Directory (Azure AD) resuelve este problema. Esta característica proporciona a los servicios de Azure una identidad administrada automáticamente en Azure AD. Puede usar esta identidad para autenticarse en cualquier servicio que admita la autenticación de Azure AD, incluido Key Vault, sin necesidad de credenciales en el código.

La característica Identidad administrada para recursos de Azure se incluye gratuitamente en Azure AD con las suscripciones de Azure. No hay costo adicional.

> [!NOTE]
> Identidad administrada para recursos de Azure es el nombre con el que ahora se conoce al servicio Managed Service Identity (MSI).

## <a name="terminology"></a>Terminología

Los siguientes términos se usan en la identidad administrada para el conjunto de documentación de los recursos de Azure:

- **Id. de cliente**: identificador único que genera Azure AD y que está asociado a una aplicación y entidad de servicio durante su aprovisionamiento inicial (consulte también [Identificador de aplicación](/azure/active-directory/develop/developer-glossary#application-id-client-id)).

- **Id. de entidad de seguridad**: identificador de objeto del objeto de entidad de servicio de la identidad administrada que se usa para conceder acceso basado en roles a los recursos de Azure.

- **Entidad de servicio**: objeto de Azure Active Directory que representa la proyección de una aplicación de AAD en un inquilino determinado (consulte también [Entidad de servicio](../active-directory/develop/developer-glossary.md#service-principal-object)).


## <a name="about-managed-identities-in-azure"></a>Acerca de las identidades administradas en Azure

- [Tipos de identidades administradas (MI) en Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview#how-does-the-managed-identities-for-azure-resources-work)

- [Funcionamiento de una identidad administrada asignada por el sistema en Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview#how-a-system-assigned-managed-identity-works-with-an-azure-vm)

- [Funcionamiento de una identidad administrada (MI) asignada por el usuario en Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview#how-a-user-assigned-managed-identity-works-with-an-azure-vm)


## <a name="supported-scenarios-for-service-fabric-applications"></a>Escenarios admitidos para las aplicaciones de Service Fabric

Las identidades administradas para Service Fabric solo se admiten en los clústeres de Service Fabric implementados de Azure y únicamente para las aplicaciones implementadas como recursos de Azure; no se puede asignar una identidad a una aplicación que no se implemente como recurso de Azure. En términos conceptuales, la compatibilidad con identidades administradas en el clúster de Azure Service Fabric consta de dos fases:

1. Asigne una o más identidades administradas al recurso de la aplicación; una aplicación puede tener asignada una única identidad asignada por el sistema y hasta 32 identidades asignadas por el usuario, respectivamente.

2. Dentro de la definición de la aplicación, asigne una de las identidades asignadas a la aplicación a cualquier servicio individual que contenga la aplicación.

La identidad asignada por el sistema de una aplicación es única de esa aplicación, mientras que una identidad asignada por el usuario es un recurso independiente, que se puede asignar a varias aplicaciones. Dentro de una aplicación, se puede asignar una identidad (ya sea asignada por el sistema o asignada por el usuario) a varios servicios de la aplicación, pero cada servicio individual solo puede tener asignada una identidad. Por último, se debe asignar una identidad explícitamente a un servicio para que tenga acceso a esta característica. De hecho, la asignación de las identidades de una aplicación a los servicios que la componen permite el aislamiento en la aplicación. Un servicio solo puede usar la identidad que se le ha asignado (y ninguna si no se le ha asignado una explícitamente).  

A continuación se muestra la lista de los escenarios admitidos para la versión preliminar:

   - Implementación de una nueva aplicación con uno o más servicios, y con una o varias identidades asignadas.

   - Asignación de una o varias identidades administradas a una aplicación existente para acceder a recursos de Azure; la aplicación debe haberse implementado como un recurso de Azure.


Los escenarios siguientes no se admiten o no se recomiendan. Tenga en cuenta que es posible que estas acciones no se bloqueen, pero podrían provocar interrupciones en las aplicaciones:

   - Eliminación o cambio de las identidades asignadas a una aplicación. Si debe realizar cambios, envíe implementaciones independientes para agregar primero una nueva asignación de identidad y, después, quitar una ya asignada. La eliminación de una identidad de una aplicación existente puede tener efectos no deseados, como dejar la aplicación en un estado que no se puede actualizar. Es seguro eliminar la aplicación por completo si es necesario quitar una identidad. Tenga en cuenta que de este modo se eliminará la identidad asignada por el sistema (si se ha definido) asociada a la aplicación y se quitarán todas las asociaciones con las identidades asignadas por el usuario que se han asignado a la aplicación.

   - En este momento, la compatibilidad de SF con las identidades administradas no se integra en [AzureServiceTokenProvider](../key-vault/service-to-service-authentication.md); la integración se logrará al final del período de versión preliminar de la característica de identidad administrada.

>
> [!NOTE]
>
> Esta característica está en versión preliminar. Como tal, puede estar sujeta a cambios frecuentes y podría no ser adecuada para implementaciones de producción.

## <a name="next-steps"></a>Pasos siguientes
* [Implementación de un nuevo clúster de Azure Service Fabric con compatibilidad con la identidad administrada](./configure-new-azure-service-fabric-enable-managed-identity.md) 
* [Habilitación de la compatibilidad con la identidad administrada en un clúster de Azure Service Fabric existente](./configure-existing-cluster-enable-managed-identity-token-service.md)
* [Implementación de una aplicación de Azure Service Fabric con una identidad administrada asignada por el sistema](./how-to-deploy-service-fabric-application-system-assigned-managed-identity.md)
* [Implementación de una aplicación de Azure Service Fabric con una identidad administrada asignada por el usuario](./how-to-deploy-service-fabric-application-user-assigned-managed-identity.md)
* [Aprovechamiento de la identidad administrada de una aplicación de Service Fabric desde el código de servicio](./how-to-managed-identity-service-fabric-app-code.md)
* [Concesión de acceso a otros recursos de Azure para una aplicación de Azure Service Fabric](./how-to-grant-access-other-resources.md)
