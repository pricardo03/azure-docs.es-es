---
title: Información general de los nodos para la solución VMware CloudSimple - Azure
description: Obtenga información sobre los conceptos y los nodos CloudSimple.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: b3c8fca1dd93f379860cc3b084fbb14d4a0c6380
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2019
ms.locfileid: "64577366"
---
# <a name="cloudsimple-nodes-overview"></a>Información general de los nodos de CloudSimple

Un nodo es:

* Una reconstrucción completa dedicado de proceso host donde está instalado el hipervisor de VMware ESXi  
* Una unidad de la informática puede comprar o reservar para crear nubes privadas  
* Disponible para comprar o reservar en una región donde está disponible el servicio CloudSimple

Los nodos son bloques de creación de una nube privada.  Para crear una nube privada, se necesita un mínimo de tres nodos de la misma SKU.  Para expandir una nube privada, agregar nodos adicionales.  Puede agregar nodos a un clúster existente. O bien, puede crear un nuevo clúster mediante la compra de los nodos en el portal de Azure y su asociación con el servicio CloudSimple.  Todos los nodos adquiridos están visibles en el servicio CloudSimple.  Crear una nube privada desde los nodos en el CloudSimple Portal adquiridos.

## <a name="purchased-nodes"></a>Nodos adquiridos

Nodos adquiridos proporcionan capacidad de pago por uso. Comprar nodos le ayuda a escalar rápidamente su clúster de VMware a petición. Puede agregar nodos según sea necesario, o eliminar un nodo adquirido para escalar verticalmente el clúster de VMware. Nodos adquiridos se factura mensualmente y carga a la suscripción en la que se compran:

* Si paga por su suscripción de Azure con tarjeta de crédito, la tarjeta se factura inmediatamente.
* Si se le factura por factura, los cargos aparecen en la próxima factura.

## <a name="vmware-solution-by-cloudsimple-nodes-sku"></a>Solución VMware CloudSimple nodos SKU

Los tipos de nodos siguientes están disponibles para su compra o de reserva.

| SKU | CS28 - nodo | CS36 - nodo |
|-----|-------------|-------------|
| CPU | 2x2.2 GHz, 28 núcleos (56 HT) | 2x2.3 GHz, 36 núcleos (72 HT) |
| RAM | 256 GB | 512 GB |
| Caché de disco |  NVMe 1,6 TB | 3.2 TB NVMe |
| Disco de capacidad | 5.625 TB sin procesar | 11,25 TB sin procesar |
| Tipo de almacenamiento | All Flash | All Flash |

## <a name="limits"></a>límites

Los límites de nodo siguientes se aplican a las nubes privadas.

| Recurso | Límite |
|----------|-------|
| Número mínimo de nodos que se va a crear una nube privada | 3 |
| Número máximo de nodos en un clúster en una nube privada | 16 |
| Número máximo de nodos en una nube privada | 64 |
| Número mínimo de nodos en un nuevo clúster | 3 |

## <a name="next-steps"></a>Pasos siguientes

* Obtenga información sobre cómo [adquiere nodos](create-nodes.md)
* Obtenga información sobre [nube privada](cloudsimple-private-cloud.md)