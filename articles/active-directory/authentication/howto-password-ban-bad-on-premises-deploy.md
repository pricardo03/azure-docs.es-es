---
title: Implementación de la protección con contraseña de Azure AD (versión preliminar)
description: Implemente la versión preliminar de la protección con contraseña de Azure AD para prohibir contraseñas incorrectas en entornos locales.
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: article
ms.date: 02/01/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: jsimmons
ms.openlocfilehash: 824bedf782d6d227f2fa3adcf52492bb5a3eb478
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2019
ms.locfileid: "55696870"
---
# <a name="preview-deploy-azure-ad-password-protection"></a>Vista previa: Implementación de la protección con contraseña de Azure AD

|     |
| --- |
| La protección con contraseña de Azure AD es una característica en versión preliminar pública de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)|
|     |

Ahora que tenemos una descripción de [cómo aplicar la protección con contraseña de Azure AD para Windows Server Active Directory](concept-password-ban-bad-on-premises.md), el paso siguiente consiste en planear y ejecutar la implementación.

## <a name="deployment-strategy"></a>Estrategia de implementación

Microsoft sugiere que todas las implementaciones se inicien en modo de auditoría. El modo de auditoría es la configuración inicial predeterminada con la que se pueden seguir estableciendo las contraseñas y que permite crear entradas en el registro de eventos de aquellas contraseñas que se bloquean. Una vez que los servidores proxy y los agentes de controladores de dominio están totalmente implementados en el modo de auditoría, es necesario realizar una supervisión regular para determinar qué impacto tendría la aplicación de la directiva de contraseñas en los usuarios y en el entorno.

Durante la fase de auditoría, muchas organizaciones consideran que:

* Necesitan mejorar los procesos operativos existentes para usar contraseñas más seguras.
* Los usuarios están habituados a elegir contraseñas no seguras con regularidad.
* Necesitan informar a los usuarios sobre los futuros cambios en el cumplimiento de la seguridad, el impacto que puede tener en ellos y ayudarlos a entender mejor cómo poder elegir contraseñas más seguras.

Una vez que la característica ha estado en ejecución en modo de auditoría durante un período razonable, la configuración del cumplimiento puede cambiar de **Auditoría** a **Exigir**, lo que puede requerir contraseñas más seguras. Es conveniente enfocar la supervisión durante dicho período.

## <a name="deployment-requirements"></a>Requisitos de implementación

* Todos los controladores de dominio en los que se instalará el servicio de agente de controlador de dominio de protección con contraseña de Azure AD deben ejecutar Windows Server 2012 o posterior.
* Todas las máquinas en las que se instalará el servicio de proxy de protección con contraseña de Azure AD deben ejecutar Windows Server 2012 R2 o posterior.
* Todas las máquinas en las que haya componentes de protección con contraseña de Azure AD instalados, incluidos los controladores de dominio, deben tener instalado Universal C Runtime.
Si es posible, esto se logra mediante la aplicación de revisiones completas a la máquina a través de Windows Update. De lo contrario, es posible que se instale un paquete de actualizaciones específico del sistema operativo: vea [Actualización para Universal C RunTime en Windows ](https://support.microsoft.com/help/2999226/update-for-universal-c-runtime-in-windows).
* Debe existir conectividad de red entre al menos un controlador de dominio de cada dominio y un servidor que hospede el servicio de proxy de protección con contraseña de Azure AD. Esta conectividad debe permitir que el controlador de dominio tenga acceso al puerto del asignador de puntos de conexión RPC (135) y al puerto del servidor RPC del servicio de proxy. De forma predeterminada, el puerto del servidor RPC es un puerto RPC dinámico, pero se puede configurar (ver abajo) para utilizar un puerto estático.
* Todas las máquinas que hospedan el servicio de proxy de protección con contraseña de Azure AD deben tener acceso de red a los puntos de conexión siguientes:

    |Punto de conexión |Propósito|
    | --- | --- |
    |`https://login.microsoftonline.com`|Solicitudes de autenticación|
    |`https://enterpriseregistration.windows.net`|Funcionalidad de protección con contraseña de Azure AD|

* Una cuenta de administrador global para registrar el servicio de proxy de protección con contraseña de Azure AD y el bosque con Azure AD.
* Una cuenta con privilegios de administrador de dominio de Active Directory en el dominio raíz del bosque para registrar el bosque de Windows Server Active Directory con Azure AD.
* Todos los dominios de Active Directory que ejecuten el software del servicio de agente de controlador de dominio deben usar DFSR para la replicación de sysvol.

## <a name="single-forest-deployment"></a>Implementación en un único bosque

En el diagrama siguiente se muestra cómo los componentes básicos de la protección con contraseña de Azure AD funcionan conjuntamente en un entorno de Active Directory local.  

![Funcionamiento conjunto de los componentes de la protección con contraseña de Azure AD](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

### <a name="download-the-software"></a>Descarga del software

Se necesitan dos instaladores para la protección con contraseña de Azure AD que se pueden descargar del [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=57071).

### <a name="install-and-configure-the-azure-ad-password-protection-proxy-service"></a>Instalación y configuración del servicio de proxy de protección con contraseña de Azure AD

1. Elija uno o varios servidores para hospedar el servicio de proxy de protección con contraseña de Azure AD.
   * Cada servicio solo puede proporcionar las directivas de contraseñas para un bosque único, y el equipo host debe estar unido a un dominio (tanto el raíz como el secundario son compatibles) de ese bosque. Para que el servicio de proxy de protección con contraseña de Azure AD cumpla su misión, debe existir conectividad de red entre al menos un controlador de dominio de cada dominio del bosque y la máquina del proxy de protección con contraseña de Azure AD.
   * Es posible instalar y ejecutar el servicio de proxy de protección con contraseña de Azure AD en un controlador de dominio con fines de prueba; la desventaja es que el controlador de dominio requiere conectividad a Internet, lo que puede plantear un problema de seguridad. Microsoft recomienda usar esta configuración solo con fines de prueba.
   * Se recomiendan al menos dos servidores proxy para fines de redundancia. Vea [Alta disponibilidad](howto-password-ban-bad-on-premises-deploy.md#high-availability).

2. Instale el servicio de proxy de protección con contraseña de Azure AD mediante el paquete MSI AzureADPasswordProtectionProxySetup.msi.
   * No es necesario reiniciar para instalar el software. La instalación del software puede automatizarse mediante procedimientos estándares de MSI, por ejemplo: `msiexec.exe /i AzureADPasswordProtectionProxySetup.msi /quiet /qn`.

      > [!NOTE]
      > El servicio Firewall de Windows debe estar en ejecución antes de instalar el paquete MSI AzureADPasswordProtectionProxySetup.msi; de lo contrario, se producirá un error de instalación. Si la instancia de Windows Firewall está configurada para no ejecutarse, la solución alternativa es habilitar temporalmente e iniciar el servicio durante la instalación. El software de proxy no tiene ninguna dependencia específica del software del Firewall de Windows después de la instalación. Si usa un firewall de terceros, debe configurarse para cumplir los requisitos de implementación (permitir el acceso entrante al puerto 135 y definir el puerto del servidor RPC del proxy en dinámico o estático). [Consulte los requisitos de implementación](howto-password-ban-bad-on-premises-deploy.md#deployment-requirements)

3. Abra una ventana de PowerShell como administrador.
   * El software del proxy de protección con contraseña de Azure AD incluye un nuevo módulo de PowerShell denominado AzureADPasswordProtection. Los pasos siguientes se basan en la ejecución de varios cmdlets de este módulo de PowerShell y, en ellos, se supone que ha abierto una nueva ventana de PowerShell y que ha importado el nuevo módulo como sigue:

      ```PowerShell
      Import-Module AzureADPasswordProtection
      ```

   * Compruebe que el servicio se está ejecutando mediante el siguiente comando de PowerShell: `Get-Service AzureADPasswordProtectionProxy | fl`.
     El resultado debe presentar el **estado** que devuelva un resultado "En ejecución".

4. Registre el proxy.
   * Una vez completado el paso 3, el servicio de proxy de protección con contraseña de Azure AD se ejecuta en la máquina, pero aún no dispone de las credenciales necesarias para comunicarse con Azure AD. El registro en Azure AD es necesario para permitir el uso del cmdlet de PowerShell `Register-AzureADPasswordProtectionProxy`. El cmdlet requiere las credenciales de administrador global para el inquilino de Azure, así como privilegios de administrador de dominios de Active Directory a nivel local en el dominio raíz del bosque. Una vez haya realizado todo esto correctamente para un servicio de proxy determinado, las invocaciones adicionales de `Register-AzureADPasswordProtectionProxy` se realizarán correctamente, pero serán innecesarias.

      El cmdlet Register-AzureADPasswordProtectionProxy admite los tres modos de autenticación distintos que se indican a continuación.

      * Modo de autenticación interactiva:

         ```PowerShell
         Register-AzureADPasswordProtectionProxy -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com'
         ```
         > [!NOTE]
         > Este modo no funcionará en sistemas operativos Server Core. En su lugar, use alguno de los modos de autenticación alternativos descritos a continuación.

         > [!NOTE]
         > Es posible que se produzca un error en este modo si se habilita la configuración de seguridad mejorada de Internet Explorer. Para solucionarlo, deshabilite IESC, registre el proxy y vuelva a habilitar IESC.

      * Modo de autenticación con codificación del dispositivo:

         ```PowerShell
         Register-AzureADPasswordProtectionProxy -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com' -AuthenticateUsingDeviceMode
         To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XYZABC123 to authenticate.
         ```

         A continuación, puede completar la autenticación siguiendo las instrucciones indicadas en un dispositivo diferente.

      * Modo de autenticación silenciosa (basada en contraseña):

         ```PowerShell
         $globalAdminCredentials = Get-Credential
         Register-AzureADPasswordProtectionForest -AzureCredential $globalAdminCredentials
         ```

         > [!NOTE]
         > Este modo generará un error si la autenticación requiere MFA por cualquier motivo. En su caso, use alguno de los dos modos anteriores para realizar la autenticación basada en MFA.

      Actualmente, no se requiere especificar el parámetro -ForestCredential, que está reservado para una futura funcionalidad.

   > [!NOTE]
   > Se prevé que el registro del servicio de proxy de protección con contraseña de Azure AD conste de un único paso durante toda la vigencia del servicio. De aquí en adelante, el servicio de proxy realizará automáticamente todas las demás tareas de mantenimiento necesarias. Una vez haya realizado todo esto correctamente para un proxy determinado, las invocaciones adicionales de "Register-AzureADPasswordProtectionProxy" se realizarán correctamente, pero serán innecesarias.

   > [!TIP]
   > Puede haber un retraso considerable (muchos segundos) la primera vez que se ejecuta este cmdlet para un inquilino de Azure determinado antes de que finalice la ejecución del cmdlet. A menos que se notifique un error, este retraso no debería considerarse alarmante.

5. Registre el bosque.
   * El bosque local de Active Directory debe inicializarse con las credenciales necesarias para comunicarse con Azure mediante el cmdlet de PowerShell `Register-AzureADPasswordProtectionForest`. El cmdlet requiere las credenciales de administrador global para el inquilino de Azure, así como privilegios de administrador de dominios de Active Directory a nivel local en el dominio raíz del bosque. Este paso se ejecuta una vez por bosque.

      El cmdlet Register-AzureADPasswordProtectionForest admite los tres modos de autenticación distintos que se indican a continuación.

      * Modo de autenticación interactiva:

         ```PowerShell
         Register-AzureADPasswordProtectionForest -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com'
         ```
         > [!NOTE]
         > Este modo no funcionará en sistemas operativos Server Core. En su lugar, use alguno de los modos de autenticación alternativos descritos a continuación.

         > [!NOTE]
         > Es posible que se produzca un error en este modo si se habilita la configuración de seguridad mejorada de Internet Explorer. Para solucionarlo, deshabilite IESC, registre el proxy y vuelva a habilitar IESC.  

      * Modo de autenticación con codificación del dispositivo:

         ```PowerShell
         Register-AzureADPasswordProtectionForest -AccountUpn 'yourglobaladmin@yourtenant.onmicrosoft.com' -AuthenticateUsingDeviceMode
         To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code XYZABC123 to authenticate.
         ```

         A continuación, puede completar la autenticación siguiendo las instrucciones indicadas en un dispositivo diferente.

      * Modo de autenticación silenciosa (basada en contraseña):
         ```PowerShell
         $globalAdminCredentials = Get-Credential
         Register-AzureADPasswordProtectionForest -AzureCredential $globalAdminCredentials
         ```

         > [!NOTE]
         > Este modo generará un error si la autenticación requiere MFA. En su caso, use alguno de los dos modos anteriores para realizar la autenticación basada en MFA.

      Los ejemplos anteriores solo generarán un resultado correcto si el usuario registrado actualmente también es un administrador de dominios de Active Directory para el dominio raíz. De no ser así, debe proporcionar credenciales de dominio alternativas con el parámetro -ForestCredential.

   > [!NOTE]
   > Si hay varios servidores proxy instalados en el entorno, no importa qué servidor proxy se usa para registrar el bosque.

   > [!TIP]
   > Puede haber un retraso considerable (muchos segundos) la primera vez que se ejecuta este cmdlet para un inquilino de Azure determinado antes de que finalice la ejecución del cmdlet. A menos que se notifique un error, este retraso no debería considerarse alarmante.

   > [!NOTE]
   > Se prevé que el registro del bosque de Active Directory conste de un solo paso durante toda la vigencia del bosque. Los agentes de controladores de dominio que se ejecutan en el bosque realizarán automáticamente todas las demás tareas de mantenimiento necesarias. Una vez haya realizado todo esto correctamente para un bosque determinado, las invocaciones adicionales de `Register-AzureADPasswordProtectionForest` se realizarán correctamente, pero serán innecesarias.

   > [!NOTE]
   > Para que `Register-AzureADPasswordProtectionForest` funcione correctamente debe haber al menos un controlador de dominio de Windows Server 2012 o posterior disponible en el dominio del servidor proxy. Pero no es necesario que el software del agente de controlador de dominio esté instalado en ningún controlador de dominio antes de este paso.

6. Configuración del servicio de proxy de protección con contraseña de Azure AD para comunicarse a través de un proxy HTTP

   Si su entorno requiere el uso de un proxy HTTP específico para comunicarse con Azure, esto puede hacerlo como sigue.

   Cree un archivo llamado `proxyservice.exe.config` en la carpeta `%ProgramFiles%\Azure AD Password Protection Proxy\Service` con el siguiente contenido:

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

   Si el proxy HTTP requiere autenticación, agregue la etiqueta useDefaultCredentials como sigue:

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

   En ambos casos, debería reemplazar `http://yourhttpproxy.com:8080` con la dirección y el puerto del servidor proxy HTTP específico.

   Si el proxy HTTP está configurado con una directiva de autorización, se debe conceder acceso a la cuenta de equipo de Active Directory de la máquina que hospeda el servicio de proxy de protección con contraseña de Azure AD.

   Debe detener y reiniciar el servicio de proxy de protección con contraseña de Azure AD después de crear o actualizar el archivo `proxyservice.exe.config`.

   El servicio de proxy de protección con contraseña de Azure AD no admite el uso de credenciales específicas para conectarse a un proxy HTTP.

7. Opcional: configure el servicio de proxy de protección con contraseña de Azure AD para que escuche en un puerto específico.
   * El software del agente de controlador de dominio de la protección con contraseña de Azure AD usa RPC a través de TCP en los controladores de dominio para comunicarse con el servicio de proxy de protección con contraseña de Azure AD. De forma predeterminada, el servicio de proxy de protección con contraseña de Azure AD escucha en cualquier punto de conexión RPC dinámico disponible. Si es necesario debido a la topología de red o a los requisitos de firewall, en su lugar, se puede configurar el servicio para que escuche en un puerto TCP específico.
      * A fin de configurar el servicio para que se ejecute en un puerto estático, use el cmdlet `Set-AzureADPasswordProtectionProxyConfiguration`.
         ```PowerShell
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort <portnumber>
         ```

         > [!WARNING]
         > Debe detener y reiniciar el servicio para que estos cambios surtan efecto.

      * A fin de configurar el servicio para que se ejecute en un puerto dinámico, aplique el mismo procedimiento, pero vuelva a establecer el puerto estático en cero de la siguiente forma:
         ```PowerShell
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort 0
         ```

         > [!WARNING]
         > Debe detener y reiniciar el servicio para que estos cambios surtan efecto.

   > [!NOTE]
   > El servicio de proxy de protección con contraseña de Azure AD requiere un reinicio manual después de cualquier cambio de configuración del puerto. No es necesario reiniciar el software del servicio de agente de controlador de dominio que se ejecuta en los controladores de dominio después de realizar cambios de configuración de esta naturaleza.

   * Puede consultar la configuración actual del servicio con el cmdlet `Get-AzureADPasswordProtectionProxyConfiguration` como se muestra en el ejemplo siguiente:

      ```PowerShell
      Get-AzureADPasswordProtectionProxyConfiguration | fl

      ServiceName : AzureADPasswordProtectionProxy
      DisplayName : Azure AD password protection Proxy
      StaticPort  : 0
      ```

### <a name="install-the-azure-ad-password-protection-dc-agent-service"></a>Instalación del servicio de agente de controlador de dominio de protección con contraseña de Azure AD

   Instale el software del servicio de agente de controlador de dominio de protección con contraseña de Azure AD con el paquete MSI `AzureADPasswordProtectionDCAgent.msi`.

   Es necesario reiniciar tras la instalación y desinstalación del software, debido al requisito del sistema operativo por el que los archivos dll del filtro de contraseñas solo se cargan o descargan tras un reinicio.

   Es posible instalar el servicio de agente de controlador de dominio en un equipo que aún no sea un controlador de dominio. En este caso, el servicio se iniciará y ejecutará, pero, sin embargo, estará inactivo hasta que se promueva el equipo para que se convierta en un controlador de dominio.

   La instalación del software puede automatizarse mediante procedimientos estándares de MSI, por ejemplo:

   `msiexec.exe /i AzureADPasswordProtectionDCAgent.msi /quiet /qn`

   > [!WARNING]
   > El comando msiexec de ejemplo anterior provocará un reinicio inmediato, pero esto se puede evitar si se especifica la marca `/norestart`.

Una vez instalado y reiniciado el controlador de dominio, se completa también la instalación del software del agente de controlador de dominio de protección con contraseña de Azure AD. No se requiere ni se permite ninguna otra configuración.

## <a name="multiple-forest-deployments"></a>Implementaciones en varios bosques

No hay ningún requisito adicional para implementar la protección con contraseña de Azure AD en varios bosques. Cada bosque se configura de forma independiente tal como se describe en la sección de implementación en un único bosque. Cada proxy de protección con contraseña de Azure AD solo puede admitir controladores de dominio del bosque al que está unido. El software de protección con contraseña de Azure AD de un bosque dado no es consciente del software de protección con contraseña de Azure AD implementado en otro bosque, independientemente de las configuraciones de confianza de Active Directory.

## <a name="read-only-domain-controllers"></a>Controladores de dominio de solo lectura

Los cambios/conjuntos de contraseñas nunca se procesan ni se guardan en controladores de dominio de solo lectura (RODC); en su lugar, se reenvían a controladores de dominio grabables. Por tanto, no es necesario instalar el software del agente de controlador de dominio en RODC.

## <a name="high-availability"></a>Alta disponibilidad

El problema principal al garantizar la alta disponibilidad de la protección con contraseña de Azure AD es la disponibilidad de los servidores proxy, cuando los controladores de dominio de un bosque intentan descargar nuevas directivas u otros datos de Azure. Cada agente de controlador de dominio usa un algoritmo de estilo round robin simple al decidir a qué servidor proxy debe llamar y omite los servidores proxy que no responden. La mayoría de las implementaciones de Active Directory conectadas totalmente con la replicación correcta (tanto con el estado de directorio como SYSVOL), los dos (2) servidores proxy deben ser suficientes para garantizar la disponibilidad y, en consecuencia, las descargas puntuales de nuevas directivas y otros datos. Se pueden implementar servidores proxy adicionales según se estime oportuno.

Los problemas habituales asociados con la alta disponibilidad se mitigan con el diseño del software del agente de controlador de dominio. El agente de controlador de dominio mantiene una caché local de la última directiva de contraseñas descargada. Aunque ningún servidor proxy esté disponible por cualquier motivo, los agentes de controlador de dominio siguen aplicando su directiva de contraseñas en caché. Una frecuencia de actualización razonable de las directivas de contraseñas en una implementación grande suele ser del orden de días, no de horas o menos. Por tanto, las breves interrupciones de los servidores proxy no repercuten significativamente en el funcionamiento de la característica de protección con contraseña de Azure AD ni en sus ventajas de seguridad.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ya ha instalado los servicios necesarios para la protección con contraseña de Azure AD en los servidores locales, lea la [información sobre la configuración y recopilación de informes después de la instalación](howto-password-ban-bad-on-premises-operations.md) para completar la implementación.

[Introducción al concepto de la protección con contraseña de Azure AD](concept-password-ban-bad-on-premises.md)
