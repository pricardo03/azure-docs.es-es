---
title: Compile e implemente un modelo de similitud de imágenes con el paquete de Azure Machine Learning para Computer Vision.
description: Aprenda a compilar, entrenar, probar e implementar un modelo de similitud de imágenes de Computer Vision con el paquete de Azure Machine Learning para Computer Vision.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 05/20/2018
ROBOTS: NOINDEX
ms.openlocfilehash: 371903e1ee080d2b98fd46ac4d6d9838416e1335
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46973429"
---
# <a name="build-and-deploy-image-similarity-models-with-azure-machine-learning"></a>Compilar e implementar modelos de similitud de imágenes con Azure Machine Learning

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]


En este artículo obtendrá información sobre cómo usar el paquete de Azure Machine Learning para Computer Vision (AMLPCV) para entrenar, probar e implementar un modelo de similitud de imágenes. Para obtener información general de este paquete y su documentación de referencia detallada, vea https://aka.ms/aml-packages/vision.

Un gran número de problemas en el dominio de Computer Vision pueden resolverse mediante la similitud de imágenes. Estos problemas incluyen compilar modelos que respondan preguntas como las siguientes:
+ _¿Hay un OBJETO en la imagen? Por ejemplo, "perro", "automóvil", "nave", etc._
+ _¿Qué gravedad de enfermedad ocular revela el escáner de retina de este paciente?_

Al compilar e implementar este modelo con AMLPCV, realiza los pasos siguientes:
1. Creación del conjunto de datos
2. Visualización y anotación de imágenes
3. Aumento de imágenes
4. Definición del modelo de Red neuronal profunda (DNN)
5. Entrenamiento de clasificadores
6. Evaluación y visualización
7. Implementación del servicio web
8. Pruebas de carga del servicio web

[CNTK](https://www.microsoft.com/cognitive-toolkit/) se usa como el marco de aprendizaje profundo, el entrenamiento se realiza de manera local en una máquina con tecnología GPU, como la máquina virtual de ciencia de datos de aprendizaje profundo ([Deep learning Data Science VM ](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)) y la implementación usa la CLI de operacionalización de Azure ML.

## <a name="prerequisites"></a>Requisitos previos

1. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

1. Deben estar configuradas e instaladas las siguientes cuentas y la siguiente aplicación:
   - Una cuenta de Experimentación de Azure Machine Learning. 
   - Una cuenta de Administración de modelos de Azure Machine Learning.
   - Azure Machine Learning Workbench instalado.

   Si todavía no se crearon o instalaron las tres, siga las instrucciones que aparecen en el artículo sobre la [instalación de Workbench y el inicio rápido de Azure Machine Learning](../desktop-workbench/quickstart-installation.md). 

1. Azure Machine Learning Package for Computer Vision debe estar instalado. Obtenga información sobre cómo instalar este paquete como se describe en https://aka.ms/aml-packages/vision.

## <a name="sample-data-and-notebook"></a>Notebook y datos de ejemplo

### <a name="get-the-jupyter-notebook"></a>Obtención de Jupyter Notebook

Descargue el Notebook para ejecutar el ejemplo que se describe aquí.

> [!div class="nextstepaction"]
> [Obtención de Jupyter Notebook](https://aka.ms/aml-packages/vision/notebooks/object_detection)

### <a name="load-the-sample-data"></a>Carga de los datos de ejemplo

En el ejemplo siguiente se usa un conjunto de datos que consta de 63 imágenes de vajilla. Cada imagen está etiquetada como perteneciente a una de cuatro clases distintas (cuencos, tazas, cubiertos, platos). La cantidad de imágenes de este ejemplo es pequeña para que podamos ejecutar rápidamente el ejemplo. En la práctica, se deben proporcionar al menos 100 imágenes por clase. Todas las imágenes se encuentran en *"../sample_data/imgs_recycling/"*, en subdirectorios llamados "bowl", "cup", "cutlery" y "plate".

![Conjunto de datos de Azure Machine Learning](media/how-to-build-deploy-image-classification-models/recycling_examples.jpg)

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre Azure Machine Learning Package for Computer Vision en estos artículos:

+ Obtenga información sobre cómo [mejorar la precisión de este modelo](how-to-improve-accuracy-for-computer-vision-models.md).

+ Lea la [información general del paquete](https://aka.ms/aml-packages/vision).

+ Explore la [documentación de referencia](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision) de este paquete.

+ Obtenga información sobre [otros paquetes Python para Azure Machine Learning](reference-python-package-overview.md).
