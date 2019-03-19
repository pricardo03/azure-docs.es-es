---
title: Inicio de sesión único en SAML para aplicaciones locales con Azure Active Directory Application Proxy (versión preliminar) | Microsoft Docs
description: Aprenda a proporcionar inicio de sesión único en local las aplicaciones publican a través de Proxy de aplicación que están protegidas con autenticación de SAML.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: celested
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: ef5679ba2a6a62955f5402e8bfaa4f1884df722d
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57840603"
---
# <a name="saml-single-sign-on-for-on-premises-applications-with-application-proxy-preview"></a>Inicio de sesión único en SAML para aplicaciones locales con el Proxy de aplicación (versión preliminar)

Puede proporcionar el inicio de sesión único (SSO) para aplicaciones locales que están protegidas con autenticación de SAML y proporcionan acceso remoto a estas aplicaciones a través del Proxy de aplicación. Con SAML single sign-on, Azure Active Directory (Azure AD) se autentica en la aplicación mediante la cuenta de Azure AD del usuario. Azure AD comunica la información de inicio de sesión a la aplicación a través de un protocolo de conexión. También puede asignar usuarios a roles de aplicación específicos en función de reglas definidas en sus notificaciones SAML. Habilitando el Proxy de aplicación además de SAML SSO, los usuarios tendrán acceso externo a la aplicación y una experiencia de inicio de sesión único.

Las aplicaciones deben ser capaz de consumir tokens SAML emitidos por **Azure Active Directory**. Esta configuración no es aplicable a las aplicaciones que usan un proveedor de identidades en el entorno local. Para estos escenarios, se recomienda revisar [los recursos para migrar aplicaciones a Azure AD](migration-resources.md).

SAML SSO con el Proxy de aplicación también funciona con la característica de cifrado de tokens SAML. Para obtener más información, consulte [cifrado de tokens SAML de Azure AD Configure](howto-saml-token-encryption.md).

## <a name="publish-the-on-premises-application-with-application-proxy"></a>Publicar la aplicación en el entorno local con el Proxy de aplicación

Antes de poder proporcionar SSO para aplicaciones locales, asegúrese de que ha habilitado al Proxy de aplicación y tiene instalado un conector. Consulte [agregar una aplicación de forma local para el acceso remoto a través del Proxy de aplicación en Azure AD](application-proxy-add-on-premises-application.md) para obtener información sobre cómo.

Cuando vas a través de este tutorial, tenga en cuenta lo siguiente:

* Publique la aplicación según las instrucciones del tutorial. Asegúrese de seleccionar **Azure Active Directory** como el **autenticación previa** método de la aplicación (paso 4 [agregar una aplicación local a Azure AD](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad
)).
* Copia el **dirección URL externa** para la aplicación.
* Como práctica recomendada, use dominios personalizados siempre que sea posible para una experiencia de usuario optimizada. Obtenga más información sobre [trabajar con dominios personalizados en Azure AD Application Proxy](application-proxy-configure-custom-domain.md).
* Agregue al menos un usuario a la aplicación y asegúrese de que la cuenta de prueba tenga acceso a la aplicación en el entorno local.

## <a name="set-up-saml-sso"></a>Configuración de SAML SSO

1. En el portal de Azure, seleccione **Azure Active Directory > aplicaciones empresariales** y seleccione la aplicación en la lista.
1. Desde la aplicación **Introducción** página, seleccione **inicio de sesión único**.
1. Seleccione **SAML** como el método de inicio de sesión único.
1. En el **establecer seguridad de sesión único con SAML** página, edite el **configuración básica de SAML** datos y siga los pasos descritos en [configuración básica de SAML de ENTRAR](configure-single-sign-on-non-gallery-applications.md#saml-based-single-sign-on) configurar basado en SAML autenticación para la aplicación.

   * Asegúrese de que el **dirección URL de respuesta** raíz coincide o es una ruta de acceso bajo la **dirección URL externa** para la aplicación en el entorno local que agregó para el acceso remoto a través del Proxy de aplicación en Azure AD.

     ![Escriba los datos de configuración básicas de SAML](./media/application-proxy-configure-single-sign-on-on-premises-apps/basic-saml-configuration.png)

     > [!NOTE]
     > Si la aplicación de back-end espera el **dirección URL de respuesta** para que sea la dirección URL interna, necesita instalar la extensión de inicio de sesión segura de mis aplicaciones en dispositivos de los usuarios. Esta extensión le redireccionará automáticamente para el servicio de Proxy de aplicación adecuado. Para instalar la extensión, consulte [extensión de inicio de sesión seguro de mis aplicaciones](../user-help/active-directory-saas-access-panel-introduction.md#my-apps-secure-sign-in-extension).

## <a name="test-your-app"></a>Prueba de la aplicación

Cuando haya completado todos los pasos, la aplicación debería estar en funcionamiento. Para probar la aplicación:

1. Abra un explorador y vaya a la dirección URL externa que creó al publicar la aplicación. 
1. Inicie sesión con la cuenta de prueba que asignó a la aplicación.

## <a name="next-steps"></a>Pasos siguientes

- [¿Cómo permite el proxy de aplicación de Azure AD el inicio de sesión único?](application-proxy-single-sign-on.md)
- [Solucionar problemas de Proxy de aplicación](application-proxy-troubleshoot.md)
