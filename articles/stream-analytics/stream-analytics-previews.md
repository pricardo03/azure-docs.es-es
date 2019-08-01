---
title: Características en vista previa (GB) de Azure Stream Analytics
description: En este artículo se enumeran las características de Azure Stream Analytics que están actualmente en versión preliminar.
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/29/2019
ms.openlocfilehash: e63937cedf44b1642e091a4744d898a26422be10
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66393687"
---
# <a name="azure-stream-analytics-preview-features"></a>Características en vista previa (GB) de Azure Stream Analytics

En este artículo se resumen todas las características actualmente en versión preliminar de Azure Stream Analytics. No se recomienda el uso de características en vista previa en un entorno de producción.

## <a name="public-previews"></a>Versiones preliminares públicas

Las características siguientes se encuentran en la versión preliminar pública. Puede aprovechar las ventajas de estas características hoy mismo, pero no las use en su entorno de producción.

### <a name="visual-studio-code-for-azure-stream-analytics-released-may-2019"></a>Visual Studio Code para Azure Stream Analytics (publicado en mayo de 2019)

Los trabajos de Azure Stream Analytics se pueden crear en Visual Studio Code. Consulte el [tutorial de introducción de VS Code](https://docs.microsoft.com/azure/stream-analytics/quick-create-vs-code).

### <a name="anomaly-detection"></a>Detección de anomalías

Azure Stream Analytics introduce nuevos modelos de aprendizaje automático con compatibilidad para a detección de *picos* y *caídas*, además de detección de tendencias bidireccionales, positivas lentas y negativas lentas. Para más información, visite [Detección de anomalías en Azure Stream Analytics](stream-analytics-machine-learning-anomaly-detection.md).

### <a name="integration-with-azure-machine-learning"></a>Integración con Azure Machine Learning

Puede escalar los trabajos de Stream Analytics con las funciones de Machine Learning (ML). Para obtener más información acerca de cómo puede usar las funciones de Machine Learning en un trabajo de Stream Analytics, visite [Escalado del trabajo de Análisis de transmisiones con funciones de Azure Machine Learning](stream-analytics-scale-with-machine-learning-functions.md). Eche un vistazo a un escenario realista con [Análisis de opiniones mediante Azure Stream Analytics y Azure Machine Learning](stream-analytics-machine-learning-integration-tutorial.md).

### <a name="javascript-user-defined-aggregate"></a>Agregado definido por el usuario en JavaScript

Azure Stream Analytics admite agregados definidos por el usuario (UDA) escritos en JavaScript, que le permiten implementar lógica empresarial con estado compleja. Obtenga información sobre cómo usar los UDA en la documentación de [Agregados definidos por el usuario en JavaScript para Azure Stream Analytics](stream-analytics-javascript-user-defined-aggregates.md). 

### <a name="live-data-testing-in-visual-studio"></a>Prueba de datos activos en Visual Studio

Las herramientas de Visual Studio para Azure Stream Analytics mejoran la función de pruebas locales que le permite realizar pruebas en las consultas en comparación con los flujos de datos de eventos activos desde fuentes en la nube como un centro de eventos o un centro de IoT. Aprenda cómo realizar una [Prueba local de datos activos mediante herramientas de Azure Stream Analytics para Visual Studio](stream-analytics-live-data-local-testing.md).

### <a name="net-user-defined-functions-on-iot-edge"></a>Funciones definidas por el usuario de .NET en IoT Edge

Con las funciones definidas por el usuario de . NET Standard, puede ejecutar código de .NET Standard como parte de la canalización de transmisiones de datos. Puede crear clases de C# simples o importar bibliotecas y proyectos completos. Las funciones de creación y depuración completas se admiten en Visual Studio. Para obtener más información, visite [Desarrollar funciones definidas por el usuario de .NET Standard para trabajos perimetrales de Azure Stream Analytics](stream-analytics-edge-csharp-udf-methods.md).

## <a name="other-previews"></a>Otras versiones preliminares

En la versión preliminar también están disponibles las características siguientes.

### <a name="c-custom-deserializer-for-azure-stream-analytics-on-iot-edge-and-cloud-announced-may-2019"></a>Deserializador personalizado en C# para Azure Stream Analytics en IoT Edge y Cloud (anunciado en mayo de 2019)

Los desarrolladores pueden implementar deserializadores personalizados en C# para deserializar los eventos recibidos por Azure Stream Analytics. Parquet, Protobuf, XML y todos los formatos binarios son algunos ejemplos de formatos que se pueden deserializar. Regístrese para esta versión preliminar [aquí](https://aka.ms/asapreview1).

### <a name="parquet-output-announced-may-2019"></a>Salida de Parquet (anunciada en mayo de 2019)
Parquet es un formato de columnas que habilita un eficaz procesamiento de los macrodatos. Al generar los datos en formato Parquet en un lago de datos, puede aprovechar las ventajas de Azure Stream Analytics para ETL de streaming de gran escala y para ejecutar el procesamiento por lotes, entrenar los algoritmos de aprendizaje automático o ejecutar consultas interactivas en los datos históricos. Regístrese para esta versión preliminar [aquí](https://aka.ms/asapreview1).

### <a name="one-click-integration-with-event-hubs-announced-may-2019"></a>Integración en un solo clic con Event Hubs (anunciado en mayo de 2019) 
Con esta integración, ahora podrá visualizar los datos entrantes y empezar a escribir una consulta de Stream Analytics con un solo clic desde el portal de Event Hubs. Una vez que la consulta esté lista, podrá usarla con unos pocos clics y comenzar a obtener conclusiones en tiempo real. De este modo, se reducirá significativamente el tiempo y el costo del desarrollo de soluciones de análisis en tiempo real. Regístrese para esta versión preliminar [aquí](https://aka.ms/asapreview1).

### <a name="support-for-azure-stack-announced-may-2019"></a>Compatibilidad con Azure Stack (anunciado en mayo de 2019)
Esta característica habilitada en el entorno de ejecución de Azure IoT Edge aprovecha las características personalizadas de Azure Stack, como la compatibilidad nativa con las entradas locales y las salidas que se ejecutan en Azure Stack (por ejemplo, Event Hubs, IoT Hub, Blob Storage). Esta nueva integración le permite crear arquitecturas híbridas que pueden analizar los datos cerca de donde se generan, disminuyen la latencia y maximizan las conclusiones.
Regístrese para esta versión preliminar [aquí](https://aka.ms/asapreview1).

