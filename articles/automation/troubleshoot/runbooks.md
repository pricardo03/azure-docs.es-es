---
title: Solución de problemas relativos a errores con runbooks de Azure Automation
description: Aprenda sobre la solución de problemas relacionados con los runbooks de Azure Automation.
services: automation
author: georgewallace
ms.author: gwallace
ms.date: 07/13/2018
ms.topic: conceptual
ms.service: automation
manager: carmonm
ms.openlocfilehash: 48b2aab9d2a3937fb53a2e63efa26efc18a894f8
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413865"
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

1. Asegúrese de que el nombre de activo de la credencial de Automation que use para conectarse a Azure no contenga caracteres especiales, incluido el carácter **@** .  
2. Compruebe que puede utilizar el nombre de usuario y la contraseña que se almacenan en la credencial de Azure Automation en su editor local ISE de PowerShell. Puede hacerlo ejecutando los siguientes cmdlets en el ISE de PowerShell:  

   ```powershell
   $Cred = Get-Credential  
   #Using Azure Service Management   
   Add-AzureAccount –Credential $Cred  
   #Using Azure Resource Manager  
   Connect-AzureRmAccount –Credential $Cred
   ```

3. Si la autenticación falla localmente, esto significa que no ha configurado correctamente las credenciales de Azure Active Directory. Vea la entrada de blog [Authenticating to Azure using Azure Active Directory (Autenticación en Azure mediante Azure Active Directory)](https://azure.microsoft.com/blog/azure-automation-authenticating-to-azure-using-azure-active-directory/) para conseguir configurar correctamente la cuenta de Active Directory.  

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

1. Asegúrese de que ejecuta **Add-AzureAccount** antes de ejecutar el cmdlet **Select-AzureSubscription**.  
2. Si continúa recibiendo este mensaje de error, modifique el código agregando el cmdlet **Get-AzureSubscription** a continuación del cmdlet **Add-AzureAccount** y luego ejecute el código. Ahora compruebe si la salida de Get-AzureSubscription contiene los detalles de su suscripción.  

   * Si no ve los detalles de la suscripción en la salida, esto significa que la suscripción no se ha inicializado todavía.  
   * Si ve los detalles de suscripción en la salida, confirme que está usando el nombre o el identificador correctos de la suscripción con el cmdlet **Select-AzureSubscription** .

### <a name="auth-failed-mfa"></a>Escenario: Error de autenticación en Azure porque está habilitada la autenticación multifactor

#### <a name="issue"></a>Problema

Recibirá el siguiente error al autenticarse en Azure con el nombre de usuario y la contraseña de Azure:

```
Add-AzureAccount: AADSTS50079: Strong authentication enrollment (proof-up) is required
```

#### <a name="cause"></a>Causa

Si dispone de autenticación multifactor en su cuenta de Azure, no puede usar un usuario de Azure Active Directory para autenticarse en Azure. En su lugar, tiene que utilizar un certificado o una entidad de servicio para autenticarse en Azure.

#### <a name="resolution"></a>Resolución

Para usar un certificado con los cmdlets del modelo de implementación clásica de Azure, consulte el artículo sobre [cómo crear y agregar un certificado para administrar los servicios de Azure](http://blogs.technet.com/b/orchestrator/archive/2014/04/11/managing-azure-services-with-the-microsoft-azure-automation-preview-service.aspx). Para usar una entidad de servicio con los cmdlets de Azure Resource Manager, consulte [Creación de aplicación de Active Directory y entidad de servicio mediante el portal](../../azure-resource-manager/resource-group-create-service-principal-portal.md) y [Autenticación de una entidad de servicio con Azure Resource Manager](../../azure-resource-manager/resource-group-authenticate-service-principal.md).

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

En su cuenta de Automation, haga clic en **Módulos** y en **Actualizar módulos de Azure**. La actualización tarda aproximadamente 15 minutos, una vez que se ha vuelto a ejecutar el runbook que producía un error.

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

2. Módulo incompatible. Puede ocurrir si las dependencias del módulo no son correctas. En este caso, el runbook normalmente devuelve un mensaje similar a "Comando no encontrado" o "No se puede enlazar el parámetro".

#### <a name="resolution"></a>Resolución

Cualquiera de las siguientes soluciones resolverá el problema:

* Algunos métodos sugeridos para trabajar dentro del límite de memoria son dividir la carga de trabajo entre varios runbooks, no procesar tantos datos en la memoria, no escribir el resultado innecesario de los runbooks o considerar cuántos puntos de control se escriben en los runbooks de flujo de trabajo de PowerShell.  

* Actualice los módulos de Azure siguiendo los pasos de [Actualización de módulos de Azure PowerShell en Azure Automation](../automation-update-azure-modules.md).  

### <a name="fails-deserialized-object"></a>Escenario: error en runbook debido a un objeto deserializado

#### <a name="issue"></a>Problema

Se produce un error en el runbook con el error:

```
Cannot bind parameter <ParameterName>.

Cannot convert the <ParameterType> value of type Deserialized <ParameterType> to type <ParameterType>.
```

#### <a name="cause"></a>Causa

Si el runbook es un flujo de trabajo de PowerShell almacena objetos complejos en un formato deserializado, para conservar el estado del Runbook si se suspende el flujo de trabajo.

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

Este error se produce cuando el motor de PowerShell no puede encontrar el cmdlet que está usando en su Runbook. Esto podría deberse a que el módulo que contiene el cmdlet no está presente en la cuenta, a que haya un conflicto de nombres con un nombre de runbook o a que el cmdlet también existe en otro módulo y Automation no puede resolver el nombre.

#### <a name="resolution"></a>Resolución

Cualquiera de las siguientes soluciones resolverá el problema:  

* Compruebe que ha escrito correctamente el nombre del cmdlet.  
* Asegúrese de que el cmdlet existe en su cuenta de Automation y de que no hay ningún conflicto. Para comprobar si está presente el cmdlet, abra un runbook en modo de edición y busque el cmdlet que quiere encontrar en la biblioteca o ejecute `Get-Command <CommandName>`. Una vez que haya comprobado que el cmdlet está disponible para la cuenta y que no hay conflictos de nombres con otros cmdlets o runbooks, agréguelo al lienzo y asegúrese de que está utilizando un parámetro válido establecido en su runbook.  
* Si tiene un conflicto de nombres y el cmdlet está disponible en dos módulos diferentes, puede resolver este problema mediante el nombre completo del cmdlet. Por ejemplo, puede usar **NombreDeMódulo\NombredeCmdlet**.  
* Si está ejecutando el runbook local en un grupo de trabajo híbrido, asegúrese de que el cmdlet o módulo está instalado en el equipo que hospeda el trabajo híbrido.

### <a name="evicted-from-checkpoint"></a>Escenario: un runbook de larga duración presenta errores constantemente con la excepción: "El trabajo no se puede seguir ejecutando porque se expulsó repetidamente desde el mismo punto de control"

#### <a name="issue"></a>Problema

Por naturaleza, este comportamiento se debe a la supervisión de "reparto justo" de los procesos de Azure Automation, que suspende automáticamente un runbook si se ejecuta durante más de tres horas. Sin embargo, el mensaje de error devuelto no proporciona opciones para qué hacer a continuación.

#### <a name="cause"></a>Causa

Un runbook se puede suspender por varios motivos. Las suspensiones se producen principalmente debido a errores. Por ejemplo, una excepción no detectada en un runbook, un error de red o un bloqueo en el servicio Runbook Worker que ejecuta el runbook hará que el runbook se suspenda y se inicie desde su último punto de control cuando se reanude.

#### <a name="resolution"></a>Resolución

La solución documentada para evitar este problema consiste en usar puntos de control en un flujo de trabajo. Para más información, consulte el artículo con [información sobre los flujos de trabajo de PowerShell](../automation-powershell-workflow.md#checkpoints). Encontrará una explicación más completa del "reparto equitativo" y los puntos de control en este artículo de blog [Using Checkpoints in Runbooks](https://azure.microsoft.com/blog/azure-automation-reliable-fault-tolerant-runbook-execution-using-checkpoints/)(Uso de puntos de control en los runbooks).

### <a name="long-running-runbook"></a>Escenario: un runbook de larga ejecución no se puede completar

#### <a name="issue"></a>Problema

Por naturaleza, este comportamiento en espacios aislados de Azure se debe a la supervisión de "distribución equilibrada" de los procesos de Azure Automation, que suspende automáticamente un runbook si se ejecuta durante más de tres horas.

#### <a name="cause"></a>Causa

El runbook se ejecutó por encima del límite de tres horas permitido por la distribución equilibrada en un espacio aislado de Azure

#### <a name="resolution"></a>Resolución

La solución recomendada consiste en ejecutar el runbook en un [Hybrid Runbook Worker](../automation-hrw-run-runbooks.md). Los Hybrid Worker no están limitados por el límite de runbook de tres horas de [distribución equilibrada](../automation-runbook-execution.md#fair-share) como los espacios aislados de Azure.

## <a name="common-errors-when-importing-modules"></a>Errores comunes al importar módulos

### <a name="module-fails-to-import"></a>Escenario: No se puede importar el módulo o no se pueden ejecutar cmdlets después de la importación

#### <a name="issue"></a>Problema

Un módulo no se puede importar o se importa correctamente, pero no se extrae ningún cmdlet.

#### <a name="cause"></a>Causa

Algunas razones comunes por las que un módulo no se importa correctamente a Azure Automation son:

* La estructura no coincide con la estructura que Automation necesita.
* El módulo depende de otro módulo que no se ha implementado en su cuenta de Automation.
* Al módulo le faltan sus dependencias en la carpeta.
* El cmdlet `New-AzureRmAutomationModule` se está usando para cargar el módulo y no se ha proporcionado la ruta de acceso de almacenamiento completa o no se ha cargado el módulo usando una URL de acceso público.

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