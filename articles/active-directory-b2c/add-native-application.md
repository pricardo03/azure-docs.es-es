---
title: 'Adición de una aplicación cliente nativa: Azure Active Directory B2C | Microsoft Docs'
description: Obtenga información sobre cómo agregar una aplicación cliente nativa en su inquilino de Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.author: davidmu
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: conceptual
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 772b6b8a4d71b7d1c6418651ee0a4ee7b03af0cc
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64703912"
---
# <a name="add-a-native-client-application-to-your-azure-active-directory-b2c-tenant"></a>Adición de una aplicación cliente nativa en su inquilino de Azure Active Directory B2C

Los recursos del cliente nativo se deben registrar en el inquilino antes de que la aplicación pueda comunicarse con Azure Active Directory B2C.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
2. Asegúrese de que usa el directorio que contiene el inquilino de Azure AD B2C. Para ello, haga clic en el **filtro de directorio y suscripción** en el menú superior y elija el directorio que contiene el inquilino.
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
