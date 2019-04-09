---
title: Cree informes interactivos con libros de Azure Monitor | Microsoft Docs
description: Simplificar complejos informes con libros con parámetros predefinidos y personalizados para Azure Monitor para las máquinas virtuales.
services: azure-monitor
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: azure-monitor
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/05/2019
ms.author: magoedte
ms.openlocfilehash: 90c236347380bb5d5e51db56d0f431d2659a7258
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59288717"
---
# <a name="create-interactive-reports-with-azure-monitor-workbooks"></a>Crear informes interactivos con libros de Azure Monitor

Libros de combinan texto, [registrar consultas](../log-query/query-language.md), métricas y los parámetros en informes interactivos enriquecidos. Otros miembros del equipo con acceso a los mismos recursos de Azure pueden editar los libros.

Los libros son útiles en escenarios como:

* Exploración del uso de la máquina virtual si no conoce de antemano las métricas de interés: Uso de CPU, espacio en disco, memoria, las dependencias de red, etcetera. A diferencia de otras herramientas de análisis de uso, los libros le permiten combinar varios tipos de visualizaciones y análisis, lo que los hace idóneos para este tipo de exploración de forma libre.
* A su equipo, explicando cómo funciona una máquina virtual aprovisionada recientemente, mostrando las métricas para contadores clave y otros eventos de registro.
* Compartir los resultados de un experimento de cambio de tamaño de la máquina virtual con otros miembros del equipo. Puede explicar los objetivos del experimento con texto y luego mostrar cada uso de consultas de métricas y analytics utilizan para evaluar el experimento, junto con claros para si cada métrica está por encima - o por debajo del objetivo.
* Notificación del impacto de una interrupción en el uso de la máquina virtual, la combinación de datos, texto de explicación y una explicación de los pasos siguientes para evitar interrupciones en el futuro.

Azure Monitor para las máquinas virtuales incluye varios libros para ayudarle a comenzar y, en la tabla siguiente se resumen todos ellos.

| Libro | DESCRIPCIÓN | Ámbito |
|----------|-------------|-------|
| Rendimiento | Proporciona una versión personalizable de nuestra lista de los N mejores y vista de gráficos en un único libro que aprovecha todos los contadores de rendimiento de Log Analytics que se han habilitado.| A escala |
| contadores de rendimiento | Una vista de gráfico Top N entre un amplio conjunto de contadores de rendimiento. | A escala |
| Conexiones | Las conexiones se ofrece una visión detallada de las conexiones entrantes y salientes de las máquinas virtuales supervisadas. | A escala |
| Puertos activos | Proporciona una lista de los procesos que se han vinculado a los puertos en las máquinas virtuales supervisadas y su actividad en el intervalo de tiempo seleccionado. | A escala |
| Open Ports (Abrir puertos) | Proporciona el número de puertos abiertos en las máquinas virtuales supervisadas y apertura de los detalles en esos puertos. | A escala |
| Conexiones con errores | Mostrar el número de conexiones con errores en las máquinas virtuales supervisadas, la tendencia del error, y si el porcentaje de errores aumenta con el tiempo. | A escala |
| Seguridad y auditoría | Un análisis del tráfico TCP/IP que informa sobre conexiones general, conexiones malintencionadas, donde se encuentran los puntos de conexión IP global.  Para habilitar todas las características, deberá habilitar la detección de seguridad. | A escala |
| Tráfico TCP | Un informe una clasificación de las máquinas virtuales supervisadas y su red enviado, recibido y total de tráfico en una cuadrícula y se muestra como una línea de tendencia. | A escala |
| Comparación de tráfico | Este workbooks le permite comparar las tendencias del tráfico de red para una sola máquina o un grupo de máquinas. | A escala |
| Rendimiento | Proporciona una versión personalizable de nuestra vista de rendimiento que aprovecha todos los contadores de rendimiento de Log Analytics que se han habilitado. | Máquina virtual única | 
| Conexiones | Las conexiones se ofrece una visión detallada de las conexiones entrantes y salientes de la máquina virtual. | Máquina virtual única |
 
## <a name="starting-with-a-template-or-saved-workbook"></a>Uso de una plantilla o libro guardado para empezar

Un libro se compone de secciones que constan de tablas, texto, controles de entrada y gráficos que se pueden editar de forma independientemente. Para entender mejor los libros, comencemos abriendo una plantilla y recorra la creación de un libro personalizado. 

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).

2. Seleccione **Máquinas virtuales**.

3. En la lista, seleccione una máquina virtual.

4. En la página de la VM, en la sección **Supervisión**, seleccione **Insights (versión preliminar)**.

5. En la página de información de máquina virtual, seleccione **rendimiento** o **mapas** pestaña y, a continuación, seleccione **libros View** desde el vínculo en la página. 

    ![Captura de pantalla de navegación a los libros](media/vminsights-workbooks/workbook-option-01.png)

6. En la lista desplegable, seleccione **ir a la galería** desde la parte inferior de la lista.

    ![Captura de pantalla de lista desplegable de libro](media/vminsights-workbooks/workbook-dropdown-gallery-01.png)

    Inicia la Galería de libro con un número de libros creados previamente que le ayudarán a empezar a trabajar.

7. Comenzaremos con la **Plantilla predeterminada**, que se encuentra en el encabezado **Inicio rápido**.

    ![Captura de pantalla de la galería de libros](media/vminsights-workbooks/workbook-gallery-01.png)

## <a name="editing-workbook-sections"></a>Edición de las secciones de libro

Los libros tienen dos modos: **modo de edición** y **modo de lectura**. Cuando se inició por primera vez el libro de plantilla predeterminada, se abre en **modo de edición**. Muestra todo el contenido del libro, incluidos los pasos y los parámetros que se encuentra oculto. El **modo de lectura** presenta una vista simplificada de estilo del informe. Modo de lectura le permite abstraer la complejidad que se incluyeron en la creación de un informe mientras se conserva la mecánica subyacente sólo unos pocos clics cuando sea necesario para su modificación.

![Azure Monitor para la sección de libros de máquinas virtuales de los controles de edición](media/vminsights-workbooks/workbook-new-workbook-editor-01.png)

1. Cuando haya terminado la edición de una sección, haga clic en **edición finalizada** en la esquina inferior izquierda de la sección.

2. Para crear un duplicado de una sección, haga clic en el icono **Clone this section** (Clonar este sección). La creación de secciones duplicadas es una forma ideal de iterar una consulta sin perder las iteraciones anteriores.

3. Para mover hacia arriba una sección de un libro, haga clic en el icono **Subir** o **Bajar**.

4. Para eliminar una sección de forma permanente, haga clic en el icono **Quitar**.

## <a name="adding-text-and-markdown-sections"></a>Adición de texto y secciones de Markdown

La adición de títulos, explicaciones y comentarios a los libros le ayuda a convertir un conjunto de tablas y gráficos en una descripción. Las secciones de texto de los libros son compatibles con la [sintaxis de Markdown](https://daringfireball.net/projects/markdown/) para dar formato a los textos como títulos, negrita, cursiva y listas con viñetas.

Para agregar una sección de texto al libro, use el botón **Agregar texto** situado en la parte inferior del libro o en la parte inferior de cualquier sección.

## <a name="adding-query-sections"></a>Incorporación de secciones de consulta

![Sección de consulta en los libros](media/vminsights-workbooks/005-workbook-query-section.png)

Para agregar una sección de consulta al libro, use el botón **Agregar consulta** situado en la parte inferior del libro o en la parte inferior de cualquier sección.

Las secciones de consulta son muy flexibles y pueden usarse para responder preguntas como las siguientes:

* ¿Cómo fue mi uso de CPU durante el mismo período de tiempo que un aumento de tráfico de red?
* ¿Cuál era la tendencia en el espacio en disco disponible en el último mes?
* ¿Cuántos errores de conexión de red experimentaron mi máquina virtual en las últimas dos semanas? 

También no está únicamente limitado a consultas desde el contexto de la máquina virtual inician el libro desde. Puede consultar en varias máquinas virtuales, así como las áreas de trabajo de Log Analytics, siempre que tengan permiso de acceso a esos recursos.

Para incluir los datos de otras áreas de trabajo de Log Analytics o desde una aplicación de Application Insights específica mediante la **área de trabajo** identificador. Para obtener más información acerca de las consultas entre recursos, consulte el [orientación oficial](../log-query/cross-workspace-query.md).

### <a name="advanced-analytic-query-settings"></a>Configuración avanzada de consultas analíticas

Cada sección tiene su propia configuración avanzada, que es accesibles a través de la configuración ![controles de edición de secciones de libros](media/vminsights-workbooks/006-settings.png) icono situado a la derecha de la **agregar parámetros** botón.

![Azure Monitor para la sección de libros de máquinas virtuales de los controles de edición](media/vminsights-workbooks/007-settings-expanded.png)

|         |          |
| ---------------- |:-----|
| **Ancho personalizado**    | Convierte un elemento en un tamaño arbitrario, por lo que puede ajustar muchos elementos en una sola línea que le permiten organizar mejor los gráficos y las tablas en informes interactivos enriquecidos.  |
| **Visible condicionalmente** | Especifique esta opción para ocultar los pasos en función de un parámetro cuando está en modo de lectura. |
| **Un parámetro de exportación**| Permitir que una fila seleccionada en la cuadrícula o gráfico para hacer que los pasos posteriores a cambiar los valores o se hacen visibles.  |
| **Mostrar consulta cuando no se está editando** | Muestra la consulta por encima del gráfico o tabla incluso cuando está en modo de lectura.
| **Mostrar el botón Abrir en Analytics cuando no se esté editando** | Agrega el icono azul de análisis a la esquina derecha del gráfico para permitir el acceso de un solo clic.|

La mayoría de estas opciones de configuración son bastante intuitivas, pero para comprender la opción **Exportar un parámetro** le recomendamos que examine un libro que use esta funcionalidad.

Uno de los libros creados previamente - **tráfico TCP**, proporciona información sobre las métricas de conexión de una máquina virtual.

La primera sección del libro se basa en los datos del registro de consultas. La segunda sección también se basa en los datos del registro de consultas, pero selecciona una fila en la primera tabla interactivamente actualizará el contenido de los gráficos:

![Azure Monitor para la sección de libros de máquinas virtuales de los controles de edición](media/vminsights-workbooks/008-workbook-tcp-traffic.png)

El comportamiento es posible mediante el uso de la **cuando se selecciona un elemento, un parámetro de exportación** opciones avanzadas, que están habilitadas en la consulta de registro de la tabla.

![Azure Monitor para la sección de libros de máquinas virtuales de los controles de edición](media/vminsights-workbooks/009-settings-export.png)

La segunda consulta de registro, a continuación, utiliza los valores exportados cuando se selecciona una fila para crear un conjunto de valores que son utilizados por el encabezado de sección y gráficos. Si no se selecciona ninguna fila, ocultan el encabezado de sección y gráficos. 

Por ejemplo, el parámetro hidden en la segunda sección utiliza la siguiente referencia de la fila seleccionada en la cuadrícula:

```
VMConnection
| where TimeGenerated {TimeRange}
| where Computer in ("{ComputerName}") or '*' in ("{ComputerName}") 
| summarize Sent = sum(BytesSent), Received = sum(BytesReceived) by bin(TimeGenerated, {TimeRange:grain})
```

## <a name="adding-metrics-sections"></a>Incorporación de secciones de métricas

Las secciones de las métricas le proporcionan acceso total para incorporar datos de métricas de Azure Monitor en sus informes interactivos. En el Monitor de Azure para máquinas virtuales, los libros creados previamente normalmente contendrá datos de consulta de análisis en lugar de datos de métrica.  Puede crear libros con datos de métrica, lo que le permite aprovechar al máximo lo mejor de ambas características en un mismo lugar. También tiene la capacidad de extraer datos de métricas de recursos en cualquiera de las suscripciones a las que tiene acceso.

Este es un ejemplo de datos de la máquina virtual se extraen en un libro para proporcionar una visualización de la cuadrícula de rendimiento de CPU:

![Azure Monitor para la sección de libros de máquinas virtuales de los controles de edición](media/vminsights-workbooks/010-metrics-grid.png)

## <a name="adding-parameter-sections"></a>Incorporación de secciones de parámetros

Los parámetros de libros le permiten cambiar valores en el libro sin tener que editar manualmente las secciones de texto o consulta. Esto elimina la necesidad de tener que comprender el lenguaje de consulta de análisis subyacente y amplía significativamente la audiencia potencial de creación de informes basados en el libro.

Para reemplazar los valores de parámetros en consultas, texto o en otras secciones de parámetros, se coloca el nombre del parámetro entre llaves, como ``{parameterName}``. Los nombres de parámetro se limitan a reglas similares como identificadores de JavaScript, caracteres alfabéticos o caracteres de subrayado, seguidos de caracteres alfanuméricos o caracteres de subrayado. Por ejemplo, **a1** está permitido, pero **1a** no lo está.

Los parámetros son lineales, desde la parte superior de un libro y hasta los pasos posteriores.  Los parámetros declarados más adelante en un libro pueden invalidar los parámetros que se declararon anteriormente. Esto también permite a los parámetros que utilizan las consultas para tener acceso a los valores de parámetros definidos anteriormente. En el propio paso de un parámetro, los parámetros también son lineales, de izquierda a derecha, donde los parámetros de la derecha pueden depender de un parámetro declarado anteriormente en ese mismo paso.
 
Hay cuatro tipos diferentes de parámetros, que se admiten actualmente:

|                  |      |
| ---------------- |:-----|
| **Texto**    | Permite al usuario editar un cuadro de texto y, opcionalmente, puede proporcionar una consulta para rellenar el valor predeterminado. |
| **Desplegable** | Permite al usuario elegir entre un conjunto de valores. |
| **Selector de intervalo de tiempo**| Permite al usuario elegir entre un conjunto predefinido de valores de intervalo de tiempo o seleccionar un intervalo de tiempo personalizado.|
| **Selector de recursos** | Permite al usuario elegir entre los recursos seleccionados para el libro.|

### <a name="using-a-text-parameter"></a>Uso de un parámetro de texto

El valor se sustituye un usuario escribe en el cuadro de texto directamente en la consulta, sin caracteres de escape o comillas. Si el valor necesario es una cadena, la consulta debe tener comillas alrededor del parámetro (como **"{parameter}"**).

El parámetro de texto permite que el valor en un cuadro de texto que se usará en cualquier lugar. Puede ser el nombre de la tabla, el nombre de la columna, el nombre de la función, el operador, etc.  El tipo de parámetro de texto tiene una configuración **obtener el valor predeterminado de consulta de analytics**, lo que permite que el autor del libro debe usar una consulta para rellenar el valor predeterminado de ese cuadro de texto.

Cuando se usa el valor predeterminado de una consulta de registro, solo el primer valor de la primera fila (fila 0, la columna 0) se utiliza como el valor predeterminado. Por lo tanto, se recomienda limitar la consulta para devolver solo una sola fila y una columna. Se omite cualquier otro dato devuelto por la consulta. 

Independientemente del valor que devuelva la consulta, se reemplazará directamente sin ningún carácter de escape ni comillas. Si la consulta no devuelve ninguna fila, el resultado del parámetro es una cadena vacía (si el parámetro no es necesario) o indefinido (si el parámetro es necesario).

### <a name="using-a-drop-down"></a>Uso de una lista desplegable

El tipo de parámetro de la lista desplegable le permite crear un control de lista desplegable, lo que permite la selección de uno o varios valores.

La lista desplegable se rellena con una consulta de registro o JSON. Si la consulta devuelve una columna, los valores de esa columna son el valor y la etiqueta en el control de lista desplegable. Si la consulta devuelve dos columnas, la primera columna es el valor y la segunda columna es la etiqueta que se muestra en la lista desplegable. Si la consulta devuelve tres columnas, la tercera columna se utiliza para indicar la selección predeterminada en esa lista desplegable. Esta columna puede ser de cualquier tipo, pero lo más sencillo es usar tipos booleanos o numéricos, donde 0 es falso y 1 es verdadero.

Si la columna es un tipo de cadena, la cadena nula o vacía se considera falsa y cualquier otro valor se considera verdadero. Para desplegables de selección única, el primer valor con un valor true se utiliza como la selección predeterminada.  Para varios selección listas desplegables, todos los valores con un valor true se usan como el conjunto seleccionado de forma predeterminada. Los elementos de la lista desplegable se muestran en cualquier orden, la consulta devuelve las filas. 

Echemos un vistazo a los parámetros presentes en el informe información general de las conexiones. Haga clic en el símbolo de edición junto al **dirección**.

![Azure Monitor para la sección de libros de máquinas virtuales de los controles de edición](media/vminsights-workbooks/011-workbook-using-dropdown.png)

Esto iniciará el **Editar parámetro** elemento de menú.

![Azure Monitor para la sección de libros de máquinas virtuales de los controles de edición](media/vminsights-workbooks/012-workbook-edit-parameter.png)

El JSON le permite generar una tabla arbitraria que se rellenan con contenido. Por ejemplo, el siguiente JSON genera dos valores en la lista desplegable:

```
[
    { "value": "inbound", "label": "Inbound"},
    { "value": "outbound", "label": "Outbound"}
]
```

Un ejemplo más aplicable usa una lista desplegable para elegir entre un conjunto de contadores de rendimiento por nombre:

```
Perf
| summarize by CounterName, ObjectName
| order by ObjectName asc, CounterName asc
| project Counter = pack('counter', CounterName, 'object', ObjectName), CounterName, group = ObjectName
```

La consulta mostrará los resultados de la siguiente forma:

![Lista desplegable de contador de rendimiento](media/vminsights-workbooks/013-workbook-edit-parameter-perf-counters.png)

Listas desplegables son increíblemente eficaces herramientas para personalizar y crear informes interactivos.

### <a name="time-range-parameters"></a>Parámetros de intervalo de tiempo

Aunque puede elaborar su propio parámetro de intervalo de tiempo personalizado mediante el tipo de parámetro de la lista desplegable, también puede usar el tipo de parámetro de intervalo de tiempo ya definido si no necesita el mismo grado de flexibilidad. 

Los tipos de parámetro de intervalo de tiempo tienen 15 intervalos predeterminados que van desde cinco minutos hasta los últimos 90 días. También hay una opción para permitir la selección del intervalo de tiempo personalizado, que permite que el operador del informe elija los valores explícitos de inicio y fin del intervalo de tiempo.

### <a name="resource-picker"></a>Selector de recursos

El tipo de parámetro del selector de recursos le permite definir el ámbito del informe para ciertos tipos de recursos. Un ejemplo de un libro creado previamente que aprovecha el tipo de selector de recursos es el **rendimiento** libro.

![Lista desplegable de áreas de trabajo](media/vminsights-workbooks/014-workbook-edit-parameter-workspaces.png)

## <a name="saving-and-sharing-workbooks-with-your-team"></a>Almacenamiento y uso compartido de libros con el equipo

Los libros se guardan dentro de un área de trabajo de Log Analytics o un recurso de máquina virtual, dependiendo de cómo tener acceso a la Galería de libros. Se puede guardar el libro en el **Mis informes** privada o en el **informes compartidos** sección que sea accesible a todos los usuarios con acceso al recurso. Para ver todos los libros del recurso, haga clic en el botón **Abrir** de la barra de acciones.

Para compartir un libro que está actualmente en **Mis informes**:

1. Haga clic en **Abrir** en la barra de acciones
2. Haga clic en el botón "..." situado junto al libro que desea compartir
3. Haga clic en **Move to Shared Reports** (Mover a informes compartidos).

Para compartir un libro con un vínculo o por correo electrónico, haga clic en **Compartir** en la barra de acciones. Tenga en cuenta que los destinatarios del vínculo necesitarán acceder a este recurso en Azure Portal para ver el libro. Para realizar ediciones, los destinatarios necesitan al menos permisos de colaborador para el recurso.

Para anclar un vínculo a un libro en un panel de Azure:

1. Haga clic en **Abrir** en la barra de acciones
2. Haga clic en el botón "..." situado junto al libro que desea anclar
3. Haga clic en **Anclar al panel**.

## <a name="next-steps"></a>Pasos siguientes
Para obtener información sobre cómo usar la característica de mantenimiento, consulte [ver mantenimiento de la máquina virtual de Azure](vminsights-health.md), o para ver las dependencias de aplicaciones detectadas, consulte [Azure Monitor de vista de mapa de las máquinas virtuales](vminsights-maps.md). 