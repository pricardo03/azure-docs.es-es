---
title: 'Azure VMware Solutions (AVS): migración de VM de carga de trabajo a la nube privada de AVS'
description: Describe cómo migrar máquinas virtuales de vCenter local a vCenter de la nube privada de AVS.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: af02bd947c73b670306704a10a09ca981b34c9a9
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77020002"
---
# <a name="migrate-workload-vms-from-on-premises-vcenter-to-avs-private-cloud-vcenter-environment"></a>Migración de VM de carga de trabajo desde la versión de vCenter local a un entorno de vCenter de la nube privada de AVS

Para migrar VM de un centro de centros local a la nube privada de AVS, existen varias opciones disponibles. La nube privada de AVS proporciona acceso nativo a VMware vCenter y las herramientas compatibles con VMware se pueden usar para la migración de cargas de trabajo. En este artículo se describen algunas de las opciones de migración de vCenter.

## <a name="prerequisites"></a>Prerequisites

La migración de VM y datos desde un centro de datos local requiere conectividad de red desde el centro de datos al entorno de nube privada de AVS. Use cualquiera de estos métodos para establecer la conectividad de red:

* [Conexión VPN de sitio a sitio](vpn-gateway.md#set-up-a-site-to-site-vpn-gateway) entre el entorno local y la nube privada de AVS.
* Conexión ExpressRoute Global Reach entre el circuito de ExpressRoute local y un circuito ExpressRoute de AVS.

La ruta de acceso de red del entorno de vCenter local a la nube privada de AVS debe estar disponible para la migración de las VM mediante vMotion. La red vMotion de la versión local de vCenter debe tener capacidades de enrutamiento. Compruebe que el firewall permite todo el tráfico de vMotion entre vCenter local y vCenter de la nube privada de AVS. (En la nube privada de AVS, el enrutamiento en la red vMotion está configurado de forma predeterminada).

## <a name="migrate-isos-and-templates"></a>Migración de archivos ISO y plantillas

Para crear máquinas virtuales en la nube privada de AVS, use archivos ISO y plantillas de VM. Para cargar y publicar los archivos ISO y las plantillas en vCenter de la nube privada de AVS, use el siguiente método.

1. Cargue el archivo ISO en vCenter de la nube privada de AVS desde la interfaz de usuario de vCenter.
2. [Publique una biblioteca de contenido](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A0F1C13-7336-45CE-B211-610D39A6E1F4.html) en vCenter de la nube privada de AVS:

    1. Publique su biblioteca de contenido local.
    2. Cree una biblioteca de contenido en vCenter de la nube privada de AVS.
    3. Suscríbase a la biblioteca de contenido local publicada.
    4. Sincronice la biblioteca de contenido para acceder al contenido suscrito.

## <a name="migrate-vms-using-powercli"></a>Migración de máquinas virtuales mediante PowerCLI

Para migrar las VM de la instancia de vCenter local a la instancia de vCenter de la nube privada de AVS, use VMware PowerCLI o la utilidad para la migración de cargas de trabajo entre vCenter disponible en VMware Labs. En el siguiente script de ejemplo se muestran los comandos de migración de PowerCLI.

```
$sourceVC = Connect-VIServer -Server <source-vCenter name> -User <source-vCenter user name> -Password <source-vCenter user password>
$targetVC = Connect-VIServer -Server <target-vCenter name> -User <target-vCenter user name> -Password <target-vCenter user password>
$vmhost = <name of ESXi host on destination>
$vm = Get-VM -Server $sourceVC <name of VM>
Move-VM -VM $vm -VMotionPriority High -Destination (Get-VMhost -Server $targetVC -Name $vmhost) -Datastore (Get-Datastore -Server $targetVC -Name <name of tgt vc datastore>)
```

> [!NOTE]
> Para usar los nombres de los hosts de ESXi y del servidor vCenter de destino, configure el reenvío de DNS desde la ubicación local a la nube privada de AVS.

## <a name="migrate-vms-using-nsx-layer-2-vpn"></a>Migración de máquinas virtuales una VPN de nivel 2 de NSX

Esta opción permite la migración en vivo de cargas de trabajo desde el entorno de VMware local a la nube privada de AVS en Azure. Con esta red de Capa 2 extendida, la subred del entorno local estará disponible en la nube privada de AVS. Después de la migración, no es preciso asignar nuevas direcciones IP a las máquinas virtuales.

En [Migración de cargas de trabajo mediante redes extendidas de Capa 2](migration-layer-2-vpn.md) se describe cómo usar una VPN de Capa 2 para extender una red de Capa 2 de un entorno local a la nube privada de AVS.

## <a name="migrate-vms-using-backup-and-disaster-recovery-tools"></a>Migración de máquinas virtuales mediante herramientas de copia de seguridad y de recuperación ante desastres

La migración de VM a la nube privada de AVS se puede realizar mediante herramientas tanto de copia de seguridad y restauración como de recuperación ante desastres. Use la nube privada de AVS como destino para la restauración de copias de seguridad creadas mediante herramientas de terceros. La nube privada de AVS también se puede usar como destino para la recuperación ante desastres mediante VMware SRM o una herramienta de terceros.

Para más información sobre el uso de estas herramientas, consulte los siguientes temas:

* [Copia de seguridad de máquinas virtuales de carga de trabajo en la nube privada de AVS mediante Veeam B&R](backup-workloads-veeam.md)
* [Configuración de la nube privada de AVS como un sitio de recuperación ante desastres de las cargas de trabajo locales de VMware](disaster-recovery-zerto.md)
