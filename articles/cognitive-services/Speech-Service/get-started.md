---
title: Prueba gratuita del servicio Voz
titleSuffix: Azure Cognitive Services
description: Comenzar con el servicio Voz es fácil y asequible. Hay dos opciones disponibles gratis para que pueda descubrir lo que puede hacer el servicio y decidir si es adecuado para sus necesidades.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.custom: seodec18, seo-javascript-october2019
ms.openlocfilehash: 30bdbf9fa0ea346892622c3e7f24f9f31652a650
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2019
ms.locfileid: "74280485"
---
# <a name="try-the-speech-service-for-free"></a>Prueba gratuita del servicio Voz

Usar el servicio Voz es un proceso fácil y asequible. Hay dos opciones disponibles gratis para que pueda descubrir lo que puede hacer el servicio y decidir si es adecuado para sus necesidades:

- Obtener una evaluación gratuita sin proporcionar la información de la tarjeta de crédito (debe tener una cuenta de Azure existente)
- Crear una cuenta de Azure sin cargo alguno durante un período de prueba (se requiere la información de la tarjeta de crédito)

En este artículo, elegirá una de estas opciones que mejor se adapte a sus necesidades.

> [!NOTE]
> El servicio Voz tiene dos niveles de servicio: gratis y suscripción, que tienen diferentes limitaciones y ventajas. Si se registra para obtener una cuenta gratuita de Azure, obtendrá 200 USD en crédito de servicios que puede aplicar a una suscripción del servicio Voz de pago durante 30 días.
>
> Si usa el nivel gratis del servicio Voz de bajo volumen, puede conservar esta suscripción gratuita incluso después de que expire la evaluación gratuita o el crédito del servicio.
>
> Para más información, consulte [Precios de Cognitive Services: Speech Services](https://azure.microsoft.com/pricing/details/cognitive-services/speech-services/).

## <a name="try-the-speech-service-without-credit-card-info"></a>Prueba del servicio Voz sin la información de la tarjeta de crédito

Aunque se recomienda probar el servicio Voz siguiendo las instrucciones de la sección siguiente, puede que prefiera las instrucciones de esta sección si se aplica lo siguiente:

- Ya tiene una cuenta activa de Azure.
- Quiere evaluar el servicio Voz sin crear una nueva cuenta de Azure.
- Prefiere que no se requiera ninguna tarjeta de crédito y que no se guarde ningún dato después del período de prueba.

> [!NOTE]
> El período de prueba se iniciará inmediatamente una vez que se hayan completado los pasos siguientes.

1. Vaya a [Pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/).
1. Seleccione la pestaña **Speech API**.
1. Elija **Obtener clave de API**.

Se le presentarán opciones de facturación. Elija la opción gratis y, después, lea y apruebe el acuerdo de usuario. Se le presentarán claves que puede usar para probar el servicio Voz durante un tiempo limitado.

## <a name="try-the-speech-service-using-a-new-azure-account"></a>Prueba del servicio Voz con una nueva cuenta de Azure

Para suscribirse a una nueva cuenta de Azure, necesitará una cuenta de Microsoft. Si no tiene un cuenta de Microsoft, puede registrarse para obtener una gratuita en el [portal de la cuenta de Microsoft](https://account.microsoft.com/account). Seleccione **Iniciar sesión con Microsoft** y, luego, cuando se le pida que inicie sesión, seleccione **Crear una cuenta de Microsoft**. Siga los pasos para crear y comprobar la nueva cuenta Microsoft.

Cuando tenga la cuenta de Microsoft, vaya a la [página de suscripción a Azure](https://azure.microsoft.com/free/ai/), seleccione **Comenzar gratis** y cree una cuenta de Azure con su cuenta de Microsoft.

### <a name="create-a-speech-resource-in-azure"></a>Crear un recurso de voz en Azure

> [!NOTE]
> Puede crear un número ilimitado de suscripciones de plan estándar en una o varias regiones. Pero solo puede crear una suscripción de plan gratuito. Las implementaciones de modelo del plan gratuito que permanezcan inactivas durante siete días se retirarán automáticamente.

Para agregar un recurso de servicio de voz (plan gratuito o de pago) a la cuenta de Azure:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) con la cuenta Microsoft.

1. Seleccione **Crear un recurso** en la parte superior izquierda del portal. Si no ve **Crear un recurso**, siempre puede encontrarlo mediante la selección del menú contraído en la parte superior izquierda:

   ![botón de navegación contraído](media/index/collapsed-nav.png)

1. En la ventana **Nuevo**, escriba "speech" en el cuadro de búsqueda y presione ENTRAR.

1. En los resultados de la búsqueda, seleccione **Voz**.

   ![resultados de la búsqueda de voz](media/index/speech-search.png)

1. Seleccione **Crear** y, después:

   - Dé un nombre único al nuevo recurso. El nombre ayuda a distinguir entre varias suscripciones al mismo servicio.
   - Elija la suscripción de Azure a la que esté asociado el recurso nuevo para determinar cómo se facturan las tarifas.
   - Elija la [región](regions.md) donde se va a usar el recurso.
   - Elija un plan de tarifa de pago (S0) o gratis (F0). Para completar la información sobre los precios y las cuotas de uso de cada plan, seleccione **Ver todos los detalles de los precios**.
   - Cree un nuevo grupo de recursos para esta suscripción de voz o asígnela a un grupo de recursos existente. Los grupos de recursos ayudan a mantener organizadas las distintas suscripciones de Azure.
   - Seleccione **Crear**. Esto le llevará a la información general de la implementación y mostrará mensajes del progreso de la implementación.

La implementación del recurso de voz nuevo puede tardar unos instantes. Una vez completada la implementación, seleccione **Ir al recurso** y, en el panel de navegación izquierdo, seleccione **Claves** para mostrar las claves de suscripción del servicio Voz. Cada suscripción tiene dos claves; puede usar cualquiera de ellas en la aplicación. Para copiar y pegar rápidamente una clave en el editor de código o en otra ubicación, seleccione el botón Copiar que se encuentra junto a cada clave, cambie de ventana para pegar el contenido del portapapeles en la ubicación deseada.

> [!IMPORTANT]
> Estas claves de suscripción se usan para tener acceso a la API de Cognitive Services. No comparta las claves. Almacénelas de forma segura, por ejemplo, con Azure Key Vault. También se recomienda regenerar estas claves periódicamente. Solo se necesita una clave para realizar una llamada API. Al volver a generar la primera clave, puede usar la segunda clave para seguir teniendo acceso al servicio.

## <a name="switch-to-a-new-subscription"></a>Cambiar a una nueva suscripción

Para cambiar de una suscripción a otra, por ejemplo, cuando la evaluación gratuita expire o al publicar la aplicación, sustituya la clave de región y suscripción del código por la clave de región y suscripción del nuevo recurso de Azure.

## <a name="about-regions"></a>Acerca de las regiones

- Si la aplicación usa [Speech SDK](speech-sdk.md), proporcione el código de región, por ejemplo, `westus`, al crear una configuración de voz.
- Si la aplicación usa una de las [API de REST](rest-apis.md) del servicio de voz, la región forma parte del URI del punto de conexión que se emplea al realizar solicitudes.
- Las claves creadas para una región son válidas únicamente en esa región. Si intenta usarlas con otras regiones se producen errores de autenticación.

## <a name="next-steps"></a>Pasos siguientes

Realice alguno de los inicios rápidos de 10 minutos o visite los ejemplos de SDK:

> [!div class="nextstepaction"]
> [Inicio rápido: Reconocimiento de voz en C#](~/articles/cognitive-services/Speech-Service/quickstarts/speech-to-text-from-microphone.md?pivots=programming-language-csharp&tabs=dotnet)
> [Ejemplos del SDK de Voz](speech-sdk.md#get-the-samples)
