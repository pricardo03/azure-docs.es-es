---
title: Uso de la extensión Visual Studio Code Tools for AI con Azure Machine Learning
description: Obtenga información sobre Visual Studio Code Tools for AI y sobre cómo comenzar a entrenar e implementar el aprendizaje automático y los modelos de aprendizaje profundo con el servicio Azure Machine Learning en VS Code.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: shwinne
author: swinner95
ms.reviewer: jmartens
ms.date: 10/1/2018
ms.openlocfilehash: 377a4bbf359b2c65136625fcef8a1093e49da728
ms.sourcegitcommit: 7804131dbe9599f7f7afa59cacc2babd19e1e4b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/17/2018
ms.locfileid: "51854312"
---
# <a name="vs-code-tools-for-ai-train-and-deploy-ml-models-from-vs-code"></a>VS Code Tools for AI: entrenar e implementar modelos de ML desde VS Code
En este artículo aprenderá a usar la extensión **VC Code Tools for AI** para entrenar e implementar el aprendizaje automático y los modelos de aprendizaje profundo con el servicio Azure Machine Learning en VS Code.

Azure Machine Learning proporciona compatibilidad para ejecutar experimentos localmente y en destinos de proceso remotos. Para cada experimento, puede realizar el seguimiento de varias ejecuciones con la frecuencia que necesite para probar de forma iterativa técnicas diferentes, hiperparámetros y mucho más. Puede usar Azure Machine Learning para realizar un seguimiento de las métricas personalizadas y las ejecuciones de los experimentos, habilitando la capacidad de auditoría y reproducción de la ciencia de datos.

Y puede implementar estos modelos para sus necesidades de pruebas y producción.

## <a name="prerequisites"></a>Requisitos previos

+ [Tener VS Code Tools para AI](how-to-vscode-tools.md) configurado para Azure Machine Learning.

+ Tener el [SDK de Azure Machine Learning para Python instalado](how-to-vscode-tools.md) con VS Code.

+ Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://aka.ms/AMLfree) antes de empezar.

## <a name="create-and-manage-compute-targets"></a>Creación y administración de destinos de proceso

Con Visual Studio Code Tools for AI, puede preparar los datos, entrenar modelos e implementarlos tanto localmente como en destinos de proceso remotos.

Esta extensión admite varios destinos de proceso remotos diferentes para Azure Machine Learning. Consulte la [lista completa de destinos de proceso admitidos](how-to-set-up-training-targets.md) para Azure Machine Learning.

### <a name="create-compute-targets-for-azure-machine-learning-in-vs-code"></a>Creación de destinos de proceso para Azure Machine Learning en VS Code

**Para crear un destino de proceso:**

1. Haga clic en el icono de Azure en la barra de actividad de Visual Studio Code. Aparecerá la barra lateral de Azure Machine Learning.

2. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo del servicio Azure Machine Learning. En la imagen animada, el nombre de la suscripción es "OpenMind Studio" y el área de trabajo es "MyWorkspace". 

3. En el nodo del área de trabajo, haga clic con el botón derecho en el nodo **Proceso** y elija **Crear proceso**.

4. Elija el tipo de destino de proceso en la lista. 

5. En el campo, escriba un nombre único para este destino de proceso y especifique el tamaño de la máquina virtual.

6. Especifique las propiedades avanzadas en el archivo de configuración JSON que se abre en una nueva pestaña. 

7. Cuando haya terminado de configurar el proceso de destino, haga clic en **Finalizar** en la parte inferior derecha.

Este es un ejemplo de Azure Batch AI: [![Creación de un proceso de Azure Batch AI en VS Code](./media/vscode-tools-for-ai/createcompute.gif)](./media/vscode-tools-for-ai/createcompute.gif#lightbox)

### <a name="use-remote-computes-for-experiments-in-vs-code"></a>Uso de procesos remotos para experimentos en VS Code

Para usar un destino de proceso remoto durante el entrenamiento, deberá crear un archivo de configuración de ejecución. Este archivo indica a Azure Machine Learning no solo dónde ejecutar el experimento, sino también cómo preparar el entorno.

#### <a name="the-run-configuration-file"></a>Archivo de "configuración de ejecución"

La extensión de VS Code creará automáticamente una configuración de ejecución para los entornos **local** y **docker** en el equipo local.

Se trata de un fragmento de código del archivo de configuración de ejecución predeterminado.

Si desea instalar todas las bibliotecas y dependencias usted mismo, establezca `userManagedDependencies: True`; posteriormente, las ejecuciones de experimentos locales usarán el entorno de Python predeterminado según lo especificado en la extensión de Python para VS Code.

```yaml
# user_managed_dependencies=True indicates that the environment will be user managed. False indicates that AzureML will manage the user environment.
    userManagedDependencies: False
# The python interpreter path
    interpreterPath: python
# Path to the conda dependencies file to use for this run. If a project
# contains multiple programs with different sets of dependencies, it may be
# convenient to manage those environments with separate files.
    condaDependenciesFile: aml_config/conda_dependencies.yml
# Docker details
    docker:
# Set True to perform this run inside a Docker container.
    enabled: false
```

#### <a name="the-conda-dependencies-file"></a>Archivo de dependencia de Conda

De forma predeterminada, se crea un nuevo entorno de Conda y se administran las dependencias de la instalación. Sin embargo, debe especificar las dependencias en el archivo `aml_config/conda_dependencies.yml`.

Se trata de un fragmento de código del archivo "aml_config/conda_dependencies.yml" predeterminado.
Puede agregar dependencias adicionales en el archivo de configuración.

```yaml
# The dependencies defined in this file will be automatically provisioned for runs with userManagedDependencies=False.

name: project_environment
dependencies:
  # The python interpreter version.

  # Currently Azure ML only supports 3.5.2 and later.

- python=3.6.2

- pip:
    # Required packages for AzureML execution, history, and data preparation.

  - --index-url https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
  - --extra-index-url https://pypi.python.org/simple
  - azureml-defaults

```

## <a name="train-and-tune-models"></a>Entrenamiento y ajuste de modelos

Use Azure Machine Learning desde VS Code para iterar rápidamente en el código, recorrer paso a paso y depurar, y usar la solución de control de código fuente de elección. 

**Para ejecutar el experimento con Azure Machine Learning:**

1. Haga clic en el icono de Azure en la barra de actividad de Visual Studio Code. Aparecerá la barra lateral de Azure Machine Learning.

1. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo del servicio Azure Machine Learning. En la imagen animada, el nombre de la suscripción es "OpenMind Studio" y el área de trabajo es "MyWorkspace". 

1. En el nodo de área de trabajo, expanda el nodo **Proceso** y haga clic con el botón derecho en **Run Config** (Configuración de ejecución) correspondiente al proceso que desea usar. 

1. Seleccione **Run Experiment** (Ejecutar experimento).

1. Haga clic en **View Experiment Run** (Ver ejecución de experimento) para ver el portal de Azure Machine Learning integrado para supervisar las ejecuciones y ver los modelos entrenados.

   [![Ejecución de un experimento de aprendizaje automático desde VS Code](./media/vscode-tools-for-ai/runexperiment.gif)](./media/vscode-tools-for-ai/runexperiment.gif#lightbox)

## <a name="deploy-and-manage-models"></a>Implementación y administración de modelos
Azure Machine Learning permite implementar y administrar los modelos de aprendizaje automático en la nube y en el perímetro. 

### <a name="register-your-model-to-azure-machine-learning-from-vs-code"></a>Registro del modelo para Azure Machine Learning desde VS Code

Ahora que ha entrenado el modelo, puede registrarlo en el área de trabajo.
Se puede realizar un seguimiento de los modelos registrados así como implementarlos.

**Para registrar el modelo:**

1. Haga clic en el icono de Azure en la barra de actividad de Visual Studio Code. Aparecerá la barra lateral de Azure Machine Learning.

1. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo del servicio Azure Machine Learning.

1. En el nodo de área de trabajo, haga clic con el botón derecho en **Modelos** y elija **Registrar modelo**.

1. En la lista, elija si desea cargar un **archivo de modelos** (para los modelos individuales) una **carpeta de modelos** (para los modelos con varios archivos, como Tensorflow). 

1. Utilice el cuadro de diálogo del selector de archivos para seleccionar el archivo o la carpeta.

   [![proceso](./media/vscode-tools-for-ai/registermodel.gif)](./media/vscode-tools-for-ai/registermodel.gif#lightbox)

> [!Warning]
> Por ahora, quite las etiquetas del archivo json generado.

### <a name="deploy-your-service-from-vs-code"></a>Implementación del servicio desde VS Code

Use VS Code; puede implementar el servicio web para:
+ Azure Container Instances (ACI): para pruebas
+ Azure Kubernetes Service (AKS): para producción 

No es necesario crear un contenedor de ACI para probar con antelación, ya que se crean sobre la marcha. Sin embargo, los clústeres de AKS necesitan configurarse de antemano. 

Obtenga más información sobre la [implementación con Azure Machine Learning](how-to-deploy-and-where.md) en general.

**Para implementar un servicio web:**

1. Haga clic en el icono de Azure en la barra de actividad de Visual Studio Code. Aparecerá la barra lateral de Azure Machine Learning.

1. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo del servicio Azure Machine Learning.

1. En el nodo del área de trabajo, expanda el nodo **Modelos**.

1. Haga clic con el botón derecho en el modelo que desea implementar y elija el comando **Implementar el servicio a partir del modelo registrado** en el menú contextual.

1. En la paleta de comandos de VS Code, elija en la lista el destino de proceso en el que se va a implementar. 

1. En el campo, escriba un nombre para este servicio. 

1. En el cuadro de diálogo, en la esquina inferior derecha, haga clic en **Examinar** y seleccione el script de puntuación. El cuadro de diálogo se cerrará.

1. Si tiene un archivo de Docker local, haga clic en **Examinar** en el segundo cuadro de diálogo que aparece. 

   Si se cancela el cuadro de diálogo y no se especifica un archivo de Docker local, se usa uno de "Azure Machine Learning" de forma predeterminada.

1. En el tercer cuadro de diálogo que aparece, haga clic en **Examinar** y seleccione la ruta de acceso del archivo local de Conda, o escriba la ruta de acceso del archivo en el editor de json más adelante.

1. Si tiene un archivo schema.json que desea usar, haga clic en **Examinar** en el cuarto cuadro de diálogo que aparece y selecciónelo.

Ahora se implementará el servicio web.

Este es un ejemplo para Azure Container Instances: [![Azure Container Instances desde VS Code](./media/vscode-tools-for-ai/deploy.gif)](./media/vscode-tools-for-ai/deploy.gif#lightbox)

## <a name="next-steps"></a>Pasos siguientes

Para un tutorial paso a paso de aprendizaje con Machine Learning fuera de VS Code, lea [Tutorial 1: Entrenamiento de un modelo de clasificación de imágenes con el servicio Azure Machine Learning](tutorial-train-models-with-aml.md).

Para un tutorial paso a paso sobre edición, ejecución y depuración del código de forma local, consulte el [tutorial de la aplicación Hola mundo en Python](https://code.visualstudio.com/docs/python/python-tutorial).
