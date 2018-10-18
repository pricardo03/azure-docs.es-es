---
title: 'Generación y exportación de certificados para conexiones de punto a sitio: Linux: CLI: Azure | Microsoft Docs'
description: Cree un certificado raíz autofirmado, exporte la clave pública y genere los certificados de cliente mediante la CLI de Linux (strongSwan).
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 09/06/2018
ms.author: cherylmc
ms.openlocfilehash: 8647b3b3eda980dbd5d5ec368b6b4b13949ecaf1
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2018
ms.locfileid: "44305734"
---
# <a name="generate-and-export-certificates-for-point-to-site-using-linux-strongswan-cli"></a>Generación y exportación de certificados para conexiones de punto a sitio con la CLI strongSwan de Linux

Las conexiones de punto a sitio utilizan certificados para realizar la autenticación. En este artículo, se muestra cómo crear un certificado raíz autofirmado y generar certificados de cliente con la CLI de Linux y strongSwan. Si busca otras instrucciones de certificado, vea los artículos sobre [Powershell](vpn-gateway-certificates-point-to-site.md) o [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md).

> [!NOTE]
> Los pasos de este artículo requieren strongSwan.
>

La configuración del equipo que se ha usado para los pasos de este artículo es la siguiente:

| | |
|---|---|
|**Equipo**| Ubuntu Server 16.04<br>ID_LIKE=debian<br>PRETTY_NAME="Ubuntu 16.04.4 LTS"<br>VERSION_ID="16.04" |
|**Dependencias**| apt-get install strongswan-ikev2 strongswan-plugin-eap-tls<br>apt-get install libstrongswan-standard-plugins |

## <a name="install-strongswan"></a>Instalación de strongSwan

1. `apt-get install strongswan-ikev2 strongswan-plugin-eap-tls`
2. `apt-get install libstrongswan-standard-plugins`

Para obtener información sobre cómo instalar strongSwan mediante la GUI, vea los pasos descritos en el artículo [Configuración del cliente](point-to-site-vpn-client-configuration-azure-cert.md#install).

## <a name="generate-keys-and-certificate"></a>Generación de claves y certificados

1. Genere el certificado de entidad de certificación.

  ```
  ipsec pki --gen --outform pem > caKey.pem
  ipsec pki --self --in caKey.pem --dn "CN=VPN CA" --ca --outform pem > caCert.pem
  ```
2. Imprima el certificado de entidad de certificación en formato base64. Es el formato compatible con Azure. Más adelante lo cargará en Azure como parte de la configuración de P2S.

  ```
  openssl x509 -in caCert.pem -outform der | base64 -w0 ; echo
  ```
3. Genere el certificado de usuario.

  ```
  export PASSWORD="password"
  export USERNAME="client"

  ipsec pki --gen --outform pem > "${USERNAME}Key.pem"
  ipsec pki --pub --in "${USERNAME}Key.pem" | ipsec pki --issue --cacert caCert.pem --cakey caKey.pem --dn "CN=${USERNAME}" --san "${USERNAME}" --flag clientAuth --outform pem > "${USERNAME}Cert.pem"
  ```
4. Genere un paquete p12 que contengan el certificado de usuario. Este paquete se usará en los pasos siguientes cuando se trabaje con los [archivos de configuración de cliente](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli).

  ```
  openssl pkcs12 -in "${USERNAME}Cert.pem" -inkey "${USERNAME}Key.pem" -certfile caCert.pem -export -out "${USERNAME}.p12" -password "pass:${PASSWORD}"
  ```

## <a name="next-steps"></a>Pasos siguientes

Continúe con la configuración de punto a sitio para [crear e instalar archivos de configuración de cliente de VPN](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli).