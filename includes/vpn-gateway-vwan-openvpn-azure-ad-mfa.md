---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 01/16/2020
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 4cef4e09038288e4609f37c4e33f225bd79113be
ms.sourcegitcommit: d29e7d0235dc9650ac2b6f2ff78a3625c491bbbf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2020
ms.locfileid: "76166626"
---
## <a name="mfa"></a>Abrir la página de MFA

1. Inicie sesión en Azure Portal.
2. Vaya a **Azure Active Directory -> Todos los usuarios**.
3. Seleccione **Multi-Factor Authentication** para abrir la página de la autenticación multifactor.

   ![Iniciar sesión](./media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa1.jpg)

## <a name="users"></a> Seleccionar usuarios

1. En la página de la **autenticación multifactor**, seleccione los usuarios para los que quiera habilitar MFA.
2. Seleccione **Habilitar**.

   ![Seleccionar](./media/vpn-gateway-vwan-openvpn-azure-ad-mfa/mfa2.jpg)

## <a name="enableauth"></a>Habilitar la autenticación

1. Vaya a **Azure Active Directory  -> Aplicaciones empresariales -> Todas las aplicaciones**.
2. En la página **Aplicaciones empresariales - Todas las aplicaciones**, seleccione **VPN de Azure**.

   ![Identificador de directorio](./media/vpn-gateway-vwan-openvpn-azure-ad-mfa/user1.jpg)
