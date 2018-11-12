---
title: Recopilación de datos en un entorno híbrido con el agente de Azure Log Analytics | Microsoft Docs
description: En este tema se le ayudará a comprender cómo recopilar datos y supervisar equipos hospedados localmente o en entornos de nube con Log Analytics.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 10/15/2018
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: b50e6de9de6498f57fd9b599c45a35f2367b84ba
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51012146"
---
# <a name="collect-data-in-a-hybrid-environment-with-log-analytics-agent"></a>Recopilación de datos en un entorno híbrido con el agente de Log Analytics

Azure Log Analytics permite recopilar y actuar sobre datos de equipos que ejecutan el sistema operativo Windows o Linux que, a su vez, se ejecutan en:

* [Máquinas virtuales de Azure](log-analytics-quick-collect-azurevm.md) con la extensión de VM de Log Analytics 
* Centros de datos, como servidores físicos o máquinas virtuales
* Máquinas virtuales en un servicio hospedado en la nube, como Amazon Web Services (AWS)

Los equipos hospedados en su entorno pueden estar conectados directamente a Log Analytics o, si ya supervisa estos equipos con System Center Operations Manager 2012 R2 o posterior, puede integrar el grupo de administración de Operations Manager con Log Analytics para mantener sus estrategias y procesos de las operaciones de servicio.  

## <a name="overview"></a>Información general

![log-analytics-agent-direct-connect-diagram](media/log-analytics-concept-hybrid/log-analytics-on-prem-comms.png)

Antes de analizar datos recopilados y actuar sobre ellos, deberá instalar y conectar agentes en todos los equipos que quiera que puedan enviar datos al servicio Log Analytics. Puede instalar los agentes en los equipos locales a través del programa de instalación, a través de la línea de comandos o utilizando Configuración de estado deseado (DSC) en Azure Automation. 

El agente para Linux y Windows se comunica con el servicio Log Analytics en el puerto TCP 443 y, si el equipo se conecta a un servidor proxy o a un firewall para comunicarse a través de Internet, consulte los requisitos siguientes para comprender qué configuración de red debe aplicarse.  Si las directivas de seguridad de TI no permiten que los equipos de la red se conecten a Internet, puede configurar una [puerta de enlace de Log Analytics](log-analytics-oms-gateway.md) y luego configurar el agente para conectarse a Log Analytics a través de la puerta de enlace. El agente, a continuación, puede recibir información de configuración y enviar los datos recopilados según qué reglas de recopilación de datos y soluciones haya habilitado. 

Si va a supervisar el equipo con System Center Operations Manager 2012 R2 o posterior, puede tener hosts múltiples con el servicio Log Analytics para recopilar datos y reenviarlos al servicio, y ser supervisados por [Operations Manager](log-analytics-om-agents.md). Los equipos de Linux supervisados por un grupo de administración de Operations Manager que se integra con Log Analytics no reciben la configuración de los orígenes de datos y reenvían los datos recopilados a través del grupo de administración. El agente de Windows puede informar acerca de hasta cuatro áreas de trabajo, mientras que el agente de Linux solo puede informar acerca de una sola área de trabajo.  

El agente para Windows y Linux no es solo para conectarse a Log Analytics, sino que también admite Azure Automation para hospedar el rol de trabajo Hybrid Runbook Worker y soluciones de administración como Change Tracking y Update Management.  Para obtener más información acerca de la función Hybrid Runbook Worker, consulte [Hybrid Runbook Worker de Azure Automation](../automation/automation-hybrid-runbook-worker.md).  

## <a name="supported-windows-operating-systems"></a>Sistemas operativos Windows compatibles
Las siguientes versiones del sistema operativo Windows son compatibles oficialmente con el agente de Windows:

* Windows Server 2008 Service Pack 1 (SP1) o versiones posteriores
* Windows 7 SP1 y versiones posteriores.

## <a name="supported-linux-operating-systems"></a>Sistemas operativos Linux compatibles
En esta sección se ofrecen detalles sobre las distribuciones de Linux compatibles.    

A partir de las versiones publicadas después de agosto de 2018, hemos realizado los siguientes cambios en nuestro modelo de soporte técnico:  

* Solo se admiten las versiones de servidor, no de cliente.  
* Siempre se admiten las nuevas versiones de [distribuciones aprobadas para Linux de Azure](../virtual-machines/linux/endorsed-distros.md).  
* Se admiten todas las versiones secundarias de cada versión principal de la lista.
* No se admiten las versiones que han superado la fecha de finalización de soporte técnico de su fabricante.  
* No se admiten las nuevas versiones de AMI.  
* Solo se admiten las versiones que ejecutan SSL 1.x de forma predeterminada.

Si utiliza una distribución o versión que no se admite actualmente y no se alinea con nuestro modelo de soporte técnico, se recomienda bifurcar este repositorio, confirmar que el soporte técnico de Microsoft no prestará asistencia con versiones de agente con bifurcación.

* Amazon Linux 2017.09 (x64)
* CentOS Linux 6 (x86/x64) y 7 (x64)  
* Oracle Linux 6 y 7 (x86/x64) 
* Red Hat Enterprise Linux Server 6 (x86/x64) y 7 (x64)
* Debian GNU/Linux 8 y 9 (x86/x64)
* Ubuntu 14.04 LTS (x86/x64), 16.04 LTS (x86/x64) y 18.04 LTS (x64)
* SUSE Linux Enterprise Server 12 (x64)

>[!NOTE]
>OpenSSL 1.1.0 solo se admite en las plataformas x86_x64 (64 bits) y OpenSSL anterior a 1.x no se admite en ninguna plataforma.
>

## <a name="tls-12-protocol"></a>Protocolo TLS 1.2
Para garantizar la seguridad de los datos en tránsito a Log Analytics, se recomienda encarecidamente configurar el agente para que use al menos Seguridad de la capa de transporte (TLS) 1.2. Las versiones anteriores de TLS/Capa de sockets seguros (SSL) han demostrado ser vulnerables y, si bien todavía funcionan para permitir la compatibilidad con versiones anteriores, **no se recomiendan**.  Para información adicional, revise [Sending data securely using TLS 1.2](log-analytics-data-security.md#sending-data-securely-using-tls-12) (Envío de datos de forma segura mediante TLS 1.2). 

## <a name="network-firewall-requirements"></a>Requisitos de firewall de red
A continuación se muestra la información de configuración de proxy y firewall requerida para que el agente de Windows y Linux se comunique con Log Analytics.  

|Recurso del agente|Puertos |Dirección |Omitir inspección de HTTPS|
|------|---------|--------|--------|   
|* .ods.opinsights.azure.com |Puerto 443 |Entrada y salida|Sí |  
|* .oms.opinsights.azure.com |Puerto 443 |Entrada y salida|Sí |  
|* .blob.core.windows.net |Puerto 443 |Entrada y salida|Sí |  
|* .azure-automation.net |Puerto 443 |Entrada y salida|Sí |  


Si tiene previsto usar Hybrid Runbook Worker de Azure Automation para conectarse al servicio Automation y registrarse en él para usar runbooks en el entorno, debe tener acceso al número de puerto y las direcciones URL descritos en [Configuración de la red para Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md#network-planning). 

El agente de Linux y Windows admite la comunicación a través de un servidor proxy o de la puerta de enlace de Log Analytics en el servicio Log Analytics con el protocolo HTTPS.  Se admite la autenticación anónima y básica (nombre de usuario/contraseña).  Si el agente de Windows está conectado directamente al servicio, la configuración de proxy se especifica durante la instalación o [después de la implementación](log-analytics-agent-manage.md#update-proxy-settings) desde el Panel de control o con PowerShell.  

Con el agente de Linux, el servidor proxy se especifica durante o [después de la instalación](log-analytics-agent-manage.md#update-proxy-settings) mediante la modificación del archivo de configuración proxy.conf.  El valor de configuración del proxy del agente de Linux tiene la siguiente sintaxis:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Aunque el servidor proxy no requiera que se autentique, el agente de Linux sí que requerirá que proporcione un pseudousuario y contraseña. Puede ser cualquier nombre de usuario o contraseña.

|Propiedad| Descripción |
|--------|-------------|
|Protocolo | https |
|user | Nombre de usuario opcional para la autenticación de proxy |
|contraseña | Contraseña opcional para la autenticación de proxy |
|proxyhost | Dirección o nombre de dominio completo (FQDN) del servidor proxy o la puerta de enlace de Log Analytics |
|puerto | Número de puerto opcional para el servidor proxy o la puerta de enlace de Log Analytics |

Por ejemplo: `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> Si usa caracteres especiales como "\@" en la contraseña, recibirá un error de conexión de proxy porque el valor no se analiza correctamente.  Para solucionar este problema, codifique la contraseña en la dirección URL con una herramienta como [URLDecode](https://www.urldecoder.org/).  

## <a name="install-and-configure-agent"></a>Instalación y configuración del agente 
Conectar los equipos locales directamente con Log Analytics puede realizarse mediante métodos diferentes según sus requisitos. En la tabla siguiente se resalta cada método para determinar cuál funciona mejor en su organización.

|Origen | Método | Descripción|
|-------|-------------|-------------|
| Equipo de Windows|- [Instalación manual](log-analytics-agent-windows.md)<br>- [DSC de Azure Automation](log-analytics-agent-windows.md#install-the-agent-using-dsc-in-azure-automation)<br>- [Plantilla de Resource Manager con Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) |Instale el agente de Microsoft Monitoring desde la línea de comandos o mediante un método automatizado, como DSC de Azure Automation, [System Center Configuration Manager](https://docs.microsoft.com/sccm/apps/deploy-use/deploy-applications), o con una plantilla de Azure Resource Manager si implementó Microsoft Azure Stack en su centro de datos.| 
|Equipo con Linux| [Instalación manual](log-analytics-quick-collect-linux-computer.md)|Instale al agente para Linux llamando a un script contenedor hospedado en GitHub. | 
| System Center Operations Manager|[Integrar Operations Manager con Log Analytics](log-analytics-om-agents.md) | Configurar la integración entre Operations Manager y Log Analytics para reenviar datos procedentes de equipos Linux y Windows que informan a un grupo de administración.|  

## <a name="next-steps"></a>Pasos siguientes

* Revise los [orígenes de datos](log-analytics-data-sources.md) para saber qué orígenes de datos hay disponibles para recopilar datos de su sistema Windows o Linux. 

* Obtenga información acerca de las [búsquedas de registros](log-analytics-queries.md) para analizar los datos recopilados de soluciones y orígenes de datos. 

* Conozca las [soluciones](../monitoring/monitoring-solutions.md) que agregan funcionalidad a Log Analytics y que también recopilan datos en el área de trabajo de Log Analytics.
