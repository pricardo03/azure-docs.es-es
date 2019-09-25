---
title: Uso de Visual Studio Code para el aprendizaje automático
titleSuffix: Azure Machine Learning
description: Aprenda a instalar Azure Machine Learning para Visual Studio Code y crear un sencillo experimento en Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: ronglu
author: ronglums
ms.date: 7/12/2019
ms.custom: seodec18
ms.openlocfilehash: 0507080a390a4bb9f981b0d911961337e9b2c62a
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2019
ms.locfileid: "70997192"
---
# <a name="get-started-with-azure-machine-learning-for-visual-studio-code"></a>Introducción a Azure Machine Learning para Visual Studio Code

En este artículo, aprenderá a usar la extensión Azure Machine Learning para Visual Studio Code para entrenar e implementar el aprendizaje automático y modelos de aprendizaje profundo.

[Azure Machine Learning](overview-what-is-azure-ml.md) proporciona compatibilidad para los experimentos que ejecute localmente y en destinos de proceso remotos. Para cada experimento, puede realizar el seguimiento de varias ejecuciones con la frecuencia que necesite para probar de forma iterativa técnicas diferentes, hiperparámetros y mucho más. Puede usar Azure Machine Learning para realizar un seguimiento de las métricas personalizadas y las ejecuciones de los experimentos, habilitando la capacidad de auditoría y reproducción de la ciencia de datos.

Además, puede implementar estos modelos para satisfacer las necesidades de pruebas y producción.

## <a name="prerequisites"></a>Requisitos previos

+ Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

+ Visual Studio Code debe estar instalado. Visual Studio Code es un editor de código fuente ligero, pero eficaz, que se ejecuta en el escritorio. Incluye compatibilidad integrada para Python y otros lenguajes de programación. Si todavía no ha instalado Visual Studio Code, [vea cómo hacerlo](https://code.visualstudio.com/docs/setup/setup-overview).

+ [Instale Python 3.5 o una versión posterior](https://www.anaconda.com/download/).


## <a name="install-the-extension-for-azure-machine-learning-for-visual-studio-code"></a>Instalar la extensión Azure Machine Learning para Visual Studio Code

Cuando instale la extensión Azure Machine Learning, se instalan automáticamente dos extensiones más (si tiene acceso a Internet). Son la extensión [Azure Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) y la extensión [Microsoft Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python).

Para trabajar con Azure Machine Learning, deberá convertir Visual Studio Code en un entorno de desarrollo integrado (IDE) en Python. Se necesita la extensión Microsoft Python para usar [Python en Visual Studio Code](https://code.visualstudio.com/docs/languages/python). Esta extensión se instala automáticamente con la extensión Azure Machine Learning. La extensión hace que Visual Studio Code sea un excelente IDE, y funciona en cualquier sistema operativo con diversos intérpretes de Python. La extensión Microsoft Python usa toda la potencia de Visual Studio Code para proporcionar características de autocompletar, IntelliSense, detección de errores, depuración y pruebas unitarias. La extensión también permite cambiar fácilmente entre entornos de Python, incluido los entornos virtuales y de conda. Para más información sobre cómo editar, ejecutar y depurar código de Python, consulte el [tutorial de hello world de Python](https://code.visualstudio.com/docs/python/python-tutorial).

Para instalar la extensión Azure Machine Learning:

1. Abra Visual Studio Code.

1. En un explorador web, vaya a [Azure Machine Learning for Visual Studio Code extension (preview)](https://aka.ms/vscodetoolsforai) (Azure Machine Learning para la extensión Visual Studio Code [versión preliminar]).

1. En la página web, seleccione **Instalar**. 

1. En la pestaña de la extensión, seleccione **Instalar**.

1. Se abre una pestaña bienvenida de la extensión en Visual Studio Code, y el símbolo de Azure (resaltado en rojo en la siguiente captura de pantalla) se agrega a la barra de actividad.

   ![Icono de Azure en la barra de actividad de Visual Studio Code](./media/vscode-tools-for-ai/azure-activity-bar.png)

1. En el cuadro de diálogo, seleccione **Sesión** y siga las indicaciones para autenticarse con Azure. 
   
   La extensión Cuenta de Azure, que se instaló junto con la extensión Azure Machine Learning para Visual Studio Code, le ayuda a autenticarse con su cuenta de Azure. Para obtener una lista de comandos, consulte la página de la [extensión Cuenta de Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

> [!Tip] 
> Consulte [IntelliCode extension for Visual Studio Code (preview)](https://go.microsoft.com/fwlink/?linkid=2006060) (Extensión IntelliCode para Visual Studio Code [versión preliminar]). IntelliCode proporciona un conjunto de funcionalidades asistidas por inteligencia artificial para IntelliSense en Python, tal como inferir el texto de autocompetar más pertinentes según el contexto de código actual.

## <a name="install-the-azure-machine-learning-sdk"></a>Instalación del SDK de Azure Machine Learning

1. Asegúrese de que Python 3.5 o una versión posterior esté instalado y que Visual Studio Code lo reconozca. Si se lo instale ahora, reinicie Visual Studio Code y [seleccione un intérprete de Python](https://code.visualstudio.com/docs/python/python-tutorial).

1. En la ventana del terminal integrado, especifique el intérprete de Python que se debe usar. O bien, presione Intro para usar el intérprete de Python predeterminado.

   ![Elección del intérprete](./media/vscode-tools-for-ai/python.png)

1. En la esquina inferior derecha de la ventana, aparece una notificación en la que se indica que el [SDK de Azure Machine Learning](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py) se instala automáticamente. El entorno de Python recién creado es local y privado, y tiene los requisitos previos de Visual Studio Code para trabajar con Azure Machine Learning.

   ![Instalar el SDK de Azure Machine Learning para Python](./media/vscode-tools-for-ai/runtimedependencies.png)

## <a name="get-started-with-azure-machine-learning"></a>Introducción a Azure Machine Learning

Antes de iniciar el aprendizaje y la implementación de modelos de aprendizaje automático en Visual Studio Code, debe crear un [área de trabajo de Azure Machine Learning](concept-workspace.md) en la nube. Esta área de trabajo contiene los modelos y los recursos. 

Para crear un área de trabajo y agregar su primer experimento:

1. En la barra de actividad de Visual Studio Code, haga clic en el icono de Azure. Aparecerá la barra lateral de Azure Machine Learning.

   [![Creación de un área de trabajo](./media/vscode-tools-for-ai/CreateaWorkspace.gif)](./media/vscode-tools-for-ai/CreateaWorkspace.gif#lightbox)


1. Haga clic con el botón derecho en su suscripción de Azure y seleccione **Crear área de trabajo**. Aparece una lista. En la imagen animada de ejemplo, el nombre de la suscripción es **Free Trial** y el área de trabajo es **TeamWorkspace**. 

1. Seleccione un grupo de recursos existente de la lista o cree uno mediante el asistente de la paleta de comandos.

1. En el campo, escriba un nombre único y claro para la nueva área de trabajo. En la imagen de ejemplo, el área de trabajo se denomina **TeamWorkspace**.

1. Presione Intro para crear el área de trabajo. Este aparece en el árbol, por debajo del nombre de la suscripción.

1. Haga clic con el botón derecho en el nodo **Experiment** (Experimento) y elija **Create Experiment** (Crear experimento) en el menú contextual.  Los experimentos realizan un seguimiento de las ejecuciones mediante Azure Machine Learning.

1. En el campo, escriba un nombre para el experimento. En las capturas de pantalla de ejemplo, el experimento se denomina **MNIST**.
 
1. Presione Intro para crear el nuevo experimento. El experimento aparece en el árbol, por debajo del nombre del área de trabajo.

1. En un área de trabajo, puede hacer clic con el botón derecho en un experimento para establecerlo como el experimento **activo**. El experimento **activo** experimento es el experimento actual. La carpeta abierta en Visual Studio Code se vinculará a este experimento en la nube. Esta carpeta debe contener los scripts de Python locales.

Ahora las métricas clave se almacenarán en el historial del experimento. De forma similar, los modelos que entrene se cargarán automáticamente en Azure Machine Learning y se almacenarán junto con las métricas y los registros del experimento. 

[![Adjunto de una carpeta a Visual Studio Code](./media/vscode-tools-for-ai/CreateAnExperiment.gif)](./media/vscode-tools-for-ai/CreateAnExperiment.gif#lightbox)


## <a name="create-and-manage-compute-targets"></a>Creación y administración de destinos de proceso

Con Azure Machine Learning para Visual Studio Code, puede preparar los datos, entrenar modelos e implementarlos tanto localmente como en destinos de proceso remotos.

Esta extensión admite varios destinos de proceso remotos diferentes para Azure Machine Learning. Para más información, consulte la [lista completa de destinos de proceso admitidos para Azure Machine Learning](how-to-set-up-training-targets.md).

### <a name="create-compute-targets-for-azure-machine-learning-in-visual-studio-code"></a>Creación de destinos de proceso para Azure Machine Learning en Visual Studio Code

Para crear un destino de proceso:

1. En la barra de actividad de Visual Studio Code, haga clic en el icono de Azure. Aparecerá la barra lateral de Azure Machine Learning.

2. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo de Azure Machine Learning. En la siguiente imagen de ejemplo, el nombre de la suscripción es **Free Trial** y el área de trabajo es **TeamWorkspace**. 

3. En el nodo del área de trabajo, haga clic con el botón derecho en el nodo **Proceso** y elija **Crear proceso**.

4. Elija el tipo de destino de proceso en la lista. 

5. En la paleta de comandos, seleccione un tamaño de máquina virtual.

6. En la paleta de comandos, en el campo, escriba un nombre para el destino de proceso. 

7. En el archivo de configuración JSON que se abre en una nueva pestaña, especifique las propiedades avanzadas. Puede especificar propiedades como, por ejemplo, el número máximo de nodos.

8. Cuando termine de configurar el destino de proceso, en la esquina inferior derecha de la ventana, seleccione **Enviar**.

A continuación proporcionamos un ejemplo de cómo crear un proceso de Azure Machine Learning (AMLCompute):

[![Creación de un proceso de Azure Machine Learning en Visual Studio Code](./media/vscode-tools-for-ai/CreateARemoteCompute.gif)](./media/vscode-tools-for-ai/CreateARemoteCompute.gif#lightbox)

#### <a name="the-run-configuration-file"></a>Archivo de "configuración de ejecución"

La extensión Visual Studio Code crea automáticamente las configuraciones de ejecución y del destino de proceso local para los entornos local y docker en el equipo local. Los archivos de configuración de ejecución se encuentran bajo el nodo de destino de proceso asociado. 

## <a name="train-and-tune-models"></a>Entrenamiento y ajuste de modelos

Use Azure Machine Learning para Visual Studio Code (versión preliminar) para iterar rápidamente en el código, recorrer paso a paso y depurar, así como usar la solución para el control de código fuente. 

Para ejecutar el experimento de forma local con Azure Machine Learning:

1. En la barra de actividad de Visual Studio Code, haga clic en el icono de Azure. Aparecerá la barra lateral de Azure Machine Learning.

1. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo de Azure Machine Learning. 

1. En el nodo del área de trabajo, expanda el nodo **Proceso** y haga clic con el botón derecho en **Run Config** (Configuración de ejecución) correspondiente al proceso que desea usar. 

1. Seleccione **Run Experiment** (Ejecutar experimento).

1. En el Explorador de archivos, seleccione el script que desea ejecutar. 

1. Seleccione **View Experiment Run** (Ver ejecución de experimento) para ver el portal de Azure Machine Learning integrado, así como supervisar las ejecuciones y ver los modelos entrenados.

A continuación proporcionamos un ejemplo de cómo ejecutar un experimento localmente:

[![Ejecución de un experimento localmente](./media/vscode-tools-for-ai/RunExperimentLocally.gif)](./media/vscode-tools-for-ai/RunExperimentLocally.gif#lightbox)

### <a name="use-remote-computes-for-experiments-in-visual-studio-code"></a>Uso de procesos remotos para los experimentos en Visual Studio Code

Para usar un destino de proceso remoto durante el entrenamiento, debe crear un archivo de configuración de ejecución. Este archivo indica a Azure Machine Learning no solo dónde ejecutar el experimento, sino también cómo preparar el entorno.

#### <a name="the-conda-dependencies-file"></a>Archivo de dependencia de Conda

De forma predeterminada, se crea un nuevo entorno conda y se administran las dependencias de la instalación. Sin embargo, debe especificar las dependencias y sus versiones en el archivo *aml_config/conda_dependencies.yml*. 

En el siguiente fragmento de código desde el valor predeterminado *aml_config/conda_dependencies.yml* se especifica `tensorflow=1.12.0`. Si no especifica la versión de la dependencia, se usará la versión más reciente. Puede agregar dependencias adicionales en el archivo de configuración.

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

Para ejecutar el experimento con Azure Machine Learning en un destino de proceso remoto:

1. En la barra de actividad de Visual Studio Code, haga clic en el icono de Azure. Aparecerá la barra lateral de Azure Machine Learning.

1. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo de Azure Machine Learning. 

1. En la ventana del editor, haga clic con el botón derecho en el script de Python y seleccione **AML: Ejecutar como experimento en Azure**. 

1. En la paleta de comandos, seleccione el destino de proceso. 

1. En la paleta de comandos, en el campo, escriba el nombre de la configuración de ejecución. 

1. Modifique el archivo *conda_dependencies.yml* para especificar las dependencias de tiempo de ejecución del experimento. Luego, en la esquina inferior derecha de la ventana, seleccione **Enviar**. 

1. Seleccione **View Experiment Run** (Ver ejecución de experimento) para ver el portal de Azure Machine Learning integrado, así como supervisar las ejecuciones y ver los modelos entrenados.

A continuación proporcionamos un ejemplo de cómo ejecutar un experimento en un destino de proceso remoto:

[![Ejecución de un experimento en un destino remoto](./media/vscode-tools-for-ai/runningOnARemoteTarget.gif)](./media/vscode-tools-for-ai/runningOnARemoteTarget.gif#lightbox)


## <a name="deploy-and-manage-models"></a>Implementación y administración de modelos
En Azure Machine Learning, puede implementar y administrar los modelos de aprendizaje automático en la nube y en el perímetro. 

### <a name="register-your-model-to-azure-machine-learning-from-visual-studio-code"></a>Registro del modelo en Azure Machine Learning desde Visual Studio Code

Ahora que ha entrenado el modelo, puede registrarlo en el área de trabajo. Puede hacer un seguimiento de los modelos registrados e implementar estos.

Para registrar el modelo:

1. En la barra de actividad de Visual Studio Code, haga clic en el icono de Azure. Aparecerá la barra lateral de Azure Machine Learning.

1. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo de Azure Machine Learning.

1. En el nodo de área de trabajo, haga clic con el botón derecho en **Modelos** y elija **Registrar modelo**.

1. En la paleta de comandos, en el campo, escriba un nombre de modelo. 

1. En la lista, elija si desea cargar un **archivo de modelos** (para los modelos individuales) o una **carpeta de modelos** (para los modelos con varios archivos, como TensorFlow). 

1. Seleccione la carpeta o el archivo.

1. Cuando termine de configurar las propiedades del modelo, en la esquina inferior derecha de la ventana, seleccione **Enviar**. 

A continuación proporcionamos un ejemplo de cómo registrar el modelo de Azure Machine Learning:

[![Registro de un modelo para Azure Machine Learning](./media/vscode-tools-for-ai/RegisteringAModel.gif)](./media/vscode-tools-for-ai/RegisteringAModel.gif#lightbox)


### <a name="deploy-your-service-from-visual-studio-code"></a>Implementación del servicio desde Visual Studio Code

En Visual Studio Code, puede implementar el servicio web para lo siguiente:
+ Azure Container Instances (ACI) para realizar pruebas.
+ Azure Kubernetes Service (AKS) para entornos de producción.

No es necesario crear un contenedor ACI para realizar pruebas por adelantado, ya que estos contenedores se crean sobre la marcha. Sin embargo, es necesario configurar clústeres AKS de antemano. Para más información, consulte [Implementación de modelos con Azure Machine Learning](how-to-deploy-and-where.md).

Para implementar un servicio web:

1. En la barra de actividad de Visual Studio Code, haga clic en el icono de Azure. Aparecerá la barra lateral de Azure Machine Learning.

1. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo de Azure Machine Learning.

1. En el nodo del área de trabajo, expanda el nodo **Modelos**.

1. Haga clic con el botón derecho en el modelo que desea implementar y seleccione **Implementar el servicio a partir del modelo registrado** en el menú contextual.

1. En la paleta de comandos, elija el destino de proceso en el que desea realizar la implementación. 

1. En la paleta de comandos, en el campo, escriba un nombre para este servicio.  

1. En la paleta de comandos, presione la tecla Intro para buscar y seleccionar el archivo de script.

1. En la paleta de comandos, presione la tecla Intro para buscar y seleccionar el archivo de dependencia conda.

1. Cuando termine de configurar las propiedades del servicio, en la esquina inferior derecha de la ventana, seleccione **Enviar**. En el archivo de propiedades del servicio, puede especificar un archivo de docker local o un archivo schema.json.

Ahora se implementará el servicio web.

A continuación proporcionamos un ejemplo de cómo implementar un servicio web:

[![Implementación de un servicio web](./media/vscode-tools-for-ai/CreatingAnImage.gif)](./media/vscode-tools-for-ai/CreatingAnImage.gif#lightbox)

### <a name="use-keyboard-shortcuts"></a>Uso de métodos abreviados de teclado

Puede usar el teclado para acceder a las características de Azure Machine Learning en Visual Studio Code. El método abreviado de teclado más importante que debería conocer es Ctrl+Mayús+P, que muestra la paleta de comandos. Desde la paleta de comandos, tendrá acceso a toda la funcionalidad de Visual Studio Code, como los métodos abreviados de teclado para las operaciones más habituales.

[![Métodos abreviados de teclado para Azure Machine Learning para Visual Studio Code](./media/vscode-tools-for-ai/commands.gif)](./media/vscode-tools-for-ai/commands.gif#lightbox)

## <a name="next-steps"></a>Pasos siguientes

* Para ver un tutorial sobre cómo entrenar con Azure Machine Learning fuera de Visual Studio Code, consulte [Tutorial: Entrenamiento de modelos con Azure Machine Learning](tutorial-train-models-with-aml.md).
* Para ver un tutorial sobre cómo editar, ejecutar y depurar código de forma local, consulte el [tutorial de hello world de Python](https://code.visualstudio.com/docs/python/python-tutorial).
