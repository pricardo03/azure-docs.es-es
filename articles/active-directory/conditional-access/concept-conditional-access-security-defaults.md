---
title: Valores de seguridad predeterminados de Azure Active Directory
description: Directivas de seguridad predeterminadas diseñadas para proteger a las organizaciones frente a ataques comunes
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: b4921f0605f16a601f6e2ee9a15b71b50d253201
ms.sourcegitcommit: 77bfc067c8cdc856f0ee4bfde9f84437c73a6141
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/16/2019
ms.locfileid: "72452926"
---
# <a name="what-are-security-defaults"></a>¿Cuáles son los valores de seguridad predeterminados?

Administrar la seguridad puede resultar difícil porque los ataques comunes relacionados con la identidad, como la difusión o reproducción de contraseñas y la suplantación de identidad, se están volviendo cada vez más populares. Crear una forma más sencilla de proteger su organización de estos ataques comunes es el objetivo de los valores de seguridad predeterminados de Azure Active Directory (AD). Los valores de seguridad predeterminados facilita la protección de la organización frente a ataques comunes. Los valores de seguridad predeterminados contienen opciones de seguridad preconfiguradas para estos ataques comunes. Microsoft pone los valores de seguridad predeterminados a disposición de todos los usuarios. El objetivo es asegurarse de que todas las organizaciones gocen de un nivel básico de seguridad sin ningún costo adicional.

![Captura de pantalla de la nueva experiencia de usuario de los valores de seguridad predeterminados](./media/concept-conditional-access-security-defaults/security-defaults-azure-ad-portal.png)
 
Las siguientes configuraciones de seguridad se activarán en el inquilino. 

## <a name="unified-mfa-registration"></a>Registro de MFA unificado

Todos los usuarios del inquilino deberán registrarse en Azure Multi-Factor Authentication (MFA). Los usuarios tendrán 14 días para registrarse en Azure MFA con la aplicación Microsoft Authenticator. Una vez transcurridos los 14 días, el usuario no podrá iniciar sesión hasta que se haya completado el registro de MFA.

Sabemos que algunos usuarios pueden estar fuera de la oficina o que no iniciarán sesión durante los 14 días inmediatamente después de habilitar los valores de seguridad predeterminados. Para que todos los usuarios tengan tiempo suficiente para registrarse en MFA, el período de 14 días es único para cada usuario. El período de 14 días de un usuario comienza después del primer inicio de sesión interactivo correcto una vez habilitados los valores de seguridad predeterminados.

## <a name="mfa-enforcement"></a>Aplicación de MFA

### <a name="protecting-administrators"></a>Protección de los administradores

Los usuarios con acceso a cuentas con privilegios tienen un mayor acceso a su entorno. Dadas las facultades de estas cuentas, debe tratarlas con un cuidado especial. Un método común para mejorar la protección de las cuentas con privilegios es exigir una forma de verificación de la cuenta más estricta cuando se emplean para iniciar sesión. En Azure Active Directory, puede exigir la autenticación multifactor para conseguir una verificación de cuentas más estricta.

Una vez completado el registro de MFA, se requerirá a los nueve roles de administrador de Azure AD siguientes que usen la autenticación multifactor cada vez que inicien sesión.

- Administrador global
- Administrador de SharePoint
- Administrador de Exchange
- Administrador de acceso condicional
- Administrador de seguridad
- Administrador del departamento de soporte técnico/administrador de contraseñas
- Administrador de facturación
- Administrador de usuarios
- Administrador de autenticación

### <a name="protecting-all-users"></a>Protección de todos los usuarios

Se tiende a pensar que las cuentas de administrador son las únicas cuentas que necesitan protección con autenticación multifactor (MFA). Los administradores tienen un amplio acceso a información confidencial y pueden realizar cambios en la configuración de toda la suscripción. Pero los delincuentes tienden a dirigirse a usuarios finales. Después de obtener acceso, estos delincuentes pueden solicitar acceso a información privilegiada en nombre del titular original de la cuenta o descargar todo el directorio para realizar un ataque de suplantación de identidad en toda la organización. Un método común para mejorar la protección de todos los usuarios es exigir a todos una forma más estricta de verificación de cuentas, como la autenticación multifactor (MFA). Una vez que se haya completado el registro de MFA, se pedirá a los usuarios que lo usen cuando sea necesario.

### <a name="blocking-legacy-authentication"></a>Bloqueo de la autenticación heredada

Para brindar a los usuarios un acceso sencillo a las aplicaciones en la nube, Azure Active Directory (Azure AD) admite una amplia variedad de protocolos de autenticación, incluida la autenticación heredada. La autenticación heredada es un término que hace referencia a una solicitud de autenticación realizada por:

- Clientes de Office antiguos que no usan la autenticación moderna (por ejemplo, el cliente de Office 2010)
- Cualquier cliente que use protocolos de correo heredados, como IMAP, SMTP o POP3

Hoy en día, la mayoría de los intentos de inicio de sesión que ponen en peligro la seguridad proceden de la autenticación heredada. La autenticación heredada no admite la autenticación multifactor (MFA). Aunque tenga una directiva MFA habilitada en su directorio, un actor malintencionado puede autenticarse mediante un protocolo heredado y omitir MFA. Una vez que los valores de seguridad predeterminados estén habilitados en el inquilino, se bloquearán todas las solicitudes de autenticación realizadas con un protocolo heredado. Los valores de seguridad predeterminados no bloquean Exchange ActiveSync.

### <a name="protecting-privileged-actions"></a>Protección de acciones con privilegios

Las organizaciones usan diversos servicios de Azure que se administran mediante la API de Azure Resource Manager, entre ellos:

- Portal de Azure 
- Azure PowerShell 
- CLI de Azure

El uso de Azure Resource Manager para administrar los servicios es una acción con privilegios elevados. Azure Resource Manager puede modificar las configuraciones de todo el inquilino, como la configuración del servicio y la facturación de la suscripción. La autenticación de factor único es vulnerable a una variedad de ataques, como la suplantación de identidad (phishing) y la difusión de contraseña. Por lo tanto, es importante comprobar la identidad de los usuarios que quieren acceder a Azure Resource Manager y actualizar las configuraciones. Para ello, se debe requerir la autenticación multifactor antes de permitir el acceso.

Una vez que los valores de seguridad predeterminados estén habilitados en el inquilino, se pedirá a todos los usuarios que accedan al Azure Portal, Azure PowerShell o la CLI de Azure que completen la autenticación multifactor. Esta directiva se aplica a todos los usuarios que acceden a Azure Resource Manager, independientemente de si son administradores o usuarios. Si el usuario no está registrado para MFA, deberá registrarse con la aplicación Microsoft Authenticator para poder continuar. No se proporcionará ningún período de registro de MFA de 14 días.

## <a name="deployment-considerations"></a>Consideraciones de la implementación

A continuación, se muestran algunas consideraciones adicionales relacionadas con la implementación de los valores de seguridad predeterminados para el inquilino.

### <a name="legacy-protocols"></a>Protocolos heredados

Los clientes de correo usan los protocolos de autenticación heredados (IMAP, SMTP, POP3, etc.) para realizar solicitudes de autenticación. Estos protocolos no son compatibles con MFA. La mayoría de las cuentas en riesgo que ha encontrado Microsoft se deben a que los delincuentes realizan ataques contra los protocolos heredados al intentar omitir MFA. Para asegurarse de que se requiere MFA al iniciar sesión en una cuenta administrativa y que los delincuentes no pueden omitir la MFA, los valores de seguridad predeterminados bloquean todas las solicitudes de autenticación realizadas a las cuentas de administrador con protocolos heredados.

> [!WARNING]
> Antes de habilitar esta configuración, asegúrese de que los administradores no estén usando protocolos de autenticación heredados. Para más información, consulte el artículo  [Cómo cambiar la autenticación heredada](concept-conditional-access-block-legacy-authentication.md).

### <a name="conditional-access"></a>Acceso condicional

El acceso condicional se puede usar para configurar directivas que proporcionan el mismo comportamiento que los valores de seguridad predeterminados. Si usa acceso condicional y tiene habilitadas directivas de acceso condicional en su entorno, los valores de seguridad predeterminados no estarán disponibles. Si tiene una licencia que proporciona acceso condicional, pero no tiene ninguna directiva de acceso condicional habilitada en su entorno, puede usar los valores de seguridad predeterminados hasta que habilite las directivas de CA.

![Valores de seguridad predeterminados o acceso condicional, no ambos](./media/concept-conditional-access-security-defaults/security-defaults-conditional-access.png)

Estas son las guías paso a paso para usar el acceso condicional para configurar directivas equivalentes:

- [Exigir autenticación multifactor para administradores](howto-conditional-access-policy-admin-mfa.md)
- [Exigir autenticación multifactor para la administración de Azure](howto-conditional-access-policy-azure-management.md)
- [Bloquear la autenticación heredada](howto-conditional-access-policy-block-legacy.md)

## <a name="enabling-security-defaults"></a>Habilitación de los valores de seguridad predeterminados

Para habilitar los valores de seguridad predeterminados en su directorio:

1. Inicie sesión en  [Azure Portal](https://portal.azure.com) como administrador global, administrador de seguridad o administrador de acceso condicional.
1. Vaya a **Azure Active Directory** > **Propiedades**.
1. Seleccione **Administrar valores predeterminados de seguridad**
1. Establezca **Habilitar valores predeterminados de seguridad** en **Sí**.
1. Haga clic en Guardar.

## <a name="next-steps"></a>Pasos siguientes

[Directivas de acceso condicional habituales](concept-conditional-access-policy-common.md)

[¿Qué es el acceso condicional?](overview.md)
