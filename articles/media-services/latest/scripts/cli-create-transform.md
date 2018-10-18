---
title: 'Ejemplo de script de la CLI de Azure: creación de una transformación | Microsoft Docs'
description: Utilice el script de la CLI de Azure para crear una transformación.
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
ms.date: 10/16/2018
ms.author: juliako
ms.openlocfilehash: 57079e1086b473347ff027a98b794f4c9baeb1c0
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/17/2018
ms.locfileid: "49376574"
---
# <a name="cli-example-create-a-transform"></a>Ejemplo de la CLI: creación de una transformación

El script de la CLI de Azure de este artículo muestra cómo crear una transformación. Las transformaciones describen un flujo de trabajo simple de tareas para procesar los archivos de vídeo o audio (que se conoce habitualmente como "receta"). Siempre debe comprobar si ya existe una transformación con el nombre y la "receta" elegidos. Si es así, debe volver a utilizarlos.

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0.20 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). 

## <a name="example-script"></a>Script de ejemplo

[!code-azurecli-interactive[main](../../../../cli_scripts/media-services/create-transform/Create-Transform.sh "Create a transform")]

## <a name="next-steps"></a>Pasos siguientes

Para más información, consulte los [ejemplos de la CLI de Azure](../cli-samples.md).
