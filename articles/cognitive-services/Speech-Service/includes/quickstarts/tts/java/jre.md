---
title: 'Inicio rápido: Síntesis de voz en Java (Windows, Linux y macOS): Servicios de voz'
titleSuffix: Azure Cognitive Services
description: En este inicio rápido, aprenderá a crear una aplicación Java sencilla que captura y sintetiza la voz a partir de texto y la reproduce con el altavoz predeterminado.
services: cognitive-services
author: yulin-li
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: include
ms.date: 09/19/2019
ms.author: yulili
ms.openlocfilehash: 0ee22c7a34ac332fac930ead6b7d62f6e7aad5d1
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73505391"
---
## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, compruebe lo siguiente:

> [!div class="checklist"]
> * [Ha creado un recurso de Voz de Azure](../../../../get-started.md)
> * [Ha configurado el entorno de desarrollo](../../../../quickstarts/setup-platform.md?tabs=jre)
> * [Ha creado un proyecto de ejemplo vacío](../../../../quickstarts/create-project.md?tabs=jre)

## <a name="add-sample-code"></a>Incorporación de código de ejemplo

1. Para agregar una nueva clase vacía al proyecto de Java, seleccione **File (Archivo)**  >  **New (Nuevo)**  >  **Class (Clase)** .

1. En la ventana **New Java Class** (Nueva clase de Java) escriba **speechsdk.quickstart** en el campo **Package** (Paquete) y **Main** en el campo **Name** (Nombre).

   ![Captura de pantalla de la ventana New Java Class (Nueva clase de Java)](~/articles/cognitive-services/Speech-Service/media/sdk/qs-java-jre-06-create-main-java.png)

1. Reemplace el código en `Main.java` con el siguiente fragmento de código:

   [!code-java[Quickstart Code](~/samples-cognitive-services-speech-sdk/quickstart/java/jre/text-to-speech/src/speechsdk/quickstart/Main.java#code)]

1. Reemplace la cadena `YourSubscriptionKey` por la clave de suscripción.

1. Reemplace la cadena `YourServiceRegion` por la [región](~/articles/cognitive-services/Speech-Service/regions.md) asociada con la suscripción (por ejemplo, `westus` para la suscripción de evaluación gratuita).

1. Guarde los cambios en el proyecto.

## <a name="build-and-run-the-app"></a>Compilación y ejecución de la aplicación

Presione F11, o seleccione **Run (Ejecutar)**  > **Debug (Depurar)** .
Escriba un texto cuando se le solicite y oirá el audio sintetizado, que se reproduce en el altavoz predeterminado.

## <a name="next-steps"></a>Pasos siguientes

[!INCLUDE [footer](./footer.md)]

## <a name="see-also"></a>Otras referencias

- [Creación de una voz personalizada](~/articles/cognitive-services/Speech-Service/how-to-custom-voice-create-voice.md)
- [Grabación de ejemplos de voz personalizada](~/articles/cognitive-services/Speech-Service/record-custom-voice-samples.md)
