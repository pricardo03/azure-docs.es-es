---
title: Conceptos de diseño con simulación acústica
titlesuffix: Azure Cognitive Services
description: Esta información general conceptual explica cómo acústica proyecto incorpora acústico simulación para el proceso de diseño de sonido.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: kegodin
ms.openlocfilehash: 4a1a0b15da091a1c020eb132f6b14b9ee14d334c
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58316089"
---
# <a name="project-acoustics-design-process-concepts"></a>Conceptos de proceso de diseño de proyectos acústica

Esta información general conceptual explica cómo acústica proyecto incorpora simulación acústico físico en el proceso de diseño del sonido.

## <a name="sound-design-with-audio-dsp-parameters"></a>Diseño del sonido con parámetros de audio DSP

Títulos interactiva 3D logran su sonido en particular con audio bloques (DSP) hospedados en un motor de audio de procesamiento de señales digitales. Intervalo de estos bloques desde mezcla simples a reverberación, eco, retraso, estabilización, compresión y la limitación y otros efectos. Seleccionar, organizar y establecer parámetros en estos efectos son responsabilidad del Diseñador de sonido, que crea un gráfico de audio que logra los objetivos estética y juego de la experiencia.

¿En un título interactivo, como mover los sonidos y el agente de escucha en todo el espacio 3D, cómo estos parámetros adaptarse a las condiciones cambiantes? A menudo, el Diseñador de sonido organizará los volúmenes en todo el espacio que se programan para desencadenar cambios de parámetro para lograr los cambios en los efectos de reverberación, por ejemplo, así como la combinación de sonidos duck como el agente de escucha se mueve de una parte de la escena a otra. Sistemas acústica también están disponibles que puede automatizar algunos de estos efectos.

Títulos 3D usan sistemas de leyes físicas de iluminación y cinemática motivado por leyes físicas pero ajusta el diseñador para lograr una combinación de los objetivos de inmersión y juego. Un diseñador visual no establece los valores de píxeles individuales, pero en su lugar ajusta los modelos 3D, materiales y sistemas de transporte claro todo físicamente basadas para compensar la estética visual y los costos de CPU. ¿Cuál sería el proceso equivalente para audio? Proyecto acústica es un primer paso en la exploración de esta pregunta. En primer lugar tocaremos en lo que significa para el transporte de energía acústica a través de un espacio.

![Escena de la captura de pantalla de AltSpace superpuesta con zonas reverberación](media/reverb-zones-altspace.png)

## <a name="impulse-responses-acoustically-connecting-two-points-in-space"></a>Respuestas al impulso: Acústicamente conecta dos puntos en el espacio

Si está familiarizado con el diseño de audio, es posible que está familiarizado con las respuestas al impulso acústico. Una respuesta al impulso acústico modela el transporte de un sonido desde un origen a un agente de escucha. Por lo tanto, una respuesta al impulso puede capturar cada efecto de acústica de la sala como oclusión y reverberación interesante. Las respuestas de impulso también tienen algunas propiedades eficaces que permiten a los efectos de audio DSP escalar. Sumar dos señales de audio y el procesamiento con una respuesta al impulso ofrece el mismo resultado que aplicar la respuesta al impulso por separado para cada señal y agregar los resultados. Las respuestas al impulso y propagación acústica también no dependen el audio está procesando, solo en la escena que se está modelada y las ubicaciones de origen y el agente de escucha. En resumen, una respuesta al impulso extrae el efecto de la escena en la propagación de sonido.

Una respuesta al impulso captura cada interesante efecto acústico sala, podríamos aplicarlo al audio eficazmente con un filtro y podemos obtener respuestas de impulso de medida o simulación. Pero ¿qué ocurre si se no desea bastante la acústica para que coincida exactamente con la física, pero en su lugar moldear para que coincida con las exigencias de una escena emocionales? Pero al igual que los valores de píxel, una respuesta al impulso es simplemente una lista de miles de números, cómo podemos posiblemente ajústelo para satisfacer necesidades estéticas? Y ¿qué pasa si queremos tener oclusión/obstrucción que varía sin problemas al pasar a través de puertas o detrás de obstáculos, cuántas respuestas al impulso necesitamos obtener un suave efecto? ¿Qué ocurre si el origen se mueve rápido? ¿Cómo se interpolará?

Parece difícil de usar las respuestas al impulso y simulación para algunos aspectos de acústica en títulos interactivos. Pero, aún podemos crear un sistema de audio de transporte que admita los ajustes de diseñador si podemos conectar nuestras respuestas al impulso de simulación con nuestros parámetros de efecto DSP de audio familiares.

## <a name="connecting-simulation-to-audio-dsp-with-parameters"></a>Conexión de simulación DSP de audio con parámetros

Una respuesta al impulso contiene cada interesantes (y cada carente de interés) acústico efecto. Bloques de audio DSP, cuando sus parámetros se establecen correctamente, pueden representar interesante efecto acústico. Usa acústica simulación para dirigir un bloque DSP de audio para automatizar el transporte de audio en una escena 3D es sólo cuestión de medición de los parámetros DSP de audio de una respuesta al impulso. Esta medida se entiende bien para determinados efectos acústica importantes y comunes incluidos reverberación, obstrucción, portalling y oclusión.

¿Pero si la simulación está conectada directamente a los parámetros DSP de audio, donde es el ajuste del diseñador? ¿Qué obtienen? Bueno, logramos una cantidad significativa de memoria vuelve al descartar las respuestas de impulso y retener unos cuantos parámetros DSP. Y para dar al diseñador algo de energía a través del resultado final, necesitamos solo encontramos una manera de insertar el Diseñador de la simulación y el audio DSP.

![Los gráficos con la respuesta al impulso estilizados con parámetros superpuestos](media/acoustic-parameters.png)

## <a name="sound-design-by-transforming-audio-dsp-parameters-from-simulation"></a>Diseño del sonido mediante la transformación de audio DSP los parámetros de simulación

Tenga en cuenta el efecto que tienen sus gafas de sol en la vista del mundo. En un día brillante, las gafas pueden reducir el brillo en algo más cómodo. En una sala oscura, es posible que no pueda ver nada en absoluto. Las gafas no establecen un cierto nivel de brillo en todas las situaciones; simplemente hacer todo más oscuro.

Si usamos una simulación para controlar nuestra DSP de audio mediante los parámetros de oclusión y reverberación, podemos agregar un filtro después el simulador para ajustar los parámetros que el DSP 've'. El filtro no exigir un cierto nivel de oclusión o reverberación mucho la longitud de cola, por ejemplo, gafas de SOL no ponerlo cada habitación el brillo de la mismo. El filtro podría simplemente hacer que cada occluder occlude menor. O occlude mucho más. Agregando y ajuste de un filtro de parámetro oclusión 'oscurecimiento', salas de gran tamaño, abra no seguiría teniendo poca o ninguna influencia oclusión mientras puertas aumentaría desde un medio de un efecto de oclusión seguro, conservando la suavidad vigente transiciones que proporciona la simulación.

En este paradigma, tareas del diseñador cambia de la elección de acústicos parámetros para cada situación, el ajuste y seleccionar filtros para aplicarlos a los parámetros más importantes de DSP procedentes de simulación. Las actividades del diseñador elevan de las preocupaciones de configurar transiciones a las preocupaciones más alto de la intensidad de los efectos de oclusión y reverberación y la presencia de fuentes en la combinación de estrechas. Por supuesto, cuando se requiera la situación, siempre está disponible un filtro es simplemente vuelva a elegir los parámetros DSP para un origen específico en una situación concreta.

## <a name="sound-design-in-project-acoustics"></a>Diseño del sonido en el proyecto acústica

El paquete del proyecto acústica integra cada uno de los componentes que se ha descrito anteriormente: un simulador, un codificador que extrae los parámetros y que crea el recurso acústica DSP de audio y una selección de filtros. Diseño del sonido con proyecto acústica implica elegir parámetros para los filtros que ajustar los parámetros de oclusión y reverberación derivado de simulación y aplicar a audio DSP, con controles dinámicos que se exponen en el editor de juego y el motor de audio.

## <a name="next-steps"></a>Pasos siguientes
* Pruebe el paradigma de diseño mediante el [acústica proyecto quickstart para Unity](unity-quickstart.md) o [acústica proyecto quickstart para Unreal](unreal-quickstart.md)
* Explore el [proyecto acústica diseñar controles para Unity](unity-workflow.md) o [proyecto acústica diseñar controles de Unreal](unreal-workflow.md)

