---
title: 'Habilitar MFA para usuarios de VPN: Autenticación de Azure AD'
description: Habilitar la autenticación multifactor para los usuarios de VPN
services: virtual-wan
author: anzaman
ms.service: virtual-wan
ms.topic: conceptual
ms.date: 01/16/2020
ms.author: alzam
ms.openlocfilehash: fcb60e80189da89b3f634c14582be606307536e6
ms.sourcegitcommit: d29e7d0235dc9650ac2b6f2ff78a3625c491bbbf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76166673"
---
# <a name="enable-azure-multi-factor-authentication-mfa-for-vpn-users"></a>Habilitar Azure Multi-Factor Authentication (MFA) para los usuarios de VPN

Si quiere que se solicite a los usuarios un segundo factor de autenticación antes de conceder acceso, puede configurar Azure Multi-Factor Authentication (MFA) para su inquilino de Azure AD. Los pasos de este artículo le ayudan a habilitar un requisito para la verificación en dos pasos.

## <a name="prereq"></a>Requisito previo

El requisito previo para esta configuración es un inquilino de Azure AD configurado mediante los pasos descritos en [Configurar un inquilino](openvpn-azure-ad-tenant.md).

[!INCLUDE [MFA steps](../../includes/vpn-gateway-vwan-openvpn-azure-ad-mfa.md)]

## <a name="enablesign"></a> Configurar las opciones de inicio de sesión

En la página **VPN de Azure - Propiedades**, configure las opciones de inicio de sesión.

1. En **¿Habilitado para que los usuarios inicien sesión?** , seleccione **Sí**. Esto permite que todos los usuarios del inquilino de AD se conecten correctamente a la VPN.
2. En **Asignación de usuarios necesaria**, seleccione **Sí** si quiere limitar el inicio de sesión solo a los usuarios que tienen permisos para la VPN de Azure.
3. Guarde los cambios.

   ![Permisos](./media/openvpn-azure-ad-mfa/user2.jpg)

## <a name="next-steps"></a>Pasos siguientes

Para conectarse a la red virtual, debe crear y configurar un perfil de cliente VPN. Consulte [Configuración de la autenticación de Azure AD para la conexión de punto a sitio con Azure](virtual-wan-point-to-site-azure-ad.md).
