---
title: Administración y uso de secretos en aplicaciones de Azure Service Fabric Mesh | Microsoft Docs
description: Almacenamiento y uso de secretos de Service Fabric Mesh.
services: service-fabric-mesh
keywords: secrets
author: v-steg
ms.author: v-steg
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: jeanpaul.connock
ms.openlocfilehash: 4268356db5f46e92862e19d6391cfe5a94511270
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60810658"
---
# <a name="service-fabric-mesh-application-secrets"></a>Secretos en la aplicación Service Fabric Mesh
Service Fabric Mesh admite secretos como recursos de Azure. Un secreto de Service Fabric Mesh puede ser cualquier información de texto confidencial, como cadenas de conexión de almacenamiento, contraseñas u otros valores que deben almacenarse y transmitirse de forma segura.

![Información general de los secretos de Mesh][sf-mesh-secrets-overview]

## <a name="mesh-secrets-resources"></a>Recursos de los secretos de Mesh
Un secreto de la aplicación Mesh consta de:
* Un recurso **Secretos**, que es un contenedor que almacena secretos de texto. Los secretos contenidos dentro del recurso **Secretos** se almacenan y se transmiten de forma segura.
* Uno o varios recursos **Secretos o valores** que se almacenan en el contenedor de recursos **Secretos**. Cada recurso **Secretos o valores** se distingue por un número de versión.

## <a name="next-steps"></a>Pasos siguientes 
Para más información acerca de los secretos de Service Fabric Mesh, consulte:
- [Administración de secretos en aplicaciones de Service Fabric Mesh](service-fabric-mesh-howto-manage-secrets.md)
- [Introducción al modelo de recursos de Service Fabric](service-fabric-mesh-service-fabric-resources.md)

<!-- pics -->
[sf-mesh-secrets-overview]: ./media/service-fabric-mesh-secrets-overview/MeshAppSecretsOverview.png
