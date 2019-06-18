---
title: Introducción a Azure Queues en Azure Storage
description: Introducción a Azure Queues
services: storage
author: mhopkins-msft
ms.service: storage
ms.topic: overview
ms.date: 06/07/2019
ms.author: mhopkins
ms.reviewer: cbrooks
ms.subservice: queues
ms.openlocfilehash: bc3045e3d3b6977555818fcdb3dcaf3246ebd200
ms.sourcegitcommit: 45e4466eac6cfd6a30da9facd8fe6afba64f6f50
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/07/2019
ms.locfileid: "66754815"
---
# <a name="what-are-azure-queues"></a>¿Qué son las colas de Azure?

Azure Queue Storage es un servicio para almacenar grandes cantidades de mensajes, a los que se puede acceder desde cualquier lugar del mundo a través de llamadas autenticadas mediante HTTP o HTTPS. Un mensaje de la cola puede llegar a tener hasta 64 KB. Una cola puede contener millones de mensajes, hasta el límite de capacidad total de una cuenta de almacenamiento.

## <a name="common-uses"></a>Usos habituales

El almacenamiento en cola suele usarse para realizar las siguientes tareas:

* Creación de trabajo pendiente para el procesamiento asincrónico
* Transferencia de mensajes de un rol web de Azure a un rol de trabajo de Azure

## <a name="queue-service-concepts"></a>Conceptos del servicio Queue

El servicio Cola contiene los siguientes componentes:

![Conceptos de cola](./media/storage-queues-introduction/queue1.png)

* **Formato de dirección URL**: Las colas son direccionables mediante el siguiente formato de dirección URL:

    `https://<storage account>.queue.core.windows.net/<queue>`
  
    La siguiente dirección URL dirige a una cola del diagrama:  
  
    `https://myaccount.queue.core.windows.net/images-to-download`

* **Cuenta de almacenamiento**: Todo el acceso a Azure Storage se realiza a través de una cuenta de almacenamiento. Consulte [Objetivos de escalabilidad y rendimiento de Azure Storage](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) para obtener información sobre la capacidad de la cuenta de almacenamiento.

* **Cola**: una cola contiene un conjunto de mensajes. El nombre de la cola **debe** escribirse en minúsculas. Para más información, consulte [Asignar nombres a colas y metadatos](https://msdn.microsoft.com/library/azure/dd179349.aspx).

* **Mensaje**: un mensaje, en cualquier formato, de hasta 64 KB. Antes de la versión 2017-07-29, el máximo tiempo de vida permitido es de siete días. A partir de la versión del 2017-07-29, inclusive, el tiempo de vida máximo puede ser cualquier número positivo o -1, lo que indica que el mensaje no expira. Si se omite este parámetro, el tiempo de vida predeterminado es siete días.

## <a name="next-steps"></a>Pasos siguientes

* [Cree una cuenta de almacenamiento](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [Introducción a las colas con .NET](storage-dotnet-how-to-use-queues.md)
