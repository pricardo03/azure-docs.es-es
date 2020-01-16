---
title: Aumento del límite de redes | Microsoft Docs
description: Aumento del límite de redes
author: anavinahar
ms.author: anavin
ms.date: 06/19/2019
ms.topic: article
ms.service: azure-supportability
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 04a22d987e27f054648637890fbd61c56533c2ee
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2020
ms.locfileid: "75898712"
---
# <a name="networking-limit-increase"></a>Aumento del límite de redes

Para ver el uso de las redes y la cuota actuales, puede visitar la hoja **Usages + Quota** (Usos y cuota) en Azure Portal. También puede utilizar la [CLI](https://docs.microsoft.com//cli/azure/network?view=azure-cli-latest#az-network-list-usages), [PowerShell](https://docs.microsoft.com/powershell/module/azurerm.network/get-azurermnetworkusage?view=azurermps-6.13.0) o la [API de uso de red](https://docs.microsoft.com/rest/api/virtualnetwork/virtualnetworks/listusage) para ver los límites y el uso de la red.

Puede solicitar un aumento a través de la hoja **Ayuda y soporte técnico** o la hoja **Usages + Quota** (Usos y cuota) en el portal.

> [!Note]
> Para cambiar el tamaño predeterminado de Prefijo de IP pública, seleccione "Longitud mínima de prefijo de red IP pública" en la lista desplegable.

## <a name="request-networking-quota-increase-at-subscription-level-using-the-help--support-blade"></a>Solicitud de un aumento de cuota de red en el nivel de suscripción mediante la hoja **Ayuda y soporte técnico**

Siga las instrucciones siguientes para crear una solicitud de soporte técnico mediante la hoja Ayuda y soporte técnico de Azure disponible en Azure Portal. 

1. En https://portal.azure.com, seleccione **Ayuda y soporte técnico**.

    ![Ayuda y soporte técnico](./media/resource-manager-core-quotas-request/helpsupport.png)
 
2.  Seleccione **Nueva solicitud de soporte técnico**. 

    ![Nueva solicitud de soporte](./media/resource-manager-core-quotas-request/newsupportrequest.png)

3. En el menú desplegable Tipo de incidencia, seleccione **Límites de servicio y suscripción (cuotas)** .

    ![Menú desplegable Tipo de incidencia](./media/resource-manager-core-quotas-request/issuetypedropdown.png)

4. Seleccione la suscripción que necesita una cuota mayor.

    ![Selección de la suscripción en la nueva solicitud de soporte técnico](./media/resource-manager-core-quotas-request/select-subscription-sr.png)
   
5. Seleccione **Redes** en la lista desplegable **Tipo de cuota**. 

    ![Selección del tipo de cuota](./media/networking-quota-request/select-quota-type-network.png)

6. En **Detalles del problema**, haga clic en **Proporcionar detalles** para aportar información adicional que ayude a procesar la solicitud.

    ![Proporcionar detalles](./media/resource-manager-core-quotas-request/provide-details.png)

7. En el panel **Detalles de la cuota**, seleccione Modelo de implementación, una ubicación y los recursos para los que desea solicitar un aumento.

    ![Modelo de implementación en Detalles de la cuota](./media/networking-quota-request/quota-details-network.png)

8.  Escriba los nuevos límites que quiere en la suscripción. Para quitar una línea, desactive el recurso de la lista desplegable Recurso o haga clic en el icono de descarte "x". Después de escribir la cuota deseada para cada recurso, haga clic en **Guardar y continuar** en el panel Detalles de la cuota para continuar con la creación de la solicitud de soporte técnico.

    ![Nuevos límites](./media/networking-quota-request/network-new-limits.png)


## <a name="request-networking-quota-increase-at-subscription-level-using-usages--quota-blade"></a>Solicitud de un aumento de cuota de red en el nivel de suscripción mediante la hoja **Usages + Quota** (Usos y cuota)

Siga las instrucciones siguientes para crear una solicitud de soporte técnico mediante la hoja "Usos y cuota" de Azure disponible en Azure Portal. 

1. En https://portal.azure.com, seleccione **Suscripciones**.

    ![Suscripciones](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Seleccione la suscripción que necesita una cuota mayor.

    ![Seleccionar suscripción](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Seleccione **Uso y cuotas**.

    ![Selección de uso y cuotas](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. En la esquina superior derecha, seleccione **Solicitar aumento**.

    ![Solicitud de aumento](./media/resource-manager-core-quotas-request/request-increase.png)

5. Siga los pasos comenzando con el paso 3 de la sección *Solicitud de un aumento de cuota de red en el nivel de suscripción* mediante la sección de la hoja **Ayuda y soporte técnico**

## <a name="about-networking-limits"></a>Acerca de los límites de red

Para más información sobre los límites de red, consulte la [sección de redes](../../azure-resource-manager/management/azure-subscription-service-limits.md#networking-limits) de la página de límites o nuestras preguntas más frecuentes sobre los límites de red
