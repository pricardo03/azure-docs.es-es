---
title: Directiva de línea base exigencia de MFA para que los administradores - Azure Active Directory
description: Directiva de acceso condicional para requerir autenticación multifactor para administradores
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 05/16/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb, rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: a1ce48126c3e8867ac7f2696d8cf7db992a9a60a
ms.sourcegitcommit: 13cba995d4538e099f7e670ddbe1d8b3a64a36fb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/22/2019
ms.locfileid: "66003287"
---
# <a name="baseline-policy-require-mfa-for-admins"></a>Directiva de línea de base: Requerir MFA para administradores

Los usuarios con acceso a cuentas con privilegios tienen acceso sin restricciones a su entorno. Dadas las facultades de estas cuentas, debe tratarlas con un cuidado especial. Un método común para mejorar la protección de las cuentas con privilegios es exigir una forma de verificación de la cuenta más estricta cuando se emplean para iniciar sesión. En Azure Active Directory, puede exigir MFA (Multi-Factor Authentication) para conseguir una verificación de cuentas más estricta.

**Requerir MFA para que los administradores** es un [directiva de línea base](concept-baseline-protection.md) que requiere MFA cada vez que uno de los siguientes roles con privilegios de administrador inicia sesión:

* Administrador global
* Administrador de SharePoint
* Administrador de Exchange
* Administrador de acceso condicional
* Administrador de seguridad
* Administrador de soporte técnico o administrador de contraseñas
* Administrador de facturación
* Administrador de usuarios

Tras la habilitación de la exigencia de MFA para los administradores directiva, los roles de nueve administrador anterior deberán registrarse en MFA mediante la aplicación Authenticator. Una vez completado el registro de MFA, los administradores deberán realizar MFA cada vez que inicio de sesión único.

![Requerir MFA para la directiva de línea de base de los administradores](./media/howto-baseline-protect-administrators/baseline-policy-require-mfa-for-admins.png)

## <a name="deployment-considerations"></a>Consideraciones de la implementación

Dado que el **exigencia de MFA para que los administradores** directiva se aplica a todos los administradores críticos, varias consideraciones deben realizarse para garantizar una implementación sin problemas. Estas consideraciones son la identificación de usuarios y entidades de servicio de Azure AD que no se puede o no se debe realizar la MFA, así como las aplicaciones y los clientes usados por la organización que no admiten la autenticación moderna.

### <a name="legacy-protocols"></a>Protocolos heredados

Protocolos de autenticación heredados (IMAP, SMTP, POP3, etc.) se usan los clientes de correo para realizar solicitudes de autenticación. Estos protocolos no son compatibles con MFA. La mayoría de las cuentas comprometidas vistos por Microsoft está provocada por los actores no válidos la realización de ataques contra los protocolos heredados intenta omitir MFA. Para asegurarse de que se requiere MFA al iniciar sesión en una cuenta administrativa y actores no válidos no pueden omitir MFA, esta directiva bloquea todas las solicitudes de autenticación realizadas a las cuentas de administrador de protocolos heredados.

> [!WARNING]
> Antes de habilitar esta directiva, asegúrese de que los administradores no usa protocolos de autenticación heredados. Consulte el artículo [Cómo: Autenticación heredada de bloque en Azure AD con el acceso condicional](howto-baseline-protect-legacy-auth.md#identify-legacy-authentication-use) para obtener más información.

### <a name="user-exclusions"></a>Exclusiones de usuario

Esta directiva de línea de base proporciona la opción de excluir a los usuarios. Antes de habilitar la directiva para el inquilino, se recomienda excluir las cuentas siguientes:

* **Acceso de emergencia** o **emergencia** cuentas para evitar el bloqueo de cuentas de todos los inquilinos. En el improbable caso de que todos los administradores quedan bloqueados fuera de su inquilino, la cuenta administrativa de acceso de emergencia puede usarse para iniciar sesión en los pasos de inquilinos take para recuperar el acceso.
   * Se puede encontrar más información en el artículo, [administrar cuentas de acceso de emergencia en Azure AD](../users-groups-roles/directory-emergency-access.md).
* **Las cuentas de servicio** y **principios de servicio**, como la cuenta Azure AD Connect Sync. Cuentas de servicio son cuentas no interactivas que no están asociadas a un usuario concreto. Normalmente se utilizan por servicios back-end y permitir el acceso mediante programación a las aplicaciones. Puesto que no se puede completar mediante programación MFA, se deben excluir las cuentas de servicio.
   * Si su organización tiene estas cuentas en uso en scripts o código, considere la posibilidad de reemplazarlos con [administra identidades](../managed-identities-azure-resources/overview.md). Como solución temporal, puede excluir estas cuentas específicas de la directiva de línea de base.
* Usuarios que no tiene o no podrá usar un Smartphone.
   * Esta directiva requiere que los administradores deben registrarse en MFA mediante la aplicación Microsoft Authenticator.

## <a name="enable-the-baseline-policy"></a>Habilitar la directiva de línea base

La directiva **directiva de línea base: Requerir MFA para que los administradores** viene previamente configurada y se mostrarán en la parte superior cuando vaya a la hoja de acceso condicional en Azure portal.

Para habilitar esta directiva y proteger a los administradores:

1. Inicie sesión en el **portal Azure** como administrador global, Administrador de seguridad o administrador de acceso condicional.
1. Vaya a **Azure Active Directory** > **acceso condicional**.
1. En la lista de directivas, seleccione **directiva de línea base: Requerir MFA para que los administradores**.
1. Establecer **Habilitar directiva** a **usar la directiva inmediatamente**.
1. Agregar exclusiones de cualquier usuario haciendo clic en **usuarios** > **Seleccionar usuarios excluidos** y elegir los usuarios que deben excluirse. Haga clic en **seleccione** , a continuación, **realiza**.
1. Haga clic en **guardar**.

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte:

* [Directivas de protección de acceso condicional básicas](concept-baseline-protection.md)
* [Cinco pasos para asegurar su infraestructura de identidad](../../security/azure-ad-secure-steps.md)
* [¿Qué es el acceso condicional en Azure Active Directory?](overview.md)
