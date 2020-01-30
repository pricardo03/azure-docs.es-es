---
title: Notas del desarrollador para las directivas personalizadas
titleSuffix: Azure AD B2C
description: Notas para desarrolladores sobre la configuración y el mantenimiento de Azure AD B2C con directivas personalizadas.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: acd02f143fe35edd867ce26f26a4cba74bd6f10b
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76850587"
---
# <a name="developer-notes-for-custom-policies-in-azure-active-directory-b2c"></a>Notas para desarrolladores sobre directivas personalizadas en Azure Active Directory B2C

La configuración de directivas personalizadas en Azure Active Directory B2C ya está disponible con carácter general. Este método de configuración está destinado a desarrolladores de identidades avanzados que crean soluciones de identidad complejas. Las directivas personalizadas permiten aprovechar la capacidad de Identity Experience Framework en los inquilinos de Azure AD B2C.
Los desarrolladores de identidades avanzados que usan directivas personalizadas deben planificar un tiempo para realizar tutoriales y leer documentos de referencia.

Aunque la mayoría de las opciones de directivas personalizadas ya se encuentran disponibles con carácter general, existen funcionalidades subyacentes, como tipos de perfiles técnicos y API de definición de contenido, que se encuentran en fases distintas del ciclo de vida del software. Próximamente se ofrecerán muchas más. En la tabla siguiente se especifica el nivel de disponibilidad a un nivel más pormenorizado.

## <a name="features-that-are-generally-available"></a>Características disponibles con carácter general

- Crear y cargar recorridos del usuario de autenticación personalizada mediante directivas personalizadas.
    - Describir los recorridos del usuario paso a paso como intercambios entre proveedores de notificaciones.
    - Definir la creación de ramas condicional en recorridos del usuario.
- Interoperar con servicios con la API REST habilitada en los recorridos del usuario de autenticación personalizada.
- Federarse con proveedores de identidades que cumplen con el protocolo OpenIDConnect.
- Federarse con proveedores de identidades que cumplen el protocolo SAML 2.0.

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>Responsabilidades de los desarrolladores de conjunto de características de directivas personalizadas

La configuración manual de directivas concede un acceso de nivel inferior a la plataforma subyacente de Azure AD B2C y da como resultado la creación de un marco de confianza único. Las múltiples permutaciones de proveedores de identidades personalizados, las relaciones de confianza, las integraciones con servicios externos y los flujos de trabajo paso a paso exigen un enfoque de diseño y configuración más metódico.

Los desarrolladores que utilizan el conjunto de características de directivas personalizadas deben cumplir las siguientes directrices:

- Conocer el lenguaje de configuración de las directivas personalizadas y la administración de claves/secretos. Para más información, consulte [TrustFrameworkPolicy](trustframeworkpolicy.md).
- Tomar posesión de escenarios e integraciones personalizadas. Documentar su trabajo e informar a la organización del sitio activo.
- Realizar pruebas metódicas de los escenarios.
- Seguir los procedimientos recomendados de desarrollo de software y almacenamiento provisional con un mínimo de un entorno de desarrollo y prueba, y un entorno de producción.
- Mantenerse informado sobre los nuevos desarrollos de los proveedores de identidad y los servicios con los que está integrado. Por ejemplo, realizar un seguimiento de cambios en los secretos y los cambios programados y no programados en el servicio.
- Configurar la supervisión activa y supervisar la capacidad de respuesta de los entornos de producción. Para más información sobre la integración con Application Insights, vea [Azure Active Directory B2C: Recopilación de registros](analytics-with-application-insights.md).
- Mantenga actualizadas las direcciones de correo electrónico de contacto y responda a los correos mensajes de correo electrónico del equipo de Microsoft.
- Realizar las acciones pertinentes cuando se lo indique el equipo del sitio activo de Microsoft.

## <a name="terms-for-features-in-public-preview"></a>Términos de características en versión preliminar pública

- Se aconseja usar las características en versión preliminar pública solo con fines de evaluación.
- Los Acuerdos de Nivel de Servicio (SLA) no se aplican a las características en versión preliminar pública.
- Las solicitudes de soporte técnico para características en versión preliminar pública pueden enviarse a través de los canales de soporte técnico habituales.

## <a name="features-by-stage-and-known-issues"></a>Características por fase y problemas conocidos

Las directivas personalizadas y las funcionalidades de Identity Experience Framework están en desarrollo constante y rápido. La siguiente tabla es un índice de disponibilidad de características y componentes.

### <a name="identity-providers-tokens-protocols"></a>Proveedores de identidades, Tokens, Protocolos

| Característica | Desarrollo | Vista previa | GA | Notas |
|-------- | :-----------: | :-------: | :--: | ----- |
| IDP-OpenIDConnect |  |  | X | Por ejemplo, Google+.  |
| IDP-OAUTH2 |  |  | X | Por ejemplo, Facebook.  |
| IDP-OAUTH1 (twitter) |  | X |  | Por ejemplo, Twitter. |
| IDP-OAUTH1 (ex-twitter) |  |  |  | No compatible |
| IDP-SAML |  |   | X | Por ejemplo, Salesforce, ADFS. |
| IDP-WSFED | X |  |  |  |
| Usuario de confianza OAUTH1 |  |  |  | No compatible. |
| Usuario de confianza OAUTH2 |  |  | X |  |
| Usuario de confianza OIDC |  |  | X |  |
| Usuario de confianza SAML | X |  |  |  |
| Usuario de confianza WSFED | X |  |  |  |
| API REST con autenticación básica y de certificado |  |  | X | Por ejemplo, Azure Logic Apps. |

### <a name="component-support"></a>Compatibilidad de componentes

| Característica | Desarrollo | Vista previa | GA | Notas |
| ------- | :-----------: | :-------: | :--: | ----- |
| Autenticación multifactor de Azure |  |  | X |  |
| Azure Active Directory como directorio local |  |  | X |  |
| Subsistema de correo electrónico de Azure para la comprobación de correo electrónico |  |  | X |  |
| Compatibilidad con varios lenguajes|  |  | X |  |
| Validaciones de predicado |  |  | X | Por ejemplo, la complejidad de la contraseña. |
| Uso de proveedores de servicios de correo electrónico de terceros | X |  |  |  |

### <a name="content-definition"></a>Definición de contenido

| Característica | Desarrollo | Vista previa | GA | Notas |
| ------- | :-----------: | :-------: | :--: | ----- |
| Página de error, api.error |  |  | X |  |
| Página de selección de IDP, api.idpselections |  |  | X |  |
| Selección de IDP para suscribirse, api.idpselections.signup |  |  | X |  |
| Ha olvidado la contraseña, api.localaccountpasswordreset |  |  | X |  |
| Inicio de sesión en cuenta local, api.localaccountsignin |  |  | X |  |
| Registro en cuenta local, api.localaccountsignup |  |  | X |  |
| Página de MFA, api.phonefactor |  |  | X |  |
| Registro en la cuenta social de autoaserción, api.selfasserted |  |  | X |  |
| Actualización de perfil de autoaserción, api.selfasserted.profileupdate |  |  | X |  |
| Página unificada de registro o inicio de sesión, api.signuporsignin, con el parámetro "disableSignup" |  |  | X |  |
| JavaScript/diseño de página |  | X |  |  |

### <a name="app-ief-integration"></a>Integración de aplicaciones IEF

| Característica | Desarrollo | Vista previa | GA | Notas |
| ------- | :-----------: | :-------: | :--: | ----- |
| Parámetro de cadena de consulta domain_hint |  |  | X | Disponible como notificación, puede pasarse al IDP. |
| Parámetro de cadena de consulta login_hint |  |  | X | Disponible como notificación, puede pasarse al IDP. |
| Insertar JSON en UserJourney a través de client_assertion | X |  |  | En desuso. |
| Insertar JSON en UserJourney como id_token_hint |  | X |  | Enfoque de avance para pasar a JSON. |
| Paso del TOKEN del IDP a la aplicación |  | X |  | Por ejemplo, de Facebook a la aplicación. |

### <a name="session-management"></a>Administración de sesiones

| Característica | Desarrollo | Vista previa | GA | Notas |
| ------- | :-----------: | :-------: | :--: | ----- |
| Proveedor de sesión SSO |  |  | X |  |
| Proveedor de sesión de inicio de sesión externa |  |  | X |  |
| Proveedor de sesión de SSO de SAML |  |  | X |  |
| Proveedor de sesión de SSO predeterminado |  |  | X |  |

### <a name="security"></a>Seguridad

| Característica | Desarrollo | Vista previa | GA | Notas |
|-------- | :-----------: | :-------: | :--: | ----- |
| Claves de directivas: generar, manual, carga |  |  | X |  |
| Claves de directivas: RSA/Certificado, secretos |  |  | X |  |
| Carga de directiva |  |  | X |  |

### <a name="developer-interface"></a>Interfaz del desarrollador

| Característica | Desarrollo | Vista previa | GA | Notas |
| ------- | :-----------: | :-------: | :--: | ----- |
| Azure Portal-IEF UX |  |  | X |  |
| Registros de UserJourney de Application Insights |  | X |  | Se usa para solucionar problemas durante el desarrollo.  |
| Registros de eventos de Application Insights (mediante pasos de orquestación) |  | X |  | Se usa para supervisar flujos de usuario en producción. |

## <a name="next-steps"></a>Pasos siguientes

Obtenga más información sobre las [directivas personalizadas y las diferencias con los flujos de usuario](custom-policy-overview.md).
