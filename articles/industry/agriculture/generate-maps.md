---
title: Generación de mapas
description: Describe cómo generar mapas en FarmBeats
author: uhabiba04
ms.topic: article
ms.date: 11/04/2019
ms.author: v-umha
ms.openlocfilehash: 635431fb87e5f164f92ab4b7a1027ee96e9d801a
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/09/2019
ms.locfileid: "73891024"
---
# <a name="generate-maps"></a>Generación de mapas

Con Azure FarmBeats puede generar los siguientes mapas mediante imágenes de satélite y entradas de datos de sensores. Los mapas ayudan a ver la ubicación geográfica de la granja y a identificar la ubicación adecuada para los dispositivos.

  -  **Mapa de colocación de sensores**: proporciona recomendaciones sobre cuántos sensores se deben usar y dónde colocarlos en una granja.
  - **Mapa de índices por satélite**: muestra los índices de vegetación y de agua de una granja.
  - **Mapa de humedad del suelo**: muestra la distribución de la humedad del suelo mediante la fusión de datos por satélite y de los sensores.

## <a name="sensor-placement-maps"></a>Mapas de colocación de sensores

El mapa de colocación de sensores de FarmBeats ayuda con la ubicación de los sensores de humedad del suelo. La salida del mapa se compone de una lista de coordenadas para la implementación de los sensores. Las entradas de estos sensores se usan junto con una imagen por satélite para generar el mapa térmico de humedad del suelo.  

Este mapa se deriva segmentando las copas de los árboles tal y como se ha observado en varias fechas a lo largo del año, incluso el suelo y los edificios forman parte de las copas de los árboles. Puede quitar los sensores que no sean necesarios en la ubicación. Este mapa es solo una guía y puede modificar la posición y el número ligeramente según su experiencia (agregar sensores no retrasa los resultados del mapa térmico de humedad del suelo, pero existe la posibilidad de que se produzca un deterioro en la precisión del mapa térmico si se reduce el número de sensores).  

## <a name="before-you-begin"></a>Antes de empezar  

Asegúrese de lo siguiente antes de intentar generar un mapa de colocación de sensores:

- El tamaño de la granja debe ser mayor que un acre.
- El número de escenas de Sentinel sin nube debe ser mayor que seis para el intervalo de fechas seleccionado.  
- Al menos seis escenas de Sentinel sin nube deben tener NDVI >= 0,3.

    > [!NOTE]
    > Sentinel solo permite dos subprocesos simultáneos por usuario. Como resultado, los trabajos se pondrán en cola y es posible que tarden más en completarse.

### <a name="dependencies-on-sentinel"></a>Dependencias en Sentinel  

Las siguientes son dependencias de Sentinel:

- Dependemos del rendimiento de Sentinel para descargar las imágenes poor satélite. Compruebe el estado de rendimiento de Sentinel y las [actividades](https://scihub.copernicus.eu/twiki/do/view/SciHubNews/WebHome) de mantenimiento.
- Sentinel solo permite dos [subprocesos de descarga](https://sentinels.copernicus.eu/web/sentinel/sentinel-data-access/typologies-and-services) simultáneos por usuario.
- La generación de mapas de precisión se verá afectada por la [cobertura de Sentinel y la frecuencia de las visitas]( https://sentinel.esa.int/web/sentinel/user-guides/sentinel-2-msi/revisit-coverage).

## <a name="create-sensor-placement-map"></a>Creación de un mapa de colocación de sensores
En la sección se detallan los procedimientos de creación de mapas de colocación de sensores.

> [!NOTE]
> Puede iniciar la colocación de sensores desde la página **Maps** (Mapas) o el menú desplegable **Generate Precision Maps** (Generar mapas de precisión) de la página **Farm Details** (Detalles de la granja).

Para ello, siga los pasos que se describen a continuación:

1. En la página principal, seleccione **Maps** (Mapas) en el menú de navegación izquierdo.
2. Seleccione **Create Maps** (Crear mapas) y **Sensor Placement** (Colocar sensores) en el menú desplegable.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/create-maps-drop-down-1.png)

3. Seleccione **Sensor Placement** (Colocar sensores).
  Se mostrará la ventana de colocación de sensores.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/sensor-placement-1.png)

4. Seleccione una granja del menú desplegable **Farm** (Granja).  
   Para buscar y seleccionar la granja, puede desplazarse por la lista desplegable o escribir el nombre de la granja en el cuadro de texto.
5. Si desea generar un mapa del último año, seleccione la opción **Recommended** (Recomendado).
6. Si desea generar un mapa de un intervalo de fechas personalizado, seleccione **Select Date Range** (Seleccionar intervalo de fechas) y proporcione la fecha de inicio y de fin del mapa de colocación de sensores.
7. Seleccione **Generate Maps** (Generar mapas).
 Aparecerá un mensaje de confirmación con detalles del trabajo.

  Para información sobre el estado del trabajo, consulte la sección **Visualización de los trabajos**. Si el estado del trabajo muestra *Failed* (Erróneo), aparecerá un mensaje de error detallado en la información sobre herramientas del estado *Failed* (Erróneo). En este caso, repita los pasos indicados anteriormente e inténtelo de nuevo.

  Si el problema persiste, consulte la sección [Troubleshoot](troubleshoot-project-farmbeats.md) (Solucionar problemas) o póngase en contacto con el [foro de Azure FarmBeats de soporte técnico](https://aka.ms/FarmBeatsMSDN) con los registros pertinentes.

### <a name="view-and-download-sensor-placement-map"></a>Visualización y descarga del mapa de colocación de sensores

Para ello, siga los pasos que se describen a continuación:

1. En la página principal, seleccione **Maps** (Mapas) en el menú de navegación izquierdo.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/view-download-sensor-placement-map-1.png)

2. Seleccione **Filter** (Filtro) en el panel de navegación izquierdo de la ventana.
  La ventana **Filter** (Filtro) muestra los criterios de búsqueda.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/view-download-filter-1.png)

3. Seleccione **Type** (Tipo), **Date** (Fecha) y **Name** (Nombre) del menú desplegable y **Apply** (Aplicar) para buscar el mapa que desee ver.
  La fecha en la que se creó el trabajo se muestra con formato tipo_nombreDeGranja_AAAA-MM-DD.
4. Desplácese por la lista de mapas disponibles mediante las barras de navegación situadas al final de la página.
5. Seleccione el trabajo que desee ver. Una ventana emergente mostrará la vista previa del mapa seleccionado.
6. Seleccione **Download** (Descargar) y descargue el archivo GeoJSON de las coordenadas del sensor.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/download-sensor-placement-map-1.png)

## <a name="satellite-indices-map"></a>Mapa de índices por satélite

En las secciones siguientes se explican los procedimientos necesarios para crear y ver el mapa de índices por satélite.

> [!NOTE]
> Puede iniciar el mapa de índices por satélite desde la página **Maps** (Mapas) o el menú desplegable **Generate Precision Maps** (Generar mapas de precisión) de la página **Farm Details** (Detalles de la granja).

FarmBeats proporciona la capacidad de generar mapas de índices de vegetación de diferencia normalizada (NDVI), de índices de vegetación mejorados (EVI) y de agua de diferencia normalizada (NDWI) para las granjas. Estos índices ayudan a determinar el crecimiento actual o anterior de los cultivos y los niveles de agua representativos en el suelo.


> [!NOTE]
> Se requiere una imagen de Sentinel para los días para los que se genera el mapa.


## <a name="create-satellite-indices-map"></a>Creación del mapa de índices por satélite

Para ello, siga los pasos que se describen a continuación:

1. En la página principal, seleccione **Maps** (Mapas) en el menú de navegación izquierdo.
2. Seleccione **Create Maps** (Crear mapas) y **Satellite Indices** (Índices por satélite) en el menú desplegable.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/create-maps-drop-down-satellite-indices-1.png)

3. Seleccione **Satellite Indices** (Índices por satélite).
  Aparecerá la ventana Satellite Indices (Índices por satélite).

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/satellitte-indices-1.png)

4. Seleccione una granja del menú desplegable.  
   Para buscar y seleccionar la granja, puede desplazarse por la lista desplegable o escribir su nombre.   
5. Si desea generar un mapa de la última semana, seleccione la opción **This Week** (Esta semana).
6. Si desea generar un mapa de un intervalo de fechas personalizado, seleccione **Select Date Range** (Seleccionar intervalo de fechas) y proporcione la fecha de inicio y de fin del mapa de índices por satélite.
7. Seleccione **Generate Maps** (Generar mapas).
    Aparecerá un mensaje de confirmación con detalles del trabajo.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/successful-satellitte-indices-1.png)

    Para información sobre el estado del trabajo, consulte la sección **Visualización de los trabajos**. Si el estado del trabajo muestra *Failed* (Erróneo), aparecerá un mensaje de error detallado en la información sobre herramientas del estado *Failed* (Erróneo). En este caso, repita los pasos indicados anteriormente e inténtelo de nuevo.

    Si el problema persiste, consulte la sección [Troubleshoot](troubleshoot-project-farmbeats.md) (Solucionar problemas) o póngase en contacto con el [foro de Azure FarmBeats de soporte técnico](https://aka.ms/FarmBeatsMSDN) con los registros pertinentes.

### <a name="view-and-download-map"></a>Visualización y descarga de mapas

Para ello, siga los pasos que se describen a continuación:

1. En la página principal, seleccione **Maps** (Mapas) en el menú de navegación izquierdo.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/view-download-sensor-placement-map-1.png)

2. Seleccione **Filter** (Filtro) en el panel de navegación izquierdo de la ventana y esta mostrará los criterios de búsqueda.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/view-download-filter-1.png)

3. Seleccione **Type** (Tipo), **Date** (Fecha) y **Name** (Nombre) del menú desplegable y **Apply** (Aplicar) para buscar el mapa que desee ver.
  La fecha en la que se creó el trabajo se muestra con formato tipo_nombreDeGranja_AAAA-MM-DD.

4. Desplácese por la lista de mapas disponibles mediante las barras de navegación situadas al final de la página.
5. Para cada combinación de **Farm Name** (Nombre de la granja) y **Date** (Fecha), están disponibles los tres mapas siguientes:
    - NDVI
    - EVI
    - NDWI
6. Seleccione el trabajo que desee ver. Una ventana emergente mostrará la vista previa del mapa seleccionado.
7. Seleccione el menú desplegable **Download** (Descargar) para seleccionar el formato de descarga y el mapa se descargará y se guardará en la carpeta local del equipo.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/download-satellite-indices-map-1.png)

## <a name="get-soil-moisture-heatmap"></a>Obtención de un mapa térmico de humedad del suelo

La humedad del suelo es el agua que se retiene en los espacios entre las partículas del suelo. El mapa térmico de humedad del suelo ayuda a comprender los datos de humedad del suelo a cualquier profundidad y a alta resolución en las granjas. Para generar un mapa térmico de humedad del suelo preciso y útil, se requiere una implementación uniforme de sensores y que estos procedan del mismo proveedor. Los diferentes proveedores tendrán diferencias en la forma en la que se mide la humedad del suelo, así como diferencias de calibración. El mapa térmico se genera para una profundidad determinada mediante los sensores implementados a esa profundidad.

### <a name="before-you-begin"></a>Antes de empezar  

Asegúrese de lo siguiente antes de intentar generar un mapa térmico de humedad del suelo:

- Se deben implementar al menos tres sensores de humedad del suelo. Microsoft recomienda no intentar crear un mapa de humedad del suelo sin que se hayan implementado antes los sensores y se hayan asociado estos a la granja.  
- La generación del mapa térmico de humedad del suelo se ve afectada por la cobertura de los caminos, la cobertura de las nubes y la sombra de las nubes de Sentinel. Al menos una escena de Sentinel sin nube debe estar disponible durante los últimos 120 días a partir del día para el que se solicitó el mapa de humedad del suelo.
- Al menos la mitad de los sensores implementados en la granja debe estar en línea y transmitir datos al centro de datos.
- El mapa térmico debe generarse mediante medidas de sensor del mismo proveedor.


## <a name="create-soil-moisture-heatmap"></a>Creación de un mapa térmico de humedad del suelo

Para ello, siga los pasos que se describen a continuación:

1. En la página principal, vaya a **Maps** (Mapas) en el menú de navegación izquierdo para ver la página de mapas.
2. Seleccione **Create Maps** (Crear mapas) y **Soil Moisture** (Humedad del suelo) en el menú desplegable.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/create-maps-drop-down-soil-moisture-1.png)

3. Seleccione **Soil Moisture** (Humedad del suelo).
    Aparecerá la ventana Soil Moisture (Humedad del suelo).

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/soil-moisture-1.png)

4. Seleccione una granja del menú desplegable **Farm** (Granja).  
   Para buscar y seleccionar la granja, puede desplazarse por la lista desplegable o escribir el nombre de la granja en el menú desplegable Select Farm (Seleccionar granja).
5. En el menú desplegable **Select Soil Moisture Sensor Measure** (Seleccionar medida del sensor de humedad del suelo), seleccione la medida del sensor de humedad del suelo (profundidad) para la que desea generar el mapa.
Para buscar la medida del sensor, vaya a **Sensors** (Sensores) y seleccione cualquier sensor de humedad del suelo. Después, en la sección **Sensor Properties** (Propiedades del sensor), use el valor en **Measure Name** (Nombre de medida).
6. Si lo desea para **Today** (Hoy) o **This Week** (Esta semana), seleccione una de las opciones.
7. Si desea generar un mapa de un intervalo de fechas personalizado, seleccione **Select Date Range** (Seleccionar intervalo de fechas) y proporcione la fecha de inicio y de fin del mapa de humedad del suelo.
8. Seleccione **Generate Maps** (Generar mapas).
 Aparecerá un mensaje de confirmación con detalles del trabajo.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/successful-soil-moisture-1.png)

    Para información sobre el estado del trabajo, consulte la sección **Visualización de los trabajos**. Si el estado del trabajo muestra *Failed* (Erróneo), aparecerá un mensaje de error detallado en la información sobre herramientas del estado *Failed* (Erróneo). En este caso, repita los pasos indicados anteriormente e inténtelo de nuevo.

    Si el problema persiste, consulte la sección [Troubleshoot](troubleshoot-project-farmbeats.md) (Solucionar problemas) o póngase en contacto con el [foro de Azure FarmBeats de soporte técnico](https://aka.ms/FarmBeatsMSDN) con los registros pertinentes.

### <a name="view-and-download-map"></a>Visualización y descarga de mapas

Para ello, siga los pasos que se describen a continuación:

1. En la página principal, seleccione **Maps** (Mapas) en el menú de navegación izquierdo.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/view-download-sensor-placement-map-1.png)

2. Seleccione **Filter** (Filtro) en el panel de navegación izquierdo de la ventana y esta mostrará dónde puede buscar los mapas.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/view-download-filter-1.png)

3.  Seleccione **Type** (Tipo), **Date** (Fecha) y **Name** (Nombre) del menú desplegable y **Apply** (Aplicar) para buscar el mapa que desee ver. La fecha en la que se creó el trabajo se muestra con formato tipo_nombreDeGranja_AAAA-MM-DD.
4. Seleccione el icono **Sort** (Ordenar) junto a los encabezados de tabla para ordenar estas por granja, fecha, fecha de creación, identificador de trabajo y tipo de trabajo.
5. Desplácese por la lista de mapas disponibles mediante los botones de navegación situados al final de la página.
6. Seleccione el trabajo que desee ver. Una ventana emergente mostrará la vista previa del mapa seleccionado.
7. Seleccione el menú desplegable **Download** (Descargar) para seleccionar el formato de descarga; el mapa se descargará y se guardará en la carpeta especificada.

    ![Proyecto de FarmBeats](./media/get-sensor-data-from-sensor-partner/download-soil-moisture-map-1.png)
