---
title: Ejecución de cargas de trabajo de Azure Machine Learning con aprendizaje automático automatizado (AutoML) en Apache Spark en Azure HDInsight
description: Aprenda a ejecutar cargas de trabajo de Azure Machine Learning con aprendizaje automático automatizado (AutoML) en Apache Spark en Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/14/2019
ms.openlocfilehash: 896cae9b7fc43765e340ba3b92351e04b5512efd
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/12/2019
ms.locfileid: "57762558"
---
# <a name="run-azure-machine-learning-workloads-with-automated-machine-learning-automl-on-apache-spark-in-azure-hdinsight"></a>Ejecución de cargas de trabajo de Azure Machine Learning con aprendizaje automático automatizado (AutoML) en Apache Spark en Azure HDInsight

Azure Machine Learning es una herramienta colaborativa de arrastrar y colocar que le permite crear, probar e implementar soluciones de análisis predictivo en sus datos. Azure Machine Learning publica modelos como servicios web que pueden utilizarse fácilmente en aplicaciones personalizadas o herramientas de BI como Excel. El aprendizaje automático automatizado (AutoML) ayuda a crear modelos de Machine Learning de alta calidad mediante una automatización y optimización inteligentes. AutoML decide el algoritmo correcto y los hiperparámetros que se usarán para los tipos de problemas concretos.

## <a name="install-azure-machine-learning-on-an-hdinsight-cluster"></a>Instalación de Azure Machine Learning en un clúster de HDInsight

> [!Note]
> El área de trabajo de Azure Machine Learning está actualmente disponible en las siguientes regiones: eastus, eastus2 y westcentralus. También se debe crear el clúster de HDInsight en una de estas regiones.

Para ver tutoriales generales sobre Azure Machine Learning y el aprendizaje automático automatizado, consulte [Tutorial: Creación del primer experimento de ciencia de datos en Azure Machine Learning Studio](../../machine-learning/studio/create-experiment.md) y [Tutorial: Uso del aprendizaje automático para crear un modelo de regresión](../../machine-learning/service/tutorial-auto-train-models.md).
Para instalar AzureML en el clúster de Azure HDInsight, ejecute la acción de script [install_aml](https://commonartifacts.blob.core.windows.net/automl/install_aml.sh) en los nodos principales y en los nodos de trabajo de un clúster de HDInsight 3.6 Spark 2.3.0 (recomendado). Esta acción de script se puede ejecutar como parte del proceso de creación del clúster, o bien en un clúster existente mediante Azure Portal.

Para más información sobre las acciones de script, lea [Personalización de clústeres de HDInsight basados en Linux mediante la acción de script](../hdinsight-hadoop-customize-cluster-linux.md). Junto con la instalación de los paquetes de Azure Machine Learning y las dependencias, el script también descarga un cuaderno de Jupyter Notebook (en la ruta `HdiNotebooks/PySpark` del almacén predeterminado). Este cuaderno de Jupyter Notebook muestra cómo usar un clasificador de aprendizaje automático automatizado para solucionar un sencillo problema de clasificación.

> [!Note]
> Los paquetes de Azure Machine Learning se instalan en el entorno de Python 3 Conda. Se debe ejecutar el cuaderno de Jupyter Notebook instalado mediante el kernel PySpark3.

## <a name="authentication-for-workspace"></a>Autenticación de áreas de trabajo

La creación de un área de trabajo y el envío de experimentos requieren un token de autenticación. Este token se puede generar mediante una [aplicación de Azure AD](../../active-directory/develop/app-objects-and-service-principals.md). Un [usuario de Azure AD](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate?view=azure-python) también puede usarse para generar el token de autenticación necesario en caso de que no esté habilitada la autenticación multifactor en la cuenta.  

El siguiente fragmento de código crea un token de autenticación mediante una **aplicación de Azure AD**.

```python
from azureml.core.authentication import ServicePrincipalAuthentication
auth_sp = ServicePrincipalAuthentication(
                tenant_id = '<Azure Tenant ID>',
                username = '<Azure AD Application ID>',
                password = '<Azure AD Application Key>'
                )
```
El siguiente fragmento de código crea un token de autenticación mediante un **usuario de Azure AD**.

```python
from azure.common.credentials import UserPassCredentials
credentials = UserPassCredentials('user@domain.com','my_smart_password')
```

## <a name="loading-dataset"></a>Carga de conjunto de datos

El aprendizaje automático automatizado en Spark usa **flujos de datos**, que son operaciones con evaluación diferida, inmutables, que se realizan en los datos.  Un flujo de datos puede cargar un conjunto de datos desde un blob con acceso de lectura público, o desde una dirección URL de un blob con un token de SAS.

```python
import azureml.dataprep as dprep

dataflow_public = dprep.read_csv(path='https://commonartifacts.blob.core.windows.net/automl/UCI_Adult_train.csv')

dataflow_with_token = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv?st=2018-06-15T23%3A01%3A42Z&se=2019-06-16T23%3A01%3A00Z&sp=r&sv=2017-04-17&sr=b&sig=ugQQCmeC2eBamm6ynM7wnI%2BI3TTDTM6z9RPKj4a%2FU6g%3D')
```

También puede registrar el almacén de datos con el área de trabajo mediante un registro de un solo uso.

## <a name="experiment-submission"></a>Realización de experimentos

En la configuración del aprendizaje automático automatizado, la propiedad `spark_context` debe establecerse para que el paquete se ejecute en modo distribuido. La propiedad `concurrent_iterations`, que es el número máximo de iteraciones que se ejecutan en paralelo, se debe establecer en un número menor que los núcleos de ejecutor de la aplicación de Spark.

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre los motivos para usar el aprendizaje automático automatizado, consulte [Release models at pace using Microsoft’s automated machine learning!](https://azure.microsoft.com/blog/release-models-at-pace-using-microsoft-s-automl/) (Lanzamiento de modelos a un ritmo mediante el aprendizaje automático automatizado de Microsoft).
* [Proyecto de AutoML de Microsoft Research](https://www.microsoft.com/research/project/automl/)