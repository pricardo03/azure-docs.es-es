---
title: Novedades Notas de la versión de Azure AD | Microsoft Docs
description: Obtenga información acerca de las novedades de Azure Active Directory (Azure AD), como, por ejemplo, las notas de la versión más recientes, los problemas conocidos, las correcciones de errores, las funcionalidades en desuso y los próximos cambios.
services: active-directory
author: eross-msft
manager: mtillman
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.component: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: lizross
ms.reviewer: dhanyahk
ms.openlocfilehash: d39cfddc42ea0e03f6b0f6c8d1c0160839742518
ms.sourcegitcommit: d1aef670b97061507dc1343450211a2042b01641
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/27/2018
ms.locfileid: "47393917"
---
# <a name="whats-new-in-azure-active-directory"></a>¿Cuáles son las novedades de Azure Active Directory?

>Reciba notificaciones para volver a visitar esta página y obtener actualizaciones; para ello, agregue esta [URL](https://docs.microsoft.com/api/search/rss?search=%22whats%20new%20in%20azure%20active%20directory%22&locale=en-us/) al lector de fuentes ![icono RSS](./media/whats-new/feed-icon-16x16.png).

En Azure AD se realizan mejoras de forma continua. Para mantenerse al día con los avances más recientes, este artículo proporciona información acerca de los elementos siguientes:

- Versiones más recientes
- Problemas conocidos
- Corrección de errores
- Funciones obsoletas
- Planes de cambios

Esta página se actualiza mensualmente, por lo que se recomienda visitarla con frecuencia.

---
## <a name="september-2018"></a>Septiembre de 2018
 
### <a name="updated-administrator-role-permissions-for-dynamic-groups"></a>Permisos de rol de administrador actualizados para grupos dinámicos

**Tipo:** fijo  
**Categoría del servicio:** administración de grupos  
**Funcionalidad del producto:** colaboración

Se ha corregido un problema, por lo que determinados roles de administrador ya pueden crear y actualizar reglas de pertenencia dinámicas, sin necesidad de ser el propietario del grupo.

Los roles son:

- Administrador global o escritor de la empresa

- Administrador de servicios de Intune

- Administrador de cuenta de usuario

Para más información, consulte [Creación de un grupo dinámico y comprobación de su estado](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-create-rule)

---

### <a name="simplified-single-sign-on-sso-configuration-settings-for-some-third-party-apps"></a>Valores de configuración simplificada de inicio de sesión único (SSO) para algunas aplicaciones de terceros

**Tipo:** nueva característica  
**Categoría del servicio:** aplicaciones empresariales  
**Funcionalidad del producto:** SSO

Somos conscientes de que la configuración del inicio de sesión único (SSO) para aplicaciones de SaaS (software como servicio) puede ser un desafío debido a la naturaleza única de la configuración de cada aplicación. Hemos creado una configuración simplificada para rellenar de forma automática los valores de configuración de SSO para las siguientes aplicaciones de SaaS de terceros:

- Zendesk

- ArcGis Online

- Jamf Pro

Para empezar a usar esta experiencia de un solo clic, vaya a la página **Azure Portal** > **Configuración de SSO** de la aplicación. Para más información, consulte [Integración de aplicación SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)

---

### <a name="azure-active-directory---where-is-your-data-located-page"></a>Página Azure Active Directory - Where is your data located? (Azure Active Directory: dónde se encuentran los datos)

**Tipo:** nueva característica  
**Categoría del servicio:** otros  
**Funcionalidad del producto:** GoLocal

Seleccione la región de su empresa en la página **Azure Active Directory - Where is your data located?** (Azure Active Directory: dónde se encuentran los datos) para ver qué centro de datos de Azure alberga los datos en reposo de Azure AD para todos los servicios de Azure AD. Puede filtrar la información por los servicios específicos de Azure AD para la región de su empresa.

Para acceder a esta característica y para más información, consulte [Azure Active Directory - Where is your data located?](http://aka.ms/AADDataMap) (Azure Active Directory: dónde se encuentran los datos).

---

### <a name="new-deployment-plan-available-for-the-my-apps-access-panel"></a>Nuevo plan de implementación disponible para el panel de acceso a Mis aplicaciones

**Tipo:** nueva característica  
**Categoría del servicio:** Mis aplicaciones  
**Funcionalidad del producto:** SSO

Consulte el nuevo plan de implementación disponible para el panel de acceso a Mis aplicaciones (http://aka.ms/deploymentplans).
El panel de acceso a Mis aplicaciones proporciona a los usuarios un único lugar en el que pueden buscar y acceder a sus aplicaciones. Este portal también proporciona a los usuarios oportunidades de autoservicio, como por ejemplo solicitar acceso a aplicaciones y grupos, o administrar el acceso a estos recursos en nombre de otros.

Para más información, consulte [¿Qué es el portal Mis aplicaciones?](https://docs.microsoft.com/azure/active-directory/user-help/active-directory-saas-access-panel-introduction)

---

### <a name="new-troubleshooting-and-support-tab-on-the-sign-ins-logs-page-of-the-azure-portal"></a>Pestaña nueva solución de problemas y soporte técnico de la página de registros de inicios de sesión de Azure Portal

**Tipo:** nueva característica  
**Categoría del servicio:** informes  
**Funcionalidad del producto:** supervisión e informes

El fin de la nueva pestaña **Solución de problemas y soporte técnico** de la página **Inicios de sesión** de Azure Portal es ayudar a los administradores e ingenieros de soporte técnico a solucionar problemas relacionados con los inicios de sesión de Azure AD. Esta nueva pestaña proporciona el código de error, mensaje de error y las recomendaciones de corrección (si existen) para ayudar a solucionar el problema. Si no puede resolver el problema, también le ofrecemos una nueva forma de crear una incidencia de soporte técnico mediante **Copiar al Portapapeles**, que rellena los campos **Id. de solicitud** y **Fecha (UTC)** del archivo de registro en su incidencia de soporte técnico.  

![Registros de inicio de sesión con la nueva pestaña](media/whats-new/troubleshooting-and-support.png)

---

### <a name="enhanced-support-for-custom-extension-properties-used-to-create-dynamic-membership-rules"></a>Compatibilidad mejorada con las propiedades de extensión personalizadas utilizadas para crear reglas de pertenencia dinámica

**Tipo:** característica modificada  
**Categoría del servicio:** administración de grupos  
**Funcionalidad del producto:** colaboración

Con esta actualización, ahora puede hacer clic en el vínculo **Get custom extension properties** (Obtener propiedades de extensión personalizadas) desde el generador de reglas de grupo de usuarios dinámico, escriba el identificador de aplicación único y recibir la lista completa de las propiedades de extensión personalizadas para usarla al crear una regla de pertenencia dinámica para los usuarios. Esta lista también se puede actualizar esta lista para obtener nuevas propiedades de extensión personalizada para la aplicación.

Para más información acerca del uso de propiedades de extensión personalizadas para las reglas de pertenencia dinámica, consulte [Propiedades de extensión y propiedades de extensión personalizadas](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-dynamic-membership#extension-properties-and-custom-extension-properties)

---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Nuevas aplicaciones cliente aprobadas para el acceso condicional basado en aplicaciones de Azure AD

**Tipo:** plan de cambio  
**Categoría del servicio:** acceso condicional  
**Funcionalidad del producto:** seguridad y protección de la identidad

Las siguientes aplicaciones se incluyen en la lista de [aplicaciones cliente aprobadas](https://docs.microsoft.com/azure/active-directory/conditional-access/technical-reference.md#approved-client-app-requirement):

- Microsoft To-Do

- Microsoft Stream

Para más información, consulte:

- [Acceso condicional basado en aplicaciones de Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="new-support-for-self-service-password-reset-from-the-windows-7881-lock-screen"></a>Nueva compatibilidad con Autoservicio de restablecimiento de contraseña desde la pantalla de bloqueo de Windows 7/8/8.1

**Tipo:** nueva característica  
**Categoría del servicio:** SSPR  
**Funcionalidad del producto:** autenticación de usuarios

Después de configurar esta nueva característica los usuarios verán un vínculo para restablecer su contraseña desde la pantalla de **bloqueo** de un dispositivo que ejecuta Windows 7, Windows 8 o Windows 8.1. Al hacer clic en ese vínculo, se guía al usuario por el mismo flujo de restablecimiento de contraseña que por el explorador web.

Para más información, consulte [How to: Enable password reset from Windows 7, 8, and 8.1](https://aka.ms/ssprforwindows78) (Procedimiento para habilitar el restablecimiento de contraseña desde Windows 7, 8, and 8.1)

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Aviso de cambio: los códigos de autorización ya no estarán disponibles para su reutilización 

**Tipo:** plan de cambio  
**Categoría del servicio:** autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** autenticación de usuarios

A partir del 10 de octubre de 2018, Azure AD dejará de aceptar los códigos de autenticación que se usaban anteriormente para las aplicaciones nuevas. Este cambio de seguridad ayuda a poner Azure AD en consonancia con la especificación de OAuth y se aplicará en los puntos de conexión v1 y v2.

Si la aplicación reutiliza códigos de autorización para obtener tokens para varios recursos, es recomendable que use el código para obtener un token de actualización y, a continuación, utilice este para adquirir tokens adicionales para otros recursos. Los códigos de autorización solo se pueden usar una vez, pero los tokens de actualización se pueden usar varias veces en varios recursos. En cualquier aplicación que intente reutilizar un código de autenticación durante el flujo de código de OAuth aparecerá el error invalid_grant.

>[!Note]
>En un esfuerzo por ayudarle a reducir al mínimo las aplicaciones interrumpidas, se ha hecho una excepción con las aplicaciones que usan este patrón y tienen más de 10 inicios de sesión por día.

Para este y otros cambios relacionados con los protocolos, consulte [la lista completa de las novedades de la autenticación](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---september-2018"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD: septiembre de 2018

**Tipo:** nueva característica  
**Categoría del servicio:** aplicaciones empresariales  
**Funcionalidad del producto:** integración de terceros
 
En septiembre de 2018, hemos agregado 16 nuevas aplicaciones con compatibilidad con la federación a la galería de aplicaciones:

[Uberflip](https://docs.microsoft.com/azure/active-directory/saas-apps/uberflip-tutorial), [Comeet Recruiting Software](https://docs.microsoft.com/azure/active-directory/saas-apps/comeetrecruitingsoftware-tutorial), [Workteam](https://docs.microsoft.com/azure/active-directory/saas-apps/workteam-tutorial), [ArcGIS Enterprise](https://docs.microsoft.com/azure/active-directory/saas-apps/arcgisenterprise-tutorial), [Nuclino](https://docs.microsoft.com/azure/active-directory/saas-apps/nuclino-tutorial), [JDA Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/jdacloud-tutorial), [Snowflake](https://docs.microsoft.com/azure/active-directory/saas-apps/snowflake-tutorial), NavigoCloud, [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-tutorial), join.me, [ZephyrSSO](https://docs.microsoft.com/azure/active-directory/saas-apps/zephyrsso-tutorial), [Silverback](https://docs.microsoft.com/azure/active-directory/saas-apps/silverback-tutorial), Riverbed Xirrus EasyPass, [Rackspace SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/rackspacesso-tutorial), Enlyft SSO for Azure, SurveyMonkey, [Convene](https://docs.microsoft.com/azure/active-directory/saas-apps/convene-tutorial) y [dmarcian](https://docs.microsoft.com/azure/active-directory/saas-apps/dmarcian-tutorial)

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial). Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="support-for-additional-claims-transformations-methods"></a>Compatibilidad con métodos de transformación de notificaciones adicionales

**Tipo:** nueva característica  
**Categoría del servicio:** aplicaciones empresariales  
**Funcionalidad del producto:** SSO

Hemos introducido nuevos métodos de transformación de notificaciones, ToLower() y ToUpper(), que se pueden aplicar a los tokens SAML desde la página **Configuración del inicio de sesión único** basada en SAML.

Para más información, consulte [Procedimientos para personalizar las notificaciones emitidas en el token SAML para aplicaciones empresariales en Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)

---

### <a name="updated-saml-based-app-configuration-ui-preview"></a>Interfaz de usuario de la configuración de aplicaciones basadas en SAML actualizada (versión preliminar)

**Tipo:** característica modificada  
**Categoría del servicio:** aplicaciones empresariales  
**Funcionalidad del producto:** SSO

Como parte de nuestra interfaz de usuario de la configuración de aplicaciones basadas en SAML actualizada, obtendrá:

- Una experiencia actualizada del tutorial para configurar aplicaciones basadas en SAML.

- Mayor visibilidad de lo que falta o es incorrecto en la configuración.

- La capacidad para agregar varias direcciones de correo electrónico para la notificación de expiración de certificados.

- Nuevos métodos de transformación de notificaciones, ToLower() ToUpper(), y más.

- Una forma de cargar su propio certificado de firma de tokens para las aplicaciones empresariales.

- Una forma de establecer el formato de NameID para aplicaciones SAML y una forma de establecer el valor de NameID como extensiones de directorio.

Para activar esta vista actualizada, haga clic en el vínculo **Try out our new experience** (Probar nuestra nueva experiencia) en la parte superior de la página **Inicio de sesión único**. Para más información, consulte [Tutorial: Configuración del inicio de sesión único basado en SAML para una aplicación con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-single-sign-on-portal).

---

## <a name="august-2018"></a>Agosto de 2018

### <a name="changes-to-azure-active-directory-ip-address-ranges"></a>Cambios realizados en los intervalos de direcciones IP de Azure Active Directory

**Tipo:** plan de cambio  
**Categoría del servicio:** otros  
**Funcionalidad del producto:** plataforma

Estamos introduciendo intervalos IP mayores en Azure AD, lo que significa que si ha configurado los intervalos IP de Azure AD para los firewalls, enrutadores o grupos de seguridad de red, deberá actualizarlos. Realizamos esta actualización para que no tenga que volver a cambiar su firewall, enrutador ni las configuraciones de intervalo IP de los grupos de seguridad de red cuando Azure AD agregue nuevos puntos de conexión. 

El tráfico se moverá a estos nuevos intervalos durante los próximos dos meses. Para continuar con un servicio ininterrumpido, debe agregar estos valores actualizados a las direcciones IP antes del 10 de septiembre de 2018:

- 20.190.128.0/18 

- 40.126.0.0/18 

Se recomienda encarecidamente no quitar los intervalos IP antiguos hasta que todo el tráfico se haya movido a los nuevos intervalos. Para actualizaciones sobre el movimiento y saber cuándo quitar los intervalos antiguos, consulte [Office 365 URLs and IP address ranges](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) (Direcciones URL e intervalos IP de Office 365).

---

### <a name="change-notice-authorization-codes-will-no-longer-be-available-for-reuse"></a>Aviso de cambio: los códigos de autorización ya no estarán disponibles para su reutilización 

**Tipo:** plan de cambio  
**Categoría del servicio:** autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** autenticación de usuarios

A partir del 10 de octubre de 2018, Azure AD dejará de aceptar los códigos de autenticación que se usaban anteriormente para las aplicaciones nuevas. Este cambio de seguridad ayuda a poner Azure AD en consonancia con la especificación de OAuth y se aplicará en los puntos de conexión v1 y v2.

Si la aplicación reutiliza códigos de autorización para obtener tokens para varios recursos, es recomendable que use el código para obtener un token de actualización y, a continuación, utilice este para adquirir tokens adicionales para otros recursos. Los códigos de autorización solo se pueden usar una vez, pero los tokens de actualización se pueden usar varias veces en varios recursos. En cualquier aplicación que intente reutilizar un código de autenticación durante el flujo de código de OAuth aparecerá el error invalid_grant.

>[!Note]
>En un esfuerzo por ayudarle a reducir al mínimo las aplicaciones interrumpidas, se ha hecho una excepción con las aplicaciones que usan este patrón y tienen más de 10 inicios de sesión por día.

Para este y otros cambios relacionados con los protocolos, consulte [la lista completa de las novedades de la autenticación](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes).
 
---

### <a name="converged-security-info-management-for-self-service-password-sspr-and-multi-factor-authentication-mfa"></a>Administración de información de seguridad convergida para restablecimiento de contraseña de autoservicio (SSPR) y Multi-Factor Authentication (MFA)

**Tipo:** nueva característica  
**Categoría del servicio:** SSPR  
**Funcionalidad del producto:** autenticación de usuarios
 
Esta nueva característica ayuda a los usuarios a administrar su información de seguridad (por ejemplo, número de teléfono, aplicación móvil, etc.) para SSPR y MFA en una sola ubicación y una misma experiencia, al contrario que sucedía anteriormente cuando se realizaba en dos ubicaciones diferentes.

Esta experiencia convergente también funciona para los usuarios que utilizan MFA o SSPR. Además, aunque la organización no aplique el registro de MFA o de SSPR, los usuarios pueden registrar todos los métodos de información de seguridad que permita la organización desde el portal Mis aplicaciones.

Se trata de una versión preliminar pública opcional. Los administradores pueden activar la nueva experiencia (si lo desean) para un grupo de usuarios seleccionado o para todos los usuarios de un inquilino. Para más información sobre la experiencia convergente, consulte el [blog de experiencia convergente](https://cloudblogs.microsoft.com/enterprisemobility/2018/08/06/mfa-and-sspr-updates-now-in-public-preview/)

---

### <a name="new-http-only-cookies-setting-in-azure-ad-application-proxy-apps"></a>Nueva configuración de las cookies solo HTTP en las aplicaciones de Azure AD Application Proxy

**Tipo:** nueva característica  
**Categoría del servicio:** proxy de aplicaciones  
**Funcionalidad del producto:** control de acceso

Hay un nuevo valor llamado, **HTTP-Only Cookies** (Cookies solo HTTP) en las aplicaciones de Application Proxy. Este valor ayuda a proporcionar un nivel extra de seguridad incluyendo la marca HTTPOnly en el encabezado de la respuesta HTTP para las cookies de acceso a Application Proxy y las de la sesión, impidiendo el acceso a cookies procedentes de un script en el lado del cliente e impidiendo acciones como la copia o modificación de la cookie. Aunque esta marca no se ha usado anteriormente, las cookies siempre se han cifrado y transmitido mediante una conexión SSL para facilitar su protección frente a modificaciones no adecuadas.

Este valor no es compatible con aplicaciones que usan controles ActiveX, como Escritorio remoto. Si este es su caso, es aconsejable que desactive este valor.

Para más información acerca del valor de cookies solo HTTP, consulte [Publicación de aplicaciones mediante Azure AD Application Proxy](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-publish-azure-portal).

---

### <a name="privileged-identity-management-pim-for-azure-resources-supports-management-group-resource-types"></a>Privileged Identity Management (PIM) para recursos de Azure es compatible con los tipos de recursos del grupo de administración

**Tipo:** nueva característica  
**Categoría del servicio:** Privileged Identity Management  
**Funcionalidad del producto:** Privileged Identity Management
 
La configuración de la activación y asignación Just-In-Time se puede aplicar ahora a los tipos de recursos del grupo de administración, al igual que ya lo hace con las suscripciones, grupos de recursos y recursos (como máquinas virtuales, App Services, etc). Además, cualquier usuario con un rol que proporcione acceso de administrador para un grupo de administración puede detectar y administrar ese recurso en PIM.

Para más información acerca de PIM y los recursos de Azure, consulte [Detectar y administrar recursos de Azure mediante Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-resource-roles-discover-resources)
 
---

### <a name="application-access-preview-provides-faster-access-to-the-azure-ad-portal"></a>La característica de acceso a la aplicación (versión preliminar) proporciona un acceso más rápido al portal de Azure AD

**Tipo:** nueva característica  
**Categoría del servicio:** Privileged Identity Management  
**Funcionalidad del producto:** Privileged Identity Management
 
En la actualidad, al activar un rol con PIM, puede que los permisos tarden más de 10 minutos en surtir efecto. Si opta por usar la característica de acceso a la aplicación, que se encuentra actualmente en versión preliminar pública, los administradores podrán acceder al portal de Azure AD tan pronto como se complete la solicitud de activación.

Actualmente, esta característica solo admite la experiencia del portal de Azure AD y los recursos de Azure. Para más información acerca de PIM y la característica de acceso a la aplicación, consulte [¿Qué es Azure AD Privileged Identity Management?](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/pim-configure)
 
---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---august-2018"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD: agosto de 2018

**Tipo:** nueva característica  
**Categoría del servicio:** aplicaciones empresariales  
**Funcionalidad del producto:** integración de terceros
 
En agosto de 2018, hemos agregado estas 16 nuevas aplicaciones con compatibilidad con la federación para la galería de aplicaciones:

[Hornbill](https://docs.microsoft.com/azure/active-directory/saas-apps/hornbill-tutorial), [Bridgeline Unbound](https://docs.microsoft.com/azure/active-directory/saas-apps/bridgelineunbound-tutorial), [Sauce Labs - Mobile and Web Testing](https://docs.microsoft.com/azure/active-directory/saas-apps/saucelabs-mobileandwebtesting-tutorial), [Meta Networks Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/metanetworksconnector-tutorial), [Way We Do](https://docs.microsoft.com/azure/active-directory/saas-apps/waywedo-tutorial), [Spotinst](https://docs.microsoft.com/azure/active-directory/saas-apps/spotinst-tutorial), [ProMaster (by Inlogik)](https://docs.microsoft.com/azure/active-directory/saas-apps/promaster-tutorial), SchoolBooking, [4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-tutorial), [Dossier](https://docs.microsoft.com/azure/active-directory/saas-apps/DOSSIER-tutorial), [N2F - Expense reports](https://docs.microsoft.com/azure/active-directory/saas-apps/n2f-expensereports-tutorial), [Comm100 Live Chat](https://docs.microsoft.com/azure/active-directory/saas-apps/comm100livechat-tutorial), [SafeConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/safeconnect-tutorial), [ZenQMS](https://docs.microsoft.com/azure/active-directory/saas-apps/zenqms-tutorial), [eLuminate](https://docs.microsoft.com/azure/active-directory/saas-apps/eluminate-tutorial), [Dovetale](https://docs.microsoft.com/azure/active-directory/saas-apps/dovetale-tutorial).

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial). Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="native-tableau-support-is-now-available-in-azure-ad-application-proxy"></a>Ahora ya está disponible la compatibilidad nativa con Tableau en Azure AD Application Proxy

**Tipo:** característica modificada  
**Categoría del servicio:** proxy de aplicaciones  
**Funcionalidad del producto:** control de acceso

Gracias a la actualización del protocolo de concesión de código de OpenID Connect a OAuth 2.0 para nuestro protocolo de autenticación previa, ya no es necesario realizar ninguna configuración adicional para usar Tableau con Application Proxy. Este cambio de protocolo también ayuda a Application Proxy a mejorar su compatibilidad con aplicaciones más modernas mediante el uso exclusivo de redirecciones HTTP, que normalmente son compatibles con las etiquetas de JavaScript y HTML.

Para más información sobre la compatibilidad nativa con Tableau, consulte [Azure AD Application Proxy now with native Tableau support](https://blogs.technet.microsoft.com/applicationproxyblog/2018/08/14/azure-ad-application-proxy-now-with-native-tableau-support) (Azure AD Application Proxy con compatibilidad nativa con Tableau).

---

### <a name="new-support-to-add-google-as-an-identity-provider-for-b2b-guest-users-in-azure-active-directory-preview"></a>Nueva compatibilidad para agregar Google como proveedor de identidades de usuarios invitados de B2B en Azure Active Directory (versión preliminar)

**Tipo:** nueva característica  
**Categoría del servicio:** B2B  
**Funcionalidad del producto:** B2B/B2C

Mediante la configuración de la federación con Google en su organización, puede permitir que usuarios invitados de Gmail inicien sesión en sus aplicaciones y recursos compartidos con su cuenta de Google existente sin tener que crear una cuenta Microsoft (MSA) personal o una cuenta de Azure AD.

Se trata de una versión preliminar pública opcional. Para más información sobre la federación con Google, consulte [Incorporación de Google como proveedor de identidades para los usuarios invitados de B2B](https://docs.microsoft.com/azure/active-directory/b2b/google-federation).

---

## <a name="july-2018"></a>Julio de 2018

### <a name="improvements-to-azure-active-directory-email-notifications"></a>Mejoras en las notificaciones de Azure Active Directory por correo electrónico

**Tipo:** característica modificada  
**Categoría del servicio:** otros  
**Funcionalidad del producto:** administración del ciclo de vida de las identidades
 
Los correos electrónicos de Azure Active Directory (Azure AD) ahora tienen un diseño actualizado, además de cambios en el nombre para mostrar del remitente y la dirección de correo electrónico del destinatario cuando los mensajes se envían desde los servicios siguientes:
 
- Revisiones de acceso de Azure AD
- Azure AD Connect Health 
- Azure AD Identity Protection 
- Administración de identidades con privilegios de Azure AD
- Aplicación empresarial: expiración de las notificaciones de certificado
- Aplicación empresarial: aprovisionamiento de las notificaciones de servicio
 
Las notificaciones de correo electrónico se enviarán desde la siguiente dirección de correo electrónico y nombre para mostrar:

- Dirección de correo electrónico: azure-noreply@microsoft.com
- Nombre para mostrar: Microsoft Azure
 
Para un ejemplo de algunos de los nuevos diseños de correo electrónico y más información, consulte [Email notifications in Azure AD PIM](https://go.microsoft.com/fwlink/?linkid=2005832) (Notificaciones por correo electrónico en Azure AD PIM).

---

### <a name="azure-ad-activity-logs-are-now-available-through-azure-monitor"></a>Los registros de actividad de Azure AD ahora están disponibles a través de Azure Monitor

**Tipo:** nueva característica  
**Categoría del servicio:** informes  
**Funcionalidad del producto:** supervisión e informes

Los registros de actividad de Azure AD ahora están disponibles en la versión preliminar pública de Azure Monitor (servicio supervisión en toda la plataforma de Azure). Azure Monitor ofrece una retención a largo plazo e integración sin problemas, además de estas mejoras:

- Retención a largo plazo mediante el enrutamiento de los archivos de registro en su propia cuenta de almacenamiento de Azure.

- Integración SIEM perfecta, sin necesidad de escribir o mantener scripts personalizados.

- Integración sin problemas con sus propias soluciones personalizadas, herramientas de análisis o soluciones de administración de incidentes.

Para más información sobre estas nuevas funcionalidades, consulte nuestro blog [Azure AD activity logs in Azure Monitor diagnostics is now in public preview](https://cloudblogs.microsoft.com/enterprisemobility/2018/07/26/azure-ad-activity-logs-in-azure-monitor-diagnostics-now-in-public-preview/) (Los registros de actividad de Azure AD en diagnósticos de Azure Monitor están ahora en versión preliminar pública) y nuestra documentación, [Registros de actividad de Azure AD en Azure Monitor (versión preliminar)](https://docs.microsoft.com/azure/active-directory/reporting-azure-monitor-diagnostics-overview).

---

### <a name="conditional-access-information-added-to-the-azure-ad-sign-ins-report"></a>Información de acceso condicional agregada al informe de inicios de sesión de Azure AD

**Tipo:** nueva característica  
**Categoría del servicio:** informes  
**Funcionalidad del producto:** protección y seguridad de la identidad
 
Esta actualización le permite ver qué directivas se evalúan cuando un usuario inicia sesión, junto con el resultado de la directiva. Además, el informe ahora incluye el tipo de aplicación cliente que el usuario utiliza, de manera que usted puede identificar el tráfico de protocolo heredado. Ahora también se pueden buscar entradas de informe para un identificador de correlación, que puede encontrarse en el mensaje de error de cara al usuario y puede usarse para identificar la solicitud de inicio de sesión coincidente y solucionar problemas en dicha solicitud.

---

### <a name="view-legacy-authentications-through-sign-ins-activity-logs"></a>Visualización de autenticaciones heredadas a través de los registros de actividad de inicios de sesión

**Tipo:** nueva característica  
**Categoría del servicio:** informes  
**Funcionalidad del producto:** supervisión e informes
 
Con la introducción del campo **Aplicación cliente** en los registros de actividad de inicio de sesión, los clientes ya pueden ver los usuarios que utilizan autenticaciones heredadas. Los clientes podrán acceder a esta información mediante los registros de inicio de sesión de MS Graph API o mediante los registros de actividad de inicio de sesión del portal de Azure AD, donde se puede usar el control **Aplicación cliente** para filtrar por autenticaciones heredadas. Para más información, consulte la documentación.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2018"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD: julio de 2018

**Tipo:** nueva característica  
**Categoría del servicio:** aplicaciones empresariales  
**Funcionalidad del producto:** integración de terceros
 
En julio de 2018, hemos agregado estas 16 nuevas aplicaciones con compatibilidad con la federación a nuestra galería de aplicaciones:

[Innovation Hub](https://docs.microsoft.com/azure/active-directory/saas-apps/innovationhub-tutorial), [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-tutorial), [Certain Admin SSO](https://docs.microsoft.com/azure/active-directory/saas-apps/certainadminsso-tutorial), PSUC Staging, [iPass SmartConnect](https://docs.microsoft.com/azure/active-directory/saas-apps/ipasssmartconnect-tutorial), [Screencast-O-Matic](https://docs.microsoft.com/azure/active-directory/saas-apps/screencast-tutorial), PowerSchool Unified Classroom, [Eli Onboarding](https://docs.microsoft.com/azure/active-directory/saas-apps/elionboarding-tutorial), [Bomgar Remote Support](https://docs.microsoft.com/azure/active-directory/saas-apps/bomgarremotesupport-tutorial), [Nimblex](https://docs.microsoft.com/azure/active-directory/saas-apps/nimblex-tutorial), [Imagineer WebVision](https://docs.microsoft.com/azure/active-directory/saas-apps/imagineerwebvision-tutorial), [Insight4GRC](https://docs.microsoft.com/azure/active-directory/saas-apps/insight4grc-tutorial), [SecureW2 JoinNow Connector](https://docs.microsoft.com/azure/active-directory/saas-apps/securejoinnow-tutorial), [Kanbanize](https://review.docs.microsoft.com/azure/active-directory/saas-apps/kanbanize-tutorial), [SmartLPA](https://review.docs.microsoft.com/azure/active-directory/saas-apps/smartlpa-tutorial) y [Skills Base](https://docs.microsoft.com/azure/active-directory/saas-apps/skillsbase-tutorial).

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial). Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://aka.ms/azureadapprequest).

---
 
### <a name="new-user-provisioning-saas-app-integrations---july-2018"></a>Nuevas integraciones de aplicaciones de SaaS de aprovisionamiento de usuarios: julio de 2018

**Tipo:** nueva característica  
**Categoría del servicio:** aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** integración de terceros
 
Azure AD permite automatizar la creación, el mantenimiento y la eliminación de identidades de usuario en aplicaciones de SaaS, como Dropbox, Salesforce, ServiceNow, etc. En julio de 2018, hemos agregado compatibilidad con el aprovisionamiento de usuarios a las siguientes aplicaciones de la Galería de aplicaciones de Azure AD:

- [Cisco Spark](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-spark-provisioning-tutorial)

- [Cisco WebEx](https://docs.microsoft.com/azure/active-directory/saas-apps/cisco-webex-provisioning-tutorial)

- [Bonusly](https://docs.microsoft.com/azure/active-directory/saas-apps/bonusly-provisioning-tutorial)

Para una lista de todas las aplicaciones que admiten el aprovisionamiento de usuarios en la Galería de Azure AD, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial).

---

### <a name="connect-health-for-sync---an-easier-way-to-fix-orphaned-and-duplicate-attribute-sync-errors"></a>Connect Health para sincronización: una manera más sencilla de corregir errores de sincronización de atributos duplicados o huérfanas

**Tipo:** nueva característica  
**Categoría del servicio:** AD Connect  
**Funcionalidad del producto:** supervisión e informes
 
Azure AD Connect Health presenta la corrección de autoservicio para ayudarle a resaltar y corregir errores de sincronización. Esta característica soluciona los errores de sincronización de atributos duplicados y repara los objetos huérfanos desde Azure AD. Este diagnóstico tiene las siguientes ventajas:

- Reduce los errores de sincronización de atributos duplicado, proporcionando correcciones específicas

- Aplica una corrección para escenarios de Azure AD dedicados, resolviendo los errores en un solo paso dedicado

- No es necesaria ninguna actualización o configuración para activar y usar esta característica

Para más información, consulte [Diagnóstico y solución de errores de sincronización de atributos duplicados](https://docs.microsoft.com/azure/active-directory/connect-health/active-directory-aadconnect-health-diagnose-sync-errors).

---

### <a name="visual-updates-to-the-azure-ad-and-msa-sign-in-experiences"></a>Actualizaciones de objetos visuales en Azure AD y experiencias de inicio de sesión de MSA

**Tipo:** característica modificada  
**Categoría del servicio:** Azure AD  
**Funcionalidad del producto:** autenticación de usuarios

Hemos actualizado la interfaz de usuario de la experiencia de inicio de sesión de los servicios en línea de Microsoft, como Office 365 y Azure. Este cambio hace que las pantallas estén menos saturadas y sean más sencillas. Para más información sobre este cambio, consulte el blog [Upcoming improvements to the Azure AD sign-in experience](https://cloudblogs.microsoft.com/enterprisemobility/2018/04/04/upcoming-improvements-to-the-azure-ad-sign-in-experience/) (Próximas mejoras en la experiencia de inicio de sesión de Azure AD).

---

### <a name="new-release-of-azure-ad-connect---july-2018"></a>Nueva versión de Azure AD Connect: julio de 2018

**Tipo:** característica modificada  
**Categoría del servicio:** aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** administración del ciclo de vida de las identidades

La versión más reciente de Azure AD Connect incluye: 

- Correcciones de errores y actualizaciones de compatibilidad 

- Disponibilidad general de la integración PingFederate

- Actualizaciones en el cliente más reciente de SQL 2012 

Para más información sobre esta actualización, consulte [Azure AD Connect: historial de versiones](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history)

---

### <a name="updates-to-the-terms-of-use-tou-end-user-ui"></a>Actualizaciones en la interfaz de usuario del usuario final correspondiente a las condiciones de uso (CDU)

**Tipo:** característica modificada  
**Categoría del servicio:** términos de uso  
**Funcionalidad del producto:** gobierno

Estamos actualizando la cadena de aceptación en la interfaz de usuario del usuario final correspondiente a las condiciones de uso.

**Texto actual.** Para acceder a los recursos de [nombreInquilino], debe aceptar las condiciones de uso.<br>**Nuevo texto.** Para acceder al recurso de [nombreInquilino], debe leer las condiciones de uso.

**Texto actual:** si elige Aceptar, significa que acepta todas las condiciones de uso anteriores.<br>**Nuevo texto:** haga clic en Aceptar para confirmar que ha leído y comprendido las condiciones de uso.

---
 
### <a name="pass-through-authentication-supports-legacy-protocols-and-applications"></a>La autenticación de paso a través es compatible con las aplicaciones y protocolos heredados

**Tipo:** característica modificada  
**Categoría del servicio:** autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** autenticación de usuarios
 
La autenticación de paso a través es compatible con las aplicaciones y los protocolos heredados. Ahora se admiten completamente las siguientes limitaciones:

- Inicios de sesión de usuario en las aplicaciones cliente de Office heredadas, Office 2010 y Office 2013, sin necesidad de autenticación moderna.

- Acceso a uso compartido del calendario e información de disponibilidad en entornos híbridos de Exchange solo en Office 2010.

- Inicios de sesión de usuarios en aplicaciones cliente de Skype Empresarial sin necesidad de autenticación moderna.

- Inicios de sesión de usuario en PowerShell 1.0.

- El Programa de inscripción de dispositivos de Apple (DEP de Apple), mediante el Asistente para configuración de iOS. 

---
 
### <a name="converged-security-info-management-for-self-service-password-reset-and-multi-factor-authentication"></a>Administración de información de seguridad convergida para restablecimiento de contraseña de autoservicio y Multi-Factor Authentication

**Tipo:** nueva característica  
**Categoría del servicio:** SSPR  
**Funcionalidad del producto:** autenticación de usuarios

Esta nueva característica permite a los usuarios administrar su información de seguridad (por ejemplo, número de teléfono, dirección de correo electrónico, aplicación móvil, etc.) para el restablecimiento de contraseña de autoservicio (SSPR) y Multi-Factor Authentication (MFA) en una sola experiencia. Los usuarios ya no tendrán que registrar la misma información de seguridad para SSPR y MFA en dos experiencias diferentes. Esta nueva experiencia también se aplica a los usuarios que tienen SSPR o MFA.

Si una organización no aplica el registro MFA o SSPR, los usuarios pueden registrar su información de seguridad a través del portal **Mis aplicaciones**. Desde allí, los usuarios pueden registrar los métodos habilitados para MFA o SSPR. 

Se trata de una versión preliminar pública opcional. Los administradores pueden activar la nueva experiencia (si lo desean) para un grupo de usuarios seleccionado o para todos los usuarios en un inquilino.

---
 
### <a name="use-the-microsoft-authenticator-app-to-verify-your-identity-when-you-reset-your-password"></a>Uso de la aplicación Microsoft Authenticator para verificar su identidad cuando restablezca la contraseña

**Tipo:** característica modificada  
**Categoría del servicio:** SSPR  
**Funcionalidad del producto:** autenticación de usuarios

Esta característica permite a los no administradores verificar su identidad al restablecer una contraseña mediante una notificación o un código de Microsoft Authenticator (o cualquier otra aplicación de autenticador). Una vez que los administradores activan este método de restablecimiento de contraseña de autoservicio, los usuarios que hayan registrado una aplicación móvil a través de aka.ms/mfasetup o aka.ms/setupsecurityinfo pueden usar su aplicación móvil como método de verificación al restablecer su contraseña.

La notificación de la aplicación móvil solo se puede activar como parte de una directiva que requiere dos métodos para restablecer la contraseña.

---

## <a name="june-2018"></a>Junio de 2018

### <a name="change-notice-security-fix-to-the-delegated-authorization-flow-for-apps-using-azure-ad-activity-logs-api"></a>Aviso de cambio: revisión de seguridad para el flujo de autorización delegado para aplicaciones que usan la API de registro de actividad de Azure AD

**Tipo:** plan de cambio  
**Categoría del servicio:** informes  
**Funcionalidad del producto:** supervisión e informes

Debido a nuestras medidas de seguridad más estrictas, hemos tenido que realizar un cambio en los permisos de las aplicaciones que usan un flujo de autorización delegado para acceder a la [API de registro de actividad de Azure AD](https://aka.ms/aadreportsapi). Este cambio se producirá el **26 de junio de 2018**.

Si alguna de sus aplicaciones usa la API de registro de actividad de Azure AD, siga estos pasos para asegurarse de que la aplicación no se interrumpa después de que se produzca el cambio.

**Para actualizar los permisos de la aplicación**

1. Inicie sesión en Azure Portal, seleccione **Azure Active Directory** y, a continuación, **Registros de aplicaciones**.
2. Seleccione la aplicación que usa la API de registro de actividad de Azure AD, seleccione **Configuración**, **Permisos necesarios** y, a continuación, seleccione la API de **Microsoft Azure Active Directory**.
3. En el área **Permisos delegados** de la hoja **Habilitar acceso**, active la casilla junto a **Leer datos de directorio** y seleccione **Guardar**.
4. Haga clic en **Conceder permisos** y, a continuación, haga clic en **Sí**.
    
    >[!Note]
    >Debe ser un administrador global para conceder permisos a la aplicación.

Para obtener más información, consulte el área [Conceder permisos](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-prerequisites-azure-portal#grant-permissions) de los requisitos previos para obtener acceso al artículo de la API de generación de informes de Azure AD.

---

### <a name="configure-tls-settings-to-connect-to-azure-ad-services-for-pci-dss-compliance"></a>Configuración de las opciones de TLS para conectarse a servicios de Azure AD para el cumplimiento de PCI DSS

**Tipo:** nueva característica  
**Categoría del servicio:** N/D  
**Funcionalidad del producto:** plataforma

La Seguridad de la capa de transporte (TLS) es un protocolo que proporciona privacidad e integridad de datos para la comunicación entre dos aplicaciones, y es el protocolo de seguridad más implementado hoy en día.

El [PCI Security Standards Council](https://www.pcisecuritystandards.org/) (Consejo de estándares de seguridad de PCI) ha determinado que las versiones anteriores de TLS y Capa de sockets seguros (SSL) deben deshabilitarse para habilitar protocolos de aplicaciones nuevos y más seguros, con el inicio del cumplimiento a partir del **30 de junio 2018**. Este cambio significa que, si se conecta a servicios de Azure AD y es necesario el cumplimiento con PCI DSS, deberá deshabilitar TLS 1.0. Hay varias versiones de TLS disponibles, pero TLS 1.2 es la más reciente para servicios de Azure Active Directory. Se recomienda encarecidamente moverse directamente a TLS 1.2 para las combinaciones de cliente/servidor y explorador/servidor.

Es posible que los exploradores obsoletos no admitan las versiones más recientes de TLS, como TLS 1.2. Para ver qué versiones de TLS son compatibles con el explorador, vaya al sitio de [Qualys SSL Labs](https://www.ssllabs.com/) y haga clic en **Test your browser** (Probar el explorador). Le recomendamos que actualice a la versión más reciente del explorador web y, si es posible,que habilite solo TLS 1.2.

**Para habilitar TLS 1.2 por explorador**

- **Microsoft Edge e Internet Explorer (ambos se establecen mediante Internet Explorer)**

    1. Abra Internet Explorer, seleccione **Herramientas** > **Opciones de Internet** > **Opciones avanzadas**.
    2. En el área de **Seguridad**, seleccione **use TLS 1.2** (usar TLS 1.2) y, a continuación, seleccione **Aceptar**.
    3. Cierre todas las ventanas del explorador y reinicie Internet Explorer. 

- **Google Chrome**

    1. Abra Google Chrome, escriba *chrome://settings/* en la barra de direcciones y presione **ENTRAR**.
    2. Expanda la **Configuración avanzada**, vaya al área de **Sistema** y seleccione **Abrir la configuración de proxy**.
    3. En el cuadro **Propiedades: Internet**, seleccione la pestaña **Opciones avanzadas**, vaya al área de **Seguridad**, seleccione **Usar TLS 1.2** y, a continuación, seleccione  **Aceptar**.
    4. Cierre todas las ventanas del explorador y reinicie Google Chrome.

- **Mozilla Firefox**

    1. Abra Firefox, escriba *about:config* en la barra de direcciones y, a continuación, presione **ENTRAR**.
    2. Busque el término *TLS* y, a continuación, seleccione la entrada **security.tls.version.max**.
    3. Establezca el valor en **3** para forzar al explorador a usar hasta la versión TLS 1.2 y, a continuación, seleccione **Aceptar**.

        >[!NOTE]
        >La versión de Firefox 60.0 admite TLS 1.3, por lo que también puede establecer el valor de security.tls.version.max en **4**.

    4. Cierre todas las ventanas del explorador y reinicie Mozilla Firefox.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2018"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD: junio de 2018

**Tipo:** nueva característica  
**Categoría del servicio:** aplicaciones empresariales  
**Funcionalidad del producto:** integración de terceros
 
En junio de 2018, hemos agregado estas 15 nuevas aplicaciones con compatibilidad con la federación para la galería de aplicaciones:

[Skytap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skytap-tutorial), [Settling music](https://docs.microsoft.com/azure/active-directory/active-directory-saas-settlingmusic-tutorial), [aplicación de LOB con el token SAML 1.1 habilitado](https://docs.microsoft.com/azure/active-directory/active-directory-saas-saml-tutorial), [Supermood](https://docs.microsoft.com/azure/active-directory/active-directory-saas-supermood-tutorial), [Autotask](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [Endpoint Backup](https://docs.microsoft.com/azure/active-directory/active-directory-saas-autotaskendpointbackup-tutorial), [Skyhigh Networks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-skyhighnetworks-tutorial), Smartway2, [TonicDM](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tonicdm-tutorial), [Moconavi](https://docs.microsoft.com/azure/active-directory/active-directory-saas-moconavi-tutorial), [Zoho One](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zohoone-tutorial), [SharePoint local](https://docs.microsoft.com/azure/active-directory/active-directory-saas-sharepoint-on-premises-tutorial), [ForeSee CX Suite](https://docs.microsoft.com/azure/active-directory/active-directory-saas-foreseecxsuite-tutorial), [Vidyard](https://docs.microsoft.com/azure/active-directory/active-directory-saas-vidyard-tutorial) y [ChronicX](https://docs.microsoft.com/azure/active-directory/active-directory-saas-chronicx-tutorial)

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial). Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---

### <a name="azure-ad-password-protection-is-available-in-public-preview"></a>La Protección con contraseña de Azure AD está disponible en versión preliminar pública.

**Tipo:** nueva característica  
**Categoría del servicio:** protección de la identidad  
**Funcionalidad del producto:** autenticación de usuarios

Use la Protección con contraseña de Azure AD para ayudar a eliminar de su entorno las contraseñas que se pueden averiguar fácilmente. Al eliminar estas contraseñas, ayuda a reducir el riesgo de un tipo de ataque de difusión de contraseña.

En concreto, la Protección con contraseña de Azure AD:

- Le ayuda a proteger las cuentas de la organización en Azure AD y Windows Server Active Directory (AD). 
- Impide que los usuarios usen las contraseñas de una lista que incluye más de 500 de las contraseñas utilizadas con más frecuencia y más de un millón de variaciones de sustitución de caracteres de esas contraseñas.
- Le ayuda a administrar la Protección con contraseña de Azure AD desde una única ubicación en el portal de Azure AD, tanto para Azure AD como para Windows Server AD local.

Para obtener más información acerca de la Protección con contraseña de Azure AD, consulte [Elimine las contraseñas incorrectas de su organización](https://aka.ms/aadpasswordprotectiondocs).

---

### <a name="new-all-guests-conditional-access-policy-template-created-during-terms-of-use-tou-creation"></a>Nueva plantilla "todos los invitados" de la directiva de acceso condicional creada durante la creación de los Términos de uso (TOU)

**Tipo:** nueva característica  
**Categoría del servicio:** términos de uso  
**Funcionalidad del producto:** gobierno

Durante la creación de los Términos de uso (TOU), también se ha creado una nueva plantilla de la directiva de acceso condicional para "todos los invitados" y "todas las aplicaciones". Esta nueva plantilla de la directiva se aplica a los Términos de uso recién creados, lo que facilita el proceso de cumplimiento y creación para los invitados.

Para obtener más información, consulte [Característica Azure Active Directory Terms of Use](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---

### <a name="new-custom-conditional-access-policy-template-created-during-terms-of-use-tou-creation"></a>Nueva plantilla "personalizado" de la directiva de acceso condicional creada durante la creación de los Términos de uso (TOU)

**Tipo:** nueva característica  
**Categoría del servicio:** términos de uso  
**Funcionalidad del producto:** gobierno

Durante la creación de los términos de uso (TOU), también se ha creado una nueva plantilla "personalizado" de la directiva de acceso condicional. Esta nueva plantilla de la directiva le permite crear los términos de uso y, a continuación, pasar inmediatamente a la hoja de creación de la directiva de acceso condicional, sin necesidad de navegar de forma manual a través del portal.

Para obtener más información, consulte [Característica Azure Active Directory Terms of Use](https://docs.microsoft.com/azure/active-directory/active-directory-tou).

---

### <a name="new-and-comprehensive-guidance-about-deploying-azure-multi-factor-authentication"></a>Guía nueva y completa acerca de cómo implementar Azure Multi-Factor Authentication

**Tipo:** nueva característica  
**Categoría del servicio:** otros  
**Funcionalidad del producto:** protección y seguridad de la identidad
 
Hemos lanzado la nueva guía paso a paso acerca de cómo implementar Azure Multi-Factor Authentication (MFA) en su organización.

Para ver la guía de implementación de MFA, vaya al repositorio [Guías de implementación de identidad](https://aka.ms/DeploymentPlans) en GitHub. Para enviar comentarios acerca de las guías de implementación, use el [formulario Comentarios del plan de implementación](https://aka.ms/deploymentplanfeedback). Si tiene alguna pregunta acerca de las guías de implementación, póngase en contacto con nosotros en [IDGitDeploy](mailto:idgitdeploy@microsoft.com).

---

### <a name="azure-ad-delegated-app-management-roles-are-in-public-preview"></a>Los roles de administración de aplicaciones delegadas de Azure AD están en versión preliminar pública.

**Tipo:** nueva característica  
**Categoría del servicio:** aplicaciones empresariales  
**Funcionalidad del producto:** control de acceso

Ahora los administradores pueden delegar tareas de administración de aplicaciones sin asignar el rol de administrador global. Los nuevos roles y funcionalidades son:

- **Nuevos roles de administrador de Azure AD estándar:**

    - **Administrador de aplicaciones** Concede la capacidad de administrar todos los aspectos de todas las aplicaciones, incluidos el registro, la configuración de SSO, las asignaciones de aplicaciones y licencias, la configuración del proxy de aplicación y el consentimiento (excepto para los recursos de Azure AD).

    - **Administrador de aplicaciones en la nube** Concede todas las capacidades de administrador de aplicaciones, excepto para el proxy de aplicación porque no proporciona acceso local.

    - **Desarrollador de aplicaciones** Concede permisos para crear registros de aplicaciones, incluso si está desactivada la opción **allow users to register apps** (permitir a los usuarios registrar aplicaciones).

- **Propiedad (configurar el registro por aplicación y la aplicación por empresa, similar al proceso de la propiedad de grupo:**
 
    - **Propietario de registro de aplicaciones** Concede la capacidad de administrar todos los aspectos del registro de aplicaciones en propiedad, incluidos el manifiesto de la aplicación y la adición de más propietarios.

    - **Propietario de aplicaciones empresariales** Concede la capacidad de administrar muchos aspectos de las aplicaciones empresariales en propiedad, incluidos el consentimiento, la configuración de SSO y las asignaciones de aplicaciones (excepto para los recursos de Azure AD).

Para obtener más información acerca de la versión preliminar pública, consulte el blog [Azure AD delegated application management roles are in public preview!](https://cloudblogs.microsoft.com/enterprisemobility/2018/06/13/hallelujah-azure-ad-delegated-application-management-roles-are-in-public-preview/) (Los roles de administración de aplicaciones delegadas de Azure AD están en versión preliminar pública). . Para obtener más información acerca de los roles y permisos, consulte [Assigning administrator roles in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) (Asignación de roles de administrador en Azure Active Directory).

---

## <a name="may-2018"></a>Mayo de 2018

### <a name="expressroute-support-changes"></a>Cambios de soporte técnico de ExpressRoute

**Tipo:** plan de cambio  
**Categoría del servicio:** autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** plataforma  

Las ofertas de software como servicio, como Azure Active Directory (Azure AD) están diseñadas para funcionar mejor a través de Internet, sin necesidad de ExpressRoute ni otros túneles VPN privados. Por este motivo, en **1 de agosto de 2018**, dejaremos de dar soporte técnico a ExpressRoute para los servicios de Azure AD que usen un emparejamiento público de Azure y a las comunidades de Azure que usen emparejamientos de Microsoft. Los servicios afectados por este cambio podrían observar que el tráfico de Azure AD gradualmente cambia de ExpressRoute a Internet.

Aunque vamos a cambiar el soporte, también sabemos que aún hay situaciones en las que tendrá que utilizar un conjunto de circuitos dedicado para el tráfico de autenticación. Por este motivo, Azure AD seguirá admitiendo restricciones de intervalos de IP por inquilino utilizando ExpressRoute y los servicios que ya tienen emparejamiento de Microsoft con la comunidad "Otros servicios en línea de Office 365". Si los servicios se ven afectados pero requieren ExpressRoute, debe hacer lo siguiente:

- **Si está en un emparejamiento público de Azure.** Cambie a un emparejamiento de Microsoft y suscríbase a la comunidad **Otros servicios en línea de Office 365 (12076:5100)**. Para más información sobre cómo cambiar de emparejamiento público de Azure a emparejamiento de Microsoft, consulte el artículo [Cambiar un emparejamiento público a emparejamiento de Microsoft](https://docs.microsoft.com/azure/expressroute/how-to-move-peering).

- **Si está en un emparejamiento de Microsoft.** Suscríbase a la comunidad **Otros servicio en línea de Office 365 (12076:5100)**. Para más información sobre los requisitos de enrutamiento, consulte la sección [Soporte técnico para las comunidades de BGP](https://docs.microsoft.com/azure/expressroute/expressroute-routing#bgp) en el artículo de requisitos de enrutamiento de ExpressRoute.

Si debe continuar usando circuitos dedicados, deberá hablar con el equipo de su cuenta Microsoft acerca de cómo obtener autorización para utilizar la comunidad **Otros servicios en línea de Office 365 (12076:5100)**. El comité de revisión administrado por MS Office comprobará si necesita los circuitos y se asegurará de que comprende las implicaciones técnicas de su conservación. Las suscripciones no autorizadas que intenten crear filtros de ruta para Office 365 recibirán un mensaje de error. 
 
---

### <a name="microsoft-graph-apis-for-administrative-scenarios-for-tou"></a>Microsoft Graph API para escenarios administrativos de términos de uso

**Tipo:** nueva característica  
**Categoría del servicio:** términos de uso  
**Funcionalidad del producto:** experiencia del desarrollador
 
Hemos agregado Microsoft Graph API para el funcionamiento de la administración de los términos de uso de Azure AD. Puede crear, actualizar y eliminar el objeto Términos de uso.

---

### <a name="add-azure-ad-multi-tenant-endpoint-as-an-identity-provider-in-azure-ad-b2c"></a>Agregar punto de conexión multiinquilino de Azure AD como proveedor de identidades en Azure AD B2C

**Tipo:** nueva característica  
**Categoría del servicio:** B2C - administración de identidades de consumidores  
**Funcionalidad del producto:** B2B/B2C
 
Con las directivas personalizadas, ahora puede agregar el punto de conexión común de Azure AD como proveedor de identidades en Azure AD B2C. Esto le permite tener un único punto de entrada para todos los usuarios de Azure AD que inician sesión en sus aplicaciones. Para más información, consulte [Azure Active Directory B2C permite a los usuarios iniciar sesión en un proveedor de identidades multiinquilino de Azure AD mediante directivas personalizadas](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-setup-commonaad-custom).

---

### <a name="use-internal-urls-to-access-apps-from-anywhere-with-our-my-apps-sign-in-extension-and-the-azure-ad-application-proxy"></a>Uso de direcciones URL internas para acceder a aplicaciones desde cualquier lugar con My Apps Sign-in Extension y Azure AD Application Proxy

**Tipo:** nueva característica  
**Categoría del servicio:** Mis aplicaciones  
**Funcionalidad del producto:** SSO
 
Los usuarios ya pueden acceder a las aplicaciones a través de direcciones URL internas, incluso desde fuera de la red corporativa, mediante My Apps Sign-in Extension para Azure AD. Esto funcionará con todas las aplicaciones que se ha publicado con Azure Active Directory Application Proxy, en cualquier explorador que también tenga instalada la extensión de explorador del Panel de acceso. La funcionalidad de redireccionamiento de direcciones URL se habilita automáticamente una vez que un usuario inicia sesión en la extensión. La extensión se puede descargar en [Edge](https://go.microsoft.com/fwlink/?linkid=845176), [Chrome](https://go.microsoft.com/fwlink/?linkid=866367) y [Firefox](https://go.microsoft.com/fwlink/?linkid=866366).

---
 
### <a name="azure-active-directory---data-in-europe-for-europe-customers"></a>Azure Active Directory: datos en Europa para los clientes europeos

**Tipo:** nueva característica  
**Categoría del servicio:** otros  
**Funcionalidad del producto:** GoLocal

Los clientes de Europa requieren que sus datos se queden en Europa y no se repliquen fuera de los centros de datos europeos para proteger la privacidad y cumplir la legislación europea. En este [artículo](https://go.microsoft.com/fwlink/?linkid=872328) se proporcionan detalles concretos no solo acerca de qué información de identidad se almacenará en Europa, sino también acerca de la que se almacenará fuera de los centros de datos europeos. 

---
 
### <a name="new-user-provisioning-saas-app-integrations---may-2018"></a>Nuevas integraciones de aplicaciones de SaaS de aprovisionamiento de usuarios: mayo de 2018

**Tipo:** nueva característica  
**Categoría del servicio:** aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** integración de terceros
 
Azure AD permite automatizar la creación, el mantenimiento y la eliminación de identidades de usuario en aplicaciones de SaaS, como Dropbox, Salesforce, ServiceNow, etc. En mayo de 2018, hemos agregado compatibilidad con el aprovisionamiento de usuarios a las siguientes aplicaciones de la Galería de aplicaciones de Azure AD:

- [BlueJeans](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bluejeans-provisioning-tutorial)

- [Cornerstone OnDemand](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cornerstone-ondemand-provisioning-tutorial)

- [Zendesk](https://docs.microsoft.com/azure/active-directory/active-directory-saas-zendesk-provisioning-tutorial)

Para obtener una lista de todas las aplicaciones que admiten el aprovisionamiento de usuarios en la Galería de Azure AD, consulte [https://aka.ms/appstutorial](https://aka.ms/appstutorial).

---
 
### <a name="azure-ad-access-reviews-of-groups-and-app-access-now-provides-recurring-reviews"></a>Ahora, las revisiones de acceso de Azure AD de grupos y aplicaciones proporcionan revisiones periódicas

**Tipo:** nueva característica  
**Categoría del servicio:** revisiones de acceso  
**Funcionalidad del producto:** gobierno
 
La revisión de acceso de grupos y aplicaciones ya está disponible de forma generalizada como parte de Azure AD Premium P2.  Los administradores podrán configurar las revisiones de acceso de los miembros de un grupo y las asignaciones de aplicaciones para que se repitan automáticamente a intervalos periódicos; por ejemplo, cada mes o cada trimestre.

---

### <a name="azure-ad-activity-logs-sign-ins-and-audit-are-now-available-through-ms-graph"></a>Los registros de actividad de Azure AD (inicios de sesión y auditoría) ahora están disponibles a través de MS Graph

**Tipo:** nueva característica  
**Categoría del servicio:** informes  
**Funcionalidad del producto:** supervisión e informes
 
Los registros de actividad de Azure AD, que incluyen los registros inicios de sesión y de auditorías, ahora están disponibles a través de MS Graph. Para acceder a dichos registros se han expuesto dos puntos de conexión a través de MS Graph. Consulte nuestros [documentos](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal) para ver cómo acceder mediante programación a las API de informes de Azure AD. 

---
 
### <a name="improvements-to-the-b2b-redemption-experience-and-leave-an-org"></a>Mejoras en la experiencia de canje de invitaciones B2B y en el abandono de una organización

**Tipo:** nueva característica  
**Categoría del servicio:** B2B  
**Funcionalidad del producto:** B2B/B2C

**Canje justo a tiempo:** una vez que se comparte un recurso con un usuario invitado mediante la API B2B, no es preciso enviar un correo electrónico de invitación especial. En la mayoría de los casos, el usuario invitado puede acceder al recurso y pasar por la experiencia de canje justo a tiempo. Se acabaron los problemas por mensajes de correo electrónico perdidos. Se acabo preguntar a los usuarios invitados: "¿Ha hecho clic en el vínculo de canje que le envió el sistema?". Esto significa que una vez que la SPO utiliza el administrador de invitaciones, los elementos adjuntos de la nube pueden tener la misma dirección URL canónica para todos los usuarios, tanto internos como externos, con cualquier estado de canje.

**Experiencia de canje moderna:** se acabaron las páginas de aterrizaje para el canje con pantalla dividida. Los usuarios verán un consentimiento moderno con la declaración de privacidad de la organización que realiza la invitación, igual que en las aplicaciones de terceros.

**Los usuarios invitados pueden abandonar la organización:** una vez que termina la relación de un usuario con una organización, este puede salir de ella sin ayuda de nadie. No será preciso pedir al administrador de la organización que le invitó que lo "elimine" ni necesitará generar incidencias de soporte técnico.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2018"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD: mayo de 2018

**Tipo:** nueva característica  
**Categoría del servicio:** aplicaciones empresariales  
**Funcionalidad del producto:** integración de terceros
 
En mayo de 2018, hemos agregado estas 18 nuevas aplicaciones con compatibilidad con la federación a nuestra galería de aplicaciones:

[AwardSpring](https://docs.microsoft.com/azure/active-directory/active-directory-saas-awardspring-tutorial), Infogix Data3Sixty Govern, [Yodeck](https://docs.microsoft.com/azure/active-directory/active-directory-saas-infogix-tutorial), [Jamf Pro](https://docs.microsoft.com/azure/active-directory/active-directory-saas-jamfprosamlconnector-tutorial), [KnowledgeOwl](https://docs.microsoft.com/azure/active-directory/active-directory-saas-knowledgeowl-tutorial), [Envi MMIS](https://docs.microsoft.com/azure/active-directory/active-directory-saas-envimmis-tutorial), [LaunchDarkly](https://docs.microsoft.com/azure/active-directory/active-directory-saas-launchdarkly-tutorial), [Adobe Captivate Prime](https://docs.microsoft.com/azure/active-directory/active-directory-saas-adobecaptivateprime-tutorial), [Montage Online](https://docs.microsoft.com/azure/active-directory/active-directory-saas-montageonline-tutorial), [まなびポケット](https://docs.microsoft.com/azure/active-directory/active-directory-saas-manabipocket-tutorial), OpenReel, [Arc Publishing - SSO](https://docs.microsoft.com/azure/active-directory/active-directory-saas-arc-tutorial), [PlanGrid](https://docs.microsoft.com/azure/active-directory/active-directory-saas-plangrid-tutorial), [iWellnessNow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-iwellnessnow-tutorial), [Proxyclick](https://docs.microsoft.com/azure/active-directory/active-directory-saas-proxyclick-tutorial), [Riskware](https://docs.microsoft.com/azure/active-directory/active-directory-saas-riskware-tutorial), [Flock](https://docs.microsoft.com/azure/active-directory/active-directory-saas-flock-tutorial), [Reviewsnap](https://docs.microsoft.com/azure/active-directory/active-directory-saas-reviewsnap-tutorial)

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial).

Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

---
 
### <a name="new-step-by-step-deployment-guides-for-azure-active-directory"></a>Nuevas guías de implementación paso a paso para Azure Active Directory

**Tipo:** nueva característica  
**Categoría del servicio:** otros  
**Funcionalidad del producto:** directorio
 
Ahora, la guía paso a paso sobre cómo implementar Azure Active Directory (Azure AD), incluido el restablecimiento de contraseña de autoservicio (SSPR), el inicio de sesión único (SSO), el acceso condicional (CA), el proxy de aplicación, el aprovisionamiento de usuarios y los Servicios de federación de Active Directory (AD FS) en Autenticación de paso a través (PTA) y AD FS para realizar la sincronización de hash de contraseña (PBS).

Para ver las guías de implementación, vaya al repositorio [Guías de implementación de identidad](https://aka.ms/DeploymentPlans) en GitHub. Para enviar comentarios acerca de las guías de implementación, use el [formulario Comentarios del plan de implementación](https://aka.ms/deploymentplanfeedback). Si tiene alguna pregunta acerca de las guías de implementación, póngase en contacto con nosotros en [IDGitDeploy](mailto:idgitdeploy@microsoft.com).

---

### <a name="enterprise-applications-search---load-more-apps"></a>Búsqueda de aplicaciones empresariales: cargar más aplicaciones

**Tipo:** nueva característica  
**Categoría del servicio:** aplicaciones empresariales  
**Funcionalidad del producto:** SSO
 
¿No consigue encontrar aplicaciones o entidades de servicio? Hemos agregado la posibilidad de cargar más aplicaciones en la lista de todas las aplicaciones empresariales. De manera predeterminada, se muestran 20 aplicaciones. Ahora puede hacer clic en **Cargar más** y ver otras aplicaciones. 

---
 
### <a name="the-may-release-of-aadconnect-contains-a-public-preview-of-the-integration-with-pingfederate-important-security-updates-many-bug-fixes-and-new-great-new-troubleshooting-tools"></a>La versión de mayo de AADConnect contiene una versión preliminar pública de la integración con PingFederate, actualizaciones de seguridad importantes, muchas correcciones de errores y nuevas y magníficas herramientas para la solución de problemas. 

**Tipo:** característica modificada  
**Categoría del servicio:** AD Connect  
**Funcionalidad del producto:** administración del ciclo de vida de las identidades
 
La versión de mayo de AADConnect contiene una versión preliminar pública de la integración con PingFederate, actualizaciones de seguridad importantes, muchas correcciones de errores y nuevas y magníficas herramientas para la solución de problemas. Puede encontrar las notas de la versión [aquí](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-version-history#118190).

---

### <a name="azure-ad-access-reviews-auto-apply"></a>Revisiones de acceso de Azure AD: aplicación automática

**Tipo:** característica modificada  
**Categoría del servicio:** revisiones de acceso  
**Funcionalidad del producto:** gobierno

Las revisiones de acceso de grupos y aplicaciones ahora están disponibles de forma generalizada como parte de Azure AD Premium P2. Un administrador puede realizar la configuración necesaria para aplicar automáticamente los cambios del revisor a un grupo o aplicación cuando se complete la revisión de acceso. El administrador también puede especificar lo que ocurre al acceso continuado del usuario si los revisores no responden: quitar el acceso, mantener el acceso o seguir las recomendaciones del sistema. 

---

### <a name="id-tokens-can-no-longer-be-returned-using-the-query-responsemode-for-new-apps"></a>Los tokens de identificador no se pueden devolver mediante el valor de response_mode de una consulta en las nuevas aplicaciones. 

**Tipo:** característica modificada  
**Categoría del servicio:** autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** autenticación de usuarios
 
Las aplicaciones creadas a partir del 25 de abril de 2018 ya no podrán solicitar un valor de **id_token** mediante el argumento response_mode de **query**.  De esta forma, Azure AD se alinea con las especificaciones de OIDC y se reduce la superficie de ataque de sus aplicaciones.  No se impide que las aplicaciones creadas antes del 25 de abril de 2018 utilicen el argumento response_mode de **query** con un valor de response_type de **id_token**.  El error que se devuelve al solicitar un id_token de AAD es **AADSTS70007: "query" no es un valor de "response_mode" que se admita al solicitar un token**.

Los valores de response_mode **fragment** y **form_post** siguen funcionando (al crear nuevos objetos de aplicación [por ejemplo, para el uso de proxy de aplicación]). Asegúrese de usar uno de estos valores de response_mode antes de que creen una aplicación nueva.  
 
---
 
## <a name="april-2018"></a>2018 de abril de 2018 

### <a name="azure-ad-b2c-access-token-are-ga"></a>Los tokens de acceso de Azure AD B2C son GA

**Tipo:** nueva característica  
**Categoría del servicio:** B2C - administración de identidades de consumidores  
**Funcionalidad del producto:** B2B/B2C 

Ya puede acceder a las API web que protege Azure AD B2C gracias a los tokens de acceso. Esta característica está pasando de la versión preliminar pública a la versión de disponibilidad general. A parte de otras mejoras menores, también se ha mejorado la experiencia de interfaz de usuario para configurar las aplicaciones de Azure AD B2C y las API web.
 
Para obtener más información, consulte [Azure AD B2C: Requesting access tokens](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-access-tokens) (Azure AD B2C: solicitar tokens de acceso).

---

### <a name="test-single-sign-on-configuration-for-saml-based-applications"></a>Probar la configuración del inicio de sesión único para aplicaciones basadas en SAML

**Tipo:** nueva característica  
**Categoría del servicio:** aplicaciones empresariales  
**Funcionalidad del producto:** SSO

Al configurar las aplicaciones SSO basadas en SAML, puede probar la integración en la página de configuración. Si se produce un error durante el inicio de sesión, puede proporcionar dicho error en la experiencia de prueba y Azure AD le proporcionará los pasos para resolver ese problema.

Para más información, consulte:

- [Configuración del inicio de sesión único en aplicaciones que no están en la Galería de aplicaciones de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps)
- [Cómo depurar el inicio de sesión único basado en SAML en aplicaciones de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

---
 
### <a name="azure-ad-terms-of-use-now-has-per-user-reporting"></a>Los Términos de uso de AD Azure ahora tienen informes por usuario

**Tipo:** nueva característica  
**Categoría del servicio:** términos de uso  
**Funcionalidad del producto:** cumplimiento
 
Los administradores pueden seleccionar una versión determinada de los Términos de uso y ver qué usuarios han dado su consentimiento y en qué fecha y hora.

Para obtener más información, consulte [Azure AD terms of use feature](https://docs.microsoft.com/azure/active-directory/active-directory-tou) (Característica de términos de uso de Azure AD).

---
 
### <a name="azure-ad-connect-health-risky-ip-for-ad-fs-extranet-lockout-protection"></a>Azure AD Connect Health: protección de bloqueo de extranet de direcciones IP de riesgo para AD FS 

**Tipo:** nueva característica  
**Categoría del servicio:** otros  
**Funcionalidad del producto:** supervisión e informes

Connect Health ahora tiene la capacidad de detectar direcciones IP que superan un umbral de inicios de sesión de U/P erróneos por hora o por día. Estas son las funcionalidades que proporciona esta característica:

- Un informe completo que muestra la dirección IP y el número de inicios de sesión erróneos generados por hora o por día y con un umbral personalizable.
- Alertas basadas en el correo electrónico que muestran el momento en que una dirección IP específica excede el umbral de inicios de sesión de U/P erróneos por hora o por día.
- Una opción de descarga para realizar un análisis detallado de los datos.

Para obtener más información, consulte [Informe de direcciones IP de riesgo](https://aka.ms/aadchriskyip).

---
 
### <a name="easy-app-config-with-metadata-file-or-url"></a>Configuración sencilla de aplicaciones mediante una dirección URL o un archivo de metadatos

**Tipo:** nueva característica  
**Categoría del servicio:** aplicaciones empresariales  
**Funcionalidad del producto:** SSO

En la página de aplicaciones Enterprise, los administradores pueden cargar un archivo de metadatos SAML para configurar el inicio de sesión basado en SAML tanto de las aplicaciones de la galería de AAD, como de las que no pertenecen a la galería.

Asimismo, puede usar la dirección URL de metadatos de la federación de aplicaciones de Azure AD para configurar el SSO con la aplicación de destino.

Para obtener más información, consulte [Configuring single sign-on to applications that are not in the Azure Active Directory application gallery](https://docs.microsoft.com/azure/active-directory/active-directory-saas-custom-apps) (Configurar el inicio de sesión único de las aplicaciones que no forman parte de la galería de aplicaciones de Azure Active Directory).

---

### <a name="azure-ad-terms-of-use-now-generally-available"></a>Los Términos de uso de Azure AD están disponibles de forma general

**Tipo:** nueva característica  
**Categoría del servicio:** términos de uso  
**Funcionalidad del producto:** cumplimiento
 

Los Términos de uso de Azure AD han dejado de estar en versión preliminar pública y ahora están disponibles de forma general.

Para obtener más información, consulte [Azure AD terms of use feature](https://docs.microsoft.com/azure/active-directory/active-directory-tou) (Característica de términos de uso de Azure AD).

---

### <a name="allow-or-block-invitations-to-b2b-users-from-specific-organizations"></a>Permitir o bloquear invitaciones a usuarios B2B procedentes de determinadas organizaciones

**Tipo:** nueva característica  
**Categoría del servicio:** B2B  
**Funcionalidad del producto:** B2B/B2C
 

Ya puede especificar con qué organizaciones de socio quiere compartir contenido y colaborar en Azure AD B2B Collaboration. Para hacer esto, puede elegir crear una lista de dominios específicos para otorgar permiso o denegarlo. Cuando se bloquea un dominio mediante estas funcionalidades, los empleados ya no pueden enviar invitaciones a los usuarios de ese dominio.

Esto le ayudará a controlar el acceso a los recursos, a la vez que ofrece una experiencia fluida a los usuarios aprobados.

La característica B2B Collaboration está disponible para todos los clientes de Azure Active Directory y se puede utilizar junto con las características de Azure AD Premium, como el acceso condicional y la protección de identidad, para saber al detalle cuándo inician sesión los usuarios comerciales externos y cómo lo hacen.

Para obtener más información, consulte [Allow or block invitations to B2B users from specific organizations](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-allow-deny-list) (Permitir o bloquear invitaciones a usuarios de B2B procedentes de determinadas organizaciones).

---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD

**Tipo:** nueva característica  
**Categoría del servicio:** aplicaciones empresariales  
**Funcionalidad del producto:** integración de terceros

En abril de 2018, hemos agregado estas 13 nuevas aplicaciones con compatibilidad con la federación a nuestra galería de aplicaciones:

Criterion HCM, [FiscalNote](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fiscalnote-tutorial), [Secret Server (local)](https://docs.microsoft.com/azure/active-directory/active-directory-saas-secretserver-on-premises-tutorial), [Dynamic Signal](https://docs.microsoft.com/azure/active-directory/active-directory-saas-dynamicsignal-tutorial), [mindWireless](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mindwireless-tutorial), [OrgChart Now](https://docs.microsoft.com/azure/active-directory/active-directory-saas-orgchartnow-tutorial), [Ziflow](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ziflow-tutorial), [AppNeta Performance Monitor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-appneta-tutorial), [Elium](https://docs.microsoft.com/azure/active-directory/active-directory-saas-elium-tutorial) , [Fluxx Labs](https://docs.microsoft.com/azure/active-directory/active-directory-saas-fluxxlabs-tutorial), [Cisco Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-ciscocloud-tutorial), Shelf, [SafetyNet](https://docs.microsoft.com/azure/active-directory/active-directory-saas-safetynet-tutorial)

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial).

Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing).

---
 
### <a name="grant-b2b-users-in-azure-ad-access-to-your-on-premises-applications-public-preview"></a>Conceder a los usuarios de B2B en Azure AD acceso a las aplicaciones locales (versión preliminar pública)

**Tipo:** nueva característica  
**Categoría del servicio:** B2B  
**Funcionalidad del producto:** B2B/B2C

Las organizaciones que usan las funcionalidades de colaboración B2B de Azure Active Directory (Azure AD) para invitar a los usuarios invitados de organizaciones asociadas a su instancia de Azure AD, ahora pueden proporcionar a estos usuarios B2B acceso a las aplicaciones locales. Estas aplicaciones locales pueden usar la autenticación basada en SAML o la autenticación de Windows integrada (IWA) con la delegación limitada de kerberos (KCD).

Para obtener más información, consulte [Conceder a los usuarios de B2B en Azure AD acceso a las aplicaciones locales](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-hybrid-cloud-to-on-premises).

---
 
### <a name="get-sso-integration-tutorials-from-the-azure-marketplace"></a>Obtener tutoriales de integración de SSO en Azure Marketplace

**Tipo:** característica modificada  
**Categoría del servicio:** otros  
**Funcionalidad del producto:** integración de terceros

Si una aplicación incluida en [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/azure-active-directory-apps?page=1) admite el proceso de inicio de sesión único basado en SAML, al hacer clic en **Obtener ahora**  se proporciona el tutorial de integración asociado a esa aplicación. 

---

### <a name="faster-performance-of-azure-ad-automatic-user-provisioning-to-saas-applications"></a>Rendimiento más rápido del aprovisionamiento de usuarios automático de Azure AD para aplicaciones SaaS

**Tipo:** característica modificada  
**Categoría del servicio:** aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** integración de terceros
 
Anteriormente, los clientes que utilizaban los conectores de aprovisionamiento de usuarios de Azure Active Directory para aplicaciones de SaaS (por ejemplo, Salesforce, ServiceNow y Box) podían experimentar un rendimiento lento si sus inquilinos de Azure AD contenían más de 100 000 usuarios y grupos combinados, y si usaban asignaciones de usuarios y grupos para determinar qué usuarios debían aprovisionarse.

El 2 de abril de 2018 se implementaron mejoras de rendimiento significativas en el servicio de aprovisionamiento de Azure AD que reducen en gran medida la cantidad de tiempo necesario para realizar las sincronizaciones iniciales entre Azure Active Directory y las aplicaciones de SaaS de destino.

Como resultado, ahora se completan en cuestión de minutos u horas sincronizaciones iniciales que muchos clientes realizaban en sus aplicaciones y que antes tardaban días en completarse o que nunca llegaban a completarse.

Para obtener más información, consulte [¿Qué ocurre durante el aprovisionamiento?](https://docs.microsoft.com/azure/active-directory/active-directory-saas-app-provisioning#what-happens-during-provisioning)

---

### <a name="self-service-password-reset-from-windows-10-lock-screen-for-hybrid-azure-ad-joined-machines"></a>Restablecimiento de contraseña de autoservicio desde la pantalla de bloqueo de Windows 10 para máquinas híbridas unidas a Azure AD

**Tipo:** característica modificada  
**Categoría del servicio:** autoservicio de restablecimiento de contraseña  
**Funcionalidad del producto:** autenticación de usuarios
 
Hemos actualizado la característica SSPR de Windows 10 para incluir soporte técnico para máquinas híbridas que se hayan unido a Azure AD. Esta característica está disponible en Windows 10 RS4 y permite a los usuarios restablecer la contraseña en la pantalla de bloqueo de una máquina Windows 10. Los usuarios que están habilitados y registrados para poder realizar el restablecimiento de contraseña de autoservicio pueden utilizar esta característica.

Para obtener más información, consulte [Azure AD password reset from the login screen](https://docs.microsoft.com/azure/active-directory/authentication/tutorial-sspr-windows) (Restablecer la contraseña de Azure AD desde la pantalla de inicio de sesión).
 
---

## <a name="march-2018"></a>Marzo de 2018
 
### <a name="certificate-expire-notification"></a>Notificación de expiración de certificado

**Tipo:** fijo  
**Categoría del servicio:** aplicaciones empresariales  
**Funcionalidad del producto:** SSO
 
Azure AD envía una notificación cuando un certificado de una aplicación incluida o no en una galería está a punto de expirar. 

Algunos usuarios no recibían notificaciones sobre aplicaciones empresariales configuradas con el inicio de sesión único basado en SAML. Este problema se ha resuelto. Azure AD envía una notificación para los certificados que expiran en 7, 30 y 60 días. Este evento puede verse en los registros de auditoría. 

Para más información, consulte:

- [Administrar certificados para inicio de sesión único federado en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs)
- [Informes de actividad de auditoría en el portal de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs)
 
---
 
### <a name="twitter-and-github-identity-providers-in-azure-ad-b2c"></a>Proveedores de identidades de Twitter y GitHub en Azure AD B2C

**Tipo:** nueva característica  
**Categoría del servicio:** B2C - administración de identidades de consumidores  
**Funcionalidad del producto:** B2B/B2C
 
Ahora, puede agregar Twitter o GitHub como proveedores de identidades en Azure AD B2C. Twitter está pasando de la versión preliminar pública a la versión de disponibilidad general. GitHub se va a publicar en la versión preliminar pública.

Para más información, consulte [¿Qué es la colaboración B2B de Azure AD?](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b)
 
---

### <a name="restrict-browser-access-using-intune-managed-browser-with-azure-ad-application-based-conditional-access-for-ios-and-android"></a>Restricción del acceso de explorador mediante Intune Managed Browser con acceso condicional basado en aplicaciones de Azure AD para iOS y Android

**Tipo:** nueva característica  
**Categoría del servicio:** acceso condicional  
**Funcionalidad del producto:** protección y seguridad de la identidad
 
**Ahora en versión preliminar pública**

**SSO de Intune Managed Browser:** los empleados pueden utilizar el inicio de sesión único por los clientes nativos (por ejemplo, Microsoft Outlook) e Intune Managed Browser para todas las aplicaciones de Azure AD conectados.

**Compatibilidad con el acceso condicional de Intune Managed Browser:** ahora puede requerir que los empleados puedan usar Intune Managed Browser mediante directivas de acceso condicional basado en la aplicación.

Lea más sobre esto en nuestra [entrada de blog](https://cloudblogs.microsoft.com/enterprisemobility/2018/03/15/the-intune-managed-browser-now-supports-azure-ad-sso-and-conditional-access/).

Para más información, consulte:

- [Configuración del acceso condicional basado en aplicaciones](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

- [Configuración de directivas de Managed Browser](https://aka.ms/managedbrowser)  

---
 
### <a name="app-proxy-cmdlets-in-powershell-ga-module"></a>Cmdlets del proxy de aplicación en el módulo de disponibilidad general de Powershell

**Tipo:** nueva característica  
**Categoría del servicio:** proxy de aplicaciones  
**Funcionalidad del producto:** control de acceso
 
Ahora, los cmdlets del proxy de aplicación pueden utilizarse en el módulo de disponibilidad general de Powershell. Es necesario mantener actualizados los módulos de Powershell; si lleva más de un año de retraso, es posible que algunos cmdlets dejen de funcionar. 

Para más información, consulte [AzureAD](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0).
 
---
 
### <a name="office-365-native-clients-are-supported-by-seamless-sso-using-a-non-interactive-protocol"></a>Loa clientes nativos de Office 365 son compatibles con SSO de conexión directa utilizando un protocolo no interactivo

**Tipo:** nueva característica  
**Categoría del servicio:** autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** autenticación de usuarios
 
Los usuarios que utilizan clientes nativos de Office 365 (versión 16.0.8730.xxxx y posteriores) pueden iniciar sesión sin que sea necesaria su intervención mediante SSO de conexión directa. Esta funcionalidad es posible gracias a la incorporación de un protocolo no interactivo (WS-Trust) en Azure AD.

Para más información, consulte [¿Cómo funciona el inicio de sesión en un cliente nativo con SSO de conexión directa?](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-how-it-works#how-does-sign-in-on-a-native-client-with-seamless-sso-work)
 
---

### <a name="users-get-a-silent-sign-on-experience-with-seamless-sso-if-an-application-sends-sign-in-requests-to-azure-ads-tenant-endpoints"></a>Con SSO de conexión directa, no es necesaria la intervención del usuario para iniciar sesión cuando una aplicación envía solicitudes de inicio de sesión a puntos de conexión de inquilinos de Azure AD.

**Tipo:** nueva característica  
**Categoría del servicio:** autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** autenticación de usuarios
 
Con el inicio de sesión único de conexión directa, los usuarios no necesitan intervenir para iniciar sesión si una aplicación (por ejemplo, `https://contoso.sharepoint.com`) envía solicitudes de inicio de sesión a puntos de conexión de inquilinos de Azure AD (por ejemplo, `https://login.microsoftonline.com/contoso.com/<..>` o `https://login.microsoftonline.com/<tenant_ID>/<..>`), en lugar de a puntos de conexión comunes de Azure AD (`https://login.microsoftonline.com/common/<...>`).

Para más información, consulte [Inicio de sesión único de conexión directa de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 

---
 
### <a name="need-to-add-only-one-azure-ad-url-instead-of-two-urls-previously-to-users-intranet-zone-settings-to-roll-out-seamless-sso"></a>Solo es necesario agregar una dirección URL de Azure AD, en lugar de las dos que se requerían anteriormente, en la configuración de la zona de intranet de los usuarios para activar el SSO de conexión directa

**Tipo:** nueva característica  
**Categoría del servicio:** autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** autenticación de usuarios
 
Si desea activar el SSO de conexión directa para los usuarios, solo debe agregar una dirección URL de Azure AD en la configuración de la zona de intranet de los usuarios utilizando una directiva de grupo de Active Directory: `https://autologon.microsoftazuread-sso.com`. Anteriormente, era necesario que los clientes agregaran dos direcciones URL.

Para más información, consulte [Inicio de sesión único de conexión directa de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). 
 
---
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Nuevas aplicaciones federadas disponibles en la Galería de aplicaciones de Azure AD

**Tipo:** nueva característica  
**Categoría del servicio:** aplicaciones empresariales  
**Funcionalidad del producto:** integración de terceros

En abril de 2018, hemos agregado estas 15 nuevas aplicaciones con compatibilidad con la federación a nuestra galería de aplicaciones:

[Boxcryptor](https://docs.microsoft.com/azure/active-directory/active-directory-saas-boxcryptor-tutorial), [CylancePROTECT](https://docs.microsoft.com/azure/active-directory/active-directory-saas-cylanceprotect-tutorial), Wrike, [SignalFx](https://docs.microsoft.com/azure/active-directory/active-directory-saas-signalfx-tutorial), Assistant by FirstAgenda, [YardiOne](https://docs.microsoft.com/azure/active-directory/active-directory-saas-yardione-tutorial), Vtiger CRM, inwink, [Amplitude](https://docs.microsoft.com/azure/active-directory/active-directory-saas-amplitude-tutorial), [Spacio](https://docs.microsoft.com/azure/active-directory/active-directory-saas-spacio-tutorial), [ContractWorks](https://docs.microsoft.com/azure/active-directory/active-directory-saas-contractworks-tutorial), [Bersin](https://docs.microsoft.com/azure/active-directory/active-directory-saas-bersin-tutorial), [Mercell](https://docs.microsoft.com/azure/active-directory/active-directory-saas-mercell-tutorial), [Trisotech Digital Enterprise Server](https://docs.microsoft.com/azure/active-directory/active-directory-saas-trisotechdigitalenterpriseserver-tutorial) y [Qumu Cloud](https://docs.microsoft.com/azure/active-directory/active-directory-saas-qumucloud-tutorial).
 
Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial).

Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---
 
### <a name="pim-for-azure-resources-is-generally-available"></a>PIM de Azure Resources está disponible con carácter general

**Tipo:** nueva característica  
**Categoría del servicio:** Privileged Identity Management  
**Funcionalidad del producto:** Privileged Identity Management
 
Si utiliza Azure AD Privileged Identity Management para los roles de directorio, ahora puede utilizar las funcionalidades de asignación y acceso con límite de tiempo de PIM con los roles de Recursos de Azure, como Suscripciones, Grupos de recursos, Máquinas virtuales y cualquier otro recurso compatible con Azure Resource Manager. Puede forzar la aplicación de la autenticación multifactor al activar roles Just-In-Time y programar activaciones de forma coordinada con los periodos de cambio aprobados. Además, esta versión incorpora mejoras que no está disponibles en la versión preliminar pública, como una interfaz de usuario actualizada, flujos de trabajo de aprobación y la capacidad de ampliar roles que van a expirar en breve y de renovar roles que han expirado.

Para obtener más información, consulte [PIM para recursos de Azure (versión preliminar)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).
 
---
 
### <a name="adding-optional-claims-to-your-apps-tokens-public-preview"></a>Incorporación de notificaciones opcionales en los tokens de aplicaciones (versión preliminar pública)

**Tipo:** nueva característica  
**Categoría del servicio:** autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** autenticación de usuarios
 
Ahora, las aplicaciones de Azure AD pueden solicitar notificaciones personalizadas u opcionales en los tokens SAML o JWT.  Estas notificaciones sobre el usuario o el inquilino no se incluyen de forma predeterminada en el token por restricciones de tamaño o aplicabilidad.  Actualmente, esta funcionalidad está en versión preliminar pública para las aplicaciones de Azure AD de los puntos de conexión 1.0 y 2.0.  Consulte la documentación para obtener información acerca de qué notificaciones pueden agregarse y cómo editar el manifiesto de aplicación para solicitarlas.  

Para más información, consulte [Optional claims in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-optional-claims) (Notificaciones opcionales de Azure AD)///.
 
---
 
### <a name="azure-ad-supports-pkce-for-more-secure-oauth-flows"></a>Azure AD permite usar PKCE para disponer de flujos de OAuth más seguros

**Tipo:** nueva característica  
**Categoría del servicio:** autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** autenticación de usuarios
 
Los documentos de Azure AD se han actualizado para que tengan en cuenta la compatibilidad con PKCE, lo que permite una comunicación más segura durante el flujo de concesión del código de autorización de OAuth 2.0.  Los puntos de conexión de la versión 1.0 y 2.0 admiten S256 y code_challenges de texto no cifrado. 

Para más información, consulte [Solicitud de un código de autorización](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-protocols-oauth-code#request-an-authorization-code). 
 
---
 
### <a name="support-for-provisioning-all-user-attribute-values-available-in-the-workday-getworkers-api"></a>Compatibilidad con el aprovisionamiento de todos los valores de atributo de usuario disponibles en Workday Get_Workers API

**Tipo:** nueva característica  
**Categoría del servicio:** aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** integración de terceros
 
Ahora, la versión preliminar pública de aprovisionamiento de entrada de Workday a Active Directory y Azure AD permite extraer y aprovisionar todos los valores de atributo disponibles en Workday Get_Workers API. De este modo, se amplía la compatibilidad con cientos de atributos estándar y personalizados, que se suman a los que ya venían con la versión inicial del conector de aprovisionamiento de entrada de Workday.

Para más información, consulte: [Personalización de la lista de atributos de usuario de Workday](https://docs.microsoft.com/azure/active-directory/active-directory-saas-workday-inbound-tutorial#customizing-the-list-of-workday-user-attributes)

---

### <a name="changing-group-membership-from-dynamic-to-static-and-vice-versa"></a>Cambio de la pertenencia a grupos de dinámica a estática, y viceversa

**Tipo:** nueva característica  
**Categoría del servicio:** administración de grupos  
**Funcionalidad del producto:** colaboración
 
Es posible cambiar cómo se administra la pertenencia a un grupo. Esto es útil cuando desea mantener el mismo nombre de grupo y el identificador en el sistema, por lo que cualquier referencia existente al grupo sigue siendo válida; crear un nuevo grupo requeriría actualizar esas referencias.
Hemos actualizado el centro de administración de Azure AD para incorporar la compatibilidad con esta funcionalidad. Ahora, los clientes pueden cambiar los grupos existentes para que tengan una pertenencia dinámica en lugar de una pertenencia asignada, y viceversa. Los cmdlets de PowerShell existentes seguirán estando disponibles.

Para más información, consulte [Cambio de la pertenencia dinámica a estática, y viceversa](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal#changing-dynamic-membership-to-static-and-vice-versa)

---

### <a name="improved-sign-out-behavior-with-seamless-sso"></a>Comportamiento de cierre de sesión mejorado con SSO de conexión directa

**Tipo:** característica modificada  
**Categoría del servicio:** autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** autenticación de usuarios
 
Anteriormente, aunque los usuarios cerraran sesión explícitamente en una aplicación protegida con Azure AD, la sesión se iniciaba de nuevo automáticamente mediante SSO de conexión directa si intentaban volver a acceder a una aplicación de Azure AD en la red corporativa desde los dispositivos unidos al dominio. Con este cambio, se puede cerrar sesión.  De este modo, los usuarios pueden elegir otra cuenta de Azure AD al iniciar sesión de nuevo, en lugar de la que la sesión se inicie automáticamente con el SSO de conexión directa.

Para más información, consulte [Inicio de sesión único de conexión directa de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso).
 
---
 
### <a name="application-proxy-connector-version-154020-released"></a>Se ha publicado la versión 1.5.402.0 del conector del proxy de aplicación

**Tipo:** característica modificada  
**Categoría del servicio:** proxy de aplicaciones  
**Funcionalidad del producto:** protección y seguridad de la identidad
 
Esta versión del conector se irá implementando gradualmente hasta noviembre. Esta nueva versión del conector incluye los siguientes cambios:

- Ahora, el conector establece cookies de nivel de dominio en lugar de cookies de nivel de subdominio. De este modo, la experiencia de SSO resulta más fluida y se evita el envío de peticiones de autenticación redundantes.
- Se admiten solicitudes de codificación fragmentada.
- Se ha mejorado la supervisión del mantenimiento del conector. 
- Se han solucionado algunos errores y se han hecho mejoras en la estabilidad.

Para más información, consulte [Descripción de los conectores de Azure Active Directory Application Proxy](https://docs.microsoft.com/azure/active-directory/application-proxy-understand-connectors).
 
---

## <a name="february-2018"></a>Febrero de 2018
 
### <a name="improved-navigation-for-managing-users-and-groups"></a>Navegación mejorada para administrar usuarios y grupos

**Tipo:** plan de cambio  
**Categoría del servicio:** administración de directorios  
**Funcionalidad del producto:** directorio

Se ha simplificado la experiencia de navegación para administrar usuarios y grupos. Ahora puede ir directamente desde la información general del directorio a la lista de todos los usuarios, con un acceso más fácil a la lista de usuarios eliminados. Ahora puede ir directamente desde la información general del directorio a la lista de todos los grupos, con un acceso más fácil a la configuración de administración de grupos. Y también en la página de información general del directorio puede buscar un usuario, grupo, aplicación empresarial o registro de la aplicación. 

---

### <a name="availability-of-sign-ins-and-audit-reports-in-microsoft-azure-operated-by-21vianet-azure-china-21vianet"></a>Disponibilidad de los informes de inicios de sesión y auditoría en Microsoft Azure controlado por 21Vianet (Azure China 21Vianet)

**Tipo:** nueva característica  
**Categoría del servicio:** Azure Stack  
**Funcionalidad del producto:** supervisión e informes

Los informes de registro de actividad de Azure AD ya están disponibles en Microsoft Azure controlado por instancias de 21Vianet (Azure China 21Vianet). Se incluyen los siguientes registros:

- **Registros de actividad de inicios de sesión**: incluye todos los registros de inicios de sesión asociados con el inquilino.

- **Registros de auditoría de contraseñas de autoservicio**: incluye todos los registros de auditoría de SSPR.

- **Registros de auditoría de administración de directorios**: incluye todos los registros de auditoría relacionados con la administración de directorios como, por ejemplo, administración de usuarios, administración de aplicaciones y otros.

Con estos registros, puede obtener información detallada sobre el funcionamiento de su entorno. Los datos proporcionados le permiten:

- Determinar cómo utilizan los usuarios las aplicaciones y servicios.

- Solucionar problemas que impiden a los usuarios finalizar su trabajo.

Para más información sobre cómo usar estos informes, consulte [Informes de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal).

---

### <a name="use-report-reader-role-non-admin-role-to-view-azure-ad-activity-reports"></a>Use el rol "Lector de informes" (rol que no es de administrador) para ver los informes de actividad de Azure AD

**Tipo:** nueva característica  
**Categoría del servicio:** informes  
**Funcionalidad del producto:** supervisión e informes

Como parte de los comentarios de los clientes para habilitar los roles que no son de administrador para acceder a los registros de actividades de Azure AD, hemos habilitado la posibilidad de que los usuarios con el rol "Lector de informes" puedan acceder a las actividades de inicios de sesión y de auditoría en Azure Portal, así como mediante Graph API. 

Para más información sobre cómo usar estos informes, consulte [Informes de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal). 

---

### <a name="employeeid-claim-available-as-user-attribute-and-user-identifier"></a>Notificación de EmployeeID disponible como atributo de usuario e identificador de usuario

**Tipo:** nueva característica  
**Categoría del servicio:** aplicaciones empresariales  
**Funcionalidad del producto:** SSO
 
Puede configurar **EmployeeID** como el identificador de usuario y el atributo de usuario para los usuarios miembros y para los invitados B2B en aplicaciones de inicio de sesión basadas en SAML desde la interfaz de usuario de la aplicación empresarial.

Para más información, consulte [Personalización de las notificaciones emitidas en el token SAML para aplicaciones empresariales en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization).

---

### <a name="simplified-application-management-using-wildcards-in-azure-ad-application-proxy"></a>Administración simplificada de aplicaciones mediante caracteres comodín en Azure AD Application Proxy

**Tipo:** nueva característica  
**Categoría del servicio:** proxy de aplicaciones  
**Funcionalidad del producto:** autenticación de usuarios
 
Para facilitar la implementación de aplicaciones y reducir la carga administrativa, ahora se admite la posibilidad de publicar aplicaciones mediante caracteres comodín. Para publicar una aplicación con comodín, puede seguir el flujo de publicación de aplicaciones estándar, pero usando un carácter comodín en las direcciones URL internas y externas.

Para más información, consulte [aplicaciones con comodín en Azure Active Directory Application Proxy](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-wildcard).

---

### <a name="new-cmdlets-to-support-configuration-of-application-proxy"></a>Nuevos cmdlets para admitir la configuración de Application Proxy

**Tipo:** nueva característica  
**Categoría del servicio:** proxy de aplicaciones  
**Funcionalidad del producto:** plataforma

La versión preliminar más reciente del módulo Azure AD PowerShell contiene nuevos cmdlets que permiten a los clientes configurar aplicaciones de Application Proxy mediante PowerShell.

Los nuevos cmdlets son: 

- Get-AzureADApplicationProxyApplication
- Get-AzureADApplicationProxyApplicationConnectorGroup
- Get-AzureADApplicationProxyConnector
- Get-AzureADApplicationProxyConnectorGroup
- Get-AzureADApplicationProxyConnectorGroupMembers
- Get-AzureADApplicationProxyConnectorMemberOf
- New-AzureADApplicationProxyApplication
- New-AzureADApplicationProxyConnectorGroup
- Remove-AzureADApplicationProxyApplication
- Remove-AzureADApplicationProxyApplicationConnectorGroup
- Remove-AzureADApplicationProxyConnectorGroup
- Set-AzureADApplicationProxyApplication
- Set-AzureADApplicationProxyApplicationConnectorGroup
- Set-AzureADApplicationProxyApplicationCustomDomainCertificate
- Set-AzureADApplicationProxyApplicationSingleSignOn
- Set-AzureADApplicationProxyConnector
- Set-AzureADApplicationProxyConnectorGroup

---
 
### <a name="new-cmdlets-to-support-configuration-of-groups"></a>Nuevos cmdlets para admitir la configuración de grupos

**Tipo:** nueva característica  
**Categoría del servicio:** proxy de aplicaciones  
**Funcionalidad del producto:** plataforma

La versión más reciente del módulo de Azure AD PowerShell contiene cmdlets para administrar grupos en Azure AD. Estos cmdlets no estaban disponibles anteriormente en el módulo AzureADPreview y ahora se han agregado al módulo AzureAD.

Los cmdlets de grupo que ya se han publicado con disponibilidad general son: 

- Get-AzureADMSGroup
- New-AzureADMSGroup
- Remove-AzureADMSGroup
- Set-AzureADMSGroup
- Get-AzureADMSGroupLifecyclePolicy
- New-AzureADMSGroupLifecyclePolicy
- Remove-AzureADMSGroupLifecyclePolicy
- Add-AzureADMSLifecyclePolicyGroup
- Remove-AzureADMSLifecyclePolicyGroup
- Reset-AzureADMSLifeCycleGroup   
- Get-AzureADMSLifecyclePolicyGroup

---
 
### <a name="a-new-release-of-azure-ad-connect-is-available"></a>Está disponible una nueva versión de Azure AD Connect

**Tipo:** nueva característica  
**Categoría del servicio:** sincronización de AD  
**Funcionalidad del producto:** plataforma
 
Azure AD Connect es la herramienta preferida para sincronizar datos entre Azure AD y los orígenes de datos locales, incluidos Windows Server Active Directory y LDAP.

>[!Important]
>Esta compilación introduce cambios en las reglas de sincronización y en el esquema. El servicio de sincronización de Azure AD Connect desencadenará pasos de importación completa y sincronización completa después de una actualización. Para obtener información sobre cómo cambiar este comportamiento, consulte [Aplazamiento de la sincronización completa después de la actualización](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-upgrade-previous-version#how-to-defer-full-synchronization-after-upgrade).

Esta versión incluye las siguientes actualizaciones y cambios:

**Problemas corregidos:**

- Corrección de la ventana de sincronización en las tareas en segundo plano para la página de filtrado de particiones al cambiar a la página siguiente.

- Se ha corregido un error que provocó una infracción de acceso durante la acción personalizada ConfigDB.

- Se ha corregido un error para recuperarse del tiempo de espera de la conexión de SQL.

- Se ha corregido un error que provocaba que los certificados con caracteres comodín de SAN generaran un error al realizar una comprobación de requisitos previos.

- Se ha corregido un error que provocaba que miiserver.exe se bloqueara durante una exportación del conector de AAD.

- Se ha corregido un error en el que un intento de contraseña incorrecta registrado en el controlador de dominio al ejecutarse provocaba que el asistente de AAD Connect cambiara la configuración.

**Nuevas características y mejoras**
 
- Telemetría de aplicaciones: los administradores pueden activar o desactivar este tipo de datos.

- Datos de mantenimiento de Azure AD: los administradores deben visitar el portal de mantenimiento para controlar la configuración de mantenimiento. Una vez se haya cambiado la directiva del servicio, los agentes la leerán y la aplicarán.

- Se han agregado acciones de configuración de escritura diferida de dispositivo y una barra de progreso para la inicialización de la página.

- Se han mejorado los diagnósticos generales con un informe HTML y una recopilación completa de datos en un informe HTML o de texto ZIP.

- Se ha mejorado la confiabilidad de la actualización automática y se ha agregado telemetría adicional para asegurarse de que se puede determinar el mantenimiento del servidor.

- Se han restringido los permisos disponibles para las cuentas con privilegios de la cuenta del conector AD. Para las nuevas instalaciones, el asistente restringe los permisos que las cuentas con privilegios tienen en la cuenta de MSOL tras la creación de esa cuenta. Los cambios afectan a las instalaciones rápidas y a las instalaciones personalizadas con la opción de creación automática de cuenta.

- Se ha cambiado el instalador, por lo que no se requiere el privilegio de asociación de seguridad en una instalación limpia de AADConnect.

- Se ha agregado una utilidad nueva para solucionar problemas de sincronización de un objeto específico. Actualmente, la utilidad comprueba los siguientes aspectos:

    - Error de coincidencia de UserPrincipalName entre el objeto de usuario sincronizado y la cuenta de usuario del inquilino de Azure AD.
  
    - Si se filtra el objeto de sincronización debido al filtrado de dominio
  
    - Si se filtra el objeto de sincronización debido al filtrado de unidad organizativa (UO)

- Se ha agregado una utilidad nueva para sincronizar el hash de contraseña actual almacenado en el Active Directory local para una cuenta de usuario específica. La utilidad no requiere un cambio de contraseña. 

---
 
### <a name="applications-supporting-intune-app-protection-policies-added-for-use-with-azure-ad-application-based-conditional-access"></a>Se han agregado aplicaciones compatibles con directivas de Intune App Protection para usarlas con el acceso condicional basado en aplicaciones de Azure AD

**Tipo:** característica modificada  
**Categoría del servicio:** acceso condicional  
**Funcionalidad del producto:** protección y seguridad de la identidad

Se han agregado más aplicaciones que admiten el acceso condicional basado en aplicaciones. Ahora, ya puede acceder a Office 365 y otras aplicaciones en la nube conectadas a Azure AD mediante estas aplicaciones cliente aprobadas.

Las siguientes aplicaciones se agregarán a finales del mes de febrero:

- Microsoft Power BI

- Microsoft Launcher

- Microsoft Invoicing

Para más información, consulte:

- [Requisito de aplicación cliente aprobada](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Acceso condicional basado en aplicaciones de Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-update-to-mobile-experience"></a>Actualización de los términos de uso relacionados con la experiencia móvil 

**Tipo:** característica modificada  
**Categoría del servicio:** términos de uso  
**Funcionalidad del producto:** cumplimiento

Cuando aparecen los términos de uso, puede hacer clic en **¿Tiene problemas con la visualización? Haga clic aquí.** Si hace clic en este vínculo se abren los términos de uso de forma nativa en el dispositivo. Independientemente del tamaño de fuente en el documento o el tamaño de pantalla del dispositivo, puede acercar o alejar y leer el documento según sea necesario. 

---
 
## <a name="january-2018"></a>Enero de 2018
 
### <a name="new-federated-apps-available-in-azure-ad-app-gallery"></a>Nuevas aplicaciones federadas disponibles en la Galería de aplicaciones de Azure AD 

**Tipo:** nueva característica  
**Categoría del servicio:** aplicaciones empresariales  
**Funcionalidad del producto:** integración de terceros

En enero de 2018, se agregaron a la galería de aplicaciones las siguientes aplicaciones nuevas compatibles con la federación:

[IBM OpenPages](https://go.microsoft.com/fwlink/?linkid=864698), [OneTrust Privacy Management Software](https://go.microsoft.com/fwlink/?linkid=861660), [Dealpath](https://go.microsoft.com/fwlink/?linkid=863526), [IriusRisk Federated Directory y [Fidelity NetBenefits](https://go.microsoft.com/fwlink/?linkid=864701).

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial).

Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 

---
 
### <a name="sign-in-with-additional-risk-detected"></a>Inicio de sesión con riesgo adicional detectado

**Tipo:** nueva característica  
**Categoría del servicio:** protección de la identidad  
**Funcionalidad del producto:** protección y seguridad de la identidad

La perspectiva que se obtiene de un evento de riesgo detectado está asociada a su suscripción de Azure AD. Con la edición de Azure AD Premium P2, obtiene la información más detallada acerca de todas las detecciones subyacentes.

Con la versión de Azure AD Premium P1, las detecciones que no están cubiertas por su licencia aparecen como el evento de riesgo Inicio de sesión con riesgo adicional detectado.

Para más información, consulte [Eventos de riesgo de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-risk-events).
 
---

### <a name="hide-office-365-applications-from-end-users-access-panels"></a>Ocultación de aplicaciones de Office 365 de los paneles de acceso del usuario final

**Tipo:** nueva característica  
**Categoría del servicio:** Mis aplicaciones  
**Funcionalidad del producto:** SSO

Ahora puede administrar mejor el modo en que se muestran las aplicaciones de Office 365 en los paneles de acceso de sus usuarios mediante una nueva configuración de usuario. Esta opción le resultará útil para reducir el número de aplicaciones de los paneles de acceso de los usuarios si prefiere mostrar solo las aplicaciones de Office en el Portal de Office. La configuración se encuentra en **Configuración de usuario** y tiene la etiqueta **Los usuarios solo pueden ver las aplicaciones de Office 365 en el Portal de Office 365**.

Para obtener más información, consulte [Ocultación de una aplicación de la experiencia del usuario en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

---
 
### <a name="seamless-sign-into-apps-enabled-for-password-sso-directly-from-apps-url"></a>Inicio de sesión sin problemas en aplicaciones habilitadas para SSO de contraseña directamente desde la dirección URL de la aplicación 

**Tipo:** nueva característica  
**Categoría del servicio:** Mis aplicaciones  
**Funcionalidad del producto:** SSO

La extensión del explorador Mis aplicaciones ahora está disponible en una cómoda herramienta que ofrece la funcionalidad de inicio de sesión único Mis aplicaciones como acceso directo del explorador. Tras la instalación, los usuarios verán un icono de gofre en el explorador que proporciona acceso rápido a las aplicaciones. Ahora, los usuarios podrán disfrutar de las ventajas siguientes:

- Capacidad para iniciar sesión directamente en aplicaciones de SSO de contraseña desde la página de inicio de sesión de la aplicación.
- Inicio de cualquier aplicación mediante la característica de búsqueda rápida.
- Accesos directos a aplicaciones usadas recientemente desde la extensión.
- La extensión está disponible para Edge, Chrome y Firefox.
 
Para obtener más información, consulte [Extensión de inicio de sesión seguro de Mis aplicaciones](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction#my-apps-secure-sign-in-extension).

---

### <a name="azure-ad-administration-experience-in-azure-classic-portal-has-been-retired"></a>Retirada de la experiencia de administración de Azure AD del Portal de Azure clásico

**Tipo:** obsoleto   
**Categoría del servicio:** Azure AD  
**Funcionalidad del producto:** directorio

El 8 de enero de 2018, la experiencia de administración de Azure AD se retiró del Portal de Azure clásico. Esto tuvo lugar junto con la retirada del Portal de Azure clásico. En el futuro, deberá usar el [Centro de administración de Azure AD](https://aad.portal.azure.com) para todas las tareas de administración de Azure AD basadas en el portal.
 
---

### <a name="the-phonefactor-web-portal-has-been-retired"></a>El portal web PhoneFactor se ha retirado.

**Tipo:** obsoleto  
**Categoría del servicio:** Azure AD  
**Funcionalidad del producto:** directorio
 
El 8 de enero de 2018 se retiró el portal web PhoneFactor. Este portal se usaba para la administración del servidor MFA; sin embargo, sus funciones se han movido a Azure Portal en portal.azure.com. 

La configuración de MFA se encuentra en: **Azure Active Directory \> Servidor MFA**.
 
---
 
### <a name="deprecate-azure-ad-reports"></a>Informes de Azure AD obsoletos

**Tipo:** obsoleto  
**Categoría del servicio:** informes  
**Funcionalidad del producto:** administración del ciclo de vida de las identidades  


Con la disponibilidad general de la nueva consola de administración de Azure Active Directory y las nuevas API disponibles para los informes de actividad y seguridad, las API de informes que se encontraban en el punto de conexión "/reports" se retiraron el 31 de diciembre de 2017.

**¿Qué está disponible?**

Como parte de la transición a la nueva consola de administración, existen dos nuevas API disponibles para recuperar los registros de actividad de Azure AD. El nuevo conjunto de API proporciona más funciones de filtro y ordenación, así como actividades de auditoría e inicio de sesión. Los datos que estaban disponibles anteriormente en los informes de seguridad ahora son accesibles a través de la API de eventos de riesgo de Identity Protection en Microsoft Graph.

Para más información, consulte:

- [Introducción a la API de informes de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

- [Introducción a Azure Active Directory Identity Protection y Microsoft Graph](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection-graph-getting-started)

---

## <a name="december-2017"></a>Diciembre de 2017

### <a name="terms-of-use-in-the-access-panel"></a>Términos de uso del panel de acceso

**Tipo:** nueva característica  
**Categoría del servicio:** términos de uso  
**Funcionalidad del producto:** cumplimiento
 
Ahora puede ir al panel de acceso y ver los términos de uso previamente aceptados.

Siga estos pasos:

1. Abra el [portal de Mis aplicaciones](https://myapps.microsoft.com) e inicie sesión.

2. En la esquina superior derecha, seleccione su nombre y seleccione **Perfil** en la lista. 

3. En su **Perfil**, seleccione **Revisar los términos de uso**. 

4. Ahora podrá revisar los términos de uso que aceptó. 

Para obtener más información, consulte [Característica Términos de uso de Azure Active Directory (versión preliminar)](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---
 
### <a name="new-azure-ad-sign-in-experience"></a>Nueva experiencia de inicio de sesión de Azure AD

**Tipo:** nueva característica  
**Categoría del servicio:** Azure AD  
**Funcionalidad del producto:** autenticación de usuarios
 
Azure AD y las interfaces de usuario del sistema de identidades de cuentas de Microsoft se han rediseñado para ofrecer un aspecto más homogéneo. Además, la página de inicio de sesión de Azure AD primero recopila el nombre de usuario, seguido de la credencial en una segunda pantalla.

Para obtener más información, consulte [The new Azure AD Signin Experience is now in Public Preview](https://cloudblogs.microsoft.com/enterprisemobility/2017/08/02/the-new-azure-ad-signin-experience-is-now-in-public-preview/) (Nueva experiencia de inicio de sesión de Azure AD disponible en versión preliminar pública).
 
---
 
### <a name="fewer-sign-in-prompts-a-new-keep-me-signed-in-experience-for-azure-ad-sign-in"></a>Menos solicitudes de inicio de sesión: nueva experiencia "Mantener la sesión iniciada" para el inicio de sesión de Azure AD

**Tipo:** nueva característica  
**Categoría del servicio:** Azure AD  
**Funcionalidad del producto:** autenticación de usuarios
 
Hemos sustituido la casilla **Mantener la sesión iniciada** de la página de inicio de sesión de Azure AD por un nuevo mensaje que se muestra al realizar la autenticación correctamente. 

Si se responde **Sí** a este mensaje, el servicio proporciona un token de actualización persistente. Este es el mismo comportamiento que se produce cuando se activa la casilla **Mantener la sesión iniciada** en la experiencia antigua. Para los inquilinos federados, este mensaje se muestra al autenticarse correctamente en el servicio federado.

Para obtener más información, consulte [Fewer login prompts: The new “Keep me signed in” experience for Azure AD is in preview](https://cloudblogs.microsoft.com/enterprisemobility/2017/09/19/fewer-login-prompts-the-new-keep-me-signed-in-experience-for-azure-ad-is-in-preview/) (Menos solicitudes de inicio de sesión: nueva experiencia "Mantener la sesión iniciada" de Azure AD disponible en versión preliminar). 

---

### <a name="add-configuration-to-require-the-terms-of-use-to-be-expanded-prior-to-accepting"></a>Agregar configuración para exigir expandir los términos de uso antes de su aceptación

**Tipo:** nueva característica  
**Categoría del servicio:** términos de uso  
**Funcionalidad del producto:** cumplimiento
 
Se ha agregado una opción para los administradores con el fin de exigir a usuarios finales que expandan los términos de uso antes de aceptarlos.

Seleccione **Activado** o **Desactivado** para exigir que los usuarios expandan los términos de uso. El valor **Activado** exige a usuarios que lean los términos de uso antes de aceptarlos.

Para obtener más información, consulte [Característica Términos de uso de Azure Active Directory (versión preliminar)](https://docs.microsoft.com/azure/active-directory/active-directory-tou).
 
---

### <a name="scoped-activation-for-eligible-role-assignments"></a>Activación de ámbito para las asignaciones de roles válidos

**Tipo:** nueva característica  
**Categoría del servicio:** Privileged Identity Management  
**Funcionalidad del producto:** Privileged Identity Management
 
Puede usar la activación con ámbito para activar las asignaciones de roles de recursos de Azure válidas con menos autonomía que los valores predeterminados de las asignaciones originales. Un ejemplo de esto es si se le asigna como propietario de una suscripción en su inquilino. Con la activación con ámbito, podrá activar el rol de propietario para hasta cinco de los recursos incluidos en la suscripción (por ejemplo, los grupos de recursos y las máquinas virtuales). La definición del ámbito de la activación puede reducir la posibilidad de ejecutar cambios no deseados en recursos importantes de Azure.

Para más información, vea [¿Qué es Azure AD Privileged Identity Management?](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure).
 
---
 
### <a name="new-federated-apps-in-the-azure-ad-app-gallery"></a>Nuevas aplicaciones federadas en la galería de aplicaciones de Azure AD

**Tipo:** nueva característica  
**Categoría del servicio:** aplicaciones empresariales  
**Funcionalidad del producto:** integración de terceros

En diciembre de 2017, hemos agregado estas 15 nuevas aplicaciones con compatibilidad con la federación a nuestra galería de aplicaciones:

[Accredible](https://go.microsoft.com/fwlink/?linkid=863523), Adobe Experience Manager, [EFI Digital StoreFront](https://go.microsoft.com/fwlink/?linkid=861685), [Communifire](https://go.microsoft.com/fwlink/?linkid=861676) CybSafe, [FactSet](https://go.microsoft.com/fwlink/?linkid=863525), [IMAGE WORKS](https://go.microsoft.com/fwlink/?linkid=863517), [MOBI](https://go.microsoft.com/fwlink/?linkid=863521), [Integración de Azure AD con MobileIron](https://go.microsoft.com/fwlink/?linkid=858027), [Reflektive](https://go.microsoft.com/fwlink/?linkid=863518), [SAML SSO for Bamboo by resolution GmbH](https://go.microsoft.com/fwlink/?linkid=863520), [SAML SSO for Bitbucket by resolution GmbH](https://go.microsoft.com/fwlink/?linkid=863519), [Vodeclic](https://go.microsoft.com/fwlink/?linkid=863522), WebHR, Zenegy Azure AD Integration.

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial).

Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-app-gallery-listing). 
 
---

### <a name="approval-workflows-for-azure-ad-directory-roles"></a>Flujos de trabajo de aprobación de roles de directorio de Azure AD

**Tipo:** característica modificada  
**Categoría del servicio:** Privileged Identity Management  
**Funcionalidad del producto:** Privileged Identity Management
 
El flujo de trabajo de aprobación de roles de directorio de Azure AD está disponible de manera general.

Con el flujo de trabajo de aprobación, los administradores de roles con privilegios pueden exigir que los miembros de roles aptos soliciten la activación de roles para poder usar el rol con privilegios. Ahora es posible delegar responsabilidades de aprobación en múltiples usuarios y grupos. Los miembros del rol aptos recibirán notificaciones cuando termine la aprobación y sus roles estén activos.

---
 
### <a name="pass-through-authentication-skype-for-business-support"></a>Autenticación de paso a través: soporte técnico de Skype Empresarial

**Tipo:** característica modificada  
**Categoría del servicio:** autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** autenticación de usuarios

Ahora, la autenticación de paso a través admite inicios de sesión de usuarios en aplicaciones cliente de Skype Empresarial compatibles con la autenticación moderna, incluidas las topologías híbridas y en línea. 

Para obtener más información, consulte [Topologías de Skype Empresarial compatibles con la autenticación moderna](https://technet.microsoft.com/library/mt803262.aspx).
 
---

### <a name="updates-to-azure-ad-privileged-identity-management-for-azure-rbac-preview"></a>Actualizaciones de Azure AD Privileged Identity Management para RBAC de Azure (versión preliminar)

**Tipo:** característica modificada  
**Categoría del servicio:** Privileged Identity Management  
**Funcionalidad del producto:** Privileged Identity Management
 
Con la actualización de versión preliminar pública de Azure AD Privileged Identity Management (PIM) para el control de acceso basado en roles (RBAC) de Azure, ahora podrá realizar lo siguiente:

* Usar Just Enough Administration.
* Solicitar aprobación para activar roles de recursos.
* Programar la activación futura de un rol que requiere la aprobación de roles de Azure AD y de RBAC de Azure.
 
Para obtener más información, consulte [PIM para recursos de Azure (versión preliminar)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).

---
 
## <a name="november-2017"></a>Noviembre de 2017
 
### <a name="access-control-service-retirement"></a>Retirada del servicio de control de acceso

**Tipo:** plan de cambio  
**Categoría del servicio:** Access Control Service  
**Funcionalidad del producto:** Access Control Service 

Azure Active Directory Access Control (también conocido como Access Control Service) se retirará a finales de 2018. En las próximas semanas se ofrecerá más información, como, por ejemplo, una programación detallada y una guía de migración de alto nivel. Puede dejar comentarios en esta página con preguntas acerca de Access Control Service y un miembro del equipo le responderá.

---

### <a name="restrict-browser-access-to-the-intune-managed-browser"></a>Restringir el acceso desde el explorador a Intune Managed Browser 

**Tipo:** plan de cambio  
**Categoría del servicio:** acceso condicional  
**Funcionalidad del producto:** seguridad y protección de la identidad

El uso de Intune Managed Browser como aplicación aprobada le permite restringir el acceso desde el explorador a Office 365 y otras aplicaciones en la nube conectadas a Azure AD. 

Ahora podrá configurar la siguiente condición para el acceso condicional basado en aplicaciones:

**Aplicaciones cliente:** explorador

**¿Cuál es el efecto del cambio?**

En la actualidad, el acceso está bloqueado cuando se usa esta condición. Cuando la vista previa está disponible, todos los accesos requerirán el uso de la aplicación Manager Browser. 

Busque esta funcionalidad y más información en las próximas entradas de blogs y notas de versión. 

Para obtener más información, consulte [Acceso condicional en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal).
 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Nuevas aplicaciones cliente aprobadas para el acceso condicional basado en aplicaciones de Azure AD

**Tipo:** plan de cambio  
**Categoría del servicio:** acceso condicional  
**Funcionalidad del producto:** seguridad y protección de la identidad

Las siguientes aplicaciones se incluyen en la lista de [aplicaciones cliente aprobadas](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- [Microsoft Kaizala](https://www.microsoft.com/garage/profiles/kaizala/)
- [Microsoft StaffHub](https://staffhub.office.com/what-it-is)

Para más información, consulte:

- [Requisito de aplicación cliente aprobada](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Acceso condicional basado en aplicaciones de Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)

---

### <a name="terms-of-use-support-for-multiple-languages"></a>Disponibilidad de los términos de uso en varios idiomas

**Tipo:** nueva característica    
**Categoría del servicio:** términos de uso  
**Funcionalidad del producto:** cumplimiento

Ahora, los administradores pueden crear nuevos términos de uso que contengan varios documentos PDF. Puede etiquetar estos documentos PDF con el idioma correspondiente. De este modo, se mostrará a los usuarios el documento PDF en el idioma correspondiente a sus preferencias. Si no hay ninguna coincidencia, se muestra el idioma predeterminado.

---
 

### <a name="real-time-password-writeback-client-status"></a>Estado del cliente de escritura diferida de contraseñas en tiempo real

**Tipo:** nueva característica  
**Categoría del servicio:** autoservicio de restablecimiento de contraseña  
**Funcionalidad del producto:** autenticación de usuarios

Ahora podrá revisar el estado de su cliente de escritura diferida de contraseñas local. Esta opción está disponible en la sección **Integración local** de la página [Restablecimiento de contraseña](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/PasswordReset). 

Si hay problemas con la conexión al cliente de escritura diferida local, se mostrará un mensaje de error con los elementos siguientes:

- Información del motivo por el que no puede conectarse a su cliente de escritura diferida local.
- Un vínculo a documentación que le ayuda a resolver el problema. 

Para obtener más información, consulte [Integración local](https://docs.microsoft.com/azure/active-directory/active-directory-passwords-how-it-works#on-premises-integration).

---

### <a name="azure-ad-app-based-conditional-access"></a>Acceso condicional basado en aplicaciones de Azure AD 
 
**Tipo:** nueva característica  
**Categoría del servicio:** Azure AD  
**Funcionalidad del producto:** seguridad y protección de la identidad

Ahora puede restringir el acceso a Office 365 y a otras aplicaciones de Azure en la nube conectadas a Azure AD para [aplicaciones cliente aprobadas](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement) que admiten directivas de Intune App Protection mediante el [acceso condicional basado en aplicaciones de Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access). Las directivas de Intune App Protection se utilizan para configurar y proteger los datos de empresa en estas aplicaciones cliente.

Mediante la combinación de directivas de acceso condicional [basado en aplicaciones](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access) con las directivas de acceso condicional [basado en dispositivos](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-policy-connected-applications), tiene la flexibilidad necesaria para proteger los datos de dispositivos personales y de la empresa.

Ahora tiene a su disposición los siguientes controles y condiciones para su uso con el acceso condicional basado en aplicaciones:

**Condición de plataforma admitida**

- iOS
- Android

**Condición de aplicaciones cliente**

- Aplicaciones móviles y aplicaciones de escritorio

**Control de acceso**

- Requerir aplicación cliente aprobada

Para obtener más información, consulte [Acceso condicional basado en aplicaciones de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access).
 
---

### <a name="manage-azure-ad-devices-in-the-azure-portal"></a>Administración de dispositivos de Azure AD en Azure Portal

**Tipo:** nueva característica  
**Categoría del servicio:** administración y registro de dispositivos  
**Funcionalidad del producto:** seguridad y protección de la identidad

Ahora podrá encontrar todos los dispositivos conectados a Azure AD y las actividades relacionadas con dispositivos en un solo lugar. Hay una nueva experiencia de administración para administrar todas las configuraciones e identidades de dispositivos en Azure Portal. En esta versión podrá hacer lo siguiente:

- Ver todos los dispositivos que están disponibles para el acceso condicional en Azure AD.
- Ver propiedades, incluidos los dispositivos unidos a Azure AD híbrido.
- Encontrar las claves de BitLocker para los dispositivos unidos a Azure AD, administrar el dispositivo con Intune y mucho más.
- Administrar la configuración relacionada con dispositivos de Azure AD.

Para obtener más información, consulte [Administración de dispositivos con Azure Portal](https://docs.microsoft.com/azure/active-directory/device-management-azure-portal).

---

### <a name="support-for-macos-as-a-device-platform-for-azure-ad-conditional-access"></a>Compatibilidad con macOS como plataforma de dispositivos para el acceso condicional de Azure AD 

**Tipo:** nueva característica    
**Categoría del servicio:** acceso condicional  
**Funcionalidad del producto:** seguridad y protección de la identidad 

Ahora puede incluir (o excluir) macOS como condición de plataforma de dispositivos en la directiva de acceso condicional de Azure AD. Con la incorporación de macOS a las plataformas de dispositivos compatibles, puede:

- **Inscribir y administrar dispositivos Mac OS con Intune.** Al igual que en otras plataformas, como iOS y Android, existe una aplicación de portal de empresa para macOS que permite realizar inscripciones unificadas. Utilice la nueva aplicación de portal de empresa para macOS para inscribir un dispositivo con Intune y registrarlo con Azure AD.
- **Asegurarse de que los dispositivos macOS se ajustan a las directivas de cumplimiento de su organización definidas en Intune.** En Intune en Azure Portal, ahora podrá configurar directivas de cumplimiento para dispositivos macOS. 
- **Restringir el acceso a las aplicaciones de Azure AD a solo los dispositivos macOS que cumplan las directivas.** La creación de directivas de acceso condicional usa macOS como una opción de plataforma de dispositivos independiente. Ahora podrá crear directivas condicionales específicas para macOS para el conjunto de aplicaciones de destino en Azure.

Para más información, consulte:

- [Creación de una directiva de cumplimiento para dispositivos macOS con Intune](https://aka.ms/macoscompliancepolicy)
- [Acceso condicional en Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)
 
---

### <a name="network-policy-server-extension-for-azure-multi-factor-authentication"></a>Extensión Servidor de directivas de redes para Azure Multi-Factor Authentication 

**Tipo:** nueva característica    
**Categoría del servicio:** autenticación multifactor  
**Funcionalidad del producto:** autenticación de usuarios

La extensión Servidor de directivas de redes para Azure Multi-Factor Authentication incorpora funcionalidades de Multi-Factor Authentication basadas en la nube para su infraestructura de autenticación mediante los servidores existentes. Con la extensión Servidor de directivas de redes, podrá agregar mecanismos de verificación mediante llamadas de teléfono, mensajes de texto o aplicaciones de teléfono al flujo de autenticación existente. Para ello, no tendrá que instalar, configurar ni mantener servidores nuevos. 

Esta extensión se creó para las organizaciones que quieren proteger las conexiones de redes privadas virtuales sin tener que implementar el Servidor Microsoft Azure Multi-Factor Authentication. La extensión Servidor de directivas de redes actúa como un adaptador entre RADIUS y Azure Multi-Factor Authentication basada en la nube para proporcionar un segundo factor de autenticación a los usuarios federados o sincronizados.


Para obtener más información, consulte [Integración de la infraestructura existente de NPS con Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-nps-extension).

 
---

### <a name="restore-or-permanently-remove-deleted-users"></a>Restauración o eliminación permanente de usuarios eliminados

**Tipo:** nueva característica    
**Categoría del servicio:** administración de usuarios  
**Funcionalidad del producto:** directorio 


En el centro de administración de Azure AD, ahora puede:

- Restaurar un usuario eliminado. 
- Eliminar un usuario permanentemente.


**Para probarlo:**

1. En el centro de administración de Azure AD, seleccione [Todos los usuarios](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All) en la sección **Administrar**. 

2. En la lista **Mostrar**, seleccione **Usuarios eliminados recientemente**. 

3. Seleccione uno o varios usuarios eliminados recientemente y después restáurelos o elimínelos permanentemente.

 
---

### <a name="new-approved-client-apps-for-azure-ad-app-based-conditional-access"></a>Nuevas aplicaciones cliente aprobadas para el acceso condicional basado en aplicaciones de Azure AD

 
**Tipo:** característica modificada  
**Categoría del servicio:** acceso condicional  
**Funcionalidad del producto:** seguridad y protección de la identidad


Las siguientes aplicaciones se agregaron a la lista de [aplicaciones cliente aprobadas](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement):

- Microsoft Planner
- Azure Information Protection 


Para más información, consulte:

- [Requisito de aplicación cliente aprobada](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-technical-reference#approved-client-app-requirement)
- [Acceso condicional basado en aplicaciones de Azure AD](https://docs.microsoft.com/azure/active-directory/conditional-access/app-based-conditional-access)


---

### <a name="use-or-between-controls-in-a-conditional-access-policy"></a>Posibilidad de usar el operador "OR" entre controles de una directiva de acceso condicional 


**Tipo:** característica modificada    
**Categoría del servicio:** acceso condicional  
**Funcionalidad del producto:** seguridad y protección de la identidad

 
Ahora es posible utilizar el operador "OR" (requerir uno de los controles seleccionados) en los controles de acceso condicional. Puede usar esta característica para crear directivas con el operador "OR" entre controles de acceso. Por ejemplo, puede usar esta característica para crear una directiva que requiera que un usuario inicie sesión mediante Multi-Factor Authentication "O" que esté en un dispositivo compatible.

Para obtener más información, consulte [Controles en el acceso condicional de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-controls).

 
---

### <a name="aggregation-of-real-time-risk-events"></a>Agregación de eventos de riesgo en tiempo real


**Tipo:** característica modificada    
**Categoría del servicio:** protección de la identidad  
**Funcionalidad del producto:** seguridad y protección de la identidad


En Azure AD Identity Protection, todos los eventos de riesgo en tiempo real que se han originado desde la misma dirección IP en un día determinado ahora se agregan para cada tipo de evento de riesgo. Este cambio limita el volumen de los eventos de riesgo que se muestran sin ningún cambio en la seguridad del usuario.

La detección en tiempo real subyacente funciona cada vez que el usuario inicia sesión. Si tiene configurada una directiva de seguridad de riesgo de inicio de sesión para Multi-Factor Authentication o para bloquear el acceso, esta se desencadena durante cada inicio de sesión con riesgo.

 
---
 




## <a name="october-2017"></a>Octubre de 2017


### <a name="deprecate-azure-ad-reports"></a>Informes de Azure AD obsoletos


**Tipo:** plan de cambio  
**Categoría del servicio:** informes  
**Funcionalidad del producto:** administración del ciclo de vida de las identidades  



Azure Portal proporciona lo siguiente:

- Una nueva consola de administración de Azure AD.
- Nuevas API de informes de actividades y de seguridad.
 
Debido a estas nuevas funcionalidades, las API de informes que se encuentran en el punto de conexión /reports se retiraron el 10 de diciembre de 2017. 

---

### <a name="automatic-sign-in-field-detection"></a>Detección automática de campos de inicio de sesión


**Tipo:** fijo   
**Categoría del servicio:** Mis aplicaciones  
**Funcionalidad del producto:** inicio de sesión único  



Azure AD admite la detección automática de campos de inicio de sesión para las aplicaciones que presentan un campo de nombre de usuario y contraseña HTML. Estos pasos se documentan en [Captura automática de campos de inicio de sesión para una aplicación](https://docs.microsoft.com/azure/active-directory/application-config-sso-problem-configure-password-sso-non-gallery#how-to-manually-capture-sign-in-fields-for-an-application). Puede encontrar esta funcionalidad mediante la adición de una aplicación *situada fuera de la galería* en la página **Aplicaciones empresariales** en [Azure Portal](http://aad.portal.azure.com). Además, en esta nueva aplicación podrá configurar el modo **Inicio de sesión único** en **Inicio de sesión único basado en contraseña**, especificar una URL web y, a continuación, guardar la página.
 
Debido a un problema del servicio, esta funcionalidad se deshabilitó temporalmente. El problema se ha resuelto y la detección automática del campo de inicio de sesión vuelve a estar disponible.

---

### <a name="new-multi-factor-authentication-features"></a>Nuevas características de Multi-Factor Authentication


**Tipo:** nueva característica  
**Categoría del servicio:** autenticación multifactor  
**Funcionalidad del producto:** seguridad y protección de la identidad  



Multi-Factor Authentication (MFA) es un componente esencial para la protección de su organización. Para hacer que las credenciales tengan mayor capacidad de adaptación y que la experiencia resulte más sencilla, se han agregado las siguientes características: 

- Integración de los resultados del desafío multifactor directamente en el informe de inicio de sesión de Azure AD, incluido el acceso mediante programación a los resultados de MFA.
- Integración más profunda de la configuración de MFA en la experiencia de configuración de Azure AD en Azure Portal.

Con esta versión preliminar pública, los informes y la administración de MFA son una parte integrada de la experiencia de configuración principal de AD Azure. Ahora podrá administrar la funcionalidad del portal de administración de MFA en la experiencia de Azure AD.

Para obtener más información, consulte [Referencia para los informes de la autenticación multifactor en Azure Portal](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins-mfa). 


---

### <a name="terms-of-use"></a>Términos de uso



**Tipo:** nueva característica  
**Categoría del servicio:** términos de uso  
**Funcionalidad del producto:** cumplimiento  



Puede usar los términos de uso de Azure AD para presentar información a los usuarios, como, por ejemplo, renuncias relevantes para los requisitos legales o de cumplimiento.

Los términos de uso de Azure AD puede utilizarse en los escenarios siguientes:

- Términos de uso generales para todos los usuarios de su organización.
- Términos de uso específicos basados en los atributos de un usuario (por ejemplo, médicos frente a enfermeras o empleados nacionales frente a internacionales, creados por grupos dinámicos).
- Términos de uso específicos para el acceso a aplicaciones empresariales de alto impacto, como, por ejemplo, Salesforce.

Para obtener más información, consulte [Términos de uso de Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-tou).


---

### <a name="enhancements-to-privileged-identity-management"></a>Mejoras en Privileged Identity Management


**Tipo:** nueva característica  
**Categoría del servicio:** Privileged Identity Management  
**Funcionalidad del producto:** Privileged Identity Management  


Con Privileged Identity Management (PIM) de Azure AD, podrá administrar, controlar y supervisar el acceso a los recursos de Azure (vista preliminar) dentro de su organización por parte de:

- Suscripciones
- Grupos de recursos
- Máquinas virtuales 

Todos los recursos de Azure Portal que usan la funcionalidad RBAC de Azure pueden usar las funcionalidades de seguridad y administración del ciclo de vida que ofrece Privileged Identity Management de Azure AD.

Para obtener más información, consulte [PIM para recursos de Azure (versión preliminar)](https://docs.microsoft.com/azure/active-directory/privileged-identity-management/azure-pim-resource-rbac).


---

### <a name="access-reviews"></a>Revisiones de acceso


**Tipo:** nueva característica  
**Categoría del servicio:** revisiones de acceso  
**Funcionalidad del producto:** cumplimiento  



Las organizaciones pueden usar las revisiones de acceso (versión preliminar) para administrar de manera eficaz las pertenencias a grupos y el acceso a las aplicaciones empresariales: 

- Puede volver a certificar el acceso de usuario invitado mediante las revisiones de acceso a las aplicaciones y la pertenencia a grupos. La información que proporcionan las revisiones de acceso permite a los revisores decidir de manera eficaz si deben permitir el acceso continuado a los invitados.
- Puede volver a certificar el acceso de los empleados a las aplicaciones y la pertenencia a grupos con las revisiones de acceso.

Puede recopilar los controles de revisiones de acceso en programas importantes para que su organización realice un seguimiento de las revisiones de aplicaciones vulnerables o de cumplimiento normativo.

Para obtener más información, consulte [Revisiones de acceso de Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview).


---

### <a name="hide-third-party-applications-from-my-apps-and-the-office-365-app-launcher"></a>Ocultación de aplicaciones de terceros de Mis aplicaciones y del iniciador de aplicaciones de Office 365



**Tipo:** nueva característica  
**Categoría del servicio:** Mis aplicaciones  
**Funcionalidad del producto:** inicio de sesión único  



Ahora podrá administrar mejor las aplicaciones que se muestran en los portales de usuario con la nueva propiedad **hide app**. Puede ocultar aplicaciones para ayudar en casos en los que se muestran iconos de aplicaciones para servicios back-end o iconos duplicados que se acumulan en los iniciadores de aplicaciones de usuarios. El botón de alternancia se encuentra en la sección **Propiedades** de la aplicación de terceros con la etiqueta **Visible to user?** (¿Visible para el usuario?). También puede ocultar una aplicación mediante programación con PowerShell. 

Para obtener más información, consulte [Ocultación de una aplicación de la experiencia del usuario en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app). 


**¿Qué está disponible?**

 Como parte de la transición a la nueva consola de administración, hay disponibles dos nuevas API que permiten recuperar los registros de actividad de Azure AD. El nuevo conjunto de API proporciona más funciones de filtro y ordenación, así como actividades de auditoría e inicio de sesión. Los datos que estaban disponibles anteriormente a través de los informes de seguridad ahora se proporcionan mediante la API de eventos de riesgo de Identity Protection en Microsoft Graph.


## <a name="september-2017"></a>Septiembre de 2017

### <a name="hotfix-for-identity-manager"></a>Revisión para Identity Manager


**Tipo:** característica modificada  
**Categoría del servicio:** Identity Manager  
**Funcionalidad del producto:** administración del ciclo de vida de las identidades  



Un paquete acumulativo de revisiones (compilación 4.4.1642.0) está disponible desde el 25 de septiembre de 2017 para Microsoft Identity Manager 2016 Service Pack 1. Este paquete acumulativo de revisiones:

- Resuelve problemas e incorpora mejoras.
- Es una actualización acumulativa que reemplaza a todas las actualizaciones de Identity Manager 2016 Service Pack 1 hasta la compilación 4.4.1459.0 de Identity Manager 2016. 
- Requiere disponer de Identity Manager 2016, compilación 4.4.1302.0. 

Para obtener más información, consulte [Paquete acumulativo de revisiones (compilación 4.4.1642.0) está disponible para Microsoft Identity Manager SP1 de 2016](https://support.microsoft.com/help/4021562). 

---