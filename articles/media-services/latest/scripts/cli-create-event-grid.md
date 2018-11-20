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
ms.date: 11/11/2018
ms.author: juliako
ms.openlocfilehash: a3cff649001adf569f1454d16a2a97b32972ef00
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/13/2018
ms.locfileid: "51612633"
---
# <a name="cli-example-create-an-azure-event-grid-subscription"></a>Ejemplo de CLI: creación de una suscripción a Azure Event Grid 

El script de la CLI de Azure de este artículo muestra cómo crear una suscripción de Event Grid a nivel de cuenta para cambios en el estado de trabajo.

## <a name="prerequisites"></a>Requisitos previos 

- Instale y use la CLI localmente, para este artículo es preciso usar la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión que tiene. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). 

    Actualmente no todos los comandos de la [CLI de Media Services v3](https://aka.ms/ams-v3-cli-ref) funcionan en Azure Cloud Shell. Se recomienda usar la CLI localmente.

- [Cree una cuenta de Media Services](../create-account-cli-how-to.md).

## <a name="example-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/create-event-grid/Create-EventGrid.sh "Create an EventGrid subscription")]

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los [ejemplos de la CLI de Azure](../cli-samples.md).
