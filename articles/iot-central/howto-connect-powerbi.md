---
title: Visualización de los datos de Azure IoT Central en un panel de Power BI | Microsoft Docs
description: Use la plantilla de solución de Power BI de análisis de Azure IoT Central para visualizar y analizar los datos de IoT Central.
ms.service: iot-central
services: iot-central
author: viv-liu
ms.author: viviali
ms.date: 07/16/2018
ms.topic: conceptual
ms.openlocfilehash: 6f8bed8aba43d77cdfad49f4fe62a4c7aa2c5ce3
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2018
ms.locfileid: "46368076"
---
# <a name="visualize-and-analyze-your-azure-iot-central-data-in-a-power-bi-dashboard"></a>Visualización y análisis de los datos de Azure IoT Central en un panel de Power BI

*Este tema se aplica a administradores*.

![Canalización de la plantilla de solución de Power BI](media/howto-connect-powerbi/iot-continuous-data-export.png)

Use la plantilla de solución de Power BI de análisis de Azure IoT Central para crear un panel eficaz de Power BI para supervisar el rendimiento de los dispositivos IoT. En el panel de Power BI, puede:
- Realizar un seguimiento de la cantidad de datos que se envían a lo largo del tiempo
- Comparar el volumen de datos entre telemetría, estados y eventos
- Identificar los dispositivos que notifican una gran cantidad de medidas
- Observar las tendencias históricas de las medidas de dispositivos
- Identificar los dispositivos problemáticos que envían una gran cantidad de eventos críticos

Esta plantilla de solución configura la canalización que toma los datos de la cuenta de Azure Blob Storage de la [exportación continua de datos](howto-export-data.md). Estos datos fluyen a través de Azure Functions, Azure Data Factory y Azure SQL Database para su procesamiento y transformación. La salida se puede visualizar y analizar en un informe de Power BI que puede descargar como archivo PBIX. Todos estos recursos se crean en la suscripción de Azure, para que pueda personalizar cada componente de acuerdo con sus necesidades. Esta plantilla de solución es completamente de código abierto. Visite el [repositorio de GitHub](https://aka.ms/iotcentralgithubpowerbisolutiontemplate) para más información sobre la arquitectura y cómo ampliar la solución.

## <a name="get-the-azure-iot-central-analytics-solution-templatehttpsakamsiotcentralpowerbisolutiontemplate-from-microsoft-appsource"></a>Obtenga la [plantilla de solución de análisis de Azure IoT Central](https://aka.ms/iotcentralpowerbisolutiontemplate) de Microsoft AppSource.

## <a name="prerequisites"></a>Requisitos previos
Para configurar la plantilla, es necesario lo siguiente:
- Acceso a una suscripción de Azure
- Datos exportados mediante la [exportación continua de datos](howto-export-data.md) desde la aplicación IoT Central. Se recomienda activar los flujos de medidas, dispositivos y plantillas de dispositivos para aprovechar al máximo el panel de Power BI.
- Power BI Desktop (versión más reciente)
- Power BI Pro (si quiere compartir el panel con otros usuarios)

## <a name="reports"></a>Informes

Dos informes se generan automáticamente. 

El primer informe muestra una vista histórica de las medidas notificadas por los dispositivos y en él se desglosan los diferentes tipos de medidas y los dispositivos que han enviado el mayor número de medidas.

![Página 1 del informe de Power BI](media/howto-connect-powerbi/template-page1-hasdata.PNG)

El segundo informe profundiza en los eventos y muestra una vista histórica de los errores y advertencias detectados. También muestra todos los dispositivos que notifican el mayor número de eventos, así como eventos de error y eventos de advertencia en concreto.

![Página 2 del informe de Power BI](media/howto-connect-powerbi/template-page2-hasdata.PNG)

## <a name="resources"></a>Recursos

Visite AppSource para obtener la [plantilla de solución de análisis de Azure IoT Central](https://aka.ms/iotcentralpowerbisolutiontemplate).

Visite el [repositorio de GitHub](https://aka.ms/iotcentralgithubpowerbisolutiontemplate) para más información sobre la arquitectura y cómo ampliar la solución.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha aprendido a visualizar los datos en Power BI, este es el siguiente paso sugerido:

> [!div class="nextstepaction"]
> [Cómo administrar dispositivos](howto-manage-devices.md)
