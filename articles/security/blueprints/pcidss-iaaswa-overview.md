---
title: 'Azure Security and Compliance Blueprint: aplicación web IaaS para PCI DSS'
description: 'Azure Security and Compliance Blueprint: aplicación web IaaS para PCI DSS'
services: security
author: meladie
ms.assetid: 9c825380-2abe-4fdd-800c-59d1fed1780b
ms.service: security
ms.topic: article
ms.date: 07/03/2018
ms.author: meladie
ms.openlocfilehash: 361d7683790128aafb11d6581c6e672b68ad893f
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60609230"
---
# <a name="azure-security-and-compliance-blueprint-iaas-web-application-for-pci-dss"></a>Plano técnico de seguridad y cumplimiento de Azure: Aplicación Web de IaaS de PCI DSS

## <a name="overview"></a>Información general

Este plano técnico Azure Security and Compliance Blueprint proporciona una guía para la implementación de un entorno de infraestructura como servicio (IaaS) compatible con las Normas de seguridad de datos del sector de las tarjetas de pago (PCI DSS 3.2) adecuado para la colección, almacenamiento y recuperación de datos de los titulares de tarjetas. Muestra una arquitectura de referencia común e indica cómo tratar adecuadamente los datos de una tarjeta de crédito (incluyendo el número de tarjeta, la fecha de caducidad y los datos de comprobación) en un entorno seguro, de niveles múltiples y compatible. Este plano técnico muestra una solución completa para satisfacer las necesidades de organizaciones que buscan un enfoque basado en la nube para reducir la carga y los costos de implementación.

Esta arquitectura de referencia, guía de implementación y modelo de amenazas proporcionan una base para que los clientes cumplan con los requisitos de PCI DSS 3.2. Esta solución proporciona una base de referencia para ayudar a los clientes a implementar cargas de trabajo en Azure de manera conforme a PCI DSS 3.2, sin embargo, esta solución no se debe usar tal como está en un entorno de producción porque se requiere configuración adicional.

Para cumplir con los requisitos de PCI DSS, debe conseguir que un asesor de seguridad cualificado (QSA) y acreditado certifique una solución de cliente de producción. Los clientes tienen la responsabilidad de realizar las evaluaciones de seguridad y cumplimiento adecuadas de cualquier solución compilada con esta arquitectura, ya que los requisitos pueden variar en función de las características de implementación de cada cliente.

## <a name="architecture-diagram-and-components"></a>Componentes y diagrama de la arquitectura

Esta solución implementa una arquitectura de referencia para una aplicación web de IaaS con un back-end de SQL Server. La arquitectura incluye un nivel de web, un nivel de datos, infraestructura de Active Directory, Application Gateway y Load Balancer. Las máquinas virtuales implementadas en los niveles de web y de datos se configuran en un conjunto de disponibilidad y las instancias de SQL Server se configuran en un grupo de disponibilidad Always On de alta disponibilidad. Las máquinas virtuales están unidas a un dominio y las directivas de grupo de Active Directory se usan para aplicar configuraciones de seguridad y cumplimiento a nivel del sistema operativo. Un host de tipo bastión de administración proporciona una conexión segura para que los administradores accedan a los recursos implementados. **Azure recomienda configurar una conexión VPN o de ExpressRoute para la administración y la importación de datos en la subred de la arquitectura de referencia.**

![Diagrama de arquitectura de referencia de aplicación web IaaS para PCI DSS](images/pcidss-iaaswa-architecture.png "IaaS Web Application for PCI DSS reference architecture diagram")

Esta solución usa los siguientes servicios de Azure. Los detalles de la arquitectura de implementación se encuentran en la sección [Arquitectura de implementación](#deployment-architecture).

- Conjuntos de disponibilidad
    - (1) Controladores de dominio de Active Directory
    - (1) nodos de clúster de SQL
    - (1) Web/IIS
- Azure Active Directory
- Azure Application Gateway
    - (1) Firewall de aplicaciones web
        - Modo de firewall: prevención
        - Conjunto de reglas: OWASP 3.0
        - Puente de agente de escucha: 443
- Azure Key Vault
- Azure Load Balancer
- Azure Monitor
- Azure Resource Manager
- Azure Security Center
- Azure Storage
    - (7) Cuentas de almacenamiento con redundancia geográfica
- Azure Virtual Machines
    - (1) administración/bastión (Windows Server 2016 Datacenter)
    - (2) Controlador de dominio de Active Directory (Windows Server 2016 Datacenter)
    - (2) nodo de clúster de SQL Server (SQL Server 2017 en Windows Server 2016)
    - (2) Web/IIS (Windows Server 2016 Datacenter)
- Azure Virtual Network
    - (1) /16 redes
    - (5) /24 redes
    - (5) Grupos de seguridad de red
- Testigo de Cloud
- Almacén de Recovery Services

## <a name="deployment-architecture"></a>Arquitectura de implementación

En la siguiente sección se detallan los elementos de desarrollo e implementación.

**Host de tipo bastión**: el host de tipo bastión es el único punto de entrada que permite a los usuarios acceder a los recursos implementados en este entorno. El host de tipo bastión proporciona una conexión segura a los recursos implementados al permitir solo el tráfico remoto desde las direcciones IP públicas de una lista segura. Para permitir el tráfico de escritorio remoto (RDP), el origen del tráfico debe definirse en el grupo de seguridad de red.

Esta solución crea una máquina virtual como host de tipo bastión unido mediante dominio con las siguientes configuraciones:
-   [Extensión antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Extensión de Diagnósticos de Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) con Azure Key Vault
-   Una [directiva de apagado automático](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) para reducir el consumo de recursos de la máquina virtual cuando no esté en uso.
-   [Credential Guard de Windows Defender](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) habilitado para que las credenciales y otros secretos se ejecuten en un entorno protegido aislado del sistema operativo en ejecución

### <a name="virtual-network"></a>Virtual network

La arquitectura define una red virtual privada con un espacio de direcciones de 10.200.0.0/16.

**Grupos de seguridad de red**: Esta solución implementa los recursos en una arquitectura con una subred web independiente, la subred de la base de datos, la subred de Active Directory y la subred de administración dentro de una red virtual. Las subredes se separan de forma lógica mediante las reglas del grupo de seguridad de red aplicadas a las subredes individuales para restringir el tráfico entre subredes unicamente según sea necesario para la funcionalidad de administración y del sistema.

Vea la configuración de los [grupos de seguridad de red](https://github.com/Azure/fedramp-iaas-webapp/blob/master/nestedtemplates/virtualNetworkNSG.json) implementada con esta solución. Las organizaciones pueden configurar grupos de seguridad de red mediante la edición del archivo anterior usando [esta documentación](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) como guía.

Cada una de las subredes tiene un grupo de seguridad de red dedicado:
- 1 grupo de seguridad de red para Application Gateway (LBNSG)
- 1 grupo de seguridad de red para host de tipo bastión (MGTNSG)
- 1 grupo de seguridad de red para los controladores de dominio principal y de copia de seguridad (ADNSG)
- 1 grupo de seguridad de red para servidores SQL Server y el testigo de Cloud (SQLNSG)
- 1 grupo de seguridad de red para el nivel de web (WEBNSG)

### <a name="data-in-transit"></a>Datos en tránsito

De manera predeterminada, Azure cifra todas las comunicaciones hacia y desde los centros de datos de Azure. Además, todas las transacciones a Azure Storage a través de Azure Portal se realizan mediante HTTPS.

### <a name="data-at-rest"></a>Datos en reposo

La arquitectura protege los datos en reposo mediante el cifrado, la auditoría de base de datos y otras medidas.

**Azure Storage**: para cumplir con los requisitos de los datos en reposo cifrados, [Azure Storage](https://azure.microsoft.com/services/storage/) usa [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Esto ayuda a proteger los datos de los titulares de tarjetas en cumplimiento de los compromisos de seguridad de la organización y los requisitos de cumplimiento definidos por PCI DSS 3.2.

**Azure Disk Encryption**: Azure Disk Encryption se utiliza para los discos cifrados de máquinas virtuales de IaaS de Windows. [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) aprovecha la función de BitLocker de Windows para proporcionar el cifrado del volumen de discos de datos y sistema operativo. La solución se integra con Azure Key Vault para ayudarle a controlar y administrar las claves de cifrado del disco.

**SQL Server**: La instancia de SQL Server usa las siguientes medidas de seguridad de base de datos:
- La [autenticación y autorización de Active Directory](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) permiten la administración de identidades de usuarios de bases de datos y otros servicios de Microsoft en una ubicación central.
-   La [auditoría de bases de datos SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) realiza un seguimiento de eventos de bases de datos y los escribe en un registro de auditoría de una cuenta de almacenamiento de Azure.
-   SQL Database está configurado para usar el [cifrado de datos transparente](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), que realiza el cifrado y descifrado en tiempo real de la base de datos, las copias de seguridad asociadas y los archivos de registro de transacciones, para proteger la información en reposo. El cifrado de datos transparente garantiza que los datos almacenados de los titulares de tarjetas no hayan estado sujetos a un acceso no autorizado.
-   Las [reglas de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) impiden el acceso a los servidores de bases de datos hasta que se otorgan los permisos adecuados. Asimismo, otorgan acceso a las bases de datos según la dirección IP de origen de cada solicitud.
-   La [detección de amenazas de SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) permite detectar y responder ante posibles amenazas a medida que se producen, mediante el envío de alertas de seguridad para actividades sospechosas en la base de datos, posibles puntos vulnerables, ataques por inyección de código SQL y patrones anómalos de acceso a la base de datos.
-   Las [columnas de Always Encrypted](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) garantizan que los datos confidenciales de los titulares de tarjetas nunca aparecen como texto no cifrado en el sistema de base de datos. Después de habilitar el cifrado de datos, solo las aplicaciones cliente o los servidores de aplicaciones con acceso a las claves pueden acceder a los datos de texto no cifrado.
- La característica [Propiedades extendidas](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) puede utilizarse para interrumpir el procesamiento de los titulares de los datos, ya que permite a los usuarios agregar propiedades personalizadas a los objetos de la base de datos y etiquetar los datos como "interrumpidos" para admitir la lógica de la aplicación y evitar el procesamiento de los datos asociados de los titulares de tarjetas.
- La [seguridad de nivel de fila](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) permite a los usuarios definir directivas para restringir el acceso a los datos a fin de interrumpir su procesamiento.
- El [enmascaramiento dinámico de datos de SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) limita la exposición de datos confidenciales de los titulares de tarjetas al enmascarar los datos para usuarios o aplicaciones sin privilegios. El enmascaramiento dinámico de datos puede detectar de forma automática datos potencialmente confidenciales y sugerir las máscaras adecuadas que se pueden aplicar. Esto ayuda a identificar y reducir el acceso a los datos de los titulares de tarjetas de modo que no salgan de la base de datos a través de un acceso no autorizado. Los clientes son responsables de ajustar la configuración del enmascaramiento dinámico de datos para cumplir con el esquema de la base de datos.

### <a name="identity-management"></a>Administración de identidades

Las siguientes tecnologías proporcionan funcionalidades de administración del acceso a datos confidenciales en el entorno de Azure:

- [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) es el directorio multiinquilino basado en la nube y el servicio de administración de identidades de Microsoft. Todos los usuarios de la solución se crean en Azure Active Directory, incluidos los usuarios que acceden a Azure SQL Database.
- La autenticación para acceder a la aplicación se realiza con Azure Active Directory. Para más información, consulte [Integración de aplicaciones con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Además, el cifrado de columnas de la base de datos usa Azure Active Directory para autenticar la aplicación en Azure SQL Database. Para más información, consulte cómo [proteger los datos confidenciales en Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
- El [control de acceso basado en rol de Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) permite a los administradores especificar permisos de acceso bien definidos para conceder solo el nivel de acceso que los usuarios necesitan para realizar su trabajo. En lugar de dar a cada usuario permisos ilimitados para los recursos de Azure, los administradores pueden permitir solo ciertas acciones para acceder a los datos. El acceso a la suscripción está limitado al administrador de la suscripción.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) permite a los clientes minimizar el número de usuarios con acceso a determinada información. Los administradores pueden usar Azure Active Directory Privileged Identity Management para detectar, restringir y supervisar las identidades con privilegios y su acceso a los recursos. Esta funcionalidad también puede utilizarse para aplicar un acceso administrativo a petición, Just-in-Time, cuando sea necesario.
- [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) detecta posibles puntos vulnerables que afectan a las identidades de una organización, configura respuestas automatizadas para las acciones sospechosas detectadas relacionadas con esas identidades, investiga incidentes sospechosos y toma las medidas oportunas para resolverlos.

### <a name="security"></a>Seguridad

**Administración de secretos**: la solución utiliza [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) para la administración de claves y secretos. Azure Key Vault ayuda a proteger claves criptográficas y secretos usados por servicios y aplicaciones en la nube. Las siguientes funcionalidades de Azure Key Vault ayudan a los clientes a proteger los datos y el acceso a dichos datos:

- Se configuran directivas de acceso avanzadas según las necesidades.
- Se definen directivas de acceso a Key Vault con los permisos mínimos requeridos para las claves y los secretos.
- Todas las claves y los secretos en Key Vault tienen fechas de expiración.
- Todas las claves de Key Vault están protegidas por módulos de seguridad de hardware especializados. El tipo de clave es una clave RSA de 2048 bits protegida por HSM.
- A todos los usuarios y entidades se les otorgan los permisos mínimos necesarios mediante el control de acceso basado en rol.
- Los registros de diagnóstico de Key Vault están habilitados con un período de retención de al menos 365 días.
- Las operaciones criptográficas permitidas para las claves están restringidas únicamente a las requeridas.
- La solución se integra con Azure Key Vault para administrar los secretos y las claves de cifrado del disco de la máquina virtual de IaaS.

**Administración de revisiones**: Las máquinas virtuales Windows implementadas como parte de esta arquitectura de referencia se configuran de forma predeterminada para recibir actualizaciones automáticas desde el servicio Windows Update. Esta solución también incluye el servicio [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro), mediante el cual se pueden crear implementaciones actualizadas para aplicar revisiones a las máquinas virtuales cuando sea necesario.

**Protección contra malware**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) para Virtual Machines proporciona una funcionalidad de protección en tiempo real que ayuda a identificar y eliminar virus, spyware y otro software malintencionado, con alertas que se pueden configurar en caso de que un software malintencionado o no deseado conocido se intente instalar o ejecutar en máquinas virtuales protegidas.

**Azure Security Center**: con [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro), los clientes pueden aplicar y administrar de forma centralizada las directivas de seguridad entre cargas de trabajo, limitar la exposición a amenazas y detectar ataques y responder a estos. Además, Azure Security Center accede a las configuraciones existentes de los servicios de Azure para proporcionar recomendaciones de configuración y servicio que ayuden a mejorar la postura de seguridad y a proteger los datos.

Azure Security Center usa una variedad de funcionalidades de detección para alertar a los clientes de posibles ataques contra sus entornos. Estas alertas contienen información útil acerca de lo que desencadenó la alerta, los recursos objetivo y el origen del ataque. Azure Security Center cuenta con un conjunto de [alertas de seguridad predefinidas](https://docs.microsoft.com/azure/security-center/security-center-alerts-type), que se desencadenan cuando tiene lugar una amenaza o actividad sospechosa. Las [reglas de alertas personalizadas](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) de Azure Security Center permiten a los clientes definir nuevas alertas de seguridad basadas en los datos ya recopilados en el entorno.

Azure Security Center proporciona alertas de seguridad e incidentes clasificados por orden de prioridad, lo que facilita a los clientes la detección y solución de posibles problemas de seguridad. Se genera un [informe de inteligencia de amenazas](https://docs.microsoft.com/azure/security-center/security-center-threat-report) por cada amenaza detectada para ayudar a los equipos de respuesta a incidentes a investigar y corregir las amenazas.

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

**Registros de Azure Monitor**: Estos registros se consolidan en [registros de Azure Monitor](https://azure.microsoft.com/services/log-analytics/) para procesamiento, almacenamiento e informes de panel. Una vez recopilados, los datos se organizan en tablas independientes para cada tipo de datos dentro de las áreas de trabajo de Log Analytics, lo que permite que todos los datos se puedan analizar conjuntamente con independencia de su origen. Además, Azure Security Center se integra con los registros de Azure Monitor permite a los clientes usar consultas de Kusto para acceder a sus datos de eventos de seguridad y la combinamos con datos de otros servicios.

El siguiente Azure [soluciones de supervisión](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) se incluyen como parte de esta arquitectura:
-   [Active Directory Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): la solución Active Directory Health Check evalúa el riesgo y el estado de los entornos de servidor a intervalos regulares y proporciona una lista prioritaria de recomendaciones específicas para la infraestructura de servidor implementada.
- [SQL Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): la solución SQL Health Check evalúa el riesgo y el estado de los entornos de servidor a intervalos regulares y proporciona a los clientes una lista prioritaria de recomendaciones específicas para la infraestructura de servidor implementada.
- [Agent Health](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): la solución Agent Health notifica el número de agentes implementados y su distribución geográfica, así como el número de agentes que no responden y el de agentes que envían datos operativos.
-   [Activity Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): la solución Activity Log Analytics ayuda a los clientes a analizar los registros de actividad de todas las suscripciones de Azure para un cliente.

**Azure Automation**: [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) almacena, ejecuta y administra runbooks. En esta solución, los runbooks ayudan a recopilar registros de Azure SQL Database. La solución [Change Tracking](https://docs.microsoft.com/azure/automation/automation-change-tracking) de Automation permite a los clientes identificar fácilmente los cambios en el entorno.

**Azure Monitor**: [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) ayuda a los usuarios a realizar un seguimiento del rendimiento, mantener la seguridad e identificar tendencias y permite a las organizaciones auditar, crear alertas y archivar datos, incluido el seguimiento de las llamadas a API en sus recursos de Azure.

## <a name="threat-model"></a>Modelo de amenazas

El diagrama de flujo de datos (DFD) de esta arquitectura de referencia está disponible para [descarga](https://aka.ms/pcidss-iaaswa-tm) y se encuentra a continuación. El modelo puede ayudar a los clientes a comprender los puntos de riesgo potencial de la infraestructura del sistema al realizar modificaciones.

![Diagrama de arquitectura de referencia de aplicación web IaaS para PCI DSS](images/pcidss-iaaswa-threat-model.png "IaaS Web Application for PCI DSS reference architecture diagram")

## <a name="compliance-documentation"></a>Documentación de cumplimiento

En [Azure Security and Compliance Blueprint: matriz de responsabilidades de clientes para PCI DSS](https://aka.ms/pcidss-crm) se enumeran las responsabilidades para todos los requisitos de PCI DSS 3.2.

En [Azure Security and Compliance Blueprint: matriz de implementación de aplicaciones web IaaS para PCI DSS](https://aka.ms/pcidss-iaaswa-cim) se ofrece información sobre qué requisitos de PCI DSS 3.2 aborda la arquitectura de aplicaciones web IaaS, incluida una descripción detallada de cómo la implementación cumple los requisitos de cada artículo cubierto.

## <a name="guidance-and-recommendations"></a>Instrucciones y recomendaciones

### <a name="vpn-and-expressroute"></a>VPN y ExpressRoute

Debe configurarse un túnel VPN seguro o [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) para establecer una conexión segura a los recursos implementados como parte de esta arquitectura de referencia de aplicaciones web de IaaS. Al configurar una VPN o ExpressRoute adecuadamente, los clientes pueden agregar una capa de protección para los datos en tránsito.

Mediante la implementación de un túnel VPN seguro con Azure, se puede crear una conexión privada virtual entre una red local y una red virtual de Azure. Esta conexión tiene lugar a través de Internet y permite a los clientes colocar con seguridad la información en un "túnel" dentro de un vínculo cifrado entre la red del cliente y Azure. La VPN de sitio a sitio es una tecnología segura y madura que empresas de todos los tamaños han implementado durante décadas. Se utiliza [el modo de túnel IPsec](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) en esta opción como mecanismo de cifrado.

Dado que el tráfico dentro del túnel VPN atraviesa Internet con una VPN de sitio a sitio, Microsoft ofrece otra opción de conexión aún más segura. Azure ExpressRoute es un vínculo de WAN dedicado entre Azure y una ubicación local o un proveedor de hospedaje de Exchange. Como las conexiones de ExpressRoute no se realizan a través de una conexión a Internet, ofrecen más confiabilidad, más velocidad, menor latencia y mayor seguridad que las conexiones a Internet normales. Además, dado que se trata de una conexión directa del proveedor de telecomunicaciones del cliente, los datos no viajan a través de Internet y, por lo tanto, no están expuestos a ella.

Hay [disponibles](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) procedimientos recomendados para implementar una red híbrida segura que extienda una red local a Azure.

## <a name="disclaimer"></a>Renuncia de responsabilidades

- Este documento es meramente informativo. MICROSOFT NO EFECTÚA NINGÚN TIPO DE GARANTÍA, YA SEA EXPRESA, IMPLÍCITA O ESTATUTARIA, SOBRE LA INFORMACIÓN INCLUIDA EN EL PRESENTE DOCUMENTO. Este documento se proporciona "tal cual". La información y las opiniones expresadas en este documento, como las direcciones URL y otras referencias a sitios web de Internet, pueden cambiar sin previo aviso. Los clientes que lean este documento se atienen a las consecuencias de usarlo.
- Este documento no proporciona a los clientes ningún derecho legal sobre ninguna propiedad intelectual de ningún producto o solución de Microsoft.
- Los clientes pueden copiar y usar este documento con fines internos y de referencia.
- Algunas de las recomendaciones de este documento pueden provocar un aumento del uso de datos, de la red o de los recursos de procesos en Azure, lo que podría incrementar los costos de las licencias o las suscripciones de Azure.
- Esta arquitectura está diseñada para servir como base para que los clientes puedan ajustarse a sus requisitos específicos y no debe usarse tal cual en un entorno de producción.
- Este documento se ha desarrollado como referencia y no debe usarse para definir todos los medios por los que un cliente puede cumplir normas y requisitos de cumplimiento específicos. Los clientes deben buscar apoyo legal de su organización sobre las implementaciones de cliente aprobadas.
