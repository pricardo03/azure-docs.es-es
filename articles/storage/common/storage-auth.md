---
title: Autorización de acceso a Azure Storage | Microsoft Docs
description: Más información sobre las distintas maneras de autorizar el acceso al Azure Storage, incluido Azure Active Directory, la autenticación con clave compartida o las firmas de acceso compartido.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: c20f699a2d2270d11935b0216b1655390ece211c
ms.sourcegitcommit: 2d9a9079dd0a701b4bbe7289e8126a167cfcb450
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2019
ms.locfileid: "71671057"
---
# <a name="authorizing-access-to-azure-storage"></a>Autorización de acceso a Azure Storage

Cada vez que accede a datos de la cuenta de almacenamiento, el cliente realiza una solicitud a través de HTTP/HTTPS a Azure Storage. Todas las solicitudes a un recurso seguro deben estar autorizadas para que el servicio garantice que el cliente tiene los permisos necesarios para acceder a los datos.

En la tabla siguiente se describen las opciones que ofrece Azure Storage para autorizar el acceso a los recursos:

|  |Clave compartida (clave de cuenta de almacenamiento)  |Firma de acceso compartido (SAS)  |Azure Active Directory (Azure AD)  |Acceso de lectura anónimo  |
|---------|---------|---------|---------|---------|
|Azure Blobs     |[Compatible](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-shared-key/)         |[Compatible](storage-sas-overview.md)         |[Compatible](storage-auth-aad.md)         |[Compatible](../blobs/storage-manage-access-to-resources.md)         |
|Azure Files (SMB)     |[Compatible](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-shared-key/)         |No compatible         |[Admitido, solo con AAD Domain Services](../files/storage-files-active-directory-overview.md)         |No compatible         |
|Azure Files (REST)     |[Compatible](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-shared-key/)         |[Compatible](storage-sas-overview.md)         |No compatible         |No compatible         |
|Colas de Azure     |[Compatible](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-shared-key/)         |[Compatible](storage-sas-overview.md)         |[Compatible](storage-auth-aad.md)         |No compatible         |
|Azure Tables     |[Compatible](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-shared-key/)         |[Compatible](storage-sas-overview.md)         |No compatible         |No compatible         |

Cada opción de autorización se describe brevemente a continuación:

- **Integración de Azure Active Directory (Azure AD)** para blobs y colas. Azure AD proporciona control de acceso basado en rol (RBAC) para el control específico de acceso de los clientes a los recursos de una cuenta de almacenamiento. Para más información sobre la integración de Azure AD para blobs y colas, consulte [Autenticación del acceso a Azure Storage con Azure Active Directory](storage-auth-aad.md).

- **Integración de Azure AD Domain Services (DS) (versión preliminar)** para archivos. Azure Files admite la autenticación basada en identidades mediante bloque de mensajes del servidor(SMB) mediante Azure AD DS. Esto proporciona RBAC para el control específico de acceso de los clientes a los recursos de una cuenta de almacenamiento. Para obtener más información sobre la integración de Azure AD para archivos mediante servicios de dominio, consulte [Introducción del soporte de la autenticación de Azure Active Directory sobre SMB para Azure Files (versión preliminar)](../files/storage-files-active-directory-overview.md).

- **Autorización con clave compartida** para blobs, archivos, colas y tablas. Los clientes con clave compartida pasan un encabezado con cada solicitud que está firmado con la clave de acceso de la cuenta de almacenamiento. Para más información, consulte el artículo sobre la [Autorización con clave compartida](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-shared-key/).
- **Firmas de acceso compartido** para blobs, archivos, colas y tablas. Las firmas de acceso compartido (SAS) proporcionan acceso delegado limitado a recursos de una cuenta de almacenamiento. Agregar restricciones al periodo de validez de la firma o a los permisos que concede proporciona flexibilidad para administrar el acceso. Para obtener más información, consulte [Uso de firmas de acceso compartido (SAS)](storage-sas-overview.md).
- **Acceso de lectura público y anónimo** para contenedores y blobs. No se necesita autorización. Para más información, consulte [Administración del acceso de lectura anónimo a contenedores y blobs](../blobs/storage-manage-access-to-resources.md).  

De forma predeterminada, todos los recursos de Azure Storage están protegidos y solo están disponibles para el propietario de la cuenta. Aunque puede usar cualquiera de las estrategias de autorización descritas anteriormente para conceder a los clientes acceso a los recursos de su cuenta de almacenamiento, Microsoft recomienda el uso de Azure AD siempre que sea posible para mayor seguridad y facilidad de uso. 
