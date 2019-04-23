---
title: Detección del punto de conexión de administración de Instancia administrada de Azure SQL Database | Microsoft Docs
description: Aprenda a obtener la dirección IP pública del punto de conexión de administración de Instancia administrada de Azure SQL Database y compruebe la protección de su firewall integrado
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: sstein, carlrab
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: b7eb9ecd6b94aad263346ad6b5c45b694e0bd46f
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/18/2019
ms.locfileid: "59797965"
---
# <a name="determine-the-management-endpoint-ip-address"></a>Determinación de la dirección IP del punto de conexión de administración

El clúster virtual de Instancia administrada de Azure SQL Database contiene un punto de conexión de administración que Microsoft utiliza para operaciones de administración. El punto de conexión de administración está protegido con un firewall integrado a nivel de red y con comprobación mutua de certificados a nivel de aplicación. Puede determinar la dirección IP del punto de conexión de administración, pero no puede acceder a él.

## <a name="determine-ip-address"></a>Determinación de la dirección IP

Supongamos que el host de Instancia administrada es `mi-demo.xxxxxx.database.windows.net`. Ejecute `nslookup` mediante el nombre de host.

![Resolución de un nombre de host interno](./media/sql-database-managed-instance-management-endpoint/01_find_internal_host.png)

Ahora use otro `nslookup` para el nombre resaltado eliminando el segmento `.vnet.`. Al ejecutar este comando, obtendrá la dirección IP pública.

![Resolución de la dirección IP pública](./media/sql-database-managed-instance-management-endpoint/02_find_public_ip.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información acerca de Instancias administradas y conectividad, consulte [Arquitectura de conectividad de Instancia administrada de Azure SQL Database](sql-database-managed-instance-connectivity-architecture.md).
