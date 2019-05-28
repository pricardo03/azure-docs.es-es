---
title: Ejemplos de consultas avanzadas
description: Use Azure Resource Graph para ejecutar consultas avanzadas, como la capacidad de VMSS, la enumeración de todas las etiquetas usadas, y la coincidencia de las máquinas virtuales con expresiones regulares.
author: DCtheGeek
ms.author: dacoulte
ms.date: 01/23/2019
ms.topic: quickstart
ms.service: resource-graph
manager: carmonm
ms.custom: seodec18
ms.openlocfilehash: 7684ae6b4ddb6320efc62ef6f9963bef1b9a66fa
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64691975"
---
# <a name="advanced-resource-graph-queries"></a>Consultas avanzadas de Resource Graph

El primer paso para entender las consultas con Azure Resource Graph es el reconocimiento básico del [lenguaje de consultas](../concepts/query-language.md). Si aún no está familiarizado con [Azure Data Explorer](../../../data-explorer/data-explorer-overview.md), se recomienda repasar los conceptos básicos para entender cómo componer solicitudes para los recursos que está buscando.

Le guiaremos por las siguientes consultas avanzadas:

> [!div class="checklist"]
> - [Obtener la capacidad y el tamaño del conjunto de escalado de máquinas virtuales](#vmss-capacity)
> - [Enumeración de todos los nombres de etiquetas](#list-all-tags)
> - [Máquinas virtuales que coinciden con regex](#vm-regex)

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free) antes de empezar.

[!INCLUDE [az-powershell-update](../../../../includes/updated-for-az.md)]

## <a name="language-support"></a>Compatibilidad con idiomas

La CLI de Azure (mediante una extensión) y Azure PowerShell (mediante un módulo) admiten Azure Resource Graph. Antes de ejecutar cualquiera de las siguientes consultas, compruebe que el entorno está listo. Consulte la [CLI de Azure](../first-query-azurecli.md#add-the-resource-graph-extension) y [Azure PowerShell](../first-query-powershell.md#add-the-resource-graph-module) para conocer los pasos para instalar y validar el entorno de shell que prefiera.

## <a name="vmss-capacity"></a>Obtener la capacidad y el tamaño del conjunto de escalado de máquinas virtuales

Esta consulta busca los recursos del conjunto de escalado de máquinas virtuales y obtiene varios detalles, incluido el tamaño de la máquina virtual y la capacidad del conjunto de escalado. La consulta usa la función `toint()` para convertir la capacidad en un número para que se pueda ordenar. Por último, se cambia el nombre de las columnas por propiedades con nombre personalizadas.

```kusto
where type=~ 'microsoft.compute/virtualmachinescalesets'
| where name contains 'contoso'
| project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name
| order by Capacity desc
```

```azurecli-interactive
az graph query -q "where type=~ 'microsoft.compute/virtualmachinescalesets' | where name contains 'contoso' | project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name | order by Capacity desc"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type=~ 'microsoft.compute/virtualmachinescalesets' | where name contains 'contoso' | project subscriptionId, name, location, resourceGroup, Capacity = toint(sku.capacity), Tier = sku.name | order by Capacity desc"
```

## <a name="list-all-tags"></a>Enumeración de todos los nombres de etiquetas

Esta consulta comienza con la etiqueta y crea un objeto JSON que enumera todos los nombres de etiqueta únicos y sus tipos correspondientes.

```kusto
project tags
| summarize buildschema(tags)
```

```azurecli-interactive
az graph query -q "project tags | summarize buildschema(tags)"
```

```azurepowershell-interactive
Search-AzGraph -Query "project tags | summarize buildschema(tags)"
```

## <a name="vm-regex"></a>Máquinas virtuales que coinciden con regex

Esta consulta busca las máquinas virtuales que coincidan con una [expresión regular](/dotnet/standard/base-types/regular-expression-language-quick-reference) (conocida como _regex_).
**matches regex \@** nos permite definir regex para coincidir, que es `^Contoso(.*)[0-9]+$`. Esa definición de regex se explica como:

- `^` - La coincidencia debe empezar al principio de la cadena.
- `Contoso`: la cadena distingue mayúsculas de minúsculas.
- `(.*)`: una coincidencia de subexpresión:
  - `.` - Coincide con cualquier carácter individual (excepto saltos de línea).
  - `*` - Coincide con el elemento anterior cero o más veces.
- `[0-9]` - Coincidencia de grupo caracteres para los números del 0 al 9.
- `+` - Coincide con el elemento anterior una o más veces.
- `$` - La coincidencia del elemento anterior se debe producir al final de la cadena.

Después de la coincidencia del nombre, la consulta proyecta el nombre y ordena por nombre de forma ascendente.

```kusto
where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$'
| project name
| order by name asc
```

```azurecli-interactive
az graph query -q "where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$' | project name | order by name asc"
```

```azurepowershell-interactive
Search-AzGraph -Query "where type =~ 'microsoft.compute/virtualmachines' and name matches regex @'^Contoso(.*)[0-9]+$' | project name | order by name asc"
```

## <a name="next-steps"></a>Pasos siguientes

- Consulte ejemplos de [consultas de inicio](starter.md)
- Más información sobre el [lenguaje de consulta](../concepts/query-language.md)
- Más información sobre la [exploración de recursos](../concepts/explore-resources.md)
