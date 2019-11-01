---
title: Consideraciones sobre las API | Azure Marketplace
description: Versiones, control de errores y problemas de autorización con las API de Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: 762c90b62ed2a9347ae88a50a11bfe02f3b23ba4
ms.sourcegitcommit: 0b1a4101d575e28af0f0d161852b57d82c9b2a7e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/30/2019
ms.locfileid: "73162669"
---
# <a name="api-considerations"></a>Consideraciones sobre las API


<a name="api-versioning"></a>Control de versiones de la API
--------------

Puede haber varias versiones de la API disponibles al mismo tiempo. Los clientes deben indicar qué versión quieren invocar para usarla proporcionando el parámetro `api-version` como parte de la cadena de consulta.

   `GET https://cloudpartner.azure.com/api/offerTypes?api-version=2017-10-31`

La respuesta a una solicitud con una versión de API desconocida o no válida es un código HTTP 400. Este error devuelve la colección de versiones de API conocidas en el cuerpo de la respuesta.

``` json
    {
        "error": { 
            "code":"InvalidAPIVersion",
            "message":"Invalid api version. Allowed values are [2016-08-01-preview]"
        }
    }
```            

<a name="errors"></a>Errors
------

La API responde a los errores con los códigos de estado HTTP correspondientes y, opcionalmente, información adicional en la respuesta serializada como JSON.
Cuando reciba un error, especialmente un error de clase 400, no vuelva a intentar la solicitud antes de corregir la causa subyacente. Por ejemplo, en la respuesta de ejemplo anterior, corrija el parámetro de la versión de la API antes de volver a enviar la solicitud.

<a name="authorization-header"></a>Encabezado de autorización
--------------------

Para todas las API de esta referencia, debe pasar el encabezado de autorización junto con el token de portador obtenido de Azure Active Directory (Azure AD). Este encabezado se requiere para recibir una respuesta válida. Si no está presente, se devuelve un error `401 Unauthorized`. 

``` HTTP
  GET https://cloudpartner.azure.com/api/offerTypes?api-version=2016-08-01-preview

    Accept: application/json 
    Authorization: Bearer <YOUR_TOKEN> 
```
