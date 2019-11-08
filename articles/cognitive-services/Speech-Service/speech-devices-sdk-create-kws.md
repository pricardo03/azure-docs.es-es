---
title: 'Creación de una palabra clave personalizada: servicio Voz'
titleSuffix: Azure Cognitive Services
description: El dispositivo siempre espera una palabra clave (o frase). Cuando el usuario dice la palabra clave, el dispositivo envía todo el audio subsiguiente a la nube, hasta que el usuario deja de hablar. La personalización de la palabra clave es una forma eficaz de diferenciar el dispositivo y de reforzar la marca.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: bf9afb66163532b4095e0d30b1167010320abbf8
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490895"
---
# <a name="create-a-custom-keyword-by-using-the-speech-service"></a>Creación de una palabra clave personalizada mediante el servicio Voz

El dispositivo siempre espera una palabra clave (o frase). Por ejemplo, "Hola Cortana" es una palabra clave para el asistente Cortana. Cuando el usuario dice la palabra clave, el dispositivo envía todo el audio subsiguiente a la nube, hasta que el usuario deja de hablar. La personalización de la palabra clave es una forma eficaz de diferenciar el dispositivo y de reforzar la marca.

En este artículo, va a aprender a crear una palabra clave personalizada para el dispositivo.

## <a name="choose-an-effective-keyword"></a>Elección de una palabra clave eficaz

Tenga en cuenta las siguientes directrices al elegir una palabra clave:

* La palabra clave debe ser una palabra o una frase en inglés. No debería tardar más de dos segundos en decirse.

* Las palabras de 4 a 7 sílabas funcionan mejor. Por ejemplo, "Hey, Computer" es una buena palabra clave, mientras que "Hey" no es adecuada.

* Las palabras clave deben seguir las reglas comunes de pronunciación en inglés.

* Una palabra única o incluso inventada que siga las reglas comunes de pronunciación en inglés puede reducir los falsos positivos. Por ejemplo, "computerama" puede ser una buena palabra clave.

* No elija una palabra común. Por ejemplo, "eat" y "go" son palabras que la gente dice con frecuencia en una conversación ordinaria. Pueden ser desencadenadores falsos para su dispositivo.

* Evite usar una palabra clave que pueda tener pronunciaciones alternativas. Los usuarios tendrían que saber la pronunciación "correcta" para que su dispositivo respondiera. Por ejemplo, "509" puede pronunciarse como "cinco cero nueve" o "quinientos nueve". "R.E.I." puede pronunciarse como "r-e-i" o "rey". "Live" puede pronunciarse como "/līv/" o "/liv/".

* No utilice caracteres especiales, símbolos o dígitos. Por ejemplo, "Go#"y "20 + cats" no serían palabras clave adecuadas. Sin embargo, "go sharp" o "twenty plus cats" pueden funcionar. Todavía puede utilizar los símbolos en su marca y utilizar el marketing y la documentación para reforzar la pronunciación adecuada.

> [!NOTE]
> Si elige una palabra registrada como marca comercial como palabra clave, asegúrese de que es el propietario de esa marca comercial, o bien de tener el permiso del propietario para usar la palabra. Microsoft no es responsable de ningún problema legal que pueda surgir de su elección de la palabra clave.

## <a name="create-your-keyword"></a>Creación de la palabra clave

Antes de poder usar una palabra clave personalizada con el dispositivo, debe crearla mediante el servicio de generación de palabras clave personalizadas de Microsoft. Después de proporcionar una palabra clave, el servicio produce un archivo que se implementa en el kit de desarrollo para habilitar la palabra clave en el dispositivo.

1. Vaya a [Speech Studio](https://aka.ms/sdsdk-speechportal) e **inicie sesión** o, si todavía no tiene una suscripción a Voz, elija [**Crear una suscripción**](https://go.microsoft.com/fwlink/?linkid=2086754).

1. En la página [Palabra clave personalizada](https://aka.ms/sdsdk-wakewordportal), escriba la palabra clave que prefiera y haga clic en **Agregar palabra clave**. Existen algunas [directrices](#choose-an-effective-keyword) para ayudarlo a elegir una palabra clave efectiva. Actualmente, la compatibilidad está limitada al idioma en-US.

    ![Escriba su palabra clave](media/speech-devices-sdk/custom-kws-portal-enter-keyword.png)

1. Ahora, el portal creará pronunciaciones candidatas para la palabra clave. Para escuchar a cada candidato, haga clic en los botones de reproducción y desactive las pronunciaciones que sean incorrectas. Una vez que solo las pronunciaciones correctas estén marcadas como activas, seleccione **Enviar** para empezar a generar la palabra clave. Si quiere cambiar la palabra clave, primero quite la existente; para ello, haga clic en el botón de eliminación que aparece en el lado derecho de la fila mientras se mantiene el puntero sobre ella.

    ![Revisión de la palabra clave](media/speech-devices-sdk/custom-kws-portal-review-keyword.png)

1. El modelo puede tardar hasta un minuto en generarse. Se le pedirá que descargue el archivo.

    ![Descarga de la palabra clave](media/speech-devices-sdk/custom-kws-portal-download-keyword.png)

1. Guarde el archivo .zip en el equipo. Necesitará este archivo para implementar la palabra clave personalizada en el kit de desarrollo.

## <a name="next-steps"></a>Pasos siguientes

Pruebe la palabra clave personalizada con el [inicio rápido del SDK de dispositivos de Voz](https://aka.ms/sdsdk-quickstart).
