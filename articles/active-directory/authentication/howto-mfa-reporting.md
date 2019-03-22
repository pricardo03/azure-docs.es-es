---
title: 'Informes de acceso y uso para Azure MFA: Azure Active Directory'
description: Aquí se describe cómo utilizar la característica de informes de Azure Multi-Factor Authentication.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 07/30/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7dd26bbb06310439aae065349c14c6df1c1c12e6
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58316939"
---
# <a name="reports-in-azure-multi-factor-authentication"></a>Informes en Azure Multi-Factor Authentication

Azure Multi-Factor Authentication proporciona varios tipos de informes que usted o su organización pueden usar a través de Azure Portal. En la tabla siguiente se muestran los informes disponibles:

| Informe | Ubicación | DESCRIPCIÓN |
|:--- |:--- |:--- |
| Historial de usuarios bloqueados | Azure AD > Servidor MFA > Bloquear o desbloquear usuarios | Muestra el historial de solicitudes para bloquear o desbloquear usuarios. |
| Alertas de fraude y de uso | Azure AD > Inicios de sesión | Proporciona información sobre el uso general, el resumen del usuario, detalles del usuario; así como un historial de alertas de fraude enviadas durante el intervalo de fechas especificado. |
| Uso de componentes locales | Azure AD > Servidor MFA > Informe de actividad | Proporciona información sobre el uso general de MFA a través de la extensión NPS, AD FS y el servidor MFA. |
| Historial de usuarios omitidos | Azure AD > Servidor MFA > Omisión por única vez | Proporciona un historial de solicitudes para omitir Multi-Factor Authentication para un usuario. |
| Estado del servidor | Azure AD > Servidor MFA > Estado del servidor | Muestra el estado de los servidores de Multi-Factor Authentication asociados a su cuenta. |

## <a name="view-mfa-reports"></a>Visualización de informes de MFA

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. A la izquierda, seleccione **Azure Active Directory** > **MFA Server** (Servidor MFA).
3. Seleccione el informe que desee ver.

   <center>
   
   ![Nube](./media/howto-mfa-reporting/report.png)</center>

## <a name="azure-ad-sign-ins-report"></a>Informe de inicios de sesión de Azure AD

Con el **informe de actividad de inicios de sesión** de [Azure Portal](https://portal.azure.com), se puede obtener toda la información necesaria para determinar cómo marcha el entorno.

El informe de inicios de sesión puede proporcionarle información acerca del uso de las aplicaciones administradas y actividades de inicio de sesión de usuario, lo que incluye información acerca del uso de la autenticación multifactor (MFA). Los datos MFA le proporcionan información acerca del funcionamiento de MFA en su organización, lo que le permite responder preguntas como estas:

- ¿Supuso un problema la MFA para el inicio de sesión?
- ¿Cómo completo el usuario la MFA?
- ¿Por qué no pudo completar el usuario la MFA?
- ¿En cuántos usuarios se usa MFA?
- ¿Cuántos usuarios no pueden completar el desafío de MFA?
- ¿Cuáles son los problemas de MFA más habituales a los que se enfrentan los usuarios finales?

Estos datos están disponibles a través de [Azure Portal](https://portal.azure.com) y la [API de informes](../reports-monitoring/concept-reporting-api.md).

![Nube](./media/howto-mfa-reporting/sign-in-report.png)

### <a name="sign-ins-report-structure"></a>Estructura del informe de inicios de sesión

Los informes de actividades de inicio de sesión referentes a la MFA le proporcionan acceso a la siguiente información:

**MFA necesaria:** si MFA es necesaria para el inicio de sesión, o no. MFA puede ser necesaria debido a la MFA por usuario, al acceso condicional o a otras razones. Los valores posibles son **Yes** o **No**.

**Resultado de MFA:** más información acerca de si la MFA se ha cumplido o denegado:

- Si se ha cumplido, esta columna proporciona más información acerca de cómo se ha hecho.
   - Azure Multi-Factor Authentication
      - completado en la nube
      - ha expirado debido a las directivas configuradas en el inquilino
      - registro solicitado
      - satisfecho por notificación en el token
      - satisfecho por notificación de proveedor externo
      - satisfecho por autenticación segura
      - se omite, ya que el flujo usado fue el flujo de inicio de sesión del agente de Windows
      - se omite debido a la aplicación de la contraseña
      - se omite debido a la ubicación
      - se omite debido al dispositivo registrado
      - se omite debido al dispositivo recordado
      - completado correctamente
   - Redireccionado a un para la autenticación multifactor

- Si se ha denegado, esta columna proporcionaría el motivo de la denegación.
   - Azure Multi-Factor Authentication denegado;
      - autenticación en curso
      - intento de duplicación de autenticación
      - se ha escribir un código incorrecto demasiadas veces
      - autenticación no válida
      - código de verificación de aplicación móvil no válido
      - error de configuración
      - la llamada de teléfono se dirigió al correo de voz
      - el número de teléfono tiene un formato no válido
      - error del servicio
      - no se puede acceder el teléfono del usuario
      - no se puede enviar la notificación de la aplicación móvil al dispositivo
      - no se puede enviar la notificación de la aplicación móvil
      - el usuario rechazó la autenticación
      - el usuario no respondió a la notificación de la aplicación móvil
      - el usuario no tiene ningún método de comprobación registrado
      - el usuario ha escrito un código incorrecto
      - el usuario ha escrito un PIN incorrecto
      - el usuario contestó la llamada de teléfono sin la autenticación
      - el usuario está bloqueado
      - el usuario no ha escrito el código de verificación
      - el usuario no se encuentra
      - el código de verificación ya se ha usado una vez

**Método de autenticación de MFA:** el método de autenticación que el usuario ha utilizado para completar la MFA. Los valores posibles son:

- mensaje de texto
- Notificación en aplicación móvil
- Llamada de teléfono (teléfono de autenticación)
- Código de verificación de la aplicación móvil
- Llamada de teléfono (teléfono profesional)
- Llamada de teléfono (teléfono de autenticación alternativo)

**Detalles de la autenticación de MFA:** versión limpia del número de teléfono, por ejemplo: + X XXXXXXXX64.

**Acceso condicional:** busque información sobre las directivas de acceso condicional que afectan al intento de inicio de sesión, lo que incluye:

- Nombre de la directiva
- Controles de concesión
- Controles de sesión
- Resultado

## <a name="powershell-reporting"></a>Informes de PowerShell

Identifique los usuarios que se han registrado en MFA mediante el PowerShell siguiente.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Identifique los usuarios que no se han registrado en MFA mediante el PowerShell siguiente.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

## <a name="next-steps"></a>Pasos siguientes

* [Para los usuarios](../user-help/multi-factor-authentication-end-user.md)
* [Lugar de implementación](concept-mfa-whichversion.md)
