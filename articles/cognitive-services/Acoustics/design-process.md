---
title: Información general del proceso de diseño para Elementos acústicos de un proyecto
titlesuffix: Azure Cognitive Services
description: En este documento se describe cómo expresar la intención del diseño en las tres fases del flujo de trabajo de los Elementos acústicos de un proyecto.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 08/17/2018
ms.author: kegodin
ms.openlocfilehash: bb5f309a96feac2caea85fbe81b7216eecfc4b79
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873944"
---
# <a name="design-process-overview"></a>Información general del proceso de diseño
Puede expresar la intención del diseño en las tres fases del flujo de trabajo Elementos acústicos del proyecto: configuración de la escena previa a la elaboración, ubicación del origen de sonido y diseño posterior a la elaboración. El proceso requiere menos marcado asociado con la colocación de los volúmenes de reverberación mientras se conserva el control de diseñador sobre cómo suena una escena.

## <a name="pre-bake-design"></a>Diseño previo a la elaboración
El proceso de configuración de la escena previa a la elaboración genera la escena y los metadatos que se usan para la simulación de ondas de sonido, lo que incluye la selección de los elementos de la escena que participarán en la simulación para proporcionar oclusiones, reflexiones y reverberaciones. Los metadatos de la escena son la selección de materiales acústicos para cada elemento de la escena. Los materiales acústicos controlan la cantidad de energía sonora que se refleja desde cada superficie.

El coeficiente de absorción predeterminado de todas las superficies es 0,04, que es altamente reflectante. Puede lograr efectos de estética y juego si ajusta los coeficientes de absorción de distintos materiales a lo largo de la escena, que son especialmente importantes para los oyentes cuando escuchan las transiciones de un área de la escena a otra. Por ejemplo, la transición desde una habitación reverberante a oscuras a una escena al aire libre luminosa y no reverberante mejora el impacto de la transición. Para lograr este efecto, ajuste los coeficientes de absorción de los materiales de la escena al aire libre para que sean más altos.

El tiempo de reverberación de un material determinado en una habitación está inversamente relacionado con su coeficiente de absorción y la mayoría de los materiales tiene valores de absorción en un rango que va desde 0,01 a 0,20. Los materiales con coeficientes de absorción que quedan fuera de este rango son muy absorbentes.

![Gráfico de tiempo de la reverberación](media/ReverbTimeGraph.png)

The [tutorial de interfaz de usuario de elaboración](bake-ui-walkthrough.md) describe en detalle los controlas que se realizan antes de la elaboración.

## <a name="sound-source-placement"></a>Selección de ubicación de origen de sonido
Ver los puntos de sondeo y lo vóxeles en tiempo de ejecución puede ayudar a depurar problemas con orígenes de sonido se estancan dentro de la geometría voxelizada. Para activar o desactivar la visualización de los puntos de sondeo y cuadrícula de vóxel, active la casilla correspondiente en el menú Gizmos que está en la esquina superior derecha de la vista de la escena. Si el origen de sonido está dentro de un vóxel de pared, muévalo a un vóxel aéreo.

![Menú Gizmos](media/GizmosMenu.png)  

La visualización del vóxel puede ayudar a determinar si los componentes visuales del juego tienen aplicada una transformación. Si es así, aplique la misma transformación al GameObject que hospeda al **Administrador de acústica**.

### <a name="voxel-size-discrepancies"></a>Discrepancias de tamaño de vóxel
Es posible que observe que el tamaño de los vóxeles que se usan para ilustrar cuál de las mallas de la escena participan en la elaboración acústica es distinto en las vistas de tiempo de ejecución y tiempo de diseño. Esta diferencia no afecta a la calidad o granularidad de la frecuencia de simulación seleccionada, sino que más bien es un biproducto del uso en tiempo de ejecución de la escena con vóxeles. En tiempo de ejecución, los vóxeles de simulación se "refinan" para admitir la interpolación entre las ubicaciones de origen. Esto también permite el posicionamiento en tiempo de diseño de los orígenes de sonido más cerca de las mallas de escena de lo que permite el tamaño de los vóxeles de simulación, dado que los orígenes dentro de un vóxel que contienen una malla tratada acústicamente no emiten ningún sonido.

Estas son dos imágenes en las que se muestra la diferencia entre vóxeles en tiempo de diseño (anteriores a la elaboración) y en tiempo de ejecución (posteriores a la elaboración), tal y como los visualiza el complemento de Unity:

Vóxeles en tiempo de diseño:

![VoxelsDesignTime](media/VoxelsDesignTime.png)

Vóxeles en tiempo de ejecución:

![VoxelsRuntime](media/VoxelsRuntime.png)

La decisión sobre si la malla de vóxeles representa con precisión las mallas de la escena de estructura o arquitectura se debe tomar con los vóxeles de modo de diseño, no mediante la visualización en tiempo de ejecución de los vóxeles refinados.

## <a name="post-bake-design"></a>Diseño posterior a la elaboración
Los resultados de la elaboración se almacenan en el archivo ACE como parámetros de oclusión y reverberación para todos los pares de ubicaciones de origen-oyente en toda la escena. Este resultado físicamente preciso se puede usar tal cual para el proyecto y se un punto inicial excelente para el diseño. El proceso de diseño posterior a la elaboración especifica reglas para transformar los parámetros de resultados de elaboración en tiempo de ejecución.

### <a name="distance-based-attenuation"></a>Atenuación basada en la distancia
El DSP de audio que proporciona el complemento de espacializador **Microsoft Acoustics** de Unity respeta la atenuación basada en distancia por origen que está integrada en el Editor Unity. Los controles de la atenuación basada en la distancia están en el componente **Audio Source** (Origen de datos) que se encuentra en el panel **Inspector** de orígenes de sonido, en **3D Sound Settings** (Ajustes de sonido 3D):

![Atenuación por distancia](media/distanceattenuation.png)

Acoustics realiza el cálculo en un cuadro de "región de simulación" centrado en la ubicación del reproductor. Si un origen de sonido está lejos del reproductor, ubicado fuera de esta región de simulación, solo la geometría dentro del cuadro afectará a la propagación del sonido (por ejemplo, provocará una oclusión), lo que funciona razonablemente bien cuando hay elementos oclusores próximos al reproductor. Pero en casos en los que el reproductor está en un espacio abierto pero hay elementos oclusores cerca de la fuente de sonido lejana, el sonido se puede desbloquear de forma poco realista. En estos casos, la solución alternativa que se recomienda es garantizar que la atenuación del sonido descienda a 0 a aproximadamente 45 m, la distancia horizontal predeterminada del reproductor con respecto el borde del cuadro.

### <a name="tuning-scene-parameters"></a>Ajuste de los parámetros de escena
Para ajustar los parámetros de todos los orígenes, haga clic en la fila de canales de **Audio Mixer** (Mezclador de audio) de Unity y ajuste los parámetros en el efecto **Project Acoustics Mixer** (Mezclador acústico del proyecto).

![Personalización del mezclador](media/MixerParameters.png)

* **Wetness Adjust** (Ajuste de humedad): ajusta la potencia de reverberación, en dB, en todos los orígenes de la escena en función de la distancia a la que se sitúa el agente de escucha de origen. Los valores positivos emiten un sonido con más reverberación, mientras que los valores negativos emiten un sonido más seco.
* **RT60 Scale** (Escala RT60): escalar multiplicativo para el tiempo de reverberación.
* **Use Panning** (Uso de panoramización): controlea si el sonido se emite como binaural (0) o con panoramización multicanal (1). Cualquier valor que sea diferente a 1 indica binaural. La salida binaural se espacializa con HRTF para su uso con auriculares y la salida multicanal se espacializa con VBAP para su uso con sistemas de altavoces de sonido envolvente multicanal. Si usa la panoramización multicanal, no olvide seleccionar el modo del altavoz que coincida con la configuración del dispositivo, lo encontrará en **Configuración del proyecto** > **Audio**.

![SpeakerMode](media/SpeakerMode.png)

### <a name="tuning-source-parameters"></a>Ajuste de los parámetros de origen
Adjuntar el script **AcousticsAdjust** a un origen permite ajustar los parámetros de ese origen. Para adjuntar el script, haga clic en **Add Component** (Agregar componente) en la parte inferior del panel **Inspector** y navegue a **Scripts > Acoustics Adjust** (Scripts > Ajuste de acústica). El script tiene seis controles:

![AcousticsAdjust](media/AcousticsAdjust.png)

* **Enable Acoustics** (Habilitación de acústica): controla si se aplica acústica a este origen. Cuando está opción está desactivada, el origen se espacializa con HRTF o se panoramiza, pero no habrá ninguna acústica. Esto no significa ninguna obstrucción, oclusión y los parámetros de reverberación dinámicos, como el tiempo de decadencia y nivel. La reverberación se sigue aplicando con un nivel fijo un tiempo de decadencia.
* **Occlusion** (Oclusión): aplica un multiplicador al nivel de decibelios de oclusión calculado por el sistema acústico. Si este multiplicador es mayor que 1, la oclusión se exagera, mientras que los valores menores de 1 hacen que el efecto de oclusión sea más sutil, y un valor de 0 deshabilita la oclusión.
* **Transmission (dB)** (Transmisión (decibelios)): establece la atenuación (en decibelios) causada por la transmisión a través de la geometría. Establezca este control deslizante en su nivel más bajo para deshabilitar la transmisión. Acoustics espacializa el audio original inicial cuando se aproxima a la geometría de la escena (creación de portales). La transmisión proporciona una llegada adicional del sonido original que se espacializa en la dirección de la línea de visión. Tenga en cuenta que también se aplica la curva de atenuación de distancia para el origen.
* **Wetness (dB)** (Sonido con efectos (decibelios)): ajusta la potencia de reverberación, en decibelios, según la distancia con respecto al origen. Los valores positivos emiten un sonido con más reverberación, mientras que los valores negativos emiten un sonido más seco. Haga clic en el control de curva (la línea de color verde) para abrir el editor de curvas. Para modificar la curva, haga clic para agregar puntos y arrástrelos para formar la función que quiera. El eje x es la distancia desde el origen, y el eje y es el ajuste de la reverberación en decibelios. Para obtener más información sobre la edición de curvas, vea este [manual de Unity](https://docs.unity3d.com/Manual/EditingCurves.html). Para restablecer la curva al valor predeterminado, haga clic con el botón derecho en **Wetness** (Sonido con efectos) y seleccione **Reset** (Restablecer).
* **Decay Time Scale** (Escala de tiempo de decadencia): ajusta un multiplicador para el tiempo de decadencia. Por ejemplo, si el resultado de la elaboración especifica un tiempo de decadencia de 750 milisegundos, pero este valor se establece en 1,5, el tiempo de decadencia aplicado en el origen es de 1125 milisegundos.
* **Outdoorness** (En exteriores): un ajuste aditivo de la estimación del sistema acústico sobre cómo debe sonar la reverberación en un origen "en exteriores". Si este valor establece en 1, el origen siempre sonará como si estuviera totalmente en el exterior, mientras que si se establece en -1, sonará como si estuviera en el interior.

Los distintos orígenes pueden requerir configuraciones diferentes para lograr ciertos efectos estéticos o del juego. Los diálogos pueden ser un ejemplo. El oído humano está más a tono con la reverberación en el habla, mientras que el diálogo a menudo debe ser comprensible en el juego. Puede tenerlo en cuenta sin convertir al diálogo en no diegético si reduce el valor **Wetness** (Sonido con efectos), ajusta el parámetro **Perceptual Distance Warp** (Distorsión de la distancia perceptual) que se describe a continuación, agrega **Transmission** (Transmisión) para la propagación del aumento de sonido original a través de las paredes, y reduce el valor 1 de **Occlusion** (Oclusión) para que llegue más sonido a través de los portales.

Adjuntar el script **AcousticsAdjustExperimental** a un origen habilita parámetros de ajuste experimentales adicionales para ese origen. Para adjuntar el script, haga clic en **Add Component** (Agregar componente) en la parte inferior del panel **Inspector** y navegue a **Scripts > Acoustics Adjust Experimental** (Scripts > Ajuste de acústica experimental). Actualmente hay un control experimental:

![AcousticsAdjustExperimental](media/AcousticsAdjustExperimental.png)

* **Perceptual Distance Warp** (Distorsión de la distancia perceptual): aplica una distorsión exponencial a la distancia que se usa para calcular la proporción entre el sonido original y con efectos. El sistema acústico calcula los niveles de efecto en todo el espacio, que varían ligeramente con la distancia y proporcionan indicaciones de distancia perceptual. Los valores de distorsión superiores a 1 exageran este efecto al aumentar los niveles de reverberación relacionados con la distancia, y hacen que el sonido sea "distante". Los valores de distorsión inferiores a 1 hacen que el cambio de reverberación basado en la distancia sea más sutil, haciendo que el sonido sea más "presente".

