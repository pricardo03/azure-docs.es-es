---
title: 'Tutorial: Reconocimiento de emociones en las caras de una imagen (Emotion API, Python)'
titlesuffix: Azure Cognitive Services
description: Use un cuaderno de Jupyter para saber cómo usar Emotion API con Python. Visualice los resultados mediante bibliotecas populares.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: emotion-api
ms.topic: tutorial
ms.date: 05/23/2017
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 9e03e5d534fa741def2010a21120edc3d7169964
ms.sourcegitcommit: de81b3fe220562a25c1aa74ff3aa9bdc214ddd65
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56237474"
---
# <a name="tutorial-use-the-emotion-api-with-a-jupyter-notebook--python"></a>Tutorial: Uso de Emotion API con un cuaderno de Jupyter y Python.

> [!IMPORTANT]
> Emotion API dejará de usarse el 15 de febrero de 2019. La funcionalidad de reconocimiento de emociones está ahora disponible con carácter general como parte de [Face API](https://docs.microsoft.com/azure/cognitive-services/face/). 

Para facilitar la introducción a Emotion API, el cuaderno de Jupyter Notebook vinculado a continuación muestra cómo utilizar la API en Python y cómo visualizar los resultados mediante algunas bibliotecas populares.

[Vínculo al cuaderno en GitHub](https://github.com/Microsoft/Cognitive-Emotion-Python/blob/master/Jupyter%20Notebook/Emotion%20Analysis%20Example.ipynb)

### <a name="using-the-jupyter-notebook"></a>Uso de Jupyter Notebook

Para usar Notebook de forma interactiva, debe clonarlo y ejecutarlo en Jupyter. Para obtener información sobre cómo empezar a trabajar con cuadernos interactivos de Jupyter Notebook, siga las instrucciones en http://jupyter.readthedocs.org/en/latest/install.html.

Para utilizar este cuaderno, necesitará una clave de suscripción para Emotion API. Visite la [página de suscripción](https://azure.microsoft.com/try/cognitive-services/) para registrarse. En la página "Inicio de sesión", use su cuenta de Microsoft para iniciar sesión y podrá suscribirse y obtener claves gratuitas. Después de completar el proceso de suscripción, pegue la clave en la sección de variables a continuación. Funcionará la clave principal o la secundaria.

```
Python Example

#Variables

_url = 'https://westus.api.cognitive.microsoft.com/emotion/v1.0/recognize'
_key = None #Here you have to paste your primary key
_maxNumRetries = 10

```
