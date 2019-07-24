---
title: Configuración de una asignación de conjunto de datos
description: Configuración de una asignación de conjunto de datos
author: joannapea
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.author: joanpo
ms.openlocfilehash: 581e1eef5f1d64e68a6501f56ce60218281c605d
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67788567"
---
# <a name="how-to-configure-a-dataset-mapping-for-a-received-share-in-azure-data-share-preview"></a>Configuración de una asignación de conjunto de datos para un recurso compartido recibido en Azure Data Share (versión preliminar)

En este artículo se explica cómo configurar una asignación de conjunto de datos para un recurso compartido recibido en Azure Data Share (versión preliminar). Deberá hacer esto si aceptó una invitación de un recurso compartido de datos pero seleccionó "Accept and configure later" (Aceptar y configurar más adelante). En caso contrario, puede cambiar la cuenta de almacenamiento de destino para los datos recibidos. 

## <a name="navigate-to-a-received-data-share"></a>Vaya a un recurso compartido de datos recibido.

En el servicio Azure Data Share, vaya al recurso compartido recibido y seleccione la pestaña **Detalles**. 

![Asignación de conjunto de datos](./media/dataset-mapping.png "Dataset mapping") 

Seleccione la casilla situada junto al conjunto de datos al que desearía asignar un destino y haga clic en **+ Map to target** (+ Asignar a destino). Es posible que primero deba anular la asignación si ya había configurado una cuenta de almacenamiento de destino y desea cambiar la asignación a una cuenta diferente. 

![Asignar a destino](./media/dataset-map-target.png "Map to target") 

## <a name="select-a-new-storage-account"></a>Selección de una nueva cuenta de almacenamiento 

Seleccione la cuenta de almacenamiento a la que le gustaría que llegaran los datos. Tenga en cuenta que todos los datos que ya existan en cualquier cuenta de almacenamiento ya asignada no se moverán automáticamente a la nueva cuenta de almacenamiento.

![Cuenta de almacenamiento de destino](./media/map-target.png "Target storage") 

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de cómo empezar a compartir datos, vaya al tutorial que cubre cómo [compartir sus datos](share-your-data.md).



