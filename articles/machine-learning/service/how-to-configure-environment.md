---
title: Configurar un entorno de desarrollo de Python
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo configurar un entorno de desarrollo al trabajar con Azure Machine Learning Service. En este artículo aprenderá a usar entornos de Conda, crear archivos de configuración y configurar instancias de Jupyter Notebook, Azure Notebooks, Azure Databricks, IDE, editores de código y Data Science Virtual Machine.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.component: core
ms.reviewer: larryfr
manager: cgronlun
ms.topic: conceptual
ms.date: 01/18/2019
ms.custom: seodec18
ms.openlocfilehash: 0f53b3cec843ca8016c61a360025b5e731b96f55
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2019
ms.locfileid: "54815878"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Configurar un entorno de desarrollo para Azure Machine Learning

En este artículo obtendrá información acerca de cómo configurar un entorno de desarrollo para trabajar con Azure Machine Learning Service. Azure Machine Learning Service es independiente de la plataforma de trabajo. 

Los únicos requisitos para el entorno de desarrollo son Python 3, Conda (para entornos aislados) y un archivo de configuración que contenga la información del área de trabajo de Azure Machine Learning.

Este artículo se centra en los siguientes entornos y herramientas:

* [Azure Notebooks](#aznotebooks): un servicio de Jupyter Notebook hospedado en Azure Cloud. Esta es la forma más fácil para empezar a trabajar, ya que el SDK de Azure Machine Learning ya está instalado.

* [Data Science Virtual Machine (DSVM)](#dsvm): es un entorno de desarrollo o experimentación preconfigurado en Azure Cloud que está diseñado para el trabajo con ciencia de datos y se puede implementar en instancias de máquina virtual solo con CPU o en instancias basadas en GPU. Python 3, Conda, Jupyter Notebooks y el SDK de Azure Machine Learning ya están instalados. La máquina virtual incluye editores, herramientas y marcos de aprendizaje automático o de aprendizaje profundo que son muy populares para desarrollar soluciones de Machine Learning. Es probable que se trate del entorno más completo de desarrollo para Machine Learning en la plataforma de Azure.

* [Jupyter Notebook](#jupyter): si ya usa instancias de Jupyter Notebook, el SDK incluye algunos elementos adicionales que debe instalar.

* [Visual Studio Code](#vscode): si usa Visual Studio Code, hay algunas extensiones útiles que puede instalar.

* [Azure Databricks](#aml-databricks): una plataforma popular para el análisis de datos basada en Apache Spark. Obtenga información para saber cómo obtener el SDK de Azure Machine Learning en el clúster para poder implementar los modelos.

Si ya tiene un entorno de Python 3, o tan solo quiere los pasos básicos para instalar el SDK, consulte la sección [Equipo local](#local).

## <a name="prerequisites"></a>Requisitos previos

- Un área de trabajo de Azure Machine Learning. Para crear el área de trabajo, consulte [Introducción a Azure Machine Learning Service](quickstart-get-started.md).

- Un administrador de paquetes [Continuum Anaconda](https://www.anaconda.com/download/) o [Miniconda](https://conda.io/miniconda.html).

    > [!IMPORTANT]
    > Si usa Azure Notebooks, no es necesario disponer de Anaconda o Miniconda.

- En Linux o en Mac OS, es necesario tener el shell de bash.

    > [!TIP]
    > Si usa Linux o Mac OS y un shell distinto de bash (por ejemplo, zsh), es posible que reciba errores al ejecutar algunos comandos. Para solucionar este problema, use el comando `bash` para iniciar un nuevo shell de bash y ejecutar sus comandos.

- En Windows, se necesita el símbolo del sistema o la solicitud de Anaconda (que se instala con Anaconda y Miniconda).

## <a id="aznotebooks"></a>Azure Notebooks

[Azure Notebooks](https://notebooks.azure.com) (versión preliminar) es un entorno de desarrollo interactivo en la nube de Azure. Esta es la manera más fácil de empezar a trabajar con el desarrollo de Azure Machine Learning.

* El SDK de Azure Machine Learning ya está instalado.
* Después de crear un área de trabajo de Azure Machine Learning Service en Azure Portal, puede hacer clic en un botón para configurar automáticamente el entorno de Azure Notebook para trabajar con el área de trabajo.

Para comenzar a desarrollar con Azure Notebooks, consulte [Introducción a Azure Machine Learning Service](quickstart-get-started.md).

De forma predeterminada, Azure Notebooks usa un nivel de servicio gratuito que está limitado a 4 GB de memoria y 1 GB de datos. Sin embargo, puede quitar estos límites si adjunta una instancia de Data Science Virtual Machine al proyecto de Azure Notebooks. Para más información, consulte [Manage and configure Azure Notebooks projects - Compute tier](/azure/notebooks/configure-manage-azure-notebooks-projects.md#compute-tier) (Administración y configuración de proyectos de Azure Notebooks: nivel de proceso).

## <a id="dsvm"></a>Data Science Virtual Machine

DSVM es una imagen de máquina virtual personalizada. Está diseñada para realizar trabajos de ciencia de datos que estén preconfigurados con:

  - Paquetes, como TensorFlow, PyTorch, Scikit-learn, XGBoost y el SDK de Azure Machine Learning
  - Herramientas de ciencia de datos populares, como Spark Standalone o Drill
  - Herramientas de Azure, como la CLI de Azure, AzCopy y el Explorador de Storage
  - Entornos de desarrollo integrado (IDE), como Visual Studio Code y PyCharm
  - Servidor de Jupyter Notebook

El SDK de Azure Machine Learning funciona en la versión de Ubuntu o de Windows de DSVM. Pero si también va a usar DSVM como destino de proceso, solo se admite Ubuntu.

Para usar DSVM como entorno de desarrollo, siga estos pasos:

1. Cree una instancia de DSVM en cualquiera de los siguientes entornos:

    * Azure Portal:

        * [Crear una instancia de Data Science Virtual Machine de Ubuntu](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/dsvm-ubuntu-intro)

        * [Crear una instancia de Data Science Virtual Machine de Windows](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/provision-vm)

    * La CLI de Azure:

        > [!IMPORTANT]
        > * Si usa la CLI de Azure, primero debe iniciar sesión en su suscripción de Azure con el comando `az login`.
        >
        > * Al usar los comandos en este paso, debe proporcionar un nombre de grupo de recursos, un nombre para la máquina virtual, un nombre de usuario y una contraseña.

        * Para crear una instancia de Data Science Virtual Machine de Ubuntu, use el siguiente comando:

            ```azurecli
            # create a Ubuntu DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            # If you need to create a new resource group use: "az group create --name YOUR-RESOURCE-GROUP-NAME --location YOUR-REGION (For example: westus2)"
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:linux-data-science-vm-ubuntu:linuxdsvmubuntu:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --generate-ssh-keys --authentication-type password
            ```

        * Para crear una instancia de Data Science Virtual Machine de Windows, use el siguiente comando:

            ```azurecli
            # create a Windows Server 2016 DSVM in your resource group
            # note you need to be at least a contributor to the resource group in order to execute this command successfully
            az vm create --resource-group YOUR-RESOURCE-GROUP-NAME --name YOUR-VM-NAME --image microsoft-dsvm:dsvm-windows:server-2016:latest --admin-username YOUR-USERNAME --admin-password YOUR-PASSWORD --authentication-type password
            ```    

2. El SDK de Azure Machine Learning ya está instalado en DSVM. Para usar el entorno de Conda que contiene el SDK, use uno de los siguientes comandos:

    * Para la instancia de DSVM de Ubuntu:

        ```shell
        conda activate py36
        ```

    * Para la instancia de DSVM de Windows:

        ```shell
        conda activate AzureML
        ```

1. Para verificar que puede acceder al SDK y comprobar la versión, use el código de Python siguiente:

    ```python
    import azureml.core
    print(azureml.core.VERSION)
    ```

1. Para configurar la instancia de DSVM que usará en el área de trabajo de Azure Machine Learning Service, consulte la sección [Crear un archivo de configuración del área de trabajo](#workspace).

Consulte [Data Science Virtual Machine](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/) para obtener más información.

## <a id="local"></a>Equipo local

Cuando use un equipo local (que también podría ser una máquina virtual remota), cree un entorno de Conda e instale el SDK siguiendo estos pasos:

1. Abra el shell o un símbolo del sistema.

1. Cree un entorno de Conda con los siguientes comandos:

    ```shell
    # create a new Conda environment with Python 3.6, NumPy, and Cython
    conda create -n myenv Python=3.6 cython numpy

    # activate the Conda environment
    conda activate myenv

    # On macOS run
    source activate myenv
    ```

    Es posible que tarde varios minutos en crear el entorno si hay que descargar Python 3.6 y otros componentes.

1. Instale el SDK de Azure Machine Learning con cuadernos adicionales y el SDK de preparación de datos mediante el comando siguiente:

     ```shell
    pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep
    ```

   > [!NOTE]
   > Si recibe un mensaje que indica que PyYAML no se puede desinstalar, use el comando siguiente en su lugar:
   >
   > `pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep --ignore-installed PyYAML`

   Puede que tarde varios minutos en instalar el SDK.

1. Instale paquetes para la experimentación de aprendizaje automático. Use el siguiente comando y reemplace *\<new package>* con el paquete que quiere instalar:

    ```shell
    conda install <new package>
    ```

1. Para comprobar si el SDK está instalado, use el código Python siguiente:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

### <a id="jupyter"></a>Jupyter Notebook

Las instancias de Jupyter Notebook forman parte de [Jupyter Project](https://jupyter.org/). Proporcionan una experiencia de codificación interactiva en la que se crean documentos que mezclan código activo con texto narrativo y gráficos. Jupyter Notebook proporciona una excelente manera de compartir los resultados con otros usuarios, ya que le permite guardar la salida de las secciones de código en el documento. Puede instalar cuadernos de Jupyter Notebook en una variedad de plataformas.

El procedimiento descrito en la sección [Equipo local](#local) permite instalar componentes opcionales para Jupyter Notebook. Para habilitar estos componentes en el entorno de Jupyter Notebook, siga estos pasos:

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

1. Para comprobar si Jupyter Notebook puede usar el SDK, abra un nuevo cuaderno y seleccione **myenv** como el kernel; a continuación, ejecute el siguiente comando en una celda del cuaderno:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Para configurar la instancia de Jupyter Notebook que usará en el área de trabajo de Azure Machine Learning Service, consulte la sección [Crear un archivo de configuración del área de trabajo](#workspace).

### <a id="vscode"></a>Visual Studio Code

Visual Studio Code es un editor de código multiplataforma. Se basa en una instalación local de Python 3 y de Conda para compatibilidad con Python, pero proporciona herramientas adicionales para trabajar con inteligencia artificial. También proporciona compatibilidad para seleccionar el entorno de Conda desde el editor de código.

Para usar Visual Studio Code para el desarrollo, siga estos pasos:

1. Para obtener información acerca de cómo usar Visual Studio Code para el desarrollo de Python, consulte [Get started with Python in VSCode](https://code.visualstudio.com/docs/python/python-tutorial) (Introducción a Python en VSCode).

1. Para seleccionar el entorno de Conda, abra VS Code y, a continuación, use Ctrl+Mayús+P (Windows y Linux) o Comando+Mayús+P (Mac).  
    Se abrirá la __paleta de comandos__. 

1. Escriba __Python: Select Interpreter__  (Seleccione intérprete) y, a continuación, seleccione el entorno de Conda.

1. Para confirmar que puede usar el SDK, cree y ejecute un nuevo archivo de Python (.py) que contenga el código siguiente:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Para instalar la extensión de Azure Machine Learning para Visual Studio Code, consulte [Tools for AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vscode-ai).

    Para obtener más información, consulte [Usar Azure Machine Learning para Visual Studio Code](how-to-vscode-tools.md).

<a name="aml-databricks"></a>

## <a name="azure-databricks"></a>Azure Databricks

Puede usar una versión personalizada del SDK de Azure Machine Learning para Azure Databricks destinada al aprendizaje automático personalizado de un extremo a otro. O bien, puede entrenar el modelo de Databricks e implementarlo usando [Visual Studio Code](how-to-vscode-train-deploy.md#deploy-your-service-from-vs-code).

Para preparar el clúster de Databricks y obtener los cuadernos de ejemplo:

1. Cree un [clúster de Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal) con las siguientes opciones:

    | Configuración | Valor |
    |----|---|
    | Nombre del clúster | nombredelclúster |
    | Entorno de tiempo de ejecución de Databricks | Cualquier entorno de tiempo de ejecución que no sea de Machine Learning (no ML 4.x, 5.x) |
    | Versión de Python | 3 |
    | Trabajos | 2 o más |

    Use esta configuración solo si va a usar Automated Machine Learning en Databricks:
    
    |   Configuración | Valor |
    |----|---|
    | Tipos de máquinas virtuales con nodos de trabajo | Se prefieren las máquinas virtuales optimizadas para memoria |
    | Habilitar escalado automático | Desactivar |
    
    El número de nodos de trabajo en el clúster de Databricks determina el número máximo de iteraciones simultáneas en la configuración de Automated Machine Learning.  

    La operación de creación del clúster tardará unos minutos. Espere hasta que el clúster se ejecute antes de continuar.

1. Instale y asocie el paquete del SDK de Azure Machine Learning al clúster.  

    * [Cree una biblioteca](https://docs.databricks.com/user-guide/libraries.html#create-a-library) con uno de los siguientes valores (_elija solo una de estas opciones_):
    
        * Para instalar el SDK de Azure Machine Learning _sin_ la funcionalidad de aprendizaje automático automatizado:
            | Configuración | Valor |
            |----|---|
            |Origen | Cargar un huevo o PyPi de Python
            |Nombre de PyPi | azureml-sdk[databricks]
    
        * Para instalar el SDK de Azure Machine Learning _con_ aprendizaje automático automatizado:
            | Configuración | Valor |
            |----|---|
            |Origen | Cargar un huevo o PyPi de Python
            |Nombre de PyPi | azureml-sdk[automl_databricks]
    
    * No seleccione **Attach automatically to all clusters** (Asociar automáticamente a todos los clústeres).

    * Seleccione **Asociar** junto al nombre del clúster.

    * Asegúrese de que no hay ningún error hasta que el estado cambie a **Attached** (Asociado). Este proceso tardará unos minutos.

    Si tiene una versión anterior del SDK, anule la selección del mismo desde las bibliotecas instaladas del clúster y muévalo a la Papelera. Instale la nueva versión del SDK y reinicie el clúster. Si hay un problema una vez hecho esto, desasocie y asocie nuevamente el clúster.

    Cuando haya terminado, la biblioteca se habrá asociado tal como se muestra en las siguientes imágenes. Tenga en cuenta estos [problemas comunes de Databricks](resource-known-issues.md#databricks).

    * Si ha instalado el SDK de Azure Machine Learning _sin_ el aprendizaje automático automatizado ![SDK sin el aprendizaje automático automatizado instalado en Databricks ](./media/how-to-configure-environment/amlsdk-withoutautoml.jpg)

    * Si ha instalado el SDK de Azure Machine Learning _con_ el aprendizaje automático automatizado ![SDK con el aprendizaje automático automatizado instalado en Databricks ](./media/how-to-configure-environment/automlonadb.jpg)

   Si se produce un error en este paso, reinicie el clúster mediante los siguientes pasos:

    a. En el panel izquierdo, seleccione **Clústeres**. 
   
   b. En la tabla, seleccione el nombre del clúster. 

   c. En la ficha **Bibliotecas**, seleccione **Reiniciar**.

1. Descargue el [archivo de Azure Databricks o del cuaderno del SDK de Azure Machine Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks/Databricks_AMLSDK_1-4_6.dbc).

   >[!Warning]
   > Muchos cuadernos de ejemplo están disponibles para usarse con el servicio Azure Machine Learning Service. Estos [cuadernos de ejemplo](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks) solo funcionan con Azure Databricks.

1.  [Importe el archivo](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-an-archive) en el clúster de Databricks y empiece a explorar tal como se describe en la página de [Notebooks de Machine Learning](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks).


## <a id="workspace"></a>Crear un archivo de configuración del área de trabajo

El archivo de configuración del área de trabajo es un archivo JSON que le indica al SDK cómo comunicarse con el área de trabajo de Azure Machine Learning Service. El archivo se denomina *config.json* y tiene el formato siguiente:

```json
{
    "subscription_id": "<subscription-id>",
    "resource_group": "<resource-group>",
    "workspace_name": "<workspace-name>"
}
```

Este archivo JSON debe estar en la estructura de directorios que contiene los scripts de Python o las instancias de Jupyter Notebook. Puede estar en el mismo directorio, en un subdirectorio denominado *aml_config*, o en un directorio principal.

Para utilizar este archivo desde el código, utilice `ws=Workspace.from_config()`. Este código carga la información del archivo y se conecta a su área de trabajo.

Hay tres formas de crear el archivo de configuración:

* **Siga el [inicio rápido de Azure Machine Learning](quickstart-get-started.md)**: Un archivo *config.json* se crea en la biblioteca de Azure Notebooks. Este archivo contiene información de configuración para el área de trabajo. Puede descargar o copiar el archivo *config.json* en otros entornos de desarrollo.

* **Cree el archivo manualmente**: Con este método deberá usar un editor de texto. Para ello, visite el área de trabajo de [Azure Portal](https://portal.azure.com) para encontrar los valores en el archivo de configuración. Copie los valores del nombre del área de trabajo, del grupo de recursos y del id. de suscripción para usarlos en el archivo de configuración.

     ![Azure Portal](./media/how-to-configure-environment/configure.png)

* **Cree el archivo mediante programación**: En el fragmento de código siguiente se muestra cómo conectarse a un área de trabajo proporcionando el id. de suscripción, el grupo de recursos y el nombre del área de trabajo. A continuación, se guarda la configuración del área de trabajo en el archivo:

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

    Este código escribe el archivo de configuración en el archivo *aml_config/config.json*.

## <a name="next-steps"></a>Pasos siguientes

- [Entrenar un modelo](tutorial-train-models-with-aml.md) en Azure Machine Learning con el conjunto de datos de MNIST]
- Consultar las referencias del [SDK de Azure Machine Learning para Python](https://aka.ms/aml-sdk)
- Obtener información sobre el [Azure Machine Learning Data Prep SDK](https://aka.ms/data-prep-sdk)
