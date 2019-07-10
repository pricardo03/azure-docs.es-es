---
title: Asignación de control de muestra del proyecto de NIST SP 800-53 R4
description: Asignación de control de muestra dle proyecto de NIST SP 800-53 R4 para Azure Policy y RBAC.
author: DCtheGeek
ms.author: dacoulte
ms.date: 06/24/2019
ms.topic: sample
ms.service: blueprints
manager: carmonm
ms.openlocfilehash: b887c4e6812d201dc83465a578f71e1742e8e9cf
ms.sourcegitcommit: a12b2c2599134e32a910921861d4805e21320159
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/24/2019
ms.locfileid: "67343087"
---
# <a name="control-mapping-of-the-nist-sp-800-53-r4-blueprint-sample"></a>Asignación de control de muestra del proyecto de NIST SP 800-53 R4

En el siguiente artículo se detalla cómo se asigna la muestra de proyecto de Azure Blueprints NIST SP 800-53 R4 a los controles de NIST SP 800-53 R4. Para más información sobre los controles, consulte [NIST SP 800-53](https://nvd.nist.gov/800-53).

Las siguientes asignaciones son para los controles de **NIST SP 800-53 (Rev. 4)** . Use el panel de navegación de la derecha para ir directamente a una asignación de control específica. Muchos de los controles asignados se implementan con una iniciativa de [Azure Policy](../../../policy/overview.md). Para revisar la iniciativa completa, abra **Policy** en Azure Portal y seleccione la página **Definiciones**. Después, busque y seleccione la **[versión preliminar]: Audite los controles NIST SP 800-53 R4 e implemente extensiones de VM específicas para admitir la iniciativa de directiva integrada de los requisitos de auditoría**.

## <a name="ac-2-account-management"></a>Administración de cuentas AC-2

Este proyecto le ayudará a revisar las cuentas que pueda que no cumplan los requisitos de administración de cuentas de su organización. Este proyecto asigna cinco definiciones de Azure Policy que auditan las cuentas externas con los permisos de lectura, escritura y propietario en una suscripción y en las cuentas en desuso. Al revisar las cuentas auditadas por estas directivas, puede tomar las medidas adecuadas para garantizar que se cumplen los requisitos de administración de cuentas.

- [Versión preliminar]: Audit deprecated accounts on a subscription
- [Versión preliminar]: Audit deprecated accounts with owner permissions on a subscription
- [Versión preliminar]: Audit external accounts with owner permissions on a subscription
- [Versión preliminar]: Audit external accounts with read permissions on a subscription
- [Versión preliminar]: Audit external accounts with write permissions on a subscription

## <a name="ac-2-7-account-management--role-based-schemes"></a>Administración de cuentas AC-2 (7) | Esquemas basados en roles

Azure implementa el [control de acceso basado en rol](../../../../role-based-access-control/overview.md) (RBAC) para permitirle administrar quién tiene acceso a los recursos de Azure. Mediante Azure Portal, puede revisar quién tiene acceso a los recursos de Azure y sus permisos. Este proyecto también asigna dos definiciones de [Azure Policy](../../../policy/overview.md) para auditar el uso de la autenticación de Azure Active Directory para los servidores SQL Server y Service Fabric. El uso de la autenticación de Azure Active Directory simplifica la administración de permisos y centraliza la administración de identidades de usuarios de base de datos y otros servicios de Microsoft.

- Auditar el aprovisionamiento de un administrador de Azure Active Directory para SQL Server
- Auditar el uso de Azure Active Directory para la autenticación de clientes en Service Fabric

## <a name="ac-2-12-account-management--account-monitoring--atypical-usage"></a>Administración de cuentas AC-2 (12) | Supervisión o uso inusual de cuentas

El acceso a máquinas virtuales Just-In-Time (JIT) bloquea el tráfico entrante a las máquinas virtuales de Azure, lo que reduce la exposición a ataques y, al mismo tiempo, se proporciona acceso sencillo para conectarse a las máquinas virtuales cuando sea necesario. Todas las solicitudes JIT para tener acceso a las máquinas virtuales se registran en el registro de actividad que le permiten supervisar el uso inusual. Esta instancia de proyecto asigna una definición de [Azure Policy](../../../policy/overview.md) que le ayuda a supervisar las máquinas virtuales que pueden admitir el acceso just-in-time pero aún no se han configurado.

- [Versión preliminar]: Supervisión del posible acceso de red Just-In-Time (JIT) en Azure Security Center

## <a name="ac-4-information-flow-enforcement"></a>Aplicación del flujo de información AC-4

El uso compartido de recursos entre orígenes (CORS) puede permitir que los recursos de App Services sean solicitados desde un dominio externo. Microsoft recomienda que solo permita que los dominios requeridos interactúen con las aplicaciones web, las funciones y las API. Este proyecto asigna una definición de [Azure Policy](../../../policy/overview.md) para ayudar a supervisar las restricciones de acceso de recursos CORS en Azure Security Center.
Conocer las implementaciones de CORS puede ayudarle a comprobar que se han implementado los controles de flujo de información.

- [Versión preliminar]: Audit CORS resource access restrictions for a Web Application

## <a name="ac-5-separation-of-duties"></a>Separación de obligaciones AC-5

Tener solo un propietario de la suscripción de Azure no permite la redundancia administrativa. Por el contrario, tener demasiados propietarios de suscripción de Azure aumenta la posibilidad de infracción de seguridad mediante una cuenta de propietario en riesgo. Este plano técnico ayuda a mantener un número adecuado de propietarios de suscripción de Azure mediante la asignación de dos definiciones de [Azure Policy](../../../policy/overview.md) que auditarán el número de estos. Este proyecto también asigna cuatro definiciones de Azure Policy que le ayudan a controlan la pertenencia del grupo Administradores en las máquinas virtuales Windows. La administración de permisos de administrador de máquina virtual y de propietario de suscripción ayuda a implementar la separación adecuada de las tareas.

- [Versión preliminar]: Audit maximum number of owners for a subscription
- [Versión preliminar]: Audit minimum number of owners for subscription
- Auditar que el grupo Administradores dentro de las máquinas virtuales Windows excluya a los miembros especificados
- Auditar que el grupo Administradores dentro de las máquinas virtuales Windows incluya a los miembros especificados
- Implementar la extensión de máquina virtual para auditar que el grupo Administradores dentro de las máquinas virtuales Windows excluya a los miembros especificados
- Implementar la extensión de máquina virtual para auditar que el grupo Administradores dentro de las máquinas virtuales Windows incluya a los miembros especificados

## <a name="ac-6-7-least-privilege--review-of-user-privileges"></a>Privilegios mínimos AC-6 (7) | Revisión de privilegios de usuario

Azure implementa el [control de acceso basado en rol](../../../../role-based-access-control/overview.md) (RBAC) para permitirle administrar quién tiene acceso a los recursos de Azure. Mediante Azure Portal, puede revisar quién tiene acceso a los recursos de Azure y sus permisos. Esta instancia de proyecto asigna seis definiciones de [Azure Policy](../../../policy/overview.md) para auditar las cuentas que deben recibir prioridad para revisarse. Revisar estos indicadores de la cuenta puede ayudarle a asegurarse de que se han implementado los controles de privilegios mínimos.

- [Versión preliminar]: Audit maximum number of owners for a subscription
- [Versión preliminar]: Audit minimum number of owners for subscription
- Auditar que el grupo Administradores dentro de las máquinas virtuales Windows excluya a los miembros especificados
- Auditar que el grupo Administradores dentro de las máquinas virtuales Windows incluya a los miembros especificados
- Implementar la extensión de máquina virtual para auditar que el grupo Administradores dentro de las máquinas virtuales Windows excluya a los miembros especificados
- Implementar la extensión de máquina virtual para auditar que el grupo Administradores dentro de las máquinas virtuales Windows incluya a los miembros especificados

## <a name="ac-17-1-remote-access--automated-monitoring--control"></a>Acceso remoto AC-17 (1) | Control y supervisión automatizados

Este proyecto le ayuda a supervisar y controlar el acceso remoto; para ello, asigna tres definiciones de [Azure Policy](../../../policy/overview.md) para los monitores que depuran de forma remota para que la aplicación de Azure App Service se desactive y dos definiciones de directiva que auditen las máquinas virtuales Linux que permiten las conexiones remotas desde cuentas sin contraseñas. Este proyecto también asigna una definición de Azure Policy que le ayuda a supervisar el acceso sin restricciones a las cuentas de almacenamiento. La supervisión de estos indicadores puede ayudarle a garantizar que los métodos de acceso remoto cumplan con la directiva de seguridad.

- [Versión preliminar]: Audit remote debugging state for a Function App
- [Versión preliminar]: Audit remote debugging state for a Web Application
- [Versión preliminar]: Audit remote debugging state for an API App
- [Versión preliminar]: Audit that Linux VMs do not allow remote connections from accounts without passwords
- [Versión preliminar]: Deploy VM extension to audit that Linux VMs do not allow remote connections from accounts without passwords
- Auditar el acceso de red sin restricciones a cuentas de almacenamiento

## <a name="au-3-2-content-of-audit-records--centralized-management-of-planned-audit-record-content"></a>Contenido de los registros de auditoría AU-3 (2)| Administración centralizada del contenido de los registros de auditoría planeada

Los datos de registro que recopila Azure Monitor se almacenan en un área de trabajo de Log Analytics que permite la configuración y administración centralizados. Este proyecto le ayuda a garantizar que los eventos se registran; para ello, asigna siete definiciones de [Azure Policy](../../../policy/overview.md) que auditan y aplican la implementación del agente de Log Analytics en las máquinas virtuales de Azure.

- [Versión preliminar]: Auditoría de la implementación del agente de Log Analytics: la imagen de la VM (SO) no está en la lista
- [Versión preliminar]: Auditoría de la implementación del agente de Log Analytics en VMSS: la imagen de la VM (SO) no está en la lista
- [Versión preliminar]: Audit Log Analytics Workspace for VM - Report Mismatch
- [Versión preliminar]: Deploy Log Analytics Agent for Linux VM Scale Sets (VMSS)
- [Versión preliminar]: Deploy Log Analytics Agent for Linux VMs
- [Versión preliminar]: Deploy Log Analytics Agent for Windows VM Scale Sets (VMSS)
- [Versión preliminar]: Deploy Log Analytics Agent for Windows VMs

## <a name="au-5-response-to-audit-processing-failures"></a>Respuesta a errores de procesamiento de auditoría AU-5

Este proyecto asigna cinco definiciones de [Azure Policy](../../../policy/overview.md) que supervisan las configuraciones del registro de eventos y auditoría. La supervisión de estas configuraciones puede proporcionar un indicador del error del sistema de auditoría o de una configuración incorrecta, y le ayudarán a tomar medidas correctivas.

- [Versión preliminar]: Monitor unaudited SQL servers in Azure Security Center
- Auditar la configuración de diagnóstico
- Auditoría de las instancias administradas de SQL sin Advanced Data Security
- Auditar configuración de auditoría de SQL en el nivel de servidor
- Auditoría de los servidores de SQL sin Advanced Data Security

## <a name="au-6-4-audit-review-analysis-and-reporting--central-review-and-analysis"></a>Revisión, análisis e informes de auditoría AU-6 (4) | Revisión y análisis central

Los datos de registro que recopila Azure Monitor se almacenan en un área de trabajo de Log Analytics que permite el análisis y los informes centralizados. Este proyecto le ayuda a garantizar que los eventos se registran; para ello, asigna siete definiciones de [Azure Policy](../../../policy/overview.md) que auditan y aplican la implementación del agente de Log Analytics en las máquinas virtuales de Azure.

- [Versión preliminar]: Auditoría de la implementación del agente de Log Analytics: la imagen de la VM (SO) no está en la lista
- [Versión preliminar]: Auditoría de la implementación del agente de Log Analytics en VMSS: la imagen de la VM (SO) no está en la lista
- [Versión preliminar]: Audit Log Analytics Workspace for VM - Report Mismatch
- [Versión preliminar]: Deploy Log Analytics Agent for Linux VM Scale Sets (VMSS)
- [Versión preliminar]: Deploy Log Analytics Agent for Linux VMs
- [Versión preliminar]: Deploy Log Analytics Agent for Windows VM Scale Sets (VMSS)
- [Versión preliminar]: Deploy Log Analytics Agent for Windows VMs

## <a name="au-12-audit-generation"></a>Generación de auditoría AU-12

Este proyecto le permite garantizar que los eventos del sistema se registran. Para ello, se asignan 15 definiciones de [Azure Policy](../../../policy/overview.md) que auditan las configuraciones de registro de los recursos de Azure. Estas definiciones de directiva auditan y aplican la implementación del agente de Log Analytics en las máquinas virtuales Azure y la configuración de la auditoría para otros tipos de recursos de Azure. Estas definiciones de directiva también auditan la configuración de los registros de diagnóstico para proporcionar información detallada sobre las operaciones realizadas dentro de los recursos de Azure. Además, la auditoría y Advanced Data Security se configuran en los servidores SQL Server.

- [Versión preliminar]: Auditoría de la implementación del agente de Log Analytics: la imagen de la VM (SO) no está en la lista
- [Versión preliminar]: Auditoría de la implementación del agente de Log Analytics en VMSS: la imagen de la VM (SO) no está en la lista
- [Versión preliminar]: Audit Log Analytics Workspace for VM - Report Mismatch
- [Versión preliminar]: Deploy Log Analytics Agent for Linux VM Scale Sets (VMSS)
- [Versión preliminar]: Deploy Log Analytics Agent for Linux VMs
- [Versión preliminar]: Deploy Log Analytics Agent for Windows VM Scale Sets (VMSS)
- [Versión preliminar]: Deploy Log Analytics Agent for Windows VMs
- [Versión preliminar]: Monitor unaudited SQL servers in Azure Security Center
- Aplicación de la configuración de diagnóstico de grupos de seguridad de red
- Auditar la configuración de diagnóstico
- Auditoría de las instancias administradas de SQL sin Advanced Data Security
- Auditar configuración de auditoría de SQL en el nivel de servidor
- Auditoría de los servidores de SQL sin Advanced Data Security
- Implementación de Advanced Data Security en los servidores de SQL Server
- Implementación de auditorías en servidores SQL Server

## <a name="cm-7-2-least-functionality--prevent-program-execution"></a>Funcionalidad mínima CM-7 (2) | Evitar la ejecución de programas

El control de aplicaciones adaptable en Azure Security Center es una solución de creación de listas de aplicaciones permitidas integral, inteligente y automatizada que puede bloquear o impedir que se ejecute software específico en las máquinas virtuales. El control de aplicaciones puede ejecutarse en modo de aplicación que prohíbe que la aplicación no autorizada se ejecute. Este proyecto asigna una definición de Azure Policy que le ayuda a supervisar las máquinas virtuales cuando se recomienda crear una lista de aplicaciones permitidas, pero aún no se ha configurado.

- [Versión preliminar]: Monitor possible app whitelisting in Azure Security Center

## <a name="cm-7-5-least-functionality--authorized-software--whitelisting"></a>Funcionalidad mínima CM-7 (5) | Creación de listas de software permitido o autorizado

El control de aplicaciones adaptable en Azure Security Center es una solución de creación de listas de aplicaciones permitidas integral, inteligente y automatizada que puede bloquear o impedir que se ejecute software específico en las máquinas virtuales. El control de aplicaciones le permite crear listas de aplicaciones aprobadas para las máquinas virtuales. Este proyecto asigna una definición de [Azure Policy](../../../policy/overview.md) que le ayuda a supervisar las máquinas virtuales cuando se recomienda crear una lista de aplicaciones permitidas, pero aún no se ha configurado.

- [Versión preliminar]: Monitor possible app whitelisting in Azure Security Center

## <a name="cm-11-user-installed-software"></a>Software instalado por el usuario CM-11

El control de aplicaciones adaptable en Azure Security Center es una solución de creación de listas de aplicaciones permitidas integral, inteligente y automatizada que puede bloquear o impedir que se ejecute software específico en las máquinas virtuales. El control de aplicaciones puede ayudarle a aplicar y supervisar el cumplimiento con las directivas de restricción de software. Este proyecto asigna una definición de [Azure Policy](../../../policy/overview.md) que le ayuda a supervisar las máquinas virtuales cuando se recomienda crear una lista de aplicaciones permitidas, pero aún no se ha configurado.

- [Versión preliminar]: Monitor possible app whitelisting in Azure Security Center

## <a name="cp-7-alternate-processing-site"></a>Sitio de procesamiento alternativo CP-7

Azure Site Recovery replica las cargas de trabajo que se ejecutan en máquinas virtuales desde una ubicación remota a una ubicación secundaria. Si se produce una interrupción en el sitio primario, la carga de trabajo conmutar por error a la ubicación secundaria. Este proyecto asigna una definición de [Azure Policy](../../../policy/overview.md) que audita las máquinas virtuales que no tienen configurada la recuperación ante desastres. Supervisar este indicador puede ayudarle a asegurarse de que los controles de contingencia necesarios están en su lugar.

- Auditoría de máquinas virtuales sin la recuperación ante desastres configurada

## <a name="ia-2-1-identification-and-authentication-organizational-users--network-access-to-privileged-accounts"></a>Identificación y autenticación IA-2 (1) (usuarios de la organización) | Acceso de red a cuentas con privilegios

Este proyecto ayuda a restringir y controlar los derechos de acceso con privilegios; para ello, asigna dos definiciones de [Azure Policy](../../../policy/overview.md) para auditar las cuentas con permisos de propietario o escritura que no tengan habilitada la autenticación multifactor. La autenticación multifactor ayuda a mantener las cuentas seguras aunque algún dato de la autenticación esté en riesgo. Mediante la supervisión de las cuentas que no tienen la autenticación multifactor habilitada puede identificar las cuentas en mayor o menor riesgo.

- [Versión preliminar]: Audit accounts with owner permissions who are not MFA enabled on a subscription
- [Versión preliminar]: Audit accounts with write permissions who are not MFA enabled on a subscription

## <a name="ia-2-2-identification-and-authentication-organizational-users--network-access-to-non-privileged-accounts"></a>Identificación y autenticación (usuarios de la organización) IA-2(2) | Acceso de red a cuentas sin privilegios

Este proyecto ayuda a restringir y controlar el acceso; para ello, asigna una definición de [Azure Policy](../../../policy/overview.md) para auditar las cuentas con permisos de lectura que no tengan habilitada la autenticación multifactor. La autenticación multifactor ayuda a mantener las cuentas seguras aunque algún dato de la autenticación esté en riesgo. Mediante la supervisión de las cuentas que no tienen la autenticación multifactor habilitada puede identificar las cuentas en mayor o menor riesgo.

- [Versión preliminar]: Audit accounts with read permissions who are not MFA enabled on a subscription

## <a name="ia-5-authenticator-management"></a>Administración de autenticadores IA-5

Este proyecto asigna cinco definiciones de [Azure Policy](../../../policy/overview.md) que auditan las máquinas virtuales Linux que permiten las conexiones remotas desde cuentas sin contraseña o que tengan permisos incorrectos establecidos en el archivo de contraseñas. Este proyecto también asigna una definición de directiva que audita la conjugación del tipo de cifrado de contraseña para las máquinas virtuales Windows. La supervisión de estos indicadores le ayuda a asegurarse de que los autenticadores del sistema cumplen con la directiva de identificación y autenticación de la organización.

- [Versión preliminar]: Audit that Linux VMs do not have accounts without passwords
- [Versión preliminar]: Deploy VM extension to audit that Linux VMs do not have accounts without passwords
- [Versión preliminar]: Audit that Linux VMs have the passwd file permissions set to 0644
- [Versión preliminar]: Audit that Windows VMs store passwords using reversible encryption
- [Versión preliminar]: Deploy VM extension to audit that Linux VMs have the passwd file permissions set to 0644

## <a name="ia-5-1-authenticator-management--password-based-authentication"></a>Administración de autenticadores IA-5 (1) | Autenticación basada en contraseña

Este proyecto ayuda a instaurar contraseñas seguras; para ello, asigna 12 definiciones de [Azure Policy](../../../policy/overview.md) que auditan las máquinas virtuales Windows que no aplican unos requisitos mínimos de seguridad de contraseña y de otros tipos. Conocer las máquinas virtuales que infringen la directiva de seguridad de las contraseñas ayuda a tomar medidas correctivas para garantizar que las contraseñas de todas las cuentas de usuario de máquina virtual cumplen la directiva de contraseñas de la organización.

- [Versión preliminar]: Audit that Windows VMs cannot re-use the previous 24 passwords
- [Versión preliminar]: Audit that Windows VMs have a maximum password age of 70 days
- [Versión preliminar]: Audit that Windows VMs have a minimum password age of 1 day
- [Versión preliminar]: Audit that Windows VMs have the password complexity setting enabled
- [Versión preliminar]: Audit that Windows VMs restrict the minimum password length to 14 characters
- [Versión preliminar]: Audit that Windows VMs store passwords using reversible encryption
- [Versión preliminar]: Deploy VM extension to audit that Windows VMs cannot re-use the previous 24 passwords
- [Versión preliminar]: Deploy VM extension to audit that Windows VMs have a maximum password age of 70 days
- [Versión preliminar]: Deploy VM extension to audit that Windows VMs have a minimum password age of 1 day
- [Versión preliminar]: Deploy VM extension to audit that Windows VMs have the password complexity setting enabled
- [Versión preliminar]: Deploy VM extension to audit that Windows VMs restrict the minimum password length to 14 characters
- [Versión preliminar]: Deploy VM extension to audit that Windows VMs store passwords using reversible encryption

## <a name="ra-5-vulnerability-scanning"></a>Examen de vulnerabilidades RA-5

Este proyecto ayuda a administrar las vulnerabilidades del sistema de información; para ello, asigna cuatro definiciones de [Azure Policy](../../../policy/overview.md) que supervisan las vulnerabilidades del sistema operativo, de SQL o de las máquinas virtuales en Azure Security Center. Azure Security Center proporciona funcionalidades de informes que permiten tener información en tiempo real sobre el estado de seguridad de los recursos de Azure implementados. Este proyecto asigna también tres definiciones de directiva que auditan y aplican Advanced Data Security en servidores SQL. Advanced Data Security incluía la evaluación de vulnerabilidades y funcionalidades de protección contra amenazas avanzada para ayudarle a entender las vulnerabilidades en los recursos implementados.

- Auditoría de las instancias administradas de SQL sin Advanced Data Security
- Auditoría de los servidores de SQL sin Advanced Data Security
- Implementación de Advanced Data Security en los servidores de SQL Server
- [Versión preliminar]: Audit OS vulnerabilities on your virtual machine scale sets in Azure Security Center
- [Versión preliminar]: Monitor OS vulnerabilities in Azure Security Center
- [Versión preliminar]: Monitor SQL vulnerability assessment results in Azure Security Center
- [Versión preliminar]: Monitor VM Vulnerabilities in Azure Security Center

## <a name="sc-5-denial-of-service-protection"></a>Protección ante la denegación de servicio SC-5

El nivel estándar de denegación de servicio distribuido (DDoS) de Azure proporciona características adicionales y funcionalidades de mitigación en el nivel de servicio básico. Estas características adicionales incluyen la integración de Azure Monitor y la capacidad de revisar los informes de mitigación posteriores al ataque. Este proyecto asigna una definición de [Azure Policy](../../../policy/overview.md) que audita si está habilitado el nivel estándar de DDoS. Conocer la diferencia de capacidad entre los niveles de servicio puede ayudarle a seleccionar la mejor solución para abordar la denegación de protecciones de servicio para el entorno de Azure.

- [Versión preliminar]: Audit standard tier of DDoS protection is enabled for a virtual network

## <a name="sc-7-boundary-protection"></a>Protección de límites SC-7

Este proyecto ayuda a administrar y controlar el límite del sistema; para ello, asigna una definición de [Azure Policy](../../../policy/overview.md) que supervisa los grupos de seguridad de red con reglas permisivas. Las reglas demasiado permisivas pueden permitir el acceso de red no deseado y deben revisarse. Este proyecto también asigna una definición de directiva que supervisa las recomendaciones de protección de los grupos de seguridad de red en Azure Security Center. Azure Security Center analiza los patrones de tráfico de las máquinas virtuales de Internet y proporciona recomendaciones de reglas de grupo de seguridad de red para reducir la superficie potencial de ataque.
Además, este proyecto también asigna tres definiciones de directiva que supervisan las cuentas de almacenamiento, las aplicaciones y los puntos de conexión desprotegidos. Los puntos de conexión y las aplicaciones que no están protegidos por firewall y las cuentas de almacenamiento con acceso sin restricciones pueden permitir el acceso no deseado a la información contenida en el sistema de información.

- [Versión preliminar]: Monitor Internet-facing virtual machines for Network Security Group traffic hardening recommendations
- [Versión preliminar]: Monitor permissive network access in Azure Security Center
- [Versión preliminar]: Monitor unprotected network endpoints in Azure Security Center
- [Versión preliminar]: Monitor unprotected web application in Azure Security Center
- Auditar el acceso de red sin restricciones a cuentas de almacenamiento

## <a name="sc-7-3-boundary-protection--access-points"></a>Protección de límites SC-7 (3) | Puntos de acceso

El acceso a máquinas virtuales Just-In-Time (JIT) bloquea el tráfico entrante a las máquinas virtuales de Azure, lo que reduce la exposición a ataques y, al mismo tiempo, se proporciona acceso sencillo para conectarse a las máquinas virtuales cuando sea necesario. El acceso a máquinas virtuales JIT le ayuda a limitar el número de conexiones externas a los recursos de Azure. Esta instancia de proyecto asigna una definición de [Azure Policy](../../../policy/overview.md) que le ayuda a supervisar las máquinas virtuales que pueden admitir el acceso just-in-time pero aún no se han configurado.

- [Versión preliminar]: Supervisión del posible acceso de red Just-In-Time (JIT) en Azure Security Center

## <a name="sc-7-4-boundary-protection--external-telecommunications-services"></a>Protección de límites SC-7 (4) | Servicios de telecomunicaciones externos

El acceso a máquinas virtuales Just-In-Time (JIT) bloquea el tráfico entrante a las máquinas virtuales de Azure, lo que reduce la exposición a ataques y, al mismo tiempo, se proporciona acceso sencillo para conectarse a las máquinas virtuales cuando sea necesario. El acceso a máquinas virtuales JIT le ayuda a administrar las excepciones a la directiva de flujo de tráfico al facilitar los procesos de solicitud y aprobación de acceso. Esta instancia de proyecto asigna una definición de [Azure Policy](../../../policy/overview.md) que le ayuda a supervisar las máquinas virtuales que pueden admitir el acceso just-in-time pero aún no se han configurado.

- [Versión preliminar]: Supervisión del posible acceso de red Just-In-Time (JIT) en Azure Security Center

## <a name="sc-28-1-protection-of-information-at-rest--cryptographic-protection"></a>Protección de la información en reposo SC-28 (1) | Protección criptográfica

Este proyecto ayuda a aplicar la directiva sobre el uso de controles criptográficos para proteger la información en reposo; para ello, asigna nueve definiciones de [Azure Policy](../../../policy/overview.md) que instauran controles criptográficos específicos y auditan el uso de configuraciones criptográficas poco seguras. Conocer dónde los recursos de Azure pueden tener configuraciones criptográficas subóptimas ayuda a tomar medidas correctivas para garantizar que los recursos se configuran conforme a la directiva de seguridad de la información. En concreto, las definiciones de directiva asignadas por este proyecto requieren cifrado para las cuentas de almacenamiento del lago de datos, requerir el cifrado de datos transparente en las bases de datos SQL y auditar si falta el cifrado en las bases de datos SQL, los discos de máquinas virtuales y las variables de cuenta de automatización.

- [Versión preliminar]: Monitor unencrypted SQL databases in Azure Security Center
- [Versión preliminar]: Monitor unencrypted VM Disks in Azure Security Center
- Auditar la transferencia segura a cuentas de almacenamiento
- Auditoría de las instancias administradas de SQL sin Advanced Data Security
- Auditoría de los servidores de SQL sin Advanced Data Security
- Auditar el estado del cifrado de datos transparente
- Implementación de Advanced Data Security en los servidores de SQL Server
- Implementación del cifrado de datos transparente de SQL DB
- Forzar el cifrado en las cuentas de Data Lake Store

## <a name="si-2-flaw-remediation"></a>Corrección de errores SI-2

Este proyecto ayuda a administrar los errores del sistema de información; para ello, asigna seis definiciones de [Azure Policy](../../../policy/overview.md) que supervisan si faltan actualizaciones del sistema o si existen vulnerabilidades del sistema operativo, de SQL o de las máquinas virtuales de Azure Security Center. Azure Security Center proporciona funcionalidades de informes que permiten tener información en tiempo real sobre el estado de seguridad de los recursos de Azure implementados. Este proyecto también asigna una definición de directiva que garantiza la actualización automática del sistema operativo para conjuntos de escalado de máquinas virtuales.

- [Versión preliminar]: Audit any missing system updates on virtual machine scale sets in Azure Security Center
- [Versión preliminar]: Audit OS vulnerabilities on your virtual machine scale sets in Azure Security Center
- [Versión preliminar]: Monitor missing system updates in Azure Security Center
- [Versión preliminar]: Monitor OS vulnerabilities in Azure Security Center
- [Versión preliminar]: Monitor SQL vulnerability assessment results in Azure Security Center
- [Versión preliminar]: Monitor VM Vulnerabilities in Azure Security Center
- Aplicación de la actualización automática del sistema operativo con comprobaciones de estado de la aplicación en VMSS

## <a name="si-3-malicious-code-protection"></a>Protección frente a código malintencionado SI-3

Este proyecto le ayuda a administrar la protección de los puntos de conexión, incluida la protección del código malintencionado; para ello, asigna tres definiciones de [Azure Policy](../../../policy/overview.md) que supervisan para comprobar si falta la protección de los puntos de conexión en las máquinas virtuales en Azure Security Center y aplicar la solución antimalware de Microsoft en máquinas virtuales de Windows.

- [Versión preliminar]: Audit the endpoint protection solution on virtual machine scale sets in Azure Security Center
- [Versión preliminar]: Monitor missing Endpoint Protection in Azure Security Center
- Implementación de la extensión de Microsoft IaaSAntimalware predeterminada para Windows Server

## <a name="si-3-1-malicious-code-protection--central-management"></a>Protección frente a código malintencionado SI-3 (1) | Administración central

Este proyecto le ayuda a administrar la protección de los puntos de conexión, incluida la protección del código malintencionado; para ello, asigna dos definiciones de [Azure Policy](../../../policy/overview.md) que supervisan para comprobar si falta la protección de los puntos de conexión en las máquinas virtuales en Azure Security Center. Azure Security Center proporciona funcionalidades de informes y administración centralizada que permiten tener información en tiempo real sobre el estado de seguridad de los recursos de Azure implementados.

- [Versión preliminar]: Audit the endpoint protection solution on virtual machine scale sets in Azure Security Center
- [Versión preliminar]: Monitor missing Endpoint Protection in Azure Security Center

## <a name="si-4-information-system-monitoring"></a>Supervisión del sistema de información SI-4

Este proyecto le ayuda a supervisar el sistema de auditoría y cumplimiento de seguridad de datos y registro en los recursos de Azure. En concreto, las directivas asignaban la auditoría e implementación del agente de Log Analytics, y mejoraban la configuración de seguridad de las bases de datos SQL, las cuentas de almacenamiento y los recursos de red. Estas funcionalidades pueden ayudarle a detectar los comportamientos anómalos y los indicadores de ataques para que pueda tomar las acciones adecuadas.

- [Versión preliminar]: Auditoría de la implementación del agente de Log Analytics: la imagen de la VM (SO) no está en la lista
- [Versión preliminar]: Auditoría de la implementación del agente de Log Analytics en VMSS: la imagen de la VM (SO) no está en la lista
- [Versión preliminar]: Audit Log Analytics Workspace for VM - Report Mismatch
- [Versión preliminar]: Deploy Log Analytics Agent for Linux VM Scale Sets (VMSS)
- [Versión preliminar]: Deploy Log Analytics Agent for Linux VMs
- [Versión preliminar]: Deploy Log Analytics Agent for Windows VM Scale Sets (VMSS)
- [Versión preliminar]: Deploy Log Analytics Agent for Windows VMs
- Auditoría de las instancias administradas de SQL sin Advanced Data Security
- Auditoría de los servidores de SQL sin Advanced Data Security
- Implementación de Advanced Data Security en los servidores de SQL Server
- Implementar Advanced Threat Protection en las cuentas de almacenamiento
- Implementación de auditorías en servidores SQL Server
- Implementar Network Watcher al crear redes virtuales
- Implementar la detección de amenazas en servidores SQL Server

## <a name="si-4-18-information-system-monitoring--analyze-traffic--covert-exfiltration"></a>Supervisión del sistema de información SI-4 (18) | Análisis del tráfico o la exfiltración encubierta

Advanced Threat Protection para Azure Storage detecta los intentos poco habituales y potencialmente peligrosos de acceder a las cuentas de almacenamiento o de vulnerarlas. Las alertas de protección incluyen patrones anómalos de acceso, cargas o extracciones anómalas, y actividad sospechosa de almacenamiento. Estos indicadores pueden ayudarle a detectar la exfiltración encubierta de información.

- Implementar Advanced Threat Protection en las cuentas de almacenamiento

## <a name="next-steps"></a>Pasos siguientes

Artículos adicionales sobre planos técnicos y cómo utilizarlos:

- Más información sobre el [ciclo de vida del plano técnico](../../concepts/lifecycle.md)
- Descubra cómo utilizar [parámetros estáticos y dinámicos](../../concepts/parameters.md).
- Aprenda a personalizar el [orden de secuenciación de planos técnicos](../../concepts/sequencing-order.md).
- Averigüe cómo usar el [bloqueo de recursos de planos técnicos](../../concepts/resource-locking.md).
- Aprenda a [actualizar las asignaciones existentes](../../how-to/update-existing-assignments.md).