---
title: 'Azure Portal: Creación de una base de datos única en Azure SQL Database | Microsoft Docs'
description: Crea y consulta una base de datos única en Azure SQL Database con Azure Portal.
services: sql-database
ms.service: sql-database
ms.subservice: single-database
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: carlrab
manager: craigg
ms.date: 02/25/2019
ms.openlocfilehash: 5aeb84e5086fb0cf5c30e175ad419ee70bed55ad
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58075192"
---
# <a name="quickstart-create-a-single-database-in-azure-sql-database-using-the-azure-portal"></a>Inicio rápido: Creación de una base de datos única en Azure SQL Database con Azure Portal

La creación de una [base de datos única](sql-database-single-database.md) es la opción de implementación más rápida y sencilla para crear una base de datos en Azure SQL Database. En esta guía de inicio rápido se muestra cómo crear, para su posterior consulta, una base de datos única mediante Azure Portal.

Si no tiene una suscripción a Azure, [cree una cuenta gratuita](https://azure.microsoft.com/free/).

Para seguir todos los pasos de este artículo de inicio rápido debe iniciar sesión en [Azure Portal](https://portal.azure.com/).

## <a name="create-a-single-database"></a>Creación de una base de datos única

Una base de datos única tiene un conjunto definido de recursos de proceso, memoria, E/S y almacenamiento y se usa uno de los dos [modelos de compra](sql-database-purchase-models.md). Cuando se crea una base de datos única, también se define un [servidor de SQL Database](sql-database-servers.md) para administrarla y colocarla dentro de un [grupo de recursos de Azure](../azure-resource-manager/resource-group-overview.md) de una región determinada.

Para crear una base de datos única que contenga los datos de ejemplo de AdventureWorksLT:

1. Haga clic en **Crear un recurso** en la esquina superior izquierda de Azure Portal.
2. Seleccione **Bases de datos** y, luego, **SQL Database**.
3. En el formulario **Crear base de datos SQL**, escriba o seleccione los siguientes valores:

   - **Nombre de base de datos**: Escriba *mySampleDatabase*.
   - **Suscripción**: Abra la lista desplegable y seleccione la suscripción correcta, en caso de que no aparezca.
   - **Grupo de recursos**: Seleccione **Crear nuevo**, escriba *myResourceGroup* y seleccione **Aceptar**.
   - **Seleccionar origen**: Abra la lista desplegable y seleccione **Sample (AdventureWorksLT)** [Ejemplo (AdventureWorksLT)].

     > [!IMPORTANT]
     > Asegúrese de seleccionar los datos de **Sample (AdventureWorksLT)** para poder seguir fácilmente este y otros inicios rápidos de Azure SQL Database que usan estos datos.
  
   ![Creación de una base de datos única](./media/sql-database-get-started-portal/create-database-1.png)

4. En **Servidor**, seleccione **Crear nuevo**.
5. En el formulario **Nuevo servidor** , escriba o seleccione los siguientes valores:

   - **Nombre del servidor**: Escriba *mysqlserver*.
   - **Inicio de sesión del administrador del servidor**: escriba *azureuser*.
   - **Contraseña**: Escriba *Azure1234567*.
   - **Confirmar contraseña**: vuelva a escribir la contraseña.
   - **Ubicación**: abra la lista desplegable y seleccione cualquier ubicación válida.  

   > [!IMPORTANT]
   > No olvide registrar el inicio de sesión y la contraseña del administrador del servidor, con el fin de poder iniciar sesión en el servidor y las bases de datos en este y otros inicios rápidos. Si olvida la contraseña o el inicio de sesión, puede obtener el nombre de inicio de sesión o restablecer la contraseña en la página de **SQL server**. Para abrir la página de **SQL server**, seleccione el nombre del servidor en la página de **información general** de la base de datos después de la creación de esta.

    ![Crear un servidor](./media/sql-database-get-started-portal/create-database-server.png)

6. Elija **Seleccionar**.
7. En el formulario **SQL Database**, seleccione **Plan de tarifa**. Explore la cantidad de almacenamiento y de DTU disponibles para cada nivel de servicio.

   > [!NOTE]
   > Este artículo de inicio rápido usa el [modelo de compra basado en DTU](sql-database-service-tiers-vcore.md), pero también está disponible el [modelo de compra basado en núcleos virtuales](sql-database-service-tiers-dtu.md).
   > [!IMPORTANT]
   > Existe más de 1 TB de almacenamiento en el nivel Premium actualmente disponible en todas las regiones excepto: Este de China, Norte de China, Centro de Alemania, Nordeste de Alemania, Centro-oeste de EE. UU., US regiones de US DoD y Centro de Gobierno de EE. UU. En estas regiones, el almacenamiento máximo en el nivel Prémium está limitado a 1 TB.  Para más información, consulte las [limitaciones actuales de P11 y P15](sql-database-single-database-scale.md#dtu-based-purchasing-model-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

8. Para este artículo de inicio rápido, seleccione el nivel de servicio **Estándar** y utilice el control deslizante para seleccionar **10 DTU (S0)** y **1** GB de almacenamiento.
9. Seleccione **Aplicar**.  

   ![Seleccionar los precios](./media/sql-database-get-started-portal/create-database-s1.png)

10. En el formulario **SQL Database**, seleccione **Crear** para implementar y aprovisionar el grupo de recursos, el servidor y la base de datos.

    La implementación tarda unos minutos. Para supervisar el proceso de implementación, puede seleccionar **Notificaciones** en la barra de herramientas.

    ![Notificación](./media/sql-database-get-started-portal/notification.png)

## <a name="query-the-database"></a>Consulta de la base de datos

Ahora que ha creado la base de datos, use la herramienta de consulta integrada en Azure Portal para conectarse a la base de datos y realizar consultas en los datos.

1. En la página **SQL Database** de su base de datos, seleccione **Editor de consultas (versión preliminar)** en el menú de la izquierda.

   ![Inicio de sesión en el Editor de consultas](./media/sql-database-get-started-portal/query-editor-login.png)

2. Escriba su información de inicio de sesión y seleccione **Aceptar**.
3. Escriba la siguiente consulta en el panel del **Editor de consultas**.

   ```sql
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

4. Seleccione **Ejecutar** y examine los resultados de la consulta en el panel **Resultados**.

   ![Resultados del Editor de consultas](./media/sql-database-get-started-portal/query-editor-results.png)

5. Cierre la página **Editor de consultas** y seleccione **Aceptar** cuando se le pida que descarte los cambios que no se hayan guardado.

## <a name="clean-up-resources"></a>Limpieza de recursos

Mantenga este grupo de recursos, el servidor de bases de datos y la base de datos única si quiere ir a [Pasos siguientes](#next-steps). Los pasos siguientes muestran cómo conectarse a la base de datos y consultarla mediante distintos métodos.

Cuando haya terminado con estos recursos, puede eliminarlos como se indica a continuación:

1. En el menú izquierdo de Azure Portal, seleccione **Grupos de recursos** y, después, **myResourceGroup**.
2. En la página del grupo de recursos, seleccione **Eliminar grupo de recursos**.
3. Escriba *myResourceGroup* en el campo y, después, seleccione **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

- Cree una regla de firewall de nivel de servidor para conectarse a la base de datos única desde herramientas locales o remotas. Para más información, consulte [Creación de una regla de firewall de nivel de servidor](sql-database-server-level-firewall-rule.md).
- Después de crear una regla de firewall de nivel de servidor, puede usar varias herramientas y lenguajes para [conectarse a la base de datos y realizar consultas en ella](sql-database-connect-query.md).
  - [Conexión y consulta con SQL Server Management Studio](sql-database-connect-query-ssms.md)
  - [Conexión y consulta de datos con Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Para crear una base de datos única mediante la CLI de Azure, consulte [Ejemplos de la CLI de Azure](sql-database-cli-samples.md).
- Para crear una base de datos única mediante Azure PowerShell, consulte [Ejemplos de Azure PowerShell](sql-database-powershell-samples.md).
