---
title: 'Autenticación de paso a través de Azure AD: inicio rápido | Microsoft Docs'
description: En este artículo se describe cómo empezar a usar la autenticación de paso a través de Azure Active Directory (Azure AD).
services: active-directory
keywords: Autenticación de paso a través de Azure AD Connect, instalación de Active Directory, componentes necesarios para Azure AD, SSO, inicio de sesión único
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 9f994aca-6088-40f5-b2cc-c753a4f41da7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/03/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 29517f057599c7bf108d1c4d525b6c67c1b6b46a
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46305174"
---
# <a name="azure-active-directory-pass-through-authentication-quick-start"></a>Autenticación de paso a través de Azure Active Directory: inicio rápido

## <a name="deploy-azure-ad-pass-through-authentication"></a>Implementación de la autenticación de paso a través de Azure AD

La autenticación de paso a través de Azure Active Directory (Azure AD) permite a los usuarios iniciar sesión en aplicaciones basadas en la nube y locales con las mismas contraseñas. Con la autenticación de paso a través, los usuarios inician sesión mediante la validación de sus contraseñas directamente en la instancia de Active Directory local.

>[!IMPORTANT]
>Si va a migrar desde AD FS (u otra tecnología de federación) a la autenticación de paso a través, es muy recomendable que siga nuestra guía de implementación detallada publicada [aquí](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx).

Siga estas instrucciones para implementar la autenticación de paso a través en su inquilino:

## <a name="step-1-check-the-prerequisites"></a>Paso 1: Comprobar los requisitos previos

Asegúrese de que se cumplen los siguientes requisitos previos.

### <a name="in-the-azure-active-directory-admin-center"></a>En el Centro de administración de Azure Active Directory

1. Cree una cuenta de administrador global solo en la nube en el inquilino de Azure AD. De esta manera, puede administrar la configuración del inquilino en caso de que los servicios locales fallen o no estén disponibles. Información acerca de la [incorporación de una cuenta de administrador global que está solo en la nube](../active-directory-users-create-azure-portal.md). Realizar este paso es esencial para garantizar que no queda bloqueado fuera de su inquilino.
2. Agregue uno o varios [nombres de dominio personalizados](../active-directory-domains-add-azure-portal.md) al inquilino de Azure AD. Los usuarios pueden iniciar sesión con uno de estos nombres de dominio.

### <a name="in-your-on-premises-environment"></a>En el entorno local

1. Identifique un servidor con Windows Server 2012 R2 o posterior en el que ejecutar Azure AD Connect. Agregue el servidor al mismo bosque de Active Directory que los usuarios cuyas contraseñas se deben validar.
2. Instale la [última versión de Azure AD Connect](https://www.microsoft.com/download/details.aspx?id=47594) en el servidor identificado en el paso anterior. Si ya tiene Azure AD Connect en ejecución, asegúrese de que la versión es 1.1.750.0 o posterior.

    >[!NOTE]
    >Las versiones de Azure AD Connect 1.1.557.0, 1.1.558.0, 1.1.561.0 y 1.1.614.0 tienen un problema relacionado con la sincronización de hash de contraseña. Si _no_ pretende utilizar la sincronización de hash de contraseña junto con la autenticación de paso a través, consulte las [notas del historial de versiones de Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#116470).

3. Identifique uno o más servidores adicionales (que ejecute Windows Server 2012 R2 o versiones posteriores) en el que poder ejecutar los agentes de autenticación independientes. Estos servidores adicionales son necesarios para garantizar la alta disponibilidad de las solicitudes de inicio de sesión. Agregue los servidores al mismo bosque de Active Directory que los usuarios cuyas contraseñas se deben validar.

    >[!IMPORTANT]
    >En entornos de producción, se recomienda tener un mínimo de 3 agentes de autenticación en ejecución en el inquilino. Hay un límite de sistema de 12 agentes de autenticación por inquilino. Y, como procedimiento recomendado, trate todos los servidores que ejecutan los agentes de autenticación como sistemas de nivel 0 (consulte la [referencia](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)).

4. Si hay un firewall entre los servidores y Azure AD, configure los elementos siguientes:
   - Asegúrese de que los agentes de autenticación pueden realizar solicitudes *salientes* a Azure AD a través de los puertos siguientes:
   
    | Número de puerto | Cómo se usa |
    | --- | --- |
    | **80** | Descarga las listas de revocación de certificados (CRL) al validar el certificado SSL |
    | **443** | Controla toda la comunicación saliente con el servicio |
    | **8080** (opcional) | Los agentes de autenticación notifican su estado cada diez minutos a través del puerto 8080, si el puerto 443 no está disponible. Este estado se muestra en el portal de Azure AD. El puerto 8080 _no_ se usa para inicios de sesión de usuario. |
   
    Si el firewall fuerza las reglas según los usuarios que las originan, abra estos puertos para el tráfico de servicios de Windows que se ejecutan como un servicio de red.
   - Si el firewall o el proxy permiten la creación de listas blancas con DNS, cree una lista blanca para las conexiones a **\*.msappproxy.net** y **\*.servicebus.windows.net**. En caso contrario, permita el acceso a los [intervalos de direcciones IP del centro de datos de Azure](https://www.microsoft.com/download/details.aspx?id=41653), que se actualizan cada semana.
   - Los agentes de autenticación necesitan acceder a **login.windows.net** y **login.microsoftonline.com** para el registro inicial. Abra el firewall también para esas direcciones URL.
   - Para la validación de certificados, desbloquee las siguientes direcciones URL: **mscrl.microsoft.com:80**, **crl.microsoft.com:80**, **ocsp.msocsp.com:80** y **www.microsoft.com:80**. Como estas direcciones URL se utilizan para la validación de certificados con otros productos de Microsoft, es posible que estas direcciones URL ya estén desbloqueadas.

## <a name="step-2-enable-the-feature"></a>Paso 2: Habilitación de la característica

Habilite la [autenticación de paso a través mediante Azure AD Connect](whatis-hybrid-identity.md).

>[!IMPORTANT]
>Puede habilitar la autenticación de paso a través en el servidor principal o provisional de Azure AD Connect. Se recomienda muy especialmente habilitarlo desde el servidor principal. Si va a configurar un servidor de almacenamiento provisional de Azure AD Connect en el futuro, **debe** continuar para elegir Autenticación de paso a través como opción de inicio de sesión; la elección de otra opción **deshabilitará** la autenticación de paso a través en el inquilino y reemplazará el valor en el servidor principal.

Si va a instalar Azure AD Connect por primera vez, elija la [ruta de acceso de instalación personalizada](how-to-connect-install-custom.md). En la página **Inicio de sesión de usuario**, elija **Autenticación de paso a través** como **método de inicio de sesión**. Si se completa correctamente, se instala un agente de autenticación de paso a través en el mismo servidor que Azure AD Connect. Además, se habilita la característica de autenticación de paso a través en el inquilino.

![Azure AD Connect: Inicio de sesión de usuario](./media/how-to-connect-pta-quick-start/sso3.png)

Si ya tiene instalado Azure AD Connect mediante la ruta de [instalación rápida](how-to-connect-install-express.md) o de [instalación personalizada](how-to-connect-install-custom.md), elija la tarea **Cambiar inicio de sesión de usuario** en Azure AD Connect y después seleccione **Siguiente**. Después, seleccione **Autenticación de paso a través** como el método de inicio de sesión. Si se completa correctamente, se instala un agente de autenticación de paso a través en el mismo servidor que Azure AD Connect y se habilita la característica en el inquilino.

![Azure AD Connect: Cambiar inicio de sesión de usuario](./media/how-to-connect-pta-quick-start/changeusersignin.png)

>[!IMPORTANT]
>La autenticación de paso a través es una característica de nivel de inquilino. Su activación afecta al inicio de sesión de los usuarios en _todos_ los dominios administrados del inquilino. Si va a cambiar de Active Directory Federation Services (AD FS) a la autenticación de paso a través, debe esperar al menos doce horas antes de apagar la infraestructura de AD FS. Este tiempo de espera sirve para garantizar que los usuarios mantengan iniciada la sesión en Exchange ActiveSync durante la transición. Para obtener más ayuda sobre la migración de AD FS a la autenticación de paso a través, consulte nuestra guía de implementación detallada publicada [aquí](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx).

## <a name="step-3-test-the-feature"></a>Paso 3: Prueba de la característica

Siga estas instrucciones para verificar que ha habilitado la autenticación de paso a través correctamente:

1. Inicie sesión en el [Centro de administración de Azure Active Directory](https://aad.portal.azure.com) con las credenciales de administrador global del inquilino.
2. Seleccione **Azure Active Directory** en el panel izquierdo.
3. Seleccione **Azure AD Connect**.
4. Verifique que la característica **Autenticación de paso a través** aparece como **Habilitado**.
5. Seleccione **Autenticación de paso a través**. En el panel **Autenticación de paso a través** se enumeran los servidores donde están instalados los agentes de autenticación.

![Centro de administración de Azure Active Directory: panel de Azure AD Connect](./media/how-to-connect-pta-quick-start/pta7.png)

![Centro de administración de Azure Active Directory: panel de Autenticación de paso a través](./media/how-to-connect-pta-quick-start/pta8.png)

En esta fase, los usuarios de todos los dominios administrados del inquilino pueden iniciar sesión con la autenticación de paso a través. Sin embargo, los usuarios de dominios federados siguen iniciando sesión mediante AD FS o cualquier otro proveedor de federación que se haya configurado previamente. Si convierte un dominio de federado a administrado, todos los usuarios del mismo empiezan automáticamente a iniciar sesión mediante la autenticación de paso a través. La característica de autenticación de paso a través no afecta a los usuarios que están solo en la nube.

## <a name="step-4-ensure-high-availability"></a>Paso 4: Garantía de alta disponibilidad

Si tiene previsto implementar la autenticación de paso a través en un entorno de producción, debe instalar un agente de autenticación independiente adicional. Instale los agentes de autenticación en servidores _distintos_ que el servidor en que se ejecuta Azure AD Connect. Esta configuración proporciona alta disponibilidad para las solicitudes de inicio de sesión.

>[!IMPORTANT]
>En entornos de producción, se recomienda tener un mínimo de 3 agentes de autenticación en ejecución en el inquilino. Hay un límite de sistema de 12 agentes de autenticación por inquilino. Y, como procedimiento recomendado, trate todos los servidores que ejecutan los agentes de autenticación como sistemas de nivel 0 (consulte la [referencia](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/securing-privileged-access-reference-material)).

Siga estas instrucciones para descargar el software de agente de autenticación:

1. Para descargar la versión más reciente del agente de autenticación (versión 1.5.193.0 o posterior), inicie sesión en el [Centro de administración de Azure Active Directory](https://aad.portal.azure.com) con las credenciales de administrador global del inquilino.
2. Seleccione **Azure Active Directory** en el panel izquierdo.
3. Seleccione **Azure AD Connect**, **Autenticación de paso a través** y **Descargar agente**.
4. Seleccione el botón **Aceptar las condiciones y descargar**.

![Centro de administración de Azure Active Directory: botón Descargar agente de autenticación](./media/how-to-connect-pta-quick-start/pta9.png)

![Centro de administración de Azure Active Directory: panel Descargar agente](./media/how-to-connect-pta-quick-start/pta10.png)

>[!NOTE]
>También puede descargar [directamente el software del agente de autenticación](https://aka.ms/getauthagent). Revise y acepte las [Condiciones del servicio](https://aka.ms/authagenteula) del agente de autenticación _antes_ de instalarlo.

Hay dos formas de implementar un agente de autenticación independiente:

En primer lugar, puede hacerlo de manera interactiva si ejecuta el archivo ejecutable del agente de autenticación que descargó y proporciona las credenciales de administrador global del inquilino cuando se le solicite hacerlo.

En segundo lugar, puede crear y ejecutar un script de implementación desatendida. Esto resulta útil cuando desea implementar varios agentes de autenticación a la vez o instalar agentes de autenticación en servidores Windows que no tienen habilitada la interfaz de usuario o a los que no puede acceder sin Escritorio remoto. Estas son las instrucciones para usar este enfoque:

1. Ejecute el comando siguiente para instalar un agente de autenticación: `AADConnectAuthAgentSetup.exe REGISTERCONNECTOR="false" /q`.
2. Puede registrar el agente de autenticación con nuestro servicio mediante Windows PowerShell. Cree un objeto de credenciales de PowerShell `$cred` que contenga un nombre de usuario y una contraseña de administrador global para el inquilino. Ejecute el comando siguiente, reemplazando *\<username\>* y *\<password\>* por los valores correspondientes:
   
        $User = "<username>"
        $PlainPassword = '<password>'
        $SecurePassword = $PlainPassword | ConvertTo-SecureString -AsPlainText -Force
        $cred = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $User, $SecurePassword
3. Vaya a **C:\Archivos de programa\Microsoft Azure AD Connect Authentication Agent** y ejecute el script siguiente con el objeto `$cred` que creó:
   
        RegisterConnector.ps1 -modulePath "C:\Program Files\Microsoft Azure AD Connect Authentication Agent\Modules\" -moduleName "AppProxyPSModule" -Authenticationmode Credentials -Usercredentials $cred -Feature PassthroughAuthentication

## <a name="next-steps"></a>Pasos siguientes
- [Migración de AD FS a la autenticación de paso a través](https://github.com/Identity-Deployment-Guides/Identity-Deployment-Guides/blob/master/Authentication/Migrating%20from%20Federated%20Authentication%20to%20Pass-through%20Authentication.docx): una guía detallada para migrar desde AD FS (u cualquier otra tecnología de federación) a la autenticación de paso a través.
- [Bloqueo inteligente](../authentication/howto-password-smart-lockout.md): obtenga información sobre cómo configurar la funcionalidad de bloqueo inteligente en el inquilino para proteger las cuentas de usuario.
- [Limitaciones actuales](how-to-connect-pta-current-limitations.md): conozca qué escenarios son compatibles con la autenticación de paso a través y cuáles no.
- [Profundización técnica](how-to-connect-pta-how-it-works.md): conozca cómo funciona la característica de autenticación de paso a través.
- [Preguntas más frecuentes](how-to-connect-pta-faq.md): encuentre respuestas a las preguntas más frecuentes.
- [Solución de problemas](tshoot-connect-pass-through-authentication.md): obtenga información sobre cómo resolver problemas comunes relacionados con la característica de autenticación de paso a través.
- [Análisis a fondo de la seguridad](how-to-connect-pta-security-deep-dive.md): obtenga información técnica sobre la característica de autenticación de paso a través.
- [SSO de conexión directa de Azure AD](how-to-connect-sso.md): obtenga más información sobre esta característica complementaria.
- [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160611-directory-synchronization-aad-connect): use el foro de Azure Active Directory para solicitar nuevas características.

