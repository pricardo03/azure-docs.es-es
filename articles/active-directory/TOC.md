# [Documentación de Azure Active Directory](index.md)

# Información general
## [¿Qué es Azure Active Directory?](fundamentals/active-directory-whatis.md)
## [Acerca de la administración de identidades de Azure](fundamentals/identity-fundamentals.md)
## [Información acerca de las soluciones de identidades de Azure](fundamentals/understand-azure-identity-solutions.md)
## [Elección de una solución de identidad híbrida](choose-hybrid-identity-solution.md)
## [Asociación de suscripciones de Azure](fundamentals/active-directory-how-subscriptions-associated-directory.md)
## [Consideraciones sobre residencia y datos](fundamentals/active-directory-data-storage-eu.md)
## [Preguntas más frecuentes](fundamentals/active-directory-faq.md)
## [Novedades](fundamentals/whats-new.md)


# Introducción
## [Introducción a Azure AD](fundamentals/get-started-azure-ad.md)
## [Suscripción a Azure AD Premium](fundamentals/active-directory-get-started-premium.md)
## [Adición de un dominio personalizado](fundamentals/add-custom-domain.md)
## [Configuración de la personalización de marca de la compañía](fundamentals/customize-branding.md)
## [Adición de usuarios a Azure AD](fundamentals/add-users-azure-active-directory.md)
## [Asignación de licencias a usuarios](fundamentals/license-users-groups.md)
## [Configuración del autoservicio de restablecimiento de contraseña](authentication/quickstart-sspr.md)
## [Incorporación de información de privacidad de su organización en Azure AD](active-directory-properties-area.md)


# Procedimientos
## Planeamiento y diseño
### [Descripción de la arquitectura de Azure AD](fundamentals/active-directory-architecture.md)
### [Asignación de notificaciones en Azure Active Directory](active-directory-claims-mapping.md)
### [Implementación de una solución de identidad híbrida](active-directory-hybrid-identity-design-considerations-overview.md)
#### Determinación de requisitos
##### [Identidad](active-directory-hybrid-identity-design-considerations-business-needs.md)
##### [Sincronización de directorios](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)
##### [Multi-factor Authentication](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)
##### [Identificación de la estrategia de ciclo de vida](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md)
#### [Planeamiento de la seguridad de datos](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)
##### [Protección de datos](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
##### [Administración de contenido](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
##### [Control de acceso](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
##### [Respuesta a los incidentes](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)
#### Planeamiento del ciclo de vida de identidad
##### [Tareas](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)
##### [Estrategia de adopción](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)
#### [Pasos siguientes](active-directory-hybrid-identity-design-considerations-nextsteps.md)
#### [Comparación de herramientas](active-directory-hybrid-identity-design-considerations-tools-comparison.md)

## Administrar usuarios
### [Adición de nuevos usuarios a Azure AD](fundamentals/add-users-azure-active-directory.md)
### [Administrar perfiles de usuario](fundamentals/active-directory-users-profile-azure-portal.md)
### [Uso compartido de cuentas](active-directory-sharing-accounts.md)
### [Asignación de usuarios a roles de administración](fundamentals/active-directory-users-assign-role-azure-portal.md)
### [Restaurar un usuario eliminado](fundamentals/active-directory-users-restore.md)
### [Adición de usuarios invitados de otro directorio (B2B)](b2b/what-is-b2b.md)
#### [Adición de usuarios de B2B por los administradores](b2b/add-users-administrator.md)
#### [Adición de usuarios de B2B por los trabajadores de la información](b2b/add-users-information-worker.md)
#### [API y personalización](b2b/customize-invitation-api.md)
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
### Administrar grupos
#### [Azure Portal](fundamentals/active-directory-groups-create-azure-portal.md)
#### [Azure AD PowerShell for Graph (v2)](users-groups-roles/groups-settings-v2-cmdlets.md)
#### [Azure AD PowerShell MSOnline](users-groups-roles/groups-settings-cmdlets.md)
### [Administración de miembros del grupo](fundamentals/active-directory-groups-members-azure-portal.md)
### [Administración de propietarios de grupo](fundamentals/active-directory-accessmanagement-managing-group-owners.md)
### [Administración de pertenencia al grupo](fundamentals/active-directory-groups-membership-azure-portal.md)
### [Asignación de licencias mediante grupos](fundamentals/active-directory-licensing-whatis-azure-portal.md)
#### [Asignación de licencias a un grupo](users-groups-roles/licensing-groups-assign.md)
#### [Identificación y resolución de problemas de licencias de un grupo](users-groups-roles/licensing-groups-resolve-problems.md)
#### [Migración de usuarios individuales con licencia a licencias basadas en grupos](users-groups-roles/licensing-groups-migrate-users.md)
#### [Migración de usuarios entre licencias de productos](users-groups-roles/licensing-groups-change-licenses.md)
#### [Escenarios adicionales para licencias basadas en grupos](users-groups-roles/licensing-group-advanced.md)
#### [Ejemplos de Azure PowerShell para licencias basadas en grupos](users-groups-roles/licensing-ps-examples.md)
#### [Referencia de productos y planes de servicio en Azure AD](users-groups-roles/licensing-service-plan-reference.md)
### [Configuración de la expiración de los grupos de Office 365](users-groups-roles/groups-lifecycle.md)
### [Aplicación una directiva de nomenclatura para grupos](users-groups-roles/groups-naming-policy.md)
### [Visualización de todos los grupos](fundamentals/active-directory-groups-view-azure-portal.md)
### [Administración del acceso de grupo a aplicaciones SaaS](users-groups-roles/groups-saasapps.md)
### [Restauración de un grupo eliminado de Office 365](fundamentals/active-directory-groups-restore-azure-portal.md)
### [Administración de la configuración de grupo](fundamentals/active-directory-groups-settings-azure-portal.md) 
### Creación de reglas avanzadas
#### [Azure Portal](users-groups-roles/groups-dynamic-membership.md)
### [Configuración de grupos de autoservicio](users-groups-roles/groups-self-service-management.md)
### [Solución de problemas](users-groups-roles/groups-troubleshooting.md)

## [Administración de informes](active-directory-reporting-azure-portal.md)
### [Actividad de inicios de sesión](active-directory-reporting-activity-sign-ins.md)
### [Actividad de auditoría](active-directory-reporting-activity-audit-logs.md)
### [Usuarios en riesgo](active-directory-reporting-security-user-at-risk.md)
### [Inicios de sesión no seguros](active-directory-reporting-security-risky-sign-ins.md)
### [Eventos de riesgo](active-directory-reporting-risk-events.md)
### [P+F](active-directory-reporting-faq.md)
### Tareas
#### [Configuración de ubicaciones con nombre](active-directory-named-locations.md)
#### [Búsqueda de informes de actividad](active-directory-reporting-migration.md)
#### [Uso del paquete de contenido de Power BI de Azure Active Directory](active-directory-reporting-power-bi-content-pack-how-to.md)
#### [Corrección de usuarios marcados en riesgo](active-directory-report-security-user-at-risk-remediation.md)
### Referencia
#### [Retención](active-directory-reporting-retention.md)
#### [Latencias](active-directory-reporting-latencies-azure-portal.md)
#### [Referencia sobre actividades de auditoría](active-directory-reporting-activity-audit-reference.md)
#### [Códigos de error de la actividad de inicio de sesión](active-directory-reporting-activity-sign-ins-errors.md)
#### [Multi-Factor Authentication](active-directory-reporting-activity-sign-ins-mfa.md)


### Solución de problemas
#### [Datos de auditoría que faltan](active-directory-reporting-troubleshoot-missing-audit-data.md)
#### [Datos que faltan en las descargas](active-directory-reporting-troubleshoot-missing-data-download.md)
#### [Errores de los paquetes de contenido de los registros de actividad de Azure Active Directory](active-directory-reporting-troubleshoot-content-pack.md)
#### [Errores de la API de informes de Azure Active Directory](active-directory-reporting-troubleshoot-graph-api.md)


### [Acceso mediante programación](active-directory-reporting-api-getting-started-azure-portal.md)
#### [Requisitos previos](active-directory-reporting-api-prerequisites-azure-portal.md)
#### [Ejemplos de auditoría](active-directory-reporting-api-audit-samples.md)
#### [Ejemplos de inicio de sesión](active-directory-reporting-api-sign-in-activity-samples.md)
#### [Uso de certificados](active-directory-reporting-api-with-certificates.md)

## Administrar contraseñas
### [Introducción a las contraseñas](authentication/active-directory-passwords-overview.md)
### Documentos de usuario
#### [Restablecimiento o modificación de la contraseña](user-help/active-directory-passwords-update-your-own-password.md)
#### [Procedimientos recomendados sobre las contraseñas](active-directory-secure-passwords.md)
#### [Registro para el autoservicio de restablecimiento de contraseñas](user-help/active-directory-passwords-reset-register.md)
### [Vea cómo funciona SSPR](authentication/concept-sspr-howitworks.md)
### [Guía de implementación de SSPR](authentication/howto-sspr-deployment.md)
### [SSPR y Windows 10](authentication/tutorial-sspr-windows.md)
### [Directivas de SSPR](authentication/concept-sspr-policy.md)
### [Personalización de SSPR](authentication/concept-sspr-customization.md)
### [Requisitos de datos de SSPR](authentication/howto-sspr-authenticationdata.md)
### [Informes de SSPR](authentication/howto-sspr-reporting.md)
### [Bloqueo inteligente](authentication/howto-password-smart-lockout.md)
### [Eliminación de contraseñas no seguras](authentication/concept-password-ban-bad.md)
### [Configuración de la lista de contraseñas prohibidas](authentication/howto-password-ban-bad.md)
### [Integración local](authentication/concept-password-ban-bad-on-premises.md)
### [Implementación de la protección de contraseñas de Azure AD](authentication/howto-password-ban-bad-on-premises.md)
### [Configuración de la protección de contraseñas de Azure AD](authentication/howto-password-ban-bad-on-premises-operations.md)
### [Supervisión de la protección de contraseñas de Azure AD](authentication/howto-password-ban-bad-on-premises-troubleshoot.md)
### Administradores de TI: Restablecimiento de contraseñas
#### [Azure Portal](fundamentals/active-directory-users-reset-password-azure-portal.md)
### [Licencia de SSPR](authentication/concept-sspr-licensing.md)
### [Escritura diferida de contraseñas](authentication/howto-sspr-writeback.md)
### [Solución de problemas](authentication/active-directory-passwords-troubleshoot.md)
### [P+F](authentication/active-directory-passwords-faq.md)


## Administración de dispositivos
### [Introducción](device-management-introduction.md)
### [Uso de Azure Portal](device-management-azure-portal.md)
### [Planear Azure AD Join](active-directory-azureadjoin-deployment-aadjoindirect.md)
### [Preguntas más frecuentes](device-management-faq.md)
### Tareas
#### [Configuración de dispositivos con Windows 10 registrados en Azure AD](user-help/device-management-azuread-registered-devices-windows10-setup.md)
#### [Configuración de dispositivos unidos a Azure AD](user-help/device-management-azuread-joined-devices-setup.md)
#### [Configuración de dispositivos híbridos unidos a Azure AD](device-management-hybrid-azuread-joined-devices-setup.md)
#### [Implementación local](active-directory-device-registration-on-premises-setup.md)
#### [Combinación de Azure AD durante la primera ejecución de Windows 10](device-management-azuread-joined-devices-frx.md)
### Solución de problemas
#### [Dispositivos combinados híbridos de Azure AD para Windows 10 y Windows Server 2016](device-management-troubleshoot-hybrid-join-windows-current.md)
#### [Dispositivos combinados híbridos de Azure AD para Windows heredado](device-management-troubleshoot-hybrid-join-windows-legacy.md)

## Administración de aplicaciones
### [Información general](manage-apps/what-is-application-management.md)
### [Introducción](manage-apps/plan-an-application-integration.md)
### [Tutoriales de integración de aplicaciones SaaS](saas-apps/tutorial-list.md)

### [Aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS](active-directory-saas-app-provisioning.md) 
#### [Tutoriales de integración de aplicaciones](saas-apps/tutorial-list.md) 
#### [Automatización del aprovisionamiento para aplicaciones habilitadas con SCIM](manage-apps/use-scim-to-provision-users-and-groups.md) 
#### [Personalización de asignaciones de atributos](active-directory-saas-customizing-attribute-mappings.md) 
#### [Escritura de expresiones para la asignación de atributos](active-directory-saas-writing-expressions-for-attribute-mappings.md) 
#### [Uso de filtros de ámbito](active-directory-saas-scoping-filters.md) 
#### [Notificación del aprovisionamiento automático de usuarios](active-directory-saas-provisioning-reporting.md) 
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
##### [Caracteres comodín](active-directory-application-proxy-wildcard.md)
##### [Eliminación de datos personales](manage-apps/application-proxy-remove-personal-data.md)


#### Tutoriales de publicación
##### [Escritorio remoto](manage-apps/application-proxy-integrate-with-remote-desktop-services.md)
##### [SharePoint](manage-apps/application-proxy-integrate-with-sharepoint-server.md)
##### [Microsoft Teams](manage-apps/application-proxy-integrate-with-teams.md)
##### [Tableau](manage-apps/application-proxy-integrate-with-tableau.md)
##### [Qlik](active-directory-application-proxy-qlik.md)
#### [PowerShell](https://docs.microsoft.com/powershell/module/azuread/?view=azureadps-2.0#application_proxy_application_management) 

#### [Solución de problemas](manage-apps/application-proxy-troubleshoot.md)

### Administración de aplicaciones empresariales
#### [Asignación de usuarios](manage-apps/assign-user-or-group-access-portal.md)
#### [Personalización de la información de marca](manage-apps/change-name-or-logo-portal.md)
#### [¿Cómo puedo deshabilitar los inicios de sesión de usuario?](manage-apps/disable-user-sign-in-portal.md)
#### [Eliminación de usuarios](manage-apps/remove-user-or-group-access-portal.md)
#### [Visualización de todas mis aplicaciones](manage-apps/view-applications-portal.md)
#### [Administración del aprovisionamiento de cuentas de usuario](manage-apps/configure-automatic-user-provisioning-portal.md)
#### [Administración de inicio de sesión único para aplicaciones empresariales](manage-apps/configure-single-sign-on-portal.md)
#### [Firma avanzada de certificados para aplicaciones SAML](manage-apps/certificate-signing-options.md)
#### [Ocultar una aplicación de una experiencia de usuario](manage-apps/hide-application-from-user-portal.md)
### [Configuración de la aceleración automática del inicio de sesión mediante la directiva de HRD](manage-apps/configure-authentication-for-federated-users-portal.md)
### [Migración de aplicaciones de AD FS a Azure AD](manage-apps/migrate-adfs-apps-to-azure.md) 
### [Administración del acceso a aplicaciones](manage-apps/what-is-access-management.md)
#### [Acceso con SSO](manage-apps/what-is-single-sign-on.md)
#### [Certificados para SSO](manage-apps/manage-certificates-for-federated-single-sign-on.md)
#### [Restricciones de inquilino](manage-apps/tenant-restrictions.md)
#### [Uso de usuarios de aprovisionamiento de SCIM](manage-apps/use-scim-to-provision-users-and-groups.md)


### Solución de problemas



#### Panel de acceso
##### [Aplicación que no aparece](application-access-panel-unexpected-application-not-appearing.md)
##### [Aparece una aplicación inesperada](application-access-panel-unexpected-application-appears.md)
##### [No se puede iniciar sesión](application-access-panel-web-sign-in-problem.md)
##### [Error durante la instalación de la extensión del explorador](application-access-panel-extension-problem-installing.md)
##### [Uso del acceso de autoservicio a las aplicaciones](application-access-panel-self-service-applications-how-to.md)
##### [Error durante el uso del acceso de autoservicio a las aplicaciones](application-access-panel-self-service-applications-problem.md)

#### Incorporación de una aplicación
##### [Elegir tipo de aplicación](application-config-add-app-problem-how-to-choose-application-type.md)
##### [Problemas habituales: aplicaciones de la galería](application-config-add-app-problem-problem-adding-gallery-app.md)
##### [Problemas habituales: aplicaciones que no son de la galería](application-config-add-app-problem-problem-adding-non-gallery-app.md)

#### Proxy de aplicación
##### [Página de aplicación que muestra los problemas](application-proxy-page-appearance-broken-problem.md)
##### [La carga de la aplicación es demasiado larga](application-proxy-page-load-speed-problem.md)
##### [Los vínculos en la página de aplicación no funcionan](application-proxy-page-links-broken-problem.md)
##### [¿Qué puertos se deben abrir para mi aplicación?](application-proxy-connectivity-ports-how-to.md)
##### [No hay ningún conector funcionando en un grupo de conectores para mi aplicación](application-proxy-connectivity-no-working-connector.md)
##### [Configuración en el portal de administración](application-proxy-config-how-to.md)
##### [Configuración del inicio de sesión único en mi aplicación](application-proxy-config-sso-how-to.md)
##### [Problema al crear una aplicación en el portal de administración](application-proxy-config-problem.md)
##### [Configuración de la delegación limitada de Kerberos](application-proxy-back-end-kerberos-constrained-delegation-how-to.md)
##### [Configuración con PingAccess](application-proxy-back-end-ping-access-how-to.md)
##### [Error "Can't Access this Corporate Application"](application-proxy-sign-in-bad-gateway-timeout-error.md)
##### [Problema al instalar el conector de agente del proxy de aplicación](application-proxy-connector-installation-problem.md)


#### Registro de la aplicación
##### [Especificar campos del objeto de aplicación](application-dev-registration-config-specific-application-property-how-to.md)
##### [Cambio de los valores predeterminados de vigencia del token](application-dev-registration-config-change-token-lifetime-how-to.md)

#### Autenticación
##### [Configuración de puntos de conexión](application-dev-registration-config-how-to.md)

#### Acceso condicional
##### [El cliente no cumplía los requisitos previos de registro de dispositivos](active-directory-conditional-access.md)
##### [Se bloquea al inquilino debido a una configuración incorrecta de las directivas de acceso condicional](active-directory-conditional-access-device-remediation.md)
##### [¿Cómo y cuándo entran en vigor las reglas ajenas a la red corporativa?](https://aka.ms/calocation)
##### [¿Cómo aumentar el número de dispositivos que el usuario puede registrar en Azure AD?](active-directory-azureadjoin-setup.md)
##### [¿Cómo se configura el acceso condicional para Exchange Online](https://aka.ms/csforexchange)
##### [¿Cómo configurar el acceso condicional para dispositivos con Windows 7?](active-directory-conditional-access.md#device-based-conditional-access)
##### [¿Qué aplicaciones son compatibles con el acceso condicional?](active-directory-conditional-access-supported-apps.md)

#### Búsqueda de una API
##### [Búsqueda de una API](application-dev-api-find-an-api-how-to.md)

#### Administración del acceso
##### [Asignación de usuarios y grupos a una aplicación](application-access-assignment-how-to-add-assignment.md)
##### [Eliminación del acceso de un usuario a una aplicación](application-access-assignment-how-to-remove-assignment.md)
##### [Configuración de la asignación de aplicaciones en modo autoservicio](application-access-self-service-how-to.md)
##### [Se ha asignado un usuario inesperado](application-access-unexpected-user-assignment.md)
##### [Hay una aplicación inesperada en la lista de aplicaciones](application-access-unexpected-application.md)

#### Aplicaciones multiinquilino
##### [Configuración de una nueva aplicación](application-dev-setup-multi-tenant-app.md)
##### [Incorporación a la galería de aplicaciones](application-dev-registration-config-multi-tenant-application-add-to-gallery-how-to.md)

#### Permisos
##### [Elección de los permisos de una API](application-dev-perms-for-given-api.md)
##### [Concesión de permisos a mi aplicación](application-dev-registration-config-grant-permissions-how-to.md)
##### [Permisos delegados frente a permisos de aplicaciones](application-dev-delegated-and-app-perms.md)
##### [Consentimiento de la aplicación](application-dev-consent-framework.md)

#### Aprovisionamiento
##### [¿Cuánto tiempo tarda?](application-provisioning-when-will-provisioning-finish-specific-user.md)
##### [Tarda horas: aplicación de la galería](application-provisioning-when-will-provisioning-finish.md)
##### [Configuración del aprovisionamiento de usuarios: aplicación de la galería](application-provisioning-config-how-to.md)
##### [Problema al configurar el aprovisionamiento de usuarios: aplicación de la galería](application-provisioning-config-problem.md)
##### [Problema al guardar las credenciales de administrador al configurar el aprovisionamiento de usuarios en una aplicación de la galería](application-provisioning-config-problem-storage-limit.md)
##### [Los usuarios no se aprovisionan: aplicación de la galería](application-provisioning-config-problem-no-users-provisioned.md)
##### [Se han aprovisionado usuarios incorrectos: aplicación de la galería](application-provisioning-config-problem-wrong-users-provisioned.md)

#### Inicio de sesión único
##### [Elección de un método](application-config-sso-how-to-choose-sign-on-method.md)
##### [Configuración](application-dev-registration-config-sso-how-to.md)
##### [Configuración federada: aplicaciones de la galería](application-config-sso-how-to-configure-federated-sso-gallery.md)
##### [Problemas habituales en la configuración federada: aplicaciones de la galería](application-config-sso-problem-configure-federated-sso-gallery.md)
##### [Configuración federada: aplicaciones que no son de la galería](application-config-sso-how-to-configure-federated-sso-non-gallery.md)
##### [Problemas habituales en la configuración federada: aplicaciones que no son de la galería](application-config-sso-problem-configure-federated-sso-non-gallery.md)
##### [Configuración de contraseñas: aplicaciones de la galería](application-config-sso-how-to-configure-password-sso-gallery.md)
##### [Problemas habituales en la configuración de contraseñas: aplicaciones de la galería](application-config-sso-problem-configure-password-sso-gallery.md)
##### [Configuración de contraseñas: aplicaciones que no son de la galería](application-config-sso-how-to-configure-password-sso-non-gallery.md)
##### [Problemas habituales en la configuración de contraseñas: aplicaciones que no son de la galería](application-config-sso-problem-configure-password-sso-non-gallery.md)

#### Problemas de inicio de sesión de usuario
##### [Petición de consentimiento inesperada](application-sign-in-unexpected-user-consent-prompt.md)
##### [Error de consentimiento de usuario](application-sign-in-unexpected-user-consent-error.md)
##### [Problemas al iniciar sesión desde el portal personalizado](application-sign-in-other-problem-deeplink.md)
##### [Problemas al iniciar sesión desde el panel de acceso](application-sign-in-other-problem-access-panel.md)
##### [Error en la página de inicio de sesión de la aplicación](application-sign-in-problem-application-error.md)
##### [Problema con el inicio de sesión único con contraseña: aplicaciones que no son de la galería](application-sign-in-problem-password-sso-non-gallery.md)
##### [Problema con el inicio de sesión único con contraseña: aplicaciones de la galería](application-sign-in-problem-password-sso-gallery.md)
##### [Problema al iniciar sesión en una aplicación de Microsoft](application-sign-in-problem-first-party-microsoft.md)
##### [Problema con el inicio de sesión único federado: aplicaciones que no son de la galería](application-sign-in-problem-federated-sso-non-gallery.md)
##### [Problema con el inicio de sesión único federado: aplicaciones de la galería](application-sign-in-problem-federated-sso-gallery.md)
##### [Problema con una aplicación desarrollada de forma personalizada](application-sign-in-problem-custom-dev.md)
##### [Problema con una aplicación local: proxy de aplicación](application-sign-in-problem-on-premises-application-proxy.md)

### [Desarrollo de aplicaciones](active-directory-applications-guiding-developers-for-lob-applications.md)
### [Biblioteca de documentos](active-directory-apps-index.md)

## Administración del directorio
### [Azure AD Connect](./connect/active-directory-aadconnect.md)
### Nombres de dominio personalizados
#### [Guía de inicio rápido](fundamentals/add-custom-domain.md)
#### [Agregar nombres de dominio personalizados](users-groups-roles/domains-manage.md)
### [Administración de directorios](fundamentals/active-directory-administer.md)
### [Eliminación de un directorio](users-groups-roles/directory-delete-howto.md)
### [Varios directorios](users-groups-roles/licensing-directory-independence.md)
### [Suscripción de autoservicio](users-groups-roles/directory-self-service-signup.md)
### [Asumir el control de un directorio no administrado](users-groups-roles/domains-admin-takeover.md)
### [Enterprise State Roaming](active-directory-windows-enterprise-state-roaming-overview.md)
#### [Habilitación](active-directory-windows-enterprise-state-roaming-enable.md)
#### [Configuración de directiva de grupo](active-directory-windows-enterprise-state-roaming-group-policy-settings.md)
#### [Configuración de Windows 10](active-directory-windows-enterprise-state-roaming-windows-settings-reference.md)
#### [Preguntas más frecuentes](active-directory-windows-enterprise-state-roaming-faqs.md)
#### [Solución de problemas](active-directory-windows-enterprise-state-roaming-troubleshooting.md)


### [Integración de identidades locales con Azure AD Connect](./connect/active-directory-aadconnect.md)

## Administración del acceso a los recursos
### [Roles de administrador](users-groups-roles/directory-assign-admin-roles.md)
#### [Visualización de los miembros de un rol de administrador](users-groups-roles//directory-manage-roles-portal.md)
#### [Asignación de un rol de administrador a un usuario](fundamentals/active-directory-users-assign-role-azure-portal.md)
#### [Comparación de permisos para miembros y para usuarios invitados](fundamentals/users-default-permissions.md)
### [Seguridad del rol de administrador](users-groups-roles/directory-admin-roles-secure.md)  
#### [Creación de cuentas de administrador de acceso de emergencia](users-groups-roles/directory-emergency-access.md)
### [Unidades administrativas](users-groups-roles/directory-administrative-units.md)
### [Configuración de la vigencia de los tokens](active-directory-configurable-token-lifetimes.md)

## Revisiones de acceso
### [Introducción a las revisiones de acceso](active-directory-azure-ad-controls-access-reviews-overview.md)
### [Finalización de una revisión de acceso](active-directory-azure-ad-controls-complete-access-review.md)
### [Creación de una revisión de acceso](active-directory-azure-ad-controls-create-access-review.md)
### [Revisión de acceso en Privileged Identity Management de Azure AD](active-directory-azure-ad-controls-perform-access-review.md)
### [Revisión del acceso](active-directory-azure-ad-controls-how-to-review-your-access.md)
### [Acceso de invitado con revisiones de acceso](active-directory-azure-ad-controls-manage-guest-access-with-access-reviews.md)
### [Administración del acceso de usuarios con revisiones](active-directory-azure-ad-controls-manage-user-access-with-access-reviews.md)
### [Administración de programas y controles](active-directory-azure-ad-controls-manage-programs-controls.md)
### [Recuperación de los resultados de revisión de acceso](active-directory-azure-ad-controls-retrieve-access-review.md)

## Protección de las identidades
### [Acceso condicional](active-directory-conditional-access-azure-portal.md)
#### [Primeros pasos](active-directory-conditional-access-azure-portal-get-started.md)
#### Guías de inicio rápido
##### [Configuración de Multi-Factor Authentication para aplicaciones por cada nube](active-directory-conditional-access-app-based-mfa.md)
##### [Solicitar la aceptación de los términos de uso](active-directory-conditional-access-tou.md)
##### [Bloqueo del acceso si se detecta un riesgo de sesión](active-directory-conditional-access-app-sign-in-risk.md)
#### Tutoriales
##### [Migración de directiva de MFA clásica](active-directory-conditional-access-migration-mfa.md)
#### Conceptos
##### [Protección básica](active-directory-conditional-access-baseline-protection.md)
##### [Condiciones](active-directory-conditional-access-conditions.md)
##### [Condiciones de ubicación](active-directory-conditional-access-locations.md)
##### [Controles](active-directory-conditional-access-controls.md)
##### [Herramienta What if](active-directory-conditional-access-whatif.md)
##### [Descripción de directivas de dispositivo para servicios de Office 365](active-directory-conditional-access-device-policies.md)
#### Guías de procedimientos
##### [procedimientos recomendados](active-directory-conditional-access-best-practices.md)
##### [Configuración de las directivas de acceso condicionales para los intentos de acceso de redes que no son de confianza](active-directory-conditional-access-untrusted-networks.md)
##### [Configuración del acceso condicional basado en dispositivos](active-directory-conditional-access-policy-connected-applications.md)
##### [Configuración del acceso condicional basado en aplicaciones](active-directory-conditional-access-mam.md)
##### [Proporcionar condiciones de uso para los usuarios y las aplicaciones](active-directory-tou.md)
##### [Migración de directivas clásicas](active-directory-conditional-access-migration.md)
##### [Configuración de la conectividad VPN](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/always-on-vpn-deploy)
##### [Configuración de SharePoint y Exchange Online](active-directory-conditional-access-no-modern-authentication.md)
##### [Corrección](active-directory-conditional-access-device-remediation.md)
#### [Referencia técnica](active-directory-conditional-access-technical-reference.md)
#### [Preguntas más frecuentes](active-directory-conditional-faqs.md)

### Autenticación basada en certificados
#### [Android](active-directory-certificate-based-authentication-android.md)
#### [iOS](active-directory-certificate-based-authentication-ios.md)
#### [Primeros pasos](active-directory-certificate-based-authentication-get-started.md)

### [Azure AD Identity Protection](active-directory-identityprotection.md)
#### [Habilitación](active-directory-identityprotection-enable.md)
#### [Detección de vulnerabilidades](active-directory-identityprotection-vulnerabilities.md)
#### [Eventos de riesgo](active-directory-identity-protection-risk-events.md)
#### [Notificaciones](active-directory-identityprotection-notifications.md)
#### [Experiencia de inicio de sesión](active-directory-identityprotection-flows.md)
#### [Simulación de eventos de riesgo](active-directory-identityprotection-playbook.md)
#### [Desbloqueo de usuarios](active-directory-identityprotection-unblock-howto.md)
#### [Preguntas más frecuentes](active-directory-identity-protection-faqs.md)
#### [Glosario](active-directory-identityprotection-glossary.md)
#### [Microsoft Graph](active-directory-identityprotection-graph-getting-started.md)
### [Privileged Identity Management](privileged-identity-management/pim-configure.md?toc=%2fazure%2factive-directory%2ftoc.json)

## Integración de otros servicios con Azure AD 
### [Integración de LinkedIn con Azure AD](users-groups-roles/linkedin-integration.md)

## [Implementación de AD FS en Azure](active-directory-aadconnect-azure-adfs.md)
### [Alta disponibilidad](active-directory-adfs-in-azure-with-azure-traffic-manager.md)
### [Cambio de algoritmo hash de firma](active-directory-federation-sha256-guidance.md)

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
## [Restricciones y límites del servicio](users-groups-roles/directory-service-limits-restrictions.md)

# Temas relacionados
## [Multi-Factor Authentication](/azure/multi-factor-authentication/)
## [Azure AD Connect](./connect/active-directory-aadconnect.md)
## [Azure AD Connect Health](./connect-health/active-directory-aadconnect-health.md)
## [Azure AD para desarrolladores](./develop/active-directory-how-to-integrate.md)
## [Administración de identidades con privilegios de Azure AD](./privileged-identity-management/pim-configure.md)

# Recursos
## [Foro de comentarios de Azure](https://feedback.azure.com/forums/169401-azure-active-directory)
## [Azure Roadmap](https://azure.microsoft.com/roadmap/?category=security-identity)
## [Foro de MSDN](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)
## [Precios](https://azure.microsoft.com/pricing/details/active-directory/)
## [Calculadora de precios](https://azure.microsoft.com/pricing/calculator/)
## [Actualizaciones del servicio](https://azure.microsoft.com/updates/?product=active-directory)
## [Desbordamiento de la pila](http://stackoverflow.com/questions/tagged/azure-active-directory)
## [Vídeos](https://azure.microsoft.com/documentation/videos/index/?services=active-directory)
