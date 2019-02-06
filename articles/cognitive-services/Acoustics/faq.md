---
title: Preguntas más frecuentes sobre Elementos acústicos de un proyecto
titlesuffix: Azure Cognitive Services
description: En esta página se proporcionan respuestas a las preguntas más frecuentes sobre Elementos acústicos de un proyecto, incluida la descarga de instrucciones y el proceso de elaboración.
services: cognitive-services
author: kegodin
manager: cgronlun
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 07e65e6ff544d6372197010a2b9d7f3f647eeb0f
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55166567"
---
# <a name="frequently-asked-questions"></a>Preguntas más frecuentes

## <a name="what-is-project-acoustics"></a>¿Qué son los elementos acústicos de un proyecto?

El complemento de Elementos acústicos de un proyecto de Unity es un sistema acústico que calcula el comportamiento de las ondas de sonido antes de tiempo de ejecución, de manera similar a una luz estática. La nube hace el trabajo pesado de los cálculos de física de ondas, por lo que el costo de CPU en tiempo de ejecución es bajo.  

## <a name="where-can-i-download-the-plugin"></a>¿Dónde puedo descargar el complemento?

Si está interesado en evaluar el complemento de acústica, se puede registrar [aquí](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u) para unirse a la versión preliminar del diseñador.

## <a name="is-azure-used-at-runtime"></a>¿Se usa Azure en tiempo de ejecución?

No, la integración de la nube solo se usa durante la fase de proceso previo como parte de la configuración de la escena.
 
## <a name="what-is-simulation-input"></a>¿Qué es la entrada de simulación? 

La entrada de simulación es la escena 3D, el entorno virtual o el nivel del juego. Los Elementos acústicos de un proyecto llevan a cabo simulaciones de ondas volumétricas en 3D que modelan de manera cercana la física del archivo de sonido, incluida la dispersión y la oclusión fluida.
 
## <a name="what-is-the-runtime-cost"></a>¿Cuál es el costo del entorno de ejecución?

La acústica usa aproximadamente el 0,01 % de CPU por origen por fotograma. El uso de RAM depende del tamaño de la escena y puede oscilar entre 10 y 100 MB.
 
## <a name="do-i-need-to-simplify-the-level-geometry-control-triangle-count-make-meshes-watertight"></a>¿Es necesario simplificar la geometría de nivel? ¿Controlar el recuento de triángulos? ¿Hacer que las mallas sean estancas?

 No. El sistema ingerirá directamente la geometría de nivel detallada. Se voxelizará para procesamiento interno.
 
## <a name="whats-in-the-runtime-lookup-table"></a>¿Qué hay en la tabla de búsqueda del entorno de ejecución?

El archivo ACE es una tabla de parámetros acústicos entre varios pares de ubicación de origen y oyente.
 
## <a name="can-it-handle-moving-sources"></a>¿Puede controlar orígenes móviles?

Sí, el complemento de espacializador **Microsoft Acoustics** de Unity consulta la tabla de búsqueda en cada tic de procesamiento de audio con las ubicaciones actuales de origen y oyente. El DSP del espacializador actualiza sin problemas los parámetros de procesamiento acústico en cada tic.
 
## <a name="can-it-handle-dynamic-geometry-closing-doors-walls-blown-away"></a>¿Puede controlar la geometría dinámica? ¿El cierre de puertas? ¿Las paredes destruidas?

 No. Los parámetros acústicos se procesan previamente en función del estado estático de un nivel de juego. Se recomienda dejar la geometría de las puertas fuera de la acústica y luego aplicar oclusión adicional en función del estado de los objetos de juego destructibles y movibles mediante el uso de técnicas establecidas.
 
## <a name="does-it-handle-materials"></a>¿Controla los materiales?

Sí. Los materiales se seleccionan de los nombres de materiales físicos en su nivel, lo que impulsa la capacidad de absorción.
 
## <a name="what-do-the-probes-represent"></a>¿Qué representan los "sondeos"?

Los sondeos son un muestreo de posibles ubicaciones del reproductor. Cada sondeo representa una simulación de onda independiente de la escena que se origina en la ubicación del sondeo. En entorno de ejecución, los parámetros acústicos de la ubicación del oyente se interpolan desde ubicaciones de sondeo cercanas.
 
## <a name="why-spend-so-much-compute-in-the-cloud-what-does-it-buy-me"></a>¿Por qué gastar tanto proceso en la nube? ¿Qué beneficio se obtiene?

Elementos acústicos de un proyecto brinda parámetros acústicos precisos y confiables incluso para entornos virtuales extremadamente complejos, que toma en cuenta cada aspecto de la arquitectura. Ofrece obstrucción/oclusión fluida sin todo el trabajo manual y la variación de reverberación dinámica sin extraer volúmenes. Todo esto mientras se mantiene el uso ligero de la CPU durante el entorno de tiempo de ejecución.

## <a name="what-exactly-happens-during-baking"></a>¿Qué ocurre exactamente durante la "elaboración"?

El sistema considera posibles ubicaciones del reproductor para generar un conjunto de posiciones de ejemplo de "sondeo" espaciadas de manera uniforme. Una simulación mediante "bake" de un nivel consta de tareas independientes para cada sondeo: el sistema considera una "región de simulación" cuboide centrada en el sondeo y realiza una simulación de onda detallada dentro en dicha región con una resolución de hasta 25 cm.

## <a name="next-steps"></a>Pasos siguientes
* Explore la [escena de ejemplo](sample-walkthrough.md)

