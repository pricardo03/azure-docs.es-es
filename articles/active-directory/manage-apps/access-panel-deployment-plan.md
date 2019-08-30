---
title: Planeación de la implementación de un panel de acceso de Azure Active Directory
description: Guía sobre la implementación de la funcionalidad del panel de acceso de Azure AD
services: active-directory
author: barbaraselden
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/16/2019
ms.author: baselden
ms.reviewer: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: 448af7075297c2b18df1eceaeaac34d5da762489
ms.sourcegitcommit: 5ded08785546f4a687c2f76b2b871bbe802e7dae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/19/2019
ms.locfileid: "69575967"
---
# <a name="plan-an-azure-active-directory-access-panel-deployment"></a>Planeación de la implementación de un panel de acceso de Azure Active Directory

El panel de acceso de Azure Active Directory es un portal basado en web que le permite reducir los costos de soporte técnico, aumentar la productividad y la seguridad, y reducir la frustración de los usuarios. El sistema incluye informes detallados de seguimiento del acceso de los usuarios al sistema y envía notificaciones de alerta a los administradores ante posibles abusos o usos indebidos.

El panel de acceso de Azure Active Directory permite a los usuarios:

* Detectar y acceder a todos los recursos de su empresa conectados a Azure Active Directory como, por ejemplo, a las aplicaciones.
* Solicitar acceso a nuevas aplicaciones y grupos, o administrar el acceso a estos recursos para otros usuarios.
* Administrar el autoservicio de restablecimiento de contraseña y la configuración de la autenticación multifactor.
* Administrar sus dispositivos.

También permite a los administradores administrar:

* Los términos del servicio
* Las organizaciones
* Revisiones de acceso


## <a name="benefits-of-azure-active-directory-access-panel-integration"></a>Ventajas de la integración del panel de acceso de Azure Active Directory

El panel de acceso de Microsoft Azure Active Directory (Azure AD) ofrece las siguientes ventajas a las empresas:

**Proporciona una experiencia de usuario intuitiva**: La habilitación del panel de acceso proporciona a los usuarios la posibilidad de tener un solo panel de inicio para todas las aplicaciones conectadas de inicio de sesión único de Azure. A medida que se agreguen funcionalidades como la administración de grupos y el autoservicio de restablecimiento de contraseña, los usuarios seguirán teniendo un portal unificado para encontrar estos valores. La experiencia intuitiva permitirá a los usuarios trabajar más rápido y ser más productivos, a la vez que se reducirá su frustración.

**Aumenta la productividad**: Todas las aplicaciones de usuario del panel de acceso tienen habilitado el inicio de sesión único. Habilitar el inicio de sesión único en las aplicaciones empresariales y en Office 365 ofrece una experiencia de inicio de sesión superior para los usuarios existentes con la reducción o eliminación de los mensajes de inicio de sesión adicionales. El panel de acceso habilita la pertenencia dinámica y de autoservicio, y mejora la seguridad global del sistema de identidades asegurándose de que los usuarios adecuados administran el acceso a las aplicaciones. El panel de acceso sirve como una página de aterrizaje coherente para que un usuario encuentre rápidamente los recursos y continúe con las tareas del trabajo.

**Administra los costos**: La habilitación del panel de acceso con Azure Active Directory puede permitir la retirada de las inversiones en infraestructuras locales. Reduce los costos de soporte técnico al permitir a los usuarios tener un portal coherente para encontrar todas sus aplicaciones, solicitar acceso a los recursos y administrar las cuentas.

**Aumenta la flexibilidad y la seguridad**: El panel de acceso le permite acceder a la seguridad y la flexibilidad que proporciona una plataforma en la nube. Los administradores pueden cambiar fácilmente la configuración de las aplicaciones y los recursos, y adaptarse a los nuevos requisitos de seguridad sin que ello afecte a los usuarios.

**Permite una eficaz auditoría y seguimiento del uso**: La auditoría y el seguimiento de uso de todas las funcionalidades del usuario final le permiten saber cuándo los usuarios usan sus recursos y le garantizan la evaluación de la seguridad.

### <a name="licensing-considerations"></a>Consideraciones acerca de las licencias

El panel de acceso es gratuito y no requiere ninguna licencia para su uso en un nivel básico para todos los usuarios. Pero es posible que el número de objetos en el directorio y las características que quiere implementar requieran licencias adicionales. Entre los escenarios de Azure AD más habituales se incluyen las siguientes características de seguridad que tienen requisitos de licencia.

* [Azure Multi-Factor Authentication](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication-how-it-works)

* [Pertenencia basada en grupos](https://docs.microsoft.com/azure/active-directory/active-directory-manage-groups)

* [Requisito de autoservicio de restablecimiento de contraseña](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr)

* [Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)

Consulte la [guía de licencias completa para Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

### <a name="prerequisites-for-deploying-the-azure-ad-access-panel"></a>Requisitos previos para la implementación del panel de acceso de Azure AD

Los siguientes requisitos previos deben completarse antes del inicio de este proyecto.

* [Integración del inicio de sesión único de las aplicaciones](https://docs.microsoft.com/azure/active-directory/manage-apps/plan-sso-deployment)

* [Infraestructura de usuarios y grupos de Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/active-directory-manage-groups)

## <a name="plan-the-azure-ad-access-panel-deployment"></a>Planeación de la implementación del panel de acceso de Azure AD

En la tabla siguiente se describen los casos de uso principales de la implementación de un panel de acceso:

| Ámbito| DESCRIPCIÓN |
| - | - |
| Access| El portal del panel de acceso es accesible desde dispositivos corporativos y personales dentro de la red corporativa. |
|Access | El portal del panel de acceso es accesible desde dispositivos corporativos fuera de la red corporativa. |
| Auditoría| Los datos de uso se descargan en sistemas corporativos al menos cada 29 días. |
| Gobernanza| El ciclo de vida de las asignaciones de usuarios a aplicaciones y grupos conectados de Azure AD se define y se supervisa. |
| Seguridad| El acceso a los recursos se controla a través de asignaciones de usuarios y grupos. Solo los usuarios autorizados pueden administrar el acceso a los recursos. |
| Rendimiento| Las escalas de tiempo de propagación de asignación de acceso se documentan y supervisan. |
| Experiencia del usuario| Los usuarios son conscientes de las funcionalidades del panel de acceso y de cómo usarlas.|
| Experiencia del usuario| Los usuarios pueden administrar automáticamente el acceso a las aplicaciones y grupos.|
| Experiencia del usuario| Los usuarios pueden administrar sus cuentas. |
| Experiencia del usuario| Los usuarios son conscientes de la compatibilidad con el explorador. |
| Soporte técnico| Los usuarios pueden encontrar soporte técnico para los problemas del panel de acceso. |

### <a name="best-practices-for-deploying-the-azure-ad-access-panel"></a>Procedimientos recomendados para la implementación del panel de acceso de Azure AD

La funcionalidad del panel de acceso se puede habilitar gradualmente. Se recomienda el siguiente orden de implementación:

1. Mis aplicaciones
   * Iniciador de aplicaciones
   * Autoservicio de administración de aplicaciones
   * Integración con Microsoft Office 365

1. Autoservicio de detección de aplicaciones
   * Restablecimiento de la contraseña de autoservicio
   * Configuración de Multi-Factor Authentication
   * Administración de dispositivos
   * Términos de uso
   * Administración de organizaciones

1. Mis grupos
   * Self-service group management (Administración de grupos de autoservicio)
1. Revisiones de acceso
   * Administración de la revisión de acceso

Empezando en Aplicaciones, se presenta a los usuarios el portal como lugar común para el acceso a los recursos. La adición del autoservicio de detección de aplicaciones se crea a partir de la experiencia de Aplicaciones. Mis grupos y las revisiones de acceso se crean en las funcionalidades de autoservicio.

### <a name="plan-configurations-for-the-azure-ad-access-panel"></a>Planeación de configuraciones para el panel de acceso de Azure AD

En la tabla siguiente se enumeran varias configuraciones importantes del panel de acceso y los valores típicos que se pueden usar:

| Configuración| Valores típicos |
| - | - |
| Determinar los grupos piloto| Identificar el grupo de seguridad de Azure AD que se va a utilizar y asegurarse de que todos los miembros piloto forman parte del grupo |
| Determinar los grupos que se van a habilitar para producción| Identificar los grupos de seguridad de Azure AD, o los grupos de AD sincronizados con Azure AD, que se van a usar Asegúrese de que todos los miembros piloto forman parte del grupo. |
| Permitir a los usuarios usar el inicio de sesión único para qué tipo de aplicaciones| Inicio de sesión único federado, OAuth, inicio de sesión único con contraseña, proxy de aplicación |
| Permitir a los usuarios el uso del autoservicio de restablecimiento de contraseña| Sí |
| Permitir a los usuarios usar la autenticación multifactor| Sí |
| Permitir a los usuarios usar la administración de grupos de autoservicio para qué tipos de grupos| Grupos de seguridad, grupos de O365 |
| Permitir a los usuarios usar la administración de aplicaciones de autoservicio| Sí |
| Permitir a los usuarios usar revisiones de acceso| Sí |

### <a name="plan-consent-strategy"></a>Planeación de la estrategia de consentimiento

Los usuarios o administradores deben dar su consentimiento a los términos de uso y a las directivas de privacidad de todas las aplicaciones. Si sus reglas de negocio lo permiten, se recomienda usar el consentimiento del administrador, ya que ofrece a los usuarios una mejor experiencia.

Para usar el consentimiento del administrador, debe ser un administrador global del inquilino y las aplicaciones deben:

* está registrada en su inquilino; o bien,

* estar registradas en otro inquilino de Azure AD y haber recibido previamente el consentimiento, como mínimo, de un usuario final.

Para más información, consulte [Configuración del modo en que los usuarios finales dan su consentimiento a una aplicación en Azure Active Directory](configure-user-consent.md).

### <a name="engage-the-right-stakeholders"></a>Interactuar con las partes interesadas adecuadas

Cuando fracasan los proyectos tecnológicos, normalmente se debe a expectativas incorrectas relacionadas con el impacto, los resultados y las responsabilidades. Para evitar estos problemas, [asegúrese de que involucra a las partes interesadas adecuadas](../fundamentals/active-directory-deployment-plans.md) y que estas conocen bien sus roles.

### <a name="plan-communications"></a>Planeamiento de las comunicaciones

La comunicación es fundamental para el éxito de cualquier servicio nuevo. Informe de forma proactiva a los usuarios sobre cómo y cuándo cambiará su experiencia y cómo obtener soporte técnico si es necesario.

Aunque el panel de acceso no suele dar problemas al usuario, es importante estar preparado. Cree guías y una lista de todos los recursos para el personal de soporte técnico antes de su lanzamiento.

#### <a name="communications-templates"></a>Plantillas de comunicaciones

Microsoft proporciona [plantillas personalizables para correos electrónicos y otras comunicaciones](https://aka.ms/APTemplates) del panel de acceso. Puede adaptar estos recursos para su uso en otros canales de comunicación, como resulte adecuado para su cultura corporativa.

## <a name="plan-your-sso-configuration"></a>Planeación de la configuración del inicio de sesión único

Cuando un usuario inicia sesión en una aplicación, pasa por un proceso de autenticación en el que debe demostrar que es quien dice ser. Sin inicio de sesión único, se almacena una contraseña en la aplicación y el usuario debe conocer esta contraseña. Con el inicio de sesión único, las credenciales de usuario se pasan a la aplicación por lo que no es necesario que vuelvan a escribirlas para cada aplicación.

Para iniciar aplicaciones en Aplicaciones, el inicio de sesión único (SSO) debe estar habilitado para las aplicaciones.

Azure AD ofrece tres métodos diferentes para habilitar el [inicio de sesión único en las aplicaciones](what-is-single-sign-on.md):

* **Inicio de sesión único federado** 
    * Permite que una aplicación redirija a Azure AD para la autenticación del usuario en lugar de solicitar una contraseña. 
    * Este sistema se admite para las aplicaciones que usan protocolos como SAML 2.0, WS-Federation u OpenID Connect, y es el modo más enriquecido de inicio de sesión único.

* **Inicio de sesión único con contraseña** 
    * Permite el almacenamiento seguro de contraseñas de las aplicaciones y la reproducción mediante una extensión de explorador web o aplicación móvil. 
    * Este método usa el proceso de inicio de sesión existente proporcionado por la aplicación, pero permite que un administrador administre las contraseñas. No es necesario que el usuario conozca la contraseña.

* **Inicio de sesión único existente** 
    * Permite a Azure AD utilizar cualquier inicio de sesión único existente que se haya configurado para la aplicación.
    * Permite que estas aplicaciones se vinculen a los portales de Office 365 o del panel de acceso de Azure AD. 
    * Permite informes adicionales en Azure AD cuando las aplicaciones se inician allí. 
    * Incluye el uso de Azure Application Proxy y el modo de inicio de sesión único vinculado.

Aprenda a configurar el modo de inicio de sesión único de una aplicación aquí: [Inicio de sesión único en aplicaciones en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis).

Para disfrutar de la mejor experiencia con la página Aplicaciones, se recomienda empezar por la integración de las aplicaciones en la nube disponibles para el inicio de sesión único federado. El inicio de sesión único federado permite a los usuarios tener una experiencia coherente de un solo clic en las superficies de inicio de la aplicación y tiende a ser más eficaz en el control de la configuración.

Use el inicio de sesión único federado con Azure AD (OpenID Connect o SAML) cuando una aplicación lo admita, en lugar del inicio de sesión único basado en contraseña y ADFS.

Para más información sobre cómo implementar y configurar aplicaciones SaaS, consulte el [plan de implementación del inicio de sesión único de SaaS](https://aka.ms/deploymentplans/sso).

#### <a name="plan-to-deploy-the-my-apps-browser-extension"></a>Planeación de la implementación de la extensión del explorador de Aplicaciones

Si las aplicaciones de inicio de sesión único basadas en contraseña se ponen a disposición de los usuarios finales, estos deben instalar la extensión de inicio de sesión seguro en Aplicaciones para iniciar sesión. La extensión ejecuta un script que transmite la contraseña en el formulario de inicio de sesión de la aplicación. Se solicitará a los usuarios que instalen la extensión la primera vez que inicien la aplicación de inicio de sesión único basado en contraseñas. Puede encontrar más información sobre la extensión en nuestra documentación sobre la [instalación de la extensión del explorador del panel de acceso](https://docs.microsoft.com/azure/active-directory/application-access-panel-extension-problem-installing?/?WT.mc_id=DMC_AAD_Manage_Apps_Troubleshooting_Nav).

Si debe integrar aplicaciones de inicio de sesión único basado en contraseñas, deberá definir un mecanismo para implementar la extensión a escala con [exploradores compatibles](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction). Las opciones incluyen:

* [Directiva de grupo para Internet Explorer](https://azure.microsoft.com/documentation/articles/active-directory-saas-ie-group-policy/)

* [System Center Configuration Manager (SCCM) para Internet Explorer](https://docs.microsoft.com/sccm/core/clients/deploy/deploy-clients-to-windows-computers)

* [Descarga y configuración de Chrome, Firefox, Microsoft Edge o IE controlada por el usuario](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

Más información: [Configuración del inicio de sesión único con contraseña](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-password-sso-non-gallery).

Los usuarios que no usan aplicaciones de inicio de sesión único basado en contraseñas también se benefician de la extensión. Entre las ventajas se incluyen la posibilidad de iniciar cualquier aplicación desde su barra de búsqueda, encontrar acceso a las aplicaciones usadas recientemente y tener un vínculo a la página Aplicaciones.

Esto es lo que el usuario verá al iniciar una aplicación de inicio de sesión único basado en contraseñas por primera vez:

![Captura de pantalla de la pantalla de instalación de la extensión del explorador de Aplicaciones ](media/access-panel-deployment-plan/ap-dp-sign-in.png)

#### <a name="plan-for-mobile-access"></a>Planeación del acceso móvil

Se necesitará un explorador protegido con una directiva de Intune (Microsoft Edge o Intune Managed Browser) para que los usuarios móviles inicien aplicaciones de inicio de sesión único basado en contraseñas. Un explorador protegido por directivas permite la transferencia de la contraseña guardada para la aplicación. Microsoft Edge o Managed Browser proporcionan un conjunto de características de protección de datos web. También puede usar Microsoft Edge para escenarios empresariales en dispositivos iOS y Android. Microsoft Edge admite los mismos escenarios de administración que Intune Managed Browser y mejora la experiencia del usuario final. Más información: [Administración del acceso web mediante un explorador protegido por directivas de Microsoft Intune](https://docs.microsoft.com/intune/app-configuration-managed-browser)

## <a name="plan-your-my-apps-deployment"></a>Planeación de la implementación de la página Aplicaciones

La base del panel de acceso es el iniciador de aplicaciones denominado Aplicaciones, al que los usuarios acceden en [https://myapps.microsoft.com](https://myapps.microsoft.com/). La página Aplicaciones proporciona a los usuarios un único lugar desde el que iniciar el trabajo y utilizar las aplicaciones necesarias. En ella, los usuarios encontrarán una lista de todas las aplicaciones a las que tienen acceso de inicio de sesión único. 

![Captura de pantalla del panel de aplicaciones](media/access-panel-deployment-plan/ap-dp-app-launcher.png)

Las mismas aplicaciones aparecerán en el iniciador de aplicaciones de Office 365 cuando los usuarios usen el portal de este.

Planee el orden en el que va a agregar las aplicaciones al iniciador de la página Aplicaciones y si las va a distribuir gradualmente o todas a la vez. Para ello, cree un inventario de aplicaciones que muestre el tipo de autenticación y cualquier integración existente de inicio de sesión único para cada aplicación.

#### <a name="add-applications-to-the-my-apps-panel"></a>Adición de aplicaciones al panel Aplicaciones

Se puede agregar cualquier aplicación habilitada para el inicio de sesión único de Azure AD al panel Aplicaciones. Otras aplicaciones se agregan mediante la opción de inicio de sesión único vinculado. Puede configurar un icono de la aplicación que se vincule a la dirección URL de la aplicación web existente. El inicio de sesión único vinculado le permite empezar a dirigir a los usuarios al portal Aplicaciones sin tener que migrar todas las aplicaciones al inicio de sesión único de Azure Active Directory. Puede moverse gradualmente a aplicaciones configuradas con inicio de sesión único de Azure AD sin interrumpir la experiencia de los usuarios.

#### <a name="plan-whether-to-use-my-apps-or-an-existing-portal"></a>Decidir si utilizar Aplicaciones o un portal ya existente

Es posible que los usuarios ya dispongan de una aplicación o portal distinto de Aplicaciones. Si es así, decida si desea admitir ambos portales o si solo va a utilizar uno.

Si ya se está usando un portal existente como punto de partida para los usuarios, puede integrar la funcionalidad Aplicaciones mediante el uso de "direcciones URL de acceso de usuarios". Los usuarios acceden a la función de direcciones URL como vínculos directos a las aplicaciones disponibles en el portal Aplicaciones. Estas direcciones URL se pueden insertar en cualquier sitio web existente. Cuando un usuario hace clic en el vínculo, este inicia la aplicación desde el portal Aplicaciones.

Puede encontrar la propiedad URL de acceso de usuario en el área Propiedades de la aplicación en Azure Portal.

![Captura de pantalla del panel de aplicaciones](media/access-panel-deployment-plan/ap-dp-user-access-url.png)


## <a name="plan-self-service-application-discovery-and-access"></a>Planeación de la detección y el acceso a aplicaciones de autoservicio

Una vez que se implementa un conjunto básico de aplicaciones en la página Aplicaciones de un usuario, se recomienda habilitar las características de autoservicio para la administración de aplicaciones. El autoservicio de detección de aplicaciones permite:
* A los usuarios encontrar nuevas aplicaciones que pueden agregar a su portal Aplicaciones. 
* A los usuarios agregar aplicaciones opcionales que no sabía que necesitaban durante la configuración.

Hay flujos de trabajo de aprobación disponibles para la aprobación explícita para acceder a las aplicaciones. Los usuarios que sean aprobadores recibirán notificaciones en el portal Aplicaciones cuando haya solicitudes pendientes de acceso a la aplicación.

## <a name="plan-self-service-group-membership"></a>Planeación de la pertenencia a grupos de autoservicio 

Puede permitir que los usuarios creen y administren sus propios grupos de seguridad o grupos de Office 365 en Azure Active Directory (Azure AD). El propietario del grupo puede aprobar o rechazar solicitudes de pertenencia y delegar el control de la pertenencia a grupos. Las características de administración de grupos de autoservicio no están disponibles para grupos de seguridad habilitados para correo electrónico o listas de distribución.

Para planear la pertenencia a grupos de autoservicio, determine si va a permitir que todos los usuarios de su organización puedan crear y administrar grupos, o solo un subconjunto de usuarios. Si solo lo va a permitir a un subconjunto de usuarios, deberá configurar un grupo al que se agreguen esas personas. Consulte [Configuración de la administración de grupos de autoservicio en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-self-service-management) para más información sobre cómo habilitar estos escenarios.

## <a name="plan-reporting-and-auditing"></a>Planeación de informes y auditoría

Azure AD proporciona [informes que ofrecen datos técnicos y empresariales](https://azure.microsoft.com/documentation/articles/active-directory-view-access-usage-reports/). Colabore con los propietarios de las aplicaciones empresariales y técnicas para que asuman la propiedad de los informes y los usen de forma periódica. En la tabla siguiente se proporcionan algunos ejemplos de escenarios de informes típicos.

|   | Administración de los riesgos.| Aumento de la productividad| Gobernanza y cumplimiento |
|  - |- | - | - |
| Tipos de informes|  Permisos y uso de la aplicación.| Actividad de aprovisionamiento de cuentas| Revise quién accede a las aplicaciones |
| Acciones posibles| Auditar el acceso; revocar los permisos| Corregir los errores de aprovisionamiento| Revocar el acceso |

Azure AD mantiene la mayoría de los datos de auditoría durante 30 días. Los datos están disponibles a través del portal de administración de Azure o de la API para que pueda descargarlos en los sistemas de análisis.

#### <a name="auditing"></a>Auditoría

Los registros de auditoría para el acceso a las aplicaciones estarán disponibles durante 30 días. Si la auditoría de seguridad de su empresa requiere una retención más prolongada, los registros deberán exportarse en una herramienta de Administración de eventos e información de seguridad (SIEM), como Splunk o ArcSight.

Para las copias de seguridad de auditoría, informes y recuperación ante desastres, documente la frecuencia necesaria de descarga, cuál es el sistema de destino y quién es el responsable de administrar cada copia de seguridad. Es posible que no necesite copias de seguridad independientes de auditoría e informes. La copia de seguridad de recuperación ante desastres debe ser una entidad independiente.

## <a name="deploy-applications-to-users-my-apps-panel"></a>Implementación de aplicaciones en el panel Aplicaciones de los usuarios

Después de configurar una aplicación para el inicio de sesión único, se asigna acceso a los grupos. Los usuarios de los grupos asignados tendrán acceso y verán la aplicación en su portal Aplicaciones y en el iniciador de aplicaciones de Office 365

Consulte [Asignación de usuarios y grupos en una aplicación de Active Directory](methods-for-assigning-users-and-groups.md).

Si durante las pruebas o la implementación quiere agregar los grupos, pero no permitir todavía que las aplicaciones aparezcan en Aplicaciones, consulte [Ocultación de una aplicación de la experiencia del usuario en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app).

### <a name="deploy-microsoft-office-365-applications-to-my-apps"></a>Implementación de aplicaciones Microsoft Office 365 en Aplicaciones

En el caso de aplicaciones de Office 365, los usuarios recibirán una copia de Office basada en las licencias que se les hayan asignado. Un requisito previo para el acceso a las aplicaciones de Office es la asignación de usuarios a las licencias correctas asociadas a las aplicaciones de Office. Al asignar a un usuario la licencia, verá automáticamente las aplicaciones asociadas a ella en la página Aplicaciones y en el iniciador de aplicaciones de O365.

Si desea ocultar un conjunto de aplicaciones de Office a los usuarios, hay una opción para ocultar aplicaciones en el portal Aplicaciones y, al mismo tiempo, permitir el acceso desde el portal de O365. Busque esta configuración en la sección de configuración de usuario de la aplicación. Más información: [Ocultación de una aplicación de la experiencia del usuario en Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-hide-third-party-app)

![Captura de pantalla de la configuración para ocultar aplicaciones](media/access-panel-deployment-plan/ap-dp-o365-portal.png)

### <a name="deploy-application-self-service-capabilities"></a>Implementación de funcionalidades de autoservicio de aplicaciones

El acceso de autoservicio a las aplicaciones permite a los usuarios detectar por sí mismos y solicitar acceso a las aplicaciones. A los usuarios se les concede libertad para acceder a las aplicaciones que necesitan sin tener que pasar siempre por un equipo de TI para solicitar acceso. Cuando un usuario solicita acceso y obtiene la aprobación de forma automática o manual por parte de un propietario de la aplicación, el usuario se agrega a un grupo en el back-end. Hay informes disponibles para saber quién ha solicitado, aprobado o eliminado el acceso y estos le proporcionan control sobre la administración de los roles asignados.

Puede delegar la aprobación de las solicitudes de acceso a las aplicaciones a los aprobadores de la empresa. El aprobador de la empresa puede establecer las contraseñas de acceso a las aplicaciones desde su página Aplicaciones.

Más información: [Uso del acceso de autoservicio a las aplicaciones](https://docs.microsoft.com/azure/active-directory/application-access-panel-self-service-applications-how-to)

![Captura de pantalla de la configuración de la administración del autoservicio de aplicaciones](media/access-panel-deployment-plan/ap-dp-salesforce.png)

## <a name="validate-your-deployment"></a>Validación de la implementación

Asegúrese de que la implementación del panel de acceso se ha probado exhaustivamente y de que hay un plan de reversión en vigor.

Las siguientes pruebas se deben llevar a cabo con dispositivos de propiedad corporativa y con dispositivos personales. Estos casos de prueba también deben reflejar los casos de uso empresarial. A continuación se muestran algunos casos basados en los requisitos empresariales de ejemplo de este documento y en escenarios técnicos habituales. Agregue otros que sean específicos a sus necesidades.

#### <a name="application-sso-access-test-case-examples"></a>Ejemplos de casos de prueba de acceso con el inicio de sesión único de aplicación:


| Oportunidad de negocio| Resultado esperado |
| - | -|
| El usuario inicia sesión en el portal Aplicaciones.| El usuario puede iniciar sesión y ver sus aplicaciones. |
| El usuario inicia una aplicación de inicio de sesión único federado.| El usuario inicia sesión automáticamente en la aplicación. |
| El usuario inicia una aplicación de inicio de sesión único con contraseña por primera vez.| El usuario debe instalar la extensión Aplicaciones. |
| El usuario inicia una aplicación de inicio de sesión único con contraseña las veces posteriores.| El usuario inicia sesión automáticamente en la aplicación. |
| El usuario inicia una aplicación desde el portal de O365.| El usuario inicia sesión automáticamente en la aplicación. |
| El usuario inicia una aplicación desde Managed Browser.| El usuario inicia sesión automáticamente en la aplicación. |


#### <a name="application-self-service-capabilities-test-case-examples"></a>Casos de prueba de ejemplo de las funcionalidades de autoservicio de aplicaciones


| Oportunidad de negocio| Resultado esperado |
| - | - |
| El usuario puede administrar la pertenencia a la aplicación.| El usuario puede agregar o quitar miembros con acceso a la aplicación. |
| El usuario puede editar la aplicación.| El usuario puede editar la descripción y las credenciales de la aplicación en el caso de aplicaciones de inicio de sesión único con contraseña. |

### <a name="rollback-steps"></a>Pasos de reversión

Es importante planear qué hacer en caso de que la implementación no funcione según lo previsto. Si se produce un error en la configuración del inicio de sesión único durante la implementación, debe saber cómo [solucionar los problemas de inicio de sesión único](https://docs.microsoft.com/azure/active-directory/hybrid/tshoot-connect-sso) y reducir el impacto para los usuarios. En circunstancias extremas, puede que necesite [revertir el inicio de sesión único](../manage-apps/plan-sso-deployment.md).


## <a name="manage-your-implementation"></a>Administrar la implementación

Microsoft recomienda usar el rol con menos privilegios para realizar una tarea necesaria en Azure Active Directory. Microsoft recomienda [revisar los distintos roles disponibles](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) y elegir el correcto para resolver las necesidades para cada rol de esta aplicación. Es posible que algunos roles se tengan que aplicar temporalmente y quitar una vez completada la implementación.

| Personas| Roles| Rol de Azure AD  |
| - | -| -|
| Administrador del departamento de soporte técnico| Soporte técnico de nivel 1| None |
| Administración de identidades| Configurar y depurar cuando haya problemas que afectan a Azure AD| Administrador global |
| Administrador de aplicaciones| Atestación de usuarios en la aplicación, configuración en usuarios con permisos| None |
| Administradores de infraestructura| Propietario de la sustitución de certificados| Administrador global |
| Propietario de la empresa o parte interesada| Atestación de usuarios en la aplicación, configuración en usuarios con permisos| None |

Se recomienda usar [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) para administrar los roles con el fin de proporcionar auditoría, control y revisión de acceso adicionales para los usuarios con permisos de directorio.

### <a name="troubleshoot-access-panel-issues"></a>Solución de problemas del panel de acceso

Cree guías de solución de problemas para su equipo de soporte técnico con escenarios comunes y con vínculos a la documentación de Microsoft sobre sus soluciones. Puede que desee crear guías que dividan el soporte técnico en los distintos niveles que usa su organización.

Consulte las siguientes guías de solución de problemas como referencia:

[Aplicaciones que no aparecen](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-troubleshoot-application-not-appearing)

[Aplicaciones inesperadas que aparecen](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-troubleshoot-unexpected-application)

[El usuario no puede iniciar sesión en el panel de acceso](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-troubleshoot-web-sign-in-problem)

[Problemas al usar el acceso de autoservicio a las aplicaciones](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-troubleshoot-self-service-access)

[Problemas con la extensión del explorador](https://docs.microsoft.com/azure/active-directory/manage-apps/manage-access-panel-browser-extension)

## <a name="next-steps"></a>Pasos siguientes

[Planeación de una implementación de Azure Active Directory con autenticación multifactor](https://aka.ms/deploymentplans/mfa)
