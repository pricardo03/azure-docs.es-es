---
title: Supervisión de temas y suscripciones de eventos - Azure Event Grid IoT Edge | Microsoft Docs
description: Supervisión de temas y suscripciones de eventos
author: banisadr
ms.author: babanisa
ms.reviewer: spelluru
ms.date: 01/09/2020
ms.topic: article
ms.service: event-grid
services: event-grid
ms.openlocfilehash: 79b223de7a0a0cfdaf799b1f80e585a2a55f7e82
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76851323"
---
# <a name="monitor-topics-and-event-subscriptions"></a>Supervisión de temas y suscripciones de eventos

Event Grid en Edge expone una serie de métricas para los temas y las suscripciones de eventos en el [formato de exposición Prometheus](https://prometheus.io/docs/instrumenting/exposition_formats/). En este artículo se describen las métricas disponibles y cómo habilitarlas.

## <a name="enable-metrics"></a>Habilitación de métricas

Para configurar el módulo y emitir métricas, establezca la variable de entorno `metrics__reporterType` en `prometheus` en las opciones de creación del contenedor:

 ```json
        {
          "Env": [
            "metrics__reporterType=prometheus"
          ],
          "HostConfig": {
            "PortBindings": {
              "4438/tcp": [
                {
                    "HostPort": "4438"
                }
              ]
            }
          }
        }
 ```    

Las métricas estarán disponibles en `5888/metrics` del módulo para HTTP y en `4438/metrics` para HTTPS. Por ejemplo, `http://<modulename>:4438/metrics?api-version=2019-01-01-preview` para HTTP. En este punto, un módulo de métricas puede sondear el punto de conexión para recopilar métricas, como se ilustra en la siguiente [arquitectura de ejemplo](https://github.com/veyalla/ehm).

## <a name="available-metrics"></a>Métricas disponibles

Tanto los temas como las suscripciones de eventos emiten métricas para proporcionarle información sobre la entrega de eventos y el rendimiento de los módulos.

### <a name="topic-metrics"></a>Métricas del tema

| Métrica | Descripción |
| ------ | ----------- |
| EventsReceived | Número de eventos publicados en el tema
| UnmatchedEvents | Número de eventos publicados en el tema que no coinciden con una suscripción de eventos y que se quitan
| SuccessRequests | Número de solicitudes de publicación de entrada que recibe el tema
| SystemErrorRequests | Número de solicitudes de publicación de entrada con errores debido a un error interno del sistema
| UserErrorRequests | Número de solicitudes de publicación de entrada con errores debido a un error de usuario como JSON con formato incorrecto
| SuccessRequestLatencyMs | Latencia de respuesta de la solicitud de publicación en milisegundos


### <a name="event-subscription-metrics"></a>Métricas de la suscripción de eventos

| Métrica | Descripción |
| ------ | ----------- |
| deliverySuccessCounts | Número de eventos entregados correctamente al punto de conexión configurado
| deliveryFailureCounts | Número de intentos de entrega de eventos con errores en el punto de conexión configurado
| deliverySuccessLatencyMs | Latencia de eventos entregados correctamente en milisegundos
| deliveryFailureLatencyMs | Latencia de errores de entrega de eventos en milisegundos
| systemDelayForFirstAttemptMs | Retraso del sistema en eventos antes del primer intento de entrega en milisegundos
| deliveryAttemptsCount | Número de intentos de entrega de eventos (correctos y erróneos)
| expiredCounts | Número de eventos que no se pueden entregar 