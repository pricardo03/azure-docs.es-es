---
title: Cómo configurar los clientes OpenVPN de Azure VPN Gateway | Microsoft Docs
description: Pasos para configurar los clientes OpenVPN de Azure VPN Gateway
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 06/14/2019
ms.author: cherylmc
ms.openlocfilehash: b8f1626da730178d2cd9c2f31c4f9876102b3d46
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477843"
---
# <a name="configure-openvpn-clients-for-azure-vpn-gateway"></a>Configuración de los clientes OpenVPN de Azure VPN Gateway

En este artículo se ofrece ayuda para configurar los clientes del **protocolo OpenVPN®**.

## <a name="before-you-begin"></a>Antes de empezar

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Compruebe que ha completado los pasos para configurar OpenVPN para VPN Gateway. Para obtener más información, consulte [Configuración de los clientes OpenVPN de Azure VPN Gateway](vpn-gateway-howto-openvpn.md).

## <a name="windows"></a>Clientes Windows

1. Descargue e instale el cliente OpenVPN (versión 2.4 o superior) desde el [sitio web de OpenVPN](https://openvpn.net/index.php/open-source/downloads.html) oficial.
2. Descargue el perfil de VPN para la puerta de enlace. Esto puede hacerse desde la pestaña de configuración de punto a sitio de Azure Portal o mediante "New-AzVpnClientConfiguration" en PowerShell.
3. Descomprima el perfil. A continuación, abra el archivo de configuración *vpnconfig.ovpn* desde la carpeta OpenVPN del Bloc de notas.
4. [Exporte](vpn-gateway-certificates-point-to-site.md#clientexport) el certificado de cliente P2P que ha creado y cargado en la configuración de P2S en la puerta de enlace.
5. Extraiga la clave privada y la huella digital de base64 del archivo *.pfx*. Hay varias formas de hacerlo. Una de ellas es usar OpenSSL en su máquina. El archivo *profileinfo.txt* contiene la clave privada y la huella digital de la entidad de certificación y el certificado de cliente. Asegúrese de usar la huella digital del certificado de cliente.

   ```
   openssl pkcs12 -in "filename.pfx" -nodes -out "profileinfo.txt"
   ```
6. Abra *profileinfo.txt* en el Bloc de notas. Para obtener la huella digital del certificado (secundario) de cliente, seleccione el texto (incluido y entre) "---BEGIN CERTIFICATE---" y "---END CERTIFICATE---" para el certificado secundario y cópielo. Puede identificar el certificado secundario si examina la línea subject=/.
7. Cambie al archivo *vpnconfig.ovpn* que abrió en el Bloc de notas en el paso 3. Busque la sección que se muestra a continuación y reemplace todo lo que hay entre "cert" y "/cert".

   ```
   # P2S client certificate
   # please fill this field with a PEM formatted cert
   <cert>
   $CLIENTCERTIFICATE
   </cert>
   ```
8. Abra el archivo *profileinfo.txt* en el Bloc de notas. Para obtener la clave privada, seleccione el texto (incluido y entre) "-----BEGIN PRIVATE KEY-----" y "-----END PRIVATE KEY-----" y cópielo.
9. Vuelva al archivo vpnconfig.ovpn en el Bloc de notas y busque esta sección. Pegue la clave privada y reemplace todo lo que hay entre "key" y "/key".

   ```
   # P2S client root certificate private key
   # please fill this field with a PEM formatted key
   <key>
   $PRIVATEKEY
   </key>
   ```
10. No cambie los demás campos. Use los datos de la configuración de entrada del cliente para conectarse a la VPN.
11. Copie el archivo vpnconfig.ovpn en la carpeta C:\Program Files\OpenVPN\config.
12. Haga clic con el botón derecho en la bandeja del sistema y, después, en Conectar.

## <a name="mac"></a>Clientes Mac

1. Descargue e instale un cliente OpenVPN, como [TunnelBlick](https://tunnelblick.net/downloads.html). 
2. Descargue el perfil de VPN para la puerta de enlace. Esto puede hacerse desde la pestaña de configuración de punto a sitio de Azure Portal o mediante "New-AzVpnClientConfiguration" en PowerShell.
3. Descomprima el perfil. Abra el archivo de configuración vpnconfig.ovpn desde la carpeta OpenVPN en el Bloc de notas.
4. Rellene la sección de certificado cliente de P2S con la clave pública del certificado cliente de P2S en Base64. En un certificado con formato PEM, puede abrir el archivo .cer y copiar la clave de base64 entre los encabezados del certificado. Consulte [Exportación de la clave pública](vpn-gateway-certificates-point-to-site.md#cer) para obtener más información acerca de cómo exportar un certificado para obtener la clave pública codificada.
5. Rellene la sección de la clave privada con la clave privada del certificado cliente de P2S en Base64. Consulte [Exportación de la clave privada](https://openvpn.net/community-resources/how-to/#pki) para obtener más información sobre cómo extraer la clave privada.
6. No cambie los demás campos. Use los datos de la configuración de entrada del cliente para conectarse a la VPN.
7. Haga doble clic en el archivo de perfil para crear el perfil en Tunnelblick.
8. Inicie Tunnelblick desde la carpeta de aplicaciones.
9. Haga clic en el icono de Tunneblick en la bandeja del sistema y seleccione Conectar.

> [!IMPORTANT]
>Solo iOS 11.0 y MacOS 10.13 (y sus respectivas versiones posteriores) son compatibles con el protocolo OpenVPN.
>

## <a name="linux"></a>Clientes Linux

1. Abra una nueva sesión de terminal. Para abrir una nueva sesión, presione "CTRL + ALT + T" al mismo tiempo.
2. Escriba el siguiente comando para instalar los componentes necesarios:

   ```
   sudo apt-get install openvpn
   sudo apt-get -y install network-manager-openvpn
   sudo service network-manager restart
   ```
3. Descargue el perfil de VPN para la puerta de enlace. Esto se puede hacer desde la pestaña de configuración de punto a sitio en Azure Portal.
4. [Exporte](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-certificates-point-to-site#clientexport) el certificado de cliente P2P que ha creado y cargado en la configuración de P2S en la puerta de enlace. 
5. Extraiga la clave privada y la huella digital de base64 del archivo .pfx. Hay varias formas de hacerlo. Una de ellas es usar OpenSSL en su equipo.

    ```
    openssl.exe pkcs12 -in "filename.pfx" -nodes -out "profileinfo.txt"
    ```
   El archivo *profileinfo.txt* contendrá la clave privada y la huella digital de la entidad de certificación y el certificado de cliente. Asegúrese de usar la huella digital del certificado de cliente.

6. Abra *profileinfo.txt* en un editor de texto. Para obtener la huella digital del certificado (secundario) de cliente, seleccione el texto (incluido y entre) "---BEGIN CERTIFICATE---" y "---END CERTIFICATE---" para el certificado secundario y cópielo. Puede identificar el certificado secundario si examina la línea subject=/.

7. Abra el archivo *vpnconfig.ovpn* y busque la sección que se muestra a continuación. Reemplace todo lo que aparece entre "cert" y "/cert".

   ```
   # P2S client certificate
   # please fill this field with a PEM formatted cert
   <cert>
   $CLIENTCERTIFICATE
   </cert>
   ```
8. Abra el archivo profileinfo.txt en un editor de texto. Para obtener la clave privada, seleccione el texto incluido y entre "-----BEGIN PRIVATE KEY-----" y "-----END PRIVATE KEY-----" y cópielo.

9. Abra el archivo vpnconfig.ovpn en un editor de texto y busque esta sección. Pegue la clave privada y reemplace todo lo que hay entre "key" y "/key".

   ```
   # P2S client root certificate private key
   # please fill this field with a PEM formatted key
   <key>
   $PRIVATEKEY
   </key>
   ```

10. No cambie los demás campos. Use los datos de la configuración de entrada del cliente para conectarse a la VPN.
11. Para conectarse mediante la línea de comandos, escriba el siguiente comando:
  
    ```
    sudo openvpn –-config <name and path of your VPN profile file>
    ```
12. Para conectarse mediante la GUI, vaya a la configuración del sistema.
13. Haga clic en **+** para agregar una nueva conexión VPN.
14. En **Agregar VPN**, seleccione **Importar desde archivo...** .
15. Vaya al archivo de perfil y haga doble clic o seleccione **Abrir**.
16. Haga clic en **Agregar** en la ventana **Agregar VPN**.
  
    ![Importar desde archivo](./media/vpn-gateway-howto-openvpn-clients/importfromfile.png)
17. Puede conectarse mediante la **activación** de VPN en la página **Configuración de red** o en el icono de red de la bandeja del sistema.

## <a name="next-steps"></a>Pasos siguientes

Si quiere que los clientes VPN tengan acceso a recursos de otra red virtual, siga las instrucciones del artículo [De red virtual a red virtual](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md) para configurar una conexión de red virtual a red virtual. Asegúrese de habilitar BGP en las puertas de enlace y las conexiones; en caso contrario, el tráfico no fluirá.

**"OpenVPN" es una marca comercial de OpenVPN Inc.**
