---
title: Descripción del control de acceso basado en roles de Azure Digital Twins | Microsoft Docs
description: Obtenga información sobre autenticación en Digital Twins con el control de acceso basado en roles.
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: lyrana
ms.openlocfilehash: 7a6d8565a0f85b4cb81d9f5f23b04fe6b2edc53e
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323958"
---
# <a name="role-based-access-control"></a>Control de acceso basado en rol

Azure Digital Twins permite el control de acceso preciso a datos, recursos y acciones específicos en un grafo espacial. Para ello, usa una administración granular de roles y permisos denominada _control de acceso basado en roles_. El control de acceso basado en roles consta de _Roles_, o el nivel de permisos, y _Asignaciones de roles_, o la asociación de un rol a un usuario o dispositivo.

Mediante el control de acceso basado en roles, pueden concederse permisos a un usuario, a un dispositivo, a una entidad de servicio, a una función definida por el usuario, a todos los usuarios que pertenecen a un dominio o a un inquilino. Además, también puede ajustarse el grado de acceso.

El control de acceso basado en roles es único en el sentido de que los permisos se heredan hacia abajo el grafo espacial.

## <a name="what-can-i-do-with-role-based-access-control"></a>¿Qué puedo hacer con el control de acceso basado en roles?

Un desarrollador puede usar el control de acceso basado en roles para:

* Conceder a un usuario la capacidad de administrar los dispositivos para todo un edificio, o solo para una sala o planta determinadas.
* Conceder a un administrador acceso global a todos los nodos del grafo espacial para un grafo completo o solo para una sección del grafo.
* Conceder a especialista en soporte técnico acceso de lectura al grafo, excepto a las claves de acceso.
* Conceder a todos los miembros de un dominio acceso de lectura a todos los objetos del grafo.

## <a name="role-based-access-control-best-practices"></a>Procedimientos recomendados del control de acceso basado en roles

[!INCLUDE [digital-twins-permissions](../../includes/digital-twins-rbac-best-practices.md)]

## <a name="roles"></a>Roles

### <a name="role-definitions"></a>Definiciones de roles

Una **definición de roles** es una recopilación de permisos y, a veces, se denomina un **rol**. La definición de roles enumera las operaciones permitidas, incluidos *crear*, *leer*, *actualizar* y *eliminar*. También especifica a qué tipos de objeto se aplican estos permisos.

Los siguientes roles están disponibles en Azure Digital Twins:

* **Administrador del espacio**: permiso para crear, leer, actualizar y eliminar para el espacio especificado y todos los nodos inferiores. Permiso global.
* **Administrador de usuarios**: permiso para crear, leer, actualizar y eliminar para usuarios y objetos relacionados con usuarios. Permiso de lectura para los espacios.
* **Administrador de dispositivo**: permiso para crear, leer, actualizar y eliminar para dispositivos y objetos relacionados con dispositivos. Permiso de lectura para los espacios.
* **Administrador de claves**: permiso para crear, leer, actualizar y eliminar para claves de acceso. Permiso de lectura para los espacios.
* **Administrador de tokens**: permiso para leer y actualizar para claves de acceso. Permiso de lectura para los espacios.
* **Usuario**: permiso de lectura para espacios, sensores y usuarios, incluidos sus correspondientes objetos relacionados.
* **Especialista de soporte técnico**: permiso de lectura para todo, excepto las claves de acceso.
* **Instalador de dispositivos**: permiso de lectura y actualización para dispositivos y sensores, incluidos sus correspondientes objetos relacionados. Permiso de lectura para los espacios.
* **Dispositivo de puerta de enlace**: permiso de creación para sensores. Permiso de lectura para dispositivos y sensores, incluidos sus correspondientes objetos relacionados.

>[!NOTE]
> *Las definiciones completas de los puntos anteriores se pueden recuperar consultando la API del sistema/roles.*

### <a name="object-types"></a>Tipos de objeto

El `ObjectIdType` hace referencia al tipo de identidad a la que se otorga un rol. Además de los tipos `DeviceId` y `UserDefinedFunctionId`, los tipos se corresponden con una propiedad de un objeto de Azure Active Directory (Azure AD):
  
* El tipo `UserId` asigna un rol a un usuario.
* El tipo `DeviceId` asigna un rol a un dispositivo.
* El tipo `DomainName` asigna un rol a un nombre de dominio. Cada usuario con el nombre de dominio especificado tendrá los derechos de acceso del rol correspondiente.
* El tipo `TenantId` asigna un rol a un inquilino. Cada usuario que pertenezca al identificador de inquilino de Azure AD especificado tendrá los derechos de acceso del rol correspondiente.
* El tipo `ServicePrincipalId` asigna un rol a un identificador de objeto de entidad de servicio.
* El tipo `UserDefinedFunctionId` asigna un rol a una función definida por el usuario (UDF).

> [!div class="nextstepaction"]
> [Consulta o identificador de objeto para un usuario](https://docs.microsoft.com/powershell/module/azuread/get-azureaduser?view=azureadps-2.0)

> [!div class="nextstepaction"]
> [Obtener el identificador de objeto para una entidad de servicio](https://docs.microsoft.com/powershell/module/azurerm.resources/get-azurermadserviceprincipal?view=azurermps-6.8.1)

> [!div class="nextstepaction"]
> [Recuperar el identificador de objeto de un inquilino de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/quickstart-create-new-tenant)

## <a name="role-assignments"></a>Asignaciones de roles

Para conceder permisos a un destinatario, se crea una asignación de roles, y se revocan al quitar una asignación de roles. Una asignación de roles de Azure Digital Twins asocia un objeto (usuario, inquilino de Azure AD, etc.), un rol y un espacio. A continuación, se conceden los permisos a todos los objetos que pertenecen a ese espacio, incluido todo el grafo espacial debajo de él.

Por ejemplo, un usuario recibe una asignación de roles con rol `DeviceInstaller` para el nodo raíz de un grafo espacial, que representa un edificio. A continuación, el usuario es capaz de leer y actualizar los dispositivos no solo para ese nodo, sino para todos los demás espacios secundarios en el edificio.

## <a name="next-steps"></a>Pasos siguientes

Para obtener información acerca de la seguridad en Azure Digital Twins, consulte [Create and manage role assignments](./security-create-manage-role-assignments.md) (Creación y administración de las asignaciones de roles).
