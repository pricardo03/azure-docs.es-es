---
title: 'Acerca del SDK de dispositivos de voz: servicios de Voz'
titleSuffix: Azure Cognitive Services
description: Introducción al SDK de dispositivos de voz. El servicio de Voz funciona con una amplia variedad de dispositivos y orígenes de audio. Ahora puede llevar las aplicaciones de voz al siguiente nivel con hardware y software coincidente. El SDK de dispositivos de voz es una biblioteca pre-ajustada que está emparejada con kits de desarrollo específicos integrados de la matriz del micrófono.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.component: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 44e993375b17960ad5d9ced11d37ed3701a58f73
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53101745"
---
# <a name="about-the-speech-devices-sdk-preview"></a>Acerca del SDK de dispositivos de voz (versión preliminar)

El [servicio Voz](overview.md) funciona con una amplia variedad de dispositivos y orígenes de audio. Ahora puede llevar las aplicaciones de voz al siguiente nivel con hardware y software coincidente. El SDK de dispositivos de voz es una biblioteca preajustada que está emparejada con kits de desarrollo específicos integrados de la matriz del micrófono.

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
