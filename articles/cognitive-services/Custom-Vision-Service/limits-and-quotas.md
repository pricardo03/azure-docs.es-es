---
title: 'Límites y cuotas: Custom Vision Service'
titleSuffix: Azure Cognitive Services
description: Obtenga información sobre los límites y las cuotas para Custom Vision Service.
services: cognitive-services
author: anrothMSFT
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-vision
ms.topic: conceptual
ms.date: 03/25/2019
ms.author: anroth
ms.openlocfilehash: 878e2b03e21c18ef4ee845251e91642037c6c5ff
ms.sourcegitcommit: 8e1fb03a9c3ad0fc3fd4d6c111598aa74e0b9bd4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70114645"
---
# <a name="limits-and-quotas"></a>Límites y cuotas

Hay dos niveles de claves para el servicio Custom Vision. Puede registrarse para obtener una suscripción F0 (gratis) o S0 (estándar) a través de Azure Portal. Consulte la [página de precios de Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/custom-vision-service/) correspondiente para obtener más información acerca de los precios y las transacciones.

Se espera que el número de etiquetas y de imágenes de aprendizaje por proyecto aumente con el tiempo en los proyectos S0.

||**F0**|**S0**|
|-----|-----|-----|
|Proyectos|2|100|
|Imágenes de aprendizaje por proyecto |5\.000|100 000|
|Predicciones/mes|10 000 |Ilimitado|
|Etiquetas/proyecto|50|500|
|Iteraciones |10|10|
|Mínimo de imágenes marcadas por etiqueta; clasificación (es recomendable 50 o más) |5|5|
|Mínimo de imágenes marcadas por etiqueta; detección de objetos (es recomendable 50 o más)|15|15|
|Durante cuánto tiempo se almacenan las imágenes de predicción|30 días|30 días|
|Operaciones [Prediction](https://go.microsoft.com/fwlink/?linkid=865445) con almacenamiento (transacciones por segundo)|2|10|
|Operaciones [Prediction](https://go.microsoft.com/fwlink/?linkid=865445) sin almacenamiento (transacciones por segundo)|2|20|
|[TrainProject](https://go.microsoft.com/fwlink/?linkid=865446) (llamadas API por segundo)|2|10|
|[Otras llamadas API](https://go.microsoft.com/fwlink/?linkid=865446) (transacciones por segundo)|10|10|
|Tipos de imágenes aceptadas|JPG, PNG, BMP, GIF|JPG, PNG, BMP, GIF|
|Alto y ancho mínimos de la imagen, en píxeles|256 (vea la nota)|256 (vea la nota)|
|Alto y ancho máximos de la imagen, en píxeles|sin límite|sin límite|
|Tamaño de imagen máximo (carga de la imagen de aprendizaje) |6 MB|6 MB|
|Tamaño de imagen máximo (predicción)|4 MB|4 MB|
|Núm. máximo de regiones por imagen de entrenamiento de detección de objetos|200|200|
|Núm. máximo de etiquetas por imagen de clasificación|30|30|

> [!NOTE]
> Se aceptarán imágenes de menos de 256 píxeles, pero se escalarán.
