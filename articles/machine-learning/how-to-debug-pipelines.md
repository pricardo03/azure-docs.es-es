---
title: Depuración y solución de problemas de canalizaciones de aprendizaje automático
titleSuffix: Azure Machine Learning
description: Depure y resuelva los problemas de las canalizaciones de aprendizaje automático con el SDK de Azure Machine Learning para Python. Obtenga información sobre los errores comunes en el desarrollo de las canalizaciones y sugerencias que le ayudarán a depurar los scripts antes y durante la ejecución remota.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: likebupt
ms.author: keli19
ms.date: 12/12/2019
ms.openlocfilehash: 991f7ebf51be5f805a8b12fa0af0fefeff0ef582
ms.sourcegitcommit: a9b1f7d5111cb07e3462973eb607ff1e512bc407
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76309564"
---
# <a name="debug-and-troubleshoot-machine-learning-pipelines"></a>Depuración y solución de problemas de canalizaciones de aprendizaje automático
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este artículo se obtiene información sobre cómo depurar y resolver los problemas de las [canalizaciones de aprendizaje automático](concept-ml-pipelines.md) en el [SDK de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) y el [diseñador de Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/concept-designer).


## <a name="debug-and-troubleshoot-in-the-azure-machine-learning-sdk"></a>Depuración y solución de problemas en el SDK de Azure Machine Learning
En la siguientes secciones se da una información general sobre los errores comunes durante la creación de canalizaciones, y sobre las diferentes estrategias para depurar el código que se ejecuta en una canalización. Cuando aparezcan problemas y una canalización no se ejecute según lo esperado, utilice las sugerencias que se ofrecen a continuación. 
### <a name="testing-scripts-locally"></a>Prueba de scripts de forma local

Uno de los errores más comunes en una canalización es que uno de los scripts adjuntos (script de limpieza de datos, script de puntuación, etc.) no se ejecute según lo previsto, o que contenga errores en tiempo de ejecución en el contexto de proceso remoto que son difíciles de depurar en el área de trabajo en Azure Machine Learning Studio. 

Las canalizaciones no se pueden ejecutar localmente, pero la ejecución de los scripts de forma aislada en la máquina local le permite depurar más rápido ya que no tiene que esperar al proceso de compilación de proceso y de entorno. Para ello, se necesita realizar algo de trabajo de desarrollo:

* Si los datos están en un almacén de datos en la nube, tendrá que descargarlos y ponerlos a disposición del script. El uso de una pequeña muestra de los datos es una buena forma de reducir el tiempo de ejecución y obtener rápidamente comentarios sobre el comportamiento del script
* Si intenta simular un paso de canalización intermedio, es posible que tenga que compilar manualmente los tipos de objeto que el script específico espera del paso anterior
* También tendrá que definir su propio entorno y replicar las dependencias definidas en el entorno de proceso remoto

Una vez que tenga una configuración de script que se ejecute en su entorno local, es mucho más fácil realizar tareas de depuración como:

* Incorporación de una configuración de depuración personalizada
* Pausa de la ejecución e inspección del estado del objeto
* Detección de errores lógicos o de tipo que no se expondrían hasta el tiempo de ejecución

> [!TIP] 
> Una vez que pueda comprobar que el script se ejecuta según lo previsto, un buen paso posterior es ejecutar el script en una canalización de un solo paso antes de intentar ejecutarlo en una canalización con varios pasos.

### <a name="debugging-scripts-from-remote-context"></a>Depuración de scripts desde un contexto remoto

La prueba de scripts de forma local es una excelente manera de depurar fragmentos de código principales y lógica compleja antes de empezar a crear una canalización, de todas formas, es probable que en algún momento tenga que depurar scripts durante la propia ejecución de canalización real, especialmente al diagnosticar el comportamiento que se produce durante la interacción entre los pasos de canalización. Se recomienda el uso generoso de las instrucciones `print()` en los scripts de paso, con el fin de ver el estado del objeto y los valores esperados durante la ejecución remota, de manera similar a como se depuraría el código de JavaScript.

El archivo de registro `70_driver_log.txt` contiene: 

* Todas las instrucciones impresas durante la ejecución del script
* El seguimiento de la pila para el script 

Para encontrar este y otros archivos de registro en el portal, primero haga clic en la canalización en el área de trabajo.

![Página de lista de ejecuciones de canalización](./media/how-to-debug-pipelines/pipelinerun-01.png)

Vaya a la página de detalles de ejecución de la canalización.

![Página de detalles de ejecución de canalización](./media/how-to-debug-pipelines/pipelinerun-02.png)

Haga clic en el identificador de ejecución del paso específico. Vaya a la pestaña **Registros**. Otros registros incluyen información sobre el proceso de generación de imágenes de entorno y scripts de preparación de pasos.

![Pestaña de registro de la página de detalles de ejecución de canalización](./media/how-to-debug-pipelines/pipelinerun-03.png)

> [!TIP]
> Las ejecuciones de las *canalizaciones publicadas* están en la pestaña **Puntos de conexión** del área de trabajo del portal. Las ejecuciones de las *canalizaciones no publicadas* se encuentran en **Experimentos** o **Canalizaciones**.

### <a name="troubleshooting-tips"></a>Sugerencias de solución de problemas

La tabla siguiente contiene problemas comunes que se pueden producir durante el desarrollo de canalizaciones y posibles soluciones a los mismo.

| Problema | Posible solución |
|--|--|
| No se pueden pasar datos al directorio `PipelineData` | Asegúrese de haber creado un directorio en el script que se corresponda con el lugar en el que la canalización espera los datos de salida del paso. En la mayoría de los casos, un argumento de entrada definirá el directorio de salida, a continuación se crea el directorio de forma explícita. Use `os.makedirs(args.output_dir, exist_ok=True)` para crear el directorio de salida. Si necesita un ejemplo de script de puntuación que muestra este patrón de diseño, acuda al [tutorial](tutorial-pipeline-batch-scoring-classification.md#write-a-scoring-script) correspondiente. |
| Errores de dependencia | Si ha desarrollado y probado los scripts localmente, pero se encuentra problemas de dependencia cuando la ejecución se realiza en un proceso remoto en la canalización, asegúrese de que las dependencias y las versiones del entorno de proceso coinciden con el entorno de prueba. |
| Errores ambiguos con destinos de proceso | Eliminar y volver a crear los destinos de proceso puede resolver determinados problemas relacionados con ellos. |
| La canalización no reutiliza los pasos | La reutilización de pasos está habilitada de forma predeterminada, asegúrese de que no la ha deshabilitado en un paso de la canalización. Si la reutilización está deshabilitada, el parámetro `allow_reuse` del paso se establecerá en `False`. |
| La canalización se está volviendo a ejecutar innecesariamente | Para asegurarse de que los pasos solo se vuelven a ejecutar cuando sus datos o scripts subyacentes cambian, desacople los directorios de cada paso. Si usa el mismo directorio de origen para varios pasos, puede experimentar la repetición innecesaria de ejecuciones. Use el parámetro `source_directory` en un objeto de paso de canalización para apuntar a su directorio aislado para ese paso, y asegúrese de que no está usando la misma ruta de acceso `source_directory` para varios pasos. |

## <a name="debug-and-troubleshoot-in-azure-machine-learning-designer"></a>Depuración y solución de problemas en el diseñador de Azure Machine Learning

En esta sección se proporciona información general sobre cómo solucionar problemas de las canalizaciones en el diseñador.
En el caso de las canalizaciones creadas en el diseñador, puede encontrar los **archivos de registro** en la página de creación o en la página de detalles de ejecución de canalización.

### <a name="access-logs-from-the-authoring-page"></a>Acceso a los registros desde la página de creación

Cuando envía una ejecución de canalización y permanece en la página de creación, puede encontrar los archivos de registro generados para cada módulo.

1. Seleccione cualquier módulo en el lienzo de creación.
1. En el panel Propiedades, vaya a la pestaña **Registros**.
1. Seleccione el archivo de registro `70_driver_log.txt`.

    ![Creación de registros del módulo de páginas](./media/how-to-debug-pipelines/pipelinerun-05.png)

### <a name="access-logs-from-pipeline-runs"></a>Acceso a registros desde las ejecuciones de canalización

También puede buscar los archivos de registro de ejecuciones específicas en la página de detalles de ejecución de canalización en las secciones **Canalizaciones** o **Experimentos**.

1. Seleccione una ejecución de canalización creada en el diseñador.
    ![Página de ejecución de la canalización](./media/how-to-debug-pipelines/pipelinerun-04.png)
1. Seleccione cualquier módulo en el panel de versión preliminar previa.
1. En el panel Propiedades, vaya a la pestaña **Registros**.
1. Seleccione el archivo de registro `70_driver_log.txt`.

## <a name="next-steps"></a>Pasos siguientes

* Consulte la referencia del SDK para encontrar ayuda para los paquetes [azureml-pipelines-core](https://docs.microsoft.com/python/api/azureml-pipeline-core/?view=azure-ml-py) y [azureml-pipelines-steps](https://docs.microsoft.com/python/api/azureml-pipeline-steps/?view=azure-ml-py).

* Vea la lista de [excepciones y códigos de error del diseñador](algorithm-module-reference/designer-error-codes.md).
