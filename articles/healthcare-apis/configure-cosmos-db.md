---
title: Configuración de Cosmos DB en Azure API for FHIR
description: En este artículo se describe cómo configurar Cosmos DB en Azure API for FHIR
author: matjazl
ms.service: healthcare-apis
ms.topic: reference
ms.date: 05/22/2019
ms.author: matjazl
ms.openlocfilehash: 3d4f2ade64c55c757634e9c2b68aa9dcef51f117
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67033764"
---
# <a name="configure-cosmos-db-settings"></a>Configuración de Cosmos DB 

Azure API for FHIR usa Cosmos DB como base de datos subyacente para almacenar datos FHIR. Al aprovisionar el nuevo servicio Azure API for FHIR tiene la posibilidad de especificar el número de unidades de solicitud (RU) para Cosmos DB que usará el servicio.

Para más información acerca de las unidades de solicitud de Cosmos DB, lea [Unidades de solicitud en Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/request-units)

Para cambiar esta configuración en Azure Portal, puede ir a Azure API for FHIR y abrir la hoja de Cosmos DB y, luego, cambiar el rendimiento al valor deseado según sus necesidades. Puede cambiar el valor hasta un máximo de 10 000 RU/s.* Si necesita un valor mayor, póngase en contacto con el equipo de soporte técnico de Azure.

***Nota:** Un valor mayor significa un mayor rendimiento de Cosmos DB y un costo mayor del servicio.

![](media/cosmosdb/cosmosdb-config.png)
