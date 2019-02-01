---
title: 'Uso de RBAC para administrar los derechos de acceso a contenedores y colas (versión preliminar): Azure Storage | Microsoft Docs'
description: Uso del control de acceso basado en rol (RBA) para asignar roles para el acceso a los datos de blobs y colas para los usuarios, grupos, entidades de servicio de aplicación o las identidades de servicio administradas. Azure Storage es compatible con roles integrados y personalizados para los derechos de acceso a los contenedores y las colas.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 12/12/2018
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 6cecb2d5fde4fc651cf0ba971189ef62fa330c13
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55241719"
---
# <a name="manage-access-rights-to-azure-blob-and-queue-data-with-rbac-preview"></a>Administración de los derechos de acceso a los datos de la cola y el blob de Azure con RBAC (versión preliminar)

Azure Active Directory (Azure AD) autoriza derechos de acceso a los recursos protegidos mediante el [control de acceso basado en rol (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview). Azure Storage define un conjunto de roles de RBAC integrados que abarcan conjuntos comunes de permisos utilizados para acceder a los contenedores o las colas. Cuando un rol RBAC se asigna a una identidad de Azure AD, se concede a esa identidad acceso a esos recursos según el ámbito especificado. El acceso se puede limitar al nivel de la suscripción, el grupo de recursos, la cuenta de almacenamiento o un contenedor individual o una cola. Puede asignar derechos de acceso para los recursos de Azure Storage con Azure Portal, las herramientas de la línea de comandos de Azure o las API de administración de Azure. 

Una identidad de Azure AD puede ser un usuario, un grupo o una entidad de servicio de aplicación, o puede ser una identidad de servicio administrada para recursos de Azure. Una entidad de seguridad puede ser un usuario, grupo o entidad de servicio de aplicación. Una [identidad administrada para recursos de Azure](../../active-directory/managed-identities-azure-resources/overview.md) es una identidad automáticamente administrada utilizada para autenticar desde aplicaciones que se ejecutan en máquinas virtuales de Azure, aplicaciones de función, conjuntos de escalado de máquinas virtuales y otros. Para información general de identidad en Azure AD, consulte [Información acerca de las soluciones de identidades de Azure](https://docs.microsoft.com/azure/active-directory/understand-azure-identity-solutions).

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="rbac-roles-for-blobs-and-queues"></a>Roles RBAC para blobs y colas

Azure Storage es compatible con roles RBAC tanto integrados como personalizados. Azure Storage proporciona estos roles integrados de RBAC para su uso con Azure AD:

- [Propietario de datos de blobs de almacenamiento (versión preliminar)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-owner-preview): se usa para establecer la propiedad y administrar el control de acceso POSIX en Azure Data Lake Storage Gen2 (versión preliminar). Para más información, consulte [Control de acceso en Azure Data Lake Storage Gen2](../blobs/data-lake-storage-access-control.md).
- [Colaborador de datos de blobs de almacenamiento (versión preliminar)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-contributor-preview): se usa para conceder permisos de lectura, escritura y eliminación a los recursos de almacenamiento de blobs.
- [Lector de datos de blobs de almacenamiento (versión preliminar)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-blob-data-reader-preview): se usa para conceder permisos de solo lectura a los recursos de almacenamiento de blobs.
- [Colaborador de datos de la cola de almacenamiento (versión preliminar)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-contributor-preview): se usa para conceder permisos de lectura, escritura y eliminación a las colas de Azure.
- [Lector de datos de la cola de almacenamiento (versión preliminar)](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#storage-queue-data-reader-preview): se usa para conceder permisos de solo lectura a las colas de Azure.

Para más información acerca de cómo se definen los roles integrados para Azure Storage, consulte [Descripción de definiciones de roles](https://docs.microsoft.com/azure/role-based-access-control/role-definitions#management-and-data-operations-preview).

También puede definir roles personalizados para su uso con contenedores y colas. Para más información, consulte [Creación de roles personalizados para el control de acceso basado en roles de Azure](https://docs.microsoft.com/azure/role-based-access-control/custom-roles). 

[!INCLUDE [storage-auth-aad-note-include](../../../includes/storage-auth-aad-note-include.md)]

## <a name="assign-a-role-to-a-security-principal"></a>Asignación de un rol a una entidad de seguridad

Asigne un rol RBAC a una identidad de Azure para conceder permisos a los contenedores o las colas en la cuenta de almacenamiento. Puede establecer el ámbito de la asignación de roles para la cuenta de almacenamiento o a una cola o un contenedor específico. La tabla siguiente resume los derechos de acceso concedidos por las funciones integradas, en función del ámbito:

|Ámbito|Propietario de datos de blob|Colaborador de datos de blob|Lector de datos de blob|Colaborador de datos de cola|Lector de datos de cola|
|---|---|---|---|---|---|
|Nivel de suscripción|Acceso de lectura y escritura, y administración del control de acceso POSIX a todos los contenedores y blobs de la suscripción|Acceso de lectura/escritura a todos los contenedores y blobs en la suscripción| Acceso de lectura a todos los contenedores y blobs en la suscripción|Acceso de lectura/escritura a todas las colas en la suscripción|Acceso de lectura a todas las colas en la suscripción|
|Nivel de grupo de recursos|Acceso de lectura y escritura, y administración del control de acceso POSIX a todos los contenedores y blobs del grupo de recursos|Acceso de lectura/escritura a todos los contenedores y blobs en el grupo de recursos|Acceso de lectura a todos los contenedores y blobs en el grupo de recursos|Acceso de lectura/escritura para todas las colas en el grupo de recursos|Acceso de lectura a todas las colas en el grupo de recursos|
|Nivel de cuenta de almacenamiento|Acceso de lectura y escritura, y administración del control de acceso POSIX a todos los contenedores y blobs de la cuenta de almacenamiento|Acceso de lectura/escritura a todos los contenedores y blobs en la cuenta de almacenamiento|Acceso de lectura a todos los contenedores y blobs en la cuenta de almacenamiento|Acceso de lectura/escritura a todas las colas en la cuenta de almacenamiento|Acceso de lectura a todas las colas en la cuenta de almacenamiento|
|Nivel de contenedor/cola|Acceso de lectura y escritura, y administración del control de acceso POSIX a los contenedores especificados y sus blobs.|Acceso de lectura/escritura al contenedor especificado y sus blobs|Acceso de lectura al contenedor especificado y sus blobs|Acceso de lectura/escritura a la cola especificada|Acceso de lectura a la cola especificada|

> [!NOTE]
> Como propietario de la cuenta de Azure Storage, no se le asignan automáticamente permisos para tener acceso a datos. Tiene que asignarse a sí mismo de forma explícita un rol RBAC para Azure Storage. Puede asignarlo al nivel de su suscripción, grupo de recursos, cuenta de almacenamiento o un contenedor o cola.

Para más información sobre los permisos requeridos para llamar a operaciones de Azure Storage, consulte ///[Permissions for calling REST operations](https://docs.microsoft.com/rest/api/storageservices/authenticate-with-azure-active-directory#permissions-for-calling-rest-operations) (Permisos para llamar a operaciones de REST).

Las secciones siguientes muestran cómo asignar un rol con ámbito limitado a la cuenta de almacenamiento o limitado a un contenedor individual.

### <a name="assign-a-role-scoped-to-the-storage-account-in-the-azure-portal"></a>Asignación de un rol limitado a la cuenta de almacenamiento en Azure Portal

Para asignar un rol integrado que conceda acceso a todos los contenedores o colas en la cuenta de almacenamiento en Azure Portal:

1. En [Azure Portal](https://portal.azure.com), vaya a la cuenta de almacenamiento.
1. Seleccione la cuenta de almacenamiento y, después, seleccione **Control de acceso (IAM)** para mostrar la configuración de control de acceso de la cuenta. Seleccione la pestaña **Asignaciones de roles** para ver la lista de asignaciones de roles.

    ![Captura de pantalla que muestra la configuración de control de acceso del almacenamiento](media/storage-auth-aad-rbac/portal-access-control.png)

1. Haga clic en el botón **Agregar asignación de roles** para agregar un rol nuevo.
1. En la ventana **Agregar asignación de roles**, seleccione el rol que se va a asignar a una identidad de Azure AD. Después realice una búsqueda para localizar la identidad a la que desea asignar ese rol. Por ejemplo, la siguiente imagen muestra el rol **Lector de datos de blobs de almacenamiento (versión preliminar)** asignado a un usuario.

    ![Captura de pantalla que muestra cómo asignar un rol de RBAC](media/storage-auth-aad-rbac/add-rbac-role.png)

1. Haga clic en **Save**(Guardar). La identidad a la que ha asignado el rol aparece enumerada debajo de ese rol. Por ejemplo, la siguiente imagen muestra que el usuario agregado tiene ahora permisos de lectura para los datos de todos los blobs de la cuenta de almacenamiento.

    ![Captura de pantalla con una lista de los usuarios asignados a un rol](media/storage-auth-aad-rbac/account-scoped-role.png)

### <a name="assign-a-role-scoped-to-a-container-or-queue-in-the-azure-portal"></a>Asignación de un rol limitado a un contenedor o una cola en Azure Portal

> [!IMPORTANT]
> Esto no se puede hacer si se usa una cuenta con el espacio de nombres jerárquico habilitado.

Los pasos para asignar un rol integrado limitado a un contenedor o a una cola son similares. El procedimiento mostrado aquí asigna un rol limitado a un contenedor, pero puede seguir los mismos pasos para asignar un rol limitado a una cola: 

1. En [Azure Portal](https://portal.azure.com), vaya a la cuenta de almacenamiento y muestre la **Introducción** para la cuenta.
1. En Servicios, seleccione **Blobs**. 
1. Busque el contenedor para el que desea asignar un rol y visualice la configuración del contenedor. 
1. Seleccione **Control de acceso (IAM)** para mostrar la configuración del control de acceso del contenedor. Seleccione la pestaña **Asignaciones de roles** para ver la lista de asignaciones de roles.

    ![Captura de pantalla que muestra la configuración del control de acceso del contenedor](media/storage-auth-aad-rbac/portal-access-control-container.png)
1. Haga clic en el botón **Agregar asignación de roles** para agregar un rol nuevo.
1. En la ventana **Agregar asignación de roles**, seleccione el rol que desea asignar a una identidad de Azure AD. Después realice una búsqueda para localizar la identidad a la que desea asignar ese rol.
1. Haga clic en **Save**(Guardar). La identidad a la que ha asignado el rol aparece enumerada debajo de ese rol. Por ejemplo, la siguiente imagen muestra que el usuario agregado tiene ahora permisos de lectura para los datos del contenedor llamado *sample-container*.

    ![Captura de pantalla con una lista de los usuarios asignados a un rol](media/storage-auth-aad-rbac/container-scoped-role.png)

## <a name="next-steps"></a>Pasos siguientes

- Para más información acerca de RBAC, consulte el artículo [¿Qué es el control de acceso basado en rol (RBAC)?](../../role-based-access-control/overview.md).
- Para información sobre cómo asignar y administrar las asignaciones de roles RBAC con Azure PowerShell, la CLI de Azure, o la API de REST, consulte estos artículos:
    - [Administración del control de acceso basado en rol (RBAC) con Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md)
    - [Administración del control de acceso basado en rol (RBAC) con la CLI de Azure](../../role-based-access-control/role-assignments-cli.md)
    - [Administración del control de acceso basado en rol (RBAC) con la API de REST](../../role-based-access-control/role-assignments-rest.md)
- Para información sobre la autorización de acceso a los contenedores y las colas desde las aplicaciones de almacenamiento, consulte el artículo sobre el [uso de Azure AD con aplicaciones de Azure Storage](storage-auth-aad-app.md).
- Para información adicional acerca de la integración de Azure AD para los contenedores y colas de Azure, consulte la entrada de blog del equipo de almacenamiento de Azure acerca del [anuncio de la versión preliminar Autenticación de Azure AD para Azure Storage](https://azure.microsoft.com/blog/announcing-the-preview-of-aad-authentication-for-storage/).
