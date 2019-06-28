---
title: 'Solución de problemas de Azure Application Gateway con App Service: redireccionamiento a una dirección URL de App Service'
description: En este artículo se proporciona información sobre cómo solucionar el problema de redireccionamiento cuando se utiliza Azure Application Gateway con Azure App Service
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 02/22/2019
ms.author: absha
ms.openlocfilehash: f456cfec82a315a2be877a52e4f3f1850b992736
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60715204"
---
# <a name="troubleshoot-application-gateway-with-app-service"></a>Solución de problemas de Application Gateway con App Service

Aprenda a diagnosticar y resolver los problemas detectados con Application Gateway y App Service como servidor back-end.

## <a name="overview"></a>Información general

En este artículo aprenderá a resolver los siguientes problemas:

> [!div class="checklist"]
> * Dirección URL de App Service que se expone en el explorador cuando hay un redireccionamiento
> * Dominio de la cookie de ARRAffinity de App Service establecido en el nombre de host de App Service (example.azurewebsites.net) en lugar del host original

Al configurar una instancia de App Service de acceso público en el grupo de back-end de Application Gateway y si tiene un redireccionamiento configurado en el código de aplicación, quizá vea cuando accede a Application Gateway que el explorador le redirige directamente a la dirección URL de App Service.

Este problema puede ocurrir debido a los siguientes motivos principales:

- Ha configurado el redireccionamiento en su instancia de App Service. El redireccionamiento puede ser tan sencillo como agregar una barra diagonal final a la solicitud.
- Tiene autenticación de Azure AD, lo que provoca el redireccionamiento.
- Se ha habilitado el modificador "Seleccionar el nombre de host de la dirección de back-end" en la configuración de HTTP de Application Gateway.
- No tiene el dominio personalizado registrado en su instancia de App Service.

Además, cuando utilice App Services detrás de Application Gateway y use un dominio personalizado para acceder a Application Gateway, es posible que vea que el valor del dominio de la cookie de ARRAffinity establecida por App Service tenga el nombre de dominio "example.azurewebsites.net". Si desea que el nombre de host original sea también el dominio de la cookie, siga la solución indicada en este artículo.

## <a name="sample-configuration"></a>Configuración de ejemplo

- Agente de escucha HTTP: básico o multisitio
- Grupo de direcciones de back-end: App Service
- Configuración de HTTP: "Seleccionar el nombre de host de la dirección de back-end" habilitado
- Sondeo: "Seleccionar el nombre de host de la configuración de HTTP de back-end" habilitado

## <a name="cause"></a>Causa

Solo se puede acceder a una instancia de App Service con los nombres de host establecidos en la configuración de dominio personalizada; de forma predeterminada, es "example.azurewebsites.net" y si desea acceder a App Service mediante Application Gateway con un nombre de host no registrado en App Service o con un FQDN de Application Gateway, tendrá que reemplazar el nombre de host en la solicitud original por el nombre de host de App Service.

Para lograr esto con Application Gateway, se usa el modificador "Seleccionar el nombre de host de la dirección de back-end" en la configuración de HTTP y para que el sondeo funcione, se utiliza "Seleccionar el nombre de host de la configuración de HTTP de back-end" en la configuración del sondeo.

![appservice-1](./media/troubleshoot-app-service-redirection-app-service-url/appservice-1.png)

Por ello, cuando App Service realiza un redireccionamiento, utiliza el nombre de host "example.azurewebsites.net" en el encabezado Location (Ubicación), en lugar del nombre de host original, a menos que esté configurado de otro modo. Puede comprobar los encabezados de solicitud y respuesta de ejemplo siguientes.
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
En el ejemplo anterior, puede observar que el encabezado de respuesta tiene un código de estado de 301 para el redireccionamiento y el encabezado de ubicación tiene el nombre de host de App Service, en lugar del nombre de host original "www.contoso.com".

## <a name="solution"></a>Solución

Este problema se puede resolver no teniendo un redireccionamiento en el lado de la aplicación; sin embargo, si esto no es posible, se debe pasar el mismo encabezado de host que recibe Application Gateways a App Service, en lugar de invalidar el host.

Una vez hecho, App Service realizará el redireccionamiento (si existe) en el mismo encabezado de host original que apunta a Application Gateway y no en el suyo propio.

Para lograrlo, debe ser propietario de un dominio personalizado y seguir el proceso que se menciona a continuación.

- Registre el dominio en la lista de dominios personalizados de App Service. Para ello, debe tener un registro CNAME en el dominio personalizado que apunte al FQDN d App Service. Para más información, consulte [Asignación de un nombre DNS personalizado existente a Azure App Service](https://docs.microsoft.com//azure/app-service/app-service-web-tutorial-custom-domain).

![appservice-2](./media/troubleshoot-app-service-redirection-app-service-url/appservice-2.png)

- Una vez hecho, App Service está listo para aceptar el nombre de host "www.contoso.com". Ahora, cambie la entrada CNAME en DNS para que apunte al FQDN de Application Gateway. Por ejemplo, "appgw.eastus.cloudapp.azure.com".

- Asegúrese de que el dominio "www.contoso.com" se resuelve en el FQDN de Application Gateway al realizar una consulta de DNS.

- Establezca el sondeo personalizado para deshabilitar "Seleccionar el nombre de host de la configuración de HTTP de back-end". Esto se puede hacer desde el portal desactivando la casilla en la configuración del sondeo y en PowerShell no utilizando el modificador -PickHostNameFromBackendHttpSettings en el comando Set-AzApplicationGatewayProbeConfig. En el campo de nombre de host del sondeo, especifique el FQDN de App Service "example.azurewebsites.net", ya que las solicitudes del sondeo enviadas desde Application Gateway mostrarán esto en el encabezado de host.

  > [!NOTE]
  > Mientras realiza el paso siguiente, asegúrese de que el sondeo personalizado no esté asociado a la configuración de back-end de HTTP porque la configuración de HTTP aún tiene el modificador "Seleccionar el nombre de host de la dirección de back-end" habilitado en ese momento.

- Establezca la configuración de HTTP de Application Gateway para deshabilitar "Seleccionar el nombre de host de la dirección de back-end". Esto se puede hacer desde el portal desactivando la casilla y en PowerShell no utilizando el modificador -PickHostNameFromBackendAddress en el comando Set-AzApplicationGatewayBackendHttpSettings.

- Asocie el sondeo personalizado a la configuración de HTTP de back-end y compruebe el estado de back-end si es correcto.

- Una vez hecho esto, Application Gateway debe reenviar ahora el mismo nombre de host "www.contoso.com" a App Service y se el redireccionamiento realizará en el mismo nombre de host. Puede comprobar los encabezados de solicitud y respuesta de ejemplo siguientes.

Para implementar los pasos mencionados anteriormente con PowerShell en una instalación existente, siga este script de PowerShell de ejemplo. Tenga en cuenta que no hemos usado los modificadores -PickHostname en la configuración de Sondeo y Configuración HTTP.

```azurepowershell-interactive
$gw=Get-AzApplicationGateway -Name AppGw1 -ResourceGroupName AppGwRG
Set-AzApplicationGatewayProbeConfig -ApplicationGateway $gw -Name AppServiceProbe -Protocol Http -HostName "example.azurewebsites.net" -Path "/" -Interval 30 -Timeout 30 -UnhealthyThreshold 3
$probe=Get-AzApplicationGatewayProbeConfig -Name AppServiceProbe -ApplicationGateway $gw
Set-AzApplicationGatewayBackendHttpSettings -Name appgwhttpsettings -ApplicationGateway $gw -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 30
Set-AzApplicationGateway -ApplicationGateway $gw
```
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
