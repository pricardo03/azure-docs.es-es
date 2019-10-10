---
title: Configuración de los registros de servidor y acceso a ellos en Azure Database for MariaDB en Azure Portal
description: En este artículo se describe cómo configurar los registros de servidor de Azure Database for MariaDB, y acceder a ellos, mediante Azure Portal.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 09/30/2019
ms.openlocfilehash: c8be9519d3393330b3022fadd2de6a49e58ecdcf
ms.sourcegitcommit: 6fe40d080bd1561286093b488609590ba355c261
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2019
ms.locfileid: "71703513"
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

## <a name="set-up-diagnostic-logs"></a>Configuración de registros de diagnósticos

1. En la sección **Supervisión** de la barra lateral, seleccione **Configuración de diagnóstico**.

1. Haga clic en "+ Agregar configuración de diagnóstico" ![Agregar configuración de diagnóstico](./media/howto-configure-server-logs-portal/add-diagnostic-setting.png)

1. Proporcione un nombre de configuración de diagnóstico.

1. Especifique a qué receptores de datos se enviarán los registros de consultas lentas (cuenta de almacenamiento, centro de eventos y área de trabajo de Log Analytics).

1. Seleccione "MySqlSlowLogs" como tipo de registro.
![Configurar las opciones de diagnóstico](./media/howto-configure-server-logs-portal/configure-diagnostic-setting.png)

1. Una vez que haya configurado los receptores de datos a los que canalizar los registros de consultas lentas, puede hacer clic en **Guardar**.
![Guardar la configuración de diagnóstico](./media/howto-configure-server-logs-portal/save-diagnostic-setting.png)

1. Acceda a los registros de consultas lentas explorándolos en los receptores de datos que configuró. Los registros pueden tardar hasta 10 minutos en aparecer.

## <a name="next-steps"></a>Pasos siguientes
- Consulte el artículo de [configuración y acceso a los registros de consultas lentas con la CLI de Azure](howto-configure-server-logs-cli.md) para más información sobre cómo descargar registros de consultas lentas mediante programación.
- Más información sobre los [registros de consultas lentas](concepts-server-logs.md) en Azure Database for MariaDB.
- Para obtener más información sobre las definiciones de parámetros y el registro, consulte la documentación de MariaDB sobre [registros](https://mariadb.com/kb/en/library/slow-query-log-overview/).