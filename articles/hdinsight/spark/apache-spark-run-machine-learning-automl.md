---
title: Ejecución de cargas de trabajo de Azure Machine Learning con aprendizaje automático automatizado (AutoML) en Apache Spark en Azure HDInsight
description: Aprenda a ejecutar cargas de trabajo de Azure Machine Learning con aprendizaje automático automatizado (AutoML) en Apache Spark en Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/14/2019
ms.openlocfilehash: 5135de0fc87af227073f96c653d928ace1a50fd0
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64917052"
---
# <a name="run-azure-machine-learning-workloads-with-automated-machine-learning-automl-on-apache-spark-in-azure-hdinsight"></a>Ejecución de cargas de trabajo de Azure Machine Learning con aprendizaje automático automatizado (AutoML) en Apache Spark en Azure HDInsight

Azure Machine Learning simplifica y acelera la creación, entrenamiento y la implementación de modelos de aprendizaje automático. En automatizadas machine learning (AutoML), comience con los datos de entrenamiento que tiene una característica de destino definido y, a continuación, recorrer en iteración las combinaciones de algoritmos y selecciones de características para seleccionar automáticamente el mejor modelo para los datos según las puntuaciones de entrenamiento. HDInsight permite a los clientes aprovisionen los clústeres con cientos de nodos. AutoML ejecutándose en Spark en un clúster de HDInsight permite a los usuarios usar la capacidad de proceso a través de estos nodos para ejecutar trabajos de entrenamiento en un modo de escalado horizontal y para ejecutar varios trabajos de entrenamiento en paralelo. Esto permite a los usuarios ejecutar experimentos AutoML mientras comparte el proceso con sus otras cargas de trabajo de macrodatos.
 

## <a name="install-azure-machine-learning-on-an-hdinsight-cluster"></a>Instalación de Azure Machine Learning en un clúster de HDInsight

Para obtener tutoriales generales del aprendizaje automático automatizadas, vea [Tutorial: Uso del aprendizaje automático para crear un modelo de regresión](../../machine-learning/service/tutorial-auto-train-models.md).
Todos los nuevos clústeres Spark de HDInsight vienen preinstalados con el SDK de Azure ml AutoML. Puede empezar a trabajar con AutoML en HDInsight con este [cuaderno de Jupyter de ejemplo](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/azure-hdi). Este cuaderno de Jupyter Notebook muestra cómo usar un clasificador de aprendizaje automático automatizado para solucionar un sencillo problema de clasificación.

> [!Note]
> Los paquetes de Azure Machine Learning se instalan en el entorno de Python 3 Conda. Se debe ejecutar el cuaderno de Jupyter Notebook instalado mediante el kernel PySpark3.

Como alternativa, puede usar cuadernos de Zeppelin para usar AutoML también.

> [!Note]
> Zeppelin tiene un [problema conocido](https://community.hortonworks.com/content/supportkb/207822/the-livypyspark3-interpreter-uses-python-2-instead.html) donde PySpark3 no elegir la versión correcta de Python. Use la solución documentada.

## <a name="authentication-for-workspace"></a>Autenticación de áreas de trabajo

La creación de un área de trabajo y el envío de experimentos requieren un token de autenticación. Este token se puede generar mediante una [aplicación de Azure AD](../../active-directory/develop/app-objects-and-service-principals.md). Un [usuario de Azure AD](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate?view=azure-python) también puede usarse para generar el token de autenticación necesario en caso de que no esté habilitada la autenticación multifactor en la cuenta.  

El siguiente fragmento de código crea un token de autenticación mediante una **aplicación de Azure AD**.

```python
from azureml.core.authentication import ServicePrincipalAuthentication
auth_sp = ServicePrincipalAuthentication(
                tenant_id = '<Azure Tenant ID>',
                service_principal_id = '<Azure AD Application ID>',
                service_principal_password = '<Azure AD Application Key>'
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

En el [configuración de aprendizaje de máquina automatizadas](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig), la propiedad `spark_context` debe establecerse para que el paquete se ejecute en modo distribuido. La propiedad `concurrent_iterations`, que es el número máximo de iteraciones que se ejecutan en paralelo, se debe establecer en un número menor que los núcleos de ejecutor de la aplicación de Spark.

## <a name="next-steps"></a>Pasos siguientes

* Para obtener más información sobre la motivación para aprendizaje automático automatizadas, consulte [modelos de versión al ritmo utilizando Microsoft automated aprendizaje automático.](https://azure.microsoft.com/blog/release-models-at-pace-using-microsoft-s-automl/)
* Para obtener más detalles sobre el uso de las capacidades de aprendizaje automático de Azure ML automatizadas, consulte [New automatizada capacidades de aprendizaje automático en el servicio de Azure Machine Learning](https://azure.microsoft.com/blog/new-automated-machine-learning-capabilities-in-azure-machine-learning-service/)
* [Proyecto de AutoML de Microsoft Research](https://www.microsoft.com/research/project/automl/)
