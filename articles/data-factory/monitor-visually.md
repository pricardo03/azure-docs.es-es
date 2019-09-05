---
title: Supervisión visual de las factorías de datos de Azure | Microsoft Docs
description: Aprenda a supervisar visualmente las factorías de datos de Azure
services: data-factory
documentationcenter: ''
author: djpmsft
ms.author: daperlov
manager: jroth
ms.reviewer: maghan
ms.service: data-factory
ms.workload: data-services
ms.topic: conceptual
ms.date: 01/19/2018
ms.openlocfilehash: 99ae0139d7b12c0bfb87030d2b749b12834a4f96
ms.sourcegitcommit: d200cd7f4de113291fbd57e573ada042a393e545
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2019
ms.locfileid: "70141095"
---
# <a name="visually-monitor-azure-data-factories"></a>Supervise visualmente las factorías de datos de Azure
Azure Data Factory es un servicio de integración de datos en la nube. Puede usarlo para crear flujos de trabajo controlados por datos para orquestar y automatizar tanto el movimiento de datos como la transformación de datos. Mediante Azure Data Factory puede:

- Creación y programación de flujos de trabajo basados en datos flujos (denominados "canalizaciones") que ingieren datos de almacenes de datos dispares.
- Procesar o transformar los datos mediante servicios de proceso como Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics y Azure Machine Learning.
- Publicación de datos de salida en almacenes de datos como Azure SQL Data Warehouse para que las aplicaciones de inteligencia empresarial los consuman.

En esta guía de inicio rápido, aprenderá a supervisar visualmente las canalizaciones de Data Factory sin escribir una sola línea de código.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="monitor-data-factory-pipelines"></a>Supervisión de las canalizaciones de Data Factory

Supervise la canalización y las ejecuciones de actividad con una simple interfaz de vista de lista. Todas las ejecuciones se muestran en la zona horaria local del explorador. Si cambia de zona horaria, todos los campos de fecha y hora se ajustan al que ha seleccionado.  

1. Inicie Microsoft Edge o Google Chrome. Actualmente, la interfaz de usuario de Data Factory solo es compatible con esos dos exploradores web.
2. Inicie sesión en [Azure Portal](https://portal.azure.com/).
3. Vaya a la hoja de la factoría de datos creada en Azure Portal. Seleccione el icono **Supervisión y administración** para iniciar la supervisión visual de Data Factory.

## <a name="monitor-pipeline-runs"></a>La supervisión de la canalización se ejecuta
La vista de lista muestra cada ejecución de las canalizaciones de Data Factory. Incluye estas columnas:

| **Nombre de la columna** | **Descripción** |
| --- | --- |
| Nombre de la canalización | Nombre de la canalización |
| Acciones | Acción individual disponible para ver ejecuciones de actividad |
| Hora de inicio de la ejecución | Fecha y hora de inicio de la ejecución de la canalización (MM/DD/AAAA, HH: MM: SS a.m./p.m.) |
| Duration | Duración de ejecución (HH) |
| Desencadenado por | Desencadenador manual o desencadenador programado |
| Status | **Error**, **Correcto** o **En curso** |
| Parámetros | Parámetros para la ejecución de la canalización (nombre/pares de valor) |
| Error | Error de ejecución de canalización (si hay) |
| Ejecutar identificador | Identificador de la ejecución de canalización |

![Vista de lista de supervisión de las ejecuciones de canalización](media/monitor-visually/pipeline-runs.png)

## <a name="monitor-activity-runs"></a>Supervisión de las ejecuciones de actividad
La vista de lista muestra ejecuciones de actividad correspondiente a cada ejecución de canalización. Para ver las ejecuciones de actividad de cada ejecución de canalización, seleccione el icono **Ejecuciones de actividad** de la columna **Acciones**. La vista de lista incluye estas columnas:

| **Nombre de la columna** | **Descripción** |
| --- | --- |
| Nombre de actividad | Nombre de la actividad dentro de la canalización |
| Tipo de actividad | El tipo de la actividad, como **Copia**, **HDInsightSpark** o **HDInsightHive** |
| Hora de inicio de la ejecución | Fecha y hora de inicio de la ejecución de actividad (MM/DD/AAAA, HH: MM: SS a.m./p.m.) |
| Duration | Duración de ejecución (HH) |
| Status | **Error**, **Correcto** o **En curso** |
| Entrada | Matriz JSON que describe las entradas de actividad |
| Output | Matriz JSON que describe las salidas de actividad |
| Error | Error en la ejecución de actividad (si lo hay) |

![Vista de lista para supervisar las ejecuciones de actividad](media/monitor-visually/activity-runs.png)

> [!IMPORTANT]
> Debe seleccionar el botón **Actualizar** en la parte superior para actualizar la lista de ejecuciones de actividad y de la canalización. La actualización automática no se admite actualmente.

![Botón Actualizar](media/monitor-visually/refresh.png)

## <a name="select-a-data-factory-to-monitor"></a>Selección de una factoría de datos para supervisar
Mantenga el puntero sobre el icono **Data Factory** de la parte superior izquierda. Seleccione el icono de flecha para ver una lista de factorías de datos y suscripciones de Azure que puede supervisar.

![Seleccionar la factoría de datos](media/monitor-visually/select-datafactory.png)

## <a name="configure-the-list-view"></a>Configuración de la vista de lista

### <a name="apply-rich-ordering-and-filtering"></a>Orden y filtrado enriquecidos

Ordene ejecuciones de la canalización en DESC/ASC en función de la hora de inicio de la ejecución. Filtre las ejecuciones de la canalización mediante las siguientes columnas:

| **Nombre de la columna** | **Descripción** |
| --- | --- |
| Nombre de la canalización | Nombre de la canalización. Las opciones incluyen filtros rápidos para las **últimas 24 horas**, la **última semana** y los **últimos 30 días**. O bien, seleccione una fecha y hora personalizadas. |
| Hora de inicio de la ejecución | Fecha y hora de inicio de la ejecución de la canalización. |
| Estado de ejecución | Filtrar ejecuciones por estado: **Correcto**, **Error** o **En curso**. |

![Opciones para filtrar](media/monitor-visually/filter.png)

### <a name="add-or-remove-columns"></a>Agregar o quitar columnas
Haga clic con el botón derecho en el encabezado de la vista de lista y elija las columnas que desee que aparezcan en la vista de lista.

![Opciones de las columnas](media/monitor-visually/columns.png)

### <a name="adjust-column-widths"></a>Ajustar el ancho de columna
Para aumentar y disminuir los anchos de columna en la vista de lista, mantenga el puntero sobre el encabezado de columna.

## <a name="promote-user-properties-to-monitor"></a>Promocionar las propiedades de usuario para supervisar

Cualquier propiedad de actividad de canalización se puede promover como propiedad de usuario para que se convierta en una entidad que pueda supervisar. Por ejemplo, puede promover las propiedades **Origen** y **Destino** de la actividad de copia de la canalización como propiedades de usuario. También puede seleccionar **Generar automáticamente** para generar las propiedades de usuario **Origen** y **Destino** de una actividad de copia.

![Creación de propiedades de usuario](media/monitor-visually/monitor-user-properties-image1.png)

> [!NOTE]
> Puede promover un máximo de cinco propiedades de la actividad de canalización como propiedades del usuario.

Después de crear las propiedades del usuario, puede supervisarlas en las vistas de lista de supervisión. Si el origen de la actividad de copia es un nombre de tabla, puede supervisar el nombre de la tabla de origen como una columna de la vista de lista en las ejecuciones de actividad.

![Lista de ejecuciones de actividad sin propiedades de usuario](media/monitor-visually/monitor-user-properties-image2.png)

![Agregar columnas para propiedades del usuario a la lista de ejecuciones de actividad](media/monitor-visually/monitor-user-properties-image3.png)

![Lista de ejecuciones de actividad con columnas de propiedades de usuario](media/monitor-visually/monitor-user-properties-image4.png)

## <a name="rerun-activities-inside-a-pipeline"></a>Volver a ejecutar las actividades en una canalización

Ahora puede volver a ejecutar las actividades en una canalización. Seleccione **Ver ejecuciones de actividad** y seleccione la actividad de la canalización a partir de la que desea volver a ejecutar la canalización.

![Ver ejecuciones de actividad](media/monitor-visually/rerun-activities-image1.png)

![Seleccionar una ejecución de actividad](media/monitor-visually/rerun-activities-image2.png)

### <a name="view-rerun-history"></a>Ver el historial de repetición de ejecuciones

Puede ver el historial de repetición de ejecuciones de todas las ejecuciones de canalización en la vista de lista.

![Visualización del historial](media/monitor-visually/rerun-history-image1.png)

También puede ver el historial de repetición de ejecuciones de una ejecución de canalización determinada.

![Ver el historial de ejecuciones de una canalización](media/monitor-visually/rerun-history-image2.png)

## <a name="gantt-views"></a>Vistas de Gantt

Use las vistas de Gantt para visualizar rápidamente las canalizaciones y ejecuciones de actividad. Puede consultar la vista de Gantt en función de la canalización o el grupo, mediante las anotaciones o etiquetas que haya creado en esas canalizaciones.

![Ejemplo de diagrama de Gantt](media/monitor-visually/gantt1.png)

![Anotaciones de diagrama de Gantt](media/monitor-visually/gantt2.png)

La longitud de la barra le informará de la duración de la canalización. Asimismo, puede seleccionar la barra para ver más detalles.

![Duración del diagrama de Gantt](media/monitor-visually/gantt3.png)

## <a name="guided-tours"></a>Visitas guiadas
Seleccione el icono de **información** de la esquina inferior izquierda. Después, seleccione **Visitas guiadas** para obtener instrucciones detalladas para supervisar las ejecuciones de actividad y de la canalización.

![Visitas guiadas](media/monitor-visually/guided-tours.png)

## <a name="feedback"></a>Comentarios
Seleccione el icono **Comentarios** para enviarnos comentarios acerca de las distintas características o de los problemas que puedan surgirle.

![Comentarios](media/monitor-visually/feedback.png)

## <a name="alerts"></a>Alertas

Puede generar alertas en función de métricas admitidas en Data Factory. Para empezar, seleccione **Supervisar** > **Alertas y métricas** en la página de supervisión de Data Factory.

![Página de supervisión de Data Factory](media/monitor-visually/alerts01.png)

Si desea una demostración y una introducción de siete minutos de esta característica, vea el siguiente vídeo:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Monitor-your-Azure-Data-Factory-pipelines-proactively-with-alerts/player]

### <a name="create-alerts"></a>Creación de alertas

1.  Seleccione **Nueva regla de alertas** para crear una alerta.

    ![Botón Nueva regla de alertas](media/monitor-visually/alerts02.png)

1.  Especifique el nombre de la regla y seleccione la gravedad de la alerta.

    ![Cuadros del nombre de la regla y de la gravedad](media/monitor-visually/alerts03.png)

1.  Seleccione los criterios de la alerta.

    ![Cuadro para los criterios de destino](media/monitor-visually/alerts04.png)

    ![Lista de criterios](media/monitor-visually/alerts05.png)

1.  Configure la lógica de la alerta. Puede crear una alerta de la métrica seleccionada para todas las canalizaciones y actividades correspondientes. También puede seleccionar un tipo de actividad, nombre de actividad, nombre de canalización o tipo de error determinados.

    ![Opciones para configurar la lógica de alerta](media/monitor-visually/alerts06.png)

1.  Configure las notificaciones push, por correo electrónico, SMS o voz de la alerta. Cree un grupo de acciones para las notificaciones de alerta o elija uno existente.

    ![Opciones para configurar notificaciones](media/monitor-visually/alerts07.png)

    ![Opciones para agregar una notificación](media/monitor-visually/alerts08.png)

1.  Cree la regla de alertas.

    ![Opciones para crear una regla de alertas](media/monitor-visually/alerts09.png)

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la supervisión y la administración de canalizaciones, consulte el artículo [Supervisión y administración de canalizaciones mediante programación](https://docs.microsoft.com/azure/data-factory/monitor-programmatically).
