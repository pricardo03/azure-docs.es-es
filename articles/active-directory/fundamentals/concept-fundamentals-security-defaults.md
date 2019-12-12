---
title: Valores de seguridad predeterminados de Azure Active Directory
description: Directivas de seguridad predeterminadas que ayudan a proteger a las organizaciones frente a ataques comunes
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 12/06/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: rogoya
ms.collection: M365-identity-device-management
ms.openlocfilehash: d899f477612e4c738314187f61551fe5c0b17f8d
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2019
ms.locfileid: "74932165"
---
# <a name="what-are-security-defaults"></a>¿Cuáles son los valores de seguridad predeterminados?

Administrar la seguridad puede resultar difícil porque los ataques comunes relacionados con la identidad se están volviendo cada vez más populares. Estos ataques incluyen la difusión de contraseñas, reproducción y suplantación de identidades (phishing).

Los valores de seguridad predeterminados en Azure Active Directory (Azure AD) facilitan la protección de la organización. Los valores de seguridad predeterminados contienen opciones de seguridad preconfiguradas para ataques comunes. 

Microsoft pone los valores de seguridad predeterminados a disposición de todos los usuarios. El objetivo es asegurarse de que todas las organizaciones gocen de un nivel básico de seguridad sin ningún costo adicional. Los valores predeterminados de seguridad se activan en Azure Portal.

![Captura de pantalla de Azure Portal con el botón de alternancia para habilitar los valores predeterminados de seguridad](./media/concept-fundamentals-security-defaults/security-defaults-azure-ad-portal.png)
 
Las siguientes configuraciones de seguridad se activarán en el inquilino. 

## <a name="unified-multi-factor-authentication-registration"></a>Registro unificado de Multi-Factor Authentication

Todos los usuarios del inquilino deben registrarse para la autenticación multifactor (MFA) en la forma del servicio Azure Multi-Factor Authentication. Los usuarios tendrán 14 días para registrarse en Multi-Factor Authentication con la aplicación Microsoft Authenticator. Una vez transcurridos los 14 días, el usuario no podrá iniciar sesión hasta que se haya completado el registro de Multi-Factor Authentication.

Sabemos que algunos usuarios pueden estar fuera de la oficina o no iniciarán sesión durante los 14 días inmediatamente después de habilitar los valores de seguridad predeterminados. Para que todos los usuarios tengan tiempo suficiente para registrarse en Multi-Factor Authentication, el período de 14 días es único para cada usuario. El período de 14 días de un usuario comienza después del primer inicio de sesión interactivo correcto después de habilitar los valores de seguridad predeterminados.

## <a name="multi-factor-authentication-enforcement"></a>Aplicación de la autenticación multifactor

### <a name="protecting-administrators"></a>Protección de los administradores

Los usuarios con acceso a cuentas con privilegios tienen un mayor acceso a su entorno. Dadas las facultades de estas cuentas, debe tratarlas con un cuidado especial. Un método común para mejorar la protección de las cuentas con privilegios es exigir una forma de verificación de la cuenta más estricta para iniciar sesión. En Azure AD, puede exigir el uso de Multi-Factor Authentication para conseguir una verificación de cuentas más estricta.

Una vez finalizado el registro con Multi-Factor Authentication, los nueve roles de administrador de Azure AD siguientes deberán realizar una autenticación adicional cada vez que inicien sesión:

- Administrador global
- Administrador de SharePoint
- Administrador de Exchange
- Administrador de acceso condicional
- Administrador de seguridad
- Administrador del departamento de soporte técnico o administrador de contraseñas
- Administrador de facturación
- Administrador de usuarios
- Administrador de autenticación

### <a name="protecting-all-users"></a>Protección de todos los usuarios

Se tiende a pensar que las cuentas de administrador son las únicas cuentas que necesitan capas adicionales de autenticación. Los administradores tienen un amplio acceso a información confidencial y pueden realizar cambios en la configuración de toda la suscripción. Pero los atacantes tienden a dirigirse a usuarios finales. 

Una vez que estos atacantes obtienen acceso, pueden solicitar acceso a información privilegiada en nombre del titular de la cuenta original. Incluso pueden descargar todo el directorio para realizar un ataque de suplantación de identidad (phishing) en toda la organización. 

Un método común para mejorar la protección de todos los usuarios es exigir a todos una forma más estricta de verificación de cuentas, como Multi-Factor Authentication (MFA). Cuando los usuarios finalicen el registro de Multi-Factor Authentication, se les pedirá una autenticación adicional siempre que sea necesario.

### <a name="blocking-legacy-authentication"></a>Bloqueo de la autenticación heredada

Para brindar a los usuarios un acceso sencillo a las aplicaciones en la nube, Azure AD admite una variedad de protocolos de autenticación, incluida la autenticación heredada. La *autenticación heredada* es un término que hace referencia a una solicitud de autenticación realizada por:

- Clientes de Office antiguos que no usan la autenticación moderna (por ejemplo, el cliente de Office 2010).
- Cualquier cliente que use protocolos de correo antiguos, como IMAP, SMTP o POP3.

Hoy en día, la mayoría de los intentos de inicio de sesión que ponen en peligro la seguridad proceden de la autenticación heredada. La autenticación heredada no admite Multi-Factor Authentication. Incluso si tiene una directiva de Multi-Factor Authentication habilitada en el directorio, un atacante puede autenticarse mediante un protocolo antiguo y omitir Multi-Factor Authentication. 

Después de habilitar los valores de seguridad predeterminados en el inquilino, se bloquearán todas las solicitudes de autenticación realizadas con un protocolo antiguo. Los valores de seguridad predeterminados no bloquean Exchange ActiveSync.

### <a name="protecting-privileged-actions"></a>Protección de acciones con privilegios

Las organizaciones usan diversos servicios de Azure que se administran mediante la API de Azure Resource Manager, entre ellos:

- Portal de Azure 
- Azure PowerShell 
- CLI de Azure

El uso de Azure Resource Manager para administrar los servicios es una acción con privilegios elevados. Azure Resource Manager puede modificar las configuraciones de todo el inquilino, como la configuración del servicio y la facturación de la suscripción. La autenticación de factor único es vulnerable a una variedad de ataques, como la suplantación de identidad (phishing) y la difusión de contraseñas. 

Es importante comprobar la identidad de los usuarios que quieren acceder a Azure Resource Manager y actualizar las configuraciones. Para comprobar su identidad, solicite una autenticación adicional antes de permitir el acceso.

Después de habilitar los valores de seguridad predeterminados en el inquilino, se pedirá a todos los usuarios que accedan al Azure Portal, Azure PowerShell o la CLI de Azure que completen una autenticación adicional. Esta directiva se aplica a todos los usuarios que acceden a Azure Resource Manager, independientemente de si son administradores o usuarios. 

Si el usuario no está registrado para Multi-Factor Authentication, deberá registrarse con la aplicación Microsoft Authenticator para poder continuar. No se proporcionará un periodo de registro de 14 días en Multi-Factor Authentication.

## <a name="deployment-considerations"></a>Consideraciones de la implementación

A continuación, se muestran consideraciones adicionales relacionadas con la implementación de los valores de seguridad predeterminados para el inquilino.

### <a name="older-protocols"></a>Protocolos antiguos

Los clientes de correo usan protocolos de autenticación anteriores (IMAP, SMTP y POP3) para realizar solicitudes de autenticación. Estos protocolos no son compatibles con Multi-Factor Authentication. La mayor parte de los peligros para las cuentas que Microsoft detecta provienen de ataques contra los protocolos antiguos que intentan omitir Multi-Factor Authentication. 

Para asegurarse de que se requiere Multi-Factor Authentication al iniciar sesión en una cuenta administrativa y que los atacantes no pueden omitir este paso, los valores de seguridad predeterminados bloquean todas las solicitudes de autenticación realizadas a las cuentas de administrador con protocolos antiguos.

> [!WARNING]
> Antes de habilitar esta configuración, asegúrese de que los administradores no estén usando protocolos de autenticación antiguos. Para más información, consulte [Cómo cambiar la autenticación heredada](concept-fundamentals-block-legacy-authentication.md).

### <a name="conditional-access"></a>Acceso condicional

Puede usar el acceso condicional para configurar directivas que proporcionan el mismo comportamiento que los valores de seguridad predeterminados. Si usa acceso condicional y tiene habilitadas directivas de acceso condicional en su entorno, los valores de seguridad predeterminados no estarán disponibles. Si tiene una licencia que proporciona acceso condicional, pero no tiene ninguna directiva de acceso condicional habilitada en su entorno, puede usar los valores de seguridad predeterminados hasta que habilite las directivas de acceso condicional.

![Mensaje de advertencia que indica que puede tener valores predeterminados de seguridad o acceso condicional, pero no ambos](./media/concept-fundamentals-security-defaults/security-defaults-conditional-access.png)

Estas son las guías paso a paso sobre cómo puede usar el acceso condicional para configurar directivas equivalentes:

- [Exigir autenticación multifactor para administradores](../conditional-access/howto-conditional-access-policy-admin-mfa.md)
- [Exigir autenticación multifactor para la administración de Azure](../conditional-access/howto-conditional-access-policy-azure-management.md)
- [Bloquear la autenticación heredada](../conditional-access/howto-conditional-access-policy-block-legacy.md)
- [Exigir autenticación multifactor para todos los usuarios](../conditional-access/howto-conditional-access-policy-all-users-mfa.md)
- [Requerir el registro de Azure MFA](../identity-protection/howto-identity-protection-configure-mfa-policy.md): Requiere Azure AD Identity Protection

## <a name="enabling-security-defaults"></a>Habilitación de los valores de seguridad predeterminados

Para habilitar los valores de seguridad predeterminados en su directorio:

1. Inicie sesión en  [Azure Portal](https://portal.azure.com) como administrador global, administrador de seguridad o administrador de acceso condicional.
1. Vaya a  **Azure Active Directory** > **Propiedades**.
1. Seleccione **Administrar valores predeterminados de seguridad**.
1. Establezca **Habilitar valores predeterminados de seguridad** en **Sí**.
1. Seleccione **Guardar**.

## <a name="disabling-security-defaults"></a>Deshabilitación de los valores predeterminados de seguridad

Las organizaciones que decidan implementar directivas de acceso condicional que reemplacen los valores predeterminados de seguridad deben deshabilitar estos últimos. 

![Mensaje de advertencia para deshabilitar los valores predeterminados de seguridad y permitir el acceso condicional](./media/concept-fundamentals-security-defaults/security-defaults-disable-before-conditional-access.png)

Para deshabilitar los valores predeterminados de seguridad en el directorio:

1. Inicie sesión en  [Azure Portal](https://portal.azure.com) como administrador global, administrador de seguridad o administrador de acceso condicional.
1. Vaya a  **Azure Active Directory** > **Propiedades**.
1. Seleccione **Administrar valores predeterminados de seguridad**.
1. Establezca **Activación de los valores predeterminados de seguridad**  en **No**.
1. Seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

[Directivas de acceso condicional habituales](../conditional-access/concept-conditional-access-policy-common.md)
