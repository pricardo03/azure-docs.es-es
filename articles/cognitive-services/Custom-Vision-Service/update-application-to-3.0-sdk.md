---
title: Cómo migrar el proyecto a la versión 3.0 de API
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre cómo migrar proyectos de Custom Vision desde la versión anterior de la API a la versión 3.0 de API.
services: cognitive-services
author: areddish
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 04/04/2019
ms.author: areddish
ms.openlocfilehash: 9dd473aadd7123cafc27209f5c34322fdbcffb71
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60816464"
---
# <a name="migrate-to-the-30-api"></a>Migrar a la versión 3.0 de API

Custom Vision ahora ha alcanzado la disponibilidad General y ha experimentado una actualización de la API.
Esta actualización incluye algunas características nuevas y, lo que es importante, algunos cambios importantes:

* Ahora, la API de predicción se dividen en dos según el tipo de proyecto.
* La opción de exportación del Kit de desarrollador de inteligencia artificial de visión (VAIDK) requiere la creación de un proyecto de una manera específica.
* Iteraciones de forma predeterminada se han quitado en favor de una publicación o cancelar la publicación de una iteración con nombre.

Esta guía le mostrará cómo actualizar los proyectos para que funcione con la nueva versión de API. Consulte la [notas de la versión](release-notes.md) para obtener una lista completa de los cambios.

## <a name="use-the-updated-prediction-api"></a>Usar la API de predicción actualizada

La API 2.x usa la misma llamada de predicción de clasificadores de imagen y proyectos del detector de objeto. Ambos tipos de proyecto eran aceptables para el **PredictImage** y **PredictImageUrl** llamadas. A partir de 3.0, hemos dividimos esta API para que sea necesario para que coincida con el tipo de proyecto a la llamada:

* Use **[ClassifyImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c15)** y **[ClassifyImageUrl](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c14)** para obtener predicciones para los proyectos de clasificación de imagen.
* Use **[DetectImage](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c19)** y **[DetectImageUrl](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Prediction_3.0/operations/5c82db60bf6a2b11a8247c18)** para obtener predicciones para los proyectos de detección de objeto.

## <a name="use-the-new-iteration-publishing-workflow"></a>Usar el nuevo flujo de trabajo de publicación de iteración

La API 2.x usa la iteración predeterminada o un identificador de la iteración especificada para elegir la iteración que desea usar para la predicción. A partir de 3.0, hemos adoptado un flujo de publicación mediante el cual se publica una iteración en un nombre especificado de la API de aprendizaje. A continuación, pase el nombre a los métodos de predicción para especificar qué iteración para usar.

> [!IMPORTANT]
> La versión 3.0 de API no utilizan la característica de iteración predeterminada. Hasta que se descartan las API anteriores, aún puede usar la API 2.x para activar o desactivar una iteración como valor predeterminado. Estas API se mantendrá durante un período de tiempo, y puede llamar a la **[UpdateIteration](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.0/operations/5c771cdcbf6a2b18a0c3b818)** método para marcar una iteración como valor predeterminado.

### <a name="publish-an-iteration"></a>Publicar una iteración

Una vez que se entrena una iteración, puede que esté disponible para la predicción usando el **[PublishIteration](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.0/operations/5c82db28bf6a2b11a8247bbc)** método. Para publicar una iteración, necesitará el identificador de recurso de predicción, que está disponible en la página de configuración del sitio Web de CustomVision.

![La página de configuración del sitio Web de visión personalizada con el identificador de recurso de predicción que se describen.](./media/update-application-to-3.0-sdk/prediction-id.png)

> [!TIP]
> También puede obtener esta información desde el [Portal de Azure](https://portal.azure.com) , vaya al recurso de predicción de Custom Vision y seleccione **propiedades**.

Una vez publicada la iteración, las aplicaciones pueden usarlo para la predicción especificando el nombre de la llamada de API de predicción. Para realizar una iteración no está disponible para las llamadas de predicción, use el **[UnpublishIteration](https://southcentralus.dev.cognitive.microsoft.com/docs/services/Custom_Vision_Training_3.0/operations/5c771cdcbf6a2b18a0c3b81a)** API.

## <a name="additional-export-options"></a>Más opciones de exportación

Con la versión 3.0 de los destinos de exportación de API nos estamos exponer dos adicionales: Arquitectura ARM y Kit de desarrollo de inteligencia artificial de visión.

* Para utilizar ARM, basta con seleccionar un dominio Compact y, a continuación, elija el archivo DockerFile y ARM, a continuación, como las opciones de exportación.
* Visión de inteligencia artificial Dev Kit, el proyecto debe crearse con el __General (compacto)__ dominio así como especificar VAIDK en el destino de exportación de argumento de plataformas.

## <a name="next-steps"></a>Pasos siguientes

* [Documentación de referencia de API de aprendizaje (REST)](https://go.microsoft.com/fwlink/?linkid=865446)
* [Documentación de referencia de API de predicción (REST)](https://go.microsoft.com/fwlink/?linkid=865445)