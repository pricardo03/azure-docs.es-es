---
title: Autenticación con Azure Maps | Microsoft Docs
description: Autenticación para usar los servicios de Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 10/24/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.custom: mvc
ms.openlocfilehash: 84af496a92bd3c7b30062e965335782f7661aa4a
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73575651"
---
# <a name="authentication-with-azure-maps"></a>Autenticación con Azure Maps

Azure Maps admite dos formas de autenticar las solicitudes: Clave compartida y Azure Active Directory (Azure AD). En este artículo se explican estos métodos de autenticación para guiar su implementación.

## <a name="shared-key-authentication"></a>Autenticación de clave compartida

La autenticación de clave compartida pasa las claves generadas por una cuenta de Azure Maps con cada solicitud a Azure Maps.  Cuando se crea la cuenta de Azure Maps se generan dos claves. Cada solicitud a los servicios de Azure Maps requiere que se agregue la clave de suscripción como parámetro a la dirección URL.

> [!Tip]
> Se recomienda regenerar las claves periódicamente. Como se le proporcionan dos claves, puede mantener las conexiones con una mientras se regenera la otra. Cuando regenere las claves, debe actualizar todas las aplicaciones que acceden a la cuenta para usar las nuevas claves.

Para información sobre cómo ver las claves, consulte [Visualización de los detalles de la autenticación](https://aka.ms/amauthdetails).

## <a name="authentication-with-azure-active-directory-preview"></a>Autenticación con Azure Active Directory (versión preliminar)

Azure Maps ofrece ahora la integración con [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) para la autenticación de solicitudes de servicios de Azure Maps. Azure AD proporciona autenticación basada en identidad, como [control de acceso basado en rol (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview), para conceder acceso de nivel de usuario, de nivel de grupo y de nivel de aplicación a recursos de Azure Maps. Las secciones siguientes pueden ayudarle a comprender los conceptos y componentes de la integración de Azure Maps con Azure AD.

## <a name="authentication-with-oauth-access-tokens"></a>Autenticación con tokens de acceso de OAuth

Azure Maps acepta tokens de acceso de **OAuth 2.0** para inquilinos de Azure AD asociados a suscripciones de Azure que contienen una cuenta de Azure Maps. Azure Maps acepta tokens de:

* Usuarios de Azure AD 
* Aplicaciones de asociados que usan permisos delegados por los usuarios
* Identidades administradas de recursos de Azure.

Azure Maps genera un *identificador único (Id. de cliente)* para cada cuenta de Azure Maps. Cuando este identificador de cliente se combina con otros parámetros, puede solicitar tokens de Azure AD mediante la especificación de valores de la siguiente tabla, en función de su entorno de Azure.

| Entorno de Azure   | Punto de conexión del token de Azure AD |
| --------------------|-------------------------|
| Azure Public        | https://login.microsoftonline.com |
| Azure Government    | https://login.microsoftonline.us |


Para más información sobre cómo configurar Azure AD y solicitar tokens para Azure Maps, consulte [Administración de la autenticación en Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

Para información general sobre la solicitud de tokens de Azure AD, consulte [¿Qué es la autenticación?](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).

## <a name="request-azure-map-resources-with-oauth-tokens"></a>Solicitud de recursos de Azure Maps con tokens de OAuth

Después de recibir un token de Azure AD, se puede enviar una solicitud a Azure Maps con los dos siguientes encabezados de solicitud requeridos establecidos:

| Encabezado de solicitud    |    Valor    |
|:------------------|:------------|
| x-ms-client-id    | 30d7cc….9f55|
| Authorization     | Bearer eyJ0e….HNIVN |

> [!Note]
> `x-ms-client-id` es el GUID basado en la cuenta de Azure Maps que aparece en la página de autenticación de Azure Maps.

A continuación se muestra un ejemplo de una solicitud de ruta de Azure Maps que usa un token de OAuth:

```
GET /route/directions/json?api-version=1.0&query=52.50931,13.42936:52.50274,13.43872 
Host: atlas.microsoft.com 
x-ms-client-id: 30d7cc….9f55 
Authorization: Bearer eyJ0e….HNIVN 
```

Para información sobre cómo ver el identificador de cliente, consulte [Visualización de los detalles de la autenticación](https://aka.ms/amauthdetails).

## <a name="control-access-with-rbac"></a>Control del acceso con RBAC

Azure AD le permite a que controlar el acceso a recursos protegidos mediante RBAC. Después de crear la cuenta de Azure Maps y de registrar la aplicación de Azure AD de Azure Maps dentro del inquilino de Azure AD, puede configurar RBAC para un usuario, un grupo, una aplicación o un recurso de Azure en la página del portal de cuentas de Azure Maps.

Azure Maps admite control de acceso de lectura para usuarios, grupos y aplicaciones de Azure AD, y servicios de Azure individuales mediante identidades administradas para recursos de Azure.

![Azure Maps Data Reader (versión preliminar)](./media/azure-maps-authentication/concept.png)

Para información sobre cómo ver la configuración de RBAC, consulte [Configuración de Azure Maps](https://aka.ms/amrbac).

## <a name="managed-identities-for-azure-resources-and-azure-maps"></a>Identidades administradas para recursos de Azure y Azure Maps

Las [identidades administradas para recursos de Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) proporcionan servicios de Azure (Azure App Service, Azure Functions, Azure Virtual Machines, etc.) con una identidad administrada automáticamente a la que se puede autorizar el acceso a servicios de Azure Maps.  

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre la autenticación de una aplicación con Azure AD y Azure Maps, consulte [Administración de la autenticación en Azure Maps](https://docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

* Para más información sobre la autenticación del Control mapa de Azure Maps y Azure AD, consulte [Uso del Control de mapa de Azure Maps](https://aka.ms/amaadmc).
