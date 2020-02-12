---
title: 'Azure VMware Solutions (AVS): seguridad de los servicios de AVS'
description: Se describen los modelos de responsabilidad compartida para la seguridad de los servicios de AVS
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: d2d45f827d4165175a4a5429f0b9393a55e2ff1e
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025000"
---
# <a name="avs-security-overview"></a>Información general sobre seguridad de AVS

En este artículo se proporciona información general sobre cómo se implementa la seguridad en el servicio Azure VMware Solution by AVS, la infraestructura y el centro de datos. Obtendrá información sobre protección y seguridad de datos, seguridad de red y cómo se administran las vulnerabilidades y las revisiones.

## <a name="shared-responsibility"></a>Responsabilidad compartida

En Azure VMware Solution by AVS se usa un modelo de responsabilidad compartida para la seguridad. La seguridad de confianza en la nube se logra a través de las responsabilidades compartidas de los clientes y Microsoft como proveedor de servicios. Esta matriz de responsabilidad proporciona mayor seguridad y elimina los casos de único punto de error.

## <a name="azure-infrastructure"></a>Infraestructura de Azure

Las consideraciones de seguridad de la infraestructura de Azure incluyen la ubicación de los centros de datos y los equipos.

### <a name="datacenter-security"></a>Seguridad del centro de datos

Microsoft cuenta con una división completa dedicada al diseño, creación y funcionamiento de las instalaciones físicas que dan soporte a Azure. Este equipo se dedica a mantener una seguridad física de última generación. Para más información sobre la seguridad física, consulte [Instalaciones de Azure, entornos locales y seguridad física](../security/azure-physical-security.md).

### <a name="equipment-location"></a>Ubicación del equipo

El equipo de hardware sin sistema operativo que ejecuta las nubes privadas de AVS se hospeda en ubicaciones de centro de datos de Azure. Las jaulas donde reside ese equipo requieren autenticación en dos fases basada en biometría para obtener acceso.

## <a name="dedicated-hardware"></a>Hardware dedicado

Como parte del servicio AVS, todos los clientes de AVS obtienen hosts dedicados sin sistema operativo con discos conectados de forma local aislados físicamente del hardware de los demás inquilinos. En cada nodo se ejecuta un hipervisor ESXi con vSAN. Los nodos se administran a través de instancias de VMware, vCenter y NSX dedicadas para el cliente. La ausencia de uso compartido del hardware entre los inquilinos proporciona una capa adicional de aislamiento y protección de seguridad.

## <a name="data-security"></a>Seguridad de los datos

Los clientes mantienen el control y la propiedad de sus datos. La administración de los datos del cliente es responsabilidad del cliente.

### <a name="data-protection-for-data-at-rest-and-data-in-motion-within-internal-networks"></a>Protección de datos para datos en reposo y en movimiento en redes internas

Para los datos en reposo en el entorno de nube privada de AVS, puede usar el cifrado de vSAN. El cifrado de vSAN funciona con servidores de administración de claves externas (KMS) con certificación de VMware en la red virtual o local propia. Las claves de cifrado de datos se controlan personalmente. Para los datos en movimiento dentro de la nube privada de AVS, vSphere admite el cifrado de datos a través de la conexión para todo el tráfico de VMkernel (incluido el de vMotion).

### <a name="data-protection-for-data-that-is-required-to-move-through-public-networks"></a>Protección de los datos que se deben desplazar por redes públicas

Para proteger los datos que se desplazan a través de redes públicas, puede crear túneles VPN IPsec y SSL para las nubes privadas de AVS. Se admiten los métodos de cifrado comunes, incluidos AES de 128 y 256 bytes. Los datos en tránsito (incluidos los de autenticación, acceso administrativo y de cliente) se cifran con mecanismos de cifrado estándar (SSH, TLS 1.2 y RDP segura). La comunicación que transporta información confidencial usa los mecanismos de cifrado estándar.

### <a name="secure-disposal"></a>Eliminación segura

Si el servicio de AVS expira o finaliza, la responsabilidad de quitar o eliminar los datos es suya. AVS cooperará con usted para eliminar o devolver todos los datos del cliente, como se indica en el contrato de cliente, excepto en la medida en que la legislación aplicable requiera a AVS conservar algunos o todos los datos personales. Si es necesario conservar datos personales, AVS los archivará e implementará medidas razonables para evitar el procesamiento adicional de los datos del cliente.

### <a name="data-location"></a>Ubicación de los datos

Al configurar las nubes privadas de AVS, se elige la región de Azure en la que se van a implementar. Los datos de VM de VMware no se mueven desde ese centro de datos físico, a menos que realice la migración de datos o una copia de seguridad de los datos fuera del sitio. También puede hospedar cargas de trabajo y almacenar datos en varias regiones de Azure si es adecuado para sus necesidades.

Los datos del cliente que residen en nodos hiperconvergidos de nubes privadas de AVS no atraviesan las ubicaciones sin la acción explícita del administrador del inquilino. Su responsabilidad es implementar las cargas de trabajo para que tengan alta disponibilidad.

### <a name="data-backups"></a>Copias de seguridad de datos

AVS no realiza copias de seguridad de los datos del cliente ni los archiva. AVS realiza una copia de seguridad periódica de los datos de vCenter y NSX para proporcionar alta disponibilidad de los servidores de administración. Antes de la copia de seguridad, todos los datos se cifran en el origen de vCenter mediante las API de VMware. Los datos cifrados se transportan y almacenan en un blob de Azure. Las claves de cifrado para las copias de seguridad se almacenan en un almacén administrado de AVS de alta seguridad que se ejecuta en la red virtual de AVS en Azure.

## <a name="network-security"></a>Seguridad de redes

La solución AVS se basa en capas de seguridad de red.

### <a name="azure-edge-security"></a>Seguridad perimetral de Azure

Los servicios de AVS se crean sobre la seguridad de red base proporcionada por Azure. Azure aplica técnicas de defensa en profundidad para la detección y respuesta a tiempo a los ataques de red asociados con patrones de tráfico de entrada o salida anómalos, y ataques de denegación de servicio distribuido (DDoS). Este control de seguridad se aplica a los entornos de nube privada de AVS y al software de plano de control desarrollado por AVS.

### <a name="segmentation"></a>Segmentación

El servicio de AVS tiene redes de Capa 2 separadas de forma lógica que restringen el acceso a las redes privadas propias en el entorno de nube privada de AVS. Puede proteger aún más las redes de nube privada de AVS mediante un firewall. El portal de AVS le permite definir reglas de control del tráfico de red norte-sur y este-oeste para todo el tráfico de red, que incluye el tráfico interno de la nube privada de AVS, entre nubes privadas de AVS, el tráfico general a Internet y el tráfico de red al entorno local a través de una conexión VPN de IPsec o Azure ExpressRoute.

## <a name="vulnerability-and-patch-management"></a>Administración de vulnerabilidades y revisiones

AVS es responsable de la aplicación de revisiones de seguridad periódicas al software administrado de VMware (ESXi, vCenter y NSX).

## <a name="identity-and-access-management"></a>Administración de identidades y acceso

Los clientes pueden autenticarse en su cuenta de Azure (en Azure AD) mediante la autenticación multifactor o el inicio de sesión único, como prefieran. Desde Azure Portal, puede iniciar el portal de AVS sin volver a escribir las credenciales.

AVS admite la configuración opcional de un origen de identidad para la instancia de vCenter de la nube privada de AVS. Puede usar un [origen de identidad local](set-vcenter-identity.md), un nuevo origen de identidad para la nube privada de AVS, o bien [Azure AD](azure-ad.md).

De manera predeterminada, a los clientes se les asignan los privilegios necesarios para las operaciones diarias de vCenter dentro de la nube privada de AVS. Este nivel de permiso no incluye el acceso administrativo a vCenter. Si de forma temporal se requiere acceso administrativo, puede [escalar los privilegios](escalate-private-cloud-privileges.md) durante un período limitado mientras completa las tareas administrativas.
