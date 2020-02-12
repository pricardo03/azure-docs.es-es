---
title: 'Habilitar MFA para usuarios de VPN: Autenticación de Azure AD'
description: Habilitar la autenticación multifactor para los usuarios de VPN
services: vpn-gateway
author: anzaman
ms.service: vpn-gateway
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: alzam
ms.openlocfilehash: 771dea2d9ae2979bc71880368ed3a9a538e8a803
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2020
ms.locfileid: "76964749"
---
# <a name="enable-azure-multi-factor-authentication-mfa-for-vpn-users"></a>Habilitar Azure Multi-Factor Authentication (MFA) para los usuarios de VPN

Si desea que se solicite a los usuarios un segundo factor de autenticación antes de conceder acceso, puede configurar Azure Multi-Factor Authentication (MFA) por usuario o aprovechar MFA mediante el [acceso condicional](../active-directory/conditional-access/overview.md) para un control más específico. La configuración de Multi-Factor Authentication por usuario se puede habilitar sin costo adicional; no obstante, al habilitar MFA por usuario, se solicitará al usuario la autenticación de segundo factor en todas las aplicaciones asociadas al inquilino de Azure AD. El acceso condicional permitirá un mayor control sobre el modo en que se debe promover un segundo factor y puede permitir la asignación de MFA solo a VPN y no a otras aplicaciones asociadas al inquilino de Azure AD.

## <a name="enableauth"></a>Habilitar la autenticación

1. Vaya a **Azure Active Directory  -> Aplicaciones empresariales -> Todas las aplicaciones**.
2. En la página **Aplicaciones empresariales - Todas las aplicaciones**, seleccione **VPN de Azure**.

   ![Identificador de directorio](../../includes/media/vpn-gateway-vwan-openvpn-azure-ad-mfa/user1.jpg)

## <a name="enablesign"></a> Configurar las opciones de inicio de sesión

En la página **VPN de Azure - Propiedades**, configure las opciones de inicio de sesión.

1. En **¿Habilitado para que los usuarios inicien sesión?** , seleccione **Sí**. Esto permite que todos los usuarios del inquilino de AD se conecten correctamente a la VPN.
2. En **Asignación de usuarios necesaria**, seleccione **Sí** si quiere limitar el inicio de sesión solo a los usuarios que tienen permisos para la VPN de Azure.
3. Guarde los cambios.

   ![Permisos](./media/openvpn-azure-ad-mfa/user2.jpg)

## <a name="option-1---enable-multi-factor-authentication-mfa-via-conditional-access"></a>Opción 1: Habilitación de Multi-Factor Authentication (MFA) mediante el acceso condicional.

El acceso condicional permite un control de acceso específico por aplicación.  Tenga en cuenta que para aprovechar el acceso condicional, debe tener licencias de Azure AD Premium 1 o superior aplicadas a los usuarios que estarán sujetos a las reglas de acceso condicional.

1. En la página **Aplicaciones empresariales: Todas las aplicaciones**, seleccione **Azure VPN**, después, **Acceso condicional** y haga clic en **Nueva directiva**.
2. En Usuarios y grupos, en la pestaña *Incluir*, active **Seleccionar usuarios y grupos**, **Usuarios y grupos** y seleccione un grupo o un conjunto de usuarios que deben estar sujetos a MFA.  Haga clic en **Done**(Listo).
![Asignaciones](../../includes/media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-assignments.png)
3. En **Conceder**, active **Conceder acceso**, active **Requerir autenticación multifactor**, active **Requerir todos los controles seleccionados** y haga clic en el botón **Seleccionar**.
![Conceder acceso: MFA](../../includes/media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-grant-mfa.png)
4. Active **Activado** en **Habilitar directiva** y haga clic en el botón **Crear**.
![Habilitar Directiva](../../includes/media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa-ca-enable-policy.png)

## <a name="option-2---enable-multi-factor-authentication-mfa-per-user"></a>Opción 2: Habilitación de Multi-Factor Authentication (MFA) por usuario

[!INCLUDE [MFA steps](../../includes/vpn-gateway-vwan-openvpn-azure-ad-mfa.md)]

## <a name="next-steps"></a>Pasos siguientes

Para conectarse a la red virtual, debe crear y configurar un perfil de cliente VPN. Consulte [Configurar un cliente VPN para conexiones P2S VPN](openvpn-azure-ad-client.md).
