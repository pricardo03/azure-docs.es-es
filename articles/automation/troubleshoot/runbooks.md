---
title: Solución de problemas relativos a errores con runbooks de Azure Automation
description: Aprenda sobre la solución de problemas relacionados con los runbooks de Azure Automation.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 10/17/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 532d3d73c939a44678091734f2bbff22267ab6b7
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50094871"
---
# <a name="troubleshoot-errors-with-runbooks"></a>Solución de problemas relativos a errores con runbooks

## <a name="authentication-errors-when-working-with-azure-automation-runbooks"></a>Errores de autenticación al trabajar con runbooks de Azure Automation

### <a name="sign-in-failed"></a>Escenario: Fallo del inicio de sesión en la cuenta de Azure

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
2. Compruebe que puede utilizar el nombre de usuario y la contraseña que se almacenan en la credencial de Azure Automation en su editor local ISE de PowerShell. Puede comprobar si el nombre de usuario y la contraseña son correctos mediante la ejecución de los siguientes cmdlets en el ISE de PowerShell:  

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
2. Asegúrese de que ejecuta el cmdlet **Add-AzureAccount** antes de ejecutar el cmdlet **Select-AzureSubscription**.  
3. Si continúa recibiendo este mensaje de error, modifique el código agregando el parámetro **-AzureRmContext** a continuación del cmdlet **Add-AzureAccount** y luego ejecute el código.

   ```powershell
   $Conn = Get-AutomationConnection -Name AzureRunAsConnection
   Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
-ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

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

Para usar un certificado con los cmdlets del modelo de implementación clásica de Azure, consulte el artículo sobre [cómo crear y agregar un certificado para administrar los servicios de Azure](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx). Para usar una entidad de servicio con los cmdlets de Azure Resource Manager, consulte [Creación de aplicación de Active Directory y entidad de servicio mediante el portal](../../active-directory/develop/howto-create-service-principal-portal.md) y [Autenticación de una entidad de servicio con Azure Resource Manager](../../active-directory/develop/howto-authenticate-service-principal-powershell.md).

## <a name="common-errors-when-working-with-runbooks"></a>Errores comunes al trabajar con runbooks

### <a name="task-was-cancelled"></a>Escenario: El runbook genera el error: Se canceló una tarea

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

### <a name="child-runbook-auth-failure"></a>Escenario: Se produce un error en un runbook secundario cuando se trabaja con varias suscripciones

#### <a name="issue"></a>Problema

Al ejecutar runbooks secundarios con `Start-AzureRmRunbook`, el runbook secundario no puede administrar los recursos de Azure.

#### <a name="cause"></a>Causa

El runbook secundario no está utilizando el contexto correcto cuando se ejecuta.

#### <a name="resolution"></a>Resolución

Si trabaja con varias suscripciones, se puede perder el contexto de suscripción al invocar runbooks secundarios. Para asegurarse de que el contexto de suscripción se pasa a los runbooks secundarios, agregue el parámetro `AzureRmContext` al cmdlet y pase el contexto a él.

```azurepowershell-interactive
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

### <a name="job-attempted-3-times"></a>Escenario: se intentó iniciar el trabajo de runbook tres veces, pero en las tres se produjo un error

#### <a name="issue"></a>Problema

Se produce un error en el runbook con el error:

```
The job was tried three times but it failed
```

#### <a name="cause"></a>Causa

Este error puede deberse a las siguientes razones:

1. Límite de memoria. Se han documentado límites en la cantidad de memoria asignada a un espacio aislado con los [límites del servicio Automation](../../azure-subscription-service-limits.md#automation-limits), de modo que un trabajo puede producir un error, si está utilizando más de 400 MB de memoria.

1. Sockets de red. Los espacios aislados de Azure se limitan a 1000 sockets de red simultáneos como se describe en [Límites del servicio Automation](../../azure-subscription-service-limits.md#automation-limits).

1. Módulo incompatible. Este error puede ocurrir si las dependencias del módulo no son correctas. Si no lo son, el runbook normalmente devuelve un mensaje similar a "Comando no encontrado" o "No se puede enlazar el parámetro".

#### <a name="resolution"></a>Resolución

Cualquiera de las siguientes soluciones resolverá el problema:

* Algunos métodos sugeridos para trabajar dentro del límite de memoria son dividir la carga de trabajo entre varios runbooks, no procesar tantos datos en la memoria, no escribir el resultado innecesario de los runbooks o considerar cuántos puntos de control se escriben en los runbooks de flujo de trabajo de PowerShell. Puede usar el método clear, como `$myVar.clear()` para borrar la variable y usar `[GC]::Collect()` para ejecutar la recolección de elementos no utilizados inmediatamente, esto reducirá la superficie de memoria de su runbook durante el tiempo de ejecución.

* Actualice los módulos de Azure siguiendo los pasos de [Actualización de módulos de Azure PowerShell en Azure Automation](../automation-update-azure-modules.md).  

* Otra solución consiste en ejecutar el runbook en un [Hybrid Runbook Worker](../automation-hrw-run-runbooks.md). Los Hybrid Worker no están limitados por los límites de memoria y de red como lo están los espacios aislados de Azure.

### <a name="fails-deserialized-object"></a>Escenario: error en runbook debido a un objeto deserializado

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

### <a name="quota-exceeded"></a>Escenario: Error de trabajo de Runbook porque superó la cuota asignada

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

### <a name="cmdlet-not-recognized"></a>Escenario: No se reconoce el Cmdlet cuando se ejecuta un runbook

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

### <a name="long-running-runbook"></a>Escenario: un runbook de larga ejecución no se puede completar

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

Los Hybrid Worker no están limitados por el límite de runbook de tres horas de [distribución equilibrada](../automation-runbook-execution.md#fair-share) como los espacios aislados de Azure. Aunque los Hybrid Runbook Workers no están restringidos por el límite de distribución equilibrada de tres horas, los runbooks ejecutados en Hybrid Runbook Workers deben desarrollarse para admitir comportamientos de reinicio en caso de problemas inesperados con la infraestructura local.

Otra opción es optimizar el runbook mediante la creación de [runbooks secundarios](../automation-child-runbooks.md). Si el runbook secundario recorre en bucle la misma función en varios recursos, como una operación de base de datos en varias bases de datos, la función se puede mover a un runbook secundario. Cada uno de estos runbooks secundarios se ejecutan en paralelo en procesos independientes, lo que reduce el tiempo que el runbook principal tarda en completarse.

Los cmdlets de PowerShell que habilitan el escenario de runbook secundario son:

[Start-AzureRMAutomationRunbook](/powershell/module/AzureRM.Automation/Start-AzureRmAutomationRunbook): este cmdlet permite iniciar un runbook y pasar parámetros al mismo.

[Get-AzureRmAutomationJob](/powershell/module/azurerm.automation/get-azurermautomationjob): este cmdlet permite comprobar el estado del trabajo de cada elemento secundario si hay operaciones que deben realizarse después de que se complete el runbook secundario.

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

* Asegúrese de que el módulo sigue el formato siguiente: NombreMódulo.Zip **->** NombreMódulo o número de versión **->** (NombreMódulo.psm1, NombreMódulo.psd1)
* Abra el archivo. psd1 y compruebe si el módulo tiene dependencias. Si es así, cargue estos módulos en la cuenta de Automation.
* Asegúrese de que todos los archivos .dll a los que se hace referencia están presentes en la carpeta del módulo.

## <a name="next-steps"></a>Pasos siguientes

Si su problema no aparece o es incapaz de resolverlo, visite uno de nuestros canales para obtener ayuda adicional:

* Obtenga respuestas de expertos de Azure en los [foros de Azure](https://azure.microsoft.com/support/forums/).
* Póngase en contacto con [@AzureSupport](https://twitter.com/azuresupport): la cuenta de Microsoft Azure oficial para mejorar la experiencia del cliente, que pone en contacto a la comunidad de Azure con los recursos adecuados: respuestas, soporte técnico y expertos.
* Si necesita más ayuda, puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y seleccione **Obtener soporte**.