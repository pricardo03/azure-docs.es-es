---
title: Configuración de los registros de servidor y acceso a ellos en Azure Database for MariaDB en Azure Portal
description: En este artículo se describe cómo configurar los registros de servidor de Azure Database for MariaDB, y acceder a ellos, mediante Azure Portal.
author: rachel-msft
ms.author: raagyema
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/11/2019
ms.openlocfilehash: 3dbf7064e409230916668e62ef861c0ce149fdbb
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67065636"
---
# <a name="configure-and-access-server-logs-in-the-azure-portal"></a>Configuración y acceso a los registros de servidor en Azure Portal

Se pueden configurar, enumerar y descargar [registros de consulta lentos de Azure Database for MariaDB](concepts-server-logs.md) desde Azure Portal.

## <a name="prerequisites"></a>Requisitos previos
Para seguir esta guía, necesitará:
- [Servidor de Azure Database for MariaDB](quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="configure-logging"></a>registro
Configure el acceso al registro de consultas lentas. 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).

2. Seleccione un servidor de Azure Database for MariaDB.

3. En la sección **Supervisión** de la barra lateral, seleccione **Registros de servidor**. 
   ![Seleccione Registros de servidor y haga clic para configurarlos](./media/howto-configure-server-logs-portal/1-select-server-logs-configure.png)

4. Seleccione el título **Haga clic aquí para habilitar los registros y configurar los parámetros** para ver los parámetros del servidor.

5. Cambie los parámetros que quiera ajustar, y cambie el estado de "slow_query_log" a "ON". Todos los cambios que realice en esta sesión se resaltan en color púrpura. 

   Cuando haya cambiado los parámetros, haga clic en **Guardar**. O bien puede **descartar** los cambios.

   ![Haga clic en Guardar o en Descartar](./media/howto-configure-server-logs-portal/3-save-discard.png)

6. Para volver a la lista de registros, haga clic en el **botón Cerrar** (icono de X) de la página **Parámetros de servidor**.

## <a name="view-list-and-download-logs"></a>Visualización de lista y descarga de registros
Una vez que comience el registro, puede ver una lista de registros de consultas lentas disponibles y descargar archivos de registro individuales en el panel Registros de servidor. 

1. Abra Azure Portal.

2. Seleccione un servidor de Azure Database for MariaDB.

3. En la sección **Supervisión** de la barra lateral, seleccione **Registros de servidor**. En la página se muestra una lista de los archivos de registro de esta forma:

   ![Lista de registros](./media/howto-configure-server-logs-portal/4-server-logs-list.png)

   > [!TIP]
   > La convención de nomenclatura del registro es **mysql-slow-< nombre de su servidor>-yyyymmddhh.log**. La fecha y hora que se utilizan en el nombre de archivo es el momento en que se emitió el registro. Los archivos de registros se rotan cada 24 horas o 7;5 GB,;lo que ocurra primero.

4. Si es necesario, utilice el **cuadro de búsqueda** para reducir rápidamente a un registro específico en función de la fecha y hora. La búsqueda se encuentra en el nombre del registro.

5. Descargue los archivos de registro individuales con el botón de **descarga** (icono de flecha hacia abajo) que hay junto a cada archivo de registro en la fila de tabla, como se muestra:

   ![Haga clic en el icono de descarga](./media/howto-configure-server-logs-portal/5-download.png)

## <a name="next-steps"></a>Pasos siguientes
- Más información sobre los [registros de consultas lentas](concepts-server-logs.md) en Azure Database for MariaDB.
- Para obtener más información sobre las definiciones de parámetros y el registro, consulte la documentación de MariaDB sobre [registros](https://mariadb.com/kb/en/library/slow-query-log-overview/).

<!--- See [Access Server Logs in CLI](howto-configure-server-logs-in-cli.md) to learn how to download logs programmatically. -->
