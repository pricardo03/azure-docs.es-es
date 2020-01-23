---
title: Solicitud del aumento del límite de cuota regional para vCPU de Azure | Microsoft Docs
description: Solicitudes de aumento de cuota regional
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: e262651a6e040c40dbe240ad3437eff1914aa3e5
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2020
ms.locfileid: "75898692"
---
# <a name="standard-quota-increase-limits-by-region"></a>Cuota estándar: aumento de los límites por región 

Azure Resource Manager admite dos tipos de cuotas de vCPU para máquinas virtuales:
* Las *máquinas virtuales de pago por uso* y las *instancias reservadas de máquina virtual* están sujetas a una *cuota de vCPU estándar*.
* Las *máquinas virtuales al contado* están sujetas a una *cuota de vCPU al contado*. 

La cuota de vCPU estándar para las instancias reservadas de máquina virtual y de pago por uso se aplica en dos niveles para cada suscripción de cada región:
* El primer nivel es el *límite de vCPU regionales totales* (en todas las series de máquinas virtuales).
* El segundo nivel es el *límite de vCPU por serie de máquinas virtuales* (por ejemplo, las vCPU de la serie D).
 
Cada vez implemente una nueva máquina virtual al contado, el uso total de las vCPU nuevas y existentes de esa serie de máquinas virtuales no debe superar la cuota de vCPU aprobada para esa serie. Además, el número total de vCPU nuevas y existentes que se implementan en todas las series de máquinas virtuales no debe superar la cuota de vCPU regionales totales aprobada para la suscripción. Si se supera alguna de estas cuotas, no se permite la implementación de la máquina virtual. 

Puede solicitar un aumento del límite de cuota de vCPU para la serie de máquinas virtuales a través de Azure Portal. Al aumentar la cuota de la serie de máquinas virtuales, se aumenta automáticamente el límite de vCPU regionales totales en la misma cantidad.

Cuando crea una nueva suscripción, el número total predeterminado de vCPU regionales no puede ser igual a la suma de las cuotas de vCPU predeterminadas para todas las series individuales de máquinas virtuales. Esta discrepancia puede dar lugar a una suscripción con suficiente cuota para cada serie individual de máquinas virtuales que quiera implementar. Pero es posible que no haya cuota suficiente para el total de vCPU regionales de todas las implementaciones. En este caso, debe enviar una solicitud para aumentar explícitamente el límite del total de vCPU regionales. El límite de vCPU regionales totales no puede superar el total de la cuota aprobada en todas las series de máquinas virtuales de la región.

Para más información sobre las cuotas de vCPU estándar, vea los artículos [Cuotas de vCPU de máquinas virtuales](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) y [Límites, cuotas y restricciones de suscripción y servicios de Microsoft Azure](https://aka.ms/quotalimits).

Para más información sobre cómo aumentar los límites de vCPU de máquinas virtuales al contado, vea el artículo [Cuota al contado: aumento de los límites para todas las series de máquinas virtuales](https://docs.microsoft.com/azure/azure-supportability/low-priority-quota).

Puede solicitar un aumento del límite de cuota de vCPU estándar por región de dos maneras, las cuales se describen en las secciones siguientes.

## <a name="request-a-quota-increase-by-region-from-the-help--support-pane"></a>Solicitud de un aumento de la cuota por región desde el panel "Ayuda y soporte técnico"

Para solicitar un aumento de la cuota de vCPU por región en el panel **Ayuda y soporte técnico**, haga lo siguiente: 

1. En el panel izquierdo de [Azure Portal](https://portal.azure.com), seleccione **Ayuda y soporte técnico**.

   ![El vínculo de "Ayuda y soporte técnico"](./media/resource-manager-core-quotas-request/helpsupport.png)
 
1. En el panel **Ayuda y soporte técnico**, haga clic en **Nueva solicitud de soporte técnico**. 

    ![Nueva solicitud de soporte](./media/resource-manager-core-quotas-request/newsupportrequest.png)

1. En la lista desplegable **Tipo de problema**, seleccione **Límites de servicio y suscripción (cuotas)** .

   ![La lista desplegable "Tipo de problema"](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

1. En la lista desplegable **Suscripción**, seleccione la suscripción cuya cuota quiera aumentar.

   ![La lista desplegable "Suscripción"](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
1. En la lista desplegable **Tipo de cuota**, seleccione **Otras solicitudes**.

   ![La lista desplegable "Tipo de cuota"](./media/resource-manager-core-quotas-request/regional-quotatype.png)

1. En el panel **Detalles del problema**, en el cuadro **Descripción**, proporcione la siguiente información adicional: 

    a. Especifique el **modelo de implementación**, en este caso **Resource Manager**.  
    b. Especifique la **región** que corresponda, por ejemplo, **Este de EE. UU. 2**.  
    c. Especifique el **nuevo límite** de vCPU para la región. Este valor no debería superar la suma de las cuotas aprobadas para las series de SKU individuales de esta suscripción.

    ![El panel "Detalles del problema"](./media/resource-manager-core-quotas-request/regional-details.png)

1. Seleccione **Guardar y continuar** para continuar con la creación de la solicitud de soporte técnico.

## <a name="request-a-quota-increase-by-region-from-the-subscriptions-pane"></a>Solicitud de un aumento de la cuota por región desde el panel "Suscripciones"

Para solicitar un aumento de la cuota de vCPU por región en el panel **Suscripciones**, haga lo siguiente: 

1. En el panel izquierdo de [Azure Portal](https://portal.azure.com), seleccione **Suscripciones**.

   ![El vínculo "Suscripciones"](./media/resource-manager-core-quotas-request/subscriptions.png)

1. Seleccione la suscripción cuya cuota quiere aumentar.

   ![El panel "Suscripciones"](./media/resource-manager-core-quotas-request/select-subscription.png)

1. En el panel izquierdo de la página de **\<nombre de su suscripción>** , seleccione **Uso y cuotas**.

   ![El vínculo "Uso y cuotas"](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

1. En la parte superior derecha, haga clic en **Solicitar aumento**.

   ![Solicitud de aumento](./media/resource-manager-core-quotas-request/request-increase.png)

1. En la lista desplegable **Tipo de cuota**, seleccione **Otras solicitudes**.

   ![La lista desplegable "Tipo de cuota"](./media/resource-manager-core-quotas-request/regional-quotatype.png)

1. En el panel **Detalles del problema**, en el cuadro **Descripción**, proporcione la siguiente información adicional: 

    a. Especifique el **modelo de implementación**, en este caso **Resource Manager**.  
    b. Especifique la **región** que corresponda, por ejemplo, **Este de EE. UU. 2**.  
    c. Especifique el **nuevo límite** de vCPU para la región. Este valor no debería superar la suma de las cuotas aprobadas para las series de SKU individuales de esta suscripción.

    ![El panel "Detalles del problema"](./media/resource-manager-core-quotas-request/regional-details.png)

1. Seleccione **Guardar y continuar** para continuar con la creación de la solicitud de soporte técnico.

