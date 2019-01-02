---
title: Detección del punto de conexión de administración de Instancia administrada de Azure SQL Database | Microsoft Docs
description: Aprenda a obtener la dirección IP pública del punto de conexión de administración de Instancia administrada de Azure SQL Database y compruebe la protección de su firewall integrado
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 83eea565a12ee5201c42b543cdbdad72ddc28ca9
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53345914"
---
# <a name="determine-the-management-endpoint-ip-address"></a>Determinación de la dirección IP del punto de conexión de administración

El clúster virtual de Instancia administrada de Azure SQL Database contiene un punto de conexión de administración que Microsoft utiliza para operaciones de administración. El punto de conexión de administración está protegido con un firewall integrado a nivel de red y con comprobación mutua de certificados a nivel de aplicación. Puede determinar la dirección IP del punto de conexión de administración, pero no puede acceder a él.

## <a name="determine-ip-address"></a>Determinación de la dirección IP

Supongamos que el host de Instancia administrada es `mi-demo.xxxxxx.database.windows.net`. Ejecute `nslookup` mediante el nombre de host.

![Resolución de un nombre de host interno](./media/sql-database-managed-instance-management-endpoint/01_find_internal_host.png)

Ahora use otro `nslookup` para el nombre resaltado eliminando el segmento `.vnet.`. Obtendrá la dirección IP pública como resultado de ejecutar este comando.

![Resolución de la dirección IP pública](./media/sql-database-managed-instance-management-endpoint/02_find_public_ip.png)


## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de Instancias administradas y conectividad, consulte [Arquitectura de conectividad de Instancia administrada de Azure SQL Database](sql-database-managed-instance-connectivity-architecture.md).
