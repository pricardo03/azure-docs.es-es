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
ms.date: 04/21/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: b80f11ef97a10728f07cebe1fe80b954e506da52
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65147892"
---
# <a name="developing-with-media-services-v3-apis"></a>Desarrollo con Media Services API v3

En este artículo se describe las reglas que se aplican a las entidades y las API cuando se desarrolla con Media Services v3.

## <a name="accessing-the-azure-media-services-api"></a>Obtener acceso a la API de Azure Media Services

Para obtener acceso a los recursos de Azure Media Services, puede usar la autenticación de entidad de servicio de Azure Active Directory (AD).
La API de Media Services requiere que el usuario o aplicación que realiza la API de REST solicita tiene acceso al recurso de la cuenta de Media Services y usar un **colaborador** o **propietario** rol. Se puede tener acceso a la API con la **lector** , pero solo para el rol **obtener** o **lista**   operaciones estará disponibles. Para obtener más información, consulte [control de acceso basado en roles para cuentas de Media Services](rbac-overview.md).

En lugar de crear a una entidad de servicio, considere el uso de identidades administradas para los recursos de Azure para tener acceso a la API de Media Services mediante Azure Resource Manager. Para obtener más información acerca de las identidades administradas para los recursos de Azure, consulte [What ' s identidades administradas para los recursos de Azure](../../active-directory/managed-identities-azure-resources/overview.md).

### <a name="azure-ad-service-principal"></a>Entidad de servicio de Azure AD 

Si va a crear una aplicación de Azure AD y el servicio principal, la aplicación debe estar en su propio inquilino. Después de crear la aplicación, asigne a la aplicación **colaborador** o **propietario** rol el acceso a la cuenta de Media Services. 

Si no estás seguro de que tenga los permisos para crear una aplicación de Azure AD, consulte [permisos necesarios](../../active-directory/develop/howto-create-service-principal-portal.md#required-permissions).

En la siguiente ilustración, los números representan el flujo de las solicitudes en orden cronológico:

![Aplicaciones de nivel intermedio](./media/use-aad-auth-to-access-ams-api/media-services-principal-service-aad-app1.png)

1. Una aplicación de nivel intermedio solicita un token de acceso de Azure AD que tiene los siguientes parámetros:  

   * Punto de conexión de inquilino de Azure AD.
   * URI del recurso de Media Services.
   * URI del recurso de Media Services de REST.
   * Valores de aplicación de Azure AD: el Id. de cliente y el secreto de cliente.
   
   Para obtener todos los valores necesarios, consulte [Access Azure Media Services API con la CLI de Azure](access-api-cli-how-to.md)

2. El token de acceso de Azure AD se envía al nivel intermedio.
4. El nivel intermedio envía una solicitud a la API de REST de Azure Media Services con el token de Azure AD.
5. El nivel intermedio recibe los datos de Media Services.

## <a name="naming-conventions"></a>Convenciones de nomenclatura

Los nombres de recursos de Azure Media Services v3 (por ejemplo, recursos, trabajos y transformaciones) están sujetos a las restricciones de nomenclatura de Azure Resource Manager. De acuerdo con Azure Resource Manager, los nombres de los recursos siempre son únicos. Por lo tanto, puede usar cualquier cadena de identificador único (por ejemplo, GUID) para los nombres de recursos. 

Los nombres de recursos de Media Services no pueden incluir: '<', '>', '%', '&', ':', '&#92;', '?', '/', '*', '+', '.', el carácter de comilla simple o cualquier carácter de control. Todos los demás caracteres se permiten. La longitud máxima de un nombre de recurso es de 260 caracteres. 

Para más información sobre la nomenclatura de Azure Resource Manager, consulte: [Requisitos de nomenclatura](https://github.com/Azure/azure-resource-manager-rpc/blob/master/v1.0/resource-api-reference.md#arguments-for-crud-on-resource) y [Convenciones de nomenclatura](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).

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

## <a name="ask-questions-give-feedback-get-updates"></a>Formule preguntas, comentarios, obtener actualizaciones

Consulte el artículo [Comunidad de Azure Media Services](media-services-community.md) para ver diferentes formas de formular preguntas, enviar comentarios y obtener actualizaciones de Media Services.

## <a name="next-steps"></a>Pasos siguientes

[Empiece a desarrollar con API de Media Services v3 mediante los SDK y herramientas](developers-guide.md)
