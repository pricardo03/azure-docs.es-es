---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: WenJason
ms.service: vpn-gateway
ms.topic: include
origin.date: 12/11/2018
ms.date: 12/24/2018
ms.author: v-jay
ms.custom: include file
ms.openlocfilehash: 4c8e7e5272f180c482ca7fdd44302f49eb888b25
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60320238"
---
Use un certificado raíz generado mediante una solución empresarial (opción recomendada) o genere un certificado autofirmado. Después de crear el certificado raíz, exporte los datos (no la clave privada) del certificado público como un archivo .cer con codificación Base64 X.509. A continuación, cargue los datos del certificado público en el servidor de Azure.

* **Certificado de empresa:** Si usa una solución empresarial, puede utilizar la cadena de certificados existente. Obtenga el archivo .cer para el certificado raíz que desee usar.
* **Certificado raíz autofirmado**: Si no usa una solución de certificado de empresa, cree un certificado raíz autofirmado. En caso contrario, los certificados que cree no serán compatibles con las conexiones de P2S y los clientes recibirán un error de conexión al intentar conectarse. Puede usar Azure PowerShell, MakeCert o bien OpenSSL. Los pasos descritos en los artículos siguientes describen cómo generar un certificado raíz autofirmado compatible:

  * [Instrucciones para PowerShell en Windows 10](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site.md): estas instrucciones requieren Windows 10 y PowerShell para generar certificados. Los certificados de cliente que se generan desde el certificado raíz pueden instalarse en cualquier cliente P2S compatible.
  * [Instrucciones para MakeCert](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-makecert.md): use MakeCert para generar certificados si no tiene acceso a un equipo con Windows 10. Aunque MakeCert está en desuso, todavía puede usarlo para generar certificados. Los certificados de cliente que se generan desde el certificado raíz pueden instalarse en cualquier cliente P2S compatible.
  * [Instrucciones para Linux](../articles/vpn-gateway/vpn-gateway-certificates-point-to-site-linux.md)
