---
title: Implementación de modelos en FPGA
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre cómo implementar un servicio web con un modelo que se ejecuta en una FPGA con Azure Machine Learning Service para obtener una inferencia de latencia ultrabaja.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: larryfr
ms.author: tedway
author: tedway
ms.date: 05/02/2019
ms.custom: seodec18
ms.openlocfilehash: 6cb9de60fe63c936da7340e6ec540a37163216f5
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67074971"
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning-service"></a>Implementación de un modelo como servicio web en una FPGA con Azure Machine Learning Service

Puede implementar un modelo como un servicio web en [matrices de puertas programables (FPGA)](concept-accelerate-with-fpgas.md) con modelos acelerados mediante hardware de Azure Machine Learning. El uso de las FPGA brinda una inferencia de latencia ultrabaja, incluso con un tamaño de lote único. La inferencia, o modelo de puntuación, es la fase donde se usa el modelo implementado para la predicción, frecuentemente en datos de producción.

Actualmente están disponibles los siguientes modelos:
  - ResNet 50
  - ResNet 152
  - DenseNet-121
  - VGG-16
  - SSD-VGG

Las FPGA están disponibles en estas regiones de Azure:
  - Este de EE. UU
  - Sudeste asiático
  - Europa occidental
  - Oeste de EE. UU. 2

> [!IMPORTANT]
> Para optimizar el rendimiento y la latencia, el envío de datos del cliente al modelo de FPGA debe hacerse en una de las regiones anteriores (aquella en la que se haya implementado el modelo).

## <a name="prerequisites"></a>Requisitos previos

- Una suscripción de Azure.  Si no tiene una, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

- Cuota de FPGA.  Use la CLI de Azure para comprobar si dispone de cuota.
    ```shell
    az vm list-usage --location "eastus" -o table
    ```

    Las otras ubicaciones son ``southeastasia``, ``westeurope`` y ``westus2``.

    En la columna "Name" (Nombre), busque "Standard PBS Family vCPUs" (vCPU de la familia PBS estándar) y asegúrese de que tiene al menos 6 vCPU en "CurrentValue" (Valor actual).

    Si no tiene cuota, envíe un formulario de solicitud [aquí](https://aka.ms/accelerateAI).

- Un área de trabajo del servicio Azure Machine Learning y el SDK de Azure Machine Learning para Python instalado. Descubra cómo obtener estos requisitos previos con el documento [How to configure a development environment](how-to-configure-environment.md) (Cómo configurar un entorno de desarrollo).
 
- El SDK de Python para modelos acelerados por hardware:

    ```shell
    pip install --upgrade azureml-accel-models
    ```

## <a name="sample-notebooks"></a>Cuadernos de ejemplo

Para su comodidad, hay disponibles [cuadernos de ejemplo](https://aka.ms/aml-accel-models-notebooks) para el ejemplo siguiente y otros ejemplos.

## <a name="create-and-containerize-your-model"></a>Creación e inclusión del modelo en un contenedor

En este documento se describe cómo crear un gráfico de TensorFlow para preprocesar la imagen de entrada, caracterizarla mediante ResNet 50 en una FPGA y, a continuación, ejecutar las características mediante un clasificador entrenado en el conjunto de datos de ImageNet.

Siga las instrucciones para:

* Definir el modelo de TensorFlow
* Convertir el modelo
* Implementación del modelo
* Consumo del modelo implementado
* Eliminación de los servicios implementados

### <a name="load-azure-ml-workspace"></a>Cargar el área de trabajo de Azure ML

Cargue el área de trabajo de Azure ML.

```python
import os
import tensorflow as tf
 
from azureml.core import Workspace

ws = Workspace.from_config()
print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')
```

### <a name="preprocess-image"></a>Imagen de preprocesamiento

La entrada al servicio web es una imagen JPEG.  El primer paso es descodificar la imagen JPEG y preprocesarla.  Las imágenes JPEG se tratan como cadenas y el resultado son tensores que serán la entrada del modelo ResNet 50.

```python
# Input images as a two-dimensional tensor containing an arbitrary number of images represented a strings
import azureml.accel.models.utils as utils
tf.reset_default_graph()

in_images = tf.placeholder(tf.string)
image_tensors = utils.preprocess_array(in_images)
print(image_tensors.shape)
```

### <a name="load-featurizer"></a>Carga de un caracterizador

Inicialice el modelo y descargue un punto de control de TensorFlow de la versión cuantificada de ResNet50 que se usará como caracterizador.  Puede reemplazar "QuantizedResnet50" en el fragmento de código siguiente mediante la importación de otras redes neuronales profundas:

- QuantizedResnet152
- QuantizedVgg16
- Densenet121

```python
from azureml.accel.models import QuantizedResnet50
save_path = os.path.expanduser('~/models')
model_graph = QuantizedResnet50(save_path, is_frozen = True)
feature_tensor = model_graph.import_graph_def(image_tensors)
print(model_graph.version)
print(feature_tensor.name)
print(feature_tensor.shape)
```

### <a name="add-classifier"></a>Incorporación de un clasificador

Este clasificador se ha entrenado en el conjunto de datos de ImageNet.  Hay ejemplos disponibles para transferir aprendizaje y entrenamiento de las ponderaciones personalizadas en el conjunto de [cuadernos de ejemplo](https://aka.ms/aml-notebooks).

```python
classifier_output = model_graph.get_default_classifier(feature_tensor)
print(classifier_output)
```

### <a name="save-the-model"></a>Guardar el modelo

Ahora que se han cargado el clasificador, el preprocesador y caracterizador de ResNet 50, guarde el gráfico y las variables asociadas como modelo.

```python
model_name = "resnet50"
model_save_path = os.path.join(save_path, model_name)
print("Saving model in {}".format(model_save_path))

with tf.Session() as sess:
    model_graph.restore_weights(sess)
    tf.saved_model.simple_save(sess, model_save_path,
                                   inputs={'images': in_images},
                                   outputs={'output_alias': classifier_output})
```

### <a name="save-input-and-output-tensors"></a>Guardar los tensores de entrada y salida
Los tensores de entrada y salida que se han creado durante los pasos de preprocesamiento y el clasificador serán necesarios para la inferencia y la conversión del modelo.

```python
input_tensors = in_images.name
output_tensors = classifier_output.name

print(input_tensors)
print(output_tensors)
```

> [!IMPORTANT]
> Guarde los tensores de entrada y salida porque los necesitará para las solicitudes de inferencia y conversión del modelo.

Los modelos disponibles y los tensores de salida del clasificador predeterminado correspondiente se muestran a continuación, y es lo que se usaría para la inferencia si se ha usado el clasificador predeterminado.

+ Resnet50, QuantizedResnet50
  ```
  output_tensors = "classifier_1/resnet_v1_50/predictions/Softmax:0"
  ```
+ Resnet152, QuantizedResnet152
  ```
  output_tensors = "classifier/resnet_v1_152/predictions/Softmax:0"
  ```
+ Densenet121, QuantizedDensenet121
  ```
  output_tensors = "classifier/densenet121/predictions/Softmax:0"
  ```
+ Vgg16, QuantizedVgg16
  ```
  output_tensors = "classifier/vgg_16/fc8/squeezed:0"
  ```
+ SsdVgg, QuantizedSsdVgg
  ```
  output_tensors = ['ssd_300_vgg/block4_box/Reshape_1:0', 'ssd_300_vgg/block7_box/Reshape_1:0', 'ssd_300_vgg/block8_box/Reshape_1:0', 'ssd_300_vgg/block9_box/Reshape_1:0', 'ssd_300_vgg/block10_box/Reshape_1:0', 'ssd_300_vgg/block11_box/Reshape_1:0', 'ssd_300_vgg/block4_box/Reshape:0', 'ssd_300_vgg/block7_box/Reshape:0', 'ssd_300_vgg/block8_box/Reshape:0', 'ssd_300_vgg/block9_box/Reshape:0', 'ssd_300_vgg/block10_box/Reshape:0', 'ssd_300_vgg/block11_box/Reshape:0']
  ```

### <a name="register-model"></a>Registro del modelo

[Registre](./concept-model-management-and-deployment.md) el modelo que se ha creado.  Agregar etiquetas y otros metadatos sobre el modelo le ayuda a realizar el seguimiento de los modelos entrenados.

```python
from azureml.core.model import Model

registered_model = Model.register(workspace = ws,
                                  model_path = model_save_path,
                                  model_name = model_name)

print("Successfully registered: ", registered_model.name, registered_model.description, registered_model.version, sep = '\t')
```

Si ya se ha registrado un modelo y quiere cargarlo, puede recuperarlo.

```python
from azureml.core.model import Model
model_name = "resnet50"
# By default, the latest version is retrieved. You can specify the version, i.e. version=1
registered_model = Model(ws, name="resnet50")
print(registered_model.name, registered_model.description, registered_model.version, sep = '\t')
```

### <a name="convert-model"></a>Conversión del modelo

Convierta el gráfico de TensorFlow al formato de intercambio de red neuronal abierta ([ONNX](https://onnx.ai/)).  Deberá proporcionar los nombres de los tensores de entrada y salida y estos nombres los utilizará el cliente cuando se consuma el servicio web.

```python
from azureml.accel import AccelOnnxConverter

convert_request = AccelOnnxConverter.convert_tf_model(ws, registered_model, input_tensors, output_tensors)

# If it fails, you can run wait_for_completion again with show_output=True.
convert_request.wait_for_completion(show_output = False)

# If the above call succeeded, get the converted model
converted_model = convert_request.result
print("\nSuccessfully converted: ", converted_model.name, converted_model.url, converted_model.version, 
      converted_model.id, converted_model.created_time, '\n')
```

### <a name="create-docker-image"></a>Creación de una imagen de Docker

El modelo convertido y todas las dependencias se agregan a una imagen de Docker.  A continuación, se puede implementar esta imagen de Docker y se puede crear una instancia de ella.  Entre los destinos de implementación admitidos se incluye AKS en la nube o un dispositivo perimetral como [Azure Data Box Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview).  También puede agregar etiquetas y descripciones para la imagen de Docker registrada.

```python
from azureml.core.image import Image
from azureml.accel import AccelContainerImage

image_config = AccelContainerImage.image_configuration()
# Image name must be lowercase
image_name = "{}-image".format(model_name)

image = Image.create(name = image_name,
                     models = [converted_model],
                     image_config = image_config, 
                     workspace = ws)
image.wait_for_creation(show_output = False)
```

Enumere las imágenes por etiqueta y obtenga los registros detallados para cualquier tipo de depuración.

```python
for i in Image.list(workspace = ws):
    print('{}(v.{} [{}]) stored at {} with build log {}'.format(i.name, i.version, i.creation_state, i.image_location, i.image_build_log_uri))
```

## <a name="model-deployment"></a>Implementación del modelo

### <a name="deploy-to-the-cloud"></a>Implementación en la nube

Para implementar el modelo como un servicio web de producción a gran escala, use Azure Kubernetes Service (AKS). Puede crear uno con el SDK de Azure Machine Learning, la CLI o Azure Portal.

```python
from azureml.core.compute import AksCompute, ComputeTarget

# Specify the Standard_PB6s Azure VM
prov_config = AksCompute.provisioning_configuration(vm_size = "Standard_PB6s",
                                                    agent_count = 1)

aks_name = 'my-aks-cluster'
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws, 
                                  name = aks_name, 
                                  provisioning_configuration = prov_config)
```

La implementación de AKS puede tardar unos 15 minutos.  Compruebe que la implementación se ha realizado correctamente.

```python
aks_target.wait_for_completion(show_output = True)
print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)
```

Implemente el contenedor en el clúster de AKS.
```python
from azureml.core.webservice import Webservice, AksWebservice

# For this deployment, set the web service configuration without enabling auto-scaling or authentication for testing
aks_config = AksWebservice.deploy_configuration(autoscale_enabled=False,
                                                num_replicas=1,
                                                auth_enabled = False)

aks_service_name ='my-aks-service'

aks_service = Webservice.deploy_from_image(workspace = ws,
                                           name = aks_service_name,
                                           image = image,
                                           deployment_config = aks_config,
                                           deployment_target = aks_target)
aks_service.wait_for_deployment(show_output = True)
```

#### <a name="test-the-cloud-service"></a>Prueba del servicio en la nube
La imagen de Docker admite gRPC y la API de "predicción" de TensorFlow Serving.  Use el cliente de ejemplo para llamar a la imagen de Docker y obtener predicciones a partir del modelo.  Hay código de cliente de ejemplo disponible:
- [Python](https://github.com/Azure/aml-real-time-ai/blob/master/pythonlib/amlrealtimeai/client.py)
- [C#](https://github.com/Azure/aml-real-time-ai/blob/master/sample-clients/csharp)

Si quiere usar TensorFlow Serving, puede [descargar un cliente de ejemplo](https://www.tensorflow.org/serving/setup).

```python
# Using the grpc client in Azure ML Accelerated Models SDK package
from azureml.accel import PredictionClient

address = aks_service.scoring_uri
ssl_enabled = address.startswith("https")
address = address[address.find('/')+2:].strip('/')
port = 443 if ssl_enabled else 80

# Initialize AzureML Accelerated Models client
client = PredictionClient(address=address,
                          port=port,
                          use_ssl=ssl_enabled,
                          service_name=aks_service.name)
```

Puesto que este clasificador se ha entrenado en el conjunto de datos [ImageNet](http://www.image-net.org/), asigne las clases a etiquetas de lenguaje natural.

```python
import requests
classes_entries = requests.get("https://raw.githubusercontent.com/Lasagne/Recipes/master/examples/resnet50/imagenet_classes.txt").text.splitlines()

# Score image with input and output tensor names
results = client.score_file(path="./snowleopardgaze.jpg", 
                             input_name=input_tensors, 
                             outputs=output_tensors)

# map results [class_id] => [confidence]
results = enumerate(results)
# sort results by confidence
sorted_results = sorted(results, key=lambda x: x[1], reverse=True)
# print top 5 results
for top in sorted_results[:5]:
    print(classes_entries[top[0]], 'confidence:', top[1])
```

### <a name="clean-up-the-service"></a>Limpieza del servicio
Elimine el servicio web, la imagen y el modelo (se debe realizar en este orden, ya que existen dependencias).

```python
aks_service.delete()
aks_target.delete()
image.delete()
registered_model.delete()
converted_model.delete()
```

## <a name="deploy-to-a-local-edge-server"></a>Implementación en un servidor perimetral local

Todos los [dispositivos perimetrales de Azure Data Box Edge](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview
) contienen una FPGA para ejecutar el modelo.  Solo se puede ejecutar un único modelo en la FPGA al mismo tiempo.  Para ejecutar otro modelo, basta con implementar un nuevo contenedor. Pueden encontrarse instrucciones y código de ejemplo en [este ejemplo de Azure](https://github.com/Azure-Samples/aml-hardware-accelerated-models).

## <a name="secure-fpga-web-services"></a>Protección de los servicios web FPGA

Para obtener información sobre cómo proteger los servicios web FPGA, consulte el documento [Protección de servicios web](how-to-secure-web-service.md).

## <a name="pbs-family-vms"></a>Máquinas virtuales de la familia PBS

La familia PBS de máquinas virtuales de Azure contiene matrices FPGA Intel Arria 10.  Esto se mostrará como "Standard PBS Family vCPUs" (vCPU de la familia PBS estándar) al revisar la asignación de cuota de Azure.  La máquina virtual PB6 tiene seis vCPU y una FPGA, y Azure ML la aprovisionará automáticamente como parte de la implementación de un modelo en una FPGA.  Solo se usa con Azure ML y no puede ejecutar secuencias de bits arbitrarias.  Por ejemplo, no podrá incorporar a la FPGA secuencias de bits para realizar cifrado, codificación, etcétera. 
