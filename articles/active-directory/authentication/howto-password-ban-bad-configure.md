---
title: 'Prohibición de contraseñas inseguras en Azure AD: Azure Active Directory'
description: Prohibición de contraseñas no seguras del entorno con las contraseñas prohibidas dinámicamente de Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: c58371edffa4400c2c2b3efd37c3ce6d7c3cad95
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/05/2019
ms.locfileid: "74847922"
---
# <a name="configuring-the-custom-banned-password-list"></a>Configuración de la lista personalizada de contraseñas prohibidas

Los usuarios de muchas organizaciones emplean palabras locales comunes, como un colegio, equipo deportivo o persona famosa, para crear las contraseñas, lo que hace que sean fáciles de averiguar. La lista personalizada de contraseñas prohibidas de Microsoft permite a las organizaciones agregar cadenas que evalúan y bloquean, además de la lista global de contraseñas prohibidas, cuando usuarios y administradores intentan cambiar o restablecer una contraseña.

## <a name="add-to-the-custom-list"></a>Adición a la lista personalizada

La configuración de la lista personalizada de contraseñas prohibidas requiere una licencia P1 o P2 de Azure Active Directory Premium. Para más información acerca de las licencias de Azure Active Directory, consulte la página [Precios de Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

1. Inicie sesión en [Azure Portal](https://portal.azure.com) y vaya a **Azure Active Directory**, **Métodos de autenticación** y **Protección con contraseña**.
1. En **Enforce custom list** (Exigir lista personalizada), seleccione la opción **Yes** (Sí).
1. Agregue cadenas a la **lista personalizada de contraseñas prohibidas**, una cadena por línea
   * La lista personalizada de contraseñas prohibidas puede contener hasta 1000 términos.
   * La lista personalizada de contraseñas prohibidas distingue mayúsculas de minúsculas.
   * La lista personalizada de contraseñas prohibidas considera la sustitución de caracteres comunes.
      * Ejemplo: "o" y "0" o "a" y "\@"
   * La longitud mínima de la cadena es cuatro caracteres, mientras que la máxima es 16 caracteres.
1. Cuando haya agregado todas las cadenas, haga clic en **Guardar**.

> [!NOTE]
> Las actualizaciones a la lista personalizada de contraseñas prohibidas pueden tardar varias horas en aplicarse.

> [!NOTE]
> La lista personalizada de contraseñas prohibidas se limita a tener un máximo de 1000 términos. No está diseñado para bloquear listas muy grandes de contraseñas. Para aprovechar al máximo las ventajas de la lista personalizada de contraseñas prohibidas, Microsoft recomienda que primero revise y comprenda el diseño y uso previstos de la lista personalizada de contraseñas prohibidas (consulte [Lista personalizada de contraseñas prohibidas](concept-password-ban-bad.md#custom-banned-password-list)), así como el algoritmo de evaluación de contraseñas (consulte [Cómo se evalúan las contraseñas](concept-password-ban-bad.md#how-are-passwords-evaluated)).

![Modifique la lista personalizada de contraseñas prohibidas en Método de autenticación de Azure Portal](./media/howto-password-ban-bad/authentication-methods-password-protection.png)

## <a name="how-it-works"></a>Cómo funciona

Cada vez que un usuario o un administrador restablecen o cambian la contraseña de Azure AD, esta atraviesa la lista de contraseñas prohibidas para confirmar que no está en ella. Esta comprobación se incluye en todas las contraseñas establecidas o cambiadas mediante Azure AD.

## <a name="what-do-users-see"></a>Lo que ven los usuarios

Si un usuario intenta restablecer una contraseña a algo que estaría prohibido, verá uno de los siguientes mensajes de error:

* Lamentablemente, la contraseña contiene una palabra, frase o patrón que permite adivinar la contraseña fácilmente. Vuelva a intentarlo con una contraseña diferente.
* No puede usar esta contraseña porque contiene palabras o caracteres que el administrador ha bloqueado. Vuelva a intentarlo con una contraseña diferente.

## <a name="next-steps"></a>Pasos siguientes

[Conceptual overview of the banned password lists](concept-password-ban-bad.md) (Introducción al concepto de lista de contraseñas prohibidas)

[Conceptual overview of Azure AD password protection](concept-password-ban-bad-on-premises.md) (Introducción al concepto de protección de contraseñas de Azure AD)

[Habilitación de la integración local con las listas de contraseñas prohibidas](howto-password-ban-bad-on-premises.md)
