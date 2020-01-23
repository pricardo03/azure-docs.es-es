---
title: Solicitud de un aumento en los límites de cuota de CPU virtual por serie de máquinas virtuales de Azure | Microsoft Docs
description: En este artículo se describe cómo solicitar aumentos en los límites de cuota por CPU virtual de máquinas virtuales.
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: fc59ffda245834c716067bc63e6c3745fa5d23b3
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2020
ms.locfileid: "75898848"
---
# <a name="standard-quota-increase-limits-by-vm-series"></a>Cuota estándar: Aumento de los límites por serie de máquinas virtuales

Azure Resource Manager admite dos tipos de cuotas de vCPU para las máquinas virtuales:
* Las *máquinas virtuales de pago por uso* y las *instancias reservadas de máquina virtual* están sujetas a una *cuota de vCPU estándar*.
* Las *máquinas virtuales al contado* están sujetas a una *cuota de vCPU al contado*. 

La cuota de vCPU estándar para las instancias reservadas de máquina virtual y de pago por uso se aplica en dos niveles para cada suscripción de cada región:
* El primer nivel es el *límite de vCPU regionales totales* (en todas las series de máquinas virtuales).
* El segundo nivel es el *límite de vCPU por serie de máquinas virtuales* (por ejemplo, las vCPU de la serie Dv3). 

Cada vez que implemente una nueva máquina virtual al contado, el uso total de las vCPU nuevas y existentes de esa serie de máquinas virtuales no deberá superar la cuota de vCPU aprobada para esa serie. Además, el número total de vCPU nuevas y existentes que se implementan en todas las series de máquinas virtuales no debe superar la cuota de vCPU regionales totales aprobada para la suscripción. Si se supera alguna de estas cuotas, no se permitirá la implementación de la máquina virtual.

Puede solicitar un aumento del límite de cuota de vCPU para la serie de máquinas virtuales a través de Azure Portal. Al aumentar la cuota de la serie de máquinas virtuales, se aumenta automáticamente el límite de vCPU regionales totales en la misma cantidad. 

Para más información sobre las cuotas de vCPU estándar, consulte los artículos [Cuotas de vCPU de máquinas virtuales](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) y [Suscripción a Azure y límites de servicio](https://docs.microsoft.com/azure/azure-supportability/classic-deployment-model-quota-increase-requests). 

Para información sobre cómo aumentar el límite de vCPU por región para la cuota estándar, consulte [Cuota estándar: aumento de los límites por región](https://docs.microsoft.com/azure/azure-supportability/regional-quota-requests). 

Para más información sobre cómo aumentar los límites de vCPU de máquinas virtuales al contado, consulte el artículo [Cuota al contado: aumento de los límites para todas las series de máquinas virtuales](https://docs.microsoft.com/azure/azure-supportability/low-priority-quota).

Puede solicitar un aumento de los límites de cuota de vCPU estándar por serie de máquinas virtuales de dos maneras, las que se describen en las secciones siguientes.

## <a name="request-a-standard-quota-increase-from-the-help--support-pane"></a>Solicitud de un aumento de la cuota estándar desde el panel "Ayuda y soporte técnico"

Para solicitar un aumento de cuota de vCPU estándar por serie de máquinas virtuales en el panel **Ayuda y soporte técnico**, haga lo siguiente: 

> [!NOTE]
> También puede solicitar un aumento del límite de cuota para varias regiones a través de un único caso de soporte técnico. Para más información, consulte el paso 8.

1. En el panel izquierdo de [Azure Portal](https://portal.azure.com), seleccione **Ayuda y soporte técnico**.

   ![El vínculo de "Ayuda y soporte técnico"](./media/resource-manager-core-quotas-request/helpsupport.png)
 
1. En el panel **Ayuda y soporte técnico**, seleccione **Nueva solicitud de soporte técnico**. 

    ![Nueva solicitud de soporte](./media/resource-manager-core-quotas-request/newsupportrequest.png)

1. En la lista desplegable **Tipo de problema**, seleccione **Límites de servicio y suscripción (cuotas)** .

   ![La lista desplegable "Tipo de problema"](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

1. En la lista desplegable **Suscripción**, seleccione la suscripción cuya cuota quiere aumentar.

   ![La lista desplegable "Suscripción"](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
1. En la lista desplegable **Tipo de cuota**, seleccione **El límite de proceso-máquina virtual (núcleos-vCPU) de la suscripción aumenta**. 

   ![La lista desplegable "Tipo de cuota"](./media/resource-manager-core-quotas-request/select-quota-type.png)

1. En la pestaña **Detalles**, en **Detalles del problema**, seleccione **Proporcionar detalles** y escriba información adicional para ayudar a procesar la solicitud.

   ![El vínculo "Proporcionar detalles"](./media/resource-manager-core-quotas-request/provide-details.png)

1. En el panel **Detalles de la cuota** en la parte superior derecha, haga lo siguiente:

   ![El panel "Detalles de la cuota"](./media/resource-manager-core-quotas-request/1-7.png)

   a. En la lista desplegable **Modelo de implementación**, seleccione el modelo adecuado.

   b. En la lista desplegable **Ubicaciones**, seleccione una ubicación. En la ubicación seleccionada, debajo de **Tipos**, en el cuadro **Seleccione un tipo**, escriba **Estándar**.

   ![El panel "Detalles de la cuota": tipos de cuota](./media/resource-manager-core-quotas-request/1-8.png)

   En **Tipos**, puede solicitar los dos tipos de cuota (Estándar y Al contado) desde una única solicitud de soporte técnico mediante la selección múltiple.
   
   Para más información sobre cómo aumentar los límites de cuota al contado, consulte [Máquinas virtuales de Azure Spot para los conjuntos de escalado](https://docs.microsoft.com/azure/virtual-machine-scale-sets/use-spot).

   c. Debajo de la lista desplegable **estándar**, seleccione la serie de SKU cuyas cuotas quiere aumentar.

   ![El panel "Detalles de cuota": serie de SKU](./media/resource-manager-core-quotas-request/1-9.png)

   d. Escriba el nuevo límite de cuota que quiere para esta suscripción. Para quitar una SKU de la lista, desactive la casilla junto a la SKU o seleccione el icono **Eliminar** (X). 

   ![El cuadro de texto "Nuevo límite de vCPU"](./media/resource-manager-core-quotas-request/1-10.png)

1. Para solicitar un aumento de cuota en más de una ubicación, seleccione una ubicación adicional en la lista desplegable y, a continuación, seleccione el tipo de máquina virtual adecuado. Después, puede especificar el límite que se aplicará a la ubicación adicional.

   ![Ubicaciones adicionales en el panel "Detalles de la cuota"](./media/resource-manager-core-quotas-request/1-11.png)
   
1. Seleccione **Guardar y continuar** para continuar con la creación de la solicitud de soporte técnico.

## <a name="request-a-standard-quota-increase-from-the-subscriptions-pane"></a>Solicitud de un aumento de la cuota estándar desde el panel "Suscripciones"

Para solicitar un aumento de cuota de vCPU estándar por serie de máquinas virtuales en el panel **Suscripciones**, haga lo siguiente:

> [!NOTE]
> También puede solicitar un aumento del límite de cuota para varias regiones a través de un único caso de soporte técnico. Para más información, consulte el paso 7.

1. En el panel izquierdo de [Azure Portal](https://portal.azure.com), seleccione **Suscripciones**.

   ![El vínculo "Suscripciones"](./media/resource-manager-core-quotas-request/subscriptions.png)

1. Seleccione la suscripción cuya cuota quiere aumentar.

   ![El panel "Suscripciones"](./media/resource-manager-core-quotas-request/select-subscription.png)

1. En el panel izquierdo de la página de **\<Nombre de su suscripción>** , seleccione **Uso y cuotas**.

   ![El vínculo "Uso y cuotas"](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

1. En la parte superior derecha, seleccione **Solicitar aumento**.

   ![Solicitud de aumento](./media/resource-manager-core-quotas-request/request-increase.png)

1. En la lista desplegable **Tipo de cuota**, seleccione **El límite de proceso-máquina virtual (núcleos-vCPU) de la suscripción aumenta**.

   ![La lista desplegable "Tipo de cuota"](./media/resource-manager-core-quotas-request/select-quota-type.png)
   
1. En el panel **Detalles de la cuota** en la parte superior derecha, haga lo siguiente:

   ![El panel "Detalles de la cuota"](./media/resource-manager-core-quotas-request/1-1-6.png)

   a. En la lista desplegable **Modelo de implementación**, seleccione el modelo adecuado.

   b. En la lista desplegable **Ubicaciones**, seleccione una ubicación. 
   
   c. En la ubicación seleccionada, en **Tipos**, seleccione **Seleccionar un tipo** y, a continuación, active la casilla **Estándar**.

   ![La casilla "Estándar"](./media/resource-manager-core-quotas-request/1-1-7.png)
   
   En **Tipos**, puede solicitar los dos tipos de cuota (Estándar y Prioridad baja) desde una única solicitud de soporte técnico mediante la selección múltiple.
   
   Para más información sobre cómo aumentar los límites de cuota al contado, consulte [Máquinas virtuales de Azure Spot para los conjuntos de escalado](https://docs.microsoft.com/azure/virtual-machine-scale-sets/use-spot).

   d. Debajo de la lista desplegable **estándar**, seleccione la serie de SKU cuyas cuotas quiere aumentar.

   ![El panel "Detalles de cuota": serie de SKU](./media/resource-manager-core-quotas-request/1-1-8.png)

   e. Escriba el nuevo límite de cuota que quiere para esta suscripción. Para quitar una SKU de la lista, desactive la casilla junto a la SKU o seleccione **Eliminar** (X). 

   ![El cuadro de texto "Nuevo límite de vCPU"](./media/resource-manager-core-quotas-request/1-1-9.png)
   
1. Para solicitar un aumento de cuota en más de una ubicación, seleccione una ubicación adicional en la lista desplegable y, a continuación, seleccione el tipo de máquina virtual adecuado. 

   Este paso carga previamente la serie de SKU seleccionada para las ubicaciones anteriores. Escriba los límites de cuota que quiere aplicar a la serie adicional.
   
   ![Ubicaciones adicionales en el panel "Detalles de la cuota"](./media/resource-manager-core-quotas-request/1-1-10.png)
 
1. Seleccione **Guardar y continuar** para continuar con la creación de la solicitud de soporte técnico.
