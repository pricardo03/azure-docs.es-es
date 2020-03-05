---
title: Introducción a la habilitación de Azure Monitor para VM (versión preliminar) | Microsoft Docs
description: Aprenda a implementar y configurar Azure Monitor para VM. Más información sobre los requisitos del sistema.
ms.subservice: ''
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 11/14/2019
ms.openlocfilehash: 44422f66f6fc995dcaf96947ea05b183c7131ea3
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2020
ms.locfileid: "77669580"
---
# <a name="enable-azure-monitor-for-vms-preview-overview"></a>Habilitación de Azure Monitor para VM (versión preliminar)

En este artículo se proporciona información general de las opciones disponibles para configurar Azure Monitor para VM. Use Azure Monitor para VM para supervisar el estado y rendimiento. Detecte dependencias de aplicaciones que se ejecutan en máquinas virtuales (VM) de Azure y conjuntos de escalado de máquinas virtuales, máquinas virtuales locales o máquinas virtuales hospedadas en otro entorno de nube.  

Para configurar Azure Monitor para VM:

* Habilite una sola máquina virtual de Azure o conjunto de escalado de máquinas virtuales, seleccionando **Insights (versión preliminar)** directamente desde la misma.
* Habilite dos o más máquinas virtuales de Azure y conjuntos de escalado de máquinas virtuales mediante Azure Policy. Este método asegura que se instalan y configuran correctamente las dependencias necesarias en las máquinas virtuales y conjuntos de escalado nuevos y existentes. Se informa de las máquinas virtuales y conjuntos de escalado no compatibles para que pueda decidir si habilitarlas y cómo corregir las incompatibilidades.
* Use PowerShell para habilitar dos o más máquinas virtuales o conjuntos de escalado de máquinas virtuales de Azure en una suscripción o grupo de recursos concreto.
* Habilite Azure Monitor para VM para supervisar las máquinas virtuales o equipos físicos hospedados en la red corporativa o en otro entorno de nube.

## <a name="prerequisites"></a>Prerrequisitos

Antes de empezar, asegúrese de conocer la información de los apartados siguientes. 

>[!NOTE]
>La siguiente información que se describe en esta sección también se aplica a la [solución Service Map](service-map.md).  

### <a name="log-analytics"></a>Log Analytics

Azure Monitor para VM admite un área de trabajo de Log Analytics en las siguientes regiones:

- Centro-Oeste de EE. UU.
- Oeste de EE. UU.
- Oeste de EE. UU. 2
- Centro-sur de EE. UU.
- Este de EE. UU.
- Este de EE. UU. 2
- Centro de EE. UU.
- Centro-Norte de EE. UU
- Centro de Canadá
- Sur de Reino Unido 2
- Norte de Europa
- Oeste de Europa
- Este de Asia
- Sudeste de Asia
- Centro de la India
- Japón Oriental
- Este de Australia
- Sudeste de Australia

>[!NOTE]
>Puede implementar máquinas virtuales de Azure en cualquier región. Estas máquinas virtuales no se limitan a las regiones admitidas por el área de trabajo de Log Analytics.
>

Si no tiene un área de trabajo, puede crear una usando alguno de los recursos siguientes:
* [La CLI de Azure](../../azure-monitor/learn/quick-create-workspace-cli.md)
* [PowerShell](../../azure-monitor/learn/quick-create-workspace-posh.md)
* [Portal de Azure](../../azure-monitor/learn/quick-create-workspace.md)
* [Azure Resource Manager](../../azure-monitor/platform/template-workspace-configuration.md)

También puede crear un área de trabajo mientras está habilitando la supervisión en una sola máquina virtual de Azure o conjunto de escalado de máquinas virtuales desde Azure Portal.

Para configurar un escenario a escala que use Azure Policy, Azure PowerShell o plantillas de Azure Resource Manager, haga lo siguiente en el área de trabajo de Log Analytics:

* Instale las soluciones ServiceMap e InfrastructureInsights. Puede completar esta instalación mediante el uso de una plantilla proporcionada de Azure Resource Manager. O bien, en la pestaña **Introducción**, seleccione **Configurar área de trabajo**.
* Configure el área de trabajo de Log Analytics para que recopile contadores de rendimiento.

Para configurar el área de trabajo para el escenario a escala, use uno de los métodos siguientes:

* Use [Azure PowerShell](vminsights-enable-at-scale-powershell.md#set-up-a-log-analytics-workspace).
* En la página de la [**cobertura de la directiva**](vminsights-enable-at-scale-policy.md#manage-policy-coverage-feature-overview) de Azure Monitor para VM, seleccione **Configurar área de trabajo**. 

### <a name="supported-operating-systems"></a>Sistemas operativos admitidos

La siguiente es una lista de los sistemas operativos Windows y Linux que son compatibles con Azure Monitor para VM. Más adelante en esta sección, encontrará una lista completa que detalla las versiones de kernel admitidas y las versiones de sistema operativo Linux principales y secundarias.

|Versión del SO |Rendimiento |Mapas |
|-----------|------------|-----|
|Windows Server 2019 | X | X |
|Windows Server 2016 1803 | X | X |
|Windows Server 2016 | X | X |
|Windows Server 2012 R2 | X | X |
|Windows Server 2012 | X | X |
|Windows Server 2008 R2 | X | X|
|Windows 10 1803 | X | X |
|Windows 8.1 | X | X |
|Windows 8 | X | X |
|Windows 7 SP1 | X | X |
|Red Hat Enterprise Linux (RHEL) 6, 7| X | X| 
|Ubuntu 18.04, 16.04 | X | X |
|CentOS Linux 7, 6 | X | X |
|SUSE Linux Enterprise Server (SLES) 12 | X | X |
|Debian 9.4, 8 | X<sup>1</sup> | |

<sup>1</sup> La característica Rendimiento de Azure Monitor para VM solo está disponible desde Azure Monitor. No está disponible directamente desde el panel izquierdo de la máquina virtual de Azure.

>[!NOTE]
>En el sistema operativo Linux:
> - Se admiten solo versiones de kernel SMP Linux y predeterminados.
> - Las versiones de kernel no estándar, como Physical Address Extension (PAE) y Xen, no son compatibles con ninguna distribución de Linux. Por ejemplo, un sistema con la cadena de versión *2.6.16.21-0.8-xen* no es compatible.
> - No se admiten los kernel personalizados, incluidas las recompilaciones de kernels estándar.
> - Se admite el kernel de CentOSPlus.
> - El kernel de Linux debe revisarse para la vulnerabilidad de Spectre. Consulte al proveedor de distribución de Linux para más información.

#### <a name="red-hat-linux-7"></a>Red Hat Linux 7

| Versión del SO | Versión del kernel |
|:--|:--|
| 7.6 | 3.10.0-957 |
| 7.5 | 3.10.0-862 |
| 7.4 | 3.10.0-693 |

#### <a name="red-hat-linux-6"></a>Red Hat Linux 6

| Versión del SO | Versión del kernel |
|:--|:--|
| 6.10 | 2.6.32-754 |
| 6.9 | 2.6.32-696 |

#### <a name="centosplus"></a>CentOSPlus

| Versión del SO | Versión del kernel |
|:--|:--|
| 6.10 | 2.6.32-754.3.5<br>2.6.32-696.30.1 |
| 6.9 | 2.6.32-696.30.1<br>2.6.32-696.18.7 |

#### <a name="ubuntu-server"></a>Ubuntu Server

| Versión del SO | Versión del kernel |
|:--|:--|
| 18,04 | 5.0 (incluye kernel optimizado para Azure)<br>4.18 *<br>4.15* |
| 16.04.3 | 4.15.* |
| 16.04 | 4.13.\*<br>4.11.\*<br>4.10.\*<br>4.8.\*<br>4.4.\* |

#### <a name="suse-linux-12-enterprise-server"></a>SUSE Linux 12 Enterprise Server

| Versión del SO | Versión del kernel |
|:--|:--|
|12 SP4 | 4.12.* (incluye kernel optimizado para Azure) |
|12 SP3 | 4.4.* |
|12 SP2 | 4.4.* |

#### <a name="debian"></a>Debian 

| Versión del SO | Versión del kernel |
|:--|:--|
| 9 | 4,9 | 

### <a name="the-microsoft-dependency-agent"></a>Microsoft Dependency Agent

La característica de asignación de Azure Monitor para VM obtiene sus datos de Microsoft Dependency Agent. Dependency Agent depende del agente de Log Analytics en lo que respecta a sus conexiones a Log Analytics. Por tanto, el sistema debe tener instalado y configurado el agente de Log Analytics con Dependency Agent.

Si habilita Azure Monitor para VM para una sola máquina virtual de Azure o si usa el método de implementación a escala, use la extensión Dependency Agent de Azure VM para [Windows](../../virtual-machines/extensions/agent-dependency-windows.md) o [Linux](../../virtual-machines/extensions/agent-dependency-linux.md), para instalar el agente como parte de la experiencia.

>[!NOTE]
>La siguiente información que se describe en esta sección también se aplica a la [solución Service Map](service-map.md).  

En un entorno híbrido, puede descargar e instalar Dependency Agent manualmente o bien usar un método automatizado.

En la tabla siguiente se describen los orígenes conectados que son compatibles con la característica Asignación en un entorno híbrido.

| Origen conectado | Compatible | Descripción |
|:--|:--|:--|
| Agentes de Windows | Sí | Junto con el [agente de Log Analytics para Windows](../../azure-monitor/platform/log-analytics-agent.md), los agentes de Windows requieren Dependency Agent. Para más información, consulte el artículo sobre los [sistemas operativos compatibles](#supported-operating-systems). |
| Agentes de Linux | Sí | Junto con el [agente de Log Analytics para Linux](../../azure-monitor/platform/log-analytics-agent.md), los agentes de Linux requieren Dependency Agent. Para más información, consulte el artículo sobre los [sistemas operativos compatibles](#supported-operating-systems). |
| Grupo de administración de System Center Operations Manager | Sin | |

Puede descargar Dependency Agent desde estas ubicaciones:

| Archivo | SO | Versión | SHA-256 |
|:--|:--|:--|:--|
| [InstallDependencyAgent-Windows.exe](https://aka.ms/dependencyagentwindows) | Windows | 9.9.2 | 6DFF19B9690E42CA190E3B69137C77904B657FA02895033EAA4C3A6A41DA5C6A |
| [InstallDependencyAgent-Linux64.bin](https://aka.ms/dependencyagentlinux) | Linux | 9.9.1 | 1CB447EF30FC042FE7499A686638F3F9B4F449692FB9D80096820F8024BE4D7C |

## <a name="role-based-access-control"></a>Control de acceso basado en rol

Para habilitar y obtener acceso a las características de Azure Monitor para VM, debe tener el rol *colaborador de Log Analytics*. Para ver los datos de rendimiento, mantenimiento y el mapa, debe tener el rol de *lector de supervisión* en la máquina virtual de Azure. El área de trabajo de Log Analytics debe configurarse para Azure Monitor para VM.

Para más información acerca de cómo controlar el acceso a un área de trabajo de Log Analytics, consulte [Administración de áreas de trabajo](../../azure-monitor/platform/manage-access.md).

## <a name="how-to-enable-azure-monitor-for-vms-preview"></a>Procedimiento para habilitar Azure Monitor para VM (versión preliminar)

Azure Monitor para VM se habilita mediante alguno de los métodos que se describen en esta tabla:

| Estado de la implementación | Método | Descripción |
|------------------|--------|-------------|
| Máquina virtual de Azure única o conjunto de escalado de máquinas virtuales de Azure | [Habilitar desde la máquina virtual](vminsights-enable-single-vm.md) | Puede habilitar una sola máquina virtual de Azure, seleccionando **Insights (versión preliminar)** directamente desde la máquina virtual o el conjunto de escalado de máquinas virtuales. |
| Varias máquinas virtuales de Azure o conjuntos de escalado de máquinas virtuales | [Habilitar a través de Azure Policy](vminsights-enable-at-scale-policy.md) | Puede habilitar varias máquinas virtuales de Azure con Azure Policy y las definiciones de directivas disponibles. |
| Varias máquinas virtuales de Azure o conjuntos de escalado de máquinas virtuales | [Habilitar a través de Azure PowerShell o las plantillas de Azure Resource Manager](vminsights-enable-at-scale-powershell.md) | Use Azure PowerShell o plantillas de Azure Resource Manager para habilitar varias máquinas virtuales o conjuntos de escalado de máquinas virtuales de Azure en una suscripción o grupo de recursos concreto. |
| Nube híbrida | [Habilitar para el entorno híbrido](vminsights-enable-hybrid-cloud.md) | Puede implementar en máquinas virtuales o equipos físicos que se hospedan en su centro de datos u otros entornos de nube. |

## <a name="performance-counters-enabled"></a>Contadores de rendimiento habilitados 

Azure Monitor para VM configura un área de trabajo de Log Analytics para recopilar los contadores de rendimiento que usa. En las tablas siguientes se enumeran los objetos y contadores que se recopilan cada 60 segundos.

>[!NOTE]
>La siguiente lista de contadores de rendimiento habilitada por Azure Monitor para VM no le impide habilitar contadores adicionales que necesita para recopilar datos de las máquinas virtuales que informan al área de trabajo. Además, si deshabilita estos contadores, impedirá que el conjunto de gráficos de rendimiento incluido en la característica Rendimiento muestre el uso de recursos de las máquinas virtuales.

### <a name="windows-performance-counters"></a>Contadores de rendimiento de Windows

|Nombre del objeto |Nombre del contador |
|------------|-------------|
|LogicalDisk |% de espacio libre |
|LogicalDisk |Prom. Segundos de disco/lecturas |
|LogicalDisk |Prom. Segundos de disco/transferencias |
|LogicalDisk |Prom. Segundos de disco/escrituras |
|LogicalDisk |Bytes de disco/s |
|LogicalDisk |Bytes de lectura de disco/s |
|LogicalDisk |Lecturas de disco/s |
|LogicalDisk |Transferencias de disco/s |
|LogicalDisk |Bytes de escritura en disco/s |
|LogicalDisk |Escrituras en disco/s |
|LogicalDisk |Megabytes libres |
|Memoria |MB disponibles |
|Adaptador de red |Bytes recibidos por segundo |
|Adaptador de red |Bytes enviados por segundo |
|Procesador |% de tiempo de procesador |

### <a name="linux-performance-counters"></a>Contadores de rendimiento de Linux

|Nombre del objeto |Nombre del contador |
|------------|-------------|
|Disco lógico |% espacio usado |
|Disco lógico |Bytes de lectura de disco/s |
|Disco lógico |Lecturas de disco/s |
|Disco lógico |Transferencias de disco/s |
|Disco lógico |Bytes de escritura en disco/s |
|Disco lógico |Escrituras en disco/s |
|Disco lógico |Megabytes libres |
|Disco lógico |Bytes de disco lógico/s |
|Memoria |MB de memoria disponibles |
|Red |Número total de bytes recibidos |
|Red |Número total de bytes transmitidos |
|Procesador |% de tiempo de procesador |

## <a name="management-packs"></a>Módulos de administración

Cuando Azure Monitor para VM está habilitado y configurado con un área de trabajo de Log Analytics, se reenvía un módulo de administración a todos los equipos de Windows que informan a esa área de trabajo. Si ha [integrado el grupo de administración de System Center Operations Manager](../../azure-monitor/platform/om-agents.md) con el área de trabajo de Log Analytics, el módulo de administración de Service Map se implementa desde el grupo de administración en los equipos Windows que informan al grupo de administración.  

El módulo de administración se denomina *Microsoft.IntelligencePacks.ApplicationDependencyMonitor*. Se escribe en la carpeta `%Programfiles%\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs\`. El origen de datos que utiliza el módulo de administración es `%Program files%\Microsoft Monitoring Agent\Agent\Health Service State\Resources\<AutoGeneratedID>\Microsoft.EnterpriseManagement.Advisor.ApplicationDependencyMonitorDataSource.dll`.

## <a name="diagnostic-and-usage-data"></a>Datos de diagnóstico y uso

Microsoft recopila automáticamente datos de uso y rendimiento a través del servicio Azure Monitor. Microsoft usa estos datos para mejorar la calidad, la seguridad y la integridad del servicio. 

Para proporcionar funcionalidades de solución de problemas precisas y eficientes, la característica de asignación incluye datos sobre la configuración del software. Los datos proporcionan información como el sistema operativo y versión, la dirección IP, el nombre DNS y el nombre de la estación de trabajo. Microsoft no recopila nombres, direcciones ni otra información de contacto.

Para más información sobre el uso y la recopilación de datos, vea la [Declaración de privacidad de Microsoft Online Services](https://go.microsoft.com/fwlink/?LinkId=512132).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-dsr-and-stp-note.md)]

Ahora que ha habilitado la supervisión para la máquina virtual, la información de supervisión está disponible para su análisis en Azure Monitor para VM.

## <a name="next-steps"></a>Pasos siguientes

Para aprender a usar la característica de supervisión Rendimiento, consulte [Visualización del rendimiento de Azure Monitor para VM](vminsights-performance.md). Para ver las dependencias de las aplicaciones detectadas, consulte [Uso de la asignación de Azure Monitor para VM (versión preliminar) para conocer los componentes de una aplicación](vminsights-maps.md).
