---
title: Movimiento de nodos en Azure Data Factory Data Flow
description: Cómo se mueven los nodos en un diagrama de Data Flow de asignación de Azure Data Factory
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/04/2018
ms.openlocfilehash: 853fff955a953052b94ad4e7e1dd55371069c30b
ms.sourcegitcommit: 11265f4ff9f8e727a0cbf2af20a8057f5923ccda
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/08/2019
ms.locfileid: "72030192"
---
# <a name="mapping-data-flow-move-nodes"></a>Nodos de movimiento de Mapping Data Flow



![Agregar opciones de transformación](media/data-flow/agghead.png "encabezado del agregador")

La superficie de diseño de flujo de datos de Azure Data Factory Data Flow es una superficie de "construcción" en la que los flujos de datos se crean de arriba a abajo y de izquierda a derecha. Hay un cuadro de herramientas asociado a cada transformación con el símbolo de más (+). Concéntrese en su lógica de negocios, en lugar de en conectar los nodos a través de los extremos de un entorno de DAG de forma libre.

Por consiguiente, sin un paradigma de arrastrar y colocar, la forma de "mover" un nodo de transformación es cambiar el flujo de entrada. En su lugar, para mover las transformaciones cambiará el "flujo de entrada".

## <a name="streams-of-data-inside-of-data-flow"></a>Flujos de datos dentro de Data Flow

En Azure Data Factory Data Flow, las secuencias representan el flujo de datos. En el panel de configuración de la transformación, verá el campo "Incoming Stream" (Secuencia de entrada). En se indica cuál es el flujo de datos de entrada que alimenta la transformación. Puede cambiar la ubicación física del nodo de la transformación en el gráfico. Para ello, solo es preciso hacer clic en el nombre del flujo de entrada y seleccionar otro flujo de datos. A continuación, tanto la transformación actual como todas las transformaciones posteriores de esa secuencia, se moverán a la nueva ubicación.

Si va a mover una transformación con una o varias transformaciones posteriores, la nueva ubicación del flujo de datos se conectará mediante una nueva rama.

Si no hay transformaciones posteriores al nodo que ha seleccionado, la primera será la única que se moverá a la nueva ubicación.

## <a name="next-steps"></a>Pasos siguientes

Después de completar el diseño de Data Flow, active el botón de depuración y pruébelo en modo de depuración, ya sea directamente en el [diseñador del flujo de datos](concepts-data-flow-debug-mode.md) o en la [depuración de canalización](control-flow-execute-data-flow-activity.md).
