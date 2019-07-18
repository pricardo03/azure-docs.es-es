---
title: Etiquetado de recursos de Azure para organización lógica | Microsoft Docs
description: Muestra cómo aplicar etiquetas para organizar los recursos de Azure para la facturación y administración.
author: tfitzmac
ms.service: azure-resource-manager
ms.topic: conceptual
ms.date: 07/11/2019
ms.author: tomfitz
ms.openlocfilehash: 77175738a0cae5c6185a8ac74e51e9b91b685235
ms.sourcegitcommit: 441e59b8657a1eb1538c848b9b78c2e9e1b6cfd5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827932"
---
# <a name="use-tags-to-organize-your-azure-resources"></a>Uso de etiquetas para organizar los recursos de Azure

[!INCLUDE [resource-manager-governance-tags](../../includes/resource-manager-governance-tags.md)]

Para aplicar etiquetas a los recursos, el usuario debe tener acceso de escritura en ese tipo de recurso. Para aplicar etiquetas a todos los tipos de recursos, use el rol [Colaborador](../role-based-access-control/built-in-roles.md#contributor). Para aplicar etiquetas a un solo tipo de recurso, use el rol de colaborador para ese recurso. Por ejemplo, para aplicar etiquetas a las máquinas virtuales, use [Colaborador de la máquina virtual](../role-based-access-control/built-in-roles.md#virtual-machine-contributor).

[!INCLUDE [Handle personal data](../../includes/gdpr-intro-sentence.md)]

## <a name="policies"></a>Directivas

Puede usar [Azure Policy](../governance/policy/overview.md) para aplicar reglas de etiquetado y convenciones. Mediante la creación de una directiva, evitará el escenario de recursos que se vayan a implementar en su suscripción que no cumplen con las etiquetas esperadas para su organización. En lugar de aplicar manualmente las etiquetas o buscar recursos que no son compatibles, puede crear una directiva que aplica automáticamente las etiquetas necesarias durante la implementación. La sección siguiente muestra las directivas de ejemplo para las etiquetas.

[!INCLUDE [Tag policies](../../includes/azure-policy-samples-general-tags.md)]

## <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

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

Para ver las etiquetas existentes de un *recurso que tiene un identificador de recurso especificado*, use:

```azurepowershell-interactive
(Get-AzResource -ResourceId /subscriptions/<subscription-id>/resourceGroups/<rg-name>/providers/Microsoft.Storage/storageAccounts/<storage-name>).Tags
```

O bien, para ver las etiquetas existentes para un *recurso que tiene un nombre y un grupo de recursos especificados*, use:

```azurepowershell-interactive
(Get-AzResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

Para obtener *grupos de recursos que tengan una etiqueta específica*, use:

```azurepowershell-interactive
(Get-AzResourceGroup -Tag @{ Dept="Finance" }).ResourceGroupName
```

Para obtener *recursos que tengan una etiqueta específica*, use:

```azurepowershell-interactive
(Get-AzResource -Tag @{ Dept="Finance"}).Name
```

Para obtener *recursos que tengan un nombre de etiqueta específico*, use:

```azurepowershell-interactive
(Get-AzResource -TagName Dept).Name
```

Cada vez que aplique etiquetas a un recurso o grupo de recursos, sobrescribirá las etiquetas existentes en ese recurso o grupo de recursos. Por lo tanto, tiene que utilizar un enfoque diferente en función de si el recurso o grupo de recursos tienen etiquetas existentes.

Para agregar etiquetas a un *grupo de recursos sin etiquetas existentes*, use:

```azurepowershell-interactive
Set-AzResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

Para agregar etiquetas a un *grupo de recursos que ya tiene etiquetas*, recupere las etiquetas existentes, agregue la nueva y vuelva a aplicar todas:

```azurepowershell-interactive
$tags = (Get-AzResourceGroup -Name examplegroup).Tags
$tags.Add("Status", "Approved")
Set-AzResourceGroup -Tag $tags -Name examplegroup
```

Para agregar etiquetas a un *recurso sin etiquetas*, use:

```azurepowershell-interactive
$r = Get-AzResource -ResourceName examplevnet -ResourceGroupName examplegroup
Set-AzResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceId $r.ResourceId -Force
```

Para agregar etiquetas a un *recurso que ya tiene etiquetas*, use:

```azurepowershell-interactive
$r = Get-AzResource -ResourceName examplevnet -ResourceGroupName examplegroup
$r.Tags.Add("Status", "Approved")
Set-AzResource -Tag $r.Tags -ResourceId $r.ResourceId -Force
```

Para aplicar todas las etiquetas de un grupo de recursos a sus recursos y *no conservar ninguna de las etiquetas existentes en los recursos*, use el siguiente script:

```azurepowershell-interactive
$groups = Get-AzResourceGroup
foreach ($g in $groups)
{
    Get-AzResource -ResourceGroupName $g.ResourceGroupName | ForEach-Object {Set-AzResource -ResourceId $_.ResourceId -Tag $g.Tags -Force }
}
```

Para aplicar todas las etiquetas de un grupo de recursos a sus recursos y *conservar las etiquetas existentes en los recursos que no son duplicados*, use el siguiente script:

```azurepowershell-interactive
$group = Get-AzResourceGroup "examplegroup"
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

## <a name="azure-cli"></a>CLI de Azure

Para ver las etiquetas existentes de un *grupo de recursos*, use:

```azurecli
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

```azurecli
az resource show -n examplevnet -g examplegroup --resource-type "Microsoft.Network/virtualNetworks" --query tags
```

Al recorrer en iteración una colección de recursos, puede mostrar el identificador de recurso por recurso. Más adelante en este artículo se muestra un ejemplo completo. Para ver las etiquetas existentes de un *recurso que tiene un identificador de recurso especificado*, use:

```azurecli
az resource show --id <resource-id> --query tags
```

Para obtener grupos de recursos que tengan una etiqueta específica, use `az group list`:

```azurecli
az group list --tag Dept=IT
```

Para obtener todos los recursos que tengan una etiqueta y un valor particulares, use `az resource list`:

```azurecli
az resource list --tag Dept=Finance
```

Cada vez que aplique etiquetas a un recurso o grupo de recursos, sobrescribirá las etiquetas existentes en ese recurso o grupo de recursos. Por lo tanto, tiene que utilizar un enfoque diferente en función de si el recurso o grupo de recursos tienen etiquetas existentes.

Para agregar etiquetas a un *grupo de recursos sin etiquetas existentes*, use:

```azurecli
az group update -n examplegroup --set tags.Environment=Test tags.Dept=IT
```

Para agregar etiquetas a un *recurso sin etiquetas*, use:

```azurecli
az resource tag --tags Dept=IT Environment=Test -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Para agregar etiquetas a un recurso que ya tiene etiquetas, recupere las etiquetas existentes, vuelva a formatear el valor y vuelva a aplicar las etiquetas existentes y nuevas:

```azurecli
jsonrtag=$(az resource show -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks" --query tags)
rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
az resource tag --tags $rt Project=Redesign -g examplegroup -n examplevnet --resource-type "Microsoft.Network/virtualNetworks"
```

Para aplicar todas las etiquetas de un grupo de recursos a sus recursos y *no conservar ninguna de las etiquetas existentes en los recursos*, use el siguiente script:

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups
do
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv)
  for resid in $r
  do
    az resource tag --tags $t --id $resid
  done
done
```

Para aplicar todas las etiquetas de un grupo de recursos a sus recursos y *conservar ninguna de las etiquetas existentes en los recursos*, use el siguiente script:

```azurecli
groups=$(az group list --query [].name --output tsv)
for rg in $groups
do
  jsontag=$(az group show -n $rg --query tags)
  t=$(echo $jsontag | tr -d '"{},' | sed 's/: /=/g')
  r=$(az resource list -g $rg --query [].id --output tsv)
  for resid in $r
  do
    jsonrtag=$(az resource show --id $resid --query tags)
    rt=$(echo $jsonrtag | tr -d '"{},' | sed 's/: /=/g')
    az resource tag --tags $t$rt --id $resid
  done
done
```

## <a name="templates"></a>Plantillas

[!INCLUDE [resource-manager-tags-in-templates](../../includes/resource-manager-tags-in-templates.md)]

## <a name="portal"></a>Portal

[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="rest-api"></a>API DE REST

Tanto Azure Portal como PowerShell usan la [API de REST de Resource Manager](https://docs.microsoft.com/rest/api/resources/) en segundo plano. Si necesita integrar el etiquetado en otro entorno, puede obtener etiquetas con un comando **GET** en el identificador de recurso y actualizar el conjunto de etiquetas con una llamada **PATCH**.

## <a name="tags-and-billing"></a>Etiquetas y facturación

Puede usar etiquetas a fin de agrupar los datos de facturación. Por ejemplo, si va a ejecutar varias máquinas virtuales para distintas organizaciones, use las etiquetas para agrupar el uso por centro de costo. También puede usar etiquetas para clasificar los costos por entorno de tiempo de ejecución; por ejemplo, el uso de facturación en máquinas virtuales que se ejecutan en el entorno de producción.

Puede recuperar información sobre las etiquetas a través de las [API de RateCard y de uso de recursos de Azure](../billing/billing-usage-rate-card-overview.md) o mediante el archivo de valores separados por coma (CSV). Puede descargar el archivo de uso del [Centro de cuentas de Azure](https://account.azure.com/Subscriptions) o de Azure Portal. Para más información, consulte [Procedimiento para descargar las datos de uso diario y de factura de Azure](../billing/billing-download-azure-invoice-daily-usage-date.md). Al descargar el archivo de uso del Centro de cuentas de Azure, seleccione **Versión 2**. En los servicios que admiten etiquetas con facturación, las etiquetas aparecen en la columna **Etiquetas**.

Para las operaciones de API de REST, vea [Referencia de API de REST de facturación de Azure](/rest/api/billing/).

## <a name="next-steps"></a>Pasos siguientes

* No todos los tipos de recursos admiten etiquetas. Para determinar si se puede aplicar una etiqueta a un tipo de recurso determinado, consulte [Tag support for Azure resources](tag-support.md) (Compatibilidad con etiquetas para los recursos de Azure).
* Para obtener información sobre cómo usar el portal, consulte [Uso de Azure Portal para administrar los recursos de Azure](manage-resource-groups-portal.md).  
