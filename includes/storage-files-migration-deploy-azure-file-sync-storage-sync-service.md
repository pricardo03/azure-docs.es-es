---
title: Implementación de un servicio de sincronización de almacenamiento
description: Implementación del recurso de nube de Azure File Sync. Un servicio de sincronización de almacenamiento. Un bloque de texto común, compartido entre los documentos de migración.
author: fauhse
ms.service: storage
ms.topic: conceptual
ms.date: 2/20/2020
ms.author: fauhse
ms.subservice: files
ms.openlocfilehash: 4468d8c6b57b38bdbaaa01208cfc4915f734f6bb
ms.sourcegitcommit: 5192c04feaa3d1bd564efe957f200b7b1a93a381
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2020
ms.locfileid: "78209291"
---
En este paso, necesita las credenciales de la suscripción a Azure. La suscripción a Azure que use puede ser diferente de la que usa para StorSimple.

El recurso principal para configurar Azure File Sync se denomina "servicio de sincronización de almacenamiento".
Se recomienda implementar solo uno para todos los servidores de la empresa que sincronizan el mismo conjunto de archivos ahora o en el futuro. Si tiene más de un dispositivo StorSimple, puede considerar la posibilidad de crear un recurso del servicio de sincronización de almacenamiento para cada uno de ellos. Sin embargo, solo debe crear varios servicios de sincronización de almacenamiento si tiene distintos conjuntos de servidores que nunca deben intercambiar datos. De lo contrario, el procedimiento recomendado es contar con un único servicio de sincronización de almacenamiento.

Elija una región de Azure para el servicio de sincronización de almacenamiento que esté cerca de la ubicación de su oficina. Todos los demás recursos de nube se deben implementar en la misma región.
El procedimiento recomendado consiste en crear un nuevo grupo de recursos en la suscripción para hospedar los recursos de sincronización y almacenamiento a fin de facilitar la administración.

En el siguiente artículo se describe cómo implementar un servicio de sincronización de almacenamiento. Solo siga esta parte del documento. Tendrá vínculos a otros apartados de este documento en pasos posteriores.

[Obtenga información sobre la implementación del servicio de sincronización de almacenamiento.](../articles/storage/files/storage-sync-files-deployment-guide.md#deploy-the-storage-sync-service)
