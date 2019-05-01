---
title: Guía de solución de problemas de implementación
titleSuffix: Azure Machine Learning service
description: Aprenda a evitar, solucionar y solucionar los errores comunes de implementación de Docker con AKS y ACI con el servicio Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: chris-lauren
ms.author: clauren
ms.reviewer: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: f81aea22014a2c7d5b37c500a546f0b5350b6435
ms.sourcegitcommit: 2028fc790f1d265dc96cf12d1ee9f1437955ad87
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/30/2019
ms.locfileid: "64925375"
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

Esto resulta útil si está utilizando el `Webservice.deploy` API, o `Webservice.deploy_from_model` API, ya que esas funciones agrupan los pasos mencionados anteriormente en una sola acción. Normalmente, esas API resultan prácticos, pero resulta útil dividir los pasos para solucionar el problema al reemplazarlos con el siguiente llama a la API.

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

### <a name="azure-key-vault-access-policy-and-azure-resource-manager-templates"></a>Directiva de acceso de Azure Key Vault y plantillas de Azure Resource Manager

La compilación de imagen también puede producir un error debido a un problema con la directiva de acceso en Azure Key Vault. Esto puede ocurrir cuando se usa una plantilla de Azure Resource Manager para crear el área de trabajo y recursos asociados (incluido Azure Key Vault), varias veces. Por ejemplo, mediante la plantilla varias veces con los mismos parámetros como parte de una canalización de implementación e integración continua.

La mayoría las operaciones de creación de recursos a través de plantillas son idempotentes, pero Key Vault borra las directivas de acceso cada vez que se usa la plantilla. Esto interrumpe el acceso al almacén de claves para cualquier área de trabajo existente que esté usando. Esto da como resultado errores al intentar crear nuevas imágenes. Los siguientes son ejemplos de los errores que se pueden recibir:

__Portal__:
```text
Create image "myimage": An internal server error occurred. Please try again. If the problem persists, contact support.
```

__SDK__:
```python
image = ContainerImage.create(name = "myimage", models = [model], image_config = image_config, workspace = ws)
Creating image
Traceback (most recent call last):
  File "C:\Python37\lib\site-packages\azureml\core\image\image.py", line 341, in create
    resp.raise_for_status()
  File "C:\Python37\lib\site-packages\requests\models.py", line 940, in raise_for_status
    raise HTTPError(http_error_msg, response=self)
requests.exceptions.HTTPError: 500 Server Error: Internal Server Error for url: https://eastus.modelmanagement.azureml.net/api/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.MachineLearningServices/workspaces/<workspace-name>/images?api-version=2018-11-19

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "C:\Python37\lib\site-packages\azureml\core\image\image.py", line 346, in create
    'Content: {}'.format(resp.status_code, resp.headers, resp.content))
azureml.exceptions._azureml_exception.WebserviceException: Received bad response from Model Management Service:
Response Code: 500
Headers: {'Date': 'Tue, 26 Feb 2019 17:47:53 GMT', 'Content-Type': 'application/json', 'Transfer-Encoding': 'chunked', 'Connection': 'keep-alive', 'api-supported-versions': '2018-03-01-preview, 2018-11-19', 'x-ms-client-request-id': '3cdcf791f1214b9cbac93076ebfb5167', 'x-ms-client-session-id': '', 'Strict-Transport-Security': 'max-age=15724800; includeSubDomains; preload'}
Content: b'{"code":"InternalServerError","statusCode":500,"message":"An internal server error occurred. Please try again. If the problem persists, contact support"}'
```

__CLI__:
```text
ERROR: {'Azure-cli-ml Version': None, 'Error': WebserviceException('Received bad response from Model Management Service:\nResponse Code: 500\nHeaders: {\'Date\': \'Tue, 26 Feb 2019 17:34:05
GMT\', \'Content-Type\': \'application/json\', \'Transfer-Encoding\': \'chunked\', \'Connection\': \'keep-alive\', \'api-supported-versions\': \'2018-03-01-preview, 2018-11-19\', \'x-ms-client-request-id\':
\'bc89430916164412abe3d82acb1d1109\', \'x-ms-client-session-id\': \'\', \'Strict-Transport-Security\': \'max-age=15724800; includeSubDomains; preload\'}\nContent:
b\'{"code":"InternalServerError","statusCode":500,"message":"An internal server error occurred. Please try again. If the problem persists, contact support"}\'',)}
```

Para evitar este problema, se recomienda uno de los enfoques siguientes:

* No implemente la plantilla de más de una vez para los mismos parámetros. O eliminar los recursos existentes antes de usar la plantilla para volver a crearlos.
* Examinar las directivas de acceso de Key Vault y úsela para establecer el `accessPolicies` propiedad de la plantilla.
* Compruebe si ya existe el recurso de Key Vault. Si es así, no volver a crearla con la plantilla. Por ejemplo, agregue un parámetro que le permite deshabilitar la creación del recurso de Key Vault si ya existe.

## <a name="service-launch-fails"></a>Error en el inicio del servicio
Después de la imagen se compila correctamente, el sistema intenta iniciar un contenedor en ACI o AKS, según la configuración de implementación. Se recomienda para probar una implementación de ACI en primer lugar, ya que es una implementación más simple de contenedor único. De este modo, puede descartar cualquier problema específico de AKS.

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

## <a name="http-status-code-503"></a>Código de estado HTTP 503

Las implementaciones de Azure Kubernetes Service admiten el escalado automático, que permite que las réplicas se agregaron para admitir una carga adicional. Sin embargo, el Escalador automático está diseñado para controlar **gradual** cambios en la carga. Si recibe grandes picos en solicitudes por segundo, los clientes pueden recibir un código de estado HTTP 503.

Hay dos cosas que pueden ayudar a evitar 503 códigos de estado:

* Cambiar el nivel de uso en que el escalado automático crea nuevas réplicas.
    
    De forma predeterminada, se establece el uso de destino de escalado automático en el 70%, lo que significa que el servicio puede controlar los picos de solicitudes por segundo (RPS) de hasta un 30%. Puede ajustar el objetivo de uso estableciendo la `autoscale_target_utilization` en un valor inferior.

    > [!IMPORTANT]
    > Este cambio no hace que las réplicas a crearse *con mayor rapidez*. En su lugar, se crean con un umbral de utilización más bajo. En lugar de esperar hasta que el servicio se utiliza de 70%, cambiar el valor a un 30% hace que las réplicas que se creará al 30% de uso se produce.
    
    Si el servicio web ya está usando las réplicas máximo actuales y sigue apareciendo 503 códigos de estado, aumentar la `autoscale_max_replicas` valor para aumentar el número máximo de réplicas.

* Cambiar el número mínimo de réplicas. Aumentar las réplicas mínimas proporciona un grupo más grande para controlar los picos entrantes.

    Para aumentar el número mínimo de réplicas, establecer `autoscale_min_replicas` en un valor superior. Puede calcular las réplicas necesarias mediante el código siguiente, reemplazando los valores por valores específicos al proyecto:

    ```python
    from math import ceil
    # target requests per second
    targetRps = 20
    # time to process the request (in seconds)
    reqTime = 10
    # Maximum requests per container
    maxReqPerContainer = 1
    # target_utilization. 70% in this example
    targetUtilization = .7

    concurrentRequests = targetRps * reqTime / targetUtilization

    # Number of container replicas
    replicas = ceil(concurrentRequests / maxReqPerContainer)
    ```

    > [!NOTE]
    > Si recibe los picos de solicitud más grande que pueden controlar las nuevas réplicas mínimas, puede recibir 503s de nuevo. Por ejemplo, como el tráfico a los aumentos de servicio, es posible que deba aumentar las réplicas mínimas.

Para obtener más información sobre cómo `autoscale_target_utilization`, `autoscale_max_replicas`, y `autoscale_min_replicas` para, consulte el [AksWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py) referencia del módulo.


## <a name="next-steps"></a>Pasos siguientes

Más información acerca de la implementación: 
* [Cómo implementar y dónde](how-to-deploy-and-where.md)

* [Tutorial: Entrenamiento e implementación de modelos](tutorial-train-models-with-aml.md)
