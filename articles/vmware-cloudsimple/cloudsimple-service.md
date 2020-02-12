---
title: 'Azure VMware Solutions (AVS): servicio'
description: Proporciona información general de los conceptos y el servicio de AVS.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: d09c8c34093e7d33122f934138ff9fdf4842508e
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024965"
---
# <a name="avs-service-overview"></a>Información general del servicio de AVS

El servicio de AVS le permite consumir Azure VMware Solution by AVS. La creación del servicio permite adquirir nodos, reservarlos y crear nubes privadas de AVS. El servicio de AVS se crea en cada región de Azure donde está disponible. El servicio define la red perimetral de Azure VMware Solution by AVS. La red perimetral admite servicios que incluyen VPN, ExpressRoute y conectividad de Internet a las nubes privadas de AVS.

## <a name="gateway-subnet"></a>Subred de puerta de enlace

Se requiere una subred de puerta de enlace por cada servicio de AVS y es única para la región en la que se crea. La subred de puerta de enlace se usa al crear la red perimetral y requiere un bloque CIDR /28. El espacio de direcciones de subred de puerta de enlace debe ser único. No se debe superponer con otras redes que se comuniquen con el entorno de AVS. Las redes que se comunican con AVS incluyen redes locales y la red virtual de Azure. Después de crear una subred de puerta de enlace no se puede eliminar. La subred de puerta de enlace se quita cuando se elimina el servicio.

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre cómo [crear un servicio de AVS en Azure](quickstart-create-cloudsimple-service.md).
