---
title: 'Solución de problemas de Azure Application Gateway con App Service: redirección a la dirección URL del servicio de aplicación'
description: Este artículo proporciona información sobre cómo solucionar el problema de redirección cuando se utiliza Azure Application Gateway con Azure App Service
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/22/2019
ms.author: absha
ms.openlocfilehash: 359d75f10f95b0e41ccd9a869d49247355f0d5d0
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "58123188"
---
# <a name="troubleshoot-application-gateway-with-app-service--redirection-to-app-services-url"></a>Solución de problemas de Application Gateway con App Service: redireccionamiento a una dirección URL de App Service

 Obtenga información sobre cómo diagnosticar y resolver problemas de redirección con Application Gateway donde introducción se expone la dirección URL del servicio de aplicación.

## <a name="overview"></a>Información general

Al configurar un servicio de aplicación en el grupo de back-end de Application Gateway públicas y tiene un redireccionamiento configurado en el código de aplicación, puede que vea, cuando tiene acceso a Application Gateway, se le redirigirá el explorador directamente a la aplicación Dirección URL del servicio.

Este problema puede ocurrir debido a los siguientes motivos principales:

- Tener redireccionamiento configurado en el servicio de aplicaciones. Redirección puede ser tan sencilla como agregar una barra oblicua final a la solicitud.
- Tener la autenticación de Azure AD, lo que hace que la redirección.
- Se ha habilitado el modificador "Seleccionar nombre de Host de back-end Address" en la configuración de HTTP de Application Gateway.
- No tiene el dominio personalizado registrado con el servicio de aplicación.

## <a name="sample-configuration"></a>Configuración de ejemplo

- Agente de escucha HTTP: Básica o de varios sitios
- Grupo de direcciones de back-end: App Service
- Configuración de HTTP: "¡Pick nombre de host de dirección de back-end" habilitada
- Sondeo: "¡Pick nombre de host de configuración de HTTP" habilitada

## <a name="cause"></a>Causa

Un servicio de aplicaciones sólo se puede acceder con los nombres de host configurado en la configuración de dominio personalizado, de forma predeterminada, es "example.azurewebsites.net" y si desea tener acceso al servicio de aplicación con Application Gateway no registrado en App Service o con un nombre de host Application Gateway FQDN, tendrá que reemplazar el nombre de host en la solicitud original al nombre de host del servicio de aplicación.

Para lograr esto con Application Gateway, se usa el modificador "Seleccionar nombre de host de dirección de back-end" en la configuración de HTTP y para el sondeo para que funcione, utilizamos "Seleccionar nombre de host de back-end HTTP configuración" en la configuración de sondeo.

![appservice-1](./media/troubleshoot-app-service-redirection-app-service-url/appservice-1.png)

Debido a esto, cuando el servicio de aplicación realiza una redirección, usa el nombre de host "example.azurewebsites.net" en el encabezado de ubicación, en lugar del nombre de host original a menos que configure de otra manera. Puede comprobar los encabezados de solicitud y respuesta de ejemplo siguiente.
```
## Request headers to Application Gateway:

Request URL: http://www.contoso.com/path

Request Method: GET

Host: www.contoso.com

## Response headers:

Status Code: 301 Moved Permanently

Location: http://example.azurewebsites.net/path/

Server: Microsoft-IIS/10.0

Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=example.azurewebsites.net

X-Powered-By: ASP.NET
```
En el ejemplo anterior, puede observar que el encabezado de respuesta tiene un código de estado de 301 para la redirección y el encabezado de ubicación tiene el nombre de host del servicio de aplicación, el nombre de host original en lugar de "www.contoso.com".

## <a name="solution"></a>Solución

Este problema puede solucionarse al no tener una redirección en el lado de la aplicación, sin embargo, si no es posible, que debemos pasar el mismo encabezado de host que recibe de la puerta de enlace de aplicaciones en App Service también en lugar de hacer una invalidación del host.

Una vez que hacemos eso, App Service realizará la redirección (si existe) en el mismo encabezado de host original que señala a la puerta de enlace de aplicaciones y no su propio.

Para lograr esto, debe tener un dominio personalizado y siga el proceso que se mencionan a continuación.

- Registre el dominio a la lista de dominios personalizados de App Service. Para ello, debe tener un registro CNAME en el dominio personalizado que apunte al FQDN del servicio de aplicación. Para obtener más información, consulte [asignar un nombre DNS personalizado a Azure App Service](https://docs.microsoft.com//azure/app-service/app-service-web-tutorial-custom-domain).

![appservice-2](./media/troubleshoot-app-service-redirection-app-service-url/appservice-2.png)

- Una vez hecho esto, App Service está listo para aceptar el nombre de host "www.contoso.com". Ahora, cambie la entrada CNAME en DNS para que apunte al FQDN de la puerta de enlace de aplicaciones. Por ejemplo, "appgw.eastus.cloudapp.azure.com".

- Asegúrese de que el dominio "www.contoso.com" se resuelve al FQDN de la puerta de enlace de aplicaciones al realizar una consulta DNS.

- Establecer el sondeo personalizado para deshabilitar "Seleccionar nombre de host de los valores de back-end HTTP". Esto puede hacerse desde el portal para ello, desactive la casilla de verificación en la configuración de sondeo y en PowerShell mediante el uso no - PickHostNameFromBackendHttpSettings cambie en el comando Set-AzApplicationGatewayProbeConfig. En el campo de nombre de host del sondeo, escriba el FQDN del servicio de aplicación "example.azurewebsites.net" como las solicitudes de sondeo enviadas desde Application Gateway llevar esto en el encabezado de host.

  > [!NOTE]
  > Mientras se realiza el paso siguiente, asegúrese de que el sondeo personalizado no está asociado a la configuración de back-end HTTP porque los todavía de la configuración de HTTP tiene el modificador "Seleccionar nombre de host de dirección de back-end" habilitado en este momento.

- Establecer configuración de HTTP de Application Gateway para deshabilitar "Seleccionar nombre de host de dirección de back-end". Esto puede hacerse desde el portal para ello, desactive la casilla de verificación y en PowerShell mediante el uso no - PickHostNameFromBackendAddress cambie en el comando Set-AzApplicationGatewayBackendHttpSettings.

- Asociar el sondeo personalizado a la configuración HTTP de back-end y comprobar el estado de back-end si es correcto.

- Una vez hecho esto, Application Gateway ahora se debe reenviar el mismo nombre de host "www.contoso.com" al servicio de aplicación y se realizará la redirección en el mismo nombre de host. Puede comprobar los encabezados de solicitud y respuesta de ejemplo siguiente.
  ```
  ## Request headers to Application Gateway:

  Request URL: http://www.contoso.com/path

  Request Method: GET

  Host: www.contoso.com

  ## Response headers:

  Status Code: 301 Moved Permanently

  Location: http://www.contoso.com/path/

  Server: Microsoft-IIS/10.0

  Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=www.contoso.com

  X-Powered-By: ASP.NET
  ```
  ## <a name="next-steps"></a>Pasos siguientes

Si los pasos anteriores no resuelven el problema, abra una [incidencia de soporte técnico](https://azure.microsoft.com/support/options/).
