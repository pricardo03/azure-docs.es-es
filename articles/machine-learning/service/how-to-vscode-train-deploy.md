---
title: Entrenar e implementar modelos a partir de VS Code
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre Azure Machine Learning para Visual Studio Code y sobre cómo comenzar a entrenar e implementar el aprendizaje automático y los modelos de aprendizaje profundo con Azure Machine Learning Service en Visual Studio Code.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: shwinne
author: swinner95
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 0910cce92c410a68dce6e2c44d29e72e594cd153
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53271715"
---
# <a name="use-visual-studio-code-to-train-and-deploy-machine-learning-models"></a>Usar Visual Studio Code para entrenar e implementar modelos de aprendizaje automático

En este artículo aprenderá a usar la extensión **Azure Machine Learning para Visual Studio Code** para entrenar e implementar el aprendizaje automático y los modelos de aprendizaje profundo con Azure Machine Learning Service en Visual Studio Code (VS Code).

Azure Machine Learning proporciona compatibilidad para ejecutar experimentos localmente y en destinos de proceso remotos. Para cada experimento, puede realizar el seguimiento de varias ejecuciones con la frecuencia que necesite para probar de forma iterativa técnicas diferentes, hiperparámetros y mucho más. Puede usar Azure Machine Learning para realizar un seguimiento de las métricas personalizadas y las ejecuciones de los experimentos, habilitando la capacidad de auditoría y reproducción de la ciencia de datos.

Y puede implementar estos modelos para sus necesidades de pruebas y producción.

## <a name="prerequisites"></a>Requisitos previos

+ Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](http://aka.ms/AMLFree).

+ Instale la extensión [Azure Machine Learning para VS Code](how-to-vscode-tools.md).

+ Tener el [SDK de Azure Machine Learning para Python instalado](how-to-vscode-tools.md) con VS Code.

## <a name="create-and-manage-compute-targets"></a>Creación y administración de destinos de proceso

Con Azure Machine Learning para VS Code, puede preparar los datos, entrenar modelos e implementarlos tanto localmente como en destinos de proceso remotos.

Esta extensión admite varios destinos de proceso remotos diferentes para Azure Machine Learning. Consulte la [lista completa de destinos de proceso admitidos](how-to-set-up-training-targets.md) para Azure Machine Learning.

### <a name="create-compute-targets-for-azure-machine-learning-in-vs-code"></a>Creación de destinos de proceso para Azure Machine Learning en VS Code

**Para crear un destino de proceso:**

1. Haga clic en el icono de Azure en la barra de actividad de Visual Studio Code. Aparecerá la barra lateral de Azure Machine Learning.

2. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo del servicio Azure Machine Learning. En la imagen animada, el nombre de la suscripción es "Evaluación gratuita" y el área de trabajo es "TeamWorkspace". 

3. En el nodo del área de trabajo, haga clic con el botón derecho en el nodo **Proceso** y elija **Crear proceso**.

4. Elija el tipo de destino de proceso en la lista. 

5. Especifique las propiedades avanzadas en el archivo de configuración JSON que se abre en una nueva pestaña. Puede especificar un nombre único para el destino de proceso en este archivo.

6. Cuando haya terminado de configurar el destino de proceso, haga clic en **Enviar** en la parte inferior derecha.

Este es un ejemplo para el proceso de Azure Machine Learning (AMLCompute): [![Crear el proceso de AML en VS Code](./media/vscode-tools-for-ai/CreateAMLCompute.gif)](./media/vscode-tools-for-ai/CreateAMLCompute.gif#lightbox)

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

Use Azure Machine Learning para VS Code (versión preliminar) para iterar rápidamente en el código, recorrer paso a paso y depurar, y usar la solución de control de código fuente de su elección. 

**Para ejecutar el experimento con Azure Machine Learning:**

1. Haga clic en el icono de Azure en la barra de actividad de Visual Studio Code. Aparecerá la barra lateral de Azure Machine Learning.

1. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo del servicio Azure Machine Learning. 

1. En el nodo de área de trabajo, expanda el nodo **Proceso** y haga clic con el botón derecho en **Run Config** (Configuración de ejecución) correspondiente al proceso que desea usar. 

1. Seleccione **Run Experiment** (Ejecutar experimento).

1. Haga clic en **View Experiment Run** (Ver ejecución de experimento) para ver el portal de Azure Machine Learning integrado para supervisar las ejecuciones y ver los modelos entrenados.

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

1. Seleccione la carpeta o el archivo.

1. Cuando haya terminado de configurar las propiedades del modelo, haga clic en **Enviar** en la parte inferior derecha. 



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

1. En la paleta de comandos, elija en la lista el destino de proceso que se va a usar para la implementación. 

1. En el campo, escriba un nombre para este servicio. 

1. En la Paleta de comandos, presione la tecla Entrar para examinar y seleccionar el archivo de script.

1. En la Paleta de comandos, presione la tecla Entrar para examinar y seleccionar el archivo de dependencia conda.

1. Cuando haya terminado de configurar las propiedades del servicio, haga clic en **Enviar** en la parte inferior derecha. En este archivo de propiedades del servicio, puede especificar el archivo de Docker local o el archivo schema.json que quiera usar.

Ahora se implementará el servicio web.

## <a name="next-steps"></a>Pasos siguientes

Para obtener un tutorial paso a paso de aprendizaje con Machine Learning fuera de VS Code, lea [Tutorial: Entrenamiento de modelos con Azure Machine Learning](tutorial-train-models-with-aml.md).

Para un tutorial paso a paso sobre edición, ejecución y depuración del código de forma local, consulte el [tutorial de la aplicación Hola mundo en Python](https://code.visualstudio.com/docs/python/python-tutorial).
