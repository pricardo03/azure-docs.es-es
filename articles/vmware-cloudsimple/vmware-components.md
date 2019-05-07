---
title: Solución de VMware de Azure por CloudSimple - componentes de VMware de la nube privada
description: Describe cómo se instalan los componentes de VMware en la nube privada
author: sharaths-cs
ms.author: dikamath
ms.date: 04/30/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 5e6548a5a04e32b374a8a9c29d2ca5f89fd65c78
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65160191"
---
# <a name="private-cloud-vmware-components"></a>Componentes de VMware de la nube privada

Una nube privada es una pila VMware aislada (hosts ESXi, vCenter, vSAN y NSX) entorno administrado por un servidor vCenter en un dominio de administración.  El servicio CloudSimple permite implementar de forma nativa VMware en Azure infraestructura sin sistema operativo en las ubicaciones de Azure.  Las nubes privadas se integran con el resto de la nube de Azure.  Una nube privada se implementa con los siguientes componentes de la pila de VMware:

* **VMware ESXi -** nodos dedicados de hipervisor de Azure
* **VMware vCenter -** dispositivo para la administración centralizada del entorno de nube privada vSphere
* **VSAN de VMware -** solución de infraestructura hiperconvergida
* **Centro de datos de VMware NSX -** virtualización y el Software de seguridad de red  

## <a name="vmware-component-versions"></a>Versiones de componentes de VMware

Una pila de VMware de la nube privada se implementa con la siguiente versión de software.

| Componente | Version | Versión con licencia |
|-----------|---------|------------------|
| ESXi | 6.7U1 | Enterprise Plus |
| vCenter | 6.7U1 | vCenter estándar |
| vSAN | 6.7 | Enterprise |
| Centro de datos NSX | 2.3 | Avanzado |

## <a name="esxi"></a>ESXi

VMware ESXi está instalado en los nodos de CloudSimple adquiridos al crear una nube privada.  ESXi ofrece el hipervisor para implementar las máquinas virtuales de carga de trabajo (VM).  Los nodos proporcionan infraestructuras hiperconvergidas (proceso y almacenamiento) en la nube privada.  Los nodos forman parte del clúster de vSphere en la nube privada.  Cada nodo tiene cuatro interfaces de las redes físicas conectadas a subpuesto red.  Dos interfaces de red físico se utilizan para crear un **conmutador distribuido de vSphere (VDS)** en vCenter y dos que se usan para crear un **NSX administrado distribuido conmutador virtual (N-VDS)**.  Interfaces de red se configuran en modo activo / activo para alta disponibilidad.

Obtenga más información sobre VMware ESXi

## <a name="vcenter-server-appliance"></a>dispositivo de servidor de vCenter

dispositivo de servidor de vCenter (VCSA) proporciona las funciones de autenticación, administración y orquestación para la solución VMware por CloudSimple. VCSA con el controlador incrustado de servicios de plataforma (PSC) se implementa al crear la nube privada.  VCSA se implementa en el clúster de vSphere que se crea al implementar la nube privada.  Cada nube privada tiene su propio VCSA.  Expansión de una nube privada agrega los nodos a la VCSA en la nube privada.

### <a name="vcenter-single-sign-on"></a>inicio de sesión único en vCenter

Controlador de servicios de plataforma incrustada en VCSA está asociado con un **vCenter Single Sign-On dominio**.  Es el nombre de dominio **cloudsimple.local**.  Un usuario predeterminado **CloudOwner@cloudsimple.com** se crea para que tener acceso a vCenter.  Puede agregar su Active Directory local y Azure [orígenes de identidad para vCenter](https://docs.azure.cloudsimple.com/set-vcenter-identity/).

## <a name="vsan-storage"></a>almacenamiento vSAN

Las nubes privadas se crean con almacenamiento totalmente configurado vSAN de memoria flash, local al clúster.  Mínimos y tres nodos de la misma SKU son necesarios para crear un clúster de vSphere con el almacén de datos vSAN.  Desduplicación y compresión están habilitados de forma predeterminada en el almacén de datos vSAN.  Se crean dos grupos de discos en cada nodo del clúster de vSphere. Cada grupo de discos contiene un disco de caché y tres discos de capacidad.

Una directiva de almacenamiento predeterminada vSAN se crea en el clúster de vSphere y se aplica al almacén de datos vSAN.  Esta directiva determina cómo los objetos de almacenamiento de máquina virtual se aprovisionan y se asigna en el almacén de datos para garantizar el nivel de servicio requerido.  Define la directiva de almacenamiento la **errores tolerables (FTT)** y **Failure (método) tolerancia**.  Puede crear nuevas directivas de almacenamiento y aplicarlas a las máquinas virtuales. Para mantener el SLA, se debe mantener la capacidad de reserva del 25% en el almacén de datos vSAN.  

### <a name="default-vsan-storage-policy"></a>Directiva de almacenamiento predeterminada vSAN

Tabla siguiente muestra el valor predeterminado vSAN parámetros de la directiva de almacenamiento.

| Número de nodos de clúster de vSphere | FTT | Método de tolerancia a errores |
|------------------------------------|-----|--------------------------|
| 3 y 4 nodos | 1 | RAID 1 (reflejo) - crea 2 copias |
| 5 a 16 nodos | 2 | RAID 1 (reflejo) - crea 3 copias |

## <a name="nsx-data-center"></a>Centro de datos NSX

Centro de datos NSX proporciona virtualización de red, la segmentación de micro y capacidades de seguridad de red en la nube privada.  Puede configurar todos los servicios compatibles con NSX de centro de datos en la nube privada a través de NSX.  Al crear una nube privada, los siguientes componentes NSX están instalados y configurados.

* NSXT Manager
* Las zonas de transporte
* Host y el perfil de vínculo superior de Edge
* Conmutador lógico para el transporte perimetral, Ext1 y Ext2
* Grupo de IP para los nodos de transporte de ESXi
* Grupo de IP para el nodo de transporte perimetral
* Nodos perimetrales
* Regla de DRS affinity para el controlador y las máquinas virtuales de Edge
* Enrutador de nivel 0.
* Habilitar BGP en Tier0 enrutador

## <a name="vsphere-cluster"></a>clúster de vSphere

Hosts de ESXi están configurados como un clúster para garantizar una alta disponibilidad de la nube privada.  Al crear una nube privada, se implementan los componentes de administración de vSphere en el primer clúster.  Se crea un grupo de recursos para los componentes de administración y todas las máquinas virtuales de administración se implementan en este grupo de recursos. No se puede eliminar el primer clúster para reducir la nube privada.  vSphere clúster proporciona alta disponibilidad para máquinas virtuales mediante **vSphere alta disponibilidad**.  Errores tolerables se basan en el número de nodos disponibles en el clúster.  Puede usar la fórmula ```Number of nodes = 2N+1``` donde ```N``` es el número de errores tolerables.

### <a name="vsphere-cluster-limits"></a>límites de clúster de vSphere

| Recurso | Límite |
|----------|-------|
| Número mínimo de nodos que se va a crear una nube privada (primer clúster de vSphere) | 3 |
| Número máximo de nodos en un vSphere del clúster en una nube privada | 16 |
| Número máximo de nodos en una nube privada | 64 |
| Número máximo de vSphere de clústeres en una nube privada | 21 |
| Número mínimo de nodos en un nuevo clúster de vSphere | 3 |

## <a name="vmware-infrastructure-maintenance"></a>Mantenimiento de la infraestructura de VMware

En ocasiones, es necesario realizar cambios en la configuración de la infraestructura de VMware. Actualmente, estos intervalos pueden producirse cada 1 ó 2 meses, pero se espera que la frecuencia de rechazar con el tiempo. Este tipo de mantenimiento normalmente puede realizarse sin interrumpir el consumo de los servicios CloudSimple normal. Durante un intervalo de mantenimiento de VMware, seguirán funcionando sin afectar los siguientes servicios:

* Las aplicaciones y el plano de administración de VMware
* acceso a vCenter
* Todas las redes y almacenamiento
* Todo el tráfico de Azure

## <a name="updates-and-upgrades"></a>Las actualizaciones

CloudSimple es responsable de la administración del ciclo de vida del software de VMware (ESXi, vCenter, PSC y NSX) en la nube privada.

Actualizaciones de software incluyen:

* **Las revisiones**. Las revisiones de seguridad o correcciones publicadas por VMware.
* **Actualizaciones**. Cambio de versión secundaria de un componente de la pila de VMware.
* **Las actualizaciones**. Cambio de versión principal de un componente de la pila de VMware.

CloudSimple comprueba una revisión de seguridad críticas en cuanto esté disponible de VMware. Por el SLA, CloudSimple implementa la revisión de seguridad en entornos de nube privada dentro de una semana.

CloudSimple proporciona actualizaciones de mantenimiento trimestrales a componentes de software de VMware. Cuando está disponible una nueva versión principal del software de VMware, CloudSimple trabaja con clientes para coordinar una ventana de mantenimiento adecuado para la actualización.  

## <a name="next-steps"></a>Pasos siguientes

* [Actualizaciones y mantenimiento CloudSimple](cloudsimple-maintenance-updates.md)