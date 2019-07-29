---
title: 'Ejemplo de script CLI de Service Fabric: enumerar las aplicaciones de un clúster'
description: 'Ejemplo de script CLI de Service Fabric: enumerar las aplicaciones aprovisionadas en un clúster de Service Fabric.'
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: ''
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.devlang: na
ms.topic: sample
ms.date: 04/13/2018
ms.author: atsenthi
ms.custom: ''
ms.openlocfilehash: ba88be001702abc09cbdf6a08a61d41953f69f73
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/29/2019
ms.locfileid: "68600096"
---
# <a name="list-applications-running-in-a-service-fabric-cluster"></a>Mostrar las aplicaciones que se ejecutan en un clúster de Service Fabric

Este script de ejemplo se conecta a un clúster de Service Fabric y, a continuación, enumera todas las aplicaciones aprovisionadas.

[!INCLUDE [links to azure cli and service fabric cli](../../../includes/service-fabric-sfctl.md)]

## <a name="sample-script"></a>Script de ejemplo

[!code-sh[main](../../../cli_scripts/service-fabric/list-application/list-application.sh "List provisioned applications from a cluster")]

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte la [documentación de la CLI de Service Fabric](../service-fabric-cli.md).

Puede encontrar ejemplos adicionales de la CLI de Service Fabric para Azure Service Fabric en los [ejemplos de la CLI de Service Fabric](../samples-cli.md).
