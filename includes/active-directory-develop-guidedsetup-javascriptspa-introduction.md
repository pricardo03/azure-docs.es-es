---
title: archivo de inclusión
description: archivo de inclusión
services: active-directory
documentationcenter: dev-center-name
author: navyasric
manager: mtillman
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/17/2018
ms.author: nacanuma
ms.custom: include file
ms.openlocfilehash: 2a4c389d063bb63f2fa2293d54236f99d7035e0e
ms.sourcegitcommit: 715813af8cde40407bd3332dd922a918de46a91a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "47061005"
---
# <a name="call-the-microsoft-graph-api-from-a-javascript-single-page-application-spa"></a>Llamar a Microsoft Graph API desde una aplicación de página única de JavaScript (SPA)

En esta guía se muestra cómo una aplicación de una sola página (SPA) de JavaScript puede iniciar sesión en cuentas personales, profesionales y educativas, obtener un token de acceso y llamar a Microsoft Graph API u otras API que requieran tokens de acceso desde el punto de conexión de Azure Active Directory v2.

## <a name="how-the-sample-app-generated-by-this-guide-works"></a>Funcionamiento de la aplicación de ejemplo generada por esta guía

![Funcionamiento de la aplicación de ejemplo generada por esta guía](media/active-directory-develop-guidedsetup-javascriptspa-introduction/javascriptspa-intro.png)

<!--start-collapse-->
### <a name="more-information"></a>Más información

La aplicación de ejemplo que se crea con esta guía permite que una aplicación de página única de JavaScript consulte a Microsoft Graph API o a una API web que acepte tokens de un punto de conexión de Azure Active Directory v2. En este escenario, después de que un usuario inicia sesión, se solicita un token de acceso y se agrega a las solicitudes HTTP mediante el encabezado de autorización. La adquisición y la renovación de los tokens se realizan por medio de la biblioteca de autenticación de Microsoft (MSAL).

<!--end-collapse-->

<!--start-collapse-->
### <a name="libraries"></a>Bibliotecas

Esta guía utiliza la siguiente biblioteca:

|Biblioteca|DESCRIPCIÓN|
|---|---|
|[msal.js](https://github.com/AzureAD/microsoft-authentication-library-for-js)|Biblioteca de autenticación de Microsoft para la vista preliminar de JavaScript|

> [!NOTE]
> *msal.js* apunta al *punto de conexión de Azure Active Directory v2*, lo que permite que las cuentas personales, profesionales y educativas inicien sesión y adquieran tokens. El *punto de conexión de Azure Active Directory v2* tiene [algunas limitaciones](..\articles\active-directory\develop\active-directory-v2-limitations.md).
> Para comprender las diferencias entre los puntos de conexión v1 y v2, lea la [comparación entre v1 y v2](../articles/active-directory/develop/azure-ad-endpoint-comparison.md).

<!--end-collapse-->
