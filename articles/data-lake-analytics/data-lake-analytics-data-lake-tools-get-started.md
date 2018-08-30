---
title: Introducción a Azure Data Lake Analytics mediante Visual Studio
description: Aprenda a instalar Data Lake Tools para Visual Studio y a desarrollar y probar scripts U-SQL.
services: data-lake-analytics
ms.service: data-lake-analytics
author: saveenr
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: ad8a6992-02c7-47d4-a108-62fc5a0777a3
ms.topic: conceptual
ms.date: 08/13/2018
ms.openlocfilehash: 2af1cf1c41ddacd973b3e1dc0f2dd7a8b6d9563a
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43045962"
---
# <a name="develop-u-sql-scripts-by-using-data-lake-tools-for-visual-studio"></a>Desarrollo de scripts U-SQL mediante Data Lake Tools para Visual Studio
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Aprenda a usar Visual Studio para crear cuentas de Azure Data Lake Analytics, definir trabajos en [U-SQL](data-lake-analytics-u-sql-get-started.md) y enviar trabajos al servicio Data Lake Analytics. Para obtener más información acerca de Análisis de Data Lake, consulte [Información general sobre Análisis de Azure Data Lake](data-lake-analytics-overview.md).

>[!IMPORTANT]
> Microsoft le recomienda que actualice a Herramientas de Azure Data Lake para Visual Studio versión 2.3.3000.4, o cualquier versión posterior. Las versiones anteriores no están disponibles para su descarga y han quedado en desuso. 
>
>**¿Qué tengo que hacer?**
>
>1. Compruebe que su versión de Herramientas de Azure Data Lake para Visual Studio es anterior a 2.3.3000.4. 
>   
>   ![Comprobación de la versión de la herramienta](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-about-data-lake.png)
> 
>2. Si su versión es anterior a 2.3.3000.4, vaya al centro de descarga y actualice Herramientas de Azure Data Lake para Visual Studio: 
>    - [Para Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=ADLTools.AzureDataLakeandStreamAnalyticsTools)
>    - [Para Visual Studio 2013 y 2015](https://www.microsoft.com/en-us/download/details.aspx?id=49504)


## <a name="prerequisites"></a>Requisitos previos

* **Visual Studio**: se admiten todas las ediciones, excepto Express.
    * Visual Studio 2017
    * Visual Studio 2015
    * Visual Studio 2013
* **Microsoft Azure SDK para .NET** versión 2.7.1, o posterior.  Instálelo con el [Instalador de plataforma web](http://www.microsoft.com/web/downloads/platform.aspx).
* Una cuenta de **Data Lake Analytics**. Para crear una cuenta, consulte [Introducción a Azure Data Lake Analytics mediante Azure Portal](data-lake-analytics-get-started-portal.md).

## <a name="install-azure-data-lake-tools-for-visual-studio"></a>Instalación de Herramientas de Azure Data Lake para Visual Studio

Este tutorial requiere que esté instalado Data Lake Tools para Visual Studio. Siga las [instrucciones de instalación](data-lake-analytics-data-lake-tools-install.md).

## <a name="connect-to-an-azure-data-lake-analytics-account"></a>Conexión a una cuenta de Azure Data Lake Analytics

1. Abra Visual Studio.

2. Abra el Explorador de servidores, para lo que debe seleccionar **Vista** > **Explorador de servidores**.

3. Haga clic con el botón derecho en **Azure**. Después, seleccione **Conectar a la suscripción de Microsoft Azure** y siga las instrucciones.

4. En el Explorador de servidores, seleccione **Azure** > **Data Lake Analytics**. Verá una lista de las cuentas de Data Lake Analytics.

## <a name="write-your-first-u-sql-script"></a>Escriba el primer script U-SQL

El siguiente texto es un script U-SQL simple. Define un conjunto de datos pequeño y lo escribe en la instancia predeterminada de Data Lake Store como un archivo denominado `/data.csv`.

```
USE DATABASE master;
USE SCHEMA dbo;
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
```

## <a name="submit-a-data-lake-analytics-job"></a>Envío de un trabajo de Análisis de Data Lake

1. Seleccione **Archivo** > **Nuevo** > **Proyecto**.

2. Seleccione el tipo **Proyecto U-SQL** y haga clic en **Aceptar**. Visual Studio crea una solución con un archivo **Script.usql**.

3. Pegue el script anterior en la ventana de **Script.usql**.

4. En la esquina superior izquierda de la ventana de **Script.usql**, especifique la cuenta de Data Lake Analytics.

    ![Enviar proyecto U-SQL de Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-submit-job.png)

5. En la esquina superior izquierda de la ventana de **Script.usql**, seleccione **Enviar**.

6. Tras el envío del trabajo, se abre la pestaña **Vista Trabajo** para mostrar el progreso del trabajo. Para ver el estado del trabajo más reciente y actualizar la pantalla, haga clic en **Actualizar**.

    ![Gráfico de rendimiento del trabajo de Data Lake Analytics de U-SQL Visual Studio](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-performance-graph.png)

   * **Resumen del trabajo** muestra el resumen del trabajo.   
   * **Grafo de trabajo** muestra el progreso del trabajo.
   * **Operaciones de metadatos** muestra todas las acciones que se realizaron en el catálogo de U-SQL.
   * **Datos** muestra todas las entradas y salidas.
   * **Historial de estado** muestra los detalles de la escala de tiempo y el estado.
   * **Análisis de AU** muestra el número de unidades de análisis que se han usado en el trabajo y explora simulaciones de diferentes estrategias de asignación de unidades de análisis.
   * **Diagnósticos** proporciona un análisis avanzado para la optimización de rendimiento y la ejecución del trabajo.

## <a name="check-job-status"></a>Comprobación del estado del trabajo

1. En el Explorador de servidores, seleccione **Azure** > **Data Lake Analytics**.

2. Expanda el nombre de la cuenta de Data Lake Analytics.

3. Haga doble clic en **Trabajos**.

4. Seleccione el trabajo que envió anteriormente.

## <a name="see-the-job-output"></a>Visualización de la salida del trabajo

1. En el Explorador de servidores, busque el trabajo que ha enviado.

2. Haga clic en la pestaña **Datos** .

3. En el pestaña **Salidas de trabajo**, seleccione el archivo `"/data.csv"`.

## <a name="next-steps"></a>Pasos siguientes

* [Prueba y depuración de trabajos U-SQL mediante la ejecución local y el SDK de U-SQL para Azure Data Lake](data-lake-analytics-data-lake-tools-local-run.md)
* [Depuración de código de C# en los trabajos de U-SQL con las Herramientas de Azure Data Lake para Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md)
* [Uso de Azure Data Lake Tools para Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md)
