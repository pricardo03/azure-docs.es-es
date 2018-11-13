---
title: 'Azure Portal: creación de una instancia administrada de SQL | Microsoft Docs'
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
ms.date: 11/01/2018
ms.openlocfilehash: 3eadc2d233fd1716716c323f4c7087ee8363c67c
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "50912329"
---
# <a name="quickstart-create-an-azure-sql-database-managed-instance"></a>Guía de inicio rápido: Creación de una Instancia administrada de Azure SQL Database

Este inicio rápido le guía por la creación de una [Instancia administrada](sql-database-managed-instance.md) de Azure SQL Database en Azure Portal. 

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="create-a-managed-instance"></a>Creación de una instancia administrada

En los pasos siguientes se muestra cómo crear una Instancia administrada.

1. Haga clic en **Crear un recurso** en la esquina superior izquierda de Azure Portal.
2. Busque **Instancia administrada** y, después, seleccione **Instancia administrada de Azure SQL**.
3. Haga clic en **Create**(Crear).

   ![Crear una instancia administrada](./media/sql-database-managed-instance-get-started/managed-instance-create.png)

4. Rellene el formulario de la instancia administrada con la información solicitada, utilizando los datos de la siguiente tabla:

   | Configuración| Valor sugerido | DESCRIPCIÓN |
   | ------ | --------------- | ----------- |
   | **Suscripción** | Su suscripción | Una suscripción en la que tiene permiso para crear recursos. |
   |**Nombre de la instancia administrada**|Cualquier nombre válido|Para conocer cuáles son los nombres válidos, consulte el artículo [Convenciones de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).|
   |**Inicio de sesión de administrador de la instancia administrada**|Cualquier nombre de usuario válido|Para conocer cuáles son los nombres válidos, consulte el artículo [Convenciones de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). No utilice "serveradmin", ya es un rol de nivel de servidor reservado.| 
   |**Contraseña**|Cualquier contraseña válida|La contraseña debe tener al menos 16 caracteres de largo y cumplir con los [requisitos de complejidad definidos](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm).|
   |**Grupo de recursos**|Un grupo de recursos nuevo o existente.|Para conocer cuáles son los nombres de grupo de recursos válidos, consulte el artículo [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Convenciones de nomenclatura).|
   |**Ubicación**|La ubicación en la que quiere crear la Instancia administrada.|Para obtener información acerca de las regiones, consulte [Regiones de Azure](https://azure.microsoft.com/regions/).|
   |**Red virtual**|Haga clic en **Crear una nueva red virtual** o seleccione una red virtual que haya creado anteriormente en el grupo de recursos que proporcionó antes en este formulario.| Para configurar una red virtual para una Instancia administrada con una configuración personalizada, vea [Configure SQL Managed Instance virtual network environment template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-sql-managed-instance-azure-environment) (Configuración de la plantilla de entorno de red virtual de Instancia administrada de SQL) en Github. Para obtener información sobre los requisitos para configurar el entorno de red para una Instancia administrada, vea [Configuración de una red virtual para Instancia administrada de Azure SQL Database](sql-database-managed-instance-vnet-configuration.md). |

   ![Formulario de la instancia administrada](./media/sql-database-managed-instance-get-started/managed-instance-create-form.png)

5. Haga clic en **Plan de tarifa** para cambiar el tamaño de los recursos de almacenamiento y de proceso, así como para revisar las opciones del plan de tarifa. El plan de tarifa de uso general con 32 GB de memoria y 16 núcleos de virtuales es el valor predeterminado.
6. Utilice los controles deslizantes o cuadros de texto para especificar la cantidad de almacenamiento y el número de núcleos virtuales. 
7. Cuando haya terminado, haga clic en **Aplicar** para guardar la selección.  
8. Haga clic en **Crear** para implementar la instancia administrada.
9. Haga clic en el icono de **notificaciones** para ver el estado de la implementación.

    ![Progreso de la implementación de la Instancia administrada](./media/sql-database-managed-instance-get-started/deployment-progress.png)

10. Haga clic en **Deployment in progress** (Implementación en curso) para abrir la ventana de la instancia administrada y seguir la supervisión del progreso de la implementación. 

> [!IMPORTANT]
> Para la primera instancia de una subred, el tiempo de implementación es típicamente mucho mayor que en las instancias siguientes. No cancele la operación de implementación porque dure más de lo previsto. La creación de la segunda Instancia administrada en la subred solo tarda un par de minutos.

## <a name="review-resources-and-retrieve-your-fully-qualified-server-name"></a>Revisión de los recursos y recuperación del nombre del servidor completo

Cuando la implementación finalice correctamente, revise los recursos creados y recupere el nombre completo del servidor para su uso en tutoriales posteriores.

1. Abra el grupo de recursos de la Instancia administrada y vea los recursos que se crearon de forma automática en el inicio rápido [Creación de una Instancia administrada](sql-database-managed-instance-get-started.md).

   ![Recursos de la Instancia administrada](./media/sql-database-managed-instance-get-started/resources.png)Abra el recurso de Instancia administrada en Azure Portal.

2. Haga clic en la Instancia administrada.
3. En el pestaña **Información general**, busque la propiedad **Host** y copie la dirección de host completa para la Instancia administrada.


   ![Recursos de la Instancia administrada](./media/sql-database-managed-instance-get-started/host-name.png)

   El nombre es similar a este: **nombre_del_equipo.neu15011648751ff.database.windows.net**.

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre cómo conectarse a una Instancia administrada, vea:
  - Para obtener información general de las opciones de conexión para las aplicaciones, vea [Conexión de la aplicación a Instancia administrada de Azure SQL Database](sql-database-managed-instance-connect-app.md).
  - Para obtener una guía de inicio rápido en la que se muestra cómo conectarse a una instancia administrada desde una máquina virtual de Azure, vea [Configure an Azure virtual machine connection](sql-database-managed-instance-configure-vm.md) (Configuración de una conexión de máquina virtual de Azure).
  - Para obtener una guía de inicio rápido en la que se muestra cómo conectarse a una Instancia administrada desde un equipo cliente local mediante una conexión de punto a sitio, vea [Configure a point-to-site connection](sql-database-managed-instance-configure-p2s.md) (Configuración de una conexión de punto a sitio).
- Para restaurar una base de datos SQL Server existente desde el entorno local en una Instancia administrada, puede usar [Azure Database Migration Service (DMS) para la migración](../dms/tutorial-sql-server-to-managed-instance.md) a fin de restaurar desde un archivo de copia de seguridad de base de datos, o bien el [comando T-SQL RESTORE](sql-database-managed-instance-get-started-restore.md) para restaurar desde un archivo de copia de seguridad de base de datos.
- Para una supervisión avanzada del rendimiento de la base de datos de Instancia administrada con inteligencia de solución de problemas integrada, consulte [Supervisión de instancias de Azure SQL Database con Azure SQL Analytics ](../log-analytics/log-analytics-azure-sql.md)
