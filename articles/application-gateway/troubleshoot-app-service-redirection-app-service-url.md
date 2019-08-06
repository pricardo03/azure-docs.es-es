---
title: 'Solución de problemas de Azure Application Gateway con App Service: redireccionamiento a una dirección URL de App Service'
description: En este artículo se proporciona información sobre cómo solucionar el problema de redireccionamiento cuando se utiliza Azure Application Gateway con Azure App Service
services: application-gateway
author: abshamsft
ms.service: application-gateway
ms.topic: article
ms.date: 07/19/2019
ms.author: absha
ms.openlocfilehash: 7baadfb549b19bb12757c82d723578202b5cf8ad
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2019
ms.locfileid: "68347885"
---
# <a name="troubleshoot-app-service-issues-in-application-gateway"></a>Solución de problemas de App Service en Application Gateway

Aprenda a diagnosticar y resolver los problemas detectados cuando App Service se usa como servidor back-end con Application Gateway.

## <a name="overview"></a>Información general

En este artículo aprenderá a resolver los siguientes problemas:

> [!div class="checklist"]
> * Dirección URL de App Service que se expone en el explorador cuando hay un redireccionamiento
> * Dominio de la cookie de ARRAffinity de App Service establecido en el nombre de host de App Service (example.azurewebsites.net) en lugar del host original

Cuando una aplicación back-end envía una respuesta de redireccionamiento, es posible que le interese redirigir el cliente a una dirección URL diferente de la especificada por la aplicación back-end. Por ejemplo, es posible que quiera hacerlo cuando un servicio de aplicaciones se hospeda detrás de una puerta de enlace de aplicaciones y requiere que el cliente realice un redireccionamiento a su ruta de acceso relativa. (Por ejemplo, un redireccionamiento de contoso.azurewebsites.net/path1 a contoso.azurewebsites.net/path2). Cuando el servicio de aplicaciones envía una respuesta de redireccionamiento, usa el mismo nombre de host en el encabezado de ubicación de su respuesta que el que aparece en la solicitud que recibe de la puerta de enlace de aplicaciones. Así pues, el cliente hará la solicitud directamente a contoso.azurewebsites.net/path2, en lugar de pasar por la puerta de enlace de aplicaciones (contoso.com/path2). No es conveniente omitir la puerta de enlace de aplicaciones.

Este problema puede ocurrir debido a los siguientes motivos principales:

- Ha configurado el redireccionamiento en su instancia de App Service. El redireccionamiento puede ser tan sencillo como agregar una barra diagonal final a la solicitud.
- Tiene autenticación de Azure AD, lo que provoca el redireccionamiento.

Además, cuando use App Services detrás de Application Gateway, el nombre de dominio asociado a Application Gateway (example.com) será diferente del nombre de dominio de App Service (por ejemplo, example.azurewebsites.net), por lo que observará que el valor de dominio para la cookie ARRAffinity establecida por App Service lleva el nombre de dominio "example.azurewebsites.net", lo que no es conveniente. El nombre de host original (example.com) debe ser el valor del nombre de dominio de la cookie.

## <a name="sample-configuration"></a>Configuración de ejemplo

- Agente de escucha HTTP: básico o multisitio
- Grupo de direcciones de back-end: App Service
- Configuración de HTTP: "Seleccionar el nombre de host de la dirección de back-end" habilitado
- Sondeo: "Seleccionar el nombre de host de la configuración de HTTP de back-end" habilitado

## <a name="cause"></a>Causa

Dado que App Service es un servicio multiinquilino, usa el encabezado de host en la solicitud para enrutar la solicitud al punto de conexión correcto. Aun así, el nombre de dominio predeterminado de App Service, *.azurewebsites.net (por ejemplo, contoso.azurewebsites.net), es diferente del nombre de dominio de la puerta de enlace de aplicaciones (por ejemplo, contoso.com). Dado que la solicitud original del cliente tiene el nombre de dominio de la puerta de enlace de aplicaciones (contoso.com) como nombre de host, debe configurar la puerta de enlace de aplicaciones para cambiar el nombre de host de la solicitud original al nombre de host del servicio de aplicaciones cuando enruta la solicitud al back-end del servicio de aplicaciones.  Para conseguirlo, use el modificador "Pick Hostname from Backend Address" (Seleccionar el nombre de host de la dirección de back-end) en la configuración de HTTP de Application Gateway y el modificador "Pick Hostname from Backend HTTP Settings" (Seleccionar el nombre de host de la configuración de HTTP de back-end) en la configuración del sondeo de mantenimiento.



![appservice-1](./media/troubleshoot-app-service-redirection-app-service-url/appservice-1.png)

Por ello, cuando App Service realiza un redireccionamiento, usa el nombre de host reemplazado "contoso.azurewebsites.net" en el encabezado de ubicación, en lugar del nombre de host original "contoso.com", a menos que esté configurado de otro modo. Puede comprobar los encabezados de solicitud y respuesta de ejemplo siguientes.
```
## Request headers to Application Gateway:

Request URL: http://www.contoso.com/path

Request Method: GET

Host: www.contoso.com

## Response headers:

Status Code: 301 Moved Permanently

Location: http://contoso.azurewebsites.net/path/

Server: Microsoft-IIS/10.0

Set-Cookie: ARRAffinity=b5b1b14066f35b3e4533a1974cacfbbd969bf1960b6518aa2c2e2619700e4010;Path=/;HttpOnly;Domain=contoso.azurewebsites.net

X-Powered-By: ASP.NET
```
En el ejemplo anterior, puede observar que el encabezado de respuesta tiene un código de estado de 301 para el redireccionamiento y el encabezado de ubicación tiene el nombre de host de App Service, en lugar del nombre de host original "www.contoso.com".

## <a name="solution-rewrite-the-location-header"></a>Solución: reescribir el encabezado de ubicación

Tendrá que establecer el nombre de host del encabezado de ubicación en el nombre de dominio de la puerta de enlace de aplicaciones. Para ello, cree una [regla de reescritura](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers) con una condición que se evalúe si el encabezado de ubicación de la respuesta contiene azurewebsites.net y que realice una acción para volver a escribir el encabezado de ubicación de forma que tenga el nombre de host de la puerta de enlace de aplicaciones.  Vea las instrucciones para [volver a escribir el encabezado de ubicación](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers#modify-a-redirection-url).

> [!NOTE]
> La compatibilidad con la reescritura de encabezados HTTP solo está disponible para [Standard_V2 y WAF_v2 SKU](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant) de Application Gateway. Si usa la SKU V1, se recomienda encarecidamente que [migre de V1 a V2](https://docs.microsoft.com/azure/application-gateway/migrate-v1-v2) para poder usar la reescritura y otras [funcionalidades avanzadas](https://docs.microsoft.com/azure/application-gateway/application-gateway-autoscaling-zone-redundant#feature-comparison-between-v1-sku-and-v2-sku) disponibles con la SKU V2.

## <a name="alternate-solution-use-app-services-custom-domain-instead-of-default-domain-name"></a>Solución alternativa: usar el dominio personalizado de App Service en lugar del nombre de dominio predeterminado

Si usa la SKU V1, no podrá reescribir el encabezado de ubicación, ya que esta funcionalidad solo está disponible para la SKU V2. Por lo tanto, para resolver el problema de redireccionamiento, deberá pasar el mismo nombre de host que recibe Application Gateway a App Service, en lugar de realizar un reemplazo del host.

Una vez hecho esto, App Service realizará el redireccionamiento (si existe) en el mismo encabezado de host original que apunta a Application Gateway, y no en el suyo propio.

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
