---
title: Visualizar datos desde el Explorador de datos de Azure mediante Sisense
description: En este artículo, obtenga información sobre cómo configurar el Explorador de datos de Azure como origen de datos de Sisense y visualizar los datos.
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 5/29/2019
ms.openlocfilehash: f0840b90e1036c23fa58d94515bfeb035299c07f
ms.sourcegitcommit: 8e76be591034b618f5c11f4e66668f48c090ddfd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358189"
---
# <a name="visualize-data-from-azure-data-explorer-in-sisense"></a>Visualizar datos desde el Explorador de datos de Azure en Sisense

Sisense es una plataforma de inteligencia empresarial de análisis que le permite compilar aplicaciones de análisis que proporcionan experiencias de usuario altamente interactiva. El panel informes de software e inteligencia empresarial le permite obtener acceso a combinar datos en unos pocos clics. Puede conectarse a datos estructurados y orígenes, combinar tablas desde varios orígenes con scripting mínima y codificación y crear informes y paneles web interactivas. En este artículo, obtendrá información sobre cómo configurar el Explorador de datos de Azure como origen de datos de Sisense y visualizar datos desde un clúster de ejemplo.

## <a name="prerequisites"></a>Requisitos previos

Necesita lo siguiente para completar este artículo:

* [Descargue e instale la aplicación de Sisense](https://documentation.sisense.com/latest/getting-started/download-install.htm) 

* Cree un clúster y la base de datos que incluye los datos de ejemplo StormEvents. Para más información, consulte [Inicio rápido: Creación de un clúster y de la base de datos de Azure Data Explorer](create-cluster-database-portal.md) e [Ingesta de datos de ejemplo en Azure Data Explorer](ingest-sample-data.md).

    [!INCLUDE [data-explorer-storm-events](../../includes/data-explorer-storm-events.md)]

## <a name="connect-to-sisense-dashboards-using-azure-data-explorer-jdbc-connector"></a>Conéctese a los paneles de Sisense mediante el conector JDBC de explorador de datos de Azure

1. Descargue y copie las versiones más recientes de los siguientes archivos jar a *... \Sisense\DataConnectors\jdbcdrivers\adx* 

    * activación 1.1.jar
    * adal4j-1.6.0.jar
    * commons-codec-1.10.jar
    * commons-collections4-4.1.jar
    * commons-lang3-3.5.jar
    * gson-2.8.0.jar
    * jcip-annotations-1.0-1.jar
    * json-smart-1.3.1.jar
    * lang-tag-1.4.4.jar
    * mail-1.4.7.jar
    * mssql-jdbc-7.2.1.jre8.jar
    * nimbus-jose-jwt-7.0.1.jar
    * oauth2-oidc-sdk-5.24.1.jar
    * slf4j-api-1.7.21.jar
    
1. Abra **Sisense** app.
1. Seleccione **datos** pestaña y seleccione **+ ElastiCube** para crear un nuevo modelo ElastiCube.
    
    ![Seleccione ElastiCube](media/sisense/data-select-elasticube.png)

1. En **Agregar nuevo modelo ElastiCube**, asignar nombre al modelo ElastiCube y **guardar**.
   
    ![Agregar nuevo modelo ElastiCube](media/sisense/add-new-elasticube-model.png)

1. Seleccione **+ datos**.

    ![Seleccione el botón de datos](media/sisense/select-data.png)

1. En **seleccione conector** pestaña, seleccione **JDBC genérico** conector.

    ![Elija el conector JDBC](media/sisense/select-connector.png)

1. En el **Connect** pestaña, complete los siguientes campos para el **JDBC genérico** conector y seleccione **siguiente**.

    ![Configuración del conector de JDBC](media/sisense/jdbc-connector.png)

    |Campo |DESCRIPCIÓN |
    |---------|---------|
    |Cadena de conexión     |   `jdbc:sqlserver://<cluster_name.region>.kusto.windows.net:1433;database=<database_name>;encrypt=true;trustServerCertificate=false;hostNameInCertificate=*.kusto.windows.net;loginTimeout=30;authentication=ActiveDirectoryPassword`      |
    |Carpeta de archivos JAR de JDBC  |    `..\Sisense\DataConnectors\jdbcdrivers\adx`     |
    |Nombre de clase del controlador    |   `com.microsoft.sqlserver.jdbc.SQLServerDriver`      |
    |User Name   |    Nombre de usuario AAD     |
    |Contraseña     |   Contraseña de usuario AAD      |

1. En el **seleccionar datos** pestaña, buscar **Seleccionar base de datos** para seleccionar la base de datos pertinente a las que tiene permisos. En este ejemplo, seleccione *test1*.

    ![seleccionar base de datos](media/sisense/select-database.png)

1. En *probar* panel (nombre de la base de datos):
    1. Seleccione el nombre de tabla para obtener una vista previa de la tabla y ver los nombres de columna de tabla. Puede quitar las columnas innecesarias.
    1. Seleccione la casilla de verificación de la tabla para seleccionar esa tabla. 
    1. Seleccione **Listo**.

    ![Seleccionar tabla](media/sisense/select-table-see-columns.png)    

1. Seleccione **compilación** para generar el conjunto de datos. 

    * En el **compilar** ventana, seleccione **compilar**.

      ![Ventana de la compilación](media/sisense/build-window.png)

    * Espere hasta que el proceso de compilación está completa y, a continuación, seleccione **compilación correcta**.

      ![Compilación correcta](media/sisense/build-succeeded.png)

## <a name="create-sisense-dashboards"></a>Crear paneles de Sisense

1. En **Analytics** ficha, seleccione **+**  >  **nuevo panel** para crear paneles en este conjunto de datos.

    ![Nuevo panel](media/sisense/new-dashboard.png)

1. Seleccione un panel y seleccione **crear**. 

    ![Crear panel](media/sisense/create-dashboard.png)

1. En **nuevo Widget**, seleccione **+ seleccionar datos** para crear un nuevo widget. 

    ![Agregar campos al panel StormEvents](media/sisense/storm-dashboard-add-field.png)  

1. Seleccione **+ agregar más datos** para agregar columnas adicionales a su gráfico. 

    ![Agregue más datos al gráfico](media/sisense/add-more-data.png)

1. Seleccione **+ Widget** para crear otro widget. Widgets de arrastrar y colocar para volver a organizar el panel.

    ![Panel de Storm](media/sisense/final-dashboard.png)

Ahora puede explorar los datos con análisis visuales, cree paneles adicionales y transformar datos en información útil para lograr un efecto en su negocio.

## <a name="next-steps"></a>Pasos siguientes

* [Write queries for Azure Data Explorer](write-queries.md) (Escritura de consultas del Explorador de datos de Azure)

