---
title: Deshabilitación del registro convergente para SSPR y MFA de Azure AD (versión preliminar pública)
description: Deshabilitación del registro de restablecimiento de contraseña de autoservicio y Multi-Factor Authentication de Azure AD (versión preliminar pública)
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 08/02/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: sahenry, michmcla
ms.openlocfilehash: 3ce08f67f001a7c43602627b9eeda3ad60f867c1
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2018
ms.locfileid: "39623223"
---
# <a name="disable-azure-ad-converged-registration-public-preview"></a>Deshabilitación del registro convergente de Azure AD (versión preliminar pública)

Cuando un usuario registra su número de teléfono o aplicación móvil en la nueva experiencia convergente, nuestro servicio imprime un conjunto de indicadores (StrongAuthenticationMethods) para esos métodos en ese usuario. Esta funcionalidad permite al usuario realizar la autenticación multifactor de Azure (MFA) con estos métodos siempre que se requiera esta autenticación.

Los métodos que los usuarios registran mediante la nueva experiencia tienen la propiedad StrongAuthenticationMethods establecida. Este comportamiento también se producirá una vez que la versión preliminar pública esté disponible. Si un administrador habilita la versión preliminar, los usuarios se registran mediante la nueva experiencia y, después, el administrador deshabilita la versión preliminar, los usuarios también pueden registrarse para la autenticación multifactor sin saberlo.

Si un usuario que ha completado el registro convergente va a la página de registro de SSPR actual, en [https://aka.ms/ssprsetup](https://aka.ms/ssprsetup), se le pedirá que realice la autenticación multifactor antes de poder acceder a esa página. Este paso es un comportamiento esperado desde un punto de vista técnico, pero para los usuarios que estaban previamente registrados en SSPR solamente, este paso es un comportamiento nuevo. Aunque este paso adicional mejora la postura de seguridad del usuario al proporcionar un nivel adicional de seguridad, los administradores pueden querer revertir a sus usuarios para que ya no puedan realizar la autenticación multifactor.  

## <a name="how-to-roll-back-users"></a>Cómo revertir a los usuarios

Si, como administrador, desea restablecer la configuración de Multi-Factor Authentication para un usuario, hemos creado un script de PowerShell que borrará la propiedad StrongAuthenticationMethods para la aplicación móvil o el número de teléfono de un usuario. Ejecutar este script para los usuarios significa que tendrán que volver a registrarse en Multi-Factor Authentication si es necesario. Se recomienda probar la reversión con uno o dos usuarios antes de revertir a todos los usuarios afectados.

Los pasos siguientes le ayudarán a revertir a un usuario o grupo de usuarios:

### <a name="pre-requisites"></a>Requisitos previos

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

### <a name="rollback"></a>Reversión

En una ventana de PowerShell, ejecute el comando siguiente después de actualizar las ubicaciones resaltadas. Introduzca las credenciales de administrador global cuando se le solicite. El script generará el resultado de cada operación de actualización de usuarios.

`<script location> -path <user file location>`

## <a name="disable-preview-experience"></a>Deshabilitación de la experiencia de versión preliminar

Para deshabilitar la experiencia de versión preliminar para los usuarios, complete los pasos siguientes:

1. Inicie sesión en Azure Portal como administrador global o administrador de usuarios.
2. Vaya a **Azure Active Directory**, **Configuración de usuario**, **Administrar la configuración de las características en versión preliminar del panel de acceso**.
3. En **Los usuarios pueden utilizar las características en versión preliminar para registrar y administrar la información de seguridad**, establezca el selector en **Ninguno** y haga clic en **Guardar**.

Ya no se pedirá a los usuarios que se registren mediante la experiencia de versión preliminar.

## <a name="next-steps"></a>Pasos siguientes

[Más información sobre la versión preliminar pública del registro convergente para el restablecimiento de contraseña de autoservicio y Azure Multi-Factor Authentication](concept-registration-mfa-sspr-converged.md).