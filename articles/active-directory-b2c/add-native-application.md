---
title: 'Adición de una aplicación cliente nativa: Azure Active Directory B2C | Microsoft Docs'
description: Obtenga información sobre cómo agregar una aplicación cliente nativa en su inquilino de Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.author: marsma
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: conceptual
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 6fc953d5c6109fbc6eacbd946ecd112db4639fa5
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71064589"
---
# <a name="add-a-native-client-application-to-your-azure-active-directory-b2c-tenant"></a>Adición de una aplicación cliente nativa en su inquilino de Azure Active Directory B2C

Los recursos del cliente nativo se deben registrar en el inquilino antes de que la aplicación pueda comunicarse con Azure Active Directory B2C.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Asegúrese de usar el directorio que contiene el inquilino de Azure AD B2C. Para ello, seleccione el filtro **Directorio y suscripción** en el menú superior y luego el directorio que contiene el inquilino.
3. Elija **Todos los servicios** en la esquina superior izquierda de Azure Portal, y busque y seleccione **Azure AD B2C**.
1. Seleccione **Aplicaciones** y **Agregar**.
2. Escriba un nombre para la aplicación. Por ejemplo, *nativeapp1*.
3. En **Incluir aplicación web o API web**, seleccione **No**.
4. En **Incluir cliente nativo**, seleccione **Sí**.
5. En **URI de redirección**, escriba un URI de redirección válido con un esquema personalizado. Hay dos aspectos importantes que se deben tener en cuenta al elegir un URI de redirección:

    - **Único**: el esquema del identificador URI de redirección debe ser único para todas las aplicaciones. En el ejemplo `com.onmicrosoft.contoso.appname://redirect/path`, `com.onmicrosoft.contoso.appname` es el esquema. Se debe seguir este patrón. Si dos aplicaciones comparten el mismo esquema, el usuario tiene opción de elegir una aplicación. Si el usuario realiza una elección incorrecta, no será posible iniciar sesión.
    - **Completo**: el URI de redirección debe tener un esquema y una ruta de acceso. La ruta de acceso debe contener al menos una barra diagonal después del dominio. Por ejemplo, `//contoso/` sirve y `//contoso` produce un error. Asegúrese de que el URI de redirección no incluya caracteres especiales, como caracteres de subrayado.

6. Haga clic en **Create**(Crear).
7. En la página de propiedades, registre el identificador de la aplicación que usará cuando configure la aplicación cliente nativa.
