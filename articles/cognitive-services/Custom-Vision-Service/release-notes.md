---
title: 'Notas de la versión: Custom Vision Service'
titlesuffix: Azure Cognitive Services
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 08/28/2018
ms.author: anroth
ms.openlocfilehash: bba65a495963142e8b3201a026dc13a8cfff8e70
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55207368"
---
# <a name="custom-vision-service-release-notes"></a>Notas de la versión de Custom Vision Service


## <a name="december-12-2018"></a>12 de diciembre de 2018
- Admite la exportación de modelos de detección de objetos (se ha introducido el dominio compacto de detección de objetos).
- Se han corregido varios problemas de accesibilidad para mejorar el lector de pantalla y el soporte para navegación del teclado. 
- Actualizaciones de la experiencia de usuario en el visor de imágenes y se ha mejorado el etiquetado de la detección de objetos, con el fin de agilizar el etiquetado.  
- Actualización del modelo base del dominio de detección de objetos para mejorar la calidad de la detección de objetos. 
- Correcciones de errores.

## <a name="november-6-2018"></a>6 de noviembre de 2018
- Se ha agregado compatibilidad con el dominio de logotipo en la detección de objetos.

## <a name="october-9-2018"></a>9 de octubre de 2018
- Detección de objetos entra en la versión preliminar de pago. Ahora puede crear proyectos de detección de objetos con un recurso de Azure.
- La característica "Mover a Azure" se ha agregado al sitio web, para que resulte más fácil actualizar un proyecto de prueba limitado para vincular a un proyecto de recursos vinculados de Azure (F0 o S0). Puede encontrarlo en la página Configuración del producto.  
- Se ha agregado exportación a ONNX 1.2, para admitir la versión Actualización de octubre de 2018 de Windows de Windows ML.
Correcciones de errores, incluidas para la exportación ONNX con caracteres especiales. 

## <a name="august-14-2018"></a>14 de agosto de 2018
- Se agregó un widget de "Introducción" al sitio customvision.ai para orientar a los usuarios a lo largo del aprendizaje del proyecto. 
- Otras mejoras en la canalización de aprendizaje automático para beneficiar a los proyectos multietiqueta (nueva capa de pérdida).

## <a name="june-28-2018"></a>28 de junio de 2018
- Corrección de errores y mejoras del back-end.
- Clasificación multiclase habilitada, para proyectos en los que las imágenes tienen exactamente una etiqueta. En las predicciones, para el modo multiclase, las probabilidades suman uno (todas las imágenes se clasifican entre las etiquetas especificadas).

## <a name="june-13-2018"></a>13 de junio de 2018
- Actualización de la experiencia del usuario centrada en la facilidad de uso y la accesibilidad. 
- Mejoras en la canalización de aprendizaje automático para beneficiar a los proyectos multietiqueta con una gran cantidad de etiquetas.
- Se corrigió el error en la exportación de TensorFlow. Se han habilitado las versiones de modelo exportado, para que las iteraciones se puedan exportar más de una vez. 

## <a name="may-7-2018"></a>7 de mayo de 2018
- Se ha incorporado la versión preliminar de la característica de detección de objetos para proyectos con versiones de prueba limitadas.
- Las API se han actualizado a la versión 2.0.
- El nivel S0 se ha ampliado hasta 250 etiquetas y 50 000 imágenes. 
- Se han incorporado mejoras importantes en los componentes back-end de la canalización de aprendizaje automático para los proyectos de clasificación de imágenes. Los proyectos entrenados después del 27 de abril de 2018 se beneficiarán de estas actualizaciones.
- Se ha agregado la exportación de modelos en ONNX para que pueda usarse con aprendizaje automático de Windows.
- Se ha agregado la exportación de modelos en Dockerfile. Esto le permite descargar los artefactos necesarios para generar sus propios contenedores de Windows o Linux, como un archivo DockerFile, un modelo de TensorFlow y un código del servicio. 
- En el caso de los modelos recién entrenados que se exportan a TensorFlow en los dominios General (compacto) y Landmark (compacto), [ahora los valores medios son (0,0,0)](https://github.com/azure-samples/cognitive-services-android-customvision-sample) para que sean coherentes en todos los proyectos. 

## <a name="march-1-2018"></a>1 de marzo de 2018
- Se ha incorporado una versión preliminar de pago y se ha integrado en Azure Portal. Ahora, los proyectos pueden vincularse a recursos de Azure utilizando un nivel F0 (gratis) o S0 (estándar). Se han incorporado proyectos en el nivel S0, lo que permite tener hasta 100 etiquetas y 25 000 imágenes. 
- Se han realizado cambios en los componentes back-end del parámetro de normalización y la canalización de aprendizaje automático. De este modo, los clientes tendrán un mayor control sobre el equilibrio entre precisión y exhaustividad al ajustar el umbral de probabilidad. Como parte de estos cambios, el umbral de probabilidad predeterminado del portal CustomVision.ai se ha establecido en 50 %.

## <a name="december-19-2017"></a>19 de diciembre de 2017

- Se ha incorporado la exportación a Android (TensorFlow), junto con la exportación a iOS (CoreML) publicada anteriormente. Esto permite exportar un modelo compacto entrenado para ejecutarlo sin conexión en una aplicación.
- Se han agregado los dominios "compactos" Retail y Landmark para permitir la exportaciones de modelos en estos dominios.
- Se ha publicado la versión [1.2 de Training API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/f2d62aa3b93843d79e948fe87fa89554/operations/5a3044ee08fa5e06b890f11f) y la versión [1.1 de Prediction API](https://southcentralus.dev.cognitive.microsoft.com/docs/services/57982f59b5964e36841e22dfbfe78fc1/operations/5a3044f608fa5e06b890f164). Las API actualizadas admiten la exportación de modelos, una nueva operación de predicción que no guarda las imágenes en "Predictions" (Predicciones). Además, se han incorporado operaciones por lotes en Training API.
- Se han realizado pequeños ajustes en la experiencia del usuario, como la capacidad para ver qué dominio se ha utilizado para entrenar una iteración.
- Se ha actualizado el [SDK y el ejemplo de C#](https://github.com/Microsoft/Cognitive-CustomVision-Windows).

