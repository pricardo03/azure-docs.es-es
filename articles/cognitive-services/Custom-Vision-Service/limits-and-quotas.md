---
title: 'Precios y límites: Custom Vision Service'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre los límites y las cuotas para Custom Vision Service.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: anroth
ms.openlocfilehash: a3fdd39cdbd4204fece145bde23b23e155500bdb
ms.sourcegitcommit: 87bd7bf35c469f84d6ca6599ac3f5ea5545159c9
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58351395"
---
# <a name="pricing-and-limits"></a>Precios y límites

Hay tres niveles de claves para el servicio Custom Vision. Los recursos de un proyecto de prueba limitado se adjuntan a su inicio de sesión de Custom Vision (es decir, a una cuenta de Azure Active Directory o MSA). Estos están diseñados para usarse en pruebas de corta duración para evaluar de forma gratuita el servicio. Puede registrarse para obtener una suscripción F0 (gratis) o S0 (estándar) a través de Azure Portal. Consulte la [página de precios de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) correspondiente para obtener más información acerca de los precios y las transacciones.

Las cuentas que se crearon durante la versión preliminar gratuita, justo antes de la introducción de las versiones preliminares de Azure (1 de marzo de 2018), retendrán sus cuotas anteriores para las versiones de prueba limitadas.

Se espera que el número de etiquetas y de imágenes de aprendizaje por proyecto aumente con el tiempo en los proyectos S0.

||**Prueba limitada**|**F0**|**S0**|
|-----|-----|-----|-----|
|Proyectos|2|2|100|
|Imágenes de aprendizaje por proyecto; clasificación|5.000|5.000|50.000|
|Imágenes de aprendizaje por proyecto; detección de objetos|5.000|5.000|10 000|
|Predicciones/mes|10 000 |10 000|Ilimitado|
|Etiquetas/proyecto|50|50|250|
|Iteraciones |10|10|10|
|Mínimo de imágenes marcadas por etiqueta; clasificación (es recomendable 50 o más) |5|5|5|
|Mínimo de imágenes marcadas por etiqueta; detección de objetos (es recomendable 50 o más)|15|15|15|
|Durante cuánto tiempo se almacenan las imágenes de predicción|30 días|30 días|30 días|
|Operaciones [Prediction](https://go.microsoft.com/fwlink/?linkid=865445) con almacenamiento (transacciones por segundo)|2|2|10|
|Operaciones [Prediction](https://go.microsoft.com/fwlink/?linkid=865445) sin almacenamiento (transacciones por segundo)|2|2|20|
|[TrainProject](https://go.microsoft.com/fwlink/?linkid=865446) (llamadas API por segundo)|2|2|10|
|[Otras llamadas API](https://go.microsoft.com/fwlink/?linkid=865446) (transacciones por segundo)|10|10|10|
|Tamaño de imagen máximo (carga de la imagen de aprendizaje) |6 MB|6 MB|6 MB|
|Tamaño de imagen máximo (predicción)|4 MB|4 MB|4 MB|