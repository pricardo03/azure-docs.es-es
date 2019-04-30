---
title: 'Azure Security and Compliance Blueprint: aplicación web PaaS para FFIEC'
description: 'Azure Security and Compliance Blueprint: aplicación web PaaS para FFIEC'
services: security
author: meladie
ms.assetid: a552e313-2b46-4001-b6e2-bed0b7757ac5
ms.service: security
ms.topic: article
ms.date: 06/20/2018
ms.author: meladie
ms.openlocfilehash: 63ad692e1050f900310f8195b79f26dd99704b93
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60609642"
---
# <a name="azure-security-and-compliance-blueprint-paas-web-application-for-ffiec-financial-services"></a>Plano técnico de seguridad y cumplimiento de Azure: Aplicación web PaaS para servicios financieros de FFIEC

## <a name="overview"></a>Información general

Este plano técnico Azure Security and Compliance Blueprint proporciona orientación para la implementación de un entorno de plataforma como servicio (PaaS) adecuada para la recopilación, el almacenamiento y la recuperación de datos financieros regulados por el Consejo Federal de Inspección de Instituciones Financieras (FFIEC). Esta solución automatiza la implementación y la configuración de recursos de Azure para una arquitectura de referencia común, la cual muestra las formas en que los clientes pueden cumplir los requisitos específicos de seguridad y cumplimiento y sirve como base para que los clientes creen y configuren sus propias soluciones en Azure. La solución implementa un subconjunto de requisitos de los manuales de FFIEC. Para más información sobre los requisitos de FFIEC y esta solución, consulte la [documentación de cumplimiento](#compliance-documentation).

Esta solución de automatización de Azure Security and Compliance Blueprint implementa una arquitectura de referencia de aplicación web PaaS con controles de seguridad configurados previamente para ayudar a los clientes a cumplir con los requisitos de FFIEC. La solución consta de plantillas de Azure Resource Manager y scripts de PowerShell que guían la configuración y la implementación de recursos.

Esta arquitectura está diseñada para servir como base para que los clientes puedan ajustarse a sus requisitos específicos y no debe usarse tal cual en un entorno de producción. Implementar una aplicación sin modificar en este entorno no es suficiente para satisfacer por completo los requisitos de los objetivos de FFIEC. Tenga en cuenta lo siguiente:
- Esta arquitectura proporciona una base de referencia para que los clientes puedan usar Azure de forma que sea compatible con FFIEC.
- Los clientes tienen la responsabilidad de realizar las evaluaciones de seguridad y cumplimiento adecuadas de cualquier solución integrada mediante esta arquitectura, ya que los requisitos pueden variar en función de las características de implementación de cada cliente.

Lograr el cumplimiento de FFIEC requiere que unos auditores cualificados certifiquen una solución de cliente de producción. Las auditorías son supervisadas por inspectores de agencias miembros del FFIEC, incluido el Consejo de Gobernadores del Sistema de Reserva Federal (FRB), la Corporación Federal de Seguros de Depósitos (FDIC), la Administración de la Unión de Crédito Nacional (NCUA), la Oficina del Controlador de la Moneda (OCC) y la Oficina de Protección Financiera del Consumidor (CFPB). Estos investigadores certifican que las auditorías están realizadas por evaluadores que mantienen su independencia de la institución auditada. Los clientes tienen la responsabilidad de realizar las evaluaciones de seguridad y cumplimiento adecuadas de cualquier solución compilada con esta arquitectura, ya que los requisitos pueden variar en función de las características de implementación de cada cliente.

Haga clic [aquí](https://aka.ms/ffiec-paaswa-repo) para ver las instrucciones de implementación.

## <a name="architecture-diagram-and-components"></a>Componentes y diagrama de la arquitectura

Esta solución de automatización de Azure Security and Compliance Blueprint implementa una arquitectura de referencia para una aplicación web PaaS con un back-end de Azure SQL Database. La aplicación web está hospedada en una instancia de Azure App Service Environment aislada, que es un entorno privado y dedicado en un centro de datos de Azure. La carga del entorno equilibra el tráfico para la aplicación web entre las máquinas virtuales administradas por Azure. Esta arquitectura también incluye grupos de seguridad de red, una instancia de Application Gateway, Azure DNS y Load Balancer.

Para mejorar el análisis y los informes, las bases de datos de Azure SQL Database pueden configurarse con índices de almacén de columnas. Las bases de datos de Azure SQL se pueden escalar o reducir verticalmente, o incluso apagarse completamente en respuesta al uso del cliente. Todo el tráfico SQL se cifra con SSL mediante la inclusión de certificados autofirmados. Como procedimiento recomendado, Azure recomienda el uso de una entidad de certificación de confianza para mejorar la seguridad.

La solución utiliza cuentas de Azure Storage que los clientes pueden configurar para que usen Storage Service Encryption para preservar la confidencialidad de los datos en reposo. Azure almacena tres copias de los datos en un centro de datos seleccionado de un cliente para proporcionar resistencia. El almacenamiento con redundancia geográfica garantiza que los datos se replicarán en un centro de datos secundario a cientos de kilómetros de distancia y que, de nuevo, se guardarán tres copias en ese centro de datos, lo que impide que un evento adverso que suceda en el centro de datos principal del cliente pueda resultar en una pérdida de datos.

Para mejorar la seguridad, todos los recursos de esta solución se administran como un grupo de recursos mediante Azure Resource Manager. El control de acceso basado en rol de Azure Active Directory se utiliza para controlar el acceso a los recursos implementados, incluidas las claves en Azure Key Vault. El mantenimiento del sistema se supervisa mediante Azure Monitor. Los clientes configuran ambos servicios de monitorización para capturar registros y mostrar el estado del sistema en un único panel de fácil navegación.

Azure SQL Database se administra comúnmente mediante SQL Server Management Studio, que se ejecuta desde una máquina local configurada para acceder a Azure SQL Database mediante una conexión segura VPN o ExpressRoute.

Además, Application Insights proporciona administración de rendimiento de aplicaciones en tiempo real y análisis a través de los registros de Azure Monitor. **Microsoft recomienda configurar una conexión VPN o de ExpressRoute para la administración y la importación de datos en la subred de la arquitectura de referencia.**

![Diagrama de arquitectura de referencia de aplicación web PaaS para FFIEC](images/ffiec-paaswa-architecture.png "PaaS Web Application for FFIEC reference architecture diagram")

Esta solución usa los siguientes servicios de Azure. Los detalles de la arquitectura de implementación se encuentran en la sección [Arquitectura de implementación](#deployment-architecture).

- Application Gateway
    - Firewall de aplicaciones web
        - Modo de firewall: prevención
        - Conjunto de reglas: OWASP
        - Puente de agente de escucha: 443
- Application Insights
- Azure Active Directory
- Azure Application Service Environment v2
- Azure Automation
- Azure DNS
- Azure Key Vault
- Azure Load Balancer
- Monitor de Azure (registros)
- Azure Resource Manager
- Azure Security Center
- Azure SQL Database
- Azure Storage
- Azure Virtual Network
    - (1) Red /16
    - (4) /24 redes
    - Grupos de seguridad de red
- Azure App Service

## <a name="deployment-architecture"></a>Arquitectura de implementación

En la siguiente sección se detallan los elementos de desarrollo e implementación.

**Azure Resource Manager**: [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) permite a los clientes trabajar con los recursos de la solución como grupo. Los clientes pueden implementar, actualizar o eliminar todos los recursos de la solución en una sola operación coordinada. Para la implementación los clientes utilizan una plantilla, y esta plantilla puede funcionar en diferentes entornos, como pruebas, ensayo y producción. Resource Manager proporciona características de seguridad, auditoría y etiquetado que ayudan a los clientes a administrar los recursos después de la implementación

**Host de tipo bastión**: el host de tipo bastión es el único punto de entrada que permite a los usuarios acceder a los recursos implementados en este entorno. El host de tipo bastión proporciona una conexión segura a los recursos implementados al permitir solo el tráfico remoto desde las direcciones IP públicas de una lista segura. Para permitir el tráfico de escritorio remoto (RDP), el origen del tráfico debe definirse en el grupo de seguridad de red.

Esta solución crea una máquina virtual como host de tipo bastión unido mediante dominio con las siguientes configuraciones:
-   [Extensión antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Extensión de Diagnósticos de Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) con Azure Key Vault
-   Una [directiva de apagado automático](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) para reducir el consumo de recursos de la máquina virtual cuando no esté en uso.
-   [Credential Guard de Windows Defender](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) habilitado para que las credenciales y otros secretos se ejecuten en un entorno protegido aislado del sistema operativo en ejecución

**App Service Environment v2**: Azure App Service Environment es una característica de Azure App Service que proporciona un entorno completamente aislado y dedicado para ejecutar de forma segura las aplicaciones de App Service a gran escala. Esta característica de aislamiento es necesaria para cumplir con los requisitos de cumplimiento de FFIEC.

Los entornos de App Service Environment están aislados para ejecutar únicamente las aplicaciones de un solo cliente, y siempre se implementan en una red virtual. Esta característica de aislamiento permite a la arquitectura de referencia tener un aislamiento de inquilino completo, quitarlo del entorno multiinquilino de Azure que prohíbe a los que son multiinquilino enumerar los recursos del entorno de App Service implementados. Los clientes tienen un mayor control sobre el tráfico de red entrante y saliente de las aplicaciones, y las aplicaciones pueden establecer conexiones seguras de alta velocidad a los recursos corporativos locales a través de redes virtuales. Los clientes pueden "escalar automáticamente" con App Service Environment en función de las métricas de carga, presupuesto disponible o de una programación definida.

El uso de App Service Environment para esta arquitectura permite las siguientes configuraciones:

- Un host dentro de una red virtual de Azure protegida y reglas de seguridad de red.
- Certificado de equilibrador de carga interno autofirmado para la comunicación HTTPS. Como procedimiento recomendado, Microsoft recomienda el uso de una entidad de certificación de confianza para mejorar la seguridad.
- [Modo de equilibrio de carga interno](https://docs.microsoft.com/azure/app-service-web/app-service-environment-with-internal-load-balancer)
- Deshabilitación de [TLS 1.0](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Cambio del [cifrado TLS](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Control de [puertos N/W de tráfico entrante](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic)
- [Firewall de aplicaciones web: restringir datos](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- Al permitir el [tráfico de Azure SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-network-architecture-overview)

**Azure App Service**: [Azure App Service](https://docs.microsoft.com/azure/app-service/) permite a los clientes crear y hospedar aplicaciones web en el lenguaje de programación que prefieran sin tener que administrar la infraestructura. Ofrece escalado automático y alta disponibilidad, es compatible con Windows y Linux y permite implementaciones automatizadas desde GitHub, Azure DevOps o cualquier repositorio Git.

### <a name="virtual-network"></a>Virtual Network

La arquitectura define una red virtual privada con un espacio de direcciones de 10.200.0.0/16.

**Grupos de seguridad de red**: los [grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) contienen listas de control de acceso que permiten o deniegan el tráfico en una red virtual. Los grupos de seguridad de red pueden usarse para proteger el tráfico en una subred o un nivel de máquina virtual individual. Existen los siguientes grupos de seguridad de red:

- 1 grupo de seguridad de red para Application Gateway
- 1 grupo de seguridad de red para App Service Environment
- 1 grupo de seguridad de red para Azure SQL Database
- 1 grupo de seguridad de red para host de tipo bastión

Cada uno de los grupos de seguridad de red tiene puertos y protocolos específicos abiertos para que la solución pueda funcionar de forma segura y correcta. Además, las siguientes opciones de configuración están habilitadas para cada grupo de seguridad de red:

- Los [eventos y registros de diagnóstico](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) están habilitados y se almacenan en la cuenta de almacenamiento.
- Registros de Azure Monitor se conecta a la [grupo de seguridad de red&#39;s registros de diagnóstico](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Subredes**: cada subred está asociada a su grupo de seguridad de red correspondiente.

**Azure DNS**: El sistema de nombres de dominio, o DNS, es responsable de traducir (o resolver) el nombre del sitio web o del servicio en su dirección IP. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) es un servicio de hospedaje para dominios DNS que ofrece resolución de nombres mediante la infraestructura de Azure. Al hospedar dominios en Azure, los usuarios pueden administrar los registros DNS con las mismas credenciales, API, herramientas y facturación que con los demás servicios de Azure. Azure DNS es compatible con dominios DNS privados.

**Azure Load Balancer**: [Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) permite a los clientes escalar las aplicaciones y crear alta disponibilidad para sus servicios. Load Balancer admite escenarios de entrada y de salida, proporciona baja latencia y alta capacidad de proceso, y puede escalar hasta millones de flujos para todas las aplicaciones TCP y UDP.

### <a name="data-in-transit"></a>Datos en tránsito

De manera predeterminada, Azure cifra todas las comunicaciones hacia y desde los centros de datos de Azure. Todas las transacciones a Azure Storage mediante Azure Portal se realizan mediante HTTPS.

### <a name="data-at-rest"></a>Datos en reposo

La arquitectura protege los datos en reposo mediante el cifrado, la auditoría de base de datos y otras medidas.

**Azure Storage**: para cumplir con los requisitos de los datos en reposo cifrados, [Azure Storage](https://azure.microsoft.com/services/storage/) usa [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Esto ayuda a proteger los datos en cumplimiento de los compromisos de seguridad de la organización y los requisitos de cumplimiento definidos por FFIEC.

**Azure Disk Encryption**: [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) aprovecha la función de BitLocker de Windows para proporcionar el cifrado del volumen de discos de datos. La solución se integra con Azure Key Vault para ayudar a controlar y administrar las claves de cifrado del disco.

**Azure SQL Database**: La instancia de Azure SQL Database usa las siguientes medidas de seguridad de base de datos:

- La [autenticación y autorización de Active Directory](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) permiten la administración de identidades de usuarios de bases de datos y otros servicios de Microsoft en una ubicación central.
- La [auditoría de bases de datos SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) realiza un seguimiento de eventos de bases de datos y los escribe en un registro de auditoría de una cuenta de almacenamiento de Azure.
- Azure SQL Database está configurado para usar el [cifrado de datos transparente](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), que realiza el cifrado y descifrado en tiempo real de la base de datos, las copias de seguridad asociadas y los archivos de registro de transacciones, para proteger la información en reposo. El cifrado de datos transparente garantiza que los datos almacenados no hayan estado sujetos a un acceso no autorizado.
- Las [reglas de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) impiden el acceso a los servidores de bases de datos hasta que se otorgan los permisos adecuados. Asimismo, otorgan acceso a las bases de datos según la dirección IP de origen de cada solicitud.
- La [detección de amenazas de SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) permite detectar y responder ante posibles amenazas a medida que se producen, mediante el envío de alertas de seguridad para actividades sospechosas en la base de datos, posibles puntos vulnerables, ataques por inyección de código SQL y patrones anómalos de acceso a la base de datos.
- Las [columnas cifradas](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) garantizan que los datos confidenciales nunca aparezcan como texto no cifrado en el sistema de base de datos. Después de habilitar el cifrado de datos, solo las aplicaciones cliente o los servidores de aplicaciones con acceso a las claves pueden acceder a los datos de texto no cifrado.
- El [enmascaramiento dinámico de datos de SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) limita la exposición de datos confidenciales al enmascarar los datos para usuarios o aplicaciones sin privilegios. El enmascaramiento dinámico de datos puede detectar de forma automática datos potencialmente confidenciales y sugerir las máscaras adecuadas que se pueden aplicar. Esto ayuda a identificar y reducir el acceso a datos de modo que no salgan de la base de datos mediante un acceso no autorizado. Los clientes son responsables de ajustar la configuración del enmascaramiento dinámico de datos para cumplir con el esquema de la base de datos.

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

**Application Insights**: [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) es un servicio de Application Performance Management extensible para desarrolladores web en varias plataformas. Application Insights detecta anomalías de rendimiento y los clientes pueden utilizarlo para supervisar la aplicación web activa. Incluye herramientas de análisis eficaces que ayudan a los clientes a diagnosticar problemas y comprender lo que hacen realmente los usuarios con la aplicación. Está diseñado para ayudar a los clientes a mejorar continuamente el rendimiento y la facilidad de uso.

## <a name="threat-model"></a>Modelo de amenazas

El diagrama de flujo de datos de esta arquitectura de referencia está disponible para su [descarga](https://aka.ms/ffiec-paaswa-tm) y se encuentra a continuación. El modelo puede ayudar a los clientes a comprender los puntos de riesgo potencial de la infraestructura del sistema al realizar modificaciones.

![Modelo de amenazas de la aplicación web PaaS para FFIEC](images/ffiec-paaswa-threat-model.png "PaaS Web Application for FFIEC threat model")

## <a name="compliance-documentation"></a>Documentación de cumplimiento

En [Azure Security and Compliance Blueprint: matriz de responsabilidades de clientes para FFIEC](https://aka.ms/ffiec-crm) se enumeran todos los objetivos de seguridad necesarios para FFIEC. En esta matriz se detalla si la implementación de cada objetivo es responsabilidad de Microsoft, el cliente o de ambos.

En [Azure Security and Compliance Blueprint: matriz de implementación de aplicaciones web PaaS para FFIEC](https://aka.ms/ffiec-paaswa-cim) se ofrece información sobre qué requisitos de FFIEC aborda la arquitectura de aplicaciones web PaaS, incluida una descripción detallada de cómo la implementación cumple los requisitos de cada objetivo cubierto.

## <a name="deploy-this-solution"></a>Implementación de la solución
Esta instancia de Azure Security and Compliance Blueprint Automation se compone de archivos de configuración de JSON y scripts de PowerShell que se controlan mediante el servicio de API de Azure Resource Manager para implementar recursos en Azure. Puede encontrar instrucciones detalladas acerca de la implementación [aquí](https://aka.ms/ffiec-paaswa-repo).

#### <a name="quickstart"></a>Guía de inicio rápido
1. Clone o descargue [este](https://aka.ms/ffiec-paaswa-repo) repositorio de GitHub en su estación de trabajo local.

2. Revise 0-Setup-AdministrativeAccountAndPermission.md y ejecute los comandos proporcionados.

3. Implemente una solución de prueba con datos de ejemplo Contoso o pruebe un entorno de producción inicial.
    - 1a-ContosoWebStoreDemoAzureResources.ps1
        - Este script implementa recursos de Azure para una demostración de una tienda web mediante datos de ejemplo de Contoso.
    - 1-DeployAndConfigureAzureResources.ps1
        - Este script implementa los recursos de Azure necesarios para admitir un entorno de producción para una aplicación web propiedad del cliente. El cliente debe personalizar aún más este entorno en función de los requisitos de la organización.

## <a name="guidance-and-recommendations"></a>Instrucciones y recomendaciones

### <a name="vpn-and-expressroute"></a>VPN y ExpressRoute

Debe configurarse un túnel VPN seguro o [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) para establecer una conexión segura a los recursos implementados como parte de esta arquitectura de referencia de aplicaciones web de PaaS. Al configurar una VPN o ExpressRoute adecuadamente, los clientes pueden agregar una capa de protección para los datos en tránsito.

Mediante la implementación de un túnel VPN seguro con Azure, se puede crear una conexión privada virtual entre una red local y una red virtual de Azure. Esta conexión tiene lugar a través de Internet y permite a los clientes colocar con seguridad la información en un &quot;túnel&quot; dentro de un vínculo cifrado entre la red del cliente y Azure. La VPN de sitio a sitio es una tecnología segura y madura que empresas de todos los tamaños han implementado durante décadas. Se utiliza [el modo de túnel IPsec](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) en esta opción como mecanismo de cifrado.

Dado que el tráfico dentro del túnel VPN atraviesa Internet con una VPN de sitio a sitio, Microsoft ofrece otra opción de conexión aún más segura. Azure ExpressRoute es un vínculo de WAN dedicado entre Azure y una ubicación local o un proveedor de hospedaje de Exchange. Como las conexiones de ExpressRoute no se realizan a través de una conexión a Internet, ofrecen más confiabilidad, más velocidad, menor latencia y mayor seguridad que las conexiones a Internet normales. Además, dado que se trata de una conexión directa del proveedor de telecomunicaciones del cliente, los datos no viajan a través de Internet y, por lo tanto, no están expuestos a ella.

Hay [disponibles](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) procedimientos recomendados para implementar una red híbrida segura que extienda una red local a Azure.

## <a name="disclaimer"></a>Renuncia de responsabilidades

- Este documento es meramente informativo. MICROSOFT NO EFECTÚA NINGÚN TIPO DE GARANTÍA, YA SEA EXPRESA, IMPLÍCITA O ESTATUTARIA, SOBRE LA INFORMACIÓN INCLUIDA EN EL PRESENTE DOCUMENTO. Este documento se proporciona &quot;tal cual&quot;. La información y las opiniones expresadas en este documento, como las direcciones URL y otras referencias a sitios web de Internet, pueden cambiar sin previo aviso. Los clientes que lean este documento se atienen a las consecuencias de usarlo.
- Este documento no proporciona a los clientes ningún derecho legal sobre ninguna propiedad intelectual de ningún producto o solución de Microsoft.
- Los clientes pueden copiar y usar este documento con fines internos y de referencia.
- Algunas de las recomendaciones de este documento pueden provocar un aumento del uso de datos, de la red o de los recursos de procesos en Azure, lo que podría incrementar los costos de las licencias o las suscripciones de Azure.
- Esta arquitectura está diseñada para servir como base para que los clientes puedan ajustarse a sus requisitos específicos y no debe usarse tal cual en un entorno de producción.
- Este documento se ha desarrollado como referencia y no debe usarse para definir todos los medios por los que un cliente puede cumplir normas y requisitos de cumplimiento específicos. Los clientes deben buscar apoyo legal de su organización sobre las implementaciones de cliente aprobadas.
