---
title: Notas de la versión de SQL Server en máquinas virtuales de Azure| Microsoft Docs
description: Conozca las nuevas características y mejoras de SQL Server en una máquina virtual de Azure.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
editor: ''
tags: azure-service-management
ms.assetid: 2fa5ee6b-51a6-4237-805f-518e6c57d11b
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 11/13/2018
ms.author: mathoma
ms.openlocfilehash: e3f44181ca1a5ea64815aadf52aa7ea792a21416
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2019
ms.locfileid: "54358582"
---
# <a name="sql-server-on-azure-virtual-machine-release-notes"></a>Notas de la versión de SQL Server en máquinas virtuales de Azure

Azure permite implementar una máquina virtual con una imagen de SQL Server integrada. En este artículo se indican las nuevas características y mejoras que puede esperar en la versión más reciente de SQL Server implementada en una máquina virtual de Azure. 

## <a name="december-2018"></a>Diciembre de 2018

| **Cambio** | Detalles |
| --- | --- |
| **Nuevo proveedor de recursos del grupo de clústeres SQL** | Hay un nuevo proveedor de recursos (Microsoft.SqlVirtualMachine/SqlVirtualMachineGroup) que define los metadatos acerca del clúster de conmutación por error de Windows. Al unir una VM con SQL Server a *SqlVirtualMachineGroup*, se arranca el servicio de clúster de conmutación por error de Windows y se une la VM al clúster.  |
|**Automatización de la configuración de implementación de un grupo de disponibilidad con plantillas de inicio rápido de Azure** |Ahora es posible crear el clúster de conmutación por error de Windows, unirlo a las VM con SQL Server, crear el agente de escucha y configurar Load Balancer interno con dos plantillas de inicio rápido de Azure. Para obtener más información, consulte [Create WSFC, listener, and configure ILB for an Always On availability group on a SQL Server VM with Azure Quickstart Template](virtual-machines-windows-sql-availability-group-quickstart-template.md) (Creación del clúster WSFC y el agente de escucha y configuración de ILB para un grupo de disponibilidad AlwaysOn en una VM con SQL Server con plantillas de inicio rápido de Azure). | 
| **Registro automático del proveedor de recursos de VM de SQL** | Las VM con SQL Server implementadas después de este mes se registran automáticamente con el nuevo proveedor de recursos de SQL Server. Las VM con SQL Server implementadas antes de este mes deberán registrarse manualmente. Para más información, consulte [Registro de VM con SQL existentes con el nuevo proveedor de recursos](virtual-machines-windows-sql-ahb.md#register-existing-sql-server-vm-with-new-resource-provider).|
| &nbsp; | &nbsp; |


## <a name="november-2018"></a>Noviembre de 2018

| **Cambio** | Detalles |
| --- | --- |
| **Nuevo proveedor de recursos de VM con SQL** |  Hay un nuevo proveedor de recursos para VM con SQL Server (Microsoft.SqlVirtualMachine) que permite una mejor administración de la VM con SQL Server. Para más información sobre cómo registrar la máquina virtual, consulte [Registro de máquinas virtuales de SQL existentes con el nuevo proveedor de recursos](virtual-machines-windows-sql-ahb.md#register-existing-sql-server-vm-with-new-resource-provider). |
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
* [Aprovisionamiento de una máquina virtual Linux con SQL Server](../../linux/sql/provision-sql-server-linux-virtual-machine.md)
* [Preguntas más frecuentes (Linux)](../../linux/sql/sql-server-linux-faq.md)
* [Documentación de SQL Server en Linux](https://docs.microsoft.com/sql/linux/sql-server-linux-overview)
