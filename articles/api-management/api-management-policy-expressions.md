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
ms.devlang: na
ms.topic: article
ms.date: 03/22/2019
ms.author: apimpm
ms.openlocfilehash: 90b2dfdbec0d6dc81a05b845832fda92fe36d98c
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58403098"
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

|Tipo|Miembros compatibles|
|--------------|-----------------------|
|Newtonsoft.Json.Formatting|Todas|
|Newtonsoft.Json.JsonConvert|SerializeObject, DeserializeObject|
|Newtonsoft.Json.Linq.Extensions|Todas|
|Newtonsoft.Json.Linq.JArray|Todas|
|Newtonsoft.Json.Linq.JConstructor|Todas|
|Newtonsoft.Json.Linq.JContainer|Todas|
|Newtonsoft.Json.Linq.JObject|Todas|
|Newtonsoft.Json.Linq.JProperty|Todas|
|Newtonsoft.Json.Linq.JRaw|Todas|
|Newtonsoft.Json.Linq.JToken|Todas|
|Newtonsoft.Json.Linq.JTokenType|Todas|
|Newtonsoft.Json.Linq.JValue|Todas|
|System.Array|Todas|
|System.BitConverter|Todas|
|System.Boolean|Todas|
|System.Byte|Todas|
|System.Char|Todas|
|System.Collections.Generic.Dictionary<TKey, TValue>|Todas|
|System.Collections.Generic.HashSet<T>|Todas|
|System.Collections.Generic.ICollection<T>|Todas|
|System.Collections.Generic.IDictionary<TKey, TValue>|Todas|
|System.Collections.Generic.IEnumerable<T>|Todas|
|System.Collections.Generic.IEnumerator<T>|Todas|
|System.Collections.Generic.IList<T>|Todas|
|System.Collections.Generic.IReadOnlyCollection<T>|Todas|
|System.Collections.Generic.IReadOnlyDictionary<TKey, TValue>|Todas|
|System.Collections.Generic.ISet<T>|Todas|
|System.Collections.Generic.KeyValuePair<TKey, TValue>|Todas|
|System.Collections.Generic.List<T>|Todas|
|System.Collections.Generic.Queue<T>|Todas|
|System.Collections.Generic.Stack<T>|Todas|
|System.Convert|Todas|
|System.DateTime|(Constructor), agregar, AddDays, AddHours, AddMilliseconds, AddMinutes, AddMonths, AddSeconds, AddTicks, AddYears, fecha, día, DayOfWeek, DayOfYear, DaysInMonth, hora, IsDaylightSavingTime, IsLeapYear, MaxValue, milisegundo, minuto, MinValue, mes, ahora , Análisis, en segundo lugar, restar, ciclos, TimeOfDay, hoy en día, ToString, UtcNow, año|
|System.DateTimeKind|UTC|
|System.DateTimeOffset|Todas|
|System.Decimal|Todas|
|System.Double|Todas|
|System.Exception|Todas|
|System.Guid|Todas|
|System.Int16|Todas|
|System.Int32|Todas|
|System.Int64|Todas|
|System.IO.StringReader|Todas|
|System.IO.StringWriter|Todas|
|System.Linq.Enumerable|Todas|
|System.Math|Todas|
|System.MidpointRounding|Todas|
|System.Net.WebUtility|Todas|
|System.Nullable|Todas|
|System.Random|Todas|
|System.SByte|Todas|
|System.Security.Cryptography.AsymmetricAlgorithm|Todas|
|System.Security.Cryptography.CipherMode|Todas|
|System.Security.Cryptography.HashAlgorithm|Todas|
|System.Security.Cryptography.HashAlgorithmName|Todas|
|System.Security.Cryptography.HMAC|Todas|
|System.Security.Cryptography.HMACMD5|Todas|
|System.Security.Cryptography.HMACSHA1|Todas|
|System.Security.Cryptography.HMACSHA256|Todas|
|System.Security.Cryptography.HMACSHA384|Todas|
|System.Security.Cryptography.HMACSHA512|Todas|
|System.Security.Cryptography.KeyedHashAlgorithm|Todas|
|System.Security.Cryptography.MD5|Todas|
|System.Security.Cryptography.Oid|Todas|
|System.Security.Cryptography.PaddingMode|Todas|
|System.Security.Cryptography.RNGCryptoServiceProvider|Todas|
|System.Security.Cryptography.RSA|Todas|
|System.Security.Cryptography.RSAEncryptionPadding|Todas|
|System.Security.Cryptography.RSASignaturePadding|Todas|
|System.Security.Cryptography.SHA1|Todas|
|System.Security.Cryptography.SHA1Managed|Todas|
|System.Security.Cryptography.SHA256|Todas|
|System.Security.Cryptography.SHA256Managed|Todas|
|System.Security.Cryptography.SHA384|Todas|
|System.Security.Cryptography.SHA384Managed|Todas|
|System.Security.Cryptography.SHA512|Todas|
|System.Security.Cryptography.SHA512Managed|Todas|
|System.Security.Cryptography.SymmetricAlgorithm|Todas|
|System.Security.Cryptography.X509Certificates.PublicKey|Todas|
|System.Security.Cryptography.X509Certificates.RSACertificateExtensions|Todas|
|System.Security.Cryptography.X509Certificates.X500DistinguishedName|name|
|System.Security.Cryptography.X509Certificates.X509Certificate|Todas|
|System.Security.Cryptography.X509Certificates.X509Certificate2|Todas|
|System.Security.Cryptography.X509Certificates.X509ContentType|Todas|
|System.Security.Cryptography.X509Certificates.X509NameType|Todas|
|System.Single|Todas|
|System.String|Todas|
|System.StringComparer|Todas|
|System.StringComparison|Todas|
|System.StringSplitOptions|Todas|
|System.Text.Encoding|Todas|
|System.Text.RegularExpressions.Capture|Index, Length, Value.|
|System.Text.RegularExpressions.CaptureCollection|Count, Item.|
|System.Text.RegularExpressions.Group|Captures, Success.|
|System.Text.RegularExpressions.GroupCollection|Count, Item.|
|System.Text.RegularExpressions.Match|Empty, Groups, Result.|
|System.Text.RegularExpressions.Regex|(Constructor), IsMatch, Match, Matches, reemplazar, Unescape, dividir|
|System.Text.RegularExpressions.RegexOptions|Todas|
|System.Text.StringBuilder|Todas|
|System.TimeSpan|Todas|
|System.TimeZone|Todas|
|System.TimeZoneInfo.AdjustmentRule|Todas|
|System.TimeZoneInfo.TransitionTime|Todas|
|System.TimeZoneInfo|Todas|
|System.Tuple|Todas|
|System.UInt16|Todas|
|System.UInt32|Todas|
|System.UInt64|Todas|
|System.Uri|Todas|
|System.UriPartial|Todas|
|System.Xml.Linq.Extensions|Todas|
|System.Xml.Linq.XAttribute|Todas|
|System.Xml.Linq.XCData|Todas|
|System.Xml.Linq.XComment|Todas|
|System.Xml.Linq.XContainer|Todas|
|System.Xml.Linq.XDeclaration|Todas|
|System.Xml.Linq.XDocument|Todos, excepto de: Cargar|
|System.Xml.Linq.XDocumentType|Todas|
|System.Xml.Linq.XElement|Todas|
|System.Xml.Linq.XName|Todas|
|System.Xml.Linq.XNamespace|Todas|
|System.Xml.Linq.XNode|Todas|
|System.Xml.Linq.XNodeDocumentOrderComparer|Todas|
|System.Xml.Linq.XNodeEqualityComparer|Todas|
|System.Xml.Linq.XObject|Todas|
|System.Xml.Linq.XProcessingInstruction|Todas|
|System.Xml.Linq.XText|Todas|
|System.Xml.XmlNodeType|Todas|

## <a name="ContextVariables"></a> Variable de contexto
Una variable denominada `context` está disponible implícitamente en todas las [expresiones](api-management-policy-expressions.md#Syntax) de directiva. Sus miembros proporcionan información relativa a `\request`. Todos los miembros de `context` son de solo lectura.

|Variable de contexto|Métodos, propiedades y valores de parámetro admitidos|
|----------------------|-------------------------------------------------------|
|contexto|Api: IApi<br /><br /> Implementación<br /><br /> Transcurrido: TimeSpan: intervalo de tiempo entre el valor de marca de tiempo y la hora actual<br /><br /> LastError<br /><br /> Operación<br /><br /> Producto<br /><br /> Solicitar<br /><br /> RequestId: Guid: identificador único de la solicitud<br /><br /> Respuesta<br /><br /> Subscription<br /><br /> Marca de tiempo: DateTime: punto en el tiempo en que se recibió la solicitud<br /><br /> Tracing: bool - indica si el seguimiento está activado o desactivado <br /><br /> Usuario<br /><br /> Variables: IReadOnlyDictionary<cadena, objeto><br /><br /> void Trace(message: cadena)|
|context.Api|Id: cadena<br /><br /> IsCurrentRevision: bool<br /><br />  Name: cadena<br /><br /> Path: cadena<br /><br /> Revision: cadena<br /><br /> ServiceUrl: IUrl<br /><br /> Version: cadena |
|context.Deployment|Region: cadena<br /><br /> ServiceName: cadena<br /><br /> Certificates: IReadOnlyDictionary<cadena, X509Certificate2>|
|context.LastError|Source: cadena<br /><br /> Reason: cadena<br /><br /> Message: cadena<br /><br /> Scope: cadena<br /><br /> Section: cadena<br /><br /> Path: cadena<br /><br /> PolicyId: cadena<br /><br /> Para obtener más información sobre context.LastError, consulte [Error handling in API Management policies](api-management-error-handling-policies.md) (Control de errores en directivas de API Management).|
|context.Operation|Id: cadena<br /><br /> Method: cadena<br /><br /> Name: cadena<br /><br /> UrlTemplate: cadena|
|context.Product|Apis: IEnumerable<IApi\><br /><br /> ApprovalRequired: bool<br /><br /> Groups: IEnumerable<IGroup\><br /><br /> Id: cadena<br /><br /> Name: cadena<br /><br /> State: enum ProductState {NotPublished, Published}<br /><br /> SubscriptionLimit: int?<br /><br /> SubscriptionRequired: bool|
|context.Request|Cuerpo: IMessageBody<br /><br /> Certificate: System.Security.Cryptography.X509Certificates.X509Certificate2<br /><br /> Encabezados: IReadOnlyDictionary<cadena, cadena[]><br /><br /> IpAddress: cadena<br /><br /> MatchedParameters: IReadOnlyDictionary<cadena, cadena><br /><br /> Method: cadena<br /><br /> OriginalUrl:IUrl<br /><br /> Url: IUrl|
|string context.Request.Headers.GetValueOrDefault(headerName: cadena, defaultValue: cadena)|headerName: cadena<br /><br /> defaultValue: cadena<br /><br /> Devuelve valores de encabezado de solicitud separados por comas o `defaultValue`, si no se encuentra el encabezado.|
|context.Response|Cuerpo: IMessageBody<br /><br /> Encabezados: IReadOnlyDictionary<cadena, cadena[]><br /><br /> StatusCode: int<br /><br /> StatusReason: cadena|
|string context.Response.Headers.GetValueOrDefault(headerName: cadena, defaultValue: cadena)|headerName: cadena<br /><br /> defaultValue: cadena<br /><br /> Devuelve valores de encabezado de respuesta separados por comas o `defaultValue`, si no se encuentra el encabezado.|
|context.Subscription|CreatedTime: DateTime<br /><br /> EndDate: DateTime?<br /><br /> Id: cadena<br /><br /> Key: cadena<br /><br /> Name: cadena<br /><br /> PrimaryKey: cadena<br /><br /> SecondaryKey: cadena<br /><br /> StartDate: DateTime?|
|context.User|Email: cadena<br /><br /> FirstName: cadena<br /><br /> Groups: IEnumerable<IGroup\><br /><br /> Id: cadena<br /><br /> Identities: IEnumerable<IUserIdentity\><br /><br /> LastName: cadena<br /><br /> Note: cadena<br /><br /> RegistrationDate: DateTime|
|IApi|Id: cadena<br /><br /> Name: cadena<br /><br /> Path: cadena<br /><br /> Protocols: IEnumerable<cadena\><br /><br /> ServiceUrl: IUrl<br /><br /> SubscriptionKeyParameterNames: ISubscriptionKeyParameterNames|
|IGroup|Id: cadena<br /><br /> Name: cadena|
|IMessageBody|As<T\>(preserveContent: bool = false): Donde T: cadena, JObject, JToken, JArray, XNode, XElement, XDocument<br /><br /> Los métodos `context.Request.Body.As<T>` y `context.Response.Body.As<T>` se usan para leer los cuerpos de un mensaje de respuesta y solicitud en un tipo `T` especificado. De forma predeterminada, el método usa la secuencia de cuerpo del mensaje original y lo presenta como no disponible tras la devolución. Para evitar este resultado haciendo que el método procese una copia de la secuencia del cuerpo, establezca el parámetro `preserveContent` en `true`. Vaya [aquí](api-management-transformation-policies.md#SetBody) para ver un ejemplo.|
|IUrl|Host: cadena<br /><br /> Path: cadena<br /><br /> Port: int<br /><br /> Consulta: IReadOnlyDictionary<cadena, cadena[]><br /><br /> QueryString: cadena<br /><br /> Scheme: cadena|
|IUserIdentity|Id: cadena<br /><br /> Provider: cadena|
|ISubscriptionKeyParameterNames|Header: cadena<br /><br /> Query: cadena|
|string IUrl.Query.GetValueOrDefault(queryParameterName: cadena, defaultValue: cadena)|queryParameterName: cadena<br /><br /> defaultValue: cadena<br /><br /> Devuelve valores de parámetro de consulta separados por comas o `defaultValue`, si no se encuentra el parámetro.|
|T context.Variables.GetValueOrDefault<T\>(variableName: cadena, defaultValue: T)|variableName: cadena<br /><br /> defaultValue: M<br /><br /> Devuelve el valor de la variable convertido al tipo `T` o `defaultValue` si no se encuentra la variable.<br /><br /> Este método produce una excepción si el tipo especificado no coincide con el tipo real de la variable devuelta.|
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
|byte[] Decrypt(entrada: este byte[], alg: System.Security.Cryptography.SymmetricAlgorithm, key:byte[], iv:byte[])|input - input: texto cifrado que se va a descifrar<br /><br />alg: algoritmo de cifrado<br /><br />key: clave de cifrado<br /><br />iv: vector de inicialización<br /><br />Devuelve texto no cifrado.|


## <a name="next-steps"></a>Pasos siguientes

Para obtener más información sobre cómo trabajar con directivas, consulte:

+ [Directivas de Azure API Management](api-management-howto-policies.md)
+ [API de transformación](transform-api.md)
+ En la [Referencia de directivas](api-management-policy-reference.md) se muestra una lista completa de declaraciones de directivas y su configuración
+ [Ejemplos de directivas](policy-samples.md)
