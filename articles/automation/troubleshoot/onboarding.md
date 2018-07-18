---
title: Solución de problemas de errores al incorporar Update Management, Change Tracking e Inventory
description: Aprenda a solucionar los errores de incorporación de las soluciones Update Management, Change Tracking e Inventory
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 06/19/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 044cb56b8991a1eb2dd6a1d35be621f2ffab3250
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064459"
---
# <a name="troubleshoot-errors-when-onboarding-solutions"></a>Solución de problemas de errores al incorporar soluciones

Al incorporar soluciones como Update Management, Change Tracking o Inventory, pueden producirse errores. En este artículo se describen los diversos errores que pueden producirse y cómo resolverlos.

## <a name="general-errors"></a>Errores generales

### <a name="computer-grou-query-format-error"></a>Escenario: ComputerGroupQueryFormatError

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

Compruebe las notificaciones en la esquina superior derecha de Azure Portal o desplácese hasta el grupo de recursos que contiene la cuenta de Automation y seleccione **Implementaciones** en **Configuración** para ver la implementación con errores. Para más información acerca de Azure Policy, visite: [Introducción a Azure Policy](../../azure-policy/azure-policy-introduction.md?toc=%2fazure%2fautomation%2ftoc.json).

## <a name="mma-extension-failures"></a>Errores de extensión MMA

Al implementar una solución, se implementan varios recursos relacionados. Uno de esos recursos es la extensión Microsoft Monitoring Agent o el Agente de OMS para Linux. Estas son extensiones de máquina virtual que instala el agente invitado de la máquina virtual que es responsable de comunicarse con el área de trabajo de Operations Management Suite (OMS), con la finalidad de coordinar posteriormente la descarga de archivos binarios y de otro tipo de los que depende la solución que se va a incorporar una vez que comienza la ejecución.
Lo normal es que primero sepa de los errores de instalación de MMA o del Agente de OMS para Linux por una notificación que aparece en el centro de notificaciones. Al hacer clic en esa notificación se proporciona más información sobre el error en concreto. Al navegar al recurso Grupos de recursos y luego al elemento Implementaciones que contiene, también se proporcionan detalles sobre los errores de implementación que se han producido.
La instalación de MMA o del Agente de OMS para Linux puede generar errores por varias razones, y los pasos para solucionar estos errores varían en función del problema. Siga los pasos específicos de solución de problemas.

En la siguiente sección se describen diversos problemas con los que se puede encontrar al incorporar soluciones que pueden ocasionar errores en la implementación de la extensión MMA.

### <a name="webclient-exception"></a>Escenario: Excepción durante una solicitud WebClient

La extensión MMA en la máquina virtual no puede comunicarse con los recursos externos y la implementación produce un error.

#### <a name="issue"></a>Problema

Los siguientes son ejemplos de mensajes de error que se devuelven:

```
Please verify the VM has a running VM agent, and can establish outbound connections to Azure storage.
```

```
'Manifest download error from https://<endpoint>/<endpointId>/Microsoft.EnterpriseCloud.Monitoring_MicrosoftMonitoringAgent_australiaeast_manifest.xml. Error: UnknownError. An exception occurred during a WebClient request.
```

#### <a name="cause"></a>Causa

Algunas de las posibles causas de este error son:

* Hay un servidor proxy configurado en la máquina virtual que solo admite puertos específicos.

* Una configuración de firewall ha bloqueado el acceso a los puertos y las direcciones necesarios.

#### <a name="resolution"></a>Resolución

Asegúrese de que los puertos y las direcciones adecuados están abiertos para la comunicación. Para obtener una lista de direcciones y puertos, consulte [Planeamiento de la red](../automation-hybrid-runbook-worker.md#network-planning).

### <a name="transient-environment-issue"></a>Escenario: Error de instalación debido a problemas de entorno transitorios

La instalación de la extensión Microsoft Monitoring Agent ha producido un error durante la implementación debido a que hay otra instalación o acción que la bloquea.

#### <a name="issue"></a>Problema

Los siguientes son ejemplos de mensajes de error que se pueden devolver:

```
The Microsoft Monitoring Agent failed to install on this machine. Please try to uninstall and reinstall the extension. If the issue persists, please contact support.
```

```
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1618'
```

```
'Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.2) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.2\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 1601'
```

#### <a name="cause"></a>Causa

Algunas de las posibles causas de este error son:

* Hay otra instalación en curso.
* El sistema se desencadenó para reiniciarse durante la implementación de plantilla.

#### <a name="resolution"></a>Resolución

Este es un error transitorio por naturaleza. Vuelva a intentar la implementación para instalar la extensión.

### <a name="installation-timeout"></a>Escenario: Tiempo de expiración de la instalación

No se completó la instalación de la extensión MMA debido a que se agotó el tiempo de espera.

#### <a name="issue"></a>Problema

El siguiente es un ejemplo de un mensaje de error que se puede devolver:

```
Install failed for plugin (name: Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent, version 1.0.11081.4) with exception Command C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\1.0.11081.4\MMAExtensionInstall.exe of Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent has exited with Exit code: 15614
```

#### <a name="cause"></a>Causa

Este error se debe a que la máquina virtual está bajo una carga pesada durante la instalación.

### <a name="resolution"></a>Resolución

Intente instalar la extensión MMA cuando la máquina virtual esté bajo una carga inferior.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.
