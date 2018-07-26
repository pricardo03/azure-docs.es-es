---
title: Recopilación de datos del entorno con Azure Log Analytics | Microsoft Docs
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
ms.date: 07/11/2018
ms.author: magoedte
ms.component: na
ms.openlocfilehash: 2a21c7867bf0dd2d6ca6ee0bd9025739315c8d0a
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39003325"
---
# <a name="collect-data-from-computers-in-your-environment-with-log-analytics"></a>Recopilar datos de equipos en su entorno con Log Analytics

Log Analytics permite recopilar y actuar sobre datos de equipos Windows o Linux que residen en:

* [Máquinas virtuales de Azure](log-analytics-quick-collect-azurevm.md) con la extensión de VM de Log Analytics 
* Centros de datos, como servidores físicos o máquinas virtuales
* Máquinas virtuales en un servicio hospedado en la nube, como Amazon Web Services (AWS)

Los equipos hospedados en su entorno pueden estar conectados directamente a Log Analytics, o si ya supervisa estos equipos con System Center Operations Manager 2012 R2, 2016 o la versión 1801, puede integrar el grupo de administración de Operations Manager con Log Analytics para mantener sus estrategias y procesos de las operaciones de servicio.  

## <a name="overview"></a>Información general

![log-analytics-agent-direct-connect-diagram](media/log-analytics-concept-hybrid/log-analytics-on-prem-comms.png)

Antes de analizar datos recopilados y actuar sobre ellos, deberá instalar y conectar agentes en todos los equipos que quiera que puedan enviar datos al servicio Log Analytics. Puede instalar los agentes en los equipos locales a través del programa de instalación, a través de la línea de comandos o utilizando Configuración de estado deseado (DSC) en Azure Automation. 

El agente para Linux y Windows se comunica con el servicio Log Analytics en el puerto TCP 443 y, si el equipo se conecta a un servidor proxy o a un firewall para comunicarse a través de Internet, consulte la [sección de requisitos previos](#prerequisites) para comprender qué configuración de red debe aplicarse.  Si las directivas de seguridad de TI no permite que los equipos de la red se conecten a Internet, puede configurar una [puerta de enlace de OMS](log-analytics-oms-gateway.md) y, a continuación, configurar el agente para conectarse a Log Analytics a través de la puerta de enlace. El agente, a continuación, puede recibir información de configuración y enviar los datos recopilados según qué reglas de recopilación de datos y soluciones haya habilitado. 

Si va a supervisar el equipo con System Center 2016 - Operations Manager u Operations Manager 2012 R2, puede tener hosts múltiples con el servicio Log Analytics para recopilar datos y reenviarlos al servicio, y ser supervisados por [Operations Manager](log-analytics-om-agents.md). Los equipos de Linux supervisados por un grupo de administración de Operations Manager que se integra con Log Analytics no reciben la configuración de los orígenes de datos y reenvían los datos recopilados a través del grupo de administración. El agente de Windows puede informar acerca de hasta cuatro áreas de trabajo, mientras que el agente de Linux solo puede informar acerca de una sola área de trabajo.  

El agente para Windows y Linux no es solo para conectarse a Log Analytics, sino que también admite Azure Automation para hospedar el rol de trabajo Hybrid Runbook Worker y soluciones de administración como Change Tracking y Update Management.  Para obtener más información acerca de la función Hybrid Runbook Worker, consulte [Hybrid Runbook Worker de Azure Automation](../automation/automation-hybrid-runbook-worker.md).  

## <a name="supported-windows-operating-systems"></a>Sistemas operativos Windows compatibles
Las siguientes versiones del sistema operativo Windows son compatibles oficialmente con el agente de Windows:

* Windows Server 2008 Service Pack 1 (SP1) o versiones posteriores
* Windows 7 SP1 y versiones posteriores.

## <a name="supported-linux-operating-systems"></a>Sistemas operativos Linux compatibles
Las distribuciones Linux siguientes son compatibles oficialmente.  Aunque también se puede ejecutar el agente de Linux en otras distribuciones que no se enumeran.  A menos que se indique lo contrario, todas las versiones secundarias son compatibles con cada versión principal de la lista.  

* Amazon Linux 2012.09 a 2015.09 (x86/x64)
* CentOS Linux 5, 6 y 7 (x86/x64)  
* Oracle Linux 5, 6 y 7 (x86/x64) 
* Red Hat Enterprise Linux Server 5, 6 y 7 (x86/x64)
* Debian GNU/Linux 6, 7 y 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 y 12 (x86/x64)

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

El agente de Linux y Windows admite la comunicación a través de un servidor proxy o la puerta de enlace de OMS para el servicio Log Analytics mediante el protocolo HTTPS.  Se admite la autenticación anónima y básica (nombre de usuario/contraseña).  Si el agente de Windows está conectado directamente al servicio, la configuración de proxy se especifica durante la instalación o [después de la implementación](log-analytics-agent-manage.md#update-proxy-settings) desde el Panel de control o con PowerShell.  

Con el agente de Linux, el servidor proxy se especifica durante o [después de la instalación](/log-analytics-agent-manage.md#update-proxy-settings) mediante la modificación del archivo de configuración proxy.conf.  El valor de configuración del proxy del agente de Linux tiene la siguiente sintaxis:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Aunque el servidor proxy no requiera que se autentique, el agente de Linux sí que requerirá que proporcione un pseudousuario y contraseña. Puede ser cualquier nombre de usuario o contraseña.

|Propiedad| Descripción |
|--------|-------------|
|Protocolo | https |
|user | Nombre de usuario opcional para la autenticación de proxy |
|contraseña | Contraseña opcional para la autenticación de proxy |
|proxyhost | Dirección o el FQDN de la puerta de enlace de OMS/servidor proxy |
|puerto | Número de puerto opcional para el servidor proxy/puerta de enlace de OMS |

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

* Obtenga información acerca de las [búsquedas de registros](log-analytics-log-searches.md) para analizar los datos recopilados de las soluciones y los orígenes de datos. 

* Obtenga información sobre las [soluciones](log-analytics-add-solutions.md) que agregan funcionalidad a Log Analytics y que también recopilan datos en el repositorio de OMS.