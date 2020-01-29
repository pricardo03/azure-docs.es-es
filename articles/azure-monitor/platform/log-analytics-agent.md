---
title: Recopilación de datos de registro con el agente de Azure Log Analytics | Microsoft Docs
description: En este tema se le ayudará a comprender cómo recopilar datos y supervisar equipos hospedados en Azure, localmente o en entornos en la nube con Log Analytics.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 12/24/2019
ms.openlocfilehash: 8e563ae095cf39cdce3e671d4099d2bf1592100a
ms.sourcegitcommit: 38b11501526a7997cfe1c7980d57e772b1f3169b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2020
ms.locfileid: "76513633"
---
# <a name="collect-log-data-with-the-log-analytics-agent"></a>Recopilación de datos de registro con el agente de Log Analytics

El agente de Azure Log Analytics, anteriormente conocido como Microsoft Monitoring Agent (MMA) o agente Linux de OMS, se desarrolló para lograr una administración completa en las máquinas locales, en los equipos que supervisaba [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/) y en las máquinas virtuales de cualquier nube. Los agentes de Windows y Linux se asocian a Azure Monitor y almacenan los datos de registro recopilados de diferentes orígenes en el área de trabajo de Log Analytics, así como cualquier registro o métrica únicos, tal como se define en una solución de supervisión. 

En este artículo se proporciona una descripción detallada del agente, de los requisitos del sistema y de la red, y de los diferentes métodos de implementación.

## <a name="overview"></a>Información general

![Diagrama de comunicación del agente de Log Analytics](./media/log-analytics-agent/log-analytics-agent-01.png)

Antes de analizar datos recopilados y actuar sobre ellos, deberá instalar y conectar agentes en todas las máquinas que quiera que puedan enviar datos al servicio Azure Monitor. Puede instalar agentes en las máquinas virtuales de Azure mediante la extensión de máquinas virtuales de Azure Log Analytics para Windows y Linux, y para las máquinas de un entorno híbrido mediante configuración, la línea de comandos o con Desired State Configuration (DSC) en Azure Automation. 

El agente para Linux y Windows se comunica con el servicio Azure Monitor en el puerto TCP 443. Si la máquina se conecta mediante un servidor proxy o firewall para comunicarse a través de Internet, consulte los requisitos siguientes para comprender qué configuración de red debe aplicarse. Si las directivas de seguridad de TI no permiten que los equipos de la red se conecten a Internet, puede configurar una [puerta de enlace de Log Analytics](gateway.md) y, luego, configurar el agente para conectarse a registros de Azure Monitor a través de la puerta de enlace. El agente, a continuación, puede recibir información de configuración y enviar los datos recopilados según qué reglas de recopilación de datos y soluciones de supervisión haya habilitado en el área de trabajo. 

Si se usan los agentes de Log Analytics para recopilar datos, es preciso conocer lo siguiente para planear la implementación de los agentes:

* Para recopilar datos de los agentes de Windows, puede [configurar cada agente para que informe a una o varias áreas de trabajo](agent-windows.md), aun cuando informen a un grupo de administración de System Center Operations Manager. El agente de Windows puede notificar hasta cuatro áreas de trabajo.
* El agente de Linux no admite el hospedaje múltiple y solo puede informar a un área de trabajo.
* El agente de Windows admite el [estándar FIPS 140](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation), mientras que el agente de Linux no lo admite.  

Si usa System Center Operations Manager 2012 R2, o cualquier versión posterior:

* Cada grupo de administración de Operations Manager se puede [conectar a una sola área de trabajo](om-agents.md).
* Los equipos Linux que informan a un grupo de administración deben estar configurados para informar directamente a un área de trabajo Log Analytics. Si los equipos Linux ya están notificando directamente a un área de trabajo y desea supervisarlos con Operations Manager, siga estos para [notificar a un grupo de administración de Operations Manager](agent-manage.md#configure-agent-to-report-to-an-operations-manager-management-group).
* El agente de Windows de Log Analytics se puede instalar en el equipo Windows y se le puede indicar que informe tanto a Operations Manager integrado con un área de trabajo como a otra área de trabajo.

El agente para Windows y Linux no es solo para conectarse a Azure Monitor, sino que también admite Azure Automation para hospedar el rol de trabajo Hybrid Runbook Worker y otros servicios como [Change Tracking](../../automation/change-tracking.md), [Update Management](../../automation/automation-update-management.md) y [Azure Security Center](../../security-center/security-center-intro.md). Para obtener más información acerca de la función Hybrid Runbook Worker, consulte [Hybrid Runbook Worker de Azure Automation](../../automation/automation-hybrid-runbook-worker.md).  

## <a name="supported-windows-operating-systems"></a>Sistemas operativos Windows compatibles

Las siguientes versiones del sistema operativo Windows son compatibles oficialmente con el agente de Windows:

* Windows Server 2019
* Windows Server 2008 SP2 (x64), 2008 R2, 2012, 2012 R2, 2016, versión 1709 y 1803
* Windows 7 SP1, Windows 8 Enterprise y Pro, y Windows 10 Enterprise y Pro

>[!NOTE]
>Aunque el agente de Log Analytics para Windows se diseñó para admitir escenarios de supervisión de servidor, se puede ejecutar el cliente de Windows para admitir las cargas de trabajo configuradas y optimizadas para el sistema operativo del servidor. El agente es compatible con el cliente de Windows, pero nuestras soluciones de supervisión no se centran en escenarios de supervisión de cliente, salvo se indique explícitamente.

## <a name="supported-linux-operating-systems"></a>Sistemas operativos Linux compatibles

En esta sección se ofrecen detalles sobre las distribuciones de Linux compatibles.

A partir de las versiones publicadas después de agosto de 2018, hemos realizado los siguientes cambios en nuestro modelo de soporte técnico:  

* Solo se admiten las versiones de servidor, no de cliente.  
* Siempre se admiten las nuevas versiones de [distribuciones aprobadas para Linux de Azure](../../virtual-machines/linux/endorsed-distros.md).  
* Se admiten todas las versiones secundarias de cada versión principal de la lista.
* No se admiten las versiones que han superado la fecha de finalización de soporte técnico de su fabricante.  
* No se admiten las nuevas versiones de AMI.  
* Solo se admiten las versiones que ejecutan SSL 1.x de forma predeterminada.

>[!NOTE]
>Si utiliza una distribución o versión que no se admite actualmente y no se alinea con nuestro modelo de soporte técnico, se recomienda bifurcar este repositorio, confirmar que el soporte técnico de Microsoft no prestará asistencia con versiones de agente con bifurcación.

* Amazon Linux 2017.09 (x64)
* CentOS Linux 6 (x86/x64) y 7 (x64)  
* Oracle Linux 6 y 7 (x86/x64) 
* Red Hat Enterprise Linux Server 6 (x86/x64) y 7 (x64)
* Debian GNU/Linux 8 y 9 (x86/x64)
* Ubuntu 14.04 LTS (x86/x64), 16.04 LTS (x86/x64) y 18.04 LTS (x64)
* SUSE Linux Enterprise Server 12 (x64) y 15 (x64)

>[!NOTE]
>OpenSSL 1.1.0 solo se admite en las plataformas x86_x64 (64 bits) y OpenSSL anterior a 1.x no se admite en ninguna plataforma.
>

### <a name="agent-prerequisites"></a>Requisitos del agente

En la tabla siguiente se indican los paquetes necesarios para las distribuciones de Linux compatibles en las que se instalará el agente.

|Paquete necesario |Descripción |Versión mínima |
|-----------------|------------|----------------|
|Glibc |    Biblioteca GNU C | 2.5-12 
|Openssl    | Bibliotecas OpenSSL | 1.0.x o 1.1.x |
|Curl | Cliente web de cURL | 7.15.5 |
|Python ctypes | | 
|PAM | Módulos de autenticación conectables | | 

>[!NOTE]
>Rsyslog o Syslog son necesarios para recopilar mensajes de Syslog. El demonio predeterminado de Syslog en la versión 5 de Red Hat Enterprise Linux, CentOS y Oracle Linux (Sysklog) no se admite para la recopilación de eventos de Syslog. Para recopilar datos de Syslog de esta versión de estas distribuciones, es necesario instalar configurar el demonio de Rsyslog para reemplazar Sysklog.

## <a name="tls-12-protocol"></a>Protocolo TLS 1.2

Para garantizar la seguridad de los datos en tránsito a registros de Azure Monitor, se recomienda encarecidamente configurar el agente para que use al menos Seguridad de la capa de transporte (TLS) 1.2. Las versiones anteriores de TLS/Capa de sockets seguros (SSL) han demostrado ser vulnerables y, si bien todavía funcionan para permitir la compatibilidad con versiones anteriores, **no se recomiendan**.  Para información adicional, revise [Sending data securely using TLS 1.2](data-security.md#sending-data-securely-using-tls-12) (Envío de datos de forma segura mediante TLS 1.2). 

## <a name="network-firewall-requirements"></a>Requisitos de firewall de red

A continuación se muestra la información de configuración de proxy y firewall requerida para que el agente de Windows y Linux se comunique con los registros de Azure Monitor.  

|Recurso del agente|Puertos |Dirección |Omitir inspección de HTTPS|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.com |Puerto 443 |Salida|Sí |  
|*.oms.opinsights.azure.com |Puerto 443 |Salida|Sí |  
|*.blob.core.windows.net |Puerto 443 |Salida|Sí |  

Para obtener información sobre el firewall necesaria para Azure Government, vea [Administración de Azure Government](../../azure-government/documentation-government-services-monitoringandmanagement.md#azure-monitor-logs). 

Si tiene previsto usar Hybrid Runbook Worker de Azure Automation para conectarse al servicio Automation y registrarse en él para usar runbooks o soluciones de administración en el entorno, debe tener acceso al número de puerto y las direcciones URL descritos en [Configuración de la red para Hybrid Runbook Worker](../../automation/automation-hybrid-runbook-worker.md#network-planning). 

El agente de Linux y Windows admite la comunicación a través de un servidor proxy o la puerta de enlace de Log Analytics con Azure Monitor mediante el protocolo HTTPS.  Se admite la autenticación anónima y básica (nombre de usuario/contraseña).  Si el agente de Windows está conectado directamente al servicio, la configuración de proxy se especifica durante la instalación o [después de la implementación](agent-manage.md#update-proxy-settings) desde el Panel de control o con PowerShell.  

Con el agente de Linux, el servidor proxy se especifica durante o [después de la instalación](agent-manage.md#update-proxy-settings) mediante la modificación del archivo de configuración proxy.conf.  El valor de configuración del proxy del agente de Linux tiene la siguiente sintaxis:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Aunque el servidor proxy no requiera que se autentique, el agente de Linux sí que requerirá que proporcione un pseudousuario y contraseña. Puede ser cualquier nombre de usuario o contraseña.

|Propiedad| Descripción |
|--------|-------------|
|Protocolo | https |
|usuario | Nombre de usuario opcional para la autenticación de proxy |
|password | Contraseña opcional para la autenticación de proxy |
|proxyhost | Dirección o nombre de dominio completo (FQDN) del servidor proxy o la puerta de enlace de Log Analytics |
|port | Número de puerto opcional para el servidor proxy o la puerta de enlace de Log Analytics |

Por ejemplo: `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> Si usa caracteres especiales como "\@" en la contraseña, recibirá un error de conexión de proxy porque el valor no se analiza correctamente.  Para solucionar este problema, codifique la contraseña en la dirección URL con una herramienta como [URLDecode](https://www.urldecoder.org/).  

## <a name="install-and-configure-agent"></a>Instalación y configuración del agente

La conexión de máquinas en la suscripción de Azure o el entorno híbrido directamente con registros de Azure Monitor se puede lograr mediante diferentes métodos según sus requisitos. En la tabla siguiente se resalta cada método para determinar cuál funciona mejor en su organización.

|Source | Método | Descripción|
|-------|-------------|-------------|
|Azure VM| - Extensión de máquinas virtuales de Log Analytics para [Windows](../../virtual-machines/extensions/oms-windows.md) o [Linux](../../virtual-machines/extensions/oms-linux.md) mediante la CLI de Azure o con una plantilla de Azure Resource Manager<br>- [Manualmente desde Azure Portal](../../azure-monitor/learn/quick-collect-azurevm.md?toc=/azure/azure-monitor/toc.json)<br>- [Aprovisionamiento automático de Azure Security Center](../../security-center/security-center-enable-data-collection.md)| - La extensión instala el agente de Log Analytics en máquinas virtuales de Azure y las inscribe en un área de trabajo de Azure Monitor existente.<br>-Azure Security Center puede aprovisionar el agente de Log Analytics tanto en las máquinas virtuales de Azure admitidas como en las nuevas que se creen si lo habilita para supervisar las amenazas y vulnerabilidades de seguridad. Si está habilitado, se aprovisionarán todas las máquinas virtuales nuevas o existentes sin un agente instalado.|
| Equipo híbrido con Windows|- [Instalación manual](agent-windows.md)<br>- [DSC de Azure Automation](agent-windows.md#install-the-agent-using-dsc-in-azure-automation)<br>- [Plantilla de Resource Manager con Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) |Instale Microsoft Monitoring Agent desde la línea de comandos o con un método automatizado, como DSC de Azure Automation, [Configuration Manager](https://docs.microsoft.com/configmgr/apps/deploy-use/deploy-applications) o una plantilla de Azure Resource Manager si implementó Microsoft Azure Stack en el centro de datos.| 
| Equipo híbrido con Linux| [Instalación manual](../../azure-monitor/learn/quick-collect-linux-computer.md)|Instale al agente para Linux llamando a un script contenedor hospedado en GitHub. | 
| System Center Operations Manager|[Integrar Operations Manager con Log Analytics](om-agents.md) | Configure la integración entre Operations Manager y los registros de Azure Monitor para reenviar datos recopilados de equipos Windows que informan a un grupo de administración.|  

## <a name="next-steps"></a>Pasos siguientes

* Revise los [orígenes de datos](agent-data-sources.md) para saber qué orígenes de datos hay disponibles para recopilar datos de su sistema Windows o Linux. 

* Obtenga información acerca de las [consultas de registros](../log-query/log-query-overview.md) para analizar los datos recopilados de soluciones y orígenes de datos. 

* Conozca las [soluciones de supervisión](../insights/solutions.md) que agregan funcionalidad a Azure Monitor y que también recopilan datos en el área de trabajo de Log Analytics.
