---
title: Exposición de una aplicación en la galería de aplicaciones de Azure Active Directory | Microsoft Docs
description: Información sobre cómo mostrar una aplicación compatible con el inicio de sesión único en la galería de aplicaciones de Azure Active Directory
services: active-directory
documentationcenter: dev-center-name
author: rwike77
manager: CelesteDG
editor: ''
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.subservice: develop
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/16/2019
ms.author: ryanwi
ms.reviewer: elisol, bryanla
ms.custom: aaddev, seoapril2019
ms.collection: M365-identity-device-management
ms.openlocfilehash: bd20928816a30ad2843ce6c876bd77152dcd69e1
ms.sourcegitcommit: be344deef6b37661e2c496f75a6cf14f805d7381
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/07/2019
ms.locfileid: "72001739"
---
# <a name="list-your-application-in-the-azure-active-directory-application-gallery"></a>Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory

En este artículo se explica el proceso para mostrar una aplicación en la galería de aplicaciones de Azure Active Directory (Azure AD), implementar el inicio de sesión único (SSO) y administrar la aplicaciones mostradas.

## <a name="what-is-the-azure-ad-application-gallery"></a>¿Qué es la galería de aplicaciones de Azure AD?

- Los clientes pueden disfrutar de la mejor experiencia posible de inicio de sesión único.
- La configuración de la aplicación resulta sencilla y apenas requiere esfuerzo.
- Basta realizar una búsqueda rápida para encontrar la aplicación en la galería.
- Todos los clientes de Azure AD de los niveles Gratis, Básico y Premium pueden disfrutar de esta integración.
- Los clientes mutuos pueden utilizar un tutorial de configuración paso a paso.
- Los clientes que usan el sistema para la administración de identidades entre dominios ([SCIM](https://techcommunity.microsoft.com/t5/Identity-Standards-Blog/Provisioning-with-SCIM-getting-started/ba-p/880010)) pueden usar el aprovisionamiento para la misma aplicación.

## <a name="prerequisites"></a>Requisitos previos

- Para las aplicaciones federadas (Open ID y SAML/WS-Fed), la aplicación debe admitir el modelo de software como servicio (SaaS) para que se incluya en la galería de Azure AD. Las aplicaciones de la galería empresarial tienen que admitir varias configuraciones de cliente y no la de un cliente específico.
- Para OpenID Connect, la aplicación debe ser de varios inquilinos y para ella se debe implementar correctamente el [marco de consentimiento de Azure AD](consent-framework.md). El usuario puede enviar la solicitud de inicio de sesión a un punto de conexión común para que los clientes puedas proporcionar su consentimiento a la aplicación. Puede controlar el acceso de usuario en función del identificador del inquilino y el UPN del usuario que se recibieron en el token.
- Para SAML 2.0 o WS-Fed, la aplicación debe ser capaz de realizar la integración de SSO de SAML o WS-Fed en modo SP o IDP. Asegúrese de que esta funcionalidad funciona correctamente antes de enviar la solicitud.
- Para el SSO con contraseña, asegúrese de que la aplicación admita la autenticación por formulario para que se pueda realizar el almacenamiento de contraseña y hacer que el inicio de sesión único funcione del modo previsto.
- Necesita una cuenta permanente para las pruebas con al menos dos usuarios registrados.

## <a name="submit-the-request-in-the-portal"></a>Envío de la solicitud en el portal

Cuando haya comprobado que la integración de aplicaciones funciona con Azure AD, envíe la solicitud de acceso en el [portal de red de aplicaciones](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Si tiene una cuenta de Office 365, utilícela para iniciar sesión en este portal. De lo contrario, utilice una cuenta Microsoft, como Outlook o Hotmail, para iniciar sesión.

Si aparece la siguiente página después de iniciar sesión, póngase en contacto con el [equipo de integración de SSO de Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>). Proporcione la cuenta de correo electrónico que desea usar para enviar la solicitud. A continuación, el equipo de Azure AD agregará la cuenta en el portal de redes de aplicaciones de Microsoft.

![Mensaje de solicitud de acceso en el portal de SharePoint](./media/howto-app-gallery-listing/errorimage.png)

Después de agregar la cuenta, puede iniciar sesión en el portal de redes de aplicaciones de Microsoft.

Si aparece la siguiente página después de iniciar sesión, escriba en el cuadro de texto la justificación empresarial por la que necesita obtener acceso. Después, seleccione **Solicitar acceso**.

  ![Cuadro de justificación empresarial en el portal de SharePoint](./media/howto-app-gallery-listing/accessrequest.png)

Nuestro equipo revisará los detalles y le proporcionará el acceso según corresponda. Una vez aprobada la solicitud, puede iniciar sesión en el portal y enviar la solicitud. Para ello, seleccione el icono **Enviar solicitud (ISV)** en la página principal.

![Icono Enviar solicitud (ISV) en la página principal](./media/howto-app-gallery-listing/homepage.png)

> [!NOTE]
> Si tiene algún problema con el acceso, póngase en contacto con el [equipo de integración del SSO de Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="implement-sso-by-using-the-federation-protocol"></a>Implementación del inicio de sesión único mediante el protocolo de federación

Para agregar una aplicación a la galería de Azure AD, debe implementar uno de los siguientes protocolos de federación compatibles con Azure AD. También necesita aceptar los términos y condiciones de la galería de aplicaciones de Azure AD. Lea los términos y condiciones de la galería de aplicaciones de Azure AD [aquí](https://azure.microsoft.com/support/legal/active-directory-app-gallery-terms/).

- **OpenID Connect**: para integrar la aplicación con Azure AD mediante el protocolo OpenID Connect, siga las [instrucciones de los desarrolladores](authentication-scenarios.md).

    ![Adición de una aplicación de OpenID Connect a la lista de la galería](./media/howto-app-gallery-listing/openid.png)

    * Si desea agregar la aplicación a la lista en la galería con OpenID Connect, seleccione **OpenID Connect & OAuth 2.0** (OAuth 2.0 y OpenID Connect) como se muestra.
    * Si tiene algún problema con el acceso, póngase en contacto con el [equipo de integración del SSO de Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

- **SAML 2.0** o **WS-Fed**: si la aplicación es compatible con SAML 2.0, se puede integrar directamente con un inquilino de Azure AD; siga [estas instrucciones para agregar una aplicación personalizada](../active-directory-saas-custom-apps.md).

  ![Adición de una aplicación de SAML 2.0 o WS-Fed a la lista de la galería](./media/howto-app-gallery-listing/saml.png)

  * Si desea agregar la aplicación a la lista en la galería mediante **SAML 2.0** o **WS-Fed**, seleccione **SAML 2.0/WS-Fed** como se muestra.
  * Si tiene algún problema con el acceso, póngase en contacto con el [equipo de integración del SSO de Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="implement-sso-by-using-the-password-sso"></a>Implementación del inicio de sesión único con el SSO por contraseña

Cree una aplicación web que tenga una página de inicio de sesión HTML para configurar el [inicio de sesión único basado en contraseña](../manage-apps/what-is-single-sign-on.md). El SSO basado en contraseña, también conocido como almacenamiento de contraseñas, permite administrar el acceso y las contraseñas de los usuarios en aplicaciones web que no admiten la federación de identidades. También es útil para escenarios en los que varios usuarios deben compartir la misma cuenta, como las cuentas de las aplicaciones de redes sociales de la organización.

![Escala de tiempo para agregar la aplicación de SSO con contraseña a la lista de galería](./media/howto-app-gallery-listing/passwordsso.png)

* Si desea agregar la aplicación a la lista de la galería mediante SSO con contraseña, seleccione **Password SSO** (SSO con contraseña) como se muestra.
* Si tiene algún problema con el acceso, póngase en contacto con el [equipo de integración del SSO de Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="request-for-user-provisioning"></a>Solicitud de aprovisionamiento de usuarios

Siga el proceso que se muestra en la siguiente imagen para solicitar el aprovisionamiento de usuarios.

   ![Solicitud de aprovisionamiento de usuarios](./media/howto-app-gallery-listing/user-provisioning.png)

## <a name="update-or-remove-an-existing-listing"></a>Actualización o eliminación de un listado existente

Para actualizar o quitar una aplicación existente en la galería de aplicaciones de Azure AD, primero debe enviar la solicitud al [portal de red de aplicaciones](https://microsoft.sharepoint.com/teams/apponboarding/Apps). Si tiene una cuenta de Office 365, utilícela para iniciar sesión en este portal. De lo contrario, utilice una cuenta Microsoft, como Outlook o Hotmail, para iniciar sesión.

- Seleccione la opción adecuada, tal como se muestra en la siguiente imagen.

    ![Listado de una aplicación SAML en la galería](./media/howto-app-gallery-listing/updateorremove.png)

    * Para actualizar una aplicación existente, seleccione la opción adecuada según sus requisitos.
    * Para quitar una aplicación existente de la galería de aplicaciones de Azure AD, seleccione **Remove my application listing from the gallery** (Quitar mi listado de aplicaciones de la galería).
    * Si tiene algún problema con el acceso, póngase en contacto con el [equipo de integración del SSO de Azure AD](<mailto:SaaSApplicationIntegrations@service.microsoft.com>).

## <a name="list-requests-by-customers"></a>Listado de solicitudes de los clientes

Los clientes pueden enviar una solicitud para mostrar una aplicación mediante la selección de **App requests by Customers** > **Submit new request** (Solicitudes de aplicaciones de los clientes -> Enviar solicitud nueva).

![Muestra el icono de aplicaciones solicitadas por los clientes.](./media/howto-app-gallery-listing/customer-submit-request.png)

Este es el flujo de las aplicaciones solicitadas por el cliente.

![Muestra el flujo de aplicaciones solicitadas por los clientes.](./media/howto-app-gallery-listing/customer-request.png)

## <a name="timelines"></a>Escalas de tiempo

La escala de tiempo para el proceso del listado de una aplicación de SAML 2.0 o WS-Fed en la galería es de entre 7 y 10 días laborables.

  ![Escala de tiempo para agregar una aplicación SAML a la lista de la galería](./media/howto-app-gallery-listing/timeline.png)

La escala de tiempo que dura el proceso para mostrar una aplicación de OpenID Connect en la lista de la galería es de entre 2 y 5 días laborables.

  ![Escala de tiempo para agregar la aplicación de OpenID Connect a la lista de la galería](./media/howto-app-gallery-listing/timeline2.png)

## <a name="escalations"></a>Extensiones

Si necesita ponerse en contacto con una instancia superior, envíe un correo electrónico al [equipo de integración de SSO de Azure AD](mailto:SaaSApplicationIntegrations@service.microsoft.com) en SaaSApplicationIntegrations@service.microsoft.com y le responderemos lo antes posible.
