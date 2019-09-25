---
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/26/2018
ms.author: tamram
ms.openlocfilehash: 935a6201e24e97deaa0a1a65e499310f74a49e2d
ms.sourcegitcommit: ca359c0c2dd7a0229f73ba11a690e3384d198f40
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71059037"
---
## <a name="what-is-queue-storage"></a>¿Qué es Queue Storage?

El almacenamiento en cola de Azure es un servicio para almacenar grandes cantidades de mensajes a los que puede obtenerse acceso desde cualquier lugar del mundo a través de llamadas autenticadas con HTTP o HTTPS. Un único mensaje en cola puede tener un tamaño de hasta 64 KB y una cola puede contener millones de mensajes, hasta el límite de capacidad total de una cuenta de almacenamiento. Queue Storage se usa normalmente para crear un trabajo pendiente del trabajo que se va a procesar de forma asincrónica.

## <a name="queue-service-concepts"></a>Conceptos del servicio Queue

Azure Queue service contiene los siguientes componentes:

![Cola1](./media/storage-queue-concepts-include/queue1.png)

* **Formato de dirección URL**: Las colas son direccionables mediante el siguiente formato de dirección URL:   
    http://`<storage account>`.queue.core.windows.net/`<queue>` 
  
    La siguiente dirección URL dirige a una cola del diagrama:  
  
    `http://myaccount.queue.core.windows.net/images-to-download`

* **Cuenta de almacenamiento**: Todo el acceso a Azure Storage se realiza a través de una cuenta de almacenamiento. Consulte [Objetivos de escalabilidad y rendimiento de Azure Storage](../articles/storage/common/storage-scalability-targets.md) para obtener información sobre la capacidad de la cuenta de almacenamiento.
* **Cola**: una cola contiene un conjunto de mensajes. Todos los mensajes deben encontrarse en una cola. Tenga en cuenta que el nombre de la cola debe ir en minúsculas. Para más información, consulte [Asignar nombres a colas y metadatos](https://msdn.microsoft.com/library/azure/dd179349.aspx).
* **Mensaje**: un mensaje, en cualquier formato, de hasta 64 KB. El tiempo máximo que un mensaje puede permanecer en la cola es de 7 días.

