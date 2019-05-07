---
title: Procedimientos recomendados de seguridad de acceso e identidad de Azure | Microsoft Docs
description: Este artículo proporciona un conjunto de procedimientos recomendados para la administración de identidades y la seguridad del control de acceso mediante las funcionalidades integradas de Azure.
services: security
documentationcenter: na
author: barclayn
manager: barbkess
editor: TomSh
ms.assetid: 07d8e8a8-47e8-447c-9c06-3a88d2713bc1
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/03/2019
ms.author: barclayn
ms.openlocfilehash: 2a669f5b46db4d5de7d1d6863b94e6c117667aee
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65153237"
---
# <a name="azure-identity-management-and-access-control-security-best-practices"></a>Procedimientos recomendados para la administración de identidades y la seguridad del control de acceso en Azure
En este artículo, se trata un conjunto de procedimientos recomendados para la seguridad del control de acceso y la administración de identidades en Azure. Estos procedimientos recomendados proceden de nuestra experiencia con [Azure AD](../active-directory/fundamentals/active-directory-whatis.md) y las experiencias de clientes como usted.

Para cada procedimiento recomendado, explicaremos:

* Qué es el procedimiento recomendado
* Por qué le conviene habilitar este procedimiento recomendado
* Cuál podría ser el resultado si no habilita el procedimiento recomendado
* Alternativas posibles al procedimiento recomendado
* Cómo aprender a habilitar el procedimiento recomendado

Este artículo sobre procedimientos recomendados para la seguridad del control de acceso y la administración de identidades en Azure se basa en una opinión consensuada y en las funcionalidades y conjuntos de características de la plataforma de Azure que existen en el momento de su publicación. Las opiniones y las tecnologías cambian con el tiempo, por lo que se actualizará de forma periódica para reflejar esos cambios.

Los procedimientos recomendados para la seguridad del control de acceso y la administración de identidades en Azure que se describen en este artículo son:

* Tratar las amenazas como el perímetro de seguridad principal
* Centralizar la administración de identidades
* Administrar inquilinos conectados
* Habilitar el inicio de sesión único
* Activar el acceso condicional
* Habilitación de la administración de contraseñas
* Exigir a los usuarios la verificación multifactor
* Uso del control de acceso basado en rol
* Menor exposición de las cuentas con privilegios
* Controlar las ubicaciones donde se encuentran los recursos
* Usar Azure AD para la autenticación de almacenamiento

## <a name="treat-identity-as-the-primary-security-perimeter"></a>Tratar las amenazas como el perímetro de seguridad principal

Muchos consideran que la identidad es el perímetro principal para la seguridad. Se trata de un cambio desde el enfoque tradicional de seguridad de red. Los perímetros de red continúan volviéndose más porosos y esa defensa perimetral no puede ser tan eficaz como lo era antes de la explosión de dispositivos y aplicaciones en la nube de [BYOD](https://aka.ms/byodcg).

[Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) es la solución de Azure para la administración de identidades y de acceso. Azure AD es un servicio de administración de identidades y directorios multiinquilino basado en la nube de Microsoft. Combina servicios de directorio fundamentales, la administración del acceso a las aplicaciones y la protección de identidades en una única solución.

En las siguientes secciones se enumeran los procedimientos recomendados de seguridad de acceso e identidades con Azure AD.

## <a name="centralize-identity-management"></a>Centralizar la administración de identidades

En un escenario de [identidad híbrida](https://resources.office.com/ww-landing-M365E-EMS-IDAM-Hybrid-Identity-WhitePaper.html?), se recomienda integrar los directorios en el entorno local y en la nube. Integración permite al equipo de TI administrar las cuentas de una ubicación, independientemente de donde se crea una cuenta. Integración también ayuda a los usuarios ser más productivos proporcionando una identidad común para acceder a recursos en la nube y locales.

**Procedimiento recomendado**: Establecer una única instancia de Azure AD. Coherencia y una sola orígenes autorizados aumentará la claridad y reducir los riesgos de seguridad de los errores humanos y complejidad de la configuración.
**Detalles**: Designar una única instancia de AD Azure directorio como el origen de autoridad para cuentas organizativas y corporativos.

**Procedimiento recomendado**: Integración de los directorios locales con Azure AD.  
**Detalles**: use [Azure AD Connect](../active-directory/connect/active-directory-aadconnect.md) para sincronizar el directorio local con el directorio en la nube.

> [!Note]
> Hay [factores que afectan al rendimiento de Azure AD Connect](../active-directory/hybrid/plan-connect-performance-factors.md). Asegúrese de que Azure AD Connect tiene suficiente capacidad para mantener con un rendimiento deficiente de los sistemas de obstaculizar la seguridad y productividad. Las organizaciones grandes o complejas (organizaciones de aprovisionamiento más de 100 000 objetos) deben seguir el [recomendaciones](../active-directory/hybrid/whatis-hybrid-identity.md) para optimizar su implementación de Azure AD Connect.

**Procedimiento recomendado**: No sincronizar las cuentas de Azure AD que tienen privilegios elevados en la instancia de Active Directory existente.
**Detalles**: No cambie el valor predeterminado [configuración de Azure AD Connect](../active-directory/hybrid/how-to-connect-sync-configure-filtering.md) que filtra estas cuentas. Esta configuración reduce el riesgo de que los adversarios dinamizar de nube a los recursos locales (que podrían crear un incidente principal).

**Procedimiento recomendado**: Activación de la sincronización de hashes de contraseñas.  
**Detalles**: Sincronización de hash de contraseña es una característica que sirve para sincronizar los hash de contraseña de usuario de una instancia de Active Directory local a una basada en la nube de Azure instancia de AD. Esta sincronización ayuda a protegerse contra las credenciales filtradas que se está reproducidas frente a ataques anteriores.

Aunque decida usar la federación con Servicios de federación de Active Directory (AD FS) u otros proveedores de identidades, si quiere, puede configurar la sincronización de hashes de contraseñas para tener una opción alternativa si los servidores locales sufren un error o dejan de estar disponibles temporalmente. Esta sincronización permite a los usuarios iniciar sesión en el servicio mediante el uso de la misma contraseña que usa para iniciar sesión su instancia de Active Directory local. También permite la protección de identidad detectar las credenciales en peligro mediante la comparación de valores hash de contraseña sincronizada con contraseñas que se conoce en peligro, si un usuario ha usado la misma dirección de correo electrónico y la contraseña en otros servicios que no están conectados a Azure AD.

Para más información, consulte [Implement password hash synchronization with Azure AD Connect sync](../active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md) (Implementación de la sincronización de hash de contraseñas mediante la sincronización de Azure AD Connect).

**Procedimiento recomendado**: Para el desarrollo de nuevas aplicaciones, use Azure AD para la autenticación.
**Detalles**: Use la capacidad correcta para admitir la autenticación:

  - Azure AD para los empleados
  - [B2B de Azure AD](https://docs.microsoft.com/azure/active-directory/b2b/) para los usuarios invitados y socios externos
  - [B2C de Azure AD](https://docs.microsoft.com/azure/active-directory-b2c/) para controlar cómo se suscriben a los clientes, inicie sesión y administran sus perfiles al usar las aplicaciones

Las organizaciones que no integren la identidad del entorno local con la identidad en la nube pueden tener mayor sobrecarga para administrar cuentas. Esta sobrecarga aumenta la probabilidad de que haya errores e infracciones de seguridad.

> [!Note]
> Debe elegir qué directorios críticos residen en cuentas y procesa si la estación de trabajo de administración usa es administrado por servicios en la nube nuevo o existente. Uso de administración existente y los procesos de aprovisionamiento de identidad puede reducir algunos riesgos pero también puede crear el riesgo de que un atacante poner en peligro una cuenta local y dinamizar en la nube. Es posible que desee usar una estrategia diferente para distintas funciones (por ejemplo, los administradores de TI frente a los administradores de unidades de negocio). Tiene dos opciones: Primera opción es crear cuentas de Azure AD que no están sincronizadas con la instancia de Active Directory local. Únase a la estación de trabajo de administración para Azure AD, que puede administrar y aplicar revisiones mediante Microsoft Intune. Segunda opción es usar cuentas de administrador existente mediante la sincronización de la instancia de Active Directory local. Use estaciones de trabajo existentes en el dominio de Active Directory para la administración y seguridad.

## <a name="manage-connected-tenants"></a>Administrar inquilinos conectados
Su organización de seguridad necesita visibilidad para evaluar el riesgo y determinar si se siguen las directivas de su organización y los requisitos normativos. Debe asegurarse de que su organización de seguridad tiene visibilidad en todas las suscripciones conectadas a su entorno de producción y la red (a través de [Azure ExpressRoute](../expressroute/expressroute-introduction.md) o [VPN de sitio a sitio](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md)). Un [Global Administrador o administrador de empresa](../active-directory/users-groups-roles/directory-assign-admin-roles.md#company-administrator) en Azure AD pueden elevar su acceso a la [Administrador de acceso de usuario](../role-based-access-control/built-in-roles.md#user-access-administrator) rol y ver todas las suscripciones y los grupos administrados conectados a su entorno.

Consulte [elevación del acceso para administrar todas las suscripciones de Azure y los grupos de administración](../role-based-access-control/elevate-access-global-admin.md) para asegurarse de que usted y su grupo de seguridad pueden ver todas las suscripciones o grupos de administración conectados a su entorno. Debe quitar este acceso con privilegios elevados después de que haya evaluado los riesgos.

## <a name="enable-single-sign-on"></a>Habilitar el inicio de sesión único

En un mundo donde la nube y la movilidad son lo primero, resulta útil habilitar el inicio de sesión único (SSO) en dispositivos, aplicaciones y servicios desde cualquier sitio, de modo que los usuarios puedan ser productivos en cualquier momento y lugar. Cuando es necesario administrar varias soluciones de identidad, esto supone un problema administrativo no solo para TI, sino también para los usuarios que tendrán que recordar varias contraseñas.

Mediante el uso de la misma solución de identidad para todas las aplicaciones y los recursos, podrá disfrutar del SSO. Además, los usuarios podrán usar el mismo conjunto de credenciales para iniciar sesión y acceder a los recursos que necesitan, con independencia de dónde se ubiquen estos recursos, tanto si es en el entorno local como en la nube.

**Procedimiento recomendado**: habilitar SSO.  
**Detalles**: Azure AD [extiende Active Directory del entorno local](../active-directory/connect/active-directory-aadconnect.md) a la nube. Los usuarios pueden usar su cuenta profesional o educativa principal para los dispositivos unidos a un dominio, los recursos de la empresa y todas las aplicaciones web y SaaS que necesitan para realizar su trabajo. Los usuarios no tienen que recordar varios nombres de usuario y contraseñas y el acceso a las aplicaciones por parte de los usuarios se puede aprovisionar (o desaprovisionar) automáticamente, en función de su pertenencia a los grupos de la organización y de su estado como empleado. Además, puede controlar el acceso de las aplicaciones de la galería o de sus propias aplicaciones locales que ha desarrollado y publicado mediante el [proxy de la aplicación de Azure AD](../active-directory/active-directory-application-proxy-get-started.md).

Use el SSO para permitir que los usuarios accedan a sus [aplicaciones SaaS](../active-directory/active-directory-appssoaccess-whatis.md) en función de su cuenta profesional o educativa en Azure AD. Esto no solo es aplicable a las aplicaciones para SaaS de Microsoft, sino también a otras aplicaciones, como [Google Apps](../active-directory/active-directory-saas-google-apps-tutorial.md) y [Salesforce](../active-directory/active-directory-saas-salesforce-tutorial.md). Puede configurar su aplicación de modo que use Azure AD como un proveedor de [identidades basado en SAML](../active-directory/fundamentals-identity.md). Como control de seguridad, Azure AD no emite ningún token que permita a los usuarios iniciar sesión en la aplicación, a menos que se les conceda acceso mediante Azure AD. Puede concederles acceso directamente o a través de un grupo al cual pertenezcan.

Las organizaciones que no crean ninguna identidad común para establecer el SSO para sus usuarios y aplicaciones están más expuestas a escenarios donde los usuarios tienen varias contraseñas. Estos escenarios aumentan la probabilidad de que los usuarios reutilicen las contraseñas o usen contraseñas débiles.

## <a name="turn-on-conditional-access"></a>Activar el acceso condicional

Los usuarios pueden acceder a los recursos de su organización mediante diversos dispositivos y aplicaciones desde cualquier lugar. Como administrador de TI, desea asegurarse de que estos dispositivos cumplen los estándares de seguridad y cumplimiento. Ya no es suficiente con centrarse en quién puede acceder a un recurso.

Para equilibrar la seguridad y productividad, debe pensar en cómo se accede a un recurso antes de que puede tomar una decisión sobre el control de acceso. Con el acceso condicional de Azure AD, puede abordar este requisito. Con el acceso condicional, puede tomar decisiones de control de acceso automatizado según las condiciones para tener acceso a las aplicaciones de nube.

**Procedimiento recomendado**: Administración y control del acceso a los recursos corporativos.  
**Detalles**: configure el [acceso condicional](../active-directory/active-directory-conditional-access-azure-portal.md) de Azure AD en función del grupo, la ubicación y la confidencialidad de la aplicación para las aplicaciones SaaS y las aplicaciones conectadas a Azure AD.

**Procedimiento recomendado**: Bloquear protocolos de autenticación heredados.
**Detalles**: Los atacantes aprovechan los puntos débiles en los protocolos anteriores cada día, especialmente para Rociar ataques a contraseñas. Configurar el acceso condicional para bloquear protocolos heredados. Vea el vídeo [Azure AD: Qué hacer y qué no](https://www.youtube.com/watch?v=wGk0J4z90GI) para obtener más información.

## <a name="enable-password-management"></a>Habilitación de la administración de contraseñas

Si tiene varios inquilinos o quiere permitir que los usuarios [restablezcan su propia contraseña](../active-directory/active-directory-passwords-update-your-own-password.md), es importante utilizar directivas de seguridad adecuadas para evitar un uso inadecuado.

**Procedimiento recomendado**: Configuración del autoservicio de restablecimiento de contraseña (SSPR) para los usuarios.  
**Detalles**: use la característica de [autoservicio de restablecimiento de contraseña](../active-directory-b2c/active-directory-b2c-reference-sspr.md) de Azure AD.

**Procedimiento recomendado**: Supervisar cómo se usa realmente SSPR o si se puede usar.  
**Detalles**: supervise los usuarios que se registran mediante el [informe de actividad de registro de restablecimiento de contraseña](../active-directory/active-directory-passwords-get-insights.md) de Azure AD. La característica de creación de informes que proporciona Azure AD le ayuda a responder preguntas mediante informes creados previamente. Si está debidamente protegido por licencia, también puede crear consultas personalizadas.

**Procedimiento recomendado**: Ampliar las directivas de contraseña en la nube para su infraestructura local.
**Detalles**: Mejore las directivas de contraseñas en su organización mediante la realización de las mismas comprobaciones para los cambios de contraseña de forma local como lo haría para los cambios de contraseña en la nube. Instalar [protección mediante contraseña de Azure AD](../active-directory/authentication/concept-password-ban-bad.md) para Windows Server Active Directory agentes locales ampliar las listas de contraseñas prohibidas en la infraestructura existente. Los usuarios y administradores que cambian, establecer o restablecer las contraseñas locales son necesarias para cumplir con la misma directiva de contraseña que los usuarios solo en la nube.

## <a name="enforce-multi-factor-verification-for-users"></a>Exigir a los usuarios la verificación multifactor

Se recomienda exigir la verificación en dos pasos a todos los usuarios. Esto incluye a los administradores y otras personas de su organización, ya que el hecho de que su cuenta esté en peligro puede tener un impacto significativo (por ejemplo, los directores financieros).

Existen varias opciones para exigir la verificación en dos pasos. La mejor opción para usted depende de sus objetivos, la edición de Azure AD que ejecuta y su programa de licencias. Consulte [Exigencia de verificación en dos pasos para un usuario](../active-directory/authentication/howto-mfa-userstates.md) para determinar la mejor opción para usted. Puede encontrar más información sobre licencias y precios en las páginas de precios de [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/) y [Azure Multi-Factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

A continuación, se indican las opciones y ventajas para habilitar la verificación en dos pasos:

**Opción 1**: [habilitar Multi-factor Authentication mediante el cambio de estado de usuario](../active-directory/authentication/howto-mfa-userstates.md).   
**Ventaja**: este es el método tradicional para exigir la verificación en dos pasos. Funciona tanto con [Azure Multi-factor Authentication en la nube como en el Servidor Azure Multi-factor Authentication](../active-directory/authentication/concept-mfa-whichversion.md). El uso de este método requiere que los usuarios realicen la verificación en dos pasos cada vez que inicien sesión e invalida las directivas de acceso condicional.

Para determinar dónde debe habilitarse la autenticación multifactor, consulte [qué versión de Azure MFA es adecuado para mi organización?](../active-directory/authentication/concept-mfa-whichversion.md).

**Opción 2**: [habilitar Multi-Factor Authentication con la directiva de acceso condicional](../active-directory/authentication/howto-mfa-getstarted.md).
**Ventaja**: esta opción te permite solicitar la verificación en dos pasos en condiciones específicas mediante el uso del [acceso condicional](../active-directory/active-directory-conditional-access-azure-portal.md). Las condiciones específicas pueden ser el inicio de sesión del usuario desde distintas ubicaciones, dispositivos no confiables o aplicaciones que considere de riesgo. Definir condiciones específicas donde exija la verificación en dos pasos le permite evitar pedirla constantemente a los usuarios, lo cual puede ser una experiencia desagradable para el usuario.

Esta es la forma más flexible de habilitar la verificación en dos pasos para los usuarios. La habilitación de la directiva de acceso condicional solo funciona con Azure Multi-Factor Authentication en la nube y es una característica premium de Azure AD. Puede encontrar más información sobre este método en [Implementación de Azure Multi-factor Authentication en la nube](../active-directory/authentication/howto-mfa-getstarted.md).

**Opción 3**: habilitar Multi-Factor Authentication con directivas de acceso condicional mediante la evaluación de riesgo de usuario e inicio de sesión de [Azure AD Identity Protection](../active-directory/authentication/tutorial-risk-based-sspr-mfa.md).   
**Ventaja**: esta opción le permite:

- Detectar posibles vulnerabilidades que afectan a las identidades de la organización.
- Configurar respuestas automatizadas a acciones sospechosas detectadas que están relacionadas con las identidades de la organización.
- Investigar incidentes sospechosos y tomar las medidas adecuadas para resolverlos.

Este método utiliza la evaluación de riesgos de Azure AD Identity Protection para determinar si se requiere la verificación en dos pasos en función de los riesgos del usuario y el inicio de sesión para todas las aplicaciones en la nube. Este método requiere una licencia de Azure Active Directory P2. Para obtener más información sobre este método, consulte [Azure Active Directory Identity Protection](../active-directory/identity-protection/overview.md).

> [!Note]
> La opción 1, en la que se habilita Multi-Factor Authentication al cambiar el estado del usuario, invalida las directivas de acceso condicional. Dado que las opciones de 2 y 3 usan directivas de acceso condicional, no puede usar la opción 1 con ellas.

Las organizaciones que no agregan capas de protección de la identidad adicionales, como la verificación en dos pasos, son más susceptibles a ataques de robo de credenciales. Un ataque de robo de credenciales puede poner en peligro la seguridad de los datos.

## <a name="use-role-based-access-control"></a>Uso del control de acceso basado en rol
Administración de acceso de recursos en la nube es fundamental para cualquier organización que utiliza la nube. [Control de acceso basado en roles (RBAC)](../role-based-access-control/overview.md) le ayuda a administrar quién tiene acceso a los recursos de Azure, lo pueden hacer con esos recursos y qué áreas que tienen acceso.

Designación de grupos o roles individuales responsables de funciones específicas de Azure ayuda a evitar la confusión que puede conducir a los usuarios y los errores de automatización que crean los riesgos de seguridad. En organizaciones que quieren aplicar directivas de seguridad para el acceso a los datos, es imperativo restringir el acceso en función de los principios de seguridad [necesidad de saber](https://en.wikipedia.org/wiki/Need_to_know) y [mínimo privilegio](https://en.wikipedia.org/wiki/Principle_of_least_privilege).

El equipo de seguridad necesita visibilidad sobre los recursos de Azure para evaluar y corregir el riesgo. Si el equipo de seguridad tiene responsabilidades operativas, necesitan permisos adicionales para realizar sus trabajos.

Puede usar [RBAC](../role-based-access-control/overview.md) para asignar permisos a los usuarios, grupos y aplicaciones en un ámbito determinado. El ámbito de una asignación de roles puede ser una suscripción, un grupo de recursos o un único recurso.

**Procedimiento recomendado**: Separar los deberes del equipo y conceder únicamente el nivel de acceso a los usuarios que necesitan para realizar sus trabajos. En lugar de proporcionar todos los empleados permisos no restringidos en su suscripción de Azure o recursos, permitir solo determinadas acciones en un ámbito determinado.
**Detalles**: Use [roles RBAC integrados](../role-based-access-control/built-in-roles.md) en Azure para asignar privilegios a los usuarios.

> [!Note]
> Permisos específicos crean complejidad innecesaria y confusión, acumular en una configuración "heredada" que es difícil corregir sin riesgo de romper algo. Evite los permisos específicos de recursos. En su lugar, utilice grupos de administración para grupos de recursos para los permisos dentro de las suscripciones y los permisos de toda la empresa. Evite los permisos específicos del usuario. En su lugar, asigne acceso a grupos en Azure AD.

**Procedimiento recomendado**: Conceder a los equipos con acceso de las responsabilidades de Azure para ver los recursos de Azure para que puedan evaluar y corregir el riesgo de seguridad.
**Detalles**: Conceder a los equipos de seguridad el RBAC [lector de seguridad](../role-based-access-control/built-in-roles.md#security-reader) rol. Puede usar el grupo de administración raíz o el grupo de administración de segmento, según el ámbito de responsabilidades:

- **Grupo de administración raíz** para los equipos responsables de todos los recursos de empresa
- **Grupo de administración de segmentos** para los equipos con un ámbito limitado (normalmente debido a límites organizativos legales u otros)

**Procedimiento recomendado**: Conceda los permisos adecuados para los equipos de seguridad que tienen responsabilidades operativas directas.
**Detalles**: Revise los roles integrados de RBAC para la asignación de rol adecuado. Si los roles integrados no satisfacen las necesidades específicas de su organización, puede crear [roles personalizados para recursos de Azure](../role-based-access-control/custom-roles.md). Como con los roles integrados, puede asignar roles personalizados a los usuarios, grupos y entidades de servicio en la suscripción, grupo de recursos y ámbitos de recursos.

**Procedimientos recomendados**: Acceso de concesión de Azure Security Center con roles de seguridad que lo necesiten. Security Center permite a los equipos de seguridad rápidamente identificar los riesgos y corregirlos.
**Detalles**: Agregue los equipos con estas necesidades de seguridad para el RBAC [Administrador de seguridad](../role-based-access-control/built-in-roles.md#security-admin) rol para que puedan ver las directivas de seguridad, ver estados de seguridad, editar las directivas de seguridad, ver alertas y recomendaciones y descartar alertas y recomendaciones. Puede hacerlo mediante el grupo de administración raíz o el grupo de administración de segmento, según el ámbito de las responsabilidades.

Organizaciones que no apliquen el control de acceso de datos mediante el uso de funcionalidades como RBAC es posible que estén concediendo más privilegios de los necesarios a sus usuarios. Esto puede poner en peligro datos permitiendo a los usuarios tener acceso a los tipos de datos (por ejemplo, un fuerte impacto empresarial) que no les conciernen.

## <a name="lower-exposure-of-privileged-accounts"></a>Menor exposición de las cuentas con privilegios

La protección del acceso con privilegios es un primer paso esencial para proteger los recursos empresariales. Minimizar el número de personas que tienen acceso a información segura o a recursos reduce la posibilidad de que los usuarios malintencionados obtengan acceso o de que un usuario autorizado, sin darse cuenta, afecte a un recurso confidencial.

Las cuentas con privilegios son cuentas que administran los sistemas de TI. Estas cuentas son el objetivo de los ciberatacantes, ya que les proporcionan acceso a los datos y los sistemas de una organización. Para proteger el acceso con privilegios, debe aislar las cuentas y los sistemas del riesgo de exposición a usuarios malintencionados.

Recomendamos el desarrollo y seguimiento de una hoja de ruta a fin de proteger el acceso con privilegios de los ciberatacantes. Para obtener información acerca de cómo crear una hoja de ruta detallada para proteger las identidades y el acceso que se administran o notifican en Azure AD, Microsoft Azure, Office 365 y otros servicios en la nube, revise el artículo [Protección del acceso con privilegios para las implementaciones híbridas y en la nube en Azure AD](../active-directory/users-groups-roles/directory-admin-roles-secure.md).

A continuación, se resumen los procedimientos recomendados que se encuentran en el artículo [Protección del acceso con privilegios para las implementaciones híbridas y en la nube en Azure AD](../active-directory/users-groups-roles/directory-admin-roles-secure.md):

**Procedimiento recomendado**: Administración, control y supervisión del acceso a las cuentas con privilegios.   
**Detalles**: active [Azure AD Privileged Identity Management](../active-directory/privileged-identity-management/active-directory-securing-privileged-access.md). Tras activar Privileged Identity Management, recibirá mensajes de correo electrónico de notificación si se producen cambios en el rol de acceso con privilegios. Estas notificaciones muestran una advertencia anticipada cuando se agregan más usuarios a roles con privilegios elevados en el directorio.

**Procedimiento recomendado**: Asegúrese de que todas las cuentas de administrador críticas se administran cuentas de Azure AD.
**Detalles**: Quite las cuentas de consumidor críticos roles de administrador (por ejemplo, cuentas de Microsoft, como hotmail.com, live.com y outlook.com).

**Procedimiento recomendado**: Asegúrese de que todos los roles de administrador críticos tienen una cuenta independiente para las tareas administrativas con el fin de evitar la suplantación de identidad y otros ataques para poner en peligro los privilegios administrativos.
**Detalles**: Cree una cuenta de administración independiente que tiene asignados los privilegios necesarios para realizar las tareas administrativas. Bloquea el uso de estas cuentas administrativas para las herramientas de productividad diarias como correo electrónico de Microsoft Office 365 o la exploración web arbitrario.

**Procedimiento recomendado**: Identificación y clasificación de las cuentas que están en roles con privilegios elevados.   
**Detalles**: después de activar Azure AD Privileged Identity Management, vea los usuarios que están en los roles de administrador global, con privilegios y otros con privilegios elevados. Quite todas las cuentas que no sean necesarias en los roles y clasifique las restantes que estén asignadas a roles de administrador:

- Se asigna de forma individual a los usuarios administrativos y se puede usar para fines no administrativos (por ejemplo, correo electrónico personal)
- Asignadas individualmente a usuarios administrativos y designadas solo para fines administrativos
- Compartidas entre varios usuarios
- Para los escenarios de acceso de emergencia
- Para scripts automatizados
- Para usuarios externos

**Procedimiento recomendado**: Implementar el acceso "Just-In-Time (JIT)" para reducir el tiempo de exposición de privilegios aún más y aumentar la visibilidad sobre el uso de cuentas con privilegios.   
**Detalles**: Azure AD Privileged Identity Management le permite:

- Limitar a los usuarios a aceptar solo sus privilegios JIT.
- Asignar roles para una duración reducida con confianza de que los privilegios se revocan automáticamente.

**Procedimiento recomendado**: Definición de un mínimo de dos de cuentas de acceso de emergencia.   
**Detalles**: las cuentas de acceso de emergencia ayudan a las organizaciones a restringir el acceso con privilegios en un entorno de Azure Active Directory existente. Estas cuentas tienen privilegios elevados y no se asignan a usuarios específicos. Las cuentas de acceso de emergencia se limitan a situaciones en las que no se pueden usar las cuentas administrativas normales. Las organizaciones deben limitar el uso de la cuenta de emergencia a la cantidad de tiempo necesaria únicamente.

Evalúe las cuentas que están asignadas al rol de administrador global o que son aptas para él. Si no ve que ninguna cuenta que se use solo en la nube mediante el dominio `*.onmicrosoft.com` (destinado al acceso de emergencia), créelas. Para más información, consulte [Administración de cuentas administrativas de acceso de emergencia en Azure AD](../active-directory/users-groups-roles/directory-emergency-access.md).

**Procedimiento recomendado**: Tiene un proceso de "emergencia" en su lugar en caso de emergencia.
**Detalles**: Siga los pasos de [privilegios de protección de acceso para las implementaciones híbridas y en la nube de Azure AD](../active-directory/users-groups-roles/directory-admin-roles-secure.md).

**Procedimiento recomendado**: Requerir que todas las cuentas de administrador crítica sea sin contraseña (opción preferida) o requerir autenticación multifactor.
**Detalles**: Use la [aplicación Microsoft Authenticator](../active-directory/authentication/howto-authentication-phone-sign-in.md) para iniciar sesión en cualquier cuenta de Azure AD sin utilizar una contraseña. Al igual que [Windows Hello para empresas](https://docs.microsoft.com/windows/security/identity-protection/hello-for-business/hello-identity-verification), Microsoft Authenticator usa la autenticación basada en claves para habilitar una credencial de usuario que está asociada a un dispositivo y usa la autenticación biométrica o un PIN.

Requerir autenticación multifactor de Azure en el inicio de sesión único para todos los usuarios individuales que están permanentemente asignadas a uno o varios de los roles de administrador de Azure AD: Administrador global, Administrador de roles con privilegios, Administrador de Exchange Online y Administrador de SharePoint Online. Habilitar [Multi-factor Authentication para sus cuentas de administrador](../active-directory/authentication/howto-mfa-userstates.md) y asegúrese de que los usuarios de cuentas de administrador se han registrado.

**Procedimiento recomendado**: Para las cuentas de administrador críticos, tener una estación de trabajo de administración donde no se permiten tareas de producción (por ejemplo, exploración y correo electrónico). Este modo, protegerá sus cuentas de administrador de vectores de ataque que usan el correo electrónico y exploración y disminuyen considerablemente el riesgo de un incidente principal.
**Detalles**: Use una estación de trabajo de administración. Elija un nivel de seguridad de la estación de trabajo:

- Dispositivos de productividad muy seguro proporcionan seguridad avanzada para la exploración y otras tareas de productividad.
- [Con privilegios de estaciones de trabajo de acceso (Paw)](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations) proporcionan un sistema operativo dedicado que está protegido contra ataques de internet y vectores de amenazas para tareas delicadas.

**Procedimiento recomendado**: Desaprovisionamiento de cuentas de administrador cuando los empleados abandonen la organización.
**Detalles**: Tiene un proceso en su lugar que deshabilita o elimina las cuentas de administrador cuando los empleados abandonen la organización.

**Procedimiento recomendado**: Probar con regularidad las cuentas de administrador mediante el uso de técnicas de ataque actual.
**Detalles**: Use el simulador de ataque de Office 365 o un oferta para ejecutar escenarios de ataque realista de la organización de terceros. Esto puede ayudarle a encontrar vulnerable a los usuarios antes de que se produzca un ataque real.

**Procedimiento recomendado**: Pasos necesarios para mitigar las técnicas de ataque usadas más frecuentemente.  
**Detalles**: [identifique las cuentas de Microsoft que tengan roles administrativos y que deban cambiarse a cuentas profesionales o educativas](../active-directory/users-groups-roles/directory-admin-roles-secure.md#identify-microsoft-accounts-in-administrative-roles-that-need-to-be-switched-to-work-or-school-accounts)  

[Asegúrese de que haya cuentas de usuario independientes y de que se lleve a cabo el reenvío de correos electrónicos para las cuentas de administrador globales](../active-directory/users-groups-roles/directory-admin-roles-secure.md)  

[Asegúrese de que las contraseñas de las cuentas administrativas hayan cambiado recientemente.](../active-directory/users-groups-roles/directory-admin-roles-secure.md#ensure-the-passwords-of-administrative-accounts-have-recently-changed)  

[Active la sincronización de hashes de contraseñas](../active-directory/users-groups-roles/directory-admin-roles-secure.md#turn-on-password-hash-synchronization)  

[Requiera Multi-Factor Authentication a los usuarios de todos los roles con privilegios, así como a los usuarios expuestos](../active-directory/users-groups-roles/directory-admin-roles-secure.md#require-multi-factor-authentication-mfa-for-users-in-all-privileged-roles-as-well-as-exposed-users)  

[Obtenga su Office 365 Secure Score (si usa Office 365)](../active-directory/users-groups-roles/directory-admin-roles-secure.md#obtain-your-office-365-secure-score-if-using-office-365)  

[Consulte la guía de seguridad y cumplimiento de Office 365 (si usa Office 365)](../active-directory/users-groups-roles/directory-admin-roles-secure.md#review-the-office-365-security-and-compliance-guidance-if-using-office-365)  

[Configure la supervisión de la actividad de Office 365 (si usa Office 365)](../active-directory/users-groups-roles/directory-admin-roles-secure.md#configure-office-365-activity-monitoring-if-using-office-365)  

[Establezca los propietarios del plan de respuesta ante incidentes o emergencias](../active-directory/users-groups-roles/directory-admin-roles-secure.md#establish-incidentemergency-response-plan-owners)  

[Proteja las cuentas administrativas con privilegios locales](../active-directory/users-groups-roles/directory-admin-roles-secure.md#turn-on-password-hash-synchronization)

Si no protege el acceso con privilegios, es posible que tenga demasiados usuarios en roles con privilegios elevados y sea más vulnerables a ataques. Los actores malintencionados, entre los que se incluyen ciberatacantes, a menudo tienen como objetivo las cuentas de administrador y otros elementos con acceso privilegiado para obtener acceso a datos confidenciales y a sistemas mediante el robo de credenciales.

## <a name="control-locations-where-resources-are-created"></a>Controlar las ubicaciones donde se crean los recursos

Es muy importante permitir que los operadores de nube realicen tareas pero al mismo tiempo impedir que transgredan las convenciones que son necesarias para administrar los recursos de la organización. Las organizaciones que quieran controlar las ubicaciones donde se crean los recursos deben codificarlas de forma segura.

Puede usar [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) para crear directivas de seguridad cuyas definiciones describan las acciones o los recursos que se deniegan específicamente. Esas definiciones de directivas se asignan en el ámbito deseado, como la suscripción, el grupo de recursos o un recurso individual.

> [!NOTE]
> Las directivas de seguridad no son las mismas que RBAC. En realidad, usan RBAC para autorizar a los usuarios la creación de estos recursos.
>
>

Las organizaciones que no controlan cómo se crean los recursos son más susceptibles a que los usuarios puedan hacer un mal uso del servicio y crear más recursos de los necesarios. Dificultar el proceso de creación de recursos es un paso importante para proteger escenarios en los que intervienen varios inquilinos.

## <a name="actively-monitor-for-suspicious-activities"></a>Supervisión activa de actividades sospechosas

Un sistema de supervisión de identidades activo puede detectar un comportamiento sospechoso y desencadenar una alerta para que se investigue. En la tabla siguiente se muestran dos de las funcionalidades de Azure AD que pueden ayudar a las organizaciones a supervisar sus identidades:

**Procedimiento recomendado**: Tener un método de identificación:

- Intentos para iniciar sesión [sin realizar ningún seguimiento](../active-directory/active-directory-reporting-sign-ins-from-unknown-sources.md).
- Ataques por [fuerza bruta](../active-directory/active-directory-reporting-sign-ins-after-multiple-failures.md) contra una cuenta determinada.
- Intentos para iniciar sesión desde varias ubicaciones.
- Inicios de sesión desde [dispositivos infectados](../active-directory/active-directory-reporting-sign-ins-from-possibly-infected-devices.md).
- Direcciones IP sospechosas.

**Detalles**: use los [informes de anomalías](../active-directory/active-directory-view-access-usage-reports.md) de Azure AD Premium. Contar con procesos y procedimientos implementados para que los administradores de TI ejecuten dichos informes diariamente o a petición (normalmente en un escenario de respuesta a incidentes).

**Procedimiento recomendado**: Contar con un sistema de supervisión activo que le informa de los riesgos y que puede ajustar el nivel de riesgo (alto, medio o bajo) a sus requisitos empresariales.   
**Detalles**: use [Azure AD Identity Protection](../active-directory/active-directory-identityprotection.md), que marca los riesgos actuales en su propio panel y envía notificaciones de resumen diarias por correo electrónico. Para ayudar a proteger las identidades de la organización, puede configurar directivas basadas en riesgos que respondan automáticamente a problemas detectados si se alcanza un nivel de riesgo específico.

Las organizaciones que no supervisen activamente sus sistemas de identidad corren el riesgo de comprometer las credenciales de los usuarios. Si las organizaciones no saben que están teniendo lugar actividades sospechosas a través de estas credenciales, no podrán mitigar este tipo de amenaza.

## <a name="use-azure-ad-for-storage-authentication"></a>Usar Azure AD para la autenticación de almacenamiento
[Almacenamiento de Azure](../storage/common/storage-auth-aad.md) admite la autenticación y autorización con Azure AD para Blob storage y Queue storage. Con la autenticación de Azure AD, puede usar el control de acceso de Azure basada en roles para conceder permisos específicos a los usuarios, grupos y aplicaciones hasta el ámbito de un contenedor de blobs individuales o una cola.

Se recomienda que use [Azure AD para autenticar el acceso al almacenamiento](https://azure.microsoft.com/blog/azure-storage-support-for-azure-ad-based-access-control-now-generally-available/).

## <a name="next-step"></a>Paso siguiente

Vea [Patrones y procedimientos recomendados de seguridad en Azure](security-best-practices-and-patterns.md) para obtener más procedimientos recomendados de seguridad que pueda aplicar cuando diseñe, implemente y administre las soluciones en la nube mediante Azure.
