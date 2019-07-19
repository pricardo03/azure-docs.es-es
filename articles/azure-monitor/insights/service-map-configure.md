---
title: Configuración de Service Map en Azure | Microsoft Docs
description: Service Map es una solución de Azure que detecta automáticamente los componentes de la aplicación en sistemas Windows y Linux y asigna la comunicación entre servicios. En este artículo se proporciona información para implementar la solución Mapa de servicio en su entorno y utilizarla en distintos escenarios.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: tysonn
ms.assetid: d3d66b45-9874-4aad-9c00-124734944b2e
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/28/2019
ms.author: magoedte
ms.openlocfilehash: 8f2b74c4c091aed0a1b5889b0a07d44d450d1922
ms.sourcegitcommit: f811238c0d732deb1f0892fe7a20a26c993bc4fc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2019
ms.locfileid: "67477077"
---
# <a name="configure-service-map-in-azure"></a>Configuración de Service Map en Azure

Mapa de servicio detecta automáticamente los componentes de la aplicación en sistemas Windows y Linux y asigna la comunicación entre servicios. Puede usarlo para ver los servidores tal como los considera: sistemas interconectados que ofrecen servicios críticos. Service Map muestra las conexiones entre servidores, procesos y puertos en cualquier arquitectura conectada TCP sin una configuración necesaria que sea distinta a la instalación de un agente.

En este artículo se describen los detalles sobre cómo configurar agentes de incorporación y de Mapa de servicio. Para información acerca de cómo usar Service Map, consulte [Uso de la solución Service Map en Azure]( service-map.md).

## <a name="supported-azure-regions"></a>Regiones de Azure compatibles

Service Map actualmente está disponible en las siguientes regiones de Azure:
- Este de EE. UU
- Centro occidental de EE.UU.
- Centro de Canadá
- Sur de Reino Unido 2
- Europa occidental
- Sudeste asiático

## <a name="supported-windows-operating-systems"></a>Sistemas operativos Windows compatibles

En las secciones siguientes se enumeran los sistemas operativos compatibles para Dependency Agent en Windows. 

>[!NOTE]
>Service Map solo es compatible con plataformas de 64 bits.
>

### <a name="windows-server"></a>Windows Server

- Windows Server 2019
- Windows Server 2016 1803
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 SP1

### <a name="windows-desktop"></a>Escritorio de Windows

- Windows 10 1803
- Windows 10
- Windows 8.1
- Windows 8
- Windows 7

## <a name="supported-linux-operating-systems"></a>Sistemas operativos Linux compatibles

En las secciones siguientes se enumeran los sistemas operativos compatibles para Dependency Agent en Linux.  

- Se admiten solo versiones de kernel SMP Linux y predeterminados.
- Las versiones de kernel no estándar, como PAE y Xen, no son compatibles con ninguna distribución de Linux. Por ejemplo, un sistema con la cadena de versión de "2.6.16.21-0.8-xen" no es compatible.
- No se admiten los kernel personalizados, incluidas las recompilaciones de kernels estándar.

### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| Versión del SO | Versión del kernel |
|:--|:--|
| 7.4 | 3.10.0-693 |
| 7.5 | 3.10.0-862 |
| 7.6 | 3.10.0-957 |

### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| Versión del SO | Versión del kernel |
|:--|:--|
| 6.9 | 2.6.32-696 |
| 6.10 | 2.6.32-754 |

### <a name="centosplus"></a>CentOSPlus

| Versión del SO | Versión del kernel |
|:--|:--|
| 6.9 | 2.6.32-696.18.7<br>2.6.32-696.30.1 |
| 6.10 | 2.6.32-696.30.1<br>2.6.32-754.3.5 |

### <a name="ubuntu-server"></a>Ubuntu Server

| Versión del SO | Versión del kernel |
|:--|:--|
| Ubuntu 18.04 | kernel 4.15.\*<br>4.18* |
| Ubuntu 16.04.3 | kernel 4.15.* |
| 16.04 | 4.4.\*<br>4.8.\*<br>4.10.\*<br>4.11.\*<br>4.13.\* |
| 14.04 | 3.13.\*<br>4.4.\* |

#### <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 Enterprise Server

| Versión del SO | Versión del kernel
|:--|:--|
|12 SP2 | 4.4.* |
|12 SP3 | 4.4.* |
|12 SP4 | 4.4.* |
|12 SP4 | Kernel optimizado para Azure |

## <a name="dependency-agent-downloads"></a>Descargas de Dependency Agent

| Archivo | OS | Versión | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.8.1 | 622C99924385CBF539988D759BCFDC9146BB157E7D577C997CDD2674E27E08DD |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.8.1 | 3037934A5D3FB7911D5840A9744AE9F980F87F620A7F7B407F05E276FE7AE4A8 |

## <a name="connected-sources"></a>Orígenes conectados

Service Map obtiene sus datos de Microsoft Dependency Agent. Dependency Agent se basa en el agente de Log Analytics en lo que respecta a sus conexiones a Log Analytics. Es decir, un servidor debe tener instalado y configurado el agente de Log Analytics con Dependency Agent.  En la tabla siguiente se describen los orígenes conectados que son compatibles con la solución Service Map.

| Origen conectado | Compatible | DESCRIPCIÓN |
|:--|:--|:--|
| Agentes de Windows | Sí | Service Map analiza y recopila datos de equipos Windows. <br><br>Además del [agente de Log Analytics para Windows](../../azure-monitor/platform/log-analytics-agent.md), los agentes de Windows requieren Microsoft Dependency Agent. Consulte los sistemas operativos compatibles para obtener una lista completa de las versiones de sistema operativo. |
| Agentes de Linux | Sí | Service Map analiza y recopila datos de equipos Linux. <br><br>Además del [agente de Log Analytics para Linux](../../azure-monitor/platform/log-analytics-agent.md), los agentes de Linux requieren Microsoft Dependency Agent. Consulte los sistemas operativos compatibles para obtener una lista completa de las versiones de sistema operativo. |
| Grupo de administración de System Center Operations Manager | Sí | Service Map analiza y recopila datos de los agentes de Windows y Linux en un [grupo de administración de System Center Operations Manager](../../azure-monitor/platform/om-agents.md) conectado. <br><br>Se requiere una conexión directa desde el equipo agente de System Center Operations Manager a Log Analytics. |
| Cuenta de almacenamiento de Azure | Sin | Service Map recopila datos de equipos agentes, así que no hay ningún dato en él que recopilar de Azure Storage. |

En Windows, tanto System Center Operations Manager como Log Analytics usan Microsoft Monitoring Agent (MMA) para recopilar y enviar los datos de supervisión. (Según el contexto, este agente se denomina agente de System Center Operations Manager, agente de Log Analytics, agente de Log Analytics, MMA o agente directo). System Center Operations Manager y Log Analytics proporcionan versiones integradas diferentes de MMA. Cada una de estas versiones puede informar a System Center Operations Manager, a Log Analytics o a ambos.  

En Linux, el agente de Log Analytics para Linux recopila y envía datos de supervisión a Log Analytics. Puede usar Service Map en servidores con agentes de Log Analytics conectados directamente al servicio o que informan a un grupo de administración de Operations Manager integrado con Log Analytics.  

En este artículo, se hace referencia a todos los agentes, ya sean de Linux o Windows, conectados a un grupo de administración de System Center Operations Manager o directamente a Log Analytics, como el *agente de Log Analytics*. 

El agente de Mapa de servicio no transmite los datos en sí y no requiere ningún cambio en los firewalls o puertos. Los datos de Service Map siempre se transmiten mediante el agente de Log Analytics al servicio Log Analytics, ya sea directamente o a través de la puerta de enlace de Log Analytics.

![Agentes de Service Map](media/service-map-configure/agents.png)

Si es un cliente de System Center Operations Manager con un grupo de administración conectado a Log Analytics:

- Si los agentes de System Center Operations Manager pueden obtener acceso a Internet para conectarse a Log Analytics, no se requiere ninguna configuración adicional.  
- Si los agentes de System Center Operations Manager no tienen acceso a Log Analytics a través de Internet, es necesario configurar la puerta de enlace de Log Analytics para que funcione con System Center Operations Manager.
  
Si los equipos Windows o Linux no pueden conectarse directamente al servicio, deberá configurar el agente de Log Analytics para que se conecte al área de trabajo de Log Analytics mediante la puerta de enlace. Para obtener más información sobre cómo implementar y configurar la puerta de enlace de Log Analytics, vea [Conectar equipos sin acceso a Internet mediante la puerta de enlace de Log Analytics](../../azure-monitor/platform/gateway.md).  

### <a name="management-packs"></a>Módulos de administración

Cuando se activa Service Map en un área de trabajo de Log Analytics, se reenvía un módulo de administración de 300 KB a todos los servidores Windows en esa área de trabajo. Si usa agentes de System Center Operations Manager en un [grupo de administración conectado](../../azure-monitor/platform/om-agents.md), el módulo de administración de Service Map se implementa desde System Center Operations Manager. 

El módulo de administración se denomina Microsoft.IntelligencePacks.ApplicationDependencyMonitor. Se escribe en %Programfiles%\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs\. El origen de datos que el módulo de administración usa es %Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Resources\<AutoGeneratedID>\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll.

## <a name="data-collection"></a>Colección de datos

Puede esperar que cada agente transmita aproximadamente 25 MB por día, dependiendo de la complejidad de las dependencias del sistema. Cada agente envía datos de dependencia de Service Map cada 15 segundos.  

Habitualmente, Dependency Agent consume un 0,1 por ciento de memoria del sistema y un 0,1 por ciento de CPU del sistema.

## <a name="diagnostic-and-usage-data"></a>Datos de diagnóstico y uso

Microsoft recopila automáticamente datos de uso y rendimiento a través del servicio Mapa de servicio. Microsoft usa estos datos para proporcionar calidad, seguridad e integridad en el servicio Service Map y para mejorarlas. Los datos incluyen información sobre la configuración del software, como el sistema operativo y la versión. También incluye dirección IP, nombre DNS y nombre de estación de trabajo para proporcionar funcionalidades precisas y eficaces de solución de problemas. No recopilamos los nombres, las direcciones ni otra información de contacto.

Para más información sobre la recopilación de datos y su utilización, consulte [Declaración de privacidad de Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).

## <a name="installation"></a>Instalación

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

### <a name="azure-vm-extension"></a>Extensión de máquina virtual de Azure

Hay una extensión disponible tanto para Windows (DependencyAgentWindows) como para Linux (DependencyAgentLinux), y puede implementar fácilmente Dependency Agent en las máquinas virtuales de Azure mediante una [extensión de máquina virtual de Azure](https://docs.microsoft.com/azure/virtual-machines/windows/extensions-features).  Con la extensión de máquina virtual de Azure, puede implementar Dependency Agent en las máquinas virtuales Windows y Linux mediante un script de PowerShell o directamente en la máquina virtual usando una plantilla de Azure Resource Manager.  Si implementa el agente mediante la extensión de máquina virtual de Azure, los agentes se actualizan automáticamente a la versión más reciente.

Para implementar la extensión de máquina virtual de Azure mediante PowerShell, puede usar el siguiente ejemplo:

```powershell
#
# Deploy the Dependency agent to every VM in a Resource Group
#

$version = "9.4"
$ExtPublisher = "Microsoft.Azure.Monitoring.DependencyAgent"
$OsExtensionMap = @{ "Windows" = "DependencyAgentWindows"; "Linux" = "DependencyAgentLinux" }
$rmgroup = "<Your Resource Group Here>"

Get-AzVM -ResourceGroupName $rmgroup |
ForEach-Object {
    ""
    $name = $_.Name
    $os = $_.StorageProfile.OsDisk.OsType
    $location = $_.Location
    $vmRmGroup = $_.ResourceGroupName
    "${name}: ${os} (${location})"
    Date -Format o
    $ext = $OsExtensionMap.($os.ToString())
    $result = Set-AzVMExtension -ResourceGroupName $vmRmGroup -VMName $name -Location $location `
    -Publisher $ExtPublisher -ExtensionType $ext -Name "DependencyAgent" -TypeHandlerVersion $version
    $result.IsSuccessStatusCode
}
```

Una manera incluso más fácil de garantizar que Dependency Agent está instalado en las máquinas virtuales consiste en incluir el agente en la plantilla de Azure Resource Manager.  El siguiente ejemplo de código de JSON se puede agregar a la sección *Recursos* de la plantilla.

```JSON
"type": "Microsoft.Compute/virtualMachines/extensions",
"name": "[concat(parameters('vmName'), '/DependencyAgent')]",
"apiVersion": "2017-03-30",
"location": "[resourceGroup().location]",
"dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', parameters('vmName'))]"
],
"properties": {
    "publisher": "Microsoft.Azure.Monitoring.DependencyAgent",
    "type": "DependencyAgentWindows",
    "typeHandlerVersion": "9.4",
    "autoUpgradeMinorVersion": true
}

```

### <a name="install-the-dependency-agent-on-microsoft-windows"></a>Instalación de Dependency Agent en Microsoft Windows

Dependency Agent puede instalarse manualmente en equipos Windows ejecutando `InstallDependencyAgent-Windows.exe`. Si ejecuta este archivo ejecutable sin opciones, se inicia un asistente para instalación que puede seguir para realizar la instalación de forma interactiva.  

>[!NOTE]
>Se requieren privilegios de administrador para instalar o desinstalar al agente.

Utilice los pasos siguientes para instalar Dependency Agent en cada equipo Windows:

1.  Instale el agente de Log Analytics para Windows mediante uno de los métodos descritos en la [información general del agente de Log Analytics](../../azure-monitor/platform/log-analytics-agent.md).
2.  Descargue el agente de Windows y ejecútelo mediante el comando siguiente: 
    
    `InstallDependencyAgent-Windows.exe`

3.  Siga el asistente para instalación para instalar el agente.
4.  Si Dependency Agent no se inicia, compruebe los registros para obtener información detallada del error. En los agentes de Windows, el directorio de registro es %Programfiles%\Microsoft Dependency Agent\logs. 

#### <a name="windows-command-line"></a>Línea de comandos de Windows

Use las opciones de la tabla siguiente para realizar la instalación desde una línea de comandos. Para ver una lista de marcas de instalación, ejecute el programa de instalación con la marca /? de la manera siguiente.

    InstallDependencyAgent-Windows.exe /?

| Marca | DESCRIPCIÓN |
|:--|:--|
| /? | Obtenga una lista de las opciones de la línea de comandos. |
| /S | Realice una instalación silenciosa sin preguntas. |

Los archivos para Windows Dependency Agent se ubican en C:\Archivos de programa\Microsoft Dependency Agent de manera predeterminada.

### <a name="install-the-dependency-agent-on-linux"></a>Instalación de Dependency Agent en Linux

Dependency Agent se instala en equipos Linux desde `InstallDependencyAgent-Linux64.bin`, un script de shell con un archivo binario autoextraíble. Puede ejecutar el archivo mediante `sh` o agregar permisos de ejecución al propio archivo.

>[!NOTE]
> Es necesario el acceso raíz para instalar o configurar el agente.

Utilice los pasos siguientes para instalar Dependency Agent en cada equipo Linux:

1.  Instale el agente de Log Analytics mediante uno de los métodos descritos en la [información general del agente de Log Analytics](../../azure-monitor/platform/log-analytics-agent.md).
2.  Instale Dependency Agent de Linux como raíz ejecutando el comando siguiente:
    
    `sh InstallDependencyAgent-Linux64.bin`

3.  Si Dependency Agent no se inicia, compruebe los registros para obtener información detallada del error. En los agentes de Linux, el directorio de registro es /var/opt/microsoft/dependency-agent/log.

Para ver una lista de marcas de instalación, ejecute el programa de instalación con la marca -help de la manera siguiente.

    InstallDependencyAgent-Linux64.bin -help

| Marca | DESCRIPCIÓN |
|:--|:--|
| -help | Obtenga una lista de las opciones de la línea de comandos. |
| -s | Realice una instalación silenciosa sin preguntas. |
| --check | Compruebe los permisos y el sistema operativo, pero no instale el agente. |

Los archivos de Dependency Agent se colocan en los directorios siguientes:

| Archivos | Location |
|:--|:--|
| Archivos principales | /opt/microsoft/dependency-agent |
| Archivos de registro | /var/opt/microsoft/dependency-agent/log |
| Archivos de configuración | /etc/opt/microsoft/dependency-agent/config |
| Archivos ejecutables de servicio | /opt/microsoft/dependency-agent/bin/microsoft-dependency-agent<br>/opt/microsoft/dependency-agent/bin/microsoft-dependency-agent-manager |
| Archivos binarios de almacenamiento | /var/opt/microsoft/dependency-agent/storage |

## <a name="installation-script-examples"></a>Ejemplos de script de instalación

Para implementar fácilmente Dependency Agent en muchos servidores a la vez, se proporciona el siguiente script de ejemplo para descargar e instalar Dependency Agent en Windows o Linux.

### <a name="powershell-script-for-windows"></a>Script de PowerShell para Windows

```powershell
Invoke-WebRequest "https://aka.ms/dependencyagentwindows" -OutFile InstallDependencyAgent-Windows.exe

.\InstallDependencyAgent-Windows.exe /S
```

### <a name="shell-script-for-linux"></a>Script de shell para Linux

```
wget --content-disposition https://aka.ms/dependencyagentlinux -O InstallDependencyAgent-Linux64.bin
sudo sh InstallDependencyAgent-Linux64.bin -s
```

## <a name="desired-state-configuration"></a>Configuración de estado deseada

Para implementar Dependency Agent mediante Desired State Configuration (DSC), puede usar el módulo xPSDesiredStateConfiguration con el siguiente código de ejemplo:

```powershell
configuration ServiceMap {

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    $DAPackageLocalPath = "C:\InstallDependencyAgent-Windows.exe"

    Node localhost
    {
        # Download and install the Dependency agent
        xRemoteFile DAPackage 
        {
            Uri = "https://aka.ms/dependencyagentwindows"
            DestinationPath = $DAPackageLocalPath
        }

        xPackage DA
        {
            Ensure="Present"
            Name = "Dependency Agent"
            Path = $DAPackageLocalPath
            Arguments = '/S'
            ProductId = ""
            InstalledCheckRegKey = "HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\DependencyAgent"
            InstalledCheckRegValueName = "DisplayName"
            InstalledCheckRegValueData = "Dependency Agent"
            DependsOn = "[xRemoteFile]DAPackage"
        }
    }
}
```

## <a name="remove-the-dependency-agent"></a>Eliminación de Dependency Agent

### <a name="uninstall-agent-on-windows"></a>Desinstalación del agente en Windows

Un administrador puede desinstalar Dependency Agent de Windows a través del Panel de control.

Un administrador también puede ejecutar %Programfiles%\Microsoft Dependency Agent\Uninstall.exe para desinstalar Dependency Agent.

### <a name="uninstall-agent-on-linux"></a>Desinstalación del agente en Linux

Puede desinstalar Dependency Agent de Linux con el siguiente comando.

RHEL, CentOS y Oracle:

```
sudo rpm -e dependency-agent
```

Ubuntu:

```
sudo apt -y purge dependency-agent
```

## <a name="troubleshooting"></a>solución de problemas

Esta sección puede ayudarle si tiene problemas para instalar o ejecutar Service Map. Si sigue sin poder resolver el problema, póngase en contacto con Soporte técnico de Microsoft.

### <a name="dependency-agent-installation-problems"></a>Problemas de instalación de Dependency Agent

#### <a name="installer-prompts-for-a-reboot"></a>El instalador solicita un reinicio

Dependency Agent, *por lo general*, no requiere un reinicio durante su instalación o desinstalación. Sin embargo, en algunos casos poco frecuentes, un servidor de Windows Server requiere un reinicio para continuar con una instalación. Esto ocurre cuando una dependencia, normalmente los redistribuibles de Visual C++ de Microsoft, requiere un reinicio debido a un archivo bloqueado.

#### <a name="message-unable-to-install-dependency-agent-visual-studio-runtime-libraries-failed-to-install-code--codenumber-appears"></a>Aparece el mensaje "Unable to install Dependency agent: Visual Studio Runtime libraries failed to install (code = [code_number])" (No se puede instalar Dependency Agent: error al instalar las bibliotecas en tiempo de ejecución de Visual Studio (código = [número_código]))

Microsoft Dependency Agent se basa en las bibliotecas del entorno de tiempo de ejecución de Microsoft Visual Studio. Recibirá un mensaje si hay algún problema durante la instalación de las bibliotecas. 

Los instaladores de la biblioteca en tiempo de ejecución crean registros en la carpeta %LOCALAPPDATA%\temp. El archivo será dd_vcredist_arch_yyyymmddhhmmss.log, donde *arch* es "x86" o "amd64" y *yyyymmddhhmmss* es la fecha y hora (con formato de 24 horas) en que se creó el registro. El registro proporciona detalles sobre el problema que bloquea la instalación.

Puede ser útil instalar uno mismo las [últimas bibliotecas en entorno de tiempo de ejecución](https://support.microsoft.com/help/2977003/the-latest-supported-visual-c-downloads) primero.

En la tabla siguiente se muestran números de código y resoluciones sugeridas.

| Código | DESCRIPCIÓN | Resolución |
|:--|:--|:--|
| 0 x 17 | El instalador de la biblioteca requiere una actualización de Windows que no se ha instalado. | Mire el registro del instalador de la biblioteca más reciente.<br><br>Si una referencia a "Windows8.1-KB2999226-x64.msu" va seguida de una línea "Error 0x80240017: Failed to execute MSU package" (Error 0x80240017: No se puede ejecutar el paquete MSU), no tiene los requisitos previos para instalar KB2999226. Siga las instrucciones que aparecen en la sección de requisitos previos en [Universal C Runtime en Windows](https://support.microsoft.com/kb/2999226). Es posible que tenga que ejecutar Windows Update y reiniciar varias veces para instalar los requisitos previos.<br><br>Ejecute de nuevo el instalador Microsoft Dependency Agent. |

### <a name="post-installation-issues"></a>Problemas posteriores a la instalación

#### <a name="server-doesnt-appear-in-service-map"></a>El servidor no aparece en Service Map

Si la instalación de Dependency Agent se ha realizado correctamente, pero no ve el servidor en la solución de Service Map:
* ¿Se ha instalado Dependency Agent correctamente? Para validarlo, compruebe si el servicio está instalado y se ejecuta.<br><br>
**Windows**: Busque el servicio llamado "Microsoft Dependency Agent".<br>
**Linux**: Busque el proceso en ejecución "microsoft-dependency-agent".

* ¿Se encuentra en el [plan de tarifa Gratis de Operations Management Suite/Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions)? El plan gratuito permite hasta cinco servidores de Service Map (Mapa de servicios) únicos. Ningún servidor posterior aparecerá en Service Map (Mapa de servicios), incluso si los cinco anteriores ya no envían datos.

* ¿Está enviando su servidor datos de registro y rendimiento a Log Analytics? Vaya a la búsqueda de registros y ejecute la siguiente consulta para el equipo: 

    Usage | where Computer == "computer-name" | summarize sum(Quantity), any(QuantityUnit) by DataType

¿obtuvo una amplia variedad de eventos en los resultados? ¿Son los datos recientes? Si es así, el agente de Log Analytics funciona correctamente y se comunica con el servicio de Log Analytics. Si no es así, compruebe el agente en el servidor: [Log Analytics agent for Windows troubleshooting](https://support.microsoft.com/help/3126513/how-to-troubleshoot-monitoring-onboarding-issues) (Cómo solucionar problemas relacionados con el agente de Windows de Log Analytics) o [Cómo solucionar problemas relacionados con el agente de Linux de Log Analytics](../../azure-monitor/platform/agent-linux-troubleshoot.md).

#### <a name="server-appears-in-service-map-but-has-no-processes"></a>El servidor aparece en Service Map, pero no tiene procesos

Si ve el servidor en Service Map (Mapa de servicios), pero no tiene datos de proceso ni de conexión, quiere decir que Dependency Agent está instalado y se ejecuta. Sin embargo, el controlador kernel no se ha cargado. 

Revise el archivo C:\Program Files\Microsoft Dependency Agent\logs\wrapper.log (Windows) o el archivo /var/opt/microsoft/dependency-agent/log/service.log (Linux). Las últimas líneas del archivo deben indicar por qué no se cargó el kernel. Por ejemplo, es posible que el kernel no sea compatible con Linux si actualizó el kernel.

## <a name="next-steps"></a>Pasos siguientes

- Obtenga información sobre cómo [usar Service Map]( service-map.md) una vez implementado y configurado.