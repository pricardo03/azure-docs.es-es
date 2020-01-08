---
title: Depuración y solución de problemas de ParallelRunStep
titleSuffix: Azure Machine Learning
description: Depure y resuelva los problemas de las canalizaciones de aprendizaje automático con el SDK de Azure Machine Learning para Python. Aprenda sobre los errores comunes en el desarrollo de las canalizaciones, y consulte sugerencias que le ayudarán a depurar los scripts antes y durante la ejecución remota.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: trbye, jmartens, larryfr, vaidyas
ms.author: trmccorm
author: tmccrmck
ms.date: 11/21/2019
ms.openlocfilehash: 20e88da6721a6715a7f31f5c423e305625528383
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/28/2019
ms.locfileid: "75535348"
---
# <a name="debug-and-troubleshoot-using-parallelrun"></a>Depuración y solución de problemas con ParallelRun
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este artículo aprenderá a depurar y resolver los problemas de la clase [ParallelRunStep](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps.parallel_run_step.parallelrunstep?view=azure-ml-py) con el [SDK de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py).

## <a name="testing-scripts-locally"></a>Prueba de scripts de forma local

Consulte la [sección Prueba de scripts de forma local](how-to-debug-pipelines.md#testing-scripts-locally) en el artículo sobre canalizaciones de aprendizaje automático. La instancia de ParallelRunStep se ejecuta como un paso en las canalizaciones de Machine Learning, por lo que la misma respuesta se aplica en ambos casos.

## <a name="debugging-scripts-from-remote-context"></a>Depuración de scripts desde un contexto remoto

La transición de la depuración de un script de puntuación de forma local, a la depuración de un script de puntuación en una canalización real puede resultar difícil. Para información sobre cómo buscar los registros en el portal, consulte la [sección de canalizaciones de aprendizaje automático en la depuración de scripts desde un contexto remoto](how-to-debug-pipelines.md#debugging-scripts-from-remote-context). La información de esa sección también se aplica a una ejecución de inferencia de lotes.

Por ejemplo, el archivo de registro `70_driver_log.txt` también contiene: 

* Todas las instrucciones impresas durante la ejecución del script.
* El seguimiento de la pila del script.

Debido a la naturaleza distribuida de los trabajos de inferencia de lotes, hay registros de distintos orígenes. Sin embargo, se crean dos archivos consolidados que proporcionan información de alto nivel:

- `~/logs/overview.txt`: Este archivo proporciona información de alto nivel sobre el número de minilotes (también conocidos como tareas) que se han creado hasta el momento y el número de minilotes que se han procesado hasta ahora. En este extremo, se muestra el resultado del trabajo. Si se produjo un error en el trabajo, se mostrará el mensaje de error y dónde se debe iniciar la solución de problemas.

- `~/logs/master.txt`: Este archivo proporciona la vista del nodo principal (también conocido como orquestador) del trabajo en ejecución. Incluye la creación de tareas,la supervisión del progreso y el resultado de la ejecución.

Cuando necesite comprender en detalle cómo ejecuta cada nodo el script de puntuación, examine los registros de proceso individuales para cada nodo. Los registros de proceso se pueden encontrar en la carpeta `worker`, agrupados por nodos de trabajo:

- `~/logs/worker/<ip_address>/Process-*.txt`: Este archivo proporciona información detallada sobre cada uno de los minilotes a medida que un trabajo lo recoge o lo completa. Para cada minilote, este archivo incluye:

    - La dirección IP y el PID del proceso de trabajo. 
    - El número total de elementos y el número de elementos procesados correctamente. 
    - Hora de inicio y de finalización en tiempo de reloj (`start1` y `end1`). 
    - Hora de inicio y de finalización en tiempo de procesador utilizado (`start2` y `end2`). 

También puede encontrar información sobre el uso de recursos de los procesos para cada trabajo. Esta información está en formato CSV y se encuentra en `~/logs/performance/<ip_address>/`. Por ejemplo, al comprobar la utilización de recursos, examine los siguientes archivos:

- `process_resource_monitor_<ip>_<pid>.csv`: Uso de recursos de proceso por trabajo. 
- `sys_resource_monitor_<ip>.csv`: Por registro de nodo.

### <a name="how-do-i-log-from-my-user-script-from-a-remote-context"></a>¿Cómo me puedo registrar desde mi script de usuario en un contexto remoto?
Puede configurar un registrador con los pasos siguientes para que los registros se muestren en la carpeta **registros/usuarios** en el portal:
1. Guarde la primera sección del código a continuación en el archivo entry_script_helper.py, y coloque el archivo en la misma carpeta que el script de entrada. Esta clase obtiene la ruta de acceso dentro de AmlCompute. Para una prueba local, puede cambiar `get_working_dir()` para devolver una carpeta local.
2. Configure un registrador en el método `init()`, y una vez creado, úselo. La segunda sección de código a continuación es un ejemplo. 

**entry_script_helper.py:**
```python
"""
This module provides helper features for entry script.
This file should be in Python search paths or in the same folder as the entry script.
"""
import os
import socket
import logging
import time
from multiprocessing import current_process
from azureml.core import Run


class EntryScriptHelper:
    """ A helper to provide common features for entry script."""

    LOG_CONFIGED = False

    def get_logger(self, name="EntryScript"):
        """ Return a logger.
            The logger will write to the 'users' folder and show up in azure portal.
        """
        return logging.getLogger(name)

    def config(self, name="EntryScript", level="INFO"):
        """ Config a logger. This should be called in init() in score module.
            Config the logger one time if not configed.
            The logger will write to the 'users' folder and show up in azure portal.
        """
        logger = logging.getLogger(name)

        formatter = logging.Formatter(
            "%(asctime)s|%(name)s|%(levelname)s|%(process)d|%(thread)d|%(funcName)s()|%(message)s"
        )
        formatter.converter = time.gmtime

        logger.setLevel(level)

        handler = logging.FileHandler(self.get_log_file_path())
        handler.setLevel(level)
        handler.setFormatter(formatter)
        logger.addHandler(handler)

        return logger

    def get_log_file_path(self):
        """ Get the log file path for users.
            Each process has its own log file, so there is not race issue among multiple processes.
        """
        ip_address = socket.gethostbyname(socket.gethostname())
        log_dir = os.path.join(self.get_log_dir(), "user", ip_address)
        os.makedirs(log_dir, exist_ok=True)
        return os.path.join(log_dir, f"{current_process().name}.txt")

    def get_log_dir(self):
        """ Return the folder for logs.
            Files and folders in it will be uploaded and show up in run detail page in the azure portal.
        """
        log_dir = os.path.join(self.get_working_dir(), "logs")
        os.makedirs(log_dir, exist_ok=True)
        return log_dir

    def get_working_dir(self):
        """ Return the working directory."""
        return os.path.join(os.environ.get("AZ_BATCHAI_INPUT_AZUREML", ""), self.get_run().id)

    def get_temp_dir(self):
        """ Return local temp directory."""
        local_temp_dir = os.path.join(
            os.environ.get("AZ_BATCHAI_JOB_TEMP", ""), "azureml-bi", str(os.getpid())
        )
        os.makedirs(local_temp_dir, exist_ok=True)
        return local_temp_dir

    def get_run(self):
        """ Return the Run from the context."""
        return Run.get_context(allow_offline=False)

```

**Uso del registrador por un script de entrada de ejemplo:**
```python
"""
This is a sample scoring module.

This module provides a sample which passes the input back without any change.
"""
import os
import logging
from entry_script_helper import EntryScriptHelper

LOG_NAME = "score_file_list"


def init():
    """ Init """
    EntryScriptHelper().config(LOG_NAME)
    logger = logging.getLogger(LOG_NAME)
    output_folder = os.path.join(os.environ.get("AZ_BATCHAI_INPUT_AZUREML", ""), "temp/output")
    logger.info(f"{__file__}.output_folder:{output_folder}")
    logger.info("init()")
    os.makedirs(output_folder, exist_ok=True)


def run(mini_batch):
    """ Accept and return the list back."""
    logger = logging.getLogger(LOG_NAME)
    logger.info(f"{__file__}, run({mini_batch})")

    output_folder = os.path.join(os.environ.get("AZ_BATCHAI_INPUT_AZUREML", ""), "temp/output")
    for file_name in mini_batch:
        with open(file_name, "r") as file:
            lines = file.readlines()
        base_name = os.path.basename(file_name)
        name = os.path.join(output_folder, base_name)
        logger.info(f"{__file__}: {name}")
        with open(name, "w") as file:
            file.write(f"ouput file {name} from {__file__}:\n")
            for line in lines:
                file.write(line)

    return mini_batch
```

## <a name="next-steps"></a>Pasos siguientes

* Consulte la referencia del SDK para obtener ayuda con el paquete [azureml-contrib-pipeline-step](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps?view=azure-ml-py) y la [documentación](https://docs.microsoft.com/python/api/azureml-contrib-pipeline-steps/azureml.contrib.pipeline.steps.parallelrunstep?view=azure-ml-py) para la clase ParallelRunStep.

* Consulte el [tutorial avanzado](tutorial-pipeline-batch-scoring-classification.md) sobre el uso de canalizaciones para la puntuación por lotes.
