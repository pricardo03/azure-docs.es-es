---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/13/2018
ms.author: andret
ms.custom: include file
ms.openlocfilehash: 9bc8f30d2bbf6a084ad680306da9b1e330d488e3
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/24/2018
ms.locfileid: "49988366"
---
# <a name="sign-in-users-and-call-the-microsoft-graph-from-an-android-app"></a>Inicie la sesión de los usuarios y llame a Microsoft Graph desde una aplicación Android

En este tutorial, obtendrá información sobre cómo compilar una aplicación Android y cómo integrarla en la plataforma de identidad de Microsoft. En concreto, esta aplicación iniciará la sesión de un usuario, obtendrá un token de acceso para llamar a Microsoft Graph API y realizará una solicitud básica a Microsoft Graph API.  

Cuando haya completado la guía, la aplicación podrá aceptar inicios de sesión de cuentas Microsoft personales (como outlook.com, live.com y otras), y cuentas profesionales y educativas de cualquier empresa u organización que utilice Azure Active Directory.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Funcionamiento de la aplicación de ejemplo generada por esta guía

![Funcionamiento de este ejemplo](media/active-directory-develop-guidedsetup-android-intro/android-intro.png)

La aplicación de este ejemplo iniciará la sesión de los usuarios y obtendrá datos en su nombre.  El acceso a los datos se realizará a través de una API remota (Microsoft Graph API en este caso) que requiere autorización y también está protegida por la plataforma de identidad de Microsoft.

Más concretamente:

* La aplicación iniciará una página web para iniciar la sesión del usuario.
* Se emitirá un token de acceso a la aplicación para Microsoft Graph API.
* El token de acceso se incluirán en la solicitud HTTP a la API web.
* Se procesa la respuesta de Microsoft Graph.

Este ejemplo utiliza la biblioteca de Microsoft Authentication para Android (MSAL) para coordinar y ayudar con la autenticación. MSAL renueva automáticamente los tokens, proporciona inicio de sesión único entre las aplicaciones del dispositivo, ayuda a administrar las cuentas y controla la mayoría de los casos de acceso condicional.

## <a name="prerequisites"></a>Requisitos previos

* Esta configuración guiada usa Android Studio 3.0.
* Se necesita Android 21 o versiones posteriores (se recomienda la versión 25 o posteriores).
* Se requiere Google Chrome o un explorador web que use pestañas personalizadas para esta versión de MSAL para Android.

## <a name="library"></a>Biblioteca

Esta guía utiliza la siguiente biblioteca de autenticación:

|Biblioteca|DESCRIPCIÓN|
|---|---|
|[com.microsoft.identity.client](http://javadoc.io/doc/com.microsoft.identity.client/msal)|Biblioteca de autenticación de Microsoft (MSAL)|
