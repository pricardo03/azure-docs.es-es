---
title: Cancelación y eliminación de un trabajo de Azure Import/Export | Microsoft Docs
description: Descubra cómo cancelar y eliminar trabajos del servicio Microsoft Azure Import/Export.
author: muralikk
services: storage
ms.service: storage
ms.topic: article
ms.date: 01/23/2017
ms.author: muralikk
ms.component: common
ms.openlocfilehash: e11cf974a5f33020c889844dd34f51612e13036e
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/06/2018
ms.locfileid: "39521020"
---
# <a name="canceling-and-deleting-azure-importexport-jobs"></a>Cancelación y eliminación de trabajos de Azure Import/Export

 Para solicitar la cancelación de un trabajo antes de que adquiera el estado `Packaging`, llame a la operación [Update Job Properties](/rest/api/storageimportexport/jobs#Jobs_Update) (Actualizar propiedades del trabajo) y establezca el elemento `CancelRequested` en `true`. El trabajo se cancela en función de la mejor opción. Si las unidades están transfiriendo datos, es posible que estos sigan transmitiéndose incluso después de haber solicitado la cancelación.

 Un trabajo cancelado se pasa al estado `Completed` y se mantiene durante 90 días, momento en que se elimina.

 Para eliminar un trabajo, llame a la operación [Delete Job](/rest/api/storageimportexport/jobs#Jobs_Delete) (Eliminar trabajo) antes de que este se haya procesado (es decir, mientras el trabajo esté en el estado `Creating`). También puede eliminar un trabajo cuando se encuentra en el estado `Completed`. Una vez eliminado un trabajo, ya no se podrá acceder a su estado mediante la API de REST o Azure Portal.

[!INCLUDE [storage-import-export-delete-personal-info.md](../../../includes/storage-import-export-delete-personal-info.md)]

## <a name="next-steps"></a>Pasos siguientes

* [Uso de la API de REST del servicio Azure Import/Export](storage-import-export-using-the-rest-api.md)
