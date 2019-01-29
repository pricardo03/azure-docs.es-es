---
title: 'Azure Portal: Creación de una Instancia administrada de SQL | Microsoft Docs'
description: Cree una instancia administrada de SQL, un entorno de red y una máquina virtual cliente para el acceso.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: Carlrab
manager: craigg
ms.date: 01/15/2019
ms.openlocfilehash: a4e829f8ae89815c68d80a9f85dfbfd58a03b6a0
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/22/2019
ms.locfileid: "54452197"
---
# <a name="quickstart-create-an-azure-sql-database-managed-instance"></a>Guía de inicio rápido: Creación de una Instancia administrada de Azure SQL Database

Este inicio rápido le guía por la creación de una [Instancia administrada](sql-database-managed-instance.md) de Azure SQL Database en Azure Portal.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="create-a-managed-instance"></a>Creación de una instancia administrada

En los pasos siguientes se muestra cómo crear una Instancia administrada.

1. Elija **Crear un recurso** en la esquina superior izquierda de Azure Portal.
2. Busque **Instancia administrada** y, después, seleccione **Instancia administrada de Azure SQL**.
3. Seleccione **Crear**.

   ![Crear una instancia administrada](./media/sql-database-managed-instance-get-started/managed-instance-create.png)

4. Rellene el formulario **Instancia administrada** con la información solicitada, para lo que debe usar los datos de la siguiente tabla:

   | Configuración| Valor sugerido | DESCRIPCIÓN |
   | ------ | --------------- | ----------- |
   | **Suscripción** | Su suscripción | Una suscripción en la que tiene permiso para crear recursos. |
   |**Nombre de la instancia administrada**|Cualquier nombre válido|Para conocer cuáles son los nombres válidos, consulte el artículo [Convenciones de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Inicio de sesión de administrador de la instancia administrada**|Cualquier nombre de usuario válido|Para conocer cuáles son los nombres válidos, consulte el artículo [Convenciones de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). No utilice "serveradmin", ya es un rol de nivel de servidor reservado.|
   |**Contraseña**|Cualquier contraseña válida|La contraseña debe tener al menos 16 caracteres de largo y cumplir con los [requisitos de complejidad definidos](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).|
   |**Intercalación**|La intercalación que desea usar para una instancia administrada|Si va a migrar bases de datos desde SQL Server, compruebe la intercalación de origen mediante `SELECT SERVERPROPERTY(N'Collation')` y use ese valor. Para información sobre las intercalaciones, consulte [Intercalaciones de nivel de servidor](https://docs.microsoft.com/sql/relational-databases/collations/set-or-change-the-server-collation).|
   |**Ubicación**|La ubicación en la que quiere crear la Instancia administrada.|Para obtener información acerca de las regiones, consulte [Regiones de Azure](https://azure.microsoft.com/regions/).|
   |**Red virtual**|Seleccione **Crear una nueva red virtual** o una subred y red virtual válidas.| Si hay una red o subred que aparece deshabilitada, se debe [modificar para cumplir con los requisitos de la red](sql-database-managed-instance-configure-vnet-subnet.md) antes de seleccionarla como destino de la nueva Instancia administrada. Para obtener información acerca los requisitos necesarios para configurar el entorno de red de Instancia administrada, consulte [Configuración de una red virtual para Instancia administrada de Azure SQL Database](sql-database-managed-instance-connectivity-architecture.md). |
   |**Grupos de recursos**|Un grupo de recursos nuevo o existente.|Para conocer cuáles son los nombres de grupo de recursos válidos, consulte el artículo [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Convenciones de nomenclatura).|

   ![Formulario de la instancia administrada](./media/sql-database-managed-instance-get-started/managed-instance-create-form.png)

5. Para utilizar la instancia administrada como un grupo de conmutación por error de la instancia secundario, seleccione la restauración y especifique la instancia administrada de DnsAzurePartner. Esta característica está en versión preliminar y no se muestra en la captura de pantalla que lo acompaña.
6. Seleccione **Plan de tarifa** para cambiar el tamaño de los recursos de almacenamiento y de proceso, así como para revisar las opciones del plan de tarifa. El plan de tarifa de uso general con 32 GB de memoria y 16 núcleos de virtuales es el valor predeterminado.
7. Utilice los controles deslizantes o cuadros de texto para especificar la cantidad de almacenamiento y el número de núcleos virtuales.
8. Cuando haya terminado, elija **Aplicar** para guardar la selección.  
9. Haga clic en **Crear** para implementar Instancia administrada.
10. Seleccione el icono **Notificaciones** para ver el estado de la implementación.

    ![Progreso de la implementación de la Instancia administrada](./media/sql-database-managed-instance-get-started/deployment-progress.png)

11. Seleccione **Deployment in progress** (Implementación en curso) para abrir la ventana de Instancia administrada y supervisar el progreso de la implementación.

> [!IMPORTANT]
> En la primera instancia de una subred el tiempo de implementación habitualmente es mucho mayor que en las siguientes. No cancele la operación de implementación porque dure más de lo previsto. La creación de la segunda Instancia administrada en la subred solo tarda un par de minutos.

## <a name="review-resources-and-retrieve-your-fully-qualified-server-name"></a>Revisión de los recursos y recuperación del nombre del servidor completo

Cuando la implementación finalice correctamente, revise los recursos creados y recupere el nombre completo del servidor para su uso en tutoriales posteriores.

1. Abra el grupo de recursos de la Instancia administrada y vea los recursos que se crearon de forma automática en el inicio rápido [Creación de una Instancia administrada](#create-a-managed-instance).

2. Seleccione una Instancia administrada.

   ![Recursos de la Instancia administrada](./media/sql-database-managed-instance-get-started/resources.png)

3. En el pestaña **Información general**, busque la propiedad **Host** y copie la dirección de host completa de la Instancia administrada.

   ![Recursos de la Instancia administrada](./media/sql-database-managed-instance-get-started/host-name.png)

   El nombre es similar **nombre_del_equipo.a1b2c3d4e5f6.database.windows.net**.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre cómo conectarse a una Instancia administrada, vea:
  - Para obtener información general de las opciones de conexión para las aplicaciones, vea [Conexión de la aplicación a Instancia administrada de Azure SQL Database](sql-database-managed-instance-connect-app.md).
  - Para obtener una guía de inicio rápido en la que se muestra cómo conectarse a una instancia administrada desde una máquina virtual de Azure, vea [Configure an Azure virtual machine connection](sql-database-managed-instance-configure-vm.md) (Configuración de una conexión de máquina virtual de Azure).
  - Para obtener una guía de inicio rápido en la que se muestra cómo conectarse a una Instancia administrada desde un equipo cliente local mediante una conexión de punto a sitio, vea [Configure a point-to-site connection](sql-database-managed-instance-configure-p2s.md) (Configuración de una conexión de punto a sitio).
- Para restaurar una base de datos SQL Server existente desde el entorno local en una Instancia administrada, puede usar [Azure Database Migration Service (DMS) para la migración](../dms/tutorial-sql-server-to-managed-instance.md) a fin de restaurar desde un archivo de copia de seguridad de base de datos, o bien el [comando T-SQL RESTORE](sql-database-managed-instance-get-started-restore.md) para restaurar desde un archivo de copia de seguridad de base de datos.
- Para una supervisión avanzada del rendimiento de la base de datos de Instancia administrada con inteligencia de solución de problemas integrada, consulte [Supervisión de instancias de Azure SQL Database con Azure SQL Analytics ](../azure-monitor/insights/azure-sql.md)
