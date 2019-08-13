---
title: 'Análisis sobre la base de conocimiento: QnA Maker'
titleSuffix: Azure Cognitive Services
description: QnA Maker almacena todos los registros de chat y otra telemetría, si ha habilitado Application Insights durante la creación del servicio QnA Maker. Ejecute las consultas de ejemplo para obtener los registros de chat de Application Insights.
services: cognitive-services
author: diberry
manager: nitinme
displayName: chat history, history, chat logs, logs
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 07/16/2019
ms.author: diberry
ms.openlocfilehash: 5c729065076f5dc9f25189632f42ed565a72df8a
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68563099"
---
# <a name="get-analytics-on-your-knowledge-base"></a>Obtener análisis en la base de conocimiento

QnA Maker almacena todos los registros de chat y otra telemetría, si ha habilitado Application Insights durante la [creación del servicio QnA Maker](./set-up-qnamaker-service-azure.md). Ejecute las consultas de ejemplo para obtener los registros de chat de Application Insights.

1. Vaya al recurso de Application Insights.

    ![Selección del recurso de Application Insights](../media/qnamaker-how-to-analytics-kb/resources-created.png)

2. Seleccione **Analytics**. Se abre una nueva ventana en la que puede consultar la telemetría de QnA Maker.

    ![Seleccione Analytics](../media/qnamaker-how-to-analytics-kb/analytics.png)

3. Pegue la consulta siguiente y ejecútela.

    ```query
    requests
    | where url endswith "generateAnswer"
    | project timestamp, id, name, resultCode, duration, performanceBucket
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | join kind= inner (
    traces | extend id = operation_ParentId
    ) on id
    | extend question = tostring(customDimensions['Question'])
    | extend answer = tostring(customDimensions['Answer'])
    | extend score = tostring(customDimensions['Score'])
    | project timestamp, resultCode, duration, id, question, answer, score, performanceBucket,KbId 
    ```

    Seleccione **Ejecutar** para ejecutar la consulta.

    ![Ejecutar consulta](../media/qnamaker-how-to-analytics-kb/run-query.png)

## <a name="run-queries-for-other-analytics-on-your-qna-maker-knowledge-base"></a>Ejecución de consultas de otros análisis en la base de conocimiento de QnA Maker

### <a name="total-90-day-traffic"></a>Tráfico total de 90 días

```query
    //Total Traffic
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by bin(timestamp, 1d), KbId
```

### <a name="total-question-traffic-in-a-given-time-period"></a>Tráfico total de preguntas en un período de tiempo determinado

```query
    //Total Question Traffic in a given time period
    let startDate = todatetime('2018-02-18');
    let endDate = todatetime('2018-03-12');
    requests
    | where timestamp <= endDate and timestamp >=startDate
    | where url endswith "generateAnswer" and name startswith "POST" 
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer" 
    | summarize ChatCount=count() by KbId
```

### <a name="user-traffic"></a>Tráfico de usuario

```query
    //User Traffic
    requests
    | where url endswith "generateAnswer"
    | project timestamp, id, name, resultCode, duration
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | join kind= inner (
    traces | extend id = operation_ParentId 
    ) on id
    | extend UserId = tostring(customDimensions['UserId'])
    | summarize ChatCount=count() by bin(timestamp, 1d), UserId, KbId
```

### <a name="latency-distribution-of-questions"></a>Distribución de latencia de preguntas

```query
    //Latency distribution of questions
    requests
    | where url endswith "generateAnswer" and name startswith "POST"
    | parse kind = regex name with *"(?i)knowledgebases/"KbId"/generateAnswer"
    | project timestamp, id, name, resultCode, performanceBucket, KbId
    | summarize count() by performanceBucket, KbId
```

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Administrar claves](./key-management.md)
