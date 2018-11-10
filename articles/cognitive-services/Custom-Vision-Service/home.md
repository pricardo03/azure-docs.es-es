---
title: ¿Qué es Azure Custom Vision?
titlesuffix: Azure Cognitive Services
description: Aprenda a usar Custom Vision Service para crear clasificadores personalizados de imágenes en la nube de Azure.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: overview
ms.date: 10/26/2018
ms.author: anroth
ms.openlocfilehash: 5e8e675b32bfd4c741b82b1ab341a80adbb0529d
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741647"
---
# <a name="what-is-azure-custom-vision"></a>¿Qué es Azure Custom Vision?

La API de Azure Custom Vision es un servicio de Azure Cognitive Services que le permite crear, implementar y mejorar clasificadores personalizados de imágenes. Un clasificador de imágenes es un servicio de IA que clasifica las imágenes en clases (etiquetas) de acuerdo con ciertas características. A diferencia de [Computer Vision](https://docs.microsoft.com/azure/cognitive-services/computer-vision/home) Service, Custom Vision le permite crear sus propias clasificaciones.

## <a name="what-it-does"></a>Qué hace

Custom Vision Service usa un algoritmo de aprendizaje automático para clasificar las imágenes. Como desarrollador debe enviar los grupos de imágenes que presenten y carezcan de las clasificaciones en cuestión. Especifique las etiquetas correctas de las imágenes en el momento del envío. Luego, el algoritmo se entrena en estos datos y calcula su propia precisión probándose a sí mismo en esos mismos datos. Cuando se entrena el modelo, puede probarlo, volver a entrenarlo y finalmente usarlo para clasificar nuevas imágenes en función de las necesidades de la aplicación. También puede exportar el mismo modelo para su uso sin conexión.

### <a name="classification-and-object-detection"></a>Clasificación y detección de objetos

La funcionalidad de Custom Vision puede dividirse en dos características. **Clasificación de imágenes**: asigna una distribución de las clasificaciones a cada imagen. **Detección de objetos**: es similar, pero también devuelve las coordenadas de la imagen donde pueden encontrarse las etiquetas aplicadas.

### <a name="optimization"></a>Optimización

En general, los métodos que Custom Vision Service se concentran en las diferencias, lo que permite comenzar la creación de prototipos con tan pocos datos. 50 imágenes por etiqueta suelen ser un buen comienzo. Esto significa, sin embargo, que el servicio no es óptimo para detectar diferencias sutiles en las imágenes (por ejemplo, detectar grietas menores o abolladuras en escenarios de control de calidad).

Además, puede elegir entre diversas variedades del algoritmo de Custom Vision que están optimizadas para cierto material temático&mdash;, por ejemplo, puntos de referencia o artículos de venta. Para más información, consulte la guía [Compilación de un clasificador](getting-started-build-a-classifier.md).

## <a name="what-it-includes"></a>Qué incluye
Custom Vision Service está disponible como un conjunto de SDK nativos, así como mediante una interfaz basada en web en la [página principal de Custom Vision](https://customvision.ai/). Puede crear, probar y entrenar un modelo mediante una de las interfaces, o ambas.

![Página principal de Custom Vision en una ventana del explorador Chrome](media/browser-home.png)

## <a name="next-steps"></a>Pasos siguientes

Siga la guía para [crear un clasificador](getting-started-build-a-classifier.md) para empezar a usar Custom Vision en la web o complete un [tutorial de clasificación de imágenes](csharp-tutorial.md) para implementar el escenario en el código.
