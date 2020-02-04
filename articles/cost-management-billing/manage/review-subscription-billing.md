---
title: Revisión de los datos de facturación de suscripción de Azure con API REST | Microsoft Docs
description: Obtenga información acerca de cómo usar las API REST de Azure para revisar los detalles de facturación de la suscripción.
services: billing
documentationcenter: na
author: lleonard-msft
manager: ''
editor: ''
ms.assetid: 82D50B98-40F2-44B1-A445-4391EA9EBBAA
ms.service: cost-management-billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/01/2019
ms.author: banders
ms.openlocfilehash: 4dc7771862bc922702c5cc321fb3c566212d799a
ms.sourcegitcommit: 67e9f4cc16f2cc6d8de99239b56cb87f3e9bff41
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2020
ms.locfileid: "75985220"
---
# <a name="review-subscription-billing-using-rest-apis"></a>Revisión de la facturación de suscripción mediante las API REST

Las API de informes de Azure le ayudarán a revisar y administrar los costos de Azure.

Los filtros ayudan a personalizar los resultados para satisfacer sus necesidades.

Aquí, aprenderá a utilizar una API REST para devolver los detalles de facturación de la suscripción para un rango de fechas determinado.

``` http
GET https://management.azure.com/subscriptions/${subscriptionID}/providers/Microsoft.Billing/billingPeriods/${billingPeriod}/providers/Microsoft.Consumption/usageDetails?$filter=properties/usageEnd ge '${startDate}' AND properties/usageEnd le '${endDate}'
Content-Type: application/json
Authorization: Bearer
```

## <a name="build-the-request"></a>Compilar la solicitud

El parámetro `{subscriptionID}` es necesario y se identifica la suscripción de destino.

El parámetro `{billingPeriod}` es necesario y especifica un [período de facturación](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-billing-periods) actual.

Los parámetros `${startDate}` y `${endDate}` son necesarios para este ejemplo, pero opcionales para el punto de conexión. Especifican el rango de fechas como cadenas en formato AAAA-MM-DD (ejemplos: `'20180501'` y `'20180615'`).

Los siguientes encabezados son obligatorios:

|Encabezado de solicitud|Descripción|
|--------------------|-----------------|
|*Content-Type:*|Necesario. Establézcalo en `application/json`.|
|*Authorization:*|Necesario. Establézcalo en un [token de acceso](https://docs.microsoft.com/rest/api/azure/#authorization-code-grant-interactive-clients) `Bearer` válido. |

## <a name="response"></a>Response

Se devuelve el código de estado 200 (OK) en el caso de una respuesta correcta, que contiene una lista costos detallados para la cuenta.

``` json
{
  "value": [
    {
      "id": "/subscriptions/{$subscriptionID}/providers/Microsoft.Billing/billingPeriods/201702/providers/Microsoft.Consumption/usageDetails/{$detailsID}",
      "name": "{$detailsID}",
      "type": "Microsoft.Consumption/usageDetails",
      "properties": {
        "billingPeriodId": "/subscriptions/${subscriptionID}/providers/Microsoft.Billing/billingPeriods/${billingPeriod}",
        "invoiceId": "/subscriptions/${subscriptionID}/providers/Microsoft.Billing/invoices/${invoiceID}",
        "usageStart": "${startDate}}",
        "usageEnd": "${endDate}",
        "currency": "USD",
        "usageQuantity": "${usageQuantity}",
        "billableQuantity": "${billableQuantity}",
        "pretaxCost": "${cost}",
        "meterId": "${meterID}",
        "meterDetails": "${meterDetails}"
      }
    }
  ],
  "nextLink": "${nextLinkURL}"
}
```

Cada elemento de **valor** representa un detalle sobre el uso de un servicio:

|Propiedad Response|Descripción|
|----------------|----------|
|**subscriptionGuid** | Identificador único global de la suscripción. |
|**startDate** | Fecha de inicio del uso. |
|**endDate** | Fecha de finalización del uso. |
|**useageQuantity** | Cantidad usada. |
|**billableQuantity** | Cantidad facturada realmente. |
|**pretaxCost** | Costo facturado, antes de impuestos aplicables. |
|**meterDetails** | La información detallada sobre el uso. |
|**nextLink**| Cuando se establece, especifica una dirección URL para la "página" siguiente de detalles. En blanco cuando la página es la última. |

Este ejemplo está abreviado; consulte los [detalles de uso de List](https://docs.microsoft.com/rest/api/consumption/usagedetails/list#usagedetailslistforbillingperiod-legacy) para una descripción completa de cada campo de respuesta.

Otros códigos de estado indican condiciones de error. En estos casos, el objeto de respuesta explica por qué se ha producido un error en la solicitud.

``` json
{
  "error": [
    {
      "code": "Error type.",
      "message": "Error response describing why the operation failed."
    }
  ]
}
```

## <a name="next-steps"></a>Pasos siguientes
- Revisión de la [introducción a Enterprise Reporting](https://docs.microsoft.com/azure/billing/billing-enterprise-api)
- Investigación de la [API REST de facturación de empresa](https://docs.microsoft.com/rest/api/billing/)
- [Get started with Azure REST API](https://docs.microsoft.com/rest/api/azure/) (Introducción a la API REST de Azure)
