---
title: Creación de una palabra de reactivación personalizada
description: Creación de una palabra de reactivación personalizada para el SDK de dispositivos de voz.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.technology: speech
ms.topic: article
ms.date: 04/28/2018
ms.author: v-jerkin
ms.openlocfilehash: 615a901c70fff92141442699ea6e4b8fce1c9ace
ms.sourcegitcommit: 068fc623c1bb7fb767919c4882280cad8bc33e3a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/27/2018
ms.locfileid: "39282580"
---
# <a name="create-a-custom-wake-word-using-speech-service"></a>Creación de una palabra de reactivación personalizada mediante el servicio Voz

El dispositivo siempre espera una palabra (o frase) de reactivación. Por ejemplo, "Hola Cortana" es una palabra de reactivación para el asistente Cortana. Cuando el usuario dice la palabra de reactivación, el dispositivo comienza a enviar todo el audio subsiguiente a la nube hasta que el usuario deja de hablar. La personalización de la palabra de reactivación es una forma eficaz de diferenciar el dispositivo y de reforzar la marca.

En este artículo, va a aprender a crear una palabra de reactivación personalizada para el dispositivo.

## <a name="choosing-an-effective-wake-word"></a>Elección de una palabra de reactivación eficaz

Tenga en cuenta las siguientes directrices al elegir una palabra de reactivación.

* La palabra de reactivación debe ser una palabra o una frase en inglés. No debería tardar más de dos segundos en decirse.

* Las palabras de 4 a 7 sílabas funcionan mejor. Por ejemplo, "Hey, Computer" es una buena palabra de reactivación, mientras que "Hey" no es adecuada.

* Las palabras de reactivación deben seguir las reglas comunes de pronunciación en inglés.

* Una palabra única o incluso inventada que siga las reglas comunes de pronunciación en inglés podría reducir los falsos positivos. Por ejemplo, "computerama" podría ser una buena palabra de reactivación.

* No elija una palabra común. Por ejemplo, "eat" y "go" son palabras que la gente dice con frecuencia en una conversación ordinaria. Podrían ser desencadenadores falsos para su dispositivo.

* Evite usar una palabra que pueda tener pronunciaciones alternativas. Los usuarios tendrían que saber la pronunciación "correcta" para que su dispositivo respondiera. Por ejemplo, "509" podría pronunciarse como "five zero nine", "five oh nine" o "five hundred and nine". "R.E.I." podría pronunciado como "R E I" o "Ray". "Live" puede pronunciarse como [līv] o [liv].

* No utilice caracteres especiales, símbolos o dígitos. Por ejemplo, "Go#"y "20 + cats" no serían palabras de reactivación adecuadas. Sin embargo, "go sharp" o "twenty plus cats" podrían funcionar. Todavía puede utilizar los símbolos en su marca, y utilizar el marketing y la documentación para reforzar la pronunciación adecuada.

> [!NOTE]
> Si elige una palabra con la palabra registrada como palabra de reactivación, asegúrese de que es el propietario de esa marca comercial, o bien de tener el permiso del propietario para usarla. Microsoft no es responsable de ningún problema legal que pueda surgir de su elección de la palabra de reactivación.

## <a name="creating-your-wake-word"></a>Creación de la palabra de reactivación

Antes de poder usar una palabra de reactivación personalizada con el dispositivo, debe crearla mediante el servicio de generación de palabras de reactivación personalizadas de Microsoft. Después de proporcionar una palabra de reactivación, el servicio produce un archivo que luego se implementa en el kit de desarrollo para habilitar la palabra de reactivación en el dispositivo.

1. Vaya al [portal de Custom Speech Service](https://cris.ai/).

2. Cree una nueva cuenta con la dirección de correo electrónico en la que ha recibido la invitación para Azure Active Directory. 

    ![crear nueva cuenta](media/speech-devices-sdk/wake-word-1.png)
 
3.  Cuando haya iniciado sesión, rellene el formulario y después, haga clic en **Start the journey** (Iniciar el proceso).

    ![sesión iniciada correctamente](media/speech-devices-sdk/wake-word-3.png)
 
4. La página **Custom Wake Word** (Palabra de reactivación personalizada) no está disponible para el público, por lo que no hay ningún vínculo que le lleve allí. En su lugar, haga clic o pegue en este vínculo: https://cris.ai/customkws.

    ![página oculta](media/speech-devices-sdk/wake-word-4.png)
 
6. Escriba la palabra de reactivación que elija y luego **envíela**.

    ![escriba la palabra de reactivación](media/speech-devices-sdk/wake-word-5.png)
 
7. Los archivos pueden tardar unos minutos en generarse. Debería ver un círculo giratorio en la pestaña del explorador. Después de un momento, aparecerá una barra de información que le pedirá que descargue un archivo `.zip`.

    ![recibiendo el archivo .zip](media/speech-devices-sdk/wake-word-6.png)

8. Guarde el archivo `.zip` en el equipo. Necesita este archivo para implementar la palabra de reactivación personalizada en el kit de desarrollo, siguiendo las instrucciones de [Introducción al SDK de dispositivos de voz](speech-devices-sdk-qsg.md).

9. Ahora puede **cerrar la sesión.**

## <a name="next-steps"></a>Pasos siguientes

Para comenzar, obtenga una [cuenta gratuita de Azure](https://azure.microsoft.com/free/) y regístrese para obtener el SDK de dispositivos de voz.

> [!div class="nextstepaction"]
> [Registrarse para obtener el SDK de dispositivos de voz](get-speech-devices-sdk.md)

