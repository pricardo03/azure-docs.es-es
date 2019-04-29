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
ms.date: 09/17/2018
ms.author: barclayn
ms.openlocfilehash: f872c61ad0597d2307cd244668fdfc258f7a45cb
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60611248"
---
# <a name="azure-identity-management-and-access-control-security-best-practices"></a>Procedimientos recomendados para la administración de identidades y la seguridad del control de acceso en Azure

Muchos consideran que la identidad es la nueva capa límite de la seguridad, asumiendo ese papel desde la perspectiva tradicional centrada en la red. Esta evolución del eje principal de la atención y las inversiones en materia de seguridad procede del hecho de que los perímetros de red se han vuelto cada vez más porosos y que la defensa perimetral no puede ser tan eficaz como en su día lo fue, antes de la explosión de los dispositivos [BYOD](https://aka.ms/byodcg) y las aplicaciones de nube.

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
* Habilitar el inicio de sesión único
* Activar el acceso condicional
* Habilitación de la administración de contraseñas
* Exigir a los usuarios la verificación multifactor
* Uso del control de acceso basado en rol
* Menor exposición de las cuentas con privilegios
* Controlar las ubicaciones donde se encuentran los recursos

## <a name="treat-identity-as-the-primary-security-perimeter"></a>Tratar las amenazas como el perímetro de seguridad principal

Muchos consideran que la identidad es el perímetro principal para la seguridad. Se trata de un cambio desde el enfoque tradicional de seguridad de red. Los perímetros de red continúan volviéndose más porosos y esa defensa perimetral no puede ser tan eficaz como lo era antes de la explosión de dispositivos y aplicaciones en la nube de [BYOD](https://aka.ms/byodcg).
[Azure Active Directory (Azure AD)](../active-directory/active-directory-whatis.md) es la solución de Azure para la administración de identidades y de acceso. Azure AD es un servicio de administración de identidades y directorios multiinquilino basado en la nube de Microsoft. Combina servicios de directorio fundamentales, la administración del acceso a las aplicaciones y la protección de identidades en una única solución.

En las siguientes secciones se enumeran los procedimientos recomendados de seguridad de acceso e identidades con Azure AD.

## <a name="centralize-identity-management"></a>Centralizar la administración de identidades

En un escenario de [identidad híbrida](https://resources.office.com/ww-landing-M365E-EMS-IDAM-Hybrid-Identity-WhitePaper.html?), se recomienda integrar los directorios en el entorno local y en la nube. La integración permite al equipo de TI administrar las cuentas desde una única ubicación, independientemente de donde se crea una cuenta. La integración también hace que los usuarios sean más productivos, ya que proporciona una identidad común para tener acceso a recursos de la nube y del entorno local.


**Procedimiento recomendado**: Integración de los directorios locales con Azure AD.  
**Detalles**: use [Azure AD Connect](../active-directory/connect/active-directory-aadconnect.md) para sincronizar el directorio local con el directorio en la nube.

**Procedimiento recomendado**: Activación de la sincronización de hashes de contraseñas.  
**Detalles**: La sincronización de hashes de contraseñas es una característica que se utiliza para sincronizar los hashes de los hashes de las contraseñas de los usuarios desde una instancia de Active Directory local con una instancia de Azure AD en la nube.

Aunque decida usar la federación con Servicios de federación de Active Directory (AD FS) u otros proveedores de identidades, si quiere, puede configurar la sincronización de hashes de contraseñas para tener una opción alternativa si los servidores locales sufren un error o dejan de estar disponibles temporalmente. Esto permite que los usuarios inicien sesión en el servicio con la misma contraseña que usan para iniciar sesión en su instancia local de Active Directory. También permite que Identity Protection detecte las credenciales que están en peligro mediante la comparación de los algoritmos hash de dichas contraseñas con contraseñas que se sepa que están en peligro, si un usuario ha usado su misma dirección de correo electrónico y contraseña en otros servicios que no estén conectados a Azure AD.

Para más información, consulte [Implement password hash synchronization with Azure AD Connect sync](../active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md) (Implementación de la sincronización de hash de contraseñas mediante la sincronización de Azure AD Connect).

Las organizaciones que no integren la identidad del entorno local con la identidad en la nube pueden tener mayor sobrecarga para administrar cuentas. Esta sobrecarga aumenta la probabilidad de que haya errores e infracciones de seguridad.

## <a name="enable-single-sign-on"></a>Habilitar el inicio de sesión único

En un mundo donde la nube y la movilidad son lo primero, resulta útil habilitar el inicio de sesión único (SSO) en dispositivos, aplicaciones y servicios desde cualquier sitio, de modo que los usuarios puedan ser productivos en cualquier momento y lugar. Cuando es necesario administrar varias soluciones de identidad, esto supone un problema administrativo no solo para TI, sino también para los usuarios que tendrán que recordar varias contraseñas.

Mediante el uso de la misma solución de identidad para todas las aplicaciones y los recursos, podrá disfrutar del SSO. Además, los usuarios podrán usar el mismo conjunto de credenciales para iniciar sesión y acceder a los recursos que necesitan, con independencia de dónde se ubiquen estos recursos, tanto si es en el entorno local como en la nube.

**Procedimiento recomendado**: habilitar SSO.  
**Detalles**: Azure AD [extiende Active Directory del entorno local](../active-directory/connect/active-directory-aadconnect.md) a la nube. Los usuarios pueden usar su cuenta profesional o educativa principal para los dispositivos unidos a un dominio, los recursos de la empresa y todas las aplicaciones web y SaaS que necesitan para realizar su trabajo. Los usuarios no tienen que recordar varios nombres de usuario y contraseñas y el acceso a las aplicaciones por parte de los usuarios se puede aprovisionar (o desaprovisionar) automáticamente, en función de su pertenencia a los grupos de la organización y de su estado como empleado. Además, puede controlar el acceso de las aplicaciones de la galería o de sus propias aplicaciones locales que ha desarrollado y publicado mediante el [proxy de la aplicación de Azure AD](../active-directory/active-directory-application-proxy-get-started.md).

Use el SSO para permitir que los usuarios accedan a sus [aplicaciones SaaS](../active-directory/active-directory-appssoaccess-whatis.md) en función de su cuenta profesional o educativa en Azure AD. Esto no solo es aplicable a las aplicaciones para SaaS de Microsoft, sino también a otras aplicaciones, como [Google Apps](../active-directory/active-directory-saas-google-apps-tutorial.md) y [Salesforce](../active-directory/active-directory-saas-salesforce-tutorial.md). Puede configurar su aplicación de modo que use Azure AD como un proveedor de [identidades basado en SAML](../active-directory/fundamentals-identity.md). Como control de seguridad, Azure AD no emite ningún token que permita a los usuarios iniciar sesión en la aplicación, a menos que se les conceda acceso mediante Azure AD. Puede concederles acceso directamente o a través de un grupo al cual pertenezcan.

Las organizaciones que no crean ninguna identidad común para establecer el SSO para sus usuarios y aplicaciones están más expuestas a escenarios donde los usuarios tienen varias contraseñas. Estos escenarios aumentan la probabilidad de que los usuarios reutilicen las contraseñas o usen contraseñas débiles.

## <a name="turn-on-conditional-access"></a>Activar el acceso condicional

Los usuarios pueden acceder a los recursos de su organización mediante diversos dispositivos y aplicaciones desde cualquier lugar. Como administrador de TI, quiere asegurarse de que estos dispositivos cumplan los estándares de seguridad y cumplimiento. Ya no es suficiente con centrarse en quién puede acceder a un recurso.

Para equilibrar la seguridad y la productividad, también debe pensar en cómo se accede a un recurso antes de que pueda tomar una decisión de control de acceso. Con el acceso condicional de Azure AD, puede abordar este requisito. Con el acceso condicional, puede tomar decisiones de control de acceso automatizadas para acceder a las aplicaciones en la nube en función de condiciones.

**Procedimiento recomendado**: Administración y control del acceso a los recursos corporativos.  
**Detalles**: configure el [acceso condicional](../active-directory/active-directory-conditional-access-azure-portal.md) de Azure AD en función del grupo, la ubicación y la confidencialidad de la aplicación para las aplicaciones SaaS y las aplicaciones conectadas a Azure AD.

## <a name="enable-password-management"></a>Habilitación de la administración de contraseñas

Si tiene varios inquilinos o quiere permitir que los usuarios [restablezcan su propia contraseña](../active-directory/active-directory-passwords-update-your-own-password.md), es importante utilizar directivas de seguridad adecuadas para evitar un uso inadecuado.

**Procedimiento recomendado**: Configuración del autoservicio de restablecimiento de contraseña (SSPR) para los usuarios.  
**Detalles**: use la característica de [autoservicio de restablecimiento de contraseña](../active-directory-b2c/active-directory-b2c-reference-sspr.md) de Azure AD.

**Procedimiento recomendado**: Supervisar cómo se usa realmente SSPR o si se puede usar.  
**Detalles**: supervise los usuarios que se registran mediante el [informe de actividad de registro de restablecimiento de contraseña](../active-directory/active-directory-passwords-get-insights.md) de Azure AD. La característica de creación de informes que proporciona Azure AD le ayuda a responder preguntas mediante informes creados previamente. Si está debidamente protegido por licencia, también puede crear consultas personalizadas.

## <a name="enforce-multi-factor-verification-for-users"></a>Exigir a los usuarios la verificación multifactor

Se recomienda exigir la verificación en dos pasos a todos los usuarios. Esto incluye a los administradores y otras personas de su organización, ya que el hecho de que su cuenta esté en peligro puede tener un impacto significativo (por ejemplo, los directores financieros).

Existen varias opciones para exigir la verificación en dos pasos. La mejor opción para usted depende de sus objetivos, la edición de Azure AD que ejecuta y su programa de licencias. Consulte [Exigencia de verificación en dos pasos para un usuario](../active-directory/authentication/howto-mfa-userstates.md) para determinar la mejor opción para usted. Puede encontrar más información sobre licencias y precios en las páginas de precios de [Azure AD](https://azure.microsoft.com/pricing/details/active-directory/) y [Azure Multi-Factor Authentication](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

A continuación, se indican las opciones y ventajas para habilitar la verificación en dos pasos:

**Opción 1**: [habilitar Multi-factor Authentication mediante el cambio de estado de usuario](../active-directory/authentication/howto-mfa-userstates.md).   
**Ventaja**: este es el método tradicional para exigir la verificación en dos pasos. Funciona tanto con [Azure Multi-factor Authentication en la nube como en el Servidor Azure Multi-factor Authentication](../active-directory/authentication/concept-mfa-whichversion.md). El uso de este método requiere que los usuarios realicen la verificación en dos pasos cada vez que inicien sesión e invalida las directivas de acceso condicional.

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

## <a name="use-role-based-access-control-rbac"></a>Uso del control de acceso basado en rol (RBAC)

En organizaciones que quieren aplicar directivas de seguridad para el acceso a los datos, es imperativo restringir el acceso en función de los principios de seguridad [necesidad de saber](https://en.wikipedia.org/wiki/Need_to_know) y [mínimo privilegio](https://en.wikipedia.org/wiki/Principle_of_least_privilege). Puede usar el [control de acceso basado en rol (RBAC)](../role-based-access-control/overview.md) para asignar permisos a los usuarios, los grupos y las aplicaciones en un ámbito determinado. El ámbito de una asignación de roles puede ser una suscripción, un grupo de recursos o un único recurso.

Puede aprovechar los roles de [RBAC integrados](../role-based-access-control/built-in-roles.md) en Azure para asignar privilegios a los usuarios. Es posible que las organizaciones que no apliquen el control de acceso a los datos mediante el uso de funcionalidades como RBAC estén concediendo más privilegios de los necesarios a sus usuarios. Esto puede poner en peligro los datos al permitir que los usuarios accedan a determinados tipos de datos (por ejemplo, aquellos que son críticos para la empresa) a los que no deberían tener acceso.

## <a name="lower-exposure-of-privileged-accounts"></a>Menor exposición de las cuentas con privilegios

La protección del acceso con privilegios es un primer paso esencial para proteger los recursos empresariales. Minimizar el número de personas que tienen acceso a información segura o a recursos reduce la posibilidad de que los usuarios malintencionados obtengan acceso o de que un usuario autorizado, sin darse cuenta, afecte a un recurso confidencial.

Las cuentas con privilegios son cuentas que administran los sistemas de TI. Estas cuentas son el objetivo de los ciberatacantes, ya que les proporcionan acceso a los datos y los sistemas de una organización. Para proteger el acceso con privilegios, debe aislar las cuentas y los sistemas del riesgo de exposición a usuarios malintencionados.

Recomendamos el desarrollo y seguimiento de una hoja de ruta a fin de proteger el acceso con privilegios de los ciberatacantes. Para obtener información acerca de cómo crear una hoja de ruta detallada para proteger las identidades y el acceso que se administran o notifican en Azure AD, Microsoft Azure, Office 365 y otros servicios en la nube, revise el artículo [Protección del acceso con privilegios para las implementaciones híbridas y en la nube en Azure AD](../active-directory/users-groups-roles/directory-admin-roles-secure.md).

A continuación, se resumen los procedimientos recomendados que se encuentran en el artículo [Protección del acceso con privilegios para las implementaciones híbridas y en la nube en Azure AD](../active-directory/users-groups-roles/directory-admin-roles-secure.md):

**Procedimiento recomendado**: Administración, control y supervisión del acceso a las cuentas con privilegios.   
**Detalles**: active [Azure AD Privileged Identity Management](../active-directory/privileged-identity-management/active-directory-securing-privileged-access.md). Tras activar Privileged Identity Management, recibirá mensajes de correo electrónico de notificación si se producen cambios en el rol de acceso con privilegios. Estas notificaciones muestran una advertencia anticipada cuando se agregan más usuarios a roles con privilegios elevados en el directorio.

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

Evalúe las cuentas que están asignadas al rol de administrador global o que son aptas para él. Si no ve que ninguna cuenta que se use solo en la nube mediante el dominio `*.onmicrosoft.com` (destinado al acceso de emergencia), créelas. Para obtener más información, consulte Administración de cuentas administrativas de acceso de emergencia en Azure AD.

**Procedimiento recomendado**: Activación de la autenticación multifactor y registro de las restantes cuentas de administrador no federadas de usuario único con privilegios elevados.  
**Detalles**: requiera Azure Multi-Factor Authentication en el inicio de sesión a todos los usuarios individuales a los que se haya asignado permanentemente uno o varios de los roles de administrador de Azure AD: administrador global, administrador de rol con privilegios, administrador de Exchange Online y administrador de SharePoint Online. Use la guía para habilitar [Multi-Factor Authentication en sus cuentas de administrador](../active-directory/authentication/howto-mfa-userstates.md) y asegúrese de que todos esos usuarios se hayan [registrado](https://aka.ms/mfasetup).

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

## <a name="next-step"></a>Paso siguiente

Vea [Patrones y procedimientos recomendados de seguridad en Azure](security-best-practices-and-patterns.md) para obtener más procedimientos recomendados de seguridad que pueda aplicar cuando diseñe, implemente y administre las soluciones en la nube mediante Azure.
