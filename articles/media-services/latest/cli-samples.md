---
title: 'Ejemplos de la CLI de Azure: Azure Media Services | Microsoft Docs'
description: Ejemplos de la CLI de Azure para el servicio Azure Media Services
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.custom: seodec18
ms.date: 02/11/2019
ms.author: juliako
ms.openlocfilehash: 919dbd057f2fa1d762775ed799505796a420864a
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56109255"
---
# <a name="azure-cli-examples-for-azure-media-services"></a>Ejemplos de la CLI de Azure para Azure Media Services

En esta tabla se incluyen vínculos a ejemplos de la CLI de Azure para Azure Media Services.

## <a name="examples"></a>Ejemplos

|  |  |
|---|---|
|**Escala**||
| [Escalado de unidades reservadas de multimedia](media-reserved-units-cli-how-to.md)|Para los trabajos de análisis de audio y vídeo desencadenados por Media Services v3 o Video Indexer, se recomienda encarecidamente aprovisionar la cuenta con 10 MRU S3. <br/>El script muestra cómo usar la CLI para escalar Unidades reservadas de multimedia (MRU).|
|**Cuenta**||
| [Creación de una cuenta de Media Services](./scripts/cli-create-account.md) | Crea una cuenta de Azure Media Services. Además, crea una entidad de servicio que se puede usar para tener acceso a las API para administrar la cuenta mediante programación. |
| [Restablecimiento de las credenciales de una cuenta](./scripts/cli-reset-account-credentials.md)|Restablece las credenciales de su cuenta y obtiene la configuración de app.config.|
|**Recursos**||
| [Creación de recursos](./scripts/cli-create-asset.md)|Crea un recurso de Media Services en el que cargar contenido.|
| [Carga de un archivo](./scripts/cli-upload-file-asset.md)|Carga un archivo local en un contenedor de almacenamiento.|
| **Transformaciones** y **trabajos**||
| [Creación de transformaciones](./scripts/cli-create-transform.md)|Muestra cómo crear transformaciones. Las transformaciones describen un flujo de trabajo de tareas simple para procesar archivos de vídeo o audio (que se conoce habitualmente como "receta").<br/> Siempre debe comprobar si ya existe una transformación con el nombre y la "receta" deseados. En caso afirmativo, vuelva a usarla. |
| [Creación de trabajos](./scripts/cli-create-jobs.md)|Envía a un trabajo a una transformación de codificación simple mediante una dirección URL de HTTPs.|
| [Creación de EventGrid](./scripts/cli-create-event-grid.md)|Crea una suscripción a Event Grid de nivel de cuenta para los cambios de estado del trabajo.|
| **Entrega**||
| [Publicación de un recurso](./scripts/cli-publish-asset.md)| Crea un localizador de streaming y obtiene las direcciones URL de streaming. |
| [Filter](filters-dynamic-manifest-cli-howto.md)| Configura un filtro para un recurso Vídeo bajo demanda y muestra cómo usar la CLI para crear [Filtros de cuenta](https://docs.microsoft.com/cli/azure/ams/account-filter?view=azure-cli-latest) y [Filtros de recurso](https://docs.microsoft.com/cli/azure/ams/asset-filter?view=azure-cli-latest). 

## <a name="see-also"></a>Otras referencias

[CLI de Azure](https://docs.microsoft.com/cli/azure/ams?view=azure-cli-latest)
