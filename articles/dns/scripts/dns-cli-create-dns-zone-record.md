---
title: 'Creación de una zona y un registro DNS para un nombre de dominio: CLI de Azure (Azure DNS)'
description: En este ejemplo de script de CLI de Azure se muestra cómo crear una zona DNS y un registro para un nombre de dominio
services: dns
author: asudbring
ms.service: dns
ms.topic: sample
ms.date: 09/20/2019
ms.author: allensu
ms.openlocfilehash: 8f5dde0a35f31d2c33ab2857659efc88339c2ead
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2019
ms.locfileid: "74210395"
---
# <a name="azure-cli-script-example-create-a-dns-zone-and-record"></a>Ejemplo de script de la CLI de Azure: Creación de una zona y un registro DNS

En este ejemplo de script de CLI de Azure se crea una zona DNS y un registro para un nombre de dominio. 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../cli_scripts/dns/create-dns-zone-and-record/create-dns-zone-and-record.sh "Create DNS zone and record")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación 

Ejecute el siguiente comando para quitar el grupo de recursos, la zona DNS y todos los recursos relacionados.

```azurecli
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos para crear un grupo de recursos, una máquina virtual, un conjunto de disponibilidad, un equilibrador de carga y todos los recursos relacionados. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az group create](/cli/azure/group#az-group-create) | Crea un grupo de recursos en el que se almacenan todos los recursos. |
| [az network dns zone create](/cli/azure/network/dns/zone#az-network-dns-zone-create) | Crea una zona de Azure DNS. |
| [az network dns record-set a add-record](/cli/azure/network/dns/record-set) | Agrega un registro *A* a una zona DNS. |
| [az network dns record-set list](/cli/azure/network/dns/record-set) | Muestra todos los conjuntos de registros *A* en una zona DNS. |
| [az group delete](https://docs.microsoft.com/cli/azure/vm/extension#az-vm-extension-set) | Elimina un grupo de recursos, incluidos todos los recursos anidados. |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la CLI de Azure, consulte la [documentación de la CLI de Azure](https://docs.microsoft.com/cli/azure).

