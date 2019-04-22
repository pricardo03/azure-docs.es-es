---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: danieldobalian
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: ios
ms.workload: identity
ms.date: 03/20/2019
ms.author: dadobali
ms.custom: include file
ms.openlocfilehash: 971ae8cd44f1b345d3a71b8fa4f256c8f25ef961
ms.sourcegitcommit: 1a19a5845ae5d9f5752b4c905a43bf959a60eb9d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/11/2019
ms.locfileid: "59502935"
---
# <a name="call-the-microsoft-graph-api-from-an-ios-application"></a>Llamada a Microsoft Graph API desde una aplicación iOS

En esta guía se muestra cómo una aplicación iOS nativa (Swift) puede llamar a las API que requieren los tokens de acceso desde el punto de conexión de la plataforma de identidad de Microsoft. La guía explica cómo obtener tokens de acceso y usarlos en las llamadas a Microsoft Graph API y otras API.

Después de completar los ejercicios de esta guía, la aplicación puede llamar a una API protegida de cualquier empresa u organización que tenga Azure AD. La aplicación puede realizar llamadas API protegidas mediante el uso de cuentas personales, como outlook.com, live.com u otras, así como cuentas profesionales o educativas.

## <a name="prerequisites"></a>Requisitos previos

- Para el ejemplo que se crea en esta guía, se requiere XCode versión 10.x. Puede descargar XCode desde [el sitio web de iTunes](https://geo.itunes.apple.com/us/app/xcode/id497799835?mt=12 "XCode Download URL").
- El administrador de dependencias de [Carthage](https://github.com/Carthage/Carthage) es necesario para la administración del paquete.

## <a name="how-this-guide-works"></a>Funcionamiento de esta guía

![Muestra cómo funciona la aplicación de ejemplo generada por este tutorial](media/active-directory-develop-guidedsetup-ios-introduction/iosintro.svg)

En esta guía, la aplicación de ejemplo permite que una aplicación iOS realice consultas a Microsoft Graph API o a una API web que acepte tokens desde un punto de conexión de la plataforma de identidad de Microsoft. En este escenario, se agrega un token a las solicitudes HTTP a través del encabezado de **autorización**. La adquisición y la renovación de los tokens se realizan por medio de la biblioteca de autenticación de Microsoft (MSAL).

### <a name="handle-token-acquisition-for-access-to-protected-web-apis"></a>Tratamiento de la adquisición de tokens para tener acceso a API web protegidas

Una vez el usuario se autentique, la aplicación de ejemplo recibe un token. El token se usa para consultar Microsoft Graph API o una API web que está protegida por el punto de conexión de la plataforma de identidad de Microsoft.

Las API, como Microsoft Graph, requieren un token de acceso para permitir el acceso a recursos específicos. Los tokens son necesarios para leer un perfil de usuario, acceder al calendario de un usuario, enviar un correo electrónico, etc. La aplicación puede solicitar un token de acceso mediante MSAL y la especificación de ámbitos de API. El token de acceso se agrega al encabezado de **autorización** de HTTP para todas las llamadas efectuadas al recurso protegido.

MSAL administra el almacenamiento en caché y la actualización de los tokens de acceso automáticamente, así que no tiene que preocuparse por ello.

## <a name="libraries"></a>Bibliotecas

Esta guía utiliza la siguiente biblioteca:

|Biblioteca|DESCRIPCIÓN|
|---|---|
|[MSAL.framework](https://github.com/AzureAD/microsoft-authentication-library-for-objc)|Versión preliminar de Biblioteca de autenticación de Microsoft para iOS|
