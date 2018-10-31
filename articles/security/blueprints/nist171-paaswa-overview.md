---
title: 'Azure Security and Compliance Blueprint: aplicación web PaaS para NIST SP 800-171'
description: 'Azure Security and Compliance Blueprint: aplicación web PaaS para NIST SP 800-171'
services: security
author: jomolesk
ms.assetid: eea21a0a-5930-43e8-937f-5419c20744c9
ms.service: security
ms.topic: article
ms.date: 07/31/2018
ms.author: jomolesk
ms.openlocfilehash: e7851b39327e61f1676ae0cf1c3bff3de75b56bd
ms.sourcegitcommit: 07a09da0a6cda6bec823259561c601335041e2b9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/18/2018
ms.locfileid: "49409278"
---
# <a name="azure-security-and-compliance-blueprint---paas-web-application-for-nist-special-publication-800-171"></a>Azure Security and Compliance Blueprint: aplicación web PaaS para NIST Special Publication 800-171

## <a name="overview"></a>Información general
En [NIST Special Publication 800-171](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-171.pdf) se proporcionan instrucciones para proteger la información sin clasificar controlada (CUI) que reside en sistemas de información y organizaciones no federales. NIST SP 800-171 establece 14 familias de requisitos de seguridad para proteger la confidencialidad de CUI.

Este documento de Azure Security and Compliance Blueprint proporciona instrucciones para ayudar a los clientes a implementar una aplicación web de plataforma como servicio (PaaS) en Azure que implementa un subconjunto de controles de NIST SP 800-171. Esta solución muestra formas en que los clientes pueden cumplir requisitos de cumplimiento y de seguridad específicos. También sirve como base para que los clientes creen y configuren su propia aplicación web en Azure.

Esta arquitectura de referencia, la guía de implementación asociada y el modelo de amenazas están diseñados como base para que los clientes puedan adaptarse a sus requisitos específicos. No deben usarse tal cual en un entorno de producción. La implementación de esta arquitectura sin modificación no es suficiente para satisfacer completamente los requisitos de NIST SP 800-171. Los clientes son responsables de llevar a cabo las evaluaciones de seguridad y cumplimiento adecuadas de todas las soluciones creadas con esta arquitectura. Los requisitos pueden variar en función de las especificaciones de implementación de cada cliente.

## <a name="architecture-diagram-and-components"></a>Componentes y diagrama de la arquitectura

Este documento de Azure Security and Compliance Blueprint proporciona una arquitectura de referencia para una aplicación web PaaS con un back-end de Azure SQL Database. La aplicación web está hospedada en un entorno de App Service aislado, que es un entorno privado y dedicado en un centro de datos de Azure. La carga del entorno equilibra el tráfico para la aplicación web entre las máquinas virtuales administradas por Azure. Esta arquitectura también incluye grupos de seguridad de red, una puerta de enlace de aplicaciones de Azure, Azure DNS y Azure Load Balancer.

Para mejorar el análisis y los informes, las bases de datos de Azure SQL Database pueden configurarse con índices de almacén de columnas. Las bases de datos de Azure SQL se pueden escalar o reducir verticalmente, o incluso apagarse completamente en respuesta al uso del cliente. Todo el tráfico SQL se cifra con SSL mediante la inclusión de certificados autofirmados. Como procedimiento recomendado, Azure recomienda el uso de una entidad de certificación de confianza para mejorar la seguridad.

La solución utiliza cuentas de Azure Storage que los clientes pueden configurar para que usen Storage Service Encryption para preservar la confidencialidad de los datos en reposo. Azure almacena tres copias de los datos en un centro de datos seleccionado de un cliente para proporcionar resistencia. El almacenamiento con redundancia geográfica garantiza que los datos se repliquen en un centro de datos secundario situado a cientos de kilómetros de distancia y que se vuelvan a almacenar como tres copias dentro de ese centro de datos. Esta disposición evita que un evento adverso en el centro de datos principal del cliente acabe con una pérdida de datos.

Para mejorar la seguridad, todos los recursos de esta solución se administran como un grupo de recursos mediante Azure Resource Manager. El control de acceso basado en roles de Azure Active Directory (Azure AD) se usa para controlar el acceso a los recursos implementados. Estos recursos incluyen claves de cliente en Azure Key Vault. El mantenimiento del sistema se supervisa mediante Azure Monitor. Los clientes configuran este servicio de supervisión para capturar registros. El mantenimiento del sistema se muestra en un panel único fácil de usar.

SQL Database se suele gestionar con SQL Server Management Studio. Se ejecuta desde un equipo local configurado para acceder a la base de datos SQL a través de una VPN segura o de una conexión de Azure ExpressRoute.

Application Insights proporciona administración de rendimiento de aplicaciones en tiempo real, así como análisis mediante Azure Log Analytics. *Microsoft recomienda configurar una conexión VPN o de ExpressRoute para administrar e importar datos de en la subred de la arquitectura de referencia.*

![Aplicación web PaaS para NIST SP 800-171: diagrama de arquitectura de referencia](images/nist171-paaswa-architecture.png "Aplicación web PaaS para NIST SP 800-171: diagrama de arquitectura de referencia")

Esta solución usa los siguientes servicios de Azure. Para obtener más información, consulte la sección sobre la [arquitectura de implementación](#deployment-architecture).

- Azure Virtual Machines
    - (1) Administración/bastión (Windows Server 2016 Datacenter)
- Azure Virtual Network
    - (1)/16 redes
    - (4)/24 redes
    - (4) Grupos de seguridad de red
- Azure Application Gateway
    - Firewall de aplicaciones web
        - Modo de firewall: prevención
        - Conjunto de reglas: OWASP
        - Agente de escucha: puerto 443
- Application Insights
- Azure Active Directory
- App Service Environment v2
- Azure Automation
- Azure DNS
- Azure Key Vault
- Azure Load Balancer
- Azure Monitor
- Azure Resource Manager
- Azure Security Center
- Azure SQL Database
- Azure Storage
- Azure Log Analytics
- Azure Automation
- Azure Web Apps

## <a name="deployment-architecture"></a>Arquitectura de implementación
En la siguiente sección se detallan los elementos de desarrollo e implementación.

**Azure Resource Manager**: los clientes pueden usar [Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) para trabajar con los recursos de la solución como grupo. Los clientes pueden implementar, actualizar o eliminar todos los recursos de la solución en una sola operación coordinada. Los clientes usan una plantilla para la implementación. Las plantillas pueden funcionar en diferentes entornos, como de prueba, de almacenamiento provisional y de producción. Resource Manager proporciona características de seguridad, auditoría y etiquetado que ayudan a los clientes a administrar los recursos después de la implementación

**Host de tipo bastión**: el host de tipo bastión es el único punto de entrada que pueden emplear los usuarios para acceder a los recursos implementados en este entorno. El host de tipo bastión proporciona una conexión segura a los recursos implementados al permitir solo el tráfico remoto desde las direcciones IP públicas de una lista segura. Para permitir el tráfico de escritorio remoto, el origen del tráfico se debe definir en el grupo de seguridad de red.

Esta solución crea una máquina virtual como host de tipo bastión unido mediante dominio con las siguientes configuraciones:
-   [Extensión antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware).
-   [Extensión de Diagnósticos de Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template).
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) con Key Vault.
-   Una [directiva de apagado automático](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) para reducir el consumo de recursos de la máquina virtual cuando no esté en uso.
-   [Credential Guard de Windows Defender](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) está habilitado para que las credenciales y otros secretos se ejecuten en un entorno protegido aislado del sistema operativo en ejecución.

**Web Apps**: [Web Apps](https://docs.microsoft.com/azure/app-service/) es una característica de Azure App Service. Los clientes pueden usarla para crear y hospedar aplicaciones web en el lenguaje de programación que prefieran sin tener que administrar la infraestructura. Ofrece escalado automático y alta disponibilidad. Es compatible con Windows y Linux y permite efectuar implementaciones automatizadas desde GitHub, Azure DevOps o cualquier repositorio Git.

**App Service Environment**: [App Service Environment](https://docs.microsoft.com/azure/app-service/environment/intro) es una característica de App Service. Proporciona un entorno completamente aislado y dedicado para ejecutar de forma segura las aplicaciones de App Service a gran escala.

App Service Environment está aislado para ejecutar una sola aplicación. Siempre se implementa en una red virtual. Debido a la característica de aislamiento, la arquitectura de referencia tiene un aislamiento completo del inquilino y se quita del entorno multiinquilino de Azure. Los clientes tienen un mayor control sobre el tráfico de red entrante y saliente de la aplicación. Las aplicaciones pueden establecer conexiones seguras a alta velocidad por redes virtuales a los recursos corporativos locales. Los clientes pueden "escalar automáticamente" con App Service Environment en función de las métricas de carga, presupuesto disponible o de una programación definida.

El uso de App Service Environment para esta arquitectura ofrece los siguientes controles y configuraciones:

- Un host dentro de una red virtual de Azure protegida y reglas de seguridad de red.
- Certificado de equilibrador de carga interno autofirmado para la comunicación HTTPS. Como procedimiento recomendado, Microsoft recomienda el uso de una entidad de certificación de confianza para mejorar la seguridad.
- [Modo de equilibrio de carga interno](https://docs.microsoft.com/azure/app-service-web/app-service-environment-with-internal-load-balancer) (modo 3).
- Deshabilitación de [TLS 1.0](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings).
- Cambio del [cifrado TLS](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings).
- Control de [puertos N/W de tráfico entrante](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic).
- [Firewall de aplicaciones web: restricción de datos](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall).
- Permitir el [tráfico de Azure SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-network-architecture-overview).

### <a name="virtual-network"></a>Virtual network
La arquitectura define una red virtual privada con un espacio de direcciones de 10.200.0.0/16.

**Grupos de seguridad de red**: los [grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) contienen listas de control de acceso que permiten o deniegan el tráfico en una red virtual. Los NSG pueden usarse para proteger el tráfico en una subred o a nivel de máquina virtual individual. Los grupos de seguridad de red siguientes ya existen:
- Un grupo de seguridad de red para Application Gateway
- Un grupo de seguridad de red para App Service Environment
- Un grupo de seguridad de red para SQL Database
- Un grupo de seguridad de red para el host de tipo bastión

Cada uno de los grupos de seguridad de red tiene puertos y protocolos específicos abiertos para que la solución pueda funcionar de forma segura y correcta. Además, las siguientes opciones de configuración están habilitadas para cada NSG:
  - Los [eventos y registros de diagnóstico](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) están habilitados y se almacenan en la cuenta de almacenamiento.
  - Log Analytics está conectado a los [diagnósticos del grupo de seguridad de red](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json).

**Subredes**: asegúrese de que cada subred esté asociada a su grupo de seguridad de red correspondiente.

**Azure DNS**: el sistema de nombres de dominio (DNS) se encarga de traducir (o resolver) el nombre del sitio web o del servicio en su dirección IP. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) es un servicio de hospedaje para dominios DNS que ofrece resolución de nombres mediante la infraestructura de Azure. Al hospedar dominios en Azure, los usuarios pueden administrar los registros DNS con las mismas credenciales, API, herramientas y facturación que con los demás servicios de Azure. Azure DNS es compatible con dominios DNS privados.

**Azure Load Balancer**: los clientes pueden usar [Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) para escalar las aplicaciones y crear alta disponibilidad para sus servicios. Load Balancer admite escenarios de entrada y salida. Proporciona baja latencia y alto rendimiento y puede escalar hasta millones de flujos para todas las aplicaciones TCP y UDP.

### <a name="data-in-transit"></a>Datos en tránsito
De forma predeterminada, Azure cifra todas las comunicaciones hacia y desde los centros de datos de Azure. Todas las transacciones a Azure Storage mediante Azure Portal se realizan mediante HTTPS.

### <a name="data-at-rest"></a>Datos en reposo

La arquitectura protege los datos en reposo mediante el cifrado, la auditoría de base de datos y otras medidas.

**Azure Storage**: para cumplir los requisitos de los datos en reposo cifrados, todo el [almacenamiento](https://azure.microsoft.com/services/storage/) usa [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Esta característica ayuda a proteger y salvaguardar los datos en apoyo de los compromisos de seguridad y requisitos de cumplimiento de la organización definidos por la directiva NIST SP 800-171.

**Azure Disk Encryption**: [Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) usa la función de BitLocker de Windows para proporcionar el cifrado del volumen de discos de datos. La solución se integra con Key Vault para ayudar a controlar y administrar las claves de cifrado del disco.

**Azure SQL Database**: la instancia de SQL Database usa las siguientes medidas de seguridad de base de datos:
-   La [autenticación y autorización de Active Directory](https://docs.microsoft.com/azure/sql-database/sql-database-AAD-authentication) permiten la administración de identidades de usuarios de bases de datos y otros servicios de Microsoft en una ubicación central.
-   La [auditoría de bases de datos SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) realiza un seguimiento de eventos de bases de datos y los escribe en un registro de auditoría de una cuenta de almacenamiento de Azure.
-   SQL Database está configurado para usar el [cifrado de datos transparente](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql). Lleva a cabo un cifrado y descifrado en tiempo real de la base de datos, copias de seguridad asociadas y archivos de registro de transacciones para proteger la información en reposo. El cifrado de datos transparente garantiza que los datos almacenados no han estado sujetos a acceso no autorizado.
-   Las [reglas de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) impiden el acceso a los servidores de bases de datos hasta que se otorgan los permisos adecuados. Asimismo, otorgan acceso a las bases de datos según la dirección IP de origen de cada solicitud.
-   [Detección de amenazas de SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) permite detectar y responder a posibles amenazas cuando se producen. Proporciona alertas de seguridad si se producen actividades sospechosas en la base de datos, aparecen vulnerabilidades potenciales y ataques por inyección de código SQL, o si existen patrones anómalos de acceso a la base de datos.
-   Las [columnas cifradas](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) garantizan que los datos confidenciales nunca aparezcan como texto no cifrado en el sistema de base de datos. Una vez habilitado el cifrado de datos, solo las aplicaciones cliente o los servidores de aplicaciones con acceso a las claves pueden acceder a los datos de texto no cifrado.
- El [enmascaramiento dinámico de datos](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) limita la exposición de los datos confidenciales al enmascarar los datos para usuarios o aplicaciones sin privilegios. Puede detectar de forma automática datos potencialmente confidenciales y sugerir las máscaras adecuadas que se pueden aplicar. El enmascaramiento dinámico de datos permite reducir el acceso para que los datos confidenciales no salgan de la base de datos por un acceso no autorizado. *Los clientes son responsables de ajustar la configuración para cumplir con el esquema de la base de datos.*

### <a name="identity-management"></a>Administración de identidades
Las siguientes tecnologías proporcionan funcionalidades de administración del acceso a datos confidenciales en el entorno de Azure:
-   [Azure AD](https://azure.microsoft.com/services/active-directory/) es el servicio de directorio y de administración de identidades multiinquilino basado en la nube de Microsoft. Todos los usuarios de esta solución se crean en Azure AD, incluidos los que acceden a la base de datos SQL.
-   La autenticación para acceder a la aplicación se efectúa mediante Azure AD. Para obtener más información, consulte cómo se [integran las aplicaciones con Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). El cifrado de las columnas de la base de datos también usa Azure AD para autenticar la aplicación en SQL Database. Para más información, consulte cómo [proteger los datos confidenciales en SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   Los administradores pueden usar [Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) para definir permisos de acceso específicos. Con Azure RBAC, pueden conceder únicamente el grado de acceso que los usuarios necesiten para llevar a cabo sus tareas. En lugar de dar a cada usuario acceso ilimitado a los recursos de Azure, los administradores pueden permitir solo ciertas acciones para acceder a datos y recursos. El acceso a la suscripción está limitado al administrador de la suscripción.
- Los clientes pueden usar [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) para minimizar el número de usuarios con acceso a determinada información. Los administradores pueden usar Azure AD Privileged Identity Management para detectar, restringir y supervisar las identidades con privilegios y su acceso a los recursos. Esta funcionalidad también se puede utilizar para aplicar un acceso administrativo a petición, Just-in-Time, cuando sea necesario.
- [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) detecta posibles vulnerabilidades que afecten a las identidades de una organización. Configura respuestas automatizadas a acciones sospechosas detectadas que están relacionadas con las identidades de una organización. También investiga incidentes sospechosos para tomar las medidas adecuadas para resolverlos.

### <a name="security"></a>Seguridad
**Administración de secretos**: la solución usa [Key Vault](https://azure.microsoft.com/services/key-vault/) para administrar claves y secretos. Key Vault ayuda a proteger claves criptográficas y secretos usados por servicios y aplicaciones en la nube. Las siguientes funcionalidades de Key Vault ayudan a los clientes a proteger los datos:
- Se configuran directivas de acceso avanzadas según las necesidades.
- Se definen directivas de acceso a Key Vault con los permisos mínimos requeridos para las claves y los secretos.
- Todas las claves y los secretos en Key Vault tienen fechas de expiración.
- Todas las claves de Key Vault están protegidas por módulos de seguridad de hardware especializados. El tipo de clave es una clave RSA de 2048 bits protegida por un módulo de seguridad de hardware.
- Se otorgan los permisos necesarios mínimos a todos los usuarios e identidades mediante RBAC.
- Los registros de diagnóstico de Key Vault están habilitados con un período de retención de al menos 365 días.
- Las operaciones criptográficas permitidas para las claves están restringidas únicamente a las requeridas.

**Azure Security Center**: con [Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro), los clientes pueden aplicar y administrar de forma centralizada las directivas de seguridad entre cargas de trabajo, limitar la exposición a amenazas y detectar y responder a ataques. Security Center también accede a las configuraciones existentes de los servicios de Azure para proporcionar recomendaciones de configuración y servicio que ayuden a mejorar la postura de seguridad y a proteger los datos.

Security Center usa una variedad de funcionalidades de detección para alertar a los clientes de posibles ataques contra sus entornos. Estas alertas contienen información útil acerca de lo que desencadenó la alerta, los recursos objetivo y el origen del ataque. Security Center cuenta con un conjunto de [alertas de seguridad predefinidas](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) que se desencadenan cuando tiene lugar una amenaza o actividad sospechosa. Los clientes pueden utilizar [reglas de alertas personalizadas](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) para definir nuevas alertas de seguridad basadas en los datos ya recopilados en el entorno.

Security Center proporciona incidentes y alertas de seguridad prioritarias. Security Center facilita a los clientes la detección y la resolución de posibles problemas de seguridad. Para cada amenaza detectada se genera un [informe de inteligencia de amenazas](https://docs.microsoft.com/azure/security-center/security-center-threat-report). Los equipos de respuesta a los incidentes pueden usar los informes para investigar y solucionar las amenazas.

**Azure Application Gateway**: la arquitectura reduce el riesgo de aparición de puntos vulnerables en la seguridad mediante una puerta de enlace de aplicaciones con un firewall de aplicaciones web configurado y el conjunto de reglas OWASP habilitado. Entre estas funcionalidades, cabe destacar:

- [SSL de un extremo a otro](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).
- Opción para habilitar la [descarga SSL](https://docs.microsoft.com/azure/application-gateway/application-gateway-ssl-portal).
- Opción para deshabilitar [TLS v1.0 y v1.1](https://docs.microsoft.com/azure/application-gateway/application-gateway-end-to-end-ssl-powershell).
- [Firewall de aplicaciones web](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-overview) (modo de prevención).
- [Modo de prevención](https://docs.microsoft.com/azure/application-gateway/application-gateway-web-application-firewall-portal) con el conjunto de reglas OWASP 3.0.
- Opción para habilitar el [registro de diagnósticos](https://docs.microsoft.com/azure/application-gateway/application-gateway-diagnostics).
- [Sondeos de estado personalizados](https://docs.microsoft.com/azure/application-gateway/application-gateway-create-gateway-portal).
- [Security Center](https://azure.microsoft.com/services/security-center) y [Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-security-recommendations) proporcionan protección adicional y notificaciones. Security Center también proporciona un sistema de reputación.

### <a name="logging-and-auditing"></a>Registro y auditoría

Los servicios de Azure proporcionan un registro completo de la actividad de usuario y del sistema, así como de mantenimiento del sistema:
- **Registros de actividad:** [los registros de actividad](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) proporcionan información detallada sobre las operaciones realizadas en los recursos de la suscripción. Los registros de actividad pueden ayudar a determinar el iniciador de una operación, el momento en que se produce y el estado.
- **Registros de diagnóstico:** [los registros de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) incluyen todos los registros emitidos por todos los recursos. Estos registros incluyen registros del sistema de eventos de Windows, registros de Azure Storage, registros de auditoría de Key Vault y registros de firewall y acceso a Application Gateway. Todos los registros de diagnóstico se escriben en una cuenta de almacenamiento de Azure centralizada y cifrada para su archivado. Los usuarios pueden configurar el período de retención (de hasta 730 días) para satisfacer sus requisitos específicos.

**Azure Log Analytics**: los registros se consolidan en [Log Analytics](https://azure.microsoft.com/services/log-analytics/) para el procesamiento, el almacenamiento y la creación de informes de panel. Una vez recopilados, los datos se organizan en tablas independientes por tipo de datos en las áreas de trabajo de Log Analytics. De este modo, todos los datos se pueden analizar juntos, independientemente de su fuente original. Security Center se integra con Log Analytics. Los clientes pueden usar las consultas de Log Analytics para acceder a sus datos de eventos de seguridad y combinarlos con los datos de otros servicios.

Las siguientes [soluciones de administración](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) de Log Analytics se incluyen como parte de esta arquitectura:
-   [Active Directory Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): la solución Active Directory Health Check evalúa el riesgo y el estado de los entornos del servidor de forma periódica. Proporciona una lista priorizada de recomendaciones específicas para la infraestructura de servidor implementada.
- [SQL Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): la solución SQL Health Check evalúa el riesgo y el estado de los entornos de servidor de forma periódica. Proporciona a los clientes una lista priorizada de recomendaciones específicas de la infraestructura de servidor implementada.
- [Agent Health](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): la solución Agent Health informa de cuántos agentes se implementan y su distribución geográfica. También informa de cuántos agentes no responden y del número de agentes que envían datos operativos.
-   [Activity Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): la solución Activity Log Analytics ayuda a los clientes a analizar los registros de actividad de Azure de todas las suscripciones de Azure para un cliente.

**Azure Automation**: [Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) almacena, ejecuta y administra runbooks. En esta solución, los runbooks ayudan a recopilar registros de SQL Database. Los clientes pueden usar la solución [Change Tracking](https://docs.microsoft.com/azure/automation/automation-change-tracking) de Automation para identificar fácilmente los cambios en el entorno.

**Azure Monitor**: [Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) ayuda a los usuarios a efectuar un seguimiento del rendimiento, a mantener la seguridad y a identificar tendencias. Las organizaciones lo pueden usar para auditar, crear alertas y archivar datos. También pueden registrar llamadas API en sus recursos de Azure.

**Application Insights**: [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) es un servicio de Application Performance Management extensible para desarrolladores web disponible en varias plataformas. Application Insights detecta anomalías en el rendimiento. Los clientes lo pueden usar para supervisar la aplicación web en directo. Application Insights incluye herramientas de análisis eficaces que ayudan a los clientes a diagnosticar problemas y comprender lo que hacen los usuarios con la aplicación. Está diseñado para ayudar a los clientes a mejorar continuamente el rendimiento y la facilidad de uso.

## <a name="threat-model"></a>Modelo de amenazas

El diagrama de flujo de datos de esta arquitectura de referencia está disponible para su [descarga](https://aka.ms/nist171-paaswa-tm) y se encuentra aquí. El modelo puede ayudar a los clientes a comprender los puntos de riesgo potencial de la infraestructura del sistema al efectuar modificaciones.

![Aplicación web PaaS para NIST SP 800-171: modelo de amenazas](images/nist171-paaswa-threat-model.png "Aplicación web PaaS para NIST SP 800-171: modelo de amenazas")

## <a name="compliance-documentation"></a>Documentación de cumplimiento
En [Azure Security and Compliance Blueprint: matriz de responsabilidades del cliente para NIST SP 800-171](https://aka.ms/nist171-crm) muestra todos los controles de seguridad que exige NIST SP 800-171. En esta matriz se detalla si la implementación de cada objetivo es responsabilidad de Microsoft, del cliente o de ambos.

En el [plano técnico de seguridad y cumplimiento de Azure: matriz de implementación de control de aplicaciones web PaaS para NIST SP 800-171](https://aka.ms/nist171-paaswa-cim) se ofrece información sobre qué controles de NIST SP 800-171 se abordan en la arquitectura de aplicaciones web PaaS. Incluye descripciones detalladas de cómo la implementación cumple los requisitos de cada control cubierto.

## <a name="guidance-and-recommendations"></a>Instrucciones y recomendaciones
### <a name="vpn-and-expressroute"></a>VPN y ExpressRoute
Se debe configurar un túnel VPN seguro o [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) para establecer una conexión segura a los recursos implementados como parte de esta arquitectura de referencia de aplicaciones web de PaaS. Al configurar una VPN o ExpressRoute adecuadamente, los clientes pueden agregar una capa de protección para los datos en tránsito.

Mediante la implementación de un túnel VPN seguro con Azure, se puede crear una conexión privada virtual entre una red local y una red virtual de Azure. Esta conexión tiene lugar a través de Internet y permite a los clientes colocar con seguridad la información en un "túnel" dentro de un vínculo cifrado entre la red del cliente y Azure. La VPN de sitio a sitio es una tecnología segura y madura que empresas de todos los tamaños han implementado durante décadas. Se utiliza [el modo de túnel IPsec](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) en esta opción como mecanismo de cifrado.

Dado que el tráfico dentro del túnel VPN atraviesa Internet con una VPN de sitio a sitio, Microsoft ofrece otra opción de conexión aún más segura. ExpressRoute es un vínculo de WAN dedicado entre Azure y una ubicación local o un proveedor de hospedaje de Exchange. Las conexiones de ExpressRoute se conectan directamente al proveedor de telecomunicaciones de un cliente. Como resultado, los datos no viajan a través de Internet y no están expuestos. Estas conexiones ofrecen más confiabilidad, velocidades más altas, menores latencias y mayor seguridad que las conexiones habituales.

Hay [disponibles](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) procedimientos recomendados para implementar una red híbrida segura que extienda una red local a Azure.

## <a name="disclaimer"></a>Renuncia de responsabilidades

 - Este documento es meramente informativo. MICROSOFT NO EFECTÚA NINGÚN TIPO DE GARANTÍA, YA SEA EXPRESA, IMPLÍCITA O ESTATUTARIA, SOBRE LA INFORMACIÓN INCLUIDA EN EL PRESENTE DOCUMENTO. Este documento se proporciona "tal cual". La información y las opiniones expresadas en este documento, como las direcciones URL y otras referencias a sitios web de Internet, pueden cambiar sin previo aviso. Los clientes que lean este documento se atienen a las consecuencias de usarlo.
 - Este documento no proporciona a los clientes ningún derecho legal sobre ninguna propiedad intelectual de ningún producto o solución de Microsoft.
 - Los clientes pueden copiar y usar este documento con fines internos y de referencia.
 - Algunas de las recomendaciones de este documento pueden provocar un aumento del uso de datos, de la red o de los recursos de procesos en Azure, lo que podría incrementar los costos de las licencias o las suscripciones de Azure.
 - Esta arquitectura está diseñada para servir como base para que los clientes puedan ajustarse a sus requisitos específicos y no debe usarse tal cual en un entorno de producción.
 - Este documento se ha desarrollado como referencia y no debe usarse para definir todos los medios por los que un cliente puede cumplir normas y requisitos de cumplimiento específicos. Los clientes deben buscar apoyo legal de su organización sobre las implementaciones de cliente aprobadas.
