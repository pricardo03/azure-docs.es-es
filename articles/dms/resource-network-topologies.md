---
title: Topologías de red para migraciones de Instancia administrada de Azure SQL Database mediante Azure Database Migration Service | Microsoft Docs
description: Más información acerca de las configuraciones de origen y de destino de Azure Database Migration Service.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/07/2019
ms.openlocfilehash: 74613599903f7cde606295a1e2d9eaaa0924cf50
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66808425"
---
# <a name="network-topologies-for-azure-sql-db-managed-instance-migrations-using-azure-database-migration-service"></a>Topologías de red para migraciones de Instancia administrada de Azure SQL Database mediante Azure Database Migration Service

En este artículo se tratan diversas topologías de red con las que Azure Database Migration Service puede trabajar para proporcionar una experiencia de migración completa desde instancias locales de SQL Server a Instancia administrada de Azure SQL Database.

## <a name="azure-sql-database-managed-instance-configured-for-hybrid-workloads"></a>Instancia administrada de Azure SQL Database configurada para cargas de trabajo híbridas 

Use esta topología si su instancia administrada de Azure SQL Database está conectada a la red local. Este enfoque proporciona el enrutamiento de red más simplificado y genera el máximo rendimiento de los datos durante la migración.

![Topología de red para cargas de trabajo híbridas](media/resource-network-topologies/hybrid-workloads.png)

**Requisitos**

- En este escenario, Instancia administrada de Azure SQL Database y la instancia de Azure Database Migration Service se crean en la misma red virtual de Azure pero usan diferentes subredes.  
- La red virtual que se utiliza en este escenario también está conectada a la red local mediante [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) o [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).

## <a name="azure-sql-database-managed-instance-isolated-from-the-on-premises-network"></a>Instancia administrada de Azure SQL Database aislada de la red local

Use esta topología de red si su entorno requiere uno o varios de los siguientes escenarios:

- Instancia administrada de Azure SQL Database está aislada de la conectividad local pero la instancia de Azure Database Migration Service está conectada a la red local.
- Si se han implementado directivas de control de acceso basado en roles (RBAC) y tiene que limitar el acceso de los usuarios a la misma suscripción que hospeda Instancia administrada de Azure SQL Database.
- Las redes virtuales que se usan para Instancia administrada de Azure SQL Database y Azure Database Migration Service se encuentran en suscripciones distintas.

![Topología de red para Instancia administrada aislada de la red local](media/resource-network-topologies/mi-isolated-workload.png)

**Requisitos**

- La red virtual que utiliza Azure Database Migration Service para este escenario también debe estar conectada a la red local mediante (https://docs.microsoft.com/azure/expressroute/expressroute-introduction) o [VPN](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpngateways).
- Configure un [emparejamiento de red virtual](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) entre la red virtual que se usa para Instancia administrada de Azure SQL Database y Azure Database Migration Service.

## <a name="cloud-to-cloud-migrations-shared-vnet"></a>Migraciones de nube a nube: red virtual compartida

Use esta topología si el servidor SQL Server de origen se hospeda en una máquina virtual de Azure y comparte la misma red virtual con Instancia administrada de Azure SQL Database y Azure Database Migration Service.

![Topología de red para migraciones de nube a nube con una red virtual compartida](media/resource-network-topologies/cloud-to-cloud.png)

**Requisitos**

- No hay requisitos adicionales.

## <a name="cloud-to-cloud-migrations-isolated-vnet"></a>Migraciones de nube a nube: red virtual aislada

Use esta topología de red si su entorno requiere uno o varios de los siguientes escenarios:

- Instancia administrada de Azure SQL Database se aprovisiona en una red virtual aislada.
- Si se han implementado directivas de control de acceso basado en roles (RBAC) y tiene que limitar el acceso de los usuarios a la misma suscripción que hospeda Instancia administrada de Azure SQL Database.
- Las redes virtuales que se usan para Instancia administrada de Azure SQL Database y Azure Database Migration Service se encuentran en suscripciones distintas.

![Topología de red para migraciones de nube a nube con una red virtual aislada](media/resource-network-topologies/cloud-to-cloud-isolated.png)

**Requisitos**

- Configure un [emparejamiento de red virtual](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) entre la red virtual que se usa para Instancia administrada de Azure SQL Database y Azure Database Migration Service.

## <a name="inbound-security-rules"></a>Reglas de seguridad de entrada

| **NOMBRE**   | **PUERTO** | **PROTOCOLO** | **ORIGEN** | **DESTINO** | **ACCIÓN** |
|------------|----------|--------------|------------|-----------------|------------|
| DMS_subnet | Cualquiera      | Cualquiera          | SUBRED DMS | Cualquiera             | PERMITIR      |

## <a name="outbound-security-rules"></a>Reglas de seguridad de entrada

| **NOMBRE**                  | **PUERTO**                                              | **PROTOCOLO** | **ORIGEN** | **DESTINO**           | **ACCIÓN** | **Motivo de la regla**                                                                                                                                                                              |
|---------------------------|-------------------------------------------------------|--------------|------------|---------------------------|------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| management                | 443,9354                                              | TCP          | Cualquiera        | Cualquiera                       | PERMITIR      | Comunicación de planos de administración mediante Service Bus y Azure Blob Storage. <br/>(Si está habilitado el emparejamiento de Microsoft, es posible que no necesite esta regla).                                                             |
| Diagnóstico               | 12000                                                 | TCP          | Cualquiera        | Cualquiera                       | PERMITIR      | DMS usa esta regla para recopilar información de diagnóstico para solucionar problemas.                                                                                                                      |
| Servidor de origen SQL         | 1433 (o puerto TCP IP en el que SQL Server escucha) | TCP          | Cualquiera        | Espacio de direcciones local | PERMITIR      | Conectividad del origen de SQL Server desde DMS <br/>(Si dispone de conectividad de sitio a sitio, es posible que no necesite esta regla).                                                                                       |
| Instancia con nombre de SQL Server | 1434                                                  | UDP          | Cualquiera        | Espacio de direcciones local | PERMITIR      | Conectividad del origen de las instancias con nombre desde DMS <br/>(Si dispone de conectividad de sitio a sitio, es posible que no necesite esta regla).                                                                        |
| Recurso compartido de SMB                 | 445                                                   | TCP          | Cualquiera        | Espacio de direcciones local | PERMITIR      | Recurso compartido de red SMB para DMS para almacenar los archivos de copia de seguridad de base de datos de las migraciones a Azure SQL Database MI y servidores SQL Server en Azure VM <br/>(Si dispone de conectividad de sitio a sitio, es posible que no necesite esta regla). |
| DMS_subnet                | Cualquiera                                                   | Cualquiera          | Cualquiera        | DMS_Subnet                | PERMITIR      |                                                                                                                                                                                                  |

## <a name="see-also"></a>Otras referencias

- [Migración de SQL Server a Instancia administrada de Azure SQL Database](https://docs.microsoft.com/azure/dms/tutorial-sql-server-to-managed-instance)
- [Introducción a los requisitos previos para usar Azure Database Migration Service](https://docs.microsoft.com/azure/dms/pre-reqs)
- [Creación de una red virtual mediante Azure Portal](https://docs.microsoft.com/azure/virtual-network/quick-create-portal)

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información general de Azure Database Migration Service, consulte el artículo [¿Qué es Azure Database Migration Service?](dms-overview.md).
- Para obtener información actualizada acerca de la disponibilidad regional de Azure Database Migration Service, consulte la página [Productos disponibles por región](https://azure.microsoft.com/global-infrastructure/services/?products=database-migration).
