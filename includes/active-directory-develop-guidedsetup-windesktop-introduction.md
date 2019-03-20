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
ms.date: 03/20/2019
ms.author: jmprieur
ms.custom: include file
ms.openlocfilehash: bb28862ad6452eab3130eeb2dc0b4c269839d306
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203229"
---
# <a name="call-the-microsoft-graph-api-from-a-windows-desktop-app"></a>Llamada a Microsoft Graph API desde una aplicación de escritorio de Windows

En esta guía se muestra cómo una aplicación nativa .NET de escritorio de Windows (XAML) puede obtener un token de acceso y llamar a Microsoft Graph API u otras API que requieren tokens de acceso desde un punto de conexión de Azure Active Directory v2.0.

Cuando haya completado la guía, la aplicación podrá llamar a una API protegida que usa cuentas personales (lo que incluye outlook.com, live.com y otras). La aplicación también usa cuentas profesionales y educativas de cualquier empresa y organización que utilice Azure Active Directory.  

> [!NOTE]
> Esta guía requiere Visual Studio 2015 Update 3 o Visual Studio 2017. ¿No tiene ninguna de estas versiones? [Descargue Visual Studio 2017 de manera gratuita](https://www.visualstudio.com/downloads/).

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Funcionamiento de la aplicación de ejemplo generada por esta guía

![Muestra cómo la aplicación de ejemplo generados por este funciona tutoriales](./media/active-directory-develop-guidedsetup-windesktop-intro/windesktophowitworks-updated.png)

La aplicación de ejemplo que se crea con esta guía permite que una aplicación de escritorio de Windows consulte Microsoft Graph API o que una API web acepte tokens de un punto de conexión de Azure Active Directory v2.0. En este escenario, agregará un token a las solicitudes HTTP mediante el encabezado de autorización. La Biblioteca de autenticación de Microsoft (MSAL) administra la adquisición y renovación de los tokens.

## <a name="handling-token-acquisition-for-accessing-protected-web-apis"></a>Tratamiento de la adquisición de tokens para tener acceso a API web protegidas

Una vez que el usuario se autentica, la aplicación de ejemplo recibe un token que se puede usar para consultar una Microsoft Graph API o a una API web protegida mediante Azure Active Directory v2.

Las API, como Microsoft Graph, requieren un token para permitir el acceso a recursos específicos. Por ejemplo, un token es necesario para leer un perfil del usuario, acceder al calendario de un usuario o enviar correo electrónico. La aplicación puede solicitar un token de acceso mediante MSAL para acceder a estos recursos mediante la especificación de ámbitos de API. Este token de acceso se agrega luego al encabezado de autorización de HTTP para todas las llamadas efectuadas al recurso protegido.

MSAL administra el almacenamiento en caché y la actualización de los tokens de acceso automáticamente, así que no tiene que preocuparse por ello.

## <a name="nuget-packages"></a>Paquetes NuGet

Esta guía utiliza los siguientes paquetes NuGet:

|Biblioteca|DESCRIPCIÓN|
|---|---|
|[Microsoft.Identity.Client](https://www.nuget.org/packages/Microsoft.Identity.Client)|Biblioteca de autenticación de Microsoft (MSAL)|
