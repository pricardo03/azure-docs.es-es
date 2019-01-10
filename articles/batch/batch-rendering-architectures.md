---
title: 'Arquitecturas de referencia de representación de Azure: Azure Batch'
description: Arquitecturas para el uso de Azure Batch y otros servicios de Azure para ampliar una granja de representación local llevándola a la nube
services: batch
author: davefellows
manager: jeconnoc
ms.author: lahugh
ms.date: 08/13/2018
ms.topic: conceptual
ms.custom: seodec18
ms.openlocfilehash: d5102ba94e2b7808a457df00a87b35ef7022c454
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2018
ms.locfileid: "53543502"
---
# <a name="reference-architectures-for-azure-rendering"></a>Arquitecturas de referencia para la representación de Azure

Este artículo muestra detallados diagramas de arquitectura para los escenarios de ampliación de una granja de representación local en Azure. Los ejemplos muestran opciones diferentes para los servicios de proceso, redes y almacenamiento de Azure.

## <a name="hybrid-with-nfs-or-cfs"></a>Híbrido con NFS o CFS

El siguiente diagrama muestra un escenario híbrido que incluye los siguientes servicios de Azure:

* **Proceso**: grupo de Azure Batch o conjunto de escalado de máquinas virtuales.

* **Red**: local: Azure ExpressRoute o VPN. Azure: red virtual de Azure.

* **Almacenamiento**: archivos de entrada y salida: NFS o CFS que usan VM de Azure, sincronizados con el almacenamiento local mediante Azure File Sync o RSync.

  ![Ampliación a la nube: híbrido con NFS o CFS](./media/batch-rendering-architectures/hybrid-nfs-cfs.png)

## <a name="hybrid-with-blobfuse"></a>Híbrido con Blobfuse

El siguiente diagrama muestra un escenario híbrido que incluye los siguientes servicios de Azure:

* **Proceso**: grupo de Azure Batch o conjunto de escalado de máquinas virtuales.

* **Red**: local: Azure ExpressRoute o VPN. Azure: red virtual de Azure.

* **Almacenamiento**: archivos de entrada y salida: Blob Storage, montados para procesar recursos a través de Azure Blobfuse.

  ![Ampliación a la nube: híbrido con Blobfuse](./media/batch-rendering-architectures/hybrid-blob-fuse.png)

## <a name="hybrid-compute-and-storage"></a>Proceso y almacenamiento híbrido

El siguiente diagrama muestra un escenario híbrido completamente conectado de proceso y almacenamiento e incluye los siguientes servicios de Azure:

* **Proceso**: grupo de Azure Batch o conjunto de escalado de máquinas virtuales.

* **Red**: local: Azure ExpressRoute o VPN. Azure: red virtual de Azure.

* **Almacenamiento**: entre locales: Avere vFXT. Archivado opcional de archivos locales a través de Azure Data Box en Blob Storage.

  ![Ampliación en la nube: proceso y almacenamiento híbrido](./media/batch-rendering-architectures/hybrid-compute-storage.png)


## <a name="next-steps"></a>Pasos siguientes

* Más información sobre el uso de [administradores de representación](batch-rendering-render-managers.md) con Azure Batch.

* Más información sobre las opciones de [representación en Azure](batch-rendering-service.md).