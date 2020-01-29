---
title: Creación de entornos de Azure Machine Learning reutilizables
titleSuffix: Azure Machine Learning
description: Cree y administre entornos para el entrenamiento y la implementación de modelos. Administre paquetes de Python y otros valores para el entorno.
services: machine-learning
author: rastala
ms.author: roastala
ms.reviewer: nibaccam
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 01/06/2020
ms.openlocfilehash: de1e01e56079753a1a9ee8debc2f2172e1dc8375
ms.sourcegitcommit: 87781a4207c25c4831421c7309c03fce5fb5793f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/23/2020
ms.locfileid: "76548312"
---
# <a name="reuse-environments-for-training--deployment-with-azure-machine-learning"></a>Reutilice entornos para entrenamiento e implementación con Azure Machine Learning.
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

En este artículo, aprenderá a crear y administrar [entornos](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py) de Azure Machine Learning para que pueda realizar el seguimiento de las dependencias de software de sus proyectos y reproducirlas a medida que evolucionan.

La administración de dependencias de software es una tarea común para los desarrolladores. Hay que asegurarse de que las compilaciones se puedan reproducir sin mucha configuración manual del software. Teniendo en cuenta soluciones para el desarrollo local, como pip y Conda, la clase Environment de Azure Machine Learning ofrece una solución para el desarrollo en la nube local y distribuido.

En los ejemplos de este artículo se muestran los siguientes procedimientos:

* Creación de un entorno y especificación de dependencias de paquetes
* Recuperación y actualización de entornos
* Uso del entorno para el entrenamiento
* Uso del entorno para la implementación de servicios web

Consulte el [artículo conceptual](concept-environments.md) para obtener información general de alto nivel sobre cómo funcionan los entornos en Azure Machine Learning.

## <a name="prerequisites"></a>Prerequisites

* El SDK de Azure Machine Learning para Python [instalado](https://docs.microsoft.com/python/api/overview/azure/ml/install?view=azure-ml-py).
* Un [área de trabajo de Azure Machine Learning](how-to-manage-workspace.md).

## <a name="create-an-environment"></a>Creación de un entorno

Hay varias maneras de crear un entorno para los experimentos.

### <a name="use-curated-environment"></a>Uso de entornos mantenidos

Puede seleccionar uno de los entornos mantenidos para empezar. 

* El entorno __AzureML-minimal__ contiene un conjunto mínimo de paquetes para habilitar el seguimiento de ejecución y la carga de recursos. Puede utilizarlo como punto inicial para su propio entorno.

* El entorno __AzureML-tutorial__ contiene paquetes de ciencia de datos comunes, como Scikit-Learn, Pandas y Matplotlib, y un conjunto mayor de paquetes de azureml-SDK.

Los entornos mantenidos están respaldados por imágenes de Docker en caché, lo que reduce el costo de preparación de la ejecución.

Use el método __Environment.get__ para seleccionar uno de los entornos mantenidos:

```python
from azureml.core import Workspace, Environment

ws = Workspace.from_config()
env = Environment.get(workspace=ws, name="AzureML-Minimal")
```

Puede enumerar los entornos mantenidos y sus paquetes mediante el código siguiente:
```python
envs = Environment.list(workspace=ws)

for env in envs:
    if env.startswith("AzureML"):
        print("Name",env)
        print("packages", envs[env].python.conda_dependencies.serialize_to_string())
```

> [!WARNING]
>  No inicie el nombre de su propio entorno con el prefijo _AzureML_. Está reservado para entornos mantenidos.

### <a name="instantiate-an-environment-object"></a>Creación de instancias de un objeto de entorno

Para crear manualmente un entorno, importe la clase Environment desde el SDK y cree una instancia de un objeto de entorno con el código siguiente.

```python
from azureml.core.environment import Environment
Environment(name="myenv")
```

### <a name="conda-and-pip-specification-files"></a>Archivos de especificación de Conda y pip

También puede crear un entorno a partir de una especificación de Conda o de un archivo de requisitos de pip.
Use el método [from_conda_specification()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-conda-specification-name--file-path-) o [from_pip_requirements()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-pip-requirements-name--file-path-) e incluya el nombre del entorno y la ruta de acceso del archivo deseado en el argumento del método.

```python
# From a Conda specification file
myenv = Environment.from_conda_specification(name = "myenv",
                                             file_path = "path-to-conda-specification-file")

#From a pip requirements file
myenv = Environment.from_pip_requirements(name = "myenv"
                                          file_path = "path-to-pip-requirements-file")
```

### <a name="existing-conda-environment"></a>Entorno de Conda existente

Si tiene un entorno de Conda en el equipo local, el servicio ofrece una solución para crear un objeto de entorno a partir de él. De este modo, puede volver a usar el entorno interactivo local en ejecuciones remotas.

El código siguiente crea un objeto de entorno fuera del entorno de Conda existente `mycondaenv` con el método [from_existing_conda_environment()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#from-existing-conda-environment-name--conda-environment-name-).

``` python
myenv = Environment.from_existing_conda_environment(name = "myenv",
                                                    conda_environment_name = "mycondaenv")
```

### <a name="automatically-create-environments"></a>Creación automática de entornos

Cree un entorno automáticamente mediante el envío de una ejecución de entrenamiento con el método submit(). Cuando se envía una ejecución de entrenamiento, la creación de un entorno puede tardar varios minutos, según el tamaño de las dependencias necesarias. 

Si no especifica un entorno en la configuración de ejecución antes de enviar la ejecución, se crea un entorno predeterminado.

```python
from azureml.core import ScriptRunConfig, Experiment, Environment
# Create experiment 
myexp = Experiment(workspace=ws, name = "environment-example")

# Attaches training script and compute target to run config
runconfig = ScriptRunConfig(source_directory=".", script="example.py")
runconfig.run_config.target = "local"

# Submit the run
run = myexp.submit(config=runconfig)

# Shows each step of run 
run.wait_for_completion(show_output=True)
```

Del mismo modo, si usa un objeto [`Estimator`](https://docs.microsoft.com//python/api/azureml-train-core/azureml.train.estimator.estimator?view=azure-ml-py) para el entrenamiento, puede enviar la instancia del estimador directamente como una ejecución sin tener que especificar un entorno. El objeto `Estimator` ya encapsula el entorno y el destino de proceso.


## <a name="add-packages-to-an-environment"></a>Adición de paquetes a un entorno

Agregue paquetes a un entorno con los archivos de Conda, pip o Private Wheel. Especifique cada dependencia de paquete mediante la [clase CondaDependency](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py) y agréguela a la parte PythonSection del entorno.

### <a name="conda-and-pip-packages"></a>Paquetes Conda y pip

Si un paquete está disponible en un repositorio de paquetes de Conda, se recomienda usar la instalación de Conda sobre pip. La razón es que los paquetes de Conda suelen incluir archivos binarios pregenerados que hacen que la instalación sea más confiable.

En el ejemplo siguiente se agrega `scikit-learn`, en concreto la versión 0.21.3, y el paquete `pillow` al entorno, `myenv` con los métodos [`add_conda_package()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py#add-conda-package-conda-package-) y [`add_pip_package()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.conda_dependencies.condadependencies?view=azure-ml-py#add-pip-package-pip-package-), respectivamente.

```python
from azureml.core.environment import Environment
from azureml.core.conda_dependencies import CondaDependencies

myenv = Environment(name="myenv")
conda_dep = CondaDependencies()

# Installs scikit-learn version 0.21.3 conda package
conda_dep.add_conda_package("scikit-learn==0.21.3")

# Adds dependencies to PythonSection of myenv
myenv.python.conda_dependencies=conda_dep
```

### <a name="private-wheel-files"></a>Archivos de Private Wheel

Para usar archivos pip de Private Wheel, primero cárguelos en el almacenamiento del área de trabajo mediante el método estático [`add_private_pip_wheel()`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.environment?view=azure-ml-py#add-private-pip-wheel-workspace--file-path--exist-ok-false-), capture luego la dirección URL de almacenamiento y pásela al método `add_pip_package()`.

```python
# During environment creation the service replaces the URL by secure SAS URL, so your wheel file is kept private and secure
whl_url = Environment.add_private_pip_wheel(workspace=ws,file_path = "my-custom.whl")
myenv = Environment(name="myenv")
conda_dep = CondaDependencies()
conda_dep.add_pip_package(whl_url)
myenv.python.conda_dependencies=conda_dep
```

## <a name="manage-environments"></a>Administración de entornos

Administre los entornos para que pueda actualizarlos, realizar su seguimiento y reutilizarlos en todos los destinos de proceso y con otros usuarios del área de trabajo.

### <a name="register-environments"></a>Registro de entornos

El entorno se registra automáticamente en el área de trabajo cuando se envía una ejecución o se implementa un servicio web. También puede registrar manualmente el entorno mediante el método [register()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#register-workspace-). Esta operación convierte el entorno en una entidad con control de versiones y de la que se realiza el seguimiento, y que se puede compartir entre los usuarios del área de trabajo.

El código siguiente registra el entorno `myenv` en el área de trabajo `ws`.

```python
myenv.register(workspace=ws)
```

Cuando se usa por primera vez el entorno, en el entrenamiento o la implementación, se registra en el área de trabajo, se compila y se implementa en el destino de proceso. Los entornos se almacenan en caché en el servicio. La reutilización de un entorno almacenado en caché tarda mucho menos tiempo que el uso de un nuevo servicio o uno que se haya actualizado.

### <a name="get-existing-environments"></a>Obtención de los entornos existentes

La clase Environment ofrece métodos que permiten recuperar los entornos existentes en el área de trabajo por nombre, en forma de lista o mediante una ejecución de entrenamiento específica con fines de solución de problemas o auditoría y de reproducibilidad.

#### <a name="view-list-of-environments"></a>Visualización de la lista de entornos

Vea los entornos del área de trabajo con [`Environment.list(workspace="workspace_name")`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#list-workspace-) y seleccione uno para reutilizarlo.

#### <a name="get-environment-by-name"></a>Obtención del entorno por nombre

También puede obtener un entorno específico por nombre y versión.
El código siguiente usa el método [get()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#get-workspace--name--version-none-) para recuperar la versión `1` del entorno `myenv` del área de trabajo `ws`.

```python
restored_environment = Environment.get(workspace=ws,name="myenv",version="1")
```

#### <a name="training-run-specific-environment"></a>Entorno específico de la ejecución de entrenamiento

Para obtener el entorno usado en una ejecución concreta después de que se complete el entrenamiento, use el método [get_environment()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run.run?view=azure-ml-py#get-environment--) en la clase Run.

```python
from azureml.core import Run
Run.get_environment()
```

### <a name="update-an-existing-environment"></a>Actualización de un entorno existente

Si realiza cambios en un entorno existente, como agregar un paquete de Python, se crea una nueva versión de un entorno cuando se envía la ejecución, se implementa el modelo o se registra manualmente el entorno. El control de versiones le permite ver los cambios en el entorno a lo largo del tiempo.

Para actualizar una versión de paquete de Python de un entorno existente, especifique el número de versión exacto de ese paquete. De lo contrario, Azure Machine Learning volverá a usar el entorno existente con las versiones de paquete de cuando se creó el entorno.

### <a name="debug-the-image-build"></a>Depuración de la compilación de la imagen

En este ejemplo se usa el método [build()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py#build-workspace-) para crear manualmente un entorno como una imagen de Docker y se supervisan los registros de salida de la compilación de la imagen mediante [wait_for_completion()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image(class)?view=azure-ml-py#wait-for-creation-show-output-false-). La imagen compilada aparece entonces en la instancia de Azure Container Registry del área de trabajo, lo que resulta útil para la depuración.

```python
from azureml.core import Image
build = env.build(workspace=ws)
build.wait_for_completion(show_output=True)
```

## <a name="docker-and-environments"></a>Docker y entornos

 [DockerSection](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment.dockersection?view=azure-ml-py) de la clase `Environments` de Azure Machine Learning le permite personalizar y controlar en detalle el sistema operativo invitado en el que se ejecuta la ejecución de entrenamiento.

Cuando se usa el comando `enable` para habilitar Docker, el servicio compila una imagen de Docker y crea un entorno de Python con las especificaciones dentro de ese contenedor de Docker. Este entorno ofrece aislamiento y reproducibilidad adicionales para las ejecuciones de entrenamiento.

```python
# Creates the environment inside a Docker container.
myenv.docker.enabled = True
```

Una vez creada, la imagen de Docker aparece en la instancia de Azure Container Registry que está asociada de forma predeterminada al área de trabajo.  El nombre del repositorio tiene el formato *azureml/azureml_\<uuid\>* . La parte del identificador único (*uuid*) corresponde a un hash calculado a partir de la configuración del entorno. Esto permite que el servicio determine si ya existe una imagen correspondiente al entorno dado para reutilizar.

Además, el servicio usa automáticamente una de las [imágenes base](https://github.com/Azure/AzureML-Containers) basadas en Ubuntu Linux e instala los paquetes de Python especificados. La imagen base tiene versiones de CPU y GPU. Azure Machine Learning detecta automáticamente qué versión se va a usar.

```python
# Specify custom Docker base image and registry, if you don't want to use the defaults
myenv.docker.base_image="your_base-image"
myenv.docker.base_image_registry="your_registry_location"
# Alternatively, you can specify the contents of dockerfile of your base image
with open("docker_file_of_your_base_image", "r") as f:
    dockerfile_contents_of_your_base_image=f.read()
myenv.docker.base_dockerfile=dockerfile_contents_of_your_base_image 
```

> [!NOTE]
> Si especifica `environment.python.user_managed_dependencies=False` al usar una imagen de Docker personalizada, el servicio creará un entorno de Conda dentro de la imagen y ejecutará la ejecución en ese entorno, en lugar de usar las bibliotecas de Python que puede haber instalado en la imagen base. Establezca el parámetro en `True` para usar sus propios paquetes instalados.

## <a name="using-environments-for-training"></a>Uso de entornos para el entrenamiento

Para enviar una ejecución de entrenamiento, debe combinar el entorno, el [destino de proceso](concept-compute-target.md) y el script de Python de entrenamiento en una configuración de ejecución; un objeto contenedor que se usa para el envío de ejecuciones.

Cuando se envía una ejecución de entrenamiento, la creación de un nuevo entorno puede tardar varios minutos, según el tamaño de las dependencias necesarias. El servicio almacena en caché los entornos, por lo tanto, siempre y cuando la definición del entorno permanezca sin cambios, solo se incurrirá una vez en el tiempo de configuración completo.

A continuación se proporciona un ejemplo de ejecución de script local en el que se usaría [ScriptRunConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.script_run_config.scriptrunconfig?view=azure-ml-py) como objeto contenedor.

```python
from azureml.core import ScriptRunConfig, Experiment
from azureml.core.environment import Environment

exp = Experiment(name="myexp", workspace = ws)
# Instantiate environment
myenv = Environment(name="myenv")

# Add training script to run config
runconfig = ScriptRunConfig(source_directory=".", script="train.py")

# Attach compute target to run config
runconfig.run_config.target = "local"

# Attach environment to run config
runconfig.run_config.environment = myenv

# Submit run 
run = exp.submit(runconfig)
```

> [!NOTE]
> Para deshabilitar el historial de ejecución o ejecutar instantáneas, use el valor de `ScriptRunConfig.run_config.history`.

Si no especifica el entorno en la configuración de ejecución, el servicio creará un entorno predeterminado cuando envíe la ejecución.

### <a name="train-with-an-estimator"></a>Entrenamiento con un estimador

Si usa un [estimador](how-to-train-ml-models.md) para el entrenamiento, puede enviar simplemente la instancia del estimador directamente, puesto que ya encapsula el entorno y el destino de proceso.

El siguiente código usa un estimador para una ejecución de entrenamiento de un solo nodo en un proceso remoto para un modelo scikit-learn; se supone que hay un objeto de destino de proceso creado previamente, `compute_target`, y un objeto de almacén de datos, `ds`.

```python
from azureml.train.estimator import Estimator

script_params = {
    '--data-folder': ds.as_mount(),
    '--regularization': 0.8
}

sk_est = Estimator(source_directory='./my-sklearn-proj',
                   script_params=script_params,
                   compute_target=compute_target,
                   entry_script='train.py',
                   conda_packages=['scikit-learn'])

# Submit the run 
run = experiment.submit(sk_est)
```

## <a name="using-environments-for-web-service-deployment"></a>Uso de entornos para la implementación de servicios web

Puede usar entornos al implementar el modelo como un servicio web. Esto permite un flujo de trabajo reproducible y conectado en el que puede entrenar, probar e implementar el modelo exactamente mediante las mismas bibliotecas en los procesos de entrenamiento e inferencia.

Para implementar un servicio web, combine el entorno, el proceso de inferencia, el script de puntuación y el modelo registrado en el objeto de implementación, [deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#deploy-workspace--name--models--inference-config-none--deployment-config-none--deployment-target-none--overwrite-false-). Más información sobre cómo [implementar servicios web](how-to-deploy-and-where.md).

En este ejemplo, suponga que ha completado una ejecución de entrenamiento y quiere implementar ese modelo en Azure Container Instance (ACI). Al compilar el servicio web, los archivos de modelo y puntuación se montan en la imagen y la pila de inferencia de Azure Machine Learning se agrega a la imagen.

```python
from azureml.core.model import InferenceConfig, Model
from azureml.core.webservice import AciWebservice, Webservice

# Register the model to deploy
model = run.register_model(model_name = "mymodel", model_path = "outputs/model.pkl")

# Combine scoring script & environment in Inference configuration
inference_config = InferenceConfig(entry_script="score.py", environment=myenv)

# Set deployment configuration
deployment_config = AciWebservice.deploy_configuration(cpu_cores = 1, memory_gb = 1)

# Define the model, inference & deployment configuration and web service name and location to deploy
service = Model.deploy(
    workspace = ws,
    name = "my_web_service",
    models = [model],
    inference_config = inference_config,
    deployment_config = deployment_config)
```

## <a name="example-notebooks"></a>Cuadernos de ejemplo

Este [cuaderno de ejemplo](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/training/using-environments) amplía los conceptos y métodos que se muestran en este artículo.

En [Implementación de un modelo mediante una imagen base personalizada de Docker](how-to-deploy-custom-docker-image.md), se muestra cómo implementar un modelo mediante una imagen base personalizada de Docker.

En este [cuaderno de ejemplo](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/deployment/spark) se muestra cómo implementar un modelo de Spark como un servicio web.

## <a name="create-and-manage-environments-with-the-cli"></a>Creación y administración de entornos con la CLI

La [CLI de Azure Machine Learning](reference-azure-machine-learning-cli.md) refleja la mayor parte de la funcionalidad del SDK de Python y se puede usar para la creación y la administración de entornos. Los siguientes comandos muestran la funcionalidad básica.

El siguiente comando aplica la técnica scaffolding a los archivos para una definición de entorno predeterminada en el directorio especificado. Estos archivos son archivos JSON que son similares en función a la clase correspondiente en el SDK y se pueden usar para crear nuevos entornos con una configuración personalizada. 

```azurecli-interactive
az ml environment scaffold -n myenv -d myenvdir
```

Ejecute el siguiente comando para registrar un entorno desde un directorio especificado.

```azurecli-interactive
az ml environment register -d myenvdir
```

Al ejecutar el comando siguiente, se enumerarán todos los entornos registrados.

```azurecli-interactive
az ml environment list
```

Descargue un entorno registrado con el siguiente comando.

```azurecli-interactive
az ml environment download -n myenv -d downloaddir
```

## <a name="next-steps"></a>Pasos siguientes

* [Tutorial: Entrenamiento de un modelo](tutorial-train-models-with-aml.md) utiliza un destino de proceso administrado para entrenar un modelo.
* Cuando tenga un modelo entrenado, aprenda [cómo y dónde implementar los modelos](how-to-deploy-and-where.md).
* Consulte la referencia del SDK de la [clase Environment](https://docs.microsoft.com/python/api/azureml-core/azureml.core.environment(class)?view=azure-ml-py).
