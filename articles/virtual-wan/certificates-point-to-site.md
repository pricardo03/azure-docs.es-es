---
title: Generación y exportación de certificados para conexiones VPN de usuario de Azure Virtual WAN | Microsoft Docs
description: Cree un certificado raíz autofirmado, exporte la clave pública y genere los certificados de cliente mediante PowerShell en Windows 10 o Windows Server 2016.
services: virtual-wan
author: cherylmc
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 10/09/2019
ms.author: cherylmc
ms.openlocfilehash: 57d730a92c687a297a35b8cd6cccd955025694af
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73510925"
---
# <a name="generate-and-export-certificates-for-virtual-wan-user-vpn-connections"></a>Generación y exportación de certificados para conexiones VPN de usuario de Virtual WAN

Las conexiones VPN de usuario utilizan certificados para realizar la autenticación. En este artículo, se muestra cómo crear un certificado raíz autofirmado y generar certificados de cliente usando PowerShell en Windows 10 o Windows Server 2016.

Debe realizar los pasos de este artículo en un equipo que ejecute Windows 10 o Windows Server 2016. Los cmdlets de PowerShell que se usan para generar certificados forman parte del sistema operativo y no funcionan en otras versiones de Windows. El equipo con Windows 10 o Windows Server 2016 solo es necesario para generar los certificados. Una vez que se generan los certificados, puede cargarlos o instalarlos en cualquier sistema operativo cliente compatible.

[!INCLUDE [Export public key](../../includes/vpn-gateway-generate-export-certificates-include.md)]

## <a name="next-steps"></a>Pasos siguientes

Continúe con los [Pasos de Virtual WAN para la conexión VPN de usuario](virtual-wan-about.md)
