---
title: Actualización local de la edición de SQL Server en una máquina virtual de Azure | Microsoft Docs
description: Aprenda a cambiar la edición de la máquina virtual de SQL Server en Azure.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: jroth
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 06/26/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: 05d2170fe9e6055179bf49d803d4739ddc05be89
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67607356"
---
# <a name="how-to-perform-an-in-place-upgrade-of-sql-server-edition-on-azure-vm"></a>Actualización local de la edición de SQL Server en una máquina virtual de Azure

En este artículo se describe cómo cambiar la edición de una instancia de SQL Server existente en una máquina virtual Windows en Azure. 

La edición de SQL Server viene determinada por la clave de producto y se especifica durante la instalación. La edición determina qué [características](/sql/sql-server/editions-and-components-of-sql-server-2017) están disponibles dentro del producto SQL Server. Puede cambiar la edición de SQL Server con el soporte de instalación: cambiar a una versión anterior para reducir los costos o actualizar para habilitar más características.

Si ha actualizado la edición de SQL Server con el soporte de instalación después de registrarse con el proveedor de recursos de máquina virtual de SQL, para actualizar la facturación de Azure en consecuencia debe establecer la propiedad de edición de SQL Server del recurso de máquina virtual de SQL como sigue:

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com). 
1. Vaya al recurso de máquina virtual de SQL Server. 
1. En **Configuración**, seleccione **Configurar** y la edición de SQL Server que quiera en la lista desplegable de **Edición**. 

   ![Metadatos del cambio de edición](media/virtual-machines-windows-sql-change-edition/edition-change-in-portal.png)

1. Revise la advertencia que notifica que primero debe cambiar la edición SQL Server y que esa propiedad edition debe coincidir con la edición de SQL Server. 
1. Seleccione **Aplicar** para aplicar los cambios en los metadatos de edición. 


## <a name="prerequisites"></a>Requisitos previos

Para hacer un cambio local de la edición de SQL Server, necesita lo siguiente: 

- Una [suscripción de Azure](https://azure.microsoft.com/free/).
- Una [máquina virtual Windows de SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision) registrada con el [proveedor de recursos de máquina virtual de SQL](virtual-machines-windows-sql-register-with-resource-provider.md).
- Soporte de instalación con la edición deseada de SQL Server. Los clientes que cuenten con [Software Assurance](https://www.microsoft.com/licensing/licensing-programs/software-assurance-default) pueden obtener el soporte de instalación desde el [centro de licencias por volumen](https://www.microsoft.com/Licensing/servicecenter/default.aspx). Los clientes que no tengan Software Assurance pueden usar el soporte de instalación de una imagen de máquina virtual de SQL Server de Marketplace que tenga la edición que deseen.


## <a name="upgrade-edition"></a>Actualización de la edición

  > [!WARNING]
  > - **Al actualizar la edición de SQL Server se reiniciará el servicio para SQL Server y los demás servicios asociados, como Analysis Services y R Services.** 

Para actualizar la edición de SQL Server, obtenga el soporte de instalación de SQL Server para la edición que desee y haga lo siguiente:

1. Ejecute Setup.exe desde el soporte de instalación de SQL Server. 
1. Vaya a **Mantenimiento** y elija la opción **Actualización de edición**. 

   ![Actualización de la edición de SQL Server](media/virtual-machines-windows-sql-change-edition/edition-upgrade.png)

1. Seleccione **Siguiente** hasta llegar a la página **Listo para actualizar la edición** y seleccione **Actualizar**. La ventana de instalación puede bloquearse durante unos minutos mientras se aplica el cambio. A continuación verá una página **Completo** de confirmación de la actualización de edición. 

Una vez actualizada la edición de SQL Server, debe modificar la propiedad "edition" de la máquina virtual de SQL en Azure Portal como se muestra anteriormente; así se actualizarán los metadatos y la facturación asociados a esta máquina virtual.

## <a name="downgrade-edition"></a>Cambio a una edición anterior

  > [!WARNING]
  > - **Para cambiar a una edición anterior de SQL Server se necesita desinstalar totalmente SQL Server, lo que puede suponer tiempo de inactividad adicional**. 


Para cambiar a una edición de SQL Server anterior, deberá desinstalarlo totalmente y volver a instalarlo con el soporte de instalación de la edición deseada. 

Para cambiar la edición de SQL Server a una anterior, siga estos pasos:

1. Realice una copia de seguridad de todas las bases de datos, incluidas del sistema. 
1. Traslade las bases de datos del sistema (master, model y msdb) a una nueva ubicación. 
1. Desinstale completamente todos los servicios asociados y SQL Server. 
1. Reinicie la máquina virtual. 
1. Instale SQL Server con el soporte de instalación con la edición deseada.
1. Instale los últimos Service Pack y las actualizaciones acumulativas.  
1. Reemplace la nuevas bases de datos del sistema que se crearon durante la instalación por las del sistema que trasladó anteriormente a otra ubicación. 

Una vez cambiada la edición de SQL Server a una anterior, debe modificar la propiedad "edition" de la máquina virtual de SQL en Azure Portal como se muestra anteriormente; así se actualizarán los metadatos y la facturación asociados a esta máquina virtual.

## <a name="remarks"></a>Comentarios

 - La propiedad edition de la máquina virtual de SQL Server debe coincidir con la edición instalada en la máquina virtual para todas las máquinas virtuales SQL, incluidos los tipos de licencia Pago por uso y Traiga su propia licencia.
 - Si quita el recurso de máquina virtual con SQL Server, volverá a la configuración de edición codificada de forma rígida de la imagen.
  - La capacidad de cambiar la edición es una característica del proveedor de recursos de máquina virtual de SQL. Al implementarse una imagen de Marketplace de una mediante Azure Portal se registra una máquina virtual con SQL Server con el proveedor de recursos. Sin embargo, los clientes que instalen automáticamente SQL Server deberán [registrar la máquina virtual con SQL Server](virtual-machines-windows-sql-register-with-resource-provider.md) manualmente.
- Para agregar una máquina virtual de SQL Server a un conjunto de disponibilidad se necesita volver a crear la máquina virtual. Por lo tanto, cualquier máquina virtual que se agregue a un conjunto de disponibilidad volverá a la edición predeterminada y esta también deberá volver a modificarse.

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los siguientes artículos. 

* [Introducción a SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-overview.md)
* [Preguntas más frecuentes para SQL Server en máquinas virtuales Windows](virtual-machines-windows-sql-server-iaas-faq.md)
* [Orientación de precios de SQL Server para máquinas virtuales Windows](virtual-machines-windows-sql-server-pricing-guidance.md)
* [SQL Server on a Windows VM release notes](virtual-machines-windows-sql-server-iaas-release-notes.md) (Notas de la versión de SQL Server en máquinas virtuales Windows)


