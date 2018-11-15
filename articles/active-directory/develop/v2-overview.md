---
title: Acerca de la versión 2.0 | Azure
description: Obtenga información sobre la plataforma y el punto de conexión de la versión 2.0.
services: active-directory
documentationcenter: dev-center-name
author: CelesteDG
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/24/2018
ms.author: celested
ms.reviewer: saeeda
ms.custom: aaddev
ms.openlocfilehash: ecb95f0440751a6cdbf81dbf02c62bed6b5e780b
ms.sourcegitcommit: 02ce0fc22a71796f08a9aa20c76e2fa40eb2f10a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/08/2018
ms.locfileid: "51286697"
---
# <a name="about-v20"></a>Acerca de la versión 2.0

La plataforma y el punto de conexión de la versión 2.0 han estado en versión preliminar y se han mejorado continuamente. Hoy en día, los escenarios de aplicación de página única (SPA) de JavaScript cuentan con todas las características. Le invitamos a que utilice MSAL.js para crear aplicaciones basadas en el explorador y nos envíe sus comentarios para que podamos actualizar el estado de versión preliminar a disponibilidad general (GA).

> [!NOTE]
> Las versiones de MSAL para .NET, iOS y Android todavía tienen características en desarrollo. Puede usarlas para crear aplicaciones y enviarnos sus comentarios.

Se actualizó significativamente la experiencia del desarrollador en Azure Portal para que, de ahora en adelante, incluya todas las aplicaciones compiladas con ADAL o MSAL y para mejorar la facilidad de uso.

Antes, los desarrolladores de aplicaciones que querían admitir cuentas de Microsoft personales y profesionales de Azure Active Directory (Azure AD) debían realizar la integración con dos sistemas independientes. El punto de conexión y la plataforma de la versión 2.0 proporcionan una versión de API de autenticación que simplifica este proceso. Permiten iniciar sesión con los dos tipos de cuenta mediante el uso de una única integración. Las aplicaciones que usan el punto de conexión de la versión 2.0 también pueden usar las API REST de [Microsoft Graph API](https://developer.microsoft.com/graph) con cualquier tipo de cuenta.

## <a name="getting-started"></a>Introducción

Elija su plataforma favorita de la lista siguiente para crear una aplicación mediante los marcos y bibliotecas de código abierto de Microsoft. También puede usar los protocolos OAuth 2.0 y OpenID Connect para enviar y recibir mensajes de protocolo directamente sin utilizar una biblioteca de autenticación.

[!INCLUDE [v2.0 endpoint platforms](../../../includes/active-directory-v2-quickstart-table.md)]

## <a name="learn-more-about-the-v20-endpoint-and-platform"></a>Obtenga más información sobre la plataforma y el punto de conexión de la versión 2.0

Obtener información sobre lo que puede hacer con el punto de conexión de la versión 2.0 de Azure AD:

* Descubra los [tipos de aplicaciones que se pueden crear con el punto de conexión de la versión 2.0 de Azure AD](v2-app-types.md).
* Conozca las [limitaciones y restricciones](active-directory-v2-limitations.md) del punto de conexión de la versión 2.0 de Azure AD.
* Vea este vídeo para una introducción al punto de conexión de la versión 2.0 de Azure AD:

>[!VIDEO https://channel9.msdn.com/Events/Build/2017/P4031/player]

## <a name="additional-resources"></a>Recursos adicionales

Explore la información detallada acerca de la versión 2.0:

* [Referencia de protocolos de la versión 2.0](active-directory-v2-protocols.md)
* [Referencia de los tokens de acceso](access-tokens.md)
* [Referencia de los tokens de identificador](id-tokens.md)
* [Referencia de las bibliotecas de autenticación de la versión 2.0](reference-v2-libraries.md)
* [Permisos y consentimiento en la versión 2.0](v2-permissions-and-consent.md)
* [Microsoft Graph API](https://developer.microsoft.com/graph)

> [!NOTE]
> Si solo necesita iniciar sesión en cuentas profesionales o educativas desde Azure Active Directory, comience con la [Guía del desarrollador de Azure AD](v1-overview.md). El punto de conexión v2.0 está diseñado para que lo usen desarrolladores que deban iniciar sesión explícitamente en cuentas personales de Microsoft.

[!INCLUDE [Help and support](../../../includes/active-directory-develop-help-support-include.md)]
