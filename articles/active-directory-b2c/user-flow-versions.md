---
title: Versiones de flujos de usuario de Azure Active Directory B2C | Microsoft Docs
description: Obtenga información sobre las versiones de los flujos de usuario disponibles en Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/09/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: c7b3ce6a4e72b90c6fd642b92d4a92eb02f4c92c
ms.sourcegitcommit: f209d0dd13f533aadab8e15ac66389de802c581b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/17/2019
ms.locfileid: "71063195"
---
# <a name="user-flow-versions-in-azure-active-directory-b2c"></a>Versiones de flujos de usuario de Azure Active Directory B2C

>[!IMPORTANT]
> Se considera que todos los flujos de usuario que aparecen están en versión preliminar pública, a menos que estén identificados como **recomendados**. Solo debe utilizar flujos de usuario recomendados en las aplicaciones de producción.

Los flujos de usuario de Azure Active Directory B2C (Azure AD B2C) le permiten configurar [directivas](active-directory-b2c-reference-policies.md) que describan de forma exhaustiva la experiencia de los clientes con las identidades. Estas experiencias incluyen las operaciones de registro, inicio de sesión, restablecimiento de contraseña y edición de perfiles. En Azure AD B2C, puede seleccionar un flujo de usuario entre una colección de flujos de usuario recomendados y de flujos de usuario en versión preliminar.

Los nuevos flujos de usuario se agregan como versiones nuevas. Cuando un flujo de usuario se hace estable, se recomienda para su uso. Los flujos de usuario se identifican como **recomendados** si se han probado exhaustivamente. Se considera que los flujos de usuario están en versión preliminar hasta que se identifican como recomendados. Utilice un flujo de usuario recomendado en las aplicaciones de producción, pero seleccione otra versión para probar las nuevas funcionalidades cuando estén disponibles. No debe usar versiones anteriores de los flujos de usuario recomendados.

## <a name="v1"></a>V1

| Flujo de usuario | Recomendado | DESCRIPCIÓN |
| --------- | ----------- | ----------- |
| Restablecimiento de contraseña | Sí | Permite a los usuarios elegir una contraseña nueva después de verificar el correo electrónico. Con este flujo de usuario, puede configurar: <ul><li>[Multi-factor Authentication](active-directory-b2c-reference-mfa.md)</li><li>Configuración de compatibilidad de tokens</li><li>[Requisitos de complejidad de la contraseña](active-directory-b2c-reference-password-complexity.md)</li></ul> |
| Edición de perfiles | Sí | Permite al usuario configurar los atributos de usuario. Con este flujo de usuario, puede configurar: <ul><li>[Vigencia de tokens](active-directory-b2c-reference-tokens.md)</li><li>Configuración de compatibilidad de tokens</li><li>Comportamiento de la sesión</li></ul> |
| Propietario del recurso | Sin | Permite a los usuarios con una cuenta local iniciar sesión directamente en las aplicaciones nativas (no se necesita un explorador). Con este flujo de usuario, puede configurar: <ul><li>[Vigencia de tokens](active-directory-b2c-reference-tokens.md)</li><li>Configuración de compatibilidad de tokens</li></ul> |
| Iniciar sesión | Sin | Permite a los usuarios iniciar sesión en sus cuentas. Con este flujo de usuario, puede configurar: <ul><li>[Multi-factor Authentication](active-directory-b2c-reference-mfa.md)</li><li>[Vigencia de tokens](active-directory-b2c-reference-tokens.md)</li><li>Configuración de compatibilidad de tokens</li><li>Comportamiento de la sesión</li><li>Bloqueo de inicios de sesión</li><li>Forzar el restablecimiento de contraseñas</li><li>Mantener la sesión iniciada (KMSI)</ul><br>No se puede personalizar la interfaz de usuario con este flujo de usuario. |
| Suscripción | Sin | Permite a los usuarios crear una cuenta. Con este flujo de usuario, puede configurar: <ul><li>[Multi-factor Authentication](active-directory-b2c-reference-mfa.md)</li><li>[Vigencia de tokens](active-directory-b2c-reference-tokens.md)</li><li>Configuración de compatibilidad de tokens</li><li>Comportamiento de la sesión</li><li>[Requisitos de complejidad de la contraseña](active-directory-b2c-reference-password-complexity.md)</li></ul> |
| Registrarse e iniciar sesión | Sí | Permite a los usuarios crear una cuenta o iniciar sesión en sus cuentas. Con este flujo de usuario, puede configurar: <ul><li>[Multi-factor Authentication](active-directory-b2c-reference-mfa.md)</li><li>[Vigencia de tokens](active-directory-b2c-reference-tokens.md)</li><li>Configuración de compatibilidad de tokens</li><li>Comportamiento de la sesión</li><li>[Requisitos de complejidad de la contraseña](active-directory-b2c-reference-password-complexity.md)</li></ul>|

## <a name="v2"></a>V2

| Flujo de usuario | Recomendado | DESCRIPCIÓN |
| --------- | ----------- | ----------- |
| Restablecimiento de contraseña v2 | Sin | Permite a los usuarios elegir una contraseña nueva después de verificar el correo electrónico. Con este flujo de usuario, puede configurar: <ul><li>[Multi-factor Authentication](active-directory-b2c-reference-mfa.md)</li><li>Configuración de compatibilidad de tokens</li><li>[Restricción de acceso por edad](basic-age-gating.md)</li><li>[Requisitos de complejidad de la contraseña](active-directory-b2c-reference-password-complexity.md)</li></ul> |
| Inicio de sesión v2 | Sin | Permite a los usuarios iniciar sesión en sus cuentas. Con este flujo de usuario, puede configurar: <ul><li>[Multi-factor Authentication](active-directory-b2c-reference-mfa.md)</li><li>[Vigencia de tokens](active-directory-b2c-reference-tokens.md)</li><li>Configuración de compatibilidad de tokens</li><li>Comportamiento de la sesión</li><li>[Restricción de acceso por edad](basic-age-gating.md)</li><li>Personalización de la página de inicio de sesión</li></ul> |
| Registrarse v2 | Sin | Permite a los usuarios crear una cuenta. Con este flujo de usuario, puede configurar: <ul><li>[Multi-factor Authentication](active-directory-b2c-reference-mfa.md)</li><li>[Vigencia de tokens](active-directory-b2c-reference-tokens.md)</li><li>Configuración de compatibilidad de tokens</li><li>Comportamiento de la sesión</li><li>[Restricción de acceso por edad](basic-age-gating.md)</li><li>[Requisitos de complejidad de la contraseña](active-directory-b2c-reference-password-complexity.md)</li></ul> |
| Registrarse e iniciar sesión v2 | Sin | Permite a los usuarios crear una cuenta o iniciar sesión en sus cuentas. Con este flujo de usuario, puede configurar: <ul><li>[Multi-factor Authentication](active-directory-b2c-reference-mfa.md)</li><li>[Restricción de acceso por edad](basic-age-gating.md)</li><li>[Requisitos de complejidad de la contraseña](active-directory-b2c-reference-password-complexity.md)</li></ul> |
