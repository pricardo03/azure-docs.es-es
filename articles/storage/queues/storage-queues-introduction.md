---
title: Introducción a Azure Queues en Azure Storage
description: Introducción a Azure Queues
services: storage
author: mhopkins-msft
ms.service: storage
ms.topic: overview
ms.date: 02/06/2019
ms.author: mhopkins
ms.reviewer: cbrooks
ms.subservice: queues
ms.openlocfilehash: 544ff9d2c624ef62bf8041afd818153c1c4bfcc8
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65142510"
---
# <a name="what-are-azure-queues"></a>¿Qué son las colas de Azure?

El almacenamiento en cola de Azure es un servicio para almacenar grandes cantidades de mensajes a los que puede obtenerse acceso desde cualquier lugar del mundo a través de llamadas autenticadas con HTTP o HTTPS. Un único mensaje en cola puede tener un tamaño de hasta 64 KB y una cola puede contener millones de mensajes, hasta el límite de capacidad total de una cuenta de almacenamiento.

## <a name="common-uses"></a>Usos habituales

El almacenamiento en cola suele usarse para realizar las siguientes tareas:

* Creación de trabajo pendiente para el procesamiento asincrónico
* Transferencia de mensajes de un rol web de Azure a un rol de trabajo de Azure

## <a name="queue-service-concepts"></a>Conceptos del servicio Queue

El servicio Cola contiene los siguientes componentes:

![Conceptos de cola](./media/storage-queues-introduction/queue1.png)

* **Formato de dirección URL**: Las colas son direccionables mediante el siguiente formato de dirección URL:   
    https://`<storage account>`.queue.core.windows.net/`<queue>` 
  
    La siguiente dirección URL dirige a una cola del diagrama:  
  
    `https://myaccount.queue.core.windows.net/images-to-download`

* **Cuenta de almacenamiento**: Todo el acceso a Azure Storage se realiza a través de una cuenta de almacenamiento. Consulte [Objetivos de escalabilidad y rendimiento de Azure Storage](../common/storage-scalability-targets.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json) para obtener información sobre la capacidad de la cuenta de almacenamiento.

* **Cola**: una cola contiene un conjunto de mensajes. Todos los mensajes deben encontrarse en una cola. Tenga en cuenta que el nombre de la cola debe ir en minúsculas. Para más información, consulte [Asignar nombres a colas y metadatos](https://msdn.microsoft.com/library/azure/dd179349.aspx).

* **Mensaje**: un mensaje, en cualquier formato, de hasta 64 KB. El tiempo máximo que un mensaje puede permanecer en la cola es de siete días.

## <a name="next-steps"></a>Pasos siguientes

* [Cree una cuenta de almacenamiento](../storage-create-storage-account.md?toc=%2fazure%2fstorage%2fqueues%2ftoc.json)
* [Introducción a las colas con .NET](storage-dotnet-how-to-use-queues.md)
