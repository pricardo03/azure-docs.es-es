---
title: Preguntas más frecuentes sobre Bing Spell Check API
titlesuffix: Azure Cognitive Services
description: Obtenga respuestas a preguntas frecuentes sobre Bing Spell Check API en Azure.
services: cognitive-services
author: HeidiSteen
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-spell-check
ms.topic: conceptual
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: 552e6fb3375c04e2f157842b13f72151e122f17b
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55866345"
---
# <a name="frequently-asked-questions-about-the-bing-spell-check-api"></a>Preguntas más frecuentes sobre Bing Spell Check API

 Encuentre respuestas a preguntas habituales sobre conceptos, código y escenarios relacionados con Bing Spell Check API de Microsoft Cognitive Services en Azure.

## <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-spell-check-api-from-javascript"></a>¿Cómo obtengo los encabezados de cliente opcionales cuando llamo a Bing Spell Check API desde JavaScript?

Los encabezados siguientes son opcionales, pero le recomendamos que los use según sea necesario. Estos encabezados ayudan a Bing Spell Check API a obtener resultados más precisos.

- X-Search-Location
- X-MSEdge-ClientID
- X-MSEdge-ClientIP

Sin embargo, cuando se llama a Bing Spell Check API desde JavaScript, las características de seguridad integradas de su explorador podrían impedirle obtener acceso a los valores de estos encabezados.

Para resolver este problema, puede realizar una solicitud de Bing Spell Check API a través de un proxy CORS. La respuesta de un proxy de este tipo tiene un encabezado `Access-Control-Expose-Headers` que agrega los encabezados de respuesta a listas blancas, y hace que estén disponibles para JavaScript.

Es fácil instalar un proxy CORS para permitir que la [aplicación de tutorial](tutorials/spellcheck.md) obtenga acceso a los encabezados de cliente opcionales. En primer lugar, si aún no lo tiene, [instale Node.js](https://nodejs.org/en/download/). A continuación, escriba el siguiente comando en un símbolo del sistema.

    npm install -g cors-proxy-server

Luego, cambie el punto de conexión de Bing Spell Check API del archivo HTML a:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/spellcheck/

Por último, inicie el proxy CORS con el siguiente comando:

    cors-proxy-server

Deje abierta la ventana de comandos mientras usa la aplicación del tutorial. Al cerrar la ventana, se detiene el proxy. En la sección expandible de encabezados HTTP que está situada bajo los resultados de búsqueda, ahora puede ver el encabezado `X-MSEdge-ClientID` (entre otras cosas) y comprobar que es el mismo para cada solicitud.

## <a name="next-steps"></a>Pasos siguientes

¿Es su pregunta acerca de una característica o funcionalidad que falta? Considere la posibilidad de solicitarla o votarla en el [sitio web de UserVoice](https://cognitive.uservoice.com/).

## <a name="see-also"></a>Otras referencias

 [StackOverflow: Cognitive Services](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
