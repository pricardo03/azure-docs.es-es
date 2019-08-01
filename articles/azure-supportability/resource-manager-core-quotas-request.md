---
title: Solicitudes de aumento de cuota de vCPU de Azure Resource Manager | Microsoft Docs
description: Solicitudes de aumento de cuota de vCPU de Azure Resource Manager
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 9a997af984b92ea59cc02d99fbd66d8967ca31bd
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67076808"
---
# <a name="quota-increase-requests"></a>Solicitudes de aumento de cuota

Las cuotas de vCPU de Resource Manager para máquinas virtuales y conjuntos de escalado de máquinas virtuales se aplican en dos niveles para cada suscripción en cada región. 

El primer nivel es el límite de vCPU regionales totales (en todas las series de máquinas virtuales) y el segundo es el límite de vCPU por serie de máquinas virtuales (como las vCPU de la serie D). Cada vez que se va a implementar una nueva máquina virtual, la suma del uso de las vCPU nuevas y existentes de esa serie de máquinas virtuales no debe superar la cuota de vCPU aprobada para la serie. Además, el número total de las vCPU nuevas y existentes implementadas en todas las series de máquinas virtuales no debería superar la cuota de las vCPU regionales totales aprobada para la suscripción. Si se supera cualquiera de esas dos cuotas, no se permitirá la implementación de la máquina virtual.
Puede solicitar un aumento del límite de la cuota de vCPU para la serie de máquinas virtuales en Azure Portal. Al aumentar la cuota de la serie de máquinas virtuales, se aumenta automáticamente el límite de vCPU regionales totales en la misma cantidad. 

Cuando se crea una nueva suscripción, el total de vCPU regionales predeterminadas no puede ser igual a la suma de las cuotas de vCPU predeterminadas para todas las series individuales de máquinas virtuales. Esto puede dar lugar a una suscripción con suficiente cuota para cada serie individual de máquinas virtuales que quiera implementar, pero sin la cuota suficiente para el total de vCPU regionales de todas las implementaciones. En este caso, deberá enviar una solicitud para aumentar explícitamente el límite del total de vCPU regionales. El límite del total de vCPU regionales no puede superar la suma de la cuota aprobada en todas las series de máquinas virtuales de la región.

Para más información acerca de las cuotas, consulte las páginas [Cuotas de vCPU de máquinas virtuales](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) y [Límites de suscripción y servicios de Azure](https://aka.ms/quotalimits). 

