---
title: Guía de solución de problemas de implementación
titleSuffix: Azure Machine Learning
description: Obtenga información sobre cómo abordar, solucionar y resolver los errores comunes de implementación de Docker con Azure Kubernetes Service y Azure Container Instances mediante Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: chris-lauren
ms.author: clauren
ms.reviewer: jmartens
ms.date: 07/09/2019
ms.custom: seodec18
ms.openlocfilehash: 08b9434dbcca96ff57e2c8182693023a5eb2eea9
ms.sourcegitcommit: 1752581945226a748b3c7141bffeb1c0616ad720
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/14/2019
ms.locfileid: "70997171"
---
# <a name="troubleshooting-azure-machine-learning-azure-kubernetes-service-and-azure-container-instances-deployment"></a>Solución de problemas con la implementación de Azure Machine Learning, Azure Kubernetes Service y Azure Container Instances

Obtenga información sobre cómo abordar, solucionar y resolver los errores comunes de implementación de Docker con Azure Container Instances (ACI) y Azure Kubernetes Service (AKS) mediante Azure Machine Learning.

Al implementar un modelo en Azure Machine Learning, el sistema realiza una serie de tareas. Las tareas de implementación son:

1. Registrar el modelo en el registro de modelos del área de trabajo.

2. Compilar una imagen de Docker, incluido:
    1. Descargue el modelo registrado desde el registro. 
    2. Cree un archivo dockerfile, con un entorno de Python basado en las dependencias que especifique en el archivo yaml del entorno.
    3. Agregue los archivos del modelo y el script de puntuación que proporciona en el archivo dockerfile.
    4. Cree una nueva imagen de Docker con el archivo dockerfile.
    5. Registre la imagen de Docker en la instancia de Azure Container Registry asociada con el área de trabajo.

    > [!IMPORTANT]
    > En función del código, la creación de imágenes se realiza automáticamente sin que sea necesaria la entrada.

3. Implementar la imagen de Docker en el servicio de instancia de contenedor de Azure (ACI) o Azure Kubernetes Service (AKS).

4. Iniciar un nuevo contenedor (o contenedores) en ACI o AKS. 

Más información sobre este proceso en la introducción a la [administración de modelos](concept-model-management-and-deployment.md).

## <a name="before-you-begin"></a>Antes de empezar

Si tiene algún problema, lo primero es dividir la tarea de implementación (descrita anteriormente) en pasos individuales para aislar el problema.

Dividir la implementación en tareas es útil si se utilizan las API [Webservice.deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#deploy-workspace--name--model-paths--image-config--deployment-config-none--deployment-target-none-) o [Webservice.deploy_from_model()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#deploy-from-model-workspace--name--models--image-config--deployment-config-none--deployment-target-none-), ya que ambas funciones realizan los pasos mencionados anteriormente como una acción única. Normalmente estas API son prácticas, pero resulta útil dividir los pasos para solucionar problemas al reemplazarlas por las siguientes llamadas API.

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
                                                      entry_script="score.py",
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

Si no se puede compilar la imagen de Docker, no se podrá realizar la llamada a [image.wait_for_creation()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.image(class)?view=azure-ml-py#wait-for-creation-show-output-false-) o [service.wait_for_deployment()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#wait-for-deployment-show-output-false-) con algunos mensajes de error que pueden dar algunas pistas. También puede encontrar más detalles acerca de los errores en el registro de compilación de la imagen. A continuación encontrará algunos ejemplos de código que muestra cómo detectar el URI del registro de compilación de la imagen.

```python
# if you already have the image object handy
print(image.image_build_log_uri)

# if you only know the name of the image (note there might be multiple images with the same name but different version number)
print(ws.images['myimg'].image_build_log_uri)

# list logs for all images in the workspace
for name, img in ws.images.items():
    print(img.name, img.version, img.image_build_log_uri)
```

El URI del registro de la imagen es una dirección URL de SAS que apunta a un archivo de registro almacenado en Azure Blob Storage. Simplemente copie el URI y péguelo en una ventana del explorador, y podrá descargar y ver el archivo de registro.

### <a name="azure-key-vault-access-policy-and-azure-resource-manager-templates"></a>Directiva de acceso de Azure Key Vault y plantillas de Azure Resource Manager

La compilación de la imagen también puede producir un error debido a un problema con la directiva de acceso en Azure Key Vault. Esta situación puede suceder cuando se usa una plantilla de Azure Resource Manager para crear el área de trabajo y los recursos asociados (incluido Azure Key Vault) varias veces. Por ejemplo, con el uso de la plantilla varias veces con los mismos parámetros como parte de una canalización de implementación e integración continuas.

La mayoría de las operaciones de creación de recursos mediante plantillas son idempotentes, pero Key Vault borra las directivas de acceso cada vez que se usa la plantilla. Al borrar las directivas de acceso se interrumpe el acceso al almacén de claves en el área de trabajo existente que lo esté usando. Esta condición produce errores cuando intenta crear nuevas imágenes. Los siguientes son ejemplos de errores que puede recibir:

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

Para evitar este problema, se recomienda uno de los siguientes enfoques:

* No implemente la plantilla de más de una vez con los mismos parámetros. O bien, elimine los recursos existentes antes de usar la plantilla para volver a crearlos.
* Examine las directivas de acceso de Key Vault y, luego, use estas directivas para establecer la propiedad `accessPolicies` de la plantilla.
* Compruebe si ya existe el recurso de Key Vault. Si es así, no lo vuelva a crear con la plantilla. Por ejemplo, agregue un parámetro que permita deshabilitar la creación del recurso de Key Vault si ya existe.

## <a name="debug-locally"></a>Depuración local

Si tiene problemas al implementar un modelo en ACI o AKS, intente implementarlo como un servicio web local. El uso de un servicio web local facilita la solución de problemas. Se descarga la imagen de Docker que contiene el modelo y se inicia en el sistema local.

> [!IMPORTANT]
> Las implementaciones de servicios web locales requieren una instalación de Docker en funcionamiento en el sistema local. Docker se debe ejecutar antes de implementar un servicio web local. Para obtener información sobre cómo instalar y usar Docker, vea [https://www.docker.com/](https://www.docker.com/).

> [!WARNING]
> No se admiten las implementaciones de servicios web locales en escenarios de producción.

Para implementar de forma local, modifique el código para usar `LocalWebservice.deploy_configuration()` con el fin de crear una configuración de implementación. Luego use `Model.deploy()` para implementar el servicio. En el ejemplo siguiente se implementa un modelo (incluido en la variable `model`) como un servicio web local:

```python
from azureml.core.model import InferenceConfig, Model
from azureml.core.webservice import LocalWebservice

# Create inference configuration. This creates a docker image that contains the model.
inference_config = InferenceConfig(runtime="python",
                                   entry_script="score.py",
                                   conda_file="myenv.yml")

# Create a local deployment, using port 8890 for the web service endpoint
deployment_config = LocalWebservice.deploy_configuration(port=8890)
# Deploy the service
service = Model.deploy(
    ws, "mymodel", [model], inference_config, deployment_config)
# Wait for the deployment to complete
service.wait_for_deployment(True)
# Display the port that the web service is available on
print(service.port)
```

En este punto, se puede trabajar con el servicio de forma habitual. Por ejemplo, en el código siguiente se muestra cómo enviar datos al servicio:

```python
import json

test_sample = json.dumps({'data': [
    [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
    [10, 9, 8, 7, 6, 5, 4, 3, 2, 1]
]})

test_sample = bytes(test_sample, encoding='utf8')

prediction = service.run(input_data=test_sample)
print(prediction)
```

### <a name="update-the-service"></a>Actualizar el servicio

Durante las pruebas locales, es posible que deba actualizar el archivo `score.py` para agregar un registro o intentar resolver los problemas que haya descubierto. Para recargar los cambios realizados en el archivo `score.py`, use `reload()`. Por ejemplo, el código siguiente recarga el script para el servicio y luego envía datos. Los datos se puntúan con el archivo `score.py` actualizado:

> [!IMPORTANT]
> El método `reload` solo está disponible para las implementaciones locales. Para obtener información sobre cómo actualizar una implementación en otro destino de proceso, consulte la sección de actualización de [Implementación de modelos](how-to-deploy-and-where.md#update).

```python
service.reload()
print(service.run(input_data=test_sample))
```

> [!NOTE]
> El script se recarga desde la ubicación que especifica el objeto `InferenceConfig` que usa el servicio.

Para cambiar el modelo, las dependencias de Conda o la configuración de implementación, use [update()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#update--args-). En el siguiente ejemplo se actualiza el modelo que usa el servicio:

```python
service.update([different_model], inference_config, deployment_config)
```

### <a name="delete-the-service"></a>Eliminación del servicio

Para eliminar el servicio, use [delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#delete--).

### <a id="dockerlog"></a> Inspección del registro de Docker

Puede imprimir los mensajes detallados de registro del motor de Docker desde el objeto de servicio. Puede ver el registro para ACI, AKS y las implementaciones locales. En el ejemplo siguiente se muestra cómo imprimir los registros.

```python
# if you already have the service object handy
print(service.get_logs())

# if you only know the name of the service (note there might be multiple services with the same name but different version number)
print(ws.webservices['mysvc'].get_logs())
```

## <a name="service-launch-fails"></a>Error en el inicio del servicio

Después de que la imagen se haya compilado correctamente, el sistema intenta iniciar un contenedor con el uso de la configuración de implementación. Como parte del proceso de puesta en marcha del contenedor, el sistema invoca la función `init()` en el script de puntuación. Si hay excepciones no detectadas en la función `init()`, es posible que vea el error **CrashLoopBackOff** en el mensaje de error.

Use la información que aparece en la sección [Inspeccionar el registro de Docker](#dockerlog) para comprobar los registros.

## <a name="function-fails-get_model_path"></a>Error en la función: get_model_path()

A menudo, en la función `init()` en el script de puntuación, se llama a la función [Model.get_model_path()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#get-model-path-model-name--version-none---workspace-none-) para buscar un archivo de modelo o una carpeta de archivos de modelo en el contenedor. Si no se encuentra el archivo de modelo o la carpeta, se produce un error en la función. La manera más fácil de depurar este error es ejecutar el siguiente código de Python en el shell del contenedor:

```python
from azureml.core.model import Model
import logging
logging.basicConfig(level=logging.DEBUG)
print(Model.get_model_path(model_name='my-best-model'))
```

En este ejemplo se imprime la ruta de acceso local (relativa a `/var/azureml-app`) en el contenedor donde el script de puntuación espera encontrar el archivo de modelo o la carpeta. A continuación, puede comprobar si el archivo o la carpeta están realmente donde se espera que estén.

Establecer el nivel de registro en DEBUG puede provocar el registro de información adicional, que puede resultar útil para identificar el error.

## <a name="function-fails-runinput_data"></a>Error en la función: run(input_data)

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

**Nota**: La devolución de mensajes de error de la llamada `run(input_data)` se debe realizar solo con fines de depuración. Por motivos de seguridad, no debe devolver los mensajes de error de este modo en un entorno de producción.

## <a name="http-status-code-503"></a>Código de estado HTTP 503

Las implementaciones de Azure Kubernetes Service admiten el escalado automático, que permite que las réplicas se agreguen para admitir una carga adicional. Sin embargo, el escalador automático está diseñado para controlar cambios **graduales** en la carga. Si recibe grandes picos en las solicitudes por segundo, los clientes pueden recibir un código de estado HTTP 503.

Hay dos cosas que ayudan a impedir los códigos de estado 503:

* Cambiar el nivel de uso en el que el escalado automático crea nuevas réplicas.
    
    De forma predeterminada, se establece el uso de destino de escalado automático en el 70 %, lo que significa que el servicio puede controlar los picos en las solicitudes por segundo (RPS) de hasta un 30 %. Puede ajustar el objetivo de uso mediante el establecimiento de `autoscale_target_utilization` en un valor inferior.

    > [!IMPORTANT]
    > Este cambio no hace que las réplicas se creen *más rápidamente*. En lugar de eso, se crean con un umbral de uso más bajo. En lugar de esperar a que el servicio se use en un 70 %, si se cambia el valor a un 30 %, las réplicas se crearán cuando se produzca este 30 % de uso.
    
    Si el servicio web ya usa el número máximo de réplicas actuales y siguen apareciendo los códigos de estado 503, aumente el valor de `autoscale_max_replicas` con el fin de aumentar el número máximo de réplicas.

* Cambie el número mínimo de réplicas. El aumento en el número mínimo de réplicas proporciona un grupo más grande para controlar los picos entrantes.

    Para aumentar el número mínimo de réplicas, establezca `autoscale_min_replicas` en un valor superior. Puede calcular las réplicas necesarias mediante el código siguiente, reemplazando los valores por valores específicos del proyecto:

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
    > Si recibe picos de solicitudes más grandes que el número mínimo nuevo de réplicas que se puede controlar, es posible que reciba códigos de estado 503 otra vez. Por ejemplo, a medida que el tráfico al servicio aumente, es posible que deba aumentar el número mínimo de réplicas.

Para obtener más información sobre cómo configurar `autoscale_target_utilization`, `autoscale_max_replicas`, y `autoscale_min_replicas`, vea la referencia de módulo [AksWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py).


## <a name="advanced-debugging"></a>Depuración avanzada

En algunos casos, es posible que tenga que depurar interactivamente el código de Python incluido en la implementación de modelo. Por ejemplo, si el script de entrada presenta errores y no se puede determinar el motivo mediante un registro adicional. Mediante el uso de Visual Studio Code y las Herramientas de Python para Visual Studio (PTVSD), se puede adjuntar al código que se ejecuta dentro del contenedor de Docker.

> [!IMPORTANT]
> Este método de depuración no funciona cuando se usa `Model.deploy()` y `LocalWebservice.deploy_configuration` para implementar un modelo de manera local. En su lugar, debe crear una imagen con la clase [ContainerImage](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.containerimage?view=azure-ml-py). 
>
> Las implementaciones de servicios web locales requieren una instalación de Docker en funcionamiento en el sistema local. Docker se debe ejecutar antes de implementar un servicio web local. Para obtener información sobre cómo instalar y usar Docker, vea [https://www.docker.com/](https://www.docker.com/).

### <a name="configure-development-environment"></a>Configuración del entorno de desarrollo

1. Para instalar las Herramientas de Python para Visual Studio (PTVSD) en el entorno de desarrollo de VS Code local, use el comando siguiente:

    ```
    python -m pip install --upgrade ptvsd
    ```

    Para más información sobre cómo usar PTVSD con VS Code, consulte [Depuración remota](https://code.visualstudio.com/docs/python/debugging#_remote-debugging).

1. Para configurar VS Code para comunicarse con la imagen de Docker, cree una configuración de depuración nueva:

    1. En VS Code, seleccione el menú __Depurar__ y, luego, seleccione __Abrir configuraciones__. Se abre un archivo denominado __launch.json__.

    1. En el archivo __launch.json__, busque la línea que contiene `"configurations": [` e inserte el texto siguiente después de ella:

        ```json
        {
            "name": "Azure Machine Learning: Docker Debug",
            "type": "python",
            "request": "attach",
            "port": 5678,
            "host": "localhost",
            "pathMappings": [
                {
                    "localRoot": "${workspaceFolder}",
                    "remoteRoot": "/var/azureml-app"
                }
            ]
        }
        ```

        > [!IMPORTANT]
        > Si ya hay otras entradas en la sección de configuraciones, agregue una coma (,) después del código que insertó.

        Esta sección se adjunta al contenedor de Docker mediante el puerto 5678.

    1. Guarde el archivo __launch.json__.

### <a name="create-an-image-that-includes-ptvsd"></a>Creación de una imagen que incluye PTVSD

1. Modifique el entorno de conda para la implementación de manera que incluya PTVSD. En el ejemplo siguiente se muestra cómo se agrega con el parámetro `pip_packages`:

    ```python
    from azureml.core.conda_dependencies import CondaDependencies 
    
    # Usually a good idea to choose specific version numbers
    # so training is made on same packages as scoring
    myenv = CondaDependencies.create(conda_packages=['numpy==1.15.4',            
                                'scikit-learn==0.19.1', 'pandas==0.23.4'],
                                 pip_packages = ['azureml-defaults==1.0.17', 'ptvsd'])
    
    with open("myenv.yml","w") as f:
        f.write(myenv.serialize_to_string())
    ```

1. Para iniciar PTVSD y esperar una conexión cuando se inicia el servicio, agregue lo siguiente en la parte superior del archivo `score.py`:

    ```python
    import ptvsd
    # Allows other computers to attach to ptvsd on this IP address and port.
    ptvsd.enable_attach(address=('0.0.0.0', 5678), redirect_output = True)
    # Wait 30 seconds for a debugger to attach. If none attaches, the script continues as normal.
    ptvsd.wait_for_attach(timeout = 30)
    print("Debugger attached...")
    ```

1. Durante la depuración, es posible que quiera hacer cambios en los archivos de la imagen sin tener que volver a crearla. Para instalar un editor de texto (vim) en la imagen de Docker, cree un archivo de texto denominado `Dockerfile.steps` y use lo siguiente como el contenido del archivo:

    ```text
    RUN apt-get update && apt-get -y install vim
    ```

    Un editor de texto le permite modificar los archivos dentro de la imagen de Docker para probar los cambios sin tener que crear una imagen.

1. Para crear una imagen que use el archivo `Dockerfile.steps`, use el parámetro `docker_file` al crear una imagen. En el ejemplo siguiente se muestra cómo hacerlo:

    > [!NOTE]
    > En este ejemplo se da por supuesto que `ws` apunta al área de trabajo de Azure Machine Learning y que `model` es el modelo que se está implementando. El archivo `myenv.yml` contiene las dependencias de conda creadas en el paso 1.

    ```python
    from azureml.core.image import Image, ContainerImage
    image_config = ContainerImage.image_configuration(runtime= "python",
                                 execution_script="score.py",
                                 conda_file="myenv.yml",
                                 docker_file="Dockerfile.steps")

    image = Image.create(name = "myimage",
                     models = [model],
                     image_config = image_config, 
                     workspace = ws)
    # Print the location of the image in the repository
    print(image.image_location)
    ```

Una vez que se crea la imagen, se muestra la ubicación de la imagen en el registro. La ubicación será similar al texto siguiente:

```text
myregistry.azurecr.io/myimage:1
```

En este ejemplo de texto, el nombre del registro es `myregistry` y la imagen se denomina `myimage`. La versión de la imagen es `1`.

### <a name="download-the-image"></a>Descarga de la imagen

1. Abra un símbolo del sistema, terminal u otro Shell y use el comando de la [CLI de Azure](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) siguiente para autenticarse en la suscripción de Azure que incluye el área de trabajo de Azure Machine Learning:

    ```azurecli
    az login
    ```

1. Para autenticarse en la instancia de Azure Container Registry (ACR) que contiene la imagen, use el comando siguiente. Reemplace `myregistry` por el registro devuelto cuando registró la imagen:

    ```azurecli
    az acr login --name myregistry
    ```

1. Para descargar la imagen a la instancia de Docker local, use el comando siguiente. Reemplace `myimagepath` por la ubicación devuelta cuando registró la imagen:

    ```bash
    docker pull myimagepath
    ```

    La ruta de acceso de imagen debe ser similar a `myregistry.azurecr.io/myimage:1`. Donde `myregistry` es el registro, `myimage` es la imagen y `1` es la versión de la imagen.

    > [!TIP]
    > La autenticación del paso anterior no es permanente. Si espera lo suficiente entre el comando de autenticación y el comando de incorporación de datos, recibirá un error de autenticación. Si esto ocurre, vuelva a autenticarse.

    El tiempo que tarda en completar la descarga depende de la velocidad de su conexión a Internet. Durante el proceso se muestra un estado de la descarga. Una vez que se complete la descarga, puede usar el comando `docker images` para comprobar que se descargó.

1. Para facilitar el trabajo con la imagen, use el comando siguiente para agregar una etiqueta. Reemplace `myimagepath` por el valor de la ubicación del paso 2.

    ```bash
    docker tag myimagepath debug:1
    ```

    Para el resto de los pasos, puede hacer referencia a la imagen local como `debug:1` en lugar del valor de la ruta de acceso completa a la imagen.

### <a name="debug-the-service"></a>Depuración del servicio

> [!TIP]
> Si establece un tiempo de expiración para la conexión de PTVSD en el archivo `score.py`, debe conectar VS Code a la sesión de depuración antes de que se cumpla el tiempo de expiración. Inicie VS Code, abra la copia local de `score.py`, establezca un punto de interrupción y prepárelo antes de seguir los pasos de esta sección.
>
> Para más información sobre la depuración y el establecimiento de puntos de interrupción, consulte [Depuración](https://code.visualstudio.com/Docs/editor/debugging).

1. Para iniciar un contenedor de Docker con la imagen, use el comando siguiente:

    ```bash
    docker run --rm --name debug -p 8000:5001 -p 5678:5678 debug:1
    ```

1. Para adjuntar VS Code a PTVSD dentro del contenedor, abra VS Code y use la tecla F5 o seleccione __Depurar__. Cuando se le solicite, seleccione la configuración __Azure Machine Learning: Docker Debug__ (Azure Machine Learning Service: depuración de Docker). También puede seleccionar el icono de depuración en la barra lateral, la entrada __Azure Machine Learning: Docker Debug__ (Azure Machine Learning Service: depuración de Docker) en el menú desplegable Depurar y, luego, use la flecha verde para adjuntar el depurador.

    ![El icono de depuración, el botón de inicio de la depuración y el selector de configuración](media/how-to-troubleshoot-deployment/start-debugging.png)

En este punto, VS Code se conecta a PTVSD dentro del contenedor de Docker y se detiene en el punto de interrupción que se estableció anteriormente. Ahora puede recorrer el código a medida que se ejecuta, ver variables, etc.

Para más información sobre cómo usar VS Code para implementar Python, consulte [Depurar el código de Python](https://docs.microsoft.com/visualstudio/python/debugging-python-in-visual-studio?view=vs-2019).

<a id="editfiles"></a>
### <a name="modify-the-container-files"></a>Modificación de los archivos de contenedor

Para hacer cambios en los archivos de la imagen, puede adjuntar el contenedor en ejecución y ejecutar un shell de Bash. Desde ahí, puede usar vim para editar los archivos:

1. Para conectarse al contenedor en ejecución e iniciar un shell de Bash en el contenedor, use este comando:

    ```bash
    docker exec -it debug /bin/bash
    ```

1. Para buscar los archivos que usa el servicio, use el comando siguiente desde el shell de Bash en el contenedor:

    ```bash
    cd /var/azureml-app
    ```

    Desde aquí, puede usar vim para editar el archivo `score.py`. Para más información sobre el uso de vim, consulte el artículo sobre cómo [usar el editor Vim](https://www.tldp.org/LDP/intro-linux/html/sect_06_02.html).

1. Por lo general, los cambios realizados en un contenedor no se conservan. Para guardar los cambios que haga, use el comando siguiente antes de salir del shell que se inició en el paso anterior (es decir, en otro shell):

    ```bash
    docker commit debug debug:2
    ```

    Este comando crea una imagen denominada `debug:2` que contiene las ediciones.

    > [!TIP]
    > Deberá detener el contenedor actual y empezar a usar la versión nueva antes de que los cambios surtan efecto.

1. Asegúrese de mantener los cambios que haga en los archivos del contenedor sincronizados con los archivos locales que usa VS Code. De lo contrario, la experiencia del depurador no funcionará según lo esperado.

### <a name="stop-the-container"></a>Detención del contenedor

Para detener el contenedor, use el comando siguiente:

```bash
docker stop debug
```

## <a name="next-steps"></a>Pasos siguientes

Más información acerca de la implementación:

* [Cómo implementar y dónde](how-to-deploy-and-where.md)
* [Tutorial: Entrenamiento e implementación de modelos](tutorial-train-models-with-aml.md)
