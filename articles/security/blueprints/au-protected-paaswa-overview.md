---
title: 'Plano técnico de seguridad y cumplimiento de Azure: aplicación web PaaS con clasificación Protegida para Australia'
description: 'Plano técnico de seguridad y cumplimiento de Azure: aplicación web PaaS con clasificación Protegida para Australia'
services: security
author: meladie
ms.assetid: 708aa129-b226-4e02-85c6-1f86e54564e4
ms.service: security
ms.topic: article
ms.date: 08/23/2018
ms.author: meladie
ms.openlocfilehash: 8fd3725a5f3cd45da261aca17bf0f89a3e5a5aa0
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/04/2019
ms.locfileid: "54055190"
---
# <a name="azure-security-and-compliance-blueprint---paas-web-application-for-australia-protected"></a>Plano técnico de seguridad y cumplimiento de Azure: aplicación web PaaS con clasificación Protegida para Australia

## <a name="overview"></a>Información general

Este Plano técnico de seguridad y cumplimiento de Azure proporciona orientación para la implementación de un entorno de plataforma como servicio (PaaS) adecuado para la colección, el almacenamiento y la recuperación de datos con clasificación Protegida del Gobierno de Australia que sea compatibles con los objetivos del Manual de Seguridad de la Información (ISM) del Gobierno australiano producido por la Dirección Australiana de Señales (ASD). Este plano técnico muestra una arquitectura de referencia común y ayuda a comprobar cómo tratar adecuadamente los datos confidenciales del Gobierno en un entorno seguro, de cumplimiento, con varios niveles.

Esta arquitectura de referencia, la guía de implementación, y el modelo de amenazas proporcionan una base para que los clientes se encarguen de sus propios procesos de planificación y acreditación del sistema, lo que ayuda a los clientes a implementar cargas de trabajo en Azure conforme a ASD. Los clientes pueden elegir implementar una instancia de Azure VPN Gateway o ExpressRoute para usar servicios federados y para integrar los recursos locales en recursos de Azure. Los clientes deben considerar las consecuencias de seguridad del uso de recursos locales. Es necesaria la configuración adicional para cumplir todos los requisitos, ya que pueden variar en función de las características propias de la implementación de cada cliente.

Lograr el cumplimiento de ASD requiere que un asesor de seguridad de información registrado audite el sistema. Los clientes tienen la responsabilidad de realizar las evaluaciones de seguridad y cumplimiento adecuadas de cualquier solución compilada con esta arquitectura, ya que los requisitos pueden variar en función de las características de implementación de cada cliente.

## <a name="architecture-diagram-and-components"></a>Componentes y diagrama de la arquitectura
Esta solución proporciona una arquitectura de referencia para una aplicación web de PaaS con un back-end de Azure SQL Database. La aplicación web está hospedada en una instancia de Azure App Service Environment aislada, que es un entorno privado y dedicado en un centro de datos de Azure. La carga del entorno equilibra el tráfico para la aplicación web entre las máquinas virtuales administradas por Azure. Todas las conexiones de aplicación web requieren TLS con una versión mínima de 1.2. Esta arquitectura también incluye grupos de seguridad de red, una instancia de Application Gateway, Azure DNS y Load Balancer.

La arquitectura puede proporcionar un entorno híbrido seguro que extiende una red local a Azure, lo que permite que los usuarios corporativos de la red de área local privada de una organización o de Internet obtengan acceso a cargas de trabajo basadas en web de forma segura. Para las soluciones locales, el cliente es responsable de todos los aspectos de seguridad, las operaciones y el cumplimiento.

Los recursos de Azure incluidos en esta solución pueden conectarse a una red local o una instalación de colocalización de centro de datos (por ejemplo, CDC en Canberra) a través de una VPN IPSec con una VPN Gateway y a través de ExpressRoute. La decisión de utilizar una VPN debe tomarse teniendo en cuenta la clasificación de los datos transmitidos y la ruta de acceso de red. Los clientes que ejecuten cargas de trabajo críticas a gran escala con requisitos de macrodatos deben considerar la opción de usar una arquitectura de red híbrida con ExpressRoute para la conectividad de red privada con los servicios de Azure. Consulte la sección [Instrucciones y recomendaciones](#guidance-and-recommendations) para obtener más información sobre los mecanismos de conexión a Azure.

La federación con Azure Active Directory debe usarse para permitir que los usuarios se autentiquen con las credenciales locales y tengan acceso a todos los recursos en la nube mediante una infraestructura local de Servicios de federación de Active Directory (AD FS). Servicios de federación de Active Directory (AD FS) puede proporcionar funcionalidades de federación de identidades simplificada, protegida e inicio de sesión único web para este entorno híbrido. Consulte la sección [Instrucciones y recomendaciones](#guidance-and-recommendations) para obtener más información sobre la configuración de Azure Active Directory.

La solución utiliza cuentas de Azure Storage que los clientes pueden configurar para que usen Storage Service Encryption para preservar la confidencialidad de los datos en reposo. Azure almacena tres copias de los datos en una región seleccionada de un cliente para proporcionar resistencia. Las regiones de Azure se implementan en pares de regiones resistentes, y el almacenamiento con redundancia geográfica garantiza que los datos se repliquen en la segunda región también con tres copias. Esto evita que un evento adverso en la ubicación de datos principal del cliente produzca una pérdida de datos.

Para mejorar la seguridad, todos los recursos de Azure de esta solución se administran como un grupo de recursos mediante Azure Resource Manager. El control de acceso basado en roles de Azure Active Directory se utiliza para controlar el acceso a los recursos y claves implementados de Azure Key Vault. El mantenimiento del sistema se supervisa mediante Azure Security Center y Azure Monitor. Los clientes configuran ambos servicios de monitorización para capturar registros y mostrar el estado del sistema en un único panel de fácil navegación. Azure Application Gateway está configurado como un firewall en modo de prevención y no permite tráfico que no sea TLS v1.2 o posterior. La solución utiliza Azure Application Service Environment v2 para aislar el nivel web en un entorno que no es multiinquilino.

![Arquitectura de referencia de una aplicación web PaaS con clasificación Protegida de Australia](images/au-protected-paaswa-architecture.png?raw=true "Diagrama de una arquitectura de referencia de una aplicación web PaaS con clasificación Protegida de Australia")

Esta solución usa los siguientes servicios de Azure. Se puede encontrar más información en la sección [Arquitectura de implementación](#deployment-architecture).

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
- Azure Monitor
- Azure Resource Manager
- Azure Security Center
- Azure SQL Database
- Azure Storage
- Azure Log Analytics
- Azure Virtual Network
    - (1) Red /16
    - (4) /24 redes
    - Grupos de seguridad de red
- Grupos de seguridad de red
- Almacén de Recovery Services
- Aplicación web de Azure

Este plano técnico contiene servicios de Azure que no están certificados para su uso en la clasificación Protegida por el Centro Australiano de Seguridad Cibernética (ACSC). Microsoft recomienda que los clientes revisen los informes de seguridad y auditoría publicados relacionados con estos servicios de Azure y usen su marco de administración de riesgos para determinar si el servicio de Azure es adecuado para su acreditación interna y el uso en el clasificación Protegida.

## <a name="deployment-architecture"></a>Arquitectura de implementación
En la siguiente sección se detallan los elementos de desarrollo e implementación.

**Azure Resource Manager**: [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) permite a los clientes trabajar con los recursos de la solución como grupo. Los clientes pueden implementar, actualizar o eliminar todos los recursos de la solución en una sola operación coordinada. Para la implementación los clientes utilizan una plantilla, y esta plantilla puede funcionar en diferentes entornos, como pruebas, ensayo y producción. Resource Manager proporciona características de seguridad, auditoría y etiquetado que ayudan a los clientes a administrar los recursos después de la implementación

**Host de tipo bastión**: el host de tipo bastión es el único punto de entrada que permite a los usuarios acceder a los recursos implementados en este entorno. El host de tipo bastión proporciona una conexión segura a los recursos implementados al permitir solo el tráfico remoto desde las direcciones IP públicas de una lista segura. Para permitir el tráfico de escritorio remoto (RDP), el origen del tráfico debe definirse en el grupo de seguridad de red.

Esta solución crea una máquina virtual como host de tipo bastión unido mediante dominio con las siguientes configuraciones:
-   [Extensión antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware)
-   [Extensión de Diagnósticos de Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template)
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) con Azure Key Vault
-   Una [directiva de apagado automático](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) para reducir el consumo de recursos de la máquina virtual cuando no esté en uso.

**App Service Environment v2**: [Azure App Service Environment](https://docs.microsoft.com/azure/app-service/environment/intro) es una característica de App Service que proporciona un entorno completamente aislado y dedicado para ejecutar de forma segura las aplicaciones de App Service a gran escala.

Los entornos de App Service Environment están aislados para ejecutar únicamente las aplicaciones de un solo cliente, y siempre se implementan en una red virtual. Los clientes tienen un mayor control sobre el tráfico de red entrante y saliente de las aplicaciones, y las aplicaciones pueden establecer conexiones seguras de alta velocidad a los recursos corporativos locales a través de redes virtuales.

El uso de entornos de App Service para esta arquitectura permite los siguientes controles o configuraciones:

- Los entornos de App Service deben configurarse para usar el plan de servicio aislado
    - Configure los distintos entornos de App Service para las aplicaciones en diferentes clasificaciones
- Un host dentro de una red virtual de Azure protegida y reglas de seguridad de red.
- Entornos de App Service configurados con un certificado de equilibrador de carga interno autofirmado para la comunicación HTTPS. Como procedimiento recomendado, Microsoft recomienda el uso de una entidad de certificación de confianza para mejorar la seguridad.
- [Modo de equilibrio de carga interno](https://docs.microsoft.com/azure/app-service-web/app-service-environment-with-internal-load-balancer) (modo 3).
- Opción para deshabilitar [TLS v1.0 y v1.1](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Cambio del [cifrado TLS](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-custom-settings)
- Control de [puertos N/W de tráfico entrante](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-control-inbound-traffic)
- [Firewall de aplicaciones web: restringir datos](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-web-application-firewall)
- Al permitir el [tráfico de Azure SQL Database](https://docs.microsoft.com/azure/app-service-web/app-service-app-service-environment-network-architecture-overview)

**Aplicación web de Azure**: [Azure App Service](https://docs.microsoft.com/azure/app-service/) permite a los clientes crear y hospedar aplicaciones web en el lenguaje de programación que prefieran sin tener que administrar la infraestructura. Ofrece escalado automático y alta disponibilidad, es compatible con Windows y Linux, y permite implementaciones automatizadas desde GitHub, Azure DevOps Services o cualquier repositorio Git.

### <a name="virtual-network"></a>Virtual Network
La arquitectura define una red virtual privada con un espacio de direcciones de 10.200.0.0/16.

**Grupos de seguridad de red**: los [grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) contienen listas de control de acceso que permiten o deniegan el tráfico en una red virtual. Los grupos de seguridad de red pueden usarse para proteger el tráfico en una subred o un nivel de máquina virtual individual. Existen los siguientes grupos de seguridad de red:
- 1 grupo de seguridad de red para Application Gateway
- 1 grupo de seguridad de red para App Service Environment
- 1 grupo de seguridad de red para Azure SQL Database
- 1 grupo de seguridad de red para host de tipo bastión

Cada uno de los grupos de seguridad de red tiene puertos y protocolos específicos abiertos para que la solución pueda funcionar de forma segura y correcta. Además, las siguientes opciones de configuración están habilitadas para cada grupo de seguridad de red:

  - Los [eventos y registros de diagnóstico](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) están habilitados y se almacenan en la cuenta de almacenamiento.
  - Azure Log Analytics está conectado a los [diagnósticos del grupo de seguridad de red](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Subredes**: cada subred está asociada a su grupo de seguridad de red correspondiente.

**Azure DNS**: El sistema de nombres de dominio, o DNS, es responsable de traducir (o resolver) el nombre del sitio web o del servicio en su dirección IP. [Azure DNS](https://docs.microsoft.com/azure/dns/dns-overview) es un servicio de hospedaje para dominios DNS que ofrece resolución de nombres mediante la infraestructura de Azure. Al hospedar dominios en Azure, los usuarios pueden administrar los registros DNS con las mismas credenciales, API, herramientas y facturación que con los demás servicios de Azure. Azure DNS es compatible con dominios DNS privados.

**Azure Load Balancer**: [Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) permite a los clientes escalar las aplicaciones y crear alta disponibilidad para sus servicios. Load Balancer admite escenarios de entrada y de salida, proporciona baja latencia y alta capacidad de proceso, y puede escalar hasta millones de flujos para todas las aplicaciones TCP y UDP.

### <a name="data-in-transit"></a>Datos en tránsito
De manera predeterminada, Azure cifra todas las comunicaciones hacia y desde los centros de datos de Azure. 

Para los datos protegidos en tránsito desde las redes pertenecientes al cliente, la arquitectura utiliza Azure, Internet o ExpressRoute con una VPN Gateway configurada con IPSEC.

Además, todas las transacciones a Azure mediante el portal de administración de Azure se realizan mediante HTTPS con TLS v1.2.

### <a name="data-at-rest"></a>Datos en reposo
La arquitectura protege los datos en reposo mediante el cifrado, la auditoría de base de datos y otras medidas.

**Azure Storage**: para cumplir con los requisitos de los datos en reposo cifrados, [Azure Storage](https://azure.microsoft.com/services/storage/) usa [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Esto ayuda a proteger y salvaguardar los datos en apoyo de los compromisos de seguridad y requisitos de cumplimiento de la organización definidos por el ISM del Gobierno australiano.

**Azure Disk Encryption**: [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) aprovecha la función de BitLocker de Windows para proporcionar el cifrado del volumen de discos de datos. La solución se integra con Azure Key Vault para ayudar a controlar y administrar las claves de cifrado del disco.

**Azure SQL Database**: La instancia de Azure SQL Database usa las siguientes medidas de seguridad de base de datos:
-   La [autenticación y autorización de Active Directory](https://docs.microsoft.com/azure/sql-database/sql-database-AAD-authentication) permiten la administración de identidades de usuarios de bases de datos y otros servicios de Microsoft en una ubicación central.
-   La [auditoría de bases de datos SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) realiza un seguimiento de eventos de bases de datos y los escribe en un registro de auditoría de una cuenta de almacenamiento de Azure.
-   Azure SQL Database está configurado para usar el [cifrado de datos transparente](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql), que realiza el cifrado y descifrado en tiempo real de la base de datos, las copias de seguridad asociadas y los archivos de registro de transacciones, para proteger la información en reposo. El cifrado de datos transparente garantiza que los datos almacenados no hayan estado sujetos a un acceso no autorizado.
-   Las [reglas de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) impiden el acceso a los servidores de bases de datos hasta que se otorgan los permisos adecuados. Asimismo, otorgan acceso a las bases de datos según la dirección IP de origen de cada solicitud.
-   La [detección de amenazas de SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) permite detectar y responder ante posibles amenazas a medida que se producen, mediante el envío de alertas de seguridad para actividades sospechosas en la base de datos, posibles puntos vulnerables, ataques por inyección de código SQL y patrones anómalos de acceso a la base de datos. La detección de amenazas de SQL integra las alertas con [Azure Security Center](https://azure.microsoft.com/services/security-center/), que incluye detalles de actividades sospechosas y acciones recomendadas sobre cómo investigar y mitigar la amenaza.
-   Las [columnas de Always Encrypted](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) garantizan que los datos confidenciales nunca aparecen como texto no cifrado en el sistema de base de datos. Después de habilitar el cifrado de datos, solo las aplicaciones cliente o los servidores de aplicaciones con acceso a las claves pueden acceder a los datos de texto no cifrado.
- El [enmascaramiento dinámico de datos de SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) limita la exposición de datos confidenciales al enmascarar los datos para usuarios o aplicaciones sin privilegios. El enmascaramiento dinámico de datos puede detectar de forma automática datos potencialmente confidenciales y sugerir las máscaras adecuadas que se pueden aplicar. Esto contribuye a reducir el acceso de forma que los datos confidenciales no salen de la base de datos por medio de acceso no autorizado. Los clientes deberán ajustar la configuración del enmascaramiento de los datos dinámicos para ajustarse al esquema de la base de datos.

### <a name="identity-management"></a>Administración de identidades
Los clientes pueden utilizar de forma local Servicios de federación de Active Directory para federar con [Azure Active Directory](https://azure.microsoft.com/services/active-directory/), que es el directorio basado en la nube multiempresa y servicio de administración de identidades de Microsoft. [Azure Active Directory Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) integra directorios locales con Azure Active Directory. Todos los usuarios de esta solución requieren cuentas de Azure Active Directory, incluidos los usuarios que acceden a Azure SQL Database. Con el inicio de sesión de federación, los usuarios pueden iniciar sesión en Azure Active Directory y autenticarse en recursos de Azure mediante credenciales locales.

Además, las siguientes funcionalidades de Azure Active Directory ayudan a administrar el acceso a los datos en el entorno de Azure:
- La autenticación para acceder a la aplicación se realiza con Azure Active Directory. Para más información, consulte [Integración de aplicaciones con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). Además, el cifrado de columnas de la base de datos usa Azure Active Directory para autenticar la aplicación en Azure SQL Database. Para más información, consulte cómo [proteger los datos confidenciales en Azure SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
- El [control de acceso basado en rol de Azure](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) permite a los administradores especificar permisos de acceso bien definidos para conceder solo el nivel de acceso que los usuarios necesitan para realizar su trabajo. En lugar de dar a cada usuario permisos ilimitados para los recursos de Azure, los administradores pueden permitir solo ciertas acciones para acceder a los datos. El acceso a la suscripción está limitado al administrador de la suscripción.
- [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) permite a los clientes minimizar el número de usuarios con acceso a determinada información. Los administradores pueden usar Azure Active Directory Privileged Identity Management para detectar, restringir y supervisar las identidades con privilegios y su acceso a los recursos. Esta funcionalidad también puede utilizarse para aplicar un acceso administrativo a petición, Just-in-Time, cuando sea necesario.
- [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) detecta posibles puntos vulnerables que afectan a las identidades de una organización, configura respuestas automatizadas si surgen acciones sospechosas relacionadas con esas identidades, investiga incidentes sospechosos y toma las medidas oportunas para resolverlos.

**Azure Multi-Factor Authentication**: para proteger las identidades, debe implementarse la autenticación multifactor. [Azure Multi-Factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication/) es una solución fácil de usar, escalable y confiable que proporciona un segundo método de autenticación para proteger a los usuarios. Azure Multi-Factor Authenticaton utiliza la potencia de la nube y se integra con la instancia local de Active Directory y aplicaciones personalizadas. Esta protección se extiende a los escenarios críticos de gran volumen.

### <a name="security"></a>Seguridad
**Administración de secretos**: la solución utiliza [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) para la administración de claves y secretos. Azure Key Vault ayuda a proteger claves criptográficas y secretos usados por servicios y aplicaciones en la nube. Las siguientes funcionalidades de Azure Key Vault ayudan a los clientes a proteger los datos:
- Se configuran directivas de acceso avanzadas según las necesidades.
- Se definen directivas de acceso a Key Vault con los permisos mínimos requeridos para las claves y los secretos.
- Todas las claves y los secretos en Key Vault tienen fechas de expiración.
- Todas las claves de Key Vault están protegidas por módulos de seguridad de hardware especializados. El tipo de clave es una clave RSA de 2048 bits protegida por un módulo de seguridad de hardware.
- A todos los usuarios y entidades se les otorgan los permisos mínimos necesarios mediante el control de acceso basado en rol.
- Los registros de diagnóstico de Key Vault están habilitados con un período de retención de al menos 365 días.
- Las operaciones criptográficas permitidas para las claves están restringidas únicamente a las requeridas.

**Azure Security Center**: con [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro), los clientes pueden aplicar y administrar de forma centralizada las directivas de seguridad entre cargas de trabajo, limitar la exposición a amenazas y detectar ataques y responder a estos. Además, Azure Security Center accede a las configuraciones existentes de los servicios de Azure para proporcionar recomendaciones de configuración y servicio que ayuden a mejorar la postura de seguridad y a proteger los datos.

Azure Security Center usa una variedad de funcionalidades de detección para alertar a los clientes de posibles ataques contra sus entornos. Estas alertas contienen información útil acerca de lo que desencadenó la alerta, los recursos objetivo y el origen del ataque. Azure Security Center cuenta con un conjunto de [alertas de seguridad predefinidas](https://docs.microsoft.com/azure/security-center/security-center-alerts-type), que se desencadenan cuando tiene lugar una amenaza o actividad sospechosa. Las [reglas de alertas personalizadas](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) de Azure Security Center permiten a los clientes definir nuevas alertas de seguridad basadas en los datos ya recopilados en el entorno.

Azure Security Center proporciona alertas de seguridad e incidentes clasificados por orden de prioridad, lo que facilita a los clientes la detección y solución de posibles problemas de seguridad. Se genera un [informe de inteligencia de amenazas](https://docs.microsoft.com/azure/security-center/security-center-threat-report) por cada amenaza detectada para ayudar a los equipos de respuesta a incidentes a investigar y corregir las amenazas.

**Application Gateway**: La arquitectura reduce el riesgo de sufrir vulnerabilidades de seguridad, ya que usa Application Gateway con el firewall de aplicaciones web y tiene el conjunto de reglas OWASP habilitado. Entre estas funcionalidades, cabe destacar:

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

**Log Analytics**: esos registros se consolidan en [Log Analytics](https://azure.microsoft.com/services/log-analytics/) para el procesamiento, el almacenamiento y la creación de informes de panel. Una vez recopilados, se organizan en tablas independientes para cada tipo, lo que permite que todos los datos se puedan analizar conjuntamente con independencia de su origen. Además, Azure Security Center se integra con Log Analytics, lo que permite a los clientes usar consultas de Log Analytics para acceder a sus datos de eventos de seguridad y combinarlos con datos de otros servicios.

Las siguientes [soluciones de administración](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) de Log Analytics se incluyen como parte de esta arquitectura:
-   [Active Directory Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): la solución Active Directory Health Check evalúa el riesgo y el estado de los entornos de servidor a intervalos regulares y proporciona una lista prioritaria de recomendaciones específicas para la infraestructura de servidor implementada.
- [SQL Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): la solución SQL Health Check evalúa el riesgo y el estado de los entornos de servidor a intervalos regulares y proporciona a los clientes una lista prioritaria de recomendaciones específicas para la infraestructura de servidor implementada.
- [Agent Health](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): la solución Agent Health notifica el número de agentes implementados y su distribución geográfica, así como el número de agentes que no responden y el de agentes que envían datos operativos.
-   [Activity Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): la solución Activity Log Analytics ayuda a los clientes a analizar los registros de actividad de todas las suscripciones de Azure para un cliente.

**Azure Automation**: [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) almacena, ejecuta y administra runbooks. En esta solución, los runbooks ayudan a recopilar registros de Azure SQL Database. La solución [Change Tracking](https://docs.microsoft.com/azure/automation/automation-change-tracking) de Automation permite a los clientes identificar fácilmente los cambios en el entorno.

**Azure Monitor**: [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) ayuda a los usuarios a realizar un seguimiento del rendimiento, mantener la seguridad e identificar tendencias y permite a las organizaciones auditar, crear alertas y archivar datos, incluido el seguimiento de las llamadas a API en sus recursos de Azure.

Azure Network Watcher: [Azure Network Watcher](https://docs.microsoft.com/azure/network-watcher/network-watcher-monitoring-overview) proporciona herramientas para supervisar, diagnosticar, ver las métricas y habilitar o deshabilitar registros de recursos en una red virtual de Azure.  Las entidades de la Commonwealth deben implementar los registros de flujo de Network Watcher para grupos de seguridad de red y máquinas virtuales. Estos registros deben almacenarse en una cuenta de almacenamiento dedicada donde solo se guarden registros de seguridad; el acceso a la cuenta de almacenamiento debe estar protegida con controles de acceso basados en rol.

## <a name="threat-model"></a>Modelo de amenazas

El diagrama de flujo de datos de esta arquitectura de referencia está disponible para su [descarga](https://aka.ms/au-protected-paaswa-tm) y se encuentra a continuación. El modelo puede ayudar a los clientes a comprender los puntos de riesgo potencial de la infraestructura del sistema al realizar modificaciones.

![Modelo de amenazas de aplicación web de PaaS con clasificación Protegida de Australia](images/au-protected-paaswa-threat-model.png?raw=true "Diagrama de modelo de amenazas de aplicación web de PaaS con clasificación Protegida de Australia")

## <a name="compliance-documentation"></a>Documentación de cumplimiento
Microsoft produce esta documentación de cumplimiento en función de las plataformas y los servicios disponibles en Microsoft. Debido a la gran variedad de implementaciones de clientes, esta documentación proporciona un enfoque generalizado para una solución que solo se hospeda en el entorno de Azure. Los clientes pueden identificar y utilizar otros productos y servicios según sus propios entornos operativos y resultados empresariales. Los clientes que elijan utilizar recursos locales deben resolver la seguridad y las operaciones de esos recursos locales. Los clientes pueden personalizar la solución documentada para abordar sus requisitos locales y de seguridad específicos.

[Azure Security and Compliance Blueprint - AU-PROTECTED Customer Responsibility Matrix](https://aka.ms/au-protected-crm) (Plano técnico de seguridad y cumplimiento de Azure: matriz de responsabilidades del cliente con clasificación Protegida de Australia) muestra todos los controles de seguridad que exige la clasificación Protegida de Australia. En esta matriz se detalla si la implementación de cada objetivo es responsabilidad de Microsoft, del cliente o de ambos.

[Azure Security and Compliance Blueprint - AU-PROTECTED PaaS Web Application Implementation Matrix](https://aka.ms/au-protected-paaswa-cim) (Plano técnico de seguridad y cumplimiento de Azure: matriz de implementación de controles de aplicación web PaaS con clasificación Protegida de Australia) se proporciona información sobre qué controles de clasificación Protegida de Australia se abordan en la arquitectura de aplicaciones web PaaS; se incluyen descripciones detalladas de cómo la implementación satisface los requisitos de cada control tratado.

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
-   [Deshabilitar la escritura diferida de contraseñas](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr)
-   [Deshabilitar la escritura diferida de dispositivo](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-feature-device-writeback)
-   Deje la configuración predeterminada para [evitar eliminaciones accidentales](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-prevent-accidental-deletes) y [actualizaciones automáticas](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-feature-automatic-upgrade)


## <a name="disclaimer"></a>Renuncia de responsabilidades

 - Este documento es meramente informativo. MICROSOFT NO EFECTÚA NINGÚN TIPO DE GARANTÍA, YA SEA EXPRESA, IMPLÍCITA O ESTATUTARIA, SOBRE LA INFORMACIÓN INCLUIDA EN EL PRESENTE DOCUMENTO. Este documento se proporciona "tal cual". La información y las opiniones expresadas en este documento, como las direcciones URL y otras referencias a sitios web de Internet, pueden cambiar sin previo aviso. Los clientes que lean este documento se atienen a las consecuencias de usarlo.
 - Este documento no proporciona a los clientes ningún derecho legal sobre ninguna propiedad intelectual de ningún producto o solución de Microsoft.
 - Los clientes pueden copiar y usar este documento con fines internos y de referencia.
 - Algunas de las recomendaciones de este documento pueden provocar un aumento del uso de datos, de la red o de los recursos de procesos en Azure, lo que podría incrementar los costos de las licencias o las suscripciones de Azure.
 - Esta arquitectura está diseñada para servir como base para que los clientes puedan ajustarse a sus requisitos específicos y no debe usarse tal cual en un entorno de producción.
 - Este documento se ha desarrollado como referencia y no debe usarse para definir todos los medios por los que un cliente puede cumplir normas y requisitos de cumplimiento específicos. Los clientes deben buscar apoyo legal de su organización sobre las implementaciones de cliente aprobadas.
