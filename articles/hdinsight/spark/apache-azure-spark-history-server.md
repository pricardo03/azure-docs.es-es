---
title: 'Uso del Servidor de historial de Spark extendido para depurar y diagnosticar aplicaciones Spark: Azure HDInsight'
description: 'Use el Servidor de historial de Spark extendido para depurar y diagnosticar aplicaciones Spark: Azure HDInsight.'
services: hdinsight
ms.service: hdinsight
author: jejiang
ms.author: jejiang
ms.reviewer: jasonh
ms.custom: hdinsightactive,hdiseo17may2017
ms.topic: conceptual
ms.date: 07/12/2018
ms.openlocfilehash: b514f23f2e8a43f99fd5bf5c3afb5ed625ad4472
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/27/2018
ms.locfileid: "43046582"
---
# <a name="use-extended-spark-history-server-to-debug-and-diagnose-spark-applications"></a>Uso del Servidor de historial de Spark extendido para depurar y diagnosticar las aplicaciones de Spark

En este artículo se proporcionan instrucciones sobre cómo usar el Servidor de historial de Spark para depurar y diagnosticar las aplicaciones Spark en ejecución y completadas. La extensión incluye actualmente la pestaña de datos y la pestaña de gráfico. En la pestaña de datos, los usuarios pueden comprobar los datos de entrada y salida del trabajo de Spark. En la pestaña de gráfico, los usuarios pueden comprobar el flujo de datos y reproducir el gráfico del trabajo.

## <a name="open-the-spark-history-server"></a>Apertura del servidor de historial de Spark

Servidor de historial de Spark es la interfaz de usuario web para aplicaciones de Spark completadas y en ejecución. 

### <a name="to-open-the-spark-history-server-web-ui-from-azure-portal"></a>Para abrir la interfaz de usuario web del Servidor de historial de Spark desde Azure Portal, siga estos pasos:

1. En [Azure Portal](https://portal.azure.com/), abra el clúster de Spark. Para obtener más información, consulte [Enumeración y visualización de clústeres](../hdinsight-administer-use-portal-linux.md#list-and-show-clusters).
2. En **Vínculos rápidos**, haga clic en **Panel de clúster** y después en **Servidor de historial de Spark**. Cuando se le pida, escriba las credenciales de administrador del clúster Spark. 

    ![Servidor de historial de Spark](./media/apache-azure-spark-history-server/launch-history-server.png "Servidor de historial de Spark")

### <a name="to-open-the-spark-history-server-web-ui-by-url"></a>Para abrir la interfaz de usuario web del Servidor de historial de Spark por la dirección URL; siga estos pasos:
Para abrir el Servidor de historial de Spark, vaya a la siguiente dirección URL y reemplace <ClusterName> por el nombre del clúster de Spark del cliente.

   ```
   https://<ClusterName>.azurehdinsight.net/sparkhistory
   ```

La interfaz de usuario web del servidor de historial de Spark tiene el aspecto siguiente:

![Servidor de historial de HDInsight Spark](./media/apache-azure-spark-history-server/hdinsight-spark-history-server.png)


## <a name="open-the-data-tab-from-spark-history-server"></a>Abrir la pestaña Datos desde el Servidor de historial de Spark
Seleccione el identificador del trabajo y haga clic en **Data** (Datos) en el menú de herramientas para obtener la vista de datos.

+ Consulte por separado las pestañas **Inputs** (Entradas), **Outputs** (Salidas) y **Table Operations** (Operaciones de tabla).

    ![Pestañas de datos](./media/apache-azure-spark-history-server/sparkui-data-tabs.png)

+ Haga clic en el botón **Copy** (Copiar) para copiar todas las filas.

    ![Copia de datos](./media/apache-azure-spark-history-server/sparkui-data-copy.png)

+ Haga clic en el botón **csv** para guardar todos los datos como un archivo CSV.

    ![Almacenamiento de datos](./media/apache-azure-spark-history-server/sparkui-data-save.png)

+ En el campo **Search** (Buscar), escriba las palabras clave; el resultado de búsqueda se muestra inmediatamente.

    ![Búsqueda de datos](./media/apache-azure-spark-history-server/sparkui-data-search.png)

+ Haga clic en el encabezado de columna para ordenar la tabla, haga clic en el signo más para expandir una fila y mostrar más detalles o haga clic en el signo menos para contraer una fila.

    ![Tabla de datos](./media/apache-azure-spark-history-server/sparkui-data-table.png)

+ Haga clic en el botón **Partial Download** (Descarga parcial) situado a la derecha para descargar un único archivo; el archivo seleccionado se descarga en local; si el archivo ya no existe, se abrirá una nueva pestaña para mostrar los mensajes de error.

    ![Fila de descarga de datos](./media/apache-azure-spark-history-server/sparkui-data-download-row.png)

+ Seleccione las opciones **Copy Full Path** (Copiar ruta de acceso completo) o **Copy Relative Path** (Copiar ruta de acceso relativa) que se expanden desde el menú de descarga para copiar una de estas rutas. Para los archivos de Azure Data Lake, **Open in Azure Storage Explorer** (Abrir en Explorador de Azure Storage) iniciará Azure Storage Explorer y buscará la carpeta al iniciar sesión.

    ![Ruta de acceso de copia de datos](./media/apache-azure-spark-history-server/sparkui-data-copy-path.png)

+ Haga clic en el número situado debajo de la tabla para desplazarse por las páginas cuando en una página se muestran demasiadas filas. 

    ![Página de datos](./media/apache-azure-spark-history-server/sparkui-data-page.png)

+ Mantenga el mouse sobre el signo de interrogación junto Data (Datos) para mostrar la información sobre herramientas, o haga clic en el signo de interrogación para obtener más información.

    ![Más información sobre los datos](./media/apache-azure-spark-history-server/sparkui-data-more-info.png)

+ Haga clic en **Provide us feedback** (Proporcione sus comentarios) para enviar comentarios sobre problemas.

    ![Comentarios de gráfico](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)


## <a name="open-the-graph-tab-from-spark-history-server"></a>Abrir la pestaña Graph (Gráfico) del Servidor de historial de Spark
Seleccione el identificador del trabajo y haga clic en **Graph** (Gráfico) en el menú de herramientas para obtener la vista gráfica del trabajo.

+ Puede consultar la información general del trabajo mediante el gráfico generado. 

+ De forma predeterminada, muestran todos los trabajos, y se pueden filtrar por **Job ID**(Id. del trabajo).

    ![Identificador de trabajo del gráfico](./media/apache-azure-spark-history-server/sparkui-graph-jobid.png)

+ De forma predeterminada, se selecciona **Progress** (Progreso), pero el usuario puede consultar el flujo de datos si selecciona **Read/Written** (leídos o escritos) en la lista desplegable de **Display** (Mostrar).

    ![Visualización del gráfico](./media/apache-azure-spark-history-server/sparkui-graph-display.png)

    La visualización de los nodos del gráfico en color que muestra el mapa térmico.

    ![Mapa térmico del gráfico](./media/apache-azure-spark-history-server/sparkui-graph-heatmap.png)

+ Haga clic en el botón **Playback** (Reproducir) para reproducir el trabajo o en Stop (Detener) para detenerlo en cualquier momento. La visualización de tareas en color para mostrar diferentes estados durante la reproducción:

    + Verde para correcto: el trabajo se ha completado correctamente.
    + Naranja para reintento: instancias de tareas que no se pudieron realizar pero que no afectan al resultado final del trabajo. Estas tareas tenían instancias duplicadas o que se volvieron a intentar que podrían completarse con éxito más tarde.
    + Rojo para error: la tarea ha generado errores.
    + Azul para en ejecución: la tarea está en ejecución.
    + Blanco para omisión o en espera: la tarea está esperando a ejecutarse, o se ha omitido la fase.

    ![Ejemplo de color de gráfico: en ejecución](./media/apache-azure-spark-history-server/sparkui-graph-color-running.png)
 
    ![Ejemplo de color de gráfico: en ejecución: error](./media/apache-azure-spark-history-server/sparkui-graph-color-failed.png)
 
    > [!NOTE]
    > Se permite la reproducción de cada trabajo. Cuando un trabajo no tiene ninguna fase o no se ha completado, no se admite la reproducción.


+ Desplace el mouse para acercar o alejar el gráfico del trabajo, o haga clic en **Zoom to fit** (Zoom para ajustar) para ajustarlo a la pantalla.
 
    ![Zoom del gráfico para ajustarlo](./media/apache-azure-spark-history-server/sparkui-graph-zoom2fit.png)

+ Mantenga el mouse sobre el nodo del gráfico para ver la información sobre herramientas cuando hay tareas con error, y haga clic en la fase para abrir la página correspondiente.

    ![información sobre herramientas del gráfico](./media/apache-azure-spark-history-server/sparkui-graph-tooltip.png)

+ El nodo del gráfico de trabajos mostrará la siguiente información de cada fase:
    + Identificador.
    + Nombre o descripción.
    + Número total de la tarea.
    + Datos leídos: la suma del tamaño de entrada y el tamaño de lectura aleatorio.
    + Datos escritos: la suma del tamaño de salida y el tamaño de escritura aleatorio.
    + Tiempo de ejecución: el tiempo entre la hora de inicio del primer intento y la hora de finalización del último intento.
    + Recuento de filas: la suma de registros de entrada, registros de salida, registros de lectura aleatorios y registros de escritura aleatorios.
    + Progreso.

    > [!NOTE]
    > De forma predeterminada, el nodo del gráfico de trabajos muestra información desde el último intento de cada fase (excepto el tiempo de ejecución de la fase), pero durante el nodo del gráfico de reproducción muestra información de cada intento.

    > [!NOTE]
    > Para el tamaño de los datos de lectura y escritura se usa 1 MB = 1000 KB = 1000 * 1000 bytes.

+ Haga clic en **Provide us feedback** (Proporcione sus comentarios) para enviar comentarios sobre problemas.

    ![Comentarios de gráfico](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)


## <a name="faq"></a>Preguntas más frecuentes

### <a name="1-revert-to-community-version"></a>1. Revertir a la versión de la comunidad

Para revertir a la versión de la comunidad, realice los pasos siguientes:

1. Abra el clúster en Ambari. Haga clic en **Spark2** en el panel izquierdo.
2. Haga clic en la pestaña **Configs** (Configuraciones).
3. Expanda el grupo **Custom spark2-defaults** (Valores predeterminados personalizados de Spark2).
4. Haga clic en **Add Property** (Agregar propiedad), agregue **spark.ui.enhancement.enabled=false** y guarde.
5. La propiedad se establece ahora en **false**.
6. Para guardar la configuración, haga clic en **Guardar**.

    ![La característica se desactiva](./media/apache-azure-spark-history-server/sparkui-turn-off.png)

7. Haga clic en **Spark2** en el panel izquierdo y, en la pestaña **Summary** (Resumen), haga clic en **Spark2 History Server** (Servidor de historial de Spark2).

    ![Reinicio del servidor 1](./media/apache-azure-spark-history-server/sparkui-restart-1.png) 

8. Haga clic en **Restart** (Reiniciar) en **Spark2 History Server** (Servidor de historial de Spark2) para reiniciar el servidor de historial.

    ![Reinicio del servidor 2](./media/apache-azure-spark-history-server/sparkui-restart-2.png)  

9. Actualice la interfaz de usuario web del Servidor de historial de Spark y volverá a la versión de la comunidad.

### <a name="2-upload-history-server-event"></a>2. Cargar los eventos del servidor de historial

Si experimenta errores en el servidor de historial, siga los pasos para proporcionar el evento:
1. Haga clic en **Download** (Descargar) en la interfaz de usuario web del servidor de historial para descargar el evento.

    ![Descarga de evento](./media/apache-azure-spark-history-server/sparkui-download-event.png)

2. Haga clic en **Provide us feedback** (Proporcione sus comentarios) en la pestaña de datos o de gráfico.

    ![Comentarios de gráfico](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

3. Proporcione el título y la descripción del error, arrastre el archivo ZIP al campo de edición y luego haga clic en **Submit new issue** (Enviar nuevo problema).

    ![Archivo de problema](./media/apache-azure-spark-history-server/sparkui-file-issue.png)


### <a name="3-upgrade-jar-file-for-hotfix-scenario"></a>3. Actualizar el archivo JAR para el escenario de revisión

Si quiere actualizar con la revisión, use el siguiente script que actualizará el archivo spark-enhancement.jar.

**upgrade_spark_enhancement.sh**:

   ```bash
    #!/usr/bin/env bash

    # Copyright (C) Microsoft Corporation. All rights reserved.

    # Arguments:
    # $1 Enhancement jar path

    if [ "$#" -ne 1 ]; then
        >&2 echo "Please provide the upgrade jar path."
        exit 1
    fi

    install_jar() {
        tmp_jar_path="/tmp/spark-enhancement-hotfix-$( date +%s )"

        if wget -O "$tmp_jar_path" "$2"; then
            for FILE in "$1"/spark-enhancement*.jar
            do
                back_up_path="$FILE.original.$( date +%s )"
                echo "Back up $FILE to $back_up_path"
                mv "$FILE" "$back_up_path"
                echo "Copy the hotfix jar file from $tmp_jar_path   to $FILE"
                cp "$tmp_jar_path" "$FILE"

                "Hotfix done."
                break
            done
        else    
            >&2 echo "Download jar file failed."
            exit 1
        fi
    }

    jars_folder="/usr/hdp/current/spark2-client/jars"
    jar_path=$1

    if ls ${jars_folder}/spark-enhancement*.jar 1>/dev/null 2>&1;   then
        install_jar "$jars_folder" "$jar_path"
    else
        >&2 echo "There is no target jar on this node. Exit with no action."
        exit 0
    fi
   ```

**Uso** 

`upgrade_spark_enhancement.sh https://${jar_path}`

**Ejemplo**:

`upgrade_spark_enhancement.sh https://${account_name}.blob.core.windows.net/packages/jars/spark-enhancement-${version}.tgz` 

**Para usar el archivo de Bash desde Azure Portal**

1. Inicie [Azure Portal](https://ms.portal.azure.com) y seleccione el clúster.
2. Haga clic en **Script actions** (Acciones de script) y luego en **Submit new** (Enviar ahora). Rellene el formulario **Enviar acción de script** y haga clic en el botón **Create** (Crear).
    
    + **Tipo de script**: seleccione **Personalizado**.
    + **Nombre**: especifique el nombre del script.
    + **URI de script de Bash**: cargue el archivo de Bash en el clúster privado y luego copie aquí la dirección URL. Como alternativa, use el URI proporcionado.
    
   ```upgrade_spark_enhancement
    https://hdinsighttoolingstorage.blob.core.windows.net/shsscriptactions/upgrade_spark_enhancement.sh
   ```

    + Marque **Principal** y **Trabajo**.
    + **Parámetros**: establezca los siguientes parámetros de uso de Bash.

    ![Cargar registro o actualizar revisión](./media/apache-azure-spark-history-server/sparkui-upload2.png)


## <a name="known-issue"></a>Problema conocido

1.  Actualmente, solo funciona para el clúster de Spark 2.3.

2.  Los datos de entrada y salida que usan RDD no se muestran en la pestaña de datos.

## <a name="next-steps"></a>Pasos siguientes

* [Administración de recursos de un clúster Spark en HDInsight](apache-spark-resource-manager.md)
* [Configuración de opciones de Spark](apache-spark-settings.md)


## <a name="contact-us"></a>Ponerse en contacto con nosotros

Si tiene algún comentario o si experimenta algún problema al usar esta herramienta, envíe un correo electrónico a ([hdivstool@microsoft.com](mailto:hdivstool@microsoft.com)).
