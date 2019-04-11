---
title: Desarrollar con las API de v3 - Azure | Microsoft Docs
description: En este artículo se describe las reglas que se aplican a las entidades y las API cuando se desarrolla con Media Services v3.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 04/08/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 18b72ceaee0ca0747a0bf2144d5f9ffddbee8b8c
ms.sourcegitcommit: 6e32f493eb32f93f71d425497752e84763070fad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2019
ms.locfileid: "59471932"
---
# <a name="developing-with-media-services-v3-apis"></a>Desarrollo con Media Services API v3

En este artículo se describe las reglas que se aplican a las entidades y las API cuando se desarrolla con Media Services v3.

## <a name="naming-conventions"></a>Convenciones de nomenclatura

Los nombres de recursos de Azure Media Services v3 (por ejemplo, recursos, trabajos y transformaciones) están sujetos a las restricciones de nomenclatura de Azure Resource Manager. De acuerdo con Azure Resource Manager, los nombres de los recursos siempre son únicos. Por lo tanto, puede usar cualquier cadena de identificador único (por ejemplo, GUID) para los nombres de recursos. 

Los nombres de recursos de Media Services no pueden incluir: '<', '>', '%', '&', ':', '&#92;', '?', '/', '*', '+', '.', el carácter de comilla simple o cualquier carácter de control. Todos los demás caracteres se permiten. La longitud máxima de un nombre de recurso es de 260 caracteres. 

Para más información sobre la nomenclatura de Azure Resource Manager, consulte: [Requisitos de nomenclatura](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource) y [Convenciones de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).

## <a name="v3-api-design-principles-and-rbac"></a>principios de diseño de API de V3 y RBAC

Uno de los principales principios de diseño de la versión v3 de la API es hacerla más segura. las API de V3 no devuelven secretos o las credenciales en **obtener** o **lista** operaciones. Las claves se hacen siempre NULL, se vacían o se sanean de la respuesta. El usuario debe llamar a un método de acción independiente para obtener los secretos o las credenciales. El **lector** rol no puede llamar a las operaciones por lo que no puede llamar a operaciones como Asset.ListContainerSas, StreamingLocator.ListContentKeys, ContentKeyPolicies.GetPolicyPropertiesWithSecrets. Tener acciones independientes le permite establecer permisos de seguridad más granulares de RBAC en un rol personalizado si lo desea.

Para más información, consulte:

- [Definiciones de roles integrados](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles)
- [Utilice RBAC para administrar el acceso](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-rest)
- [Control de acceso basado en roles para cuentas de Media Services](rbac-overview.md)
- [Obtener la directiva de clave de contenido - .NET](get-content-key-policy-dotnet-howto.md).

## <a name="long-running-operations"></a>Operaciones de larga ejecución

Las operaciones marcan con `x-ms-long-running-operation` de Azure Media Services [swagger archivos](https://github.com/Azure/azure-rest-api-specs/blob/master/specification/mediaservices/resource-manager/Microsoft.Media/stable/2018-07-01/streamingservice.json) son largos las operaciones de ejecución. 

Para obtener más información acerca de cómo realizar un seguimiento de las operaciones asincrónicas de Azure, consulte [operaciones asincrónicas](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-async-operations#monitor-status-of-operation).

Media Services tiene las siguientes operaciones de larga ejecución:

* Crear LiveEvent
* Actualizar LiveEvent
* Eliminar LiveEvent
* Iniciar LiveEvent
* Detener LiveEvent
* Restablecer LiveEvent
* Crear LiveOutput
* Eliminar LiveOutput
* Crear StreamingEndpoint
* Actualizar StreamingEndpoint
* Eliminar StreamingEndpoint
* Iniciar StreamingEndpoint
* Detener StreamingEndpoint
* Escala StreamingEndpoint

## <a name="filtering-ordering-paging-of-media-services-entities"></a>Filtrado, ordenación y paginación de entidades de Media Services

Consulte [filtrado, ordenación, paginación de entidades de Azure Media Services](entities-overview.md)

## <a name="next-steps"></a>Pasos siguientes

[Empiece a desarrollar con la API de Media Services v3, SDK y herramientas](developers-guide.md)
