---
title: Solución de problemas relativos a errores con runbooks de Azure Automation
description: Aprenda sobre la solución de problemas relacionados con los runbooks de Azure Automation.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 01/04/2019
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 3968b05f119227552f88a50e96d3acbce6a19143
ms.sourcegitcommit: d4f728095cf52b109b3117be9059809c12b69e32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/10/2019
ms.locfileid: "54199126"
---
# <a name="troubleshoot-errors-with-runbooks"></a>Solución de problemas relativos a errores con runbooks

## <a name="authentication-errors-when-working-with-azure-automation-runbooks"></a>Errores de autenticación al trabajar con runbooks de Azure Automation

### <a name="sign-in-failed"></a>Escenario: Error al iniciar de sesión en la cuenta de Azure

#### <a name="issue"></a>Problema

Recibirá el siguiente error al trabajar con los cmdlets `Add-AzureAccount` o `Connect-AzureRmAccount`.
:

```
Unknown_user_type: Unknown User Type
```

#### <a name="cause"></a>Causa

Este error se produce si el nombre de activo de credencial no es válido o si el nombre de usuario y la contraseña que usó para configurar el activo de credencial de Automation no son válidos.

#### <a name="resolution"></a>Resolución

Para determinar cuál es el problema, siga estos pasos:  

1. Asegúrese de que el nombre de activo de la credencial de Automation que use para conectarse a Azure no contenga caracteres especiales, incluido el carácter **@**.  
2. Compruebe que puede usar el nombre de usuario y la contraseña que se almacenan en la credencial de Azure Automation en su editor local de ISE de PowerShell. Puede comprobar si el nombre de usuario y la contraseña son correctos mediante la ejecución de los siguientes cmdlets en el ISE de PowerShell:  

   ```powershell
   $Cred = Get-Credential  
   #Using Azure Service Management
   Add-AzureAccount –Credential $Cred  
   #Using Azure Resource Manager  
   Connect-AzureRmAccount –Credential $Cred
   ```

3. Si la autenticación falla localmente, esto significa que no ha configurado correctamente las credenciales de Azure Active Directory. Vea la entrada de blog [Authenticating to Azure using Azure Active Directory (Autenticación en Azure mediante Azure Active Directory)](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) para conseguir configurar correctamente la cuenta de Active Directory.  

4. Si parece un error transitorio, intente agregar lógica de reintento a su rutina de autenticación para hacer de la autenticación un proceso más sólido.

   ```powershell
   # Get the connection "AzureRunAsConnection"
   $connectionName = "AzureRunAsConnection"
   $servicePrincipalConnection = Get-AutomationConnection -Name $connectionName

   $logonAttempt = 0
   $logonResult = $False

   while(!($connectionResult) -And ($logonAttempt -le 10))
   {
   $LogonAttempt++
   # Logging in to Azure...
   $connectionResult = Connect-AzureRmAccount `
      -ServicePrincipal `
      -TenantId $servicePrincipalConnection.TenantId `
      -ApplicationId $servicePrincipalConnection.ApplicationId `
      -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint

   Start-Sleep -Seconds 30
   }
   ```

### <a name="unable-to-find-subscription"></a>Escenario: No se encuentra la suscripción de Azure

#### <a name="issue"></a>Problema

Recibirá el siguiente error al trabajar con los cmdlets `Select-AzureSubscription` o `Select-AzureRmSubscription`:

```
The subscription named <subscription name> cannot be found.
```

#### <a name="error"></a>Error

Este error se produce si el nombre de la suscripción no es válido o si el usuario de Azure Active Directory que está intentando obtener los detalles de suscripción no está configurado como un administrador de la suscripción.

#### <a name="resolution"></a>Resolución

Para determinar si se ha autenticado correctamente en Azure y tener acceso a la suscripción que intenta seleccionar, siga estos pasos:  

1. Pruebe el script fuera de Azure Automation para asegurarse de que funciona de forma independiente.
2. Asegúrese de que ejecuta el cmdlet `Add-AzureAccount` antes que el cmdlet `Select-AzureSubscription`. 
3. Agregue `Disable-AzureRmContextAutosave –Scope Process` al principio del runbook. De esta forma se garantiza que las credenciales solo se aplican a la ejecución del runbook actual.
4. Si continúa recibiendo este mensaje de error, modifique el código; para ello, agregue el parámetro **-AzureRmContext** a continuación del cmdlet `Add-AzureAccount` y luego ejecute el código.

   ```powershell
   Disable-AzureRmContextAutosave –Scope Process

   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

   $context = Get-AzureRmContext

   Get-AzureRmVM -ResourceGroupName myResourceGroup -AzureRmContext $context
    ```

### <a name="auth-failed-mfa"></a>Escenario: Error de autenticación en Azure porque está habilitada la autenticación multifactor

#### <a name="issue"></a>Problema

Recibirá el siguiente error al autenticarse en Azure con el nombre de usuario y la contraseña de Azure:

```
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

#### <a name="cause"></a>Causa

Si dispone de autenticación multifactor en su cuenta de Azure, no puede usar un usuario de Azure Active Directory para autenticarse en Azure. En su lugar, tiene que utilizar un certificado o una entidad de servicio para autenticarse en Azure.

#### <a name="resolution"></a>Resolución

Para usar un certificado con los cmdlets del modelo de implementación clásica de Azure, consulte el artículo sobre [cómo crear y agregar un certificado para administrar los servicios de Azure](https://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx). Para usar una entidad de servicio con los cmdlets de Azure Resource Manager, consulte [Creación de aplicación de Active Directory y entidad de servicio mediante el portal](../../active-directory/develop/howto-create-service-principal-portal.md) y [Autenticación de una entidad de servicio con Azure Resource Manager](../../active-directory/develop/howto-authenticate-service-principal-powershell.md).

## <a name="common-errors-when-working-with-runbooks"></a>Errores comunes al trabajar con runbooks

### <a name="task-was-cancelled"></a>Escenario: Se produce el siguiente error en el runbook: Se ha cancelado una tarea

#### <a name="issue"></a>Problema

El runbook genera un error similar al ejemplo siguiente:

```
Exception: A task was canceled.
```

#### <a name="cause"></a>Causa

Este error puede deberse al uso de módulos de Azure obsoletos.

#### <a name="resolution"></a>Resolución

Este error se puede resolver con la actualización de los módulos de Azure a la versión más reciente.

En su cuenta de Automation, haga clic en **Módulos** y en **Actualizar módulos de Azure**. La actualización tarda aproximadamente 15 minutos, una vez que se ha vuelto a ejecutar el runbook que producía un error. Para más información acerca de cómo actualizar los módulos, consulte [Actualización de módulos de Azure en Azure Automation](../automation-update-azure-modules.md).

### <a name="runbook-auth-failure"></a>Escenario: se produce un error en los runbooks cuando se trabaja con varias suscripciones

#### <a name="issue"></a>Problema

Al ejecutar runbooks con `Start-AzureRmAutomationRunbook`, el runbook no puede administrar los recursos de Azure.

#### <a name="cause"></a>Causa

El runbook no está utilizando el contexto correcto cuando se ejecuta.

#### <a name="resolution"></a>Resolución

Si trabaja con varias suscripciones, se puede perder el contexto de la suscripción al invocar runbooks. Para asegurarse de que el contexto de la suscripción se pasa a los runbooks, agregue el parámetro `AzureRmContext` al cmdlet y pase el contexto a él. También se recomienda usar el cmdlet `Disable-AzureRmContextAutosave` con el ámbito **Proceso** para asegurarse de que las credenciales que usa solo se emplean con el runbook actual.

```azurepowershell-interactive
# Ensures that any credentials apply only to the execution of this runbook
Disable-AzureRmContextAutosave –Scope Process

# Connect to Azure with RunAs account
$ServicePrincipalConnection = Get-AutomationConnection -Name 'AzureRunAsConnection'

Add-AzureRmAccount `
    -ServicePrincipal `
    -TenantId $ServicePrincipalConnection.TenantId `
    -ApplicationId $ServicePrincipalConnection.ApplicationId `
    -CertificateThumbprint $ServicePrincipalConnection.CertificateThumbprint

$AzureContext = Select-AzureRmSubscription -SubscriptionId $ServicePrincipalConnection.SubscriptionID

$params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true}

Start-AzureRmAutomationRunbook `
    –AutomationAccountName 'MyAutomationAccount' `
    –Name 'Test-ChildRunbook' `
    -ResourceGroupName 'LabRG' `
    -AzureRmContext $AzureContext `
    –Parameters $params –wait
```

### <a name="not-recognized-as-cmdlet"></a>Escenario: Se produce un error en el runbook debido a un cmdlet que falta

#### <a name="issue"></a>Problema

El runbook genera un error similar al ejemplo siguiente:

```
The term 'Connect-AzureRmAccount' is not recognized as the name of a cmdlet, function, script file, or operable program.  Check the spelling of the name, or if the path was included verify that the path is correct and try again.
```

#### <a name="cause"></a>Causa

Este error puede deberse a las siguientes razones:

1. El módulo que contiene el cmdlet no se importa en la cuenta de Automation.
2. El módulo que contiene el cmdlet se importa, pero no está actualizado.

#### <a name="resolution"></a>Resolución

Este error puede resolverse realizando una de las siguientes tareas:

Si el módulo es un módulo de Azure, consulte [Actualización de módulos de Azure PowerShell en Azure Automation](../automation-update-azure-modules.md) para aprender a actualizar los módulos en su cuenta de Automation.

Si es un módulo independiente, asegúrese de que el módulo se importa en su cuenta de Automation.

### <a name="job-attempted-3-times"></a>Escenario: Se intentó iniciar el trabajo de runbook tres veces, pero en las tres se produjo un error

#### <a name="issue"></a>Problema

Se produce un error en el runbook con el error:

```
The job was tried three times but it failed
```

#### <a name="cause"></a>Causa

Este error puede deberse a las siguientes razones:

1. Límite de memoria. Los límites documentados sobre la cantidad de memoria que se asigna a un espacio aislado se encuentran en [Límites del servicio Automation](../../azure-subscription-service-limits.md#automation-limits). Un trabajo puede producir un error si emplea más de 400 MB de memoria.

2. Sockets de red. Los espacios aislados de Azure se limitan a 1000 sockets de red simultáneos como se describe en [Límites del servicio Automation](../../azure-subscription-service-limits.md#automation-limits).

3. Módulo incompatible. Este error puede ocurrir si las dependencias del módulo no son correctas. Si no lo son, el runbook normalmente devuelve un mensaje similar a "Comando no encontrado" o "No se puede enlazar el parámetro".

#### <a name="resolution"></a>Resolución

Cualquiera de las siguientes soluciones resolverá el problema:

* Algunos métodos sugeridos para trabajar dentro del límite de memoria son dividir la carga de trabajo entre varios runbooks, no procesar tantos datos en la memoria, no escribir el resultado innecesario de los runbooks o considerar cuántos puntos de control se escriben en los runbooks de flujo de trabajo de PowerShell. Puede usar el método clear, como `$myVar.clear()` para borrar la variable y usar `[GC]::Collect()` para ejecutar la recolección de elementos no utilizados inmediatamente, esto reducirá la superficie de memoria de su runbook durante el tiempo de ejecución.

* Actualice los módulos de Azure siguiendo los pasos de [Actualización de módulos de Azure PowerShell en Azure Automation](../automation-update-azure-modules.md).  

* Otra solución consiste en ejecutar el runbook en un [Hybrid Runbook Worker](../automation-hrw-run-runbooks.md). Los Hybrid Worker no están limitados por los límites de memoria y de red como lo están los espacios aislados de Azure.

### <a name="fails-deserialized-object"></a>Escenario: Error en runbook debido a un objeto deserializado

#### <a name="issue"></a>Problema

Se produce un error en el runbook con el error:

```
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

#### <a name="cause"></a>Causa

Si el runbook es un flujo de trabajo de PowerShell, almacena objetos complejos en un formato deserializado para conservar el estado del Runbook si se suspende el flujo de trabajo.

#### <a name="resolution"></a>Resolución

Cualquiera de las siguientes tres alternativas solucionará este problema:

1. Si canaliza objetos complejos de un cmdlet a otro, encapsule estos cmdlets en un InlineScript.
2. En lugar de pasar el objeto complejo entero, pase solamente el nombre o valor del mismo que necesite.
3. Use un runbook de PowerShell en lugar de un runbook de flujo de trabajo de PowerShell.

### <a name="quota-exceeded"></a>Escenario: Error de trabajo de runbook porque superó la cuota asignada

#### <a name="issue"></a>Problema

Se produce un error en el trabajo de runbook con el error:

```
The quota for the monthly total job run time has been reached for this subscription
```

#### <a name="cause"></a>Causa

Este error se produce cuando la ejecución del trabajo supera la cuota gratuita de 500 minutos para su cuenta. Esta cuota se aplica a todos los tipos de tareas de ejecución de trabajo como realizar pruebas de un trabajo, iniciar un trabajo desde el portal, ejecutar un trabajo usando Webhook y programar un trabajo para ejecutar mediante el Portal de Azure o en su centro de datos. Para obtener más información sobre precios para, consulte [Precios de Automation](https://azure.microsoft.com/pricing/details/automation/).

#### <a name="resolution"></a>Resolución

Si desea usar más de 500 minutos de procesamiento por mes tiene que cambiar la suscripción del nivel Gratis al nivel Básico. Puede actualizar al nivel Básico realizando los pasos siguientes:  

1. Inicie sesión en la suscripción de Azure  
2. Seleccione la cuenta de Automation que desee actualizar  
3. Haga clic en **Configuración** > **Precios**.
4. Haga clic en **Habilitar** en la parte inferior de la página para actualizar su cuenta al nivel **Básico**.

### <a name="cmdlet-not-recognized"></a>Escenario: No se reconoce el cmdlet cuando se ejecuta un runbook

#### <a name="issue"></a>Problema

Se produce un error en el trabajo de runbook con el error:

```
<cmdlet name>: The term <cmdlet name> is not recognized as the name of a cmdlet, function, script file, or operable program.
```

#### <a name="cause"></a>Causa

Este error se produce cuando el motor de PowerShell no puede encontrar el cmdlet que está usando en su runbook. Esto podría deberse a que el módulo que contiene el cmdlet no está presente en la cuenta, a que haya un conflicto de nombres con un nombre de runbook o a que el cmdlet también existe en otro módulo y Automation no puede resolver el nombre.

#### <a name="resolution"></a>Resolución

Cualquiera de las siguientes soluciones resolverá el problema:  

* Compruebe que ha escrito correctamente el nombre del cmdlet.  
* Asegúrese de que el cmdlet existe en su cuenta de Automation y de que no hay ningún conflicto. Para comprobar si está presente el cmdlet, abra un runbook en modo de edición y busque el cmdlet que quiere encontrar en la biblioteca o ejecute `Get-Command <CommandName>`. Una vez que haya comprobado que el cmdlet está disponible para la cuenta y que no hay conflictos de nombres con otros cmdlets o runbooks, agréguelo al lienzo y asegúrese de que está utilizando un parámetro válido establecido en su runbook.  
* Si tiene un conflicto de nombres y el cmdlet está disponible en dos módulos diferentes, puede resolver este problema mediante el nombre completo del cmdlet. Por ejemplo, puede usar **NombreDeMódulo\NombredeCmdlet**.  
* Si está ejecutando el runbook en un entorno local en un grupo de trabajo híbrido, asegúrese de que el cmdlet o el módulo están instalados en la máquina que hospeda el trabajo híbrido.

### <a name="long-running-runbook"></a>Escenario: Un runbook de larga ejecución no se puede completar

#### <a name="issue"></a>Problema

El runbook muestra un estado **Detenido** después de ejecutarse durante tres horas. También puede recibir el error:

```
The job was evicted and subsequently reached a Stopped state. The job cannot continue running
```

Por naturaleza, este comportamiento en espacios aislados de Azure se debe a la supervisión de "distribución equilibrada" de los procesos de Azure Automation, que detiene automáticamente un runbook si se ejecuta durante más de tres horas. El estado de un runbook que va más allá del límite de tiempo de "distribución equilibrada" varía según el tipo de runbook. Runbooks de PowerShell y Python se establecen en un estado **Detenido**. Los runbooks del flujo de trabajo de PowerShell se establecen en **Error**.

#### <a name="cause"></a>Causa

El runbook se ejecutó por encima del límite de tres horas permitido por la distribución equilibrada en un espacio aislado de Azure.

#### <a name="resolution"></a>Resolución

La solución recomendada consiste en ejecutar el runbook en un [Hybrid Runbook Worker](../automation-hrw-run-runbooks.md).

Los Hybrid Worker no están limitados por el límite de runbook de tres horas de [distribución equilibrada](../automation-runbook-execution.md#fair-share) como los espacios aislados de Azure. Aunque los Hybrid Runbook Workers no están restringidos por el límite de uso compartido equitativo de tres horas, los runbooks ejecutados en ellos deben desarrollarse para admitir comportamientos de reinicio en caso de problemas inesperados con la infraestructura local.

Otra opción es optimizar el runbook mediante la creación de [runbooks secundarios](../automation-child-runbooks.md). Si el runbook secundario recorre en bucle la misma función en varios recursos, como una operación de base de datos en varias bases de datos, la función se puede mover a un runbook secundario. Cada uno de estos runbooks secundarios se ejecuta en paralelo en procesos independientes. Este comportamiento reduce la cantidad total de tiempo que tarda en completarse el runbook primario.

Los cmdlets de PowerShell que habilitan el escenario de runbook secundario son:

[Start-AzureRMAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook): este cmdlet permite iniciar un runbook y pasar parámetros al mismo.

[Get-AzureRmAutomationJob](/powershell/module/azurerm.automation/get-azurermautomationjob): este cmdlet permite comprobar el estado del trabajo de cada elemento secundario si hay operaciones que deben realizarse después de que se complete el runbook secundario.

### <a name="expired webhook"></a>Escenario: Estado: Solicitud incorrecta 400 al invocar un webhook

#### <a name="issue"></a>Problema

Cuando intenta invocar un webhook para un runbook de Azure Automation, recibe el siguiente error.

```error
400 Bad Request : This webhook has expired or is disabled
```

#### <a name="cause"></a>Causa

El webhook que intenta invocar está deshabilitado o ha expirado.

#### <a name="resolution"></a>Resolución

Si el webhook está deshabilitado, puede volver a habilitarlo a través de Azure Portal. Si el webhook ha expirado, debe eliminarse y crearse de nuevo. Solo puede [renovar un webhook](../automation-webhooks.md#renew-webhook) si ya no ha expirado.

### <a name="429"></a>Escenario: 429: Actualmente la tasa de solicitud es demasiado grande. Vuelva a intentarlo.

#### <a name="issue"></a>Problema

Recibe el siguiente mensaje de error al ejecutar el cmdlet `Get-AzureRmAutomationJobOutput`:

```
429: The request rate is currently too large. Please try again
```

#### <a name="cause"></a>Causa

Este error puede producirse al recuperar la salida de trabajo de un runbook que tiene muchas [flujos detallados](../automation-runbook-output-and-messages.md#verbose-stream).

#### <a name="resolution"></a>Resolución

Hay dos maneras de resolver este error:

* Edite el runbook y reduzca el número de flujos de trabajo que emite.
* Reduzca el número de flujos para recuperar cuando se ejecuta el cmdlet. Para seguir este comportamiento, puede especificar el parámetro `-Stream Output` para el cmdlet `Get-AzureRmAutomationJobOutput` a fin de recuperar solo los flujos de salida. 

## <a name="common-errors-when-importing-modules"></a>Errores comunes al importar módulos

### <a name="module-fails-to-import"></a>Escenario: No se puede importar el módulo o no se pueden ejecutar cmdlets después de la importación

#### <a name="issue"></a>Problema

Un módulo no se puede importar o se importa correctamente, pero no se extrae ningún cmdlet.

#### <a name="cause"></a>Causa

Algunas razones comunes por las que un módulo no se importa correctamente a Azure Automation son:

* La estructura no coincide con la estructura que Automation necesita.
* El módulo depende de otro módulo que no se ha implementado en su cuenta de Automation.
* Al módulo le faltan sus dependencias en la carpeta.
* El cmdlet `New-AzureRmAutomationModule` se está usando para cargar el módulo y no se ha proporcionado la ruta de acceso de almacenamiento completa o no se ha cargado el módulo usando una dirección URL de acceso público.

#### <a name="resolution"></a>Resolución

Cualquiera de las siguientes soluciones resolverá el problema:

* Asegúrese de que el módulo sigue el formato siguiente: nombreDeMódulo.Zip **->** nombreDeMódulo o un número de versión **->** (nombreDeMódulo.psm1, nombreDeMódulo.psd1)
* Abra el archivo. psd1 y compruebe si el módulo tiene dependencias. Si es así, cargue estos módulos en la cuenta de Automation.
* Asegúrese de que todos los archivos .dll a los que se hace referencia están presentes en la carpeta del módulo.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.