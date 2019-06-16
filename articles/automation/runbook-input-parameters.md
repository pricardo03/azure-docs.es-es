---
title: Parámetros de entrada de Runbook
description: Los parámetros de entrada de Runbook aumentan la flexibilidad de los Runbooks porque permiten pasar datos a un Runbook cuando se inicia. En este artículo se describen los distintos escenarios donde se usan parámetros de entrada en Runbooks.
services: automation
ms.service: automation
ms.subservice: process-automation
author: georgewallace
ms.author: gwallace
ms.date: 02/14/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 5f190d60a059108b9763f35e2ee8cf99ae77b694
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60500044"
---
# <a name="runbook-input-parameters"></a>Parámetros de entrada de Runbook

Los parámetros de entrada de Runbook aumentan la flexibilidad de los runbooks porque permiten pasarle datos cuando se inicia. Los parámetros permiten dirigir acciones de Runbook a entornos y escenarios específicos. En este artículo, le guiaremos por los distintos escenarios donde se usan parámetros de entrada en runbooks.

## <a name="configure-input-parameters"></a>Configuración de parámetros de entrada

Los parámetros de entrada pueden configurarse en los runbooks gráficos, de PowerShell, de flujo de trabajo de PowerShell y de Python. Un Runbook puede tener varios parámetros con tipos de datos diferentes o sin parámetros. Los parámetros de entrada pueden ser obligatorios u opcionales, y puede asignar un valor predeterminado a los parámetros opcionales. La asignación de valores a los parámetros de entrada para un runbook se realiza al iniciarlo mediante uno de los métodos disponibles. Entre estos métodos, se incluye iniciar un runbook desde Azure Portal, un servicio web o PowerShell. También puede iniciarlo como Runbook secundario al que se llama insertado en otro Runbook.

## <a name="configure-input-parameters-in-powershell-runbooks"></a>Configuración de parámetros de entrada en runbooks de PowerShell

Los runbooks de PowerShell y del flujo de trabajo de PowerShell en Azure Automation admiten parámetros de entrada que se definen mediante los siguientes atributos:  

| **Propiedad** | **Descripción** |
|:--- |:--- |
| `Type` |Necesario. Tipo de datos previsto para el valor del parámetro. Es válido cualquier tipo .NET. |
| `Name` |Necesario. El nombre del parámetro. Debe ser único dentro del Runbook y puede contener solo letras, números o caracteres de subrayado. Debe empezar por una letra. |
| `Mandatory` |Opcional. Especifica si se debe proporcionar un valor para el parámetro. Si se establece en **\$true**, es necesario proporcionar un valor cuando se inicia el runbook. Si se establece en **\$false**, el valor es opcional. |
| `Default value` |Opcional. Especifica un valor que se usará para el parámetro si no se pasa un valor al iniciar el runbook. Se puede establecer un valor predeterminado para cualquier parámetro y hará automáticamente que el parámetro sea opcional independientemente de la configuración de Mandatory. |

Windows PowerShell admite más atributos de parámetros de entrada de los enumerados aquí, como validación, alias y conjuntos de parámetros. Sin embargo, Azure Automation actualmente solo admite los parámetros de entrada enumerados anteriormente.

Una definición de parámetro en los Runbooks de flujo de trabajo de PowerShell tiene el formato general siguiente, donde los distintos parámetros están separados por comas.

```powershell
Param
(
  [Parameter (Mandatory= $true/$false)]
  [Type] $Name1 = <Default value>,

  [Parameter (Mandatory= $true/$false)]
  [Type] $Name2 = <Default value>
)
```

> [!NOTE]
> Al definir parámetros, si no se especifica el atributo **Mandatory** , el parámetro se considera opcional de forma predeterminada. Además, si se establece un valor predeterminado para un parámetro en runbooks de flujo de trabajo de PowerShell, PowerShell lo tratará como un parámetro opcional, independientemente del valor del atributo **Mandatory**.

Por ejemplo, vamos a configurar los parámetros de entrada para un Runbook de flujo de trabajo de PowerShell que proporciona detalles sobre las máquinas virtuales, una o varias, de un grupo de recursos. Este Runbook tiene dos parámetros, como se muestra en la siguiente captura de pantalla: el nombre de la máquina virtual y el nombre del grupo de recursos.

![Automation, flujo de trabajo de PowerShell](media/automation-runbook-input-parameters/automation-01-powershellworkflow.png)

En esta definición de parámetros, los parámetros **\$VMName** y **\$resourceGroupName** son parámetros simples de tipo String. Sin embargo, los Runbooks de PowerShell y de flujo de trabajo de PowerShell admiten todos los tipos simples y complejos, como **object** o **PSCredential**, como parámetros de entrada.

Si el runbook tiene un parámetro de entrada de tipo object, use una tabla hash de PowerShell con pares (nombre,valor) para pasar un valor. Por ejemplo, si tiene el siguiente parámetro en un Runbook:

```powershell
[Parameter (Mandatory = $true)]
[object] $FullName
```

Puede pasar el siguiente valor al parámetro:

```powershell
@{"FirstName"="Joe";"MiddleName"="Bob";"LastName"="Smith"}
```

> [!NOTE]
> Cuando no se pasa ningún valor a un parámetro opcional de tipo `[String]` con un _valor predeterminado_ de `\$null`, entonces el valor del parámetro será una _cadena vacía_, y **no** `\$null`.

## <a name="configure-input-parameters-in-graphical-runbooks"></a>Configuración de parámetros de entrada en Runbooks gráficos

Para [configurar un Runbook gráfico](automation-first-runbook-graphical.md) con parámetros de entrada, vamos a crear un Runbook gráfico que proporciona detalles sobre las máquinas virtuales, una o varias, de un grupo de recursos. La configuración de un Runbook consta de dos actividades principales, como se describe a continuación.

[**Autenticación de Runbooks con una cuenta de ejecución de Azure**](automation-sec-configure-azure-runas-account.md) para autenticarse con Azure.

[**Get-AzureRmVm**](/powershell/module/azurerm.compute/get-azurermvm) para obtener las propiedades de una máquina virtual.

Puede usar la actividad [**Write-Output**](/powershell/module/microsoft.powershell.utility/write-output) para obtener los nombres de las máquinas virtuales. La actividad **Get AzureRmVm** acepta dos parámetros, el **nombre de máquina virtual** y el **nombre del grupo de recursos**. Puesto que estos parámetros requieren valores diferentes cada vez que se inicia el Runbook, puede agregar parámetros de entrada a su Runbook. Estos son los pasos para agregar parámetros de entrada:

1. Seleccione el Runbook gráfico de la hoja **Runbooks** y [**edítelo**](automation-graphical-authoring-intro.md).
2. En el editor de Runbooks, haga clic en **Entrada y salida** para abrir la hoja **Entrada y salida**.

   ![Runbook gráfico de Automation](media/automation-runbook-input-parameters/automation-02-graphical-runbok-editor.png)

3. La hoja **Entrada y salida** muestra una lista de parámetros de entrada definidos para el Runbook. En esta hoja, puede agregar un nuevo parámetro de entrada o editar la configuración de uno existente. Para agregar un nuevo parámetro para el Runbook, haga clic en **Agregar entrada** para abrir la hoja **Parámetro de entrada de Runbook**. En ella puede configurar los siguientes parámetros:

   | **Propiedad** | **Descripción** |
   |:--- |:--- |
   | `Name` |Necesario. El nombre del parámetro. Debe ser único dentro del Runbook y puede contener solo letras, números o caracteres de subrayado. Debe empezar por una letra. |
   | `Description` |Opcional. Descripción del propósito del parámetro de entrada. |
   | `Type` |Opcional. Tipo de datos previsto para el valor del parámetro. Los tipos de parámetro admitidos son **String**, **Int32**, **Int64**, **Decimal**, **Boolean**, **DateTime** y **Object**. Si no se selecciona ningún tipo de datos, el valor predeterminado es **String**. |
   | `Mandatory` |Opcional. Especifica si se debe proporcionar un valor para el parámetro. Si elige **yes**, se debe proporcionar un valor cuando se inicia el Runbook. Si elige **no**, no se necesita un valor cuando se inicia el Runbook y se puede establecer un valor predeterminado. |
   | `Default Value` |Opcional. Especifica un valor que se usará para el parámetro si no se pasa un valor al iniciar el runbook. Puede establecerse un valor predeterminado para un parámetro que no sea obligatorio. Para establecer un valor predeterminado, elija **Personalizado**. Este valor se usa a menos que se proporcione otro valor al iniciar el Runbook. Elija **Ninguno** si no desea proporcionar ningún valor predeterminado. |

    ![Agregar nueva entrada](media/automation-runbook-input-parameters/automation-runbook-input-parameter-new.png)
4. Cree dos parámetros con las siguientes propiedades que usará la actividad **Get-AzureRmVm**:

   * **Parameter1:**
     * Nombre: VMName
     * Tipo: String
     * Obligatorio: No
   * **Parameter2:**
     * Nombre: resourceGroupName
     * Tipo: String
     * Obligatorio: No
     * Valor predeterminado: Personalizado
     * Valor predeterminado personalizado: \<Nombre del grupo de recursos que contiene las máquinas virtuales\>

5. Una vez agregados los parámetros, haga clic en **Aceptar**. Ahora puede verlos en la **página Entrada y salida**. Haga clic en **Aceptar** de nuevo y después en **Guardar** y en **Publicar** para publicar el Runbook.

## <a name="configure-input-parameters-in-python-runbooks"></a>Configuración de parámetros de entrada en runbooks de Python

A diferencia de los runbooks gráficos, de PowerShell y de flujo de trabajo de PowerShell, los runbooks de Python no toman parámetros con nombre.
Todos los parámetros de entrada se analizan como una matriz de valores de argumento.
Para obtener acceso a la matriz, debe importar el módulo `sys` al script de Python y usar la matriz `sys.argv`.
Es importante tener en cuenta que el primer elemento de la matriz, `sys.argv[0]`, es el nombre del script, por lo que el primer parámetro de entrada real es `sys.argv[1]`.

Para obtener un ejemplo de cómo usar los parámetros de entrada en un runbook de Python, vea [My first Python runbook in Azure Automation](automation-first-runbook-textual-python2.md) (Mi primer runbook de Python en Azure Automation).

## <a name="assign-values-to-input-parameters-in-runbooks"></a>Asignación de valores a parámetros de entrada en Runbooks

Puede pasar valores a parámetros de entrada en runbooks en los siguientes escenarios:

### <a name="start-a-runbook-and-assign-parameters"></a>Inicio de un Runbook y asignación de parámetros

Se puede iniciar un Runbook de muchas maneras: mediante Azure Portal, con un webhook, con cmdlets de PowerShell, con la API de REST o con el SDK. A continuación, se describen distintos métodos para iniciar un Runbook y asignar parámetros.

#### <a name="start-a-published-runbook-by-using-the-azure-portal-and-assign-parameters"></a>Inicio de un Runbook publicado mediante el Portal de Azure y asignación de parámetros

Al [iniciar el runbook](start-runbooks.md#start-a-runbook-with-the-azure-portal), se abre la hoja **Iniciar runbook** y puede indicar los valores de los parámetros que creó.

![Empezar a usar el portal](media/automation-runbook-input-parameters/automation-04-startrunbookusingportal.png)

En la etiqueta bajo el cuadro de entrada, puede ver los atributos que se han definido para el parámetro. Entre los atributos, se incluye si es obligatorio u opcional, el tipo y el valor predeterminado. En el globo de ayuda junto al nombre del parámetro puede ver toda la información importante que necesita para tomar decisiones acerca de los valores de entrada de los parámetros. Esta información incluye si un parámetro es obligatorio u opcional. También incluye el tipo y el valor predeterminado (si existe), además de otras notas útiles.

> [!NOTE]
> Los parámetros de tipo String admiten valores de cadena **Empty** .  Si escribe **[EmptyString]** en el cuadro del parámetro de entrada, pasará una cadena vacía al parámetro. Además, los parámetros de tipo String no permiten que se pasen valores **Null** . Si no pasa ningún valor al parámetro de tipo String, PowerShell lo interpretará como Null.

#### <a name="start-a-published-runbook-by-using-powershell-cmdlets-and-assign-parameters"></a>Inicio de un Runbook publicado mediante cmdlets de PowerShell y asignación de parámetros

* **Cmdlets de Azure Resource Manager**: puede iniciar un Runbook de automatización creado en un grupo de recursos mediante [Start-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook).
  
  **Ejemplo:**

  ```powershell
  $params = @{"VMName"="WSVMClassic";"resourceGroupeName"="WSVMClassicSG"}
  
  Start-AzureRmAutomationRunbook -AutomationAccountName "TestAutomation" -Name "Get-AzureVMGraphical" –ResourceGroupName $resourceGroupName -Parameters $params
  ```

* **Cmdlets del modelo de implementación clásica de Azure:** puede iniciar un runbook de automatización creado en un grupo de recursos predeterminado mediante [Start-AzureRmAutomationRunbook](/powershell/module/servicemanagement/azure/start-azureautomationrunbook).
  
  **Ejemplo:**

  ```powershell
  $params = @{"VMName"="WSVMClassic"; "ServiceName"="WSVMClassicSG"}
  
  Start-AzureAutomationRunbook -AutomationAccountName "TestAutomation" -Name "Get-AzureVMGraphical" -Parameters $params
  ```

> [!NOTE]
> Cuando se inicia un Runbook mediante cmdlets de PowerShell, se crea un parámetro predeterminado, **MicrosoftApplicationManagementStartedBy**, con el valor **PowerShell**. Puede ver este parámetro en la página **Detalles del trabajo**.  

#### <a name="start-a-runbook-by-using-an-sdk-and-assign-parameters"></a>Inicio de un Runbook con un SDK y asignación de parámetros

* **Método de Azure Resource Manager:** puede iniciar un runbook mediante el SDK de un lenguaje de programación. A continuación, se muestra un fragmento de código C# para iniciar un Runbook en su cuenta de Automation. Puede ver todo el código en nuestro [repositorio de GitHub](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ResourceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).  

  ```csharp
   public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
      {
        var response = AutomationClient.Jobs.Create(resourceGroupName, automationAccount, new JobCreateParameters
         {
            Properties = new JobCreateProperties
             {
                Runbook = new RunbookAssociationProperty
                 {
                   Name = runbookName
                 },
                   Parameters = parameters
             }
         });
      return response.Job;
      }
  ```

* **Método del modelo de implementación clásica de Azure:** puede iniciar un runbook mediante el SDK de un lenguaje de programación. A continuación, se muestra un fragmento de código C# para iniciar un Runbook en su cuenta de Automation. Puede ver todo el código en nuestro [repositorio de GitHub](https://github.com/Azure/azure-sdk-for-net/blob/master/src/ServiceManagement/Automation/Automation.Tests/TestSupport/AutomationTestBase.cs).

  ```csharp
  public Job StartRunbook(string runbookName, IDictionary<string, string> parameters = null)
    {
      var response = AutomationClient.Jobs.Create(automationAccount, new JobCreateParameters
    {
      Properties = new JobCreateProperties
         {
           Runbook = new RunbookAssociationProperty
         {
           Name = runbookName
              },
                Parameters = parameters
              }
       });
      return response.Job;
    }
  ```

  Para iniciar este método, cree un diccionario para almacenar los parámetros del Runbook, **VMName** y **resourceGroupName**, así como sus valores. A continuación, inicie el Runbook. A continuación, se muestra el fragmento de código C# para llamar al método definido más arriba.

  ```csharp
  IDictionary<string, string> RunbookParameters = new Dictionary<string, string>();
  
  // Add parameters to the dictionary.
  RunbookParameters.Add("VMName", "WSVMClassic");
  RunbookParameters.Add("resourceGroupName", "WSSC1");
  
  //Call the StartRunbook method with parameters
  StartRunbook("Get-AzureVMGraphical", RunbookParameters);
  ```

#### <a name="start-a-runbook-by-using-the-rest-api-and-assign-parameters"></a>Inicio de un Runbook con la API de REST y asignación de parámetros

Se puede crear un trabajo de runbook e iniciarlo con la API REST de Azure Automation mediante el método **PUT** con el siguiente identificador URI de solicitud: `https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Automation/automationAccounts/{automationAccountName}/jobs/{jobName}?api-version=2017-05-15-preview`


En el identificador URI de solicitud, reemplace los siguientes parámetros:

* **subscriptionId:** el identificador de la suscripción a Azure.  
* **resourceGroupName:** el nombre del grupo de recursos de la cuenta de Automation.
* **automationAccountName:** nombre de la cuenta de automatización hospedada en el servicio en la nube especificado.  
* **jobName:** GUID para el id. de trabajo. En PowerShell, los GUID pueden crearse con el comando **[GUID]::NewGuid().ToString()** .

Para pasar parámetros al trabajo de Runbook, use el cuerpo de la solicitud. Admite las dos propiedades siguientes proporcionadas en formato JSON:

* **Nombre de runbook:** Necesario. El nombre del Runbook para que se inicie el trabajo.  
* **Parámetros de runbook:** Opcional. Diccionario de la lista de parámetros en formato (nombre, valor), donde el nombre debe ser de tipo String y el valor puede ser cualquier valor JSON válido.

Si desea iniciar el Runbook **Get-AzureVMTextual** creado antes con **VMName** y **resourceGroupName** como parámetros, use el siguiente formato JSON para el cuerpo de la solicitud.

   ```json
    {
      "properties":{
        "runbook":{
        "name":"Get-AzureVMTextual"},
      "parameters":{
         "VMName":"WindowsVM",
         "resourceGroupName":"ContosoSales"}
        }
    }
   ```

Si el trabajo se crea correctamente, se devuelve un código de estado HTTP 201. Para más información sobre los encabezados y el cuerpo de la respuesta, consulte el artículo acerca de cómo [crear un trabajo de runbook mediante la API de REST](/rest/api/automation/job/create)

### <a name="test-a-runbook-and-assign-parameters"></a>Prueba de un Runbook y asignación de parámetros

Cuando se [prueba la versión de borrador del runbook](automation-testing-runbook.md) mediante la opción de prueba, se abre la hoja **Prueba** y puede configurar los valores de los parámetros que creó.

![Prueba y asignación de parámetros](media/automation-runbook-input-parameters/automation-06-testandassignparameters.png)

### <a name="link-a-schedule-to-a-runbook-and-assign-parameters"></a>Vinculación de una programación a un Runbook y asignación de parámetros

También puede [vincular una programación](automation-schedules.md) a su Runbook para que este se inicie en un momento determinado. Asigne los parámetros de entrada al crear la programación y el runbook los usará cuando se inicie mediante la programación. No se puede guardar la programación hasta que se proporcionen todos los valores de los parámetros obligatorios.

![Programación y asignación de parámetros](media/automation-runbook-input-parameters/automation-07-scheduleandassignparameters.png)

### <a name="create-a-webhook-for-a-runbook-and-assign-parameters"></a>Creación de un Webhook para un Runbook y asignación de parámetros

Puede crear un [Webhook](automation-webhooks.md) para el Runbook y configurar los parámetros de entrada del Runbook. No se puede guardar el Webhook hasta que se proporcionen todos los valores de los parámetros obligatorios.

![Creación de un Webhook y asignación de parámetros](media/automation-runbook-input-parameters/automation-08-createwebhookandassignparameters.png)

Cuando se ejecuta un Runbook mediante un webhook, se envía un parámetro de entrada predefinido **[Webhookdata](automation-webhooks.md#details-of-a-webhook)** , junto con los parámetros de entrada que definió. Puede hacer clic para expandir el parámetro **WebhookData** para más detalles.

![Parámetro WebhookData](media/automation-runbook-input-parameters/automation-09-webhook-data-parameters.png)

## <a name="pass-a-json-object-to-a-runbook"></a>Paso de un objeto JSON a un runbook

Puede ser útil almacenar los datos que desea pasar a un runbook en un archivo JSON.
Por ejemplo, podría crear un archivo JSON que contiene todos los parámetros que desea pasar a un runbook. Para hacerlo, tiene que convertir el archivo JSON en una cadena y luego convertir la cadena en un objeto de PowerShell antes de pasarlo al runbook.

En este ejemplo, tiene un script de PowerShell que llamará a [Start-AzureRmAutomationRunbook](/powershell/module/azurerm.automation/start-azurermautomationrunbook) para iniciar un runbook de PowerShell, pasando el contenido del archivo JSON al runbook.
El runbook de PowerShell inicia una máquina virtual de Azure y se obtienen los parámetros de la máquina virtual desde el archivo JSON que se pasó.

### <a name="create-the-json-file"></a>Creación del archivo JSON

Escriba el texto siguiente en un archivo de texto y guárdelo como `test.json` en algún lugar del equipo local.

```json
{
   "VmName" : "TestVM",
   "ResourceGroup" : "AzureAutomationTest"
}
```

### <a name="create-the-runbook"></a>Creación del runbook

Cree un runbook de PowerShell denominado "Test-Json" en Azure Automation.
Para saber cómo crear un runbook de PowerShell, consulte [Mi primer runbook de PowerShell](automation-first-runbook-textual-powershell.md).

Para aceptar los datos de JSON, el runbook debe tomar un objeto como parámetro de entrada.

De ese modo, el runbook puede usar las propiedades definidas en el archivo JSON.

```powershell
Param(
     [parameter(Mandatory=$true)]
     [object]$json
)

# Connect to Azure account
$Conn = Get-AutomationConnection -Name AzureRunAsConnection
Connect-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID `
    -ApplicationID $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint

# Convert object to actual JSON
$json = $json | ConvertFrom-Json

# Use the values from the JSON object as the parameters for your command
Start-AzureRmVM -Name $json.VMName -ResourceGroupName $json.ResourceGroup
```

Guarde y publique este runbook en su cuenta de Automation.

### <a name="call-the-runbook-from-powershell"></a>Llamada al runbook desde PowerShell

Ahora puede llamar al runbook desde la máquina local mediante Azure PowerShell.
Ejecute los comandos de PowerShell siguientes:

1. Inicie de sesión en Azure:

   ```powershell
   Connect-AzureRmAccount
   ```

   Se le pedirá que escriba las credenciales de Azure.

   > [!IMPORTANT]
   > **Add-AzureRmAccount** es ahora un alias de **Connect-AzureRMAccount**. Al buscar elementos de biblioteca, si no ve **Connect-AzureRMAccount**, puede usar **Add-AzureRmAccount** o actualizar los módulos en su cuenta de Automation.

1. Obtenga el contenido del archivo JSON y conviértalo en una cadena:

   ```powershell
   $json =  (Get-content -path 'JsonPath\test.json' -Raw) | Out-string
   ```

   `JsonPath` es la ruta de acceso donde se guardó el archivo JSON.

1. Convierta el contenido de la cadena de `$json` en un objeto de PowerShell:

   ```powershell
   $JsonParams = @{"json"=$json}
   ```

1. Cree una tabla hash para los parámetros de `Start-AzureRmAutomationRunbook`:

   ```powershell
   $RBParams = @{
        AutomationAccountName = 'AATest'
        ResourceGroupName = 'RGTest'
        Name = 'Test-Json'
        Parameters = $JsonParams
   }
   ```

   Tenga en cuenta que está estableciendo el valor de `Parameters` en el objeto de PowerShell que contiene los valores del archivo JSON.
1. Inicio del runbook

   ```powershell
   $job = Start-AzureRmAutomationRunbook @RBParams
   ```

## <a name="next-steps"></a>Pasos siguientes

* Para más información acerca de diferentes maneras de iniciar un Runbook, consulte [Inicio de un runbook en Automatización de Azure](automation-starting-a-runbook.md).
* Para editar un Runbook de texto, consulte [Edición de runbooks de texto en Automatización de Azure](automation-edit-textual-runbook.md).
* Para editar un Runbook gráfico, consulte [Creación gráfica en Azure Automation](automation-graphical-authoring-intro.md).
