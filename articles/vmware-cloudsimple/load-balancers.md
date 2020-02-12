---
title: 'Azure VMware Solution: elección de una solución de equilibrio de carga para nubes privadas de AVS'
description: Se describen las opciones de equilibrio de carga que implementan una aplicación en una nube privada de AVS.
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 405bc9d95b8d82e2181e2fb828d6bcc00c8c4639
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77014885"
---
# <a name="choose-a-load-balancing-solution-for-avs-private-clouds"></a>Elección de una solución de equilibrio de carga para nubes privadas de AVS

Al implementar una aplicación en una nube privada de AVS, puede elegir cualquiera de las diversas opciones para el equilibrio de carga.

Puede elegir un equilibrador de carga virtual o basado en software en la nube privada de AVS o incluso usar el equilibrador de carga de Azure L7 que se ejecuta en la suscripción de Azure para ejecutar el front-end de las VM de nivel web que se ejecutan en la nube privada de AVS. A continuación, se enumeran algunas opciones:

## <a name="virtual-load-balancers"></a>Equilibradores de carga virtuales

Puede implementar aplicaciones de equilibrador de carga virtual en su entorno de VMware a través de la interfaz de vCenter y configurarlas para ejecutar el front-end del tráfico de la aplicación.

Algunos proveedores populares son: NginX: http://nginx.org/en/docs/http/load_balancing.html F5- BigIP - Traffic Manager: https://www.f5.com/products/big-ip-services/virtual-editions Citrix ADC: https://www.citrix.com/products/citrix-adc/

## <a name="azure-l7-load-balancer"></a>Equilibrador de carga de Azure L7

Cuando se usa Azure Application Gateway como equilibrador de carga L7 para la aplicación que se ejecuta en una nube privada de AVS, no es necesario administrar el software del equilibrador de carga. Azure administra el software del equilibrador de carga. Todas las VM de nivel web de la nube privada de AVS usan direcciones IP privadas y no requieren reglas NAT adicionales ni direcciones IP públicas para resolver nombres. Las VM de nivel web se comunican con Azure Application Gateway a través de una conexión privada, de baja latencia y de ancho de banda alto.

Para más información sobre cómo configurar esta solución, consulte la guía de soluciones sobre el uso de Azure Application Gateway como equilibrador de carga L7.

## <a name="azure-internal-load-balancer"></a>Equilibrador de carga interno de Azure

Si elige ejecutar la aplicación en una implementación híbrida en la que el nivel de front-end web se está ejecutando en una red virtual de Azure en su suscripción a Azure y el nivel de base de datos de la aplicación se ejecuta en VM de VMware en la nube privada de AVS, puede usar el equilibrador de carga interno de Azure (equilibrador de carga L4) delante de las VM de nivel de base de datos para la administración del tráfico.

Para más información, consulte la documentación de [equilibrador de carga interno](../load-balancer/concepts-limitations.md#internalloadbalancer) de Azure.

## <a name="global-server-load-balancer"></a>Equilibrio de carga del servidor global

Si busca un equilibrador de carga basado en DNS, puede usar soluciones de terceros disponibles en Azure Marketplace o ir a la solución nativa de Azure.

Azure Traffic Manager es un equilibrador de carga de tráfico basado en DNS que le permite distribuir el tráfico de forma óptima a servicios de regiones de Azure globales y en el entorno local, al tiempo que proporciona una alta disponibilidad y capacidad de respuesta. Para más información, consulte la documentación de [Traffic Manager](../traffic-manager/traffic-manager-configure-geographic-routing-method.md) de Azure.
