---
title: Creación e implementación de modelos de ONNX interoperables
titleSuffix: Azure Machine Learning service
description: Información sobre ONNX y cómo usar Azure Machine Learning para crear e implementar modelos de ONNX
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: prasantp
author: prasanthpul
ms.date: 12/3/2018
ms.custom: seodec18
ms.openlocfilehash: 33a93aa01499beb978f616f633588ba75e4b62a3
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59259191"
---
# <a name="onnx-and-azure-machine-learning-create-and-deploy-interoperable-ai-models"></a>ONNX y Azure Machine Learning: creación e implementación de modelos de IA interoperables

El formato [Open Neural Network Exchange](https://onnx.ai) (ONNX) es un estándar abierto para representar modelos de aprendizaje automático. ONNX tiene el apoyo de una [comunidad de asociados](https://onnx.ai/supported-tools), incluido Microsoft, que crean herramientas y marcos compatibles. Microsoft se compromete con una inteligencia artificial abierta e interoperable para que los desarrolladores y científicos de datos puedan:

+ Usar el marco que quieran para crear y entrenar modelos.
+ Implementar modelos multiplataforma con el mínimo trabajo de integración.

Microsoft admite ONNX en sus productos, incluidos Azure y Windows, para ayudarle a lograr estos objetivos.  

## <a name="why-choose-onnx"></a>¿Por qué elegir ONNX?

La interoperabilidad que obtiene con ONNX le permite poner grandes ideas en producción con mayor rapidez. Con ONNX, los científicos de datos pueden elegir su marco preferido para el trabajo. De forma similar, los desarrolladores pueden dedicar menos tiempo a preparar los modelos para producción e implementar en la nube y el perímetro.  

Puede crear modelos ONNX desde muchos marcos, incluidos PyTorch, Chainer, MXNet, ML.Net, TensorFlow, Keras, SciKit-Learn, Microsoft Cognitive Toolkit y más.

También hay un ecosistema de herramientas para visualizar y acelerar modelos de ONNX. Además, hay disponibles varios modelos de ONNX previamente entrenados para escenarios comunes.

[Se pueden implementar modelos de ONNX](#deploy) en la nube con Azure Machine Learning y ONNX Runtime. También se pueden implementar en dispositivos Windows 10 mediante [Windows ML](https://docs.microsoft.com/windows/ai/). Incluso se pueden implementar en otras plataformas mediante convertidores que están disponibles en la comunidad de ONNX. 

[![Diagrama de flujo ONNX que muestra el entrenamiento, los convertidores de tipos y la implementación](media/concept-onnx/onnx.png)](./media/concept-onnx/onnx.png#lightbox)

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
|TensorFlow|[tensorflow-onnx converter](https://github.com/onnx/tensorflow-onnx)|
|Chainer|[Jupyter Notebook](https://github.com/onnx/tutorials/blob/master/tutorials/ChainerOnnxExport.ipynb)|
|MXNet|[Jupyter Notebook](https://github.com/onnx/tutorials/blob/master/tutorials/MXNetONNXExport.ipynb)|
|Keras, ScitKit-Learn, CoreML<br/>XGBoost y libSVM|[WinMLTools](https://docs.microsoft.com/windows/ai/convert-model-winmltools)|
|Microsoft&nbsp;Cognitive&nbsp;Kit de herramientas|[Jupyter Notebook](https://github.com/onnx/tutorials/blob/master/tutorials/CntkOnnxExport.ipynb)|

Puede encontrar la lista más reciente de marcos y convertidores admitidos en el [sitio de tutoriales de ONNX](https://github.com/onnx/tutorials).

<a name="deploy"></a>

## <a name="deploy-onnx-models-in-azure"></a>Implementación de modelos de ONNX en Azure

Con el servicio Azure Machine Learning, puede implementar, administrar y supervisar sus modelos de ONNX. Mediante el [flujo de trabajo de implementación](concept-model-management-and-deployment.md) estándar y ONNX Runtime, puede crear un punto de conexión REST hospedado en la nube. Vea un ejemplo completo de un cuaderno de Jupyter Notebook al final de este artículo para probarlo usted mismo. 

### <a name="install-and-configure-onnx-runtime"></a>Instalación y configuración de ONNX Runtime

ONNX Runtime es un motor de inferencia de alto rendimiento de código abierto para los modelos de ONNX. Proporciona aceleración de hardware de CPU y GPU, con API disponibles para Python, C# y C. ONNX Runtime admite modelos ONNX 1.2 + y se ejecuta en Linux, Windows y Mac. Los paquetes de Python están disponibles en [PyPi.org](https://pypi.org) ([CPU](https://pypi.org/project/onnxruntime), [GPU](https://pypi.org/project/onnxruntime-gpu)), y el [paquete de C#](https://www.nuget.org/packages/Microsoft.ML.OnnxRuntime/) en [Nuget.org](https://www.nuget.org). Obtenga más información sobre el proyecto en [GitHub](https://github.com/Microsoft/onnxruntime). Lea [requisitos del sistema](https://github.com/Microsoft/onnxruntime#system-requirements) antes de la instalación.

Para instalar ONNX Runtime para Python, use lo siguiente:
```python
pip install onnxruntime
```

Para llamar a ONNX Runtime en el script de Python, use lo siguiente:
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

Para obtener la referencia completa de API de Python, vea la [documentación de referencia de ONNX Runtime](https://aka.ms/onnxruntime-python).

### <a name="example-deployment-steps"></a>Pasos de implementación de ejemplo

Aquí tiene un ejemplo para implementar un modelo de ONNX:

1. Inicialice el área de trabajo de Azure Machine Learning Service. Si aún no tiene uno, aprenda cómo [crear un área de trabajo](setup-create-workspace.md).

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

   > [!TIP]
   > El ejemplo anterior utiliza la imagen predeterminada proporcionada por el servicio de Azure Machine Learning. También puede usar una imagen personalizada. Para más información, consulte la sección de configuración y registro de imágenes de [Implementación de modelos](how-to-deploy-and-where.md#configureimage).

   El archivo `score.py` contiene la lógica de puntuación y debe incluirse en la imagen. Este archivo se usa para ejecutar el modelo en la imagen. Vea este [tutorial](tutorial-deploy-models-with-aml.md#create-scoring-script) para obtener instrucciones sobre cómo crear un script de puntuación. A continuación se muestra un archivo de ejemplo de un modelo de ONNX:

   ```python
   import onnxruntime
   import json
   import numpy as np
   import sys
   from azureml.core.model import Model

   def init():
       global session
       model = Model.get_model_path(model_name = 'MyONNXModel')
       session = onnxruntime.InferenceSession(model)

   def preprocess(input_data_json):
       # convert the JSON data into the tensor input
       return np.array(json.loads(input_data_json)['data']).astype('float32')

   def postprocess(result):
       return np.array(result).tolist()

   def run(input_data_json):
       try:
           start = time.time()   # start timer
           input_data = preprocess(input_data_json)
           input_name = session.get_inputs()[0].name  # get the id of the first input of the model   
           result = session.run([], {input_name: input_data})
           end = time.time()     # stop timer
           return {"result": postprocess(result),
                   "time": end - start}
       except Exception as e:
           result = str(e)
           return {"error": result}
   ```

   El archivo `myenv.yml` describe las dependencias necesarias para la imagen. Vea este [tutorial](tutorial-deploy-models-with-aml.md#create-environment-file) para obtener instrucciones sobre cómo crear un archivo de entorno, como este archivo de ejemplo:

   ```python
   from azureml.core.conda_dependencies import CondaDependencies 

   myenv = CondaDependencies.create(pip_packages=["numpy","onnxruntime","azureml-core"])

   with open("myenv.yml","w") as f:
    f.write(myenv.serialize_to_string())
   ```

4. Para implementar el modelo, vea el documento [Cómo y dónde implementar](how-to-deploy-and-where.md).


## <a name="examples"></a>Ejemplos

Vea [how-to-use-azureml/deployment/onnx](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx) para obtener cuadernos de ejemplo en los que se crean e implementan modelos de ONNX.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="more-info"></a>Más información

Obtenga más información sobre ONNX o contribuya al proyecto:
+ [Sitio web del proyecto de ONNX](https://onnx.ai)

+ [Código de ONNX en GitHub](https://github.com/onnx/onnx)

Obtenga más información sobre ONNX Runtime o contribuya al proyecto:
+ [Repositorio de ONNX Runtime en GitHub](https://github.com/Microsoft/onnxruntime)


