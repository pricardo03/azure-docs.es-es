---
title: Uso de la aplicación precompilada Cortana de LUIS | Microsoft Docs
description: Use el asistente personal Cortana, una aplicación precompilada de Language Understanding Intelligent Service (LUIS).
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: c7249cb8d8cff29f419412025c69e3b2b76b49d1
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37110664"
---
# <a name="cortana-prebuilt-app"></a>Aplicación precompilada Cortana

> [!IMPORTANT]
> Se recomienda utilizar los [dominios precompilados](./luis-how-to-use-prebuilt-domains.md), en lugar de la aplicación precompilada Cortana. Por ejemplo, en lugar de **builtin.intent.calendar.create_calendar_entry**, use **Calendar.Add** desde el dominio precompilado **Calendario**.
> Los dominios precompilados ofrecen estas ventajas: 
> * Proporcionan paquetes de intenciones y entidades precompiladas y preentrenadas que se diseñan para que funcionen bien entre sí. Puede integrar un dominio precompilado directamente en la aplicación. Por ejemplo, si va a compilar una aplicación de seguimiento de fitness, puede agregar el dominio **Fitness** y disponer de un conjunto completo de intenciones y entidades para realizar un seguimiento de actividades de fitness, incluidas las intenciones para realizar el seguimiento del peso y el plan de comidas, el tiempo o la distancia restantes y guardar notas sobre la actividad de fitness.
> * Las intenciones del dominio precompilado son personalizables. Por ejemplo, si desea proporcionar opiniones de hoteles, puede entrenar y personalizar la intención **Places.GetReviews** desde el dominio **Lugares** para reconocer solicitudes de opiniones de hoteles.
> * Los dominios precompilados son extensibles. Por ejemplo, si desea usar el dominio precompilado **Lugares** en un bot que busca restaurantes y necesita una intención para obtener el tipo de cocina, puede compilar y entrenar una intención **Places.GetCuisine**.

Además de permitirle compilar sus propias aplicaciones, LUIS también ofrece intenciones y entidades en el asistente personal de Microsoft Cortana como una aplicación precompilada. No se puede cambiar el comportamiento de esta aplicación LUIS disponible públicamente. Las intenciones y entidades de esta aplicación no se pueden editar ni integrar en otras aplicaciones LUIS. Si desea que la aplicación cliente tenga acceso tanto a esta aplicación precompilada como a su propia aplicación LUIS, entonces la aplicación cliente tiene que hacer referencia a ambas aplicaciones LUIS.

La aplicación del asistente personal precompilada está disponible en estas referencias culturales (configuraciones regionales): inglés, francés, italiano, español y chino.

## <a name="get-the-endpoint-for-the-cortana-prebuilt-app"></a>Obtención del punto de conexión de la aplicación precompilada Cortana

Puede acceder a la aplicación precompilada Cortana con los siguientes puntos de conexión: 

| Idioma | Punto de conexión|
|--------| ------------------|
| English| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c413b2ef-382c-45bd-8ff0-f76d60e2a821|
|    Chino| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c27c4af7-d44a-436f-a081-841bb834fa29|
|    Francés| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/0355ead1-2d08-4955-ab95-e263766e8392|
|    Español| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/cb2675e5-fbea-4f8b-8951-f071e9fc7b38|
|    Italiano| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/30a0fddc-36f4-4488-b022-03de084c1633|


> [!NOTE]
> Las direcciones URL del punto de conexión también están disponibles en [apps - Get personal assistant applications](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c32) API.

## <a name="try-out-the-personal-assistant-app"></a>Prueba de la aplicación de asistente personal
Para llamar al punto de conexión, puede anexar el argumento de la clave de punto de conexión y la cadena de consulta al punto de conexión. 

Por ejemplo, si la expresión que desea interpretar es "crear una cita de la reunión del equipo", puede anexar esa expresión a la dirección URL del punto de conexión. 

```
https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/c413b2ef-382c-45bd-8ff0-f76d60e2a821?subscription-key={YOUR-SUBSCRIPTION-KEY}&q=create an appointment for team meeting
```

Puede pegar la dirección URL en un explorador web y sustituir la clave de punto de conexión para el campo `{YOUR-SUBSCRIPTION-KEY}`.

En el explorador, puede ver que la aplicación precompilada Cortana identifica `builtin.intent.calendar.create_calendar_entry` como la intención y `builtin.calendar.title` como el tipo de entidad, y para la expresión `create an appointment for team meeting`.

![Uso de la aplicación precompilada Cortana](./media/luis-how-to-prebuilt-cortana/luis-prebuilt-cortana-browser.png)

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Referencia a la aplicación creada previamente con Cortana](./luis-reference-cortana-prebuilt.md)

