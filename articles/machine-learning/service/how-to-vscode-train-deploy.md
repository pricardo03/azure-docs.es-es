---
title: Entrenar e implementar modelos a partir de VS Code
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre Azure Machine Learning para Visual Studio Code y sobre cómo comenzar a entrenar e implementar el aprendizaje automático y los modelos de aprendizaje profundo con Azure Machine Learning Service en Visual Studio Code.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: shwinne
author: swinner95
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 8d63d5f6281de022e8f7e5c9e0a208866fef3f4f
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "57889563"
---
# <a name="use-visual-studio-code-to-train-and-deploy-machine-learning-models"></a>Usar Visual Studio Code para entrenar e implementar modelos de aprendizaje automático

En este artículo aprenderá a usar la extensión **Azure Machine Learning para Visual Studio Code** para entrenar e implementar el aprendizaje automático y los modelos de aprendizaje profundo con Azure Machine Learning Service en Visual Studio Code (VS Code).

Azure Machine Learning proporciona compatibilidad para ejecutar experimentos localmente y en destinos de proceso remotos. Para cada experimento, puede realizar el seguimiento de varias ejecuciones con la frecuencia que necesite para probar de forma iterativa técnicas diferentes, hiperparámetros y mucho más. Puede usar Azure Machine Learning para realizar un seguimiento de las métricas personalizadas y las ejecuciones de los experimentos, habilitando la capacidad de auditoría y reproducción de la ciencia de datos.

Y puede implementar estos modelos para sus necesidades de pruebas y producción.

## <a name="prerequisites"></a>Requisitos previos

+ Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

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

5. En la paleta de comandos, seleccione un tamaño de máquina virtual.

6. En la paleta de comandos, escriba un nombre para el destino de proceso en el campo.

7. Especifique las propiedades avanzadas en el archivo de configuración JSON que se abre en una nueva pestaña. Puede especificar propiedades como el número máximo de nodos.

8. Cuando haya terminado de configurar el destino de proceso, haga clic en **Enviar** en la esquina inferior derecha de la pantalla.

Este es un ejemplo para crear un proceso de Azure Machine Learning (AMLCompute): [![Crear el proceso de AML en VS Code](./media/vscode-tools-for-ai/CreateARemoteCompute.gif)](./media/vscode-tools-for-ai/CreateARemoteCompute.gif#lightbox)

#### <a name="the-run-configuration-file"></a>Archivo de "configuración de ejecución"

La extensión de VS Code creará automáticamente las configuraciones de ejecución y del destino de proceso local para los entornos **local** y **docker** en el equipo local. Los archivos de configuración de ejecución pueden encontrarse en el destino de proceso asociado.

Se trata de un fragmento de código del archivo de configuración de ejecución local predeterminado. De forma predeterminada, `userManagedDependencies: True` de modo que debe instalar todas las dependencias o bibliotecas y, después, las ejecuciones de experimentos locales usarán el entorno de Python predeterminado según lo especificado en la extensión de Python para VS Code.

```yaml
# user_managed_dependencies = True indicates that the environment will be user managed. False indicates that Azure Machine Learning service will manage the user environment.
    userManagedDependencies: True
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

## <a name="train-and-tune-models"></a>Entrenamiento y ajuste de modelos

Use Azure Machine Learning para VS Code (versión preliminar) para iterar rápidamente en el código, recorrer paso a paso y depurar, y usar la solución de control de código fuente de su elección.

**Para ejecutar el experimento de forma local con Azure Machine Learning:**

1. Haga clic en el icono de Azure en la barra de actividad de Visual Studio Code. Aparecerá la barra lateral de Azure Machine Learning.

1. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo del servicio Azure Machine Learning.

1. En el nodo de área de trabajo, expanda el nodo **Proceso** y haga clic con el botón derecho en **Run Config** (Configuración de ejecución) correspondiente al proceso que desea usar.

1. Seleccione **Run Experiment** (Ejecutar experimento).

1. Seleccione el script que se ejecutará desde el Explorador de archivos.

1. Haga clic en **View Experiment Run** (Ver ejecución de experimento) para ver el portal de Azure Machine Learning integrado para supervisar las ejecuciones y ver los modelos entrenados.

Este es un ejemplo para ejecutar un experimento localmente: [![Ejecución local de un experimento](./media/vscode-tools-for-ai/RunExperimentLocally.gif)](./media/vscode-tools-for-ai/RunExperimentLocally.gif#lightbox)

### <a name="use-remote-computes-for-experiments-in-vs-code"></a>Uso de procesos remotos para experimentos en VS Code

Para usar un destino de proceso remoto durante el entrenamiento, deberá crear un archivo de configuración de ejecución. Este archivo indica a Azure Machine Learning no solo dónde ejecutar el experimento, sino también cómo preparar el entorno.

#### <a name="the-conda-dependencies-file"></a>Archivo de dependencia de Conda

De forma predeterminada, se crea un nuevo entorno de Conda y se administran las dependencias de la instalación. Sin embargo, debe especificar las dependencias y sus versiones en el archivo `aml_config/conda_dependencies.yml`. 

Se trata de un fragmento de código del archivo "aml_config/conda_dependencies.yml" predeterminado. Por ejemplo, puede especificar "tensorflow = 1.12.0" tal y como se muestra a continuación. Si no especifica la versión de la dependencia, se usará la versión más reciente.  
Puede agregar dependencias adicionales en el archivo de configuración.

```yaml
# The dependencies defined in this file will be automatically provisioned for runs with userManagedDependencies=False.

name: project_environment
dependencies:
  # The python interpreter version.

  # Currently Azure Machine Learning service only supports 3.5.2 and later.

- python=3.6.2
- tensorflow=1.12.0

- pip:
    # Required packages for Azure Machine Learning service execution, history, and data preparation.

  - --index-url https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
  - --extra-index-url https://pypi.python.org/simple
  - azureml-defaults

```

**Para ejecutar el experimento con Azure Machine Learning en un destino de proceso remoto:**

1. Haga clic en el icono de Azure en la barra de actividad de Visual Studio Code. Aparecerá la barra lateral de Azure Machine Learning.

1. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo del servicio Azure Machine Learning.

1. Haga clic con el botón derecho en el script de python en la ventana del editor y seleccione **AML: Ejecutar como experimento en Azure**.

1. En la paleta de comandos, seleccione el destino de proceso.

1. En la paleta de comandos, escriba el nombre de la configuración de ejecución en el campo.

1. Edite el archivo conda_dependencies.yml para especificar las dependencias del entorno de ejecución del experimento; a continuación, haga clic en **Enviar** en la esquina inferior derecha de la pantalla.

1. Haga clic en **View Experiment Run** (Ver ejecución de experimento) para ver el portal de Azure Machine Learning integrado para supervisar las ejecuciones y ver los modelos entrenados.

Este es un ejemplo para ejecutar un experimento en un destino de proceso remoto: [![Ejecución de un experimento en un destino remoto](./media/vscode-tools-for-ai/runningOnARemoteTarget.gif)](./media/vscode-tools-for-ai/runningOnARemoteTarget.gif#lightbox)


## <a name="deploy-and-manage-models"></a>Implementación y administración de modelos
Azure Machine Learning permite implementar y administrar los modelos de aprendizaje automático en la nube y en el perímetro.

### <a name="register-your-model-to-azure-machine-learning-from-vs-code"></a>Registro del modelo para Azure Machine Learning desde VS Code

Ahora que ha entrenado el modelo, puede registrarlo en el área de trabajo.
Se puede realizar un seguimiento de los modelos registrados así como implementarlos.

**Para registrar el modelo:**

1. Haga clic en el icono de Azure en la barra de actividad de Visual Studio Code. Aparecerá la barra lateral de Azure Machine Learning.

1. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo del servicio Azure Machine Learning.

1. En el nodo de área de trabajo, haga clic con el botón derecho en **Modelos** y elija **Registrar modelo**.

1. En la paleta de comandos, escriba un nombre de modelo en el campo.

1. En la lista, elija si desea cargar un **archivo de modelos** (para los modelos individuales) una **carpeta de modelos** (para los modelos con varios archivos, como Tensorflow).

1. Seleccione la carpeta o el archivo.

1. Cuando haya terminado de configurar las propiedades del modelo, haga clic en **Enviar** en la esquina inferior derecha de la pantalla.

Este es un ejemplo para registrar el modelo para Azure Machine Learning: [![Registro de un modelo para Azure Machine Learning](./media/vscode-tools-for-ai/RegisteringAModel.gif)](./media/vscode-tools-for-ai/RegisteringAModel.gif#lightbox)


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

1. En la paleta de comandos, escriba un nombre para este servicio en el campo.

1. En la Paleta de comandos, presione la tecla Entrar para examinar y seleccionar el archivo de script.

1. En la Paleta de comandos, presione la tecla Entrar para examinar y seleccionar el archivo de dependencia conda.

1. Cuando haya terminado de configurar las propiedades del servicio, haga clic en **Enviar** en la esquina inferior derecha de la pantalla para implementarlo. En este archivo de propiedades del servicio, puede especificar un archivo de Docker local o el archivo schema.json que desee usar.

Ahora se implementará el servicio web.

Aquí tiene un ejemplo para implementar un servicio web: [![Implementación de un servicio web](./media/vscode-tools-for-ai/CreatingAnImage.gif)](./media/vscode-tools-for-ai/CreatingAnImage.gif#lightbox)

## <a name="next-steps"></a>Pasos siguientes

Para obtener un tutorial paso a paso de aprendizaje con Machine Learning fuera de VS Code, lea [Tutorial: Entrenamiento de modelos con Azure Machine Learning](tutorial-train-models-with-aml.md).

Para un tutorial paso a paso sobre edición, ejecución y depuración del código de forma local, consulte el [tutorial de la aplicación Hola mundo en Python](https://code.visualstudio.com/docs/python/python-tutorial).
