---
title: Ampliar la compatibilidad de SQL Server 2008 y SQL Server 2008 R2 con Azure
description: Obtenga información sobre cómo ampliar la compatibilidad de SQL Server 2008 y SQL Server 2008 R2 al migrar su instancia de SQL Server a Azure o mediante la compra de compatibilidad ampliada para mantener las instancias locales.
services: virtual-machines-windows
documentationcenter: ''
author: MashaMSFT
manager: craigg
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/08/2019
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: b16fecd6db1f4bed319c832795a2252f8de96c6c
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2019
ms.locfileid: "67607133"
---
# <a name="extend-support-for-sql-server-2008-and-sql-server-2008-r2-with-azure"></a>Ampliar la compatibilidad de SQL Server 2008 y SQL Server 2008 R2 con Azure

SQL Server 2008 y SQL Server 2008 R2 están llegando el [fin de ciclo de vida de soporte (EOS)](https://www.microsoft.com/sql-server/sql-server-2008). Dado que muchos de nuestros clientes siguen usando ambas versiones, ofrecemos varias opciones para seguir recibiendo soporte técnico. Puede migrar las instancias de SQL Server locales a máquinas virtuales (VM) de Azure, migrar a Azure SQL Database o mantener el entorno local y comprar actualizaciones de seguridad ampliadas.

A diferencia de una instancia administrada, la migración a una VM de Azure no requiere la recertificación de sus aplicaciones. Además, a diferencia de mantener un entorno local, al migrar a una VM de Azure recibirá revisiones de seguridad ampliadas gratuitas.

En el resto de este artículo se proporcionan consideraciones sobre la migración de la instancia de SQL Server a una VM de Azure.

## <a name="provisioning"></a>Aprovisionamiento

En Azure Marketplace se proporciona una imagen de pago por uso `SQL Server 2008 R2 on Windows Server 2008 R2`.

Los clientes que usan SQL Server 2008 deberán realizar la autoinstalación o actualizar a SQL Server 2008 R2. Del mismo modo, los clientes que usan Windows Server 2008 deberán implementar su VM desde un disco duro virtual personalizado o actualizar a Windows Server 2008 R2.

Las imágenes implementadas a través de Marketplace tienen la extensión SQL IaaS preinstalada. La extensión SQL IaaS es un requisito para las licencias flexibles y la aplicación de revisiones automatizada. Los clientes que implementen VM autoinstaladas deberán instalar manualmente la extensión SQL IaaS. La extensión SQL IaaS no se admite en Windows 2008.

  > [!NOTE]
  > Si bien el servidor SQL Server `Create` y las hojas `Manage` funcionará con la imagen de SQL Server 2008 R2 en Azure Portal, _no se admiten_ las siguientes características: Copias de seguridad automáticas, integración de Azure Key Vault, R Services y configuración de almacenamiento.

## <a name="licensing"></a>Licencias
Las implementaciones de SQL Server 2008 R2 de pago por uso se pueden convertir en [Ventaja híbrida de Azure (AHB)](https://azure.microsoft.com/pricing/hybrid-benefit/).

Para convertir una licencia basada en Software Assurance (SA) a una licencia de pago por uso, los clientes deben registrarse en el [proveedor de recursos](virtual-machines-windows-sql-register-with-resource-provider.md) de la VM de SQL. Una vez que se hayan registrado en el proveedor de recursos de la VM de SQL, el tipo de licencia de SQL será intercambiable entre AHB y pago por uso.

Las instancias de SQL Server 2008 o SQL Server 2008 R2 autoinstaladas en la VM de Azure se pueden registrar en el proveedor de recursos de SQL y convertir su tipo de licencia a pago por uso.

## <a name="migration"></a>Migración
Puede migrar instancias de EOS SQL Server a una VM de Azure con métodos de copia de seguridad y restauración manuales. Este es el método de migración más común desde un entorno local a una VM de Azure.

### <a name="azure-site-recovery"></a>Azure Site Recovery

Para las migraciones masivas, se recomienda usar el servicio [Azure Site Recovery](/azure/site-recovery/site-recovery-overview). Con Azure Site Recovery, los clientes pueden replicar la VM completa, incluido SQL Server desde un entorno local a una VM de Azure.

SQL Server requiere instantáneas de Azure Site Recovery coherentes con la aplicación para garantizar la recuperación, y Azure Site Recovery admite instantáneas coherentes con la aplicación con un intervalo mínimo de 1 hora. El RPO mínimo posible para SQL Server con las migraciones de Azure Site Recovery es 1 hora y el RTO es de 2 horas más el tiempo de recuperación de SQL Server.

### <a name="database-migration-service"></a>Database Migration Service

[Database Migration Service](/azure/dms/dms-overview) es una opción para los clientes a la hora de migrar desde un entorno local a una VM de Azure mediante la actualización de SQL Server a SQL Server 2012 y versiones posteriores.

## <a name="disaster-recovery"></a>Recuperación ante desastres

Las soluciones de recuperación ante desastres para EOS SQL Server en las VM de Azure son las siguientes:

- **Copias de seguridad de SQL Server**: use Azure Backup para proteger el servidor EOS SQL Server contra ransomware, la eliminación accidental y daños. Actualmente, la solución se ofrece como versión preliminar para EOS SQL Server y admite SQL Server 2008 y 2008 R2 que se ejecutan en Windows 2008 R2 SP1. Consulte [este](https://docs.microsoft.com/azure/backup/backup-azure-sql-database#support-for-sql-server-2008-and-sql-server-2008-r2) artículo para más detalles.
- **Trasvase de registros**: puede crear una réplica de trasvase de registros en otra zona o región de Azure con restauraciones continuas para reducir el RTO. Los clientes deberán configurar manualmente el trasvase de registros.
- **Azure Site Recovery**: Puede replicar la VM entre zonas y regiones a través de la replicación de Azure Site Recovery. SQL Server necesita instantáneas coherentes con la aplicación para garantizar la recuperación en caso de desastre. Azure Site Recovery ofrece un RPO mínimo de 1 hora y dos horas + un RTO de 2 horas de recuperación de SQL Server para la recuperación ante desastres de EOS SQL Server.

## <a name="security-patching"></a>Aplicación de revisiones de seguridad
Las actualizaciones de seguridad ampliadas para las VM de SQL Server se entregarán a través de los canales de Microsoft Update cuando la VM de SQL Server se haya registrado en el [proveedor de recursos](virtual-machines-windows-sql-register-with-resource-provider.md) de SQL. Las revisiones se pueden descargar manual o automáticamente.

**Automated patching** está habilitada de forma predeterminada. La aplicación de revisiones automatizada permite a Azure aplicar automáticamente las revisiones de SQL Server y del sistema operativo. Puede especificar un día de la semana, una hora y una duración para una ventana de mantenimiento si está instalada la extensión de SQL IaaS. Azure realiza la aplicación de revisión en esta ventana de mantenimiento. La programación de la ventana de mantenimiento utiliza la configuración regional de la máquina virtual para la hora.  Para más información, consulte [Automated Patching para SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-automated-patching.md).


## <a name="next-steps"></a>Pasos siguientes

Migración de la VM de SQL Server a Azure

* [Migración de una base de datos SQL Server a SQL Server en una VM de Azure](virtual-machines-windows-migrate-sql.md)

Introducción a SQL Server en máquinas virtuales de Azure:

* [Creación de una máquina virtual con SQL Server en Azure Portal](quickstart-sql-vm-create-portal.md)

Obtenga respuestas a las preguntas más habituales acerca de las máquinas virtuales con SQL:

* [Preguntas más frecuentes sobre SQL Server en Azure Virtual Machines](virtual-machines-windows-sql-server-iaas-faq.md)
