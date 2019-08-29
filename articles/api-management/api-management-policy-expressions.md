---
title: Expresiones de las directivas de Azure API Management | Microsoft Docs
description: Obtenga información acerca de las expresiones de las directivas de Azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.assetid: ea160028-fc04-4782-aa26-4b8329df3448
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 03/22/2019
ms.author: apimpm
ms.openlocfilehash: fa5e84ba62896969458b84cf014e2b35ee869df7
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70072168"
---
# <a name="api-management-policy-expressions"></a>Expresiones de las directivas de API Management
Este artículo describe la sintaxis de expresiones de directiva C# 7. Cada expresión tiene acceso a la variable de [contexto](api-management-policy-expressions.md#ContextVariables) proporcionada de forma implícita y a un [subconjunto](api-management-policy-expressions.md#CLRTypes) permitido de tipos de .NET Framework.

Para obtener más información:

- Vea cómo proporcionar información de contexto al servicio back-end. Use las directivas [Establecer el parámetro de cadena de consulta](api-management-transformation-policies.md#SetQueryStringParameter) y [Establecer encabezado HTTP](api-management-transformation-policies.md#SetHTTPheader) para proporcionar esta información.
- Vea cómo utilizar la directiva de [validación de JWT](api-management-access-restriction-policies.md#ValidateJWT) para preautorizar el acceso a operaciones según notificaciones de token.
- Vea cómo se utiliza un seguimiento de [API Inspector](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) para ver cómo se evalúan las directivas y los resultados de las evaluaciones.
- Vea cómo usar expresiones con las directivas [Obtener de caché](api-management-caching-policies.md#GetFromCache) y [Almacenar en caché](api-management-caching-policies.md#StoreToCache) para configurar el almacenamiento en caché de la respuesta de API Management. Defina una duración que coincida con el almacenamiento en caché de la respuesta del servicio back-end especificado por la directiva `Cache-Control` del servicio back-end.
- Vea cómo realizar el filtrado de contenido. Quite elementos de datos de la respuesta recibida desde el servicio back-end mediante las directivas [Flujo de control](api-management-advanced-policies.md#choose) y [Establecer cuerpo](api-management-transformation-policies.md#SetBody).
- Para descargar las declaraciones de directiva, vaya a [api-management-samples/policies](https://github.com/Azure/api-management-samples/tree/master/policies) en el repositorio de GitHub.


## <a name="Syntax"></a> Sintaxis
Las expresiones de declaración única se incluyen en `@(expression)`, donde `expression` es una instrucción de expresión bien formada de C#.

Las expresiones de múltiples declaraciones se incluyen en `@{expression}`. Todas las rutas de código de las expresiones de múltiples declaraciones deben terminar con una declaración `return`.

## <a name="PolicyExpressionsExamples"></a> Ejemplos

```
@(true)

@((1+1).ToString())

@("Hi There".Length)

@(Regex.Match(context.Response.Headers.GetValueOrDefault("Cache-Control",""), @"max-age=(?<maxAge>\d+)").Groups["maxAge"]?.Value)

@(context.Variables.ContainsKey("maxAge") ? int.Parse((string)context.Variables["maxAge"]) : 3600)

@{
  string value;
  if (context.Request.Headers.TryGetValue("Authorization", out value))
  {
    return Encoding.UTF8.GetString(Convert.FromBase64String(value));
  }
  else
  {
    return null;
  }
}
```

## <a name="PolicyExpressionsUsage"></a>Uso
Las expresiones pueden utilizarse como valores de atributos o valores de texto en cualquier [directiva](api-management-policies.md) de API Management, a menos que la referencia de la directiva especifique lo contrario.

> [!IMPORTANT]
> Al utilizar expresiones de directiva, solo hay una verificación limitada de estas cuando se define la directiva. La puerta de enlace ejecuta las expresiones en tiempo de ejecución, y las excepciones generadas por las expresiones de directiva generan un error en tiempo de ejecución.

## <a name="CLRTypes"></a>Tipos de .NET framework que se permiten en expresiones de directiva
En la tabla siguiente se enumeran los tipos de .NET Framework que se permiten en las expresiones de directiva y sus miembros.

|type|Miembros compatibles|
|--------------|-----------------------|
|Newtonsoft.Json.Formatting|Todo|
|Newtonsoft.Json.JsonConvert|SerializeObject, DeserializeObject|
|Newtonsoft.Json.Linq.Extensions|Todo|
|Newtonsoft.Json.Linq.JArray|Todo|
|Newtonsoft.Json.Linq.JConstructor|Todo|
|Newtonsoft.Json.Linq.JContainer|Todo|
|Newtonsoft.Json.Linq.JObject|Todo|
|Newtonsoft.Json.Linq.JProperty|Todo|
|Newtonsoft.Json.Linq.JRaw|Todo|
|Newtonsoft.Json.Linq.JToken|Todo|
|Newtonsoft.Json.Linq.JTokenType|Todo|
|Newtonsoft.Json.Linq.JValue|Todo|
|System.Array|Todo|
|System.BitConverter|Todo|
|System.Boolean|Todo|
|System.Byte|Todo|
|System.Char|Todo|
|System.Collections.Generic.Dictionary<TKey, TValue>|Todo|
|System.Collections.Generic.HashSet\<T>|Todo|
|System.Collections.Generic.ICollection\<T>|Todo|
|System.Collections.Generic.IDictionary<TKey, TValue>|Todo|
|System.Collections.Generic.IEnumerable\<T>|Todo|
|System.Collections.Generic.IEnumerator\<T>|Todo|
|System.Collections.Generic.IList\<T>|Todo|
|System.Collections.Generic.IReadOnlyCollection\<T>|Todo|
|System.Collections.Generic.IReadOnlyDictionary<TKey, TValue>|Todo|
|System.Collections.Generic.ISet\<T>|Todo|
|System.Collections.Generic.KeyValuePair<TKey, TValue>|Todo|
|System.Collections.Generic.List\<T>|Todo|
|System.Collections.Generic.Queue\<T>|Todo|
|System.Collections.Generic.Stack\<T>|Todo|
|System.Convert|Todo|
|System.DateTime|(Constructor), Add, AddDays, AddHours, AddMilliseconds, AddMinutes, AddMonths, AddSeconds, AddTicks, AddYears, Date, Day, DayOfWeek, DayOfYear, DaysInMonth, Hour, IsDaylightSavingTime, IsLeapYear, MaxValue, Millisecond, Minute, MinValue, Month, Now, Parse, Second, Subtract, Ticks, TimeOfDay, Today, ToString, UtcNow, Year|
|System.DateTimeKind|UTC|
|System.DateTimeOffset|Todo|
|System.Decimal|Todo|
|System.Double|Todo|
|System.Exception|Todo|
|System.Guid|Todo|
|System.Int16|Todo|
|System.Int32|Todo|
|System.Int64|Todo|
|System.IO.StringReader|Todo|
|System.IO.StringWriter|Todo|
|System.Linq.Enumerable|Todo|
|System.Math|Todo|
|System.MidpointRounding|Todo|
|System.Net.WebUtility|Todo|
|System.Nullable|Todo|
|System.Random|Todo|
|System.SByte|Todo|
|System.Security.Cryptography.AsymmetricAlgorithm|Todo|
|System.Security.Cryptography.CipherMode|Todo|
|System.Security.Cryptography.HashAlgorithm|Todo|
|System.Security.Cryptography.HashAlgorithmName|Todo|
|System.Security.Cryptography.HMAC|Todo|
|System.Security.Cryptography.HMACMD5|Todo|
|System.Security.Cryptography.HMACSHA1|Todo|
|System.Security.Cryptography.HMACSHA256|Todo|
|System.Security.Cryptography.HMACSHA384|Todo|
|System.Security.Cryptography.HMACSHA512|Todo|
|System.Security.Cryptography.KeyedHashAlgorithm|Todo|
|System.Security.Cryptography.MD5|Todo|
|System.Security.Cryptography.Oid|Todo|
|System.Security.Cryptography.PaddingMode|Todo|
|System.Security.Cryptography.RNGCryptoServiceProvider|Todo|
|System.Security.Cryptography.RSA|Todo|
|System.Security.Cryptography.RSAEncryptionPadding|Todo|
|System.Security.Cryptography.RSASignaturePadding|Todo|
|System.Security.Cryptography.SHA1|Todo|
|System.Security.Cryptography.SHA1Managed|Todo|
|System.Security.Cryptography.SHA256|Todo|
|System.Security.Cryptography.SHA256Managed|Todo|
|System.Security.Cryptography.SHA384|Todo|
|System.Security.Cryptography.SHA384Managed|Todo|
|System.Security.Cryptography.SHA512|Todo|
|System.Security.Cryptography.SHA512Managed|Todo|
|System.Security.Cryptography.SymmetricAlgorithm|Todo|
|System.Security.Cryptography.X509Certificates.PublicKey|Todo|
|System.Security.Cryptography.X509Certificates.RSACertificateExtensions|Todo|
|System.Security.Cryptography.X509Certificates.X500DistinguishedName|NOMBRE|
|System.Security.Cryptography.X509Certificates.X509Certificate|Todo|
|System.Security.Cryptography.X509Certificates.X509Certificate2|Todo|
|System.Security.Cryptography.X509Certificates.X509ContentType|Todo|
|System.Security.Cryptography.X509Certificates.X509NameType|Todo|
|System.Single|Todo|
|System.String|Todo|
|System.StringComparer|Todo|
|System.StringComparison|Todo|
|System.StringSplitOptions|Todo|
|System.Text.Encoding|Todo|
|System.Text.RegularExpressions.Capture|Index, Length, Value.|
|System.Text.RegularExpressions.CaptureCollection|Count, Item.|
|System.Text.RegularExpressions.Group|Captures, Success.|
|System.Text.RegularExpressions.GroupCollection|Count, Item.|
|System.Text.RegularExpressions.Match|Empty, Groups, Result.|
|System.Text.RegularExpressions.Regex|(Constructor), IsMatch, Match, Matches, Replace, Unescape, Split|
|System.Text.RegularExpressions.RegexOptions|Todo|
|System.Text.StringBuilder|Todo|
|System.TimeSpan|Todo|
|System.TimeZone|Todo|
|System.TimeZoneInfo.AdjustmentRule|Todo|
|System.TimeZoneInfo.TransitionTime|Todo|
|System.TimeZoneInfo|Todo|
|System.Tuple|Todo|
|System.UInt16|Todo|
|System.UInt32|Todo|
|System.UInt64|Todo|
|System.Uri|Todo|
|System.UriPartial|Todo|
|System.Xml.Linq.Extensions|Todo|
|System.Xml.Linq.XAttribute|Todo|
|System.Xml.Linq.XCData|Todo|
|System.Xml.Linq.XComment|Todo|
|System.Xml.Linq.XContainer|Todo|
|System.Xml.Linq.XDeclaration|Todo|
|System.Xml.Linq.XDocument|Todos, excepto: Carga|
|System.Xml.Linq.XDocumentType|Todo|
|System.Xml.Linq.XElement|Todo|
|System.Xml.Linq.XName|Todo|
|System.Xml.Linq.XNamespace|Todo|
|System.Xml.Linq.XNode|Todo|
|System.Xml.Linq.XNodeDocumentOrderComparer|Todo|
|System.Xml.Linq.XNodeEqualityComparer|Todo|
|System.Xml.Linq.XObject|Todo|
|System.Xml.Linq.XProcessingInstruction|Todo|
|System.Xml.Linq.XText|Todo|
|System.Xml.XmlNodeType|Todo|

## <a name="ContextVariables"></a> Variable de contexto
Una variable denominada `context` está disponible implícitamente en todas las [expresiones](api-management-policy-expressions.md#Syntax) de directiva. Sus miembros proporcionan información relativa a `\request`. Todos los miembros de `context` son de solo lectura.

|Variable de contexto|Métodos, propiedades y valores de parámetro admitidos|
|----------------------|-------------------------------------------------------|
|context|[API](#ref-context-api): [IApi](#ref-iapi)<br /><br /> [Implementación](#ref-context-deployment)<br /><br /> Transcurrido: TimeSpan: intervalo de tiempo entre el valor de marca de tiempo y la hora actual<br /><br /> [LastError](#ref-context-lasterror)<br /><br /> [operación](#ref-context-operation)<br /><br /> [Producto](#ref-context-product)<br /><br /> [Solicitud](#ref-context-request)<br /><br /> RequestId: Guid: identificador único de la solicitud<br /><br /> [Respuesta](#ref-context-response)<br /><br /> [Suscripción](#ref-context-subscription)<br /><br /> Marca de tiempo: DateTime: punto en el tiempo en que se recibió la solicitud<br /><br /> Tracing: bool - indica si el seguimiento está activado o desactivado <br /><br /> [User](#ref-context-user)<br /><br /> [Variables](#ref-context-variables): IReadOnlyDictionary<cadena, objeto><br /><br /> void Trace(message: cadena)|
|<a id="ref-context-api"></a>context.Api|Id: cadena<br /><br /> IsCurrentRevision: bool<br /><br />  Name: cadena<br /><br /> Path: cadena<br /><br /> Revision: cadena<br /><br /> ServiceUrl: [IUrl](#ref-iurl)<br /><br /> Version: cadena |
|<a id="ref-context-deployment"></a>context.Deployment|Region: cadena<br /><br /> ServiceName: cadena<br /><br /> Certificates: IReadOnlyDictionary<cadena, X509Certificate2>|
|<a id="ref-context-lasterror"></a>context.LastError|Source: cadena<br /><br /> Reason: cadena<br /><br /> Message: cadena<br /><br /> Scope: cadena<br /><br /> Section: cadena<br /><br /> Path: cadena<br /><br /> PolicyId: cadena<br /><br /> Para obtener más información sobre context.LastError, consulte [Error handling in API Management policies](api-management-error-handling-policies.md) (Control de errores en directivas de API Management).|
|<a id="ref-context-operation"></a>context.Operation|Id: cadena<br /><br /> Method: cadena<br /><br /> Name: cadena<br /><br /> UrlTemplate: cadena|
|<a id="ref-context-product"></a>context.Product|Apis: IEnumerable<[IApi](#ref-iapi)\><br /><br /> ApprovalRequired: bool<br /><br /> Groups: IEnumerable<[IGroup](#ref-igroup)\><br /><br /> Id: cadena<br /><br /> Name: cadena<br /><br /> State: enum ProductState {NotPublished, Published}<br /><br /> SubscriptionLimit: int?<br /><br /> SubscriptionRequired: bool|
|<a id="ref-context-request"></a>context.Request|Cuerpo: [IMessageBody](#ref-imessagebody) o `null` si la solicitud no tiene un cuerpo.<br /><br /> Certificate: System.Security.Cryptography.X509Certificates.X509Certificate2<br /><br /> [Encabezados](#ref-context-request-headers): IReadOnlyDictionary<cadena, cadena[]><br /><br /> IpAddress: cadena<br /><br /> MatchedParameters: IReadOnlyDictionary<cadena, cadena><br /><br /> Method: cadena<br /><br /> OriginalUrl: [IUrl](#ref-iurl)<br /><br /> Url: [IUrl](#ref-iurl)|
|<a id="ref-context-request-headers"></a>string context.Request.Headers.GetValueOrDefault(headerName: string, defaultValue: string)|headerName: cadena<br /><br /> defaultValue: cadena<br /><br /> Devuelve valores de encabezado de solicitud separados por comas o `defaultValue`, si no se encuentra el encabezado.|
|<a id="ref-context-response"></a>context.Response|Cuerpo: [IMessageBody](#ref-imessagebody)<br /><br /> [Encabezados](#ref-context-response-headers): IReadOnlyDictionary<cadena, cadena[]><br /><br /> StatusCode: int<br /><br /> StatusReason: cadena|
|<a id="ref-context-response-headers"></a>string context.Response.Headers.GetValueOrDefault(headerName: string, defaultValue: string)|headerName: cadena<br /><br /> defaultValue: cadena<br /><br /> Devuelve valores de encabezado de respuesta separados por comas o `defaultValue`, si no se encuentra el encabezado.|
|<a id="ref-context-subscription"></a>context.Subscription|CreatedTime: DateTime<br /><br /> EndDate: DateTime?<br /><br /> Id: cadena<br /><br /> Key: cadena<br /><br /> Name: cadena<br /><br /> PrimaryKey: cadena<br /><br /> SecondaryKey: cadena<br /><br /> StartDate: DateTime?|
|<a id="ref-context-user"></a>context.User|Email: cadena<br /><br /> FirstName: cadena<br /><br /> Groups: IEnumerable<[IGroup](#ref-igroup)\><br /><br /> Id: cadena<br /><br /> Identities: IEnumerable<[IUserIdentity](#ref-iuseridentity)\><br /><br /> LastName: cadena<br /><br /> Note: cadena<br /><br /> RegistrationDate: DateTime|
|<a id="ref-iapi"></a>IApi|Id: cadena<br /><br /> Name: cadena<br /><br /> Path: cadena<br /><br /> Protocols: IEnumerable<cadena\><br /><br /> ServiceUrl: [IUrl](#ref-iurl)<br /><br /> SubscriptionKeyParameterNames: [ISubscriptionKeyParameterNames](#ref-isubscriptionkeyparameternames)|
|<a id="ref-igroup"></a>IGroup|Id: cadena<br /><br /> Name: cadena|
|<a id="ref-imessagebody"></a>IMessageBody|As<T\>(preserveContent: bool = false): Donde T: string, byte[],JObject, JToken, JArray, XNode, XElement, XDocument<br /><br /> Los métodos `context.Request.Body.As<T>` y `context.Response.Body.As<T>` se usan para leer los cuerpos de un mensaje de respuesta y solicitud en un tipo `T` especificado. De forma predeterminada, el método usa la secuencia de cuerpo del mensaje original y lo presenta como no disponible tras la devolución. Para evitar este resultado haciendo que el método procese una copia de la secuencia del cuerpo, establezca el parámetro `preserveContent` en `true`. Vaya [aquí](api-management-transformation-policies.md#SetBody) para ver un ejemplo.|
|<a id="ref-iurl"></a>IUrl|Host: cadena<br /><br /> Path: cadena<br /><br /> Port: int<br /><br /> [Consultar](#ref-iurl-query) IReadOnlyDictionary<cadena, cadena[]><br /><br /> QueryString: cadena<br /><br /> Scheme: cadena|
|<a id="ref-iuseridentity"></a>IUserIdentity|Id: cadena<br /><br /> Provider: cadena|
|<a id="ref-isubscriptionkeyparameternames"></a>ISubscriptionKeyParameterNames|Header: cadena<br /><br /> Query: cadena|
|<a id="ref-iurl-query"></a>string IUrl.Query.GetValueOrDefault(queryParameterName: string, defaultValue: string)|queryParameterName: cadena<br /><br /> defaultValue: cadena<br /><br /> Devuelve valores de parámetro de consulta separados por comas o `defaultValue`, si no se encuentra el parámetro.|
|<a id="ref-context-variables"></a>T context.Variables.GetValueOrDefault<T\>(variableName: string, defaultValue: T)|variableName: cadena<br /><br /> defaultValue: T<br /><br /> Devuelve el valor de la variable convertido al tipo `T` o `defaultValue` si no se encuentra la variable.<br /><br /> Este método produce una excepción si el tipo especificado no coincide con el tipo real de la variable devuelta.|
|BasicAuthCredentials AsBasic(input: esta cadena)|input: cadena<br /><br /> Si el parámetro de entrada contiene un valor válido de encabezado de solicitud de autorización de autenticación básica HTTP, el método devuelve un objeto de tipo `BasicAuthCredentials`; en caso contrario, el método devuelve nulo.|
|bool TryParseBasic(input: esta cadena, result: BasicAuthCredentials de salida)|input: cadena<br /><br /> resultado: out BasicAuthCredentials<br /><br /> Si el parámetro de entrada contiene un valor válido de autorización de autenticación básica HTTP en el encabezado de solicitud, el método devuelve `true` y el parámetro de resultado contiene un valor del tipo `BasicAuthCredentials`; en caso contrario, el método devuelve `false`.|
|BasicAuthCredentials|Password: cadena<br /><br /> UserId: cadena|
|Jwt AsJwt(input: esta cadena)|input: cadena<br /><br /> Si el parámetro de entrada contiene un valor de token JWT válido, el método devuelve un objeto de tipo `Jwt`; en caso contrario, el método devuelve `null`.|
|bool TryParseJwt(input: esta cadena, result: Jwt de salida)|input: cadena<br /><br /> resultado: JWT de salida<br /><br /> Si el parámetro de entrada contiene un valor de token JWT válido, el método devuelve `true` y el parámetro de resultado contiene un valor de tipo `Jwt`; en caso contrario, el método devuelve `false`.|
|Jwt|Algorithm: cadena<br /><br /> Audience: IEnumerable<cadena\><br /><br /> Claims: IReadOnlyDictionary<cadena, cadena[]><br /><br /> ExpirationTime: DateTime?<br /><br /> Id: cadena<br /><br /> Issuer: cadena<br /><br /> IssuedAt: DateTime?<br /><br /> NotBefore: DateTime?<br /><br /> Subject: cadena<br /><br /> Type: cadena|
|cadena Jwt.Claims.GetValueOrDefault(claimName: cadena, defaultValue: cadena)|claimName: cadena<br /><br /> defaultValue: cadena<br /><br /> Devuelve valores de notificación separados por comas o `defaultValue`, si no se encuentra el encabezado.|
|byte[] Encrypt(input: this byte[], alg: string, key:byte[], iv:byte[])|input: texto no cifrado que se va a cifrar<br /><br />alg: nombre de un algoritmo de cifrado simétrico<br /><br />key: clave de cifrado<br /><br />iv: vector de inicialización<br /><br />Devuelve cifrado el texto no cifrado.|
|byte[] Encrypt(entrada: este byte[], alg: System.Security.Cryptography.SymmetricAlgorithm)|input: texto no cifrado que se va a cifrar<br /><br />alg: algoritmo de cifrado<br /><br />Devuelve cifrado el texto no cifrado.|
|byte[] Encrypt(entrada: este byte[], alg: System.Security.Cryptography.SymmetricAlgorithm, key:byte[], iv:byte[])|input: texto no cifrado que se va a cifrar<br /><br />alg: algoritmo de cifrado<br /><br />key: clave de cifrado<br /><br />iv: vector de inicialización<br /><br />Devuelve cifrado el texto no cifrado.|
|byte[] Decrypt(input: this byte[], alg: string, key:byte[], iv:byte[])|input: texto cifrado que se va a descifrar<br /><br />alg: nombre de un algoritmo de cifrado simétrico<br /><br />key: clave de cifrado<br /><br />iv: vector de inicialización<br /><br />Devuelve texto no cifrado.|
|byte[] Decrypt(entrada: este byte[], alg: System.Security.Cryptography.SymmetricAlgorithm)|input: texto cifrado que se va a descifrar<br /><br />alg: algoritmo de cifrado<br /><br />Devuelve texto no cifrado.|
|byte[] Decrypt(entrada: este byte[], alg: System.Security.Cryptography.SymmetricAlgorithm, key:byte[], iv:byte[])|input: texto cifrado que se va a descifrar<br /><br />alg: algoritmo de cifrado<br /><br />key: clave de cifrado<br /><br />iv: vector de inicialización<br /><br />Devuelve texto no cifrado.|
|bool VerifyNoRevocation(input: this System.Security.Cryptography.X509Certificates.X509Certificate2)|Realiza una validación de la cadena X.509 sin comprobar el estado de revocación de los certificados.<br /><br />input - certificate object<br /><br />Devuelve `true` si la validación es correcta; `false` si hay un error.|


## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo trabajar con directivas, consulte:

+ [Directivas de Azure API Management](api-management-howto-policies.md)
+ [API de transformación](transform-api.md)
+ En la [Referencia de directivas](api-management-policy-reference.md) se muestra una lista completa de declaraciones de directivas y su configuración
+ [Ejemplos de directivas](policy-samples.md)
