---
title: 'Obtener ayuda con el portal mis aplicaciones: Azure Active Directory | Microsoft Docs'
description: Obtenga ayuda para iniciar sesión en y realizar tareas comunes en el portal mis aplicaciones.
services: active-directory
author: eross-msft
manager: daveba
ms.service: active-directory
ms.subservice: user-help
ms.workload: identity
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: lizross
ms.reviewer: kasimpso
ms.custom: user-help, seo-update-azuread-jan
ms.collection: M365-identity-device-management
ms.openlocfilehash: a6e87ae0a8adf28176d9a97cbf1b78943179884a
ms.sourcegitcommit: 02d17ef9aff49423bef5b322a9315f7eab86d8ff
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58340115"
---
# <a name="troubleshoot-problems-with-the-my-apps-portal"></a>Solucionar problemas relacionados con el portal mis aplicaciones
Si experimenta problemas con el inicio de sesión en o utilizando el **mis aplicaciones** portal, pruebe estas sugerencias para solucionar problemas antes de que ponerse en contacto con soporte técnico o con el administrador para obtener ayuda.

## <a name="im-having-trouble-installing-the-my-apps-secure-sign-in-extension"></a>Tengo problemas para instalar la extensión de inicio de sesión seguro de mis aplicaciones
Si tiene problemas para instalar la extensión de inicio de sesión seguro de mis aplicaciones:

- Asegúrese de que está utilizando un explorador compatible, incluidos:

    - **Microsoft Edge.** Se está ejecutando en Windows 10 Anniversary Edition o posterior.
    - **Google Chrome.** Se está ejecutando en Windows 7 o versiones posteriores y en Mac OS X o posterior.
    - **Mozilla Firefox 26.0 o posterior.** Se está ejecutando en Windows XP SP2 o versiones posteriores y en Mac OS X 10.6 o posterior.
    - **Internet Explorer 11.** Se está ejecutando en Windows 7 o posterior (compatibilidad limitada).

- Asegúrese de que la configuración de extensión del explorador está activada.

- Intente reiniciar el explorador e inicie sesión en el **mis aplicaciones** portal nuevo.

- Intente borrar las cookies del explorador y, a continuación, reinicie e inicie sesión el **mis aplicaciones** portal nuevo.

## <a name="i-cant-sign-in-to-the-my-apps-portal"></a>No puedo iniciar sesión en el **mis aplicaciones** portal
Si tiene dificultades para iniciar sesión en el **mis aplicaciones** portal, puede intentar lo siguiente:

- Asegúrese de que está utilizando la dirección URL correcta. Debe ser https://myapps.microsoft.com o una página personalizada para su organización, como https://myapps.microsoft.com/contoso.com.

- Asegúrese de que la contraseña es correcta y no ha expirado. Para obtener más información, consulte [restablecer su contraseña profesional o educativa](active-directory-passwords-update-your-own-password.md).

- Asegúrese de que la información de comprobación está actualizada y precisa. Para obtener más información, consulte [¿qué significa Azure Multi-factor Authentication para mí?](multi-factor-authentication-end-user.md) o [cambiar los métodos de información de seguridad y la información de](security-info-add-update-methods-overview.md).

- Agregar el **My App** portal URL a la **propiedades de Internet > seguridad > sitios de confianza** configuración.

- Borrar la caché del explorador e intente iniciar sesión de nuevo.

## <a name="my-password-isnt-working"></a>No funciona mi contraseña
Si nunca ha olvidado la contraseña ha recibido ninguna de su organización, quedan bloqueados fuera de su cuenta o desea cambiar su contraseña, consulte [ayuda, he olvidado mi contraseña de Azure AD](active-directory-passwords-update-your-own-password.md).

## <a name="i-want-to-be-able-to-reset-my-own-password"></a>Quiero poder restablecer mi contraseña
Para poder restablecer la contraseña, el administrador debe primero Active la característica para su organización y, a continuación, debe actualizar y comprobar los métodos de verificación necesario. Para obtener más información sobre cómo actualizar los métodos de comprobación, vea [registrarme para restablecer la contraseña de autoservicio](active-directory-passwords-reset-register.md).

## <a name="im-getting-an-access-denied-message-when-i-start-an-app"></a>Obtengo un mensaje acceso denegado al iniciar una aplicación
Si va a recibir un **acceso denegado** mensaje después de iniciar una aplicación desde el **My App** portal, puede intentar lo siguiente:

- Asegúrese de que ha instalado el [seguro de mis aplicaciones de inicio de sesión de la extensión](my-apps-portal-end-user-access.md#download-and-install-the-my-apps-secure-sign-in-extension) y que esté usando un [explorador compatible](my-apps-portal-end-user-access.md#supported-browsers).

- Asegúrese de que está usando la dirección URL correcta para la aplicación y que se encuentra la dirección URL en su **propiedades de Internet > seguridad > sitios de confianza** lista.

- Asegúrese de que la contraseña es correcta y no ha expirado. Para obtener más información, consulte [restablecer su contraseña profesional o educativa](active-directory-passwords-update-your-own-password.md).

- Asegúrese de que la información de comprobación está actualizada y precisa. Para obtener más información, consulte [¿qué significa Azure Multi-factor Authentication para mí?](multi-factor-authentication-end-user.md) o [cambiar los métodos de información de seguridad y la información de](security-info-add-update-methods-overview.md).

- Borrar la caché del explorador e intente iniciar sesión de nuevo.

Si después de probar todo esto todavía no puede acceder la aplicación, debe ponerse en contacto con soporte técnico de su organización para obtener ayuda.

## <a name="next-steps"></a>Pasos siguientes
Después de iniciar sesión el **mis aplicaciones** portal, también puede actualizar su perfil y la información de la cuenta, la información del grupo y el acceso revisión información (si tiene permiso).

- [Obtener acceso y usar aplicaciones en el portal mis aplicaciones](my-apps-portal-end-user-access.md).

- [Cambiar la información del perfil](my-apps-portal-end-user-update-profile.md).

- [Ver y actualizar la información de grupos](my-apps-portal-end-user-groups.md).

- [Realizar revisiones de acceso de su propio](my-apps-portal-end-user-access-reviews.md).