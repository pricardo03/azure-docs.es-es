---
title: Consideraciones sobre las API | Microsoft Docs
description: Versiones, control de errores y problemas de autorización con las API de Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: c6cfb41cb6254145821ab3fef662e9a5e54f6298
ms.sourcegitcommit: 9eaf634d59f7369bec5a2e311806d4a149e9f425
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2018
ms.locfileid: "48808083"
---
<a name="api-considerations"></a>Consideraciones sobre las API
=================

<a name="api-versioning"></a>Control de versiones de la API
--------------

Puede haber varias versiones de la API disponibles al mismo tiempo. Los clientes deben indicar qué versión quieren invocar para usarla proporcionando el parámetro `api-version` como parte de la cadena de consulta.

   `GET https://cloudpartner.azure.com/api/offerTypes?api-version=2017-10-31`

La respuesta a una solicitud con una versión de API desconocida o no válida es un código HTTP 400. Este error devuelve la colección de versiones de API conocidas en el cuerpo de la respuesta.

``` json
    {
        "error”: { 
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
