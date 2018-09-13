---
title: 'Azure AD Connect: preguntas más frecuentes sobre el inicio de sesión único de conexión directa | Microsoft Docs'
description: Respuestas a las preguntas más frecuentes sobre el inicio de sesión único de conexión directa de Azure Active Directory.
services: active-directory
keywords: qué es Azure AD Connect, instalar Active Directory, componentes necesarios para Azure AD, SSO, inicio de sesión único
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 5f654ce8730af1e66e0186d7087aa130b00afd2b
ms.sourcegitcommit: e2348a7a40dc352677ae0d7e4096540b47704374
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2018
ms.locfileid: "43782115"
---
# <a name="azure-active-directory-seamless-single-sign-on-frequently-asked-questions"></a>Preguntas más frecuentes sobre el inicio de sesión único de conexión directa de Azure Active Directory

En este artículo se ofrece respuesta a las preguntas más frecuentes sobre el inicio de sesión único de conexión directa (SSO de conexión directa) de Azure Active Directory. Siga comprobando si hay contenido nuevo.

## <a name="what-sign-in-methods-do-seamless-sso-work-with"></a>¿Con qué métodos de inicio de sesión funciona el SSO de conexión directa?

SSO de conexión directa se puede combinar con los métodos de inicio de sesión mediante [sincronización de hash de contraseñas](active-directory-aadconnectsync-implement-password-hash-synchronization.md) o [autenticación de paso a través](active-directory-aadconnect-pass-through-authentication.md). Pero esta característica no se puede usar con los Servicios de federación de Active Directory (AD FS).

## <a name="is-seamless-sso-a-free-feature"></a>¿SSO de conexión directa es una característica gratuita?

SSO de conexión directa es una característica gratuita y no es necesario usar ninguna versión de pago de Azure AD para usarla.

## <a name="is-seamless-sso-available-in-the-microsoft-azure-germany-cloudhttpwwwmicrosoftdecloud-deutschland-and-the-microsoft-azure-government-cloudhttpsazuremicrosoftcomfeaturesgov"></a>¿Está disponible el inicio de sesión único de conexión directa en la [nube Microsoft Azure Alemania](http://www.microsoft.de/cloud-deutschland) y en la [nube de Microsoft Azure Government](https://azure.microsoft.com/features/gov/)?

No. El inicio de sesión único de conexión directa solo está disponible en la instancia mundial de Azure AD.

## <a name="what-applications-take-advantage-of-domainhint-or-loginhint-parameter-capability-of-seamless-sso"></a>¿Qué aplicaciones aprovechan la funcionalidad de parámetro `domain_hint` o `login_hint` de SSO de conexión directa?

La siguiente es una lista parcial de las aplicaciones que envían estos parámetros a Azure AD y, por lo tanto, proporciona a los usuarios una experiencia de inicio de sesión silenciosa con SSO de conexión directa (es decir, los usuarios no tendrán que especificar su nombre de usuario o contraseñas):

| Nombre de la aplicación | La dirección URL de la aplicación que se va a usar |
| -- | -- |
| Panel de acceso | https://myapps.microsoft.com/contoso.com |
| Outlook en la Web | https://outlook.office365.com/contoso.com |
| Portal de Office 365 | https://portal.office.com?domain_hint=contoso.com |

Además, los usuarios obtienen una experiencia de inicio de sesión silenciosa si una aplicación envía solicitudes de inicio de sesión a los puntos de conexión con inquilino de Azure AD; es decir, https://login.microsoftonline.com/contoso.com/<..> o https://login.microsoftonline.com/<tenant_ID>/<..>, en lugar del punto de conexión común de Azure AD; es decir, https://login.microsoftonline.com/common/<...>. A continuación se incluye una lista no exhaustiva de aplicaciones que hacen estos tipos de solicitudes de inicio de sesión.

| Nombre de la aplicación | La dirección URL de la aplicación que se va a usar |
| -- | -- |
| SharePoint Online | https://contoso.sharepoint.com |
| Azure Portal | https://portal.azure.com/contoso.com |

En las tablas anteriores, reemplace "contoso.com" por el nombre de dominio para obtener las direcciones URL de aplicación correctas para el inquilino.

Si quiere que otras aplicaciones utilicen la experiencia de inicio de sesión silenciosa, comuníquenoslo en la sección de comentarios.

## <a name="does-seamless-sso-support-alternate-id-as-the-username-instead-of-userprincipalname"></a>¿SSO de conexión directa admite `Alternate ID` como nombre de usuario en lugar de `userPrincipalName`?

Sí. SSO de conexión directa admite `Alternate ID` como el nombre de usuario cuando se configura en Azure AD Connect, tal como se muestra [aquí](active-directory-aadconnect-get-started-custom.md). No todas las aplicaciones de Office 365 admiten `Alternate ID`. Consulte la documentación de la aplicación específica para conocer la declaración de compatibilidad.

## <a name="what-is-the-difference-between-the-single-sign-on-experience-provided-by-azure-ad-joinactive-directory-azureadjoin-overviewmd-and-seamless-sso"></a>¿Cuál es la diferencia entre la experiencia de inicio de sesión único que proporciona [Azure AD Join](../active-directory-azureadjoin-overview.md) y SSO de conexión directa?

[Azure AD Join](../active-directory-azureadjoin-overview.md) permite el inicio de sesión único a los usuarios si sus dispositivos se han registrado con Azure AD. Estos dispositivos no deben estar unidos al dominio necesariamente. SSO se proporciona mediante *tokens de actualización principal* o *PRT*, y no con Kerberos. La experiencia del usuario es más adecuada en dispositivos Windows 10. SSO se realiza automáticamente en el explorador Edge. También funciona en Chrome mediante una extensión de explorador.

Puede usar tanto Azure AD Join como SSO de conexión directa en el inquilino. Estas dos características son complementarias. Si ambas están activadas, SSO de Azure AD Join tiene prioridad sobre SSO de conexión directa.

## <a name="i-want-to-register-non-windows-10-devices-with-azure-ad-without-using-ad-fs-can-i-use-seamless-sso-instead"></a>Quiero registrar dispositivos no de Windows 10 con Azure AD, sin usar AD FS. ¿Puedo usar SSO de conexión directa en su lugar?

Sí, en este escenario se necesita la versión 2.1 o posterior del [cliente para unirse al área de trabajo](https://www.microsoft.com/download/details.aspx?id=53554).

## <a name="how-can-i-roll-over-the-kerberos-decryption-key-of-the-azureadssoacc-computer-account"></a>¿Cómo puedo implementar la clave de descifrado de Kerberos de la cuenta de equipo `AZUREADSSOACC`?

Es importante implementar con frecuencia la clave de descifrado de Kerberos de la cuenta de equipo `AZUREADSSOACC` (que representa a Azure AD) creada en el bosque local de AD.

>[!IMPORTANT]
>Se recomienda implementar la clave de descifrado de Kerberos al menos cada 30 días.

Siga estos pasos en el servidor local donde se ejecuta Azure AD Connect:

### <a name="step-1-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>Paso 1. Obtención de la lista de bosques de AD en los que se habilitó SSO de conexión directa

1. Primero, descargue e instale [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview).
2. Vaya a la carpeta `%programfiles%\Microsoft Azure Active Directory Connect`.
3. Importe el módulo de PowerShell de SSO de conexión directa mediante este comando: `Import-Module .\AzureADSSO.psd1`.
4. Ejecute PowerShell como administrador. En PowerShell, llame a `New-AzureADSSOAuthenticationContext`. Este comando debería mostrar un cuadro emergente para escribir las credenciales de administrador global del inquilino.
5. Llame a `Get-AzureADSSOStatus`. Aparecerá la lista de bosques de AD (examine la lista "Dominios") en la que se ha habilitado esta característica.

### <a name="step-2-update-the-kerberos-decryption-key-on-each-ad-forest-that-it-was-set-it-up-on"></a>Paso 2. Actualización de la clave de descifrado de Kerberos en cada bosque de AD en el que se haya configurado

1. Llame a `$creds = Get-Credential`. Cuando se le pida, escriba las credenciales del administrador de dominio para el bosque de AD deseado.

    >[!NOTE]
    >Usamos el nombre de usuario del Administrador de dominio, que se proporciona con el formato de nombres principales de usuario (UPN) (johndoe@contoso.com), o bien con el formato de nombre de dominio completo de cuenta SAM (contoso\johndoe o contoso.com\johndoe), para encontrar el bosque de AD deseado. Si usa el nombre de dominio completo de cuenta SAM, usamos la parte del dominio del nombre de usuario para [localizar el controlador de dominio del Administrador de dominio con DNS](https://social.technet.microsoft.com/wiki/contents/articles/24457.how-domain-controllers-are-located-in-windows.aspx). Si usa UPN en su lugar, [la traducimos a un nombre de cuenta SAM de dominio completo](https://docs.microsoft.com/windows/desktop/api/ntdsapi/nf-ntdsapi-dscracknamesa) antes de localizar el controlador de dominio adecuado.

2. Llame a `Update-AzureADSSOForest -OnPremCredentials $creds`. Este comando actualiza la clave de descifrado de Kerberos de la cuenta de equipo `AZUREADSSOACC` en este bosque de AD concreto y la actualiza en Azure AD.
3. Repita los pasos anteriores para cada bosque de AD en el que haya configurado la característica.

>[!IMPORTANT]
>Asegúrese de _no_ ejecutar el comando `Update-AzureADSSOForest` más de una vez. De lo contrario, la característica deja de funcionar hasta que expira la hora de los vales Kerberos de los usuarios y se vuelven a emitir por parte de la instancia local de Active Directory.

## <a name="how-can-i-disable-seamless-sso"></a>¿Cómo se deshabilita SSO de conexión directa?

### <a name="step-1-disable-the-feature-on-your-tenant"></a>Paso 1. Deshabilite la característica en su inquilino

#### <a name="option-a-disable-using-azure-ad-connect"></a>Opción A: deshabilitar el uso de Azure AD Connect

1. Ejecute Azure AD Connect, elija **Change user sign-in page** (Cambiar página de inicio de sesión de usuario) y haga clic en **Siguiente**.
2. Desactive la opción **Enable single sign on** (Habilitar el inicio de sesión único). Continúe con el asistente.

Cuando haya finalizado con el asistente, el SSO de conexión directa estará deshabilitado en su inquilino. Sin embargo, verá un mensaje en pantalla en el que se anuncia lo siguiente:

"El inicio de sesión único ya está deshabilitado, pero es necesario completar otros pasos manualmente para finalizar el proceso. Más información".

Para completar el proceso de limpieza, siga los pasos 2 y 3 en el servidor local donde se ejecuta Azure AD Connect.

#### <a name="option-b-disable-using-powershell"></a>Opción B: deshabilitar el uso de PowerShell

Ejecute los pasos siguientes en el servidor local donde se ejecuta Azure AD Connect:

1. Primero, descargue e instale [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview).
2. Vaya a la carpeta `%programfiles%\Microsoft Azure Active Directory Connect`.
3. Importe el módulo de PowerShell de SSO de conexión directa mediante este comando: `Import-Module .\AzureADSSO.psd1`.
4. Ejecute PowerShell como administrador. En PowerShell, llame a `New-AzureADSSOAuthenticationContext`. Este comando debería mostrar un cuadro emergente para escribir las credenciales de administrador global del inquilino.
5. Llame a `Enable-AzureADSSO -Enable $false`.

>[!IMPORTANT]
>Si deshabilita SSO de conexión directa con PowerShell, no cambiará el estado en Azure AD Connect. SSO de conexión directa se mostrará como habilitado en la página **Change user sign-in** (Cambiar inicio de sesión de usuario).

### <a name="step-2-get-list-of-ad-forests-where-seamless-sso-has-been-enabled"></a>Paso 2. Obtención de la lista de bosques de AD en los que se habilitó SSO de conexión directa

Realice las tareas 1 a 4 a continuación si ha deshabilitado el inicio de sesión único de conexión directa con Azure AD Connect. Si, en cambio, ha deshabilitado el inicio de sesión único de conexión directa con PowerShell, avance a la tarea 5 a continuación.

1. Primero, descargue e instale [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/overview).
2. Vaya a la carpeta `%programfiles%\Microsoft Azure Active Directory Connect`.
3. Importe el módulo de PowerShell de SSO de conexión directa mediante este comando: `Import-Module .\AzureADSSO.psd1`.
4. Ejecute PowerShell como administrador. En PowerShell, llame a `New-AzureADSSOAuthenticationContext`. Este comando debería mostrar un cuadro emergente para escribir las credenciales de administrador global del inquilino.
5. Llame a `Get-AzureADSSOStatus`. Aparecerá la lista de bosques de AD (examine la lista "Dominios") en la que se ha habilitado esta característica.

### <a name="step-3-manually-delete-the-azureadssoacct-computer-account-from-each-ad-forest-that-you-see-listed"></a>Paso 3. Elimine manualmente la cuenta de equipo `AZUREADSSOACCT` de cada bosque de AD que encuentre en la lista.

## <a name="next-steps"></a>Pasos siguientes

- [**Inicio rápido**](active-directory-aadconnect-sso-quick-start.md): desarrollo y ejecución de SSO de conexión directa de Azure AD.
- [**Profundización técnica**](active-directory-aadconnect-sso-how-it-works.md): descripción del funcionamiento de esta característica.
- [**Solución de problemas**](active-directory-aadconnect-troubleshoot-sso.md): información para resolver problemas habituales de esta característica.
- [**UserVoice**](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): para la tramitación de solicitudes de nuevas características.
