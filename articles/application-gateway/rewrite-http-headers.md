---
title: Reescribir encabezados HTTP en Azure Application Gateway | Microsoft Docs
description: En este artículo, se proporciona información general sobre la función para reescribir encabezados HTTP en Azure Application Gateway.
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 04/11/2019
ms.author: absha
ms.openlocfilehash: 20c484779e7ffe74ae01e33472b4cf8761d81b66
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59682687"
---
# <a name="rewrite-http-headers-with-application-gateway"></a>Vuelva a escribir los encabezados HTTP con Application Gateway

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Los encabezados HTTP permiten que el cliente y el servidor pasen información adicional con la solicitud o la respuesta. Volver a escribir estos encabezados HTTP le ayudará a realizar varios escenarios importantes como la adición de campos de encabezado relacionados con la seguridad como HSTS / X-XSS-Protection Quitar encabezado de respuesta de los campos que puede revelar información confidencial, quitando información del puerto desde Encabezados X-Forwarded-For, etcetera. Application gateway admite la capacidad de agregar, quitar o actualizar los encabezados de solicitud y respuesta HTTP mientras la solicitud y los paquetes de respuesta se mueven entre los grupos de cliente y el back-end. Proporciona la funcionalidad para agregar condiciones para asegurarse de que los encabezados especificados se vuelven a escribir sólo cuando se cumplen ciertas condiciones. La capacidad también admite varios [variables de servidor](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers#server-variables) que ayuda a almacenar información adicional acerca de las solicitudes y respuestas, lo que permite que las reglas de reescritura eficaces.
> [!NOTE]
>
> La compatibilidad con la reescritura de encabezados HTTP solo está disponible para la [nueva SKU [Standard_V2\]](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant)

![Reescritura de encabezados](media/rewrite-http-headers/rewrite-headers.png)

## <a name="headers-supported-for-rewrite"></a>Vuelva a escribir los encabezados compatibles con

La capacidad le permite volver a escribir todos los encabezados de solicitud y respuesta de bloqueo de los encabezados de Host, la conexión y la actualización. También puede usar la puerta de enlace de aplicaciones para crear los encabezados personalizados y agregarlos a la solicitud y las respuestas que se enrutan a través de él. 

## <a name="rewrite-conditions"></a>Condiciones de reescritura

Uso de la reescritura condiciones puede evaluar el contenido de las respuestas y solicitudes HTTP (S) y realizar un encabezado de reescritura solo cuando se cumplen una o varias condiciones. Los siguientes 3 tipos de variables se utilizan la puerta de enlace de aplicaciones para evaluar el contenido de las respuestas y solicitudes HTTP (S):

- Encabezados HTTP en la solicitud
- Encabezados HTTP en la respuesta
- Variables de servidor de puerta de enlace de aplicaciones

Una condición puede usarse para evaluar si la variable especificada está presente, si la variable especificada coincide exactamente con un valor específico, o si la variable especificada coincide exactamente con un patrón específico. [Biblioteca de expresiones regulares compatibles (PCRE) de Perl](https://www.pcre.org/) se utiliza para implementar en las condiciones de coincidencia de patrones de expresión regular. Para obtener información sobre la sintaxis de expresiones regulares, vea el [expresiones regulares de Perl man página](https://perldoc.perl.org/perlre.html).

## <a name="rewrite-actions"></a>Acciones de reescritura

Vuelva a escribir las acciones se usan para especificar los encabezados de solicitud y respuesta que se va a escribir y el nuevo valor que deben volver a escribir en los encabezados del original. Puede crear un nuevo encabezado, modifique el valor de un encabezado existente o eliminar un encabezado existente. El valor de un nuevo encabezado o un encabezado existente puede establecerse en los siguientes tipos de valores:

- Texto 
- Encabezado de solicitud: Para especificar un encabezado de solicitud, deberá usar la sintaxis {http_req_*headerName*}
- Encabezado de respuesta: Para especificar un encabezado de respuesta, deberá usar la sintaxis {http_resp_*headerName*}
- Variable de servidor: Para especificar una variable de servidor, deberá usar la sintaxis {var_*serverVariable*}
- Combinación de texto, el encabezado de solicitud, el encabezado de respuesta y una variable de servidor.

## <a name="server-variables"></a>Variables de servidor

Application gateway usa variables de servidor para almacenar información útil acerca del servidor, la conexión con el cliente y la solicitud actual en la conexión, como dirección IP del cliente o el tipo de explorador web. Estas variables se cambian dinámicamente, como cuando se carga una nueva página o un formulario se publica. Puede usar estas variables de servidor para evaluar las condiciones de reescritura y vuelva a escribir los encabezados. 

Application gateway admite las siguientes variables de servidor:

| Variables de servidor admitidas | DESCRIPCIÓN                                                  |
| -------------------------- | :----------------------------------------------------------- |
| add_x_forwarded_for_proxy  | Contiene la "X-Forwarded-For" cliente solicitud campo de cabecera con la `client_ip` (se explica en la siguiente tabla) variable anexado a él en el formato (IP1, IP2, IP3,...). Si el campo "X-Forwarded-For" no está presente en el encabezado de solicitud de cliente, el `add_x_forwarded_for_proxy` variable es igual a la `$client_ip` variable. Esta variable es especialmente útil en escenarios donde los clientes se va a volver a escribir el encabezado X-Forwarded-For establecido por la puerta de enlace de aplicaciones, tal que el encabezado contiene solo la dirección IP sin la información de puerto. |
| ciphers_supported          | devuelve la lista de cifrados admitidos por el cliente.          |
| ciphers_used               | devuelve la cadena de cifrados usada para una conexión SSL establecida. |
| client_ip                  | Dirección IP del cliente desde el que la puerta de enlace de la aplicación recibió la solicitud. Si hay un proxy inverso antes de la puerta de enlace de aplicaciones y el cliente de origen, a continuación, *client_ip* devolverá la dirección IP del proxy inverso. |
| client_port                | puerto del cliente.                                                  |
| client_tcp_rtt             | información sobre la conexión TCP del cliente; disponible en sistemas que admiten la opción de socket TCP_INFO. |
| client_user                | al usar la autenticación HTTP, el nombre de usuario proporcionado para la autenticación. |
| host                       | en este orden de prioridad: nombre de host de la línea de la solicitud, nombre de host del campo de encabezado de la solicitud del “Host”, o bien el nombre del servidor que coincida con una solicitud. |
| cookie_*name*              | el *nombre* cookie                                            |
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

## <a name="rewrite-configuration"></a>Configuración de reescritura

Para configurar la reescritura del encabezado HTTP, deberá:

1. Cree los objetos necesarios para reescribir los encabezados HTTP:

   - **Vuelva a escribir la acción**: se utiliza para especificar la solicitud y los campos de encabezado de solicitud que va a escribir y el nuevo valor que deben volver a escribir en los encabezados del original. Puede asociar uno o más de reescritura de condición con una acción de reescritura.

   - **Vuelva a escribir la condición**: Es una configuración opcional. Si se agrega una condición de reescritura, se evaluará el contenido de las respuestas y solicitudes HTTP (S). Si la solicitud HTTP (S) o la respuesta coincide con la condición de reescritura, se basará la decisión de ejecutar la acción de reescritura asociada con la condición de reescritura. 

     Si más de una de las condiciones asociadas con una acción y, a continuación, la acción se ejecutará sólo cuando se cumplen todas las condiciones, es decir, se realizará una operación AND lógica.

   - **La regla de reescritura**: regla de reescritura contiene varias acciones de reescritura - vuelva a escribir las combinaciones de condición.

   - **Secuencia de regla**: ayuda a determinar el orden en que las distintas reglas de reescritura se ejecutan. Esto resulta útil cuando hay varias reglas de reescritura en un conjunto de reescritura. La regla de reescritura con el menor valor de secuencia de la regla se ejecuta en primer lugar. Si proporciona la misma secuencia de regla para dos reglas de reescritura, a continuación, el orden de ejecución será no determinista.

   - **Vuelva a escribir conjunto**: contiene varias reglas de reescritura que se van a asociadas a una regla de enrutamiento de solicitud.

2. Será necesario para asociar el conjunto de reescritura (*rewriteRuleSet*) con una regla de enrutamiento. Esto es porque la configuración de reescritura se ha asociado al agente de escucha de origen a través de la regla de enrutamiento. Al usar una regla de enrutamiento básica, la configuración de reescritura de encabezados se asocia a un agente de escucha de origen y es una reescritura de encabezados global. Al usar una regla de enrutamiento basada en rutas, la configuración de reescritura de encabezado se define en la asignación de la ruta de URL. Por lo tanto, solo se aplica al área específica de la ruta de acceso de un sitio.

Puede crear varios conjuntos de reescritura de encabezado de http y cada conjunto de reescritura se puede aplicar a varios agentes de escucha. Sin embargo, puede aplicar sólo una reescritura establecido en un agente de escucha concreto.

## <a name="common-scenarios"></a>Escenarios comunes

A continuación se indican algunos de los escenarios comunes que requieren reescritura de encabezado.

### <a name="remove-port-information-from-the-x-forwarded-for-header"></a>Quitar la información del puerto desde el encabezado X-Forwarded-For

Application gateway inserta el encabezado X-Forwarded-For a todas las solicitudes antes de reenviar las solicitudes de back-end. El formato para este encabezado es una lista separada por comas de IP: Port. Sin embargo, puede haber escenarios donde los servidores back-end requieren el encabezado contenga solo las direcciones IP. Para llevar a cabo estos escenarios, la reescritura de encabezado puede utilizarse para quitar la información del puerto desde el encabezado X-Forwarded-For. Una manera de hacerlo es establecer el encabezado en la variable de servidor add_x_forwarded_for_proxy. 

![Quitar puerto](media/rewrite-http-headers/remove-port.png)

### <a name="modify-the-redirection-url"></a>Modificar la dirección URL de redireccionamiento

Cuando una aplicación de back-end envía una respuesta de redirección, es posible que desee redirigir al cliente a una dirección URL diferente a la especificada por la aplicación de back-end. Uno de estos escenarios es cuando un servicio de aplicación se hospeda detrás de una puerta de enlace de la aplicación y requiere que el cliente se realiza una redirección a su ruta de acceso relativa (redirección desde contoso.azurewebsites.net/path1 a contoso.azurewebsites.net/path2). 

Puesto que app service es un servicio multiempresa, utiliza el encabezado de host en la solicitud para enrutar al extremo correcto. Servicios de aplicación tienen un nombre de dominio predeterminado *. azurewebsites.net (por ejemplo, contoso.azurewebsites.net), que es distinto del nombre de dominio de la aplicación la puerta de enlace (por ejemplo, contoso.com). Puesto que la solicitud original desde el cliente tiene el nombre de dominio de contoso.com la puerta de enlace de aplicación como nombre de host, la puerta de enlace de la aplicación cambia el nombre de host para contoso.azurewebsites.net, para que el servicio de la aplicación puede dirigirlo al punto de conexión correcto. Cuando el servicio de la aplicación envía una respuesta de redirección, usa el mismo nombre de host en el encabezado location de la respuesta que aparece en la solicitud que recibe de la puerta de enlace de la aplicación. Por lo tanto, el cliente pueda realizar la solicitud directamente a contoso.azurewebsites.net/path2, en lugar de pasar a través de la puerta de enlace de aplicaciones (contoso.com/path2). Omitiendo la puerta de enlace de la aplicación no es deseable. 

Este problema puede resolverse configurando el nombre de host en el encabezado de ubicación al nombre de dominio de la aplicación la puerta de enlace. Para ello, puede crear una regla de reescritura con una condición que evalúa si el encabezado de ubicación en la respuesta contiene azurewebsites.net escribiendo `(https?):\/\/.*azurewebsites\.net(.*)$` como el patrón y realiza una acción para volver a escribir el encabezado de ubicación para que la puerta de enlace de aplicaciones nombre de host escribiendo `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` como el valor del encabezado.

![Modificar encabezado de ubicación](media/rewrite-http-headers/app-service-redirection.png)

### <a name="implement-security-http-headers-to-prevent-vulnerabilities"></a>Implementar los encabezados HTTP de seguridad para evitar las vulnerabilidades

Varias vulnerabilidades de seguridad pueden corregirse mediante la implementación de encabezados necesarios en la respuesta de la aplicación. Algunos de estos encabezados de seguridad son X-XSS-Protection, Strict-Transport-Security, Content-Security-Policy, etcetera. Puede usar la puerta de enlace de aplicación para establecer estos encabezados para todas las respuestas.

![Encabezado de seguridad](media/rewrite-http-headers/security-header.png)

### <a name="delete-unwanted-headers"></a>Eliminar encabezados no deseados

Es posible que desee quitar esos encabezados de la respuesta HTTP que se revele información confidencial, como el nombre del servidor back-end, el sistema operativo, detalles de biblioteca, etcetera. Puede usar la puerta de enlace de aplicaciones para quitarlas.

![Eliminar encabezado](media/rewrite-http-headers/remove-headers.png)

### <a name="check-presence-of-a-header"></a>Compruebe la presencia de un encabezado

Puede evaluar el encabezado de solicitud o respuesta HTTP de la presencia de una variable de encabezado o el servidor. Esto es útil cuando quiere realizar una reescritura de encabezado solo cuando está presente un encabezado determinado.

![Comprobando la presencia de un encabezado](media/rewrite-http-headers/check-presence.png)

## <a name="limitations"></a>Limitaciones

- Volver a escribir los encabezados de la conexión, actualización y Host no se admite todavía.

- Los nombres de encabezado solo pueden contener caracteres alfanuméricos y símbolos específicos definidos en [RFC 7230](https://tools.ietf.org/html/rfc7230#page-27). Pero actualmente no se admite el carácter especial de subrayado (\_) en el nombre de encabezado. 

## <a name="need-help"></a>¿Necesita ayuda?

Si necesita ayuda con esta función, póngase en contacto con nosotros en [AGHeaderRewriteHelp@microsoft.com](mailto:AGHeaderRewriteHelp@microsoft.com).

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo volver a escribir los encabezados HTTP, vea:

-  [Vuelva a escribir los encabezados HTTP mediante Azure portal](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers-portal)
-  [Vuelva a escribir los encabezados HTTP con Azure PowerShell](add-http-header-rewrite-rule-powershell.md)
