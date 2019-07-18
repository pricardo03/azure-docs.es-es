---
title: Solicitudes de aumento de cuota de vCPU por máquina virtual en Azure | Microsoft Docs
description: Solicitudes de aumento de cuota de vCPU por máquina virtual
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 79879cd7f5ea5af1b794735f32e6e1367458e124
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2019
ms.locfileid: "67310725"
---
# <a name="vm-series-vcpu-limit-increase"></a>Aumento del límite de máquinas virtuales de la serie vCPU

Las cuotas de vCPU de Resource Manager para máquinas virtuales y conjuntos de escalado de máquinas virtuales se aplican en dos niveles para cada suscripción en cada región. 

El primer nivel es el **límite de vCPU regionales totales** (en todas las series de máquinas virtuales) y el segundo es el **límite de vCPU por serie de máquinas virtuales** (como las vCPU de la serie D). Cada vez que se va a implementar una nueva máquina virtual, la suma del uso de las vCPU nuevas y existentes de esa serie de máquinas virtuales no debe superar la cuota de vCPU aprobada para la serie. Además, el número total de las vCPU nuevas y existentes implementadas en toda la serie de máquinas virtuales no debería superar la cuota de las vCPU regionales totales aprobada para la suscripción. Si se supera cualquiera de esas dos cuotas, no se permitirá la implementación de la máquina virtual.
Puede solicitar un aumento del límite de la cuota de vCPU para la serie de máquinas virtuales en Azure Portal. Al aumentar la cuota de la serie de máquinas virtuales, se aumenta automáticamente el límite de vCPU regionales totales en la misma cantidad. 

Cuando se crea una suscripción, el total de vCPU regionales predeterminadas no puede ser igual a la suma de las cuotas de vCPU predeterminadas para todas las series individuales de máquinas virtuales. Esto puede dar lugar a una suscripción con suficiente cuota para cada serie individual de máquinas virtuales que quiera implementar, pero sin la cuota suficiente para el total de vCPU regionales de todas las implementaciones. En este caso, deberá enviar una solicitud para aumentar explícitamente el límite del total de vCPU regionales. El límite del total de vCPU regionales no puede superar la suma de la cuota aprobada en todas las series de máquinas virtuales de la región.

Para obtener más información sobre las cuotas, visite las páginas [Cuotas de vCPU de máquinas virtuales](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) y [Límites de suscripción y servicios de Azure](https://aka.ms/quotalimits). 

Ahora puede solicitar un aumento a través de las hojas **Ayuda y soporte técnico** y **Usages + Quota** (Usos y cuota) en el portal. 

## <a name="request-per-vm-series-vcpu-quota-increase-at-subscription-level-using-the-help--support-blade"></a>Solicitud de un aumento de cuota de vCPU por serie de máquinas virtuales en el nivel de suscripción a través de la hoja **Ayuda y soporte técnico**

Siga las instrucciones siguientes para crear una solicitud de soporte técnico mediante la hoja Ayuda y soporte técnico de Azure disponible en Azure Portal. 

1. En https://portal.azure.com, seleccione **Ayuda y soporte técnico**.

![Ayuda y soporte técnico](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  Seleccione **Nueva solicitud de soporte técnico**. 

![Nueva solicitud de soporte](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. En el menú desplegable Tipo de problema, seleccione **Límites de servicio y suscripción (cuotas)** .

![Menú desplegable Tipo de problema](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Seleccione la suscripción que necesita una cuota mayor.

![Selección de la suscripción en la nueva solicitud de soporte técnico](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. Seleccione **Compute-VM (cores-vCPUs) subscription limit increases** (El límite de la suscripción a proceso/máquina virtual (núcleos/vCPU) aumenta) en el menú desplegable **Tipo de cuota**. 

![Selección del tipo de cuota](./media/resource-manager-core-quotas-request/select-quota-type.png)

6. En **Detalles del problema**, haga clic en **Proporcionar detalles** para aportar información adicional que ayude a procesar la solicitud.

![Proporcionar detalles](./media/resource-manager-core-quotas-request/provide-details.png)

7. En el panel **Detalles de la cuota**, seleccione el modelo de implementación y una ubicación.

![Modelo de implementación en Detalles de la cuota](./media/resource-manager-core-quotas-request/quota-details.png)

8. Seleccione las **familias de SKU** que requieren un aumento. 

![Familia de SKU](./media/resource-manager-core-quotas-request/sku-family.png)

9. Escriba los nuevos límites que quiere en la suscripción. Para quitar una línea, desactive la SKU de la lista desplegable de familias de SKU o haga clic en el icono de descartar "x". Después de escribir la cuota deseada para cada familia de SKU, haga clic en **Guardar y continuar** en el panel Detalles de la cuota para continuar con la creación de la solicitud de soporte técnico.

![Nuevos límites](./media/resource-manager-core-quotas-request/new-limits.png)


## <a name="request-per-vm-series-vcpu-quota-increase-at-subscription-level-using-usages--quota-blade"></a>Solicitud de un aumento de cuota de vCPU por serie de máquinas virtuales en el nivel de suscripción a través de la hoja **Usages + Quota** (Usos y cuota)

Siga las instrucciones siguientes para crear una solicitud de soporte técnico mediante la hoja de uso y cuota de Azure disponible en Azure Portal. 

1. En https://portal.azure.com, seleccione **Suscripciones**.

![Suscripciones](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Seleccione la suscripción que necesita una cuota mayor.

![Selección de la suscripción](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Seleccione **Uso y cuotas**.

![Selección de uso y cuotas](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. En la esquina superior derecha, seleccione **Solicitar aumento**.

![Solicitud de aumento](./media/resource-manager-core-quotas-request/request-increase.png)

5. Seleccione **Compute-VM (cores-vCPUs) subscription limit increases** (El límite de la suscripción a proceso/máquina virtual (núcleos/vCPU) aumenta) como tipo de cuota. 

![Rellenado del formulario](./media/resource-manager-core-quotas-request/forms.png)
   
6. En el panel **Detalles de la cuota**, seleccione el modelo de implementación y una ubicación.

![Hoja de problema de Cuota](./media/resource-manager-core-quotas-request/problemstep.png)

7. Seleccione las **familias de SKU** que requieren un aumento.

![Series de SKU seleccionadas](./media/resource-manager-core-quotas-request/sku-family.png)

8. Escriba los nuevos límites que quiere en la suscripción. Para quitar una línea, desactive la SKU de la lista desplegable de familias de SKU o haga clic en el icono de descartar "x". Después de escribir la cuota deseada para cada familia de SKU, haga clic en **Guardar y continuar** en la página de pasos del problema para continuar con la creación de la solicitud de soporte técnico.

![Nueva solicitud de cuota de SKU](./media/resource-manager-core-quotas-request/new-limits.png)

