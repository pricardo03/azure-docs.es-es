---
author: MightyPen
ms.service: sql-database
ms.topic: include
ms.date: 12/10/2018
ms.author: genemi
ms.openlocfilehash: ab31ee82e8035fe888fa70b5796aef2c2b2939b2
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/21/2018
ms.locfileid: "53728568"
---
## <a name="sign-in-to-the-azure-portal"></a>Inicio de sesión en Azure Portal

Inicie sesión en el [Portal de Azure](https://portal.azure.com/)

## <a name="create-a-blank-sql-database"></a>Crear una instancia en blanco en SQL Database

Hay una instancia de Azure SQL Database con un conjunto definido de [recursos de proceso y almacenamiento](../articles/sql-database/sql-database-service-tiers-dtu.md). La base de datos se encuentra dentro de un [grupo de recursos de Azure](../articles/azure-resource-manager/resource-group-overview.md) y de un [servidor lógico de Azure SQL Database](../articles/sql-database/sql-database-features.md).

Siga estos pasos para crear una instancia en blanco de SQL Database.

1. Haga clic en **Crear un recurso** en la esquina superior izquierda de Azure Portal.

1. En la página **Nuevo**, haga clic en **Bases de datos** > **SQL Database**.

   ![crear una base de datos en blanco](../articles/sql-database/media/sql-database-design-first-database/create-empty-database.png)

1. En el panel **SQL Database**, escriba o seleccione los valores siguientes:

   | Configuración       | Valor sugerido | DESCRIPCIÓN |
   | ------------ | ------------------ | ------------------------------------------------- |
   | **Nombre de la base de datos** | *yourDatabase* | Para conocer los nombres de base de datos válidos, consulte [Identificadores de base de datos](/sql/relational-databases/databases/database-identifiers). |
   | **Suscripción** | *yourSubscription*  | Para más información acerca de sus suscripciones, consulte [Suscripciones](https://account.windowsazure.com/Subscriptions). |
   | **Grupos de recursos** | *yourResourceGroup* | Para conocer cuáles son los nombres de grupo de recursos válidos, consulte el artículo [Naming conventions](/azure/architecture/best-practices/naming-conventions) (Convenciones de nomenclatura). |
   | **Seleccionar origen** | Base de datos en blanco | Especifica que se debe crear una base de datos en blanco. |

   ![crear base de datos](../articles/sql-database/media/sql-database-design-first-database/create-database.png)

   1. Seleccione **Servidor** con el fin de configurar un servidor para la nueva base de datos. Luego, escriba o seleccione los valores siguientes:

      | Configuración       | Valor sugerido | DESCRIPCIÓN |
      | ------------ | ------------------ | ------------------------------------------------- |
      | **Nombre del servidor** | Cualquier nombre globalmente único | Para conocer cuáles son los nombres de servidor válidos, consulte el artículo [Naming conventions](/azure/architecture/best-practices/naming-conventions) (Convenciones de nomenclatura). |
      | **Inicio de sesión del administrador del servidor** | Cualquier nombre válido | Para conocer los nombres de inicio de sesión válidos, consulte [Identificadores de base de datos](/sql/relational-databases/databases/database-identifiers).|
      | **Contraseña** | Cualquier contraseña válida | La contraseña debe tener un mínimo de 8 caracteres y usar caracteres de tres de las siguientes categorías: caracteres en mayúsculas, caracteres en minúsculas, números y caracteres no alfanuméricos. |
      | **Ubicación** | Cualquier ubicación válida | Para obtener información sobre las regiones, consulte [Regiones de Azure](https://azure.microsoft.com/regions/). |

      Elija **Seleccionar**.

      ![create database-server](../articles/sql-database/media/sql-database-design-first-database/create-database-server.png)

   1. Seleccione **Plan de tarifa** para especificar el nivel de servicio, el número de DTU y la cantidad de almacenamiento. Explore las opciones de almacenamiento y de DTU que están a su disposición para cada nivel de servicio.

      Después de seleccionar el nivel del servidor, el número de DTU y la cantidad de almacenamiento, seleccione **Aplicar**.

   1. Introduzca una **intercalación** para la base de datos en blanco (para este tutorial, use el valor predeterminado). Para obtener más información sobre las intercalaciones, vea [COLLATE (Transact-SQL)](/sql/t-sql/statements/collations).

1. Una vez completado el formulario de **SQL Database**, seleccione **Crear** para crear la base de datos. Este paso puede tardar hasta un minuto y medio en completarse.

1. En la barra de herramientas, haga clic en **Notificaciones** para supervisar el proceso de implementación.

     ![notificación](../articles/sql-database/media/sql-database-design-first-database/notification.png)

## <a name="create-a-firewall-rule"></a>Creación de una regla de firewall

El servicio SQL Database crea un firewall en el nivel de servidor, lo que impide que herramientas y aplicaciones externas se conecten al servidor o a las bases de datos del servidor. Siga estos pasos para crear una [regla de firewall de nivel de servidor de SQL Database](../articles/sql-database/sql-database-firewall-configure.md) para la dirección IP del cliente. Este proceso permite la conectividad externa a través del firewall de SQL Database solo para su dirección IP.

> [!NOTE]
> SQL Database se comunica a través del puerto 1433. Si intenta conectarse desde dentro de una red corporativa, es posible que el firewall de la red no permita el tráfico de salida a través del puerto 1433. En ese caso, no puede conectarse al servidor de Azure SQL Database, salvo que el administrador abra el puerto 1433.

1. Cuando se haya finalizado la implementación, seleccione **Bases de datos SQL** en el menú de la izquierda y, después, elija *yourDatabase* en la página **Bases de datos SQL**. Se abre la página de **información general** de la base de datos, que muestra el nombre del servidor completo (por ejemplo, *suservidor.database.windows.net*) y proporciona opciones para otras configuraciones.

1. Copie este nombre del servidor completo para conectarse al servidor y a sus bases de datos en los pasos posteriores.

   ![nombre del servidor](../articles/sql-database/media/sql-database-design-first-database/server-name.png)

1. Seleccione **Establecer el firewall del servidor** en la barra de herramientas. Se abrirá la página **Configuración del firewall** del servidor de SQL Database.

   ![regla de firewall del servidor](../articles/sql-database/media/sql-database-design-first-database/server-firewall-rule.png)

   1. Seleccione **Agregar IP de cliente** en la barra de herramientas para agregar la dirección IP actual a la nueva regla de firewall. La regla de firewall puede abrir el puerto 1433 para una única dirección IP o un intervalo de direcciones IP.

   1. Elija **Guardar**. Se crea una regla de firewall de nivel de servidor para el puerto 1433 de la dirección IP actual en el servidor lógico.

   1. Seleccione **Aceptar** y después cierre la página **Configuración de firewall**.

Su dirección IP ahora puede atravesar el firewall y conectarse al servidor de SQL Database y sus bases de datos mediante SSMS u otra herramienta que elija. Asegúrese de usar la cuenta de administrador del servidor que creó anteriormente.

> [!IMPORTANT]
> De forma predeterminada, el acceso a través del firewall de SQL Database está habilitado para todos los servicios de Azure. Seleccione **DESACTIVAR** en esta página para deshabilitar todos los servicios de Azure.
