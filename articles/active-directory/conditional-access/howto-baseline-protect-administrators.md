---
title: 'Directiva de base de referencia Requerir MFA para administradores: Azure Active Directory'
description: Directiva de acceso condicional para requerir autenticación multifactor para los administradores
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
ms.openlocfilehash: 4474283b9a233e39497cd05f0f04ea0984f02401
ms.sourcegitcommit: d3b1f89edceb9bff1870f562bc2c2fd52636fc21
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/04/2019
ms.locfileid: "67560949"
---
# <a name="baseline-policy-require-mfa-for-admins-preview"></a>Directiva de base de referencia: Requerir MFA para administradores (versión preliminar)

Los usuarios con acceso a cuentas con privilegios tienen acceso sin restricciones a su entorno. Dadas las facultades de estas cuentas, debe tratarlas con un cuidado especial. Un método común para mejorar la protección de las cuentas con privilegios es exigir una forma de verificación de la cuenta más estricta cuando se emplean para iniciar sesión. En Azure Active Directory, puede exigir MFA (Multi-Factor Authentication) para conseguir una verificación de cuentas más estricta.

**Requerir MFA para administradores (versión preliminar)**  es un [directiva de base de referencia](concept-baseline-protection.md) que requiere MFA cada vez que uno de los siguientes roles de administrador con privilegios inicia sesión:

* Administrador global
* Administrador de SharePoint
* Administrador de Exchange
* Administrador de acceso condicional
* Administrador de seguridad
* Administrador del departamento de soporte técnico/administrador de contraseñas
* Administrador de facturación
* Administrador de usuarios

Tras la habilitación de la directiva Requerir MFA para administradores directiva, los nueve roles de administrador anteriores deberán registrarse en MFA mediante la aplicación Authenticator. Una vez completado el registro de MFA, los administradores deberán ejecutar MFA cada vez que inicien sesión.

## <a name="deployment-considerations"></a>Consideraciones de la implementación

Dado que la directiva **Requerir MFA para administradores (versión preliminar)** se aplica a todos los administradores críticos, deben realizarse varias consideraciones para garantizar una implementación sin problemas. Estas consideraciones incluyen la identificación de usuarios y entidades de servicio de Azure AD que no pueden o no deben ejecutar MFA, así como aplicaciones y clientes que se usan en la organización y que no admiten la autenticación moderna.

### <a name="legacy-protocols"></a>Protocolos heredados

Los clientes de correo usan los protocolos de autenticación heredados (IMAP, SMTP, POP3, etc.) para realizar solicitudes de autenticación. Estos protocolos no son compatibles con MFA. La mayoría de las cuentas en riesgo que ha encontrado Microsoft se deben a que los delincuentes realizan ataques contra los protocolos heredados al intentar omitir MFA. Para asegurarse de que se requiere MFA al iniciar sesión en una cuenta administrativa y que los delincuentes no pueden omitir MFA, esta directiva bloquea todas las solicitudes de autenticación realizadas a las cuentas de administrador de protocolos heredados.

> [!WARNING]
> Antes de habilitar esta directiva, asegúrese de que los administradores no usan protocolos de autenticación heredados. Consulte el artículo sobre [cómo bloquear la autenticación heredada a Azure AD con acceso condicional](howto-baseline-protect-legacy-auth.md#identify-legacy-authentication-use) para obtener más información.

## <a name="enable-the-baseline-policy"></a>Habilitar la directiva de base de referencia

La **Directiva de base de referencia: Requerir MFA para administradores (versión preliminar)** viene previamente configurada y se mostrará al principio cuando navegue a la hoja Acceso condicional en Azure Portal.

Para habilitar esta directiva y proteger a los administradores:

1. Inicie sesión en  **Azure Portal** como administrador global, administrador de seguridad o administrador de acceso condicional.
1. Vaya a **Azure Active Directory** > **Acceso condicional**.
1. En la lista de directivas, seleccione **Directiva de base de referencia: Requerir MFA para administradores (versión preliminar)** .
1. Establezca **Habilitar directiva** en **Usar la directiva inmediatamente**.
1. Haga clic en **Guardar**.

> [!WARNING]
> Existía una opción **Habilitar automáticamente la directiva en el futuro** cuando esta directiva estaba en versión preliminar. Se ha eliminado esta opción para minimizar el impacto repentino para el usuario. Si seleccionó esta opción cuando estaba disponible, ahora estará seleccionada automáticamente la opción **No usar la directiva**. Si desean usar esta directiva de base de referencia, consulte los pasos anteriores para habilitarla.

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte:

* [Directivas de la protección de base de referencia de acceso condicional](concept-baseline-protection.md)
* [Cinco pasos para asegurar su infraestructura de identidad](../../security/azure-ad-secure-steps.md)
* [¿Qué es el acceso condicional en Azure Active Directory?](overview.md)
