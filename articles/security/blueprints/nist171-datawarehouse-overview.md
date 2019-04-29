---
title: 'Azure Security and Compliance Blueprint: almacenamiento de datos para NIST SP 800-171'
description: 'Azure Security and Compliance Blueprint: almacenamiento de datos para NIST SP 800-171'
services: security
author: jomolesk
ms.assetid: dbc9cafe-115d-4965-b0d4-fcf235a064c8
ms.service: security
ms.topic: article
ms.date: 07/31/2018
ms.author: jomolesk
ms.openlocfilehash: a1850ecfbb21eb9495bb0e6de362dc8dee3026a2
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60609586"
---
# <a name="azure-security-and-compliance-blueprint---data-warehouse-for-nist-sp-800-171"></a>Azure Security and Compliance Blueprint: almacenamiento de datos para NIST SP 800-171

## <a name="overview"></a>Información general
En [NIST Special Publication 800-171](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-171.pdf) se proporcionan instrucciones para proteger la información sin clasificar controlada (CUI) que reside en sistemas de información y organizaciones no federales. NIST SP 800-171 establece 14 familias de requisitos de seguridad para proteger la confidencialidad de CUI.

Este documento de Azure Security and Compliance Blueprint proporciona instrucciones para ayudar a los clientes a implementar una arquitectura de almacenamiento de datos de Azure que implementa un subconjunto de controles de NIST SP 800-171. Esta solución muestra formas en que los clientes pueden cumplir requisitos de cumplimiento y de seguridad específicos. También sirve como base para que los clientes creen y configuren sus propias soluciones de almacenamiento de datos en Azure.

Esta arquitectura de referencia, la guía de implementación asociada y el modelo de amenazas están diseñados como base para que los clientes puedan adaptarse a sus requisitos específicos. No deben usarse tal cual en un entorno de producción. Los clientes son responsables de llevar a cabo las evaluaciones de seguridad y cumplimiento adecuadas de todas las soluciones creadas con esta arquitectura. Los requisitos pueden variar en función de las especificaciones de implementación de cada cliente.

## <a name="architecture-diagram-and-components"></a>Componentes y diagrama de la arquitectura
Esta solución proporciona una arquitectura de referencia que implementa un almacenamiento de datos de alto rendimiento y seguro en la nube. En esta arquitectura hay dos niveles de datos independientes. En un nivel, los datos se importan y se almacenan (definitiva o provisionalmente) en un entorno en clúster de SQL. El otro nivel está pensado para SQL Data Warehouse. Con este nivel, los datos se cargan con una herramienta de extracción, transformación y carga (ETL) (por ejemplo, las consultas T-SQL de [PolyBase](https://docs.microsoft.com/azure/sql-data-warehouse/load-data-from-azure-blob-storage-using-polybase)) para su procesamiento. Una vez que los datos están almacenados en SQL Data Warehouse, se pueden llevar a cabo análisis a gran escala.

Azure ofrece una variedad de servicios de informes y análisis para el cliente. Esta solución incluye SQL Server Reporting Services para la rápida creación de informes desde SQL Data Warehouse. Todo el tráfico SQL se cifra con SSL mediante la inclusión de certificados autofirmados. Como procedimiento recomendado, le recomendamos que use una entidad de certificación de confianza para mejorar la seguridad.

Azure SQL Data Warehouse almacena los datos en tablas relacionales con almacenamiento en columnas. Este formato reduce considerablemente los costos de almacenamiento de datos, a la vez que mejora el rendimiento de las consultas. Según los requisitos de uso, los recursos de proceso de SQL Data Warehouse se pueden escalar y reducir verticalmente o apagarse completamente si no hay ningún proceso activo que necesite recursos de proceso.

Un equilibrador de carga de SQL Server administra el tráfico SQL para garantizar un rendimiento alto. Todas las máquinas virtuales de esta arquitectura de referencia se implementan en un conjunto de disponibilidad con instancias de SQL Server configuradas en un grupo de disponibilidad AlwaysOn. Esta configuración proporciona funcionalidades de recuperación ante desastres y alta disponibilidad.

Esta arquitectura de referencia de almacenamiento de datos también incluye un nivel de Active Directory para la administración de recursos dentro de la arquitectura. La subred de Active Directory permite la fácil adopción en una gran estructura de bosques de Active Directory. De este modo, el entorno puede funcionar de manera continua, aunque no se pueda acceder al bosque más grande. Todas las máquinas virtuales están unidas a un dominio en la capa de Active Directory. Usan directivas de grupo de Active Directory para aplicar configuraciones de seguridad y cumplimiento a nivel del sistema operativo.

La solución utiliza cuentas de Azure Storage que los clientes pueden configurar para que usen Storage Service Encryption para preservar la confidencialidad de los datos en reposo. Azure almacena tres copias de los datos en un centro de datos seleccionado de un cliente para proporcionar resistencia. El almacenamiento con redundancia geográfica garantiza que los datos se repliquen en un centro de datos secundario situado a cientos de kilómetros de distancia y que se vuelvan a almacenar como tres copias dentro de ese centro de datos. Esta disposición evita que un evento adverso en el centro de datos principal del cliente acabe con una pérdida de datos.

Para mejorar la seguridad, todos los recursos de esta solución se administran como un grupo de recursos mediante Azure Resource Manager. El control de acceso basado en roles de Azure Active Directory (Azure AD) se usa para controlar el acceso a los recursos implementados. Estos recursos incluyen claves de cliente en Azure Key Vault. El mantenimiento del sistema se supervisa mediante Azure Security Center y Azure Monitor. Los clientes configuran ambos servicios de supervisión para capturar registros. El mantenimiento del sistema se muestra en un panel único fácil de usar.

Una máquina virtual actúa como host de tipo bastión de administración. Proporciona una conexión segura para que los administradores accedan a los recursos implementados. Los datos se cargan en el área de ensayo a través de este host de tipo bastión de administración. *Le recomendamos que configure una conexión VPN o de Azure ExpressRoute para la administración y la importación de datos de en la subred de la arquitectura de referencia.*

![Diagrama de la arquitectura de referencia de Almacenamiento de datos para NIST SP 800-171](images/nist171-dw-architecture.png "Data Warehouse for NIST SP 800-171 reference architecture diagram")

Esta solución usa los siguientes servicios de Azure. Para obtener más información, consulte la sección sobre la [arquitectura de implementación](#deployment-architecture).

- Conjuntos de disponibilidad
    - Controladores de dominio de Active Directory
    - Testigo y nodos de clúster de SQL Server
- Azure Active Directory
- Azure Data Catalog
- Azure Key Vault
- Monitor de Azure (registros)
- Azure Security Center
- Azure Load Balancer
- Azure Storage
- Azure Virtual Machines
    - (1) Host de tipo bastión
    - (2) Controlador de dominio de Active Directory
    - (2) Nodo de clúster de SQL Server
    - (1) Testigo de SQL Server
- Azure Virtual Network
    - (1)/16 redes
    - (4)/24 redes
    - (4) Grupos de seguridad de red
- Almacén de Recovery Services
- SQL Data Warehouse
- SQL Server Reporting Services

## <a name="deployment-architecture"></a>Arquitectura de implementación
En la siguiente sección se detallan los elementos de desarrollo e implementación.

**Azure SQL Data Warehouse**: [SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-overview-what-is) es un almacén de datos empresarial que aprovecha las ventajas de procesamiento paralelo masivo para ejecutar rápidamente consultas complejas en petabytes de datos. Los usuarios pueden usar consultas T-SQL de PolyBase simples para importar macrodatos a SQL Data Warehouse y usar la potencia del procesamiento paralelo masivo para ejecutar análisis de alto rendimiento.

**SQL Server Reporting Services**: [SQL Server Reporting Services](https://docs.microsoft.com/sql/reporting-services/report-data/sql-azure-connection-type-ssrs) proporciona la rápida creación de informes con tablas, gráficos, mapas, medidores, matrices y más para SQL Data Warehouse.

**Azure Data Catalog**: [Data Catalog](https://docs.microsoft.com/azure/data-catalog/data-catalog-what-is-data-catalog) facilita que los usuarios que administran los datos puedan detectar y comprender los orígenes de datos. En los orígenes de datos comunes se pueden registrar, etiquetar y buscar datos. Los datos permanecen en la ubicación existente, pero se agrega una copia de sus metadatos a Data Catalog. Se incluye una referencia a la ubicación de los orígenes de datos. Los metadatos se indexan para que sea fácil detectar los orígenes de datos mediante una búsqueda. La indexación también facilita su comprensión para los usuarios que los detecten.

**Host de tipo bastión**: El host bastión es el único punto de entrada que los usuarios pueden usar para tener acceso a los recursos implementados en este entorno. El host de tipo bastión proporciona una conexión segura a los recursos implementados al permitir solo el tráfico remoto desde las direcciones IP públicas de una lista segura. Para permitir el tráfico del escritorio remoto, el origen del tráfico se debe definir en el grupo de seguridad de red.

Esta solución crea una máquina virtual como host de tipo bastión unido mediante dominio con las siguientes configuraciones:
-   [Extensión antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware).
-   [Extensión de Diagnósticos de Azure](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-extensions-diagnostics-template).
-   [Azure Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) con Key Vault.
-   Una [directiva de apagado automático](https://azure.microsoft.com/blog/announcing-auto-shutdown-for-vms-using-azure-resource-manager/) para reducir el consumo de recursos de la máquina virtual cuando no esté en uso.
-   [Credential Guard de Windows Defender](https://docs.microsoft.com/windows/access-protection/credential-guard/credential-guard) está habilitado para que las credenciales y otros secretos se ejecuten en un entorno protegido aislado del sistema operativo en ejecución.

### <a name="virtual-network"></a>Virtual network
La arquitectura de referencia define una red virtual privada con un espacio de direcciones de 10.0.0.0/16.

**Grupos de seguridad de red**: los [grupos de seguridad de red](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) (NSG) contienen listas de control de acceso que permiten o deniegan el tráfico en una red virtual. Los NSG pueden usarse para proteger el tráfico en una subred o a nivel de máquina virtual individual. Los grupos de seguridad de red siguientes ya existen:
  - Un grupo de seguridad de red para la capa de datos (clústeres de SQL Server, testigo de SQL Server y equilibrador de carga de SQL)
  - Un grupo de seguridad de red para el host de tipo bastión
  - Un grupo de seguridad de red para Active Directory
  - Un grupo de seguridad de red para SQL Server Reporting Services

Cada uno de los grupos de seguridad de red tiene puertos y protocolos específicos abiertos para que la solución pueda funcionar de forma segura y correcta. Además, las siguientes opciones de configuración están habilitadas para cada NSG:
  - Los [eventos y registros de diagnóstico](https://docs.microsoft.com/azure/virtual-network/virtual-network-nsg-manage-log) están habilitados y se almacenan en la cuenta de almacenamiento.
  - Registros de Azure Monitor se conecta a la [diagnósticos del NSG](https://github.com/krnese/AzureDeploy/blob/master/AzureMgmt/AzureMonitor/nsgWithDiagnostics.json).

**Subredes**: Cada subred se asocia a su NSG correspondiente.

### <a name="data-at-rest"></a>Datos en reposo
La arquitectura protege los datos en reposo aplicando varias medidas. Entre estas medidas está el cifrado y la auditoría de base de datos.

**Azure Storage**: para cumplir los requisitos de los datos en reposo cifrados, todo [Storage](https://azure.microsoft.com/services/storage/) usa [Storage Service Encryption](https://docs.microsoft.com/azure/storage/storage-service-encryption). Esta característica ayuda a proteger los datos en cumplimiento de los compromisos de seguridad de la organización y los requisitos de cumplimiento.

**Azure Disk Encryption**: [Disk Encryption](https://docs.microsoft.com/azure/security/azure-security-disk-encryption) usa la característica BitLocker de Windows para proporcionar el cifrado del volumen de discos de datos y sistema operativo. La solución se integra con Key Vault para ayudar a controlar y administrar las claves de cifrado del disco.

**Azure SQL Database**: la instancia de SQL Database usa las siguientes medidas de seguridad de base de datos:
-   La [autenticación y autorización de Active Directory](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) permiten la administración de identidades de usuarios de bases de datos y otros servicios de Microsoft en una ubicación central.
-   La [auditoría de SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-auditing-get-started) realiza un seguimiento de eventos de bases de datos y los escribe en un registro de auditoría de una cuenta de Azure Storage.
-   SQL Database está configurado para usar el [cifrado de datos transparente](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql). Lleva a cabo un cifrado y descifrado en tiempo real de la base de datos, copias de seguridad asociadas y archivos de registro de transacciones para proteger la información en reposo. El cifrado de datos transparente garantiza que los datos almacenados no han estado sujetos a acceso no autorizado.
-   Las [reglas de firewall](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) impiden el acceso a los servidores de bases de datos hasta que se otorgan los permisos adecuados. Asimismo, otorgan acceso a las bases de datos según la dirección IP de origen de cada solicitud.
-   [Detección de amenazas de SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection-get-started) permite detectar y responder a posibles amenazas cuando se producen. Proporciona alertas de seguridad si se producen actividades sospechosas en la base de datos, aparecen vulnerabilidades potenciales y ataques por inyección de código SQL, o si existen patrones anómalos de acceso a la base de datos.
-   Las [columnas cifradas](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault) garantizan que los datos confidenciales nunca aparezcan como texto no cifrado en el sistema de base de datos. Una vez habilitado el cifrado de datos, solo las aplicaciones cliente o los servidores de aplicaciones con acceso a las claves pueden acceder a los datos de texto no cifrado.
- Las [propiedades extendidas](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-addextendedproperty-transact-sql) se pueden usar para interrumpir el procesamiento de los interesados. Los usuarios pueden agregar propiedades personalizadas a los objetos de base de datos. También pueden etiquetar los datos como "Suspendido" para admitir una lógica de aplicación para evitar el procesamiento de datos financieros asociados.
- La [seguridad de nivel de fila](https://docs.microsoft.com/sql/relational-databases/security/row-level-security) permite a los usuarios definir directivas para restringir el acceso a los datos a fin de interrumpir su procesamiento.
- El [enmascaramiento dinámico de datos de SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-dynamic-data-masking-get-started) limita la exposición de los datos confidenciales al enmascarar los datos para usuarios o aplicaciones sin privilegios. Puede detectar de forma automática datos potencialmente confidenciales y sugerir las máscaras adecuadas que se pueden aplicar. El enmascaramiento dinámico de datos permite reducir el acceso para que los datos confidenciales no salgan de la base de datos por un acceso no autorizado. *Los clientes son responsables de ajustar la configuración para cumplir con el esquema de la base de datos.*

### <a name="identity-management"></a>Administración de identidades
Las siguientes tecnologías proporcionan funcionalidades de administración del acceso a datos confidenciales en el entorno de Azure:
-   [Azure AD](https://azure.microsoft.com/services/active-directory/) es el servicio de directorio y de administración de identidades multiinquilino basado en la nube de Microsoft. Todos los usuarios de esta solución se crean en Azure AD, incluidos los que acceden a la base de datos SQL.
-   La autenticación para acceder a la aplicación se efectúa mediante Azure AD. Para obtener más información, consulte cómo se [integran las aplicaciones con Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications). El cifrado de las columnas de la base de datos también usa Azure AD para autenticar la aplicación en SQL Database. Para más información, consulte cómo [proteger los datos confidenciales en SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
-   Los administradores pueden usar [Azure RBAC](https://docs.microsoft.com/azure/active-directory/role-based-access-control-configure) para definir permisos de acceso específicos. Con Azure RBAC, pueden conceder únicamente el grado de acceso que los usuarios necesiten para llevar a cabo sus tareas. En lugar de dar a cada usuario acceso ilimitado a los recursos de Azure, los administradores pueden permitir solo ciertas acciones para acceder a datos y recursos. El acceso a la suscripción está limitado al administrador de la suscripción.
- Los clientes pueden usar [Azure Active Directory Privileged Identity Management](https://docs.microsoft.com/azure/active-directory/active-directory-privileged-identity-management-getting-started) para minimizar el número de usuarios con acceso a determinada información, como los datos. Los administradores pueden usar Azure AD Privileged Identity Management para detectar, restringir y supervisar las identidades con privilegios y su acceso a los recursos. Esta funcionalidad también se puede utilizar para aplicar un acceso administrativo a petición, Just-in-Time, cuando sea necesario.
- [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) detecta posibles vulnerabilidades que afecten a las identidades de una organización. Configura respuestas automatizadas a acciones sospechosas detectadas que están relacionadas con las identidades de una organización. También investiga incidentes sospechosos para tomar las medidas adecuadas para resolverlos.

### <a name="security"></a>Seguridad
**Administración de secretos**: la solución utiliza [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) para la administración de claves y secretos. Key Vault ayuda a proteger claves criptográficas y secretos usados por servicios y aplicaciones en la nube. Las siguientes funcionalidades de Key Vault ayudan a los clientes a proteger los datos:
- Se configuran directivas de acceso avanzadas según las necesidades.
- Se definen directivas de acceso a Key Vault con los permisos mínimos requeridos para las claves y los secretos.
- Todas las claves y los secretos en Key Vault tienen fechas de expiración.
- Todas las claves de Key Vault están protegidas por módulos de seguridad de hardware especializados. El tipo de clave es una clave RSA de 2048 bits protegida por un módulo de seguridad de hardware.
- Se otorgan los permisos necesarios mínimos a todos los usuarios e identidades mediante RBAC.
- Los registros de diagnóstico de Key Vault están habilitados con un período de retención de al menos 365 días.
- Las operaciones criptográficas permitidas para las claves están restringidas únicamente a las requeridas.

**Administración de revisiones**: Implementado como parte de esta arquitectura de referencia de las máquinas virtuales de Windows se configuran de forma predeterminada para recibir actualizaciones automáticas de Windows Update Service. Esta solución también incluye el servicio [Azure Automation](https://docs.microsoft.com/azure/automation/automation-intro), con el que se pueden crear implementaciones actualizadas para aplicar revisiones a las máquinas virtuales cuando sea necesario.

**Protección contra malware**: [Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) para las VM que proporciona funcionalidad de protección en tiempo real que ayuda a identifica y quita virus, spyware y otro software malintencionado. Los clientes pueden configurar alertas para que se generen si un software malintencionado o no deseado conocido intenta instalarse o ejecutarse en máquinas virtuales protegidas.

**Azure Security Center**: con [Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro), los clientes pueden aplicar y administrar de forma centralizada las directivas de seguridad entre cargas de trabajo, limitar la exposición a amenazas y detectar ataques y responder a estos. Security Center también accede a las configuraciones existentes de los servicios de Azure para proporcionar recomendaciones de configuración y servicio que ayuden a mejorar la postura de seguridad y a proteger los datos.

Security Center usa una variedad de funcionalidades de detección para alertar a los clientes de posibles ataques contra sus entornos. Estas alertas contienen información útil acerca de lo que desencadenó la alerta, los recursos objetivo y el origen del ataque. Security Center cuenta con un conjunto de [alertas de seguridad predefinidas](https://docs.microsoft.com/azure/security-center/security-center-alerts-type) que se desencadenan cuando tiene lugar una amenaza o actividad sospechosa. Los clientes pueden utilizar [reglas de alertas personalizadas](https://docs.microsoft.com/azure/security-center/security-center-custom-alert) para definir nuevas alertas de seguridad basadas en los datos ya recopilados en el entorno.

Security Center proporciona incidentes y alertas de seguridad prioritarias. Security Center facilita a los clientes la detección y la resolución de posibles problemas de seguridad. Para cada amenaza detectada se genera un [informe de inteligencia de amenazas](https://docs.microsoft.com/azure/security-center/security-center-threat-report). Los equipos de respuesta a los incidentes pueden usar los informes para investigar y solucionar las amenazas.

Esta arquitectura de referencia también usa la capacidad de [evaluación de vulnerabilidad](https://docs.microsoft.com/azure/security-center/security-center-vulnerability-assessment-recommendations) en Security Center. Una vez configurado, un agente de asociado (por ejemplo, Qualys) informa de los datos de vulnerabilidades a la plataforma de administración del asociado. Este, a su vez, proporciona datos de supervisión del estado y las vulnerabilidades a Security Center. Los clientes pueden usar esta información para identificar rápidamente las máquinas virtuales vulnerables.

### <a name="business-continuity"></a>Continuidad del negocio
**Alta disponibilidad**: Las cargas de trabajo de servidor se agrupan en una [conjunto de disponibilidad](https://docs.microsoft.com/azure/virtual-machines/virtual-machines-windows-manage-availability?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) para ayudar a garantizar una alta disponibilidad de máquinas virtuales en Azure. Durante un evento de mantenimiento planeado o no planeado hay al menos una máquina virtual disponible para cumplir el 99,95% del Acuerdo de Nivel de Servicio de Azure.

**Almacén de Recovery Services**: El [almacén de Recovery Services](https://docs.microsoft.com/azure/backup/backup-azure-recovery-services-vault-overview) aloja datos de copia de seguridad y protege las configuraciones de máquinas virtuales en esta arquitectura. Con los almacenes de Recovery Services, los clientes pueden restaurar archivos y carpetas desde una máquina virtual de IaaS sin tener que restaurar toda la máquina virtual. Este proceso acelera los tiempos de restauración.

### <a name="logging-and-auditing"></a>Registro y auditoría

Los servicios de Azure proporcionan un registro completo de la actividad de usuario y del sistema, así como de mantenimiento del sistema:
- **Registros de actividad**: los [registros de actividad](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) proporcionan información detallada sobre las operaciones realizadas en los recursos de la suscripción. Los registros de actividad pueden ayudar a determinar el iniciador de una operación, el momento en que se produce y el estado.
- **Registros de diagnóstico**: los [registros de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) son todos los registros emitidos por todos los recursos. Estos registros incluyen registros del sistema de eventos de Windows, registros de Azure Storage, registros de auditoría de Key Vault, y registros de firewall y acceso a Azure Application Gateway. Todos los registros de diagnóstico se escriben en una cuenta de almacenamiento de Azure centralizada y cifrada para su archivado. Los usuarios pueden configurar el período de retención (de hasta 730 días) para satisfacer sus requisitos específicos.

**Registros de Azure Monitor**: Estos registros se consolidan en [registros de Azure Monitor](https://azure.microsoft.com/services/log-analytics/) para procesamiento, almacenamiento e informes de panel. Una vez recopilados, los datos se organizan en tablas independientes por tipo de datos en las áreas de trabajo de Log Analytics. De este modo, todos los datos se pueden analizar juntos, independientemente de su fuente original. Security Center se integra con los registros de Azure Monitor. Los clientes pueden usar las consultas de Kusto para acceder a sus datos de eventos de seguridad y la combinamos con datos de otros servicios.

El siguiente Azure [soluciones de supervisión](https://docs.microsoft.com/azure/log-analytics/log-analytics-add-solutions) se incluyen como parte de esta arquitectura:
-   [Active Directory Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-ad-assessment): la solución Active Directory Health Check evalúa el riesgo y el estado de los entornos de servidor de forma periódica. Proporciona una lista priorizada de recomendaciones específicas para la infraestructura de servidor implementada.
- [SQL Assessment](https://docs.microsoft.com/azure/log-analytics/log-analytics-sql-assessment): la solución SQL Health Check evalúa el riesgo y el estado de los entornos de servidor de forma periódica. Proporciona a los clientes una lista priorizada de recomendaciones específicas de la infraestructura de servidor implementada.
- [Agent Health](https://docs.microsoft.com/azure/operations-management-suite/oms-solution-agenthealth): la solución Agent Health informa de cuántos agentes se implementan y su distribución geográfica. También informa de cuántos agentes no responden y del número de agentes que envían datos operativos.
-   [Activity Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-activity): la solución Activity Log Analytics ayuda a los clientes a analizar los registros de actividad de todas las suscripciones de Azure para un cliente.

**Azure Automation**: [Automation](https://docs.microsoft.com/azure/automation/automation-hybrid-runbook-worker) almacena, ejecuta y administra runbooks. En esta solución, los runbooks ayudan a recopilar registros de SQL Database. Los clientes pueden usar la solución [Change Tracking](https://docs.microsoft.com/azure/automation/automation-change-tracking) de Automation para identificar fácilmente los cambios en el entorno.

**Azure Monitor**: [Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) ayuda a los usuarios a efectuar un seguimiento del rendimiento, a mantener la seguridad y a identificar tendencias. Las organizaciones lo pueden usar para auditar, crear alertas y archivar datos. También pueden registrar llamadas API en sus recursos de Azure.

## <a name="threat-model"></a>Modelo de amenazas

El diagrama de flujo de datos de esta arquitectura de referencia está disponible para su [descarga](https://aka.ms/nist171-dw-tm) y se encuentra aquí. El modelo puede ayudar a los clientes a comprender los puntos de riesgo potencial de la infraestructura del sistema al efectuar modificaciones.

![Modelo de amenazas de almacenamiento de datos para NIST SP 800-171](images/nist171-dw-threat-model.png "Data Warehouse for NIST SP 800-171 threat model")

## <a name="compliance-documentation"></a>Documentación de cumplimiento
[Azure Security and Compliance Blueprint: matriz de responsabilidades del cliente para NIST SP 800-171](https://aka.ms/nist171-crm) muestra todos los controles de seguridad que exige NIST SP 800-171. En esta matriz se detalla si la implementación de cada objetivo es responsabilidad de Microsoft, del cliente o de ambos.

En [Azure Security and Compliance Blueprint: matriz de implementación de control de almacenamiento de datos para NIST SP 800-171](https://aka.ms/nist171-dw-cim) se ofrece información sobre qué controles de NIST SP 800-171 se tratan en la arquitectura de almacenamiento de datos. Incluye descripciones detalladas de cómo la implementación cumple los requisitos de cada control cubierto.

## <a name="guidance-and-recommendations"></a>Instrucciones y recomendaciones

### <a name="vpn-and-expressroute"></a>VPN y ExpressRoute
Se debe configurar [ExpressRoute](https://docs.microsoft.com/azure/expressroute/expressroute-introduction) o un túnel VPN seguro para establecer una conexión segura a los recursos implementados como parte de esta arquitectura de referencia de almacenamiento de datos. Al configurar una VPN o ExpressRoute adecuadamente, los clientes pueden agregar una capa de protección para los datos en tránsito.

Mediante la implementación de un túnel VPN seguro con Azure, se puede crear una conexión privada virtual entre una red local y una red virtual de Azure. Esta conexión se efectúa a través de Internet. Los clientes pueden usar esta conexión para colocar con seguridad la información en un "túnel" dentro de un vínculo cifrado entre la red del cliente y Azure. La VPN de sitio a sitio es una tecnología segura y madura que empresas de todos los tamaños han implementado durante décadas. Se utiliza [el modo de túnel IPsec](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc786385(v=ws.10)) en esta opción como mecanismo de cifrado.

Dado que el tráfico dentro del túnel VPN atraviesa Internet con una VPN de sitio a sitio, Microsoft ofrece otra opción de conexión aún más segura. ExpressRoute es un vínculo de WAN dedicado entre Azure y una ubicación local o un proveedor de hospedaje de Exchange. Las conexiones de ExpressRoute se conectan directamente al proveedor de telecomunicaciones del cliente. Como resultado, los datos no viajan a través de Internet y no están expuestos. Estas conexiones ofrecen más confiabilidad, velocidades más altas, menores latencias y mayor seguridad que las conexiones habituales.

Hay [disponibles](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/secure-vnet-hybrid) procedimientos recomendados para implementar una red híbrida segura que extienda una red local a Azure.

### <a name="extract-transform-load-process"></a>Proceso de extracción, transformación y carga
[PolyBase](https://docs.microsoft.com/sql/relational-databases/polybase/polybase-guide) puede cargar datos en SQL Data Warehouse sin necesidad de una herramienta ETL o de importación independiente. PolyBase permite el acceso a los datos mediante consultas T-SQL. Con PolyBase se puede usar la inteligencia empresarial y la pila de análisis de Microsoft, así como herramientas de terceros compatibles con SQL Server.

### <a name="azure-ad-setup"></a>Configuración de Azure AD
[Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-whatis) es esencial para la administración de la implementación y la concesión de acceso al personal que interactúa con el entorno. Active Directory local se puede integrar con Azure AD en [cuatro clics](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-express). Los clientes también pueden vincular la infraestructura implementada de Active Directory (controladores de dominio) con Azure AD. Para ello, debe convertir la infraestructura implementada de Active Directory en un subdominio de un bosque de Azure AD.

### <a name="optional-services"></a>Servicios opcionales
Azure ofrece una variedad de servicios para ayudar con el almacenamiento y el proceso de almacenamiento provisional de datos con formato y sin formato. Los servicios siguientes se pueden agregar a esta arquitectura de referencia en función de los requisitos del cliente:
-   [Azure Data Factory](https://docs.microsoft.com/azure/data-factory/introduction) es un servicio en la nube administrado creado para complejos proyectos híbridos de ETL (extracción, transformación y carga) y de integración de datos. Data Factory tiene capacidades para hacer un seguimiento de los datos y localizarlos. Las herramientas de supervisión y visualización identifican el momento en que llegan los datos y su procedencia. Los clientes pueden crear y programar flujos de trabajo controlados por datos (denominados canalizaciones) que ingieren datos de distintos almacenes de datos. Pueden usar las canalizaciones para ingerir datos desde orígenes internos y externos. Después, pueden procesar y transformar los datos para la salida a almacenes de datos, como SQL Data Warehouse.
- Los clientes pueden almacenar de manera provisional los datos no estructurados en [Azure Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) para capturar datos de cualquier tamaño, tipo y velocidad de ingesta en un lugar para llevar a cabo análisis exploratorios y operativos. Azure Data Lake tiene funcionalidades que permiten la extracción y conversión de los datos. Data Lake Store es compatible con la mayoría de los componentes de código abierto del ecosistema de Hadoop. También se integra perfectamente con otros servicios de Azure, como SQL Data Warehouse.

## <a name="disclaimer"></a>Renuncia de responsabilidades

 - Este documento es meramente informativo. MICROSOFT NO EFECTÚA NINGÚN TIPO DE GARANTÍA, YA SEA EXPRESA, IMPLÍCITA O ESTATUTARIA, SOBRE LA INFORMACIÓN INCLUIDA EN EL PRESENTE DOCUMENTO. Este documento se proporciona "tal cual". La información y las opiniones expresadas en este documento, como las direcciones URL y otras referencias a sitios web de Internet, pueden cambiar sin previo aviso. Los clientes que lean este documento se atienen a las consecuencias de usarlo.
 - Este documento no proporciona a los clientes ningún derecho legal sobre ninguna propiedad intelectual de ningún producto o solución de Microsoft.
 - Los clientes pueden copiar y usar este documento con fines internos y de referencia.
 - Algunas de las recomendaciones de este documento pueden provocar un aumento del uso de datos, de la red o de los recursos de procesos en Azure, lo que podría incrementar los costos de las licencias o las suscripciones de Azure.
 - Esta arquitectura está diseñada para servir como base para que los clientes puedan ajustarse a sus requisitos específicos y no debe usarse tal cual en un entorno de producción.
 - Este documento se ha desarrollado como referencia y no debe usarse para definir todos los medios por los que un cliente puede cumplir normas y requisitos de cumplimiento específicos. Los clientes deben buscar apoyo legal de su organización sobre las implementaciones de cliente aprobadas.
