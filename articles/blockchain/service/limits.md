---
title: Límites de Azure Blockchain
description: Información general del servicio y los límites funcionales en el servicio de Azure Blockchain
services: azure-blockchain
keywords: cadena de bloques
author: PatAltimore
ms.author: patricka
ms.date: 05/02/2019
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: janders
manager: femila
ms.openlocfilehash: 169ec7a8ef407af3f754046aa8e3b06793a7e962
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/02/2019
ms.locfileid: "65028176"
---
# <a name="limits-in-azure-blockchain-service"></a>Límites de servicio de Azure Blockchain

Azure Blockchain Service tiene el servicio y los límites funcionales, como el número de nodos de que un miembro puede tener restricciones consortium y cantidades de almacenamiento.

## <a name="pricing-tier"></a>Plan de tarifa

Los límites máximos de las transacciones y los nodos de control de validación dependen de aprovisionamiento de Azure Blockchain Service los niveles básico o estándar, los planes de tarifa.

| Plan de tarifa | Máximo de nodos de transacción | Máximo de nodos de control de validación |
|:---|:---:|:---:|
| Básica | 10 | 1 |
| Estándar | 10 | 2 |

Después de que no se admite la creación de miembros, cambiar el plan de tarifa de básico y estándar.

## <a name="storage-capacity"></a>Capacidad de almacenamiento

La cantidad máxima de almacenamiento que se puede utilizar para los registros y datos de libro de contabilidad por nodo es de 1 terabyte.

No se admite la reducción del tamaño de almacenamiento de libro de contabilidad y de registro.

## <a name="consortium-limits"></a>Límites del Consorcio

* **Nombres de miembro y Consortium deben ser únicos** desde otros nombres de miembro y consortium en el servicio de la cadena de bloques de Azure.

* **No se puede cambiar los nombres de miembro y consortium**

* **Todos los miembros de un consorcio deben estar en el mismo plan de tarifa**

* **Todos los miembros que participan en un consorcio deben residir en la misma región**

    El primer miembro creado en un consorcio dicta la región. Miembros invitados en el consorcio deben residir en la misma región que el primer miembro. Limitación de todos los miembros a la misma región ayuda a garantizar un consenso de red no se ve afectado.

* **Un consorcio debe tener al menos un administrador**

    Si hay solo un administrador en un consorcio, que no pueden quitarse del consortium o eliminar a su miembro hasta que otro administrador se agrega o se promocionan en el consorcio.

* **Los miembros quitados del consortium no pueden agregarse de nuevo**

    En su lugar, debe ser invitados a participar en el consorcio y crear a un nuevo miembro. No se eliminan sus recursos de miembro existente para conservar las transacciones históricas.

* **Todos los miembros de un consorcio deben usar la misma versión de libro de contabilidad**

    Para obtener más información sobre la aplicación de revisiones, actualizaciones y versiones de libro de contabilidad disponibles en Azure Blockchain Service, consulte [aplicación de revisiones, actualizaciones y las versiones](ledger-versions.md).

## <a name="next-steps"></a>Pasos siguientes

* [Aplicación de revisiones, actualizaciones y versiones](ledger-versions.md)
