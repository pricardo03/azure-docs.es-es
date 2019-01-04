---
author: PatAltimore
ms.service: active-directory-b2c
ms.topic: include
ms.date: 11/30/2018
ms.author: patricka
ms.openlocfilehash: 17c0213d63879687e9c6d5f8dca06b9113c44af8
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2018
ms.locfileid: "52742486"
---
Si solo quiere habilitar el registro en su aplicación, use un flujo de usuario de **registro**. Este flujo de usuario describe las experiencias de los clientes durante el registro y el contenido de los tokens que recibe la aplicación al completarse la operación correctamente.

[!INCLUDE [active-directory-b2c-portal-navigate-b2c-service](active-directory-b2c-portal-navigate-b2c-service.md)]

En **Administrar**, seleccione **Flujos de usuario**.

Haga clic en **+Nuevo flujo de usuario** en la parte superior de la hoja.

En **Seleccionar un tipo de flujo de usuario**, seleccione **Todos** y, luego, seleccione la versión de **registro** que quiere usar.

El **nombre** determina el nombre del flujo de usuario de registro usado por la aplicación. Por ejemplo, escriba **SiUp**.

En **Proveedores de identidades**, seleccione **Registro por correo electrónico**. También puede seleccionar proveedores de identidades sociales, si ya se han configurado.

En **Atributos y notificaciones de usuario**, haga clic en **Mostrar más**.

En la columna **Recopilar atributo**, elija los atributos que quiere recopilar del consumidor durante el registro. Por ejemplo, seleccione **País o región**, **Nombre para mostrar** y **Código postal**.

En la columna **Notificación de devolución**, elija las notificaciones que quiere que se devuelvan en los tokens enviados de vuelta a su aplicación después de una experiencia de registro correcta. Por ejemplo, seleccione **Nombre para mostrar**, **Proveedor de identidades**, **Código postal**, **El usuario es nuevo** e **Id. de objeto del usuario**.

Haga clic en **OK**.

Haga clic en **Create**(Crear). El flujo de usuario creado aparece como **B2C_1_SiUp** (el fragmento **B2C\_1\_** se agrega automáticamente).

Haga clic en **Ejecutar flujo de usuario**.

Seleccione la **aplicación Contoso B2C** en la lista desplegable **Aplicación** y `https://localhost:44321/` en la lista desplegable **URL de respuesta**.

Haga clic en **Ejecutar flujo de usuario**. Se abrirá una nueva pestaña del explorador y podrá recorrer la experiencia del consumidor de registro en su aplicación.

> [!NOTE]
> Tiene que transcurrir hasta un minuto para que se cree el flujo de usuario y las actualizaciones surtan efecto.
>