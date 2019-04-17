---
title: Control de acceso basado en roles para cuentas de Media Services - Azure | Microsoft Docs
description: Este artículo describe el control de acceso basado en roles (RBAC) para las cuentas de Azure Media Services.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/15/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 82c438ea246ba18fa7dac2281d68a3ea7ba41db8
ms.sourcegitcommit: fec96500757e55e7716892ddff9a187f61ae81f7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/16/2019
ms.locfileid: "59617723"
---
# <a name="role-based-access-control-rbac-for-media-services-accounts"></a>Control de acceso basado en roles (RBAC) para cuentas de Media Services

Actualmente, Azure Media Services no define los roles personalizados específicos para el servicio. Para obtener acceso completo a la cuenta de Media Services, los clientes pueden usar las funciones integradas de **propietario** o **colaborador**. Es la diferencia principal entre estos roles: el **propietario** puede controlar quién tiene acceso a un recurso y el **colaborador** no. Integrado **lector** también se puede usar la función pero el usuario o la aplicación solo tendrá acceso de lectura a las API de Media Services. 

## <a name="design-principles"></a>Principios de diseño

Uno de los principales principios de diseño de la versión v3 de la API es hacerla más segura. las API de V3 no devuelven secretos o las credenciales en **obtener** o **lista** operaciones. Las claves se hacen siempre NULL, se vacían o se sanean de la respuesta. El usuario debe llamar a un método de acción independiente para obtener los secretos o las credenciales. El **lector** rol no puede llamar a operaciones como Asset.ListContainerSas, StreamingLocator.ListContentKeys, ContentKeyPolicies.GetPolicyPropertiesWithSecrets. Tener acciones independientes le permite establecer permisos de seguridad más granulares de RBAC en un rol personalizado si lo desea.

Para obtener una lista de las operaciones de Media Services admite, haga lo:

```csharp
foreach (Microsoft.Azure.Management.Media.Models.Operation a in client.Operations.List())
{
    Console.WriteLine($"{a.Name} - {a.Display.Operation} - {a.Display.Description}");
}
```

El [definiciones de roles integrados](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles) artículo explica lo que concede el rol. 

Para obtener más información, consulte los siguientes artículos:

- [Roles de administrador de suscripciones clásico de Azure RBAC y roles de administrador de Azure AD](https://docs.microsoft.com/azure/role-based-access-control/rbac-and-directory-admin-roles)
- [¿Qué es RBAC para recursos de Azure?](https://docs.microsoft.com/azure/role-based-access-control/overview)
- [Utilice RBAC para administrar el acceso](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest)

## <a name="next-steps"></a>Pasos siguientes

- [Desarrollo con Media Services API v3](media-services-apis-overview.md)
- [Obtener la directiva de clave de contenido mediante .NET de Media Services](get-content-key-policy-dotnet-howto.md)
