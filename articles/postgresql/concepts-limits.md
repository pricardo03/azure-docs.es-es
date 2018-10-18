---
title: Limitaciones en Azure Database for PostgreSQL
description: En este artículo se describen las limitaciones de Azure Database for PostgreSQL como el número de conexiones o las opciones de motor de almacenamiento.
services: postgresql
author: rachel-msft
ms.author: raagyema
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/30/2018
ms.openlocfilehash: 0bd894e99fd575fca734a101a1cde0f0a2979682
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2018
ms.locfileid: "45983772"
---
# <a name="limitations-in-azure-database-for-postgresql"></a>Limitaciones en Azure Database for PostgreSQL
En las secciones siguientes se describen los límites de capacidad y funcionales en el servicio de base de datos.

## <a name="maximum-connections"></a>Número máximo de conexiones
El número máximo de conexiones por plan de tarifa y núcleos virtuales es el siguiente: 

|**Plan de tarifa**| **Núcleos virtuales**| **Conexiones máximas** |
|---|---|---|
|Básica| 1| 50 |
|Básica| 2| 100 |
|Uso general| 2| 150|
|Uso general| 4| 250|
|Uso general| 8| 480|
|Uso general| 16| 950|
|Uso general| 32| 1.500|
|Memoria optimizada| 2| 150|
|Memoria optimizada| 4| 250|
|Memoria optimizada| 8| 480|
|Memoria optimizada| 16| 950|

Si las conexiones superan el límite, puede que reciba el error siguiente:
> FATAL:  sorry, too many clients already

El sistema de Azure requiere cinco conexiones para supervisar el servidor de Azure Database for PostgreSQL. 

## <a name="functional-limitations"></a>Limitaciones funcionales
### <a name="scale-operations"></a>Operaciones de escalado
- El escalado dinámico a y desde niveles de precios Básico no se admite en este momento.
- La reducción del tamaño de almacenamiento del servidor no se admite actualmente.

### <a name="server-version-upgrades"></a>Actualizaciones de la versión de servidor
- La migración automatizada entre las principales versiones del motor de base de datos no se admite en este momento. Si desea actualizar a la siguiente versión principal, realice un [volcado de memoria y restáurelo ](./howto-migrate-using-dump-and-restore.md) a un servidor que se haya creado con la nueva versión del motor.

### <a name="vnet-service-endpoints"></a>Puntos de conexión de servicio de red virtual
- La compatibilidad con puntos de conexión de servicio de red virtual solo existe para servidores de uso general y optimizados para memoria.

### <a name="restoring-a-server"></a>Restauración de un servidor
- Al usar la característica PITR, el nuevo servidor se crea con la misma configuración de plan de tarifa que el servidor en el que se basa.
- El servidor creado durante una restauración no tiene las reglas de firewall que existían en el servidor original. Las reglas de firewall deben configurarse por separado para este nuevo servidor.
- La restauración a un servidor que se ha eliminado no se admite en este momento.

## <a name="next-steps"></a>Pasos siguientes
- Comprenda lo que [hay disponible en cada plan de tarifa](concepts-pricing-tiers.md).
- Conozca las [versiones de base de datos de PostgreSQL admitidas](concepts-supported-versions.md).
- Revise [cómo hacer una copia de seguridad de un servidor y restaurarlo en Azure Database for PostgreSQL mediante Azure Portal](howto-restore-server-portal.md).
