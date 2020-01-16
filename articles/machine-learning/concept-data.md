---
title: Datos en Azure Machine Learning
titleSuffix: Azure Machine Learning
description: Aprenda cómo Azure Machine Learning interactúa con los datos y cómo se usa en los experimentos de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: nibaccam
author: nibaccam
ms.author: nibaccam
ms.date: 12/09/2019
ms.openlocfilehash: 4149e90e07bbcd03a0df41060b42b8902b89e774
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/28/2019
ms.locfileid: "75535740"
---
# <a name="data-access-in-azure-machine-learning"></a>Acceso a los datos en Azure Machine Learning

En este artículo obtendrá información sobre la administración de datos y las soluciones de integración de Azure Machine Learning para las tareas de aprendizaje automático. En este artículo se supone que ya ha creado una [cuenta de almacenamiento de Azure](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal) y un [servicio de almacenamiento de Azure](https://docs.microsoft.com/azure/storage/common/storage-introduction).

Cuando esté listo para usar los datos en el almacenamiento, se recomienda realizar lo siguiente:

1. Cree un almacén de datos de Azure Machine Learning.
2. A partir del almacén de datos, cree un conjunto de datos de Azure Machine Learning. 
3. Use ese conjunto de datos en el experimento de aprendizaje automático; puede: 
    1. Montarlo en el destino de proceso del experimento para el entrenamiento del modelo

        **OR** 

    1. Consumirlo directamente en soluciones de Azure Machine Learning como las ejecuciones de experimentos de aprendizaje automático automatizado (ML automatizado), las canalizaciones de aprendizaje automático y el [diseñador de Azure Machine Learning](concept-designer.md).
4. Cree monitores de conjunto de datos para el conjunto de datos de salida del modelo con el fin de detectar desfases de datos. 
5. Si se detecta un desfase de datos, actualice el conjunto de datos de entrada y vuelva a entrenar el modelo como corresponde.

En el siguiente diagrama se ofrece una demostración visual de este flujo de trabajo recomendado para el acceso a los datos.

![Diagrama de concepto de datos](./media/concept-data/data-concept-diagram.svg)

## <a name="access-data-in-storage"></a>Acceder a los datos en el almacenamiento

Para acceder a los datos de la cuenta de almacenamiento, Azure Machine Learning ofrece almacenes y conjuntos de datos. Los almacenes de datos responden a la pregunta: ¿cómo me conecto de forma segura a mis datos que están en mi instancia de Azure Storage? Los almacenes de datos proporcionan un nivel de abstracción en el servicio de almacenamiento. Esto contribuye a la seguridad y la facilidad de acceso al almacenamiento, ya que la información de conexión se mantiene en el almacén de datos y no se expone en los scripts. 

Los conjuntos de datos responden a la pregunta: ¿cómo obtengo archivos de datos específicos en mi almacén de datos? Los conjuntos de datos apuntan al archivo o archivos específicos del almacenamiento subyacente que se desea usar para el experimento de aprendizaje automático. Juntos, los almacenes y los conjuntos de datos ofrecen un flujo de trabajo de entrega de datos seguro, escalable y reproducible para las tareas de aprendizaje automático.

### <a name="datastores"></a>Almacenes de datos

Un almacén de datos de Azure Machine Learning es una abstracción de almacenamiento en los servicios de almacenamiento de Azure. [Registre y cree un almacén de datos](how-to-access-data.md) para conectarse fácilmente a su cuenta de almacenamiento de Azure y acceder a los datos de los servicios de almacenamiento de Azure subyacentes.

A continuación se indican los servicios de almacenamiento de Azure compatibles que se pueden registrar como almacenes de datos:
+ Azure Blob Container
+ Recurso compartido de archivos de Azure
+ Azure Data Lake
+ Azure Data Lake Gen2
+ Azure SQL Database
+ Azure Database for PostgreSQL
+ Sistema de archivos de Databricks
+ Azure Database for MySQL

### <a name="datasets"></a>Conjuntos de datos

[Cree un conjunto de datos de Azure Machine Learning](how-to-create-register-datasets.md) para interactuar con los datos de los almacenes de datos y para empaquetar los datos en un objeto consumible para las tareas de aprendizaje automático. Registre el conjunto de datos en el área de trabajo para compartirlo y reutilizarlo en distintos experimentos sin las complejidades de la ingesta de datos.

Los conjuntos de datos se pueden crear a partir de archivos locales, direcciones URL públicas, instancias de [Azure Open Datasets](#open) o archivos específicos de los almacenes de datos. Para crear un conjunto de datos a partir de un dataframe de Pandas en memoria, escriba los datos en un archivo local —como un archivo CSV— y cree el conjunto de datos a partir de ese archivo. Los conjuntos de datos no son copias de los datos, sino referencias que apuntan a los datos del servicio de almacenamiento, por lo que no se genera ningún costo de almacenamiento adicional. 

En el siguiente diagrama se muestra que, si no se dispone de un servicio de almacenamiento de Azure, se puede crear un conjunto de datos directamente a partir de archivos locales, direcciones URL públicas o instancias de Azure Open DataSets. Al hacerlo, se conectará el conjunto de datos al almacén de datos predeterminado que se creó automáticamente con el [área de trabajo de Azure Machine Learning](concept-workspace.md) del experimento.

![Diagrama de concepto de datos](./media/concept-data/dataset-workflow.svg)

En la siguiente documentación se pueden encontrar funcionalidades adicionales de los conjuntos de datos:

+ [Versión y seguimiento](how-to-version-track-datasets.md) del linaje del conjunto de datos.
+ [Supervisión del conjunto de datos](how-to-monitor-datasets.md) para ayudar con la detección de desfases de datos.
+  Consulte las siguientes referencias para obtener información sobre los dos tipos de conjuntos de datos:
    + [TabularDatasets](https://docs.microsoft.com/python/api/azureml-core/azureml.data.tabulardataset?view=azure-ml-py) representa los datos en formato tabular mediante el análisis del archivo o la lista de archivos proporcionados. Permite materializar los datos en dataframes de Pandas o Spark para una mayor manipulación y limpieza. Para una lista completa de los archivos a partir de los cuales se pueden crear objetos TabularDataset, consulte la clase [TabularDatasetFactory](https://aka.ms/tabulardataset-api-reference).

    + [FileDataset](https://docs.microsoft.com/python/api/azureml-core/azureml.data.file_dataset.filedataset?view=azure-ml-py) hace referencia a uno o varios archivos de los almacenes de datos o direcciones URL públicas. Con este método, se pueden descargar o montar los archivos que se prefieran en el destino de proceso como un objeto FileDataset.

## <a name="work-with-your-data"></a>Trabajo con los datos

Con los conjuntos de datos puede realizar una serie de tareas de aprendizaje automático, gracias a la perfecta integración con las características de Azure Machine Learning. 

+ [Entrenar modelos de Machine Learning](how-to-train-with-datasets.md).
+ Consumir conjuntos de datos en 
     + [experimentos de ML automatizado](how-to-create-portal-experiments.md)
     + el [diseñador](tutorial-designer-automobile-price-train-score.md#import-data) 
+ Acceder a los conjuntos de datos para la puntuación con la inferencia por lotes en [canalizaciones de aprendizaje automático](how-to-create-your-first-pipeline.md).
+ Crear un [proyecto de etiquetado de datos](#label).
+ Configurar un monitor de conjunto de datos para la detección de [desfase de datos](#drift).

<a name="open"></a>

## <a name="azure-open-datasets"></a>Conjuntos de datos abiertos de Azure

[Azure Open Datasets](how-to-create-register-datasets.md#create-datasets-with-azure-open-datasets) son conjuntos de datos públicos mantenidos que puede usar para agregar características de escenarios específicos a soluciones de aprendizaje automático a fin de obtener modelos más precisos. Las instancias de Azure Open Datasets se encuentran en la nube en Microsoft Azure y se integran en Azure Machine Learning. También puede acceder a los conjuntos de datos a través de distintas API y usarlos en otros productos, como Power BI y Azure Data Factory.

En Azure Open Datasets se incluyen datos de dominio público sobre el clima, censos, días festivos, seguridad pública y ubicación, que le ayudarán a entrenar los modelos de Machine Learning y enriquecer las soluciones predictivas. También puede compartir sus conjuntos de datos públicos en Azure Open Datasets.

<a name="label"></a>

## <a name="data-labeling"></a>Etiquetado de datos

El etiquetado de grandes cantidades de datos a menudo ha resultado un dolor de cabeza en los proyectos de aprendizaje automático. Los que incluyen un componente de Computer Vision, como la clasificación de imágenes o la detección de objetos, normalmente requieren miles de imágenes y sus etiquetas correspondientes.

Azure Machine Learning le proporciona una ubicación central para crear, administrar y supervisar proyectos de etiquetado. Los proyectos de etiquetado ayudan a coordinar los datos, las etiquetas y los miembros del equipo, lo que le permite administrar de forma más eficaz las tareas de etiquetado. Actualmente, las tareas admitidas son la clasificación de imágenes (de varias etiquetas y de varias clases) y la identificación de objetos mediante cuadros de límite.

+ Cree un [proyecto de etiquetado de datos](how-to-create-labeling-projects.md) y genere un conjunto de datos para su uso en experimentos de aprendizaje automático.

<a name="drift"></a>

## <a name="data-drift"></a>Desfase de datos

En el contexto de aprendizaje automático, el desfase de datos es el cambio en los datos de entrada del modelo que conduce a la degradación del rendimiento del modelo. Es uno de los principales motivos por los que la precisión del modelo se degrada con el tiempo. Por lo tanto, la supervisión del desfase de datos ayuda a detectar problemas de rendimiento del modelo.
Consulte el artículo sobre [creación de monitores de conjunto de datos](how-to-monitor-datasets.md) para obtener información sobre cómo detectar y alertar sobre el desfase de datos en los nuevos datos de un conjunto de datos.

## <a name="next-steps"></a>Pasos siguientes 

+ Cree un conjunto de datos en Azure Machine Learning Studio o con el SDK de Python [mediante estos pasos](how-to-create-register-datasets.md).
+ Pruebe los ejemplos de entrenamiento del conjunto de datos con nuestros [cuadernos de ejemplo](https://aka.ms/dataset-tutorial).
+ Para ver ejemplos de desfase de datos, consulte este [tutorial de desfase de datos](https://aka.ms/datadrift-notebook).
