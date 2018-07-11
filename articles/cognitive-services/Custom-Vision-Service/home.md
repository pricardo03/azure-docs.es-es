---
title: Introducción al aprendizaje automático de Custom Vision Service - Azure Cognitive Services | Microsoft Docs
description: Custom Vision Service es un servicio de Microsoft Cognitive Services que le permite generar clasificadores personalizados de imágenes en la plataforma Azure.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: overview
ms.date: 05/02/2018
ms.author: anroth
ms.openlocfilehash: de45fc399470a806fb7456cbbe936cecf659ee7c
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2018
ms.locfileid: "37087802"
---
# <a name="what-is-the-custom-vision-service"></a>¿Qué es Custom Vision Service?

Custom Vision Service es un servicio de Microsoft Cognitive Services que le permite generar clasificadores personalizados de imágenes. Con este servicio, resulta muy fácil y rápido crear, implementar y mejorar un clasificador de imágenes. Custom Vision Service cuenta con una API REST y una interfaz web que permite cargar imágenes y entrenar el clasificador.

## <a name="what-does-custom-vision-service-do-well"></a>¿Cómo funciona Custom Vision Service?

Custom Vision Service funciona mejor cuando el elemento que intenta clasificar destaca claramente en la imagen. 

Para crear un clasificador o detector, se necesitan pocas imágenes. Para iniciar el prototipo, son suficientes 50 imágenes de cada clase. Los métodos que Custom Vision Service se concentran en las diferencias, lo que permite comenzar la creación de prototipos con tan pocos datos. Por tanto, Custom Vision Service no funciona bien en escenarios en los que se pretende detectar diferencias sutiles; por ejemplo, grietas o abolladuras pequeñas en escenarios de control de calidad.

## <a name="release-notes"></a>Notas de la versión

### <a name="may-7-2018"></a>7 de mayo de 2018
- Se ha incorporado la versión preliminar de la característica de detección de objetos para proyectos con versiones de prueba limitadas.
- Las API se han actualizado a la versión 2.0.
- El nivel S0 se ha ampliado hasta 250 etiquetas y 50 000 imágenes. 
- Se han incorporado mejoras importantes en los componentes back-end de la canalización de aprendizaje automático para los proyectos de clasificación de imágenes. Los proyectos entrenados después del 27 de abril de 2018 se beneficiarán de estas actualizaciones.
- Se ha agregado la exportación de modelos en ONNX para que pueda usarse con aprendizaje automático de Windows.
- Se ha agregado la exportación de modelos en Dockerfile. Esto le permite descargar los artefactos necesarios para generar sus propios contenedores de Windows o Linux, como un archivo DockerFile, un modelo de TensorFlow y un código del servicio. 
- En el caso de los modelos entrenados que acaban de exportarse a TensorFlow en los dominios General (compacto) y Landmark (compacto), [ahora los valores medios son (0,0,0)](https://github.com/azure-samples/cognitive-services-android-customvision-sample) para que sean coherentes en todos los proyectos. 

### <a name="march-1-2018"></a>1 de marzo de 2018
- Se ha incorporado una versión preliminar de pago y se ha integrado en Azure Portal. Ahora, los proyectos pueden vincularse a recursos de Azure utilizando un nivel F0 (gratis) o S0 (estándar). Se han incorporado proyectos en el nivel S0, lo que permite tener hasta 100 etiquetas y 25 000 imágenes. 
- Se han realizado cambios en los componentes back-end del parámetro de normalización y la canalización de aprendizaje automático. De este modo, los clientes tendrán un mayor control sobre el equilibrio entre precisión y exhaustividad al ajustar el umbral de probabilidad. Como parte de estos cambios, el umbral de probabilidad predeterminado del portal CustomVision.ai se ha establecido en 50 %.

### <a name="december-19-2017"></a>19 de diciembre de 2017

- Se ha incorporado la exportación a Android (TensorFlow), junto con la exportación a iOS (CoreML) publicada anteriormente. Esto permite exportar un modelo compacto entrenado para ejecutarlo sin conexión en una aplicación.
- Se han agregado los dominios "compactos" Retail y Landmark para permitir la exportaciones de modelos en estos dominios.
- Se ha publicado la versión [1.2 de Training API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/f2d62aa3b93843d79e948fe87fa89554/operations/5a3044ee08fa5e06b890f11f) y la versión [1.1 de Prediction API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/57982f59b5964e36841e22dfbfe78fc1/operations/5a3044f608fa5e06b890f164). Las API actualizadas admiten la exportación de modelos, una nueva operación de predicción que no guarda las imágenes en "Predictions" (Predicciones). Además, se han incorporado operaciones por lotes en Training API.
- Se han realizado pequeños ajustes en la experiencia del usuario, como la capacidad para ver qué dominio se ha utilizado para entrenar una iteración.
- Se ha actualizado el [SDK y el ejemplo de C#](https://github.com/Microsoft/Cognitive-CustomVision-Windows).

## <a name="next-steps"></a>Pasos siguientes

[Aprenda a crear un clasificador](getting-started-build-a-classifier.md)
