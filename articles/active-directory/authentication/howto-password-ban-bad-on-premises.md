---
title: Implementación de la versión preliminar de la protección con contraseña de Azure AD
description: Implemente la versión preliminar de la protección con contraseña de Azure AD para prohibir contraseñas incorrectas en entornos locales
services: active-directory
ms.service: active-directory
ms.component: authentication
ms.topic: conceptual
ms.date: 07/11/2018
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.reviewer: jsimmons
ms.openlocfilehash: 9c0519181ec03394e7d732a8eb608501d6dd6657
ms.sourcegitcommit: 1478591671a0d5f73e75aa3fb1143e59f4b04e6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/19/2018
ms.locfileid: "39161837"
---
# <a name="preview-deploy-azure-ad-password-protection"></a>Versión preliminar: Implementación de la protección con contraseña de Azure AD

|     |
| --- |
| Protección con contraseña y Lista personalizada de contraseñas prohibidas de Azure AD son versiones preliminares públicas de Azure Active Directory. Para más información sobre las versiones preliminares, consulte [Términos de uso complementarios de las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).|
|     |

Ahora que tenemos una descripción de [cómo aplicar la protección con contraseña de Azure AD para Windows Server Active Directory](concept-password-ban-bad-on-premises.md), el paso siguiente consiste en planear y ejecutar la implementación.

## <a name="deployment-strategy"></a>Estrategia de implementación

Microsoft sugiere que todas las implementaciones se inicien en modo de auditoría. El modo de auditoría es la configuración inicial predeterminada con que se pueden seguir estableciendo las contraseñas y que permite crear entradas en el registro de eventos de aquellas contraseñas que se bloquean. Una vez que los servidores proxy y los agentes de controladores de dominio están totalmente implementados en el modo de auditoría, es necesario realizar una supervisión regular para determinar qué impacto tendría la aplicación de la directiva de contraseñas en los usuarios y en el entorno.

Durante la fase de auditoría, muchas organizaciones consideran que:

* Necesitan mejorar los procesos operativos existentes para usar contraseñas más seguras.
* Los usuarios están habituados a elegir contraseñas no seguras con regularidad.
* Necesitan informar a los usuarios sobre los futuros cambios en el cumplimiento de la seguridad, el impacto que puede tener en ellos y ayudarlos a entender mejor cómo poder elegir contraseñas más seguras.

Una vez que la característica ha estado en ejecución en modo de auditoría durante un período razonable, la configuración del cumplimiento puede cambiar de **Auditoría** a **Exigir**, lo que puede requerir contraseñas más seguras. Es conveniente enfocar la supervisión durante dicho período.

## <a name="known-limitation"></a>Limitación conocida

Existe una limitación conocida en la versión preliminar del proxy de protección con contraseña de Azure AD. No se admite el uso de cuentas de administrador de inquilinos que requieran MFA. Una actualización futura del proxy de protección con contraseña de Azure AD admitirá el registro de proxy con cuentas de administrador que requieren MFA.

## <a name="single-forest-deployment"></a>Implementación en un único bosque

La versión preliminar de la protección con contraseña de Azure AD se implementa con el servicio de proxy hasta en dos servidores, y el servicio de agente de controlador de dominio puede implementarse de forma incremental en todos los controladores de dominio en el bosque de Active Directory.

![Funcionamiento conjunto de los componentes de protección con contraseña de Azure AD](./media/concept-password-ban-bad-on-premises/azure-ad-password-protection.png)

### <a name="download-the-software"></a>Descarga del software

Se necesitan dos instaladores de la protección con contraseña de Azure AD que se pueden descargar del [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=57071)

### <a name="install-and-configure-the-azure-ad-password-protection-proxy-service"></a>Instalación y configuración del servicio de proxy de protección con contraseña de Azure AD

1. Elija uno o varios servidores para hospedar el servicio de proxy de protección con contraseña de Azure AD.
   * Cada servicio solo puede proporcionar las directivas de contraseñas para un bosque único, y el equipo host debe estar unido a un dominio (tanto el raíz como el secundario son compatibles) de ese bosque. Para que el servicio de proxy de protección con contraseña de Azure AD cumpla su misión, debe existir conectividad de red entre al menos un controlador de dominio de cada dominio del bosque y el equipo host del proxy de protección con contraseña de Azure AD.
   * Es posible instalar y ejecutar el servicio de proxy de protección con contraseña de Azure AD en un controlador de dominio con fines de prueba pero, después, se requiere conectividad a Internet.

   > [!NOTE]
   > La versión preliminar pública admite un máximo de dos (2) servidores proxy por bosque.

2. Instale el software del servicio de proxy de la directiva de contraseñas mediante el paquete MSI AzureADPasswordProtectionProxy.msi.
   * No es necesario reiniciar para instalar el software. La instalación del software puede automatizarse mediante procedimientos estándares de MSI, por ejemplo: `msiexec.exe /i AzureADPasswordProtectionProxy.msi /quiet /qn`.

3. Abra una ventana de PowerShell como administrador.
   * El software del proxy de protección con contraseña de Azure AD incluye un nuevo módulo de PowerShell denominado AzureADPasswordProtection. Los pasos siguientes se basan en la ejecución de varios cmdlets de este módulo de PowerShell y, en ellos, se supone que ha abierto una nueva ventana de PowerShell y que ha importado el nuevo módulo como sigue:
      * `Import-Module AzureADPasswordProtection`

      > [!NOTE]
      > El software de instalación modifica la variable de entorno PSModulePath del equipo host. Para que este cambio surta efecto, a fin de que se pueda importar y usar el módulo de PowerShell AzureADPasswordProtection, debe abrir una nueva ventana de la consola de PowerShell.

   * Compruebe que el servicio se está ejecutando mediante el siguiente comando de PowerShell: `Get-Service AzureADPasswordProtectionProxy | fl`.
      * El resultado debe presentar el **estado** que devuelva un resultado "En ejecución".

4. Registre el proxy.
   * Una vez completado el paso 3, el servicio de proxy de protección con contraseña de Azure AD se ejecuta en el equipo, pero aún no dispone de las credenciales necesarias para comunicarse con Azure AD. El registro en Azure AD es necesario para permitir el uso del cmdlet de PowerShell `Register-AzureADPasswordProtectionProxy`. El cmdlet requiere las credenciales de administrador global para el inquilino de Azure, así como privilegios de administrador de dominios de Active Directory a nivel local en el dominio raíz del bosque. Una vez haya realizado todo esto correctamente para un servicio de proxy determinado, las invocaciones adicionales de `Register-AzureADPasswordProtectionProxy` se realizarán correctamente, pero serán innecesarias.
      * El cmdlet puede ejecutarse como sigue:

         ```
         $tenantAdminCreds = Get-Credential
         Register-AzureADPasswordProtectionProxy -AzureCredential $tenantAdminCreds
         ```

         El ejemplo solo funciona si el usuario registrado actualmente también es un administrador global de dominios de Active Directory para el dominio raíz. Una alternativa consiste en proporcionar las credenciales de dominio necesarias con el parámetro `-ForestCredential`.

   > [!TIP]
   > Puede haber un retraso considerable (muchos segundos) la primera vez que se ejecuta este cmdlet para un inquilino de Azure determinado antes de que finalice la ejecución del cmdlet. A menos que se notifique un error, este retraso no debería considerarse alarmante.

   > [!NOTE]
   > Se prevé que registro del servicio de proxy de protección con contraseña de Azure AD conste de un único paso durante toda la vigencia del servicio. De aquí en adelante, el servicio de proxy realizará automáticamente todas las demás tareas de mantenimiento necesarias. Una vez haya realizado todo esto correctamente para un bosque determinado, las invocaciones adicionales de "Register-AzureADPasswordProtectionProxy" se realizarán correctamente, pero serán innecesarias.

5. Registre el bosque.
   * El bosque local de Active Directory debe inicializarse con las credenciales necesarias para comunicarse con Azure mediante el cmdlet de PowerShell `Register-AzureADPasswordProtectionForest`. El cmdlet requiere las credenciales de administrador global para el inquilino de Azure, así como privilegios de administrador de dominios de Active Directory a nivel local en el dominio raíz del bosque. Este paso se ejecuta una vez por bosque.
      * El cmdlet puede ejecutarse como sigue:

         ```
         $tenantAdminCreds = Get-Credential
         Register-AzureADPasswordProtectionForest -AzureCredential $tenantAdminCreds
         ```

         El ejemplo solo funciona si el usuario registrado actualmente también es un administrador global de dominios de Active Directory para el dominio raíz. Una alternativa consiste en proporcionar las credenciales de dominio necesarias con el parámetro -ForestCredential.

         > [!NOTE]
         > Si hay varios servidores proxy instalados en su entorno, no importa en qué servidor proxy está especificado en el procedimiento anterior.

         > [!TIP]
         > Puede haber un retraso considerable (muchos segundos) la primera vez que se ejecuta este cmdlet para un inquilino de Azure determinado antes de que finalice la ejecución del cmdlet. A menos que se notifique un error, este retraso no debería considerarse alarmante.

   > [!NOTE]
   > Se prevé que el registro del bosque de Active Directory conste de un solo paso durante toda la vigencia del bosque. Los agentes de controladores de dominio que se ejecutan en el bosque realizarán automáticamente todas las demás tareas de mantenimiento necesarias. Una vez haya realizado todo esto correctamente para un bosque determinado, las invocaciones adicionales de `Register-AzureADPasswordProtectionForest` se realizarán correctamente, pero serán innecesarias.

6. Opcional: Configure el servicio de proxy de protección con contraseña de Azure AD para que escuche en un puerto específico.
   * El software del agente de controlador de dominio de la protección con contraseña de Azure AD usa RPC a través de TCP en los controladores de dominio para comunicarse con el servicio de proxy de protección con contraseña de Azure AD. De forma predeterminada, el servicio de proxy de la directiva de contraseñas de protección con contraseña de Azure AD escucha en cualquier punto de conexión RPC dinámico disponible. Si es necesario debido a la topología de red o a los requisitos de firewall, en su lugar, se puede configurar el servicio para que escuche en un puerto TCP específico.
      * A fin de configurar el servicio para que se ejecute en un puerto estático, use el cmdlet `Set-AzureADPasswordProtectionProxyConfiguration`.
         ```
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort <portnumber>
         ```

         > [!WARNING]
         > Debe detener y reiniciar el servicio para que estos cambios surtan efecto.

      * A fin de configurar el servicio para que se ejecute en un puerto dinámico, aplique el mismo procedimiento, pero vuelva a establecer el puerto estático en cero de la siguiente forma:
         ```
         Set-AzureADPasswordProtectionProxyConfiguration –StaticPort 0
         ```

         > [!WARNING]
         > Debe detener y reiniciar el servicio para que estos cambios surtan efecto.

   > [!NOTE]
   > El servicio de proxy de protección con contraseña de Azure AD requiere un reinicio manual después de cualquier cambio de configuración del puerto. No es necesario reiniciar el software del servicio de agente de controlador de dominio que se ejecuta en los controladores de dominio después de realizar cambios de configuración de esta naturaleza.

   * Puede consultar la configuración actual del servicio con el cmdlet `Get-AzureADPasswordProtectionProxyConfiguration` como se muestra en el ejemplo siguiente:

      ```
      Get-AzureADPasswordProtectionProxyConfiguration | fl

      ServiceName : AzureADPasswordProtectionProxy
      DisplayName : Azure AD password protection Proxy
      StaticPort  : 0 
      ```

### <a name="install-the-azure-ad-password-protection-dc-agent-service"></a>Instalación del servicio de agente de controlador de dominio de protección con contraseña de Azure AD

* Instale el software del servicio de agente de controlador de dominio de protección con contraseña de Azure AD con el paquete MSI `AzureADPasswordProtectionDCAgent.msi`:
   * Es necesario reiniciar tras la instalación y desinstalación del software, debido al requisito del sistema operativo por el que los archivos dll del filtro de contraseñas solo se cargan o descargan tras un reinicio.
   * Es posible instalar el servicio de agente de controlador de dominio en un equipo que aún no sea un controlador de dominio. En este caso, el servicio se iniciará y ejecutará, pero, sin embargo, estará inactivo hasta que se promueva el equipo para que se convierta en un controlador de dominio.

   La instalación del software puede automatizarse mediante procedimientos estándares de MSI, por ejemplo: `msiexec.exe /i AzureADPasswordProtectionDCAgent.msi /quiet /qn`.

   > [!WARNING]
   > El comando msiexec de ejemplo provocará un reinicio inmediato, pero esto se puede evitar si se especifica la marca `/norestart`.

Una vez instalado y reiniciado el controlador de dominio, se completa también la instalación del software del agente de controlador de dominio de protección con contraseña de Azure AD. No se requiere ni se permite ninguna otra configuración.

## <a name="multiple-forest-deployments"></a>Implementaciones en varios bosques

No hay ningún requisito adicional para implementar la protección con contraseña de Azure AD en varios bosques. Cada bosque se configura de forma independiente tal como se describe en la sección de implementación en un único bosque. Cada proxy de protección con contraseña de Azure AD solo puede admitir controladores de dominio del bosque al que está unido. El software de protección con contraseña de Azure AD de un bosque dado no es consciente del software de protección con contraseña de Azure AD implementado en otro bosque, independientemente de las configuraciones de confianza de Active Directory.

## <a name="read-only-domain-controllers"></a>Controladores de dominio de solo lectura

Los cambios/conjuntos de contraseñas nunca se procesan ni se guardan en controladores de dominio de solo lectura (RODC); en su lugar, se reenvían a controladores de dominio grabables. Por tanto, no es necesario instalar el software del agente de controlador de dominio en RODC.

## <a name="high-availability"></a>Alta disponibilidad

El problema principal al garantizar la alta disponibilidad de la protección con contraseña de Azure AD es la disponibilidad de los servidores proxy, cuando los controladores de dominio de un bosque intentan descargar nuevas directivas u otros datos de Azure. La versión preliminar pública admite un máximo de dos servidores proxy por bosque. Cada agente de controlador de dominio usa un algoritmo de estilo round robin simple al decidir a qué servidor proxy debe llamar y omite los servidores proxy que no responden.
Los problemas habituales asociados con la alta disponibilidad se mitigan con el diseño del software del agente de controlador de dominio. El agente de controlador de dominio mantiene una caché local de la última directiva de contraseñas descargada. Aunque ningún servidor proxy esté disponible por cualquier motivo, los agentes de controlador de dominio siguen aplicando su directiva de contraseñas en caché. Una frecuencia de actualización razonable de las directivas de contraseñas en una implementación grande suele ser del orden de días, no de horas o menos.   Por tanto, las breves interrupciones de los servidores proxy no repercuten significativamente en el funcionamiento de la característica de protección con contraseña de Azure AD ni en sus ventajas de seguridad.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ya ha instalado los servicios necesarios para la protección con contraseña de Azure AD en los servidores locales, lea la [información sobre la configuración y recopilación de informes después de la instalación](howto-password-ban-bad-on-premises-operations.md) para completar la implementación.

[Información general conceptual de protección con contraseña de Azure AD](concept-password-ban-bad-on-premises.md)
