---
title: Preguntas más frecuentes (P+F) acerca de Azure Autosuggest API | Microsoft Docs
description: Obtenga respuestas a preguntas habituales acerca de Azure Cognitive Services Autosuggest API en Azure.
services: cognitive-services
author: HeidiSteen
manager: jhubbard
ms.service: cognitive-services
ms.component: bing-autosuggest
ms.topic: article
ms.date: 07/26/2017
ms.author: heidist
ms.openlocfilehash: 00b91728bcfec52ff30697f080d5c2619bab79a8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382074"
---
# <a name="frequently-asked-questions-faq-about-autosuggest-api-cognitive-services"></a>Preguntas frecuentes (P+F) acerca de Autosuggest API (Cognitive Services)
 
 Encuentre respuestas a preguntas habituales acerca de conceptos, códigos y escenarios relacionados con Autosuggest API para Azure Cognitive Services.

### <a name="how-do-i-get-the-optional-client-headers-when-calling-the-bing-autosuggest-api-from-javascript"></a>¿Cómo obtengo los encabezados de cliente opcionales cuando llamo a Bing Autosuggest API desde JavaScript?

Los encabezados siguientes son opcionales, pero es aconsejable usarlos cuando sea necesario. Estos encabezados ayudan a Bing Autosuggest API a obtener resultados más precisos.

- X-Search-Location
- X-MSEdge-ClientID
- X-MSEdge-ClientIP

Sin embargo, cuando se llama a Bing Autosuggest API desde JavaScript, las características de seguridad integradas de su explorador podrían impedirle acceder a los valores de estos encabezados.

Para resolverlo, puede realizar una solicitud de Bing Autosuggest API a través de un proxy CORS. La respuesta de un proxy de este tipo tiene un encabezado `Access-Control-Expose-Headers` que agrega los encabezados de respuesta a listas blancas y hace que estén disponibles para JavaScript.

Es fácil instalar un proxy CORS para permitir que nuestra [aplicación de tutorial](tutorials/autosuggest.md) obtenga acceso a los encabezados de cliente opcionales. En primer lugar, si aún no lo tiene, [instale Node.js](https://nodejs.org/en/download/). A continuación, escriba el siguiente comando en un símbolo del sistema.

    npm install -g cors-proxy-server

Luego, cambie el punto de conexión de Bing Autosuggest API del archivo HTML a:

    http://localhost:9090/https://api.cognitive.microsoft.com/bing/v7.0/Suggestions

Por último, inicie el proxy CORS con el siguiente comando:

    cors-proxy-server

Deje abierta la ventana de comandos mientras usa la aplicación del tutorial, ya que si la cierra, se detendrá el proxy. En la sección de encabezados HTTP expandibles situada bajo los resultados de la búsqueda, puede ver el encabezado `X-MSEdge-ClientID` (entre otras cosas) y comprobar que es el mismo en todas las solicitudes.

## <a name="next-steps"></a>Pasos siguientes

¿Es su pregunta acerca de una característica o funcionalidad que falta? Considere la posibilidad de solicitarla o votarla en nuestro [sitio web de User Voice](https://cognitive.uservoice.com/).

## <a name="see-also"></a>Otras referencias

- [Stack Overflow: Cognitive Services](http://stackoverflow.com/questions/tagged/microsoft-cognitive)
