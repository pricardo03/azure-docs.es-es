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
ms.openlocfilehash: 0182df40a4e7815560a85e60fe9062ccd8001c18
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52978848"
---
> [!NOTE]
> - La versión preliminar de la autenticación de Azure AD para blobs y colas solo está destinada al uso que no sea de producción. En este momento no hay contratos de nivel de servicio de producción disponibles. Si la autenticación de Azure AD no se admite todavía para su escenario, siga usando la autorización con clave compartida o tokens de SAS en las aplicaciones.
>
> - Durante la versión preliminar, las asignaciones de roles de control de acceso basado en rol pueden tardar hasta cinco minutos en propagarse.
>
> - Para autorizar las operaciones de blob y cola con un token de OAuth, debe usar HTTPS.
>
> - Azure Portal ahora permite utilizar credenciales de Azure AD para leer y escribir datos de blobs y colas en la versión preliminar.
> 
> - [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/) actualmente usa la clave de la cuenta de almacenamiento para acceder a los datos de blobs y colas. Se admite el acceso OAuth para blobs.
>
> - Azure Files solo admite autenticación con Azure AD a través de SMB para máquinas virtuales unidas a dominios (versión preliminar). Para aprender a usar Azure AD a través de SMB de Azure Files, consulte [Introducción a la autenticación de Azure Active Directory a través de SMB para Azure Files (versión preliminar)](../articles/storage/files/storage-files-active-directory-overview.md).