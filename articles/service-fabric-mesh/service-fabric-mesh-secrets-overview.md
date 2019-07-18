---
title: Administración y uso de secretos en aplicaciones de Azure Service Fabric Mesh | Microsoft Docs
description: Almacenamiento y uso de secretos de Service Fabric Mesh.
services: service-fabric-mesh
keywords: secrets
author: v-steg
ms.author: jeconnoc
ms.date: 10/25/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: jeconnoc
ms.openlocfilehash: b5cfa93298222d914069b6ab63deb8ba8a9b59c3
ms.sourcegitcommit: de47a27defce58b10ef998e8991a2294175d2098
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/15/2019
ms.locfileid: "67875343"
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
