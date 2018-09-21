---
title: Mover datos hacia y desde Azure Blob Storage | Microsoft Docs
description: Mover datos hacia y desde Azure Blob Storage
services: machine-learning,storage
documentationcenter: ''
author: deguhath
manager: jhubbard
editor: cgronlun
ms.assetid: d6681e30-ab45-45ea-a9fb-ac8acefe544d
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: deguhath
ms.openlocfilehash: 854c671d4db6cdca2b019ed9adb0475e588281b1
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2018
ms.locfileid: "35622558"
---
# <a name="move-data-to-and-from-azure-blob-storage"></a>Movimiento de datos hacia y desde Azure Blob Storage
[!INCLUDE [cap-ingest-data-selector](../../../includes/cap-ingest-data-selector.md)]

<!-- just in case, adding this to separate these two include references -->

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

El método más adecuado para usted dependerá de su escenario. El artículo [Escenarios para análisis avanzado en Azure Machine Learning](plan-sample-scenarios.md) lo ayudará a determinar los recursos que necesita para una variedad de flujos de trabajo de ciencia de datos utilizados en el proceso de análisis avanzado.

> [!NOTE]
> Para ver una introducción completa a Azure Blob Storage, consulte [Aspectos básicos de Azure Blob](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) y [Azure Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).
> 
> 

Como alternativa, puede usar [Azure Data Factory](https://azure.microsoft.com/services/data-factory/) para: 

* crear y programar una canalización que descarga los datos desde Azure Blob Storage, 
* pasarla a un servicio web Azure Machine Learning publicado, 
* recibir los resultados de análisis predictivo y 
* cargar los resultados al almacenamiento. 

Consulte [Creación de canalizaciones predictivas mediante Factoría de datos de Azure y Azure Machine Learning](../../data-factory/transform-data-using-machine-learning.md)para obtener más información.

## <a name="prerequisites"></a>Requisitos previos
En este documento se supone que tiene una suscripción de Azure y una cuenta de almacenamiento y la clave de almacenamiento correspondiente para dicha cuenta. Antes de cargar o descargar datos, debe conocer su nombre de cuenta de almacenamiento de Azure y la clave de cuenta.

* Para configurar una suscripción a Azure, consulte [Prueba gratuita de un mes](https://azure.microsoft.com/pricing/free-trial/).
* Para obtener instrucciones sobre la creación de una cuenta de almacenamiento y para obtener información de cuentas y claves, vea [Acerca de las cuentas de almacenamiento de Azure](../../storage/common/storage-create-storage-account.md).

