---
title: 'Azure Security and Compliance Blueprint: análisis de datos para NIST SP 800-171'
description: 'Azure Security and Compliance Blueprint: análisis de datos para NIST SP 800-171'
services: security
author: jomolesk
ms.assetid: ca75d2a8-4e20-489b-9a9f-3a61e74b032d
ms.service: security
ms.topic: article
ms.date: 07/31/2018
ms.author: jomolesk
ms.openlocfilehash: f79ba9ae60454d4e73c914fc1c8af675a6d07d5d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60608867"
---
# <a name="azure-security-and-compliance-blueprint---data-analytics-for-nist-sp-800-171"></a>Azure Security and Compliance Blueprint: análisis de datos para NIST SP 800-171

## <a name="overview"></a>Información general
En [NIST Special Publication 800-171](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-171.pdf) se proporcionan instrucciones para proteger la información sin clasificar controlada (CUI) que reside en sistemas de información y organizaciones no federales. NIST SP 800-171 establece 14 familias de requisitos de seguridad para proteger la confidencialidad de CUI.

Este documento de Azure Security and Compliance Blueprint proporciona instrucciones para ayudar a los clientes a implementar una arquitectura de análisis de datos de Azure que implementa un subconjunto de controles de NIST SP 800-171. Esta solución muestra formas en que los clientes pueden cumplir requisitos de cumplimiento y de seguridad específicos. También sirve como base para que los clientes creen y configuren sus propias soluciones de análisis de datos en Azure.

Esta arquitectura de referencia, la guía de implementación asociada y el modelo de amenazas están diseñados como base para que los clientes puedan adaptarse a sus requisitos específicos. No deben usarse tal cual en un entorno de producción. La implementación de esta arquitectura sin modificación no es suficiente para satisfacer completamente los requisitos de NIST SP 800-171. Los clientes son responsables de llevar a cabo las evaluaciones de seguridad y cumplimiento adecuadas de todas las soluciones creadas que usen esta arquitectura. Los requisitos pueden variar en función de las especificaciones de implementación de cada cliente.

## <a name="architecture-diagram-and-components"></a>Componentes y diagrama de la arquitectura
Esta solución proporciona una plataforma analítica sobre la cual los clientes pueden crear sus propias herramientas de análisis. En la arquitectura de referencia se describe un caso de uso genérico. Los clientes lo pueden usar para introducir datos mediante importaciones de datos masivas con el administrador SQL o de datos. También lo pueden usar para introducir datos mediante actualizaciones de datos operativas por medio de un usuario operativo. Ambas series de tareas incorporan Azure Functions para importar datos en Azure SQL Database. El cliente debe configurar Azure Functions en Azure Portal para controlar las tareas de importación exclusivas de los requisitos de análisis propios del cliente.

Azure ofrece una variedad de servicios de informes y análisis para el cliente. Esta solución usa los servicios de Azure Machine Learning y SQL Database para examinar rápidamente los datos y entregar resultados más rápidos mediante un modelado más inteligente de los datos. Machine Learning está pensado para aumentar la velocidad de las consultas mediante el descubrimiento de nuevas relaciones entre conjuntos de datos. Inicialmente, los datos se entrenan con distintas funciones estadísticas. Luego, se pueden sincronizar hasta siete grupos de consultas adicionales con los mismos modelos tabulares para repartir la carga de trabajo de las consultas y reducir los tiempos de respuesta. El servidor del cliente aumenta el total de grupos de consulta a ocho.

Para mejorar el análisis y los informes, SQL Database puede configurarse con índices de almacén de columnas. Machine Learning y SQL Database se pueden escalar o reducir verticalmente o apagarse completamente en respuesta al uso del cliente. Todo el tráfico SQL se cifra con SSL mediante la inclusión de certificados autofirmados. Como procedimiento recomendado, le recomendamos que use una entidad de certificación de confianza para mejorar la seguridad.

Una vez cargados los datos en la base de datos SQL y entrenados con Machine Learning, los digiere el usuario operativo y el administrador de datos/SQL con Power BI. Power BI muestra los datos de forma intuitiva y reúne la información a través de múltiples conjuntos de datos para obtener un mayor conocimiento. Power BI tiene un alto grado de adaptabilidad y se integra fácilmente con SQL Database. Los clientes lo pueden configurar para controlar una amplia gama de escenarios necesarios desde un punto de vista empresarial.

Toda la solución se basa en el servicio Azure Storage, que los clientes configuran desde Azure Portal. Azure Storage cifra todos los datos con Storage Service Encryption para mantener la confidencialidad de los datos en reposo. El almacenamiento con redundancia geográfica garantiza que un evento adverso en el centro de datos principal del cliente no comporte una pérdida de datos. Se almacena una segunda copia en una ubicación independiente situada a cientos de kilómetros de distancia.

Para mejorar la seguridad, todos los recursos de esta solución se administran como un grupo de recursos mediante Azure Resource Manager. El control de acceso basado en roles de Azure Active Directory (Azure AD) se usa para controlar el acceso a los recursos implementados. Estos recursos incluyen claves de cliente en Azure Key Vault. El mantenimiento del sistema se supervisa mediante Azure Security Center y Azure Monitor. Los clientes configuran ambos servicios de supervisión para capturar registros. El mantenimiento del sistema se muestra en un panel único fácil de usar.

SQL Database se suele gestionar con SQL Server Management Studio. Se ejecuta desde un equipo local configurado para acceder a la base de datos SQL a través de una VPN segura o de una conexión de Azure ExpressRoute. *Le recomendamos que configure una conexión VPN o ExpressRoute para la administración y la importación de datos en el grupo de recursos*.

![Análisis de datos para NIST SP 800-171: diagrama de arquitectura de referencia](images/nist171-analytics-architecture.png "Análisis de datos para NIST SP 800-171: diagrama de arquitectura de referencia")

Esta solución usa los siguientes servicios de Azure. Para obtener más información, consulte la sección sobre la [arquitectura de implementación](#deployment-architecture).

- Application Insights
- Azure Active Directory
- Azure Data Catalog
- Azure Disk Encryption
- Azure Event Grid
- Azure Functions
- Azure Key Vault
- Azure Machine Learning
- Monitor de Azure (registros)
- Azure Security Center
- Azure SQL Database
- Azure Storage
- Azure Virtual Network
    - (1) /16 redes
    - (2) /24 redes
    - (2) Grupos de seguridad de red
- Panel de Power BI

## <a name="deployment-architecture"></a>Arquitectura de implementación
En la siguiente sección se detallan los elementos de desarrollo e implementación.

**Azure Event Grid**: con [Event Grid](https://docs.microsoft.com/azure/event-grid/overview), los clientes pueden crear fácilmente aplicaciones con arquitecturas basadas en eventos. Los usuarios deben seleccionar el recurso de Azure al que desean suscribirse. Luego, asignan al controlador de eventos o webhook un punto de conexión al que se enviará el evento. Los clientes pueden proteger los puntos de conexión de webhook agregando parámetros de consulta a la URL de webhook al crear una suscripción a eventos. Event Grid solo admite puntos de conexión de webhook HTTPS. Con Event Grid, los clientes pueden controlar el nivel de acceso concedido a distintos usuarios para llevar a cabo distintas operaciones de administración. Los usuarios pueden enumerar suscripciones de eventos, crear nuevas y generar claves. Event Grid usa Azure RBAC.

**Azure Functions**: [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview) es un servicio de procesos sin servidor que ejecuta código a petición. No es necesario que aprovisione o administre la infraestructura explícitamente. Use Azure Functions para ejecutar un script o un fragmento de código en respuesta a diversos eventos.

**Azure Machine Learning Service**: [Machine Learning](https://docs.microsoft.com/azure/machine-learning/service/) es una técnica de ciencia de datos que permite a los equipos utilizar datos existentes para prever tendencias, resultados y comportamientos futuros.

**Azure Data Catalog**: [Data Catalog](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) facilita que los usuarios que administran los datos puedan detectar y comprender los orígenes de datos. En los orígenes de datos comunes se pueden registrar, etiquetar y buscar datos. Los datos permanecen en la ubicación existente, pero se agrega una copia de sus metadatos a Data Catalog. Se incluye una referencia a la ubicación de los orígenes de datos. Los metadatos se indexan para que sea fácil detectar los orígenes de datos mediante una búsqueda. La indexación también facilita su comprensión para los usuarios que los detecten.

### <a name="virtual-network"></a>Virtual network
La arquitectura de referencia define una red virtual privada con un espacio de direcciones de 10.0.0.0/16.

**Grupos de seguridad de red**: los [grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSG) contienen listas de control de acceso que permiten o deniegan el tráfico en una red virtual. Los grupos de seguridad de red se pueden usar para proteger el tráfico en una subred o un nivel de máquina virtual individual. Los grupos de seguridad de red siguientes ya existen:
  - Un grupo de seguridad de red para Active Directory
  - Un grupo de seguridad de red para la carga de trabajo

Cada uno de los grupos de seguridad de red tiene puertos y protocolos específicos abiertos para que la solución pueda funcionar de forma segura y correcta. Además, las siguientes opciones de configuración están habilitadas para cada NSG:
  - Los [eventos y registros de diagnóstico](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) están habilitados y se almacenan en la cuenta de almacenamiento.
  - Registros de Azure Monitor se conecta a la [diagnósticos del NSG](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Subredes**: Cada subred se asocia a su NSG correspondiente.

### <a name="data-in-transit"></a>Datos en tránsito
De forma predeterminada, Azure cifra todas las comunicaciones hacia y desde los centros de datos de Azure. Todas las transacciones a Azure Storage mediante Azure Portal se efectúan por HTTPS.

### <a name="data-at-rest"></a>Datos en reposo

La arquitectura protege los datos en reposo mediante el cifrado, la auditoría de base de datos y otras medidas.

**Azure Storage**: para cumplir los requisitos de los datos en reposo cifrados, todo [Storage](https://azure.microsoft.com/services/storage/) usa [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Esta característica ayuda a proteger y salvaguardar los datos en apoyo de los compromisos de seguridad y requisitos de cumplimiento de la organización definidos por la directiva NIST SP 800-171.

**Azure Disk Encryption**: [Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) usa la función de BitLocker de Windows para proporcionar el cifrado del volumen de discos de datos. La solución se integra con Azure Key Vault para ayudar a controlar y administrar las claves de cifrado del disco.

**Azure SQL Database**: la instancia de SQL Database usa las siguientes medidas de seguridad de base de datos:
-   La [autenticación y autorización de Active Directory](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) permiten la administración de identidades de usuarios de bases de datos y otros servicios de Microsoft en una ubicación central.
-   La [auditoría de bases de datos SQL](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) realiza un seguimiento de eventos de bases de datos y los escribe en un registro de auditoría de una cuenta de almacenamiento de Azure.
-   SQL Database está configurado para usar el [cifrado de datos transparente](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql). Lleva a cabo un cifrado y descifrado en tiempo real de la base de datos, copias de seguridad asociadas y archivos de registro de transacciones para proteger la información en reposo. El cifrado de datos transparente garantiza que los datos almacenados no han estado sujetos a acceso no autorizado.
-   Las [reglas de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) impiden el acceso a los servidores de bases de datos hasta que se otorgan los permisos adecuados. Asimismo, otorgan acceso a las bases de datos según la dirección IP de origen de cada solicitud.
-   [Detección de amenazas de SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) permite detectar y responder a posibles amenazas cuando se producen. Proporciona alertas de seguridad si se producen actividades sospechosas en la base de datos, aparecen vulnerabilidades potenciales y ataques por inyección de código SQL, o si existen patrones anómalos de acceso a la base de datos.
-   Las [columnas cifradas](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) garantizan que los datos confidenciales nunca aparezcan como texto no cifrado en el sistema de base de datos. Una vez habilitado el cifrado de datos, solo las aplicaciones cliente o los servidores de aplicaciones con acceso a las claves pueden acceder a los datos de texto no cifrado.
- El [enmascaramiento dinámico de datos de SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) limita la exposición de los datos confidenciales al enmascarar los datos para usuarios o aplicaciones sin privilegios. Puede detectar de forma automática datos potencialmente confidenciales y sugerir las máscaras adecuadas que se pueden aplicar. El enmascaramiento dinámico de datos permite reducir el acceso para que los datos confidenciales no salgan de la base de datos por un acceso no autorizado. *Los clientes son responsables de ajustar la configuración para cumplir con el esquema de la base de datos.*

### <a name="identity-management"></a>Administración de identidades
Las siguientes tecnologías proporcionan funcionalidades de administración del acceso a datos confidenciales en el entorno de Azure:
-   [Azure AD](https://azure.microsoft.com/services/active-directory/) es el servicio de directorio y de administración de identidades multiinquilino basado en la nube de Microsoft. Todos los usuarios de esta solución se crean en Azure AD, incluidos los que acceden a SQL Database.
-   La autenticación para acceder a la aplicación se efectúa mediante Azure AD. Para obtener más información, consulte cómo se [integran las aplicaciones con Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). El cifrado de las columnas de la base de datos también usa Azure AD para autenticar la aplicación en SQL Database. Para más información, consulte cómo [proteger los datos confidenciales en SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   Los administradores pueden usar [Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) para definir permisos de acceso específicos. Con Azure RBAC, pueden conceder únicamente el grado de acceso que los usuarios necesiten para llevar a cabo sus tareas. En lugar de dar a cada usuario permisos ilimitados para los recursos de Azure, los administradores pueden permitir solo ciertas acciones para acceder a los datos. El acceso a la suscripción está limitado al administrador de la suscripción.
- Los clientes pueden usar [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) para minimizar el número de usuarios con acceso a determinada información, como los datos. Los administradores pueden usar Azure AD Privileged Identity Management para detectar, restringir y supervisar las identidades con privilegios y su acceso a los recursos. Esta funcionalidad también se puede utilizar para aplicar un acceso administrativo a petición, Just-in-Time, cuando sea necesario.
-   [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) detecta posibles vulnerabilidades que afecten a las identidades de una organización. Configura respuestas automatizadas a acciones sospechosas detectadas que están relacionadas con las identidades de una organización. También investiga incidentes sospechosos para tomar las medidas adecuadas para resolverlos.

### <a name="security"></a>Seguridad
**Administración de secretos**: la solución utiliza [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) para la administración de claves y secretos. Key Vault ayuda a proteger claves criptográficas y secretos usados por servicios y aplicaciones en la nube. Las siguientes funcionalidades de Key Vault ayudan a los clientes a proteger los datos:
- Se configuran directivas de acceso avanzadas según las necesidades.
- Se definen directivas de acceso a Key Vault con los permisos mínimos requeridos para las claves y los secretos.
- Todas las claves y los secretos en Key Vault tienen fechas de expiración.
- Todas las claves de Key Vault están protegidas por módulos de seguridad de hardware especializados. El tipo de clave es una clave RSA de 2048 bits protegida por un módulo de seguridad de hardware.
- Se otorgan los permisos necesarios mínimos a todos los usuarios e identidades mediante RBAC.
- Los registros de diagnóstico de Key Vault están habilitados con un período de retención de al menos 365 días.
- Las operaciones criptográficas permitidas para las claves están restringidas únicamente a las requeridas.

**Azure Security Center**: con [Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro), los clientes pueden aplicar y administrar de forma centralizada las directivas de seguridad entre cargas de trabajo, limitar la exposición a amenazas y detectar ataques y responder a estos. Security Center también accede a las configuraciones existentes de los servicios de Azure para proporcionar recomendaciones de configuración y servicio que ayuden a mejorar la postura de seguridad y a proteger los datos.

 Security Center usa una variedad de funcionalidades de detección para alertar a los clientes de posibles ataques contra sus entornos. Estas alertas contienen información útil acerca de lo que desencadenó la alerta, los recursos objetivo y el origen del ataque. Security Center cuenta con un conjunto de [alertas de seguridad predefinidas](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) que se desencadenan cuando tiene lugar una amenaza o actividad sospechosa. Los clientes pueden utilizar [reglas de alertas personalizadas](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) para definir nuevas alertas de seguridad basadas en los datos ya recopilados en el entorno.

 Security Center proporciona incidentes y alertas de seguridad prioritarias. Security Center facilita a los clientes la detección y la resolución de posibles problemas de seguridad. Para cada amenaza detectada se genera un [informe de inteligencia de amenazas](https://docs.microsoft.com/azure/security-center/security-center-threat-report). Los equipos de respuesta a los incidentes pueden usar los informes para investigar y solucionar las amenazas.

### <a name="logging-and-auditing"></a>Registro y auditoría

Los servicios de Azure proporcionan un registro completo de la actividad de usuario y del sistema, así como de mantenimiento del sistema:
- **Registros de actividad**: los [registros de actividad](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) proporcionan información detallada sobre las operaciones realizadas en los recursos de la suscripción. Los registros de actividad pueden ayudar a determinar el iniciador de una operación, el momento en que se produce y el estado.
- **Registros de diagnóstico**: los [registros de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) son todos los registros emitidos por todos los recursos. Estos registros incluyen registros del sistema de eventos de Windows, registros de Azure Storage, registros de auditoría de Key Vault, y registros de firewall y acceso a Azure Application Gateway. Todos los registros de diagnóstico se escriben en una cuenta de almacenamiento de Azure centralizada y cifrada para su archivado. Los usuarios pueden configurar el período de retención (de hasta 730 días) para satisfacer sus requisitos específicos.

**Registros de Azure Monitor**: Los registros se consolidan en [registros de Azure Monitor](https://azure.microsoft.com/services/log-analytics/) para procesamiento, almacenamiento e informes de panel. Una vez recopilados, los datos se organizan en tablas independientes por tipo de datos en las áreas de trabajo de Log Analytics. De este modo, todos los datos se pueden analizar juntos, independientemente de su fuente original. Security Center se integra con los registros de Azure Monitor. Los clientes pueden usar las consultas de Kusto para acceder a sus datos de eventos de seguridad y la combinamos con datos de otros servicios.

El siguiente Azure [soluciones de supervisión](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) se incluyen como parte de esta arquitectura:
-   [Active Directory Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): la solución Active Directory Health Check evalúa el riesgo y el estado de los entornos de servidor de forma periódica. Proporciona una lista priorizada de recomendaciones específicas para la infraestructura de servidor implementada.
- [SQL Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): la solución SQL Health Check evalúa el riesgo y el estado de los entornos de servidor de forma periódica. Proporciona a los clientes una lista priorizada de recomendaciones específicas de la infraestructura de servidor implementada.
- [Agent Health](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): la solución Agent Health informa de cuántos agentes se implementan y su distribución geográfica. También informa de cuántos agentes no responden y del número de agentes que envían datos operativos.
-   [Activity Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): la solución Activity Log Analytics ayuda a los clientes a analizar los registros de actividad de todas las suscripciones de Azure para un cliente.

**Azure Automation**: [Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) almacena, ejecuta y administra runbooks. En esta solución, los runbooks ayudan a recopilar registros de SQL Database. Los clientes pueden usar la solución [Change Tracking](https://docs.microsoft.com/azure/automation/automation-change-tracking) de Automation para identificar fácilmente los cambios en el entorno.

**Azure Monitor**: [Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) ayuda a los usuarios a efectuar un seguimiento del rendimiento, a mantener la seguridad y a identificar tendencias. Las organizaciones lo pueden usar para auditar, crear alertas y archivar datos. También pueden registrar llamadas API en sus recursos de Azure.

**Application Insights**: [Application Insights](https://docs.microsoft.com/azure/application-insights/) es un servicio de Application Performance Management extensible para desarrolladores web en varias plataformas. Detecta anomalías en el rendimiento e incluye potentes herramientas de análisis. Las herramientas permiten diagnosticar problemas y ayudan a los clientes a comprender qué hacen los usuarios con la aplicación. Está diseñado para ayudar a los usuarios a mejorar continuamente el rendimiento y la facilidad de uso.

## <a name="threat-model"></a>Modelo de amenazas

El diagrama de flujo de datos de esta arquitectura de referencia está disponible para su [descarga](https://aka.ms/nist171-analytics-tm) y se encuentra aquí. El modelo puede ayudar a los clientes a comprender los puntos de riesgo potencial de la infraestructura del sistema al efectuar modificaciones.

![Análisis de datos para NIST SP 800-171: modelo de amenazas](images/nist171-analytics-threat-model.png "Análisis de datos para NIST SP 800-171: modelo de amenazas")

## <a name="compliance-documentation"></a>Documentación de cumplimiento
En [Azure Security and Compliance Blueprint: matriz de responsabilidades del cliente para NIST SP 800-171](https://aka.ms/nist171-crm) muestra todos los controles de seguridad que exige NIST SP 800-171. En esta matriz se detalla si la implementación de cada objetivo es responsabilidad de Microsoft, del cliente o de ambos.

En [Azure Security and Compliance Blueprint: matriz de implementación de control de análisis de datos para NIST SP 800-171](https://aka.ms/nist171-analytics-cim) se ofrece información sobre qué controles de NIST SP 800-171 se abordan en la arquitectura de análisis de datos. Incluye descripciones detalladas de cómo la implementación cumple los requisitos de cada control cubierto.


## <a name="guidance-and-recommendations"></a>Instrucciones y recomendaciones
### <a name="vpn-and-expressroute"></a>VPN y ExpressRoute
Se debe configurar un túnel VPN seguro o [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) para establecer una conexión segura a los recursos implementados como parte de esta arquitectura de referencia de análisis de datos. Al configurar una VPN o ExpressRoute adecuadamente, los clientes pueden agregar una capa de protección para los datos en tránsito.

Mediante la implementación de un túnel VPN seguro con Azure, se puede crear una conexión privada virtual entre una red local y una red virtual de Azure. Esta conexión se efectúa a través de Internet. Los clientes pueden usar la conexión para colocar con seguridad la información en un "túnel" dentro de un vínculo cifrado entre su red y Azure. La VPN de sitio a sitio es una tecnología segura y madura que empresas de todos los tamaños han implementado durante décadas. Se utiliza [el modo de túnel IPsec](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) en esta opción como mecanismo de cifrado.

Dado que el tráfico dentro del túnel VPN atraviesa Internet con una VPN de sitio a sitio, Microsoft ofrece otra opción de conexión aún más segura. ExpressRoute es un vínculo de WAN dedicado entre Azure y una ubicación local o un proveedor de hospedaje de Exchange. Las conexiones de ExpressRoute se conectan directamente al proveedor de telecomunicaciones de un cliente. Como resultado, los datos no viajan a través de Internet y no están expuestos. Estas conexiones ofrecen más confiabilidad, velocidades más altas, menores latencias y mayor seguridad que las conexiones habituales. 

Hay [disponibles](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) procedimientos recomendados para implementar una red híbrida segura que extienda una red local a Azure.

### <a name="extract-transform-load-process"></a>Proceso de extracción, transformación y carga
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) puede cargar datos en SQL Database sin necesidad de una herramienta independiente para extraer, transformar, cargar o importar. PolyBase permite el acceso a los datos mediante consultas T-SQL. Con PolyBase se puede usar la inteligencia empresarial y la pila de análisis de Microsoft, así como herramientas de terceros compatibles con SQL Server.

### <a name="azure-ad-setup"></a>Configuración de Azure AD
[Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) es esencial para la administración de la implementación y la concesión de acceso al personal que interactúa con el entorno. Active Directory local se puede integrar con Azure AD en [cuatro clics](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Los clientes también pueden vincular la infraestructura implementada de Active Directory (controladores de dominio) con Azure AD. Para ello, debe convertir la infraestructura implementada de Active Directory en un subdominio de un bosque de Azure AD.

## <a name="disclaimer"></a>Renuncia de responsabilidades

 - Este documento es meramente informativo. MICROSOFT NO EFECTÚA NINGÚN TIPO DE GARANTÍA, YA SEA EXPRESA, IMPLÍCITA O ESTATUTARIA, SOBRE LA INFORMACIÓN INCLUIDA EN EL PRESENTE DOCUMENTO. Este documento se proporciona "tal cual". La información y las opiniones expresadas en este documento, como las direcciones URL y otras referencias a sitios web de Internet, pueden cambiar sin previo aviso. Los clientes que lean este documento se atienen a las consecuencias de usarlo.
 - Este documento no proporciona a los clientes ningún derecho legal sobre ninguna propiedad intelectual de ningún producto o solución de Microsoft.
 - Los clientes pueden copiar y usar este documento con fines internos y de referencia.
 - Algunas de las recomendaciones de este documento pueden provocar un aumento del uso de datos, de la red o de los recursos de procesos en Azure, lo que podría incrementar los costos de las licencias o las suscripciones de Azure.
 - Esta arquitectura está diseñada para servir como base para que los clientes puedan ajustarse a sus requisitos específicos y no debe usarse tal cual en un entorno de producción.
 - Este documento se ha desarrollado como referencia y no debe usarse para definir todos los medios por los que un cliente puede cumplir normas y requisitos de cumplimiento específicos. Los clientes deben buscar apoyo legal de su organización sobre las implementaciones de cliente aprobadas.
