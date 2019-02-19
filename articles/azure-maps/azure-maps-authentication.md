---
title: Autenticación con Azure Maps | Microsoft Docs
description: Autenticación para usar los servicios de Azure Maps.
author: walsehgal
ms.author: v-musehg
ms.date: 02/12/2019
ms.topic: tutorial
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.custom: mvc
ms.openlocfilehash: b97c303433eb8fadcda51257d37447f052ce4a3b
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56118846"
---
# <a name="authentication-with-azure-maps"></a>Autenticación con Azure Maps

Azure Maps admite dos formas de autenticar las solicitudes. Clave compartida o Azure Active Directory (Azure AD) ofrecen distintos métodos de autorizar las solicitudes que se envían a Azure Maps. Este artículo tiene como fin explicar ambos métodos de autenticación para guiarle en su implementación de la autenticación.

## <a name="shared-key-authentication"></a>Autenticación de clave compartida

La autenticación de clave compartida se basa en pasar las claves generadas por la cuenta de Azure Maps con cada solicitud a Azure Maps.  Cuando se crea la cuenta de Azure Maps se generan dos claves.  Cada solicitud a los servicios de Azure Maps requiere que se agregue la clave de suscripción como parámetro a la dirección URL.

> [!Tip]
> Se recomienda regenerar las claves periódicamente. Como se le proporcionan dos claves de acceso, puede mantener las conexiones con una mientras se regenera la otra. Cuando regenere las claves, debe actualizar todas las aplicaciones que acceden a esta cuenta para usar las nuevas claves.

Para ver las claves, consulte [Authentication Details](https://aka.ms/amauthdetails) (Detalles de autenticación).

## <a name="authentication-with-azure-active-directory-preview"></a>Autenticación con Azure Active Directory (versión preliminar)

Azure Maps ofrece ahora la integración de [Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-whatis) para la autenticación de solicitudes de servicios de Azure Maps.  Azure AD proporciona autenticación basada en identificación, como [control de acceso basado en rol (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/overview) para conceder el nivel de acceso de usuario o aplicación a recursos de Azure Maps. Este artículo tiene como fin ayudarle a entender los conceptos y componentes de la integración de Azure AD de Azure Maps.

## <a name="authentication-with-oauth-access-tokens"></a>Autenticación con tokens de acceso de OAuth

Azure Maps acepta tokens de acceso de **OAuth 2.0** para inquilinos de Azure AD asociados a suscripciones de Azure que contienen una cuenta de Azure Maps.  Azure Maps acepta tokens de:

* Usuarios de Azure AD 
* Aplicaciones de terceros que usan permisos delegados por los usuarios
* Identidades administradas de recursos de Azure

Azure Maps genera un valor de `unique identifier (client ID)` para cada cuenta de Azure Maps.  Cuando se combina el identificador de cliente con parámetros adicionales, se pueden solicitar tokens de Azure AD mediante la especificación del siguiente valor:

```
https://login.microsoftonline.com
```
Para más información sobre cómo configurar Azure AD y solicitar tokens de Azure Maps, consulte [Administración de la autenticación](https://review.docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

Para información general sobre la solicitud de tokens de Azure AD, consulte [Conceptos básicos sobre la autenticación en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/authentication-scenarios).

## <a name="requesting-azure-map-resources-with-oauth-tokens"></a>Solicitud de recursos de Azure Map con tokens de OAuth

Una vez que se adquiere un token de Azure AD, se puede enviar una solicitud a Azure Maps con los dos siguientes encabezados de solicitud requeridos establecidos:

| Encabezado de solicitud    |    Valor    |
|:------------------|:------------|
| x-ms-client-id    | 30d7cc….9f55|
| Autorización     | Bearer eyJ0e….HNIVN |

> [!Note]
> `x-ms-client-id` es el GUID basado en una cuenta de Azure Maps mostrado en la página de autenticación de Azure Maps.

A continuación se muestra un ejemplo de solicitud de ruta de Azure Maps con un token de OAuth:

```
GET /route/directions/json?api-version=1.0&query=52.50931,13.42936:52.50274,13.43872 
Host: atlas.microsoft.com 
x-ms-client-id: 30d7cc….9f55 
Authorization: Bearer eyJ0e….HNIVN 
```

Para ver el identificador de cliente, consulte [Authentication Details](https://aka.ms/amauthdetails) (Detalles de autenticación).

## <a name="control-access-with-role-based-access-control-rbac"></a>Control del acceso con RBAC (control de acceso basado en rol)

Una característica clave de Azure AD es el control del acceso a recursos protegidos mediante RBAC. Una vez que se crea la cuenta de Azure Maps y se registra la aplicación de Azure AD de Azure Maps dentro de su inquilino de Azure AD, puede configurar RBAC para un usuario, una aplicación o un recurso de Azure en la página del portal de cuentas de Azure Map. 

Azure Maps admite actualmente el control de acceso de lectura para usuarios y aplicaciones de Azure AD, o servicios de Azure individuales mediante identidades administradas para recursos de Azure.

![concepto](./media/azure-maps-authentication/concept.png)

Para ver la configuración de RBAC, consulte [How To configure RBAC for Azure Maps](https://aka.ms/amrbac) (Configuración de RBAC para Azure Maps).

## <a name="managed-identities-for-azure-resources-and-azure-maps"></a>Identidades administradas para Azure Resources y Azure Maps

Las [identidades administradas para recursos de Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) proporcionan servicios de Azure (Azure App Service, Azure Functions, Virtual Machines, etc.) con una identidad administrada automáticamente a la que se puede autorizar el acceso a servicios de Azure Maps.  

## <a name="next-steps"></a>Pasos siguientes

* Para más información sobre la autenticación de una aplicación con Azure AD y Azure Maps, consulte [Administración de la autenticación](https://review.docs.microsoft.com/azure/azure-maps/how-to-manage-authentication).

* Para más información sobre la autenticación de Azure Maps, Control de mapa y Azure AD, consulte [Azure AD y Control de mapa de Azure Maps](https://aka.ms/amaadmc).