---
title: Administración de máquinas virtuales con SQL Server en Azure mediante Azure Portal | Microsoft Docs
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
ms.openlocfilehash: 59a85e855c9fab9f2a3437c83c867b8076f55049
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67607222"
---
# <a name="manage-sql-server-vms-in-azure-using-the-azure-portal"></a>Administración de máquinas virtuales con SQL Server en Azure mediante Azure Portal

Hay un nuevo punto de acceso para administrar su VM con SQL Server en Azure mediante [Azure Portal](https://portal.azure.com). 

El recurso **máquinas virtuales SQL** ahora es un servicio de administración independiente que permite ver todas las máquinas de virtuales con SQL Server al mismo tiempo y modificar la configuración dedicada a SQL Server: 

![Recurso máquinas virtuales SQL](media/virtual-machines-windows-sql-manage-portal/sql-vm-manage.png)


## <a name="remarks"></a>Comentarios

- El recurso **máquinas virtuales SQL** es el método recomendado para ver y administrar máquinas virtuales con SQL Server. Sin embargo, actualmente, el recurso **máquinas virtuales de SQL** no admite la administración de [fin del soporte técnico (EOS)](virtual-machines-windows-sql-server-2008-eos-extend-support.md) las máquinas virtuales con SQL Server. Para administrar el EOS de configuración de las máquinas virtuales con SQL Server, utilice la [pestaña de configuración de SQL Server](#access-sql-server-configuration-tab) en desuso. 
- El **máquinas virtuales de SQL** recursos solo está disponible para las máquinas virtuales con SQL Server que se hayan [registrado en el proveedor de recursos de VM SQL](virtual-machines-windows-sql-register-with-resource-provider.md). 


## <a name="access-sql-virtual-machine-resource"></a>Acceso al recurso máquinas virtuales SQL
Para acceder al recurso máquinas virtuales SQL, siga estos pasos:

1. Abra [Azure Portal](https://portal.azure.com). 
1. Seleccione **Todos los servicios**. 
1. Escriba `SQL virtual machines` en el cuadro de búsqueda.
1. (Optional): Seleccione la estrella que hay junto a las **máquinas virtuales SQL** para agregar esta opción al menú Favoritos. 
1. Seleccione **Máquinas virtuales SQL**. 

   ![Buscar máquinas virtuales SQL en todos los servicios](media/virtual-machines-windows-sql-manage-portal/sql-vm-search.png)

1. De esta forma se enumeran todas las máquinas virtuales con SQL Server disponibles en la suscripción. Seleccione la que desea administrar para iniciar el recurso **máquinas virtuales de SQL**. Utilice el cuadro de búsqueda si la VM con SQL Server no es evidente. 

![Todas las máquinas virtuales SQL disponibles](media/virtual-machines-windows-sql-manage-portal/all-sql-vms.png)

Al seleccionar la máquina virtual con SQL Server se abrirá el recurso **máquinas virtuales SQL**: 


![Recurso máquinas virtuales SQL](media/virtual-machines-windows-sql-manage-portal/sql-vm-resource.png)

  > [!TIP]
  > El recurso **máquinas virtuales de SQL** es para la configuración de SQL Server dedicada. Seleccione el nombre de la máquina virtual en el campo **Máquina Virtual** para ir a los valores específicos de la máquina virtual, no a los exclusivos de SQL Server. 

## <a name="access-sql-server-configuration-tab"></a>Acceso a la pestaña de configuración de SQL Server
La pestaña de configuración de SQL Server ha quedado en desuso. Actualmente, este es el único método para administrar el [fin del soporte técnico (EOS)](virtual-machines-windows-sql-server-2008-eos-extend-support.md) de las máquinas virtuales con SQL Server y las máquinas virtuales con SQL Server que no se han [registrado en el proveedor de recursos de máquinas virtuales SQL VM](virtual-machines-windows-sql-register-with-resource-provider.md).

Para acceder a la pestaña de configuración SQL Server en desuso, debe ir al recurso **Máquinas virtuales**. Para ello, haga lo siguiente:

1. Abra [Azure Portal](https://portal.azure.com). 
1. Seleccione **Todos los servicios**. 
1. Escriba `virtual machines` en el cuadro de búsqueda.
1. (Optional): Seleccione la estrella que hay junto a **Máquinas virtuales** para agregar esta opción al menú Favoritos. 
1. Seleccione **Máquinas virtuales**. 

   ![Buscar máquinas virtuales](media/virtual-machines-windows-sql-manage-portal/vm-search.png)

1. Se enumerarán todas las máquinas virtuales de la suscripción. Seleccione la que desea administrar para iniciar el recurso **Máquinas virtuales**. Utilice el cuadro de búsqueda si la VM con SQL Server no es evidente. 
1. Seleccione **Configuración de SQL Server** en el panel **Configuración** para administrar su servidor SQL Server. 

![Configuración de SQL Server](media/virtual-machines-windows-sql-manage-portal/sql-vm-configuration.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los siguientes artículos. 

* [Introducción a SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-overview.md)
* [Preguntas más frecuentes para SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-faq.md)
* [Orientación de precios de SQL Server para máquinas virtuales Windows](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server on a Windows VM release notes](virtual-machines-windows-sql-server-iaas-release-notes.md) (Notas de la versión de SQL Server en máquinas virtuales Windows)


