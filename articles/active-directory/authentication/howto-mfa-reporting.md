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
ms.openlocfilehash: 988e8982b6f06fb1210330c5cafdb696892794fe
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66235528"
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

   ![Informe de estado de servidor MFA server en Azure portal](./media/howto-mfa-reporting/report.png)

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

![Informe de inicios de sesión de Azure AD en Azure portal](./media/howto-mfa-reporting/sign-in-report.png)

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

## <a name="powershell-reporting-on-users-registered-for-mfa"></a>Creación de informes sobre los usuarios registrados para MFA de PowerShell

En primer lugar, asegúrese de que tiene el [módulo MSOnline de PowerShell V1](https://docs.microsoft.com/powershell/azure/active-directory/overview?view=azureadps-1.0) instalado.

Identifique los usuarios que se han registrado en MFA mediante el PowerShell siguiente.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods -ne $null} | Select-Object -Property UserPrincipalName```

Identifique los usuarios que no se han registrado en MFA mediante el PowerShell siguiente.

```Get-MsolUser -All | where {$_.StrongAuthenticationMethods.Count -eq 0} | Select-Object -Property UserPrincipalName```

## <a name="possible-results-in-activity-reports"></a>Resultados posibles en los informes de actividad

En la tabla siguiente puede utilizarse para solucionar problemas de autenticación multifactor con la versión descargada del informe de actividad de autenticación multifactor. No aparecerá directamente en el portal de Azure.

| Resultado de llamada | DESCRIPCIÓN | Amplia descripción |
| --- | --- | --- |
| SUCCESS_WITH_PIN | PIN especificado | El usuario especificó un PIN.  Si la autenticación se realizó correctamente, a continuación, escribió el PIN correcto.  Si se deniega la autenticación, a continuación, especificó un PIN incorrecto o el usuario se establece en modo estándar. |
| SUCCESS_NO_PIN | Solo # especificado | Si el usuario está configurado en modo de PIN y se deniega la autenticación, esto significa que el usuario no haya escrito el PIN y solo escribió el carácter #.  Si el usuario se establece en modo estándar y la autenticación realiza esto significa que el usuario solo escribió el carácter # que es lo correcto en el modo estándar. |
| SUCCESS_WITH_PIN_BUT_TIMEOUT | # No presionado tras la entrada | El usuario no envió los dígitos DTMF porque no se especificó #.  Los otros dígitos especificados no se envían a menos que se presione # para indicar la finalización de la entrada. |
|SUCCESS_NO_PIN_BUT_TIMEOUT | Sin entrada de teléfono - tiempo de espera | Se respondió la llamada, pero no hubo respuesta.  Esto suele indicar que la llamada ha recogido correo de voz. |
| SUCCESS_PIN_EXPIRED | PIN expirado y no cambiado | El PIN del usuario ha expirado y, se le pide que cambiarlo, pero no se ha completado correctamente el cambio del PIN. |
| SUCCESS_USED_CACHE | Caché usada | La autenticación se realizó correctamente sin una llamada de Multi-factor Authentication desde que ocurrió una autenticación correcta anterior para el mismo nombre de usuario en el período de tiempo de caché configurada. |
| SUCCESS_BYPASSED_AUTH | Autenticación omitida | Autenticación correcta mediante una omisión por única vez iniciada por el usuario.  Ver el informe de historial de usuarios omitidos para obtener más detalles sobre la omisión. |
| SUCCESS_USED_IP_BASED_CACHE | Usa la caché basada en IP | La autenticación se realizó correctamente sin una llamada de Multi-factor Authentication desde una autenticación correcta anterior para el mismo nombre de usuario, el tipo de autenticación, el nombre de la aplicación y IP se ha producido en el período de tiempo de caché configurada. |
| SUCCESS_USED_APP_BASED_CACHE | Caché usada en la aplicación | La autenticación se realizó correctamente sin una llamada de Multi-factor Authentication desde una autenticación correcta anterior para el mismo nombre de usuario, el tipo de autenticación y el nombre de la aplicación en el período de tiempo de caché configurada. |
| SUCCESS_INVALID_INPUT | Entrada de teléfono no válido | La respuesta enviada desde el teléfono no es válida.  Podría tratarse de una máquina de fax o módem o el usuario puede haber escrito * como parte de su PIN. |
| SUCCESS_USER_BLOCKED | El usuario está bloqueado | Número de teléfono del usuario está bloqueado.  Un número bloqueado puede ser iniciado por el usuario durante una llamada de autenticación o por un administrador mediante el portal de Azure. <br> NOTA:   Bloqueo de un número también es un subproducto de la alerta de fraude. |
| SUCCESS_SMS_AUTHENTICATED | Mensaje de texto autenticado | Para mensaje de prueba bidireccional, el usuario respondió correctamente con su código de acceso de un solo uso (OTP) o OTP + PIN. |
| SUCCESS_SMS_SENT | Mensaje de texto enviado | Mensaje de texto, se envió correctamente el mensaje de texto que contiene el código de acceso de un solo uso (OTP).  El usuario especificará la OTP u OTP + PIN en la aplicación para completar la autenticación. |
| SUCCESS_PHONE_APP_AUTHENTICATED | Aplicación móvil autenticada | El usuario que se autenticó correctamente mediante la aplicación móvil. |
| SUCCESS_OATH_CODE_PENDING | Código OATH pendiente | El usuario se le solicitará el código OATH, pero no respondió. |
| SUCCESS_OATH_CODE_VERIFIED | Código OATH verificado | El usuario especificó un código OATH válido cuando se le solicite. |
| SUCCESS_FALLBACK_OATH_CODE_VERIFIED | Código OATH de reserva verificado | El usuario se denegó la autenticación mediante su método principal de la autenticación multifactor y, a continuación, se proporciona un código OATH válido para la reserva. |
| SUCCESS_FALLBACK_SECURITY_QUESTIONS_ANSWERED | Respuestas a preguntas de seguridad de reserva | El usuario se denegó la autenticación mediante su método principal de la autenticación multifactor y, a continuación, responder sus preguntas de seguridad correctamente para la reserva. |
| FAILED_PHONE_BUSY | Autenticación en curso | Autenticación multifactor ya está procesando una autenticación para este usuario.  A menudo esto se debe a los clientes RADIUS que envían varias solicitudes de autenticación durante el mismo inicio de sesión. |
| CONFIG_ISSUE | Teléfono inaccesible | Se intentó realizar la llamada, pero no pudo ser coloca o no se ha respondido.  Esto incluye la señal de ocupado, señal rápida de ocupado (desconectado), tonos triples (número ya no está en servicio), tiempo de espera al timbre, etcetera. |
| FAILED_INVALID_PHONENUMBER | Formato de número de teléfono no válido | El número de teléfono tiene un formato no válido.  Números de teléfono deben ser numéricos y deben tener 10 dígitos para el código de país + 1 (Estados Unidos y Canadá). |
| FAILED_USER_HUNGUP_ON_US | El usuario colgó el teléfono | El usuario respondió al teléfono, pero colgó sin presionar ningún botón. |
| FAILED_INVALID_EXTENSION | Extensión no válida | La extensión contiene caracteres no válidos.  Solo los dígitos, comas, *, y se permiten #.  Un prefijo @ también pueden usarse. |
| FAILED_FRAUD_CODE_ENTERED | Código de fraude especificado | El usuario elige para notificar fraudes durante la llamada a resultante en denegará la autenticación y un número de teléfono bloqueado.| 
| FAILED_SERVER_ERROR | No se puede realizar la llamada | El servicio de autenticación multifactor no pudo realizar la llamada. |
| FAILED_SMS_NOT_SENT | No se pudo enviar el mensaje de texto | No se pudo enviar el mensaje de texto.  Se deniega la autenticación. |
| FAILED_SMS_OTP_INCORRECT | Mensaje de texto incorrecto de OTP | El usuario especificó un código de acceso incorrecto un solo uso (OTP) desde que reciben el mensaje de texto.  Se deniega la autenticación. |
| FAILED_SMS_OTP_PIN_INCORRECT | Texto del mensaje OTP + PIN incorrecto | El usuario especificó un código de acceso incorrecto un solo uso (OTP) o un PIN de usuario incorrecto.  Se deniega la autenticación. |
| FAILED_SMS_MAX_OTP_RETRY_REACHED | Ha superado los intentos de OTP de mensajes de texto máximo | El usuario ha superado el número máximo de intentos de contraseña de un solo uso (OTP). |
| FAILED_PHONE_APP_DENIED | Aplicación móvil denegada | El usuario denegó la autenticación en la aplicación móvil, presione el botón Denegar. |
| FAILED_PHONE_APP_INVALID_PIN | PIN de aplicación móvil no válido | El usuario especificó un PIN no válido al autenticarse en la aplicación móvil. |
| FAILED_PHONE_APP_PIN_NOT_CHANGED | No puede cambiado el PIN de aplicación móvil | El usuario no completó correctamente un cambio de PIN requerido en la aplicación móvil. |
| FAILED_FRAUD_REPORTED | Fraude notificado | El usuario notificó un fraude en la aplicación móvil. |
| FAILED_PHONE_APP_NO_RESPONSE | Aplicación móvil sin respuesta | El usuario no respondió a la solicitud de autenticación de aplicación móvil. |
| FAILED_PHONE_APP_ALL_DEVICES_BLOCKED | Aplicación todos los dispositivos móvil bloqueados | Los dispositivos de aplicación móvil para este usuario ya no están respondiendo a las notificaciones y se han bloqueado. |
| FAILED_PHONE_APP_NOTIFICATION_FAILED | Error en notificación de aplicación móvil | Se produjo un error al intentar enviar una notificación a la aplicación móvil en el dispositivo del usuario. |
| FAILED_PHONE_APP_INVALID_RESULT | Resultado de la aplicación móvil no válido | La aplicación móvil devolvió un resultado no válido. |
| FAILED_OATH_CODE_INCORRECT | Código OATH incorrecto | El usuario especificó un código OATH incorrecto.  Se deniega la autenticación. |
| FAILED_OATH_CODE_PIN_INCORRECT | Código OATH y PIN incorrectos | El usuario especificó un código OATH incorrecto o un PIN de usuario incorrecto.  Se deniega la autenticación. |
| FAILED_OATH_CODE_DUPLICATE | Código OATH duplicado | El usuario especificó un código OATH que se había utilizado previamente.  Se deniega la autenticación. |
| FAILED_OATH_CODE_OLD | Código OATH no actualizado | El usuario especificó un código OATH que precede a un código OATH que se había utilizado previamente.  Se deniega la autenticación. |
| FAILED_OATH_TOKEN_TIMEOUT | Tiempo de espera de resultado de código OATH | El usuario tardó demasiado tiempo en escribir el código OATH y el intento de autenticación multifactor ya ha agotado. |
| FAILED_SECURITY_QUESTIONS_TIMEOUT | Tiempo de espera de resultado de preguntas de seguridad | El usuario tardó demasiado tiempo a escribir la respuesta a preguntas de seguridad y el intento de autenticación multifactor ya ha agotado. |
| FAILED_AUTH_RESULT_TIMEOUT | Tiempo de espera de resultado de autenticación | El usuario tardó demasiado en completar el intento de autenticación multifactor. |
| FAILED_AUTHENTICATION_THROTTLED | Autenticación limitada | El intento de autenticación multifactor se ha limitado por el servicio. |

## <a name="next-steps"></a>Pasos siguientes

* [Para los usuarios](../user-help/multi-factor-authentication-end-user.md)
* [Lugar de implementación](concept-mfa-whichversion.md)
