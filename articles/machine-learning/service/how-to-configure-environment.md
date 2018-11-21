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
ms.date: 11/6/2018
ms.openlocfilehash: 2fd2d35bde95a3e268f46b398f2163f9d40ab1ee
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "51613960"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Configurar un entorno de desarrollo para Azure Machine Learning

En este documento obtendrá información acerca de cómo configurar un entorno de desarrollo para trabajar con el servicio Azure Machine Learning. El servicio Azure Machine Learning es independiente de la plataforma de trabajo. Los únicos requisitos para el entorno de desarrollo son __Python 3__, __Conda__ (para entornos aislados) y un archivo de configuración que contenga la información del área de trabajo de Azure Machine Learning.

Este documento se centra en los siguientes entornos y herramientas específicos:

* [Azure Notebooks](#aznotebooks): es un servicio de Jupyter Notebook en la nube de Azure. Es __la forma más fácil__ para empezar a trabajar, ya que el SDK de Azure Machine Learning ya está instalado.
* [Data Science Virtual Machine](#dsvm): es una máquina virtual en la nube de Azure que está __diseñada para trabajar con ciencia de datos__. Python 3, Conda, Jupyter Notebooks y el SDK de Azure Machine Learning ya están instalados. La máquina virtual incluye editores, herramientas y marcos de Machine Learning populares para desarrollar soluciones de Machine Learning. Es probable que se trate __del entorno más completo__ de desarrollo para Machine Learning en la plataforma de Azure.
* [Jupyter Notebook](#jupyter): si ya usa instancias de Jupyter Notebook, el SDK incluye algunos elementos adicionales que debe instalar.
* [Visual Studio Code](#vscode): si usa Visual Studio Code, hay algunas extensiones útiles que puede instalar.

Si ya tiene un entorno de Python 3, o tan solo quiere los pasos básicos para instalar el SDK, consulte la sección [Equipo local](#local).

## <a name="prerequisites"></a>Requisitos previos

- Un área de trabajo de Azure Machine Learning. Siga los pasos de [Guía de inicio rápido: Introducción al servicio Azure Machine Learning](quickstart-get-started.md) para crear una.

- Un administrador de paquetes [Continuum Anaconda](https://www.anaconda.com/download/) o [Miniconda](https://conda.io/miniconda.html).

    > [!IMPORTANT]
    > Si usa Azure Notebooks, no es necesario disponer de Anaconda ni de Miniconda.

- En Linux o en Mac OS, es necesario el shell de bash.

    > [!TIP]
    > Si usa Linux o Mac OS y un shell distinto de bash (por ejemplo, zsh), es posible que reciba errores al ejecutar algunos comandos. Para solucionar este problema, use el comando `bash` para iniciar un nuevo shell de bash y ejecutar sus comandos.

- En Windows, se necesita el símbolo del sistema o la solicitud de Anaconda (que se instala con Anaconda y Miniconda).

## <a id="anotebooks"></a>Azure Notebooks

[Azure Notebooks](https://notebooks.azure.com) (versión preliminar) es un entorno de desarrollo interactivo en la nube de Azure. Es __la manera más fácil__ para empezar a trabajar con el desarrollo de Azure Machine Learning.

* El SDK de Azure Machine Learning __ya está instalado__.
* Después de crear un área de trabajo del servicio de Azure Machine Learning en Azure Portal, puede hacer clic en un botón para configurar automáticamente el entorno de Azure Notebook para trabajar con el área de trabajo.

Para comenzar a desarrollar con Azure Notebooks, siga el documento [Introducción al servicio Azure Machine Learning](quickstart-get-started.md).

## <a id="dsvm"></a>Data Science Virtual Machine

Data Science Virtual Machine (DSVM) es una imagen de máquina virtual personalizada que está **diseñada para trabajar con ciencia de datos**.  Incluye:

  - Herramientas de ciencia de datos populares
  - Entornos de desarrollo integrado (IDE), como PyCharm y RStudio
  - Paquetes como Jupyter Notebook y Tensorflow

El SDK de Azure Machine Learning funciona en la versión de Ubuntu o de Windows de DSVM. Para usar DSVM como entorno de desarrollo, siga estos pasos:

1. Para crear una instancia de Data Science Virtual Machine, siga los pasos de uno de los siguientes documentos:

    * [Crear una instancia de Data Science Virtual Machine de Ubuntu](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro)
    * [Crear una instancia de Data Science Virtual Machine de Windows](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/provision-vm)

1. El SDK de Azure Machine Learning **ya está instalado** en DSVM. Para usar el entorno de Conda que contiene el SDK, use uno de los siguientes comandos:

    * En la DSVM de __Ubuntu__, use este comando:

        ```shell
        conda activate py36
        ```

    * En la DSVM de __Windows__, use este comando:

        ```shell
        conda activate AzureML
        ```

1. Para verificar que puede acceder al SDK y comprobar la versión, use el código de Python siguiente:

    ```python
    import azureml.core
    print(azureml.core.VERSION)
    ```

1. Para configurar la instancia de DSVM para usar el área de trabajo del servicio de Azure Machine Learning, consulte la sección [Configurar área de trabajo](#workspace).

Para más información sobre Data Science Virtual Machine, vea [Data Science Virtual Machine](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/).

## <a id="local"></a>Equipo local

Cuando se usa un equipo local (que también podría ser una máquina virtual remota), use los pasos siguientes para crear un entorno de Conda e instalar el SDK:

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

   > [!NOTE]
   > Si recibe un mensaje que indica que `PyYAML` no se puede desinstalar, utilice el comando siguiente en su lugar:
   >
   > `pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep --ignore-installed PyYAML`

   Puede que tarde varios minutos en instalar el SDK.

1. Instale paquetes para la experimentación de aprendizaje automático. Use el siguiente comando y reemplace `<new package>` por el paquete que quiere instalar:

    ```shell
    conda install <new package>
    ```

1. Para comprobar si el SDK está instalado, use el código Python siguiente:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

### <a id="jupyter"></a>Jupyter Notebook

Las instancias de Jupyter Notebook forman parte de [Jupyter Project](https://jupyter.org/). Proporcionan una experiencia de codificación interactiva en la que se crean documentos que mezclan código activo con texto narrativo y gráficos. Jupyter Notebook proporciona una excelente manera de compartir los resultados con otros usuarios, ya que permite guardar la salida de las secciones de código en el documento. Puede instalar cuadernos de Jupyter Notebook en una variedad de plataformas.

Los pasos descritos en la sección [Equipo local](#local) permiten instalar componentes opcionales para Jupyter Notebook. Para habilitar estos componentes en el entorno de Jupyter Notebook, use los pasos siguientes:

1. Abra el shell o un símbolo del sistema.

1. Para instalar un servidor de Jupyter Notebook compatible con Conda y habilitar los widgets de experimento, use los comandos siguientes:

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

1. Para comprobar si Jupyter Notebook puede usar el SDK, abra un nuevo cuaderno y seleccione "myenv" como el kernel. A continuación, ejecute el siguiente comando en una celda del cuaderno:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Para configurar la instancia de Jupyter Notebook para usar el área de trabajo del servicio de Azure Machine Learning, consulte la sección [Configurar área de trabajo](#workspace).

### <a id="vscode"></a>Visual Studio Code

Visual Studio Code es un editor de código multiplataforma. Se basa en una instalación local de Python 3 y de Conda para compatibilidad con Python, pero proporciona herramientas adicionales para trabajar con inteligencia artificial. También proporciona compatibilidad para seleccionar el entorno de Conda desde el editor de código.

Para usar Visual Studio Code para desarrollar, use los pasos siguientes:

1. Para obtener información acerca de cómo usar Visual Studio Code para el desarrollo de Python, consulte el documento [Get started with Python in VSCode](https://code.visualstudio.com/docs/python/python-tutorial) (Introducción a Python en VSCode).

1. Para seleccionar el entorno de Conda, abra VS Code y, después, use __CTROL-Mayús-P__ (Windows y Linux) o __Comando-Mayús-P__ (Mac) para obtener la __paleta de comandos__. Escriba __Python: Select Interpreter__ y, después, seleccione el entorno de Conda.

1. Para validar que puede usar el SDK, cree un nuevo archivo de Python (.py) que contenga el código siguiente. Después, ejecute el archivo:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Para instalar la extensión Tools for AI de Visual Studio Code, consulte la página [Tools for AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai).

    Para obtener más información, consulte el documento [Use VS Code Tools for AI with Azure Machine Learning](how-to-vscode-tools.md) (Usar Visual Studio Code Tools for AI con Azure Machine Learning).

## <a id="workspace"></a>Crear un archivo de configuración del área de trabajo

El archivo de configuración del área de trabajo es un documento JSON que le indica al SDK cómo comunicarse con el área de trabajo del servicio Azure Machine Learning. El archivo se denomina `config.json` y tiene el formato siguiente:

```json
{
    "subscription_id": "<subscription-id>",
    "resource_group": "<resource-group>",
    "workspace_name": "<workspace-name>"
}
```

Este archivo debe estar en la estructura de directorios que contiene los scripts de Python o las instancias de Jupyter Notebook. Puede estar en el mismo directorio, en un subdirectorio denominado `aml_config`, o en un directorio primario.

Para utilizar este archivo desde el código, utilice `ws=Workspace.from_config()`. Este código carga la información del archivo y se conecta a su área de trabajo.

Hay tres formas de crear el archivo de configuración:

* Si sigue la [guía de inicio rápido de Azure Machine Learning](quickstart-get-started.md), se crea un archivo `config.json` en la biblioteca de Azure Notebooks. Este archivo contiene información de configuración para el área de trabajo. Puede descargar o copiar la información `config.json` a otros entornos de desarrollo.

* También puede **crear manualmente el archivo** mediante un editor de texto. Para ello, visite el área de trabajo de [Azure Portal](https://portal.azure.com) para encontrar los valores en el archivo de configuración. Copia los valores de __Nombre del área de trabajo__, de __Grupo de recursos__ y de __Id. de suscripción__ para usarlos en el archivo de configuración.
        ![Azure Portal](./media/how-to-configure-environment/configure.png)

* También puede **crear el archivo mediante programación**. El fragmento de código siguiente muestra cómo conectarse a un área de trabajo, proporcionando el id. de suscripción, el grupo de recursos y el nombre del área de trabajo. A continuación, guarda la configuración de área de trabajo en el archivo:

    ```python
    from azureml.core import Workspace

    subscription_id = '<subscription-id>'
    resource_group  = '<resource-group>'
    workspace_name  = '<workspace-name>'

    try:
        ws = Workspace(subscription_id = subscription_id, resource_group = resource_group, workspace_name = workspace_name)
        ws.write_config()
        print('Library configuration succeeded')
    except:
        print('Workspace not found')
    ```

    Este código escribe el archivo de configuración en el archivo `aml_config/config.json`.

## <a name="next-steps"></a>Pasos siguientes

- [Entrenar un modelo en Azure Machine Learning con el conjunto de datos de MNIST](tutorial-train-models-with-aml.md)
- [SDK de Azure Machine Learning para Python](https://aka.ms/aml-sdk)
- [SDK de preparación de datos de Azure Machine Learning](https://aka.ms/data-prep-sdk)