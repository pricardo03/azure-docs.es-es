---
title: Ejemplo de eliminación de script de la CLI de Azure Service Fabric (sfctl)
description: Eliminación de una aplicación de un clúster de Azure Service Fabric mediante la CLI de Azure Service Fabric
services: service-fabric
documentationcenter: ''
author: aljo-microsoft
manager: timlt
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 12/06/2017
ms.author: aljo
ms.custom: mvc
ms.openlocfilehash: 47f0636437fb903d43c0aaa439bb41309b56dcc9
ms.sourcegitcommit: 7f7c2fe58c6cd3ba4fd2280e79dfa4f235c55ac8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2019
ms.locfileid: "56804423"
---
# <a name="remove-an-application-from-a-service-fabric-cluster"></a>Eliminación de una aplicación de un clúster de Service Fabric

Este script de ejemplo elimina una instancia de aplicación de Service Fabric en ejecución y anula el registro del tipo y la versión de una aplicación del clúster.  La eliminación de la instancia de la aplicación también elimina todas las instancias de servicio en ejecución asociadas a esa aplicación. A continuación, los archivos de aplicación se eliminan del almacén de imágenes. 

Instale la [CLI de Service Fabric](../service-fabric-cli.md) si es necesario.

## <a name="sample-script"></a>Script de ejemplo

[!code-sh[main](../../../cli_scripts/service-fabric/remove-application/remove-application.sh "Remove an application from a cluster")]

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte la [documentación de la CLI de Service Fabric](../service-fabric-cli.md).

Puede encontrar ejemplos adicionales de la CLI de Service Fabric para Azure Service Fabric en los [ejemplos de la CLI de Service Fabric](../samples-cli.md).
