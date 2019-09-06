---
title: Análisis de datos en Azure Data Lake Storage Gen2 con Power BI | Microsoft Docs
description: Uso de Power BI para analizar datos almacenados en Azure Data Lake Storage Gen2
author: normesta
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.topic: conceptual
ms.date: 07/18/2019
ms.author: normesta
ms.reviewer: bensack
ms.openlocfilehash: 59574f0a07f4ecc145f2b0efd430e65cbdebcc65
ms.sourcegitcommit: 007ee4ac1c64810632754d9db2277663a138f9c4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/23/2019
ms.locfileid: "69991574"
---
# <a name="analyze-data-in-azure-data-lake-storage-gen2-by-using-power-bi"></a>Análisis de datos en Azure Data Lake Storage Gen2 con Power BI

En este artículo aprenderá a usar Power BI Desktop para analizar y visualizar los datos que están almacenados en una cuenta de almacenan con un espacio de nombres jerárquico (Azure Data Lake Storage Gen2).

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar este tutorial, debe contar con lo siguiente:

> [!div class="checklist"]
> * Una suscripción de Azure. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).
> * Una cuenta de almacenamiento con un espacio de nombres jerárquico. Siga [estas](data-lake-storage-quickstart-create-account.md) instrucciones para crear uno.
> En este artículo se da por supuesto que creó una cuenta denominada `myadlsg2`.
> * Un archivo de datos de ejemplo denominado `Drivers.txt` ubicado en la cuenta de almacenamiento.
> Puede descargar este ejemplo desde el [repositorio Git de Azure Data Lake](https://github.com/Azure/usql/tree/master/Examples/Samples/Data/AmbulanceDataDrivers.txt) y, luego, cargue ese archivo en la cuenta de almacenamiento.
> * **Power BI Desktop**. Puede descargarla del [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=45331). 

## <a name="create-a-report-in-power-bi-desktop"></a>Creación de un informe en Power BI Desktop

1. Inicie Power BI Desktop en el equipo.
2. En la pestaña **Inicio** de la Cinta, haga clic en **Obtener datos** y, luego, en **Más**.
3. En el cuadro de diálogo **Obtener datos**, haga clic en **Azure**, en **Azure Data Lake Store Gen2 (beta)** y, luego, en **Conectar**.

    ![Página Obtener datos](media/data-lake-storage-use-power-bi/get-data-page.png)

4. En el cuadro de diálogo **Azure Data Lake Storage Gen2**, puede proporcionar la dirección URL a la cuenta de Azure Data Lake Storage Gen2, al sistema de archivos o a la subcarpeta con el formato de punto de conexión del contenedor. Las direcciones URL para Data Lake Storage Gen2 tienen el patrón siguiente, `https://<accountname>.dfs.core.windows.net/<filesystemname>/<subfolder>`, y luego haga clic en **Aceptar**.

    ![URL](media/data-lake-storage-use-power-bi/adls-url.png)

5. En el cuadro de diálogo siguiente, haga clic en **Iniciar sesión** para iniciar sesión en la cuenta de almacenamiento. Se le redirigirá a la página de inicio de sesión de su organización. Siga las indicaciones para iniciar sesión en la cuenta.

    ![Página de inicio de sesión](media/data-lake-storage-use-power-bi/sign-in.png)

6. Cuando haya iniciado sesión correctamente, haga clic en **Conectar**.

    ![Página de sesión iniciada](media/data-lake-storage-use-power-bi/signed-in.png)

7. En el cuadro de diálogo siguiente se muestran todos los archivos en la dirección URL que proporcionó en el paso 4, incluido el archivo que cargó en la cuenta de almacenamiento. Compruebe la información y, luego, haga clic en **Cargar**.

    ![Sistemas de archivos](media/data-lake-storage-use-power-bi/file-systems.png)

8. Después de cargar correctamente los datos en Power BI, verá los siguientes campos en la pestaña **Campos**.

    ![Pestaña Campos](media/data-lake-storage-use-power-bi/fields.png)

    Pero, para visualizar y analizar los datos, es preferible que estos estén disponibles por los campos siguientes.

    ![Fields](media/data-lake-storage-use-power-bi/preferred-fields.png)

    En los pasos siguientes, se actualizará la consulta para convertir los datos importados al formato deseado.

9. En la pestaña **Inicio** de la Cinta, haga clic en **Editar consultas**.

    ![Consultas](media/data-lake-storage-use-power-bi/queries.png)

10. En el **Editor de consultas**, en la columna **Contenido**, haga clic en **Binario**. El archivo se detectará automáticamente como archivo .csv y se debería ver una salida como la que se muestra a continuación. Los datos están ahora disponibles en un formato que puede usar para crear visualizaciones.

    ![Output](media/data-lake-storage-use-power-bi/binary.png)

11. En la pestaña **Inicio** de la Cinta, haga clic en **Cerrar** y en **Aplicar** y, luego, haga clic en **Cerrar** y en **Aplicar**.

    ![Cerrar y aplicar](media/data-lake-storage-use-power-bi/close-apply.png)

12. Después de actualizar la consulta, la pestaña **Campos** mostrará los nuevos campos disponibles para su visualización.

    ![Nuevos campos](media/data-lake-storage-use-power-bi/new-fields.png)

13. Vamos a crear un gráfico circular para representar los conductores de cada ciudad de un país determinado. Para ello, realice las selecciones siguientes.

    En la pestaña **Visualizaciones**, haga clic en el símbolo de un gráfico circular.

    ![Visualizaciones](media/data-lake-storage-use-power-bi/visualizations.png)

    Las columnas que se van a usar son Columna 4 (nombre de la ciudad) y Columna 7 (nombre del país). Arrastre estas columnas de la pestaña **Campos** a la pestaña **Visualizaciones**, tal y como se muestra a continuación.

    ![Arrastrar campos](media/data-lake-storage-use-power-bi/visualizations-drag-fields.png)

    El gráfico circular ahora debe ser similar al que se muestra a continuación.

    ![Gráfico circular](media/data-lake-storage-use-power-bi/pie-chart.png)

14. Si selecciona un país específico en los filtros de nivel de página, ahora puede ver el número de conductores de cada ciudad del país seleccionado. Por ejemplo, en la pestaña **Visualizaciones**, en **Filtros de nivel de página**, seleccione **Brasil**.

    ![Filtros de página](media/data-lake-storage-use-power-bi/page-filters.png)

15. El gráfico circular se actualiza automáticamente para mostrar los conductores de las ciudades de Brasil.

    ![Brasil](media/data-lake-storage-use-power-bi/pie-chart-updated.png)

16. En el menú **Archivo**, haga clic en **Guardar** para guardar la visualización como archivo de Power BI Desktop.

## <a name="publish-report-to-power-bi-service"></a>Publicación del informe en el servicio Power BI

Después de crear las visualizaciones en Power BI Desktop, puede compartirlas con otros usuarios publicándolas en el servicio Power BI. Para obtener instrucciones sobre cómo hacerlo, consulte [Publicar desde Power BI Desktop](https://powerbi.microsoft.com/documentation/powerbi-desktop-upload-desktop-files/).