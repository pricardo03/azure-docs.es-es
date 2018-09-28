---
title: Consultas avanzadas de Azure Resource Graph
description: Utilice Azure Resource Graph para ejecutar algunas consultas avanzadas.
services: resource-graph
author: DCtheGeek
ms.author: dacoulte
ms.date: 09/18/2018
ms.topic: quickstart
ms.service: resource-graph
manager: carmonm
ms.custom: mvc
ms.openlocfilehash: 6798ef3f81ff8e733886fccc56f6474afc2ee5e9
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46992497"
---
# <a name="advanced-resource-graph-queries"></a>Consultas avanzadas de Resource Graph

El primer paso para entender las consultas con Azure Resource Graph es el reconocimiento básico del [lenguaje de consultas](../concepts/query-language.md). Si aún no está familiarizado con el lenguaje de consulta Kusto (KQL), se recomienda repasar los conceptos básicos para entender cómo componer solicitudes para los recursos que está buscando.

Le guiaremos por las siguientes consultas avanzadas:

> [!div class="checklist"]
> - [Búsqueda de conjuntos de escalado de máquinas virtuales que no tengan cifrado de disco o que el cifrado no esté habilitado](#vmss-not-encrypted)
> - [Enumeración de todos los nombres de etiquetas](#list-all-tags)

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free) antes de empezar.

## <a name="language-support"></a>Compatibilidad con lenguajes

La CLI de Azure (mediante una extensión) y Azure PowerShell (mediante un módulo) admiten Azure Resource Graph. Antes de realizar cualquiera de las siguientes consultas, compruebe que el entorno está listo. Consulte la [CLI de Azure](../first-query-azurecli.md#add-the-resource-graph-extension) y [Azure PowerShell](../first-query-powershell.md#add-the-resource-graph-module) para conocer los pasos para instalar y validar el entorno de shell que prefiera.

## <a name="vmss-not-encrypted"></a>Búsqueda de conjuntos de escalado de máquinas virtuales que no tengan cifrado de disco o que el cifrado no esté habilitado

Esta consulta busca recursos de conjuntos de escalado de máquinas virtuales, expande los detalles de las extensiones, examina las propiedades de las extensiones **Type** y **EncryptionOperation**, resume y devuelve los recursos donde el número de configuraciones esperadas era 0.

```Query
where type =~ 'microsoft.compute/virtualmachinescalesets'
| project id, extension = properties.virtualMachineProfile.extensionProfile.extensions
| where extension !has 'AzureDiskEncryption' or extension !has '"EnableEncryption"'
| project id
```

```azurecli-interactive
az graph query -q "where type =~ 'microsoft.compute/virtualmachinescalesets' | project id, extension = properties.virtualMachineProfile.extensionProfile.extensions | where extension !has 'AzureDiskEncryption' or extension !has '"EnableEncryption"' | project id"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "where type =~ 'microsoft.compute/virtualmachinescalesets' | project id, extension = properties.virtualMachineProfile.extensionProfile.extensions | where extension !has 'AzureDiskEncryption' or extension !has '"EnableEncryption"' | project id"
```

## <a name="list-all-tags"></a>Enumeración de todos los nombres de etiquetas

Esta consulta comienza con la etiqueta y crea un objeto JSON que enumera todos los nombres de etiqueta únicos y sus tipos correspondientes.

```Query
project tags
| summarize buildschema(tags)
```

```azurecli-interactive
az graph query -q "project tags | summarize buildschema(tags)"
```

```azurepowershell-interactive
Search-AzureRmGraph -Query "project tags | summarize buildschema(tags)"
```

## <a name="next-steps"></a>Pasos siguientes

- Consulte ejemplos de [consultas de inicio](starter.md)
- Más información sobre el [lenguaje de consulta](../concepts/query-language.md)
- Más información sobre la [exploración de recursos](../concepts/explore-resources.md)