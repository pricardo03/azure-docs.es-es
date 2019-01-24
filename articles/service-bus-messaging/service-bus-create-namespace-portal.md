---
title: Creación de un espacio de nombres de Service Bus en Azure Portal | Microsoft Docs
description: Creación de un espacio de nombres de Service Bus mediante Azure Portal.
services: service-bus-messaging
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: fbb10e62-b133-4851-9d27-40bd844db3ba
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: get-started-article
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: c456fa392c3ba07ad26e300eff07a89fa849b354
ms.sourcegitcommit: 8115c7fa126ce9bf3e16415f275680f4486192c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2019
ms.locfileid: "54856322"
---
# <a name="create-a-service-bus-namespace-using-the-azure-portal"></a>Creación de un espacio de nombres de Service Bus mediante Azure Portal

Un espacio de nombres es un contenedor con un ámbito para todos los componentes de la mensajería. Varias colas y temas pueden residir en un único espacio de nombres, y los espacios de nombres suelen servir de contenedores de aplicación. Existen dos formas de crear espacios de nombres de Service Bus:

1. Azure Portal (este artículo)
2. [Plantillas de Resource Manager][create-namespace-using-arm]

## <a name="create-a-namespace-in-the-azure-portal"></a>Creación de un espacio de nombres en Azure Portal

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Felicidades Ha creado un espacio de nombres de mensajería de Service Bus.

## <a name="next-steps"></a>Pasos siguientes

Consulte los [ejemplos de GitHub][github-samples] para Service Bus, que muestran algunas de las características más avanzadas de la mensajería de Service Bus.

[create-namespace-using-arm]: service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure/azure-service-bus/tree/master/samples
