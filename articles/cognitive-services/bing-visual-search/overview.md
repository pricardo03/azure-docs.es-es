---
title: Información general sobre Bing Visual Search API | Microsoft Docs
titleSuffix: Bing Web Search APIs - Cognitive Services
description: Muestra cómo obtener detalles o conclusiones sobre una imagen, como imágenes similares u orígenes de compras.
services: cognitive-services
author: swhite-msft
manager: rosh
ms.service: cognitive-services
ms.technology: bing-visual-search
ms.topic: article
ms.date: 04/10/2018
ms.author: scottwhi
ms.openlocfilehash: 95f10d8ea7ebe1d40d45231a8ea40df81543fe8b
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/23/2018
ms.locfileid: "35382263"
---
# <a name="what-is-bing-visual-search-api"></a>¿Qué es Bing Visual Search API?

Bing Visual Search API proporciona una experiencia similar a los detalles de imagen que se muestran en Bing.com/images. Visual Search permite cargar una imagen y obtener conclusiones sobre la imagen, como otras imágenes visualmente similares, orígenes de compras, páginas web que incluyen la imagen, etc. En lugar de cargar una imagen, también puede proporcionar un token de conclusión que obtenga de una imagen de los resultados de búsqueda de imágenes (consulte [Bing Images API](../bing-image-search/overview.md)).

Visual Search puede identificar celebridades, monumentos y puntos de referencia, obras de arte, mobiliario, moda, productos, reconocimiento de caracteres (OCR) y mucho más.

Estas son las conclusiones que Visual Search le permite detectar.

- Imágenes visualmente similares: una lista de imágenes visualmente similares a la imagen de entrada
- Productos visualmente similares: una lista de imágenes que contienen productos visualmente similares al producto que se muestra en la imagen de entrada
- Orígenes de compra: una lista de lugares donde puede comprar el producto que se muestra en la imagen de entrada
- Búsquedas relacionadas: una lista de búsquedas relacionadas realizados por otros usuarios o basadas en el contenido de la imagen
- Páginas web que incluyen la imagen: una lista de páginas web que incluyen la imagen de entrada
- Recetas: una lista de páginas web que incluyen recetas para hacer el plato que se muestra en la imagen de entrada

Además de estas conclusiones, Visual Search también devuelve un conjunto de términos variados (etiquetas) derivados de la imagen de entrada. Estas etiquetas permiten a los usuarios explorar los conceptos que se encuentran en la imagen. Por ejemplo, si la imagen de entrada es un atleta famoso, una de las etiquetas podría ser el nombre del atleta y otra, deportes. O bien, si la imagen de entrada es un pastel de manzana, las etiquetas podrían ser pastel de manzana, pasteles o postres, para que los usuarios puedan explorar conceptos relacionados.

Los resultados de Visual Search también incluyen rectángulos de selección para las regiones de interés de la imagen. Por ejemplo, si la imagen contiene varias celebridades, los resultados pueden incluir rectángulos de selección para cada una de las celebridades reconocidas en la imagen. O bien, si Bing reconoce un producto o prenda en la imagen, el resultado puede incluir un rectángulo de selección para el producto o prenda que se ha reconocido.

> [!IMPORTANT]
> Si utiliza el punto de conexión /images/details para [obtener conclusiones de imágenes](../bing-image-search/image-insights.md), debe actualizar el código para usar Visual Search, ya que proporciona conclusiones más completas.


## <a name="the-request"></a>La solicitud

A continuación se indican las opciones para obtener conclusiones acerca de una imagen. 

- Envíe un token de conclusión obtenido de una imagen en una llamada anterior a uno de los puntos de conexión de [Bing Images API](https://docs.microsoft.com/rest/api/cognitiveservices/bing-images-api-v7-reference)
- Envíe la dirección URL de una imagen
- Cargue una imagen (binaria)


Si envía a Visual Search una dirección URL o un token de imagen, a continuación se muestra el objeto JSON que se debe incluir en el cuerpo de POST. 

```json
{
    "imageInfo" : {
        "url" : "",
        "imageInsightsToken" : "",
        "cropArea" : {
            "top" : 0.1,
            "left" : 0.5,
            "right" : 0.9,
            "bottom" : 0.9
        }
    },
    "knowledgeRequest" : {
      "filters" : {
        "site" : ""
      }
    }
}
```

El objeto `imageInfo` debe incluir el campo `url` o `imageInsightsToken`, pero no ambos. Defina el campo `url` con la dirección URL de una imagen accesible de Internet. El tamaño máximo de imagen admitido es 1 MB.

`imageInsightsToken` debe establecerse en un token de conclusión. Para obtener un token de conclusión, llame a Bing Image API. La respuesta contiene una lista de objetos `Image`. Cada objeto `Image` contiene un campo `imageInsightsToken`, que contiene el token.

El campo `cropArea` es opcional. El área de recorte especifica las esquinas superior izquierda e inferior derecha de una región de interés. Especifique los valores en el intervalo de 0.0 a 1.0. Los valores son un porcentaje del ancho o alto total. Por ejemplo, en el ejemplo anterior, se marca la mitad derecha de la imagen como región de interés. Inclúyala si quiere limitar la solicitud de conclusión a la región de interés.

El objeto `filters` contiene un filtro de sitio (vea el campo `site`) que puede utilizar para restringir los resultados de imágenes y productos similares a un dominio específico. Por ejemplo, si la imagen es de un Surface Book, puede establecer `site` en www.microsoft.com. 

Si quiere obtener conclusiones acerca de una copia local de una imagen, cargue la imagen como datos binarios.

Para obtener más información sobre cómo incluir estas opciones en el cuerpo de POST, vea [Tipos de formulario de contenido](#content-form-types).


### <a name="endpoint"></a>Punto de conexión

El punto de conexión de Visual Search es: https:\/\/api.cognitive.microsoft.com/bing/v7.0/images/visualsearch.

Las solicitudes solo se deben enviar como solicitudes HTTP POST. 


### <a name="query-parameters"></a>Parámetros de consulta

Estos son los parámetros de consulta que debe especificar la solicitud. Como mínimo, debe incluir el parámetro de consulta `mkt`.

|NOMBRE|Valor|Escriba|Obligatorio|  
|----------|-----------|----------|--------------|  
|<a name="cc" />cc|Un código de país de 2 caracteres del país del que provienen los resultados.<br /><br /> Si establece este parámetro, debe especificar también el encabezado [Accept-Language](#acceptlanguage). Bing usa el primer idioma compatible que encuentra en la lista de idiomas y combina el idioma con el código de país que especifique para determinar el mercado del que debe devolver resultados. Si la lista de idiomas no incluye ningún idioma compatible, Bing busca el idioma y el mercado más cercanos que admiten la solicitud. También puede usar un mercado agregado o predeterminado para los resultados, en lugar del especificado.<br /><br /> Debe utilizar este parámetro de consulta y el parámetro de consulta `Accept-Language` solo si especifica varios idiomas. En caso contrario, debe utilizar los parámetros de consulta `mkt` y `setLang`.<br /><br /> Este parámetro y el parámetro de consulta [mkt](#mkt) son mutuamente excluyentes: no especifique los dos.|string|Sin |  
|<a name="mkt" />mkt|El mercado de donde proceden los resultados. <br /><br /> **NOTA:** Se recomienda especificar siempre el mercado, si se conoce. Especificar el mercado ayuda a Bing a enrutar la solicitud y devolver una respuesta adecuada y óptima.<br /><br /> Este parámetro y el parámetro de consulta [cc](#cc) son mutuamente excluyentes: no especifique los dos.|string|Sí|  
|<a name="safesearch" />safeSearch|Un filtro usado para filtrar el contenido para adultos. Estos son los posibles valores posibles de filtro (no distingue entre mayúsculas y minúsculas).<br /><ul><li>Desactivado: devuelve páginas web con texto o imágenes para adultos.<br /><br/></li><li>Moderado: devuelve páginas web con texto para adultos, pero no imágenes para adultos.<br /><br/></li><li>Estricto: no devuelve páginas web con texto o imágenes para adultos.</li></ul><br /> El valor predeterminado es Moderado.<br /><br /> **Nota:** Si la solicitud proviene de un mercado para el que la directiva para adultos de Bing requiere que `safeSearch` se defina como Estricto, Bing ignora el valor `safeSearch` y usa Estricto.<br/><br/>**NOTA:** Si usa el operador de consulta `site:`, es probable que la respuesta contenga contenido para adultos, independientemente del valor asignado al parámetro de consulta `safeSearch`. Use `site:` solo si es consciente del contenido del sitio y el escenario admite la posibilidad de contenido para adultos. |string|Sin |  
|<a name="setlang" />setLang|El idioma que se usará para las cadenas de la interfaz de usuario. Especifique el idioma con el código ISO 639-1 de 2 letras. Por ejemplo, el código de idioma para inglés es EN. El valor predeterminado es EN (inglés).<br /><br /> Aunque es opcional, siempre debe especificar el idioma. Normalmente, se establece `setLang` en el mismo idioma especificado por `mkt`, a menos que el usuario quiera que las cadenas de la interfaz de usuario se muestren en otro idioma.<br /><br /> Este parámetro y el encabezado [Accept-Language](#acceptlanguage) son mutuamente excluyentes: no especifique los dos.<br /><br /> Una cadena de interfaz de usuario es una cadena que se usa como etiqueta en una interfaz de usuario. Hay algunas cadenas de interfaz de usuario en los objetos de respuesta JSON. Además, los vínculos a las propiedades de Bing.com de los objetos de respuesta aplican el idioma especificado.|string|Sin | 

### <a name="headers"></a>encabezados

Estos son los encabezados que debe especificar la solicitud. Los encabezados Content-Type y Ocp-Apim-Subscription-Key son los únicos encabezados necesarios, pero también debe incluir User-Agent, X-MSEdge-ClientID, X-MSEdge-ClientIP y X-Search-Location.


|Encabezado|DESCRIPCIÓN|  
|------------|-----------------|  
|<a name="acceptlanguage" />Accept-Language|Encabezado de solicitud opcional.<br /><br /> Una lista delimitada por comas de los idiomas que usará para cadenas de la interfaz de usuario. La lista se muestra en orden de preferencia decreciente. Para obtener más información, incluido el formato esperado, consulte [RFC2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> Este encabezado y el parámetro de consulta [setLang](#setlang) son mutuamente excluyentes: no especifique los dos.<br /><br /> Si establece este encabezado, debe especificar también el parámetro de consulta [cc](#cc). Para determinar el mercado para el que hay que devolver resultados, Bing usa el primer idioma compatible que encuentra en la lista y lo combina con el valor del parámetro `cc`. Si la lista no incluye ningún lenguaje compatible, Bing busca el idioma y el mercado más cercanos que admiten la solicitud, o usa un mercado agregado o predeterminado para los resultados. Para determinar el mercado que usó Bing, consulte el encabezado BingAPIs-Market.<br /><br /> Use este encabezado y el parámetro de consulta `cc` solo si especifica varios idiomas. En caso contrario, use los parámetros de consulta [mkt](#mkt) y [setLang](#setlang).<br /><br /> Una cadena de interfaz de usuario es una cadena que se usa como etiqueta en una interfaz de usuario. Hay algunas cadenas de interfaz de usuario en los objetos de respuesta JSON. Cualquier vínculo a las propiedades de Bing.com de los objetos de respuesta aplica el idioma especificado.|  
|<a name="contenttype" />Content-Type|Encabezado de solicitud obligatorio.<br /><br />Debe establecerse en multipart/form-data e incluir un parámetro de límite (por ejemplo, multipart/form-data; boundary=\<cadena de límite\>). Para obtener más información, consulte [Tipos de formulario de contenido](#content-form-types).
|<a name="market" />BingAPIs-Market|Encabezado de respuesta.<br /><br /> El mercado que usa la solicitud. El formato es \<códigoIdioma\>-\<códigoPaís\>. Por ejemplo, en-US.|  
|<a name="traceid" />BingAPIs-TraceId|Encabezado de respuesta.<br /><br /> El identificador de la entrada de registro que contiene los detalles de la solicitud. Si se produce un error, capture este identificador. Si no puede determinar y resolver el problema, incluya este identificador junto con el resto de información que proporcione al equipo de soporte técnico.|  
|<a name="subscriptionkey" />Ocp-Apim-Subscription-Key|Encabezado de solicitud obligatorio.<br /><br /> La clave de suscripción que recibió al suscribirse a este servicio en [Cognitive Services](https://www.microsoft.com/cognitive-services/).|  
|<a name="pragma" />Pragma|Encabezado de solicitud opcional<br /><br /> De forma predeterminada, Bing devuelve contenido almacenado en caché, si está disponible. Para que Bing no devuelva contenido almacenado en caché, establezca el encabezado Pragma en no-cache (por ejemplo, Pragma: no-cache).
|<a name="useragent" />User-Agent|Encabezado de solicitud opcional.<br /><br /> El agente de usuario que origina la solicitud. Bing usa el agente de usuario para proporcionar a los usuarios móviles una experiencia optimizada. Aunque es opcional, se recomienda especificar siempre este encabezado.<br /><br /> El agente de usuario debe ser la misma cadena que envían los exploradores que más se usan. Para obtener información acerca de los agentes de usuario, consulte [RFC 2616](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).<br /><br /> A continuación se incluyen ejemplos de cadenas user-agent.<br /><ul><li>Windows Phone: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)<br /><br /></li><li>Android: Mozilla/5.0 (Linux; U; Android 2.3.5; en-us; SCH-I500 Build/GINGERBREAD) AppleWebKit/533.1 (KHTML; like Gecko) Version/4.0 Mobile Safari/533.1<br /><br /></li><li>iPhone: Mozilla/5.0 (iPhone; CPU iPhone OS 6_1 like Mac OS X) AppleWebKit/536.26 (KHTML; like Gecko) Mobile/10B142 iPhone4;1 BingWeb/3.03.1428.20120423<br /><br /></li><li>PC: Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; Touch; rv:11.0) like Gecko<br /><br /></li><li>iPad: Mozilla/5.0 (iPad; CPU OS 7_0 like Mac OS X) AppleWebKit/537.51.1 (KHTML, like Gecko) Version/7.0 Mobile/11A465 Safari/9537.53</li></ul>|
|<a name="clientid" />X-MSEdge-ClientID|Encabezado de solicitud y respuesta opcional.<br /><br /> Bing usa este encabezado para proporcionar a los usuarios un comportamiento coherente en las llamadas API de Bing. Bing incorpora a menudo mejoras y características piloto nuevas, y usa el identificador de cliente como clave para asignar el tráfico en distintos paquetes piloto. Si no usa el mismo identificador de cliente para un usuario en varias solicitudes, Bing puede asignar el usuario a varios paquetes piloto en conflicto. La asignación a varios paquetes piloto en conflicto puede generar una experiencia de usuario incoherente. Por ejemplo, si la segunda solicitud tiene una asignación de paquete piloto distinta de la primera, puede que se produzca una experiencia inesperada. Además, Bing puede usar el identificador de cliente para personalizar los resultados web en el historial de búsqueda del identificador de cliente, lo que proporciona una experiencia enriquecida para el usuario.<br /><br /> Bing también usa este encabezado para ayudar a mejorar las clasificaciones de resultados mediante el análisis de la actividad que genera un identificador de cliente. Las mejoras de relevancia ayudan con una mayor calidad de los resultados que devuelven las API de Bing y, a su vez, permiten un mayor porcentaje de clics para el consumidor de la API.<br /><br /> **IMPORTANTE:** Aunque es opcional, considere este encabezado necesario. Conservar el identificador de cliente en varias solicitudes para la misma combinación de usuario final y dispositivo permite que 1) el consumidor de API reciba una experiencia de usuario coherente y 2) mayor porcentaje de clics a través de una mejor calidad de los resultados de las API de Bing.<br /><br /> Estas son las reglas de uso básicas que se aplican a este encabezado.<br /><ul><li>Cada usuario que usa la aplicación en el dispositivo debe tener un identificador de cliente único generado por Bing.<br /><br/>Si no incluye este encabezado en la solicitud, Bing genera un identificador y lo devuelve en el encabezado de respuesta X-MSEdge-ClientID. La única ocasión en que NO debe incluir este encabezado en una solicitud es la primera vez que el usuario usa la aplicación en el dispositivo.<br /><br/></li><li>**ATENCIÓN:** Debe asegurarse de que este identificador de cliente no se pueda vincular a la información de cualquier cuenta de usuario autenticada.</li><li>Use el identificador de cliente para cada solicitud de API de Bing que haga la aplicación para este usuario en el dispositivo.<br /><br/></li><li>Conserve el identificador de cliente. Para conservar el identificador en una aplicación de explorador, use una cookie HTTP persistente para asegurarse de que se use ese identificador en todas las sesiones. No use una cookie de sesión. Para otras aplicaciones, como aplicaciones móviles, use el almacenamiento persistente del dispositivo para conservar el identificador.<br /><br/>La próxima vez que el usuario use la aplicación en ese dispositivo, obtenga el identificador de cliente que conservó.</li></ul><br /> **NOTA:** Las respuestas de Bing pueden incluir este encabezado o no incluirlo. Si la respuesta incluye este encabezado, capture el identificador de cliente y úselo para todas las solicitudes posteriores de Bing para el usuario en dicho dispositivo.<br /><br /> **NOTA:** Si incluye X-MSEdge-ClientID, no debe incluir cookies en la solicitud.|  
|<a name="clientip" />X-MSEdge-ClientIP|Encabezado de solicitud opcional.<br /><br /> La dirección IPv4 o IPv6 del dispositivo cliente. La dirección IP se usa para detectar la ubicación del usuario. Bing usa la información de ubicación para determinar el comportamiento de la Búsqueda segura.<br /><br /> **NOTA:** Aunque es opcional, se recomienda especificar siempre este encabezado y el encabezado X-Search-Location.<br /><br /> No ofusque la dirección (por ejemplo, cambiando el último octeto a 0). Al ofuscar la dirección, la ubicación no se encuentra cerca de la ubicación real, lo que puede provocar que Bing devuelva resultados con errores.|  
|<a name="location" />X-Search-Location|Encabezado de solicitud opcional.<br /><br /> Una lista delimitada por punto y coma de pares clave-valor que describen la ubicación geográfica del cliente. Bing usa la información de ubicación para determinar el comportamiento de la Búsqueda segura y devolver contenido local pertinente. Especifique el par clave-valor como \<clave\>:\<valor\>. A continuación se incluyen las claves que se usan para especificar la ubicación del usuario.<br /><br /><ul><li>lat: obligatoria. La latitud de la ubicación del cliente, en grados. La latitud debe ser mayor o igual que -90.0, y menor o igual que +90.0. Los valores negativos indican latitudes del sur, mientras que los valores positivos indican latitudes del norte.<br /><br /></li><li>long: obligatoria. La longitud de la ubicación del cliente, en grados. La longitud debe ser mayor o igual que -180.0, y menor o igual que +180.0. Los valores negativos indican longitudes occidentales, mientras que los valores positivos indican longitudes orientales.<br /><br /></li><li>re: obligatorio. El radio, en metros, que especifica la precisión horizontal de las coordenadas. Pase el valor devuelto por el servicio de ubicación del dispositivo. Los valores típicos pueden ser 22m para GPS/Wi-Fi, 380m para la triangulación de antena repetidora de telefonía móvil y 18,000m para la búsqueda inversa de IP.<br /><br /></li><li>ts: opcional. La marca de tiempo UTC UNIX del momento en que el cliente estaba en la ubicación. (La marca de tiempo UNIX es el número de segundos transcurridos desde el 1 de enero de 1970).<br /><br /></li><li>head: opcional. El encabezado relativo del cliente o la dirección de desplazamiento. Especifique la dirección de desplazamiento como grados de 0 a 360, en la dirección de las agujas del reloj con respecto al norte verdadero. Especifique esta clave solo si la clave `sp` es distinta de cero.<br /><br /></li><li>sp: opcional. La velocidad horizontal, en metros por segundo, a la que se desplaza el dispositivo de cliente.<br /><br /></li><li>alt: opcional. La altitud del dispositivo de cliente, en metros.<br /><br /></li><li>are: opcional. El radio, en metros, que especifica la precisión vertical de las coordenadas. Especifique esta clave solo si especificó la clave `alt`.<br /><br /></li></ul> **NOTA:** Aunque muchas de las claves son opcionales, cuanta más información proporcione, más precisos serán los resultados de ubicación.<br /><br /> **NOTA:** Aunque es opcional, se recomienda especificar siempre la ubicación geográfica del usuario. Proporcionar la ubicación es especialmente importante si la dirección IP del cliente no refleja con precisión la ubicación física del usuario (por ejemplo, si el cliente usa VPN). Para obtener unos resultados óptimos, debe incluir este encabezado y el encabezado X-MSEdge-ClientIP, pero, como mínimo, debe incluir este encabezado.|

> [!NOTE] 
> Recuerde que los Términos de uso exigen el cumplimiento de todas las leyes aplicables, incluso en relación con el uso de estos encabezados. Por ejemplo, en ciertas jurisdicciones, como Europa, hay requisitos para obtener el consentimiento del usuario antes de colocar determinados dispositivos de seguimiento en los dispositivos de usuario.


<a name="content-form-types" />

### <a name="content-form-types"></a>Tipos de formulario de contenido

Cada solicitud debe incluir el encabezado Content-Type. El encabezado debe establecerse en: multipart/form-data; boundary=\<cadena delimitadora\>, donde \<cadena delimitadora\> es una cadena opaca única que identifica el límite de los datos del formulario. Por ejemplo, boundary=boundary_1234-abcd.


Si envía a Visual Search una dirección URL o token de imagen, se muestran los datos de formulario que debe incluir en el cuerpo de POST. Los datos del formulario deben incluir el encabezado Content-Disposition y su parámetro `name` debe establecerse en "knowledgeRequest". Para obtener más información sobre el objeto `imageInfo`, consulte [La solicitud](#the-request).


```
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "url" : "https://contoso.com/2018/05/fashion/red.jpg"
    }
}

--boundary_1234-abcd--
```

Si carga una imagen local, a continuación verá los datos del formulario que debe incluir en el cuerpo del elemento POST. Los datos del formulario deben incluir el encabezado Content-Disposition. Asimismo, el parámetro `name` se debe establecer en "imagen" y el parámetro `filename` se puede establecer en cualquier cadena. El encabezado Content-Type se puede establecer en cualquier tipo MIME de imagen común. El contenido del formulario es el binario de la imagen. Recuerde que el tamaño de imagen máximo que puede cargar es de 1 MB. 


```
--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="myimagefile.jpg"
Content-Type: image/jpeg

ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```

A continuación se muestra cómo especificar la región de interés de una imagen cargada.

```
--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "cropArea" : {
            "top" : 0.2,
            "left" : 0.3,
            "bottom" : 0.7,
            "right" : 0.6
        }
    }
}

--boundary_1234-abcd
Content-Disposition: form-data; name="image"; filename="image"
Content-Type: image/jpeg


ÿØÿà JFIF ÖÆ68g-¤CWŸþ29ÌÄøÖ‘º«™æ±èuZiÀ)"óÓß°Î= ØJ9á+*G¦...

--boundary_1234-abcd--
```



### <a name="example-request"></a>Solicitud de ejemplo

A continuación se muestra una solicitud de conclusión de imagen completa que pasa un token de imagen y una región de interés. El token de conclusión se obtiene de una llamada anterior a /images/search.


```  
POST https://api.cognitive.microsoft.com/bing/v7.0/images/visualsearch?mkt=en-us HTTP/1.1  
Content-Type: multipart/form-data; boundary=boundary_1234-abcd
Ocp-Apim-Subscription-Key: 123456789ABCDE  
X-MSEdge-ClientIP: 999.999.999.999  
X-Search-Location: lat:47.60357;long:-122.3295;re:100  
X-MSEdge-ClientID: <blobFromPriorResponseGoesHere>  
Host: api.cognitive.microsoft.com 

--boundary_1234-abcd
Content-Disposition: form-data; name="knowledgeRequest"

{
    "imageInfo" : {
        "imageInsightsToken" : "mid_D6426898706EC7..."
        "cropArea" : {
            "top" : 0.1,
            "left" : 0.2,
            "bottom" : 0.7,
            "right" : 0.5
        }
    }
}

--boundary_1234-abcd--
```


## <a name="the-response"></a>La respuesta

Si hay conclusiones disponibles para la imagen, la respuesta contiene uno o varios valores `tags` que contienen las conclusiones. El campo `image` contiene el token de conclusión de la imagen de entrada.

```json
{
  "_type" : "ImageKnowledge",
  "tags" : [
    {...},
    {...},
    {...},
    {...},
    {...}
  ],
  "image" : {
    "imageInsightsToken" : "bcid_AF8C9CA409421B..."
  }
}
```

El campo `tags` contiene un nombre para mostrar y una lista de acciones (conclusiones). Una de las etiquetas contiene un campo `displayName` que se establece en una cadena vacía. Esta etiqueta contiene las conclusiones predeterminadas, como las páginas web que incluyen la imagen, imágenes visualmente similares y orígenes de compra para los productos de la imagen. Dado que toda la imagen es de interés, la etiqueta de conclusión predeterminada no incluye rectángulos de selección para las áreas de interés.


```json
{
  "_type" : "ImageKnowledge",
  "tags" : [
    {
      "displayName" : "",
      "actions" : [
        {...},
        {...},
        {...},
        {...}
      ]
    },
    {...},
    {...},
    {...},
    {...}
  ],
  "image" : {
    "imageInsightsToken" : "bcid_AF8C9CA409421B..."
  }
}
```

Para obtener una lista de conclusiones predeterminadas, consulte [Conclusiones predeterminadas](./default-insights-tag.md).



Las etiquetas restantes contienen otras conclusiones que pueden ser de interés para el usuario. Por ejemplo, si la imagen contiene texto, una de las etiquetas puede incluir una conclusión TextResults, que contiene el texto reconocido. O bien, si Bing reconoce una entidad (persona, lugar o cosa) en la imagen, una de las etiquetas puede identificar la entidad. Visual Search también devuelve un conjunto de términos variados (etiquetas) derivados de la imagen de entrada. Estas etiquetas permiten a los usuarios explorar los conceptos que se encuentran en la imagen. Por ejemplo, si la imagen de entrada es de un atleta famoso, una de las etiquetas podría ser deportes, que contiene vínculos a imágenes de deportes.

Cada etiqueta incluye un nombre para mostrar que puede usar para clasificar las conclusiones, el rectángulo de selección que identifica la región de interés a que se aplica la conclusión, las propias conclusiones y una miniatura de la imagen. Por ejemplo, si la imagen es de una persona con un jersey de deportes, una de las etiquetas podría incluir un rectángulo de selección que delimite el jersey e incluya conclusiones de VisualSearch y ProductVisualSearch. Por otra parte, otra etiqueta podría incluir una conclusión de ImageResults que contenga una URL para una solicitud de API /images/search para obtener imágenes cuyos temas estén relacionados o una dirección URL de búsqueda de Bing.com que lleve al usuario a los resultados de búsqueda de imágenes de Bing.com.

El resto de etiquetas, distintas a la etiqueta de conclusión predeterminada, incluyen rectángulos de selección que identifican las regiones de interés de la imagen. Por ejemplo, si la imagen incluye varias personas conocidas, las etiquetas podrían incluir rectángulos de selección para cada una de ellas. O bien, si la imagen contiene prendas de vestir conocidas, las etiquetas podrían incluir rectángulos de selección para cada prenda reconocida. Puede usar los rectángulos de selección para crear zonas activas en la imagen que, cuando se haga clic sobre ellas, proporcionen detalles sobre el contenido de dicha región de la imagen. No debe incluir zonas activas en una imagen para rectángulos de selección que identifiquen toda la imagen.

### <a name="text-recognition"></a>Reconocimiento de texto

Si la imagen contiene texto que reconoce el servicio, una de las etiquetas contendrá una conclusión (acción) de TextResults. El elemento `displayName` de la conclusión contiene el texto reconocido. 

```json
    {
        "image" : {
            "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23Text..."
        },
        "displayName" : "##TextRecognition",
        "boundingBox" : {
            "queryRectangle" : {
                "topLeft" : {"x" : 0, "y" : 0},
                "topRight" : {"x" : 1, "y" : 0},
                "bottomRight" : {"x" : 1, "y" : 1},
                "bottomLeft" : {"x" : 0, "y" : 1}
            },
            "displayRectangle" : {
                "topLeft" : {"x" : 0, "y" : 0},
                "topRight" : {"x" : 1, "y" : 0},
                "bottomRight" : {"x" : 1, "y" : 1},
                "bottomLeft" : {"x" : 0, "y" : 1}
            }
        },
        "actions" : [{
            "displayName" : "WALK BIKE ACROSS BRIDGE",
            "actionType" : "TextResults"
        }],
        "sources" : ["OCR"]
    }
```

Dado que el campo `displayName` de la etiqueta contiene ##TextRecognition, no lo use como título de categoría en la experiencia de usuario. Esto se aplica a cualquier nombre para mostrar que empiece por ##. En su lugar, use el nombre para mostrar de la acción.


El reconocimiento de texto también puede reconocer la información de contacto en tarjetas de presentación, como números de teléfono y direcciones de correo electrónico. El rectángulo delimitador identifica la ubicación de la información de contacto en la tarjeta. 

```json
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0.635, "y" : 0},
          "topRight" : {"x" : 0.77, "y" : 0},
          "bottomRight" : {"x" : 0.77, "y" : 0.4873333},
          "bottomLeft" : {"x" : 0.635, "y" : 0.4873333}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0.635, "y" : 0},
          "topRight" : {"x" : 0.77, "y" : 0},
          "bottomRight" : {"x" : 0.77, "y" : 0.4873333},
          "bottomLeft" : {"x" : 0.635, "y" : 0.4873333}
        }
      },
      "actions" : [
        {
          "url" : "tel:888%20555%201212",
          "actionType" : "Uri"
        }
      ],
      "sources" : ["OCR"]
    },
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0.63, "y" : 0},
          "topRight" : {"x" : 0.866, "y" : 0},
          "bottomRight" : {"x" : 0.866, "y" : 0.5553334},
          "bottomLeft" : {"x" : 0.63, "y" : 0.5553334}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0.63, "y" : 0},
          "topRight" : {"x" : 0.866, "y" : 0},
          "bottomRight" : {"x" : 0.866, "y" : 0.5553334},
          "bottomLeft" : {"x" : 0.63, "y" : 0.5553334}
        }
      },
      "actions" : [
        {
          "url" : "mailto:someone@outlook.com",
          "actionType" : "Uri"
        }
      ],
      "sources" : ["OCR"]
    },
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse3.mm.bing.net\/th?q=%23%23TextRecognition..."
      },
      "displayName" : "##TextRecognition",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0, "y" : 0},
          "topRight" : {"x" : 1, "y" : 0},
          "bottomRight" : {"x" : 1, "y" : 1},
          "bottomLeft" : {"x" : 0, "y" : 1}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0, "y" : 0},
          "topRight" : {"x" : 1, "y" : 0},
          "bottomRight" : {"x" : 1, "y" : 1},
          "bottomLeft" : {"x" : 0, "y" : 1}
        }
      },
      "actions" : [
        {
          "displayName" : "CHARLENE WHITNEY Graphic Designer 888 555 1212 someone@outlook.com www.contoso.com",
          "actionType" : "TextResults"
        }
      ],
      "sources" : ["OCR"]
    }
```

Si la imagen contiene una entidad reconocida, como una persona, lugar o cosa, una de las etiquetas puede incluir una conclusión de entidad. Las entidades también pueden incluir curiosidades, tal como se muestra en el ejemplo siguiente:

```json
    {
      "image" : {
        "thumbnailUrl" : "https:\/\/tse4.mm.bing.net\/th?q=Statue+of+Liberty..."
      },
      "displayName" : "Statue of Liberty",
      "boundingBox" : {
        "queryRectangle" : {
          "topLeft" : {"x" : 0.40625, "y" : 0.1757813},
          "topRight" : {"x" : 0.6171875, "y" : 0.1757813},
          "bottomRight" : {"x" : 0.6171875, "y" : 0.3867188},
          "bottomLeft" : {"x" : 0.40625, "y" : 0.3867188}
        },
        "displayRectangle" : {
          "topLeft" : {"x" : 0.40625, "y" : 0.1757813},
          "topRight" : {"x" : 0.6171875, "y" : 0.1757813},
          "bottomRight" : {"x" : 0.6171875, "y" : 0.3867188},
          "bottomLeft" : {"x" : 0.40625, "y" : 0.3867188}
        }
      },
      "actions" : [
        {
          "_type" : "ImageEntityAction",
          "webSearchUrl" : "https:\/\/www.bing.com\/search?q=Statue+of+Liberty",
          "displayName" : "Statue of Liberty",
          "actionType" : "Entity",
        },
        {
          "_type" : "ImageModuleAction",
          "actionType" : "Trivia",
          "data" : {
            "value" : [
              {
                "name" : "Where was the cornerstone of the statue of liberty laid",
                "text" : "<the answer>",
                "hostPageUrl" : "http:\/\/contoso.com\/history\/...",
              },
              {
                "name" : "Why Is the Statue of Liberty Green",
                "text" : "<the answer>",
                "hostPageUrl" : "https:\/\/www.contoso.com\/why-statue-of-liberty-is-green",
              },
              {
                "name" : "What is the Statue of Liberty made of",
                "text" : "<the answer>",
                "hostPageUrl" : "https:\/\/www.contoso.com\/art-literature\/statue-liberty-made",
              }
            ]
          }
        }
      ]
    }
```



## <a name="next-steps"></a>Pasos siguientes

Para empezar a trabajar rápidamente con la primera solicitud, consulte las guías de inicio rápido sobre [C#](quickstarts/csharp.md) | [Java](quickstarts/java.md) | [node.js](quickstarts/nodejs.md) | [Python](quickstarts/python.md).

Pruebe la API. Vaya a la [consola de prueba de Visual Search API](https://dev.cognitive.microsoft.com/docs/services/878c38e705b84442845e22c7bff8c9ac). 


Familiarícese con la [referencia de Visual Search API](https://aka.ms/bingvisualsearchreferencedoc). La referencia contiene la lista de los puntos de conexión, encabezados y parámetros de consulta que se usan para solicitar los resultados de la búsqueda. También incluye definiciones de los objetos de respuesta. 

No olvide leer los [Requisitos de visualización y uso de Bing](./use-and-display-requirements.md) para evitar infringir alguna de las reglas sobre el uso de los resultados de búsqueda.


