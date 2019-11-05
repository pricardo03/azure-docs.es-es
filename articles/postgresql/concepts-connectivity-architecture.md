---
title: Arquitectura de la conectividad en Azure Database for PostgreSQL
description: Describe la arquitectura de conectividad para su servidor de Azure Database for PostgreSQL.
author: kummanish
ms.author: manishku
ms.service: postgresql
ms.topic: conceptual
ms.date: 05/23/2019
ms.openlocfilehash: 5288c3025e523e767082f4ab7cfbb08805a74eaf
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73500087"
---
# <a name="connectivity-architecture-in-azure-database-for-postgresql"></a>Arquitectura de la conectividad en Azure Database for PostgreSQL
En este artículo se explica la arquitectura de la conectividad de Azure Database for PostgreSQL y cómo se dirige el tráfico a la instancia de base de datos de Azure Database for PostgreSQL desde los clientes de dentro y de fuera de Azure.

## <a name="connectivity-architecture"></a>Arquitectura de conectividad
La conexión a la base de datos de Azure Database for PostgreSQL se establece a través de una puerta de enlace que se encarga de enrutamiento de las conexiones entrantes a la ubicación física del servidor en nuestros clústeres. En el siguiente diagrama se muestra este flujo de tráfico.

![Información general de la arquitectura de conectividad](./media/concepts-connectivity-architecture/connectivity-architecture-overview-proxy.png)

Al conectarse a la base de datos, los clientes obtienen una cadena de conexión para conectarse a la puerta de enlace. Esta puerta de enlace tiene una dirección IP pública que escucha el puerto 5432. Dentro del clúster de base de datos, el tráfico se reenvía a la instancia de Azure Database for PostgreSQL adecuada. Por tanto, para conectarse al servidor, como en las redes corporativas, es necesario abrir el firewall del lado cliente para permitir que el tráfico saliente llegue a nuestras puertas de enlace. A continuación encontrará una lista completa de las direcciones IP que usan nuestras puertas de enlace por región.

## <a name="azure-database-for-postgresql-gateway-ip-addresses"></a>Direcciones IP de la puerta de enlace de Azure Database for PostgreSQL
En la siguiente tabla se enumeran las direcciones IP principales y secundarias de la puerta de enlace de Azure Database for PostgreSQL para todas las regiones de datos. La dirección IP principal es la dirección IP actual de la puerta de enlace y la dirección IP secundaria es una dirección IP de conmutación por error en caso de que falle la principal. Como ya se ha mencionado, los clientes deben permitir el tráfico saliente a ambas direcciones IP. La dirección IP secundaria no escucha en ningún servicio hasta que Azure Database for PostgreSQL la activa para aceptar conexiones.

| **Nombre de la región** | **Dirección IP principal** | **Dirección IP secundaria** |
|:----------------|:-------------|:------------------------|
| Este de Australia | 13.75.149.87 | 40.79.161.1 |
| Sudeste de Australia | 191.239.192.109 | 13.73.109.251 |
| Sur de Brasil | 104.41.11.5 | |
| Centro de Canadá | 40.85.224.249 | |
| Este de Canadá | 40.86.226.166 | |
| Centro de EE. UU. | 23.99.160.139 | 13.67.215.62 |
| Este de China 1 | 139.219.130.35 | |
| Este de China 2 | 40.73.82.1 | |
| Norte de China 1 | 139.219.15.17 | |
| Norte de China 2 | 40.73.50.0 | |
| Asia oriental | 191.234.2.139 | 52.175.33.150 |
| Este de EE. UU. 1 | 191.238.6.43 | 40.121.158.30 |
| Este de EE. UU. 2 | 191.239.224.107 | 40.79.84.180 * |
| Centro de Francia | 40.79.137.0 | 40.79.129.1 |
| Centro de Alemania | 51.4.144.100 | |
| India central | 104.211.96.159 | |
| Sur de India | 104.211.224.146 | |
| India occidental | 104.211.160.80 | |
| Este de Japón | 191.237.240.43 | 13.78.61.196 |
| Oeste de Japón | 191.238.68.11 | 104.214.148.156 |
| Corea Central | 52.231.32.42 | |
| Corea del Sur | 52.231.200.86 |  |
| Centro-Norte de EE. UU | 23.98.55.75 | 23.96.178.199 |
| Europa del Norte | 191.235.193.75 | 40.113.93.91 |
| Centro-Sur de EE. UU | 23.98.162.75 | 13.66.62.124 |
| Sudeste de Asia | 23.100.117.95 | 104.43.15.0 |
| Norte de Sudáfrica | 102.133.152.0 | |
| Oeste de Sudáfrica | 102.133.24.0 | |
| Norte de Emiratos Árabes Unidos | 65.52.248.0 | |
| Sur de Reino Unido 2 | 51.140.184.11 | |
| Oeste de Reino Unido | 51.141.8.11| |
| Europa occidental | 191.237.232.75 | 40.68.37.158 |
| Oeste de EE. UU. 1 | 23.99.34.75 | 104.42.238.205 |
| Oeste de EE. UU. 2 | 13.66.226.202 | |
||||

> [!NOTE]
> La zona del *Este de EE. UU. 2* también tiene una dirección IP terciaria de `52.167.104.0`.

## <a name="next-steps"></a>Pasos siguientes

* [Create and manage Azure Database for PostgreSQL firewall rules using the Azure portal](./howto-manage-firewall-using-portal.md) (Creación y administración de reglas de firewall de Azure Database for PostgreSQL mediante Azure Portal)
* [Create and manage Azure Database for PostgreSQL firewall rules using Azure CLI](./howto-manage-firewall-using-cli.md) (Creación y administración de reglas de firewall de Azure Database for PostgreSQL mediante la CLI de Azure)
