---
title: Solicitudes de aumento de cuota de vCPU de Azure Resource Manager | Microsoft Docs
description: Solicitudes de aumento de cuota de vCPU de Azure Resource Manager
author: ganganarayanan
ms.author: gangan
ms.date: 6/13/2018
ms.topic: article
ms.service: azure
ms.assetid: ce37c848-ddd9-46ab-978e-6a1445728a3b
ms.openlocfilehash: 204deaf3a67984c0dd5eca5352686719e7767885
ms.sourcegitcommit: d89b679d20ad45d224fd7d010496c52345f10c96
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57792300"
---
# <a name="resource-manager-vcpu-quota-increase-requests"></a>Solicitudes de aumento de cuota de vCPU de Resource Manager

Las cuotas de vCPU de Resource Manager se aplican en el nivel de región y de familia de SKU.
Aprenda más sobre cómo se imponen cuotas en la página [Límites de servicios y suscripciones de Azure](https://aka.ms/quotalimits).
Para más información sobre las familias de SKU, puede comparar coste y el rendimiento de la [precios de máquinas virtuales](https://aka.ms/pricingcompute) página.

Para solicitar un aumento, siga las instrucciones siguientes para crear una solicitud de soporte técnico mediante la hoja de uso y cuota de Azure disponible en Azure Portal. 

## <a name="request-quota-increase-at-subscription-level"></a>Solicitud de un aumento de cuota en el nivel de suscripción

1. En https://portal.azure.com, seleccione **Suscripciones**.

   ![Suscripciones](./media/resource-manager-core-quotas-request/subscriptions.png)

2. Seleccione la suscripción que necesita una cuota mayor.

   ![Selección de la suscripción](./media/resource-manager-core-quotas-request/select-subscription.png)

3. Seleccione **Uso y cuotas**.

   ![Selección de uso y cuotas](./media/resource-manager-core-quotas-request/select-usage-quotas.png)

4. En la esquina superior derecha, seleccione **Solicitar aumento**.

   ![Solicitud de aumento](./media/resource-manager-core-quotas-request/request-increase.png)

5. Paso 1: Seleccione **Núcleos** como tipo de cuota. 

   ![Rellenado del formulario](./media/resource-manager-core-quotas-request/forms.png)
   
6. Paso 2: Seleccione el modelo de implementación "Resource Manager" y seleccione una ubicación.

    ![Hoja de problema de Cuota](./media/resource-manager-core-quotas-request/Problem-step.png)

3. Seleccione las familias de SKU que requieren un aumento.

    ![Series de SKU seleccionadas](./media/resource-manager-core-quotas-request/SKU-selected.png)

4. Escriba los nuevos límites que quiere en la suscripción.

    ![Nueva solicitud de cuota de SKU](./media/resource-manager-core-quotas-request/SKU-new-quota.png)

- Para quitar una línea, desactive la SKU de la lista desplegable de familias de SKU o haga clic en el icono de descartar "x".
Después de escribir la cuota deseada para cada familia de SKU, haga clic en "Siguiente" en la página de pasos del problema para continuar con la creación de la solicitud de soporte técnico.

