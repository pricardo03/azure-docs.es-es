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
ms.date: 05/02/2018
ms.custom: seodec18
ms.openlocfilehash: 3730e4a0bfa05e6606e50b9bbd9d9152e2488954
ms.sourcegitcommit: 16cb78a0766f9b3efbaf12426519ddab2774b815
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65851716"
---
# <a name="troubleshooting-azure-machine-learning-service-aks-and-aci-deployments"></a>Solución de problemas de implementaciones de AKS y ACI de Azure Machine Learning Service

Aprenda a evitar o solucionar errores comunes de implementación de Docker con Azure Container Instances (ACI) y con el servicio Azure Machine Learning con Azure Kubernetes Service (AKS).

Al implementar un modelo en el servicio de Azure Machine Learning, el sistema realiza una serie de tareas. Las tareas de implementación son:

1. Registrar el modelo en el registro de modelos del área de trabajo.

2. Compilar una imagen de Docker, incluido:
    1. Descargue el modelo registrado desde el registro. 
    2. Cree un archivo dockerfile, con un entorno de Python basado en las dependencias que especifique en el archivo yaml del entorno.
    3. Agregue los archivos del modelo y el script de puntuación que proporciona en el archivo dockerfile.
    4. Cree una nueva imagen de Docker con el archivo dockerfile.
    5. Registre la imagen de Docker en la instancia de Azure Container Registry asociada con el área de trabajo.

    > [!IMPORTANT]
    > Dependiendo de su código, creación de imágenes se realiza automáticamente sin la intervención del usuario.

3. Implementar la imagen de Docker en el servicio de instancia de contenedor de Azure (ACI) o Azure Kubernetes Service (AKS).

4. Iniciar un nuevo contenedor (o contenedores) en ACI o AKS. 

Más información sobre este proceso en la introducción a la [administración de modelos](concept-model-management-and-deployment.md).

## <a name="before-you-begin"></a>Antes de empezar

Si tiene algún problema, lo primero es dividir la tarea de implementación (descrita anteriormente) en pasos individuales para aislar el problema.

Dividir la implementación en tareas es útil si está utilizando el [Webservice.deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#deploy-workspace--name--model-paths--image-config--deployment-config-none--deployment-target-none-) API, o [Webservice.deploy_from_model()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#deploy-from-model-workspace--name--models--image-config--deployment-config-none--deployment-target-none-) API, como ambas funciones realizar los pasos mencionados anteriormente como un acción única. Normalmente, esas API resultan prácticos, pero resulta útil dividir los pasos para solucionar el problema al reemplazarlos con el siguiente llama a la API.

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

Si no se puede generar la imagen de Docker, el [image.wait_for_creation()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.image(class)?view=azure-ml-py#wait-for-creation-show-output-false-) o [service.wait_for_deployment()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#wait-for-deployment-show-output-false-) llamada produce un error con algunos mensajes de error que pueden ofrecer algunas pistas. También puede encontrar más detalles acerca de los errores en el registro de compilación de la imagen. A continuación encontrará algunos ejemplos de código que muestra cómo detectar el URI del registro de compilación de la imagen.

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

La compilación de imagen también puede producir un error debido a un problema con la directiva de acceso en Azure Key Vault. Esta situación puede producirse cuando se usa una plantilla de Azure Resource Manager para crear el área de trabajo y recursos asociados (incluido Azure Key Vault), varias veces. Por ejemplo, mediante la plantilla varias veces con los mismos parámetros como parte de una canalización de implementación e integración continua.

La mayoría las operaciones de creación de recursos a través de plantillas son idempotentes, pero Key Vault borra las directivas de acceso cada vez que se usa la plantilla. Al desactivar el acceso de saltos de directivas de acceso al almacén de claves para cualquier área de trabajo existente que esté usando. Esta condición da como resultado errores al intentar crear nuevas imágenes. Los siguientes son ejemplos de los errores que se pueden recibir:

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
* Examine las directivas de acceso de Key Vault y, a continuación, use estas directivas para establecer el `accessPolicies` propiedad de la plantilla.
* Compruebe si ya existe el recurso de Key Vault. Si es así, no volver a crearla con la plantilla. Por ejemplo, agregue un parámetro que le permite deshabilitar la creación del recurso de Key Vault si ya existe.

## <a name="debug-locally"></a>La depuración local

Si encuentra problemas al implementar un modelo en ACI o AKS, vuelva a implementarlo como un servicio web local. Mediante un servicio web local facilita la solución de problemas. Se descarga la imagen de Docker que contiene el modelo y se inicia en el sistema local.

> [!IMPORTANT]
> Las implementaciones de servicios web locales requieren una instalación de Docker en el sistema local en funcionamiento. Docker se debe ejecutar antes de implementar un servicio web local. Para obtener información sobre cómo instalar y usar Docker, consulte [ https://www.docker.com/ ](https://www.docker.com/).

> [!WARNING]
> No se admiten las implementaciones de servicio web local para escenarios de producción.

Para implementar localmente, modificar el código para usar `LocalWebservice.deploy_configuration()` para crear una configuración de implementación. A continuación, usar `Model.deploy()` para implementar el servicio. En el ejemplo siguiente se implementa un modelo (incluidos en el `model` variable) como un servicio web local:

```python
from azureml.core.model import InferenceConfig
from azureml.core.webservice import LocalWebservice

# Create inference configuration. This creates a docker image that contains the model.
inference_config = InferenceConfig(runtime= "python", 
                                   execution_script="score.py",
                                   conda_file="myenv.yml")

# Create a local deployment, using port 8890 for the web service endpoint
deployment_config = LocalWebservice.deploy_configuration(port=8890)
# Deploy the service
service = Model.deploy(ws, "mymodel", [model], inference_config, deployment_config)
# Wait for the deployment to complete
service.wait_for_deployment(True)
# Display the port that the web service is available on
print(service.port)
```

En este momento, puede trabajar con el servicio de forma habitual. Por ejemplo, el código siguiente muestra cómo enviar datos al servicio:

```python
import json

test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})

test_sample = bytes(test_sample,encoding = 'utf8')

prediction = service.run(input_data=test_sample)
print(prediction)
```

### <a name="update-the-service"></a>Actualizar el servicio

Durante las pruebas locales, es posible que deba actualizar el `score.py` archivo para agregar un registro o intentar resolver los problemas que haya descubierto. Para volver a cargar los cambios realizados en el `score.py` , debe usar `reload()`. Por ejemplo, el código siguiente vuelve a cargar la secuencia de comandos para el servicio y, a continuación, envía datos a él. Se usa para puntuar los datos mediante la actualización `score.py` archivo:

```python
service.reload()
print(service.run(input_data=test_sample))
```

> [!NOTE]
> El script se vuelve a cargar desde la ubicación especificada por el `InferenceConfig` objeto utilizado por el servicio.

Para cambiar la configuración de implementación, dependencias de Conda o el modelo, use [update()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#update--args-). El ejemplo siguiente actualiza el modelo usado por el servicio:

```python
service.update([different_model], inference_config, deployment_config)
```

### <a name="delete-the-service"></a>Eliminar el servicio

Para eliminar el servicio, use [delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#delete--).

### <a id="dockerlog"></a> Inspeccionar el registro de Docker

Puede imprimir los mensajes detallados de registro del motor de Docker desde el objeto de servicio. Puede ver el registro para las implementaciones locales, ACI y AKS. El ejemplo siguiente muestra cómo imprimir los registros.

```python
# if you already have the service object handy
print(service.get_logs())

# if you only know the name of the service (note there might be multiple services with the same name but different version number)
print(ws.webservices['mysvc'].get_logs())
```

## <a name="service-launch-fails"></a>Error en el inicio del servicio

Después de la imagen se compile correctamente, el sistema intenta iniciar un contenedor con la configuración de implementación. Como parte del proceso de puesta en marcha del contenedor, el sistema invoca la función `init()` en el script de puntuación. Si hay excepciones no detectadas en la función `init()`, es posible que vea el error **CrashLoopBackOff** en el mensaje de error.

Use la información en el [inspeccionar el registro de Docker](#dockerlog) sección comprobar los registros.

## <a name="function-fails-getmodelpath"></a>Error en la función: get_model_path()

A menudo, en el `init()` función en el script de puntuación, [Model.get_model_path()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#get-model-path-model-name--version-none---workspace-none-) función se invoca para buscar un archivo de modelo o una carpeta de archivos de modelo en el contenedor. Si no se encuentra el archivo de modelo o la carpeta, se produce un error en la función. La manera más fácil de depurar este error es ejecutar el siguiente código de Python en el shell del contenedor:

```python
import logging
logging.basicConfig(level=logging.DEBUG)
from azureml.core.model import Model
print(Model.get_model_path(model_name='my-best-model'))
```

Este ejemplo imprime la ruta de acceso local (relativo a `/var/azureml-app`) en el contenedor donde se espera el script de puntuación para buscar el archivo de modelo o una carpeta. A continuación, puede comprobar si el archivo o la carpeta están realmente donde se espera que estén.

Establecer el nivel de registro para la depuración, puede generar información adicional que se registre, que puede ser útil para identificar el error.

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

**Nota**: La devolución de mensajes de error de la llamada `run(input_data)` se debe realizar solo con fines de depuración. Por motivos de seguridad no debe devolver mensajes de error de este modo en un entorno de producción.

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
