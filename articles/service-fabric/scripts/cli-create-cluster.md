---
title: Ejemplo de implementación de Script de la CLI de Azure
description: Cree un clúster Linux seguro de Service Fabric en Azure mediante la CLI de Azure.
services: service-fabric
documentationcenter: ''
author: aljo-microsoft
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 01/18/2018
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: 3a50d6672833ab1b4986e38ca1ddf24a8c33c562
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58664461"
---
# <a name="create-a-secure-service-fabric-linux-cluster-in-azure"></a>Creación de un clúster seguro de Linux de Service Fabric en Azure

Con este comando se crea un certificado autofirmado, se agrega a un almacén de claves y se descarga el certificado al sitio local.  El nuevo certificado se usa para proteger el clúster al implementarlo.  También puede utilizar un certificado existente en lugar de crear uno.  En cualquier caso, el nombre del firmante del certificado debe coincidir con el dominio de acceso al clúster de Service Fabric. Esta coincidencia es necesaria para proporcionar SSL a los puntos de conexión de administración HTTPS y de Service Fabric Explorer del clúster. No puede obtener un certificado SSL de una entidad de certificación (CA) para el dominio `.cloudapp.azure.com`. Debe adquirir un nombre de dominio personalizado para el clúster. Cuando solicite un certificado de una CA, el nombre de sujeto del certificado debe coincidir con el nombre del dominio personalizado del clúster.

Si es necesario, instale la [CLI de Azure](/en-us/cli/azure/install-azure-cli?view=azure-cli-latest).

## <a name="sample-script"></a>Script de ejemplo

[!code-sh[main](../../../cli_scripts/service-fabric/create-cluster/create-cluster.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Limpieza de la implementación

Después de ejecutar el script de ejemplo, se puede usar el comando siguiente para quitar el grupo de recursos, el clúster y todos los recursos relacionados.

```azurecli
ResourceGroupName = "aztestclustergroup"
az group delete --name $ResourceGroupName
```

## <a name="script-explanation"></a>Explicación del script

Este script usa los siguientes comandos. Cada comando de la tabla crea un vínculo a documentación específica del comando.

| Get-Help | Notas |
|---|---|
| [az sf cluster create](https://docs.microsoft.com/cli/azure/sf/cluster?view=azure-cli-latest) | Crea un clúster de Azure Service Fabric.  |

## <a name="next-steps"></a>Pasos siguientes

Puede encontrar ejemplos adicionales de la CLI de Service Fabric para Azure Service Fabric en los [ejemplos de la CLI de Service Fabric](../samples-cli.md).
