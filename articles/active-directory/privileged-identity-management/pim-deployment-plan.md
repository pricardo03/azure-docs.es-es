---
title: Implementación de Azure AD Privileged Identity Management (PIM) | Microsoft Docs
description: Se describe cómo planear la implementación de Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.service: active-directory
ms.topic: conceptual
ms.workload: identity
ms.subservice: pim
ms.date: 02/08/2019
ms.author: rolyon
ms.custom: ''
ms.collection: M365-identity-device-management
ms.openlocfilehash: 54fa8d09d930069191fb48e0ab015d436496b725
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2019
ms.locfileid: "56166409"
---
# <a name="deploy-azure-ad-privileged-identity-management-pim"></a>Implementación de Azure AD Privileged Identity Management (PIM)

En esta guía detallada se describe cómo planear la implementación de Azure AD Privileged Identity Management (PIM) en su organización.

> [!TIP]
> En este documento, verá los elementos marcados como:
>
> :heavy_check_mark: **Microsoft recomienda**
>
> Estas son recomendaciones generales que solo debe implementar si se aplican a sus necesidades empresariales específicas.

## <a name="step-1-learn-about-pim"></a>Paso 1. Más información sobre PIM

Azure AD Privileged Identity Management (PIM) le permite administrar roles con privilegios administrativos en Azure AD, recursos de Azure y otros servicios de Microsoft Online Services. En un escenario en el que las identidades con privilegios se asignan y olvidan, PIM ofrece soluciones como acceso de tipo Just-In-Time, flujos de trabajo de aprobación de solicitudes y revisiones de acceso completamente integradas para que pueda identificar, detectar y evitar en tiempo real las actividades malintencionadas de roles con privilegios. La implementación de PIM para administrar los roles con privilegios en toda la organización reducirá notablemente el riesgo al presentar información importante acerca de las actividades de sus roles con privilegios.

### <a name="business-value-of-pim"></a>Valor empresarial de PIM

**Administrar riesgos**: proteja su organización al aplicar el principio de [acceso con privilegios mínimos](/windows-server/identity/ad-ds/plan/security-best-practices/implementing-least-privilege-administrative-models) y acceso Just-In-Time. Al reducir el número de asignaciones permanentes de usuarios a roles con privilegios y aplicar aprobaciones y MFA para elevación, puede reducir considerablemente los riesgos de seguridad relativos al acceso con privilegios en su organización. La aplicación de acceso con privilegios mínimos y Just-In-Time también permitirá ver un historial de acceso a roles con privilegios y realizar un seguimiento de los problemas de seguridad cuando ocurran.

**Abordar el cumplimiento y el gobierno**: la implementación de PIM crea un entorno para el gobierno de identidades en curso. La elevación Just-In-Time de identidades con privilegios ofrece una forma para que PIM realice un seguimiento de las actividades de acceso con privilegios en la organización. También podrá ver y recibir notificaciones de todas las asignaciones de roles aptos y permanentes dentro de la organización. Mediante una revisión de acceso, puede auditar y eliminar con regularidad las identidades con privilegios innecesarias y asegurarse de que su organización cumple las normas de seguridad, acceso e identidad más estrictas.

**Reducir los costos**: reduzca los costos mediante la eliminación de deficiencias, errores humanos y problemas de seguridad mediante la implementación de PIM correctamente. El resultado neto es una reducción de los delitos cibernéticos asociados con identidades con privilegios, de los que resulta difícil y costoso recuperarse. PIM también ayudará a su organización a reducir los costos asociados con la auditoría de la información de acceso a efectos de cumplir con reglamentos y normas.

Para más información, vea [¿Qué es Azure AD Privileged Identity Management?](pim-configure.md).

### <a name="licensing-requirements"></a>Requisitos de concesión de licencia

Para usar PIM, el directorio debe tener una de las siguientes licencias de pago o de evaluación:

- Azure AD Premium P2
- Enterprise Mobility + Security (EMS) E5
- Microsoft 365 M5

Para más información, consulte los [Requisitos de licencia para usar PIM](subscription-requirements.md).

### <a name="key-pim-terminology"></a>Terminología clave de PIM

| Término o concepto | DESCRIPCIÓN |
| --- | --- |
| Apto | Asignación de roles que requiere que un usuario realice una o varias acciones para usar el rol. Si un usuario es apto para un rol, eso significa que puede activarlo cuando necesite para realizar tareas con privilegios. No hay ninguna diferencia en el acceso proporcionado de forma permanente a una persona o una asignación de roles aptos. La única diferencia es que algunas personas no necesitan ese acceso todo el tiempo. |
| activar | Proceso de realizar una o varias acciones para usar un rol para el que es apto un usuario. Entre las acciones se puede incluir realizar una comprobación de autenticación multifactor (MFA), proporcionar una justificación de negocios o solicitar la aprobación de los aprobadores designados. |
| Acceso Just-In-Time (JIT) | Modelo en el que los usuarios reciben permisos temporales para realizar tareas con privilegios, lo que impide que usuarios malintencionados o sin autorización obtengan acceso después de que el permiso haya expirado. El acceso se concede solo cuando los usuarios lo necesitan. |
| Principio de acceso con privilegios mínimos | Una práctica de seguridad recomendada en la que se proporcionan a todos los usuarios únicamente los privilegios mínimos necesarios para realizar las tareas que están autorizados a realizar. Esta práctica minimiza el número de administradores globales, ya que en su lugar utiliza roles de administrador específicos para determinados escenarios. |

Para más información, vea [Terminología](pim-configure.md#terminology).

### <a name="high-level-overview-of-how-pim-works"></a>Información general de alto nivel sobre el funcionamiento de PIM

1. PIM se configura para que los usuarios sean aptos para roles con privilegios.
1. Cuando un usuario apto necesita utilizar sus roles con privilegios, se activa el rol en PIM.
1. Según la configuración de PIM seleccionada para el rol, el usuario debe completar algunos pasos (por ejemplo, realizar la autenticación multifactor, obtener la aprobación o especificar un motivo).
1. Una vez que el usuario activa correctamente su rol, obtendrá el rol durante un período de tiempo configurado previamente.
1. Los administradores pueden ver un historial de todas las actividades de PIM en el registro de auditoría. También pueden proteger más a sus inquilinos y satisfacer el cumplimiento con las características de PIM, como las revisiones de acceso y las alertas.

Para más información, vea [¿Qué es Azure AD Privileged Identity Management?](pim-configure.md).

### <a name="roles-that-can-be-managed-by-pim"></a>Roles que se pueden administrar con PIM

**Roles de Azure AD**: estos roles son todos los roles de directorio dentro de Azure Active Directory (como administrador global, administrador de Exchange y administrador de seguridad). Puede leer más sobre los roles y sus funcionalidades en [Permisos de roles de administrador en Azure Active Directory](../users-groups-roles/directory-assign-admin-roles.md). Para obtener ayuda con la determinación de qué roles asignar a los administradores, consulte [Roles con privilegios mínimos por tarea](../users-groups-roles/roles-delegate-by-task.md).

**Roles de recursos de Azure**: estas funciones están vinculadas a un recurso, grupo de recursos, suscripción o grupo de administración de Azure. PIM ofrece acceso Just-In-Time a ambos roles integrados, como propietario, administrador de acceso de usuario y colaborador, así como [roles personalizados](../../role-based-access-control/custom-roles.md). Para más información sobre roles de recursos de Azure, vea [Control de acceso basado en rol (RBAC)](../../role-based-access-control/overview.md).

Para más información, vea [Roles que no puede administrar en PIM](pim-roles.md).

## <a name="step-2-plan-your-deployment"></a>Paso 2. Planeamiento de la implementación

Esta sección se centra en lo que necesita hacer antes de implementar PIM en su organización. Es fundamental seguir las instrucciones y comprender los conceptos de esta sección, ya que le guiarán en la creación del mejor plan adaptado a las identidades con privilegios de la organización.

### <a name="identify-your-stakeholders"></a>Identificación de las partes interesadas

La siguiente sección le ayuda a identificar a todas las partes interesadas que intervienen en el proyecto y necesitan aprobar, revisar o mantenerse informados. Incluye tablas independientes para implementar PIM para roles de Azure AD y PIM para roles de recursos de Azure. Agregue partes interesadas a la siguiente tabla según proceda para su organización.

- SO = aprobar este proyecto
- R = revisar este proyecto y proporcionar entrada
- I = Informado de este proyecto

#### <a name="stakeholders-pim-for-azure-ad-roles"></a>Partes interesadas: PIM para roles de Azure AD

| NOMBRE | Rol | . |
| --- | --- | --- |
| Nombre y correo electrónico | **Arquitecto de identidades o administrador global de Azure**<br/>Un representante del equipo de administración de identidades responsable de definir cómo este cambio se alinea con la infraestructura de administración de identidades principal de su organización. | SO/R/I |
| Nombre y correo electrónico | **Propietario del servicio/Administrador de línea**<br/>Un representante de los propietarios de TI de un servicio o un grupo de servicios. Son fundamentales para tomar decisiones y ayudar a implementar PIM para su equipo. | SO/R/I |
| Nombre y correo electrónico | **Propietario de seguridad**<br/>Un representante del equipo de seguridad que puede aprobar que el plan cumple los requisitos de seguridad de la organización. | SO/R |
| Nombre y correo electrónico | **Administrador de soporte técnico de TI o departamento de soporte técnico**<br/>Un representante de la organización de soporte técnico de TI que puede proporcionar información sobre la compatibilidad de este cambio desde una perspectiva del departamento de soporte técnico. | R/I |
| Nombre y correo electrónico de los usuarios piloto | **Usuarios de roles con privilegios**<br/>El grupo de usuarios para el que se implementa la administración de identidades con privilegios. Necesitan saber cómo activar sus roles una vez que se implementa PIM. | I |

#### <a name="stakeholders-pim-for-azure-resource-roles"></a>Partes interesadas: PIM para roles de recursos de Azure

| NOMBRE | Rol | . |
| --- | --- | --- |
| Nombre y correo electrónico | **Propietario de recurso/suscripción**<br/>Un representante de los propietarios de TI de cada suscripción o recurso para los que desea implementar PIM. | SO/R/I |
| Nombre y correo electrónico | **Propietario de seguridad**<br/>Un representante del equipo de seguridad que puede aprobar que el plan cumple los requisitos de seguridad de la organización. | SO/R |
| Nombre y correo electrónico | **Administrador de soporte técnico de TI o departamento de soporte técnico**<br/>Un representante de la organización de soporte técnico de TI que puede proporcionar información sobre la compatibilidad de este cambio desde una perspectiva del departamento de soporte técnico. | R/I |
| Nombre y correo electrónico de los usuarios piloto | **Usuarios de roles RBAC**<br/>El grupo de usuarios para el que se implementa la administración de identidades con privilegios. Necesitan saber cómo activar sus roles una vez que se implementa PIM. | I |

### <a name="enable-pim"></a>Habilitación de PIM

Como parte del proceso de planeamiento, primero debe dar su consentimiento a PIM y habilitarlo según las indicaciones del [documento sobre cómo empezar a utilizar PIM](pim-getting-started.md). Al habilitar PIM, puede acceder a algunas características diseñadas específicamente para facilitar la implementación.

Si su objetivo es implementar PIM para recursos de Azure, debe seguir las indicaciones del [documento sobre la detección de recursos de Azure que se administran en PIM](pim-resource-roles-discover-resources.md). Solo los propietarios de cada recurso, grupo de recursos y suscripción podrán detectarlos dentro de PIM. Si es un administrador global e intenta implementar PIM para los recursos de Azure, puede [elevar el acceso para administrar todas las suscripciones de Azure](../../role-based-access-control/elevate-access-global-admin.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json) para tener acceso a todos los recursos de Azure en el directorio a fin de poder detectarlos. Sin embargo, se recomienda obtener la aprobación de cada uno de los propietarios de las suscripciones antes de administrar sus recursos con PIM.

### <a name="enforce-principle-of-least-privilege"></a>Aplicación del principio de privilegio mínimo

Es importante asegurarse de que ha aplicado el principio de privilegio mínimo en la organización para los roles de recursos de Azure y Azure AD.

#### <a name="azure-ad-roles"></a>Roles de Azure AD

Para los roles de Azure AD, es habitual que las organizaciones asignen el rol de administrador global a un número elevado de administradores, cuando la mayoría de ellos solo necesitan uno o dos roles de administrador específicos. Las asignaciones de roles con privilegios también tienden a dejarse sin administrar.

> [!NOTE]
> Errores habituales en la delegación de roles:
>
> - El administrador a cargo de Exchange tiene el rol de administrador global, aunque solo necesita el rol de administrador de Exchange para realizar su trabajo diario.
> - El rol de administrador global se asigna a un administrador de Office porque el administrador necesita roles de administrador de seguridad y de SharePoint y resulta más fácil delegar un solo rol.
> - Al administrador se le asignó un rol de administrador de seguridad para realizar una tarea hace mucho tiempo, pero nunca se ha quitado.

Siga estos pasos para aplicar el principio de privilegio mínimo para los roles de Azure AD.

1. Para conocer la granularidad de los roles, lea y entienda los [roles de administrador de Azure AD disponibles](../users-groups-roles/directory-assign-admin-roles.md#available-roles). Usted y su equipo también deben consultar [Roles de administrador por tarea de administrador en Azure Active Directory](../users-groups-roles/roles-delegate-by-task.md), donde se explica el rol con privilegios mínimos para tareas específicas.

1. Especifique quién tiene roles con privilegios en su organización. Puede usar el [asistente de PIM](pim-security-wizard.md#run-the-wizard) para acceder a una página similar a la siguiente.

    ![Detección de roles con privilegios](./media/pim-deployment-plan/discover-privileged-roles-users.png)

1. Analice por qué todos los administradores globales de la organización necesitan el rol. Según la documentación leída anteriormente, si el trabajo de la persona de que se trata pueden realizarlo uno o varios roles de administrador granulares, debe quitarles el rol de administrador global y realizar las asignaciones correspondientes dentro de Azure Active Directory (como referencia: actualmente, Microsoft tiene solo unos diez administradores con el rol de administrador global. Obtenga más información en el artículo sobre [cómo Microsoft usa PIM](https://www.microsoft.com/itshowcase/Article/Content/887/Using-Azure-AD-Privileged-Identity-Management-for-elevated-access)).

1. Para todos los demás roles de Azure AD, revise la lista de asignaciones, identifique a los administradores que ya no necesitan el rol y quítelos de sus asignaciones.

Para automatizar los pasos 3 y 4, puede utilizar la función de revisión de acceso dentro de PIM. Con los pasos descritos en [Inicio de una revisión de acceso para los roles de directorio de Azure AD en PIM](pim-how-to-start-security-review.md), puede configurar una revisión de acceso de cada rol de Azure AD que tenga uno o varios miembros.

![Creación de una revisión de acceso](./media/pim-deployment-plan/create-access-review.png)

Debe establecer los revisores como **Miembros (autoasignado)**. De esta forma, se enviará un correo electrónico a todos los miembros del rol para que confirmen si necesitan acceso. También debe activar **Requerir motivo de la aprobación** en la configuración avanzada para que los usuarios puedan indicar por qué necesitan el rol. Según esta información, podrá eliminar a los usuarios de los roles innecesarios y delegar más roles de administrador granulares en el caso de los administradores globales.

Las revisiones de acceso se basan en los correos electrónicos para recordar a los usuarios que deben revisar su acceso a los roles. Si tiene cuentas con privilegios que no tienen correos electrónicos vinculados, asegúrese de rellenar el campo de correo electrónico secundario de dichas cuentas. Para más información, vea la información sobre el [atributo proxyAddresses en Azure AD](https://support.microsoft.com/help/3190357/how-the-proxyaddresses-attribute-is-populated-in-azure-ad).

#### <a name="azure-resource-roles"></a>Roles de recursos de Azure

Para las suscripciones y los recursos de Azure, puede configurar un proceso de revisión de acceso similar para revisar los roles de cada suscripción o recurso. El objetivo de este proceso es minimizar las asignaciones de administrador de acceso de usuario y propietario conectadas a cada suscripción o recurso, así como quitar las asignaciones innecesarias. Sin embargo, las organizaciones suelen delegar dichas tareas al propietario de cada suscripción o recurso porque este conoce mejor los roles específicos (sobre todo los soles personalizados).

Si es un administrador de TI con el rol de administrador global e intenta implementar PIM para los recursos de Azure en su organización, puede [elevar el acceso para administrar todas las suscripciones de Azure](../../role-based-access-control/elevate-access-global-admin.md?toc=%2fazure%2factive-directory%2fprivileged-identity-management%2ftoc.json) para poder acceder a cada suscripción. A continuación, puede encontrar a cada propietario de la suscripción y trabajar con él para quitar las asignaciones innecesarias y minimizar la asignación del rol de propietario.

Los usuarios con el rol de propietario para una suscripción de Azure también pueden utilizar las [revisiones de acceso para recursos de Azure](pim-resource-roles-start-access-review.md) a fin de auditar y quitar las asignaciones de roles innecesarias con un proceso similar al descrito anteriormente para los roles de Azure AD.

### <a name="decide-which-role-assignments-should-be-protected-by-pim"></a>Decidir qué asignaciones de roles deben protegerse mediante PIM

Después de limpiar las asignaciones de roles con privilegios en su organización, deberá decidir qué roles desea proteger con PIM.

Si un rol está protegido con PIM, los usuarios aptos asignados a él deben elevarse para usar los privilegios concedidos por el rol. El proceso de elevación también puede incluir la obtención de la aprobación, la realización de la autenticación multifactor y la explicación del motivo por el que se realiza la activación. PIM también puede realizar un seguimiento de las elevaciones mediante notificaciones y los registros de eventos de auditoría de PIM y Azure AD.

La elección de los roles que se desean proteger con PIM puede resultar un proceso difícil que variará para cada organización. En esta sección se proporciona asesoramiento sobre procedimientos recomendados para roles de Azure y Azure AD.

#### <a name="azure-ad-roles"></a>Roles de Azure AD

Es importante dar prioridad a la protección de los roles de Azure AD que tienen el mayor número de permisos. Según los patrones de uso entre todos los clientes de PIM, los diez roles principales de Azure AD administrados por PIM son:

1. Administrador global
1. Administrador de seguridad
1. Administrador de cuenta de usuario
1. Administrador de Exchange
1. Administrador de SharePoint
1. Administrador de servicios de Intune
1. Lector de seguridad
1. Administrador de servicios
1. Administrador de facturación
1. Administrador de Skype Empresarial

> [!TIP]
> :heavy_check_mark: **Microsoft recomienda** administrar todos los administradores globales y administradores de seguridad mediante PIM como primer paso, ya que son los roles que pueden causar más daños al vulnerar su seguridad.

Es importante tener en cuenta qué datos y permisos son más sensibles para su organización. Por ejemplo, puede que algunas organizaciones deseen proteger su rol de administrador de Power BI o su rol de administrador de equipos con PIM, ya que tienen la capacidad de acceder a datos o cambiar los flujos de trabajo principales.

Si existen roles con usuarios invitados asignados, serán particularmente vulnerables a ataques.

> [!TIP]
> :heavy_check_mark: **Microsoft recomienda** administrar todos los roles con usuarios invitados mediante PIM para reducir los riesgos asociados a las cuentas de usuario invitado en peligro.

Los roles de lector, como lector de directorio, lector del Centro de mensajes y lector de seguridad, a veces se consideran menos importantes en comparación con otros roles porque no tienen permiso de escritura. Sin embargo, se ha observado que algunos clientes también protegen estos roles porque los atacantes que consiguen acceder a estas cuentas pueden leer información confidencial, como la información de identificación personal (DCP). También debe tener esto en cuenta al decidir si los roles de lector de la organización deben administrase con PIM.

#### <a name="azure-resource-roles"></a>Roles de recursos de Azure

Al decidir qué asignaciones de roles deben administrarse mediante PIM para recursos de Azure, primero debe identificar las suscripciones y los recursos que son más vitales para su organización. Ejemplos de tales suscripciones y recursos son:

- Recursos que hospedan la información más confidencial.
- Recursos de los que dependen las principales aplicaciones orientadas al cliente.

Si es un administrador global y tiene problemas para decidir qué suscripciones y recursos son más importantes, debe ponerse en contacto con los propietarios de la suscripción de su organización para recopilar una lista de los recursos administrados por cada suscripción. A continuación, debe colaborar con los propietarios de la suscripción para agrupar los recursos en función del nivel de gravedad en caso de que estén en peligro (bajo, medio o alto). Debe dar prioridad a la administración de recursos con PIM en función de este nivel de gravedad.

> [!TIP]
> :heavy_check_mark: **Microsoft recomienda** colaborar con los propietarios de las suscripciones y recursos de servicios críticos para configurar el flujo de trabajo de PIM de todos los roles dentro de suscripciones y recursos confidenciales.

PIM para recursos de Azure es compatible con las cuentas de servicio de tiempo limitado. Debe tratar las cuentas de servicio exactamente de la misma forma en que trataría una cuenta de usuario normal.

En el caso de las suscripciones y los recursos que no son tan críticos, no necesitará configurar PIM para todos los roles. Sin embargo, todavía debe proteger los roles de administrador de acceso de usuario y propietario con PIM.

> [!TIP]
> :heavy_check_mark: **Microsoft recomienda** administrar los roles de propietario y de administrador de acceso de usuario de todas las suscripciones y recursos mediante PIM.

### <a name="decide-which-role-assignments-should-be-permanent-or-eligible"></a>Decidir qué asignaciones de roles deben ser permanentes o válidas

Una vez que haya decidido la lista de roles que se deben administrar con PIM, debe decidir qué usuarios deben obtener el rol válido frente al rol activo permanentemente. Los roles activos permanentemente son roles normales asignados mediante recursos de Azure y Azure Active Directory, mientras que los roles válidos solo se pueden asignar en PIM.

> [!TIP]
> :heavy_check_mark: **Microsoft recomienda** tener cero asignaciones activas permanentemente para los roles de Azure AD y los roles de recursos de Azure aparte de las [dos cuentas de acceso para escenarios de máxima emergencia](../users-groups-roles/directory-emergency-access.md) recomendadas, que deben tener el rol de administrador global permanente.

Aunque se recomienda no tener ningún administrador permanente, a veces resulta complicado para las organizaciones conseguir esto con rapidez. A continuación, se indican los aspectos que se deben tener en cuenta al tomar esta decisión:

- Frecuencia de elevación: si el usuario solo necesita la asignación con privilegios una vez, no debería tener la asignación permanente. Por otro lado, si el usuario necesita el rol para su trabajo cotidiano y el uso de PIM reduciría considerablemente su productividad, se puede considerar asignarle el rol permanente.
- Casos específicos de su organización: si la persona a la que se va asignar el rol válido pertenece a un equipo muy remoto o se trata de un ejecutivo de alto rango, hasta el punto que la comunicación y la aplicación del proceso de elevación resultan complicadas, se puede considerar asignarle el rol permanente.

> [!TIP]
> :heavy_check_mark: **Microsoft recomienda** configurar revisiones de acceso recurrentes para los usuarios con asignaciones de roles permanentes (en caso de que haya alguno). Obtenga más información sobre la revisión de acceso recurrente en la última sección de este plan de implementación.

### <a name="draft-your-pim-settings"></a>Borrador de la configuración de PIM

Antes de implementar la solución PIM, es aconsejable realizar un borrador de la configuración de PIM para cada rol con privilegios que su organización usa. Esta sección incluye algunos ejemplos de configuración de PIM para roles particulares (son solo para referencia y pueden ser diferentes para su organización). Cada una de estas configuraciones se explica de forma detallada con las recomendaciones de Microsoft proporcionadas después de las tablas.

#### <a name="pim-settings-for-azure-ad-roles"></a>Configuración de PIM para roles de Azure AD

| Rol | Requerir MFA | Notificación | Vale de incidente | Requerir aprobación | Aprobador | Duración de la activación | Administrador permanente |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Administrador global | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | Otros administradores globales | 1 hora | Cuentas de acceso de emergencia |
| Administrador de Exchange | :heavy_check_mark: | :heavy_check_mark: | :x: | :x: | None | 2 horas | None |
| Administrador del departamento de soporte técnico | :x: | :x: | :heavy_check_mark: | :x: | None | 8 horas | None |

#### <a name="pim-settings-for-azure-resource-roles"></a>Configuración de PIM para roles de recursos de Azure

| Rol | Requerir MFA | Notificación | Requerir aprobación | Aprobador | Duración de la activación | Administrador activo | Expiración de rol activo | Expiración de rol válido |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Propietario de las suscripciones críticas | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | Otros propietarios de la suscripción | 1 hora | None | N/D | 3 meses |
| Administrador de acceso de usuario de suscripciones menos críticas | :heavy_check_mark: | :heavy_check_mark: | :x: | None | 1 hora | None | N/D | 3 meses |
| Colaborador de la máquina virtual | :x: | :heavy_check_mark: | :x: | None | 3 horas | None | N/D | 6 meses |

En la tabla siguiente se describe cada configuración.

| Configuración | DESCRIPCIÓN |
| --- | --- |
| Rol | Nombre del rol para el que se define la configuración. |
| Requerir MFA | Si el usuario apto necesita realizar MFA antes de activar el rol.<br/><br/>:heavy_check_mark: **Microsoft recomienda** aplicar MFA para todos los roles de administrador, especialmente si los roles tienen usuarios invitados. |
| Notificación | Si se establece en true, el administrador global, el administrador de roles con privilegios y el administrador de seguridad de la organización recibirán una notificación por correo electrónico cuando un usuario apto activa el rol.<br/><br/>**Nota:** Algunas organizaciones no tienen una dirección de correo electrónico asociada a sus cuentas de administrador para obtener estas notificaciones por correo electrónico; para obtenerlas, debe establecer una dirección de correo electrónico alternativa para que los administradores reciban estos correos. |
| Vale de incidente | Si el usuario apto necesita registrar un número de vale de incidente al activar el rol. Esta configuración ayuda a una organización a identificar cada activación con un número de incidente interno para mitigar las activaciones no deseadas.<br/><br/>:heavy_check_mark: **Microsoft recomienda** sacar provecho de los números de vales de incidentes para vincular PIM con el sistema interno. Esto es especialmente útil para los aprobadores que necesitan contexto para la activación. |
| Requerir aprobación | Si el usuario apto necesita obtener aprobación para activar el rol.<br/><br/>:heavy_check_mark: **Microsoft recomienda** configurar la aprobación con el permiso máximo. Según los patrones de uso de todos los clientes de PIM, los roles de administrador global, administrador de usuarios, administrador de Exchange, administrador de seguridad y administrador de contraseñas son los más comunes con la configuración de la aprobación. |
| Aprobador | Si se requiere aprobación para activar el rol válido, indique qué personas deben aprobar la solicitud. De forma predeterminada, PIM establece que el aprobador sean todos los usuarios que sean administradores de roles con privilegios, ya sean permanentes o válidos.<br/><br/>**Nota:** Si un usuario es apto para un rol de Azure AD y un aprobador del rol, no podrá realizar aprobaciones para sí mismo.<br/><br/>:heavy_check_mark: **Microsoft recomienda** elegir como aprobadores a quienes conozcan mejor el rol específico y sus usuarios frecuentes, en lugar de a un administrador global. |
| Duración de la activación | El período de tiempo durante el cual el usuario estará activado en el rol antes de que este expire. |
| Administrador permanente | Enumere los usuarios que serán administradores permanentes del rol (nunca tienen que ser activos).<br/><br/>:heavy_check_mark: **Microsoft recomienda** tener cero administradores permanente para todos los roles, excepto en el caso de los administradores globales. Lea más información al respecto en la sección de este plan que trata sobre quién debe establecerse como válido y quién como activo permanentemente. |
| Administrador activo | Para recursos de Azure, el administrador activo es la lista de usuarios que nunca tendrán que ser activos para usar el rol. Esto no se conoce como administrador permanente como en los roles de Azure AD, porque puede establecer un tiempo de expiración para cuándo el usuario perderá este rol. |
| Expiración de rol activo | Una asignación de roles activos para roles de recursos de Azure expirará una vez transcurrido este período de tiempo configurado. Puede elegir entre 15 días, 1 mes, 3 meses, 6 meses, 1 año o activo permanentemente. |
| Expiración de rol válido | Una asignación de roles válidos para roles de recursos de Azure expirará una vez transcurrido este período de tiempo configurado. Puede elegir entre 15 días, 1 mes, 3 meses, 6 meses, 1 año o válido permanentemente. |

## <a name="step-3-implement-your-solution"></a>Paso 3. Implementación de la solución

La base para un planeamiento adecuado es aquella sobre la que se puede implementar una aplicación correctamente con Azure Active Directory.  Proporciona una integración y una seguridad inteligentes, lo que simplifica la incorporación a la vez que reduce el tiempo necesario para realizar implementaciones correctas.  Esta combinación garantiza una integración sencilla de la aplicación, a la vez que mitiga el tiempo de inactividad para los usuarios finales.

### <a name="identify-test-users"></a>Identificación de los usuarios de prueba

Utilice esta sección para identificar un conjunto de usuarios y grupos de usuarios para validar la implementación. Según la configuración seleccionada en la sección de planeamiento, identifique a los usuarios que desea probar para cada rol.

> [!TIP]
> :heavy_check_mark: **Microsoft recomienda** establecer a los propietarios del servicio de cada rol de Azure AD como usuarios de prueba para que estos se familiaricen con el proceso y se conviertan en un autor interno de la implementación.

En esta tabla, identifique a los usuarios de prueba que verificarán si la configuración de cada rol funciona.

| Nombre de rol | Usuarios de prueba |
| --- | --- |
| &lt;Nombre de rol&gt; | &lt;Usuarios para probar el rol&gt; |
| &lt;Nombre de rol&gt; | &lt;Usuarios para probar el rol&gt; |

### <a name="test-implementation"></a>Prueba de la implementación

Ahora que ha identificado a los usuarios de prueba, use este paso para configurar PIM para los usuarios de prueba. Si la organización desea incorporar el flujo de trabajo de PIM en su propia aplicación interna en lugar de utilizar la interfaz de usuario de PIM dentro de Azure Portal, todas las operaciones de PIM también se admitirán con [Graph API](https://docs.microsoft.com/graph/api/resources/privilegedidentitymanagement-root).

#### <a name="configure-pim-for-azure-ad-roles"></a>Configuración de PIM para roles de Azure AD

1. [Defina la configuración del rol de directorio de Azure AD](pim-how-to-change-default-settings.md) en función de lo que haya planeado.

1. Vaya a **Roles de Azure AD**, haga clic en **Roles** y, a continuación, seleccione el rol que acaba de configurar.

1. Para el grupo de usuarios de prueba, si estos ya están establecidos como un administrador permanente, puede establecerlos como válidos buscándolos y convirtiéndolos de permanentes a válidos al hacer clic en los tres puntos de su fila. Si todavía no tienen ninguna asignación de roles, puede [realizar una nueva asignación válida](pim-how-to-add-role-to-user.md#make-a-user-eligible-for-a-role).

1. Repita los pasos 1 a 3 para todos los roles que desea probar.

1. Una vez haya configurado los usuarios de prueba, deberá volver a enviarles el vínculo para saber cómo [activar su rol de Azure AD](pim-how-to-activate-role.md).

#### <a name="configure-pim-for-azure-resource-roles"></a>Configuración de PIM para roles de recursos de Azure

1. [Defina la configuración del rol de recursos de Azure](pim-resource-roles-configure-role-settings.md) para un rol dentro de una suscripción o un recurso que desee probar.

1. Vaya a **Recursos de Azure** para esa suscripción y haga clic en **Roles** y, a continuación, seleccione el rol que acaba de configurar.

1. Para el grupo de usuarios de prueba, si ya son un administrador activo, puede convertirlos en válidos buscándolos y [actualizando su asignación de roles](pim-resource-roles-assign-roles.md#update-or-remove-an-existing-role-assignment). Si todavía no tienen ningún rol, puede [asignar uno nuevo](pim-resource-roles-assign-roles.md#assign-a-role).

1. Repita los pasos 1 a 3 para todos los roles que desea probar.

1. Una vez haya configurado los usuarios de prueba, deberá volver a enviarles el vínculo para saber cómo [activar su rol de recursos de Azure](pim-resource-roles-activate-your-roles.md).

Esta fase debe usarse para verificar si toda la configuración establecida para los roles funciona correctamente. Utilice la siguiente tabla para documentar las pruebas. También debe usar esta fase para optimizar la comunicación con los usuarios afectados.

| Rol | Comportamiento esperado durante la activación | Resultados reales |
| --- | --- | --- |
| Administrador global | (1) Requerir MFA<br/>(2) Requerir aprobación<br/>(3) El aprobador recibe la notificación y puede aprobar<br/>(4) El rol expira después del tiempo preestablecido |  |
| Propietario de la suscripción *X* | (1) Requerir MFA<br/>(2) La asignación válida expira después del período de tiempo configurado |  |

### <a name="communicate-pim-to-affected-stakeholders"></a>Comunicación de PIM con las partes interesadas afectadas

La implementación de PIM requerirá pasos adicionales para los usuarios de roles con privilegios. Aunque PIM reduce significativamente los problemas de seguridad asociados con las identidades con privilegios, es necesario comunicar el cambio de forma eficaz antes de la implementación en todo el espacio empresarial. Dependiendo del número de administradores afectados, las organizaciones a menudo optan por crear un documento interno, un vídeo o un correo electrónico sobre el cambio. En estas comunicaciones suele incluirse la siguiente información:

- ¿Qué es PIM?
- ¿Qué ventaja aporta para la organización?
- ¿A quién afectará?
- ¿Cuándo se implementará PIM?
- ¿Qué pasos adicionales serán necesarios para que los usuarios activen su rol?
    - Debe enviar vínculos a la documentación:
    - [Activación de roles de Azure AD](pim-how-to-activate-role.md)
    - [Activación de roles de recursos de Azure](pim-resource-roles-activate-your-roles.md)
- Información de contacto o vínculo al departamento de soporte técnico para problemas asociados con PIM

> [!TIP]
> :heavy_check_mark: **Microsoft recomienda** dedicar tiempo al equipo/departamento de soporte técnico para ofrecerle orientación sobre el flujo de trabajo de PIM (en caso de que su organización disponga de un equipo interno de soporte técnico de TI). Debe proporcionarle los documentos apropiados y su información de contacto.

### <a name="move-to-production"></a>Paso a producción

Una vez haya finalizado las pruebas satisfactoriamente, mueva PIM a producción repitiendo todos los pasos de las fases de prueba para todos los usuarios de cada rol que haya definido en la configuración de PIM. En el caso de PIM para roles de Azure AD, las organizaciones suelen probar e implementar PIM para administradores globales antes de probarlo e implementarlo para otros roles. En cambio, en el caso de los recursos de Azure, las organizaciones suelen probar e implementar PIM en una suscripción de Azure cada vez.

### <a name="in-the-case-a-rollback-is-needed"></a>En caso de que se necesite una reversión

Si PIM no funciona según lo previsto en el entorno de producción, los pasos de reversión siguientes pueden ayudarle a revertir a un estado correcto conocido antes de la configuración de PIM:

#### <a name="azure-ad-roles"></a>Roles de Azure AD

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
1. Abra **Azure AD Privileged Identity Management**.
1. Haga clic en **Roles de Azure AD** y, a continuación, haga clic en **Roles**.
1. Para cada rol que haya configurado, haga clic en los puntos suspensivos (**...**) correspondientes a todos los usuarios que tengan una asignación válida.
1. Haga clic en la opción **Hacer permanente** para que la asignación de roles sea permanente.

#### <a name="azure-resource-roles"></a>Roles de recursos de Azure

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
1. Abra **Azure AD Privileged Identity Management**.
1. Haga clic en **Recursos de Azure** y, a continuación, haga clic en una suscripción o un recurso que desee revertir.
1. Haga clic en **Roles**.
1. Para cada rol que haya configurado, haga clic en los puntos suspensivos (**...**) correspondientes a todos los usuarios que tengan una asignación válida.
1. Haga clic en la opción **Hacer permanente** para que la asignación de roles sea permanente.

## <a name="step-4-next-steps-after-deploying-pim"></a>Paso 4 Pasos siguientes después de implementar PIM

La implementación correcta de PIM en producción es un importante paso adelante para proteger las identidades con privilegios de su organización. La implementación de PIM ofrece características adicionales que debe usar para fines de seguridad y cumplimiento.

### <a name="use-pim-alerts-to-safeguard-your-privileged-access"></a>Uso de alertas de PIM para proteger el acceso con privilegios

Debe utilizar la funcionalidad integrada de alertas de PIM para proteger mejor su inquilino. Para más información, vea [Alertas de seguridad](pim-how-to-configure-security-alerts.md#security-alerts). Estas alertas se incluyen: los administradores no usan roles con privilegios, los roles se asignan fuera de PIM, se activan roles con demasiada frecuencia y muchas más. Para ofrecer una protección total a su organización, debe consultar regularmente la lista de alertas y corregir los problemas. Puede ver y corregir las alertas de la manera siguiente:

1. Inicie sesión en el [Azure Portal](https://portal.azure.com/).
1. Abra **Azure AD Privileged Identity Management**.
1. Haga clic en **Roles de Azure AD** y, luego, en **Alertas**.

> [!TIP]
> :heavy_check_mark: **Microsoft recomienda** solucionar inmediatamente todas las alertas marcadas con gravedad alta. En el caso de las alertas de seguridad de gravedad media y baja, debe mantenerse informado y realizar cambios si considera que existe una amenaza de seguridad.

Si alguna de las alertas específicas no es útil o no es aplicable a su organización, siempre puede descartarla en la página de alertas. Siempre puede revertir este descargue más tarde en la página de configuración de Azure AD.

### <a name="set-up-recurring-access-reviews-to-regularly-audit-your-organizations-privileged-identities"></a>Configuración de revisiones de acceso periódicas para auditar regularmente las identidades con privilegios de su organización

Las revisiones de acceso son la mejor forma de preguntar a los usuarios con roles con privilegios asignados o a revisores específicos si cada usuario necesita la identidad con privilegios. Las revisiones de acceso son excelentes si desea reducir la superficie expuesta a ataques y mantener el cumplimiento. Para más información sobre cómo iniciar una revisión de acceso, vea los artículos sobre [revisiones de acceso de roles de Azure AD](pim-how-to-start-security-review.md) y [revisiones de acceso de roles de recursos de Azure](pim-resource-roles-start-access-review.md). En algunas organizaciones, es necesario realizar revisiones de acceso periódicas para cumplir las leyes y los reglamentos, mientras que, en otras, las revisiones de acceso son la mejor manera de aplicar el principio del privilegio mínimo en toda la organización.

> [!TIP]
> :heavy_check_mark: **Microsoft recomienda** configurar revisiones de acceso trimestrales para todos los roles de Azure AD y de recursos de Azure.

En la mayoría de los casos, los revisores de los roles de Azure AD son los propios usuarios, mientras que los revisores de los roles de recursos de Azure son los propietarios de la suscripción en la que se encuentra el rol. Sin embargo, suele darse el caso en el que las empresas tienen cuentas con privilegios que no están vinculadas con la dirección de correo electrónico de una persona concreta. En esos casos, nadie lee ni revisa el acceso.

> [!TIP]
> :heavy_check_mark: **Microsoft recomienda** agregar una dirección de correo electrónico secundaria para todas las cuentas con asignaciones de roles con privilegios que no están vinculadas a una dirección de correo electrónico revisada con regularidad.

### <a name="get-the-most-out-of-your-audit-log-to-improve-security-and-compliance"></a>Sacar el máximo partido de su registro de auditoría para mejorar la seguridad y el cumplimiento

El registro de auditoría le permite mantenerse informado y cumplir los reglamentos. Actualmente, PIM almacena el historial de toda la organización durante 30 días dentro de su registro de auditoría, con información que incluye:

- Activación/desactivación de roles válidos
- Actividades de asignación de roles dentro y fuera de PIM
- Cambios en la configuración de roles
- Actividades de solicitud, aprobación o denegación de activación de roles con la configuración de aprobación
- Actualización de alertas

Puede acceder a estos registros de auditoría si es un administrador global o un administrador de roles con privilegios. Para más información, vea el [historial de auditoría para los roles de Azure AD](pim-how-to-use-audit-log.md) y el [historial de auditoría para los roles de recursos de Azure](azure-pim-resource-rbac.md).

> [!TIP]
> :heavy_check_mark: **Microsoft recomienda** tener al menos un administrador que lea todos los eventos de auditoría cada semana y los exporte una vez al mes.

Si desea almacenar automáticamente los eventos de auditoría durante un período de tiempo más largo, el registro de auditoría de PIM se sincroniza automáticamente en los [registros de auditoría de Azure AD](../reports-monitoring/concept-audit-logs.md).

> [!TIP]
> :heavy_check_mark: **Microsoft recomienda** configurar la [supervisión de registros de Azure](../reports-monitoring/concept-activity-logs-azure-monitor.md) para archivar los eventos de auditoría en una cuenta de almacenamiento de Azure para fines de seguridad y cumplimiento.
