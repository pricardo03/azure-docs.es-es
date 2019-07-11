---
title: Documentación de Swagger Voz (servicios de voz)
titleSuffix: Azure Cognitive Services
description: La documentación de Swagger se puede utilizar para generar automáticamente SDK para varios lenguajes de programación. Todas las operaciones de nuestro servicio son compatibles con Swagger
services: cognitive-services
author: PanosPeriorellis
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: reference
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: f6420ea8918816f7356e567ec48cb0f7456065e0
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604769"
---
# <a name="swagger-documentation"></a>Documentación de Swagger

Los servicios de voz ofrecen una especificación de Swagger para interactuar con varias de las API REST que se usan para importar datos, crear modelos, probar la precisión de los modelos, crear puntos de conexión personalizados, poner en cola las transcripciones en lote y administrar las suscripciones. La mayoría de las operaciones disponibles a través del portal de Custom Speech se pueden completar mediante programación con estas API.

> [!NOTE]
> Se admiten las operaciones tanto de voz a texto como de texto a voz y están disponibles como API REST, que a su vez se documentan en la especificación de Swagger.

## <a name="generating-code-from-the-swagger-specification"></a>Generación de código desde la especificación Swagger

La [especificación Swagger](https://cris.ai/swagger/ui/index) tiene opciones que permiten probar rápidamente varias rutas de acceso. Sin embargo, a veces es conveniente generar código para todas las rutas de acceso, lo que crea una sola biblioteca de llamadas en la que se puedan basar las soluciones futuras. Echemos un vistazo al proceso para generar una biblioteca Python.

Tendrá que establecer Swagger en la misma región que su suscripción a Speech Service. Puede confirmar la región en Azure Portal en el recurso de servicios de voz. Para ver una lista completa de las regiones admitidas, consulte [Regiones](regions.md).

1. Vaya a https://editor.swagger.io.
2. Haga clic en **File** (Archivo) y, luego, en **Import** (Importar)
3. Escriba la dirección URL de Swagger, incluida la región de la suscripción a los servicios de voz `https://<your-region>.cris.ai/docs/v2.0/swagger`
4. Haga clic en **Generate Client** (Generar cliente) y seleccione Python
5. Guarde la biblioteca cliente

Puede usar la biblioteca Python que genera con los [ejemplos de servicios de voz en GitHub](https://aka.ms/csspeech/samples).

## <a name="reference-docs"></a>Documentos de referencia

* [REST (Swagger): Batch transcription and customization](https://westus.cris.ai/swagger/ui/index) (API de REST: Transcripción y personalización de Azure Batch)
* [API REST: Speech-to-text](rest-speech-to-text.md) (API de REST: Voz a texto)
* [API REST: Text-to-speech](rest-text-to-speech.md) (API de REST: Texto a voz)

## <a name="next-steps"></a>Pasos siguientes

* [Ejemplos de servicios de voz en GitHub](https://aka.ms/csspeech/samples).
* [Get a Speech Services subscription key for free](get-started.md) (Consiga una clave de suscripción a los servicios de voz gratis)
