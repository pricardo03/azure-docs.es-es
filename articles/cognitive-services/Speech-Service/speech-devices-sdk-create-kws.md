---
title: 'Creación de una palabra clave personalizada: servicio de voz'
titleSuffix: Azure Cognitive Services
description: El dispositivo siempre espera una palabra clave (o frase). Cuando el usuario dice la palabra clave, el dispositivo envía todo el audio subsiguiente a la nube, hasta que el usuario deja de hablar. La personalización de la palabra clave es una forma eficaz de diferenciar el dispositivo y de reforzar la marca.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: erhopf
ms.openlocfilehash: 42bcc336bfeb325a08c3d65438d66690c0b35100
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2019
ms.locfileid: "74896412"
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

Antes de poder usar una palabra clave personalizada, deberá crear una palabra clave con la página [Custom Keyword](https://aka.ms/sdsdk-wakewordportal) (Palabra clave personalizada) en [Speech Studio](https://aka.ms/sdsdk-speechportal). Después de proporcionar una palabra clave, se genera un archivo que se implementa en el dispositivo.

1. Vaya a [Speech Studio](https://aka.ms/sdsdk-speechportal) e **inicie sesión** o, si todavía no tiene una suscripción a Voz, elija [**Crear una suscripción**](https://go.microsoft.com/fwlink/?linkid=2086754).

1. En la página [Custom Keyword](https://aka.ms/sdsdk-wakewordportal) (Palabra clave personalizada), cree un **Nuevo proyecto**. 

1. Escriba un **Nombre**, una **Descripción** opcional y seleccione el idioma. Necesitará un proyecto por idioma, y la compatibilidad está limitada actualmente al idioma en-US.

    ![Descripción del proyecto de palabra clave](media/custom-keyword/custom-kws-portal-new-project.png)

1. Seleccione el proyecto de la lista. 

    ![Selección del proyecto de palabra clave](media/custom-keyword/custom-kws-portal-project-list.png)

1. Para iniciar un nuevo modelo de palabra clave, haga clic en **Entrenar modelo**.

1. Escriba un **Nombre** para el modelo de palabra clave y una **Descripción** opcional, y escriba la **Palabra clave** de su elección y haga clic en **Siguiente**. Existen algunas [directrices](#choose-an-effective-keyword) para ayudarlo a elegir una palabra clave efectiva.

    ![Escriba su palabra clave](media/custom-keyword/custom-kws-portal-new-model.png) 

1. Ahora, el portal creará pronunciaciones candidatas para la palabra clave. Para escuchar a cada candidato, haga clic en los botones de reproducción y desactive las pronunciaciones que sean incorrectas. Una vez que solo las pronunciaciones correctas estén marcadas como activas, haga clic en **Entrenar** para empezar a generar la palabra clave. 

    ![Revisión de la palabra clave](media/custom-keyword/custom-kws-portal-choose-prons.png) 

1. El modelo puede tardar hasta diez minutos en generarse. La lista de palabras clave cambiará de **Procesando** a **Correcto** cuando el modelo esté completo. Luego puede descargar el archivo.

    ![Revisión de la palabra clave](media/custom-keyword/custom-kws-portal-download-model.png) 

1. Guarde el archivo .zip en el equipo. Necesitará este archivo para implementar la palabra clave personalizada en el dispositivo.

## <a name="next-steps"></a>Pasos siguientes

Pruebe la palabra clave personalizada con el [inicio rápido del SDK de dispositivos de Voz](https://aka.ms/sdsdk-quickstart).
