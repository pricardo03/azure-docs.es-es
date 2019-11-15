---
title: Condiciones de coincidencia del motor de reglas estándar de Azure CDN de Microsoft | Microsoft Docs
description: Documentación de referencia para las condiciones de coincidencia del motor de reglas de Azure Content Delivery Network de Microsoft.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: magattus
ms.openlocfilehash: a72452d37b152a9463a5aee0e199fd42ea852236
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73615843"
---
# <a name="azure-cdn-from-microsoft-standard-rules-engine-match-conditions"></a>Condiciones de coincidencia del motor de reglas estándar de Azure CDN de Microsoft

En este artículo se muestran descripciones detalladas de las condiciones de coincidencia disponibles para el [motor de reglas estándar](cdn-standard-rules-engine.md) de Azure Content Delivery Network (CDN) de Microsoft.

La primera parte de una regla es un conjunto de condiciones de coincidencia. Cada regla puede tener hasta 4 condiciones de coincidencia. Una condición de coincidencia identifica tipos específicos de solicitudes para los que se realizarán las acciones definidas en la regla. Si usa varias condiciones de coincidencia, se agruparán con la lógica AND.

Por ejemplo, puede usar una condición de coincidencia para:

- Filtrar las solicitudes generadas a partir de una dirección IP o país o región específicos.
- Filtrar las solicitudes por la información de encabezado.
- Filtrar las solicitudes de los dispositivos móviles o de escritorio.

## <a name="match-conditions"></a>Condiciones de coincidencia

Están disponibles para su uso las siguientes condiciones de coincidencia. 

### <a name="device-type"></a>Tipo de dispositivo 

La condición de coincidencia Tipo de dispositivo identifica solicitudes realizadas desde un dispositivo móvil o de escritorio en función de sus propiedades.  

**Campos obligatorios**

Operator | Valor admitido
---------|----------------
Es igual a, No es igual a | Móvil, Escritorio


### <a name="http-version"></a>Versión HTTP

La condición de coincidencia Versión HTTP identifica las solicitudes basadas en la versión HTTP con la que llega la solicitud.

**Campos obligatorios**

Operator | Valor admitido
---------|----------------
Es igual a, No es igual a | 2.0, 1.1, 1.0, 0.9, Todas


### <a name="request-cookies"></a>Cookies de solicitud

La condición de coincidencia Cookies de solicitud identifica las solicitudes en función de la información de las cookies de la solicitud entrante.

**Campos obligatorios**

Nombre de la cookie | Operator | Valor de la cookie | Transformación de caso
------------|----------|--------------|---------------
Cadena | [Lista de operadores estándar](#standard-operator-list) | Cadena, Int | Sin transformación, en mayúsculas, en minúsculas

Información importante
- Los valores de carácter comodín, incluidos los asteriscos (*), no se admiten al especificar un nombre de cookie, solo las coincidencias exactas de nombre de cookie son aptas para la comparación.
- Solo se puede especificar un nombre de cookie por instancia de esta condición de coincidencia.
- Las comparaciones de nombres de cookie distinguen entre mayúsculas y minúsculas.
- Especifique varios valores de cookie mediante la delimitación de cada uno con un espacio. 
- Los valores de cookie pueden aprovechar los valores de carácter comodín.
- Si no se ha especificado un valor de carácter comodín, solo una coincidencia exacta cumplirá esta condición de coincidencia. Por ejemplo, especificar que "Value" coincidirá con "Value", pero no con "Value1". 

### <a name="post-argument"></a>Argumento POST

**Campos obligatorios**

Nombre de argumento | Operator | Valor de argumento | Transformación de caso
--------------|----------|----------------|---------------
Cadena | [Lista de operadores estándar](#standard-operator-list) | Cadena, Int | Sin transformación, en mayúsculas, en minúsculas

### <a name="query-string"></a>Cadena de consulta

Las condiciones de coincidencia Cadena de consulta identifican las solicitudes que contienen un parámetro de cadena de consulta especificado. Este parámetro se establece en un valor que coincide con un patrón especificado. Los parámetros de cadena de consulta (por ejemplo, parameter=value) en la dirección URL de la solicitud determinan si se cumple esta condición. Esta condición de coincidencia identifica un parámetro de cadena de consulta por su nombre y acepta uno o varios valores para el valor del parámetro.

**Campos obligatorios**

Operator | Cadena de consulta | Transformación de caso
---------|--------------|---------------
[Lista de operadores estándar](#standard-operator-list) | Cadena, Int | Sin transformación, en mayúsculas, en minúsculas

### <a name="remote-address"></a>Dirección remota

Las condiciones de coincidencia Dirección remota identifican solicitudes en función de la ubicación del solicitante o la dirección IP.

**Campos obligatorios**

Operator | Valores admitidos
---------|-----------------
Any | N/D
Coincidencia geográfica | Códigos de país
Coincidencia de IP | Direcciones IP (espacio separado)
Ninguna | N/D
No tiene ninguna coincidencia geográfica | Códigos de país
No tiene ninguna coincidencia de IP | Direcciones IP (espacio separado)

Información importante:

- Utilice la notación CIDR.
- Especifique varias direcciones IP o bloques de direcciones IP al delimitarlas individualmente con un espacio. Por ejemplo:
  - **Ejemplo IPv4**: 1.2.3.4 10.20.30.40 coincide con las solicitudes que llegan desde la dirección 1.2.3.4 o 10.20.30.40.
  - **Ejemplo IPv6**: 1:2:3:4:5:6:7:8 10:20:30:40:50:60:70:80 coincide con las solicitudes que llegan desde la dirección 1:2:3:4:5:6:7:8 o 10:20:30:40:50:60:70:80.
- La sintaxis de un bloque de direcciones IP es la dirección IP de base seguida por una barra diagonal y el tamaño del prefijo. Por ejemplo:
  - **Ejemplo IPv4**: 5.5.5.64/26 coincide con las solicitudes que llegan desde la dirección 5.5.5.64 hasta la 5.5.5.127.
  - **Ejemplo IPv6**: 1:2:3:/48 coincide con las solicitudes que llegan desde la dirección 1:2:3:0:0:0:0:0 hasta la 1:2:3:ffff:ffff:ffff:ffff:ffff.

### <a name="request-body"></a>Cuerpo de la solicitud

**Campos obligatorios**

Operator | Cuerpo de la solicitud | Transformación de caso
---------|--------------|---------------
[Lista de operadores estándar](#standard-operator-list) | Cadena, Int | Sin transformación, en mayúsculas, en minúsculas

### <a name="request-header"></a>Encabezado de solicitud

**Campos obligatorios**
Nombre de encabezado | Operator | Valor de encabezado | Transformación de caso
------------|----------|--------------|---------------
Cadena | [Lista de operadores estándar](#standard-operator-list) | Cadena, Int | Sin transformación, en mayúsculas, en minúsculas

### <a name="request-method"></a>Método de solicitud

**Campos obligatorios**

Operator | Valor admitido
---------|----------------
Es igual a, No es igual a | GET, POST, PUT, DELETE, HEAD, OPTIONS, TRACE

Información importante:

- Solo el método de solicitud GET puede generar contenido almacenado en caché en la red CDN. Los demás métodos de solicitud los procesa el proxy mediante la red. 

### <a name="request-protocol"></a>Protocolo de solicitud

**Campos obligatorios**

Operator | Valor admitido
---------|----------------
Es igual a, No es igual a | HTTP, HTTPS

### <a name="request-url"></a>URL de la solicitud

**Campos obligatorios**

Operator | URL de la solicitud | Transformación de caso
---------|-------------|---------------
[Lista de operadores estándar](#standard-operator-list) | Cadena, Int | Sin transformación, en mayúsculas, en minúsculas

Información importante:

- Al escribir la dirección URL de la solicitud, asegúrese de incluir la información del protocolo. Por ejemplo, "https://www.[sudominio].com".

### <a name="url-file-extension"></a>Extensión de archivo de URL

**Campos obligatorios**

Operator | Extensión | Transformación de caso
---------|-----------|---------------
[Lista de operadores estándar](#standard-operator-list) | Cadena, Int | Sin transformación, en mayúsculas, en minúsculas

Información importante:

- En el caso de la extensión no incluya un punto inicial; por ejemplo, utilice html en lugar de .html.

### <a name="url-file-name"></a>Nombre de archivo de la dirección URL

**Campos obligatorios**

Operator | Nombre de archivo | Transformación de caso
---------|-----------|---------------
[Lista de operadores estándar](#standard-operator-list) | Cadena, Int | Sin transformación, en mayúsculas, en minúsculas

Información importante:

- Puede especificar varios nombres de archivo delimitando cada nombre de archivo con un único espacio. 

### <a name="url-path"></a>Ruta de acceso URL

**Campos obligatorios**

Operator | Valor | Transformación de caso
---------|-------|---------------
[Lista de operadores estándar](#standard-operator-list) | Cadena, Int | Sin transformación, en mayúsculas, en minúsculas

Información importante:

- Un valor de nombre de archivo puede aprovechar los valores de carácter comodín. Por ejemplo, cada patrón de nombre de archivo puede contener uno o varios asteriscos (*), donde cada asterisco coincide con una secuencia de uno o más caracteres.

## <a name="reference-for-rules-engine-match-conditions"></a>Referencia de las condiciones de coincidencia del motor de reglas

### <a name="standard-operator-list"></a>Lista de operadores estándar

En el caso de las reglas que contienen la lista de operadores estándar, los operadores siguientes son válidos:

- Any
- Equals 
- Contains 
- Empieza por 
- Termina por 
- Menor que
- Menor o igual que
- Mayor que
- Mayor o igual que
- Ninguno
- No contiene
- No empieza por 
- No termina con 
- No es menor que
- No es menor o igual que
- No es mayor que
- No es mayor o igual que

En el caso de los operadores numéricos, como "Menor que" o "Mayor o igual que", la comparación usada se basará en la longitud. En este caso, el valor de la condición de coincidencia debe ser un entero, igual a la longitud que quiere comparar. 

---

[Volver arriba](#match-conditions)

</br>

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a Azure Content Delivery Network](cdn-overview.md)
- [Referencia del motor de reglas](cdn-standard-rules-engine-reference.md)
- [Acciones del motor de reglas](cdn-standard-rules-engine-actions.md)
- [Aplicación de HTTPS mediante el motor de reglas estándar](cdn-standard-rules-engine.md)
