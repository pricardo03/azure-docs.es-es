---
title: Cancelación y eliminación de un trabajo de Azure Import/Export | Microsoft Docs
description: Descubra cómo cancelar y eliminar trabajos del servicio Microsoft Azure Import/Export.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.subservice: common
ms.openlocfilehash: 6eb56413319208feef1b4ab51296fe12a1e0bcf2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "61483152"
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>Cancelación y eliminación de trabajos de Azure Import/Export

 Para solicitar la cancelación de un trabajo antes de que adquiera el estado `Packaging`, llame a la operación [Update Job Properties](/rest/api/storageimportexport/jobs) (Actualizar propiedades del trabajo) y establezca el elemento `CancelRequested` en `true`. El trabajo se cancela en función de la mejor opción. Si las unidades están transfiriendo datos, es posible que estos sigan transmitiéndose incluso después de haber solicitado la cancelación.

 Un trabajo cancelado se pasa al estado `Completed` y se mantiene durante 90 días, momento en que se elimina.

 Para eliminar un trabajo, llame a la operación [Delete Job](/rest/api/storageimportexport/jobs) (Eliminar trabajo) antes de que este se haya procesado (es decir, mientras el trabajo esté en el estado `Creating`). También puede eliminar un trabajo cuando se encuentra en el estado `Completed`. Una vez eliminado un trabajo, ya no se podrá acceder a su estado mediante la API de REST o Azure Portal.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Uso de la API de REST del servicio Azure Import/Export](storage-import-export-using-the-rest-api.md)
