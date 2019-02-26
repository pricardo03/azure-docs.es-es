---
title: 'Tutorial: Realización de operaciones de imagen - Python'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre cómo usar Computer Vision API con Python utilizando Jupyter Notebook. Visualice los resultados mediante bibliotecas populares.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: tutorial
ms.date: 11/06/2018
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: b55b1fdf3b93d3865bdb115e43ff8583af61b45d
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/16/2019
ms.locfileid: "56326432"
---
# <a name="tutorial-computer-vision-api-python"></a>Tutorial: Computer Vision API para Python

En este tutorial se muestra cómo usar Computer Vision API en Python y cómo visualizar los resultados mediante algunas bibliotecas conocidas. Usará Jupyter para realizar el tutorial. Para información sobre cómo empezar a trabajar con cuadernos interactivos de Jupyter, consulte la [documentación de Jupyter](https://jupyter.readthedocs.io/en/latest/index.html).

## <a name="prerequisites"></a>Requisitos previos

- [Python 2.7+ o 3.5+](https://www.python.org/downloads/)
- Herramienta [pip](https://pip.pypa.io/en/stable/installing/)
- [Jupyter Notebook](https://jupyter.org/install) instalado

## <a name="open-the-tutorial-notebook-in-jupyter"></a>Apertura del cuaderno del tutorial en Jupyter 

1. Vaya al repositorio de GitHub de [Cognitive Vision Python](https://github.com/Microsoft/Cognitive-Vision-Python). 
2. Haga clic en el botón verde para clonar o descargar el repositorio. 
3. Abra un símbolo del sistema y vaya a la carpeta **Cognitive-Vision-Python-master\Jupyter Notebook**.
1. Asegúrese de tener todas las bibliotecas necesarias; para ello, ejecute el comando `pip install requests opencv-python numpy matplotlib` desde el símbolo del sistema.
1. Para iniciar Jupyter, ejecute el comando `jupyter notebook` desde el símbolo del sistema.
1. En la ventana de Jupyter, haga clic en _Computer Vision API Example.ipynb_ para abrir el cuaderno del tutorial.

## <a name="run-the-tutorial"></a>Ejecución del tutorial

Para utilizar este cuaderno, necesitará una clave de suscripción para Computer Vision API. Visite la [página de suscripción](https://azure.microsoft.com/try/cognitive-services/) para registrarse. En la página de **inicio de sesión**, use su cuenta Microsoft para iniciar sesión y podrá suscribirse y obtener claves gratuitas. Después de completar el proceso de suscripción, pegue la clave en la sección `Variables` del cuaderno (se reproduce a continuación). Servirá la clave principal o la secundaria. Asegúrese de encerrar la clave entre comillas para convertirla en una cadena.

También debe asegurarse de que el campo `_region` coincida con la región que corresponde a la suscripción.

```python
# Variables
_region = 'westcentralus' #Here you enter the region of your subscription
_url = 'https://{}.api.cognitive.microsoft.com/vision/v2.0/analyze'.format(_region)
_key = None #Here you have to paste your primary key
_maxNumRetries = 10
```

Al realizar el tutorial, podrá agregar imágenes para analizar desde una dirección URL y desde el almacenamiento local. El script mostrará las imágenes y la información de análisis en el cuaderno.
