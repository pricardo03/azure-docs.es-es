---
title: Referencia de Project URL Preview
titlesuffix: Azure Cognitive Services
description: Referencia de punto de conexión de Project URL Preview.
services: cognitive-services
author: mikedodaro
manager: cgronlun
ms.service: cognitive-services
ms.subservice: url-preview
ms.topic: reference
ms.date: 03/29/2018
ms.author: rosh
ms.openlocfilehash: f7925c3eb14915c2b811ccfcd3a3803b9bd7c806
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2019
ms.locfileid: "55222922"
---
# <a name="project-url-preview-v7-reference"></a>Referencia de Project URL Preview v7

URL Preview admite descripciones breves de recursos web para entradas de blog, discusiones de foros, páginas de vista previa, etc. La dirección URL puede ser cualquier tipo de recurso de Internet: página web, noticias, imagen o vídeo. La consulta debe ser una dirección URL absoluta con un esquema http o https; no se admiten las direcciones URL relativas ni otros esquemas, como ftp://.

Las aplicaciones que utilizan URL Preview envían solicitudes web al punto de conexión con una dirección URL para la vista previa en un parámetro de consulta. La solicitud debe incluir el encabezado *Ocp-Apim-Subscription-Key*.

Se puede analizar la respuesta JSON para obtener la información de la vista previa: nombre, descripción del recurso, *isFamilyFriendly* y vínculos que proporcionen acceso a una imagen representativa y al recurso completo en línea.

Debe usar solo los datos de URL Preview para mostrar fragmentos de código de versión preliminar e imágenes en miniatura con hipervínculos a sus sitios de origen, en la URL iniciada por el usuario final compartida en redes sociales, bot de chat u ofertas similares. No debe copiar, almacenar ni almacenar en caché los datos que reciba de Project URL Preview. Debe respetar las solicitudes para deshabilitar las versiones preliminares que pueda recibir del sitio web o los propietarios del contenido.

## <a name="endpoint"></a>Punto de conexión
Para solicitar los resultados de URL Preview, envíe una solicitud al punto de conexión siguiente. Use los encabezados y parámetros de dirección URL para definir especificaciones adicionales.

Solicitud GET del punto de conexión:
```
https://api.labs.cognitive.microsoft.com/urlpreview/v7.0/search?q=queryURL

```

La solicitud debe utilizar el protocolo HTTPS e incluir el parámetro de consulta siguiente:

q: consulta que identifica la dirección URL para la vista previa

En las secciones siguientes se proporcionan detalles técnicos acerca de los objetos de respuesta, los parámetros de consulta y los encabezados que afectan a los resultados de la búsqueda.

Para obtener información acerca de los encabezados que las solicitudes deben incluir, consulte [Encabezados](#headers).

Para obtener información acerca de los parámetros de consulta que las solicitudes deben incluir, consulte [Parámetros de consulta](#query-parameters).

Para obtener información acerca de los objetos JSON que incluye la respuesta, consulte [Objetos de respuesta](#response-objects).

La longitud máxima de la dirección URL de consulta es de 2048 caracteres. Para asegurarse de que la longitud de la dirección URL no supera el límite, la longitud máxima de los parámetros de consulta debe ser inferior a 1500 caracteres. Si la dirección URL supera los 2048 caracteres, el servidor devuelve 404 No encontrado.

Para obtener información sobre el uso permitido y la visualización de los resultados, consulte [Requisitos de uso y visualización](use-display-requirements.md).

> [!NOTE]
> Algunos encabezados de solicitud que son significativos para otras API de búsqueda no afectan a URL Preview.
> - Pragma: el autor de la llamada no controla si URL Preview utiliza la memoria caché.
> - User-Agent: por ahora, URL Preview API no proporciona respuestas distintas para las llamadas procedentes de PC, portátil o móvil.

> Además, algunos parámetros no son actualmente significativos para URL Preview API, pero pueden utilizarse en el futuro para la globalización mejorada.

## <a name="headers"></a>encabezados
A continuación se indican los encabezados que una solicitud y una respuesta pueden incluir.

|Encabezado|DESCRIPCIÓN|
|------------|-----------------|
|<a name="market" />BingAPIs-Market|Encabezado de respuesta.<br /><br /> Mercado que usa la solicitud. El formato es \<CódigoIdioma\>-\<CódigoPaís\>. Por ejemplo, en-US.|
|<a name="traceid" />BingAPIs-TraceId|Encabezado de respuesta.<br /><br /> Identificador de la entrada de registro que contiene los detalles de la solicitud. Si se produce un error, capture este identificador. Si no puede determinar y resolver el problema, incluya este identificador junto con el resto de información que proporcione al equipo de soporte técnico.|
|<a name="subscriptionkey" />Ocp-Apim-Subscription-Key|Encabezado de solicitud obligatorio.<br /><br /> Clave de suscripción que recibió al suscribirse a este servicio en [Cognitive Services](https://www.microsoft.com/cognitive-services/).|
|<a name="clientid" />X-MSEdge-ClientID|Encabezado de solicitud y respuesta opcional.<br /><br /> Bing usa este encabezado para proporcionar a los usuarios un comportamiento coherente en las llamadas API de Bing. Bing incorpora a menudo mejoras y características piloto nuevas, y usa el identificador de cliente como clave para asignar el tráfico en distintos paquetes piloto. Si no usa el mismo identificador de cliente para un usuario en varias solicitudes, Bing puede asignar el usuario a varios paquetes piloto en conflicto. La asignación a varios paquetes piloto en conflicto puede generar una experiencia de usuario incoherente. Por ejemplo, si la segunda solicitud tiene una asignación de paquete piloto distinta de la primera, puede que se produzca una experiencia inesperada. Además, Bing puede usar el identificador de cliente para personalizar los resultados web en el historial de búsqueda del identificador de cliente, lo que proporciona una experiencia enriquecida para el usuario.<br /><br /> Bing también utiliza este encabezado para ayudar a mejorar las clasificaciones de resultados mediante el análisis de la actividad que genera un identificador de cliente. Las mejoras de relevancia ayudan con una mayor calidad de los resultados que devuelven las API de Bing y, a su vez, permiten un mayor porcentaje de clics para el consumidor de la API.<br /><br />Estas son las reglas de uso básico que se aplican a este encabezado.<br /><ul><li>Cada usuario que utiliza la aplicación en el dispositivo debe tener un identificador de cliente único generado por Bing.<br /><br/>Si no incluye este encabezado en la solicitud, Bing genera un identificador y lo devuelve en el encabezado de respuesta X-MSEdge-ClientID. La única ocasión en que NO debe incluir este encabezado en una solicitud es la primera vez que el usuario usa la aplicación en el dispositivo.<br /><br/></li><li>Use el identificador de cliente para cada solicitud de API de Bing que haga la aplicación para este usuario en el dispositivo.<br /><br/></li><li>**ATENCIÓN:** debe asegurarse de que este identificador de cliente no se pueda vincular a la información de cualquier cuenta de usuario autenticable.</li><br/><li>Conserve el identificador de cliente. Para conservar el identificador en una aplicación de explorador, use una cookie HTTP persistente para asegurarse de que se use ese identificador en todas las sesiones. No use una cookie de sesión. Para otras aplicaciones, como aplicaciones móviles, use el almacenamiento persistente del dispositivo para conservar el identificador.<br /><br/>La próxima vez que el usuario use la aplicación en ese dispositivo, obtenga el identificador de cliente que conservó.</li></ul><br /> **NOTA:** Las respuestas de Bing pueden incluir este encabezado o no incluirlo. Si la respuesta incluye este encabezado, capture el identificador de cliente y úselo para todas las solicitudes posteriores de Bing del usuario en dicho dispositivo.<br /><br /> **NOTA:** Si incluye X-MSEdge-ClientID, no debe incluir cookies en la solicitud.|
|<a name="clientip" />X-MSEdge-ClientIP|Encabezado de solicitud opcional.<br /><br /> Dirección IPv4 o IPv6 del dispositivo cliente. La dirección IP se usa para detectar la ubicación del usuario. Bing usa la información de ubicación para determinar el comportamiento de la búsqueda segura.<br /><br /> No ofusque la dirección (por ejemplo, cambiando el último octeto a 0). Al ofuscar los resultados de dirección, la ubicación no se encuentra cerca de la ubicación real, lo que puede provocar que Bing devuelva resultados erróneos.|

## <a name="query-parameters"></a>Parámetros de consulta
La solicitud puede incluir los siguientes parámetros de consulta. Consulte la columna Obligatorio para saber cuáles son los parámetros obligatorios. Debe codificar como dirección URL los parámetros de la consulta. La consulta debe ser una dirección URL absoluta con un esquema http o https; no se admiten las direcciones URL relativas ni otros esquemas, como ftp://.

|NOMBRE|Valor|Type|Obligatorio|
|----------|-----------|----------|--------------|
|<a name="mkt" />mkt|Mercado de donde proceden los resultados. <br /><br />Para obtener una lista de los valores de mercado posibles, consulte [Códigos de mercado](#market-codes).<br /><br /> **NOTA:** URL Preview API solo admite actualmente la geografía de Estados Unidos y el idioma inglés.<br /><br />|string|SÍ|
|<a name="query" />q|Dirección URL para la vista previa.|string|SÍ|
|<a name="responseformat" />responseFormat|Tipo de medio que se usará para la respuesta. A continuación se indican los valores posibles que no distinguen entre mayúsculas y minúsculas.<br /><ul><li>JSON</li><li>JSONLD</li></ul><br /> El valor predeterminado es JSON. Para obtener información acerca de los objetos JSON que contiene la respuesta, consulte [Objetos de respuesta](#response-objects).<br /><br />Si especifica JsonLd, el cuerpo de respuesta incluye objetos JSON-LD que contienen los resultados de la búsqueda. Para obtener información acerca de JSON-LD, consulte [JSON-LD](http://json-ld.org/).|string|Sin |
|<a name="safesearch"/>safeSearch|El contenido para adultos ilegal o contenido pirateado se bloquea con el código de error 400 y no se devuelve la marca *isFamilyFriendly*. <p>Para el contenido para adultos legal, se muestra el comportamiento a continuación. Código de estado devuelve 200 y la marca *isFamilyFriendly* se establece en false.<ul><li>safeSearch=strict: no se devolverán los valores de título, descripción, dirección URL e imagen.</li><li>safeSearch=moderate: se obtienen los valores de título, dirección URL y descripción, pero no la imagen descriptiva.</li><li>safeSearch=off: se obtienen todos los objetos/elementos de respuesta: título, dirección URL, descripción e imagen.</li></ul> |string|No se requiere. </br> Se establece de manera predeterminada en safeSearch=strict.|

## <a name="response-objects"></a>Objetos de respuesta
El esquema de respuesta es un objeto [WebPage] o ErrorResponse, como se muestra en Web Search API. Si se produce un error en la solicitud, el objeto de nivel superior es el objeto [ErrorResponse](#errorresponse).

|Objeto|DESCRIPCIÓN|
|------------|-----------------|
|[WebPage](#webpage)|Objeto JSON de nivel superior que contiene los atributos de la vista previa.|

### <a name="error"></a>Error
Define el error que se produjo.

|Elemento|DESCRIPCIÓN|Type|
|-------------|-----------------|----------|
|<a name="error-code" />code|Código de error que identifica la categoría de error. Para obtener una lista de los códigos posibles, consulte [Códigos de error](#error-codes).|string|
|<a name="error-message" />message|Descripción del error.|string|
|<a name="error-moredetails" />moreDetails|Descripción que proporciona información adicional sobre el error.|string|
|<a name="error-parameter" />parameter|Parámetro de consulta de la solicitud que causó el error.|string|
|<a name="error-subcode" />subCode|Código de error que identifica el error. Por ejemplo, si `code` es InvalidRequest, `subCode` puede ser ParameterInvalid o ParameterInvalidValue. |string|
|<a name="error-value" />value|Valor del parámetro de consulta que no era válido.|string|

### <a name="errorresponse"></a>ErrorResponse
Objeto de nivel superior que la respuesta incluye cuando se produce un error en la solicitud.

|NOMBRE|Valor|Type|
|----------|-----------|----------|
|_type|Sugerencia de tipo.|string|
|<a name="errors" />errors|Lista de errores que describen los motivos que causaron un error en la solicitud.|[Error](#error)[]|

### <a name="webpage"></a>WebPage
Define la información acerca de una página web en la vista previa.

|NOMBRE|Valor|Type|
|----------|-----------|----------|
|Nombre|Título de la página (no necesariamente el título HTML).|string|
|URL|Dirección URL que se rastreó realmente (la solicitud puede tener redireccionamientos con seguimiento)|string|
|description|Breve descripción de la página y el contenido|string|
|isFamilyFriendly|Más preciso para elementos del índice web; las capturas en tiempo real realizan esta detección basada únicamente en la dirección URL y no en el contenido de la página.|boolean|
|primaryImageOfPage/contentUrl|Dirección URL de una imagen representativa que se incluirá en la vista previa.|string|

### <a name="identifiable"></a>Identifiable
|NOMBRE|Valor|Type|
|-------------|-----------------|----------|
|id|Identificador de recursos.|string|

## <a name="error-codes"></a>Códigos de error

A continuación se indican los códigos de estado HTTP posibles que devuelve una solicitud.

|Código de estado|DESCRIPCIÓN|
|-----------------|-----------------|
|200|Correcta.|
|400|Uno de los parámetros de consulta falta o no es válido.|
|400|ServerError, subcódigo ResourceError: no se pudo alcanzar la dirección URL solicitada|
|400|ServerError, subcódigo ResourceError: la dirección URL solicitada no devolvió ningún código correcto (incluso si devolvió HTTP 404).|
|400|InvalidRequest, subcódigo Blocked: es posible que la dirección URL incluyera contenido para adultos y se bloqueó.|
|401|Falta la clave de suscripción o no es válida.|
|403|El usuario está autenticado (por ejemplo, usó una clave de suscripción válida), pero no tiene permiso para el recurso solicitado.<br /><br /> Bing también puede devolver este estado si el autor de la llamada supera la cuota de consultas por mes.|
|410|La solicitud usó HTTP en lugar del protocolo HTTPS. HTTPS es el único protocolo admitido.|
|429|El autor de la llamada superó la cuota de consultas por segundo.|
|500|Evento de servidor inesperado.|

Si se produce un error en la solicitud, la respuesta contiene un objeto [ErrorResponse](#errorresponse), que contiene una lista de objetos [Error](#error) que describen la causa del error. Si el error está relacionado con un parámetro, el campo `parameter` identifica el parámetro del problema. Si el error está relacionado con un valor de parámetro, el campo `value` identifica el valor que no es válido.

```json
{
  "_type": "ErrorResponse",
  "errors": [
    {
      "code": "InvalidRequest",
      "subCode": "ParameterMissing",
      "message": "Required parameter is missing.",
      "parameter": "q"
    }
  ]
}

{
  "_type": "ErrorResponse",
  "errors": [
    {
      "code": "InvalidAuthorization",
      "subCode": "AuthorizationMissing",
      "message": "Authorization is required.",
      "moreDetails": "Subscription key is not recognized."
    }
  ]
}
```

A continuación se indican los valores de código de error y subcódigo de error posibles.

|Código|Subcódigo|DESCRIPCIÓN
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>NotImplemented|El código de estado de HTTP es 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Bloqueado|Bing devuelve InvalidRequest siempre que alguna parte de la solicitud no es válida. Por ejemplo, falta un parámetro necesario o un valor de parámetro no es válido.<br/><br/>Si el error es ParameterMissing o ParameterInvalidValue, el código de estado HTTP es 400.<br/><br/>Si usa el protocolo HTTP en lugar de HTTPS, Bing devuelve HttpNotAllowed y el código de estado HTTP es 410.
|RateLimitExceeded|No hay subcódigos|Bing devuelve RateLimitExceeded cada vez que se supera la cuota de consultas por segundo (QPS) o de consultas por mes (QPM).<br/><br/>Si se supera la cuota QPS, Bing devuelve el código de estado HTTP 429, mientras que si se supera la cuota QPM, devuelve 403.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing devuelve InvalidAuthorization cuando no puede autenticar el autor de la llamada. Por ejemplo, falta el encabezado `Ocp-Apim-Subscription-Key` o la clave de suscripción no es válida.<br/><br/>Se produce redundancia si especifica más de un método de autenticación.<br/><br/>Si el error es InvalidAuthorization, el código de estado HTTP es 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing devuelve InsufficientAuthorization cuando el autor de la llamada no tiene permisos para acceder al recurso. Esto puede ocurrir si la clave de suscripción se ha deshabilitado o ha expirado. <br/><br/>Si el error es InsufficientAuthorization, el código de estado HTTP es 403.

## <a name="next-steps"></a>Pasos siguientes
- [Inicio rápido de C#](csharp.md)
- [Inicio rápido de Java](java-quickstart.md)
- [Inicio rápido de JavaScript](javascript.md)
- [Inicio rápido de Node](node-quickstart.md)
- [Inicio rápido de Python](python-quickstart.md)
