---
title: Prueba gratuita del servicio Voz | Microsoft Docs
description: Descubra cómo puede probar el servicio Voz sin costo alguno.
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 05/17/2018
ms.author: v-jerkin
ms.openlocfilehash: 3feef04694336d9173b419285a96fcaef543e18f
ms.sourcegitcommit: 944d16bc74de29fb2643b0576a20cbd7e437cef2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2018
ms.locfileid: "35383385"
---
# <a name="try-the-speech-service-for-free"></a>Prueba gratuita del servicio Voz

Comenzar con el servicio Voz es fácil y asequible. Una prueba gratuita de 30 días permite descubrir lo que el servicio puede hacer y decidir si es adecuado para las necesidades de la aplicación.

Si necesita más tiempo, regístrese para obtener una cuenta de Microsoft Azure: incluye 200 USD en crédito de servicio que puede aplicar a una suscripción del servicio Voz de pago durante 30 días.

Por último, el servicio Voz ofrece un plan gratuito de bajo volumen adecuado para el desarrollo de aplicaciones. Puede mantener esta suscripción gratuita incluso después de que expire el crédito de servicio.

## <a name="free-trial"></a>Evaluación gratuita

La evaluación gratuita de 30 días le da acceso al plan de tarifa estándar S0 durante un tiempo limitado. Para suscribirse para obtener una prueba gratuita de 30 días, siga estos pasos.

1. Vaya a la página [Pruebe Cognitive Services](https://azure.microsoft.com/try/cognitive-services/).

1. Cambie a la pestaña Speech (Voz) y haga clic en el botón **Obtener clave de API** situado junto a "Servicios de voz".

   ![Pestaña Servicios de voz](media/index/try-speech-api-free-trial1.png)<br>
   ![Clave de API](media/index/try-speech-api-free-trial2.png)

3. Acepte los términos y seleccione la configuración regional en el menú desplegable.

   ![Aceptación de los términos](media/index/try-speech-api-free-trial3.png)

4. Inicie sesión con su cuenta de Microsoft, Facebook, LinkedIn o GitHub. O bien, puede registrarse para obtener una cuenta gratuita de Microsoft:

    * Vaya al [portal Cuenta de Microsoft](https://account.microsoft.com/account).
    * Haga clic en **Iniciar sesión con Microsoft**.

    ![Iniciar sesión](media/index/try-speech-api-free-trial4.png)

    * Cuando se le pida que inicie sesión, haga clic en "Crear una".

    ![Crear nueva cuenta](media/index/try-speech-api-free-trial5.png)

    * En los pasos siguientes, escriba su dirección de correo electrónico o número de teléfono, asigne una contraseña y siga las instrucciones para comprobar la nueva cuenta de Microsoft.

Después de iniciar la sesión, comienza la prueba gratuita. En la página web que se muestra se enumeran todos los Cognitive Services para los que actualmente tiene suscripciones de prueba. Junto a "Servicios de voz" se muestran dos claves de suscripción. Puede usar cualquiera de las claves en las aplicaciones.

> [!NOTE]
> Todas las suscripciones de prueba están en la región Oeste de EE. UU. Asegúrese de usar el punto de conexión correspondiente a su región al realizar las solicitudes.

## <a name="new-azure-account"></a>Nueva cuenta de Azure

Las cuentas nuevas de Azure reciben un crédito de servicio de 200 USD que dura hasta 30 días. Este crédito se puede usar para explorar más el servicio de voz o comenzar el desarrollo de aplicaciones.

Para suscribirse para obtener una cuenta nueva de Azure, siga estos pasos.

1. Vaya a la [página de registro de Azure](https://azure.microsoft.com/free/ai/). 

1. Haga clic en **Empiece gratis**.

    ![Inicio gratuito](media/index/try-speech-api-new-azure1.png)

3. Inicie sesión con la cuenta de Microsoft. Si no tiene una:

    * Vaya al [portal Cuenta de Microsoft](https://account.microsoft.com/account).
    * Haga clic en **Iniciar sesión con Microsoft**.
    * Cuando se le pida que inicie sesión, haga clic en "Crear una".
    * En los pasos siguientes, escriba su dirección de correo electrónico o número de teléfono, asigne una contraseña y siga las instrucciones para comprobar la nueva cuenta de Microsoft.

1. Escriba el resto de la información solicitada para suscribirse para obtener una cuenta. Especifique su país y el nombre, y proporcione una dirección de correo electrónico y número de teléfono.

    ![Escribir la información](media/index/try-speech-api-new-azure2.png)

    Verifique su identidad por teléfono y proporcionando un número de tarjeta de crédito, y después acepte el contrato de usuario de Azure. (La tarjeta de crédito no recibirá ningún cargo).

    ![Aceptación del contrato](media/index/try-speech-api-new-azure3.png)

Se crea la cuenta de Azure gratuita. Siga los pasos descritos en la sección siguiente para iniciar una suscripción al servicio de voz.

## <a name="create-a-speech-resource-in-azure"></a>Crear un recurso de voz en Azure

Para agregar un recurso de servicio de voz a la cuenta de Azure, siga estos pasos.

1. Inicie sesión en [Azure Portal](https://ms.portal.azure.com/) con la cuenta de Microsoft.

1. Haga clic en **Crear un recurso** (el icono **+** de color verde) en la parte superior izquierda del portal.

    ![Crear el recurso](media/index/try-speech-api-create-speech1.png)

1. En la ventana Nuevos, busque Voz.

    ![Haga clic en Voz](media/index/try-speech-api-create-speech2.png)

1. En los resultados de la búsqueda, haga clic en "Voz (versión preliminar)".

1. Haga clic en el botón **Crear** situado en la parte inferior del panel del servicio de voz.

    ![Click Create](media/index/try-speech-api-create-speech3.png)

1. En el panel Crear, escriba:

    * Un nombre para el recurso nuevo. El nombre ayuda a distinguir entre varias suscripciones al mismo servicio.
    * Elija la suscripción de Azure a la que esté asociado el recurso nuevo para determinar cómo se facturan las tarifas.
    * Elija la región donde se va a usar el recurso. En la actualidad, el servicio Voz está disponible en las regiones de Asia Oriental, Europa del Norte y Oeste de EE. UU.
    * Elija el plan de tarifa, ya sea F0 (suscripción gratuita limitada) o S0 (suscripción estándar). Haga clic en **Ver todos los detalles de los precios** para obtener información completa sobre los precios y las cuotas de uso de cada plan.
    * Cree un grupo de recursos para esta suscripción de voz o asígnela a otro existente. Los grupos de recursos ayudan a mantener organizadas las distintas suscripciones de Azure.
    * Para obtener acceso fácilmente a la suscripción en el futuro, active la casilla **Anclar al panel**.
    * Haga clic en **Create** (Crear).

    ![Haga clic en Crear en el panel](media/index/try-speech-api-create-speech4.png)

    La creación e implementación del recurso de voz nuevo puede tardar unos instantes. Aparece el panel Inicio rápido con información sobre el recurso nuevo.

    ![Panel Inicio rápido](media/index/try-speech-api-create-speech5.png)

1. Haga clic en el vínculo **Claves** del Paso 1 en el panel Inicio rápido para mostrar las claves de suscripción. Cada suscripción tiene dos claves; puede usar cualquiera de ellas en la aplicación. Haga clic en el botón situado junto a cada clave para copiarla en el Portapapeles para pegarla en el código.

> [!NOTE]
> Puede crear cualquier número de suscripciones de plan estándar en una o varias regiones. Pero solo puede crear una suscripción de plan gratuito.

## <a name="next-steps"></a>Pasos siguientes

Descargue un SDK y ejemplos de código para experimentar el servicio de voz.

> [!div class="nextstepaction"]
> [SDK de Voz](speech-sdk.md)

> [!div class="nextstepaction"]
> [Código de ejemplo](samples.md)
