---
title: Solución de VMware CloudSimple - dirección IP pública de Azure
description: Obtenga información sobre las direcciones IP públicas y sus beneficios de la solución VMware CloudSimple
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: a366ec821e5e9663908f790ab3a88424fa956075
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2019
ms.locfileid: "64576976"
---
# <a name="cloudsimple-public-ip-address-overview"></a>Resumen de dirección IP pública CloudSimple

Una dirección IP pública permite los recursos de internet para la comunicación entrante a los recursos de nube privada en una dirección IP privada. La dirección IP privada es una máquina virtual o un equilibrador de carga de software. Es la dirección IP privada en el nube privada de vCenter. La dirección IP pública permite exponer servicios que se ejecutan en la nube privada a internet.

La dirección IP pública está dedicada a la dirección IP privada hasta que se anula su asignación. Solo se puede asignar una dirección IP pública a una dirección IP privada.

Un recurso asociado con una dirección IP pública siempre usa la dirección IP pública para acceder a internet. De forma predeterminada, se permite solo acceso saliente a internet en una dirección IP pública.  Se deniega el tráfico entrante en la dirección IP pública.  Para permitir el tráfico entrante, cree una regla de firewall para la dirección IP pública en el puerto específico.

## <a name="benefits"></a>Ventajas

Uso de una dirección IP pública para la comunicación entrante proporciona:

* Por denegación de prevención de ataques de servicio (DDoS) distribuido. Esta protección se habilita automáticamente para la dirección IP pública.
* Continua del tráfico en tiempo real y supervisión mitigación de ataques de nivel de red comunes. Estas defensas son la misma defensa que usan Microsoft online Services.
* La escala completa de la red global de Azure. La red puede utilizarse para distribuir y reducir el tráfico de ataques entre regiones.  

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [asignar una dirección IP pública](https://docs.azure.cloudsimple.com/publicips/)
