---
title: Descripción de las métricas de Azure Spring Cloud
description: Aprenda a revisar las métricas en Azure Spring Cloud
author: jpconnock
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/06/2019
ms.author: jeconnoc
ms.openlocfilehash: 952dfc1c707df3b7fa61443ea6ea18630352f0dc
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607807"
---
# <a name="metrics-for-azure-spring-cloud"></a>Métricas en Azure Spring Cloud

El explorador de métricas de Azure Monitor es un componente de Azure Portal que permite trazar gráficos, correlacionar tendencias visualmente e investigar máximos y mínimos en las métricas. Utilice el Explorador de métricas para investigar el estado y la utilización de recursos. En Azure Spring Cloud, ofrecemos dos opciones para ver las métricas: los gráficos de la página **Información general de aplicación** y la página Métricas del nivel de servicio.

## <a name="application-overview-page"></a>Página Información general de aplicación

La página **Información general de aplicación** de cada aplicación muestra un gráfico de métricas que le permite realizar una comprobación rápida del estado de la aplicación.  Vaya a la página del servicio Azure Spring Cloud y seleccione **Panel de la aplicación** y, después, seleccione una aplicación en la lista.  

Cada minuto se proporcionan cinco gráficos con métricas de los siguientes valores:

* **Errores de servidor HTTP**: recuento de errores en las solicitudes HTTP a su aplicación.
* **Entrada de datos**: los bytes que ha recibido su aplicación.
* **Salida de datos**: los bytes que se han enviado a su aplicación.
* **Solicitud**: las solicitudes que ha recibido su aplicación.
* **Tiempo promedio de respuesta**: tiempo promedio de respuesta de su aplicación.

Puede seleccionar un intervalo de tiempo para el gráfico de entre 1 hora y 7 días.

## <a name="service-level-metric-queries"></a>Consultas de métricas de nivel de servicio

Azure Spring Cloud permite supervisar una variedad de métricas de la aplicación. Para más información acerca de este servicio, consulte la [guía de introducción](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-getting-started) con las métricas de Azure Monitor.

Para examinar los datos de métricas, seleccione la métrica, la **agregación** y el intervalo de tiempo.  Estos conceptos se explican a continuación.

### <a name="aggregation"></a>Agregación 

Azure sondea y actualiza las métricas cada minuto. Azure proporciona tres formas de agregar datos para un período seleccionado:

* **Total**: suma todas las métricas como salida del destino.
* **Average**: use el valor promedio del período como salida del destino.
* **Máx./mín.** : Use el valor Máx./mín. del período como salida del destino.

### <a name="time-range"></a>Intervalo de tiempo

Seleccione un intervalo de tiempo predeterminado o defina el suyo propio.

### <a name="modifying-the-granularity-of-your-metric-query"></a>Modificación de la granularidad de la consulta de métricas

De forma predeterminada, Azure agrega las métricas de todas las aplicaciones de un servicio Azure Spring Cloud. Para examinar las métricas en el nivel de aplicación o de instancia, utilice la función de filtro.  
Seleccione **Agregar filtro**, establezca la propiedad en **Aplicación** y seleccione la aplicación de destino que desea supervisar. Opcionalmente, use la opción **Apply splitting** (Aplicar división) para dibujar líneas independientes para cada aplicación del gráfico.

>[!TIP]
> Puede crear sus propios gráficos en la página de métricas y anclarlos a su **panel**. Empiece por asignar un nombre al gráfico.  Después, seleccione **Anclar al panel en la esquina superior derecha**. Ya puede comprobar la aplicación en el **panel** del portal.