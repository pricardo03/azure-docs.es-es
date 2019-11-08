---
title: Adición de roles de aplicación en la aplicación registrada en Azure Active Directory y recepción en el token
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo agregar roles de aplicación en una aplicación registrada en Azure Active Directory, asignar usuarios y grupos a estos roles y recibirlos en la notificación `roles` del token.
services: active-directory
documentationcenter: ''
author: kkrishna
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: kkrishna
ms.reviewer: ''
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2c838c6fe256e2960842c5b40f58f8a5d5b1fdf3
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73473772"
---
# <a name="how-to-add-app-roles-in-your-application-and-receive-them-in-the-token"></a>Procedimientos para: Agregar roles de aplicación en la aplicación y recibirlos en el token

El control de acceso basado en rol (RBAC) es un mecanismo popular para exigir la autorización en las aplicaciones. Cuando se usa RBAC, un administrador concede permisos a roles y no a usuarios individuales o grupos. El administrador, a continuación, puede asignar roles a diferentes usuarios y grupos para controlar quién tiene acceso a qué contenido y funcionalidad.

Mediante el uso de RBAC con roles de aplicación y notificaciones de rol, los desarrolladores pueden exigir de forma segura la autorización en las aplicaciones con poco esfuerzo por su parte.

Otro enfoque consiste en usar grupos de Azure AD y notificaciones de grupo, como se muestra en el ejemplo [WebApp-GroupClaims-DotNet](https://github.com/Azure-Samples/WebApp-GroupClaims-DotNet). Los grupos de Azure AD y los roles de aplicación no son mutuamente excluyentes, se pueden usar conjuntamente para proporcionar un control de acceso aún más preciso.

## <a name="declare-roles-for-an-application"></a>Declaración de roles para una aplicación

Estos roles de aplicación se definen en [Azure Portal](https://portal.azure.com) en el manifiesto de registro de la aplicación.  Cuando un usuario inicia sesión en la aplicación, Azure AD emite una notificación `roles` por cada rol que se ha concedido al usuario individualmente y por su pertenencia a grupos.  La asignación de usuarios y grupos a los roles se puede realizar mediante la interfaz de usuario del portal o mediante programación con [Microsoft Graph](https://developer.microsoft.com/graph/docs/concepts/azuread-identity-access-management-concept-overview).

### <a name="declare-app-roles-using-azure-portal"></a>Declaración de roles de aplicación mediante Azure Portal

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. En la barra superior, seleccione la cuenta y, a continuación, seleccione **Cambiar directorio**.
1. Una vez que se abre el panel **Directorio y suscripción**, elija el inquilino de Active Directory en el que desea registrar la aplicación desde la lista **Favoritos** o **Todos los directorios**.
1. Seleccione **Todos los servicios** en el panel de navegación izquierdo y elija **Azure Active Directory**.
1. En el panel **Azure Active Directory**, seleccione **Registros de aplicaciones (característica heredada)** para ver una lista de todas las aplicaciones.

     Si no ve la aplicación que desea, utilice los distintos filtros de la parte superior de la lista **Registros de aplicaciones (característica heredada)** para restringir la lista o desplácese hacia abajo en la lista para buscar la aplicación.

1. Seleccione la aplicación para la que desea definir roles de aplicación.
1. En la hoja de la aplicación, seleccione **Manifiesto**.
1. Localice la opción de configuración `appRoles` para editar el manifiesto de la aplicación y agregar los roles de aplicación.

     > [!NOTE]
     > Cada definición de roles de aplicación de este manifiesto debe tener un GUID válido diferente para la propiedad `id`. 
     > 
     > La propiedad `value` de cada definición de rol de la aplicación debe coincidir exactamente con las cadenas que se usan en el código de la aplicación. La propiedad `value` no puede contener espacios. Si es así, recibirá un error al guardar el manifiesto.
     
1. Guarde el manifiesto.

### <a name="examples"></a>Ejemplos

El ejemplo siguiente muestra los distintos `appRoles` que se pueden asignar a `users`.

> [!NOTE]
>El elemento `id` debe ser un GUID único.

```Json
"appId": "8763f1c4-f988-489c-a51e-158e9ef97d6a",
"appRoles": [
    {
      "allowedMemberTypes": [
        "User"
      ],
      "displayName": "Writer",
      "id": "d1c2ade8-98f8-45fd-aa4a-6d06b947c66f",
      "isEnabled": true,
      "description": "Writers Have the ability to create tasks.",
      "value": "Writer"
    }
  ],
"availableToOtherTenants": false,
```

> [!NOTE]
>`displayName` no puede contener espacios.

Puede definir roles de aplicación para `users`, `applications` o ambos. Cuando están disponibles para `applications`, los roles de aplicación aparecen como permisos de aplicación en la hoja **Permisos necesarios**. El ejemplo siguiente muestra un rol de aplicación dirigido a un elemento `Application`.

```Json
"appId": "8763f1c4-f988-489c-a51e-158e9ef97d6a",
"appRoles": [
    {
      "allowedMemberTypes": [
        "Application"
      ],
      "displayName": "ConsumerApps",
      "id": "47fbb575-859a-4941-89c9-0f7a6c30beac",
      "isEnabled": true,
      "description": "Consumer apps have access to the consumer data.",
      "value": "Consumer"
    }
  ],
"availableToOtherTenants": false,
```

El número de roles definidos afecta a los límites que tiene el manifiesto de aplicación. Se describen detalladamente en la página de [límites del manifiesto](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest#manifest-limits).

### <a name="assign-users-and-groups-to-roles"></a>Asignación de usuarios y grupos a roles

Una vez que haya agregado los roles de aplicación en la aplicación, puede asignar usuarios y grupos a estos roles.

1. En el panel **Azure Active Directory**, seleccione **Aplicaciones empresariales** en el menú de navegación izquierdo **Azure Active Directory**.
1. Seleccione **Todas las aplicaciones** para ver una lista de todas las aplicaciones.

     Si no ve la aplicación que desea, utilice los distintos filtros de la parte superior de la lista **Registros de aplicaciones** para restringir la lista o desplácese hacia abajo en la lista para buscar la aplicación.

1. Seleccione la aplicación en la que desea asignar usuarios o grupos de seguridad a los roles.
1. Seleccione el panel **Usuarios y grupos** en el menú de navegación izquierdo de la aplicación.
1. En la parte superior de la lista **Usuarios y grupos**, seleccione el botón **Agregar usuario** para abrir el panel **Agregar asignación**.
1. Seleccione el selector **Usuarios y grupos** del panel **Agregar asignación**.

     Se mostrará una lista de usuarios y grupos de seguridad junto con un cuadro de texto para buscar un usuario o grupo determinado. Esta pantalla permite seleccionar varios usuarios y grupos en una sola operación.

1. Cuando haya terminado la selección de los usuarios y grupos, presione el botón **Seleccionar** en la parte inferior para desplazarse al apartado siguiente.
1. Elija el selector **Seleccionar rol** del panel **Agregar asignación**. Se mostrarán todos los roles que se declararon previamente en el manifiesto de la aplicación.
1. Elija un rol y presione el botón **Seleccionar**.
1. Presione el botón **Asignar** en la parte inferior para finalizar las asignaciones de usuarios y grupos a la aplicación.
1. Confirme que los usuarios y grupos que agregó se muestran en la lista **Usuarios y grupos** actualizada.

## <a name="more-information"></a>Más información

- [Autorización en una aplicación web con roles de aplicación de Azure AD &amp; notificaciones de rol (ejemplo)](https://github.com/Azure-Samples/active-directory-dotnet-webapp-roleclaims)
- [Uso de grupos de seguridad y roles de aplicación en las aplicaciones (vídeo)](https://www.youtube.com/watch?v=V8VUPixLSiM)
- [Azure Active Directory, ahora con notificaciones de grupo y roles de aplicación](https://cloudblogs.microsoft.com/enterprisemobility/2014/12/18/azure-active-directory-now-with-group-claims-and-application-roles)
- [Manifiesto de la aplicación de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/reference-app-manifest)
- [Tokens de acceso de AAD](access-tokens.md)
- [`id_tokens` de AAD](id-tokens.md)
