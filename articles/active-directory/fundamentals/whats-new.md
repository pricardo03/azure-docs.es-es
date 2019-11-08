---
title: Novedades Notas de la versión de Azure Active Directory | Microsoft Docs
description: Obtenga información acerca de las novedades de Azure Active Directory, como, por ejemplo, las notas de la versión más recientes, los problemas conocidos, las correcciones de errores, las funcionalidades en desuso y los próximos cambios.
services: active-directory
author: eross-msft
manager: daveba
featureFlags:
- clicktale
ms.assetid: 06a149f7-4aa1-4fb9-a8ec-ac2633b031fb
ms.service: active-directory
ms.subservice: fundamentals
ms.workload: identity
ms.topic: conceptual
ms.date: 10/17/2019
ms.author: lizross
ms.reviewer: dhanyahk
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7db35b89903de6c5027bb7ea445245d84b96d6ef
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73473310"
---
# <a name="whats-new-in-azure-active-directory"></a>¿Cuáles son las novedades de Azure Active Directory?

>Reciba notificaciones para volver a visitar esta página y obtener actualizaciones; para ello, copie y pegue la dirección URL `https://docs.microsoft.com/api/search/rss?search=%22release+notes+for+azure+AD%22&locale=en-us` en el ![icono del lector de fuentes icono RSS](./media/whats-new/feed-icon-16x16.png) lector de fuentes.

En Azure AD se realizan mejoras de forma continua. Para mantenerse al día con los avances más recientes, este artículo proporciona información acerca de los elementos siguientes:

- Versiones más recientes
- Problemas conocidos
- Corrección de errores
- Funciones obsoletas
- Planes de cambios

Esta página se actualiza mensualmente, por lo que se recomienda visitarla con frecuencia. Si busca elementos que tengan más de 6 meses, puede encontrarlos en el [Archivo de novedades de Azure Active Directory](whats-new-archive.md).

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

Para obtener más información sobre la nueva experiencia Registros de aplicaciones, consulte la sección sobre [Registros de aplicaciones en la guía de aprendizaje de Azure Portal](https://docs.microsoft.com/azure/active-directory/develop/app-registrations-training-guide).

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

### <a name="update-to-the-my-apps-page-along-with-new-workspaces-public-preview"></a>Actualización de la página Mis aplicaciones y nueva funcionalidad Áreas de trabajo (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Mis aplicaciones  
**Funcionalidad del producto:** Control de acceso

Ahora puede personalizar la forma en que los usuarios de su organización ven y acceden a la nueva experiencia Mis aplicaciones, incluido el uso de la nueva característica Áreas de trabajo, para que sea más fácil buscar aplicaciones. La nueva funcionalidad Áreas de trabajo actúa como un filtro para las aplicaciones a las que los usuarios de su organización ya tienen acceso.

Para obtener más información sobre cómo implementar la nueva experiencia Mis aplicaciones y crear áreas de trabajo, consulte [Crear áreas de trabajo en el portal Mis aplicaciones (versión preliminar)](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-workspaces).

---

### <a name="support-for-the-monthly-active-user-based-billing-model-general-availability"></a>Compatibilidad con el modelo de facturación basado en usuarios activos mensuales (disponibilidad general)

**Tipo:** Nueva característica  
**Categoría del servicio:** B2C: administración de identidades de consumidor  
**Funcionalidad del producto:** B2B/B2C

Azure AD B2C admite ahora la facturación de usuarios activos mensuales (MAU). La facturación de MAU se basa en el número de usuarios únicos con actividad de autenticación durante un mes natural. Los clientes existentes pueden pasarse a este nuevo método de facturación en cualquier momento.

A partir del 1 de noviembre de 2019, a todos los clientes nuevos se les facturará automáticamente con este método. Este método de facturación beneficia a los clientes gracias a las ventajas que ofrece en cuanto a costos y a la posibilidad de planear con anterioridad.

Para obtener más información, consulte [Actualización al modelo de facturación de usuarios activos mensuales](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-how-to-enable-billing#upgrade-to-monthly-active-users-billing-model).

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

Para más información, consulte [Tutorial: Creación y configuración de una instancia de Azure Active Directory Domain Services](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance).

---

## <a name="september-2019"></a>Septiembre de 2019

### <a name="plan-for-change-deprecation-of-the-power-bi-content-packs"></a>Planeado su cambio: Desuso de los paquetes de contenido de Power BI

**Tipo:** Plan de cambio  
**Categoría del servicio:** Informes  
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

[Civic Platform](https://docs.microsoft.com/azure/active-directory/saas-apps/civic-platform-tutorial), [Amazon Business](https://docs.microsoft.com/azure/active-directory/saas-apps/amazon-business-tutorial), [ProNovos Ops Manager](https://docs.microsoft.com/azure/active-directory/saas-apps/pronovos-ops-manager-tutorial), [Cognidox](https://docs.microsoft.com/azure/active-directory/saas-apps/cognidox-tutorial), [Viareport's Inativ Portal (Europe)](https://docs.microsoft.com/azure/active-directory/saas-apps/viareports-inativ-portal-europe-tutorial), [Azure Databricks](https://azure.microsoft.com/services/databricks), [Robin](https://docs.microsoft.com/azure/active-directory/saas-apps/robin-tutorial), [Academy Attendance](https://docs.microsoft.com/azure/active-directory/saas-apps/academy-attendance-tutorial), [Priority Matrix](https://sync.appfluence.com/pmwebng/), [Cousto MySpace](https://cousto.platformers.be/account/login), [Uploadcare](https://uploadcare.com/accounts/signup/), [Carbonite Endpoint Backup](https://docs.microsoft.com/azure/active-directory/saas-apps/carbonite-endpoint-backup-tutorial), [CPQSync by Cincom](https://docs.microsoft.com/azure/active-directory/saas-apps/cpqsync-by-cincom-tutorial), [Chargebee](https://docs.microsoft.com/azure/active-directory/saas-apps/chargebee-tutorial), [deliver.media™ Portal](https://portal.deliver.media), [Frontline Education](https://docs.microsoft.com/azure/active-directory/saas-apps/frontline-education-tutorial), [F5](https://www.f5.com/products/security/access-policy-manager), [stashcat AD connect](https://www.stashcat.com), [Blink](https://docs.microsoft.com/azure/active-directory/saas-apps/blink-tutorial), [Vocoli](https://docs.microsoft.com/azure/active-directory/saas-apps/vocoli-tutorial), [ProNovos Analytics](https://docs.microsoft.com/azure/active-directory/saas-apps/pronovos-analytics-tutorial), [Sigstr](https://docs.microsoft.com/azure/active-directory/saas-apps/sigstr-tutorial), [Darwinbox](https://docs.microsoft.com/azure/active-directory/saas-apps/darwinbox-tutorial), [Watch by Colors](https://docs.microsoft.com/azure/active-directory/saas-apps/watch-by-colors-tutorial), [Harness](https://docs.microsoft.com/azure/active-directory/saas-apps/harness-tutorial), [EAB Navigate Strategic Care](https://docs.microsoft.com/azure/active-directory/saas-apps/eab-navigate-strategic-care-tutorial)

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

Para obtener más información, consulte [Administrar reglas de pertenencia dinámica](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-update-rule).

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
**Categoría del servicio:** Informes  
**Funcionalidad del producto:** Supervisión e informes

Nos complace anunciar que los registros de actividad de Azure AD ahora están disponibles para las instancias de la nube de administración pública en Azure Monitor Ahora puede enviar registros de Azure AD a su cuenta de almacenamiento o a un centro de eventos para integrarlos con las herramientas SIEM, como [Sumologic](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-sumologic), [Splunk](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-splunk) y [Arcsight](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-arcsight). 

Para obtener más información sobre la configuración de Azure Monitor, consulte [Registros de actividad de Azure AD en Azure Monitor](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-activity-logs-azure-monitor#cost-considerations).

---

### <a name="update-your-users-to-the-new-enhanced-security-info-experience"></a>Actualice a sus usuarios a la nueva experiencia de información de seguridad mejorada

**Tipo:** Característica modificada  
**Categoría del servicio:**  Autenticaciones (inicios de sesión)   
**Funcionalidad del producto:** Autenticación de usuarios

El 25 de septiembre de 2019, desactivaremos la antigua experiencia de información de seguridad no mejorada para registrar y administrar la información de seguridad del usuario y solo activaremos la nueva [versión mejorada](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Cool-enhancements-to-the-Azure-AD-combined-MFA-and-password/ba-p/354271). Esto significa que los usuarios ya no podrán usar la experiencia antigua.

Para obtener más información sobre la experiencia de información de seguridad mejorada, consulte nuestra [documentación de administración](https://aka.ms/securityinfodocs) y nuestra [documentación de usuario](https://aka.ms/securityinfoguide).

#### <a name="to-turn-on-this-new-experience-you-must"></a>Para activar esta nueva experiencia, debe:

1. Inicie sesión en Azure Portal como administrador global o administrador de usuarios.

2. Vaya a  **Azure Active Directory>Configuración de usuario>Administrar la configuración de las características en versión preliminar del panel de acceso**.

3. En el **los usuarios pueden usar las característica en vista previa para registrar y administrar la información de seguridad: área** mejorada, seleccione **Seleccionado**, y luego elija un grupo de usuarios o elija **Todos** para activar esta función para todos los usuarios del inquilino.

4. En el ** Los usuarios pueden usar las características de vista previa para registrar y administrar el área de información ** de seguridad **, seleccione **Ninguno**.

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

## <a name="july-2019"></a>Julio de 2019

### <a name="plan-for-change-application-proxy-service-update-to-support-only-tls-12"></a>Planeado su cambio: actualización del servicio Proxy de la aplicación para que admita solo TLS 1.2

**Tipo:** Plan de cambio  
**Categoría del servicio:** Proxy de aplicaciones  
**Funcionalidad del producto:** Control de acceso

Como ayuda para que le proporcionemos nuestro cifrado más seguro, vamos a empezar a limitar el acceso del servicio Proxy de la aplicación para solo a los protocolos TLS 1.2. Esta limitación se distribuirá inicialmente a los clientes que ya usen protocolos TLS 1,2, por lo que no verá el impacto. Los protocolos TLS 1.0 y TLS 1.1 quedarán totalmente en desuso el 31 de agosto de 2019. Los clientes que aún usen TLS 1.0 y TLS 1.1 recibirán un aviso con tiempo suficiente para prepararse para este cambio.

Para mantener la conexión con el servicio Proxy de la aplicación durante todo este cambio, es aconsejable asegurarse de que las combinaciones de cliente-servidor y explorador web-servidor se actualizan para que usen TLS 1.2. También es aconsejable asegurarse de incluir los sistemas cliente que usan los empleados para acceder a las aplicaciones publicadas mediante el servicio Proxy de la aplicación.

Para obtener más información, vea [Adición de una aplicación local para el acceso remoto mediante el proxy de aplicación en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application).

---

### <a name="plan-for-change-design-updates-are-coming-for-the-application-gallery"></a>Planeado su cambio: hay actualizaciones de diseño disponibles para la Galería de aplicaciones

**Tipo:** Plan de cambio  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** SSO

Se van a realizar nuevos cambios en la interfaz de usuario en el diseño del área **Agregar desde la galería** de la hoja **Agregar una aplicación**. Estos cambios le ayudarán a encontrar más fácilmente las aplicaciones que admiten el aprovisionamiento automático, OpenID Connect, Lenguaje de marcado de aserción de seguridad (SAML) y el inicio de sesión único (SSO) con contraseña.

---

### <a name="plan-for-change-removal-of-the-mfa-server-ip-address-from-the-office-365-ip-address"></a>Planeado su cambio: Eliminación de la dirección IP del servidor MFA de la dirección IP de Office 365

**Tipo:** Plan de cambio  
**Categoría del servicio:** MFA  
**Funcionalidad del producto:** Seguridad y protección de la identidad

Vamos a quitar la dirección IP del servidor MFA del [servicio web de URL y dirección IP de Office 365](https://docs.microsoft.com/office365/enterprise/office-365-ip-web-service). Si actualmente utiliza estas páginas para actualizar la configuración del firewall, debe asegurarse de incluir también la lista de direcciones IP documentadas en la sección **requisitos del firewall del servidor Azure Multi-Factor Authentication** del artículo[Introducción a Servidor Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy#azure-multi-factor-authentication-server-firewall-requirements).

---

### <a name="app-only-tokens-now-require-the-client-app-to-exist-in-the-resource-tenant"></a>Los tokens que solo sean de aplicación ahora requieren que la aplicación cliente exista en el inquilino de recursos

**Tipo:** Corregido  
**Categoría del servicio:** Autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** Autenticación de usuarios

El 26 de julio de 2019 cambiamos la forma en que proporcionamos tokens solo de aplicaciones a través [de la concesión de credenciales de cliente](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow). Anteriormente, las aplicaciones podían obtener tokens para llamar a otras aplicaciones, independientemente de si la aplicación cliente se encontraba en el inquilino. Hemos actualizado este comportamiento para que a los recursos de inquilino único, a veces denominados API Web, solo puedan llamarlos las aplicaciones cliente que existen en el inquilino de recursos.

Si la aplicación no se encuentra en el inquilino de recursos, recibirá el siguiente mensaje de error: `The service principal named <app_name> was not found in the tenant named <tenant_name>. This can happen if the application has not been installed by the administrator of the tenant.` Para solucionar este problema, debe crear la entidad de servicio de la aplicación cliente en el inquilino, para lo que debe usar el [punto de conexión de consentimiento del administrador](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#using-the-admin-consent-endpoint) o [mediante PowerShell](https://docs.microsoft.com/azure/active-directory/develop/howto-authenticate-service-principal-powershell), lo que garantiza que el inquilino ha dado a la aplicación permiso para operar dentro del inquilino.

Para más información, consulte [Novedades en la autenticación](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#app-only-tokens-for-single-tenant-applications-are-only-issued-if-the-client-app-exists-in-the-resource-tenant).

> [!NOTE]
> El consentimiento existente entre el cliente y la API sigue sin ser necesario. Las aplicaciones aún deben realizar sus propias comprobaciones para saber si tienen autorización.

---

### <a name="new-passwordless-sign-in-to-azure-ad-using-fido2-security-keys"></a>Nuevo inicio de sesión sin contraseña para Azure AD mediante claves de seguridad FIDO2

**Tipo:** Nueva característica  
**Categoría del servicio:** Autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** Autenticación de usuarios

Los clientes de Azure AD ahora pueden establecer directivas para administrar las claves de seguridad FIDO2 para los usuarios y grupos de su organización. Los usuarios finales también pueden registrar automáticamente sus claves de seguridad, usar las claves para iniciar sesión con sus cuentas Microsoft en sitios web mientras estén en dispositivos compatibles con FIDO, así como iniciar sesión en sus dispositivos de Windows 10 unidos a Azure AD.

Para más información, consulte [Habilitación del inicio de sesión sin contraseña para Azure AD (versión preliminar)](/azure/active-directory/authentication/concept-authentication-passwordless) para obtener información relacionada con el administrador y [Configuración de la información de seguridad para usar una clave de seguridad (versión preliminar)](https://docs.microsoft.com/azure/active-directory/user-help/security-info-setup-security-key) para la información relacionada con el usuario final.

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---july-2019"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD (julio de 2019)

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros

En julio de 2019, hemos agregado estas 18 nuevas aplicaciones con compatibilidad con la federación a la galería de aplicaciones:

[Ungerboeck Software](https://docs.microsoft.com/azure/active-directory/saas-apps/ungerboeck-software-tutorial), [Bright Pattern Omnichannel Contact Center](https://docs.microsoft.com/azure/active-directory/saas-apps/bright-pattern-omnichannel-contact-center-tutorial), [Clever Nelly](https://docs.microsoft.com/azure/active-directory/saas-apps/clever-nelly-tutorial), [AcquireIO](https://docs.microsoft.com/azure/active-directory/saas-apps/acquireio-tutorial), [Looop](https://www.looop.co/schedule-a-demo/), [productboard](https://docs.microsoft.com/azure/active-directory/saas-apps/productboard-tutorial), [MS Azure SSO Access for Ethidex Compliance Office™](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-single-sign-on#password-based-sso), [Hype](https://docs.microsoft.com/azure/active-directory/saas-apps/hype-tutorial), [Abstract](https://docs.microsoft.com/azure/active-directory/saas-apps/abstract-tutorial), [Ascentis](https://docs.microsoft.com/azure/active-directory/saas-apps/ascentis-tutorial), [Flipsnack](https://www.flipsnack.com/accounts/sign-in-sso.html), [Wandera](https://docs.microsoft.com/azure/active-directory/saas-apps/wandera-tutorial), [TwineSocial](https://twinesocial.com/), [Kallidus](https://docs.microsoft.com/azure/active-directory/saas-apps/kallidus-tutorial), [HyperAnna](https://docs.microsoft.com/azure/active-directory/saas-apps/hyperanna-tutorial), [PharmID WasteWitness](https://www.pharmid.com/), [i2B Connect](https://www.i2b-online.com/sign-up-to-use-i2b-connect-here-sso-access/) y [JFrog Artifactory](https://docs.microsoft.com/azure/active-directory/saas-apps/jfrog-artifactory-tutorial)

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial). Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Automatización del aprovisionamiento de cuentas de usuario para estas aplicaciones SaaS recién admitidas

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Supervisión e informes

Ahora, puede automatizar la creación, actualización y eliminación de cuentas de usuario para estas aplicaciones recién integradas:

- [Dialpad](https://docs.microsoft.com/azure/active-directory/saas-apps/dialpad-provisioning-tutorial)

- [Federated Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/federated-directory-provisioning-tutorial)

- [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-provisioning-tutorial)

- [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-provisioning-tutorial)

- [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-provisioning-tutorial)

- [Smartsheet](https://docs.microsoft.com/azure/active-directory/saas-apps/smartsheet-provisioning-tutorial)

Para obtener más información sobre cómo proteger mejor la organización mediante el aprovisionamiento de cuentas de usuario de forma automatizada, vea [Automatización del aprovisionamiento de usuarios para aplicaciones SaaS con Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="new-azure-ad-domain-services-service-tag-for-network-security-group"></a>Nueva etiqueta de servicio de Azure AD Domain Services para el grupo de seguridad de red

**Tipo:** Nueva característica  
**Categoría del servicio:** Azure AD Domain Services  
**Funcionalidad del producto:** Azure AD Domain Services

Si está cansado de administrar largas listas de rangos y direcciones IP, puede usar la nueva etiqueta de servicio de red **AzureActiveDirectoryDomainServices** en el grupo de seguridad de red de Azure para ayudar a proteger el tráfico que entra en la subred de la red virtual de Azure AD Domain Services.

Para más información acerca de esta nueva etiqueta de servicio, consulte [Grupos de seguridad de red para Azure AD Domain Services](../../active-directory-domain-services/network-considerations.md#network-security-groups-and-required-ports).

---

### <a name="new-security-audits-for-azure-ad-domain-services-public-preview"></a>Nuevas auditorías de seguridad para Azure AD Domain Services (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Azure AD Domain Services  
**Funcionalidad del producto:** Azure AD Domain Services

Nos complace anunciar el lanzamiento de la auditoría de seguridad de Azure AD Domain Services en versión preliminar pública. La auditoría de seguridad le ayuda a proporcionar información crítica acerca de los servicios de autenticación mediante la transmisión en secuencias de eventos de auditoría de seguridad a recursos de destino, entre los que se incluyen Azure Storage, áreas de trabajo de Azure Log Analytics y Azure Event Hubs, mediante el portal de Azure AD Domain Services.

Para más información, consulte [Habilitación de auditorías de seguridad para Azure AD Domain Services (versión preliminar)](https://docs.microsoft.com/azure/active-directory-domain-services/security-audit-events).

---

### <a name="new-authentication-methods-usage--insights-public-preview"></a>Conclusiones y uso de los nuevos métodos de autenticación (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Restablecimiento de contraseñas de autoservicio  
**Funcionalidad del producto:** Supervisión e informes

Los informes de las conclusiones y uso de los nuevos métodos de autenticación pueden ayudarle a entender cómo se registran y usan características como Azure Multi-Factor Authentication y el restablecimiento de contraseña de autoservicio en su organización, lo que incluye el número de usuarios registrados para cada característica, la frecuencia con que se usa el restablecimiento de contraseña de autoservicio para restablecer contraseñas y el método de restablecimiento.

Para más información, consulte [Conclusiones y uso de los métodos de autenticación (versión preliminar)](https://docs.microsoft.com/azure/active-directory/authentication/howto-authentication-methods-usage-insights).

---

### <a name="new-security-reports-are-available-for-all-azure-ad-administrators-public-preview"></a>Hay nuevos informes de seguridad disponibles para todos los administradores de Azure AD (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Protección de identidad  
**Funcionalidad del producto:** Seguridad y protección de la identidad

Ahora, todos los administradores de Azure ad pueden seleccionar el banner de la parte superior de los informes de seguridad existentes, como el informe **Usuarios marcados en riesgo**, para empezar a usar la nueva experiencia de seguridad, como se muestra en los informes **Usuarios de riesgo**  e **Inicios de sesión de riesgo**. Con el tiempo, todos los informes de seguridad pasarán de las versiones anteriores a las nuevas y los informes nuevos le proporcionarán las siguientes funcionalidades adicionales:

- Filtrado y ordenación avanzados

- Acciones en masa, como descartar el riesgo del usuario

- Confirmación de entidades en peligro o seguras

- Estado de riesgo, que abarca: En riesgo, Descartado, Corregido y Confirmado (en peligro)

Para más información, consulte [Informe de usuarios de riesgo](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-users) e [Informe de inicios de sesión peligrosos](https://docs.microsoft.com/azure/active-directory/identity-protection/howto-investigate-risk#risky-sign-ins).

---

### <a name="new-security-audits-for-azure-ad-domain-services-public-preview"></a>Nuevas auditorías de seguridad para Azure AD Domain Services (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Azure AD Domain Services  
**Funcionalidad del producto:** Azure AD Domain Services

Nos complace anunciar el lanzamiento de la auditoría de seguridad de Azure AD Domain Services en versión preliminar pública. La auditoría de seguridad le ayuda a proporcionar información crítica acerca de los servicios de autenticación mediante la transmisión en secuencias de eventos de auditoría de seguridad a recursos de destino, entre los que se incluyen Azure Storage, áreas de trabajo de Azure Log Analytics y Azure Event Hubs, mediante el portal de Azure AD Domain Services.

Para más información, consulte [Habilitación de auditorías de seguridad para Azure AD Domain Services (versión preliminar)](https://docs.microsoft.com/azure/active-directory-domain-services/security-audit-events).

---

### <a name="new-b2b-direct-federation-using-samlws-fed-public-preview"></a>Nueva federación directa B2B mediante SAML/WS-FED (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** B2B  
**Funcionalidad del producto:** B2B/B2C

La federación directa facilita el trabajo con aquellos con asociados cuya solución de identidad administrada de tecnologías de la información no es Azure AD, ya que se trabaja con sistemas de identidad que admiten los estándares SAML o WS-Fed. Después de configurar una relación de federación directa con un asociado, todos los usuarios a los que invite desde dicho dominio pueden colaborar con usted mediante su cuenta de organización existente, lo que hace que la experiencia de sus invitados sea más fluida.

Para más información, consulte [Federación directa con AD FS y proveedores de terceros para usuarios invitados (versión preliminar)](https://docs.microsoft.com/azure/active-directory/b2b/direct-federation).

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Automatización del aprovisionamiento de cuentas de usuario para estas aplicaciones SaaS recién admitidas

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Supervisión e informes

Ahora, puede automatizar la creación, actualización y eliminación de cuentas de usuario para estas aplicaciones recién integradas:

- [Dialpad](https://docs.microsoft.com/azure/active-directory/saas-apps/dialpad-provisioning-tutorial)

- [Federated Directory](https://docs.microsoft.com/azure/active-directory/saas-apps/federated-directory-provisioning-tutorial)

- [Figma](https://docs.microsoft.com/azure/active-directory/saas-apps/figma-provisioning-tutorial)

- [Leapsome](https://docs.microsoft.com/azure/active-directory/saas-apps/leapsome-provisioning-tutorial)

- [Peakon](https://docs.microsoft.com/azure/active-directory/saas-apps/peakon-provisioning-tutorial)

- [Smartsheet](https://docs.microsoft.com/azure/active-directory/saas-apps/smartsheet-provisioning-tutorial)

Para más información acerca de cómo proteger mejor una organización mediante el aprovisionamiento automatizado de cuentas de usuario, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="new-check-for-duplicate-group-names-in-the-azure-ad-portal"></a>Nueva comprobación de nombres de grupos duplicados en el portal de Azure AD

**Tipo:** Nueva característica  
**Categoría del servicio:** Administración de grupos  
**Funcionalidad del producto:** Colaboración

Ahora, al crear o actualizar un nombre de grupo desde el portal de Azure AD, realizaremos una comprobación para ver si está duplicando un nombre de grupo existente en el recurso. Si determinamos que el nombre ya lo está usando otro grupo, se le pedirá que lo modifique.

Para más información, consulte [Administración de grupos en el portal de Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal?context=azure/active-directory/users-groups-roles/context/ugr-context).

---

### <a name="azure-ad-now-supports-static-query-parameters-in-reply-redirect-uris"></a>Azure AD ahora admite parámetros de consulta estáticos en las direcciones URI de respuesta (redireccionamiento)

**Tipo:** Nueva característica  
**Categoría del servicio:** Autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** Autenticación de usuarios

Las aplicaciones de Azure AD ahora pueden registrar y usar identificadores URI (redireccionamiento) con parámetros de consulta estáticos (por ejemplo, `https://contoso.com/oauth2?idp=microsoft`) para las solicitudes de OAuth 2.0. El parámetro de consulta estático está sujeto a la coincidencia de cadenas en los identificadores URI de respuesta, al igual que las restantes partes del identificador URI. Si no hay ninguna cadena registrada que coincida con el identificador URI de redireccionamiento con la URL descodificada, se rechazará la solicitud. Si se encuentra el identificador URI, se usa toda la cadena para redirigir al usuario, incluido el parámetro de consulta estático.

Los identificadores URI de redireccionamiento dinámico siguen estando prohibidos, ya que representan un riesgo para la seguridad y no se pueden usar para conservar la información de estado a través de una solicitud de autenticación. Para este propósito, use el parámetro `state`.

Actualmente, las pantallas de registro de aplicaciones del Azure Portal aún bloquean los parámetros de consulta. Sin embargo, puede editar manualmente el manifiesto de la aplicación para agregar y probar los parámetros de consulta en su aplicación. Para más información, consulte [Novedades en la autenticación](https://docs.microsoft.com/azure/active-directory/develop/reference-breaking-changes#redirect-uris-can-now-contain-query-string-parameters).

---

### <a name="activity-logs-ms-graph-apis-for-azure-ad-are-now-available-through-powershell-cmdlets"></a>Los registros de actividad (MS Graph API) para Azure AD ahora están disponibles a través de los cmdlets de PowerShell

**Tipo:** Nueva característica  
**Categoría del servicio:** Informes  
**Funcionalidad del producto:** Supervisión e informes

Nos complace anunciar que los registros de actividad de Azure AD (informes de auditoría e inicio de sesión) ahora están disponibles a través del módulo de Azure AD PowerShell. Antes podía crear sus propios scripts mediante los de MS Graph API y ahora hemos ampliado esa funcionalidad a los cmdlets de PowerShell.

Para más información acerca de cómo usar estos cmdlets, consulte [Cmdlets de PowerShell de Azure AD para informes](https://docs.microsoft.com/azure/active-directory/reports-monitoring/reference-powershell-reporting).

---

### <a name="updated-filter-controls-for-audit-and-sign-in-logs-in-azure-ad"></a>Controles de filtro actualizados para los registros de auditoría e inicio de sesión en Azure AD

**Tipo:** Característica modificada  
**Categoría del servicio:** Informes  
**Funcionalidad del producto:** Supervisión e informes

Hemos actualizado los informes de auditoría e inicio de sesión, por lo que ahora puede aplicar varios filtros sin tener que agregarlos como columnas en las pantallas de informes. Además, ahora puede decidir el número de filtros que desea mostrar en la pantalla. Estas actualizaciones funcionan conjuntamente para que los informes sean más fáciles de leer y se ajusten más a sus necesidades.

Para más información acerca de estas actualizaciones, consulte [Filtrado de registros de auditoría](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-audit-logs#filtering-audit-logs) y [Filtrado de las actividades de inicio de sesión](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-sign-ins#filter-sign-in-activities)de actividades de inicio de sesión.

---

## <a name="june-2019"></a>Junio de 2019

### <a name="new-riskdetections-api-for-microsoft-graph-public-preview"></a>Nueva API riskDetections de Microsoft Graph (versión preliminar)

**Tipo:** Nueva característica  
**Categoría del servicio:** Protección de identidad  
**Funcionalidad del producto:** Seguridad y protección de la identidad

Nos complace anunciar que la nueva API riskDetections de Microsoft Graph ya se encuentra en fase de versión preliminar pública. Puede usar esta nueva API para ver una lista de detecciones de riesgos de inicio de sesión y usuario relativos a Identity Protection de su organización. También puede utilizarla para consultar más eficazmente las detecciones de riesgos, incluidos detalles sobre el tipo de detección, el estado, el nivel y mucho más.

Para obtener más información, vea la [documentación de referencia de la API de detección de riesgos](https://docs.microsoft.com/graph/api/resources/riskdetection?view=graph-rest-beta).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---june-2019"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD (junio de 2019)

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros

En junio de 2019, hemos agregado estas 22 nuevas aplicaciones con compatibilidad con la federación a la galería de aplicaciones:

[Azure AD SAML Toolkit](https://docs.microsoft.com/azure/active-directory/saas-apps/saml-toolkit-tutorial), [Otsuka Shokai (大塚商会)](https://docs.microsoft.com/azure/active-directory/saas-apps/otsuka-shokai-tutorial), [ANAQUA](https://docs.microsoft.com/azure/active-directory/saas-apps/anaqua-tutorial), [Azure VPN Client](https://portal.azure.com/), [ExpenseIn](https://docs.microsoft.com/azure/active-directory/saas-apps/expensein-tutorial), [Helper Helper](https://docs.microsoft.com/azure/active-directory/saas-apps/helper-helper-tutorial), [Costpoint](https://docs.microsoft.com/azure/active-directory/saas-apps/costpoint-tutorial), [GlobalOne](https://docs.microsoft.com/azure/active-directory/saas-apps/globalone-tutorial), [Mercedes-Benz In-Car Office](https://me.secure.mercedes-benz.com/), [Skore](https://app.justskore.it/), [Oracle Cloud Infrastructure Console](https://docs.microsoft.com/azure/active-directory/saas-apps/oracle-cloud-tutorial), [CyberArk SAML Authentication](https://docs.microsoft.com/azure/active-directory/saas-apps/cyberark-saml-authentication-tutorial), [Scrible Edu](https://www.scrible.com/sign-in/#/create-account), [PandaDoc](https://docs.microsoft.com/azure/active-directory/saas-apps/pandadoc-tutorial), [Perceptyx](https://apexdata.azurewebsites.net/docs.microsoft.com/azure/active-directory/saas-apps/perceptyx-tutorial), [Proptimise OS](https://proptimise.co.uk/software/), [Vtiger CRM (SAML)](https://docs.microsoft.com/azure/active-directory/saas-apps/vtiger-crm-saml-tutorial), Oracle Access Manager for Oracle Retail Merchandising, Oracle Access Manager for Oracle E-Business Suite, Oracle IDCS for E-Business Suite, Oracle IDCS for PeopleSoft y Oracle IDCS for JD Edwards

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial). Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="automate-user-account-provisioning-for-these-newly-supported-saas-apps"></a>Automatización del aprovisionamiento de cuentas de usuario para estas aplicaciones SaaS recién admitidas

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Supervisión e informes

Ahora, puede automatizar la creación, actualización y eliminación de cuentas de usuario para estas aplicaciones recién integradas:

- [Zoom](https://docs.microsoft.com/azure/active-directory/saas-apps/zoom-provisioning-tutorial)

- [Envoy](https://docs.microsoft.com/azure/active-directory/saas-apps/envoy-provisioning-tutorial)

- [Proxyclick](https://docs.microsoft.com/azure/active-directory/saas-apps/proxyclick-provisioning-tutorial)

- [4me](https://docs.microsoft.com/azure/active-directory/saas-apps/4me-provisioning-tutorial)

Para obtener más información sobre cómo proteger mejor la organización mediante el aprovisionamiento de cuentas de usuario de forma automatizada, vea [Automatización del aprovisionamiento de usuarios para aplicaciones SaaS con Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="view-the-real-time-progress-of-the-azure-ad-provisioning-service"></a>Consulta del progreso en tiempo real del servicio de aprovisionamiento de Azure AD

**Tipo:** Característica modificada  
**Categoría del servicio:** Aprovisionamiento de aplicaciones  
**Funcionalidad del producto:** Administración del ciclo de vida de la identidad

Hemos actualizado la experiencia de aprovisionamiento de Azure AD para incluir una nueva barra de progreso que muestra a qué altura está el proceso de aprovisionamiento de usuario. Esta experiencia mejorada también proporciona información sobre el número de usuarios aprovisionados durante el ciclo actual, así como los usuarios aprovisionados hasta la fecha.

Para obtener más información, vea [Comprobación del estado de aprovisionamiento](https://docs.microsoft.com/azure/active-directory/manage-apps/application-provisioning-when-will-provisioning-finish-specific-user).

---

### <a name="company-branding-now-appears-on-sign-out-and-error-screens"></a>Ahora, la personalización de marca aparece en las pantallas de cierre de sesión y de error

**Tipo:** Característica modificada  
**Categoría del servicio:** Autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** Autenticación de usuarios

Hemos actualizado Azure AD para que, ahora, la personalización de marca de su empresa aparezca en las pantallas de cierre de sesión y de error, así como en la página de inicio de sesión. Para activar esta característica no es preciso hacer nada, Azure AD usa los recursos que ya están configurados en el área **Personalización de marca de empresa** de Azure Portal.

Para obtener más información sobre cómo configurar la personalización de marca de su empresa, vea [Incorporación de la personalización de marca en las páginas de Azure Active Directory de la organización](https://docs.microsoft.com/azure/active-directory/fundamentals/customize-branding).

---

### <a name="azure-multi-factor-authentication-mfa-server-is-no-longer-available-for-new-deployments"></a>El Servidor Azure Multi-Factor Authentication (MFA) ya no está disponible para nuevas implementaciones

**Tipo:** En desuso  
**Categoría del servicio:** MFA  
**Funcionalidad del producto:** Seguridad y protección de la identidad

A partir del 1 de julio de 2019, Microsoft ya no ofrecerá el Servidor MFA para implementaciones nuevas. Ahora, los clientes nuevos que quieran exigir la autenticación multifactor en su organización deberán usar Azure Multi-factor Authentication en la nube. Los clientes que tengan activado el Servidor MFA desde antes del 1 de julio no apreciarán ningún cambio. Podrán seguir descargando las versiones más recientes, obteniendo actualizaciones futuras y generando credenciales de activación.

Para obtener más información, vea [Introducción al Servidor Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfaserver-deploy). Para obtener más información sobre Azure Multi-Factor Authentication en la nube, vea [Planeación de una implementación de Azure Multi-Factor Authentication en la nube](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-getstarted).

---

## <a name="may-2019"></a>Mayo de 2019

### <a name="service-change-future-support-for-only-tls-12-protocols-on-the-application-proxy-service"></a>Cambio de servicio: próxima compatibilidad únicamente con protocolos TLS 1.2 en el servicio Application Proxy

**Tipo:** Plan de cambio  
**Categoría del servicio:** Proxy de aplicaciones  
**Funcionalidad del producto:** Control de acceso

A fin de proporcionar el mejor cifrado a nuestros clientes, estamos limitando el acceso a únicamente protocolos TLS 1.2 en el servicio Application Proxy. Este cambio se está implantando gradualmente en clientes que ya usan protocolos TLS 1.2 exclusivamente, por lo que no deberían apreciar cambio alguno.

La obsolescencia de TLS 1.0 y TLS 1.1 tendrá lugar el 31 de agosto de 2019, pero avisaremos con antelación para que tenga tiempo para prepararse para este cambio. Para prepararse para este cambio, asegúrese de que sus combinaciones de servidor cliente y servidor de examinador (incluidos los clientes que los usuarios usan para tener acceso a aplicaciones publicadas a través de Application Proxy) se actualizan para usar el protocolo TLS 1.2 y, así, mantengan la conexión con el servicio Application Proxy. Para obtener más información, vea [Adición de una aplicación local para el acceso remoto mediante el proxy de aplicación en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application#before-you-begin).

---

### <a name="use-the-usage-and-insights-report-to-view-your-app-related-sign-in-data"></a>Utilización del informe de uso y conclusiones para ver datos de inicio de sesión relativos a las aplicaciones

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Supervisión e informes

Ahora, puede usar el informe de uso y conclusiones, ubicado en el área **Aplicaciones empresariales** de Azure Portal, para obtener una vista de los datos de inicio de sesión relativos a las aplicaciones, con información sobre lo siguiente:

- Aplicaciones más usadas en la organización

- Aplicaciones con los inicios de sesión con más errores

- Principales errores de inicio de sesión de cada aplicación

Para obtener más información sobre esta característica, vea [Informe de uso y conclusiones en el portal de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/reports-monitoring/concept-usage-insights-report).

---

### <a name="automate-your-user-provisioning-to-cloud-apps-using-azure-ad"></a>Automatización del aprovisionamiento de usuarios a aplicaciones en la nube mediante Azure AD

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Supervisión e informes

Siga estos nuevos tutoriales para utilizar el servicio de aprovisionamiento de Azure AD para automatizar la creación, eliminación y actualización de cuentas de usuario de las siguientes aplicaciones en la nube:

- [Comeet](https://docs.microsoft.com/azure/active-directory/saas-apps/comeet-recruiting-software-provisioning-tutorial)

- [DynamicSignal](https://docs.microsoft.com/azure/active-directory/saas-apps/dynamic-signal-provisioning-tutorial)

- [KeeperSecurity](https://docs.microsoft.com/azure/active-directory/saas-apps/keeper-password-manager-digitalvault-provisioning-tutorial)

También puede seguir este nuevo [tutorial de Dropbox](https://docs.microsoft.com/azure/active-directory/saas-apps/dropboxforbusiness-provisioning-tutorial), que proporciona información sobre cómo aprovisionar objetos de grupo.

Para obtener más información sobre cómo proteger mejor la organización mediante el aprovisionamiento de cuentas de usuario de forma automatizada, vea [Automatización del aprovisionamiento de usuarios para aplicaciones SaaS con Azure AD](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning).

---

### <a name="identity-secure-score-is-now-available-in-azure-ad-general-availability"></a>La puntuación segura de identidad ya está disponible en Azure AD (disponibilidad general)

**Tipo:** Nueva característica  
**Categoría del servicio:** N/D  
**Funcionalidad del producto:** Seguridad y protección de la identidad

Ahora, puede supervisar y mejorar su posición de seguridad de identidad gracias a la característica de puntuación segura de identidad de Azure AD. La característica de puntuación segura de identidad utiliza un mismo panel para ayudarle a:

- Medir de forma objetiva su nivel de seguridad de la identidad, según una puntuación de entre 1 y 223.

- Planear la realización de mejoras en la seguridad de la identidad.

- Ver si las mejoras han logrado sus objetivos de seguridad.

Para obtener más información sobre la característica de puntuación segura de identidad, vea [¿Qué es la puntuación segura de identidad en Azure Active Directory?](https://docs.microsoft.com/azure/active-directory/fundamentals/identity-secure-score)

---

### <a name="new-app-registrations-experience-is-now-available-general-availability"></a>Nueva experiencia Registros de aplicaciones ya disponible (disponibilidad general)

**Tipo:** Nueva característica  
**Categoría del servicio:** Autenticaciones (inicios de sesión)  
**Funcionalidad del producto:** Experiencia para el desarrollador

La nueva experiencia [Registros de aplicaciones](https://aka.ms/appregistrations) ya está disponible en versión de disponibilidad general. Esta nueva experiencia incluye todas las características clave a las que está acostumbrado de Azure Portal y del portal de Registros de aplicaciones, y las mejora a través de lo siguiente:

- **Una mejor administración de las aplicaciones.** En lugar de ver las aplicaciones en diversos portales, ahora puede verlas todas en una misma ubicación.

- **Un registro de aplicaciones simplificado.** Desde la experiencia de navegación mejorada a la experiencia de selección de permisos renovada, ahora es más fácil registrar y administrar sus aplicaciones.

- **Una información más pormenorizada.** Encontrará más detalles sobre la aplicación, incluidas guías de inicio rápido y otras muchas cosas.

Para obtener más información, vea [Plataforma de identidad de Microsoft](https://docs.microsoft.com/azure/active-directory/develop/) y la entrada de blog que anuncia que la [experiencia Registros de aplicación ya está disponible de forma general](https://developer.microsoft.com/identity/blogs/new-app-registrations-experience-is-now-generally-available/) .

---

### <a name="new-capabilities-available-in-the-risky-users-api-for-identity-protection"></a>Nuevas capacidades disponibles en la API correspondiente a los usuarios de riesgo de Identity Protection

**Tipo:** Nueva característica  
**Categoría del servicio:** Protección de identidad  
**Funcionalidad del producto:** Seguridad y protección de la identidad

Nos complace anunciar que, ahora, puede usar la API correspondiente a los usuarios de riesgo para recuperar el historial de riesgos de los usuarios, descartar usuarios de riesgo y confirmar usuarios como usuarios de riesgo. Este cambio ayuda a actualizar el estado de riesgo de los usuarios de forma más eficaz y a entender sus historiales de riesgos.

Para obtener más información, vea la [documentación de referencia de la API correspondiente a los usuarios de riesgo](https://docs.microsoft.com/graph/api/resources/riskyuser?view=graph-rest-beta).

---

### <a name="new-federated-apps-available-in-azure-ad-app-gallery---may-2019"></a>Nuevas aplicaciones federadas disponibles en la galería de aplicaciones de Azure AD (mayo de 2019)

**Tipo:** Nueva característica  
**Categoría del servicio:** Aplicaciones empresariales  
**Funcionalidad del producto:** Integración de terceros

En mayo de 2019, hemos agregado estas 21 nuevas aplicaciones con compatibilidad con la federación a la galería de aplicaciones:

[Freedcamp](https://docs.microsoft.com/azure/active-directory/saas-apps/freedcamp-tutorial), [Real Links](https://docs.microsoft.com/azure/active-directory/saas-apps/real-links-tutorial), [Kianda](https://app.kianda.com/sso/OpenID/AzureAD/), [Simple Sign](https://docs.microsoft.com/azure/active-directory/saas-apps/simple-sign-tutorial), [Braze](https://docs.microsoft.com/azure/active-directory/saas-apps/braze-tutorial), [Displayr](https://docs.microsoft.com/azure/active-directory/saas-apps/displayr-tutorial), [Templafy](https://docs.microsoft.com/azure/active-directory/saas-apps/templafy-tutorial), [Marketo Sales Engage](https://toutapp.com/login), [ACLP](https://docs.microsoft.com/azure/active-directory/saas-apps/aclp-tutorial), [OutSystems](https://docs.microsoft.com/azure/active-directory/saas-apps/outsystems-tutorial), [Meta4 Global HR](https://docs.microsoft.com/azure/active-directory/saas-apps/meta4-global-hr-tutorial), [Quantum Workplace](https://docs.microsoft.com/azure/active-directory/saas-apps/quantum-workplace-tutorial), [Cobalt](https://docs.microsoft.com/azure/active-directory/saas-apps/cobalt-tutorial), [webMethods API Cloud](https://docs.microsoft.com/azure/active-directory/saas-apps/webmethods-integration-cloud-tutorial), [RedFlag](https://pocketstop.com/redflag/), [Whatfix](https://docs.microsoft.com/azure/active-directory/saas-apps/whatfix-tutorial), [Control](https://docs.microsoft.com/azure/active-directory/saas-apps/control-tutorial), [JOBHUB](https://docs.microsoft.com/azure/active-directory/saas-apps/jobhub-tutorial), [NEOGOV](https://docs.microsoft.com/azure/active-directory/saas-apps/neogov-tutorial), [Foodee](https://docs.microsoft.com/azure/active-directory/saas-apps/foodee-tutorial) y [MyVR](https://docs.microsoft.com/azure/active-directory/saas-apps/myvr-tutorial)

Para obtener más información acerca de las aplicaciones, consulte [Integración de aplicación SaaS con Azure Active Directory](https://aka.ms/appstutorial). Para obtener más información para que una aplicación se muestre en la galería de aplicaciones de Azure AD, consulte [Aprenda a mostrar su aplicación en la galería de aplicaciones de Azure Active Directory](https://aka.ms/azureadapprequest).

---

### <a name="improved-groups-creation-and-management-experiences-in-the-azure-ad-portal"></a>Experiencias mejoradas de administración y creación de grupos en el portal de Azure AD

**Tipo:** Nueva característica  
**Categoría del servicio:** Administración de grupos  
**Funcionalidad del producto:** Colaboración

Hemos realizado mejoras en las experiencias relativas a grupos del portal de Azure AD. Estas mejoras permiten a los administradores administrar de mejor forma las listas de grupos y las listas de miembros, así como proporcionar más opciones de creación.

Estas mejoras incluyen:

- Filtrado básico por tipo de pertenencia y tipo de grupo.

- Incorporación de columnas nuevas, como Origen y Dirección de correo electrónico.

- Posibilidad de seleccionar varios grupos, miembros y listas de propietarios para eliminarlos con facilidad.

- Posibilidad de elegir una dirección de correo electrónico y de agregar propietarios durante la creación del grupo.

Para obtener más información vea [Creación de un grupo básico e incorporación de miembros con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-groups-create-azure-portal).

---

### <a name="configure-a-naming-policy-for-office-365-groups-in-azure-ad-portal-general-availability"></a>Configuración de una directiva de nomenclatura para grupos de Office 365 en el portal de Azure AD (disponibilidad general)

**Tipo:** Característica modificada  
**Categoría del servicio:** Administración de grupos  
**Funcionalidad del producto:** Colaboración

Ahora, los administradores pueden configurar una directiva de nomenclatura de grupos de Office 365 mediante el portal de Azure AD. Este cambio ayuda a aplicar las convenciones de nomenclatura coherentes para los grupos de Office 365 creados o editados por los usuarios de su organización.

La directiva de nomenclatura de grupos de Office 365 se puede configurar de dos maneras diferentes:

- Definir prefijos o sufijos, que se agregan automáticamente al nombre de un grupo.

- Cargar un conjunto personalizado de palabras bloqueadas de la organización que no se permiten en los nombres de grupo (por ejemplo, "CEO, nómina, RR. HH.").

Para obtener más información, vea [Aplicación de una directiva de nomenclatura en los grupos de Office 365](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-naming-policy).

---

### <a name="microsoft-graph-api-endpoints-are-now-available-for-azure-ad-activity-logs-general-availability"></a>Ahora, los puntos de conexión de la API de Microsoft Graph están disponibles para los registros de actividad de Azure AD (disponibilidad general)

**Tipo:** Característica modificada  
**Categoría del servicio:** Informes  
**Funcionalidad del producto:** Supervisión e informes

Nos complace anunciar la disponibilidad general de compatibilidad de puntos de conexión de la API de Microsoft Graph en los registros de actividad de Azure AD. Con este lanzamiento, ahora puede usar la versión 1.0 tanto de los registros de auditoría de Azure AD como de las API de registros de inicio de sesión.

Para obtener más información, vea [Información general sobre la API de registros de auditoría de Azure AD](https://docs.microsoft.com/graph/api/resources/azure-ad-auditlog-overview?view=graph-rest-1.0).

---

### <a name="administrators-can-now-use-conditional-access-for-the-combined-registration-process-public-preview"></a>Ahora, los administradores pueden usar el acceso condicional en el proceso de registro combinado (versión preliminar pública)

**Tipo:** Nueva característica  
**Categoría del servicio:** Acceso condicional  
**Funcionalidad del producto:** Seguridad y protección de la identidad  

Ahora, los administradores pueden crear directivas de acceso condicional para su uso en la página de registro combinado. Esto incluye la puesta en marcha de directivas que permiten el registro bajo estos supuestos:

- Los usuarios están en una red de confianza.

- Los usuarios presentan un bajo riesgo de inicio de sesión.

- Los usuarios están en un dispositivo administrado.

- Los usuarios han aceptado las condiciones de uso de la organización.

Para obtener más información sobre el acceso condicional y el restablecimiento de contraseña, puede ver la entrada de blog sobre el [acceso condicional en la experiencia de registro combinado de restablecimiento de contraseña y MFA de Azure AD](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-Identity/Conditional-access-for-the-Azure-AD-combined-MFA-and-password/ba-p/566348). Para obtener más información sobre las directivas de acceso condicional en el proceso de registro combinado, vea [Directivas de acceso condicional para el registro combinado](https://docs.microsoft.com/azure/active-directory/authentication/howto-registration-mfa-sspr-combined#conditional-access-policies-for-combined-registration). Para obtener más información sobre la característica Condiciones de uso de Azure AD, vea [Característica Condiciones de uso de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/conditional-access/terms-of-use).

---
