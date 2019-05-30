---
title: 'Línea base directiva bloque autenticación heredados (versión preliminar): Azure Active Directory'
description: Directiva de acceso condicional para bloquear protocolos de autenticación heredados
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
ms.openlocfilehash: e7eebc68ae8a55d636f3bc85e179bd7d6813be8d
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235555"
---
# <a name="baseline-policy-block-legacy-authentication-preview"></a>Directiva de línea de base: Autenticación heredados de bloque (versión preliminar)

Para brindar a los usuarios un acceso sencillo a las aplicaciones en la nube, Azure Active Directory (Azure AD) admite una amplia variedad de protocolos de autenticación, incluida la autenticación heredada. Autenticación heredados es un término que se hace referencia a una solicitud de autenticación realizada por:

* Clientes de Office más antiguos que no usan autenticación moderna (por ejemplo, el cliente de Office 2010)
* Cualquier cliente que usa los protocolos de correo antiguos, como IMAP, SMTP/POP3

Hoy en día, la mayoría de todos los intentos de inicio de sesión de poner en peligro proceden de autenticación heredados. La autenticación heredada no admite la autenticación multifactor (MFA). Incluso si tiene una directiva MFA habilitada en su directorio, un actor perjudicial puede autenticar mediante un protocolo heredado y omitir MFA.

La mejor manera de proteger la cuenta de las solicitudes de autenticación malintencionado realizadas por los protocolos heredados es bloquear todos juntos estos intentos. Para facilitar la bloquear todas las solicitudes de inicio de sesión realizadas por los protocolos heredados, hemos creado una directiva de línea de base que hace justamente eso.

![Autenticación heredados de bloque con el acceso condicional](./media/howto-baseline-protect-legacy-auth/baseline-policy-block-legacy-authentication.png)

**Autenticación heredados bloque** es [directiva de línea base](concept-baseline-protection.md) que bloquea todas las solicitudes de autenticación realizadas desde protocolos heredados. Autenticación moderna debe usarse para iniciar sesión correctamente para todos los usuarios. Se usa junto con las otras directivas de línea de base, se bloquearán todas las solicitudes procedentes de protocolos heredados y todos los usuarios deberán MFA siempre que sea necesario. Esta directiva no bloquea a Exchange ActiveSync.

## <a name="identify-legacy-authentication-use"></a>Identificar el uso de autenticación heredados

Antes de autenticación heredados pueden bloquear en su directorio, debe entender primero si los usuarios tienen las aplicaciones que usan autenticación heredados y cómo afecta a su directorio global. Registros de inicio de sesión de AD Azure pueden usarse para comprender si está usando autenticación heredados.

1. Navegue hasta la **portal Azure** > **Azure Active Directory** > **inicios de sesión**.
1. Agregar la columna de la aplicación cliente si no se muestra al hacer clic en **columnas** > **aplicación cliente**.
1. Filtrar por **aplicación cliente** > **otros clientes** y haga clic en **aplicar**.

Filtrado, sólo los intentos de presentación que inicie sesión que se han realizado mediante protocolos de autenticación heredados. Al hacer clic en cada intento de inicio de sesión individual se mostrarán detalles adicionales. El **aplicación cliente** campo bajo el **información básica** ficha indicará qué protocolo de autenticación heredados se utilizó.

Estos registros se indicará qué usuarios se sigue dependiendo de autenticación heredados y qué aplicaciones están usando protocolos heredados para realizar solicitudes de autenticación. Para los usuarios que no aparecen en estos registros y se confirman que no se utiliza la autenticación heredada, implementar una directiva de acceso condicional o habilitar la **directiva de línea base: autenticación heredados de bloque** para solo estos usuarios.

## <a name="moving-away-from-legacy-authentication"></a>Alejándose de autenticación heredados

Una vez que tenga una idea más clara de quién está usando autenticación heredados en el directorio y las aplicaciones que dependen de él, el siguiente paso es actualizar a los usuarios para usar la autenticación moderna. Autenticación moderna es un método de administración de identidades que ofrece más segura autenticación y autorización. Si tiene una directiva de MFA en el directorio en su lugar, la autenticación moderna garantiza que se solicita al usuario MFA cuando sea necesario. Es la alternativa más segura a los protocolos de autenticación heredados.

En esta sección se ofrece una introducción paso a paso sobre cómo actualizar el entorno para la autenticación moderna. Lea los pasos siguientes antes de habilitar un bloqueo de directiva en su organización de autenticación heredados.

### <a name="step-1-enable-modern-authentication-in-your-directory"></a>Paso 1: Habilitar la autenticación moderna en su directorio

El primer paso para habilitar la autenticación moderna es asegurarse de que el directorio admite la autenticación moderna. Autenticación moderna está habilitada de forma predeterminada para los directorios que creó a partir del 1 de agosto de 2017. Si el directorio se creó antes de esta fecha, deberá habilitar manualmente la autenticación moderna para su directorio mediante los pasos siguientes:

1. Compruebe si el directorio ya admite la autenticación moderna mediante la ejecución de `Get-CsOAuthConfiguration` desde el [Skype para el módulo de PowerShell en línea de negocio](https://docs.microsoft.com/office365/enterprise/powershell/manage-skype-for-business-online-with-office-365-powershell).
1. Si el comando devuelve un valor vacío `OAuthServers` propiedad y, a continuación, la autenticación moderna está deshabilitado. Actualizar la configuración para habilitar el uso de la autenticación moderna `Set-CsOAuthConfiguration`. Si su `OAuthServers` propiedad contiene una entrada, que está listo para continuar.

Asegúrese de completar este paso antes de continuar. Es fundamental que las configuraciones de directorio se modifican en primer lugar ya que dictan qué protocolo se usará en todos los clientes de Office. Incluso si usa a clientes de Office que admiten la autenticación moderna, su valor predeterminado será al uso de protocolos heredados si está deshabilitada la autenticación moderna en su directorio.

### <a name="step-2-office-applications"></a>Paso 2: Aplicaciones de Office

Una vez que se ha habilitado la autenticación moderna en su directorio, puede empezar a actualizar las aplicaciones habilitando la autenticación moderna para clientes de Office. Office 2016 o posterior clientes admiten la autenticación moderna de forma predeterminada. Se requiere ningún paso adicional.

Si usa clientes de Office 2013 Windows o una versión anterior, se recomienda actualizar a Office 2016 o posterior. Incluso después de completar el paso anterior de habilitar la autenticación moderna en su directorio, las aplicaciones de Office anteriores seguirán usar protocolos de autenticación heredados. Si usa clientes de Office 2013 y no pueden actualizar inmediatamente a Office 2016 o posterior, siga los pasos descritos en el siguiente artículo para [Habilitar autenticación moderna de Office 2013 en los dispositivos de Windows](https://docs.microsoft.com/office365/admin/security-and-compliance/enable-modern-authentication). Para ayudar a proteger su cuenta mientras usa la autenticación heredada, se recomienda usar contraseñas seguras en su directorio. Desproteger [protección mediante contraseña de Azure AD](../authentication/concept-password-ban-bad.md) prohibir las contraseñas a través de su directorio.

Office 2010 no admite la autenticación moderna. Deberá actualizar todos los usuarios con Office 2010 a una versión más reciente de Office. Se recomienda actualizar a Office 2016 o posterior, ya que bloquea la autenticación heredada de forma predeterminada.

Si usa MacOS, se recomienda actualizar a Office para Mac 2016 o posterior. Si usa al cliente de correo electrónico nativa, deberá tener MacOS versión 10.14 o posterior en todos los dispositivos.

### <a name="step-3-exchange-and-sharepoint"></a>Paso 3: Exchange y SharePoint

Para que los clientes de Outlook basado en Windows usan la autenticación moderna, Exchange Online debe ser también habilitada la autenticación moderna. Si se deshabilita la autenticación moderna para Exchange Online, los clientes de Outlook basado en Windows que admiten autenticación moderna (Outlook 2013 o versiones posterior) usará la autenticación básica para conectarse a buzones de Exchange Online.

SharePoint Online está habilitado de forma predeterminada la autenticación moderna. Para los directorios creados después del 1 de agosto de 2017, la autenticación moderna está habilitada de forma predeterminada en Exchange Online. Sin embargo, si anteriormente tenía deshabilitada la autenticación moderna o está usando un directorio creado antes de esta fecha, siga los pasos descritos en el siguiente artículo para [habilitar la autenticación moderna en Exchange Online](https://docs.microsoft.com/exchange/clients-and-mobile-in-exchange-online/enable-or-disable-modern-authentication-in-exchange-online).

### <a name="step-4-skype-for-business"></a>Paso 4: Skype Empresarial

Para evitar que las solicitudes de autenticación heredados realizadas por Skype para la empresa, es necesario habilitar la autenticación moderna para Skype empresarial Online. Para los directorios creados después del 1 de agosto de 2017, la autenticación moderna para Skype para la empresa está habilitada de forma predeterminada.

Para habilitar la autenticación moderna de Skype para la empresa, se recomienda realizar la transición a Microsoft Teams, que admite la autenticación moderna de forma predeterminada. Sin embargo, si no es capaz de tr en este momento, deberá habilitar la autenticación moderna para Skype empresarial Online para que empiece de Skype para los clientes empresariales que usan autenticación moderna. Siga estos pasos en el artículo [Skype para las topologías de negocio compatibles con la autenticación moderna](https://docs.microsoft.com/skypeforbusiness/plan-your-deployment/modern-authentication/topologies-supported), para conocer los pasos habilitar la autenticación moderna para Skype para la empresa.

Además de habilitar la autenticación moderna para Skype empresarial Online, se recomienda modernas autenticación habilitarse para Exchange Online cuando se habilita la autenticación moderna para Skype para la empresa. Este proceso le ayudarán a sincronizar el estado de la autenticación moderna en Exchange Online y Skype para la empresa en línea y evitará que varias solicitudes de inicio de sesión de Skype para los clientes empresariales.

### <a name="step-5-using-mobile-devices"></a>Paso 5: Uso de dispositivos móviles

Las aplicaciones en su dispositivo móvil necesitan bloquear también autenticación heredados. Se recomienda usar Outlook para dispositivos móviles. Outlook Mobile admite la autenticación moderna de forma predeterminada y cumplirá otras directivas de protección de la línea de base MFA.

Para poder usar al cliente de correo electrónico nativas para iOS, deberá ejecutar iOS versión 11.0 o versiones posteriores para asegurarse de que el cliente de correo se ha actualizado para bloquear autenticación heredados.

### <a name="step-6-on-premises-clients"></a>Paso 6: Clientes locales

Si es un cliente híbrida con Exchange Server local y Skype empresarial local, ambos servicios deberá actualizarse para habilitar la autenticación moderna. Cuando se usa la autenticación moderna en un entorno híbrido, todavía está autenticando usuarios locales. La historia de autorizar su acceso a los cambios de recursos (archivos o mensajes de correo electrónico).

Para poder habilitar la autenticación moderna de forma local, asegúrese de cumplir theIf cumplen los requisitos, ahora está preparado para habilitar la autenticación moderna de forma local.

Pasos para habilitar la autenticación moderna se encuentra en los siguientes artículos:

* [Cómo configurar Exchange Server local para usar la autenticación moderna híbrida](https://docs.microsoft.com/office365/enterprise/configure-exchange-server-for-hybrid-modern-authentication)
* [Cómo usar la autenticación moderna (ADAL) con Skype para la empresa](https://docs.microsoft.com/skypeforbusiness/manage/authentication/use-adal)

## <a name="enable-the-baseline-policy"></a>Habilitar la directiva de línea base

La directiva **directiva de línea base: Autenticación heredados de bloque (versión preliminar)** viene previamente configurada y se mostrarán en la parte superior cuando vaya a la hoja de acceso condicional en Azure portal.

Para habilitar esta directiva y proteger su organización:

1. Inicie sesión en el **portal Azure** como administrador global, Administrador de seguridad o administrador de acceso condicional.
1. Vaya a **Azure Active Directory** > **acceso condicional**.
1. En la lista de directivas, seleccione **directiva de línea base: Autenticación heredados de bloque (versión preliminar)** .
1. Establecer **Habilitar directiva** a **usar la directiva inmediatamente**.
1. Agregar exclusiones de cualquier usuario haciendo clic en **usuarios** > **Seleccionar usuarios excluidos** y elegir los usuarios que deben excluirse. Haga clic en **seleccione** , a continuación, **realiza**.
1. Haga clic en **guardar**.

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte:

* [Directivas de protección de acceso condicional básicas](concept-baseline-protection.md)
* [Cinco pasos para asegurar su infraestructura de identidad](../../security/azure-ad-secure-steps.md)
* [¿Qué es el acceso condicional en Azure Active Directory?](overview.md)
