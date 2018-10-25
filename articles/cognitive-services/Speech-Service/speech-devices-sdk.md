---
title: Introducción a Speech Devices SDK
description: Obtenga una introducción al SDK de dispositivos de voz.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: erhopf
ms.service: cognitive-services
ms.component: speech
ms.topic: article
ms.date: 05/07/2018
ms.author: erhopf
ms.openlocfilehash: ba91d5fd556cdc189f6303ac216c8fdd9495c74b
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2018
ms.locfileid: "49165427"
---
# <a name="about-the-speech-devices-sdk-preview"></a>Acerca del SDK de dispositivos de voz (versión preliminar)

El [servicio Microsoft Speech](overview.md) funciona con una amplia variedad de dispositivos y orígenes de audio. Ahora puede llevar las aplicaciones de voz al siguiente nivel con hardware y software coincidente. El SDK de dispositivos de voz es una biblioteca preajustada que está emparejada con kits de desarrollo específicos integrados de la matriz del micrófono. 

El SDK de dispositivos de voz puede ayudarle a:
* Probar rápidamente nuevos escenarios de voz.
* Integrar más fácilmente el servicio de voz basado en la nube en el dispositivo.
* Crear una experiencia de usuario excepcional para sus clientes. 

El SDK de dispositivos de voz consume el [SDK de voz](speech-sdk.md). Usa el SDK de voz para enviar el audio que procesa nuestro algoritmo de procesamiento avanzado de audio desde la matriz del micrófono del dispositivo al [servicio de voz](overview.md). Usa audio multicanal para proporcionar un [reconocimiento de voz](speech-to-text.md) de campo lejano más preciso mediante la supresión de ruido, la cancelación del eco, la formación de haces y la eliminación de la reverberación.

También puede usar el SDK de dispositivos de voz para compilar dispositivos ambientales que tienen una [palabra de reactivación personalizada](speech-devices-sdk-create-kws.md) propia, de modo que la indicación que inicia la interacción del usuario es única para su marca. 

El SDK de dispositivos de voz facilita una variedad de escenarios habilitados para la voz, como sistemas de pedidos de comida para llevar, asistentes para el establecimiento o el hogar, y altavoces inteligentes. Puede responder a los usuarios con texto, hablarles con una voz predeterminada o [personalizada](how-to-customize-voice-font.md), proporcionar resultados de búsqueda, [traducir](speech-translation.md) a otros idiomas y mucho más. Estamos deseando ver lo que ha compilado.

## <a name="development-kit-providers"></a>Proveedores del kit de desarrollo

Actualmente, estos completos diseños de referencia del sistema están disponibles: 

|||
|-|-|
|[![Logotipo de ROOBO](media/speech-devices-sdk/roobo-logo.png)](http://ddk.roobo.com/)|ROOBO proporciona soluciones de sistema de inteligencia artificial (IA) completas para electrodomésticos, automóviles, robots, juguetes y otros sectores. Los diseños de referencia de ROOBO reducen en gran medida el tiempo de comercialización de desarrollo mediante la integración con el servicio Microsoft Speech. [Visite ROOBO](http://ddk.roobo.com/).|

## <a name="next-steps"></a>Pasos siguientes

Para comenzar, obtenga una [cuenta gratuita de Azure](https://azure.microsoft.com/free/ai/) y regístrese para obtener el SDK de dispositivos de voz.

> [!div class="nextstepaction"]
> [Registrarse para obtener el SDK de dispositivos de voz](get-speech-devices-sdk.md)

