---
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 10/26/2018
ms.author: cynthn
ms.openlocfilehash: 8ae22ec7f75b9cd0d7958977dfd97169706c9389
ms.sourcegitcommit: 6e09760197a91be564ad60ffd3d6f48a241e083b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/29/2018
ms.locfileid: "50227371"
---
Después de crear el agente de escucha del grupo de disponibilidad, puede que sea necesario ajustar los parámetros de clúster RegisterAllProvidersIP y HostRecordTTL para el recurso del agente de escucha. Estos parámetros pueden reducir el tiempo de reconexión tras una conmutación por error que puede evitar los tiempos de expiración de la conexión. Para más información sobre estos parámetros, así como código de ejemplo, consulte [Crear o configurar un agente de escucha del grupo de disponibilidad](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover).

