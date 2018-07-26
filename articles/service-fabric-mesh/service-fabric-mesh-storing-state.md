---
title: Opciones de almacenamiento de estado en Azure Service Fabric mesh | Microsoft Docs
description: Obtenga información sobre el almacenamiento confiable del estado en las aplicaciones de Service Fabric mesh que se ejecutan en Azure Service Fabric mesh.
services: service-fabric-mesh
keywords: ''
author: rwike77
ms.author: ryanwi
ms.date: 07/12/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 6aa268cf56bfb8be9c27a9e0d9e5c9f4464b0c9d
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39075942"
---
# <a name="state-management-with-service-fabric"></a>Administración del estado con Service Fabric
Service Fabric admite muchas opciones distintas para el almacenamiento de estado. Para obtener información general conceptual de los patrones de administración de estado y Service Fabric, consulte [Conceptos de Service Fabric: estado](/azure/service-fabric/service-fabric-concepts-state). Estos mismos conceptos se aplican tanto si los servicios se ejecutan dentro de Service Fabric mesh como fuera. 

## <a name="state-storage-options-in-azure-service-fabric-mesh"></a>Opciones de almacenamiento de estado en Azure Service Fabric mesh
Con Service Fabric mesh, puede implementar una nueva aplicación fácilmente y conectarla a un almacén de datos existente hospedado en Azure. Además de usar cualquier base de datos remota, hay varias opciones para almacenar datos, dependiendo de si el servicio desea almacenamiento local o remoto. 

* Datos replicados almacenados localmente
  * Colecciones confiables (no disponibles en versión preliminar)
    * Una biblioteca que implementa las estructuras de datos, como las colas y los pares clave-valor que se usarán en el servicio
    * Esto proporciona la manera más fácil y rápida para interactuar con datos, al mismo tiempo que proporciona un enrutamiento de particiones fácil en combinación con el Enrutamiento inteligente de Service Fabric mesh
  * Controlador de volumen de Service Fabric (no disponible en versión preliminar)
    * Un controlador de volumen de Docker para montar un volumen local en un contenedor
    * Esto proporciona la máxima flexibilidad para almacenar datos localmente, mediante cualquier API, lo que admite el almacenamiento de archivos.

* Almacenamiento remoto
  * Controlador de volumen de Azure Files
    * Un controlador de volumen de Docker para montar un recurso compartido de Azure Files en un contenedor
    * Ofrece un rendimiento menor, pero también una opción de datos más económica, completa, flexible y confiable, mediante cualquier API, lo que admite el almacenamiento de archivos.
    * [Guía de procedimientos: implementar una aplicación con un volumen de Azure Files](service-fabric-mesh-howto-deploy-app-azurefiles-volume.md)
    
## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre el modelo de aplicación, consulte [Recursos de Service Fabric](service-fabric-mesh-service-fabric-resources.md).
