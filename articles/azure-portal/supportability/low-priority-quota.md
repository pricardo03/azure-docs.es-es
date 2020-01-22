---
title: Cuotas de máquina virtual al contado | Microsoft Docs
description: Aumente los límites de cuota mediante solicitudes de cuota al contado
author: sowmyavenkat86
ms.author: svenkat
ms.date: 11/19/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 9567df95e4e66d9c6b82b29cb9bba9adaf39349d
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2020
ms.locfileid: "75898864"
---
# <a name="spot-quota-increase-limits-for-all-vm-series"></a>Cuota al contado: aumento de los límites para todas las series de máquinas virtuales

Las máquinas virtuales (VM) al contado ofrecen un modelo de uso de Azure diferente. Permiten asumir costos menores a cambio de que Azure pueda quitar máquinas virtuales según sea necesario para las implementaciones de pago por uso o instancia reservada de máquina virtual. Para obtener más información acerca de las máquinas virtuales al contado, consulte [Máquinas virtuales de Azure Spot para los conjuntos de escalado](https://docs.microsoft.com/azure/virtual-machine-scale-sets/use-spot).

Azure Resource Manager admite dos tipos de cuotas de vCPU para las máquinas virtuales:
* Las *máquinas virtuales de pago por uso* y las *instancias reservadas de máquina virtual* están sujetas a una *cuota de vCPU estándar*.
* Las *máquinas virtuales al contado* están sujetas a una *cuota de vCPU al contado*. 

Para el tipo de *cuota de vCPU al contado*, las cuotas de vCPU de Resource Manager se aplican en todas las series de máquinas virtuales disponibles como un límite regional único.

Cada vez que se implementa una nueva máquina virtual al contado, el total de uso de vCPU nuevo y existente para todas las instancias de máquina virtual al contado no debe superar el límite de cuota de vCPU al contado aprobado. Si se supera la cuota al contado, no se permite la implementación de máquinas virtuales al contado. 

En este artículo se explica cómo solicitar un aumento en el límite de cuota de vCPU al contado con Azure Portal. 

Para obtener más información sobre las cuotas de vCPU estándar, consulte los artículos [Cuotas de vCPU de máquinas virtuales](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) y [Suscripción a Azure y límites de servicio](https://aka.ms/quotalimits). 

Para obtener información sobre cómo aumentar el límite de vCPU por región, consulte [Cuota estándar: Aumento del límite de vCPU regional](https://docs.microsoft.com/azure/azure-supportability/regional-quota-requests).

## <a name="request-a-quota-limit-increase-from-the-help--support-pane"></a>Solicitud de un aumento del límite de cuota desde el panel "Ayuda y soporte técnico" 

Para solicitar un aumento del límite de cuota al contado para todas las series de máquinas virtuales desde el panel **Ayuda y soporte técnico**, haga lo siguiente:

> [!NOTE]
> También puede solicitar un aumento del límite de cuota para varias regiones a través de un único caso de soporte técnico. Para más información, consulte el paso 8. 

1. En el panel izquierdo de [Azure Portal](https://portal.azure.com), seleccione **Ayuda y soporte técnico**.

   ![El vínculo de "Ayuda y soporte técnico"](./media/resource-manager-core-quotas-request/helpsupport.png)
 
1. En el panel **Ayuda y soporte técnico**, seleccione **Nueva solicitud de soporte técnico**. 

    ![Nueva solicitud de soporte](./media/resource-manager-core-quotas-request/newsupportrequest.png)

1. En la lista desplegable **Tipo de problema**, seleccione **Límites de servicio y suscripción (cuotas)** .

   ![La lista desplegable "Tipo de problema"](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

1. En la lista desplegable **Suscripción**, seleccione la suscripción cuya cuota quiera aumentar.

   ![La lista desplegable "Suscripción"](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
1. En la lista desplegable **Tipo de cuota**, seleccione **Compute-VM (cores-vCPUs) subscription limit increases** (Aumento del límite de suscripción para VM de proceso [núcleos-vCPU]). 

   ![La lista desplegable "Tipo de cuota"](./media/resource-manager-core-quotas-request/select-quota-type.png)

1. Seleccione la pestaña **Detalles** y, a continuación, en **Detalles del problema**, seleccione **Proporcionar detalles** y escriba información adicional para ayudar a procesar la solicitud.

   ![El vínculo "Proporcionar detalles"](./media/resource-manager-core-quotas-request/provide-details.png)
   
1. En el panel **Detalles de la cuota** en la parte superior derecha, haga lo siguiente:

   ![El panel "Detalles de la cuota"](./media/resource-manager-core-quotas-request/3-7.png)

   a. En la lista desplegable **Modelo de implementación**, seleccione el modelo adecuado.

   b. En la lista desplegable **Ubicaciones**, seleccione una ubicación. En la ubicación seleccionada, debajo de **Tipos**, en el cuadro **Seleccione un tipo**, escriba **Al contado**. 
   
   ![Haga clic en la pestaña "Nueva solicitud de soporte técnico"](./media/resource-manager-core-quotas-request/3-8.png)

    En **Tipos**, puede solicitar los dos tipos de cuota (Estándar y Al contado) desde una única solicitud de soporte técnico mediante la selección múltiple. 
    
    Para más información, consulte [Cuota estándar: Aumento del límite de vCPU por serie de VM](https://docs.microsoft.com/azure/azure-supportability/per-vm-quota-requests).

   c. Escriba el nuevo límite de cuota que quiere para esta suscripción. 
 
   ![El cuadro de texto "Nuevo límite de vCPU"](./media/resource-manager-core-quotas-request/3-9.png)

1. Para solicitar un aumento de cuota en más de una ubicación, seleccione una ubicación adicional en la lista desplegable y, a continuación, seleccione el tipo de máquina virtual adecuado. Después, puede especificar el límite que se aplicará a la ubicación adicional.

   ![Ubicaciones adicionales en el panel "Detalles de la cuota"](./media/resource-manager-core-quotas-request/3-10.png)

1. Seleccione **Guardar y continuar** para continuar con la creación de la solicitud de soporte técnico.

## <a name="request-a-quota-limit-increase-from-the-subscriptions-pane"></a>Solicitud de un aumento del límite de cuota desde el panel "Suscripciones"

Para solicitar un aumento del límite de cuota al contado para todas las series de máquinas virtuales desde el panel **Suscripciones**, haga lo siguiente:

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

1. En la lista desplegable **Tipo de cuota**, seleccione **Compute-VM (cores-vCPUs) subscription limit increases** (Aumento del límite de suscripción para VM de proceso [núcleos-vCPU]).

   ![La lista desplegable "Tipo de cuota"](./media/resource-manager-core-quotas-request/select-quota-type.png)

1. En el panel **Detalles de la cuota** en la parte superior derecha, haga lo siguiente:

   ![El panel "Detalles de la cuota"](./media/resource-manager-core-quotas-request/3-2-6.png)
 
   a. En la lista desplegable **Modelo de implementación**, seleccione el modelo adecuado.

   b. En la lista desplegable **Ubicaciones**, seleccione una ubicación. 
   
   c. En la ubicación seleccionada, debajo de **Tipos**, en el cuadro **Seleccione un tipo**, escriba **Al contado**.

   ![El panel "Detalles de la cuota"](./media/resource-manager-core-quotas-request/3-2-7.png)

   Para más información, consulte [Cuota estándar: Aumento del límite de vCPU por serie de VM](https://docs.microsoft.com/azure/azure-supportability/per-vm-quota-requests).

   d. Escriba el nuevo límite de cuota que quiere para esta suscripción.

   ![El cuadro de texto "Nuevo límite de vCPU"](./media/resource-manager-core-quotas-request/3-2-8.png)
 
1. Para solicitar un aumento de cuota en más de una ubicación, seleccione una ubicación adicional en la lista desplegable y, a continuación, seleccione el tipo de máquina virtual adecuado. Después, puede especificar el límite que se aplicará a la ubicación adicional.

   ![Ubicaciones adicionales en el panel "Detalles de la cuota"](./media/resource-manager-core-quotas-request/3-2-9.png)
 
1. Seleccione **Guardar y continuar** para continuar con la creación de la solicitud de soporte técnico.


