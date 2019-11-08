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
ms.openlocfilehash: 7772b3ee5d0e27c09e83f7d118eb9f67f17e0d07
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73523791"
---
## <a name="register-your-application-with-your-ad-tenant"></a>Registro de la aplicación con el inquilino de AD
En primer lugar, tendrá que registrar la aplicación con el inquilino de Azure Active Directory (Azure AD). De este modo, se generará un id. de aplicación para la aplicación y también se habilitará esta para que reciba tokens.

1. Inicie sesión en el [Azure Portal](https://portal.azure.com).
   
1. Elija el inquilino de Azure AD; para ello, seleccione su cuenta en la esquina superior derecha de la página y, a continuación, haga clic en el menú de navegación **Cambiar directorio** y seleccione el inquilino adecuado. 
   - Omita este paso si tiene solo un inquilino de Azure AD en su cuenta o si ya ha seleccionado el inquilino de Azure AD adecuado.
   
1. En Azure Portal, busque y seleccione **Azure Active Directory**.
   
1. En el menú izquierdo de **Azure Active Directory**, seleccione **Registros de aplicaciones** y **Nuevo registro**.
   
1. Siga las indicaciones y cree una nueva aplicación. Para este tutorial, no importa si se trata de una aplicación web o un cliente público (móvil y de escritorio), pero si quiere ver ejemplos específicos de aplicaciones web o aplicaciones cliente públicas, consulte nuestras [guías de inicio rápido](../articles/active-directory/develop/v1-overview.md).
   
   - **Nombre** es el nombre de la aplicación y describe la aplicación a los usuarios finales.
   - En **Supported account types** (Tipos de cuenta compatibles), seleccione **Accounts in any organizational directory and personal Microsoft accounts** (Cuentas en cualquier directorio de organización y cuentas personales de Microsoft).
   - Proporcione el **URI de redirección**. Para aplicaciones web, esta es la URL base de la aplicación donde los usuarios pueden iniciar sesión.  Por ejemplo, `http://localhost:12345`. Para un cliente público (móvil y de escritorio), Azure AD lo usa para devolver las respuestas de token. Escriba un valor específico para la aplicación.  Por ejemplo, `http://MyFirstAADApp`.
   <!--TODO: add once App ID URI is configurable: The **App ID URI** is a unique identifier for your application. The convention is to use `https://<tenant-domain>/<app-name>`, e.g. `https://contoso.onmicrosoft.com/my-first-aad-app`-->  
   
1. Una vez que haya completado el registro, Azure AD asignará a su aplicación un identificador de cliente único (el **identificador de aplicación**). Este valor se necesita en las siguientes secciones, así que cópielo de la página de aplicación.
   
1. Para encontrar la aplicación en Azure Portal, haga clic en **Registros de aplicaciones** y, a continuación, seleccione **Ver todas las aplicaciones**.
