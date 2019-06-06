---
title: Solicitudes de aumento de cuota de vCPU de Azure Resource Manager | Microsoft Docs
description: Solicitudes de aumento de cuota de vCPU de Azure Resource Manager
author: sowmyavenkat86
ms.author: svenkat
ms.date: 05/09/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 9d19d1a993d574777aa630b9c58f2472b94716cd
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66479417"
---
# <a name="resource-manager-vcpu-quota-increase-requests"></a>Solicitudes de aumento de cuota de vCPU de Resource Manager

Se imponen cuotas de vCPU de Resource Manager para máquinas virtuales y conjuntos de escalado de máquinas virtuales en dos niveles para cada suscripción, en cada región. 

El primer nivel es el límite de vCPU regionales totales (a través de todas las Series de máquinas virtuales) y el segundo nivel es el límite de vCPU de Series de máquinas virtuales (por ejemplo, las vCPU de la serie D). Cada vez que es una nueva máquina virtual para su implementación, la suma del uso de vCPU nuevas y existentes de esa serie de máquina virtual no puede superar la cuota de vCPU aprobada para ese determinado más de serie de máquina virtual, el recuento de vCPU total implementado a través de todas las Series de máquina virtual no puede exceder la cuota de vCPU regionales totales  aprobado para la suscripción. Si se supera cualquiera de esas dos cuotas, no se permitirá la implementación de la máquina virtual.
Puede solicitar un aumento del límite de cuota de vCPU para la serie de máquina virtual de Azure portal. Un aumento de la cuota de Series de máquinas virtuales automáticamente aumenta el límite de vCPU regionales totales en la misma cantidad. 

Cuando se crea una nueva suscripción, el valor predeterminado totales regionales vCPU no puede ser igual a la suma de las cuotas de vCPU de forma predeterminada para todos los individuales VM serie este puede producir en una suscripción con suficiente cuota para cada serie de máquina virtual individual que desee implementar , pero la suscripción no tiene suficiente cuota de vCPU regionales totales para todas las implementaciones. En este caso, deberá enviar una solicitud para aumentar de forma explícita el aumento de cuota Total para el límite de región. Límite de vCPU regionales totales no puede superar la suma de cuota aprobada a través de todas las series de máquinas virtuales para la región.

Más información sobre las cuotas en el [página de cuotas de vCPU de máquina Virtual](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) y [suscripción de Azure y límites de servicio](https://aka.ms/quotalimits) página. 

Ahora puede solicitar un aumento a través de **ayuda y soporte técnico** hoja o **usos + cuota** hoja en el portal. 

## <a name="request-quota-increase-using-the-help--support-blade"></a>Aumento de cuota de solicitud mediante la **ayuda y soporte técnico** hoja

Siga las instrucciones siguientes para crear una solicitud de soporte técnico a través de "Ayuda y soporte técnico" de la hoja Azure disponible en el portal de Azure. 

1. Desde https://portal.azure.com, seleccione **ayuda y soporte técnico**.

![Ayuda y soporte técnico](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  Seleccione **Nueva solicitud de soporte técnico**. 

![Nueva solicitud de soporte](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. En la lista desplegable Tipo de problema, elija **límites de servicio y suscripción (cuotas)** .

![Lista desplegable Tipo de problema](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Seleccione la suscripción que necesita una cuota mayor.

![Seleccione la suscripción newSR](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. Seleccione **aumenta el límite de suscripción - máquinas virtuales (vCPU de núcleos) de proceso** en **tipo de cuota** lista desplegable. 

![Seleccione el tipo de cuota](./media/resource-manager-core-quotas-request/select-quota-type.png)

6. En **detalles del problema**, proporcione información adicional para facilitar el proceso de la solicitud haciendo **proporcionan detalles**.

![Proporcione detalles](./media/resource-manager-core-quotas-request/provide-details.png)

7. En el **detalles de la cuota** panel, seleccione el modelo de implementación y seleccione una ubicación.

![Detalles de la cuota DM](./media/resource-manager-core-quotas-request/quota-details.png)

8. Seleccione el **familias de SKU** que requieren un aumento. 

![Familia de SKU](./media/resource-manager-core-quotas-request/sku-family.png)

9. Escriba los nuevos límites que quiere en la suscripción. Para quitar una línea, desactive la SKU de la lista desplegable de familias de SKU o haga clic en el icono de descartar "x". Después de escribir la cuota deseada para cada familia SKU, haga clic en **guardar y continuar** en el panel de detalles de la cuota para continuar con la creación de la solicitud de soporte técnico.

![Nuevos límites](./media/resource-manager-core-quotas-request/new-limits.png)


## <a name="request-quota-increase-at-subscription-level-using-usages--quota"></a>Solicitar aumento de cuota en nivel de suscripción con los usos + cuota

Siga las instrucciones siguientes se usa para crear una solicitud de soporte técnico a través de Azure 'uso y cuota' hoja disponible en el portal de Azure. 

1. En https://portal.azure.com, seleccione **Suscripciones**.

![Suscripciones](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Seleccione la suscripción que necesita una cuota mayor.

![Selección de la suscripción](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Seleccione **Uso y cuotas**.

![Selección de uso y cuotas](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. En la esquina superior derecha, seleccione **Solicitar aumento**.

![Solicitud de aumento](./media/resource-manager-core-quotas-request/request-increase.png)

5. Seleccione **aumenta el límite de suscripción de proceso-VM (núcleos de vCPU)** como tipo de cuota. 

![Rellenado del formulario](./media/resource-manager-core-quotas-request/forms.png)
   
6. En el **detalles de la cuota** panel, seleccione el modelo de implementación y seleccione una ubicación.

![Hoja de problema de Cuota](./media/resource-manager-core-quotas-request/problemstep.png)

7. Seleccione el **familias de SKU** que requieren un aumento.

![Series de SKU seleccionadas](./media/resource-manager-core-quotas-request/sku-family.png)

8. Escriba los nuevos límites que quiere en la suscripción. Para quitar una línea, desactive la SKU de la lista desplegable de familias de SKU o haga clic en el icono de descartar "x". Después de escribir la cuota deseada para cada familia SKU, haga clic en **guardar y continuar** en la página de paso de problema para continuar con la creación de la solicitud de soporte técnico.

![Nueva solicitud de cuota de SKU](./media/resource-manager-core-quotas-request/new-limits.png)


