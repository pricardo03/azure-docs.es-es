---
title: 'Inicio rápido: Detección de caras en una imagen con la API REST de Azure y Python'
titleSuffix: Azure Cognitive Services
description: En esta guía de inicio rápido, usará la API REST de Azure Face con Python para detectar caras en una imagen.
services: cognitive-services
author: PatrickFarley
manager: cgronlun
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: quickstart
ms.date: 11/09/2018
ms.author: pafarley
ms.openlocfilehash: 606f01059620443c8c42a24f416128d0856eff50
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55213793"
---
# <a name="quickstart-detect-faces-in-an-image-using-the-face-rest-api-and-python"></a>Inicio rápido: Detección de caras en una imagen mediante Face REST API y Python

En esta guía de inicio rápido, usará la API REST de Azure Face con Python para detectar caras humanas en una imagen. El script dibujará marcos alrededor de las caras y superpondrá información de género y edad en la imagen.

![Un hombre y una mujer, cada uno con un rectángulo dibujado alrededor de sus caras y mostrando información sobre la edad y el sexo en la imagen.](../images/labelled-faces-python.png)

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar. 


## <a name="prerequisites"></a>Requisitos previos

- Una clave de suscripción de Face API. Puede obtener una clave de la suscripción de evaluación gratuita en la página [Pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=face-api). O bien, siga las instrucciones de [Creación de una cuenta de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) para suscribirse al servicio Face API y obtener la clave.

## <a name="run-the-jupyter-notebook"></a>Ejecutar Jupyter Notebook

Puede ejecutar esta guía de inicio rápido como una instancia de Jupyter Notebook en [MyBinder](https://mybinder.org). Para iniciar Binder, seleccione el botón siguiente. Después, siga las instrucciones del Notebook.

[![Binder](https://mybinder.org/badge.svg)](https://mybinder.org/v2/gh/Microsoft/cognitive-services-notebooks/master?filepath=FaceAPI.ipynb)

## <a name="next-steps"></a>Pasos siguientes

A continuación, explore la documentación de referencia de Face API para más información sobre los escenarios admitidos.

> [!div class="nextstepaction"]
> [Face API](https://westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236)
