---
title: 'Tutorial: Entrenamiento e implementación de un modelo: Machine Learning en Azure IoT Edge'
description: 'Tutorial: Entrene un modelo de Machine Learning mediante Azure Machine Learning y, a continuación, empaquete el modelo como una imagen de contenedor que puede implementarse como un módulo de Azure IoT Edge.'
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 11/11/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 8cb3d0329c41256823a66fc3276711c43da61cdd
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/15/2019
ms.locfileid: "74112470"
---
# <a name="tutorial-train-and-deploy-an-azure-machine-learning-model"></a>Tutorial: Entrenamiento e implementación de un modelo de Azure Machine Learning

> [!NOTE]
> Este artículo forma parte de un tutorial sobre el uso de Azure Machine Learning en IoT Edge. Si ha llegado directamente a este artículo, le recomendamos que comience con el [primer el artículo](tutorial-machine-learning-edge-01-intro.md) de la serie para obtener los mejores resultados.

En este artículo, primero se usa Azure Notebooks para entrenar un modelo de Machine Learning mediante Azure Machine Learning y, a continuación, se empaqueta el modelo como una imagen de contenedor que puede implementarse como un módulo de Azure IoT Edge. Azure Notebooks hace uso de un área de trabajo de Azure Machine Learning, un bloque fundamental para experimentar, entrenar e implementar modelos de aprendizaje automático.

Las actividades en esta parte del tutorial se dividen en dos cuadernos.

* **01-turbofan\_regression.ipynb:** este cuaderno le guía a través de los pasos necesarios para entrenar y publicar un modelo con Azure Machine Learning. En términos generales, los pasos necesarios suelen ser:

  1. Descargar, preparar y explorar los datos de entrenamiento.
  2. Usar el área de trabajo de servicio para crear y ejecutar un experimento de aprendizaje automático.
  3. Evaluar los resultados del modelo a partir del experimento.
  4. Publicar el mejor modelo en el área de trabajo de servicio.

* **02-turbofan\_deploy\_model.ipynb:** este cuaderno toma el modelo creado en el cuaderno anterior y lo usa para crear una imagen de contenedor lista para implementarse en un dispositivo de Azure IoT Edge.

  1. Cree un script de puntuación para el modelo.
  2. Cree y publique la imagen.
  3. Implemente la imagen como un servicio web en la instancia de Azure Container.
  4. Use el servicio web para validar el modelo y el trabajo de la imagen según lo previsto.

Normalmente, los científicos de datos se encargan de los pasos descritos en este artículo.

## <a name="set-up-azure-notebooks"></a>Configuración de Azure Notebooks

Usamos Azure Notebooks para hospedar los dos Jupyter Notebook y los archivos auxiliares. A continuación, creamos y configuramos un proyecto de Azure Notebooks. Si no ha usado Azure Notebooks o Jupyter Notebooks, los siguientes son un par de documentos de introducción:

* **Inicio rápido:** [Creación y uso compartido de un cuaderno](../notebooks/quickstart-create-share-jupyter-notebook.md)
* **Tutorial:** [Creación y ejecución de un cuaderno de Jupyter con Python](../notebooks/tutorial-create-run-jupyter-notebook.md)

Al igual que con la máquina virtual de desarrollador, Azure Notebooks garantiza un entorno coherente para el ejercicio.

> [!NOTE]
> Una vez configurado, puede tener acceso al servicio de Azure Notebooks desde cualquier equipo. Durante el proceso de configuración, debe usar la máquina virtual de desarrollo que tiene todos los archivos que necesitará.

### <a name="create-an-azure-notebooks-account"></a>Creación de una cuenta de Azure Notebooks

Las cuentas de Azure Notebooks son independientes de las suscripciones a Azure. Para usar Azure Notebooks, deberá crear una cuenta.

1. Vaya a [Azure Notebooks](https://notebooks.azure.com).

2. Haga clic en **Iniciar sesión** en la esquina superior derecha de la página.

3. Inicie sesión con su cuenta profesional o educativa (Azure Active Directory) o su cuenta personal (cuenta de Microsoft).

4. Si no ha usado Azure Notebooks antes, se le pedirá que conceda acceso a la aplicación de Azure Notebooks.

5. Cree un Id. de usuario para Azure Notebooks.

### <a name="upload-jupyter-notebooks-files"></a>Carga de archivos de Jupyter Notebooks

En este paso, se crea un nuevo proyecto de Azure Notebooks y se cargan archivos en él. En concreto, los archivos que se cargan son:

* **01-turbofan\_regression.ipynb**: un archivo de Jupyter Notebooks que le guía por el proceso de descargar los datos generados por el agente de dispositivo de la cuenta de almacenamiento de Azure. Es decir, le muestra cómo examinar y preparar los datos para entrenar el clasificador, entrenar el modelo, probar los datos con el conjunto de datos de prueba en el archivo Test\_FD003.txt y, por último, guardar el modelo del clasificador en el área de trabajo de Machine Learning Service.

* **02-turbofan\_deploy\_model.ipynb:** cuaderno de Jupyter que le guía por el proceso de usar el modelo de clasificación guardado en el área de trabajo de Machine Learning Service para generar una imagen de contenedor. Una vez creada la imagen, el cuaderno le guía por el proceso de implementación de la imagen como un servicio web para que pueda validar que funciona según lo previsto. La imagen validada se implementará en el dispositivo IoT Edge en la sección [Creación e implementación de módulos personalizados de IoT Edge](tutorial-machine-learning-edge-06-custom-modules.md) de este tutorial.

* **Test\_FD003.txt:** este archivo contiene los datos que usaremos como conjunto de prueba al validar el clasificador entrenado. Decidimos usar los datos de prueba tal y como se proporcionan para el concurso original como nuestro conjunto de pruebas para que el ejemplo sea más simple.

* **RUL\_FD003.txt:** este archivo contiene la vida útil restante del último ciclo de todos los dispositivos en el archivo Test\_FD003.txt. Consulte los archivos **readme.txt** y **Damage Propagation Modeling.pdf** (modelado de propagación de daños) en C:\\source\\IoTEdgeAndMlSample\\data\\Turbofan para obtener una explicación detallada de los datos.

* **Utils.py:** contiene un conjunto de funciones de utilidad de Python para trabajar con los datos. El primer cuaderno contiene una explicación detallada de las funciones.

* **README.md:** archivo Léame que describe el uso de los cuadernos.

Cree un nuevo proyecto y cargue los archivos en el cuaderno.

1. Seleccione **Mis proyectos** desde la barra de menús superior.

1. Seleccione **+ New project** (+ Nuevo proyecto). Proporcione un nombre y un identificador. No hace falta que el proyecto sea público o tenga un archivo Léame.

1. Seleccione **Cargar** y elija **From Computer** (Desde el equipo).

1. Seleccione **Choose files** (Elegir archivos).

1. Vaya a **C:\source\IoTEdgeAndMlSample\AzureNotebooks**. Seleccione todos los archivos y haga clic en **Abrir**.

1. Seleccione **Cargar** para empezar a cargarlos y, a continuación, seleccione **Listo** una vez completado el proceso.

## <a name="run-azure-notebooks"></a>Ejecución de Azure Notebooks

Ya que el proyecto se ha creado, ejecute el cuaderno **01-turbofan\_regression.ipynb**.

1. En la página del proyecto turbofan, seleccione **01-turbofan\_regression.ipynb**.

    ![Seleccionar el primer cuaderno que se ejecutará](media/tutorial-machine-learning-edge-04-train-model/select-turbofan-regression-notebook.png)

2. Si se le pide, elija el kernel 3.6 de Python en el cuadro de diálogo y seleccione **Set Kernel** (Establecer kernel).

3. Si el cuaderno aparece como **Not Trusted** (No de confianza), haga clic en el widget **No de confianza** en la parte superior derecha del cuaderno. Cuando se muestre el cuadro de diálogo, seleccione **Trust** (Confiar).

4. Siga las instrucciones del cuaderno.

    * `Ctrl + Enter` ejecuta una celda.
    * `Shift + Enter` ejecuta una celda y navega a la celda siguiente.
    * Cuando se ejecuta una celda, tiene un asterisco entre corchetes con el siguiente aspecto: **[\*]** . Cuando se completa, el asterisco se reemplaza con un número y es posible que se muestre el resultado pertinente. Dado que las celdas suelen basarse en los resultados de celdas anteriores, solo puede ejecutar una a la vez.

5. Cuando haya terminado de ejecutar el cuaderno **01-turbofan\_regression.ipynb**, vuelva a la página del proyecto.

6. Abra **02-turbofan\_deploy\_model.ipynb** y repita los pasos de esta sección para ejecutar el segundo cuaderno.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, usamos dos cuadernos Jupyter Notebook que se ejecutan en Azure Notebooks para usar los datos de los dispositivos turbofan para el entrenamiento de un clasificador de vida útil restante, guardar el clasificador como un modelo, crear una imagen de contenedor y para implementar y probar la imagen como un servicio web.

Consulte el artículo siguiente para crear un dispositivo IoT Edge.

> [!div class="nextstepaction"]
> [Configure un dispositivo IoT Edge](tutorial-machine-learning-edge-05-configure-edge-device.md)
