---
title: 'Solución de problemas de registro combinado para SSPR de Azure AD y Multi-factor Authentication (versión preliminar): Azure Active Directory'
description: Solución de problemas de autenticación multifactor de Azure AD y el registro de combinada de restablecimiento de contraseña de autoservicio (versión preliminar)
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 02/20/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 40918493071fe0dd694c43e2b087a2bf7eb197d8
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2019
ms.locfileid: "59489200"
---
# <a name="troubleshooting-combined-security-information-registration-preview"></a>Solución de problemas de combina la información del registro de seguridad (versión preliminar)

La información de este artículo está pensada para guiar a los administradores que solución los problemas notificados por los usuarios de la experiencia de registro combinado.

|     |
| --- |
| Información del registro de seguridad combinada para Azure Multi-factor Authentication y restablecimiento de contraseña de autoservicio de Azure Active Directory (Azure AD) es una característica de versión preliminar pública de Azure AD. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

## <a name="audit-logs"></a>Registros de auditoría

Los eventos registrados para el registro combinado están en la categoría de métodos de autenticación en Azure AD los registros de auditoría.

![Eventos de registro que muestra la interfaz de registros de auditoría de Azure AD](media/howto-registration-mfa-sspr-combined-troubleshoot/combined-security-info-audit-log.png)

En la tabla siguiente se enumera todos los eventos de auditoría generados por el registro combinado:

| Actividad | Status | Motivo | DESCRIPCIÓN |
| --- | --- | --- | --- |
| El usuario registró toda la información de seguridad necesarias | Correcto | El usuario registró toda la información de seguridad necesarios. | Este evento se produce cuando un usuario ha completado correctamente el registro.|
| El usuario registró toda la información de seguridad necesarias | Error | El usuario cancelado el registro de información de seguridad. | Este evento se produce cuando un usuario cancela el registro del modo de interrupción.|
| Información de seguridad de usuario registrado | Correcto | Usuario registrado *método*. | Este evento se produce cuando un usuario registra un método individual. *Método* puede ser autenticador app, teléfono, correo electrónico, seguridad preguntas, aplicación contraseña, teléfono alternativo y así sucesivamente.| 
| Información de seguridad de usuario revisado | Correcto | Usuario había revisado correctamente la información de seguridad. | Este evento se produce cuando un usuario selecciona **tiene buen aspecto** en la página de revisión de información de seguridad.|
| Información de seguridad de usuario revisado | Error | No se pudo revisar la información de seguridad de usuario. | Este evento se produce cuando un usuario selecciona **tiene buen aspecto** en la información de seguridad de la página Revisar, pero algo no va bien en el back-end.|
| Información de seguridad del usuario eliminado | Correcto | Usuario eliminado *método*. | Este evento se produce cuando un usuario elimina un método individual. *Método* puede ser autenticador app, teléfono, correo electrónico, seguridad preguntas, aplicación contraseña, teléfono alternativo y así sucesivamente.|
| Información de seguridad del usuario eliminado | Error | No se pudo eliminar el usuario *método*. | Este evento se produce cuando un usuario intenta eliminar un método pero el intento falla por alguna razón. *Método* puede ser autenticador app, teléfono, correo electrónico, seguridad preguntas, aplicación contraseña, teléfono alternativo y así sucesivamente.|
| Información de seguridad de usuario puede cambiar de forma predeterminada | Correcto | El usuario cambió la información de seguridad predeterminada para *método*. | Este evento se produce cuando un usuario cambia el método predeterminado. *Método* puede ser un código de mi aplicación authenticator o el token, llame al + X XXXXXXXXXX, texto de notificación de aplicación Authenticator un código a + X XXXXXXXXX y así sucesivamente.|
| Información de seguridad de usuario puede cambiar de forma predeterminada | Error | No se pudo cambiar la información de seguridad predeterminado de usuario *método*. | Este evento se produce cuando un usuario intenta cambiar el método predeterminado, pero el intento falla por alguna razón. *Método* puede ser un código de mi aplicación authenticator o el token, llame al + X XXXXXXXXXX, texto de notificación de aplicación Authenticator un código a + X XXXXXXXXX y así sucesivamente.|

## <a name="troubleshooting-interrupt-mode"></a>Solucionar problemas del modo de interrupción

| Síntoma | Pasos para solucionar problemas |
| --- | --- |
| No veo los métodos que esperaba ver. | 1. Compruebe si el usuario tiene un rol de administrador de Azure AD. Si es así, ver las diferencias de directiva de administración SSPR. <br> 2. Determinar si el usuario se interrumpe debido a la aplicación de registro de autenticación multifactor o aplicación de registro SSPR. Consulte la [flowchart](../../active-directory/authentication/concept-registration-mfa-sspr-combined.md#combined-registration-modes) en "Modos de registro Combined" para determinar qué métodos se deben mostrar. <br> 3. Determinar cómo se cambió la directiva de autenticación multifactor o SSPR. Si el cambio se recientes, puede tardar algún tiempo para la directiva actualizada se propaguen.|

## <a name="troubleshooting-manage-mode"></a>Administrar el modo de solución de problemas

| Síntoma | Pasos para solucionar problemas |
| --- | --- |
| No tengo la opción para agregar un método determinado. | 1. Determinar si el método está habilitado para la autenticación multifactor o para SSPR. <br> 2. Si el método está habilitado, vuelva a guardar las directivas y espere entre 1 y 2 horas antes de volver a probar. <br> 3. Si el método está habilitado, asegúrese de que el usuario ya no se ha configurado el número máximo de ese método que se pueden configurar.|

## <a name="disable-combined-registration"></a>Deshabilitar registro combinado

Cuando un usuario registra un número de teléfono o aplicación móvil en el nuevo combina la experiencia, las marcas de nuestro servicio un conjunto de marcadores (StrongAuthenticationMethods) para esos métodos en los que el usuario. Esta funcionalidad permite al usuario llevar a cabo la autenticación multifactor con esos métodos cada vez que se requiere la autenticación multifactor.

Si un administrador habilita la versión preliminar, los usuarios se registran a través de la nueva experiencia y, a continuación, el administrador deshabilita la versión preliminar, los usuarios podrían sin saberlo registrarse para la autenticación multifactor también.

Si un usuario que ha completado el registro combinado llega a la página de registro de restablecimiento (SSPR) de contraseña de autoservicio actual en [ https://aka.ms/ssprsetup ](https://aka.ms/ssprsetup), se pedirá al usuario a realizar la autenticación multifactor para poder tener acceso esa página. Se espera que este paso desde un punto de vista técnico, pero es nuevo para los usuarios que anteriormente se han registrado en SSPR solo. Aunque este paso adicional mejorar la postura de seguridad del usuario al proporcionar otro nivel de seguridad, los administradores que desee revertir a los usuarios para que ya no podrá realizar la autenticación multifactor.  

### <a name="how-to-roll-back-users"></a>Cómo revertir a los usuarios

Si, como un administrador desea restablecer la configuración de autenticación multifactor de un usuario, puede usar el script de PowerShell proporcionado en la sección siguiente. El script borrará la propiedad StrongAuthenticationMethods para la aplicación móvil de un usuario o el número de teléfono. Si ejecuta este script para los usuarios, deberá volver a registrar para la autenticación multifactor cuando la necesiten. Se recomienda reversión pruebas con uno o dos usuarios antes de revertir todos los usuarios afectados.

Los pasos siguientes le ayudará a revertir a un usuario o grupo de usuarios.

#### <a name="prerequisites"></a>Requisitos previos

1. Instale los módulos de PowerShell de Azure AD adecuados. En una ventana de PowerShell, ejecute estos comandos para instalar los módulos:

   ```powershell
   Install-Module -Name MSOnline
   Import-Module MSOnline
   ```

1. Guarde la lista de identificadores de objeto de usuario afectado en el equipo como un archivo de texto con un identificador por línea. Tome nota de la ubicación del archivo.
1. Guarde el siguiente script en el equipo y tome nota de la ubicación de la secuencia de comandos:

   ```powershell
   <# 
   //********************************************************
   //*                                                      *
   //*   Copyright (C) Microsoft. All rights reserved.      *
   //*                                                      *
   //********************************************************
   #>

   param($path)

   # Define Remediation Fn
   function RemediateUser {

       param  
       (
           $ObjectId
       )

       $user = Get-MsolUser -ObjectId $ObjectId

       Write-Host "Checking if user is eligible for rollback: UPN: "  $user.UserPrincipalName  " ObjectId: "  $user.ObjectId -ForegroundColor Yellow

       $hasMfaRelyingParty = $false
       foreach($p in $user.StrongAuthenticationRequirements)
       {
           if ($p.RelyingParty -eq "*")
           {
               $hasMfaRelyingParty = $true
               Write-Host "User was enabled for per-user MFA." -ForegroundColor Yellow
           }
       }

       if ($user.StrongAuthenticationMethods.Count -gt 0 -and -not $hasMfaRelyingParty)
       {
           Write-Host $user.UserPrincipalName " is eligible for rollback" -ForegroundColor Yellow
           Write-Host "Rolling back user ..." -ForegroundColor Yellow
           Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName $user.UserPrincipalName
           Write-Host "Successfully rolled back user " $user.UserPrincipalName -ForegroundColor Green
       }
       else
       {
           Write-Host $user.UserPrincipalName " is not eligible for rollback. No action required."
       }

       Write-Host ""
       Start-Sleep -Milliseconds 750
   }

   # Connect
   Import-Module MSOnline
   Connect-MsolService

   foreach($line in Get-Content $path)
   {
       RemediateUser -ObjectId $line
   }
   ```

#### <a name="rollback"></a>Reversión

En una ventana de PowerShell, ejecute el siguiente comando, que proporciona las ubicaciones de archivo de script y el usuario. Introduzca las credenciales de administrador global cuando se le solicite. El script generará el resultado de cada operación de actualización de usuarios.

`<script location> -path <user file location>`

### <a name="disable-the-preview-experience"></a>Deshabilitar la experiencia de versión preliminar

Para deshabilitar la experiencia de versión preliminar para los usuarios, siga estos pasos:

1. Inicie sesión como un usuario administrador en el portal de Azure.
2. Vaya a **Azure Active Directory** > **configuración de usuario** > **administrar la configuración de características de vista previa del panel de acceso**.
3. En **los usuarios pueden usar características de vista previa para registrar y administrar información de seguridad**, Establece el selector en **ninguno**y, a continuación, seleccione **guardar**.

Ya no se solicitará a los usuarios registrar mediante la experiencia de versión preliminar.

## <a name="next-steps"></a>Pasos siguientes

* [Más información sobre la versión preliminar pública de registro combinado para el restablecimiento de contraseña de autoservicio y Azure Multi-factor Authentication](concept-registration-mfa-sspr-combined.md)
