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
ms.openlocfilehash: f0cc888eaf3724737e9c868c69a641094a19348c
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/23/2019
ms.locfileid: "66121595"
---
# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>Llamada a Microsoft Graph API desde una aplicación de escritorio de Windows

En esta guía se muestra cómo una aplicación nativa .NET de escritorio de Windows (XAML) puede obtener un token de acceso y llamar a Microsoft Graph API o a otras API que requieran tokens de acceso desde el punto de conexión de una plataforma de identidad de Microsoft para desarrolladores (lo que antes se conocía como Azure AD v2.0).

Cuando haya completado la guía, la aplicación podrá llamar a una API protegida que usa cuentas personales (lo que incluye outlook.com, live.com y otras). La aplicación también usa cuentas profesionales y educativas de cualquier empresa y organización que utilice Azure Active Directory.  

> [!NOTE]
> Esta guía requiere Visual Studio 2015 Update 3 o Visual Studio 2017. ¿No tiene ninguna de estas versiones? [Descargue Visual Studio 2017 de manera gratuita](https://www.visualstudio.com/downloads/).

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Funcionamiento de la aplicación de ejemplo generada por esta guía

![Muestra cómo funciona la aplicación de ejemplo generada por este tutorial](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks.svg)

La aplicación de ejemplo que se crea con esta guía permite que una aplicación de escritorio de Windows consulte Microsoft Graph API o que una API web acepte tokens de un punto de conexión de una plataforma de identidad de Microsoft. En este escenario, agregará un token a las solicitudes HTTP mediante el encabezado de autorización. La Biblioteca de autenticación de Microsoft (MSAL) administra la adquisición y renovación de los tokens.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Tratamiento de la adquisición de tokens para tener acceso a API web protegidas

Una vez que el usuario se autentica, la aplicación de ejemplo recibe un token que se puede usar para consultar una Microsoft Graph API o a una API web protegida mediante una plataforma de identidad de Microsoft para desarrolladores.

Las API, como Microsoft Graph, requieren un token para permitir el acceso a recursos específicos. Por ejemplo, un token es necesario para leer un perfil del usuario, acceder al calendario de un usuario o enviar correo electrónico. La aplicación puede solicitar un token de acceso mediante MSAL para acceder a estos recursos mediante la especificación de ámbitos de API. Este token de acceso se agrega luego al encabezado de autorización de HTTP para todas las llamadas efectuadas al recurso protegido.

MSAL administra el almacenamiento en caché y la actualización de los tokens de acceso automáticamente, así que no tiene que preocuparse por ello.

## <a name="nuget-packages"></a>Paquetes NuGet

Esta guía utiliza los siguientes paquetes NuGet:

|Biblioteca|DESCRIPCIÓN|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Biblioteca de autenticación de Microsoft (MSAL.NET)|
