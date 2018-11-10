---
title: Obtener datos de cumplimiento en Azure Policy
description: Las evaluaciones y los efectos de Azure Policy determinan el cumplimiento. Obtenga información sobre cómo obtener los detalles de cumplimiento.
services: azure-policy
author: DCtheGeek
ms.author: dacoulte
ms.date: 10/29/2018
ms.topic: conceptual
ms.service: azure-policy
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: f88e68150aa2708557775df2719409228166520b
ms.sourcegitcommit: fbdfcac863385daa0c4377b92995ab547c51dd4f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2018
ms.locfileid: "50233419"
---
# <a name="getting-compliance-data"></a>Obtención de datos de cumplimiento

Una de las ventajas más grandes de Azure Policy es la información y los controles que proporciona sobre los recursos de una suscripción o [grupo de administración](../../management-groups/overview.md) de suscripciones. Este control puede ejercerse de muchas maneras diferentes, por ejemplo: evitar que los recursos se creen en una ubicación incorrecta, forzar un uso común y coherente de las etiquetas, o auditar los recursos existentes para las opciones y configuraciones apropiadas. En todos los casos, Policy genera los datos para que pueda conocer el estado de cumplimiento de su entorno.

Hay varias maneras de acceder a la información de cumplimiento generada por la directiva y las asignaciones de iniciativa:

- Usando [Azure Portal](#portal)
- Mediante scripting de [línea de comandos](#command-line)

Antes de pasar a los métodos que informan sobre el cumplimiento, veamos cuándo se actualiza la información de cumplimiento y la frecuencia y eventos que desencadenan un ciclo de evaluación.

> [!WARNING]
> Si el estado de cumplimiento se notifica como **No registrado**, compruebe que el proveedor de recursos **Microsoft.PolicyInsights** esté registrado y que el usuario tenga los permisos de control de acceso basado en rol (RBAC) adecuados, tal como se describe [aquí](../overview.md#rbac-permissions-in-azure-policy).

## <a name="evaluation-triggers"></a>Desencadenadores de evaluación

Los resultados de un ciclo de evaluación completo se reflejan en el proveedor de recursos `Microsoft.PolicyInsights` por medio de las operaciones `PolicyStates` y `PolicyEvents`. Para obtener más información sobre las opciones y capacidades de la API de REST de Policy Insights, vea [Policy Insights](/rest/api/policy-insights/).

Las evaluaciones de directivas asignadas e iniciativas se producen como resultado de varios eventos:

- Una directiva o iniciativa se asigna recientemente a un ámbito. Cuando esto ocurre, la asignación tarda unos 30 minutos en aplicarse al ámbito definido. Una vez que se aplica, comienza el ciclo de evaluación de recursos dentro de ese ámbito con la directiva o iniciativa recién asignada y, dependiendo de los efectos usados por la directiva o iniciativa, los recursos se marcan como compatibles o no compatibles. Una directiva o iniciativa grande evaluada en un ámbito amplio de recursos puede tardar bastante tiempo, por lo que no hay una predicción de cuándo se completará el ciclo de la evaluación. Una vez completado, los resultados de cumplimiento actualizados están disponibles en el portal y en los SDK.
- Una directiva o iniciativa que ya está asignada a un ámbito se actualiza. El ciclo de evaluación y control de tiempo en este escenario es el mismo que para una nueva asignación a un ámbito.
- Un recurso se implementa en un ámbito con una asignación a través del Administrador de recursos, REST, CLI de Azure o Azure PowerShell. En este escenario, el evento de efecto (anexar, auditar, denegar, implementar) y la información de estado de cumplimiento para el recurso individual están disponibles en el portal y en los SDK unos 15 minutos más tarde. Este evento no causa una evaluación de otros recursos.
- Ciclo de evaluación de cumplimiento estándar. Una vez cada 24 horas, las asignaciones se evalúan automáticamente. Una directiva o iniciativa grande evaluada en un ámbito amplio de recursos puede tardar bastante tiempo, por lo que no hay una predicción de cuándo se completará el ciclo de la evaluación. Una vez completado, los resultados de cumplimiento actualizados están disponibles en el portal y en los SDK.
- Examen a petición

### <a name="on-demand-evaluation-scan"></a>Examen de evaluación a petición

Con una llamada a la API de REST se puede iniciar un examen de evaluación de una suscripción o un grupo de recursos. Se trata de un proceso asincrónico. Por ello, el punto de conexión de REST que iniciará el examen no espera hasta que este esté completo para responder. En su lugar, proporciona un URI para consultar el estado de la evaluación solicitada.

En cada identificador URI de la API REST, hay variables usadas que se deben reemplazar por sus propios valores:

- `{YourRG}`: reemplácelo por el nombre del grupo de recursos
- `{subscriptionId}`: reemplácelo por el identificador de suscripción

El examen admite la evaluación de recursos de una suscripción o de un grupo de recursos. Inicie un examen para el ámbito deseado con un comando **POST** de API REST mediante las siguientes estructuras de URI:

- Subscription

  ```http
  POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/triggerEvaluation?api-version=2018-07-01-preview
  ```

- Grupos de recursos

  ```http
  POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{YourRG}/providers/Microsoft.PolicyInsights/policyStates/latest/triggerEvaluation?api-version=2018-07-01-preview
  ```

La llamada devuelve un estado **202 - Aceptado**. En el encabezado de la respuesta se incluye una propiedad **Location** con el formato siguiente:

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/asyncOperationResults/{ResourceContainerGUID}?api-version=2018-07-01-preview
```

`{ResourceContainerGUID}` se genera estáticamente para el ámbito solicitado. Si un ámbito ya está realizando un examen a petición, no se iniciará un nuevo examen. En su lugar, se proporciona a la nueva solicitud el mismo URI de `{ResourceContainerGUID}` **Location** para el estado. Un comando **GET** de API REST en el URI **Location** devolverá una respuesta **202 - Aceptado** mientras la evaluación esté en curso. Cuando haya finalizado el examen de evaluación, devolverá un estado **200 OK**. El cuerpo de un examen completo es una respuesta JSON con el estado:

```json
{
    "status": "Succeeded"
}
```

## <a name="how-compliance-works"></a>Cómo funciona el cumplimiento

En una asignación, un recurso es **No compatible** si no cumple las reglas de iniciativa o directiva. En la tabla siguiente se muestra cómo funcionan los distintos efectos de directiva con la evaluación de condición para el estado de cumplimiento resultante:

| Estado del recurso | Efecto | Evaluación de directiva | Estado de cumplimiento |
| --- | --- | --- | --- |
| Exists | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | True | Incompatible |
| Exists | Deny, Audit, Append\*, DeployIfNotExist\*, AuditIfNotExist\* | False | Compatible |
| Nuevo | Audit, AuditIfNotExist\* | True | Incompatible |
| Nuevo | Audit, AuditIfNotExist\* | False | Compatible |

\* Los efectos Append, DeployIfNotExist y AuditIfNotExist requieren que la instrucción IF sea TRUE.
Los efectos requieren también que la condición de existencia sea FALSE para ser no compatibles. Si es TRUE, la condición IF desencadena la evaluación de la condición de existencia de los recursos relacionados.

Por ejemplo, suponga que tiene un grupo de recursos (ContosoRG) con varias cuentas de almacenamiento (resaltadas en rojo) expuestas a redes públicas.

![Cuentas de almacenamiento expuestas a redes públicas](../media/getting-compliance-data/resource-group01.png)

En este ejemplo, debe tener cuidado con los riesgos de seguridad. Ahora que ha creado una asignación de directiva, se evalúa para todas las cuentas de almacenamiento en el grupo de recursos ContosoRG. Se auditan las tres cuentas de almacenamiento no compatibles, con lo que sus estados cambian a **No compatible**.

![Cuentas de almacenamiento auditadas no compatibles](../media/getting-compliance-data/resource-group03.png)

Además de **Conforme** y **No compatible**, las directivas y los recursos tienen otros tres estados:

- **En conflicto**: existen dos o más directivas con reglas en conflicto (por ejemplo, dos directivas que anexan la misma etiqueta con valores diferentes).
- **No iniciado**: no se ha iniciado el ciclo de evaluación de la directiva o del recurso.
- **No registrado**: no se ha registrado el proveedor de recursos de Azure Policy o la cuenta con que se ha iniciado sesión no tiene permiso para leer datos de cumplimiento.

La directiva utiliza los campos **Tipo** y **Nombre** en la definición de la regla de la directiva para determinar si un recurso coincide. Si es así, se considera aplicable y tendría un estado de **Conforme** o **No compatible**. Si **Tipo** o **Nombre** son las únicas propiedades en la definición de la regla de la directiva, todos los recursos se consideran aplicables y se evaluarán.

El porcentaje de cumplimiento se determina al dividir los recursos **Conforme** por el _total de recursos_.
El _total de recursos_ se define como la suma de los recursos **Conforme**, **No compatible** y **En conflicto**. La cifra de cumplimiento general es la suma de los distintos recursos que son **Conforme** dividida por la suma de todos los recursos distintos. En la imagen siguiente, hay 20 recursos diferentes que son aplicables y solo uno es **No compatible**. Por tanto, el cumplimiento general de recursos es 19/20 o 95 %.

![Ejemplo de cumplimiento simple](../media/getting-compliance-data/simple-compliance.png)

## <a name="portal"></a>Portal

Azure Portal presenta de forma gráfica la visualización y el reconocimiento del estado de cumplimiento de normas en su entorno. En la página **Directiva**, la opción **Información general** proporciona detalles sobre el cumplimiento de directivas e iniciativas por parte de los ámbitos disponibles. Además del recuento y el estado de cumplimiento por asignación, contiene un gráfico que muestra el cumplimiento durante los últimos siete días. La página **Cumplimiento** contiene gran parte de esta misma información (excepto el gráfico), pero ofrece más opciones de filtrado y ordenación.

![Página cumplimiento de directiva](../media/getting-compliance-data/compliance-page.png)

Dado que una directiva o iniciativa se puede asignar a distintos ámbitos, observe en la tabla el ámbito de cada asignación y el tipo de definición que se asignó a ese ámbito. También se proporciona el número de directivas y recursos no compatibles para cada asignación. Al hacer clic en una directiva o iniciativa de la tabla, se proporciona más información sobre el cumplimiento de esa asignación en particular.

![Detalles de cumplimiento de directiva](../media/getting-compliance-data/compliance-details.png)

La lista de recursos de la pestaña **Cumplimiento de los recursos** (que de manera predeterminada es **No compatible**, si bien se puede filtrar) refleja el estado de evaluación de los recursos existentes de la asignación actual, mientras que los eventos (anexar, auditar, denegar, implementar) desencadenados por la solicitud para crear un recurso se muestran en la pestaña **Eventos**.

![Eventos de cumplimiento de directiva](../media/getting-compliance-data/compliance-events.png)

Haga clic con el botón derecho en la fila del evento sobre el que quiere recopilar información más detallada y seleccione **Mostrar los registros de actividad**. Se abre la página de registro de actividad y se filtra previamente para mostrar detalles de la asignación y los eventos. El registro de actividad proporciona contexto e información adicionales sobre esos eventos.

![Registro de actividades de cumplimiento de directiva](../media/getting-compliance-data/compliance-activitylog.png)

## <a name="command-line"></a>Línea de comandos

La misma información que está disponible en el portal se puede recuperar directamente mediante la API de REST (incluso con [ARMClient](https://github.com/projectkudu/ARMClient)) o Azure PowerShell con la API de REST. Para obtener detalles completos sobre la API de REST, consulte la referencia de [Policy Insights](/rest/api/policy-insights/). Las páginas de referencia de la API de REST tienen un botón verde en cada operación para realizar una prueba en el explorador.

Para usar los siguientes ejemplos en Azure PowerShell, cree un token de autenticación con este código de ejemplo. A continuación, reemplace el $restUri por la cadena que quiera en los ejemplos para recuperar un objeto JSON que, a continuación, se puede analizar.

```azurepowershell-interactive
# Login first with Connect-AzureRmAccount if not using Cloud Shell

$azContext = Get-AzureRmContext
$azProfile = [Microsoft.Azure.Commands.Common.Authentication.Abstractions.AzureRmProfileProvider]::Instance.Profile
$profileClient = New-Object -TypeName Microsoft.Azure.Commands.ResourceManager.Common.RMProfileClient -ArgumentList ($azProfile)
$token = $profileClient.AcquireAccessToken($azContext.Subscription.TenantId)
$authHeader = @{
    'Content-Type'='application/json'
    'Authorization'='Bearer ' + $token.AccessToken
}

# Define the REST API to communicate with
# Use double quotes for $restUri as some endpoints take strings passed in single quotes
$restUri = "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/summarize?api-version=2018-04-04"

# Invoke the REST API
$response = Invoke-RestMethod -Uri $restUri -Method POST -Headers $authHeader

# View the response object (as JSON)
$response
```

### <a name="summarize-results"></a>Resumen de resultados

Con la API de REST, puede realizarse un resumen por grupo de administración, suscripción, grupo de recursos, recurso, iniciativa, directiva, asignación de nivel de suscripción o asignación de nivel de grupo de recursos. A continuación se muestra un ejemplo de resumen en el nivel de suscripción con [Resumen de la suscripción](/rest/api/policy-insights/policystates/summarizeforsubscription) de Policy Insights:

```http
POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/summarize?api-version=2018-04-04
```

La salida resume la suscripción. En la salida de ejemplo siguiente, la compatibilidad resumida está en **value.results.nonCompliantResources** y **value.results.nonCompliantPolicies**. Esta solicitud proporciona más detalles, incluida cada asignación que compone los números no compatibles y la información de definición de cada asignación. Cada objeto de directiva de la jerarquía proporciona un **queryResultsUri** que puede utilizarse para obtener detalles adicionales en ese nivel.

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#summary",
    "@odata.count": 1,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#summary/$entity",
        "results": {
            "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false",
            "nonCompliantResources": 15,
            "nonCompliantPolicies": 1
        },
        "policyAssignments": [{
            "policyAssignmentId": "/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77",
            "policySetDefinitionId": "",
            "results": {
                "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77'",
                "nonCompliantResources": 15,
                "nonCompliantPolicies": 1
            },
            "policyDefinitions": [{
                "policyDefinitionReferenceId": "",
                "policyDefinitionId": "/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
                "effect": "deny",
                "results": {
                    "queryResultsUri": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77' and PolicyDefinitionId eq '/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62'",
                    "nonCompliantResources": 15
                }
            }]
        }]
    }]
}
```

### <a name="query-for-resources"></a>Consulta de recursos

En el ejemplo anterior, **value.policyAssignments.policyDefinitions.results.queryResultsUri** nos proporcionó un Uri de ejemplo para obtener todos los recursos no compatibles para una definición de directiva específica. Examinando el valor de **$filter**, IsCompliant es igual (eq) a false, se especifica PolicyAssignmentId para la definición de directiva y, después, el propio PolicyDefinitionId.
La razón para incluir PolicyAssignmentId en el filtro es que PolicyDefinitionId podría existir en varias asignaciones de directivas o de iniciativas con diversos ámbitos. Al especificar PolicyAssignmentId y PolicyDefinitionId, podremos ser explícitos en los resultados que estamos buscando. Anteriormente, hemos usado **latest** para PolicyStates (el único valor permitido para **policyStatesSummaryResource** en el operador de resumen de la suscripción), que establece automáticamente el período de tiempo **from** y **to** en las últimas 24 horas.

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/latest/queryResults?api-version=2018-04-04&$from=2018-05-18 04:28:22Z&$to=2018-05-19 04:28:22Z&$filter=IsCompliant eq false and PolicyAssignmentId eq '/subscriptions/{subscriptionId}/resourcegroups/rg-tags/providers/microsoft.authorization/policyassignments/37ce239ae4304622914f0c77' and PolicyDefinitionId eq '/providers/microsoft.authorization/policydefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62'
```

Por razones de brevedad, la respuesta de ejemplo siguiente se ha truncado para mostrar únicamente un solo recurso no compatible (tenga en cuenta que @odata.count es realmente 15 y coincide con el número de recursos no compatibles del ejemplo anterior). La respuesta detallada proporciona varios elementos de datos sobre el recurso, la directiva (o iniciativa) y la asignación. Tenga en cuenta que también puede ver qué parámetros de asignación se han pasado a la definición de directiva.

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest",
    "@odata.count": 15,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyStates/$metadata#latest/$entity",
        "timestamp": "2018-05-19T04:41:09Z",
        "resourceId": "/subscriptions/{subscriptionId}/resourceGroups/rg-tags/providers/Microsoft.Compute/virtualMachines/linux",
        "policyAssignmentId": "/subscriptions/{subscriptionId}/resourceGroups/rg-tags/providers/Microsoft.Authorization/policyAssignments/37ce239ae4304622914f0c77",
        "policyDefinitionId": "/providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62",
        "effectiveParameters": "",
        "isCompliant": false,
        "subscriptionId": "{subscriptionId}",
        "resourceType": "/Microsoft.Compute/virtualMachines",
        "resourceLocation": "westus2",
        "resourceGroup": "RG-Tags",
        "resourceTags": "tbd",
        "policyAssignmentName": "37ce239ae4304622914f0c77",
        "policyAssignmentOwner": "tbd",
        "policyAssignmentParameters": "{\"tagName\":{\"value\":\"costCenter\"},\"tagValue\":{\"value\":\"Contoso-Test\"}}",
        "policyAssignmentScope": "/subscriptions/{subscriptionId}/resourceGroups/RG-Tags",
        "policyDefinitionName": "1e30110a-5ceb-460c-a204-c1c3969c6d62",
        "policyDefinitionAction": "deny",
        "policyDefinitionCategory": "tbd",
        "policySetDefinitionId": "",
        "policySetDefinitionName": "",
        "policySetDefinitionOwner": "",
        "policySetDefinitionCategory": "",
        "policySetDefinitionParameters": "",
        "managementGroupIds": "",
        "policyDefinitionReferenceId": ""
    }]
}
```

### <a name="view-events"></a>Ver eventos

Cuando se crea o actualiza un recurso, se genera un resultado de evaluación de directiva. Los resultados se denominan _eventos de directiva_. Utilice el siguiente Uri para ver eventos recientes de directiva asociados a la suscripción.

```http
https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyEvents/default/queryResults?api-version=2018-04-04
```

Los resultados deben tener una apariencia similar al ejemplo siguiente:

```json
{
    "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyEvents/$metadata#default",
    "@odata.count": 1,
    "value": [{
        "@odata.id": null,
        "@odata.context": "https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.PolicyInsights/policyEvents/$metadata#default/$entity",
        "NumAuditEvents": 16
    }]
}
```

Para más información sobre cómo consultar los eventos de directiva, consulte el artículo de referencia de [estados de directiva](/rest/api/policy-insights/policyevents).

### <a name="azure-powershell"></a>Azure PowerShell

El módulo Azure PowerShell para Policy está disponible en la Galería de PowerShell como [AzureRM.PolicyInsights](https://www.powershellgallery.com/packages/AzureRM.PolicyInsights). Con el uso de PowerShellGet, puede instalar el módulo con `Install-Module -Name AzureRM.PolicyInsights` (asegúrese de tener instalada la última versión de [Azure PowerShell](/powershell/azure/install-azurerm-ps)):

```azurepowershell-interactive
# Install from PowerShell Gallery via PowerShellGet
Install-Module -Name AzureRM.PolicyInsights

# Import the downloaded module
Import-Module AzureRM.PolicyInsights

# Login with Connect-AzureRmAccount if not using Cloud Shell
Connect-AzureRmAccount
```

El módulo tiene tres cmdlets:

- `Get-AzureRmPolicyStateSummary`
- `Get-AzureRmPolicyState`
- `Get-AzureRmPolicyEvent`

Ejemplo: Obtener el resumen de estado para la directiva asignada superior con el mayor número de recursos no compatibles.

```azurepowershell-interactive
PS> Get-AzureRmPolicyStateSummary -Top 1

NonCompliantResources : 15
NonCompliantPolicies  : 1
PolicyAssignments     : {/subscriptions/{subscriptionId}/resourcegroups/RG-Tags/providers/micros
                        oft.authorization/policyassignments/37ce239ae4304622914f0c77}
```

Ejemplo: Obtener el registro de estado para el recurso evaluado más recientemente (el valor predeterminado es por marca de tiempo en orden descendente).

```azurepowershell-interactive
PS> Get-AzureRmPolicyState -Top 1

Timestamp                  : 5/22/2018 3:47:34 PM
ResourceId                 : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Network/networkInterfaces/linux316
PolicyAssignmentId         : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Authorization/policyAssignments/37ce239ae4304622914f0c77
PolicyDefinitionId         : /providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62
IsCompliant                : False
SubscriptionId             : {subscriptionId}
ResourceType               : /Microsoft.Network/networkInterfaces
ResourceLocation           : westus2
ResourceGroup              : RG-Tags
ResourceTags               : tbd
PolicyAssignmentName       : 37ce239ae4304622914f0c77
PolicyAssignmentOwner      : tbd
PolicyAssignmentParameters : {"tagName":{"value":"costCenter"},"tagValue":{"value":"Contoso-Test"}}
PolicyAssignmentScope      : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags
PolicyDefinitionName       : 1e30110a-5ceb-460c-a204-c1c3969c6d62
PolicyDefinitionAction     : deny
PolicyDefinitionCategory   : tbd
```

Ejemplo: Obtener los detalles de todos los recursos de red virtual no compatibles.

```azurepowershell-interactive
PS> Get-AzureRmPolicyState -Filter "ResourceType eq '/Microsoft.Network/virtualNetworks'"

Timestamp                  : 5/22/2018 4:02:20 PM
ResourceId                 : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Network/virtualNetworks/RG-Tags-vnet
PolicyAssignmentId         : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Authorization/policyAssignments/37ce239ae4304622914f0c77
PolicyDefinitionId         : /providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62
IsCompliant                : False
SubscriptionId             : {subscriptionId}
ResourceType               : /Microsoft.Network/virtualNetworks
ResourceLocation           : westus2
ResourceGroup              : RG-Tags
ResourceTags               : tbd
PolicyAssignmentName       : 37ce239ae4304622914f0c77
PolicyAssignmentOwner      : tbd
PolicyAssignmentParameters : {"tagName":{"value":"costCenter"},"tagValue":{"value":"Contoso-Test"}}
PolicyAssignmentScope      : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags
PolicyDefinitionName       : 1e30110a-5ceb-460c-a204-c1c3969c6d62
PolicyDefinitionAction     : deny
PolicyDefinitionCategory   : tbd
```

Ejemplo: Obtener eventos relacionados con recursos de red virtual no compatibles que se produjeron después de una fecha concreta.

```azurepowershell-interactive
PS> Get-AzureRmPolicyEvent -Filter "ResourceType eq '/Microsoft.Network/virtualNetworks'" -From '2018-05-19'

Timestamp                  : 5/19/2018 5:18:53 AM
ResourceId                 : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Network/virtualNetworks/RG-Tags-vnet
PolicyAssignmentId         : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags/providers/Mi
                             crosoft.Authorization/policyAssignments/37ce239ae4304622914f0c77
PolicyDefinitionId         : /providers/Microsoft.Authorization/policyDefinitions/1e30110a-5ceb-460c-a204-c1c3969c6d62
IsCompliant                : False
SubscriptionId             : {subscriptionId}
ResourceType               : /Microsoft.Network/virtualNetworks
ResourceLocation           : eastus
ResourceGroup              : RG-Tags
ResourceTags               : tbd
PolicyAssignmentName       : 37ce239ae4304622914f0c77
PolicyAssignmentOwner      : tbd
PolicyAssignmentParameters : {"tagName":{"value":"costCenter"},"tagValue":{"value":"Contoso-Test"}}
PolicyAssignmentScope      : /subscriptions/{subscriptionId}/resourceGroups/RG-Tags
PolicyDefinitionName       : 1e30110a-5ceb-460c-a204-c1c3969c6d62
PolicyDefinitionAction     : deny
PolicyDefinitionCategory   : tbd
TenantId                   : {tenantId}
PrincipalOid               : {principalOid}
```

El campo **PrincipalOid** se puede utilizar para obtener un usuario específico con el cmdlet `Get-AzureRmADUser` de Azure PowerShell. Reemplace **{principalOid}** por la respuesta que se obtiene del ejemplo anterior.

```azurepowershell-interactive
PS> (Get-AzureRmADUser -ObjectId {principalOid}).DisplayName
Trent Baker
```

## <a name="log-analytics"></a>Log Analytics

Si tiene un área de trabajo de [Log Analytics](../../../log-analytics/log-analytics-overview.md) con la solución `AzureActivity` vinculada a su suscripción, entonces también puede ver los resultados no compatibles del ciclo de evaluación mediante consultas sencillas de Kusto y la tabla `AzureActivity`. Con detalles de no cumplimiento en Log Analytics, esto también significa que se pudieron configurar alertas para comprobar el cumplimiento de un determinado recurso, grupo de recursos o incluso un umbral de elementos no compatibles, como más de 10 en las últimas 24 horas.

![Cumplimiento de directiva con Log Analytics](../media/getting-compliance-data/compliance-loganalytics.png)

## <a name="next-steps"></a>Pasos siguientes

- Puede consultar ejemplos en [Ejemplos de Azure Policy](../samples/index.md)
- Consulte la [Estructura de definición de directivas](../concepts/definition-structure.md)
- Consulte [Descripción de los efectos de directivas](../concepts/effects.md)
- Entender cómo se pueden [crear directivas mediante programación](programmatically-create.md)
- Descubrir cómo se pueden [corregir recursos no compatibles](remediate-resources.md)
- En [Organización de los recursos con grupos de administración de Azure](../../management-groups/overview.md), obtendrá información sobre lo que es un grupo de administración.