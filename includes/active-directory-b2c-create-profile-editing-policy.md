---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
origin.date: 11/30/2018
ms.date: 04/04/2019
ms.author: v-junlch
ms.openlocfilehash: 0d9f0a24d84bd18bdf1fac84c744cc34a7d89ab3
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60456046"
---
Si desea habilitar la edición de perfiles en la aplicación, debe usar un flujo de usuario de **edición de perfiles**. Este flujo de usuario describe las experiencias de los consumidores durante la edición de perfiles y el contenido de los tokens que recibirá la aplicación al finalizarla correctamente.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

En **Administrar**, seleccione **Flujos de usuario** y haga clic en **+Nuevo flujo de usuario**.

![Selección del nuevo flujo de usuario](./media/active-directory-b2c-create-profile-editing-policy/add-b2c-new-user-flow.png)

En la pestaña **Recomendado**, seleccione **Edición de perfiles**.

Escriba un **nombre** de flujo de usuario al que la aplicación haga referencia. Por ejemplo, escriba: `SiPe`.

En **Proveedores de identidades**, active **Inicio de sesión de cuenta local**. También puede seleccionar proveedores de identidades sociales, si ya se han configurado.

![Seleccionar inicio de sesión de cuenta local como proveedor de identidades y hacer clic en el botón Aceptar](./media/active-directory-b2c-create-profile-editing-policy/add-b2c-profile-editing-identity-providers.png)

En **Atributos de usuario**, haga clic en **Mostrar más**. En la columna **Recopilar atributo**, elija los atributos que el consumidor puede ver y editar en su perfil. Por ejemplo, seleccione **País o región**, **Nombre para mostrar** y **Código postal**.

En la columna **Notificación de devolución**, elija las notificaciones que quiere que se devuelvan en los tokens de autorización enviados de vuelta a su aplicación después de una experiencia de edición de perfiles correcta. Por ejemplo, seleccione **Nombre para mostrar** y **Código postal**.

Haga clic en **OK**.

![Seleccionar algunas notificaciones de aplicación y hacer clic en el botón Aceptar](./media/active-directory-b2c-create-profile-editing-policy/add-b2c-user-attributes.png)

Haga clic en **Crear** para agregar el flujo de usuario. El flujo de usuario aparece como **B2C_1_SiPe**. El prefijo **B2C_1_** se anexa al nombre.

Seleccione **Ejecutar flujo de usuario**. Compruebe la configuración especificada en la tabla y, luego, haga clic en **Ejecutar flujo de usuario**.

![Seleccione el flujo de usuario y ejecútelo.](./media/active-directory-b2c-create-profile-editing-policy/add-b2c-profile-editing-run-user-flow.png)

| Configuración      | Value  |
| ------------ | ------ |
| **Aplicación** | Aplicación B2C de Contoso |
| **URL de respuesta** | `https://localhost:44316/` |

Se abrirá una nueva pestaña del explorador y podrá comprobar la experiencia del consumidor de edición de perfiles que configuró.

> [!NOTE]
> Tiene que transcurrir hasta un minuto para que se cree el flujo de usuario y las actualizaciones surtan efecto.
>

