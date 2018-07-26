---
title: Terminología de Azure Service Fabric Mesh | Microsoft Docs
description: Obtenga información sobre los términos comunes de Azure Service Fabric Mesh.
services: service-fabric-mesh
keywords: ''
author: rwike77
ms.author: ryanwi
ms.date: 07/12/2018
ms.topic: conceptual
ms.service: service-fabric-mesh
manager: timlt
ms.openlocfilehash: 7c3ce5571c54d6c613114ea49999e450934c8ff4
ms.sourcegitcommit: dc646da9fbefcc06c0e11c6a358724b42abb1438
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/18/2018
ms.locfileid: "39136205"
---
# <a name="service-fabric-mesh-terminology"></a>Terminología de Service Fabric Mesh

Azure Service Fabric Mesh es un servicio totalmente administrado que permite a los desarrolladores implementar aplicaciones de microservicios sin necesidad de administrar máquinas virtuales, almacenamiento o redes. En este artículo se describe la terminología que se usa en Azure Service Fabric Mesh para comprender los términos que se utilizan en la documentación.

## <a name="service-fabric"></a>Service Fabric

Service Fabric es una plataforma de sistemas distribuidos de código abierto que facilita el empaquetamiento, la implementación y la administración de microservicios escalables y confiables. Service Fabric es el orquestador que sustenta Service Fabric Mesh. Service Fabric proporciona opciones para compilar y ejecutar las aplicaciones de microservicios. Puede usar cualquier marco de trabajo para escribir los servicios y elegir entre varias opciones de entorno desde las que ejecutar la aplicación.

## <a name="deployment-and-application-models"></a>Modelos de implementación y aplicación 

Para implementar los servicios, debe describir cómo se deben ejecutar. Service Fabric admite tres modelos de implementación diferentes:

### <a name="resource-model"></a>Modelo de recursos
Los recursos son todo lo que se puede implementar de manera individual en Service Fabric, como aplicaciones, servicios, redes y volúmenes. Los recursos se definen mediante un archivo YAML o JSON con el esquema de modelo de recursos de Azure.

El modelo de recursos es la manera más sencilla de describir las aplicaciones de Service Fabric. Su objetivo principal es en una implementación simple y la administración de servicios en contenedores.

Los recursos se pueden implementar en cualquier lugar en el que se ejecute Service Fabric.

### <a name="native-model"></a>Modelo nativo
El modelo de aplicación nativo proporciona a las aplicaciones acceso completo de bajo nivel a Service Fabric. Las aplicaciones y los servicios se definen como tipos registrados en archivos de manifiesto XML.

El modelo nativo es compatible con el marco de Reliable Services, que proporciona acceso a las API de tiempo de ejecución de Service Fabric y a las de administración de clústeres de C# y Java. El modelo nativo también admite archivos ejecutables y contenedores arbitrarios.

El modelo nativo no se admite en el entorno de Service Fabric Mesh.

### <a name="docker-compose"></a>Docker Compose 
[Docker Compose](https://docs.docker.com/compose/) forma parte del proyecto Docker. Service Fabric proporciona compatibilidad limitada para la implementación de aplicaciones con el modelo de Docker Compose.

## <a name="environments"></a>Entornos

Service Fabric es una tecnología de plataforma de código abierto en la que se basan varios productos y servicios diferentes. Microsoft proporciona las opciones siguientes:

 - **Service Fabric Mesh**: un servicio totalmente administrado para ejecutar aplicaciones de Service Fabric en Microsoft Azure.
 - **Azure Service Fabric**: la oferta de clúster de Service Fabric hospedado en Azure. Proporciona integración entre Service Fabric y la infraestructura de Azure, junto con la administración de actualización y configuración de clústeres de Service Fabric.
 - **Service Fabric independiente**: un conjunto de herramientas de instalación y configuración para [implementar clústeres de Service Fabric en cualquier lugar](/azure/service-fabric/service-fabric-deploy-anywhere) (de forma local o en cualquier proveedor de nube). No se administra mediante Azure.
 - **Clúster de desarrollo de Service Fabric**: proporciona una experiencia de desarrollo local en Windows, Linux o Mac para el desarrollo de aplicaciones de Service Fabric.

## <a name="environment-framework-and-deployment-model-support-matrix"></a>Matriz de compatibilidad de entorno, marco de trabajo y modelo de implementación
Cada entorno tiene un nivel de compatibilidad diferente con los marcos de trabajo y los modelos de implementación. En la tabla siguiente se describen las combinaciones admitidas de marco de trabajo y modelo de implementación.

| Tipo de aplicación | Descrito por | Azure Service Fabric Mesh | Clústeres de Azure Service (cualquier sistema operativo)| Clúster local: Windows | Clúster local: Linux | Clúster local: Mac | Clúster independiente (Windows)
|---|---|---|---|---|---|---|---|---|---|
| Aplicaciones de Service Fabric Mesh | Modelo de recursos (YAML & JSON) | Compatible |No compatible | Compatible |No compatible | No compatible | No compatible |
|Aplicaciones nativas de Service Fabric | Modelo de aplicación nativo (XML) | No compatible| Compatible|Compatible|Compatible|Compatible|Compatible|

En la tabla siguiente se describen los distintos modelos de aplicación y las herramientas que existen para ellos en Service Fabric.

| Tipo de aplicación | Descrito por | Visual Studio 2017 | Visual Studio 2015 | Eclipse | Código de VS | SFCTL | CLI de AZ | PowerShell
|---|---|---|---|---|---|---|---|---|---|
| Aplicaciones de Service Fabric Mesh | Modelo de recursos (YAML & JSON) | Compatible |No compatible |No compatible |No compatible |No compatible | Compatible: solo para el entorno de Mesh | No compatible
|Aplicaciones nativas de Service Fabric | Modelo de aplicación nativo (XML) | Compatible| Compatible|Compatible|Compatible|Compatible|Compatible|Compatible|

## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre Service Fabric mesh, lea la introducción:
- [Service Fabric Mesh overview](service-fabric-mesh-overview.md) (Introducción a Service Fabric Mesh)
