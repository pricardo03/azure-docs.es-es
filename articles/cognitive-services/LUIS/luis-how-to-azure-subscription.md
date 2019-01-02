---
title: Administración de recursos de Azure
titleSuffix: Language Understanding - Azure Cognitive Services
description: En este artículo, se crea una clave de punto de conexión de uso medido para la cuenta de LUIS para proporcionar tráfico ilimitado al punto de conexión en función de un plan de pago.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 5d3e837cf644e0cb9b35a0cfc715a9dcace592e3
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53082992"
---
# <a name="manage-azure-endpoint-subscription-keys"></a>Administración de las claves de suscripción de punto de conexión de Azure

Para pruebas y prototipos solo, use el plan gratuito (F0). Para los sistemas de producción, use un plan de [pago](https://aka.ms/luis-price-tier). 

> [!NOTE]
> No use la [clave de creación](luis-concept-keys.md#authoring-key) para las consultas de punto de conexión en producción.

<a name="create-luis-service"></a>
## <a name="create-luis-endpoint-key"></a>Creación de la clave de punto de conexión de LUIS

1. Inicie sesión en **[Microsoft Azure](https://ms.portal.azure.com/)**. 
2. Haga clic en el signo **+** de color verde en el panel de la parte superior izquierda y busque "LUIS" en Marketplace y, después, haga clic en **Language Understanding** y siga la **experiencia de creación** para crear una cuenta de suscripción de LUIS. 

    ![Azure Search](./media/luis-azure-subscription/azure-search.png) 

3. Configure la suscripción con la configuración, incluido el nombre de cuenta, los planes de tarifa, etc. 

    ![Elección de la API de Azure](./media/luis-azure-subscription/azure-api-choice.png) 

4. Una vez creado el servicio LUIS, puede ver las claves de acceso que se generan en **Administración de recursos -> Claves**.  

    ![Claves de Azure](./media/luis-azure-subscription/azure-keys.png)

    > [!Note] 
    > Inicie sesión en el sitio Web de [LUIS](luis-reference-regions.md) de la región y [asigne la nueva clave de punto de conexión de LUIS](luis-how-to-manage-keys.md#assign-endpoint-key). Necesita el nombre de la suscripción de LUIS del paso 3.

## <a name="change-luis-pricing-tier"></a>Cambiar el plan de tarifa de LUIS

1.  En [Azure](https://portal.azure.com), busque la suscripción de LUIS. Haga clic en la suscripción de LUIS.
    ![Buscar la suscripción de LUIS](./media/luis-usage-tiers/find.png)
2.  Haga clic en **Plan de tarifa** para ver los planes de tarifa disponibles. 
    ![Ver los planes de tarifa](./media/luis-usage-tiers/subscription.png)
3.  Haga clic en el plan de tarifa y después en **Seleccionar** para guardar el cambio. 
    ![Cambiar el plan de pago de LUIS](./media/luis-usage-tiers/plans.png)
4.  Una vez completado el cambio de tarifa, el plan de tarifa nuevo se comprueba en una ventana emergente. 
    ![Comprobar el plan de pago de LUIS](./media/luis-usage-tiers/updated.png)
5. No olvide [asignar esta clave de punto de conexión](luis-how-to-manage-keys.md#assign-endpoint-key) en la página **Publicar** y usarla en todas las consultas de punto de conexión. 

## <a name="exceed-pricing-tier-usage"></a>Superar el uso del plan de tarifa
Cada plan permite solicitudes de punto de conexión a la cuenta de LUIS a una tasa específica. Si la tasa de solicitudes es mayor que la permitida para la cuenta de uso medido por minuto o por mes, las solicitudes reciben un error HTTP de "429: demasiadas solicitudes".

Cada plan permite solicitudes acumulativas por mes. Si el número total de solicitudes es más alto que la tasa permitida, las solicitudes reciben un error HTTP "403: prohibido".  

## <a name="viewing-summary-usage"></a>Ver uso de resumen
En Azure puede ver la información de uso de LUIS. En la página **Información general** se muestra información de resumen reciente incluidos los errores y las llamadas. Si realiza una solicitud de punto de conexión de LUIS, consulte inmediatamente la **página Información general**, y deje que pasen hasta cinco minutos hasta que se muestre el uso.

![Ver uso de resumen](./media/luis-usage-tiers/overview.png)

## <a name="customizing-usage-charts"></a>Personalización de los gráficos de uso
Las métricas proporcionan una vista más detallada de los datos.

![Métricas predeterminadas](./media/luis-usage-tiers/metrics-default.png)

Puede configurar los gráficos de métricas para el período de tiempo y el tipo de métrica. 

![Métricas personalizadas](./media/luis-usage-tiers/metrics-custom.png)

## <a name="total-transactions-threshold-alert"></a>Alerta de umbral de transacciones totales
Si le gustaría saber cuándo se alcanza un umbral de transacciones determinado (por ejemplo 10.000) puede crear una alerta. 

![Alertas predeterminadas](./media/luis-usage-tiers/alert-default.png)

Agregue una alerta de métrica para la métrica **Llamadas totales** para un período de tiempo concreto. Agregue las direcciones de correo electrónico de todos los usuarios que deben recibir la alerta. Agregue webhooks para todos los sistemas que deben recibir la alerta. También puede ejecutar una aplicación lógica cuando se desencadene la alerta. 

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo usar las [versiones](luis-how-to-manage-versions.md) para administrar los cambios en la aplicación de LUIS.