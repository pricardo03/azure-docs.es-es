---
title: Prueba gratuita del servicio Voz
description: Descubra cómo puede probar el servicio Voz sin costo alguno.
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: v-jerkin
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 09/24/2018
ms.author: v-jerkin
ms.openlocfilehash: 1615e3a492cd91fb998680094f131db454945303
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2018
ms.locfileid: "47182576"
---
# <a name="try-the-speech-service-for-free"></a>Prueba gratuita del servicio Voz

Comenzar con el servicio Voz es fácil y asequible. Una prueba gratuita de 30 días permite descubrir lo que el servicio puede hacer y decidir si es adecuado para las necesidades de la aplicación.

Si necesita más tiempo, regístrese para obtener una cuenta de Microsoft Azure: incluye 200 USD en crédito de servicio que puede aplicar a una suscripción del servicio Voz de pago durante 30 días.

Por último, el servicio Voz ofrece un plan gratuito de bajo volumen adecuado para el desarrollo de aplicaciones. Puede mantener esta suscripción gratuita incluso después de que expire el crédito de servicio.

## <a name="free-trial"></a>Evaluación gratuita

La evaluación gratuita de 30 días le da acceso al plan de tarifa estándar durante un tiempo limitado.

Para registrarse a fin de obtener una evaluación gratuita de 30 días:

1. Vaya a [Pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/).

1. Seleccione la pestaña **Speech API**.

   ![Pestaña Servicios de voz](media/index/try-speech-api-free-trial1.png)
   
1. En **Servicios de voz**, seleccione el botón **Obtener clave de API**.

   ![Clave de API](media/index/try-speech-api-free-trial2.png)

1. Acepte los términos y seleccione la configuración regional en el menú desplegable.

   ![Aceptación de los términos](media/index/try-speech-api-free-trial3.png)

1. Inicie sesión con su cuenta de Microsoft, Facebook, LinkedIn o GitHub.

    Puede registrarse para obtener una cuenta Microsoft gratuita en el [portal de cuentas de Microsoft](https://account.microsoft.com/account). Para empezar, haga clic en **Iniciar sesión con Microsoft** y, luego, cuando se le pida que inicie sesión, haga clic en **Crear una**. Siga los pasos para crear y comprobar la nueva cuenta Microsoft.

Después de iniciar sesión en Pruebe Cognitive Services, comienza la evaluación gratuita. En la página web que se muestra, se enumeran todos los servicios de Cognitive Services para los que actualmente tiene suscripciones de prueba. Junto a **Servicios de voz** se muestran dos claves de suscripción. Puede usar cualquiera de ellas en las aplicaciones.

> [!NOTE]
> Todas las suscripciones de prueba están en la región Oeste de EE. UU. Cuando realice solicitudes, asegúrese de usar el punto de conexión `westus`.

## <a name="new-azure-account"></a>Nueva cuenta de Azure

Las cuentas nuevas de Azure reciben un crédito de servicio de 200 USD que está disponible un máximo de 30 días. Este crédito se puede usar para explorar más a fondo el servicio de voz o para comenzar a desarrollar aplicaciones.

Para registrarse y obtener una nueva cuenta de Azure, vaya a la [página de suscripción a Azure](https://azure.microsoft.com/free/ai/), haga clic en **Comenzar gratis** y cree una nueva cuenta de Azure con su cuenta Microsoft.

Puede registrarse para obtener una cuenta Microsoft gratuita en el [portal de cuentas de Microsoft](https://account.microsoft.com/account). Para empezar, haga clic en **Iniciar sesión con Microsoft** y, luego, cuando se le pida que inicie sesión, haga clic en **Crear una**. Siga los pasos para crear y comprobar la nueva cuenta Microsoft.

Después de crear la cuenta de Azure, siga los pasos de la sección siguiente para iniciar una suscripción al servicio de voz.

## <a name="create-a-speech-resource-in-azure"></a>Crear un recurso de voz en Azure

Para agregar un recurso de servicio de voz (plan gratuito o de pago) a la cuenta de Azure:

1. Inicie sesión en [Azure Portal](https://ms.portal.azure.com/) con la cuenta Microsoft.

1. Seleccione **Crear un recurso** en la parte superior izquierda del portal.

    ![Creación de un recurso](media/index/try-speech-api-create-speech1.png)

1. En la ventana **Nuevo**, busque **voz**.

1. En los resultados de la búsqueda, seleccione **Voz**.

    ![Selección de Voz](media/index/try-speech-api-create-speech2.png)

1. En **Voz**, seleccione el botón **Crear**.

    ![Selección del botón Crear](media/index/try-speech-api-create-speech3.png)

1. En **Crear**, escriba:

    * Un nombre para el recurso nuevo. El nombre ayuda a distinguir entre varias suscripciones al mismo servicio.
    * Elija la suscripción de Azure a la que esté asociado el recurso nuevo para determinar cómo se facturan las tarifas.
    * Elija la región donde se va a usar el recurso. En la actualidad, el servicio Voz está disponible en las regiones de Asia Oriental, Europa del Norte y Oeste de EE. UU.
    * Elija un plan de tarifa de pago o gratuito. Haga clic en **Ver todos los detalles de los precios** para obtener información completa sobre los precios y las cuotas de uso de cada plan.
    * Cree un nuevo grupo de recursos para esta suscripción de voz o asígnela a un grupo de recursos existente. Los grupos de recursos ayudan a mantener organizadas las distintas suscripciones de Azure.
    * Para obtener acceso fácilmente a la suscripción en el futuro, active la casilla **Anclar al panel**.
    * Seleccione **Crear**.

    ![Selección del botón Crear](media/index/try-speech-api-create-speech4.png)

    La creación e implementación del recurso de voz nuevo puede tardar unos instantes. Seleccione **Inicio rápido** para ver información sobre el recurso nuevo.

    ![Panel Inicio rápido](media/index/try-speech-api-create-speech5.png)

1. En **Inicio rápido**, haga clic en el vínculo **Claves** del paso 1 para mostrar las claves de suscripción. Cada suscripción tiene dos claves; puede usar cualquiera de ellas en la aplicación. Seleccione el botón situado junto a cada clave para copiarla en el Portapapeles a fin de pegarla en el código.

> [!NOTE]
> Puede crear un número ilimitado de suscripciones de plan estándar en una o varias regiones. Pero solo puede crear una suscripción de plan gratuito. Las implementaciones de modelo del plan gratuito que permanezcan inactivas durante siete días se retirarán automáticamente.

## <a name="switch-to-a-new-subscription"></a>Cambiar a una nueva suscripción

Para cambiar de una suscripción a otra, por ejemplo, cuando la evaluación gratuita expire o al publicar la aplicación, sustituya la clave de región y suscripción del código por la clave de región y suscripción del nuevo recurso de Azure.

> [!NOTE]
> Las claves de evaluación gratuita se crean en la región Oeste de EE. UU. (`westus`). Una suscripción creada mediante el panel de Azure puede estar en alguna otra región, si así lo decide.

* Si la aplicación usa [Speech SDK](speech-sdk.md), proporcione el código de región, por ejemplo, `westus`, al crear una configuración de voz.
* Si la aplicación usa una de las [API de REST](rest-apis.md) del servicio de voz, la región forma parte del URI del punto de conexión que se emplea al realizar solicitudes.

Las claves creadas para una región son válidas únicamente en esa región. Si intenta usarlas con otras regiones se producen errores de autenticación.

## <a name="next-steps"></a>Pasos siguientes

Realice alguno de los inicios rápidos de 10 minutos o visite los ejemplos de SDK:

> [!div class="nextstepaction"]
> [Guía de inicio rápido: Reconocimiento de voz en C#](quickstart-csharp-dotnet-windows.md)
> [Ejemplos de Speech SDK](speech-sdk.md#get-the-samples)
