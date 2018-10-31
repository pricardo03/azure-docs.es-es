---
title: ONNX y Azure Machine Learning | Creación e implementación de modelos
description: Información sobre ONNX y cómo usar Azure Machine Learning para crear e implementar modelos de ONNX
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: prasantp
author: prasanthpul
ms.date: 09/24/2018
ms.openlocfilehash: 190b7fff24c9d6b3dee86471b56ad68c962e51ce
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "49116885"
---
# <a name="onnx-and-azure-machine-learning-create-and-deploy-interoperable-ai-models"></a>ONNX y Azure Machine Learning: creación e implementación de modelos de inteligencia artificial interoperables

El formato [Open Neural Network Exchange](http://onnx.ai) (ONNX) es un estándar abierto para representar modelos de aprendizaje automático. ONNX tiene el apoyo de una [comunidad de asociados](http://onnx.ai/supported-tools), incluido Microsoft, que crean herramientas y marcos compatibles. Microsoft se compromete con una inteligencia artificial abierta e interoperable para que los desarrolladores y científicos de datos puedan:

+ Usar el marco que quieran para crear y entrenar modelos.
+ Implementar modelos multiplataforma con el mínimo trabajo de integración.

Microsoft admite ONNX en sus productos, incluidos Azure y Windows, para ayudarle a lograr estos objetivos.  

## <a name="why-choose-onnx"></a>¿Por qué elegir ONNX?
La interoperabilidad que obtiene con ONNX le permite poner grandes ideas en producción con mayor rapidez. Con ONNX, los científicos de datos pueden elegir su marco preferido para el trabajo. De forma similar, los desarrolladores pueden dedicar menos tiempo a preparar los modelos para producción e implementar en la nube y el perímetro.  

Puede crear modelos de ONNX desde muchos marcos, incluidos PyTorch, Chainer, Microsoft Cognitive Toolkit (CNTK), MXNet, ML.Net, TensorFlow, Keras y SciKit-Learn, entre otros.

También hay un ecosistema de herramientas para visualizar y acelerar modelos de ONNX. Además, hay disponibles varios modelos de ONNX previamente entrenados para escenarios comunes.

[Se pueden implementar modelos de ONNX](#deploy) en la nube con Azure Machine Learning y el entorno de ejecución de ONNX. También se pueden implementar en dispositivos Windows 10 mediante [Windows ML](https://docs.microsoft.com/windows/ai/). Incluso se pueden implementar en otras plataformas mediante convertidores que están disponibles en la comunidad de ONNX. 

[ ![Diagrama de flujo de ONNX que muestra el aprendizaje, los convertidores y la implementación](media/concept-onnx/onnx.png) ] (./media/concept-onnx/onnx.png#lightbox)

## <a name="get-onnx-models"></a>Obtención de modelos de ONNX

Puede obtener modelos de ONNX de varias maneras:
+ Obtenga un modelo previamente entrenado de ONNX desde [ONNX Model Zoo](https://github.com/onnx/models) (consulte el ejemplo al final de este artículo)
+ Genere un modelo de ONNX personalizado desde [Azure Custom Vision Service](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/). 
+ Convierta un modelo existente de otro formato a ONNX (consulte el ejemplo al final de este artículo) 
+ Entrene un nuevo modelo de ONNX en Azure Machine Learning Service (consulte el ejemplo al final de este artículo)

## <a name="saveconvert-your-models-to-onnx"></a>Guardado o conversión de modelos de ONNX

Puede convertir los modelos existentes a ONNX o guardarlos como ONNX al final del entrenamiento.

|Plataforma para el modelo|Ejemplo o herramienta de conversión|
|-----|-------|
|PyTorch|[Jupyter Notebook](https://github.com/onnx/tutorials/blob/master/tutorials/PytorchOnnxExport.ipynb)|
|Microsoft&nbsp;Cognitive&nbsp;Toolkit&nbsp;(CNTK)|[Jupyter Notebook](https://github.com/onnx/tutorials/blob/master/tutorials/CntkOnnxExport.ipynb)|
|TensorFlow|[tensorflow-onnx converter](https://github.com/onnx/tensorflow-onnx)|
|Chainer|[Jupyter Notebook](https://github.com/onnx/tutorials/blob/master/tutorials/ChainerOnnxExport.ipynb)|
|MXNet|[Jupyter Notebook](https://github.com/onnx/tutorials/blob/master/tutorials/MXNetONNXExport.ipynb)|
|Keras, ScitKit-Learn, CoreML<br/>XGBoost y libSVM|[WinMLTools](https://docs.microsoft.com/windows/ai/convert-model-winmltools)|

Puede encontrar la lista más reciente de marcos y convertidores admitidos en el [sitio de tutoriales de ONNX](https://github.com/onnx/tutorials).

<a name="deploy"></a>

## <a name="deploy-onnx-models-in-azure"></a>Implementación de modelos de ONNX en Azure

Con el servicio Azure Machine Learning, puede implementar, administrar y supervisar sus modelos de ONNX. Mediante el [flujo de trabajo de implementación](concept-model-management-and-deployment.md) estándar y el entorno de ejecución de ONNX, puede crear un punto de conexión REST hospedado en la nube. Vea un ejemplo completo de un cuaderno de Jupyter Notebook al final de este artículo para probarlo usted mismo. 

### <a name="install-and-configure-the-onnx-runtime"></a>Instalación y configuración del entorno de ejecución de ONNX

El entorno de ejecución de ONNX es un motor de inferencia de alto rendimiento para los modelos de ONNX. Incluye una API de Python y proporciona aceleración de hardware en la CPU y la GPU. Actualmente admite modelos de ONNX 1.2 y se ejecuta en Ubuntu 16.04 Linux. Ambos paquetes [CPU](https://pypi.org/project/onnxruntime) y [GPU](https://pypi.org/project/onnxruntime-gpu) están disponibles en [PyPi.org](https://pypi.org).

Para instalar el entorno de ejecución de ONNX, use:
```python
pip install onnxruntime
```

Para llamar al entorno de ejecución de ONNX en el script de Python, use:
```python
import onnxruntime

session = onnxruntime.InferenceSession("path to model")
```

La documentación que acompaña al modelo suele indicar las entradas y salidas para usar el modelo. También puede usar una herramienta de visualización como [Netron](https://github.com/lutzroeder/Netron) para ver el modelo. El entorno de ejecución de ONNX también le permite consultar los metadatos, las entradas y las salidas del modelo:
```python
session.get_modelmeta()
first_input_name = session.get_inputs()[0].name
first_output_name = session.get_outputs()[0].name
```

Para realizar la inferencia de su modelo, use `run` y pase la lista de salidas que quiera que se devuelvan (déjelo vacío si quiere todas) y un mapa de los valores de entrada. El resultado es una lista de las salidas.
```python
results = session.run(["output1", "output2"], {"input1": indata1, "input2": indata2})
results = session.run([], {"input1": indata1, "input2": indata2})
```

Para obtener una referencia completa de la API, consulte [los documentos de referencia del entorno de ejecución de ONNX](https://aka.ms/onnxruntime-python).

### <a name="example-deployment-steps"></a>Pasos de implementación de ejemplo

Aquí tiene un ejemplo para implementar un modelo de ONNX:

1. Inicialice el área de trabajo de Azure Machine Learning Service. Si aún no tiene uno, obtenga información sobre cómo crear un área de trabajo en [este inicio rápido](quickstart-get-started.md).

   ```python
   from azureml.core import Workspace

   ws = Workspace.from_config()
   print(ws.name, ws.resource_group, ws.location, ws.subscription_id, sep = '\n')
   ```

2. Registre el modelo con Azure Machine Learning.

   ```python
   from azureml.core.model import Model

   model = Model.register(model_path = "model.onnx",
                          model_name = "MyONNXmodel",
                          tags = ["onnx"],
                          description = "test",
                          workspace = ws)
   ```

3. Cree una imagen con el modelo y las dependencias.

   ```python
   from azureml.core.image import ContainerImage
   
   image_config = ContainerImage.image_configuration(execution_script = "score.py",
                                                     runtime = "python",
                                                     conda_file = "myenv.yml",
                                                     description = "test",
                                                     tags = ["onnx"]
                                                    )

   image = ContainerImage.create(name = "myonnxmodelimage",
                                 # this is the model object
                                 models = [model],
                                 image_config = image_config,
                                 workspace = ws)

   image.wait_for_creation(show_output = True)
   ```

   El archivo `score.py` contiene la lógica de puntuación y debe incluirse en la imagen. Este archivo se usa para ejecutar el modelo en la imagen. Vea este [tutorial](tutorial-deploy-models-with-aml.md#create-scoring-script) para obtener instrucciones sobre cómo crear un script de puntuación. A continuación se muestra un archivo de ejemplo de un modelo de ONNX:

   ```python
   import onnxruntime
   import json
   import numpy as np
   import sys
   from azureml.core.model import Model

   def init():
       global model_path
       model_path = Model.get_model_path(model_name = 'MyONNXmodel')

   def run(raw_data):
       try:
           data = json.loads(raw_data)['data']
           data = np.array(data)
        
           sess = onnxruntime.InferenceSession(model_path)
           result = sess.run(["outY"], {"inX": data})
        
           return json.dumps({"result": result.tolist()})
       except Exception as e:
           result = str(e)
           return json.dumps({"error": result})
   ```

   El archivo `myenv.yml` describe las dependencias necesarias para la imagen. Vea este [tutorial](tutorial-deploy-models-with-aml.md#create-environment-file) para obtener instrucciones sobre cómo crear un archivo de entorno, como este archivo de ejemplo:

   ```python
   from azureml.core.conda_dependencies import CondaDependencies 

   myenv = CondaDependencies()
   myenv.add_pip_package("numpy")
   myenv.add_pip_package("azureml-core")
   myenv.add_pip_package("onnxruntime")

   with open("myenv.yml","w") as f:
    f.write(myenv.serialize_to_string())
   ```

4. Implemente su modelo de ONNX con Azure Machine Learning en:
   + Azure Container Instances (ACI): [más información](how-to-deploy-to-aci.md)

   + Azure Kubernetes Service (AKS): [más información](how-to-deploy-to-aks.md)


## <a name="examples"></a>Ejemplos
 
En los siguientes cuadernos se muestra cómo crear modelos de ONNX e implementarlos con Azure Machine Learning: 
+ `/onnx/onnx-modelzoo-aml-deploy-resnet50.ipynb` 
+ `/onnx/onnx-convert-aml-deploy-tinyyolo.ipynb`
+ `/onnx/onnx-train-pytorch-aml-deploy-mnist.ipynb`

En los siguientes cuadernos se muestra cómo implementar modelos de ONNX existentes con Azure Machine Learning: 
+ [onnx/onnx-inference-mnist.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/onnx/onnx-inference-mnist.ipynb) 
+ [onnx/onnx-inference-emotion-recognition.ipynb](https://github.com/Azure/MachineLearningNotebooks/blob/master/onnx/onnx-inference-emotion-recognition.ipynb)
 
Obtenga estos cuadernos:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="more-info"></a>Más información

Obtenga más información sobre ONNX o contribuya al proyecto:
+ [Sitio web del proyecto de ONNX](http://onnx.ai)

+ [Código de ONNX en GitHub](https://github.com/onnx/onnx)
