---
title: VMware Solution by CloudSimple - Dirección IP pública de Azure
description: Obtenga información sobre las direcciones IP públicas y sus ventajas para VMware Solution by CloudSimple
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 29d2bdb75a7f4409e6b3ffee2608cfe7adde6b1f
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812527"
---
# <a name="cloudsimple-public-ip-address-overview"></a>Introducción a la dirección IP pública de CloudSimple

Una dirección IP pública permite la comunicación entrante de los recursos de Internet en recursos de nube privada en una dirección IP privada. La dirección IP privada es una máquina virtual o un equilibrador de carga de software. La dirección IP privada se encuentra en el vCenter de la nube privada. La dirección IP pública le permite exponer a Internet los servicios que se ejecutan en la nube privada.

La dirección IP pública estará dedicada a la dirección IP privada hasta que anule su asignación. Una dirección IP pública solo puede asignarse a una dirección IP privada.

Un recurso asociado con una dirección IP pública siempre usa la dirección IP pública para acceder a Internet. De forma predeterminada, en una dirección IP pública solo se permite el acceso saliente a Internet.  Se deniega el tráfico entrante a la dirección IP pública.  Para permitir el tráfico entrante, cree una regla de firewall para la dirección IP pública en el puerto específico.

## <a name="benefits"></a>Ventajas

El uso de una dirección IP pública para la comunicación entrante proporciona las siguientes ventajas:

* Prevención de ataques de denegación de servicio distribuido (DDoS). Esta protección se habilita automáticamente para la dirección IP pública.
* Supervisión continua del tráfico y mitigación en tiempo real de los ataques al nivel de red habituales. Esta protección es la misma que usan los servicios en línea de Microsoft.
* La escala completa de la red global de Azure. La red puede utilizarse para distribuir y mitigar el tráfico de ataques entre regiones.  

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [Asignar una dirección IP pública](https://docs.azure.cloudsimple.com/public-ips/)
