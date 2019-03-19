---
title: Notas de la versión de SQL Server en máquinas virtuales de Azure| Microsoft Docs
description: Conozca las nuevas características y mejoras de SQL Server en una máquina virtual de Azure.
services: virtual-machines-windows
author: MashaMSFT
ms.author: mathoma
manager: craigg
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 2/13/2019
ms.openlocfilehash: d5c003b2588813f8e8a1dfc2923b9d4c8d8c6cc8
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/26/2019
ms.locfileid: "56820440"
---
# <a name="sql-server-on-azure-virtual-machine-release-notes"></a>Notas de la versión de SQL Server en máquinas virtuales de Azure

Azure permite implementar una máquina virtual con una imagen de SQL Server integrada. En este artículo se resumen las nuevas características y mejoras de las versiones recientes de [SQL Server en VM de Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/). En el artículo también se enumeran las actualizaciones importantes de contenido que no están directamente relacionadas con la versión, pero que se publican en el mismo período de tiempo. Para mejoras en otros servicios de Azure, consulte [las Actualizaciones del servicio](https://azure.microsoft.com/updates).


## <a name="february-2019"></a>Febrero de 2019

### <a name="service-improvements"></a>Mejoras en el servicio

| Mejoras en el servicio | Detalles |
| --- | --- |
| **Mejora del portal** | Ahora es posible cambiar el modelo de licencias para una VM de SQL Server de pago por uso para aportar a su propia-licencia utilizando la [portal Azure](virtual-machines-windows-sql-ahb.md#with-the-azure-portal-1).|
|**Simplificación de la implementación de grupo de disponibilidad con la CLI de máquina virtual de SQL Azure** | Ahora es más fácil que nunca para implementar un grupo de disponibilidad en una máquina virtual de SQL Server en Azure. CLI de máquina virtual de Azure SQL le permite crear el agente de escucha WSFC, el ILB y el grupo de disponibilidad desde la línea de comandos y en un tiempo récord. Para obtener más información, consulte [CLI de máquina virtual Use Azure SQL para configurar el grupo de disponibilidad AlwaysOn para SQL Server en una máquina virtual de Azure](virtual-machines-windows-sql-availability-group-cli.md). | 
| &nbsp; | &nbsp; |

### <a name="documentation-improvements"></a>Mejoras en la documentación

| Mejoras en la documentación | Detalles |
| --- | --- |
|None | |
| | |

## <a name="december-2018"></a>Diciembre de 2018

| Mejoras en el servicio | Detalles |
| --- | --- |
| **Nuevo proveedor de recursos del grupo de clústeres SQL** | Un nuevo proveedor de recursos (Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups) que define los metadatos del clúster de conmutación por error de Windows. Al unir una VM con SQL Server a *SqlVirtualMachineGroups*, se arranca el servicio de clúster de conmutación por error de Windows y se une la VM al clúster.  |
|**Automatización de la configuración de implementación de un grupo de disponibilidad con plantillas de inicio rápido de Azure** |Ahora es posible crear el clúster de conmutación por error de Windows, unirlo a las VM con SQL Server, crear el agente de escucha y configurar Load Balancer interno con dos plantillas de inicio rápido de Azure. Para obtener más información, consulte [plantilla de inicio rápido de Azure de uso para configurar el grupo de disponibilidad AlwaysOn para SQL Server en una máquina virtual de Azure](virtual-machines-windows-sql-availability-group-quickstart-template.md). | 
| **Registro automático del proveedor de recursos de VM de SQL** | Las VM con SQL Server implementadas después de este mes se registran automáticamente con el nuevo proveedor de recursos de SQL Server. Las VM con SQL Server implementadas antes de este mes todavía deben registrarse manualmente. Para obtener más información, consulte [registrar VM de SQL existente con el proveedor de recursos de VM de SQL](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider).|
| &nbsp; | &nbsp; |


## <a name="november-2018"></a>Noviembre de 2018

| Mejoras en el servicio | Detalles |
| --- | --- |
| **Nuevo proveedor de recursos de VM con SQL** |  Un nuevo proveedor de recursos para VM con SQL Server (Microsoft.SqlVirtualMachine) que proporciona una mejor administración de la VM con SQL Server. Para más información sobre cómo registrar la máquina virtual, consulte [Registro de máquinas virtuales de SQL existentes con el nuevo proveedor de recursos](virtual-machines-windows-sql-ahb.md#register-sql-server-vm-with-sql-resource-provider). |
|**Cambiar el modelo de licencia** |Aahora puede cambiar entre los modelos de pago por uso y traer su propia licencia para la VM con SQL mediante PowerShell o la CLI de Azure. Para más información, consulte [Modificación del modelo de licencia para una máquina virtual de SQL en Azure](virtual-machines-windows-sql-ahb.md). | 
| &nbsp; | &nbsp; |


## <a name="additional-resources"></a>Recursos adicionales

**Máquinas virtuales Windows**:

* [Introducción a SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-overview.md)
* [Aprovisionamiento de una máquina virtual Windows con SQL Server](virtual-machines-windows-portal-sql-server-provision.md)
* [Migración de una base de datos a SQL Server en una máquina virtual de Azure](virtual-machines-windows-migrate-sql.md)
* [Alta disponibilidad y recuperación ante desastres para SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-high-availability-dr.md)
* [Prácticas recomendadas para mejorar el rendimiento para SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-performance.md)
* [Estrategias de desarrollo y patrones de aplicación de SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-server-app-patterns-dev-strategies.md)

**Máquinas virtuales Linux**:

* [Introducción a SQL Server en máquinas virtuales Linux](../../linux/sql/sql-server-linux-virtual-machines-overview.md)
* [Aprovisionamiento de una máquina virtual de Linux de SQL Server](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [Preguntas más frecuentes (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [Documentación de SQL Server en Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)