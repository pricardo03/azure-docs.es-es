---
title: 'Ejemplo de script de la CLI de Azure: Uso con pares clave-valor en un almacén de Azure App Configuration | Microsoft Docs'
description: Proporciona información sobre cómo trabajar con pares clave-valor en un almacén de Azure App Configuration
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: azure-app-configuration
ms.date: 02/24/2019
ms.author: yegu
ms.custom: mvc
ms.openlocfilehash: e69e2ca5ccd8e8edc2f55d74a0cca03eaabc9f49
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56884250"
---
# <a name="work-with-key-values-in-an-azure-app-configuration-store"></a>Uso de pares clave-valor en un almacén de Azure App Configuration

Este ejemplo de script crea un nuevo par clave-valor en un almacén de Azure App Configuration, enumera todos los pares clave-valor existentes, actualiza el valor de la clave recién creada y, por último, la elimina.

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli).

Para instalar la extensión de la CLI de Azure App Configuration primero debe ejecutar el comando siguiente:

        az extension add -n appconfig

## <a name="sample-script"></a>Script de ejemplo

```azurecli-interactive
#!/bin/bash

appConfigName=myTestAppConfigStore
newKey="TestKey"

# Create a new key-value 
az appconfig kv set --name $appConfigName --key $newKey --value "Value 1"

# List current key-values
az appconfig kv list --name $appConfigName

# Update new key's value
az appconfig kv set --name $appConfigName --value "Value 2"

# List current key-values
az appconfig kv list --name $appConfigName

# Delete new key
az appconfig kv delete  --name $appConfigName --key $newKey

# List current key-values
az appconfig kv list --name $appConfigName
```

[!INCLUDE [cli-script-cleanup](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos para operar en pares clave-valor de un almacén de App Configuration. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az appconfig kv set](/cli/azure/ext/appconfig/appconfig#ext-appconfig-az-appconfig-kv-set) | Crea o actualiza un par clave-valor. |
| [az appconfig kv list](/cli/azure/ext/appconfig/appconfig#ext-appconfig-az-appconfig-kv-list) | Enumera los pares clave-valor en un almacén de App Configuration. |
| [az appconfig kv delete](/cli/azure/ext/appconfig/appconfig#ext-appconfig-az-appconfig-kv-delete) | Elimina un par clave-valor. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](/cli/azure).

Puede encontrar ejemplos de script adicionales de la CLI de App Configuration en la [documentación de Azure App Configuration](../cli-samples.md).
