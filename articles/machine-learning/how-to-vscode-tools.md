---
title: Azure Machine Learning en Visual Studio Code
titleSuffix: Azure Machine Learning
description: Aprenda a instalar Azure Machine Learning para Visual Studio Code y cree un sencillo experimento en Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: jimgries
author: greazer
ms.date: 09/20/2019
ms.custom: seodec18
ms.openlocfilehash: d5843937233309e1b37f0127d364066a8a747e21
ms.sourcegitcommit: ce4a99b493f8cf2d2fd4e29d9ba92f5f942a754c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/28/2019
ms.locfileid: "75534620"
---
# <a name="get-started-with-azure-machine-learning-for-visual-studio-code"></a>Introducción a Azure Machine Learning para Visual Studio Code

En este artículo, aprenderá a usar la extensión **Azure Machine Learning para Visual Studio Code** para entrenar e implementar los modelos de Machine Learning.

[Azure Machine Learning](overview-what-is-azure-ml.md) simplifica la creación, el entrenamiento y la implementación de los modelos de Machine Learning.
+ Para el entrenamiento, proporciona compatibilidad para ejecutar experimentos localmente o de forma remota. En cada experimento, puede registrar métricas personalizadas de varias ejecuciones para ajustar los hiperparámetros.
+ Asimismo, también puede usar Azure Machine Learning para implementar fácilmente modelos de Machine Learning para sus necesidades de prueba y producción.

## <a name="prerequisites"></a>Prerequisites

+ Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe la [versión gratuita o de pago de Azure Machine Learning](https://aka.ms/AMLFree).

+ Instale [Visual Studio Code](https://code.visualstudio.com/docs/setup/setup-overview), un editor de código ligero que se ejecuta en Windows, Mac y Linux.

+ [Instale Python 3.5 o una versión posterior](https://www.anaconda.com/download/).


## <a name="install-the-extension"></a>Instalación de la extensión

Cuando instale la extensión de Azure Machine Learning, se instalan automáticamente dos extensiones más. Son la extensión [Azure Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) y la extensión [Microsoft Python](https://marketplace.visualstudio.com/items?itemName=ms-Python.Python). Para obtener más información sobre cómo editar, ejecutar y depurar código de Python mediante la extensión de Python, consulte el [tutorial de hello world de Python](https://code.visualstudio.com/docs/Python/Python-tutorial).

Para instalar la extensión Azure Machine Learning:

1. Abra Visual Studio Code.

1. Cambie a la pestaña Extensiones y busque "Azure Machine Learning".

1. En la pestaña de la extensión, seleccione **Instalar**.

1. Se abrirá una pestaña de bienvenida de la extensión en Visual Studio Code, y el símbolo de Azure (resaltado en rojo en la siguiente captura de pantalla) se agrega a la barra de actividad.

   ![Icono de Azure en la barra de actividad de Visual Studio Code](./media/how-to-vscode-tools/azure-activity-bar.png)

1. En el cuadro de diálogo, seleccione **Sesión** y siga las indicaciones para autenticarse con Azure.

   La extensión Cuenta de Azure, que se instaló junto con la extensión Azure Machine Learning para Visual Studio Code, le ayuda a autenticarse con su cuenta de Azure. Para obtener una lista de comandos, consulte la página de la [extensión Cuenta de Azure](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account).

> [!TIP]
> También puede descargar el instalador de la extensión directamente desde [Azure  Machine Learning para la extensión de Visual Studio Code (versión preliminar)](https://aka.ms/vscodetoolsforai).

## <a name="quickstart-with-azure-machine-learning"></a>Inicio rápido con Azure Machine Learning
Existen varias maneras de ejecutar los scripts de entrenamiento mediante Azure Machine Learning. Si acaba de empezar, veremos primero cómo enviar rápidamente un script de entrenamiento para que se ejecute en Azure.

Si ya está familiarizado con los conceptos de Azure Machine Learning y quiere obtener más información sobre cómo administrarlos y usarlos con la extensión, consulte los [detalles de Azure Machine Learning con VS Code](how-to-vscode-tools.md#azure-machine-learning-in-depth-with-vs-code) a continuación.

## <a name="run-an-existing-python-training-script-in-azure"></a>Ejecutar un script de entrenamiento de Python existente en Azure
Si tiene un script de entrenamiento existente, la extensión de Azure Machine Learning para VS Code no solo proporciona una excelente experiencia de edición, depuración y administración del código fuente, sino que también facilita la ejecución y el almacenamiento de las métricas del script en Azure.

Comencemos. Puede usar su propio script de entrenamiento si ya lo tiene listo o clonar el ejemplo [vscode-tools-for-ai repo](https://github.com/microsoft/vscode-tools-for-ai). Este es el repositorio público para la presentación de problemas de esta extensión. También contiene una pequeña carpeta de ejemplo **mnist** que usaremos en este ejemplo.

1. Abra la carpeta **mnist** en VS Code.

1. Cree un nuevo entorno de Python con su paquete de entorno virtual favorito o con Anaconda e instale los paquetes TensorFlow y NumPy.

1. Seleccione el nuevo entorno que creó como intérprete de Python en la esquina inferior izquierda de la barra de estado de VS Code.

1. Abra **train.py** y ejecútelo; para ello, abra el depurador y pulse el botón de ejecución (o simplemente presione F5).

   [![Ejecutar el entrenamiento de MNIST](./media/how-to-vscode-tools/run-mnist.gif)](./media/how-to-vscode-tools/run-mnist.gif#lightbox)

Si todo está instalado correctamente, el script se ejecutará y creará un modelo de TensorFlow en la carpeta de salidas.

[![Mostrar el modelo de TensorFlow](./media/how-to-vscode-tools/show-tensorflow-model.gif)](./media/how-to-vscode-tools/show-tensorflow-model.gif#lightbox)

Ahora que sabe que el script se ejecuta correctamente, vamos a ejecutarlo en Azure.

Esto se puede hacer fácilmente sin ninguna modificación adicional de **train.py**. Sin embargo, con solo unos pocos y sencillos cambios, puede usar Azure Machine Learning para realizar un seguimiento automático de las métricas importantes que quiera sobre cada ejecución de entrenamiento.

### <a name="make-azure-aware-of-your-run-metrics"></a>Haga que Azure tenga en cuenta las métricas de ejecución
Para modificar el proyecto de forma que Azure pueda tener en cuenta la información importante en las ejecuciones, realice lo siguiente:

1. Cree un archivo denominado **amlrun.py** en la misma carpeta que **train.py**.

    ```Python
    import azureml
    from azureml.core import Run

    # Access the Azure ML run
    # Init run param to check if running within AML
    def get_AMLRun():
        try:
            run = Run.get_submitted_run()
            return run
        except Exception as e:
            print("Caught = {}".format(e.message))
            return None
    ```

2. Importe el archivo amlrun en **train.py**.

    ```Python
    ...
    from utils import prepare_data
    from amlrun import get_AMLRun
    ...
    ```
3. Inicialice el objeto de ejecución en **train.py**.

    ```Python
    ...
    init = tf.global_variables_initializer()
    saver = tf.train.Saver()
    run = get_AMLRun()
    ...
    ```
4. Registre las métricas en Azure con la función run.log():

    ```Python
    ...
            acc_val = acc_op.eval(feed_dict = {X: X_test, y: y_test})

            # Log accuracies to AML logger if using AML
            if run != None:
                run.log('Validation Accuracy', np.float(acc_val))
                run.log('Training Accuracy', np.float(acc_train))

            print(epoch, '-- Training accuracy:', acc_train, '\b Validation
    ...
    ```
### <a name="run-the-script-in-azure"></a>Ejecutar el script en Azure
Eso es todo. Ahora solo tiene que usar la extensión para ejecutar el script en la nube. Tenga en cuenta que en el siguiente vídeo del tutorial nos tomamos la libertad de reducir la cantidad de tiempo que se tarda en crear un nuevo área de trabajo y proceso de Azure Machine Learning, así como el tiempo que se tarda en ejecutar el script de entrenamiento.

   [![Inicio de un experimento de Azure Machine Learning](./media/how-to-vscode-tools/start-golden-path.gif)](./media/how-to-vscode-tools/start-golden-path.gif#lightbox)

Después de hacer clic en el botón para ejecutar experimento, responda a los mensajes tal como se indica a continuación:

1. Elija la suscripción de Azure.
1. Elija crear una *nueva* área de trabajo de Azure Machine Learning.
1. Elija un conjunto de plantillas preconfiguradas para inicializar el entorno de Python para las ejecuciones. Las plantillas proporcionan un punto inicial e incluyen la configuración de:
    1. **PyTorch**, **TensorFlow**, o **Scikit-learn**
    1. Entrenamiento de procesos **simple** o **distribuido**
    1. **General** para entornos personalizados
1. Asegúrese de que la lista de paquetes PIP y Conda esté completa en el script agregando cualquier paquete que no esté incluido en la plantilla.
1. Revise los nombres y las especificaciones predeterminados para la ejecución del experimento y haga clic en el vínculo **Enviar experimento** en el archivo JSON. El archivo JSON no se guardará, ya que simplemente está allí para que revise o cambie la configuración del experimento antes de enviarlo.
1. Siéntese y relájese mientras la extensión configura todo para usted y ejecuta su script.

    [![Entrenamiento en la nube](./media/how-to-vscode-tools/run-golden-path.gif)](./media/how-to-vscode-tools/run-golden-path.gif#lightbox)

En unos segundos, se le notificará que el experimento se ha enviado a Azure; en ese momento puede ver su progreso en Azure Machine Learning Studio haciendo clic en el vínculo **Ver ejecución del experimento** en la notificación de VS Code o en VS Code, si pulsa el botón de actualización de la pestaña de Azure.

Por el momento, la visualización de métricas en ejecución solo se admite en Studio. El vínculo **Ver ejecución del experimento** mencionado anteriormente le llevará al proceso de ejecución donde verá las métricas que registró.
[![ Experimento ejecutado en el portal](./media/how-to-vscode-tools/experiment-run-on-portal.PNG)](./media/how-to-vscode-tools/experiment-run-on-portal.PNG#lightbox)

## <a name="azure-machine-learning-in-depth-with-vs-code"></a>Detalles de Azure Machine Learning con VS Code

En el tutorial anterior le presentamos un experimento siguiendo los pasos más fáciles. Como habrá notado, la extensión minimiza los pasos que necesita realizar para ejecutar un experimento. En esta sección, le indicaremos cómo administrar todos los conceptos de Azure Machine Learning individualmente para que pueda obtener el máximo control.

Antes de iniciar el aprendizaje y la implementación de modelos de aprendizaje automático en Visual Studio Code, debe crear un [área de trabajo de Azure Machine Learning](concept-workspace.md) en la nube. Esta área de trabajo contiene los modelos y los recursos.

### <a name="create-a-workspace"></a>Crear un área de trabajo

1. En la barra de actividad de Visual Studio Code, haga clic en el icono de Azure. Aparecerá la barra lateral de Azure Machine Learning.

    [![Creación de un área de trabajo](./media/how-to-vscode-tools/create-workspace.gif)](./media/how-to-vscode-tools/create-workspace.gif#lightbox)


1. Haga clic con el botón derecho en su suscripción de Azure y seleccione **Crear área de trabajo**. De forma predeterminada, se crea un nombre que contiene la fecha y la hora de la creación. Cambie el nombre a **TeamWorkspace** y presione Entrar.

1. Seleccione un grupo de recursos de la lista si sabe cuál elegir o cree uno nuevo. Si crea uno nuevo, elija la ubicación que esté más cerca de la ubicación donde quiere implementar el modelo. En este caso, elegiremos **Oeste de EE. UU. 2**.

1. Después de presionar Entrar, Azure Machine Learning recibirá la solicitud para crear el área de trabajo. Recibirá una notificación del proceso en el área de notificaciones de Visual Studio Code.

1. Expanda el nodo de suscripción para buscar el área de trabajo recién creado.

### <a name="create-an-experiment"></a>Creación de un experimento
Se pueden crear uno o más experimentos en el área de trabajo y así poder realizar un seguimiento y analizar la ejecuciones de entrenamiento de modelos individuales. Las ejecuciones se pueden realizar en la nube de Azure o en su máquina local.

1. Expanda el área de trabajo **TeamWorkspace**. Haga clic con el botón derecho en el nodo **Experiments** (Experimentos) y elija **Create Experiment** (Crear experimento) en el menú contextual.

1. En la solicitud, escriba un nombre para el experimento. En las capturas de pantalla de ejemplo, el experimento se denomina **MNIST**.

1. Presione Intro para crear el nuevo experimento. El nuevo experimento aparece en el árbol como elemento secundario del nodo **Experiments** (Experimentos).

1. En un área de trabajo, puede hacer clic con el botón derecho en un experimento para establecerlo como el experimento **activo**. El experimento **Activo** vincula ese experimento en la nube a la carpeta que actualmente tiene abierta en Visual Studio Code. Esta carpeta debe contener los scripts de Python locales. Al establecer un experimento activo, las métricas clave de todas las ejecuciones de entrenamiento se almacenarán en el mismo experimento, independientemente de dónde se ejecuten.

    [![Crear un experimento](./media/how-to-vscode-tools/create-experiment.gif)](./media/how-to-vscode-tools/create-experiment.gif#lightbox)


### <a name="create-and-manage-compute-targets"></a>Creación y administración de destinos de proceso

Con Azure Machine Learning para Visual Studio Code, puede preparar los datos, entrenar modelos e implementarlos tanto localmente como en destinos de proceso remotos.

Esta extensión admite varios destinos de proceso remotos diferentes para Azure Machine Learning. Para más información, consulte la [lista completa de destinos de proceso admitidos para Azure Machine Learning](how-to-set-up-training-targets.md).

### <a name="create-compute-targets-for-azure-machine-learning-in-visual-studio-code"></a>Creación de destinos de proceso para Azure Machine Learning en Visual Studio Code

Para crear un destino de proceso:

1. En la barra de actividad de Visual Studio Code, haga clic en el icono de Azure. Aparecerá la barra lateral de Azure Machine Learning.

1. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo de Azure Machine Learning.

1. En el nodo del área de trabajo, haga clic con el botón derecho en el nodo **Proceso** y elija **Crear proceso**.

1. Elija el tipo de destino de proceso en la lista.

1. En la paleta de comandos, seleccione un tamaño de máquina virtual. Puede filtrar los procesos con texto, como "gpu".

1. En el indicador de la paleta de comandos, escriba un nombre para el destino de proceso.

1. Después de escribir el nombre, el proceso se creará mediante los parámetros predeterminados. Para cambiar esos parámetros, haga clic derecho en el nuevo proceso y seleccione **Edit Compute** (Editar proceso).

1. En el elemento JSON que se muestra, realice los cambios que quiera y haga clic en la opción "Save and continue" (Guardar y continuar) de CodeLens (si usa el teclado puede presionar **ctrl-shift-p** para invocar la paleta de comandos y ejecutar el comando **Azure Machine Learning: Save and Continue**).

A continuación, le proporcionamos un ejemplo para que vea cómo crear y editar un proceso de Azure Machine Learning (AMLCompute):

[![Creación de un proceso de Azure Machine Learning en Visual Studio Code](./media/how-to-vscode-tools/create-remote-compute.gif)](./media/how-to-vscode-tools/create-remote-compute.gif#lightbox)

#### <a name="the-run-configuration-file"></a>Archivo de "configuración de ejecución"

Para ejecutar un experimento de Azure Machine Learning en un proceso, ese mismo proceso debe configurarse adecuadamente. Un archivo de configuración de ejecución es el mecanismo mediante el cual se especifica el entorno.

Aquí tiene un ejemplo que muestra cómo crear una configuración de ejecución para el elemento AmlCompute, creado anteriormente.

[![ rear una configuración de ejecución para un proceso](./media/how-to-vscode-tools/create-runconfig.gif)](./media/how-to-vscode-tools/create-runconfig.gif#lightbox)

Para ejecutar experimentos de Azure Machine Learning en su máquina local, es necesario tener un archivo de configuración de ejecución. Al crear una configuración de ejecución local, el entorno de Python que use tomará por defecto la ruta de acceso al intérprete que haya establecido en VS Code.

### <a name="train-and-tune-models"></a>Entrenamiento y ajuste de modelos

Al usar de la extensión de Azure Machine Learning para VS Code, encontrá varias formas de ejecutar un script de entrenamiento en un experimento.

1. Haga clic derecho en el script de entrenamiento y seleccione **Azure Machine Learning: ejecutar como experimento en Azure**.
1. Haga clic en el icono de la barra de herramientas para ejecutar el experimento.
1. Haga clic derecho en un nodo de configuración de ejecución.
1. Use la paleta de comandos de VS Code para ejecutar **Azure Machine Learning: ejecutar experimento**

Para ejecutar un experimento de Azure Machine Learning:

1. En la barra de actividad de Visual Studio Code, haga clic en el icono de Azure.

1. En la vista de árbol, expanda su suscripción de Azure y el área de trabajo de Azure Machine Learning.

1. En el nodo del área de trabajo, expanda el nodo **Experiments** (Experimentos) y haga clic con el botón derecho en el experimento que quiera ejecutar.

1. Seleccione **Run Experiment** (Ejecutar experimento).

1. Elija el nombre del archivo de Python que quiera ejecutar para entrenar el modelo y presione Entrar para enviar la ejecución. Nota: El archivo que elija debe estar en la carpeta que tenga abierta actualmente en VS Code.

1. Después de enviar la ejecución, aparecerá un **nodo de ejecución** debajo del experimento que eligió. Use ese nodo para supervisar el estado de sus ejecuciones. Nota: Es posible que necesite actualizar periódicamente la ventana para ver el estado más reciente.

Aquí tiene un ejemplo de cómo ejecutar un experimento en el proceso creado previamente:

[![Ejecución de un experimento localmente](./media/how-to-vscode-tools/run-experiment.gif)](./media/how-to-vscode-tools/run-experiment.gif#lightbox)

### <a name="deploy-and-manage-models"></a>Implementación y administración de modelos
En Azure Machine Learning, puede implementar y administrar los modelos de aprendizaje automático en la nube y en el perímetro.

#### <a name="register-your-model-to-azure-machine-learning-from-visual-studio-code"></a>Registro del modelo en Azure Machine Learning desde Visual Studio Code

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

[![Registro de un modelo para Azure Machine Learning](./media/how-to-vscode-tools/register-model.gif)](./media/how-to-vscode-tools/register-model.gif#lightbox)


#### <a name="deploy-your-service-from-visual-studio-code"></a>Implementación del servicio desde Visual Studio Code

En Visual Studio Code, puede implementar el servicio web para lo siguiente:
+ Azure Container Instances (ACI) para realizar pruebas.
+ Azure Kubernetes Service (AKS) para entornos de producción.

No es necesario crear un contenedor ACI para realizar pruebas por adelantado, ya que estos contenedores se crean según se necesitan. Sin embargo, es necesario configurar clústeres AKS de antemano. Para más información, consulte [Implementación de modelos con Azure Machine Learning](how-to-deploy-and-where.md).

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

[![Implementación de un servicio web](./media/how-to-vscode-tools/create-image.gif)](./media/how-to-vscode-tools/create-image.gif#lightbox)

### <a name="experiment-with-additional-features"></a>Experimentar con características adicionales

Puede usar la paleta de comandos para obtener acceso a las características de Azure Machine Learning en Visual Studio Code. Para invocar el tipo de paleta de comandos, presione Ctrl + Mayús + P. Desde aquí podrá buscar características adicionales de Azure Machine Learning de la extensión.

[![Métodos abreviados de teclado para Azure Machine Learning para Visual Studio Code](./media/how-to-vscode-tools/commands.gif)](./media/how-to-vscode-tools/commands.gif#lightbox)

## <a name="next-steps"></a>Pasos siguientes

* Para ver un tutorial sobre cómo entrenar con Azure Machine Learning fuera de Visual Studio Code, consulte [Tutorial: Entrenamiento de modelos con Azure Machine Learning](tutorial-train-models-with-aml.md).
* Para ver un tutorial sobre cómo editar, ejecutar y depurar código de forma local, consulte el [tutorial de hello world de Python](https://code.visualstudio.com/docs/Python/Python-tutorial).
