---
title: Preguntas más frecuentes sobre el proyecto acústica
titlesuffix: Azure Cognitive Services
description: En esta página se proporcionan respuestas a las preguntas más frecuentes sobre Elementos acústicos de un proyecto, incluida la descarga de instrucciones y el proceso de elaboración.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: resources
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 3426681aad19bbe01c0f7e88ca16e79c0b490c36
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58317313"
---
# <a name="project-acoustics-frequently-asked-questions"></a>Preguntas más frecuentes sobre el proyecto acústica

## <a name="what-is-project-acoustics"></a>¿Qué son los elementos acústicos de un proyecto?

El conjunto de acústica del proyecto de complementos es un sistema de acústica que calcula el comportamiento de onda de sonido antes de tiempo de ejecución, similar a la iluminación estático. La nube hace el trabajo pesado de los cálculos de física de ondas, por lo que el costo de CPU en tiempo de ejecución es bajo.  

## <a name="where-can-i-download-the-plugin"></a>¿Dónde puedo descargar el complemento?

Puede descargar el [proyecto acústica Unity complemento](https://www.microsoft.com/download/details.aspx?id=57346) o [proyecto acústica Unreal complemento](https://www.microsoft.com/download/details.aspx?id=58090).

## <a name="does-project-acoustics-support-x-platform"></a>¿Es compatible con el proyecto acústica <x> plataforma?

Compatibilidad con plataformas de proyecto acústica evoluciona en función de las necesidades del cliente. Póngase en contacto con nosotros en el [foros del proyecto acústica](https://social.msdn.microsoft.com/Forums/en-US/home?forum=projectacoustics) para solicitar información sobre la compatibilidad de plataformas adicionales.

## <a name="is-azure-used-at-runtime"></a>¿Se usa Azure en tiempo de ejecución?

No, la integración de la nube solo se usa durante la fase de proceso previo como parte de la configuración de la escena.
 
## <a name="what-is-simulation-input"></a>¿Qué es la entrada de simulación? 

La entrada de simulación es la escena 3D, el entorno virtual o el nivel del juego. Los Elementos acústicos de un proyecto llevan a cabo simulaciones de ondas volumétricas en 3D que modelan de manera cercana la física del archivo de sonido, incluida la dispersión y la oclusión fluida.
 
## <a name="what-is-the-runtime-cost"></a>¿Cuál es el costo del entorno de ejecución?

La acústica usa aproximadamente el 0,01 % de CPU por origen por fotograma. El uso de RAM depende del tamaño de la escena y puede oscilar entre 10 y 100 MB.
 
## <a name="do-i-need-to-simplify-the-level-geometry-control-triangle-count-make-meshes-watertight"></a>¿Es necesario simplificar la geometría de nivel? ¿Controlar el recuento de triángulos? ¿Hacer que las mallas sean estancas?

 No. El sistema ingerirá directamente la geometría de nivel detallada. Se voxelizará para procesamiento interno.
 
## <a name="whats-in-the-runtime-lookup-table"></a>¿Qué hay en la tabla de búsqueda del entorno de ejecución?

Incluye el archivo ACE es una tabla de parámetros acústicos entre numerosas origen y pares de ubicación del agente de escucha, así como geometría de escena voxelized usada para la interpolación de parámetro.
 
## <a name="can-project-acoustics-handle-moving-sources"></a>¿Puede acústica proyecto administrar orígenes de móviles?

Sí, proyecto acústica consulta la tabla de búsqueda y actualiza el audio DSP en cada paso, para que pueda administrar el movimiento de orígenes y agente de escucha.
 
## <a name="can-project-acoustics-handle-dynamic-geometry-closing-doors-walls-blown-away"></a>¿Es capaz de geometría dinámica acústica proyecto? ¿El cierre de puertas? ¿Las paredes destruidas?

 No. Los parámetros acústicos se procesan previamente en función del estado estático de un nivel de juego. Se recomienda dejar la geometría puerta fuera acústica y aplicando después oclusión adicional según el estado de puede destruir y puede mover objetos de juego mediante establece técnicas.
 
## <a name="does-project-acoustics-use-acoustic-materials"></a>¿Acústica proyecto utiliza materiales acústicos?

Sí. Los materiales se seleccionan de los nombres de materiales físicos en su nivel, lo que impulsa la capacidad de absorción.
 
## <a name="what-do-the-probes-represent"></a>¿Qué representan los "sondeos"?

Los sondeos son un muestreo de posibles ubicaciones del reproductor. Cada sondeo representa una simulación de onda independiente de la escena que se origina en la ubicación del sondeo. En entorno de ejecución, los parámetros acústicos de la ubicación del oyente se interpolan desde ubicaciones de sondeo cercanas.
 
## <a name="why-spend-so-much-compute-in-the-cloud-what-does-it-buy-me"></a>¿Por qué gastar tanto proceso en la nube? ¿Qué beneficio se obtiene?

Elementos acústicos de un proyecto brinda parámetros acústicos precisos y confiables incluso para entornos virtuales extremadamente complejos, que toma en cuenta cada aspecto de la arquitectura. Proporciona la oclusión smooth y obstrucción y variación reverberación dinámico sin el trabajo manual de los volúmenes de dibujo. Todo esto mientras se mantiene el uso ligero de la CPU durante el entorno de tiempo de ejecución.

## <a name="what-exactly-happens-during-baking"></a>¿Qué ocurre exactamente durante la "elaboración"?

Un hornearlos consta de simulaciones de wave acústico de regiones de simulación cuboid centradas en cada sondeo del agente de escucha.

## <a name="next-steps"></a>Pasos siguientes
* Pruebe el [contenido de ejemplo de proyecto acústica Unity](unity-quickstart.md) o [contenido de ejemplo Unreal](unreal-quickstart.md)

