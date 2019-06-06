---
title: Vuelva a escribir los encabezados HTTP con Azure Application Gateway | Microsoft Docs
description: Este artículo proporciona información general de reescritura de encabezados HTTP en Azure Application Gateway
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 04/29/2019
ms.author: absha
ms.openlocfilehash: 9160d300270bf1ab5043bee632d27bcc4b7bf332
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66476041"
---
# <a name="rewrite-http-headers-with-application-gateway"></a>Vuelva a escribir los encabezados HTTP con Application Gateway

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Encabezados HTTP permiten que un cliente y el servidor pasar información adicional con una solicitud o respuesta. Al volver a escribir estos encabezados, puede realizar tareas importantes, como la adición de campos de encabezado relacionados con la seguridad como HSTS / X-XSS-Protection, quitar campos de encabezado de respuesta que podrían revelar información confidencial y quitar la información del puerto desde Encabezados X-Forwarded-For.

Instancia de Application Gateway le permite agregar, quitar o actualizar los encabezados de solicitud y respuesta HTTP mientras la solicitud y mover paquetes de respuesta entre el cliente y los grupos de back-end. Y permite agregar condiciones para asegurarse de que los encabezados especificados se vuelven a escribir sólo cuando se cumplen ciertas condiciones.

Instancia de Application Gateway también admite varios [variables de servidor](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers#server-variables) que ayudan a almacenar información adicional acerca de las solicitudes y respuestas. Esto facilita la creación de reglas de reescritura eficaces.

> [!NOTE]
>
> Compatibilidad con la reescritura de encabezados de HTTP solo está disponible para el [Standard_V2 y SKU WAF_v2](application-gateway-autoscaling-zone-redundant.md).

![Reescritura de encabezados](media/rewrite-http-headers/rewrite-headers.png)

## <a name="supported-headers"></a>Encabezados admitidos

Puede volver a escribir todos los encabezados de solicitudes y respuestas, excepto para los encabezados de Host, la conexión y la actualización. También puede usar la puerta de enlace de aplicaciones para crear los encabezados personalizados y agregarlos a las solicitudes y respuestas que se enrutan a través de él.

## <a name="rewrite-conditions"></a>Condiciones de reescritura

Puede utilizar condiciones de reescritura para evaluar el contenido de las solicitudes HTTP (S) y las respuestas y realizar una reescritura del encabezado cuando uno o más condiciones se cumplen. La puerta de enlace de la aplicación utiliza estos tipos de variables para evaluar el contenido de respuestas y solicitudes HTTP (S):

- Encabezados HTTP en la solicitud.
- Encabezados HTTP en la respuesta.
- Variables de servidor de puerta de enlace de aplicaciones.

Puede usar una condición para evaluar si una variable especificada está presente, si una variable especificada coincide con un valor específico, o si una variable especificada coincide con un patrón específico. Usa el [biblioteca de expresiones regulares compatibles (PCRE) de Perl](https://www.pcre.org/) para configurar las condiciones de coincidencia de patrón de expresión regular. Para obtener información sobre la sintaxis de expresiones regulares, vea el [página principal de las expresiones regulares de Perl](https://perldoc.perl.org/perlre.html).

## <a name="rewrite-actions"></a>Acciones de reescritura

Usar acciones de reescritura para especificar los encabezados de solicitud y respuesta que desea volver a escribir y el nuevo valor para los encabezados. Puede crear un nuevo encabezado, modifique el valor de un encabezado existente o eliminar un encabezado existente. El valor de un nuevo encabezado o un encabezado existente se puede establecer para estos tipos de valores:

- Texto.
- Encabezado de la solicitud. Para especificar un encabezado de solicitud, debe usar la sintaxis {http_req_*headerName*}.
- Encabezado de respuesta. Para especificar un encabezado de respuesta, deberá usar la sintaxis {http_resp_*headerName*}.
- Variable de servidor. Para especificar una variable de servidor, deberá usar la sintaxis {var_*serverVariable*}.
- Una combinación de texto, un encabezado de solicitud, un encabezado de respuesta y una variable de servidor.

## <a name="server-variables"></a>Variables de servidor

Application Gateway utiliza las variables de servidor para almacenar información útil sobre el servidor, la conexión con el cliente y la solicitud actual en la conexión. Dirección IP del cliente y el tipo de explorador web son ejemplos de información almacenada. Variables de servidor cambian dinámicamente, por ejemplo, cuando se carga una página nueva o cuando se publica un formulario. Puede usar estas variables para evaluar las condiciones de reescritura y vuelva a escribir los encabezados.

Application gateway admite estas variables de servidor:

| Nombre de la variable | DESCRIPCIÓN                                                  |
| -------------------------- | :----------------------------------------------------------- |
| add_x_forwarded_for_proxy  | El campo de encabezado de solicitud de cliente X-Forwarded-For con la `client_ip` variable (consulte la explicación más adelante en esta tabla) anexado a él en el formato IP1, IP2, IP3 y así sucesivamente. Si el campo X-Forwarded-For no se encuentra en el encabezado de solicitud de cliente, el `add_x_forwarded_for_proxy` variable es igual a la `$client_ip` variable. Esta variable es especialmente útil cuando desee volver a escribir el encabezado X-Forwarded-For establecido por la puerta de enlace de aplicaciones para que el encabezado contiene solo la dirección IP sin la información de puerto. |
| ciphers_supported          | Una lista de los cifrados compatibles con el cliente.          |
| ciphers_used               | La cadena de cifrado que se utiliza para una conexión SSL establecida. |
| client_ip                  | La dirección IP del cliente desde el que la puerta de enlace de la aplicación recibió la solicitud. Si hay un proxy inverso antes de la puerta de enlace de aplicaciones y el cliente de origen, *client_ip* devolverá la dirección IP del proxy inverso. |
| client_port                | El puerto del cliente.                                                  |
| client_tcp_rtt             | Información sobre la conexión TCP de cliente. Está disponible en los sistemas que admiten la opción de socket TCP_INFO. |
| client_user                | Cuando se usa la autenticación de HTTP, el nombre de usuario proporcionado para la autenticación. |
| host                       | En este orden de prioridad: el nombre de host desde la línea de solicitud, el nombre de host desde el campo de encabezado de solicitud de Host o el nombre del servidor una solicitud de coincidencia. |
| cookie_*name*              | El *nombre* cookie.                                            |
| http_method                | el método utilizado para realizar la solicitud de dirección URL. Por ejemplo, GET o POST. |
| http_status                | El estado de sesión. Por ejemplo, 200, 400 o 403.                       |
| http_version               | El protocolo de solicitud. Normalmente, HTTP/1.0, HTTP/1.1 o 2.0/HTTP. |
| query_string               | La lista de pares clave-valor en la variable que sigue a la "?" en la dirección URL solicitada. |
| received_bytes             | La longitud de la solicitud (incluidos la línea de solicitud, el encabezado y el cuerpo de la solicitud). |
| request_query              | Los argumentos de la línea de solicitud.                                |
| request_scheme             | El esquema de solicitud: http o https.                            |
| request_uri                | La solicitud original completo URI (con argumentos).                   |
| sent_bytes                 | El número de bytes enviados a un cliente.                             |
| server_port                | El puerto del servidor que aceptó una solicitud.                 |
| ssl_connection_protocol    | El protocolo de una conexión SSL establecido.        |
| ssl_enabled                | "On" si la conexión funciona en modo SSL. En caso contrario, una cadena vacía. |

## <a name="rewrite-configuration"></a>Configuración de reescritura

Para configurar la reescritura del encabezado HTTP, es preciso completar estos pasos.

1. Crear los objetos que son necesarios para la reescritura del encabezado HTTP:

   - **Vuelva a escribir la acción**: Se usa para especificar la solicitud y el nuevo valor para los encabezados y los campos de encabezado de solicitud que desea volver a escribir. Puede asociar uno o más condiciones con una acción de reescritura de reescritura.

   - **Vuelva a escribir la condición**: Una configuración opcional. Condiciones de reescritura evalúan el contenido de las respuestas y solicitudes HTTP (S). Si la solicitud HTTP (S) o la respuesta coincide con la condición de reescritura, se producirá la acción de reescritura.

     Si más de una condición se asocia con una acción, se produce la acción solo cuando se cumplen todas las condiciones. En otras palabras, la operación es una operación AND lógica.

   - **La regla de reescritura**: Contiene varias acciones de reescritura o vuelva a escribir las combinaciones de condición.

   - **Secuencia de regla**: Ayuda a determina el orden en el que se ejecutan las reglas de reescritura. Esta configuración es útil cuando haya varias reglas de reescritura en un conjunto de reescritura. Una regla de reescritura que tiene un valor de secuencia más bajo de regla se ejecuta primera. Si asigna la misma secuencia de la regla a dos reglas de reescritura, el orden de ejecución es no determinista.

   - **Vuelva a escribir conjunto**: Contiene varias reglas de reescritura que se asociarán con una regla de enrutamiento de solicitud.

2. Asociar el conjunto de reescritura (*rewriteRuleSet*) a una regla de enrutamiento. La configuración de reescritura se adjunta al agente de escucha de origen a través de la regla de enrutamiento. Cuando se usa una regla de enrutamiento básica, la configuración de reescritura de encabezado está asociada con un agente de escucha de origen y es una reescritura encabezado global. Cuando se usa una regla de enrutamiento basada en ruta de acceso, la configuración de reescritura de encabezado se define en el mapa de ruta de acceso de dirección URL. En ese caso, se aplica solo al área de ruta de acceso específica de un sitio.

Puede crear varios conjuntos de reescritura de encabezado HTTP y aplicar cada reescritura establecido en varios agentes de escucha. Pero puede aplicar sólo una reescritura establecido en un agente de escucha concreto.

## <a name="common-scenarios"></a>Escenarios comunes

Estos son algunos escenarios habituales para usar la reescritura de encabezado.

### <a name="remove-port-information-from-the-x-forwarded-for-header"></a>Quitar la información del puerto desde el encabezado X-Forwarded-For

Instancia de Application Gateway inserta un encabezado X-Forwarded-For en todas las solicitudes antes de reenviar las solicitudes de back-end. Este encabezado es una lista separada por comas de puertos IP. Puede haber escenarios en que los servidores back-end necesitan solo los encabezados que se va a contener las direcciones IP. Puede usar la reescritura de encabezado para quitar la información del puerto desde el encabezado X-Forwarded-For. Una manera de hacerlo es establecer el encabezado en la variable de servidor add_x_forwarded_for_proxy:

![Quitar puerto](media/rewrite-http-headers/remove-port.png)

### <a name="modify-a-redirection-url"></a>Modificar una dirección URL de redirección

Cuando una aplicación back-end envía una respuesta de redirección, es posible que desee redirigir al cliente a una dirección URL diferente a la especificada por la aplicación de back-end. Por ejemplo, es posible que desee hacer esto cuando un servicio de aplicación se hospeda detrás de una puerta de enlace de la aplicación y requiere que el cliente realizar una redirección a su ruta de acceso relativa. (Por ejemplo, un redireccionamiento de contoso.azurewebsites.net/path1 a contoso.azurewebsites.net/path2.)

Dado que App Service es un servicio multiempresa, utiliza el encabezado de host en la solicitud para enrutar la solicitud al extremo correcto. Servicios de aplicación tienen un nombre de dominio predeterminado *. azurewebsites.net (por ejemplo, contoso.azurewebsites.net) que es diferente del nombre de dominio de la aplicación la puerta de enlace (por ejemplo, contoso.com). Dado que la solicitud original desde el cliente tiene el nombre de dominio (contoso.com) de la aplicación la puerta de enlace como el nombre de host, la puerta de enlace de la aplicación cambia el nombre de host para contoso.azurewebsites.net. Realiza este cambio para que el servicio de aplicación puede enrutar la solicitud al extremo correcto.

Cuando el servicio de la aplicación envía una respuesta de redirección, usa el mismo nombre de host en el encabezado location de la respuesta que aparece en la solicitud que recibe de la puerta de enlace de la aplicación. Por lo que el cliente pueda realizar la solicitud directamente a contoso.azurewebsites.net/path2 en lugar de pasar a través de la puerta de enlace de aplicaciones (contoso.com/path2). Omitiendo la puerta de enlace de la aplicación no es deseable.

Para resolver este problema, puede establecer el nombre de host en el encabezado de ubicación al nombre de dominio de la aplicación la puerta de enlace.

Estos son los pasos para reemplazar el nombre de host:

1. Crear una regla de reescritura con una condición que evalúa si el encabezado de ubicación en la respuesta contiene azurewebsites.net. Especifique el patrón `(https?):\/\/.*azurewebsites\.net(.*)$`.
1. Realizar una acción para volver a escribir el encabezado de ubicación para que tenga el nombre de host de la aplicación la puerta de enlace. Hacer esto escribiendo `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` como el valor del encabezado.

![Modificar encabezado de ubicación](media/rewrite-http-headers/app-service-redirection.png)

### <a name="implement-security-http-headers-to-prevent-vulnerabilities"></a>Implementar los encabezados HTTP de seguridad para evitar las vulnerabilidades

Puede corregir varias vulnerabilidades de seguridad mediante la implementación de encabezados necesarios en la respuesta de la aplicación. Estos encabezados de seguridad incluyen X-XSS-Protection Strict-Transport-Security y Content-Security-Policy. Puede usar Application Gateway para establecer estos encabezados para todas las respuestas.

![Encabezado de seguridad](media/rewrite-http-headers/security-header.png)

### <a name="delete-unwanted-headers"></a>Eliminar encabezados no deseados

Es posible que desee quitar encabezados que revelen información confidencial de una respuesta HTTP. Por ejemplo, es posible que desee quitar información como el nombre del servidor back-end, el sistema operativo o los detalles de la biblioteca. Puede usar la puerta de enlace de la aplicación para quitar estos encabezados:

![Eliminar encabezado](media/rewrite-http-headers/remove-headers.png)

### <a name="check-for-the-presence-of-a-header"></a>Comprobar la presencia de un encabezado

Puede evaluar un encabezado de solicitud o respuesta HTTP para la presencia de una variable de encabezado o el servidor. Esta evaluación es útil cuando desea realizar una reescritura de encabezado solo cuando está presente un encabezado determinado.

![Comprobando la presencia de un encabezado](media/rewrite-http-headers/check-presence.png)

## <a name="limitations"></a>Limitaciones

- Si una respuesta tiene varios encabezados con el mismo nombre, a continuación, volver a escribir el valor de uno de esos encabezados dará como resultado la eliminación de los demás encabezados en la respuesta. Normalmente, esto puede suceder con encabezado Set-Cookie ya que puede tener más de un encabezado Set-Cookie en una respuesta. Uno de estos escenarios es cuando se usa un servicio de aplicaciones con una puerta de enlace de aplicaciones y ha configurado la afinidad de sesión basada en cookies en la puerta de enlace de la aplicación. En este caso, la respuesta contendrá 2 encabezados Set-Cookie: uno utilizado por el servicio de aplicación, es decir, `Set-Cookie: ARRAffinity=ba127f1caf6ac822b2347cc18bba0364d699ca1ad44d20e0ec01ea80cda2a735;Path=/;HttpOnly;Domain=sitename.azurewebsites.net` y otro para la afinidad de la puerta de enlace de aplicación, es decir, `Set-Cookie: ApplicationGatewayAffinity=c1a2bd51lfd396387f96bl9cc3d2c516; Path=/`. Volver a escribir uno de los encabezados de Set-Cookie en este escenario podría quitar el otro encabezado Set-Cookie de la respuesta.

- Actualmente no se admite la reescritura de los encabezados de Host, actualización y conexión.

- Los nombres de encabezado pueden contener los caracteres alfanuméricos y símbolos específicos, tal como se define en [RFC 7230](https://tools.ietf.org/html/rfc7230#page-27). Actualmente no se admite el carácter de subrayado (\_) caracteres especiales en nombres de encabezado.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información sobre cómo volver a escribir los encabezados HTTP, vea:

- [Vuelva a escribir los encabezados HTTP mediante Azure portal](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers-portal)
- [Vuelva a escribir los encabezados HTTP con Azure PowerShell](add-http-header-rewrite-rule-powershell.md)
