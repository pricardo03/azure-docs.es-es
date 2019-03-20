---
title: API de recuperación de ofertas | Microsoft Docs
description: La API recupera una lista resumida de ofertas de un espacio de nombres del anunciante.
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
ms.topic: reference
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: de9261548ec79e206b0db87caabc1fa4c9ad6771
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/21/2019
ms.locfileid: "56591557"
---
<a name="retrieve-offers"></a>Recuperación de ofertas
===============

Recupera una lista resumida de ofertas de un espacio de nombres del anunciante.

 `GET https://cloudpartner.azure.com/api/publishers/<publisherId>/offers?api-version=2017-10-31`


<a name="uri-parameters"></a>Parámetros del identificador URI
--------------

| **Nombre**         |  **Descripción**                         |  **Tipo de datos** |
| -------------    |  ------------------------------------    |  -----------   |
|  publisherId     | Identificador del publicador, por ejemplo `contoso`. |   string    |
|  api-version     | Última versión de la API.                    |    Date        |
|  |  |


<a name="header"></a>Encabezado
------

|  **Nombre**        |         **Valor**       |
|  --------------- |       ----------------  |
|  Content-Type    | `application/json`      |
|  Autorización   | `Bearer YOUR_TOKEN`     |
|  |  |


<a name="body-example"></a>Ejemplo de cuerpo
------------

### <a name="response"></a>Response

``` json
  200 OK 
  [ 
      {  
          "offerTypeId": "microsoft-azure-virtualmachines",
          "publisherId": "contoso",
          "status": "published",
          "id": "059afc24-07de-4126-b004-4e42a51816fe",
          "version": 1,
          "definition": {
              "displayText": "Contoso Virtual Machine"
          },
          "changedTime":"2017-05-23T23:33:47.8802283Z"
      }
  ]
```

### <a name="response-body-properties"></a>Propiedades del cuerpo de la respuesta

|  **Nombre**       |       **Descripción**                                                                                                  |
|  -------------  |      --------------------------------------------------------------------------------------------------------------    |
|  offerTypeId    | Identifica el tipo de oferta                                                                                           |
|  publisherId    | El identificador que distingue de manera exclusiva al anunciante                                                                      |
|  status         | Estado de la oferta. Para obtener la lista de valores posibles, consulte la sección [Estado de la oferta](#offer-status) a continuación.                         |
|  id             | GUID que identifica de forma exclusiva a la oferta del espacio de nombres del anunciante.                                                    |
|  version        | Versión actual de la oferta. El cliente no puede modificar la propiedad de versión. Se incrementa después de cada publicación. |
|  definición     | Contiene una vista resumida de la definición real de la carga de trabajo. Para obtener una definición detallada, use la API de [Recuperación de una oferta específica](./cloud-partner-portal-api-retrieve-specific-offer.md). |
|  changedTime    | Hora UTC en la que se modificó por última vez la oferta                                                                              |
|  |  |


### <a name="response-status-codes"></a>Códigos de estado de respuesta

| **Código**  |  **Descripción**                                                                                                   |
| -------   |  ----------------------------------------------------------------------------------------------------------------- |
|  200      | `OK`: La solicitud se ha procesado correctamente y todas las ofertas del anunciante se han devuelto al cliente.  |
|  400      | `Bad/Malformed request`: El cuerpo de la respuesta del error puede contener más información.                                    |
|  403      | `Forbidden`: El cliente no tiene acceso al espacio de nombres especificado.                                          |
|  404      | `Not found`: La entidad especificada no existe.                                                                 |
|  |  |


### <a name="offer-status"></a>Estado de la oferta

|  **Nombre**                    | **Descripción**                                  |
|  ------------------------    | -----------------------------------------------  |
|  NeverPublished              | La oferta nunca se ha publicado.                  |
|  NotStarted                  | La oferta es nueva y no se ha iniciado.                 |
|  WaitingForPublisherReview   | La oferta está esperando la aprobación del anunciante.         |
|  En ejecución                     | Se está procesando el envío de la oferta.             |
|  Succeeded                   | Se terminó de procesar el envío de la oferta.       |
|  Canceled                    | Se canceló el envío de la oferta.                   |
|  Con error                      | Error en el envío de la oferta.                         |
|  |  |
