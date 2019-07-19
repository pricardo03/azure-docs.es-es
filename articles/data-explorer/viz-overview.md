---
title: Visualización de datos de Azure Data Explorer
description: Obtenga información sobre las distintas formas en que puede visualizar los datos de Azure Data Explorer.
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/30/2019
ms.openlocfilehash: d1c73d8eb65ed5d67d5250b4a3bca3b80450001e
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67536730"
---
# <a name="data-visualization-with-azure-data-explorer"></a>Visualización de datos con Azure Data Explorer 

Azure Data Explorer es un servicio de exploración de datos altamente escalable para los datos de registro y telemetría que se usa para compilar soluciones complejas de análisis para grandes cantidades de datos. Azure Data Explorer se integra con varias herramientas de visualización, por lo que puede visualizar los datos y compartir los resultados en toda la organización. Estos datos se pueden convertir en información práctica que tendrá impacto en su empresa.

La generación de informes y visualización de los datos es un paso crítico del proceso de análisis de datos. Azure Data Explorer admite muchos servicios de inteligencia empresarial, por lo que puede usar el servicio que mejor se adapte a su escenario y presupuesto.

## <a name="kusto-query-language-visualizations"></a>Visualizaciones del lenguaje de consulta Kusto

El [`render operator`](/azure/kusto/query/renderoperator) del lenguaje de consulta Kusto ofrece varios tipos de visualizaciones, como tablas, gráficos circulares y gráficos de barras para mostrar los resultados de la consulta. Las visualizaciones de las consultas resultan útiles para la previsión y la detección de anomalías, el aprendizaje automático, etc.

## <a name="power-bi"></a>Power BI

Azure Data Explorer brinda la funcionalidad para conectarse a [Power BI](https://powerbi.microsoft.com) con varios métodos: 

  * [Built-in native Power BI connector](/azure/data-explorer/power-bi-connector) (Conector nativo de Power BI integrado)

  * [Query import from Azure Data Explorer into Power BI](/azure/data-explorer/power-bi-imported-query) (Importación de consultas desde Azure Data Explorer a Power BI)
 
  * [SQL query](/azure/data-explorer/power-bi-sql-query)

## <a name="microsoft-excel"></a>Microsoft Excel

Azure Data Explorer brinda la funcionalidad para conectarse a [Microsoft Excel](https://products.office.com/excel) con el conector nativo de Excel integrado o de importar una consulta desde Azure Data Explorer a Excel.

## <a name="grafana"></a>Grafana

[Grafana](https://grafana.com) proporciona un complemento para Azure Data Explorer que le permite visualizar los datos desde Azure Data Explorer. Puede [configurar Azure Data Explorer como un origen de datos para Grafana y, luego, visualizar los datos](/azure/data-explorer/grafana). 

## <a name="odbc-connector"></a>Conector ODBC

Azure Data Explorer proporciona un [conector para la conectividad abierta de bases de datos (ODBC)](connect-odbc.md), por lo que cualquier aplicación compatible con ODBC se puede conectar a Azure Data Explorer.

## <a name="tableau"></a>Tableau

Azure Data Explorer brinda la funcionalidad para conectarse a [Tableau](https://www.tableau.com) mediante el [conector ODBC](/azure/data-explorer/connect-odbc) y, luego, [visualizar los datos en Tableau](tableau.md).

## <a name="qlik"></a>Qlik

Azure Data Explorer brinda la funcionalidad para conectarse a [Qlik](https://www.qlik.com) mediante el [conector ODBC](/azure/data-explorer/connect-odbc) y, luego, crear paneles de Qlik Sense y visualizar los datos. En el siguiente vídeo, descubrirá cómo visualizar los datos de Azure Data Explorer con Qlik. 

> [!VIDEO https://www.youtube.com/embed/nhWIiBwxjjU]  

## <a name="sisense"></a>Sisense

Azure Data Explorer brinda la funcionalidad para conectarse a [Sisense](https://www.sisense.com) mediante el conector JDBC. Puede [configurar Azure Data Explorer como un origen de datos para Sisense y, luego, visualizar los datos](/azure/data-explorer/sisense).