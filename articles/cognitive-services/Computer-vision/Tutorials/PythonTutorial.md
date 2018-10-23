---
title: 'Tutorial: Computer Vision API para Python'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre cómo usar Computer Vision API con Python utilizando Jupyter Notebook. Visualice los resultados mediante bibliotecas populares.
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: tutorial
ms.date: 02/25/2017
ms.author: kefre
ms.openlocfilehash: 046250d3d2142badaac35490eff27bcac220fea9
ms.sourcegitcommit: 1aacea6bf8e31128c6d489fa6e614856cf89af19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2018
ms.locfileid: "49344904"
---
# <a name="tutorial-computer-vision-api-python"></a>Tutorial: Computer Vision API para Python

En este tutorial se muestra cómo utilizar Computer Vision API en Python y cómo visualizar los resultados mediante algunas bibliotecas populares. Utilice Jupyter para ejecutar el tutorial. Para obtener información sobre cómo empezar a trabajar con cuadernos interactivos de Jupyter Notebook, consulte la [documentación de Jupyter](http://jupyter.readthedocs.io/en/latest/index.html). 

## <a name="open-the-tutorial-notebook-in-jupyter"></a>Apertura del cuaderno del tutorial en Jupyter 

1. Navegue hasta el [cuaderno del tutorial en GitHub](https://github.com/Microsoft/Cognitive-Vision-Python). 
2. Haga clic en el botón verde para clonar o descargar el tutorial. 
3. Abra un símbolo del sistema y vaya a la carpeta _Cognitive-Vision-Python-master\Jupyter Notebook_. 
4. Ejecute el comando **jupyter notebook** desde el símbolo del sistema. Esto iniciará Jupyter.
5. En la ventana de Jupyter, haga clic en _Computer Vision API Example.ipynb_ para abrir el cuaderno del tutorial. 

## <a name="run-the-tutorial"></a>Ejecución del tutorial

Para utilizar este cuaderno, necesitará una clave de suscripción para Computer Vision API. Visite la [página de suscripción](https://azure.microsoft.com/try/cognitive-services/) para registrarse. En la página "Inicio de sesión", use su cuenta de Microsoft para iniciar sesión y podrá suscribirse y obtener claves gratuitas. Después de completar el proceso de suscripción, pegue la clave en la sección de variables del cuaderno (se reproduce a continuación). Funcionará la clave principal o la secundaria. Asegúrese de encerrar la clave entre comillas para que sea una cadena.

```python
# Variables

_url = 'https://westcentralus.api.cognitive.microsoft.com/vision/v1/analyses'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10
```
