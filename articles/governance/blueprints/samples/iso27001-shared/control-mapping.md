---
title: 'Asignación de Control de ejemplo: plano técnico de servicios compartidos de ISO 27001:'
description: Asignación de control de la muestra de plano técnico servicios compartidos de ISO 27001 para Azure Policy y RBAC.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 03/14/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 3ce755b62ce2ba0328852b551225ffa828a6c1bf
ms.sourcegitcommit: 3341598aebf02bf45a2393c06b136f8627c2a7b8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/01/2019
ms.locfileid: "58804690"
---
# <a name="control-mapping-of-the-iso-27001-shared-services-blueprint-sample"></a>Asignación de control de la muestra de plano técnico de servicios compartidos de ISO 27001

El siguiente artículo detalla cómo el plano técnico ISO 27001 servicios compartidos para Azure blueprint asignaciones del ejemplo a los controles ISO 27001. Para obtener más información acerca de los controles, vea [ISO 27001](https://www.iso.org/isoiec-27001-information-security.html).

Las asignaciones siguientes son para el **ISO 27001: 2013** controles. Use el panel de navegación de la derecha para ir directamente a una asignación de un control específico. Muchos de los controles asignados se implementan con un [Azure Policy](../../../policy/overview.md) iniciativa. Para revisar la iniciativa completa, abra **directiva** en Azure portal y seleccione el **definiciones** página. A continuación, busque y seleccione el  **[Versión preliminar] auditoría ISO 27001: 2013 controla e implementar extensiones específicas de VM para admitir los requisitos de auditoría** iniciativa de directiva integrada.

## <a name="a612-segregation-of-duties"></a>A.6.1.2 segregación de controles

Tener solo un propietario de la suscripción de Azure no permite una redundancia administrativa. Por el contrario, tener demasiados propietarios de suscripción de Azure puede aumentar la posibilidad de una infracción a través de una cuenta de propietario en peligro. Este plano técnico ayuda a mantener un número adecuado de los propietarios de suscripción de Azure mediante la asignación de dos [Azure Policy](../../../policy/overview.md) definiciones que el número de propietarios de suscripciones de Azure de auditoría. Administrar permisos de propietario de la suscripción puede ayudarle a implementar la separación adecuada de las tareas.

- [Versión preliminar]: Audit minimum number of owners for subscription
- [Versión preliminar]: Audit maximum number of owners for a subscription

## <a name="a912-access-to-networks-and-network-services"></a>A.9.1.2 acceso a redes y servicios de red

Azure implementa [control de acceso basado en roles](../../../../role-based-access-control/overview.md) (RBAC) para administrar quién tiene acceso a recursos de Azure. Esta instancia de blueprint le ayuda a controlar el acceso a recursos de Azure mediante la asignación de siete [Azure Policy](../../../policy/overview.md) definiciones. Estas directivas auditar el uso de tipos de recursos y configuraciones que pueden permitir más permisivas que el acceso a los recursos.
Recursos de descripción que infringen estas pueden directivas ayuda tomar acciones correctivas para garantizar el acceso a los recursos de Azure está restringido a los usuarios autorización.

- [Versión preliminar]: Deploy VM extension to audit Linux VM accounts with no passwords
- [Versión preliminar]: Deploy VM extension to audit Linux VM allowing remote connections from accounts with no
  Contraseñass
- [Versión preliminar]: Audit Linux VM accounts with no passwords
- [Versión preliminar]: Audit Linux VM allowing remote connections from accounts with no passwords
- Auditar el uso de las cuentas de almacenamiento clásicas
- Auditar el uso de máquinas virtuales clásicas
- Auditar las máquinas virtuales que no utilizan discos administrados

## <a name="a922-user-access-provisioning"></a>Aprovisionamiento de acceso de usuario A.9.2.2

Azure implementa [control de acceso basado en roles](../../../../role-based-access-control/overview.md) (RBAC) para administrar quién tiene acceso a recursos de Azure. Esta instancia de blueprint asigna tres [Azure Policy](../../../policy/overview.md) definiciones para auditar el usan de [Azure Active Directory](../../../../active-directory/fundamentals/active-directory-whatis.md) autenticación para servidores SQL Server y [Service Fabric](../../../../service-fabric/service-fabric-overview.md). Con Azure Active Directory permite autenticación de administración de permisos simplificada y administración de identidades centralizado de los usuarios de base de datos y otros servicios de Microsoft. Esta instancia de blueprint también asigna una definición de directiva de Azure para auditar el uso de reglas RBAC personalizados. Descripción donde las reglas de RBAC personalizadas son implemente puede ayudar a comprobar la necesidad y la implementación adecuada, como las reglas de RBAC personalizadas son propensas a errores.

- Auditar el aprovisionamiento de un administrador de Azure Active Directory para SQL Server
- Auditar el uso de Azure Active Directory para la autenticación de clientes en Service Fabric
- Auditar el uso de reglas de RBAC personalizadas

## <a name="a923-management-of-privileged-access-rights"></a>Administración de A.9.2.3 de privilegios derechos de acceso

Esta instancia de blueprint le ayuda a restringir y controlar los derechos de acceso con privilegios mediante la asignación de cuatro [Azure Policy](../../../policy/overview.md) definiciones para auditar las cuentas externas con propietario, cuentas y permisos de escritura con el propietario o permisos de escritura que no tienen habilitada la autenticación multifactor.

- [Versión preliminar]: Audit accounts with owner permissions who are not MFA enabled on a subscription
- [Versión preliminar]: Audit accounts with write permissions who are not MFA enabled on a subscription
- [Versión preliminar]: Audit external accounts with owner permissions on a subscription
- [Versión preliminar]: Audit external accounts with write permissions on a subscription

## <a name="a924-management-of-secret-authentication-information-of-users"></a>Administración de A.9.2.4 secreta información de autenticación de usuarios

Esta instancia de blueprint asigna tres [Azure Policy](../../../policy/overview.md) definiciones para auditar las cuentas que no tienen habilitada la autenticación multifactor. Multi-factor authentication ayuda a proteger las cuentas incluso si se pone en peligro una parte de la información de autenticación. Mediante la supervisión de cuentas sin la autenticación multifactor habilitada, puede identificar las cuentas que tiene más probabilidades de estar en peligro. Esta instancia de blueprint asigna también dos definiciones de directiva de Azure que auditar Linux VM permisos de archivo de contraseña para alertar si se establecen incorrectamente. Esta configuración le permite tomar medidas correctivas para asegurarse de que no están en peligro los autenticadores.

- [Versión preliminar]: Audit accounts with owner permissions who are not MFA enabled on a subscription
- [Versión preliminar]: Audit accounts with read permissions who are not MFA enabled on a subscription
- [Versión preliminar]: Audit accounts with write permissions who are not MFA enabled on a subscription
- [Versión preliminar]: Deploy VM extension to audit Linux VM passwd file permissions
- [Versión preliminar]: Audit Linux VM /etc/passwd file permissions are set to 0644

## <a name="a925-review-of-user-access-rights"></a>Revisión A.9.2.5 de derechos de acceso de usuario

Azure implementa [control de acceso basado en roles](../../../../role-based-access-control/overview.md) (RBAC) que ayuda a administrar quién tiene acceso a los recursos en Azure. Mediante el portal de Azure, puede revisar quién tiene acceso a los recursos de Azure y sus permisos. Esta instancia de blueprint asigna cuatro [Azure Policy](../../../policy/overview.md) definiciones para auditar las cuentas que deben recibir prioridad para su revisión, incluidas cuentas de depreciación y las cuentas externas con permisos elevados.

- [Versión preliminar]: Audit deprecated accounts on a subscription
- [Versión preliminar]: Audit deprecated accounts with owner permissions on a subscription
- [Versión preliminar]: Audit external accounts with owner permissions on a subscription
- [Versión preliminar]: Audit external accounts with write permissions on a subscription

## <a name="a926-removal-or-adjustment-of-access-rights"></a>Eliminación de A.9.2.6 o ajuste de derechos de acceso

Azure implementa [control de acceso basado en roles](../../../../role-based-access-control/overview.md) (RBAC) que ayuda a administrar quién tiene acceso a los recursos en Azure. Uso de [Azure Active Directory](../../../../active-directory/fundamentals/active-directory-whatis.md) y RBAC, puede actualizar los roles de usuario para reflejar los cambios en la organización. Cuando sea necesario, las cuentas pueden no podrá iniciar sesión (o quitarse), lo que elimina inmediatamente los derechos de acceso a recursos de Azure. Esta instancia de blueprint asigna dos [Azure Policy](../../../policy/overview.md) definiciones para auditar depreciado cuenta que debe tenerse en cuenta para su eliminación.

- [Versión preliminar]: Audit deprecated accounts on a subscription
- [Versión preliminar]: Audit deprecated accounts with owner permissions on a subscription

## <a name="a943-password-management-system"></a>Sistema de administración de contraseñas A.9.4.3

Esta instancia de blueprint le ayuda a aplicar unas contraseñas seguras mediante la asignación de 10 [Azure Policy](../../../policy/overview.md) definiciones que auditar máquinas virtuales de Windows que no aplican resistencia mínima y otros requisitos de contraseña. Reconocimiento de las máquinas virtuales de infracción de la directiva de seguridad de contraseña le ayuda a tomar medidas correctivas para garantizar que sean compatibles con la directiva de contraseñas para todas las cuentas de usuario de máquina virtual.

- [Versión preliminar]: Deploy VM extension to audit Windows VM enforces password complexity requirements
- [Versión preliminar]: Deploy VM extension to audit Windows VM maximum password age 70 days
- [Versión preliminar]: Deploy VM extension to audit Windows VM minimum password age 1 day
- [Versión preliminar]: Deploy VM extension to audit Windows VM passwords must be at least 14 characters
- [Versión preliminar]: Deploy VM extension to audit Windows VM should not allow previous 24 passwords
- [Versión preliminar]: Audit Windows VM enforces password complexity requirements
- [Versión preliminar]: Audit Windows VM maximum password age 70 days
- [Versión preliminar]: Audit Windows VM minimum password age 1 day
- [Versión preliminar]: Audit Windows VM passwords must be at least 14 characters
- [Versión preliminar]: Audit Windows VM should not allow previous 24 passwords

## <a name="a1011-policy-on-the-use-of-cryptographic-controls"></a>Directiva A.10.1.1 sobre el uso de controles criptográficos

Esta instancia de blueprint le ayuda a aplicar la directiva en el uso de criptografía controles mediante la asignación de 13 [Azure Policy](../../../policy/overview.md) usan definiciones que aplican controles específicos de criptografía y auditoría de configuración de cifrado débil.
Descripción de donde los recursos de Azure pueden tener configuraciones cifradas que no sea óptimo puede ayudarle a tomar acciones correctivas para garantizar que los recursos están configurados según la directiva de seguridad de información. En concreto, las directivas asignadas por esta instancia de blueprint requieren cifrado para las cuentas de almacenamiento de blobs y cuentas de almacenamiento del lago de datos; Requerir cifrado de datos transparente en bases de datos SQL; auditar cifrado que faltan en las cuentas de almacenamiento, bases de datos SQL, discos de máquinas virtuales y las variables de cuenta de automation; auditar las conexiones no seguras para las cuentas de almacenamiento, aplicaciones de función, aplicación Web, aplicaciones de API y Redis Cache; auditar cifrado de contraseña de máquina virtual débil; y auditoría de comunicación de Service Fabric sin cifrar.

- [Versión preliminar]: Audit HTTPS only access for a Function App
- [Versión preliminar]: Audit HTTPS only access for a Web Application
- [Versión preliminar]: Audit HTTPS only access for an API App
- [Versión preliminar]: Audit missing blob encryption for storage accounts
- [Versión preliminar]: Deploy VM extension to audit Windows VM should not store passwords using reversible
  El cifradon
- [Versión preliminar]: Audit Windows VM should not store passwords using reversible encryption
- [Versión preliminar]: Monitor unencrypted SQL database in Azure Security Center
- [Versión preliminar]: Monitor unencrypted VM Disks in Azure Security Center
- Auditar la habilitación del cifrado de variables de cuentas de Automation
- Auditar la habilitación solo de conexiones seguras a Redis Cache
- Auditar la transferencia segura a cuentas de almacenamiento
- Auditar la configuración de la propiedad ClusterProtectionLevel como EncryptAndSign en Service Fabric
- Auditar el estado del cifrado de datos transparente

## <a name="a1241-event-logging"></a>Registro de eventos A.12.4.1

Esta instancia de blueprint le ayuda a asegurarse de que se registran los eventos del sistema mediante la asignación de siete [Azure Policy](../../../policy/overview.md) definiciones de registro de auditoría en los recursos de Azure. Una directiva asignada también audita si las máquinas virtuales no están enviando registros a un área de trabajo de análisis de registro especificado.

- [Versión preliminar]: Implementación de agente de dependencia de auditoría - dados de baja de imagen de la máquina virtual (SO)
- [Versión preliminar]: Implantación de agentes de dependencia en VMSS - dados de baja de imagen de la máquina virtual (SO) de auditoría
- [Versión preliminar]: Auditoría de la implementación de agente de Log Analytics - dados de baja de imagen de la máquina virtual (SO)
- [Versión preliminar]: Implantación de agentes de análisis de registro de auditoría en VMSS - dados de baja de imagen de la máquina virtual (SO)
- [Versión preliminar]: Monitor unaudited SQL database in Azure Security Center
- Auditar la configuración de diagnóstico
- Auditar configuración de auditoría de SQL en el nivel de servidor

## <a name="a121-management-of-technical-vulnerabilities"></a>A.12.1 administración de vulnerabilidades técnicas

Esta instancia de blueprint le ayuda a administrar las vulnerabilidades del sistema de información mediante la asignación de cinco [Azure Policy](../../../policy/overview.md) definiciones que supervisan que faltan actualizaciones del sistema, vulnerabilidades del sistema operativo, las vulnerabilidades SQL y máquina virtual vulnerabilidades. Estas informaciones proporcionan información en tiempo real sobre el estado de seguridad de los recursos implementados y pueden ayudarle a priorizar las acciones de corrección.

- [Versión preliminar]: Monitor missing Endpoint Protection in Azure Security Center
- [Versión preliminar]: Monitor missing system updates in Azure Security Center
- [Versión preliminar]: Monitor OS vulnerabilities in Azure Security Center
- [Versión preliminar]: Monitor SQL vulnerability assessment results in Azure Security Center
- [Versión preliminar]: Monitor VM Vulnerabilities in Azure Security Center

## <a name="a1262-restrictions-on-software-installation"></a>Restricciones de A.12.6.2 en la instalación de software

Control de aplicaciones adaptables es la solución de Azure Security Center que le permite controlar qué aplicaciones puede ejecutarse en las máquinas virtuales ubicadas en Azure. Esta instancia de blueprint asigna una definición de directiva de Azure que supervisa los cambios realizados en el conjunto de aplicaciones permitidas. Restricciones de instalación de software pueden ayudarle a reducir la probabilidad de introducción de las vulnerabilidades del software.

- [Versión preliminar]: Monitor possible app Whitelisting in Azure Security Center

## <a name="a1311-network-controls"></a>Controles de red A.13.1.1

Esta instancia de blueprint le ayuda a administrar y controlar las redes asignando un [Azure Policy](../../../policy/overview.md) definición que supervisa los grupos de seguridad de red con reglas permisivas. Las reglas que son demasiado permisivas pueden permitir el acceso de red no deseado y deben revisarse.

- [Versión preliminar]: Monitor permissive network access in Azure Security Center

## <a name="a1321-information-transfer-policies-and-procedures"></a>Procedimientos y directivas de transferencia de información A.13.2.1

El plano técnico le ayuda a garantizar la transferencia de información con servicios de Azure es segura mediante la asignación de dos [Azure Policy](../../../policy/overview.md) definiciones para auditar las conexiones no seguras a las cuentas de almacenamiento y Redis Cache.

- Auditar la habilitación solo de conexiones seguras a Redis Cache
- Auditar la transferencia segura a cuentas de almacenamiento

## <a name="a1413-protecting-application-services-transactions"></a>Las transacciones de servicios de aplicación de protección A.14.1.3

Esta instancia de blueprint le ayuda a proteger los activos de información del sistema mediante la asignación de tres [Azure Policy](../../../policy/overview.md) definiciones que supervisión puntos de conexión desprotegidos, aplicaciones y cuentas de almacenamiento. Los puntos de conexión y las aplicaciones que no están protegidas por un firewall y las cuentas de almacenamiento con acceso sin restricciones pueden permitir el acceso no deseado a la información contenida en el sistema de información.

- [Versión preliminar]: Monitor unprotected network endpoints in Azure Security Center
- [Versión preliminar]: Monitor unprotected web application in Azure Security Center
- Auditar el acceso de red sin restricciones a cuentas de almacenamiento

## <a name="a1613-reporting-information-security-weaknesses"></a>Vulnerabilidades de seguridad de información de generación de informes A.16.1.3

Esta instancia de blueprint le ayuda a mantener el conocimiento de las vulnerabilidades del sistema mediante la asignación de cinco [Azure Policy](../../../policy/overview.md) definiciones que supervisan las vulnerabilidades, estado de la revisión y alertas de malware en Azure Security Center. Azure Security Center proporciona funcionalidades de informes que le permiten tener información en tiempo real sobre el estado de seguridad de los recursos de Azure implementados.

- [Versión preliminar]: Monitor missing Endpoint Protection in Azure Security Center
- [Versión preliminar]: Monitor missing system updates in Azure Security Center
- [Versión preliminar]: Monitor OS vulnerabilities in Azure Security Center
- [Versión preliminar]: Monitor SQL vulnerability assessment results in Azure Security Center
- [Versión preliminar]: Monitor VM Vulnerabilities in Azure Security Center

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha revisado la asignación de control de la instancia de blueprint servicios compartidos de ISO 27001, visite los siguientes artículos para obtener información sobre la arquitectura y cómo implementar este ejemplo:

> [!div class="nextstepaction"]
> [Plano técnico de servicios compartidos de ISO 27001: información general sobre](./index.md)
> [servicios compartidos de ISO 27001 blueprint: pasos de implementación](./deploy.md)

Artículos adicionales sobre planos técnicos y cómo utilizarlos:

- Más información sobre el [ciclo de vida del plano técnico](../../concepts/lifecycle.md)
- Descubra cómo utilizar [parámetros estáticos y dinámicos](../../concepts/parameters.md).
- Aprenda a personalizar el [orden de secuenciación de planos técnicos](../../concepts/sequencing-order.md).
- Averigüe cómo usar el [bloqueo de recursos de planos técnicos](../../concepts/resource-locking.md).
- Aprenda a [actualizar las asignaciones existentes](../../how-to/update-existing-assignments.md).