---
title: Guía de inicio rápido para aprender a llamar a una aplicación de Language Understanding (LUIS) mediante Python | Microsoft Docs
description: En esta guía de inicio rápido, aprenderá a llamar a una aplicación de LUIS mediante Python.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: quickstart
ms.date: 06/27/2018
ms.author: diberry
ms.openlocfilehash: bc7ae912d762a98c34b9a1b2d6a82d5630c4794b
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "43771752"
---
# <a name="quickstart-call-a-luis-endpoint-using-python"></a>Guía de inicio rápido: Llamada a un punto de conexión de LUIS mediante Python
En esta guía de inicio rápido, pasará expresiones a un punto de conexión de LUIS y obtendrá entidades e intenciones.

<!-- green checkmark -->
<!--
> [!div class="checklist"]
> * Create LUIS subscription and copy key value for later use
> * View LUIS endpoint results from browser to public sample IoT app
> * Create Visual Studio C# console app to make HTTPS call to LUIS endpoint
-->

Para este artículo, necesita una cuenta gratuita de [LUIS](luis-reference-regions.md#luis-website) para crear la aplicación.

<a name="create-luis-subscription-key"></a>
## <a name="create-luis-endpoint-key"></a>Creación de la clave de punto de conexión de LUIS
Para realizar llamadas a la aplicación de LUIS de ejemplo que se utiliza en este tutorial se necesita la clave de la API de Cognitive Services. 

Para obtener una clave de API, siga estos pasos: 

1. En primer lugar, hay que crear una [cuenta de Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) en Azure Portal. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

2. Inicie sesión en Azure Portal en https://portal.azure.com. 

3. Para obtener una clave, siga los pasos que se describen en [Creación de claves de punto de conexión con Azure](./luis-how-to-azure-subscription.md).

4. Vuelva al sitio web de [LUIS](luis-reference-regions.md) e inicie sesión con su cuenta de Azure. 

    [![](media/luis-get-started-node-get-intent/app-list.png "Captura de pantalla de una lista de aplicaciones")](media/luis-get-started-node-get-intent/app-list.png)

## <a name="understand-what-luis-returns"></a>Descripción de lo que LUIS devuelve

Para comprender lo que devuelve una aplicación de LUIS, puede pegar la dirección URL de una aplicación de LUIS de ejemplo en una ventana del explorador. La aplicación de ejemplo es una aplicación de IoT que detecta si el usuario desea encender o apagar las luces.

1. El punto de conexión de la aplicación de ejemplo está en este formato: `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=<YOUR_API_KEY>&verbose=false&q=turn%20on%20the%20bedroom%20light` Copie la dirección URL y sustituya la clave de punto de conexión por el valor del campo `subscription-key`.
2. Pegue la dirección URL en una ventana del explorador y presione ENTRAR. El explorador muestra el resultado JSON que indica que LUIS ha detectado la intención `HomeAutomation.TurnOn` y la entidad `HomeAutomation.Room` con el valor `bedroom`.

    ![El resultado JSON detecta la intención TurnOn](./media/luis-get-started-node-get-intent/turn-on-bedroom.png)
3. Cambie el valor del parámetro `q=` en la dirección URL por `turn off the living room light` y presione ENTRAR. Ahora, el resultado indica que LUIS ha detectado la intención `HomeAutomation.TurnOff` y la entidad `HomeAutomation.Room` con el valor `living room`. 

    ![El resultado JSON detecta la intención TurnOff](./media/luis-get-started-node-get-intent/turn-off-living-room.png)


## <a name="consume-a-luis-result-using-the-endpoint-api-with-python"></a>Uso de un resultado de LUIS mediante la API de punto de conexión con Python

Puede usar Python para acceder a los mismos resultados que vio en la ventana del explorador del paso anterior.

1. Copie uno de los siguientes fragmentos de código en un archivo denominado `quickstart-call-endpoint.py`:

   [!code-python[Console app code that calls a LUIS endpoint for Python 2.7](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/2.x/quickstart-call-endpoint-2-7.py)]

   [!code-python[Console app code that calls a LUIS endpoint for Python 3.6](~/samples-luis/documentation-samples/quickstarts/analyze-text/python/3.x/quickstart-call-endpoint-3-6.py)]

2. Sustituya el valor del campo `Ocp-Apim-Subscription-Key` por su clave de punto de conexión de LUIS.

3. Instale las dependencias con `pip install requests`.

4. Ejecute el script con `python ./quickstart-call-endpoint.py`. Muestra el mismo código JSON que vio anteriormente en la ventana del explorador.
<!-- 
![Console window displays JSON result from LUIS](./media/luis-get-started-python-get-intent/console-turn-on.png)
-->

## <a name="clean-up-resources"></a>Limpieza de recursos
Los dos recursos creados en este tutorial son la clave de punto de conexión de LUIS y el proyecto de C#. Elimine la clave de punto de conexión de LUIS desde Azure Portal. Cierre el proyecto de Visual Studio y quite el directorio del sistema de archivos. 

## <a name="next-steps"></a>Pasos siguientes
> [!div class="nextstepaction"]
> [Adición de grabaciones de voz](luis-get-started-python-add-utterance.md)