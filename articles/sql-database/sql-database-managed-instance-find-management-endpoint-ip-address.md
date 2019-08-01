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
ms.date: 12/04/2018
ms.openlocfilehash: a0d47c61bf84cfb22c767fd09b3feed74f55b7fc
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567493"
---
# <a name="determine-the-management-endpoint-ip-address"></a>Determinación de la dirección IP del punto de conexión de administración

El clúster virtual de Instancia administrada de Azure SQL Database contiene un punto de conexión de administración que Microsoft utiliza para operaciones de administración. El punto de conexión de administración está protegido con un firewall integrado a nivel de red y con comprobación mutua de certificados a nivel de aplicación. Puede determinar la dirección IP del punto de conexión de administración, pero no puede acceder a él.

Para determinar la dirección IP de administración, realice una búsqueda DNS en el FQDN de la instancia administrada: `mi-name.zone_id.database.windows.net`. Esto devolverá una entrada DNS como `trx.region-a.worker.vnet.database.windows.net`. Después, puede hacer una búsqueda DNS en este FQDN con ".vnet" eliminado. Esto devolverá la dirección IP de administración. 

Esta instancia de PowerShell lo hará automáticamente si reemplaza \<MI FQDN\> con la entrada DNS de la instancia administrada: `mi-name.zone_id.database.windows.net`.
  
``` powershell
  $MIFQDN = "<MI FQDN>"
  resolve-dnsname $MIFQDN | select -first 1  | %{ resolve-dnsname $_.NameHost.Replace(".vnet","")}
```

Para más información acerca de Instancias administradas y conectividad, consulte [Arquitectura de conectividad de Instancia administrada de Azure SQL Database](sql-database-managed-instance-connectivity-architecture.md).
