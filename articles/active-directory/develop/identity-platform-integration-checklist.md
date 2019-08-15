---
title: Integración con la plataforma de identidad de Microsoft | Azure
description: Conozca los procedimientos recomendados y las omisiones más comunes al integrarse con la plataforma de identidad de Microsoft (v2.0).
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/07/2019
ms.author: ryanwi
ms.reviewer: lenalepa, sureshja, jesakowi
ms.custom: aaddev, identityplatformtop40
ms.openlocfilehash: 33128cbece3b217778182b3831b02e2f3f654f3b
ms.sourcegitcommit: 670c38d85ef97bf236b45850fd4750e3b98c8899
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2019
ms.locfileid: "68853217"
---
# <a name="microsoft-identity-platform-integration-checklist"></a>Lista de comprobación de la integración con la plataforma de identidad de Microsoft

La lista de comprobación de la integración con la plataforma de identidad de Microsoft tiene por objeto guiarle hacia una integración segura y de alta calidad. Destaca los procedimientos recomendados y los descuidos más comunes al integrarse con la plataforma de identidad de Microsoft, por lo que debe revisar la lista periódicamente para asegurarse de que mantiene la calidad y seguridad de la integración de la aplicación con la plataforma de identidad. La lista de comprobación no está pensada para revisar toda la aplicación. El contenido de la lista de comprobación está sujeto a cambios a medida que introducimos mejoras en la plataforma.

Si acaba de empezar, consulte la [documentación](index.yml) para obtener información básica sobre autenticación, escenarios de aplicación en la plataforma de identidad de Microsoft y mucho más.

## <a name="testing-your-integration"></a>Prueba de la integración

Utilice la siguiente lista de comprobación para asegurarse de que su aplicación está efectivamente integrada con la [plataforma de identidad de Microsoft](https://docs.microsoft.com/azure/active-directory/develop/).

### <a name="basics"></a>Aspectos básicos

|   |   |
|---|---|
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | Lea y comprenda las [directivas de la plataforma de Microsoft](https://go.microsoft.com/fwlink/?linkid=2090497&clcid=0x409). Asegúrese de que la aplicación se adhiere a los términos descritos, ya que están pensados para proteger a los usuarios y a la plataforma. |

### <a name="ownership"></a>Propiedad

|   |   |
|---|---|
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | Asegúrese de que la información asociada con la cuenta que ha usado para registrar y administrar las aplicaciones está actualizada. |

### <a name="branding"></a>Personalización de marca

|   |   |
|---|---|
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | Siga las [directrices de personalización de marca para aplicaciones](howto-add-branding-in-azure-ad-apps.md). |
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | Proporcione un nombre significativo y un logotipo para la aplicación. Esta información aparece en la petición de consentimiento de la aplicación. Asegúrese de que el nombre y el logotipo son representativos de su producto y empresa para que los usuarios puedan tomar decisiones informadas. Asegúrese de que no infringen ninguna marca comercial. |

### <a name="privacy"></a>Privacidad

|   |   |
|---|---|
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | Proporciona los vínculos a los términos del servicio y la declaración de privacidad de la aplicación. |

### <a name="security"></a>Seguridad

|   |   |
|---|---|
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | Mantenga la propiedad de todos los URI de redireccionamiento y mantenga actualizados los registros DNS de los mismos. No utilice caracteres comodín (*) en los URI. Para las aplicaciones web, asegúrese de que todos los URI son seguros y están cifrados (por ejemplo, mediante esquemas de https). Para los clientes públicos, utilice los URI de redireccionamiento específicos de la plataforma si es aplicable (sobre todo para iOS y Android). En caso contrario, use el URI de redireccionamiento con una gran cantidad de aleatoriedad para evitar conflictos cuando llame de nuevo a la aplicación. Si se utiliza la aplicación desde un agente web aislado, puede usar https://login.microsoftonline.com/nativeclient. Revise y recorte todos los identificadores URI de redirección que no se usen o que sean innecesarios de manera regular. |
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | Si la aplicación se registra en un directorio, minimice y supervise manualmente la lista de propietarios del registro de aplicaciones. |
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | No habilite la compatibilidad con el [flujo de concesión implícita de OAuth2](v2-oauth2-implicit-grant-flow.md) a menos que se requiera de forma explícita. Más información sobre el escenario válido [aquí](v1-oauth2-implicit-grant-flow.md#suitable-scenarios-for-the-oauth2-implicit-grant). |
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | No use el [flujo de credenciales de la contraseña de propietario del recurso (ROPC)](v2-oauth-ropc.md), que controla directamente las contraseñas de usuario. Este flujo requiere un alto grado de confianza y exposición del usuario, por lo que solo se debería usar cuando no se puedan usar otros flujos más seguros. |
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | Proteja y administre las credenciales de la aplicación. Utilice las [credenciales del certificado](active-directory-certificate-credentials.md), no las credenciales de la contraseña (secretos de cliente). Si debe utilizar una credencial de contraseña, no la configure manualmente. No almacene las credenciales en el código o en la configuración y nunca permita que las manipulen los seres humanos. Si es posible, utilice [identidades administradas para los recursos de Azure](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview) o [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis) para almacenar y cambiar periódicamente las credenciales. |
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | Asegúrese de que la aplicación solicita permisos con privilegios mínimos. Solo pida los permisos que la aplicación necesita absolutamente y únicamente cuando los necesite. Comprenda los diferentes [tipos de permisos](v1-permissions-and-consent.md#types-of-permissions). Utilice únicamente los permisos de aplicación si es necesario; utilice permisos delegados siempre que sea posible. Para obtener una lista completa de los permisos de Microsoft Graph, consulte esta [referencia sobre los permisos](https://docs.microsoft.com/graph/permissions-reference). |
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | Si protege una API mediante la plataforma de identidad de Microsoft, piense detenidamente en los permisos que debería exponer. Considere cuál es la granularidad correcta para la solución y qué permisos requieren el consentimiento del administrador. Compruebe los permisos esperados en los tokens entrantes antes de tomar cualquier decisión de autorización. |

### <a name="implementation"></a>Implementación

|   |   |
|---|---|
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | Utilice soluciones de autenticación modernas (OAuth 2.0, [OpenID Connect](v2-protocols-oidc.md)) para iniciar sesión de forma segura. |
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | No implemente los protocolos usted mismo: utilice las [bibliotecas de autenticación compatibles con Microsoft](reference-v2-libraries.md) (MSAL, middleware de servidor). Asegúrese de que está utilizando la versión más reciente de la biblioteca de autenticación con la que se ha integrado. |
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | Si los datos que necesita la aplicación están disponibles con [Microsoft Graph](https://developer.microsoft.com/graph), solicite permisos para estos datos mediante el punto de conexión de Microsoft Graph en lugar de la API individual. |

### <a name="end-user-experience"></a>Experiencia del usuario final

|   |   |
|---|---|
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | [Comprenda la experiencia de consentimiento](application-consent-experience.md) y configure las partes de la petición de consentimiento de la aplicación para que los usuarios finales y los administradores tengan suficiente información para determinar si confían en la aplicación. |
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | Minimice el número de veces que un usuario necesita especificar las credenciales de inicio de sesión mientras utiliza la aplicación al intentar la autenticación silenciosa (adquisición de tokens silenciosos) antes de los flujos interactivos. |
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | No use "prompt=consent" para cada inicio de sesión. Solo utilice prompt=consent si ha determinado que necesita pedir consentimiento para obtener permisos adicionales (por ejemplo, si ha cambiado los permisos necesarios de la aplicación). |
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | Si procede, enriquecer su aplicación con datos de usuario. Usar [Microsoft Graph API](https://developer.microsoft.com/graph) es una forma fácil de hacerlo. La herramienta [Probador de Graph](https://developer.microsoft.com/graph/graph-explorer) que le ayudará a empezar. |
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | Registre el conjunto completo de permisos que la aplicación requiere para que los administradores puedan otorgar el consentimiento fácilmente al inquilino. Utilice el [consentimiento incremental](azure-ad-endpoint-comparison.md#incremental-and-dynamic-consent) en tiempo de ejecución para ayudar a los usuarios a comprender por qué la aplicación está solicitando permisos que pueden afectar o confundir a los usuarios cuando se solicitan al iniciar la aplicación por primera vez. |
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | Implemente una [experiencia de cierre de sesión único limpia](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-6-SignOut). Es un requisito de privacidad y seguridad, y contribuye a una buena experiencia de usuario. |

### <a name="testing"></a>Prueba

|   |   |
|---|---|
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | Compruebe si hay [directivas de acceso condicional](https://github.com/Azure-Samples/active-directory-aspnetcore-webapp-openidconnect-v2/tree/master/1-WebApp-OIDC/1-6-SignOut) que puedan afectar a la capacidad de los usuarios para utilizar la aplicación. |
| ![casilla](./media/active-directory-integration-checklist/checkbox-two.svg) | Pruebe la aplicación con todas las cuentas posibles que piensa admitir (por ejemplo, cuentas profesionales o educativas, cuentas personales de Microsoft, cuentas secundarias y cuentas soberanas). |

## <a name="additional-resources"></a>Recursos adicionales

Explore la información detallada acerca de la versión 2.0:

* [Introducción a la Plataforma de identidad de Microsoft (versión 2.0)](v2-overview.md)
* [Referencia a los protocolos de la plataforma de identidad de Microsoft](active-directory-v2-protocols.md)
* [Referencia de los tokens de acceso](access-tokens.md)
* [Referencia de los tokens de identificador](id-tokens.md)
* [Referencia de las bibliotecas de autenticación](reference-v2-libraries.md)
* [Permisos y consentimiento en la plataforma de identidad de Microsoft](v2-permissions-and-consent.md)
* [Microsoft Graph API](https://developer.microsoft.com/graph)
