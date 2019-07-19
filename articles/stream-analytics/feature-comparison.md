---
title: Comparación de características de Azure Stream Analytics
description: En este artículo se comparan las características admitidas en los trabajos en la nube de Azure Stream Analytics y en los trabajos de IoT Edge en Azure Portal, Visual Studio y Visual Studio Code.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: 22d7ef90ee0cf4d09467516b7bb0664327b7dabe
ms.sourcegitcommit: 79496a96e8bd064e951004d474f05e26bada6fa0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2019
ms.locfileid: "67509692"
---
# <a name="azure-stream-analytics-feature-comparison"></a>Comparación de características de Azure Stream Analytics

Con Azure Stream Analytics, puede crear soluciones de streaming en la nube y en IoT Edge mediante [Azure Portal](stream-analytics-quick-create-portal.md), [Visual Studio](stream-analytics-quick-create-vs.md) y [Visual Studio Code](quick-create-vs-code.md). En las tablas de este artículo se muestran las características que se admiten con cada plataforma en ambos tipos de trabajos.

## <a name="cloud-job-features"></a>Características del trabajo en la nube


|Característica  |Portal  |Visual Studio  |Visual Studio Code  |
|---------|---------|---------|---------|
|Multiplataforma     |Mac</br>Linux</br>Windows         |Windows        |Mac</br>Linux</br>Windows          |
|Creación de scripts     |Sí         |Sí         |Sí         |
|Script Intellisense     |Resaltado de sintaxis         |Resaltado de sintaxis</br>Finalización de código</br>Marcador de errores         |Resaltado de sintaxis</br>Finalización de código</br>Marcador de errores         |
|Definir entradas, salidas y configuraciones de trabajos     |Sí         |Sí         |Sí         |
|Particiones de salida de blobs     |Sí         |Sí         |Sí         |
|Power BI como salida     |Sí         |Sí         |Sin         |
|Datos de referencia de SQL Database     |Sí         |Sí         |Sí         |
|Propiedades de mensajes personalizados     |Sí         |No         |Sin         |
|Compartir entradas y salidas entre varias consultas     |Sin         |Sí         |Sí         |
|UDF y UDA de JavaScript     |Sí         |Sí         |Solo Windows         |
|Llamadas de Machine Learning     |Sí, pero la consulta no se puede probar        |Sí, pero no se puede probar localmente         |Sin         |
|Nivel de compatibilidad     |1.0</br>1.1</br>1.2         |1.0</br>1.1</br>1.2          |1.0</br>1.1</br>1.2          |
|Funciones integradas de detección de anomalías basadas en ML     |Sí         |Sí         |Sí         |
|Funciones geoespaciales integradas     |Sí         |Sí         |Sí         |
|Pruebas de consulta con un archivo de ejemplo     |Sí         |Sí         |Sí         |
|Pruebas locales de datos en vivo     |Sin         |Sí         |Sin         |
|Enumerar los trabajos y ver las entidades job     |Sí         |Sí         |Sí         |
|Exportar un trabajo a un proyecto local     |Sin         |Sí         |Sí         |
|Enviar, iniciar y detener trabajos     |Sí         |Sí         |Sí         |
|Control de código fuente     |Sin         |Sí         |Sí         |
|Compatibilidad con CI/CD     |Parcial         |Sí         |Sí         |
|Ver métricas y diagramas de trabajos     |Sí         |Sí         |Abrir en el portal         |
|Ver errores en tiempo de ejecución de trabajos     |Sí         |Sí         |Sin         |
|Registros de diagnóstico     |Sí         |No         |Sin         |


## <a name="iot-edge-job-features"></a>Características de trabajos de IoT Edge

|Característica  |Portal  |Visual Studio  |Visual Studio Code  |
|---------|---------|---------|---------|
|Creación de trabajos     |Sí         |Sí         |Sin         |
|Control de código fuente     |Sin         |Sí         |Sin         |
|Exportar un trabajo a un proyecto local     |Sin         |Sí         |Sin         |
|Pruebas de consulta con un archivo de ejemplo     |Sí         |Sí         |Sin         |
|Compartir entradas y salidas entre varias consultas     |Sin         |Sí         |Sin         |
|UDF de C#     |Sin         |Sí         |Sin         |
|Enviar, iniciar y detener trabajos     |Sí         |Sí         |Sin         |
|Enumerar los trabajos y ver las entidades job     |Sí         |Sí         |Sin         |
|Ver métricas y diagramas de trabajos     |Sí         |Parcial         |Sin         |
|Ver errores en tiempo de ejecución de trabajos     |Sí         |Parcial         |Sin         |
|Compatibilidad con CI/CD     |Sin         |No         |Sin         |


## <a name="next-steps"></a>Pasos siguientes

* [Azure Stream Analytics en IoT Edge](stream-analytics-edge.md)
* [Tutorial: Escritura de una función definida por el usuario en C# para un trabajo de Azure Stream Analytics en IoT Edge (versión preliminar)](stream-analytics-edge-csharp-udf.md)
* [Desarrollo de trabajos para Stream Analytics en IoT Edge mediante las herramientas de Visual Studio](stream-analytics-tools-for-visual-studio-edge-jobs.md)
* [Uso de Visual Studio para ver trabajos de Azure Stream Analytics](stream-analytics-vs-tools.md)
* [Exploración de Azure Stream Analytics con Visual Studio Code (versión preliminar)](vscode-explore-jobs.md)


