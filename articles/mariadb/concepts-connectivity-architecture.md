---
title: 'Arquitectura de la conectividad: Azure Database for MariaDB'
description: Describe la arquitectura de conectividad para el servidor de Azure Database for MariaDB.
author: kummanish
ms.author: manishku
ms.service: mariadb
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: d658a1fb590d117e116321bc854abd2753b5c0f1
ms.sourcegitcommit: 984c5b53851be35c7c3148dcd4dfd2a93cebe49f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2020
ms.locfileid: "76775054"
---
# <a name="connectivity-architecture-in-azure-database-for-mariadb"></a>Arquitectura de la conectividad en Azure Database for MariaDB
En este artículo se explica la arquitectura de la conectividad de Azure Database for MariaDB y cómo se dirige el tráfico a la instancia de Azure Database for MariaDB desde los clientes de dentro y de fuera de Azure.

## <a name="connectivity-architecture"></a>Arquitectura de conectividad

La conexión a la base de datos de Azure Database for MariaDB se establece a través de una puerta de enlace que se encarga de enrutamiento de las conexiones entrantes a la ubicación física del servidor en nuestros clústeres. En el diagrama siguiente se muestra este flujo de tráfico.

![Información general de la arquitectura de conectividad](./media/concepts-connectivity-architecture/connectivity-architecture-overview-proxy.png)

Al conectarse a la base de datos, los clientes obtienen una cadena de conexión para conectarse a la puerta de enlace. Esta puerta de enlace tiene una dirección IP pública que escucha el puerto 3306. Dentro, el tráfico del clúster se reenvía a la instancia de Azure Database for MariaDB adecuada. Por tanto, para conectarse al servidor, como en las redes corporativas, es necesario abrir el firewall del lado cliente para permitir que el tráfico saliente llegue a nuestras puertas de enlace. A continuación encontrará una lista completa de las direcciones IP que usan nuestras puertas de enlace por región.

## <a name="azure-database-for-mariadb-gateway-ip-addresses"></a>Direcciones IP de la puerta de enlace de Azure Database for MariaDB

En la siguiente tabla se enumeran las direcciones IP principales y secundarias de la puerta de enlace de Azure Database for MariaDB para todas las regiones de datos. La dirección IP principal es la dirección IP actual de la puerta de enlace y la dirección IP secundaria es una dirección IP de conmutación por error en caso de que se produzca un error en la principal. Como ya se ha mencionado, los clientes deben permitir el tráfico saliente a las dos direcciones IP. La dirección IP secundaria no escucha en ningún servicio hasta que Azure Database for MariaDB la activa para aceptar conexiones.

| **Nombre de la región** | **Direcciones IP de puerta de enlace** |
|:----------------|:-------------|
| Centro de Australia| 20.36.105.0     |
| Centro de Australia 2     | 20.36.113.0   |
| Este de Australia | 13.75.149.87, 40.79.161.1     |
| Sudeste de Australia |191.239.192.109, 13.73.109.251   |
| Sur de Brasil | 104.41.11.5, 191.233.201.8, 191.233.200.16  |
| Centro de Canadá |40.85.224.249  |
| Este de Canadá | 40.86.226.166    |
| Centro de EE. UU. | 23.99.160.139, 13.67.215.62   |
| Este de China 2 | 40.73.82.1  |
| Norte de China 2 | 40.73.50.0     |
| Asia oriental | 191.234.2.139, 52.175.33.150, 13.75.33.20, 13.75.33.21     |
| Este de EE. UU. | 40.121.158.30, 191.238.6.43  |
| Este de EE. UU. 2 |40.79.84.180, 191.239.224.107, 52.177.185.181   |
| Centro de Francia | 40.79.137.0, 40.79.129.1  |
| Centro de Alemania | 51.4.144.100     |
| Nordeste de Alemania | 51.5.144.179  |
| India central | 104.211.96.159     |
| Sur de India | 104.211.224.146  |
| India occidental | 104.211.160.80    |
| Japón Oriental | 13.78.61.196, 191.237.240.43  |
| Japón Occidental | 104.214.148.156, 191.238.68.11, 40.74.96.6, 40.74.96.7    |
| Centro de Corea del Sur | 52.231.32.42   |
| Corea del Sur | 52.231.200.86    |
| Centro-Norte de EE. UU | 23.96.178.199, 23.98.55.75, 52.162.104.35, 52.162.104.36    |
| Norte de Europa | 40.113.93.91, 191.235.193.75    |
| Norte de Sudáfrica  | 102.133.152.0    |
| Oeste de Sudáfrica | 102.133.24.0   |
| Centro-sur de EE. UU. |13.66.62.124, 23.98.162.75, 20.45.120.0, 104.214.16.39   |
| Sudeste Asiático | 104.43.15.0, 23.100.117.95, 40.78.233.2, 23.98.80.12     |
| Centro de Emiratos Árabes Unidos | 20.37.72.64  |
| Norte de Emiratos Árabes Unidos | 65.52.248.0    |
| Sur de Reino Unido 2 | 51.140.184.11   |
| Oeste de Reino Unido | 51.141.8.11  |
| Centro-Oeste de EE. UU. | 13.78.145.25     |
| Oeste de Europa | 40.68.37.158, 191.237.232.75     |
| Oeste de EE. UU. | 104.42.238.205, 23.99.34.75  |
| Oeste de EE. UU. 2 | 13.66.226.202  |
||||

## <a name="next-steps"></a>Pasos siguientes

* [Create and manage Azure Database for MariaDB firewall rules using the Azure portal](./howto-manage-firewall-portal.md) (Creación y administración de reglas de firewall de Azure Database for MariaDB mediante Azure Portal)
* [Create and manage Azure Database for MariaDB firewall rules using Azure CLI](./howto-manage-firewall-cli.md) (Creación y administración de reglas de firewall de Azure Database for MariaDB mediante la CLI de Azure)
