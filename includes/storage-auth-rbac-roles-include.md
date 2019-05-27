---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/21/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 026717dff2b6883eb643497dec91226e4afe8133
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66115582"
---
Azure proporciona los siguientes roles RBAC integrados para autorizar el acceso a datos blob y cola con Azure AD y OAuth:

- [Propietario del almacenamiento de datos Blob](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-owner): se usa para establecer la propiedad y administrar el control de acceso POSIX en Azure Data Lake Storage Gen2 (versión preliminar). Para más información, consulte [Control de acceso en Azure Data Lake Storage Gen2](../articles/storage/blobs/data-lake-storage-access-control.md).
- [Colaborador de datos de Blob de almacenamiento](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-contributor): se usa para conceder permisos de lectura, escritura y eliminación a los recursos de almacenamiento de blobs.
- [Lector de datos de Blob de almacenamiento](../articles/role-based-access-control/built-in-roles.md#storage-blob-data-reader): se usa para conceder permisos de solo lectura a los recursos de almacenamiento de blobs.
- [Colaborador de datos de almacenamiento cola](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-contributor): se usa para conceder permisos de lectura, escritura y eliminación a las colas de Azure.
- [Lector de datos de almacenamiento cola](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-reader): se usa para conceder permisos de solo lectura a las colas de Azure.
- [Procesador de mensajes de datos de la cola de almacenamiento](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-processor): Use grant peek, recuperar y permisos de eliminación para los mensajes en colas de Azure Storage.
- [Remitente del mensaje de datos de la cola de almacenamiento](../articles/role-based-access-control/built-in-roles.md#storage-queue-data-message-sender): Usar para conceder agregar permisos a los mensajes en colas de Azure Storage.

> [!NOTE]
> Tenga en cuenta que las asignaciones de roles RBAC pueden tardar hasta cinco minutos en propagarse.
