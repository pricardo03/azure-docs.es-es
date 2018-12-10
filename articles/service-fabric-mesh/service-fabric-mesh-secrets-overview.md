---
title: Administración y uso de secretos en aplicaciones de Azure Service Fabric Mesh | Microsoft Docs
description: Almacenamiento y uso de secretos de Service Fabric Mesh.
services: service-fabric-mesh
keywords: secrets
author: v-steg
ms.author: v-steg
ms.date: 10/25/2018
ms.topic: get-started-article
ms.service: service-fabric-mesh
manager: jeanpaul.connock
ms.openlocfilehash: 63ac34e184d537eba2b915d4d108c7c1d8368aba
ms.sourcegitcommit: 2bb46e5b3bcadc0a21f39072b981a3d357559191
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2018
ms.locfileid: "52891979"
---
# <a name="service-fabric-mesh-application-secrets"></a>Secretos en la aplicación Service Fabric Mesh
Service Fabric Mesh admite secretos como recursos de Azure. Un secreto de Service Fabric Mesh puede ser cualquier información de texto confidencial, como cadenas de conexión de almacenamiento, contraseñas u otros valores que deben almacenarse y transmitirse de forma segura.

![Información general de los secretos de Mesh][sf-mesh-secrets-overview]

## <a name="mesh-secrets-resources"></a>Recursos de los secretos de Mesh
Un secreto de la aplicación Mesh consta de:
* Un recurso **Secretos**, que es un contenedor que almacena secretos de texto. Los secretos contenidos dentro del recurso **Secretos** se almacenan y se transmiten de forma segura.
* Uno o varios recursos **Secretos o valores** que se almacenan en el contenedor de recursos **Secretos**. Cada recurso **Secretos o valores** se distingue por un número de versión.

## <a name="inline-or-stored-in-azure-key-vault"></a>Inserción o almacenamiento en Azure Key Vault
- Las aplicaciones Mesh y los recursos de servicio incluyen Managed Service Identity (MSI) con Azure Active Directory (AAD) para poder acceder a los secretos en Azure Key Vault.
- Los secretos y certificados se pueden transferir automáticamente con directivas.

## <a name="next-steps"></a>Pasos siguientes 
Para más información acerca de los secretos de Service Fabric Mesh, consulte:
- [Administración de secretos en aplicaciones de Service Fabric Mesh](service-fabric-mesh-howto-manage-secrets.md)
- [Introducción al modelo de recursos de Service Fabric](service-fabric-mesh-service-fabric-resources.md)

<!-- pics -->
[sf-mesh-secrets-overview]: ./media/service-fabric-mesh-secrets-overview/MeshAppSecretsOverview.png
