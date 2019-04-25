---
title: Descripción de la autenticación en las API desde Azure Digital Twins | Microsoft Docs
description: Uso de Azure Digital Twins para conectarse y autenticarse en las API
author: lyrana
manager: alinast
ms.service: digital-twins
services: digital-twins
ms.topic: conceptual
ms.date: 11/13/2018
ms.author: lyrana
ms.openlocfilehash: 4ea4479d77e06940bed50859341952ffbcbbda46
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60533837"
---
# <a name="connect-and-authenticate-to-apis"></a>Conexión y autenticación en las API

Azure Digital Twins utiliza Azure Active Directory (Azure AD) para autenticar a los usuarios y proteger a las aplicaciones. Azure AD admite la autenticación de diversas arquitecturas modernas. Todas ellas se basan en los protocolos estándar del sector OAuth 2.0 u OpenID Connect. Además, los desarrolladores pueden usar Azure AD para compilar las aplicaciones de inquilino único y línea de negocio (LOB). Los desarrolladores pueden usar Azure AD para desarrollar aplicaciones para varios inquilinos.

Para obtener información general de Azure AD, visite la [página de aspectos básicos](https://docs.microsoft.com/azure/active-directory/fundamentals/index) para acceder a guías paso a paso, conceptos y guías de inicio rápido.

Para integrar una aplicación o un servicio con Azure AD, el desarrollador debe registrar primero la aplicación en Azure AD. Para obtener instrucciones detalladas y capturas de pantalla, consulte [este inicio rápido](https://docs.microsoft.com/azure/active-directory/develop/quickstart-v1-add-azure-ad-app).

Azure AD admite [cinco escenarios principales de aplicación](https://docs.microsoft.com/azure/active-directory/develop/v2-app-types):

* Aplicación de página única (SPA): Un usuario debe iniciar sesión una aplicación de una página que está protegida por Azure AD.
* Explorador Web a aplicación web: Un usuario debe iniciar sesión en una aplicación web que está protegida por Azure AD.
* Aplicación nativa a API web: Una aplicación nativa que se ejecuta en un teléfono, tableta o PC debe autenticar un usuario para obtener recursos de una API web protegida por Azure AD.
* Aplicación Web a API web: una aplicación web tiene que obtener recursos de una API web protegida por Azure AD.
* Aplicación de servidor o demonio a API web: Una aplicación de demonio o una aplicación de servidor con ningún web UI necesita obtener recursos desde una API web protegida por Azure AD.

La biblioteca de autenticación de Azure para Windows ofrece muchas alternativas para adquirir tokens de Active Directory. Para obtener más información en la biblioteca y ejemplos de código, vea [este artículo](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki).

## <a name="call-digital-twins-from-a-middle-tier-web-api"></a>Llamada a Digital Twins desde una API web de nivel intermedio

Cuando los desarrolladores diseñan soluciones de Digital Twins, normalmente crean una aplicación o API web de nivel intermedio. La aplicación o la API, a continuación, llama a la API de Digital Twins de un nivel inferior. Para admitir la arquitectura de la solución web estándar, asegúrese de que los usuarios primero:

1. Se autentiquen con la aplicación de nivel intermedio.

1. Adquieran un token en nombre de OAuth 2.0 durante la autenticación.

1. A continuación, el token adquirido se usa para autenticarse con API o para llamar a API que están en niveles inferiores con el flujo "en nombre de".

Para obtener instrucciones sobre cómo coordinar el flujo en el nombre de otra persona, vea [Flujo en nombre de OAuth 2.0](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-on-behalf-of-flow). También puede ver ejemplos de código en [Calling a downstream web API](https://azure.microsoft.com/resources/samples/active-directory-dotnet-webapi-onbehalfof/) (Llamada a una API web de bajada).

## <a name="next-steps"></a>Pasos siguientes

Para configurar y probar Azure Digital Twins mediante el flujo de concesión implícita de OAuth 2.0, lea [Configure Postman](./how-to-configure-postman.md) (Configurar Postman).

Para obtener información acerca de la seguridad en Azure Digital Twins, consulte [Create and manage role assignments](./security-create-manage-role-assignments.md) (Creación y administración de las asignaciones de roles).