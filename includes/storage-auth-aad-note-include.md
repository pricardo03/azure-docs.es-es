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
ms.openlocfilehash: 50217829be7aff3bcdbf417f19fff9f1fd7c6901
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/22/2019
ms.locfileid: "58372681"
---
> [!NOTE]
> - Las asignaciones de roles RBAC pueden tardar hasta dos minutos en propagarse.
>
> Para autorizar las operaciones de blob y cola con un token de OAuth, debe usar HTTPS.
>
> - Azure Portal ahora permite utilizar credenciales de Azure AD para leer y escribir datos de blobs y colas en la versión preliminar. Para obtener acceso a datos blob y cola en el portal de Azure, un usuario debe tener asignado el Administrador de recursos de Azure **lector** rol, además de la función adecuada para el acceso de blob o cola. Para obtener más información, consulte [conceder acceso a los contenedores de Azure y colas con RBAC en Azure portal](../articles/storage/common/storage-auth-aad-rbac.md). 
> 
> - [Explorador de Azure Storage](https://azure.microsoft.com/features/storage-explorer/) actualmente usa la clave de la cuenta de almacenamiento para acceder a los datos de blobs y colas. Si la clave no está disponible, autorización de Azure AD se usa para tener acceso a los blobs. Autorización de Azure AD no se admite actualmente para las colas. 
>
> - Azure Files solo admite autenticación con Azure AD a través de SMB para máquinas virtuales unidas a dominios (versión preliminar). Para aprender a usar Azure AD a través de SMB de Azure Files, consulte [Introducción a la autenticación de Azure Active Directory a través de SMB para Azure Files (versión preliminar)](../articles/storage/files/storage-files-active-directory-overview.md).