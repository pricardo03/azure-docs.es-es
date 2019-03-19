---
title: Solución de problemas de registro combinado para SSPR de Azure AD y MFA (versión preliminar)
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
ms.openlocfilehash: b247a733c6874b023d3dc8114b0538e422baccfd
ms.sourcegitcommit: 7723b13601429fe8ce101395b7e47831043b970b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/21/2019
ms.locfileid: "56589436"
---
# <a name="troubleshooting-combined-security-information-registration-preview"></a>Solución de problemas de combina la información del registro de seguridad (versión preliminar)

La información proporcionada en este artículo puede guiar a los administradores que solución problemas con la experiencia de registro combinado notificada por los usuarios finales.

|     |
| --- |
| Información del registro de seguridad combinada para el restablecimiento de contraseña de autoservicio de Azure Multi-factor Authentication y Azure AD es una característica de versión preliminar pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

## <a name="audit-logs"></a>Registros de auditoría

Los eventos registrados para el registro combinado están en la categoría de "Métodos de autenticación" en Azure AD los registros de auditoría.

![Interfaz que muestra algunos de seguridad de los eventos de registro de información para un nuevo usuario en el directorio de registros de auditoría de Azure AD](media/howto-registration-mfa-sspr-combined-troubleshoot/combined-security-info-audit-log.png)

A continuación enumeran todos los eventos de auditoría generados por el registro combinado:

| Actividad | Status | Motivo | DESCRIPCIÓN |
| --- | --- | --- | --- |
| El usuario registró toda la información de seguridad necesarias | Correcto | El usuario registró toda la información de seguridad necesarios. | Este evento se produce cuando un usuario ha completado correctamente el registro.|
| El usuario registró toda la información de seguridad necesarias | Error | El usuario cancelado el registro de información de seguridad. | Este evento se produce cuando un usuario cancela el registro del modo de interrupción.|
| Información de seguridad de usuario registrado | Correcto | Usuario registrado "method". | Este evento se produce cuando un usuario registra un método individual. "Method" puede ser una aplicación de autenticación, teléfono, correo electrónico, las preguntas de seguridad, contraseña de aplicación, teléfono alternativo, etcetera.| 
| Información de seguridad de usuario revisado | Correcto | Usuario había revisado correctamente la información de seguridad. | Este evento se produce cuando un usuario hace clic en "Tiene buen aspecto" en la página de revisión de información de seguridad.|
| Información de seguridad de usuario revisado | Error | No se pudo revisar la información de seguridad de usuario. | Este evento se produce cuando un usuario hace clic en "Parece correcto" en la información de seguridad de la página Revisar, pero algo falla en el back-end.|
| Información de seguridad del usuario eliminado | Correcto | Usuario eliminado "method". | Este evento se produce cuando un usuario elimina un método individual. "Method" puede ser una aplicación de autenticación, teléfono, correo electrónico, las preguntas de seguridad, contraseña de aplicación, teléfono alternativo, etcetera.|
| Información de seguridad del usuario eliminado | Error | Usuario no se pudo eliminar "method". | Este evento se produce cuando un usuario intenta eliminar un método, pero se produce un error por algún motivo. "Method" puede ser una aplicación de autenticación, teléfono, correo electrónico, las preguntas de seguridad, contraseña de aplicación, teléfono alternativo, etcetera.|
| Información de seguridad de usuario puede cambiar de forma predeterminada | Correcto | El usuario cambió la información de seguridad predeterminada para "method". | Este evento se produce cuando un usuario cambia su método de forma predeterminada. "Method" puede ser la notificación de la aplicación de autenticador, código de mi aplicación authenticator o el token, llame al + X XXXXXXXXXX, texto un código a + X XXXXXXXXX, etcetera.|
| Información de seguridad de usuario puede cambiar de forma predeterminada | Error | Usuario no se pudo cambiar la información de seguridad predeterminada para "method". | Este evento se produce cuando un usuario intenta cambiar su método de forma predeterminada, pero se produce un error por algún motivo. Puede ser "Method" notificación de aplicación de autenticador, un código de mi aplicación authenticator o el símbolo (token), llamada + X XXXXXXXXXX, texto de un código a + X XXXXXXXXX, etcetera.|

## <a name="troubleshooting-interrupt-mode"></a>Solucionar problemas del modo de interrupción

| Síntoma | Pasos para solucionar problemas |
| --- | --- |
| No veo los métodos que esperaba ver. | 1. Compruebe si el usuario tiene un rol de administrador de Azure AD. Si es así, revise las diferencias de directiva del administrador SSPR. <br> 2. Determinar si el usuario se interrumpe debido a la aplicación de registro MFA o de cumplimiento de registro SSPR. Revise el diagrama de flujo en los modos de registro combinado para determinar qué métodos se deben mostrar. <br> 3. Determinar cómo se cambió la directiva de MFA o SSPR. Si el cambio se recientes, puede tardar algún tiempo para la directiva actualizada se propaguen.|

## <a name="troubleshooting-manage-mode"></a>Administrar el modo de solución de problemas

| Síntoma | Pasos para solucionar problemas |
| --- | --- |
| No tengo la opción para agregar un método determinado. | 1. Determinar si el método está habilitado para MFA o SSPR. <br> 2. Si el método está habilitado, vuelva a guardar las directivas y espere entre 1 y 2 horas antes de volver a probar. <br> 3. Si el método está habilitado, asegúrese de que el usuario ya no se ha configurado el número máximo de ese método que se pueden configurar.|

## <a name="disable-combined-registration"></a>Deshabilitar registro combinado

Cuando un usuario registra su número de teléfono o aplicación móvil en el nuevo combina la experiencia, las marcas de nuestro servicio un conjunto de marcadores (StrongAuthenticationMethods) para esos métodos en los que el usuario. Esta funcionalidad permite al usuario realizar la autenticación multifactor de Azure (MFA) con estos métodos siempre que se requiera esta autenticación.

Los métodos que los usuarios registran mediante la nueva experiencia tienen la propiedad StrongAuthenticationMethods establecida. Si un administrador habilita la versión preliminar, los usuarios se registran mediante la nueva experiencia y, después, el administrador deshabilita la versión preliminar, los usuarios también pueden registrarse para la autenticación multifactor sin saberlo.

Si un usuario que ha completado combina el registro se desplaza a la página de registro de restablecimiento (SSPR) de contraseña de autoservicio actual, en [ https://aka.ms/ssprsetup ](https://aka.ms/ssprsetup), se le pedirá que realice MFA para poder tener acceso a esa página. Este paso es un comportamiento esperado desde un punto de vista técnico, pero para los usuarios que estaban previamente registrados en SSPR solamente, este paso es un comportamiento nuevo. Aunque este paso adicional mejora la postura de seguridad del usuario al proporcionar un nivel adicional de seguridad, los administradores pueden querer revertir a sus usuarios para que ya no puedan realizar la autenticación multifactor.  

### <a name="how-to-roll-back-users"></a>Cómo revertir a los usuarios

Si, como administrador, desea restablecer la configuración de Multi-Factor Authentication para un usuario, hemos creado un script de PowerShell que borrará la propiedad StrongAuthenticationMethods para la aplicación móvil o el número de teléfono de un usuario. Ejecutar este script para los usuarios significa que tendrán que volver a registrarse en Multi-Factor Authentication si es necesario. Se recomienda probar la reversión con uno o dos usuarios antes de revertir a todos los usuarios afectados.

Los pasos siguientes le ayudarán a revertir a un usuario o grupo de usuarios:

#### <a name="prerequisites"></a>Requisitos previos

1. Deberá instalar los módulos de Azure AD PowerShell adecuados. En una ventana de PowerShell, ejecute estos comandos para instalar los módulos:

   ```powershell
   Install-Module -Name MSOnline
   Import-Module MSOnline
   ```

1. Guarde la lista de los identificadores de objetos de usuario afectados en la máquina como un archivo de texto con un identificador por línea. Tome nota de la ubicación del archivo.
1. Guarde el siguiente script en la máquina y tome nota de su ubicación:

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

En una ventana de PowerShell, ejecute el comando siguiente después de actualizar las ubicaciones resaltadas. Introduzca las credenciales de administrador global cuando se le solicite. El script generará el resultado de cada operación de actualización de usuarios.

`<script location> -path <user file location>`

### <a name="disable-preview-experience"></a>Deshabilitación de la experiencia de versión preliminar

Para deshabilitar la experiencia de versión preliminar para los usuarios, complete los pasos siguientes:

1. Inicie sesión en Azure Portal como administrador global o administrador de usuarios.
2. Vaya a **Azure Active Directory**, **Configuración de usuario**, **Administrar la configuración de las características en versión preliminar del panel de acceso**.
3. En **Los usuarios pueden utilizar las características en versión preliminar para registrar y administrar la información de seguridad**, establezca el selector en **Ninguno** y haga clic en **Guardar**.

Ya no se pedirá a los usuarios que se registren mediante la experiencia de versión preliminar.

## <a name="next-steps"></a>Pasos siguientes

[Más información sobre la versión preliminar pública de registro combinado para el restablecimiento de contraseña de autoservicio y Azure Multi-factor Authentication](concept-registration-mfa-sspr-combined.md)
