---
title: 'Ajustar la salida de texto a voz: servicios de voz'
titleSuffix: Azure Cognitive Services
description: Habilitar el registro en el SDK de voz.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 8d3e25f8217f3cc8772de9fbbb06a407008ca6f6
ms.sourcegitcommit: 1fbc75b822d7fe8d766329f443506b830e101a5e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/14/2019
ms.locfileid: "65593880"
---
# <a name="fine-tune-text-to-speech-output"></a>Ajuste de la salida de texto a voz

Servicios de voz de Azure le permiten ajustar la velocidad pronunciación, volumen, cabeceo y contorno del texto a voz de salida mediante [lenguaje de marcado de síntesis de voz (SSML)](speech-synthesis-markup.md). SSML es un lenguaje de marcado basado en XML que usa etiquetas para informar al servicio sobre qué característica requiere el ajuste. A continuación, se envía el mensaje SSML en el cuerpo de cada solicitud al servicio de texto a voz. Para simplificar el proceso de personalización, los servicios de voz ahora ofrecen una [optimización voz](https://aka.ms/voicetuning) da como resultado de herramienta que permite inspeccionar visualmente y ajustar el texto a voz en tiempo real.

La herramienta de ajuste de voz es compatible con Microsoft [estándar](language-support.md#standard-voices), [neuronales](language-support.md#text-to-speech), y [voces personalizadas](how-to-customize-voice-font.md).

## <a name="get-started-with-the-voice-tuning-tool"></a>Introducción a la herramienta de ajuste de voz

Antes de empezar a ajustar la salida de texto a voz con la herramienta de ajuste de voz, necesita completar estos pasos:

1. Crear un [cuenta de Microsoft gratuita](https://account.microsoft.com/account) si aún no tiene uno.
2. Crear un [cuenta gratuita de Azure](https://azure.microsoft.com/free/) si aún no tiene uno. Haga clic en **comenzar gratis**y crear una nueva cuenta de Azure con su cuenta de Microsoft.

3. Crear una suscripción de servicios de voz en el portal de Azure. Instrucciones paso a paso para [cómo crear un recurso de voz](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#create-a-speech-resource-in-azure) están disponibles.
   >[!NOTE]
   >Al crear un recurso de voz en el portal de Azure, la información de ubicación de Azure debe coincidir con la región de la voz TTS. Voz TTS neuronal admite un conjunto de sub de ubicaciones de Azure. Para obtener una lista completa de soporte técnico, consulte [regiones](regions.md#text-to-speech).

   >[!NOTE]
   >Debe tener un F0 o una clave de S0 creada en el portal de Azure para poder usar el servicio. Optimización de voz **no** admiten la [clave de prueba gratuita de 30 días](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started?branch=release-build-cogserv-speech-services#free-trial).

4. Inicie sesión en el [optimización voz](https://aka.ms/voicetuning) portal y conéctese a su suscripción de servicios de voz. Elija una sola suscripción de servicios de voz y, a continuación, cree un proyecto.
5. Seleccione **nueva optimización**. A continuación, siga estos pasos:

   * Busque y seleccione **todas las suscripciones**.  
   * Seleccione **Connect Existing Subscription** (Conectar la suscripción existente).  
     ![Conectar una suscripción existente](./media/custom-voice/custom-voice-connect-subscription.png).
   * Escriba la clave de suscripción de servicios de voz de Azure y luego seleccione **agregar**. Las claves de suscripción están disponibles en el portal de la personalización de voz desde el [página de suscripción](https://go.microsoft.com/fwlink/?linkid=2090458). También puede obtener las claves en el panel de administración de recursos en el [portal Azure](https://portal.azure.com/).
   * Si tiene más de una suscripción de servicios de voz que se va a usar, repita estos pasos para cada suscripción.

## <a name="customize-the-text-to-speech-output"></a>Personalizar la salida de texto a voz

Ahora que ha creado las cuentas y vinculado la suscripción, puede iniciar la optimización de la salida de texto a voz.

1. Elija una voz.
2. Escriba el texto que desea editar.
3. Antes de empezar a realizar modificaciones, reproducir el audio para hacerse una idea de la salida.
4. Seleccione la palabra o frase que desea refinar y comenzar a experimentar con diferentes funciones de SSML.

>[!TIP]
> Para obtener información detallada sobre cómo ajustar SSML y optimización de la salida de voz, consulte [lenguaje de marcado de síntesis de voz (SSML)](speech-synthesis-markup.md).

## <a name="limitations"></a>Limitaciones

Optimización Neural de voz es ligeramente diferente que la optimización de voces estándar y personalizado.

* No se admite entonación de voces neuronales.
* Características de cabeceo y volumen solo funcionan con frases completas. Estas características no están disponibles en el nivel de word.
* Frecuencia, se pueden optimizar algunas voces neuronales basado en palabras, mientras que otras requieren que seleccione oraciones completas.

> [!TIP]
> La herramienta de ajuste de voz proporciona información contextual sobre las características y la optimización.

## <a name="next-steps"></a>Pasos siguientes
* [Crear un recurso de voz en Azure](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#create-a-speech-resource-in-azure)
* [Iniciar la optimización de voz](https://speech.microsoft.com/app.html#/VoiceTuning)
* [Lenguaje de marcado de síntesis de voz (SSML)](speech-synthesis-markup.md)
