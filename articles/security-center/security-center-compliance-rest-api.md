---
title: Supervisión del cumplimiento de Azure Security Center mediante la API REST | Microsoft Docs
description: Revise los resultados de los exámenes de cumplimiento automatizados con la API REST de Azure Security Center.
services: security-center
documentationcenter: na
author: rloutlaw
manager: angerobe
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2018
ms.author: rloutlaw
ms.openlocfilehash: 651d7737258f1b1b17c8392a09882388ddf95635
ms.sourcegitcommit: 58c5cd866ade5aac4354ea1fe8705cee2b50ba9f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2018
ms.locfileid: "42819572"
---
# <a name="review-security-center-compliance-results-using-the-azure-rest-apis"></a>Revisión de los resultados de cumplimiento de Security Center mediante las API REST de Azure

En este artículo, aprenderá a recuperar la información de cumplimiento de seguridad para una lista de suscripciones que usan las API REST de Azure.

## <a name="review-compliance-for-each-subscription"></a>Revisión del cumplimiento de cada suscripción

En el ejemplo siguiente se obtiene información de valoración de la seguridad para un cumplimiento y una suscripción determinados mediante la operación [Obtener cumplimientos](/rest/api/securitycenter/compliances/get).

```http
GET https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Security/compliances/{complianceName}?api-version=2017-08-01-preview
```

El parámetro `{complianceName}` es necesario y debe contener el nombre del cumplimiento evaluado para `{subscription-id}`. La salida contendrá los resultados de la valoración, como:

```json
{
...
  "properties": {
    "assessmentResult": [
      {
        "segmentType": "Compliant",
        "percentage": 77.777777777777786
      }
    ],
    "resourceCount": 18,
    "assessmentTimestampUtcDate": "2018-01-01T00:00:00Z"
  }
}
```

## <a name="review-compliance-for-multiple-subscriptions"></a>Revisión del cumplimiento de varias suscripciones

Para obtener datos de varias suscripciones, realice la siguiente llamada para obtener primero una lista de suscripciones que usan la operación [Enumerar suscripciones](/rest/api/resources/subscriptions/list). A continuación, invoque la operación anterior [Obtener cumplimientos](/rest/api/securitycenter/compliances/get) para cada uno de los identificadores de suscripción devueltos.

La llamada API es:

```http
GET https://management.azure.com/subscriptions?api-version=2016-06-01
```

Que devuelve una matriz de valores como los siguientes. Use los valores de subscriptionId de la llamada anterior para revisar la información de cumplimiento de todas las suscripciones.

```json
"value": [
    {
      "id": "/subscriptions/{subscription-id}",
      "subscriptionId": "{subscription-id}",
      "displayName": "Demo subscription",
      ...
    }
```






