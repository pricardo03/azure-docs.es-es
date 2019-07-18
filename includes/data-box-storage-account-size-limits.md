---
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: include
ms.date: 06/18/2019
ms.author: alkohli
ms.openlocfilehash: 0975df8ee9da4a239dbd72e468b967c88ab3feed
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2019
ms.locfileid: "67277461"
---
Estos son los límites del tamaño de los datos que se copian en la cuenta de almacenamiento. Asegúrese de que los datos que carga se ajustan a estos límites. Para tener la información más actualizada sobre estos límites, vaya a [Objetivos de escalabilidad de Azure Blob Storage](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-blob-storage-scale-targets) y [Objetivos de escalabilidad de Azure Files](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets#azure-files-scale-targets).

| Tamaño de los datos que se copian en la cuenta de almacenamiento de Azure                      | Límite predeterminado          |
|---------------------------------------------------------------------|------------------------|
| Blob en bloques y blob en páginas                                            | 2 PB para Estados Unidos y Europa.<br>500 TB para todas las demás regiones, que incluye Reino Unido.  <br> Aquí se incluyen datos de todos los orígenes, incluido Data Box Disk.|
| Archivos de Azure                                                          | 5 TB por recurso compartido.<br> Todas las carpetas de *StorageAccount_AzureFiles* deben seguir este límite.       |
