---
title: Carga y almacenamiento en la nube con Azure Media Services | Microsoft Docs
description: Este artículo trata los conceptos de carga y almacenamiento en la nube.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 02/21/2019
ms.author: juliako
ms.openlocfilehash: cda029dd11e8cb4cb07e9fce7eef95d6d4d78d7e
ms.sourcegitcommit: fdd6a2927976f99137bb0fcd571975ff42b2cac0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/27/2019
ms.locfileid: "56960230"
---
# <a name="cloud-upload-and-storage"></a>Carga y almacenamiento en la nube

Para iniciar la administración, el cifrado, la codificación, el análisis y el streaming de contenido multimedia en Azure, debe crear una cuenta de Media Services. Al crear una cuenta de Media Services, debe proporcionar el nombre de un recurso de cuenta de Azure Storage. La cuenta de almacenamiento especificada está asociada a su cuenta de Media Services. 

La cuenta de Media Services y todas las cuentas de almacenamiento asociadas deben estar en la misma suscripción de Azure. Se recomienda encarecidamente usar cuentas de almacenamiento que se encuentren en la misma ubicación que la cuenta de Media Services para evitar costos adicionales debidos a la latencia y a la salida de datos.

Debe tener una cuenta de almacenamiento **Principal** y puede tener cualquier número de cuentas de almacenamiento **Secundarias** asociadas a su cuenta de Media Services. Media Services admite cuentas de **Uso general v2** (GPv2) o **Uso general v1** (GPv1). 

>[!NOTE]
> No se permiten cuentas de solo BLOB como **Principal**. 

Se recomienda usar GPv2 para que pueda tener la ventaja de elegir entre niveles de acceso frecuente y niveles de acceso esporádico. Para más información sobre las cuentas de almacenamiento, consulte la [Información general acerca de la cuenta de Azure Storage](../../storage/common/storage-account-overview.md). 

Hay diferentes SKU, puede elegir la cuenta de almacenamiento. Para más información, consulte los comandos [storage accounts](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest). Si quiere experimentar con las cuentas de almacenamiento, use `--sku Standard_LRS`. Sin embargo, al seleccionar una SKU de producción debe considerar `--sku Standard_RAGRS`, que proporciona replicación geográfica para la continuidad empresarial. 

## <a name="assets-in-a-storage-account"></a>Recursos de una cuenta de almacenamiento

En Media Services v3, se usan las API de almacenamiento para cargar archivos en recursos. Para obtener más información, consulte [concepto activos](assets-concept.md).

> [!Note]
> No debe intentar cambiar el contenido de los contenedores de blobs que generó el SDK de Media Services sin usar las API de Media Services.
 
## <a name="next-steps"></a>Pasos siguientes

Para información sobre cómo adjuntar una cuenta de almacenamiento a la cuenta de Media Services, consulte [Creación de una cuenta](create-account-cli-quickstart.md).
