---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 07/25/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: f3b9f6c27fb8d423350eac5d286c9859ad6fbd37
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/28/2019
ms.locfileid: "70104297"
---
## <a name="about-the-user-delegation-sas-preview"></a>Acerca de la SAS de delegación de usuarios (versión preliminar)

Un token de SAS para el acceso a un contenedor o blob se puede proteger mediante credenciales de Azure AD credenciales o una clave de cuenta. Una SAS protegida con credenciales de Azure AD se denomina SAS de delegación de usuarios, porque el token de OAuth 2.0 usado para firmar la SAS se solicita en nombre del usuario.

Microsoft recomienda usar credenciales de Azure AD cuando sea posible como procedimiento recomendado de seguridad, en lugar de usar la clave de cuenta, que se puede poner en peligro más fácilmente. Cuando el diseño de la aplicación requiera firmas de acceso compartido, use credenciales de Azure AD para crear una SAS de delegación de usuarios de cara a una seguridad superior. Para más información sobre la SAS de delegación de usuarios, consulte [Creación de una SAS de delegación de usuario](/rest/api/storageservices/create-user-delegation-sas).

> [!NOTE]
> Esta versión preliminar de la SAS de delegación de usuarios está pensada para usos distintos del de producción.

> [!CAUTION]
> Cualquier cliente que posea una SAS válida puede acceder a los datos de la cuenta de almacenamiento según lo permitido por esa SAS. Es importante proteger una SAS de uso malintencionado o no intencionado. Sea cauto al distribuir una SAS y tenga un plan para revocar una SAS en peligro.

Para más información sobre las firmas de acceso compartido, consulte [Grant limited access to Azure Storage resources using shared access signatures (SAS)](../articles/storage/common/storage-sas-overview.md) (Otorgar acceso limitado a recursos de Azure Storage con firmas de acceso compartido [SAS]).
