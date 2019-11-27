---
title: 'Sobre los perfiles de cliente de VPN de punto a sitio: Azure VPN Gateway | Microsoft Docs'
description: Esto le ayuda a trabajar con el archivo de perfil del cliente
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 11/04/2019
ms.author: cherylmc
ms.openlocfilehash: 340ff6d9b112f4ff04950499b2708d8882fa6120
ms.sourcegitcommit: 39da2d9675c3a2ac54ddc164da4568cf341ddecf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2019
ms.locfileid: "73960627"
---
# <a name="about-p2s-vpn-client-profiles"></a>Sobre los perfiles de cliente de P2S VPN

El archivo de perfil descargado contiene información necesaria para configurar una conexión VPN. Este artículo le ayudará a obtener y comprender la información necesaria para un perfil de cliente de VPN.

## <a name="1-download-the-file"></a>1. Descargue el archivo

Ejecute los comandos siguientes: Copie la dirección URL del resultado en el explorador para descargar el archivo zip del perfil.

```azurepowershell-interactive
$profile = New-AzVpnClientConfiguration -ResourceGroupName AADAuth -Name AADauthGW -AuthenticationMethod "EapTls"
   
$PROFILE.VpnProfileSASUrl
```

## <a name="2-extract-the-zip-file"></a>2. Extraiga el archivo ZIP

Extraiga el archivo ZIP. El archivo contiene las siguientes carpetas:

* AzureVPN
* Genérico
* OpenVPN (si ha habilitado la configuración de autenticación de OpenVPN y Azure AD en la puerta de enlace. Consulte [Creación de un inquilino](openvpn-azure-ad-tenant.md)).

## <a name="3-retrieve-information"></a>3. Recuperar información

En la carpeta **AzureVPN**, desplácese hasta el archivo ***azurevpnconfig. XML*** y ábralo con el Bloc de notas. Tome nota del texto entre las etiquetas siguientes.

```
<audience>          </audience>
<issuer>            </issuer>
<tennant>           </tennant>
<fqdn>              </fqdn>
<serversecret>      </serversecret>
```

## <a name="profile-details"></a>Detalles de perfil

Cuando agregue una conexión, use la información que recopiló en el paso anterior para la página de detalles del perfil. Los campos corresponden con la siguiente información:

   * **Audiencia:** Identifica el recurso de destinatario al que está destinado el token
   * **Emisor:** Identifica el Servicio de token de seguridad (STS) que emitió el token, así como el inquilino de Azure AD
   * **Inquilino:** Contiene un identificador único e inmutable del inquilino de directorio que emitió el token
   * **FQDN:** Nombre de dominio completo (FQDN) en la Azure VPN gateway
   * **ServerSecret:** La clave previamente compartida de VPN Gateway

## <a name="folder-contents"></a>Contenido de la carpeta

* La **carpeta OpenVPN** contiene el perfil de *ovpn* que debe modificarse para incluir la clave y el certificado. Para más información, consulte [Configurar clientes de OpenVPN para Azure VPN Gateway](vpn-gateway-howto-openvpn-clients.md#windows).

* La **carpeta genérica** contiene el certificado de servidor público y el archivo VpnSettings.xml. El archivo VpnSettings.xml contiene la información necesaria para configurar un cliente genérico.

* El archivo zip descargado también puede contener carpetas **WindowsAmd64** y **WindowsX86**. Estas carpetas contienen el instalador de SSTP y IKEv2 para clientes de Windows. Necesita derechos de administrador en el cliente para instalarlos.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la ubicación de punto a sitio, consulte [Sobre ubicación de punto a sitio](point-to-site-about.md).
