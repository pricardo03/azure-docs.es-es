---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/30/2018
ms.author: patricka
ms.openlocfilehash: 0ab34d6234db9c13ffe82ccd0e8580217085f631
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2019
ms.locfileid: "58115629"
---
Si solo quiere habilitar el inicio de sesión en la aplicación, deberá usar un flujo de usuario de **inicio de sesión**. Este flujo de usuario describe las experiencias de los consumidores durante el inicio de sesión y el contenido de los tokens que recibirá la aplicación al realizarse el inicio de sesión correctamente.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]
En **Administrar**, seleccione **Flujos de usuario**.

Haga clic en **+Nuevo flujo de usuario** en la parte superior de la hoja.

En **Seleccionar un tipo de flujo de usuario**, seleccione **Todos** y, luego, seleccione la versión de **registro** que quiere usar.

El **nombre** determina el nombre del flujo de usuario de registro usado por la aplicación. Por ejemplo, escriba **SiIn**.

En **Proveedores de identidades**, seleccione una opción. También puede seleccionar proveedores de identidades sociales, si ya se han configurado. Haga clic en **OK**.

En **Notificaciones de la aplicación**, haga clic en **Mostrar más**.

En la columna **Notificación de devolución**, elija las notificaciones que quiere que se devuelvan en los tokens enviados de vuelta a su aplicación después de una experiencia de registro correcta. Por ejemplo, seleccione **Nombre para mostrar**, **Proveedor de identidades**, **Código Postal** e **Id. de objeto del usuario**. Haga clic en **OK**.

Haga clic en **Create**(Crear). Tenga en cuenta que el flujo de usuario que acaba de crear aparece como **B2C_1_SiIn** (el fragmento **B2C\_1\_** se agrega automáticamente).

Haga clic en **Ejecutar flujo de usuario**.

Seleccione la **aplicación Contoso B2C** en la lista desplegable **Aplicación** y `https://localhost:44321/` en la lista desplegable **URL de respuesta**.

Haga clic en **Ejecutar flujo de usuario**. Se abrirá una nueva pestaña del explorador y podrá recorrer la experiencia del consumidor de inicio de sesión en su aplicación.

> [!NOTE]
> Tiene que transcurrir hasta un minuto para que se cree el flujo de usuario y las actualizaciones surtan efecto.
>