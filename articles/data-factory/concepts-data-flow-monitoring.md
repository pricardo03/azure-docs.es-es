---
title: Supervisión visual de flujos de datos de asignación de Azure Data Factory
description: Aprenda a supervisar visualmente los flujos de datos de Azure Data Factory
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/01/2019
ms.openlocfilehash: 90aa6261aebb9d1f7da89c101854bad8061dd6ff
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/23/2019
ms.locfileid: "56737865"
---
# <a name="monitor-data-flows"></a>Supervisión de flujos de datos

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Después de haber completado la compilación y depuración del flujo de datos, desea programarlo para ejecutarse según una programación en el contexto de una canalización. Puede programar la canalización de Azure Data Factory mediante desencadenadores. También puede usar la opción Desencadenar ahora desde el generador de canalización de Azure Data Factory para ejecutar una sola ejecución con el fin de probar el flujo de datos dentro del contexto de canalización.

Cuando se ejecuta la canalización, podrá supervisar la canalización y todas las actividades contenidas en la canalización, incluida la actividad de Data Flow. Haga clic en el icono de supervisión en el panel izquierdo de la interfaz de usuario de Azure Data Factory. Se muestra una pantalla similar a la siguiente. Los iconos resaltados permitirán profundizar en las actividades de la canalización, incluidas las actividades de Data Flow.

<img src="media/data-flow/mon001.png" width="800">

Verá estadísticas en este nivel, así como los tiempos de ejecución y estado. El identificador de ejecución en el nivel de actividad es diferente al del nivel de canalización. El identificador de ejecución en el nivel anterior es para la canalización. Al hacer clic en icono de las gafas, verá detalles de la ejecución del flujo de datos.

<img src="media/data-flow/mon002.png" width="800">

En la vista de supervisión del nodo gráfico, verá una versión simplificada de solo lectura del gráfico de flujo de datos.

<img src="media/data-flow/mon003.png" width="800">

## <a name="view-data-flow-execution-plans"></a>Visualización de planes de ejecución de Data Flow

Cuando se ejecuta el flujo de datos en Databricks, Azure Data Factory determina las rutas de acceso de código óptimo según la integridad del flujo de datos. Además, las rutas de ejecución pueden producirse en distintos nodos de escalabilidad horizontal y particiones de datos. Por lo tanto, el gráfico de supervisión representa el diseño del flujo, teniendo en cuenta la ruta de acceso de ejecución de las transformaciones. Al hacer clic en los nodos individuales, verá "agrupaciones" que representan el código que se ejecutó juntos en el clúster. Los intervalos y recuentos que ve representan esos grupos en lugar de los pasos individuales del diseño.

<img src="media/data-flow/mon004.png" width="800"> 

* Al hacer clic en el espacio abierto de la ventana de supervisión, las estadísticas del panel inferior mostrarán los recuentos de filas y el tiempo de cada receptor y las transformaciones que dieron lugar a los datos de receptor de linaje de transformación.

* Cuando se seleccionan transformaciones individuales, recibirá más comentarios en el panel derecho que muestra estadísticas de partición, recuentos de columna, sesgo (con qué uniformidad se distribuyen los datos entre particiones) y curtosis (cuántos picos tienen los datos).

* Al hacer clic en el receptor en la vista del nodo, verá el linaje de columna. Hay tres métodos diferentes que las columnas acumulan a lo largo de su flujo de datos para colocarse en el receptor. Son las siguientes:

  * Calculado: Use la columna para el procesamiento condicional o dentro de una expresión en el flujo de datos, pero no la coloque en el receptor.
  * Derivado: La columna es una columna nueva que generó en el flujo, es decir, no estaba presente en el origen.
  * Asignada: La columna se origina desde el origen y la asigna a un campo de receptor.
  
## <a name="monitor-icons"></a>Iconos de supervisión

Este icono significa que los datos de transformación se almacenaron en caché en el clúster, por lo que los intervalos y la ruta de acceso de ejecución se han tenido en cuenta:

<img src="media/data-flow/mon005.png" width="800"> 

También verá los iconos de círculo verde en la transformación. Representan un recuento del número de receptores en los que fluyen los datos.
