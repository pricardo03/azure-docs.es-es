---
title: Compatibilidad con los contenedores
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: cbf11c13bfb5c90739ea67fab92df08796a88e50
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717286"
---
## <a name="create-an-computer-vision-resource"></a>Creación de un recurso de Computer Vision

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
1. Haga clic en [Crear **un recurso de Computer Vision**](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesComputerVision).
1. Establezca todas las opciones de configuración necesarias:

    |Configuración|Valor|
    |--|--|
    |NOMBRE|Nombre que desee (2-64 caracteres)|
    |Subscription|Seleccione una suscripción adecuada|
    |Location|Seleccione cualquier ubicación disponible, cercana|
    |Nivel de precios|`F0`: el plan de tarifa mínimo|
    |Grupo de recursos|Seleccione un grupo de recursos disponible|

1. Haga clic en **Create** (Crear) y espere a que el recurso se cree. Después de crearlo, vaya a la página de recursos
1. Recopile el elemento `endpoint` configurado y una clave de API:

    |Pestaña de recursos en Portal|Configuración|Valor|
    |--|--|--|
    |**Información general**|Punto de conexión|Copie el punto de conexión. Es similar a `https://computer-vision.cognitiveservices.azure.com/`|
    |**Claves**|Clave de API|Copie una de las dos claves. Es una cadena de 32 caracteres alfanuméricos sin espacios ni guiones, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
