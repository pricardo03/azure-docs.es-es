---
title: Control de acceso basado en rol de Azure Digital Twins | Microsoft Docs
description: Obtenga información sobre autenticación en Digital Twins con el control de acceso basado en roles.
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: lyrana
ms.openlocfilehash: b9ccdb9030a24520be8f24f757c279241f3a07e1
ms.sourcegitcommit: 00dd50f9528ff6a049a3c5f4abb2f691bf0b355a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/05/2018
ms.locfileid: "51014795"
---
# <a name="role-based-access-control"></a>Control de acceso basado en rol

Azure Digital Twins permite el control de acceso preciso a datos, recursos y acciones específicos en un grafo espacial. Para ello, se usa la administración pormenorizada de roles y permisos que se conoce como control de acceso basado en rol (RBAC). RBAC consta de _roles_ y _asignaciones de roles_. Los roles identifican el nivel de permiso. Las asignaciones de roles asocian un rol a un usuario o dispositivo.

Con RBAC, se puede conceder permiso a:

- Un usuario
- Un dispositivo
- Una entidad de servicio
- Una función definida por el usuario 
- Todos los usuarios que pertenezcan a un dominio 
- Un inquilino
 
El grado de acceso también se puede ajustar.

El control de acceso basado en rol es único en el sentido de que los permisos se heredan hacia abajo del grafo espacial.

## <a name="what-can-i-do-with-rbac"></a>¿Qué puedo hacer con RBAC?

Un programador puede usar RBAC para:

* Conceder a un usuario la capacidad de administrar los dispositivos de todo un edificio, o de una sala o planta determinadas.
* Conceder a un administrador acceso global a todos los nodos del grafo espacial para un grafo completo o solo para una sección del grafo.
* Conceder a especialista en soporte técnico acceso de lectura al grafo, excepto a las claves de acceso.
* Conceder a todos los miembros de un dominio acceso de lectura a todos los objetos del grafo.

## <a name="rbac-best-practices"></a>Procedimientos recomendados de RBAC

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="roles"></a>Roles

### <a name="role-definitions"></a>Definiciones de roles

Una definición de roles es una recopilación de permisos y, a veces, se denomina simplemente rol. La definición de roles enumera las operaciones permitidas, como crear, leer, actualizar y eliminar. También especifica a qué tipos de objeto se aplican estos permisos.

Los siguientes roles están disponibles en Azure Digital Twins:

* **Administrador del espacio**: puede crear, leer, actualizar y eliminar permisos para el espacio especificado y todos los nodos inferiores. Permiso global.
* **Administrador de usuarios**: puede crear, leer, actualizar y eliminar permisos para usuarios y objetos relacionados con usuarios. Permiso de lectura para los espacios.
* **Administrador de dispositivos**: permiso para crear, leer, actualizar y eliminar dispositivos y objetos relacionados con dispositivos. Permiso de lectura para los espacios.
* **Administrador de claves**: permiso para crear, leer, actualizar y eliminar permisos de claves de acceso. Permiso de lectura para los espacios.
* **Administrador de tokens**: permiso para leer y actualizar claves de acceso. Permiso de lectura para los espacios.
* **Usuario**: permiso para leer espacios, sensores y usuarios, incluidos sus correspondientes objetos relacionados.
* **Especialista de soporte técnico**: permiso de lectura para todo, excepto las claves de acceso.
* **Instalador de dispositivos**: permiso para leer y actualizar dispositivos y sensores, incluidos sus correspondientes objetos relacionados. Permiso de lectura para los espacios.
* **Dispositivo de puerta de enlace**: permiso de creación para sensores. Permiso para leer dispositivos y sensores, incluidos sus correspondientes objetos relacionados.

>[!NOTE]
> Para recuperar las definiciones completas de los roles anteriores, consulte la API de sistema o roles.

### <a name="object-types"></a>Tipos de objeto

`ObjectIdType` hace referencia al tipo de identidad a la que se otorga un rol. Además de los tipos `DeviceId` y `UserDefinedFunctionId`, los tipos se corresponden con una propiedad de un objeto de Azure Active Directory (Azure AD):
  
* El tipo `UserId` asigna un rol a un usuario.
* El tipo `DeviceId` asigna un rol a un dispositivo.
* El tipo `DomainName` asigna un rol a un nombre de dominio. Cada usuario con el nombre de dominio especificado tiene los derechos de acceso del rol correspondiente.
* El tipo `TenantId` asigna un rol a un inquilino. Cada usuario al que pertenezca el identificador de inquilino de Azure AD especificado tiene los derechos de acceso del rol correspondiente.
* El tipo `ServicePrincipalId` asigna un rol a un identificador de objeto de entidad de servicio.
* El tipo `UserDefinedFunctionId` asigna un rol a una función definida por el usuario (UDF).

> [!div class="nextstepaction"]
> [Consulta o identificador de objeto para un usuario](https://docs.microsoft.com/powershell/module/azuread/get-azureaduser?view=azureadps-2.0)

> [!div class="nextstepaction"]
> [Obtener el identificador de objeto para una entidad de servicio](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermadserviceprincipal?view=azurermps-6.8.1)

> [!div class="nextstepaction"]
> [Recuperar el identificador de objeto de un inquilino de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant)

## <a name="role-assignments"></a>Asignaciones de roles

Para conceder permisos a un destinatario, cree una asignación de roles. Para revocar permisos, quite la asignación de roles. Una asignación de roles de Azure Digital Twins asocia un objeto, como un usuario o un inquilino de Azure AD, a un rol y un espacio. Los permisos se conceden a todos los objetos que pertenecen a dicho espacio. El espacio incluye todo el grafo espacial que hay debajo.

Por ejemplo, un usuario recibe una asignación de roles con el rol `DeviceInstaller` para el nodo raíz de un grafo espacial, que representa un edificio. Así que el usuario puede leer y actualizar los dispositivos de ese nodo y todos los demás espacios secundarios del edificio.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de la seguridad en Azure Digital Twins, consulte [Create and manage role assignments](./security-create-manage-role-assignments.md) (Creación y administración de las asignaciones de roles).
