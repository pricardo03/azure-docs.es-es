---
title: Cómo configurar Instancia administrada de Azure SQL Database| Microsoft Docs
description: Aprenda cómo configurar y administrar Instancia administrada de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: carlr
manager: craigg
ms.date: 12/14/2018
ms.openlocfilehash: 10a33ac09b5cfa588bef88e6c1587d67036b1aef
ms.sourcegitcommit: c2e61b62f218830dd9076d9abc1bbcb42180b3a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/15/2018
ms.locfileid: "53439522"
---
# <a name="how-to-use-managed-instance"></a>Cómo usar Instancia administrada

En esta sección encontrará varias guías, scripts y explicaciones que pueden ayudarle a administrar y configurar Instancia administrada de Azure SQL Database.

## <a name="migration"></a>Migración

- [Migrar a Instancia administrada](sql-database-managed-instance-migrate.md) : obtenga información sobre el proceso de migración y las herramientas que se recomiendan para la migración a Instancia administrada.

- [Migrar el certificado de TDE a Instancia administrada](sql-database-managed-instance-migrate-tde-certificate.md): si su base de datos de SQL Server está protegida con cifrado de datos transparente (TDE), deberá migrar el certificado que Instancia administrada puede usar para descifrar la copia de seguridad que quiere restaurar en Azure.

## <a name="network-configuration"></a>Network configuration (Configuración de red)

- [Determinar el tamaño de la subred de Instancia administrada](sql-database-managed-instance-determine-size-vnet-subnet.md): Instancia administrada se coloca en subred dedicadas que no se pueden cambiar de tamaño una vez se le agregan recursos. Por lo tanto, debería calcular el intervalo IP de direcciones necesario para la subred según el número y los tipos de instancias que quiera implementar en la subred.
- [Crear una nueva red virtual y una subred para Instancia administrada](sql-database-managed-instance-create-vnet-subnet.md): la red virtual de Azure y la subred donde quiere implementar las instancias administradas deben configurarse de acuerdo con los [requisitos de red que se describen aquí](sql-database-managed-instance-connectivity-architecture.md#network-requirements). En esta guía puede encontrar la manera más fácil de crear la red virtual y la subred nuevas configuradas correctamente para las instancias administradas.
- [Configurar la red virtual y la subred existentes para Instancia administrada](sql-database-managed-instance-configure-vnet-subnet.md): si quiere configurar la red virtual y la subred existentes para implementar instancias administradas dentro de estas, aquí puede encontrar el script que comprueba los [requisitos de red](sql-database-managed-instance-connectivity-architecture.md#network-requirements) y configurar la subred según los requisitos.
- [Configurar el DNS personalizado](sql-database-managed-instance-custom-dns.md): debe configurar el DNS personalizado si quiere acceder a recursos externos en los dominios personalizados desde su Instancia administrada a través de un servidor vinculado de perfiles de correo de base de datos.
- [Sincronizar la configuración de red](sql-database-managed-instance-sync-network-configuration.md): puede suceder que aunque haya [integrado la aplicación con una red de Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md) no pueda establecer conexión con Instancia administrada. Puede intentar actualizar la configuración de red para el plan de servicio que tenga.
- [Buscar dirección IP de punto de conexión de administración](sql-database-managed-instance-find-management-endpoint-ip-address.md): Instancia administrada utiliza el punto de conexión público solo con fines administrativos. Puede determinar la dirección IP del punto de conexión de administración mediante el script que se describe aquí.
- [Comprobar la protección de firewall integrado](sql-database-managed-instance-management-endpoint-verify-built-in-firewall.md): la Instancia administrada está protegida con un firewall integrado que permite el tráfico solo en los puertos necesarios. Puede comprobar las reglas de firewall integradas mediante el script que se describe en esta guía.
- [Conectar aplicaciones](sql-database-managed-instance-connect-app.md): Instancia administrada se coloca en su propia red virtual privada de Azure con la dirección IP privada. Obtenga información sobre los distintos patrones para conectar las aplicaciones a su Instancia administrada.

## <a name="feature-configuration"></a>Configuración de características

- La [replicación transaccional](replication-with-sql-database-managed-instance.md) permite replicar los datos entre instancias administradas, o desde un entorno local de SQL Server a Instancia administrada y viceversa. Puede encontrar más información sobre cómo usar y configurar la replicación de transacciones en esta guía.
- [Configurar la detección de amenazas](sql-database-managed-instance-threat-detection.md):la [detección de amenazas](sql-database-threat-detection-overview.md) es una característica integrada de Azure SQL Database que detecta diversos ataques potenciales, como la inyección de código SQL o el acceso desde ubicaciones sospechosas. En esta guía obtendrá información sobre cómo habilitar y configurar la [detección de amenazas](sql-database-threat-detection-overview.md) para Instancia administrada.

## <a name="next-steps"></a>Pasos siguientes
- Obtenga más información sobre las [guías de procedimientos en Base de datos única](sql-database-howto-single-database.md).