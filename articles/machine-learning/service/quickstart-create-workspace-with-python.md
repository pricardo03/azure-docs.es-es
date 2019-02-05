---
title: 'Guía de inicio rápido: Introducción a Python'
titleSuffix: Azure Machine Learning service
description: Introducción a Azure Machine Learning Service en Python. Use el SDK de Python para crear un área de trabajo, que es el bloque fundamental en la nube que se utiliza para experimentar, entrenar e implementar modelos de aprendizaje automático.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: quickstart
ms.reviewer: sgilley
author: hning86
ms.author: haining
ms.date: 01/22/2019
ms.custom: seodec18
ms.openlocfilehash: e0c235a9fd3898fa4525651d514c77432627603c
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55238965"
---
# <a name="quickstart-use-the-python-sdk-to-get-started-with-azure-machine-learning"></a>Inicio rápido: Uso del SDK de Python para empezar a usar Azure Machine Learning

En este artículo, usará el SDK de Azure Machine Learning para Python 3 para crear y después usar un [área de trabajo](concept-azure-machine-learning-architecture.md) de Azure Machine Learning Service. El área de trabajo se encuentra en la nube y es el bloque fundamental que se utiliza para experimentar, entrenar e implementar modelos de aprendizaje automático con Machine Learning.

Se empieza por configurar su propio entorno de Python y el servidor de Jupyter Notebook. Para ejecutarlo sin necesidad de instalación, consulte [Guía de inicio rápido: Uso de Azure Portal para empezar a trabajar con Azure Machine Learning](quickstart-get-started.md). 

Vea una versión en vídeo de este artículo de inicio rápido:

> [!VIDEO https://www.microsoft.com/en-us/videoplayer/embed/RE2G9N6]

En esta guía de inicio rápido:

* Instalará el SDK de Python.
* Crear un área de trabajo en la suscripción de Azure.
* Crear un archivo de configuración para dicha área de trabajo para usarlo posteriormente en otros cuadernos y scripts.
* Escribir código que registra valores dentro del área de trabajo.
* Verá los valores registrados en el área de trabajo.

Puede crear un área de trabajo y un archivo de configuración para usarlos como requisitos previos de otros artículos de procedimientos y tutoriales de Machine Learning. Igual que en otros servicios de Azure, ciertos límites y cuotas están asociados a Machine Learning. [Más información acerca de las cuotas y cómo solicitarlas.](how-to-manage-quotas.md)

Los siguientes recursos de Azure se agregan automáticamente al área de trabajo cuando estén disponibles por regiones:
 
- [Azure Container Registry](https://azure.microsoft.com/services/container-registry/)
- [Azure Storage](https://azure.microsoft.com/services/storage/)
- [Azure Application Insights](https://azure.microsoft.com/services/application-insights/) 
- [Azure Key Vault](https://azure.microsoft.com/services/key-vault/)

>[!NOTE]
> El código de este artículo requiere el SDK de Azure Machine Learning, versión 1.0.2 o posterior, y se ha probado con la versión 1.0.8.


Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](http://aka.ms/AMLFree).

## <a name="install-the-sdk"></a>Instalación del SDK

> [!IMPORTANT]
> Omita esta sección si utiliza Azure Data Science Virtual Machine o Azure Databricks.
> * Las instancias de Azure Data Science Virtual Machine creadas después del 27 de septiembre de 2018 tienen preinstalado el SDK de Python.
> * En el entorno de Azure Databricks, use los [pasos de instalación de Databricks](how-to-configure-environment.md#azure-databricks) en su lugar.

Antes de instalar el SDK, se recomienda crear un entorno aislado de Python. Aunque en este artículo se usa [Miniconda](https://docs.conda.io/en/latest/miniconda.html), también puede usar la versión completa de [Anaconda](https://www.anaconda.com/) instalada o [Python virtualenv](https://virtualenv.pypa.io/en/stable/).

### <a name="install-miniconda"></a>Instalación de Miniconda

[Descargue e instale Miniconda](https://docs.conda.io/en/latest/miniconda.html). Seleccione Python 3.7 o una versión posterior para instalar. No seleccione la versión 2.x de Python.  

### <a name="create-an-isolated-python-environment"></a>Creación de un entorno aislado de Python

1. Abra una ventana de línea de comandos, cree después un nuevo entorno de conda denominado *myenv* e instale Python 3.6. El SDK de Azure Machine Learning funcionará con Python 3.5.2 o posterior, pero los componentes de aprendizaje automático automatizado no son completamente funcionales en Python 3.7.

    ```shell
    conda create -n myenv -y Python=3.6
    ```

1. Active el entorno.

    ```shell
    conda activate myenv
    ```

### <a name="install-the-sdk"></a>Instalación del SDK

1. En el entorno de conda activado, instale los componentes principales del SDK de Machine Learning con funcionalidades para cuadernos de Jupyter.  La instalación tarda unos minutos en completarse, según la configuración de la máquina.

  ```shell
    pip install --upgrade azureml-sdk[notebooks]
    ```

1. Instale un servidor de Jupyter Notebook en el entorno de conda.

  ```shell
    conda install nb_conda
    ```

1. Para usar este entorno para los tutoriales de Azure Machine Learning, instale estos paquetes.

    ```shell
    conda install -y cython matplotlib pandas
    ```

1. Para usar este entorno para los tutoriales de Azure Machine Learning, instale los componentes de aprendizaje automático automatizado.

    ```shell
    pip install --upgrade azureml-sdk[automl]
    ```

## <a name="create-a-workspace"></a>Crear un área de trabajo

Cree el área de trabajo en Jupyter Notebook con el SDK de Python.

1. Cree o cambie al directorio que desea usar para el inicio rápido y los tutoriales.

1. Para iniciar Jupyter Notebook, escriba este comando:

    ```shell
    jupyter notebook
    ```

1. En la ventana del explorador, cree un cuaderno nuevo mediante el kernel `Python 3` predeterminado. 

1. Para mostrar la versión del SDK, escriba y ejecute el código de Python siguiente en una celda del cuaderno:

   [!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=import)]

1. Busque un valor para el parámetro `<azure-subscription-id>` en la [lista de suscripciones de Azure Portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Use cualquier suscripción en la que su rol sea propietario o colaborador.

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


## <a name="write-a-configuration-file"></a>Escritura de un archivo de configuración

Guarde los detalles del área de trabajo en un archivo de configuración del directorio actual. Dicho archivo se llama *aml_config\config.json*.  

Este archivo de configuración del área de trabajo facilita la carga posterior de la misma área de trabajo. Puede cargarlo con otros cuadernos y scripts en el mismo directorio o en un subdirectorio.  

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=writeConfig)]

La llamada API `write_config()` crea el archivo de configuración en el directorio actual. El archivo *config.json* contiene lo siguiente:

```json
{
    "subscription_id": "<azure-subscription-id>",
    "resource_group": "myresourcegroup",
    "workspace_name": "myworkspace"
}
```

## <a name="use-the-workspace"></a>Uso del área de trabajo

Ejecute código que use las API básicas del SDK para realizar un seguimiento de las ejecuciones del experimento:

1. Cree un experimento en el área de trabajo.
1. Registre un valor individual en el experimento.
1. Registre una lista de valores en el experimento.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=useWs)]

## <a name="view-logged-results"></a>Visualización de los resultados registrados
Cuando finaliza la ejecución, puede ver la ejecución del experimento en Azure Portal. Para imprimir una dirección URL que lleva a los resultados de la última ejecución, use el código siguiente:

```python
print(run.get_portal_url())
```

Use el vínculo para ver los valores registrados en Azure Portal en el explorador.

![Valores registrados en Azure Portal](./media/quickstart-create-workspace-with-python/logged-values.png)

## <a name="clean-up-resources"></a>Limpieza de recursos 
>[!IMPORTANT]
>Puede usar los recursos que creó aquí como requisitos previos para otros tutoriales y artículos de procedimientos de Machine Learning.

Si no tiene planeado usar los recursos que creó en este artículo, elimínelos para evitar incurrir en cualquier cargos.

[!code-python[](~/aml-sdk-samples/ignore/doc-qa/quickstart-create-workspace-with-python/quickstart.py?name=delete)]

## <a name="next-steps"></a>Pasos siguientes

En este artículo, creó los recursos necesarios para experimentar con los modelos e implementarlos. Ejecutó código en un cuaderno y exploró el historial de ejecuciones del código en el área de trabajo en la nube.

> [!div class="nextstepaction"]
> [Tutorial: Entrenamiento de un modelo de clasificación de imágenes](tutorial-train-models-with-aml.md)

También puede explorar [ejemplos más avanzados en GitHub](https://aka.ms/aml-notebooks).
