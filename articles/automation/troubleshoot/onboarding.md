---
title: Solución de problemas de errores al incorporar Update Management, Change Tracking e Inventory
description: Aprenda a solucionar los errores de incorporación de las soluciones Update Management, Change Tracking e Inventory
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 05/22/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 8867912d98897a695c1e59ebd4177301230281bb
ms.sourcegitcommit: d89032fee8571a683d6584ea87997519f6b5abeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/30/2019
ms.locfileid: "66399770"
---
# <a name="troubleshoot-errors-when-onboarding-solutions"></a>Solución de problemas de errores al incorporar soluciones

Al incorporar soluciones como Update Management, Change Tracking o Inventory, pueden producirse errores. En este artículo se describen los diversos errores que pueden producirse y cómo resolverlos.

## <a name="general-errors"></a>Errores generales

### <a name="missing-write-permissions"></a>Escenario: error de incorporación con el mensaje: no se puede habilitar la solución

#### <a name="issue"></a>Problema

Recibirá uno de los mensajes siguientes cuando se intenta incorporar una máquina virtual a una solución:

```error
The solution cannot be enabled due to missing permissions for the virtual machine or deployments
```

```error
The solution cannot be enabled on this VM because the permission to read the workspace is missing
```

#### <a name="cause"></a>Causa

Este error se debe a permisos incorrectos o que faltan en la máquina virtual, el área de trabajo, o para el usuario.

#### <a name="resolution"></a>Resolución

Asegúrese de tener los permisos correctos para incorporar la máquina virtual. Revise los [permisos necesarios para incorporar máquinas](../automation-role-based-access-control.md#onboarding) y vuelva a intentar incorporar la solución. Si recibe el error `The solution cannot be enabled on this VM because the permission to read the workspace is missing`, asegúrese de que tiene el `Microsoft.OperationalInsights/workspaces/read` permiso para encontrará si la máquina virtual esté incorporada a un área de trabajo.

### <a name="diagnostic-logging"></a>Escenario: Se produce un error en la incorporación con el mensaje: no se pudo configurar la cuenta de Automation para el registro de diagnóstico

#### <a name="issue"></a>Problema

Al intentar incorporar una máquina virtual a una solución, recibe el mensaje siguiente:

```error
Failed to configure automation account for diagnostic logging
```

#### <a name="cause"></a>Causa

Este error puede deberse a si el plan de tarifa no coincide con el modelo de facturación de la suscripción. Para obtener más información, consulte [supervisión del uso y costos estimados en Azure Monitor](http://aka.ms/PricingTierWarning).

#### <a name="resolution"></a>Resolución

Crear manualmente el área de trabajo de Log Analytics y repita el proceso de incorporación para seleccionar el área de trabajo creada.

### <a name="computer-group-query-format-error"></a>Escenario: ComputerGroupQueryFormatError

#### <a name="issue"></a>Problema

Este código de error significa que la consulta de búsqueda guardada del grupo de equipos que se utilizó tomando a la solución como objetivo no tenía un formato correcto. 

#### <a name="cause"></a>Causa

Puede que haya cambiado la consulta, o puede que lo haya hecho el sistema.

#### <a name="resolution"></a>Resolución

Puede eliminar la consulta para esta solución y reincorporar la solución, con lo que se vuelve a crear la consulta. La consulta puede encontrarse en el área de trabajo, en **Búsquedas guardadas**. El nombre de la consulta es **MicrosoftDefaultComputerGroup**, y la categoría de la consulta es el nombre de la solución asociada a esta consulta. Si se habilitan varias soluciones, **MicrosoftDefaultComputerGroup** se muestra varias veces en **Búsquedas guardadas**.

### <a name="policy-violation"></a>Escenario: PolicyViolation

#### <a name="issue"></a>Problema

Este código de error significa que no se pudo realizar la implementación debido a la infracción de una o varias directivas.

#### <a name="cause"></a>Causa 

Existe una directiva que impide que se complete la operación.

#### <a name="resolution"></a>Resolución

Para implementar correctamente la solución, debe considerar modificar la directiva indicada. Dado que hay muchos tipos diferentes de directivas que se pueden definir, los cambios específicos necesarios dependen de la directiva que se ha infringido. Por ejemplo, si se define una directiva en un grupo de recursos que deniega el permiso para cambiar el contenido de ciertos tipos de recursos dentro de ese grupo de recursos podría realizar cualquiera de las siguientes acciones:

* Eliminar por completo la directiva.
* Intentar realizar una incorporación a otro grupo de recursos.
* Revisar la directiva, por ejemplo:
  * Cambiar el destino de la directiva por un recurso concreto (por ejemplo, por una cuenta de Automation específica).
  * Revisar el conjunto de recursos para los que se configuró en la directiva denegar el acceso.

Compruebe las notificaciones en la esquina superior derecha de Azure portal o navegue hasta el grupo de recursos que contiene la cuenta de automation y seleccione **implementaciones** en **configuración** para ver el error despliegue. Para obtener más información sobre Azure Policy, visite: [Introducción a Azure Policy](../../governance/policy/overview.md?toc=%2fazure%2fautomation%2ftoc.json).

### <a name="unlink"></a>Escenario: Error al intentar desvincular un área de trabajo

#### <a name="issue"></a>Problema

Recibirá el siguiente error al intentar desvincular un área de trabajo:

```error
The link cannot be updated or deleted because it is linked to Update Management and/or ChangeTracking Solutions.
```

#### <a name="cause"></a>Causa

Este error se produce cuando todavía dispone de soluciones activas en el área de trabajo de Log Analytics que dependen de su cuenta de Automation y Log Analytics del área de trabajo que se están vinculando.

### <a name="resolution"></a>Resolución

Para resolver este problema deberá quitar las siguientes soluciones del área de trabajo si está usando:

* Administración de actualizaciones
* Seguimiento de cambios
* Inicio y detención de máquinas virtuales durante las horas de trabajo

Una vez que quite las soluciones puede desvincular el área de trabajo. Es importante limpiar los artefactos de esas soluciones desde el área de trabajo y la cuenta de Automation existentes.  

* Administración de actualizaciones
  * Quitar las implementaciones de actualizaciones (programaciones) de la cuenta de Automation
* Inicio y detención de máquinas virtuales durante las horas de trabajo
  * Quitar los bloqueos de componentes de la solución en la cuenta de Automation en **configuración** > **bloqueos**.
  * Para conocer pasos adicionales quitar el Start/Stop VMs During OFF-hours ver, [quitar la máquina virtual de inicios y paradas During OFF-hours](../automation-solution-vm-management.md##remove-the-solution).

## <a name="mma-extension-failures"></a>Errores de extensión MMA

[!INCLUDE [log-analytics-agent-note](../../../includes/log-analytics-agent-note.md)] 

Al implementar una solución, se implementan varios recursos relacionados. Uno de esos recursos es la extensión Microsoft Monitoring Agent o el Agente de Log Analytics para Linux. Estos son extensiones de máquina Virtual instalado por el agente de invitado de la máquina virtual que es responsable de comunicarse con el área de trabajo configurada de Log Analytics, con el fin de coordinación posterior de la descarga de archivos binarios y otros archivos que el solución le dependen de incorporación una vez que comienza la ejecución.
Lo normal es que primero sepa de los errores de instalación de MMA o del Agente de Log Analytics para Linux por una notificación que aparece en el centro de notificaciones. Al hacer clic en esa notificación se proporciona más información sobre el error en concreto. Al navegar al recurso Grupos de recursos y luego al elemento Implementaciones que contiene, también se proporcionan detalles sobre los errores de implementación que se han producido.
La instalación de MMA o del Agente de Log Analytics para Linux puede generar errores por varias razones, y los pasos para solucionar estos errores varían en función del problema. Siga los pasos específicos de solución de problemas.

La siguiente sección describe varios problemas que puede encontrarse cuando incorporación que se puede producir un error en la implementación de la extensión de MMA.

### <a name="webclient-exception"></a>Escenario: Excepción durante una solicitud WebClient

La extensión MMA en la máquina virtual no puede comunicarse con los recursos externos y la implementación produce un error.

#### <a name="issue"></a>Problema

Los siguientes son ejemplos de mensajes de error que se devuelven:

```error
Please verify the VM has a running VM agent, and can establish outbound connections to Azure storage.
```

```error
'Manifest download error from https://<endpoint>/<endpointId>/Microsoft.EnterpriseCloud.Monitoring_MicrosoftMonitoringAgent_australiaeast_manifest.xml. Error: UnknownError. An exception occurred during a WebClient request.
```

#### <a name="cause"></a>Causa

Algunas de las posibles causas de este error son:

* Hay un proxy configurado en la máquina virtual, que sólo admite puertos específicos.

* Una configuración de firewall ha bloqueado el acceso a los puertos y las direcciones necesarios.

#### <a name="resolution"></a>Resolución

Asegúrese de que los puertos y las direcciones adecuados están abiertos para la comunicación. Para obtener una lista de direcciones y puertos, consulte [Planeamiento de la red](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="transient-environment-issue"></a>Escenario: No se pudo instalar debido a problemas de un entorno transitorio

Error en la instalación de la extensión Microsoft Monitoring Agent durante la implementación debido a otra instalación o la acción de bloqueo de la instalación

#### <a name="issue"></a>Problema

Los siguientes son ejemplos de mensajes de error que se pueden devolver:

```error
The Microsoft Monitoring Agent failed to install on this machine. Please try to uninstall and reinstall the extension. If the issue persists, please contact support.
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1618'
```

```error
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.2) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.2\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1601'
```

#### <a name="cause"></a>Causa

Algunas de las posibles causas de este error son:

* Hay otra instalación en curso.
* Se desencadena el sistema para reiniciar durante la implementación de plantilla

#### <a name="resolution"></a>Resolución

Este es un error transitorio por naturaleza. Vuelva a intentar la implementación para instalar la extensión.

### <a name="installation-timeout"></a>Escenario: Tiempo de expiración de la instalación

No se completó la instalación de la extensión de MMA debido a un tiempo de espera.

#### <a name="issue"></a>Problema

El ejemplo siguiente es de un mensaje de error que puede devolver:

```error
Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 15614
```

#### <a name="cause"></a>Causa

Este error se produce porque la máquina virtual está sometido a una carga elevada durante la instalación.

### <a name="resolution"></a>Resolución

Intente instalar la extensión MMA cuando la máquina virtual esté bajo una carga inferior.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
