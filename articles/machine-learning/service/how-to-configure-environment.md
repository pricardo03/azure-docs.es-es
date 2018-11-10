---
title: Configuración de un entorno de desarrollo para Azure Machine Learning | Microsoft Docs
description: Obtenga información sobre cómo configurar un entorno de desarrollo al trabajar con el servicio Azure Machine Learning. En este documento, aprenda a usar entornos de Conda, crear archivos de configuración y configurar instancias de Jupyter Notebook, Azure Notebooks, IDE, editores de código y Data Science Virtual Machine.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.component: core
ms.reviewer: larryfr
manager: cgronlun
ms.topic: conceptual
ms.date: 10/24/2018
ms.openlocfilehash: 6c2d5a776f603161ef730028168b91844c120aec
ms.sourcegitcommit: 48592dd2827c6f6f05455c56e8f600882adb80dc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50158999"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Configurar un entorno de desarrollo para Azure Machine Learning

En este artículo se enseña a configurar un entorno de desarrollo para trabajar con el servicio Azure Machine Learning, lo que incluye:

- Cómo crear un archivo de configuración que asocia su entorno con un área de trabajo del servicio Azure Machine Learning.
- Cómo configurar los entornos de desarrollo siguientes:
  - Instancias de Jupyter Notebook en su equipo
  - Visual Studio Code
  - Editor de código personalizado
- Cómo configurar un [entorno virtual de Conda](https://conda.io/docs/user-guide/tasks/manage-environments.html) y usarlo con Azure Machine Learning. Se recomienda usar Continuum Anaconda para aislar el entorno de trabajo a fin de evitar conflictos de dependencia entre paquetes.

## <a name="prerequisites"></a>Requisitos previos

- Configurar un área de trabajo del servicio Azure Machine Learning. Seguir los pasos de [Guía de inicio rápido: Introducción al servicio Azure Machine Learning](quickstart-get-started.md).
- Instalar el administrador de paquetes de [Continuum Anaconda](https://www.anaconda.com/download/) o [Miniconda](https://conda.io/miniconda.html).
- Si utiliza Visual Studio Code, obtener la [extensión de Python](https://code.visualstudio.com/docs/python/python-tutorial).

> [!NOTE]
> Puede probar los comandos de shell que se muestran en este artículo mediante bash (en Linux y Mac OS) o el símbolo del sistema (en Windows).

## <a name="create-a-workspace-configuration-file"></a>Crear un archivo de configuración del área de trabajo

El SDK de Azure Machine Learning utiliza el archivo de configuración del área de trabajo para comunicarse con el área de trabajo del servicio Azure Machine Learning.

- Para crear el archivo de configuración, complete el [inicio rápido de Azure Machine Learning](quickstart-get-started.md).
  - El proceso de inicio rápido crea un archivo `config.json` en Azure Notebooks. Este archivo contiene información de configuración para el área de trabajo.
  - Descargue o copie el archivo `config.json` en el mismo directorio que los scripts o cuadernos que hacen referencia a él.

- Como alternativa, puede crear el archivo manualmente siguiendo estos pasos:

    1. Abra el área de trabajo en [Azure Portal](https://portal.azure.com). Copie el __Nombre de área de trabajo__, __Grupo de recursos__ e __Id. de suscripción__. Estos valores se usan para crear el archivo de configuración.
        ![Azure Portal](./media/how-to-configure-environment/configure.png)

    1. Cree el archivo con el siguiente código de Python y asegúrese de ejecutar el código en el mismo directorio que los scripts o cuadernos que hacen referencia el área de trabajo:

        ```python
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
        El código escribe el siguiente archivo `aml_config/config.json`:

        ```json
        {
        "subscription_id": "<subscription-id>",
        "resource_group": "<resource-group>",
        "workspace_name": "<workspace-name>"
        }
        ```
        Puede copiar el directorio `aml_config` o simplemente el archivo `config.json` en cualquier otro directorio que haga referencia al área de trabajo.

       > [!NOTE]
       > Otros scripts o cuadernos del mismo directorio o los situados por debajo cargarán el área de trabajo con `ws=Workspace.from_config()`.

## <a name="azure-notebooks-and-data-science-virtual-machines"></a>Azure Notebooks y Data Science Virtual Machine

Azure Notebooks y Azure Data Science Virtual Machine (DSVM) vienen preconfigurados para trabajar con el servicio Azure Machine Learning. Estos entornos incluyen componentes necesarios tales como el SDK de Azure Machine Learning.

- Azure Notebooks es un servicio de Jupyter Notebook en la nube de Azure.
- Data Science Virtual Machine es una imagen de máquina virtual personalizada que está diseñada para trabajar con ciencia de datos. Incluye:
  - Herramientas más populares
  - Entornos de desarrollo integrados (IDE)
  - Paquetes tales como cuadernos de Jupyter notebook, PyCharm y Tensorflow
- Aún necesitará un archivo de configuración de área de trabajo para usar estos entornos.

Para obtener un ejemplo del uso de Azure Notebooks con el servicio Azure Machine Learning, vea [Get started with Azure Machine Learning service](quickstart-get-started.md) (Introducción al servicio Azure Machine Learning).

Para más información sobre Data Science Virtual Machine, vea [Data Science Virtual Machine](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a name="configure-jupyter-notebooks-on-your-computer"></a>Configurar Jupyter Notebook en su equipo

1. Abra el shell o un símbolo del sistema.

1. Cree un entorno de Conda con los siguientes comandos:

    ```shell
    # create a new conda environment with Python 3.6, numpy, and cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the conda environment
    conda activate myenv

    # On Mac OS run
    source activate myenv
    ```

    Es posible que tarde varios minutos en crear el entorno si hay que descargar Python 3.6 y otros componentes.

1. Instale el SDK de Azure Machine Learning con cuadernos adicionales y el SDK de preparación de datos mediante el comando siguiente:

     ```shell
    pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep
    ```

   Puede ver los documentos de referencia de Python para las clases y los métodos en los siguientes SDK:
   + [SDK de Azure Machine Learning para Python](https://aka.ms/aml-sdk)
   + [SDK de preparación de datos de Azure Machine Learning](https://aka.ms/data-prep-sdk)

   > [!NOTE]
   > Si recibe un mensaje que indica que `PyYAML` no se puede desinstalar, utilice el comando siguiente en su lugar:
   >
   > `pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep --ignore-installed PyYAML`

   Puede que tarde varios minutos en instalar el SDK.

1. Instale paquetes para la experimentación de aprendizaje automático. Use el siguiente comando y reemplace `<new package>` por el paquete que quiere instalar:

    ```shell
    conda install <new package>
    ```

1. Instale un servidor de Jupyter Notebook compatible con Conda y habilite los widgets de experimento (para ver información de la ejecución). Use los comandos siguientes:

    ```shell
    # install Jupyter
    conda install nb_conda

    # install experiment widget
    jupyter nbextension install --py --user azureml.train.widgets

    # enable experiment widget
    jupyter nbextension enable --py --user azureml.train.widgets
    ```

1. Abra Jupyter Notebook con el comando siguiente:

    ```shell
    jupyter notebook
    ```

1. Abra un nuevo cuaderno, seleccione "myenv" como kernel y luego compruebe que tiene instalado el SDK de Azure Machine Learning. Ejecute el siguiente comando en una celda del cuaderno:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

## <a name="configure-visual-studio-code"></a>Configuración deVisual Studio Code

1. Abra el shell o un símbolo del sistema.

1. Cree un entorno de Conda con los siguientes comandos:

    ```shell
    # create a new conda environment with Python 3.6, numpy and cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the conda environment
    conda activate myenv

    # If you are running Mac OS you should run
    source activate myenv
    ```

1. Instale el SDK de Azure Machine Learning y el SDK de preparación de datos con el comando siguiente:

    ```shell
    pip install --upgrade azureml-sdk[automl] azureml-dataprep
    ```

1. Instale la extensión Tools for AI de Visual Studio Code. Vea [Tools for AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai).

1. Instale paquetes para la experimentación de aprendizaje automático. Utilice el siguiente comando para reemplazar `<new package>` por el paquete que quiere instalar:

    ```shell
    conda install <new package>
    ```

1. Abra Visual Studio Code y luego use **CTRL-MAYÚS-P** (en Windows) o **COMANDO+MAYÚSCULAS-P** (en Mac OS) para obtener la **paleta de comandos**. Escriba _Python: Select Interpreter_ y seleccione el entorno de Conda que creó.

   > [!NOTE]
   > Visual Studio Code reconoce automáticamente los entornos de conda en el equipo. Para obtener más información, consulte la [documentación de Visual Studio Code](https://code.visualstudio.com/docs/python/environments#_conda-environments).

1. Valide la configuración mediante Visual Studio Code para crear un archivo de script de Python con el código siguiente y ejecútelo:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

## <a name="configure-a-custom-code-editor"></a>Configurar un editor de código personalizado

Puede usar el editor de código que prefiera con el SDK de Azure Machine Learning.

1. Cree el entorno de Conda tal y como se describe en el paso 2 de [Configuración de Visual Studio Code](#configure-visual-studio-code) anterior.
1. Siga las instrucciones de cada editor para usar el entorno de Conda. Por ejemplo, puede seguir las [instrucciones de PyCharm](https://www.jetbrains.com/help/pycharm/2018.2/conda-support-creating-conda-virtual-environment.html).

## <a name="next-steps"></a>Pasos siguientes

- [Entrenar un modelo en Azure Machine Learning con el conjunto de datos de MNIST](tutorial-train-models-with-aml.md)
