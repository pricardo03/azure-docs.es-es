---
title: Medidores y cuotas de Custom Speech Service en Azure | Microsoft Docs
description: Información sobre medidores y cuotas de Custom Speech Service en Azure.
services: cognitive-services
author: PanosPeriorellis
manager: onano
ms.service: cognitive-services
ms.component: custom-speech
ms.topic: article
ms.date: 07/08/2017
ms.author: panosper
ROBOTS: NOINDEX
ms.openlocfilehash: 0ddd6274051fed5de86a88270a0dcc2e8288d885
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46974534"
---
# <a name="custom-speech-service-meters-and-quotas"></a>Medidores y cuotas de Custom Speech Service

Con Custom Speech Service basado en la nube, puede personalizar los modelos de voz para la transcripción de voz a texto.

Para comenzar a usar Custom Speech Service, vaya al [portal de Custom Speech Service](https://cris.ai).

Para información sobre los medidores de precios actuales, vaya a la página [Precios de Cognitive Services para Custom Speech Service](https://azure.microsoft.com/pricing/details/cognitive-services/custom-speech-service/).

## <a name="tiers-explained"></a>Niveles explicados
Solo para pruebas y prototipos, se propone el uso del nivel F0. Para los sistemas de producción, se propone el uso del nivel S2. Con el nivel S2, puede escalar su departamento al número de unidades de escalado (SU) que necesita su escenario.

> [!NOTE]
> *No* puede migrar entre el nivel F0 y el nivel S2.
>

## <a name="meters-explained"></a>Medidores explicados

### <a name="scale-out"></a>Escalabilidad horizontal
La escalabilidad horizontal es una nueva característica que hemos publicado con el nuevo modelo de precios. Con la escalabilidad horizontal, puede controlar el número de solicitudes simultáneas que puede procesar el modelo.

Puede establecer solicitudes simultáneas mediante la medida de SU en la vista **Create Model Deployment** (Crear implementación de modelos). Para más información, consulte [Creación de un punto de conexión personalizado de voz a texto](CustomSpeech-How-to-Topics/cognitive-services-custom-speech-create-endpoint.md). En función de la cantidad de tráfico que prevea que consume el modelo, puede elegir un número adecuado de SU. 

> [!NOTE]
> Cada unidad de escalado garantiza 5 solicitudes simultáneas. Puede comprar 1 o varias SU, según sea conveniente. Dado que el número de SU aumenta en incrementos de 1, se garantiza que el número de solicitudes simultáneas aumenta en incrementos de 5.
>

### <a name="log-management"></a>Administración de registros
Puede optar por desactivar las trazas de audio para un modelo recién implementado sin costo adicional. Custom Speech Service no registra las solicitudes de audio ni las transcripciones de ese modelo.

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre cómo usar Custom Speech Service, vaya al [portal de Custom Speech Service](https://cris.ai).

* [Primeros pasos](cognitive-services-custom-speech-get-started.md)
* [P+F](cognitive-services-custom-speech-faq.md)
* [Glosario](cognitive-services-custom-speech-glossary.md)
 