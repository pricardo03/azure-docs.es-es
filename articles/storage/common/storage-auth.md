---
title: Autorización de acceso a Azure Storage | Microsoft Docs
description: Más información sobre las distintas maneras de autorizar el acceso al Azure Storage, incluido Azure Active Directory, la autenticación con clave compartida o las firmas de acceso compartido.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 05/18/2018
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 8289f358bbfc2fe6229abf4eefc7bc8e57f05d73
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55464320"
---
# <a name="authorizing-access-to-azure-storage"></a>Autorización de acceso a Azure Storage

Cada vez que accede a datos de la cuenta de almacenamiento, el cliente realiza una solicitud a través de HTTP/HTTPS a Azure Storage. Todas las solicitudes a un recurso seguro deben estar autorizadas para que el servicio garantice que el cliente tiene los permisos necesarios para acceder a los datos. Azure Storage ofrece estas opciones para autorizar el acceso a los recursos seguros:

- **Integración de Active Directory (Azure AD) de Azure (versión preliminar)** para blobs y colas. Azure AD proporciona control de acceso basado en rol (RBAC) para el control específico de acceso de los clientes a los recursos de una cuenta de almacenamiento. Para más información, consulte [Autenticación del acceso a Azure Storage con Azure Active Directory (versión preliminar)](storage-auth-aad.md).
- **Autorización con clave compartida** para blobs, archivos, colas y tablas. Los clientes con clave compartida pasan un encabezado con cada solicitud que está firmado con la clave de acceso de la cuenta de almacenamiento. Para más información, consulte el artículo sobre la [Autorización con clave compartida](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-shared-key/).
- **Firmas de acceso compartido** para blobs, archivos, colas y tablas. Las firmas de acceso compartido (SAS) proporcionan acceso delegado limitado a recursos de una cuenta de almacenamiento. Agregar restricciones al periodo de validez de la firma o a los permisos que concede proporciona flexibilidad para administrar el acceso. Para obtener más información, consulte [Uso de firmas de acceso compartido (SAS)](storage-dotnet-shared-access-signature-part-1.md).
- **Acceso de lectura público y anónimo** para contenedores y blobs. No se necesita autorización. Para más información, consulte [Administración del acceso de lectura anónimo a contenedores y blobs](../blobs/storage-manage-access-to-resources.md).  

De forma predeterminada, todos los recursos de Azure Storage están protegidos y solo están disponibles para el propietario de la cuenta. Aunque puede usar cualquiera de las estrategias de autorización descritas anteriormente para conceder a los clientes acceso a los recursos de su cuenta de almacenamiento, Microsoft recomienda el uso de Azure AD siempre que sea posible para mayor seguridad y facilidad de uso. 



