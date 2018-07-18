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
ms.openlocfilehash: 9a17f34333503436d3da340670abdde154e45ef6
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38727537"
---
A partir del 1 de julio de 2018, se elimina la compatibilidad con TLS 1.0 y 1.1 de Azure VPN Gateway. VPN Gateway será solo compatible con TLS 1.2. Para mantener la compatibilidad y la conectividad con TLS para los clientes punto a sitio de Windows 7 y Windows 8 que usan TLS, se recomienda que instale las actualizaciones siguientes:

•   [Actualización de EAP de Microsoft que permite el uso de TLS](https://support.microsoft.com/help/2977292/microsoft-security-advisory-update-for-microsoft-eap-implementation-th)

•   [Actualización para habilitar TLS 1.1 y 1.2 de TLS como protocolos seguros de forma predeterminada de WinHTTP en Windows](https://support.microsoft.com/help/3140245/update-to-enable-tls-1-1-and-tls-1-2-as-a-default-secure-protocols-in)

Los siguientes algoritmos heredados también pasarán a estar en desuso para TLS a partir del 1 de julio de 2018:

* RC4 (Rivest Cipher 4)
* DES (Algoritmo de cifrado de datos)
* 3DES (Triple algoritmo de cifrado de datos)
* MD5 (Código hash 5)
