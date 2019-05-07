---
title: 'Web de aplicación de demonio que realiza la llamada API (información general): la plataforma de identidad de Microsoft'
description: Obtenga información sobre cómo compilar una aplicación demonio que llama a las API web
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
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65075886"
---
# <a name="scenario-daemon-application-that-calls-web-apis"></a>Escenario: Aplicación de demonio que llama a las API web

Aprenda todo lo que necesita para compilar una aplicación de demonio que llama a las API web.

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [Pre-requisites](../../../includes/active-directory-develop-scenarios-prerequisites.md)]

## <a name="overview"></a>Información general

La aplicación puede adquirir un token para llamar a una API web en nombre propio (no en el nombre de un usuario). Este escenario es útil para aplicaciones de demonio. Utiliza el estándar de OAuth 2.0 [las credenciales del cliente](v2-oauth2-client-creds-grant-flow.md) conceder.

![Aplicaciones de demonio](./media/scenario-daemon-app/daemon-app.svg)

Estos son algunos ejemplos de casos de uso de aplicaciones de demonio:

- Las aplicaciones Web que se usan para aprovisionar o administración los usuarios o procesos en un directorio de batch
- Aplicaciones de escritorio como (servicios de windows en Windows) o los procesos de demonios de Linux que realizan los trabajos por lotes o un servicio de sistema operativo que se ejecuta en segundo plano
- API Web que necesitan manipular directorios, los usuarios no específicos

Hay otro caso habitual donde las aplicaciones que no son daemon usen las credenciales del cliente: incluso cuando actúan en nombre de usuarios, que necesitan tener acceso a una API web o un recurso con su identidad por motivos técnicos. Un ejemplo es el acceso a los secretos en Key Vault o Azure SQL database para una memoria caché.

Aplicaciones que adquieren un token para sus propias identidades:

- Son aplicaciones de cliente confidencial. Estas aplicaciones, dado que acceden a recursos independientemente de un usuario, deben demostrar su identidad. También son las aplicaciones confidenciales en su lugar, lo que necesitan ser aprobados por administradores de inquilinos de Azure Active Directory (Azure AD).
- Ha registrado un secreto (contraseña de la aplicación o certificado) con Azure AD. Este secreto es pasado durante la llamada a Azure AD para obtener un token.

## <a name="specifics"></a>Características específicas

> [!IMPORTANT]
>
> - Interacción del usuario no es posible con una aplicación de demonio. Una aplicación de demonio requiere su propia identidad. Este tipo de aplicación solicita un token de acceso con su identidad de aplicación y presenta su Id. de aplicación, credenciales (contraseña o certificado) y aplicación de URI de Id. de Azure AD. Tras una autenticación correcta, el demonio recibe un token de acceso (y un token de actualización) desde el extremo de plataforma de identidad de Microsoft, que, a continuación, se usa para llamar a la API web (y se actualiza según sea necesario).
> - Dado que no es posible la interacción del usuario, el consentimiento incremental no será posible. Todos los permisos necesarios de API deben configurarse en el registro de aplicación y el código de la aplicación simplemente solicita permisos definidos estáticamente. Esto también significa que las aplicaciones de demonio no admiten consentimiento incremental.

Para los desarrolladores, la experiencia de extremo a otro para este escenario tiene los siguientes aspectos:

- Aplicaciones demonio solo pueden funcionar en inquilinos de Azure AD. Para crear una aplicación demonio que intenta manipular cuentas personales de Microsoft no tendría sentido. Si es un desarrollador de aplicaciones de línea de negocio (LOB), creará la aplicación de demonio en el inquilino. Si es un ISV, que es posible que desee crear una aplicación de demonio de varios inquilinos. Deberá ser aceptado por cada administrador de inquilinos.
- Durante la [registro de la aplicación](./scenario-daemon-app-registration.md), **URI de respuesta** no es necesario. Necesita compartir certificados o secretos con Azure AD, y deberá solicitar permisos de aplicaciones y conceder consentimiento para usar esos permisos de aplicación.
- El [configuración de la aplicación](./scenario-daemon-app-configuration.md) tiene que proporcionar credenciales de cliente como compartido con Azure AD durante el registro de aplicación.
- El [ámbito](scenario-daemon-acquire-token.md#scopes-to-request) utilizada para adquirir un token con las credenciales del cliente flujo debe ser un ámbito estático.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Aplicación de demonio - registro de aplicación](./scenario-daemon-app-registration.md)
