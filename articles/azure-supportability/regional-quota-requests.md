---
title: Solicitudes de aumento de cuota regional de Azure | Microsoft Docs
description: Solicitudes de aumento de cuota regional
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: e73f22b0e617ad8f20b98c3bb0fb1647bf5fe61d
ms.sourcegitcommit: 961468fa0cfe650dc1bec87e032e648486f67651
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2019
ms.locfileid: "72249071"
---
# <a name="total-regional-vcpu-limit-increase"></a>Aumento del límite de vCPU regionales totales 

Las cuotas de vCPU de Resource Manager para máquinas virtuales y conjuntos de escalado de máquinas virtuales se aplican en dos niveles para cada suscripción en cada región. 

El primer nivel es el **límite de vCPU regionales totales** (en todas las series de máquinas virtuales) y el segundo es el **límite de vCPU por serie de máquinas virtuales** (como las vCPU de la serie D). Cada vez que se va a implementar una nueva máquina virtual, la suma del uso de las vCPU nuevas y existentes de esa serie de máquinas virtuales no debe superar la cuota de vCPU aprobada para la serie. Además, el número total de las vCPU nuevas y existentes implementadas en todas las series de máquinas virtuales no debería superar la cuota de las vCPU regionales totales aprobada para la suscripción. Si se supera cualquiera de esas dos cuotas, no se permitirá la implementación de la máquina virtual.
Puede solicitar un aumento del límite de la cuota de vCPU para la serie de máquinas virtuales en Azure Portal. Al aumentar la cuota de la serie de máquinas virtuales, se aumenta automáticamente el límite de vCPU regionales totales en la misma cantidad. 

Cuando se crea una nueva suscripción, el total de vCPU regionales predeterminadas no puede ser igual a la suma de las cuotas de vCPU predeterminadas para todas las series individuales de máquinas virtuales. Esto puede dar lugar a una suscripción con suficiente cuota para cada serie individual de máquinas virtuales que quiera implementar, pero sin la cuota suficiente para el total de vCPU regionales de todas las implementaciones. En este caso, deberá enviar una solicitud para aumentar explícitamente el límite del total de vCPU regionales. El límite del total de vCPU regionales no puede superar la suma de la cuota aprobada en todas las series de máquinas virtuales de la región.

Para más información sobre las cuotas, consulte las páginas [Cuotas de vCPU de máquinas virtuales](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) y [Límites de suscripción y servicios de Azure](https://aka.ms/quotalimits). 

Ahora puede solicitar un aumento mediante las hojas **Ayuda y soporte técnico** y **Usos y cuota** en el portal. 

## <a name="request-total-regional-vcpus-quota-increase-at-subscription-level-using-the-help--support-blade"></a>Solicitud de un aumento de cuota de vCPU regional total en el nivel de suscripción mediante la hoja **Ayuda y soporte técnico**

Siga las instrucciones siguientes para crear una solicitud de soporte técnico mediante la hoja Ayuda y soporte técnico de Azure disponible en Azure Portal. 

1. En https://portal.azure.com, seleccione **Ayuda y soporte técnico**.

![Ayuda y soporte técnico](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  Seleccione **Nueva solicitud de soporte técnico**. 

![Nueva solicitud de soporte](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. En el menú desplegable Tipo de incidencia, seleccione **Límites de servicio y suscripción (cuotas)** .

![Menú desplegable Tipo de incidencia](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Seleccione la suscripción que necesita una cuota mayor.

![Selección de la suscripción en la nueva solicitud de soporte técnico](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. Seleccione **Otras solicitudes** en la lista desplegable **Tipo de cuota**.

![Tipo de cuota](./media/resource-manager-core-quotas-request/regional-quotatype.png)

6. En el panel **Detalles**, proporcione información adicional según el ejemplo siguiente, para ayudar en el proceso de la solicitud y continuar con la creación del caso. 
    1.  **Modelo de implementación**: especifique "Resource Manager".
    2.  **Región solicitada**: especifique la región solicitada; por ejemplo, Este de EE. UU. 2.
    3.  **Nuevo valor del límite**: especifique el nuevo límite de región. Este no debería superar la suma de las cuotas aprobadas para las familias de SKU individuales de esta suscripción.

![Detalles de la cuota](./media/resource-manager-core-quotas-request/regional-details.png)

## <a name="request-total-regional-vcpus-quota-increase-at-subscription-level-using-the-usages--quota-blade"></a>Solicitud de un aumento de cuota de vCPU regional total en el nivel de suscripción mediante la hoja **Usos y cuota**

Siga las instrucciones siguientes para crear una solicitud de soporte técnico mediante la hoja "Usos y cuota" de Azure disponible en Azure Portal. 

1. En https://portal.azure.com, seleccione **Suscripciones**.

![Suscripciones](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Seleccione la suscripción que necesita una cuota mayor.

![Selección de la suscripción](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Seleccione **Uso y cuotas**.

![Selección de uso y cuotas](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. En la esquina superior derecha, seleccione **Solicitar aumento**.

![Solicitud de aumento](./media/resource-manager-core-quotas-request/request-increase.png)

5. Seleccione **Otras solicitudes** en la lista desplegable **Tipo de cuota**.

![Tipo de cuota](./media/resource-manager-core-quotas-request/regional-quotatype.png)

6. En el panel **Detalles**, proporcione información adicional según el ejemplo siguiente, para ayudar en el proceso de la solicitud y continuar con la creación del caso. 
    1.  **Modelo de implementación**: especifique "Resource Manager".
    2.  **Región solicitada**: especifique la región solicitada; por ejemplo, Este de EE. UU. 2.
    3.  **Nuevo valor del límite**: especifique el nuevo límite de región. Este no debería superar la suma de las cuotas aprobadas para las familias de SKU individuales de esta suscripción.

![Detalles de la cuota](./media/resource-manager-core-quotas-request/regional-details.png)



