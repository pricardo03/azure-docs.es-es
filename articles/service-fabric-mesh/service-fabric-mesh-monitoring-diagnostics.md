---
title: Supervisión y diagnóstico en aplicaciones de Azure Service Fabric mesh | Microsoft Docs
description: Obtenga información sobre la supervisión y el diagnóstico de aplicaciones en Service Fabric mesh en Azure.
services: service-fabric-mesh
documentationcenter: .net
author: srrengar
manager: timlt
editor: ''
ms.assetid: ''
ms.service: service-fabric-mesh
ms.devlang: dotNet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/22/2018
ms.author: srrengar
ms.custom: mvc, devcenter
ms.openlocfilehash: 79e5622dd73e53854204675b435e99d187a3ab26
ms.sourcegitcommit: 0b05bdeb22a06c91823bd1933ac65b2e0c2d6553
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/17/2018
ms.locfileid: "39075883"
---
# <a name="monitoring-and-diagnostics"></a>Supervisión y diagnóstico
Azure Service Fabric mesh es un servicio totalmente administrado que permite a los desarrolladores implementar aplicaciones de microservicios sin administración de máquinas virtuales, almacenamiento o red. La supervisión y el diagnóstico para Service Fabric mesh se clasifican en tres tipos principales de datos de diagnóstico:

- Registros de aplicación: se definen como registros de aplicaciones en contenedores, según cómo haya instrumentado la aplicación (por ejemplo, registros de Docker)
- Eventos de plataforma: eventos de la plataforma mesh pertinentes para la operación de contenedor. Actualmente, se incluyen la activación, desactivación y terminación del contenedor.
- Métricas de contenedor: métricas de rendimiento y uso de recursos para los contenedores (estadísticas de Docker)

En este artículo se describen las opciones de supervisión y diagnóstico para la última versión preliminar disponible.

## <a name="application-logs"></a>Registros de aplicación

Puede ver los registros de Docker de los contenedores implementados, por contenedor. En el modelo de aplicación de Service Fabric mesh, cada contenedor es un paquete de código en la aplicación. Para ver los registros asociados con un paquete de código, use el siguiente comando:

```cli
az mesh code-package-log get --resource-group <nameOfRG> --app-name <nameOfApp> --service-name <nameOfService> --replica-name <nameOfReplica> --code-package-name <nameOfCodePackage>
```

> [!NOTE]
> Puede usar el comando "az mesh servicereplica" para obtener el nombre de la réplica. Los nombres de réplica son números incrementales a partir de 0.*

Este es el aspecto de los registros del contenedor VotingWeb.Code desde la aplicación de votación:

```cli
az mesh code-package-log get --resource-group <nameOfRG> --application-name SbzVoting --service-name VotingWeb --replica-name 0 --code-package-name VotingWeb.Code
```

## <a name="next-steps"></a>Pasos siguientes
Para obtener más información sobre Service Fabric mesh, lea la introducción:
- [Introducción a Service Fabric mesh](service-fabric-mesh-overview.md)
