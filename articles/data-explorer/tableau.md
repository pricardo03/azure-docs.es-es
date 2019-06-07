---
title: Usar conexión de Open Database Connectivity (ODBC) al explorador de datos de Azure para visualizar datos con una plantilla
description: En este artículo, aprenderá a usar una conexión de Open Database Connectivity (ODBC) para la conexión del explorador de datos de Azure para visualizar datos con una plantilla.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: a0948ae35a5c23768df117979db819861ac64529
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66499090"
---
# <a name="visualize-data-from-azure-data-explorer-in-tableau"></a>Visualizar datos desde el Explorador de datos de Azure en una plantilla

 [Tableau](https://www.tableau.com/) es una plataforma de análisis de objetos visuales de inteligencia empresarial. Para conectarse al explorador de datos de Azure desde una plantilla y trae datos de un clúster de ejemplo, use el controlador de Open Database Connectivity (ODBC) de SQL Server. 

## <a name="prerequisites"></a>Requisitos previos

Necesita lo siguiente para completar este artículo:

* [Conectarse al explorador de datos de Azure con ODBC](connect-odbc.md) con el controlador ODBC de SQL Server, para conectarse al explorador de datos de Azure desde una plantilla. 

* Tableau Desktop, completa, o [prueba](https://www.tableau.com/products/desktop/download) versión.

* Un clúster que incluya los datos de ejemplo de StormEvents. Para obtener más información, consulte [crear un clúster de explorador de datos de Azure y la base de datos](create-cluster-database-portal.md) y [ingerir datos de ejemplo en el Explorador de Azure Data](ingest-sample-data.md).

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="visualize-data-in-tableau"></a>Visualizar datos en una plantilla 

Una vez que haya terminado de configurar ODBC, puede traer datos de ejemplo a Tableau.

1. En Tableau Desktop, en el menú izquierdo, seleccione **otras bases de datos (ODBC)** .

    ![Conexión con ODBC](media/tableau/connect-odbc.png)

1. Para **DSN**, seleccione el origen de datos que creó para ODBC y luego seleccione **sesión**.

    ![Inicio de sesión de ODBC](media/tableau/odbc-sign-in.png)

1. Para **base de datos**, seleccione la base de datos en el clúster de ejemplo, como *TestDatabase*. Para **esquema**, seleccione *dbo*y para **tabla**, seleccione el *StormEvents* tabla de ejemplo.

    ![Seleccione la base de datos y tabla](media/tableau/select-database-table.png)

1. Tableau ahora muestra el esquema para los datos de ejemplo. Seleccione **actualizar ahora** para poner los datos en una plantilla.

    ![Actualización de datos](media/tableau/update-data.png)

    Cuando se importan los datos, Tableau muestra filas de datos similar a la siguiente imagen.

    ![Conjunto de resultados](media/tableau/result-set.png)

1. Ahora puede crear visualizaciones en Tableau según los datos que han aportado desde el Explorador de datos de Azure. Para obtener más información, consulte [Tableau Learning](https://www.tableau.com/learn).

## <a name="next-steps"></a>Pasos siguientes

* [Write queries for Azure Data Explorer](write-queries.md) (Escritura de consultas del Explorador de datos de Azure)