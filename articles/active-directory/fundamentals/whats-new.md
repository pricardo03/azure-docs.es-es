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
ms.openlocfilehash: dc4e421808ab0f79070224edea4b75f527affaf0
ms.sourcegitcommit: c61c98a7a79d7bb9d301c654d0f01ac6f9bb9ce5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/27/2018
ms.locfileid: "52426244"
---
# <a name="whats-new-in-azure-active-directory"></a>¿Cuáles son las novedades de Azure Active Directory?

>Reciba notificaciones para volver a visitar esta página y obtener actualizaciones; para ello, copie y pegue esta URL: `https://docs.microsoft.com/api/search/rss?search=%22release+notes+for+azure+AD%22&locale=en-us` al lector de fuentes ![icono RSS](./media/whats-new/feed-icon-16x16.png).

En Azure AD se realizan mejoras de forma continua. Para mantenerse al día con los avances más recientes, este artículo proporciona información acerca de los elementos siguientes:

- Versiones más recientes
- Problemas conocidos
- Corrección de errores
- Funciones obsoletas
- Planes de cambios

Esta página se actualiza mensualmente, por lo que se recomienda visitarla con frecuencia. Si busca elementos que tengan más de 6 meses, puede encontrarlos en el [Archivo de novedades de Azure Active Directory](whats-new-archive.md).

---
## <a name="october-2018"></a>Octubre de 2018

### <a name="azure-ad-logs-now-work-with-azure-log-analytics-public-preview"></a>Los registros de Azure AD ahora funcionan con Azure Log Analytics (versión preliminar pública)

**Tipo:** nueva característica  
**Categoría del servicio:** informes  
**Funcionalidad del producto:** supervisión e informes

Nos complace anunciar que ahora puede reenviar los registros de Azure AD a Azure Log Analytics. Esta característica tan solicitada le ayuda a obtener un acceso aún mejor al análisis para su negocio, las operaciones y la seguridad, además de contribuir a supervisar la infraestructura. Para más información, consulte el blog sobre [los registros de actividad de Azure Active Directory en Azure Log Analytics que ya está disponible](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Azure-Active-Directory-Activity-logs-in-Azure-Log-Analytics-now/ba-p/274843).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---october-2018"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD: octubre de 2018

**Tipo:** nueva característica  
**Categoría del servicio:** aplicaciones empresariales  
**Funcionalidad del producto:** integración de terceros
 
En octubre de 2018, hemos agregado 14 nuevas aplicaciones con compatibilidad con la federación a la galería de aplicaciones:

[My Award Points](https://docs.microsoft.com/azure/active-directory/saas-apps/myawardpoints-tutorial), [Vibe HCM](https://docs.microsoft.com/azure/active-directory/saas-apps/vibehcm-tutorial), ambyint, [MyWorkDrive](https://docs.microsoft.com/azure/active-directory/saas-apps/myworkdrive-tutorial), [BorrowBox](https://docs.microsoft.com/azure/active-directory/saas-apps/borrowbox-tutorial), Dialpad, [ON24 Virtual Environment](https://docs.microsoft.com/azure/active-directory/saas-apps/on24-tutorial), [RingCentral](https://docs.microsoft.com/azure/active-directory/saas-apps/ringcentral-tutorial), [Zscaler Three](https://docs.microsoft.com/azure/active-directory/saas-apps/zscaler-three-tutorial), [Phraseanet](https://docs.microsoft.com/azure/active-directory/saas-apps/phraseanet-tutorial), [Appraisd](https://docs.microsoft.com/azure/active-directory/saas-apps/appraisd-tutorial), [Workspot Control](https://docs.microsoft.com/azure/active-directory/saas-apps/workspotcontrol-tutorial), [Shuccho Navi](https://docs.microsoft.com/azure/active-directory/saas-apps/shucchonavi-tutorial), [Glassfrog](https://docs.microsoft.com/azure/active-directory/saas-apps/glassfrog-tutorial)

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial). Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="azure-ad-domain-services-email-notifications"></a>Notificaciones por correo electrónico de Azure AD Domain Services

**Tipo:** nueva característica  
**Categoría del servicio:** Azure AD Domain Services  
**Funcionalidad del producto:** Azure AD Domain Services

Azure AD Domain Services proporciona alertas en Azure Portal sobre configuraciones incorrectas o problemas con el dominio administrado. Estas alertas incluyen guías paso a paso para que pueda intentar corregir los problemas sin tener que ponerse en contacto con el servicio de soporte técnico.

A partir de octubre, podrá personalizar la configuración de notificaciones para el dominio administrado por lo que, cuando se produzcan nuevas alertas, se enviará un correo electrónico a un grupo de personas designado, sin necesidad de tener que comprobar constantemente si en el portal hay actualizaciones.

Para más información, consulte [Configuración de notificaciones de Azure AD Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-notifications).

---

### <a name="azure-ad-portal-supports-using-the-forcedelete-domain-api-to-delete-custom-domains"></a>El portal de Azure AD admite el uso de la API de dominio ForceDelete para eliminar los dominios personalizados 

**Tipo:** característica modificada  
**Categoría del servicio:** administración de directorios  
**Funcionalidad del producto:** directorio

Nos complace anunciar que ahora puede usar la API de dominio ForceDelete para eliminar los nombres de dominio personalizados cambiando las referencias de forma asincrónica, como los usuarios, los grupos y las aplicaciones del nombre de dominio personalizado (contoso.com) al nombre de dominio predeterminado inicial (contoso.onmicrosoft.com).

Este cambio ayuda a eliminar más rápidamente los nombres de dominio personalizados si la organización ya no utiliza el nombre o si necesita utilizar ese nombre de dominio con otra instancia de Azure AD.

Para más información, consulte [Eliminación de un nombre de dominio personalizado](https://docs.microsoft.com/azure/active-directory/users-groups-roles/domains-manage#delete-a-custom-domain-name).

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

Para acceder a esta característica y para más información, consulte [Azure Active Directory - Where is your data located?](https://aka.ms/AADDataMap) (Azure Active Directory: dónde se encuentran los datos).

---

### <a name="new-deployment-plan-available-for-the-my-apps-access-panel"></a>Nuevo plan de implementación disponible para el panel de acceso a Mis aplicaciones

**Tipo:** nueva característica  
**Categoría del servicio:** Mis aplicaciones  
**Funcionalidad del producto:** SSO

Consulte el nuevo plan de implementación disponible para el panel de acceso a Mis aplicaciones (https://aka.ms/deploymentplans).
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

Las siguientes aplicaciones se incluyen en la lista de [aplicaciones cliente aprobadas](https://docs.microsoft.com/azure/active-directory/conditional-access/technical-reference#approved-client-app-requirement):

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

A partir del 15 de noviembre de 2018, Azure AD dejará de aceptar los códigos de autenticación que se usaban anteriormente para las aplicaciones. Este cambio de seguridad ayuda a poner Azure AD en consonancia con la especificación de OAuth y se aplicará en los puntos de conexión v1 y v2.

Si la aplicación reutiliza códigos de autorización para obtener tokens para varios recursos, es recomendable que use el código para obtener un token de actualización y, a continuación, utilice este para adquirir tokens adicionales para otros recursos. Los códigos de autorización solo se pueden usar una vez, pero los tokens de actualización se pueden usar varias veces en varios recursos. Una aplicación que intente reutilizar un código de autenticación durante el flujo de código de OAuth obtendrá el error invalid_grant.

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

A partir del 15 de noviembre de 2018, Azure AD dejará de aceptar los códigos de autenticación que se usaban anteriormente para las aplicaciones. Este cambio de seguridad ayuda a poner Azure AD en consonancia con la especificación de OAuth y se aplicará en los puntos de conexión v1 y v2.

Si la aplicación reutiliza códigos de autorización para obtener tokens para varios recursos, es recomendable que use el código para obtener un token de actualización y, a continuación, utilice este para adquirir tokens adicionales para otros recursos. Los códigos de autorización solo se pueden usar una vez, pero los tokens de actualización se pueden usar varias veces en varios recursos. Una aplicación que intente reutilizar un código de autenticación durante el flujo de código de OAuth obtendrá el error invalid_grant.

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
 
Los usuarios ya pueden acceder a las aplicaciones a través de direcciones URL internas, incluso desde fuera de la red corporativa, mediante My Apps Sign-in Extension para Azure AD. Esto funcionará con todas las aplicaciones que se ha publicado con Azure Active Directory Application Proxy, en cualquier explorador que también tenga instalada la extensión de explorador del Panel de acceso. La funcionalidad de redireccionamiento de direcciones URL se habilita automáticamente una vez que un usuario inicia sesión en la extensión. La extensión se puede descargar en [Microsoft Edge](https://go.microsoft.com/fwlink/?linkid=845176), [Chrome](https://go.microsoft.com/fwlink/?linkid=866367) y [Firefox](https://go.microsoft.com/fwlink/?linkid=866366).

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