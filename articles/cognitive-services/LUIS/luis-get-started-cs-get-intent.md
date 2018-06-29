---
title: Tutorial para aprender a llamar a una aplicación de Language Understanding Intelligent Service (LUIS) mediante C# | Microsoft Docs
description: En este tutorial, aprenderá a llamar a una aplicación de LUIS mediante C#.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: tutorial
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 0416d19d27810a2ab8eeb20e16b2f921fc7826ee
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36263496"
---
# <a name="tutorial-call-a-luis-endpoint-using-c"></a>Tutorial: Llamada a un punto de conexión de LUIS mediante C#

Pase las expresiones a un punto de conexión de LUIS y obtenga las entidades e intenciones.

<!-- green checkmark -->
> [!div class="checklist"]
> * Creación de una suscripción de LUIS y copia del valor de la clave para su uso posterior
> * Visualización de los resultados de punto de conexión de LUIS desde el explorador a la aplicación de ejemplo pública de IoT
> * Creación de una aplicación de consola de Visual Studio C# para realizar una llamada HTTPS a un punto de conexión de LUIS

<!-- link to free account --> Para este artículo, necesita una cuenta de [LUIS][LUIS] gratuita para crear la aplicación de LUIS.

## <a name="create-luis-subscription-key"></a>Creación de una clave de suscripción de LUIS
1. En primer lugar, hay que crear una [cuenta de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) en Azure Portal. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

2. Inicie sesión en Azure Portal en https://portal.azure.com. 

3. Para obtener una clave, siga los pasos de [Creación de claves de suscripción mediante Azure](./luis-how-to-azure-subscription.md).

4. Vuelva al sitio web de [LUIS](luis-reference-regions.md). Inicie sesión con la cuenta de Azure. 

    [![](media/luis-get-started-cs-get-intent/app-list.png "Captura de pantalla de una lista de aplicaciones")](media/luis-get-started-cs-get-intent/app-list.png)

## <a name="understand-what-luis-returns"></a>Descripción de lo que LUIS devuelve

Para comprender lo que devuelve una aplicación de LUIS, puede pegar la dirección URL de una aplicación de LUIS de ejemplo en una ventana del explorador. La aplicación de ejemplo es una aplicación de IoT que detecta si el usuario desea encender o apagar las luces.

1. El punto de conexión de la aplicación de ejemplo está en este formato: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=<YOUR_API_KEY>&verbose=false&q=turn%20on%20the%20bedroom%20light` Copie la dirección URL y sustituya la clave de suscripción por el valor del campo `subscription-key`.
2. Pegue la dirección URL en una ventana del explorador y presione ENTRAR. El explorador muestra el resultado JSON que indica que LUIS ha detectado la intención `HomeAutomation.TurnOn` y la entidad `HomeAutomation.Room` con el valor `bedroom`.

    ![El resultado JSON detecta la intención TurnOn](./media/luis-get-started-cs-get-intent/turn-on-bedroom.png)
3. Cambie el valor del parámetro `q=` en la dirección URL por `turn off the living room light` y presione ENTRAR. Ahora, el resultado indica que LUIS ha detectado la intención `HomeAutomation.TurnOff` y la entidad `HomeAutomation.Room` con el valor `living room`. 

    ![El resultado JSON detecta la intención TurnOff](./media/luis-get-started-cs-get-intent/turn-off-living-room.png)


## <a name="consume-a-luis-result-using-the-endpoint-api-with-c"></a>Uso de un resultado de LUIS mediante la API de punto de conexión con C# 

Puede usar C# para acceder a los mismos resultados que vio en la ventana del explorador del paso anterior. 

1. Cree una aplicación de consola en Visual Studio. Copie el código siguiente y guárdelo en un archivo *.cs:
    
   [!code-csharp[Console app code that calls a LUIS endpoint](~/samples-luis/documentation-samples/endpoint-api-samples/csharp/Program.cs)]
1. Sustituya el valor de la variable `subscriptionKey` por su clave de suscripción de LUIS.

3. En el proyecto de Visual Studio, agregue una referencia a **System.Web**.

4. Ejecución de la aplicación de consola. Muestra el mismo código JSON que vio anteriormente en la ventana del explorador.

![Ventana de la consola que muestra el resultado JSON de LUIS](./media/luis-get-started-cs-get-intent/console-turn-on.png)

## <a name="clean-up-resources"></a>Limpieza de recursos
Los dos recursos creados en este tutorial son la clave de suscripción de LUIS y el proyecto de C#. Elimine la clave de suscripción de LUIS en Azure Portal. Cierre el proyecto de Visual Studio y quite el directorio del sistema de archivos. 

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Adición de grabaciones de voz](luis-get-started-cs-add-utterance.md)

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website