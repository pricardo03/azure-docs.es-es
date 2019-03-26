---
title: Reescribir encabezados HTTP en Azure Application Gateway | Microsoft Docs
description: En este artículo, se proporciona información general sobre la función para reescribir encabezados HTTP en Azure Application Gateway.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 12/20/2018
ms.author: absha
ms.openlocfilehash: 846f07051ee65a542b56624fa84a9bdc4ca0f4e6
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58418013"
---
# <a name="rewrite-http-headers-with-application-gateway-public-preview"></a>Reescribir encabezados HTTP a Application Gateway (versión preliminar pública)

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Los encabezados HTTP permiten que el cliente y el servidor pasen información adicional con la solicitud o la respuesta. La reescritura de estos encabezados HTTP permite alcanzar varios escenarios importantes, como agregar campos de encabezado relacionados con la seguridad (como HSTS/ X-XSS-Protection) o quitar campos de encabezado de respuesta que pueden revelar información confidencial, como el nombre del servidor back-end.

Ahora, Application Gateway admite la capacidad de reescribir los encabezados de las solicitudes HTTP entrantes, así como las respuestas HTTP salientes. Podrá agregar, quitar o actualizar los encabezados HTTP de solicitud y respuesta mientras los paquetes de solicitud/respuesta se mueven entre los grupos de back-end y de cliente. Puede reescribir tanto campos de encabezado estándar como no estándar.

> [!NOTE]
> 
> La compatibilidad con la reescritura de encabezados HTTP solo está disponible para la [nueva SKU [Standard_V2\]](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant)

La compatibilidad de reescritura de encabezados de Application Gateway ofrece lo siguiente:

- **Reescritura de encabezados global**: Se pueden reescribir encabezados específicos para todas las solicitudes y respuestas que pertenezcan al sitio.
- **Encabezado basados en la ruta de acceso de reescritura**: este tipo de reescritura permite la reescritura de encabezado de solo esas solicitudes y respuestas que pertenecen solo en un área de sitio específico, por ejemplo un área de carro de la compra que se indica con /cart/\*.

Con este cambio, necesita realizar lo siguiente:

1. Cree los objetos necesarios para reescribir los encabezados HTTP: 
   - **RequestHeaderConfiguration**: este objeto se usa para especificar los campos del encabezado de solicitud que va a reescribir y el nuevo valor con el que se reescribirán los encabezados originales.
   - **ResponseHeaderConfiguration**: este objeto se usa para especificar los campos del encabezado de respuesta que quiera reescribir y el nuevo valor con el que se reescribirán los encabezados originales.
   - **ActionSet**: este objeto contiene las configuraciones de los encabezados de solicitud y respuesta especificados anteriormente. 
   - **RewriteRule**: este objeto contiene todos los elementos *actionSets* especificados anteriormente. 
   - **RewriteRuleSet**: este objeto contiene todos los elementos *rewriteRules* y tendrá que conectarse a una regla de enrutamiento de solicitudes (básica o basada en ruta de acceso).
2. A continuación, tendrá que asociar el conjunto de reglas de reescritura a una regla de enrutamiento. Una vez creada, esta configuración de reescritura se conecta al agente de escucha de origen mediante la regla de enrutamiento. Al usar una regla de enrutamiento básica, la configuración de reescritura de encabezados se asocia a un agente de escucha de origen y es una reescritura de encabezados global. Al usar una regla de enrutamiento basada en rutas, la configuración de reescritura de encabezado se define en la asignación de la ruta de URL. Por lo tanto, solo se aplica al área específica de la ruta de acceso de un sitio.

Puede crear varios conjuntos de reglas de reescritura de encabezados HTTP y cada conjunto de reglas de reescritura se puede aplicar a varios agentes de escucha. Pero solo se puede aplicar un conjunto de reglas de reescritura HTTP a un agente de escucha específico.

Puede reescribir el valor en los encabezados a:

- Valor de texto. 

  *Ejemplo:* 

  ```azurepowershell-interactive
  $responseHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Strict-Transport-Security" -  HeaderValue "max-age=31536000")
  ```

- Valor de otro encabezado. 

  *Ejemplo 1:* 

  ```azurepowershell-interactive
  $requestHeaderConfiguration= New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "X-New-RequestHeader" -HeaderValue {http_req_oldHeader}
  ```

  > [!Note] 
  > Para especificar un encabezado de solicitud, necesita usar la sintaxis siguiente: {http_req_headerName}

  *Ejemplo 2*:

  ```azurepowershell-interactive
  $responseHeaderConfiguration= New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "X-New-ResponseHeader" -HeaderValue {http_resp_oldHeader}
  ```

  > [!Note] 
  > Para especificar un encabezado de respuesta, necesita usar la sintaxis siguiente: {http_resp_headerName}

- Valor de variables de servidor admitidas.

  *Ejemplo:* 

  ```azurepowershell-interactive
  $requestHeaderConfiguration = New-AzApplicationGatewayRewriteRuleHeaderConfiguration -HeaderName "Ciphers-Used" -HeaderValue "{var_ciphers_used}"
  ```

  > [!Note] 
  > Para especificar una variable de servidor, necesita usar la sintaxis siguiente: {var_serverVariable}

- Una combinación de las anteriores.

## <a name="server-variables"></a>Variables de servidor

Las variables de servidor almacenan información útil en un servidor web. Estas variables proporcionan información sobre el servidor, la conexión con el cliente y la solicitud actual en la conexión, como la dirección IP del cliente o el tipo de explorador web. Cambian dinámicamente; por ejemplo, cuando se carga una nueva página o se publica un formulario.  Con estas variables, los usuarios pueden establecer encabezados de solicitud, así como encabezados de respuesta. 

Esta función admite la reescritura de encabezados en las siguientes variables de servidor:

| Variables de servidor admitidas | DESCRIPCIÓN                                                  |
| -------------------------- | :----------------------------------------------------------- |
| ciphers_supported          | devuelve la lista de cifrados admitidos por el cliente.          |
| ciphers_used               | devuelve la cadena de cifrados usada para una conexión SSL establecida. |
| client_ip                  | Dirección IP del cliente; especialmente útil en escenarios donde los clientes se va a volver a escribir el encabezado X-Forwarded-For establecido por la puerta de enlace de aplicaciones, para que el encabezado contiene solo la dirección IP sin la información de puerto. |
| client_port                | puerto del cliente.                                                  |
| client_tcp_rtt             | información sobre la conexión TCP del cliente; disponible en sistemas que admiten la opción de socket TCP_INFO. |
| client_user                | al usar la autenticación HTTP, el nombre de usuario proporcionado para la autenticación. |
| host                       | en este orden de prioridad: nombre de host de la línea de la solicitud, nombre de host del campo de encabezado de la solicitud del “Host”, o bien el nombre del servidor que coincida con una solicitud. |
| cookie_*name*              | el *nombre* cookie |
| http_method                | método usado para realizar la solicitud de URL. Por ejemplo, GET, POST, etc. |
| http_status                | estado de la sesión, por ejemplo: 200, 400, 403, etc.                       |
| http_version               | protocolo de la solicitud, que suele ser “HTTP/1.0”, “HTTP/1.1” o “HTTP/2.0”. |
| query_string               | la lista de pares de variable-valor que aparecen después de “?” en la URL solicitada. |
| received_bytes             | longitud de la solicitud (incluida la línea de la solicitud, el encabezado y el cuerpo de la solicitud). |
| request_query              | argumentos en la línea de la solicitud.                                |
| request_scheme             | esquema de la solicitud (“HTTP” o “HTTPS”).                            |
| request_uri                | URI original completo de la solicitud (con argumentos).                   |
| sent_bytes                 | número de bytes enviados a un cliente.                             |
| server_port                | puerto del servidor que ha aceptado una solicitud.                 |
| ssl_connection_protocol    | devuelve el protocolo de una conexión SSL establecida.        |
| ssl_enabled                | “on” (activado) si la conexión opera en el modo SSL; de lo contrario, una cadena vacía. |

## <a name="limitations"></a>Limitaciones

- Esta función para reescribir encabezados HTTP solo está disponible actualmente mediante Azure PowerShell, la API de Azure y Azure SDK. La compatibilidad mediante el portal y la CLI de Azure estará disponible en breve.

- La compatibilidad de la reescritura de encabezados HTTP solo se admite en la nueva SKU [Standard_V2](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant). La función no se admitirá en las SKU anteriores.

- Aún no se admite la reescritura de los encabezados de conexión, actualización y hospedaje.

- La función para reescribir de forma condicional los encabezados HTTP estará disponible en breve.

- Los nombres de encabezado solo pueden contener caracteres alfanuméricos y símbolos específicos definidos en [RFC 7230](https://tools.ietf.org/html/rfc7230#page-27). Pero actualmente no se admite el carácter especial de subrayado (\_) en el nombre de encabezado. 

## <a name="need-help"></a>¿Necesita ayuda?

Si necesita ayuda con esta función, póngase en contacto con nosotros en [AGHeaderRewriteHelp@microsoft.com](mailto:AGHeaderRewriteHelp@microsoft.com).

## <a name="next-steps"></a>Pasos siguientes

Después de obtener información sobre la función para reescribir encabezados HTTP, vaya a [Crear una instancia de Application Gateway de escalado automático y redundancia de zona que reescriba encabezados HTTP](tutorial-http-header-rewrite-powershell.md) o [Reescribir encabezados SCP en una instancia de Application Gateway existente de escalado automático y con redundancia de zona](add-http-header-rewrite-rule-powershell.md).
