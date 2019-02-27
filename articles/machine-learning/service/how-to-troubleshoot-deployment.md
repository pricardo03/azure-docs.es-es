---
title: Guía de solución de problemas de implementación
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo abordar, solucionar y resolver los errores comunes de implementación de Docker con AKS y ACI con la instancia de Azure Machine Learning Service
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: chris-lauren
ms.author: clauren
ms.reviewer: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 112fff011ebfedc1abf6981661da5fd4d97fc3d0
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/14/2019
ms.locfileid: "56267156"
---
# <a name="troubleshooting-azure-machine-learning-service-aks-and-aci-deployments"></a>Solución de problemas de implementaciones de AKS y ACI de Azure Machine Learning Service

En este artículo, aprenderá cómo abordar, solucionar y resolver los errores comunes de implementación de Docker con Azure Container Instances (ACI) y Azure Kubernetes Service (AKS) con Azure Machine Learning Service.

Al implementar un modelo en el servicio de Azure Machine Learning, el sistema realiza una serie de tareas. Se trata de una secuencia de eventos compleja y, a veces, surgen problemas. Las tareas de implementación son:

1. Registrar el modelo en el registro de modelos del área de trabajo.

2. Compilar una imagen de Docker, incluido:
    1. Descargue el modelo registrado desde el registro. 
    2. Cree un archivo dockerfile, con un entorno de Python basado en las dependencias que especifique en el archivo yaml del entorno.
    3. Agregue los archivos del modelo y el script de puntuación que proporciona en el archivo dockerfile.
    4. Cree una nueva imagen de Docker con el archivo dockerfile.
    5. Registre la imagen de Docker en la instancia de Azure Container Registry asociada con el área de trabajo.

3. Implementar la imagen de Docker en el servicio de instancia de contenedor de Azure (ACI) o Azure Kubernetes Service (AKS).

4. Iniciar un nuevo contenedor (o contenedores) en ACI o AKS. 

Más información sobre este proceso en la introducción a la [administración de modelos](concept-model-management-and-deployment.md).

## <a name="before-you-begin"></a>Antes de empezar

Si tiene algún problema, lo primero es dividir la tarea de implementación (descrita anteriormente) en pasos individuales para aislar el problema. 

Esto es especialmente útil si usa la API `Webservice.deploy` o la API `Webservice.deploy_from_model`, ya que esas funciones agrupan los pasos mencionados anteriormente en una sola acción. Normalmente estas API son bastante prácticas, pero resulta útil dividir los pasos para solucionar problemas al reemplazarlas por las siguientes llamadas API.

1. Registre el modelo. Este es un código de ejemplo:

    ```python
    # register a model out of a run record
    model = best_run.register_model(model_name='my_best_model', model_path='outputs/my_model.pkl')

    # or, you can register a file or a folder of files as a model
    model = Model.register(model_path='my_model.pkl', model_name='my_best_model', workspace=ws)
    ```

2. Compile la imagen. Este es un código de ejemplo:

    ```python
    # configure the image
    image_config = ContainerImage.image_configuration(runtime="python",
                                                      execution_script="score.py",
                                                      conda_file="myenv.yml")

    # create the image
    image = Image.create(name='myimg', models=[model], image_config=image_config, workspace=ws)

    # wait for image creation to finish
    image.wait_for_creation(show_output=True)
    ```

3. Implemente la imagen como servicio. Este es un código de ejemplo:

    ```python
    # configure an ACI-based deployment
    aci_config = AciWebservice.deploy_configuration(cpu_cores=1, memory_gb=1)

    aci_service = Webservice.deploy_from_image(deployment_config=aci_config, 
                                               image=image, 
                                               name='mysvc', 
                                               workspace=ws)
    aci_service.wait_for_deployment(show_output=True)    
    ```

Una vez que haya dividido el proceso de implementación en tareas individuales, se pueden observar algunos de los errores más comunes.

## <a name="image-building-fails"></a>Error de compilación de la imagen
Si el sistema no puede compilar la imagen de Docker, la llamada `image.wait_for_creation()` produce un error con algunos mensajes que pueden ofrecer algunas pistas. También puede encontrar más detalles acerca de los errores en el registro de compilación de la imagen. A continuación encontrará algunos ejemplos de código que muestra cómo detectar el URI del registro de compilación de la imagen.

```python
# if you already have the image object handy
print(image.image_build_log_uri)

# if you only know the name of the image (note there might be multiple images with the same name but different version number)
print(ws.images['myimg'].image_build_log_uri)

# list logs for all images in the workspace
for name, img in ws.images.items():
    print (img.name, img.version, img.image_build_log_uri)
```
El URI del registro de la imagen es una dirección URL de SAS que apunta a un archivo de registro almacenado en Azure Blob Storage. Simplemente copie el URI y péguelo en una ventana del explorador, y podrá descargar y ver el archivo de registro.


## <a name="service-launch-fails"></a>Error en el inicio del servicio
Después de la imagen se compila correctamente, el sistema intenta iniciar un contenedor en ACI o AKS, según la configuración de implementación. Por lo general, se recomienda probar una implementación de ACI en primer lugar, ya que es una implementación más simple de contenedor único. De este modo, puede descartar cualquier problema específico de AKS.

Como parte del proceso de puesta en marcha del contenedor, el sistema invoca la función `init()` en el script de puntuación. Si hay excepciones no detectadas en la función `init()`, es posible que vea el error **CrashLoopBackOff** en el mensaje de error. A continuación, se muestran algunas sugerencias para ayudarle a solucionar el problema.

### <a name="inspect-the-docker-log"></a>Inspección del registro de Docker
Puede imprimir los mensajes detallados de registro del motor de Docker desde el objeto de servicio.

```python
# if you already have the service object handy
print(service.get_logs())

# if you only know the name of the service (note there might be multiple services with the same name but different version number)
print(ws.webservices['mysvc'].get_logs())
```

### <a name="debug-the-docker-image-locally"></a>Depuración local de la imagen de Docker
En ocasiones, el registro de Docker no emite información suficiente sobre la causa del error. Puede ir un paso más allá y extraer la imagen de Docker compilada, iniciar un contenedor local y depurar directamente dentro del contenedor en vivo de manera interactiva. Para iniciar un contenedor local, debe tener un motor de Docker que se ejecute localmente, y sería mucho más fácil si también tiene [azure-cli](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) instalada.

Primero debe averiguar la ubicación de la imagen:

```python
# print image location
print(image.image_location)
```

La ubicación de la imagen tiene este formato: `<acr-name>.azurecr.io/<image-name>:<version-number>`, por ejemplo, `myworkpaceacr.azurecr.io/myimage:3`. 

Ahora, vaya a la ventana de línea de comandos. Si tiene instalada azure-cli, puede escribir los comandos siguientes para iniciar sesión en el ACR (Azure Container Registry) asociado con el área de trabajo donde se almacena la imagen. 

```sh
# log on to Azure first if you haven't done so before
$ az login

# make sure you set the right subscription in case you have access to multiple subscriptions
$ az account set -s <subscription_name_or_id>

# now let's log in to the workspace ACR
# note the acr-name is the domain name WITHOUT the ".azurecr.io" postfix
# e.g.: az acr login -n myworkpaceacr
$ az acr login -n <acr-name>
```
Si no tiene instalada azure-cli, puede usar el comando `docker login` para iniciar sesión en ACR. Pero antes deberá recuperar el nombre de usuario y la contraseña de ACR desde Azure Portal.

Una vez que haya iniciado sesión en ACR, puede extraer la imagen de Docker e iniciar un contenedor localmente y, luego, iniciar una sesión de bash para la depuración mediante el comando `docker run`:

```sh
# note the image_id is <acr-name>.azurecr.io/<image-name>:<version-number>
# for example: myworkpaceacr.azurecr.io/myimage:3
$ docker run -it <image_id> /bin/bash
```

Una vez que inicie una sesión de bash para el contenedor en ejecución, puede encontrar los scripts de puntuación en la carpeta `/var/azureml-app`. A continuación, puede iniciar una sesión de Python para depurar los scripts de puntuación. 

```sh
# enter the directory where scoring scripts live
cd /var/azureml-app

# find what Python packages are installed in the python environment
pip freeze

# sanity-check on score.py
# you might want to edit the score.py to trigger init().
# as most of the errors happen in init() when you are trying to load the model.
python score.py
```
Si necesita un editor de texto para modificar los scripts, puede instalar vim, nano, Emacs o su editor favorito.

```sh
# update package index
apt-get update

# install a text editor of your choice
apt-get install vim
apt-get install nano
apt-get install emacs

# launch emacs (for example) to edit score.py
emacs score.py

# exit the container bash shell
exit
```

También puede iniciar el servicio web localmente y enviar tráfico HTTP al servicio. El servidor de Flask en el contenedor de Docker se ejecuta en el puerto 5001. Puede asignarlo a otros puertos disponibles en el equipo host.
```sh
# you can find the scoring API at: http://localhost:8000/score
$ docker run -p 8000:5001 <image_id>
```

## <a name="function-fails-getmodelpath"></a>Error en la función: get_model_path()
A menudo, en la función `init()` en el script de puntuación, se llama a la función `Model.get_model_path()` para buscar un archivo de modelo o una carpeta de archivos de modelo en el contenedor. Este suele ser un origen de error si no se encuentra el archivo de modelo o la carpeta. La manera más fácil de depurar este error es ejecutar el siguiente código de Python en el shell del contenedor:

```python
import logging
logging.basicConfig(level=logging.DEBUG)
from azureml.core.model import Model
print(Model.get_model_path(model_name='my-best-model'))
```

Esto proporciona la ruta de acceso local (relativa a `/var/azureml-app`) en el contenedor donde el script de puntuación espera encontrar el archivo de modelo o la carpeta. A continuación, puede comprobar si el archivo o la carpeta están realmente donde se espera que estén.

Establecer el nivel de registro en DEBUG puede provocar el registro de información adicional, que puede resultar útil para identificar el error.

## <a name="function-fails-runinputdata"></a>Error en la función: run(input_data)
Si el servicio se implementa correctamente, pero se bloquea al publicar datos en el punto de conexión de puntuación, puede agregar una instrucción de captura de errores en la función `run(input_data)` para que devuelva un mensaje de error detallado en su lugar. Por ejemplo: 

```python
def run(input_data):
    try:
        data = json.loads(input_data)['data']
        data = np.array(data)
        result = model.predict(data)
        return json.dumps({"result": result.tolist()})
    except Exception as e:
        result = str(e)
        # return error message back to the client
        return json.dumps({"error": result})
```
**Nota**: La devolución de mensajes de error de la llamada `run(input_data)` se debe realizar solo con fines de depuración. Por motivos de seguridad, no sería recomendable hacer esto en un entorno de producción.


## <a name="next-steps"></a>Pasos siguientes

Más información acerca de la implementación: 
* [Cómo implementar y dónde](how-to-deploy-and-where.md)

* [Tutorial: Entrenamiento e implementación de modelos](tutorial-train-models-with-aml.md)
