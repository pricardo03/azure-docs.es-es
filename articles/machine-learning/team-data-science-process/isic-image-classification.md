---
title: Clasificación de imágenes con Azure Machine Learning Package for Computer Vision y con el Proceso de ciencia de los datos en equipos (TDSP) | Microsoft Docs
description: Describe el uso del Proceso de ciencia de los datos en equipos (TDSP) y Azure Machine Learning Package for Computer Vision para la clasificación de imágenes.
services: machine-learning, team-data-science-process
documentationcenter: ''
author: xibingao
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2018
ms.author: xibingao
ms.openlocfilehash: f9e88cfb7185845e96f287b39bebaaa24320f537
ms.sourcegitcommit: ea5193f0729e85e2ddb11bb6d4516958510fd14c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2018
ms.locfileid: "36300814"
---
# <a name="skin-cancer-image-classification-with-the-azure-machine-learning-package-for-computer-vision-and-team-data-science-process"></a>Clasificación de imágenes de cáncer de piel con Azure Machine Learning Package for Computer Vision y con el Proceso de ciencia de los datos en equipos

En este artículo, se muestra cómo usar [Azure Machine Learning Package for Computer Vision (AMLPCV)](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest) para entrenar, probar e implementar un modelo de *clasificación de imágenes*. En el ejemplo se usan las plantillas y la estructura del Proceso de ciencia de los datos en equipos (TDSP) en [Azure Machine Learning Workbench](https://docs.microsoft.com/en-us/azure/machine-learning/service/quickstart-installation). El ejemplo completo se proporciona en este tutorial. Usa [Microsoft Cognitive Toolkit](https://www.microsoft.com/en-us/cognitive-toolkit/) como marco de aprendizaje profundo y el entrenamiento se realiza en una máquina GPU de [Data Science Virtual Machine ](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview). La implementación usa la CLI de operacionalización de Azure Machine Learning.

Muchas aplicaciones del dominio Computer Vision se pueden enmarcar como problemas de clasificación de imágenes. Entre ellas se incluyen modelos de creación que responden a preguntas como "¿Hay un objeto presente en la imagen?", donde el objeto puede ser un perro, un coche o un barco. También incluye respuestas a preguntas más complejas, como "¿Qué gravedad de enfermedad ocular revela el escáner de retina de este paciente?". Azure Machine Learning Package for Computer Vision simplifica el procesamiento de datos de clasificación de imágenes y la canalización de modelado. 

## <a name="link-to-the-github-repository"></a>Vínculo al repositorio de GitHub
Este artículo es un documento de resumen sobre el ejemplo. Puede encontrar documentación más extensa en el [sitio de GitHub](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification).

## <a name="team-data-science-process-walkthrough"></a>Tutorial del Proceso de ciencia de los datos en equipos

Este tutorial usa el ciclo de vida del [Proceso de ciencia de los datos en equipos (TDSP)](https://docs.microsoft.com/en-us/azure/machine-learning/team-data-science-process/overview). En el tutorial se describen los siguientes pasos del ciclo de vida.

### <a name="1-data-acquisitionhttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode01dataacquisitionandunderstanding"></a>[1. Adquisición de datos](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/01_data_acquisition_and_understanding)
El conjunto de datos de ISIC (siglas en inglés del proyecto de Colaboración internacional de imagen de la piel) se usa para la tarea de clasificación de imágenes. ISIC es una asociación entre las instituciones académicas y el sector que facilita la aplicación de imágenes digitales de la piel para estudiar la mortalidad por melanoma, así como para ayudar a reducirla. El [archivo de ISIC](https://isic-archive.com/#images) contiene más de 13 000 imágenes de lesiones cutáneas con etiquetas que indican si son benignas o malignas. Descargue un ejemplo de las imágenes del archivo de ISIC.

### <a name="2-modelinghttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode02modeling"></a>[2. Modelado](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/02_modeling)
En el paso de modelado, se llevan a cabo los subpasos siguientes.

#### <a name="dataset-creation"></a>Creación del conjunto de datos

Genere un objeto de conjunto de datos en Azure Machine Learning Package for Computer Vision. Para ello, proporcione un directorio raíz de imágenes en el disco local. 

#### <a name="image-visualization-and-annotation"></a>Visualización y anotación de imágenes

Visualice las imágenes del objeto de conjunto de datos y corrija las etiquetas según sea necesario.

#### <a name="image-augmentation"></a>Aumento de imágenes

Use las transformaciones que se describen en la biblioteca [imgaug](https://github.com/aleju/imgaug) para aumentar un objeto de conjunto de datos.

#### <a name="dnn-model-definition"></a>Definición del modelo de DNN

Defina la arquitectura del modelo que se utiliza en el paso de entrenamiento. Se admiten seis modelos de redes neuronales profundas entrenados previamente en Azure Machine Learning Package for Computer Vision: AlexNet, Resnet-18, Resnet-34, Resnet-50, Resnet-101 y Resnet-152.

#### <a name="classifier-training"></a>Entrenamiento de clasificadores

Entrene las redes neurales con parámetros predeterminados o personalizados.

#### <a name="evaluation-and-visualization"></a>Evaluación y visualización

Proporcione funcionalidad para evaluar el rendimiento del modelo entrenado en un conjunto de datos de prueba independiente. Las métricas de evaluación incluyen precisión y recuperación, así como la curva ROC.

Los subpasos se explican con detalle en la aplicación Jupyter Notebook correspondiente. El bloc de notas también proporciona instrucciones para activar los parámetros, como la velocidad de aprendizaje, el tamaño de lote mini y la tasa de retirada, para mejorar aún más el rendimiento del modelo.

### <a name="3-deploymenthttpsgithubcomazuremachinelearningsamples-amlvisionpackage-isicimageclassificationblobmastercode03deployment"></a>[3. Implementación](https://github.com/Azure/MachineLearningSamples-AMLVisionPackage-ISICImageClassification/blob/master/code/03_deployment)

En este paso, se pone en marcha el modelo generado en el paso de modelado. Presenta los requisitos previos y la configuración necesaria. También se explica el consumo del servicio web. En este tutorial, aprenderá a crear modelos de aprendizaje profundos con Azure Machine Learning Package for Computer Vision y a operacionalizar el modelo en Azure.

## <a name="next-steps"></a>Pasos siguientes
- Lea la documentación adicional sobre [Azure Machine Learning Package for Computer Vision](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest).
- Lea la documentación de [Proceso de ciencia de los datos en equipos](https://aka.ms/tdsp) para empezar.


## <a name="references"></a>Referencias

* [Azure Machine Learning Package for Computer Vision](https://docs.microsoft.com/en-us/python/api/overview/azure-machine-learning/computer-vision?view=azure-ml-py-latest)
* [Azure Machine Learning Workbench](https://docs.microsoft.com/en-us/azure/machine-learning/service/quickstart-installation)
* [Data Science Virtual Machine](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)

