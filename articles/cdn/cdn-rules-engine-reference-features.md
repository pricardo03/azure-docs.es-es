---
title: Características del motor de reglas de Azure CDN | Microsoft Docs
description: Documentación de referencia sobre las características del motor de reglas de Azure CDN.
services: cdn
documentationcenter: ''
author: mdgattuso
manager: danielgi
editor: ''
ms.assetid: 669ef140-a6dd-4b62-9b9d-3f375a14215e
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2018
ms.author: magattus
ms.openlocfilehash: 205a8dae55394a82a60f54ed32bad95324a59517
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60324273"
---
# <a name="azure-cdn-rules-engine-features"></a>Características del motor de reglas de Azure CDN
En este tema se muestran descripciones detalladas de las características disponibles para el [motor de reglas](cdn-rules-engine.md)de Azure Content Delivery Network (CDN).

La tercera parte de una regla es la característica. Una característica define el tipo de acción que se aplica al tipo de solicitud que se identifica con un conjunto de condiciones de coincidencia.

## <a name="access-features"></a>Características de acceso

Estas características están diseñadas para controlar el acceso al contenido.

Name | Propósito
-----|--------
[Denegar acceso (403)](#deny-access-403) | Determina si todas las solicitudes se rechazan con una respuesta 403-Prohibido.
[Autenticación de token](#token-auth) | Determina si se aplica una solicitud de autenticación basada en tokens a una solicitud.
[Código de denegación de autenticación de tokens](#token-auth-denial-code) | Determina el tipo de respuesta que se devolverá al usuario cuando se deniega una solicitud debido a la autenticación basada en tokens.
[Ignorar mayúsculas y minúsculas en URL de autenticación de tokens](#token-auth-ignore-url-case) | Determina si las comparaciones de URL realizadas por la autenticación basada en tokens distinguen mayúsculas de minúsculas.
[Parámetro de autenticación de tokens](#token-auth-parameter) | Determina si debe cambiarse el parámetro de cadena de consulta de autenticación basada en tokens.


## <a name="caching-features"></a>Características de almacenamiento en caché

Estas características están diseñadas para personalizar cuándo y cómo se almacena el contenido en caché.

Name | Propósito
-----|--------
[Parámetros de ancho de banda](#bandwidth-parameters) | Determina si los parámetros de limitación de ancho de banda (por ejemplo, ec_rate y ec_prebuf) están activos.
[Limitación de ancho de banda](#bandwidth-throttling) | Limita el ancho de banda de la respuesta proporcionada por el punto de presencia (POP).
[Omisión de la memoria caché](#bypass-cache) | Determina si la solicitud debe omitir el almacenamiento en caché.
[Tratamiento de encabezados Cache-Control](#cache-control-header-treatment) | Controla la generación de encabezados `Cache-Control` mediante el punto de presencia cuando está activa la característica externa de antigüedad máxima.
[Cadena de consulta de clave de caché](#cache-key-query-string) | Determina si la clave de caché incluye o excluye los parámetros de cadena de consulta asociados a una solicitud.
[Reescritura de clave de caché](#cache-key-rewrite) | Reescribe la clave de caché asociada a una solicitud.
[Relleno de la memoria caché completa](#complete-cache-fill) | Determina lo que ocurre cuando una solicitud tiene como resultado un error de caché parcial en un punto de presencia.
[Comprimir tipos de archivo](#compress-file-types) | Define los formatos de los archivos que se van a comprimir en el servidor.
[Max-Age interna predeterminada](#default-internal-max-age) | Determina el intervalo predeterminado de antigüedad máxima del punto de presencia en la revalidación de caché del servidor de origen.
[Tratamiento del encabezado Expires](#expires-header-treatment) | Controla la generación de encabezados `Expires` mediante un punto de presencia cuando está activa la característica externa de antigüedad máxima.
[Max-Age externa](#external-max-age) | Determina el intervalo de antigüedad máxima para el explorador en la revalidación de caché del punto de presencia.
[Forzar Max-Age interna](#force-internal-max-age) | Determina el intervalo de antigüedad máxima del punto de presencia en la revalidación de caché del servidor de origen.
[Compatibilidad de H.264 (descarga progresiva de HTTP)](#h264-support-http-progressive-download) | Determina los tipos de formatos de archivo H.264 que pueden usarse para transmitir contenido en streaming.
[Respetar la solicitud de no almacenar en caché](#honor-no-cache-request) | Determina si las solicitudes de no almacenar en caché de un cliente HTTP se reenvían al servidor de origen.
[Ignorar la opción de no almacenar en caché de origen](#ignore-origin-no-cache) | Determina si la CDN ignora determinadas directivas procedentes de un servidor de origen.
[Ignorar intervalos que no se puedan satisfacer](#ignore-unsatisfiable-ranges) | Determina la respuesta que se devolverá a los clientes cuando una solicitud genere un código de estado "416 - No se puede satisfacer el intervalo solicitado".
[Max-Stale interna](#internal-max-stale) | Controla cuánto tiempo después de la hora de expiración normal puede atenderse un recurso almacenado en caché desde un punto de presencia cuando este no puede volver a validar el recurso almacenado en caché con el servidor de origen.
[Uso compartido de caché parcial](#partial-cache-sharing) | Determina si una solicitud puede generar contenido almacenado parcialmente en caché.
[Prevalidar el contenido guardado en caché](#prevalidate-cached-content) | Determina si el contenido almacenado en caché es apto para la revalidación temprana antes de que expire su período de vida.
[Actualizar archivos de caché de cero bytes](#refresh-zero-byte-cache-files) | Determina cómo controlan los puntos de presencia la solicitud de un cliente HTTP para un recurso de la caché de 0 bytes.
[Establecer códigos de estado almacenables en caché](#set-cacheable-status-codes) | Define el conjunto de códigos de estado que puede dar lugar a contenido almacenado en caché.
[Entrega de contenido obsoleto en caso de error](#stale-content-delivery-on-error) | Determina si se entrega el contenido almacenado en caché cuando se produce un error durante la revalidación de caché o al recuperar el contenido solicitado desde el servidor de origen del cliente.
[Obsoleto durante revalidación](#stale-while-revalidate) | Mejora el rendimiento al permitir que los puntos de presencia sirvan un cliente obsoleto para el solicitante mientras se lleva a cabo la revalidación.

## <a name="comment-feature"></a>Característica de comentario

Esta característica está diseñada para proporcionar información adicional dentro de una regla.

Name | Propósito
-----|--------
[Comment](#comment) | Permite agregar una nota en una regla.
 
## <a name="header-features"></a>Características de encabezado

Estas características están diseñadas para agregar, modificar o eliminar encabezados de la solicitud o respuesta.

Name | Propósito
-----|--------
[Encabezado de respuesta Age](#age-response-header) | Determina si se incluye un encabezado de respuesta Age en la respuesta enviada al solicitante.
[Depurar encabezados de respuesta de la caché](#debug-cache-response-headers) | Determina si una respuesta puede incluir el encabezado de respuesta X-EC-Debug que proporciona información sobre la directiva de caché del recurso solicitado.
[Modificar encabezado de solicitud de cliente](#modify-client-request-header) | Sobrescribe, agrega o elimina un encabezado en una solicitud.
[Modificar encabezado de respuesta de cliente](#modify-client-response-header) | Sobrescribe, agrega o elimina un encabezado en una respuesta.
[Establecer encabezado personalizado de IP de cliente](#set-client-ip-custom-header) | Permite que la dirección IP del cliente solicitante se agregue a la solicitud como encabezado de solicitud personalizado.


## <a name="logging-features"></a>Características de registro

Estas características están diseñadas para personalizar los datos almacenados en archivos de registro sin procesar.

Name | Propósito
-----|--------
[Campo de registro personalizado 1](#custom-log-field-1) | Determina el formato y el contenido que se asigna al campo de registro personalizado en un archivo de registro sin procesar.
[Cadena de consulta del registro](#log-query-string) | Determina si una cadena de consulta se almacena con la dirección URL en los registros de acceso.


<!---
## Optimize

These features determine whether a request will undergo the optimizations provided by Edge Optimizer.

Name | Purpose
-----|--------
Edge Optimizer | Determines whether Edge Optimizer can be applied to a request.
Edge Optimizer – Instantiate Configuration | Instantiates or activates the Edge Optimizer configuration associated with a site.

### Edge Optimizer
**Purpose:** Determines whether Edge Optimizer can be applied to a request.

If this feature has been enabled, then the following criteria must also be met before the request will be processed by Edge Optimizer:

- The requested content must use an edge CNAME URL.
- The edge CNAME referenced in the URL must correspond to a site whose configuration has been activated in a rule.

This feature requires the ADN platform and the Edge Optimizer feature.

Value|Result
-|-
Enabled|Indicates that the request is eligible for Edge Optimizer processing.
Disabled|Restores the default behavior. The default behavior is to deliver content over the ADN platform without any additional processing.

**Default Behavior:** Disabled
 

### Edge Optimizer - Instantiate Configuration
**Purpose:** Instantiates or activates the Edge Optimizer configuration associated with a site.

This feature requires the ADN platform and the Edge Optimizer feature.

Key information:

- Instantiation of a site configuration is required before requests to the corresponding edge CNAME can be processed by Edge Optimizer.
- This instantiation only needs to be performed a single time per site configuration. A site configuration that has been instantiated will remain in that state until the Edge Optimizer – Instantiate Configuration feature that references it is removed from the rule.
- The instantiation of a site configuration does not mean that all requests to the corresponding edge CNAME will automatically be processed by Edge Optimizer. The Edge Optimizer feature determines whether an individual request will be processed.

If the desired site does not appear in the list, then you should edit its configuration and verify that the Active option has been marked.

**Default Behavior:** Site configurations are inactive by default.
--->

## <a name="origin-features"></a>Características de origen

Estas características están diseñadas para controlar la forma en que la red CDN se comunica con un servidor de origen.

Name | Propósito
-----|--------
[Número máximo de solicitudes de conexión persistente](#maximum-keep-alive-requests) | Define el número máximo de solicitudes de conexión persistente antes de cerrarse.
[Encabezados de proxy especiales](#proxy-special-headers) | Define el conjunto de encabezados de solicitud específicos de la red CDN que se reenvían desde un punto de presencia a un servidor de origen.


## <a name="specialty-features"></a>Características de especialidad

Estas características ofrecen funcionalidades avanzadas que solo deben utilizar los usuarios avanzados.

Name | Propósito
-----|--------
[Métodos HTTP almacenables en caché](#cacheable-http-methods) | Determina el conjunto de métodos HTTP adicionales que pueden almacenarse en caché en la red.
[Tamaño del cuerpo de solicitud almacenable en caché](#cacheable-request-body-size) | Define el umbral que determina si una respuesta POST se puede almacenar en caché.
[Variable USER](#user-variable) | Solo para uso interno.

 
## <a name="url-features"></a>Características URL

Estas características permiten reescribir una solicitud o redirigirla a una dirección URL diferente.

Name | Propósito
-----|--------
[Seguir redireccionamientos](#follow-redirects) | Determina si las solicitudes se pueden redirigir al nombre de host definido en el encabezado Ubicación que devuelve el servidor de origen de un cliente.
[Redirección de direcciones URL](#url-redirect) | Redirige las solicitudes a través del encabezado Ubicación.
[Reescritura de direcciones URL](#url-rewrite)  | Reescribe la dirección URL de la solicitud.



## <a name="azure-cdn-rules-engine-features-reference"></a>Referencia de las características del motor de reglas de Azure CDN

---
### <a name="age-response-header"></a>Encabezado de respuesta Age
**Propósito**: Determina si se incluye un encabezado de respuesta Age en la respuesta enviada al solicitante.

Value|Resultado
--|--
Enabled | El encabezado de respuesta Age se incluye en la respuesta enviada al solicitante.
Disabled | El encabezado de respuesta Age se excluye de la respuesta enviada al solicitante.

**El comportamiento predeterminado de**: Deshabilitado.

[Volver arriba](#azure-cdn-rules-engine-features)

<br>

---
### <a name="bandwidth-parameters"></a>Parámetros de ancho de banda
**Propósito:** determina si los parámetros de limitación de ancho de banda (por ejemplo, ec_rate y ec_prebuf) están activos.

Los parámetros de limitación de ancho de banda determinan si la velocidad de transferencia de datos para la solicitud de un cliente se limita a una velocidad personalizada.

Value|Resultado
--|--
Enabled|Permite que los puntos de presencia admitan las solicitudes de limitación de ancho de banda.
Disabled|Hace que los puntos de presencia omitan los parámetros de limitación de ancho de banda. El contenido solicitado se sirve normalmente (es decir, sin limitación de ancho de banda).

**Comportamiento predeterminado:** habilitado.
 
[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="bandwidth-throttling"></a>Limitación de ancho de banda
**Propósito:** limita el ancho de banda de la respuesta de los puntos de presencia.

Dos de las siguientes opciones deben definirse para configurar correctamente la limitación de ancho de banda.

Opción|DESCRIPCIÓN
--|--
Kbytes por segundo|Establezca esta opción en el ancho de banda máximo (Kb por segundo) que puede utilizarse para entregar la respuesta.
Segundos de búfer previo|Establezca esta opción en el número de segundos que los puntos de presencia deben esperar hasta limitar el ancho de banda. El propósito de este período de tiempo de ancho de banda no restringido es evitar que un reproductor multimedia experimente problemas de cortes o de almacenamiento en búfer debido a la limitación del ancho de banda.

**Comportamiento predeterminado:** deshabilitado.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="bypass-cache"></a>Omisión de la memoria caché
**Propósito:** determina si la solicitud debe omitir el almacenamiento en caché.

Value|Resultado
--|--
Enabled|Hace que todas las solicitudes pasen al servidor de origen, aunque el contenido se haya almacenado antes en caché en los puntos de presencia.
Disabled|Hace que los puntos de presencia almacenen en caché los recursos según la directiva de caché definida en sus encabezados de respuesta.

**Comportamiento predeterminado**:

- **HTTP grande:** deshabilitado

<!---
- **ADN:** Enabled

--->

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="cacheable-http-methods"></a>Métodos HTTP almacenables en caché
**Propósito:** determina el conjunto de métodos HTTP adicionales que pueden almacenarse en caché en la red.

Información importante:

- Esta característica da por supuesto que las respuestas GET siempre deben almacenarse en caché. Por lo tanto, el método HTTP GET no se debe incluir al establecer esta característica.
- Esta característica solo admite el método HTTP POST. Para habilitar el almacenamiento en caché de la respuesta POST, establezca esta característica en `POST`.
- De forma predeterminada, solo se almacenan en caché las solicitudes cuyo cuerpo sea inferior a 14 Kb. Use la característica de tamaño de cuerpo de solicitud almacenable en caché para establecer el tamaño del cuerpo de solicitud máximo.

**Comportamiento predeterminado:** se almacenan en caché sólo respuestas GET.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="cacheable-request-body-size"></a>Tamaño del cuerpo de solicitud almacenable en caché
**Propósito:** define el umbral para determinar si se puede almacenar en caché una respuesta POST.

Este umbral se determina especificando un tamaño máximo de cuerpo de solicitud. Las solicitudes que contengan un cuerpo de solicitud más grandes no se almacenan en caché.

Información importante:

- Esta característica solo es aplicable cuando las respuestas POST son aptas para su almacenamiento en caché. Usar la característica de métodos HTTP almacenables en caché para habilitar el almacenamiento en caché de la solicitud POST.
- El cuerpo de la solicitud se tiene en cuenta para:
    - valores x-www-form-urlencoded
    - Garantizar una clave de caché única
- Definir un tamaño de cuerpo de solicitud máximo grande puede afectar al rendimiento de la entrega de datos.
    - **Valor recomendado:** 14 Kb
    - **Valor mínimo:** 1 Kb

**Comportamiento predeterminado:** 14 Kb

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="cache-control-header-treatment"></a>Tratamiento de encabezado Cache-Control
**Propósito:** controla la generación de `Cache-Control` encabezados por el punto de presencia cuando está activa la característica Max-Age externa.

La manera más fácil de lograr este tipo de configuración es colocar las características Max-Age externa y Tratamiento de encabezados Cache-Control en la misma instrucción.

Value|Resultado
--|--
Sobrescribir|Garantiza que se llevarán a cabo las siguientes acciones:<br/> -Sobrescribe el `Cache-Control` encabezado generado por el servidor de origen. <br/>: Agrega el `Cache-Control` encabezado generado por la característica Max-Age externa a la respuesta.
Pass Through|Garantiza que el `Cache-Control` encabezado generado por la característica Max-Age externa nunca se agregará a la respuesta. <br/> Si el servidor de origen genera un `Cache-Control` encabezado, pasa al usuario final. <br/> Si el servidor de origen no genera un `Cache-Control` encabezado, esta opción puede provocar que el encabezado de respuesta no contenga un `Cache-Control` encabezado.
Add if Missing|Si un `Cache-Control` encabezado no se recibió desde el servidor de origen y, después, esta opción agrega el `Cache-Control` encabezado generado por la característica Max-Age externa. Esta opción es útil para garantizar que todos los recursos se asignan una `Cache-Control` encabezado.
Remove| Esta opción garantiza que un `Cache-Control` encabezado no se incluye con la respuesta de encabezado. Si un `Cache-Control` ya se asignó el encabezado y, después, se quita de la respuesta de encabezado.

**Comportamiento predeterminado:** sobrescribir.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="cache-key-query-string"></a>Cadena de consulta de clave de caché
**Propósito:** determina si la clave de caché incluye o excluye los parámetros de cadena de consulta asociados a una solicitud.

Información importante:

- Especifique uno o varios nombres de parámetro de consulta y separe cada nombre de parámetro con un solo espacio.
- Esta característica determina si los parámetros de cadena de consulta se incluyen o excluyen de la clave de caché. En la tabla siguiente se proporciona información adicional para cada opción.

Type|DESCRIPCIÓN
--|--
 Include|  Indica que cada parámetro especificado debe estar incluido en la clave de caché. Se genera una clave de caché única para cada solicitud que contenga un valor único para un parámetro de cadena de consulta definido en esta característica. 
 Include All  |Indica que se crea una clave de caché única para cada solicitud a un recurso que incluya una cadena de consulta única. Normalmente no se recomienda este tipo de configuración porque puede dar lugar a un pequeño porcentaje de aciertos de caché. Un bajo número de aciertos de caché aumenta la carga en el servidor de origen, porque debe atender más solicitudes. Esta configuración reproduce el comportamiento de almacenamiento en caché conocido como "unique-cache" en la página de almacenamiento en caché de cadena de consulta. 
 Exclude | Indica que solo los parámetros especificados se excluyen de la clave de caché. Todos los demás parámetros de cadena de consulta se incluyen en la clave de caché. 
 Exclude All  |Indica que todos los parámetros de cadena de consulta se excluyen de la clave de caché. Esta configuración reproduce el comportamiento en la página de almacenamiento en caché de cadena de consulta predeterminado "standard-cache".  

El motor de reglas permite personalizar la manera en que se implementará el almacenamiento en caché de cadenas de consulta. Por ejemplo, puede especificar que el almacenamiento en caché de cadena consultas solo se realice en determinadas ubicaciones o tipos de archivo.

Para duplicar la cadena de consulta "no-cache" almacenamiento en caché de comportamiento en la página de almacenamiento en caché de cadena de consulta, cree una regla que contenga una condición de coincidencia de caracteres comodín de consulta de dirección URL y una característica de caché de omisión. Establezca la condición de coincidencia de caracteres comodín de consulta de dirección URL en un asterisco (*).

>[!IMPORTANT] 
> Si está habilitada la autorización de token en cualquier ruta de acceso de esta cuenta, el modo de caché estándar es el único modo que se puede usar para el almacenamiento en caché de la cadena de consulta. Para más información, consulte [Control del comportamiento del almacenamiento en caché de Azure CDN con cadenas de consulta](cdn-query-string-premium.md).

#### <a name="sample-scenarios"></a>Escenarios de ejemplo

El siguiente ejemplo de uso de esta característica proporciona una solicitud de ejemplo y la clave de caché predeterminada:

- **Solicitud de ejemplo:** http://wpc.0001.&lt; dominio&gt;/800001/Origin/folder/asset.htm?sessionid=1234 & language = EN & userid = 01
- **Clave de caché predeterminada:** /800001/Origin/folder/asset.htm

##### <a name="include"></a>Include

Configuración de ejemplo:

- **Tipo:** incluyen
- **Parámetros:** idioma

Este tipo de configuración generaría la siguiente clave de caché de parámetro de cadena de consulta:

    /800001/Origin/folder/asset.htm?language=EN

##### <a name="include-all"></a>Include All

Configuración de ejemplo:

- **Tipo:** incluir todos

Este tipo de configuración generaría la siguiente clave de caché de parámetro de cadena de consulta:

    /800001/Origin/folder/asset.htm?sessionid=1234&language=EN&userid=01

##### <a name="exclude"></a>Exclude

Configuración de ejemplo:

- **Tipo:** excluir
- **Parámetros:** id_sesión id_usuario

Este tipo de configuración generaría la siguiente clave de caché de parámetro de cadena de consulta:

    /800001/Origin/folder/asset.htm?language=EN

##### <a name="exclude-all"></a>Exclude All

Configuración de ejemplo:

- **Tipo:** excluir todos

Este tipo de configuración generaría la siguiente clave de caché de parámetro de cadena de consulta:

    /800001/Origin/folder/asset.htm

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="cache-key-rewrite"></a>Reescritura de clave de caché
**Propósito:** vuelve a escribir la clave de caché asociada a una solicitud.

Una clave de caché es la ruta de acceso relativa que identifica un recurso con fines de almacenamiento en caché. En otras palabras, los servidores buscan una versión almacenada en caché de un recurso de acuerdo a la ruta de acceso que definió la clave de caché.

Para configurar esta característica, puede definir las siguientes opciones:

Opción|DESCRIPCIÓN
--|--
Ruta de acceso original| Define la ruta de acceso relativa a los tipos de solicitudes cuyas claves de caché se reescriben. Al seleccionar una ruta de acceso de origen de base y, a continuación, definir un patrón de expresión regular, se puede definir una ruta de acceso relativa.
Nueva ruta de acceso|Define la ruta de acceso relativa de la nueva clave de caché. Para definir una ruta de acceso relativa, seleccione una ruta de acceso de origen de base y, a continuación, define un patrón de expresión regular. Esta ruta de acceso relativa se puede construir dinámicamente mediante el uso de [variables HTTP](cdn-http-variables.md).

**Comportamiento predeterminado:** clave de caché de una solicitud viene determinada por el URI de solicitud.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="comment"></a>Comentario
**Propósito:** permite una nota que se va a agregar dentro de una regla.

Un uso de esta característica es proporcionar información adicional sobre el uso general de una regla o por qué se agregó a la regla una determinada condición de coincidencia o característica.

Información importante:

- Se puede especificar un máximo de 150 caracteres.
- Use únicamente caracteres alfanuméricos.
- Esta característica no afecta al comportamiento de la regla. Su único objetivo es ofrecer un área donde se puede especificar información para futura referencia o que pueda ayudar a solucionar problemas de la regla.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="complete-cache-fill"></a>Relleno de la memoria caché completa
**Propósito:** determina lo que ocurre cuando una solicitud tiene como resultado un error de caché parcial en un servidor POP.

Un error de caché parcial describe el estado de la memoria caché para un recurso que no se ha descargado por completo en un punto de presencia. Si un recurso se ha almacenado en caché solo parcialmente en un punto de presencia, la siguiente solicitud para ese recurso se reenviará de nuevo al servidor de origen.
<!---
This feature is not available for the ADN platform. The typical traffic on this platform consists of relatively small assets. The size of the assets served through these platforms helps mitigate the effects of partial cache misses, since the next request will typically result in the asset being cached on that POP.

--->
Un error de caché parcial normalmente se produce después de que un usuario anule una descarga o en el caso de los recursos que se solicitan únicamente con solicitudes de intervalos HTTP. Esta característica resulta especialmente útil para recursos de gran tamaño que no se suelen descargar de principio a fin (por ejemplo, vídeos). Por lo tanto, esta característica está habilitada de forma predeterminada en la plataforma HTTP Large. Está deshabilitada en todas las demás plataformas.

Deje la configuración predeterminada de la plataforma HTTP Large, ya que reduce la carga en el servidor de origen del cliente y aumenta la velocidad con la que los clientes descargan el contenido.

Value|Resultado
--|--
Enabled|Restablece el comportamiento predeterminado. El comportamiento predeterminado consiste en forzar al punto de presencia a que inicie una captura en segundo plano del recurso desde el servidor de origen. Después de eso, el recurso estará en la caché local del punto de presencia.
Disabled|Evita que un punto de presencia realice una captura en segundo plano del recurso. Como resultado, la próxima solicitud de ese recurso realizada desde esa región hace que un punto de presencia lo solicite desde el servidor de origen del cliente.

**Comportamiento predeterminado:** habilitado.

#### <a name="compatibility"></a>Compatibilidad
Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta característica no se puede asociar con las siguientes condiciones de coincidencia: 
- Número de sistema autónomo (AS)
- Dirección IP de cliente
- Parámetro de cookie
- Regex de parámetro de cookie
- País
- Dispositivo
- Cname perimetral
- Dominio de referencia
- Literal de encabezado de solicitud
- Regex de encabezado de solicitud
- Carácter comodín de encabezado de solicitud
- Método de solicitud
- Esquema de solicitud
- Literal de consulta de dirección URL
- Regex de consulta de dirección URL
- Carácter comodín de consulta de dirección URL
- Parámetro de consulta de dirección URL

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="compress-file-types"></a>Comprimir tipos de archivo
**Propósito:** define los formatos de archivo para los archivos que se comprimen en el servidor.

Los formatos de archivo se pueden especificar con su tipo de medio de Internet (es decir, Content-Type). El tipo de medio de Internet son los metadatos independientes de la plataforma que permiten que los servidores identifiquen el formato de archivo de un recurso determinado. La siguiente es la lista de los tipos de medios más comunes de Internet.

Tipo de medio de Internet|DESCRIPCIÓN
--|--
text/plain|Texto sin formato
text/html| Archivos HTML
text/css|Hojas de estilos en cascada (CSS)
application/x-javascript|Javascript
application/javascript|Javascript

Información importante:

- Especifique varios tipos de medios de Internet delimitando cada uno con un solo espacio. 
- Esta característica solo comprime recursos con un tamaño inferior a 1 MB. Los servidores no comprimen recursos de mayor tamaño.
- Algunos tipos de contenido, como las imágenes, los vídeos y los recursos multimedia de audio (p. ej., JPG, MP3, MP4, etc.), ya están comprimidos. Debido a que una compresión adicional en estos tipos de recursos no reduce significativamente el tamaño del archivo, se recomienda no habilitar la compresión en ellos.
- No se admiten caracteres comodín, como asteriscos.
- Antes de agregar esta característica a una regla, asegúrese de establecer la opción de compresión deshabilitado en la página de compresión para la plataforma a la que se aplica esta regla.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="custom-log-field-1"></a>Campo de registro personalizado 1
**Propósito:** determina el formato y el contenido que se asignará al campo de registro personalizado en un archivo de registro sin procesar.

El propósito principal de este campo personalizado es que le permite determinar qué valores de encabezado de solicitud y respuesta se almacenarán en los archivos de registro.

De forma predeterminada, el campo de registro personalizado se llama "x-ec_custom-1". El nombre de este campo se puede personalizar en la página Configuración de registro sin procesar.

El formato para especificar los encabezados de solicitud y respuesta se define de la siguiente manera:

Tipo de encabezado|Formato|Ejemplos
-|-|-
Encabezado de solicitud|`%{[RequestHeader]()}[i]()` | %{Accept-Encoding}i <br/> {Referer}i <br/> %{Authorization}i
Encabezado de respuesta|`%{[ResponseHeader]()}[o]()`| %{Age}o <br/> %{Content-Type}o <br/> %{Cookie}o

Información importante:

- Un campo de registro personalizado puede contener cualquier combinación de campos de encabezado y texto sin formato.
- Los caracteres válidos en este campo son los siguientes: alfanumérico (0-9, a-z y A-Z), guiones, signo de dos puntos, punto y coma, apóstrofos, comas, puntos, caracteres de subrayado, signos de igual, paréntesis, corchetes y espacios. Solo se permiten los símbolos de porcentaje y las llaves cuando se usa para especificar un campo de encabezado.
- La ortografía para cada campo de encabezado especificado debe coincidir con el nombre de encabezado de solicitud o respuesta deseado.
- Si quiere especificar varios encabezados, use un separador para indicar cada uno de ellos. Por ejemplo, podría utilizar una abreviatura para cada encabezado:
    - AE: %{Accept-Encoding}i A: %{Authorization}i CT: %{Content-Type}o 

**Valor predeterminado:** -

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="debug-cache-response-headers"></a>Depurar encabezados de respuesta de la caché
**Propósito:** determina si una respuesta puede incluir [encabezados de respuesta X-EC-Debug](cdn-http-debug-headers.md), que proporciona información sobre la directiva de caché para el recurso solicitado.

Los encabezados de respuesta de depuración de caché se incluirán en la respuesta cuando se cumplan las dos condiciones siguientes:

- La característica Encabezados de respuesta de caché de depuración se ha habilitado en la solicitud especificada.
- La solicitud especificada define el conjunto de encabezados de respuesta de caché de depuración que se incluirán en la respuesta.

Para solicitar los encabezados de respuesta de caché de depuración, incluya el siguiente encabezado y las directivas especificadas en la solicitud:

`X-EC-Debug: _&lt;Directive1&gt;_,_&lt;Directive2&gt;_,_&lt;DirectiveN&gt;_`

**Ejemplo:**

X-EC-Debug: x-ec-cache,x-ec-check-cacheable,x-ec-cache-key,x-ec-cache-state

Value|Resultado
-|-
Enabled|Las solicitudes de los encabezados de respuesta de caché de depuración devolverá una respuesta que incluye el encabezado X-EC-Debug.
Disabled|El encabezado de respuesta X-EC-Debug se excluirá de la respuesta.

**Comportamiento predeterminado:** deshabilitado.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="default-internal-max-age"></a>Max-Age interna predeterminada
**Propósito:** determina el intervalo de max-age predeterminado de presencia en la revalidación de caché del servidor de origen. En otras palabras, la cantidad de tiempo que transcurrirá antes de que un punto de presencia compruebe si un recurso almacenado en caché coincide con el recurso que se almacena en el servidor de origen.

Información importante:

- Esta acción sólo llevará a cabo para las respuestas desde un servidor de origen que no asignen una indicación de max-age en la `Cache-Control` o `Expires` encabezado.
- Esta acción no se realizará para los recursos que no se consideran almacenables en caché.
- Esta acción no afecta a las revalidaciones de caché del explorador en el punto de presencia. Estos tipos de Revalidaciones vienen determinados por la `Cache-Control` o `Expires` encabezados enviados al explorador, que se puede personalizar con la característica Max-Age externa.
- Los resultados de esta acción no tienen un efecto visible en los encabezados de respuesta y el contenido devuelto por los puntos de presencia, pero puede afectar a la cantidad de tráfico de revalidación enviado desde los puntos de presencia al servidor de origen.
- Para configurar esta característica:
    - Seleccione el código de estado al que se puede aplicar un valor de max-age interna predeterminada.
    - Especifique un valor entero y seleccione la unidad de tiempo que desee (por ejemplo, segundos, minutos, horas, etc.). Este valor define el intervalo de max-age interna predeterminada.

- Establezca la unidad de tiempo en "Off" para asignar un intervalo de max-age interna predeterminada de 7 días para las solicitudes que no se ha asignado una indicación de max-age en su `Cache-Control` o `Expires` encabezado.

**Valor predeterminado:** 7 días

#### <a name="compatibility"></a>Compatibilidad
Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta característica no se puede asociar con las siguientes condiciones de coincidencia: 
- Número de sistema autónomo (AS)
- Dirección IP de cliente
- Parámetro de cookie
- Regex de parámetro de cookie
- País
- Dispositivo
- Cname perimetral
- Dominio de referencia
- Literal de encabezado de solicitud
- Regex de encabezado de solicitud
- Carácter comodín de encabezado de solicitud
- Método de solicitud
- Esquema de solicitud
- Literal de consulta de dirección URL
- Regex de consulta de dirección URL
- Carácter comodín de consulta de dirección URL
- Parámetro de consulta de dirección URL

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="deny-access-403"></a>Denegar acceso (403)
**Propósito**: Determina si todas las solicitudes se rechazan con una respuesta 403-Prohibido.

Value | Resultado
------|-------
Enabled| Hace que todas las solicitudes que cumplen los criterios de coincidencia se rechacen con respuesta 403 Prohibido.
Disabled| Restablece el comportamiento predeterminado. El comportamiento predeterminado es permitir que el servidor de origen determine el tipo de respuesta que se devolverá.

**El comportamiento predeterminado de**: Disabled

> [!TIP]
   > Un uso posible para esta característica es asociarla a una condición de coincidencia del encabezado de solicitud para bloquear el acceso a los remitentes HTTP que usen vínculos en línea al contenido.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="expires-header-treatment"></a>Tratamiento del encabezado Expires
**Propósito:** controla la generación de `Expires` encabezados por un punto de presencia cuando está activa la característica Max-Age externa.

La manera más fácil de lograr este tipo de configuración es colocar las características Max-Age externa y Tratamiento de encabezados Expires en la misma instrucción.

Value|Resultado
--|--
Sobrescribir|Garantiza que se llevarán a cabo las siguientes acciones:<br/>-Sobrescribe el `Expires` encabezado generado por el servidor de origen.<br/>: Agrega el `Expires` encabezado generado por la característica Max-Age externa a la respuesta.
Pass Through|Garantiza que el `Expires` encabezado generado por la característica Max-Age externa nunca se agregará a la respuesta. <br/> Si el servidor de origen genera un `Expires` encabezado, se pasará directamente al usuario final. <br/>Si el servidor de origen no genera un `Expires` encabezado, esta opción puede provocar que el encabezado de respuesta no contenga un `Expires` encabezado.
Add if Missing| Si un `Expires` encabezado no se recibió desde el servidor de origen y, después, esta opción agrega el `Expires` encabezado generado por la característica Max-Age externa. Esta opción es útil para garantizar que todos los recursos se asignarán una `Expires` encabezado.
Remove| Garantiza que un `Expires` encabezado no se incluye con la respuesta de encabezado. Si un `Expires` ya se asignó el encabezado y, después, se quita de la respuesta de encabezado.

**Comportamiento predeterminado:** sobrescribir

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="external-max-age"></a>Max-Age externa
**Propósito:** determina el intervalo de max-age para el explorador en la revalidación de caché POP. En otras palabras, cantidad de tiempo que transcurrirá antes de que un explorador compruebe si hay una nueva versión de un recurso en un punto de presencia.

Si habilita esta característica se generará `Cache-Control: max-age` y `Expires` encabezados a partir de los puntos de presencia y enviarlos al cliente HTTP. De forma predeterminada, estos encabezados sobrescribirán los creados por el servidor de origen. Sin embargo, pueden utilizarse el tratamiento de encabezados Cache-Control y las características de tratamiento de encabezados Expires para modificar este comportamiento.

Información importante:

- Esta acción no afecta a las revalidaciones de caché del punto de presencia en el servidor de origen. Estos tipos de Revalidaciones vienen determinados por la `Cache-Control` y `Expires`  encabezados recibidos del servidor de origen y se pueden personalizar con la Max-Age interna predeterminada y las características de forzar Max-Age interna.
- Para configurar esta característica, especifique un valor entero y seleccione la unidad de tiempo que desee (por ejemplo, segundos, minutos, horas, etc.).
- Establezca esta característica en un valor negativo hace que los puntos de presencia enviar un `Cache-Control: no-cache` y un `Expires` hora en que se establece en el pasado con cada respuesta al explorador. Aunque un cliente HTTP no almacenará la respuesta en caché, esta configuración no afectará a la capacidad de los puntos de presencia para almacenar en caché la respuesta del servidor de origen.
- Al establecer la unidad de tiempo en "Off" se deshabilitará esta característica. El `Cache-Control` y `Expires`  encabezados que se almacena en caché con la respuesta del servidor de origen se pasarán al explorador.

**Comportamiento predeterminado:** desactivado

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="follow-redirects"></a>Seguir redireccionamientos
**Propósito:** determina si se pueden redirigir las solicitudes para el nombre de host definido en el encabezado de ubicación devuelto por un servidor de origen del cliente.

Información importante:

- Las solicitudes solo se pueden redirigir a los servidores perimetrales CNAME que correspondan a la misma plataforma.

Value|Resultado
-|-
Enabled|Las solicitudes se pueden redirigir.
Disabled|Las solicitudes no se redirigirán.

**Comportamiento predeterminado:** deshabilitado.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="force-internal-max-age"></a>Forzar Max-Age interna
**Propósito:** determina el intervalo de max-age de presencia en la revalidación de caché del servidor de origen. En otras palabras, la cantidad de tiempo que transcurrirá antes de que un punto de presencia compruebe si un recurso almacenado en caché coincide con el recurso que se almacena en el servidor de origen.

Información importante:

- Esta característica invalidará el intervalo de max-age definido en `Cache-Control` o `Expires` encabezados generados a partir de un servidor de origen.
- Esta característica no afecta a las revalidaciones de caché del explorador en el punto de presencia. Estos tipos de Revalidaciones vienen determinados por la `Cache-Control` o `Expires` encabezados enviados al explorador.
- Esta característica no tiene un efecto visible en la respuesta entregada por un punto de presencia al solicitante. Sin embargo, puede afectar a la cantidad de tráfico de revalidación enviado desde los puntos de presencia al servidor de origen.
- Para configurar esta característica:
    - Seleccione el código de estado al que se aplicará un valor de max-age interna.
    - Especifique un valor entero y seleccione la unidad de tiempo que desee (por ejemplo, segundos, minutos, horas, etc.). Este valor define el intervalo de max-age interna de la solicitud.

- Al establecer la unidad de tiempo en "Off" se deshabilitará esta característica. No se asignará un intervalo de max-age interna a los recursos solicitados. Si el encabezado original no incluye instrucciones de almacenamiento en caché, a continuación, el recurso se almacenará en caché según la configuración activa en la característica Max-Age interna predeterminada.

**Comportamiento predeterminado:** desactivado

#### <a name="compatibility"></a>Compatibilidad
Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta característica no se puede asociar con las siguientes condiciones de coincidencia: 
- Número de sistema autónomo (AS)
- Dirección IP de cliente
- Parámetro de cookie
- Regex de parámetro de cookie
- País
- Dispositivo
- Cname perimetral
- Dominio de referencia
- Literal de encabezado de solicitud
- Regex de encabezado de solicitud
- Carácter comodín de encabezado de solicitud
- Método de solicitud
- Esquema de solicitud
- Literal de consulta de dirección URL
- Regex de consulta de dirección URL
- Carácter comodín de consulta de dirección URL
- Parámetro de consulta de dirección URL

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="h264-support-http-progressive-download"></a>Compatibilidad de H.264 (Descarga progresiva de HTTP)
**Propósito:** determina los tipos de formatos de archivo H.264 que pueden utilizarse para transmitir contenido.

Información importante:

- Definir un conjunto delimitado por espacios de extensiones de nombre de archivo H.264 permitidas en la opción de extensiones de archivo. La opción de extensiones de archivo invalidará el comportamiento predeterminado. Para mantener la compatibilidad con MP4 y F4V, incluya esas extensiones de nombre de archivo al establecer esta opción. 
- Incluya un punto al especificar cada extensión de nombre de archivo (por ejemplo, _.mp4_, _.f4v_).

**Comportamiento predeterminado:** descarga progresiva de HTTP es compatible con medios MP4 y F4V de forma predeterminada.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="honor-no-cache-request"></a>Respetar solicitud de no almacenar en caché
**Propósito:** determina si de un cliente HTTP no almacenar en caché las solicitudes se reenviarán al servidor de origen.

Una solicitud de no almacenar en caché se produce cuando el cliente HTTP envía un `Cache-Control: no-cache` o `Pragma: no-cache` encabezado en la solicitud HTTP.

Value|Resultado
--|--
Enabled|Permite reenviar las solicitudes de no almacenar en caché de un cliente HTTP al servidor de origen, y el servidor de origen devolverá los encabezados y el cuerpo de respuesta a través del punto de presencia al cliente HTTP.
Disabled|Restablece el comportamiento predeterminado. El comportamiento predeterminado es impedir que las solicitudes de no almacenar en caché se reenvíen al servidor de origen.

Para todo el tráfico de producción, se recomienda dejar esta característica deshabilitada de forma predeterminada. De lo contrario, los servidores de origen no estarán protegidos frente a usuarios finales que accidentalmente podrían desencadenar muchas solicitudes de no almacenar en caché al actualizar las páginas web, o frente a muchos reproductores de medios populares que están codificados para enviar un encabezado de no almacenar en caché con cada solicitud de vídeo. No obstante, esta característica puede ser útil para aplicarla a determinados directorios de ensayo o pruebas que no son de producción, para poder extraer contenido nuevo a petición desde el servidor de origen.

El estado de la caché que se notificará para una solicitud que se puede reenviar a un servidor de origen debido a esta característica es `TCP_Client_Refresh_Miss`. El informe de estado de caché, que está disponible en el módulo principal de informes, proporciona información estadística por estado de la caché. Este informe le permite realizar un seguimiento del número y el porcentaje de solicitudes que se reenvían a un servidor de origen debido a esta característica.

**Comportamiento predeterminado:** deshabilitado.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="ignore-origin-no-cache"></a>Ignorar no almacenar en caché de origen
**Propósito:** determina si la CDN omitirá las siguientes directivas procedentes de un servidor de origen:

- `Cache-Control: private`
- `Cache-Control: no-store`
- `Cache-Control: no-cache`
- `Pragma: no-cache`

Información importante:

- Para configurar esta característica, defina una lista delimitada por espacios de códigos de estado para los que se pasarán por alto las directivas mencionadas anteriormente.
- El conjunto de códigos de estado válido para esta característica son: 200, 203, 300, 301, 302, 305, 307, 400, 401, 402, 403, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 500, 501, 502, 503, 504 y 505.
- Para deshabilitar esta característica, establézcala en un valor en blanco.

**Comportamiento predeterminado:** el comportamiento predeterminado es respetar las directivas mencionadas anteriormente.

#### <a name="compatibility"></a>Compatibilidad
Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta característica no se puede asociar con las siguientes condiciones de coincidencia: 
- Número de sistema autónomo (AS)
- Dirección IP de cliente
- Parámetro de cookie
- Regex de parámetro de cookie
- País
- Dispositivo
- Cname perimetral
- Dominio de referencia
- Literal de encabezado de solicitud
- Regex de encabezado de solicitud
- Carácter comodín de encabezado de solicitud
- Método de solicitud
- Esquema de solicitud
- Literal de consulta de dirección URL
- Regex de consulta de dirección URL
- Carácter comodín de consulta de dirección URL
- Parámetro de consulta de dirección URL

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="ignore-unsatisfiable-ranges"></a>Ignorar intervalos que no se puedan satisfacer 
**Propósito:** determina la respuesta que se devolverá a los clientes cuando una solicitud genere un código de estado 416 intervalo no se puede satisfacer solicitado.

De forma predeterminada, este código de estado se devuelve cuando no puede satisfacer la solicitud de intervalo de bytes especificado por un punto de presencia y no se especificó un campo de encabezado de solicitud If-Range.

Value|Resultado
-|-
Enabled|Impide que los puntos de presencia respondan a una solicitud de intervalo de bytes no válida con un código de estado 416 intervalo no se puede satisfacer solicitado. En su lugar, los servidores se proporcionarán al recurso solicitado y devolverán 200 OK al cliente.
Disabled|Restablece el comportamiento predeterminado. El comportamiento predeterminado es respetar el código de estado 416 intervalo no se puede satisfacer solicitado.

**Comportamiento predeterminado:** deshabilitado.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="internal-max-stale"></a>Max-Stale interna
**Propósito:** controla cuánto tiempo después de la hora de expiración normal puede atenderse un recurso almacenado en caché desde un punto de presencia cuando este no se puede validar el recurso almacenado en caché con el servidor de origen.

Normalmente, cuando se agota el tiempo de max-age del activo, el punto de presencia envía una solicitud de revalidación al servidor de origen. El servidor de origen se responde con 304 no modificado para proporcionar el punto de presencia una nueva concesión sobre el recurso almacenado en caché, o con 200 OK para proporcionar el punto de presencia con una versión actualizada del recurso almacenado en caché.

Si el punto de presencia no puede establecer una conexión con el servidor de origen al intentar esta revalidación, esta característica Max-Stale interna controla si el punto de presencia puede continuar proporcionando el recurso ahora obsoleto, y durante cuánto tiempo.

Tenga en cuenta que este intervalo de tiempo se inicia cuando expira la duración máxima del recurso, no cuando se produce la revalidación errónea. Por lo tanto, el período máximo durante el cual puede proporcionarse un recurso sin una revalidación correcta es la cantidad de tiempo especificado por la combinación de max-age más max-stale. Por ejemplo, si un recurso se almacenó en caché a las 9:00 con una antigüedad máxima de 30 minutos y un max-stale de 15 minutos, un intento de revalidación errónea de 9:44 daría lugar en un usuario final recibe el recurso almacenado en caché obsoleto, mientras que daría como resultado la en un intento de revalidación errónea a las 9:46 usuario recibe un tiempo de espera de puerta de enlace 504 d.

Los valores configurados para esta característica se ha reemplazado por `Cache-Control: must-revalidate` o `Cache-Control: proxy-revalidate` encabezados recibidos del servidor de origen. Si alguno de los encabezados se recibe desde el servidor de origen cuando un recurso se almacena inicialmente en caché, el punto de presencia no proporcionará un recurso almacenado en caché obsoleto. En tal caso, si el punto de presencia no puede revalidar con el origen ha expirado el intervalo de max-age del recurso, el servidor POP devuelve un error de tiempo de espera de puerta de enlace 504.

Información importante:

- Para configurar esta característica:
    - Seleccione el código de estado al que se aplicará un valor de max-stale.
    - Especifique un valor entero y seleccione la unidad de tiempo que desee (por ejemplo, segundos, minutos, horas, etc.). Este valor define la max-stale interna que se aplicará.

- Al establecer la unidad de tiempo en "Off" se deshabilitará esta característica. No se proporcionarán recursos almacenados en caché más allá de la fecha de expiración normal.

**Comportamiento predeterminado:** dos minutos

#### <a name="compatibility"></a>Compatibilidad
Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta característica no se puede asociar con las siguientes condiciones de coincidencia: 
- Número de sistema autónomo (AS)
- Dirección IP de cliente
- Parámetro de cookie
- Regex de parámetro de cookie
- País
- Dispositivo
- Cname perimetral
- Dominio de referencia
- Literal de encabezado de solicitud
- Regex de encabezado de solicitud
- Carácter comodín de encabezado de solicitud
- Método de solicitud
- Esquema de solicitud
- Literal de consulta de dirección URL
- Regex de consulta de dirección URL
- Carácter comodín de consulta de dirección URL
- Parámetro de consulta de dirección URL

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="log-query-string"></a>Cadena de consulta del registro
**Propósito:** determina si una cadena de consulta se almacenará junto con la dirección URL en los registros de acceso.

Value|Resultado
-|-
Enabled|Permite almacenar cadenas de consulta cuando se anotan direcciones URL en un registro de acceso. Si una dirección URL no contiene una cadena de consulta, esta opción no tendrá efecto.
Disabled|Restablece el comportamiento predeterminado. El comportamiento predeterminado es pasar por alto las cadenas de consulta cuando se anotan direcciones URL en un registro de acceso.

**Comportamiento predeterminado:** deshabilitado.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="maximum-keep-alive-requests"></a>Número máximo de solicitudes de conexión persistente
**Propósito:** define el número máximo de solicitudes para una conexión Keep-Alive antes de cerrarse.

Es recomendable no establecer un valor bajo en el número máximo de solicitudes porque puede dar como resultado una degradación del rendimiento.

Información importante:

- Especifique este valor como un número entero.
- No incluya comas ni puntos en el valor especificado.

**Valor predeterminado:** 10.000 solicitudes

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="modify-client-request-header"></a>Modificar encabezado de solicitud de cliente
**Propósito:** cada solicitud contiene un conjunto de encabezados de solicitud que lo describen. Esta característica puede:

- Anexar o sobrescribir el valor asignado a un encabezado de solicitud. Si el encabezado de solicitud especificado no existe, esta característica lo agregará a la solicitud.
- Eliminar un encabezado de solicitud de la solicitud.

Las solicitudes que se reenvían a un servidor de origen reflejarán los cambios realizados por esta característica.

En un encabezado de solicitud se puede realizar una de las siguientes acciones:

Opción|DESCRIPCIÓN|Ejemplo
-|-|-
Append|El valor especificado se agregará al final del valor del encabezado de solicitud existente.|**Valor de encabezado de solicitud (cliente):**<br/>Valor1<br/>**Valor de encabezado de solicitud (motor de reglas):**<br/>Valor2 <br/>**Nuevo valor de encabezado de solicitud:** <br/>Valor1Valor2
Sobrescribir|El valor del encabezado de solicitud se establecerá en el valor especificado.|**Valor de encabezado de solicitud (cliente):**<br/>Valor1<br/>**Valor de encabezado de solicitud (motor de reglas):**<br/>Valor2<br/>**Nuevo valor de encabezado de solicitud:**<br/> Valor2 <br/>
Eliminar|Elimina el encabezado de solicitud especificado.|**Valor de encabezado de solicitud (cliente):**<br/>Valor1<br/>**Modificar configuración de encabezado de solicitud de cliente:**<br/>Eliminar el encabezado de solicitud en cuestión.<br/>**Resultado:**<br/>El encabezado de solicitud especificado no se reenviará al servidor de origen.

Información importante:

- Asegúrese de que el valor especificado en la opción de nombre es una coincidencia exacta para el encabezado de solicitud deseado.
- No se tienen en cuenta las mayúsculas y minúsculas para identificar un encabezado. Por ejemplo, cualquiera de las siguientes variaciones de la `Cache-Control` nombre de encabezado puede usarse para identificarlo:
    - cache-control
    - CACHE-CONTROL
    - cachE-Control
- Al especificar un nombre de encabezado, use solamente caracteres alfanuméricos, guiones o caracteres de subrayado.
- Si elimina un encabezado, impedirá que los puntos de presencia lo reenvíen a un servidor de origen.
- Los encabezados siguientes están reservados y esta característica no los puede modificar:
    - forwarded
    - host
    - via
    - Warning (Advertencia)
    - x-forwarded-for
    - Todos los nombres de encabezado que empiezan por "x-ce" están reservados.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="modify-client-response-header"></a>Modificar encabezado de respuesta de cliente
Cada respuesta contiene un conjunto de encabezados de respuesta que lo describen. Esta característica puede:

- Anexar o sobrescribir el valor asignado a un encabezado de respuesta. Si el encabezado de respuesta especificado no existe, esta característica lo agregará a la respuesta.
- Eliminar un encabezado de respuesta de la respuesta.

De forma predeterminada, un servidor de origen y los puntos de presencia definen los valores del encabezado de respuesta.

En un encabezado de respuesta se puede realizar una de las siguientes acciones:

Opción|DESCRIPCIÓN|Ejemplo
-|-|-
Append|El valor especificado se agregará al final del valor del encabezado de respuesta existente.|**Valor de encabezado de respuesta (cliente):**<br />Valor1<br/>**Valor de encabezado de respuesta (motor de reglas):**<br/>Valor2<br/>**Nuevo valor de encabezado de respuesta:**<br/>Valor1Valor2
Sobrescribir|El valor del encabezado de respuesta se establecerá en el valor especificado.|**Valor de encabezado de respuesta (cliente):**<br/>Valor1<br/>**Valor de encabezado de respuesta (motor de reglas):**<br/>Valor2 <br/>**Nuevo valor de encabezado de respuesta:**<br/>Valor2 <br/>
Eliminar|Elimina el encabezado de respuesta especificado.|**Valor de encabezado de respuesta (cliente):**<br/>Valor1<br/>**Modificar configuración de encabezado de respuesta de cliente:**<br/>Eliminar el encabezado de respuesta en cuestión.<br/>**Resultado:**<br/>El encabezado de respuesta especificado no se reenviará al solicitante.

Información importante:

- Asegúrese de que el valor especificado en la opción de nombre es una coincidencia exacta para el encabezado de respuesta deseado. 
- No se tienen en cuenta las mayúsculas y minúsculas para identificar un encabezado. Por ejemplo, cualquiera de las siguientes variaciones de la `Cache-Control` nombre de encabezado puede usarse para identificarlo:
    - cache-control
    - CACHE-CONTROL
    - cachE-Control
- Si elimina un encabezado, impedirá que se reenvíe al solicitante.
- Los encabezados siguientes están reservados y esta característica no los puede modificar:
    - accept-encoding
    - age
    - connection
    - content-encoding
    - content-length
    - content-range
    - date
    - Servidor
    - trailer
    - transfer-encoding
    - upgrade
    - vary
    - via
    - Warning (Advertencia)
    - Todos los nombres de encabezado que empiezan por "x-ce" están reservados.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="partial-cache-sharing"></a>Uso compartido de caché parcial
**Propósito:** determina si una solicitud puede generar contenido almacenado parcialmente en caché.

Esta caché parcial puede utilizarse para atender nuevas solicitudes de ese contenido hasta que se almacene en caché todo el contenido solicitado.

Value|Resultado
-|-
Enabled|Las solicitudes pueden generar contenido almacenado parcialmente en caché.
Disabled|Las solicitudes solamente pueden generar una versión completamente almacenada en caché del contenido solicitado.

**Comportamiento predeterminado:** deshabilitado.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="prevalidate-cached-content"></a>PreValidate contenido en caché
**Propósito:** determina si contenido almacenado en caché será apto para la revalidación temprana antes de que expire su período de vida.

Defina la cantidad de tiempo antes de la expiración del período de vida del contenido solicitado durante el cual será apto para la revalidación temprana.

Información importante:

- Cuando se selecciona "Off" como unidad de tiempo, la revalidación debe realizarse después de que el contenido almacenado en caché haya expirado. No se debe especificar el tiempo y se pasará por alto.

**Comportamiento predeterminado:** Off. La revalidación solo puede realizarse después de que expire el período de vida del contenido almacenado en caché.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="proxy-special-headers"></a>Encabezados de proxy especiales
**Propósito:** define el conjunto de [encabezados de solicitud HTTP específicos de Verizon](cdn-verizon-http-headers.md) que se reenviará desde un punto de presencia a un servidor de origen.

Información importante:

- Cada encabezado de solicitud específico de la red CDN definido en esta característica se reenviará a un servidor de origen. No se reenviarán los encabezados excluidos.
- Para impedir que se reenvíe un encabezado de solicitud específico de la red CDN, quítelo de la lista separada por espacios en el campo de lista de encabezados.

En la lista predeterminada, se incluyen los siguientes encabezados HTTP:
- Via
- X-Forwarded-For
- X-Forwarded-Proto
- X-Host
- X-Midgress
- X-Gateway-List
- X-EC-Name
- Host

**Comportamiento predeterminado:** todos los encabezados de solicitud específicos de la red CDN se reenviarán al servidor de origen.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="refresh-zero-byte-cache-files"></a>Actualizar archivos de caché de cero bytes
**Propósito:** determina cómo controlan los puntos de presencia la solicitud del cliente HTTP para un recurso de la caché de 0 bytes.

Los valores válidos son:

Value|Resultado
--|--
Enabled|Hace que el punto de presencia vuelva a capturar el recurso desde el servidor de origen.
Disabled|Restablece el comportamiento predeterminado. El comportamiento predeterminado es atender los recursos de caché válidos cuando se soliciten.

Esta característica no es necesaria para la entrega de contenido y un almacenamiento en caché correcto, pero puede resultar útil para solucionar este problema. Por ejemplo, los generadores de contenido dinámico en los servidores de origen pueden provocar accidentalmente que se envíen respuestas de 0 bytes a los puntos de presencia. Normalmente, los puntos de presencia almacenan estos tipos de respuestas en la caché. Si sabe que una respuesta de 0 bytes nunca es una respuesta válida 

para ese tipo de contenido, esta característica puede impedir que se proporcionen estos tipos de recursos a los clientes.

**Comportamiento predeterminado:** deshabilitado.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="set-cacheable-status-codes"></a>Establecer códigos de estado almacenables en caché
**Propósito:** define el conjunto de códigos de estado que puede dar lugar a contenido almacenado en caché.

De forma predeterminada, el almacenamiento en caché solo está habilitada para las 200 respuestas OK.

Defina un conjunto de los códigos de estado deseados, delimitados por espacios.

Información importante:

- Habilitar la característica ignorar origen No-Cache. Si no está habilitada esta característica, a continuación, las respuestas que no sean 200 OK podrían no ser en caché.
- El conjunto de códigos de estado válido para esta característica son: 203, 300, 301, 302, 305, 307, 400, 401, 402, 403, 404, 405, 406, 407, 408, 409, 410, 411, 412, 413, 414, 415, 416, 417, 500, 501, 502, 503, 504 y 505.
- No se puede usar esta característica para deshabilitar el almacenamiento en caché para las respuestas que generan un código de 200 estado OK.

**Comportamiento predeterminado:** almacenamiento en caché está habilitada solo para las respuestas que generan un código de 200 estado OK.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="set-client-ip-custom-header"></a>Establecer encabezado personalizado de IP de cliente
**Propósito:** agrega un encabezado personalizado que identifica al cliente solicitante por dirección IP para la solicitud.

La opción de nombre de encabezado define el nombre del encabezado de solicitud personalizado donde se almacena la dirección IP del cliente.

Esta característica permite al servidor de origen del cliente averiguar las direcciones IP del cliente mediante un encabezado de solicitud personalizado. Si la solicitud se envió desde la memoria caché, no se informará al servidor de origen sobre la dirección IP del cliente. Por lo tanto, se recomienda que se usa esta característica a los recursos que no se almacenan en caché.

Asegúrese de que el nombre de encabezado especificado no coincida con ninguno de los nombres siguientes:

- Nombres de encabezado de solicitud estándar. Encontrará una lista de nombres de encabezado estándar en [RFC 2616](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html).
- Nombres de encabezado reservados:
    - forwarded-for
    - host
    - vary
    - via
    - Warning (Advertencia)
    - x-forwarded-for
    - Todos los nombres de encabezado que empiezan por "x-ce" están reservados.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="stale-content-delivery-on-error"></a>Entrega de contenido obsoleto en caso de error
**Propósito:** determina si se entregará el contenido almacenado en caché expirado cuando se produce un error durante la revalidación de caché o al recuperar el contenido solicitado desde el servidor de origen de cliente.

Value|Resultado
-|-
Enabled|Cuando se produce un error durante una conexión a un servidor de origen, se sirve contenido obsoleto al solicitante.
Disabled|El error del servidor de origen se reenvía al solicitante.

**Comportamiento predeterminado:** deshabilitado

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="stale-while-revalidate"></a>Obsoleto durante revalidación
**Propósito:** mejora el rendimiento al permitir que los puntos de presencia proporcionen contenido obsoleto al solicitante mientras lleva a cabo la revalidación.

Información importante:

- El comportamiento de esta característica varía según la unidad de tiempo seleccionada.
    - **Unidad de tiempo:** especifique un período de tiempo y seleccione una unidad de tiempo (por ejemplo, segundos, minutos, horas, etc.) para permitir la entrega de contenido obsoleto. Este tipo de configuración permite que la red CDN amplíe el período de tiempo que puede entregar contenido antes de requerir la validación según la siguiente fórmula: **TTL** + **obsoletos al tiempo de revalidación** 
    - **OFF:** seleccione "Off" para exigir la renovación antes de la solicitud para atender contenido obsoleto.
        - No especifique un período de tiempo ya que no es aplicable y se pasará por alto.

**Comportamiento predeterminado:** Off. La revalidación debe realizarse antes de que se pueda servir el contenido solicitado.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="token-auth"></a>Autenticación de token
**Propósito:** determina si se aplicará a una solicitud de autenticación basada en Token.

Si está habilitada la autenticación basada en token, solo se admitirán las solicitudes que proporcionen un token cifrado y cumplan los requisitos especificados por ese token.

La clave de cifrado que se utiliza para cifrar y descifrar los valores de token viene determinada por la clave principal y las opciones de copia de seguridad clave en la página de autenticación de Token. Tenga en cuenta que las claves de cifrado son específicas de la plataforma.

**Comportamiento predeterminado:** deshabilitado.

Esta característica tiene prioridad sobre la mayoría de las características con la excepción de la característica Reescritura de direcciones URL.

Value | Resultado
------|---------
Enabled | Protege el contenido solicitado con autenticación basada en token. Solo se admitirán las solicitudes de los clientes que proporcionen un token válido y cumplan los requisitos. Las transacciones de FTP se excluyen de la autenticación basada en token.
Disabled| Restablece el comportamiento predeterminado. El comportamiento predeterminado es permitir la configuración de la autenticación basada en token para determinar si se protegerá una solicitud.

#### <a name="compatibility"></a>Compatibilidad
No use la autenticación de token con una condición de coincidencia Siempre. 

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="token-auth-denial-code"></a>Código de denegación de autorización de token
**Propósito:** determina el tipo de respuesta que se devolverá al usuario cuando se deniega una solicitud debido a la autenticación basada en token.

Los códigos de respuesta disponibles se enumeran en la tabla siguiente.

Código de respuesta|Nombre de la respuesta|DESCRIPCIÓN
-------------|-------------|--------
301|Movido permanentemente|Este código de estado redirige a los usuarios no autorizados a la dirección URL especificada en el encabezado Location.
302|Encontrado|Este código de estado redirige a los usuarios no autorizados a la dirección URL especificada en el encabezado Location. Este código de estado es el método estándar del sector para llevar a cabo una redirección.
307|Redirección temporal|Este código de estado redirige a los usuarios no autorizados a la dirección URL especificada en el encabezado Location.
401|No autorizado|Este código de estado de combinación con el encabezado de respuesta WWW-Authenticate permite solicitar al usuario para la autenticación.
403|Prohibido|Este mensaje es el mensaje estándar 403 Prohibido estado que un usuario no autorizado verá al intentar acceder al contenido protegido.
404|Archivo no encontrado|Este código de estado indica que el cliente HTTP fue capaz de comunicarse con el servidor, pero no se encontró el contenido solicitado.

#### <a name="compatibility"></a>Compatibilidad
No use el Código de denegación de autorización de token con una condición de coincidencia Siempre. En su lugar, use la sección **Custom Denial Handling** (Control de denegación personalizado) en la página **Token de autenticación** del portal **Administrar**. Para más información, consulte [Protección de los activos de Azure CDN con autenticación por tokens](cdn-token-auth.md).

#### <a name="url-redirection"></a>Redirección de URL

Esta característica admite la redirección de URL a una dirección URL definida por el usuario cuando se configura para devolver un código de estado 3xx. Para especificar esta dirección URL definida por el usuario, siga los pasos siguientes:

1. Seleccione un código de respuesta 3xx para la característica de código de denegación de autorización de Token.
2. Seleccione "Location" en la opción de nombre de encabezado opcional.
3. Establezca la opción de valor de encabezado opcional en la dirección URL deseada.

Si una dirección URL no está definida para un código de estado 3xx, se devolverá al usuario la página de respuesta estándar para un código de estado 3xx.

Redirección de URL solo es aplicable a códigos de respuesta 3xx.

La opción de valor de encabezado opcional admite caracteres alfanuméricos, comillas y espacios.

#### <a name="authentication"></a>Authentication

Esta característica es compatible con la capacidad de incluir el encabezado WWW-Authenticate al responder a una solicitud no autorizada para el contenido protegido por autenticación basada en Token. Si el encabezado WWW-Authenticate se ha establecido en "basic" en la configuración, el usuario no autorizado se pedirá las credenciales de cuenta.

Para realizar la configuración anterior, siga los pasos siguientes:

1. Seleccione “401” como código de respuesta para la característica de código de denegación de autenticación de token.
2. Seleccione "WWW-Authenticate" en la opción de nombre de encabezado opcional.
3. Establecer la opción de valor de encabezado opcional "basic".

El encabezado WWW-Authenticate solo es aplicable para los códigos de respuesta 401.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="token-auth-ignore-url-case"></a>Ignorar mayúsculas y minúsculas en URL de autenticación de token
**Propósito:** determina si las comparaciones de URL realizadas por la autenticación basada en tokens distinguen mayúsculas de minúsculas.

Los parámetros que se ve afectados por esta característica son:

- ec_url_allow
- ec_ref_allow
- ec_ref_deny

Los valores válidos son:

Value|Resultado
---|----
Enabled|Hace que el punto de presencia no tenga en cuenta las mayúsculas y minúsculas al comparar las direcciones URL de los parámetros de autenticación basada en tokens.
Disabled|Restablece el comportamiento predeterminado. Es el comportamiento predeterminado de la comparación de direcciones URL es que la autenticación basada en token distinga mayúsculas de minúsculas.

**Comportamiento predeterminado:** deshabilitado.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="token-auth-parameter"></a>Parámetro de autenticación de token
**Propósito:** determina si debe cambiarse el parámetro de cadena de consulta de autenticación basada en Token.

Información importante:

- La opción de valor define el nombre del parámetro de cadena de consulta a través del cual se puede especificar un token.
- La opción de valor no puede establecerse en "ec_token."
- Asegúrese de que el nombre definido en la opción de valor contiene caracteres de dirección URL solo es válidos.

Value|Resultado
----|----
Enabled|La opción de valor define el nombre del parámetro de cadena de consulta a través del cual se deben definir los tokens.
Disabled|Un token se puede especificar como un parámetro de cadena de consulta no definido en la dirección URL de la solicitud.

**Comportamiento predeterminado:** deshabilitado. Un token se puede especificar como un parámetro de cadena de consulta no definido en la dirección URL de la solicitud.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="url-redirect"></a>Redirección de URL
**Propósito:** redirige las solicitudes a través del encabezado de ubicación.

Para configurar esta característica hay que establecer las siguientes opciones:

Opción|DESCRIPCIÓN
-|-
Código|Seleccione el código de respuesta que se devolverá al solicitante.
Origen y patrón| Esta opción define un patrón de URI de solicitud que identifica el tipo de solicitudes que se pueden redirigir. Solo se redirigirán las solicitudes cuya dirección URL satisfaga ambos criterios siguientes: <br/> <br/> **Origen (o punto de acceso al contenido):** seleccione una ruta de acceso relativa que identifica un servidor de origen. Esta ruta es la sección _/XXXX/_ y el nombre del punto de conexión. <br/><br/> **Origen (patrón):** se debe definir un patrón que identifica solicitudes por ruta de acceso relativa. Este patrón de expresión regular debe definir una ruta de acceso que comienza directamente después del acceso al contenido seleccionado anteriormente señalan (vea más arriba). <br/> - Asegúrese de que los criterios de URI de solicitud (es decir, el origen y el patrón) definidos anteriormente no entren en conflicto con las condiciones de coincidencia definidas para esta característica. <br/> -Especifique un patrón; si usa un valor en blanco como patrón, se busca la coincidencia con todas las cadenas.
Destino| Defina la dirección URL a la que se redirigirán las solicitudes anteriores. <br/><br/> Construya esta dirección URL dinámicamente mediante: <br/> -Un patrón de expresión regular <br/>- [Variables HTTP](cdn-http-variables.md) <br/><br/> Sustituya los valores capturados en el modelo de origen en el patrón de destino mediante $_n_ donde _n_ identifica un valor por el orden en que se capturó. Por ejemplo, $1 representa el primer valor capturado en el patrón de origen, mientras que $2 representa el segundo valor. <br/> 
Se recomienda usar una dirección URL absoluta. El uso de direcciones URL relativas podría redirigir direcciones URL de la red CDN a rutas de acceso no válidas.

**Escenario de ejemplo**

En este ejemplo, se muestra cómo redirigir una dirección URL del servidor perimetral CNAME que se resuelve en esta dirección URL de la red CDN base: http:\//marketing.azureedge.net/brochures

Las solicitudes aptas se redirigirán a esta dirección URL del servidor perimetral CNAME: http:\//cdn.mydomain.com/resources

Esta redirección de URL se puede lograr a través de la configuración siguiente: ![Redirección de URL](./media/cdn-rules-engine-reference/cdn-rules-engine-redirect.png)

**Puntos clave:**

- La característica Redirección de URL define las direcciones URL de solicitud que se redirigirán. Como resultado, no se necesitan otras condiciones de coincidencia. Aunque la condición de coincidencia se definió como "Always", solo se redirigirán las solicitudes que apunten a la carpeta "brochures" en el origen del cliente "marketing". 
- Todas las solicitudes coincidentes se redirigirán a la dirección URL de CNAME definida en la opción de destino de edge. 
    - Escenario de ejemplo 1: 
        - Solicitud de ejemplo (dirección URL de CDN): http:\//marketing.azureedge.net/brochures/widgets.pdf 
        - URL de solicitud (después de la redirección): http:\//cdn.mydomain.com/resources/widgets.pdf  
    - Escenario de ejemplo 2: 
        - Solicitud de ejemplo (URL de servidor perimetral CNAME): http:\//marketing.mydomain.com/brochures/widgets.pdf 
        - URL de solicitud (después de la redirección): http:\//cdn.mydomain.com/resources/widgets.pdf
    - Escenario de ejemplo 3: 
        - Solicitud de ejemplo (URL de servidor perimetral CNAME): http:\//brochures.mydomain.com/campaignA/final/productC.ppt 
        - URL de solicitud (después de la redirección): http:\//cdn.mydomain.com/resources/campaignA/final/productC.ppt  
- La variable de esquema de solicitud (% {scheme}) se aprovecha en la opción de destino, lo que garantiza que el esquema de la solicitud permanece sin cambios después de la redirección.
- Los segmentos de dirección URL que se capturaron de la solicitud se anexan a la nueva dirección URL a través de "$1".

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="url-rewrite"></a>Reescritura de direcciones URL
**Propósito:** reescribe la dirección URL de solicitud.

Información importante:

- Para configurar esta característica hay que establecer las siguientes opciones:

Opción|DESCRIPCIÓN
-|-
 Origen y patrón | Esta opción define un patrón de URI de solicitud que identifica el tipo de solicitudes que se pueden reescribir. Solo se reescribirán las solicitudes cuya dirección URL satisfaga ambos criterios siguientes: <br/><br/>  - **Origen (o punto de acceso al contenido):** seleccione una ruta de acceso relativa que identifica un servidor de origen. Esta ruta es la sección _/XXXX/_ y el nombre del punto de conexión. <br/><br/> - **Origen (patrón):** se debe definir un patrón que identifica solicitudes por ruta de acceso relativa. Este patrón de expresión regular debe definir una ruta de acceso que comienza directamente después del acceso al contenido seleccionado anteriormente señalan (vea más arriba). <br/> Compruebe que los criterios de URI de solicitud (es decir, el origen y el patrón) definidos anteriormente no entren en conflicto con las condiciones de coincidencia definidas para esta característica. Especifique un patrón; si usa un valor en blanco como patrón, se busca la coincidencia con todas las cadenas. 
 Destino  |Defina la dirección URL relativa en la que se sobrescribirán las solicitudes anteriores: <br/>    1. Selección de un punto de acceso al contenido que identifica un servidor de origen. <br/>    2. Defina el uso de una ruta de acceso relativa: <br/>        -Un patrón de expresión regular <br/>        - [Variables HTTP](cdn-http-variables.md) <br/> <br/> Sustituya los valores capturados en el modelo de origen en el patrón de destino mediante $_n_ donde _n_ identifica un valor por el orden en que se capturó. Por ejemplo, $1 representa el primer valor capturado en el patrón de origen, mientras que $2 representa el segundo valor. 

 Esta característica permite que los puntos de presencia vuelvan a escribir la dirección URL sin realizar una redirección tradicional. Es decir, el solicitante recibirá el mismo código de respuesta que si hubiera solicitado la reescritura de la dirección URL.

**Escenario de ejemplo 1**

En este ejemplo, se muestra cómo redirigir una dirección URL del servidor perimetral CNAME que se resuelve en esta dirección URL de la red CDN base: http:\//marketing.azureedge.net/brochures/

Las solicitudes aptas se redirigirán a esta dirección URL del servidor perimetral CNAME: http:\//MyOrigin.azureedge.net/resources/

Esta redirección de URL se puede lograr a través de la configuración siguiente: ![Redirección de URL](./media/cdn-rules-engine-reference/cdn-rules-engine-rewrite.png)

**Escenario de ejemplo 2**

En este ejemplo, se muestra cómo redirigir una dirección URL del servidor perimetral CNAME de MAYÚSCULAS a minúsculas mediante expresiones regulares.

Esta redirección de URL se puede lograr a través de la configuración siguiente: ![Redirección de URL](./media/cdn-rules-engine-reference/cdn-rules-engine-to-lowercase.png)


**Puntos clave:**

- La característica Reescritura de URL define las direcciones URL de solicitud que se reescribirán. Como resultado, no se necesitan otras condiciones de coincidencia. Aunque la condición de coincidencia se definió como "Always", solo se reescribirán las solicitudes que apunten a la carpeta "brochures" en el origen del cliente "marketing".

- Los segmentos de dirección URL que se capturaron de la solicitud se anexan a la nueva dirección URL a través de "$1".

#### <a name="compatibility"></a>Compatibilidad
Esta característica incluye los criterios de coincidencia que deben cumplirse para poder aplicarla a una solicitud. Para evitar configurar criterios de coincidencia que generen conflictos, esta característica es incompatible con las siguientes condiciones de coincidencia:

- Número de sistema autónomo (AS)
- Origen de red CDN
- Dirección IP de cliente
- Origen de cliente
- Esquema de solicitud
- Directorio de ruta de acceso URL
- Extensión de ruta de acceso URL
- Nombre de archivo de ruta de acceso URL
- Literal de ruta de acceso URL
- Regex de ruta de acceso URL
- Carácter comodín de ruta de acceso URL
- Literal de consulta de dirección URL
- Parámetro de consulta de dirección URL
- Regex de consulta de dirección URL
- Carácter comodín de consulta de dirección URL

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

---
### <a name="user-variable"></a>Variable USER
**Propósito:** solo para uso interno.

[Volver arriba](#azure-cdn-rules-engine-features)

</br>

## <a name="next-steps"></a>Pasos siguientes
* [Referencia del motor de reglas](cdn-rules-engine-reference.md)
* [Expresiones condicionales del motor de reglas](cdn-rules-engine-reference-conditional-expressions.md)
* [Condiciones de coincidencia del motor de reglas](cdn-rules-engine-reference-match-conditions.md)
* [Invalidación del comportamiento HTTP mediante el motor de reglas](cdn-rules-engine.md)
* [Información general de Azure CDN](cdn-overview.md)
