---
title: ¿Cómo funciona el servicio Azure Machine Learning?
description: Obtenga información sobre la arquitectura, la terminología y los conceptos que conforman el servicio de Azure Machine Learning. También podrá informarse sobre el flujo de trabajo general de utilizar el servicio y sobre los servicios de Azure que utiliza el servicio de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: haining
author: hning86
ms.reviewer: larryfr
ms.date: 10/24/2018
ms.openlocfilehash: 0acf41cc0a2673ba665d1815b493df928fa4507d
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706813"
---
# <a name="how-the-azure-machine-learning-service-works-architecture-and-concepts"></a>Cómo funciona el servicio Azure Machine Learning: arquitectura y conceptos

En este documento se describen la arquitectura y los conceptos del servicio Azure Machine Learning. En el siguiente diagrama se muestran los componentes principales del servicio y se ilustra el flujo de trabajo general al utilizar el servicio: 

[![Arquitectura y flujo de trabajo de Azure Machine Learning Services](./media/concept-azure-machine-learning-architecture/workflow.png)](./media/concept-azure-machine-learning-architecture/workflow.png#lightbox)

Normalmente, el flujo de trabajo sigue estos pasos:

1. Los scripts de entrenamiento del aprendizaje automático se desarrollan en __Python__.
1. Se crea y configura un __destino de proceso__.
1. __Se envían los scripts__ al destino de proceso configurado para ejecutarse en ese entorno. Durante el entrenamiento, el destino de proceso almacena los registros de ejecución en un __almacén de datos__. Allí los registros se guardan en un __experimento__.
1. __Se consulta el experimento__ para las métricas registradas en ejecuciones actuales y anteriores. Si las métricas no ofrecen el resultado deseado, se vuelve en bucle al paso 1 y se repiten los scripts.
1. Cuando se encuentra una ejecución satisfactoria, se registra el modelo guardado en el __registro del modelo__.
1. Se desarrolla un script de puntuación.
1. __Se crea una imagen__ y se registra en el __registro de la imagen__. 
1. __Se implementa la imagen__ como un __servicio web__ en Azure.


[!INCLUDE [aml-preview-note](../../../includes/aml-preview-note.md)]

> [!NOTE]
> Aunque en este documento se definen los términos y conceptos utilizados por Azure Machine Learning, no se definen los términos y conceptos de la plataforma de Azure. Para obtener más información sobre la terminología de la plataforma de Azure, consulte el [glosario de Microsoft Azure](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology).

## <a name="workspace"></a>Área de trabajo

El área de trabajo es el recurso de nivel superior para el servicio Azure Machine Learning. Proporciona un lugar centralizado para trabajar con todos los artefactos que crea al usar Azure Machine Learning Services.

El área de trabajo conserva una lista de destinos de proceso que se puede usar para entrenar el modelo. También conserva un historial de las ejecuciones del entrenamiento, que incluye registros, métricas, resultados y una instantánea de sus scripts. Esta información se usa para determinar qué ejecución de entrenamiento produce el mejor modelo.

Los modelos se registran con el área de trabajo. Se utilizan un modelo registrado y los scripts de puntuación para crear una imagen. Después, la imagen se puede implementar en Azure Container Instances, en Azure Kubernetes Service o en una matriz de puerta programable en el campo (FPGA) como un punto de conexión HTTP basado en REST. También puede implementarse en un dispositivo Azure IoT Edge como un módulo.

Puede crear varias áreas de trabajo, y cada área de trabajo se puede compartir entre varias personas. Al compartir un área de trabajo, controle el acceso al área de trabajo mediante la asignación de los siguientes roles a los usuarios:

* Propietario
* Colaborador
* Lector

Al crear una nueva área de trabajo, se crean automáticamente varios recursos de Azure que el área de trabajo utiliza:

* [Azure Container Registry](https://azure.microsoft.com/services/container-registry/): registra los contenedores de Docker, que se usan durante el entrenamiento y al implementar un modelo.
* [Azure Storage](https://azure.microsoft.com/services/storage/): se usa como almacén de datos predeterminado para el área de trabajo.
* [Azure Application Insights](https://azure.microsoft.com/services/application-insights/): almacena información sobre la supervisión de los modelos.
* [Azure Key Vault](https://azure.microsoft.com/services/key-vault/): almacena secretos usados por los destinos de proceso y otra información confidencial que el área de trabajo necesita.

> [!NOTE]
> En lugar de crear nuevas versiones, también puede usar los servicios de Azure existentes. 

El diagrama siguiente es una taxonomía del área de trabajo:

[![Taxonomía del área de trabajo](./media/concept-azure-machine-learning-architecture/taxonomy.png)](./media/concept-azure-machine-learning-architecture/taxonomy.png#lightbox)

## <a name="model"></a>Modelo

En su forma más simple, un modelo es un fragmento de código que toma una entrada y genera una salida. El hecho de crear un modelo de Machine Learning implica seleccionar un algoritmo, proporcionarle datos y ajustar los hiperparámetros. El entrenamiento es un proceso iterativo que genera un modelo entrenado, que encapsula lo que el modelo ha aprendido durante el proceso de aprendizaje.

Un modelo se genera mediante una ejecución en Azure Machine Learning. También puede usar un modelo entrenado fuera de Azure Machine Learning. Un modelo se puede registrar en un área de trabajo de Azure Machine Learning Services.

Azure Machine Learning Services es independiente del marco de trabajo. Puede usar cualquier marco de aprendizaje automático popular al crear un modelo, como scikit-learn, xgboost, PyTorch, TensorFlow, Chainer y CNTK.

Para obtener un ejemplo de entrenamiento de un modelo, vea el documento [Guía de inicio rápido: Uso de Azure Portal para empezar a trabajar con Azure Machine Learning](quickstart-get-started.md).

### <a name="model-registry"></a>Registro de modelos

El registro de modelo realiza un seguimiento de todos los modelos del área de trabajo de Azure Machine Learning Services. 

Los modelos se identifican por el nombre y la versión. Cada vez que registra un modelo con el mismo nombre que uno existente, el registro interpreta que se trata de una nueva versión. La versión se incrementa y el nuevo modelo se registra con el nombre en cuestión.

Puede proporcionar etiquetas de metadatos adicionales al registrar el modelo y, después, utilizar estas etiquetas al buscar modelos.

No puede eliminar los modelos una imagen esté utilizando.

Para obtener un ejemplo de registro de un modelo, vea el documento [Train an image classification model with Azure Machine Learning](tutorial-train-models-with-aml.md) (Entrenamiento de un modelo de clasificación de imágenes con Azure Machine Learning).

## <a name="image"></a>Imagen

Las imágenes ofrecen una forma fiable de implementar un modelo, junto con todos los componentes necesarios para usar el modelo. Una imagen contiene los siguientes elementos:

* Un modelo.
* Una aplicación o un script de puntuación. Este script se utiliza para pasar la entrada del modelo y devolver el resultado del modelo.
* Las dependencias que necesita el modelo o el script o la aplicación de puntuación.  Por ejemplo, puede incluir un archivo de entorno de Conda en el que se detallen las dependencias de paquetes de Python.

Hay dos tipos de imágenes que Azure Machine Learning puede crear:

* Imagen de FPGA: se utiliza al realizar una implementación en una matriz de puerta programable por campo en la nube de Azure.
* Imagen de Docker: se usa al implementar para calcular los destinos que no son FPGA. Por ejemplo, Azure Container Instances y Azure Kubernetes Service.

Para obtener un ejemplo de creación de una imagen, consulte el documento [Deploy an image classification model in Azure Container Instance](tutorial-deploy-models-with-aml.md) (Implementación de un modelo de clasificación de imágenes en Azure Container Instance).

### <a name="image-registry"></a>Registro de imágenes

El registro de imágenes realiza un seguimiento de las imágenes creadas a partir de sus modelos. Puede proporcionar etiquetas de metadatos adicionales al crear la imagen. El registro de imágenes almacena las etiquetas de metadatos, que se pueden consultar para encontrar la imagen.

## <a name="deployment"></a>Implementación

Una implementación es una instancia de la imagen en un servicio web que puede hospedarse en la nube o un módulo de IoT para las implementaciones de dispositivos integrados. 

### <a name="web-service"></a>Servicio web

Un servicio web implementado puede usar Azure Container Instances, Azure Kubernetes Service o matrices de puertas programables por campo (FPGA).
El servicio se crea a partir de una imagen que encapsula el modelo, los scripts y los archivos asociados. La imagen tiene un punto de conexión HTTP de carga equilibrada que recibe solicitudes de puntuación enviadas al servicio web.

Azure le ayuda a supervisar la implementación del servicio web mediante la recopilación de telemetría de Application Insight o telemetría de modelos, si ha optado por habilitar esta característica. Solo usted puede acceder a los datos de telemetría, que se almacenan en las instancias de su cuenta de almacenamiento y Application Insights.

Si ha habilitado el ajuste automático, Azure ajustará automáticamente su implementación.

Para obtener un ejemplo de implementación de un modelo como servicio web, consulte el documento [Deploy an image classification model in Azure Container Instance](tutorial-deploy-models-with-aml.md) (Implementación de un modelo de clasificación de imágenes en Azure Container Instance).

### <a name="iot-module"></a>Módulo de IoT

Un módulo de IoT implementado es un contenedor de Docker que incluye el modelo y el script asociado o la aplicación y las dependencias adicionales. Estos módulos se implementan con Azure IoT Edge en dispositivos perimetrales. 

Si ha habilitado la supervisión, Azure recopila datos de telemetría desde el modelo dentro del módulo de Azure IoT Edge. Solo usted puede acceder a los datos de telemetría, que se almacenan en las instancias de su cuenta de almacenamiento.

Azure IoT Edge garantiza que el módulo se esté ejecutando y supervisa el dispositivo que lo hospeda.

## <a name="datastore"></a>Almacén de datos

Un almacén de datos es una abstracción de almacenamiento en una cuenta de Azure Storage. El almacén de datos puede usar un contenedor de blobs de Azure o un recurso compartido de archivos de Azure como almacenamiento back-end. Cada área de trabajo tiene un almacén de datos predeterminado y el usuario puede registrar almacenes de datos adicionales. 

Use la API del SDK de Python o la CLI de Azure Machine Learning para almacenar y recuperar archivos desde el almacén de datos. 

## <a name="run"></a>Ejecute

Una ejecución es un registro que contiene la información siguiente:

* Metadatos sobre la ejecución (marca de tiempo, duración, etc.).
* Métricas que el script registra.
* Archivos de salida recopilados automáticamente por el experimento o cargados explícitamente por el usuario.
* Instantánea del directorio que contiene los scripts antes de la ejecución.

Una ejecución se produce cuando se envía un script para entrenar un modelo. Una ejecución puede tener cualquier número de ejecuciones secundarias. De este modo, la ejecución de nivel superior puede tener dos ejecuciones secundarias, cada una de las cuales puede tener sus propias ejecuciones secundarias.

Para obtener un ejemplo de ejecuciones de visualización mediante el entrenamiento de un modelo, vea el documento [Guía de inicio rápido: Introducción al servicio Azure Machine Learning](quickstart-get-started.md).

## <a name="experiment"></a>Experimento

Un experimento es una agrupación de varias ejecuciones de un script determinado. Siempre pertenece a un área de trabajo. Cuando envíe una ejecución, proporcione un nombre de experimento. La información de la ejecución se almacena en ese experimento. Si envía una ejecución y especifica un nombre de experimento que no existe, se crea automáticamente un experimento nuevo con ese nombre.

Para obtener un ejemplo del uso de un experimento, vea el documento [Guía de inicio rápido: Introducción al servicio Azure Machine Learning](quickstart-get-started.md).

## <a name="pipeline"></a>Canalización

Las canalizaciones de aprendizaje automático se usan para crear y administrar flujos de trabajo que unen las fases de aprendizaje automático. Por ejemplo, una canalización podría incluir las fases de preparación de los datos, entrenamiento del modelo, implementación de modelo e inferencia. Cada fase puede estar formada por varios pasos, cada uno de los cuales puede ejecutarse en modo desatendido en varios destinos de proceso.

Para más información sobre las canalizaciones de aprendizaje automático con este servicio, consulte el artículo [Canalizaciones y Azure Machine Learning](concept-ml-pipelines.md).

## <a name="compute-target"></a>Destino de proceso

Un destino de proceso es el recurso de proceso que se usa para ejecutar el script de aprendizaje o para hospedar la implementación del servicio web. Los destinos de proceso admitidos son los siguientes: 

| Destino de proceso | Cursos | Implementación |
| ---- |:----:|:----:|
| Equipo local | ✓ | &nbsp; |
| Una única máquina virtual Linux en Azure</br>(como Data Science Virtual Machine) | ✓ | &nbsp; |
| Clúster de Azure Batch AI | ✓ | &nbsp; |
| Azure Databricks | ✓ | &nbsp; | &nbsp; |
| Análisis con Azure Data Lake | ✓ | &nbsp; |
| Apache Spark para HDInsight | ✓ | &nbsp; |
| Azure Container Instances | ✓ | ✓ |
| Azure Kubernetes Service | &nbsp; | ✓ |
| Azure IoT Edge | &nbsp; | ✓ |
| Project Brainwave</br>(matriz de puertas programables por campo) | &nbsp; | ✓ |

Los destinos de proceso están asociados a un área de trabajo. Los usuarios del área de trabajo comparten los destinos de proceso que no sean el equipo local.

La mayoría de destinos de proceso se pueden crear directamente a través del área de trabajo mediante Azure Portal, el SDK de Azure Machine Learning o la CLI de Azure. Si dispone de los destinos de proceso creados por otro proceso (por ejemplo, Azure Portal o la CLI de Azure), puede agregarlos (asociarlos) al área de trabajo. Algunos procesos de destino deben crearse fuera del área de trabajo y adjuntarse posteriormente.

Para obtener información sobre cómo seleccionar un destino de proceso para el aprendizaje, consulte el documento [Select and use a compute target to train your model](how-to-set-up-training-targets.md) (Selección y uso un destino de proceso para entrenar el modelo).

Para obtener información sobre cómo seleccionar un destino de proceso para la implementación, consulte el documento [Deploy models with the Azure Machine Learning service](how-to-deploy-and-where.md) (Implementación de modelos con el servicio de Azure Machine Learning).

## <a name="run-configuration"></a>Configuración de ejecución

Una configuración de ejecución es un conjunto de instrucciones que define cómo se debe ejecutar un script en un destino de proceso determinado. Incluye un amplio conjunto de definiciones de comportamiento, por ejemplo, si quiere usar un entorno de Python existente o un entorno de Conda creado a partir de la especificación.

Una configuración de ejecución puede conservarse en un archivo dentro del directorio que contiene el script de aprendizaje o crearse como un objeto en memoria y usarse para enviar una ejecución.

Para ver configuraciones de ejecución de ejemplo, vea el documento [Select and use a compute target to train your model](how-to-set-up-training-targets.md) (Selección y uso de un destino de proceso para entrenar el modelo).

## <a name="training-script"></a>Script de entrenamiento

Para entrenar un modelo, especifique el directorio que contiene el script de entrenamiento y los archivos asociados. También puede especificar un nombre del experimento, que se usará para almacenar la información recopilada durante el entrenamiento. Durante dicho proceso de entrenamiento, se copia todo el directorio en el entorno de entrenamiento (destino de proceso) y se inicia el script especificado por la configuración de ejecución. También se almacena una instantánea del directorio en el experimento del área de trabajo.

Puede encontrar un ejemplo en [Creación de un área de trabajo con Python](quickstart-get-started.md).

## <a name="logging"></a>Registro

Al desarrollar la solución, use el SDK de Python de Azure Machine Learning en el script de Python para registrar las métricas arbitrarias. Después de la ejecución, consulte las métricas para determinar si la ejecución ha generado el modelo que quiere implementar. 

## <a name="snapshot"></a>Instantánea

Al enviar una ejecución, Azure Machine Learning comprime el directorio que contiene el script como un archivo zip y lo envía al destino de proceso. A continuación, el archivo .zip se expande y el script se ejecuta en él. Azure Machine Learning también almacena el archivo .zip como una instantánea como parte del registro de ejecución. Cualquier persona con acceso al área de trabajo puede buscar un registro de ejecución y descargar la instantánea.

## <a name="activity"></a>Actividad

Una actividad representa una operación de larga ejecución. Las operaciones siguientes son ejemplos de actividades:

* Creación o eliminación de un destino de proceso
* Ejecución de un script en un destino de proceso

Las actividades pueden proporcionar notificaciones a través del SDK o la interfaz de usuario web para que pueda supervisar fácilmente el progreso de estas operaciones.

## <a name="next-steps"></a>Pasos siguientes

Consulte los vínculos siguientes para empezar a usar Azure Machine Learning:

* [¿Qué es el servicio Azure Machine Learning?](overview-what-is-azure-ml.md)
* [Guía de inicio rápido: Creación de un área de trabajo con Python](quickstart-get-started.md)
* [Tutorial: Entrenamiento de un modelo](tutorial-train-models-with-aml.md)
