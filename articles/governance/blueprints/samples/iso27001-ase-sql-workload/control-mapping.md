---
title: 'Ejemplo: plano técnico de cargas de trabajo de ASE o SQL según la norma ISO 27001 (asignación de controles)'
description: Asignación de controles del ejemplo de plano técnico para cargas de trabajo de App Service Environment y SQL Database compatibles con ISO 27001 para Azure Policy y RBAC.
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: sample
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 7a760bfe70fa2a83c43a0b41b77ba9bf45e809ca
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/08/2019
ms.locfileid: "59258614"
---
# <a name="control-mapping-of-the-iso-27001-asesql-workload-blueprint-sample"></a>Asignación de controles del ejemplo de plano técnico para cargas de trabajo de ASE y SQL compatibles con ISO 27001

En el siguiente artículo se detalla cómo el ejemplo de plano técnico para cargas de trabajo de ASE y SQL compatibles asigna los controles de ISO 27001. Para más información sobre los controles, consulte [ISO 27001](https://www.iso.org/isoiec-27001-information-security.html).

Las siguientes asignaciones son para los controles de **ISO 27001:2013**. Use el panel de navegación de la derecha para ir directamente a una asignación de control específica. Muchos de los controles asignados se implementan con una iniciativa de [Azure Policy](../../../policy/overview.md). Para revisar la iniciativa completa, abra **Policy** en Azure Portal y seleccione la página **Definiciones**. Después, busque y seleccione la iniciativa de directiva **[Versión preliminar] Auditar los controles ISO 27001:2013 e implementar extensiones de VM específicas para admitir los requisitos de auditoría** integrada.

## <a name="a612-segregation-of-duties"></a>A.6.1.2 Separación de obligaciones

Tener solo un propietario de la suscripción de Azure no permite la redundancia administrativa. Por el contrario, tener demasiados propietarios de suscripción de Azure aumenta la posibilidad de infracción de seguridad mediante una cuenta de propietario en riesgo. Este plano técnico ayuda a mantener un número adecuado de propietarios de suscripción de Azure mediante la asignación de dos definiciones de [Azure Policy](../../../policy/overview.md) que auditarán el número de estos. La administración de permisos de propietario de suscripción ayuda a implementar la separación adecuada de las tareas.

- [Vista previa]: Audit minimum number of owners for subscription
- [Vista previa]: Audit maximum number of owners for a subscription

## <a name="a912-access-to-networks-and-network-services"></a>A.9.1.2 Acceso a redes y servicios de red

Azure implementa el [control de acceso basado en rol](../../../../role-based-access-control/overview.md) (RBAC) para administrar quién tiene acceso a los recursos de Azure. Este plano técnico ayuda a controlar el acceso a los recursos de Azure mediante la asignación de siete definiciones de [Azure Policy](../../../policy/overview.md). Estas directivas auditan el uso de los tipos de recursos y las configuraciones que pueden permitir un acceso más flexible a los recursos.
Conocer los recursos que infringen estas directivas ayuda a tomar medidas correctivas para garantizar que el acceso a los recursos de Azure se limita a los usuarios autorizados.

- [Vista previa]: Deploy VM extension to audit Linux VM accounts with no passwords
- [Vista previa]: Deploy VM extension to audit Linux VM allowing remote connections from accounts with no
  contraseñass
- [Vista previa]: Audit Linux VM accounts with no passwords
- [Vista previa]: Audit Linux VM allowing remote connections from accounts with no passwords
- Auditar el uso de las cuentas de almacenamiento clásicas
- Auditar el uso de máquinas virtuales clásicas
- Auditar las máquinas virtuales que no utilizan discos administrados

## <a name="a922-user-access-provisioning"></a>A.9.2.2 Aprovisionamiento de acceso de usuario

Azure implementa el [control de acceso basado en rol](../../../../role-based-access-control/overview.md) (RBAC) para administrar quién tiene acceso a los recursos de Azure. Este plano técnico asigna tres definiciones de [Azure Policy](../../../policy/overview.md) para auditar el uso de la autenticación de [Azure Active Directory](../../../../active-directory/fundamentals/active-directory-whatis.md) para SQL Server y [Service Fabric](../../../../service-fabric/service-fabric-overview.md). El uso de la autenticación de Azure Active Directory simplifica la administración de permisos y centraliza la administración de identidades de usuarios de base de datos y otros servicios de Microsoft. Este plano técnico también asigna una definición de Azure Policy para auditar el uso de reglas RBAC personalizadas. Conocer dónde se implementan las reglas RBAC personalizadas ayuda a verificar que la implementación se necesita y se realiza correctamente, ya que estas suelen producir errores.

- Auditar el aprovisionamiento de un administrador de Azure Active Directory para SQL Server
- Auditar el uso de Azure Active Directory para la autenticación de clientes en Service Fabric
- Auditar el uso de reglas de RBAC personalizadas

## <a name="a923-management-of-privileged-access-rights"></a>A.9.2.3 Administración de los derechos de acceso con privilegios

Este plano técnico ayuda a restringir y controlar los derechos de acceso con privilegios mediante la asignación de cuatro definiciones de [Azure Policy](../../../policy/overview.md) para auditar las cuentas externas con propietario, o escribir permisos y cuentas con propietario o permisos que no tienen la autenticación multifactor habilitada.

- [Vista previa]: Audit accounts with owner permissions who are not MFA enabled on a subscription
- [Vista previa]: Audit accounts with write permissions who are not MFA enabled on a subscription
- [Vista previa]: Audit external accounts with owner permissions on a subscription
- [Vista previa]: Audit external accounts with write permissions on a subscription

## <a name="a924-management-of-secret-authentication-information-of-users"></a>A.9.2.4 Administración de la información de autenticación secreta de los usuarios

Este plano técnico asigna tres definiciones de [Azure Policy](../../../policy/overview.md) para auditar cuentas que no tienen la autenticación multifactor habilitada. La autenticación multifactor ayuda a mantener las cuentas seguras aunque algún dato de la autenticación esté en riesgo. Mediante la supervisión de las cuentas que no tienen la autenticación multifactor habilitada puede identificar las cuentas en mayor o menor riesgo. Este plano técnico también asigna dos definiciones de Azure Policy que auditan los permisos de los archivos con contraseña de máquina virtual Linux y alertan si la configuración es incorrecta. Esta configuración permite tomar medidas correctivas para garantizar que los autenticadores no están en riesgo.

- [Vista previa]: Audit accounts with owner permissions who are not MFA enabled on a subscription
- [Vista previa]: Audit accounts with read permissions who are not MFA enabled on a subscription
- [Vista previa]: Audit accounts with write permissions who are not MFA enabled on a subscription
- [Vista previa]: Deploy VM extension to audit Linux VM passwd file permissions
- [Vista previa]: Audit Linux VM /etc/passwd file permissions are set to 0644

## <a name="a925-review-of-user-access-rights"></a>A.9.2.5 Revisión de los derechos de acceso de los usuarios

Azure implementa el [control de acceso basado en rol](../../../../role-based-access-control/overview.md) (RBAC) para ayudar a administrar quién tiene acceso a los recursos en Azure. Mediante Azure Portal, puede revisar quién tiene acceso a los recursos de Azure y sus permisos. Este plano técnico asigna cuatro definiciones de [Azure Policy](../../../policy/overview.md) para auditar las cuentas que deberían priorizarse para la revisión, incluidas las cuentas en desuso y externas con permisos elevados.

- [Vista previa]: Audit deprecated accounts on a subscription
- [Vista previa]: Audit deprecated accounts with owner permissions on a subscription
- [Vista previa]: Audit external accounts with owner permissions on a subscription
- [Vista previa]: Audit external accounts with write permissions on a subscription

## <a name="a926-removal-or-adjustment-of-access-rights"></a>A.9.2.6 Eliminación o ajuste de los derechos de acceso

Azure implementa el [control de acceso basado en rol](../../../../role-based-access-control/overview.md) (RBAC) para ayudar a administrar quién tiene acceso a los recursos en Azure. Con [Azure Active Directory](../../../../active-directory/fundamentals/active-directory-whatis.md) y RBAC puede actualizar los roles de usuario para reflejar los cambios en la organización. Cuando sea necesario, se puede bloquear el inicio de sesión de las cuentas (o eliminar estas), lo cual eliminaría inmediatamente los derechos de acceso a los recursos de Azure. Este plano técnico asigna dos definiciones de [Azure Policy](../../../policy/overview.md) para auditar las cuentas en desuso cuya eliminación debería considerarse.

- [Vista previa]: Audit deprecated accounts on a subscription
- [Vista previa]: Audit deprecated accounts with owner permissions on a subscription

## <a name="a943-password-management-system"></a>A.9.4.3 Sistema de administración de contraseñas

Este plano técnico ayuda a instaurar contraseñas seguras mediante la asignación de diez definiciones de [Azure Policy](../../../policy/overview.md) que auditan las máquinas virtuales Windows que no tienen unos requisitos mínimos de seguridad de contraseña y de otros tipos. Conocer las máquinas virtuales que infringen la directiva de seguridad de las contraseñas ayuda a tomar medidas correctivas para garantizar que las contraseñas de todas las cuentas de usuario de máquina virtual cumplen la directiva.

- [Vista previa]: Deploy VM extension to audit Windows VM enforces password complexity requirements
- [Vista previa]: Deploy VM extension to audit Windows VM maximum password age 70 days
- [Vista previa]: Deploy VM extension to audit Windows VM minimum password age 1 day
- [Vista previa]: Deploy VM extension to audit Windows VM passwords must be at least 14 characters
- [Vista previa]: Deploy VM extension to audit Windows VM should not allow previous 24 passwords
- [Vista previa]: Audit Windows VM enforces password complexity requirements
- [Vista previa]: Audit Windows VM maximum password age 70 days
- [Vista previa]: Audit Windows VM minimum password age 1 day
- [Vista previa]: Audit Windows VM passwords must be at least 14 characters
- [Vista previa]: Audit Windows VM should not allow previous 24 passwords

## <a name="a1011-policy-on-the-use-of-cryptographic-controls"></a>A.10.1.1 Directiva sobre el uso de controles criptográficos

Este plano técnico ayuda a instaurar la directiva sobre el uso de controles criptográficos mediante la asignación de trece definiciones de [Azure Policy](../../../policy/overview.md) que instauran controles criptográficos específicos y auditan el uso de configuraciones criptográficas poco seguras.
Conocer dónde los recursos de Azure pueden tener configuraciones criptográficas subóptimas ayuda a tomar medidas correctivas para garantizar que los recursos se configuran conforme a la directiva de seguridad de la información. En concreto, las directivas asignadas por este plano técnico requieren cifrado para las cuentas de Blob Storage y Data Lake Storage; cifrado de datos transparente en las bases de datos SQL; auditan el cifrado que falta en las cuentas de almacenamiento, las bases de datos SQL, los discos de máquina virtual y las variables de cuenta de Automation; auditan las conexiones no seguras a cuentas de almacenamiento, Function App, Web Apps, API Apps, y Redis Cache; auditan el cifrado de contraseñas no seguras de las máquinas virtuales y la comunicación sin cifrado de Service Fabric.

- [Vista previa]: Audit HTTPS only access for a Function App
- [Vista previa]: Audit HTTPS only access for a Web Application
- [Vista previa]: Audit HTTPS only access for an API App
- [Vista previa]: Audit missing blob encryption for storage accounts
- [Vista previa]: Deploy VM extension to audit Windows VM should not store passwords using reversible
  El cifradon
- [Vista previa]: Audit Windows VM should not store passwords using reversible encryption
- [Vista previa]: Monitor unencrypted SQL database in Azure Security Center
- [Vista previa]: Monitor unencrypted VM Disks in Azure Security Center
- Auditar la habilitación del cifrado de variables de cuentas de Automation
- Auditar la habilitación solo de conexiones seguras a Redis Cache
- Auditar la transferencia segura a cuentas de almacenamiento
- Auditar la configuración de la propiedad ClusterProtectionLevel como EncryptAndSign en Service Fabric
- Auditar el estado del cifrado de datos transparente

## <a name="a1241-event-logging"></a>A.12.4.1 Registro de eventos

Este plano técnico ayuda a garantizar que los eventos del sistema se registran; para ello, se asignan siete definiciones de [Azure Policy](../../../policy/overview.md) que auditan las configuraciones de registro de los recursos de Azure. Una directiva asignada también audita si las máquinas virtuales no envían registros a un área de trabajo de Log Analytics específica.

- [Versión preliminar]: Auditoría de la implementación de Dependency Agent: la imagen de la VM (SO) no está en la lista
- [Versión preliminar]: Auditoría de la implementación de Dependency Agent en VMSS: la imagen de la VM (SO) no está en la lista
- [Versión preliminar]: Auditoría de la implementación del agente de Log Analytics: la imagen de la VM (SO) no está en la lista
- [Versión preliminar]: Auditoría de la implementación del agente de Log Analytics en VMSS: la imagen de la VM (SO) no está en la lista
- [Vista previa]: Monitor unaudited SQL database in Azure Security Center
- Auditar la configuración de diagnóstico
- Auditar configuración de auditoría de SQL en el nivel de servidor

## <a name="a121-management-of-technical-vulnerabilities"></a>A.12.1 Administración de vulnerabilidades técnicas

Este plano técnico ayuda a administrar las vulnerabilidades del sistema de información al asignar cinco definiciones de [Azure Policy](../../../policy/overview.md) que supervisan si faltan actualizaciones del sistema, o si existen vulnerabilidades del sistema operativo, de SQL o de las máquinas virtuales. Esta información proporciona detalles en tiempo real sobre el estado de seguridad de los recursos implementados y ayuda a priorizar las medidas correctivas.

- [Vista previa]: Monitor missing Endpoint Protection in Azure Security Center
- [Vista previa]: Monitor missing system updates in Azure Security Center
- [Vista previa]: Monitor OS vulnerabilities in Azure Security Center
- [Vista previa]: Monitor SQL vulnerability assessment results in Azure Security Center
- [Vista previa]: Monitor VM Vulnerabilities in Azure Security Center

## <a name="a1262-restrictions-on-software-installation"></a>A.12.6.2 Restricciones de instalación de software

El control de aplicaciones adaptable es la solución de Azure Security Center que permite controlar qué aplicaciones pueden ejecutarse en sus máquinas virtuales ubicadas en Azure. Este plano técnico asigna una definición de Azure Policy que supervisa los cambios en el conjunto de aplicaciones permitidas. Las restricciones de instalación de software pueden ayudar a reducir la probabilidad de que se introduzcan vulnerabilidades de software.

- [Vista previa]: Monitor possible app Whitelisting in Azure Security Center

## <a name="a1311-network-controls"></a>A.13.1.1 Controles de red

Este plano técnico ayuda a administrar y controlar las redes; para ello, asigna una definición de [Azure Policy](../../../policy/overview.md) que supervisa los grupos de seguridad de red con reglas permisivas. Las reglas demasiado permisivas pueden permitir el acceso de red no deseado y deben revisarse.

- [Vista previa]: Monitor permissive network access in Azure Security Center

## <a name="a1321-information-transfer-policies-and-procedures"></a>A.13.2.1 Directivas y procedimientos de transferencia de información

El plano técnico ayuda a garantizar que la transferencia de información con los servicios de Azure es segura; para ello, asigna dos definiciones de [Azure Policy](../../../policy/overview.md) para auditar las conexiones no seguras a las cuentas de almacenamiento y a Redis Cache.

- Auditar la habilitación solo de conexiones seguras a Redis Cache
- Auditar la transferencia segura a cuentas de almacenamiento

## <a name="a1413-protecting-application-services-transactions"></a>A.14.1.3 Protección de las transacciones de los servicios de aplicación

Este plano técnico ayuda a proteger los recursos del sistema de información; para ello, asigna tres definiciones de [Azure Policy](../../../policy/overview.md) que supervisan los puntos de conexión, las aplicaciones y las cuentas de almacenamiento desprotegidos. Los puntos de conexión y las aplicaciones que no están protegidos por firewall y las cuentas de almacenamiento con acceso sin restricciones pueden permitir el acceso no deseado a la información contenida en el sistema de información.

- [Vista previa]: Monitor unprotected network endpoints in Azure Security Center
- [Vista previa]: Monitor unprotected web application in Azure Security Center
- Auditar el acceso de red sin restricciones a cuentas de almacenamiento

## <a name="a1613-reporting-information-security-weaknesses"></a>A.16.1.3 Informes sobre las vulnerabilidades de seguridad de la información

Este plano técnico ayuda a conocer las vulnerabilidades del sistema; para ello, asigna cinco definiciones de [Azure Policy](../../../policy/overview.md) que supervisan las vulnerabilidades, el estado de la revisión y alertas de malware en Azure Security Center. Azure Security Center proporciona funcionalidades de informes que permiten tener información en tiempo real sobre el estado de seguridad de los recursos de Azure implementados.

- [Vista previa]: Monitor missing Endpoint Protection in Azure Security Center
- [Vista previa]: Monitor missing system updates in Azure Security Center
- [Vista previa]: Monitor OS vulnerabilities in Azure Security Center
- [Vista previa]: Monitor SQL vulnerability assessment results in Azure Security Center
- [Vista previa]: Monitor VM Vulnerabilities in Azure Security Center

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha revisado la asignación de controles del ejemplo de plano técnico para cargas de trabajo de App Service Environment y SQL Database compatibles con ISO 27001, lea los siguientes artículos para aprender sobre la arquitectura y la implementación de este ejemplo:

> [!div class="nextstepaction"]
> [Plano técnico para cargas de trabajo de App Service Environment y SQL Database compatibles con la norma ISO 27001: introducción](./index.md)
> [Plano técnico para cargas de trabajo de App Service Environment y SQL Database compatibles con la norma ISO 27001: pasos de implementación](./deploy.md)

Artículos adicionales sobre planos técnicos y cómo utilizarlos:

- Más información sobre el [ciclo de vida del plano técnico](../../concepts/lifecycle.md)
- Descubra cómo utilizar [parámetros estáticos y dinámicos](../../concepts/parameters.md).
- Aprenda a personalizar el [orden de secuenciación de planos técnicos](../../concepts/sequencing-order.md).
- Averigüe cómo usar el [bloqueo de recursos de planos técnicos](../../concepts/resource-locking.md).
- Aprenda a [actualizar las asignaciones existentes](../../how-to/update-existing-assignments.md).