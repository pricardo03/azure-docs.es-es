---
title: Use la conexión de conectividad abierta de bases de datos (ODBC) a Azure Data Explorer para visualizar datos con Tableau.
description: En este artículo, aprenderá a usar una conexión de conectividad abierta de bases de datos (ODBC) a Azure Data Explorer para visualizar datos con Tableau.
author: orspod
ms.author: orspodek
ms.reviewer: gabil
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 4dd8fbd761a3442536919e17bae5465adf6b945f
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2019
ms.locfileid: "74023861"
---
# <a name="visualize-data-from-azure-data-explorer-in-tableau"></a>Visualización de datos de Azure Data Explorer en Tableau

 [Tableau](https://www.tableau.com/) es una plataforma de análisis visual para inteligencia empresarial. Para conectarse a Azure Data Explorer desde Tableau y transferir datos de un clúster de ejemplo, use el controlador de conectividad abierta de bases de datos (ODBC) de SQL Server. 

## <a name="prerequisites"></a>Requisitos previos

Para completar este artículo, necesitará lo siguiente:

* [Conectarse a Azure Data Explorer con ODBC](connect-odbc.md) con el controlador de ODBC de SQL Server, para conectarse a Azure Data Explorer desde Tableau. 

* Tableau Desktop, versión completa o de [prueba](https://www.tableau.com/products/desktop/download).

* Un clúster que incluya los datos de ejemplo de StormEvents. Para más información, consulte [Creación de un clúster y una base de datos de Azure Data Explorer](create-cluster-database-portal.md) e [Ingesta de datos de ejemplo en el Explorador de datos de Azure](ingest-sample-data.md).

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="visualize-data-in-tableau"></a>Visualización de datos en Tableau 

Una vez que haya terminado de configurar ODBC, puede transferir datos de ejemplo a Tableau.

1. En Tableau Desktop, en el menú izquierdo, seleccione **Other Databases (ODBC)** [Otras bases de datos (ODBC)].

    ![Conexión con ODBC](media/tableau/connect-odbc.png)

1. Para **DSN**, seleccione el origen de datos que creó para ODBC y, a continuación, seleccione **Iniciar sesión**.

    ![Inicio de sesión de ODBC](media/tableau/odbc-sign-in.png)

1. Para **Base de datos**, seleccione la base de datos del clúster de ejemplo, como *TestDatabase*. Para **Esquema**, seleccione *dbo* y, para **Tabla**, seleccione la tabla de ejemplo *StormEvents*.

    ![Selección de una base de datos y una tabla](media/tableau/select-database-table.png)

1. Tableau ahora muestra el esquema para los datos de ejemplo. Seleccione **Actualizar ahora** para transferir los datos a Tableau.

    ![Actualización de datos](media/tableau/update-data.png)

    Cuando se importan los datos, Tableau muestra filas de datos como en la siguiente imagen.

    ![Conjunto de resultados](media/tableau/result-set.png)

1. Ahora puede crear visualizaciones en Tableau basadas en los datos que ha transferido desde Azure Data Explorer. Para más información, consulte la [formación de Tableau](https://www.tableau.com/learn).

## <a name="next-steps"></a>Pasos siguientes

* [Write queries for Azure Data Explorer](write-queries.md) (Escritura de consultas del Explorador de datos de Azure)