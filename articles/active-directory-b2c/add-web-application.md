---
title: 'Adición de una aplicación web: Azure Active Directory B2C | Microsoft Docs'
description: Obtenga información sobre cómo agregar una aplicación web en su inquilino de Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 04/16/2019
ms.custom: mvc
ms.topic: conceptual
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: a1d098550c6fb733e088f8ad211d29f48f55d2d6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66511698"
---
# <a name="add-a-web-api-application-to-your-azure-active-directory-b2c-tenant"></a>Adición de una aplicación de API web al inquilino de Azure Active Directory B2C

 Registre recursos de API web en el inquilino para que puedan aceptar y responder a solicitudes de aplicaciones cliente que presenten un token de acceso. En este artículo se muestra cómo registrar una aplicación en Azure Active Directory (Azure AD) B2C.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Seleccione el **filtro de directorio y suscripciones** del menú superior y elija el directorio que contiene la suscripción.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
4. Seleccione **Aplicaciones** y **Agregar**.
5. Escriba un nombre para la aplicación. Por ejemplo, *webapi1*.
6. En **Incluir aplicación web o API web** y **Permitir flujo implícito**, seleccione **Sí**.
7. En **Dirección URL de respuesta**, escriba un punto de conexión donde Azure AD B2C devolverá los tokens que solicite la aplicación. En la aplicación de producción, puede establecer la dirección URL de respuesta en un valor como `https://localhost:44332`. Para fines de prueba, establezca la dirección URL de respuesta en `https://jwt.ms`.
8. En **URI de id. de aplicación**, escriba el identificador usado para la API web. Se genera el identificador URI completo, incluido el dominio. Por ejemplo, `https://contosotenant.onmicrosoft.com/api`.
9. Haga clic en **Create**(Crear).
10. En la página de propiedades, registre el identificador de la aplicación que usará cuando configure la aplicación web.

## <a name="configure-scopes"></a>Configuración de ámbitos

Los ámbitos proporcionan una manera de controlar el acceso a los recursos protegidos. La API web utiliza los ámbitos para implementar el control de acceso basado en el ámbito. Por ejemplo, los usuarios de la API web pueden tener ambos accesos de lectura y de escritura, o pueden tener solo acceso de lectura. En este tutorial, utilizará ámbitos para definir los permisos de lectura y escritura para la API web.

1. Seleccione **Aplicaciones** y, a continuación, *webapi1*.
2. Seleccione **Ámbitos publicados**.
3. Como **ámbito**, escriba `Read` y para la descripción, escriba `Read access to the application`.
4. Como **ámbito**, escriba `Write` y para la descripción, escriba `Write access to the application`.
5. Haga clic en **Save**(Guardar).

Los ámbitos publicados se pueden utilizar para conceder a una aplicación cliente permiso para la API web.

## <a name="grant-permissions"></a>Concesión de permisos

Para llamar a una API web protegida desde una aplicación, deberá conceder permisos de aplicación a la API. Por ejemplo, en [Tutorial: registro de una aplicación en Azure Active Directory B2C](tutorial-register-applications.md), se crea una aplicación web en Azure AD B2C denominada *webapp1*. Puede usar esta aplicación para llamar a la API web.

1. Seleccione **Aplicaciones** y, a continuación, seleccione su aplicación web.
2. Seleccione **Acceso de API** y, a continuación, seleccione **Agregar**.
3. En el menú desplegable **Seleccionar API**, seleccione *webapi1*.
4. En el menú desplegable **Seleccionar ámbitos**, seleccione los ámbitos **Read** y **Write** que definió previamente.
5. Haga clic en **OK**.

La aplicación está registrada para llamar a la API web protegida. Un usuario se autentica con Azure AD B2C para utilizar la aplicación. La aplicación obtiene una concesión de autorización de Azure AD B2C para acceder a la API web protegida.
