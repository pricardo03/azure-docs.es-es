---
title: ¿Qué ha ocurrido con Machine Learning Workbench?
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre lo que ocurrió con la aplicación Machine Learning Workbench, qué cambió en Azure Machine Learning Service y qué es la escala de tiempo del soporte técnico.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: overview
ms.reviewer: jmartens
author: j-martens
ms.author: jmartens
ms.date: 05/14/2019
ms.custom: seodec18
ms.openlocfilehash: 40a2f8716b438837cf3efe300286ab9053fc8b07
ms.sourcegitcommit: 94ee81a728f1d55d71827ea356ed9847943f7397
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2019
ms.locfileid: "70035827"
---
# <a name="what-happened-to-azure-machine-learning-workbench"></a>¿Qué ha ocurrido con Azure Machine Learning Workbench?

La aplicación Azure Machine Learning Workbench y algunas otras características tempranas han quedado en desuso y se han reemplazado en la versión de septiembre de 2018 para dar paso a una [arquitectura](concept-azure-machine-learning-architecture.md) mejorada.

Para mejorar la experiencia, la versión contiene muchas actualizaciones importantes solicitadas por los comentarios de los clientes. La funcionalidad básica de ejecuciones de experimento hasta la implementación de modelos no ha cambiado, pero ahora puede usar el <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> sólido y la [CLI de Azure](reference-azure-machine-learning-cli.md) para realizar sus tareas y canalizaciones de aprendizaje automático.

La mayoría de los artefactos creados en la versión anterior de Azure Machine Learning Service se almacenan en su propio almacenamiento local o en la nube. Estos artefactos nunca desaparecerán.

En este artículo obtendrá información sobre qué ha cambiado y cómo ello afecta al trabajo previamente existente con Azure Machine Learning Workbench y sus API.

>[!Warning]
>Este artículo no es para usuarios de Azure Machine Learning Studio. Es para clientes del servicio Azure Machine Learning que tengan instalada la aplicación Workbench (versión preliminar) o dispongan de cuentas de la versión preliminar de administración de modelos o de experimentación.


## <a name="what-changed"></a>¿Qué ha cambiado?

La versión más reciente de Azure Machine Learning Service incluye las características siguientes:
+ Un [modelo simplificado de los recursos de Azure](concept-azure-machine-learning-architecture.md).
+ Una [nueva interfaz de usuario del portal](how-to-track-experiments.md) para administrar sus experimentos y destinos de proceso.
+ Un <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> nuevo y más completo de Python.
+ La nueva [extensión de la CLI de Azure](reference-azure-machine-learning-cli.md) expandida para el aprendizaje automático.

La [arquitectura](concept-azure-machine-learning-architecture.md) se rediseñó para facilitar el uso. En lugar de varias cuentas y recursos de Azure, solo necesita un [área de trabajo del servicio Azure Machine Learning](concept-workspace.md). Puede crear áreas de trabajo rápidamente en [Azure Portal](how-to-manage-workspace.md). Al usar un área de trabajo, varios usuarios pueden almacenar destinos de proceso de entrenamiento e implementación, experimentos de modelos, imágenes de Docker, modelos implementados, etc.

Aunque hay nuevos clientes mejorados de CLI y SDK en la versión actual, se ha retirado la propia aplicación de escritorio Workbench. Se pueden administrar experimentos en el [panel del área de trabajo de Azure Portal](how-to-track-experiments.md#view-the-experiment-in-the-azure-portal). Use el panel para obtener el historial de experimentos, administrar los destinos de proceso asociados al área de trabajo, administrar los modelos e imágenes de Docker e incluso implementar servicios web.

<a name="timeline"></a>

## <a name="support-timeline"></a>Escala de tiempo del soporte técnico

El 9 de enero de 2019 ha finalizado la compatibilidad con las cuentas de Machine Learning Workbench, Experimentación de Azure Machine Learning y Administración de modelos, y con sus SDK y CLI asociados.

Todas las funcionalidades más recientes están disponibles si se usa el <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a>, la [CLI](reference-azure-machine-learning-cli.md) y el [portal](how-to-manage-workspace.md).

## <a name="what-about-run-histories"></a>¿Qué sucede con los historiales de ejecución?

Los historiales de ejecución anteriores ya no son accesibles, ¿cómo puede aún ver las ejecuciones en la última versión?

Los historiales de ejecución ahora se denominan **experimentos**. Puede recopilar los experimentos de su modelo y explorarlos mediante el SDK, la CLI o Azure Portal.

El panel del área de trabajo del portal solo es compatible con los exploradores Microsoft Edge, Chrome y Firefox:

[![Portal en línea](./media/overview-what-happened-to-workbench/image001.png)](./media/overview-what-happened-to-workbench/image001.png#lightbox)

Empiece a entrenar los modelos y a seguir los historiales de ejecución con la nueva CLI y el SDK. Puede obtener más información en el [Tutorial: Entrenamiento de modelos con el servicio Azure Machine Learning](tutorial-train-models-with-aml.md).

## <a name="can-i-still-prep-data"></a>¿Aún se puede preparar datos?

Los archivos de preparación de datos ya existentes no se pueden portar a la versión más reciente, puesto que ya no tenemos Machine Learning Workbench. Pero aún puede preparar conjuntos de datos de cualquier tamaño para el modelado.

Con los conjuntos de datos de cualquier tamaño puede usar el [paquete de preparación de datos para Azure Machine Learning](https://aka.ms/data-prep-sdk) para preparar rápidamente los datos antes del modelado mediante la escritura de código de Python.

## <a name="will-projects-persist"></a>¿Se conservarán los proyectos?

No perderá ningún código ni trabajo. En la versión anterior, los proyectos son entidades en la nube con un directorio local. En la versión más reciente, los directorios locales se asocian al área de trabajo de Azure Machine Learning Service mediante un archivo de configuración local. Consulte un [diagrama de la arquitectura más reciente](concept-azure-machine-learning-architecture.md).

Gran parte del contenido del proyecto ya estaba en la máquina local, por lo tanto, solo necesita crear un archivo de configuración en ese directorio y hacer referencia a él en el código para conectarse al área de trabajo. Para continuar usando el directorio local que contiene los archivos y scripts, especifique el nombre del directorio en el comando de Python ["experiment.submit"](https://docs.microsoft.com/python/api/azureml-core/azureml.core.experiment.experiment?view=azure-ml-py) o con el comando de la CLI `az ml project attach`.  Por ejemplo:
```python
run = exp.submit(source_directory=script_folder,
                 script='train.py', run_config=run_config_system_managed)
```

[Cree un área de trabajo](how-to-manage-workspace.md) para empezar a trabajar.

## <a name="what-about-my-registered-models-and-images"></a>¿Qué sucede con mis imágenes y modelos registrados?

Debe migrar los modelos que ha registrado en el registro del modelo antiguo a la nueva área de trabajo si quiere seguir usándolos. Para migrar los modelos, descárguelos y vuelva a registrarlos en el área de trabajo nueva.

Las imágenes que ha creado en el antiguo registro de imágenes no se pueden migrar directamente al área de trabajo nueva. En la mayoría de los casos, el modelo puede implementarse sin tener que crear una imagen. Si es necesario, puede crear una imagen para el modelo en el área de trabajo nueva. Para más información, consulte [Administración, registro, implementación y supervisión de modelos de Machine Learning](concept-model-management-and-deployment.md).

## <a name="what-about-deployed-web-services"></a>¿Qué sucede con los servicios web implementados?

Ahora que ha terminado la compatibilidad con la anterior CLI, no puede ya volver a implementar los modelos ni administrar los servicios web que implementó originalmente con la cuenta de Administración de modelos. Sin embargo, dichos servicios web seguirán funcionando siempre y cuando todavía se admita Azure Container Service (ACS).

En la versión más reciente, los modelos se implementan como servicios web para clústeres de Azure Container Instances (ACI) o Azure Kubernetes Service (AKS). También puede implementar en FPGA y en Azure IoT Edge.

Para más información, consulte estos artículos:
+ [Dónde y cómo implementar modelos](how-to-deploy-and-where.md)
+ [Tutorial: Implementación de modelos con Azure Machine Learning Service](tutorial-deploy-models-with-aml.md)

## <a name="what-about-the-old-sdk-and-cli"></a>¿Qué sucede con el SDK y la CLI antiguos?

Seguirán funcionando hasta enero. Consulte la [escala de tiempo](#timeline) anterior. Es recomendable que comience a crear nuevos experimentos y modelos con el SDK o la CLI más reciente.

Con el nuevo SDK de Python de la versión más reciente, puede interactuar con Azure Machine Learning Service en cualquier entorno de Python. Más información sobre cómo instalar el <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> más reciente. También puede usar la [extensión de la CLI de Azure Machine Learning](reference-azure-machine-learning-cli.md) actualizada con el amplio conjunto de comandos `az ml` para interactuar con el servicio en cualquier entorno de línea de comandos, incluido Azure Cloud Shell.

## <a name="what-about-visual-studio-code-tools-for-ai"></a>Más información acerca de Visual Studio Code Tools for AI

En esta versión más reciente, se ha cambiado el nombre de la extensión a Azure Machine Learning para Visual Studio Code y se ha ampliado y mejorado para trabajar con las nuevas características mencionadas.

[![Azure Machine Learning para Visual Studio Code](./media/overview-what-happened-to-workbench/vscode.png)](./media/overview-what-happened-to-workbench/vscode-big.png#lightbox)

## <a name="what-about-domain-packages"></a>¿Qué sucede con los paquetes de dominio?

Los paquetes de dominio para Computer Vision, Text Analytics y Forecasting no se pueden usar con la versión más reciente de Azure Machine Learning. Sin embargo, todavía puede crear y entrenar modelos de Computer Vision, Text y Forecasting con el <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> más reciente de Python de Azure Machine Learning. Para más información sobre cómo migrar los modelos ya existentes creados con los paquetes Computer Vision, Text Analytics y Forecasting, póngase en contacto con [AML-Packages@microsoft.com](mailto:AML-Packages@microsoft.com).

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre la [arquitectura más reciente para Azure Machine Learning Service](concept-azure-machine-learning-architecture.md).

Para obtener información general sobre el servicio, lea [¿Qué es Azure Machine Learning Service?](overview-what-is-azure-ml.md).

Cree su primer experimento con el tutorial de dos partes para [configurar el entorno y el área de trabajo](tutorial-1st-experiment-sdk-setup.md) y [entrenar su primer modelo](tutorial-1st-experiment-sdk-train.md).

Para obtener una experiencia pormenorizada de este flujo de trabajo, siga el [tutorial completo](tutorial-train-models-with-aml.md) que contiene los pasos detallados para el entrenamiento y la implementación de modelos con el servicio Azure Machine Learning.
