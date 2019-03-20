---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/30/2018
ms.author: patricka
ms.openlocfilehash: f23d2b02bc2a23c5333a48a50532c03f3aa6a031
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2019
ms.locfileid: "58115425"
---
[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

En **Administrar**, seleccione **Flujos de usuario** y haga clic en **+Nuevo flujo de usuario**.

![Selección del nuevo flujo de usuario](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-user-flow.png)

En la pestaña **Recomendado**, haga clic en **Sign up and sign in** (Registrarse e iniciar sesión).

![Selección del flujo de usuario de registro e inicio de sesión](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-user-flow-type.png)

Escriba un **nombre** de flujo de usuario al que la aplicación haga referencia. Por ejemplo, escriba: `SiUpIn`.

Seleccione **Proveedores de identidades** y active **Registro por correo electrónico**. También puede seleccionar proveedores de identidades sociales, si ya se han configurado.

En **Autenticación multifactor**, elija **Habilitado** o **Deshabilitado**.

![Escribir un nombre y seleccionar Registro por correo electrónico como proveedor de identidades](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-name-identity-providers.png)

En **Atributos y notificaciones de usuario**, seleccione **Mostrar más** para ver la lista completa de atributos y notificaciones de los que puede elegir.

En la columna **Recopilar atributo**, elija los atributos que quiere recopilar del cliente durante el registro. Por ejemplo, seleccione **País o región**, **Nombre para mostrar** y **Código postal**.

En la columna **Notificación de devolución**, elija las notificaciones que quiere que se devuelvan en los tokens de autorización enviados de vuelta a su aplicación después de una experiencia correcta de registro o inicio de sesión. Por ejemplo, seleccione **Nombre para mostrar**, **Proveedor de identidades**, **Código postal**, **El usuario es nuevo** e **Id. de objeto del usuario**.

Haga clic en **OK**.

![Seleccionar algunos atributos de usuario y notificaciones y hacer clic en el botón Aceptar](media/active-directory-b2c-create-sign-in-sign-up-policy/add-b2c-signup-signin-sign-up-all-attributes.png)

Haga clic en **Crear** para agregar el flujo de usuario. El flujo de usuario aparece como **B2C_1_SiUpIn**. El prefijo **B2C_1_** se anexa al nombre.

Seleccione **Ejecutar flujo de usuario**. Compruebe la configuración especificada en la tabla y, luego, haga clic en **Ejecutar flujo de usuario**.

![Seleccionar Ejecutar flujo de usuario](media/active-directory-b2c-create-sign-in-sign-up-policy/run-user-flow-b2c-signup-signin.png)

| Configuración      | Valor  |
| ------------ | ------ |
| **Aplicación** | Aplicación B2C de Contoso |
| **URL de respuesta** | `https://localhost:44316/` |

Se abrirá una nueva pestaña del explorador y podrá comprobar la experiencia del consumidor de registro o de inicio de sesión tal como se configuró.

> [!NOTE]
> Tiene que transcurrir hasta un minuto para que se cree el flujo de usuario y las actualizaciones surtan efecto.
>