---
title: Uso de Azure Stream Analytics
description: Sugerencias para usar Azure Stream Analytics con Azure SQL Data Warehouse para el desarrollo de soluciones.
services: sql-data-warehouse
author: mlee3gsd
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: integration
ms.date: 03/22/2019
ms.author: martinle
ms.reviewer: igorstan
ms.custom: seo-lt-2019
ms.openlocfilehash: a655ada93cd9db9db95295d445c0b4f27d772148
ms.sourcegitcommit: f52ce6052c795035763dbba6de0b50ec17d7cd1d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2020
ms.locfileid: "76721207"
---
# <a name="use-azure-stream-analytics-with-azure-synapse-analytics"></a>Uso de Azure Stream Analytics con Azure Synapse Analytics
Azure Stream Analytics es un servicio totalmente administrado que proporciona un procesamiento completo de eventos de baja latencia, alta disponibilidad y escalable a través el streaming de datos en la nube. Para aprender los conceptos básicos, lea [Introducción a Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md). Después puede aprender a crear una solución de extremo a extremo siguiendo el tutorial [Introducción al uso de Azure Stream Analytics](../stream-analytics/stream-analytics-real-time-fraud-detection.md) .

En este artículo, aprenderá a usar la base de datos de almacenamiento de datos como receptor de salida para los trabajos de Stream Analytics.

## <a name="prerequisites"></a>Prerequisites
En primer lugar, ejecute los pasos siguientes del tutorial [Introducción al uso de Azure Stream Analytics](../stream-analytics/stream-analytics-real-time-fraud-detection.md) .  

1. Creación de una entrada de Centro de eventos
2. Configuración e inicio de la aplicación del generador de eventos
3. Aprovisionamiento de un trabajo de Stream Analytics
4. Especificación de una consulta y entrada de trabajo

Luego cree una base de datos de Azure SQL Data Warehouse.

## <a name="specify-job-output-azure-sql-data-warehouse-database"></a>Especificación de la salida del trabajo: Base de datos de Azure SQL Data Warehouse
### <a name="step-1"></a>Paso 1
En el trabajo de Stream Analytics, haga clic en **SALIDA** en la parte superior de la página y luego en **AGREGAR**.

### <a name="step-2"></a>Paso 2
Seleccione SQL Database.

### <a name="step-3"></a>Paso 3
Escriba estos valores en la página siguiente:

* *Alias de salida*: escriba un nombre descriptivo para esta salida de trabajo.
* *Suscripción*:
  * Si la base de datos de SQL Data Warehouse está en la misma suscripción que el trabajo de Stream Analytics, seleccione Usar SQL Database de la suscripción actual".
  * Si la base de datos está en una suscripción diferente, seleccione Usar SQL Database de otra suscripción.
* *Base de datos*: especifique el nombre de una base de datos de destino.
* *Server Name* (Nombre del servidor): especifique el nombre del servidor para la base de datos especificada. Para encontrarlo, puede usar Azure Portal.

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png)

* *Nombre de usuario*: especifique el nombre de usuario de una cuenta que tenga permisos de escritura para la base de datos.
* *Contraseña*: proporcione la contraseña de la cuenta de usuario especificada.
* *Tabla*: especifique el nombre de la tabla de destino en la base de datos.

![](./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png)

### <a name="step-4"></a>Paso 4
Haga clic en el botón de comprobación para agregar esta salida de trabajo y comprobar que Stream Analytics puede conectarse correctamente a la base de datos.

Cuando la conexión a la base de datos se realice correctamente, verá una notificación en el portal. Puede hacer clic en Probar para probar la conexión a la base de datos.

## <a name="next-steps"></a>Pasos siguientes
Para obtener información general sobre la integración, consulte [Integrar otros servicios](sql-data-warehouse-overview-integrate.md).
Para obtener más consejos de desarrollo, consulte [Decisiones de diseño y técnicas de codificación para almacenamientos de datos](sql-data-warehouse-overview-develop.md).

