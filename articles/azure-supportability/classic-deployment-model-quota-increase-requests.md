---
title: Modelo de implementación clásica de Azure | Microsoft Docs
description: modelo de implementación clásica de Azure
author: sowmyavenkat86
ms.author: svenkat
ms.date: 06/20/2019
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 70ad44eade871d52591014ee24e645b95c52f1e5
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234866"
---
# <a name="classic-deployment-model"></a>Modelo de implementación clásica

El modelo de implementación clásica es el modo de implementación de Azure de la generación anterior que impone un límite de cuota de vCPU global para máquinas virtuales y conjuntos de escalado de máquinas virtuales. El modelo de implementación clásica ya no se recomienda y ahora se reemplaza por el modelo de Resource Manager. Para más información sobre estos dos modelos de implementación y las ventajas de Resource Manager, consulte la página del modelo de implementación de Resource Manager. Cuando se crea una nueva suscripción, se asigna una cuota predeterminada de vCPU a ella. Cada vez que se implementa una nueva máquina virtual mediante el modelo de implementación clásica, la suma del uso de vCPU nuevas y existentes en todas las regiones no debe superar la cuota de vCPU aprobada para dicho modelo. Más información sobre las cuotas en la [página de límites de servicio y suscripción de Azure](https://aka.ms/quotalimits).

Puede solicitar un aumento en el límite de vCPU para el modelo de implementación clásica a través de la hoja Ayuda y soporte técnico o la hoja Usages + Quota (Usos y cuota) en el portal.

## <a name="request-per-vm-series-vcpu-quota-increase-at-subscription-level-using-the-help--support-blade"></a>Solicitud de un aumento de cuota de vCPU por serie de máquinas virtuales en el nivel de suscripción a través de la hoja **Ayuda y soporte técnico**

Siga las instrucciones siguientes para crear una solicitud de soporte técnico mediante la hoja Ayuda y soporte técnico de Azure disponible en Azure Portal. 

1. En https://portal.azure.com, seleccione **Ayuda y soporte técnico**.

   ![Ayuda y soporte técnico](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  Seleccione **Nueva solicitud de soporte técnico**. 

      ![Nueva solicitud de soporte](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. En el menú desplegable Tipo de incidencia, seleccione **Límites de servicio y suscripción (cuotas)** .

   ![Menú desplegable Tipo de incidencia](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Seleccione la suscripción que necesita una cuota mayor.

   ![Selección de la suscripción en la nueva solicitud de soporte técnico](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. Seleccione **Compute-VM (cores-vCPUs) subscription limit increases** (El límite de la suscripción a proceso/máquina virtual (núcleos/vCPU) aumenta) en el menú desplegable **Tipo de cuota**. 

   ![Selección del tipo de cuota](./media/resource-manager-core-quotas-request/select-quota-type.png)

6. En **Detalles del problema**, haga clic en **Proporcionar detalles** para aportar información adicional que ayude a procesar la solicitud.

   ![Proporcionar detalles](./media/resource-manager-core-quotas-request/provide-details.png)

7. En el panel **Detalles de la cuota**, seleccione Clásico y elija una ubicación.

   ![Modelo de implementación en Detalles de la cuota](./media/resource-manager-core-quotas-request/quota-details-classic.png)

8. Seleccione las **familias de SKU** que requieren un aumento. 

   ![Familia de SKU](./media/resource-manager-core-quotas-request/sku-family-classic.png)

9. Escriba los nuevos límites que quiere en la suscripción. Para quitar una línea, desactive la SKU de la lista desplegable de familias de SKU o haga clic en el icono de descartar "x". Después de escribir la cuota deseada para cada familia de SKU, haga clic en **Guardar y continuar** en el panel Detalles de la cuota para continuar con la creación de la solicitud de soporte técnico.

   ![Nuevos límites](./media/resource-manager-core-quotas-request/new-limits-classic.png)

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

   ![Rellenado del formulario](./media/resource-manager-core-quotas-request/select-quota-type.png)
   
6. En **Detalles del problema**, haga clic en **Proporcionar detalles** para aportar información adicional que ayude a procesar la solicitud.

   ![Proporcionar detalles](./media/resource-manager-core-quotas-request/provide-details.png)

7. En el panel **Detalles de la cuota**, seleccione Clásico y elija una ubicación.

   ![Modelo de implementación en Detalles de la cuota](./media/resource-manager-core-quotas-request/quota-details-classic.png)

8. Seleccione las **familias de SKU** que requieren un aumento. 

   ![Familia de SKU](./media/resource-manager-core-quotas-request/sku-family-classic.png)

9. Escriba los nuevos límites que quiere en la suscripción. Para quitar una línea, desactive la SKU de la lista desplegable de familias de SKU o haga clic en el icono de descartar "x". Después de escribir la cuota deseada para cada familia de SKU, haga clic en **Guardar y continuar** en el panel Detalles de la cuota para continuar con la creación de la solicitud de soporte técnico.

   ![Nuevos límites](./media/resource-manager-core-quotas-request/new-limits-classic.png)

