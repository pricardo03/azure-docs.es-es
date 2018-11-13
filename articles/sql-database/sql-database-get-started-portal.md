---
title: 'Azure Portal: Creación de una base de datos SQL | Microsoft Docs'
description: Cree un servidor lógico de SQL Database, una regla de firewall de nivel de servidor y una base de datos en Azure Portal y realice consultas en ella.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: carlrab
manager: craigg
ms.date: 11/01/2018
ms.openlocfilehash: 66ee4ac8fe946696d6760891a086a672fa9fc412
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "50914608"
---
# <a name="quickstart-create-an-azure-sql-database-in-the-azure-portal"></a>Guía de inicio rápido: Creación de una instancia de Azure SQL Database en Azure Portal

Esta guía de inicio rápido le guía por el proceso de creación de una base de datos SQL en Azure mediante el [método de compra basado en DTU](sql-database-service-tiers-dtu.md). Azure SQL Database es una oferta de "base de datos como servicio" que permite ejecutar y escalar bases de datos de SQL Server de alta disponibilidad en la nube. En este inicio rápido se muestra cómo comenzar mediante la creación y la posterior consulta de una base de datos SQL a través de Azure Portal.

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

  >[!NOTE]
  >Este tutorial usa el modelo de compra basado en DTU, pero también está disponible el [modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md).

## <a name="log-in-to-the-azure-portal"></a>Iniciar sesión en Azure Portal

Inicie sesión en el [Azure Portal](https://portal.azure.com/).

## <a name="create-a-sql-database"></a>Creación de una base de datos SQL

Se crea una instancia de Azure SQL Database con un conjunto definido de [recursos de proceso y almacenamiento](sql-database-service-tiers-dtu.md). La base de datos se crea dentro de un [grupo de recursos de Azure](../azure-resource-manager/resource-group-overview.md) y en un [servidor lógico de Azure SQL Database](sql-database-features.md).

Siga estos pasos para crear una base de datos SQL que contenga los datos de ejemplo de Adventure Works LT.

1. Haga clic en **Crear un recurso** en la esquina superior izquierda de Azure Portal.

2. Seleccione **Bases de datos** en la página **Nuevo** y seleccione **Crear** en **SQL Database** en la página **Nuevo**.

   ![create database-1](./media/sql-database-get-started-portal/create-database-1.png)

3. Rellene el formulario de SQL Database con la siguiente información, como se muestra en la imagen anterior:   

   | Configuración       | Valor sugerido | DESCRIPCIÓN |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Nombre de la base de datos** | mySampleDatabase | Para conocer los nombres de base de datos válidos, consulte [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) (Identificadores de base de datos). |
   | **Suscripción** | Su suscripción  | Para más información acerca de sus suscripciones, consulte [Suscripciones](https://account.windowsazure.com/Subscriptions). |
   | **Grupos de recursos**  | myResourceGroup | Para conocer cuáles son los nombres de grupo de recursos válidos, consulte el artículo [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Convenciones de nomenclatura). |
   | **Seleccionar origen** | Ejemplo (AdventureWorksLT) | Carga el esquema y los datos de AdventureWorksLT en la base de datos nueva |

   > [!IMPORTANT]
   > Debe seleccionar la base de datos de ejemplo de este formulario porque se utiliza en el resto de esta guía de inicio rápido.
   >

4. En **Servidor**, haga clic en **Configurar los valores obligatorios** y rellene el formulario de SQL Server (servidor lógico) con la siguiente información, como se muestra en la imagen siguiente:   

   | Configuración       | Valor sugerido | DESCRIPCIÓN |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Nombre del servidor** | Cualquier nombre globalmente único | Para conocer cuáles son los nombres de servidor válidos, consulte el artículo [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Convenciones de nomenclatura). |
   | **Inicio de sesión del administrador del servidor** | Cualquier nombre válido | Para conocer los nombres de inicio de sesión válidos, consulte [Database Identifiers](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers) (Identificadores de base de datos). |
   | **Contraseña** | Cualquier contraseña válida | La contraseña debe tener un mínimo de 8 caracteres y debe contener caracteres de tres de las siguientes categorías: caracteres en mayúsculas, caracteres en minúsculas, números y caracteres no alfanuméricos. |
   | **Suscripción** | Su suscripción | Para más información acerca de sus suscripciones, consulte [Suscripciones](https://account.windowsazure.com/Subscriptions). |
   | **Grupos de recursos** | myResourceGroup | Para conocer cuáles son los nombres de grupo de recursos válidos, consulte el artículo [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Convenciones de nomenclatura). |
   | **Ubicación** | Cualquier ubicación válida | Para obtener información acerca de las regiones, consulte [Regiones de Azure](https://azure.microsoft.com/regions/). |

   > [!IMPORTANT]
   > El inicio de sesión y la contraseña de administrador del servidor que especifique aquí serán necesarios para iniciar sesión más adelante en ese servidor y en las bases de datos que se especificarán en esta guía de inicio rápido. Recuerde o grabe esta información para su uso posterior.
   >  

   ![create database-server](./media/sql-database-get-started-portal/create-database-server.png)

5. Cuando haya completado el formulario, haga clic en **Seleccionar**.

6. Haga clic en **Plan de tarifa** para especificar el nivel de servicio, el número de DTU y la cantidad de almacenamiento. Explore las opciones de cantidad de almacenamiento y de DTU que están a su disposición para cada nivel de servicio.

   > [!IMPORTANT]
   > Hay más de 1 TB de almacenamiento en el nivel Premium disponible actualmente en todas las regiones excepto en las siguientes: Norte de Reino Unido, Centro-oeste de EE. UU., Sur de Reino Unido 2, Este de China, US DoD (centro), Centro de Alemania, US DoD (este), US Gov (suroeste), US Gov (centro-sur), Nordeste de Alemania, Norte de China y US Gov (este). En otras regiones, el almacenamiento máximo del nivel Premium está limitado a 1 TB. Consulte [Limitaciones actuales P11-P15]( sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

7. Para esta guía de inicio rápido, seleccione el nivel de servicio **Estándar** y utilice el control deslizante para seleccionar **10 DTU (S0)** y **1** GB de almacenamiento.

   ![create database-s1](./media/sql-database-get-started-portal/create-database-s1.png)

8. Acepte los términos de la versión preliminar para usar la opción de **almacenamiento de complementos**.

   > [!IMPORTANT]
   > Hay más de 1 TB de almacenamiento en el nivel Premium disponible actualmente en todas las regiones, excepto en las siguientes: Centro-oeste de EE. UU., Este de China, USDoD (centro), USGov Iowa, Centro de Alemania, USDoD (este), US Gov (suroeste), Nordeste de Alemania, Norte de China. En otras regiones, el almacenamiento máximo del nivel Premium está limitado a 1 TB. Consulte [Limitaciones actuales P11-P15]( sql-database-dtu-resource-limits-single-databases.md#single-database-limitations-of-p11-and-p15-when-the-maximum-size-greater-than-1-tb).  

9. Después de seleccionar el nivel del servidor, el número de DTU y la cantidad de almacenamiento, haga clic en **Aplicar**.  

10. Una vez completado el formulario de SQL Database, haga clic en **Crear** para aprovisionar la base de datos. El aprovisionamiento tarda unos minutos.

11. En la barra de herramientas, haga clic en **Notificaciones** para supervisar el proceso de implementación.

     ![notificación](./media/sql-database-get-started-portal/notification.png)

## <a name="query-the-sql-database"></a>Consulta a SQL Database

Ahora que ha creado una base de datos de ejemplo en Azure, vamos a usar la herramienta de consulta integrada en Azure Portal para confirmar que puede conectarse a la base de datos y consultar los datos.

1. En la barra de herramientas de la página SQL Database de la base de datos, haga clic en **Editor de consultas (versión preliminar)** y en **Iniciar sesión**.

   ![inicio de sesión](./media/sql-database-get-started-portal/query-editor-login.png)

2. Seleccione la autenticación de SQL Server, proporcione la información de inicio de sesión necesaria y, haga clic en **Aceptar** para iniciar sesión.

3. Una vez autenticado como **ServerAdmin**, escriba la siguiente consulta en el panel del editor de consultas.

   ```sql
   SELECT TOP 20 pc.Name as CategoryName, p.name as ProductName
   FROM SalesLT.ProductCategory pc
   JOIN SalesLT.Product p
   ON pc.productcategoryid = p.productcategoryid;
   ```

4. Haga clic en **Ejecución** y revise los resultados de la consulta en el panel **Resultados**.

   ![resultados del editor de consultas](./media/sql-database-get-started-portal/query-editor-results.png)

5. Cierre la página **Editor de consultas** y haga clic en **Aceptar** para descartar los cambios que no se han guardado.

## <a name="clean-up-resources"></a>Limpieza de recursos

Guarde estos recursos si desea volver a la sección [Pasos siguientes](#next-steps) y aprender a conectarse y consultar la base de datos mediante una serie de métodos diferentes. Sin embargo, si desea eliminar los recursos que ha creado en esta guía de inicio rápido, siga estos pasos.


1. En el menú izquierdo de Azure Portal, haga clic en **Grupos de recursos** y en **myResourceGroup**.
2. En la página del grupo de recursos, haga clic en **Eliminar**, escriba **myResourceGroup** en el cuadro de texto y haga clic en **Eliminar**.

## <a name="next-steps"></a>Pasos siguientes

- Ahora que tiene una base de datos, debe crear una regla de firewall de nivel de servidor para conectarse a ella desde las herramientas locales. Vea [Create server-level firewall rule](sql-database-get-started-portal-firewall.md) (Crear regla de firewall de nivel de servidor)
- Si crea una regla de firewall de nivel de servidor, puede [conectarse y consultar](sql-database-connect-query.md) mediante una de sus herramientas preferidas o lenguajes, lo que incluye
  - [Conectar y consultar con SQL Server Management Studio](sql-database-connect-query-ssms.md)
  - [Conectar y consultar con Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Para crear bases de datos mediante la CLI de Azure, vea [Ejemplos de la CLI de Azure](sql-database-cli-samples.md)
- Para crear bases de datos mediante Azure PowerShell, vea [Ejemplos de Azure PowerShell](sql-database-powershell-samples.md)