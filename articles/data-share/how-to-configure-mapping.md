---
title: Configuración de una asignación de conjunto de datos en Azure Data Share
description: Aprenda a configurar una asignación de conjunto de datos para un recurso compartido recibido en Azure Data Share.
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: 754977788c5f6e5e574500552f670ba9083cf683
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490621"
---
# <a name="how-to-configure-a-dataset-mapping-for-a-received-share-in-azure-data-share"></a>Configuración de una asignación de conjunto de datos para un recurso compartido recibido en Azure Data Share

En este artículo se explica cómo configurar una asignación de conjunto de datos para un recurso compartido recibido en Azure Data Share. Deberá hacer esto si aceptó una invitación de un recurso compartido de datos pero seleccionó "Accept and configure later" (Aceptar y configurar más adelante). Es posible que quiera configurar una asignación de conjunto de datos si necesita cambiar el destino de los datos que se comparten con usted o si quiere recibir datos en una instancia de SQL Server. 

## <a name="navigate-to-a-received-data-share"></a>Vaya a un recurso compartido de datos recibido.

En el servicio Azure Data Share, vaya al recurso compartido recibido y seleccione la pestaña **Detalles**. 

![Asignación de conjuntos de datos](./media/dataset-mapping.png "Asignación de conjuntos de datos") 

Active la casilla situada junto al conjunto de datos al que desearía asignar un destino. Seleccione **Unmap** (Desasignar) para desasignar la asignación existente. Seleccione **+ Map to target** (+ Asignar a destino) para elegir un nuevo almacén de destino. 

![Asignación a destino](./media/dataset-map-target.png "Asignar a destino") 

## <a name="select-a-new-destination-store"></a>Selección de un almacén de destino nuevo

Seleccione un tipo de datos de destino en el que desee colocar los datos. Tenga en cuenta que todos los datos que ya existan en cualquier cuenta de almacenamiento ya asignada no se moverán automáticamente al destino nuevo.

![Cuenta de almacenamiento de destino](./media/dataset-map-target-sql.png "Almacenamiento de destino") 

## <a name="select-a-file-format-sql-sources-only"></a>Selección de un formato de archivo (solo orígenes de SQL)

Si los datos de origen proceden de un origen basado en SQL, puede elegir el formato en el que se reciben. 

![Elegir formato](./media/sql-file-formats.png "Formatos de archivo SQL")

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de cómo empezar a compartir datos, vaya al tutorial que cubre cómo [compartir sus datos](share-your-data.md).



