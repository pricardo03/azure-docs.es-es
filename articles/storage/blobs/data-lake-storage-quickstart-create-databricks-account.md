---
title: 'Inicio rápido: Análisis de datos en Azure Data Lake Storage Gen2 con Azure Databricks | Microsoft Docs'
description: Aprenda a ejecutar un trabajo de Spark en Azure Databricks mediante Azure Portal y una cuenta de almacenamiento de Azure Data Lake Storage Gen2.
services: storage
author: normesta
ms.author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: quickstart
ms.date: 02/15/2019
ms.openlocfilehash: c5c69ded05e5ec6d1df6bd2befb4fe89417bae06
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226800"
---
# <a name="quickstart-analyze-data-in-azure-data-lake-storage-gen2-by-using-azure-databricks"></a>Inicio rápido: Análisis de datos en Azure Data Lake Storage Gen2 con Azure Databricks

Este inicio rápido muestra cómo ejecutar un trabajo de Apache Spark mediante Azure Databricks para realizar el análisis de los datos almacenados en una cuenta de almacenamiento que tiene habilitado Azure Data Lake Storage Gen2.

Como parte del trabajo de Spark, analizará los datos de la suscripción a un canal de radio para obtener información detallada acerca del uso gratis o de pago basado en datos demográficos.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

* Cree una cuenta de almacenamiento de Data Lake Gen2. Consulte [Quickstart: Creación de una cuenta de almacenamiento de Azure Data Lake Storage Gen2](data-lake-storage-quickstart-create-account.md)

  Pegue el nombre de la cuenta de almacenamiento en un archivo de texto. Lo necesitará pronto.

* Crear una entidad de servicio. Consulte [Configuración de los portal para crear una aplicación de Azure AD y una entidad de servicio que puedan acceder a los recursos](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal).

  Hay un par de cosas que tendrá que hacer cuando realice los pasos de este artículo.

  :heavy_check_mark: Al realizar los pasos que se describen en la sección [Asignación de la aplicación a un rol](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#assign-the-application-to-a-role), asegúrese de asignar el rol de **Colaborador de datos de blobs de almacenamiento** a la entidad de servicio.

  > [!IMPORTANT]
  > Asegúrese de asignar el rol en el ámbito de la cuenta de almacenamiento de Data Lake Storage Gen2. Puede asignar un rol al grupo de recursos o suscripción primario, pero recibirá errores relacionados con los permisos hasta que esas asignaciones de roles se propaguen a la cuenta de almacenamiento.

  :heavy_check_mark: Al realizar los pasos que se describen en la sección [Obtención de valores para el inicio de sesión](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#get-values-for-signing-in) del artículo, pegue el identificador del inquilino, el identificador de la aplicación y los valores de la clave de autenticación en una clave de texto, ya que los necesitará pronto.

## <a name="create-an-azure-databricks-workspace"></a>Creación de un área de trabajo de Azure Databricks

En esta sección, creará un área de trabajo de Azure Databricks mediante Azure Portal.

1. En Azure Portal, seleccione **Crear un recurso** > **Análisis** > **Azure Databricks**.

    ![Databricks en Azure Portal](./media/data-lake-storage-quickstart-create-databricks-account/azure-databricks-on-portal.png "Databricks en Azure Portal")

2. En **Azure Databricks Service**, proporcione los valores para crear un área de trabajo de Databricks.

    ![Creación de un área de trabajo de Azure Databricks](./media/data-lake-storage-quickstart-create-databricks-account/create-databricks-workspace.png "Creación de un área de trabajo de Azure Databricks")

    Proporcione los valores siguientes:

    |Propiedad  |Descripción  |
    |---------|---------|
    |**Workspace name** (Nombre del área de trabajo)     | Proporcione un nombre para el área de trabajo de Databricks        |
    |**Suscripción**     | En el cuadro desplegable, seleccione la suscripción de Azure.        |
    |**Grupos de recursos**     | Especifique si desea crear un nuevo grupo de recursos o utilizar uno existente. Un grupo de recursos es un contenedor que almacena los recursos relacionados con una solución de Azure. Para más información, consulte [Información general del grupo de recursos de Azure](../../azure-resource-manager/resource-group-overview.md). |
    |**Ubicación**     | Seleccione **Oeste de EE. UU. 2**. No dude en seleccionar otra región pública si lo prefiere.        |
    |**Plan de tarifa**     |  Elija entre **Standard** o **Premium**. Para más información sobre estos planes, consulte la [página de precios de Databricks](https://azure.microsoft.com/pricing/details/databricks/).       |

    Seleccione **Anclar al panel** y luego haga clic en **Crear**.

3. El área de trabajo tarda un poco en crearse. Mientras se crea el área de trabajo, el icono **Enviando implementación para Azure Databricks** aparece a la derecha. Para ver el título, es posible que deba desplazarse a la derecha del panel. También hay una barra de progreso que aparece cerca de la parte superior de la pantalla. Puede ver cualquier área para el progreso.

    ![Icono de implementación de Databricks](./media/data-lake-storage-quickstart-create-databricks-account/databricks-deployment-tile.png "Databricks deployment tile")

## <a name="create-a-spark-cluster-in-databricks"></a>Creación de un clúster de Spark en Databricks

1. En Azure Portal, vaya al área de trabajo de Databricks que ha creado y, después, seleccione **Launch Workspace** (Iniciar área de trabajo).

2. Se le redirigirá al portal de Azure Databricks. En el portal, seleccione **Nuevo** > **Clúster**.

    ![Databricks en Azure](./media/data-lake-storage-quickstart-create-databricks-account/databricks-on-azure.png "Databricks en Azure")

3. En la página **Nuevo clúster**, proporcione los valores para crear un clúster.

    ![Creación de un clúster de Databricks Spark en Azure](./media/data-lake-storage-quickstart-create-databricks-account/create-databricks-spark-cluster.png "Creación de un clúster de Databricks Spark en Azure")

    Acepte los demás valores predeterminados, salvo los siguientes:

    * Escriba un nombre para el clúster.
    * Cree un clúster con el runtime **5.1**.
    * Asegúrese de que selecciona la casilla **Terminate after 120 minutes of inactivity** (Terminar después de 120 minutos de inactividad). Proporcione una duración (en minutos) para terminar el clúster, si este no se usa.

4. Seleccione **Create cluster** (Crear clúster). Una vez que el clúster se está ejecutando, puede asociarle notebooks y ejecutar trabajos de Spark.

Para obtener más información sobre la creación de clústeres, consulte [Create a Spark cluster in Azure Databricks](https://docs.azuredatabricks.net/user-guide/clusters/create.html) (Creación de un clúster de Spark en Azure Databricks).

## <a name="create-storage-account-file-system"></a>Creación de un sistema de archivos de la cuenta de almacenamiento

En esta sección, creará un cuaderno en el área de trabajo de Azure Databricks y, a continuación, ejecutará fragmentos de código para configurar la cuenta de almacenamiento.

1. En [Azure Portal](https://portal.azure.com), vaya al área de trabajo de Azure Databricks que creó y seleccione **Launch Workspace** (Iniciar área de trabajo).

2. En el panel izquierdo, seleccione **Workspace** (Área de trabajo). En la lista desplegable **Workspace** (Área de trabajo), seleccione **Create** > **Notebook** (Crear > Cuaderno).

    ![Creación de notebooks en Databricks](./media/data-lake-storage-quickstart-create-databricks-account/databricks-create-notebook.png "Creación de notebooks en Databricks")

3. En el cuadro de diálogo **Create Notebook** (Crear cuaderno), escriba un nombre para el cuaderno. Seleccione **Scala** como lenguaje y, a continuación, seleccione el clúster de Spark que creó anteriormente.

    ![Creación de notebooks en Databricks](./media/data-lake-storage-quickstart-create-databricks-account/databricks-notebook-details.png "Creación de notebooks en Databricks")

    Seleccione **Crear**.

4. Copie y pegue el siguiente bloque de código en la primera celda, pero no ejecute el código aún.

   ```scala
   spark.conf.set("fs.azure.account.auth.type.<storage-account-name>.dfs.core.windows.net", "OAuth")
   spark.conf.set("fs.azure.account.oauth.provider.type.<storage-account-name>.dfs.core.windows.net", "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider")
   spark.conf.set("fs.azure.account.oauth2.client.id.<storage-account-name>.dfs.core.windows.net", "<application-id>")
   spark.conf.set("fs.azure.account.oauth2.client.secret.<storage-account-name>.dfs.core.windows.net", "<authentication-key>")
   spark.conf.set("fs.azure.account.oauth2.client.endpoint.<storage-account-name>.dfs.core.windows.net", "https://login.microsoftonline.com/<tenant-id>/oauth2/token")
   spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "true")
   dbutils.fs.ls("abfss://<file-system-name>@<storage-account-name>.dfs.core.windows.net/")
   spark.conf.set("fs.azure.createRemoteFileSystemDuringInitialization", "false")

   ```

    > [!NOTE]
    > Dicho bloque de código accede directamente al punto de conexión de Data Lake Gen2 mediante OAuth, pero hay otras maneras de conectarse el área de trabajo de Databricks con su cuenta de Data Lake Storage Gen2. Por ejemplo, podría montar el sistema de archivos mediante OAuth, o bien usar un acceso directo con una clave compartida. <br>Para ver ejemplos de estos métodos, consulte el artículo [Azure Data Lake Storage Gen2](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html) en el sitio web de Azure Databricks.

5. En este bloque de código, reemplace los valores de marcador de posición `storage-account-name`, `application-id`, `authentication-id` y `tenant-id` por los valores que recopiló cuando creó la entidad de servicio. Establezca el valor del marcador `file-system-name` en el nombre que desea dar el sistema de archivos.

    > [!NOTE]
    > En una configuración de producción, considere la posibilidad de almacenar su clave de autenticación en Azure Databricks. A continuación, agregue una clave de búsqueda a su bloque de código en lugar de la clave de autenticación. Una vez completado este inicio rápido, consulte el artículo [Azure Data Lake Storage Gen2](https://docs.azuredatabricks.net/spark/latest/data-sources/azure/azure-datalake-gen2.html) en el sitio Web de Azure Databricks para ver ejemplos de este enfoque.

6. Presione las teclas **MAYÚS + ENTRAR** para ejecutar el código de este bloque.

## <a name="ingest-sample-data"></a>Ingesta de datos de ejemplo

Antes de empezar esta sección, debe completar lo siguientes requisitos previos:

Escriba el código siguiente en una celda del cuaderno:

    %sh wget -P /tmp https://raw.githubusercontent.com/Azure/usql/master/Examples/Samples/Data/json/radiowebsite/small_radio_json.json

En la celda, presione **MAYÚS + ENTRAR** para ejecutar el código.

Ahora, en una nueva celda debajo de esta, escriba el siguiente código y reemplace los valores que aparecen entre paréntesis por los mismos valores que usó anteriormente:

    dbutils.fs.cp("file:///tmp/small_radio_json.json", "abfss://<file-system>@<account-name>.dfs.core.windows.net/")

En la celda, presione **MAYÚS + ENTRAR** para ejecutar el código.

## <a name="run-a-spark-sql-job"></a>Ejecución de un trabajo de Spark SQL

Realice las siguientes tareas para ejecutar un trabajo de Spark SQL en los datos.

1. Ejecute una instrucción SQL para crear una tabla temporal con datos desde el archivo de datos JSON de ejemplo, **small_radio_json.json**. En el siguiente fragmento de código, reemplace los valores del marcador de posición con el nombre del sistema de archivos y el nombre de la cuenta de almacenamiento. Use el cuaderno que ha creado antes, pegue el fragmento de código en una nueva celda de código y, después, presione MAYÚS + ENTRAR.

    ```sql
    %sql
    DROP TABLE IF EXISTS radio_sample_data;
    CREATE TABLE radio_sample_data
    USING json
    OPTIONS (
     path  "abfss://<file-system-name>@<storage-account-name>.dfs.core.windows.net/<PATH>/small_radio_json.json"
    )
    ```

    Una vez que el comando se completa correctamente, tiene todos los datos desde el archivo JSON como una tabla en el clúster de Databricks.

    El comando mágico del lenguaje `%sql` le permite ejecutar un código SQL desde el notebook, aunque este sea de otro tipo. Para obtener más información, consulte [Combinación de lenguajes en un notebook](https://docs.azuredatabricks.net/user-guide/notebooks/index.html#mixing-languages-in-a-notebook).

2. Echemos un vistazo a una instantánea de los datos JSON de ejemplo para entender mejor la consulta que se ejecuta. Pegue el siguiente fragmento de código en una celda vacía y presione **MAYÚS + ENTRAR**.

    ```sql
    %sql
    SELECT * from radio_sample_data
    ```

3. Verá un resultado tabular como se muestra en la siguiente captura de pantalla (solo se ven algunas columnas):

    ![Datos JSON de ejemplo](./media/data-lake-storage-quickstart-create-databricks-account/databricks-sample-csv-data.png "Datos JSON de ejemplo")

    Entre otros detalles, los datos de ejemplo capturan el género de la audiencia de un canal de radio (nombre de columna, **género**) y si su suscripción es gratis o de pago (nombre de columna, **nivel**).

4. Ahora cree una representación visual de estos datos para mostrar para cada género, cuántos usuarios tienen cuentas gratis y cuántas son de suscriptores pagados. En la parte inferior de la salida tabular, haga clic en el icono **Gráfico de barras** y, a continuación, haga clic en **Opciones de trazado**.

    ![Creación del gráfico de barras](./media/data-lake-storage-quickstart-create-databricks-account/create-plots-databricks-notebook.png "Creación del gráfico de barras")

5. En **Personalizar trazado**, arrastre y coloque los valores como se muestra en la captura de pantalla.

    ![Personalización del gráfico de barras](./media/data-lake-storage-quickstart-create-databricks-account/databricks-notebook-customize-plot.png "Personalización del gráfico de barras")

    - Establezca **Claves** en **gender** (género).
    - Establezca **Agrupaciones de serie** en **level** (nivel).
    - Establezca **Valores** en **level** (nivel).
    - Establezca **Agregación** en **COUNT** (recuento).

6. Haga clic en **Aplicar**.

7. El resultado muestra la representación visual, como se describe en la siguiente captura de pantalla:

     ![Personalización del gráfico de barras](./media/data-lake-storage-quickstart-create-databricks-account/databricks-sql-query-output-bar-chart.png "Personalización del gráfico de barras")

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya finalizado con este artículo, puede terminar el clúster. En el área de trabajo de Azure Databricks, seleccione **Clusters** (Clústeres) y localice el clúster que desea terminar. Mueva el cursor sobre el botón de puntos suspensivos en la columna **Actions** (Acciones) y seleccione el icono **Terminate** (Finalizar).

![Detener un clúster de Databricks](./media/data-lake-storage-quickstart-create-databricks-account/terminate-databricks-cluster.png "Stop a Databricks cluster")

Si no finaliza manualmente el clúster, este se detendrá automáticamente si seleccionó la casilla **Terminate after \_\_ minutes of inactivity** (Finalizar después de \_\_ minutos de inactividad) al crear el clúster. Si establece esta opción, el clúster se detendrá después de haber estado inactivo durante la cantidad de tiempo designada.

## <a name="next-steps"></a>Pasos siguientes

En este artículo, ha creado un clúster de Spark en Azure Databricks y ha ejecutado un trabajo de Spark mediante los datos de una cuenta de almacenamiento con Data Lake Storage Gen2 habilitado. También puede mirar [Spark data sources](https://docs.azuredatabricks.net/spark/latest/data-sources/index.html) (Orígenes de datos de Spark) para aprender a importar datos desde otros orígenes de datos en Azure Databricks. Vaya al siguiente artículo para aprender cómo realizar una operación ETL (extraer, transformar y cargar datos) mediante Azure Databricks.

> [!div class="nextstepaction"]
>[Extracción, transformación y carga de datos mediante Azure Databricks](../../azure-databricks/databricks-extract-load-sql-data-warehouse.md)
