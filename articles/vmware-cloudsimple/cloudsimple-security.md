---
title: Seguridad de los servicios de CloudSimple
description: Describe los modelos de responsabilidad compartida para la seguridad de los servicios CloudSimple
author: sharaths-cs
ms.author: b-shsury
ms.date: 4/27/19
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: f5f3fe32e03a9a2bb0186854a83917f8918c6647
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358129"
---
# <a name="cloudsimple-security-overview"></a>Introducción a la seguridad CloudSimple

En este artículo se proporciona información general de cómo se implementa la seguridad en el centro de datos, infraestructura y servicio CloudSimple.  Obtendrá información sobre la protección de datos y seguridad, seguridad de red y cómo se administran las vulnerabilidades y revisiones.

## <a name="shared-responsibility"></a>Responsabilidad compartida

Solución de VMware de Azure por CloudSimple utiliza un modelo de responsabilidad compartida para la seguridad. Seguridad de confianza en la nube se logra a través de las responsabilidades compartidas de clientes y Microsoft como proveedor de servicios. Esta matriz de responsabilidad reporta una mayor seguridad y elimina los puntos únicos de error. 

## <a name="azure-infrastructure"></a>Infraestructura de Azure 

Consideraciones de seguridad de la infraestructura de Azure incluyen la ubicación de los centros de datos y equipos. 

### <a name="datacenter-security"></a>Seguridad de centro de datos 

Microsoft tiene una división toda destinados al diseño, creación y funcionamiento de las instalaciones físicas que admiten Azure. Este equipo se dedica a mantener una seguridad física de última generación. Para obtener más información sobre la seguridad física, consulte [instalaciones de Azure, localmente y la seguridad física](https://docs.microsoft.com/azure/security/azure-physical-security).

### <a name="equipment-location"></a>Ubicación del equipo

El equipo de hardware sin sistema operativo que se ejecuta a las nubes privadas se hospeda en ubicaciones de centro de datos de Azure.  Las jaulas donde están que los equipos, requiere autenticación en dos fases basada en biométricos para tener acceso.

## <a name="dedicated-hardware"></a>Hardware dedicado

Como parte del servicio CloudSimple, todos los clientes de CloudSimple obtención hosts sin sistema operativo dedicados con discos conectados locales que están aislados físicamente en otro hardware de inquilino. Un hipervisor ESXi con vSAN se ejecuta en cada nodo. Los nodos se administran a través de clientes dedicados VMware vCenter y NSX. No uso compartido de hardware entre los inquilinos proporciona una capa adicional de aislamiento y protección de seguridad.

## <a name="data-security"></a>Seguridad de los datos

Los clientes mantienen el control y la propiedad de sus datos. Centralización de datos de los datos del cliente es responsabilidad del cliente.

### <a name="data-protection-for-data-at-rest-and-data-in-motion-within-internal-networks"></a>Protección de datos para datos en reposo y en movimiento en redes internas

Para los datos en reposo en el entorno de nube privada, puede usar el cifrado de la vSAN. cifrado vSAN funciona con VMware certificada servidores de administración de claves externas (KMS) en su propia red virtual o local.  Controlar las claves de cifrado de datos. Para los datos en movimiento en la nube privada, vSphere admite el cifrado de datos a través del cable para todo el tráfico vmkernel (incluido el tráfico de vMotion).

### <a name="data-protection-for-data-that-is-required-to-move-through-public-networks"></a>Protección de datos para los datos que se requiere para desplazarse por las redes públicas

Para proteger los datos que se mueven a través de redes públicas, puede crear IPsec y VPN SSL de túneles para las nubes privadas. Se admiten métodos de cifrado comunes, incluidos AES 128 y 256 bytes. Datos en tránsito (como autenticación, acceso administrativo y los datos del cliente) se cifran con mecanismos de cifrado estándar (SSH, TLS 1.2 y proteger RDP). La comunicación que transporta información confidencial utiliza los mecanismos de cifrado estándar.

### <a name="secure-disposal"></a>Eliminación segura 

Si su servicio CloudSimple expira o se termina, usted es responsable de quitar o eliminar los datos. CloudSimple cooperarán con usted para eliminar o devolver todos los datos del cliente, como se indica en el contrato de cliente, excepto en la medida CloudSimple es requerido por la ley aplicable para conservar algunos o todos los datos personales. Si es necesario para conservar los datos personales, CloudSimple archivará los datos e implementar las medidas razonables para evitar que los datos del cliente más procesamiento.

### <a name="data-location"></a>Ubicación de los datos

Al configurar las nubes privadas, elija la región de Azure donde se implementarán. Datos de máquinas virtuales de VMware no se mueven desde ese centro de datos físico a menos que realice la migración de datos o de copia de seguridad de datos fuera del sitio. También puede hospedar cargas de trabajo y almacenar datos en varias regiones de Azure si es adecuado para sus necesidades.

Los datos del cliente que reside en los nodos hiperconvergidos de nube privada no atraviesan las ubicaciones sin la acción explícita del administrador del inquilino. Es su responsabilidad para implementar las cargas de trabajo de una manera altamente disponible.

### <a name="data-backups"></a>Copias de seguridad de datos
CloudSimple no copia de seguridad o archivar los datos del cliente. CloudSimple realizar copia de seguridad periódica de vCenter y los datos NSX para proporcionar alta disponibilidad de servidores de administración. Antes de la copia de seguridad, todos los datos se cifran en el origen de vCenter con VMware APIs. Se transporta los datos cifrados y se almacenan en blobs de Azure. Las claves de cifrado para copias de seguridad se almacenan en un almacén administrado de CloudSimple muy seguro que se ejecutan en la red virtual CloudSimple en Azure.

## <a name="network-security"></a>Seguridad de redes

La solución CloudSimple se basa en las capas de seguridad de red.

### <a name="azure-edge-security"></a>Seguridad de Azure edge

Los servicios de CloudSimple se basan en la seguridad de red base proporcionada por Azure. Azure aplica técnicas de defensa en profundidad para la detección y respuesta a tiempo a los ataques de red asociado con la entrada anómalo o patrones de tráfico de salida y los ataques distribuidos (DDoS) de denegación de servicio. Este control de seguridad se aplica a entornos de nube privada y el software de plano de control desarrollado por CloudSimple.

### <a name="segmentation"></a>Segmentación

El servicio de CloudSimple tiene redes lógicamente independientes de nivel 2 que restringen el acceso a sus propias redes privadas en el entorno de nube privada. Puede proteger aún más sus redes de nube privada mediante un firewall. El Portal de CloudSimple permite definir UEVA y NS tráfico controles reglas de red para todo el tráfico de red, incluidos dentro de un tráfico de nube privada, el tráfico en la nube privado entre, tráfico general a Internet, y el tráfico de red a un entorno local a través de VPN de IPsec o Conexión de ExpressRoute.

## <a name="vulnerability-and-patch-management"></a>Una vulnerabilidad y la administración de revisiones 

CloudSimple es responsable de la aplicación de revisiones de seguridad periódica de software administrado de VMware (ESXi, vCenter y NSX).

## <a name="identity-and-access-management"></a>Administración de identidad y acceso

Los clientes pueden autenticarse en su cuenta de Azure (en Azure AD) mediante la autenticación multifactor o inicio de sesión único como el preferido. Desde el portal de Azure, puede iniciar el CloudSimple Portal sin volver a escribir las credenciales.

CloudSimple es compatible con la configuración opcional de un origen de identidad para la nube privada de vCenter. Puede usar un [origen de identidad local](https://docs.azure.cloudsimple.com/set-vcenter-identity), un nuevo origen de identidad para la nube privada, o [Azure AD](https://docs.azure.cloudsimple.com/azure-ad).

De forma predeterminada, los clientes tienen los privilegios necesarios para las operaciones diarias de vCenter dentro de la nube privada. Este nivel de permiso no incluye el acceso administrativo a vCenter. Si se requiere temporalmente acceso administrativo, puede [escalar sus privilegios](https://docs.azure.cloudsimple.com/escalate-private-cloud-privileges) durante un período limitado, mientras se completan las tareas administrativas.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [crear un servicio CloudSimple en Azure](quickstart-create-cloudsimple-service.md)
* Obtenga información sobre cómo [crear una nube privada](https://docs.azure.cloudsimple.com/create-private-cloud/)
* Obtenga información sobre cómo [configurar un entorno de nube privada](quickstart-create-private-cloud.md)
