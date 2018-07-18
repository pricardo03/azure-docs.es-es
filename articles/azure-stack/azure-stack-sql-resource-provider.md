---
title: Uso de bases de datos SQL en Azure Stack | Microsoft Docs
description: Aprenda a implementar las bases de datos SQL como un servicio en Azure Stack y los pasos para implementar de forma rápida el adaptador del proveedor de recursos de SQL Server.
services: azure-stack
documentationCenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/21/2018
ms.author: jeffgilb
ms.reviewer: jeffgo
ms.openlocfilehash: 55d0e51606e8768a01c0b5a7766dbafe24d97a0d
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/21/2018
ms.locfileid: "36307832"
---
# <a name="use-sql-databases-on-microsoft-azure-stack"></a>Uso de bases de datos SQL en Microsoft Azure Stack

Use la API del adaptador del proveedor de recursos de SQL Server para exponer las bases de datos SQL como un servicio de [Azure Stack](azure-stack-poc.md). Después de instalar el proveedor de recursos y conectarse a una o varias instancias de SQL Server, usted y sus usuarios pueden crear:

- Bases de datos para las aplicaciones nativas de la nube.
- Sitios web que usan SQL.
- Cargas de trabajo que usan SQL.

El proveedor de recursos no proporciona todas las capacidades de administración de bases de datos de [Azure SQL Database](https://azure.microsoft.com/services/sql-database/). Por ejemplo, no se admiten grupos elásticos que asignan recursos automáticamente. Sin embargo, el proveedor de recursos admite operaciones similares de creación, lectura, actualización y eliminación (CRUD) en una base de datos de SQL Server. Para obtener más información acerca de la API del proveedor de recursos, consulte [Windows Azure Pack SQL Server Resource Provider REST API Reference](https://msdn.microsoft.com/library/dn528529.aspx) (Referencia de la API de REST del proveedor de recursos de SQL Server de Windows Azure Pack).

>[!NOTE]
La API del proveedor de recursos de SQL Server no es compatible con Azure SQL Database.

## <a name="sql-resource-provider-adapter-architecture"></a>Arquitectura del adaptador del proveedor de recursos de SQL

El proveedor de recursos consta de los componentes siguientes:

- **La máquina virtual (VM) del adaptador del proveedor de recursos de SQL**, que es una máquina virtual de Windows Server que ejecuta los servicios del proveedor.
- **El proveedor de recursos**, que procesa las solicitudes y accede a los recursos de las bases de datos.
- **Los servidores que hospedan SQL Server**, que proporcionan capacidad para las bases de datos, conocidos como servidores de hospedaje.

Debe crear como mínimo una instancia de SQL Server o proporcionar acceso a instancias externas de SQL Server.

> [!NOTE]
> Los servidores de hospedaje que se instalan en sistemas integrados de Azure Stack se deben crear mediante una suscripción de inquilino. No se pueden crear a partir de la suscripción de proveedor predeterminada. Se deben crear desde el portal de inquilino o mediante PowerShell con el inicio de sesión adecuado. Todos los servidores de hospedaje son máquinas virtuales facturables y deben tener licencias. El administrador del servicio puede ser el propietario de la suscripción de inquilino.

## <a name="next-steps"></a>Pasos siguientes

[Implementar un proveedor de recursos de SQL Server](azure-stack-sql-resource-provider-deploy.md)
