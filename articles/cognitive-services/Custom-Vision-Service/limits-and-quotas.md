---
title: Límites y cuotas de Custom Vision Service en Azure Cognitive Services | Microsoft Docs
description: Obtenga información sobre los límites y las cuotas de Custom Vision Service para Azure Cognitive Services.
services: cognitive-services
author: anrothMSFT
manager: corncar
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: article
ms.date: 03/16/2018
ms.author: anroth
ms.openlocfilehash: 44666d5d7f2a51e4017c704205d21b1f6d06908c
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382459"
---
# <a name="limits-and-quotas"></a>Límites y cuotas

Hay tres niveles de claves para Custom Vision Service. Los recursos F0 y S0 se obtienen a través de Azure Portal. Los detalles sobre las definiciones de precios y transacciones se encuentran en la [página de precios](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/).  Los proyectos F0 se pueden actualizar a proyectos S0.

Los recursos de un proyecto de prueba limitado se adjuntan a su inicio de sesión de Custom Vision (es decir, a una cuenta de AAD o MSA). Estos están diseñados para usarse en pruebas de corta duración para evaluar de forma gratuita el servicio.  Las cuentas que se crearon durante la versión preliminar gratuita, justo antes de la introducción de las versiones preliminares de Azure (1 de marzo de 2018), retendrán sus cuotas anteriores para las versiones de prueba limitadas. 

||**Prueba limitada**|**F0 (Azure)**|**S0 (Azure)**|
|-----|-----|-----|-----|
|Proyectos|2|2|100|
|Imágenes de aprendizaje por proyecto|5.000|5.000|50.000|
|Predicciones/mes|10.000 |10.000|Ilimitado|
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

Se espera que las limitaciones en *# imágenes de aprendizaje por proyecto*  y en *# etiquetas por proyecto* aumenten con el tiempo en los proyectos S0. 
