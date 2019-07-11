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
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: f43cbedc633b26a3a7fcbfb5f6a75da514bf0c45
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67604863"
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

1. Vaya al [portal de Custom Speech Service](https://aka.ms/sdsdk-speechportal) e **inicie sesión**; o, si no tiene una suscripción a Voz, elija [ **Create a subscription**](https://go.microsoft.com/fwlink/?linkid=2086754) (Crear una suscripción).

    ![Portal de Custom Speech Service](media/speech-devices-sdk/wake-word-4.png)

1. En la página [Custom Wake Word](https://aka.ms/sdsdk-wakewordportal) (Palabra de reactivación personalizada), escriba la palabra de reactivación de su elección y haga clic en **Add wake word** (Agregar palabra de reactivación). Existen algunas [directrices](#choose-an-effective-wake-word) para ayudarlo a elegir una palabra clave efectiva. Actualmente solo se admite el idioma en-US.

    ![Introducción de la palabra de reactivación](media/speech-devices-sdk/wake-word-5.png)

1. Se crea tres pronunciaciones alternativas de la palabra de reactivación. Puede elegir todas las pronunciaciones que quiera. A continuación, seleccione **Submit** (Enviar) para generar la palabra de reactivación. Si quiere cambiar la palabra de reactivación, quite primero la existente; cuando mantenga el mouse sobre la línea de pronunciación aparecerá el icono de eliminación.

    ![Revisión de la palabra de reactivación](media/speech-devices-sdk/wake-word-6.png)

1. El modelo puede tardar hasta un minuto en generarse. Se le pedirá que descargue el archivo.

    ![Descarga de la palabra de reactivación](media/speech-devices-sdk/wake-word-7.png)

1. Guarde el archivo .zip en el equipo. Necesitará este archivo para implementar la palabra de reactivación personalizada en el kit de desarrollo.

## <a name="next-steps"></a>Pasos siguientes

Pruebe la palabra de reactivación personalizado con el [inicio rápido del SDK de dispositivos de voz](https://aka.ms/sdsdk-quickstart).
