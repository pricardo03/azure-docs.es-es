---
title: Configuración de un entorno de desarrollo para Azure Machine Learning | Microsoft Docs
description: Obtenga información sobre cómo configurar un entorno de desarrollo al trabajar con el servicio Azure Machine Learning. En este documento, aprenda a usar entornos de Conda, crear archivos de configuración y configurar instancias de Jupyter Notebook, Azure Notebooks, IDE, editores de código y Data Science Virtual Machine.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.reviewer: larryfr
manager: cgronlun
ms.topic: conceptual
ms.date: 8/6/2018
ms.openlocfilehash: 675dae022376fc62292f3b079bd735939b9199c2
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220302"
---
# <a name="configure-a-development-environment-for-the-azure-machine-learning-service"></a>Configuración de un entorno de desarrollo para el servicio Azure Machine Learning

Obtenga información sobre cómo configurar un entorno de desarrollo al trabajar con el servicio Azure Machine Learning. Aprenderá a crear un archivo de configuración que asocia su entorno con un área de trabajo de Azure Machine Learning. También aprenderá a configurar los entornos de desarrollo siguientes:

* Instancias de Jupyter Notebook en su propio equipo
* Visual Studio Code
* El editor de código de su elección

El enfoque recomendado es usar [entornos virtuales conda](https://conda.io/docs/user-guide/tasks/manage-environments.html) de Continuum Anaconda para aislar el entorno de trabajo con el fin de evitar conflictos de dependencia entre los paquetes. En este artículo se muestran los pasos de configuración de un entorno de conda y su uso para Azure Machine Learning.


## <a name="prerequisites"></a>Requisitos previos

* Un área de trabajo de Azure Machine Learning. Para crear una, siga los pasos del documento [Get started with Azure Machine Learning service](quickstart-get-started.md) (Introducción al servicio Azure Machine Learning).

* Administrador de paquetes de [Continuum Anaconda](https://www.anaconda.com/download/) o [Miniconda](https://conda.io/miniconda.html).

 * Para el entorno de Visual Studio Code, la [extensión de Python](https://code.visualstudio.com/docs/python/python-tutorial).

## <a name="create-workspace-configuration-file"></a>Creación de un archivo de configuración del área de trabajo

El SDK usa el archivo de configuración del área de trabajo para comunicarse con el área de trabajo del servicio Azure Machine Learning.  Existen dos formas de obtener este archivo:

* Complete la [guía de inicio rápido](quickstart-get-started.md) para crear un archivo de configuración y el área de trabajo. El archivo `config.json` se crea automáticamente en Azure Notebooks.  Este archivo contiene información de configuración para el área de trabajo.  Descárguelo o cópielo en el mismo directorio que los scripts o cuadernos que hacen referencia a él.


* Cree el archivo de configuración usted mismo con los pasos siguientes:

    1. Abra el área de trabajo en [Azure Portal](https://portal.azure.com). Copie el __Nombre de área de trabajo__, __Grupo de recursos__ e __Id. de suscripción__. Estos valores se usan para crear el archivo de configuración.

        ![Azure Portal](./media/how-to-configure-environment/configure.png) 
    
    1. Cree el archivo con este código de Python. Ejecute el código en el mismo directorio que los scripts o cuadernos que hacen referencia al área de trabajo.
        ```
        from azureml.core import Workspace

        subscription_id ='<subscription-id>'
        resource_group ='<resource-group>'
        workspace_name = '<workspace-name>'
        
        try:
           ws = Workspace(subscription_id = subscription_id, resource_group = resource_group, workspace_name = workspace_name)
           ws.write_config()
           print('Library configuration succeeded')
        except:
           print('Workspace not found')
        ```
        Se escribe el siguiente archivo `aml_config/config.json`: 
    
        ```json
        {
        "subscription_id": "<subscription-id>",
        "resource_group": "<resource-group>",
        "workspace_name": "<workspace-name>"
        }
        ```
        Puede copiar el directorio `aml_config` o simplemente el archivo `config.json` en cualquier otro directorio que haga referencia al área de trabajo.

>[!NOTE] 
>Otros scripts o cuadernos del mismo directorio o uno inferiorp cargarán el área de trabajo con `ws=Workspace.from_config()`.

## <a name="azure-notebooks-and-data-science-virtual-machine"></a>Azure Notebooks y Data Science Virtual Machine

Azure Notebooks y Azure Data Science Virtual Machine (DSVM) están preconfigurados para funcionar con el servicio Azure Machine Learning. Los componentes necesarios, como el SDK de Azure Machine Learning, están preinstalados en estos entornos.

Azure Notebooks es un servicio de Jupyter Notebook en la nube de Azure. Data Science Virtual Machine es una imagen de VM que está preconfigurada para el trabajo de ciencia de datos. La VM incluye herramientas populares, IDE y paquetes, como Jupyter Notebook, PyCharm y Tensorflow.

Aún necesita un archivo de configuración de área de trabajo para usar estos entornos.

Para obtener más información sobre Data Science Virtual Machine, consulte la documentación de [Data Science Virtual Machine](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

Para obtener un ejemplo del uso de Azure Notebooks con el servicio Azure Machine Learning, consulte el documento [Get started with Azure Machine Learning service](quickstart-get-started.md) (Introducción al servicio Azure Machine Learning).

## <a name="configure-jupyter-notebooks-on-your-own-computer"></a>Configure Jupyter Notebook en su propio equipo

1. Abra un símbolo del sistema o el shell.

2. Para crear un entorno de conda, use los siguientes comandos:

    ```shell
    # create a new conda environment with Python 3.6, numpy and cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the conda environment
    conda activate myenv

    # If you are running Mac OS you should run
    source activate myenv
    ```

    Puede tardar varios minutos en crear el entorno, ya que puede ser necesario descargar Python 3.6 y otros componentes.

3. Para instalar el SDK de Azure Machine Learning con cuadernos adicionales, use el siguiente comando:

     ```shell
    pip install --upgrade azureml-sdk[notebooks,automl]
    ```

    Puede tardar varios minutos en instalar el SDK.

4. Para instalar paquetes para la experimentación de aprendizaje automático, use el siguiente comando y reemplace `<new package>` con el paquete que quiere instalar:

    ```shell
    conda install <new package>
    ```

5. Para instalar a un servidor de Jupyter Notebook compatible con conda y habilitar los widgets de experimento (para ver la información de la ejecución), use los comandos siguientes:

    ```shell
    # install Jupyter 
    conda install nb_conda

    # install experiment widget
    jupyter nbextension install --py --user azureml.train.widgets

    # enable experiment widget
    jupyter nbextension enable --py --user azureml.train.widgets
    ```

6. Para iniciar Jupyter Notebook, use el comando siguiente:

    ```shell
    jupyter notebook
    ```

7. Abra un nuevo cuaderno y seleccione "myenv" como kernel. A continuación, ejecute el comando siguiente en la celda del cuaderno para validar que tiene instalado el SDK de Azure Machine Learning:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

## <a name="configure-visual-studio-code"></a>Configuración deVisual Studio Code

1. Abra un símbolo del sistema o el shell.

2. Para crear un entorno de conda, use los siguientes comandos:

    ```shell
    # create a new conda environment with Python 3.6, numpy and cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the conda environment
    conda activate myenv

    # If you are running Mac OS you should run
    source activate myenv
    ```

2. Para instalar el SDK de Azure Machine Learning, use el siguiente comando:
 
    ```shell
    pip install --upgrade azureml-sdk[automl]
    ```

4. Para instalar Visual Studio Tools for AI, consulte la entrada de Visual Studio Marketplace para [Tools for AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai). 

5. Para instalar paquetes para la experimentación de aprendizaje automático, use el siguiente comando y reemplace `<new package>` con el paquete que quiere instalar:

    ```shell
    conda install <new package>
    ```

6. Inicie Visual Studio Code y luego use __Ctrl-Mayús-P__ para obtener la __Paleta de comandos__. Escriba *Python: Select Interpreter* y seleccione el entorno de conda que creó.

    > [!NOTE]
    > Visual Studio Code reconoce automáticamente los entornos de conda en el equipo. Para obtener más información, consulte la [documentación de Visual Studio Code](https://code.visualstudio.com/docs/python/environments#_conda-environments).

7. Para validar la configuración, use Visual Studio Code para crear un nuevo archivo de script de Python con el código siguiente y, a continuación, ejecútelo:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

## <a name="configure-code-editor-of-your-choice"></a>Configuración del editor de código de su elección

Para utilizar un editor de código personalizado con el SDK de Azure Machine Learning, cree primero el entorno de conda como se describió anteriormente. A continuación, siga las instrucciones de cada editor para usar el entorno de conda. Por ejemplo, las instrucciones de PyCharm se encuentran en [https://www.jetbrains.com/help/pycharm/2018.2/conda-support-creating-conda-virtual-environment.html](https://www.jetbrains.com/help/pycharm/2018.2/conda-support-creating-conda-virtual-environment.html).
 
## <a name="next-steps"></a>Pasos siguientes

* [Entrenar un modelo en Azure Machine Learning con el conjunto de datos de MNIST](tutorial-train-models-with-aml.md)
