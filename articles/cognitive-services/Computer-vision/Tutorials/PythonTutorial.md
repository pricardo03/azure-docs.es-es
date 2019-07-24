---
title: Realización de operaciones de imagen - Python
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre cómo usar Computer Vision API con Python utilizando Jupyter Notebook. Visualice los resultados mediante bibliotecas populares.
services: cognitive-services
author: KellyDF
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 07/03/2019
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: b18f41db772c1c214bdf48c9bb765b1ce7e6d2c2
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604151"
---
# <a name="computer-vision-api-jupyter-notebook"></a>Cuaderno de Jupyter de la API Computer Vision

En esta guía se muestra cómo usar la API Computer Vision en Python y cómo visualizar los resultados mediante algunas bibliotecas conocidas. Usará Jupyter para realizar el tutorial. Para información sobre cómo empezar a trabajar con cuadernos interactivos de Jupyter, consulte la [documentación de Jupyter](https://jupyter.readthedocs.io/en/latest/index.html).

## <a name="prerequisites"></a>Requisitos previos

- [Python 2.7+ o 3.5+](https://www.python.org/downloads/)
- Herramienta [pip](https://pip.pypa.io/en/stable/installing/)
- [Jupyter Notebook](https://jupyter.org/install) instalado

## <a name="open-the-notebook-in-jupyter"></a>Apertura del cuaderno en Jupyter 

1. Vaya al repositorio de GitHub de [Cognitive Vision Python](https://github.com/Microsoft/Cognitive-Vision-Python). 
2. Haga clic en el botón verde para clonar o descargar el repositorio. 
3. Abra un símbolo del sistema y vaya a la carpeta **Cognitive-Vision-Python-master\Jupyter Notebook**.
1. Asegúrese de tener todas las bibliotecas necesarias; para ello, ejecute el comando `pip install requests opencv-python numpy matplotlib` desde el símbolo del sistema.
1. Para iniciar Jupyter, ejecute el comando `jupyter notebook` desde el símbolo del sistema.
1. En la ventana de Jupyter, haga clic en _Computer Vision API Example.ipynb_ para abrir el cuaderno del tutorial.

## <a name="run-the-notebook"></a>Ejecución del cuaderno

Para utilizar este cuaderno, necesitará una clave de suscripción para Computer Vision API. Visite la [página de suscripción](https://azure.microsoft.com/try/cognitive-services/) para registrarse. En la página de **inicio de sesión**, use su cuenta Microsoft para iniciar sesión y podrá suscribirse y obtener claves gratuitas. Después de completar el proceso de suscripción, pegue la clave en la sección `Variables` del cuaderno (se reproduce a continuación). Servirá la clave principal o la secundaria. Asegúrese de encerrar la clave entre comillas para convertirla en una cadena.

También debe asegurarse de que el campo `_region` coincida con la región que corresponde a la suscripción.

```python
# Variables
_region = 'westcentralus'  # Here you enter the region of your subscription
_url = 'https://{}.api.cognitive.microsoft.com/vision/v2.0/analyze'.format(
    _region)
_key = None  # Here you have to paste your primary key
_maxNumRetries = 10
```

Al realizar el tutorial, podrá agregar imágenes para analizar desde una dirección URL y desde el almacenamiento local. El script mostrará las imágenes y la información de análisis en el cuaderno.
