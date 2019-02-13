---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 01/16/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: c6f9065786879749eee6187e93283f4c026b7fff
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2019
ms.locfileid: "55568811"
---
La siguiente configuración del equipo se usó para los pasos siguientes:

  | | |
  |---|---|
  |Equipo| Ubuntu Server 16.04<br>ID_LIKE=debian<br>PRETTY_NAME="Ubuntu 16.04.4 LTS"<br>VERSION_ID="16.04" |
  |Dependencias| strongSwan |

#### <a name="1-install-strongswan"></a>1. Instalación de strongSwan

Ejecute los comandos siguientes para instalar la configuración necesaria de strongSwan:

```
apt-get install strongswan-ikev2 strongswan-plugin-eap-tls
```

```
apt-get install libstrongswan-standard-plugins
```

```
apt-get install strongswan-pki
```

#### <a name="2-generate-keys-and-certificate"></a>2. Generación de claves y certificados

Genere el certificado de entidad de certificación.

  ```
  ipsec pki --gen --outform pem > caKey.pem
  ipsec pki --self --in caKey.pem --dn "CN=VPN CA" --ca --outform pem > caCert.pem
  ```

Imprima el certificado de entidad de certificación en formato base64. Es el formato compatible con Azure. Más adelante lo cargará en Azure como parte de la configuración de P2S.

  ```
  openssl x509 -in caCert.pem -outform der | base64 -w0 ; echo
  ```

Genere el certificado de usuario.

  ```
  export PASSWORD="password"
  export USERNAME="client"

  ipsec pki --gen --outform pem > "${USERNAME}Key.pem"
  ipsec pki --pub --in "${USERNAME}Key.pem" | ipsec pki --issue --cacert caCert.pem --cakey caKey.pem --dn "CN=${USERNAME}" --san "${USERNAME}" --flag clientAuth --outform pem > "${USERNAME}Cert.pem"
  ```

Genere un paquete p12 que contengan el certificado de usuario. Este conjunto de productos se usará en los pasos siguientes cuando se trabaje con los archivos de configuración de cliente.

  ```
  openssl pkcs12 -in "${USERNAME}Cert.pem" -inkey "${USERNAME}Key.pem" -certfile caCert.pem -export -out "${USERNAME}.p12" -password "pass:${PASSWORD}"
  ```