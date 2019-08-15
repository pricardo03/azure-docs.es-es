---
title: Referencia de cambios importantes en Azure Active Directory B2C | Microsoft Docs
description: Obtenga información sobre los cambios realizados en los protocolos de Azure AD que pueden afectar a la aplicación.
services: active-directory
documentationcenter: ''
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 68517c83-1279-4cc7-a7c1-c7ccc3dbe146
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/26/2019
ms.author: ryanwi
ms.reviewer: hirsin
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: 38383685f74020f5208d42df4428f896931fbe2a
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2019
ms.locfileid: "68931784"
---
# <a name="whats-new-for-authentication"></a>Novedades en la autenticación 

>Reciba notificaciones de las actualizaciones a esta página. Basta con agregar [esta dirección URL](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20for%20authentication%22&locale=en-us) al lector de fuentes RSS.

El sistema de autenticación altera y agrega características constantemente para aumentar la seguridad y mejorar el cumplimiento de normas. Para mantenerse al día con los avances más recientes, este artículo proporciona información acerca de los detalles siguientes:

- Características más recientes
- Problemas conocidos
- Cambios en el protocolo
- Funciones obsoletas

> [!TIP] 
> Esta página se actualiza regularmente, por lo que se recomienda visitarla a menudo. Salvo que se indique lo contrario, estos cambios solo entrarán en vigor en las aplicaciones recién registradas.  

## <a name="upcoming-changes"></a>Próximos cambios

Agosto de 2019: Se aplicará la semántica de POST de acuerdo con las reglas de análisis de direcciones URL: los parámetros duplicados desencadenarán un error, las comillas entre los parámetros ya no se omitirán y se omitirá la marca [BOM](https://www.w3.org/International/questions/qa-byte-order-mark).

## <a name="july-2019"></a>Julio de 2019

### <a name="app-only-tokens-for-single-tenant-applications-are-only-issued-if-the-client-app-exists-in-the-resource-tenant"></a>Los tokens solo de aplicación para aplicaciones de inquilino único solo se emiten si la aplicación cliente existe en el inquilino de recursos.

**Fecha efectiva**: 26 de julio de 2019

**Puntos de conexión afectados**: [v1.0](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow) y [v2.0](https://docs.microsoft.com/azure/active-directory/develop/v2-oauth2-client-creds-grant-flow)

**Protocolo afectado**: [credenciales de cliente (tokens de solo aplicación)](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow)

El 26 de julio se aplicó un cambio de seguridad que modifica la manera en la que se emiten los tokens de solo aplicación (a través de la concesión de credenciales de cliente). Anteriormente, las aplicaciones podían obtener tokens para llamar a cualquier otra aplicación, sin tener en cuenta su presencia en el inquilino o los roles con consentimiento para esa aplicación.  Este comportamiento se ha actualizado de modo que, en el caso de los recursos (a veces denominados API web) configurados para ser inquilino único (el valor predeterminado), la aplicación cliente debe existir en el inquilino de recursos.  Tenga en cuenta que aún no se requiere que exista consentimiento entre el cliente y la API, y que las aplicaciones todavía deben realizar sus propias comprobaciones de autorización para asegurarse de que existe una notificación `roles` y de que contiene el valor esperado para la API.

El mensaje de error de este escenario indica actualmente: 

`The service principal named <appName> was not found in the tenant named <tenant_name>. This can happen if the application has not been installed by the administrator of the tenant.`

Para solucionar este problema, use la experiencia de consentimiento del administrador con el fin de crear la entidad de servicio de la aplicación cliente en el inquilino o bien créela manualmente.  Este requisito garantiza que el inquilino ha dado permiso a la aplicación para que opere dentro del inquilino.  

#### <a name="example-request"></a>Solicitud de ejemplo

`https://login.microsoftonline.com/contoso.com/oauth2/authorize?resource=https://gateway.contoso.com/api&response_type=token&client_id=14c88eee-b3e2-4bb0-9233-f5e3053b3a28&...` En este ejemplo, el inquilino de recursos (autoridad) es contoso.com, la aplicación de recursos es una aplicación de un solo inquilino denominada `gateway.contoso.com/api` para el inquilino de Contoso y la aplicación cliente es `14c88eee-b3e2-4bb0-9233-f5e3053b3a28`.  Si la aplicación cliente tiene una entidad de servicio en Contoso.com, esta solicitud puede continuar.  Si no la tiene, se producirá el error anterior en la solicitud.  

Si la aplicación de puerta de enlace de Contoso fuera una aplicación de varios inquilinos, la solicitud continuaría, independientemente de que la aplicación cliente tuviera o no una entidad de servicio en Contoso.com.  

### <a name="redirect-uris-can-now-contain-query-string-parameters"></a>Los identificadores URI de redireccionamiento ahora pueden contener parámetros de cadena de consulta.

**Fecha efectiva**: 22 de julio de 2019

**Puntos de conexión afectados**: v1.0 y v2.0

**Protocolo afectado**: Todos los flujos

Según la solicitud [RFC 6749](https://tools.ietf.org/html/rfc6749#section-3.1.2), las aplicaciones de Azure AD ahora pueden registrar y usar identificadores URI de redireccionamiento (respuesta) con parámetros de consulta estáticos (como https://contoso.com/oauth2?idp=microsoft) ) para solicitudes de OAuth 2.0.  Los identificadores URI de redireccionamiento dinámico siguen estando prohibidos, ya que representan un riesgo de seguridad, y no se pueden usar para conservar la información de estado a través de una solicitud de autenticación; para ello, use el parámetro `state`.

El parámetro de consulta estático está sujeto a la coincidencia de cadenas para los identificadores URI de redireccionamiento, al igual que cualquier otra parte del URI de redireccionamiento; si no hay ninguna cadena registrada que coincida con el parámetro redirect_uri descodificado con URI, se rechazará la solicitud.  Si se encuentra el identificador URI en el registro de la aplicación, se usará toda la cadena para redirigir al usuario, incluido el parámetro de consulta estático. 

Tenga en cuenta que en este momento (final de julio de 2019), la experiencia de usuario del registro de aplicaciones en Azure Portal sigue bloqueando los parámetros de consulta.  Sin embargo, puede editar el manifiesto de aplicación manualmente para agregar parámetros de consulta y probar este procedimiento en la aplicación.  


## <a name="march-2019"></a>Marzo de 2019

### <a name="looping-clients-will-be-interrupted"></a>Se interrumpirán los clientes en bucle

**Fecha efectiva**: 25 de marzo de 2019

**Puntos de conexión afectados**: v1.0 y v2.0

**Protocolo afectado**: Todos los flujos

En ocasiones, las aplicaciones cliente pueden comportarse de manera incorrecta, emitiendo cientos de veces la misma solicitud de inicio de sesión durante un breve período de tiempo.  Estas solicitudes pueden completarse satisfactoriamente o no, pero todas contribuyen a una deficiente experiencia para el usuario y a un incremento de las cargas de trabajo para el IDP, lo que aumenta la latencia para todos los usuarios y reduce la disponibilidad del IDP.  Estas aplicaciones operan fuera de los límites de uso normal y deben actualizarse para que se comporten correctamente.  

A los clientes que emitan solicitudes duplicadas varias veces se les enviará un error `invalid_grant`: `AADSTS50196: The server terminated an operation because it encountered a loop while processing a request`. 

La mayoría de los clientes no tendrán que cambiar su comportamiento para evitar este error.  Solo los clientes incorrectamente configurados (aquellos sin almacenamiento en caché de tokens o los que ya presentan bucles de mensajes) se verán afectados por este error.  Se realiza un seguimiento de los clientes por instancia a nivel local (mediante cookie) sobre los siguientes factores:

* Sugerencia de usuario, si existe

* Ámbitos o recursos solicitados

* Id. de cliente

* URI de redireccionamiento

* Tipo y modo de respuesta

Las aplicaciones que realizan varias solicitudes (más de 15) en un breve período de tiempo (5 minutos) recibirán un error `invalid_grant` que explica que están en bucle.  Los tokens solicitados tienen una vigencia de una duración suficiente (10 minutos como mínimo, 60 minutos de forma predeterminada), por lo que las solicitudes repetidas durante este período de tiempo son innecesarias.  

Todas las aplicaciones deben controlar `invalid_grant` mostrando un aviso interactivo, en lugar de solicitar un token de forma silenciosa.  Para evitar este error, los clientes deberían asegurarse de que almacenan en caché correctamente los tokens que reciben.


## <a name="october-2018"></a>Octubre de 2018

### <a name="authorization-codes-can-no-longer-be-reused"></a>No se podrán reutilizar los códigos de autorización

**Fecha efectiva**: 15 de noviembre de 2018

**Puntos de conexión afectados**: v1.0 y v2.0

**Protocolo afectado**: [flujo de código](v2-oauth2-auth-code-flow.md)

A partir del 15 de noviembre de 2018, Azure AD dejará de aceptar los códigos de autenticación que se usaban anteriormente para las aplicaciones. Este cambio de seguridad ayuda a poner Azure AD en consonancia con la especificación de OAuth y se aplicará en los puntos de conexión v1 y v2.

Si la aplicación reutiliza códigos de autorización para obtener tokens para varios recursos, es recomendable que use el código para obtener un token de actualización y, a continuación, utilice este para adquirir tokens adicionales para otros recursos. Los códigos de autorización solo se pueden usar una vez, pero los tokens de actualización se pueden usar varias veces en varios recursos. Cualquier nueva aplicación que intente reutilizar un código de autenticación durante el flujo de código de OAuth obtendrá el error invalid_grant.

Para más información acerca de los tokens de actualización, consulte [Actualización de los tokens de acceso](v1-protocols-oauth-code.md#refreshing-the-access-tokens).  Si usa ADAL o MSAL, la biblioteca lo controla automáticamente. Sustituya la segunda instancia de 'AcquireTokenByAuthorizationCodeAsync' por 'AcquireTokenSilentAsync'. 

## <a name="may-2018"></a>Mayo de 2018

### <a name="id-tokens-cannot-be-used-for-the-obo-flow"></a>No se puede usar los tokens de identificador para el flujo de OBO

**Fecha**: 1 de mayo de 2018

**Puntos de conexión afectados**: v1.0 y v2.0

**Protocolos afectados**: flujo implícito y [flujo OBO](v1-oauth2-on-behalf-of-flow.md)

A partir del 1 de mayo de 2018, id_tokens no se puede utilizar como instrucción de aserción en un flujo de OBO en las aplicaciones nuevas. En su lugar deben usarse tokens de acceso para proteger las API, incluso entre un cliente y el nivel intermedio de la misma aplicación. Las aplicaciones registradas antes del 1 de mayo de 2018 seguirán funcionando y podrán intercambiar id_tokens por un token de acceso (pero tenga en cuenta que este patrón no se considera un procedimiento recomendado).

Para ofrecer una solución alternativa al problema que pueda provocar este cambio, puede hacer lo siguiente:

1. Cree una API web para la aplicación con uno o más ámbitos. Este punto de entrada explícito le permitirá mayor control y seguridad.
1. En el manifiesto de la aplicación, [Azure Portal](https://portal.azure.com) o el [portal de registro de aplicaciones](https://apps.dev.microsoft.com), asegúrese de que la aplicación tiene permiso para emitir tokens de acceso mediante el flujo implícito, lo que se controla mediante la clave `oauth2AllowImplicitFlow`.
1. Cuando la aplicación cliente solicita un id_token a través de `response_type=id_token`, también solicita un token de acceso (`response_type=token`) para la API web creada anteriormente. Por consiguiente, cuando se usa el punto de conexión v2.0 el parámetro `scope` debe ser similar a `api://GUID/SCOPE`. En el punto de conexión v1.0, el parámetro `resource` debe ser el identificador URI de la aplicación de la API web.
1. Pase este token de acceso al nivel intermedio en lugar de id_token.  
