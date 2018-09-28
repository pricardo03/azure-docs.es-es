---
title: Implementación de un modelo como servicio web en una FPGA con Azure Machine Learning
description: Obtenga información sobre cómo implementar un servicio web con un modelo que se ejecuta en una FPGA con Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: tedway
author: tedway
ms.date: 09/24/2018
ms.openlocfilehash: ee67585a523ab96b1442d9eee3e9dfd55a758d32
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46971491"
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning"></a>Implementación de un modelo como servicio web en una FPGA con Azure Machine Learning

Puede implementar un modelo como un servicio web en [Field-programmable Gate Arrays (FPGA)](concept-accelerate-with-fpgas.md).  El uso de las FPGA brinda una inferencia de latencia muy baja, incluso con un tamaño de lote único.   

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción de Azure. Si no tiene una, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

- Un área de trabajo de Azure Machine Learning y el SDK de Azure Machine Learning para Python instalado. Descubra cómo obtener estos requisitos previos con el documento [How to configure a development environment](how-to-configure-environment.md) (Cómo configurar un entorno de desarrollo).
 
  - El área de trabajo debe estar en la región *Este de EE. UU. 2*.

  - Instale los extras de "contrib":

    ```shell
    pip install --upgrade azureml-sdk[contrib]
    ```  

## <a name="create-and-deploy-your-model"></a>Creación e implementación de un modelo
Cree una canalización para preprocesar la imagen de entrada, caracterizarla mediante ResNet 50 en una FPGA y, a continuación, ejecute las características a través de un clasificador entrenado en el conjunto de datos de ImageNet.

Siga las instrucciones para:

* Definir la canalización del modelo
* Implementación del modelo
* Consumo del modelo implementado
* Eliminación de los servicios implementados

> [!IMPORTANT]
> Para optimizar la latencia y el rendimiento, el cliente debe estar en la misma región de Azure que el punto de conexión.  Actualmente, las API se crean en la región de Azure Este de EE. UU.

### <a name="get-the-notebook"></a>Obtención del cuaderno

Para su comodidad, este tutorial está disponible como un cuaderno de Jupyter. Use cualquiera de estos métodos para ejecutar el cuaderno `project-brainwave/project-brainwave-quickstart.ipynb`:

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-in-azure-notebook.md)]

### <a name="preprocess-image"></a>Imagen de preprocesamiento
La primera fase de la canalización es preprocesar las imágenes.

```python
import os
import tensorflow as tf

# Input images as a two-dimensional tensor containing an arbitrary number of images represented a strings
import azureml.contrib.brainwave.models.utils as utils
in_images = tf.placeholder(tf.string)
image_tensors = utils.preprocess_array(in_images)
print(image_tensors.shape)
```
### <a name="add-featurizer"></a>Incorporación de un caracterizador
Inicialice el modelo y descargue un punto de control de TensorFlow de la versión cuantificada de ResNet50 que se usará como caracterizador.

```python
from azureml.contrib.brainwave.models import QuantizedResnet50, Resnet50
model_path = os.path.expanduser('~/models')
model = QuantizedResnet50(model_path, is_frozen = True)
feature_tensor = model.import_graph_def(image_tensors)
print(model.version)
print(feature_tensor.name)
print(feature_tensor.shape)
```

### <a name="add-classifier"></a>Incorporación de un clasificador
Este clasificador se ha entrenado en el conjunto de datos de ImageNet.

```python
classifier_input, classifier_output = Resnet50.get_default_classifier(feature_tensor, model_path)
```

### <a name="create-service-definition"></a>Cree la definición de servicio
Ahora que ha definido el procesamiento previo de imágenes, el caracterizador y el clasificador que se ejecuta en el servicio, puede crear una definición de servicio. La definición de servicio es un conjunto de archivos generado a partir del modelo que se implementa en el servicio FPGA. La definición del servicio consta de una canalización. La canalización consta de una serie de fases que se ejecutan en orden.  Se admiten las fases TensorFlow, Keras y BrainWave.  Las fases se ejecutan en el orden en el servicio, con la salida de cada entrada de fase en la fase posterior.

Para crear una fase TensorFlow, especifique una sesión que contiene el gráfico (en este caso se usa el gráfico de forma predeterminada) y los tensores de entrada y salida para esta fase.  Esta información se usa para guardar el gráfico para que se pueda ejecutar en el servicio.

```python
from azureml.contrib.brainwave.pipeline import ModelDefinition, TensorflowStage, BrainWaveStage

save_path = os.path.expanduser('~/models/save')
model_def_path = os.path.join(save_path, 'service_def.zip')

model_def = ModelDefinition()
with tf.Session() as sess:
    model_def.pipeline.append(TensorflowStage(sess, in_images, image_tensors))
    model_def.pipeline.append(BrainWaveStage(sess, model))
    model_def.pipeline.append(TensorflowStage(sess, classifier_input, classifier_output))
    model_def.save(model_def_path)
    print(model_def_path)
```

### <a name="deploy-model"></a>Implementación del modelo
Cree un servicio de la definición del servicio.  El área de trabajo debe estar en la ubicación Este de EE. UU. 2.

```python
from azureml.core import Workspace

ws = Workspace.from_config()
print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')

from azureml.core.model import Model
model_name = "resnet-50-rtai"
registered_model = Model.register(ws, model_def_path, model_name)

from azureml.core.webservice import Webservice
from azureml.exceptions import WebserviceException
from azureml.contrib.brainwave import BrainwaveWebservice, BrainwaveImage
service_name = "imagenet-infer"
service = None
try:
    service = Webservice(ws, service_name)
except WebserviceException:
    image_config = BrainwaveImage.image_configuration()
    deployment_config = BrainwaveWebservice.deploy_configuration()
    service = Webservice.deploy_from_model(ws, service_name, [registered_model], image_config, deployment_config)
    service.wait_for_deployment(true)
```

### <a name="test-the-service"></a>Probar el servicio
Para enviar una imagen a la API y probar la respuesta, agregue una asignación de identificador de la clase de salida en el nombre de clase ImageNet.

```python
import requests
classes_entries = requests.get("https://raw.githubusercontent.com/Lasagne/Recipes/master/examples/resnet50/imagenet_classes.txt").text.splitlines()
```

Llame al servicio y reemplace el nombre de archivo "your-image.jpg" a continuación con una imagen del equipo. 

```python
with open('your-image.jpg') as f:
    results = service.run(f)
# map results [class_id] => [confidence]
results = enumerate(results)
# sort results by confidence
sorted_results = sorted(results, key=lambda x: x[1], reverse=True)
# print top 5 results
for top in sorted_results[:5]:
    print(classes_entries[top[0]], 'confidence:', top[1])
``` 

### <a name="clean-up-service"></a>Limpieza del servicio
Elimina el servicio web.

```python
service.delete()
    
registered_model.delete()
```

## <a name="secure-fpga-web-services"></a>Protección de los servicios web FPGA

Los modelos de Azure Machine Learning que se ejecutan en FPGA proporcionan compatibilidad con el protocolo SSL y autenticación basada en claves. Esto le permite restringir el acceso a su servicio y proteger los datos que envían los clientes.

> [!IMPORTANT]
> La autenticación solo está habilitada para los servicios que han proporcionado un certificado SSL y una clave. 
>
> Si no habilita SSL, cualquier usuario de Internet podrá hacer llamadas al servicio.
>
> Si habilita SSL, se requiere una clave de autenticación cuando se accede al servicio.

El protocolo SSL cifra los datos que se envían entre el cliente y el servicio. También lo usa el cliente para comprobar la identidad del servidor.

Puede implementar un servicio con SSL habilitado, o bien actualizar un servicio ya implementado para habilitarlo. Los pasos son los mismos:

1. Adquirir un nombre de dominio.

2. Adquirir un certificado SSL.

3. Implementar o actualizar el servicio con SSL habilitado.

4. Actualizar el DNS para que apunte al servicio.

### <a name="acquire-a-domain-name"></a>Adquisición de un nombre de dominio

Si todavía no tiene un nombre de dominio, puede comprar uno en un __registrador de nombres de dominio__. El proceso varía entre los distintos registradores, al igual que el costo. El registrador también proporciona herramientas para administrar el nombre de dominio. Estas herramientas se usan para asignar un nombre de dominio completo (como www.contoso.com) a la dirección IP donde se hospeda el servicio.

### <a name="acquire-an-ssl-certificate"></a>Adquisición de un certificado SSL

Hay muchas maneras de obtener un certificado SSL. La más común es comprar uno en una __entidad de certificación__ (CA). Independientemente de dónde se obtiene el certificado, se necesitan estos archivos:

* Un __certificado__. El certificado debe incluir la cadena de certificados completa y debe estar codificado en PEM.
* Una __clave__. La clave debe estar codificada en PEM.

> [!TIP]
> Si la entidad de certificación no puede proporcionar el certificado y la clave como archivos codificados en PEM, puede usar una utilidad como [OpenSSL](https://www.openssl.org/) para cambiar el formato.

> [!IMPORTANT]
> Los certificados autofirmados solo se deben usar para desarrollo. No se deben usar en el entorno de producción.
>
> Si usa un certificado autofirmado, consulte la sección [Consumo de servicios con certificados autofirmados](#self-signed) para instrucciones específicas.

> [!WARNING]
> Cuando solicite un certificado, debe proporcionar el nombre de dominio completo (FQDN) de la dirección que pretende usar para el servicio. Por ejemplo, www.contoso.com. La dirección que aparece en el certificado y la dirección que usan los clientes se comparan cuando se valida la identidad del servicio.
>
> Si no coinciden, se generará un error. 

### <a name="deploy-or-update-the-service-with-ssl-enabled"></a>Implementación o actualización del servicio con el protocolo SSL habilitado

Para implementar el servicio con el protocolo SSL habilitado, establezca el parámetro `ssl_enabled` en `True`. Establezca el parámetro `ssl_certificate` en el valor del archivo __certificate__ y `ssl_key` en el valor del archivo __key__. En el ejemplo siguiente se muestra cómo implementar un servicio con el protocolo SSL habilitado:

```python
from amlrealtimeai import DeploymentClient

subscription_id = "<Your Azure Subscription ID>"
resource_group = "<Your Azure Resource Group Name>"
model_management_account = "<Your AzureML Model Management Account Name>"
location = "eastus2"

model_name = "resnet50-model"
service_name = "quickstart-service"

deployment_client = DeploymentClient(subscription_id, resource_group, model_management_account, location)

with open('cert.pem','r') as cert_file:
    with open('key.pem','r') as key_file:
        cert = cert_file.read()
        key = key_file.read()
        service = deployment_client.create_service(service_name, model_id, ssl_enabled=True, ssl_certificate=cert, ssl_key=key)
```

La respuesta de la operación `create_service` incluye la dirección IP del servicio. La dirección IP se usa al asignar el nombre DNS a la dirección IP del servicio.

La respuesta también incluye una __clave principal__ y una __clave secundaria__ que se usan para consumir el servicio.

### <a name="update-your-dns-to-point-to-the-service"></a>Actualización del DNS para que apunte al servicio

Use las herramientas que proporciona el registrador de nombres de dominio para actualizar el registro de DNS de su nombre de dominio. El registro debe apuntar a la dirección IP del servicio.

> [!NOTE]
> En función del registrador y del período de vida (TTL) configurado para el nombre de dominio, pueden pasar de varios minutos a varias horas antes de que los clientes puedan resolver el nombre de dominio.

### <a name="consume-authenticated-services"></a>Consumo de servicios autenticados

En los ejemplos siguientes se muestra cómo consumir un servicio autenticado con Python y C#:

> [!NOTE]
> Reemplace `authkey` por la clave principal o secundaria que se devolvió al crear el servicio.

```python
from amlrealtimeai import PredictionClient
client = PredictionClient(service.ipAddress, service.port, use_ssl=True, access_token="authKey")
image_file = R'C:\path_to_file\image.jpg'
results = client.score_image(image_file)
```

```csharp
var client = new ScoringClient(host, 50051, useSSL, "authKey");
float[,] result;
using (var content = File.OpenRead(image))
    {
        IScoringRequest request = new ImageRequest(content);
        result = client.Score<float[,]>(request);
    }
```

Otros clientes gRPC pueden autenticar las solicitudes al establecer un encabezado de autorización. El enfoque general es crear un objeto `ChannelCredentials` que combina `SslCredentials` con `CallCredentials`. Esto se agrega al encabezado de autorización de la solicitud. Para más información sobre cómo implementar la compatibilidad con los encabezados específicos, consulte [https://grpc.io/docs/guides/auth.html](https://grpc.io/docs/guides/auth.html).

En los ejemplos siguientes se muestra cómo establecer el encabezado en C# y Go:

```csharp
creds = ChannelCredentials.Create(baseCreds, CallCredentials.FromInterceptor(
                      async (context, metadata) =>
                      {
                          metadata.Add(new Metadata.Entry("authorization", "authKey"));
                          await Task.CompletedTask;
                      }));

```

```go
conn, err := grpc.Dial(serverAddr, 
    grpc.WithTransportCredentials(credentials.NewClientTLSFromCert(nil, "")),
    grpc.WithPerRPCCredentials(&authCreds{
    Key: "authKey"}))

type authCreds struct {
    Key string
}

func (c *authCreds) GetRequestMetadata(context.Context, uri ...string) (map[string]string, error) {
    return map[string]string{
        "authorization": c.Key,
    }, nil
}

func (c *authCreds) RequireTransportSecurity() bool {
    return true
}
```

### <a id="self-signed"></a>Consumo de servicios con certificados autofirmados

Hay dos maneras de permitir que el cliente se autentique en un servicio protegido con un certificado autofirmado:

* En el sistema cliente, establezca la variable de entorno `GRPC_DEFAULT_SSL_ROOTS_FILE_PATH` en el sistema cliente para que apunte al archivo de certificado.

* Cuando construya un objeto `SslCredentials`, pase el contenido del archivo de certificado al constructor.

Usar cualquiera de los métodos hace que gRPC use el certificado como el certificado raíz.

> [!IMPORTANT]
> gRPC no acepta certificados que no son de confianza. Si se usa un certificado que no es de confianza, se generará un error con el código de estado `Unavailable`. Los detalles del error incluyen `Connection Failed`.
