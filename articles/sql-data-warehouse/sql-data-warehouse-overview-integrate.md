---
title: Creación de soluciones integradas con SQL Data Warehouse | Microsoft Docs
description: 'Herramientas y asociados con soluciones que se integran con SQL Data Warehouse. '
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: integration
ms.date: 04/17/2018
ms.author: martinle
ms.reviewer: igorstan
ms.openlocfilehash: 43a714ae175e0d60f20b5e7ad79e1fa90125b0f8
ms.sourcegitcommit: 4c2b9bc9cc704652cc77f33a870c4ec2d0579451
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/17/2019
ms.locfileid: "65873337"
---
# <a name="integrate-other-services-with-sql-data-warehouse"></a>Integrar otros servicios con SQL Data Warehouse
Además de su funcionalidad básica, SQL Data Warehouse permite a los usuarios integrar muchos de los otros servicios de Azure. Algunos de estos servicios son:

* Power BI
* Azure Data Factory
* Azure Machine Learning
* Azure Stream Analytics

SQL Data Warehouse se sigue integrando con más servicios en Azure y más [asociados de integración](sql-data-warehouse-partner-data-integration.md).

## <a name="power-bi"></a>Power BI
La integración de Power BI le permite combinar la capacidad de procesamiento de SQL Data Warehouse con los informes dinámicos y la visualización de Power BI. La integración de Power BI actualmente incluye:

* **Direct Connect**: una conexión más avanzada con aplicación de lógica en SQL Data Warehouse. La aplicación proporciona un análisis más rápido a mayor escala.
* **Open in Power BI**: El botón "Open in Power BI" (Abrir en Power BI) pasa la información de la instancia a Power BI para lograr una conexión más fluida.

Para más información, consulte [Integración con Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md) o la [documentación de Power BI](https://powerbi.microsoft.com/blog/exploring-azure-sql-data-warehouse-with-power-bi/).

## <a name="azure-data-factory"></a>Azure Data Factory
Azure Data Factory ofrece a los usuarios una plataforma administrada para crear canalizaciones complejas de extracción y carga. La integración de SQL Data Warehouse con Azure Data Factory incluye lo siguiente:

* **Procedimientos almacenados**: coordinar la ejecución de procedimientos almacenados en SQL Data Warehouse.
* **Copia**: use ADF para mover datos a SQL Data Warehouse. Esta operación puede utilizar el mecanismo estándar de movimiento de datos de ADF o PolyBase en segundo plano. 

Para más información, consulte el tema sobre la [integración con Azure Data Factory](https://docs.microsoft.com/azure/data-factory/load-azure-sql-data-warehouse?toc=/azure/sql-data-warehouse/toc.json).

## <a name="azure-machine-learning"></a>Azure Machine Learning
Azure Machine Learning es un servicio de análisis totalmente administrado que permite crear modelos complejos aprovechando un amplio conjunto de herramientas de predicción. SQL Data Warehouse se admite como origen y destino para estos modelos con la siguiente funcionalidad:

* **Lectura de datos**: producir modelos a escala usando T-SQL en SQL Data Warehouse.
* **Escritura de datos**: confirmar los cambios de cualquier modelo en SQL Data Warehouse.

Para más información, consulte el tema sobre la [integración con Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md).

## <a name="azure-stream-analytics"></a>Azure Stream Analytics
Azure Stream Analytics es una infraestructura compleja y totalmente administrada para el procesamiento y consumo de datos de eventos generados por el Centro de eventos de Azure.  La integración con SQL Data Warehouse permite que los datos de transmisión se procesen y almacenen junto con los datos relacionales para permitir un análisis más profundo y avanzado de los datos.  

* **Resultado del trabajo:** enviar los resultados de los trabajos de Stream Analytics directamente a SQL Data Warehouse.

Para más información, consulte el tema sobre la [integración con Azure Stream Analytics](sql-data-warehouse-integrate-azure-stream-analytics.md).


