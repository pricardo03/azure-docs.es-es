---
title: Translator Text API V2.0
titleSuffix: Azure Cognitive Services
description: Documentación de referencia para Translator Text API V2.0.
services: cognitive-services
author: v-pawal
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: reference
ms.date: 05/15/2018
ms.author: v-jansko
ms.openlocfilehash: 961dd277034db7e5406e671233f26b4fd8fe5f26
ms.sourcegitcommit: 1c2cf60ff7da5e1e01952ed18ea9a85ba333774c
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/12/2019
ms.locfileid: "59527292"
---
# <a name="translator-text-api-v20"></a>Translator Text API v2.0

> [!IMPORTANT]
> Esta versión de Translator Text API está en desuso. [Consulte la documentación de v3 de Translator Text API](v3-0-reference.md).

Translator Text API V2 puede integrarse perfectamente en las aplicaciones, sitios web, herramientas u otras soluciones para proporcionar experiencias de usuario con varios idiomas. El aprovechamiento de los estándares del sector se puede utilizar en cualquier plataforma de hardware y con cualquier sistema operativo para realizar la traducción de idiomas y otras operaciones lingüísticas relacionadas, como la detección del idioma de texto o texto a voz. Haga clic aquí para obtener más información acerca de Microsoft Translator API.

## <a name="getting-started"></a>Introducción
Para acceder a Translator Text API, debe [suscribirse a Microsoft Azure](../translator-text-how-to-signup.md).

## <a name="authorization"></a>Autorización
Todas las llamadas a Translator Text API requieren una clave de suscripción para la autenticación. La API admite dos modos de autenticación:

* Mediante un token de acceso. Use la clave de suscripción a la que se hace referencia en el **paso** 9 para generar un token de acceso mediante la realización de una solicitud POST en el servicio de autorización. Consulte la documentación de servicio de token para obtener más información. Pase el token de acceso al servicio de Traductor con el encabezado de autorización o el parámetro de consulta access_token. Cada token de acceso tiene una validez de 10 minutos. Obtenga un nuevo token de acceso cada 10 minutos y siga usando el mismo para solicitudes repetidas durante esos 10 minutos.

* Mediante una clave de suscripción directamente. Pase la clave de suscripción como valor en el encabezado de `Ocp-Apim-Subscription-Key` junto con la solicitud a Translator API. En este modo, no tiene que llamar al servicio de token de autenticación para generar el token de acceso.

Considere la clave de suscripción y el token de acceso como secretos que deben ocultarse.

## <a name="profanity-handling"></a>Control de palabras soeces
Normalmente, el servicio de Traductor conserva las palabras soeces que están presentes en el origen de la traducción. El grado de palabras soeces y el contexto que las convierte en ello difieren entre culturales y, como resultado, el grado de palabras soeces del idioma de destino puede aumentar o reducirse.

Si quiere evitar obtener palabras soeces en la traducción, independientemente de su presencia en el texto de origen, puede usar la opción de filtrado de palabras soeces en los métodos que la admiten. La opción le permite elegir si quiere ver las palabras soeces eliminadas o marcadas con etiquetas adecuadas, o bien si no quiere realizar ninguna acción. Los valores aceptados de `ProfanityAction` son `NoAction` (valor predeterminado), Marcado y `Deleted`.


|ProfanityAction    |. |Ejemplo de origen (japonés)  |Ejemplo de traducción (inglés)  |
|:--|:--|:--|:--|
|NoAction   |Predeterminada. Igual que si no se configura la opción. Las palabras soeces pasarán del origen al destino.        |彼はジャッカスです。     |Es un idiota.   |
|Marked     |Palabras soeces aparecerá rodeadas por etiquetas XML \<blasfemias > y \</profanity >.       |彼はジャッカスです。 |Es un \<blasfemias > jackass\</profanity >.  |
|Deleted    |Las palabras soeces se quitarán de la salida sin reemplazo.     |彼はジャッカスです。 |Es un.   |

    
## <a name="excluding-content-from-translation"></a>Contenido excluido de la traducción
Al traducir el contenido con etiquetas, como HTML (`contentType=text/html`), a veces resulta útil para excluir contenido específico de la traducción. Puede utilizar el atributo `class=notranslate` para especificar contenido que debería permanecer en el idioma original. En el ejemplo siguiente, el contenido del primer elemento `div` no se traducirá, mientras que el del segundo elemento `div`, sí.

```HTML
<div class="notranslate">This will not be translated.</div>
<div>This will be translated. </div>
```

## <a name="get-translate"></a>GET /Translate

### <a name="implementation-notes"></a>Notas de implementación
Traduce una cadena de texto de un idioma a otro.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/Translate`.

**Valor devuelto:** Una cadena que representa el texto traducido.

Si anteriormente se usaba `AddTranslation` o `AddTranslationArray` para introducir una traducción con una clasificación de 5 o superior para la misma oración de origen, `Translate` devuelve solo la principal opción que está disponible para el sistema. "Misma oración de origen" significa exactamente lo mismo (coincidencia 100 %), excepto en el caso del uso de mayúsculas y minúsculas, espacio en blanco, valores de etiquetas y puntuación al final de una oración. Si no se almacena ninguna clasificación con una puntuación de 5 o superior, el resultado devuelto será la traducción automática de Microsoft Translator.

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)

string

Tipo de contenido de la respuesta: application/xml 

### <a name="parameters"></a>Parámetros

|Parámetro|Valor|DESCRIPCIÓN    |Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|appid  |(vacío)    |Necesario. Si se usa el encabezado Ocp-Apim-Subscription-Key o de autorización, deje el campo appid vacío o incluya una cadena que contenga "Portador" + " " + "access_token".|query|string|
|text|(vacío)   |Necesario. Una cadena que representa el texto que se va a traducir. El tamaño del texto no debe superarlos 10 000 caracteres.|query|string|
|De|(vacío)   |Opcional. Una cadena que representa el código de idioma del texto de traducción. Por ejemplo, en para inglés.|query|string|
|to|(vacío) |Necesario. Una cadena que representa el código de idioma al que se va a traducir el texto.|query|string|
|contentType|(vacío)    |Opcional. El formato del texto que se va a traducir. Los formatos admitidos son text/plain (valor predeterminado) y text/html. El código HTML debe ser un elemento completo y bien formado.|query|string|
|categoría|(vacío)   |Opcional. Una cadena que contiene la categoría (dominio) de la traducción. El valor predeterminado es "general".|query|string|
|Autorización|(vacío)  |Es necesario si no se ha especificado el campo appid o el encabezado Ocp-Apim-Subscription-Key. Token de autorización:  "Portador" + " " + "token_de_acceso".|encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)  |Es necesario si no se ha especificado el campo appid o el encabezado de autorización.|encabezado|string|


### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401    |Credenciales no válidas|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta X-MS-Trans-Info.|
|503    |Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="post-translatearray"></a>POST /TranslateArray

### <a name="implementation-notes"></a>Notas de implementación
Use el método `TranslateArray` para recuperar traducciones de varios textos de origen.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/TranslateArray`.

El formato del cuerpo de la solicitud debe ser como se indica a continuación:

```
<TranslateArrayRequest>
  <AppId />
  <From>language-code</From>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <ReservedFlags xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" />
    <State xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >int-value</State>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</Uri>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" >string-value</User>
  </Options>
  <Texts>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
  </Texts>
  <To>language-code</To>
</TranslateArrayRequest>
```

Los elementos dentro de `TranslateArrayRequest` son:


* `appid`: Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje el campo appid vacío o incluya una cadena que contenga `"Bearer" + " " + "access_token"`.
* `from`: Opcional. Una cadena que representa el código de idioma del que se va a traducir el texto. Si se deja vacía, la respuesta incluirá el resultado de la detección automática de idioma.
* `options`: Opcional. Un objeto `Options` que contiene los valores que se indican a continuación. Son opcionales y los valores predeterminados son las opciones de configuración más comunes. Los elementos especificados deben incluirse en orden alfabético.
    - `Category`: Una cadena que contiene la categoría (dominio) de la traducción. De manera predeterminada, su valor es `general`.
    - `ContentType`: El formato del texto que se va a traducir. Los formatos admitidos son `text/plain` (valor predeterminado), `text/xml` y `text/html`. El código HTML debe ser un elemento completo y bien formado.
    - `ProfanityAction`: Especifica cómo se tratan las blasfemias según se explicó anteriormente. Los valores aceptados de `ProfanityAction` son `NoAction` (valor predeterminado), `Marked` y `Deleted`.
    - `State`: Estado del usuario para ayudar a poner en correlación la solicitud y la respuesta. Se devolverá el mismo contenido en la respuesta.
    - `Uri`: Filtrar los resultados por este URI. Valor predeterminado: `all`.
    - `User`: Filtrar los resultados por este usuario. Valor predeterminado: `all`.
* `texts`: Necesario. Matriz que contiene los textos para la traducción. Todas las cadenas deben ser del mismo idioma. El total de todos los textos que se deben traducir no debe superar los 10000 caracteres. El número máximo de elementos de la matriz es 2000.
* `to`: Necesario. Una cadena que representa el código de idioma al que se va a traducir el texto.

Los elementos opcionales pueden omitirse. Los elementos que son elementos secundarios directos de TranslateArrayRequest deben aparecer en orden alfabético.

El método TranslateArray acepta `application/xml` o `text/xml` para `Content-Type`.

**Valor devuelto:** Una matriz `TranslateArrayResponse`. Cada `TranslateArrayResponse` tiene los siguientes elementos:

* `Error`: Indica un error si se ha producido uno. En caso contrario, se establece en null.
* `OriginalSentenceLengths`: Una matriz de enteros que indica la longitud de cada oración del texto de origen original. La longitud de la matriz indica el número de oraciones.
* `TranslatedText`: El texto traducido.
* `TranslatedSentenceLengths`: Una matriz de enteros que indica la longitud de cada oración del texto traducido. La longitud de la matriz indica el número de oraciones.
* `State`: Estado del usuario para ayudar a poner en correlación la solicitud y la respuesta. Devuelve el mismo contenido que la solicitud.

El formato del cuerpo de la respuesta es el siguiente.

```
<ArrayOfTranslateArrayResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"
  xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <TranslateArrayResponse>
    <From>language-code</From>
    <OriginalTextSentenceLengths xmlns:a="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
      <a:int>int-value</a:int>
    </OriginalTextSentenceLengths>
    <State/>
    <TranslatedText>string-value</TranslatedText>
    <TranslatedTextSentenceLengths xmlns:a="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
      <a:int>int-value</a:int>
    </TranslatedTextSentenceLengths>
  </TranslateArrayResponse>
</ArrayOfTranslateArrayResponse>
```

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)
Una respuesta correcta incluye una matriz de `TranslateArrayResponse` en el formato descrito anteriormente.

string

Tipo de contenido de la respuesta: application/xml

### <a name="parameters"></a>Parámetros

|Parámetro|Valor|DESCRIPCIÓN|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|Autorización|(vacío)) |Es necesario si no se ha especificado el campo appid o el encabezado Ocp-Apim-Subscription-Key. Token de autorización:  "Portador" + " " + "token_de_acceso".|encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)|Es necesario si no se ha especificado el campo appid o el encabezado de autorización.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP   |Motivo|
|:--|:--|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado. Estos son algunos de los errores comunes: <ul><li>El elemento de matriz no puede estar vacío</li><li>Categoría no válida</li><li>El idioma de origen no es válido</li><li>El idioma de destino no es válido</li><li>La solicitud contiene demasiados elementos</li><li>El idioma de origen no se admite</li><li>El idioma de destino no se admite</li><li>La solicitud de traducción tiene demasiados datos</li><li>HTML no tiene el formato correcto</li><li>Se pasaron demasiadas cadenas en la solicitud de traducción</li></ul>|
|401    |Credenciales no válidas|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta X-MS-Trans-Info.|
|503    |Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="post-getlanguagenames"></a>POST /GetLanguageNames

### <a name="implementation-notes"></a>Notas de implementación
Recupera los nombres descriptivos de los idiomas que se pasaron como parámetro `languageCodes` y que se localizaron con el idioma de la configuración regional que se pasó.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/GetLanguageNames`.

El cuerpo de la solicitud incluye una matriz de cadenas que representa los códigos de idioma ISO 639-1 para los que se van a recuperar los nombres descriptivos. Por ejemplo: 

```
<ArrayOfstring xmlns:i="https://www.w3.org/2001/XMLSchema-instance"  xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>zh</string>
    <string>en</string>
</ArrayOfstring>
```

**Valor devuelto:** Una matriz de cadenas que contiene los nombres de idiomas que admite el servicio de Traductor, localizados en el idioma solicitado.

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)
Una matriz de cadenas que contiene los nombres de idiomas que admite el servicio de Traductor, localizados en el idioma solicitado.

string

Tipo de contenido de la respuesta: application/xml
 
### <a name="parameters"></a>Parámetros

|Parámetro|Valor|DESCRIPCIÓN|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|appid|(vacío)|Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje el campo appid vacío o incluya una cadena que contenga `"Bearer" + " " + "access_token"`.|query|string|
|locale|(vacío) |Necesario. Una cadena que representa una combinación de un código de referencia cultural de dos letras minúsculas de ISO 639 asociado con un idioma y un código de referencia de subcultura de dos letras mayúsculas de ISO 3166 para localizar los nombres de idiomas o un código de referencia cultural en minúscula de ISO 639 por sí solo.|query|string|
|Autorización|(vacío)  |Es necesario si no se ha especificado el campo appid o el encabezado `Ocp-Apim-Subscription-Key`. Token de autorización: `"Bearer" + " " + "access_token"`.|encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)  |Es necesario si no se ha especificado el campo appid o el encabezado `Authorization`.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401    |Credenciales no válidas|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta X-MS-Trans-Info.|
|503    |Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="get-getlanguagesfortranslate"></a>GET /GetLanguagesForTranslate

### <a name="implementation-notes"></a>Notas de implementación
Obtenga una lista de códigos de idioma que represente los idiomas que admite el servicio de traducción.  `Translate` y `TranslateArray` puede traducir entre cualquier par de estos idiomas.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForTranslate`.

**Valor devuelto:** Una matriz de cadenas que contiene los códigos de idioma que admiten los servicios de Traductor.

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)
Una matriz de cadenas que contiene los códigos de idioma que admiten los servicios de Traductor.

string

Tipo de contenido de la respuesta: application/xml
 
### <a name="parameters"></a>Parámetros

|Parámetro|Valor|DESCRIPCIÓN|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|appid|(vacío)|Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje el campo appid vacío o incluya una cadena que contenga `"Bearer" + " " + "access_token"`.|query|string|
|Autorización|(vacío)  |Es necesario si no se ha especificado el campo `appid` o el encabezado `Ocp-Apim-Subscription-Key`. Token de autorización: `"Bearer" + " " + "access_token"`.|encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)|Es necesario si no se ha especificado el campo `appid` o el encabezado `Authorization`.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401    |Credenciales no válidas|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta X-MS-Trans-Info.|
|503|Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="get-getlanguagesforspeak"></a>GET /GetLanguagesForSpeak

### <a name="implementation-notes"></a>Notas de implementación
Recupera los idiomas disponibles para la síntesis de voz.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/GetLanguagesForSpeak`.

**Valor devuelto:** Una matriz de cadenas que contiene los códigos de idioma que admite la síntesis de voz de los servicios de Traductor.

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)
Una matriz de cadenas que contiene los códigos de idioma que admite la síntesis de voz de los servicios de Traductor.

string

Tipo de contenido de la respuesta: application/xml

### <a name="parameters"></a>Parámetros

|Parámetro|Valor|DESCRIPCIÓN|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|appid|(vacío)|Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje el campo appid vacío o incluya una cadena que contenga `"Bearer" + " " + "access_token"`.|query|string|
|Autorización|(vacío)|Es necesario si no se ha especificado el campo `appid` o el encabezado `Ocp-Apim-Subscription-Key`. Token de autorización: `"Bearer" + " " + "access_token"`.|encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)|Es necesario si no se ha especificado el campo `appid` o el encabezado `Authorization`.|encabezado|string|
 
### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400|Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401|Credenciales no válidas|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta `X-MS-Trans-Info`.|
|503    |Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="get-speak"></a>GET /Speak

### <a name="implementation-notes"></a>Notas de implementación
Devuelve una secuencia con formato WAVE o mp3 del texto que se ha pasado reproducido en el idioma deseado.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/Speak`.

**Valor devuelto:** Una secuencia con formato WAVE o mp3 del texto que se ha pasado reproducido en el idioma deseado.

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)

binary

Tipo de contenido de la respuesta: application/xml 

### <a name="parameters"></a>Parámetros

|Parámetro|Valor|DESCRIPCIÓN|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|appid|(vacío)|Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje el campo appid vacío o incluya una cadena que contenga `"Bearer" + " " + "access_token"`.|query|string|
|text|(vacío)   |Necesario. Una cadena que contiene una o varias oraciones del idioma especificado que se va a reproducir para la secuencia con formato WAVE. El tamaño del texto que se va a reproducir no debe superar los 2000 caracteres.|query|string|
|language|(vacío)   |Necesario. Una cadena que representa el código de idioma admitido en el que se va a reproducir el texto. El código debe estar presente en la lista de códigos devueltos por el método `GetLanguagesForSpeak`.|query|string|
|formato|(vacío)|Opcional. Una cadena que especifica el Id. de content-type. Actualmente, `audio/wav` y `audio/mp3` están disponibles. El valor predeterminado es `audio/wav`.|query|string|
|options|(vacío)    |<ul><li>Opcional. Una cadena que especifica propiedades de la voz sintetizada:<li>`MaxQuality` y `MinSize` están disponibles para especificar la calidad de las señales de audio. Con `MaxQuality`, puede obtener las voces con la máxima calidad y, con `MinSize`, puede obtener las voces con el tamaño más pequeño. El valor predeterminado es `MinSize`.</li><li>`female` y `male` están disponibles para especificar el género de voz deseado. El valor predeterminado es `female`. Use la barra vertical <code>\|</code> para incluir varias opciones. Por ejemplo: `MaxQuality|Male`.</li></li></ul> |query|string|
|Autorización|(vacío)|Es necesario si no se ha especificado el campo `appid` o el encabezado `Ocp-Apim-Subscription-Key`. Token de autorización: `"Bearer" + " " + "access_token"`.|encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)  |Es necesario si no se ha especificado el campo `appid` o el encabezado `Authorization`.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401    |Credenciales no válidas|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta `X-MS-Trans-Info`.|
|503    |Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="get-detect"></a>GET /Detect

### <a name="implementation-notes"></a>Notas de implementación
Use el método `Detect` para identificar el idioma de una parte de texto seleccionada.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/Detect`.

**Valor devuelto:** Una cadena que contiene un código de idioma de dos caracteres para el texto indicado. .

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)

string

Tipo de contenido de la respuesta: application/xml

### <a name="parameters"></a>Parámetros

|Parámetro|Valor|DESCRIPCIÓN|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|appid|(vacío)  |Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje el campo appid vacío o incluya una cadena que contenga `"Bearer" + " " + "access_token"`.|query|string|
|text|(vacío)|Necesario. Una cadena que contiene el texto cuyo idioma se va a identificar. El tamaño del texto no debe superarlos 10 000 caracteres.|query| string|
|Autorización|(vacío)|Es necesario si no se ha especificado el campo `appid` o el encabezado `Ocp-Apim-Subscription-Key`. Token de autorización: `"Bearer" + " " + "access_token"`.|encabezado|string|
|Ocp-Apim-Subscription-Key  |(vacío)    |Es necesario si no se ha especificado el campo `appid` o el encabezado `Authorization`.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400|Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401    |Credenciales no válidas|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta `X-MS-Trans-Info`.|
|503    |Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|


## <a name="post-detectarray"></a>POST /DetectArray

### <a name="implementation-notes"></a>Notas de implementación
Use el método `DetectArray` para identificar el idioma de una matriz de cadena de una vez. Realiza la detección independiente de cada elemento de matriz individual y devuelve un resultado para cada fila de la matriz.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/DetectArray`.

El formato del cuerpo de la solicitud debe ser como se indica a continuación.

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">
    <string>string-value-1</string>
    <string>string-value-2</string>
</ArrayOfstring>
```

El tamaño del texto no debe superarlos 10 000 caracteres.

**Valor devuelto:** Una matriz de cadenas que contiene un código de idioma de dos caracteres para cada fila de la matriz de entrada.

El formato del cuerpo de la respuesta es el siguiente.

```
<ArrayOfstring xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays" xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <string>language-code-1</string>
  <string>language-code-2</string>
</ArrayOfstring>
```

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)
DetectArray fue correcta. Devuelve una matriz de cadenas que contiene un código de idioma de dos caracteres para cada fila de la matriz de entrada.

string

Tipo de contenido de la respuesta: application/xml
 
### <a name="parameters"></a>Parámetros

|Parámetro|Valor|DESCRIPCIÓN|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|appid|(vacío)|Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje el campo appid vacío o incluya una cadena que contenga `"Bearer" + " " + "access_token"`.|query|string|
|Autorización|(vacío)|Es necesario si no se ha especificado el campo `appid` o el encabezado `Ocp-Apim-Subscription-Key`. Token de autorización: `"Bearer" + " " + "access_token"`.|encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)|Es necesario si no se ha especificado el campo `appid` o el encabezado de autorización.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401    |Credenciales no válidas|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta X-MS-Trans-Info.|
|503    |Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="get-addtranslation"></a>GET /AddTranslation

### <a name="implementation-notes"></a>Notas de implementación

> [!IMPORTANT]
> **NOTA DE DESUSO:** Después del 31 de enero de 2018, este método no aceptará envíos de nuevas oraciones, y el usuario obtendrá un mensaje de error. Consulte este anuncio acerca de los cambios realizados en las funciones de Collaborative Translation.

Agrega una traducción a la memoria de traducción.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/AddTranslation`.

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)

string

Tipo de contenido de la respuesta: aplicación: xml
 
### <a name="parameters"></a>Parámetros

|Parámetro|Valor|DESCRIPCIÓN|Tipo de parámetro|Tipo de datos   |
|:--|:--|:--|:--|:--|
|appid|(vacío)|Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje el campo appid vacío o incluya una cadena que contenga `"Bearer" + " " + "access_token"`.|query|string|
|originalText|(vacío)|Necesario. Una cadena que contiene el texto del que se va a traducir. La cadena tiene una longitud máxima de 1000 caracteres.|query|string|
|translatedText|(vacío) |Necesario. Una cadena que contiene el texto traducido en el idioma de destino. La cadena tiene una longitud máxima de 2000 caracteres.|query|string|
|De|(vacío)   |Necesario. Una cadena que representa el código de idioma del texto de traducción. en = inglés, de = alemán, etc.|query|string|
|to|(vacío)|Necesario. Una cadena que representa el código de idioma al que se va a traducir el texto.|query|string|
|rating|(vacío) |Opcional. Un entero que representa la clasificación de calidad de esta cadena. Valor entre -10 y 10. De manera predeterminada, su valor es 1.|query|integer|
|contentType|(vacío)    |Opcional. El formato del texto que se va a traducir. Los formatos admitidos son "text/plain" and "text/html". El código HTML debe ser un elemento completo y bien formado.   |query|string|
|categoría|(vacío)|Opcional. Una cadena que contiene la categoría (dominio) de la traducción. El valor predeterminado es "general".|query|string|
|user|(vacío)|Necesario. Una cadena que se usa para realizar el seguimiento del envío.|query|string|
|uri|(vacío)|Opcional. Una cadena que contiene la ubicación del contenido de esta traducción.|query|string|
|Autorización|(vacío)|Es necesario si no se ha especificado el campo appid o el encabezado `Ocp-Apim-Subscription-Key`. Token de autorización: `"Bearer" + " " + "access_token"`.    |encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)|Es necesario si no se ha especificado el campo `appid` o el encabezado `Authorization`.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401    |Credenciales no válidas|
|410|Ya no se admite AddTranslation.|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta X-MS-Trans-Info.|
|503    |Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="post-addtranslationarray"></a>POST /AddTranslationArray

### <a name="implementation-notes"></a>Notas de implementación

> [!IMPORTANT]
> **NOTA DE DESUSO:** Después del 31 de enero de 2018, este método no aceptará envíos de nuevas oraciones, y el usuario obtendrá un mensaje de error. Consulte este anuncio acerca de los cambios realizados en las funciones de Collaborative Translation.

Agrega una matriz de traducciones para agregar memoria de traducción. Se trata de una versión de la matriz de `AddTranslation`.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/AddTranslationArray`.

El formato del cuerpo de la solicitud es el siguiente.

```
<AddtranslationsRequest>
  <AppId></AppId>
  <From>A string containing the language code of the source language</From>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</User>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Uri>
  </Options>
  <To>A string containing the language code of the target language</To>
  <Translations>
    <Translation xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">
      <OriginalText>string-value</OriginalText>
      <Rating>int-value</Rating>
      <Sequence>int-value</Sequence>
      <TranslatedText>string-value</TranslatedText>
    </Translation>
  </Translations>
</AddtranslationsRequest>
```

Los elementos dentro del elemento AddtranslationsRequest son:

* `AppId`: Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje el campo appid vacío o incluya una cadena que contenga `"Bearer" + " " + "access_token"`.
* `From`: Necesario. Una cadena que contiene el código de idioma del idioma de origen. Debe ser uno de los idiomas devueltos por el método `GetLanguagesForTranslate`.
* `To`: Necesario. Una cadena que contiene el código de idioma del idioma de destino. Debe ser uno de los idiomas devueltos por el método `GetLanguagesForTranslate`.
* `Translations`: Necesario. Una matriz de traducciones para agregar memoria de traducción. Cada traducción debe contener: originalText, translatedText y la clasificación. El tamaño de cada originalText y translatedText está limitado a 1000 caracteres. El total de todos los originalText y translatedText no debe superar los 10000 caracteres. El número máximo de elementos de la matriz es 100.
* `Options`: Necesario. Un conjunto de opciones, incluidos la categoría, el tipo de contenido, el URI y el usuario. El usuario es obligatorio. La categoría, el tipo de contenido y el URI son opcionales. Los elementos especificados deben incluirse en orden alfabético.

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)
El método AddTranslationArray fue correcto. Después del 31 de enero de 2018, no se aceptarán envíos de oraciones. El servicio responderá con el código de error 410.

string

Tipo de contenido de la respuesta: application/xml
 
### <a name="parameters"></a>Parámetros

|Parámetro|Valor|DESCRIPCIÓN|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|Autorización|(vacío)|Es necesario si no se ha especificado el campo appid o el encabezado Ocp-Apim-Subscription-Key. Token de autorización:  "Portador" + " " + "token_de_acceso".|encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)|Es necesario si no se ha especificado el campo appid o el encabezado de autorización.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401    |Credenciales no válidas|
|410    |Ya no se admite AddTranslation.|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta `X-MS-Trans-Info`.|
|503|Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="get-breaksentences"></a>GET /BreakSentences

### <a name="implementation-notes"></a>Notas de implementación
Divide un fragmento de texto en oraciones y devuelve una matriz que contiene la longitud de cada oración.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/BreakSentences`.

**Valor devuelto:** Una matriz de enteros que representan las longitudes de las oraciones. La longitud de la matriz es el número de oraciones y los valores son la longitud de cada oración.

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)
Una matriz de enteros que representan las longitudes de las oraciones. La longitud de la matriz es el número de oraciones y los valores son la longitud de cada oración.

integer

Tipo de contenido de la respuesta: application/xml 

### <a name="parameters"></a>Parámetros

|Parámetro|Valor|DESCRIPCIÓN|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|appid|(vacío)  |Necesario. Si se usa el encabezado Ocp-Apim-Subscription-Key o de autorización, deje el campo appid vacío o incluya una cadena que contenga "Portador" + " " + "access_token".|query| string|
|text|(vacío)   |Necesario. Una cadena que representa el texto que se va a dividir en oraciones. El tamaño del texto no debe superarlos 10 000 caracteres.|query|string|
|language   |(vacío)    |Necesario. Una cadena que representa el código de idioma del texto de entrada.|query|string|
|Autorización|(vacío)|Es necesario si no se ha especificado el campo appid o el encabezado Ocp-Apim-Subscription-Key. Token de autorización:  "Portador" + " " + "token_de_acceso".    |encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)|Es necesario si no se ha especificado el campo appid o el encabezado de autorización.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400|Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401|Credenciales no válidas|
|500|Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta X-MS-Trans-Info.|
|503|Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="post-gettranslations"></a>POST /GetTranslations

### <a name="implementation-notes"></a>Notas de implementación
Recupera una matriz de traducciones de un par de idiomas determinado desde el almacén y el motor de traducción automática. GetTranslations difiere de la traducción ya que devuelve todas las traducciones disponibles.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/GetTranslations`.

El cuerpo de la solicitud incluye el objeto opcional TranslationOptions, que tiene el formato siguiente.

```
<TranslateOptions xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">
  <Category>string-value</Category>
  <ContentType>text/plain</ContentType>
  <ReservedFlags></ReservedFlags>
  <State>int-value</State>
  <Uri>string-value</Uri>
  <User>string-value</User>
</TranslateOptions>
```

El objeto `TranslateOptions` contiene los valores que se indican a continuación. Son opcionales y los valores predeterminados son las opciones de configuración más comunes. Los elementos especificados deben incluirse en orden alfabético.

* `Category`: Una cadena que contiene la categoría (dominio) de la traducción. El valor predeterminado es "general".
* `ContentType`: La única opción admitida, así como la predeterminada, es "text/plain".
* `IncludeMultipleMTAlternatives`: una marca booleana para determinar si se debe devolver más de una alternativa del motor de traducción automática. Los valores válidos son true y false (distingue mayúsculas de minúsculas). El valor predeterminado es false e incluye solo una alternativa. El establecimiento de la marca en true permite generar alternativas artificiales en la traducción, totalmente integradas con Collaborative Translation Framework (CTF). La característica permite la devolución de alternativas de oraciones que no tienen ninguna alternativa en CTF, agregando alternativas artificiales de la lista de las mejores opciones del descodificador.
    - Clasificaciones: las clasificaciones se aplican como sigue: (1) La mejor traducción automática tiene una clasificación de 5. (2) Las alternativas de CTF reflejan la autoridad del revisor, de -10 a + 10. (3) Las alternativas de traducción (las mejores opciones) generadas automáticamente tienen una clasificación de 0 y un grado de coincidencia de 100.
    - Número de alternativas: el número de alternativas devueltas va hasta maxTranslations, pero puede ser menor.
    - Pares de idiomas: esta funcionalidad no está disponible para las traducciones entre chino simplificado y tradicional, ambas direcciones. Está disponible para el resto de pares de idiomas admitidos de Microsoft Translator.
* `State`: Estado del usuario para ayudar a poner en correlación la solicitud y la respuesta. Se devolverá el mismo contenido en la respuesta.
* `Uri`: Filtrar los resultados por este URI. Si no se establece ningún valor, el valor predeterminado es todo.
* `User`: Filtrar los resultados por este usuario. Si no se establece ningún valor, el valor predeterminado es todo.

La solicitud `Content-Type` debe ser `text/xml`.

**Valor devuelto:** El formato de la respuesta es el siguiente.

```
<GetTranslationsResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"
  xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <From>Two character language code</From>
  <State/>
  <Translations>
    <TranslationMatch>
      <Count>int-value</Count>
      <MatchDegree>int-value</MatchDegree>
      <MatchedOriginalText/>
      <Rating>int value</Rating>
      <TranslatedText>string-value</TranslatedText>
    </TranslationMatch>
  </Translations>
</GetTranslationsResponse>
```

Esto incluye un elemento `GetTranslationsResponse` que contiene los valores siguientes:

* `Translations`: Una matriz de las coincidencias encontradas, almacenadas en objetos TranslationMatch (ver a continuación). Las traducciones pueden incluir ligeras variantes del texto original (coincidencia aproximada). Las traducciones se ordenarán: las coincidencias del 100 % en primer lugar, luego las coincidencias aproximadas.
* `From`: Si el método no especificó un idioma de origen, será el resultado de la detección automática de idioma. En caso contrario, será el idioma de origen indicado.
* `State`: Estado del usuario para ayudar a poner en correlación la solicitud y la respuesta. Contiene el mismo valor tal como se indica en el parámetro TranslateOptions.

El objeto TranslationMatch consta del siguiente contenido:

* `Error`: Si se ha producido un error de una cadena de entrada específica, se almacena el código de error. En caso contrario, el campo está vacío.
* `MatchDegree`: El sistema hace coincidir las oraciones según el almacén, incluidas las coincidencias inexactas.  MatchDegree indica el grado de coincidencia del texto de entrada con el texto original encontrado en el almacén. El valor no devuelto oscila entre 0 y 100, donde 0 equivale a ninguna similitud y 100 a una coincidencia exacta con distinción de mayúsculas y minúsculas.
MatchedOriginalText: El texto original del que se encontraron coincidencias para este resultado. Solo se devuelve si el texto original de la coincidencia era diferente del texto de entrada. Se utiliza para devolver el texto de origen de una coincidencia aproximada. No se devuelve para los resultados de Microsoft Translator.
* `Rating`: Indica la autoridad de la persona que toma la decisión de calidad. Los resultados de la traducción automática tendrán una clasificación de 5. Las traducciones proporcionadas de forma anónima normalmente tendrán una clasificación de 1 a 4, mientras que las proporcionadas de forma autoritativa normalmente tendrán una clasificación de 6 a 10.
* `Count`: El número de veces que se ha seleccionado esta traducción con esta clasificación. El valor será 0 para la respuesta traducida automáticamente.
* `TranslatedText`: El texto traducido.

### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)
Un objeto `GetTranslationsResponse` en el formato que se ha descrito anteriormente.

string

Tipo de contenido de la respuesta: application/xml
 
### <a name="parameters"></a>Parámetros

|Parámetro|Valor|DESCRIPCIÓN|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|appid|(vacío)|Necesario. Si se usa el encabezado `Authorization` o `Ocp-Apim-Subscription-Key`, deje el campo appid vacío o incluya una cadena que contenga `"Bearer" + " " + "access_token"`.|query|string|
|text|(vacío)|Necesario. Una cadena que representa el texto que se va a traducir. El tamaño del texto no debe superarlos 10 000 caracteres.|query|string|
|De|(vacío)|Necesario. Una cadena que representa el código de idioma del texto de traducción.|query|string|
|to |(vacío)    |Necesario. Una cadena que representa el código de idioma al que se va a traducir el texto.|query|string|
|maxTranslations|(vacío)|Necesario. Un entero que representa el número máximo de traducciones que se van a devolver.|query|integer|
|Autorización| (vacío)|Es necesario si no se ha especificado el campo `appid` o el encabezado `Ocp-Apim-Subscription-Key`. Token de autorización: `"Bearer" + " " + "access_token"`.|string| encabezado|
|Ocp-Apim-Subscription-Key|(vacío)  |Es necesario si no se ha especificado el campo `appid` o el encabezado `Authorization`.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401    |Credenciales no válidas|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta `X-MS-Trans-Info`.|
|503|Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="post-gettranslationsarray"></a>POST /GetTranslationsArray

### <a name="implementation-notes"></a>Notas de implementación
Use el método `GetTranslationsArray` para recuperar varios candidatos de traducción de varios textos de origen.

El URI de solicitud es `https://api.microsofttranslator.com/V2/Http.svc/GetTranslationsArray`.

El formato del cuerpo de la solicitud es el siguiente.

```
<GetTranslationsArrayRequest>
  <AppId></AppId>
  <From>language-code</From>
  <MaxTranslations>int-value</MaxTranslations>
  <Options>
    <Category xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Category>
    <ContentType xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">text/plain</ContentType>
    <ReservedFlags xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2"/>
    <State xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">int-value</State>
    <Uri xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</Uri>
    <User xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2">string-value</User>
  </Options>
  <Texts>
    <string xmlns="http://schemas.microsoft.com/2003/10/Serialization/Arrays">string-value</string>
  </Texts>
  <To>language-code</To>
</GetTranslationsArrayRequest>
```

`GetTranslationsArrayRequest` incluye los siguientes elementos:

* `AppId`: Necesario. Si se usa el encabezado de autorización, deje el campo appid vacío o incluya una cadena que contenga `"Bearer" + " " + "access_token"`.
* `From`: Necesario. Una cadena que representa el código de idioma del texto de traducción.
* `MaxTranslations`: Necesario. Un entero que representa el número máximo de traducciones que se van a devolver.
* `Options`: Opcional. Un objeto de Opciones que contiene los valores que se indican a continuación. Son opcionales y los valores predeterminados son las opciones de configuración más comunes. Los elementos especificados deben incluirse en orden alfabético.
    - Categoría: Una cadena que contiene la categoría (dominio) de la traducción. El valor predeterminado es general.
    - `ContentType`: La única opción admitida, así como la predeterminada, es text/plain.
    - `IncludeMultipleMTAlternatives`: una marca booleana para determinar si se debe devolver más de una alternativa del motor de traducción automática. Los valores válidos son true y false (distingue mayúsculas de minúsculas). El valor predeterminado es false e incluye solo una alternativa. El establecimiento de la marca en true permite generar alternativas artificiales en la traducción, totalmente integradas con Collaborative Translation Framework (CTF). La característica permite la devolución de alternativas de oraciones que no tienen ninguna alternativa en CTF, agregando alternativas artificiales de la lista de las mejores opciones del descodificador.
        - Clasificaciones: las clasificaciones se aplican como sigue: (1) La mejor traducción automática tiene una clasificación de 5. (2) Las alternativas de CTF reflejan la autoridad del revisor, de -10 a + 10. (3) Las alternativas de traducción (las mejores opciones) generadas automáticamente tienen una clasificación de 0 y un grado de coincidencia de 100.
        - Número de alternativas: el número de alternativas devueltas va hasta maxTranslations, pero puede ser menor.
        - Pares de idiomas: esta funcionalidad no está disponible para las traducciones entre chino simplificado y tradicional, ambas direcciones. Está disponible para el resto de pares de idiomas admitidos de Microsoft Translator.
* `State`: Estado del usuario para ayudar a poner en correlación la solicitud y la respuesta. Se devolverá el mismo contenido en la respuesta.
* `Uri`: Filtrar los resultados por este URI. Si no se establece ningún valor, el valor predeterminado es todo.
* `User`: Filtrar los resultados por este usuario. Si no se establece ningún valor, el valor predeterminado es todo.
* `Texts`: Necesario. Matriz que contiene los textos para la traducción. Todas las cadenas deben ser del mismo idioma. El total de todos los textos que se deben traducir no debe superar los 10000 caracteres. El número máximo de elementos de la matriz es 10.
* `To`: Necesario. Una cadena que representa el código de idioma al que se va a traducir el texto.

Los elementos opcionales pueden omitirse. Los elementos que son elementos secundarios directos de `GetTranslationsArrayRequest` deben aparecer en orden alfabético.

La solicitud `Content-Type` debe ser `text/xml`.

**Valor devuelto:** El formato de la respuesta es el siguiente.

```
<ArrayOfGetTranslationsResponse xmlns="http://schemas.datacontract.org/2004/07/Microsoft.MT.Web.Service.V2" xmlns:i="https://www.w3.org/2001/XMLSchema-instance">
  <GetTranslationsResponse>
    <From>language-code</From>
    <State/>
    <Translations>
      <TranslationMatch>
        <Count>int-value</Count>
        <MatchDegree>int-value</MatchDegree>
        <MatchedOriginalText>string-value</MatchedOriginalText>
        <Rating>int-value</Rating>
        <TranslatedText>string-value</TranslatedText>
      </TranslationMatch>
      <TranslationMatch>
        <Count>int-value</Count>
        <MatchDegree>int-value</MatchDegree>
        <MatchedOriginalText/>
        <Rating>int-value</Rating>
        <TranslatedText>string-value</TranslatedText>
      </TranslationMatch>
    </Translations>
  </GetTranslationsResponse>
</ArrayOfGetTranslationsResponse>
```

Cada elemento `GetTranslationsResponse` contiene los valores siguientes:

* `Translations`: Una matriz de las coincidencias encontradas, almacenadas en objetos `TranslationMatch` (ver a continuación). Las traducciones pueden incluir ligeras variantes del texto original (coincidencia aproximada). Las traducciones se ordenarán: las coincidencias del 100 % en primer lugar, luego las coincidencias aproximadas.
* `From`: Si el método no especificó un idioma `From`, será el resultado de la detección automática de idioma. En caso contrario, será el idioma de origen indicado.
* `State`: Estado del usuario para ayudar a poner en correlación la solicitud y la respuesta. Contiene el mismo valor tal como se indica en el parámetro `TranslateOptions`.

Un objeto `TranslationMatch` consta de lo siguiente:
* `Error`: Si se ha producido un error de una cadena de entrada específica, se almacena el código de error. En caso contrario, el campo está vacío.
* `MatchDegree`: El sistema hace coincidir las oraciones según el almacén, incluidas las coincidencias inexactas.  `MatchDegree` indica el grado de coincidencia del texto de entrada con el texto original encontrado en el almacén. El valor no devuelto oscila entre 0 y 100, donde 0 equivale a ninguna similitud y 100 a una coincidencia exacta con distinción de mayúsculas y minúsculas.
* `MatchedOriginalText`: El texto original del que se encontraron coincidencias para este resultado. Solo se devuelve si el texto original de la coincidencia era diferente del texto de entrada. Se utiliza para devolver el texto de origen de una coincidencia aproximada. No se devuelve para los resultados de Microsoft Translator.
* `Rating`: Indica la autoridad de la persona que toma la decisión de calidad. Los resultados de la traducción automática tendrán una clasificación de 5. Las traducciones proporcionadas de forma anónima normalmente tendrán una clasificación de 1 a 4, mientras que las proporcionadas de forma autoritativa normalmente tendrán una clasificación de 6 a 10.
* `Count`: El número de veces que se ha seleccionado esta traducción con esta clasificación. El valor será 0 para la respuesta traducida automáticamente.
* `TranslatedText`: El texto traducido.


### <a name="response-class-status-200"></a>Clase de respuesta (estado 200)

string

Tipo de contenido de la respuesta: application/xml
 
### <a name="parameters"></a>Parámetros

|Parámetro|Valor|DESCRIPCIÓN|Tipo de parámetro|Tipo de datos|
|:--|:--|:--|:--|:--|
|Autorización  |(vacío)    |Es necesario si no se ha especificado el campo `appid` o el encabezado `Ocp-Apim-Subscription-Key`. Token de autorización: `"Bearer" + " " + "access_token"`.|encabezado|string|
|Ocp-Apim-Subscription-Key|(vacío)  |Es necesario si no se ha especificado el campo `appid` o el encabezado `Authorization`.|encabezado|string|

### <a name="response-messages"></a>Mensajes de respuesta

|Código de estado HTTP|Motivo|
|:--|:--|
|400    |Solicitud incorrecta. Compruebe los parámetros de entrada y la respuesta del error detallado.|
|401    |Credenciales no válidas|
|500    |Error de servidor. Si el error continúa, infórmenos. Facilítenos la fecha y hora aproximadas de la solicitud, junto con el Id. de solicitud incluido en el encabezado de respuesta `X-MS-Trans-Info`.|
|503    |Servicio no disponible temporalmente. Vuelva a intentarlo e infórmenos si el error continúa.|

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Migración a v3 Translator Text API](../migrate-to-v3.md)










