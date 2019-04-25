---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
origin.date: 11/30/2018
ms.date: 04/04/2019
ms.author: v-junlch
ms.openlocfilehash: 78abb190dccd27c5bf70dfe12f978e1118601815
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60456953"
---
Para habilitar en la aplicación el restablecimiento de contraseña específica se usa un flujo de usuario de **restablecimiento de contraseña**. Tenga en cuenta que la opción de restablecimiento de contraseña de todo un inquilino se especifica [aquí](../articles/active-directory-b2c/active-directory-b2c-reference-sspr.md). Este flujo de usuario describe las experiencias de los consumidores durante el restablecimiento de contraseña y el contenido de los tokens que recibirá la aplicación al finalizarlo correctamente.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

En **Administrar**, seleccione **Flujos de usuario** y haga clic en **+Nuevo flujo de usuario**.

![Selección del nuevo flujo de usuario](./media/active-directory-b2c-create-password-reset-policy/add-b2c-new-user-flow.png)

En la pestaña **Recomendado**, seleccione **Restablecimiento de contraseña**.

Escriba un **nombre** de flujo de usuario al que la aplicación haga referencia. Por ejemplo, escriba: `SSPR`.

Seleccione **Proveedores de identidades** y active la opción **Reset password using email address** (Restablecer contraseña mediante la dirección de correo electrónico).

![Escribir el nombre y seleccionar la opción para restablecer la contraseña mediante la dirección de correo electrónico como proveedor de identidades](./media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-identity-providers.png)

En **Notificaciones de la aplicación**, haga clic en **Mostrar más** y elija las notificaciones que quiere que se devuelvan en los tokens de autorización enviados de vuelta a su aplicación después de una experiencia correcta de restablecimiento de contraseña. Por ejemplo, seleccione **Id. de objeto del usuario**.

Haga clic en **OK**.

![Seleccionar algunas notificaciones de aplicación y hacer clic en el botón Aceptar](./media/active-directory-b2c-create-password-reset-policy/add-b2c-password-reset-application-claims.png)

Haga clic en **Crear** para agregar el flujo de usuario. El flujo de usuario aparece como **B2C_1_SSPR**. El prefijo **B2C_1_** se anexa al nombre.

Haga clic en **Ejecutar flujo de usuario**. Compruebe la configuración especificada en la tabla y, luego, haga clic en **Ejecutar flujo de usuario**.

![Seleccione el flujo de usuario y ejecútelo.](./media/active-directory-b2c-create-password-reset-policy/add-b2c-sspr-run-user-flow.png)

| Configuración      | Value  |
| ------------ | ------ |
| **Aplicación** | Aplicación B2C de Contoso |
| **Seleccionar dirección URL de respuesta** | `https://localhost:44316/` |

Se abrirá una nueva pestaña del explorador y podrá comprobar la experiencia del usuario de restablecimiento de contraseña en su aplicación.

> [!NOTE]
> Se tarda hasta un minuto en que la creación de directivas y las actualizaciones surtan efecto.
>

