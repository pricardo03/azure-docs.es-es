---
title: 'Límites y cuotas: Custom Vision Service'
titlesuffix: Azure Cognitive Services
description: Obtenga información sobre los límites y las cuotas para Custom Vision Service.
services: cognitive-services
author: anrothMSFT
manager: cgronlun
ms.service: cognitive-services
ms.component: custom-vision
ms.topic: conceptual
ms.date: 03/16/2018
ms.author: anroth
ms.openlocfilehash: ce06effbce12abb6271e050829d3218f4fbbfbf4
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902868"
---
# <a name="limits-and-quotas"></a>Límites y cuotas

Hay tres niveles de claves para Custom Vision Service. Los recursos F0 y S0 se obtienen a través de Azure Portal. Los detalles sobre las definiciones de precios y transacciones se encuentran en la [página de precios](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/).  Los proyectos F0 se pueden actualizar a proyectos S0.

Los recursos de un proyecto de prueba limitado se adjuntan a su inicio de sesión de Custom Vision (es decir, a una cuenta de AAD o MSA). Estos están diseñados para usarse en pruebas de corta duración para evaluar de forma gratuita el servicio.  Las cuentas que se crearon durante la versión preliminar gratuita, justo antes de la introducción de las versiones preliminares de Azure (1 de marzo de 2018), retendrán sus cuotas anteriores para las versiones de prueba limitadas. 

||**Prueba limitada**|**F0 (Azure)**|**S0 (Azure)**|
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

Se espera que las limitaciones en *# imágenes de aprendizaje por proyecto*  y en *# etiquetas por proyecto* aumenten con el tiempo en los proyectos S0. 
