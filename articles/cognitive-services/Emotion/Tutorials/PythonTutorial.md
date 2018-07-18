---
title: Tutorial de Emotion API en Python | Microsoft Docs
description: Utilice Jupyter Notebook para obtener información sobre cómo usar Emotion API de Cognitive Services con Python. Visualice los resultados mediante bibliotecas populares.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: emotion-api
ms.topic: article
ms.date: 05/23/2017
ms.author: anroth
ms.openlocfilehash: 70c8ca48c651601f3d7cbb3717c32bfe112176fe
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35380690"
---
# <a name="emotion-api-using-python-tutorial"></a>Tutorial de Emotion API con Python

> [!IMPORTANT]
> La versión preliminar de Video API terminará el 30 de octubre de 2017. Pruebe la versión preliminar de la nueva [Video Indexer API con una extracción sencilla de detalles en](https://azure.microsoft.com/services/cognitive-services/video-indexer/) vídeos y mejore las experiencias de detección de contenido, como los resultados de la búsqueda, gracias al reconocimiento de texto oral, caras, caracteres y emociones. [Más información](https://docs.microsoft.com/azure/cognitive-services/video-indexer/video-indexer-overview).

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
