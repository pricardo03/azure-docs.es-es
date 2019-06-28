---
title: 'Plano técnico de seguridad y cumplimiento de Azure: aplicación web IaaS con clasificación Protegida para Australia'
description: 'Plano técnico de seguridad y cumplimiento de Azure: aplicación web IaaS con clasificación Protegida para Australia'
services: security
author: meladie
ms.assetid: f53a25c4-1c75-42d6-a0e7-a91661673891
ms.service: security
ms.topic: article
ms.date: 08/23/2018
ms.author: meladie
ms.openlocfilehash: 3c82a88ea15b52672f9bed428e2e7af40a65309c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "60610205"
---
# <a name="azure-security-and-compliance-blueprint---iaas-web-application-for-australia-protected"></a>Plano técnico de seguridad y cumplimiento de Azure: aplicación web IaaS con clasificación Protegida para Australia

## <a name="overview"></a>Información general
Este Plano técnico de seguridad y cumplimiento de Azure proporciona orientación para la implementación de un entorno de infraestructura como servicio (IaaS) adecuado para la colección, el almacenamiento y la recuperación de datos con clasificación Protegida del Gobierno de Australia que sea compatibles con los objetivos del Manual de Seguridad de la Información (ISM) del Gobierno australiano producido por la Dirección Australiana de Señales (ASD). Este plano técnico muestra una arquitectura de referencia común y ayuda a comprobar cómo tratar adecuadamente los datos confidenciales del Gobierno en un entorno seguro, de cumplimiento, con varios niveles.

Esta arquitectura de referencia, la guía de implementación, y el modelo de amenazas proporcionan una base para que los clientes se encarguen de sus propios procesos de planificación y acreditación del sistema, lo que ayuda a los clientes a implementar cargas de trabajo en Azure conforme a ASD. Los clientes pueden elegir implementar una instancia de Azure VPN Gateway o ExpressRoute para usar servicios federados y para integrar los recursos locales en recursos de Azure. Los clientes deben considerar las consecuencias de seguridad del uso de recursos locales. Es necesaria la configuración adicional para cumplir todos los requisitos, ya que pueden variar en función de las características propias de la implementación de cada cliente.

Lograr el cumplimiento de ASD requiere que un asesor de seguridad de información registrado audite el sistema. Los clientes tienen la responsabilidad de realizar las evaluaciones de seguridad y cumplimiento adecuadas de cualquier solución compilada con esta arquitectura, ya que los requisitos pueden variar en función de las características de implementación de cada cliente.

## <a name="architecture-diagram-and-components"></a>Componentes y diagrama de la arquitectura
Esta solución implementa una arquitectura de referencia para una aplicación web de IaaS con un back-end de SQL Server. La arquitectura incluye un nivel de web, un nivel de datos, infraestructura de Active Directory, Application Gateway y Load Balancer. Las máquinas virtuales implementadas en los niveles de web y de datos se configuran en un conjunto de disponibilidad y las instancias de SQL Server se configuran en un grupo de disponibilidad Always On de alta disponibilidad. Las máquinas virtuales están unidas a un dominio y las directivas de grupo de Active Directory se usan para aplicar configuraciones de seguridad y cumplimiento a nivel del sistema operativo. Un host de tipo bastión de administración proporciona una conexión segura para que los administradores accedan a los recursos implementados.

La arquitectura puede proporcionar un entorno híbrido seguro que extiende una red local a Azure, lo que permite que los usuarios corporativos de la red de área local privada de una organización o de Internet obtengan acceso a cargas de trabajo basadas en web de forma segura. Para las soluciones locales, el cliente es responsable de todos los aspectos de seguridad, las operaciones y el cumplimiento.

Los recursos de Azure incluidos en esta solución pueden conectarse a una red local o una instalación de colocalización de centro de datos (por ejemplo, CDC en Canberra) a través de una VPN IPSec con Azure VPN Gateway o a través de ExpressRoute. La decisión de utilizar una VPN debe tomarse teniendo en cuenta la clasificación de los datos transmitidos y la ruta de acceso de red. Los clientes que ejecuten cargas de trabajo críticas a gran escala con requisitos de macrodatos deben considerar la opción de usar una arquitectura de red híbrida con ExpressRoute para la conectividad de red privada con los servicios de Azure. Consulte la sección Instrucciones y recomendaciones para obtener más información sobre los mecanismos de conexión a Azure.

La federación con Azure Active Directory debe usarse para permitir que los usuarios se autentiquen con las credenciales locales y tengan acceso a todos los recursos en la nube mediante una infraestructura local de Servicios de federación de Active Directory (AD FS). Servicios de federación de Active Directory (AD FS) puede proporcionar funcionalidades de federación de identidades simplificada, protegida e inicio de sesión único web para este entorno híbrido. Consulte la sección Instrucciones y recomendaciones para obtener más información sobre la configuración de Azure Active Directory.

La solución utiliza cuentas de Azure Storage que los clientes pueden configurar para que usen Storage Service Encryption para preservar la confidencialidad de los datos en reposo. Azure almacena tres copias de los datos en una región seleccionada de un cliente para proporcionar resistencia. Las regiones de Azure se implementan en pares de regiones resistentes, y el almacenamiento con redundancia geográfica garantiza que los datos se repliquen en la segunda región también con tres copias. Esto evita que un evento adverso en la ubicación de datos principal del cliente produzca una pérdida de datos.

Para mejorar la seguridad, todos los recursos de esta solución se administran como un grupo de recursos mediante Azure Resource Manager. El control de acceso basado en roles de Azure Active Directory se utiliza para controlar el acceso a los recursos y claves implementados de Azure Key Vault. El mantenimiento del sistema se supervisa mediante Azure Security Center y Azure Monitor. Los clientes configuran ambos servicios de monitorización para capturar registros y mostrar el estado del sistema en un único panel de fácil navegación. Azure Application Gateway está configurado como firewall en modo de prevención y requiere tráfico TLS con una versión mínima de 1.2.

![Arquitectura de referencia de una aplicación web IaaS con clasificación Protegida de Australia](images/au-protected-iaaswa-architecture.png?raw=true "Diagrama de una arquitectura de referencia de una aplicación web IaaS con clasificación Protegida de Australia") 

Esta solución usa los siguientes servicios de Azure. Se puede encontrar más información en la sección [Arquitectura de implementación](#deployment-architecture).

- Conjuntos de disponibilidad
    - (1) nodos de clúster de SQL
    - (1) Web/IIS
- Azure Active Directory
- Azure Application Gateway
    - (1) Firewall de aplicaciones web
        - Modo de firewall: prevención
        - Conjunto de reglas: OWASP 3.0
        - Puente de agente de escucha: 443
- Testigo de Azure Cloud
- Azure Key Vault
- Azure Load Balancer
- Azure Monitor
- Azure Resource Manager
- Azure Security Center
- Registros de Azure Monitor
- Azure Storage
- Azure Virtual Machines
    - (1) administración/bastión (Windows Server 2016 Datacenter)
    - (2) nodo de clúster de SQL Server (SQL Server 2017 en Windows Server 2016)
    - (2) Web/IIS (Windows Server 2016 Datacenter)
- Azure Virtual Network
    - (4) Grupos de seguridad de red
    - Azure Network Watcher
- Almacén de Recovery Services

Este plano técnico contiene servicios de Azure que no están certificados para su uso en la clasificación Protegida por el Centro Australiano de Seguridad Cibernética (ACSC). ACSC ha certificado todos los servicios incluidos en esta arquitectura de referencia en el nivel de marcadores de limitación de difusión (DLM). Microsoft recomienda que los clientes revisen los informes de seguridad y auditoría publicados relacionados con estos servicios de Azure y usen su marco de administración de riesgos para determinar si el servicio de Azure es adecuado para su acreditación interna y el uso en el clasificación Protegida.

## <a name="deployment-architecture"></a>Arquitectura de implementación
En la siguiente sección se detallan los elementos de desarrollo e implementación.

**Host de tipo bastión**: el host de tipo bastión es el único punto de entrada que permite a los usuarios acceder a los recursos implementados en este entorno. El host de tipo bastión proporciona una conexión segura a los recursos implementados al permitir solo el tráfico remoto desde las direcciones IP públicas de una lista segura. Para permitir el tráfico de escritorio remoto (RDP), el origen del tráfico debe definirse en el grupo de seguridad de red.

Esta solución crea una máquina virtual como host de tipo bastión unido mediante dominio con las siguientes configuraciones:
-   [Extensión antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Extensión de Diagnósticos de Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) con Azure Key Vault
-   Una [directiva de apagado automático](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) para reducir el consumo de recursos de la máquina virtual cuando no esté en uso.

### <a name="virtual-network"></a>Virtual network
La arquitectura define una red virtual privada con un espacio de direcciones de 10.200.0.0/16.

**Grupos de seguridad de red**: Esta solución implementa los recursos en una arquitectura con una subred web independiente, la subred de la base de datos, la subred de Active Directory y la subred de administración dentro de una red virtual. Las subredes se separan de forma lógica mediante las reglas del grupo de seguridad de red aplicadas a las subredes individuales para restringir el tráfico entre subredes unicamente según sea necesario para la funcionalidad de administración y del sistema.

Vea la configuración de los [grupos de seguridad de red](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) implementados con esta solución. Las organizaciones pueden configurar grupos de seguridad de red mediante la edición del archivo anterior usando [esta documentación](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) como guía.

Cada una de las subredes tiene un grupo de seguridad de red dedicado:
- 1 grupo de seguridad de red para Application Gateway (LBNSG)
- 1 grupo de seguridad de red para host de tipo bastión (MGTNSG)
- 1 grupo de seguridad de red para servidores SQL Server y el testigo de Cloud (SQLNSG)
- 1 grupo de seguridad de red para el nivel de web (WEBNSG)

### <a name="data-in-transit"></a>Datos en tránsito
De manera predeterminada, Azure cifra todas las comunicaciones hacia y desde los centros de datos de Azure. 

Para los datos protegidos en tránsito desde las redes pertenecientes al cliente, la arquitectura utiliza Internet o ExpressRoute con una VPN Gateway configurada con IPSEC.

Además, todas las transacciones a Azure mediante el portal de administración de Azure se realizan mediante HTTPS con TLS 1.2.

### <a name="data-at-rest"></a>Datos en reposo
La arquitectura protege los datos en reposo mediante el cifrado, la auditoría de base de datos y otras medidas.

**Azure Storage**: para cumplir con los requisitos de los datos en reposo cifrados, [Azure Storage](https://azure.microsoft.com/services/storage/) usa [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Esto ayuda a proteger y salvaguardar los datos en apoyo de los compromisos de seguridad y requisitos de cumplimiento de la organización definidos por el ISM del Gobierno australiano.

**Azure Disk Encryption**: [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) aprovecha la función de BitLocker de Windows para proporcionar el cifrado del volumen de discos de datos. La solución se integra con Azure Key Vault para ayudar a controlar y administrar las claves de cifrado del disco.

**SQL Server**: La instancia de SQL Server usa las siguientes medidas de seguridad de base de datos:
-   La [auditoría de SQL Server](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine?view=sql-server-2017) realiza el seguimiento de eventos de base de datos y los escribe en registros de auditoría.
-   El [cifrado de datos transparente](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) realiza el cifrado y descifrado en tiempo real de la base de datos, las copias de seguridad asociadas y los archivos de registro de transacciones, para proteger la información en reposo. El cifrado de datos transparente garantiza que los datos almacenados no hayan estado sujetos a un acceso no autorizado.
-   Las [reglas de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) impiden el acceso a los servidores de bases de datos hasta que se otorgan los permisos adecuados. Asimismo, otorgan acceso a las bases de datos según la dirección IP de origen de cada solicitud.
-   Las [columnas cifradas](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-wizard?view=sql-server-2017) garantizan que los datos confidenciales nunca aparezcan como texto no cifrado en el sistema de base de datos. Después de habilitar el cifrado de datos, solo las aplicaciones cliente o los servidores de aplicaciones con acceso a las claves pueden acceder a los datos de texto no cifrado.
- El [enmascaramiento dinámico de datos](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking?view=sql-server-2017) limita la exposición de los datos confidenciales al enmascarar los datos para usuarios o aplicaciones sin privilegios. El enmascaramiento dinámico de datos puede detectar de forma automática datos potencialmente confidenciales y sugerir las máscaras adecuadas que se pueden aplicar. Esto contribuye a reducir el acceso de forma que los datos confidenciales no salen de la base de datos por medio de acceso no autorizado. **Los clientes son responsables de ajustar la configuración del enmascaramiento dinámico de datos para cumplir con el esquema de la base de datos.**

### <a name="identity-management"></a>Administración de identidades
Los clientes pueden utilizar de forma local Servicios de federación de Active Directory para federar con [Azure Active Directory](https://azure.microsoft.com/services/active-directory/), que es el directorio basado en la nube multiempresa y servicio de administración de identidades de Microsoft. [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) integra directorios locales con Azure Active Directory. Todos los usuarios de esta solución requieren cuentas de Azure Active Directory. Con el inicio de sesión de federación, los usuarios pueden iniciar sesión en Azure Active Directory y autenticarse en recursos de Azure mediante credenciales locales.

Además, las siguientes funcionalidades de Azure Active Directory ayudan a administrar el acceso a los datos en el entorno de Azure:
- La autenticación para acceder a la aplicación se realiza con Azure Active Directory. Para más información, consulte [Integración de aplicaciones con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).
- El [control de acceso basado en rol de Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) permite a los administradores especificar permisos de acceso bien definidos para conceder solo el nivel de acceso que los usuarios necesitan para realizar su trabajo. En lugar de dar a cada usuario permisos ilimitados para los recursos de Azure, los administradores pueden permitir solo ciertas acciones para acceder a los datos. El acceso a la suscripción está limitado al administrador de la suscripción.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) permite a los clientes minimizar el número de usuarios con acceso a determinada información. Los administradores pueden usar Azure Active Directory Privileged Identity Management para detectar, restringir y supervisar las identidades con privilegios y su acceso a los recursos. Esta funcionalidad también puede utilizarse para aplicar un acceso administrativo a petición, Just-in-Time, cuando sea necesario.
- [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) detecta posibles puntos vulnerables que afectan a las identidades de una organización, configura respuestas automatizadas para las acciones sospechosas detectadas relacionadas con esas identidades, investiga incidentes sospechosos y toma las medidas oportunas para resolverlos.

**Azure Multi-Factor Authentication**: para proteger las identidades, debe implementarse la autenticación multifactor. [Azure Multi-Factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication/) es una solución fácil de usar, escalable y confiable que proporciona un segundo método de autenticación para proteger a los usuarios. Azure Multi-Factor Authenticaton utiliza la potencia de la nube y se integra con la instancia local de Active Directory y aplicaciones personalizadas. Esta protección se extiende a los escenarios críticos de gran volumen.

### <a name="security"></a>Seguridad
**Administración de secretos**: la solución utiliza [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) para la administración de claves y secretos. Azure Key Vault ayuda a proteger claves criptográficas y secretos usados por servicios y aplicaciones en la nube. Las siguientes funcionalidades de Azure Key Vault ayudan a los clientes a proteger los datos y el acceso a dichos datos:

- Se configuran directivas de acceso avanzadas según las necesidades.
- Se definen directivas de acceso a Key Vault con los permisos mínimos requeridos para las claves y los secretos.
- Todas las claves y los secretos en Key Vault tienen fechas de expiración.
- Todas las claves de Key Vault están protegidas por módulos de seguridad de hardware especializados. El tipo de clave es una clave RSA de 2048 bits protegida por un módulo de seguridad de hardware.
- A todos los usuarios y entidades se les otorgan los permisos mínimos necesarios mediante el control de acceso basado en rol.
- Los registros de diagnóstico de Key Vault están habilitados con un período de retención de al menos 365 días.
- Las operaciones criptográficas permitidas para las claves están restringidas únicamente a las requeridas.
- La solución se integra con Azure Key Vault para administrar los secretos y las claves de cifrado del disco de la máquina virtual de IaaS.

**Administración de revisiones**: Las máquinas virtuales Windows implementadas como parte de esta arquitectura de referencia se configuran de forma predeterminada para recibir actualizaciones automáticas desde el servicio Windows Update. Esta solución también incluye el servicio [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro), mediante el cual se pueden crear implementaciones actualizadas para aplicar revisiones a las máquinas virtuales cuando sea necesario.

**Protección contra malware**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) para Virtual Machines proporciona una funcionalidad de protección en tiempo real que ayuda a identificar y eliminar virus, spyware y otro software malintencionado, con alertas que se pueden configurar en caso de que un software malintencionado o no deseado conocido se intente instalar o ejecutar en máquinas virtuales protegidas.

**Azure Security Center**: con [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro), los clientes pueden aplicar y administrar de forma centralizada las directivas de seguridad entre cargas de trabajo, limitar la exposición a amenazas y detectar ataques y responder a estos. Además, Azure Security Center accede a las configuraciones existentes de los servicios de Azure para proporcionar recomendaciones de configuración y servicio que ayuden a mejorar la postura de seguridad y a proteger los datos.

Azure Security Center usa una variedad de funcionalidades de detección para alertar a los clientes de posibles ataques contra sus entornos. Estas alertas contienen información útil acerca de lo que desencadenó la alerta, los recursos objetivo y el origen del ataque. Azure Security Center cuenta con un conjunto de [alertas de seguridad predefinidas](https://docs.microsoft.com/azure/security-center/security-center-alerts-type), que se desencadenan cuando tiene lugar una amenaza o actividad sospechosa. Las [reglas de alertas personalizadas](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) de Azure Security Center permiten a los clientes definir nuevas alertas de seguridad basadas en los datos ya recopilados en el entorno.

Azure Security Center proporciona alertas de seguridad e incidentes clasificados por orden de prioridad, lo que facilita a los clientes la detección y solución de posibles problemas de seguridad. Se genera un [informe de inteligencia de amenazas](https://docs.microsoft.com/azure/security-center/security-center-threat-report) por cada amenaza detectada para ayudar a los equipos de respuesta a incidentes a investigar y corregir las amenazas.

Además, esta arquitectura de referencia usa la [evaluación de vulnerabilidad](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations) en Azure Security Center. Una vez configurado, un agente de asociado (por ejemplo, Qualys) informa de los datos de vulnerabilidades a la plataforma de administración del asociado. A su vez, plataforma de administración del asociado proporciona a Azure Security Center, los datos de supervisión de vulnerabilidades y mantenimiento, lo que permite a los clientes identificar rápidamente las máquinas virtuales vulnerables.

**Azure Application Gateway**: la arquitectura reduce el riesgo de aparición de puntos vulnerables en la seguridad, ya que usa Azure Application Gateway con el firewall de aplicaciones web configurado y el conjunto de reglas OWASP habilitado. Entre estas funcionalidades, cabe destacar:

- [SSL de un extremo a otro](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Opción para habilitar la [descarga SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Opción para deshabilitar [TLS v1.0 y v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Firewall de aplicaciones web](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (modo de prevención)
- [Modo de prevención](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) con el conjunto de reglas OWASP 3.0
- Opción para habilitar el [registro de diagnósticos](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)
- [Sondeos de estado personalizados](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Security Center](https://azure.microsoft.com/services/security-center) y [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) que proporcionan protección adicional y notificaciones. Asimismo, Azure Security Center también proporciona un sistema de reputación.

### <a name="business-continuity"></a>Continuidad del negocio
**Alta disponibilidad**: La solución implementa todas las máquinas virtuales en un [conjunto de disponibilidad](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Los conjuntos de disponibilidad garantizan que las máquinas virtuales se distribuyan entre varios clústeres de hardware aislados para mejorar la disponibilidad. Durante un evento de mantenimiento planeado o no planeado, al menos una máquina virtual está disponible para cumplir el 99,95 % del Acuerdo de Nivel de Servicio de Azure.

**Almacén de Recovery Services**: El [almacén de Recovery Services](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) aloja datos de copia de seguridad y protege todas las configuraciones de Azure Virtual Machines en esta arquitectura. Con los almacenes de Recovery Services, los clientes pueden restaurar archivos y carpetas desde una máquina virtual de IaaS sin tener que restaurar toda la máquina virtual, lo que permite unos tiempos de restauración más rápidos.

**Testigo en la nube**: El [testigo en la nube](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) es un tipo de testigo del cuórum de clúster de conmutación por error en Windows Server 2016 que usa Azure como punto de arbitraje. El testigo en la nube, como cualquier otro testigo del cuórum, tiene voto y puede participar en los cálculos del cuórum, pero usa Azure Blob Storage estándar públicamente disponible. Con esto, se elimina la sobrecarga de mantenimiento adicional de las máquinas virtuales hospedadas en una nube pública.

### <a name="logging-and-auditing"></a>Registro y auditoría
Los servicios de Azure proporcionan un registro completo de la actividad de usuario y del sistema, así como de mantenimiento del sistema:
- **Registros de actividad**: los [registros de actividad](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) proporcionan información detallada sobre las operaciones realizadas en los recursos de la suscripción. Los registros de actividad pueden ayudar a determinar el iniciador de una operación, el momento en que se produce y el estado.
- **Registros de diagnóstico**: los [registros de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) son todos los registros emitidos por todos los recursos. Estos registros incluyen registros del sistema de eventos de Windows, registros de Azure Storage, registros de auditoría de Key Vault, y registros de firewall y acceso a Application Gateway. Todos los registros de diagnóstico se escriben en una cuenta de almacenamiento de Azure centralizada y cifrada para su archivado. El usuario puede configurar la retención hasta 730 días para cumplir los requisitos de retención específicos de una organización.

**Registros de Azure Monitor**: esos registros se consolidan en [registros de Azure Monitor](https://azure.microsoft.com/services/log-analytics/) para el procesamiento, el almacenamiento y la creación de informes de panel. Una vez recopilados, se organizan en tablas independientes para cada tipo, lo que permite que todos los datos se puedan analizar conjuntamente con independencia de su origen. Además, Azure Security Center se integra con los registros de Azure Monitor, lo que permite a los clientes usar consultas de Kusto para acceder a sus datos de eventos de seguridad y combinarlos con datos de otros servicios.

Como parte de esta arquitectura se incluyen las siguientes [soluciones de supervisión](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) de Azure:
-   [Active Directory Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): la solución Active Directory Health Check evalúa el riesgo y el estado de los entornos de servidor a intervalos regulares y proporciona una lista prioritaria de recomendaciones específicas para la infraestructura de servidor implementada.
- [SQL Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): la solución SQL Health Check evalúa el riesgo y el estado de los entornos de servidor a intervalos regulares y proporciona a los clientes una lista prioritaria de recomendaciones específicas para la infraestructura de servidor implementada.
- [Agent Health](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): la solución Agent Health notifica el número de agentes implementados y su distribución geográfica, así como el número de agentes que no responden y el de agentes que envían datos operativos.
-   [Activity Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): la solución Activity Log Analytics ayuda a los clientes a analizar los registros de actividad de todas las suscripciones de Azure para un cliente.

**Azure Automation**: [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) almacena, ejecuta y administra runbooks. En esta solución, los runbooks ayudan a recopilar registros de Azure SQL Server. La solución [Change Tracking](https://docs.microsoft.com/azure/automation/automation-change-tracking) de Automation permite a los clientes identificar fácilmente los cambios en el entorno.

**Azure Monitor**: [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) ayuda a los usuarios a realizar un seguimiento del rendimiento, mantener la seguridad e identificar tendencias y permite a las organizaciones auditar, crear alertas y archivar datos, incluido el seguimiento de las llamadas a API en sus recursos de Azure.

[Azure Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview): Azure Network Watcher proporciona herramientas para supervisar, diagnosticar, ver las métricas y habilitar o deshabilitar registros de recursos en una red virtual de Azure.  Las entidades de la Commonwealth deben implementar los registros de flujo de Network Watcher para grupos de seguridad de red y máquinas virtuales. Estos registros deben almacenarse en una cuenta de almacenamiento dedicada donde solo se guarden registros de seguridad; el acceso a la cuenta de almacenamiento debe estar protegida con controles de acceso basados en rol.

## <a name="threat-model"></a>Modelo de amenazas
El diagrama de flujo de datos de esta arquitectura de referencia está disponible para su [descarga](https://aka.ms/au-protected-iaaswa-tm) y se encuentra a continuación. El modelo puede ayudar a los clientes a comprender los puntos de riesgo potencial de la infraestructura del sistema al realizar modificaciones.

![Modelo de amenazas de aplicación web de IaaS con clasificación Protegida de Australia](images/au-protected-iaaswa-threat-model.png?raw=true "Diagrama de modelo de amenazas de aplicación web de IaaS con clasificación Protegida de Australia")

## <a name="compliance-documentation"></a>Documentación de cumplimiento
Microsoft produce esta documentación de cumplimiento en función de las plataformas y los servicios disponibles en Microsoft. Debido a la gran variedad de implementaciones de clientes, esta documentación proporciona un enfoque generalizado para una solución que solo se hospeda en el entorno de Azure. Los clientes pueden identificar y utilizar otros productos y servicios según sus propios entornos operativos y resultados empresariales. Los clientes que elijan utilizar recursos locales deben resolver la seguridad y las operaciones de esos recursos locales. Los clientes pueden personalizar la solución documentada para abordar sus requisitos locales y de seguridad específicos.

[Azure Security and Compliance Blueprint - AU-PROTECTED Customer Responsibility Matrix](https://aka.ms/au-protected-crm) (Plano técnico de seguridad y cumplimiento de Azure: matriz de responsabilidades del cliente con clasificación Protegida de Australia) muestra todos los controles de seguridad que exige la clasificación Protegida de Australia. En esta matriz se detalla si la implementación de cada objetivo es responsabilidad de Microsoft, del cliente o de ambos.

[Azure Security and Compliance Blueprint - AU-PROTECTED IaaS Web Application Implementation Matrix](https://aka.ms/au-protected-iaaswa-cim) (Plano técnico de seguridad y cumplimiento de Azure: matriz de implementación de controles de aplicación web IaaS con clasificación Protegida de Australia) se proporciona información sobre qué controles de clasificación Protegida de Australia se abordan en la arquitectura de aplicaciones web PaaS; se incluyen descripciones detalladas de cómo la implementación satisface los requisitos de cada control tratado.

## <a name="guidance-and-recommendations"></a>Instrucciones y recomendaciones
### <a name="vpn-and-expressroute"></a>VPN y ExpressRoute
Para la información clasificada, debe configurarse un túnel VPN IPSec seguro para establecer una conexión segura a los recursos implementados como parte de esta arquitectura de referencia de aplicaciones web de IaaS. Al configurar una VPN IPSec adecuadamente, los clientes pueden agregar una capa de protección para los datos en tránsito.

Mediante la implementación de un túnel VPN IPSec seguro con Azure, se puede crear una conexión privada virtual entre una red local y una red virtual de Azure. Esta conexión puede tener lugar a través de Internet y permite a los clientes colocar con seguridad la información en un "túnel" dentro de un vínculo cifrado entre la red del cliente y Azure. La VPN de sitio a sitio es una tecnología segura y madura que empresas de todos los tamaños han implementado durante décadas. 

Dado que el tráfico dentro del túnel VPN atraviesa Internet con una VPN de sitio a sitio, Microsoft ofrece una opción de conexión privada. Azure ExpressRoute es un vínculo dedicado entre Azure y una ubicación local o un proveedor de hospedaje de Exchange, y se considera una red privada. Como las conexiones de ExpressRoute no se realizan a través de Internet, estas conexiones ofrecen más confiabilidad, más velocidad y menor latencia que las conexiones a Internet normales. Además, dado que se trata de una conexión directa del proveedor de telecomunicaciones del cliente, los datos no viajan a través de Internet y, por lo tanto, no están expuestos a ella.

Hay [disponibles](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) procedimientos recomendados para implementar una red híbrida segura que extienda una red local a Azure. 

Para ayudar a proteger los datos clasificados, si usa Internet o Azure ExpressRoute, los clientes deben configurar su VPN IPSec aplicando las siguientes opciones:

• El iniciador VPN del cliente debe configurarse para una vigencia de SA no superior a 14 400 segundos.
• El iniciador VPN del cliente debe deshabilitar el modo absoluto IKEv1.
• El iniciador VPN del cliente debe configurar confidencialidad directa total (PFS).
• El iniciador VPN del cliente debe configurar el uso de HMAC-SHA256 o superior.

Las opciones de configuración para los dispositivos VPN y los parámetros de IPSec/IKE están [disponibles](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices) para su revisión.

### <a name="azure-active-directory-setup"></a>Configuración de Azure Active Directory
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) es esencial para la administración de la implementación y la concesión de acceso a las personas que interactúan con el entorno. Se puede integrar una instancia de Windows Server Active Directory con Azure Active Directory en [cuatro clics](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express).

Además, [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) permite a los clientes configurar la federación con [Servicios de federación de Active Directory (AD FS)]( https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-azure-adfs) locales y Azure Active Directory. Con el inicio de sesión de federación, los clientes pueden permitir que los usuarios inicien sesión en los servicios basados en Azure Active Directory con sus contraseñas locales y sin necesidad de volver a escribir sus contraseñas mientras se encuentren en la red corporativa. Mediante la opción de federación con Servicios de federación de Active Directory (AD FS), puede implementar una nueva instalación de AD FS o puede especificar una instalación existente en una granja de Windows Server 2012 R2.

Para evitar que los datos clasificados se sincronicen con Azure Active Directory, los clientes pueden restringir los atributos que se replican en Azure Active Directory mediante la aplicación de la siguiente configuración en Azure Active Directory Connect:
- [Habilitar el filtrado](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-configure-filtering)
- [Deshabilitar la sincronización de hash de contraseñas](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization)
- [Deshabilitar la escritura diferida de contraseñas](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr)
- [Deshabilitar la escritura diferida de dispositivo](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-feature-device-writeback)
- Deje la configuración predeterminada para [evitar eliminaciones accidentales](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-prevent-accidental-deletes) y [actualizaciones automáticas](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-feature-automatic-upgrade)

## <a name="disclaimer"></a>Renuncia de responsabilidades
- Este documento es meramente informativo. MICROSOFT NO EFECTÚA NINGÚN TIPO DE GARANTÍA, YA SEA EXPRESA, IMPLÍCITA O ESTATUTARIA, SOBRE LA INFORMACIÓN INCLUIDA EN EL PRESENTE DOCUMENTO. Este documento se proporciona "tal cual". La información y las opiniones expresadas en este documento, como las direcciones URL y otras referencias a sitios web de Internet, pueden cambiar sin previo aviso. Los clientes que lean este documento se atienen a las consecuencias de usarlo.
- Este documento no proporciona a los clientes ningún derecho legal sobre ninguna propiedad intelectual de ningún producto o solución de Microsoft.
- Los clientes pueden copiar y usar este documento con fines internos y de referencia.
- Algunas de las recomendaciones de este documento pueden provocar un aumento del uso de datos, de la red o de los recursos de procesos en Azure, lo que podría incrementar los costos de las licencias o las suscripciones de Azure.
- Esta arquitectura está diseñada para servir como base para que los clientes puedan ajustarse a sus requisitos específicos y no debe usarse tal cual en un entorno de producción.
- Este documento se ha desarrollado como referencia y no debe usarse para definir todos los medios por los que un cliente puede cumplir normas y requisitos de cumplimiento específicos. Los clientes deben buscar apoyo legal de su organización sobre las implementaciones de cliente aprobadas.
