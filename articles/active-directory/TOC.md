# [Documentación de Azure Active Directory](index.md)

# Información general
## [¿Qué es Azure Active Directory?](fundamentals/active-directory-whatis.md)
## [Acerca de la administración de identidades de Azure](fundamentals/identity-fundamentals.md)
## [Información acerca de las soluciones de identidades de Azure](fundamentals/understand-azure-identity-solutions.md)
## [Asociación de suscripciones de Azure](fundamentals/active-directory-how-subscriptions-associated-directory.md)
## [Consideraciones sobre residencia y datos](fundamentals/active-directory-data-storage-eu.md)
## [Preguntas más frecuentes](fundamentals/active-directory-faq.md)
## [Novedades](fundamentals/whats-new.md)


# Introducción
## [Suscripción a Azure AD Premium](fundamentals/active-directory-get-started-premium.md)
## [Adición de un dominio personalizado](fundamentals/add-custom-domain.md)
## [Configuración de la personalización de marca de la compañía](fundamentals/customize-branding.md)
## [Adición de usuarios a Azure AD](fundamentals/add-users-azure-active-directory.md)
## [Asignación de licencias a usuarios](fundamentals/license-users-groups.md)
## [Configuración del autoservicio de restablecimiento de contraseña](authentication/quickstart-sspr.md)
## [Incorporación de información de privacidad de su organización en Azure AD](active-directory-properties-area.md)
## [Acceso a Azure Active Directory para crear un nuevo inquilino](fundamentals/active-directory-access-create-new-tenant.md)


# Procedimientos
## Planeamiento y diseño
### [Descripción de la arquitectura de Azure AD](fundamentals/active-directory-architecture.md)
### [Asignación de notificaciones en Azure Active Directory](develop/active-directory-claims-mapping.md)
### [Implementación de una solución de identidad híbrida](hybrid/plan-hybrid-identity-design-considerations-overview.md)
#### Determinación de requisitos
##### [Identidad](hybrid/plan-hybrid-identity-design-considerations-business-needs.md)
##### [Sincronización de directorios](hybrid/plan-hybrid-identity-design-considerations-directory-sync-requirements.md)
##### [Multi-factor Authentication](hybrid/plan-hybrid-identity-design-considerations-multifactor-auth-requirements.md)
##### [Identificación de la estrategia de ciclo de vida](hybrid/plan-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)
#### [Planeamiento de la seguridad de datos](hybrid/plan-hybrid-identity-design-considerations-data-protection-strategy.md)
##### [Protección de datos](hybrid/plan-hybrid-identity-design-considerations-dataprotection-requirements.md)
##### [Administración de contenido](hybrid/plan-hybrid-identity-design-considerations-contentmgt-requirements.md)
##### [Control de acceso](hybrid/plan-hybrid-identity-design-considerations-accesscontrol-requirements.md)
##### [Respuesta a los incidentes](hybrid/plan-hybrid-identity-design-considerations-incident-response-requirements.md)
#### Planeamiento del ciclo de vida de identidad
##### [Tareas](hybrid/plan-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)
##### [Estrategia de adopción](hybrid/plan-hybrid-identity-design-considerations-identity-adoption-strategy.md)
#### [Pasos siguientes](hybrid/plan-hybrid-identity-design-considerations-nextsteps.md)
#### [Comparación de herramientas](hybrid/plan-hybrid-identity-design-considerations-tools-comparison.md)

## Administrar usuarios
### [Adición de nuevos usuarios a Azure AD](fundamentals/add-users-azure-active-directory.md)
### [Administrar perfiles de usuario](fundamentals/active-directory-users-profile-azure-portal.md)
### [Restablecimiento de contraseñas de los usuarios](fundamentals/active-directory-users-reset-password-azure-portal.md)
### [Asignación de usuarios a roles de administración](fundamentals/active-directory-users-assign-role-azure-portal.md)
### [Adición de usuarios invitados de otro directorio (B2B)](b2b/what-is-b2b.md)
#### [Adición de usuarios de B2B por los administradores](b2b/add-users-administrator.md)
#### [Adición de usuarios de B2B por los trabajadores de la información](b2b/add-users-information-worker.md)
#### [API y personalización](b2b/customize-invitation-api.md)
#### [Federación de Google](b2b/google-federation.md)
#### [Ejemplos de Code y Azure PowerShell](b2b/code-samples.md)
#### [Ejemplo de portal de registro de autoservicio](b2b/self-service-portal.md)
#### [Invitación por correo electrónico](b2b/invitation-email-elements.md)
#### [Canje de invitación](b2b/redemption-experience.md)
#### [Incorporación de usuarios B2B sin invitación](b2b/add-user-without-invite.md)
#### [Permiso o bloqueo de invitaciones](b2b/allow-deny-list.md)
#### [Acceso condicional para B2B](b2b/conditional-access.md)
#### [Directivas de uso compartido de B2B](b2b/delegate-invitations.md)
#### [Adición de un usuario B2B a un rol](b2b/add-guest-to-role.md)
#### [Grupos dinámicos y usuarios de B2B](b2b/use-dynamic-groups.md)
#### [Salida de una organización](b2b/leave-the-organization.md)
#### [Informes y auditoría](b2b/auditing-and-reporting.md)
#### [B2B para organizaciones híbridas](b2b/hybrid-organizations.md)
##### [Concesión a los usuarios B2B de acceso a las aplicaciones locales](b2b/hybrid-cloud-to-on-premises.md)
##### [Concesión a los usuarios locales de acceso a las aplicaciones en la nube](b2b/hybrid-on-premises-to-cloud.md)
#### [Uso compartido externo de B2B y Office 365](b2b/o365-external-user.md)
#### [Licencias B2B](b2b/licensing-guidance.md)
#### [Limitaciones actuales](b2b/current-limitations.md)
#### [P+F](b2b/faq.md)
#### [Solución de problemas de B2B](b2b/troubleshoot.md)
#### [Descripción del usuario de B2B](b2b/user-properties.md)
#### [Token de usuario de B2B](b2b/user-token.md)
#### [B2B para las aplicaciones integradas de Azure AD](b2b/configure-saas-apps.md)
#### [Asignación de notificaciones de usuario de B2B](b2b/claims-mapping.md)
#### [Comparación de la colaboración B2B con B2C](b2b/compare-with-b2c.md)
#### [Obtención de soporte técnico para B2B](b2b/get-support.md)

## [Administración de grupos y miembros](fundamentals/active-directory-manage-groups.md)
### [Administración de grupos](fundamentals/active-directory-groups-create-azure-portal.md)
### [Eliminación de un grupo y sus miembros](fundamentals/active-directory-groups-delete-group.md)
### [Administración de la configuración de grupo](fundamentals/active-directory-groups-settings-azure-portal.md)
## [Administración de informes](reports-monitoring/overview-reports.md)
### [Actividad de inicios de sesión](reports-monitoring/concept-sign-ins.md)
### [Actividad de auditoría](reports-monitoring/concept-audit-logs.md)
### [Usuarios en riesgo](reports-monitoring/concept-user-at-risk.md)
### [Inicios de sesión no seguros](reports-monitoring/concept-risky-sign-ins.md)
### [Eventos de riesgo](reports-monitoring/concept-risk-events.md)
### [Supervisión de registros mediante Azure Monitor](reports-monitoring/concept-activity-logs-in-azure-monitor.md)
### [P+F](reports-monitoring/reports-faq.md)

### Tareas
#### [Descarga de un informe de inicio de sesión](reports-monitoring/quickstart-download-sign-in-report.md)
#### [Descarga de un informe de auditoría](reports-monitoring/quickstart-download-audit-report.md)
#### [Configuración de ubicaciones con nombre](reports-monitoring/quickstart-configure-named-locations.md)
#### [Búsqueda de informes de actividad](reports-monitoring/howto-find-activity-reports.md)
#### [Uso del paquete de contenido de Power BI de Azure AD](reports-monitoring/howto-power-bi-content-pack.md)
#### [Corrección de usuarios marcados en riesgo](reports-monitoring/howto-remediate-users-flagged-for-risk.md)
#### [Enrutamiento de registros de actividad a un centro de eventos de Azure](reports-monitoring/quickstart-azure-monitor-stream-logs-to-event-hub.md)
#### [Archivado de registros de actividad en una cuenta de almacenamiento de Azure](reports-monitoring/quickstart-azure-monitor-route-logs-to-storage-account.md)
#### [Integración de registros de actividad con Splunk mediante Azure Monitor](reports-monitoring/tutorial-integrate-activity-logs-with-splunk.md)
#### [Integración de registros de actividad con SumoLogic mediante Azure Monitor](reports-monitoring/howto-integrate-activity-logs-with-sumologic.md)
#### [Integración de registros de actividad con Log Analytics mediante Azure Monitor](reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md)

### Referencia
#### [Retención](reports-monitoring/reference-reports-data-retention.md)
#### [Latencias](reports-monitoring/reference-reports-latencies.md)
#### [Referencia sobre actividades de auditoría](reports-monitoring/reference-audit-activities.md)
#### [Códigos de error de la actividad de inicio de sesión](reports-monitoring/reference-sign-ins-error-codes.md)
#### [Interpretación del esquema de registro de auditoría en Azure Monitor](reports-monitoring/reference-azure-monitor-audit-log-schema.md)
#### [Interpretación del esquema de registro de inicio de sesión en Azure Monitor](reports-monitoring/reference-azure-monitor-sign-ins-log-schema.md)

### Solución de problemas
#### [Datos que faltan en los registros de actividad de Azure AD](reports-monitoring/troubleshoot-missing-audit-data.md)
#### [Datos que faltan en las descargas](reports-monitoring/troubleshoot-missing-data-download.md)
#### [Errores de los paquetes de contenido de los registros de actividad de Azure AD](reports-monitoring/troubleshoot-content-pack.md)
#### [Errores de Reporting API de Azure AD](reports-monitoring/troubleshoot-graph-api.md)

### [Acceso mediante programación](reports-monitoring/concept-reporting-api.md)
#### [Requisitos previos](reports-monitoring/howto-configure-prerequisites-for-reporting-api.md)
#### [Uso de certificados](reports-monitoring/tutorial-access-api-with-certificates.md)

## [Administrar contraseñas](authentication/concept-sspr-howitworks.md)

## Administración de aplicaciones
### [Información general](manage-apps/what-is-application-management.md)
### [Introducción](manage-apps/plan-an-application-integration.md)
### [Tutoriales de integración de aplicaciones SaaS](saas-apps/tutorial-list.md)

### [Aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS](manage-apps/user-provisioning.md) 
#### [Tutoriales de integración de aplicaciones](saas-apps/tutorial-list.md) 
#### [Automatización del aprovisionamiento para aplicaciones habilitadas con SCIM](manage-apps/use-scim-to-provision-users-and-groups.md) 
#### [Personalización de asignaciones de atributos](manage-apps/customize-application-attributes.md) 
#### [Escritura de expresiones para la asignación de atributos](manage-apps/functions-for-customizing-application-data.md) 
#### [Uso de filtros de ámbito](manage-apps/define-conditional-rules-for-provisioning-user-accounts.md) 
#### [Notificación del aprovisionamiento automático de usuarios](manage-apps/check-status-user-account-provisioning.md) 
#### [Solución de problemas de aprovisionamiento de usuarios](active-directory-application-provisioning-content-map.md) 

### [Acceso remoto a las aplicaciones con el proxy de aplicación](manage-apps/application-proxy.md)
#### Introducción
##### [Habilitación del proxy de aplicación](manage-apps/application-proxy-enable.md)
##### [Publicación de aplicaciones](manage-apps/application-proxy-publish-azure-portal.md)
##### [Dominios personalizados](manage-apps/application-proxy-configure-custom-domain.md)
#### [Inicio de sesión único](manage-apps/application-proxy-single-sign-on.md)
##### [SSO con KCD](manage-apps/application-proxy-configure-single-sign-on-with-kcd.md)
##### [SSO con encabezados](manage-apps/application-proxy-configure-single-sign-on-with-ping-access.md)
##### [SSO con almacenamiento de contraseñas](manage-apps/application-proxy-configure-single-sign-on-password-vaulting.md)
#### Conceptos
##### [Conectores](manage-apps/application-proxy-connectors.md)
##### [Seguridad](manage-apps/application-proxy-security.md)
##### [Redes](manage-apps/application-proxy-network-topology.md)


##### [Actualización desde TMG o UAG](manage-apps/application-proxy-migration.md)

#### Configuraciones avanzadas
##### [Publicación en redes independientes](manage-apps/application-proxy-connector-groups.md)
##### [Servidores proxy](manage-apps/application-proxy-configure-connectors-with-proxy-servers.md)
##### [Aplicaciones compatibles con notificaciones](manage-apps/application-proxy-configure-for-claims-aware-applications.md)
##### [Aplicaciones de cliente nativo](manage-apps/application-proxy-configure-native-client-application.md)
##### [Instalación silenciosa](manage-apps/application-proxy-register-connector-powershell.md)
##### [Página de inicio personalizada](manage-apps/application-proxy-configure-custom-home-page.md)
##### [Traducción de vínculos insertados](manage-apps/application-proxy-configure-hard-coded-link-translation.md)
##### [Caracteres comodín](manage-apps/application-proxy-wildcard.md)
##### [Eliminación de datos personales](manage-apps/application-proxy-remove-personal-data.md)


#### Tutoriales de publicación
##### [Escritorio remoto](manage-apps/application-proxy-integrate-with-remote-desktop-services.md)
##### [SharePoint](manage-apps/application-proxy-integrate-with-sharepoint-server.md)
##### [Microsoft Teams](manage-apps/application-proxy-integrate-with-teams.md)
##### [Tableau](manage-apps/application-proxy-integrate-with-tableau.md)
##### [Qlik](manage-apps/application-proxy-qlik.md)
#### [PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#application_proxy_application_management) 

#### [Solución de problemas](manage-apps/application-proxy-troubleshoot.md)

### Administración de aplicaciones empresariales
#### [Agregar una aplicación](manage-apps/add-application-portal.md)
#### [Visualización de aplicaciones de inquilino](manage-apps/view-applications-portal.md)
#### [Configuración del inicio de sesión único](manage-apps/configure-single-sign-on-portal.md)
#### [Asignación de usuarios](manage-apps/assign-user-or-group-access-portal.md)
#### [Personalización de la información de marca](manage-apps/change-name-or-logo-portal.md)
#### [¿Cómo puedo deshabilitar los inicios de sesión de usuario?](manage-apps/disable-user-sign-in-portal.md)
#### [Eliminación de usuarios](manage-apps/remove-user-or-group-access-portal.md)

#### [Administración del aprovisionamiento de cuentas de usuario](manage-apps/configure-automatic-user-provisioning-portal.md)

#### [Firma avanzada de certificados para aplicaciones SAML](manage-apps/certificate-signing-options.md)
#### [Ocultar una aplicación de una experiencia de usuario](manage-apps/hide-application-from-user-portal.md)
### [Configuración de la aceleración automática del inicio de sesión mediante la directiva de HRD](manage-apps/configure-authentication-for-federated-users-portal.md)
### [Migración de aplicaciones de AD FS a Azure AD](manage-apps/migrate-adfs-apps-to-azure.md) 
### [Administración del acceso a aplicaciones](manage-apps/what-is-access-management.md)
#### [Acceso con SSO](manage-apps/what-is-single-sign-on.md)
#### [Certificados para SSO](manage-apps/manage-certificates-for-federated-single-sign-on.md)
#### [Restricciones de inquilino](manage-apps/tenant-restrictions.md)
#### [Uso de usuarios de aprovisionamiento de SCIM](manage-apps/use-scim-to-provision-users-and-groups.md)

### [Descripción de las experiencias de consentimiento de la aplicación de Azure AD](develop/application-consent-experience.md)

### Solución de problemas



#### Panel de acceso
##### [Aplicación que no aparece](manage-apps/access-panel-troubleshoot-application-not-appearing.md)
##### [Aparece una aplicación inesperada](manage-apps/access-panel-troubleshoot-unexpected-application.md)
##### [No se puede iniciar sesión](manage-apps/access-panel-troubleshoot-web-sign-in-problem.md)
##### [Error durante la instalación de la extensión del explorador](manage-apps/access-panel-extension-problem-installing.md)
##### [Uso del acceso de autoservicio a las aplicaciones](manage-apps/access-panel-manage-self-service-access.md)
##### [Error durante el uso del acceso de autoservicio a las aplicaciones](manage-apps/access-panel-troubleshoot-self-service-access.md)

#### Incorporación de una aplicación
##### [Elegir tipo de aplicación](manage-apps/choose-application-type.md)
##### [Problemas habituales: aplicaciones de la galería](manage-apps/adding-gallery-app-common-problems.md)
##### [Problemas habituales: aplicaciones que no son de la galería](manage-apps/adding-non-gallery-app-common-problems.md)

#### Proxy de aplicación
##### [Página de aplicación que muestra los problemas](manage-apps/application-proxy-page-appearance-broken-problem.md)
##### [La carga de la aplicación es demasiado larga](manage-apps/application-proxy-page-load-speed-problem.md)
##### [Los vínculos en la página de aplicación no funcionan](manage-apps/application-proxy-page-links-broken-problem.md)
##### [¿Qué puertos se deben abrir para mi aplicación?](manage-apps/application-proxy-connectivity-ports-how-to.md)
##### [No hay ningún conector funcionando en un grupo de conectores para mi aplicación](manage-apps/application-proxy-connectivity-no-working-connector.md)
##### [Configuración en el portal de administración](manage-apps/application-proxy-config-how-to.md)
##### [Configuración del inicio de sesión único en mi aplicación](manage-apps/application-proxy-config-sso-how-to.md)
##### [Problema al crear una aplicación en el portal de administración](manage-apps/application-proxy-config-problem.md)
##### [Configuración de la delegación limitada de Kerberos](manage-apps/application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
##### [Configuración con PingAccess](manage-apps/application-proxy-back-end-ping-access-how-to.md)
##### [Error "Can't Access this Corporate Application"](manage-apps/application-proxy-sign-in-bad-gateway-timeout-error.md)
##### [Problema al instalar el conector de agente del proxy de aplicación](manage-apps/application-proxy-connector-installation-problem.md)


#### Registro de la aplicación
##### [Especificar campos del objeto de aplicación](develop/registration-config-specific-application-property-how-to.md)
##### [Cambio de los valores predeterminados de vigencia del token](develop/registration-config-change-token-lifetime-how-to.md)

#### Autenticación
##### [Configuración de puntos de conexión](develop/registration-config-how-to.md)

#### Acceso condicional
##### [El cliente no cumplía los requisitos previos de registro de dispositivos](active-directory-conditional-access.md)
##### [¿Cómo y cuándo entran en vigor las reglas ajenas a la red corporativa?](https://aka.ms/calocation)
##### [¿Cómo aumentar el número de dispositivos que el usuario puede registrar en Azure AD?](active-directory-azureadjoin-setup.md)
##### [¿Cómo se configura el acceso condicional para Exchange Online](https://aka.ms/csforexchange)
##### [¿Cómo configurar el acceso condicional para dispositivos con Windows 7?](active-directory-conditional-access.md#device-based-conditional-access)
##### [¿Qué aplicaciones son compatibles con el acceso condicional?](active-directory-conditional-access-supported-apps.md)

#### Búsqueda de una API
##### [Búsqueda de una API](develop/api-find-an-api-how-to.md)

#### Administración del acceso
##### [Asignación de usuarios y grupos a una aplicación](manage-apps/methods-for-assigning-users-and-groups.md)
##### [Eliminación del acceso de un usuario a una aplicación](manage-apps/methods-for-removing-user-access.md)
##### [Configuración de la asignación de aplicaciones en modo autoservicio](manage-apps/manage-self-service-access.md)
##### [Se ha asignado un usuario inesperado](manage-apps/ways-users-get-assigned-to-applications.md)
##### [Hay una aplicación inesperada en la lista de aplicaciones](manage-apps/application-types.md)

#### Aplicaciones multiinquilino
##### [Configuración de una nueva aplicación](develop/setup-multi-tenant-app.md)
##### [Incorporación a la galería de aplicaciones](develop/registration-config-multi-tenant-application-add-to-gallery-how-to.md)

#### Permisos
##### [Elección de los permisos de una API](develop/perms-for-given-api.md)
##### [Permisos delegados frente a permisos de aplicaciones](develop/delegated-and-app-perms.md)
##### [Consentimiento de la aplicación](develop/consent-framework.md)

#### Aprovisionamiento
##### [¿Cuánto tiempo tarda?](manage-apps/application-provisioning-when-will-provisioning-finish-specific-user.md)
##### [Tarda horas: aplicación de la galería](manage-apps/application-provisioning-when-will-provisioning-finish.md)
##### [Configuración del aprovisionamiento de usuarios: aplicación de la galería](manage-apps/application-provisioning-config-how-to.md)
##### [Problema al configurar el aprovisionamiento de usuarios: aplicación de la galería](manage-apps/application-provisioning-config-problem.md)
##### [Problema al guardar las credenciales de administrador al configurar el aprovisionamiento de usuarios en una aplicación de la galería](manage-apps/application-provisioning-config-problem-storage-limit.md)
##### [Los usuarios no se aprovisionan: aplicación de la galería](manage-apps/application-provisioning-config-problem-no-users-provisioned.md)
##### [Se han aprovisionado usuarios incorrectos: aplicación de la galería](manage-apps/application-provisioning-config-problem-wrong-users-provisioned.md)

#### Inicio de sesión único
##### [Elección de un método](manage-apps/single-sign-on-modes.md)
##### [Configuración](develop/registration-config-sso-how-to.md)
##### [Configuración federada: aplicaciones de la galería](manage-apps/configure-federated-single-sign-on-gallery-applications.md)
##### [Problemas habituales en la configuración federada: aplicaciones de la galería](manage-apps/configure-federated-single-sign-on-gallery-applications-problems.md)
##### [Configuración federada: aplicaciones que no son de la galería](manage-apps/configure-federated-single-sign-on-non-gallery-applications.md)
##### [Problemas habituales en la configuración federada: aplicaciones que no son de la galería](manage-apps/configure-federated-single-sign-on-non-gallery-applications-problems.md)
##### [Configuración de contraseñas: aplicaciones de la galería](manage-apps/configure-password-single-sign-on-gallery-applications.md)
##### [Problemas habituales en la configuración de contraseñas: aplicaciones de la galería](manage-apps/configure-password-single-sign-on-gallery-applications-problems.md)
##### [Configuración de contraseñas: aplicaciones que no son de la galería](manage-apps/configure-password-single-sign-on-non-gallery-applications.md)
##### [Problemas habituales en la configuración de contraseñas: aplicaciones que no son de la galería](manage-apps/configure-password-single-sign-on-non-gallery-applications-problems.md)

#### Problemas de inicio de sesión de usuario
##### [Petición de consentimiento inesperada](manage-apps/application-sign-in-unexpected-user-consent-prompt.md)
##### [Error de consentimiento de usuario](manage-apps/application-sign-in-unexpected-user-consent-error.md)
##### [Problemas al iniciar sesión desde el portal personalizado](manage-apps/application-sign-in-other-problem-deeplink.md)
##### [Problemas al iniciar sesión desde el panel de acceso](manage-apps/application-sign-in-other-problem-access-panel.md)
##### [Error en la página de inicio de sesión de la aplicación](manage-apps/application-sign-in-problem-application-error.md)
##### [Problema con el inicio de sesión único con contraseña: aplicaciones que no son de la galería](manage-apps/application-sign-in-problem-password-sso-non-gallery.md)
##### [Problema con el inicio de sesión único con contraseña: aplicaciones de la galería](manage-apps/application-sign-in-problem-password-sso-gallery.md)
##### [Problema al iniciar sesión en una aplicación de Microsoft](manage-apps/application-sign-in-problem-first-party-microsoft.md)
##### [Problema con el inicio de sesión único federado: aplicaciones que no son de la galería](manage-apps/application-sign-in-problem-federated-sso-non-gallery.md)
##### [Problema con el inicio de sesión único federado: aplicaciones de la galería](manage-apps/application-sign-in-problem-federated-sso-gallery.md)
##### [Problema con una aplicación desarrollada de forma personalizada](manage-apps/application-sign-in-problem-custom-dev.md)
##### [Problema con una aplicación local: proxy de aplicación](manage-apps/application-sign-in-problem-on-premises-application-proxy.md)

### [Desarrollo de aplicaciones](active-directory-applications-guiding-developers-for-lob-applications.md)


## Administración del directorio
### [Azure AD Connect](hybrid/whatis-hybrid-identity.md)
### Nombres de dominio personalizados
#### [Guía de inicio rápido](fundamentals/add-custom-domain.md)
### [Administración de directorios](fundamentals/active-directory-administer.md)
### [Integración de identidades locales con Azure AD Connect](hybrid/whatis-hybrid-identity.md)

### [Configuración de la vigencia de los tokens](develop/active-directory-configurable-token-lifetimes.md)

## Protección de las identidades

### [Privileged Identity Management](privileged-identity-management/pim-configure.md?toc=%2fazure%2factive-directory%2ftoc.json)



## [Solución de problemas](fundamentals/active-directory-troubleshooting-support-howto.md)

## Implementación de la prueba de concepto (PoC) de Azure AD
### [Guía de la prueba de concepto: Introducción](active-directory-playbook-intro.md)
### [Guía de la prueba de concepto: Ingredientes](active-directory-playbook-ingredients.md)
### [Guía de la prueba de concepto: Implementación](active-directory-playbook-implementation.md)
### [Guía de la prueba de concepto: Bloques de creación](active-directory-playbook-building-blocks.md)

# Referencia
## [Ejemplos de código](https://azure.microsoft.com/resources/samples/?service=active-directory)
## [cmdlets de Azure PowerShell](/powershell/azure/overview)
## [Referencia de la API de Java](/java/api)
## [API de .NET](/active-directory/adal/microsoft.identitymodel.clients.activedirectory)

# Temas relacionados
## [Multi-Factor Authentication](/azure/multi-factor-authentication/)
## [Azure AD Connect](hybrid/whatis-hybrid-identity.md)
## [Azure AD Connect Health](hybrid/whatis-hybrid-identity-health.md)
## [Azure AD para desarrolladores](./develop/active-directory-how-to-integrate.md)
## [Azure AD Privileged Identity Management](./privileged-identity-management/pim-configure.md)

# Recursos
## [Planes de implementación de Azure AD](./fundamentals/active-directory-deployment-plans.md)
## [Foro de comentarios de Azure](https://feedback.azure.com/forums/169401-azure-active-directory)
## [Azure Roadmap](https://azure.microsoft.com/roadmap/?category=security-identity)
## [Foro de MSDN](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)
## [Precios](https://azure.microsoft.com/pricing/details/active-directory/)
## [Calculadora de precios](https://azure.microsoft.com/pricing/calculator/)
## [Actualizaciones del servicio](https://azure.microsoft.com/updates/?product=active-directory)
## [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory)
## [Vídeos](https://azure.microsoft.com/documentation/videos/index/?services=active-directory)
