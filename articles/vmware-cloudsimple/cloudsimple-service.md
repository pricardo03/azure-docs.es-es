---
title: Solución de VMware del servicio CloudSimple
description: Se describe la información general de los conceptos y el servicio CloudSimple.
author: sharaths-cs
ms.author: b-shsury
ms.date: 04/24/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 0bebc68129ee2ff79241bcefec1ce0c3ca0b472d
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67595312"
---
# <a name="cloudsimple-service-overview"></a>Información general del servicio CloudSimple

Con el servicio CloudSimple, puede usar la Solución de Azure VMware de CloudSimple. Después de crear el servicio, puede aprovisionar nodos y reservarlos, y crear nubes privadas. El servicio CloudSimple se crea en cada región de Azure donde está disponible. El servicio define la red perimetral de la solución de Azure VMware de CloudSimple. La red perimetral admite servicios que incluyen VPN, Azure ExpressRoute y conectividad de Internet a las nubes privadas.

## <a name="gateway-subnet"></a>Subred de puerta de enlace

Se requiere una subred de puerta de enlace por cada servicio CloudSimple y es única para la región en la que se crea. La subred de puerta de enlace se usa al crear la red perimetral y requiere un bloque CIDR /28. El espacio de direcciones de subred de puerta de enlace debe ser único. No se debe superponer con otras redes que se comuniquen con el entorno de CloudSimple. Las redes que se comunican con CloudSimple incluyen redes locales y redes virtuales de Azure. Después de crear una subred de puerta de enlace no se puede eliminar. La subred de puerta de enlace se quita cuando se elimina el servicio.

## <a name="next-steps"></a>Pasos siguientes

* Más información sobre cómo [crear un servicio CloudSimple en Azure](quickstart-create-cloudsimple-service.md).
