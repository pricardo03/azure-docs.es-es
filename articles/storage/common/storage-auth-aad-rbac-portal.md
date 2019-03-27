---
title: Usar el portal de Azure para administrar los derechos de acceso de Azure AD a los datos de blob y cola con RBAC - almacenamiento de Azure | Microsoft Docs
description: Utilice el control de acceso basado en roles (RBAC) de Azure portal para asignar el acceso a los contenedores y las colas a las entidades de seguridad. Almacenamiento de Azure admite los roles RBAC integrados y personalizados para la autenticación a través de Azure AD.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 03/21/2019
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 8214ff821bad8a46eb710c8b9506d337715db103
ms.sourcegitcommit: f0f21b9b6f2b820bd3736f4ec5c04b65bdbf4236
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/26/2019
ms.locfileid: "58450047"
---
# <a name="grant-access-to-azure-blob-and-queue-data-with-rbac-in-the-azure-portal"></a>Conceder acceso a datos blob y cola de Azure con RBAC en Azure portal

Azure Active Directory (Azure AD) autoriza derechos de acceso a los recursos protegidos mediante el [control de acceso basado en rol (RBAC)](../../role-based-access-control/overview.md). Almacenamiento de Azure define un conjunto de roles RBAC integrados que abarcan conjuntos de permisos utilizados para tener acceso a datos blob o cola comunes. 

Cuando un rol de RBAC se asigna a una entidad de seguridad de Azure AD, Azure concede acceso a esos recursos para esa entidad de seguridad. El acceso se puede limitar al nivel de la suscripción, el grupo de recursos, la cuenta de almacenamiento o un contenedor individual o una cola. Una entidad de seguridad de Azure AD puede ser un usuario, un grupo, una entidad de servicio de aplicación, o un [la identidad de los recursos de Azure administrada](../../active-directory/managed-identities-azure-resources/overview.md).

En este artículo se describe cómo usar el portal de Azure para asignar roles RBAC. El portal de Azure proporciona una interfaz sencilla para la asignación de roles de RBAC y administrar el acceso a los recursos de almacenamiento. También puede asignar roles RBAC para los recursos de blob y cola con las herramientas de línea de comandos de Azure o las API de administración de almacenamiento de Azure. Para obtener más información acerca de los roles RBAC para los recursos de almacenamiento, consulte [autenticar el acceso a Azure blobs y colas con Azure Active Directory](storage-auth-aad.md). 

## <a name="rbac-roles-for-blobs-and-queues"></a>Roles RBAC para blobs y colas

[!INCLUDE [storage-auth-rbac-roles-include](../../../includes/storage-auth-rbac-roles-include.md)]

## <a name="determine-resource-scope"></a>Determinar el ámbito del recurso 

[!INCLUDE [storage-auth-resource-scope-include](../../../includes/storage-auth-resource-scope-include.md)]

## <a name="assign-rbac-roles-using-the-azure-portal"></a>Asignación de roles RBAC con Azure portal

Después de determinar el ámbito adecuado para una asignación de roles, vaya a ese recurso en el portal de Azure. Mostrar el **Access Control (IAM)** configuración para el recurso y siga estas instrucciones para administrar las asignaciones de roles:

1. Asignar el rol de RBAC de Azure Storage adecuado para conceder acceso a una entidad de seguridad de Azure AD.

1. Asignar el Administrador de recursos de Azure [lector](../../role-based-access-control/built-in-roles.md#reader) roles a los usuarios que necesitan tener acceso a contenedores o las colas a través del portal de Azure con sus credenciales de Azure AD. 

Las secciones siguientes describen cada uno de estos pasos con más detalle.

### <a name="assign-a-built-in-rbac-role"></a>Asignar un rol RBAC integrado

Antes de asignar un rol a una entidad de seguridad, asegúrese de tener en cuenta el ámbito de los permisos que se va a conceder. Revise el [determinar el ámbito del recurso](#determine-resource-scope) sección para decidir el ámbito adecuado.

El procedimiento mostrado aquí asigna un rol limitado a un contenedor, pero puede seguir los mismos pasos para asignar un rol limitado a una cola: 

1. En [Azure Portal](https://portal.azure.com), vaya a la cuenta de almacenamiento y muestre la **Introducción** para la cuenta.
1. En Servicios, seleccione **Blobs**. 
1. Busque el contenedor para el que desea asignar un rol y visualice la configuración del contenedor. 
1. Seleccione **Control de acceso (IAM)** para mostrar la configuración del control de acceso del contenedor. Seleccione la pestaña **Asignaciones de roles** para ver la lista de asignaciones de roles.

    ![Captura de pantalla muestra la configuración de control de acceso de contenedor](media/storage-auth-aad-rbac-portal/portal-access-control-container.png)

1. Haga clic en el botón **Agregar asignación de roles** para agregar un rol nuevo.
1. En el **Agregar asignación de roles** ventana, seleccione el rol de Azure Storage que desea asignar. A continuación, busque la entidad de seguridad a la que desea asignar ese rol.

    ![Captura de pantalla que muestra cómo asignar un rol de RBAC](media/storage-auth-aad-rbac-portal/add-rbac-role.png)

1. Haga clic en **Save**(Guardar). La identidad a la que ha asignado el rol aparece enumerada debajo de ese rol. Por ejemplo, la siguiente imagen muestra que el usuario agregado tiene ahora permisos de lectura para los datos del contenedor llamado *sample-container*.

    ![Captura de pantalla que muestra la lista de los usuarios asignados a un rol](media/storage-auth-aad-rbac-portal/container-scoped-role.png)

Puede seguir pasos similares para asignar un rol con ámbito en la cuenta de almacenamiento, grupo de recursos o suscripción.

> [!NOTE]
> Como propietario de la cuenta de Azure Storage, no se le asignan automáticamente permisos para tener acceso a datos. Tiene que asignarse a sí mismo de forma explícita un rol RBAC para Azure Storage. Puede asignarlo al nivel de su suscripción, grupo de recursos, cuenta de almacenamiento o un contenedor o cola.
> 
> No se puede asignar un rol con ámbito a un contenedor o una cola si la cuenta de almacenamiento tiene habilitado un espacio de nombres jerárquico.

### <a name="assign-the-reader-role-for-portal-access"></a>Asigne el rol de lector para acceder al portal

Al asignar un rol integrado o personalizado para Azure Storage a una entidad de seguridad, está concediendo permisos a esa entidad de seguridad para realizar operaciones en los datos en la cuenta de almacenamiento. Integrado **lector de datos** roles proporcionan permisos de lectura para los datos en un contenedor o una cola, mientras la integrada **colaborador de Data** roles proporcionan leer, escribir y eliminar permisos a un contenedor o cola. Los permisos se limitan al recurso especificado.  

Por ejemplo, si asigna el **colaborador de datos de Blob de almacenamiento** rol de usuario María en el nivel de un contenedor denominado **contenedor-ejemplo**, a continuación, se le concede lectura Mary, escribir y eliminar el acceso a todos los blobs en ese contenedor.

Sin embargo, si Mary desea ver un blob en Azure portal, la **colaborador de datos de almacenamiento Blob** rol por sí solo no proporcionará suficientes permisos para explorar a través del portal en el blob para poder verla. Adicionales se necesitan permisos de Azure AD para navegar a través del portal y ver los otros recursos que están visibles no existe.

Si los usuarios necesitan poder acceder a blobs en el portal de Azure, a continuación, asignarles un rol RBAC adicional, el [lector](../../role-based-access-control/built-in-roles.md#reader) roles a los usuarios, en el nivel de la cuenta de almacenamiento o versiones posteriores. El **lector** rol es un rol de Azure Resource Manager que permite a los usuarios ver recursos de la cuenta de almacenamiento, pero no modificarlas. No proporciona permisos de lectura a los datos en Azure Storage, sino solo a los recursos de administración de cuenta.

Siga estos pasos para asignar el **lector** rol para que un usuario puede acceder a blobs de Azure portal. En este ejemplo, la asignación se limita a la cuenta de almacenamiento:

1. En [Azure Portal](https://portal.azure.com), vaya a la cuenta de almacenamiento.
1. Seleccione **control de acceso (IAM)** para mostrar la configuración de control de acceso para la cuenta de almacenamiento. Seleccione la pestaña **Asignaciones de roles** para ver la lista de asignaciones de roles.
1. En el **Agregar asignación de roles** ventana, seleccione el **lector** rol. 
1. Desde el **asignar acceso a** campos, seleccione **usuario, grupo o entidad de servicio de Azure AD**.
1. Buscar para buscar a la entidad de seguridad a la que desea asignar el rol.
1. Guardar la asignación de roles.

> [!NOTE]
> Asignar el rol de lector sólo es necesario para los usuarios que necesitan tener acceso a blobs o colas mediante el portal de Azure. 

## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información acerca de los roles RBAC para los recursos de almacenamiento, consulte [autenticar el acceso a Azure blobs y colas con Azure Active Directory](storage-auth-aad.md). 
- Para más información acerca de RBAC, consulte el artículo [¿Qué es el control de acceso basado en rol (RBAC)?](../../role-based-access-control/overview.md).
- Para información sobre cómo asignar y administrar las asignaciones de roles RBAC con Azure PowerShell, la CLI de Azure, o la API de REST, consulte estos artículos:
    - [Administración del control de acceso basado en rol (RBAC) con Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md)
    - [Administración del control de acceso basado en rol (RBAC) con la CLI de Azure](../../role-based-access-control/role-assignments-cli.md)
    - [Administración del control de acceso basado en rol (RBAC) con la API de REST](../../role-based-access-control/role-assignments-rest.md)
- Para información sobre la autorización de acceso a los contenedores y las colas desde las aplicaciones de almacenamiento, consulte el artículo sobre el [uso de Azure AD con aplicaciones de Azure Storage](storage-auth-aad-app.md).
