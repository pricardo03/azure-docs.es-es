---
title: Configuración del entorno de desarrollo de Python
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo configurar un entorno de desarrollo al trabajar con el servicio Azure Machine Learning. En este documento, aprenda a usar entornos de Conda, crear archivos de configuración y configurar instancias de Jupyter Notebook, Azure Notebooks, IDE, editores de código y Data Science Virtual Machine.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.component: core
ms.reviewer: larryfr
manager: cgronlun
ms.topic: conceptual
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 59f847dc38cddfd9185cfd169cf1ef34c744f8f7
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53192641"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Configurar un entorno de desarrollo para Azure Machine Learning

En este documento obtendrá información acerca de cómo configurar un entorno de desarrollo para trabajar con el servicio Azure Machine Learning. El servicio Azure Machine Learning es independiente de la plataforma de trabajo. Los únicos requisitos para el entorno de desarrollo son __Python 3__, __Conda__ (para entornos aislados) y un archivo de configuración que contenga la información del área de trabajo de Azure Machine Learning.

Este documento se centra en los siguientes entornos y herramientas específicos:

* [Azure Notebooks](#aznotebooks): Servicio de Jupyter Notebook hospedado en la nube de Azure. Es __la forma más fácil__ para empezar a trabajar, ya que el SDK de Azure Machine Learning ya está instalado.
* [Data Science Virtual Machine](#dsvm): Un __entorno de desarrollo/experimentación preconfigurado__ en la nube de Azure que está __diseñado para el trabajo de ciencia de datos__ y se puede implementar en cualquier CPU para instancias de VM solo con CPU o basadas en GPU. Python 3, Conda, Jupyter Notebooks y el SDK de Azure Machine Learning ya están instalados. La máquina virtual incluye editores, herramientas y marcos de Machine Learning o de aprendizaje profundo populares para desarrollar soluciones de Machine Learning. Es probable que se trate __del entorno más completo__ de desarrollo para Machine Learning en la plataforma de Azure.
* [Jupyter Notebooks](#jupyter): si ya usa instancias de Jupyter Notebook, el SDK incluye algunos elementos adicionales que debe instalar.
* [Visual Studio Code](#vscode): si usa Visual Studio Code, hay algunas extensiones útiles que puede instalar.
* [Azure Databricks](#aml-databricks): una plataforma popular para el análisis de datos basada en Apache Spark. Sepa cómo obtener el SDK de Azure Machine Learning en el clúster para poder implementar los modelos.

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

Data Science Virtual Machine (DSVM) es una imagen de máquina virtual personalizada que está **diseñada para trabajar con ciencia de datos** y que se configura previamente con:

  - Paquetes como Tensorflow, Pytorch, scikit-learn, Xgboost y Azure ML SDK
  - Herramientas de ciencia de datos populares como Spark independiente o Drill
  - Herramientas de Azure como la CLI, Azcopy y el explorador de Azure
  - Entornos de desarrollo integrado (IDE), como Visual Studio Code, PyCharm y RStudio
  - Servidor de Jupyter Notebook 

El SDK de Azure Machine Learning funciona en la versión de Ubuntu o de Windows de DSVM. Para usar Data Science Virtual Machine como un entorno de desarrollo, siga estos pasos:

1. Para crear una instancia de Data Science Virtual Machine, siga uno de los siguientes métodos:

    * En Azure Portal:

        * [Creación de una instancia de Data Science Virtual Machine](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro) de __Ubuntu__

        * [Creación de una instancia de Data Science Virtual Machine](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/provision-vm) de __Windows__

    * En la CLI de Azure:

        > [!IMPORTANT]
        > Si usa la CLI de Azure, primero debe iniciar sesión en su suscripción de Azure con el comando `az login`.
        >
        > Al utilizar los comandos en este paso, debe proporcionar un nombre de grupo de recursos, un nombre para la máquina virtual, un nombre de usuario y una contraseña.

        * Para crear una instancia de Data Science Virtual Machine de __Ubuntu__, use el siguiente comando:

            ```azurecli
            # create a Ubuntu DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            # If you need to create a new resource group use: "az group create --name YOUR-RESOURCE-GROUP-NAME --location YOUR-REGION (For example: westus2)"
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:linux-data-science-vm-ubuntu:linuxdsvmubuntu:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --generate-ssh-keys --authentication-type password
            ```

        * Para crear una instancia de Data Science Virtual Machine de __Windows__, use el siguiente comando:

            ```azurecli
            # create a Windows Server 2016 DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:dsvm-windows:server-2016:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --authentication-type password
            ```    

2. El SDK de Azure Machine Learning **ya está instalado** en DSVM. Para usar el entorno de Conda que contiene el SDK, use uno de los siguientes comandos:

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

1. Para instalar un servidor de Jupyter Notebook compatible con Conda, use el comando siguiente:

    ```shell
    # install Jupyter
    conda install nb_conda
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

1. Para instalar la extensión de Azure Machine Learning para Visual Studio Code, consulte la página [Tools for AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai).

    Para más información, consulte [Uso de Azure Machine Learning para Visual Studio Code](how-to-vscode-tools.md).

<a name="aml-databricks"></a>

## <a name="azure-databricks"></a>Azure Databricks

Puede usar una versión personalizada del SDK de Azure Machine Learning para Azure Databricks destinada al aprendizaje automático personalizado de un extremo a otro. O bien, entrene el modelo dentro de Databricks y use [Visual Studio Code](how-to-vscode-train-deploy.md#deploy-your-service-from-vs-code) para implementarlo.

Para preparar el clúster de Databricks y obtener los cuadernos de ejemplo:

1. Cree un [clúster de Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) con la versión 4.x del entorno de ejecución de Databricks (con preferencia, de alta simultaneidad) con **Python 3**. 

1. Cree una biblioteca para [instalar y adjuntar](https://docs.databricks.com/user-guide/libraries.html#create-a-library) el paquete PyPi del SDK de Azure Machine Learning para Python `azureml-sdk[databricks]` al clúster. Cuando haya terminado, verá la biblioteca conectada como se muestra en esta imagen. Tenga en cuenta estos [problemas comunes de Databricks](resource-known-issues.md#databricks).

   ![SDK instalado en Databricks ](./media/how-to-azure-machine-learning-on-databricks/sdk-installed-on-databricks.jpg)

   Si se produce un error en este paso, reinicie el clúster:
   1. En el panel izquierdo, seleccione `Clusters`. Seleccione el nombre del clúster en la tabla. 
   1. En la pestaña `Libraries`, seleccione `Restart`.

1. Descargue el [archivo de Azure Databricks o del cuaderno del SDK de Azure Machine Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/databricks/Databricks_AMLSDK_github.dbc).

   >[!Warning]
   > Muchos cuadernos de ejemplo están disponibles para usarse con el servicio Azure Machine Learning Service. Estos cuadernos de ejemplo solo funcionan con Azure Databricks: https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks

1.  [Importe este archivo](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-an-archive) en el clúster de Databricks y empiece a explorar según se [describe aquí](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks).


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
