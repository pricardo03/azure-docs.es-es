---
title: Introducción al aprendizaje automático de Custom Vision Service - Azure Cognitive Services | Microsoft Docs
description: Custom Vision Service es un servicio de Microsoft Cognitive Services que le permite generar clasificadores personalizados de imágenes en la plataforma Azure.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: overview
ms.date: 05/02/2018
ms.author: anroth
ms.openlocfilehash: d2daf7c211f9474f5636b6af69c5b700d597aa14
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/30/2018
ms.locfileid: "43285251"
---
# <a name="what-is-the-custom-vision-service"></a>¿Qué es Custom Vision Service?

Custom Vision Service es un servicio de Microsoft Cognitive Services que le permite generar clasificadores personalizados de imágenes. Con este servicio, resulta muy fácil y rápido crear, implementar y mejorar un clasificador de imágenes. Custom Vision Service cuenta con una API REST y una interfaz web que permite cargar imágenes y entrenar el clasificador.

## <a name="what-does-custom-vision-service-do-well"></a>¿Cómo funciona Custom Vision Service?

Custom Vision Service funciona mejor cuando el elemento que intenta clasificar destaca claramente en la imagen. 

Para crear un clasificador o detector, se necesitan pocas imágenes. Para iniciar el prototipo, son suficientes 50 imágenes de cada clase. Los métodos que Custom Vision Service se concentran en las diferencias, lo que permite comenzar la creación de prototipos con tan pocos datos. Por tanto, Custom Vision Service no funciona bien en escenarios en los que se pretende detectar diferencias sutiles; por ejemplo, grietas o abolladuras pequeñas en escenarios de control de calidad.

## <a name="next-steps"></a>Pasos siguientes

[Aprenda a crear un clasificador](getting-started-build-a-classifier.md)
