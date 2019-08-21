---
title: Administración de máquinas virtuales con SQL Server en Azure mediante Azure Portal | Microsoft Docs
description: Aprenda a acceder al recurso de máquina virtual SQL en Azure Portal para una máquina virtual con SQL Server hospedada en Azure.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 05/13/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 398eea4b968bb77017415e1dc259004c697b8dda
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2019
ms.locfileid: "68846177"
---
# <a name="manage-sql-server-vms-in-azure-by-using-the-azure-portal"></a>Administración de máquinas virtuales con SQL Server en Azure mediante Azure Portal

En [Azure Portal](https://portal.azure.com), el recurso **Máquinas virtuales SQL** es un servicio de administración independiente. Puede usarlo para ver todas las máquinas virtuales con SQL Server simultáneamente y modificar la configuración dedicada a SQL Server: 

![Recurso máquinas virtuales SQL](media/virtual-machines-windows-sql-manage-portal/sql-vm-manage.png)


## <a name="remarks"></a>Comentarios

- Se recomienda usar el recurso **Máquinas virtuales SQL** para ver y administrar las máquinas virtuales con SQL Server en Azure. Sin embargo, actualmente, el recurso **Máquinas virtuales SQL** no permite administrar máquinas virtuales con SQL Server con [finalización del soporte técnico](virtual-machines-windows-sql-server-2008-eos-extend-support.md). Para administrar la configuración de las máquinas virtuales con SQL Server con finalización del soporte técnico, utilice en su lugar la [pestaña de configuración de SQL Server](#access-the-sql-server-configuration-tab) en desuso. 
- El recurso **Máquinas virtuales SQL** solo está disponible para las máquinas virtuales con SQL Server que se hayan [registrado con el proveedor de recursos de máquina virtual con SQL Server](virtual-machines-windows-sql-register-with-resource-provider.md). 


## <a name="access-the-sql-virtual-machines-resource"></a>Acceso al recurso Máquinas virtuales SQL
Para acceder al recurso **Máquinas virtuales SQL**, haga lo siguiente:

1. Abra [Azure Portal](https://portal.azure.com). 
1. Seleccione **Todos los servicios**. 
1. Escriba **Máquinas virtuales SQL** en el cuadro de búsqueda.
1. (Optional): Seleccione la estrella que hay junto a **Máquinas virtuales SQL** para agregar esta opción al menú **Favoritos**. 
1. Seleccione **Máquinas virtuales SQL**. 

   ![Búsqueda de máquinas virtuales con SQL Server en todos los servicios](media/virtual-machines-windows-sql-manage-portal/sql-vm-search.png)

1. En el portal se enumeran todas las máquinas virtuales con SQL Server disponibles en la suscripción. Seleccione la que desee administrar para iniciar el recurso **Máquinas virtuales SQL**. Utilice el cuadro de búsqueda si la máquina virtual con SQL Server no aparece. 

   ![Todas las máquinas virtuales con SQL Server disponibles](media/virtual-machines-windows-sql-manage-portal/all-sql-vms.png)

   Al seleccionar la máquina virtual con SQL Server, se abrirá el recurso **Máquinas virtuales SQL**: 


   ![Recurso máquinas virtuales SQL](media/virtual-machines-windows-sql-manage-portal/sql-vm-resource.png)

> [!TIP]
> El recurso **máquinas virtuales de SQL** es para la configuración de SQL Server dedicada. Seleccione el nombre de la máquina virtual en el campo **Máquina virtual** para ir a los valores específicos de la máquina virtual, no a los exclusivos de SQL Server. 

## <a name="access-the-sql-server-configuration-tab"></a>Acceso a la pestaña de configuración de SQL Server
La pestaña **Configuración de SQL Server** ha quedado en desuso. Actualmente, este es el único método para administrar las máquinas virtuales con SQL Server con [finalización del soporte técnico](virtual-machines-windows-sql-server-2008-eos-extend-support.md) y las máquinas virtuales con SQL Server que no se hayan [registrado con el proveedor del recursos de máquina virtual con SQL Server](virtual-machines-windows-sql-register-with-resource-provider.md).

Para acceder a la pestaña **Configuración de SQL Server** en desuso, debe ir al recurso **Máquinas virtuales**. Para ello, siga los pasos que se describen a continuación:

1. Abra [Azure Portal](https://portal.azure.com). 
1. Seleccione **Todos los servicios**. 
1. Escriba **máquinas virtuales** en el cuadro de búsqueda.
1. (Optional): Seleccione la estrella que hay junto a **Máquinas virtuales** para agregar esta opción al menú **Favoritos**. 
1. Seleccione **Máquinas virtuales**. 

   ![Buscar máquinas virtuales](media/virtual-machines-windows-sql-manage-portal/vm-search.png)

1. El portal enumera todas las máquinas virtuales en la suscripción. Seleccione la que desee administrar para abrir el recurso **Máquinas virtuales**. Utilice el cuadro de búsqueda si la máquina virtual con SQL Server no aparece. 
1. Seleccione **Configuración de SQL Server** en el panel **Configuración** para administrar la máquina virtual con SQL Server. 

   ![Configuración de SQL Server](media/virtual-machines-windows-sql-manage-portal/sql-vm-configuration.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los siguientes artículos. 

* [Introducción a SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-overview.md)
* [Preguntas más frecuentes para SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-faq.md)
* [Orientación de precios de SQL Server en una máquina virtual Windows](virtual-machines-windows-sql-server-pricing-guidance.md)
* [Notas de la versión para SQL Server en una máquina virtual Windows](virtual-machines-windows-sql-server-iaas-release-notes.md)


