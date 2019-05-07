---
title: Usar el portal de Azure para tener acceso a blob o cola datos - almacenamiento de Azure
description: Al tener acceso a blobs o datos de la cola mediante Azure portal, el portal hace las solicitudes de Azure Storage en segundo plano. Estas solicitudes a Azure Storage pueden ser autenticadas y autorizan utilizando su cuenta de Azure AD o la clave de acceso de la cuenta de almacenamiento.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/19/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 00f34fa9a1932aebd467163e0ed7441c993387df
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65154007"
---
# <a name="use-the-azure-portal-to-access-blob-or-queue-data"></a>Usar el portal de Azure para acceder a los datos blob o cola

Al acceder a blob o cola de datos mediante el [portal Azure](https://portal.azure.com), el portal realiza solicitudes a Azure Storage en segundo plano. Estas solicitudes a Azure Storage pueden ser autenticadas y autorizan utilizando su cuenta de Azure AD o la clave de acceso de la cuenta de almacenamiento. El portal indica qué método de autenticación que utiliza y le permite cambiar entre las dos si tiene los permisos adecuados.  

## <a name="permissions-needed-to-access-blob-or-queue-data"></a>Permisos necesarios para tener acceso a datos blob o cola

Dependiendo de cómo desea autenticar el acceso a los datos de blob o cola en el portal de Azure, necesitará permisos específicos. En la mayoría de los casos, estos permisos se proporcionan a través del control de acceso basado en roles (RBAC). Para obtener más información sobre RBAC, consulte [¿qué es el control de acceso basado en roles (RBAC)?](../../role-based-access-control/overview.md).

### <a name="account-access-key"></a>Clave de acceso de cuenta

Para obtener acceso a datos blob y cola con la clave de acceso de cuenta, debe tener asignado un rol de RBAC que incluye la acción de RBAC **Microsoft.Storage/storageAccounts/listkeys/action**. Este rol de RBAC puede ser un integrado o un rol personalizado. Funciones integradas que admiten **Microsoft.Storage/storageAccounts/listkeys/action** incluyen:

- Azure Resource Manager [propietario](../../role-based-access-control/built-in-roles.md#owner) rol
- Azure Resource Manager [colaborador](../../role-based-access-control/built-in-roles.md#contributor) rol
- El [colaborador de la cuenta de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-account-contributor) rol

Al intentar tener acceso a datos blob o cola en el portal de Azure, el portal comprueba primero si se le ha asignado un rol con **Microsoft.Storage/storageAccounts/listkeys/action**. Si se ha asignado un rol con esta acción, el portal usa la clave de cuenta para tener acceso a datos blob y cola. Si no se ha asignado un rol con esta acción, el portal intenta obtener acceso a datos mediante la cuenta de Azure AD.

> [!NOTE]
> Los roles de administrador de suscripción clásica, Administrador de servicios y Coadministrador son el equivalente de Azure Resource Manager [propietario](../../role-based-access-control/built-in-roles.md#owner) rol. El **propietario** rol incluye todas las acciones, incluida la **Microsoft.Storage/storageAccounts/listkeys/action**, por lo que un usuario con uno de estos roles administrativos también puede tener acceso a datos blob y cola con el clave de cuenta. Para obtener más información, consulte [roles de administrador de suscripciones clásico](../../role-based-access-control/rbac-and-directory-admin-roles.md#classic-subscription-administrator-roles).

### <a name="azure-ad-account"></a>Cuenta de Azure AD

Para tener acceso a datos blob o cola desde el portal de Azure con su cuenta de Azure AD, dos de las instrucciones siguientes deben ser verdaderas para usted:

- Se le ha asignado el Administrador de recursos de Azure [lector](../../role-based-access-control/built-in-roles.md#reader) rol, como mínimo, con ámbito en el nivel de la cuenta de almacenamiento o una versión posterior. El **lector** rol concede los permisos más restringidos, pero otro rol de Azure Resource Manager que concede acceso a los recursos de administración de cuenta de almacenamiento también es aceptable.
- Se le ha asignado ya sea un rol integrado o personalizado que proporciona acceso a datos blob o cola.

El **lector** otra asignación de roles de Azure Resource Manager o de asignación de roles es necesario para que el usuario puede ver y navegar por los recursos de administración de la cuenta de almacenamiento en el portal de Azure. Los roles RBAC que conceden acceso a datos blob o cola no conceder acceso a los recursos de administración de cuenta de almacenamiento. Para obtener acceso a datos blob o cola en el portal, el usuario necesita permisos para explorar los recursos de la cuenta de almacenamiento. Para obtener más información sobre este requisito, consulte [asignar el rol de lector para acceder al portal](../common/storage-auth-aad-rbac-portal.md#assign-the-reader-role-for-portal-access).

Las funciones integradas que admiten el acceso a los datos blob o cola incluyen:

- [Propietario del almacenamiento de datos Blob](../../role-based-access-control/built-in-roles.md#storage-blob-data-owner): Control de acceso POSIX para Azure Data Lake Storage Gen2.
- [Colaborador de datos de Blob de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-blob-data-contributor): Permisos de lectura, escritura y eliminación para blobs.
- [Lector de datos de Blob de almacenamiento](../../role-based-access-control/built-in-roles.md#storage-blob-data-reader): Permisos de solo lectura para blobs.
- [Colaborador de datos de almacenamiento cola](../../role-based-access-control/built-in-roles.md#storage-queue-data-contributor): Permisos de lectura, escritura y eliminación de colas.
- [Lector de datos de almacenamiento cola](../../role-based-access-control/built-in-roles.md#storage-queue-data-reader): Permisos de solo lectura para las colas.
    
Roles personalizados pueden admitir diferentes combinaciones de los mismos permisos proporcionados por los roles integrados. Para obtener más información acerca de cómo crear roles personalizados de RBAC, consulte [roles personalizados para recursos de Azure](../../role-based-access-control/custom-roles.md) y [comprender las definiciones de roles para los recursos de Azure](../../role-based-access-control/role-definitions.md).

> [!NOTE]
> Enumerar colas con un rol de administrador de suscripciones clásico no se admite. Para enumerar pone en cola, un usuario debe haber asignado a ellos, el Administrador de recursos de Azure **lector** rol, el **lector de datos de cola de almacenamiento** rol, o la **colaborador de datos de almacenamiento cola** rol.

## <a name="navigate-to-blobs-or-queues-in-the-portal"></a>Vaya a blobs o colas en el portal

Para ver los datos de blob o cola en el portal, vaya a la **Introducción** para la cuenta de almacenamiento y haga clic en los vínculos para **Blobs** o **colas**. O bien puede navegar hasta el **servicio Blob** y **servicio cola** secciones en el menú. 

![Vaya a los datos de blob o cola en el portal de Azure](media/storage-access-blobs-queues-portal/blob-queue-access.png)

## <a name="determine-the-current-authentication-method"></a>Determinar el método de autenticación actual

Al navegar a un contenedor o una cola, el portal de Azure indica si actualmente está utilizando la clave de acceso de cuenta o la cuenta de Azure AD para autenticar.

Los ejemplos de esta sección muestran el acceso a un contenedor y sus blobs, pero el portal muestra el mismo mensaje al tener acceso a una cola y sus mensajes, o enumerar las colas.

### <a name="account-access-key"></a>Clave de acceso de cuenta

Si se autentica utilizando la clave de acceso de cuenta, verá **clave de acceso** especificado como método de autenticación en el portal:

![Actualmente tienen acceso a datos del contenedor con la clave de cuenta](media/storage-access-blobs-queues-portal/auth-method-access-key.png)

Para cambiar al uso de la cuenta de Azure AD, haga clic en el vínculo resaltado en la imagen. Si tiene los permisos adecuados a través de los roles RBAC que están asignados, podrá continuar. Sin embargo, si no tiene los permisos adecuados, verá un mensaje de error como el siguiente:

![Error que se muestra si la cuenta de Azure AD no admite el acceso](media/storage-access-blobs-queues-portal/auth-error-azure-ad.png)

Tenga en cuenta que ningún BLOB aparece en la lista si su cuenta de Azure AD no tiene permisos para verlos. Haga clic en el **cambiar a la clave de acceso** vínculo que se usa la clave de acceso para la autenticación de nuevo.

### <a name="azure-ad-account"></a>Cuenta de Azure AD

Si se autentican con su cuenta de Azure AD, verá **cuenta de usuario de Azure AD** especificado como método de autenticación en el portal:

![Actualmente tienen acceso a datos del contenedor con la cuenta de Azure AD](media/storage-access-blobs-queues-portal/auth-method-azure-ad.png)

Para cambiar al uso de la clave de acceso de cuenta, haga clic en el vínculo resaltado en la imagen. Si tiene acceso a la clave de cuenta, a continuación, podrá continuar. Sin embargo, si no tiene acceso a la clave de cuenta, verá un mensaje de error como el siguiente:

![Error que se muestra si no tiene acceso a la clave de cuenta](media/storage-access-blobs-queues-portal/auth-error-access-key.png)

Tenga en cuenta que ningún BLOB aparece en la lista si no tiene acceso a las claves de cuenta. Haga clic en el **cambiar a la cuenta de usuario de Azure AD** vínculo que se usa la cuenta de Azure AD autentique de nuevo.

## <a name="next-steps"></a>Pasos siguientes

- [Autenticar el acceso a los blobs de Azure y colas con Azure Active Directory](storage-auth-aad.md)
- [Conceder acceso a los contenedores de Azure y colas con RBAC en Azure portal](storage-auth-aad-rbac-portal.md)
- [Conceder acceso a datos de blob y cola de Azure con RBAC, mediante la CLI de Azure](storage-auth-aad-rbac-cli.md)
- [Conceder acceso a datos de blob y cola de Azure con RBAC con PowerShell](storage-auth-aad-rbac-powershell.md)
