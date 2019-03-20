---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/20/2019
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: a6fcbc0e8adac75f17d7379ff512ba650d0bb118
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203350"
---
# <a name="call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Llamada a Microsoft Graph API desde una aplicación de página única (SPA) de JavaScript

En esta guía se muestra cómo una aplicación de página única (SPA) de JavaScript puede iniciar sesión en cuentas personales, profesionales y educativas, obtener un token de acceso y llamar a Microsoft Graph API u otras API que requieran tokens de acceso desde el punto de conexión de Azure Active Directory v2.0.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Funcionamiento de la aplicación de ejemplo generada por esta guía

![Muestra cómo la aplicación de ejemplo generados por este funciona tutoriales](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro-updated.png)

<!--start-collapse-->
### <a name="more-information"></a>Más información

La aplicación de ejemplo que se crea con esta guía permite que una aplicación de página única de JavaScript consulte a Microsoft Graph API o a una API web que acepte tokens de un punto de conexión de Azure Active Directory v2.0. En este escenario, después de que un usuario inicia sesión, se solicita un token de acceso y se agrega a las solicitudes HTTP mediante el encabezado de autorización. La adquisición y la renovación de los tokens se realizan por medio de la biblioteca de autenticación de Microsoft (MSAL).

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Bibliotecas

Esta guía utiliza la siguiente biblioteca:

|Biblioteca|DESCRIPCIÓN|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|Biblioteca de autenticación de Microsoft para la vista preliminar de JavaScript|

> [!NOTE]
> *msal.js* apunta al *punto de conexión de Azure Active Directory v2.0*, lo que permite que las cuentas personales, profesionales y educativas inicien sesión y adquieran tokens. El *punto de conexión de Azure Active Directory v2.0* tiene [algunas limitaciones](../articles/active-directory/develop/active-directory-v2-limitations.md).
> Para comprender las diferencias entre los puntos de conexión v1.0 y v2.0, lea la [guía de comparación entre puntos de conexión](../articles/active-directory/develop/azure-ad-endpoint-comparison.md).

<!--end-collapse-->
