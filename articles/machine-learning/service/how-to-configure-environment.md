---
title: Configurar un entorno de desarrollo de Python
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo configurar un entorno de desarrollo al trabajar con Azure Machine Learning Service. En este artículo aprenderá a usar entornos de Conda, crear archivos de configuración y configurar su propio servidor de cuadernos basado en la nube, Jupyter Notebook, Azure Databricks, Azure Notebooks, IDE, editores de código y Data Science Virtual Machine.
services: machine-learning
author: rastala
ms.author: roastala
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.topic: conceptual
ms.date: 05/14/2019
ms.custom: seodec18
ms.openlocfilehash: 7be6c9eda6d0a70d929efe4c00f661eb67105820
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65606415"
---
# <a name="configure-a-development-environment-for-azure-machine-learning"></a>Configurar un entorno de desarrollo para Azure Machine Learning

En este artículo obtendrá información acerca de cómo configurar un entorno de desarrollo para trabajar con Azure Machine Learning Service. Azure Machine Learning Service es independiente de la plataforma de trabajo.

Los únicos requisitos para el entorno de desarrollo son Python 3, Anaconda (para entornos aislados) y un archivo de configuración que contenga la información del área de trabajo de Azure Machine Learning.

Este artículo se centra en los siguientes entornos y herramientas:

* Su propia [máquina virtual de cuadernos basada en la nube](#notebookvm): use un recurso de proceso en su estación de trabajo para ejecutar cuadernos de Jupyter Notebook. Esta es la forma más fácil para empezar a trabajar, ya que el SDK de Azure Machine Learning ya está instalado.

* [Data Science Virtual Machine (DSVM)](#dsvm): es un entorno de desarrollo o experimentación preconfigurado en Azure Cloud que está diseñado para el trabajo con ciencia de datos y se puede implementar en instancias de máquina virtual solo con CPU o en instancias basadas en GPU. Python 3, Conda, Jupyter Notebooks y el SDK de Azure Machine Learning ya están instalados. La máquina virtual incluye editores, herramientas y marcos de aprendizaje automático o de aprendizaje profundo que son muy populares para desarrollar soluciones de Machine Learning. Es probable que se trate del entorno más completo de desarrollo para Machine Learning en la plataforma de Azure.

* [Jupyter Notebook](#jupyter): si ya usa instancias de Jupyter Notebook, el SDK incluye algunos elementos adicionales que debe instalar.

* [Visual Studio Code](#vscode): si usa Visual Studio Code, hay algunas extensiones útiles que puede instalar.

* [Azure Databricks](#aml-databricks): una plataforma popular para el análisis de datos basada en Apache Spark. Obtenga información para saber cómo obtener el SDK de Azure Machine Learning en el clúster para poder implementar los modelos.

* [Azure Notebooks](#aznotebooks): un servicio de Jupyter Notebook hospedado en Azure Cloud. Además, es una forma fácil de empezar a trabajar, ya que el SDK de Azure Machine Learning ya está instalado.  

Si ya tiene un entorno de Python 3, o tan solo quiere los pasos básicos para instalar el SDK, consulte la sección [Equipo local](#local).

## <a name="prerequisites"></a>Requisitos previos

Un área de trabajo de Azure Machine Learning. Para crear el área de trabajo, vea [Crear un área de trabajo de Azure Machine Learning Service](setup-create-workspace.md). Un área de trabajo es todo lo que necesita para empezar a trabajar con su propio [servidor de cuadernos basado en la nube](#notebookvm), una instancia de [DSVM](#dsvm), [Azure Databricks](#aml-databricks) o [Azure Notebooks](#aznotebooks).

Para instalar el entorno de SDK para el [equipo local](#local), el [servidor de Jupyter Notebook](#jupyter) o [Visual Studio Code](#vscode), también necesita:

- El administrador de paquetes de [Anaconda](https://www.anaconda.com/download/) o [Miniconda](https://conda.io/miniconda.html).

- En Linux o en Mac OS, es necesario tener el shell de bash.

    > [!TIP]
    > Si usa Linux o Mac OS y un shell distinto de bash (por ejemplo, zsh), es posible que reciba errores al ejecutar algunos comandos. Para solucionar este problema, use el comando `bash` para iniciar un nuevo shell de bash y ejecutar sus comandos.

- En Windows, se necesita el símbolo del sistema o la solicitud de Anaconda (que se instala con Anaconda y Miniconda).

## <a id="notebookvm"></a>Su propia máquina virtual de cuadernos basada en la nube

La máquina virtual de cuadernos (versión preliminar) es una estación de trabajo de Azure segura y basada en la nube que proporciona a los científicos de datos un servidor de Jupyter Notebook, JupyterLab y un entorno de aprendizaje automático completamente preparado. 

La máquina virtual de cuadernos se distingue por ser un elemento: 

+ **Seguro**. Puesto que el acceso a la máquina virtual y los cuadernos está protegido con HTTPS y Azure Active Directory de forma predeterminada, los profesionales de TI pueden aplicar fácilmente el inicio de sesión único y otras características de seguridad, como la autenticación multifactor.

+ **Preconfigurado**. Este entorno de aprendizaje automático de Python completamente preparado tiene su origen en la popular Data Science VM de IaaS e incluye lo siguiente:
  + SDK de Python de Azure ML (versión más reciente)
  + Configuración automática para que funcione con el área de trabajo
  + Servidor de Jupyter Notebook
  + IDE de JupyterLab Notebook
  + Controladores de GPU preconfigurados 
  + Selección de marcos de aprendizaje profundo
 

  Si le interesa el código, la máquina virtual incluye tutoriales y ejemplos que le ayudarán a obtener información sobre el uso de Azure Machine Learning Service. Los cuadernos de ejemplo se almacenan en la cuenta de Azure Blob Storage del área de trabajo, de modo que pueden compartirse entre máquinas virtuales. Cuando se ejecutan, también tienen acceso a los almacenes de datos y recursos de proceso del área de trabajo. 

+ **Configuración simple**. Cree una en cualquier momento desde el área de trabajo de Azure Machine Learning. Basta con que proporcione un nombre y especifique un tipo de máquina virtual de Azure. Pruébelo ahora con este [Inicio rápido: Uso de un servidor de cuadernos basado en la nube para empezar a trabajar con Azure Machine Learning](quickstart-run-cloud-notebook.md).

+ **Personalizable**. Aunque es una oferta de máquina virtual administrada y segura, usted conservará el acceso completo a las capacidades de hardware y podrá personalizarlo todo lo que quiera. Por ejemplo, puede crear rápidamente una máquina virtual con la tecnología NVidia V100 más reciente para llevar a cabo la depuración paso a paso de la novedosa arquitectura de red neuronal.

[Detenga la máquina virtual de cuadernos](quickstart-run-cloud-notebook.md#stop-the-notebook-vm) para dejar de incurrir en cargos por su uso. 

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

Cuando use un equipo local (que también podría ser una máquina virtual remota), cree un entorno de Anaconda e instale el SDK siguiendo estos pasos:

1. Descargue e instale [Anaconda](https://www.anaconda.com/distribution/#download-section) (versión 3.7 de Python) si aún no lo tiene.

1. Abra un símbolo del sistema de Anaconda y cree un entorno con los comandos siguientes:

    Ejecute el comando siguiente para crear el entorno.

    ```shell
    conda create -n myenv python=3.6.5
    ```

    Después, active el entorno.

    ```shell
    conda activate myenv
    ```

    En este ejemplo se crea un entorno con Python 3.6.5, pero se puede elegir cualquier versión secundaria. La compatibilidad con el SDK no puede garantizarse con determinadas versiones principales (se recomienda la 3.5 y versiones posteriores). Además, se aconseja probar una versión principal o secundaria diferente en el entorno de Anaconda si se experimentan errores. La creación del entorno tardará unos minutos mientras se descargan los componentes y los paquetes.

1. Ejecute los comandos siguientes en el nuevo entorno para habilitar los kernels de IPython específicos del entorno. Así se garantizará el comportamiento esperado de importación de paquetes y kernel al trabajar con cuadernos de Jupyter Notebook en entornos de Anaconda:

    ```shell
    conda install notebook ipykernel
    ```

    Después, ejecute el siguiente comando para crear el kernel:

    ```shell
    ipython kernel install --user
    ```

1. Use los comandos siguientes para instalar los paquetes:

    Este comando instala el SDK básico de Azure Machine Learning con elementos adicionales de cuadernos y automl. El elemento adicional `automl` es una instalación de gran tamaño y puede quitarse de los corchetes si no se van a ejecutar experimentos automatizados de aprendizaje automático. El elemento adicional `automl` también incluye de forma predeterminada el SDK de preparación de datos de Azure Machine Learning como una dependencia.

     ```shell
    pip install azureml-sdk[notebooks,automl]
    ```

    Use este comando para instalar solo el SDK de preparación de datos de Azure Machine Learning:

    ```shell
    pip install azureml-dataprep
    ```

   > [!NOTE]
   > Si recibe un mensaje que indica que PyYAML no se puede desinstalar, use el comando siguiente en su lugar:
   >
   > `pip install --upgrade azureml-sdk[notebooks,automl] azureml-dataprep --ignore-installed PyYAML`

   El SDK tardará varios minutos en instalarse. Vea la [guía de instalación](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py) para obtener más información sobre las opciones de instalación.

1. Instale otros paquetes para experimentar con el aprendizaje automático.

    Use cualquiera de los comandos siguientes y reemplace *\<new package>* por el paquete que quiera instalar. La instalación de paquetes a través de `conda install` requiere que el paquete forme parte de los canales actuales (se pueden agregar nuevos canales en la nube de Anaconda).

    ```shell
    conda install <new package>
    ```

    También puede instalar paquetes a través de `pip`.

    ```shell
    pip install <new package>
    ```

### <a id="jupyter"></a>Jupyter Notebook

Las instancias de Jupyter Notebook forman parte de [Jupyter Project](https://jupyter.org/). Proporcionan una experiencia de codificación interactiva en la que se crean documentos que mezclan código activo con texto narrativo y gráficos. Jupyter Notebook proporciona una excelente manera de compartir los resultados con otros usuarios, ya que le permite guardar la salida de las secciones de código en el documento. Puede instalar cuadernos de Jupyter Notebook en una variedad de plataformas.

Mediante el procedimiento descrito en la sección [Equipo local](#local) se instalan los componentes necesarios para ejecutar cuadernos de Jupyter Notebook en un entorno de Anaconda. Para habilitar estos componentes en el entorno de Jupyter Notebook, siga estos pasos:

1. Abra un símbolo del sistema de Anaconda y active el entorno.

    ```shell
    conda activate myenv
    ```

1. Inicie el servidor de Jupyter Notebook con el comando siguiente:

    ```shell
    jupyter notebook
    ```

1. Para comprobar si Jupyter Notebook puede usar el SDK, cree un cuaderno **Nuevo**, seleccione **Python 3** como kernel y ejecute el comando siguiente en una celda del cuaderno:

    ```python
    import azureml.core
    azureml.core.VERSION
    ```

1. Si encuentra problemas al importar los módulos y recibe un error de tipo `ModuleNotFoundError`, asegúrese de que el kernel de Jupyter está conectado a la ruta de acceso correcta del entorno. Para ello, ejecute el código siguiente en una celda del cuaderno.

    ```python
    import sys
    sys.path
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
Azure Databricks es un entorno basado en Apache Spark de la nube de Azure. Proporciona un entorno de colaboración basado en cuadernos con un clúster de proceso basado en CPU o GPU.

Funcionamiento de Azure Databricks con Azure Machine Learning Service:
+ Puede entrenar un modelo con Spark MLlib e implementar el modelo en ACI o AKS desde Azure Databricks. 
+ También puede usar funcionalidades de [aprendizaje automático automatizado](concept-automated-ml.md) en un SDK de Azure ML especial con Azure Databricks.
+ Puede usar Azure Databricks como destino de proceso desde una [canalización de Azure Machine Learning](concept-ml-pipelines.md). 

### <a name="set-up-your-databricks-cluster"></a>Configuración del clúster de Databricks

Cree un [clúster de Databricks](https://docs.microsoft.com/azure/azure-databricks/quickstart-create-databricks-workspace-portal). Algunas opciones solo se aplican si instala el SDK para el aprendizaje automático automatizado en Databricks.
**La operación de creación del clúster tardará unos minutos.**

Use estos valores de configuración:

| Configuración |Se aplica a| Valor |
|----|---|---|
| Nombre del clúster |Siempre| nombredelclúster |
| Entorno de tiempo de ejecución de Databricks |Siempre| Cualquier entorno de tiempo de ejecución que no sea de Machine Learning (no ML 4.x, 5.x) |
| Versión de Python |Siempre| 3 |
| Trabajos |Siempre| 2 o más |
| Tipos de máquinas virtuales con nodos de trabajo <br>(determina el número máximo de iteraciones simultáneas) |Machine Learning Automatizado<br>solo| Se prefieren las máquinas virtuales optimizadas para memoria |
| Habilitar escalado automático |Machine Learning Automatizado<br>solo| Desactivar |

Espere hasta que el clúster se ejecute antes de continuar.

### <a name="install-the-correct-sdk-into-a-databricks-library"></a>Instalación del SDK correcto en una biblioteca de Databricks
Una vez que se esté ejecutando el clúster, [cree una biblioteca](https://docs.databricks.com/user-guide/libraries.html#create-a-library) para adjuntar al clúster el paquete del SDK adecuado de Azure Machine Learning. 

1. Elija **solo una** opción (no se admite ninguna otra instalación de SDK).

   |Elementos adicionales del&nbsp;paquete&nbsp;del SDK|Origen|Nombre de&nbsp;PyPi&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|
   |----|---|---|
   |Para Databricks| Cargar un huevo o PyPi de Python | azureml-sdk[databricks]|
   |Para Databricks con<br> funcionalidades de ML automatizado| Cargar un huevo o PyPi de Python | azureml-sdk[automl_databricks]|

   > [!Warning]
   > No se pueden instalar otros elementos adicionales del SDK. Elija solo una de las opciones anteriores: [databricks] o [automl_databricks].

   * No seleccione **Attach automatically to all clusters** (Asociar automáticamente a todos los clústeres).
   * Seleccione **Asociar** junto al nombre del clúster.

1. Supervise los errores hasta que el estado cambie a **Asociado**, lo que podría tardar varios minutos.  Si se produce un error en este paso, compruebe lo siguiente: 

   Pruebe a reiniciar el clúster de la manera siguiente:
   1. En el panel izquierdo, seleccione **Clústeres**.
   1. En la tabla, seleccione el nombre del clúster.
   1. En la ficha **Bibliotecas**, seleccione **Reiniciar**.
      
   También debe tener en cuenta lo siguiente:
   + En la configuración de Automl, al usar Azure Databricks, agregue los siguientes parámetros:
       1. ```max_concurrent_iterations``` se basa en el número de nodos de trabajo del clúster. 
        2. ```spark_context=sc``` se basa en el contexto de Spark predeterminado. 
   + Si tiene una versión anterior del SDK, anule la selección del mismo desde las bibliotecas instaladas del clúster y muévalo a la Papelera. Instale la nueva versión del SDK y reinicie el clúster. Si hay un problema una vez hecho esto, desasocie y asocie nuevamente el clúster.

Si la instalación fue correcta, la biblioteca importada debería tener un aspecto similar a uno de los ejemplos siguientes:
   
SDK para Databricks **_sin_** aprendizaje automático automatizado ![SDK de Azure Machine Learning para Databricks](./media/how-to-configure-environment/amlsdk-withoutautoml.jpg)

SDK para Databricks **CON** aprendizaje automático automatizado ![SDK con aprendizaje automático automatizado instalado en Databricks](./media/how-to-configure-environment/automlonadb.jpg)

### <a name="start-exploring"></a>Comienzo de la exploración

Pruebe lo siguiente:
+ Descargue el [archivo de cuadernos](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks/Databricks_AMLSDK_1-4_6.dbc) para el SDK de Azure Machine Learning o Azure Databricks e [impórtelo](https://docs.azuredatabricks.net/user-guide/notebooks/notebook-manage.html#import-an-archive) en el clúster de Databricks.  
  Aunque hay muchos cuadernos de ejemplo disponibles, **solo [estos cuadernos de ejemplo](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/azure-databricks) funcionan con Azure Databricks.**
  
+ Obtenga información sobre cómo [crear una canalización con Databricks como proceso de entrenamiento](how-to-create-your-first-pipeline.md).

## <a id="aznotebooks"></a>Azure Notebooks

[Azure Notebooks](https://notebooks.azure.com) (versión preliminar) es un entorno de desarrollo interactivo en la nube de Azure. Es una manera sencilla de empezar a trabajar con el desarrollo de Azure Machine Learning.

* El SDK de Azure Machine Learning ya está instalado.
* Después de crear un área de trabajo de Azure Machine Learning Service en Azure Portal, puede hacer clic en un botón para configurar automáticamente el entorno de Azure Notebook para trabajar con el área de trabajo.

Utilice [Azure Portal](https://portal.azure.com) para comenzar a usar Azure Notebooks.  Abra el área de trabajo y, en la sección **Introducción**, seleccione **Introducción a Azure Notebooks**.

De forma predeterminada, Azure Notebooks usa un nivel de servicio gratuito que está limitado a 4 GB de memoria y 1 GB de datos. Sin embargo, puede quitar estos límites si adjunta una instancia de Data Science Virtual Machine al proyecto de Azure Notebooks. Para más información, consulte [Manage and configure Azure Notebooks projects - Compute tier](/azure/notebooks/configure-manage-azure-notebooks-projects#compute-tier) (Administración y configuración de proyectos de Azure Notebooks: nivel de proceso).

## <a id="workspace"></a>Crear un archivo de configuración del área de trabajo

El archivo de configuración del área de trabajo es un archivo JSON que le indica al SDK cómo comunicarse con el área de trabajo de Azure Machine Learning Service. El archivo se denomina *config.json* y tiene el formato siguiente:

```json
{
    "subscription_id": "<subscription-id>",
    "resource_group": "<resource-group>",
    "workspace_name": "<workspace-name>"
}
```

Este archivo JSON debe estar en la estructura de directorios que contiene los scripts de Python o las instancias de Jupyter Notebook. Puede estar en el mismo directorio, en un subdirectorio denominado *.azureml* o en un directorio principal.

Para utilizar este archivo desde el código, utilice `ws=Workspace.from_config()`. Este código carga la información del archivo y se conecta a su área de trabajo.

Hay tres formas de crear el archivo de configuración:

* **Siga los pasos indicados en [Crear un área de trabajo de Azure Machine Learning Service](setup-create-workspace.md#sdk)** : Un archivo *config.json* se crea en la biblioteca de Azure Notebooks. Este archivo contiene información de configuración para el área de trabajo. Puede descargar o copiar el archivo *config.json* en otros entornos de desarrollo.

* **Descargue el archivo**: En [Azure Portal](https://ms.portal.azure.com), seleccione **Descargar config.json** desde la sección **Información general** del área de trabajo.

     ![Portal de Azure](./media/how-to-configure-environment/configure.png)

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

    Este código escribe el archivo de configuración en el archivo *.azureml/config.json*.


## <a name="next-steps"></a>Pasos siguientes

- [Entrenar un modelo](tutorial-train-models-with-aml.md) en Azure Machine Learning con el conjunto de datos de MNIST
- Consultar las referencias del [SDK de Azure Machine Learning para Python](https://aka.ms/aml-sdk)
- Obtener información sobre el [paquete de preparación de datos de Azure Machine Learning](https://aka.ms/data-prep-sdk)
