---
title: archivo de inclusión
description: archivo de inclusión
services: scheduler
ms.service: scheduler
author: derek1ee
ms.topic: include
ms.date: 08/16/2016
ms.author: deli
ms.custom: include file
ms.openlocfilehash: be52bf289898dfcd3ee669f12329669f6fabd356
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47006525"
---
En la tabla siguiente se describe cada uno de los principales aceleradores, valores predeterminados, límites y cuotas de Azure Scheduler.

| Recurso | Descripción de límites |
| -------- | ----------------- |
| **Tamaño del trabajo** | El tamaño máximo del trabajo es 16 K. Si una operación PUT o PATCH produce un tamaño de trabajo mayor que este límite, se devuelve un código de estado 400 Solicitud incorrecta. | 
| **Colecciones de trabajos** | El número máximo de colecciones de trabajos por suscripción de Azure es 200 000. | 
| **Trabajos por colección** | De forma predeterminada, el número máximo de trabajos es de cinco trabajos en una colección de trabajos gratuita y de 50 trabajos en una colección de trabajos estándar. <p>Puede cambiar el número máximo de trabajos en una colección de trabajos. Todos los trabajos de una colección de trabajos se limitan al valor establecido en la colección de trabajos. Si intenta crear un número de trabajos superior a la cuota máxima de trabajos, la solicitud genera un error con un código de estado 409 Conflicto. | 
| **Tiempo hasta la hora de inicio** | El tiempo hasta la hora de inicio máximo es de 18 meses. |
| **Intervalo de periodicidad** | El intervalo máximo de periodicidad es de 18 meses. | 
| **Frecuencia** | De forma predeterminada, la cuota máxima de frecuencia es de una hora en una colección de trabajos gratuita y de un minuto en una colección de trabajos estándar. <p>Puede hacer que la frecuencia máxima de una colección de trabajos sea inferior a la máxima. Todos los trabajos de la colección de trabajos se limitan al valor establecido en la colección de trabajos. Si intenta crear un trabajo con una frecuencia mayor que la frecuencia máxima en la colección de trabajos, la solicitud genera un error con un código de estado 409 Conflicto. | 
| **Tamaño del cuerpo** | El tamaño del cuerpo máximo de una solicitud es de 8192 caracteres. |
| **Tamaño de la dirección URL de la solicitud** | El tamaño máximo de la dirección URL de la solicitud es de 2048 caracteres. |
| **Recuento de encabezados** | El recuento máximo de encabezados es de 50 encabezados. | 
| **Tamaño del encabezado de agregado** | El tamaño máximo del encabezado de agregado es de 4096 caracteres. |
| **Tiempo de espera** | El tiempo de espera de solicitud es estático (no configurable) y es de 60 segundos para las acciones de HTTP. Para operaciones más largas, siga los protocolos asincrónicos HTTP. Por ejemplo, devolver un error 202 inmediatamente pero continuar trabajando en segundo plano. | 
| **Historial de trabajos** | El tamaño máximo del cuerpo de la respuesta que se almacena en el historial de trabajos es de 2048 bytes. |
| **Retención de historial de trabajos** | El historial de trabajos se conserva hasta dos meses o hasta las últimas 1000 ejecuciones. | 
| **Retención de trabajos completados y con errores** | Los trabajos completados y con errores se conservan durante 60 días. |
||| 

