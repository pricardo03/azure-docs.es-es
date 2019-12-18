---
title: Cuota de Spot | Microsoft Docs
description: Solicitudes de cuota de Spot
author: sowmyavenkat86
ms.author: svenkat
ms.date: 11/19/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 09c9d7940314b691e6351353e6a0076510fdcb13
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74850574"
---
# <a name="spot-quota-limit-increase-for-all-vm-series"></a>Cuota de Spot: aumento del límite para todas las series de máquinas virtuales

Las máquinas virtuales de Spot proporcionan un modelo diferente de uso de Azure, con un costo más bajo al permitir que Azure elimine una máquina virtual según sea necesario para las implementaciones de instancia reservada de máquina virtual o de pago por uso. Más información sobre las máquinas virtuales de Spot [aquí](https://docs.microsoft.com/azure/virtual-machine-scale-sets/use-spot).

Resource Manager admite dos tipos de cuotas de vCPU para máquinas virtuales. Las **máquinas virtuales de pago por uso y las instancias reservadas de máquina virtual** usan la cuota estándar. **Las máquinas virtuales de Spot** usan la cuota de Spot. 

Para el tipo **Cuota de Spot**, las cuotas de vCPU de Resource Manager se aplican en todas las series de máquinas virtuales disponibles como un límite regional único.

Cada vez que se implemente una nueva máquina virtual de Spot, la suma del uso de vCPU nuevo y existente para todas las instancias de máquina virtual de Spot no debe superar el límite de cuota de vCPU de Spot aprobado. Si se supera la cuota de Spot, no se permitirá la implementación de máquinas virtuales de Spot. Puede solicitar un aumento del límite de cuota de vCPU de Spot desde Azure Portal. 

Para más información acerca de las cuotas de vCPU estándar, consulte la página sobre las cuotas de vCPU de máquinas virtuales y los límites de suscripción y servicios de Azure. Obtenga más información sobre cómo aumentar el límite de vCPU regional para la cuota estándar en esta [página](https://docs.microsoft.com/azure/azure-supportability/regional-quota-requests).

Ahora puede solicitar un aumento en los **límites de cuota de Spot para todas las series de máquinas virtuales** mediante las hojas **Ayuda y soporte técnico** y **Usos y cuota** en el portal.

## <a name="request-spot-quota-limit-increase-for-all-vm-series-per-subscription-using-the-help--support-blade"></a>Solicitud de un aumento del límite de cuota de Spot para todas las series de máquinas virtuales por suscripción mediante la hoja Ayuda y soporte técnico

Siga las instrucciones siguientes para crear una solicitud de soporte técnico mediante la hoja Ayuda y soporte técnico de Azure disponible en Azure Portal.

También puede **solicitar una cuota para varias regiones** a través de un único caso de soporte técnico. Consulte el paso 10 a continuación para obtener más información. 


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
   
7.  En el panel **Detalles de la cuota***, seleccione **Modelo de implementación** y elija una **ubicación**.

![Proporcionar detalles](./media/resource-manager-core-quotas-request/3-7.png)

8. Para la ubicación seleccionada, seleccione el valor de **Type** como **"Spot"** . Puede solicitar tipos de cuota Estándar y Spot desde una única solicitud de soporte técnico mediante la multi-selección en el campo **Type**. Obtenga más información sobre el **aumento de la cuota estándar por serie de máquinas virtuales** en esta [página](https://docs.microsoft.com/azure/azure-supportability/per-vm-quota-requests).

![Proporcionar detalles](./media/resource-manager-core-quotas-request/3-8.png)

9.  Escriba el nuevo límite que quiere en la suscripción. 
 
 ![Proporcionar detalles](./media/resource-manager-core-quotas-request/3-9.png)

10. Para solicitar la cuota para más de una ubicación, puede activar otra ubicación en la lista desplegable y seleccionar el tipo de máquina virtual adecuado. Luego, puede escribir los nuevos límites que quiera.

![Proporcionar detalles](./media/resource-manager-core-quotas-request/3-10.png)

11. Después de escribir la cuota deseada, haga clic en **Guardar y continuar** en el panel Detalles de la cuota para continuar con la creación de la solicitud de soporte técnico.

## <a name="request-spot-quota-limit-increase-for-all-vm-series-per-subscription-using-usages--quota-blade"></a>Solicitud de un aumento del límite de cuota de Spot para todas las series de máquinas virtuales por suscripción mediante la hoja Usos y cuota

Siga las instrucciones siguientes para crear una solicitud de soporte técnico mediante la hoja "Usos y cuota" de Azure disponible en Azure Portal.

También puede **solicitar una cuota para varias regiones** a través de un único caso de soporte técnico. Consulte el paso 9 a continuación para obtener más información. 

1.  En https://portal.azure.com, seleccione **Suscripciones**.

 ![Suscripciones](./media/resource-manager-core-quotas-request/subscriptions.png)

2.  Seleccione la suscripción que necesita una cuota mayor.

 ![Selección de la suscripción](./media/resource-manager-core-quotas-request/select-subscription.png)

3.  Seleccione **Uso y cuotas**.

 ![Selección de uso y cuotas](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4.  En la esquina superior derecha, seleccione **Solicitar aumento**.

   ![Solicitud de aumento](./media/resource-manager-core-quotas-request/request-increase.png)

5.  Seleccione **Compute-VM (cores-vCPUs) subscription limit increases** (El límite de la suscripción a proceso/máquina virtual (núcleos/vCPU) aumenta) como tipo de cuota.

  ![Rellenado del formulario](./media/resource-manager-core-quotas-request/select-quota-type.png)

6.  En el panel **Detalles de la cuota**, seleccione el modelo de implementación y una ubicación.

  ![Rellenado del formulario](./media/resource-manager-core-quotas-request/3-2-6.png)
 
7.  Para la ubicación seleccionada, seleccione el valor de **Type** como **"Spot"** . Puede solicitar tipos de cuota Estándar y Spot desde una única solicitud de soporte técnico mediante la multi-selección en el campo **Type**. Obtenga más información sobre el **aumento de la cuota estándar por serie de máquinas virtuales** en esta [página](https://docs.microsoft.com/azure/azure-supportability/per-vm-quota-requests).

  ![Rellenado del formulario](./media/resource-manager-core-quotas-request/3-2-7.png)
 
8.  Escriba el nuevo límite que quiere en la suscripción.

  ![Rellenado del formulario](./media/resource-manager-core-quotas-request/3-2-8.png)
 
9.  Para solicitar la cuota para más de una ubicación, puede activar otra **ubicación** en la lista desplegable y seleccionar el tipo de máquina virtual adecuado. Luego, puede escribir los nuevos límites que quiera.

  ![Rellenado del formulario](./media/resource-manager-core-quotas-request/3-2-9.png)
 
10. Después de escribir la cuota deseada, haga clic en **Guardar y continuar** en el panel Detalles de la cuota para continuar con la creación de la solicitud de soporte técnico.


