---
title: 'Aplicación de demonio que llama a las API web (información general): Plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación de demonio que llama a las API web
services: active-directory
documentationcenter: dev-center-name
author: jmprieur
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: jmprieur
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 578b7cdb38b7df3fab5885d773354a36f76a4cfb
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "65075886"
---
# <a name="scenario-daemon-application-that-calls-web-apis"></a>Escenario: Aplicación de demonio que llama a las API web

Obtenga toda la información necesaria para compilar una aplicación de demonio que llama a las API web.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="overview"></a>Información general

La aplicación puede adquirir un token para llamar a una API web en nombre propio (no en el nombre de un usuario). Este escenario es útil para aplicaciones de demonio. Usa la concesión de [credenciales del cliente](v2-oauth2-client-creds-grant-flow.md) OAuth 2.0 estándar.

![Aplicaciones de demonio](./media/scenario-daemon-app/daemon-app.svg)

Estos son algunos ejemplos de casos de usos para aplicaciones de demonio:

- Aplicaciones web que se usan para aprovisionar o administrar usuarios, o llevar a cabo procesos en lote en un directorio
- Aplicaciones de escritorio (como servicios de ventanas en Windows o procesos de demonios en Linux) que realizan trabajos por lotes, o un servicio de sistema operativo que se ejecuta en segundo plano
- API Web que necesitan manipular directorios, no usuarios específicos

Hay otro caso habitual donde las aplicaciones que no son demonios usan las credenciales del cliente: incluso cuando actúan en nombre de usuarios, deben tener acceso a una API web o a un recurso con su identidad por motivos técnicos. Un ejemplo es el acceso a los secretos en Key Vault o una base de datos de Azure SQL para una memoria caché.

Las aplicaciones que adquieren un token para sus propias identidades:

- Son aplicaciones cliente confidenciales. Estas aplicaciones, dado que acceden a recursos independientemente de un usuario, deben demostrar su identidad. También son aplicaciones bastante confidenciales, por lo que tiene que ser aprobadas por los administradores de inquilinos de Azure Active Directory (Azure AD).
- Han registrado un secreto (contraseña de la aplicación o certificado) en Azure AD. Este secreto se pasa durante la llamada a Azure AD para obtener un token.

## <a name="specifics"></a>Características específicas

> [!IMPORTANT]
>
> - La interacción del usuario no es posible con una aplicación de demonio. Una aplicación de demonio requiere su propia identidad. Este tipo de aplicación solicita un token de acceso con su identidad de aplicación y presenta el identificador de la aplicación, las credenciales (contraseña o certificado) y el identificador URI del identificador de la aplicación a Azure AD. Tras una autenticación correcta, el demonio recibe un token de acceso (y un token de actualización) del punto de conexión de la Plataforma de identidad de Microsoft, que, luego, se usa para llamar a la API web (y se actualiza según sea necesario).
> - Dado que no es posible la interacción del usuario, el consentimiento incremental no será posible. Todos los permisos necesarios de API deben configurarse en el registro de aplicación, y el código de la aplicación simplemente solicita permisos definidos estáticamente. Esto también significa que las aplicaciones de demonio no admiten el consentimiento incremental.

Para los desarrolladores, la experiencia total de este escenario tiene los siguientes aspectos:

- Las aplicaciones de demonio solo pueden funcionar en inquilinos de Azure AD. No tendría sentido crear una aplicación de demonio que intentara manipular cuentas personales de Microsoft. Si es desarrollador de aplicaciones de línea de negocio (LOB), creará la aplicación de demonio en el inquilino. Si es ISV, es posible que quiera crear una aplicación de demonio de varios inquilinos. Cada administrador de inquilinos deberá dar su consentimiento.
- Durante el [registro de la aplicación](./scenario-daemon-app-registration.md), no es necesario el **URI de respuesta**. Tiene que compartir secretos o certificados con Azure AD, y tiene que solicitar permisos de aplicación y conceder el consentimiento del administrador para usar esos permisos de aplicación.
- La [configuración de la aplicación](./scenario-daemon-app-configuration.md) tiene que proporcionar las credenciales de cliente tal cual se comparten con Azure AD durante el registro de la aplicación.
- El [ámbito](scenario-daemon-acquire-token.md#scopes-to-request) usado para adquirir un token con el flujo de credenciales del cliente debe ser un ámbito estático.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aplicación de demonio: registro de la aplicación](./scenario-daemon-app-registration.md)
