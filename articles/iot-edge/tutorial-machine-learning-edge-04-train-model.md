---
title: 'Tutorial: Entrenamiento e implementación de un modelo: Machine Learning en Azure IoT Edge'
description: En este tutorial aprenderá a entrenar un modelo de Machine Learning mediante Azure Machine Learning y, a continuación, empaquetar el modelo como una imagen de contenedor que puede implementarse como un módulo de Azure IoT Edge.
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 2/10/2020
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: a5c754373ba9437c631e62acbb5d6d246db4c862
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2020
ms.locfileid: "77650765"
---
# <a name="tutorial-train-and-deploy-an-azure-machine-learning-model"></a>Tutorial: Entrenamiento e implementación de un modelo de Azure Machine Learning

> [!NOTE]
> Este artículo forma parte de un tutorial sobre el uso de Azure Machine Learning en IoT Edge. Si ha llegado directamente a este artículo, le recomendamos que comience con el [primer el artículo](tutorial-machine-learning-edge-01-intro.md) de la serie para obtener los mejores resultados.

En este artículo, realizará las tareas siguientes:

* Usará Azure Notebooks para entrenar un modelo de Machine Learning.
* Empaquetará el modelo entrenado como una imagen de contenedor.
* Implementará la imagen de contenedor como un módulo de Azure IoT Edge.

Azure Notebooks hace uso de un área de trabajo de Azure Machine Learning, un bloque fundamental para experimentar, entrenar e implementar modelos de Machine Learning.

Normalmente, los científicos de datos se encargan de los pasos descritos en este artículo.

## <a name="set-up-azure-notebooks"></a>Configuración de Azure Notebooks

Usamos Azure Notebooks para hospedar los dos Jupyter Notebook y los archivos auxiliares. A continuación, creamos y configuramos un proyecto de Azure Notebooks. Si no ha usado Azure Notebooks o Jupyter Notebooks, los siguientes son un par de documentos de introducción:

* **Inicio rápido:** [Creación y uso compartido de un cuaderno](../notebooks/quickstart-create-share-jupyter-notebook.md)
* **Tutorial:** [Creación y ejecución de un cuaderno de Jupyter con Python](../notebooks/tutorial-create-run-jupyter-notebook.md)

El uso de Azure Notebooks garantiza un entorno coherente para el ejercicio.

> [!NOTE]
> Una vez configurado, puede tener acceso al servicio de Azure Notebooks desde cualquier equipo. Durante el proceso de configuración, debe usar la máquina virtual de desarrollo que tiene todos los archivos que necesitará.

### <a name="create-an-azure-notebooks-account"></a>Creación de una cuenta de Azure Notebooks

Para usar Azure Notebooks, deberá crear una cuenta. Las cuentas de Azure Notebooks son independientes de las suscripciones a Azure.

1. Vaya a [Azure Notebooks](https://notebooks.azure.com).

1. Haga clic en **Iniciar sesión** en la esquina superior derecha de la página.

1. Inicie sesión con su cuenta profesional o educativa (Azure Active Directory) o su cuenta personal (cuenta de Microsoft).

1. Si no ha usado Azure Notebooks antes, se le pedirá que conceda acceso a la aplicación de Azure Notebooks.

1. Cree un Id. de usuario para Azure Notebooks.

### <a name="upload-jupyter-notebook-files"></a>Carga de archivos de Jupyter Notebook

Cargaremos archivos de cuaderno de ejemplo en un nuevo proyecto de Azure Notebooks.

1. En la página usuario de la nueva cuenta, seleccione **My projects** (Mis proyectos)en la barra de menús superior.

1. En el cuadro de diálogo **Create New Project** (Crear nuevo proyecto), proporcione un **nombre de proyecto** que también conforme automáticamente el **identificador del proyecto**.

1. Deje **Public** (Público) y **README** (Léame) sin seleccionar, ya que no hace falta que el proyecto sea público o tenga un archivo Léame.

1. Seleccione **Crear**.

1. Seleccione **Upload** (Cargar) —el icono de flecha hacia arriba— y elija **From Computer** (Desde el equipo).

1. Seleccione **Choose files** (Elegir archivos).

1. Vaya a **C:\source\IoTEdgeAndMlSample\AzureNotebooks**. Seleccione todos los archivos de la lista y haga clic en **Open** (Abrir).

1. Seleccione **Cargar** para empezar a cargarlos y, a continuación, seleccione **Listo** una vez completado el proceso.

### <a name="azure-notebook-files"></a>Archivos de Azure Notebooks

Vamos a revisar los archivos cargados en el proyecto de Azure Notebooks. Las actividades de esta parte del tutorial abarcan dos archivos de cuaderno, que usan algunos archivos auxiliares.

* **01-turbofan\_regression.ipynb:** Este cuaderno usa el área de trabajo de Machine Learning Service para crear y ejecutar un experimento de aprendizaje automático. En líneas generales, el cuaderno realiza los siguientes pasos:

  1. Descarga datos de la cuenta de Azure Storage generada por la herramienta de ejecución de dispositivos.
  1. Explora y prepara los datos, y usa los datos para entrenar el modelo clasificador.
  1. Evalúe el modelo desde el experimento mediante un conjunto de datos de prueba (Test\_FD003.txt).
  1. Publica el mejor modelo de clasificación en el área de trabajo de Machine Learning Service.

* **02-turbofan\_deploy\_model.ipynb:** este cuaderno toma el modelo creado en el cuaderno anterior y lo usa para crear una imagen de contenedor lista para implementarse en un dispositivo de Azure IoT Edge. El cuaderno realiza los siguientes pasos:

  1. Crea un script de puntuación para el modelo.
  1. Genera una imagen de contenedor mediante el modelo de clasificación que se guardó en el área de trabajo de Machine Learning Service.
  1. Implementa la imagen como un servicio web en Azure Container Instances.
  1. Usa el servicio web para validar el modelo y el trabajo de la imagen según lo previsto. La imagen validada se implementará en el dispositivo IoT Edge en la sección [Creación e implementación de módulos personalizados de IoT Edge](tutorial-machine-learning-edge-06-custom-modules.md) de este tutorial.

* **Test\_FD003.txt:** este archivo contiene los datos que usaremos como conjunto de prueba al validar el clasificador entrenado. Decidimos usar los datos de prueba tal, y como se proporcionan para el concurso original, como nuestro conjunto de pruebas por su sencillez.

* **RUL\_FD003.txt:** este archivo contiene la vida útil restante del último ciclo de todos los dispositivos en el archivo Test\_FD003.txt. Consulte los archivos readme.txt y Damage Propagation Modeling.pdf (modelado de propagación de daños) en C:\\source\\IoTEdgeAndMlSample\\data\\Turbofan para obtener una explicación detallada de los datos.

* **Utils.py:** contiene un conjunto de funciones de utilidad de Python para trabajar con los datos. El primer cuaderno contiene una explicación detallada de las funciones.

* **README.md:** archivo Léame que describe el uso de los cuadernos.  

## <a name="run-azure-notebooks"></a>Ejecución de Azure Notebooks

Ahora que se ha creado el proyecto, puede ejecutar los cuadernos. 

1. En la página del proyecto, seleccione **01-turbofan\_regression.ipynb**.

    ![Seleccionar el primer cuaderno que se ejecutará](media/tutorial-machine-learning-edge-04-train-model/select-turbofan-regression-notebook.png)

1. Si se le pide, elija el kernel 3.6 de Python en el cuadro de diálogo y seleccione **Set Kernel** (Establecer kernel).

1. Si el cuaderno aparece como **Not Trusted** (No de confianza), haga clic en el widget **No de confianza** en la parte superior derecha del cuaderno. Cuando se muestre el cuadro de diálogo, seleccione **Trust** (Confiar).

1. En el cuaderno, desplácese hacia abajo hasta la celda que sigue a las instrucciones de **Set global properties** (Establecer propiedades globales) que comienza con el código `AZURE_SUBSCRIPTION_ID =` y rellene los valores de la suscripción, la configuración y los recursos de Azure.

    ![Establecimiento de las propiedades globales en el cuaderno](media/tutorial-machine-learning-edge-04-train-model/set-global-properties.png)

1. Ejecute la celda seleccionando **Run** (Ejecutar) en la barra de herramientas.

    Cuando se ejecuta una celda, muestra un asterisco entre corchetes ([\*]). Cuando se completa la operación de la celda, el asterisco se reemplaza por un número y es posible que se muestre el resultado pertinente. Las celdas de un cuaderno se compilan secuencialmente y no puede haber más de una en ejecución a la vez.

    Siga las instrucciones del cuaderno. También puede usar opciones de ejecución del menú **Cell** (Celda), `Ctrl` + `Enter` para ejecutar una celda y `Shift` + `Enter` para ejecutar una celda y pasar a la celda siguiente.

    > [!TIP]
    > Para las operaciones de celda coherentes, evite ejecutar el mismo cuaderno desde varias pestañas del explorador.

1. Desplácese hacia abajo hasta la celda que sigue inmediatamente al texto de información general de **Create a workspace** (Crear un área de trabajo) y ejecute esa celda. En la salida de la celda, busque el vínculo que le indica que inicie sesión para autenticarse. 

    ![Solicitud de inicio de sesión para la autenticación de dispositivos](media/tutorial-machine-learning-edge-04-train-model/sign-in-prompt.png)

    Abra el vínculo y escriba el código especificado. Este procedimiento de inicio de sesión autentica el cuaderno de Jupyter Notebook para acceder a los recursos de Azure mediante la interfaz de la línea de comandos multiplataforma de Azure.  

    ![Aplicación de autenticación en la confirmación del dispositivo](media/tutorial-machine-learning-edge-04-train-model/cross-platform-cli.png)

1. En este punto, puede ejecutar el resto de las celdas. Lo ideal es ejecutar todas las celdas para que el código de las celdas se ejecute secuencialmente. Seleccione **Run All** (Ejecutar todas) en el menú **Cell** (Celda). Desplácese hacia arriba a través del cuaderno y revise cómo se completan las operaciones de celda.

    En la sección **Explore the data** (Exploración de los datos), puede revisar las celdas de la subsección **Sensor readings and RUL** (Lecturas de sensores y RUL) que representan gráficos de dispersión de las mediciones de sensores.

    ![Gráficos de dispersión de lecturas de sensores](media/tutorial-machine-learning-edge-04-train-model/sensor-readings.png)

1. Guarde el cuaderno y vuelva a la página del proyecto; para ello, haga clic en el nombre del proyecto en la esquina superior derecha del cuaderno o vuelva en el explorador.

1. Abra **02-turbofan\_deploy\_model.ipynb** y repita los pasos de este procedimiento para ejecutar el segundo cuaderno.

1. Guarde el cuaderno y vuelva a la página del proyecto; para ello, haga clic en el nombre del proyecto en la esquina superior derecha del cuaderno o vuelva en el explorador.

### <a name="verify-success"></a>Comprobación de que la operación se ha completado correctamente

Para confirmar que los cuadernos se han completado correctamente, compruebe si se han creado algunos elementos.

1. En la página de proyecto de Azure Notebooks, seleccione **Show hidden items** (Mostrar elementos ocultos) para que aparezcan los nombres de los elementos que comienzan con un punto.

1. Compruebe que se hayan creado los archivos siguientes:

    | Archivo | Descripción |
    | --- | --- |
    | ./aml_config/.azureml/config.json | Archivo de configuración usado para crear el área de trabajo de Azure Machine Learning. |
    | ./aml_config/model_config.json | Archivo de configuración que se necesitará para implementar el modelo en el área de trabajo de Machine Learning **turbofanDemo** en Azure. |
    | myenv.yml| Proporciona información sobre las dependencias para el modelo de Machine Learning implementado.|

1. Compruebe en Azure Portal que el área de trabajo de Machine Learning **turboFanDemo** existe en el grupo de recursos.

### <a name="debugging"></a>Depuración

Puede insertar instrucciones de Python en el cuaderno para su depuración, principalmente el comando `print()`. Si ve variables u objetos que no están definidos, ejecute las celdas en las que se declaren o se hayan creado instancias de ellos primero.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, usamos dos cuadernos Jupyter Notebook que se ejecutan en Azure Notebooks para usar los datos de los dispositivos turbofan para el entrenamiento de un clasificador de vida útil restante, guardar el clasificador como un modelo, crear una imagen de contenedor y para implementar y probar la imagen como un servicio web.

Consulte el artículo siguiente para crear un dispositivo IoT Edge.

> [!div class="nextstepaction"]
> [Configure un dispositivo IoT Edge](tutorial-machine-learning-edge-05-configure-edge-device.md)
