---
title: 'Ejemplo de script de la CLI de Azure: creación de una suscripción a Azure Event Grid | Microsoft Docs'
description: El script de la CLI de Azure de este tema muestra cómo crear una suscripción de Event Grid a nivel de cuenta para cambios en el estado de trabajo.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.assetid: ''
ms.service: media-services
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/25/2019
ms.author: juliako
ms.openlocfilehash: 2fcacd559db0ab143ffa7ed49bbd546785765154
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/28/2019
ms.locfileid: "55098912"
---
# <a name="cli-example-create-an-azure-event-grid-subscription"></a>Ejemplo de la CLI: creación de una suscripción a Azure Event Grid 

El script de la CLI de Azure de este artículo muestra cómo crear una suscripción de Event Grid a nivel de cuenta para cambios en el estado de trabajo.

## <a name="prerequisites"></a>Requisitos previos 

[Cree una cuenta de Media Services](../create-account-cli-how-to.md).

[!INCLUDE [media-services-cli-instructions.md](../../../../includes/media-services-cli-instructions.md)]

## <a name="example-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/create-event-grid/Create-EventGrid.sh "Create an EventGrid subscription")]

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los [ejemplos de la CLI de Azure](../cli-samples.md).
