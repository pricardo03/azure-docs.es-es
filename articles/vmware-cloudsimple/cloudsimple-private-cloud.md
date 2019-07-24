---
title: 'Nubes privadas en la solución de VMware de CloudSimple: Azure'
description: Obtenga información sobre los conceptos y las nubes privadas de CloudSimple.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: e3ab53466cae5bd39d85f8d846c8e59ad94f1f2d
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165745"
---
# <a name="cloudsimple-private-cloud-overview"></a>Introducción a la nube privada de CloudSimple

CloudSimple transforma y amplía las cargas de trabajo de VMware en nubes públicas en cuestión de minutos. Con el servicio CloudSimple, puede implementar de forma nativa VMware en una infraestructura de Azure sin sistema operativo. La implementación reside en las ubicaciones de Azure y se integra por completo con el resto de la nube de Azure.

* La solución CloudSimple proporciona una continuidad operativa de VMware completa. Esta solución le ofrece las siguientes ventajas de la nube pública:
  * Elasticidad
  * Innovación
  * Eficacia
* Con CloudSimple, podrá beneficiarse de un modelo de consumo en la nube que reduce el costo total de propiedad. También ofrece aprovisionamiento a petición, pago según el crecimiento y optimización de la capacidad.
* CloudSimple es totalmente compatible con:
  * Herramientas existentes
  * Aptitudes
  * Procesos
* Esta compatibilidad permite a los equipos administrar las cargas de trabajo en la nube de Azure, sin interrumpir las directivas:
  * Red
  * Seguridad  
  * Protección de datos  
  * Auditoría
* CloudSimple administra la infraestructura y todos los servicios de redes y administración necesarios. El servicio CloudSimple permite a su equipo centrarse en lo siguiente:
  * Valor empresarial
  * Aprovisionamiento de aplicaciones
  * Continuidad del negocio
  * Soporte técnico
  * Aplicación de directivas

## <a name="private-cloud-environment-overview"></a>Introducción al entorno de nube privada

Una nube privada es una pila de VMware aislada, como estos entornos:

* Hosts de ESXi
* vCenter
* vSAN
* NSX

Las nubes privadas se administran mediante un servidor de vCenter en su propio dominio de administración.

La pila se ejecuta en:

* Nodos dedicados
* Nodos de hardware sin sistema operativo aislados

Los usuarios consumen la pila mediante herramientas nativas de VMware, incluidas las siguientes:

* vCenter
* NSX Manager

Puede implementar nodos dedicados en ubicaciones de Azure. Después, puede administrarlos con Azure y CloudSimple. Una nube privada consta de uno o varios clústeres de vSphere. Cada uno de ellos contiene de 3 a 16 nodos.

Puede crear una nube privada con nodos aprovisionados:

* Nodos de pago por uso
* Nodos dedicados y reservados

Puede conectar la nube privada a su entorno local y la red de Azure mediante las siguientes conexiones:

* Protección
* VPN privada
* Azure ExpressRoute

El entorno de nube privada está diseñado para no tener un único punto de error:

* Los clústeres de ESXi están configurados con alta disponibilidad de vSphere y tienen un tamaño adecuado para tener al menos un nodo libre para obtener resistencia.
* vSAN proporciona almacenamiento principal redundante. vSAN requiere al menos tres nodos para proteger frente a un único error. Puede configurar vSAN para proporcionar mayor resistencia para clústeres más grandes.
* Puede configurar las máquinas virtuales de vCenter, PSC y NSX Manager con la directiva de almacenamiento de RAID-10 para protegerse frente a errores de almacenamiento. Después, la alta disponibilidad de vSphere las protege frente a errores de nodo o red.

## <a name="scenarios-for-deploying-a-private-cloud"></a>Escenarios de implementación de una nube privada

* **Migración o retirada del centro de datos**

  * Obtenga una capacidad adicional cuando alcance los límites de su centro de datos existente o actualice el hardware.
  * Agregue la capacidad necesaria en la nube para no tener problemas al administrar las actualizaciones de hardware.
  * Reduzca el riesgo y el costo de las migraciones de nube, en comparación con los cambios de arquitectura o las conversiones que requieren mucho tiempo.
  * Use habilidades o herramientas conocidas de VMware para acelerar las migraciones de nube. En la nube, use servicios de Azure para modernizar las aplicaciones a su ritmo.

* **Expansión a petición**

  * Expanda a la nube para satisfacer necesidades no anticipadas, como nuevos entornos de desarrollo o picos estacionales de capacidad.
  * Cree una capacidad a petición y manténgala solo cuando la necesite.
  * Reduzca la inversión inicial, acelere la velocidad de aprovisionamiento y reduzca la complejidad con la misma arquitectura y directivas en un entorno local y en la nube.

* **Recuperación ante desastres y escritorios virtuales en la nube de Azure**

  * Establezca el acceso remoto a los datos, las aplicaciones y los escritorios en la nube de Azure. Con conexiones de gran ancho de banda, cargará o descargará los datos rápidamente para recuperarse de incidentes. Las redes de baja latencia le proporcionan los tiempos de respuesta rápidos que los usuarios esperan de una aplicación de escritorio.

  * Replique todas las directivas y las redes en la nube mediante el portal de CloudSimple y las herramientas conocidas de VMware. Esta replicación reduce el esfuerzo y el riesgo de crear y administrar implementaciones de DR y VDI.

* **Aplicaciones y bases de datos de alto rendimiento**

  * Ejecute las cargas de trabajo más exigentes con la arquitectura hiperconvergida que proporciona CloudSimple.
  * Ejecute Oracle, Microsoft SQL server, sistemas de middleware y bases de datos de alto rendimiento que no son SQL.

  * Pruebe la nube como su propio centro de datos con conexiones de red de alta velocidad de 25 Gbps. Las conexiones de alta velocidad le permiten ejecutar aplicaciones híbridas que abarcan cargas de trabajo locales, de VMware en Azure y privadas de Azure, sin comprometer el rendimiento.

* **Realmente híbrida**

  * Unifique DevOps en servicios de Azure y VMware.
  * Optimice la administración de VMware para servicios y soluciones de Azure que se pueden aplicar en todas las cargas de trabajo.
  * Acceda a servicios en la nube pública sin tener que ampliar su centro de datos ni cambiar la arquitectura de sus aplicaciones.
  * Centralice las identidades, las directivas de control de acceso, el registro y la supervisión de aplicaciones de VMware en Azure.

## <a name="limits"></a>límites

En la siguiente tabla se muestran los límites de nodos en los recursos de una nube privada.

| Recurso | Límite |
|----------|-------|
| Número mínimo de nodos para crear una nube privada | 3 |
| Número máximo de nodos en un clúster en una nube privada | 16 |
| Número máximo de nodos en una nube privada | 64 |
| Número mínimo de nodos en un clúster nuevo | 3 |

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre cómo [crear una nube privada](https://docs.azure.cloudsimple.com/create-private-cloud/)
* Más información sobre cómo [configurar un entorno de nube privada](quickstart-create-private-cloud.md)