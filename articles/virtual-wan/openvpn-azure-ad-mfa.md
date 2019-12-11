---
title: 'Habilitar MFA para usuarios de VPN: Autenticación de Azure AD'
description: Habilitar la autenticación multifactor para los usuarios de VPN
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: alzam
ms.openlocfilehash: 3700014752233e29d157f1695906babe4fa35c74
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2019
ms.locfileid: "74820418"
---
# <a name="enable-azure-multi-factor-authentication-mfa-for-vpn-users"></a>Habilitar Azure Multi-Factor Authentication (MFA) para los usuarios de VPN

Si quiere que se solicite a los usuarios un segundo factor de autenticación antes de conceder acceso, puede configurar Azure Multi-Factor Authentication (MFA) para su inquilino de Azure AD. Los pasos de este artículo le ayudan a habilitar un requisito para la verificación en dos pasos.

## <a name="prereq"></a>Requisito previo

El requisito previo para esta configuración es un inquilino de Azure AD configurado mediante los pasos descritos en [Configurar un inquilino](openvpn-azure-ad-tenant.md).

## <a name="mfa"></a>Abrir la página de MFA

1. Inicie sesión en el Portal de Azure.
2. Vaya a **Azure Active Directory -> Todos los usuarios**.
3. Seleccione **Multi-Factor Authentication** para abrir la página de la autenticación multifactor.

   ![Iniciar sesión](./media/openvpn-azure-ad-mfa/mfa1.jpg)

## <a name="users"></a> Seleccionar usuarios

1. En la página de la **autenticación multifactor**, seleccione los usuarios para los que quiera habilitar MFA.
2. Seleccione **Habilitar**.

   ![Seleccionar](./media/openvpn-azure-ad-mfa/mfa2.jpg)

## <a name="enableauth"></a>Habilitar la autenticación

1. Vaya a **Azure Active Directory  -> Aplicaciones empresariales -> Todas las aplicaciones**.
2. En la página **Aplicaciones empresariales - Todas las aplicaciones**, seleccione **VPN de Azure**.

   ![Identificador de directorio](./media/openvpn-azure-ad-mfa/user1.jpg)

## <a name="enablesign"></a> Configurar las opciones de inicio de sesión

En la página **VPN de Azure - Propiedades**, configure las opciones de inicio de sesión.

1. En **¿Habilitado para que los usuarios inicien sesión?** , seleccione **Sí**. Esto permite que todos los usuarios del inquilino de AD se conecten correctamente a la VPN.
2. En **Asignación de usuarios necesaria**, seleccione **Sí** si quiere limitar el inicio de sesión solo a los usuarios que tienen permisos para la VPN de Azure.
3. Guarde los cambios.

   ![Permisos](./media/openvpn-azure-ad-mfa/user2.jpg)

## <a name="next-steps"></a>Pasos siguientes

Para conectarse a la red virtual, debe crear y configurar un perfil de cliente VPN. Consulte [Configuración de la autenticación de Azure AD para la conexión de punto a sitio con Azure](virtual-wan-point-to-site-azure-ad.md).
