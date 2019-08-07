---
title: Creación de dependencias de desencadenadores de ventana de saltos de tamaño constante en Azure Data Factory | Microsoft Docs
description: Aprenda a crear dependencia en un desencadenador de ventana de saltos de tamaño constante en Azure Data Factory.
services: data-factory
documentationcenter: ''
author: djpmsft
manager: craigg
editor: ''
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/29/2019
ms.author: daperlov
ms.openlocfilehash: bf8534ce40460637141bea2b9582e63a2a5cd04a
ms.sourcegitcommit: 08d3a5827065d04a2dc62371e605d4d89cf6564f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68620636"
---
# <a name="create-a-tumbling-window-trigger-dependency"></a>Creación de una dependencia de un desencadenador de ventana de saltos de tamaño constante

En este artículo se exponen los pasos necesarios para crear una dependencia en un desencadenador de ventana de saltos de tamaño constante. Para obtener información general acerca de los desencadenadores de ventanas de saltos de tamaño constante, consulte [Cómo crear un desencadenador de ventana de saltos de tamaño constante](how-to-create-tumbling-window-trigger.md).

Con el fin de generar una cadena de dependencia y de asegurarse de que un desencadenador se ejecuta solo después de la correcta ejecución de otro desencadenador en la factoría de datos, utilice esta característica avanzada para crear una dependencia de ventana de saltos de tamaño constante.

## <a name="create-a-dependency-in-the-data-factory-ui"></a>Creación de una dependencia en la interfaz de usuario de Data Factory

Para crear una dependencia en un desencadenador, seleccione **Desencadenador > Avanzado > Nuevo**, y luego elija el desencadenador del que se va a depender con el desplazamiento y tamaño adecuados. Seleccione **Finalizar** y publique los cambios de la factoría de datos para que las dependencias surta efecto.

![Creación de dependencias](media/tumbling-window-trigger-dependency/tumbling-window-dependency01.png "Dependency Creation")

## <a name="tumbling-window-dependency-properties"></a>Propiedades de dependencia de ventana de saltos de tamaño constante

Un desencadenador de ventana de saltos de tamaño constante con una dependencia tiene las siguientes propiedades:

```json
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
        "runtimeState": <<Started/Stopped/Disabled - readonly>>,
        "typeProperties": {
            "frequency": <<Minute/Hour>>,
            "interval": <<int>>,
            "startTime": <<datetime>>,
            "endTime": <<datetime – optional>>,
            "delay": <<timespan – optional>>,
            "maxConcurrency": <<int>> (required, max allowed: 50),
            "retryPolicy": {
                "count": <<int - optional, default: 0>>,
                "intervalInSeconds": <<int>>,
            },
            "dependsOn": [
                {
                    "type": "TumblingWindowTriggerDependencyReference",
                    "size": <<timespan – optional>>,
                    "offset": <<timespan – optional>>,
                    "referenceTrigger": {
                        "referenceName": "MyTumblingWindowDependency1",
                        "type": "TriggerReference"
                    }
                },
                {
                    "type": "SelfDependencyTumblingWindowTriggerReference",
                    "size": <<timespan – optional>>,
                    "offset": <<timespan>>
                }
            ]
        }
    }
}
```

La tabla siguiente proporciona la lista de los atributos necesarios para definir una dependencia de ventana de saltos de tamaño constante.

| **Nombre de la propiedad** | **Descripción**  | **Tipo** | **Obligatorio** |
|---|---|---|---|
| Tipo  | Todos los desencadenadores de ventana de saltos de tamaño constante existentes se muestran en este menú desplegable. Elija el desencadenador del que se va a depender.  | TumblingWindowTriggerDependencyReference o SelfDependencyTumblingWindowTriggerReference. | Sí |
| offset | Desplazamiento del desencadenador de dependencia. Proporcione un valor en formato de intervalo de tiempo; admite desplazamientos negativos y positivos. Esta propiedad es obligatoria si el desencadenador depende de sí mismo; en todos los demás casos, es opcional. La autodependencia debe tener siempre un valor de desplazamiento negativo. Si no se especifica ningún valor, la ventana es igual al desencadenador. | TimeSpan<br/>(hh:mm:ss) | Autodependencia: Sí<br/>Otros: Sin |
| size | Tamaño de la ventana de saltos de tamaño constante de dependencia. Proporcione un valor de intervalo de tiempo positivo. Esta propiedad es opcional. | TimeSpan<br/>(hh:mm:ss) | Sin  |

> [!NOTE]
> Un desencadenador de ventana de saltos de tamaño constante puede depender de un máximo de otros dos desencadenadores.

## <a name="tumbling-window-self-dependency-properties"></a>Propiedades de la autodependencia de ventana de saltos de tamaño constante

En los escenarios en los que el desencadenador no debe continuar a la siguiente ventana hasta que se haya completado correctamente la ventana anterior, cree una autodependencia. Un desencadenador de dependencia automática que depende del éxito de las ejecuciones anteriores durante la hora anterior tendrá las propiedades siguientes:

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
## <a name="usage-scenarios-and-examples"></a>Escenarios de uso y ejemplos

A continuación se muestran las ilustraciones de los escenarios y el uso de las propiedades de dependencia de ventana de saltos de tamaño constante.

### <a name="dependency-offset"></a>Desplazamiento de la dependencia

![Ejemplo de desplazamiento](media/tumbling-window-trigger-dependency/tumbling-window-dependency02.png "Offset Example")

### <a name="dependency-size"></a>Tamaño de la dependencia

![Ejemplo de tamaño](media/tumbling-window-trigger-dependency/tumbling-window-dependency03.png "Size example")

### <a name="self-dependency"></a>Autodependencia

![Dependencia automática](media/tumbling-window-trigger-dependency/tumbling-window-dependency04.png "Self-dependency")

### <a name="dependency-on-another-tumbling-window-trigger"></a>Dependencia de otro desencadenador de ventana de saltos de tamaño constante

Un trabajo de procesamiento de datos de telemetría diario que dependa de otro trabajo diario que agregue la salida de los últimos siete días de salida y genere secuencias de ventana gradual de siete días:

![Ejemplo de dependencia](media/tumbling-window-trigger-dependency/tumbling-window-dependency05.png "Dependency example")

### <a name="dependency-on-itself"></a>Dependencia de sí mismo

Un trabajo diario sin interrupciones en los flujos de salida del trabajo:

![Ejemplo de dependencia automática](media/tumbling-window-trigger-dependency/tumbling-window-dependency06.png "Self-dependency example")

## <a name="monitor-dependencies"></a>Supervisión de dependencias

Puede supervisar la cadena de dependencias y las ventanas correspondientes desde la página de supervisión de ejecución del desencadenador. Vaya a  **Supervisión > Trigger Runs (Ejecuciones de desencadenador)** .

![Supervisión de las ejecuciones del desencadenador](media/tumbling-window-trigger-dependency/tumbling-window-dependency07.png "Monitor trigger runs")

Haga clic en el icono de acción para ver todas las ejecuciones del desencadenador dependiente de la ventana seleccionada.

![Supervisión de dependencias](media/tumbling-window-trigger-dependency/tumbling-window-dependency08.png "Monitor dependencies")

En el ejemplo anterior, un desencadenador diario depende de un desencadenador por horas sin ninguna ventana definida y un desplazamiento de 3 horas. Como resultado, el desencadenador se ejecuta después de 24 ejecuciones correctas de la dependencia.

## <a name="next-steps"></a>Pasos siguientes

Consulte [Cómo crear un desencadenador de ventana de saltos de tamaño constante](how-to-create-tumbling-window-trigger.md).