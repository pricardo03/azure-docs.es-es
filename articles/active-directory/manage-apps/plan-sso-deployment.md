---
title: Planear una implementación de inicio de sesión única de Azure Active Directory
description: Guía para ayudarle a planear, implementar y administrar el inicio de sesión único en su organización.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.topic: conceptual
ms.workload: identity
ms.date: 05/22/2019
ms.author: baselden
ms.reviewer: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 29569302d20e23c95b6508a5b58c7ed96e005885
ms.sourcegitcommit: 600d5b140dae979f029c43c033757652cddc2029
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66499255"
---
# <a name="plan-a-single-sign-on-deployment"></a>Planear una implementación de inicio de sesión única

Inicio de sesión único (SSO) significa tener acceso a todas las aplicaciones y los recursos de un usuario necesita iniciar sesión en una sola vez usando una cuenta de usuario único. Con SSO, los usuarios acceder a las aplicaciones necesarias todo sin tener que autenticarse una segunda vez.

## <a name="benefits-of-sso"></a>Ventajas de inicio de sesión único

Inicio de sesión único (SSO) agrega seguridad y comodidad cuando los usuarios inician sesión aplicaciones de Azure Active Directory (Azure AD). 

Muchas organizaciones confían en software como servicio (SaaS), como Office 365, Box y Salesforce, para la productividad del usuario final. Históricamente, el personal de TI necesita para crear y actualizar cuentas de usuario en cada aplicación SaaS y es necesarios recordar una contraseña para cada uno de los usuarios individualmente.

Azure Marketplace tiene más de 3000 aplicaciones previamente integradas conexión de inicio de sesión único, lo que facilita integrarlos en el inquilino.

## <a name="licensing"></a>Licencias

- **Licencias de Azure AD** -inicio de sesión único para aplicaciones SaaS preintegradas es gratuito. Sin embargo, el número de objetos en su directorio y las características que desee realizar la implementación puede requerir licencias adicionales. Para obtener una lista completa de los requisitos de licencia, consulte [precios de Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).
- **Las licencias de aplicaciones** -necesitará las licencias correspondientes para las aplicaciones de SaaS satisfacer sus necesidades empresariales. Trabajar con el propietario de la aplicación para determinar si los usuarios asignados a la aplicación tienen las licencias adecuadas para sus funciones dentro de la aplicación. Si Azure AD administra el aprovisionamiento automático basado en roles, los roles asignados en Azure AD deben alinearse con el número de licencias que posee dentro de la aplicación. Número incorrecto de licencias que posee en la aplicación puede dar lugar a errores durante el aprovisionamiento o actualización de un usuario.

## <a name="plan-your-sso-team"></a>Planear su equipo de inicio de sesión único

- **Interactuar con las partes interesadas derechos** : cuando la tecnología que fracasan, los proyectos suele ser debido a expectativas no coincidentes en efecto, resultados y responsabilidades. Para evitar estos problemas, [asegurarse de que está atractivas las partes interesadas derechos](https://aka.ms/deploymentplans) y que las partes interesadas comprendan sus roles.
- **Planeamiento de las comunicaciones** -comunicación es fundamental para el éxito de cualquier servicio nuevo. Comunicar de forma proactiva a los usuarios acerca de cómo cambiará su experiencia, cuándo cambiará y cómo obtener soporte técnico si experimentan problemas. Revise las opciones para [cómo los usuarios finales tendrán acceso a su inicio de sesión único habilitado aplicaciones](end-user-experiences.md)y elaborar las comunicaciones para que coincida con la selección. 

## <a name="plan-your-sso-protocol"></a>Planear el protocolo de inicio de sesión único

Una implementación de SSO basada en protocolos de federación que mejora la seguridad, confiabilidad, y las experiencias de usuario final y es más fácil de implementar. Muchas aplicaciones están previamente integradas en Azure AD con [paso a paso lo guía disponible](../saas-apps/tutorial-list.md). Puede encontrarlos en nuestra [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/). Encontrará información detallada sobre cada método de inicio de sesión único en el artículo [inicio de sesión único para aplicaciones de Azure Active Directory](what-is-single-sign-on.md).

Hay dos formas principales en el que se pueden permitir que los usuarios para inicio de sesión único a las aplicaciones:

- **Con un inicio de sesión único federado** Azure AD autentica al usuario a la aplicación mediante su cuenta de Azure AD. Este método se admite para las aplicaciones que la compatibilidad con protocolos como SAML 2.0, WS-Federation u OpenID Connect y es el modo más enriquecido de inicio de sesión único. Se recomienda usar SSO federado con Azure AD cuando una aplicación lo admite, en lugar de inicio de sesión único basado en contraseña y ADFS.

- **Con basado en contraseña single sign-on** los usuarios inician sesión la aplicación con un nombre de usuario y contraseña la primera vez, que acceden a ella. Después del primer inicio de sesión, Azure AD proporciona el nombre de usuario y la contraseña a la aplicación. El inicio de sesión único basado en contraseña permite el almacenamiento seguro de contraseñas de las aplicaciones y la reproducción mediante una extensión de explorador web o aplicación móvil. Esta opción aprovecha el proceso de inicio de sesión existente proporcionado por la aplicación, permite que un administrador administrar las contraseñas y no requiere que el usuario sepa la contraseña.

### <a name="considerations-for-federation-based-sso"></a>Consideraciones para el inicio de sesión único basado en federación

- **Uso de OAuth y OpenID Connect** : si la aplicación se está conectando a admite, utilice el método OIDC/OAuth 2.0 para habilitar el inicio de sesión único para esa aplicación. Este método requiere menos configuración y permite una experiencia del usuario. Para obtener más información, consulte [OAuth 2.0](../develop/v2-oauth2-auth-code-flow.md), [OpenID Connect 1.0](../develop/v2-protocols-oidc.md), y [Guía del desarrollador de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-developers-guide).
- **Las configuraciones de punto de conexión para el inicio de sesión único basado en SAML** -si usa SAML, los desarrolladores necesitará información específica antes de configurar la aplicación. Para obtener más información, consulte [configurar las opciones básicas de SAML](configure-single-sign-on-portal.md).
- **Certificado de administración basado en SAML SSO** : al habilitar el inicio de sesión único federado para su aplicación, Azure AD crea un certificado que sea válido durante tres años de forma predeterminada. Puede personalizar la fecha de expiración para ese certificado si es necesario. Asegúrese de que tiene procesos en lugar de renovar certificados antes de su caducidad. Para obtener más información, consulte [certificados de administración de Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-sso-certs).

### <a name="considerations-for-password-based-sso"></a>Consideraciones para SSO basado en contraseña

Uso de Azure AD para SSO basado en contraseña requiere implementar una extensión de explorador que recuperará las credenciales de forma segura y rellene los formularios de inicio de sesión. Define un mecanismo para implementar la extensión a escala con [exploradores compatibles](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction). Las opciones son:

- [Directiva de grupo para Internet Explorer ](https://azure.microsoft.com/documentation/articles/active-directory-saas-ie-group-policy/)
- [System Center Configuration Manager (SCCM) para Internet Explorer ](https://docs.microsoft.com/sccm/core/clients/deploy/deploy-clients-to-windows-computers)
- [Descarga y configuración de Chrome, Firefox, Microsoft Edge o IE controlada por el usuario ](https://docs.microsoft.com/azure/active-directory/active-directory-saas-access-panel-introduction)

Para obtener más información, consulte [cómo configurar inicio de sesión único de la contraseña](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-password-sso-non-gallery).

#### <a name="capturing-login-forms-metadata-for-applications-that-arent-in-the-gallery"></a>Captura de metadatos de formularios de inicio de sesión para las aplicaciones que no están en la Galería

Microsoft admite la captura de metadatos en una aplicación web para contraseñas (captura de los campos nombre de usuario y contraseña). Vaya a la dirección URL de inicio de sesión durante el proceso de configuración de la aplicación para capturar los metadatos de formularios. Pida al propietario de la aplicación para la dirección URL de inicio de sesión exactas. Esta información se usa durante el proceso de inicio de sesión, la asignación de credenciales de Azure AD a la aplicación durante el inicio de sesión.

Para obtener más información, consulte [¿qué es el acceso a aplicaciones y el inicio de sesión único con Azure AD?: SSO basado en contraseña](https://azure.microsoft.com/documentation/articles/active-directory-appssoaccess-whatis/).

#### <a name="indications-that-metadata-in-forms-needs-to-be-recaptured"></a>Las indicaciones que los metadatos en formularios deben recuperarse

Cuando las aplicaciones cambian su diseño HTML, es posible que deba volver a capturar los metadatos que se va a ajustar para que los cambios. Los síntomas comunes que indican que el diseño HTML tiene cambios incluyen:

- Usuarios de informes que al hacer clic en la aplicación se "bloquea" en la página de inicio de sesión
- Usuarios que no se rellena el nombre de usuario o la contraseña de informes

#### <a name="shared-accounts"></a>Cuentas compartidas

Desde la perspectiva del inicio de sesión, las aplicaciones con las cuentas compartidas no son diferentes de una aplicación de la galería que utiliza la contraseña de inicio de sesión único para usuarios individuales. Sin embargo, hay algunos pasos adicionales necesarios al planear y configurar una aplicación diseñada para usar las cuentas compartidas:

1. Trabajar con los usuarios de aplicaciones empresariales documentar lo siguiente:
   1. Conjunto de usuarios de la organización que usará la aplicación
   1. Conjunto de credenciales en la aplicación asociada con el conjunto de usuarios existente 
1. Para cada combinación de conjunto de usuarios y credenciales, cree un grupo de seguridad en la nube o local según sus requisitos.
1. Restablecer las credenciales compartidas. Una vez que la aplicación se implementa en Azure AD, los usuarios no tienen la contraseña de la cuenta compartida. Puesto que Azure AD va a almacenar la contraseña, puede establecerla a ser muy larga y compleja. 
1. Configure la sustitución automática de la contraseña si la aplicación lo admite. De este modo, ni siquiera el administrador que realizó la instalación inicial sabrá la contraseña de la cuenta compartida. 

## <a name="plan-your-authentication-method"></a>Planear el método de autenticación

La elección del método de autenticación correcto es una primera decisión fundamental en la configuración de una solución de identidad híbrida de Azure AD. Implemente el método de autenticación que se configura mediante Azure AD Connect, y que también aprovisionará a los usuarios en la nube.

Para elegir un método de autenticación, es necesario tener en cuenta el tiempo, la infraestructura existente, la complejidad y el costo de implementar cada opción. Estos factores varían con cada organización y pueden cambiar con el tiempo. Debe elegir lo que mejor coincida con su escenario concreto. Para obtener más información, consulte [elegir el método de autenticación adecuado para su solución de identidad híbrida de Azure Active Directory](https://docs.microsoft.com/azure/security/azure-ad-choose-authn).

## <a name="plan-your-security-and-governance"></a>Planear la seguridad y gobierno 

Identidad es el nuevo objeto pivot principal para la atención de seguridad y las inversiones porque los perímetros de red se han vuelto cada vez más poroso y menos eficaz con la expansión de los dispositivos BYOD y aplicaciones en la nube. 

### <a name="plan-access-reviews"></a>Planear las revisiones de acceso

[Las revisiones de acceso](https://docs.microsoft.com/azure/active-directory/governance/create-access-review) permiten a las organizaciones a administrar las pertenencias a grupos de forma eficaz, el acceso a aplicaciones empresariales y las asignaciones de roles. Debe planear que revisen el acceso de usuario de forma periódica para asegurarse de que sólo las personas adecuadas tener acceso continuado.

Algunos de los temas claves para planear al configurar las revisiones de acceso incluyen:

1. Que identifica una cadencia de las revisiones de acceso que se adapte a sus necesidades empresariales. Esto puede ser tan frecuente como una vez por semana, mensualmente, anualmente o como un ejercicio y a petición.

1. Crear grupos que representan los revisores de los informes de acceso de la aplicación. Deberá asegurarse de que las partes interesadas más familiarizadas con la aplicación y sus usuarios de destino y casos de uso son los participantes en sus revisiones de acceso

1. Completar una revisión de acceso incluye ya que elimina los permisos de acceso de aplicación a los usuarios que ya no necesitan tener acceso. Plan para controlar potencial solicitudes de soporte técnico de los usuarios denegados. Un usuario eliminado permanecerá eliminado en Azure AD durante 30 días durante el cual se pueden restaurar por un administrador si es necesario. Después de 30 días, ese usuario se eliminará definitivamente. Mediante el portal de Azure Active Directory, un administrador Global puede explícitamente eliminar permanentemente un usuario recién eliminado antes de alcanza ese período de tiempo.

### <a name="plan-auditing"></a>Plan de auditoría

Azure AD proporciona [los informes que contienen los conocimientos técnicos y empresariales](https://azure.microsoft.com/documentation/articles/active-directory-view-access-usage-reports/). 

Seguridad y los informes de actividad están disponibles. Informes de seguridad muestran los usuarios marcados en riesgo e inicios de sesión. Informes de actividad le ayudan a comprender el comportamiento de los usuarios de su organización que detalla la actividad de inicio de sesión y proporcionando las trazas de auditoría de todos los inicios de sesión. Puede usar informes para administrar los riesgos, aumentar la productividad y supervisar el cumplimiento.

| Tipo de informe | Revisión de acceso | Informes de seguridad | Inicio de sesión de informe |
|-------------|---------------|------------------|----------------|
| Se utiliza para revisar | Permisos de la aplicación y el uso. | Cuentas potencialmente en peligro | Quién tiene acceso a las aplicaciones |
| Acciones posibles | Auditar el acceso; revocar permisos | Revocar el acceso; forzar el restablecimiento de la seguridad | Revocar el acceso |

Azure AD conserva los datos de auditorías más de 30 días y hace que los datos disponibles a través del portal de administración de Azure o a través de una API para su descarga en los sistemas de análisis.

### <a name="consider-using-microsoft-cloud-application-security"></a>Considere el uso de Microsoft Cloud Application Security

Microsoft Cloud App Security (MCAS) es una solución de agente de seguridad de acceso en la nube (CASB). Proporciona visibilidad sobre sus servicios y aplicaciones en la nube, proporciona análisis sofisticados para identificar y combatir las ciberamenazas y le permite controlar cómo se transitan los datos.

Le permite implementar MCAS:

- Use Cloud Discovery para asignar e identificar el entorno de nube y las aplicaciones de nube que usa la organización.
- Anular autorizar y autorización de aplicaciones en la nube
- Usar conectores de aplicaciones fáciles de implementar que aprovechan las API del proveedor, para la visibilidad y gobierno de aplicaciones que se conexión a
- Usar la protección de Control de aplicaciones de acceso condicional para obtener visibilidad en tiempo real y control de acceso y las actividades dentro de las aplicaciones de nube
- Le ayuda a tener un control continuo por valor y posterior ajuste permanente, las directivas.

Control de sesión de seguridad de aplicaciones en la nube de Microsoft (MCAS) está disponible para cualquier explorador en cualquier plataforma principal en cualquier sistema operativo. Aplicaciones de escritorio y aplicaciones móviles también se bloquea o permite. Mediante la integración nativa con Azure AD, cualquier aplicaciones que están configuradas con SAML u OpenID Connect con el inicio de sesión único de Azure AD puede admitirse, incluidos [varias aplicaciones destacadas](https://docs.microsoft.com/cloud-app-security/proxy-intro-aad).

Para obtener información sobre MCAS, consulte el [información general de Microsoft Cloud App Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security). MCAS es un servicio de suscripción basada en usuario. Puede revisar los detalles de licencias en el [hoja de datos de licencias de MCAS](https://query.prod.cms.rt.microsoft.com/cms/api/am/binary/RE2NXYO).

### <a name="use-conditional-access"></a>Use el acceso condicional

Con el acceso condicional, puede automatizar las decisiones de control de acceso basado en los criterios para las aplicaciones de nube.

Las directivas de acceso condicional se aplican una vez que se completa la autenticación en una fase. Por lo tanto, el acceso condicional no está pensado como una defensa de primera línea para escenarios como los ataques de denegación de servicio (DoS), pero puede usar las señales de estos eventos para determinar el acceso. Por ejemplo el nivel de riesgo de inicio de sesión, puede usar la ubicación de la solicitud y así sucesivamente. Para obtener más información sobre el acceso condicional, consulte [la información general de](https://docs.microsoft.com/azure/active-directory/conditional-access/plan-conditional-access) y [plan de implementación](https://docs.microsoft.com/azure/active-directory/conditional-access/plan-conditional-access).

## <a name="azure-sso-technical-requirements"></a>Requisitos técnicos de Azure SSO

La siguiente sección detallan los requisitos para configurar la aplicación específica, incluidos los entornos necesarios, los puntos de conexión, la asignación de notificaciones, atributos necesarios, certificados y protocolos usados. Necesitará esta información para configurar el inicio de sesión único en la [portal de Azure AD](https://portal.azure.com/).

### <a name="authentication-mechanism-details"></a>Detalles del mecanismo de autenticación

Para todas las aplicaciones SaaS preintegradas, Microsoft proporciona un tutorial y no necesita esta información. Si la aplicación no está en nuestro catálogo de aplicaciones / Galería de imágenes, es posible que deba recopilar los elementos de datos siguientes:

- **Proveedor de identidad actual usa la aplicación para inicio de sesión único en su caso** , por ejemplo: AD FS, PingFederate, Okta
- **Protocolos admitidos por la aplicación de destino** ; por ejemplo, SAML 2.0, OpenID Connect, OAuth, autenticación basada en formularios, WS-Fed, WS-Trust
- **Protocolo que se ha configurado con Azure AD** ; por ejemplo, SAML 2.0 o 1.1, OpenID Connect, OAuth, basada en formularios, WS-Fed

### <a name="attribute-requirements"></a>Requisitos de los atributos

Hay un conjunto preconfigurado de atributos y asignaciones de atributos entre los objetos de usuario de Azure AD y los objetos de usuario de cada aplicación SaaS. Algunas aplicaciones administran otros tipos de objetos como los grupos. Planear la asignación de atributos de usuario de Azure AD a la aplicación y [personalizar las asignaciones de atributos predeterminado](https://docs.microsoft.com/azure/active-directory/manage-apps/customize-application-attributes) según su empresa necesita.

### <a name="certificate-requirements"></a>Requisitos de certificados

El certificado de la aplicación debe estar actualizado o no hay riesgo de que los usuarios no puedan acceder a la aplicación. La mayoría de los certificados de aplicación de SaaS son buenos para 36 meses. Cambiar esa duración del certificado en la hoja de la aplicación. Asegúrese de que la expiración de documento y saber cómo administrará la renovación de certificados. 

Hay dos maneras de administrar sus certificados. 

- **Sustitución automática de certificados** -Microsoft admite [cadencia de sustitución de claves en Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-signing-key-rollover). Aunque este es el método preferido para administrar certificados, no todos los ISV admite este escenario.

- **Actualizar manualmente** -cada aplicación tiene su propio certificado expira en función de cómo se define. Antes de que expire el certificado de la aplicación, cree un nuevo certificado y enviarlo a lo ISV. Esta información puede extraerse de los metadatos de federación. [Lea más acerca de los metadatos de federación aquí.](https://docs.microsoft.com/azure/active-directory/develop/active-directory-federation-metadata)

## <a name="implement-sso"></a>Implementar inicio de sesión único

Use las siguientes fases para planear e implementar la solución en su organización:

### <a name="user-configuration-for-sso"></a>Configuración de usuario para el inicio de sesión único

- **Identificar los usuarios de prueba**

   Póngase en contacto con el propietario de la aplicación y pídale que cree un mínimo de usuarios de la serie de pruebas dentro de la aplicación. Asegúrese de la información que va a usar como identificador principal está rellenada correctamente y que coincide con un atributo que está disponible en Azure AD. En la mayoría de los casos, esto se asignará a "NameID" para SAML-based applications. Para los tokens JWT, es "preferred_username".
   
   Crear el usuario en Azure AD ya sea manualmente como un usuario basado en la nube o sincronizar el usuario local mediante el motor de sincronización de Azure AD Connect. Asegúrese de que la información coincide con las notificaciones que se envían a la aplicación.

- **Configurar el inicio de sesión único**

   Desde el [lista de aplicaciones](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list), busque y abra el tutorial de inicio de sesión único para la aplicación y después siga los pasos del tutorial de configurar correctamente la aplicación de SaaS.

   Si no se puede encontrar la aplicación, consulte [documentación de la aplicación personalizada](https://docs.microsoft.com/azure/active-directory/application-config-sso-how-to-configure-federated-sso-non-gallery). Esto le guiará a través de cómo agregar una aplicación que no se encuentra en la Galería de Azure AD.

   Si lo desea, puede usar las notificaciones emitidas en el token SAML para aplicaciones de negocio con [documentación de la Guía de Microsoft](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping). Asegúrese de que se asigna a lo que espera recibir en la respuesta SAML para la aplicación. Si encuentra problemas durante la configuración, use nuestra guía en [cómo la integración de SSO depurar](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging).

Incorporación de la aplicación personalizada es una característica de licencias de Azure AD Premium P1 o P2.

### <a name="provide-sso-change-communications-to-end-users"></a>Proporcionar comunicaciones de cambio de inicio de sesión único a los usuarios finales

Implementar el plan de comunicación. Asegúrese de que permite que los usuarios finales saber que un cambio es, cuando llegó, qué hacer ahora y cómo buscar ayuda.

### <a name="verify-end-user-scenarios-for-sso"></a>Compruebe los escenarios de usuario final para el inicio de sesión único

Puede usar los siguientes casos de prueba para realizar pruebas en la empresa y dispositivos personales para asegurar las configuraciones de inicio de sesión único se funcionan según lo esperado. Los escenarios siguientes se suponen que un usuario navegar a una dirección URL de la aplicación y pasar por un flujo de autenticación iniciado por el proveedor de servicios (flujo de autenticación iniciada por SP).

| Escenario | Resultado esperado en el flujo de autenticación iniciado por el usuario |
|----------|---------------------------------------------------|
| Inicie sesión en la aplicación con Internet Explorer mientras se encuentra en la red corporativa. | Se produce la autenticación integrada de Windows (IWA) sin solicitudes adicionales. |
| Inicie sesión en la aplicación con Internet Explorer mientras ajenas con nuevo intento de inicio de sesión. | Mensaje basada en formularios en el servidor de AD FS. Usuario inicia sesión correctamente y el explorador solicita MFA. |
| Inicie sesión en la aplicación con Internet Explorer mientras ajenas a una sesión actual y nunca se ha realizado la MFA. | Usuario no recibirá el símbolo del sistema para el primer factor. Usuario recibe de símbolo del sistema para MFA. |
| Inicie sesión en la aplicación con Internet Explorer mientras ajenas a una sesión actual y ya se ha realizado la MFA en esta sesión. | Usuario no recibirá el símbolo del sistema para el primer factor. Usuario no recibirá MFA. SSOs de usuario en la aplicación. |
| Inicie sesión en la aplicación de Chrome, Firefox o Safari mientras ajenas a una sesión actual y ya se ha realizado la MFA en esta sesión. | Usuario no recibirá el símbolo del sistema para el primer factor. Usuario no recibirá MFA. Usuario de inicio de sesión único en la aplicación. |
| Inicie sesión en la aplicación de Chrome, Firefox o Safari mientras ajenas con nuevo intento de inicio de sesión. | Mensaje basada en formularios en el servidor de AD FS. Usuario inicia sesión correctamente y el explorador solicita MFA. |
| Inicie sesión en la aplicación de Chrome o Firefox mientras se encuentra en la red corporativa con una sesión actual. | Usuario no recibirá el símbolo del sistema para el primer factor. Usuario no recibirá MFA. Usuario de inicio de sesión único en la aplicación. |
| Inicie sesión en la aplicación con la aplicación móvil de aplicación con un nuevo intento de inicio de sesión. | Mensaje basada en formularios en el servidor de AD FS. Usuario inicia sesión correctamente y solicita MFA cliente ADAL. |
| Usuario no autorizado intenta iniciar sesión en la aplicación con la dirección URL de inicio de sesión. | Mensaje basada en formularios en el servidor de AD FS. No se puede iniciar sesión con el primer factor de usuario. |
| Usuario autorizado intenta iniciar sesión pero escribe una contraseña incorrecta. | Usuario navega a la dirección URL de la aplicación y recibe un error de nombre de usuario o contraseña incorrectos. |
| Usuario autorizado hace clic en el vínculo en un correo electrónico y ya está autenticado. | Usuario hace clic en la dirección URL y ha iniciado sesión en la aplicación sin solicitudes adicionales. |
| Usuario autorizado hace clic en el vínculo en un correo electrónico y aún no se ha autenticado. | Símbolo del sistema para autenticarse con el primer factor de usuario y hace clic en la dirección URL. |
| Autorizado los registros de usuario en la aplicación con la aplicación móvil de aplicaciones (iniciado por SP) con un nuevo intento de inicio de sesión. | Mensaje basada en formularios en el servidor de AD FS. Usuario inicia sesión correctamente y solicita MFA cliente ADAL. |
| Usuario no autorizado intenta iniciar sesión en la aplicación con la dirección URL de inicio de sesión (iniciado por SP). | Mensaje basada en formularios en el servidor de AD FS. No se puede iniciar sesión con el primer factor de usuario. |
| Usuario autorizado intenta iniciar sesión pero escribe una contraseña incorrecta.| Usuario navega a la dirección URL de la aplicación y recibe un error de nombre de usuario o contraseña incorrectos. |
| Usuario autorizado cierra la sesión y, a continuación, inicia sesión. | Si se configura la dirección URL de cierre de sesión, sesión del usuario en todos los servicios y el símbolo del sistema para autenticarse. |
| Usuario autorizado cierra la sesión y, a continuación, inicia sesión. | Si no se configura la dirección URL de cierre de sesión, usuario iniciará sesión automáticamente de nuevo con un token existente desde la sesión del explorador de Azure AD existente. |
| Usuario autorizado hace clic en el vínculo en un correo electrónico y ya está autenticado. | Usuario hace clic en la dirección URL y ha iniciado sesión en la aplicación sin solicitudes adicionales. |
| Usuario autorizado hace clic en el vínculo en un correo electrónico y aún no se ha autenticado. | Símbolo del sistema para autenticarse con el primer factor de usuario y hace clic en la dirección URL. |

## <a name="manage-sso"></a>Administración de SSO

En esta sección se describe los requisitos y recomendaciones para administrar correctamente el inicio de sesión único.

### <a name="required-administrative-roles"></a>Roles administrativos necesarios

Utilice siempre la función con los permisos mínimos para realizar la tarea requerida en Azure Active Directory. Microsoft recomienda [revisar los distintos roles que están disponibles](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) y elegir la correcta para resolver sus necesidades para cada rol para esta aplicación. Algunos roles que deba aplicarse temporalmente y quitan una vez completada la implementación.

| Persona| Roles | Rol de Azure AD (si es necesario) |
|--------|-------|-----------------------------|
| Ayudar al administrador del servicio de asistencia | Soporte técnico de nivel 1 | None |
| Administración de identidad | Configurar y depurar cuando afectan a problemas de Azure AD | Administrador global |
| Administración de aplicaciones | Atestación de usuario en la aplicación, la configuración de los usuarios con permisos | None |
| Administradores de infraestructura | Propietario de la sustitución de certificados | Administrador global |
| Propietario de la empresa o parte interesada | Atestación de usuario en la aplicación, la configuración de los usuarios con permisos | None |

Se recomienda usar [Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-configure) (PIM) para administrar los roles para proporcionar revisión auditoría adicional, control y acceso para los usuarios con permisos de directorio.

### <a name="sso-certificate-lifecycle-management"></a>Administración de ciclo de vida de certificados de inicio de sesión único

Es importante identificar los roles adecuados y encargadas de administrar el ciclo de vida del certificado entre Azure AD firmado de listas de distribución de correo electrónico y la aplicación que se está configurando con inicio de sesión único. Estas son algunas de las funciones claves, que se recomienda tener en su lugar:

- Propietario para actualizar las propiedades de usuario en la aplicación
- Propietario de la llamada para obtener soporte break-fix en aplicación
- Lista de distribución de correo electrónico estrechamente supervisado notificaciones de cambios relacionados con el certificado

La duración máxima de un certificado es de tres años. Se recomienda establecer un proceso en cómo controlar un cambio de certificado entre Azure AD y la aplicación. Esto puede ayudar a evitar o minimizar una interrupción debido a la expiración del certificado o forzar la sustitución del certificado.

### <a name="rollback-process"></a>Proceso de reversión

Después de completar las pruebas en función de los casos de prueba, es momento de pasar a producción con la aplicación. Pasar a producción significa que se implementan las configuraciones de planeado y probadas en el inquilino de producción y aplicarlo a los usuarios. Sin embargo, es importante planear qué hacer en caso de que la implementación no funciona según lo previsto. Si se produce un error en la configuración de SSO durante la implementación, debe aprender a mitigar cualquier interrupción y reducir el impacto en los usuarios.

La disponibilidad de los métodos de autenticación dentro de la aplicación determinará la mejor estrategia. Asegúrese siempre de que dispone de documentación detallada para los propietarios de la aplicación en exactamente cómo obtener el estado de configuración de inicio de sesión original en caso de que la implementación se ejecuta en los problemas.

- **Si la aplicación admite varios proveedores de identidades**, por ejemplo, LDAP y AD FS y Ping, no elimine la configuración de inicio de sesión único existente durante la implementación. En su lugar, deshabilítela durante la migración en caso de que deba cambiar volver más tarde. 

- **Si la aplicación no admite varios IDP** pero permite a los usuarios iniciar sesión con autenticación basada en formularios (nombre de usuario/contraseña), asegúrese de que los usuarios pueden revertir a este enfoque en caso de que se produce un error en la implementación de configuración de SSO nueva.

### <a name="access-management"></a>administración de acceso

Se recomienda elegir un enfoque escalado al administrar el acceso a los recursos. Los enfoques comunes incluyen el uso de grupos locales mediante la sincronización a través de Azure AD Connect, [crear grupos dinámicos en Azure AD basado en atributos de usuario](https://docs.microsoft.com/azure/active-directory/active-directory-groups-dynamic-membership-azure-portal), o crear [grupos de autoservicio](https://docs.microsoft.com/azure/active-directory/active-directory-accessmanagement-self-service-group-management) en Azure AD administrado por un propietario del recurso.

### <a name="monitor-security"></a>Supervisar la seguridad

Se recomienda configurar regularmente en la que revisar los diferentes aspectos de seguridad de aplicaciones SaaS y realizar acciones correctoras que se necesitan.

### <a name="troubleshooting"></a>solución de problemas

Los vínculos siguientes presentan escenarios de solución de problemas. Es posible que desee crear a una guía específica para el personal de soporte técnico que incorpore estos escenarios y los pasos para corregirlos.

#### <a name="consent-issues"></a>Problemas de consentimiento

- [Error de consentimiento inesperada](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-unexpected-user-consent-prompt)

- [Error de consentimiento de usuario](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-unexpected-user-consent-error)

#### <a name="sign-in-issues"></a>Problemas de inicio de sesión

- [Problemas al iniciar sesión desde un portal personalizado](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-other-problem-deeplink)

- [Problemas al iniciar sesión desde el panel de acceso](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-other-problem-access-panel)

- [Error en la página de inicio de sesión de la aplicación](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-application-error)

- [Problema al iniciar sesión en una aplicación de Microsoft](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-first-party-microsoft)

#### <a name="sso-issues-for-applications-listed-in-the-azure-application-gallery"></a>Problemas de inicio de sesión único para aplicaciones que aparecen en la Galería de aplicaciones de Azure

- [Problema con la contraseña de inicio de sesión único para las aplicaciones enumeradas en la Galería de aplicaciones de Azure](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-password-sso-gallery) 

- [Problema con el inicio de sesión único federado para aplicaciones que aparecen en la Galería de aplicaciones de Azure](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-federated-sso-gallery)   

#### <a name="sso-issues-for-applications-not-listed-in-the-azure-application-gallery"></a>Problemas de inicio de sesión único para aplicaciones que no aparecen en la Galería de aplicaciones de Azure

- [Problema con la contraseña de inicio de sesión único para aplicaciones que no aparecen en la Galería de aplicaciones de Azure](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-password-sso-non-gallery) 

- [Problema con el inicio de sesión único federado para aplicaciones que no aparecen en la Galería de aplicaciones de Azure](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-federated-sso-non-gallery)

## <a name="next-steps"></a>Pasos siguientes

[Depuración de SSO basado en SAML](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

[Asignación de notificación para las aplicaciones a través de PowerShell](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping)

[Personalización de notificaciones emitidas en el token SAML](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization)

[Protocolo SAML de inicio de sesión único](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

[Protocolo SAML de cierre de sesión único](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-out-protocol-reference)

[B2B de Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) (para los usuarios externos como los socios y proveedores)

[Acceso condicional de Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)

[Azure Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection)

[Acceso con SSO](https://docs.microsoft.com/azure/active-directory/active-directory-appssoaccess-whatis)

[Tutorial de inicio de sesión único de aplicación](https://docs.microsoft.com/azure/active-directory/saas-apps/tutorial-list)
