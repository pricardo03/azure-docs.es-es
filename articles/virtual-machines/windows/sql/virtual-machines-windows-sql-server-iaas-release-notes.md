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
ms.date: 08/01/2019
ms.openlocfilehash: e656cd4d901ad9f3180963047a85cd980e56df8b
ms.sourcegitcommit: 4b5dcdcd80860764e291f18de081a41753946ec9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/03/2019
ms.locfileid: "68774215"
---
# <a name="sql-server-on-azure-virtual-machine-release-notes"></a>Notas de la versión de SQL Server en máquinas virtuales de Azure

Azure permite implementar una máquina virtual con una imagen de SQL Server integrada. En este artículo se resumen las nuevas características y mejoras de las versiones recientes de [SQL Server en VM de Azure](https://azure.microsoft.com/services/virtual-machines/sql-server/). En el artículo también se enumeran las actualizaciones importantes de contenido que no están directamente relacionadas con la versión, pero que se publican en el mismo período de tiempo. Para mejoras en otros servicios de Azure, consulte [las Actualizaciones del servicio](https://azure.microsoft.com/updates).

## <a name="july-2019"></a>Julio de 2019

### <a name="documentation-improvements"></a>Mejoras en la documentación

| Documentación | Detalles |
| --- | --- |
| **Traslado de una máquina virtual de SQL a otra región** | Use Azure Site Recovery para [migrar las máquinas virtuales con SQL Server de una región de Azure a otra](virtual-machines-windows-sql-move-different-region.md). |
| &nbsp; | &nbsp; |

## <a name="june-2019"></a>Junio de 2019

### <a name="service-improvements"></a>Mejoras en el servicio

| Mejoras en el servicio | Detalles |
| --- | --- |
| **Nuevos modos de instalación de IaaS de SQL** | Ahora es posible instalar la extensión IaaS de SQL en [modo ligero](virtual-machines-windows-sql-server-agent-extension.md) para evitar tener que reiniciar el servicio SQL Server.  |
| **Modificación de la edición de SQL Server** | Ahora puede cambiar la [propiedad edition](virtual-machines-windows-sql-change-edition.md) de la VM con SQL Server. |
| **Cambio del proveedor de recursos de VM con SQL** | Puede [registrar la máquina virtual con SQL Server con el proveedor de recursos de VM con SQL](virtual-machines-windows-sql-register-with-resource-provider.md#register-with-sql-vm-resource-provider) (incluso [imágenes de Windows 2008](virtual-machines-windows-sql-register-with-resource-provider.md#register-sql-server-2008r2-on-windows-server-2008-vms)) mediante los nuevos modos de IaaS de SQL. |
| **Imágenes BYOL con AHUB** | Ahora es posible cambiar a ["Pago por uso"](virtual-machines-windows-sql-ahb.md#remarks) el tipo de licencia de las imágenes BYOL implementadas desde Marketplace.| 
| &nbsp; | &nbsp; |


## <a name="may-2019"></a>Mayo de 2019

### <a name="service-improvements"></a>Mejoras en el servicio

| Mejoras en el servicio | Detalles |
| --- | --- |
| **Nueva administración de VM con SQL en Azure Portal** | Ahora existe una nueva forma de administrar una VM con SQL Server en Azure Portal. Para más información, consulte [Administración de VM con SQL Server en Azure Portal](virtual-machines-windows-sql-manage-portal.md).  |
| &nbsp; | &nbsp; |

### <a name="documentation-improvements"></a>Mejoras en la documentación

| Documentación | Detalles |
| --- | --- |
| **Nueva administración en el portal de VM con SQL** | Se han actualizado alrededor de una docena de artículos con la nueva experiencia de portal de administración de VM con SQL en el portal. | 
| &nbsp; | &nbsp; |




## <a name="april-2019"></a>Abril de 2019

### <a name="service-improvements"></a>Mejoras en el servicio

| Mejoras en el servicio | Detalles |
| --- | --- |
| **Compatibilidad ampliada con SQL Server 2008/2008 R2** | [Amplíe la compatibilidad](virtual-machines-windows-sql-server-2008-eos-extend-support.md) con SQL Server 2008 y SQL Server 2008 R2 mediante una migración *tal cual* a una máquina virtual de Azure. | 
| &nbsp; | &nbsp; |


## <a name="march-2019"></a>Marzo de 2019

| Mejoras en el servicio | Detalles |
| --- | --- |
| **Compatibilidad de imágenes personalizada** | Ahora puede instalar la [extensión IaaS de SQL](virtual-machines-windows-sql-server-agent-extension.md#installation) en imágenes de SQL y sistemas operativos personalizadas, lo que ofrece una funcionalidad limitada de [licencias flexibles](virtual-machines-windows-sql-ahb.md). Al registrar la imagen personalizada con el proveedor de recursos SQL, especifique el tipo de licencia como "AHUB" o, de lo contrario, se producirá un error durante el registro. | 
| **Compatibilidad de instancias con nombre** | Ahora puede usar la [extensión IaaS de SQL](virtual-machines-windows-sql-server-agent-extension.md#installation) con una instancia con nombre, si la instancia predeterminada se ha desinstalado correctamente. | 
| **Renovación del portal** | La experiencia de Azure Portal para implementar una VM con SQL Server se ha renovado para mejorar la facilidad de uso. Para más información, vea el breve [Inicio rápido](quickstart-sql-vm-create-portal.md) y la guía [paso a paso](virtual-machines-windows-portal-sql-server-provision.md) detallada para implementar una VM con SQL Server.|
| &nbsp; | &nbsp; |


## <a name="february-2019"></a>Febrero de 2019

| Mejoras en el servicio | Detalles |
| --- | --- |
| **Mejora del portal** | Ahora es posible cambiar el modelo de licencias para una VM con SQL Server de pago por uso a traiga su propia licencia mediante [Azure Portal](virtual-machines-windows-sql-ahb.md#change-license-for-vms-already-registered-with-resource-provider)|
|**Simplificación de la implementación de grupos de disponibilidad con la CLI de la máquina virtual de Azure SQL** | Ahora es más fácil que nunca implementar un grupo de disponibilidad en una VM con SQL Server en Azure. La [CLI de la máquina virtual de Azure SQL](/cli/azure/sql/vm?view=azure-cli-2018-03-01-hybrid) le permite crear el cliente de escucha del grupo de disponibilidad, WSFC e ILB desde la línea de comandos y en un tiempo récord. Para obtener más información, consulte [Usar la CLI de máquina virtual de Azure SQL para configurar un grupo de disponibilidad Always On para SQL Server en una máquina virtual de Azure](virtual-machines-windows-sql-availability-group-cli.md). | 
| &nbsp; | &nbsp; |


## <a name="december-2018"></a>Diciembre de 2018

| Mejoras en el servicio | Detalles |
| --- | --- |
| **Nuevo proveedor de recursos del grupo de clústeres SQL** | Un nuevo proveedor de recursos (Microsoft.SqlVirtualMachine/SqlVirtualMachineGroups) que define los metadatos del clúster de conmutación por error de Windows. Al unir una VM con SQL Server a *SqlVirtualMachineGroups*, se arranca el servicio de clúster de conmutación por error de Windows y se une la VM al clúster.  |
|**Automatización de la configuración de implementación de un grupo de disponibilidad con plantillas de inicio rápido de Azure** |Ahora es posible crear el clúster de conmutación por error de Windows, unirlo a las VM con SQL Server, crear el agente de escucha y configurar Load Balancer interno con dos plantillas de inicio rápido de Azure. Para obtener más información, consulte [Usar la plantilla de inicio rápido de Azure para configurar un grupo de disponibilidad Always On para SQL Server en una máquina virtual de Azure](virtual-machines-windows-sql-availability-group-quickstart-template.md). | 
| **Registro automático del proveedor de recursos de VM de SQL** | Las VM con SQL Server implementadas después de este mes se registran automáticamente con el nuevo proveedor de recursos de SQL Server. Las VM con SQL Server implementadas antes de este mes todavía deben registrarse manualmente. Para más información, consulte [Registro de VM con SQL existentes con el proveedor de recursos de VM con SQL](virtual-machines-windows-sql-register-with-resource-provider.md).|
| &nbsp; | &nbsp; |


## <a name="november-2018"></a>Noviembre de 2018

| Mejoras en el servicio | Detalles |
| --- | --- |
| **Nuevo proveedor de recursos de VM con SQL** |  Un nuevo proveedor de recursos para VM con SQL Server (Microsoft.SqlVirtualMachine) que proporciona una mejor administración de la VM con SQL Server. Para más información sobre cómo registrar la máquina virtual, consulte [Registro de máquinas virtuales de SQL existentes con el nuevo proveedor de recursos](virtual-machines-windows-sql-register-with-resource-provider.md). |
|**Cambiar el modelo de licencia** | Aahora puede cambiar entre los modelos de pago por uso y traer su propia licencia para la VM con SQL mediante PowerShell o la CLI de Azure. Para más información, consulte [Modificación del modelo de licencia para una máquina virtual de SQL en Azure](virtual-machines-windows-sql-ahb.md). | 
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
