---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/09/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 2d641287bcf095f13719667a91b7f61295309b5d
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/19/2018
ms.locfileid: "49430364"
---
> [!NOTE]
> - La versión preliminar de la autenticación de Azure AD para blobs y colas solo está destinada al uso que no sea de producción. En este momento no hay contratos de nivel de servicio de producción disponibles. Si la autenticación de Azure AD no se admite todavía para su escenario, siga usando la autorización con clave compartida o tokens de SAS en las aplicaciones.
>
> - Durante la versión preliminar, las asignaciones de roles de control de acceso basado en rol pueden tardar hasta cinco minutos en propagarse.
>
> - Debe usar HTTPS para autenticarse con Azure AD al llamar a operaciones de blobs y colas.
>
> - Azure Portal admite el uso de credenciales de Azure AD para leer y escribir datos de blobs como parte de la versión preliminar.
> 
> - Actualmente, Azure Portal no admite el uso de credenciales de Azure AD para leer y escribir datos de colas. Puede accederse a los datos de colas a través de las claves de la cuenta de almacenamiento.
>
> - [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/) actualmente usa la clave de la cuenta de almacenamiento para acceder a los datos de blobs y colas.
>
> - Azure Files solo admite autenticación con Azure AD a través de SMB para máquinas virtuales unidas a dominios (versión preliminar). Para aprender a usar Azure AD a través de SMB de Azure Files, consulte [Introducción a la autenticación de Azure Active Directory a través de SMB para Azure Files (versión preliminar)](../articles/storage/files/storage-files-active-directory-overview.md).



