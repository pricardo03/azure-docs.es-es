---
title: Visualización de datos de Azure Data Explorer
description: Obtenga información sobre las distintas formas que puede visualizar los datos del explorador de datos de Azure
services: data-explorer
author: orspod
ms.author: orspodek
ms.reviewer: rkarlin
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/03/2019
ms.openlocfilehash: 85c37b6d626fc9942f5df956e738431d2727d282
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481840"
---
# <a name="data-visualization-with-azure-data-explorer"></a>Visualización de datos con el Explorador de datos de Azure 

El Explorador de datos de Azure es un servicio de exploración de datos rápida y muy escalable para los datos de telemetría y registros que se usan para crear soluciones de análisis complejos para grandes cantidades de datos. El Explorador de datos de Azure se integra con diversas herramientas de visualización, por lo que puede visualizar los datos y compartir los resultados en toda la organización. Estos datos se pueden transformar en información útil para lograr un efecto en su negocio.

Visualización de datos y los informes es un paso crítico en el proceso de análisis de datos. El Explorador de datos de Azure es compatible con muchos servicios de BI, por lo que puede usar lo que mejor se adapte a su escenario y su presupuesto.

* Visualizaciones en el Explorador de datos de Azure: Mediante el lenguaje de consulta Kusto el [ `render operator` ](/azure/kusto/query/renderoperator) ofrece diversos tipos de visualización para describir los resultados de la consulta. Visualizaciones de consulta son útiles para la detección de anomalías y previsión, aprendizaje automático y mucho más.

* [Power BI](https://powerbi.microsoft.com): El Explorador de datos de Azure proporciona la funcionalidad para conectarse a Power BI mediante distintos métodos: 

  * [Conector de Power BI nativa integrado](/azure/data-explorer/power-bi-connector)

  * [Importación de la consulta desde el Explorador de datos de Azure en Power BI](/azure/data-explorer/power-bi-imported-query)
 
  * [Consulta SQL](/azure/data-explorer/power-bi-sql-query).

* [Microsoft Excel](https://products.office.com/excel): El Explorador de datos de Azure proporciona la capacidad de conectarse a Excel mediante el conector de Excel nativo integrado o importar una consulta desde el Explorador de datos de Azure en Excel.

* [Grafana](https://grafana.com): Grafana proporciona un complemento de explorador de datos de Azure que permite visualizar datos desde el Explorador de datos de Azure. Le [configurar el Explorador de datos de Azure como origen de datos de Grafana y, a continuación, visualizar los datos](/azure/data-explorer/grafana)

* [Sisense](https://www.sisense.com): El Explorador de datos de Azure proporciona la capacidad para conectarse a Sisense mediante el conector JDBC. Le [configurar el Explorador de datos de Azure como origen de datos de Sisense y, a continuación, visualizar los datos](/azure/data-explorer/sisense).

* [Tableau](https://www.tableau.com): El Explorador de datos de Azure proporciona la capacidad para conectarse a una plantilla mediante el [conector ODBC y visualizar los datos en Tableau](/azure/data-explorer/connect-odbc).

* [Qlik](https://www.qlik.com): El Explorador de datos de Azure proporciona la capacidad para conectarse a Qlik mediante la [conector ODBC](/azure/data-explorer/connect-odbc).