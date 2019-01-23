---
title: 'Plano técnico de seguridad y cumplimiento de Azure: aplicación web de PaaS para FedRAMP'
description: 'Plano técnico de seguridad y cumplimiento de Azure: aplicación web de PaaS para FedRAMP'
services: security
author: jomolesk
ms.assetid: 41570fc1-4d74-48ed-afb0-ef1be857029e
ms.service: security
ms.topic: article
ms.date: 06/01/2018
ms.author: jomolesk
ms.openlocfilehash: 9bfd748e6b10106587e3fce64e15d7f1f6d50e59
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/14/2019
ms.locfileid: "54261594"
---
# <a name="azure-security-and-compliance-blueprint-paas-web-application-for-fedramp"></a>Plano técnico de seguridad y cumplimiento de Azure: Aplicación web PaaS para FedRAMP

## <a name="overview"></a>Información general

[Federal Risk and Authorization Management Program (FedRAMP)](https://www.fedramp.gov/) es un programa a nivel del gobierno de los Estados Unidos que proporciona un enfoque estandarizado para la evaluación de la seguridad, la concesión de autorizaciones, y la supervisión continua de servicios y productos en la nube. Este plano técnico de seguridad y cumplimiento de Azure proporciona una guía para la entrega de una arquitectura de plataforma como servicio (PaaS) de Microsoft Azure que ayuda a implementar un subconjunto de controles elevados de FedRAMP. Esta solución ofrece una guía de la implementación y configuración de los recursos de Azure para lograr una arquitectura de referencia común, muestra las distintas formas en que los clientes pueden cumplir los requisitos específicos de seguridad y cumplimiento, y sirve como base para que los clientes compilen y configuren sus propias soluciones en Azure.

Esta arquitectura de referencia, las guías de implementación de los controles asociados y los modelos de amenazas están diseñados como base para que los clientes puedan ajustarse a sus requisitos específicos y no debe usarse tal cual en un entorno de producción. Implementar una aplicación sin modificar en este entorno no es suficiente para satisfacer por completo los requisitos de la base de referencia de FedRAMP High. Tenga en cuenta lo siguiente:
- Esta arquitectura proporciona una base de referencia para ayudar a los clientes a implementar cargas de trabajo en Azure de manera compatible con FedRAMP.
- Los clientes tienen la responsabilidad de realizar las evaluaciones de seguridad y cumplimiento adecuadas de cualquier solución integrada mediante esta arquitectura, ya que los requisitos pueden variar en función de las características de implementación de cada cliente.

## <a name="architecture-diagram-and-components"></a>Componentes y diagrama de la arquitectura
Esta solución proporciona una arquitectura de referencia para una aplicación web de PaaS con un back-end de Azure SQL Database. La aplicación web está hospedada en una instancia de Azure App Service Environment aislada, que es un entorno privado y dedicado en un centro de datos de Azure. La carga del entorno equilibra el tráfico para la aplicación web en máquinas virtuales administradas por Azure. Esta arquitectura también incluye grupos de seguridad de red, una instancia de Application Gateway, Azure DNS y Load Balancer. Además, Azure Monitor proporciona análisis en tiempo real del mantenimiento del sistema. **Azure recomienda configurar una conexión VPN o de ExpressRoute para la administración y la importación de datos en la subred de la arquitectura de referencia.**

![Diagrama de arquitectura de referencia de aplicación web de PaaS para FedRAMP](images/fedramp-paaswa-architecture.png?raw=true "Diagrama de arquitectura de referencia de aplicación web de PaaS para FedRAMP")

Esta solución usa los siguientes servicios de Azure. Los detalles de la arquitectura de implementación se encuentran en la sección [Arquitectura de implementación](#deployment-architecture).

- Azure Active Directory
- Azure Key Vault
- Azure SQL Database
- Application Gateway
    - (1) Firewall de aplicaciones web
        - Modo de firewall: prevención
        - Conjunto de reglas: OWASP 3.0
        - Agente de escucha: puerto 443
- red virtual de Azure
- Grupos de seguridad de red
- Azure DNS
- Azure Storage
- Azure Monitor
- App Service Environment v2
- Azure Load Balancer
- Aplicación web de Azure
- Azure Resource Manager

## <a name="deployment-architecture"></a>Arquitectura de implementación
En la siguiente sección se detallan los elementos de desarrollo e implementación.

**Azure Resource Manager**: [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) permite a los clientes trabajar con los recursos de la solución como grupo. Los clientes pueden implementar, actualizar o eliminar todos los recursos de la solución en una sola operación coordinada. Para la implementación los clientes utilizan una plantilla, y esta plantilla puede funcionar en diferentes entornos, como pruebas, ensayo y producción. Resource Manager proporciona características de seguridad, auditoría y etiquetado que ayudan a los clientes a administrar los recursos después de la implementación

**App Service Environment v2**: [Azure App Service Environment (ASE)](https://docs.microsoft.com/azure/app-service/environment/intro) es una característica de App Service que proporciona un entorno completamente aislado y dedicado para ejecutar de forma segura las aplicaciones de App Service a gran escala.

Las instancias de ASE están aisladas para ejecutar únicamente las aplicaciones de un solo cliente y siempre se implementan en una red virtual. Los clientes tienen un mayor control sobre el tráfico de red entrante y saliente de las aplicaciones, y las aplicaciones pueden establecer conexiones seguras de alta velocidad a los recursos corporativos locales a través de redes virtuales.

Puede usar las instancias de ASE de esta arquitectura en los siguientes controles o configuraciones:

- Un host dentro de una instancia protegida de Azure Virtual Network y reglas de seguridad de red.
- Una instancia de ASE configurada con certificados ILB autofirmados para la comunicación HTTPS.
- [Modo de equilibrio de carga interno](https://docs.microsoft.com/azure/app-service-web/app-service-environment-with-internal-load-balancer)
- Deshabilitación de [TLS 1.0](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Al cambiar el [cifrado TLS](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings).
- Control de [puertos N/W de tráfico entrante](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic)
- [Firewall de aplicaciones web: restringir datos](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- Al permitir el [tráfico de Azure SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-network-architecture-overview)

La sección [Guía y recomendaciones](#guidance-and-recommendations) contiene más información acerca de ASE.

**Aplicación web de Azure**: [Azure App Service](https://docs.microsoft.com/azure/app-service/) permite a los clientes crear y hospedar aplicaciones web en el lenguaje de programación que prefieran sin tener que administrar la infraestructura. Ofrece escalado automático y alta disponibilidad, es compatible con Windows y Linux y permite implementaciones automatizadas desde GitHub, Azure DevOps o cualquier repositorio Git.

### <a name="virtual-network"></a>Virtual Network
La arquitectura define una red virtual privada con un espacio de direcciones de 10.200.0.0/16.

**Grupos de seguridad de red**: los [grupos de seguridad de red (NSG)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) contienen listas de control de acceso que permiten o deniegan el tráfico en una red virtual. Los NSG pueden usarse para proteger el tráfico en una subred o a nivel de máquina virtual individual. Los grupos de seguridad de red siguientes ya existen:
- Un grupo de seguridad de red para Application Gateway
- Un grupo de seguridad de red para App Service Environment
- Un grupo de seguridad de red para Azure SQL Database

Cada uno de los grupos de seguridad de red tiene puertos y protocolos específicos abiertos para que la solución pueda funcionar de forma segura y correcta. Además, las siguientes opciones de configuración están habilitadas para cada NSG:
  - Los [eventos y registros de diagnóstico](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) están habilitados y se almacenan en la cuenta de almacenamiento.
  - Log Analytics está conectado a los [diagnósticos del grupo de seguridad de red](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json).

**Subredes**: Cada subred se asocia a su NSG correspondiente.

**Azure DNS**: El sistema de nombres de dominio, o DNS, es responsable de traducir (o resolver) el nombre del sitio web o del servicio en su dirección IP. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) es un servicio de hospedaje para dominios DNS que ofrece resolución de nombres mediante la infraestructura de Azure. Al hospedar dominios en Azure, los usuarios pueden administrar los registros DNS con las mismas credenciales, API, herramientas y facturación que con los demás servicios de Azure. Azure DNS es compatible con dominios DNS privados.

**Azure Load Balancer**: [Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) permite a los clientes escalar las aplicaciones y crear alta disponibilidad para sus servicios. Load Balancer admite escenarios de entrada y de salida, proporciona baja latencia y alta capacidad de proceso, y puede escalar hasta millones de flujos para todas las aplicaciones TCP y UDP.

### <a name="data-in-transit"></a>Datos en tránsito
De manera predeterminada, Azure cifra todas las comunicaciones hacia y desde los centros de datos de Azure. Todas las transacciones a Azure Storage mediante Azure Portal se realizan mediante HTTPS.

### <a name="data-at-rest"></a>Datos en reposo
La arquitectura protege los datos en reposo mediante el cifrado, la auditoría de base de datos y otras medidas.

**Azure Storage**: para cumplir con los requisitos de los datos en reposo cifrados, [Azure Storage](https://azure.microsoft.com/services/storage/) usa [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption).

**Azure Disk Encryption**
[Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) aprovecha la función de BitLocker de Windows para proporcionar el cifrado del volumen de discos de datos. La solución se integra con Azure Key Vault para ayudar a controlar y administrar las claves de cifrado del disco.

**Azure SQL Database**: La instancia de Azure SQL Database usa las siguientes medidas de seguridad de base de datos:
-   La [autenticación y autorización de AD](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) permite la administración de identidades de usuarios de bases de datos y otros servicios de Microsoft en una ubicación central.
-   La [auditoría de bases de datos SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) realiza un seguimiento de eventos de bases de datos y los escribe en un registro de auditoría de una cuenta de almacenamiento de Azure.
-   Azure SQL Database está configurado para usar el [Cifrado de datos transparente (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), que realiza el cifrado y descifrado de archivos de datos y de registro en tiempo real de la base de datos, de las copias de seguridad asociadas y de archivos de registro de transacciones para proteger la información en reposo.
-   Las [reglas de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) impiden el acceso a los servidores de bases de datos hasta que se otorgan los permisos adecuados. Asimismo, otorgan acceso a las bases de datos según la dirección IP de origen de cada solicitud.
-   La [detección de amenazas de SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) permite detectar y responder ante posibles amenazas a medida que se producen, mediante el envío de alertas de seguridad para actividades sospechosas en la base de datos, posibles puntos vulnerables, ataques por inyección de código SQL y patrones anómalos de acceso a la base de datos.
-   Las [columnas de Always Encrypted](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) garantizan que los datos confidenciales nunca aparecen como texto no cifrado en el sistema de base de datos. Después de habilitar el cifrado de datos, solo las aplicaciones cliente o los servidores de aplicaciones con acceso a las claves pueden acceder a los datos de texto no cifrado.
- La [seguridad de nivel de fila](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) permite a los usuarios definir directivas para restringir el acceso a los datos a fin de interrumpir su procesamiento.
- El [enmascaramiento de los datos dinámicos de SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) puede realizarse después de la implementación de la arquitectura de referencia. Los clientes deberán ajustar la configuración del enmascaramiento de los datos dinámicos para ajustarse al esquema de la base de datos.

### <a name="identity-management"></a>Administración de identidades
Las siguientes tecnologías proporcionan funcionalidades de administración de identidad en el entorno de Azure:
-   [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) es el directorio multiinquilino basado en la nube y el servicio de administración de identidades de Microsoft. Todos los usuarios para esta solución se crean en AAD, incluidos los usuarios que acceden a la base de datos de Azure SQL Database.
-   La autenticación para acceder a la aplicación se realiza con AAD. Para obtener más información, consulte [Integración de aplicaciones con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Además, el cifrado de columnas de la base de datos usa Azure Active Directory para autenticar la aplicación en Azure SQL Database. Para más información, consulte cómo [proteger los datos confidenciales en Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   El [control de acceso basado en rol de Azure ](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) permite administrar al detalle el control de acceso de Azure. El acceso a la suscripción se limita al administrador de la suscripción y el acceso a los recursos se puede limitar según el rol de usuario.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) permite a los clientes minimizar el número de usuarios con acceso a determinada información.  Los administradores pueden usar AAD Privileged Identity Management para detectar, restringir y supervisar las identidades con privilegios y su acceso a los recursos. Esta funcionalidad también puede utilizarse para aplicar un acceso administrativo a petición, Just-in-Time, cuando sea necesario.
- [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) detecta posibles puntos vulnerables que afectan a las identidades de una organización, configura respuestas automatizadas si surgen acciones sospechosas relacionadas con esas identidades, investiga incidentes sospechosos y toma las medidas oportunas para resolverlos.

### <a name="security"></a>Seguridad
**Administración de secretos**: la solución utiliza [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) para la administración de claves y secretos. Azure Key Vault ayuda a proteger claves criptográficas y secretos usados por servicios y aplicaciones en la nube. Las siguientes funcionalidades de Azure Key Vault ayudan a los clientes a proteger los datos y el acceso a dichos datos:
- Se configuran directivas de acceso avanzadas según las necesidades.
- Se definen directivas de acceso a Key Vault con los permisos mínimos requeridos para las claves y los secretos.
- Todas las claves y los secretos en Key Vault tienen fechas de expiración.
- Todas las claves de Key Vault están protegidas por módulos de seguridad de hardware especializados (HSM). El tipo de clave es una clave RSA de 2048 bits protegida por HSM.
- A todos los usuarios y entidades se les otorgan los permisos mínimos necesarios mediante el control de acceso basado en rol.
- Los registros de diagnóstico de Key Vault están habilitados con un período de retención de al menos 365 días.
- Las operaciones criptográficas permitidas para las claves están restringidas únicamente a las requeridas.

**Application Gateway**: la arquitectura reduce el riesgo de aparición de puntos vulnerables en la seguridad, ya que usa Application Gateway con el firewall de aplicaciones web (WAF) y el conjunto de reglas OWASP está habilitado. Entre estas funcionalidades, cabe destacar:
- [SSL de un extremo a otro](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- Opción para habilitar la [descarga SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal)
- Opción para deshabilitar [TLS v1.0 y v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell)
- [Firewall de aplicaciones web](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview)
- [Modo de prevención](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) con el conjunto de reglas OWASP 3.0
- Opción para habilitar el [registro de diagnósticos](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics)
- [Sondeos de estado personalizados](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal)
- [Azure Security Center](https://azure.microsoft.com/services/security-center) y [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) que proporcionan protección adicional y notificaciones. Asimismo, Azure Security Center también proporciona un sistema de reputación.

### <a name="logging-and-auditing"></a>Registro y auditoría
Azure Monitor ofrece un registro completo de la actividad de usuario y del sistema, además de mantenimiento del sistema. Recopila y analiza los datos generados por los recursos en Azure y en los entornos locales.
- **Registros de actividad**: los [registros de actividad](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) proporcionan información detallada sobre las operaciones realizadas en los recursos de la suscripción. Los registros de actividad pueden ayudar a determinar el iniciador de una operación, el momento en que se produce y el estado.
- **Registros de diagnóstico**: los [registros de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) son todos los registros emitidos por todos los recursos. Estos registros incluyen registros del sistema de eventos de Windows, registros de Azure Storage, registros de auditoría de Key Vault, y registros de firewall y acceso a Application Gateway.
- **Archivado de registros**: Todos los registros de diagnóstico se escriben en una cuenta de almacenamiento de Azure centralizada y cifrada para su archivado. El usuario puede configurar la retención hasta 730 días para cumplir los requisitos de retención específicos de una organización. Esos registros se conectan a Azure Log Analytics para el procesamiento, el almacenamiento y la creación de informes de panel.

Además, como parte de esta arquitectura, se incluyen las siguientes soluciones de supervisión:
-   [Active Directory Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): la solución Active Directory Health Check evalúa el riesgo y el estado de los entornos de servidor a intervalos regulares y proporciona una lista prioritaria de recomendaciones específicas para la infraestructura de servidor implementada.
-   [Antimalware Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-malware): la solución de antimalware notifica sobre el estado de malware, las amenazas y la protección.
-   [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker): la solución Azure Automation almacena, ejecuta y administra runbooks. En esta solución, los runbooks ayudan a recopilar registros de Application Insights y Azure SQL Database.
-   [Security and Audit](https://docs.microsoft.com/azure/operations-management-suite/oms-security-getting-started): el panel Security and Audit proporciona una visión de alto nivel del estado de seguridad de los recursos al proporcionar información sobre métricas de los dominios de seguridad, problemas importantes, detecciones, inteligencia sobre amenazas y consultas comunes de seguridad.
-   [SQL Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): la solución SQL Health Check evalúa el riesgo y el estado de los entornos de servidor a intervalos regulares y proporciona a los clientes una lista prioritaria de recomendaciones específicas para la infraestructura de servidor implementada.
-   [Update Management](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-update-management): la solución Update Management permite la administración de cliente de las actualizaciones de seguridad del sistema operativo, incluido el estado de las actualizaciones disponibles y el proceso de instalación de las actualizaciones necesarias.
-   [Agent Health](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): la solución Agent Health notifica el número de agentes implementados y su distribución geográfica, así como el número de agentes que no responden y el de agentes que envían datos operativos.
-   [Registros de actividad de Azure](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): la solución Activity Log Analytics ayuda a los clientes a analizar los registros de actividad de todas las suscripciones de Azure para un cliente.
-   [Change Tracking](https://docs.microsoft.com/azure/automation/automation-change-tracking): la solución Change Tracking permite a los clientes identificar fácilmente los cambios en el entorno.

**Azure Monitor**
[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) ayuda a los usuarios a realizar un seguimiento del rendimiento, mantener la seguridad e identificar tendencias permitiendo a las organizaciones auditar, crear alertas y archivar datos, incluido el seguimiento de las llamadas a la API en los recursos Azure de los clientes.

## <a name="threat-model"></a>Modelo de amenazas

El diagrama de flujo de datos de esta arquitectura de referencia está disponible para su [descarga](https://aka.ms/fedrampPaaSWebAppDFD) y se encuentra a continuación. El modelo puede ayudar a los clientes a comprender los puntos de riesgo potencial de la infraestructura del sistema al realizar modificaciones.

![Modelo de amenazas de la aplicación web de PaaS para FedRAMP](images/fedramp-paaswa-threat-model.png?raw=true "Modelo de amenazas de la aplicación web de PaaS para FedRAMP")

## <a name="compliance-documentation"></a>Documentación de cumplimiento
En la [matriz de responsabilidades de los clientes de FedRAMP High de Azure Security and Compliance Blueprint](https://aka.ms/blueprinthighcrm) se enumeran todos los controles de seguridad necesarios para la línea de base de FedRAMP High. La matriz denota si la implementación de cada control es la responsabilidad de Microsoft, del cliente o de ambos.

En la [matriz de implementación de control elevado de aplicaciones web de PaaS para FedRAMP de Azure Security and Compliance Blueprint](https://aka.ms/fedrampPaaSWebAppCIM) se enumeran todos los controles de seguridad que necesita la línea de base de FedRAMP High. La matriz proporciona información acerca de los controles que cubre la arquitectura de aplicaciones web PaaS, lo que incluye descripciones detalladas de la forma en que la implementación cumple los requisitos de todos y cada uno de los controles cubiertos.

## <a name="guidance-and-recommendations"></a>Instrucciones y recomendaciones
### <a name="vpn-and-expressroute"></a>VPN y ExpressRoute
Debe configurarse un túnel VPN seguro o [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) para establecer una conexión segura a los recursos implementados como parte de esta arquitectura de referencia de aplicaciones web de PaaS. Al configurar una VPN o ExpressRoute adecuadamente, los clientes pueden agregar una capa de protección para los datos en tránsito.

Mediante la implementación de un túnel VPN seguro con Azure, se puede crear una conexión VPN entre una red local y una red virtual de Azure. Esta conexión tiene lugar a través de Internet y permite a los clientes colocar con seguridad la información en un "túnel" dentro de un vínculo cifrado entre la red del cliente y Azure. La VPN de sitio a sitio es una tecnología segura y madura que empresas de todos los tamaños han implementado durante décadas. Se utiliza [el modo de túnel IPsec](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) en esta opción como mecanismo de cifrado.

Dado que el tráfico dentro del túnel VPN atraviesa Internet con una VPN de sitio a sitio, Microsoft ofrece otra opción de conexión aún más segura. Azure ExpressRoute es un vínculo de WAN dedicado entre Azure y una ubicación local o un proveedor de hospedaje de Exchange. Como las conexiones de ExpressRoute no se realizan a través de una conexión a Internet, ofrecen más confiabilidad, más velocidad, menor latencia y mayor seguridad que las conexiones a Internet normales. Además, dado que se trata de una conexión directa del proveedor de telecomunicaciones del cliente, los datos no viajan a través de Internet y, por lo tanto, no están expuestos a ella.

Hay [disponibles](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) procedimientos recomendados para implementar una red híbrida segura que extienda una red local a Azure.

## <a name="disclaimer"></a>Renuncia de responsabilidades

 - Este documento es meramente informativo. MICROSOFT NO EFECTÚA NINGÚN TIPO DE GARANTÍA, YA SEA EXPRESA, IMPLÍCITA O ESTATUTARIA, SOBRE LA INFORMACIÓN INCLUIDA EN EL PRESENTE DOCUMENTO. Este documento se proporciona "tal cual". La información y las opiniones expresadas en este documento, como las direcciones URL y otras referencias a sitios web de Internet, pueden cambiar sin previo aviso. Los clientes que lean este documento se atienen a las consecuencias de usarlo.
 - Este documento no proporciona a los clientes ningún derecho legal sobre ninguna propiedad intelectual de ningún producto o solución de Microsoft.
 - Los clientes pueden copiar y usar este documento con fines internos y de referencia.
 - Algunas de las recomendaciones de este documento pueden provocar un aumento del uso de datos, de la red o de los recursos de procesos en Azure, lo que podría incrementar los costos de las licencias o las suscripciones de Azure.
 - Esta arquitectura está diseñada para servir como base para que los clientes puedan ajustarse a sus requisitos específicos y no debe usarse tal cual en un entorno de producción.
 - Este documento se ha desarrollado como referencia y no debe usarse para definir todos los medios por los que un cliente puede cumplir normas y requisitos de cumplimiento específicos. Los clientes deben buscar apoyo legal de su organización sobre las implementaciones de cliente aprobadas.
