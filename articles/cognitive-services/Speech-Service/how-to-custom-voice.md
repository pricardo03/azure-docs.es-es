---
title: '¿Qué es la voz personalizada? : Servicios de voz'
titlesuffix: Azure Cognitive Services
description: Voz personalizada es un conjunto de herramientas en línea que le permiten crear una voz reconocible, uno de tipo para su marca. Todo que lo necesario para empezar a trabajar son unos cuantos archivos de audio y las transcripciones asociados. Siga los vínculos siguientes para empezar a crear una experiencia personalizada de voz a texto.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: erhopf
ms.openlocfilehash: 8cf9dc6cbfc96448462aac3a64807f8beb6036ad
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65156916"
---
# <a name="get-started-with-custom-voice"></a>Empezar a trabajar con voz personalizado

Voz personalizada es un conjunto de herramientas en línea que le permiten crear una voz reconocible, uno de tipo para su marca. Todo que lo necesario para empezar a trabajar son unos cuantos archivos de audio y las transcripciones asociados. Siga los vínculos siguientes para empezar a crear una experiencia personalizada en texto a voz.

## <a name="whats-in-custom-voice"></a>¿Novedades de voz personalizados?

Antes de empezar con voz personalizada, necesitará una cuenta de Azure y una suscripción de servicios de voz. Una vez que haya creado una cuenta, puede preparar los datos, entrenar y probar sus modelos, evaluar la calidad de voz y finalmente implementar el modelo de voz personalizados.

El diagrama siguiente resalta los pasos para crear un modelo de voz personalizados mediante el portal de voz personalizados. Use los vínculos para obtener más información.

![Diagrama de arquitectura de voz personalizado](media/custom-voice/custom-voice-diagram.png)

1.  [Suscríbase y cree un proyecto](https://review.docs.microsoft.com/azure/cognitive-services/speech-service/how-to-custom-voice?branch=release-build-cogserv-speech-services#set-up-your-azure-account) : crear una cuenta de Azure y crear una suscripción de servicios de voz. Esta suscripción unificada proporciona acceso a voz a texto, texto a voz, traducción de voz y el portal de voz personalizado. A continuación, usar su suscripción de servicios de voz, crear su primer proyecto de voz personalizado.

2.  [Cargar datos](https://review.docs.microsoft.com/azure/cognitive-services/speech-service/how-to-customize-voice-font?branch=release-build-cogserv-speech-services#upload-your-datasets) -carga de datos (audio y texto) mediante el portal de voz personalizado o una API personalizada de voz. Desde el portal, puede investigar y evaluar las puntuaciones de pronunciación y relación señal / ruido. Para obtener más información, consulte [cómo preparar los datos de voz personalizado](how-to-custom-voice-prepare-data.md).

3.  [Entrenar el modelo](https://review.docs.microsoft.com/azure/cognitive-services/speech-service/how-to-customize-voice-font?branch=release-build-cogserv-speech-services#build-your-voice-font) : usar los datos para crear un modelo de voz personalizados. Puede entrenar un modelo en diferentes idiomas. Después del entrenamiento, la prueba del modelo y, si está satisfecho con el resultado, puede implementar el modelo.

4.  [Implementar el modelo](https://review.docs.microsoft.com/azure/cognitive-services/speech-service/how-to-customize-voice-font?branch=release-build-cogserv-speech-services#create-and-use-a-custom-endpoint) : crear un punto de conexión personalizado para el modelo de voz y usarlo para la síntesis de voz en sus productos, herramientas y aplicaciones.

## <a name="set-up-your-azure-account"></a>Configurar una cuenta de Azure

Para poder usar el portal de voz personalizado para crear un modelo personalizado, se necesita una suscripción de servicios de voz. Siga estas instrucciones para crear una suscripción de servicios de voz en Azure. Si no tiene una cuenta de Azure, puede registrarse para una nueva.  

Una vez que ha creado una cuenta de Azure y una suscripción de servicios de voz, deberá iniciar sesión en el portal de voz personalizado y conectarse a su suscripción.

1. Obtenga su clave de suscripción de servicios de voz desde el portal de Azure.
2. Inicie sesión en el [portal personalizado voz](https://aka.ms/custom-voice).
3. Seleccione la suscripción y crear un proyecto de voz.
4. Si desea cambiar a otra suscripción de voz, utilice el icono de engranaje situado en el panel de navegación superior.

> [!NOTE]
> El servicio de voz personalizado no es compatible con la clave de prueba gratuita de 30 días. Debe tener un F0 o una clave de S0 creada en Azure para poder usar el servicio.

## <a name="how-to-create-a-project"></a>Cómo crear un proyecto

Contenido, como datos, modelos, las pruebas y los puntos de conexión se organizan en **proyectos** en el portal de voz personalizado. Cada proyecto es específica de un lenguaje de país y el sexo de la voz que desea crear. Por ejemplo, puede crear un proyecto para una voz femenina para bots de chat de su centro de llamadas que utiliza a inglés de Estados Unidos (en-US).

Para crear su primer proyecto, seleccione el **Text-to-Speech/Custom voz** y, después, haga clic en **nuevo proyecto**. Siga las instrucciones proporcionadas por el Asistente para crear el proyecto. Después de crear un proyecto, verá cuatro pestañas: **Datos**, **entrenamiento**, **pruebas**, y **implementación**. Use los vínculos proporcionados en [pasos siguientes](https://review.docs.microsoft.com/azure/cognitive-services/speech-service/how-to-custom-voice?branch=release-build-cogserv-speech-services#next-steps) para obtener información sobre cómo usar cada pestaña.

## <a name="next-steps"></a>Pasos siguientes

- [Preparar los datos de voz personalizado](how-to-custom-voice.md)
- [Creación de una voz personalizada](how-to-custom-voice-create-voice.md)
- [Guía: Registre sus ejemplos de voz](record-custom-voice-samples.md)
