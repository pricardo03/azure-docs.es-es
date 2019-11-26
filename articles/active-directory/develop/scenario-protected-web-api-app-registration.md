---
title: 'API web protegida: registro de aplicaciones'
titleSuffix: Microsoft identity platform
description: Obtenga información sobre cómo crear una API web protegida y la información que necesita para registrar la aplicación.
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0c905dfd86fd80b9e55aa7bd5a9b9b03f277570c
ms.sourcegitcommit: be8e2e0a3eb2ad49ed5b996461d4bff7cba8a837
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/23/2019
ms.locfileid: "72802548"
---
# <a name="protected-web-api-app-registration"></a>API web protegida: Registro de aplicación

En este artículo se explican los detalles específicos de registro de la aplicación para una API web protegida.

Consulte [Quickstart: Registro de una aplicación en la plataforma de identidad de Microsoft](quickstart-register-app.md) para obtener información sobre los pasos comunes para registrar una aplicación.

## <a name="accepted-token-version"></a>Versión del token aceptada

El punto de conexión de la plataforma de identidad de Microsoft puede emitir dos tipos de tokens: v1.0 y v2.0. Para obtener más información sobre estos tokens, consulte [Tokens de acceso](access-tokens.md). La versión del token aceptada depende de los **Tipos de cuentas admitidos** que eligió al crear la aplicación:

- Si el valor de **tipos de cuentas admitidos** es **Accounts in any organizational directory and personal Microsoft accounts** (Cuentas en cualquier directorio organizativo y cuentas Microsoft personales) como, por ejemplo, Skype, Xbox y Outlook.com, la versión del token será v2.0.
- En caso contrario, la versión del token aceptada será v1.0.

Después de crear la aplicación, puede determinar o cambiar la versión del token aceptada siguiendo estos pasos:

1. En Azure Portal, seleccione la aplicación y, a continuación, seleccione el **Manifiesto** para la aplicación.
2. En el manifiesto, busque **"accessTokenAcceptedVersion"** . Observe que su valor es **2**. Esta propiedad especifica a Azure Active Directory (Azure AD) que la API web acepta tokens v2.0. Si el valor es **null**, la versión del token aceptada es v1.0.
3. Si ha cambiado la versión de token, seleccione **Guardar**.

> [!NOTE]
> La API web especifica qué versión del token (v1.0 o v2.0) acepta. Cuando los clientes solicitan un token para la API web desde el punto de conexión de la plataforma de identidad de Microsoft (v2.0), reciben un token que indica qué versión acepta la API web.

## <a name="no-redirect-uri"></a>URI sin redireccionamiento

Las API web no necesitan registrar ningún URI de redireccionamiento porque no hay ningún usuario con la sesión iniciada de forma interactiva.

## <a name="expose-an-api"></a>Exponer una API

Otra configuración específica para las API web es la API expuesta y los ámbitos expuestos.

### <a name="resource-uri-and-scopes"></a>Ámbitos y URI de recursos

Normalmente, los ámbitos tienen el formato `resourceURI/scopeName`. Para Microsoft Graph, los ámbitos tienen accesos directos, como `User.Read`. Esta cadena es un acceso directo para `https://graph.microsoft.com/user.read`.

Durante el registro de la aplicación, debe definir estos parámetros:

- URI del recurso. De forma predeterminada, el portal de registro de aplicación recomienda utilizar `api://{clientId}`. Este URI de recurso es único, pero los humanos no lo pueden leer. Puede cambiarlo, pero asegúrese de que el nuevo valor sea único.
- Uno o varios *ámbitos*. (Para las aplicaciones cliente, se mostrarán como *permisos delegados* para su API web).
- Uno o varios *roles de aplicación*. (Para las aplicaciones cliente, se mostrarán como *permisos de aplicación* para su API web).

Los ámbitos también se muestran en la pantalla de consentimiento que se presenta a los usuarios finales de la aplicación. Por lo que deberá proporcionar las cadenas correspondientes que describen el ámbito:

- Como las ve el usuario final.
- Como las ve el administrador del inquilino, que puede otorgar el consentimiento del administrador.

### <a name="exposing-delegated-permissions-scopes"></a>Exponer permisos delegados (ámbitos)

1. Seleccione la sección **Exponer una API** en el registro de aplicación.
1. Seleccione **Agregar un ámbito**.
1. Si se le pide, acepte el URI de identificador de aplicación propuesto (`api://{clientId}`) seleccionando **Guardar y continuar**.
1. Especifique estos parámetros:
      - Para **Nombre de ámbito**, use **access_as_user**.
      - Para **¿Quién puede dar el consentimiento?** , asegúrese de que esté seleccionada la opción **Administradores y usuarios**.
      - En **Nombre para mostrar del consentimiento del administrador**, escriba **Access TodoListService as a user**.
      - En **Descripción del consentimiento del administrador**, escriba **Accesses the TodoListService Web API as a user**.
      - En **Nombre para mostrar del consentimiento del usuario** , escriba **Access TodoListService as a user**.
      - En **Descripción del consentimiento del administrador**, escriba **Accesses the TodoListService Web API as a user**.
      - Deje el **Estado** establecido en **Habilitado**.
      - Seleccione **Agregar ámbito**.

### <a name="if-your-web-api-is-called-by-a-daemon-app"></a>Si una aplicación de demonio llama a la API web

En esta sección, aprenderá a registrar su API web protegida para que las aplicaciones de demonio puedan invocarla de forma segura.

- Deberá exponer los *permisos de aplicación*. Solo declarará permisos de aplicación porque las aplicaciones de demonio no interactúan con los usuarios, de modo que los permisos delegados no tendrían sentido.
- Los administradores de inquilinos pueden requerir que Azure Active Directory (Azure AD) emita tokens para la API web solo para las aplicaciones que se han registrado para tener acceso a uno de los permisos de aplicación de la API web.

#### <a name="exposing-application-permissions-app-roles"></a>Exposición de permisos de aplicación (roles de aplicación)

Para exponer permisos de aplicación, debe editar el manifiesto.

1. En el registro de aplicación para la aplicación, seleccione **Manifiesto**.
1. Localice la opción de configuración `appRoles` y agregue uno o varios roles de aplicación para editar el manifiesto. En el siguiente bloque de JSON de ejemplo se proporciona la definición del rol. Deje `allowedMemberTypes` establecido solo en `"Application"`. Asegúrese de que `id` es un GUID único y de que `displayName` y `value` no contienen espacios.
1. Guarde el manifiesto.

En el ejemplo siguiente se muestra el contenido de `appRoles`. (`id` puede ser cualquier GUID único).

```JSon
"appRoles": [
    {
    "allowedMemberTypes": [ "Application" ],
    "description": "Accesses the TodoListService-Cert as an application.",
    "displayName": "access_as_application",
    "id": "ccf784a6-fd0c-45f2-9c08-2f9d162a0628",
    "isEnabled": true,
    "lang": null,
    "origin": "Application",
    "value": "access_as_application"
    }
],
```

#### <a name="ensuring-that-azure-ad-issues-tokens-for-your-web-api-to-only-allowed-clients"></a>Asegurarse de que Azure AD emite tokens para la API web solo para los clientes permitidos

La API web comprueba el rol de aplicación. Es la manera que tiene el desarrollador de exponer los permisos de aplicación. Pero también puede configurar Azure AD para emitir un token para la API web solo para las aplicaciones que ha aprobado el administrador de inquilinos para que accedan a la API. Para agregar esta seguridad mejorada:

1. En la página **Introducción** de la aplicación para el registro de la aplicación, seleccione el vínculo con el nombre de la aplicación en **Aplicación administrada en directorio local**. Es posible que el título de este campo se muestre truncado. Por ejemplo, puede que vea **Aplicación administrada en...**

   > [!NOTE]
   >
   > Al seleccionar este vínculo, se le dirigirá a la página de **Introducción a la aplicación empresarial** asociada con la entidad de servicio para la aplicación en el inquilino donde la creó. Para volver a la página de registro de la aplicación, use el botón Atrás del explorador.

1. Seleccione la página **Propiedades** de la sección **Administrar** de las páginas de la aplicación empresarial.
1. Para que Azure AD permita el acceso a la API web solo desde determinados clientes, establezca **¿Asignación de usuarios?** en **Sí**.

   > [!IMPORTANT]
   >
   > Si establece la opción **¿Asignación de usuarios?**  en **Sí**, Azure AD comprobará las asignaciones de roles de aplicación de los clientes cuando soliciten un token de acceso para la API web. Si el cliente no está asignado a ningún rol de aplicación, Azure AD devolverá el error `invalid_client: AADSTS501051: Application <application name> is not assigned to a role for the <web API>`.
   >
   > Si define **¿Asignación de usuarios?** como **No**, *Azure AD no comprobará las asignaciones de rol de la aplicación cuando un cliente solicite un token de acceso para la API web*. Cualquier cliente de demonio (es decir, cualquier cliente que use el flujo de credenciales de cliente) podrá obtener un token de acceso para la API, simplemente, especificando su público. Cualquier aplicación podrá acceder a la API sin tener que solicitar permiso. Pero la API web siempre puede, como se explica en la sección anterior, comprobar que la aplicación tiene el rol correcto (autorizado por el administrador de inquilinos). Para realizar esta verificación, la API valida que el token de acceso tiene una notificación de roles con un valor correcto. (En nuestro caso, el valor es `access_as_application`).

1. Seleccione **Guardar**.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Configuración del código de la aplicación](scenario-protected-web-api-app-configuration.md)
