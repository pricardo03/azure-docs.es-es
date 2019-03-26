---
title: 'Notas de desarrollador para las directivas personalizadas: Azure Active Directory B2C | Microsoft Docs'
description: Notas para desarrolladores sobre la configuración y el mantenimiento de Azure AD B2C con directivas personalizadas.
services: active-directory-b2c
author: davidmu1
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 03/18/2019
ms.author: davidmu
ms.subservice: B2C
ms.openlocfilehash: cf9c6f6a54c38f00e477e2a9d62e72ab5faccdef
ms.sourcegitcommit: 72cc94d92928c0354d9671172979759922865615
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/25/2019
ms.locfileid: "58418931"
---
# <a name="developer-notes-for-custom-policies-in-azure-active-directory-b2c"></a>Notas de desarrollador para las directivas personalizadas en Azure Active Directory B2C

Configuración de directiva personalizada en Azure Active Directory B2C ya está disponible con carácter general. Este método de configuración está dirigido a los desarrolladores de identidades avanzados crear soluciones complejas de identidad. Las directivas personalizadas de disposición la potencia de Identity Experience Framework en inquilinos de Azure AD B2C. Avanzado de identidades mediante directivas personalizadas de los desarrolladores deben planear invertir un tiempo a completar los tutoriales y leer los documentos de referencia.

Aunque la mayoría de las opciones de directiva personalizada disponibles ahora está disponible con carácter general, hay capacidades subyacentes, como los tipos de perfil técnico y las API que se encuentran en distintas fases del ciclo de vida de software de definición de contenido. Se incorporarán muchos más. En la tabla siguiente especifica el nivel de disponibilidad en un nivel más granular.  

## <a name="features-that-are-generally-available"></a>Características que están disponibles con carácter general

- Crear y cargar recorridos del usuario de autenticación personalizada mediante directivas personalizadas.  
    - Describir los recorridos del usuario paso a paso como intercambios entre proveedores de notificaciones.  
    - Definir la creación de ramas condicional en recorridos del usuario.  
- Interoperar con servicios habilitados para API de REST en los recorridos de usuario de autenticación personalizada.  
- Federación con proveedores de identidades que son compatibles con el protocolo OpenIDConnect.  
- Federación con proveedores de identidades que cumplen el protocolo SAML 2.0.   

## <a name="responsibilities-of-custom-policy-feature-set-developers"></a>Responsabilidades de los desarrolladores de conjunto de características de directivas personalizadas

Configuración manual de directivas concede acceso de nivel inferior a la plataforma subyacente de Azure AD B2C y da como resultado la creación de un marco único, de confianza. Las permutaciones posibles muchos de los proveedores de identidades personalizados, las relaciones de confianza, las integraciones con servicios externos y flujos de trabajo paso a paso requieren un enfoque metódico para diseño y la configuración. 

Los desarrolladores que consumen el conjunto de características de directiva personalizada deben cumplir las siguientes directrices:

- Familiarizarse con el lenguaje de configuración de la administración de directivas y claves/secretos personalizado. Para obtener más información, consulte [TrustFrameworkPolicy](trustframeworkpolicy.md). 
- Tomar posesión de escenarios e integraciones personalizadas. Documentar su trabajo e informar a la organización del sitio activo.  
- Realizar pruebas metódicas de los escenarios. 
- Seguir los procedimientos recomendados de desarrollo de software y almacenamiento provisional con un mínimo de un entorno de desarrollo y prueba, y un entorno de producción. 
- Mantenerse informado sobre los nuevos desarrollos de los proveedores de identidad y los servicios con los que está integrado. Por ejemplo, realizar un seguimiento de cambios en los secretos y los cambios programados y no programados en el servicio. 
- Configurar la supervisión activa y supervisar la capacidad de respuesta de los entornos de producción. Para obtener más información sobre la integración con Application Insights, consulte [Azure Active Directory B2C: Recopilación de registros](active-directory-b2c-custom-guide-eventlogger-appins.md). 
- Mantenga actualizadas las direcciones de correo electrónico de contacto y responda a los correos mensajes de correo electrónico del equipo de Microsoft. 
- Realizar las acciones pertinentes cuando se lo indique el equipo del sitio activo de Microsoft.

## <a name="terms-for-features-in-public-preview"></a>Términos de características en versión preliminar pública

- Le animamos a usar las características de versión preliminar pública solo con fines de evaluación. 
- Acuerdos de nivel de servicio (SLA) no se aplican a las características de versión preliminar pública.
- Solicitudes de soporte técnico para las características de vista previa pública pueden enviarse a través de canales de soporte técnico habituales. 

## <a name="features-by-stage-and-known-issues"></a>Características por fase y problemas conocidos

Funcionalidades de marco de experiencia de identidad/directivas personalizadas están en desarrollo constante y rápido. En la tabla siguiente es un índice de las características y la disponibilidad de componentes.

### <a name="identity-providers-tokens-protocols"></a>Proveedores de identidades, Tokens, Protocolos

| Característica | Desarrollo | Vista previa | GA | Notas |
|-------- | ----------- | ------- | -- | ----- |
| IDP-OpenIDConnect |  |  | X | Por ejemplo, Google +.  |
| IDP-OAUTH2 |  |  | X | Por ejemplo, Facebook.  |
| IDP-OAUTH1 (twitter) |  | X |  | Por ejemplo, Twitter. |
| IDP-OAUTH1 (twitter-ex) |  |  |  | No compatible |
| IDP-SAML |  |   | X | Por ejemplo, Salesforce, ADFS. |
| IDP-WSFED | X |  |  |  |
| Usuario de confianza de terceros OAUTH1 |  |  |  | No compatible. |
| OAUTH2 de entidad de usuario de confianza |  |  | X |  |
| Usuario de confianza OIDC |  |  | X |  |
| Usuario de confianza SAML | X |  |  |  |
| Usuario de confianza WSFED | X |  |  |  |
| API de REST con basic y autenticación de certificado |  |  | X | Por ejemplo, Azure Logic Apps. |

### <a name="component-support"></a>Compatibilidad de componentes

| Característica | Desarrollo | Vista previa | GA | Notas |
| ------- | ----------- | ------- | -- | ----- |
| Autenticación multifactor de Azure |  |  | X |  |
| Azure Active Directory como directorio local |  |  | X |  |
| Subsistema de correo electrónico de Azure para la comprobación de correo electrónico |  |  | X |  |
| Compatibilidad con varios lenguajes|  |  | X |  |
| Validaciones de predicado |  |  | X | Por ejemplo, la complejidad de contraseñas. |
| Uso de proveedores de servicios de correo electrónico de terceros | X |  |  |  |

### <a name="content-definition"></a>Definición de contenido

| Característica | Desarrollo | Vista previa | GA | Notas |
| ------- | ----------- | ------- | -- | ----- |
| Página de error, api.error |  |  | X |  |
| Página de selección de IDP, api.idpselections |  |  | X |  |
| Selección de IDP para suscribirse, api.idpselections.signup |  |  | X |  |
| Ha olvidado la contraseña, api.localaccountpasswordreset |  |  | X |  |
| Inicio de sesión en cuenta local, api.localaccountsignin |  |  | X |  |
| Registro en cuenta local, api.localaccountsignup |  |  | X |  |
| Página de MFA, api.phonefactor |  |  | X |  |
| Cuenta de redes sociales autoafirmada registro, api.selfasserted |  |  | X |  |
| Actualización de perfil de autoaserción, api.selfasserted.profileupdate |  |  | X |  |
| Página de inicio de sesión, api.signuporsignin, con el parámetro "disableSignup" o de registro unificado |  |  | X |  |
| JavaScript / página de contrato |  | X |  |  |

### <a name="app-ief-integration"></a>Integración de aplicaciones IEF

| Característica | Desarrollo | Vista previa | GA | Notas |
| ------- | ----------- | ------- | -- | ----- |
| Parámetro de cadena de consulta domain_hint |  |  | X | Disponible como notificación, puede pasarse al IDP. |
| Parámetro de cadena de consulta login_hint |  |  | X | Disponible como notificación, puede pasarse al IDP. |
| Insertar JSON en UserJourney a través de client_assertion | X |  |  | Dejará de utilizarse. |
| Insertar JSON en UserJourney como id_token_hint |  | X |  | Enfoque de avance para pasar a JSON. |
| Pasar el TOKEN IDP a la aplicación |  | X |  | Por ejemplo, desde Facebook para la aplicación. |

### <a name="session-management"></a>Administración de sesiones

| Característica | Desarrollo | Vista previa | GA | Notas |
| ------- | ----------- | ------- | -- | ----- |
| Proveedor de sesión SSO |  |  | X |  |
| Proveedor de sesión de inicio de sesión externa |  |  | X |  |
| Proveedor de sesión de SSO de SAML |  |  | X |  |
| Proveedor de sesión de inicio de sesión único de forma predeterminada |  |  | X |  |

### <a name="security"></a>Seguridad

| Característica | Desarrollo | Vista previa | GA | Notas |
|-------- | ----------- | ------- | -- | ----- |
| Claves de directivas: generar, manual, carga |  |  | X |  |
| Claves de directivas: RSA/Certificado, secretos |  |  | X |  |
| Carga de directiva |  |  | X |  |

### <a name="developer-interface"></a>Interfaz del desarrollador

| Característica | Desarrollo | Vista previa | GA | Notas |
| ------- | ----------- | ------- | -- | ----- |
| Azure Portal-IEF UX |  |  | X |  |
| Registros de UserJourney de Application Insights |  | X |  | Se usa para solucionar problemas durante el desarrollo.  |
| Registros de eventos de Application Insights (a través de los pasos de orquestación) |  | X |  | Se usa para supervisar flujos de usuario en producción. |

## <a name="next-steps"></a>Pasos siguientes
[Más información sobre las directivas personalizadas y las diferencias con los flujos de usuario](active-directory-b2c-overview-custom.md).
