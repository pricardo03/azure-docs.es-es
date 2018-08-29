---
title: 'Información general del proceso de diseño para la acústica: Cognitive Services'
description: En este documento se describe cómo expresar la intención del diseño en las tres fases del flujo de trabajo de los Elementos acústicos de un proyecto.
services: cognitive-services
author: kegodin
manager: noelc
ms.service: cognitive-services
ms.component: acoustics
ms.topic: article
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: 3b35f78d66a6ae66b0a56818f5d4be455ce00de5
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/17/2018
ms.locfileid: "40181428"
---
# <a name="design-process-overview"></a>Información general del proceso de diseño
Puede expresar la intención del diseño en las tres fases del flujo de trabajo de los Elementos acústicos de proyecto: diseño previo a la elaboración, integrar previamente el diseño, selección de ubicación de origen de sonido y el diseño posterior a la elaboración. El proceso requiere menos marcado asociado con la colocación de los volúmenes de reverberación mientras se conserva el control de diseñador sobre cómo suena una escena.

## <a name="pre-bake-design"></a>Diseño previo a la elaboración
El proceso de diseño previo a la elaboración genera la escena y los metadatos que se usan para la simulación de ondas de sonido, que incluye seleccionar los elementos de la escena que participarán en la simulación para proporcionar oclusiones, reflexiones y reverberaciones. Los metadatos de la escena son la selección de materiales acústicos para cada elemento de la escena. Los materiales acústicos controlan la cantidad de energía sonora que se refleja desde cada superficie.

El coeficiente de absorción predeterminado de todas las superficies es 0,04, que es altamente reflectante. Puede lograr efectos de estética y juego si ajusta los coeficientes de absorción de distintos materiales a lo largo de la escena, que son especialmente importantes para los oyentes cuando escuchan las transiciones de un área de la escena a otra. Por ejemplo, la transición desde una habitación reverberante a oscuras a una escena al aire libre luminosa y no reverberante mejora el impacto de la transición. Para lograr este efecto, ajuste los coeficientes de absorción de los materiales de la escena al aire libre para que sean más altos.

El tiempo de reverberación de un material determinado en una habitación está inversamente relacionado con su coeficiente de absorción y la mayoría de los materiales tiene valores de absorción en un rango que va desde 0,01 a 0,20. Los materiales con coeficientes de absorción que quedan fuera de este rango son muy absorbentes.

![Gráfico de tiempo de la reverberación](media/ReverbTimeGraph.png)

The [tutorial de interfaz de usuario de elaboración](bake-ui-walkthrough.md) describe en detalle los controlas que se realizan antes de la elaboración.

## <a name="sound-source-placement"></a>Selección de ubicación de origen de sonido
Ver los puntos de sondeo y lo vóxeles en tiempo de ejecución puede ayudar a depurar problemas con orígenes de sonido se estancan dentro de la geometría voxelizada. Para activar o desactivar la visualización de los puntos de sondeo y cuadrícula de vóxel, active la casilla correspondiente en el menú Gizmos que está en la esquina superior derecha de la vista de la escena. Si el origen de sonido está dentro de un vóxel de pared, muévalo a un vóxel aéreo.

![Menú Gizmos](media/GizmosMenu.png)  

La visualización del vóxel puede ayudar a determinar si los componentes visuales del juego tienen aplicada una transformación. Si es así, aplique la misma transformación al GameObject que hospeda al **Administrador de acústica**.

## <a name="post-bake-design"></a>Diseño posterior a la elaboración
Los resultados de la elaboración se almacenan en el archivo ACE como parámetros de oclusión y reverberación para todos los pares de ubicaciones de origen-oyente en toda la escena. Este resultado físicamente preciso se puede usar tal cual para el proyecto y se un punto inicial excelente para el diseño. El proceso de diseño posterior a la elaboración especifica reglas para transformar los parámetros de resultados de elaboración en tiempo de ejecución.

### <a name="distance-based-attenuation"></a>Atenuación basada en la distancia
El DSP de audio que proporciona el complemento de espacializador **Microsoft Acoustics** de Unity respeta la atenuación basada en distancia por origen que está integrada en el Editor Unity. Los controles de la atenuación basada en la distancia están en el componente **Audio Source** (Origen de datos) que se encuentra en el panel **Inspector** de orígenes de sonido, en **3D Sound Settings** (Ajustes de sonido 3D):

![Atenuación por distancia](media/distanceattenuation.png)

### <a name="tuning-scene-parameters"></a>Ajuste de los parámetros de escena
Para ajustar los parámetros de todos los orígenes, haga clic en la fila de canales de **Audio Mixer** (mezclador de audio) de Unity y ajuste los parámetros en el efecto **Acoustics Mixer**.

![Personalización del mezclador](media/MixerParameters.png)

### <a name="tuning-source-parameters"></a>Ajuste de los parámetros de origen
Adjuntar el script **AcousticsSourceCustomization** a un origen permite ajustar los parámetros de ese origen. Para adjuntar el script, haga clic en **Add Component** (Agregar componente) en la parte inferior del panel **Inspector** y navegue a **Scripts > Acoustics Source Customization** (Scripts > Personalización de orígenes de acústica). El script tiene tres parámetros:

![Personalización del origen](media/SourceCustomization.png)

* **Reverb Power Adjust** (Ajuste de potencia de reverberación): ajuste la potencia de reverberación, en dB. Los valores positivos emiten un sonido con más reverberación, mientras que los valores negativos emiten un sonido más seco.
* **Decay Time Scale** (Escala de tiempo de decadencia): ajusta un multiplicador para el tiempo de decadencia. Por ejemplo, si el resultado de la elaboración especifica un tiempo de decadencia de 750 milisegundos, pero este valor se establece en 1,5, el tiempo de decadencia aplicado en el origen es de 1125 milisegundos.
* **Enable Acoustics** (Habilitación de acústica): controla si se aplica acústica a este origen. Cuando la opción está desactivada, el origen se espacializará con HRTF, pero sin acústica, lo que significa sin parámetros de obstrucción, oclusión y reverberación dinámica, como el nivel y el tiempo de decadencia. La reverberación se sigue aplicando con un nivel fijo un tiempo de decadencia.

Los distintos orígenes pueden requerir configuraciones diferentes para lograr ciertos efectos estéticos o del juego. Los diálogos pueden ser un ejemplo. El oído humano está más a tono con la reverberación en el habla, mientras que el diálogo a menudo debe ser comprensible en el juego. Puede considerar esto sin que el diálogo deba ser diegético. Para ello, ajuste hacia abajo la potencia de la reverberación.
