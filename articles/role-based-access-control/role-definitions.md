---
title: Descripción de definiciones de roles en RBAC de Azure | Microsoft Docs
description: Obtenga información acerca de las definiciones de roles en el control de acceso basado en rol (RBAC) y cómo definir roles personalizados para la administración de acceso específico de recursos en Azure.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: ''
ms.service: role-based-access-control
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/18/2018
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: ''
ms.openlocfilehash: 9bb7808f2b483fe9cd7d22c6df3fe80d4a98f1f4
ms.sourcegitcommit: 1b8665f1fff36a13af0cbc4c399c16f62e9884f3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/11/2018
ms.locfileid: "35266863"
---
# <a name="understand-role-definitions"></a>Descripción de definiciones de roles

Si quiere comprender el funcionamiento de un rol o si va a crear su propio [rol personalizado](custom-roles.md), resulta útil entender cómo se definen los roles. En este artículo se describen los detalles de las definiciones de roles y se proporcionan algunos ejemplos.

## <a name="role-definition-structure"></a>Estructura de definición de roles

Una *definición de roles* es una recopilación de permisos. A veces, se denomina *rol* simplemente. Una definición de roles enumera las operaciones que se pueden realizar, por ejemplo, de lectura, escritura y eliminación. También puede enumerar las operaciones que no se pueden realizar u operaciones relacionadas con datos subyacentes. Tiene la siguiente estructura:

```
assignableScopes []
description
id
name
permissions []
  actions []
  dataActions []
  notActions []
  notDataActions []
roleName
roleType
type
```

Se especifican las operaciones con cadenas que tienen el formato siguiente:

- `Microsoft.{ProviderName}/{ChildResourceType}/{action}`

La parte `{action}` de una cadena de la operación especifica el tipo de operaciones que puede realizar en un tipo de recurso. Por ejemplo, verá las siguientes subcadenas en `{action}`:

| Subcadena de acción    | DESCRIPCIÓN         |
| ------------------- | ------------------- |
| `*` | El carácter comodín concede acceso a todas las operaciones que coinciden con la cadena. |
| `read` | Permite operaciones de lectura (GET). |
| `write` | Permite operaciones de escritura (PUT, POST y PATCH). |
| `delete` | Permite operaciones de eliminación (DELETE). |

Esta es la definición de roles [Colaborador](built-in-roles.md#contributor) en formato JSON. La operación de carácter comodín (`*`) en `actions` indica que la entidad de seguridad asignada a este rol puede realizar todas las acciones o, en otras palabras, administrar todo el contenido. Esto incluye las acciones definidas en el futuro, a medida que Azure agregue nuevos tipos de recursos. Las operaciones en `notActions` se restan de `actions`. En el caso del rol [Colaborador](built-in-roles.md#contributor), `notActions` le quita la capacidad para administrar y asignar el acceso a los recursos.

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you manage everything except access to resources.",
    "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c",
    "name": "b24988ac-6180-42a0-ab88-20f7382dd24c",
    "permissions": [
      {
        "actions": [
          "*"
        ],
        "additionalProperties": {},
        "dataActions": [],
        "notActions": [
          "Microsoft.Authorization/*/Delete",
          "Microsoft.Authorization/*/Write",
          "Microsoft.Authorization/elevateAccess/Action"
        ],
        "notDataActions": []
      }
    ],
    "roleName": "Contributor",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

## <a name="management-and-data-operations-preview"></a>Administración y operaciones de datos (versión preliminar)

El control de acceso basado en rol para las operaciones de administración se especifica en las secciones `actions` y `notActions` de una definición de roles. A continuación, se incluyen algunos ejemplos de operaciones de administración de Azure:

- Administrar el acceso a una cuenta de almacenamiento
- Crear, actualizar o eliminar un contenedor de blobs
- Eliminar un grupo de recursos y todos sus recursos

El acceso a la administración no se hereda con los datos. Esta separación impide que los roles con caracteres comodín (`*`) tengan acceso no restringido a los datos. Por ejemplo, si un usuario tiene el rol [Lector](built-in-roles.md#reader) en una suscripción, podrá ver la cuenta de almacenamiento, pero, de forma predeterminada, no podrá ver los datos subyacentes.

Anteriormente, el control de acceso basado en rol no se usaba para operaciones de datos. La autorización para las operaciones de datos variaba entre proveedores de recursos. El mismo modelo de autorización de control de acceso basado en rol que se utiliza para las operaciones de administración se ha ampliado a las operaciones de datos (actualmente en versión preliminar).

Para admitir las operaciones de datos, se agregaron nuevas secciones de datos a la estructura de la definición de roles. Las operaciones de datos se especifican en las secciones `dataActions` y `notDataActions`. Al agregar estas secciones de datos, se mantiene la separación entre la administración y los datos. Esto impide que las asignaciones de roles actuales con caracteres comodín (`*`) de repente tengan acceso a los datos. Estas son algunas operaciones de datos que se pueden especificar en `dataActions` y `notDataActions`:

- Leer una lista de blobs en un contenedor
- Escribir un blob de almacenamiento en un contenedor
- Eliminar un mensaje de una cola

Esta es la definición de roles del [Lector de datos de blobs de almacenamiento (versión preliminar)](built-in-roles.md#storage-blob-data-reader-preview), que incluye operaciones en las secciones `actions` y `dataActions`. Esta función permite leer el contenedor de blobs y también los datos de blob subyacentes.

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Allows for read access to Azure Storage blob containers and data.",
    "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
    "name": "2a2b9908-6ea1-4ae2-8e65-a410df84e7d1",
    "permissions": [
      {
        "actions": [
          "Microsoft.Storage/storageAccounts/blobServices/containers/read"
        ],
        "additionalProperties": {},
        "dataActions": [
          "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read"
        ],
        "notActions": [],
        "notDataActions": []
      }
    ],
    "roleName": "Storage Blob Data Reader (Preview)",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

Solo se pueden agregar las operaciones de datos a las secciones `dataActions` y `notDataActions`. Los proveedores de recursos identifican qué operaciones son operaciones de datos, al establecer la propiedad `isDataAction` en `true`. Para ver una lista de las operaciones donde `isDataAction` es `true`, consulte [Resource provider operations](resource-provider-operations.md) (Operaciones de proveedor de recursos). Los roles que no tienen operaciones de datos no necesitan las secciones `dataActions` y `notDataActions` en la definición de roles.

Azure Resource Manager controla la autorización de todas las llamadas API de operación de administración. Un proveedor de recursos o Azure Resource Manager controlan la autorización de las llamadas API de operación de datos.

### <a name="data-operations-example"></a>Ejemplo de operaciones de datos

Para comprender mejor cómo funcionan las operaciones de datos y de administración, veamos un ejemplo concreto. A Alice se le ha asignado el rol [Propietario](built-in-roles.md#owner) en el ámbito de la suscripción. A Bob se le ha asignado el rol [Colaborador de datos de blobs de almacenamiento (versión preliminar)](built-in-roles.md#storage-blob-data-contributor-preview) en un ámbito de la cuenta de almacenamiento. En el siguiente diagrama se muestra este ejemplo.

![El control de acceso basado en rol se ha ampliado para admitir operaciones de datos y de administración](./media/role-definitions/rbac-management-data.png)

El rol [Propietario](built-in-roles.md#owner) para Alice y el rol [Colaborador de datos de blobs de almacenamiento (versión preliminar)](built-in-roles.md#storage-blob-data-contributor-preview) para Bob tienen las siguientes acciones:

Propietario

&nbsp;&nbsp;&nbsp;&nbsp;Acciones<br>
&nbsp;&nbsp;&nbsp;&nbsp;`*`

Colaborador de datos de blobs de almacenamiento (versión preliminar)

&nbsp;&nbsp;&nbsp;&nbsp;Acciones<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/delete`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/read`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/write`<br>
&nbsp;&nbsp;&nbsp;&nbsp;DataActions<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read`<br>
&nbsp;&nbsp;&nbsp;&nbsp;`Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write`

Puesto que Alicia tiene una acción de carácter comodín (`*`) en un ámbito de suscripción, sus permisos se heredan para poder realizar todas las acciones de administración. Sin embargo, Alice no puede realizar operaciones de datos. Por ejemplo, de forma predeterminada, Alice no puede leer los blobs de un contenedor, pero puede leer, escribir y eliminar contenedores.

Los permisos de Bob se restringen a solo los `actions` y `dataActions` especificados en el rol [Colaborador de datos de blobs de almacenamiento (versión preliminar)](built-in-roles.md#storage-blob-data-contributor-preview). Según el rol, Bob puede realizar operaciones de datos y de administración. Por ejemplo, Bob puede leer, escribir y eliminar los contenedores de la cuenta de almacenamiento especificada y también puede leer, escribir y eliminar los blobs.

### <a name="what-tools-support-using-rbac-for-data-operations"></a>¿Qué herramientas se admiten cuando se usa RBAC para las operaciones de datos?

Para visualizar y trabajar con operaciones de datos, debe tener las versiones correctas de las herramientas o SDK:

| Herramienta  | Versión  |
|---------|---------|
| [Azure PowerShell](/powershell/azure/install-azurerm-ps) | 5.6.0 o posterior |
| [CLI de Azure](/cli/azure/install-azure-cli) | 2.0.30 o posterior |
| [Azure para .NET](/dotnet/azure/) | 2.8.0-versión preliminar o posterior |
| [Azure SDK para Go](/go/azure/azure-sdk-go-install) | 15.0.0 o posterior |
| [Azure para Java](/java/azure/) | 1.9.0 o posterior |
| [Azure para Python](/python/azure) | 0.40.0 o posterior |
| [SDK de Azure para Ruby](https://rubygems.org/gems/azure_sdk) | 0.17.1 o posterior |

## <a name="actions"></a>actions

El permiso `actions` especifica las operaciones de administración a las que concede acceso el rol. Se trata de una colección de cadenas de operación que identifican a las operaciones protegibles de proveedores de recursos de Azure. A continuación, se incluyen algunos ejemplos de operaciones de administración que se pueden usar en `actions`.

| Cadena de la operación    | DESCRIPCIÓN         |
| ------------------- | ------------------- |
| `*/read` | Concede acceso a las operaciones de lectura a todos los tipos de recursos de todos los proveedores de recursos de Azure.|
| `Microsoft.Compute/*` | Concede acceso a todas las operaciones a todos los tipos de recursos del proveedor de recursos Microsoft.Compute.|
| `Microsoft.Network/*/read` | Concede acceso a las operaciones de lectura a todos los tipos de recursos del proveedor de recursos Microsoft.Network.|
| `Microsoft.Compute/virtualMachines/*` | Concede acceso a todas las operaciones de las máquinas virtuales y a sus tipos de recursos secundarios.|
| `microsoft.web/sites/restart/Action` | Concede acceso para reiniciar una aplicación web.|

## <a name="notactions"></a>notActions

El permiso `notActions` especifica las operaciones de administración que se excluyen de los `actions` permitidos. Use el permiso `notActions` si el conjunto de operaciones que quiere permitir se define más fácilmente mediante la exclusión de las operaciones restringidas. El acceso concedido por un rol (permisos vigentes) se calcula restando las operaciones de `notActions` de las de `actions`.

> [!NOTE]
> Si un usuario tiene asignado un rol que excluye una operación en `notActions` y se le asigna un segundo rol que sí que concede acceso a esa operación, el usuario puede realizar dicha operación. `notActions` no es una regla de denegación, es simplemente una manera cómoda de crear un conjunto de operaciones permitidas cuando es necesario excluir operaciones específicas.
>

## <a name="dataactions-preview"></a>dataActions (versión preliminar)

El permiso `dataActions` especifica las operaciones de datos a las que el rol concede acceso a los datos en ese objeto. Por ejemplo, si un usuario tiene acceso para leer datos de blobs de una cuenta de almacenamiento, puede leer los blobs en esa cuenta de almacenamiento. A continuación, se incluyen algunos ejemplos de operaciones de datos que se pueden usar en `dataActions`.

| Cadena de la operación    | DESCRIPCIÓN         |
| ------------------- | ------------------- |
| `Microsoft.Storage/storageAccounts/ blobServices/containers/blobs/read` | Devuelve un blob o una lista de blobs. |
| `Microsoft.Storage/storageAccounts/ blobServices/containers/blobs/write` | Devuelve el resultado de la escritura de un blob. |
| `Microsoft.Storage/storageAccounts/ queueServices/queues/messages/read` | Devuelve un mensaje. |
| `Microsoft.Storage/storageAccounts/ queueServices/queues/messages/*` | Devuelve un mensaje o el resultado de la escritura o eliminación de un mensaje. |

## <a name="notdataactions-preview"></a>notDataActions (versión preliminar)

El permiso `notDataActions` especifica las operaciones de datos que se excluyen de los valores `dataActions` permitidos. El acceso concedido por un rol (permisos vigentes) se calcula restando las operaciones de `notDataActions` de las de `dataActions`. Cada proveedor de recursos proporciona su conjunto respectivo de API para completar las operaciones de datos.

> [!NOTE]
> Si un usuario tiene asignado un rol que excluye una operación de datos en `notDataActions` y se le asigna un segundo rol que sí que concede acceso a esa operación de datos, el usuario puede realizar dicha operación de datos. `notDataActions` no es una regla de denegación, es simplemente una manera cómoda de crear un conjunto de operaciones de datos permitidas cuando es necesario excluir operaciones de datos específicas.
>

## <a name="assignablescopes"></a>assignableScopes

La sección `assignableScopes` especifica los ámbitos (grupos de administración [actualmente en versión preliminar], suscripciones, grupos de recursos o recursos) en los que el rol está disponible para la asignación. Puede permitir que el rol esté disponible para su asignación solamente en las suscripciones o los grupos de recursos que lo requieran, sin necesidad de abarrotar la experiencia de usuario con el resto de las suscripciones o grupos de recursos. Tiene que utilizar al menos un grupo de administración, una suscripción, un grupo de recursos o un identificador de recurso.

Los roles integrados tienen `assignableScopes` establecido en el ámbito raíz (`"/"`). El ámbito raíz indica que el rol está disponible para la asignación en todos los ámbitos. No puede usar el ámbito raíz en sus propios roles personalizados. Si lo intenta, se producirá un error de autorización.

Ejemplos de ámbitos asignables válidos son:

| Escenario | Ejemplo |
|----------|---------|
| El rol está disponible para la asignación en una única suscripción. | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e"` |
| El rol está disponible para la asignación en dos suscripciones. | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e", "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624"` |
| El rol está disponible para la asignación solo en el grupo de recursos de red. | `"/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network"` |
| El rol está disponible para la asignación en todos los ámbitos. | `"/"` |

## <a name="assignablescopes-and-custom-roles"></a>assignableScopes y roles personalizados

La sección `assignableScopes` de un rol personalizado también controla quién puede crear, eliminar, modificar o ver dicho rol.

| Task | Operación | DESCRIPCIÓN |
| --- | --- | --- |
| Creación o eliminación de un rol personalizado | `Microsoft.Authorization/ roleDefinition/write` | Los usuarios que tienen acceso a esta operación en todos los ámbitos `assignableScopes` del rol personalizado pueden crear (o eliminar) roles personalizados para usar en esos ámbitos. Por ejemplo, los [propietarios](built-in-roles.md#owner) y [administradores del acceso de los usuarios](built-in-roles.md#user-access-administrator) de las suscripciones, los grupos de recursos y los recursos. |
| Modificación de un rol personalizado | `Microsoft.Authorization/ roleDefinition/write` | Los usuarios que tienen acceso a esta operación en todos los ámbitos `assignableScopes` del rol personalizado pueden modificar roles personalizados en esos ámbitos. Por ejemplo, los [propietarios](built-in-roles.md#owner) y [administradores del acceso de los usuarios](built-in-roles.md#user-access-administrator) de las suscripciones, los grupos de recursos y los recursos. |
| Visualización de un rol personalizado | `Microsoft.Authorization/ roleDefinition/read` | Los usuarios que tienen acceso a esta operación en un ámbito pueden ver los roles personalizados que están disponibles para su asignación en ese ámbito. Todos los roles integrados permiten que los roles personalizados estén disponibles para la asignación. |

## <a name="role-definition-examples"></a>Ejemplos de definición de roles

En el siguiente ejemplo, se muestra la definición de roles [Lector](built-in-roles.md#reader) tal como se muestra mediante la CLI de Azure:

```json
[
  {
    "additionalProperties": {},
    "assignableScopes": [
      "/"
    ],
    "description": "Lets you view everything, but not make any changes.",
    "id": "/subscriptions/{subscriptionId}/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "name": "acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "permissions": [
      {
        "actions": [
          "*/read"
        ],
        "additionalProperties": {},
        "dataActions": [],
        "notActions": [],
        "notDataActions": []
      }
    ],
    "roleName": "Reader",
    "roleType": "BuiltInRole",
    "type": "Microsoft.Authorization/roleDefinitions"
  }
]
```

A continuación, se muestra un ejemplo de rol personalizado para supervisar y reiniciar máquinas virtuales, tal como se muestra mediante Azure PowerShell:

```json
{
  "Name":  "Virtual Machine Operator",
  "Id":  "88888888-8888-8888-8888-888888888888",
  "IsCustom":  true,
  "Description":  "Can monitor and restart virtual machines.",
  "Actions":  [
                  "Microsoft.Storage/*/read",
                  "Microsoft.Network/*/read",
                  "Microsoft.Compute/*/read",
                  "Microsoft.Compute/virtualMachines/start/action",
                  "Microsoft.Compute/virtualMachines/restart/action",
                  "Microsoft.Authorization/*/read",
                  "Microsoft.Resources/subscriptions/resourceGroups/read",
                  "Microsoft.Insights/alertRules/*",
                  "Microsoft.Insights/diagnosticSettings/*",
                  "Microsoft.Support/*"
  ],
  "NotActions":  [

                 ],
  "DataActions":  [

                  ],
  "NotDataActions":  [

                     ],
  "AssignableScopes":  [
                           "/subscriptions/{subscriptionId1}",
                           "/subscriptions/{subscriptionId2}",
                           "/subscriptions/{subscriptionId3}"
                       ]
}
```

## <a name="see-also"></a>Otras referencias

* [Roles integrados](built-in-roles.md)
* [Roles personalizados](custom-roles.md)
* [Operaciones del proveedor de recursos de Azure Resource Manager](resource-provider-operations.md)
