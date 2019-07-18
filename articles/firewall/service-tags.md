---
title: Información general de las etiquetas del servicio Azure Firewall
description: Este artículo es una introducción a las etiquetas del servicio Azure Firewall.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 6/27/2019
ms.author: victorh
ms.openlocfilehash: d0ac36e415c056dffc9c75d00968ff74c2156e63
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450162"
---
# <a name="azure-firewall-service-tags"></a>Etiquetas de servicio de Azure Firewall

Una etiqueta de servicio representa un grupo de prefijos de direcciones IP que ayudan a reducir la complejidad de la creación de reglas de seguridad. No puede crear su propia etiqueta de servicio, ni especificar qué direcciones IP se incluyen dentro de una etiqueta. Microsoft administra los prefijos de direcciones que incluye la etiqueta de servicio y actualiza automáticamente esta a medida que las direcciones cambian.

Las etiquetas de servicio de Azure Firewall se pueden usar en el campo de destino de las reglas de red. Puede usarlas en lugar de direcciones IP específicas.

## <a name="supported-service-tags"></a>Etiquetas de servicio admitidas

Consulte los [grupos de seguridad](../virtual-network/security-overview.md#service-tags) para obtener una lista de etiquetas de servicio que están disponibles para su uso en las reglas de red del firewall de Azure.

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de las reglas de Azure Firewall, consulte [Lógica de procesamiento de reglas de Azure Firewall](rule-processing.md).