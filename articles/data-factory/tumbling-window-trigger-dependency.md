---
title: Creación de dependencias de desencadenadores de ventana de saltos de tamaño constante en Azure Data Factory | Microsoft Docs
description: Aprenda a crear dependencia en un desencadenador de ventana de saltos de tamaño constante en Azure Data Factory.
services: data-factory
documentationcenter: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/11/2018
author: nabhishek
ms.author: abnarain
manager: craigg
ms.openlocfilehash: f3a547f5b953262d7263d1be0897161cf4091a1d
ms.sourcegitcommit: 30a0007f8e584692fe03c0023fe0337f842a7070
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/07/2019
ms.locfileid: "57574396"
---
# <a name="create-a-tumbling-window-trigger-dependency"></a>Creación de una dependencia de un desencadenador de ventana de saltos de tamaño constante

En este artículo se exponen los pasos necesarios para crear una dependencia en un desencadenador de ventana de saltos de tamaño constante. Para obtener información general acerca de los desencadenadores de ventanas de saltos de tamaño constante, consulte [Cómo crear un desencadenador de ventana de saltos de tamaño constante](how-to-create-tumbling-window-trigger.md).

Con el fin de generar una cadena de dependencia y de asegurarse de que un desencadenador se ejecuta solo después de la correcta ejecución de otro desencadenador en la factoría de datos, utilice esta característica avanzada para crear una dependencia de ventana de saltos de tamaño constante.

## <a name="create-a-dependency-in-the-data-factory-ui"></a>Creación de una dependencia en la interfaz de usuario de Data Factory

Para crear una dependencia en un desencadenador, seleccione **Desencadenador > Avanzado > Nuevo**, y luego elija el desencadenador del que se va a depender con el desplazamiento y tamaño adecuados. Seleccione **Finalizar** y publique los cambios de la factoría de datos para que las dependencias surta efecto.

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency01.png)

## <a name="tumbling-window-dependency-properties"></a>Propiedades de dependencia de ventana de saltos de tamaño constante

Una dependencia de desencadenador de ventana de saltos de tamaño constante tiene las siguientes propiedades:

```json
{
    "name": "DemoTrigger",
    "properties": {
        "runtimeState": "Started",
        "pipeline": {
            "pipelineReference": {
                "referenceName": "Demo",
                "type": "PipelineReference"
            }
        },
        "type": "TumblingWindowTrigger",
        "typeProperties": {
            "frequency": "Hour",
            "interval": 1,
            "startTime": "2018-10-04T00:00:00.000Z",
            "delay": "00:00:00",
            "maxConcurrency": 50,
            "retryPolicy": {
                "intervalInSeconds": 30
            },
            "dependsOn": [
                {
                    "type": "TumblingWindowTriggerDependencyReference",
                    "size": "-02:00:00",
                    "offset": "-01:00:00",
                    "referenceTrigger": {
                        "referenceName": "DemoDependency1",
                        "type": "TriggerReference"
                    }
                }
            ]
        }
    }
}
```

La tabla siguiente proporciona la lista de los atributos necesarios para definir una dependencia de ventana de saltos de tamaño constante.

| **Nombre de la propiedad** | **Descripción**  | **Tipo** | **Obligatorio** |
|---|---|---|---|
| Desencadenador  | Todos los desencadenadores de ventana de saltos de tamaño constante existentes se muestran en este menú desplegable. Elija el desencadenador del que se va a depender.  | TumblingWindowTrigger | Sí |
| Offset | Desplazamiento del desencadenador de dependencia. Proporcione un valor en formato de intervalo de tiempo; admite desplazamientos negativos y positivos. Este parámetro es obligatorio si el desencadenador depende de sí mismo; en todos los demás casos, es opcional. La autodependencia debe tener siempre un valor de desplazamiento negativo. | TimeSpan | Autodependencia: Sí Otros: Sin  |
| Tamaño de ventana | Tamaño de la ventana de saltos de tamaño constante de dependencia. Proporcione un valor en el formato de intervalo de tiempo. Este parámetro es opcional. | TimeSpan | Sin   |
|||||

## <a name="tumbling-window-self-dependency-properties"></a>Propiedades de la autodependencia de ventana de saltos de tamaño constante

En los escenarios en los que el desencadenador no debe continuar a la siguiente ventana hasta que se haya completado correctamente la ventana anterior, cree una autodependencia. El desencadenador de autodependencia tendrá las propiedades siguientes:

```json
{
    "name": "DemoSelfDependency",
    "properties": {
        "runtimeState": "Started",
        "pipeline": {
            "pipelineReference": {
                "referenceName": "Demo",
                "type": "PipelineReference"
            }
        },
        "type": "TumblingWindowTrigger",
        "typeProperties": {
            "frequency": "Hour",
            "interval": 1,
            "startTime": "2018-10-04T00:00:00Z",
            "delay": "00:01:00",
            "maxConcurrency": 50,
            "retryPolicy": {
                "intervalInSeconds": 30
            },
            "dependsOn": [
                {
                    "type": "SelfDependencyTumblingWindowTriggerReference",
                    "size": "01:00:00",
                    "offset": "-01:00:00"
                }
            ]
        }
    }
}
```

A continuación se muestran las ilustraciones de los escenarios y el uso de las propiedades de dependencia de ventana de saltos de tamaño constante.

## <a name="dependency-offset"></a>Desplazamiento de la dependencia

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency02.png)

## <a name="dependency-size"></a>Tamaño de la dependencia

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency03.png)

## <a name="self-dependency"></a>Autodependencia

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency04.png)

## <a name="usage-scenarios"></a>Escenarios de uso

### <a name="dependency-on-another-tumbling-window-trigger"></a>Dependencia de otro desencadenador de ventana de saltos de tamaño constante

Por ejemplo, un trabajo de procesamiento de datos de telemetría diario que dependa de otro trabajo diario que agregue la salida de los últimos siete días de salida y genere secuencias de ventana gradual de siete días:

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency05.png)

### <a name="dependency-on-itself"></a>Dependencia de sí mismo

Un trabajo diario sin interrupciones en los flujos de salida del trabajo:

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency06.png)

## <a name="monitor-dependencies"></a>Supervisión de dependencias

Puede supervisar la cadena de dependencias y las ventanas correspondientes desde la página de supervisión de ejecución del desencadenador. Vaya a  **Supervisión > Trigger Runs (Ejecuciones de desencadenador)**.

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency07.png)

Haga clic en la lupa para ver todas las ejecuciones del desencadenador dependiente de la ventana seleccionada.

![](media/tumbling-window-trigger-dependency/tumbling-window-dependency08.png)

## <a name="next-steps"></a>Pasos siguientes

Consulte [Cómo crear un desencadenador de ventana de saltos de tamaño constante](how-to-create-tumbling-window-trigger.md).