---
title: Migración al servicio Azure Machine Learning
description: Aprenda a actualizar o migrar a la versión más reciente del servicio Azure Machine Learning desde una versión anterior.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: haining
author: haining
ms.date: 09/24/2018
ms.openlocfilehash: 9778d348cf49d4066f034931dc350a1f4a608ad2
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2018
ms.locfileid: "48236591"
---
# <a name="migrate-to-the-latest-version-of-azure-machine-learning-service"></a>Migración a la versión más reciente del servicio Azure Machine Learning 

**Si ha instalado la aplicación Workbench (versión preliminar) o tiene cuentas de la versión preliminar para la administración de modelos o de experimentación, use este artículo para migrar a la versión más reciente.**  Si no tiene instalada la versión preliminar de Workbench ni una cuenta de administración de modelos o de experimentación, no es necesario migrar nada.

## <a name="what-can-i-migrate"></a>¿Qué puedo migrar?
La mayoría de los artefactos creados en la primera versión preliminar del servicio Azure Machine Learning se almacenan en su propio almacenamiento local o en la nube. Estos artefactos no desaparecerán. Para migrar, registre los artefactos con el servicio de Azure Machine Learning actualizado. 

En la tabla y el artículo siguientes se explica lo que puede hacer con los recursos actuales antes o después de pasar a la versión más reciente del servicio Azure Machine Learning. También puede seguir utilizando la versión anterior y sus recursos durante algún tiempo ([consulte la escala de tiempo de compatibilidad de la transición](overview-what-happened-to-workbench.md#timeline)).

|Recursos de una versión antigua|¿Puedo migrarlos?|Acciones|
|-----------------|:-------------:|-------------|
|Modelos de aprendizaje automático (como archivos locales)|SÍ|Ninguno. Funciona igual que antes.|
|Esquemas y dependencias de modelos (como archivos locales)|SÍ|Ninguno. Funciona igual que antes.|
|Proyectos|SÍ|[Asocie la carpeta local a la nueva área de trabajo](#projects).|
|Historiales de ejecución|Sin |[Se puede descargar](#history) durante un tiempo.|
|Archivos de preparación de datos|Sin |[Prepare un conjunto de datos de cualquier tamaño](#dataprep) para el modelado con el nuevo SDK de preparación de datos de Azure Machine Learning o use Azure Databricks.|
|Destinos de proceso|Sin |Regístrelos en el nuevo área de trabajo.|
|Modelos registrados|Sin |Vuelva a registrar el modelo en un área de trabajo.|
|Manifiestos registrados|Sin |Ninguno. Los manifiestos ya no existen como un concepto en el área de trabajo nueva.|
|Imágenes registradas|Sin |Vuelva a crear la imagen de Docker de la implementación en una nueva área de trabajo.|
|Servicios web implementados|Sin |Ninguno. Seguirán funcionando tal cual <br/>o bien [impleméntelos de nuevo con la versión más reciente](#services).|
|Experimentación y <br/>Cuentas de administración de modelos|Sin |[Cree una área de trabajo](#resources) en su lugar.|
|CLI y SDK de Machine Learning|Sin |Use los nuevos [CLI](reference-azure-machine-learning-cli.md) y [SDK](http://aka.ms/aml-sdk) para el nuevo trabajo.|


Más información acerca de lo [que cambió en esta versión](overview-what-happened-to-workbench.md).

>[!Warning]
>Este artículo no es para usuarios de Azure Machine Learning Studio. Es para clientes del servicio Azure Machine Learning que tengan instalada la aplicación Workbench (versión preliminar) o dispongan de cuentas de la versión preliminar de administración de modelos o de experimentación.

<a name="resources"></a>

## <a name="azure-resources"></a>Recursos de Azure

Recursos como las cuentas de experimentación, las cuentas de administración de modelos y los entornos de proceso del aprendizaje automático no se pueden migrar a la versión más reciente del servicio Azure Machine Learning. Consulte en la [escala de tiempo](overview-what-happened-to-workbench.md#timeline) cuánto tiempo seguirán funcionando los recursos.

Introducción a la versión más reciente mediante la creación de un área de trabajo del servicio Azure Machine Learning en [Azure Portal](quickstart-get-started.md). El panel del área de trabajo del portal solo es compatible con los exploradores Edge, Chrome y Firefox.

Esta nueva área de trabajo es el recurso de servicio de nivel superior y le permite usar todas las características más recientes del servicio Azure Machine Learning. Más información sobre esta [área de trabajo y esta arquitectura](concept-azure-machine-learning-architecture.md).

<a name="projects"></a>

## <a name="projects"></a>Proyectos

En lugar de tener sus proyectos en un área de trabajo en la nube, ahora son directorios en el equipo local en la versión más reciente. Consulte un diagrama de la [arquitectura más reciente](concept-azure-machine-learning-architecture.md). 

Para continuar usando el directorio local que contiene los archivos y scripts, especifique el nombre del directorio en el comando de Python ['experiment.submit'](http://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py) o con el comando de la CLI "az ml project attach".

Por ejemplo: 
```python
run = exp.submit(source_directory = script_folder, script = 'train.py', run_config = run_config_system_managed)
```

<a name="services"></a>

## <a name="deployed-web-services"></a>Servicios web implementados

Para migrar los servicios web, vuelva a implementar los modelos mediante el nuevo SDK o la CLI en los nuevos destinos de implementación. No es necesario cambiar su archivo original de puntuación, los archivos de dependencias de archivos de modelo, el archivo de entorno ni los archivos de esquema. 

En la versión más reciente, los modelos se implementan como servicios web para clústeres de [Azure Container Instances](how-to-deploy-to-aci.md) (ACI) o [Azure Kubernetes Service](how-to-deploy-to-aks.md) (AKS). 

Para más información, consulte estos artículos:
+ [Implementación en ACI](how-to-deploy-to-aci.md)
+ [Implementación en AKS](how-to-deploy-to-aks.md)
+ [Tutorial: Implementación de modelos con el servicio Azure Machine Learning](tutorial-deploy-models-with-aml.md)

Cuando la [compatibilidad con la CLI finalice](overview-what-happened-to-workbench.md#timeline), no podrá administrar los servicios web que implementó originalmente con su cuenta de Administración de modelos de Machine Learning. Sin embargo, dichos servicios web seguirán funcionando siempre y cuando todavía se admita Azure Container Service (ACS).

<a name="history"></a>

## <a name="run-history-records"></a>Registros del historial de ejecución

Si bien no puede continuar agregando a los historiales de ejecución existentes en el área de trabajo anterior, puede usar la CLI anterior para exportar los historiales que tenga. Cuando la [compatibilidad con la CLI anterior finalice](overview-what-happened-to-workbench.md#timeline), no podrá volver a exportar estos historiales de ejecución.

Empiece a entrenar los modelos y a seguir los historiales de ejecución con la nueva CLI y el SDK. Puede obtener más información en el [Tutorial: Entrenamiento de modelos con el servicio Azure Machine Learning](tutorial-train-models-with-aml.md).

Para exportar el historial de ejecución con la CLI anterior:

```azurecli
#list all runs
az ml history list

#get details about a particular run
az ml history info

# download all artifacts of a run
az ml history download
```

<a name="dataprep"></a>

## <a name="data-preparation-files"></a>Archivos de preparación de los datos
Los archivos de preparación de los datos no son portables sin Workbench. Sin embargo, puede seguir preparando un conjunto de datos de cualquier tamaño para el modelado con el nuevo SDK de preparación de datos de Azure Machine Learning o bien usar Azure Databricks para conjuntos de macrodatos.  [Obtenga información acerca de cómo obtener el SDK de preparación de datos](how-to-data-prep.md). 

## <a name="next-steps"></a>Pasos siguientes

Para ver una guía de inicio rápido que muestre cómo crear un área de trabajo y un proyecto, ejecutar un script y explorar el historial de ejecución del script con la versión más reciente del servicio Azure Machine Learning, consulte [Introducción al servicio Azure Machine Learning](quickstart-get-started.md).

Para obtener una experiencia pormenorizada de este flujo de trabajo, siga el tutorial completo que contiene los pasos detallados para el entrenamiento y la implementación de modelos con el servicio Azure Machine Learning. 

> [!div class="nextstepaction"]
> [Tutorial: Entrenamiento e implementación de modelos](tutorial-train-models-with-aml.md)
