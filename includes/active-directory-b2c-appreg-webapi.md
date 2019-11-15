---
author: mmacy
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: marsma
ms.openlocfilehash: a286f8d401ec98a5069c07b1a82acdf4af2b8185
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73641833"
---
Los recursos de API web tienen que registrarse en el inquilino antes de que puedan aceptar y responder a solicitudes de recursos protegidos por aplicaciones cliente que presenten un token de acceso.

Para registrar una aplicación en el inquilino de Azure AD B2C, puede usar la experiencia **Aplicaciones** actual o la nueva experiencia **Registros de aplicaciones (versión preliminar)** unificada. [Más información acerca de la nueva experiencia](https://aka.ms/b2cappregintro).

#### <a name="applicationstabapplications"></a>[Aplicaciones](#tab/applications/)

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En el menú de la izquierda, seleccione **Azure AD B2C**. O bien, seleccione **Todos los servicios** y busque y seleccione **Azure AD B2C**.
1. Seleccione **Aplicaciones** y **Agregar**.
1. Escriba un nombre para la aplicación. Por ejemplo, *webapi1*.
1. En **Aplicación web/API web**, seleccione **Sí**.
1. En **Permitir flujo implícito**, seleccione **Sí**.
1. En **Dirección URL de respuesta**, escriba un punto de conexión donde Azure AD B2C devolverá los tokens que solicite la aplicación. En este tutorial, el ejemplo se ejecuta localmente y escucha en el puerto `https://localhost:5000`.
1. En **URI de id. de aplicación**, agregue un identificador de punto de conexión de API para el identificador URI que se muestra. Para este tutorial, escriba `api`, con el fin de que el identificador URI completo sea similar a `https://contosob2c.onmicrosoft.com/api`.
1. Seleccione **Crear**.
1. Anote el valor de **ID. DE APLICACIÓN** para usarlo en un paso posterior.

#### <a name="app-registrations-previewtabapp-reg-preview"></a>[Registros de aplicaciones (versión preliminar)](#tab/app-reg-preview/)

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En el menú de la izquierda, seleccione **Azure AD B2C**. O bien, seleccione **Todos los servicios** y busque y seleccione **Azure AD B2C**.
1. Seleccione **Registros de aplicaciones (versión preliminar)** y luego **Nuevo registro**.
1. Escriba un **Nombre** para la aplicación. Por ejemplo, *webapi1*.
1. En **URI de redirección**, seleccione **Web** y escriba el punto de conexión donde Azure AD B2C deberá devolver los tokens que solicite la aplicación. En este tutorial, el ejemplo se ejecuta localmente y escucha en el puerto `http://localhost:5000`.
1. Seleccione **Registrar**.
1. Anote el **Id. de aplicación (cliente)** para usarlo en un paso posterior.

A continuación, habilite el flujo de concesión implícita:

1. En **Administrar**, seleccione **Autenticación**.
1. Seleccione **Probar la nueva experiencia** (si se muestra).
1. En **Concesión implícita**, active las casillas **Tokens de acceso** y **Tokens de id.**
1. Seleccione **Guardar**.