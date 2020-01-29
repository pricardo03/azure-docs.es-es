---
title: Inicio de un runbook de Azure Automation con un webhook
description: Un webhook que permite a un cliente iniciar un runbook en Azure Automation desde una llamada HTTP.  Este artículo describe cómo crear un webhook y cómo llamar a uno para que inicie un runbook.
services: automation
ms.subservice: process-automation
ms.date: 01/16/2020
ms.topic: conceptual
ms.openlocfilehash: f86193e818a91132f9bbca447acadd7e81747522
ms.sourcegitcommit: 276c1c79b814ecc9d6c1997d92a93d07aed06b84
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2020
ms.locfileid: "76155832"
---
# <a name="starting-an-azure-automation-runbook-with-a-webhook"></a>Inicio de un runbook de Azure Automation con un webhook

Un *webhook* le permite iniciar un runbook determinado en Azure Automation a través de una sola solicitud HTTP. Esto permite que servicios externos, como Azure DevOps Services, GitHub, registros de Azure Monitor o aplicaciones personalizadas, inicien runbooks sin necesidad de implementar una solución completa mediante la API de Azure Automation.
![WebhooksOverview](media/automation-webhooks/webhook-overview-image.png)

Puede comparar webhooks con otros métodos para iniciar un runbook [a partir un runbook de Azure Automation.](automation-starting-a-runbook.md)

> [!NOTE]
> No se admite el uso de un webhook para iniciar un runbook de Python.

## <a name="details-of-a-webhook"></a>Detalles de un webhook

En la tabla siguiente se describen las propiedades que debe configurar para un webhook.

| Propiedad | Descripción |
|:--- |:--- |
| Nombre |Puede proporcionar cualquier nombre que desee para un webhook, ya que esto no se expone al cliente. Solo se utiliza para que identifique el runbook en Azure Automation. <br> Como práctica recomendada, debe proporcionar al webhook un nombre relacionado con el cliente que lo usa. |
| URL |La dirección URL del webhook es la dirección única que llama a un cliente con una solicitud HTTP POST para iniciar el runbook vinculado al webhook. Se genera automáticamente al crear el webhook. No se puede especificar una dirección URL personalizada. <br> <br> La dirección URL contiene un token de seguridad que permite que el runbook se invoque por un sistema de terceros sin autenticación adicional. Por este motivo, debe tratarse como una contraseña. Por motivos de seguridad, solo puede ver la dirección URL en el Portal de Azure en el momento en que se crea el Webhook. Anote la dirección URL en una ubicación segura para su uso futuro. |
| Fecha de expiración |Al igual que un certificado, cada webhook tiene una fecha de caducidad en la que ya no se puede usar. Esta fecha de expiración se puede modificar una vez creado el webhook, siempre y cuando no haya expirado. |
| habilitado |Al crearse, el webhook se habilita de forma predeterminada. Si se establece como deshabilitado, ningún cliente podrá usarlo. Puede establecer la propiedad **Enabled** al crear el webhook o en cualquier momento una vez creado. |

### <a name="parameters"></a>Parámetros

Un webhook puede definir valores para parámetros de runbook que se usan cuando se inicia dicho webhook inicia el runbook. El webhook debe incluir los valores de los parámetros obligatorios del runbook y puede incluir valores para los parámetros opcionales. Un valor de parámetro configurado para un webhook se puede modificar incluso después de crear el webhook. Varios webhooks vinculados a un único runbook puede utilizar distintos valores de parámetros.

Cuando un cliente inicia un runbook mediante un webhook, no puede reemplazar los valores de los parámetros definidos en el webhook. Para recibir datos del cliente, el runbook puede aceptar un único parámetro llamado **$WebhookData**. Este parámetro es de tipo [object] que contiene datos que el cliente incluye en la solicitud POST.

![Propiedades de Webhookdata](media/automation-webhooks/webhook-data-properties.png)

El objeto **$WebhookData** tiene las siguientes propiedades:

| Propiedad | Descripción |
|:--- |:--- |
| WebhookName |Nombre del webhook. |
| RequestHeader |Tabla hash que contiene los encabezados de la solicitud POST entrante. |
| RequestBody |El cuerpo de la solicitud POST entrante. Esto conserva todo el formato como cadena de JSON, XML o datos codificados de formulario. El runbook debe escribirse para que trabaje con el formato de datos que se espera. |

No hay ninguna configuración del webhook obligatoria para admitir el parámetro **$WebhookData** y el runbook no tiene que aceptarlo. Si el runbook no define el parámetro, se ignoran los detalles de la solicitud enviados desde el cliente.

> [!NOTE]
> Al llamar a un webhook, siempre debe almacenar los valores de los parámetros en caso de que se produzca un error en la llamada. Si se produce un problema de conexión o de interrupción de la red, no podrá recuperar las llamadas de webhook con errores.

Si especifica un valor para $WebhookData al crear el webhook, el valor se reemplaza cuando el webhook inicia el runbook con los datos de la solicitud POST del cliente, incluso si el cliente no incluye ningún dato en el cuerpo de la solicitud. Si inicia un runbook con $WebhookData mediante un método que no sea un webhook, puede proporcionar un valor para $Webhookdata que el runbook reconocerá. Este valor debe ser un objeto con las mismas [propiedades](#details-of-a-webhook) que $Webhookdata para que el runbook pueda funcionar correctamente con él con si trabajara con WebhookData pasado por un webhook.

Por ejemplo, si se está iniciando el siguiente runbook desde Azure Portal y quiere pasar algún WebhookData de ejemplo para prueba, porque WebhookData es un objeto, se debe pasar como JSON en la interfaz de usuario.

![Parámetro WebhookData de la interfaz de usuario](media/automation-webhooks/WebhookData-parameter-from-UI.png)

Para el siguiente runbook, si tiene las siguientes propiedades para el parámetro WebhookData:

* WebhookName: *MyWebhook*
* RequestBody: *[{'ResourceGroup': 'myResourceGroup','Name': 'vm01'},{'ResourceGroup': 'myResourceGroup','Name': 'vm02'}]*

Luego pasaría el siguiente valor JSON en la interfaz de usuario para el parámetro WebhookData. El siguiente ejemplo con los retornos de carro y los caracteres de nueva línea coincide con el formato que se pasa desde un webhook.

```json
{"WebhookName":"mywebhook","RequestBody":"[\r\n {\r\n \"ResourceGroup\": \"vm01\",\r\n \"Name\": \"vm01\"\r\n },\r\n {\r\n \"ResourceGroup\": \"vm02\",\r\n \"Name\": \"vm02\"\r\n }\r\n]"}
```

![Iniciar el parámetro WebhookData de la interfaz de usuario](media/automation-webhooks/Start-WebhookData-parameter-from-UI.png)

> [!NOTE]
> Los valores de todos los parámetros de entrada se registran con el trabajo de runbook. Esto significa que se registrará cualquier entrada que proporcione el cliente en la solicitud de webhook y que estará disponible para cualquiera con acceso al trabajo de automatización.  Por este motivo, debe tener cuidado en cómo incluir información confidencial en las llamadas de webhook.

## <a name="security"></a>Seguridad

La seguridad de un webhook se basa en la privacidad de su dirección URL, que contiene un token de seguridad que permite que se invoque. Azure Automation no realiza ninguna autenticación en la solicitud siempre que se haga en la dirección URL correcta. Por esta razón, no deben utilizarse webhooks de runbooks que realicen funciones altamente confidenciales sin utilizar medios alternativos para validar la solicitud.

Puede incluir lógica en el runbook para determinar que se llamó por un webhook; para ello, active la propiedad **WebhookName** del parámetro $WebhookData. El runbook puede realizar más validaciones buscando información concreta en las propiedades **RequestHeader** o **RequestBody**.

Otra estrategia es hacer que el runbook realice alguna validación de una condición externa cuando recibe una solicitud de webhook. Por ejemplo, considere un runbook llamado por GitHub cuando hay una nueva confirmación a un repositorio de GitHub. El runbook puede conectarse a GitHub para asegurarse de que una nueva confirmación se ha realizado antes de continuar.

## <a name="creating-a-webhook"></a>Creación de un webhook

Use el procedimiento siguiente para crear un nuevo Webhook vinculado a un Runbook en el Portal de Azure.

1. Desde la **página Runbooks** en Azure Portal, haga clic en el runbook que el webhook inicia para ver su página de detalles. Asegúrese de que el **Estado** del runbook es **Publicado**.
2. Haga clic en **Webhook** en la parte superior de la página para abrir la página **Add Webhook** (Agregar webhook).
3. Haga clic en **Create new webhook** (Create new webhook) para abrir la página **Create webhook** (Crear webhook).
4. Especifique el **nombre** y la **fecha de expiración** del webhook y si debe habilitarse. Vea [Detalles de un webhook](#details-of-a-webhook) para más información sobre estas propiedades.
5. Haga clic en el icono de copiar y presione Ctrl+C para copiar la dirección URL del webhook. A continuación, guárdela en un lugar seguro. **Una vez creado el webhook, la dirección URL no se podrá volver a recuperar.**

   ![Dirección URL de Webhook](media/automation-webhooks/copy-webhook-url.png)

1. Haga clic en **Parámetros** para especificar los valores de los parámetros del runbook. Si el runbook tiene parámetros obligatorios, no puede crear el webhook, salvo que se especifiquen los valores.
1. Haga clic en **Crear** para crear el proyecto.

## <a name="using-a-webhook"></a>Uso de un webhook

Para utilizar un webhook después de que se haya creado, la aplicación cliente debe emitir una solicitud HTTP POST con la dirección URL del webhook. La sintaxis del webhook tiene el formato siguiente:

```http
http://<Webhook Server>/token?=<Token Value>
```

El cliente recibe uno de los siguientes códigos de retorno de la solicitud POST.

| Código | Texto | Descripción |
|:--- |:--- |:--- |
| 202 |Accepted |La solicitud se aceptó y el runbook se puso en cola correctamente. |
| 400 |Bad Request |No se aceptó la solicitud por uno de los siguientes motivos: <ul> <li>El webhook ha expirado.</li> <li>El webhook está deshabilitado.</li> <li>El token de la dirección URL no es válido.</li>  </ul> |
| 404 |No encontrado |No se aceptó la solicitud por uno de los siguientes motivos: <ul> <li>No se encontró el webhook.</li> <li>No se encontró el runbook.</li> <li>No se encontró la cuenta.</li>  </ul> |
| 500 |Internal Server Error |La dirección URL es válida, pero se produjo un error. Vuelva a enviar la solicitud. |

Asumiendo que la solicitud sea correcta, la respuesta del webhook contendrá el identificador de trabajo en formato JSON como se muestra a continuación. Contendrá un solo identificador de trabajo, pero el formato JSON permite realizar potenciales mejoras en el futuro.

```json
{"JobIds":["<JobId>"]}
```

El cliente no puede determinar cuando se completa el trabajo del runbook ni su estado de finalización a partir del webhook. Sin embargo, puede determinar esta información si usa el identificador de trabajo con otro método como [Windows PowerShell](https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azureautomationjob) o la [API de Azure Automation](/rest/api/automation/job).

## <a name="renew-webhook"></a>Renovar un webhook

Cuando se crea un webhook, este tiene un tiempo de validez de diez años. Después de ese período, el webhook caduca automáticamente. Cuando expira un webhook, no puede volver a activarse; en este caso, hay que quitarlo y volver a crearlo. Si un webhook no ha alcanzado su tiempo de expiración, su duración puede extenderse.

Para extender un webhook, desplácese hasta el runbook que contiene el webhook. Seleccione **Webhooks** en **Recursos**. Haga clic en el webhook que quiere extender; esta acción abre la página **Webhook**.  Elija una nueva fecha y hora de expiración y haga clic en **Guardar**.

## <a name="sample-runbook"></a>Runbook de ejemplo

El siguiente runbook de ejemplo acepta los datos de webhook e inicia las máquinas virtuales especificadas en el cuerpo de la solicitud. Para probar este runbook, en su cuenta de Automation bajo **Runbooks**, haga clic en **+ Agregar un runbook**. Si no sabe cómo crear un runbook, consulte [Creación de un runbook](automation-quickstart-create-runbook.md).

```powershell
param
(
    [Parameter (Mandatory = $false)]
    [object] $WebhookData
)



# If runbook was called from Webhook, WebhookData will not be null.
if ($WebhookData) {

    # Check header for message to validate request
    if ($WebhookData.RequestHeader.message -eq 'StartedbyContoso')
    {
        Write-Output "Header has required information"}
    else
    {
        Write-Output "Header missing required information";
        exit;
    }

    # Retrieve VM's from Webhook request body
    $vms = (ConvertFrom-Json -InputObject $WebhookData.RequestBody)

    # Authenticate to Azure by using the service principal and certificate. Then, set the subscription.

    Write-Output "Authenticating to Azure with service principal and certificate"
    $ConnectionAssetName = "AzureRunAsConnection"
    Write-Output "Get connection asset: $ConnectionAssetName"

    $Conn = Get-AutomationConnection -Name $ConnectionAssetName
            if ($Conn -eq $null)
            {
                throw "Could not retrieve connection asset: $ConnectionAssetName. Check that this asset exists in the Automation account."
            }
            Write-Output "Authenticating to Azure with service principal."
            Add-AzureRmAccount -ServicePrincipal -Tenant $Conn.TenantID -ApplicationId $Conn.ApplicationID -CertificateThumbprint $Conn.CertificateThumbprint | Write-Output

        # Start each virtual machine
        foreach ($vm in $vms)
        {
            $vmName = $vm.Name
            Write-Output "Starting $vmName"
            Start-AzureRMVM -Name $vm.Name -ResourceGroup $vm.ResourceGroup
        }
}
else {
    # Error
    write-Error "This runbook is meant to be started from an Azure alert webhook only."
}
```

## <a name="test-the-example"></a>Probar el ejemplo

En el ejemplo siguiente se usa Windows PowerShell para iniciar un runbook con un webhook. Cualquier lenguaje que pueda realizar una solicitud HTTP puede utilizar un webhook; Windows PowerShell se usa aquí como ejemplo.

El runbook espera una lista de máquinas virtuales con formato JSON en el cuerpo de la solicitud. El runbook valida también que las cabeceras contengan un mensaje definido para validar que el autor de llamada del webhook es válido.

```azurepowershell-interactive
$uri = "<webHook Uri>"

$vms  = @(
            @{ Name="vm01";ResourceGroup="vm01"},
            @{ Name="vm02";ResourceGroup="vm02"}
        )
$body = ConvertTo-Json -InputObject $vms
$header = @{ message="StartedbyContoso"}
$response = Invoke-WebRequest -Method Post -Uri $uri -Body $body -Headers $header
$jobid = (ConvertFrom-Json ($response.Content)).jobids[0]
```

En el siguiente ejemplo se muestra el cuerpo de la solicitud que está disponible para el runbook en la propiedad **RequestBody** de **WebhookData**. Este valor tiene el formato JSON porque ese era el formato que se incluía en el cuerpo de la solicitud.

```json
[
    {
        "Name":  "vm01",
        "ResourceGroup":  "myResourceGroup"
    },
    {
        "Name":  "vm02",
        "ResourceGroup":  "myResourceGroup"
    }
]
```

La siguiente imagen muestra la solicitud que se envía desde Windows PowerShell y la respuesta resultante. El identificador de trabajo se extrae de la respuesta y se convierte en una cadena.

![Botón Webhooks](media/automation-webhooks/webhook-request-response.png)

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre cómo utilizar Azure Automation para tomar medidas relativas a las alertas de Azure, consulte [Uso de una alerta para desencadenar un runbook de Azure Automation](automation-create-alert-triggered-runbook.md).

