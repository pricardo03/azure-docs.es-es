---
title: 'Estados de usuario de Azure Multi-Factor Authentication: Azure Active Directory'
description: Obtenga información sobre los estados de usuario en Azure Multi-Factor Authentication.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 01/11/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: bc83f59d51e1552d9d68089d0b9fa23d924f491c
ms.sourcegitcommit: 39d95a11d5937364ca0b01d8ba099752c4128827
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2019
ms.locfileid: "69561295"
---
# <a name="how-to-require-two-step-verification-for-a-user"></a>Exigencia de verificación en dos pasos para un usuario

Puede utilizar uno de estos dos métodos para requerir la verificación en dos pasos, los cuales requieren el uso de una cuenta de administrador global. La primera opción consiste en habilitar a cada usuario para Azure Multi-factor Authentication (MFA). Cuando los usuarios se habilitan de forma individual, realizan la verificación en dos pasos cada vez que inician sesión (con algunas excepciones, como cuando inician sesión desde direcciones IP de confianza o si se activa la característica _recordar dispositivos_). La segunda opción consiste en configurar una directiva de acceso condicional que requiere la verificación en dos pasos en determinadas condiciones.

> [!TIP]
> La habilitación de Azure Multi-Factor Authentication mediante directivas de acceso condicional es el enfoque recomendado. El cambio de estado del usuario ya no se recomienda, a menos que las licencias no incluyan el acceso condicional, ya que esto requerirá que los usuarios realicen MFA cada vez que inicien sesión.

## <a name="choose-how-to-enable"></a>Elección del modo de habilitación

**Enabled by changing user state** (Habilitada mediante el cambio de estado de usuario): se trata del método tradicional para exigir la verificación en dos pasos y se describe en este artículo. Funciona tanto en Azure MFA en la nube como en el servidor de Azure MFA. El uso de este método requiere que los usuarios realicen la verificación en dos pasos **cada vez** que inicien sesión e invalida las directivas de acceso condicional.

Enabled by conditional access policy (Habilitada mediante la directiva de acceso condicional): se trata del medio más flexible para habilitar la verificación en dos pasos para los usuarios. La habilitación de la directiva de acceso condicional solo funciona con Azure MFA en la nube y es una característica premium de Azure AD. Puede encontrar más información sobre este método en [Implementación de Azure Multi-factor Authentication en la nube](howto-mfa-getstarted.md).

Enabled by Azure AD Identity Protection (Habilitada por Azure AD Identity Protection): este método utiliza la directiva de riesgos de Azure AD Identity Protection para exigir la verificación en dos pasos solo según el riesgo de inicio de sesión para todas las aplicaciones en la nube. Este método requiere una licencia de Azure Active Directory P2. Para más información sobre este método, vea [Azure Active Directory Identity Protection](../identity-protection/howto-sign-in-risk-policy.md).

> [!Note]
> Puede encontrar más información sobre licencias y precios en las páginas de precios de [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/
) y [Multi-factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

## <a name="enable-azure-mfa-by-changing-user-state"></a>Habilitar Azure MFA mediante el cambio de estado del usuario

Las cuentas de usuario de Azure Multi-Factor Authentication tienen los siguientes tres estados:

| Status | DESCRIPCIÓN | Aplicaciones que no son de explorador afectadas | Aplicaciones que son de explorador afectadas | Autenticación moderna afectada |
|:---:|:---:|:---:|:--:|:--:|
| Disabled |Estado predeterminado para un usuario nuevo no inscrito en Azure MFA. |Sin |No |Sin |
| habilitado |El usuario se ha inscrito en Azure MFA, pero no se ha registrado. La próxima vez que inicie sesión, se le pedirá registrarse. |No.  Continúa funcionando hasta que se complete el proceso de registro. | Sí. Una vez que expire la sesión, se requerirá el registro de Azure MFA.| Sí. Una vez que expire el token de acceso, se requerirá el registro de Azure MFA. |
| Aplicado |El usuario se ha inscrito y ha completado el proceso de registro de Azure MFA. |Sí. Las aplicaciones requieren contraseñas de aplicación. |Sí. Se requiere Azure MFA en el inicio de sesión. | Sí. Se requiere Azure MFA en el inicio de sesión. |

El estado de un usuario refleja si un administrador lo ha inscrito en Azure MFA, y si ha completado el proceso de registro.

Todos los usuarios comienzan con el estado *Deshabilitado*. Cuando inscribe usuarios en Azure MFA, cambia a *Habilitado*. Cuando los usuarios habilitados inician sesión y completan el proceso de registro, el estado cambia a *Aplicado*.  

### <a name="view-the-status-for-a-user"></a>Ver el estado de un usuario

Para acceder a la página donde puede ver y administrar los estados de usuario, siga estos pasos:

1. Inicie sesión en [Azure Portal](https://portal.azure.com) como administrador.
2. Vaya a **Azure Active Directory** > **Usuarios y grupos** > **Todos los usuarios**.
3. Seleccione **Multi-Factor Authentication**.
   Seleccione ![Multi-Factor Authentication](./media/howto-mfa-userstates/selectmfa.png)
4. Se abre una nueva página que muestra los estados de usuario.
   ![Estado de usuario de Multi-Factor Authentication (captura de pantalla)](./media/howto-mfa-userstates/userstate1.png)

### <a name="change-the-status-for-a-user"></a>Cambiar el estado de un usuario

1. Use los pasos anteriores para acceder a la página de **usuarios** de Azure Multi-Factor Authentication.
2. Busque el usuario que desea habilitar para Azure MFA. Puede que necesite cambiar la vista en la parte superior.
   ![Seleccione el usuario para cambiar su estado desde la pestaña de usuarios](./media/howto-mfa-userstates/enable1.png)
3. Active la casilla situada junto a su nombre.
4. En el lado derecho, bajo los **pasos rápidos**, elija **Habilitar** o **Deshabilitar**.
   ![Habilite el usuario seleccionado haciendo clic en Habilitar en el menú de pasos rápidos](./media/howto-mfa-userstates/user1.png)

   > [!TIP]
   > *Habilitado* significa que los usuarios cambian automáticamente a *Aplicado* cuando se registren en Azure MFA. No cambie manualmente el estado de usuario a *Aplicado*.

5. Confirme la selección en la ventana emergente que se abre.

Después de habilitar los usuarios, notifíquelos por correo electrónico. Debe indicarles que se les pedirá que se registren la próxima vez que inicien sesión. Además, si su organización utiliza aplicaciones sin explorador que no son compatibles con la autenticación moderna, deben crear contraseñas de aplicación. También puede incluir un vínculo a la [Guía del usuario final de Azure MFA](../user-help/multi-factor-authentication-end-user.md), que les ayudará a empezar a trabajar.

### <a name="use-powershell"></a>Uso de PowerShell

Para cambiar el estado del usuario mediante [Azure AD PowerShell](/powershell/azure/overview), debe cambiar `$st.State`. Hay tres estados posibles:

* habilitado
* Aplicado
* Disabled  

No mueva a los usuarios directamente a estado *Aplicado*. Si lo hace, las aplicaciones que no son de explorador dejan de funcionar porque el usuario no ha pasado a través del proceso de registro de Azure MFA y obtenido una [contraseña de aplicación](howto-mfa-mfasettings.md#app-passwords).

Instale el módulo en primer lugar, mediante:

   ```PowerShell
   Install-Module MSOnline
   ```

> [!TIP]
> No se olvide de conectarse primero con **Connect-MsolService**

Este ejemplo de script de PowerShell habilita MFA para un usuario individual:

   ```PowerShell
   Import-Module MSOnline
   $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
   $st.RelyingParty = "*"
   $st.State = "Enabled"
   $sta = @($st)
   Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta
   ```

El uso de PowerShell es una buena opción cuando necesite habilitar usuarios de forma masiva. Por ejemplo, el script siguiente recorre en bucle una lista de usuarios y habilita MFA en sus cuentas:

   ```PowerShell
   $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
   foreach ($user in $users)
   {
       $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
       $st.RelyingParty = "*"
       $st.State = "Enabled"
       $sta = @($st)
       Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
   }
   ```

Para deshabilitar MFA, use este script:

   ```PowerShell
   Get-MsolUser -UserPrincipalName user@domain.com | Set-MsolUser -StrongAuthenticationMethods @()
   ```

que también se puede abreviar como:

   ```PowerShell
   Set-MsolUser -UserPrincipalName user@domain.com -StrongAuthenticationRequirements @()
   ```

### <a name="convert-users-from-per-user-mfa-to-conditional-access-based-mfa"></a>Convertir a los usuarios de MFA por usuario a MFA basado en el acceso condicional

El siguiente comando de PowerShell puede ayudarle a realizar la conversión a Azure Multi-Factor Authentication basado en el acceso condicional.

```PowerShell
# Sets the MFA requirement state
function Set-MfaState {

    [CmdletBinding()]
    param(
        [Parameter(ValueFromPipelineByPropertyName=$True)]
        $ObjectId,
        [Parameter(ValueFromPipelineByPropertyName=$True)]
        $UserPrincipalName,
        [ValidateSet("Disabled","Enabled","Enforced")]
        $State
    )

    Process {
        Write-Verbose ("Setting MFA state for user '{0}' to '{1}'." -f $ObjectId, $State)
        $Requirements = @()
        if ($State -ne "Disabled") {
            $Requirement =
                [Microsoft.Online.Administration.StrongAuthenticationRequirement]::new()
            $Requirement.RelyingParty = "*"
            $Requirement.State = $State
            $Requirements += $Requirement
        }

        Set-MsolUser -ObjectId $ObjectId -UserPrincipalName $UserPrincipalName `
                     -StrongAuthenticationRequirements $Requirements
    }
}

# Wrapper to disable MFA with the option to keep the MFA methods (to avoid having to proof-up again later)
function Disable-Mfa {

    [CmdletBinding()]
    param(
        [Parameter(ValueFromPipeline=$True)]
        $User,
        [switch] $KeepMethods
    )

    Process {

        Write-Verbose ("Disabling MFA for user '{0}'" -f $User.UserPrincipalName)
        $User | Set-MfaState -State Disabled

        if ($KeepMethods) {
            # Restore the MFA methods which got cleared when disabling MFA
            Set-MsolUser -ObjectId $User.ObjectId `
                         -StrongAuthenticationMethods $User.StrongAuthenticationMethods
        }
    }
}

# Disable MFA for all users, keeping their MFA methods intact
Get-MsolUser -All | Disable-MFA -KeepMethods
```

## <a name="next-steps"></a>Pasos siguientes

* ¿Por qué se solicitó o no a un usuario que realizará MFA? Consulte la sección [Informe de inicios de sesión de Azure AD en los informes del documento Azure Multi-factor Authentication](howto-mfa-reporting.md#azure-ad-sign-ins-report).
* Para configurar opciones adicionales tales como direcciones IP de confianza, mensajes de voz personalizados y alertas de fraude, vea el artículo [Configuración de Azure Multi-Factor Authentication](howto-mfa-mfasettings.md)
* Puede encontrar información sobre cómo administrar la configuración de usuario de Azure Multi-factor Authentication en el artículo [Manage user settings with Azure Multi-Factor Authentication in the cloud](howto-mfa-userdevicesettings.md) (Administración de la configuración de usuario con Azure Multi-Factor Authentication en la nube).
