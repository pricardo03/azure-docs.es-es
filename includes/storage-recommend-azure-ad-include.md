---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/20/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 825a9f5668cc80f1306d74623db2ea54614ba4a3
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2019
ms.locfileid: "68985382"
---
> [!TIP]
> Azure Storage admite la autorización de solicitudes a almacenamiento de blobs y colas mediante Azure Active Directory (Azure AD). La autorización de usuarios o aplicaciones mediante un token de OAuth 2.0 devuelto por Azure AD proporciona una seguridad superior y facilidad de uso sobre la autorización de clave compartida. Con Azure AD, no hay ninguna necesidad de almacenar la clave de acceso de cuenta con su código y arriesgarse a posibles vulnerabilidades de seguridad.
>
> Además, Azure Storage admite la firma de acceso compartido (SAS) de delegación de usuario para Blob Storage. La SAS de delegación de usuario está firmada con credenciales de Azure AD. Cuando el diseño de la aplicación requiera firmas de acceso compartido para el acceso a Blob Storage, utilice credenciales de Azure AD para crear una SAS de delegación de usuario para una seguridad superior.
>
> Microsoft recomienda usar Azure AD con las aplicaciones de Azure Storage cuando sea posible. Para más información, consulte [Autenticación del acceso a blobs y colas de Azure con Azure Active Directory](../articles/storage/common/storage-auth-aad.md).
