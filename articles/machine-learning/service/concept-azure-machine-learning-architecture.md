---
title: 'Machine Learning en la nube: términos y arquitectura'
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre la arquitectura, la terminología y los conceptos que conforman el servicio Azure Machine Learning. También podrá informarse sobre el flujo de trabajo general que supone usar el servicio y sobre los servicios de Azure que usa Azure Machine Learning Service.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: haining
author: hning86
ms.reviewer: larryfr
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: a5a5b306341780ead737def90306fe6cb6a47db1
ms.sourcegitcommit: c31a2dd686ea1b0824e7e695157adbc219d9074f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2019
ms.locfileid: "54401984"
---
# <a name="how-azure-machine-learning-service-works-architecture-and-concepts"></a>Cómo funciona Azure Machine Learning Service: Arquitectura y conceptos

En este artículo se describen la arquitectura y los conceptos de Azure Machine Learning Service. En el siguiente diagrama se muestran los componentes principales del servicio y se ilustra el flujo de trabajo general al usar el servicio: 

[![Arquitectura y flujo de trabajo de Azure Machine Learning Service](./media/concept-azure-machine-learning-architecture/workflow.png)](./media/concept-azure-machine-learning-architecture/workflow.png#lightbox)

Normalmente, el flujo de trabajo sigue estos pasos:

1. Los scripts de entrenamiento del aprendizaje automático se desarrollan en **Python**.
1. Se crea y configura un **destino de proceso**.
1. **Se envían los scripts** al destino de proceso configurado para ejecutarse en ese entorno. Durante el entrenamiento, los scripts pueden leer o escribir en el **almacén de datos**. Y los registros de ejecución se guardan como **ejecuciones** en el **área de trabajo**, agrupados en **experimentos**.
1. **Se consulta el experimento** para las métricas registradas en ejecuciones actuales y anteriores. Si las métricas no ofrecen el resultado deseado, se vuelve en bucle al paso 1 y se repiten los scripts.
1. Cuando se encuentra una ejecución satisfactoria, se registra el modelo guardado en el **registro de modelos**.
1. Se desarrolla un script de puntuación.
1. **Se crea una imagen** y se registra en el **registro de imágenes**. 
1. **Se implementa la imagen** como un **servicio web** en Azure.


> [!NOTE]
> Aunque en este artículo se definen los términos y conceptos que usa Azure Machine Learning Service, no se definen los términos y conceptos de la plataforma Azure. Para obtener más información sobre la terminología de la plataforma Azure, consulte el [glosario de Microsoft Azure](https://docs.microsoft.com/azure/azure-glossary-cloud-terminology).

## <a name="workspace"></a>Área de trabajo

El área de trabajo es el recurso de nivel superior de Azure Machine Learning Service. Proporciona un lugar centralizado para trabajar con todos los artefactos que cree al usar Azure Machine Learning Service.

El área de trabajo conserva una lista de destinos de proceso que se puede usar para entrenar el modelo. También conserva un historial de las ejecuciones del entrenamiento, que incluye registros, métricas, resultados y una instantánea de sus scripts. Esta información se usa para determinar qué ejecución de entrenamiento crea el mejor modelo.

Los modelos se registran con el área de trabajo. Debe usar un modelo registrado y los scripts de puntuación para crear una imagen. A continuación, la imagen se puede implementar en Azure Container Instances, en Azure Kubernetes Service o en una matriz de puerta programable en el campo (FPGA) como un punto de conexión HTTP basado en REST. También puede implementar la imagen en un dispositivo de Azure IoT Edge como un módulo.

Puede crear varias áreas de trabajo, y cada área de trabajo se puede compartir entre varias personas. Al compartir un área de trabajo, puede controlar el acceso a la misma si asigna los siguientes roles a los usuarios:

* Propietario
* Colaborador
* Lector

Al crear una nueva área de trabajo, se crean automáticamente varios recursos de Azure que el área de trabajo utiliza:

* [Azure Container Registry](https://azure.microsoft.com/services/container-registry/): registra los contenedores de docker que usa durante el entrenamiento y al implementar un modelo.
* [Cuenta de Azure Storage](https://azure.microsoft.com/services/storage/): se usa como almacén de datos predeterminado para el área de trabajo.
* [Azure Application Insights](https://azure.microsoft.com/services/application-insights/): almacena información sobre la supervisión de los modelos.
* [Azure Key Vault](https://azure.microsoft.com/services/key-vault/): almacena secretos que usan los destinos de proceso y otra información confidencial que el área de trabajo necesita.

> [!NOTE]
> Además de crear nuevas versiones, también puede usar los servicios de Azure existentes. 

El diagrama siguiente es una taxonomía del área de trabajo:

[![Taxonomía del área de trabajo](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.svg)](./media/concept-azure-machine-learning-architecture/azure-machine-learning-taxonomy.png#lightbox)

## <a name="model"></a>Modelo

En su forma más simple, un modelo es un fragmento de código que toma una entrada y genera una salida. El hecho de crear un modelo de Machine Learning implica seleccionar un algoritmo, proporcionarle datos y ajustar los hiperparámetros. El entrenamiento es un proceso iterativo que genera un modelo entrenado, que encapsula lo que el modelo ha aprendido durante el proceso de aprendizaje.

Un modelo se genera mediante una ejecución en Azure Machine Learning. También puede usar un modelo entrenado fuera de Azure Machine Learning. Puede registrar el modelo en un área de trabajo de Azure Machine Learning Service.

Recuerde que Azure Machine Learning Service es independiente del marco de trabajo. Cuando cree un modelo, podrá usar cualquier marco de aprendizaje automático conocido, como Scikit-learn, XGBoost, PyTorch, TensorFlow, Chainer y Microsoft Cognitive Toolkit (antes conocido como CNTK).

Para obtener un ejemplo de entrenamiento de un modelo, consulte el documento [Inicio rápido: Creación de un área de trabajo de Machine Learning Service](quickstart-get-started.md).

### <a name="model-registry"></a>Registro de modelos

El registro de modelo realiza un seguimiento de todos los modelos del área de trabajo de Azure Machine Learning Services. 

Los modelos se identifican por el nombre y la versión. Cada vez que registra un modelo con el mismo nombre que uno ya existente, el registro interpreta que se trata de una nueva versión. La versión se incrementa y el nuevo modelo se registra con el nombre en cuestión.

Una vez registrado el modelo, puede proporcionar etiquetas de metadatos adicionales y, después, usar esas etiquetas al buscar modelos.

No se pueden eliminar los modelos que una imagen esté utilizando.

Para obtener un ejemplo de registro de un modelo, consulte [Entrenamiento de un modelo de clasificación de imágenes con Azure Machine Learning](tutorial-train-models-with-aml.md).

## <a name="image"></a>Imagen

Las imágenes ofrecen una forma fiable de implementar un modelo, junto con todos los componentes necesarios para usar el mismo. Una imagen contiene los siguientes elementos:

* Un modelo.
* Una aplicación o un script de puntuación. Este script se usa para pasar la entrada del modelo y devolver el resultado del mismo.
* Las dependencias que necesita el modelo, el script de puntuación o la aplicación. Por ejemplo, puede incluir un archivo de entorno de Conda en el que se detallen las dependencias de paquetes de Python.

Azure Machine Learning puede crear dos tipos de imágenes:

* **Imagen de FPGA**: se usa al realizar una implementación en una matriz de puerta programable de campo en Azure.
* **Imagen de Docker**: se usa al realizar implementaciones en destinos de proceso que no sean FPGA. Por ejemplo, Azure Container Instances y Azure Kubernetes Service.

Para obtener un ejemplo de creación de una imagen, consulte [Implementación de un modelo de clasificación de imágenes en Azure Container Instances](tutorial-deploy-models-with-aml.md).

### <a name="image-registry"></a>Registro de imágenes

El registro de imágenes realiza un seguimiento de las imágenes creadas a partir de sus modelos. Puede proporcionar etiquetas de metadatos adicionales al crear la imagen. El registro de imágenes almacena las etiquetas de metadatos que se pueden consultar para encontrar la imagen.

## <a name="deployment"></a>Implementación

Una implementación es una instancia de la imagen en un servicio web que puede hospedarse en la nube o un módulo de IoT para las implementaciones de dispositivos integrados. 

### <a name="web-service"></a>Servicio web

Un servicio web implementado puede usar Azure Container Instances, Azure Kubernetes Service o FPGA. El servicio se crea a partir de una imagen que encapsula el modelo, los scripts y los archivos asociados. La imagen tiene un punto de conexión HTTP de carga equilibrada que recibe solicitudes de puntuación que se envían al servicio web.

Azure le ayuda a supervisar la implementación del servicio web mediante la recopilación de telemetría de Application Insight o telemetría de modelos, si es que ha optado por habilitar esta característica. Solo usted puede obtener acceso a los datos de telemetría, que se almacenan en las instancias de su cuenta de almacenamiento y Application Insights.

Si ha habilitado el ajuste automático de escala, Azure ajustará automáticamente la escala de su implementación.

Para obtener un ejemplo de implementación de un modelo como servicio web, consulte [Implementación de un modelo de clasificación de imágenes en Azure Container Instances](tutorial-deploy-models-with-aml.md).

### <a name="iot-module"></a>Módulo de IoT

Un módulo de IoT implementado es un contenedor de Docker que incluye el modelo y el script asociado o la aplicación y las dependencias adicionales. Estos módulos se implementan con Azure IoT Edge en dispositivos Edge. 

Si ha habilitado la supervisión, Azure recopila datos de telemetría desde el modelo que está en el módulo de Azure IoT Edge. Solo usted puede obtener acceso a los datos de telemetría, que se almacenan en las instancias de su cuenta de almacenamiento.

Azure IoT Edge garantiza que el módulo se esté ejecutando y supervisa el dispositivo que lo hospeda.

## <a name="datastore"></a>Almacén de datos

Un almacén de datos es una abstracción de almacenamiento en una cuenta de Azure Storage. El almacén de datos puede usar un contenedor de blobs de Azure o un recurso compartido de archivos de Azure como almacenamiento back-end. Cada área de trabajo tiene un almacén de datos predeterminado en el que puede registrar almacenes de datos adicionales. 

Use la API del SDK de Python o la CLI de Azure Machine Learning para almacenar y recuperar archivos desde el almacén de datos. 

## <a name="run"></a>Ejecute

Una ejecución es un registro que contiene la información siguiente:

* Metadatos sobre la ejecución (marca de tiempo, duración, etc.).
* Métricas que el script registra.
* Archivos de salida que recopila automáticamente el experimento o que carga explícitamente usted mismo.
* Instantánea del directorio que contiene los scripts antes de la ejecución.

Una ejecución se produce cuando se envía un script para entrenar un modelo. Una ejecución puede tener cualquier número de ejecuciones secundarias. Por ejemplo, la ejecución de nivel superior puede tener dos ejecuciones secundarias, cada una de las cuales puede tener sus propias ejecuciones secundarias.

Para obtener un ejemplo de las vistas de ejecución que se producen al entrenar un modelo, consulte [Inicio rápido: Introducción a Azure Machine Learning Service](quickstart-get-started.md).

## <a name="experiment"></a>Experimento

Un experimento es una agrupación de varias ejecuciones de un script determinado. Siempre pertenece a un área de trabajo. Cuando envíe una ejecución, proporcione un nombre de experimento. La información de la ejecución se almacena en ese experimento. Si envía una ejecución y especifica un nombre de experimento que no existe, se crea automáticamente un experimento nuevo con ese nombre.

Para obtener un ejemplo del uso de un experimento, consulte [Inicio rápido: Introducción a Azure Machine Learning Service](quickstart-get-started.md).

## <a name="pipeline"></a>Canalización

Las canalizaciones de aprendizaje automático se usan para crear y administrar flujos de trabajo que unen las fases de aprendizaje automático. Por ejemplo, una canalización podría incluir las fases de preparación de los datos, entrenamiento del modelo, implementación de modelo e inferencia. Cada fase puede estar formada por varios pasos, cada uno de los cuales puede ejecutarse en modo desatendido en varios destinos de proceso.

Para obtener más información sobre las canalizaciones de aprendizaje automático con este servicio, consulte el artículo [Canalizaciones y Azure Machine Learning](concept-ml-pipelines.md).

## <a name="compute-target"></a>Destino de proceso

Un destino de proceso es el recurso de proceso que se usa para ejecutar el script de entrenamiento o para hospedar la implementación del servicio web. Los destinos de proceso admitidos son los siguientes: 

| Destino de proceso | Cursos | Implementación |
| ---- |:----:|:----:|
| Equipo local | ✓ | &nbsp; |
| Proceso de Azure Machine Learning | ✓ | &nbsp; |
| Una única máquina virtual Linux en Azure</br>(como Data Science Virtual Machine) | ✓ | &nbsp; |
| Azure Databricks | ✓ | &nbsp; | &nbsp; |
| Análisis con Azure Data Lake | ✓ | &nbsp; |
| Apache Spark para HDInsight | ✓ | &nbsp; |
| Azure Container Instances | &nbsp; | ✓ |
| Azure Kubernetes Service | &nbsp; | ✓ |
| Azure IoT Edge | &nbsp; | ✓ |
| Project Brainwave</br>(matriz de puertas programables por campo) | &nbsp; | ✓ |

Los destinos de proceso están asociados a un área de trabajo. Los usuarios del área de trabajo comparten los destinos de proceso que no sean el equipo local.

### <a name="managed-and-unmanaged-compute-targets"></a>Destinos de proceso administrados y no administrados

* **Administrado**: destinos de proceso que crea y administra Azure Machine Learning Service. Estos destinos de proceso están optimizados para cargas de trabajo de Machine Learning. El proceso de Azure Machine Learning es el único destino de proceso administrado desde el 4 de diciembre de 2018. En el futuro pueden agregarse destinos de proceso administrados adicionales. 

    Puede crear instancias del proceso de Machine Learning directamente en el área de trabajo si usa Azure Portal, el SDK de Azure Machine Learning o la CLI de Azure. Todos los demás destinos de proceso deben crearse fuera del área de trabajo y adjuntarse posteriormente.

* **No administrado**: destinos de proceso que *no* administra Azure Machine Learning Service. Es posible que tenga que crearlos fuera de Azure Machine Learning y, después, conectarlos al área de trabajo antes de usarlos. Estos destinos de proceso no administrados pueden requerir pasos adicionales para mantener o mejorar el rendimiento de las cargas de trabajo de Machine Learning.

Para obtener información sobre cómo seleccionar un destino de proceso para el aprendizaje, consulte [Selección y uso de un destino de proceso para entrenar el modelo](how-to-set-up-training-targets.md).

Para obtener información sobre cómo seleccionar un destino de proceso para la implementación, consulte [Implementación de modelos con Azure Machine Learning Service](how-to-deploy-and-where.md).

## <a name="run-configuration"></a>Configuración de ejecución

Una configuración de ejecución es un conjunto de instrucciones que define cómo se debe ejecutar un script en un destino de proceso determinado. Esta configuración incluye un amplio conjunto de definiciones de comportamiento como, por ejemplo, si quiere usar un entorno de Python existente o un entorno de Conda creado a partir de la especificación.

Una configuración de ejecución puede conservarse en un archivo del directorio que contiene el script de entrenamiento o puede crearse como un objeto en la memoria y usarse para enviar una ejecución.

Para ver configuraciones de ejecución de ejemplo, consulte [Selección y uso de un destino de proceso para entrenar el modelo](how-to-set-up-training-targets.md).

## <a name="training-script"></a>Script de entrenamiento

Para entrenar un modelo, especifique el directorio que contiene el script de entrenamiento y los archivos asociados. También puede especificar un nombre del experimento que se usará para almacenar la información recopilada durante el entrenamiento. Durante dicho proceso de entrenamiento, se copia todo el directorio en el entorno de entrenamiento (destino de proceso) y se inicia el script que especifique la configuración de ejecución. También se almacena una instantánea del directorio en el experimento del área de trabajo.

Puede encontrar un ejemplo en [Creación de un área de trabajo con Python](quickstart-get-started.md).

## <a name="logging"></a>Registro

Al desarrollar la solución, use el SDK de Python de Azure Machine Learning en el script de Python para registrar las métricas arbitrarias. Después de la ejecución, consulte las métricas para determinar si la ejecución ha generado el modelo que quiere implementar. 

## <a name="snapshot"></a>Instantánea

Al enviar una ejecución, Azure Machine Learning comprime el directorio que contiene el script como un archivo zip y lo envía al destino de proceso. A continuación, el archivo .zip se extrae y el script se ejecuta. Azure Machine Learning también almacena el archivo .zip como una instantánea como parte del registro de ejecución. Cualquier persona con acceso al área de trabajo puede buscar un registro de ejecución y descargar la instantánea.

## <a name="activity"></a>Actividad

Una actividad representa una operación de larga ejecución. Las operaciones siguientes son ejemplos de actividades:

* Creación o eliminación de un destino de proceso
* Ejecución de un script en un destino de proceso

Las actividades pueden proporcionar notificaciones a través del SDK o la interfaz de usuario web para que pueda supervisar fácilmente el progreso de estas operaciones.

## <a name="next-steps"></a>Pasos siguientes

Para comenzar a usar Azure Machine Learning Service, consulte:

* [¿Qué es Azure Machine Learning Service?](overview-what-is-azure-ml.md)
* [Inicio rápido: Creación de un área de trabajo con Python](quickstart-get-started.md)
* [Tutorial: Entrenamiento de un modelo](tutorial-train-models-with-aml.md)
