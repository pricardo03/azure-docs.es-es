---
title: Detección de fugas de memoria - Detección inteligente de Azure Application Insights
description: Supervise las aplicaciones con Azure Application Insights para detectar posibles fugas de memoria.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 12/12/2017
ms.openlocfilehash: cc6074732a86be02a8d8c596c46e154db4e8ad7f
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/25/2019
ms.locfileid: "75406459"
---
# <a name="memory-leak-detection-preview"></a>Detección de fugas de memoria (versión preliminar)

Application Insights analiza automáticamente el consumo de memoria de cada uno de los procesos de la aplicación y le avisará sobre posibles fugas de memoria o de un aumento en el consumo de memoria.

Esta característica no requiere ninguna configuración especial, excepto la [configuración de los contadores de rendimiento](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) de la aplicación. Se activa cuando la aplicación genera suficiente telemetría en los contadores de rendimiento de memoria (por ejemplo, bytes privados).

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>¿Cuándo recibiré este tipo de notificación de detección inteligente?
Una notificación típica suele generarse por un aumento uniforme del consumo de memoria durante un largo período de tiempo, en uno o más procesos y/o en uno o varios equipos, que forman parte de la aplicación. Los algoritmos de aprendizaje automático se utilizan para detectar un consumo de memoria aumentado que coincida con un patrón de fuga de memoria.

## <a name="does-my-app-really-have-a-problem"></a>Entonces, ¿mi aplicación tiene realmente un problema?
No, una notificación no significa que la aplicación tenga un problema. Aunque los patrones de fuga de memoria generalmente indican un problema de aplicación, estos patrones pueden ser típicos de su proceso específico, o pueden tener una justificación comercial natural, y pueden ignorarse.

## <a name="how-do-i-fix-it"></a>¿Cómo puedo corregirlo?
Las notificaciones incluyen información de diagnóstico para facilitar el proceso de análisis de diagnóstico:
1. **Evaluación de prioridades**. La notificación muestra cuánto aumentó la memoria (en GB) y el intervalo de tiempo en el que se produjo ese aumento. Esto puede ayudarlo a asignar una prioridad al problema.
2. **Ámbito.** ¿Cuántas máquinas mostraron el patrón de fuga de memoria? ¿Cuántas excepciones se desencadenaron durante la posible fuga de memoria? Esta información puede obtenerse de la notificación.
3. **Diagnóstico.** La detección contiene el patrón de fuga de memoria, y muestra el consumo de memoria del proceso a lo largo del tiempo. También puede usar los elementos relacionados y los vínculos de informes para obtener información que puede ayudarle a efectuar un diagnóstico más exhaustivo del problema.
