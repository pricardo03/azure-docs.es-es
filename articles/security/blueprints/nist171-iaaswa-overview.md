---
title: 'Azure Security and Compliance Blueprint: aplicación web IaaS para NIST SP 800-171'
description: 'Azure Security and Compliance Blueprint: aplicación web IaaS para NIST SP 800-171'
services: security
author: jomolesk
ms.assetid: 1f1ad27f-32c3-4e76-abb9-ea768d01747f
ms.service: security
ms.topic: article
ms.date: 07/31/2018
ms.author: jomolesk
ms.openlocfilehash: b30094e264086f018acbf84144300df46c60ac4e
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60610270"
---
# <a name="azure-security-and-compliance-blueprint---iaas-web-application-for-nist-sp-800-171"></a>Azure Security and Compliance Blueprint: aplicación web IaaS para NIST SP 800-171

## <a name="overview"></a>Información general
En [NIST Special Publication 800-171](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-171.pdf) se proporcionan instrucciones para proteger la información sin clasificar controlada (CUI) que reside en sistemas de información y organizaciones no federales. NIST SP 800-171 establece 14 familias de requisitos de seguridad para proteger la confidencialidad de CUI.

Azure Security and Compliance Blueprint proporciona instrucciones para ayudar a los clientes a implementar una arquitectura de aplicaciones web en Azure que implementa un subconjunto de controles de NIST SP 800-171. Esta solución muestra formas en que los clientes pueden cumplir requisitos de cumplimiento y de seguridad específicos. También sirve como base para que los clientes creen y configuren sus propias aplicaciones web en Azure.

Esta arquitectura de referencia, la guía de implementación asociada y el modelo de amenazas están diseñados como base para que los clientes puedan adaptarse a sus requisitos específicos. No deben usarse tal cual en un entorno de producción. La implementación de esta arquitectura sin modificación no es suficiente para satisfacer completamente los requisitos de NIST SP 800-171. Los clientes son responsables de llevar a cabo las evaluaciones de seguridad y cumplimiento adecuadas de todas las soluciones creadas con esta arquitectura. Los requisitos pueden variar en función de las especificaciones de implementación de cada cliente.

## <a name="architecture-diagram-and-components"></a>Componentes y diagrama de la arquitectura
Este plano técnico de seguridad y cumplimiento de Azure implementa una arquitectura de referencia para una aplicación web IaaS con un back-end de SQL Server. La arquitectura incluye un nivel de web, un nivel de datos, infraestructura de Active Directory, Azure Application Gateway y Azure Load Balancer. Las máquinas virtuales implementadas en los niveles web y de datos se configuran en un conjunto de disponibilidad. Las instancias de SQL Server se configuran en un grupo de disponibilidad AlwaysOn para alta disponibilidad. Las máquinas virtuales están unidas a un dominio. Las directivas de grupo de Active Directory aplican configuraciones de seguridad y cumplimiento a nivel del sistema operativo.

Toda la solución se basa en el servicio Azure Storage, que los clientes configuran desde Azure Portal. Azure Storage cifra todos los datos con Storage Service Encryption para mantener la confidencialidad de los datos en reposo. El almacenamiento con redundancia geográfica garantiza que un evento adverso en el centro de datos principal del cliente no comporte una pérdida de datos. Se almacena una segunda copia en una ubicación independiente situada a cientos de kilómetros de distancia.

Para mejorar la seguridad, todos los recursos de esta solución se administran como un grupo de recursos mediante Azure Resource Manager. El control de acceso basado en roles de Azure Active Directory (Azure AD) se usa para controlar el acceso a las claves y recursos implementados en Azure Key Vault. El mantenimiento del sistema se supervisa mediante Azure Monitor. Los clientes configuran ambos servicios de supervisión para capturar registros. El mantenimiento del sistema se muestra en un panel único fácil de usar.

Un host de tipo bastión de administración proporciona una conexión segura para que los administradores accedan a los recursos implementados. *Microsoft recomienda configurar una conexión VPN o de Azure ExpressRoute para la administración y la importación de datos de en la subred de la arquitectura de referencia.*


![Aplicación web IaaS para NIST SP 800-171: diagrama de arquitectura de referencia](images/nist171-iaaswa-architecture.png "Aplicación web IaaS para NIST SP 800-171: diagrama de arquitectura de referencia")

Esta solución usa los siguientes servicios de Azure. Para obtener más información, consulte la sección sobre la [arquitectura de implementación](#deployment-architecture).

- Azure Virtual Machines
    - (1) Administración/bastión (Windows Server 2016 Datacenter)
    - (2) Controlador de dominio de Active Directory (Windows Server 2016 Datacenter)
    - (2) nodo de clúster de SQL Server (SQL Server 2017 en Windows Server 2016)
    - (2) Web/IIS (Windows Server 2016 Datacenter)
- Azure Virtual Network
    - (1)/16 redes
    - (5)/24 redes
    - (5) Grupos de seguridad de red
- Conjuntos de disponibilidad
    - (1) Controladores de dominio de Active Directory
    - (1) nodos de clúster de SQL
    - (1) Web/IIS
- Azure Application Gateway
    - (1) Firewall de aplicaciones web
        - Modo de firewall: prevención
        - Conjunto de reglas: OWASP 3.0
        - Puente de agente de escucha: 443
- Azure Active Directory
- Azure Key Vault
- Azure Load Balancer
- Monitor de Azure (registros)
- Azure Resource Manager
- Azure Security Center
- Azure Storage
- Azure Automation
- Testigo de Cloud
- Almacén de Recovery Services

## <a name="deployment-architecture"></a>Arquitectura de implementación
En la siguiente sección se detallan los elementos de desarrollo e implementación.

**Host de tipo bastión**: El host bastión es el único punto de entrada que los usuarios pueden usar para tener acceso a los recursos implementados en este entorno. El host de tipo bastión proporciona una conexión segura a los recursos implementados al permitir solo el tráfico remoto desde las direcciones IP públicas de una lista segura. Para permitir el tráfico del escritorio remoto, el origen del tráfico se debe definir en el grupo de seguridad de red (NSG).

Esta solución crea una máquina virtual como host de tipo bastión unido mediante dominio con las siguientes configuraciones:
-   [Extensión antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware).
-   [Extensión de Diagnósticos de Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template).
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) con Key Vault.
-   Una [directiva de apagado automático](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) para reducir el consumo de recursos de la máquina virtual cuando no esté en uso.
-   [Credential Guard de Windows Defender](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) está habilitado para que las credenciales y otros secretos se ejecuten en un entorno protegido aislado del sistema operativo en ejecución.

### <a name="virtual-network"></a>Virtual network
La arquitectura define una red virtual privada con un espacio de direcciones de 10.200.0.0/16.

**Grupos de seguridad de red**: Esta solución implementa los recursos en una arquitectura con subredes independientes para web, base de datos, Active Directory y administración dentro de una red virtual. Las subredes se separan lógicamente con reglas de grupos de seguridad de red aplicadas a las distintas subredes. Las reglas restringen el tráfico entre las subredes a solo el que es necesario para la funcionalidad del sistema y de la administración.

Consulte la configuración de los [grupos de seguridad de red](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) implementados con esta solución. Las organizaciones pueden configurar grupos de seguridad de red editando el archivo anterior usando [esta documentación](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) como guía.

Cada una de las subredes tiene un grupo de seguridad de red dedicado:
- Un grupo de seguridad de red para Application Gateway (LBNSG)
- Un grupo de seguridad de red para el host de tipo bastión (MGTNSG)
- Un grupo de seguridad de red para los controladores de dominio principal y de copia de seguridad (ADNSG)
- Un grupo de seguridad de red para servidores SQL Server y el testigo de Cloud (SQLNSG)
- Un grupo de seguridad de red para el nivel de web (WEBNSG)

### <a name="data-in-transit"></a>Datos en tránsito
De forma predeterminada, Azure cifra todas las comunicaciones hacia y desde los centros de datos de Azure. Además, todas las transacciones a Azure Storage a través de Azure Portal se efectúan por HTTPS.

### <a name="data-at-rest"></a>Datos en reposo
La arquitectura protege los datos en reposo aplicando varias medidas. Entre estas medidas está el cifrado y la auditoría de base de datos.

**Azure Storage**: para cumplir los requisitos de los datos en reposo cifrados, todo [Storage](https://azure.microsoft.com/services/storage/) usa [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Esta característica ayuda a proteger y salvaguardar los datos en apoyo de los compromisos de seguridad y requisitos de cumplimiento de la organización definidos por la directiva NIST SP 800-171.

**Azure Disk Encryption**: Cifrado de disco se utiliza para los discos de máquina virtual IaaS con Windows cifrados. [Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) usa la característica BitLocker de Windows para proporcionar el cifrado del volumen de discos de datos y sistema operativo. La solución se integra con Key Vault para ayudarle a controlar y administrar las claves de cifrado del disco.

**SQL Server**: La instancia de SQL Server usa las siguientes medidas de seguridad de base de datos:
-   La [auditoría de SQL Server](https://docs.microsoft.com/sql/relational-databases/security/auditing/sql-server-audit-database-engine?view=sql-server-2017) realiza el seguimiento de eventos de base de datos y los escribe en registros de auditoría.
-   El [cifrado de datos transparente](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017) realiza el cifrado y descifrado en tiempo real de la base de datos, las copias de seguridad asociadas y los archivos de registro de transacciones, para proteger la información en reposo. El cifrado de datos transparente garantiza que los datos almacenados no han estado sujetos a acceso no autorizado.
-   Las [reglas de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) impiden el acceso a los servidores de bases de datos hasta que se otorgan los permisos adecuados. Asimismo, otorgan acceso a las bases de datos según la dirección IP de origen de cada solicitud.
-   Las [columnas cifradas](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-wizard?view=sql-server-2017) garantizan que los datos confidenciales nunca aparezcan como texto no cifrado en el sistema de base de datos. Una vez habilitado el cifrado de datos, solo las aplicaciones cliente o los servidores de aplicaciones con acceso a las claves pueden acceder a los datos de texto no cifrado.
- El [enmascaramiento dinámico de datos](https://docs.microsoft.com/sql/relational-databases/security/dynamic-data-masking?view=sql-server-2017) limita la exposición de los datos confidenciales al enmascarar los datos para usuarios o aplicaciones sin privilegios. Puede detectar de forma automática datos potencialmente confidenciales y sugerir las máscaras adecuadas que se pueden aplicar. El enmascaramiento dinámico de datos permite reducir el acceso para que los datos confidenciales no salgan de la base de datos por un acceso no autorizado. *Los clientes son responsables de ajustar la configuración para cumplir con el esquema de la base de datos.*

### <a name="identity-management"></a>Administración de identidades
Las siguientes tecnologías proporcionan funcionalidades de administración del acceso a datos confidenciales en el entorno de Azure:
-   [Azure AD](https://azure.microsoft.com/services/active-directory/) es el servicio de directorio y de administración de identidades multiinquilino basado en la nube de Microsoft. Todos los usuarios de esta solución se crean en Azure AD, incluidos los que acceden a la instancia de SQL Server.
-   La autenticación para acceder a la aplicación se efectúa mediante Azure AD. Para obtener más información, consulte cómo se [integran las aplicaciones con Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).
-   Los administradores pueden usar [Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) para definir permisos de acceso específicos para conceder solo el nivel de acceso que los usuarios necesitan para llevar a cabo sus tareas. En lugar de dar a cada usuario permisos ilimitados para los recursos de Azure, los administradores pueden permitir solo ciertas acciones para acceder a los datos. El acceso a la suscripción está limitado al administrador de la suscripción.
- Los clientes pueden usar [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) para minimizar el número de usuarios con acceso a determinados recursos. Los administradores pueden usar Azure AD Privileged Identity Management para detectar, restringir y supervisar las identidades con privilegios y su acceso a los recursos. Esta funcionalidad también se puede utilizar para aplicar un acceso administrativo a petición, Just-in-Time, cuando sea necesario.
- [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) detecta posibles vulnerabilidades que afecten a las identidades de una organización. Configura respuestas automatizadas a acciones sospechosas detectadas que están relacionadas con las identidades de una organización. También investiga incidentes sospechosos para tomar las medidas adecuadas para resolverlos.

### <a name="security"></a>Seguridad
**Administración de secretos**: La solución utiliza [Key Vault](https://azure.microsoft.com/services/key-vault/) para la administración de claves y secretos. Key Vault ayuda a proteger claves criptográficas y secretos usados por servicios y aplicaciones en la nube. Las siguientes funcionalidades de Key Vault ayudan a los clientes a proteger los datos:
- Se configuran directivas de acceso avanzadas según las necesidades.
- Se definen directivas de acceso a Key Vault con los permisos mínimos requeridos para las claves y los secretos.
- Todas las claves y los secretos en Key Vault tienen fechas de expiración.
- Todas las claves de Key Vault están protegidas por módulos de seguridad de hardware especializados. El tipo de clave es una clave RSA de 2048 bits protegida por un módulo de seguridad de hardware.
- Se otorgan los permisos necesarios mínimos a todos los usuarios e identidades mediante RBAC.
- Los registros de diagnóstico de Key Vault están habilitados con un período de retención de al menos 365 días.
- Las operaciones criptográficas permitidas para las claves están restringidas únicamente a las requeridas.
- La solución se integra con Key Vault para administrar los secretos y las claves de cifrado de disco de las máquinas virtuales de IaaS.

**Administración de revisiones**: Implementado como parte de esta arquitectura de referencia de las máquinas virtuales de Windows se configuran de forma predeterminada para recibir actualizaciones automáticas de Windows Update Service. Esta solución también incluye el servicio [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro), con el que se pueden crear implementaciones actualizadas para aplicar revisiones a las máquinas virtuales cuando sea necesario.

**Protección contra malware**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) para las VM que proporciona funcionalidad de protección en tiempo real que ayuda a identifica y quita virus, spyware y otro software malintencionado. Los clientes pueden configurar alertas para que se generen si un software malintencionado o no deseado conocido intenta instalarse o ejecutarse en máquinas virtuales protegidas.

**Azure Security Center**: con [Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro), los clientes pueden aplicar y administrar de forma centralizada las directivas de seguridad entre cargas de trabajo, limitar la exposición a amenazas y detectar ataques y responder a estos. Security Center también accede a las configuraciones existentes de los servicios de Azure para proporcionar recomendaciones de configuración y servicio que ayuden a mejorar la postura de seguridad y a proteger los datos.

Security Center usa una variedad de funcionalidades de detección para alertar a los clientes de posibles ataques contra sus entornos. Estas alertas contienen información útil acerca de lo que desencadenó la alerta, los recursos objetivo y el origen del ataque. Security Center cuenta con un conjunto de [alertas de seguridad predefinidas](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) que se desencadenan cuando tiene lugar una amenaza o actividad sospechosa. Los clientes pueden utilizar [reglas de alertas personalizadas](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) para definir nuevas alertas de seguridad basadas en los datos ya recopilados en el entorno.

Security Center proporciona incidentes y alertas de seguridad prioritarias. Security Center facilita a los clientes la detección y la resolución de posibles problemas de seguridad. Para cada amenaza detectada se genera un [informe de inteligencia de amenazas](https://docs.microsoft.com/azure/security-center/security-center-threat-report). Los equipos de respuesta a los incidentes pueden usar los informes para investigar y solucionar las amenazas.

Esta arquitectura de referencia usa la capacidad de [evaluación de vulnerabilidad](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations) en Security Center. Una vez configurado, un agente de asociado (por ejemplo, Qualys) informa de los datos de vulnerabilidades a la plataforma de administración del asociado. Este, a su vez, proporciona datos de supervisión del estado y las vulnerabilidades a Security Center. Los clientes pueden usar esta información para identificar rápidamente las máquinas virtuales vulnerables.

**Azure Application Gateway**: La arquitectura reduce el riesgo de vulnerabilidades de seguridad mediante el uso de application gateway con firewall de aplicaciones web configurado y el conjunto de reglas OWASP habilitado. Entre estas funcionalidades, cabe destacar:

- [SSL de un extremo a otro](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).
- Opción para habilitar la [descarga SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal).
- Opción para deshabilitar [TLS v1.0 y v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).
- [Firewall de aplicaciones web](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (modo de prevención).
- [Modo de prevención](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) con el conjunto de reglas OWASP 3.0.
- Opción para habilitar el [registro de diagnósticos](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).
- [Sondeos de estado personalizados](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal).
- [Security Center](https://azure.microsoft.com/services/security-center) y [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) proporcionan protección adicional y notificaciones. Security Center también proporciona un sistema de reputación.

### <a name="business-continuity"></a>Continuidad del negocio

**Alta disponibilidad**: La solución implementa todas las máquinas virtuales en un [conjunto de disponibilidad](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets). Los conjuntos de disponibilidad garantizan que las máquinas virtuales se distribuyan entre varios clústeres de hardware aislados para mejorar la disponibilidad. Durante un evento de mantenimiento planeado o no planeado hay al menos una máquina virtual disponible para cumplir el 99,95% del Acuerdo de Nivel de Servicio de Azure.

**Almacén de Recovery Services**: El [almacén de Recovery Services](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) aloja datos de copia de seguridad y protege todas las configuraciones de Azure Virtual Machines en esta arquitectura. Con los almacenes de Recovery Services, los clientes pueden restaurar archivos y carpetas desde una máquina virtual de IaaS sin tener que restaurar toda la máquina virtual. Este proceso acelera los tiempos de restauración.

**Testigo en la nube**: [Testigo en la nube](https://docs.microsoft.com/windows-server/failover-clustering/whats-new-in-failover-clustering#BKMK_CloudWitness) es un tipo de testigo de quórum de clúster de conmutación por error de Windows Server 2016 que utiliza Azure como punto de arbitraje. Testigo en la nube, como cualquier otro testigo del cuórum, tiene voto y puede participar en los cálculos del cuórum. Usa el almacenamiento de blobs de Azure estándar disponible públicamente. Esta disposición elimina la sobrecarga de mantenimiento adicional de las máquinas virtuales hospedadas en una nube pública.

### <a name="logging-and-auditing"></a>Registro y auditoría

Los servicios de Azure proporcionan un registro completo de la actividad de usuario y del sistema, así como de mantenimiento del sistema:
- **Registros de actividad**: los [registros de actividad](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) proporcionan información detallada sobre las operaciones realizadas en los recursos de la suscripción. Los registros de actividad pueden ayudar a determinar el iniciador de una operación, el momento en que se produce y el estado.
- **Registros de diagnóstico**: los [registros de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) son todos los registros emitidos por todos los recursos. Estos registros incluyen registros del sistema de eventos de Windows, registros de Azure Storage, registros de auditoría de Key Vault y registros de firewall y acceso a Application Gateway. Todos los registros de diagnóstico se escriben en una cuenta de almacenamiento de Azure centralizada y cifrada para su archivado. Los usuarios pueden configurar el período de retención (de hasta 730 días) para satisfacer sus requisitos específicos.

**Registros de Azure Monitor**: Estos registros se consolidan en [registros de Azure Monitor](https://azure.microsoft.com/services/log-analytics/) para procesamiento, almacenamiento e informes de panel. Una vez recopilados, los datos se organizan en tablas independientes por tipo de datos en las áreas de trabajo de Log Analytics. De este modo, todos los datos se pueden analizar juntos, independientemente de su fuente original. Security Center se integra con los registros de Azure Monitor. Los clientes pueden usar las consultas de Kusto para acceder a sus datos de eventos de seguridad y la combinamos con datos de otros servicios.

El siguiente Azure [soluciones de supervisión](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) se incluyen como parte de esta arquitectura:
-   [Active Directory Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): la solución Active Directory Health Check evalúa el riesgo y el estado de los entornos de servidor de forma periódica. Proporciona una lista priorizada de recomendaciones específicas para la infraestructura de servidor implementada.
- [SQL Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): la solución SQL Health Check evalúa el riesgo y el estado de los entornos de servidor de forma periódica. Proporciona a los clientes una lista priorizada de recomendaciones específicas de la infraestructura de servidor implementada.
- [Agent Health](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): la solución Agent Health informa de cuántos agentes se implementan y su distribución geográfica. También informa de cuántos agentes no responden y del número de agentes que envían datos operativos.
-   [Activity Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): la solución Activity Log Analytics ayuda a los clientes a analizar los registros de actividad de todas las suscripciones de Azure para un cliente.

**Azure Automation**: [Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) almacena, ejecuta y administra runbooks. En esta solución, los runbooks ayudan a recopilar registros de SQL Server. Los clientes pueden usar la solución [Change Tracking](https://docs.microsoft.com/azure/automation/automation-change-tracking) de Automation para identificar fácilmente los cambios en el entorno.

**Azure Monitor**: [Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) ayuda a los usuarios a efectuar un seguimiento del rendimiento, a mantener la seguridad y a identificar tendencias. Las organizaciones lo pueden usar para auditar, crear alertas y archivar datos. También pueden registrar llamadas API en sus recursos de Azure.

## <a name="threat-model"></a>Modelo de amenazas

El diagrama de flujo de datos de esta arquitectura de referencia está disponible para su [descarga](https://aka.ms/nist171-iaaswa-tm) y se encuentra aquí. El modelo puede ayudar a los clientes a comprender los puntos de riesgo potencial de la infraestructura del sistema al efectuar modificaciones.

![Aplicación web IaaS para NIST SP 800-171: modelo de amenazas](images/nist171-iaaswa-threat-model.png "Aplicación web IaaS para NIST SP 800-171: modelo de amenazas")

## <a name="compliance-documentation"></a>Documentación de cumplimiento

En [Azure Security and Compliance Blueprint: matriz de responsabilidades del cliente para NIST SP 800-171](https://aka.ms/nist171-crm) muestra todos los controles de seguridad que exige NIST SP 800-171. En esta matriz se detalla si la implementación de cada objetivo es responsabilidad de Microsoft, del cliente o de ambos.

En el [plano técnico de seguridad y cumplimiento de Azure: matriz de implementación de control de aplicaciones web IaaS para NIST SP 800-171](https://aka.ms/nist171-iaaswa-cim) se ofrece información sobre qué controles de NIST SP 800-171 se abordan en la arquitectura de aplicaciones web IaaS. Incluye descripciones detalladas de cómo la implementación cumple los requisitos de cada control cubierto.

## <a name="guidance-and-recommendations"></a>Instrucciones y recomendaciones
### <a name="vpn-and-expressroute"></a>VPN y ExpressRoute
Se debe configurar un túnel VPN seguro o [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) para establecer una conexión segura a los recursos implementados como parte de esta arquitectura de referencia de aplicaciones web de IaaS. Al configurar una VPN o ExpressRoute adecuadamente, los clientes pueden agregar una capa de protección para los datos en tránsito.

Mediante la implementación de un túnel VPN seguro con Azure, se puede crear una conexión privada virtual entre una red local y una red virtual de Azure. Esta conexión se efectúa a través de Internet. Los clientes pueden usar esta conexión para colocar con seguridad la información en un "túnel" dentro de un vínculo cifrado entre la red del cliente y Azure. La VPN de sitio a sitio es una tecnología segura y madura que empresas de todos los tamaños han implementado durante décadas. Se utiliza [el modo de túnel IPsec](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) en esta opción como mecanismo de cifrado.

Dado que el tráfico dentro del túnel VPN atraviesa Internet con una VPN de sitio a sitio, Microsoft ofrece otra opción de conexión aún más segura. ExpressRoute es un vínculo de WAN dedicado entre Azure y una ubicación local o un proveedor de hospedaje de Exchange. Las conexiones de ExpressRoute se conectan directamente al proveedor de telecomunicaciones del cliente. Como resultado, los datos no viajan a través de Internet y no están expuestos. Estas conexiones ofrecen más confiabilidad, velocidades más altas, menores latencias y mayor seguridad que las conexiones habituales. 

Hay [disponibles](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) procedimientos recomendados para implementar una red híbrida segura que extienda una red local a Azure.

## <a name="disclaimer"></a>Renuncia de responsabilidades

- Este documento es meramente informativo. MICROSOFT NO EFECTÚA NINGÚN TIPO DE GARANTÍA, YA SEA EXPRESA, IMPLÍCITA O ESTATUTARIA, SOBRE LA INFORMACIÓN INCLUIDA EN EL PRESENTE DOCUMENTO. Este documento se proporciona "tal cual". La información y las opiniones expresadas en este documento, como las direcciones URL y otras referencias a sitios web de Internet, pueden cambiar sin previo aviso. Los clientes que lean este documento se atienen a las consecuencias de usarlo. 
- Este documento no proporciona a los clientes ningún derecho legal sobre ninguna propiedad intelectual de ningún producto o solución de Microsoft. 
- Los clientes pueden copiar y usar este documento con fines internos y de referencia. 
- Algunas de las recomendaciones de este documento pueden provocar un aumento del uso de datos, de la red o de los recursos de procesos en Azure, lo que podría incrementar los costos de las licencias o las suscripciones de Azure. 
- Esta arquitectura está diseñada para servir como base para que los clientes puedan ajustarse a sus requisitos específicos y no debe usarse tal cual en un entorno de producción.
- Este documento se ha desarrollado como referencia y no debe usarse para definir todos los medios por los que un cliente puede cumplir normas y requisitos de cumplimiento específicos. Los clientes deben buscar apoyo legal de su organización sobre las implementaciones de cliente aprobadas.
