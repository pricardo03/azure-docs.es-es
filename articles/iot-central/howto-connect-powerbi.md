---
title: Visualización de los datos de Azure IoT Central en un panel de Power BI | Microsoft Docs
description: Use la solución de Power BI para Azure IoT Central a fin de visualizar y analizar los datos de IoT Central.
ms.service: iot-central
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 10/4/2019
ms.topic: conceptual
ms.openlocfilehash: 3ce2f4304787107d0d6875333e4630dae8d7d1dd
ms.sourcegitcommit: c2e7595a2966e84dc10afb9a22b74400c4b500ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2019
ms.locfileid: "71973770"
---
# <a name="visualize-and-analyze-your-azure-iot-central-data-in-a-power-bi-dashboard"></a>Visualización y análisis de los datos de Azure IoT Central en un panel de Power BI

[!INCLUDE [iot-central-original-pnp](../../includes/iot-central-original-pnp-note.md)]

*Este tema se aplica a administradores*.

![Canalización de las soluciones de Power BI](media/howto-connect-powerbi/iot-continuous-data-export.png)

Use la solución de Power BI para Azure IoT Central a fin de crear un panel eficaz de Power BI y supervisar el rendimiento de los dispositivos IoT. En el panel de Power BI, puede:
- Realizar un seguimiento de la cantidad de datos que se envían a lo largo del tiempo
- Comparar el volumen de datos entre telemetría, estados y eventos
- Identificar los dispositivos que notifican una gran cantidad de medidas
- Observar las tendencias históricas de las medidas de dispositivos
- Identificar los dispositivos problemáticos que envían una gran cantidad de eventos críticos

Esta solución configura la canalización que toma los datos de la cuenta de Azure Blob Storage de la [exportación continua de datos](howto-export-data.md). Estos datos fluyen a través de Azure Functions, Azure Data Factory y Azure SQL Database para su procesamiento y transformación. La salida se puede visualizar y analizar en un informe de Power BI que puede descargar como archivo PBIX. Todos estos recursos se crean en la suscripción de Azure, para que pueda personalizar cada componente de acuerdo con sus necesidades.

> [!Note] 
> La solución de Power BI para Azure IoT Central funciona con aplicaciones de IoT Central que no admiten IoT Plug and Play (hoy en día, versiones preliminares)

## <a name="get-the-power-bi-solution-for-azure-iot-centralhttpsakamsiotcentralpowerbisolutiontemplate-from-microsoft-appsource"></a>Obtenga la [solución de Power BI para Azure IoT Central](https://aka.ms/iotcentralpowerbisolutiontemplate) en Microsoft AppSource.

## <a name="prerequisites"></a>Requisitos previos
Para configurar la solución, es necesario lo siguiente:
- Acceso a una suscripción de Azure
- Aplicación de IoT Central que no admite IoT Plug and Play (hoy en día, versiones preliminares)
- Configuración de la exportación de datos continua a Azure Blob Storage desde la aplicación de IoT Central
    - Asegúrese de que el formato de datos es Avro
    - Se recomienda activar los flujos de medidas, dispositivos y plantillas de dispositivos para aprovechar al máximo el panel de Power BI.
    - Aprenda a [configurar la exportación de datos continua](howto-export-data-blob-storage.md).
- Power BI Desktop (versión más reciente)
- Power BI Pro (si quiere compartir el panel con otros usuarios)

## <a name="reports"></a>Informes

Dos informes se generan automáticamente. 

El primer informe muestra una vista histórica de las medidas notificadas por los dispositivos y en él se desglosan los diferentes tipos de medidas y los dispositivos que han enviado el mayor número de medidas.

![Página 1 del informe de Power BI](media/howto-connect-powerbi/template-page1-hasdata.PNG)

El segundo informe profundiza en los eventos y muestra una vista histórica de los errores y advertencias detectados. También muestra todos los dispositivos que notifican el mayor número de eventos, así como eventos de error y eventos de advertencia en concreto.

![Página 2 del informe de Power BI](media/howto-connect-powerbi/template-page2-hasdata.PNG)

## <a name="architecture"></a>Arquitectura
En Azure Portal se puede tener acceso a todos los recursos que se han creado. Deberían estar en un grupo de recursos.

![Vista de Azure Portal del grupo de recursos](media/howto-connect-powerbi/azure-deployment.PNG)

A continuación se describen las características específicas de cada recurso y cómo se usan.

### <a name="azure-functions"></a>Azure Functions
La aplicación Azure Function se desencadena cada vez que se escribe un nuevo archivo en el almacenamiento en blobs. Las funciones extraen los campos dentro de cada archivo de mediciones, dispositivos y plantillas de dispositivo, y rellenan varias tablas SQL intermedias que Azure Data Factory usará.

### <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory se conecta a la base de datos SQL como un servicio vinculado. Ejecuta las actividades del procedimiento almacenado que procesa los datos y los almacena en las tablas de análisis.

### <a name="azure-sql-database"></a>Azure SQL Database
Estas tablas se crean automáticamente para rellenar los informes predeterminados. Explore estos esquemas en Power BI. Puede crear sus propias visualizaciones en estos datos.

| Nombre de tabla |
|------------|
|[analytics].[Measurements]|
|[analytics].[Messages]|
|[stage].[Measurements]|
|[analytics].[Properties]|
|[analytics].[PropertyDefinitions]|
|[analytics].[MeasurementDefinitions]|
|[analytics].[Devices]|
|[analytics].[DeviceTemplates]|
|[dbo].[date]|
|[dbo].[ChangeTracking]|

## <a name="estimated-costs"></a>Costos estimados

Presentamos una estimación de los costos de Azure (Azure Functions, Data Factory, Azure SQL) implicados. Todos los precios están en dólares estadounidenses. Tenga en cuenta que los precios pueden variar según la región, por lo que siempre debe buscar los más recientes para los servicios individuales a fin de obtener los precios reales.
Los siguientes valores predeterminados se establecen automáticamente en la plantilla (puede modificar cualquiera de ellos después de la configuración):

- Azure Functions: Plan S1 de App Service, 74,40 $/mes
- S1 de SQL Azure, ~30 $/mes

Es conveniente que conozca las distintas opciones de precios y ajuste las opciones para satisfacer sus necesidades.

## <a name="resources"></a>Recursos

Visite AppSource para obtener la [solución de Power BI para Azure IoT Central](https://aka.ms/iotcentralpowerbisolutiontemplate).

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a visualizar los datos en Power BI, este es el siguiente paso sugerido:

> [!div class="nextstepaction"]
> [Cómo administrar dispositivos](howto-manage-devices.md)