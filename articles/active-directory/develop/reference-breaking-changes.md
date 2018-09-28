---
title: Referencia de cambios importantes en Azure Active Directory B2C | Microsoft Docs
description: Cambios realizados en los protocolos de Azure AD que pueden afectar a la aplicación.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: 68517c83-1279-4cc7-a7c1-c7ccc3dbe146
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/31/2018
ms.author: celested
ms.reviewer: hirsin
ms.custom: aaddev
ms.openlocfilehash: 14217d03cdb56c5c641ab8f8c3fc0748e8e815a2
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46987796"
---
# <a name="whats-new-for-authentication"></a>Novedades en la autenticación 

|
>Reciba notificaciones para volver a visitar esta página y obtener actualizaciones; para ello, agregue esta [URL](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20for%20authentication%22&locale=en-us) al lector de fuentes RSS.

El sistema de autenticación altera y agrega características constantemente para aumentar la seguridad y mejorar el cumplimiento de normas. Para mantenerse al día con los avances más recientes, este artículo proporciona información acerca de los elementos siguientes:

- Las características más recientes
- Problemas conocidos
- Cambios en el protocolo
- Funciones obsoletas

> [!TIP] 
> Esta página se actualiza regularmente, por lo que se recomienda visitarla a menudo. Salvo que se indique lo contrario, estos cambios solo entrarán en vigor en las aplicaciones recién registradas.  

## <a name="upcoming-changes"></a>Próximos cambios

### <a name="authorization-codes-can-no-longer-be-reused"></a>No se podrán reutilizar los códigos de autorización

**Fecha de vigencia**: 10 de octubre de 2018 **Puntos de conexión afectados**: versiones 1.0 y 2.0 **Protocolo afectado**: [flujo de código](v2-oauth2-auth-code-flow.md)

A partir del 10 de octubre de 2018, Azure AD dejará de aceptar los códigos de autenticación usados anteriormente para las nuevas aplicaciones. Cualquier aplicación que creara antes del 10 de octubre de 2018, todavía podrá usar códigos de autenticación. Este cambio de seguridad ayuda a poner Azure AD en consonancia con la especificación de OAuth y se aplicará en los puntos de conexión v1 y v2.

Si la aplicación reutiliza códigos de autorización para obtener tokens para varios recursos, es recomendable que use el código para obtener un token de actualización y, a continuación, utilice este para adquirir tokens adicionales para otros recursos. Los códigos de autorización solo se pueden usar una vez, pero los tokens de actualización se pueden usar varias veces en varios recursos. Cualquier nueva aplicación que intente reutilizar un código de autenticación durante el flujo de código de OAuth obtendrá el error invalid_grant.

Para más información acerca de los tokens de actualización, consulte [Actualización de los tokens de acceso](v1-protocols-oauth-code.md#refreshing-the-access-tokens).

## <a name="may-2018"></a>Mayo de 2018

### <a name="id-tokens-cannot-be-used-for-the-obo-flow"></a>No se puede usar los tokens de identificador para el flujo de OBO

**Fecha**: 1 de mayo de 2018 **Puntos de conexión afectados**: versiones 1.0 y 2.0 **Protocolos afectados**: [flujo implícito y flujo de OBO](v1-oauth2-on-behalf-of-flow.md)

A partir del 1 de mayo de 2018, id_tokens no se puede utilizar como instrucción de aserción en un flujo de OBO en las aplicaciones nuevas.  En su lugar deben usarse tokens de acceso para proteger las API, incluso entre un cliente y el nivel intermedio de la misma aplicación.  Las aplicaciones registradas antes del 1 de mayo de 2018 seguirán funcionando y podrán intercambiar id_tokens por un token de acceso (pero tenga en cuenta que esto no se considera un procedimiento recomendado).

Para mitigar el problema que pueda provocar este cambio, puede hacer lo siguiente:

1. Cree una API web para la aplicación de nivel intermedio, con uno o más ámbitos,  lo que permitirá mayor control y seguridad.
1. En el manifiesto de la aplicación, [Azure Portal](https://portal.azure.com) o el [portal de registro de aplicaciones](https://apps.dev.microsoft.com), asegúrese de que la aplicación tiene permiso para emitir tokens de acceso mediante el flujo implícito, lo que se controla mediante la clave `oauth2AllowImplicitFlow`.
1. Cuando la aplicación cliente solicita un id_token a través de `response_type=id_token`, también solicita un token de acceso (`response_type=token`) para la API web creada anteriormente.  Por consiguiente, cuando se usa el punto de conexión v2.0 el parámetro `scope` debe ser similar a `api://GUID/SCOPE`.  En el punto de conexión v1.0, el parámetro `resource` debe ser el identificador URI de la aplicación de la API web.
1. Pase este token de acceso al nivel intermedio en lugar de id_token.  