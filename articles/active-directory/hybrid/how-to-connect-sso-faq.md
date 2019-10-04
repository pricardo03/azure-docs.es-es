---
title: 'Azure AD Connect: preguntas más frecuentes sobre el inicio de sesión único de conexión directa | Microsoft Docs'
description: Respuestas a las preguntas más frecuentes sobre el inicio de sesión único de conexión directa de Azure Active Directory.
services: active-directory
keywords: qué es Azure AD Connect, instalar Active Directory, componentes necesarios para Azure AD, SSO, inicio de sesión único
documentationcenter: ''
author: billmath
manager: daveba
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/14/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 96d0253e1e656f4bdb5180af0d57824aceb4f0dd
ms.sourcegitcommit: 83df2aed7cafb493b36d93b1699d24f36c1daa45
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2019
ms.locfileid: "71176660"
---
# <a name="azure-active-directory-seamless-single-sign-on-frequently-asked-questions"></a>Inicio de sesión único de conexión directa de Azure Active Directory: Preguntas más frecuentes

En este artículo se ofrece respuesta a las preguntas más frecuentes sobre el inicio de sesión único de conexión directa (SSO de conexión directa) de Azure Active Directory. Siga comprobando si hay contenido nuevo.

**P: ¿Con qué métodos de inicio de sesión funciona el inicio de sesión único de conexión directa?**

SSO de conexión directa se puede combinar con los métodos de inicio de sesión mediante [sincronización de hash de contraseñas](how-to-connect-password-hash-synchronization.md) o [autenticación de paso a través](how-to-connect-pta.md). Pero esta característica no se puede usar con los Servicios de federación de Active Directory (AD FS).

**P: ¿El inicio de sesión único de conexión directa es una característica gratuita?**

SSO de conexión directa es una característica gratuita y no es necesario usar ninguna versión de pago de Azure AD para usarla.

**P: ¿Está disponible el inicio de sesión único de conexión directa en la [nube de Microsoft Azure Alemania](https://www.microsoft.de/cloud-deutschland) y en la [nube de Microsoft Azure Government?](https://azure.microsoft.com/features/gov/)**

No. El inicio de sesión único de conexión directa solo está disponible en la instancia mundial de Azure AD.

**P: ¿Qué aplicaciones aprovechan la funcionalidad de parámetro `domain_hint` o `login_hint` del inicio de sesión único de conexión directa?**

La siguiente es una lista parcial de las aplicaciones que envían estos parámetros a Azure AD y, por lo tanto, proporciona a los usuarios una experiencia de inicio de sesión silenciosa con SSO de conexión directa (es decir, los usuarios no tendrán que especificar su nombre de usuario o contraseñas):

| Nombre de la aplicación | La dirección URL de la aplicación que se va a usar |
| -- | -- |
| Panel de acceso | https:\//myapps.microsoft.com/contoso.com |
| Outlook en la Web | https:\//outlook.office365.com/contoso.com |
| Portales de Office 365 | https:\//portal.office.com?domain_hint=contoso.com, https:\//www.office.com?domain_hint=contoso.com |

Además, no es necesaria la intervención del usuario para iniciar sesión cuando una aplicación envía solicitudes de inicio de sesión a puntos de conexión de Azure AD configurados como inquilinos; es decir, https:\//login.microsoftonline.com/contoso.com/<..> o https:\//login.microsoftonline.com/<tenant_ID>/<..> en lugar del punto de conexión común de Azure AD; es decir, https:\//login.microsoftonline.com/common/<...>. A continuación se incluye una lista no exhaustiva de aplicaciones que hacen estos tipos de solicitudes de inicio de sesión.

| Nombre de la aplicación | La dirección URL de la aplicación que se va a usar |
| -- | -- |
| SharePoint Online | https:\//contoso.sharepoint.com |
| Portal de Azure | https:\//portal.azure.com/contoso.com |

En las tablas anteriores, reemplace "contoso.com" por el nombre de dominio para obtener las direcciones URL de aplicación correctas para el inquilino.

Si quiere que otras aplicaciones utilicen la experiencia de inicio de sesión silenciosa, comuníquenoslo en la sección de comentarios.

**P: ¿El inicio de sesión único de conexión directa admite `Alternate ID` como nombre de usuario en lugar de `userPrincipalName`?**

Sí. SSO de conexión directa admite `Alternate ID` como el nombre de usuario cuando se configura en Azure AD Connect, tal como se muestra [aquí](how-to-connect-install-custom.md). No todas las aplicaciones de Office 365 admiten `Alternate ID`. Consulte la documentación de la aplicación específica para conocer la declaración de compatibilidad.

**P: ¿Cuál es la diferencia entre la experiencia de inicio de sesión único que proporciona [Azure AD Join](../active-directory-azureadjoin-overview.md) y el inicio de sesión único de conexión directa?**

[Azure AD Join](../active-directory-azureadjoin-overview.md) permite el inicio de sesión único a los usuarios si sus dispositivos se han registrado con Azure AD. Estos dispositivos no deben estar unidos al dominio necesariamente. SSO se proporciona mediante *tokens de actualización principal* o *PRT*, y no con Kerberos. La experiencia del usuario es más adecuada en dispositivos Windows 10. SSO se realiza automáticamente en el explorador Microsoft Edge. También funciona en Chrome mediante una extensión de explorador.

Puede usar tanto Azure AD Join como SSO de conexión directa en el inquilino. Estas dos características son complementarias. Si ambas están activadas, SSO de Azure AD Join tiene prioridad sobre SSO de conexión directa.

**P: Quiero registrar dispositivos no de Windows 10 con Azure AD, sin usar AD FS. ¿Puedo usar el inicio de sesión único de conexión directa en su lugar?**

Sí, en este escenario se necesita la versión 2.1 o posterior del [cliente para unirse al área de trabajo](https://www.microsoft.com/download/details.aspx?id=53554).

**P: ¿Cómo puedo implementar la clave de descifrado de Kerberos de la cuenta de equipo `AZUREADSSOACC`?**

Es importante implementar con frecuencia la clave de descifrado de Kerberos de la cuenta de equipo `AZUREADSSOACC` (que representa a Azure AD) creada en el bosque local de AD.

>[!IMPORTANT]
>Se recomienda implementar la clave de descifrado de Kerberos al menos cada 30 días.

Siga estos pasos en el servidor local donde se ejecuta Azure AD Connect:

    **Step 1. Get list of AD forests where Seamless SSO has been enabled**

    1. Primero, descargue e instale [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview).
    2. Vaya a la carpeta `%programfiles%\Microsoft Azure Active Directory Connect`.
    3. Importe el módulo de PowerShell de SSO de conexión directa mediante este comando: `Import-Module .\AzureADSSO.psd1`.
    4. Ejecute PowerShell como administrador. En PowerShell, llame a `New-AzureADSSOAuthenticationContext`. Este comando debería mostrar un cuadro emergente para escribir las credenciales de administrador global del inquilino.
    5. Llame a `Get-AzureADSSOStatus | ConvertFrom-Json`. Aparecerá la lista de bosques de AD (examine la lista "Dominios") en la que se ha habilitado esta característica.

    **Paso 2. Actualización de la clave de descifrado de Kerberos en cada bosque de AD en el que se haya configurado**

    1. Llame a `$creds = Get-Credential`. Cuando se le pida, escriba las credenciales del administrador de dominio para el bosque de AD deseado.

    > [!NOTE]
    > Usamos el nombre de usuario del Administrador de dominio, que se proporciona con el formato de nombres principales de usuario (UPN) (johndoe@contoso.com), o bien con el formato de nombre de dominio completo de cuenta SAM (contoso\johndoe o contoso.com\johndoe), para encontrar el bosque de AD deseado. Si usa el nombre de dominio completo de cuenta SAM, usamos la parte del dominio del nombre de usuario para [localizar el controlador de dominio del Administrador de dominio con DNS](https://social.technet.microsoft.com/wiki/contents/articles/24457.how-domain-controllers-are-located-in-windows.aspx). Si usa UPN en su lugar, [la traducimos a un nombre de cuenta SAM de dominio completo](https://docs.microsoft.com/windows/desktop/api/ntdsapi/nf-ntdsapi-dscracknamesa) antes de localizar el controlador de dominio adecuado.

    2. Llame a `Update-AzureADSSOForest -OnPremCredentials $creds`. Este comando actualiza la clave de descifrado de Kerberos de la cuenta de equipo `AZUREADSSOACC` en este bosque de AD concreto y la actualiza en Azure AD.
    3. Repita los pasos anteriores para cada bosque de AD en el que haya configurado la característica.

    >[!IMPORTANT]
    >Asegúrese de _no_ ejecutar el comando `Update-AzureADSSOForest` más de una vez. De lo contrario, la característica deja de funcionar hasta que expira la hora de los vales Kerberos de los usuarios y se vuelven a emitir por parte de la instancia local de Active Directory.

**P: ¿Cómo se deshabilita el inicio de sesión único de conexión directa?**

    **Step 1. Disable the feature on your tenant**

        **Option A: Disable using Azure AD Connect**

        1. Run Azure AD Connect, choose **Change user sign-in page** and click **Next**.
        2. Uncheck the **Enable single sign on** option. Continue through the wizard.

        After completing the wizard, Seamless SSO will be disabled on your tenant. However, you will see a message on screen that reads as follows:

        "Single sign-on is now disabled, but there are additional manual steps to perform in order to complete clean-up. Learn more"

        To complete the clean-up process, follow steps 2 and 3 on the on-premises server where you are running Azure AD Connect.

        **Option B: Disable using PowerShell**

        Run the following steps on the on-premises server where you are running Azure AD Connect:

        1. First, download, and install [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview).
        2. Navigate to the `%programfiles%\Microsoft Azure Active Directory Connect` folder.
        3. Import the Seamless SSO PowerShell module using this command: `Import-Module .\AzureADSSO.psd1`.
        4. Run PowerShell as an Administrator. In PowerShell, call `New-AzureADSSOAuthenticationContext`. This command should give you a popup to enter your tenant's Global Administrator credentials.
        5. Call `Enable-AzureADSSO -Enable $false`.

        >[!IMPORTANT]
        >Disabling Seamless SSO using PowerShell will not change the state in Azure AD Connect. Seamless SSO will show as enabled in the **Change user sign-in** page.

    **Step 2. Get list of AD forests where Seamless SSO has been enabled**

    Follow tasks 1 through 4 below if you have disabled Seamless SSO using Azure AD Connect. If you have disabled Seamless SSO using PowerShell instead, jump ahead to task 5 below.

    1. Primero, descargue e instale [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview).
    2. Vaya a la carpeta `%programfiles%\Microsoft Azure Active Directory Connect`.
    3. Importe el módulo de PowerShell de SSO de conexión directa mediante este comando: `Import-Module .\AzureADSSO.psd1`.
    4. Ejecute PowerShell como administrador. En PowerShell, llame a `New-AzureADSSOAuthenticationContext`. Este comando debería mostrar un cuadro emergente para escribir las credenciales de administrador global del inquilino.
    5. Llame a `Get-AzureADSSOStatus | ConvertFrom-Json`. Aparecerá la lista de bosques de AD (examine la lista "Dominios") en la que se ha habilitado esta característica.

    **Paso 3. Elimine manualmente la cuenta de equipo `AZUREADSSOACCT` de cada bosque de AD que encuentre en la lista.**

## <a name="next-steps"></a>Pasos siguientes

- [**Inicio rápido**](how-to-connect-sso-quick-start.md): desarrollo y ejecución de SSO de conexión directa de Azure AD.
- [**Profundización técnica**](how-to-connect-sso-how-it-works.md): descripción del funcionamiento de esta característica.
- [**Solución de problemas**](tshoot-connect-sso.md): información para resolver problemas habituales de esta característica.
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): para la tramitación de solicitudes de nuevas características.
