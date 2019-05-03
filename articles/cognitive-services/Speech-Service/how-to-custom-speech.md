---
title: Empezar a trabajar con la conversión de voz personalizados - servicios de voz
titlesuffix: Azure Cognitive Services
description: Conversión personalizada de voz es un conjunto de herramientas en línea que le permiten evaluar y mejorar la precisión de texto a voz de Microsoft para sus productos, herramientas y aplicaciones. Todo que lo necesario para empezar a trabajar son un puñado de archivos de audio de prueba. Siga los vínculos siguientes para empezar a crear una experiencia personalizada de voz a texto.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: erhopf
ms.openlocfilehash: f2441d5660ed84a8680c1861e2e20a1a17ee4762
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65025836"
---
# <a name="what-is-custom-speech"></a>¿Qué es la voz personalizada?

[Conversión personalizada de voz](https://aka.ms/custom-speech) es un conjunto de herramientas en línea que le permiten evaluar y mejorar la precisión de texto a voz de Microsoft para sus productos, herramientas y aplicaciones. Todo que lo necesario para empezar a trabajar son un puñado de archivos de audio de prueba. Siga los vínculos siguientes para empezar a crear una experiencia personalizada de voz a texto.

## <a name="whats-in-custom-speech"></a>¿Qué es personalizado de voz?

Antes de hacer cualquier cosa con voz personalizada, necesitará una cuenta de Azure y una suscripción de servicios de voz. Cuando esté satisfecho con una cuenta, puede preparación de los datos, entrenar y probar sus modelos, inspeccionar la calidad del reconocimiento, evaluar la precisión y en última instancia, implementar y utilizar el modelo de voz a texto personalizado.

Este diagrama resalta las partes que componen el portal de voz personalizado. Use los vínculos siguientes para obtener más información sobre cada paso.

![Se resaltan los distintos componentes que constituyen el portal de voz personalizado.](./media/custom-speech/custom-speech-overview.png)

1. [Suscríbase y cree un proyecto](#set-up-your-azure-account) : crear una cuenta de Azure y suscribirse a los servicios de voz. Esta suscripción unificada proporciona acceso a voz a texto, texto a voz, traducción de voz y el portal de voz personalizado. A continuación, usar su suscripción de servicios de voz, crear su primer proyecto de voz personalizado.

2. [Cargar datos de prueba](how-to-custom-speech-test-data.md) -los datos de prueba de carga (archivos de audio) para evaluar speech-to-text de Microsoft oferta para sus productos, herramientas y aplicaciones.

3. [Inspeccionar la calidad del reconocimiento](how-to-custom-speech-inspect-data.md) -usar el portal de voz personalizados para reproducir sonido cargado e inspeccione la calidad del reconocimiento de voz de los datos de prueba. Para las medidas cuantitativas, consulte [inspeccionar datos](how-to-custom-speech-inspect-data.md).

4. [Evalúe su precisión](how-to-custom-speech-evaluate-data.md) -evaluar la precisión del modelo de texto a voz. El portal de Custom Speech proporcionará un *tasa de errores de Word*, que puede utilizarse para determinar si se requiere aprendizaje adicional. Si está satisfecho con la precisión, puede usar directamente las API de servicios de voz. Si desea mejorar la precisión de una media relativa del 5 al 20%, use el **entrenamiento** ficha en el portal para cargar los datos de entrenamiento adicionales, como humanos etiquetados transcripciones y texto relacionado.

5. [Entrenar el modelo](how-to-custom-speech-train-model.md) : mejorar la precisión del modelo de texto a voz proporcionando las transcripciones escritas (10-1.000 horas) y el texto relacionado (< 200 MB), junto con el audio de los datos de pruebas. Estos datos ayudan a entrenar el modelo de texto a voz. Después del entrenamiento, vuelva a probar, y si está satisfecho con el resultado, puede implementar el modelo.

6. [Implementar el modelo](how-to-custom-speech-deploy-model.md) : crear un punto de conexión personalizado para el modelo de voz a texto y usarla en sus aplicaciones, herramientas o productos.

## <a name="set-up-your-azure-account"></a>Configurar una cuenta de Azure

Para poder usar el portal de voz personalizado para crear un modelo personalizado, se necesita una suscripción de servicios de voz. Siga estas instrucciones para crear una suscripción estándar de servicios de voz: [Crear una suscripción de voz](https://docs.microsoft.com/en-us/azure/cognitive-services/speech-service/get-started#new-azure-account).

> [!NOTE]
> Asegúrese de crear las suscripciones de estándar (S0), no se admiten suscripciones de prueba gratuita (F0).

Una vez que ha creado una cuenta de Azure y una suscripción de servicios de voz, deberá iniciar sesión en el portal de voz personalizado y conectarse a su suscripción.

1. Obtenga su clave de suscripción de servicios de voz desde el portal de Azure.
2. Inicie sesión en el [portal de voz personalizado](https://aka.ms/custom-speech).
3. Seleccione la suscripción que necesita para trabajar en y crear un proyecto de voz.
4. Si desea modificar su suscripción, utilice el **engranaje** icono se encuentra en el panel de navegación superior.

## <a name="how-to-create-a-project"></a>Cómo crear un proyecto

Contenido, como datos, modelos, las pruebas y los puntos de conexión se organizan en **proyectos** en el portal de voz personalizado. Cada proyecto es específica a un dominio y el país y el idioma. Por ejemplo, puede crear un proyecto para centros de llamadas que usan el inglés de Estados Unidos.

Para crear su primer proyecto, seleccione el **voz de voz a texto/personalizada**, a continuación, haga clic en **nuevo proyecto**. Siga las instrucciones proporcionadas por el Asistente para crear el proyecto. Después de crear un proyecto, debería ver cuatro pestañas: **Datos**, **pruebas**, **entrenamiento**, y **implementación**. Use los vínculos proporcionados en [pasos siguientes](#next-steps) para obtener información sobre cómo usar cada pestaña.

## <a name="next-steps"></a>Pasos siguientes

* [Preparar y probar los datos](how-to-custom-speech-test-data.md)
* [Inspeccione los datos](how-to-custom-speech-inspect-data.md)
* [Evaluar los datos](how-to-custom-speech-evaluate-data.md)
* [Entrenar el modelo.](how-to-custom-speech-train-model.md)
* [Implementar el modelo](how-to-custom-speech-deploy-model.md)
