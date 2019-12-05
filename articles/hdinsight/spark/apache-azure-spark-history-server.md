---
title: 'Servidor de historial de Spark extendido para depurar aplicaciones: Azure HDInsight'
description: 'Use el Servidor de historial de Spark extendido para depurar y diagnosticar aplicaciones Spark: Azure HDInsight.'
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive,hdiseo17may2017
ms.date: 11/25/2019
ms.openlocfilehash: 7e9ab0e41086a4c9478f95c5a56754640feeab4e
ms.sourcegitcommit: c31dbf646682c0f9d731f8df8cfd43d36a041f85
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2019
ms.locfileid: "74561823"
---
# <a name="use-extended-apache-spark-history-server-to-debug-and-diagnose-apache-spark-applications"></a>Uso del servidor de historial de Apache Spark extendido para depurar y diagnosticar las aplicaciones de Spark

En este artículo se proporcionan instrucciones sobre cómo usar el Servidor de historial de Apache Spark para depurar y diagnosticar las aplicaciones Spark en ejecución y completadas. La extensión incluye la pestaña de datos, la pestaña de gráfico y la pestaña de diagnóstico. En la pestaña **Data** (Datos), los usuarios pueden comprobar los datos de entrada y salida del trabajo de Spark. En la pestaña **Graph** (Gráfico), los usuarios pueden comprobar el flujo de datos y reproducir el gráfico del trabajo. En la pestaña **Diagnóstico**, el usuario puede consultar la **Asimetría de datos**, el **Desfase horario** y el **Análisis de uso del ejecutor**.

## <a name="get-access-to-apache-spark-history-server"></a>Acceso al servidor de historial de Apache Spark

El servidor de historial de Apache Spark es la interfaz de usuario web para aplicaciones de Spark completadas y en ejecución.

### <a name="open-the-apache-spark-history-server-web-ui-from-azure-portal"></a>Apertura de la interfaz de usuario web del servidor de historial de Apache Spark desde Azure Portal

1. En [Azure Portal](https://portal.azure.com/), abra el clúster de Spark. Para obtener más información, consulte [Enumeración y visualización de clústeres](../hdinsight-administer-use-portal-linux.md#showClusters).
2. En **Paneles de clúster**, seleccione **Servidor de historial de Spark**. Cuando se le pida, escriba las credenciales de administrador del clúster Spark.

    ![Inicio del portal del servidor de historial de Spark](./media/apache-azure-spark-history-server/azure-portal-dashboard-spark-history.png "Servidor de historial de Spark")

### <a name="open-the-spark-history-server-web-ui-by-url"></a>Apertura de la interfaz de usuario web del Servidor de historial de Spark por la dirección URL

Abra el Servidor de historial de Spark; para ello, vaya a `https://CLUSTERNAME.azurehdinsight.net/sparkhistory`, donde CLUSTERNAME es el nombre del clúster de Spark.

La interfaz de usuario web del Servidor de historial de Spark puede tener un aspecto similar al siguiente:

![Servidor de historial de HDInsight Spark](./media/apache-azure-spark-history-server/hdinsight-spark-history-server.png)

## <a name="data-tab-in-spark-history-server"></a>Pestaña de Datos en el Servidor de historial de Spark

Seleccione el identificador del trabajo y luego **Datos** en el menú de herramientas para obtener la vista de datos.

+ Revise por separado las pestañas **Entradas**, **Salidas** y **Operaciones de tabla**.

    ![Datos de aplicaciones Spark: pestañas](./media/apache-azure-spark-history-server/apache-spark-data-tabs.png)

+ Seleccione el botón **Copiar** para copiar todas las filas.

    ![Datos de la aplicación Spark: copiar](./media/apache-azure-spark-history-server/apache-spark-data-copy.png)

+ Seleccione el botón **csv** para guardar todos los datos como un archivo CSV.

    ![Datos de la aplicación Spark: guardar](./media/apache-azure-spark-history-server/apache-spark-data-save.png)

+ En el campo **Search** (Buscar), escriba las palabras clave; el resultado de búsqueda se muestra inmediatamente.

    ![Datos de la aplicación Spark: buscar](./media/apache-azure-spark-history-server/apache-spark-data-search.png)

+ Seleccione el encabezado de columna para ordenar la tabla, seleccione el signo más para expandir una fila y mostrar más detalles o seleccione el signo menos para contraer una fila.

    ![Datos de la aplicación Spark: tabla](./media/apache-azure-spark-history-server/apache-spark-data-table.png)

+ Seleccione el botón **Descarga parcial** situado a la derecha para descargar un único archivo; el archivo seleccionado se descarga en el entorno local. Si el archivo ya no existe, se abrirá una nueva pestaña para mostrar los mensajes de error.

    ![Datos de la aplicación Spark: descargar fila](./media/apache-azure-spark-history-server/sparkui-data-download-row.png)

+ Seleccione las opciones **Copy Full Path** (Copiar ruta de acceso completo) o **Copy Relative Path** (Copiar ruta de acceso relativa) que se expanden desde el menú de descarga para copiar una de estas rutas. Para los archivos de Azure Data Lake, **Abrir en Explorador de Azure Storage** iniciará Azure Storage Explorer y buscará la carpeta al iniciar sesión.

    ![Datos de la aplicación Spark: copiar ruta de acceso](./media/apache-azure-spark-history-server/sparkui-data-copy-path.png)

+ Seleccione el número situado debajo de la tabla para desplazarse por las páginas cuando en una página se muestran demasiadas filas.

    ![Datos de la aplicación Spark: página](./media/apache-azure-spark-history-server/apache-spark-data-page.png)

+ Mantenga el mouse sobre el signo de interrogación junto a Datos para mostrar la información sobre herramientas, o seleccione el signo de interrogación para obtener más información.

    ![Datos de la aplicación Spark: más información](./media/apache-azure-spark-history-server/sparkui-data-more-info.png)

+ Haga clic en **Provide us feedback** (Proporcione sus comentarios) para enviar comentarios sobre problemas.

    ![El gráfico de Spark proporciona de nuevo comentarios](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

## <a name="graph-tab-in-apache-spark-history-server"></a>Pestaña Gráfico en el servidor de historial de Apache Spark

Seleccione el identificador del trabajo y haga clic en **Graph** (Gráfico) en el menú de herramientas para obtener la vista gráfica del trabajo.

+ Puede revisar la información general del trabajo mediante el gráfico generado.

+ De forma predeterminada, muestran todos los trabajos, y se pueden filtrar por **Job ID**(Id. del trabajo).

    ![Aplicación Spark y gráfico de trabajos: identificador del trabajo](./media/apache-azure-spark-history-server/apache-spark-graph-jobid.png)

+ De forma predeterminada, se selecciona **Progress** (Progreso), pero el usuario puede consultar el flujo de datos si selecciona **Read/Written** (leídos o escritos) en la lista desplegable de **Display** (Mostrar).

    ![Aplicación Spark y gráfico de trabajos: mostrar](./media/apache-azure-spark-history-server/sparkui-graph-display.png)

    La visualización de los nodos del gráfico en color que muestra el mapa térmico.

    ![Aplicación Spark y gráfico de trabajos: mapa térmico](./media/apache-azure-spark-history-server/sparkui-graph-heatmap.png)

+ Seleccione el botón **Reproducir** para reproducir el trabajo o Detener para detenerlo en cualquier momento. La visualización de tareas en color para mostrar diferentes estados durante la reproducción:

    |Color |DESCRIPCIÓN |
    |---|---|
    |Verde|El trabajo se completó correctamente.|
    |Naranja|Las instancias de tareas que no se han podido realizar pero que no afectan al resultado final del trabajo. Estas tareas tenían instancias duplicadas o que se volvieron a intentar que podrían completarse con éxito más tarde.|
    |Azul|La tarea se está ejecutando.|
    |Blanco|La tarea está esperando a ejecutarse o se ha omitido la fase.|
    |Rojo|Error en la tarea.|

    ![Aplicación Spark y gráfico de trabajos: ejemplo de color, en ejecución](./media/apache-azure-spark-history-server/sparkui-graph-color-running.png)

    La fase omitida se muestra en blanco.
    ![Aplicación Spark y gráfico de trabajos: ejemplo de color, omitir](./media/apache-azure-spark-history-server/sparkui-graph-color-skip.png)

    ![Aplicación Spark y gráfico de trabajos: ejemplo de color, error](./media/apache-azure-spark-history-server/sparkui-graph-color-failed.png)

    > [!NOTE]  
    > Se permite la reproducción de cada trabajo. No se admite la reproducción de un trabajo incompleto.

+ Desplace el mouse para acercar o alejar el gráfico del trabajo, o haga clic en **Zoom to fit** (Zoom para ajustar) para ajustarlo a la pantalla.

    ![Aplicación Spark y gráfico de trabajos: zoom para ajustar](./media/apache-azure-spark-history-server/sparkui-graph-zoom2fit.png)

+ Mantenga el mouse sobre el nodo del gráfico para ver la información sobre herramientas cuando hay tareas con error, y haga clic en la fase para abrir la página correspondiente.

    ![Aplicación Spark y gráfico de trabajos: información sobre herramientas](./media/apache-azure-spark-history-server/sparkui-graph-tooltip.png)

+ En la pestaña del gráfico del trabajo, las fases tendrán información sobre herramientas y un icono pequeño si tienen tareas que cumplen las siguientes condiciones:
  + Asimetría de datos: tamaño de lectura de datos > tamaño promedio de lectura de datos de todas las tareas de esta fase * 2 y tamaño de lectura de datos > 10 MB.
  + Desfase horario: tiempo de ejecución > tiempo de ejecución promedio de todas las tareas de esta fase * 2 y tiempo de ejecución > 2 minutos.

    ![Aplicación Spark y gráfico de trabajos: icono de asimetría](./media/apache-azure-spark-history-server/sparkui-graph-skew-icon.png)

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

+ Seleccione **Proporcione sus comentarios** para enviar comentarios sobre incidencias.

    ![Aplicación Spark y gráfico de trabajos: comentarios](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

## <a name="diagnosis-tab-in-apache-spark-history-server"></a>Pestaña Diagnóstico en el servidor de historial de Apache Spark

Seleccione el identificador del trabajo y luego **Diagnóstico** en el menú de herramientas para obtener la vista de diagnóstico del trabajo. La pestaña de diagnóstico incluye **Data Skew** (Asimetría de datos), **Time Skew** (Desfase horario) y **Executor Usage Analysis** (Análisis de uso del ejecutor).

+ Seleccione las pestañas correspondientes para revisar la **Asimetría de datos**, el **Desfase horario** y el **Análisis de uso del ejecutor**.

    ![Diagnóstico de la interfaz de usuario de Spark: de nuevo la pestaña de asimetría de datos](./media/apache-azure-spark-history-server/sparkui-diagnosis-tabs.png)

### <a name="data-skew"></a>Asimetría de datos

Seleccione la pestaña **Asimetría de datos** para mostrar las tareas con desfase según los parámetros especificados.

+ **Especificar parámetros**: la primera sección muestra los parámetros que se usan para detectar la Asimetría de datos. La regla integrada es: La lectura de datos de tarea es tres veces mayor que la lectura de datos de tarea promedio y la lectura de datos de tarea es mayor que 10 MB. Si desea definir su propia regla para las tareas con desfase, puede elegir los parámetros y las secciones **Skewed Stage** (Fase sesgada) y **Skew Chart** (Gráfico de desfase) se actualizarán según corresponda.

+ **Fase sesgada**: la segunda sección muestra las fases que tienen tareas con desfase que cumplen los criterios especificados anteriormente. Si hay más de una tarea con desfase en una fase, la tabla de fases sesgadas muestra solo la tarea más sesgada (por ejemplo, los datos más grandes con asimetría de datos).

    ![Diagnóstico de la interfaz de usuario de Spark: pestaña de asimetría de datos](./media/apache-azure-spark-history-server/sparkui-diagnosis-dataskew-section2.png)

+ **Skew Chart** (Gráfico de desfase): cuando se selecciona una fila en la tabla de fases sesgadas, el gráfico de desfase muestra más detalles de las distribuciones de tareas basados en los datos de lectura y el tiempo de ejecución. Las tareas con desfase se marcan en rojo y las tareas normales se marcan en azul. Por motivos de rendimiento, el gráfico solo muestra hasta 100 tareas de ejemplo. Los detalles de la tarea se muestran en el panel inferior derecho.

    ![Interfaz de usuario de Spark: gráfico de desfase de la fase 10](./media/apache-azure-spark-history-server/sparkui-diagnosis-dataskew-section3.png)

### <a name="time-skew"></a>Desfase horario

La pestaña **Time Skew** (Desfase horario) muestra las tareas con desfase en función del tiempo de ejecución de la tarea.

+ **Especificar parámetros**: la primera sección muestra los parámetros que se usan para detectar el desfase horario. Los criterios predeterminados para detectar el desfase horario son: el tiempo de ejecución de la tarea es mayor que tres veces el tiempo de ejecución promedio y el tiempo de ejecución de la tarea es superior a 30 segundos. Puede cambiar los parámetros según sus necesidades. **Skewed Stage** (Fase sesgada) y **Skew Chart** (Gráfico de desfase) muestran la información de las tareas y fases correspondientes al igual que la pestaña **Data Skew** (Asimetría de datos) anterior.

+ Seleccione **Desfase horario** y, después, se muestran los resultados filtrados en la sección **Fase sesgada** según los parámetros establecidos en la sección **Especificar parámetros**. Seleccione un elemento de la sección **Fase sesgada** para mostrar el gráfico correspondiente en la sección 3 y los detalles de la tarea en el panel inferior derecho.

    ![Diagnóstico de la interfaz de usuario de Spark: sección de desfase horario](./media/apache-azure-spark-history-server/sparkui-diagnosis-timeskew-section2.png)

### <a name="executor-usage-analysis"></a>Análisis de uso del ejecutor

El gráfico de uso del ejecutor visualiza la asignación real del ejecutor del trabajo de Spark y el estado de ejecución.  

+ Seleccione **Análisis de uso del ejecutor** y se trazarán cuatro tipos de curvas sobre el uso del ejecutor, incluidas **Ejecutores asignados**, **Ejecutores en ejecución**, **Ejecutores sin uso** e **Instancias de ejecutor máximas**. Con respecto a los ejecutores asignados, cada evento "Ejecutor agregado" o "Ejecutor eliminado" aumentará o disminuirá los ejecutores asignados, puede comprobar "Event Timeline" (Escala de tiempo del evento) en la pestaña "Jobs" (Trabajos) para más comparativas.

    ![Diagnóstico de la interfaz de usuario de Spark: pestaña de ejecutores](./media/apache-azure-spark-history-server/sparkui-diagnosis-executors.png)

+ Seleccione el icono de color para seleccionar o anular la selección del contenido correspondiente en todos los borradores.

    ![Diagnóstico de la interfaz de usuario de Spark: seleccionar gráfico](./media/apache-azure-spark-history-server/sparkui-diagnosis-select-chart.png)

## <a name="faq"></a>Preguntas más frecuentes

### <a name="1-revert-to-community-version"></a>1. Revertir a la versión de la comunidad

Para revertir a la versión de la comunidad, realice los pasos siguientes:

1. Abra el clúster en Ambari.
1. Vaya a **Spark2** > **CONFIGS** > **Custom spark2-defaults**.
1. Seleccione **Agregar propiedad...** agregue **spark.ui.enhancement.enabled=false** y guarde.
1. La propiedad se establece ahora en **false**.
1. Para guardar la configuración, seleccione **Guardar**.

    ![La característica Apache Ambari se desactiva](./media/apache-azure-spark-history-server/apache-spark-turn-off.png)

1. Seleccione **Spark2** en el panel izquierdo y, en la pestaña **Resumen**, seleccione **Servidor de historial de Spark2**.

    ![Vista de resumen de Spark2 de Apache Ambari](./media/apache-azure-spark-history-server/apache-spark-restart1.png)

1. Seleccione **Reiniciar** en **Servidor de historial de Spark2** para reiniciar el servidor de historial.

    ![Reinicio del historial de Spark2 de Apache Ambari](./media/apache-azure-spark-history-server/apache-spark-restart2.png)  
1. Actualice la interfaz de usuario web del Servidor de historial de Spark y volverá a la versión de la comunidad.

### <a name="2-upload-history-server-event"></a>2. Cargar los eventos del servidor de historial

Si experimenta errores en el servidor de historial, siga los pasos para proporcionar el evento:

1. Seleccione **Descargar** en la interfaz de usuario web del servidor de historial para descargar el evento.

    ![Servidor de historial de Spark2: descargar](./media/apache-azure-spark-history-server/sparkui-download-event.png)

2. Seleccione **Proporcione sus comentarios** en la pestaña de datos o de gráfico.

    ![Gráfico de Spark: proporcione sus comentarios](./media/apache-azure-spark-history-server/sparkui-graph-feedback.png)

3. Proporcione el título y la descripción del error, arrastre el archivo ZIP al campo de edición y luego haga clic en **Submit new issue** (Enviar nuevo problema).

    ![Archivo de Apache Spark: ejemplo de problema](./media/apache-azure-spark-history-server/apache-spark-file-issue.png)

### <a name="3-upgrade-jar-file-for-hotfix-scenario"></a>3. Actualizar el archivo JAR para el escenario de revisión

Si quiere actualizar con la revisión, use el siguiente script que actualizará el archivo spark-enhancement.jar\*.

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

`upgrade_spark_enhancement.sh https://${account_name}.blob.core.windows.net/packages/jars/spark-enhancement-${version}.jar`

**Para usar el archivo de Bash desde Azure Portal**

1. Inicie [Azure Portal](https://ms.portal.azure.com) y seleccione el clúster.
2. Complete una [acción de script](../hdinsight-hadoop-customize-cluster-linux.md) con los parámetros siguientes:

    |Propiedad |Valor |
    |---|---|
    |Tipo de script|- Personalizado|
    |NOMBRE|UpgradeJar|
    |URI de script de Bash|`https://hdinsighttoolingstorage.blob.core.windows.net/shsscriptactions/upgrade_spark_enhancement.sh`|
    |Tipos de nodo|Principal, trabajo|
    |Parámetros|`https://${account_name}.blob.core.windows.net/packages/jars/spark-enhancement-${version}.jar`|

     ![Envío de acción de script en Azure Portal](./media/apache-azure-spark-history-server/apache-spark-upload1.png)

## <a name="known-issues"></a>Problemas conocidos

+ Actualmente, solo funciona para el clúster de Spark 2.3 y 2.4.

+ Los datos de entrada y salida que usan RDD no se mostrarán en la pestaña de datos.

## <a name="next-steps"></a>Pasos siguientes

+ [Administración de recursos de un clúster Apache Spark en HDInsight](apache-spark-resource-manager.md)
+ [Configuración de opciones de Apache Spark](apache-spark-settings.md)

## <a name="contact-us"></a>Ponerse en contacto con nosotros

Si tiene algún comentario o si experimenta alguna incidencia al usar esta herramienta, envíe un correo a ([hdivstool@microsoft.com](mailto:hdivstool@microsoft.com)).
