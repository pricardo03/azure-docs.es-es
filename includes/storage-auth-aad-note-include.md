---
title: archivo de inclusión
description: archivo de inclusión
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 09/06/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 3a3ee0cda3643ac73581f868e47905841d9f1563
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/07/2018
ms.locfileid: "44095598"
---
> [!IMPORTANT]
> - La versión preliminar de la autenticación de Azure AD para blobs y colas solo está destinada al uso que no sea de producción. En este momento no hay contratos de nivel de servicio de producción disponibles. Si la autenticación de Azure AD no se admite todavía para su escenario, siga usando la autorización con clave compartida o tokens de SAS en las aplicaciones.
>
> - Durante la versión preliminar, las asignaciones de roles de control de acceso basado en rol pueden tardar hasta cinco minutos en propagarse.
>
> - Debe usar HTTPS para autenticarse con Azure AD al llamar a operaciones de blobs y colas.
>
> - En la versión preliminar, Azure Portal no usa credenciales de Azure AD para leer y escribir datos de blobs y colas. En lugar de ello, el portal sigue confiando en la clave de acceso de cuenta. Para ver o actualizar los datos de blobs o colas en el portal, el usuario debe tener asignado un rol de RBAC que abarque [Microsoft.Storage/storageAccounts/listkeys/action](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-account-key-operator-service-role), que concede permisos para llamar a [Storage Accounts - List Keys](https://docs.microsoft.com/rest/api/storagerp/storageaccounts/listkeys) (Cuentas de almacenamiento: enumeración de claves). Los roles de colaborador y lector de blobs y colas no incluyen actualmente la acción **listkeys** como parte de la versión preliminar y por eso no proporcionan acceso a los datos a través de Azure Portal. Para explorar el acceso basado en identidades a datos de blobs y colas durante la versión preliminar, utilice PowerShell o la CLI de Azure.
