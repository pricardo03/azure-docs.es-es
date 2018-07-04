---
title: Revisión de los datos de facturación de inscripción de empresa de Azure con API REST | Microsoft Docs
description: Aprenda cómo usar las API REST de Azure para revisar la información de facturación de inscripción de empresa.
services: billing
documentationcenter: na
author: lleonard-msft
manager: MBaldwin
editor: ''
ms.assetid: 82D50B98-40F2-44B1-A445-4391EA9EBBAA
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/06/2018
ms.author: alleonar
ms.openlocfilehash: 046b2e31aaefa5916a42b3652f9e6a8fdceff367
ms.sourcegitcommit: f06925d15cfe1b3872c22497577ea745ca9a4881
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/27/2018
ms.locfileid: "37064694"
---
# <a name="review-enterprise-enrollment-billing-using-rest-apis"></a>Revisión de la facturación de inscripción de empresa mediante las API REST

Las API de informes de Azure le ayudarán a revisar y administrar los costos de Azure.

Aquí, aprenderá a recuperar la factura actual asociada con la inscripción de una cuenta de empresa.

Para recuperar la factura actual:
``` http
GET https://consumption.azure.com/v2/enrollments/{enrollmentID}/usagedetails
Content-Type: application/json   
Authorization: Bearer
```

## <a name="build-the-request"></a>Compilar la solicitud  

El parámetro `{enrollmentID}` es necesario y debe contener el identificador de inscripción para la cuenta de empresa.

Los siguientes encabezados son obligatorios: 

|Encabezado de solicitud|DESCRIPCIÓN|  
|--------------------|-----------------|  
|*Content-Type:*|Necesario. Establézcalo en `application/json`.|  
|*Authorization:*|Necesario. Establézcalo en una `Bearer` [clave de API](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based) válida. |  

Este ejemplo muestra una llamada sincrónica que devuelve detalles para el ciclo de facturación actual. Por razones de rendimiento, las llamadas sincrónicas devuelven información del último mes.  También puede llamar a la [API de forma asincrónica](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail#asynchronous-call-polling-based) para devolver datos durante 36 meses.


## <a name="response"></a>Response  

Se devuelve el código de estado 200 (OK) en el caso de una respuesta correcta, que contiene una lista costos detallados para la cuenta.

``` json
{
    "id": "${id}",
    "data": [
        {
            "cost": ${cost}, 
            "departmentId": ${departmentID},
            "subscriptionGuid" : ${subscriptionGuid} 
            "date": "${date}",
            "tags": "${tags}",
            "resourceGroup": "${resourceGroup}"
        } // ...
    ],
    "nextLink": "${nextLinkURL}"
}
```  

Cada elemento de **datos** representa un cargo:

|Propiedad Response|DESCRIPCIÓN|
|----------------|----------|
|**cost** | Cantidad cobrada, en una moneda apropiada para la ubicación del centro de datos. |
|**subscriptionGuid** | Identificador único global de la suscripción. | 
|**departmentId** | Identificador para el departamento, si lo hay. |
|**date** | Fecha en que se factura el cargo. |
|**etiquetas** | Cadena JSON que contiene las etiquetas asociadas con la suscripción. |
|**resourceGroup**|Nombre del grupo de recursos que contiene el objeto que ha incurrido en el costo. |
|**nextLink**| Cuando se establece, especifica una dirección URL para la "página" siguiente de detalles. En blanco cuando la página es la última. |  
||
  
El administrador de EA define los identificadores de departamento, los grupos de recursos, las etiquetas y los campos relacionados.  

Este ejemplo está abreviado; consulte la [lista de detalles de Get](https://docs.microsoft.com/rest/api/billing/enterprise/billing-enterprise-api-usage-detail) para una descripción completa de cada campo de respuesta. 

Otros códigos de estado indican condiciones de error. En estos casos, el objeto de respuesta explica por qué se ha producido un error en la solicitud.

``` json
{  
  "error": [  
    { "code": "Error type." 
      "message": "Error response describing why the operation failed."  
    }  
  ]  
}  
```  

## <a name="next-steps"></a>Pasos siguientes 
- Revisión de la [introducción a Enterprise Reporting](https://docs.microsoft.com/azure/billing/billing-enterprise-api)
- Investigación de la [API REST de facturación de empresa](https://docs.microsoft.com/rest/api/billing/)   
- [Get started with Azure REST API](https://docs.microsoft.com/rest/api/azure/) (Introducción a la API REST de Azure)   
