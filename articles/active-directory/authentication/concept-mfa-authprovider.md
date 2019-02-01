---
title: ¿Cuándo y cómo usar un proveedor de Azure Multi-factor Authentication?
description: ¿Cuándo se debe usar un proveedor de autenticación con Azure MFA?
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/27/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.openlocfilehash: 2d7156a1387184cff2eb4252648f0e844e8b1f50
ms.sourcegitcommit: 58dc0d48ab4403eb64201ff231af3ddfa8412331
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/26/2019
ms.locfileid: "55079722"
---
# <a name="when-to-use-an-azure-multi-factor-authentication-provider"></a>Cuándo usar un proveedor de Azure Multi-Factor Authentication

La verificación en dos pasos está disponible de forma predeterminada para los administradores globales que tienen usuarios de Azure Active Directory y Office 365. Sin embargo, si desea aprovechar las [características avanzadas](howto-mfa-mfasettings.md), debe adquirir la versión completa de Azure Multi-Factor Authentication (MFA).

Un proveedor de Azure Multi-Factor Authentication se usa para aprovechar las características que Azure Multi-factor Authentication proporciona para los usuarios que **no tienen licencias**.

Si tiene licencias que cubren a todos los usuarios de la organización, no necesita un Proveedor de Azure Multi-Factor Authentication. Solo cree un proveedor de Azure Multi-Factor Authentication si también tiene que ptoporcionar verificación en dos pasos para algunos usuarios que no tienen licencia.

> [!NOTE]
> A partir del 1 de septiembre de 2018, no se pueden crear nuevos clientes de autenticación. Los proveedores de autenticación existentes se pueden seguir usando y actualizando. La autenticación multifactor seguirá estando disponible como una característica en las licencias de Azure AD Premium.

## <a name="caveats-related-to-the-azure-mfa-sdk"></a>Advertencias relacionadas con el SDK de Azure MFA

Tenga en cuenta que el SDK ha quedado en desuso y solo seguirá funcionando hasta el 14 de noviembre de 2018. Después de ese momento, se producirá un error en las llamadas al SDK.

## <a name="what-is-an-mfa-provider"></a>¿Qué es un proveedor de MFA?

Hay dos tipos de proveedores de autenticación y la diferencia radica en cómo se aplicarán cargos a su suscripción de Azure. La opción "por autenticación" calcula el número de autenticaciones que se realizan en el inquilino en un mes. Esta opción es la más adecuada si tiene un número de usuarios que se autentican solo en algunas ocasiones. La opción "por usuario" calcula el número de personas en el inquilino que realizan la verificación en dos pasos en un mes. Esta opción es mejor si tiene algunos usuarios con licencias pero necesita ampliar MFA a más usuarios de los permitidos por su licencia.

## <a name="manage-your-mfa-provider"></a>Administración del proveedor de MFA

No se puede cambiar el modelo de uso (por usuario habilitado o por autenticación) después de crear un proveedor de MFA.

Si compró licencias suficientes para cubrir todos los usuarios que están habilitados para MFA, puede eliminar por completo el proveedor de MFA.

Si el proveedor de MFA no está vinculado a un inquilino de Azure AD o si vincula el nuevo proveedor de MFA a un inquilino de Azure AD diferente, las opciones de configuración y parámetros de usuario no se transferirán. Además, los servidores Azure MFA existentes se tendrán que reactivar mediante las credenciales de activación generadas a través del proveedor de MFA. La reactivación de los servidores MFA para vincularlos al proveedor de MFA no afecta a la autenticación por llamada telefónica y mensaje de texto, pero las notificaciones de aplicación móvil dejarán de funcionar para todos los usuarios hasta que reactiven la aplicación móvil.

## <a name="next-steps"></a>Pasos siguientes

[Configuración de Azure Multi-Factor Authentication](howto-mfa-mfasettings.md)
