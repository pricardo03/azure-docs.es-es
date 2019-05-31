---
title: Crear un área de trabajo
titleSuffix: Azure Machine Learning service
description: Usar el portal de Azure, el SDK, una plantilla o la CLI para crear el área de trabajo del servicio de Azure Machine Learning. Esta área de trabajo proporciona un lugar centralizado para trabajar con todos los artefactos que se crea al usar el servicio de Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: sgilley
ms.author: sgilley
author: sdgilley
ms.date: 05/21/2019
ms.openlocfilehash: 36f3d421ee0b41a0ff71b549a4d4b5646188c3fa
ms.sourcegitcommit: c05618a257787af6f9a2751c549c9a3634832c90
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66417358"
---
# <a name="create-an-azure-machine-learning-service-workspace"></a>Crear un área de trabajo del servicio de Azure Machine Learning

Para usar el servicio Azure Machine Learning, necesita un [ **área de trabajo de servicio de Azure Machine Learning**](concept-workspace.md).  Esta área de trabajo es el recurso de nivel superior para el servicio y le proporciona un lugar centralizado para trabajar con todos los artefactos que cree. 

En este artículo, aprenderá a crear un área de trabajo mediante cualquiera de estos métodos: 
* El [portal Azure](#portal) interfaz
* El [de Azure Machine Learning SDK para Python](#sdk)
* Una plantilla de Azure Resource Manager
* El [de Azure Machine Learning CLI](#cli)

Crear mediante los pasos descritos aquí en el área de trabajo puede usarse como un requisito previo para otros tutoriales y artículos de procedimientos.

Si desea usar un script para el programa de instalación automatizada machine learning en un entorno local de Python, consulte el [GitHub de Azure/MachineLearningNotebooks](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/automated-machine-learning) para obtener instrucciones.  

Cuando se crea un área de trabajo se agregan los siguientes recursos de Azure automáticamente (si están disponibles por regiones):
 
- [Azure Container Registry](https://azure.microsoft.com/services/container-registry/): Para minimizar los costos, ACR es **carga diferida** hasta que se crean las imágenes de implementación.
- [Azure Storage](https://azure.microsoft.com/services/storage/)
- [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 
- [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)

>[!Note]
>Igual que en otros servicios de Azure, ciertos límites y cuotas están asociados a Machine Learning. [Más información acerca de las cuotas y cómo solicitarlas.](how-to-manage-quotas.md)


## <a name="prerequisites"></a>Requisitos previos
Para crear un área de trabajo, necesita una suscripción de Azure. Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

## <a name="portal"></a> Portal de Azure

[!INCLUDE [aml-create-portal](../../../includes/aml-create-in-portal.md)]

Independientemente de cómo se creó, puede ver el área de trabajo en el [portal Azure](https://portal.azure.com/).  Consulte [ver un área de trabajo](how-to-manage-workspace.md#view) para obtener más información.

## <a name="sdk"></a> SDK de Python

Crear área de trabajo mediante el SDK de Python. Primero deberá instalar el SDK.

> [!IMPORTANT]
> Omitir la instalación del SDK si usa una máquina Virtual de ciencia de datos de Azure o Azure Databricks.
> * Las instancias de Azure Data Science Virtual Machine creadas después del 27 de septiembre de 2018 tienen preinstalado el SDK de Python. Omitir la instalación y comenzar con [crear un área de trabajo con el SDK](#sdk-create).
> * En el entorno de Azure Databricks, use los [pasos de instalación de Databricks](how-to-configure-environment.md#azure-databricks) en su lugar.

>[!NOTE]
> Siga estas instrucciones para instalar y usar el SDK desde el equipo local. Para usar Jupyter en una máquina virtual remota, establezca un control remoto de escritorio o X de la sesión de terminal.

Antes de instalar el SDK, se recomienda crear un entorno aislado de Python. Aunque en este artículo se usa [Miniconda](https://docs.conda.io/en/latest/miniconda.html), también puede usar la versión completa de [Anaconda](https://www.anaconda.com/) instalada o [Python virtualenv](https://virtualenv.pypa.io/en/stable/).

Las instrucciones de este artículo instalarán todos los paquetes que necesita para ejecutar los cuadernos de inicio rápido y el tutorial.  Otros cuadernos de ejemplo pueden requerir la instalación de componentes adicionales.  Para más información sobre estos componentes, consulte [Install the Azure Machine Learning SDK for Python](https://docs.microsoft.com/python/api/overview/azure/ml/install) (Instalación del SDK de Azure Machine Learning para Python).

### <a name="install-miniconda"></a>Instalación de Miniconda

[Descargue e instale Miniconda](https://docs.conda.io/en/latest/miniconda.html). Seleccione la versión 3.7 de Python para instalarla. No seleccione la versión 2.x de Python.  

### <a name="create-an-isolated-python-environment"></a>Creación de un entorno aislado de Python

1. Abra el símbolo del sistema de Anaconda y, después, cree un nuevo entorno de conda denominado *myenv* e instale Python 3.6.5. El SDK de Azure Machine Learning funcionará con Python 3.5.2 o posterior, pero los componentes de aprendizaje automático automatizado no son completamente funcionales en Python 3.7.  La creación del entorno tardará unos minutos mientras se descargan los componentes y los paquetes. 

    ```shell
    conda create -n myenv python=3.6.5
    ```

1. Active el entorno.

    ```shell
    conda activate myenv
    ```

1. Habilite los kernels de ipython específicos del entorno:

    ```shell
    conda install notebook ipykernel
    ```

    Después, cree el kernel:

    ```shell
    ipython kernel install --user
    ```

### <a name="install-the-sdk"></a>Instalación del SDK

1. En el entorno de conda activado, instale los componentes principales del SDK de Machine Learning con funcionalidades para cuadernos de Jupyter. La instalación tarda unos minutos en completarse, según la configuración de la máquina.

    ```shell
    pip install --upgrade azureml-sdk[notebooks]
    ```

1. Para usar este entorno para los tutoriales de Azure Machine Learning, instale estos paquetes.

    ```shell
    conda install -y cython matplotlib pandas
    ```

1. Para usar este entorno para los tutoriales de Azure Machine Learning, instale los componentes de aprendizaje automático automatizado.

    ```shell
    pip install --upgrade azureml-sdk[automl]
    ```

> [!IMPORTANT]
> En algunas herramientas de línea de comandos, es posible que deba agregar comillas, como se muestra a continuación:
> *  'azureml-sdk[notebooks]'
> * 'azureml-sdk[automl]'
>

### <a name='sdk-create'></a> Crear un área de trabajo con el SDK

Cree el área de trabajo en Jupyter Notebook con el SDK de Python.

1. Cree o cambie al directorio que desea usar para el inicio rápido y los tutoriales.

1. Para iniciar Jupyter Notebook, escriba este comando:

    ```shell
    jupyter notebook
    ```

1. En la ventana del explorador, cree un cuaderno nuevo mediante el kernel `Python 3` predeterminado. 

1. Para mostrar la versión del SDK, escriba y ejecute el código de Python siguiente en una celda del cuaderno:

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=import)]

1. Busque un valor para el parámetro `<azure-subscription-id>` en la [lista de suscripciones de Azure Portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Use cualquier suscripción en la que su rol sea propietario o colaborador. Para obtener más información sobre los roles, consulte [administrar el acceso a un área de trabajo de Azure Machine Learning](how-to-assign-roles.md) artículo.

   ```python
   from azureml.core import Workspace
   ws = Workspace.create(name='myworkspace',
                         subscription_id='<azure-subscription-id>', 
                         resource_group='myresourcegroup',
                         create_resource_group=True,
                         location='eastus2' 
                        )
   ```

   Cuando ejecute el código, es posible que se le pida iniciar sesión con la cuenta de Azure. Después de iniciar sesión, el token de autenticación se almacena en la caché local.

1. Para ver los detalles del área de trabajo, como el almacenamiento asociado, el registro de contenedor y el almacén de claves, escriba el código siguiente:

    [!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=getDetails)]


### <a name="write-a-configuration-file"></a>Escritura de un archivo de configuración

Guarde los detalles del área de trabajo en un archivo de configuración del directorio actual. Este archivo se denomina *.azureml/config.json*.  

Este archivo de configuración del área de trabajo facilita la carga posterior de la misma área de trabajo. Se puede cargar otros blocs de notas y las secuencias de comandos en el mismo directorio o en un subdirectorio con el código `ws=Workspace.from_config()` . 

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=writeConfig)]

La llamada API `write_config()` crea el archivo de configuración en el directorio actual. El *.azureml/config.json* archivo contiene lo siguiente:

```json
{
    "subscription_id": "<azure-subscription-id>",
    "resource_group": "myresourcegroup",
    "workspace_name": "myworkspace"
}
```

> [!TIP]
> Para usar el área de trabajo en Jupyter Notebooks que se encuentran en otros directorios o en scripts de Python, copie este archivo en ese directorio. El archivo puede estar en el mismo directorio, un subdirectorio denominado *.azureml*, o en un directorio primario.

## <a name="resource-manager-template"></a>Plantilla de Resource manager

Para crear un área de trabajo con una plantilla, consulte [crear un área de trabajo del servicio de Azure Machine Learning mediante una plantilla](how-to-create-workspace-template.md)

<a name="cli"></a>
## <a name="command-line-interface"></a>Interfaz de la línea de comandos

Para crear un área de trabajo con la CLI, consulte [usar la extensión de la CLI para el servicio de Azure Machine Learning](reference-azure-machine-learning-cli.md).

## <a name="clean-up-resources"></a>Limpieza de recursos 

[!INCLUDE [aml-delete-resource-group](../../../includes/aml-delete-resource-group.md)]

## <a name="next-steps"></a>Pasos siguientes

* Independientemente de cómo se creó, puede ver el área de trabajo en el [portal Azure](https://portal.azure.com/).  Consulte [ver un área de trabajo](how-to-manage-workspace.md#view) para obtener más información.

* Pruebe el área de trabajo con estos inicios rápidos y tutoriales.

    * Inicio rápido: [Ejecutar el cuaderno de Jupyter notebook en la nube](quickstart-run-cloud-notebook.md).
    * Inicio rápido: [Ejecute el cuaderno de Jupyter notebook en su propio servidor](quickstart-run-local-notebook.md).
    * Tutorial de dos partes: [Entrenar](tutorial-train-models-with-aml.md) y [implementar](tutorial-deploy-models-with-aml.md) un modo de clasificación de imagen.
    * Tutorial de dos partes: [Preparar datos](tutorial-data-prep.md) y [usar automatizada machine learning](tutorial-auto-train-models.md) para crear un modelo de regresión.

* Más información acerca de cómo [configurar un entorno de desarrollo](how-to-configure-environment.md).

* Obtenga más información sobre la [SDK de Azure Machine Learning para Python](https://aka.ms/aml-sdk).
