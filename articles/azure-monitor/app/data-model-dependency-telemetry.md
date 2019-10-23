---
title: 'Modelo de datos de telemetría de Azure Application Insights: telemetría de dependencias | Microsoft Docs'
description: Modelo de datos de Application Insights para la telemetría de dependencias
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 04/17/2017
ms.reviewer: sergkanz
ms.openlocfilehash: e0923c20b11aa02f380af1faa6766d2346ad1fb2
ms.sourcegitcommit: 1bd2207c69a0c45076848a094292735faa012d22
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/21/2019
ms.locfileid: "72677429"
---
# <a name="dependency-telemetry-application-insights-data-model"></a>Telemetría de dependencia: Modelo de datos de Application Insights

La telemetría de dependencias (en [Application Insights](../../azure-monitor/app/app-insights-overview.md)) representa una interacción del componente supervisado con un componente remoto como SQL o un punto de conexión HTTP.

## <a name="name"></a>NOMBRE

Nombre del comando que se inició con esta llamada de dependencia. Valor de cardinalidad bajo. Algunos ejemplos son el nombre del procedimiento almacenado y la plantilla de ruta de acceso de dirección URL.

## <a name="id"></a>id

Identificador de una instancia de llamada de dependencia. Se usa para la correlación con el elemento de telemetría de solicitud correspondiente a esta llamada de dependencia. Para más información, vea la página de [correlación](../../azure-monitor/app/correlation.md).

## <a name="data"></a>Datos

Comando iniciado por esta llamada de dependencia. Algunos ejemplos son la instrucción SQL y la dirección URL HTTP con todos los parámetros de consulta.

## <a name="type"></a>type

Nombre del tipo de dependencia. Valor de cardinalidad bajo para una agrupación lógica de dependencias y la interpretación de otros campos como commandName y resultCode. Algunos ejemplos son SQL, tabla de Azure y HTTP.

## <a name="target"></a>Destino

Sitio de destino de una llamada de dependencia. Algunos ejemplos son el nombre del servidor y la dirección de host. Para obtener más información, vea la página de [correlación](../../azure-monitor/app/correlation.md).

## <a name="duration"></a>Duration

Duración de la solicitud en formato: `DD.HH:MM:SS.MMMMMM`. Debe ser menor de `1000` días.

## <a name="result-code"></a>Código de resultado

Código de resultado de una llamada de dependencia. Algunos ejemplos son el código de error SQL y el código de estado HTTP.

## <a name="success"></a>Correcto

Indicación de si la llamada es correcta o no.

## <a name="custom-properties"></a>Propiedades personalizadas

[!INCLUDE [application-insights-data-model-properties](../../../includes/application-insights-data-model-properties.md)]

## <a name="custom-measurements"></a>Medidas personalizadas

[!INCLUDE [application-insights-data-model-measurements](../../../includes/application-insights-data-model-measurements.md)]


## <a name="next-steps"></a>Pasos siguientes

- Configure el seguimiento de dependencias para [.NET](../../azure-monitor/app/asp-net-dependencies.md).
- Configure el seguimiento de dependencias para [Java](../../azure-monitor/app/java-agent.md).
- [Escritura de una telemetría de dependencia personalizada](../../azure-monitor/app/api-custom-events-metrics.md#trackdependency)
- Consulte [modelo de datos](data-model.md) para los tipos y el modelo de datos de Application Insights.
- Consulte las [plataformas](../../azure-monitor/app/platforms.md) compatibles con Application Insights.
