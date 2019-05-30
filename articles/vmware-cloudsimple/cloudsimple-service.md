---
title: Soluciones de VMware CloudSimple - servicio
description: Describe la información general de conceptos y servicio CloudSimple.
author: sharaths-cs
ms.author: b-shsury
ms.date: 4/24/19
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: f7b4be0ff3997e27dd5b5321dd44b5006ae52102
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358114"
---
# <a name="cloudsimple-service-overview"></a>Información general sobre CloudSimple Service

El servicio CloudSimple permite utilizar la solución de VMware de Azure por CloudSimple.  Creación del servicio le permite adquirir nodos, reserva de nodos y crear nubes privadas.  Cree el servicio CloudSimple en cada región de Azure donde está disponible el servicio CloudSimple.  El servicio define la red perimetral de la solución de VMware de Azure por CloudSimple.  La red perimetral es compatible con los servicios que incluyen conectividad a internet para las nubes privadas, ExpressRoute y VPN.

## <a name="gateway-subnet"></a>Subred de puerta de enlace

Una subred de puerta de enlace es un servicio CloudSimple y es única para la región en la que se crea. La subred de puerta de enlace se usa al crear la red perimetral y requiere una de/28 bloque CIDR.  El espacio de direcciones de subred de puerta de enlace debe ser único. No debe superponerse con cualquier red que se comunica con el entorno CloudSimple. Las redes que se comunican con CloudSimple incluyen redes locales y red virtual de Azure.  No se puede eliminar una subred de puerta de enlace una vez que se crea.  La subred de puerta de enlace se quita cuando se elimina el servicio.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [crear un servicio CloudSimple en Azure](quickstart-create-cloudsimple-service.md)