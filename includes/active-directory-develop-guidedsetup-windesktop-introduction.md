---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/10/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: ae17ef749a353cd60227e31ba4dadf328b1dc935
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66482289"
---
# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>Llamada a Microsoft Graph API desde una aplicación de escritorio de Windows

En esta guía se muestra cómo una aplicación nativa de Windows Desktop .NET (XAML) utiliza un token de acceso para llamar a Microsoft Graph API. La aplicación también puede acceder a otras API que requieren tokens de acceso desde una plataforma de identidad de Microsoft para desarrolladores del punto de conexión v2.0. Esta plataforma se denominaba anteriormente Azure AD.

Cuando haya completado la guía, la aplicación podrá llamar a una API protegida que usa cuentas personales (lo que incluye outlook.com, live.com y otras). La aplicación también usa cuentas profesionales y educativas de cualquier empresa y organización que utilice Azure Active Directory.  

> [!NOTE]
> Esta guía requiere Visual Studio 2015 Update 3 o Visual Studio 2019. ¿No tiene ninguna de estas versiones? [Descargue Visual Studio 2019 de manera gratuita](https://www.visualstudio.com/downloads/).

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Funcionamiento de la aplicación de ejemplo generada por esta guía

![Muestra cómo funciona la aplicación de ejemplo generada por este tutorial](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks.svg)

La aplicación de ejemplo que se crea con esta guía permite que una aplicación de escritorio de Windows consulte Microsoft Graph API o que una API web acepte tokens de un punto de conexión de una plataforma de identidad de Microsoft. En este escenario, agregará un token a las solicitudes HTTP mediante el encabezado de autorización. La Biblioteca de autenticación de Microsoft (MSAL) administra la adquisición y renovación de los tokens.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Tratamiento de la adquisición de tokens para tener acceso a API web protegidas

Una vez que el usuario se autentica, la aplicación de ejemplo recibe un token que se puede usar para consultar Microsoft Graph API o a una API web protegida mediante una plataforma de identidad de Microsoft para desarrolladores.

Las API, como Microsoft Graph, requieren un token para permitir el acceso a recursos específicos. Por ejemplo, un token es necesario para leer un perfil del usuario, acceder al calendario de un usuario o enviar correo electrónico. La aplicación puede solicitar un token de acceso mediante MSAL para acceder a estos recursos mediante la especificación de ámbitos de API. Este token de acceso se agrega luego al encabezado de autorización de HTTP para todas las llamadas efectuadas al recurso protegido.

MSAL administra el almacenamiento en caché y la actualización de los tokens de acceso automáticamente, así que no tiene que preocuparse por ello.

## <a name="nuget-packages"></a>Paquetes NuGet

Esta guía utiliza los siguientes paquetes NuGet:

|Biblioteca|DESCRIPCIÓN|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Biblioteca de autenticación de Microsoft (MSAL.NET)|
