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
ms.openlocfilehash: cfe21d2119b92665c5950d792dec6500257c6316
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65024180"
---
# <a name="deploy-a-model-as-a-web-service-on-an-fpga-with-azure-machine-learning-service"></a>Implementación de un modelo como servicio web en una FPGA con Azure Machine Learning Service

Puede implementar un modelo como un servicio web en [(FPGA) de matrices de puertas programables por campos](concept-accelerate-with-fpgas.md) con modelos de acelerados de Hardware de Azure Machine Learning. El uso de las FPGA brinda una inferencia de latencia muy baja, incluso con un tamaño de lote único.

Actualmente están disponibles los siguientes modelos:
  - ResNet 50
  - ResNet 152
  - DenseNet-121
  - VGG-16
  - SSD-VGG

Las FPGA están disponibles en estas regiones de Azure:
  - Este de EE. UU
  - Oeste de EE. UU. 2
  - Europa occidental
  - Sudeste asiático

> [!IMPORTANT]
> Para optimizar el rendimiento y la latencia, el cliente envía datos al modelo de FPGA debe estar en una de las regiones encima (la que implementó el modelo a).

## <a name="prerequisites"></a>Requisitos previos

- Si no tiene una suscripción a Azure, cree una cuenta gratuita antes de empezar. Pruebe hoy mismo la [versión gratuita o de pago de Azure Machine Learning Service](https://aka.ms/AMLFree).

- Un área de trabajo del servicio Azure Machine Learning y el SDK de Azure Machine Learning para Python instalado. Descubra cómo obtener estos requisitos previos con el documento [How to configure a development environment](how-to-configure-environment.md) (Cómo configurar un entorno de desarrollo).
 
  - Instale el SDK de Python para los modelos acelerados de hardware:

    ```shell
    pip install --upgrade azureml-accel-models
    ```

## <a name="sample-notebooks"></a>Cuadernos de ejemplo

Para su comodidad, [cuadernos de ejemplo](https://aka.ms/aml-notebooks) están disponibles para el ejemplo a continuación y, además de otros ejemplos.  Busque en carpetas how-to-use-azureml y la implementación de modelos acelerados.

## <a name="create-and-containerize-your-model"></a>Crear e incluir el modelo

Este documento se describe cómo crear un gráfico de TensorFlow preprocesar la imagen de entrada, asegúrese de un caracterizador con ResNet 50 en una FPGA y, a continuación, ejecute las características a través de un clasificador entrenado en el conjunto de datos ImageNet.

Siga las instrucciones para:

* Definir el modelo de TensorFlow
* Implementación del modelo
* Consumo del modelo implementado
* Eliminación de los servicios implementados

### <a name="load-azure-ml-workspace"></a>Cargar área de trabajo de aprendizaje automático de Azure

Cargar el área de trabajo de aprendizaje automático de Azure.

```python
import os
import tensorflow as tf
 
from azureml.core import Workspace
 
ws = Workspace.from_config()
print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')
```

### <a name="preprocess-image"></a>Imagen de preprocesamiento

La entrada al servicio web es una imagen JPEG.  El primer paso es descodificar la imagen JPEG y preprocesarlo.  Las imágenes JPEG se tratan como cadenas y el resultado son tensors que serán la entrada para el modelo de ResNet 50.

```python
# Input images as a two-dimensional tensor containing an arbitrary number of images represented a strings
import azureml.accel.models.utils as utils
in_images = tf.placeholder(tf.string)
image_tensors = utils.preprocess_array(in_images)
print(image_tensors.shape)
```

### <a name="load-featurizer"></a>Caracterizador de carga

Inicialice el modelo y descargue un punto de control de TensorFlow de la versión cuantificada de ResNet50 que se usará como caracterizador.  Puede reemplazar "QuantizedResnet50" en el fragmento de código siguiente con importando otras redes neurales profundas:

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

### <a name="add-classifier"></a>Agregar clasificador

Este clasificador se ha entrenado en el conjunto de datos de ImageNet.  Ejemplos para la transferencia de aprendizaje y entrenamiento de los pesos personalizados están disponibles en el conjunto de [cuadernos de ejemplo](https://aka.ms/aml-notebooks).

```python
classifier_output = model_graph.get_default_classifier(feature_tensor)
print(classifier_output)
```

### <a name="save-the-model"></a>Guardar el modelo

Ahora que se han cargado el clasificador, el preprocesador y caracterizador de ResNet 50, guarde el gráfico y las variables asociadas como un modelo.

```python
model_name = "resnet50"
model_def_path = os.path.join(save_path, model_name)
print("Saving model in {}".format(model_def_path))

with tf.Session() as sess:
    model_graph.restore_weights(sess)
    tf.saved_model.simple_save(sess, model_def_path,
                                   inputs={'images': in_images},
                                   outputs={'output_alias': classifier_output})
```

### <a name="register-model"></a>Registro del modelo

[Registrar](./concept-model-management-and-deployment.md) el modelo que ha creado.  Agregar etiquetas y otros metadatos sobre el modelo le ayuda a realizar un seguimiento de los modelos entrenados.

```python
from azureml.core.model import Model

registered_model = Model.register(workspace = ws
                                  model_path = model_def_path,
                                  model_name = model_name)

print("Successfully registered: ", registered_model.name, registered_model.description, registered_model.version, sep = '\t')
```

Si ya se ha registrado un modelo y desea realizar la carga, puede recuperarlo.

```python
from azureml.core.model import Model
model_name = "resnet50"
# By default, the latest version is retrieved. You can specify the version, i.e. version=1
registered_model = Model(ws, name="resnet50")
print(registered_model.name, registered_model.description, registered_model.version, sep = '\t')
```

### <a name="convert-model"></a>Convertir a modelo

El gráfico de TensorFlow debe convertirse al formato de intercambio de red neuronal abierta ([ONNX](https://onnx.ai/)).  Deberá proporcionar los nombres de la entrada y salidas tensors y estos nombres se utilizarán por parte del cliente al consumir el servicio web.

```python
input_tensor = in_images.name
output_tensors = classifier_output.name

print(input_tensor)
print(output_tensors)


from azureml.accel.accel_onnx_converter import AccelOnnxConverter

convert_request = AccelOnnxConverter.convert_tf_model(ws, registered_model, input_tensor, output_tensors)
convert_request.wait_for_completion(show_output=True)

# If the above call succeeded, get the converted model
converted_model = convert_request.result
print(converted_model.name, converted_model.url, converted_model.version, converted_model.id,converted_model.created_time)
```

### <a name="create-docker-image"></a>Crear imagen de Docker

El modelo convertido y todas las dependencias se agregan a una imagen de Docker.  Esta imagen de Docker, a continuación, se puede implementar y crea una instancia en la nube o un dispositivo perimetral compatibles como [borde del cuadro de datos de Azure](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview).  También puede agregar etiquetas y descripciones para la imagen de Docker registrada.

```python
from azureml.core.image import Image
from azureml.accel.accel_container_image import AccelContainerImage

image_config = AccelContainerImage.image_configuration()
image_name = "{}-image".format(model_name)

image = Image.create(name = image_name,
                     models = [converted_model],
                     image_config = image_config, 
                     workspace = ws)


image.wait_for_creation(show_output = True)
```

Enumerar las imágenes por etiqueta y obtener los registros detallados para cualquier tipo de depuración.

```python
for i in Image.list(workspace = ws):
    print('{}(v.{} [{}]) stored at {} with build log {}'.format(i.name, i.version, i.creation_state, i.image_location, i.image_build_log_uri))
```

## <a name="model-deployment"></a>Implementación del modelo

### <a name="deploy-to-the-cloud"></a>Implementar en la nube

Para implementar el modelo como un servicio web de producción a gran escala, use Azure Kubernetes Service (AKS). Puede crear uno nuevo con el SDK de Azure Machine Learning, la CLI o el portal de Azure.

```python
# Use the default configuration (can also provide parameters to customize)
prov_config = AksCompute.provisioning_configuration()

aks_name = 'my-aks-9' 
# Create the cluster
aks_target = ComputeTarget.create(workspace = ws, 
                                  name = aks_name, 
                                  provisioning_configuration = prov_config)

%%time
aks_target.wait_for_completion(show_output = True)
print(aks_target.provisioning_state)
print(aks_target.provisioning_errors)

#Set the web service configuration (using default here)
aks_config = AksWebservice.deploy_configuration()

%%time
aks_service_name ='aks-service-1'

aks_service = Webservice.deploy_from_image(workspace = ws, 
                                           name = aks_service_name,
                                           image = image,
                                           deployment_config = aks_config,
                                           deployment_target = aks_target)
aks_service.wait_for_deployment(show_output = True)
print(aks_service.state)
print(aks_service.scoring_uri)
```

#### <a name="test-the-cloud-service"></a>Probar el servicio en la nube
Es compatible con la imagen de Docker gRPC y TensorFlow que actúa "predecir" API.  Use el cliente de ejemplo para llamar a la imagen de Docker para obtener las predicciones del modelo.  Código de cliente de ejemplo está disponible:
- [Python](https://github.com/Azure/aml-real-time-ai/blob/master/pythonlib/amlrealtimeai/client.py)
- [C#](https://github.com/Azure/aml-real-time-ai/blob/master/sample-clients/csharp)

Si desea usar servir TensorFlow, puede [descargar un cliente de ejemplo](https://www.tensorflow.org/serving/setup).

```python
import requests
classes_entries = requests.get("https://raw.githubusercontent.com/Lasagne/Recipes/master/examples/resnet50/imagenet_classes.txt").text.splitlines()

# Score image using input and output tensor names
results = client.score_file(path="./snowleopardgaze.jpg", 
                             input_name=input_tensor, 
                             outputs=output_tensors)

# map results [class_id] => [confidence]
results = enumerate(results)
# sort results by confidence
sorted_results = sorted(results, key=lambda x: x[1], reverse=True)
# print top 5 results
for top in sorted_results[:5]:
    print(classes_entries[top[0]], 'confidence:', top[1])
```

### <a name="clean-up-the-service"></a>El servicio de limpieza
Eliminar el servicio web, la imagen y el modelo (se debe realizar en este orden ya que existen dependencias).

```python
aks_service.delete()
image.delete()
registered_model.delete()
converted_model.delete()
```

## <a name="deploy-to-a-local-edge-server"></a>Implementar en un servidor perimetral local

Todos los [dispositivos perimetrales de cuadro de datos de Azure](https://docs.microsoft.com/azure/databox-online/data-box-edge-overview
) contienen una FPGA para ejecutar el modelo.  Un único modelo puede ejecutarse en el FPGA al mismo tiempo.  Para ejecutar un modelo diferente, basta con implementar un nuevo contenedor. Instrucciones y código de ejemplo pueden encontrarse en [este ejemplo Azure](https://github.com/Azure-Samples/aml-hardware-accelerated-models).

## <a name="secure-fpga-web-services"></a>Protección de los servicios web FPGA

Para obtener información sobre cómo proteger los servicios web FPGA, consulte el documento [Protección de servicios web](how-to-secure-web-service.md).