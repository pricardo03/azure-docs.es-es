---
title: Grupos de disponibilidad de SQL Server - Azure Virtual Machines - Información general | Microsoft Docs
description: En este artículo se describen los grupos de disponibilidad de SQL Server en Azure Virtual Machines.
services: virtual-machines
documentationCenter: na
author: MikeRayMSFT
manager: craigg
editor: monicar
tags: azure-service-management
ms.assetid: 601eebb1-fc2c-4f5b-9c05-0e6ffd0e5334
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 01/13/2017
ms.author: mikeray
ms.openlocfilehash: b9977965dc076ec36aa90680a1732b6640b1e41a
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60325822"
---
# <a name="introducing-sql-server-always-on-availability-groups-on-azure-virtual-machines"></a>Introducción a grupos de disponibilidad de SQL Server AlwaysOn en Azure Virtual Machines #

En este artículo se describen los grupos de disponibilidad de SQL Server en Azure Virtual Machines. 

Los grupos de disponibilidad AlwaysOn en Azure Virtual Machines son similares a los grupos de disponibilidad AlwaysOn locales. Para obtener más información, consulte [Grupos de disponibilidad AlwaysOn (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx). 

El diagrama muestra las partes de un grupo de disponibilidad de SQL Server completo en Azure Virtual Machines.

![Grupo de disponibilidad](./media/virtual-machines-windows-portal-sql-availability-group-tutorial/00-EndstateSampleNoELB.png)

La diferencia clave para un grupo de disponibilidad en Azure Virtual Machines es que las máquinas virtuales de Azure requieren un [equilibrador de carga](../../../load-balancer/load-balancer-overview.md). El equilibrador de carga almacena las direcciones IP del agente de escucha del grupo de disponibilidad. Si tiene más de un grupo de disponibilidad, se necesita un agente de escucha por grupo. Un equilibrador de carga puede admitir varios agentes de escucha.

Además, en un clúster de conmutación por error invitado de máquina virtual de IaaS de Azure, se recomienda una sola NIC por servidor (nodo de clúster) y una sola subred. La red de Azure tiene redundancia física, que hace que las NIC y subredes adicionales sean innecesarias en un clúster invitado de VM de IaaS de Azure. Aunque el informe de validación del clúster emita una advertencia acerca de que los nodos solo son accesibles en una única red, esta advertencia puede omitirse en los clústeres de conmutación por error invitados de VM de IaaS de Azure. 

|  | Versión de Windows Server | Versión de SQL Server | Edición de SQL Server | Configuración de quórum WSFC | Recuperación ante desastres con varias regiones | Compatibilidad con varias subredes | Compatibilidad con una instancia existente de AD | Recuperación ante desastres con la misma región de varias zona | Soporte técnico de dist-AG con ningún dominio de AD | Soporte técnico de dist-AG con ningún clúster |  
| :------ | :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----| :-----|
| [CLI DE MÁQUINA VIRTUAL DE SQL](virtual-machines-windows-sql-availability-group-cli.md) | 2016 | 2017 </br>2016   | ente | Testigo en la nube | Sin  | Sí | Sí | Sí | No | Sin  |
| [Plantillas de inicio rápido](virtual-machines-windows-sql-availability-group-quickstart-template.md) | 2016 | 2017</br>2016  | ente | Testigo en la nube | Sin  | Sí | Sí | Sí | No | Sin  |
| [Plantilla del portal](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) | 2016 </br>2012 R2 | 2016</br>2014 | ente | Recurso compartido de archivos | Sin  | No | No | No | No | Sin  |
| [Manual](virtual-machines-windows-portal-sql-availability-group-prereq.md) | Todo | Todo | Todo | Todo | Sí | Sí | Sí | Sí | Sí | Sí |
| &nbsp; | &nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |&nbsp; |

Cuando esté listo para crear un grupo de disponibilidad de SQL Server en Azure Virtual Machines, consulte estos tutoriales.

## <a name="manually-with-azure-cli"></a>Manualmente con la CLI de Azure
Mediante la CLI de Azure para configurar e implementar un grupo de disponibilidad es la opción recomendada, ya que es el mejor en términos de simplicidad y velocidad de implementación. Con la CLI de Azure, la creación del clúster de conmutación por error de Windows, unir las máquinas virtuales de SQL Server en el clúster, así como la creación del agente de escucha y el equilibrador de carga interno todo se puede lograr en menos de 30 minutos. Esta opción todavía requiere la creación de un manual del grupo de disponibilidad, pero que automatiza todos los demás pasos de configuración necesarios. 

Para obtener más información, consulte [CLI de máquina virtual Use Azure SQL para configurar el grupo de disponibilidad AlwaysOn para SQL Server en una máquina virtual de Azure](virtual-machines-windows-sql-availability-group-cli.md). 

## <a name="automatically-with-azure-quickstart-templates"></a>Automáticamente con plantillas de inicio rápido de Azure
Las plantillas de inicio rápido de Azure usan el proveedor de recursos de VM de SQL para implementar el clúster de conmutación por error de Windows, unir las máquinas virtuales de SQL Server a ella, cree el agente de escucha y configurar el equilibrador de carga interno. Esta opción todavía requiere la creación de un manual del grupo de disponibilidad y equilibrador de carga interno (ILB) pero automatiza y simplifica todos los demás pasos de configuración necesarios (incluida la configuración del ILB). 

Para obtener más información, consulte [plantilla de inicio rápido de Azure de uso para configurar el grupo de disponibilidad AlwaysOn para SQL Server en una máquina virtual de Azure](virtual-machines-windows-sql-availability-group-quickstart-template.md).


## <a name="automatically-with-an-azure-portal-template"></a>Automáticamente con una plantilla de Azure Portal

[Configure Always On availability group in Azure VM automatically - Resource Manager](virtual-machines-windows-portal-sql-alwayson-availability-groups.md) (Configuración automática de grupos de disponibilidad Always On de máquinas virtuales de Azure - Resource Manager)


## <a name="manually-in-azure-portal"></a>Manualmente en el portal de Azure

También puede crear las máquinas virtuales sin la plantilla. En primer lugar, complete los requisitos previos y después cree el grupo de disponibilidad. Vea los siguientes temas: 

- [Configure prerequisites for SQL Server Always On availability groups on Azure Virtual Machines](virtual-machines-windows-portal-sql-availability-group-prereq.md) (Configuración de requisitos previos para grupos de disponibilidad de SQL Server AlwaysOn en Azure Virtual Machines)///

- [Creación de un grupo de disponibilidad AlwaysOn para mejorar la disponibilidad y recuperación ante desastres](virtual-machines-windows-portal-sql-availability-group-tutorial.md)

## <a name="next-steps"></a>Pasos siguientes

[Configuración de un grupo de disponibilidad de SQL Server AlwaysOn en Azure Virtual Machines en regiones distintas](virtual-machines-windows-portal-sql-availability-group-dr.md)
