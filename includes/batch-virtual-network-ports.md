---
title: archivo de inclusión
description: archivo de inclusión
services: batch
documentationcenter: ''
author: laurenhughes
manager: gwallace
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: ''
ms.date: 07/16/2019
ms.author: lahugh
ms.custom: include file
ms.openlocfilehash: c8b25858556538835d6a84bf0d6699f9906f1438
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68322652"
---
### <a name="general-requirements"></a>Requisitos generales

* La red virtual debe estar en la misma región y suscripción que la cuenta de Batch que se utiliza para crear el grupo.

* El grupo que usa la red virtual puede tener 4096 nodos como máximo.

* La subred especificada para el grupo debe tener suficientes direcciones IP sin asignar para acoger el número de VM destinadas al grupo; esto es, la suma de las propiedades `targetDedicatedNodes` y `targetLowPriorityNodes` del grupo. Si la subred no tiene suficientes direcciones IP sin asignar, el grupo asigna parcialmente los nodos de proceso y se produce un error de cambio de tamaño. 

* Todos los servidores DNS personalizados que dan servicio a la red virtual deben resolver el punto de conexión de Azure Storage. En concreto, las direcciones URL del tipo `<account>.table.core.windows.net`, `<account>.queue.core.windows.net` y `<account>.blob.core.windows.net` deben poderse resolver. 

Los requisitos adicionales de red virtual difieren en función de si el grupo de Batch se encuentra en la configuración de máquina virtual o en la de servicios en la nube. Para las nuevas implementaciones de grupo en una red virtual, se recomienda la configuración de máquina virtual.

### <a name="pools-in-the-virtual-machine-configuration"></a>Grupos en la configuración de máquina virtual

**Redes virtuales admitidas**: solo redes virtuales basadas en Azure Resource Manager

**Id. de subred**: al especificar la subred mediante las API de Batch, utilice el *identificador de recurso* de la subred. El identificador de subred tiene el formato:

  ```
  /subscriptions/{subscription}/resourceGroups/{group}/providers/Microsoft.Network/virtualNetworks/{network}/subnets/{subnet}
  ```

**Permisos**: compruebe si sus directivas de seguridad o bloqueos del grupo de recursos o la suscripción de la red virtual restringen los permisos de un usuario para administrar la red virtual.

**Recursos de red adicionales**: Batch asigna automáticamente los recursos de red adicionales del grupo de recursos que contiene la red virtual. Por cada 50 nodos dedicados (o cada 20 nodos de prioridad baja), Batch asigna: 1 grupo de seguridad de red (NSG), 1 dirección IP pública y 1 equilibrador de carga. Estos recursos están limitados por las [cuotas de recursos](../articles/azure-subscription-service-limits.md) de la suscripción. Para los grupos grandes puede que deba solicitar un aumento de cuota para uno o más de estos recursos.

#### <a name="network-security-groups"></a>Grupos de seguridad de red

La subred debe permitir las comunicaciones entrantes desde el servicio Batch para poder programar tareas en los nodos de proceso y la comunicación saliente para comunicarse con Azure Storage u otros recursos. Respecto a los grupos en la configuración de máquina virtual, Batch agrega los grupos de seguridad de red a nivel de las interfaces de red (NIC) conectadas a las máquinas virtuales. Estos grupos de seguridad de red configuran automáticamente las reglas de entrada y salida para permitir el siguiente tráfico:

* Tráfico TCP de entrada en los puertos 29876 y 29877 desde las direcciones IP del rol del servicio Batch. 
* Tráfico TCP de entrada en el puerto 22 (nodos de Linux) o el puerto 3389 (nodos de Windows) para permitir el acceso remoto. Para determinados tipos de tareas de instancias múltiples en Linux (como MPI), tendrá que permitir también el tráfico del puerto 22 de SSH para las direcciones IP en la subred que contiene los nodos de ejecución de Batch.
* Tráfico saliente en cualquier puerto para la red virtual.
* Tráfico saliente en cualquier puerto para Internet.

> [!IMPORTANT]
> Tenga cuidado si modifica reglas de entrada o salida en los grupos de seguridad de red configurados para Batch o las agrega a ellos. Si un NSG deniega la comunicación con los nodos de proceso en la subred especificada, el servicio Batch establece el estado de dichos nodos en **No utilizable**.

No es necesario especificar los grupos de seguridad de red a nivel de subred, porque Batch configura los suyos propios. Sin embargo, si la subred especificada tiene asociados grupos de seguridad de red o un firewall, configure las reglas de seguridad de entrada y salida como se muestra en las tablas siguientes. Configure el tráfico entrante en el puerto 3389 (Windows) o 22 (Linux) solo si tiene que permitir el acceso remoto a las máquinas virtuales del grupo desde orígenes externos. No es necesario para que se puedan usar las máquinas virtuales del grupo. Tenga en cuenta que tendrá que habilitar el tráfico de subred de red virtual en el puerto 22 para Linux si usa determinados tipos de tareas de instancias múltiples, como MPI.

**Reglas de seguridad de entrada**

| Direcciones IP de origen | Etiqueta de servicio de origen | Puertos de origen | Destino | Puertos de destino | Protocolo | . |
| --- | --- | --- | --- | --- | --- | --- |
| N/D | `BatchNodeManagement` [Etiqueta de servicio](../articles/virtual-network/security-overview.md#service-tags) | * | Any | 29876-29877 | TCP | Allow |
| Las direcciones IP de origen de usuario para tener acceso remoto a los nodos de ejecución o la subred de nodo de ejecución para tareas de instancias múltiples de Linux, si es necesario. | N/D | * | Any | 3389 (Windows), 22 (Linux) | TCP | Allow |

**Reglas de seguridad de salida**

| Source | Puertos de origen | Destino | Etiqueta de servicio de destino | Puertos de destino | Protocolo | . |
| --- | --- | --- | --- | --- | --- | --- |
| Any | * | [Etiqueta de servicio](../articles/virtual-network/security-overview.md#service-tags) | `Storage` (en la misma región que la cuenta de Batch y la red virtual) | 443 | TCP | Allow |

### <a name="pools-in-the-cloud-services-configuration"></a>Grupos de la configuración de servicios en la nube

**Redes virtuales admitidas**: solo redes virtuales clásicas

**Id. de subred**: al especificar la subred mediante las API de Batch, utilice el *identificador de recurso* de la subred. El identificador de subred tiene el formato:

  ```
  /subscriptions/{subscription}/resourceGroups/{group}/providers/Microsoft.ClassicNetwork /virtualNetworks/{network}/subnets/{subnet}
  ```

**Permisos**: para utilizar una red virtual clásica, la entidad de servicio `Microsoft Azure Batch` debe tener el rol de Control de acceso basado en roles (RBAC) `Classic Virtual Machine Contributor` para la red virtual especificada.

#### <a name="network-security-groups"></a>Grupos de seguridad de red

La subred debe permitir las comunicaciones entrantes desde el servicio Batch para poder programar tareas en los nodos de proceso y la comunicación saliente para comunicarse con Azure Storage u otros recursos.

No es necesario especificar un grupo de seguridad de red, ya que Batch configura la comunicación de entrada únicamente de las direcciones IP de Batch a los nodos del grupo. Sin embargo, si la subred especificada tiene asociados grupos de seguridad de red o un firewall, configure las reglas de seguridad de entrada y salida como se muestra en las tablas siguientes. Si un NSG deniega la comunicación con los nodos de proceso en la subred especificada, el servicio Batch establece el estado de dichos nodos en **No utilizable**.

Configure el tráfico entrante en el puerto 3389 para Windows si tiene que permitir el RDP a los nodos del grupo. No es necesario para que se puedan usar los nodos del grupo.

**Reglas de seguridad de entrada**

| Direcciones IP de origen | Puertos de origen | Destino | Puertos de destino | Protocolo | . |
| --- | --- | --- | --- | --- | --- |
Any <br /><br />Aunque esto requiere "permitir todo" realmente, el servicio Batch aplica una regla de lista de control de acceso a nivel de cada nodo que filtra todas las direcciones IP de servicio que no sean de Batch. | * | Any | 10100, 20100, 30100 | TCP | Allow |
| Opcional, para permitir el acceso RDP en nodos de ejecución. | * | Any | 3389 | TCP | Allow |

**Reglas de seguridad de salida**

| Source | Puertos de origen | Destino | Puertos de destino | Protocolo | . |
| --- | --- | --- | --- | --- | --- |
| Any | * | Any | 443  | Any | Allow |
