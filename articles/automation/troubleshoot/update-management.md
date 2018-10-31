---
title: Solucionar problemas relativos a errores con Update Management
description: Obtenga información acerca de la solución de problemas relacionados con Update Management.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 10/17/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 41883fd677d276f8f26721fdccc3ded020c3278b
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2018
ms.locfileid: "49405236"
---
# <a name="troubleshooting-issues-with-update-management"></a>Solución de problemas relacionados con Update Management

En este artículo se describen soluciones para resolver problemas que pueden surgir al usar Update Management.

## <a name="general"></a>General

### <a name="components-enabled-not-working"></a>Escenario: se han habilitado los componentes de la solución "Update Management" y ahora se está configurando la máquina virtual.

#### <a name="issue"></a>Problema

Continúa recibiendo el mensaje siguiente en una máquina virtual 15 minutos después de la incorporación:

```
The components for the 'Update Management' solution have been enabled, and now this virtual machine is being configured. Please be patient, as this can sometimes take up to 15 minutes.
```

#### <a name="cause"></a>Causa

Este error puede deberse a las siguientes razones:

1. Está bloqueada la comunicación a la cuenta de Automation.
2. La máquina virtual que se incorpora puede provenir de una máquina clonada que no estaba preparada con sysprep con Microsoft Monitoring Agent instalado.

#### <a name="resolution"></a>Resolución

1. Visite [Planeamiento de red](../automation-hybrid-runbook-worker.md#network-planning) para obtener información acerca de qué direcciones y puertos deben permitirse para que Update Management funcione.
2. Si se usa una imagen clonada, primero prepare con sysprep la imagen e instale al agente de MMA después del hecho.

## <a name="windows"></a>Windows

Si se producen problemas al intentar incorporar la solución en una máquina virtual, compruebe el registro de eventos de **Operations Manager** en **Registros de aplicaciones y servicios** en la máquina local en busca de eventos con el identificador de evento **4502** y el mensaje de evento que contenga **Microsoft.EnterpriseManagement.HealthService.AzureAutomation.HybridAgent**.

En la sección siguiente se destacan los mensajes de error específicos y una posible solución para cada uno. Para otros problemas sobre la incorporación, consulte el artículo sobre la [solución de problemas de incorporación de la solución](onboarding.md).

### <a name="machine-already-registered"></a>Escenario: La máquina ya está registrada en una cuenta diferente

#### <a name="issue"></a>Problema

Aparece el siguiente mensaje de error:

```
Unable to Register Machine for Patch Management, Registration Failed with Exception System.InvalidOperationException: {"Message":"Machine is already registered to a different account."}
```

#### <a name="cause"></a>Causa

La máquina ya está incorporada a otra área de trabajo para Update Management.

#### <a name="resolution"></a>Resolución

Realice una limpieza de los artefactos antiguos en la máquina mediante la [eliminación del grupo Hybrid Runbook](../automation-hybrid-runbook-worker.md#remove-a-hybrid-worker-group) y vuelva a intentarlo.

### <a name="machine-unable-to-communicate"></a>Escenario: La máquina no puede comunicarse con el servicio

#### <a name="issue"></a>Problema

Aparece uno de los siguientes mensajes de error:

```
Unable to Register Machine for Patch Management, Registration Failed with Exception System.Net.Http.HttpRequestException: An error occurred while sending the request. ---> System.Net.WebException: The underlying connection was closed: An unexpected error occurred on a receive. ---> System.ComponentModel.Win32Exception: The client and server can't communicate, because they do not possess a common algorithm
```

```
Unable to Register Machine for Patch Management, Registration Failed with Exception Newtonsoft.Json.JsonReaderException: Error parsing positive infinity value.
```

```
The certificate presented by the service <wsid>.oms.opinsights.azure.com was not issued by a certificate authority used for Microsoft services. Contact your network administrator to see if they are running a proxy that intercepts TLS/SSL communication.
```

#### <a name="cause"></a>Causa

Puede haber un servidor proxy, una puerta de enlace o un firewall bloqueando la comunicación de red.

#### <a name="resolution"></a>Resolución

Revise la red y asegúrese de que se permiten las direcciones y los puertos correspondientes. Consulte los [requisitos de red](../automation-hybrid-runbook-worker.md#network-planning), para obtener una lista de puertos y direcciones que necesitan Update Management y las instancias de Hybrid Runbook Worker.

### <a name="unable-to-create-selfsigned-cert"></a>Escenario: error al crear el certificado autofirmado.

#### <a name="issue"></a>Problema

Aparece uno de los siguientes mensajes de error:

```
Unable to Register Machine for Patch Management, Registration Failed with Exception AgentService.HybridRegistration. PowerShell.Certificates.CertificateCreationException: Failed to create a self-signed certificate. ---> System.UnauthorizedAccessException: Access is denied.
```

#### <a name="cause"></a>Causa

Hybrid Runbook Worker no pudo generar un certificado autofirmado

#### <a name="resolution"></a>Resolución

Verifique que la cuenta del sistema tiene acceso de lectura a la carpeta **C:\ProgramData\Microsoft\Crypto\RSA** e inténtelo de nuevo.

### <a name="hresult"></a>Escenario: La máquina aparece como No evaluado y se muestra una excepción HResult

#### <a name="issue"></a>Problema

Tiene máquinas que aparecen como **No evaluado** en **Cumplimiento**, y verá un mensaje de excepción debajo de él.

#### <a name="cause"></a>Causa

Windows Update no está configurado correctamente en la máquina.

#### <a name="resolution"></a>Resolución

Haga doble clic en la excepción que se muestra en rojo para ver el mensaje de excepción completo. Revise la siguiente tabla para ver posibles soluciones o acciones que deben realizarse:

|Excepción  |Acción o resolución  |
|---------|---------|
|`Exception from HRESULT: 0x……C`     | Busque el código de error correspondiente en [Lista de códigos de error de Windows Update](https://support.microsoft.com/help/938205/windows-update-error-code-list) para buscar detalles adicionales sobre la causa de la excepción.        |
|`0x8024402C` o `0x8024401C`     | Estos errores son problemas de conectividad de red. Asegúrese de que la máquina tenga la conectividad de red adecuada para Update Management. Consulte la sección sobre [planeamiento de red](../automation-update-management.md#ports) para obtener una lista de puertos y direcciones que se requieren.        |
|`0x8024402C`     | Si usa un servidor WSUS, asegúrese de que los valores de registro de `WUServer` y `WUStatusServer` bajo la clave del registro `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate` tienen el servidor WSUS correcto.        |
|`The service cannot be started, either because it is disabled or because it has no enabled devices associated with it. (Exception from HRESULT: 0x80070422)`     | Asegúrese de que el servicio Windows Update (wuauserv) se está ejecutando y no está deshabilitado.        |
|Cualquier otra excepción genérica     | Realice una búsqueda en Internet para ver las soluciones posibles y colabore con el equipo de soporte técnico de TI local.         |

## <a name="linux"></a>Linux

### <a name="scenario-update-run-fails-to-start"></a>Escenario: No se puede iniciar la ejecución de la actualización

#### <a name="issue"></a>Problema

No se pudo iniciar una ejecución de actualización en una máquina Linux.

#### <a name="cause"></a>Causa

La instancia de Hybrid Worker de Linux no es correcta.

#### <a name="resolution"></a>Resolución

Realice una copia del archivo de registro siguiente y consérvelo para la solución de problemas:

```
/var/opt/microsoft/omsagent/run/automationworker/worker.log
```

### <a name="scenario-update-run-starts-but-encounters-errors"></a>Escenario: La ejecución de actualización se inicia, pero encuentra errores

#### <a name="issue"></a>Problema

Se inicia una ejecución de la actualización, pero encuentra errores durante la ejecución.

#### <a name="cause"></a>Causa

Las posibles causas pueden ser:

* El estado del administrador de paquetes no es correcto
* Paquetes específicos pueden interferir con la aplicación de revisiones basada en la nube
* Otros motivos

#### <a name="resolution"></a>Resolución

Si se producen errores durante una ejecución de actualizaciones después de que se ha iniciado correctamente en Linux, compruebe el trabajo de salida desde la máquina afectada en la ejecución. Puede encontrar mensajes de error específicos procedentes del Administrador de paquetes de su máquina que puede investigar e intentar solucionar. Update Management requiere que el administrador de paquetes tenga un estado correcto para que las implementaciones de actualizaciones se realicen con éxito.

En algunos casos, las actualizaciones de paquetes pueden interferir con Update Management e impedir que finalice una implementación de actualizaciones. Si ese fuera el caso, deberá excluir estos paquetes de las futuras ejecuciones de actualizaciones o instalarlos manualmente por su cuenta.

Si no puede resolver un problema de aplicación de revisiones, realice una copia del archivo de registro siguiente y consérvela **antes** de que se inicie la siguiente implementación de actualizaciones para solucionar los problemas:

```
/var/opt/microsoft/omsagent/run/automationworker/omsupdatemgmt.log
```

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.