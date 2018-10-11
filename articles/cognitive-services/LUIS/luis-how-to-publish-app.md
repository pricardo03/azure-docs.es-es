---
title: Publicación de una aplicación de LUIS en el punto de conexión de predicción
titleSuffix: Azure Cognitive Services
description: Cuando termine de compilar y probar la aplicación de LUIS activa, haga que esté disponible para la aplicación cliente mediante su publicación en el punto de conexión.
services: cognitive-services
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 6eb48fd0f3290fbc3a249bc3880c809ace9f9ddb
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/09/2018
ms.locfileid: "48886518"
---
# <a name="publish-your-trained-app"></a>Publicar la aplicación entrenada

Cuando termine de compilar y probar la aplicación de LUIS activa, haga que esté disponible para la aplicación cliente mediante su publicación en el punto de conexión. 

<a name="publish-your-trained-app-to-an-http-endpoint"></a>

## <a name="publishing"></a>Publicación

Para publicar en el punto de conexión, seleccione **Publicar** en el panel superior derecho. 

![Barra de navegación superior derecha](./media/luis-how-to-publish-app/publish-top-nav-bar.png)

Seleccione el espacio correcto cuando se muestre la ventana emergente: ensayo o producción. Si usa dos espacios de publicación, puede tener dos versiones distintas con puntos de conexión publicados o la misma versión en dos puntos de conexión diferentes. 

La aplicación se publica en todas las regiones asociadas a los recursos de LUIS agregados en el portal de LUIS. Por ejemplo, para una aplicación creada en [www.luis.ai](https://www.luis.ai), si crea un recurso de LUIS en **westus** y lo agrega a la aplicación como un recurso, la aplicación se publicará en dicha región. Para obtener más información sobre las regiones de LUIS, consulte [Regiones](luis-reference-regions.md).
 
![Ventana emergente de publicación](./media/luis-how-to-publish-app/publish-pop-up.png)

Cuando la aplicación se publica correctamente, aparece un cuadro de notificación verde en la parte superior del navegador. La barra de notificación verde también incluye un vínculo a los puntos de conexión. 

![Ventana emergente de publicación](./media/luis-how-to-publish-app/publish-success.png)

Si necesita la dirección URL del punto de conexión, seleccione el vínculo. También puede obtener las direcciones URL de punto de conexión seleccionando **Administrar** en el menú superior y, a continuación, **Claves y puntos de conexión** en el menú de la izquierda. 

## <a name="configuring-publish-settings"></a>Configuración de los ajustes de publicación

Para configurar los ajustes de publicación, seleccione **Administrar** en el panel de navegación superior derecho y, a continuación, seleccione **Configuración de publicación**. 

![Configuración Publicar](./media/luis-how-to-publish-app/publish-settings.png)

### <a name="publish-after-enabling-sentiment-analysis"></a>Publicación después de habilitar el análisis de opiniones

<a name="enable-sentiment-analysis"></a>

El análisis de opiniones permite que LUIS se integre con [Text Analytics](https://azure.microsoft.com/services/cognitive-services/text-analytics/) para proporcionar un análisis de frases clave y opiniones. 

No tiene que proporcionar una clave de Text Analytics y no se carga ningún costo de facturación a su cuenta de Azure por este servicio. Una vez seleccionada esta configuración, es persistente. 

Los datos de opinión son una puntuación entre 1 y 0 que indica el valor de opinión positiva (más cercano a 1) o negativa (más cercano a 0) de los datos.

Para obtener más información acerca de la respuesta del punto de conexión JSON con análisis de sentimiento, consulte [Análisis de sentimiento](luis-concept-data-extraction.md#sentiment-analysis).



## <a name="next-steps"></a>Pasos siguientes

* Consulte [Administrar claves](./luis-how-to-manage-keys.md) para agregar claves a la clave de suscripción de Azure a LUIS y para obtener información sobre cómo establecer la clave de Bing Spell Check e incluir todas las intenciones en los resultados.
* Consulte [Entrenar y probar la aplicación](luis-interactive-test.md) para obtener instrucciones sobre cómo probar la aplicación publicada en la consola de prueba.

