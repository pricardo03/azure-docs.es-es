---
title: Go Live | Azure Marketplace
description: Go Live API inicia el proceso de publicación de la oferta.
services: Azure, Marketplace, Cloud Partner Portal,
author: v-miclar
ms.service: marketplace
ms.topic: reference
ms.date: 09/13/2018
ms.author: pabutler
ms.openlocfilehash: ac56f86bad132f3e00a4b5c2507d65c0722c628c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "64935488"
---
<a name="go-live"></a>Go Live
=======

Esta API inicia el proceso para insertar una aplicación en producción. Esta operación es normalmente larga. Esta llamada usa la lista de correos electrónicos de notificación de la operación [Publicar](./cloud-partner-portal-api-publish-offer.md) de la API.

 `POST  https://cloudpartner.azure.com/api/publishers/<publisherId>/offers/<offerId>/golive?api-version=2017-10-31` 

<a name="uri-parameters"></a>Parámetros del identificador URI
--------------

|  **Nombre**      |   **Descripción**                                                           | **Tipo de datos** |
|  --------      |   ---------------                                                           | ------------- |
| publisherId    | Identificador del anunciante de la oferta que se va a recuperar, por ejemplo `contoso`       |  Cadena       |
| offerId        | Identificador de la oferta que se va a recuperar                                   |  Cadena       |
| api-version    | La versión más reciente de la API                                                   |  Date         |
|  |  |  |


<a name="header"></a>Encabezado
------

|  **Nombre**       |     **Valor**       |
|  ---------      |     ----------      |
| Content-Type    | `application/json`  |
| Autorización   | `Bearer YOUR_TOKEN` |
|  |  |


<a name="body-example"></a>Ejemplo de cuerpo
------------

### <a name="response"></a>Response

`Operation-Location: https://cloudpartner.azure.com/api/publishers/contoso/offers/contoso-virtualmachineoffer/operations/56615b67-2185-49fe-80d2-c4ddf77bb2e8`


### <a name="response-header"></a>Encabezado de respuesta

|  **Nombre**             |      **Valor**                                                            |
|  --------             |      ----------                                                           |
| Operation-Location    |  Dirección URL que se puede consultar para determinar el estado actual de la operación            |
|  |  |


### <a name="response-status-codes"></a>Códigos de estado de respuesta

| **Código** |  **Descripción**                                                                        |
| -------- |  ----------------                                                                        |
|  202     | `Accepted`: La solicitud se aceptó correctamente. La respuesta contiene una ubicación para realizar un seguimiento del estado de la operación. |
|  400     | `Bad/Malformed request`: Puede encontrar información adicional sobre el error en el cuerpo de la respuesta. |
|  404     |  `Not found`: La entidad especificada no existe.                                       |
|  |  |
