---
title: Introducción a los análisis de registros de Azure Monitor | Microsoft Docs
description: En este artículo se proporciona un tutorial de uso de Log Analytics en Azure Portal para escribir consultas.
ms.subservice: logs
ms.topic: tutorial
author: bwren
ms.author: bwren
ms.date: 07/19/2019
ms.openlocfilehash: 1cf1695db50e6aee2a5dae24ed5231fdda7c12de
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2020
ms.locfileid: "77670243"
---
# <a name="get-started-with-log-analytics-in-azure-monitor"></a>Introducción a los análisis de registros de Azure Monitor

> [!NOTE]
> Puede trabajar en este ejercicio en su propio entorno si va a recopilar datos de al menos una máquina virtual. Si no es así, use nuestro [entorno de demostración](https://portal.loganalytics.io/demo), que incluye gran cantidad de datos de ejemplo.

En este tutorial aprenderá a usar los análisis de registros de Azure Portal para escribir consultas de registro de Azure Monitor. Le mostrará cómo:

- Usar los análisis de registros para escribir una consulta sencilla
- Comprender el esquema de los datos
- Filtrar, ordenar y agrupar los resultados
- Aplicar intervalos de tiempo
- Crear gráficos
- Guardar y cargar consultas
- Exportar y compartir consultas

Para ver un tutorial sobre cómo escribir consultas de registro, consulte [Introducción a las consultas de registro en Azure Monitor](get-started-queries.md).<br>
Para más información sobre las consultas de registros, consulte [Introducción a las consultas de registros en Azure Monitor](log-query-overview.md).

## <a name="meet-log-analytics"></a>Descubrir los análisis de registros
Los análisis de registros son una herramienta web que se usa para escribir y ejecutar consultas de registros en Azure Monitor. Seleccione **Registros** en el menú de Azure Monitor para abrirla. Se inicia con una nueva consulta en blanco.

![Página de inicio](media/get-started-portal/homepage.png)

## <a name="firewall-requirements"></a>Requisitos de firewall
Para usar análisis de registros, el explorador requiere acceso a las direcciones siguientes. Si el explorador tiene acceso a Azure Portal a través de un firewall, debe habilitar el acceso a estas direcciones.

| Identificador URI | IP | Puertos |
|:---|:---|:---|
| portal.loganalytics.io | Dinámica | 80 443 |
| api.loganalytics.io | Dinámica | 80 443 |
| docs.loganalytics.io | Dinámica | 80 443 |

## <a name="basic-queries"></a>Consultas básicas
Las consultas se pueden usar para buscar términos, identificar tendencias o analizar patrones y proporcionan muchas otras conclusiones basadas en los datos. Comencemos con una consulta básica:

```Kusto
Event | search "error"
```

Esta consulta busca en la tabla _Event_ los registros que contienen el término _error_ en cualquier propiedad.

Las consultas pueden comenzar por un nombre de tabla o un comando [search](/azure/kusto/query/searchoperator). El ejemplo anterior comienza con el nombre de tabla _Event_, que recupera todos los registros de la tabla de eventos. El carácter de barra vertical (|) separa los comandos,de manera que la salida del primero sea la entrada del siguiente. A una consulta se puede agregar cualquier cantidad de comandos.

Otra forma de escribir la misma consulta sería:

```Kusto
search in (Event) "error"
```

En este ejemplo, **search** tiene como ámbito la tabla _Event_ y el término _error_ se busca en todos sus registros.

## <a name="running-a-query"></a>Ejecución de una consulta
Las consultas se ejecutan al hacer clic en el botón **Run** (Ejecutar) o al presionar **Mayús + Intro**. Tenga en cuenta los siguientes detalles que determinan el código que se va a ejecutar y los datos que se van a devolver:

- Saltos de línea: Un único salto facilita la lectura de la consulta. Varios saltos de línea la dividen en consultas independientes.
- Cursor: coloque el cursor en algún lugar dentro de la consulta para ejecutarla. Se considera que la consulta actual está compuesta por el código hasta que aparece una línea en blanco.
- Intervalo de tiempo: de forma predeterminada, se establece uno de las _últimas 24 horas_. Con el selector de hora puede elegir un intervalo distinto. Como alternativa, agregue un filtro de intervalo de tiempo a la consulta.


## <a name="understand-the-schema"></a>Información sobre el esquema
El esquema es una colección de tablas que visualmente se agrupan en una categoría lógica. Varias categorías provienen de las soluciones de supervisión. La categoría _LogManagement_ contiene datos comunes, como eventos de Windows y Syslog, datos de rendimiento y latidos de cliente.

![Schema](media/get-started-portal/schema.png)

En cada tabla, los datos se organizan en columnas con tipos de datos diferentes, como indican los iconos junto al nombre de la columna. Por ejemplo, la tabla _Event_ que se muestra en la captura de pantalla contiene columnas como _Computer_ (texto), _EventCategory_ (cifra) y  _TimeGenerated_ (fecha y hora).

## <a name="filter-the-results"></a>Filtrado de los resultados
Comencemos por obtener todo el contenido de la tabla _Event_.

```Kusto
Event
```

Los análisis de registro establecen automáticamente el ámbito de los resultados por:

- Intervalo de tiempo:  de forma predeterminada, las consultas se limitan a las últimas 24 horas.
- Número de resultados: limitado a 10 000 registros como máximo.

Esta consulta es muy general y devuelve demasiados resultados para ser útil. Puede filtrar los resultados a través de los elementos de la tabla o al agregar explícitamente un filtro a la consulta. El filtro de los resultados de los elementos de tabla se aplica al conjunto de resultados existente, mientras que un filtro en la propia consulta devolverá un nuevo conjunto de resultados filtrados y, por tanto, los resultados serían más precisos.

### <a name="add-a-filter-to-the-query"></a>Incorporación de un filtro a la consulta
Cada registro tiene una flecha a la izquierda. Haga clic en ella para abrir los detalles del registro en concreto.

Mantenga el mouse por encima de un nombre de columna para que aparezcan los iconos "+" y "-". Para agregar un filtro que devuelva únicamente los registros con el mismo valor, haga clic en el signo "+". Haga clic en "-" para excluir los registros con este valor y, a continuación, haga clic en **Run** (Ejecutar) para volver a ejecutar la consulta.

![Agregar filtro a consulta](media/get-started-portal/add-filter.png)

### <a name="filter-through-the-table-elements"></a>Filtro en los elementos de la tabla
Ahora vamos a centrarnos en los eventos con gravedad _Error_. Esto se especifica en una columna denominada _EventLevelName_. Deberá desplazarse hacia la derecha para ver esta columna.

Haga clic en el icono de filtro junto al título de columna y, en la ventana emergente, seleccione los valores _Start with_ (Empieza por) y el texto _error_:

![Filter](media/get-started-portal/filter.png)


## <a name="sort-and-group-results"></a>Filtrado y agrupación de los resultados
Los resultados ahora se reducen para incluir solo los eventos de error de SQL Server generados en las últimas 24 horas. Sin embargo, los resultados no se ordenan. Para ordenar los resultados por una columna específica, como por ejemplo _timestamp_, haga clic en el título de la columna. Con un solo clic se ordena en orden ascendente, mientras que un segundo clic el orden será descendente.

![Ordenación de la columna](media/get-started-portal/sort-column.png)

Otra manera de organizar los resultados es por grupos. Para agrupar los resultados por una columna específica, simplemente arrastre el encabezado de columna por encima de las demás. Para crear subgrupos, arrastre otras columnas a la barra superior también.

![Grupos](media/get-started-portal/groups.png)

## <a name="select-columns-to-display"></a>Selección de columnas para mostrar
La tabla de resultados incluye a menudo una gran cantidad de columnas. Es posible que algunas de las columnas devueltas no se muestren de forma predeterminada o que desee quitar algunas de las que se muestran. Para seleccionar las columnas que desea mostrar, haga clic en el botón Columns (Columnas):

![Select columns](media/get-started-portal/select-columns.png)


## <a name="select-a-time-range"></a>Selección de un intervalo de tiempo
De manera predeterminada, en los análisis de registros se aplica el intervalo de tiempo de _últimas 24 horas_. Para usar un intervalo diferente, seleccione otro valor mediante el selector de hora y haga clic en **Run** (Ejecutar). Además de los valores preestablecidos, puede usar la opción de _intervalo de tiempo personalizado_ para seleccionar un intervalo absoluto para la consulta.

![Selector de hora](media/get-started-portal/time-picker.png)

Al seleccionar un intervalo de tiempo personalizado, con los valores seleccionados se utiliza la zona horaria UTC, que podría diferir de la suya.

Si la consulta contiene explícitamente un filtro para _TimeGenerated_, en el título del selector de hora mostrará _Set in query_ (Establecer en consulta). La selección manual se deshabilitará para evitar conflictos.


## <a name="charts"></a>Gráficos
Además de devolver los resultados de consulta en una tabla, se pueden presentar en formatos visuales. Use la siguiente consulta como ejemplo:

```Kusto
Event 
| where EventLevelName == "Error" 
| where TimeGenerated > ago(1d) 
| summarize count() by Source 
```

De forma predeterminada, los resultados se muestran en una tabla. Haga clic en _Chart_ (Gráfico) para verlos en un gráfico:

![Gráfico de barras](media/get-started-portal/bar-chart.png)

Los resultados se muestran en un gráfico de barras apiladas. Haga clic en _Stacked Column_ (Columna apilada) y seleccione _Pie_ (Gráfico circular) para mostrar otra vista de los resultados:

![Gráfico circular](media/get-started-portal/pie-chart.png)

Las diferentes propiedades de la vista, como los ejes x e y, o las preferencias de agrupación y división, se pueden cambiar manualmente en la barra de control.

También puede establecer la vista que prefiera en la propia consulta mediante el operador de representación.

### <a name="smart-diagnostics"></a>Smart Diagnostics
En un gráfico de tiempo, si se produce un pico o salto repentino en los datos, es posible que vea un punto en la línea resaltado. Esto indica que _Smart Diagnostics_ ha identificado una combinación de propiedades que filtran los cambios repentinos. Haga clic en el punto para obtener más detalles del filtro y para ver la versión filtrada. Esto puede ayudar a identificar lo que ha causado el cambio:

![Smart Diagnostics](media/get-started-portal/smart-diagnostics.png)

## <a name="pin-to-dashboard"></a>Anclar al panel
Para anclar un diagrama o una tabla a uno de los paneles compartidos, haga clic en el icono de la chincheta. Tenga en cuenta que este icono se ha movido a la parte superior de la ventana de Log Analytics, diferente de la captura de pantalla mostrada a continuación.

![Anclar al panel](media/get-started-portal/pin-dashboard.png)

A un gráfico se le aplican determinadas simplificaciones se le ancla a un panel:

- Columnas y filas de la tabla: para anclar una tabla al panel, debe tener cuatro columnas como máximo. Se muestran solo las siete filas superiores.
- Restricción de tiempo: las consultas se limitan de manera automática a los últimos 14 días.
- Restricción de recuento de intervalos: si se muestra un gráfico con numerosos intervalos discretos, los intervalos menos poblados se agrupan automáticamente en un solo intervalo denominado _others_.

## <a name="save-queries"></a>Guardado de consultas
Una vez creada una consulta útil, puede guardarla o compartirla. El icono **Save** (Guardar) se encuentra en la barra superior.

Puede guardar la página de consulta completa o una sola consulta como función. Las funciones son consultas a las que también pueden hacer referencia otras consultas. Para guardar una consulta como función debe proporcionar un alias de función, que es el nombre que se utiliza para llamar a esta consulta cuando otras hagan referencia a ella.

![Función Guardar](media/get-started-portal/save-function.png)

>[!NOTE]
>Se admiten estas características `a–z, A–Z, 0-9, -, _, ., <space>, (, ), |` en el campo **Nombre** al guardar o editar la consulta guardada.

Las consultas de Log Analytics se guardan siempre en un área de trabajo determinada y se comparten con otros usuarios de esa área de trabajo.

## <a name="load-queries"></a>Carga de consultas
El icono de Explorador de consultas se encuentra en la parte superior derecha. Enumera todas las consultas guardadas por categoría. También permite marcar consultas específicas como favoritas para encontrarlas rápidamente en el futuro. Haga doble clic en una consulta guardada para agregarla a la ventana actual.

![Explorador de consultas](media/get-started-portal/query-explorer.png)

## <a name="export-and-share-as-link"></a>Exportación y vínculo compartido
Los análisis de registros admiten varios métodos de exportación:

- Excel: los resultados se guardan en un archivo CSV.
- Power BI: los resultados se exportan a Power BI. Consulte [Importación de datos de registro de Azure Monitor en Power BI](../../azure-monitor/platform/powerbi.md) para obtener más detalles.
- Compartir un vínculo: la propia consulta se puede compartir como vínculo y enviárselo a otros usuarios de esa área de trabajo para que estos la ejecuten.

## <a name="next-steps"></a>Pasos siguientes

- Más información sobre la [escritura de consultas de registro en Azure Monitor](get-started-queries.md).
