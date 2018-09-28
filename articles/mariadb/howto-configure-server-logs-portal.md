---
title: Configuración y acceso a los registros de servidor de Azure Database for MariaDB en Azure Portal
description: En este artículo se describe cómo configurar los registros de servidor de Azure Database for MariaDB, y acceder a ellos, mediante Azure Portal.
author: rachel-msft
ms.author: raagyema
editor: jasonwhowell
services: mariadb
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 0784dffc48aeddc854aa540cba24f1834cd41827
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46952841"
---
# <a name="configure-and-access-server-logs-in-the-azure-portal"></a>Configuración y acceso a los registros de servidor en Azure Portal

Los [registros de servidor de Azure Database for MariaDB](concepts-server-logs.md) se pueden configurar, enumerar y descargar desde Azure Portal.

## <a name="prerequisites"></a>Requisitos previos
Para seguir esta guía, necesitará:
- [Servidor de Azure Database for MariaDB](quickstart-create-mariadb-server-database-using-azure-portal.md)

## <a name="configure-logging"></a>registro
Configure el acceso al registro de consultas lentas. 

1. Inicie sesión en el [Azure Portal](http://portal.azure.com/).

2. Seleccione un servidor de Azure Database for MariaDB.

3. En la sección **Supervisión** de la barra lateral, seleccione **Registros de servidor**. 
   ![Seleccione Registros de servidor y haga clic para configurarlos](./media/howto-configure-server-logs-portal/1-select-server-logs-configure.png)

4. Seleccione el título **Haga clic aquí para habilitar los registros y configurar los parámetros** para ver los parámetros del servidor.

5. Cambie los parámetros que quiera ajustar, y cambie el estado de "slow_query_log" a "ON". Todos los cambios que realice en esta sesión se resaltan en color púrpura. 

   Cuando haya cambiado los parámetros, haga clic en **Guardar**. O bien puede **descartar** los cambios.

   ![Haga clic en Guardar o en Descartar](./media/howto-configure-server-logs-portal/3-save-discard.png)

6. Para volver a la lista de registros, haga clic en el **botón Cerrar** (icono de X) de la página **Parámetros de servidor**.

## <a name="view-list-and-download-logs"></a>Visualización de lista y descarga de registros
Una vez que comience el registro, puede ver una lista de registros disponibles y descargar archivos de registro individuales en el panel Registros de servidor. 

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
- Obtenga más información sobre los [registros del servidor](concepts-server-logs.md) en Azure Database for MariaDB.
- Para obtener más información sobre las definiciones de parámetros y el registro, consulte la documentación de MariaDB sobre [registros](https://mariadb.com/kb/en/library/slow-query-log-overview/).

<!-- - See [Access Server Logs in CLI](howto-configure-server-logs-in-cli.md) to learn how to download logs programmatically. -->