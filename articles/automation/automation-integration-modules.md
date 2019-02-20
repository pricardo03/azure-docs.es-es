---
title: Creación de un módulo de integración de Azure Automation
description: En este tutorial se explica cómo crear y probar módulos de integración en Azure Automation y se proporcionan ejemplos de uso.
services: automation
ms.service: automation
ms.subservice: shared-capabilities
author: georgewallace
ms.author: gwallace
ms.date: 01/24/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9122cf5cc908d578d8b781c6fdc49d7b04b0ab58
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2019
ms.locfileid: "55990349"
---
# <a name="azure-automation-integration-modules"></a>Módulos de integración de Azure Automation

PowerShell es la principal tecnología que se esconde detrás de Azure Automation. Desde que Azure Automation se integró en PowerShell, los módulos de PowerShell resultan clave para la extensibilidad de Azure Automation. En este artículo, obtendrá información sobre los detalles del uso de Azure Automation de módulos de PowerShell, que se conoce como Módulos de integración. También aprenderá las prácticas recomendadas para crear sus propios módulos de PowerShell para asegurarse de que funcionan como módulos de integración en Azure Automation. 

## <a name="what-is-a-powershell-module"></a>¿Qué son los módulos de PowerShell?

Un módulo de PowerShell es un grupo de cmdlets de PowerShell como **Get-Date** o **Copy-Item**, que puede usarse desde:

* La consola de PowerShell
* scripts
* workflows
* runbooks
* Recursos de DSC

Toda la funcionalidad de PowerShell se expone a través de los cmdlets y recursos de DSC. Cada cmdlet y recurso de DSC está respaldado por un módulo de PowerShell, muchos de los cuales se suministran con PowerShell. Por ejemplo, el cmdlet **Get-Date** forma parte del módulo `Microsoft.PowerShell.Utility` de PowerShell, el cmdlet **Copy-Item** forma parte del módulo `Microsoft.PowerShell.Management` de PowerShell y el recurso Package de DSC forma parte del módulo PSDesiredStateConfiguration de PowerShell. Estos módulos se suministran con PowerShell. Muchos de los módulos de PowerShell no se incluyen como parte de PowerShell. Estos módulos se distribuyen con productos de primeros o terceros o en lugares como la Galería de PowerShell. Los módulos resultan muy útiles, ya que simplifican las tareas complejas gracias a su funcionalidad encapsulada.  Puede obtener más información sobre los [módulos de PowerShell en MSDN](https://msdn.microsoft.com/library/dd878324%28v=vs.85%29.aspx). 

## <a name="what-is-an-azure-automation-integration-module"></a>¿Qué son los módulos de integración de Azure Automation?

Los módulos de integración se parecen mucho a los módulos de PowerShell. Sencillamente, se trata de módulos de PowerShell que, de manera opcional, pueden contener un archivo adicional, un archivo de metadatos que especifica un tipo de conexión de Azure Automation que se va a utilizar con los cmdlets del módulo en los runbooks. Los módulos de PowerShell pueden importarse en Azure Automation para que sus cmdlets estén disponibles y puedan usarse en runbooks y para que sus recursos de DSC estén disponibles y puedan usarse en las configuraciones de DSC. En segundo plano, Azure Automation almacena estos módulos y en el tiempo de ejecución del trabajo de compilación de DSC y del trabajo de runbook, los carga en espacios aislados de Azure Automation, donde se ejecutan los runbooks y se compilan las configuraciones de DSC. Todos los recursos de DSC en módulos se sitúan automáticamente en el servidor de extracción de DSC de Automatización. Las máquinas los pueden extraer cuando se aplican las configuraciones de DSC.  

Incluimos varios módulos de Azure PowerShell para que pueda usarlos desde el primer momento en Azure Automation. Pero puede importar módulos de PowerShell para cualquier sistema, servicio o herramienta con que desee integrarlo.

> [!NOTE]
> Algunos módulos se suministran como **módulos globales** en el servicio Automation. Estos módulos globales están disponibles cuando se crea una cuenta de automatización y los actualizamos a veces, lo que los expulsa automáticamente de su cuenta de Automation. Si no desea que se actualicen automáticamente, siempre puede importar el mismo módulo, que tendrá prioridad sobre la versión global de dicho módulo que se incluye en el servicio.

El formato para importar un paquete de módulo de integración es un archivo comprimido que tiene el mismo nombre del módulo y una extensión .zip. El paquete contiene el módulo de Windows PowerShell y todos los archivos auxiliares, como el archivo de manifiesto (.psd1), si es que el módulo tiene uno.

Si el módulo debe contener un tipo de conexión de Azure Automation, también debe incluir un archivo llamado `<ModuleName>-Automation.json` que especifique las propiedades del tipo de conexión. Se trata de un archivo json, que se coloca dentro de la carpeta del módulo del archivo .zip comprimido. Este archivo contiene los campos de una **conexión** que se necesita para conectarse al sistema o servicio que representa el módulo. Esta configuración finaliza la creación de un tipo de conexión en Azure Automation. Con este archivo, puede configurar los nombres y los tipos de campo del tipo de conexión del módulo, además de especificar si los campos deben cifrarse o el cifrado es opcional. El ejemplo siguiente incluye una plantilla con el formato del archivo json:

```json
{ 
   "ConnectionFields": [
   {
      "IsEncrypted":  false,
      "IsOptional":  false,
      "Name":  "ComputerName",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  false,
      "IsOptional":  true,
      "Name":  "Username",
      "TypeName":  "System.String"
   },
   {
      "IsEncrypted":  true,
      "IsOptional":  false,
      "Name":  "Password",
   "TypeName":  "System.String"
   }],
   "ConnectionTypeName":  "DataProtectionManager",
   "IntegrationModuleName":  "DataProtectionManager"
}
```

Si ha implementado Service Management Automation y ha creado paquetes de módulos de integración para los runbooks de automatización, esta plantilla le resultará familiar.

## <a name="authoring-best-practices"></a>Procedimientos recomendados sobre la creación de los módulos

Aunque los módulos de integración son módulos de PowerShell, hay una serie de elementos que debe tener en cuenta al crear un módulo de PowerShell para su uso en Azure Automation. Algunas recomendaciones son útiles para que los módulos funcionen bien en el flujo de trabajo de PowerShell.

1. Incluya una sinopsis, una descripción y un identificador URI de ayuda para todos los cmdlets del módulo. En PowerShell, puede definir cierta información de ayuda para los cmdlets de forma que al usuario le resulte más fácil usarlos con el cmdlet **Get-Help** . Por ejemplo, a continuación le mostramos cómo puede definir una sinopsis y un identificador URI de ayuda para un módulo de PowerShell escrito en un archivo .psm1. 
   
    ```powershell
    <#
        .SYNOPSIS
         Gets all outgoing phone numbers for this Twilio account 
    #>
    function Get-TwilioPhoneNumbers {
    [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
    HelpUri='https://www.twilio.com/docs/api/rest/outgoing-caller-ids')]
    param(
       [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [string]
       $AccountSid,
   
       [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [string]
       $AuthToken,
   
       [Parameter(ParameterSetName='UseConnectionObject', Mandatory=$true)]
       [ValidateNotNullOrEmpty()]
       [Hashtable]
       $Connection
    )
   
    $cred = CreateTwilioCredential -Connection $Connection -AccountSid $AccountSid -AuthToken $AuthToken
   
    $uri = "$TWILIO_BASE_URL/Accounts/" + $cred.UserName + "/IncomingPhoneNumbers"
   
    $response = Invoke-RestMethod -Method Get -Uri $uri -Credential $cred
   
    $response.TwilioResponse.IncomingPhoneNumbers.IncomingPhoneNumber
    }
    ```

   Al proporcionar esta información se muestra esta ayuda mediante el cmdlet **Get-Help** en la consola de PowerShell. Esta información también se expone en Azure Automation.  Por ejemplo, al insertar actividades durante la creación de runbooks. Al hacer clic en "Ver ayuda detallada", el identificador URI de ayuda se abrirá en otra pestaña del explorador web que esté utilizando para acceder a Azure Automation.

   ![Ayuda para los módulos de integración](media/automation-integration-modules/automation-integration-module-activitydesc.png)

2. Si el módulo se ejecuta en un sistema remoto:

   1. debería incluir un archivo de metadatos del módulo de integración que defina la información necesaria para conectarse a ese sistema remoto (es decir, el tipo de conexión).
   2. En segundo lugar, cada cmdlet del módulo debería poder tomar como parámetro un objeto de conexión (una instancia del tipo de conexión).  

    Los cmdlets del módulo resultan más fáciles de usar en Azure Automation si permite que se les pase como parámetro un objeto con los campos del tipo de conexión. Esto permite a los usuarios asignar parámetros del recurso de conexión a los parámetros correspondientes del cmdlet cada vez que invoquen un cmdlet.

    En el ejemplo de Runbook anterior, se utiliza un recurso de conexión Twilio denominado «CorpTwilio» para acceder a Twilio y devolver todos los números de teléfono de la cuenta.  Observe cómo se asignan los campos de la conexión a los parámetros del cmdlet.

    ```powershell
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'

      Get-TwilioPhoneNumbers 
        -AccountSid $CorpTwilio.AccountSid  
        -AuthToken $CorptTwilio.AuthToken
    }
    ```
  
    Existe otra forma más fácil y eficaz de afrontar este comportamiento, y consiste en pasar directamente el objeto de conexión al cmdlet.

    ```powershell
    workflow Get-CorpTwilioPhones
    {
      $CorpTwilio = Get-AutomationConnection -Name 'CorpTwilio'

      Get-TwilioPhoneNumbers -Connection $CorpTwilio
    }
    ```

    Para habilitar un comportamiento como el del ejemplo anterior en sus cmdlets, permítales que acepten directamente un objeto de conexión como parámetro en lugar de usar solamente campos de conexión en los parámetros. Normalmente, necesitará un conjunto de parámetros para cada uno de ellos, de forma que los usuarios que no utilicen Azure Automation puedan llamar a los cmdlets sin necesidad de crear una tabla hash que actúe como objeto de conexión. El conjunto de parámetros **SpecifyConnectionFields** se utiliza para pasar las propiedades de los campos de conexión de una en una. **UseConnectionObject** permite pasar directamente la conexión. Como puede ver, el cmdlet Send-TwilioSMS del [módulo de PowerShell de Twilio](https://gallery.technet.microsoft.com/scriptcenter/Twilio-PowerShell-Module-8a8bfef8) permite utilizar cualquiera de estos mecanismos para pasar valores:

    ```powershell
    function Send-TwilioSMS {
      [CmdletBinding(DefaultParameterSetName='SpecifyConnectionFields', `
      HelpUri='https://www.twilio.com/docs/api/rest/sending-sms')]
      param(
         [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [string]
         $AccountSid,
   
         [Parameter(ParameterSetName='SpecifyConnectionFields', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [string]
         $AuthToken,
   
         [Parameter(ParameterSetName='UseConnectionObject', Mandatory=$true)]
         [ValidateNotNullOrEmpty()]
         [Hashtable]
         $Connection
   
       )
    }
    ```

3. Defina el tipo de salida de todos los cmdlets del módulo. Definir un tipo de salida en un cmdlet permite que IntelliSense le ayude a determinar, en tiempo de diseño, las propiedades de salida del cmdlet, lo que resultará útil durante la creación. Esto resulta especialmente útil durante la creación gráfica de un runbook de Automation, donde la información en tiempo de diseño resulta esencial para facilitar la experiencia del usuario con el módulo.

   ![Tipo de salida de Runbooks gráficos](media/automation-integration-modules/runbook-graphical-module-output-type.png)

   Este comportamiento es similar a la funcionalidad "type ahead" de la salida de los cmdlets de PowerShell ISE sin necesidad de ejecutarla.

   ![POSH IntelliSense](media/automation-integration-modules/automation-posh-ise-intellisense.png)

4. Los cmdlets del módulo no deben tomar tipos de objetos complejos como parámetros. El flujo de trabajo de PowerShell se diferencia del propio PowerShell en que almacena tipos complejos con formato deserializado. Los tipos primitivos siguen siendo primitivos. Sin embargo, los tipos complejos se convierten en sus versiones deserializadas, por lo que básicamente son bolsas de propiedades. Por ejemplo, si utiliza el cmdlet **Get-Process** en un Runbook (o, en este caso, en un flujo de trabajo de PowerShell), el cmdlet devolverá un objeto de tipo [Deserialized.System.Diagnostic.Process] en lugar del tipo esperado: [System.Diagnostic.Process]. Este tipo tiene las mismas propiedades que el tipo no deserializado, pero no contiene ninguno de los métodos. Además, si intenta pasar este valor como parámetro a un cmdlet (donde el cmdlet espera un valor [System.Diagnostic.Process] para este parámetro), aparecerá el siguiente error: *No se puede procesar la transformación del argumento del parámetro 'process'. Error: "No se puede convertir el valor "System.Diagnostics.Process (CcmExec)" de tipo "Deserialized.System.Diagnostics.Process" al tipo "System.Diagnostics.Process".*   Esto se debe a que hay un error de concordancia de tipos entre el tipo esperado, [System.Diagnostic.Process], y el tipo proporcionado, [Deserialized.System.Diagnostic.Process]. El modo de solucionar este problema es garantizar que los cmdlets del módulo no toman tipos complejos como parámetros. Este es el modo incorrecto de hacerlo.

    ```powershell
    function Get-ProcessDescription {
      param (
            [System.Diagnostic.Process] $process
      )
      $process.Description
    }
    ```

    El modo correcto es tomar un primitivo que el cmdlet usa internamente para captar el objeto complejo y utilizarlo. Como los cmdlets se ejecutan en el contexto de PowerShell, y no en el del flujo de trabajo de PowerShell, el cmdlet $process se transforma en el tipo adecuado, [System.Diagnostic.Process].  

    ```powershell
    function Get-ProcessDescription {
      param (
            [String] $processName
      )
      $process = Get-Process -Name $processName
   
      $process.Description
    }
    ```

   Los recursos de conexión de los runbooks son tablas hash, que son de tipo complejo. Sin embargo, parece que estas tablas hash pueden pasarse perfectamente a los cmdlets como parámetro 'Connection, sin ninguna excepción de conversión. Técnicamente, algunos tipos de PowerShell pueden convertirse correctamente de su formato serializado al formato deserializado y, por tanto, pueden pasarse a los cmdlets en los parámetros que aceptan tipos no deserializados. Las tablas hash son uno de ellos. Es posible que los tipos definidos por el creador de un módulo se implementen de forma que también puedan deserializarse correctamente, pero hay que tener en cuenta algunas consideraciones. El tipo debe tener un constructor predeterminado, todas sus propiedades deben ser públicas y debe tener un objeto PSTypeConverter. Sin embargo, en el caso de los tipos que ya están definidos y que no son propiedad del creador del módulo, no hay forma de "corregirlos" y, por tanto, se recomienda evitar tipos complejos en todos los parámetros. Sugerencia sobre la creación de Runbooks: Si sus cmdlets necesitan tomar un parámetro de tipo complejo, la solución en los flujos de PowerShell consiste en encapsular el cmdlet que genera el tipo complejo y el cmdlet que utiliza el tipo complejo en la misma actividad de InlineScript. Dado que InlineScript ejecuta su contenido como PowerShell y no como el flujo de trabajo de PowerShell, el cmdlet que genera el tipo complejo crearía el tipo correcto, y no el tipo complejo deserializado.

5. Cree todos los cmdlets del módulo sin estado. El flujo de trabajo de PowerShell ejecuta cada uno de los cmdlets que se invocan durante el flujo de trabajo en una sesión diferente. Esto significa que los cmdlets que dependan de un estado de sesión creado o modificado por otros cmdlets del mismo módulo no funcionarán en los runbooks del flujo de trabajo de PowerShell.  A continuación se muestra un ejemplo de lo que no hay que hacer:
   
    ```powershell
    $globalNum = 0
    function Set-GlobalNum {
       param(
           [int] $num
       )
   
       $globalNum = $num
    }
    function Get-GlobalNumTimesTwo {
       $output = $globalNum * 2
   
       $output
    }
    ```

6. El módulo debe estar incluido en su totalidad en un paquete compatible con Xcopy. Los módulos de Azure Automation se distribuyen en los espacios aislados de Automation cuando los runbooks tengan que ejecutarse. Los módulos deben trabajar independientemente del host en que se están ejecutando. Debe ser capaz de comprimir y mover un paquete de módulo y conseguir que funcione con normalidad cuando se importe en el entorno de PowerShell de otro host. Para que esto ocurra, el módulo no debería depender de los archivos de fuera de la carpeta del módulo. Esta carpeta es la carpeta que se comprime al importar el módulo en Azure Automation. El módulo tampoco debe depender de ninguna configuración única del Registro en un host, como los parámetros que se establecen cuando se instala un producto. Si no se sigue este procedimiento recomendado, el módulo no podrá utilizarse en Azure Automation.  

7. Si hace referencia a [módulos de Azure Powershell Az](/powershell/azure/new-azureps-module-az?view=azps-1.1.0) en el módulo, asegúrese de que no hace también referencia a `AzureRM`. El módulo `Az` no se puede usar junto con los módulos `AzureRM`. `Az` se admite en los runbooks, pero no se importa de forma predeterminada. Para obtener información sobre los módulos de `Az` y las consideraciones que debe tener en cuenta, consulte [Az module support in Azure Automation](az-modules.md) (Soporte técnico para módulos de Az en Azure Automation).

## <a name="next-steps"></a>Pasos siguientes

* Para empezar a trabajar con Runbooks de flujo de trabajo de PowerShell, consulte [Mi primer runbook de flujo de trabajo de PowerShell](automation-first-runbook-textual.md)
* Para más información sobre la creación de módulos de PowerShell, consulte [Writing a Windows PowerShell Module](https://msdn.microsoft.com/library/dd878310%28v=vs.85%29.aspx)
