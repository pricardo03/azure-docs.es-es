---
title: Reducción de los costos de servicio con Azure Advisor | Microsoft Docs
description: Utilice Azure Advisor para optimizar el costo de las implementaciones de Azure.
services: advisor
documentationcenter: NA
author: kasparks
ms.service: advisor
ms.topic: article
ms.date: 01/29/2019
ms.author: kasparks
ms.openlocfilehash: c76c7bdb398184cc297831c9395063e7bf0f6bdc
ms.sourcegitcommit: 5978d82c619762ac05b19668379a37a40ba5755b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55492545"
---
# <a name="reduce-service-costs-using-azure-advisor"></a>Reducción de los costos de servicio con Azure Advisor

Advisor lo ayuda a optimizar y reducir el gasto global de Azure mediante la identificación de recursos inactivos e infrautilizados. Puede obtener recomendaciones sobre el costo en la pestaña **Cost** (Costo) del panel de Advisor.

## <a name="optimize-virtual-machine-spend-by-resizing-or-shutting-down-underutilized-instances"></a>Optimización del gasto en máquinas virtuales mediante la adecuación del tamaño o el apagado en instancias infrautilizadas 

Mientras que determinados escenarios de aplicaciones pueden dar lugar a un uso escaso debido al diseño, a menudo puede ahorrar dinero administrando el tamaño y número de máquinas virtuales. Advisor supervisa la utilización de las máquinas virtuales durante 14 días e identifica aquellas con una utilización escasa. Se considera que una máquina virtual tiene una utilización escasa si su utilización de la CPU es del 5 % o menos y el de la red es de 7 MB o menos durante cuatro o más días.

Advisor muestra el costo estimado de continuar ejecutando la máquina virtual, para que puede elegir entre apagarla o cambiar su tamaño.

Si desea que sea más exigente en la identificación de las máquinas virtuales infrautilizadas, puede ajustar la regla de uso promedio de la CPU según la suscripción.

## <a name="reduce-costs-by-eliminating-unprovisioned-expressroute-circuits"></a>Reducción de los costos mediante la eliminación de circuitos ExpressRoute no aprovisionados

Advisor identifica los circuitos ExpressRoute que han permanecido en el estado de proveedor de *No aprovisionado* durante más de un mes y recomienda eliminar el circuito si no piensa realizar el aprovisionamiento del circuito con el proveedor de conectividad.

## <a name="reduce-costs-by-deleting-or-reconfiguring-idle-virtual-network-gateways"></a>Reducción de costos mediante la eliminación o reconfiguración de puertas de enlace de red virtual inactivas

Advisor identifica las puertas de red virtual que han estado inactivas durante más de 90 días. Dado que estas puertas de enlace se facturan por hora, debe considerar volver a configurar o eliminarlas si no va a utilizarlas. 

## <a name="buy-reserved-virtual-machine-instances-to-save-money-over-pay-as-you-go-costs"></a>Compra de instancias reservadas de máquina virtual para ahorrar dinero en los costos de pago por uso

Advisor revisará el uso de la máquina virtual durante los últimos 30 días y determinará si podría ahorrar dinero mediante la adquisición de una reserva de Azure. Advisor le mostrará las regiones y los tamaños en los que, potencialmente, puede ahorrar más, así como el ahorro estimado al comprar reservas. 

Con las reservas de Azure, puede adquirir previamente los costos de base de las máquinas virtuales. Se aplicarán descuentos automáticamente a las máquinas virtuales nuevas o existentes que tengan el mismo tamaño y la misma región que sus reservas. [Más información sobre Azure Reserved VM Instances](https://azure.microsoft.com/pricing/reserved-vm-instances/).

## <a name="how-to-access-cost-recommendations-in-azure-advisor"></a>Obtención de acceso a las recomendaciones sobre el costo en Azure Advisor

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y después abra [Azure Advisor](https://aka.ms/azureadvisordashboard).

2.  En el panel de Advisor, haga clic en la pestaña **Costo**.

## <a name="next-steps"></a>Pasos siguientes

Para aprender más sobre las recomendaciones de Advisor, consulte:
* [Introducción a Advisor](advisor-overview.md)
* [Primeros pasos](advisor-get-started.md)
* [Recomendaciones sobre rendimiento de Advisor](advisor-cost-recommendations.md)
* [Recomendaciones sobre alta disponibilidad de Advisor](advisor-cost-recommendations.md)
* [Recomendaciones sobre seguridad de Advisor](advisor-cost-recommendations.md)
