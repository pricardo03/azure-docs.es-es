---
title: Cómo configurar los clientes OpenVPN de Azure VPN Gateway | Microsoft Docs
description: Pasos para configurar los clientes OpenVPN de Azure VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 09/21/2018
ms.author: cherylmc
ms.openlocfilehash: 141f29ea1c7ebdd8208dda6bc39a60f7873dab7d
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46977849"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway-preview"></a>Configuración de los clientes OpenVPN de Azure VPN Gateway (versión preliminar)

En este artículo se ofrece ayuda para configurar los clientes OpenVPN.

> [!IMPORTANT]
> Esta versión preliminar pública se proporciona sin un contrato de nivel de servicio y no debe usarse para cargas de trabajo de producción. Puede que algunas características no se admitan, que tengan funcionalidades limitadas o que no estén disponibles en todas las ubicaciones de Azure. Para más información, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="before-you-begin"></a>Antes de empezar

Compruebe que ha completado los pasos para configurar OpenVPN para VPN Gateway. Para obtener más información, consulte [Configuración de los clientes OpenVPN de Azure VPN Gateway](vpn-gateway-howto-openvpn.md).

## <a name="windows"></a>Clientes Windows

1. Descargue e instale el cliente OpenVPN desde el [sitio web de OpenVPN](https://openvpn.net/index.php/open-source/downloads.html) oficial.
2. Descargue el perfil de VPN para la puerta de enlace. Esto puede hacerse desde la pestaña de configuración de punto a sitio de Azure Portal o mediante "New-AzureRmVpnClientConfiguration"en PowerShell.
3. Descomprima el perfil. A continuación, abra el archivo de configuración vpnconfig.ovpn desde la carpeta OpenVPN del Bloc de notas.
4. Rellene la sección de certificado cliente de P2S con la clave pública del certificado cliente de P2S en base64. En un certificado con formato PEM, puede abrir el archivo .cer y copiar la clave de base64 entre los encabezados del certificado. Vea aquí cómo exportar un certificado para obtener la clave pública codificada.
5. Rellene la sección de la clave privada con la clave privada del certificado cliente de P2S en base64. Para obtener más información sobre cómo extraer la clave privada, consulte [exportación de la clave](vpn-gateway-certificates-point-to-site.md#clientexport).
6. No cambie los demás campos. Use el relleno de la configuración de entrada del cliente para conectarse a la VPN.
7. Copie el archivo vpnconfig.ovpn en la carpeta C:\Program Files\OpenVPN\config.
8. Haga clic con el botón derecho en la bandeja del sistema y, después, en Conectar.

## <a name="mac"></a>Clientes Mac

1. Descargue e instale un cliente OpenVPN, como [TunnelBlik](https://tunnelblick.net/downloads.html). 
2. Descargue el perfil de VPN para la puerta de enlace. Esto puede hacerse desde la pestaña de configuración de punto a sitio de Azure Portal o mediante "New-AzureRmVpnClientConfiguration"en PowerShell.
3. Descomprima el perfil. Abra el archivo de configuración vpnconfig.ovpn desde la carpeta OpenVPN del Bloc de notas.
4. Rellene la sección de certificado cliente de P2S con la clave pública del certificado cliente de P2S en base64. En un certificado con formato PEM, puede abrir el archivo .cer y copiar la clave de base64 entre los encabezados del certificado. Consulte [Exportación de la clave pública](vpn-gateway-certificates-point-to-site.md#cer) para obtener más información acerca de cómo exportar un certificado para obtener la clave pública codificada.
5. Rellene la sección de la clave privada con la clave privada del certificado cliente de P2S en base64. Consulte [Exportación de la clave](https://www.geotrust.eu/en/support/manuals/microsoft/all+windows+servers/export+private+key+or+certificate/) para obtener más información sobre cómo extraer la clave privada.
6. No cambie los demás campos. Use el relleno de la configuración de entrada del cliente para conectarse a la VPN.
7. Haga doble clic en el archivo de perfil para crear el perfil en Tunnelblik.
8. Inicie Tunnelblik desde la carpeta de aplicaciones.
9. Haga clic en el icono de Tunneblik en la bandeja del sistema y seleccione Conectar.

## <a name="linux"></a>Clientes Linux

1. Abra una nueva sesión de terminal. Para abrir una nueva sesión, presione "CTRL + ALT + T" al mismo tiempo.
2. Escriba el siguiente comando para instalar los componentes necesarios:

  ```
  sudo apt-get install openvpn
  sudo apt-get -y install network-manager-openvpn
  sudo service network-manager restart
  ```
3. Descargue el perfil de VPN para la puerta de enlace. Esto puede hacerse desde la pestaña de configuración de punto a sitio de Azure Portal o mediante "New-AzureRmVpnClientConfiguration"en PowerShell.
4. Rellene la sección de la clave privada con la clave privada del certificado cliente de P2S en base64. Consulte [Exportación de la clave](https://www.geotrust.eu/en/support/manuals/microsoft/all+windows+servers/export+private+key+or+certificate/) para obtener más información sobre cómo extraer la clave privada.
5. Para conectarse mediante la línea de comandos, escriba lo siguiente:
  
  ```
  Sudo openvpn –config <name and path of your VPN profile file>
  ```
5. Para conectarse mediante la GUI, vaya a la configuración del sistema.
6. Haga clic en **+** para agregar una nueva conexión VPN.
7. En **Agregar VPN**, seleccione **Importar desde archivo...**.
8. Vaya al archivo de perfil y haga doble clic o seleccione **Abrir**.
9. Haga clic en **Agregar** en la ventana **Agregar VPN**.
  
  ![Importar desde archivo](./media/vpn-gateway-howto-openvpn-clients/importfromfile.png)
10. Puede conectarse mediante la **activación** de VPN en la página **Configuración de red** o en el icono de red de la bandeja del sistema.

## <a name="next-steps"></a>Pasos siguientes

Si desea que los clientes de VPN tengan acceso a recursos de otra red virtual (producción), siga las instrucciones del artículo [De red virtual a red virtual](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) para configurar una conexión de red virtual a red virtual. Asegúrese de habilitar BGP en las puertas de enlace y las conexiones. En caso contrario, el tráfico no fluirá.
