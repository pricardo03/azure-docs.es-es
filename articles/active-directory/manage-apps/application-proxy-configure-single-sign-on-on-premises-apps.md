---
title: Inicio de sesión único de SAML para aplicaciones en el entorno local con Azure Active Directory Application Proxy (versión preliminar) | Microsoft Docs
description: Aprenda a proporcionar un inicio de sesión único para las aplicaciones en el entorno local publicadas con Application Proxy que se protegen con la autenticación de SAML.
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/20/2019
ms.author: mimart
ms.reviewer: japere
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 907cb598d708bfa26f53d2e43fef5456258c21b1
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "66393045"
---
# <a name="saml-single-sign-on-for-on-premises-applications-with-application-proxy-preview"></a>Inicio de sesión único de SAML para aplicaciones en el entorno local con Application Proxy (versión preliminar)

Puede proporcionar el inicio de sesión único (SSO) para las aplicaciones en el entorno local que están protegidas con la autenticación de SAML y acceso remoto a estas aplicaciones mediante Application Proxy. Con el inicio de sesión único de SAML, Azure Active Directory (Azure AD) se autentica en la aplicación mediante el uso de la cuenta de Azure AD del usuario. Azure AD comunica la información de inicio de sesión a la aplicación a través de un protocolo de conexión. También puede asignar los usuarios a roles de aplicación específicos según las reglas que defina en las notificaciones de SAML. Al habilitar Application Proxy además de SSO de SAML, los usuarios tendrán acceso externo a la aplicación y una experiencia fluida de inicio de sesión único.

Las aplicaciones deben ser capaces de consumir los tokens SAML emitidos por **Azure Active Directory**. Esta configuración no es aplicable a las aplicaciones que usan un proveedor de identidades en el entorno local. Para estos escenarios, se recomienda revisar los [recursos para migrar aplicaciones a Azure AD](migration-resources.md).

SSO de SAML con Application Proxy también funciona con la característica de cifrado de tokens de SAML. Para más información, consulte [Configuración del cifrado de tokens SAML de Azure AD](howto-saml-token-encryption.md).

## <a name="publish-the-on-premises-application-with-application-proxy"></a>Publicación de la aplicación en el entorno local con Application Proxy

Para proporcionar SSO para aplicaciones en el entorno local, asegúrese de habilitar Application Proxy y de tener instalado un conector. Consulte [Adición de una aplicación local para el acceso remoto mediante Application Proxy en Azure Active Directory](application-proxy-add-on-premises-application.md) para más información.

Tenga en cuenta lo siguiente cuando consulte el tutorial:

* Publique la aplicación según las instrucciones del tutorial. Asegúrese de seleccionar **Azure Active Directory** como el método de **autenticación previa** de la aplicación (paso 4 de [Incorporación de una aplicación local a Azure AD](application-proxy-add-on-premises-application.md#add-an-on-premises-app-to-azure-ad
)).
* Copie la **dirección URL externa** para la aplicación.
* Como procedimiento recomendado, use dominios personalizados siempre que sea posible para obtener la mejor experiencia de usuario. Más información acerca del [Uso de dominios personalizados en Azure AD Application Proxy](application-proxy-configure-custom-domain.md).
* Agregue al menos un usuario a la aplicación y asegúrese de que la cuenta de prueba tenga acceso a la aplicación en el entorno local. Mediante la cuenta de prueba, puede probar si es posible conectarse a la aplicación; para ello, visite la **dirección URL externa** para comprobar si Application Proxy se ha configurado correctamente. Para información de solución de problemas, consulte [Solución de problemas y mensajes de error de Application Proxy](application-proxy-troubleshoot.md).

## <a name="set-up-saml-sso"></a>Configuración de SSO de SAML

1. En Azure Portal, seleccione **Azure Active Directory > Aplicaciones empresariales** y, después, la aplicación en la lista.
1. Desde la página **Introducción** de la aplicación, seleccione **Inicio de sesión único**.
1. Seleccione **SAML** como método de inicio de sesión único.
1. En la página **Configuración del inicio de sesión único con SAML**, edite los datos de **Configuración básica de SAML** y siga los pasos descritos en [Enter basic SAML configuration](configure-single-sign-on-non-gallery-applications.md#saml-based-single-sign-on) (Escribir la configuración básica de SAML) para configurar la autenticación basada en SAML en la aplicación.

   * Asegúrese de que la **dirección URL de respuesta** coincide con la **dirección URL externa** para la aplicación local que publicó con Application Proxy o que es una ruta de acceso debajo de la **dirección URL externa**.
   * Con un flujo iniciado por IDP, donde la aplicación requiere una **dirección URL de respuesta** diferente para la configuración de SAML, agregue este elemento como una dirección URL **adicional** en la lista y marque la casilla situada junto a ella para designarla como la **dirección URL de respuesta** principal.
   * Con un flujo iniciado por SP, asegúrese de que la aplicación de back-end especifica el valor de la **dirección URL de respuesta** correcto o la dirección URL del servicio de consumidor de aserciones que se utilizará para recibir el token de autenticación.

     ![Especificación de la configuración básica de SAML](./media/application-proxy-configure-single-sign-on-on-premises-apps/basic-saml-configuration.png)

    > [!NOTE]
    > Si la aplicación de back-end espera que la **dirección URL de respuesta** sea la dirección URL interna, necesitará usar [dominios personalizados](application-proxy-configure-custom-domain.md) para tener direcciones URL internas y externas que coincidan o instalar la extensión de inicio de inicio de sesión segura de Mis aplicaciones en los dispositivos de los usuarios. Esta extensión le redireccionará automáticamente al servicio Application Proxy adecuado. Para instalar la extensión, consulte [	Extensión de inicio de sesión seguro de mis aplicaciones](../user-help/my-apps-portal-end-user-access.md#download-and-install-the-my-apps-secure-sign-in-extension).

## <a name="test-your-app"></a>Prueba de la aplicación

Cuando haya completado todos los pasos, la aplicación debería estar en funcionamiento. Para probar la aplicación:

1. Abra un explorador y vaya a la dirección URL externa que creó al publicar la aplicación. 
1. Inicie sesión con la cuenta de prueba que asignó a la aplicación. Debe ser capaz de cargar la aplicación y tener el inicio de sesión único en la aplicación.

## <a name="next-steps"></a>Pasos siguientes

- [¿Cómo permite el proxy de aplicación de Azure AD el inicio de sesión único?](application-proxy-single-sign-on.md)
- [Solucionar problemas de Proxy de aplicación](application-proxy-troubleshoot.md)
