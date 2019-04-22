---
title: 'Cómo configurar OpenVPN en Azure VPN Gateway: PowerShell | Microsoft Docs'
description: Pasos para configurar OpenVPN de Azure VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 01/16/2019
ms.author: cherylmc
ms.openlocfilehash: 8cc2a6d4ad06bf4a55d4ef078970823df1e0d910
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59281971"
---
# <a name="configure-openvpn-for-azure-point-to-site-vpn-gateway-preview"></a>Configuración de OpenVPN para la puerta de enlace de VPN de punto a sitio de Azure (versión preliminar)

En este artículo le ayuda a configurar **OpenVPN® protocolo** en Azure VPN Gateway. Se supone que ya tiene un entorno de trabajo punto a sitio. En caso contrario, utilice las instrucciones del paso 1 para crear una VPN de punto a sitio.

> [!IMPORTANT]
> Esta versión preliminar pública se proporciona sin un contrato de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="register"></a>Registro de esta característica

Haga clic en **Probar** en estos pasos para registrar esta característica fácilmente mediante Azure Cloud Shell.

>[!NOTE]
>Si no la registra, no podrá usarla.
>

Después de hacer clic en **Probar** para abrir Azure Cloud Shell, copie y pegue los siguientes comandos:

```azurepowershell-interactive
Register-AzProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```
 
```azurepowershell-interactive
Get-AzProviderFeature -ProviderNamespace Microsoft.Network -FeatureName AllowVnetGatewayOpenVpnProtocol
```

Una vez que la característica se muestre como registrada, vuelva a registrar la suscripción en el espacio de nombres Microsoft.Network.

```azurepowershell-interactive
Register-AzResourceProvider -ProviderNamespace Microsoft.Network
```

## <a name="vnet"></a>1. Configuración de una VPN de punto a sitio

Si no dispone de un entorno de punto a sitio funcionando, siga las instrucciones para crear uno. Consulte [Creación de una VPN de punto a sitio](vpn-gateway-howto-point-to-site-resource-manager-portal.md) para crear y configurar una puerta de enlace de VPN de punto a sitio con autenticación de certificados nativos de Azure. 

> [!IMPORTANT]
> La SKU de nivel Básico no es compatible con OpenVPN.

## <a name="enable"></a>2. Habilitación de OpenVPN en la puerta de enlace

Habilite OpenVPN en la puerta de enlace. Asegúrese de que la puerta de enlace ya está configurada para conexiones de punto a sitio (IKEv2 o SSTP) antes de ejecutar los comandos siguientes:

```azurepowershell-interactive
$gw = Get-AzVirtualNetworkGateway -ResourceGroupName $rgname -name $name
Set-AzVirtualNetworkGateway -VirtualNetworkGateway $gw -VpnClientProtocol OpenVPN
```

## <a name="next-steps"></a>Pasos siguientes

Para configurar clientes para OpenVPN, consulte [Configuración de clientes de OpenVPN](vpn-gateway-howto-openvpn-clients.md).

**"OpenVPN" es una marca de OpenVPN Inc.**