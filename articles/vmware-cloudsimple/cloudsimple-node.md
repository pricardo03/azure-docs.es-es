---
title: 'Azure VMware Solutions (AVS): información general sobre nodos'
description: Obtenga información sobre los conceptos y los nodos de AVS.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 08d8fc3e6f1f2f83cf3c4fee3fdafb0bd07e336c
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77024932"
---
# <a name="avs-nodes-overview"></a>Información general sobre los nodos de AVS

Los nodos son los bloques de creación de una nube privada de AVS. Un nodo:

* Es un host de proceso completo dedicado en el que se instala el hipervisor ESXi de VMware.  
* Es una unidad de computación que puede comprar o reservar para crear nubes privadas de AVS.
* Está disponible para comprar o reservar en una región donde está disponible el servicio de AVS.

Puede crear una nube privada de AVS a partir de los nodos comprados. Para crear una nube privada de AVS, necesita un mínimo de tres nodos de la misma SKU. Para expandir una nube privada de AVS, agregue más nodos. Puede agregar nodos a un clúster o crear un clúster nuevo; para ello, compre nodos en Azure Portal y asócielos al servicio de AVS. Todos los nodos comprados aparecen en el servicio de AVS. 

## <a name="provisioned-nodes"></a>Nodos aprovisionados

Los nodos aprovisionados proporcionan capacidad de pago por uso. Al aprovisionar nodos, puede escalar rápidamente su clúster de VMware a petición. Puede agregar nodos según sea necesario o eliminar un nodo aprovisionado para reducir verticalmente el clúster de VMware. Los nodos aprovisionados se facturan mensualmente y se cargan a la suscripción en la que están aprovisionados.

* Si paga la suscripción a Azure con tarjeta de crédito, se factura de inmediato en la tarjeta.
* Si se le factura, los cargos aparecerán en la próxima factura.

## <a name="vmware-solution-by-avs-nodes-sku"></a>SKU de nodos de Azure VMware Solution by AVS

Los siguientes tipos de nodos están disponibles para aprovisionar o reservar.

| SKU           | Nodo CS28                 | Nodo CS36                 | Nodo CS36m                |
|---------------|-----------------------------|-----------------------------|-----------------------------|
| Region        | Este de EE. UU., Oeste de EE. UU.            | Este de EE. UU., Oeste de EE. UU.            | Oeste de Europa                 |
| CPU           | 2 × 2,2 GHz, 28 núcleos (56 HT) | 2 × 2,3 GHz, 36 núcleos (72 HT) | 2 × 2,3 GHz, 36 núcleos (72 HT) |
| RAM           | 256 GB                      | 512 GB                      | 576 GB                      |
| Disco de caché    | 1,6 TB NVMe                 | 3,2 TB NVMe                 | 3,2 TB NVMe                 |
| Disco de capacidad | 5,625 TB sin formato                | 11,25 TB sin formato                | 15,36 TB sin formato                |
| Tipo de almacenamiento  | All Flash                   | All Flash                   | All Flash                   |

## <a name="limits"></a>límites

Se aplican los siguientes límites de nodos a las nubes privadas de AVS.

| Resource | Límite |
|----------|-------|
| Número mínimo de nodos para crear una nube privada de AVS | 3 |
| Número máximo de nodos en un clúster en una nube privada de AVS | 16 |
| Número máximo de nodos en una nube privada de AVS | 64 |
| Número mínimo de nodos en un clúster nuevo | 3 |

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre cómo [comprar nodos](create-nodes.md).
* Obtenga información sobre las [nubes privadas de AVS](cloudsimple-private-cloud.md).
* Obtenga información sobre cómo [aprovisionar nodos](create-nodes.md).
* Obtenga más información sobre las [nubes privadas](cloudsimple-private-cloud.md).
