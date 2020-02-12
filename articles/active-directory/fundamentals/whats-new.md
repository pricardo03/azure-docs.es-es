---
title: Novedades Notas de la versión de Azure Active Directory | Microsoft Docs
description: Obtenga información acerca de las novedades de Azure Active Directory, como, por ejemplo, las notas de la versión más recientes, los problemas conocidos, las correcciones de errores, las funcionalidades en desuso y los próximos cambios.
services: active-directory
author: msmimart
manager: daveba
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 01/27/2020
ms.author: mimart
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 69f0c0ed1efb727c4f4f258dbb3ce0179b52de95
ms.sourcegitcommit: fa6fe765e08aa2e015f2f8dbc2445664d63cc591
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2020
ms.locfileid: "76931429"
---
# <a name="whats-new-in-azure-active-directory"></a>¿Cuáles son las novedades de Azure Active Directory?

>Reciba notificaciones para volver a visitar esta página y obtener actualizaciones; para ello, copie y pegue la dirección URL `https://docs.microsoft.com/api/search/rss?search=%22release+notes+for+azure+AD%22&locale=en-us` en el ![icono del lector de fuentes icono RSS](./media/whats-new/feed-icon-16x16.png) lector de fuentes.

En Azure AD se realizan mejoras de forma continua. Para mantenerse al día de los avances más recientes, este artículo proporciona información acerca de los elementos siguientes:

- Versiones más recientes
- Problemas conocidos
- Corrección de errores
- Funciones obsoletas
- Planes de cambios

Esta página se actualiza mensualmente, por lo que se recomienda visitarla con frecuencia. Si busca elementos que tengan más de 6 meses, puede encontrarlos en el [Archivo de novedades de Azure Active Directory](whats-new-archive.md).

---

## <a name="january-2020"></a>Enero de 2020
 
### <a name="the-new-my-apps-portal-is-now-generally-available"></a>El nuevo portal Mis aplicaciones ya está disponible con carácter general

**Tipo:** Plan de cambio  
**Categoría del servicio:** Mis aplicaciones  
**Funcionalidad del producto:** Experiencias de usuario final
 
Actualice su organización al nuevo portal Mis aplicaciones que ya está disponible con carácter general. Puede encontrar más información sobre el nuevo portal y las colecciones en [Creación de colecciones en el portal Mis aplicaciones](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-collections).

---
 
### <a name="workspaces-in-azure-ad-have-been-renamed-to-collections"></a>Las áreas de trabajo de Azure AD se han renombrado a colecciones

**Tipo:** Característica modificada  
**Categoría del servicio:** Mis aplicaciones   
**Funcionalidad del producto:** Experiencias de usuario final
 
Las áreas de trabajo, filtros que los administradores pueden configurar para organizar las aplicaciones de sus usuarios, ahora se denominarán colecciones. Obtenga más información sobre cómo configurarlas en [Creación de colecciones en el portal Mis aplicaciones](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-collections).

---
 
### <a name="azure-ad-b2c-phone-sign-up-and-sign-in-using-custom-policy-public-preview"></a>Registro e inicio de sesión mediante teléfono en Azure AD B2C con una directiva personalizada (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** B2C: administración de identidades de consumidor  
**Funcionalidad del producto:** B2B/B2C
 
Con el registro y el inicio de sesión mediante el número de teléfono, los desarrolladores y las empresas pueden permitir a sus clientes registrarse e iniciar sesión con una contraseña de un solo uso enviada al número de teléfono del usuario a través de un SMS. Esta característica también permite al cliente cambiar su número de teléfono si pierde el acceso a su dispositivo. Con la eficacia de las directivas personalizadas, el registro y el inicio de sesión mediante el teléfono permiten a los desarrolladores y a las empresas comunicar su marca a través de la personalización de la página. Aprenda cómo [configurar el registro y el inicio de sesión en el teléfono con directivas personalizadas en Azure AD B2C](https://docs.microsoft.com/azure/active-directory-b2c/phone-authentication).
 
---
 
### <a name="new-provisioning-connectors-in-the-azure-ad-application-gallery---january-2020"></a>Nuevos conectores de aprovisionamiento en la galería de aplicaciones de Azure AD (enero de 2020)

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros
 
Ahora, puede automatizar la creación, actualización y eliminación de cuentas de usuario para estas aplicaciones recién integradas:

- [Promapp]( https://docs.microsoft.com/azure/active-directory/saas-apps/promapp-provisioning-tutorial)
- [Zscaler Private Access](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-private-access-provisioning-tutorial)

Para más información acerca de cómo proteger mejor una organización mediante el aprovisionamiento automatizado de cuentas de usuario, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery---january-2020"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD (enero de 2020)

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros
 
En enero de 2020, hemos agregado estas 33 nuevas aplicaciones con compatibilidad con la federación a nuestra galería de aplicaciones: 

[JOSA](https://docs.microsoft.com/azure/active-directory/saas-apps/josa-tutorial), [Fastly Edge Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/fastly-edge-cloud-tutorial), [Terraform Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/terraform-enterprise-tutorial), [Spintr SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/spintr-sso-tutorial), [Abibot Netlogistik](https://1030-review-develop-3zknud.netlogistik.com/), [SkyKick Cloud Backup for Office 365](https://login.skykick.com/login?state=g6Fo2SBTd3M5Q0xBT0JMd3luS2JUTGlYN3pYTE1remJQZnR1c6N0aWTZIDhCSkwzYVQxX2ZMZjNUaWxNUHhCSXg2OHJzbllTcmYto2NpZNkgM0h6czk3ZlF6aFNJV1VNVWQzMmpHeFFDbDRIMkx5VEc&client=3Hzs97fQzhSIWUMUd32jGxQCl4H2LyTG&protocol=oauth2&audience=https://papi.skykick.com&response_type=code&redirect_uri=https://portal.skykick.com/callback&scope=openid%20profile%20offline_access), [Upshotly](https://docs.microsoft.com/azure/active-directory/saas-apps/upshotly-tutorial), [LeaveBot](https://leavebot.io/#home), [DataCamp](https://docs.microsoft.com/azure/active-directory/saas-apps/datacamp-tutorial), [TripActions](https://docs.microsoft.com/azure/active-directory/saas-apps/tripactions-tutorial), [SmartWork](https://www.intumit.com/english/SmartWork.html), [Dotcom-Monitor](https://docs.microsoft.com/azure/active-directory/saas-apps/dotcom-monitor-tutorial), [SSOGEN - Azure AD SSO Gateway for Oracle E-Business Suite - EBS, PeopleSoft, and JDE](https://docs.microsoft.com/azure/active-directory/saas-apps/ssogen-tutorial), [Hosted MyCirqa SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/hosted-mycirqa-sso-tutorial), [Yuhu Property Management Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/yuhu-property-management-platform-tutorial), [LumApps](https://sites.lumapps.com/login), [Upwork Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/upwork-enterprise-tutorial), [Talentsoft](https://docs.microsoft.com/azure/active-directory/saas-apps/talentsoft-tutorial), [SmartDB for Microsoft Teams](http://teams.smartdb.jp/login/), [PressPage](https://docs.microsoft.com/azure/active-directory/saas-apps/presspage-tutorial), [ContractSafe Saml2 SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/contractsafe-saml2-sso-tutorial), [Maxient Conduct Manager Software](https://docs.microsoft.com/azure/active-directory/saas-apps/maxient-conduct-manager-software-tutorial), [Helpshift](https://docs.microsoft.com/azure/active-directory/saas-apps/helpshift-tutorial), [PortalTalk 365](https://www.portaltalk.com/), [CoreView](https://portal.coreview.com/), [Squelch Cloud Office365 Connector](https://laxmi.squelch.io/login), [PingFlow Authentication](https://app-staging.pingview.io/), [ PrinterLogic SaaS](https://docs.microsoft.com/azure/active-directory/saas-apps/printerlogic-saas-tutorial), [Taskize Connect](https://docs.microsoft.com/azure/active-directory/saas-apps/taskize-connect-tutorial), [Sandwai](https://app.sandwai.com/), [EZRentOut](https://docs.microsoft.com/azure/active-directory/saas-apps/ezrentout-tutorial), [AssetSonar](https://docs.microsoft.com/azure/active-directory/saas-apps/assetsonar-tutorial), [Akari Virtual Assistant](https://akari.io/ava/)

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial). Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="two-new-identity-protection-detections"></a>Dos nuevas detecciones de Identity Protection

**Tipo:** Nueva característica  
**Categoría del servicio:** Protección de identidad  
**Funcionalidad del producto:** Seguridad y protección de la identidad
 
Hemos agregado a Identity Protection dos nuevos tipos de detección vinculados al inicio de sesión: Reglas de manipulación sospechosa de la bandeja de entrada y Viaje imposible. Microsoft Cloud App Security (MCAS) descubre estas detecciones sin conexión e influye en los riesgos de usuario y de inicio de sesión en Identity Protection. Para más información sobre estas detecciones, consulte nuestros [tipos de riesgo de inicio de sesión](https://docs.microsoft.com/azure/active-directory/identity-protection/concept-identity-protection-risks#sign-in-risk).
 
---
 
### <a name="breaking-change-uri-fragments-will-not-be-carried-through-the-login-redirect"></a>Cambio de última hora: los fragmentos de URI no se trasladarán a través de la redirección de inicio de sesión

**Tipo:** Característica modificada  
**Categoría del servicio:** Autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** Autenticación de usuarios
 
A partir del 8 de febrero de 2020, cuando se envíe una solicitud a login.microsoftonline.com para que un usuario inicie sesión, el servicio anexará un fragmento vacío a la solicitud.  Esto evita una clase de ataques de redireccionamiento, ya que se asegura de que el explorador borra todo fragmento existente en la solicitud. Ninguna aplicación debe tener una dependencia de este comportamiento. Para más información, consulte [Cambios importantes](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#february-2020) en la documentación de la plataforma de identidad de Microsoft.

---

## <a name="december-2019"></a>Diciembre de 2019

### <a name="integrate-sap-successfactors-provisioning-into-azure-ad-and-on-premises-ad-public-preview"></a>Integración del aprovisionamiento de SAP SuccessFactors en Azure AD y AD local (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** Administración del ciclo de vida de la identidad

Ahora puede integrar SAP SuccessFactors como origen de identidad relevante en Azure AD. Esta integración le ayuda a automatizar el ciclo de vida de la identidad de un extremo a otro, incluido el uso de eventos basados en recursos humanos, como nuevas contrataciones o resoluciones de contrato, para controlar el aprovisionamiento de cuentas de Azure AD.

Para obtener más información sobre cómo configurar el aprovisionamiento de entrada de SAP SuccessFactors para Azure AD, consulte el tutorial sobre [configuración del aprovisionamiento automático de SAP SuccessFactors](https://aka.ms/SAPSuccessFactorsInboundTutorial).

---

### <a name="support-for-customized-emails-in-azure-ad-b2c-public-preview"></a>Compatibilidad con correos electrónicos personalizados en Azure AD B2C (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** B2C: administración de identidades de consumidor  
**Funcionalidad del producto:** B2B/B2C

Ahora puede usar Azure AD B2C para crear correos electrónicos personalizados cuando los usuarios se suscriban para usar las aplicaciones. Mediante el uso de DisplayControls (actualmente en versión preliminar) y un proveedor de correo electrónico de terceros (por ejemplo, [SendGrid](https://sendgrid.com/), [SparkPost](https://sparkpost.com/) o una API REST personalizada), puede usar su propia plantilla de correo electrónico, dirección **De** y texto del asunto, además de admitir la localización y la configuración personalizada de contraseña de un solo uso (OTP).

Para obtener más información, consulte el artículo sobre [comprobación de correo electrónico personalizada en Azure Active Directory B2C](https://docs.microsoft.com/azure/active-directory-b2c/custom-email).

---

### <a name="replacement-of-baseline-policies-with-security-defaults"></a>Sustitución de directivas de base de referencia por valores predeterminados de seguridad

**Tipo:** Característica modificada  
**Categoría del servicio:** Otros  
**Funcionalidad del producto:** Seguridad y protección de la identidad

Como parte de un modelo predeterminado seguro para la autenticación, vamos a quitar las directivas de protección de base de referencia existentes de todos los inquilinos. Se prevé que esta retirada se complete a finales de febrero. El reemplazo de estas directivas de protección de base de referencia son los valores predeterminados de seguridad. Si ha usado directivas de protección de base de referencia, debe planear la migración a la nueva directiva de valores predeterminados de seguridad o al acceso condicional. Si no ha usado estas directivas, no tiene que realizar ninguna acción.

Para obtener más información sobre los nuevos valores predeterminados de seguridad, consulte [¿Qué son los valores predeterminados de seguridad?](https://docs.microsoft.com/azure/active-directory/fundamentals/concept-fundamentals-security-defaults) Para obtener más información sobre las directivas de acceso condicional, consulte [Directivas de acceso condicional habituales](https://docs.microsoft.com/azure/active-directory/conditional-access/concept-conditional-access-policy-common).

---

## <a name="november-2019"></a>Noviembre de 2019

### <a name="support-for-the-samesite-attribute-and-chrome-80"></a>Compatibilidad con el atributo SameSite y Chrome 80

**Tipo:** Plan de cambio  
**Categoría del servicio:** Autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** Autenticación de usuarios

Como parte de un modelo predeterminado y seguro para las cookies, el explorador Chrome 80 cambia el modo en que trata las cookies sin el atributo `SameSite`. Cualquier cookie que no especifique el atributo `SameSite` se tratará como si se hubiera establecido en `SameSite=Lax`, lo que producirá un bloqueo de Chrome en ciertos escenarios de uso compartido de cookies entre dominios de los que puede depender su aplicación. Para mantener el comportamiento de Chrome anterior, puede utilizar el atributo `SameSite=None` y agregar un atributo `Secure` adicional, por lo que solo se puede tener acceso a las cookies entre sitios a través de conexiones HTTPS. Chrome está programado para completar este cambio el 4 de febrero de 2020.

Se recomienda que todos los desarrolladores prueben sus aplicaciones siguiendo esta guía:

- Establecer el valor predeterminado de la opción **Usar cookies seguras** en **Sí**.

- Establecer el valor predeterminado del atributo **SameSite** en **Ninguno**.

- Agregar un atributo `SameSite` adicional de **Seguro**.

Para obtener más información, consulte el artículo sobre [próximos cambios en la cookie de SameSite en ASP.NET y ASP.NET Core](https://devblogs.microsoft.com/aspnet/upcoming-samesite-cookie-changes-in-asp-net-and-asp-net-core/) y [Posible interrupción para los sitios web de los clientes y los servicios de Microsoft en la versión 79 de Chrome Beta y versiones posteriores](https://support.microsoft.com/help/4522904/potential-disruption-to-microsoft-services-in-chrome-beta-version-79).

---

### <a name="new-hotfix-for-microsoft-identity-manager-mim-2016-service-pack-2-sp2"></a>Nueva revisión para Microsoft Identity Manager (MIM) 2016 Service Pack 2 (SP2)

**Tipo:** Corregido  
**Categoría del servicio:** Microsoft Identity Manager  
**Funcionalidad del producto:** Administración del ciclo de vida de la identidad

Un paquete acumulativo de revisiones (compilación 4.6.34.0) está disponible para Microsoft Identity Manager (MIM) 2016 Service Pack 2 (SP2). Este paquete acumulativo resuelve problemas y agrega mejoras que se describen en la sección sobre "problemas corregidos y mejoras agregadas en esta actualización".

Para obtener más información y descargar el paquete de revisiones, consulte el artículo [El paquete acumulativo de actualizaciones de Microsoft Identity Manager 2016 Service Pack 2 (compilación 4.6.34.0) está disponible](https://support.microsoft.com/help/4512924/microsoft-identity-manager-2016-service-pack-2-build-4-6-34-0-update-r).

---

### <a name="new-ad-fs-app-activity-report-to-help-migrate-apps-to-azure-ad-public-preview"></a>Nuevo informe de actividad de aplicaciones de AD FS para ayudar a migrar las aplicaciones a Azure AD (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** SSO

Use el nuevo informe de actividad de aplicaciones de Servicios de federación de Active Directory (AD FS) en Azure Portal para identificar las aplicaciones que se pueden migrar a Azure AD. El informe evalúa todas las aplicaciones de AD FS con respecto a la compatibilidad con Azure AD, comprueba si hay problemas y proporciona instrucciones sobre cómo preparar aplicaciones concretas para su migración.

Para obtener más información, consulte [Uso del informe de actividades de aplicaciones de AD FS para migrar aplicaciones a Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/migrate-adfs-application-activity).

---

### <a name="new-workflow-for-users-to-request-administrator-consent-public-preview"></a>Nuevo flujo de trabajo para que los usuarios soliciten el consentimiento del administrador (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Control de acceso

El flujo de trabajo de consentimiento del administrador proporciona a los administradores una manera de conceder acceso a las aplicaciones que requieren la aprobación del administrador. Si un usuario intenta acceder a una aplicación, pero no puede dar su consentimiento, puede enviar una solicitud de aprobación del administrador. La solicitud se envía por correo electrónico, y se coloca en una cola a la que se puede acceder desde Azure Portal, a todos los administradores que se han designado como revisores. Una vez que un revisor realiza una acción en una solicitud pendiente, se notifica la acción a los usuarios que realizan la solicitud.

Para obtener más información, consulte [Configuración del flujo de trabajo de consentimiento del administrador (versión preliminar)](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-admin-consent-workflow).

---

### <a name="new-azure-ad-app-registrations-token-configuration-experience-for-managing-optional-claims-public-preview"></a>Nueva experiencia de configuración del token de registros de aplicación de Azure AD para administrar notificaciones opcionales (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Otros  
**Funcionalidad del producto:** Experiencia para el desarrollador

La nueva hoja de **configuración del token de registros de aplicación de Azure AD** de Azure Portal muestra ahora a los desarrolladores de aplicaciones una lista dinámica de notificaciones opcionales para sus aplicaciones. Esta nueva experiencia ayuda a simplificar las migraciones de aplicaciones de Azure AD y a minimizar las configuraciones incorrectas de las notificaciones opcionales.

Para obtener más información, consulte [Proporcionar notificaciones opcionales a la aplicación de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims).

---

### <a name="new-two-stage-approval-workflow-in-azure-ad-entitlement-management-public-preview"></a>Nuevo flujo de trabajo de aprobación en dos fases de administración de derechos de Azure AD (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Otros  
**Funcionalidad del producto:** Administración de derechos

Hemos presentado un nuevo flujo de trabajo de aprobación en dos fases que le permite exigir dos aprobadores para aprobar la solicitud de un usuario a un paquete de acceso. Por ejemplo, puede establecerlo de manera que el administrador del usuario que realiza la solicitud deba dar su aprobación en primer lugar y, a continuación, también puede exigir que un propietario del recurso dé su aprobación. Si uno de los aprobadores no da su aprobación, no se concede el acceso.

Para obtener más información, consulte [Cambio de la configuración de la solicitud y aprobación para un paquete de acceso de administración de derechos de Azure AD](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-access-package-request-policy).

---

### <a name="updates-to-the-my-apps-page-along-with-new-workspaces-public-preview"></a>Actualizaciones de la página Mis aplicaciones con nuevas áreas de trabajo (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Mis aplicaciones  
**Funcionalidad del producto:** Integración de terceros

Ahora puede personalizar la forma en que los usuarios de la organización ven y acceden a la experiencia actualizada de Mis aplicaciones. Esta nueva experiencia también incluye la nueva característica de áreas de trabajo, que facilita a los usuarios la búsqueda y organización de aplicaciones.

Para obtener más información sobre la nueva experiencia de Mis aplicaciones y la creación de áreas de trabajo, consulte el artículo sobre [creación de áreas de trabajo en el portal Mis aplicaciones](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-workspaces).

---

### <a name="google-social-id-support-for-azure-ad-b2b-collaboration-general-availability"></a>Compatibilidad con identificadores sociales de Google para la colaboración B2B de Azure AD (disponibilidad general)

**Tipo:** Nueva característica  
**Categoría del servicio:** B2B  
**Funcionalidad del producto:** Autenticación de usuarios

La nueva compatibilidad con el uso de identificadores sociales de Google (cuentas Gmail) en Azure AD ayuda a simplificar la colaboración entre sus usuarios y partners. Ya no hay necesidad de que los partners creen y administren una cuenta específica de Microsoft. Microsoft Teams ahora es totalmente compatible con los usuarios de Google en todos los clientes y en los puntos de conexión de autenticación comunes y relacionados con los inquilinos.

Para obtener más información, consulte [Incorporación de Google como proveedor de identidades para los usuarios invitados de B2B](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).

---

### <a name="microsoft-edge-mobile-support-for-conditional-access-and-single-sign-on-general-availability"></a>Compatibilidad de Microsoft Edge para dispositivos móviles con el acceso condicional y el inicio de sesión único (disponibilidad general)

**Tipo:** Nueva característica  
**Categoría del servicio:** Acceso condicional  
**Funcionalidad del producto:** Seguridad y protección de la identidad

Azure AD para Microsoft Edge en iOS y Android ahora admite acceso condicional e inicio de sesión único de Azure AD:

- **Inicio de sesión único (SSO) de Microsoft Edge:** El inicio de sesión único ahora está disponible en los clientes nativos (como Microsoft Outlook y Microsoft Edge) para todas las aplicaciones conectadas a Azure AD.

- **Acceso condicional de Microsoft Edge:** A través de las directivas de acceso condicional basado en aplicaciones, los usuarios deben usar exploradores protegidos por Microsoft Intune, como Microsoft Edge.

Para obtener más información sobre el acceso condicional y el inicio de sesión único con Microsoft Edge, consulte la entrada de blog sobre [compatibilidad de Microsoft Edge para dispositivos móviles con el acceso condicional y el inicio de sesión único ahora disponible con carácter general](https://techcommunity.microsoft.com/t5/Intune-Customer-Success/Microsoft-Edge-Mobile-Support-for-Conditional-Access-and-Single/ba-p/988179). Para obtener más información sobre cómo configurar las aplicaciones cliente con [acceso condicional basado en aplicaciones](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) o [acceso condicional basado en dispositivos](https://docs.microsoft.com/azure/active-directory/conditional-access/require-managed-devices), consulte [Administración del acceso web mediante un explorador protegido por directivas de Microsoft Intune](https://docs.microsoft.com/intune/apps/app-configuration-managed-browser).

---

### <a name="azure-ad-entitlement-management-general-availability"></a>Administración de derechos de Azure AD (disponibilidad general)

**Tipo:** Nueva característica  
**Categoría del servicio:** Otros  
**Funcionalidad del producto:** Administración de derechos

La administración de derechos de Azure AD es una nueva característica de gobernanza de identidades, que ayuda a las organizaciones a administrar el ciclo de vida de identidad y acceso a escala. Esta nueva característica ayuda mediante la automatización de los flujos de trabajo de solicitud de acceso, las asignaciones de acceso, las revisiones y la expiración entre grupos, aplicaciones y sitios de SharePoint Online.

Con la administración de derechos de Azure AD, puede administrar el acceso de forma más eficaz tanto para los empleados como para los usuarios ajenos a la organización que han de acceder a esos recursos.

Para obtener más información, consulte [¿Qué es la administración de derechos de Azure AD?](https://docs.microsoft.com/azure/active-directory/governance/entitlement-management-overview#license-requirements)

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Automatización del aprovisionamiento de cuentas de usuario para estas aplicaciones SaaS recién admitidas

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros  

Ahora, puede automatizar la creación, actualización y eliminación de cuentas de usuario para estas aplicaciones recién integradas:

[SAP Cloud Platform Identity Authentication Service](https://docs.microsoft.com/azure/active-directory/saas-apps/sap-hana-cloud-platform-identity-authentication-tutorial), [RingCentral](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-provisioning-tutorial), [SpaceIQ](https://docs.microsoft.com/azure/active-directory/saas-apps/spaceiq-provisioning-tutorial), [Miro](https://docs.microsoft.com/azure/active-directory/saas-apps/miro-provisioning-tutorial), [Cloudgate](https://docs.microsoft.com/azure/active-directory/saas-apps/soloinsight-cloudgate-sso-provisioning-tutorial), [Infor CloudSuite](https://docs.microsoft.com/azure/active-directory/saas-apps/infor-cloudsuite-provisioning-tutorial), [OfficeSpace Software](https://docs.microsoft.com/azure/active-directory/saas-apps/officespace-software-provisioning-tutorial), [Priority Matrix](https://docs.microsoft.com/azure/active-directory/saas-apps/priority-matrix-provisioning-tutorial)

Para más información acerca de cómo proteger mejor una organización mediante el aprovisionamiento automatizado de cuentas de usuario, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---november-2019"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD: noviembre de 2019

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros

En noviembre de 2019, hemos agregado estas 21 nuevas aplicaciones con compatibilidad con la federación a la galería de aplicaciones:

[Airtable](https://docs.microsoft.com/azure/active-directory/saas-apps/airtable-tutorial), [Hootsuite](https://docs.microsoft.com/azure/active-directory/saas-apps/hootsuite-tutorial), [Blue Access for Members (BAM)](https://docs.microsoft.com/azure/active-directory/saas-apps/blue-access-for-members-tutorial), [Bitly](https://docs.microsoft.com/azure/active-directory/saas-apps/bitly-tutorial), [Riva](https://docs.microsoft.com/azure/active-directory/saas-apps/riva-tutorial), [ResLife Portal](https://app.reslifecloud.com/hub5_signin/microsoft_azuread/?g=44BBB1F90915236A97502FF4BE2952CB&c=5&uid=0&ht=2&ref=), [NegometrixPortal Single Sign On (SSO)](https://docs.microsoft.com/azure/active-directory/saas-apps/negometrixportal-tutorial), [TeamsChamp](https://login.microsoftonline.com/551f45da-b68e-4498-a7f5-a6e1efaeb41c/adminconsent?client_id=ca9bbfa4-1316-4c0f-a9ee-1248ac27f8ab&redirect_uri=https://admin.teamschamp.com/api/adminconsent&state=6883c143-cb59-42ee-a53a-bdb5faabf279), [Motus](https://docs.microsoft.com/azure/active-directory/saas-apps/motus-tutorial), [MyAryaka](https://docs.microsoft.com/azure/active-directory/saas-apps/myaryaka-tutorial), [BlueMail](https://loginself1.bluemail.me/), [Beedle](https://teams-web.beedle.co/#/), [Visma](https://docs.microsoft.com/azure/active-directory/saas-apps/visma-tutorial), [OneDesk](https://docs.microsoft.com/azure/active-directory/saas-apps/onedesk-tutorial), [Foko Retail](https://docs.microsoft.com/azure/active-directory/saas-apps/foko-retail-tutorial), [Qmarkets Idea & Innovation Management](https://docs.microsoft.com/azure/active-directory/saas-apps/qmarkets-idea-innovation-management-tutorial), [Netskope User Authentication](https://docs.microsoft.com/azure/active-directory/saas-apps/netskope-user-authentication-tutorial), [uniFLOW Online](https://docs.microsoft.com/azure/active-directory/saas-apps/uniflow-online-tutorial), [Claromentis](https://docs.microsoft.com/azure/active-directory/saas-apps/claromentis-tutorial), [Jisc Student Voter Registration](https://docs.microsoft.com/azure/active-directory/saas-apps/jisc-student-voter-registration-tutorial), [e4enable](https://portal.e4enable.com/)

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial). Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="new-and-improved-azure-ad-application-gallery"></a>La nueva y mejorada galería de aplicaciones de Azure AD

**Tipo:** Característica modificada  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** SSO

Hemos actualizado la galería de aplicaciones de Azure AD para que le resulte más fácil encontrar aplicaciones integradas previamente que admitan el aprovisionamiento, OpenID Connect y SAML en su inquilino de Azure Active Directory.

Para obtener más información, consulte [Incorporación de una aplicación a un inquilino de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/add-application-portal).

---

### <a name="increased-app-role-definition-length-limit-from-120-to-240-characters"></a>Límite de longitud de definición de roles de aplicación aumentado de 120 a 240 caracteres

**Tipo:** Característica modificada  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** SSO

Hemos escuchado de los clientes que el límite de longitud del valor de definición de rol de aplicación en algunas aplicaciones y servicios es demasiado corto en 120 caracteres. En respuesta, hemos aumentado la longitud máxima de la definición de valor de rol a 240 caracteres.

Para obtener más información sobre el uso de definiciones de roles específicas de la aplicación, consulte Para ver más ejemplos e información, consulte [Procedimiento para agregar roles de aplicación en la aplicación y recibirlos en el token](https://docs.microsoft.com/azure/active-directory/develop/howto-add-app-roles-in-azure-ad-apps).

---

## <a name="october-2019"></a>Octubre de 2019

### <a name="deprecation-of-the-identityriskevent-api-for-azure-ad-identity-protection-risk-detections"></a>Desuso de la API de identityRiskEvent para las detecciones de riesgo de Azure AD Identity Protection  

**Tipo:** Plan de cambio  
**Categoría del servicio:** Protección de identidad  
**Funcionalidad del producto:** Seguridad y protección de la identidad

En respuesta a los comentarios de los desarrolladores, los suscriptores de Azure AD Premium P2 ahora pueden realizar consultas complejas sobre los datos de detección de riesgos de Azure AD Identity Protection mediante la nueva API de riskDetection para Microsoft Graph. La versión beta existente de la API de [identityRiskEvent](https://docs.microsoft.com/graph/api/resources/identityriskevent?view=graph-rest-beta) dejará de devolver datos en torno al **10 de enero de 2020**. Si su organización usa la API de identityRiskEvent, debe realizar la transición a la nueva API de riskDetection.

Para obtener más información sobre la nueva API de riskDetection, consulte la [documentación de referencia de la API de detección de riesgos](https://aka.ms/RiskDetectionsAPI).

---

### <a name="application-proxy-support-for-the-samesite-attribute-and-chrome-80"></a>Compatibilidad del proxy de aplicación con el atributo SameSite y Chrome 80

**Tipo:** Plan de cambio  
**Categoría del servicio:** Proxy de aplicaciones  
**Funcionalidad del producto:** Control de acceso

Un par de semanas antes del lanzamiento del explorador Chrome 80, tenemos previsto actualizar el modo en que las cookies del proxy de aplicación tratan el atributo **SameSite**. Con el lanzamiento de Chrome 80, cualquier cookie que no especifique el atributo **SameSite** se tratará como si se hubiera establecido en `SameSite=Lax`.

Para ayudar a evitar posibles impactos negativos debido a este cambio, vamos a actualizar las cookies de sesión y el acceso del proxy de aplicación de la siguiente manera:

- Estableciendo el valor predeterminado de la opción **Usar cookies seguras** en **Sí**.

- Estableciendo el valor predeterminado del atributo **SameSite** en **Ninguno**.

    >[!NOTE]
    > Las cookies de acceso del proxy de aplicación siempre se han transmitido exclusivamente a través de canales seguros. Estos cambios solo se aplican a las cookies de sesión.

Para obtener más información sobre la configuración de las cookies del proxy de aplicación, consulte [Configuración de las cookies para el acceso a aplicaciones locales en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-cookie-settings).

---

### <a name="app-registrations-legacy-and-converged-app-management-from-the-application-registration-portal-appsdevmicrosoftcom-will-no-longer-be-available"></a>La característica Registros de aplicaciones (heredada) y la funcionalidad de administración de aplicaciones convergentes desde el Portal de registro de aplicaciones (apps.dev.microsoft.com) ya no estarán disponibles

**Tipo:** Plan de cambio  
**Categoría del servicio:** N/D  
**Funcionalidad del producto:** Experiencia para el desarrollador

En un futuro próximo, los usuarios con cuentas de Azure AD ya no podrán registrar y administrar aplicaciones convergentes mediante el Portal de registro de aplicaciones (apps.dev.microsoft.com), ni registrar ni administrar aplicaciones en la experiencia Registros de aplicaciones (heredada) de Azure Portal.

Para obtener más información sobre la nueva experiencia Registros de aplicaciones, consulte la sección sobre [Registros de aplicaciones en la guía de aprendizaje de Azure Portal](../develop/app-registrations-training-guide-for-app-registrations-legacy-users.md).

---

### <a name="users-are-no-longer-required-to-re-register-during-migration-from-per-user-mfa-to-conditional-access-based-mfa"></a>Ya no es necesario que los usuarios vuelvan a registrarse durante la migración de MFA por usuario a MFA basada en el acceso condicional

**Tipo:** Corregido  
**Categoría del servicio:** MFA  
**Funcionalidad del producto:** Seguridad y protección de la identidad

Hemos corregido un problema conocido por el que se requería que los usuarios se volvieran a registrar si estaban deshabilitados para la autenticación multifactor (MFA) por usuario y luego se habilitaban para la MFA a través de una directiva de acceso condicional.

Para requerir que los usuarios vuelvan a registrarse, puede seleccionar la opción **Requerir volver a registrar MFA** en los métodos de autenticación del usuario en el portal de Azure AD. Para obtener más información sobre la migración de usuarios de MFA por usuario a MFA basada en el acceso condicional, consulte [Conversión de los usuarios de MFA por usuario a MFA basado en acceso condicional](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted#convert-users-from-per-user-mfa-to-conditional-access-based-mfa).

---

### <a name="new-capabilities-to-transform-and-send-claims-in-your-saml-token"></a>Nuevas funcionalidades para transformar y enviar notificaciones en el token SAML

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** SSO

Hemos agregado funcionalidades adicionales que lo ayudarán a personalizar y enviar notificaciones en el token SAML. Estas son algunas de ellas:

- Funciones de transformación de notificaciones adicionales, lo que lo ayudará a modificar el valor que envía en la notificación.

- Capacidad de aplicar varias transformaciones a una única notificación.

- Capacidad de especificar el origen de la notificación, en función del tipo de usuario y el grupo al que pertenece el usuario.

Para obtener información detallada sobre estas nuevas funcionalidades, incluido cómo usarlas, consulte [Personalización de las notificaciones emitidas en el token SAML para aplicaciones empresariales](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="new-my-sign-ins-page-for-end-users-in-azure-ad"></a>Nueva página Mis inicios de sesión para los usuarios finales en Azure AD

**Tipo:** Nueva característica  
**Categoría del servicio:** Autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** Supervisión e informes

Hemos agregado una nueva página **Mis inicios de sesión** (https://mysignins.microsoft.com) ) para que los usuarios de la organización puedan ver su historial de inicio de sesión reciente con el fin de comprobar si hay alguna actividad inusual. Esta nueva página permite a los usuarios comprobar lo siguiente:

- Si alguien intenta adivinar la contraseña.

- Si un atacante inició sesión correctamente en su cuenta y desde qué ubicación.

- Las aplicaciones a las que el atacante ha intentado acceder.

Para obtener más información, consulte la entrada de blog sobre cómo [los usuarios pueden comprobar ahora su historial de inicio de sesión para actividades inusuales](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Users-can-now-check-their-sign-in-history-for-unusual-activity/ba-p/916066).

---

### <a name="migration-of-azure-ad-domain-services-azure-ad-ds-from-classic-to-azure-resource-manager-virtual-networks"></a>Migración de Azure AD Domain Services (Azure AD DS) de las redes virtuales clásicas a las de Azure Resource Manager

**Tipo:** Nueva característica  
**Categoría del servicio:** Azure AD Domain Services  
**Funcionalidad del producto:** Azure AD Domain Services

Tenemos excelentes noticias para los clientes que siguen usando las redes virtuales clásicas. Ahora puede realizar una migración puntual desde una red virtual clásica a una de Resource Manager. Después de migrar a la red virtual de Resource Manager, podrá aprovechar las ventajas de las características adicionales y actualizadas, como las directivas de contraseñas específicas, las notificaciones por correo electrónico y los registros de auditoría.

Para obtener más información, consulte [Versión preliminar: Migración de Azure AD Domain Services desde el modelo de red virtual clásica a Resource Manager](https://docs.microsoft.com/azure/active-directory-domain-services/migrate-from-classic-vnet).

---

### <a name="updates-to-the-azure-ad-b2c-page-contract-layout"></a>Actualizaciones del diseño del contrato de página de Azure AD B2C

**Tipo:** Nueva característica  
**Categoría del servicio:** B2C: administración de identidades de consumidor  
**Funcionalidad del producto:** B2B/B2C

Hemos introducido algunos cambios nuevos en la versión 1.2.0 del contrato de página de Azure AD B2C. En esta versión actualizada, ahora puede controlar el orden de carga de los elementos, lo que también puede ayudar a detener el parpadeo que se produce cuando se carga la hoja de estilos (CSS).

Para ver una lista completa de los cambios realizados en el contrato de página, vea el [registro de cambios de versión](https://docs.microsoft.com/azure/active-directory-b2c/page-layout#120).

---

### <a name="update-to-the-my-apps-page-along-with-new-workspaces-public-preview"></a>Actualización de la página Mis aplicaciones con nuevas áreas de trabajo (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Mis aplicaciones  
**Funcionalidad del producto:** Control de acceso

Ahora puede personalizar la forma en que los usuarios de su organización ven y acceden a la nueva experiencia de Mis aplicaciones, incluido el uso de la nueva característica de áreas de trabajo para que sea más fácil buscar aplicaciones. La nueva funcionalidad de áreas de trabajo actúa como un filtro para las aplicaciones a las que los usuarios de su organización ya tienen acceso.

Para obtener más información sobre cómo implementar la nueva experiencia de Mis aplicaciones y crear áreas de trabajo, consulte el artículo sobre [creación de áreas de trabajo en el portal Mis aplicaciones (versión preliminar)](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-workspaces).

---

### <a name="support-for-the-monthly-active-user-based-billing-model-general-availability"></a>Compatibilidad con el modelo de facturación basado en usuarios activos mensuales (disponibilidad general)

**Tipo:** Nueva característica  
**Categoría del servicio:** B2C: administración de identidades de consumidor  
**Funcionalidad del producto:** B2B/B2C

Azure AD B2C admite ahora la facturación de usuarios activos mensuales (MAU). La facturación de MAU se basa en el número de usuarios únicos con actividad de autenticación durante un mes natural. Los clientes existentes pueden pasarse a este nuevo método de facturación en cualquier momento.

A partir del 1 de noviembre de 2019, a todos los clientes nuevos se les facturará automáticamente con este método. Este método de facturación beneficia a los clientes gracias a las ventajas que ofrece en cuanto a costos y a la posibilidad de planear con anterioridad.

Para obtener más información, consulte [Actualización al modelo de facturación de usuarios activos mensuales](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-how-to-enable-billing#upgrade-to-monthly-active-users-billing-model).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---october-2019"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD: octubre de 2019

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros

En octubre de 2019, hemos agregado estas 35 nuevas aplicaciones con compatibilidad con la federación a la galería de aplicaciones:

[In Case of Crisis – Mobile](https://docs.microsoft.com/azure/active-directory/saas-apps/in-case-of-crisis-mobile-tutorial), [Juno Journey](https://docs.microsoft.com/azure/active-directory/saas-apps/juno-journey-tutorial), [ExponentHR](https://docs.microsoft.com/azure/active-directory/saas-apps/exponenthr-tutorial), [Tact](https://tact.ai/assistant/), [OpusCapita Cash Management](http://cm1.opuscapita.com/tenantname), [Salestim](https://prd.salestim.io/forms), [Learnster](https://docs.microsoft.com/azure/active-directory/saas-apps/learnster-tutorial), [Dynatrace](https://docs.microsoft.com/azure/active-directory/saas-apps/dynatrace-tutorial), [HunchBuzz](https://login.hunchbuzz.com/integrations/azure/process), [Freshworks](https://docs.microsoft.com/azure/active-directory/saas-apps/freshworks-tutorial), [eCornell](https://docs.microsoft.com/azure/active-directory/saas-apps/ecornell-tutorial), [ShipHazmat](https://docs.microsoft.com/azure/active-directory/saas-apps/shiphazmat-tutorial), [Netskope Cloud Security](https://docs.microsoft.com/azure/active-directory/saas-apps/netskope-cloud-security-tutorial), [Contentful](https://docs.microsoft.com/azure/active-directory/saas-apps/contentful-tutorial), [Bindtuning](https://bindtuning.com/login), [HireVue Coordinate – EU](https://www.hirevue.com/), [HireVue Coordinate - USOnly](https://www.hirevue.com/), [HireVue Coordinate - US](https://www.hirevue.com/), [WittyParrot Knowledge Box](https://wittyapi.wittyparrot.com/wittyparrot/api/provision/trail/signup), [Cloudmore](https://docs.microsoft.com/azure/active-directory/saas-apps/cloudmore-tutorial), [Visit.org](https://docs.microsoft.com/azure/active-directory/saas-apps/visitorg-tutorial), [Cambium Xirrus EasyPass Portal](https://login.xirrus.com/azure-signup), [Paylocity](https://docs.microsoft.com/azure/active-directory/saas-apps/paylocity-tutorial), [Mail Luck!](https://docs.microsoft.com/azure/active-directory/saas-apps/mail-luck-tutorial), [Teamie](https://theteamie.com/), [Velocity for Teams](https://velocity.peakup.org/teams/login), [SIGNL4](https://account.signl4.com/manage), [EAB Navigate IMPL](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-impl-tutorial), [ScreenMeet](https://console.screenmeet.com/), [Omega Point](https://pi.ompnt.com/), [Speaking Email for Intune (iPhone)](https://speaking.email/FAQ/98/email-access-via-microsoft-intune), [Speaking Email for Office 365 Direct (iPhone/Android)](https://speaking.email/FAQ/126/email-access-via-microsoft-office-365-direct), [ExactCare SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/exactcare-sso-tutorial), [iHealthHome Care Navigation System](https://ihealthnav.com/account/signin), [Qubie](https://qubie.azurewebsites.net/static/adminTab/authorize.html)

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial). Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="consolidated-security-menu-item-in-the-azure-ad-portal"></a>Elemento de menú consolidado Seguridad en el portal de Azure AD

**Tipo:** Característica modificada  
**Categoría del servicio:** Protección de identidad  
**Funcionalidad del producto:** Seguridad y protección de la identidad

Ahora puede acceder a todas las características de seguridad de Azure AD disponibles en el nuevo elemento de menú **Seguridad** y en la barra **Buscar** de Azure Portal. Además, la nueva página de aterrizaje **Seguridad** denominada **Seguridad - Introducción** proporcionará vínculos a nuestra documentación pública, así como guías de seguridad y de implementación.

El nuevo menú **Seguridad** incluye lo siguiente:

- Acceso condicional
- Protección de identidad
- Security Center
- Puntuación segura de identidad
- Métodos de autenticación
- MFA
- Informes de riesgo: usuarios de riesgo, inicios de sesión peligrosos, detecciones de riesgo, etc.
- Y mucho más.

Para obtener más información, visite [Seguridad - Introducción](https://portal.azure.com/#blade/Microsoft_AAD_IAM/SecurityMenuBlade/GettingStarted).

---

### <a name="office-365-groups-expiration-policy-enhanced-with-autorenewal"></a>Directiva de expiración de grupos de Office 365 mejorada con renovación automática

**Tipo:** Característica modificada  
**Categoría del servicio:** Administración de grupos  
**Funcionalidad del producto:** Administración del ciclo de vida de la identidad

La directiva de expiración de grupos de Office 365 se ha mejorado para renovar automáticamente los grupos que los miembros estén usando activamente. Los grupos se renuevan en función de la actividad del usuario en todas las aplicaciones de Office 365, como Outlook, SharePoint y Teams.

Esta mejora ayuda a reducir las notificaciones de expiración de los grupos, así como a garantizar que los grupos activos sigan estando disponibles. Si ya tiene una directiva de expiración activa para grupos de Office 365, no es necesario hacer nada para activar esta nueva funcionalidad.

Para obtener más información, vea [Configuración de la directiva de expiración de grupos de Office 365](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-lifecycle).

---

### <a name="updated-azure-ad-domain-services-azure-ad-ds-creation-experience"></a>Experiencia de creación de Azure AD Domain Services (Azure AD DS) actualizada

**Tipo:** Característica modificada  
**Categoría del servicio:** Azure AD Domain Services  
**Funcionalidad del producto:** Azure AD Domain Services

Hemos actualizado Azure AD Domain Services (Azure AD DS) para incluir una experiencia de creación nueva y mejorada, que lo ayudará a crear un dominio administrado en tan solo tres clics. Además, ahora puede cargar e implementar instancias de Azure AD DS desde una plantilla.

Para más información, consulte el [Tutorial: Creación y configuración de una instancia de Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance).

---

## <a name="september-2019"></a>Septiembre de 2019

### <a name="plan-for-change-deprecation-of-the-power-bi-content-packs"></a>Planeado su cambio: Desuso de los paquetes de contenido de Power BI

**Tipo:** Plan de cambio  
**Categoría del servicio:** Notificación  
**Funcionalidad del producto:** Supervisión e informes

A partir del 1 de octubre de 2019, Power BI comenzará a dejar de usar todos los paquetes de contenido, incluido el paquete de contenido de Azure AD Power BI. Como alternativa a este paquete de contenido, puede usar libros de Azure AD para obtener información sobre los servicios relacionados con Azure AD. Próximos libros de trabajo adicionales, incluidos los libros sobre las directivas de acceso condicional en modo de solo informes, información basada en el consentimiento de la aplicación, etc.

Para obtener más información acerca de los libros, consulte [Cómo usar los libros de Azure Monitor en informes de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-use-azure-monitor-workbooks). Para obtener más información sobre el desuso de los paquetes de contenido, consulte la publicación de blog [Anuncio de disponibilidad general de aplicaciones de plantilla de Power BI](https://powerbi.microsoft.com/blog/announcing-power-bi-template-apps-general-availability/).

---

### <a name="my-profile-is-renaming-and-integrating-with-the-microsoft-office-account-page"></a>Mi perfil cambia de nombre y se integra con la página de la cuenta de Microsoft Office

**Tipo:** Plan de cambio  
**Categoría del servicio:** Mi perfil/cuenta  
**Funcionalidad del producto:** Colaboración

A partir de octubre, la experiencia Mi perfil se convertirá en Mi cuenta. Como parte de ese cambio, en todas partes en las que aparezca **Mi perfil**, este pasará a llamarse **Mi cuenta**. Además del cambio de nomenclatura y algunas mejoras de diseño, la experiencia actualizada ofrecerá una integración adicional con la página de la cuenta de Microsoft Office. En concreto, podrá obtener acceso a las instalaciones y suscripciones de Office desde la página **Overview Account** (Descripción general de la cuenta), junto con las preferencias de contacto relacionadas con Office desde la página **Privacy** (Privacidad).

Para obtener más información sobre la experiencia Mi perfil (versión preliminar), consulte la [descripción general del portal Mi perfil (versión preliminar)](https://docs.microsoft.com/azure/active-directory/user-help/myprofile-portal-overview).

---

### <a name="bulk-manage-groups-and-members-using-csv-files-in-the-azure-ad-portal-public-preview"></a>Administración masiva de grupos y miembros mediante archivos CSV en el portal de Azure AD (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Administración de grupos  
**Funcionalidad del producto:** Colaboración

Nos complace anunciar la disponibilidad de la versión preliminar pública de las experiencias de administración masiva de grupos en el portal de Azure AD. Ahora puede usar un archivo CSV y el portal de Azure AD para administrar grupos y listas de miembros, entre los que se incluyen:

- Incorporación o eliminación de miembros de un grupo.

- Descarga de la lista de grupos del directorio.

- Descarga de la lista de miembros del grupo para un grupo específico.

Para obtener más información, consulte [Agregar miembros de forma masiva](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-import-members), [Eliminar miembros de forma masiva](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-remove-members), [Descarga masiva de los miembros de la lista](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download-members) y [Descarga masiva de los grupos de la lista](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-bulk-download).

---

### <a name="dynamic-consent-is-now-supported-through-a-new-admin-consent-endpoint"></a>Además se admite el consentimiento dinámico a través de un nuevo punto de conexión de consentimiento del administrador.

**Tipo:** Nueva característica  
**Categoría del servicio:** Autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** Autenticación de usuarios

Hemos creado un nuevo punto de conexión de consentimiento del administrador para admitir la opción de consentimiento dinámico, lo que le resultará útil para las aplicaciones que vayan a usar el modelo de consentimiento dinámico en la plataforma de Microsoft Identity.

Para obtener más información sobre cómo usar este nuevo punto de conexión, consulte [Usar el punto de conexión del consentimiento de administrador](https://docs.microsoft.com/azure/active-directory/develop/v2-admin-consent).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2019"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD: septiembre de 2019

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros

En septiembre de 2019, hemos agregado estas 29 nuevas aplicaciones con compatibilidad con la federación a la galería de aplicaciones:

[ScheduleLook](https://schedulelook.bbsonlineservices.net/), [MS Azure SSO Access for Ethidex Compliance Office™ - Single sign-on](https://docs.microsoft.com/azure/active-directory/saas-apps/ms-azure-sso-access-for-ethidex-compliance-office-tutorial), [iServer Portal](https://docs.microsoft.com/azure/active-directory/saas-apps/iserver-portal-tutorial), [SKYSITE](https://docs.microsoft.com/azure/active-directory/saas-apps/skysite-tutorial), [Concur Travel and Expense](https://docs.microsoft.com/azure/active-directory/saas-apps/concur-travel-and-expense-tutorial), [WorkBoard](https://docs.microsoft.com/azure/active-directory/saas-apps/workboard-tutorial), [YeeFlow](https://apps.yeeflow.com/), [ARC Facilities](https://docs.microsoft.com/azure/active-directory/saas-apps/arc-facilities-tutorial), [Luware Stratus Team](https://stratus.emea.luware.cloud/login), [Wide Ideas](https://wideideas.online/wideideas/), [Prisma Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/prisma-cloud-tutorial), [JDLT Client Hub](https://clients.jdlt.co.uk/login), [RENRAKU](https://docs.microsoft.com/azure/active-directory/saas-apps/renraku-tutorial), [SealPath Secure Browser](https://protection.sealpath.com/SealPathInterceptorWopiSaas/Open/InstallSealPathEditorOneDrive), [Prisma Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/prisma-cloud-tutorial), [Penneo](https://app.penneo.com/), [Hiretual](https://app.testhtm.com/settings/email-integration), [Cintoo Cloud](https://aec.cintoo.com/login), [Whitesource](https://docs.microsoft.com/azure/active-directory/saas-apps/whitesource-tutorial), [Hosted Heritage Online SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/hosted-heritage-online-sso-tutorial), [IDC](https://docs.microsoft.com/azure/active-directory/saas-apps/idc-tutorial), [CakeHR](https://docs.microsoft.com/azure/active-directory/saas-apps/cakehr-tutorial), [BIS](https://docs.microsoft.com/azure/active-directory/saas-apps/bis-tutorial), [Coo Kai Team Build](https://ms-contacts.coo-kai.jp/), [Sonarqube](https://docs.microsoft.com/azure/active-directory/saas-apps/sonarqube-tutorial), [Adobe Identity Management](https://docs.microsoft.com/azure/active-directory/saas-apps/adobe-identity-management-tutorial), [Discovery Benefits SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/discovery-benefits-sso-tutorial), [Amelio](https://app.amelio.co/), [iTask](https://itask.yipinapp.com/)

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial). Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="new-azure-ad-global-reader-role"></a>Nuevo rol de lector global de Azure AD

**Tipo:** Nueva característica  
**Categoría del servicio:** RBAC  
**Funcionalidad del producto:** Control de acceso

A partir del 24 de septiembre de 2019, empezaremos a implementar un nuevo rol de Azure Active Directory (AD) denominado Lector global. Este lanzamiento comenzará con los clientes de producción y de la nube global (GCC), y finalizará en octubre a nivel global.

El rol del Lector global es la contrapartida de la opción de solo lectura del administrador global. Los usuarios de este rol pueden leer la configuración y la información administrativa en los servicios de Microsoft 365, pero no pueden llevar a cabo acciones de administración. Por ello, hemos creado el rol del Lector global para que pueda reducir la cantidad de administradores globales en su organización. Debido a que las cuentas de administrador global son a la vez eficaces y vulnerables a los ataques, le recomendamos que tenga menos de cinco administradores globales. Igualmente, le recomendamos usar el rol de Lector global para la planificación, las auditorías o las investigaciones. También le recomendamos usar el rol de Lector global en combinación con otros roles de administrador limitados, como el de administrador de Exchange, para que pueda realizar el trabajo necesario sin tener que usar el rol de administrador global.

El rol del Lector global funciona con el nuevo Centro de administración de Microsoft 365, el Centro de administración de Exchange, el Centro de administración de Teams, el Centro de seguridad, el Centro de cumplimiento, el Centro de administración de Azure AD y el Centro de administración de la administración de dispositivos.

>[!NOTE]
> Al comienzo de la versión preliminar pública, el rol de Lector global no funcionará con: SharePoint, Privileged Access Management, Caja de seguridad del cliente, etiquetas de confidencialidad, Ciclo de vida de Teams, Creación de informes y análisis de llamadas de Teams, Administración de dispositivos de teléfono IP de Teams y Catálogo de aplicaciones de Teams. Todos estos servicios están diseñados para funcionar con el rol en el futuro.

Para obtener más información, consulte los [permisos del rol de administrador en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles).

---

### <a name="access-an-on-premises-report-server-from-your-power-bi-mobile-app-using-azure-active-directory-application-proxy"></a>Acceder a un servidor de informes local desde la aplicación Power BI Mobile con Azure Active Directory Application Proxy

**Tipo:** Nueva característica  
**Categoría del servicio:** Proxy de aplicaciones  
**Funcionalidad del producto:** Control de acceso

La nueva integración entre la aplicación Power BI Mobile y Azure AD Application Proxy le permite iniciar sesión de forma segura en la aplicación Power BI Mobile y ver cualquiera de los informes de la organización hospedados en la instancia local de Power BI Report Server.

Para obtener más información acerca de la aplicación de Power BI Mobile, incluyendo el lugar desde dónde descargar la aplicación, consulte el [sitio de Power BI](https://powerbi.microsoft.com/mobile/). Para obtener más información sobre cómo configurar la aplicación de Power BI Mobile con Azure AD Application Proxy, consulte [Habilitar el acceso remoto a Power BI Mobile con Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-integrate-with-power-bi).

---

### <a name="new-version-of-the-azureadpreview-powershell-module-is-available"></a>Hay una nueva versión del módulo de PowerShell de AzureADPreview disponible

**Tipo:** Característica modificada  
**Categoría del servicio:** Otros  
**Funcionalidad del producto:** Directorio

Se agregaron nuevos cmdlets al módulo AzureADPreview, para ayudar a definir y asignar roles personalizados en Azure AD, que incluyen:

- `Add-AzureADMSFeatureRolloutPolicyDirectoryObject`
- `Get-AzureADMSFeatureRolloutPolicy`
- `New-AzureADMSFeatureRolloutPolicy`
- `Remove-AzureADMSFeatureRolloutPolicy`
- `Remove-AzureADMSFeatureRolloutPolicyDirectoryObject`
- `Set-AzureADMSFeatureRolloutPolicy`

---

### <a name="new-version-of-azure-ad-connect"></a>Nueva versión de Azure AD Connect

**Tipo:** Característica modificada  
**Categoría del servicio:** Otros  
**Funcionalidad del producto:** Directorio

Hemos publicado una versión actualizada de Azure AD Connect para clientes que usen la actualización automática. Esta nueva versión incluye varias características nuevas, mejoras y correcciones de errores. Para obtener más información sobre esta versión nueva, consulte [Azure AD Connect: historial de versiones](https://docs.microsoft.com/azure/active-directory/hybrid/reference-connect-version-history#14250).

---

### <a name="azure-multi-factor-authentication-mfa-server-version-802-is-now-available"></a>Ya está disponible la versión 8.0.2 del servidor de Multi-Factor Authentication de Azure (MFA)

**Tipo:** Corregido  
**Categoría del servicio:** MFA  
**Funcionalidad del producto:** Seguridad y protección de la identidad

Si es un cliente ya existente que activó el servidor MFA antes del 1 de julio de 2019, ya puede descargar la versión más reciente del servidor MFA (versión 8.0.2). En esta nueva versión, hemos realizado lo siguiente:

- Se corrigió un problema por el que, cuando Azure AD Sync cambia el estado de un usuario de "deshabilitado" a "habilitado", se envía un correo electrónico al mismo.

- Se corrigió un problema para que los clientes puedan realizar sus actualizaciones correctamente, al tiempo que se sigue usando la funcionalidad de etiquetas.

- Se agregó el código de país de Kosovo (+ 383).

- Se agregó un registro de auditoría de omisión por única vez a MultiFactorAuthSvc.log.

- Se mejoró el rendimiento del SDK del servicio web.

- Se corrigieron otros errores menores.

A partir del 1 de julio de 2019, Microsoft dejó de ofrecer el servidor de MFA en las nuevas implementaciones. Los clientes nuevos que quieran exigir la autenticación multifactor a sus usuarios deberán usar Azure Multi-Factor Authentication basado en la nube. Para obtener más información, consulte [Planificación de una implementación de Azure Multi-Factor Authentication basada en la nube](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted).

---

## <a name="august-2019"></a>Agosto de 2019

### <a name="enhanced-search-filtering-and-sorting-for-groups-is-available-in-the-azure-ad-portal-public-preview"></a>Funciones de búsqueda, filtrado y ordenación mejorados para los grupos están disponibles en el portal de Azure AD (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Administración de grupos  
**Funcionalidad del producto:** Colaboración

Nos complace anunciar la disponibilidad de la versión preliminar pública de las experiencias mejoradas relacionadas con grupos en el portal de Azure AD. Estas mejoras ayudan a administrar mejor los grupos y las listas de miembros, ya que proporcionan:

- Funcionalidades de búsqueda avanzada, como la búsqueda de subcadenas en listas de grupos.
- Opciones avanzadas de filtrado y ordenación en las listas de miembros y propietarios.
- Nuevas funcionalidades de búsqueda para listas de miembros y propietarios.
- Recuentos de grupos más precisos para grupos grandes.

Para obtener más información, consulte [Administración de grupos en Azure Portal](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-members-azure-portal?context=azure/active-directory/users-groups-roles/context/ugr-context).

---

### <a name="new-custom-roles-are-available-for-app-registration-management-public-preview"></a>Nuevos roles personalizados están disponibles para la administración de registro de aplicaciones (Versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** RBAC  
**Funcionalidad del producto:** Control de acceso

Los roles personalizados (disponibles con una suscripción Azure AD P1 o P2) ahora pueden ayudarle a proporcionar acceso específico, permitiéndole crear definiciones de roles con permisos específicos y, a continuación, asignar esos roles a recursos específicos. Actualmente, se crean roles personalizados mediante el uso de permisos para administrar los registros de aplicaciones y, a continuación, se asigna el rol a una aplicación específica. Para obtener más información sobre los roles personalizados, consulte [Roles de administrador personalizados en Azure Active Directory (versión preliminar)](https://docs.microsoft.com/azure/active-directory/users-groups-roles/roles-custom-overview).

Si necesita permisos adicionales o recursos admitidos, que no ve actualmente, puede enviar comentarios a nuestro [sitio de comentarios de Azure](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=166032) y agregaremos su solicitud a nuestra guía de ruta de actualización.

---

### <a name="new-provisioning-logs-can-help-you-monitor-and-troubleshoot-your-app-provisioning-deployment-public-preview"></a>Los nuevos registros de aprovisionamiento pueden ayudarle a supervisar y solucionar problemas de la implementación de aprovisionamiento de aplicaciones (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** Administración del ciclo de vida de la identidad

Nuevos registros de aprovisionamiento disponibles para ayudarle a supervisar y solucionar problemas de la implementación de aprovisionamiento de usuarios y grupos. Estos nuevos archivos de registro incluyen información sobre:

- Qué grupos se han creado correctamente en [ServiceNow](https://docs.microsoft.com/azure/active-directory/saas-apps/servicenow-provisioning-tutorial)
- Cómo se han importado los roles de [Amazon Web Services(AWS)](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-web-service-tutorial#configure-and-test-azure-ad-single-sign-on-for-amazon-web-services-aws)
- Qué empleados no se importaron desde [WorkDay](https://docs.microsoft.com/azure/active-directory/saas-apps/workday-inbound-tutorial)

Para obtener más información, consulte [Informes de aprovisionamiento en el portal de Azure Active Directory (versión preliminar)](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-provisioning-logs).

---

### <a name="new-security-reports-for-all-azure-ad-administrators-general-availability"></a>Nuevos informes de seguridad para todos los administradores de Azure AD (Disponibilidad general)

**Tipo:** Nueva característica  
**Categoría del servicio:** Protección de identidad  
**Funcionalidad del producto:** Seguridad y protección de la identidad

De forma predeterminada, todos los administradores de Azure AD podrán acceder pronto a informes de seguridad modernos dentro de Azure AD. Hasta el final de septiembre, podrá usar el banner de la parte superior de los informes de seguridad modernos para volver a los informes antiguos.

Los informes de seguridad modernos proporcionarán funciones adicionales de las versiones anteriores, entre las que se incluyen:

- Filtrado y ordenación avanzados
- Acciones en masa, como descartar el riesgo del usuario
- Confirmación de entidades en peligro o seguras
- Estado de riesgo, que abarca: En riesgo, Descartado, Corregido y Confirmado (en peligro)
- Nuevas detecciones relacionadas con riesgos (disponibles para suscriptores de Azure AD Premium)

Para obtener más información, consulte [ Usuarios riesgosos ](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-users), [ Inicios de sesión riesgosos ](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-sign-ins) y [ Detecciones de riesgos ](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risk-detections).

---

### <a name="user-assigned-managed-identity-is-available-for-virtual-machines-and-virtual-machine-scale-sets-general-availability"></a>La identidad administrada asignada por el usuario está disponible para máquinas virtuales y conjuntos de escalado de máquinas virtuales (Disponibilidad general)

**Tipo:** Nueva característica  
**Categoría del servicio:** Identidades administradas de recursos de Azure  
**Funcionalidad del producto:** Experiencia para el desarrollador

Las identidades administradas asignadas por el usuario ahora están generalmente disponibles para máquinas virtuales y conjuntos de escalado de máquinas virtuales. Como parte de esto, Azure puede crear una identidad en el inquilino de Azure AD que sea de confianza para la suscripción en uso y se puede asignar a una o varias instancias de servicio de Azure. Para obtener más información sobre las identidades administradas asignadas por el usuario, consulte [ ¿Qué son las identidades administradas para los recursos de Azure? ](https://aka.ms/azuremanagedidentity).

---

### <a name="users-can-reset-their-passwords-using-a-mobile-app-or-hardware-token-general-availability"></a>Los usuarios pueden restablecer sus contraseñas mediante una aplicación móvil o un token de hardware (Disponibilidad general)

**Tipo:** Característica modificada  
**Categoría del servicio:** Restablecimiento de contraseñas de autoservicio  
**Funcionalidad del producto:** Autenticación de usuarios

Los usuarios que han registrado una aplicación móvil en su organización ahora pueden restablecer su propia contraseña mediante la aprobación de una notificación de la aplicación de Microsoft Authenticator o mediante la introducción de un código de su aplicación móvil o token de hardware.

Para obtener más información, [Consulte cómo funciona: Autoservicio de restablecimiento de contraseña de Azure AD](https://aka.ms/authappsspr). Para obtener más información sobre la experiencia del usuario, consulte [Información general sobre cómo restablecer su propia contraseña profesional o educativa](https://docs.microsoft.com/azure/active-directory/user-help/user-help-password-reset-overview).

---

### <a name="adalnet-ignores-the-msalnet-shared-cache-for-on-behalf-of-scenarios"></a>ADAL.NET ignora la memoria caché compartida de MSAL.NET para escenarios en representación

**Tipo:** Corregido  
**Categoría del servicio:** Autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** Autenticación de usuarios

A partir de la versión 5.0.0-preview de la biblioteca de autenticación de Azure AD (ADAL.NET), los desarrolladores de aplicaciones deben [serializar una caché por cuenta para las aplicaciones web y API web](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/wiki/Token-cache-serialization#custom-token-cache-serialization-in-web-applications--web-api). En caso contrario, algunos escenarios que usan el [flujo en representación de](https://docs.microsoft.com/azure/active-directory/develop/scenario-web-api-call-api-app-configuration#on-behalf-of-flow), junto con algunos casos de uso específicos de `UserAssertion`, pueden dar como resultado una elevación de privilegios. Para evitar esta vulnerabilidad, ADAL.NET ignora ahora la caché compartida de la biblioteca de autenticación de Microsoft para dotnet (MSAL.NET) para escenarios en representación.

Para más información acerca de este problema, consulte [Vulnerabilidad de elevación de privilegios de la Biblioteca de autenticación de Active Directory](https://portal.msrc.microsoft.com/security-guidance/advisory/CVE-2019-1258).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2019"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD: agosto de 2019

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros

En agosto de 2019, hemos agregado estas 26 nuevas aplicaciones con compatibilidad con la federación para la galería de aplicaciones:

[Civic Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/civic-platform-tutorial), [Amazon Business](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-business-tutorial), [ProNovos Ops Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/pronovos-ops-manager-tutorial), [Cognidox](https://docs.microsoft.com/azure/active-directory/saas-apps/cognidox-tutorial), [Viareport's Inativ Portal (Europa)](https://docs.microsoft.com/azure/active-directory/saas-apps/viareports-inativ-portal-europe-tutorial), [Azure Databricks](https://azure.microsoft.com/services/databricks), [Robin](https://docs.microsoft.com/azure/active-directory/saas-apps/robin-tutorial), [Academy Attendance](https://docs.microsoft.com/azure/active-directory/saas-apps/academy-attendance-tutorial), [Priority Matrix](https://sync.appfluence.com/pmwebng/), [Cousto MySpace](https://cousto.platformers.be/account/login), [Uploadcare](https://uploadcare.com/accounts/signup/), [Carbonite Endpoint Backup](https://docs.microsoft.com/azure/active-directory/saas-apps/carbonite-endpoint-backup-tutorial), [CPQSync by Cincom](https://docs.microsoft.com/azure/active-directory/saas-apps/cpqsync-by-cincom-tutorial), [Chargebee](https://docs.microsoft.com/azure/active-directory/saas-apps/chargebee-tutorial), [deliver.media™ Portal](https://portal.deliver.media), [Frontline Education](https://docs.microsoft.com/azure/active-directory/saas-apps/frontline-education-tutorial), [F5](https://www.f5.com/products/security/access-policy-manager), [stashcat AD connect](https://www.stashcat.com), [Blink](https://docs.microsoft.com/azure/active-directory/saas-apps/blink-tutorial), [Vocoli](https://docs.microsoft.com/azure/active-directory/saas-apps/vocoli-tutorial), [ProNovos Analytics](https://docs.microsoft.com/azure/active-directory/saas-apps/pronovos-analytics-tutorial), [Sigstr](https://docs.microsoft.com/azure/active-directory/saas-apps/sigstr-tutorial), [Darwinbox](https://docs.microsoft.com/azure/active-directory/saas-apps/darwinbox-tutorial), [Watch by Colors](https://docs.microsoft.com/azure/active-directory/saas-apps/watch-by-colors-tutorial), [Harness](https://docs.microsoft.com/azure/active-directory/saas-apps/harness-tutorial), [EAB Navigate Strategic Care](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-strategic-care-tutorial)

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial). Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="new-versions-of-the-azuread-powershell-and-azureadpreview-powershell-modules-are-available"></a>Están disponibles nuevas versiones de los módulos AzureAD PowerShell y AzureADPreview PowerShell.

**Tipo:** Característica modificada  
**Categoría del servicio:** Otros  
**Funcionalidad del producto:** Directorio

Están disponibles nuevas versiones de los módulos AzureAD PowerShell y AzureADPreview PowerShell.

- Se agregó un nuevo parámetro `-Filter` al parámetro `Get-AzureADDirectoryRole` en el módulo AzureAD. Este parámetro le ayuda a filtrar por los roles de directorio devueltos por el cmdlet.
- Se agregaron nuevos cmdlets al módulo AzureADPreview, para ayudar a definir y asignar roles personalizados en Azure AD, que incluyen:

    - `Get-AzureADMSRoleAssignment`
    - `Get-AzureADMSRoleDefinition`
    - `New-AzureADMSRoleAssignment`
    - `New-AzureADMSRoleDefinition`
    - `Remove-AzureADMSRoleAssignment`
    - `Remove-AzureADMSRoleDefinition`
    - `Set-AzureADMSRoleDefinition`

---

### <a name="improvements-to-the-ui-of-the-dynamic-group-rule-builder-in-the-azure-portal"></a>Mejoras en la interfaz de usuario del generador de reglas de grupos dinámicos en Azure Portal

**Tipo:** Característica modificada  
**Categoría del servicio:** Administración de grupos  
**Funcionalidad del producto:** Colaboración

Hemos realizado algunas mejoras de la interfaz de usuario en el generador de reglas de grupo dinámico, disponible en el Azure Portal, para ayudarle a configurar más fácilmente una nueva regla o cambiar las reglas existentes. Esta mejora del diseño le permite crear reglas con hasta cinco expresiones, en lugar de solo una. También hemos actualizado la lista de propiedades del dispositivo para quitar las propiedades del dispositivo en desuso.

Para obtener más información, consulte [Administrar reglas de pertenencia dinámica](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership).

---

### <a name="new-microsoft-graph-app-permission-available-for-use-with-access-reviews"></a>Nuevo permiso de aplicación Microsoft Graph disponible para su uso con las revisiones de acceso

**Tipo:** Característica modificada  
**Categoría del servicio:** Revisiones de acceso  
**Funcionalidad del producto:** Identity Governance

Hemos introducido un nuevo permiso de aplicación Microsoft Graph, `AccessReview.ReadWrite.Membership`, que permite a las aplicaciones crear y recuperar automáticamente revisiones de acceso para pertenencias a grupos y asignaciones de aplicaciones. Este permiso se puede usar en los trabajos programados o como parte de la automatización, sin necesidad de un contexto de usuario que haya iniciado sesión.

Para obtener más información, consulte el [Ejemplo de cómo crear revisiones de acceso de Azure AD mediante los permisos de la aplicación Microsoft Graph con el blog de PowerShell ](https://techcommunity.microsoft.com/t5/Azure-Active-Directory/Example-how-to-create-Azure-AD-access-reviews-using-Microsoft/m-p/807241).

---

### <a name="azure-ad-activity-logs-are-now-available-for-government-cloud-instances-in-azure-monitor"></a>Los registros de actividad de Azure AD ahora están disponibles para las instancias de la nube de administración pública en Azure Monitor

**Tipo:** Característica modificada  
**Categoría del servicio:** Notificación  
**Funcionalidad del producto:** Supervisión e informes

Nos complace anunciar que los registros de actividad de Azure AD ahora están disponibles para las instancias de la nube de administración pública en Azure Monitor Ahora puede enviar registros de Azure AD a su cuenta de almacenamiento o a un centro de eventos para integrarlos con las herramientas SIEM, como [Sumologic](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-sumologic), [Splunk](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-splunk) y [Arcsight](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-arcsight). 

Para obtener más información sobre la configuración de Azure Monitor, consulte [Registros de actividad de Azure AD en Azure Monitor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor#cost-considerations).

---

### <a name="update-your-users-to-the-new-enhanced-security-info-experience"></a>Actualice a sus usuarios a la nueva experiencia de información de seguridad mejorada

**Tipo:** Característica modificada  
**Categoría del servicio:**  Autenticaciones (inicios de sesión)   
**Funcionalidad del producto:** Autenticación de usuarios

El 25 de septiembre de 2019, desactivaremos la antigua experiencia de información de seguridad no mejorada para registrar y administrar la información de seguridad del usuario y solo activaremos la nueva [versión mejorada](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Cool-enhancements-to-the-Azure-AD-combined-MFA-and-password/ba-p/354271). Esto significa que los usuarios ya no podrán usar la experiencia antigua.

Para más información sobre la experiencia de información de seguridad mejorada, consulte nuestra [documentación de administración](https://aka.ms/securityinfodocs) y nuestra [documentación de usuario](https://aka.ms/securityinfoguide).

#### <a name="to-turn-on-this-new-experience-you-must"></a>Para activar esta nueva experiencia, debe:

1. Inicie sesión en Azure Portal como administrador global o administrador de usuarios.

2. Vaya a **Azure Active Directory > Configuración de usuario > Administrar la configuración de las características en versión preliminar del panel de acceso**.

3. En el área **Mejoras para los usuarios a la hora de utilizar las características en versión preliminar para registrar y administrar la información de seguridad**, seleccione **Seleccionado** y luego elija un grupo de usuarios o elija **Todos** a fin de activar esta función para todos los usuarios del inquilino.

4. En el **Los usuarios pueden usar las características de vista previa para registrar y administrar el área de información **de seguridad**,** seleccione **Ninguno**.

5. Guarde la configuración.

    Después de guardar la configuración, ya no tendrá acceso a la antigua experiencia de información de seguridad.

>[!Important]
>Si no completa estos pasos antes del 25 de septiembre de 2019, el inquilino de Azure Active Directory se habilitará automáticamente para mejorar la experiencia. Si le queda alguna duda, póngase en contacto con nosotros registrationpreview@microsoft.com.

---

### <a name="authentication-requests-using-post-logins-will-be-more-strictly-validated"></a>Las solicitudes de autenticación que utilizan inicios de sesión POST serán validadas de forma más rigurosa

**Tipo:** Característica modificada  
**Categoría del servicio:** Autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** Estándares

A partir del 2 de septiembre de 2019, las solicitudes de autenticación que usan el método POST se validarán de forma más rigurosa con los estándares HTTP. Concretamente, los espacios y las comillas dobles (") ya no se quitarán de los valores del formulario de solicitud. No se espera que estos cambios interrumpan ningún cliente existente y ayudarán a garantizar que las solicitudes enviadas a Azure AD se controlan de forma confiable en todo momento.

Para obtener más información, consulte [Notificaciones de cambios de última hora de Azure AD](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#post-form-semantics-will-be-enforced-more-strictly---spaces-and-quotes-will-be-ignored).

---
