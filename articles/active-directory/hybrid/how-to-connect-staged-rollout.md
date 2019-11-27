---
title: 'Azure AD Connect: autenticación en la nube: lanzamiento preconfigurado | Microsoft Docs'
description: Explica cómo migrar desde la autenticación federada a la autenticación en la nube mediante un lanzamiento preconfigurado.
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/07/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2596091324acde5c4fdc3f7c467849f90266fec9
ms.sourcegitcommit: 16c5374d7bcb086e417802b72d9383f8e65b24a7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2019
ms.locfileid: "73847223"
---
# <a name="cloud-authentication-staged-rollout-public-preview"></a>Autenticación en la nube: lanzamiento preconfigurado (versión preliminar pública)

Esta característica permite migrar desde la autenticación federada a la autenticación en la nube mediante un enfoque por fases.

El abandono de la autenticación federada tiene implicaciones. Por ejemplo, si tiene alguno de los siguientes elementos:
    
-  un servidor de MFA local 
-  uso de tarjetas inteligentes para la autenticación 
-  otras características solo para federación

Estas características deben tenerse en cuenta antes de cambiar a la autenticación en la nube.  Antes de probar esta característica, le recomendamos que revise la guía sobre cómo elegir el método de autenticación correcto. Consulte [esta tabla](https://docs.microsoft.com/azure/security/fundamentals/choose-ad-authn#comparing-methods) para más información.

>[!VIDEO https://www.microsoft.com/videoplayer/embed/RE3inQJ]



## <a name="prerequisites"></a>Requisitos previos

-   Dispone de un inquilino de Azure AD con dominios federados.

-   Ha decidido pasar a la sincronización de hash de contraseñas + SSO de conexión directa **(opción A)** o la autenticación de paso a través + SSO de conexión directa **(opción B)** . Aunque el SSO de conexión directa es opcional, se recomienda habilitarlo para lograr una experiencia de inicio de sesión silenciosa para los usuarios que usan máquinas unidas a un dominio desde dentro de la red corporativa.

-   Ha configurado todas las directivas de acceso condicional y la personalización de marca del inquilino adecuadas que necesita para los usuarios que se van a migrar a la autenticación en la nube.

-   Si tiene previsto usar Azure Multi-Factor Authentication, le recomendamos que use el [registro convergente para el autoservicio de restablecimiento de contraseña (SSPR) y Azure MFA](../authentication/concept-registration-mfa-sspr-combined.md) para que los usuarios registren sus métodos de autenticación una vez.

-   Para usar esta característica, debe ser administrador global en el inquilino.

-   Para habilitar SSO de conexión directa en un bosque de AD específico, debe ser administrador de dominio.

## <a name="supported-scenarios"></a>Escenarios admitidos

Se admiten estos escenarios para el lanzamiento preconfigurado:

- Esta característica solo funciona para los usuarios aprovisionados en Azure AD con Azure AD Connect y no es aplicable a los usuarios solo en la nube.

- Esta característica solo funciona para el tráfico de inicio de sesión de usuario en exploradores y clientes de autenticación modernos. Las aplicaciones o los servicios en la nube que usan la autenticación heredada revertirán a los flujos de autenticación federada. (Ejemplo: Exchange Online con la autenticación moderna desactivada o Outlook 2010, que no admite la autenticación moderna).

## <a name="unsupported-scenarios"></a>Escenarios no admitidos

No se admiten estos escenarios para el lanzamiento preconfigurado:

- Ciertas aplicaciones envían el parámetro de consulta "domain\_hint" a Azure AD durante la autenticación. Estos flujos continuarán y los usuarios habilitados para el lanzamiento preconfigurado seguirán usando la federación para la autenticación.

<!-- -->

- El administrador puede implementar la autenticación en la nube mediante grupos de seguridad. (Se recomiendan los grupos de seguridad en la nube para evitar la latencia de sincronización cuando se usan grupos de seguridad de AD locales).

    - Puede usar un **máximo de 10 grupos por característica**; es decir, para cada una de las opciones Sincronización de hash de contraseñas / Autenticación de paso a través / SSO de conexión directa.

    - **No se admiten** grupos anidados. Este también es el ámbito de la versión preliminar pública.

    - Los grupos dinámicos **no se admiten** en el lanzamiento preconfigurado.

    - Los objetos de contacto dentro del grupo bloquearán el formulario de grupo que se va a agregar.

- El traslado final de la autenticación federada a la nube todavía debe realizarse con Azure AD Connect o PowerShell. Esta característica no cambia los dominios de federado a administrado.

- La primera vez que se agrega un grupo de seguridad para el lanzamiento preconfigurado, se limita a 200 usuarios para evitar que se agote el tiempo de espera de la experiencia de usuario. Una vez que se agrega el grupo en la experiencia de usuario, puede agregar más usuarios directamente al grupo según sea necesario.

## <a name="get-started-with-staged-rollout"></a>Introducción al lanzamiento preconfigurado

Si quiere probar el inicio de sesión de sincronización de hash de contraseñas (PHS) mediante el lanzamiento preconfigurado, complete los trabajos previos siguientes para habilitar el lanzamiento preconfigurado de sincronización de hash de contraseñas.

Para más información sobre los cmdlets de PowerShell que se usan, consulte [Azure AD 2.0 Versión preliminar](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview#staged_rollout)

## <a name="pre-work-for-password-hash-sync"></a>Trabajos previos para la sincronización de hash de contraseñas

1. Habilite la Sincronización de hash de contraseñas en la página [Características opcionales](how-to-connect-install-custom.md#optional-features) de Azure AD Connect. 

   ![Captura de pantalla de la página Características opcionales de Azure Active Directory Connect](media/how-to-connect-staged-rollout/sr1.png)

1. Asegúrese de que se ha ejecutado un ciclo de sincronización de hash de contraseñas completo para que todos los hash de contraseña de los usuarios se hayan sincronizado con Azure AD. Puede usar el diagnóstico de PowerShell indicado [aquí](tshoot-connect-password-hash-synchronization.md) para comprobar el estado de la sincronización de hash de contraseñas.

   ![Captura de pantalla del registro de solución de problemas de AAD Connect](./media/how-to-connect-staged-rollout/sr2.png)

Si quiere probar el inicio de sesión de autenticación de paso a través (PTA) mediante el lanzamiento preconfigurado, complete los trabajos previos siguientes para habilitar PTA para el lanzamiento preconfigurado.

## <a name="pre-work-for-pass-through-authentication"></a>Trabajos previos para la autenticación de paso a través

1. Identifique un servidor que ejecute Windows Server 2012 R2 o posterior en el que desea que se ejecute el agente de la autenticación de paso a través (**no elija el servidor de Azure AD Connect**). Asegúrese de que el servidor está unido a un dominio, que puede autenticar a los usuarios seleccionados con Active Directory y que puede comunicarse con Azure AD en los puertos y direcciones URL de salida (consulte los [requisitos previos](how-to-connect-sso-quick-start.md) detallados).

1. [Descargue](https://aka.ms/getauthagent)  e instale el agente de autenticación de Microsoft Azure AD Connect en el servidor. 

1. Para habilitar la [alta disponibilidad](how-to-connect-sso-quick-start.md), instale agentes de autenticación adicionales en otros servidores.

1. Asegúrese de que ha configurado las [opciones de Bloqueo inteligente](../authentication/howto-password-smart-lockout.md) adecuadamente. Esto se hace para asegurarse de que las cuentas de Active Directory locales de los usuarios no sean bloqueadas por actores no válidos.

Se recomienda habilitar SSO de conexión directa con independencia del método de inicio de sesión (PHS o PTA) que seleccione para el lanzamiento preconfigurado. Complete los siguientes trabajos previos para habilitar SSO de conexión directa para el lanzamiento preconfigurado.

## <a name="pre-work-for-seamless-sso"></a>Trabajos previos para SSO de conexión directa

Habilite el inicio de sesión único de conexión directa en los bosques de AD mediante PowerShell. Si tiene más de un bosque de AD, habilite el mismo para cada bosque individualmente. El inicio de sesión único de conexión directa solo se desencadenará para los usuarios seleccionados para el lanzamiento preconfigurado y no afectará a la configuración de federación existente.

**Resumen de los pasos**

1. En primer lugar, inicie sesión en el servidor de Azure AD Connect.

2. Vaya a la carpeta %ProgramFiles%\\Microsoft Azure Active Directory Connect.

3. Importe el módulo de PowerShell de SSO de conexión directa mediante este comando:  `Import-Module .\AzureADSSO.psd1`.

4. Ejecute PowerShell como administrador. En PowerShell, llame a `New-AzureADSSOAuthenticationContext`. Este comando debería mostrar un cuadro de diálogo en el que puede escribir las credenciales del administrador global del inquilino.

5. Llame a `Get-AzureADSSOStatus | ConvertFrom-Json`. Este comando le proporciona la lista de bosques de AD (examine la lista \"Dominios\") en los que se ha habilitado esta característica. De forma predeterminada, se establece en False en el nivel de inquilino.

   > **Ejemplo:** 
   > ![Ejemplo de la salida de Windows PowerShell](./media/how-to-connect-staged-rollout/sr3.png)

6. Llame a `$creds = Get-Credential`. Cuando se le pida, escriba las credenciales del administrador de dominio para el bosque de AD deseado.

7. Llame a `Enable-AzureADSSOForest -OnPremCredentials $creds`. Este comando cuenta la cuenta del equipo AZUREADSSOACC del controlador de dominio local para este bosque de Active Directory específico que se requiere para SSO de conexión directa.

8. El inicio de sesión único de conexión directa requiere que las direcciones URL estén en la zona de intranet. Consulte la [guía de inicio rápido del inicio de sesión único de conexión directa](how-to-connect-sso-quick-start.md#step-3-roll-out-the-feature) para implementar dichas direcciones URL mediante directivas de grupo.

9.  También puede descargar los [planes de implementación](https://aka.ms/SeamlessSSODPDownload) para SSO de conexión directa para obtener un tutorial completo.

## <a name="enable-staged-rollout"></a>Habilitación del lanzamiento preconfigurado

Siga estos pasos para implementar una característica específica (Autenticación de paso a través / Sincronización de hash de contraseñas / SSO de conexión directa) en un conjunto seleccionado de usuarios de un grupo:

### <a name="enable-the-staged-rollout-of-a-specific-feature-on-your-tenant"></a>Habilitación del lanzamiento preconfigurado de una característica específica en el inquilino

Puede implementar una de estas opciones.

-   Sincronización de hash de contraseñas + SSO de conexión directa **(Opción A)**

-   Autenticación de paso a través + SSO de conexión directa **(Opción B)**

-   Sincronización de hash de contraseñas + Autenticación de paso a través + SSO de conexión directa **-\>** ***No se admite***

Siga estos pasos:

1. Inicie sesión en el portal de Azure AD mediante la siguiente dirección URL para acceder a la versión preliminar de la experiencia de usuario.

   > <https://aka.ms/stagedrolloutux>

2. Haga clic en Habilitación del lanzamiento preconfigurado para el inicio de sesión de usuarios administrados (versión preliminar)

   *Por ejemplo:* (**Opción B**) Si desea habilitar la sincronización de hash de contraseñas y el inicio de sesión único de conexión directa, establezca las características Sincronización de hash de contraseñas e Inicio de sesión único de conexión directa en **"ON"** , como se muestra a continuación.

   ![](./media/how-to-connect-staged-rollout/sr4.png)

   ![](./media/how-to-connect-staged-rollout/sr5.png)

3. Agregue los grupos respectivos a la característica para habilitar la autenticación de paso a través y el inicio de sesión único de conexión directa. Asegúrese de que los grupos de seguridad no tienen más de 200 miembros inicialmente para evitar agotar el tiempo de espera de la experiencia de usuario.

   ![](./media/how-to-connect-staged-rollout/sr6.png)

   >[!NOTE]
   >Los miembros de un grupo se habilitan automáticamente para el lanzamiento preconfigurado. Los grupos anidados y dinámicos no se admiten en el lanzamiento preconfigurado.

## <a name="auditing"></a>Auditoría

Hemos habilitado eventos de auditoría para las diferentes acciones que realizamos en el lanzamiento preconfigurado.

- Evento de auditoría al habilitar el lanzamiento preconfigurado para la Sincronización de hash de contraseñas, la Autenticación de paso a través o el Inicio de sesión único de conexión directa.

  >[!NOTE]
  >Evento de auditoría que se registra cuando se establece SSO de conexión directa en **On** mediante el lanzamiento preconfigurado.

  ![](./media/how-to-connect-staged-rollout/sr7.png)

  ![](./media/how-to-connect-staged-rollout/sr8.png)

- Evento de auditoría al agregar un grupo a la Sincronización de hash de contraseñas, la Autenticación de paso a través o el Inicio de sesión único de conexión directa.

  >[!NOTE]
  >Evento de auditoría registrado cuando se agrega un grupo a la sincronización de hash de contraseñas para el lanzamiento preconfigurado

  ![](./media/how-to-connect-staged-rollout/sr9.png)

  ![](./media/how-to-connect-staged-rollout/sr10.png)

- Evento de auditoría cuando un usuario agregado al grupo se habilita para el lanzamiento preconfigurado

  ![](media/how-to-connect-staged-rollout/sr11.png)

  ![](./media/how-to-connect-staged-rollout/sr12.png)

## <a name="validation"></a>Validación

Para probar el inicio de sesión con la sincronización de hash de contraseñas o la autenticación de paso a través (inicio de sesión con nombre de usuario y contraseña):

1. Vaya a <https://myapps.microsoft.com> en una sesión privada del explorador desde la extranet y escriba el nombre principal de usuario (UPN) de la cuenta de usuario seleccionada para el lanzamiento preconfigurado.

1. Si el usuario se ha seleccionado para el lanzamiento preconfigurado, no se redirigirá al usuario a la página de inicio de sesión federado y, en su lugar, se le pedirá que inicie sesión en la página de inicio de sesión con la marca del inquilino de Azure AD.

1. Asegúrese de que el inicio de sesión se muestra correctamente en el [informe de actividad de inicio de sesión de Azure AD](../reports-monitoring/concept-sign-ins.md) mediante el filtrado por el nombre principal de usuario.

Para probar el inicio de sesión con SSO de conexión directa:

1. Vaya a <https://myapps.microsoft.com>/ en una sesión privada del explorador desde la intranet y escriba el nombre principal de usuario (UPN) de la cuenta de usuario seleccionada para el lanzamiento preconfigurado.

1. Si el usuario se ha seleccionado para el lanzamiento preconfigurado del inicio de sesión único de conexión directa, el usuario verá el mensaje "Intentando iniciar sesión..." antes de que se inicie sesión de forma silenciosa.

1. Asegúrese de que el inicio de sesión se muestra correctamente en el [informe de actividad de inicio de sesión de Azure AD](../reports-monitoring/concept-sign-ins.md) mediante el filtrado por el nombre principal de usuario.

Para comprobar que los inicios de sesión de los usuarios siguen ocurriendo en los proveedores de federación:

A continuación se muestra cómo puede realizar un seguimiento de que los inicios de sesión de los usuarios siguen ocurriendo en AD FS para los usuarios seleccionados para el lanzamiento preconfigurado con [estas instrucciones](https://docs.microsoft.com/windows-server/identity/ad-fs/troubleshooting/ad-fs-tshoot-logging#types-of-events). Consulte la documentación del proveedor para obtener información sobre cómo comprobarlo en proveedores de federación de terceros.

## <a name="roll-back"></a>Reversión

### <a name="remove-a-user-from-staged-rollout"></a>Eliminación de un usuario del lanzamiento preconfigurado

1.  Al eliminar el usuario del grupo, se deshabilita el lanzamiento preconfigurado para el usuario.

2.  Si desea deshabilitar la característica Lanzamiento preconfigurado, deslice la característica de nuevo al estado **"Desactivado"** para desactivar el lanzamiento preconfigurado.


## <a name="frequently-asked-questions"></a>Preguntas más frecuentes

-   **P: ¿Puede un cliente usar esta funcionalidad en producción?**

-   R: Sí, esta característica se puede usar en el inquilino de producción, pero se recomienda que pruebe primero esta funcionalidad en el inquilino de prueba.

-   **P: ¿Se puede usar esta característica para mantener una "coexistencia" permanente en la que algunos usuarios usan la autenticación federada y otros la autenticación en la nube?**

-   R: No, esta característica está diseñada para la migración de la autenticación federada a la nube en fases y, después, a la autenticación en la nube. No se recomienda un estado mixto permanente, ya que esto podría provocar flujos de autenticación inesperados.

-   **P: ¿Se puede usar PowerShell para realizar el lanzamiento preconfigurado?**

-   R: Sí, busque la documentación sobre el uso de PowerShell para realizar el lanzamiento preconfigurado [aquí](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview#staged_rollout).

## <a name="next-steps"></a>Pasos siguientes
- [Azure AD 2.0 (versión preliminar)](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0-preview#staged_rollout )
