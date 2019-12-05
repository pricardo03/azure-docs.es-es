---
title: Cuota de prioridad baja | Microsoft Docs
description: Solicitudes de cuota de prioridad baja
author: sowmyavenkat86
ms.author: svenkat
ms.date: 11/19/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 49ac478330cf73dff050a3edcc15933692fa6448
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/26/2019
ms.locfileid: "74534728"
---
# <a name="low-priority-quota-limit-increase-for-all-vm-series"></a>Cuota de prioridad baja: aumento del límite para toda la serie de máquinas virtuales

Las máquinas virtuales de prioridad baja proporcionan un modelo diferente de uso de Azure, con un costo más bajo para permitir que Azure quite una máquina virtual según sea necesario para las implementaciones de instancia reservada de máquina virtual o de pago por uso. Obtenga más información acerca de las VM de prioridad baja [aquí](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-use-low-priority).

Resource Manager admite dos tipos de cuotas de vCPU para las máquinas virtuales. Las **máquinas virtuales de pago por uso y las instancias reservadas de máquina virtual** usan la cuota estándar. Las **máquinas virtuales de prioridad baja** usan la cuota de prioridad baja. 

Para el tipo de **cuota de prioridad baja**, las cuotas de vCPU de Resource Manager se aplican en todas las series de máquinas virtuales disponibles como un límite regional único.

Cada vez que se implemente una nueva máquina virtual de prioridad baja, la suma del uso de vCPU nuevo y existente para todas las instancias de máquina virtual de prioridad baja no debe superar el límite de cuota de vCPU de prioridad baja aprobado. Si se supera la cuota de prioridad baja, no se permitirá la implementación de máquinas virtuales de prioridad baja. Puede solicitar un aumento del límite de la cuota de vCPU de prioridad baja desde Azure Portal. 

Para más información acerca de las cuotas de vCPU estándar, consulte la página sobre las cuotas de vCPU de máquinas virtuales y los límites de suscripción y servicios de Azure. Obtenga más información sobre cómo aumentar el límite de vCPU regional para la cuota estándar en esta [página](https://docs.microsoft.com/azure/azure-supportability/regional-quota-requests).

Ahora puede solicitar un aumento en los **límites de cuota de prioridad baja para todas las series de máquinas virtuales** mediante las hojas **Ayuda y soporte técnico** y **Usos y cuota** en el portal.

## <a name="request-low-priority-quota-limit-increase-for-all-vm-series-per-subscription-using-the-help--support-blade"></a>Solicitud de aumento del límite de cuota de prioridad baja para todas las series de máquinas virtuales por suscripción mediante la hoja Ayuda y soporte técnico

Siga las instrucciones siguientes para crear una solicitud de soporte técnico mediante la hoja Ayuda y soporte técnico de Azure disponible en Azure Portal.

También puede **solicitar una cuota para varias regiones** a través de un único caso de soporte técnico. Consulte el paso 10 a continuación para obtener más información. 


1. En https://portal.azure.com, seleccione **Ayuda y soporte técnico**.

   ![Ayuda y soporte técnico](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  Seleccione **Nueva solicitud de soporte técnico**. 

     ![Nueva solicitud de soporte](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. En el menú desplegable Tipo de incidencia, seleccione **Límites de servicio y suscripción (cuotas)** .

   ![Menú desplegable Tipo de incidencia](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Seleccione la suscripción que necesita una cuota mayor.

   ![Selección de la suscripción en la nueva solicitud de soporte técnico](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. Seleccione **El límite de la suscripción a Compute/VM (núcleos/vCPU) aumenta** en el menú desplegable **Tipo de cuota**. 

   ![Selección del tipo de cuota](./media/resource-manager-core-quotas-request/select-quota-type.png)

6. En **Detalles del problema**, haga clic en **Proporcionar detalles** para aportar información adicional que ayude a procesar la solicitud.

   ![Proporcionar detalles](./media/resource-manager-core-quotas-request/provide-details.png)
   
7.  En el panel **Detalles de la cuota***, seleccione **Modelo de implementación** y elija una **ubicación**.

![Proporcionar detalles](./media/resource-manager-core-quotas-request/3-7.png)

8. En la ubicación seleccionada, seleccione el valor de **Tipo** como **Prioridad baja**. Puede solicitar los tipos de cuota de prioridad estándar y baja desde un único caso de soporte técnico a través de la selección múltiple en el campo **Tipo**. Obtenga más información sobre el **aumento de la cuota estándar por serie de máquinas virtuales** en esta [página](https://docs.microsoft.com/azure/azure-supportability/per-vm-quota-requests).

![Proporcionar detalles](./media/resource-manager-core-quotas-request/3-8.png)

9.  Escriba el nuevo límite que quiere en la suscripción. 
 
 ![Proporcionar detalles](./media/resource-manager-core-quotas-request/3-9.png)

10. Para solicitar la cuota para más de una ubicación, puede activar otra ubicación en la lista desplegable y seleccionar el tipo de máquina virtual adecuado. Luego, puede escribir los nuevos límites que quiera.

![Proporcionar detalles](./media/resource-manager-core-quotas-request/3-10.png)

11. Después de escribir la cuota deseada, haga clic en **Guardar y continuar** en el panel Detalles de la cuota para continuar con la creación de la solicitud de soporte técnico.

## <a name="request-low-priority-quota-limit-increase-for-all-vm-series-per-subscription-using-usages--quota-blade"></a>Solicitud de aumento del límite de cuota de prioridad baja para todas las series de máquinas virtuales por suscripción mediante la hoja Usos y cuota

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
 
7.  En la ubicación seleccionada, seleccione el valor de **Tipo** como **Prioridad baja**. Puede solicitar los tipos de cuota de prioridad estándar y baja desde un único caso de soporte técnico a través de la selección múltiple en el campo **Tipo**. Obtenga más información sobre el **aumento de la cuota estándar por serie de máquinas virtuales** en esta [página](https://docs.microsoft.com/azure/azure-supportability/per-vm-quota-requests).

  ![Rellenado del formulario](./media/resource-manager-core-quotas-request/3-2-7.png)
 
8.  Escriba el nuevo límite que quiere en la suscripción.

  ![Rellenado del formulario](./media/resource-manager-core-quotas-request/3-2-8.png)
 
9.  Para solicitar la cuota para más de una ubicación, puede activar otra **ubicación** en la lista desplegable y seleccionar el tipo de máquina virtual adecuado. Luego, puede escribir los nuevos límites que quiera.

  ![Rellenado del formulario](./media/resource-manager-core-quotas-request/3-2-9.png)
 
10. Después de escribir la cuota deseada, haga clic en **Guardar y continuar** en el panel Detalles de la cuota para continuar con la creación de la solicitud de soporte técnico.

