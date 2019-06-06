---
title: Arquitectura y conceptos clave
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre la arquitectura, los términos, conceptos y flujo de trabajo que componen el servicio Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: larryfr
author: Blackmist
ms.date: 04/15/2019
ms.custom: seodec18
ms.openlocfilehash: 8bb06d04aec8e98308c0f5595b6b39e4b98302ff
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66480063"
---
# <a name="how-azure-machine-learning-service-works-architecture-and-concepts"></a>Cómo funciona Azure Machine Learning Service: Arquitectura y conceptos

Obtenga información sobre la arquitectura, los conceptos y flujo de trabajo para el servicio Azure Machine Learning. En el siguiente diagrama se muestran los componentes principales del servicio y se ilustra el flujo de trabajo general al usar el servicio:

[![Arquitectura y flujo de trabajo de Azure Machine Learning Service](./media/concept-azure-machine-learning-architecture/workflow.png)](./media/concept-azure-machine-learning-architecture/workflow.png#lightbox)

## <a name="workflow"></a>Flujo de trabajo

Por lo general, el flujo de trabajo de machine learning sigue esta secuencia:

1. Los scripts de entrenamiento del aprendizaje automático se desarrollan en **Python**.
1. Se crea y configura un **destino de proceso**.
1. **Se envían los scripts** al destino de proceso configurado para ejecutarse en ese entorno. Durante el entrenamiento, los scripts pueden leer o escribir en el **almacén de datos**. Y los registros de ejecución se guardan como **ejecuciones** en el **área de trabajo**, agrupados en **experimentos**.
1. **Se consulta el experimento** para las métricas registradas en ejecuciones actuales y anteriores. Si las métricas no ofrecen el resultado deseado, se vuelve en bucle al paso 1 y se repiten los scripts.
1. Cuando se encuentra una ejecución satisfactoria, se registra el modelo guardado en el **registro de modelos**.
1. Desarrollar un script de puntuación que usa el modelo y **implantarlo** como un **servicio web** en Azure, o a un **dispositivo de IoT Edge**.

Siga estos pasos con cualquiera de las siguientes acciones:
+ [SDK de Azure Machine Learning para Python](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
+ [Azure Machine Learning CLI](https://docs.microsoft.com/azure/machine-learning/service/reference-azure-machine-learning-cli)
+ [Extensión de Azure Machine Learning VS Code](how-to-vscode-tools.md)
+  El [interfaz visual (versión preliminar) para el servicio Azure Machine Learning](ui-concept-visual-interface.md)

> [!NOTE]
> Aunque en este artículo se definen los términos y conceptos que usa Azure Machine Learning Service, no se definen los términos y conceptos de la plataforma Azure. Para obtener más información sobre la terminología de la plataforma Azure, consulte el [glosario de Microsoft Azure](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology).

## <a name="workspace"></a>Área de trabajo

[El área de trabajo](concept-workspace.md) es el recurso de nivel superior para el servicio Azure Machine Learning. Proporciona un lugar centralizado para trabajar con todos los artefactos que cree al usar Azure Machine Learning Service.

El diagrama siguiente es una taxonomía del área de trabajo:

[![Taxonomía del área de trabajo](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png)](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png#lightbox)

Para obtener más información acerca de las áreas de trabajo, consulte [¿qué es un área de trabajo de Azure Machine Learning?](concept-workspace.md).

## <a name="experiment"></a>Experimento

Un experimento es una agrupación de varias ejecuciones de un script determinado. Siempre pertenece a un área de trabajo. Cuando envíe una ejecución, proporcione un nombre de experimento. La información de la ejecución se almacena en ese experimento. Si envía una ejecución y especifica un nombre de experimento que no existe, se crea automáticamente un experimento nuevo con ese nombre.

Para obtener un ejemplo del uso de un experimento, consulte [Inicio rápido: Introducción a Azure Machine Learning Service](quickstart-run-cloud-notebook.md).

## <a name="model"></a>Modelo

En su forma más simple, un modelo es un fragmento de código que toma una entrada y genera una salida. El hecho de crear un modelo de Machine Learning implica seleccionar un algoritmo, proporcionarle datos y ajustar los hiperparámetros. El entrenamiento es un proceso iterativo que genera un modelo entrenado, que encapsula lo que el modelo ha aprendido durante el proceso de aprendizaje.

Un modelo se genera mediante una ejecución en Azure Machine Learning. También puede usar un modelo entrenado fuera de Azure Machine Learning. Puede registrar el modelo en un área de trabajo de Azure Machine Learning Service.

Recuerde que Azure Machine Learning Service es independiente del marco de trabajo. Cuando se crea un modelo, puede usar cualquier marco de aprendizaje automático populares, como Scikit-learn, XGBoost, PyTorch, TensorFlow y Chainer.

Para obtener un ejemplo de entrenamiento de un modelo, vea [Tutorial: Entrenamiento de un modelo de clasificación de imágenes con Azure Machine Learning Service](tutorial-train-models-with-aml.md).

### <a name="model-registry"></a>Registro de modelos

El registro de modelo realiza un seguimiento de todos los modelos del área de trabajo de Azure Machine Learning Services.

Los modelos se identifican por el nombre y la versión. Cada vez que registra un modelo con el mismo nombre que uno ya existente, el registro interpreta que se trata de una nueva versión. La versión se incrementa y el nuevo modelo se registra con el nombre en cuestión.

Una vez registrado el modelo, puede proporcionar etiquetas de metadatos adicionales y, después, usar esas etiquetas al buscar modelos.

No se puede eliminar los modelos que están siendo utilizados por una implementación activa.

Para obtener un ejemplo de registro de un modelo, consulte [Entrenamiento de un modelo de clasificación de imágenes con Azure Machine Learning](tutorial-train-models-with-aml.md).

## <a name="run-configuration"></a>Configuración de ejecución

Una configuración de ejecución es un conjunto de instrucciones que define cómo se debe ejecutar un script en un destino de proceso determinado. Esta configuración incluye un amplio conjunto de definiciones de comportamiento como, por ejemplo, si quiere usar un entorno de Python existente o un entorno de Conda creado a partir de la especificación.

Una configuración de ejecución puede conservarse en un archivo del directorio que contiene el script de entrenamiento o puede crearse como un objeto en la memoria y usarse para enviar una ejecución.

Para ver configuraciones de ejecución de ejemplo, consulte [Selección y uso de un destino de proceso para entrenar el modelo](how-to-set-up-training-targets.md).

## <a name="dataset"></a>Dataset

Azure Machine Learning conjuntos de datos (versión preliminar) facilitan el acceso y trabajar con los datos. Los conjuntos de datos administran datos en varios escenarios, como el entrenamiento del modelo y creación de la canalización. Con el SDK de Azure Machine Learning, puede tener acceso al almacenamiento subyacente, explorar y preparar datos, administrar el ciclo de vida de las definiciones de conjunto de datos diferentes y comparar entre conjuntos de datos utilizados en entrenamiento y en producción.

Los conjuntos de datos proporciona métodos para trabajar con datos en formatos populares, como el uso de `from_delimited_files()` o `to_pandas_dataframe()`.

Para obtener más información, consulte [crear y registrar los conjuntos de datos de Azure Machine Learning](how-to-create-register-datasets.md).

Para obtener un ejemplo del uso de conjuntos de datos, vea el [cuadernos de ejemplo](https://aka.ms/dataset-tutorial).

## <a name="datastore"></a>Almacén de datos

Un almacén de datos es una abstracción de almacenamiento en una cuenta de Azure Storage. El almacén de datos puede usar un contenedor de blobs de Azure o un recurso compartido de archivos de Azure como almacenamiento back-end. Cada área de trabajo tiene un almacén de datos predeterminado en el que puede registrar almacenes de datos adicionales.

Use la API del SDK de Python o la CLI de Azure Machine Learning para almacenar y recuperar archivos desde el almacén de datos.

## <a name="compute-target"></a>Destino de proceso

Un destino de proceso es el recurso de proceso que se usa para ejecutar el script de entrenamiento o para hospedar la implementación del servicio web. Los destinos de proceso admitidos son los siguientes:

| Destino de proceso | Cursos | Implementación |
| ---- |:----:|:----:|
| Equipo local | ✓ | &nbsp; |
| Proceso de Azure Machine Learning | ✓ | &nbsp; |
| Una única máquina virtual Linux en Azure</br>(como Data Science Virtual Machine) | ✓ | &nbsp; |
| Azure Databricks | ✓ | &nbsp; |
| Análisis con Azure Data Lake | ✓ | &nbsp; |
| Apache Spark para HDInsight | ✓ | &nbsp; |
| Azure Container Instances | &nbsp; | ✓ |
| Azure Kubernetes Service | &nbsp; | ✓ |
| Azure IoT Edge | &nbsp; | ✓ |
| Matriz de puertas programables por campo (FPGA) | &nbsp; | ✓ |

Los destinos de proceso están asociados a un área de trabajo. Los usuarios del área de trabajo comparten los destinos de proceso que no sean el equipo local.

### <a name="managed-and-unmanaged-compute-targets"></a>Destinos de proceso administrados y no administrados

* **Administrado**: destinos de proceso que crea y administra Azure Machine Learning Service. Estos destinos de proceso están optimizados para cargas de trabajo de Machine Learning. El proceso de Azure Machine Learning es el único destino de proceso administrado desde el 4 de diciembre de 2018. En el futuro pueden agregarse destinos de proceso administrados adicionales.

    Puede crear instancias del proceso de Machine Learning directamente en el área de trabajo si usa Azure Portal, el SDK de Azure Machine Learning o la CLI de Azure. Todos los demás destinos de proceso deben crearse fuera del área de trabajo y adjuntarse posteriormente.

* **No administrado**: destinos de proceso que *no* administra Azure Machine Learning Service. Es posible que tenga que crearlos fuera de Azure Machine Learning y, después, conectarlos al área de trabajo antes de usarlos. Estos destinos de proceso no administrados pueden requerir pasos adicionales para mantener o mejorar el rendimiento de las cargas de trabajo de Machine Learning.

Para obtener información sobre cómo seleccionar un destino de proceso para el aprendizaje, consulte [Selección y uso de un destino de proceso para entrenar el modelo](how-to-set-up-training-targets.md).

Para obtener información sobre cómo seleccionar un destino de proceso para la implementación, consulte [Implementación de modelos con Azure Machine Learning Service](how-to-deploy-and-where.md).

## <a name="training-script"></a>Script de entrenamiento

Para entrenar un modelo, especifique el directorio que contiene el script de entrenamiento y los archivos asociados. También puede especificar un nombre del experimento que se usará para almacenar la información recopilada durante el entrenamiento. Durante dicho proceso de entrenamiento, se copia todo el directorio en el entorno de entrenamiento (destino de proceso) y se inicia el script que especifique la configuración de ejecución. También se almacena una instantánea del directorio en el experimento del área de trabajo.

Si quiere ver un ejemplo, consulte [Tutorial: Entrenamiento de un modelo de clasificación de imágenes con Azure Machine Learning Service](tutorial-train-models-with-aml.md).

## <a name="run"></a>Ejecute

Una ejecución es un registro que contiene la información siguiente:

* Metadatos sobre la ejecución (marca de tiempo, duración, etc.).
* Métricas que el script registra.
* Archivos de salida que recopila automáticamente el experimento o que carga explícitamente usted mismo.
* Instantánea del directorio que contiene los scripts antes de la ejecución.

Una ejecución se produce cuando se envía un script para entrenar un modelo. Una ejecución puede tener cualquier número de ejecuciones secundarias. Por ejemplo, la ejecución de nivel superior puede tener dos ejecuciones secundarias, cada una de las cuales puede tener sus propias ejecuciones secundarias.

Para obtener un ejemplo de las vistas de ejecución que se producen al entrenar un modelo, consulte [Inicio rápido: Introducción a Azure Machine Learning Service](quickstart-run-cloud-notebook.md).

## <a name="github-tracking-and-integration"></a>Integración y seguimiento de GitHub

Cuando se inicia un entrenamiento que se ejecute, donde el directorio de origen es un repositorio Git local, se almacena información sobre el repositorio en el historial de ejecución. Por ejemplo, el identificador de confirmación actual para el repositorio se registra como parte del historial. Esto funciona con ejecuciones que se envía mediante un Estimador, canalización ML o ejecución del script. También funciona para las ejecuciones enviadas desde el SDK o la CLI de Machine Learning.

## <a name="snapshot"></a>Instantánea

Al enviar una ejecución, Azure Machine Learning comprime el directorio que contiene el script como un archivo zip y lo envía al destino de proceso. A continuación, el archivo .zip se extrae y el script se ejecuta. Azure Machine Learning también almacena el archivo .zip como una instantánea como parte del registro de ejecución. Cualquier persona con acceso al área de trabajo puede buscar un registro de ejecución y descargar la instantánea.

> [!NOTE]
> Para evitar que se incluyan en la instantánea de archivos innecesarios, asegúrese de un archivo ignore (.gitignore o .amlignore). Coloque este archivo en el directorio de instantáneas y agregue los nombres de archivo para pasar por alto en ella. El archivo .amlignore usa el mismo [sintaxis y patrones que el archivo .gitignore](https://git-scm.com/docs/gitignore). Si ambos archivos se encuentran, el archivo .amlignore tiene prioridad.

## <a name="activity"></a>Actividad

Una actividad representa una operación de larga ejecución. Las operaciones siguientes son ejemplos de actividades:

* Creación o eliminación de un destino de proceso
* Ejecución de un script en un destino de proceso

Las actividades pueden proporcionar notificaciones a través del SDK o la interfaz de usuario web para que pueda supervisar fácilmente el progreso de estas operaciones.

## <a name="image"></a>Image

Las imágenes ofrecen una forma fiable de implementar un modelo, junto con todos los componentes necesarios para usar el mismo. Una imagen contiene los siguientes elementos:

* Un modelo.
* Una aplicación o un script de puntuación. Este script se usa para pasar la entrada del modelo y devolver el resultado del mismo.
* Las dependencias que necesita el modelo, el script de puntuación o la aplicación. Por ejemplo, puede incluir un archivo de entorno de Conda en el que se detallen las dependencias de paquetes de Python.

Azure Machine Learning puede crear dos tipos de imágenes:

* **Imagen de FPGA**: se usa al realizar una implementación en una matriz de puerta programable de campo en Azure.
* **Imagen de Docker**: se usa al realizar implementaciones en destinos de proceso que no sean FPGA. Por ejemplo, Azure Container Instances y Azure Kubernetes Service.

El servicio de Azure Machine Learning proporciona una imagen base, que se usa de forma predeterminada. También puede proporcionar sus propias imágenes personalizadas.

Para obtener un ejemplo de creación de una imagen, consulte [Implementación de un modelo de clasificación de imágenes en Azure Container Instances](tutorial-deploy-models-with-aml.md).

### <a name="image-registry"></a>Registro de imágenes

El registro de imágenes realiza un seguimiento de las imágenes creadas a partir de sus modelos. Puede proporcionar etiquetas de metadatos adicionales al crear la imagen. El registro de imágenes almacena las etiquetas de metadatos que se pueden consultar para encontrar la imagen.

## <a name="deployment"></a>Implementación

Una implementación es una instancia del modelo en un servicio web que se puede hospedar en la nube o un módulo de IoT para las implementaciones de dispositivos integrados.

### <a name="web-service"></a>Servicio web

Un servicio web implementado puede usar Azure Container Instances, Azure Kubernetes Service o FPGA. Cree el servicio desde el modelo, scripts y archivos asociados. Estas se encapsulan en una imagen, que proporciona el entorno de tiempo de ejecución para el servicio web. La imagen tiene un punto de conexión HTTP de carga equilibrada que recibe solicitudes de puntuación que se envían al servicio web.

Azure le ayuda a supervisar la implementación del servicio web mediante la recopilación de telemetría de Application Insight o telemetría de modelos, si es que ha optado por habilitar esta característica. Solo usted puede obtener acceso a los datos de telemetría, que se almacenan en las instancias de su cuenta de almacenamiento y Application Insights.

Si ha habilitado el ajuste automático de escala, Azure ajustará automáticamente la escala de su implementación.

Para obtener un ejemplo de implementación de un modelo como servicio web, consulte [Implementación de un modelo de clasificación de imágenes en Azure Container Instances](tutorial-deploy-models-with-aml.md).

### <a name="iot-module"></a>Módulo de IoT

Un módulo de IoT implementado es un contenedor de Docker que incluye el modelo y el script asociado o la aplicación y las dependencias adicionales. Implemente estos módulos mediante el uso de Azure IoT Edge en dispositivos perimetrales.

Si ha habilitado la supervisión, Azure recopila datos de telemetría desde el modelo que está en el módulo de Azure IoT Edge. Solo usted puede obtener acceso a los datos de telemetría, que se almacenan en las instancias de su cuenta de almacenamiento.

Azure IoT Edge garantiza que el módulo se esté ejecutando y supervisa el dispositivo que lo hospeda.

## <a name="pipeline"></a>Canalización

Las canalizaciones de aprendizaje automático se usan para crear y administrar flujos de trabajo que unen las fases de aprendizaje automático. Por ejemplo, podría incluir una canalización de preparación de datos, entrenamiento del modelo, implementación de modelos y las fases de inferencia/puntuación. Cada fase puede estar formada por varios pasos, cada uno de los cuales puede ejecutarse en modo desatendido en varios destinos de proceso.

Para obtener más información sobre las canalizaciones de aprendizaje automático con este servicio, consulte el artículo [Canalizaciones y Azure Machine Learning](concept-ml-pipelines.md).

## <a name="logging"></a>Registro

Al desarrollar la solución, use el SDK de Python de Azure Machine Learning en el script de Python para registrar las métricas arbitrarias. Después de la ejecución, consulte las métricas para determinar si la ejecución ha generado el modelo que quiere implementar.

## <a name="next-steps"></a>Pasos siguientes

Para comenzar a usar Azure Machine Learning Service, consulte:

* [¿Qué es Azure Machine Learning Service?](overview-what-is-azure-ml.md)
* [Crear un área de trabajo del servicio de Azure Machine Learning](setup-create-workspace.md)
* [Tutorial: Entrenamiento de un modelo](tutorial-train-models-with-aml.md)
* [Crear un área de trabajo con una plantilla de Resource Manager](how-to-create-workspace-template.md)
