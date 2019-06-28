---
title: 'Ajuste de la salida de texto a voz: servicios de Voz'
titleSuffix: Azure Cognitive Services
description: Habilite el registro en Speech SDK.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: 8d3e25f8217f3cc8772de9fbbb06a407008ca6f6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65593880"
---
# <a name="fine-tune-text-to-speech-output"></a>Ajuste de la salida de texto a voz

Los servicios de Voz de Azure le permiten ajustar la velocidad, la pronunciación, el volumen, el tono y el contorno de la salida de texto a voz mediante el [lenguaje de marcado de síntesis de voz (SSML)](speech-synthesis-markup.md). SSML es un lenguaje de marcado basado en XML que usa etiquetas para informar al servicio sobre qué característica requiere ajuste. Luego, el mensaje SSML se envía en el cuerpo de cada solicitud al servicio de conversión de texto a voz. Para simplificar el proceso de personalización, los servicios de Voz ahora ofrecen una herramienta de [ajuste de voz](https://aka.ms/voicetuning) que permite inspeccionar visualmente las salidas de texto a voz y ajustarlas en tiempo real.

La herramienta de ajuste de voz es compatible con las voces [estándar](language-support.md#standard-voices), [neuronales](language-support.md#text-to-speech), y [personalizadas](how-to-customize-voice-font.md) de Microsoft.

## <a name="get-started-with-the-voice-tuning-tool"></a>Introducción a la herramienta de ajuste de voz

Antes de empezar a ajustar la salida de texto a voz con la herramienta de ajuste de voz, necesita realizar estos pasos:

1. Cree una [cuenta Microsoft gratuita](https://account.microsoft.com/account) si aún no tiene ninguna.
2. Cree una [cuenta de Azure gratuita](https://azure.microsoft.com/free/) si aún no tiene una. Haga clic en **Iniciar gratis** y cree una cuenta de Azure con su cuenta Microsoft.

3. Cree una suscripción de servicios de Voz en Azure Portal. Están disponibles instrucciones paso a paso para [crear un recurso de voz](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#create-a-speech-resource-in-azure).
   >[!NOTE]
   >Al crear un recurso de voz en Azure Portal, la información de la ubicación de Azure debe coincidir con la región de voz TTS. La voz TTS neuronal admite un subconjunto de ubicaciones de Azure. Para obtener una lista completa de compatibilidad, consulte [Regiones](regions.md#text-to-speech).

   >[!NOTE]
   >Debe haber creado una clave F0 o S0 en Azure Portal para poder usar el servicio. El ajuste de voz **no** admiten la [clave de prueba gratuita de 30 días](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started?branch=release-build-cogserv-speech-services#free-trial).

4. Inicie sesión en el portal de [ajuste de voz](https://aka.ms/voicetuning) y conéctese a su suscripción de servicios de Voz. Elija una sola suscripción de servicios de Voz y, luego, cree un proyecto.
5. Seleccione **New Tuning** (Nuevo ajuste). A continuación, siga estos pasos:

   * Busque y seleccione **All Subscriptions** (Todas las suscripciones).  
   * Seleccione **Connect Existing Subscription** (Conectar la suscripción existente).  
     ![Conéctese a una suscripción existente](./media/custom-voice/custom-voice-connect-subscription.png).
   * Escriba la clave de la suscripción de servicios de Voz de Azure y luego seleccione **Add** (Agregar). Las claves de suscripción están disponibles en el portal de personalización de Voz desde la [página de suscripción](https://go.microsoft.com/fwlink/?linkid=2090458). También puede obtener las claves del panel de administración de recursos de [Azure Portal](https://portal.azure.com/).
   * Si tiene más de una suscripción de los servicios de Voz que planea usar, repita estos pasos con cada una.

## <a name="customize-the-text-to-speech-output"></a>Personalización de la salida de texto a voz

Ahora que ha creado las cuentas y vinculado la suscripción, puede iniciar el ajuste de la salida de texto a voz.

1. Elija una voz.
2. Escriba el texto que quiere editar.
3. Antes de empezar a realizar modificaciones, reproduzca el audio para hacerse una idea de la salida.
4. Seleccione la palabra o frase que quiere refinar, y comience a experimentar con diferentes funciones basadas en SSML.

>[!TIP]
> Para más información sobre cómo ajustar SSML y optimizar la salida de voz, consulte [Lenguaje de marcado de síntesis de voz (SSML)](speech-synthesis-markup.md).

## <a name="limitations"></a>Limitaciones

El ajuste de voz neuronal es ligeramente diferente del ajuste de voces estándar y personalizadas.

* La entonación no se admite en las voces neuronales.
* Las características de tono y volumen solo funcionan con frases completas. Estas características no están disponibles en el nivel de palabra.
* En cuanto a la frecuencia, algunas voces neuronales se pueden ajustar en función de las palabras, mientras que otras requieren que seleccione frases completas.

> [!TIP]
> La herramienta de ajuste de voz proporciona información contextual sobre las características y la optimización.

## <a name="next-steps"></a>Pasos siguientes
* [Crear un recurso de voz en Azure](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#create-a-speech-resource-in-azure)
* [Inicio del ajuste de voz](https://speech.microsoft.com/app.html#/VoiceTuning)
* [Lenguaje de marcado de síntesis de voz (SSML)](speech-synthesis-markup.md)
