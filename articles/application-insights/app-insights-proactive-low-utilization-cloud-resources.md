---
title: 'Detección inteligente: Azure Application Insights detectó un uso reducido de los recursos de nube | Microsoft Docs'
description: Supervise las aplicaciones con Azure Application Insights para detectar un uso reducido de los recursos de nube.
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.assetid: ea2a28ed-4cd9-4006-bd5a-d4c76f4ec20b
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: conceptual
ms.date: 12/12/2017
ms.author: mbullwin
ms.openlocfilehash: ca4f944f605db96a2cedf2682f3ff4c811007ffb
ms.sourcegitcommit: cc4fdd6f0f12b44c244abc7f6bc4b181a2d05302
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47094919"
---
# <a name="low-cpu-utilization-in-cloud-resources-preview"></a>Uso reducido de CPU en los recursos en la nube (versión preliminar)

Application Insights analiza automáticamente el consumo de CPU de cada instancia de rol de la aplicación y detecta instancias con un uso de CPU reducido. Esta detección permite reducir los recursos de Azure y reducir costos, reduciendo el número de instancias de rol que utiliza cada rol, o reduciendo el número de roles.

Esta característica no requiere ninguna configuración especial, excepto la [configuración de los contadores de rendimiento](https://docs.microsoft.com/azure/application-insights/app-insights-performance-counters) de la aplicación. Está activa cuando la aplicación genera suficiente telemetría en los contadores de rendimiento de CPU (% de tiempo de procesador).

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>¿Cuándo recibiré este tipo de notificación de detección inteligente?
Una notificación típica suele generarse cuando muchas de las instancias de rol de trabajo o web muestran un uso de CPU reducido.

## <a name="does-my-app-definitely-consume-too-many-resources"></a>¿Significa esto que mi aplicación consume demasiados recursos?
No, una notificación no significa que la aplicación consuma demasiados recursos. Aunque estos patrones de uso de CPU reducido normalmente indican que se puede reducir el consumo de recursos, este comportamiento podría ser normal para su determinado rol o podría tener una justificación comercial natural, y pueden ignorarse. Por ejemplo, es posible que se necesitan varias instancias para otros recursos, como memoria o red, y no CPU.

## <a name="how-do-i-fix-it"></a>¿Cómo puedo corregirlo?
Las notificaciones incluyen información de diagnóstico para facilitar el proceso de diagnóstico:
1. **Evaluación de errores.** La notificación muestra los roles de la aplicación que muestran un uso de CPU reducido. Esto puede ayudarlo a asignar una prioridad al problema.
2. **Ámbito.** ¿Cuántos roles muestran un uso de CPU reducido y cuántas instancias en cada rol tienen un uso de CPU reducido? Esta información puede obtenerse de la notificación.
3. **Diagnóstico.** La detección contiene el porcentaje de CPU utilizado, que muestra el uso de CPU de cada instancia a lo largo del tiempo. También puede usar los elementos relacionados y los vínculos de informes para obtener información, como percentiles del uso de CPU, que puede ayudarle a efectuar un diagnóstico exhaustivo del problema.
