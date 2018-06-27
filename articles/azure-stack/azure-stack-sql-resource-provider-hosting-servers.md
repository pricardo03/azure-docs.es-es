---
title: Servidores de hospedaje SQL en Azure Stack | Microsoft Docs
description: Procedimientos para agregar instancias SQL para el aprovisionamiento mediante el proveedor de recursos de adaptador de SQL.
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
ms.date: 06/18/2018
ms.author: jeffgilb
ms.openlocfilehash: 183d9479ae18e557b00d0867cad79600145da7bd
ms.sourcegitcommit: 301855e018cfa1984198e045872539f04ce0e707
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36265235"
---
# <a name="add-hosting-servers-for-the-sql-resource-provider"></a>Incorporación de servidores de hospedaje para el proveedor de recursos SQL

Puede hospedar una instancia SQL en una máquina virtual en [Azure Stack](azure-stack-poc.md), o en una máquina virtual fuera de su entorno de Azure Stack, siempre y cuando el proveedor de recursos SQL pueda conectarse a la instancia.

## <a name="overview"></a>Información general

Los requisitos generales para los servidores de hospedaje de SQL son:

* La instancia de SQL debe ser de uso exclusivo para el proveedor de recursos y las cargas de trabajo de usuario. No se puede utilizar una instancia de SQL que esté siendo utilizada por otro consumidor. Esta restricción también se aplica a App Services.
* La máquina virtual del proveedor de recursos SQL no está unida al dominio y solo se puede conectar mediante la autenticación de SQL.
* Debe configurar una cuenta con los privilegios adecuados para que la use el proveedor de recursos.
* El proveedor de recursos y los usuarios, como Web Apps, utilizan la red del usuario, por lo que se requiere conectividad a la instancia SQL en esta red. Este requisito normalmente significa que la dirección IP para las instancias de SQL debe estar en una red pública.
* La administración de las instancias de SQL y sus hosts es responsabilidad suya. Por ejemplo, el proveedor de recursos no aplica actualizaciones, administra copias de seguridad ni controla la rotación de credenciales.
* Puede usar SKU que admitan distintas clases de capacidades de SQL, como el rendimiento y la alta disponibilidad con grupos de disponibilidad AlwaysOn.

### <a name="sql-server-virtual-machine-images"></a>Imágenes de máquinas virtuales de SQL Server

Hay varias imágenes de máquina virtual IaaS de SQL disponibles en la característica Administración de Marketplace. Estas imágenes son las mismas que las máquinas virtuales de SQL disponibles en Azure.

Asegúrese de descargar siempre la versión más reciente de la **extensión IaaS de SQL** antes de implementar una máquina virtual mediante un elemento de Marketplace.  La extensión de IaaS y las correspondientes mejoras del portal proporcionan características adicionales tales como la copia de seguridad y la aplicación de revisiones.

Hay otras opciones para implementar máquinas virtuales de SQL, incluidas las plantillas de la [Galería de inicio rápido de Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates).

> [!NOTE]
> Cualquier servidor de hospedaje instalado en una instancia de Azure Stack de varios nodos debe crearse a partir de una suscripción de usuario. No se pueden crear desde la suscripción de proveedor predeterminado. Se deben crear desde el portal de usuario o desde una sesión de PowerShell con un inicio de sesión adecuado. Todos los servidores de hospedaje son máquinas virtuales facturables y deben tener las licencias de SQL adecuadas. El administrador de servicios _puede_ ser el propietario de dicha suscripción.

### <a name="required-privileges"></a>Privilegios necesarios

Puede crear un usuario administrativo con menos privilegios que un administrador del sistema de SQL. El usuario solo necesita permisos para las operaciones siguientes:

- Base de datos: crear, modificar, con contención (solo para Always On), eliminar, copia de seguridad
- Grupo de disponibilidad: modificar, combinar, agregar o quitar base de datos
- Inicio de sesión: crear, seleccionar, modificar, eliminar, revocar
- Operaciones Select: \[master\].\[sys\].\[availability_group_listeners\] (AlwaysOn), sys.availability_replicas (AlwaysOn), sys.databases, \[master\].\[sys\].\[dm_os_sys_memory\], SERVERPROPERTY, \[master\].\[sys\].\[availability_groups\] (AlwaysOn), sys.master_files

## <a name="provide-capacity-by-connecting-to-a-standalone-hosting-sql-server"></a>Agregar capacidad a través de la conexión a un servidor SQL de hospedaje independiente

Puede usar servidores SQL (que no sean HA) independientes con cualquier edición de SQL Server 2014 o SQL Server 2016. Asegúrese de que tiene las credenciales de una cuenta con privilegios de administrador del sistema.

Para agregar un servidor de hospedaje independiente que ya se haya configurado, siga estos pasos:

1. Inicie sesión en el portal de operador de Azure Stack como administrador de servicios.

2. Seleccione **Examinar** &gt; **RECURSOS ADMINISTRATIVOS** &gt; **SQL Hosting Servers** (Servidores de hospedaje SQL).

   ![Servidores de hospedaje SQL](./media/azure-stack-sql-rp-deploy/sqlhostingservers.png)

   En **SQL Hosting Servers** (Servidores de hospedaje SQL) puede conectar el proveedor de recursos de SQL a instancias de SQL Server que actúan como back-end del proveedor de recursos.

   ![Panel del adaptador de SQL](./media/azure-stack-sql-rp-deploy/sqladapterdashboard.png)

3. Rellene el formulario con los detalles de conexión de la instancia de SQL Server.

   ![Agregar un servidor de hospedaje SQL](./media/azure-stack-sql-rp-deploy/sqlrp-newhostingserver.png)

    Tiene la opción de incluir un nombre de instancia y un número de puerto si la instancia no está asignada al puerto predeterminado 1433.

   > [!NOTE]
   > Siempre y cuando el usuario y el administrador en Azure Resource Manager puedan acceder a la instancia de SQL, esta puede ponerse bajo el control del proveedor de recursos. La instancia de SQL __debe__ asignarse exclusivamente al proveedor de recursos.

4. A medida que agregue servidores, debe asignarlos a una SKU nueva o existente para diferenciar las ofertas de servicio. Por ejemplo, puede tener una instancia de SQL Enterprise que proporciona:
  
   - funcionalidad de base de datos
   - copia de seguridad automática
   - reserva de servidores de alto rendimiento para departamentos individuales

   Todos los servidores de hospedaje en una SKU deben tener las mismas funcionalidades. El valor de **Nombre** debe reflejar las propiedades de la SKU para que los usuarios puedan implementar sus bases de datos en el SKU adecuado.

   > [!IMPORTANT]
   > No se admiten caracteres especiales, espacios y puntos incluidos, en los nombres de **familia** o **capa** al crear una SKU para los proveedores de recursos SQL y MySQL.

   Por ejemplo: 

   ![Crear SKU](./media/azure-stack-sql-rp-deploy/sqlrp-newsku.png)

   >[!NOTE]
   > Las SKU pueden tardar hasta una hora en estar visibles en el portal. Los usuarios no pueden crear una base de datos hasta que se cree totalmente la SKU.

## <a name="provide-high-availability-using-sql-always-on-availability-groups"></a>Alta disponibilidad mediante grupos de disponibilidad SQL Always On

La configuración de instancias de SQL Always On requiere pasos adicionales e implica al menos tres máquinas virtuales (o máquinas físicas). En este artículo se da por hecho que ya conoce bien los grupos de disponibilidad Always On. Para más información, consulte:

* [Introducción a los grupos de disponibilidad Always On de SQL Server en Azure Virtual Machines](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview)
* [Grupos de disponibilidad Always On (SQL Server)](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server?view=sql-server-2017)

> [!NOTE]
> El proveedor de recursos del adaptador SQL _solo_ admite instancias de SQL 2016 SP1 Enterprise o versiones posteriores para Always On. Esta configuración de adaptador requiere nuevas características SQL como la propagación automática.

Además de la lista de requisitos anterior, debe habilitar la [propagación automática](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group) en cada grupo de disponibilidad para cada instancia de SQL Server.

Para habilitar la propagación automática en todas las instancias, edite y ejecute el siguiente comando SQL para cada instancia:

  ```
  ALTER AVAILABILITY GROUP [<availability_group_name>]
      MODIFY REPLICA ON 'InstanceName'
      WITH (SEEDING_MODE = AUTOMATIC)
  GO
  ```

En las instancias secundarias, edite y ejecute el siguiente comando SQL para cada instancia:

  ```
  ALTER AVAILABILITY GROUP [<availability_group_name>] GRANT CREATE ANY DATABASE
  GO
  ```

### <a name="to-add-sql-always-on-hosting-servers"></a>Para agregar servidores de hospedaje SQL Always On

1. Inicie sesión en el portal de administración de Azure Stack como administrador de servicios.

2. Seleccione **Examinar** &gt; **RECURSOS ADMINISTRATIVOS** &gt; **SQL Hosting Servers** (Servidores de hospedaje SQL) &gt; **+Agregar**.

   En **SQL Hosting Servers** (Servidores de hospedaje SQL), puede conectar el proveedor de recursos del servidor SQL a instancias reales de SQL Server que actúan como back-end del proveedor de recursos.

3. Rellene el formulario con los detalles de conexión de la instancia de SQL Server. Asegúrese de que usa la dirección completa del agente de escucha Always On (y número de puerto opcional). Proporcione la información de la cuenta que ha configurado con privilegios de administrador del sistema.

4. Active la casilla Grupos de disponibilidad Always On para habilitar la compatibilidad con instancias de grupos de disponibilidad SQL Always On.

   ![Habilitar Always On](./media/azure-stack-sql-rp-deploy/AlwaysOn.PNG)

5. Agregue la instancia de SQL Always On a una SKU.

   > [!IMPORTANT]
   > No puede mezclar servidores independientes con instancias de Always On en la misma SKU. Si se intentan combinar tipos después de agregar los primeros resultados del servidor de hospedaje, se produce un error.

## <a name="make-the-sql-databases-available-to-users"></a>Poner las bases de datos SQL a disposición de los usuarios

Cree planes y ofertas para poner las bases de datos SQL a disposición de los usuarios. Agregue el servicio **Microsoft.SqlAdapter** al plan y agregue la cuota predeterminada o cree una nueva.

![Crear planes y ofertas para incluir las bases de datos](./media/azure-stack-sql-rp-deploy/sqlrp-newplan.png)

## <a name="next-steps"></a>Pasos siguientes

[Agregar bases de datos](azure-stack-sql-resource-provider-databases.md)
