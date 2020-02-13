---
title: Recursos de etiquetado para la organización lógica
description: Muestra cómo aplicar etiquetas para organizar los recursos de Azure para la facturación y administración.
ms.topic: conceptual
ms.date: 01/03/2020
ms.openlocfilehash: c7f8d8672e205fa677bff33c8ed173c1105b26c6
ms.sourcegitcommit: bdf31d87bddd04382effbc36e0c465235d7a2947
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2020
ms.locfileid: "77166601"
---
# <a name="use-tags-to-organize-your-azure-resources"></a>Uso de etiquetas para organizar los recursos de Azure

Se aplican etiquetas a los recursos de Azure para organizarlos de forma lógica en una taxonomía. Cada etiqueta consta de un nombre y un par de valores. Por ejemplo, puede aplicar el nombre "Environment" y el valor "Production" a todos los recursos en producción.

Después de aplicar las etiquetas, puede recuperar todos los recursos de la suscripción que tengan ese nombre y valor de etiqueta. Las etiquetas le permiten recuperar los recursos relacionados que se encuentran en distintos grupos de recursos. Este enfoque puede resultar útil si necesita organizar recursos para facturación o administración.

La taxonomía debe considerar una estrategia de autoservicio de etiquetado de metadatos además de una estrategia de autoetiquetado para reducir la carga sobre los usuarios y aumentar la precisión.

[!INCLUDE [Handle personal data](../../../includes/gdpr-intro-sentence.md)]

## <a name="limitations"></a>Limitaciones

Se aplican las siguientes limitaciones a las etiquetas:

* No todos los tipos de recursos admiten etiquetas. Para determinar si se puede aplicar una etiqueta a un tipo de recurso determinado, consulte [Tag support for Azure resources](tag-support.md) (Compatibilidad con etiquetas para los recursos de Azure).
* Cada recurso o grupo de recursos puede tener un máximo de cincuenta pares de nombre/valor de etiqueta. Si necesita aplicar más etiquetas que el número máximo permitido, use una cadena JSON para el valor de etiqueta. La cadena JSON puede contener muchos valores que se aplican a un sol nombre de etiqueta. Un grupo de recursos puede contener muchos recursos con cincuenta pares de clave/valor de etiqueta cada uno.
* El nombre de etiqueta está limitado a 512 caracteres y el valor de la etiqueta, a 256. En las cuentas de almacenamiento, el nombre de etiqueta se limita a 128 caracteres y el valor de la etiqueta, a 256.
* Las VM generalizadas no admiten etiquetas.
* Los recursos de un grupo de recursos no heredan las etiquetas aplicadas a este.
* No se pueden aplicar etiquetas a recursos clásicos como Cloud Services.
* Los nombres de etiqueta no pueden contener estos caracteres: `<`, `>`, `%`, `&`, `\`, `?`, `/`

   > [!NOTE]
   > Actualmente las zonas de Azure DNS y los servicios de Traffic Manager tampoco permiten el uso de espacios en la etiqueta. 

## <a name="required-access"></a>Acceso necesario

Para aplicar etiquetas a los recursos, el usuario debe tener acceso de escritura en ese tipo de recurso. Para aplicar etiquetas a todos los tipos de recursos, use el rol [Colaborador](../../role-based-access-control/built-in-roles.md#contributor). Para aplicar etiquetas a un solo tipo de recurso, use el rol de colaborador para ese recurso. Por ejemplo, para aplicar etiquetas a las máquinas virtuales, use [Colaborador de la máquina virtual](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor).

## <a name="policies"></a>Directivas

Puede usar [Azure Policy](../../governance/policy/overview.md) para aplicar reglas de etiquetado y convenciones. Mediante la creación de una directiva, evitará el escenario de recursos que se vayan a implementar en su suscripción que no cumplen con las etiquetas esperadas para su organización. En lugar de aplicar manualmente las etiquetas o buscar recursos que no son compatibles, puede crear una directiva que aplica automáticamente las etiquetas necesarias durante la implementación. También se pueden aplicar etiquetas a los recursos existentes con el nuevo efecto de [modificación](../../governance/policy/concepts/effects.md#modify) y una [tarea de corrección](../../governance/policy/how-to/remediate-resources.md). La sección siguiente muestra las directivas de ejemplo para las etiquetas.

[!INCLUDE [Tag policies](../../../includes/azure-policy-samples-policies-tags.md)]

## <a name="powershell"></a>PowerShell

Para ver las etiquetas existentes de un *grupo de recursos*, use:

```azurepowershell-interactive
(Get-AzResourceGroup -Name examplegroup).Tags
```

Ese script devuelve el siguiente formato:

```powershell
Name                           Value
----                           -----
Dept                           IT
Environment                    Test
```

Para ver las etiquetas existentes para un *recurso que tiene un nombre y un grupo de recursos especificados*, use:

```azurepowershell-interactive
(Get-AzResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

O bien, si tiene el identificador de recurso de un recurso, puede pasar ese identificador de recurso para obtener las etiquetas.

```azurepowershell-interactive
(Get-AzResource -ResourceId /subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>).Tags
```

Para obtener *grupos de recursos que tengan un nombre y valor de etiqueta específicos*, use :

```azurepowershell-interactive
(Get-AzResourceGroup -Tag @{ "Dept"="Finance" }).ResourceGroupName
```

Para obtener *recursos que tengan un nombre y valor de etiqueta específicos*, use :

```azurepowershell-interactive
(Get-AzResource -Tag @{ "Dept"="Finance"}).Name
```

Para obtener *recursos que tengan un nombre de etiqueta específico*, use:

```azurepowershell-interactive
(Get-AzResource -TagName "Dept").Name
```

Cada vez que aplique etiquetas a un recurso o grupo de recursos, sobrescribirá las etiquetas existentes en ese recurso o grupo de recursos. Por lo tanto, tiene que utilizar un enfoque diferente en función de si el recurso o grupo de recursos tienen etiquetas existentes.

Para agregar etiquetas a un *grupo de recursos sin etiquetas existentes*, use:

```azurepowershell-interactive
Set-AzResourceGroup -Name examplegroup -Tag @{ "Dept"="IT"; "Environment"="Test" }
```

Para agregar etiquetas a un *grupo de recursos que ya tiene etiquetas*, recupere las etiquetas existentes, agregue la nueva y vuelva a aplicar todas:

```azurepowershell-interactive
$tags = (Get-AzResourceGroup -Name examplegroup).Tags
$tags.Add("Status", "Approved")
Set-AzResourceGroup -Tag $tags -Name examplegroup
```

Para agregar etiquetas a un *recurso sin etiquetas*, use:

```azurepowershell-interactive
$resource = Get-AzResource -ResourceName examplevnet -ResourceGroupName examplegroup
Set-AzResource -Tag @{ "Dept"="IT"; "Environment"="Test" } -ResourceId $resource.ResourceId -Force
```

Puede tener más de un recurso con el mismo nombre en un grupo de recursos. En ese caso, puede establecer cada recurso con los siguientes comandos:

```azurepowershell-interactive
$resource = Get-AzResource -ResourceName sqlDatabase1 -ResourceGroupName examplegroup
$resource | ForEach-Object { Set-AzResource -Tag @{ "Dept"="IT"; "Environment"="Test" } -ResourceId $_.ResourceId -Force }
```

Para agregar etiquetas a un *recurso que ya tiene etiquetas*, use:

```azurepowershell-interactive
$resource = Get-AzResource -ResourceName examplevnet -ResourceGroupName examplegroup
$resource.Tags.Add("Status", "Approved")
Set-AzResource -Tag $resource.Tags -ResourceId $resource.ResourceId -Force
```

Para aplicar todas las etiquetas de un grupo de recursos a sus recursos y *no conservar ninguna de las etiquetas existentes en los recursos*, use el siguiente script:

```azurepowershell-interactive
$group = Get-AzResourceGroup -Name examplegroup
Get-AzResource -ResourceGroupName $group.ResourceGroupName | ForEach-Object {Set-AzResource -ResourceId $_.ResourceId -Tag $group.Tags -Force }
```

Para aplicar todas las etiquetas de un grupo de recursos a sus recursos y *conservar las etiquetas existentes en los recursos que no son duplicados*, use el siguiente script:

```azurepowershell-interactive
$group = Get-AzResourceGroup -Name examplegroup
if ($null -ne $group.Tags) {
    $resources = Get-AzResource -ResourceGroupName $group.ResourceGroupName
    foreach ($r in $resources)
    {
        $resourcetags = (Get-AzResource -ResourceId $r.ResourceId).Tags
        if ($resourcetags)
        {
            foreach ($key in $group.Tags.Keys)
            {
                if (-not($resourcetags.ContainsKey($key)))
                {
                    $resourcetags.Add($key, $group.Tags[$key])
                }
            }
            Set-AzResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
        else
        {
            Set-AzResource -Tag $group.Tags -ResourceId $r.ResourceId -Force
        }
    }
}
```

Para quitar todas las etiquetas, pase una tabla hash vacía:

```azurepowershell-interactive
Set-AzResourceGroup -Tag @{} -Name examplegroup
```

## <a name="azure-cli"></a>Azure CLI

Para ver las etiquetas existentes de un *grupo de recursos*, use:

```azurecli-interactive
az group show -n examplegroup --query tags
```

Ese script devuelve el siguiente formato:

```json
{
  "Dept"        : "IT",
  "Environment" : "Test"
}
```

O bien, para ver las etiquetas existentes para un *recurso que tiene un nombre, un tipo y un grupo de recursos especificados*, use:

```azurecli-interactive
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

Al recorrer en iteración una colección de recursos, puede mostrar el identificador de recurso por recurso. Más adelante en este artículo se muestra un ejemplo completo. Para ver las etiquetas existentes de un *recurso que tiene un identificador de recurso especificado*, use:

```azurecli-interactive
az resource show --id <resource-id> --query tags
```

Para obtener grupos de recursos que tengan una etiqueta específica, use `az group list`:

```azurecli-interactive
az group list --tag Dept=IT
```

Para obtener todos los recursos que tengan una etiqueta y un valor particulares, use `az resource list`:

```azurecli-interactive
az resource list --tag Dept=Finance
```

Al agregar etiquetas a un grupo de recursos o recurso, puede sobrescribir las etiquetas existentes o agregar nuevas etiquetas a las etiquetas existentes.

Para sobrescribir las etiquetas existentes en un grupo de recursos, use:

```azurecli-interactive
az group update -n examplegroup --tags 'Environment=Test' 'Dept=IT'
```

Para anexar una etiqueta a las etiquetas existentes en un grupo de recursos, use:

```azurecli-interactive
az group update -n examplegroup --set tags.'Status'='Approved'
```

Para sobrescribir las etiquetas en un recurso, use:

```azurecli-interactive
az resource tag --tags 'Dept=IT' 'Environment=Test' -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Para anexar una etiqueta a las etiquetas existentes en un recurso, use:

```azurecli-interactive
az resource update --set tags.'Status'='Approved' -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Para aplicar todas las etiquetas de un grupo de recursos a sus recursos y *no conservar ninguna de las etiquetas existentes en los recursos*, use el siguiente script:

```azurecli-interactive
jsontags=$(az group show --name examplegroup --query tags -o json)
tags=$(echo $jsontags | tr -d '"{},' | sed 's/: /=/g')
resourceids=$(az resource list -g examplegroup --query [].id --output tsv)
for id in $resourceids
do
  az resource tag --tags $tags --id $id
done
```

Para aplicar todas las etiquetas de un grupo de recursos a sus recursos y *conservar ninguna de las etiquetas existentes en los recursos*, use el siguiente script:

```azurecli-interactive
jsontags=$(az group show --name examplegroup --query tags -o json)
tags=$(echo $jsontags | tr -d '"{},' | sed 's/: /=/g')

resourceids=$(az resource list -g examplegroup --query [].id --output tsv)
for id in $resourceids
do
  resourcejsontags=$(az resource show --id $id --query tags -o json)
  resourcetags=$(echo $resourcejsontags | tr -d '"{},' | sed 's/: /=/g')
  az resource tag --tags $tags$resourcetags --id $id
done
```

Si los nombres o valores de etiqueta incluyen espacios, debe realizar un par de pasos adicionales. En el ejemplo siguiente se aplican todas las etiquetas de un grupo de recursos a sus recursos cuando las etiquetas pueden contener espacios.

```azurecli-interactive
jsontags=$(az group show --name examplegroup --query tags -o json)
tags=$(echo $jsontags | tr -d '{}"' | sed 's/: /=/g' | sed "s/\"/'/g" | sed 's/, /,/g' | sed 's/ *$//g' | sed 's/^ *//g')
origIFS=$IFS
IFS=','
read -a tagarr <<< "$tags"
resourceids=$(az resource list -g examplegroup --query [].id --output tsv)
for id in $resourceids
do
  az resource tag --tags "${tagarr[@]}" --id $id
done
IFS=$origIFS
```

## <a name="templates"></a>Plantillas

Para etiquetar un recurso durante la implementación, agregue el elemento `tags` al recurso que se va a implementar. Proporcione el nombre y el valor de la etiqueta.

### <a name="apply-a-literal-value-to-the-tag-name"></a>Aplicación de un valor literal al nombre de etiqueta

En el ejemplo siguiente se muestra una cuenta de almacenamiento con dos etiquetas (`Dept` y `Environment`) que se establecen en valores literales:

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[parameters('location')]",
            "tags": {
                "Dept": "Finance",
                "Environment": "Production"
            },
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ]
}
```

Para establecer una etiqueta en un valor de datetime, use la [función utcNow](../templates/template-functions-string.md#utcnow).

### <a name="apply-an-object-to-the-tag-element"></a>Aplicación de un objeto al elemento de etiqueta

Puede definir un parámetro de objeto que almacene varias etiquetas y aplicar ese objeto al elemento de etiqueta. Cada propiedad del objeto se convierte en una etiqueta independiente para el recurso. El siguiente ejemplo tiene un parámetro denominado `tagValues` que se aplica al elemento de etiqueta.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        },
        "tagValues": {
            "type": "object",
            "defaultValue": {
                "Dept": "Finance",
                "Environment": "Production"
            }
        }
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[parameters('location')]",
            "tags": "[parameters('tagValues')]",
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ]
}
```

### <a name="apply-a-json-string-to-the-tag-name"></a>Aplicación de una cadena JSON al nombre de etiqueta

Para almacenar muchos valores en una única etiqueta, aplique una cadena JSON que represente los valores. Toda la cadena JSON se almacena como una etiqueta que no puede superar los 256 caracteres. En el ejemplo siguiente se muestra una etiqueta denominada `CostCenter` que contiene varios valores de una cadena JSON:  

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[parameters('location')]",
            "tags": {
                "CostCenter": "{\"Dept\":\"Finance\",\"Environment\":\"Production\"}"
            },
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ]
}
```

### <a name="apply-tags-from-resource-group"></a>Aplicación de etiquetas del grupo de recursos

Para aplicar etiquetas desde un grupo de recursos a un recurso, use la función [resourceGroup](../templates/template-functions-resource.md#resourcegroup). Cuando obtenga el valor de la etiqueta, use la sintaxis `tags[tag-name]` en lugar de la sintaxis `tags.tag-name`, porque algunos caracteres no se analizan correctamente en la notación de puntos.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "location": {
            "type": "string",
            "defaultValue": "[resourceGroup().location]"
        }
    },
    "resources": [
        {
            "apiVersion": "2019-04-01",
            "type": "Microsoft.Storage/storageAccounts",
            "name": "[concat('storage', uniqueString(resourceGroup().id))]",
            "location": "[parameters('location')]",
            "tags": {
                "Dept": "[resourceGroup().tags['Dept']]",
                "Environment": "[resourceGroup().tags['Environment']]"
            },
            "sku": {
                "name": "Standard_LRS"
            },
            "kind": "Storage",
            "properties": {}
        }
    ]
}
```

## <a name="portal"></a>Portal

[!INCLUDE [resource-manager-tag-resource](../../../includes/resource-manager-tag-resources.md)]

## <a name="rest-api"></a>API DE REST

Tanto Azure Portal como PowerShell usan la [API de REST de Resource Manager](/rest/api/resources/) en segundo plano. Si necesita integrar el etiquetado en otro entorno, puede obtener etiquetas con un comando **GET** en el identificador de recurso y actualizar el conjunto de etiquetas con una llamada **PATCH**.

## <a name="tags-and-billing"></a>Etiquetas y facturación

Puede usar etiquetas a fin de agrupar los datos de facturación. Por ejemplo, si va a ejecutar varias máquinas virtuales para distintas organizaciones, use las etiquetas para agrupar el uso por centro de costo. También puede usar etiquetas para clasificar los costos por entorno de tiempo de ejecución; por ejemplo, el uso de facturación en máquinas virtuales que se ejecutan en el entorno de producción.

Puede recuperar información sobre las etiquetas a través de las [API de RateCard y de uso de recursos de Azure](../../billing/billing-usage-rate-card-overview.md) o mediante el archivo de valores separados por coma (CSV). Puede descargar el archivo de uso del [Centro de cuentas de Azure](https://account.azure.com/Subscriptions) o de Azure Portal. Para más información, consulte [Procedimiento para descargar las datos de uso diario y de factura de Azure](../../billing/billing-download-azure-invoice-daily-usage-date.md). Al descargar el archivo de uso del Centro de cuentas de Azure, seleccione **Versión 2**. En los servicios que admiten etiquetas con facturación, las etiquetas aparecen en la columna **Etiquetas**.

Para las operaciones de API de REST, vea [Referencia de API de REST de facturación de Azure](/rest/api/billing/).

## <a name="next-steps"></a>Pasos siguientes

* No todos los tipos de recursos admiten etiquetas. Para determinar si se puede aplicar una etiqueta a un tipo de recurso determinado, consulte [Tag support for Azure resources](tag-support.md) (Compatibilidad con etiquetas para los recursos de Azure).
* Para obtener información sobre cómo usar el portal, consulte [Uso de Azure Portal para administrar los recursos de Azure](manage-resource-groups-portal.md).  
