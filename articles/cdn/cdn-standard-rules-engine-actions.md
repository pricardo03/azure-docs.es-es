---
title: Acciones del motor de reglas estándar de Azure CDN de Microsoft | Microsoft Docs
description: Documentación de referencia para las acciones del motor de reglas estándar de Azure CDN de Microsoft.
services: cdn
author: mdgattuso
ms.service: azure-cdn
ms.topic: article
ms.date: 11/01/2019
ms.author: magattus
ms.openlocfilehash: dbde93cc7ffd21e341653407e6e4f910e4620974
ms.sourcegitcommit: b2fb32ae73b12cf2d180e6e4ffffa13a31aa4c6f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73615851"
---
# <a name="azure-cdn-from-microsoft-standard-rules-engine-actions"></a>Acciones del motor de reglas estándar de Azure CDN de Microsoft

En este artículo se muestran descripciones detalladas de las acciones disponibles para el [motor de reglas estándar](cdn-standard-rules-engine.md) de Azure Content Delivery Network (CDN) de Microsoft.

La segunda parte de una regla es una acción. Una acción define el comportamiento que se aplica al tipo de solicitud que identifica un conjunto de condiciones de coincidencia.

## <a name="actions"></a>Acciones

Están disponibles para su uso las siguientes soluciones. 

## <a name="cache-expiration"></a>Expiración de la caché

Esta acción permite sobrescribir el TTL del punto de conexión para las solicitudes que especifican las condiciones de coincidencia de reglas.

**Campos obligatorios**

Comportamiento de la caché |                
---------------|----------------
Omisión de la memoria caché | Cuando se selecciona esta opción y la regla coincide, el contenido no se almacenará en caché.
Invalidar | Cuando se selecciona esta opción y la regla coincide, el valor de TTL devuelto desde el origen se sobrescribirá con el valor especificado en la acción.
Establecer si falta | Cuando se selecciona esta opción y la regla coincide, si no ha habido ningún valor de TTL devuelto desde el origen, la regla establecerá el TTL con el valor especificado en la acción.

**Campos adicionales**

Days (Días) | Horas | Minutos | Segundos
-----|-------|---------|--------
Int | Int | Int | Int 

## <a name="cache-key-query-string"></a>Cadena de consulta de clave de caché

Esta acción le permite modificar la clave de caché en función de las cadenas de consulta.

**Campos obligatorios**

Comportamiento | DESCRIPCIÓN
---------|------------
Include | Cuando se selecciona esta opción y la regla coincide, las cadenas de consulta especificadas en los parámetros se incluirán al generar la clave de caché. 
Almacenar en caché todas las URL únicas | Cuando se selecciona esta opción y la regla coincide, cada dirección URL única tendrá su propia clave de caché. 
Exclude | Cuando se selecciona esta opción y la regla coincide, las cadenas de consulta especificadas en los parámetros se excluirán al generar la clave de caché.
Pasar por alto las cadenas de consulta | Cuando se selecciona esta opción y la regla coincide, las cadenas de consulta no se tendrán en cuenta al generar la clave de caché. 

## <a name="modify-request-header"></a>Modificación del encabezado de solicitud

Esta acción permite modificar los encabezados presentes en las solicitudes enviadas a su origen.

**Campos obligatorios**

. | Nombre del encabezado HTTP | Valor
-------|------------------|------
Append | Cuando se selecciona esta opción y la regla coincide, el encabezado especificado en el nombre del encabezado se agregará a la solicitud con el valor especificado. Si el encabezado ya está presente, este valor se anexará al valor existente. | Cadena
Sobrescribir | Cuando se selecciona esta opción y la regla coincide, el encabezado especificado en el nombre del encabezado se agregará a la solicitud con el valor especificado. Si el encabezado ya está presente, este valor se sobrescribirá al valor existente. | Cadena
Eliminar | Cuando se selecciona esta opción, la regla coincide y el encabezado especificado en la regla está presente, se eliminará de la solicitud. | Cadena

## <a name="modify-response-header"></a>Modificación del encabezado de respuesta

Esta acción permite modificar los encabezados presentes en las respuestas devueltas a los clientes finales.

**Campos obligatorios**

. | Nombre del encabezado HTTP | Valor
-------|------------------|------
Append | Cuando se selecciona esta opción y la regla coincide, el encabezado especificado en el nombre del encabezado se agregará a la respuesta con el valor especificado. Si el encabezado ya está presente, este valor se anexará al valor existente. | Cadena
Sobrescribir | Cuando se selecciona esta opción y la regla coincide, el encabezado especificado en el nombre del encabezado se agregará a la respuesta con el valor especificado. Si el encabezado ya está presente, este valor se sobrescribirá al valor existente. | Cadena
Eliminar | Cuando se selecciona esta opción, la regla coincide y el encabezado especificado en la regla está presente, se eliminará de la respuesta. | Cadena

## <a name="url-redirect"></a>Redirección de URL

Esta acción permite redirigir a los clientes finales a una nueva dirección URL. 

**Campos obligatorios**

Campo | DESCRIPCIÓN 
------|------------
type | Seleccione el tipo de respuesta que se devolverá al solicitante. Las opciones son: 302 Encontrado, 301 Movido, 307 Redirección temporal y 308 Redirección permanente.
Protocolo | Confrontar solicitud, HTTP o HTTPS.
Nombre de host. | Seleccione el nombre de host al que se redirigirá la solicitud. Déjelo en blanco para conservar el host entrante.
Path | Defina la ruta de acceso que se va a usar en la redirección. Déjela en blanco para conservar la ruta de acceso entrante.  
Cadena de consulta | Defina la cadena de consulta utilizada en la redirección. Déjela en blanco para conservar la cadena de consulta entrante. 
Fragmento | Defina el fragmento que se va a usar en la redirección. Déjelo en blanco para conservar el fragmento entrante. 

Es muy recomendable usar una dirección URL absoluta. El uso de direcciones URL relativas podría redirigir direcciones URL de la red CDN a rutas de acceso no válidas. 

## <a name="url-rewrite"></a>Reescritura de direcciones URL

Esta acción permite reescribir la ruta de acceso de una solicitud en camino en el origen.

**Campos obligatorios**

Campo | DESCRIPCIÓN 
------|------------
Patrón de origen | Defina el patrón de origen en la ruta de acceso URL que se va a reemplazar. Actualmente, el patrón de origen usa una coincidencia basada en el prefijo. Para que coincida con todas las rutas de acceso de las direcciones URL, use "/" como valor de patrón de origen.
Destination | Defina la ruta de acceso de destino que se va a usar en la reescritura. Esto sobrescribirá el patrón de origen.
Conservar la ruta de acceso sin coincidencia | En caso afirmativo, el resto de la ruta de acceso después del patrón de origen se anexará a la nueva ruta de acceso de destino. 


[Volver arriba](#actions)

</br>

## <a name="next-steps"></a>Pasos siguientes

- [Introducción a Azure Content Delivery Network](cdn-overview.md)
- [Referencia del motor de reglas](cdn-standard-rules-engine-reference.md)
- [Condiciones de coincidencia del motor de reglas](cdn-standard-rules-engine-match-conditions.md)
- [Aplicación de HTTPS mediante el motor de reglas estándar](cdn-standard-rules-engine.md)
