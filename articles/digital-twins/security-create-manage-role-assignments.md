---
title: Descripción de la conectividad y la autenticación de dispositivos con Azure Digital Twins | Microsoft Docs
description: Uso de Azure Digital Twins para conectar y autenticar dispositivos
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: lyrana
ms.openlocfilehash: adfb4c369ea1b324da8562a5b0b245ebdecff602
ms.sourcegitcommit: 74941e0d60dbfd5ab44395e1867b2171c4944dbe
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/15/2018
ms.locfileid: "49323922"
---
# <a name="create-and-manage-role-assignments"></a>Creación y administración de asignaciones de roles

Azure Digital Twins usa el control de acceso basado en rol ([RBAC](./security-role-based-access-control.md)) para administrar el acceso a los recursos.

Cada asignación de roles incluye:

* Un **identificador de objeto** (un identificador de Azure Active Directory, el identificador de objeto de la entidad de servicio o el nombre de dominio).
* Un **tipo de identificador de objeto**.
* Un **identificador de definición de roles**.
* Una **ruta de acceso al espacio**.
* (En la mayoría de los casos) un **identificador de inquilino** de Azure Active Directory.

## <a name="role-definition-identifiers"></a>Identificadores de definición de roles

En la siguiente tabla se muestra lo que se puede obtener al consultar las API de sistema o de roles:

| **Rol** | **Identificador** |
| --- | --- |
| Administrador del espacio | 98e44ad7-28d4-4007-853b-b9968ad132d1 |
| Administrador de usuarios| dfaac54c-f583-4dd2-b45d-8d4bbc0aa1ac |
| Administrador de dispositivos | 3cdfde07-bc16-40d9-bed3-66d49a8f52ae |
| Administrador de claves | 5a0b1afc-e118-4068-969f-b50efb8e5da6 |
| Administrador de tokens | 38a3bb21-5424-43b4-b0bf-78ee228840c3 |
| Usuario | b1ffdb77-c635-4e7e-ad25-948237d85b30 |
| Especialista de soporte técnico | 6e46958b-dc62-4e7c-990c-c3da2e030969 |
| Instalador de dispositivos | b16dd9fe-4efe-467b-8c8c-720e2ff8817c |
| Dispositivo de puerta de enlace | d4c69766-e9bd-4e61-bfc1-d8b6e686c7a8 |

## <a name="supported-objectidtypes"></a>ObjectIdTypes admitidos

Los `ObjectIdTypes` admitidos son:

* `UserId`
* `DeviceId`
* `DomainName`
* `TenantId`
* `ServicePrincipalId`
* `UserDefinedFunctionId`

## <a name="create-a-role-assignment"></a>Crear una asignación de rol

```plaintext
HTTP POST /api/v1.0/roleassignments
```

| **Nombre** | **Obligatorio** | **Tipo** | **Descripción** |
| --- | --- | --- | --- |
| roleId| SÍ |string | Identificador de definición de rol. Las definiciones de roles y sus identificadores se encuentran la consultar la API de sistema. |
| objectId | SÍ |string | Identificador de objeto para la asignación de roles que debe tener el formato en función del tipo asociado. Para el ObjectIdType `DomainName`, ObjectId debe comenzar con el carácter `“@”`. |
| objectIdType | SÍ |string | Tipo de asignación de roles. Debe ser una de las siguientes filas de la tabla. |
| tenantId | Varía | string |Identificador de inquilino. No se permite para los ObjectIdType `DeviceId` y `TenantId`. Obligatorio para los ObjectIdType `UserId` y `ServicePrincipalId`. Opcional para el ObjectIdType DomainName. |
| path* | SÍ | string |Ruta de acceso completa al objeto `Space`. Ejemplo: `/{Guid}/{Guid}` Si un identificador, necesita la asignación de roles para todo el grafo, especifique `"/"` (que designa la raíz). Sin embargo, su uso no se recomienda y **debe siempre seguir el principio de privilegio mínimo**. |

## <a name="sample-configuration"></a>Configuración de ejemplo

Un usuario necesita permiso del administrador para acceder a una planta del espacio del inquilino:

  ```JSON
    {
      "RoleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
      "ObjectId" : " 0fc863bb-eb51-4704-a312-7d635d70e599",
      "ObjectIdType" : "UserId",
      "TenantId": " a0c20ae6-e830-4c60-993d-a91ce6032724",
      "Path": "/ 091e349c-c0ea-43d4-93cf-6b57abd23a44/ d84e82e6-84d5-45a4-bd9d-006a118e3bab"
    }
  ```

Una aplicación que ejecuta simulaciones de dispositivos y sensores en escenarios de prueba:

  ```JSON
    {
      "RoleId": "98e44ad7-28d4-4007-853b-b9968ad132d1",
      "ObjectId" : "cabf7acd-af0b-41c5-959a-ce2f4c26565b",
      "ObjectIdType" : "ServicePrincipalId",
      "TenantId": " a0c20ae6-e830-4c60-993d-a91ce6032724",
      "Path": "/"
    }
  ```

Todos los usuarios que forman parte de un dominio tendrán permiso de lectura para espacios, sensores y usuarios, incluidos sus correspondientes objetos relacionados:

  ```JSON
    {
      "RoleId": " b1ffdb77-c635-4e7e-ad25-948237d85b30",
      "ObjectId" : "@microsoft.com",
      "ObjectIdType" : "DomainName",
      "Path": "/091e349c-c0ea-43d4-93cf-6b57abd23a44"
    }
  ```

Para obtener (GET) una asignación de roles:

```plaintext
HTTP GET /api/v1/roleassignments?path={path}
```

| **Nombre** | **In** | **Obligatorio** |    **Tipo** |  **Descripción** |
| --- | --- | --- | --- | --- |
| Ruta de acceso | Ruta de acceso | True | string | Ruta de acceso completa al espacio |

Para eliminar (DELETE) una asignación de roles:

```plaintext
HTTP DELETE /api/v1/roleassignments/{id}
```

| **Nombre** | **In** | **Obligatorio** | **Tipo** | **Descripción** |
| --- | --- | --- | --- | --- |
| ID | Ruta de acceso | True | string |   Identificador de la asignación de roles |

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre la seguridad de Azure Digital Twins, consulte el artículo sobre la [autenticación en las API](./security-authenticating-apis.md).
