---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 06/05/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 383c4a53913333a3e25006980dd7533b9e243a4a
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "35236698"
---
A partir del 1 de julio de 2018, se elimina la compatibilidad con TLS 1.0 y 1.1 de Azure VPN Gateway. VPN Gateway será solo compatible con TLS 1.2. Para mantener la compatibilidad y la conectividad con TLS para los clientes punto a sitio de Windows 7 y Windows 8 que usan TLS, se recomienda que instale las actualizaciones siguientes:

•   [Actualización de EAP de Microsoft que permite el uso de TLS](https://support.microsoft.com/help/2977292/microsoft-security-advisory-update-for-microsoft-eap-implementation-th)

•   [Actualización para habilitar TLS 1.1 y 1.2 de TLS como protocolos seguros de forma predeterminada de WinHTTP en Windows](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-a-default-secure-protocols-in)

Los siguientes algoritmos heredados también pasarán a estar en desuso para TLS a partir del 1 de julio de 2018:

* RC4 (Rivest Cipher 4)
* DES (Algoritmo de cifrado de datos)
* 3DES (Triple algoritmo de cifrado de datos)
* MD5 (Código hash 5)
* SHA-1 (Algoritmo hash seguro 1)