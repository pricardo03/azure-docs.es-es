---
title: 'Azure Security and Compliance Blueprint: análisis de datos para el Sistema sanitario nacional del Reino Unido'
description: 'Azure Security and Compliance Blueprint: análisis de datos para el Sistema sanitario nacional del Reino Unido'
services: security
author: jomolesk
ms.assetid: 103dff31-e262-44a6-9b50-3b3467c0cb3a
ms.service: security
ms.topic: article
ms.date: 06/15/2018
ms.author: jomolesk
ms.openlocfilehash: d2e3b4b199065510d244d384ff69d5231bef5919
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60609071"
---
# <a name="azure-security-and-compliance-blueprint-data-analytics-for-uk-nhs"></a>Plano técnico de seguridad y cumplimiento de Azure: Análisis de datos para NHS Reino Unido

## <a name="overview"></a>Información general

Este plano técnico Azure Security and Compliance Blueprint proporciona una arquitectura de referencia y una guía para una solución de análisis de datos adecuada para la recopilación, el almacenamiento, el análisis y la recuperación de datos sanitarios. Esta solución muestra las formas en que los clientes pueden cumplir con las instrucciones proporcionadas en la publicación [Cloud Security Good Practice Guide](https://digital.nhs.uk/data-and-information/looking-after-information/data-security-and-information-governance/nhs-and-social-care-data-off-shoring-and-the-use-of-public-cloud-services/health-and-social-care-cloud-security-good-practice-guide) (Guía de las buenas prácticas para la seguridad en la nube) de [NHS Digital](https://digital.nhs.uk/), un asociado de Department of Health and Social Care (DHSC) (Departamento de Sanidad y Bienestar Social) del Reino Unido. Esta publicación se basa en los 14 [principios de seguridad en la nube](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) publicados por National Cyber Security Centre (NCSC) (Centro de seguridad cibernética nacional) del Reino Unido.

Esta arquitectura de referencia, la guía de implementación y el modelo de amenazas están diseñados como base para que los clientes puedan ajustarse a sus requisitos específicos y no debe usarse tal cual en un entorno de producción sin configuración adicional. Los clientes tienen la responsabilidad de realizar las evaluaciones de seguridad y cumplimiento adecuadas de cualquier solución compilada con esta arquitectura, ya que los requisitos pueden variar en función de las características de implementación de cada cliente.

## <a name="architecture-diagram-and-components"></a>Componentes y diagrama de la arquitectura

Esta solución proporciona una plataforma analítica sobre la cual los clientes pueden crear sus propias herramientas de análisis. La arquitectura de referencia esboza un caso de uso genérico en el que los clientes introducen datos ya sea a través de importaciones de datos masivos por parte del administrador de datos/SQL o mediante actualizaciones de datos operativos a través de un usuario operativo. Ambos flujos de trabajo incorporan Azure Functions para importar datos en Azure SQL Database y todas las conexiones externas requieren TLSv1.2. El cliente debe configurar Azure Functions en Azure Portal para controlar las tareas de importación exclusivas de los requisitos de análisis propios del cliente.

Azure ofrece varios servicios de informes y análisis al cliente; sin embargo, esta solución incorpora los servicios de Azure Analysis Services junto con Azure SQL Database para examinar rápidamente los datos y entregar resultados más rápidos a través de un modelado más inteligente de los datos del cliente. Los servicios de Azure Analysis Service es una forma de aprendizaje automático destinada a aumentar la velocidad de las consultas mediante el descubrimiento de nuevas relaciones entre conjuntos de datos. Una vez que los datos se han entrenado mediante varias funciones estadísticas, se pueden sincronizar hasta siete grupos de consulta adicionales (ocho en total incluido el servidor del cliente) con los mismos modelos tabulares para distribuir la carga de trabajo de la consulta y reducir el tiempo de respuesta.

Para mejorar el análisis y los informes, las bases de datos de Azure SQL Database pueden configurarse con índices de almacén de columnas. Tanto los servicios de Azure Analytics Services como las bases de datos de Azure SQL Database se pueden escalar o reducir verticalmente o apagarse completamente en respuesta al uso del cliente. Todo el tráfico SQL se cifra con SSL mediante la inclusión de certificados autofirmados. Como procedimiento recomendado, Azure recomienda el uso de una entidad de certificación de confianza para mejorar la seguridad.

Cuando se suben los datos a la instancia de Azure SQL Database y se entrenan mediante los servicios de Azure Analysis Service, los digiere el usuario operativo y el administrador de datos o SQL con Power BI. Power BI muestra los datos de forma intuitiva y reúne la información a través de múltiples conjuntos de datos para obtener un mayor conocimiento. Su alto grado de adaptabilidad y fácil integración con Azure SQL Database asegura que los clientes puedan configurarlo para tratar una amplia gama de escenarios según sus necesidades de negocio.

La solución utiliza cuentas de Azure Storage que los clientes pueden configurar para que usen Storage Service Encryption para preservar la confidencialidad de los datos en reposo. Azure almacena tres copias de datos en un centro de datos elegido por el cliente con fines de resistencia. El almacenamiento con redundancia geográfica garantiza que los datos se replicarán en un centro de datos secundario a cientos de kilómetros de distancia y que, de nuevo, se guardarán tres copias en ese centro de datos, lo que impide que un evento adverso que suceda en el centro de datos principal del cliente pueda provocar una pérdida de datos.

Para mejorar la seguridad, todos los recursos de esta solución se administran como un grupo de recursos mediante Azure Resource Manager. El control de acceso basado en rol de Azure Active Directory se utiliza para controlar el acceso a los recursos implementados, incluidas las claves en Azure Key Vault. El mantenimiento del sistema se supervisa mediante Azure Security Center y Azure Monitor. Los clientes configuran ambos servicios de monitorización para capturar registros y mostrar el estado del sistema en un único panel de fácil navegación.

Azure SQL Database se administra comúnmente mediante SQL Server Management Studio, que se ejecuta desde una máquina local configurada para acceder a Azure SQL Database mediante una conexión segura VPN o ExpressRoute. **Microsoft recomienda configurar una conexión VPN o de ExpressRoute para la administración y la importación de datos en el grupo de recursos de la arquitectura de referencia**.

![Diagrama de arquitectura de referencia de análisis para el Sistema sanitario nacional del Reino Unido](images/uknhs-analytics-architecture.png?raw=true "Analytics for UK NHS reference architecture diagram")

Esta solución usa los siguientes servicios de Azure. Los detalles de la arquitectura de implementación se encuentran en la sección [Arquitectura de implementación](#deployment-architecture).

- Azure Active Directory
- Azure Analysis Service
- Azure Automation
- Azure Data Catalog
- Azure Disk Encryption
- Azure Event Grid
- Azure Functions
- Azure Key Vault
- Azure Monitor
- Azure Security Center
- Azure SQL Database
- Azure Storage
- Azure Virtual Network
    - (1) /16 redes
    - (2) /24 redes
    - (2) Grupos de seguridad de red
- Panel de Power BI

## <a name="deployment-architecture"></a>Arquitectura de implementación

En la siguiente sección se detallan los elementos de desarrollo e implementación.

**Azure Functions**: [Azure Functions](https://docs.microsoft.com/azure/azure-functions/functions-overview) es un servicio de proceso sin servidor que permite ejecutar código a petición sin necesidad de aprovisionar ni administrar explícitamente la infraestructura. Use Azure Functions para ejecutar un script o un fragmento de código en respuesta a diversos eventos.

**Servicio de análisis de Azure**: [Servicio de análisis de Azure](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview) proporciona modelado de datos de empresa e integración con servicios de plataforma de datos de Azure. Azure Analysis Service acelera la navegación a través de cantidades masivas de datos mediante la combinación de datos de múltiples orígenes en un único modelo de datos.

### <a name="virtual-network"></a>Virtual network

La arquitectura define una red virtual privada con un espacio de direcciones de 10.200.0.0/16.

**Grupos de seguridad de red**: los [grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) contienen listas de control de acceso que permiten o deniegan el tráfico en una red virtual. Los grupos de seguridad de red pueden usarse para proteger el tráfico en una subred o un nivel de máquina virtual individual. Existen los siguientes grupos de seguridad de red:

  - Un grupo de seguridad de red para Active Directory
  - Un grupo de seguridad de red para la carga de trabajo

Cada uno de los grupos de seguridad de red tiene puertos y protocolos específicos abiertos para que la solución pueda funcionar de forma segura y correcta. Además, las siguientes opciones de configuración están habilitadas para cada grupo de seguridad de red:

- Los [eventos y registros de diagnóstico](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) están habilitados y se almacenan en la cuenta de almacenamiento.
- Registros de Azure Monitor se conecta a la [grupo de seguridad de red&#39;los registros de diagnóstico s](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json)

**Subredes**: cada subred está asociada a su grupo de seguridad de red correspondiente.

### <a name="data-in-transit"></a>Datos en tránsito

De manera predeterminada, Azure cifra todas las comunicaciones hacia y desde los centros de datos de Azure. Todas las transacciones a Azure Storage mediante Azure Portal se realizan mediante HTTPS.

### <a name="data-at-rest"></a>Datos en reposo

La arquitectura protege los datos en reposo mediante el cifrado, la auditoría de base de datos y otras medidas.

**Azure Storage**: para cumplir con los requisitos de los datos en reposo cifrados, [Azure Storage](https://azure.microsoft.com/services/storage/) usa [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Esto ayuda a proteger los datos en cumplimiento de los compromisos de seguridad de la organización y los requisitos de cumplimiento definidos por NHS Digital.

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
- Todas las claves de Key Vault están protegidas por módulos de seguridad de hardware especializados. El tipo de clave es una clave RSA de 2048 bits protegida por un módulo de seguridad de hardware.
- A todos los usuarios y entidades se les otorgan los permisos mínimos necesarios mediante el control de acceso basado en rol.
- Los registros de diagnóstico de Key Vault están habilitados con un período de retención de al menos 365 días.
- Las operaciones criptográficas permitidas para las claves están restringidas únicamente a las requeridas.

**Azure Security Center**: con [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro), los clientes pueden aplicar y administrar de forma centralizada las directivas de seguridad entre cargas de trabajo, limitar la exposición a amenazas y detectar ataques y responder a estos. Además, Azure Security Center accede a las configuraciones existentes de los servicios de Azure para proporcionar recomendaciones de configuración y servicio que ayuden a mejorar la postura de seguridad y a proteger los datos.

Azure Security Center usa una variedad de funcionalidades de detección para alertar a los clientes de posibles ataques contra sus entornos. Estas alertas contienen información útil acerca de lo que desencadenó la alerta, los recursos objetivo y el origen del ataque. Azure Security Center cuenta con un conjunto de [alertas de seguridad predefinidas](https://docs.microsoft.com/azure/security-center/security-center-alerts-type), que se desencadenan cuando tiene lugar una amenaza o actividad sospechosa. Las [reglas de alertas personalizadas](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) de Azure Security Center permiten a los clientes definir nuevas alertas de seguridad basadas en los datos ya recopilados en el entorno.

Azure Security Center proporciona alertas de seguridad e incidentes clasificados por orden de prioridad, lo que facilita a los clientes la detección y solución de posibles problemas de seguridad. Se genera un [informe de inteligencia de amenazas](https://docs.microsoft.com/azure/security-center/security-center-threat-report) por cada amenaza detectada para ayudar a los equipos de respuesta a incidentes a investigar y corregir las amenazas.

### <a name="logging-and-auditing"></a>Registro y auditoría

Los servicios de Azure proporcionan un registro completo de la actividad de usuario y del sistema, así como de mantenimiento del sistema:
- **Registros de actividad**: los [registros de actividad](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) proporcionan información detallada sobre las operaciones realizadas en los recursos de la suscripción. Los registros de actividad pueden ayudar a determinar el iniciador de una operación, el momento en que se produce y el estado.
- **Registros de diagnóstico**: los [registros de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) son todos los registros emitidos por todos los recursos. Estos registros incluyen registros del sistema de eventos de Windows, registros de Azure Storage, registros de auditoría de Key Vault, y registros de firewall y acceso a Application Gateway. Todos los registros de diagnóstico se escriben en una cuenta de almacenamiento de Azure centralizada y cifrada para su archivado. El usuario puede configurar la retención hasta 730 días para cumplir los requisitos de retención específicos de una organización.

**Registros de Azure Monitor**: Estos registros se consolidan en [registros de Azure Monitor](https://azure.microsoft.com/services/log-analytics/) para procesamiento, almacenamiento e informes de panel. Una vez recopilados, se organizan en tablas independientes para cada tipo, lo que permite que todos los datos se puedan analizar conjuntamente con independencia de su origen. Además, Azure Security Center se integra con los registros de Azure Monitor permite a los clientes usar consultas de Kusto para acceder a sus datos de eventos de seguridad y la combinamos con datos de otros servicios.

El siguiente Azure [soluciones de supervisión](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) se incluyen como parte de esta arquitectura:
-   [Active Directory Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): la solución Active Directory Health Check evalúa el riesgo y el estado de los entornos de servidor a intervalos regulares y proporciona una lista prioritaria de recomendaciones específicas para la infraestructura de servidor implementada.
- [SQL Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): la solución SQL Health Check evalúa el riesgo y el estado de los entornos de servidor a intervalos regulares y proporciona a los clientes una lista prioritaria de recomendaciones específicas para la infraestructura de servidor implementada.
- [Agent Health](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): la solución Agent Health notifica el número de agentes implementados y su distribución geográfica, así como el número de agentes que no responden y el de agentes que envían datos operativos.
-   [Activity Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): la solución Activity Log Analytics ayuda a los clientes a analizar los registros de actividad de todas las suscripciones de Azure para un cliente.

**Azure Automation**: [Azure Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) almacena, ejecuta y administra runbooks. En esta solución, los runbooks ayudan a recopilar registros de Azure SQL Database. La solución [Change Tracking](https://docs.microsoft.com/azure/automation/automation-change-tracking) de Automation permite a los clientes identificar fácilmente los cambios en el entorno.

**Azure Monitor**: [Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) ayuda a los usuarios a realizar un seguimiento del rendimiento, mantener la seguridad e identificar tendencias y permite a las organizaciones auditar, crear alertas y archivar datos, incluido el seguimiento de las llamadas a API en sus recursos de Azure.

## <a name="threat-model"></a>Modelo de amenazas

El diagrama de flujo de datos de esta arquitectura de referencia está disponible para su [descarga](https://aka.ms/uknhs-analytics-tm) y se encuentra a continuación. El modelo puede ayudar a los clientes a comprender los puntos de riesgo potencial de la infraestructura del sistema al realizar modificaciones.

![Análisis del modelo de amenazas del Sistema sanitario nacional del Reino Unido](images/uknhs-analytics-threat-model.png?raw=true "Analytics for UK NHS threat model")

## <a name="compliance-documentation"></a>Documentación de cumplimiento

[Azure Security and Compliance Blueprint: matriz de responsabilidades del cliente del Sistema sanitario nacional del Reino Unido](https://aka.ms/uknhs-crm) indica todos los principios de seguridad que requiere este sistema. En esta matriz se detalla si la implementación de cada principio es responsabilidad de Microsoft, del cliente o compartida.

[Azure Security and Compliance Blueprint: matriz de implementación de análisis de datos para el Sistema sanitario nacional del Reino Unido](https://aka.ms/uknhs-analytics-cim) proporciona información sobre qué requisitos del Sistema sanitario nacional del Reino Unido aborda la arquitectura de análisis de datos, incluida una descripción detallada de cómo la implementación cumple los requisitos de cada principio cubierto.


## <a name="guidance-and-recommendations"></a>Instrucciones y recomendaciones

### <a name="vpn-and-expressroute"></a>VPN y ExpressRoute

Debe configurarse un túnel VPN seguro o [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) para establecer una conexión segura a los recursos implementados como parte de esta arquitectura de referencia de análisis de datos. Al configurar una VPN o ExpressRoute adecuadamente, los clientes pueden agregar una capa de protección para los datos en tránsito.

Mediante la implementación de un túnel VPN seguro con Azure, se puede crear una conexión privada virtual entre una red local y una red virtual de Azure. Esta conexión tiene lugar a través de Internet y permite a los clientes colocar con seguridad la información en un &quot;túnel&quot; dentro de un vínculo cifrado entre la red del cliente y Azure. La VPN de sitio a sitio es una tecnología segura y madura que empresas de todos los tamaños han implementado durante décadas. Se utiliza [el modo de túnel IPsec](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) en esta opción como mecanismo de cifrado.

Dado que el tráfico dentro del túnel VPN atraviesa Internet con una VPN de sitio a sitio, Microsoft ofrece otra opción de conexión aún más segura. Azure ExpressRoute es un vínculo de WAN dedicado entre Azure y una ubicación local o un proveedor de hospedaje de Exchange. Como las conexiones de ExpressRoute no se realizan a través de una conexión a Internet, ofrecen más confiabilidad, más velocidad, menor latencia y mayor seguridad que las conexiones a Internet normales. Además, dado que se trata de una conexión directa del proveedor de telecomunicaciones del cliente, los datos no viajan a través de Internet y, por lo tanto, no están expuestos a ella.

Hay [disponibles](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) procedimientos recomendados para implementar una red híbrida segura que extienda una red local a Azure.

### <a name="extract-transform-load-process"></a>Proceso de extracción, transformación y carga

[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) puede cargar datos en Azure SQL Database sin necesidad de una herramienta independiente para extraer, transformar, cargar o importar. PolyBase permite el acceso a los datos mediante consultas T-SQL. Con PolyBase se puede usar la inteligencia empresarial y la pila de análisis de Microsoft, así como herramientas de terceros compatibles con SQL Server.

### <a name="azure-active-directory-setup"></a>Configuración de Azure Active Directory
[Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) es esencial para la administración de la implementación y la concesión de acceso a las personas que interactúan con el entorno. Se puede integrar una instancia de Windows Server Active Directory con Azure Active Directory en [cuatro clics](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Los clientes también pueden enlazar la infraestructura de Active Directory implementada (controladores de dominio) con una instancia de Azure Active Directory existente al convertir la infraestructura de Active Directory implementada en un subdominio de un bosque de Azure Active Directory.

## <a name="disclaimer"></a>Renuncia de responsabilidades

 - Este documento es meramente informativo. MICROSOFT NO EFECTÚA NINGÚN TIPO DE GARANTÍA, YA SEA EXPRESA, IMPLÍCITA O ESTATUTARIA, SOBRE LA INFORMACIÓN INCLUIDA EN EL PRESENTE DOCUMENTO. Este documento se proporciona "tal cual". La información y las opiniones expresadas en este documento, como las direcciones URL y otras referencias a sitios web de Internet, pueden cambiar sin previo aviso. Los clientes que lean este documento se atienen a las consecuencias de usarlo.
 - Este documento no proporciona a los clientes ningún derecho legal sobre ninguna propiedad intelectual de ningún producto o solución de Microsoft.
 - Los clientes pueden copiar y usar este documento con fines internos y de referencia.
 - Algunas de las recomendaciones de este documento pueden provocar un aumento del uso de datos, de la red o de los recursos de procesos en Azure, lo que podría incrementar los costos de las licencias o las suscripciones de Azure.
 - Esta arquitectura está diseñada para servir como base para que los clientes puedan ajustarse a sus requisitos específicos y no debe usarse tal cual en un entorno de producción.
 - Este documento se ha desarrollado como referencia y no debe usarse para definir todos los medios por los que un cliente puede cumplir normas y requisitos de cumplimiento específicos. Los clientes deben buscar apoyo legal de su organización sobre las implementaciones de cliente aprobadas.
