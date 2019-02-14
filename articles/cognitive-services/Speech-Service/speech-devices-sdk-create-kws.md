---
title: 'Creación de una palabra de reactivación personalizada: servicios de Voz'
titleSuffix: Azure Cognitive Services
description: El dispositivo siempre espera una palabra (o frase) de reactivación. Cuando el usuario dice la palabra de reactivación, el dispositivo envía todo el audio subsiguiente a la nube, hasta que el usuario deja de hablar. La personalización de la palabra de reactivación es una forma eficaz de diferenciar el dispositivo y de reforzar la marca.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 22d270d9bc337b9d7ad776baf5dd35f877c05eae
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55856434"
---
# <a name="create-a-custom-wake-word-by-using-the-speech-service"></a>Creación de una palabra de reactivación personalizada mediante Speech Service

El dispositivo siempre espera una palabra (o frase) de reactivación. Por ejemplo, "Hola Cortana" es una palabra de reactivación para el asistente Cortana. Cuando el usuario dice la palabra de reactivación, el dispositivo envía todo el audio subsiguiente a la nube, hasta que el usuario deja de hablar. La personalización de la palabra de reactivación es una forma eficaz de diferenciar el dispositivo y de reforzar la marca.

En este artículo, va a aprender a crear una palabra de reactivación personalizada para el dispositivo.

## <a name="choose-an-effective-wake-word"></a>Elección de una palabra de reactivación eficaz

Tenga en cuenta las siguientes directrices al elegir una palabra de reactivación:

* La palabra de reactivación debe ser una palabra o una frase en inglés. No debería tardar más de dos segundos en decirse.

* Las palabras de 4 a 7 sílabas funcionan mejor. Por ejemplo, "Hey, Computer" es una buena palabra de reactivación, mientras que "Hey" no es adecuada.

* Las palabras de reactivación deben seguir las reglas comunes de pronunciación en inglés.

* Una palabra única o incluso inventada que siga las reglas comunes de pronunciación en inglés puede reducir los falsos positivos. Por ejemplo, "computerama" puede ser una buena palabra de reactivación.

* No elija una palabra común. Por ejemplo, "eat" y "go" son palabras que la gente dice con frecuencia en una conversación ordinaria. Pueden ser desencadenadores falsos para su dispositivo.

* Evite usar una palabra que pueda tener pronunciaciones alternativas. Los usuarios tendrían que saber la pronunciación "correcta" para que su dispositivo respondiera. Por ejemplo, "509" puede pronunciarse como "cinco cero nueve" o "quinientos nueve". "R.E.I." puede pronunciarse como "r-e-i" o "rey". "Live" puede pronunciarse como "/līv/" o "/liv/".

* No utilice caracteres especiales, símbolos o dígitos. Por ejemplo, "Go#"y "20 + cats" no serían palabras de reactivación adecuadas. Sin embargo, "go sharp" o "twenty plus cats" pueden funcionar. Todavía puede utilizar los símbolos en su marca y utilizar el marketing y la documentación para reforzar la pronunciación adecuada.

> [!NOTE]
> Si elige una palabra registrada como marca comercial como palabra de reactivación, asegúrese de que es el propietario de esa marca comercial, o bien de tener el permiso del propietario para usar la palabra. Microsoft no es responsable de ningún problema legal que pueda surgir de su elección de la palabra de reactivación.

## <a name="create-your-wake-word"></a>Creación de la palabra de reactivación

Antes de poder usar una palabra de reactivación personalizada con el dispositivo, debe crearla mediante el servicio de generación de palabras de reactivación personalizadas de Microsoft. Después de proporcionar una palabra de reactivación, el servicio produce un archivo que se implementa en el kit de desarrollo para habilitar la palabra de reactivación en el dispositivo.

1. Vaya al [portal de Custom Speech Service](https://cris.ai/).

1. Cree una nueva cuenta con la dirección de correo electrónico en la que ha recibido la invitación para Azure Active Directory.

    ![Creación de una nueva cuenta](media/speech-devices-sdk/wake-word-1.png)

1. La página **Custom Wake Word** (Palabra de reactivación personalizada) no está disponible para el público, por lo que no hay ningún vínculo directo que le lleve allí. La característica Custom Speech requiere una suscripción de Azure, mientras que la característica de palabra de reactivación personalizada, no. Si recibe el mensaje de error **No Subscriptions found** (No se han encontrado suscripciones), simplemente sustituya **"Subscriptions?errorMessage=No%20Subscriptions%20found"** por "**customkws**" en la dirección URL y pulse Intro. La dirección URL debe ser una de las siguientes: https://westus.cris.ai/customkws, https://eastasia.cris.ai/customkws o https://northeurope.cris.ai/customkws, dependiendo de dónde esté su región.

    ![Palabra de reactivación personalizada oculta](media/speech-devices-sdk/wake-word-4.png)

1. Escriba la palabra de reactivación que elija y seleccione **Submit the word** (Enviar la palabra).

    ![Introducción de la palabra de reactivación](media/speech-devices-sdk/wake-word-5.png)

1. Los archivos pueden tardar unos minutos en generarse. Debería ver un círculo giratorio en la ventana del explorador. Seguidamente, aparecerá una barra de información que le pedirá que descargue un archivo .zip.

    ![Recepción del archivo .zip](media/speech-devices-sdk/wake-word-6.png)

1. Guarde el archivo .zip en el equipo. Necesitará este archivo para implementar la palabra de reactivación personalizada en el kit de desarrollo. Para implementar la palabra de reactivación personalizada, siga las instrucciones de [Introducción al SDK de dispositivos de voz](speech-devices-sdk-qsg.md).

1. Seleccione **Sign out** (Cerrar sesión).

## <a name="next-steps"></a>Pasos siguientes

Para comenzar, obtenga una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) y regístrese para obtener el SDK de dispositivos de voz.

> [!div class="nextstepaction"]
> [Registrarse para obtener el SDK de dispositivos de voz](get-speech-devices-sdk.md)
