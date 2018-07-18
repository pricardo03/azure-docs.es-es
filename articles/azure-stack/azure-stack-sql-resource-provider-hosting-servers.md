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
ms.date: 06/27/2018
ms.author: jeffgilb
ms.openlocfilehash: af820f90c5d8822dbdaa768b16360d534fd47828
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37060049"
---
# <a name="add-hosting-servers-for-the-sql-resource-provider"></a>Incorporación de servidores de hospedaje para el proveedor de recursos SQL

Puede hospedar una instancia SQL en una máquina virtual en [Azure Stack](azure-stack-poc.md), o en una máquina virtual fuera de su entorno de Azure Stack, siempre y cuando el proveedor de recursos SQL pueda conectarse a la instancia.

## <a name="overview"></a>Información general

Antes de agregar un servidor de hospedaje SQL, revise los siguientes requisitos obligatorios y generales.

**Requisitos obligatorios**

* Habilite la autenticación de SQL en la instancia de SQL Server. Dado que la máquina virtual del proveedor de recursos SQL no está unida a ningún dominio, solo se puede conectar a un servidor de hospedaje mediante la autenticación de SQL.
* Configure las direcciones IP para las instancias de SQL como Públicas. El proveedor de recursos y los usuarios, como Web Apps, se comunican a través de la red del usuario, por lo que se requiere conectividad a la instancia de SQL en esta red.

**Requisitos generales**

* Dedique la instancia de SQL para que la use el proveedor de recursos y las cargas de trabajo de usuario. No se puede utilizar una instancia de SQL que esté siendo utilizada por otro consumidor. Esta restricción también se aplica a App Services.
* Configure una cuenta con los niveles de privilegios adecuados para que la use el proveedor de recursos.
* Usted es responsable de administrar las instancias de SQL y sus hosts.  Por ejemplo, el proveedor de recursos no aplica actualizaciones, administra copias de seguridad ni controla la rotación de credenciales.

### <a name="sql-server-virtual-machine-images"></a>Imágenes de máquinas virtuales de SQL Server

Hay varias imágenes de máquina virtual IaaS de SQL disponibles en la característica Administración de Marketplace. Estas imágenes son las mismas que las máquinas virtuales de SQL disponibles en Azure.

Asegúrese de descargar siempre la versión más reciente de la **extensión IaaS de SQL** antes de implementar una máquina virtual de SQL mediante un elemento de Marketplace. La extensión de IaaS y las correspondientes mejoras del portal proporcionan características adicionales tales como la copia de seguridad y la aplicación de revisiones. Para obtener más información acerca de esta extensión, consulte [Automatizar las tareas de administración en Azure Virtual Machines con la extensión del Agente SQL Server](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-server-agent-extension).

Hay otras opciones para implementar máquinas virtuales de SQL, incluidas las plantillas de la [Galería de inicio rápido de Azure Stack](https://github.com/Azure/AzureStack-QuickStart-Templates).

> [!NOTE]
> Cualquier servidor de hospedaje instalado en una instancia de Azure Stack de varios nodos debe crearse a partir de una suscripción de usuario. No se pueden crear desde la suscripción de proveedor predeterminado. Se deben crear desde el portal de usuario o desde una sesión de PowerShell con un inicio de sesión adecuado. Todos los servidores de hospedaje son máquinas virtuales facturables y deben tener las licencias de SQL adecuadas. El administrador de servicios _puede_ ser el propietario de dicha suscripción.

### <a name="required-privileges"></a>Privilegios necesarios

Puede crear un usuario administrativo con menos privilegios que un administrador del sistema de SQL. El usuario solo necesita permisos para las operaciones siguientes:

* Base de datos: crear, modificar, con contención (solo para Always On), eliminar, copia de seguridad
* Grupo de disponibilidad: modificar, combinar, agregar o quitar base de datos
* Inicio de sesión: crear, seleccionar, modificar, eliminar, revocar
* Operaciones Select: \[master\].\[sys\].\[availability_group_listeners\] (AlwaysOn), sys.availability_replicas (AlwaysOn), sys.databases, \[master\].\[sys\].\[dm_os_sys_memory\], SERVERPROPERTY, \[master\].\[sys\].\[availability_groups\] (AlwaysOn), sys.master_files

## <a name="provide-capacity-by-connecting-to-a-standalone-hosting-sql-server"></a>Agregar capacidad a través de la conexión a un servidor SQL de hospedaje independiente

Puede usar servidores SQL (que no sean HA) independientes con cualquier edición de SQL Server 2014 o SQL Server 2016. Asegúrese de que tiene las credenciales de una cuenta con privilegios de administrador del sistema.

Para agregar un servidor de hospedaje independiente que ya se haya configurado, siga estos pasos:

1. Inicie sesión en el portal de operador de Azure Stack como administrador de servicios.

2. Seleccione **Examinar** &gt; **RECURSOS ADMINISTRATIVOS** &gt; **SQL Hosting Servers** (Servidores de hospedaje SQL).

   ![Servidores de hospedaje SQL](./media/azure-stack-sql-rp-deploy/sqlhostingservers.png)

   En **SQL Hosting Servers** (Servidores de hospedaje SQL) puede conectar el proveedor de recursos de SQL a instancias de SQL Server que actúan como back-end del proveedor de recursos.

   ![Panel del adaptador de SQL](./media/azure-stack-sql-rp-deploy/sqladapterdashboard.png)

3. En **Add a SQL Hosting Server** (Agregar un servidor de hospedaje SQL), proporcione los detalles de conexión para la instancia de SQL Server.

   ![Agregar un servidor de hospedaje SQL](./media/azure-stack-sql-rp-deploy/sqlrp-newhostingserver.png)

    Tiene la opción de incluir un nombre de instancia y un número de puerto si la instancia no está asignada al puerto predeterminado 1433.

   > [!NOTE]
   > Siempre y cuando el usuario y el administrador en Azure Resource Manager puedan acceder a la instancia de SQL, esta puede ponerse bajo el control del proveedor de recursos. La instancia de SQL __debe__ asignarse exclusivamente al proveedor de recursos.

4. A medida que agregue servidores, debe asignarlos a una SKU existente o crear una nueva. En **Add a Hosting Server** (Agregar un servidor de hospedaje), seleccione **SKU**.

   * Para usar una SKU existente, elija una SKU disponible y, a continuación, seleccione **Crear**.
   * Para crear una SKU, seleccione **+ Create new SKU** (Crear nueva SKU). En **Create SKU** (Crear SKU), introduzca la información necesaria y seleccione **Aceptar**.

     > [!IMPORTANT]
     > Los caracteres especiales, incluidos los espacios y los puntos, no son compatibles con el campo **Nombre**. Use los ejemplos de la siguiente captura de pantalla para introducir valores para los campos **Familia**, **Capa** y **Edición**.

     ![Creación de una SKU](./media/azure-stack-sql-rp-deploy/sqlrp-newsku.png)

      Las SKU pueden tardar hasta una hora en estar visibles en el portal. Los usuarios no pueden crear una base de datos hasta que se cree totalmente la SKU.

### <a name="sku-notes"></a>Notas de la SKU

Puede usar las SKU para diferenciar las ofertas de servicio. Por ejemplo, puede tener una instancia de SQL Enterprise que tenga las siguientes características:
  
* alta capacidad
* alto rendimiento
* alta disponibilidad

Puede crear una SKU para el ejemplo anterior, limitando el acceso a grupos específicos que necesitan una base de datos de alto rendimiento.

>[!TIP]
>Use un nombre de SKU que describa las capacidades de los servidores de la SKU, como la capacidad y el rendimiento. El nombre sirve de ayuda para que los usuarios implementen sus bases de datos en la SKU adecuada.

Como procedimiento recomendado, todos los servidores de hospedaje de una SKU deben tener las mismas características de rendimiento y recursos.

## <a name="provide-high-availability-using-sql-always-on-availability-groups"></a>Alta disponibilidad mediante grupos de disponibilidad SQL Always On

La configuración de instancias de SQL de Always On requiere pasos adicionales e implica tres máquinas virtuales (o máquinas físicas). En este artículo se da por hecho que ya conoce bien los grupos de disponibilidad Always On. Para más información, consulte los siguientes artículos.

* [Introducción a los grupos de disponibilidad Always On de SQL Server en Azure Virtual Machines](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-overview)
* [Grupos de disponibilidad Always On (SQL Server)](https://docs.microsoft.com/en-us/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server?view=sql-server-2017)

> [!NOTE]
> El proveedor de recursos del adaptador SQL _solo_ admite instancias de SQL 2016 SP1 Enterprise o versiones posteriores para Always On. Esta configuración de adaptador requiere nuevas características SQL como la propagación automática.

Además, debe habilitar la opción [Propagación automática](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/automatically-initialize-always-on-availability-group) en cada grupo de disponibilidad para cada instancia de SQL Server.

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
