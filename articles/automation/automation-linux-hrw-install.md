---
title: Hybrid Runbook Worker de Azure Automation en Linux
description: En este artículo encontrará información sobre cómo instalar Hybrid Runbook Worker de Azure Automation para poder ejecutar runbooks en equipos Linux en su centro de datos local o en su entorno de nube.
services: automation
ms.service: automation
ms.component: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 04/25/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 8507cf99ea22b24aa3026565cb7c4139e4c3742d
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36267867"
---
# <a name="deploy-a-linux-hybrid-runbook-worker"></a>Implementación de Hybrid Runbook Worker en Linux

La característica Hybrid Runbook Worker de Azure Automation permite ejecutar runbooks directamente en el equipo que hospeda el rol y en los recursos del entorno para administrar dichos recursos locales. Hybrid Runbook Worker en Linux ejecuta runbooks como un usuario especial que puede tener permisos elevados para ejecutar comandos que necesitan permisos elevados. Los runbooks se almacenan y administran en Azure Automation y después se entregan a uno o más equipos designados.

En este artículo se describe cómo instalar Hybrid Runbook Worker en un equipo Linux.

## <a name="supported-linux-operating-systems"></a>Sistemas operativos Linux compatibles

La característica Hybrid Runbook Worker admite las siguientes distribuciones:

* Amazon Linux 2012.09 a 2015.09 (x86/x64)
* CentOS Linux 5, 6 y 7 (x86/x64)
* Oracle Linux 5, 6 y 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 y 7 (x86/x64)
* Debian GNU/Linux 6, 7 y 8 (x86/x64)
* Ubuntu 12.04 LTS, 14.04 LTS y 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 y 12 (x86/x64)

## <a name="installing-a-linux-hybrid-runbook-worker"></a>Instalación de Hybrid Runbook Worker en Linux

Para instalar y configurar una instancia de Hybrid Runbook Worker en un equipo Linux, hay que seguir un procedimiento sencillo para instalar y configurar el rol manualmente. Es necesario habilitar la solución **Automation Hybrid Worker** en el área de trabajo de Azure Log Analytics y, después, ejecutar un conjunto de comandos para registrar el equipo como un trabajo y agregarlo a un grupo.

Estos son los requisitos mínimos de Hybrid Runbook Worker en Linux:

* Dos núcleos
* 4 GB de RAM
* Puerto 443 (saliente)

### <a name="package-requirements"></a>Requisitos de paquete

| **Paquetes necesarios** | **Descripción** | **Versión mínima**|
|--------------------- | --------------------- | -------------------|
|Glibc |Biblioteca GNU C| 2.5-12 |
|Openssl| Bibliotecas OpenSSL | 0.9.8E o 1.0|
|Curl | Cliente web de cURL | 7.15.5|
|Python ctypes | |
|PAM | Módulos de autenticación conectables|
| **Paquete opcional** | **Descripción** | **Versión mínima**|
| PowerShell Core | Para ejecutar runbooks de PowerShell, PowerShell debe estar instalado; consulte [Instalación de PowerShell Core en Linux](/powershell/scripting/setup/installing-powershell-core-on-linux) para más información sobre cómo instalarlo.  | 6.0.0 |

### <a name="installation"></a>Instalación

Antes de continuar, anote el área de trabajo de Log Analytics que está vinculado a su cuenta de Automation. Anote también la clave principal de la cuenta de Automation. Encontrará ambos datos en Azure Portal: seleccione la cuenta de Automation y, para el identificador de área de trabajo, seleccione **Área de trabajo** y, para la clave principal, seleccione **Claves**. Para más información sobre los puertos y las direcciones que se necesitan para Hybrid Runbook Worker, consulte [Configuración de la red](automation-hybrid-runbook-worker.md#network-planning).

1. Habilite la solución **Automation Hybrid Worker** en Azure con uno de los métodos siguientes:

   * Agregue la solución **Automation Hybrid Worker** a la suscripción con el procedimiento que se describe en [Adición de soluciones de administración de Azure Log Analytics al área de trabajo](../log-analytics/log-analytics-add-solutions.md).
   * Ejecute el siguiente cmdlet:

        ```azurepowershell-interactive
         Set-AzureRmOperationalInsightsIntelligencePack -ResourceGroupName  <ResourceGroupName> -WorkspaceName <WorkspaceName> -IntelligencePackName  "AzureAutomation" -Enabled $true
        ```

1. Instale el agente de OMS para Linux con el comando siguiente. Reemplace \<WorkspaceID\> y \<WorkspaceKey\> por los valores correspondientes a su área de trabajo.

   ```bash
   wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
   ```

1. Ejecute el siguiente comando y cambie los valores de los parámetros *-w*, *-k*, *-g* y *-e*. Para el parámetro *-g*, reemplace el valor con el nombre del grupo de Hybrid Runbook Worker al que se debe unir el nuevo Hybrid Runbook Worker de Linux. Si el nombre no existe en su cuenta de Automation, se crea un nuevo grupo de Hybrid Runbook Worker con ese nombre.

   ```bash
   sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/onboarding.py --register -w <LogAnalyticsworkspaceId> -k <AutomationSharedKey> -g <hybridgroupname> -e <automationendpoint>
   ```

1. Una vez completado el comando, la página **Grupos de Hybrid Worker** de Azure Portal muestra el nuevo grupo y el número de miembros. Si se trata de un grupo existente, se incrementa el número de miembros. Puede seleccionar el grupo de la lista en la página **Grupos de Hybrid Worker** y seleccionar el icono **Hybrid Workers**. En la página **Hybrid Workers**, verá que aparece cada miembro del grupo.

## <a name="turning-off-signature-validation"></a>Desactivación de la validación de firma

De forma predeterminada, las instancias de Hybrid Runbook Worker de Linux requieren la validación de la firma. Si ejecuta un runbook sin firmar en un trabajo, verá un error que indica que no se ha podido validar la firma. Para desactivar la validación de la firma, ejecute el siguiente comando. Reemplace el segundo parámetro por el identificador de área de trabajo de Log Analytics.

 ```bash
 sudo python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/scripts/require_runbook_signature.py --false <LogAnalyticsworkspaceId>
 ```

## <a name="supported-runbook-types"></a>Tipos de runbook admitidos

Las instancias de Hybrid Runbook Worker en Linux no admiten el conjunto completo de tipos de runbook de Azure Automation.

Los siguientes tipos de runbook funcionan en instancias de Hybrid Worker Linux:

* Python 2
* PowerShell

  > [!NOTE]
  > Los runbooks de PowerShell requieren que PowerShell Core esté instalado en el equipo Linux. Consulte [Instalación de PowerShell Core en Linux](/powershell/scripting/setup/installing-powershell-core-on-linux) para obtener información sobre cómo instalarlo.

Los siguientes tipos de runbook no funcionan en instancias de Hybrid Worker en Linux:

* Flujo de trabajo de PowerShell
* Gráfico
* Flujo de trabajo gráfico de PowerShell

## <a name="troubleshooting"></a>solución de problemas

Hybrid Runbook Worker de Linux depende del agente de OMS para Linux a fin de comunicarse con su cuenta de Automation para registrar el trabajo, recibir trabajos de runbook e informar del estado. Si se produce un error de registro del trabajo, estas son algunas de las causas posibles:

### <a name="the-oms-agent-for-linux-isnt-running"></a>El agente de OMS para Linux no se está ejecutando

Si el agente de OMS para Linux no se está ejecutando, Hybrid Runbook Worker en Linux no se puede comunicar con Azure Automation. Escriba el comando `ps -ef | grep python` para comprobar si el agente se está ejecutando. 

Debería ver una salida similar a la siguiente (los procesos de Python con la cuenta de usuario **nxautomation**). Si no están habilitadas las soluciones Update Management o Azure Automation, no se ejecutará ninguno de los siguientes procesos.

```bash
nxautom+   8567      1  0 14:45 ?        00:00:00 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/main.py /var/opt/microsoft/omsagent/state/automationworker/oms.conf rworkspace:<workspaceId> <Linux hybrid worker version>
nxautom+   8593      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/state/automationworker/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
nxautom+   8595      1  0 14:45 ?        00:00:02 python /opt/microsoft/omsconfig/modules/nxOMSAutomationWorker/DSCResources/MSFT_nxOMSAutomationWorkerResource/automationworker/worker/hybridworker.py /var/opt/microsoft/omsagent/<workspaceId>/state/automationworker/diy/worker.conf managed rworkspace:<workspaceId> rversion:<Linux hybrid worker version>
```

En la siguiente lista, se muestran los procesos que se inician para una instancia de Hybrid Runbook Worker en Linux. Se encuentran en el directorio `/var/opt/microsoft/omsagent/state/automationworker/`.

* **oms.conf**: proceso de administrador de trabajos. Se inicia directamente desde la configuración de estado deseado (DSC).

* **worker.conf**: proceso de Auto Registered Hybrid Worker. Lo inicia el administrador de trabajos. Este proceso lo usa Update Management y es transparente para el usuario. Este proceso solo estará presente si la solución Update Management está habilitada en el equipo.

* **diy/worker.conf**: proceso de DIY Hybrid Worker. El proceso de DIY Hybrid Worker se usa para ejecutar runbooks de usuario en Hybrid Runbook Worker. Difiere del proceso de Auto Registered Hybrid Worker solo en que usa otra configuración. Este proceso solo estará presente si la solución Azure Automation está habilitada y DIY Hybrid Worker en Linux está registrado.

Si el agente de OMS para Linux no se está ejecutando, ejecute el siguiente comando para iniciar el servicio: `sudo /opt/microsoft/omsagent/bin/service_control restart`.

### <a name="the-specified-class-doesnt-exist"></a>La clase especificada no existe

Si ve el error "La clase especificada no existe" en `/var/opt/microsoft/omsconfig/omsconfig.log`, el agente de OMS para Linux debe actualizarse. Ejecute el siguiente comando para volver a instalar el agente de OMS:

```bash
wget https://raw.githubusercontent.com/Microsoft/OMS-Agent-for-Linux/master/installer/scripts/onboard_agent.sh && sh onboard_agent.sh -w <WorkspaceID> -s <WorkspaceKey>
```

Para ver pasos adicionales de solución de problemas relacionados con Update Management, consulte [Update Management: solución de problemas](automation-update-management.md#troubleshooting).

## <a name="next-steps"></a>Pasos siguientes

* Consulte [Ejecución de runbooks en Hybrid Runbook Worker](automation-hrw-run-runbooks.md) para más información sobre cómo configurar los runbooks para automatizar los procesos del centro de datos local o de otros entornos de nube.
* Para ver instrucciones sobre cómo quitar instancias de Hybrid Runbook Worker, consulte [Quitar instancias de Hybrid Runbook Worker de Azure Automation](automation-hybrid-runbook-worker.md#remove-a-hybrid-runbook-worker).
