---
author: mmacy
ms.service: active-directory-b2c
ms.subservice: B2C
ms.topic: include
ms.date: 10/16/2019
ms.author: marsma
ms.openlocfilehash: dc75d78f2e73d1aa42f5fc84d39a8b22d6a121bb
ms.sourcegitcommit: 359930a9387dd3d15d39abd97ad2b8cb69b8c18b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2019
ms.locfileid: "73642600"
---
Para registrar una aplicación en el inquilino de Azure AD B2C, puede usar la experiencia **Aplicaciones** actual o la nueva experiencia **Registros de aplicaciones (versión preliminar)** unificada. [Más información acerca de la nueva experiencia](https://aka.ms/b2cappregintro).

#### <a name="applicationstabapplications"></a>[Aplicaciones](#tab/applications/)

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En el menú de la izquierda, seleccione **Azure AD B2C**. O bien, seleccione **Todos los servicios** y busque y seleccione **Azure AD B2C**.
1. Seleccione **Aplicaciones** y **Agregar**.
1. Escriba un nombre para la aplicación. Por ejemplo, *testapp1*.
1. En **Incluir aplicación web o API web**, seleccione **Sí**.
1. En **Dirección URL de respuesta**, escriba `https://jwt.ms`.
1. Seleccione **Crear**.

#### <a name="app-registrations-previewtabapp-reg-preview"></a>[Registros de aplicaciones (versión preliminar)](#tab/app-reg-preview/)

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
1. Seleccione el filtro **Directorio y suscripción** en el menú superior y, luego, elija el directorio que contiene el inquilino de Azure AD B2C.
1. En el menú de la izquierda, seleccione **Azure AD B2C**. O bien, seleccione **Todos los servicios** y busque y seleccione **Azure AD B2C**.
1. Seleccione **Registros de aplicaciones (versión preliminar)** y luego **Nuevo registro**.
1. Escriba un **nombre** para la aplicación. Por ejemplo, *testapp1*.
1. Seleccione **Cuentas de cualquier directorio de la organización o cualquier proveedor de identidades**.
1. En **URI de redirección**, seleccione **Web** y escriba `https://jwt.ms` en el cuadro de texto.
1. En **Permisos**, active la casilla *Conceda permiso del administrador a los permisos openid y offline_access*.
1. Seleccione **Registrar**.

Una vez completado el registro de la aplicación, habilite el flujo de concesión implícita:

1. En **Administrar**, seleccione **Autenticación**.
1. Seleccione **Probar la nueva experiencia** (si se muestra).
1. En **Concesión implícita**, active las casillas **Tokens de acceso** y **Tokens de id.**
1. Seleccione **Guardar**.