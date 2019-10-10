---
title: Incorporación de conjuntos de datos a una versión preliminar de Azure Data Share existente
description: Incorporación de conjuntos de datos a un recurso compartido de datos existente
author: joannapea
ms.author: joanpo
ms.service: data-share
ms.topic: conceptual
ms.date: 07/10/2019
ms.openlocfilehash: bd8cd7af72c349060eb035dc32e9ddd1a7f9920e
ms.sourcegitcommit: e9936171586b8d04b67457789ae7d530ec8deebe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2019
ms.locfileid: "71327520"
---
# <a name="how-to-add-datasets-to-an-existing-share-in-azure-data-share-preview"></a>Incorporación de conjuntos de datos a un recurso compartido existente en la versión preliminar de Azure Data Share

En este artículo se explica cómo agregar conjuntos de datos a un recurso compartido de datos existente mediante la versión preliminar de Azure Data Share. Esto le permite compartir más datos con los mismos destinatarios sin tener que crear un nuevo recurso compartido.

Para información sobre cómo agregar conjuntos de datos a medida que crea un recurso compartido, consulte el tutorial [Uso compartido de datos](share-your-data.md).

## <a name="navigate-to-a-sent-data-share"></a>Desplazamiento a un recurso compartido de datos enviado

En la versión preliminar de Azure Data Share, vaya a su recurso compartido enviado y seleccione la pestaña **Conjuntos de datos**. Haga clic en el botón **+ Add Datasets** (Agregar conjuntos de datos) para agregar más conjuntos de valores.

![Incorporación de conjuntos de datos](./media/how-to/how-to-add-datasets/add-datasets.png)

En el panel de la derecha, seleccione el tipo de conjunto de datos que quiere agregar y haga clic en **Siguiente**. Seleccione la suscripción y el grupo de recursos de los datos que desea agregar. Con las flechas desplegables, busque y active la casilla situada junto a los datos que se van a agregar.

![Incorporación de conjuntos de datos](./media/how-to/how-to-add-datasets/add-datasets-side.png)

Una vez que haga clic en **Add Datasets** (Agregar conjuntos de datos), los conjuntos de datos se agregarán al recurso compartido. Nota: Los consumidores deben desencadenar una instantánea para que puedan ver los nuevos conjuntos de datos. Si hay configuraciones de instantánea definidas, los consumidores verán los nuevos conjuntos de datos una vez que se complete la siguiente instantánea programada. Sin una configuración de instantánea definida, el consumidor debe desencadenar manualmente una copia completa o incremental de los datos para recibir las actualizaciones. Para más información sobre las instantáneas, consulte [Instantáneas](terminology.md).

## <a name="next-steps"></a>Pasos siguientes
Obtenga más información sobre cómo [agregar destinatarios a un recurso compartido de datos existente](how-to-add-recipients.md).