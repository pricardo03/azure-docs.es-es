---
title: Determinación del tamaño de la red virtual y la subred para Instancia administrada de Azure SQL Database | Microsoft Docs
description: En este tema se describe cómo calcular el tamaño de la subred en la que se implementarán Instancias administradas de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, bonova, carlrab
manager: craigg
ms.date: 02/22/2019
ms.openlocfilehash: 2a10876bc3c9558de29caf9fee2ae0b06ee87f28
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/09/2019
ms.locfileid: "59358924"
---
# <a name="determine-vnet-subnet-size-for-azure-sql-database-managed-instance"></a>Determinación del tamaño de la red virtual y la subred para Instancia administrada de Azure SQL Database

La Instancia administrada de Azure SQL Database debe implementarse dentro de una [red virtual](../virtual-network/virtual-networks-overview.md) de Azure.

El número de Instancias administradas que se puede implementar en la subred de la red virtual depende del tamaño de la subred (intervalo de subred).

Cuando se crea una instancia administrada, Azure asigna un número de máquinas virtuales según el nivel que seleccione durante el aprovisionamiento. Debido a que estas máquinas virtuales están asociadas a la subred, requieren direcciones IP. Para garantizar la alta disponibilidad durante las operaciones normales y el mantenimiento del servicio, Azure puede asignar máquinas virtuales adicionales. Como resultado, el número de direcciones IP necesarias en una subred es mayor que el número de instancias administradas en esa subred.

Por definición, una instancia administrada necesita un mínimo de 16 direcciones IP en una subred y puede utilizar hasta 256 direcciones IP. Como resultado, puede usar un máscaras de subred entre /28 y /24 al definir los intervalos IP de subred. Un poco de máscara de red de/28 (14 hosts por red) es un buen tamaño para una implementación crítico para la empresa o el único propósito general. Un poco de máscara de/27 (30 hosts por red) es ideal para una varias implementaciones de instancia administrada dentro de la misma red virtual. Configuración de bits de máscara de /26 (62 hosts) y /24 (254 hosts) permite aumentar la escala fuera de la red virtual para admitir instancias administradas adicionales.

> [!IMPORTANT]
> Un tamaño de subred con 16 direcciones IP es lo mínimo con limitado potencial de la otra instancia administrada escalada. Se recomienda elegir una subred con el prefijo /27 o uno inferior.

## <a name="determine-subnet-size"></a>Determinación del tamaño de la subred

Si tiene previsto implementar varias instancias administradas dentro de la subred y tiene que optimizar el tamaño de la subred, utilice estos parámetros para hacer un cálculo:

- Azure utiliza cinco direcciones IP de la subred para sus propias necesidades
- Cada instancia de uso general necesita dos direcciones
- Cada instancia de Crítico para la empresa necesita cuatro direcciones

**Ejemplo**: planea tener tres instancias administradas de Propósito general y dos de Crítico para la empresa. Esto significa que necesita 5 + 3 * 2 + 2 * 4 = 19 direcciones IP. Como los intervalos de IP se definen en potencias de 2, necesita el intervalo de IP de 32 (2 ^ 5) direcciones IP. Por lo tanto, tiene que reservar la subred con la máscara de subred de /27.

> [!IMPORTANT]
> El cálculo que se muestra arriba quedará obsoleto con las futuras mejoras.

## <a name="next-steps"></a>Pasos siguientes

- Para información general, consulte [¿Qué es Instancia administrada?](sql-database-managed-instance.md)
- Obtenga información sobre la [arquitectura de conectividad para Instancia administrada](sql-database-managed-instance-connectivity-architecture.md).
- Consulte cómo [crear la red virtual en la que implementará Instancias administradas](sql-database-managed-instance-create-vnet-subnet.md)
- Para problemas DNS, consulte [Configuración de un DNS personalizado](sql-database-managed-instance-custom-dns.md)
