---
title: Preguntas más frecuentes de Bing Spell Check API - Azure Cognitive Services | Microsoft Docs
description: Obtenga respuestas a preguntas frecuentes sobre Bing Spell Check API en Azure.
services: cognitive-services
author: HeidiSteen
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-spell-check
ms.topic: conceptual
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: 87b1f3ed3e0aaa9f3c3c804dc9eac3ee60b4a565
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382390"
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
