---
title: Nubes privadas en la solución VMware CloudSimple - Azure
description: Obtenga información sobre los conceptos y las nubes privadas de CloudSimple.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: dc07b4eea553e6cb3d9b522826e860ddbfbc1513
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2019
ms.locfileid: "64577051"
---
# <a name="cloudsimple-private-cloud-overview"></a>Información general de la nube privada CloudSimple

CloudSimple transforma y extiende las cargas de trabajo de VMware a nubes públicas en cuestión de minutos. Con el servicio CloudSimple, puede implementar VMware de forma nativa en Azure infraestructura sin sistema operativo. La implementación reside en las ubicaciones de Azure y se integra completamente con el resto de la nube de Azure.

* La solución CloudSimple proporciona continuidad operativa de VMware completa. Esta solución le ofrece las ventajas de la nube pública de:
  * Elasticidad
  * Innovación
  * Eficiencia
* Con CloudSimple, podrá beneficiarse de un modelo de consumo en la nube que reduce el costo total de propiedad. También ofrece aprovisionamiento, pague a medida crece bajo demanda y optimización de capacidad.
* CloudSimple es totalmente compatible con:
  * Herramientas existentes
  * Aptitudes
  * Procesos
* Esta compatibilidad permite a los equipos administrar las cargas de trabajo en la nube de Azure, sin interrumpir las directivas:
  * Red
  * Seguridad  
  * Protección de datos  
  * Auditoría
* CloudSimple administra la infraestructura y todos los servicios necesarios de redes y administración. El servicio CloudSimple permite centrarse en su equipo:
  * Valor empresarial
  * Aprovisionamiento de aplicaciones
  * Continuidad del negocio
  * Soporte técnico
  * Aplicación de directivas

## <a name="private-cloud-environment-overview"></a>Introducción al entorno de nube privada

Una nube privada es una pila de VMware aislada, como estos entornos:

* Hosts ESXi
* vCenter
* vSAN
* NSX

Las nubes privadas se administran mediante un servidor de vCenter en su propio dominio de administración.

La pila se ejecuta en:

* Nodos dedicados
* Nodos aislados de hardware sin sistema operativo

Los usuarios consumen la pila a través de herramientas nativas de VMware, incluidos:

* vCenter
* NSX Manager

Puede implementar los nodos dedicados en ubicaciones de Azure. A continuación, puede administrar con Azure y CloudSimple. Una nube privada se compone de uno o varios clústeres de vSphere, y cada clúster contiene 3 y 16 nodos.

Puede crear una nube privada utilizando adquirido nodos:

* Nodos de pago por uso
* Reservado y dedicados de nodos

La nube privada puede conectarse a su entorno local y la red de Azure mediante las siguientes conexiones:

* Protección
* VPN privada
* Azure ExpressRoute

El entorno de nube privada está diseñado para eliminar la necesidad de un único punto de error:

* ESXi clústeres están configurados con vSphere alta disponibilidad y el tamaño se ajusta para que tenga al menos un nodo de repuesto para proporcionar resistencia.
* vSAN proporciona redundancia de almacenamiento principal. vSan requiere al menos tres nodos para proporcionar protección frente a un único error. Puede configurar vSAN para proporcionar mayor resistencia para clústeres más grandes.
* Puede configurar vCenter PSC, NSX Manager máquinas virtuales y con la directiva de almacenamiento de RAID 10 para protegerse frente a errores de almacenamiento. A continuación, que están protegidos por vSphere alta disponibilidad frente a errores de nodo y la red.

## <a name="scenarios-for-deploying-a-private-cloud"></a>Escenarios de implementación de una nube privada

* **Retirada de centro de datos o la migración**

  * Obtener capacidad adicional cuando se alcanzan los límites de su centro de datos existente o actualiza el hardware.
  * Agregar la capacidad necesaria en la nube y eliminar los problemas de administración de actualizaciones de hardware.
  * Reducir el riesgo y el costo de las migraciones de nube, en comparación con las conversiones que requieren mucho tiempo o mejorarlo.
  * Use herramientas conocidas de VMware y las habilidades para acelerar las migraciones de nube. En la nube, use servicios de Azure para modernizar las aplicaciones a su ritmo.

* **Expanda bajo demanda**

  * Expandir a la nube para satisfacer las necesidades no anticipadas, como nuevos entornos de desarrollo o picos estacionales de capacidad.
  * Crear nueva capacidad bajo demanda y manténgalo solo siempre que lo necesite.
  * Reducir la inversión inicial, aceleran la velocidad de aprovisionamiento y reduzca la complejidad con la misma arquitectura y las directivas en un entorno local y la nube.

* **Recuperación ante desastres y escritorios virtuales en la nube de Azure**

  * Establecer el acceso remoto a datos, aplicaciones y escritorios en la nube de Azure. Con conexiones de gran ancho de banda, cargar o descargar datos rápida para recuperarse de incidentes. Dar de baja latencia redes rápida respuesta el tiempo que los usuarios esperan de una aplicación de escritorio.

  * Replicar todas las directivas y las redes en la nube mediante el portal de CloudSimple y herramientas conocidas de VMware. Esta replicación reduce el esfuerzo y riesgo de crear y administrar implementaciones de VDI y de recuperación ante desastres.

* **Las bases de datos y aplicaciones de alto rendimiento**

  * Ejecute las cargas de trabajo más exigentes, con la arquitectura hiperconvergido proporcionada por CloudSimple.
  * Ejecute Oracle, Microsoft SQL server, los sistemas de software intermedio y las bases de datos NOSQL de alto rendimiento.

  * Experiencia en la nube como su propio centro de datos con conexiones de red de alta velocidad 25 Gbps. Conexiones de alta velocidad le permiten ejecutar aplicaciones híbridas que abarcan de forma local VMware en Azure, y cargas de trabajo privadas Azure, sin comprometer el rendimiento.

* **Híbrida verdadera**

  * Unificar DevOps en servicios de Azure y VMware.
  * Optimizar la administración de VMware para servicios de Azure y soluciones que se pueden aplicar en todas las cargas de trabajo.
  * Obtener acceso a servicios de nube pública sin tener que ampliar su centro de datos o rediseñar sus aplicaciones.
  * Centralizar las identidades, las directivas de control de acceso de registro y supervisión de aplicaciones de VMware en Azure.

## <a name="limits"></a>límites

Tabla siguiente muestra los límites de nodo en los recursos de una nube privada.

| Recurso | Límite |
|----------|-------|
| Número mínimo de nodos que se va a crear una nube privada | 3 |
| Número máximo de nodos en un clúster en una nube privada | 16 |
| Número máximo de nodos en una nube privada | 64 |
| Número mínimo de nodos en un nuevo clúster | 3 |

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [crear una nube privada](https://docs.azure.cloudsimple.com/create-private-cloud/)
* Obtenga información sobre cómo [configurar un entorno de nube privada](quickstart-create-private-cloud.md)