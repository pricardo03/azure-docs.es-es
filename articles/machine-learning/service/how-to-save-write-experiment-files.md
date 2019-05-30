---
title: Evitar las limitaciones de almacenamiento y experimentar latencia con directorios de entrada y salida
description: En este artículo, obtendrá información sobre dónde guardar los archivos de entrada del experimento y donde se escribe archivos de salida para evitar errores de limitación de almacenamiento y experimentar latencia.
services: machine-learning
author: rastala
ms.author: roastala
manager: danielsc
ms.reviewer: jmartens, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 05/28/2019
ms.openlocfilehash: 28d8c47db8ea9c8a51bc8e9deb0a689eb0b20177
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66392901"
---
# <a name="where-to-save-and-write-files-for-azure-machine-learning-experiments"></a>Donde guardar y escribir archivos de experimentos de Azure Machine Learning

En este artículo, obtendrá información sobre dónde guardar los archivos de entrada y dónde escribir archivos de salida de los experimentos para evitar el almacenamiento de limitan los errores y experimentar latencia.

Al iniciar el aprendizaje se ejecuta en un [destino de proceso](how-to-set-up-training-targets.md), están aisladas entornos exteriores. El propósito de este diseño es garantizar la reproducibilidad y la portabilidad del experimento. Si ejecuta dos veces la misma secuencia de comandos, en el mismo o en otro destino de proceso, recibirá los mismos resultados. Con este diseño, puede tratar destinos de proceso como recursos de cálculo sin estado, cada uno con y sin afinidad con los trabajos que se ejecutan una vez finalizados.

## <a name="where-to-save-input-files"></a>Dónde guardar los archivos de entrada

Antes de poder iniciar un experimento en el equipo local o en un destino de proceso, debe asegurarse de que los archivos necesarios están disponibles para ese destino de proceso, como archivos de dependencia y los archivos de datos que necesita ejecutar su código.

Azure Machine Learning ejecuta scripts de entrenamiento copiando la carpeta de la secuencia de comandos completa en el contexto de proceso de destino y, a continuación, toma una instantánea. El límite de almacenamiento de instantáneas del experimento es 300 MB o archivos de 2000.

Por este motivo, se recomienda:

* **Almacenar los archivos en un equipo con Azure Machine Learning [datastore](https://docs.microsoft.com/python/api/azureml-core/azureml.data?view=azure-ml-py).** Esto evita problemas de latencia del experimento y tiene las ventajas de acceso a datos desde un destino de proceso remoto, lo que significa que la autenticación y montaje de archivos administrados por el servicio Azure Machine Learning. Más información sobre cómo especificar un almacén de datos como el directorio de origen y cargar archivos en el almacén de datos en el [tener acceso a datos desde los almacenes de datos](how-to-access-data.md) artículo.

* **Si solo necesita dos archivos de datos y secuencias de comandos de dependencia y no puede usar un almacén de datos,** colocar los archivos en el mismo directorio de la carpeta que el script de entrenamiento. Especifique esta carpeta como su `source_directory` directamente en el script de entrenamiento, o en el código que llama a su script de entrenamiento.

<a name="limits"></a>

### <a name="storage-limits-of-experiment-snapshots"></a>Límites de almacenamiento de instantáneas del experimento

Para los experimentos, Azure Machine Learning realiza automáticamente una instantánea del experimento de su código basado en el directorio que sugerir al configurar la ejecución. Esto tiene un límite total de 300 MB o archivos de 2000. Si se supera este límite, verá el siguiente error:

```Python
While attempting to take snapshot of .
Your total snapshot size exceeds the limit of 300.0 MB
```

Para resolver este error, almacenar los archivos de experimento en un almacén de datos. Si no se puede usar un almacén de datos, la tabla siguiente ofrece soluciones alternativas posibles.

Experimento&nbsp;descripción|Solución de límite de almacenamiento
---|---
Menos de 2000 archivos y no se puede usar un almacén de datos| Invalidar el límite de tamaño de instantánea con <br> `azureml._restclient.snapshots_client.SNAPSHOT_MAX_SIZE_BYTES = 'insert_desired_size'`<br> Esto puede tardar varios minutos según el número y tamaño de los archivos.
Debe usar el directorio específico de la secuencia de comandos| Realizar una `.amlignore` archivo para excluir archivos de la instantánea del experimento que no forman parte del código fuente. Agregue los nombres de archivo para el `.amlignore` de archivo y colóquelo en el mismo directorio que el script de entrenamiento. El `.amlignore` archivo usa el mismo [sintaxis y patrones](https://git-scm.com/docs/gitignore) como un `.gitignore` archivo.
Canalización|Utilizar un subdirectorio diferente en cada paso
Jupyter Notebook| Crear un `.amlignore` de archivos o mover el Bloc de notas en un subdirectorio nuevo y vacío, y vuelva a ejecutar el código.

## <a name="where-to-write-files"></a>Donde se escribe en archivos

Debido al aislamiento de experimentos de entrenamiento, los cambios en los archivos que se producen durante las ejecuciones no necesariamente persisten fuera de su entorno. Si la secuencia de comandos modifica los archivos locales de proceso, no se conservan los cambios para su siguiente ejecución del experimento y no se propagan automáticamente a la máquina cliente. Por lo tanto, los cambios realizados durante el primer experimento de ejecución no y no deberían afectar a los de la segunda.

Al escribir los cambios, se recomienda escribir archivos en un almacén de datos de Azure Machine Learning. Consulte [tener acceso a datos desde los almacenes de datos](how-to-access-data.md).

Si no necesita un almacén de datos, escribir archivos en el `./outputs` o `./logs` carpeta.

>[!Important]
> Las dos carpetas, *genera* y *registros*, reciben un tratamiento especial por Azure Machine Learning. Durante el entrenamiento, al escribir archivos en`./outputs` y`./logs` carpetas, los archivos automáticamente cargará en su historial de ejecución para que tengan acceso a ellos, una vez finalizada la ejecución.

* **Para la salida como mensajes de estado o los resultados de puntuación,** grabar archivos en el `./outputs` carpeta, por lo que se conservan como artefactos en el historial de ejecución. Esté atento el número y tamaño de los archivos escritos en esta carpeta, como la latencia puede producirse cuando el contenido se carga en el historial de ejecución. Si la latencia es un problema, se recomienda la escritura de archivos en un almacén de datos.

* **Para guardar el archivo escrito como registros de historial de ejecución** grabar archivos en `./logs` carpeta. Los registros se cargan en tiempo real, por lo que este método es adecuado para las actualizaciones directas de una ejecución remota de transmisión por secuencias.

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre [acceso a datos desde los almacenes de datos](how-to-access-data.md).

* Obtenga más información sobre [cómo establecer objetivos de aprendizaje](how-to-set-up-training-targets.md).
