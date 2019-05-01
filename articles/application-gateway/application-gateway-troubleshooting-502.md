---
title: Solucionar errores de Azure Application Gateway puerta de enlace incorrecta (502)
description: Obtenga información sobre cómo solucionar errores 502 en el servicio Application Gateway.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 4/25/2019
ms.author: amsriva
ms.openlocfilehash: 2a1c7e480e896da6852949c9d765d17290e4e9ce
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64697164"
---
# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>Solución de errores de puerta de enlace incorrecta en el servicio Puerta de enlace de aplicaciones

Obtenga información sobre cómo solucionar errores de puerta de enlace incorrecta (502) recibidos al utilizar Azure Application Gateway.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="overview"></a>Información general

Después de configurar una puerta de enlace de aplicaciones, uno de los errores que puede ver es "Error de servidor: 502 - Web server received an invalid response while acting as a gateway or proxy server". (502: el servidor web ha recibido una respuesta no válida mientras actuaba como puerta de enlace o servidor proxy". Este error puede ocurrir por los siguientes motivos principales:

* NSG, UDR o DNS personalizado está bloqueando el acceso a miembros del grupo de back-end.
* Máquinas virtuales o instancias del conjunto de escalado de máquinas virtuales de back-end no responden al sondeo de estado de forma predeterminada.
* La configuración de las sondas de estado personalizadas no es válida o adecuada.
* Azure Application Gateway [grupo back-end no está configurado o está vacío](#empty-backendaddresspool).
* Ninguna de las máquinas virtuales o instancias del [conjunto de escalado de máquina virtual está en buen estado](#unhealthy-instances-in-backendaddresspool).
* [El tiempo de espera de solicitud se superó o hay problemas de conectividad](#request-time-out) con las solicitudes de usuario.

## <a name="network-security-group-user-defined-route-or-custom-dns-issue"></a>Problema con grupo de seguridad de red, ruta definida por el usuario o DNS personalizado

### <a name="cause"></a>Causa

Si el acceso al back-end se bloquea debido a un NSG, UDR o DNS personalizado, instancias de application gateway no pueden alcanzar el grupo de back-end. Esto hace que los errores de sondeo, lo que produce 502 errores.

El NSG/UDR pueden estar presente en la subred de puerta de enlace de aplicaciones o la subred donde se implementan las máquinas virtuales de la aplicación.

De forma similar, la presencia de un DNS personalizado en la red virtual también podría provocar problemas. Un FQDN utilizado para los miembros del grupo de back-end no se puede resolver correctamente el servidor DNS configurado por el usuario para la red virtual.

### <a name="solution"></a>Solución

Valide la configuración de NSG, UDR y DNS mediante los pasos siguientes:

* Compruebe el que NSG asociado con la subred de puerta de enlace de aplicación. Asegúrese de que no se bloquea la comunicación al back-end.
* Compruebe que UDR asociada con la subred de puerta de enlace de aplicaciones. Asegúrese de que la UDR no es dirigir el tráfico fuera de la subred de back-end. Por ejemplo, compruebe el enrutamiento para las aplicaciones virtuales o las rutas predeterminadas anunciadas a la subred de puerta de enlace de aplicaciones a través de ExpressRoute/VPN de red.

```azurepowershell
$vnet = Get-AzVirtualNetwork -Name vnetName -ResourceGroupName rgName
Get-AzVirtualNetworkSubnetConfig -Name appGwSubnet -VirtualNetwork $vnet
```

* Compruebe el grupo de seguridad de red y la ruta efectivos con la máquina virtual de back-end.

```azurepowershell
Get-AzEffectiveNetworkSecurityGroup -NetworkInterfaceName nic1 -ResourceGroupName testrg
Get-AzEffectiveRouteTable -NetworkInterfaceName nic1 -ResourceGroupName testrg
```

* Compruebe la presencia de DNS personalizado en la red virtual. El DNS se puede comprobar examinando los detalles de las propiedades de red virtual en la salida.

```json
Get-AzVirtualNetwork -Name vnetName -ResourceGroupName rgName 
DhcpOptions            : {
                           "DnsServers": [
                             "x.x.x.x"
                           ]
                         }
```
Si está presente, asegúrese de que el servidor DNS puede resolver correctamente el nombre de dominio completo del miembro del grupo de back-end.

## <a name="problems-with-default-health-probe"></a>Problemas con la sonda de estado predeterminada

### <a name="cause"></a>Causa

502 errores también pueden ser indicativos frecuentes de que el sondeo de estado predeterminada no puede llegar a las máquinas virtuales de back-end.

Cuando se aprovisiona una instancia de application gateway, configura automáticamente un sondeo de estado predeterminado para cada BackendAddressPool que utiliza las propiedades de BackendHttpSetting. El usuario no tiene que escribir datos para establecer esta sonda. En concreto, cuando se configura una regla de equilibrio de carga, se crea una asociación entre un BackendHttpSetting y BackendAddressPool. Un sondeo predeterminado está configurado para cada una de estas asociaciones y la puerta de enlace de la aplicación inicia una conexión de comprobación de estado periódicas a cada instancia de BackendAddressPool del puerto especificado en el elemento BackendHttpSetting. 

En la tabla siguiente se enumera los valores asociados con la sonda de estado predeterminada:

| Propiedad de sondeo | Valor | DESCRIPCIÓN |
| --- | --- | --- |
| Dirección URL de sondeo |`http://127.0.0.1/` |Ruta de acceso URL |
| Interval |30 |Intervalo de sondeo en segundos |
| Tiempo de espera |30 |Tiempo de espera del sondeo en segundos |
| Umbral incorrecto |3 |Número de reintentos de sondeo. El servidor back-end se marca como inactivo después de que el número de errores de sondeo consecutivos alcanza el umbral incorrecto. |

### <a name="solution"></a>Solución

* Asegúrese de que se ha configurado un sitio predeterminado y de que está escuchando en 127.0.0.1.
* Si BackendHttpSetting especifica un puerto distinto de 80, se debe configurar que el sitio predeterminado escuche en ese puerto.
* La llamada a `http://127.0.0.1:port` debe devolver el código de resultado HTTP 200. Esto se debe devolver dentro del período de tiempo de espera de 30 segundos.
* Asegúrese de que el puerto configurado está abierto y que no hay ninguna regla de firewall ni grupos de seguridad de red de Azure, que bloquear el tráfico entrante o saliente en el puerto configurado.
* Si se usa máquinas virtuales de Azure clásicas o servicio en la nube con un FQDN o una dirección IP pública, asegúrese de que el correspondiente [extremo](../virtual-machines/windows/classic/setup-endpoints.md?toc=%2fazure%2fapplication-gateway%2ftoc.json) se abre.
* Si la máquina virtual se configura a través de Azure Resource Manager y está fuera de la red virtual donde se implementa la puerta de enlace de aplicaciones, un [Network Security Group](../virtual-network/security-overview.md) debe configurarse para permitir el acceso en el puerto deseado.

## <a name="problems-with-custom-health-probe"></a>Problemas con la sonda de estado personalizada

### <a name="cause"></a>Causa

Las sondas de estado personalizadas dotan al comportamiento de sondeo predeterminado de mayor flexibilidad. Cuando utilice sondeos personalizados, puede configurar el intervalo de sondeo, la dirección URL, la ruta de acceso para probar y cuántas respuestas erróneas se aceptan antes de marcar la instancia del grupo de back-end como en mal estado.

Se agregan las siguientes propiedades adicionales:

| Propiedad de sondeo | DESCRIPCIÓN |
| --- | --- |
| NOMBRE |Nombre del sondeo. Este nombre se usa para hacer referencia al sondeo en la configuración de HTTP de back-end. |
| Protocol |Protocolo usado para enviar el sondeo. El sondeo utiliza el protocolo definido en la configuración de HTTP del back-end. |
| Host |Nombre de host para enviar el sondeo. Sólo es aplicable cuando se configura varios sitios en application gateway. Es diferente al nombre de host de máquina virtual. |
| Ruta de acceso |Ruta de acceso relativa del sondeo. La ruta de acceso válida se inicia desde '/'. La sonda se envía a \<protocolo\>://\<host\>:\<puerto\>\<ruta de acceso\> |
| Interval |Intervalo de sondeo en segundos. Es el intervalo de tiempo entre dos sondeos consecutivos. |
| Tiempo de espera |Tiempo de espera del sondeo en segundos. Si no se recibe una respuesta válida dentro de este período de tiempo de espera, el sondeo se marca como errónea. |
| Umbral incorrecto |Número de reintentos de sondeo. El servidor back-end se marca como inactivo después de que el número de errores de sondeo consecutivos alcanza el umbral incorrecto. |

### <a name="solution"></a>Solución

Valide que el sondeo de mantenimiento personalizado esté configurado correctamente según la tabla anterior. Además de realizar los pasos de solución de problemas anteriores, no se olvide tampoco de llevar a cabo estos:

* Asegúrese de que la sonda se ha especificado correctamente según la [guía](application-gateway-create-probe-ps.md).
* Si la puerta de enlace de aplicaciones está configurado para un único sitio, de forma predeterminada, el Host del nombre debe especificarse como `127.0.0.1`, a menos que en caso contrario, se configura en el sondeo personalizado.
* Asegúrese de que una llamada a http://\<host\>:\<puerto\>\<ruta de acceso\> devuelva el código de resultado HTTP 200.
* Garantizan que el intervalo de tiempo de espera y UnhealtyThreshold estén dentro de rangos aceptables.
* Si utiliza un sondeo HTTPS, asegúrese de que el servidor de back-end no requiera SNI; para ello, configure un certificado de reserva en dicho servidor.

## <a name="request-time-out"></a>Tiempo de espera de solicitud superado

### <a name="cause"></a>Causa

Cuando se recibe una solicitud de usuario, application gateway aplica las reglas configuradas para la solicitud y lo enruta a una instancia del grupo de back-end. Espera, durante un intervalo de tiempo que puede configurarse, una respuesta de la instancia back-end. De forma predeterminada, este intervalo es **20** segundos. Si la puerta de enlace de la aplicación no recibe una respuesta de aplicación de back-end en este intervalo, la solicitud del usuario obtiene un error 502.

### <a name="solution"></a>Solución

Application Gateway permite configurar esta opción mediante BackendHttpSetting, que, a continuación, se puede aplicar a diferentes grupos. Diferentes grupos de back-end pueden tener distintos elementos BackendHttpSetting y un tiempo de espera de solicitudes diferente configurado.

```azurepowershell
    New-AzApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60
```

## <a name="empty-backendaddresspool"></a>Valor de BackendAddressPool vacío

### <a name="cause"></a>Causa

Si la puerta de enlace de la aplicación no tiene máquinas virtuales o conjunto de escalado de máquina virtual configurado en el grupo de direcciones de back-end, no puede enrutar solicitudes de clientes y envía un error de puerta de enlace incorrecta.

### <a name="solution"></a>Solución

Asegúrese de que el grupo de direcciones de back-end no está vacío. Puede hacerlo mediante PowerShell, la CLI o el Portal.

```azurepowershell
Get-AzApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"
```

El resultado del cmdlet anterior debe contener un grupo de direcciones back-end que no esté vacío. El ejemplo siguiente muestra dos grupos devuelven que se configuran con un FQDN o una direcciones IP para las máquinas virtuales de back-end. El estado de aprovisionamiento de BackendAddressPool debe ser "Correcto".

BackendAddressPoolsText:

```json
[{
    "BackendAddresses": [{
        "ipAddress": "10.0.0.10",
        "ipAddress": "10.0.0.11"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool01",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
}, {
    "BackendAddresses": [{
        "Fqdn": "xyx.cloudapp.net",
        "Fqdn": "abc.cloudapp.net"
    }],
    "BackendIpConfigurations": [],
    "ProvisioningState": "Succeeded",
    "Name": "Pool02",
    "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
    "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
}]
```

## <a name="unhealthy-instances-in-backendaddresspool"></a>Instancias incorrectas en BackendAddressPool

### <a name="cause"></a>Causa

Si todas las instancias de BackendAddressPool son incorrectas, application gateway no tiene ningún back-end para enrutar la solicitud al usuario. Esto puede ser también el caso cuando las instancias de back-end están en buen estadas, pero no tienen implementada la aplicación necesaria.

### <a name="solution"></a>Solución

Asegúrese de que las instancias son correctas y de que la aplicación está configurada de la forma adecuada. Compruebe si las instancias de back-end pueden responder a un ping desde otra máquina virtual en la misma red virtual. Si se configura con un punto de conexión público, asegúrese de que es reparables por una solicitud de explorador a la aplicación web.

## <a name="next-steps"></a>Pasos siguientes

Si los pasos anteriores no resuelven el problema, abra un [incidencia de soporte técnico](https://azure.microsoft.com/support/options/).

