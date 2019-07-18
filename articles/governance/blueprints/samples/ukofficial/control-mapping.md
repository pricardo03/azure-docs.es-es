---
title: 'Ejemplos: planos técnicos de UK OFFICIAL y UK NHS (asignación de controles)'
description: Asignación de controles de los ejemplos de planos técnicos de UK OFFICIAL y UK NHS.
services: blueprints
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/26/2019
ms.topic: conceptual
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: 01a8e104f6d590113784db28e4bfde849d78b15f
ms.sourcegitcommit: ac1cfe497341429cf62eb934e87f3b5f3c79948e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/01/2019
ms.locfileid: "67491908"
---
# <a name="control-mapping-of-the-uk-official-and-uk-nhs-blueprint-samples"></a>Asignación de controles de los ejemplos de planos técnicos de UK OFFICIAL y UK NHS.

En el artículo siguiente se detalla cómo se asignan los ejemplos de planos técnicos de UK OFFICIAL y UK NHS a los controles UK OFFICIAL y UK NHS. Para obtener más información sobre los controles, consulte [UK OFFICIAL](https://www.gov.uk/government/publications/government-security-classifications).

Las asignaciones siguientes son para los controles **UK OFFICIAL** y **UK NHS**. Use el panel de navegación de la derecha para ir directamente a una asignación de control específica. Muchos de los controles asignados se implementan con una iniciativa de [Azure Policy](../../../policy/overview.md). Para revisar la iniciativa completa, abra **Policy** en Azure Portal y seleccione la página **Definiciones**. Después, busque y seleccione la iniciativa de directiva integrada **[Versión preliminar] Auditar los controles UK OFFICIAL y UK NHS e implementar extensiones de VM específicas para cumplir con los requisitos de la auditoría**.

## <a name="1-data-in-transit-protection"></a>1\. Protección de datos en tránsito

El plano técnico ayuda a garantizar que la transferencia de información con los servicios de Azure es segura. Para ello, asigna definiciones de [Azure Policy](../../../policy/overview.md) para auditar las conexiones no seguras a las cuentas de almacenamiento y a Redis Cache.

- Se deben habilitar solo las conexiones seguras a Redis Cache.
- Se debe habilitar la transferencia segura a cuentas de almacenamiento.

## <a name="23-data-at-rest-protection"></a>2.3. Protección de los datos en reposo

Este plano técnico ayuda a instaurar la directiva sobre el uso de controles criptográficos mediante la asignación de definiciones de [Azure Policy](../../../policy/overview.md) que instauran controles criptográficos específicos y auditan el uso de configuraciones criptográficas poco seguras.
Conocer dónde los recursos de Azure pueden tener configuraciones criptográficas subóptimas ayuda a tomar medidas correctivas para garantizar que los recursos se configuran conforme a la directiva de seguridad de la información. En concreto, las directivas asignadas por este plano técnico requieren cifrado para las cuentas de Data Lake Storage y cifrado de datos transparente en las bases de datos SQL; auditan el cifrado que falta en las cuentas de almacenamiento, las bases de datos SQL, los discos de máquina virtual y las variables de cuenta de Automation; auditan las conexiones no seguras a cuentas de almacenamiento y Redis Cache; y auditan el cifrado de contraseñas no seguras de las máquinas virtuales, así como la comunicación sin cifrado de Service Fabric.

- Supervisión de bases de datos SQL sin cifrar en Azure Security Center.
- Se debe aplicar el cifrado de discos en máquinas virtuales.
- Se deben cifrar las variables de cuenta de Automation.
- Se debe habilitar la transferencia segura a cuentas de almacenamiento.
- Se debe establecer la propiedad ClusterProtectionLevel en EncryptAndSign en los clústeres de Service Fabric.
- Se debe habilitar el Cifrado de datos transparente en bases de datos SQL.
- Implementación del Cifrado de datos transparente de SQL DB.
- Exigencia de cifrado en las cuentas de Data Lake Store.
- Ubicaciones permitidas (codificadas de forma rígida como "UK SOUTH" y "UK WEST").
- Ubicaciones permitidas para grupos de recursos (codificadas de forma rígida como "UK SOUTH" y "UK WEST").

## <a name="52-vulnerability-management"></a>5.2. Administración de vulnerabilidades

Este plano técnico ayuda a administrar las vulnerabilidades del sistema de información al asignar definiciones de [Azure Policy](../../../policy/overview.md) que supervisan si falta la protección del punto de conexión, si faltan actualizaciones del sistema o si existen vulnerabilidades del sistema operativo, de SQL o de las máquinas virtuales. Esta información proporciona detalles en tiempo real sobre el estado de seguridad de los recursos implementados y ayuda a priorizar las medidas correctivas.

- Supervisión de la falta de Endpoint Protection en Azure Security Center
- Se deben instalar actualizaciones del sistema en los equipos.
- Se deben corregir vulnerabilidades en la configuración de seguridad en las máquinas.
- Se deben corregir las vulnerabilidades en las bases de datos SQL.
- Se deben corregir las vulnerabilidades mediante una solución de evaluación de vulnerabilidades.

## <a name="53-protective-monitoring"></a>5.3. Supervisión de protección

Este plano técnico ayuda a proteger los recursos de información del sistema mediante la asignación de definiciones de [Azure Policy](../../../policy/overview.md) que proporcionan supervisión de protección para el acceso sin restricciones, la actividad de la lista blanca y las amenazas.

- Auditar el acceso de red sin restricciones a cuentas de almacenamiento
- Deben habilitarse los controles de aplicación adaptables en las máquinas virtuales.
- Implementación de la detección de amenazas en servidores SQL Server.
- Implementación de la extensión antimalware predeterminada de Microsoft IaaS para Windows Server.

## <a name="9-secure-user-management--10-identity-and-authentication"></a>9\. Administración segura de usuarios / 10. Identidad y autenticación

Azure implementa el control de acceso basado en rol (RBAC) para ayudar a administrar quién tiene acceso a los recursos en Azure. Mediante Azure Portal, puede revisar quién tiene acceso a los recursos de Azure y sus permisos. Este plano técnico ayuda a restringir y controlar los derechos de acceso mediante la asignación de definiciones de [Azure Policy](../../../policy/overview.md) para auditar las cuentas externas con permisos de propietario y de lectura/escritura y las cuentas con permisos de propietario y de lectura/escritura que no tienen habilitada la autenticación multifactor.

- MFA debe estar habilitada en las cuentas con permisos de propietario en la suscripción.
- MFA debe estar habilitada en las cuentas con permisos de escritura en la suscripción.
- MFA debe estar habilitada en las cuentas con permisos de lectura en la suscripción.
- Las cuentas externas con permisos de propietario deben quitarse de la suscripción.
- Las cuentas externas con permisos de escritura deben quitarse de la suscripción.
- Las cuentas externas con permisos de lectura deben quitarse de la suscripción.

Este plano técnico asigna definiciones de Azure Policy para auditar el uso de la autenticación de Azure Active Directory para servidores SQL Server y Service Fabric. El uso de la autenticación de Azure Active Directory simplifica la administración de permisos y centraliza la administración de identidades de usuarios de base de datos y otros servicios de Microsoft.

- El administrador de Azure Active Directory debe aprovisionarse para servidores SQL Server.
- Los clústeres de Service Fabric solo deben usar Azure Active Directory para la autenticación de cliente.

Este plano técnico también asigna definiciones de Azure Policy para auditar las cuentas que deberían priorizarse para la revisión, incluidas las cuentas en desuso y externas. Cuando sea necesario, se puede bloquear el inicio de sesión de las cuentas (o eliminar estas), lo cual eliminaría inmediatamente los derechos de acceso a los recursos de Azure. Este plano técnico asigna dos definiciones de Azure Policy para auditar las cuentas en desuso cuya eliminación debería considerarse.

- Las cuentas en desuso deben quitarse de la suscripción.
- Las cuentas en desuso con permisos de propietario deben quitarse de la suscripción.
- Las cuentas externas con permisos de propietario deben quitarse de la suscripción.
- Las cuentas externas con permisos de escritura deben quitarse de la suscripción.

Este plano técnico también asigna una definición de Azure Policy que audita los permisos de los archivos con contraseña de máquina virtual Linux y alerta si la configuración es incorrecta. Este diseño permite tomar medidas correctivas para garantizar que los autenticadores no están en riesgo.

- [Versión preliminar]: Audit Linux VM /etc/passwd file permissions are set to 0644

Este plano técnico ayuda a instaurar contraseñas seguras mediante la asignación de definiciones de Azure Policy que auditan las máquinas virtuales Windows que no tienen unos requisitos mínimos de seguridad de contraseña y de otros tipos. Conocer las máquinas virtuales que infringen la directiva de seguridad de las contraseñas ayuda a tomar medidas correctivas para garantizar que las contraseñas de todas las cuentas de usuario de máquina virtual cumplen la directiva.

- [Versión preliminar]: Deploy requirements to audit Windows VMs that do not have the password complexity setting enabled
- [Versión preliminar]: Deploy requirements to audit Windows VMs that do not have a maximum password age of 70 days
- [Versión preliminar]: Deploy requirements to audit Windows VMs that do not have a minimum password age of 1 day
- [Versión preliminar]: Deploy requirements to audit Windows VMs that do not restrict the minimum password length to 14 characters
- [Versión preliminar]: Deploy requirements to audit Windows VMs that allow re-use of the previous 24 passwords
- [Versión preliminar]: Audit Windows VMs that do not have the password complexity setting enabled
- [Versión preliminar]: Audit Windows VMs that do not have a maximum password age of 70 days
- [Versión preliminar]: Audit Windows VMs that do not have a minimum password age of 1 day
- [Versión preliminar]: Audit Windows VMs that do not restrict the minimum password length to 14 characters
- [Versión preliminar]: Audit Windows VMs that allow re-use of the previous 24 passwords

Este plano técnico también ayuda a controlar el acceso a los recursos de Azure mediante la asignación de definiciones de Azure Policy. Estas directivas auditan el uso de los tipos de recursos y las configuraciones que pueden permitir un acceso más flexible a los recursos. Conocer los recursos que infringen estas directivas ayuda a tomar medidas correctivas para garantizar que el acceso a los recursos de Azure se limita a los usuarios autorizados.

- [Versión preliminar]: Deploy requirements to audit Linux VMs that have accounts without passwords
- [Versión preliminar]: Deploy requirements to audit Linux VMs that allow remote connections from accounts without passwords
- [Versión preliminar]: Audit Linux VMs that have accounts without passwords
- [Versión preliminar]: Audit Linux VMs that allow remote connections from accounts without passwords
- Se deben migrar las cuentas de almacenamiento a nuevos recursos de Azure Resource Manager.
- Se deben migrar las máquinas virtuales a nuevos recursos de Azure Resource Manager.
- Auditar las máquinas virtuales que no utilizan discos administrados

## <a name="11-external-interface-protection"></a>11. Protección de la interfaz externa

Además de usar más de 25 directivas para garantizar una administración segura de usuarios adecuada, este plano técnico ayuda a proteger las interfaces de servicio contra el acceso no autorizado mediante la asignación de una definición de [Azure Policy](../../../policy/overview.md) que supervisa las cuentas de almacenamiento sin restricciones. Las cuentas de almacenamiento con acceso sin restricciones pueden permitir el acceso no deseado a la información contenida en el sistema de información. Este plano técnico también asigna una directiva que habilita controles de aplicación adaptables en máquinas virtuales.

- Auditar el acceso de red sin restricciones a cuentas de almacenamiento
- Deben habilitarse los controles de aplicación adaptables en las máquinas virtuales.

## <a name="12-secure-service-administration"></a>12. Administración segura del servicio

Azure implementa el control de acceso basado en rol (RBAC) para ayudar a administrar quién tiene acceso a los recursos en Azure. Mediante Azure Portal, puede revisar quién tiene acceso a los recursos de Azure y sus permisos. Este plano técnico ayuda a restringir y controlar los derechos de acceso con privilegios mediante la asignación de cinco definiciones de [Azure Policy](../../../policy/overview.md) para auditar las cuentas externas con permisos de propietario y de escritura y las cuentas con permisos de propietario y de escritura que no tienen habilitada la autenticación multifactor.

Los sistemas que se usan para la administración de un servicio en la nube tendrán acceso con privilegios elevados a ese servicio. Ponerlos en peligro conlleva un impacto significativo, lo cual incluye los medios para burlar los controles de seguridad y robar o manipular grandes volúmenes de datos. Los métodos que usen los administradores del proveedor de servicios para administrar el servicio operativo deben estar diseñados para reducir cualquier riesgo de vulneración que ponga en peligro la seguridad del servicio. Si no se pone en práctica este principio, un atacante podría contar con los medios necesarios para burlar los controles de seguridad y robar o manipular grandes volúmenes de datos.

- MFA debe estar habilitada en las cuentas con permisos de propietario en la suscripción.
- MFA debe estar habilitada en las cuentas con permisos de escritura en la suscripción.
- Las cuentas externas con permisos de propietario deben quitarse de la suscripción.
- Las cuentas externas con permisos de escritura deben quitarse de la suscripción.

Este plano técnico asigna definiciones de Azure Policy para auditar el uso de la autenticación de Azure Active Directory para servidores SQL Server y Service Fabric. El uso de la autenticación de Azure Active Directory simplifica la administración de permisos y centraliza la administración de identidades de usuarios de base de datos y otros servicios de Microsoft.

- El administrador de Azure Active Directory debe aprovisionarse para servidores SQL Server.
- Los clústeres de Service Fabric solo deben usar Azure Active Directory para la autenticación de cliente.

Este plano técnico también asigna definiciones de Azure Policy para auditar las cuentas que deberían priorizarse para la revisión, incluidas las cuentas en desuso y externas con permisos elevados. Cuando sea necesario, se puede bloquear el inicio de sesión de las cuentas (o eliminar estas), lo cual eliminaría inmediatamente los derechos de acceso a los recursos de Azure. Este plano técnico asigna dos definiciones de Azure Policy para auditar las cuentas en desuso cuya eliminación debería considerarse.

- Las cuentas en desuso deben quitarse de la suscripción.
- Las cuentas en desuso con permisos de propietario deben quitarse de la suscripción.
- Las cuentas externas con permisos de propietario deben quitarse de la suscripción.
- Las cuentas externas con permisos de escritura deben quitarse de la suscripción.

Este plano técnico también asigna una definición de Azure Policy que audita los permisos de los archivos con contraseña de máquina virtual Linux y alerta si la configuración es incorrecta. Este diseño permite tomar medidas correctivas para garantizar que los autenticadores no están en riesgo.

- [Versión preliminar]: Audit Linux VM /etc/passwd file permissions are set to 0644

## <a name="13-audit-information-for-users"></a>13. Información de auditoría para los usuarios

Este plano técnico permite garantizar que los eventos del sistema se registran. Para ello, se asignan definiciones de [Azure Policy](../../../policy/overview.md) que auditan las configuraciones de registro de los recursos de Azure. Una directiva asignada también audita si las máquinas virtuales no envían registros a un área de trabajo específica de Log Analytics.

- Supervisión de servidores SQL Server no auditados en Azure Security Center.
- Auditar la configuración de diagnóstico
- Auditar configuración de auditoría de SQL en el nivel de servidor
- [Versión preliminar]: Deploy Log Analytics Agent for Linux VMs
- [Versión preliminar]: Deploy Log Analytics Agent for Windows VMs
- Implementar Network Watcher al crear redes virtuales.

## <a name="next-steps"></a>Pasos siguientes

Ahora que ha revisado la asignación de controles de los planos técnicos UK OFFICIAL y UK NHS, visite los siguientes artículos para obtener información sobre la arquitectura y cómo implementar este ejemplo:

> [!div class="nextstepaction"]
> [Planos técnicos de UK OFFICIAL y UK NHS: introducción](./index.md)
> [Planos técnicos de UK OFFICIAL y UK NHS: pasos de implementación](./deploy.md)

Artículos adicionales sobre planos técnicos y cómo utilizarlos:

- Más información sobre el [ciclo de vida del plano técnico](../../concepts/lifecycle.md)
- Descubra cómo utilizar [parámetros estáticos y dinámicos](../../concepts/parameters.md).
- Aprenda a personalizar el [orden de secuenciación de planos técnicos](../../concepts/sequencing-order.md).
- Averigüe cómo usar el [bloqueo de recursos de planos técnicos](../../concepts/resource-locking.md).
- Aprenda a [actualizar las asignaciones existentes](../../how-to/update-existing-assignments.md).
