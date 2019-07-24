---
title: Condiciones de coincidencia del motor de reglas de Azure CDN de Verizon Premium | Microsoft Docs
description: Documentación de referencia sobre las condiciones de coincidencia del motor de reglas de Azure Content Delivery Network de Verizon Premium.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus
ms.openlocfilehash: 1660dca34b2f128ef5889145fcdeed0d2523b9bb
ms.sourcegitcommit: ccb9a7b7da48473362266f20950af190ae88c09b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67593199"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-match-conditions"></a>Condiciones de coincidencia del motor de reglas de Azure CDN de Verizon Premium

En este artículo se muestran descripciones detalladas de las condiciones de coincidencia disponibles para el [motor de reglas](cdn-verizon-premium-rules-engine.md) de Azure Content Delivery Network (CDN) de Verizon Premium.

La segunda parte de una regla es la condición de coincidencia. Una condición de coincidencia identifica tipos específicos de solicitudes para los que se ejecutará un conjunto de características.

Por ejemplo, puede usar una condición de coincidencia para:

- Filtrar las solicitudes de contenido de una ubicación concreta.
- Filtrar las solicitudes generadas a partir de una dirección IP o país o región específicos.
- Filtrar las solicitudes por la información de encabezado.

## <a name="always-match-condition"></a>Condición de coincidencia Siempre

La condición de coincidencia Siempre aplica un conjunto predeterminado de características a todas las solicitudes.

NOMBRE | Propósito
-----|--------
[Siempre](#always) | Se aplica un conjunto predeterminado de características a todas las solicitudes.

## <a name="device-match-condition"></a>Condición de coincidencia Dispositivo

La condición de coincidencia Dispositivo identifica solicitudes realizadas desde un dispositivo móvil en función de sus propiedades.  

NOMBRE | Propósito
-----|--------
[Dispositivo](#device) | Identifica solicitudes realizadas desde un dispositivo móvil en función de sus propiedades.

## <a name="location-match-conditions"></a>Condiciones de coincidencia Ubicación

Las condiciones de coincidencia Ubicación identifican solicitudes en función de la ubicación del solicitante.

NOMBRE | Propósito
-----|--------
[Número de sistema autónomo (AS)](#as-number) | Identifica solicitudes que se originan en una red determinada.
[País](#country) | Identifica solicitudes que se originan en países o regiones determinados.

## <a name="origin-match-conditions"></a>Condiciones de coincidencia Origen

Las condiciones de coincidencia Origen identifican solicitudes que señalan al servidor de origen de un cliente o de almacenamiento de Content Delivery Network.

NOMBRE | Propósito
-----|--------
[Origen de red CDN](#cdn-origin) | Identifica las solicitudes de contenido almacenado en el almacenamiento de Content Delivery Network.
[Origen de cliente](#customer-origin) | Identifica solicitudes de contenido almacenado en el servidor de origen de un cliente específico.

## <a name="request-match-conditions"></a>Condiciones de coincidencia Solicitud

Las condiciones de coincidencia Solicitud identifican solicitudes en función de sus propiedades.

NOMBRE | Propósito
-----|--------
[Dirección IP de cliente](#client-ip-address) | Identifica solicitudes que se originan en una dirección IP determinada.
[Parámetro de cookie](#cookie-parameter) | Busca el valor especificado en las cookies asociadas a cada solicitud.
[Regex de parámetro de cookie](#cookie-parameter-regex) | Busca la expresión regular especificada en las cookies asociadas a cada solicitud.
[Cname perimetral](#edge-cname) | Identifica solicitudes que señalan a un CNAME perimetral específico .
[Dominio de referencia](#referring-domain) | Identifica solicitudes a las que se haga referencia en los nombres de host especificados.
[Literal de encabezado de solicitud](#request-header-literal) | Identifica solicitudes que contienen el encabezado que se especifique en un valor concreto.
[Regex de encabezado de solicitud](#request-header-regex) | Identifica solicitudes que contienen el encabezado que se especifique establecido en la expresión regular especificada.
[Carácter comodín de encabezado de solicitud](#request-header-wildcard) | Identifica solicitudes que contienen el encabezado que se especifique establecido en un valor que coincide con el patrón especificado.
[Método de solicitud](#request-method) | Identifica solicitudes por su método HTTP.
[Esquema de solicitud](#request-scheme) | Identifica solicitudes por su protocolo HTTP.

## <a name="url-match-conditions"></a>Condiciones de coincidencia URL

Las condiciones de coincidencia URL identifican solicitudes en función de sus direcciones URL.

NOMBRE | Propósito
-----|--------
[Directorio de la ruta de acceso URL](#url-path-directory) | Identifica solicitudes por su ruta de acceso relativa.
[Extensión de la ruta de acceso URL](#url-path-extension) | Identifica solicitudes por su extensión de nombre de archivo.
[Nombre de archivo de la ruta de acceso URL](#url-path-filename) | Identifica solicitudes por su nombre de archivo.
[Literal de la ruta de acceso URL](#url-path-literal) | Compara la ruta de acceso relativa de una solicitud con el valor especificado.
[Regex de la ruta de acceso URL](#url-path-regex) | Compara la ruta de acceso relativa de una solicitud con la expresión regular especificada.
[Carácter comodín de la ruta de acceso URL](#url-path-wildcard) | Compara la ruta de acceso relativa de una solicitud con el patrón especificado.
[Literal de la consulta URL](#url-query-literal) | Compara la cadena de consulta de una solicitud con el valor especificado.
[Parámetro de la consulta URL](#url-query-parameter) | Identifica solicitudes que contienen el parámetro de cadena de consulta que se especifique establecido en un valor que coincide con el patrón especificado.
[Regex de la consulta URL](#url-query-regex) | Identifica solicitudes que contienen el parámetro de cadena de consulta que se especifique establecido en un valor que coincide con la expresión regular especificada.
[Carácter comodín de la consulta URL](#url-query-wildcard) | Compara los valores especificados con la cadena de consulta de la solicitud.

## <a name="reference-for-rules-engine-match-conditions"></a>Referencia de las condiciones de coincidencia del motor de reglas

---

### <a name="always"></a>Siempre

La condición de coincidencia Siempre aplica un conjunto predeterminado de características a todas las solicitudes.

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="as-number"></a>Número de sistema autónomo (AS)

La red del número de sistema autónomo se define por el número de sistema autónomo (ASN). 

La opción **Matches**/**Does Not Match** (Coincide/No coincide) determina las condiciones para que se cumpla la condición de coincidencia Número de sistema autónomo (AS):

- **Matches** (Coincide): requiere que el ASN de la red del cliente coincida con uno de los ASN especificados. 
- **Does Not Match** (No coincide): requiere que el ASN de la red del cliente no coincida con ninguno de los ASN especificados.

Información importante:

- Especifique varios números de sistema autónomo mediante la delimitación de cada uno con un espacio. Por ejemplo, 64514 64515 coincide con las solicitudes que llegan desde 64514 o 64515.
- Algunas solicitudes no pueden devolver un ASN válido. Un signo de interrogación (?) coincidirá con las solicitudes para las que no se pueda determinar un ASN válido.
- Especifique el ASN completo para la red deseada. No se encontrarán coincidencias con valores parciales.
- Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta condición de coincidencia es incompatible con las siguientes características:
  - Relleno de la memoria caché completa
  - Max-Age interna predeterminada
  - Forzar Max-Age interna
  - Ignorar no almacenar en caché de origen
  - Max-Stale interna

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="cdn-origin"></a>Origen de red CDN

La condición de coincidencia Origen de red CDN se cumple cuando lo hacen ambas condiciones siguientes:

- Se solicitó contenido del almacenamiento de la red CDN.
- El identificador URI de la solicitud usa el tipo de punto de acceso al contenido (por ejemplo, /000001) que se define en esta condición de coincidencia:
  - Dirección URL de la red CDN: el identificador URI de la solicitud debe contener el punto de acceso al contenido seleccionado.
  - Dirección URL de CNAME perimetral: la configuración de CNAME perimetral correspondiente debe apuntar al punto de acceso al contenido seleccionado.
  
Información importante:

- El punto de acceso al contenido identifica el servicio que debe atender el contenido solicitado.
- No use una instrucción AND IF para combinar determinadas condiciones de coincidencia. Por ejemplo, la combinación de una condición de coincidencia Origen de red CDN con una Origen de cliente crearía un patrón de coincidencia que no se puede cumplir. Por esta razón, no se pueden combinar dos condiciones de coincidencia Origen de red CDN mediante una instrucción AND IF.

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="client-ip-address"></a>Dirección IP de cliente

La opción **Matches**/**Does Not Match** (Coincide/No coincide) determina las condiciones para que se cumpla la condición de coincidencia Dirección IP del cliente:

- **Matches** (Coincide): requiere que la dirección IP del cliente coincida con una de las direcciones IP especificadas. 
- **Does Not Match** (No coincide): requiere que la dirección IP del cliente no coincida con ninguna de las direcciones IP especificadas. 

Información importante:

- Utilice la notación CIDR.
- Especifique varias direcciones IP o bloques de direcciones IP al delimitarlas individualmente con un espacio. Por ejemplo:
  - **Ejemplo IPv4**: 1.2.3.4 10.20.30.40 coincide con las solicitudes que llegan desde la dirección 1.2.3.4 o 10.20.30.40.
  - **Ejemplo IPv6**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 coincide con las solicitudes que llegan desde la dirección 1:2:3:4:5:6:7:8 o 10:20:30:40:50:60:70:80.
- La sintaxis de un bloque de direcciones IP es la dirección IP de base seguida por una barra diagonal y el tamaño del prefijo. Por ejemplo:
  - **Ejemplo IPv4**: 5.5.5.64/26 coincide con las solicitudes que llegan desde la dirección 5.5.5.64 hasta la 5.5.5.127.
  - **Ejemplo IPv6**: 1:2:3:/48 coincide con las solicitudes que llegan desde la dirección 1:2:3:0:0:0:0:0 hasta la 1:2:3:ffff:ffff:ffff:ffff:ffff.
- Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta condición de coincidencia es incompatible con las siguientes características:
  - Relleno de la memoria caché completa
  - Max-Age interna predeterminada
  - Forzar Max-Age interna
  - Ignorar no almacenar en caché de origen
  - Max-Stale interna

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="cookie-parameter"></a>Parámetro de cookie

La opción **Matches**/**Does Not Match** (Coincide/No coincide) determina las condiciones para que se cumpla la condición de coincidencia Parámetro de cookie.

- **Matches** (Coincide): requiere que una solicitud contenga la cookie especificada con un valor que coincida con al menos uno de los que se definen en esta condición de coincidencia.
- **Does Not Match** (No coincide): requiere que la solicitud cumpla cualquiera de los siguientes criterios:
  - No contenga la cookie especificada.
  - Contenga la cookie especificada, pero que su valor no coincida con ninguno de los que se definen en esta condición de coincidencia.
  
Información importante:

- Nombre de la cookie:
  - Dado que no se admiten caracteres comodín, incluidos los asteriscos (*), al especificar un nombre de cookie, solo las coincidencias de nombre de cookie exacto son aptas para la comparación.
  - Solo se puede especificar un nombre de cookie por instancia de esta condición de coincidencia.
  - Las comparaciones de nombres de cookie distinguen entre mayúsculas y minúsculas.
- Valor de la cookie:
  - Especifique varios valores de cookie mediante la delimitación de cada uno con un espacio.
  - Un valor de cookie puede aprovechar las ventajas de los [valores de carácter comodín](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - Si no se ha especificado un valor de carácter comodín, solo una coincidencia exacta cumplirá esta condición de coincidencia. Por ejemplo, especificar que "Value" coincida con "Value", pero no "Value1" o "Value2".
  - Utilice la opción **Ignore Case** (No distinguir mayúsculas y minúsculas) para controlar si en la comparación se distinguirán mayúsculas y minúsculas en el valor de la cookie de la solicitud.
- Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta condición de coincidencia es incompatible con las siguientes características:
  - Relleno de la memoria caché completa
  - Max-Age interna predeterminada
  - Forzar Max-Age interna
  - Ignorar no almacenar en caché de origen
  - Max-Stale interna

[Volver arriba](#reference-for-rules-engine-match-conditions)
</br>

---

### <a name="cookie-parameter-regex"></a>Regex de parámetro de cookie

La condición de coincidencia Regex de parámetro de cookie define el valor y el nombre de la cookie. Puede utilizar [expresiones regulares](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) para definir el valor de la cookie deseado.

La opción **Matches**/**Does Not Match** (Coincide/No coincide) determina las condiciones para que se cumpla la condición de coincidencia Regex de parámetro de cookie.

- **Matches** (Coincide): identifica solicitudes que contienen la cookie determinada con un valor que coincida con la expresión regular especificada.
- **Does Not Match** (No coincide): requiere que la solicitud cumpla cualquiera de los siguientes criterios:
  - No contenga la cookie especificada.
  - Contenga la cookie especificada, pero que su valor no coincida con la expresión regular especificada.
  
Información importante:

- Nombre de la cookie:
  - Dado que no se admiten expresiones regulares ni caracteres comodín, incluidos los asteriscos (*), al especificar un nombre de cookie, solo las coincidencias de nombre de cookie exacto son aptas para la comparación.
  - Solo se puede especificar un nombre de cookie por instancia de esta condición de coincidencia.
  - Las comparaciones de nombres de cookie distinguen entre mayúsculas y minúsculas.
- Valor de la cookie:
  - Para los valores de cookie se pueden utilizar expresiones regulares.
  - Utilice la opción **Ignore Case** (No distinguir mayúsculas y minúsculas) para controlar si en la comparación se distinguirán mayúsculas y minúsculas en el valor de la cookie de la solicitud.
- Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta condición de coincidencia es incompatible con las siguientes características:
  - Relleno de la memoria caché completa
  - Max-Age interna predeterminada
  - Forzar Max-Age interna
  - Ignorar no almacenar en caché de origen
  - Max-Stale interna

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="country"></a>Country

Puede especificar un país mediante el código de país. 

La opción **Matches**/**Does Not Match** (Coincide/No coincide) determina las condiciones para que se cumpla la condición de coincidencia País:

- **Matches** (Coincide): requiere que la solicitud contenga los valores de código de país especificados. 
- **Does Not Match** (No coincide): requiere que la solicitud no contenga los valores de código de país especificados.

Información importante:

- Especifique varios códigos de país mediante la delimitación de cada uno con un espacio.
- No se admiten caracteres comodín al especificar un código de país.
- Los códigos de país "EU" y "AP" no incluyen todas las direcciones IP de esas regiones.
- Algunas solicitudes pueden no devolver un código de país válido. Un signo de interrogación (?) coincidirá con las solicitudes para las que no se pueda determinar un código de país válido.
- Los códigos de país distinguen entre mayúsculas y minúsculas.
- Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta condición de coincidencia es incompatible con las siguientes características:
  - Relleno de la memoria caché completa
  - Max-Age interna predeterminada
  - Forzar Max-Age interna
  - Ignorar no almacenar en caché de origen
  - Max-Stale interna

#### <a name="implementing-country-filtering-by-using-the-rules-engine"></a>Implementación del filtrado por país utilizando el motor de reglas

Esta condición de coincidencia permite realizar un gran número de personalizaciones en función de la ubicación desde la que se originó una solicitud. Por ejemplo, el comportamiento de la característica de filtrado por país puede replicarse a través de la configuración siguiente:

- Coincidencia de caracteres comodín de la ruta de URL: establezca la [condición de coincidencia de caracteres comodín de la ruta de URL](#url-path-wildcard) en el directorio que se va a proteger. 
    Anexe un asterisco al final de la ruta de acceso relativa para asegurarse de que el acceso a todos sus elementos secundarios se limitará mediante esta regla.

- Coincidencia de país: establezca la condición de coincidencia de país en el conjunto deseado de países.
  - Permitir: establezca la condición de coincidencia de país en **Does Not Match** (No coincide) para permitir únicamente el acceso de los países especificados al contenido almacenado en la ubicación definida por la condición de coincidencia de caracteres comodín de ruta de URL.
  - Bloquear: establezca la condición de coincidencia de país en **Matches** (Coincide) para bloquear el acceso de los países especificados al contenido almacenado en la ubicación definida por la condición de coincidencia de caracteres comodín de ruta de URL.

- Característica de denegación de acceso (403): habilite la [característica de denegación de acceso (403)](cdn-verizon-premium-rules-engine-reference-features.md#deny-access-403) para replicar la parte permitidos o bloqueados de la característica de filtrado por país.

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="customer-origin"></a>Origen de cliente

Información importante:

- La condición de coincidencia Origen de cliente se cumplirá independientemente de si se solicita contenido mediante una dirección URL de la red CDN o una dirección URL CNAME perimetral que apunte al origen del cliente seleccionado.
- No se puede eliminar de la página Origen de cliente una configuración de origen de cliente a la que haga referencia una regla. Antes de intentar eliminar una configuración de origen de cliente, asegúrese de que las siguientes configuraciones de no hacen referencia a ella:
  - Una condición de coincidencia Origen de cliente
  - Una configuración de CNAME perimetral
- No use una instrucción AND IF para combinar determinadas condiciones de coincidencia. Por ejemplo, la combinación de una condición de coincidencia Origen de cliente con una Origen de red CDN crearía un patrón de coincidencia que no se puede cumplir. Por esta razón, no se pueden combinar dos condiciones de coincidencia Origen de cliente mediante una instrucción AND IF.

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="device"></a>Dispositivo

La condición de coincidencia Dispositivo identifica solicitudes realizadas desde un dispositivo móvil en función de sus propiedades. La detección de dispositivos móviles se realiza a través de [WURFL](http://wurfl.sourceforge.net/). 

La opción **Matches**/**Does Not Match** (Coincide/No coincide) determina las condiciones para que se cumpla la condición de coincidencia Dispositivo:

- **Matches** (Coincide): requiere que el dispositivo del solicitante coincida con el valor especificado. 
- **Does Not Match** (No coincide): requiere que el dispositivo del solicitante no coincida con el valor especificado.

Información importante:

- Use la opción **Ignore Case** (No distinguir mayúsculas y minúsculas) para especificar si el valor especificado distingue mayúsculas de minúsculas.
- Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta condición de coincidencia es incompatible con las siguientes características:
  - Relleno de la memoria caché completa
  - Max-Age interna predeterminada
  - Forzar Max-Age interna
  - Ignorar no almacenar en caché de origen
  - Max-Stale interna

#### <a name="string-type"></a>Tipo de cadena

Normalmente, una funcionalidad WURFL acepta cualquier combinación de números, letras y símbolos. Debido a la naturaleza flexible de esta funcionalidad, debe elegir cómo se interpreta el valor asociado a esta condición de coincidencia. La tabla siguiente describe el conjunto de opciones disponibles:

type     | DESCRIPCIÓN
---------|------------
Literal  | Seleccione esta opción para evitar que la mayoría de los caracteres tengan un significado especial mediante el uso de su [valor literal](cdn-verizon-premium-rules-engine-reference.md#literal-values).
Wildcard (Carácter comodín) | Seleccione esta opción para aprovechar las ventajas de todos los [caracteres comodín] ([valores de caracteres comodín](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
Regex    | Seleccione esta opción para usar [expresiones regulares](cdn-verizon-premium-rules-engine-reference.md#regular-expressions). Las expresiones regulares son útiles para definir un patrón de caracteres.

#### <a name="wurfl-capabilities"></a>Funcionalidades WURFL

Una funcionalidad WURFL hace referencia a una categoría que describe los dispositivos móviles. La funcionalidad seleccionada determina el tipo de descripción de dispositivo móvil que se utiliza para identificar las solicitudes.

En la tabla siguiente se enumeran las funcionalidades WURFL y sus variables para el motor de reglas.

> [!NOTE]
> Las variables siguientes se admiten en las funciones **Modify Client Request Header** (Modificar encabezado de solicitud de cliente) y **Modify Client Response Header** (Modificar encabezado de respuesta de cliente).

Capacidad | Variable | DESCRIPCIÓN | Valores de ejemplo
-----------|----------|-------------|----------------
Nombre de la marca | %{wurfl_cap_brand_name} | Una cadena que indica el nombre de marca del dispositivo. | Samsung
Sistema operativo del dispositivo | %{wurfl_cap_device_os} | Una cadena que indica el sistema operativo instalado en el dispositivo. | IOS
Versión del sistema operativo del dispositivo | %{wurfl_cap_device_os_version} | Una cadena que indica el número de versión del sistema operativo instalado en el dispositivo. | 1.0.1
Orientación dual | %{wurfl_cap_dual_orientation} | Un valor booleano que indica si el dispositivo admite la orientación dual. | true
DTD preferido de HTML | %{wurfl_cap_html_preferred_dtd} | Una cadena que indica la definición de tipo de documento (DTD) preferido del dispositivo móvil para el contenido HTML. | None<br/>xhtml_basic<br/>html5
Imagen de inclusión | %{wurfl_cap_image_inlining} | Un valor booleano que indica si el dispositivo admite imágenes con codificación Base64. | false
Es Android | %{wurfl_vcap_is_android} | Un valor booleano que indica si el dispositivo usa el sistema operativo Android. | true
Es IOS | %{wurfl_vcap_is_ios} | Un valor booleano que indica si el dispositivo usa iOS. | false
Es Smart TV | %{wurfl_cap_is_smarttv} | Un valor booleano que indica si el dispositivo es una Smart TV. | false
Es smartphone | %{wurfl_vcap_is_smartphone} | Un valor booleano que indica si el dispositivo es un smartphone. | true
Es tablet | %{wurfl_cap_is_tablet} | Un valor booleano que indica si el dispositivo es una tablet. Esta descripción es independiente del sistema operativo. | true
Es dispositivo inalámbrico | %{wurfl_cap_is_wireless_device} | Un valor booleano que indica si el dispositivo se considera un dispositivo inalámbrico. | true
Nombre de marca | %{wurfl_cap_marketing_name} | Una cadena que indica el nombre de marca del dispositivo. | BlackBerry 8100 Pearl
Explorador móvil | %{wurfl_cap_mobile_browser} | Una cadena que indica el explorador que se usa para solicitar el contenido del dispositivo. | Chrome
Versión del explorador móvil | %{wurfl_cap_mobile_browser_version} | Una cadena que indica la versión del explorador que se usa para solicitar el contenido del dispositivo. | 31
Nombre del modelo | %{wurfl_cap_model_name} | Una cadena que indica el nombre del modelo del dispositivo. | s3
Descarga progresiva | %{wurfl_cap_progressive_download} | Un valor booleano que indica si el dispositivo admite la reproducción de audio y vídeo durante la descarga. | true
Fecha de lanzamiento | %{wurfl_cap_release_date} | Una cadena que indica el año y el mes en el que se agregó el dispositivo a la base de datos WURFL.<br/><br/>Formato: `yyyy_mm` | 2013_december
Altura de resolución | %{wurfl_cap_resolution_height} | Un entero que indica la altura del dispositivo en píxeles. | 768
Anchura de resolución | %{wurfl_cap_resolution_width} | Un entero que indica la anchura del dispositivo en píxeles. | 1024

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="edge-cname"></a>Cname perimetral

Información importante:

- La lista de CNAME perimetrales disponibles se limita a los que se han configurado en la página de CNAME perimetral que se corresponde con la plataforma donde se configura el motor de reglas.
- Antes de eliminar una configuración de CNAME perimetral, asegúrese de que no hace referencia a ella ninguna condición de coincidencia CNAME perimetral. No se pueden eliminar de la página de CNAME perimetral las configuraciones de CNAME perimetral que se hayan definido en una regla.
- No use una instrucción AND IF para combinar determinadas condiciones de coincidencia. Por ejemplo, la combinación de una condición de coincidencia CNAME perimetral con una Origen de cliente crearía un patrón de coincidencia que no se puede cumplir. Por esta razón, no se pueden combinar dos condiciones de coincidencia CNAME perimetral mediante una instrucción AND IF.
- Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta condición de coincidencia es incompatible con las siguientes características:
  - Relleno de la memoria caché completa
  - Max-Age interna predeterminada
  - Forzar Max-Age interna
  - Ignorar no almacenar en caché de origen
  - Max-Stale interna

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="referring-domain"></a>Dominio de referencia

El nombre de host asociado con el origen de referencia mediante el cual se solicitó contenido determina si se cumple la condición Dominio de referencia.

La opción **Matches**/**Does Not Match** (Coincide/No coincide) determina las condiciones para que se cumpla la condición de coincidencia Dominio de referencia:

- **Matches** (Coincide): requiere que el nombre de host de referencia coincida con el valor especificado. 
- **Does Not Match** (No coincide): requiere que el nombre de host de referencia no coincida con el valor especificado.

Información importante:

- Especifique varios nombres de host mediante la delimitación de cada uno con un espacio.
- Esta condición de coincidencia admite [valores de caracteres comodín](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
- Si el valor especificado no contiene asteriscos, debe ser una coincidencia exacta para el nombre de host del sitio de referencia. Por ejemplo, especificar "mydomain.com" no coincidiría con "www.mydomain.com."
- Utilice la opción **Ignore Case** (No distinguir mayúsculas y minúsculas) para controlar si en la comparación se distinguirán mayúsculas y minúsculas.
- Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta condición de coincidencia es incompatible con las siguientes características:
  - Relleno de la memoria caché completa
  - Max-Age interna predeterminada
  - Forzar Max-Age interna
  - Ignorar no almacenar en caché de origen
  - Max-Stale interna

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---  

### <a name="request-header-literal"></a>Literal de encabezado de solicitud

La opción **Matches**/**Does Not Match** (Coincide/No coincide) determina las condiciones para que se cumpla la condición de coincidencia Literal de encabezado de solicitud.

- **Matches** (Coincide): requiere que la solicitud contenga el encabezado especificado. Su valor debe coincidir con el que se define en esta condición de coincidencia.
- **Does Not Match** (No coincide): requiere que la solicitud cumpla cualquiera de los siguientes criterios:
  - No contenga el encabezado especificado.
  - Contenga el encabezado especificado, pero que su valor no coincida con el que se define en esta condición de coincidencia.
  
Información importante:

- Las comparaciones de nombres de encabezado distinguen siempre entre mayúsculas y minúsculas. Utilice la opción **Ignore Case** (No distinguir mayúsculas y minúsculas) para controlar si las comparaciones de valores de encabezado distinguen las mayúsculas y las minúsculas.
- Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta condición de coincidencia es incompatible con las siguientes características:
  - Relleno de la memoria caché completa
  - Max-Age interna predeterminada
  - Forzar Max-Age interna
  - Ignorar no almacenar en caché de origen
  - Max-Stale interna

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---  

### <a name="request-header-regex"></a>Regex de encabezado de solicitud

La opción **Matches**/**Does Not Match** (Coincide/No coincide) determina las condiciones para que se cumpla la condición de coincidencia Regex de encabezado de solicitud.

- **Matches** (Coincide): requiere que la solicitud contenga el encabezado especificado. Su valor debe coincidir con el patrón que se define en la [expresión regular](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) especificada.
- **Does Not Match** (No coincide): requiere que la solicitud cumpla cualquiera de los siguientes criterios:
  - No contenga el encabezado especificado.
  - Contenga el encabezado especificado, pero que su valor no coincida con la expresión regular especificada.

Información importante:

- Nombre de encabezado:
  - Las comparaciones de nombres de encabezado distinguen entre mayúsculas y minúsculas.
  - Reemplace los espacios en el nombre del encabezado por "%20".
- Valor de encabezado:
  - Para los valores de encabezado se pueden utilizar expresiones regulares.
  - Utilice la opción **Ignore Case** (No distinguir mayúsculas y minúsculas) para controlar si las comparaciones de valores de encabezado distinguen las mayúsculas y las minúsculas.
  - La condición de coincidencia solo se cumple cuando un valor de encabezado coincide exactamente con al menos uno de los patrones especificados.
- Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta condición de coincidencia es incompatible con las siguientes características:
  - Relleno de la memoria caché completa
  - Max-Age interna predeterminada
  - Forzar Max-Age interna
  - Ignorar no almacenar en caché de origen
  - Max-Stale interna

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="request-header-wildcard"></a>Carácter comodín de encabezado de solicitud

La opción **Matches**/**Does Not Match** (Coincide/No coincide) determina las condiciones para que se cumpla la condición de coincidencia Caracteres comodín de encabezado de solicitud.

- **Matches** (Coincide): requiere que la solicitud contenga el encabezado especificado. Su valor debe coincidir con al menos uno de los valores que se definen en esta condición de coincidencia.
- **Does Not Match** (No coincide): requiere que la solicitud cumpla cualquiera de los siguientes criterios:
  - No contenga el encabezado especificado.
  - Contenga el encabezado especificado, pero que su valor no coincida con ninguno de los valores especificados.
  
Información importante:

- Nombre de encabezado:
  - Las comparaciones de nombres de encabezado distinguen entre mayúsculas y minúsculas.
  - Los espacios en el nombre del encabezado deben reemplazarse por "%20". También puede utilizar este valor para especificar espacios en un valor de encabezado.
- Valor de encabezado:
  - Un valor de encabezado puede aprovechar las ventajas de los [valores de carácter comodín](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - Utilice la opción **Ignore Case** (No distinguir mayúsculas y minúsculas) para controlar si las comparaciones de valores de encabezado distinguen las mayúsculas y las minúsculas.
  - La condición de coincidencia se cumple cuando un valor de encabezado coincide exactamente con al menos uno de los patrones especificados.
  - Especifique varios valores de encabezado mediante la delimitación de cada uno con un espacio.
- Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta condición de coincidencia es incompatible con las siguientes características:
  - Relleno de la memoria caché completa
  - Max-Age interna predeterminada
  - Forzar Max-Age interna
  - Ignorar no almacenar en caché de origen
  - Max-Stale interna

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="request-method"></a>Método de solicitud

La condición de coincidencia Método de solicitud se cumple solo cuando los recursos se solicitan mediante el método de solicitud seleccionado. Los métodos de solicitud disponibles son:

- GET
- HEAD
- POST
- OPCIONES
- PUT
- DELETE
- TRACE
- CONNECT

Información importante:

- De forma predeterminada, solo el método de solicitud GET genera contenido almacenado en caché en la red. Los demás métodos de solicitud los procesa el proxy mediante la red.
- Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta condición de coincidencia es incompatible con las siguientes características:
  - Relleno de la memoria caché completa
  - Max-Age interna predeterminada
  - Forzar Max-Age interna
  - Ignorar no almacenar en caché de origen
  - Max-Stale interna

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="request-scheme"></a>Esquema de solicitud

La condición de coincidencia Esquema de solicitud se cumple solo cuando los recursos se solicitan mediante el protocolo seleccionado. Los protocolos disponibles son:

- HTTP
- HTTPS

Información importante:

- Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta condición de coincidencia es incompatible con las siguientes características:
  - Relleno de la memoria caché completa
  - Max-Age interna predeterminada
  - Forzar Max-Age interna
  - Ignorar no almacenar en caché de origen
  - Max-Stale interna

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-directory"></a>Directorio de ruta de acceso URL

Identifica una solicitud por su ruta de acceso relativa, que excluye el nombre de archivo del recurso solicitado.

La opción **Matches**/**Does Not Match** (Coincide/No coincide) determina las condiciones para que se cumpla la condición de coincidencia Directorio de ruta de acceso URL.

- **Matches** (Coincide): requiere que la solicitud contenga una ruta de acceso URL relativa, sin incluir el nombre de archivo, que coincida con el patrón de dirección URL especificado.
- **Does Not Match** (No coincide): requiere que la solicitud contenga una ruta de acceso URL relativa, sin incluir el nombre de archivo, que no coincida con el patrón de dirección URL especificado.

Información importante:

- Use la opción **Relative to** (Relativo a) para especificar si la comparación de la dirección URL debe comenzar antes o después del punto de acceso al contenido. El punto de acceso al contenido es la parte de la ruta de acceso que aparece entre el nombre de host de Verizon CDN y la ruta de acceso relativa al recurso solicitado (por ejemplo, /800001/CustomerOrigin). Identifica una ubicación por tipo de servidor (por ejemplo, red CDN u origen del cliente) y el número de cuenta de cliente.

   Los valores siguientes están disponibles para la opción **Relative to** (Relativo a):
  - **Root** (Raíz): indica que el punto de comparación de la dirección URL comienza directamente después del nombre de host de la red CDN. 

  Por ejemplo: http:\//wpc.0001.&lt;dominio&gt;/**800001/myorigin/myfolder**/index.htm

  - **Origin** (Origen): indica que el punto de comparación de la dirección URL comienza después del punto de acceso al contenido (por ejemplo, /000001 o /800001/myorigin). Dado que el CNAME \*. azureedge.net se crea en relación con el directorio de origen en el nombre de host de Verizon CDN de forma predeterminada, los usuarios de Azure CDN deben utilizar el valor **Origin** (Origen). 

  Por ejemplo: https:\//&lt;punto_de_conexión&gt;.azureedge.net/**myfolder**/index.htm 

  Esta dirección URL señala al nombre de host de Verizon CDN siguiente: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder**/index.htm

- Se vuelve a escribir un dirección URL de CNAME perimetral a una dirección URL de la red CDN antes de la comparación de la dirección URL.

    Por ejemplo, las siguientes direcciones URL apuntan al mismo recurso y, por tanto, tienen la misma ruta de acceso URL.
  - Dirección URL de la red CDN: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
    
  - Dirección URL del CNAME perimetral: http:\//&lt;punto_de_conexión&gt;.azureedge.net/path/asset.htm
    
    Información adicional:
  - Dominio personalizado: https:\//my.domain.com/path/asset.htm
    
    - Ruta de acceso de dirección URL (relativa a la raíz): / 800001/CustomerOrigin/path/
    
    - Ruta de acceso de dirección URL (relativa al origen): /path/

- La parte de la dirección URL que se utiliza para la comparación de la dirección URL termina justo antes del nombre de archivo del recurso solicitado. Una barra diagonal final es el último carácter de este tipo de ruta de acceso.

- Reemplace los espacios en el patrón de la ruta de acceso de dirección URL por "%20".

- Cada patrón de ruta de acceso de dirección URL puede contener uno o varios asteriscos (*), donde cada asterisco coincide con una secuencia de uno o más caracteres.

- Especifique varias rutas de acceso URL en el patrón mediante la delimitación de cada una con un espacio.

    Por ejemplo: */sales/ */marketing/

- Una especificación de ruta de acceso de dirección URL puede aprovechar las ventajas de los [valores de caracteres comodín](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).

- Utilice la opción **Ignore Case** (No distinguir mayúsculas y minúsculas) para controlar si en la comparación se distinguirán mayúsculas y minúsculas.

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-extension"></a>Extensión de ruta de acceso URL

Identifica las solicitudes por la extensión de archivo del recurso solicitado.

La opción **Matches**/**Does Not Match** (Coincide/No coincide) determina las condiciones para que se cumpla la condición de coincidencia Extensión de ruta de acceso URL.

- **Matches** (Coincide): requiere que la dirección URL de la solicitud contenga una extensión de archivo que coincida exactamente con el patrón especificado.

   Por ejemplo, si especifica "htm", coinciden los recursos "htm" pero no los recursos "html".  

- **Does Not Match** (No coincide): requiere que la dirección URL de la solicitud contenga una extensión de archivo que no coincida con el patrón especificado.

Información importante:

- Debe especificar las extensiones de archivo para que coincidan con el cuadro **Valor**. No incluya un punto inicial; por ejemplo, utilice htm en lugar de .htm.

- Utilice la opción **Ignore Case** (No distinguir mayúsculas y minúsculas) para controlar si en la comparación se distinguirán mayúsculas y minúsculas.

- Especifique varias extensiones de archivo delimitando cada extensión con un único espacio. 

    Por ejemplo: htm html

- Por ejemplo, la especificación "htm" coincide con los recursos "htm" pero no con los recursos "html".

#### <a name="sample-scenario"></a>Escenario de ejemplo

En la configuración del ejemplo siguiente se da por supuesto que esta condición de coincidencia se cumple cuando una solicitud coincide con una de las extensiones especificadas.

Especificación de valor: asp aspx php html

Esta condición de coincidencia se cumple cuando se encuentran direcciones URL que terminan con las siguientes extensiones:

- .asp
- .aspx
- .php
- .html

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-filename"></a>Nombre de archivo de ruta de acceso URL

Identifica las solicitudes por el nombre de archivo del recurso solicitado. A efectos de esta condición de coincidencia, un nombre de archivo consta del nombre del recurso solicitado, un punto y la extensión de archivo (por ejemplo, index.html).

La opción **Matches**/**Does Not Match** (Coincide/No coincide) determina las condiciones para que se cumpla la condición de coincidencia Nombre de archivo de ruta de acceso URL.

- **Matches** (Coincide): requiere que la solicitud contenga un nombre de archivo en la ruta de acceso URL que coincida con el patrón especificado.
- **Does Not Match** (No coincide): requiere que la solicitud contenga un nombre de archivo en la ruta de acceso URL que no coincida con el patrón especificado.

Información importante:

- Utilice la opción **Ignore Case** (No distinguir mayúsculas y minúsculas) para controlar si en la comparación se distinguirán mayúsculas y minúsculas.

- Puede especificar varias extensiones de archivo delimitando cada extensión con un único espacio.

    Por ejemplo: index.htm index.html

- Reemplace los espacios en un valor de nombre de archivo por "%20".

- Un valor de nombre de archivo puede aprovechar las ventajas de los [valores de carácter comodín](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). Por ejemplo, cada patrón de nombre de archivo puede contener uno o varios asteriscos (*), donde cada asterisco coincide con una secuencia de uno o más caracteres.

- Si no se ha especificado ningún carácter comodín, solo una coincidencia exacta cumplirá esta condición de coincidencia.

    Por ejemplo, especificar "presentation.ppt" coincide con un recurso llamado "presentation.ppt", pero no uno con nombre "presentation.pptx."

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-literal"></a>Literal de ruta de acceso URL

Compara la ruta de acceso URL de la solicitud, incluido el nombre de archivo, con el valor especificado.

La opción **Matches**/**Does Not Match** (Coincide/No coincide) determina las condiciones para que se cumpla la condición de coincidencia Literal de ruta de acceso URL.

- **Matches** (Coincide): requiere que la solicitud contenga una ruta de acceso URL que coincida con el patrón especificado.
- **Does Not Match** (No coincide): requiere que la solicitud contenga una ruta de acceso URL que no coincida con el patrón especificado.

Información importante:

- Use la opción **Relative to** (Relativo a) para especificar si la comparación de la dirección URL debe comenzar antes o después del punto de acceso al contenido. 

    Los valores siguientes están disponibles para la opción **Relative to** (Relativo a):
  - **Root** (Raíz): indica que el punto de comparación de la dirección URL comienza directamente después del nombre de host de la red CDN.

    Por ejemplo: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder/index.htm**

  - **Origin** (Origen): indica que el punto de comparación de la dirección URL comienza después del punto de acceso al contenido (por ejemplo, /000001 o /800001/myorigin). Dado que el CNAME \*. azureedge.net se crea en relación con el directorio de origen en el nombre de host de Verizon CDN de forma predeterminada, los usuarios de Azure CDN deben utilizar el valor **Origin** (Origen). 

    Por ejemplo: https:\//&lt;punto_de_conexión&gt;.azureedge.net/**myfolder/index.htm**

  Esta dirección URL señala al nombre de host de Verizon CDN siguiente: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder/index.htm**

- Se vuelve a escribir una dirección URL de CNAME perimetral a una dirección URL de la red CDN antes de la comparación de la dirección URL.

Por ejemplo, las siguientes direcciones URL apuntan al mismo recurso y, por tanto, tienen la misma ruta de acceso URL:

- Dirección URL de la red CDN: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
- Dirección URL del CNAME perimetral: http:\//&lt;punto_de_conexión&gt;.azureedge.net/path/asset.htm

    Información adicional:
    
    - Ruta de acceso URL (relativa a la raíz): /800001/CustomerOrigin/path/asset.htm
   
    - Ruta de acceso URL (relativa al origen): /path/asset.htm

- Se omiten las cadenas de consulta en la dirección URL.
- Utilice la opción **Ignore Case** (No distinguir mayúsculas y minúsculas) para controlar si en la comparación se distinguirán mayúsculas y minúsculas.
- El valor especificado para esta condición de coincidencia se compara con la ruta de acceso relativa de la solicitud exacta realizada por el cliente.

- Para hacer coincidir todas las solicitudes realizadas a un directorio determinado, utilice la condición de coincidencia [directorio de la ruta de acceso URL](#url-path-directory) o [carácter comodín de la ruta de acceso URL](#url-path-wildcard).

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-regex"></a>Regex de ruta de acceso URL

Compara la ruta de acceso de una solicitud con la [expresión regular](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) especificada.

La opción **Matches**/**Does Not Match** (Coincide/No coincide) determina las condiciones para que se cumpla la condición de coincidencia Regex de ruta de acceso URL.

- **Matches** (Coincide): requiere que la solicitud contenga una ruta de acceso URL que coincida con la expresión regular especificada.
- **Does Not Match** (No coincide): requiere que la solicitud contenga una ruta de acceso URL que no coincida con la expresión regular especificada.

Información importante:

- Se vuelve a escribir una dirección URL de CNAME perimetral a una dirección URL de la red CDN antes de la comparación de la dirección URL.

    Por ejemplo, las direcciones URL apuntan al mismo recurso y, por tanto, tienen la misma ruta de acceso URL.

     - Dirección URL de la red CDN: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm

     - Dirección URL de CNAME perimetral: http:\//my.domain.com/path/asset.htm

    Información adicional:
    
     - Ruta de acceso URL: /800001/CustomerOrigin/path/asset.htm

- Se omiten las cadenas de consulta en la dirección URL.
    
- Utilice la opción **Ignore Case** (No distinguir mayúsculas y minúsculas) para controlar si en la comparación se distinguirán mayúsculas y minúsculas.
    
- Los espacios en la ruta de acceso URL deben reemplazarse por "%20".

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-path-wildcard"></a>Carácter comodín de ruta de acceso URL

Compara la ruta de acceso URL relativa de una solicitud con el patrón de caracteres comodín especificado.

La opción **Matches**/**Does Not Match** (Coincide/No coincide) determina las condiciones para que se cumpla la condición de coincidencia Caracteres comodín de ruta de acceso URL.

- **Matches** (Coincide): requiere que la solicitud contenga una ruta de acceso URL que coincida con el patrón de caracteres comodín especificado.
- **Does Not Match** (No coincide): requiere que la solicitud contenga una ruta de acceso URL que no coincida con el patrón de caracteres comodín especificado.

Información importante:

- **Relative to** (Relativo a): esta opción determina si la comparación de la dirección URL debe comenzar antes o después del punto de acceso al contenido.

   Esta opción puede tener los valores siguientes:
     - **Root** (Raíz): indica que el punto de comparación de la dirección URL comienza directamente después del nombre de host de la red CDN.

       Por ejemplo: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder/index.htm**

     - **Origin** (Origen): indica que el punto de comparación de la dirección URL comienza después del punto de acceso al contenido (por ejemplo, /000001 o /800001/myorigin). Dado que el CNAME \*. azureedge.net se crea en relación con el directorio de origen en el nombre de host de Verizon CDN de forma predeterminada, los usuarios de Azure CDN deben utilizar el valor **Origin** (Origen). 

       Por ejemplo: https:\//&lt;punto_de_conexión&gt;.azureedge.net/**myfolder/index.htm**

     Esta dirección URL señala al nombre de host de Verizon CDN siguiente: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder/index.htm**

- Se vuelve a escribir una dirección URL de CNAME perimetral a una dirección URL de la red CDN antes de la comparación de la dirección URL.

    Por ejemplo, las siguientes direcciones URL apuntan al mismo recurso y, por tanto, tienen la misma ruta de acceso URL:
     - Dirección URL de la red CDN: http://wpc.0001.&lt ;dominio&gt; /800001/CustomerOrigin/path/asset.htm
     - Dirección URL del CNAME perimetral: http:\//&lt;punto_de_conexión&gt;.azureedge.net/path/asset.htm
    
    Información adicional:
    
     - Ruta de acceso URL (relativa a la raíz): /800001/CustomerOrigin/path/asset.htm
    
     - Ruta de acceso URL (relativa al origen): /path/asset.htm
    
- Especifique varias rutas de acceso URL mediante la delimitación de cada una con un espacio único.

   Por ejemplo: /marketing/asset.* /sales/\*.htm

- Se omiten las cadenas de consulta en la dirección URL.
    
- Utilice la opción **Ignore Case** (No distinguir mayúsculas y minúsculas) para controlar si en la comparación se distinguirán mayúsculas y minúsculas.
    
- Reemplace los espacios en la ruta de acceso URL por "%20".
    
- El valor especificado para una ruta de acceso URL puede aprovechar las ventajas de los [valores de caracteres comodín](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). Cada patrón de ruta de acceso URL puede contener uno o varios asteriscos (*), donde cada asterisco coincide con una secuencia de uno o más caracteres.

#### <a name="sample-scenarios"></a>Escenarios de ejemplo

En las configuraciones de ejemplo de la tabla siguiente se da por supuesto que esta condición de coincidencia se cumple cuando una solicitud coincide con una de los patrones de URL especificados:

Valor                   | Relativo a    | Resultado 
------------------------|----------------|-------
\*/test.html \*/test.php  | Raíz u origen | Este patrón coincide con solicitudes de recursos llamados "test.html" o "test.php" en cualquier carpeta.
/80ABCD/origin/text/*   | Raíz           | Este patrón coincide cuando el recurso solicitado cumple los criterios siguientes: <br />- Debe residir en un origen de cliente que se llama "origin". <br />- La ruta de acceso relativa debe comenzar con una carpeta llamada "text". Es decir, el recurso solicitado puede residir en la carpeta "text" o una de sus subcarpetas recursivas.
*/css/* */js/*          | Raíz u origen | Este patrón coincide con todas las direcciones URL de CNAME perimetral o de la red CDN que contengan una carpeta llamada css o js.
*.jpg *.gif *.png       | Raíz u origen | Este patrón coincide con todas las direcciones URL de CNAME perimetral o de la red CDN que terminen con .jpg, .gif o .png. Una manera alternativa para especificar este patrón es con la [condición de coincidencia Extensión de ruta de acceso URL](#url-path-extension).
/images/\* /media/\*      | Origen         | Este patrón coincide con las direcciones URL de CNAME perimetral o de la red CDN cuya ruta de acceso relativa comienza por una carpeta "images" o "media". <br />- Dirección URL de la red CDN: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/images/sales/event1.png<br />- Dirección URL de CNAME perimetral de ejemplo: http:\//cdn.mydomain.com/images/sales/event1.png

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-query-literal"></a>Literal de consulta de dirección URL

Compara la cadena de consulta de una solicitud con el valor especificado.

La opción **Matches**/**Does Not Match** (Coincide/No coincide) determina las condiciones para que se cumpla la condición de coincidencia Literal de la consulta URL.

- **Matches** (Coincide): requiere que la solicitud contenga una cadena de consulta URL que coincida con la cadena de consulta especificada.
- **Does Not Match** (No coincide): requiere que la solicitud contenga una cadena de consulta URL que no coincida con la cadena de consulta especificada.

Información importante:

- Solo las coincidencias de cadenas de consulta exactas satisfacen esta condición de coincidencia.
    
- Utilice la opción **Ignore Case** (No distinguir mayúsculas y minúsculas) para controlar si las comparaciones de cadena de consulta distinguen las mayúsculas y las minúsculas.
    
- No incluya un signo de interrogación (?) inicial en el texto del valor de la cadena de consulta.
    
- Ciertos caracteres requieren codificación URL. Utilice el símbolo de porcentaje para codificar en formato URL los caracteres siguientes:

   Character | Codificación URL
   ----------|---------
   Espacio     | %20
   &         | %25

- Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta condición de coincidencia es incompatible con las siguientes características:
   - Relleno de la memoria caché completa
   - Max-Age interna predeterminada
   - Forzar Max-Age interna
   - Ignorar no almacenar en caché de origen
   - Max-Stale interna

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-query-parameter"></a>Parámetro de consulta de dirección URL

Identifica las solicitudes que contienen el parámetro de cadena de consulta especificado. Este parámetro se establece en un valor que coincide con un patrón especificado. Los parámetros de cadena de consulta (por ejemplo, parameter=value) en la dirección URL de la solicitud determinan si se cumple esta condición. Esta condición de coincidencia identifica un parámetro de cadena de consulta por su nombre y acepta uno o varios valores para el valor del parámetro. 

La opción **Matches**/**Does Not Match** (Coincide/No coincide) determina las condiciones para que se cumpla la condición de coincidencia Parámetro de consulta URL.

- **Matches** (Coincide): requiere que una solicitud contenga el parámetro especificado con un valor que coincida con al menos uno de los que se definen en esta condición de coincidencia.
- **Does Not Match** (No coincide): requiere que la solicitud cumpla cualquiera de los siguientes criterios:
  - No contiene el parámetro especificado.
  - Contiene el parámetro especificado, pero su valor no coincide con ninguno de los que se definen en esta condición de coincidencia.

Esta condición de coincidencia proporciona una manera sencilla para especificar las combinaciones de nombre/valor de los parámetros. Para obtener mayor flexibilidad en las coincidencias de parámetros de cadena de consulta, considere el uso de la condición de coincidencia [Carácter comodín de la consulta URL](#url-query-wildcard) coincide con la condición.

Información importante:

- Solo se puede especificar un nombre de parámetro de la cadena de consulta por instancia de esta condición de coincidencia.
    
- Dado que no se admiten valores de caracteres comodín cuando se especifica un nombre de parámetro, solo las coincidencias de nombre de parámetro exactas son aptas para la comparación.
- Los valores de los parámetros pueden incluir [valores de caracteres comodín](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
   - Cada patrón de valor de parámetro puede contener uno o varios asteriscos (*), donde cada asterisco coincide con una secuencia de uno o más caracteres.
   - Ciertos caracteres requieren codificación URL. Utilice el símbolo de porcentaje para codificar en formato URL los caracteres siguientes:

       Character | Codificación URL
       ----------|---------
       Espacio     | %20
       &         | %25

- Especifique varios valores de parámetro de la cadena de consulta mediante la delimitación de cada uno con un espacio. Esta condición de coincidencia se cumple cuando una solicitud contiene una de las combinaciones de nombre/valor especificadas.

   - Ejemplo 1:

     - Configuración:

       ValueA ValueB

     - Esta configuración coincide con los siguientes parámetros de la cadena de consulta:

       Parameter1=ValueA
    
       Parameter1=ValueA

   - Ejemplo 2:

     - Configuración: 

        Value%20A Value%20B

     - Esta configuración coincide con los siguientes parámetros de la cadena de consulta:

       Parameter1=Value%20A

       Parameter1=Value%20B

- Esta condición de coincidencia se cumple solo cuando hay una coincidencia exacta con al menos una de las combinaciones de nombre/valor de la cadena de consulta especificada.

   Por ejemplo, si se usa la configuración del ejemplo anterior, la combinación de nombre/valor del parámetro "Parameter1=ValueAdd" no se consideraría una coincidencia. Sin embargo, si se especifica cualquiera de los valores siguientes, coincidirá con esa combinación de nombre/valor:

   - ValueA ValueB ValueAdd
   - ValueA* ValueB

- Utilice la opción **Ignore Case** (No distinguir mayúsculas y minúsculas) para controlar si las comparaciones de cadena de consulta distinguen las mayúsculas y las minúsculas.
    
- Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta condición de coincidencia es incompatible con las siguientes características:
   - Relleno de la memoria caché completa
   - Max-Age interna predeterminada
   - Forzar Max-Age interna
   - Ignorar no almacenar en caché de origen
   - Max-Stale interna

#### <a name="sample-scenarios"></a>Escenarios de ejemplo

En el ejemplo siguiente se muestra cómo funciona esta opción en situaciones específicas:

NOMBRE  | Valor |  Resultado
------|-------|--------
Usuario  | Joe   | Este patrón coincide cuando la cadena de consulta para una dirección URL solicitada es "?user=joe".
Usuario  | *     | Este patrón coincide cuando la cadena de consulta para una dirección URL solicitada contiene el parámetro User.
Email | Joe\* | Este patrón coincide cuando la cadena de consulta para una dirección URL solicitada contiene un parámetro Email que comienza por "Joe".

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-query-regex"></a>Regex de consulta de dirección URL

Identifica las solicitudes que contienen el parámetro de cadena de consulta especificado. Este parámetro se establece en un valor que coincide con una [expresión regular](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) especificada.

La opción **Matches**/**Does Not Match** (Coincide/No coincide) determina las condiciones para que se cumpla la condición de coincidencia Regex de consulta URL.

- **Matches** (Coincide): requiere que la solicitud contenga una cadena de consulta URL que coincida con la expresión regular especificada.
- **Does Not Match** (No coincide): requiere que la solicitud contenga una cadena de consulta URL que no coincida con la expresión regular especificada.

Información importante:

- Solo las coincidencias exactas con la expresión regular especificada satisfacen esta condición de coincidencia.
    
- Utilice la opción **Ignore Case** (No distinguir mayúsculas y minúsculas) para controlar si las comparaciones de cadena de consulta distinguen las mayúsculas y las minúsculas.
    
- A los efectos de esta opción, una cadena de consulta comienza con el primer carácter después del delimitador de signo de interrogación (?) de la cadena de consulta.
    
- Ciertos caracteres requieren codificación URL. Utilice el símbolo de porcentaje para codificar en formato URL los caracteres siguientes:

   Character | Codificación URL | Valor
   ----------|--------------|------
   Espacio     | %20          | \%20
   &         | %25          | \%25

   Note that percentage symbols must be escaped.

- Double-escape special regular expression characters (for example, \^$.+) to include a backslash in the regular expression.

   For example:

   Value | Interpreted As 
   ------|---------------
   \\+    | +
   \\\\+   | \\+

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Wildcard

Compares the specified value(s) against the request's query string.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Wildcard match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified wildcard value.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified wildcard value.

Key information:

- For the purposes of this option, a query string starts with the first character after the question mark (?) delimiter for the query string.
- Parameter values can include [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values):
   - Each parameter value pattern can consist of one or more asterisks (*), where each asterisk can match a sequence of one or more characters.
   - Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

     Character | URL Encoding
     ----------|---------
     Space     | %20
     &         | %25

- Specify multiple values by delimiting each one with a single space.

   For example: *Parameter1=ValueA* *ValueB* *Parameter1=ValueC&Parameter2=ValueD*

- Only exact matches to at least one of the specified query string patterns satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

#### Sample scenarios

The following example demonstrates how this option works in specific situations:

 Name                 | Description
 ---------------------|------------
user=joe              | This pattern is matched when the query string for a requested URL is "?user=joe."
\*user=\* \*optout=\* | This pattern is matched when the CDN URL query contains either the user or optout parameter.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

## Next steps

- [Azure Content Delivery Network overview](cdn-overview.md)
- [Rules engine reference](cdn-verizon-premium-rules-engine-reference.md)
- [Rules engine conditional expressions](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Rules engine features](cdn-verizon-premium-rules-engine-reference-features.md)
- [Overriding default HTTP behavior using the rules engine](cdn-verizon-premium-rules-engine.md)\`20
title: Azure CDN from Verizon Premium rules engine match conditions | Microsoft Docs
description: Reference documentation for Azure Content Delivery Network from Verizon Premium rules engine match conditions.
services: cdn
author: mdgattuso

ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus

---

# Azure CDN from Verizon Premium rules engine match conditions

This article lists detailed descriptions of the available match conditions for the Azure Content Delivery Network (CDN) from Verizon Premium [rules engine](cdn-verizon-premium-rules-engine.md).

The second part of a rule is the match condition. A match condition identifies specific types of requests for which a set of features will be performed.

For example, you can use a match condition to:

- Filter requests for content at a particular location.
- Filter requests generated from a particular IP address or country/region.
- Filter requests by header information.

## Always match condition

The Always match condition applies a default set of features to all requests.

Name | Purpose
-----|--------
[Always](#always) | Applies a default set of features to all requests.

## Device match condition

The Device match condition identifies requests made from a mobile device based on its properties.  

Name | Purpose
-----|--------
[Device](#device) | Identifies requests made from a mobile device based on its properties.

## Location match conditions

The Location match conditions identify requests based on the requester's location.

Name | Purpose
-----|--------
[AS Number](#as-number) | Identifies requests that originate from a particular network.
[Country](#country) | Identifies requests that originate from the specified countries/regions.

## Origin match conditions

The Origin match conditions identify requests that point to Content Delivery Network storage or a customer origin server.

Name | Purpose
-----|--------
[CDN Origin](#cdn-origin) | Identifies requests for content stored in Content Delivery Network storage.
[Customer Origin](#customer-origin) | Identifies requests for content stored on a specific customer origin server.

## Request match conditions

The Request match conditions identify requests based on their properties.

Name | Purpose
-----|--------
[Client IP Address](#client-ip-address) | Identifies requests that originate from a particular IP address.
[Cookie Parameter](#cookie-parameter) | Checks the cookies associated with each request for the specified value.
[Cookie Parameter Regex](#cookie-parameter-regex) | Checks the cookies associated with each request for the specified regular expression.
[Edge Cname](#edge-cname) | Identifies requests that point to a specific edge CNAME.
[Referring Domain](#referring-domain) | Identifies requests that were referred from the specified host names.
[Request Header Literal](#request-header-literal) | Identifies requests that contain the specified header set to a specified value.
[Request Header Regex](#request-header-regex) | Identifies requests that contain the specified header set to a value that matches the specified regular expression.
[Request Header Wildcard](#request-header-wildcard) | Identifies requests that contain the specified header set to a value that matches the specified pattern.
[Request Method](#request-method) | Identifies requests by their HTTP method.
[Request Scheme](#request-scheme) | Identifies requests by their HTTP protocol.

## URL match conditions

The URL match conditions identify requests based on their URLs.

Name | Purpose
-----|--------
[URL Path Directory](#url-path-directory) | Identifies requests by their relative path.
[URL Path Extension](#url-path-extension) | Identifies requests by their file name extension.
[URL Path Filename](#url-path-filename) | Identifies requests by their file name.
[URL Path Literal](#url-path-literal) | Compares a request's relative path to the specified value.
[URL Path Regex](#url-path-regex) | Compares a request's relative path to the specified regular expression.
[URL Path Wildcard](#url-path-wildcard) | Compares a request's relative path to the specified pattern.
[URL Query Literal](#url-query-literal) | Compares a request's query string to the specified value.
[URL Query Parameter](#url-query-parameter) | Identifies requests that contain the specified query string parameter set to a value that matches a specified pattern.
[URL Query Regex](#url-query-regex) | Identifies requests that contain the specified query string parameter set to a value that matches a specified regular expression.
[URL Query Wildcard](#url-query-wildcard) | Compares the specified value against the request's query string.

## Reference for rules engine match conditions

---

### Always

The Always match condition applies a default set of features to all requests.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### AS Number

The AS Number network is defined by its autonomous system number (ASN). 

The **Matches**/**Does Not Match** option determines the conditions under which the AS Number match condition is met:

- **Matches**: Requires that the ASN of the client network matches one of the specified ASNs. 
- **Does Not Match**: Requires that the ASN of the client network does not match any of the specified ASNs.

Key information:

- Specify multiple ASNs by delimiting each one with a single space. For example, 64514 64515 matches requests that arrive from either 64514 or 64515.
- Certain requests might not return a valid ASN. A question mark (?) will match requests for which a valid ASN could not be determined.
- Specify the entire ASN for the desired network. Partial values will not be matched.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### CDN Origin

The CDN Origin match condition is met when both of the following conditions are met:

- Content from CDN storage was requested.
- The request URI uses the type of content access point (for example, /000001) that's defined in this match condition:
  - CDN URL: The request URI must contain the selected content access point.
  - Edge CNAME URL: The corresponding edge CNAME configuration must point to the selected content access point.
  
Key information:

- The content access point identifies the service that should serve the requested content.
- Don't use an AND IF statement to combine certain match conditions. For example, combining a CDN Origin match condition with a Customer Origin match condition would create a match pattern that could never be matched. For this reason, two CDN Origin match conditions cannot be combined through an AND IF statement.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Client IP Address

The **Matches**/**Does Not Match** option determines the conditions under which the Client IP Address match condition is met:

- **Matches**: Requires that the client's IP address matches one of the specified IP addresses. 
- **Does Not Match**: Requires that the client's IP address does not match any of the specified IP addresses. 

Key information:

- Use CIDR notation.
- Specify multiple IP addresses and/or IP address blocks by delimiting each one with a single space. For example:
  - **IPv4 example**: 1.2.3.4 10.20.30.40 matches any requests that arrive from either address 1.2.3.4 or 10.20.30.40.
  - **IPv6 example**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 matches any requests that arrive from either address 1:2:3:4:5:6:7:8 or 10:20:30:40:50:60:70:80.
- The syntax for an IP address block is the base IP address followed by a forward slash and the prefix size. For example:
  - **IPv4 example**: 5.5.5.64/26 matches any requests that arrive from addresses 5.5.5.64 through 5.5.5.127.
  - **IPv6 example**: 1:2:3:/48 matches any requests that arrive from addresses 1:2:3:0:0:0:0:0 through 1:2:3:ffff:ffff:ffff:ffff:ffff.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Cookie Parameter

The **Matches**/**Does Not Match** option determines the conditions under which the Cookie Parameter match condition is met.

- **Matches**: Requires a request to contain the specified cookie with a value that matches at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified cookie.
  - It contains the specified cookie, but its value does not match any of the values that are defined in this match condition.
  
Key information:

- Cookie name:
  - Because wildcard values, including asterisks (*), are not supported when you're specifying a cookie name, only exact cookie name matches are eligible for comparison.
  - Only a single cookie name can be specified per instance of this match condition.
  - Cookie name comparisons are case-insensitive.
- Cookie value:
  - Specify multiple cookie values by delimiting each one with a single space.
  - A cookie value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - If a wildcard value has not been specified, then only an exact match will satisfy this match condition. For example, specifying "Value" will match "Value," but not "Value1" or "Value2."
  - Use the **Ignore Case** option to control whether a case-sensitive comparison is made against the request's cookie value.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)
</br>

---

### Cookie Parameter Regex

The Cookie Parameter Regex match condition defines a cookie name and value. You can use [regular expressions](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) to define the desired cookie value.

The **Matches**/**Does Not Match** option determines the conditions under which the Cookie Parameter Regex match condition is met.

- **Matches**: Requires a request to contain the specified cookie with a value that matches the specified regular expression.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified cookie.
  - It contains the specified cookie, but its value does not match the specified regular expression.
  
Key information:

- Cookie name:
  - Because regular expressions and wildcard values, including asterisks (*), are not supported when you're specifying a cookie name, only exact cookie name matches are eligible for comparison.
  - Only a single cookie name can be specified per instance of this match condition.
  - Cookie name comparisons are case-insensitive.
- Cookie value:
  - A cookie value can take advantage of regular expressions.
  - Use the **Ignore Case** option to control whether a case-sensitive comparison is made against the request's cookie value.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Country

You can specify a country through its country code. 

The **Matches**/**Does Not Match** option determines the conditions under which the Country match condition is met:

- **Matches**: Requires the request to contain the specified country code values. 
- **Does Not Match**: Requires that the request does not contain the specified country code values.

Key information:

- Specify multiple country codes by delimiting each one with a single space.
- Wildcards are not supported when you're specifying a country code.
- The "EU" and "AP" country codes do not encompass all IP addresses in those regions.
- Certain requests might not return a valid country code. A question mark (?) will match requests for which a valid country code could not be determined.
- Country codes are case-sensitive.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

#### Implementing Country Filtering by using the rules engine

This match condition allows you to perform a multitude of customizations based on the location from which a request originated. For example, the behavior of the Country Filtering feature can be replicated through the following configuration:

- URL Path Wildcard match: Set the [URL Path Wildcard match condition](#url-path-wildcard) to the directory that will be secured. 
    Append an asterisk to the end of the relative path to ensure that access to all of its children will be restricted by this rule.

- Country match: Set the Country match condition to the desired set of countries.
  - Allow: Set the Country match condition to **Does Not Match** to allow only the specified countries access to content stored in the location defined by the URL Path Wildcard match condition.
  - Block: Set the Country match condition to **Matches** to block the specified countries from accessing content stored in the location defined by the URL Path Wildcard match condition.

- Deny Access (403) Feature: Enable the [Deny Access (403) feature](cdn-verizon-premium-rules-engine-reference-features.md#deny-access-403) to replicate the allow or block portion of the Country Filtering feature.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Customer Origin

Key information:

- The Customer Origin match condition is met regardless of whether content is requested through a CDN URL or an edge CNAME URL that points to the selected customer origin.
- A customer origin configuration that's referenced by a rule cannot be deleted from the Customer Origin page. Before you attempt to delete a customer origin configuration, make sure that the following configurations do not reference it:
  - A Customer Origin match condition
  - An edge CNAME configuration
- Don't use an AND IF statement to combine certain match conditions. For example, combining a Customer Origin match condition with a CDN Origin match condition would create a match pattern that could never be matched. For this reason, two Customer Origin match conditions cannot be combined through an AND IF statement.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Device

The Device match condition identifies requests made from a mobile device based on its properties. Mobile device detection is achieved through [WURFL](http://wurfl.sourceforge.net/). 

The **Matches**/**Does Not Match** option determines the conditions under which the Device match condition is met:

- **Matches**: Requires the requester's device to match the specified value. 
- **Does Not Match**: Requires that the requester's device does not match the specified value.

Key information:

- Use the **Ignore Case** option to specify whether the specified value is case-sensitive.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

#### String Type

A WURFL capability typically accepts any combination of numbers, letters, and symbols. Due to the flexible nature of this capability, you must choose how the value associated with this match condition is interpreted. The following table describes the available set of options:

Type     | Description
---------|------------
Literal  | Select this option to prevent most characters from taking on special meaning by using their [literal value](cdn-verizon-premium-rules-engine-reference.md#literal-values).
Wildcard | Select this option to take advantage of all [wildcard characters]([wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
Regex    | Select this option to use [regular expressions](cdn-verizon-premium-rules-engine-reference.md#regular-expressions). Regular expressions are useful for defining a pattern of characters.

#### WURFL capabilities

A WURFL capability refers to a category that describes mobile devices. The selected capability determines the type of mobile device description that is used to identify requests.

The following table lists WURFL capabilities and their variables for the rules engine.

> [!NOTE]
> The following variables are supported in the **Modify Client Request Header** and **Modify Client Response Header** features.

Capability | Variable | Description | Sample values
-----------|----------|-------------|----------------
Brand Name | %{wurfl_cap_brand_name} | A string that indicates the brand name of the device. | Samsung
Device OS | %{wurfl_cap_device_os} | A string that indicates the operating system installed on the device. | IOS
Device OS Version | %{wurfl_cap_device_os_version} | A string that indicates the version number of the operating system installed on the device. | 1.0.1
Dual Orientation | %{wurfl_cap_dual_orientation} | A Boolean that indicates whether the device supports dual orientation. | true
HTML Preferred DTD | %{wurfl_cap_html_preferred_dtd} | A string that indicates the mobile device's preferred document type definition (DTD) for HTML content. | none<br/>xhtml_basic<br/>html5
Image Inlining | %{wurfl_cap_image_inlining} | A Boolean that indicates whether the device supports Base64 encoded images. | false
Is Android | %{wurfl_vcap_is_android} | A Boolean that indicates whether the device uses the Android OS. | true
Is IOS | %{wurfl_vcap_is_ios} | A Boolean that indicates whether the device uses iOS. | false
Is Smart TV | %{wurfl_cap_is_smarttv} | A Boolean that indicates whether the device is a smart TV. | false
Is Smartphone | %{wurfl_vcap_is_smartphone} | A Boolean that indicates whether the device is a smartphone. | true
Is Tablet | %{wurfl_cap_is_tablet} | A Boolean that indicates whether the device is a tablet. This description is  OS-independent. | true
Is Wireless Device | %{wurfl_cap_is_wireless_device} | A Boolean that indicates whether the device is considered a wireless device. | true
Marketing Name | %{wurfl_cap_marketing_name} | A string that indicates the device's marketing name. | BlackBerry 8100 Pearl
Mobile Browser | %{wurfl_cap_mobile_browser} | A string that indicates the browser that's used to request content from the device. | Chrome
Mobile Browser Version | %{wurfl_cap_mobile_browser_version} | A string that indicates the version of the browser that's used to request content from the device. | 31
Model Name | %{wurfl_cap_model_name} | A string that indicates the device's model name. | s3
Progressive Download | %{wurfl_cap_progressive_download} | A Boolean that indicates whether the device supports the playback of audio and video while it is still being downloaded. | true
Release Date | %{wurfl_cap_release_date} | A string that indicates the year and month on which the device was added to the WURFL database.<br/><br/>Format: `yyyy_mm` | 2013_december
Resolution Height | %{wurfl_cap_resolution_height} | An integer that indicates the device's height in pixels. | 768
Resolution Width | %{wurfl_cap_resolution_width} | An integer that indicates the device's width in pixels. | 1024

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Edge Cname

Key information:

- The list of available edge CNAMEs is limited to those edge CNAMEs that have been configured on the Edge CNAMEs page for the platform on which the rules engine is being configured.
- Before you attempt to delete an edge CNAME configuration, make sure that an Edge Cname match condition does not reference it. Edge CNAME configurations that have been defined in a rule cannot be deleted from the Edge CNAMEs page.
- Don't use an AND IF statement to combine certain match conditions. For example, combining an Edge Cname match condition with a Customer Origin match condition would create a match pattern that could never be matched. For this reason, two Edge Cname match conditions cannot be combined through an AND IF statement.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Referring Domain

The host name associated with the referrer through which content was requested determines whether the Referring Domain condition is met.

The **Matches**/**Does Not Match** option determines the conditions under which the Referring Domain match condition is met:

- **Matches**: Requires the referring host name to match the specified values. 
- **Does Not Match**: Requires that the referring host name does not match the specified value.

Key information:

- Specify multiple host names by delimiting each one with a single space.
- This match condition supports [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
- If the specified value does not contain an asterisk, it must be an exact match for the referrer's host name. For example, specifying "mydomain.com" would not match "www.mydomain.com."
- Use the **Ignore Case** option to control whether a case-sensitive comparison is made.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---  

### Request Header Literal

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Literal match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match the one that's defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match the one that's defined in this match condition.
  
Key information:

- Header name comparisons are always case-insensitive. Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---  

### Request Header Regex

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Regex match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match the pattern that's defined in the specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match the specified regular expression.

Key information:

- Header name:
  - Header name comparisons are case-insensitive.
  - Replace spaces in the header name with "%20."
- Header value:
  - A header value can take advantage of regular expressions.
  - Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
  - The match condition is met only when a header value exactly matches at least one of the specified patterns.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Header Wildcard

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Wildcard match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match any of the specified values.
  
Key information:

- Header name:
  - Header name comparisons are case-insensitive.
  - Spaces in the header name should be replaced with "%20." You can also use this value to specify spaces in a header value.
- Header value:
  - A header value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
  - This match condition is met when a header value exactly matches to at least one of the specified patterns.
  - Specify multiple values by delimiting each one with a single space.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Method

The Request Method match condition is met only when assets are requested through the selected request method. The available request methods are:

- GET
- HEAD
- POST
- OPTIONS
- PUT
- DELETE
- TRACE
- CONNECT

Key information:

- By default, only the GET request method can generate cached content on the network. All other request methods are proxied through the network.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Scheme

The Request Scheme match condition is met only when assets are requested through the selected protocol. The available protocols are:

- HTTP
- HTTPS

Key information:

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Directory

Identifies a request by its relative path, which excludes the file name of the requested asset.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Directory match condition is met.

- **Matches**: Requires the request to contain a relative URL path, excluding the file name, that matches the specified URL pattern.
- **Does Not Match**: Requires the request to contain a relative URL path, excluding file name, that does not match the specified URL pattern.

Key information:

- Use the **Relative to** option to specify whether the URL comparison starts before or after the content access point. The content access point is the portion of the path that appears between the Verizon CDN hostname and the relative path to the requested asset (for example, /800001/CustomerOrigin). It identifies a location by server type (for example, CDN or customer origin) and your customer account number.

   The following values are available for the **Relative to** option:
  - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname. 

  For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder**/index.htm

  - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

  For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder**/index.htm 

  This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder**/index.htm

- An edge CNAME URL is rewritten to a CDN URL prior to the URL comparison.

    For example, both of the following URLs point to the same asset and therefore have the same URL path.
  - CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
    
  - Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm
    
    Additional information:
  - Custom domain: https:\//my.domain.com/path/asset.htm
    
    - URL path (relative to root): /800001/CustomerOrigin/path/
    
    - URL path (relative to origin): /path/

- The portion of the URL that is used for the URL comparison ends just before the file name of the requested asset. A trailing forward slash is the last character in this type of path.

- Replace any spaces in the URL path pattern with "%20."

- Each URL path pattern can contain one or more asterisks (*), where each asterisk matches a sequence of one or more characters.

- Specify multiple URL paths in the pattern by delimiting each one with a single space.

    For example: */sales/ */marketing/

- A URL path specification can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Extension

Identifies requests by the file extension of the requested asset.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Extension match condition is met.

- **Matches**: Requires the URL of the request to contain a file extension that exactly matches the specified pattern.

   For example, if you specify "htm", "htm" assets are matched, but not "html" assets.  

- **Does Not Match**: Requires the URL request to contain a file extension that does not match the specified pattern.

Key information:

- Specify the file extensions to match in the **Value** box. Do not include a leading period; for example, use htm instead of .htm.

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

- Specify multiple file extensions by delimiting each extension with a single space. 

    For example: htm html

- For example, specifying "htm" matches "htm" assets, but not "html" assets.

#### Sample Scenario

The following sample configuration assumes that this match condition is met when a request matches one of the specified extensions.

Value specification: asp aspx php html

This match condition is met when it finds URLs that end with the following extensions:

- .asp
- .aspx
- .php
- .html

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Filename

Identifies requests by the file name of the requested asset. For the purposes of this match condition, a file name consists of the name of the requested asset, a period, and the file extension (for example, index.html).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Filename match condition is met.

- **Matches**: Requires the request to contain a file name in its URL path that matches the specified pattern.
- **Does Not Match**: Requires the request to contain a file name in its URL path that does not match the specified pattern.

Key information:

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

- To specify multiple file extensions, separate each extension with a single space.

    For example: index.htm index.html

- Replace spaces in a file name value with "%20."

- A file name value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). For example, each file name pattern can consist of one or more asterisks (*), where each asterisk matches a sequence of one or more characters.

- If wildcard characters are not specified, then only an exact match will satisfy this match condition.

    For example, specifying "presentation.ppt" matches an asset named "presentation.ppt," but not one named "presentation.pptx."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Literal

Compares a request's URL path, including file name, to the specified value.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Literal match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified pattern.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified pattern.

Key information:

- Use the **Relative to** option to specify whether the URL comparison point begins before or after the content access point. 

    The following values are available for the **Relative to** option:
  - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname.

    For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder/index.htm**

  - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

    For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

  This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder/index.htm**

- An edge CNAME URL is rewritten to a CDN URL prior to a URL comparison.

For example, both of the following URLs point to the same asset and therefore have the same URL path:

- CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
- Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm

    Additional information:
    
    - URL path (relative to root): /800001/CustomerOrigin/path/asset.htm
   
    - URL path (relative to origin): /path/asset.htm

- Query strings in the URL are ignored.
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
- The value specified for this match condition is compared against the relative path of the exact request made by the client.

- To match all requests made to a particular directory, use the [URL Path Directory](#url-path-directory) or the [URL Path Wildcard](#url-path-wildcard) match condition.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Regex

Compares a request's URL path to the specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Regex match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified regular expression.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified regular expression.

Key information:

- An edge CNAME URL is rewritten to a CDN URL prior to URL comparison.

    For example, both URLs point to the same asset and therefore have the same URL path.

     - CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm

     - Edge CNAME URL: http:\//my.domain.com/path/asset.htm

    Additional information:
    
     - URL path: /800001/CustomerOrigin/path/asset.htm

- Query strings in the URL are ignored.
    
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
    
- Spaces in the URL path should be replaced with "%20."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Wildcard

Compares a request's relative URL path to the specified wildcard pattern.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Wildcard match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified wildcard pattern.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified wildcard pattern.

Key information:

- **Relative to** option: This option determines whether the URL comparison point begins before or after the content access point.

   This option can have the following values:
     - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname.

       For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder/index.htm**

     - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

       For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

     This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder/index.htm**

- An edge CNAME URL is rewritten to a CDN URL prior to URL comparison.

    For example, both of the following URLs point to the same asset and therefore have the same URL path:
     - CDN URL: http://wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
     - Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm
    
    Additional information:
    
     - URL path (relative to root): /800001/CustomerOrigin/path/asset.htm
    
     - URL path (relative to origin): /path/asset.htm
    
- Specify multiple URL paths by delimiting each one with a single space.

   For example: /marketing/asset.* /sales/*.htm

- Query strings in the URL are ignored.
    
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
    
- Replace spaces in the URL path with "%20."
    
- The value specified for a URL path can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). Each URL path pattern can contain one or more asterisks (*), where each asterisk can match a sequence of one or more characters.

#### Sample Scenarios

The sample configurations in the following table assume that this match condition is met when a request matches the specified URL pattern:

Value                   | Relative to    | Result 
------------------------|----------------|-------
*/test.html */test.php  | Root or Origin | This pattern is matched by requests for assets named "test.html" or "test.php" in any folder.
/80ABCD/origin/text/*   | Root           | This pattern is matched when the requested asset meets the following criteria: <br />- It must reside on a customer origin called "origin." <br />- The relative path must start with a folder called "text." That is, the requested asset can either reside in the "text" folder or one of its recursive subfolders.
*/css/* */js/*          | Root or Origin | This pattern is matched by all CDN or edge CNAME URLs that contain a css or js folder.
*.jpg *.gif *.png       | Root or Origin | This pattern is matched by all CDN or edge CNAME URLs ending with .jpg, .gif, or .png. An alternative way to specify this pattern is with the [URL Path Extension match condition](#url-path-extension).
/images/* /media/*      | Origin         | This pattern is matched by CDN or edge CNAME URLs whose relative path starts with an "images" or "media" folder. <br />- CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/images/sales/event1.png<br />- Sample edge CNAME URL: http:\//cdn.mydomain.com/images/sales/event1.png

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Literal

Compares a request's query string to the specified value.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Literal match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified query string.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified query string.

Key information:

- Only exact query string matches satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Do not include a leading question mark (?) in the query string value text.
    
- Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

   Character | URL Encoding
   ----------|---------
   Space     | %20
   &         | %25

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Parameter

Identifies requests that contain the specified query string parameter. This parameter is set to a value that matches a specified pattern. Query string parameters (for example, parameter=value) in the request URL determine whether this condition is met. This match condition identifies a query string parameter by its name and accepts one or more values for the parameter value. 

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Parameter match condition is met.

- **Matches**: Requires a request to contain the specified parameter with a value that matches at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified parameter.
  - It contains the specified parameter, but its value does not match any of the values that are defined in this match condition.

This match condition provides an easy way to specify parameter name/value combinations. For more flexibility if you are matching a query string parameter, consider using the [URL Query Wildcard](#url-query-wildcard) match condition.

Key information:

- Only a single URL query parameter name can be specified per instance of this match condition.
    
- Because wildcard values are not supported when a parameter name is specified, only exact parameter name matches are eligible for comparison.
- Parameter value(s) can include [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
   - Each parameter value pattern can consist of one or more asterisks (*), where each asterisk can match a sequence of one or more characters.
   - Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

       Character | URL Encoding
       ----------|---------
       Space     | %20
       &         | %25

- Specify multiple query string parameter values by delimiting each one with a single space. This match condition is met when a request contains one of the specified name/value combinations.

   - Example 1:

     - Configuration:

       ValueA ValueB

     - This configuration matches the following query string parameters:

       Parameter1=ValueA
    
       Parameter1=ValueB

   - Example 2:

     - Configuration: 

        Value%20A Value%20B

     - This configuration matches the following query string parameters:

       Parameter1=Value%20A

       Parameter1=Value%20B

- This match condition is met only when there is an exact match to at least one of the specified query string name/value combinations.

   For example, if you use the configuration in the previous example, the parameter name/value combination "Parameter1=ValueAdd" would not be considered a match. However, if you specify either of the following values, it will match that name/value combination:

   - ValueA ValueB ValueAdd
   - ValueA* ValueB

- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

#### Sample scenarios

The following example demonstrates how this option works in specific situations:

Name  | Value |  Result
------|-------|--------
User  | Joe   | This pattern is matched when the query string for a requested URL is "?user=joe."
User  | *     | This pattern is matched when the query string for a requested URL contains a User parameter.
Email | Joe\* | This pattern is matched when the query string for a requested URL contains an Email parameter that starts with "Joe."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Regex

Identifies requests that contain the specified query string parameter. This parameter is set to a value that matches a specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Regex match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified regular expression.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified regular expression.

Key information:

- Only exact matches to the specified regular expression satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- For the purposes of this option, a query string starts with the first character after the question mark (?) delimiter for the query string.
    
- Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

   Character | URL Encoding | Value
   ----------|--------------|------
   Space     | %20          | \%20
   &         | %25          | \%25

   Note that percentage symbols must be escaped.

- Double-escape special regular expression characters (for example, \^$.+) to include a backslash in the regular expression.

   For example:

   Value | Interpreted As 
   ------|---------------
   \\+    | +
   \\\\+   | \\+

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Wildcard

Compares the specified value(s) against the request's query string.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Wildcard match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified wildcard value.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified wildcard value.

Key information:

- For the purposes of this option, a query string starts with the first character after the question mark (?) delimiter for the query string.
- Parameter values can include [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values):
   - Each parameter value pattern can consist of one or more asterisks (*), where each asterisk can match a sequence of one or more characters.
   - Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

     Character | URL Encoding
     ----------|---------
     Space     | %20
     &         | %25

- Specify multiple values by delimiting each one with a single space.

   For example: *Parameter1=ValueA* *ValueB* *Parameter1=ValueC&Parameter2=ValueD*

- Only exact matches to at least one of the specified query string patterns satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

#### Sample scenarios

The following example demonstrates how this option works in specific situations:

 Name                 | Description
 ---------------------|------------
user=joe              | This pattern is matched when the query string for a requested URL is "?user=joe."
\*user=\* \*optout=\* | This pattern is matched when the CDN URL query contains either the user or optout parameter.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

## Next steps

- [Azure Content Delivery Network overview](cdn-overview.md)
- [Rules engine reference](cdn-verizon-premium-rules-engine-reference.md)
- [Rules engine conditional expressions](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Rules engine features](cdn-verizon-premium-rules-engine-reference-features.md)
- [Overriding default HTTP behavior using the rules engine](cdn-verizon-premium-rules-engine.md)\`25
title: Azure CDN from Verizon Premium rules engine match conditions | Microsoft Docs
description: Reference documentation for Azure Content Delivery Network from Verizon Premium rules engine match conditions.
services: cdn
author: mdgattuso

ms.service: azure-cdn
ms.topic: article
ms.date: 05/31/2019
ms.author: magattus

---

# Azure CDN from Verizon Premium rules engine match conditions

This article lists detailed descriptions of the available match conditions for the Azure Content Delivery Network (CDN) from Verizon Premium [rules engine](cdn-verizon-premium-rules-engine.md).

The second part of a rule is the match condition. A match condition identifies specific types of requests for which a set of features will be performed.

For example, you can use a match condition to:

- Filter requests for content at a particular location.
- Filter requests generated from a particular IP address or country/region.
- Filter requests by header information.

## Always match condition

The Always match condition applies a default set of features to all requests.

Name | Purpose
-----|--------
[Always](#always) | Applies a default set of features to all requests.

## Device match condition

The Device match condition identifies requests made from a mobile device based on its properties.  

Name | Purpose
-----|--------
[Device](#device) | Identifies requests made from a mobile device based on its properties.

## Location match conditions

The Location match conditions identify requests based on the requester's location.

Name | Purpose
-----|--------
[AS Number](#as-number) | Identifies requests that originate from a particular network.
[Country](#country) | Identifies requests that originate from the specified countries/regions.

## Origin match conditions

The Origin match conditions identify requests that point to Content Delivery Network storage or a customer origin server.

Name | Purpose
-----|--------
[CDN Origin](#cdn-origin) | Identifies requests for content stored in Content Delivery Network storage.
[Customer Origin](#customer-origin) | Identifies requests for content stored on a specific customer origin server.

## Request match conditions

The Request match conditions identify requests based on their properties.

Name | Purpose
-----|--------
[Client IP Address](#client-ip-address) | Identifies requests that originate from a particular IP address.
[Cookie Parameter](#cookie-parameter) | Checks the cookies associated with each request for the specified value.
[Cookie Parameter Regex](#cookie-parameter-regex) | Checks the cookies associated with each request for the specified regular expression.
[Edge Cname](#edge-cname) | Identifies requests that point to a specific edge CNAME.
[Referring Domain](#referring-domain) | Identifies requests that were referred from the specified host names.
[Request Header Literal](#request-header-literal) | Identifies requests that contain the specified header set to a specified value.
[Request Header Regex](#request-header-regex) | Identifies requests that contain the specified header set to a value that matches the specified regular expression.
[Request Header Wildcard](#request-header-wildcard) | Identifies requests that contain the specified header set to a value that matches the specified pattern.
[Request Method](#request-method) | Identifies requests by their HTTP method.
[Request Scheme](#request-scheme) | Identifies requests by their HTTP protocol.

## URL match conditions

The URL match conditions identify requests based on their URLs.

Name | Purpose
-----|--------
[URL Path Directory](#url-path-directory) | Identifies requests by their relative path.
[URL Path Extension](#url-path-extension) | Identifies requests by their file name extension.
[URL Path Filename](#url-path-filename) | Identifies requests by their file name.
[URL Path Literal](#url-path-literal) | Compares a request's relative path to the specified value.
[URL Path Regex](#url-path-regex) | Compares a request's relative path to the specified regular expression.
[URL Path Wildcard](#url-path-wildcard) | Compares a request's relative path to the specified pattern.
[URL Query Literal](#url-query-literal) | Compares a request's query string to the specified value.
[URL Query Parameter](#url-query-parameter) | Identifies requests that contain the specified query string parameter set to a value that matches a specified pattern.
[URL Query Regex](#url-query-regex) | Identifies requests that contain the specified query string parameter set to a value that matches a specified regular expression.
[URL Query Wildcard](#url-query-wildcard) | Compares the specified value against the request's query string.

## Reference for rules engine match conditions

---

### Always

The Always match condition applies a default set of features to all requests.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### AS Number

The AS Number network is defined by its autonomous system number (ASN). 

The **Matches**/**Does Not Match** option determines the conditions under which the AS Number match condition is met:

- **Matches**: Requires that the ASN of the client network matches one of the specified ASNs. 
- **Does Not Match**: Requires that the ASN of the client network does not match any of the specified ASNs.

Key information:

- Specify multiple ASNs by delimiting each one with a single space. For example, 64514 64515 matches requests that arrive from either 64514 or 64515.
- Certain requests might not return a valid ASN. A question mark (?) will match requests for which a valid ASN could not be determined.
- Specify the entire ASN for the desired network. Partial values will not be matched.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### CDN Origin

The CDN Origin match condition is met when both of the following conditions are met:

- Content from CDN storage was requested.
- The request URI uses the type of content access point (for example, /000001) that's defined in this match condition:
  - CDN URL: The request URI must contain the selected content access point.
  - Edge CNAME URL: The corresponding edge CNAME configuration must point to the selected content access point.
  
Key information:

- The content access point identifies the service that should serve the requested content.
- Don't use an AND IF statement to combine certain match conditions. For example, combining a CDN Origin match condition with a Customer Origin match condition would create a match pattern that could never be matched. For this reason, two CDN Origin match conditions cannot be combined through an AND IF statement.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Client IP Address

The **Matches**/**Does Not Match** option determines the conditions under which the Client IP Address match condition is met:

- **Matches**: Requires that the client's IP address matches one of the specified IP addresses. 
- **Does Not Match**: Requires that the client's IP address does not match any of the specified IP addresses. 

Key information:

- Use CIDR notation.
- Specify multiple IP addresses and/or IP address blocks by delimiting each one with a single space. For example:
  - **IPv4 example**: 1.2.3.4 10.20.30.40 matches any requests that arrive from either address 1.2.3.4 or 10.20.30.40.
  - **IPv6 example**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 matches any requests that arrive from either address 1:2:3:4:5:6:7:8 or 10:20:30:40:50:60:70:80.
- The syntax for an IP address block is the base IP address followed by a forward slash and the prefix size. For example:
  - **IPv4 example**: 5.5.5.64/26 matches any requests that arrive from addresses 5.5.5.64 through 5.5.5.127.
  - **IPv6 example**: 1:2:3:/48 matches any requests that arrive from addresses 1:2:3:0:0:0:0:0 through 1:2:3:ffff:ffff:ffff:ffff:ffff.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Cookie Parameter

The **Matches**/**Does Not Match** option determines the conditions under which the Cookie Parameter match condition is met.

- **Matches**: Requires a request to contain the specified cookie with a value that matches at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified cookie.
  - It contains the specified cookie, but its value does not match any of the values that are defined in this match condition.
  
Key information:

- Cookie name:
  - Because wildcard values, including asterisks (*), are not supported when you're specifying a cookie name, only exact cookie name matches are eligible for comparison.
  - Only a single cookie name can be specified per instance of this match condition.
  - Cookie name comparisons are case-insensitive.
- Cookie value:
  - Specify multiple cookie values by delimiting each one with a single space.
  - A cookie value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - If a wildcard value has not been specified, then only an exact match will satisfy this match condition. For example, specifying "Value" will match "Value," but not "Value1" or "Value2."
  - Use the **Ignore Case** option to control whether a case-sensitive comparison is made against the request's cookie value.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)
</br>

---

### Cookie Parameter Regex

The Cookie Parameter Regex match condition defines a cookie name and value. You can use [regular expressions](cdn-verizon-premium-rules-engine-reference.md#regular-expressions) to define the desired cookie value.

The **Matches**/**Does Not Match** option determines the conditions under which the Cookie Parameter Regex match condition is met.

- **Matches**: Requires a request to contain the specified cookie with a value that matches the specified regular expression.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified cookie.
  - It contains the specified cookie, but its value does not match the specified regular expression.
  
Key information:

- Cookie name:
  - Because regular expressions and wildcard values, including asterisks (*), are not supported when you're specifying a cookie name, only exact cookie name matches are eligible for comparison.
  - Only a single cookie name can be specified per instance of this match condition.
  - Cookie name comparisons are case-insensitive.
- Cookie value:
  - A cookie value can take advantage of regular expressions.
  - Use the **Ignore Case** option to control whether a case-sensitive comparison is made against the request's cookie value.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Country

You can specify a country through its country code. 

The **Matches**/**Does Not Match** option determines the conditions under which the Country match condition is met:

- **Matches**: Requires the request to contain the specified country code values. 
- **Does Not Match**: Requires that the request does not contain the specified country code values.

Key information:

- Specify multiple country codes by delimiting each one with a single space.
- Wildcards are not supported when you're specifying a country code.
- The "EU" and "AP" country codes do not encompass all IP addresses in those regions.
- Certain requests might not return a valid country code. A question mark (?) will match requests for which a valid country code could not be determined.
- Country codes are case-sensitive.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

#### Implementing Country Filtering by using the rules engine

This match condition allows you to perform a multitude of customizations based on the location from which a request originated. For example, the behavior of the Country Filtering feature can be replicated through the following configuration:

- URL Path Wildcard match: Set the [URL Path Wildcard match condition](#url-path-wildcard) to the directory that will be secured. 
    Append an asterisk to the end of the relative path to ensure that access to all of its children will be restricted by this rule.

- Country match: Set the Country match condition to the desired set of countries.
  - Allow: Set the Country match condition to **Does Not Match** to allow only the specified countries access to content stored in the location defined by the URL Path Wildcard match condition.
  - Block: Set the Country match condition to **Matches** to block the specified countries from accessing content stored in the location defined by the URL Path Wildcard match condition.

- Deny Access (403) Feature: Enable the [Deny Access (403) feature](cdn-verizon-premium-rules-engine-reference-features.md#deny-access-403) to replicate the allow or block portion of the Country Filtering feature.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Customer Origin

Key information:

- The Customer Origin match condition is met regardless of whether content is requested through a CDN URL or an edge CNAME URL that points to the selected customer origin.
- A customer origin configuration that's referenced by a rule cannot be deleted from the Customer Origin page. Before you attempt to delete a customer origin configuration, make sure that the following configurations do not reference it:
  - A Customer Origin match condition
  - An edge CNAME configuration
- Don't use an AND IF statement to combine certain match conditions. For example, combining a Customer Origin match condition with a CDN Origin match condition would create a match pattern that could never be matched. For this reason, two Customer Origin match conditions cannot be combined through an AND IF statement.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Device

The Device match condition identifies requests made from a mobile device based on its properties. Mobile device detection is achieved through [WURFL](http://wurfl.sourceforge.net/). 

The **Matches**/**Does Not Match** option determines the conditions under which the Device match condition is met:

- **Matches**: Requires the requester's device to match the specified value. 
- **Does Not Match**: Requires that the requester's device does not match the specified value.

Key information:

- Use the **Ignore Case** option to specify whether the specified value is case-sensitive.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

#### String Type

A WURFL capability typically accepts any combination of numbers, letters, and symbols. Due to the flexible nature of this capability, you must choose how the value associated with this match condition is interpreted. The following table describes the available set of options:

Type     | Description
---------|------------
Literal  | Select this option to prevent most characters from taking on special meaning by using their [literal value](cdn-verizon-premium-rules-engine-reference.md#literal-values).
Wildcard | Select this option to take advantage of all [wildcard characters]([wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
Regex    | Select this option to use [regular expressions](cdn-verizon-premium-rules-engine-reference.md#regular-expressions). Regular expressions are useful for defining a pattern of characters.

#### WURFL capabilities

A WURFL capability refers to a category that describes mobile devices. The selected capability determines the type of mobile device description that is used to identify requests.

The following table lists WURFL capabilities and their variables for the rules engine.

> [!NOTE]
> The following variables are supported in the **Modify Client Request Header** and **Modify Client Response Header** features.

Capability | Variable | Description | Sample values
-----------|----------|-------------|----------------
Brand Name | %{wurfl_cap_brand_name} | A string that indicates the brand name of the device. | Samsung
Device OS | %{wurfl_cap_device_os} | A string that indicates the operating system installed on the device. | IOS
Device OS Version | %{wurfl_cap_device_os_version} | A string that indicates the version number of the operating system installed on the device. | 1.0.1
Dual Orientation | %{wurfl_cap_dual_orientation} | A Boolean that indicates whether the device supports dual orientation. | true
HTML Preferred DTD | %{wurfl_cap_html_preferred_dtd} | A string that indicates the mobile device's preferred document type definition (DTD) for HTML content. | none<br/>xhtml_basic<br/>html5
Image Inlining | %{wurfl_cap_image_inlining} | A Boolean that indicates whether the device supports Base64 encoded images. | false
Is Android | %{wurfl_vcap_is_android} | A Boolean that indicates whether the device uses the Android OS. | true
Is IOS | %{wurfl_vcap_is_ios} | A Boolean that indicates whether the device uses iOS. | false
Is Smart TV | %{wurfl_cap_is_smarttv} | A Boolean that indicates whether the device is a smart TV. | false
Is Smartphone | %{wurfl_vcap_is_smartphone} | A Boolean that indicates whether the device is a smartphone. | true
Is Tablet | %{wurfl_cap_is_tablet} | A Boolean that indicates whether the device is a tablet. This description is  OS-independent. | true
Is Wireless Device | %{wurfl_cap_is_wireless_device} | A Boolean that indicates whether the device is considered a wireless device. | true
Marketing Name | %{wurfl_cap_marketing_name} | A string that indicates the device's marketing name. | BlackBerry 8100 Pearl
Mobile Browser | %{wurfl_cap_mobile_browser} | A string that indicates the browser that's used to request content from the device. | Chrome
Mobile Browser Version | %{wurfl_cap_mobile_browser_version} | A string that indicates the version of the browser that's used to request content from the device. | 31
Model Name | %{wurfl_cap_model_name} | A string that indicates the device's model name. | s3
Progressive Download | %{wurfl_cap_progressive_download} | A Boolean that indicates whether the device supports the playback of audio and video while it is still being downloaded. | true
Release Date | %{wurfl_cap_release_date} | A string that indicates the year and month on which the device was added to the WURFL database.<br/><br/>Format: `yyyy_mm` | 2013_december
Resolution Height | %{wurfl_cap_resolution_height} | An integer that indicates the device's height in pixels. | 768
Resolution Width | %{wurfl_cap_resolution_width} | An integer that indicates the device's width in pixels. | 1024

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Edge Cname

Key information:

- The list of available edge CNAMEs is limited to those edge CNAMEs that have been configured on the Edge CNAMEs page for the platform on which the rules engine is being configured.
- Before you attempt to delete an edge CNAME configuration, make sure that an Edge Cname match condition does not reference it. Edge CNAME configurations that have been defined in a rule cannot be deleted from the Edge CNAMEs page.
- Don't use an AND IF statement to combine certain match conditions. For example, combining an Edge Cname match condition with a Customer Origin match condition would create a match pattern that could never be matched. For this reason, two Edge Cname match conditions cannot be combined through an AND IF statement.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Referring Domain

The host name associated with the referrer through which content was requested determines whether the Referring Domain condition is met.

The **Matches**/**Does Not Match** option determines the conditions under which the Referring Domain match condition is met:

- **Matches**: Requires the referring host name to match the specified values. 
- **Does Not Match**: Requires that the referring host name does not match the specified value.

Key information:

- Specify multiple host names by delimiting each one with a single space.
- This match condition supports [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
- If the specified value does not contain an asterisk, it must be an exact match for the referrer's host name. For example, specifying "mydomain.com" would not match "www.mydomain.com."
- Use the **Ignore Case** option to control whether a case-sensitive comparison is made.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---  

### Request Header Literal

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Literal match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match the one that's defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match the one that's defined in this match condition.
  
Key information:

- Header name comparisons are always case-insensitive. Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---  

### Request Header Regex

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Regex match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match the pattern that's defined in the specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match the specified regular expression.

Key information:

- Header name:
  - Header name comparisons are case-insensitive.
  - Replace spaces in the header name with "%20."
- Header value:
  - A header value can take advantage of regular expressions.
  - Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
  - The match condition is met only when a header value exactly matches at least one of the specified patterns.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Header Wildcard

The **Matches**/**Does Not Match** option determines the conditions under which the Request Header Wildcard match condition is met.

- **Matches**: Requires the request to contain the specified header. Its value must match at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified header.
  - It contains the specified header, but its value does not match any of the specified values.
  
Key information:

- Header name:
  - Header name comparisons are case-insensitive.
  - Spaces in the header name should be replaced with "%20." You can also use this value to specify spaces in a header value.
- Header value:
  - A header value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
  - Use the **Ignore Case** option to control the case-sensitivity of header value comparisons.
  - This match condition is met when a header value exactly matches to at least one of the specified patterns.
  - Specify multiple values by delimiting each one with a single space.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Method

The Request Method match condition is met only when assets are requested through the selected request method. The available request methods are:

- GET
- HEAD
- POST
- OPTIONS
- PUT
- DELETE
- TRACE
- CONNECT

Key information:

- By default, only the GET request method can generate cached content on the network. All other request methods are proxied through the network.
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### Request Scheme

The Request Scheme match condition is met only when assets are requested through the selected protocol. The available protocols are:

- HTTP
- HTTPS

Key information:

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
  - Complete Cache Fill
  - Default Internal Max-Age
  - Force Internal Max-Age
  - Ignore Origin No-Cache
  - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Directory

Identifies a request by its relative path, which excludes the file name of the requested asset.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Directory match condition is met.

- **Matches**: Requires the request to contain a relative URL path, excluding the file name, that matches the specified URL pattern.
- **Does Not Match**: Requires the request to contain a relative URL path, excluding file name, that does not match the specified URL pattern.

Key information:

- Use the **Relative to** option to specify whether the URL comparison starts before or after the content access point. The content access point is the portion of the path that appears between the Verizon CDN hostname and the relative path to the requested asset (for example, /800001/CustomerOrigin). It identifies a location by server type (for example, CDN or customer origin) and your customer account number.

   The following values are available for the **Relative to** option:
  - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname. 

  For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder**/index.htm

  - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

  For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder**/index.htm 

  This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder**/index.htm

- An edge CNAME URL is rewritten to a CDN URL prior to the URL comparison.

    For example, both of the following URLs point to the same asset and therefore have the same URL path.
  - CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
    
  - Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm
    
    Additional information:
  - Custom domain: https:\//my.domain.com/path/asset.htm
    
    - URL path (relative to root): /800001/CustomerOrigin/path/
    
    - URL path (relative to origin): /path/

- The portion of the URL that is used for the URL comparison ends just before the file name of the requested asset. A trailing forward slash is the last character in this type of path.

- Replace any spaces in the URL path pattern with "%20."

- Each URL path pattern can contain one or more asterisks (*), where each asterisk matches a sequence of one or more characters.

- Specify multiple URL paths in the pattern by delimiting each one with a single space.

    For example: */sales/ */marketing/

- A URL path specification can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Extension

Identifies requests by the file extension of the requested asset.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Extension match condition is met.

- **Matches**: Requires the URL of the request to contain a file extension that exactly matches the specified pattern.

   For example, if you specify "htm", "htm" assets are matched, but not "html" assets.  

- **Does Not Match**: Requires the URL request to contain a file extension that does not match the specified pattern.

Key information:

- Specify the file extensions to match in the **Value** box. Do not include a leading period; for example, use htm instead of .htm.

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

- Specify multiple file extensions by delimiting each extension with a single space. 

    For example: htm html

- For example, specifying "htm" matches "htm" assets, but not "html" assets.

#### Sample Scenario

The following sample configuration assumes that this match condition is met when a request matches one of the specified extensions.

Value specification: asp aspx php html

This match condition is met when it finds URLs that end with the following extensions:

- .asp
- .aspx
- .php
- .html

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Filename

Identifies requests by the file name of the requested asset. For the purposes of this match condition, a file name consists of the name of the requested asset, a period, and the file extension (for example, index.html).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Filename match condition is met.

- **Matches**: Requires the request to contain a file name in its URL path that matches the specified pattern.
- **Does Not Match**: Requires the request to contain a file name in its URL path that does not match the specified pattern.

Key information:

- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.

- To specify multiple file extensions, separate each extension with a single space.

    For example: index.htm index.html

- Replace spaces in a file name value with "%20."

- A file name value can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). For example, each file name pattern can consist of one or more asterisks (*), where each asterisk matches a sequence of one or more characters.

- If wildcard characters are not specified, then only an exact match will satisfy this match condition.

    For example, specifying "presentation.ppt" matches an asset named "presentation.ppt," but not one named "presentation.pptx."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Literal

Compares a request's URL path, including file name, to the specified value.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Literal match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified pattern.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified pattern.

Key information:

- Use the **Relative to** option to specify whether the URL comparison point begins before or after the content access point. 

    The following values are available for the **Relative to** option:
  - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname.

    For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder/index.htm**

  - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

    For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

  This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder/index.htm**

- An edge CNAME URL is rewritten to a CDN URL prior to a URL comparison.

For example, both of the following URLs point to the same asset and therefore have the same URL path:

- CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
- Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm

    Additional information:
    
    - URL path (relative to root): /800001/CustomerOrigin/path/asset.htm
   
    - URL path (relative to origin): /path/asset.htm

- Query strings in the URL are ignored.
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
- The value specified for this match condition is compared against the relative path of the exact request made by the client.

- To match all requests made to a particular directory, use the [URL Path Directory](#url-path-directory) or the [URL Path Wildcard](#url-path-wildcard) match condition.

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Regex

Compares a request's URL path to the specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Regex match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified regular expression.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified regular expression.

Key information:

- An edge CNAME URL is rewritten to a CDN URL prior to URL comparison.

    For example, both URLs point to the same asset and therefore have the same URL path.

     - CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm

     - Edge CNAME URL: http:\//my.domain.com/path/asset.htm

    Additional information:
    
     - URL path: /800001/CustomerOrigin/path/asset.htm

- Query strings in the URL are ignored.
    
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
    
- Spaces in the URL path should be replaced with "%20."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Path Wildcard

Compares a request's relative URL path to the specified wildcard pattern.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Path Wildcard match condition is met.

- **Matches**: Requires the request to contain a URL path that matches the specified wildcard pattern.
- **Does Not Match**: Requires the request to contain a URL path that does not match the specified wildcard pattern.

Key information:

- **Relative to** option: This option determines whether the URL comparison point begins before or after the content access point.

   This option can have the following values:
     - **Root**: Indicates that the URL comparison point begins directly after the CDN hostname.

       For example: http:\//wpc.0001.&lt;domain&gt;/**800001/myorigin/myfolder/index.htm**

     - **Origin**: Indicates that the URL comparison point begins after the content access point (for example, /000001 or /800001/myorigin). Because the \*.azureedge.net CNAME is created relative to the origin directory on the Verizon CDN hostname by default, Azure CDN users should use the **Origin** value. 

       For example: https:\//&lt;endpoint&gt;.azureedge.net/**myfolder/index.htm**

     This URL points to the following Verizon CDN hostname: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/**myfolder/index.htm**

- An edge CNAME URL is rewritten to a CDN URL prior to URL comparison.

    For example, both of the following URLs point to the same asset and therefore have the same URL path:
     - CDN URL: http://wpc.0001.&lt;domain&gt;/800001/CustomerOrigin/path/asset.htm
     - Edge CNAME URL: http:\//&lt;endpoint&gt;.azureedge.net/path/asset.htm
    
    Additional information:
    
     - URL path (relative to root): /800001/CustomerOrigin/path/asset.htm
    
     - URL path (relative to origin): /path/asset.htm
    
- Specify multiple URL paths by delimiting each one with a single space.

   For example: /marketing/asset.* /sales/*.htm

- Query strings in the URL are ignored.
    
- Use the **Ignore Case** option to control whether a case-sensitive comparison is performed.
    
- Replace spaces in the URL path with "%20."
    
- The value specified for a URL path can take advantage of [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values). Each URL path pattern can contain one or more asterisks (*), where each asterisk can match a sequence of one or more characters.

#### Sample Scenarios

The sample configurations in the following table assume that this match condition is met when a request matches the specified URL pattern:

Value                   | Relative to    | Result 
------------------------|----------------|-------
*/test.html */test.php  | Root or Origin | This pattern is matched by requests for assets named "test.html" or "test.php" in any folder.
/80ABCD/origin/text/*   | Root           | This pattern is matched when the requested asset meets the following criteria: <br />- It must reside on a customer origin called "origin." <br />- The relative path must start with a folder called "text." That is, the requested asset can either reside in the "text" folder or one of its recursive subfolders.
*/css/* */js/*          | Root or Origin | This pattern is matched by all CDN or edge CNAME URLs that contain a css or js folder.
*.jpg *.gif *.png       | Root or Origin | This pattern is matched by all CDN or edge CNAME URLs ending with .jpg, .gif, or .png. An alternative way to specify this pattern is with the [URL Path Extension match condition](#url-path-extension).
/images/* /media/*      | Origin         | This pattern is matched by CDN or edge CNAME URLs whose relative path starts with an "images" or "media" folder. <br />- CDN URL: http:\//wpc.0001.&lt;domain&gt;/800001/myorigin/images/sales/event1.png<br />- Sample edge CNAME URL: http:\//cdn.mydomain.com/images/sales/event1.png

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Literal

Compares a request's query string to the specified value.

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Literal match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified query string.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified query string.

Key information:

- Only exact query string matches satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Do not include a leading question mark (?) in the query string value text.
    
- Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

   Character | URL Encoding
   ----------|---------
   Space     | %20
   &         | %25

- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Parameter

Identifies requests that contain the specified query string parameter. This parameter is set to a value that matches a specified pattern. Query string parameters (for example, parameter=value) in the request URL determine whether this condition is met. This match condition identifies a query string parameter by its name and accepts one or more values for the parameter value. 

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Parameter match condition is met.

- **Matches**: Requires a request to contain the specified parameter with a value that matches at least one of the values that are defined in this match condition.
- **Does Not Match**: Requires that the request meets either of the following criteria:
  - It does not contain the specified parameter.
  - It contains the specified parameter, but its value does not match any of the values that are defined in this match condition.

This match condition provides an easy way to specify parameter name/value combinations. For more flexibility if you are matching a query string parameter, consider using the [URL Query Wildcard](#url-query-wildcard) match condition.

Key information:

- Only a single URL query parameter name can be specified per instance of this match condition.
    
- Because wildcard values are not supported when a parameter name is specified, only exact parameter name matches are eligible for comparison.
- Parameter value(s) can include [wildcard values](cdn-verizon-premium-rules-engine-reference.md#wildcard-values).
   - Each parameter value pattern can consist of one or more asterisks (*), where each asterisk can match a sequence of one or more characters.
   - Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

       Character | URL Encoding
       ----------|---------
       Space     | %20
       &         | %25

- Specify multiple query string parameter values by delimiting each one with a single space. This match condition is met when a request contains one of the specified name/value combinations.

   - Example 1:

     - Configuration:

       ValueA ValueB

     - This configuration matches the following query string parameters:

       Parameter1=ValueA
    
       Parameter1=ValueB

   - Example 2:

     - Configuration: 

        Value%20A Value%20B

     - This configuration matches the following query string parameters:

       Parameter1=Value%20A

       Parameter1=Value%20B

- This match condition is met only when there is an exact match to at least one of the specified query string name/value combinations.

   For example, if you use the configuration in the previous example, the parameter name/value combination "Parameter1=ValueAdd" would not be considered a match. However, if you specify either of the following values, it will match that name/value combination:

   - ValueA ValueB ValueAdd
   - ValueA* ValueB

- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- Due to the manner in which cache settings are tracked, this match condition is incompatible with the following features:
   - Complete Cache Fill
   - Default Internal Max-Age
   - Force Internal Max-Age
   - Ignore Origin No-Cache
   - Internal Max-Stale

#### Sample scenarios

The following example demonstrates how this option works in specific situations:

Name  | Value |  Result
------|-------|--------
User  | Joe   | This pattern is matched when the query string for a requested URL is "?user=joe."
User  | *     | This pattern is matched when the query string for a requested URL contains a User parameter.
Email | Joe\* | This pattern is matched when the query string for a requested URL contains an Email parameter that starts with "Joe."

[Back to top](#reference-for-rules-engine-match-conditions)

</br>

---

### URL Query Regex

Identifies requests that contain the specified query string parameter. This parameter is set to a value that matches a specified [regular expression](cdn-verizon-premium-rules-engine-reference.md#regular-expressions).

The **Matches**/**Does Not Match** option determines the conditions under which the URL Query Regex match condition is met.

- **Matches**: Requires the request to contain a URL query string that matches the specified regular expression.
- **Does Not Match**: Requires the request to contain a URL query string that does not match the specified regular expression.

Key information:

- Only exact matches to the specified regular expression satisfy this match condition.
    
- Use the **Ignore Case** option to control the case-sensitivity of query string comparisons.
    
- For the purposes of this option, a query string starts with the first character after the question mark (?) delimiter for the query string.
    
- Certain characters require URL encoding. Use the percentage symbol to URL encode the following characters:

   Character | URL Encoding | Value
   ----------|--------------|------
   Space     | %20          | \%20
   &         | %25          | \%25

   Tenga en cuenta que los símbolos de porcentaje deben usarse en forma de secuencia de escape.

- Realice una doble secuencia de escape en los caracteres especiales de las expresiones regulares (por ejemplo, \^$.+) para incluir una barra diagonal inversa en la expresión regular.

   Por ejemplo:

   Valor | Se interpreta como 
   ------|---------------
   \\+    | +
   \\\\+   | \\+

- Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta condición de coincidencia es incompatible con las siguientes características:
   - Relleno de la memoria caché completa
   - Max-Age interna predeterminada
   - Forzar Max-Age interna
   - Ignorar no almacenar en caché de origen
   - Max-Stale interna

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

---

### <a name="url-query-wildcard"></a>Carácter comodín de consulta de dirección URL

Compara los valores especificados con la cadena de consulta de una solicitud.

La opción **Matches**/**Does Not Match** (Coincide/No coincide) determina las condiciones para que se cumpla la condición de coincidencia Caracteres comodín de la consulta URL.

- **Matches** (Coincide): requiere que la solicitud contenga una cadena de consulta URL que coincida con el valor del carácter comodín especificado.
- **Does Not Match** (No coincide): requiere que la solicitud contenga una cadena de consulta URL que no coincida con el valor del carácter comodín especificado.

Información importante:

- A los efectos de esta opción, una cadena de consulta comienza con el primer carácter después del delimitador de signo de interrogación (?) de la cadena de consulta.
- Los valores de los parámetros pueden incluir [valores de caracteres comodín](cdn-verizon-premium-rules-engine-reference.md#wildcard-values):
   - Cada patrón de valor de parámetro puede contener uno o varios asteriscos (*), donde cada asterisco coincide con una secuencia de uno o más caracteres.
   - Ciertos caracteres requieren codificación URL. Utilice el símbolo de porcentaje para codificar en formato URL los caracteres siguientes:

     Character | Codificación URL
     ----------|---------
     Espacio     | %20
     &         | %25

- Especifique varios valores de encabezado mediante la delimitación de cada uno con un espacio.

   Por ejemplo:  *Parameter1=ValueA* *ValueB* *Parameter1=ValueC&Parameter2=ValueD*

- Solo las coincidencias exactas con al menos uno de los patrones de cadena de consulta especificados satisfacen esta condición de coincidencia.
    
- Utilice la opción **Ignore Case** (No distinguir mayúsculas y minúsculas) para controlar si las comparaciones de cadena de consulta distinguen las mayúsculas y las minúsculas.
    
- Debido a la manera en que se realiza el seguimiento de la configuración de la memoria caché, esta condición de coincidencia es incompatible con las siguientes características:
   - Relleno de la memoria caché completa
   - Max-Age interna predeterminada
   - Forzar Max-Age interna
   - Ignorar no almacenar en caché de origen
   - Max-Stale interna

#### <a name="sample-scenarios"></a>Escenarios de ejemplo

En el ejemplo siguiente se muestra cómo funciona esta opción en situaciones específicas:

 NOMBRE                 | DESCRIPCIÓN
 ---------------------|------------
user=joe              | Este patrón coincide cuando la cadena de consulta para una dirección URL solicitada es "?user=joe".
\*user=\* \*optout=\* | Este patrón coincide cuando la consulta URL de la red CDN contiene el parámetro user o el parámetro optout.

[Volver arriba](#reference-for-rules-engine-match-conditions)

</br>

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a Azure Content Delivery Network](cdn-overview.md)
- [Referencia del motor de reglas](cdn-verizon-premium-rules-engine-reference.md)
- [Expresiones condicionales del motor de reglas](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Características del motor de reglas](cdn-verizon-premium-rules-engine-reference-features.md)
- [Invalidación del comportamiento HTTP predeterminado mediante el motor de reglas](cdn-verizon-premium-rules-engine.md)
