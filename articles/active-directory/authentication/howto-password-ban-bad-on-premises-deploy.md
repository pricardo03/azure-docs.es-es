---
title: Implementar la protección de contraseña de Azure AD - Azure Active Directory
description: Implementar la protección de contraseña de Azure AD para prohibir contraseñas incorrectas en el entorno local
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.collection: M365-identity-device-management
ms.openlocfilehash: f1c24ec49652cfe9105aa66fd1d5e26c81afcd14
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "58904634"
---
# <a name="deploy-azure-ad-password-protection"></a>Implementación de la protección de contraseñas de Azure AD

Ahora que comprende [cómo aplicar la protección de contraseña de Azure AD para Windows Server Active Directory](concept-password-ban-bad-on-premises.md), el paso siguiente consiste en planear y ejecutar la implementación.

## <a name="deployment-strategy"></a>Estrategia de implementación

Le recomendamos que inicie las implementaciones en modo auditoría. Modo auditoría es el valor predeterminado inicial, donde pueden seguir las contraseñas se puede establecer. Las contraseñas que se bloquearía se registran en el registro de eventos. Después de implementar los servidores proxy y los agentes de DC en el modo auditoría, debe supervisar el impacto que tendrá la directiva de contraseñas en el entorno y los usuarios cuando se aplica la directiva.

Durante la fase de auditoría, muchas organizaciones consideran que:

* Necesitan mejorar los procesos operativos existentes para usar contraseñas más seguras.
* Los usuarios normalmente usan las contraseñas no seguras.
* Debe informar a los usuarios sobre el próximo cambio de cumplimiento de seguridad posible impacto en ellos y cómo elegir contraseñas más seguras.

Después de la característica de ejecutarla en modo de auditoría durante un período razonable, puede cambiar la configuración de *auditoría* a *exigir* para requerir contraseñas más seguras. Es conveniente enfocar la supervisión durante dicho período.

## <a name="deployment-requirements"></a>Requisitos de implementación

* Todos los controladores de dominio que obtienen al agente de controlador de dominio de servicio de protección de contraseña de Azure AD instalada debe ejecutar Windows Server 2012 o posterior. Este requisito no implica que el dominio de Active Directory o el bosque también debe estar en el nivel funcional de dominio o bosque de Windows Server 2012. Como se mencionó en [los principios de diseño](concept-password-ban-bad-on-premises.md#design-principles), hay ningún mínimo nivel funcional de dominio o FFL necesarios para que el controlador de dominio proxy o el agente de software ejecutar.
* Todos los equipos que obtienen instalado el servicio de agente de controlador de dominio deben tener instalado .NET 4.5.
* Todos los equipos que obtienen al proxy de servicio de protección de contraseña de Azure AD instalada debe ejecutar Windows Server 2012 R2 o posterior.
* Todas las máquinas donde se instalará el servicio de Proxy de la protección de contraseña de Azure AD deben tener .NET 4.7 instalado.
  .NET 4.7 ya debe instalarse en un servidor de Windows completamente actualizada. Si esto no es el caso, descargue y ejecute el instalador se encuentra en [The .NET Framework 4.7 instalador sin conexión para Windows](https://support.microsoft.com/en-us/help/3186497/the-net-framework-4-7-offline-installer-for-windows).
* Todos los equipos, incluidos los controladores de dominio, que obtienen instalados componentes de protección de contraseña de Azure AD deben tener instalado el Runtime de C universales. Puede obtener el tiempo de ejecución porque nos aseguramos de que tiene todas las actualizaciones desde Windows Update. O bien, puede obtenerlo en un paquete de actualización del sistema operativo específico. Para obtener más información, consulte [actualización de Universal C Runtime en Windows](https://support.microsoft.com/help/2999226/update-for-uniersal-c-runtime-in-windows).
* Conectividad de red debe existir entre al menos un controlador de dominio en cada dominio y al menos un servidor que hospeda el servicio de proxy para la protección con contraseña. Esta conectividad debe permitir que el controlador de dominio tener acceso a puerto de asignador de extremos RPC 135 y el puerto del servidor RPC en el servicio de proxy. De forma predeterminada, el puerto del servidor RPC es un puerto RPC dinámico, pero se puede configurar para [utilizar un puerto estático](#static).
* Todas las máquinas que hospedan el servicio de proxy deben tener acceso a la red a los puntos de conexión siguientes:

    |**Punto de conexión**|**Propósito**|
    | --- | --- |
    |`https://login.microsoftonline.com`|Solicitudes de autenticación|
    |`https://enterpriseregistration.windows.net`|Funcionalidad de protección de contraseña de Azure AD|

* Todas las máquinas que hospedan el servicio de proxy para la protección con contraseña deben configurarse para permitir el tráfico saliente de HTTP de TLS 1.2.
* Una cuenta de administrador Global para registrar el servicio de proxy para la protección con contraseña y el bosque con Azure AD.
* Una cuenta que tenga privilegios de administrador de dominio de Active Directory en el dominio raíz del bosque para registrar el bosque de Windows Server Active Directory con Azure AD.
* Cualquier dominio de Active Directory que se ejecuta el software del servicio del agente de controlador de dominio debe utilizar la replicación del sistema de archivos distribuido (DFSR) para la replicación de sysvol.
* El servicio de distribución de claves debe habilitarse en todos los controladores de dominio en el dominio que ejecutan Windows Server 2012. De forma predeterminada, este servicio se habilita a través de inicio del desencadenador manual.

## <a name="single-forest-deployment"></a>Implementación de bosque único

El diagrama siguiente muestra cómo los componentes básicos de protección mediante contraseña de Azure AD funcionan conjuntamente en un entorno de Active Directory local.

![Funcionamiento conjunto de los componentes de Protección con contraseña de Azure AD](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

Es una buena idea revisar cómo funciona el software antes de implementarla. Consulte [información general Conceptual de protección mediante contraseña de Azure AD](concept-password-ban-bad-on-premises.md).

### <a name="download-the-software"></a>Descarga del software

Hay dos instaladores necesarios para la protección de contraseña de Azure AD. Están disponibles desde el [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=57071).

### <a name="install-and-configure-the-proxy-service-for-password-protection"></a>Instalar y configurar el servicio de proxy para la protección con contraseña

1. Elija uno o más servidores para hospedar el servicio de proxy para la protección con contraseña.
   * Cada servicio de este tipo solo puede proporcionar directivas de contraseñas para un único bosque. El equipo host debe estar unido a un dominio de ese bosque. Se admiten los dominios raíz y secundario. Necesita conectividad de red entre al menos un controlador de dominio en cada dominio del bosque y el equipo de protección de contraseña.
   * Puede ejecutar el servicio de proxy en un controlador de dominio para las pruebas. Pero ese controlador de dominio, a continuación, requiere conectividad a internet, que puede ser un problema de seguridad. Se recomienda esta configuración solo para pruebas.
   * Se recomienda al menos dos servidores proxy para la redundancia. Consulte [alta disponibilidad](howto-password-ban-bad-on-premises-deploy.md#high-availability).

1. Servicio de Proxy de la protección de contraseña de instalar Azure AD mediante el `AzureADPasswordProtectionProxySetup.exe` instalador de software.
   * No es necesario reiniciar para instalar el software. La instalación del software puede automatizarse mediante procedimientos estándares de MSI, por ejemplo:

      `AzureADPasswordProtectionProxySetup.exe /quiet`

      > [!NOTE]
      > El servicio de Firewall de Windows debe estar ejecutándose antes de instalar el paquete AzureADPasswordProtectionProxySetup.msi para evitar un error de instalación. Si Firewall de Windows está configurado para no ejecutar, la solución alternativa es temporalmente, habilitar y ejecutar el servicio de Firewall durante la instalación. El software de proxy no tiene ninguna dependencia específica en Firewall de Windows después de la instalación. Si usa un firewall de terceros, todavía debe configurarse para satisfacer los requisitos de implementación. Estos incluyen lo que permite el acceso de entrada en el puerto 135 y el puerto RPC del servidor proxy. Consulte [los requisitos de implementación](howto-password-ban-bad-on-premises-deploy.md#deployment-requirements).

1. Abra una ventana de PowerShell como administrador.
   * El software de proxy de la protección de contraseña incluye un nuevo módulo de PowerShell, *AzureADPasswordProtection*. Los pasos siguientes ejecutan varios cmdlets de este módulo de PowerShell. Importe el módulo nuevo como sigue:

      ```powershell
      Import-Module AzureADPasswordProtection
      ```

   * Para comprobar que el servicio se está ejecutando, use el siguiente comando de PowerShell:

      `Get-Service AzureADPasswordProtectionProxy | fl`.

     El resultado debería mostrar un **estado** "Running".

1. Registre el proxy.
   * Después de completar el paso 3, el servicio de proxy se está ejecutando en el equipo. Pero el servicio aún no tiene las credenciales necesarias para comunicarse con Azure AD. Es necesario el registro con Azure AD:

     `Register-AzureADPasswordProtectionProxy`

     Este cmdlet requiere credenciales de administrador global para su inquilino de Azure. También necesita privilegios de administrador de dominio de Active Directory de forma local en el dominio raíz del bosque. Después de este comando se ejecuta correctamente una vez para un servicio de proxy, las invocaciones adicionales del mismo, se realizará correctamente pero no son necesarios.

      El `Register-AzureADPasswordProtectionProxy` cmdlet admite los siguientes tres modos de autenticación.

     * Modo de autenticación interactiva:

        ```powershell
        Register-AzureADPasswordProtectionProxy -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com'
        ```

        > [!NOTE]
        > Este modo no funciona en sistemas operativos de Server Core. En su lugar, use uno de los siguientes modos de autenticación. Además, este modo puede producir un error si la configuración de seguridad mejorada de Internet Explorer está habilitada. La solución consiste en deshabilitar esa configuración, registrar al servidor proxy y, a continuación, volver a habilitarla.

     * Modo de autenticación con codificación del dispositivo:

        ```powershell
        Register-AzureADPasswordProtectionProxy -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com' -AuthenticateUsingDeviceCode
        To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XYZABC123 to authenticate.
        ```

        A continuación, completar la autenticación siguiendo las instrucciones que aparecen en un dispositivo diferente.

     * Modo de autenticación silenciosa (basada en contraseña):

        ```powershell
        $globalAdminCredentials = Get-Credential
        Register-AzureADPasswordProtectionProxy -AzureCredential $globalAdminCredentials
        ```

        > [!NOTE]
        > Este modo se produce un error si se requiere autenticación multifactor de Azure. En ese caso, utilice uno de los modos de autenticación en dos anteriores.

       Actualmente no tiene que especificar el *ForestCredential -* parámetro, que está reservado para futuras funcionalidad.

   Registro del servicio de proxy para la protección con contraseña es necesario solamente una vez en la duración del servicio. Después de eso, el servicio de proxy realizará automáticamente cualquier otras tareas de mantenimiento necesarias.

   > [!TIP]
   > Puede haber un retraso notable antes de completarse la primera vez que se ejecuta este cmdlet para un inquilino de Azure específico. A menos que se notifica un error, no se preocupe este retraso.

1. Registre el bosque.
   * Debe inicializar el bosque de Active Directory local con las credenciales necesarias para comunicarse con Azure mediante el `Register-AzureADPasswordProtectionForest` cmdlet de PowerShell. El cmdlet requiere credenciales de administrador global para su inquilino de Azure. También requiere privilegios de administrador de dominio de Active Directory de forma local en el dominio raíz del bosque. Este paso se ejecuta una vez por bosque.

      El `Register-AzureADPasswordProtectionForest` cmdlet admite los siguientes tres modos de autenticación.

     * Modo de autenticación interactiva:

        ```powershell
        Register-AzureADPasswordProtectionForest -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com'
        ```

        > [!NOTE]
        > Este modo no funcionará en sistemas operativos de Server Core. En su lugar, use uno de los siguientes modos de autenticación en dos. Además, este modo puede producir un error si la configuración de seguridad mejorada de Internet Explorer está habilitada. La solución consiste en deshabilitar esa configuración, registrar al servidor proxy y, a continuación, volver a habilitarla.  

     * Modo de autenticación con codificación del dispositivo:

        ```powershell
        Register-AzureADPasswordProtectionForest -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com' -AuthenticateUsingDeviceCode
        To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XYZABC123 to authenticate.
        ```

        A continuación, completar la autenticación siguiendo las instrucciones que aparecen en un dispositivo diferente.

     * Modo de autenticación silenciosa (basada en contraseña):

        ```powershell
        $globalAdminCredentials = Get-Credential
        Register-AzureADPasswordProtectionForest -AzureCredential $globalAdminCredentials
        ```

        > [!NOTE]
        > Este modo se produce un error si se requiere autenticación multifactor de Azure. En ese caso, utilice uno de los modos de autenticación en dos anteriores.

       Estos ejemplos solo se realizará correctamente si el usuario ha iniciado sesión actualmente también es un administrador de dominio de Active Directory para el dominio raíz. Si esto no es así, puede proporcionar las credenciales de dominio alternativo a través de la *ForestCredential -* parámetro.

   > [!NOTE]
   > Si varios servidores proxy están instalados en su entorno, no importa qué servidor proxy se usa para registrar el bosque.
   >
   > [!TIP]
   > Puede haber un retraso notable antes de completarse la primera vez que se ejecuta este cmdlet para un inquilino de Azure específico. A menos que se notifica un error, no se preocupe este retraso.

   Registro del bosque de Active Directory es necesario solamente una vez en la vigencia del bosque. Después de eso, los agentes del controlador de dominio del bosque realizará automáticamente cualquier otras tareas de mantenimiento necesarias. Después de `Register-AzureADPasswordProtectionForest` se ejecuta correctamente para un bosque, las invocaciones adicionales del cmdlet se realice correctamente, pero no son necesarios.

   Para `Register-AzureADPasswordProtectionForest` para tener éxito, al menos un controlador de dominio que ejecuta Windows Server 2012 o posterior debe estar disponible en el dominio del servidor proxy. Pero el software del agente de controlador de dominio no tiene que instalarse en controladores de dominio antes de este paso.

1. Configurar el servicio de proxy para la protección con contraseña para comunicarse a través de un proxy HTTP.

   Si su entorno requiere el uso de un proxy específico de HTTP para comunicarse con Azure, use este método: Crear un *AzureADPasswordProtectionProxy.exe.config* archivo en la carpeta de %ProgramFiles%\Azure AD Proxy\Service de protección de contraseña. Incluya el siguiente contenido:

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

   Si el proxy HTTP requiere autenticación, agregue el *useDefaultCredentials* etiqueta:

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

   En ambos casos, reemplace `http://yourhttpproxy.com:8080` con la dirección y puerto del servidor proxy HTTP específico.

   Si el proxy HTTP está configurado para nosotros una directiva de autorización, debe conceder acceso a la cuenta de equipo de Active Directory de la máquina que hospeda el servicio de proxy para la protección con contraseña.

   Se recomienda detener y reiniciar el servicio de proxy después de crear o actualizar el *AzureADPasswordProtectionProxy.exe.config* archivo.

   El servicio de proxy no admite el uso de credenciales específicas para conectarse a un servidor proxy HTTP.

1. Opcional: Configurar el servicio de proxy para la protección con contraseña para que escuche en un puerto específico.
   * El software del agente de controlador de dominio para la protección de contraseña en los controladores de dominio usa RPC a través de TCP para comunicarse con el servicio de proxy. De forma predeterminada, el servicio de proxy escucha en cualquier punto de conexión RPC dinámico disponible. Pero puede configurar el servicio para que escuche en un puerto TCP específico, si es necesario debido a la topología de red o los requisitos de firewall en su entorno.
      * <a id="static" /></a>Para configurar el servicio para ejecutarse en un puerto estático, use el `Set-AzureADPasswordProtectionProxyConfiguration` cmdlet.

         ```powershell
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort <portnumber>
         ```

         > [!WARNING]
         > Debe detener y reiniciar el servicio para que estos cambios surtan efecto.

      * Para configurar el servicio para ejecutarse en un puerto dinámico, use el mismo procedimiento pero establecer *StaticPort* a cero:

         ```powershell
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort 0
         ```

         > [!WARNING]
         > Debe detener y reiniciar el servicio para que estos cambios surtan efecto.

   > [!NOTE]
   > El servicio de proxy para la protección con contraseña requiere un reinicio manual después de cualquier cambio en la configuración del puerto. Pero no tendrá que reiniciar el software del servicio del agente de controlador de dominio en los controladores de dominio después de realizar estos cambios de configuración.

   * Para consultar la configuración actual del servicio, utilice el `Get-AzureADPasswordProtectionProxyConfiguration` cmdlet:

      ```powershell
      Get-AzureADPasswordProtectionProxyConfiguration | fl

      ServiceName : AzureADPasswordProtectionProxy
      DisplayName : Azure AD password protection Proxy
      StaticPort  : 0
      ```

### <a name="install-the-dc-agent-service"></a>Instalar el servicio del agente de DC

   Instalar el servicio Agente de controlador de dominio para la protección con contraseña mediante el `AzureADPasswordProtectionDCAgentSetup.msi` paquete.

   La instalación de software o la desinstalación, requiere un reinicio. Esto es porque los archivos DLL de filtro de contraseña solo se cargan o descargan a un reinicio.

   Puede instalar el servicio del agente de controlador de dominio en un equipo que todavía no es un controlador de dominio. En este caso, el servicio se inicie y ejecute pero permanecerá inactivo hasta que el equipo pasa a ser un controlador de dominio.

   Puede automatizar la instalación de software mediante procedimientos estándares de MSI. Por ejemplo: 

   `msiexec.exe /i AzureADPasswordProtectionDCAgentSetup.msi /quiet /qn`

   > [!WARNING]
   > El comando de msiexec de ejemplo hace que un reinicio inmediato. Para evitar esto, utilice el `/norestart` marca.

Completada la instalación después de que está instalado el software del agente de controlador de dominio en un controlador de dominio, y ese equipo se reinicia. No se requiere ni se permite ninguna otra configuración.

## <a name="multiple-forest-deployments"></a>Implementaciones en varios bosques

No hay ningún requisito adicional para implementar la protección con contraseña de Azure AD en varios bosques. Cada bosque de forma independiente está configurado como se describe en la sección "implementación de bosque único". Cada proxy de protección de contraseña solo puede admitir los controladores de dominio del bosque que se une a. El software de protección de contraseña en cualquier bosque no es consciente del software de protección de contraseña que se implementa en otros bosques, independientemente de las configuraciones de confianza de Active Directory.

## <a name="read-only-domain-controllers"></a>Controladores de dominio de solo lectura

Contraseña/conjuntos de cambios no se procesan y se conserva en controladores de dominio de solo lectura (RODC). Que se reenvíen a controladores de dominio de escritura. Por lo tanto, no tienes que instalar el software del agente de controlador de dominio en el RODC.

## <a name="high-availability"></a>Alta disponibilidad

El problema de disponibilidad principal para protección con contraseña es la disponibilidad de los servidores proxy cuando los controladores de dominio en un bosque intentan descargar nuevas directivas u otros datos de Azure. Cada agente de controlador de dominio usa un algoritmo round-robin-style simple al decidir qué servidor proxy para llamar a. El agente omite los servidores proxy que no responden. Para las implementaciones de Active Directory más completamente conectadas con la replicación de estado de la carpeta directory y sysvol en buen estado, dos servidores proxy es suficiente para garantizar la disponibilidad. Esto da como resultado oportuna descarga de nuevas directivas y otros datos. Pero se pueden implementar servidores proxy adicionales.

El diseño del software del agente del controlador de dominio mitiga los problemas habituales que se asocian con alta disponibilidad. El agente de DC mantiene una caché local de la directiva de contraseñas descargada más recientemente. Incluso si todos los registrados los servidores proxy que dejan de estar disponibles, los agentes de DC siguen aplicar su directiva de contraseñas en caché. Suele ser una frecuencia de actualización razonable para directivas de contraseña en una implementación grande *días*, no en horas o menos. Por lo tanto, breves interrupciones de los servidores proxy no afectar significativamente a la protección con contraseña Azure AD.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha instalado los servicios que necesita para la protección de contraseña de Azure AD en los servidores locales, [realizar la configuración posterior a la instalación y recopilar información de notificación](howto-password-ban-bad-on-premises-operations.md) para completar la implementación.

[Conceptual overview of Azure AD password protection](concept-password-ban-bad-on-premises.md) (Introducción al concepto de protección de contraseñas de Azure AD)
