---
title: Compatibilidad con los contenedores
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre cómo crear un recurso de Text Analytics de Cognitive Services.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 022ffcd806d4d4f89f8a8cf256a541518ea12602
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455086"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Crear un recurso de Text Analytics de Cognitive Services

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Seleccione **+ Create a resource** (+ Crear un recurso), vaya a **AI + Machine Learning > Text Analytics** (IA + Machine Learning > Text Analytics) o haga clic en [Create **Text Analytics**](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics) (Crear > Text Analytics).
1. Establezca todas las opciones de configuración necesarias:

    |Configuración|Valor|
    |--|--|
    |NOMBRE|Nombre que desee (2-64 caracteres)|
    |Subscription|Seleccione una suscripción adecuada|
    |Location|Seleccione una ubicación cercana|
    |Nivel de precios|`S` - Plan de tarifa estándar|
    |Grupo de recursos|Seleccione un grupo de recursos disponible|

1. Haga clic en **Create** (Crear) y espere a que el recurso que se va a crear. Después de crearlo, el explorador se redirige automáticamente a la página de recursos recién creada.
1. Recopile el elemento `endpoint` configurado y una clave de API:

    |Pestaña de recursos en Portal|Configuración|Valor|
    |--|--|--|
    |**Información general**|Punto de conexión|Copie el punto de conexión. Es similar a `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`|
    |**Claves**|Clave de API|Copie una de las dos claves. Es una cadena de 32 caracteres alfanuméricos sin espacios ni guiones, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|