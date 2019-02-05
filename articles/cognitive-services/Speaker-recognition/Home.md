---
title: ¿Qué es Speaker Recognition API?
titleSuffix: Azure Cognitive Services
description: Use los algoritmos avanzados para la verificación e identificación del hablante con Speaker Recognition API en Cognitive Services.
services: cognitive-services
author: dwlin
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speaker-recognition
ms.topic: overview
ms.date: 10/01/2018
ms.author: dwlin
ms.openlocfilehash: 7267663bae71b793923bc10a3b52ab54c12168af
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55216768"
---
# <a name="speaker-recognition-api"></a>Speaker Recognition API

Le damos la bienvenida a las Speaker Recognition API de Azure Cognitive Services. Las Speaker Recognition API son API basadas en la nube que proporcionan los algoritmos más avanzados para la comprobación e identificación del hablante. Speaker Recognition puede dividirse en dos categorías: verificación del hablante y la identificación del hablante.


## <a name="speaker-verification"></a>Verificación del hablante

La voz tiene características únicas que se pueden usar para identificar a una persona, de manera similar a una huella digital.  La utilización de la voz como señal para el control de acceso y los escenarios de autenticación ha emergido como una nueva herramienta innovadora; que ofrece, en esencia, un nivel de seguridad superior que simplifica la experiencia de autenticación de los clientes.

Las Speaker Verification API pueden comprobar y autenticar automáticamente los usuarios a través de su voz.

### <a name="enrollment"></a>Inscripción

La inscripción de la comprobación del hablante depende del texto, lo que significa que los hablantes deben elegir una frase de contraseña específica para usar durante las fases de inscripción y comprobación.

En la inscripción, la voz del hablante se graba diciendo una frase específica y, luego, se extraen diversas características y se reconoce la frase elegida. En conjunto, las características extraídas y la frase elegida conforman una firma de voz única.

### <a name="verification"></a>Comprobación

En la comprobación, se compara una frase y una voz de entrada con la firma de voz y la frase de la inscripción, con el fin de comprobar si pertenecen o no a la misma persona y si se dice la frase correcta.

Para más información acerca de la verificación del hablante, consulte  [Speaker API: verificación](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/563309b7778daf06340c9652).

## <a name="speaker-identification"></a>Identificación del hablante

Las Speaker Identification API pueden identificar automáticamente a la persona que habla en un archivo de audio dentro de un grupo de posibles hablantes. El audio de entrada se compara con el grupo de hablantes proporcionado y, en caso de que exista una coincidencia, se devuelve la identidad del hablante.

Todos los hablantes deben someterse primero a un proceso de inscripción para grabar su voz en el sistema y para que se cree una huella de voz.


### <a name="enrollment"></a>Inscripción

La inscripción para la identificación del hablante depende del texto, lo que significa que no hay restricciones con respecto a lo que el hablante dice en el audio. Se graba la voz del hablante y se extraen diversas características para confirmar una firma de voz única.


### <a name="recognition"></a>Reconocimiento

Durante la fase de reconocimiento, se proporciona el audio del hablante desconocido, junto con el grupo de posibles hablantes. La voz de entrada se compara con todos los hablantes a fin de determinar a quién pertenece la voz y, en caso de que se encuentre una coincidencia, se devuelve la identidad del hablante.

Para más información acerca de la identificación del hablante, consulte  [Speaker API: identificación](https://westus.dev.cognitive.microsoft.com/docs/services/563309b6778daf02acc0a508/operations/5645c068e597ed22ec38f42e).
