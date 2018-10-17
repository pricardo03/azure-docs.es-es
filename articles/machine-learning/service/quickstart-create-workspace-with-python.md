---
title: 'Guía de inicio rápido: Uso del SDK de Python para crear un área de trabajo del servicio de aprendizaje automático (Azure Machine Learning)'
description: Introducción a Azure Machine Learning.  Instale el SDK de Python y úselo para crear un área de trabajo. Esta área de trabajo se encuentra en la nube y es el bloque fundamental para experimentar, entrenar e implementar modelos de aprendizaje automático con el servicio Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: quickstart
ms.reviewer: sgilley
author: hning86
ms.author: haining
ms.date: 09/24/2018
ms.openlocfilehash: ee24c1797d0f52d2529ed583a0cfe90cc9e27035
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "49067775"
---
# <a name="quickstart-use-python-to-get-started-with-azure-machine-learning"></a>Guía de inicio rápido: Uso de Python para empezar a usar Azure Machine Learning

En esta guía de inicio rápido, usará el SDK de Azure Machine Learning para Python para crear y después usar un [área de trabajo](concept-azure-machine-learning-architecture.md) del servicio Machine Learning. Esta área de trabajo es el bloque fundamental para experimentar, entrenar e implementar modelos de aprendizaje automático en la nube con el servicio Azure Machine Learning.

En este tutorial, instalará el SDK de Python y:
* Creará un área de trabajo en la suscripción de Azure
* Creará un archivo de configuración para dicho área de trabajo para usarlo posteriormente en otros blocs de notas y scripts
* Escribirá código que registra valores dentro del área de trabajo
* Ver los valores registrados en el área de trabajo

El área de trabajo de trabajo y su archivo de configuración que cree en esta guía de inicio rápido se pueden usar como requisitos previos para otros tutoriales y artículos de procedimientos de Azure Machine Learning. Igual que en otros servicios de Azure, hay límites y cuotas asociados al servicio Azure Machine Learning. [Más información acerca de las cuotas y cómo solicitarlas.](how-to-manage-quotas.md)

Para su comodidad, los siguientes recursos de Azure se agregarán automáticamente al área de trabajo cuando estén disponibles de forma regional: [Container Registry](https://azure.microsoft.com/services/container-registry/), [Storage](https://azure.microsoft.com/services/storage/), [Application Insights](https://azure.microsoft.com/services/application-insights/) y [Key Vault](https://azure.microsoft.com/services/key-vault/).

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.


##  <a name="install-the-sdk"></a>Instalación del SDK

**Omita esta sección si usa** una instancia de Data Science Virtual Machine (DSVM) creada después del 27 de septiembre de 2018, ya que dichas instancias de DSVM vienen con el SDK de Python preinstalado.

Antes de instalar el SDK, se recomienda crear un entorno aislado de Python. Aunque en esta guía de inicio rápido se usa [Miniconda](https://conda.io/docs/user-guide/install/index.html), también puede usar la versión completa de [Anaconda](https://www.anaconda.com/) instalada o [Python virtualenv](https://virtualenv.pypa.io/en/stable/).

### <a name="install-miniconda"></a>Instalación de Miniconda


[Descargue](https://conda.io/miniconda.html) e instale Miniconda. Elija Python 3.7 o cualquier versión posterior. No elija la versión 2.x de Python.

### <a name="create-an-isolated-python-environment"></a>Creación de un entorno aislado de Python 

Inicie una ventana de línea de comandos y cree un nuevo entorno de conda denominado `myenv` con Python 3.6.

```sh
conda create -n myenv -y Python=3.6
```

Active el entorno.

  ```sh
  conda activate myenv
  ```

### <a name="install-the-sdk"></a>Instalación del SDK

En el entorno de conda activado, instale el SDK. Este código instala los componentes principales del SDK de Azure Machine Learning, así como un servidor de Jupyter Notebook en el entorno de conda `myenv`.  Esta instalación tarda **aproximadamente 4 minutos** en completarse.

```sh
pip install azureml-sdk[notebooks]
```

## <a name="create-a-workspace"></a>Creación de un área de trabajo

Inicie Jupyter Notebook, para lo que debe escribir este comando.
```sh
jupyter notebook
```

En la ventana del explorador, cree un cuaderno nuevo mediante el kernel `Python 3` predeterminado. 

Muestre la versión de SDK, para lo que debe escribir el siguiente código de Python en una celda del cuaderno y ejecútelo.

```python
import azureml.core
print(azureml.core.VERSION)
```

Cree un nuevo grupo de recursos de Azure y un nuevo área de trabajo.

Busque un valor para `<azure-subscription-id>` en la [lista de suscripciones de Azure Portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade). Use cualquier suscripción en la que su rol sea propietario o colaborador.

```python
from azureml.core import Workspace
ws = Workspace.create(name='myworkspace',
                      subscription_id='<azure-subscription-id>',
                      resource_group='myresourcegroup',
                      create_resource_group=True,
                      location='eastus2' # or other supported Azure region
                     )
```

La ejecución del código anterior puede desencadenar una nueva ventana del explorador en la que puede iniciar sesión en su cuenta de Azure. Una vez que inicie sesión, el token de autenticación se almacenará en la caché local.

Para ver los detalles del área de trabajo, incluidos el almacenamiento asociado, el registro de contenedor y el almacén de claves, escriba:

```python
ws.get_details()
```

## <a name="write-a-configuration-file"></a>Escritura de un archivo de configuración

Guarde los detalles del área de trabajo en un archivo de configuración del directorio actual. Dicho archivo se llama 'aml_config\config.json'.  

Este archivo de configuración del área de trabajo facilita la carga de esta misma área de trabajo más adelante con otros cuadernos y scripts en el mismo directorio o en un subdirectorio. 

```python
# Create the configuration file.
ws.write_config()

# Use this code to load the workspace from 
# other scripts and notebooks in this directory.
# ws = Workspace.from_config()
```

La llamada API `write_config()` crea el archivo de configuración en el directorio actual. El archivo `config.json` contiene los componentes siguientes:

```json
{
    "subscription_id": "<azure-subscription-id>",
    "resource_group": "myresourcegroup",
    "workspace_name": "myworkspace"
}
```

## <a name="use-the-workspace"></a>Uso del área de trabajo

Escriba código que use las API básicas del SDK para realizar un seguimiento de las ejecuciones del experimento.

```python
from azureml.core import Experiment

# create a new experiemnt
exp = Experiment(workspace=ws, name='myexp')

# start a run
run = exp.start_logging()

# log a number
run.log('my magic number', 42)

# log a list (Fibonacci numbers)
run.log_list('my list', [1, 1, 2, 3, 5, 8, 13, 21, 34, 55]) 

# finish the run
run.complete()
```

## <a name="view-logged-results"></a>Visualización de los resultados registrados
Cuando se completa la ejecución, puede ver la ejecución del experimento en Azure Portal. Utilice el código siguiente para imprimir una dirección URL en los resultados de la última ejecución.

```python
print(run.get_portal_url())
```

Use el vínculo para ver los valores registrados en Azure Portal en el explorador.

![valores registrados en el portal](./media/quickstart-create-workspace-with-python/logged-values.png)

## <a name="clean-up-resources"></a>Limpieza de recursos 
>[!IMPORTANT]
>Los recursos que creó pueden usarse como requisitos previos para otros tutoriales y artículos de procedimientos de Azure Machine Learning.

Si no va a usar lo que ha creado aquí, elimine los recursos que ha creado en esta guía de inicio rápido para no incurrir en gastos.

```python
ws.delete(delete_dependent_resources=True)
```

## <a name="next-steps"></a>Pasos siguientes

Ya ha creado los recursos necesarios para comenzar a experimentar e implementar modelos. También ha ejecutado código en un cuaderno y ha explorado el historial de ejecuciones desde dicho código en el área de trabajo en la nube.

Necesitará algunos paquetes más en su entorno para usarlo con los tutoriales de Azure Machine Learning:

1. En el explorador, cierre el cuaderno.
1. En la ventana de línea de comandos, use `Ctrl`+`C` para detener el servidor del cuaderno.
1. Instale los paquetes adicionales.

    ```sh
    conda install -y cython matplotlib scikit-learn pandas numpy
    pip install azureml-sdk[automl]
    ```

Tras instalar dichos paquetes, siga los tutoriales para entrenar un modelo e implementarlo.  

> [!div class="nextstepaction"]
> [Tutorial: Train an image classification model](tutorial-train-models-with-aml.md) (Tutorial: Entrenamiento de un modelo de clasificación de imágenes)

También puede explorar [ejemplos más avanzados en GitHub](https://aka.ms/aml-notebooks).
