---
title: 'Implementación de la protección con contraseña de Azure AD: Azure Active Directory'
description: Implementar la protección con contraseña de Azure AD para prohibir contraseñas incorrectas en el entorno local
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4a3eb121b68311084fd516c6abb7e00ad70eba8b
ms.sourcegitcommit: 390cfe85629171241e9e81869c926fc6768940a4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2020
ms.locfileid: "78226836"
---
# <a name="deploy-azure-ad-password-protection"></a>Implementación de la protección de contraseñas de Azure AD

Ahora que comprende [cómo aplicar la protección con contraseña de Azure AD para Windows Server Active Directory](concept-password-ban-bad-on-premises.md), el paso siguiente consiste en planear y ejecutar la implementación.

## <a name="deployment-strategy"></a>Estrategia de implementación

Le recomendamos que inicie las implementaciones en modo de auditoría. El modo de auditoría es el valor inicial predeterminad, donde se puede seguir estableciendo contraseñas. Las contraseñas que estuvieran bloqueadas se registran en el registro de eventos. Después de implementar los servidores proxy y los agentes de controlador de dominio en el modo de auditoría, debe supervisar el impacto que tendrá la directiva de contraseñas en el entorno y los usuarios cuando se aplique la directiva.

Durante la fase de auditoría, muchas organizaciones concluyen lo siguiente:

* Necesitan mejorar los procesos operativos existentes para usar contraseñas más seguras.
* Los usuarios suelen usan contraseñas no seguras.
* Debe informar a los usuarios sobre el próximo cambio en el cumplimiento de seguridad, el posible impacto para ellos y cómo elegir contraseñas más seguras.

También es posible que la validación de contraseñas más segura afecte a la automatización de la implementación de controladores de dominio de Active Directory existente. Es aconsejable que, al menos, se produzcan una promoción de DC y una degradación de DC durante la evaluación del período de auditoría con el fin de ayudar a descubrir dichos problemas con antelación.  Para más información, consulte:

* [Ntdsutil. exe no puede establecer una contraseña no segura de Modo de reparación de servicios de directorio](howto-password-ban-bad-on-premises-troubleshoot.md#ntdsutilexe-fails-to-set-a-weak-dsrm-password)
* [No se puede realizar la promoción de la réplica del controlador de dominio debido a una contraseña de Modo de reparación de servicios de directorio no segura](howto-password-ban-bad-on-premises-troubleshoot.md#domain-controller-replica-promotion-fails-because-of-a-weak-dsrm-password)
* [No se puede realizar la degradación del controlador de dominio debido a una contraseña no segura del administrador local](howto-password-ban-bad-on-premises-troubleshoot.md#domain-controller-demotion-fails-due-to-a-weak-local-administrator-password)

Después de que la característica se haya ejecutado en modo de auditoría durante un período razonable, puede cambiar la configuración de *Auditoría* a *Exigir* para requerir contraseñas más seguras. Es conveniente enfocar la supervisión durante dicho período.

## <a name="deployment-requirements"></a>Requisitos de implementación

* Los requisitos de licencia para la protección con contraseña de Azure AD se encuentran en el artículo [Eliminación de contraseñas incorrectas en su organización](concept-password-ban-bad.md#license-requirements).
* Todas las máquinas donde se instalará el software del agente de controlador de dominio de Protección con contraseña de Azure AD deben ejecutar Windows Server 2012 o posterior. Este requisito no implica que el dominio o el bosque de Active Directory también debe estar en el nivel funcional del dominio o bosque de Windows Server 2012. Tal como se mencionó en [los principios de diseño](concept-password-ban-bad-on-premises.md#design-principles), no se necesita ningún DFL o FFL mínimo para la ejecución del agente de controlador de dominio o software de proxy.
* Todos los equipos en los que se instala el servicio de agente de controlador de dominio deben tener instalado .NET 4.5.
* Todas las máquinas en las que se instalará el servicio de proxy de Protección con contraseña de Azure AD deben ejecutar Windows Server 2012 R2 o posterior.
   > [!NOTE]
   > La implementación del servicio de proxy es un requisito obligatorio para la implementación de Protección con contraseña de Azure AD, aunque el controlador de dominio pueda tener conectividad saliente directa a Internet. 
   >
* Todos los equipos en los que se instalará donde se instalará el servicio de proxy de protección con contraseña de Azure AD deben tener .NET 4.7 instalado.
  NET 4.7 ya debería estar instalado en un servidor Windows completamente actualizado. Si es necesario, descargue y ejecute el instalador que se encuentra en [ El instalador fuera de línea de .NET Framework 4.7 para Windows ](https://support.microsoft.com/help/3186497/the-net-framework-4-7-offline-installer-for-windows).
* Todos los equipos, incluidos los controladores de dominio, en los que se instalen los componentes de Protección con contraseña de Azure AD deben tener instalado Universal C Runtime. Puede obtener el tiempo de ejecución al asegurarse de que tenga todas las actualizaciones de Windows Update. O bien, puede obtenerlo en un paquete de actualización del sistema operativo específico. Para más información, consulte [Actualización para Universal C RunTime en Windows](https://support.microsoft.com/help/2999226/update-for-uniersal-c-runtime-in-windows).
* Debe existir conectividad de red entre al menos un controlador de dominio de cada dominio y un servidor que hospede el servicio de proxy para la protección con contraseña. Esta conectividad debe permitir que el controlador de dominio acceda al puerto 135 del asignador de puntos de conexión RPC y al puerto del servidor RPC del servicio de proxy. De manera predeterminada, el puerto del servidor RPC es un puerto RPC dinámico, pero se puede configurar para [usar un puerto estático](#static).
* Todas las máquinas donde se instalará el servicio de proxy de Protección con contraseña de Azure AD deben tener acceso de red a los puntos de conexión siguientes:

    |**Punto de conexión**|**Propósito**|
    | --- | --- |
    |`https://login.microsoftonline.com`|Solicitudes de autenticación|
    |`https://enterpriseregistration.windows.net`|Funcionalidad de protección de contraseña de Azure AD|
 
* Requisitos previos de Agent Updater de Microsoft Azure AD Connect

  El servicio de actualización de Agent Updater de Microsoft Azure AD Connect se instala junto con el servicio Proxy de protección de contraseña de Azure AD. Se requiere una configuración adicional para que el servicio de Agent Updater de Microsoft Azure AD Connect pueda funcionar:

  Si su entorno utiliza un servidor proxy http, debe seguir las pautas especificadas en [Trabajar con servidores proxy locales existentes](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-connectors-with-proxy-servers).

  El servicio Agent Updater de Microsoft Azure AD Connect también requiere los pasos de TLS 1.2 especificados en [Requisitos de TLS](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#tls-requirements).

  El acceso a la red debe estar habilitado para el conjunto de puertos y URL especificados en los [procedimientos de configuración del entorno de Proxy de aplicación](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#prepare-your-on-premises-environment).

  > [!WARNING]
  > El proxy de protección de contraseña de Azure AD y el proxy de aplicación instalan diferentes versiones del servicio de Agent Update de Microsoft Azure AD Connect, por lo que las instrucciones se refieren al contenido del proxy de aplicación. Estas versiones diferentes son incompatibles cuando se instalan una al lado de la otra, por lo que no se recomienda instalar el Proxy de protección de contraseña de Azure AD y el Proxy de aplicación en la misma máquina.

* Todas las máquinas que hospedan el servicio de proxy para la protección con contraseña deben estar configuradas para conceder a los controladores de dominio la posibilidad de iniciar sesión en el servicio de proxy. Esta capacidad se controla a través de la asignación del privilegio "Tener acceso a este equipo desde la red".
* Todos los equipos que hospedan el servicio de proxy para la protección con contraseña deben configurarse para permitir el tráfico saliente de HTTP de TLS 1.2.
* Una cuenta de administrador global para registrar el servicio de proxy para la protección con contraseña y el bosque con Azure AD.
* Una cuenta con privilegios de administrador de dominio de Active Directory en el dominio raíz del bosque para registrar el bosque de Windows Server Active Directory en Azure AD.
* Cualquier dominio de Active Directory que ejecute el software del servicio del agente de controlador de dominio debe usar la replicación del Sistema de archivos distribuido (DFSR) para la replicación de sysvol.

  Si el dominio no usa aún DFSR, DEBE migrarlo para que lo utilice antes de instalar la protección con contraseña de Azure AD. Para más información, consulte el vínculo siguiente:

  [Guía de migración de replicación de SYSVOL: Replicación de FRS a DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd640019(v=ws.10))

  > [!WARNING]
  > El software del agente de DC de protección con contraseña de Azure AD se instalará actualmente en los controladores de dominio de los dominios que usan aún FRS (la tecnología predecesora a DFSR) para la replicación de SYSVOL, pero NO funcionará correctamente en este entorno. Otros efectos secundarios negativos incluyen archivos que no se pueden replicar y procedimientos de restauración de SYSVOL que aparentemente funcionan pero que en realidad no pueden replicar todos los archivos. Migre el dominio para usar DFSR lo antes posible, tanto por las ventajas inherentes de DFSR como también para desbloquear la implementación de Protección con contraseña de Azure AD. Las versiones futuras del software se deshabilitarán automáticamente cuando se ejecuten en un dominio que aún use FRS.

* El servicio de distribución de claves debe habilitarse en todos los controladores de dominio del dominio que ejecutan Windows Server 2012. De manera predeterminada, este servicio se habilita a través de inicio de un desencadenador manual.

## <a name="single-forest-deployment"></a>Implementación de bosque único

En el diagrama siguiente se muestra cómo los componentes básicos de la protección con contraseña de Azure AD funcionan conjuntamente en un entorno de Active Directory local.

![Funcionamiento conjunto de los componentes de Protección con contraseña de Azure AD](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

Es una buena idea revisar cómo funciona el software antes de implementarlo. Consulte [Introducción al concepto de la protección con contraseña de Azure AD](concept-password-ban-bad-on-premises.md).

### <a name="download-the-software"></a>Descarga del software

Hay dos instaladores requeridos para la protección con contraseña de Azure AD. Están disponibles en el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=57071).

### <a name="install-and-configure-the-proxy-service-for-password-protection"></a>Instalación y configuración del servicio de proxy para la protección con contraseña

1. Elija uno o varios servidores para hospedar el servicio de proxy para la protección con contraseña.
   * Cada servicio de este tipo solo puede proporcionar directivas de contraseñas para un único bosque. El equipo host debe estar unido a un dominio de ese bosque. Se admiten los dominios raíz y secundarios. Se necesita conectividad de red entre al menos un controlador de dominio en cada dominio del bosque y el equipo con protección con contraseña.
   * Puede ejecutar el servicio de proxy en un controlador de dominio para realizar pruebas. Sin embargo, ese controlador de dominio entonces requiere conectividad a internet, que puede representar un problema de seguridad. Esta configuración es recomendable solo para realizar pruebas.
   * Se recomienda disponer de al menos dos servidores proxy para la redundancia. Consulte [Alta disponibilidad](howto-password-ban-bad-on-premises-deploy.md#high-availability).
   * No se admite la ejecución del servicio de proxy en un controlador de dominio de solo lectura.

1. Instale el servicio de proxy de protección con contraseña de Azure AD mediante el instalador de software `AzureADPasswordProtectionProxySetup.exe`.
   * No es necesario reiniciar para instalar el software. La instalación del software puede automatizarse mediante procedimientos estándares de MSI, por ejemplo:

      `AzureADPasswordProtectionProxySetup.exe /quiet`

      > [!NOTE]
      > Para evitar errores de instalación, el servicio Firewall de Windows debe estar en ejecución antes de instalar el paquete AzureADPasswordProtectionProxySetup.msi. Si la instancia de Windows Firewall está configurada para no ejecutarse, la solución alternativa es habilitar temporalmente el servicio de firewall y ejecutarlo durante la instalación. El software de proxy no tiene ninguna dependencia específica de Firewall de Windows después de la instalación. Si usa un firewall de terceros, aún debe configurarse para satisfacer los requisitos de implementación. Entre ellos se incluye permitir el acceso de entrada en el puerto 135 y el puerto RPC del servidor proxy. Consulte los [requisitos de implementación](howto-password-ban-bad-on-premises-deploy.md#deployment-requirements).

1. Abra una ventana de PowerShell como administrador.
   * El software del proxy de protección con contraseña incluye un nuevo módulo de PowerShell, *AzureADPasswordProtection*. Los pasos siguientes ejecutan distintos cmdlets de este módulo de PowerShell. Importe el módulo nuevo de la siguiente manera:

      ```powershell
      Import-Module AzureADPasswordProtection
      ```

   * Para comprobar que el servicio se está ejecutando, use el siguiente comando de PowerShell:

      `Get-Service AzureADPasswordProtectionProxy | fl`.

     El resultado debería mostrar el **estado** "Running".

1. Registre el proxy.
   * Una vez completado el paso 3, el servicio de proxy se ejecuta en la máquina, pero aún no dispone de las credenciales necesarias para comunicarse con Azure AD. Es necesario el registro con Azure AD:

     `Register-AzureADPasswordProtectionProxy`

     Este cmdlet requiere credenciales de administrador global para el inquilino de Azure. También necesita privilegios de administrador de dominio de Active Directory de forma local en el dominio raíz del bosque. Este cmdlet también se debe ejecutar mediante una cuenta con privilegios de administrador local.

     Cuando este comando se ejecute correctamente una vez para un servicio de proxy, las invocaciones adicionales se realizarán correctamente, pero no son necesarias.

      El cmdlet `Register-AzureADPasswordProtectionProxy` admite los siguientes tres modos de autenticación. Los dos primeros modos son compatibles con Azure Multi-Factor Authentication pero el tercero no. Consulte los comentarios siguientes para obtener más detalles.

     * Modo de autenticación interactiva:

        ```powershell
        Register-AzureADPasswordProtectionProxy -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com'
        ```

        > [!NOTE]
        > Este modo no funcionará en sistemas operativos Server Core. En su lugar, use uno de los siguientes modos de autenticación. Además, es posible que se produzca un error en este modo si se habilita la configuración de seguridad mejorada de Internet Explorer. La solución alternativa consiste en deshabilitar esa configuración, registrar el servidor proxy y luego volver a habilitarla.

     * Modo de autenticación con codificación del dispositivo:

        ```powershell
        Register-AzureADPasswordProtectionProxy -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com' -AuthenticateUsingDeviceCode
        To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XYZABC123 to authenticate.
        ```

        A continuación, puede completar la autenticación al seguir las instrucciones que se indican en un dispositivo diferente.

     * Modo de autenticación silenciosa (basada en contraseña):

        ```powershell
        $globalAdminCredentials = Get-Credential
        Register-AzureADPasswordProtectionProxy -AzureCredential $globalAdminCredentials
        ```

        > [!NOTE]
        > Se produce un error en este modo si se requiere Azure Multi-Factor Authentication para su cuenta. En ese caso, use uno de los dos modos de autenticación anteriores, o use una cuenta diferente que no requiera MFA.
        >
        > Puede que también vea que es necesaria la MFA si el registro de dispositivos de Azure (que usa en segundo plano la protección con contraseña de Azure AD) se ha configurado para requerir MFA de forma global. Para solucionar este problema, puede usar una cuenta diferente que admita MFA con uno de los dos modos de autenticación anteriores, o bien también puede relajar temporalmente el requisito de MFA de registro de dispositivos de Azure. Para ello, vaya al portal de administración de Azure, luego a Azure Active Directory, después a Dispositivos, a Configuración de dispositivo y, por último, establezca la opción "Requerir Multi-factor Authentication para conectar dispositivos" en No. Asegúrese de volver a configurar esta opción en Sí una vez que se haya completado el registro.
        >
        > Se recomienda omitir los requisitos de MFA solo con fines de prueba.

       Actualmente, no se requiere especificar el parámetro *-ForestCredential*, que está reservado para una futura funcionalidad.

   El registro del servicio de proxy para la protección con contraseña solo se debe hacer una vez a lo largo del ciclo de vida del servicio. De aquí en adelante, el servicio de proxy realizará automáticamente todas las demás tareas de mantenimiento necesarias.

   > [!TIP]
   > Puede haber un retraso notable antes de completarse la primera vez que se ejecuta este cmdlet para un inquilino de Azure específico. A menos que se notifica un error, no se preocupe por este retraso.

1. Registre el bosque.
   * Inicialice el bosque local de Active Directory con las credenciales necesarias para comunicarse con Azure mediante el cmdlet `Register-AzureADPasswordProtectionForest` de PowerShell.

      Este cmdlet requiere credenciales de administrador global para el inquilino de Azure.  También debe ejecutar este cmdlet mediante una cuenta con privilegios de administrador local. También requiere privilegios de administrador de empresa de Active Directory local. Este paso se ejecuta una vez por bosque.

      El cmdlet `Register-AzureADPasswordProtectionForest` admite los siguientes tres modos de autenticación. Los dos primeros modos son compatibles con Azure Multi-Factor Authentication pero el tercero no. Consulte los comentarios siguientes para obtener más detalles.

     * Modo de autenticación interactiva:

        ```powershell
        Register-AzureADPasswordProtectionForest -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com'
        ```

        > [!NOTE]
        > Este modo no funcionará en sistemas operativos Server Core. En su lugar, use uno de los dos siguientes modos de autenticación. Además, es posible que se produzca un error en este modo si se habilita la configuración de seguridad mejorada de Internet Explorer. La solución alternativa consiste en deshabilitar esa configuración, registrar el bosque y luego volver a habilitarla.  

     * Modo de autenticación con codificación del dispositivo:

        ```powershell
        Register-AzureADPasswordProtectionForest -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com' -AuthenticateUsingDeviceCode
        To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XYZABC123 to authenticate.
        ```

        A continuación, puede completar la autenticación al seguir las instrucciones que se indican en un dispositivo diferente.

     * Modo de autenticación silenciosa (basada en contraseña):

        ```powershell
        $globalAdminCredentials = Get-Credential
        Register-AzureADPasswordProtectionForest -AzureCredential $globalAdminCredentials
        ```

        > [!NOTE]
        > Se produce un error en este modo si se requiere Azure Multi-Factor Authentication para su cuenta. En ese caso, use uno de los dos modos de autenticación anteriores, o use una cuenta diferente que no requiera MFA.
        >
        > Puede que también vea que es necesaria la MFA si el registro de dispositivos de Azure (que usa en segundo plano la protección con contraseña de Azure AD) se ha configurado para requerir MFA de forma global. Para solucionar este problema, puede usar una cuenta diferente que admita MFA con uno de los dos modos de autenticación anteriores, o bien también puede relajar temporalmente el requisito de MFA de registro de dispositivos de Azure. Para ello, vaya al portal de administración de Azure, luego a Azure Active Directory, después a Dispositivos, a Configuración de dispositivo y, por último, establezca la opción "Requerir Multi-factor Authentication para conectar dispositivos" en No. Asegúrese de volver a configurar esta opción en Sí una vez que se haya completado el registro.
        >
        > Se recomienda omitir los requisitos de MFA solo con fines de prueba.

       Los ejemplos anteriores solo generarán un resultado correcto si el usuario que haya iniciado sesión también es un administrador de dominios de Active Directory para el dominio raíz. De no ser así, puede proporcionar credenciales de dominio alternativas a través del parámetro *-ForestCredential*.

   > [!NOTE]
   > Si hay varios servidores proxy instalados en el entorno, no importa qué servidor proxy use para registrar el bosque.
   >
   > [!TIP]
   > Puede haber un retraso notable antes de completarse la primera vez que se ejecuta este cmdlet para un inquilino de Azure específico. A menos que se notifica un error, no se preocupe por este retraso.

   El registro del bosque de Active Directory es se tiene que hacer una vez durante el ciclo de vida del bosque. Después de eso, los agentes del controlador de dominio del bosque realizarán automáticamente las otras tareas de mantenimiento necesarias. Después de que `Register-AzureADPasswordProtectionForest` se ejecute correctamente para un bosque, las invocaciones adicionales del cmdlet se realizan correctamente, pero no son necesarias.

   Para que `Register-AzureADPasswordProtectionForest` funcione correctamente, debe haber al menos un controlador de dominio que ejecute Windows Server 2012 o una versión posterior disponible en el dominio del servidor proxy. No hace falta instalar el software del agente de controlador en controladores de dominio antes de este paso.

1. Configure el servicio de proxy de protección con contraseña para que se comunique a través de un proxy HTTP.

   Si su entorno requiere el uso de un proxy de HTTP específico para comunicarse con Azure, use este método: Cree un archivo *AzureADPasswordProtectionProxy.exe.config* en la carpeta %Archivos de programa%\Azure AD Password Protection Proxy\Service. Incluya el siguiente contenido:

      ```xml
      <configuration>
        <system.net>
          <defaultProxy enabled="true">
           <proxy bypassonlocal="true"
               proxyaddress="http://yourhttpproxy.com:8080" />
          </defaultProxy>
        </system.net>
      </configuration>
      ```

   Si el proxy de HTTP requiere autenticación, agregue la etiqueta *useDefaultCredentials*:

      ```xml
      <configuration>
        <system.net>
          <defaultProxy enabled="true" useDefaultCredentials="true">
           <proxy bypassonlocal="true"
               proxyaddress="http://yourhttpproxy.com:8080" />
          </defaultProxy>
        </system.net>
      </configuration>
      ```

   En ambos casos, reemplace `http://yourhttpproxy.com:8080` con la dirección y el puerto del servidor proxy HTTP específico.

   Si el proxy HTTP se configuró para usar una directiva de autorización, debe conceder acceso a la cuenta del equipo de Active Directory del equipo que hospeda el servicio de proxy para la protección con contraseña.

   Se recomienda detener y reiniciar el servicio de proxy después de crear o actualizar el archivo *AzureADPasswordProtectionProxy.exe.config*.

   El servicio de proxy no admite el uso de credenciales específicas para conectarse a un servidor proxy de HTTP.

1. Opcional: Configure el servicio de proxy para la protección con contraseña de modo que escuche en un puerto específico.
   * El software del agente de controlador de dominio para la protección con contraseña en los controladores de dominio usa RPC a través de TCP para comunicarse con el servicio de proxy. De manera predeterminada, el servicio de proxy escucha en cualquier punto de conexión RPC dinámico disponible. Puede configurar el servicio de modo que escuche en un puerto TCP específico, si fuera necesario debido a la topología de red o los requisitos de firewall del entorno.
      * <a id="static" /></a>Para configurar el servicio de modo que se ejecute en un puerto estático, use el cmdlet `Set-AzureADPasswordProtectionProxyConfiguration`.

         ```powershell
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort <portnumber>
         ```

         > [!WARNING]
         > Debe detener y reiniciar el servicio para que estos cambios surtan efecto.

      * Para configurar el servicio de modo que se ejecute en un puerto dinámico, aplique el mismo procedimiento, pero vuelva a establecer el valor de *Puerto estático* en cero:

         ```powershell
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort 0
         ```

         > [!WARNING]
         > Debe detener y reiniciar el servicio para que estos cambios surtan efecto.

   > [!NOTE]
   > El servicio de proxy para la protección con contraseña requiere un reinicio manual después de haber realizado cualquier cambio en la configuración del puerto. Sin embargo, no tendrá que reiniciar el software del servicio del agente de controlador de dominio en los controladores de dominio después de realizar estos cambios de configuración.

   * Para consultar la configuración actual del servicio, use el cmdlet `Get-AzureADPasswordProtectionProxyConfiguration`:

      ```powershell
      Get-AzureADPasswordProtectionProxyConfiguration | fl

      ServiceName : AzureADPasswordProtectionProxy
      DisplayName : Azure AD password protection Proxy
      StaticPort  : 0
      ```

### <a name="install-the-dc-agent-service"></a>Instalación del servicio del agente de controlador de dominio

   Instale el servicio del agente de controlador de dominio para la protección con contraseña mediante el paquete `AzureADPasswordProtectionDCAgentSetup.msi`.

   La instalación de software, o la desinstalación, requiere un reinicio. Este requisito se debe a que los archivos DLL de filtro de contraseña solo se cargan o descargan al reiniciar.

   Puede instalar el servicio del agente de controlador de dominio en un equipo que todavía no es un controlador de dominio. En este caso, el servicio se iniciará y ejecutará, pero estará inactivo hasta que el equipo se promueva para que se convierta en un controlador de dominio.

   Puede automatizar la instalación de software mediante procedimientos estándares de MSI. Por ejemplo:

   `msiexec.exe /i AzureADPasswordProtectionDCAgentSetup.msi /quiet /qn /norestart`

   Puede omitir la marca `/norestart` si prefiere que el instalador reinicie automáticamente el equipo.

La instalación se habrá completado después de que se haya instalado el software del agente de controlador de dominio en un controlador de dominio y ese equipo se reinicie. No se requiere ni se permite ninguna otra configuración.

## <a name="upgrading-the-proxy-agent"></a>Actualización del agente proxy

Cuando haya disponible una versión más reciente del software proxy de protección con contraseña de Azure AD, la actualización se realiza mediante la ejecución de la versión más reciente del instalador de software `AzureADPasswordProtectionProxySetup.exe`. La versión más reciente del software está disponible en el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=57071).

No es necesario desinstalar la versión actual del software proxy (el instalador realizará una actualización en contexto). Cuando se actualiza el software proxy no es preciso reiniciar. La actualización del software puede automatizarse mediante procedimientos MSI estándar, por ejemplo: `AzureADPasswordProtectionProxySetup.exe /quiet`.

El agente proxy ahora admite la actualización automática. La actualización automática utiliza el servicio Agent Updater de Microsoft Azure AD Connect, que se instala junto con el servicio de proxy. La actualización automática está activada de forma predeterminada y puede habilitarse o deshabilitarse mediante el cmdlet `Set-AzureADPasswordProtectionProxyConfiguration`. La configuración actual se puede consultar mediante el cmdlet `Get-AzureADPasswordProtectionProxyConfiguration`. Microsoft recomienda que la configuración de actualización automática siempre esté habilitada.

El cmdlet `Get-AzureADPasswordProtectionProxy` se puede usar para consultar la versión del software de todos los agentes proxy instalados actualmente en un bosque.

## <a name="upgrading-the-dc-agent"></a>Actualización del agente de controlador de dominio

Cuando haya disponible una versión más reciente del software agente de controlador de dominio de protección con contraseña de Azure AD, la actualización se realiza mediante la ejecución de la versión más reciente del paquete de software `AzureADPasswordProtectionDCAgentSetup.msi`. La versión más reciente del software está disponible en el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=57071).

No es necesario desinstalar la versión actual del software agente de controlador de dominio (el instalador realizará una actualización en contexto). Al actualizar el software del agente de controlador de dominio siempre es preciso reiniciar el equipo, este requisito es causado por el comportamiento principal de Windows. 

La actualización del software puede automatizarse mediante procedimientos MSI estándar, por ejemplo: `msiexec.exe /i AzureADPasswordProtectionDCAgentSetup.msi /quiet /qn /norestart`.

Puede omitir la marca `/norestart` si prefiere que el instalador reinicie automáticamente el equipo.

El cmdlet `Get-AzureADPasswordProtectionDCAgent` se puede usar para consultar la versión del software de todos los agentes de controlador de dominio instalados actualmente en un bosque.

## <a name="multiple-forest-deployments"></a>Implementaciones en varios bosques

No hay ningún requisito adicional para implementar la protección con contraseña de Azure AD en varios bosques. Cada bosque se configura de forma independiente tal como se describe en la sección "Implementación de bosque único". Cada proxy de protección con contraseña solo puede admitir controladores de dominio del bosque al que está unido. El software de protección con contraseña de cualquier bosque desconoce el software de protección con contraseña que se implementa en otros bosques, independientemente de las configuraciones de confianza de Active Directory.

## <a name="read-only-domain-controllers"></a>Controladores de dominio de solo lectura

Los cambios o establecimientos de contraseña no se procesan ni conserva en los controladores de dominio de solo lectura (RODC). Se reenvíen a controladores de dominio de escritura. Por lo tanto, no hace falta instalar el software del agente de controlador de dominio en el RODC.

No se admite la ejecución del servicio de proxy en un controlador de dominio de solo lectura.

## <a name="high-availability"></a>Alta disponibilidad

El problema de disponibilidad principal para protección con contraseña es la disponibilidad de los servidores proxy cuando los controladores de dominio de un bosque intentan descargar nuevas directivas u otros datos de Azure. Cada agente de controlador de dominio usa un algoritmo de tipo round-robin simple al decidir a qué servidor proxy llamar. El agente omite los servidores proxy que no responden. Para la mayoría de las implementaciones de Active Directory completamente conectadas que tengan una replicación correcta del estado de directorio y carpeta sysvol, dos servidores proxy son suficientes para garantizar la disponibilidad. Esto da como resultado la descarga oportuna de nuevas directivas y otros datos. Sin embargo, se pueden implementar servidores proxy adicionales.

El diseño del software del agente de controlador de dominio mitiga los problemas habituales asociados con la alta disponibilidad. El agente de controlador de dominio mantiene una memoria caché local de la directiva de contraseñas descargada más recientemente. Incluso si ningún servidor proxy se hace disponible, los agentes de controlador de dominio siguen aplicando su directiva de contraseñas en caché. Una frecuencia de actualización razonable de las directivas de contraseñas en una implementación de gran tamaño suele ser días, no horas o menos. Por lo tanto, las interrupciones breves de los servidores proxy no afectan significativamente a la protección con contraseña de Azure AD.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ya instaló los servicios necesarios para la protección con contraseña de Azure AD en los servidores locales, [realice la configuración posterior a la instalación y recopile los datos para la generación informes](howto-password-ban-bad-on-premises-operations.md) con el fin de completar la implementación.

[Conceptual overview of Azure AD password protection](concept-password-ban-bad-on-premises.md) (Introducción al concepto de protección de contraseñas de Azure AD)
