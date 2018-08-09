---
title: Lista de todos los trabajos de Azure Import/Export | Microsoft Docs
description: Descubra cómo enumerar todos los trabajos del servicio Azure Import/Export de una suscripción.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: 69daac71b69969a7ad9acfeb7095053f8138bf53
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39520887"
---
# <a name="enumerating-jobs-in-the-azure-importexport-service"></a>Enumeración de los trabajos del servicio Azure Import/Export
Para enumerar todos los trabajos de una suscripción, llame a la operación [List Jobs](/rest/api/storageimportexport/jobs#Jobs_List) (Enumerar trabajos). `List Jobs` devuelve una lista de trabajos, así como los siguientes atributos:

-   El tipo de trabajo (importación o exportación)

-   El estado actual del trabajo

-   La cuenta de almacenamiento asociada al trabajo

## <a name="next-steps"></a>Pasos siguientes

* [Uso de la API de REST del servicio Azure Import/Export](storage-import-export-using-the-rest-api.md)
