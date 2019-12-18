---
title: Solicitudes de aumento de cuota de vCPU por máquina virtual en Azure | Microsoft Docs
description: Solicitudes de aumento de cuota de vCPU por máquina virtual
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/07/2019
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 280561126186e4c70399b3a1ddd177ff4eb54400
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74850047"
---
# <a name="standard-quota-per-vm-series-vcpu-limit-increase"></a>Cuota estándar: aumento del límite de vCPU por serie de VM

Resource Manager admite dos tipos de cuotas de vCPU para las máquinas virtuales. Las **máquinas virtuales de pago por uso y las instancias reservadas de máquina virtual** usan la cuota estándar. **Las máquinas virtuales de Spot** usan la cuota de Spot. La cuota de vCPU estándar para las instancias reservadas de máquina virtual y de pago por uso se aplica en dos niveles para cada suscripción de cada región.

El primer nivel es el **límite de vCPU regionales totales** (en todas las series de máquinas virtuales) y el segundo es el **límite de vCPU por serie de máquinas virtuales** (como las vCPU de la serie Dv3). Cada vez que se va a implementar una nueva máquina virtual, la suma del uso de las vCPU nuevas y existentes de esa serie de máquinas virtuales no debe superar la cuota de vCPU aprobada para la serie. Además, el número total de las vCPU nuevas y existentes implementadas en todas las series de máquinas virtuales no debería superar la cuota de las vCPU regionales totales aprobada para la suscripción. Si se supera cualquiera de esas dos cuotas, no se permitirá la implementación de la máquina virtual.
Puede solicitar un aumento del límite de la cuota de vCPU para la serie de máquinas virtuales en Azure Portal. Al aumentar la cuota de la serie de máquinas virtuales, se aumenta automáticamente el límite de vCPU regionales totales en la misma cantidad. 

Para obtener más información acerca de las cuotas de vCPU estándar, consulte la [página sobre las cuotas de vCPU de máquinas virtuales](https://docs.microsoft.com/azure/virtual-machines/windows/quotas) y los [límites de suscripción y servicios de Azure](https://docs.microsoft.com/azure/azure-supportability/classic-deployment-model-quota-increase-requests). 

Obtenga más información sobre cómo aumentar el límite de vCPU regional para la cuota estándar [aquí](https://docs.microsoft.com/azure/azure-supportability/regional-quota-requests). 

Más información sobre cómo **aumentar los límites de vCPU de máquina virtual de Spot** [aquí](https://docs.microsoft.com/azure/azure-supportability/low-priority-quota).

Puede solicitar un aumento en los **límites de cuota de vCPU por serie de VM** través de la hoja **Ayuda y soporte técnico** o la hoja **Usos y cuota** en el portal.

## <a name="request-standard-vcpu-quota-increase-per-vm-series-at-subscription-level-using-the-help--support-blade"></a>Solicitud de un aumento de cuota de vCPU por serie de máquinas virtuales en el nivel de suscripción a través de la hoja Ayuda y soporte técnico

Siga las instrucciones siguientes para crear una solicitud de soporte técnico mediante la hoja Ayuda y soporte técnico de Azure disponible en Azure Portal. 

También puede solicitar una cuota para varias regiones a través de un único caso de soporte técnico. Consulte el paso 11 a continuación para obtener más información.

1. En https://portal.azure.com, seleccione **Ayuda y soporte técnico**.

   ![Ayuda y soporte técnico](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  Seleccione **Nueva solicitud de soporte técnico**. 

     ![Nueva solicitud de soporte](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. En el menú desplegable Tipo de problema, seleccione **Límites de servicio y suscripción (cuotas)** .

   ![Menú desplegable Tipo de problema](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Seleccione la suscripción que necesita una cuota mayor.

   ![Selección de la suscripción en la nueva solicitud de soporte técnico](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. Seleccione **El límite de la suscripción a Proceso/VM (núcleos/vCPU) aumenta** en el menú desplegable **Tipo de cuota**. 

   ![Selección del tipo de cuota](./media/resource-manager-core-quotas-request/select-quota-type.png)

6. En **Detalles del problema**, haga clic en **Proporcionar detalles** para aportar información adicional que ayude a procesar la solicitud.

   ![Proporcionar detalles](./media/resource-manager-core-quotas-request/provide-details.png)

7. En el panel **Detalles de la cuota**, seleccione el **Modelo de implementación** y una **ubicación**.

   ![Modelo de implementación en Detalles de la cuota](./media/resource-manager-core-quotas-request/1-7.png)

8. En la ubicación seleccionada, seleccione el valor **Tipo** como **"Estándar"** . Puede solicitar tipos de cuota Estándar y Spot desde una única solicitud de soporte técnico mediante la multi-selección en el campo **Type**. Obtenga información acerca de cómo **Aumentar los límites de cuota de Spot** en esta [página](https://docs.microsoft.com/azure/virtual-machine-scale-sets/use-spot).

   ![Familia de SKU](./media/resource-manager-core-quotas-request/1-8.png)

9. Seleccione las **familias de SKU** que requieren un aumento.

   ![Familia de SKU](./media/resource-manager-core-quotas-request/1-9.png)

10. Escriba los nuevos límites que quiere en la suscripción. Para quitar una línea, desactive la SKU de la lista desplegable de familias de SKU o haga clic en el icono de descartar "x". 

   ![Nuevos límites](./media/resource-manager-core-quotas-request/1-10.png)

11. Para solicitar la cuota para más de una ubicación, se puede activar otra **ubicación** en la lista desplegable y seleccionar el tipo de máquina virtual adecuado. En este paso se cargan previamente las familias de SKU seleccionadas para la **ubicación** anterior en la ubicación nueva y luego puede simplemente especificar los nuevos límites que le gustaría aplicar.

   ![Varias ubicaciones](./media/resource-manager-core-quotas-request/1-11.png)
   
12. Después de escribir la cuota deseada para cada familia de SKU, haga clic en **Guardar y continuar** en el panel Detalles de la cuota para continuar con la creación de la solicitud de soporte técnico.

## <a name="request-standard-vcpu-quota-increase-per-vm-series-at-subscription-level-using-usages--quota-blade"></a>Solicitud de un aumento de cuota de vCPU por serie de máquinas virtuales en el nivel de suscripción a través de la hoja Usos y cuota

Siga las instrucciones siguientes para crear una solicitud de soporte técnico mediante la hoja "Usos y cuota" de Azure disponible en Azure Portal.

También puede **solicitar una cuota para varias regiones** a través de un único caso de soporte técnico. Consulte el paso 10 a continuación para obtener más información.

1. En https://portal.azure.com, seleccione **Suscripciones**.

   ![Suscripciones](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Seleccione la suscripción que necesita una cuota mayor.

   ![Selección de la suscripción](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Seleccione **Uso y cuotas**.

   ![Selección de uso y cuotas](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. En la esquina superior derecha, seleccione **Solicitar aumento**.

   ![Solicitud de aumento](./media/resource-manager-core-quotas-request/request-increase.png)

5. Seleccione **Compute-VM (cores-vCPUs) subscription limit increases** (El límite de la suscripción a proceso/máquina virtual (núcleos/vCPU) aumenta) como tipo de cuota. 

   ![Rellenado del formulario](./media/resource-manager-core-quotas-request/select-quota-type.png)
   
6. En el panel **Detalles de la cuota**, seleccione el modelo de implementación y una ubicación.

   ![Hoja de problema de Cuota](./media/resource-manager-core-quotas-request/1-1-6.png)

7. En la ubicación seleccionada, seleccione el valor **Tipo** como **"Estándar"** . Puede solicitar los tipos de cuota de prioridad estándar y baja desde un único caso de soporte técnico a través de la selección múltiple en el campo **Tipo**. Obtenga información acerca de cómo **Aumentar los límites de vCPU de Spot** en esta [página](https://docs.microsoft.com/azure/virtual-machine-scale-sets/use-spot).

   ![Series de SKU seleccionadas](./media/resource-manager-core-quotas-request/1-1-7.png)
   
   
8. Seleccione las **familias de SKU** que requieren un aumento.

   ![Series de SKU seleccionadas](./media/resource-manager-core-quotas-request/1-1-8.png)

9. Escriba los nuevos límites que quiere en la suscripción. Para quitar una línea, desactive la SKU de la lista desplegable de familias de SKU o haga clic en el icono de descartar "x". 

   ![Nueva solicitud de cuota de SKU](./media/resource-manager-core-quotas-request/1-1-9.png)
   

10. Para solicitar la cuota para más de una ubicación, se puede activar otra **ubicación** en la lista desplegable y seleccionar el tipo de máquina virtual adecuado. En este paso se cargan previamente las familias de SKU seleccionadas para la **ubicación** anterior en la ubicación nueva y luego puede simplemente especificar los nuevos límites que le gustaría aplicar.
   
    ![Nueva solicitud de cuota de SKU](./media/resource-manager-core-quotas-request/1-1-10.png)
 
