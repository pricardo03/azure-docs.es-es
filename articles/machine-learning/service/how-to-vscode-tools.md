---
title: Uso de Visual Studio Code para machine learning
titleSuffix: Azure Machine Learning service
description: Aprenda a instalar Azure Machine Learning para Visual Studio Code y crear un sencillo experimento en Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: shwinne
author: swinner95
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 19873256f8253fff75cfd42df7b876106a9e98e5
ms.sourcegitcommit: e6d53649bfb37d01335b6bcfb9de88ac50af23bd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2019
ms.locfileid: "65464784"
---
# <a name="get-started-with-azure-machine-learning-for-visual-studio-code"></a>Introducción a Azure Machine Learning para Visual Studio Code

En este artículo, obtendrá información sobre cómo usar la extensión de Azure Machine Learning para Visual Studio Code para entrenar e implementar el aprendizaje automático y modelos de aprendizaje con el servicio Azure Machine Learning en Visual Studio Code profundo.

El servicio de Azure Machine Learning proporciona compatibilidad para los experimentos que se ejecutan localmente y en los destinos de proceso remoto. Para cada experimento, puede realizar el seguimiento de varias ejecuciones con la frecuencia que necesite para probar de forma iterativa técnicas diferentes, hiperparámetros y mucho más. Puede usar Azure Machine Learning para realizar un seguimiento de las métricas personalizadas y las ejecuciones de los experimentos, habilitando la capacidad de auditoría y reproducción de la ciencia de datos.

También puede implementar estos modelos para sus necesidades de pruebas y producción.

## <a name="prerequisites"></a>Requisitos previos

+ Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe el [versión gratuita o de pago del servicio Azure Machine Learning](https://aka.ms/AMLFree).

+ Visual Studio Code debe estar instalado. Visual Studio Code es un editor de código fuente ligero pero eficaz que se ejecuta en el escritorio. Incluye compatibilidad integrada para Python y otros lenguajes de programación. Si todavía no ha instalado Visual Studio Code, [averiguar cómo](https://code.visualstudio.com/docs/setup/setup-overview).

+ [Instalar Python 3.5 o posterior](https://www.anaconda.com/download/).


## <a name="install-the-extension-for-azure-machine-learning-for-visual-studio-code"></a>Instalar la extensión de Azure Machine Learning para Visual Studio Code

Cuando se instala la extensión de Azure Machine Learning, más dos extensiones se instalan automáticamente (si tiene acceso a internet). Son el [extensión cuenta de Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) y [extensión de Python de Microsoft](https://marketplace.visualstudio.com/items?itemName=ms-python.python).

Para trabajar con Azure Machine Learning, deberá convertir el código de Visual Studio en un entorno de desarrollo de Python integrado (IDE). Se necesita la extensión de Python de Microsoft para usar [Python en Visual Studio Code](https://code.visualstudio.com/docs/languages/python). Esta extensión se instala automáticamente con la extensión de Azure Machine Learning. Hace que la extensión de Visual Studio Code un excelente IDE y funciona en cualquier sistema operativo con una variedad de los intérpretes de Python. La extensión de Python de Microsoft usa toda la potencia de Visual Studio Code para proporcionar Autocompletar, IntelliSense, detección de errores, depuración y pruebas unitarias. La extensión también permite cambiar fácilmente entre entornos de Python, incluido virtual y los entornos de conda. Para obtener más información acerca de cómo editar, ejecutar y depurar código de Python, consulte el [tutorial de hello world de Python](https://code.visualstudio.com/docs/python/python-tutorial).

Para instalar la extensión de Azure Machine Learning:

1. Abra Visual Studio Code.

1. En un explorador web, vaya a [Azure Machine Learning para la extensión de Visual Studio Code (versión preliminar)](https://aka.ms/vscodetoolsforai).

1. En la página web, seleccione **instalar**. 

1. En la pestaña de la extensión, seleccione **instalar**.

1. Se abre una pestaña bienvenida de la extensión en Visual Studio Code, y el símbolo de Azure (resaltado en rojo en la siguiente captura de pantalla) se agrega a la barra de actividad.

   ![Icono de Azure en la barra de actividad de código de Visual Studio](./media/vscode-tools-for-ai/azure-activity-bar.png)

1. En el cuadro de diálogo, seleccione **sesión** y siga las indicaciones para autenticarse con Azure. 
   
   La extensión de la cuenta de Azure, que se instaló junto con Azure Machine Learning para la extensión de Visual Studio Code, le ayuda a autenticarse con su cuenta de Azure. Para obtener una lista de comandos, consulte la página para el [extensión cuenta de Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

> [!Tip] 
> Consulte la [IntelliCode extensión para Visual Studio Code (versión preliminar)](https://go.microsoft.com/fwlink/?linkid=2006060). IntelliCode proporciona un conjunto de capacidades asistido por inteligencia artificial para IntelliSense en Python, como deducir la autocompletions más relevantes según el contexto de código actual.

## <a name="install-the-azure-machine-learning-sdk"></a>Instalar Azure Machine Learning SDK

1. Asegúrese de que Python 3.5 o posterior está instalado y que es reconocido por el código de Visual Studio. Si se lo instale ahora, reinicie Visual Studio Code y [seleccionar un intérprete de Python](https://code.visualstudio.com/docs/python/python-tutorial).

1. En la ventana del terminal integrado, especifique el intérprete de Python para usar. O bien, presione ENTRAR para usar el intérprete de Python de forma predeterminada.

   ![Elección del intérprete](./media/vscode-tools-for-ai/python.png)

1. En la esquina inferior derecha de la ventana, aparecerá una notificación que indica que el SDK de Azure Machine Learning se está instalando automáticamente. El entorno de Python recién creado es local y privada, y tiene los requisitos previos de Visual Studio Code para trabajar con el servicio de Azure Machine Learning.

   ![Instalar el SDK de Azure Machine Learning para Python](./media/vscode-tools-for-ai/runtimedependencies.png)

## <a name="get-started-with-azure-machine-learning"></a>Introducción a Azure Machine Learning

Antes de iniciar el aprendizaje y la implementación de modelos de aprendizaje automático en Visual Studio Code, debe crear un [área de trabajo de servicio de Azure Machine Learning](concept-azure-machine-learning-architecture.md#workspace) en la nube. Esta área de trabajo contiene los modelos y los recursos. 

Para crear un área de trabajo y agregar su primer experimento:

1. En la barra de actividad de código de Visual Studio, seleccione el icono de Azure. Aparecerá la barra lateral de Azure Machine Learning.

   [![Crear un área de trabajo](./media/vscode-tools-for-ai/CreateaWorkspace.gif)](./media/vscode-tools-for-ai/CreateaWorkspace.gif#lightbox)


1. Haga clic con el botón derecho en su suscripción de Azure y seleccione **Crear área de trabajo**. Aparece una lista. En la imagen animada de ejemplo, el nombre de la suscripción es **gratuita**, y el área de trabajo **TeamWorkspace**. 

1. Seleccione un grupo de recursos de la lista o cree uno nuevo mediante el Asistente en la paleta de comandos.

1. En el campo, escriba un nombre único y claro para la nueva área de trabajo. En la imagen de ejemplo, el área de trabajo se denomina **TeamWorkspace**.

1. Presione ENTRAR para crear el área de trabajo. Aparece en el árbol, debajo del nombre de la suscripción.

1. Haga clic en el **experimento** nodo y elija **crear experimento** en el menú contextual.  Los experimentos realizan un seguimiento de las ejecuciones mediante Azure Machine Learning.

1. En el campo, escriba un nombre para el experimento. En las capturas de pantalla de ejemplo, se denomina el experimento **MNIST**.
 
1. Presione ENTRAR para crear el nuevo experimento. El experimento aparece en el árbol, debajo del nombre del área de trabajo.

1. En un área de trabajo, haga clic en un experimento para establecerla como la **Active** experimentar. El **Active** experimento es el experimento actual. La carpeta abierta en Visual Studio Code se vinculará a este experimento en la nube. Esta carpeta debe contener los scripts de Python locales.

Ahora las métricas claves se almacena en el historial del experimento. De forma similar, los modelos que entrena se automáticamente cargados en Azure Machine Learning y se almacenará junto con su experimento métricas y registros. 

[![Adjuntar una carpeta en Visual Studio Code](./media/vscode-tools-for-ai/CreateAnExperiment.gif)](./media/vscode-tools-for-ai/CreateAnExperiment.gif#lightbox)


## <a name="create-and-manage-compute-targets"></a>Creación y administración de destinos de proceso

Con Azure Machine Learning para Visual Studio Code, puede preparar los datos, entrenar modelos e implementarlos localmente y en los destinos de proceso remoto.

La extensión admite varios destinos de proceso remoto para Azure Machine Learning. Para obtener más información, consulte la lista completa de admitidos [destinos de proceso de Azure Machine Learning](how-to-set-up-training-targets.md).

### <a name="create-compute-targets-for-azure-machine-learning-in-visual-studio-code"></a>Crear destinos de proceso para Azure Machine Learning en Visual Studio Code

Para crear un destino de proceso:

1. En la barra de actividad de código de Visual Studio, seleccione el icono de Azure. Aparecerá la barra lateral de Azure Machine Learning.

2. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo del servicio Azure Machine Learning. En la imagen de ejemplo siguiente, el nombre de la suscripción es **gratuita**, y el área de trabajo **TeamWorkspace**. 

3. En el nodo del área de trabajo, haga clic con el botón derecho en el nodo **Proceso** y elija **Crear proceso**.

4. Elija el tipo de destino de proceso en la lista. 

5. En la paleta de comandos, seleccione un tamaño de máquina virtual.

6. En la paleta de comandos en el campo, escriba un nombre para el destino de proceso. 

7. En el archivo de configuración JSON que se abre en una nueva pestaña, especifique las propiedades avanzadas. Puede especificar propiedades como un número de nodos máximo.

8. Cuando termine de configurar el destino de proceso, en la esquina inferior derecha de la ventana, seleccione **enviar**.

Este es un ejemplo de cómo crear un proceso de Azure Machine Learning (AMLCompute):

[![Crear proceso de aprendizaje automático de AZURE en Visual Studio Code](./media/vscode-tools-for-ai/CreateARemoteCompute.gif)](./media/vscode-tools-for-ai/CreateARemoteCompute.gif#lightbox)

#### <a name="the-run-configuration-file"></a>El archivo de configuración de ejecución

La extensión de código de Visual Studio crea automáticamente un destino de proceso local y las configuraciones de ejecución para los entornos local y docker en el equipo local. Puede encontrar los archivos de configuración de ejecución bajo el nodo de destino de proceso asociados. 

## <a name="train-and-tune-models"></a>Entrenamiento y ajuste de modelos

Use Azure Machine Learning para Visual Studio Code (versión preliminar) para iterar en el código, recorrer paso a paso y depurar y usar su solución para control de código fuente rápidamente. 

Para ejecutar el experimento localmente mediante el uso de Azure Machine Learning:

1. En la barra de actividad de código de Visual Studio, seleccione el icono de Azure. Aparecerá la barra lateral de Azure Machine Learning.

1. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo del servicio Azure Machine Learning. 

1. En el nodo de área de trabajo, expanda el **proceso** nodo y con el botón secundario el **configuración de ejecución** del proceso que desea usar. 

1. Seleccione **Run Experiment** (Ejecutar experimento).

1. En el Explorador de archivos, seleccione la secuencia de comandos que desea ejecutar. 

1. Seleccione **vista ejecución de experimento** para ver el portal de Azure Machine Learning integrado para supervisar la ejecución y ver los modelos entrenados.

Este es un ejemplo de cómo ejecutar un experimento localmente:

[![Ejecutar un experimento localmente](./media/vscode-tools-for-ai/RunExperimentLocally.gif)](./media/vscode-tools-for-ai/RunExperimentLocally.gif#lightbox)

### <a name="use-remote-computes-for-experiments-in-visual-studio-code"></a>Usar remoto calcula los experimentos en Visual Studio Code

Para usar un destino de proceso remoto para el entrenamiento, deberá crear un archivo de configuración de ejecución. Este archivo indica a Azure Machine Learning no solo dónde ejecutar el experimento, sino también cómo preparar el entorno.

#### <a name="the-conda-dependencies-file"></a>Archivo de dependencia de Conda

De forma predeterminada, se crea un nuevo entorno de conda y se administran las dependencias de instalación. Sin embargo, debe especificar las dependencias y sus versiones en el *aml_config/conda_dependencies.yml* archivo. 

El siguiente fragmento de código desde el valor predeterminado *aml_config/conda_dependencies.yml* especifica `tensorflow=1.12.0`. Si no se especifica la versión de la dependencia, se usará la versión más reciente. Puede agregar dependencias adicionales en el archivo de configuración.

```yaml
# The dependencies defined in this file will be automatically provisioned for runs with userManagedDependencies=False.

name: project_environment
dependencies:
  # The python interpreter version.

  # Currently Azure ML only supports 3.5.2 and later.

- python=3.6.2
- tensorflow=1.12.0

- pip:
    # Required packages for AzureML execution, history, and data preparation.

  - --index-url https://azuremlsdktestpypi.azureedge.net/sdk-release/Preview/E7501C02541B433786111FE8E140CAA1
  - --extra-index-url https://pypi.python.org/simple
  - azureml-defaults

```

Para ejecutar el experimento con Azure Machine Learning en un control remoto de destino de proceso:

1. En la barra de actividad de código de Visual Studio, seleccione el icono de Azure. Aparecerá la barra lateral de Azure Machine Learning.

1. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo del servicio Azure Machine Learning. 

1. En la ventana del editor, haga clic en el script de Python y seleccione **AML: Ejecutar como experimento en Azure**. 

1. En la paleta de comandos, seleccione el destino de proceso. 

1. En la paleta de comandos en el campo, escriba el nombre de la configuración de ejecución. 

1. Editar el *conda_dependencies.yml* archivo para especificar las dependencias de tiempo de ejecución del experimento. A continuación, en la esquina inferior derecha de la ventana, seleccione **enviar**. 

1. Seleccione **vista ejecución de experimento** para ver el portal de Azure Machine Learning integrado para supervisar la ejecución y ver los modelos entrenados.

Este es un ejemplo de cómo ejecutar un experimento en un destino de proceso remoto:

[![Ejecutar un experimento en un destino remoto](./media/vscode-tools-for-ai/runningOnARemoteTarget.gif)](./media/vscode-tools-for-ai/runningOnARemoteTarget.gif#lightbox)


## <a name="deploy-and-manage-models"></a>Implementación y administración de modelos
En Azure Machine Learning, puede implementar y administrar los modelos en la nube y en el perímetro de aprendizaje automático. 

### <a name="register-your-model-to-azure-machine-learning-from-visual-studio-code"></a>Registrar el modelo para Azure Machine Learning desde Visual Studio Code

Ahora que ha entrenado el modelo, puede registrarlo en el área de trabajo. Puede realizar un seguimiento e implementar modelos registrados.

Para registrar el modelo:

1. En la barra de actividad de código de Visual Studio, seleccione el icono de Azure. Aparecerá la barra lateral de Azure Machine Learning.

1. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo del servicio Azure Machine Learning.

1. En el nodo de área de trabajo, haga clic con el botón derecho en **Modelos** y elija **Registrar modelo**.

1. En la paleta de comandos en el campo, escriba un nombre de modelo. 

1. En la lista, elija si desea cargar un **archivo modelo** (para los modelos únicos) o un **carpeta modelo** (para los modelos con varios archivos, como TensorFlow). 

1. Seleccione la carpeta o el archivo.

1. Cuando termine de configurar las propiedades del modelo, en la esquina inferior derecha de la ventana, seleccione **enviar**. 

Este es un ejemplo de cómo registrar el modelo de Azure Machine Learning:

[![Registro de un modelo para Azure Machine Learning](./media/vscode-tools-for-ai/RegisteringAModel.gif)](./media/vscode-tools-for-ai/RegisteringAModel.gif#lightbox)


### <a name="deploy-your-service-from-visual-studio-code"></a>Implementar el servicio desde Visual Studio Code

En Visual Studio Code, puede implementar el servicio web para:
+ Azure Container Instances (ACI) para las pruebas.
+ Azure Kubernetes Service (AKS) para la producción.

No es necesario crear un contenedor ACI para probar con antelación, ya que se crean contenedores ACI sobre la marcha. Sin embargo, es necesario configurar clústeres AKS de antemano. Para obtener más información, consulte [implementar modelos con el servicio de Azure Machine Learning](how-to-deploy-and-where.md).

Para implementar un servicio web:

1. En la barra de actividad de código de Visual Studio, seleccione el icono de Azure. Aparecerá la barra lateral de Azure Machine Learning.

1. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo del servicio Azure Machine Learning.

1. En el nodo del área de trabajo, expanda el nodo **Modelos**.

1. Haga clic en el modelo que desea implementar y elija **implementar el servicio desde el modelo registrado** en el menú contextual.

1. En la paleta de comandos, elija el destino de proceso que desea implementar en. 

1. En la paleta de comandos en el campo, escriba un nombre para este servicio.  

1. En la paleta de comandos, seleccione la tecla ENTRAR del teclado para buscar y seleccionar el archivo de script.

1. En la paleta de comandos, seleccione la tecla ENTRAR del teclado para buscar y seleccionar el archivo de dependencia de conda.

1. Cuando termine de configurar las propiedades del servicio, en la esquina inferior derecha de la ventana, seleccione **enviar** para implementar. En el archivo de propiedades del servicio, puede especificar un archivo de docker local o un archivo schema.json.

Ahora se implementará el servicio web.

Este es un ejemplo de cómo implementar un servicio web:

[![Implementar un servicio web](./media/vscode-tools-for-ai/CreatingAnImage.gif)](./media/vscode-tools-for-ai/CreatingAnImage.gif#lightbox)

### <a name="use-keyboard-shortcuts"></a>Uso de métodos abreviados de teclado

Puede usar el teclado para tener acceso a características de Azure Machine Learning en Visual Studio Code. El método abreviado de teclado más importante saber es Ctrl + Mayús + P, que muestra la paleta de comandos. Desde la paleta de comandos, tendrá acceso a toda la funcionalidad de Visual Studio Code, incluidos los métodos abreviados de teclado para las operaciones más habituales.

[![Métodos abreviados de teclado para Azure Machine Learning para Visual Studio Code](./media/vscode-tools-for-ai/commands.gif)](./media/vscode-tools-for-ai/commands.gif#lightbox)

## <a name="next-steps"></a>Pasos siguientes

* Para ver un tutorial sobre cómo entrenar con Azure Machine Learning fuera de Visual Studio Code, consulte [Tutorial: Entrenamiento de modelos con Azure Machine Learning](tutorial-train-models-with-aml.md).
* Para ver un tutorial sobre cómo editar, ejecutar y depurar código de forma local, consulte el [tutorial de hello world de Python](https://code.visualstudio.com/docs/python/python-tutorial).
