---
title: 'Generación y exportación de certificados para conexiones de punto a sitio: Linux: CLI: Azure | Microsoft Docs'
description: Cree un certificado raíz autofirmado, exporte la clave pública y genere los certificados de cliente mediante la CLI de Linux (strongSwan).
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 01/31/2019
ms.author: cherylmc
ms.openlocfilehash: b673be47d4951adab08f04efc56410095f549356
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2019
ms.locfileid: "55564804"
---
# <a name="generate-and-export-certificates"></a>Generación y exportación de certificados

Las conexiones de punto a sitio utilizan certificados para realizar la autenticación. En este artículo, se muestra cómo crear un certificado raíz autofirmado y generar certificados de cliente con la CLI de Linux y strongSwan. Si busca otras instrucciones de certificado, vea los artículos sobre [Powershell](vpn-gateway-certificates-point-to-site.md) o [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md). Para obtener información sobre cómo instalar strongSwan mediante la GUI en lugar de la CLI, vea los pasos descritos en el artículo [Configuración del cliente](point-to-site-vpn-client-configuration-azure-cert.md#install).

## <a name="generate-and-export"></a>Generación y exportación
[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

## <a name="next-steps"></a>Pasos siguientes

Continúe con la configuración de punto a sitio para [crear e instalar archivos de configuración de cliente de VPN](point-to-site-vpn-client-configuration-azure-cert.md#linuxinstallcli).
