---
title: Alto rendimiento, entre la inferencia de plataforma con ONNX
titleSuffix: Azure Machine Learning service
description: Obtenga información sobre ONNX y el tiempo de ejecución ONNX para acelerar los modelos
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: prasantp
author: prasanthpul
ms.date: 04/24/2019
ms.custom: seodec18
ms.openlocfilehash: 6bda1af8095e7026c79b7d8ffe45b39f6cbb3508
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028701"
---
# <a name="onnx-and-azure-machine-learning-create-and-accelerate-ml-models"></a>ONNX y Azure Machine Learning: Crear y acelerar los modelos de aprendizaje automático

Obtenga información sobre cómo usar el [intercambio de red neuronal abierta](https://onnx.ai) (ONNX) puede ayudar a optimizar los modelos de aprendizaje automático.

Optimizar modelos de machine learning para la inferencia es difícil, ya que necesita optimizar el modelo y la biblioteca de inferencia para aprovechar al máximo las capacidades de hardware. El problema se vuelve extremadamente difícil si desea obtener un rendimiento óptimo en diferentes tipos de plataformas (en la nube o edge, CPU/GPU, etc.), puesto que cada uno tenga características y capacidades diferentes. La complejidad aumenta si tiene modelos desde una variedad de marcos de trabajo que se debe ejecutar en una variedad de plataformas. Es muy lento para optimizar las distintas combinaciones de marcos de trabajo y hardware. Se necesita una solución para entrenar una vez en su marco preferido y ejecutar en cualquier lugar en la nube o perimetral. Esto es donde entra en juego ONNX.

Microsoft y una comunidad de asociados crean ONNX como un estándar abierto para representar modelos de aprendizaje automático. Modelos de [muchos marcos](https://onnx.ai/supported-tools) incluidos TensorFlow, PyTorch, SciKit-Learn, Keras, Chainer, MXNet y MATLAB pueden exportar o convertir al formato ONNX estándar. Una vez que los modelos tienen el formato ONNX, se puede ejecutar en una variedad de plataformas y dispositivos.

[En tiempo de ejecución ONNX](https://github.com/Microsoft/onnxruntime) es un motor de inferencia de alto rendimiento para la implementación de modelos ONNX a producción. Se está optimizado para la nube y perimetrales y funciona en Linux, Windows y Mac. Escrito en C++, también tiene C, Python, y C# API. En tiempo de ejecución ONNX proporciona soporte técnico para todos los de la especificación ONNX ML y también se integra con los aceleradores en un hardware diferente como TensorRT en GPU de NVidia.

El tiempo de ejecución ONNX se usa en servicios de Microsoft de gran escala, como Bing, Office y Cognitive Services. Mejoras de rendimiento dependen de una serie de factores, pero estos servicios de Microsoft han visto un __promedio de 2 x ganancia de rendimiento de CPU__. También se utiliza en tiempo de ejecución ONNX como parte del aprendizaje automático de Windows en cientos de millones de dispositivos. Puede usar el tiempo de ejecución con los servicios de Azure Machine Learning. Mediante el uso de ONNX en tiempo de ejecución, puede beneficiarse de las optimizaciones amplias de producción, mejoras de pruebas y en curso.

[![Diagrama de flujo ONNX que muestra el entrenamiento, los convertidores de tipos y la implementación](media/concept-onnx/onnx.png) ](./media/concept-onnx/onnx.png#lightbox)

## <a name="get-onnx-models"></a>Obtención de modelos de ONNX

Puede obtener modelos de ONNX de varias maneras:
+ Entrenar un modelo ONNX nuevo en el servicio de Azure Machine Learning (vea los ejemplos en la parte inferior de este artículo)
+ Convertir el modelo existente de otro formato a ONNX (consulte la [tutoriales](https://github.com/onnx/tutorials)) 
+ Obtener un modelo previamente entrenado de ONNX desde el [Zoo de modelo ONNX](https://github.com/onnx/models) (vea los ejemplos en la parte inferior de este artículo)
+ Genere un modelo de ONNX personalizado desde [Azure Custom Vision Service](https://docs.microsoft.com/azure/cognitive-services/Custom-Vision-Service/). 

Muchos modelos, incluida la clasificación de imágenes, detección de objetos y el procesamiento de texto pueden representarse como modelos ONNX. Sin embargo es posible que algunos modelos no pueda convertirse correctamente. Si experimenta esta situación, registre un problema en GitHub del convertidor correspondiente que ha usado. Puede seguir usando el modelo de formato existente hasta que se solucione el problema.

## <a name="deploy-onnx-models-in-azure"></a>Implementación de modelos de ONNX en Azure

Con el servicio Azure Machine Learning, puede implementar, administrar y supervisar sus modelos de ONNX. Mediante el [flujo de trabajo de implementación](concept-model-management-and-deployment.md) estándar y ONNX Runtime, puede crear un punto de conexión REST hospedado en la nube. Consulte cuadernos de Jupyter de ejemplo al final de este artículo para probarlo usted mismo. 

### <a name="install-and-use-onnx-runtime-with-python"></a>Instalar y usar en tiempo de ejecución ONNX con Python

Están disponibles en paquetes de Python para el tiempo de ejecución ONNX [PyPi.org](https://pypi.org) ([CPU](https://pypi.org/project/onnxruntime), [GPU](https://pypi.org/project/onnxruntime-gpu)). Lea [requisitos del sistema](https://github.com/Microsoft/onnxruntime#system-requirements) antes de la instalación. 

 Para instalar ONNX en tiempo de ejecución para Python, use uno de los siguientes comandos: 
```python   
pip install onnxruntime       # CPU build
pip install onnxruntime-gpu   # GPU build
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

## <a name="examples"></a>Ejemplos

Vea [how-to-use-azureml/deployment/onnx](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/deployment/onnx) para obtener cuadernos de ejemplo en los que se crean e implementan modelos de ONNX.

[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="more-info"></a>Más información

Obtenga más información sobre ONNX o contribuya al proyecto:
+ [Sitio web del proyecto de ONNX](https://onnx.ai)
+ [Código de ONNX en GitHub](https://github.com/onnx/onnx)

Obtenga más información sobre ONNX Runtime o contribuya al proyecto:
+ [Repositorio de ONNX Runtime en GitHub](https://github.com/Microsoft/onnxruntime)


