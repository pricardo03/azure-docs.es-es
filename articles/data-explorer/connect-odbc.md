---
title: Conectar con el Explorador de datos de Azure con ODBC
description: En este tema de procedimientos, aprenda a configurar una conexión ODBC con el Explorador de datos de Azure, a continuación, usar esa conexión para visualizar datos con una plantilla.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 02/21/2019
ms.openlocfilehash: d01c825e50e30e3545a0d47e432835c658d677af
ms.sourcegitcommit: 8313d5bf28fb32e8531cdd4a3054065fa7315bfd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/05/2019
ms.locfileid: "59043888"
---
# <a name="connect-to-azure-data-explorer-with-odbc"></a>Conectar con el Explorador de datos de Azure con ODBC

Abre la conectividad de base de datos ([ODBC](/sql/odbc/reference/odbc-overview)) es una interfaz de programación de aplicaciones ampliamente aceptado (API) para el acceso a la base de datos. Usar ODBC para conectarse al explorador de datos de Azure desde las aplicaciones que no tienen un conector específico.

En segundo plano, las aplicaciones llaman a funciones en la interfaz ODBC, que se implementan en los módulos específicos de la base de datos denominados *controladores*. El Explorador de datos de Azure admite un subconjunto del protocolo de comunicación de SQL Server ([MS TDS](/azure/kusto/api/tds/)); por lo tanto puede usar el controlador ODBC para SQL Server.

En este artículo, aprenderá a usar el controlador ODBC de SQL Server, para que pueda conectarse al explorador de datos de Azure desde cualquier aplicación que admite ODBC. Puede, a continuación, si lo desea conectarse al explorador de datos de Azure desde una plantilla y traer datos de un clúster de ejemplo.

## <a name="prerequisites"></a>Requisitos previos

Necesita lo siguiente para completar este procedimiento:

* [Microsoft ODBC Driver para SQL Server versión 17.2.0.1 o posterior](/sql/connect/odbc/download-odbc-driver-for-sql-server) para su sistema operativo.

* Si desea seguir nuestro ejemplo de una plantilla, también debe:

  * Tableau Desktop, completa o [prueba](https://www.tableau.com/products/desktop/download) versión.

  * Un clúster que incluya los datos de ejemplo de StormEvents. Para más información, consulte [Guía de inicio rápido: Creación de un clúster y de la base de datos de Azure Data Explorer](create-cluster-database-portal.md) e [Ingesta de datos de ejemplo en Azure Data Explorer](ingest-sample-data.md).

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="configure-the-odbc-data-source"></a>Configurar el origen de datos ODBC

Siga estos pasos para configurar un origen de datos ODBC mediante el controlador ODBC para SQL Server.

1. En Windows, busque *orígenes de datos ODBC*y abra la aplicación de escritorio de orígenes de datos ODBC.

1. Seleccione **Agregar**.

    ![Agregar origen de datos](media/connect-odbc/add-data-source.png)

1. Seleccione **ODBC Driver 17 for SQL Server** , a continuación, **finalizar**.

    ![Seleccionar controlador](media/connect-odbc/select-driver.png)

1. Escriba un nombre y una descripción para la conexión y el clúster que desea conectarse, a continuación, seleccione **siguiente**. El clúster de dirección URL debe tener el formato  *\<ClusterName\>.\< Región\>. kusto.windows.net*.

    ![Seleccionar servidor](media/connect-odbc/select-server.png)

1. Seleccione **integrado en Active Directory** , a continuación, **siguiente**.

    ![Autenticación integrada de Active Directory](media/connect-odbc/active-directory-integrated.png)

1. Seleccione la base de datos con los datos de ejemplo, a continuación, **siguiente**.

    ![Cambiar base de datos predeterminada](media/connect-odbc/change-default-database.png)

1. En la siguiente pantalla, deje todas las opciones como a continuación, seleccione el valor predeterminado es **finalizar**.

1. Seleccione **Probar origen de datos**.

    ![Origen de datos de prueba](media/connect-odbc/test-data-source.png)

1. Compruebe que la prueba se realizó correctamente, a continuación, seleccione **Aceptar**. Si la prueba no se realizó correctamente, compruebe los valores que especificó en los pasos anteriores y asegúrese de que tiene permisos suficientes para conectarse al clúster.

    ![Prueba se realizó correctamente](media/connect-odbc/test-succeeded.png)

## <a name="visualize-data-in-tableau-optional"></a>Visualizar datos en una plantilla (opcional)

Ahora que ha terminado de configurar ODBC, puede traer datos de ejemplo a Tableau.

1. En Tableau Desktop, en el menú izquierdo, seleccione **otras bases de datos (ODBC)**.

    ![Conexión con ODBC](media/connect-odbc/connect-odbc.png)

1. Para **DSN**, seleccione el origen de datos que creó para ODBC y luego seleccione **sesión**.

    ![Inicio de sesión de ODBC](media/connect-odbc/odbc-sign-in.png)

1. Para **base de datos**, seleccione la base de datos en el clúster de ejemplo, como *TestDatabase*. Para **esquema**, seleccione *dbo*y para **tabla**, seleccione el *StormEvents* tabla de ejemplo.

    ![Seleccione la base de datos y tabla](media/connect-odbc/select-database-table.png)

1. Tableau ahora muestra el esquema para los datos de ejemplo. Seleccione **actualizar ahora** para poner los datos en una plantilla.

    ![Actualización de datos](media/connect-odbc/update-data.png)

    Cuando se importan los datos, Tableau muestra filas de datos similar a la siguiente imagen.

    ![Conjunto de resultados](media/connect-odbc/result-set.png)

1. Ahora puede crear visualizaciones en Tableau según los datos que han aportado desde el Explorador de datos de Azure. Para obtener más información, consulte [Tableau Learning](https://www.tableau.com/learn).

## <a name="next-steps"></a>Pasos siguientes

[Escribir consultas para el Explorador de datos de Azure](write-queries.md)

[Tutorial: Visualizar datos desde el Explorador de datos de Azure en Power BI](visualize-power-bi.md)