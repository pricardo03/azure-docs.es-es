---
title: 'Solución de Azure VMware de CloudSimple: introducción a los nodos'
description: Obtenga información sobre los conceptos y los nodos CloudSimple.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 729b832c068dcd401fa6e9d2f4af9193d3859a83
ms.sourcegitcommit: 15e3bfbde9d0d7ad00b5d186867ec933c60cebe6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/03/2019
ms.locfileid: "71845544"
---
# <a name="cloudsimple-nodes-overview"></a>Introducción a los nodos CloudSimple

Los nodos son los bloques de compilación de una nube privada. Un nodo:

* Es un host de proceso completo dedicado en el que se instala el hipervisor ESXi de VMware.  
* Es una unidad de computación que puede comprar o reservar para crear nubes privadas.
* Está disponible para comprar o reservar en una región donde está disponible el servicio CloudSimple.

Puede crear una nube privada a partir de los nodos comprados. Para crear una nube privada, necesita un mínimo de tres nodos de la misma SKU. Para expandir una nube privada, agregue más nodos.  Puede agregar nodos a un clúster o crear un clúster nuevo; para ello, compre nodos en Azure Portal y asócielos al servicio CloudSimple.  Todos los nodos comprados aparecen en el servicio CloudSimple.  

## <a name="purchased-nodes"></a>Nodos comprados

Los nodos comprados proporcionan capacidad de pago por uso. Al comprar nodos, puede escalar rápidamente su clúster de VMware a petición. Puede agregar nodos según sea necesario o eliminar un nodo comprado para reducir verticalmente el clúster de VMware. Los nodos comprados se facturan mensualmente y se cargan en la suscripción en la que se compraron.

* Si paga la suscripción a Azure con tarjeta de crédito, se factura de inmediato en la tarjeta.
* Si se le factura, los cargos aparecerán en la próxima factura.

## <a name="vmware-solution-by-cloudsimple-nodes-sku"></a>SKU de los nodos de la solución de VMware de CloudSimple

Los siguientes tipos de nodos están disponibles para comprar o reservar.

| SKU           | Nodo CS28                 | Nodo CS36                 | Nodo CS36m                |
|---------------|-----------------------------|-----------------------------|-----------------------------|
| Region        | Este de EE. UU., Oeste de EE. UU.            | Este de EE. UU., Oeste de EE. UU.            | Europa occidental                 |
| CPU           | 2 × 2,2 GHz, 28 núcleos (56 HT) | 2 × 2,3 GHz, 36 núcleos (72 HT) | 2 × 2,3 GHz, 36 núcleos (72 HT) |
| RAM           | 256 GB                      | 512 GB                      | 576 GB                      |
| Disco de caché    | 1,6 TB NVMe                 | 3,2 TB NVMe                 | 3,2 TB NVMe                 |
| Disco de capacidad | 5,625 TB sin formato                | 11,25 TB sin formato                | 15,36 TB sin formato                |
| Tipo de almacenamiento  | All Flash                   | All Flash                   | All Flash                   |

## <a name="limits"></a>límites

Se aplican los siguientes límites de nodo a las nubes privadas.

| Resource | Límite |
|----------|-------|
| Número mínimo de nodos para crear una nube privada | 3 |
| Número máximo de nodos en un clúster en una nube privada | 16 |
| Número máximo de nodos en una nube privada | 64 |
| Número mínimo de nodos en un clúster nuevo | 3 |

## <a name="next-steps"></a>Pasos siguientes

* Obtenga más información sobre cómo [comprar nodos](create-nodes.md).
* Obtenga más información sobre las [nubes privadas](cloudsimple-private-cloud.md).
