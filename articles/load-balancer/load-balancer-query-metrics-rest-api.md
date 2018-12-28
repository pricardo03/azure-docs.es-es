---
title: Recuperación de métricas con la API de REST
titlesuffix: Azure Load Balancer
description: Use las API REST de Azure para recopilar métricas de mantenimiento y de utilización de Load Balancer para un intervalo de tiempo y fechas determinados.
services: sql-database
author: KumudD
ms.reviewer: routlaw
manager: jeconnoc
ms.service: load-balancer
ms.custom: REST, seodec18
ms.topic: article
ms.date: 06/06/2017
ms.author: KumudD
ms.openlocfilehash: 43de0de6fb81d50459ba5b027a8ae1718a89c9a3
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53183869"
---
# <a name="get-load-balancer-utilization-metrics-using-the-rest-api"></a>Obtención de métricas de utilización de Load Balancer con la API REST

Este procedimiento muestra cómo recopilar el número de bytes procesados por un servicio [Standard Load Balancer](/azure/load-balancer/load-balancer-standard-overview) para un intervalo de tiempo mediante la [API REST de Azure](/rest/api/azure/).

Hay unna completa documentación de referencia y ejemplos adicionales para la API REST en [Azure Monitor REST reference](/rest/api/monitor) (Referencia de REST de Azure Monitor). 

## <a name="build-the-request"></a>Compilar la solicitud

Utilice la siguiente solicitud GET para recopilar la [métrica de ByteCount](/azure/load-balancer/load-balancer-standard-diagnostics#a-name--multidimensionalmetricsamulti-dimensional-metrics) desde un servicio Standard Load Balancer. 

```http
GET https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}/providers/microsoft.insights/metrics?api-version=2018-01-01&metricnames=ByteCount&timespan=2018-06-05T03:00:00Z/2018-06-07T03:00:00Z
```

### <a name="request-headers"></a>Encabezados de solicitud

Los siguientes encabezados son obligatorios: 

|Encabezado de solicitud|DESCRIPCIÓN|  
|--------------------|-----------------|  
|*Content-Type:*|Necesario. Establézcalo en `application/json`.|  
|*Authorization:*|Necesario. Establézcalo en un [token de acceso](/rest/api/azure/#authorization-code-grant-interactive-clients) `Bearer` válido. |  

### <a name="uri-parameters"></a>Parámetros del identificador URI

| NOMBRE | DESCRIPCIÓN |
| :--- | :---------- |
| subscriptionId | El id. de suscripción que identifica una suscripción de Azure. Si tiene varias suscripciones, consulte [Trabajo con varias suscripciones](https://docs.microsoft.com/cli/azure/manage-azure-subscriptions-azure-cli?view=azure-cli-latest#working-with-multiple-subscriptions). |
| resourceGroupName | Nombre del grupo de recursos que contiene el recurso. Puede obtener este valor en la API de Azure Resource Manager, la CLI o en Azure Portal. |
| loadBalancerName | El nombre de Azure Load Balancer. |
| metricnames | Lista separada por comas de [métricas válidas de Load Balancer](/azure/load-balancer/load-balancer-standard-diagnostics). |
| api-version | La versión de API que se usará para la solicitud.<br /><br /> En este documento se describe la versión `2018-01-01` de la API que se incluye en la dirección URL anterior.  |
| timespan | El intervalo de tiempo de la consulta. Es una cadena con el siguiente formato `startDateTime_ISO/endDateTime_ISO`. Este parámetro opcional se establece para devolver los datos recopilados durante un día en el ejemplo. |
| &nbsp; | &nbsp; |

### <a name="request-body"></a>Cuerpo de la solicitud

No se necesita ningún cuerpo de solicitud para esta operación.

## <a name="handle-the-response"></a>Control de la respuesta

Cuando la lista de valores de métricas se devuelve correctamente, se devuelve el código de estado 200. Una lista completa de códigos de error está disponible en la [documentación de referencia](/rest/api/monitor/metrics/list#errorresponse).

## <a name="example-response"></a>Respuesta de ejemplo 

```json
{
    "cost": 0,
    "timespan": "2018-06-05T03:00:00Z/2018-06-07T03:00:00Z",
    "interval": "PT1M",
    "value": [
        {
            "id": "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Network/loadBalancers/{loadBalancerName}/providers/Microsoft.Insights/metrics/ByteCount",
            "type": "Microsoft.Insights/metrics",
            "name": {
                "value": "ByteCount",
                "localizedValue": "Byte Count"
            },
            "unit": "Count",
            "timeseries": [
                {
                    "metadatavalues": [],
                    "data": [
                        {
                            "timeStamp": "2018-06-06T17:24:00Z",
                            "total": 1067921034.0
                        },
                        {
                            "timeStamp": "2018-06-06T17:25:00Z",
                            "total": 0.0
                        },
                        {
                            "timeStamp": "2018-06-06T17:26:00Z",
                            "total": 3781344.0
                        },
                    ]
                }
            ]
        }
    ],
    "namespace": "Microsoft.Network/loadBalancers",
    "resourceregion": "eastus"
}
```