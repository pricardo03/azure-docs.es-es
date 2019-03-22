---
title: Restablecimiento de contraseña de autoservicio de Azure AD, Windows 7 y 8.1 - Azure Active Directory
description: Habilitación del autoservicio de restablecimiento de contraseña mediante la página de contraseña olvidada en la pantalla de inicio de sesión de Windows 7 o 8.1
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7a7752fac54f9dfb2f8fb0aecd3b6249c52c3bcf
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/21/2019
ms.locfileid: "58316361"
---
# <a name="how-to-enable-password-reset-from-windows-7-8-and-81"></a>Procedimientos para: Procedimiento para habilitar el restablecimiento de contraseña desde Windows 7, 8, and 8.1

Como administrador ha habilitado el autoservicio de restablecimiento de contraseña (SSPR), pero los usuarios continúan llamando al servicio de soporte técnico para restablecer sus contraseñas porque no pueden llegar una ventana del explorador que les permita acceder al [portal de SSPR](https://aka.ms/sspr). Para equipos con Windows 10, puede habilitar el vínculo "Restablecer contraseña" en la pantalla de inicio de sesión con el tutorial [Restablecimiento de la contraseña de Azure AD desde la pantalla de inicio de sesión](tutorial-sspr-windows.md), las instrucciones siguientes le ayudarán a habilitar a los usuarios de Windows 7, 8 y 8.1 para que restablezcan su contraseña mediante SSPR en la pantalla de inicio de sesión de Windows.

A diferencia de los equipos con Windows 10, los equipos con Windows 7, 8 y 8.1 no tienen un requisito de instancia de Azure AD unida a dominio para el restablecimiento de contraseña.

![Ejemplo de pantalla de inicio de sesión de Windows 7 con el vínculo "¿Olvidó la contraseña?" vínculo que aparece](media/howto-sspr-windows-7-8/windows-7-logon-screen.png)

## <a name="prerequisites"></a>Requisitos previos

* El autoservicio de restablecimiento de contraseña de Azure AD debe estar habilitado.
* Sistema operativo Windows 7 o Windows 8.1 revisado.
* TLS 1.2 habilitada mediante las instrucciones que se encuentran en [Configuración del registro de TLS](https://docs.microsoft.com/windows-server/security/tls/tls-registry-settings#tls-12).

> [!WARNING]
> Se debe habilitar TLS 1.2, no hay solo que establecerla en negociación automática.

## <a name="install"></a>Instalación

1. Descargue el instalador adecuado para la versión de Windows que desea habilitar.

   1. El software está disponible en el centro de descarga de Microsoft en [https://aka.ms/sspraddin](https://aka.ms/sspraddin)

1. Inicie sesión en la máquina donde desea instalar y ejecutar el instalador.
1. Después de la instalación, se recomienda encarecidamente reiniciar el equipo.
1. Después del reinicio, en la pantalla de inicio de sesión, elija un usuario y haga clic en "¿Olvidó la contraseña?" para iniciar el flujo de trabajo de restablecimiento de contraseña.
1. Complete el flujo de trabajo siguiendo los pasos que aparecen en la pantalla para restablecer su contraseña.

![Ejemplo de Windows 7 después de hacer clic en "¿Olvidó la contraseña?" flujo de restablecimiento de la contraseña de autoservicio](media/howto-sspr-windows-7-8/windows-7-sspr.png)

### <a name="silent-installation"></a>Instalación silenciosa

* Para una instalación silenciosa, use el comando "msiexec /i SsprWindowsLogon.PROD.msi /qn"
* Para una desinstalación silenciosa, use el comando "msiexec /x SsprWindowsLogon.PROD.msi /qn"

## <a name="caveats"></a>Advertencias

Debe registrarse en SSPR para poder usar el vínculo "¿Olvidó la contraseña?".

![Se necesita información de seguridad adicional para restablecer la contraseña.](media/howto-sspr-windows-7-8/windows-7-sspr-need-security-info.png)

El uso de la aplicación Microsoft Authenticator para notificaciones y códigos para restablecer la contraseña no funciona en esta versión inicial. Los usuarios deben tener métodos alternativos registrados que cumplan los requisitos de la directiva.

## <a name="troubleshooting"></a>solución de problemas

Los eventos se registrarán tanto en la máquina como en Azure AD.

Los eventos de Azure AD incluirán información sobre la dirección IP y el ClientType donde se produjo el restablecimiento de contraseña.

![Ejemplo de pantalla de inicio de sesión de Windows 7 con restablecimiento de contraseña en el registro de auditoría de Azure AD](media/howto-sspr-windows-7-8/windows-7-sspr-azure-ad-audit-log.png)

Si se requiere un registro adicional, se puede cambiar una clave del Registro en la máquina para habilitar el registro detallado. Habilite el registro detallado únicamente para solucionar problemas.

```
HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Authentication\Credential Providers\{86D2F0AC-2171-46CF-9998-4E33B3D7FD4F}
```

* Para habilitar el registro detallado, cree un REG_DWORD: "EnableLogging" y establézcalo en 1.
* Para deshabilitar el registro detallado, cambie REG_DWORD: "EnableLogging" a 0.

Si las máquinas de Windows 7, 8 y 8.1 se encuentran detrás de un servidor proxy o firewall, se debe permitir el tráfico HTTPS (443) para passwordreset.microsoftonline.com.

## <a name="next-steps"></a>Pasos siguientes

[Permitir que los usuarios de Windows 10 restablezcan su contraseña en la pantalla de inicio de sesión](tutorial-sspr-windows.md)
