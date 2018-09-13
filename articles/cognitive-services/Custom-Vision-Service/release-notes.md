---
title: Notas de la versión de Custom Vision Service
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.date: 08/28/2018
ms.author: anroth
ms.openlocfilehash: 8423051fa5169eb8acddc7297e36188e297cb9e4
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "43288664"
---
# <a name="custom-vision-service-release-notes"></a>Notas de la versión de Custom Vision Service

## <a name="august-14-2018"></a>14 de agosto de 2018
- Se agregó un widget de "Introducción" al sitio customvision.ai para orientar a los usuarios a lo largo del aprendizaje del proyecto. 
- Otras mejoras en la canalización de aprendizaje automático para beneficiar a los proyectos multietiqueta (nueva capa de pérdida).

## <a name="june-28-2018"></a>28 de junio de 2018
- Corrección de errores y mejoras del back-end.
- Clasificación multiclase habilitada para proyectos en que las imágenes tienen, exactamente, una etiqueta. En las predicciones, para el modo multiclase, las probabilidades suman uno (todas las imágenes se clasifican entre las etiquetas especificadas).

## <a name="june-13-2018"></a>13 de junio de 2018
- Actualización de la experiencia del usuario centrada en la facilidad de uso y la accesibilidad. 
- Mejoras en la canalización de aprendizaje automático para beneficiar a los proyectos multietiqueta con una gran cantidad de etiquetas.
- Se corrigió el error en la exportación de TensorFlow. Se habilitaron las versiones de modelo exportado para que las iteraciones se puedan exportar más de una vez. 

## <a name="may-7-2018"></a>7 de mayo de 2018
- Se ha incorporado la versión preliminar de la característica de detección de objetos para proyectos con versiones de prueba limitadas.
- Las API se han actualizado a la versión 2.0.
- El nivel S0 se ha ampliado hasta 250 etiquetas y 50 000 imágenes. 
- Se han incorporado mejoras importantes en los componentes back-end de la canalización de aprendizaje automático para los proyectos de clasificación de imágenes. Los proyectos entrenados después del 27 de abril de 2018 se beneficiarán de estas actualizaciones.
- Se ha agregado la exportación de modelos en ONNX para que pueda usarse con aprendizaje automático de Windows.
- Se ha agregado la exportación de modelos en Dockerfile. Esto le permite descargar los artefactos necesarios para generar sus propios contenedores de Windows o Linux, como un archivo DockerFile, un modelo de TensorFlow y un código del servicio. 
- En el caso de los modelos entrenados que acaban de exportarse a TensorFlow en los dominios General (compacto) y Landmark (compacto), [ahora los valores medios son (0,0,0)](https://github.com/azure-samples/cognitive-services-android-customvision-sample) para que sean coherentes en todos los proyectos. 

## <a name="march-1-2018"></a>1 de marzo de 2018
- Se ha incorporado una versión preliminar de pago y se ha integrado en Azure Portal. Ahora, los proyectos pueden vincularse a recursos de Azure utilizando un nivel F0 (gratis) o S0 (estándar). Se han incorporado proyectos en el nivel S0, lo que permite tener hasta 100 etiquetas y 25 000 imágenes. 
- Se han realizado cambios en los componentes back-end del parámetro de normalización y la canalización de aprendizaje automático. De este modo, los clientes tendrán un mayor control sobre el equilibrio entre precisión y exhaustividad al ajustar el umbral de probabilidad. Como parte de estos cambios, el umbral de probabilidad predeterminado del portal CustomVision.ai se ha establecido en 50 %.

## <a name="december-19-2017"></a>19 de diciembre de 2017

- Se ha incorporado la exportación a Android (TensorFlow), junto con la exportación a iOS (CoreML) publicada anteriormente. Esto permite exportar un modelo compacto entrenado para ejecutarlo sin conexión en una aplicación.
- Se han agregado los dominios "compactos" Retail y Landmark para permitir la exportaciones de modelos en estos dominios.
- Se ha publicado la versión [1.2 de Training API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/f2d62aa3b93843d79e948fe87fa89554/operations/5a3044ee08fa5e06b890f11f) y la versión [1.1 de Prediction API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/57982f59b5964e36841e22dfbfe78fc1/operations/5a3044f608fa5e06b890f164). Las API actualizadas admiten la exportación de modelos, una nueva operación de predicción que no guarda las imágenes en "Predictions" (Predicciones). Además, se han incorporado operaciones por lotes en Training API.
- Se han realizado pequeños ajustes en la experiencia del usuario, como la capacidad para ver qué dominio se ha utilizado para entrenar una iteración.
- Se ha actualizado el [SDK y el ejemplo de C#](https://github.com/Microsoft/Cognitive-CustomVision-Windows).

