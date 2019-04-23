---
title: 'Definiciones de cookie: Azure Active Directory B2C | Microsoft Docs'
description: Proporciona definiciones para las cookies utilizadas en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ac422a00a919903063c96ac096882036b99a63e3
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59786751"
---
# <a name="cookies-definitions-for-azure-active-directory-b2c"></a>Definiciones de las cookies para Azure Active Directory B2C

En la tabla siguiente se enumera las cookies utilizadas en Azure Active Directory B2C.

| NOMBRE | Dominio | Expiration | Propósito |
| ----------- | ------ | -------------------------- | --------- |
| x-ms-cpim-admin | main.b2cadmin.ext.azure.com | Terminar de [sesión del explorador](active-directory-b2c-token-session-sso.md) | Contiene los datos de pertenencia de usuario entre los inquilinos. Los inquilinos de un usuario es miembro de y nivel de pertenencia (Administrador o usuario). |
| x-ms-cpim-slice | Login.microsoftonline.com, b2clogin.com, dominio con marca | Terminar de [sesión del explorador](active-directory-b2c-token-session-sso.md) | Se utiliza para enrutar las solicitudes a la instancia de producción adecuados. |
| x-ms-cpim-trans | Login.microsoftonline.com, b2clogin.com, dominio con marca | Terminar de [sesión del explorador](active-directory-b2c-token-session-sso.md) | Se utiliza para realizar el seguimiento de las transacciones (número de solicitudes de autenticación a Azure AD B2C) y la transacción actual. |
| x-ms-cpim-sso:{Id} | Login.microsoftonline.com, b2clogin.com, dominio con marca | Terminar de [sesión del explorador](active-directory-b2c-token-session-sso.md) | Se utiliza para mantener la sesión de inicio de sesión único. |
| x-ms-cpim-cache:{id}_n | Login.microsoftonline.com, b2clogin.com, dominio con marca | Terminar de [sesión del explorador](active-directory-b2c-token-session-sso.md), una autenticación correcta | Se utiliza para mantener el estado de solicitud. |
| x-ms-cpim-csrf | Login.microsoftonline.com, b2clogin.com, dominio con marca | Terminar de [sesión del explorador](active-directory-b2c-token-session-sso.md) | Token de falsificación de solicitud entre sitios usado para la protección de CRSF. |
| x-ms-cpim-dc | Login.microsoftonline.com, b2clogin.com, dominio con marca | Terminar de [sesión del explorador](active-directory-b2c-token-session-sso.md) | Se utiliza para el enrutamiento de red de Azure AD B2C. |
| x-ms-cpim-ctx | Login.microsoftonline.com, b2clogin.com, dominio con marca | Terminar de [sesión del explorador](active-directory-b2c-token-session-sso.md) | Context |
| x-ms-cpim-rp | Login.microsoftonline.com, b2clogin.com, dominio con marca | Terminar de [sesión del explorador](active-directory-b2c-token-session-sso.md) | Se usa para almacenar datos de pertenencia en el inquilino del proveedor de recursos. |
| x-ms-cpim-rc | Login.microsoftonline.com, b2clogin.com, dominio con marca | Terminar de [sesión del explorador](active-directory-b2c-token-session-sso.md) | Se usa para almacenar la cookie de retransmisión. |

