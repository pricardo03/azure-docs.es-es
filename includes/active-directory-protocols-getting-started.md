---
title: Introducción al protocolo .NET de Azure AD | Microsoft Docs
description: Descubra cómo utilizar mensajes HTTP para autorizar el acceso a aplicaciones y API web en su inquilino de Azure AD.
services: active-directory
documentationcenter: .net
author: priyamohanram
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 05/22/2019
ms.author: priyamo
ms.openlocfilehash: b6dd4cd55755ae2c92afd327ad72ffe6966b9a07
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/18/2019
ms.locfileid: "67186488"
---
## <a name="register-your-application-with-your-ad-tenant"></a>Registro de la aplicación con el inquilino de AD
En primer lugar, tendrá que registrar la aplicación con el inquilino de Azure Active Directory (Azure AD). De este modo, se generará un id. de aplicación para la aplicación y también se habilitará esta para que reciba tokens.

* Inicie sesión en el [Azure Portal](https://portal.azure.com).
* Elija el inquilino de Azure AD, para lo que debe hacer clic en su cuenta en la esquina superior derecha de la página y, después, haga clic en el menú de navegación **Cambiar directorio** y seleccione el inquilino adecuado. 
  * Omita este paso si tiene solo un inquilino de Azure AD en su cuenta o si ya ha seleccionado el inquilino de Azure AD adecuado.
* En el panel de navegación izquierdo, haga clic en **Azure Active Directory**.
* Haga clic en **Registros de aplicaciones** y elija **Nuevo registro**.
* Siga las indicaciones y cree una nueva aplicación. Para este tutorial, no importa si se trata de una aplicación web o un cliente público (móvil y de escritorio), pero si quiere ver ejemplos específicos de aplicaciones web o aplicaciones cliente públicas, consulte nuestras [guías de inicio rápido](../articles/active-directory/develop/v1-overview.md).
  * **Nombre** es el nombre de la aplicación y describe la aplicación a los usuarios finales.
  * En **Supported account types** (Tipos de cuenta compatibles), seleccione **Accounts in any organizational directory and personal Microsoft accounts** (Cuentas en cualquier directorio de organización y cuentas personales de Microsoft).
  * Proporcione el **URI de redirección**. Para aplicaciones web, esta es la URL base de la aplicación donde los usuarios pueden iniciar sesión.  Por ejemplo, `http://localhost:12345`. Para un cliente público (móvil y de escritorio), Azure AD lo usa para devolver las respuestas de token. Escriba un valor específico para la aplicación.  Por ejemplo, `http://MyFirstAADApp`.
    <!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->  
* Una vez que haya completado el registro, Azure AD asignará a su aplicación un identificador de cliente único (el **identificador de aplicación**). Este valor se necesita en las siguientes secciones, así que cópielo de la página de aplicación.
* Para encontrar la aplicación en Azure Portal, haga clic en **Registros de aplicaciones** y, a continuación, haga clic en **Ver todas las aplicaciones**.
