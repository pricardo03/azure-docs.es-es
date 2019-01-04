---
author: wesmc7777
ms.service: redis-cache
ms.topic: include
ms.date: 11/21/2018
ms.author: wesmc
ms.openlocfilehash: dd9700c9472e07daf294eca12b766e3dc4832955
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/08/2018
ms.locfileid: "53111648"
---
### <a name="cacheskuname"></a>cacheSKUName
El plan de tarifa de la nueva instancia de Azure Cache for Redis.

    "cacheSKUName": {
      "type": "string",
      "allowedValues": [
        "Basic",
        "Standard"
      ],
      "defaultValue": "Basic",
      "metadata": {
        "description": "The pricing tier of the new Azure Cache for Redis."
      }
    },

La plantilla define los valores permitidos para este parámetro ( Básico o Estándar) y asigna un valor predeterminado (Básico) si no se especifica ningún valor. Básico ofrece un único nodo con varios tamaños disponibles hasta 53 GB.
Estándar proporciona dos nodos, principal/réplica, con varios tamaños disponibles de hasta 53 GB y un contrato de nivel de servicio del 99,9%.

### <a name="cacheskufamily"></a>cacheSKUFamily
La familia de la SKU.

    "cacheSKUFamily": {
      "type": "string",
      "allowedValues": [
        "C"
      ],
      "defaultValue": "C",
      "metadata": {
        "description": "The family for the sku."
      }
    },


### <a name="cacheskucapacity"></a>cacheSKUCapacity
El tamaño de la nueva instancia de Azure Cache for Redis. 

    "cacheSKUCapacity": {
      "type": "int",
      "allowedValues": [
        0,
        1,
        2,
        3,
        4,
        5,
        6
      ],
      "defaultValue": 0,
      "metadata": {
        "description": "The size of the new Azure Cache for Redis instance. "
      }
    }


La plantilla define los valores permitidos para este parámetro (0, 1, 2, 3, 4, 5 o 6) y asigna un valor predeterminado (0) si no se especifica ningún valor. Los números corresponden a los siguientes tamaños de caché: 0 = 250 MB, 1 = 1 GB, 2 = 2.5 GB, 3 = 6 GB, 4 = 13 GB, 5 = 26 GB, 6 = 53 GB

