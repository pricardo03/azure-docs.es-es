---
title: Use la CLI para escalar unidades reservadas de multimedia en Azure | Microsoft Docs
description: En este tema se muestra cómo usar la CLI para escalar el procesamiento multimedia con Azure Media Services.
services: media-services
documentationcenter: ''
author: juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2018
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 2b10de83e00b3668f70461f76634c560bcbea1a4
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53133798"
---
# <a name="scaling-media-processing"></a>Escalado de procesamiento de elementos multimedia

Azure Media Services le permite escalar el procesamiento de elementos multimedia en su cuenta mediante la administración de unidades reservadas de multimedia (MRU). Para información más detallada, consulte el [escalado de procesamiento de medios](../previous/media-services-scale-media-processing-overview.md). 

En este artículo se muestra cómo usar la [CLI de Media Services v3](https://aka.ms/ams-v3-cli-ref) para escalar las MRU.

> [!NOTE]
> Para los trabajos de análisis de audio y vídeo desencadenados por Media Services v3 o Video Indexer, se recomienda encarecidamente aprovisionar la cuenta con 10 MRU S3. <br/>Si necesita más de 10 MRU S3, abra una incidencia de soporte técnico desde [Azure Portal](https://portal.azure.com/).

## <a name="prerequisites"></a>Requisitos previos 

- Instale y use la CLI localmente, para este artículo es preciso usar la CLI de Azure versión 2.0 o posterior. Ejecute `az --version` para encontrar la versión que tiene. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](/cli/azure/install-azure-cli). 

    Actualmente no todos los comandos de la [CLI de Media Services v3](https://aka.ms/ams-v3-cli-ref) funcionan en Azure Cloud Shell. Se recomienda usar la CLI localmente.

- [Cree una cuenta de Media Services](create-account-cli-how-to.md).

## <a name="scale-media-reserved-units-with-cli"></a>Escalado de unidades reservadas de multimedia con CLI

El comando [mru de la cuenta de ams az](https://docs.microsoft.com/cli/azure/ams/account/mru?view=azure-cli-latest) siguiente establece las unidades reservadas de multimedia en cuenta "amsaccount" mediante los parámetros **count** y **type**.

```azurecli
az account set mru -n amsaccount -g amsResourceGroup --count 10 --type S3
```

## <a name="billing"></a>Facturación

Se le cobra en función del número, el tipo y la cantidad de tiempo que se aprovisionan las MRU en su cuenta. Los cargos se aplican así ejecute trabajos o no. Para obtener una explicación detallada, vea la sección de preguntas más frecuentes de la página de [precios de Media Services](https://azure.microsoft.com/pricing/details/media-services/).   

## <a name="next-step"></a>Paso siguiente

[Análisis de vídeos](analyze-videos-tutorial-with-api.md) 

## <a name="see-also"></a>Otras referencias

[CLI de Azure](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
