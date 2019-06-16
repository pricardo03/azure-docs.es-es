---
title: Supervisión visual de las factorías de datos de Azure | Microsoft Docs
description: Aprenda a supervisar visualmente las factorías de datos de Azure
services: data-factory
documentationcenter: ''
author: sharonlo101
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/19/2018
ms.author: shlo
ms.openlocfilehash: df684860cd3d1b6a002a300682ca4c6398461ba6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60717073"
---
# <a name="visually-monitor-azure-data-factories"></a>Supervise visualmente las factorías de datos de Azure
Azure Data Factory es un servicio de integración de datos basado en la nube que le permite crear flujos de trabajo basados en datos en la nube a fin de coordinar y automatizar el movimiento y la transformación de datos. Mediante Azure Data Factory, puede crear y programar flujos de trabajo orientados a datos (llamados canalizaciones) que pueden ingerir datos de almacenes de datos dispares, procesar o transformar los datos mediante servicios de proceso, como Azure HDInsight Hadoop, Spark, Azure Data Lake Analytics y Azure Machine Learning, y publicar datos de salida en almacenes de datos, como Azure SQL Data Warehouse para que los consuman las aplicaciones de inteligencia empresarial (BI).

En esta guía de inicio rápido, aprenderá a supervisar visualmente las canalizaciones de Data Factory sin escribir una sola línea de código.

Si no tiene una suscripción a Azure, cree una cuenta [gratuita](https://azure.microsoft.com/free/) antes de empezar.

## <a name="monitor-data-factory-pipelines"></a>Supervisión de las canalizaciones de Data Factory

Supervise las ejecuciones de actividad y canalización con una simple interfaz de vista de lista. Se muestran todas las ejecuciones en la zona horaria local del explorador. Puede cambiar la zona horaria y todos los campos de fecha y hora se ajustarán a la seleccionada.  

1. Inicie el explorador web **Microsoft Edge** o **Google Chrome**. Actualmente, la interfaz de usuario de Data Factory solo se admite en los exploradores web Microsoft Edge y Google Chrome.
2. Inicie sesión en [Azure Portal](https://portal.azure.com/).
3. Vaya a la hoja de factoría de datos creada en Azure Portal y haga clic en el mosaico "Supervisar & Administrar" para iniciar la experiencia de supervisión visual de Data Factory.

## <a name="monitor-pipeline-runs"></a>La supervisión de la canalización se ejecuta
Vista de lista que muestra cada ejecución de canalización para las canalizaciones de la factoría de datos v2. Columnas incluidas:

| **Nombre de la columna** | **Descripción** |
| --- | --- |
| Nombre de la canalización | Nombre de la canalización. |
| Acciones | Acción única disponible para ver ejecuciones de la actividad. |
| Hora de inicio de la ejecución | Fecha y hora de inicio de la ejecución de canalización (MM/DD/AAAA HH: mm: SS a.m./p.m.) |
| Duration | Duración de ejecución (HH) |
| Desencadenado por | Desencadenador manual, desencadenador de programación |
| Status | No se pudo, Se realizó correctamente, En curso |
| Parámetros | Canalización de parámetros de ejecución (nombre, pares de valor) |
| Error | Error de ejecución de canalización (if/any) |
| Ejecutar identificador | Identificador de la ejecución de canalización |

![La supervisión de la canalización se ejecuta](media/monitor-visually/pipeline-runs.png)

## <a name="monitor-activity-runs"></a>Supervisión de las ejecuciones de actividad
Vista de lista muestra ejecuciones de actividad correspondiente a cada ejecución de canalización. Haga clic en el icono **'Se ejecuta la actividad'** bajo la columna **"Acciones"** para ver que la actividad se ejecuta para cada ejecución de canalización. Columnas incluidas:

| **Nombre de la columna** | **Descripción** |
| --- | --- |
| Nombre de actividad | Nombre de la actividad dentro de la canalización. |
| Tipo de actividad | Tipo de la actividad, como copia, HDInsightSpark, HDInsightHive, etc. |
| Hora de inicio de la ejecución | Hora y fecha de inicio de ejecución de actividad (MM/DD/AAAA HH:MM:SS a.m./p.m.) |
| Duration | Duración de ejecución (HH) |
| Status | No se pudo, Se realizó correctamente, En curso |
| Entrada | Matriz JSON que describe las entradas de actividad |
| Salida | Matriz JSON que describe las salidas de actividad |
| Error | Error de ejecución de actividad (if/any) |

![Supervisión de las ejecuciones de actividad](media/monitor-visually/activity-runs.png)

> [!IMPORTANT]
> Debe hacer clic en el icono **"Actualizar"** en la parte superior para actualizar la lista de canalizaciones y ejecuciones de actividad. La actualización automática no se admite actualmente.

![Actualizar](media/monitor-visually/refresh.png)

## <a name="select-a-data-factory-to-monitor"></a>Selección de una factoría de datos para supervisar
Mantenga el mouse sobre el icono **Data Factory** en la parte superior izquierda. Haga clic en el icono de 'Flecha' para ver una lista de factorías de datos y suscripciones de Azure que puede supervisar.

![Selección de la factoría de datos](media/monitor-visually/select-datafactory.png)

## <a name="configure-the-list-view"></a>Configuración de la vista de lista

### <a name="apply-rich-ordering-and-filtering"></a>Orden y filtrado enriquecidos

Las ejecuciones de canalización con orden se ejecutan de forma ascendente o descendente con Inicio de ejecución y las ejecuciones de las canalizaciones con filtro, por las columnas siguientes:

| **Nombre de la columna** | **Descripción** |
| --- | --- |
| Nombre de la canalización | Nombre de la canalización. Las opciones incluyen filtros rápidos para las 'últimas 24 horas', la 'última semana' y los 'últimos 30 días', o bien puede seleccionar una fecha y hora personalizadas. |
| Hora de inicio de la ejecución | Fecha y hora de inicio de la ejecución de canalización |
| Estado de ejecución | Ejecuciones con filtro por estado: correcto, con error, en curso |

![Filtrar](media/monitor-visually/filter.png)

### <a name="add-or-remove-columns"></a>Agregar o quitar columnas
Haga clic con el botón derecho en el encabezado de la vista de lista y elija las columnas que quiere que aparezcan en la vista de lista.

![Columnas](media/monitor-visually/columns.png)

### <a name="adjust-column-widths"></a>Ajustar el ancho de columna
Aumentar y disminuir los anchos de columna en la vista de lista simplemente manteniendo el puntero sobre el encabezado de columna

## <a name="promote-user-properties-to-monitor"></a>Promocionar las propiedades de usuario para supervisar

Cualquier propiedad de actividad de canalización se puede promover como propiedad de usuario para que se convierta en una entidad que pueda supervisar. Por ejemplo, puede promover las propiedades **Origen** y **Destino** de la actividad de copia de la canalización como propiedades de usuario. También puede seleccionar **Generar automáticamente** para generar las propiedades de usuario **Origen** y **Destino** de una actividad de copia.

![Creación de propiedades de usuario](media/monitor-visually/monitor-user-properties-image1.png)

> [!NOTE]
> Solo puede promocionar hasta 5 propiedades de actividad de canalización como propiedades de usuario.

Después de crear las propiedades de usuario, puede supervisarlas en las vistas de lista de supervisión. Si el origen de la actividad de copia es un nombre de tabla, puede supervisar el nombre de la tabla de origen como una columna de la vista de lista de ejecuciones de actividad.

![Lista de ejecuciones de actividad sin propiedades de usuario](media/monitor-visually/monitor-user-properties-image2.png)

![Agregar columnas para propiedades de usuario a la lista de ejecuciones de actividad](media/monitor-visually/monitor-user-properties-image3.png)

![Lista de ejecuciones de actividad con columnas de propiedades de usuario](media/monitor-visually/monitor-user-properties-image4.png)

## <a name="rerun-activities-inside-a-pipeline"></a>Volver a ejecutar las actividades en una canalización

Ahora puede volver a ejecutar las actividades en una canalización. Haga clic en **Ver ejecuciones de actividad** y seleccione la actividad en la canalización a partir de la cual quiere volver a ejecutar la canalización.

![Ver ejecuciones de actividad](media/monitor-visually/rerun-activities-image1.png)

![Seleccionar una ejecución de actividad](media/monitor-visually/rerun-activities-image2.png)

### <a name="view-rerun-history"></a>Ver el historial de repetición de ejecuciones

Puede ver el historial de repetición de ejecuciones de todas las ejecuciones de canalización en la vista de lista.

![Visualización del historial](media/monitor-visually/rerun-history-image1.png)

También puede ver el historial de repetición de ejecuciones de una ejecución de canalización determinada.

![Ver el historial de ejecuciones de una canalización](media/monitor-visually/rerun-history-image2.png)

## <a name="guided-tours"></a>Visitas guiadas
Haga clic en el 'Icono de información' en la parte inferior izquierda y haga clic en 'Visitas guiadas' para obtener instrucciones paso a paso sobre cómo supervisar las ejecuciones de actividad y canalización.

![Visitas guiadas](media/monitor-visually/guided-tours.png)

## <a name="feedback"></a>Comentarios
Haga clic en el icono "Comentarios" para enviarnos comentarios de varias características o de los problemas a los que quizás se enfrente.

![Comentarios](media/monitor-visually/feedback.png)

## <a name="alerts"></a>Alertas

Puede generar alertas en función de métricas admitidas en Data Factory. Seleccione **Supervisar -> Alertas y métricas**  en la página de supervisión de Data Factory.

![](media/monitor-visually/alerts01.png)

Si desea una demostración y una introducción de siete minutos de esta característica, vea el siguiente vídeo:

> [!VIDEO https://channel9.msdn.com/shows/azure-friday/Monitor-your-Azure-Data-Factory-pipelines-proactively-with-alerts/player]

### <a name="create-alerts"></a>Creación de alertas

1.  Haga clic en **Nueva regla de alertas**  para crear una nueva alerta.

    ![](media/monitor-visually/alerts02.png)

1.  Especifique el nombre de la regla y seleccione la alerta **Gravedad**.

    ![](media/monitor-visually/alerts03.png)

1.  Seleccione los criterios de la alerta.

    ![](media/monitor-visually/alerts04.png)

    ![](media/monitor-visually/alerts05.png)

1.  Configure la lógica de la alerta. Puede crear una alerta de la métrica seleccionada para todas las canalizaciones y actividades correspondientes. También puede seleccionar un tipo de actividad, nombre de actividad, nombre de canalización o tipo de error determinados.

    ![](media/monitor-visually/alerts06.png)

1.  Configure las notificaciones de **correo electrónico, SMS, push o de voz** de la alerta. Cree o elija un **grupo de acciones** existente para las notificaciones de alerta.

    ![](media/monitor-visually/alerts07.png)

    ![](media/monitor-visually/alerts08.png)

1.  Cree la regla de alertas.

    ![](media/monitor-visually/alerts09.png)

## <a name="next-steps"></a>Pasos siguientes

Consulte el artículo [Supervisión y administración de canalizaciones mediante programación](https://docs.microsoft.com/azure/data-factory/monitor-programmatically) para obtener información sobre la supervisión y administración de canalizaciones.
