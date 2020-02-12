---
title: 'Azure VMware Solutions (AVS): nubes privadas de AVS'
description: Obtenga información sobre los conceptos y las nubes privadas de AVS.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 2688edf281a6d8bc3d61e8e294c920f115f0f3f6
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024964"
---
# <a name="avs-private-cloud-overview"></a>Introducción a la nube privada de AVS

AVS transforma y amplía las cargas de trabajo de VMware en nubes públicas en cuestión de minutos. Con el servicio de AVS, puede implementar de forma nativa VMware en una infraestructura de Azure sin sistema operativo. La implementación reside en las ubicaciones de Azure y se integra por completo con el resto de la nube de Azure.

La solución AVS proporciona una continuidad operativa de VMware completa. Esta solución le ofrece las siguientes ventajas de la nube pública:

* Elasticidad
* Innovación
* Eficacia

Con AVS, puede beneficiarse de un modelo de consumo en la nube que reduce el costo total de propiedad. También ofrece aprovisionamiento a petición, pago según el crecimiento y optimización de la capacidad.

AVS es totalmente compatible con:

* Herramientas existentes
* Aptitudes
* Procesos

Esta compatibilidad permite a los equipos administrar las cargas de trabajo en la nube de Azure, sin interrumpir estos tipos de directivas:

* Red
* Seguridad  
* Protección de los datos  
* Auditoría

AVS administra la infraestructura y todos los servicios de redes y administración necesarios. El servicio de AVS permite a su equipo centrarse en lo siguiente:

* Valor empresarial
* Aprovisionamiento de aplicaciones
* Continuidad empresarial
* Soporte técnico
* Aplicación de directivas

## <a name="avs-private-cloud-environment-overview"></a>Introducción al entorno de nube privada de AVS

Una nube privada de AVS es una pila de VMware aislada compatible con los siguientes entornos:

* Hosts de ESXi
* vCenter
* vSAN
* NSX

Las nubes privadas de AVS se administran desde el portal de AVS. Tienen su propio servidor vCenter en su propio dominio de administración.

La pila se ejecuta en:

* Nodos dedicados
* Nodos de hardware sin sistema operativo aislados

Los usuarios consumen la pila mediante herramientas nativas de VMware, incluidas las siguientes:

* vCenter
* NSX Manager

Puede implementar nodos dedicados en ubicaciones de Azure. Después, puede administrarlos con Azure y AVS. Una nube privada de AVS consta de uno o varios clústeres de vSphere, cada uno de los cuales contiene de 3 a 16 nodos.

Puede crear una nube privada de AVS mediante nodos adquiridos, de pago por uso o nodos reservados y dedicados.

Puede conectar la nube privada de AVS a su entorno local y la red de Azure mediante las siguientes conexiones:

* Seguridad
* VPN privada
* Azure ExpressRoute

El entorno de nube privada de AVS está diseñado para no tener ningún único punto de error:

* Los clústeres de ESXi están configurados con alta disponibilidad de vSphere y tienen un tamaño adecuado para tener al menos un nodo libre para obtener resistencia.
* vSAN proporciona almacenamiento principal redundante. vSAN requiere al menos tres nodos para proteger frente a un único error. Puede configurar vSAN para proporcionar mayor resistencia para clústeres más grandes.
* Puede configurar las máquinas virtuales de vCenter, PSC y NSX Manager con la directiva de almacenamiento de RAID-10 para protegerse frente a errores de almacenamiento. La alta disponibilidad de vSphere protege frente a errores de nodo o red.

## <a name="scenarios-for-deploying-an-avs-private-cloud"></a>Escenarios de implementación de una nube privada de AVS

Estos son algunos ejemplos de casos de uso para la implementación de la nube privada de AVS.

### <a name="data-center-retirement-or-migration"></a>Migración o retirada del centro de datos

* Obtenga una capacidad adicional cuando alcance los límites de su centro de datos existente o actualice el hardware.
* Agregue la capacidad necesaria en la nube para no tener problemas al administrar las actualizaciones de hardware.
* Reduzca el riesgo y el costo de las migraciones de nube, en comparación con los cambios de arquitectura o las conversiones que requieren mucho tiempo.
* Use aptitudes o herramientas conocidas de VMware para acelerar las migraciones de nube. En la nube, use servicios de Azure para modernizar las aplicaciones a su ritmo.

### <a name="expand-on-demand"></a>Expansión a petición

* expanda a la nube para satisfacer necesidades no anticipadas, como nuevos entornos de desarrollo o picos estacionales de capacidad.
* Cree una capacidad a petición y manténgala solo cuando la necesite.
* Reduzca la inversión inicial, acelere la velocidad de aprovisionamiento y reduzca la complejidad con la misma arquitectura y directivas en entornos locales y en la nube.

### <a name="disaster-recovery-and-virtual-desktops-in-the-azure-cloud"></a>Recuperación ante desastres y escritorios virtuales en la nube de Azure

* Establezca el acceso remoto a los datos, las aplicaciones y los escritorios en la nube de Azure. Con conexiones de gran ancho de banda, cargará o descargará los datos rápidamente para recuperarse de incidentes. Las redes de baja latencia le proporcionan los tiempos de respuesta rápidos que los usuarios esperan de una aplicación de escritorio.

* Replique todas las directivas y las redes en la nube mediante el portal de AVS y las herramientas conocidas de VMware. La replicación reduce el esfuerzo y el riesgo de crear y administrar implementaciones de DR y VDI.

### <a name="high-performance-applications-and-databases"></a>Aplicaciones y bases de datos de alto rendimiento

* Ejecute las cargas de trabajo más exigentes con la arquitectura hiperconvergida que proporciona AVS.
* Ejecute Oracle, Microsoft SQL server, sistemas de middleware y bases de datos de alto rendimiento que no son SQL.
* Pruebe la nube como su propio centro de datos con conexiones de red de alta velocidad de 25 Gbps. Las conexiones de alta velocidad le permiten ejecutar aplicaciones híbridas que abarcan cargas de trabajo locales, de VMware en Azure y privadas de Azure, sin comprometer el rendimiento.

### <a name="true-hybrid"></a>Realmente híbrida

* Unifique DevOps en servicios de Azure y VMware.
* Optimice la administración de VMware para servicios y soluciones de Azure que se pueden aplicar en todas las cargas de trabajo.
* Acceda a servicios en la nube pública sin tener que ampliar su centro de datos ni cambiar la arquitectura de sus aplicaciones.
* Centralice las identidades, las directivas de control de acceso, el registro y la supervisión de aplicaciones de VMware en Azure.

## <a name="limits"></a>límites

En la siguiente tabla se muestran los límites de nodos en los recursos de una nube privada de AVS.

| Resource | Límite |
|----------|-------|
| Número mínimo de nodos para crear una nube privada de AVS | 3 |
| Número máximo de nodos en un clúster en una nube privada de AVS | 16 |
| Número máximo de nodos en una nube privada de AVS | 64 |
| Número mínimo de nodos en un clúster nuevo | 3 |

## <a name="next-steps"></a>Pasos siguientes

* Aprenda a [crear una nube privada de AVS](create-private-cloud.md)
* Aprenda a [configurar un entorno de nube privada de AVS](quickstart-create-private-cloud.md)
