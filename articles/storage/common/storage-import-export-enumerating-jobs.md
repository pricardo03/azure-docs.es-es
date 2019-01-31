---
title: Lista de todos los trabajos de Azure Import/Export | Microsoft Docs
description: Descubra cómo enumerar todos los trabajos del servicio Azure Import/Export de una suscripción.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 017208c6fca7c4e55a45070f5aa21652e83e7e8d
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55462933"
---
# <a name="enumerating-jobs-in-the-azure-importexport-service"></a>Enumeración de los trabajos del servicio Azure Import/Export
Para enumerar todos los trabajos de una suscripción, llame a la operación [List Jobs](/rest/api/storageimportexport/jobs#Jobs_List) (Enumerar trabajos). `List Jobs` devuelve una lista de trabajos, así como los siguientes atributos:

-   El tipo de trabajo (importación o exportación)

-   El estado actual del trabajo

-   La cuenta de almacenamiento asociada al trabajo

## <a name="next-steps"></a>Pasos siguientes

* [Uso de la API de REST del servicio Azure Import/Export](storage-import-export-using-the-rest-api.md)
