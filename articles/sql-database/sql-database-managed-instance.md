---
title: Introducción a Instancia administrada de Azure SQL Database | Microsoft Docs
description: En este tema se describe una instancia de Instancia administrada de Azure SQL Database y se explica cómo funciona y cómo difiere de una base de datos única de Azure SQL Database.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab
manager: craigg
ms.date: 10/30/2018
ms.openlocfilehash: e89245a946848e46f3c7c502b6cd0e8017327e07
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/31/2018
ms.locfileid: "50419901"
---
# <a name="use-sql-database-managed-instance-with-virtual-networks-and-near-100-compatibility"></a>Usar Instancia administrada de SQL Database con redes virtuales y casi un 100 % de compatibilidad

La Instancia administrada de Azure SQL Database es un nuevo modelo de implementación de Azure SQL Database que proporciona casi un 100 % de compatibilidad con el motor de base de datos local más reciente de SQL Server (Enterprise Edition), una implementación nativa de [red virtual (VNet)](../virtual-network/virtual-networks-overview.md) que permite solucionar problemas de seguridad habituales y un [modelo de negocio](https://azure.microsoft.com/pricing/details/sql-database/) favorable para los clientes locales de SQL Server. Instancia administrada permite a los clientes existentes de SQL Server migrar mediante lift-and-shift sus aplicaciones locales a la nube con cambios mínimos en la aplicación y la base de datos. Al mismo tiempo, la Instancia administrada conserva todas las funcionalidades de PaaS (aplicación de revisiones y actualizaciones de versión automáticas, [copia de seguridad automática](sql-database-automated-backups.md), [alta disponibilidad](sql-database-high-availability.md)), lo cual reduce drásticamente la sobrecarga de administración y el costo total de propiedad.

> [!IMPORTANT]
> Para obtener una lista de regiones en las que Instancia administrada está actualmente disponible, consulte [Regiones admitidas](sql-database-managed-instance-resource-limits.md#supported-regions).

En el diagrama siguiente se describen las características principales de Instancia administrada:

![principales características](./media/sql-database-managed-instance/key-features.png)

La Instancia administrada de Azure SQL Database está diseñada para aquellos clientes que quieran migrar un gran número de aplicaciones desde un entorno local o de IaaS, compilado automáticamente o que hayan proporcionado los fabricantes de software independientes, a un entorno en la nube de PaaS totalmente administrado, con el menor esfuerzo de migración posible. Mediante el uso totalmente automatizado de [Azure Database Migration Service (DMS)](../dms/tutorial-sql-server-to-managed-instance.md#create-an-azure-database-migration-service-instance), los clientes pueden migrar mediante lift-and-shift sus instancias locales de SQL Server a una Instancia administrada que ofrezca compatibilidad con SQL Server local y aislamiento completo de las instancias de cliente con compatibilidad nativa con redes virtuales.  Con Software Assurance, puede intercambiar sus licencias existentes para obtener descuentos en Instancia administrada de SQL Database mediante la [Ventaja híbrida de Azure para SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).  Instancia administrada de SQL Database es el mejor destino de migración en la nube para instancias de SQL Server que requieren alta seguridad y una completa superficie de programación.

En cuanto a la disponibilidad general, Instancia administrada quiere proporcionar una compatibilidad de área expuesta cercana al 100% con la última versión de SQL Server local mediante un plan de lanzamiento gradual.

Para decidir entre la base de datos única de Azure SQL Database, la instancia administrada de Azure SQL Database y la IaaS de SQL Server hospedada en la máquina virtual, consulte [cómo elegir la versión correcta de SQL Server en Azure Cloud](sql-database-paas-vs-sql-server-iaas.md).

## <a name="key-features-and-capabilities"></a>Funcionalidades y características clave

La Instancia administrada de Azure SQL Database combina las mejores características que están disponibles tanto en Azure SQL Database como en el Motor de base de datos de SQL Server.

> [!IMPORTANT]
> Una instancia administrada se ejecuta con todas las características de la versión más reciente de SQL Server, incluidas las operaciones en línea, las correcciones automáticas del plan y otras mejoras de rendimiento empresarial. Se explica una comparación de las características disponibles en [Comparación de funciones: Azure SQL Database frente a SQL Server](sql-database-features.md).

| **Ventajas de PaaS** | **Continuidad del negocio** |
| --- | --- |
|No hay compras de hardware ni administración <br>Ninguna sobrecarga de administración a la hora de administrar la infraestructura subyacente <br>Aprovisionamiento y escalado de servicio rápidos <br>Aplicación de revisiones y actualización de versiones automatizadas <br>Integración con otros servicios de datos de PaaS |Acuerdo de Nivel de Servicio de tiempo de actividad del 99,99 %  <br>[Alta disponibilidad](sql-database-high-availability.md) integrada <br>Datos protegidos con [copias de seguridad automatizadas](sql-database-automated-backups.md) <br>Período de retención de copia de seguridad configurable por el cliente (fijado en 7 días en la versión preliminar pública) <br>[Copias de seguridad](https://docs.microsoft.com/sql/t-sql/statements/backup-transact-sql?view=azuresqldb-mi-current) que haya iniciado el usuario <br>Funcionalidad de [restauración de base de datos a un momento dado](sql-database-recovery-using-backups.md#point-in-time-restore) |
|**Seguridad y cumplimiento normativo** | **Administración**|
|Entorno aislado ([integración con red virtual](sql-database-managed-instance-vnet-configuration.md), servicio de inquilino único y procesos y almacenamiento dedicados) <br>[Cifrado de datos transparente (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql)<br>[Autenticación de Azure AD](sql-database-aad-authentication.md), compatibilidad con el inicio de sesión único <br>Se adhiere a las mismas normas de cumplimiento estándar que Azure SQL Database <br>[Auditoría de SQL](sql-database-managed-instance-auditing.md) <br>[Detección de amenazas](sql-database-managed-instance-threat-detection.md) |API de Azure Resource Manager para automatizar el aprovisionamiento y escalado del servicio <br>Funcionalidad de Azure Portal para el aprovisionamiento y escalado manuales del servicio <br>Data Migration Service

Las características principales de Instancia administrada se muestran en la tabla siguiente:

|Característica | DESCRIPCIÓN|
|---|---|
| Versión de SQL Server | Motor de base de datos de SQL Server (versión estable más reciente) |
| Administración de copias de seguridad automatizadas | SÍ |
| Supervisión y métricas integradas de instancias y bases de datos | SÍ |
| Aplicación automática de revisiones de software | SÍ |
| Características del motor de base de datos más recientes | SÍ |
| Número de archivos de datos (ROWS) por base de datos | Múltiple |
| Número de archivos de registro (LOG) por base de datos | 1 |
| Redes virtuales: implementación de Azure Resource Manager | SÍ |
| Redes virtuales: modelo de implementación clásica de Azure | Sin  |
| Soporte técnico del portal | SÍ|
| Integration Service (SSIS) integrado | No: SSIS es una parte de [PaaS de Azure Data Factory](https://docs.microsoft.com/azure/data-factory/tutorial-deploy-ssis-packages-azure) |
| Analysis Service (SSAS) integrado | No: SSAS es un servicio [PaaS](https://docs.microsoft.com/azure/analysis-services/analysis-services-overview) independiente. |
| Reporting Service (SSRS) integrado | No: use Power BI o IaaS de SSRS |
|||

## <a name="vcore-based-purchasing-model"></a>Modelo de compra basado en núcleos virtuales

El [modelo de compra basado en núcleos virtuales](sql-database-service-tiers-vcore.md) de Instancia administrada le ofrece flexibilidad, control, transparencia y facilidad para trasladar sus necesidades de carga de trabajo del entorno local a la nube. Este modelo le permite cambiar los recursos de proceso, memoria y almacenamiento en función de las necesidades de la carga de trabajo. El modelo de núcleos virtuales también permite disfrutar de hasta un 30 % de ahorro con la [Ventaja híbrida de Azure para SQL Server](../virtual-machines/windows/hybrid-use-benefit-licensing.md).

En el modelo de núcleos virtuales, puede elegir entre distintas generaciones de hardware.

- Las CPU lógicas de **4.ª generación** se basan en procesadores Intel E5-2673 v3 (Haswell) de 2,4 GHz, SSD conectado, núcleos físicos, 7 GB de RAM por núcleo y tamaños de proceso entre 8 y 24 núcleos virtuales.
- Las CPU lógicas de **5.ª generación** se basan en procesadores Intel E5-2673 v4 (Broadwell) de 2,3 GHz, SSD eNVM rápido, núcleo lógico con Hyper-Threading, 7 GB de RAM por núcleo y tamaños de proceso entre 8 y 80 núcleos.

Encuentre más información sobre la diferencia entre las generaciones de hardware en el artículo sobre [límites de recursos de Instancia administrada](sql-database-managed-instance-resource-limits.md#hardware-generation-characteristics).

## <a name="managed-instance-service-tiers"></a>Niveles de servicio de Instancia administrada

Instancia administrada está disponible en dos niveles de servicio:

- **Uso general**: diseñada para aplicaciones con rendimiento y requisitos de latencia de E/S comunes.
- **Crítico para la empresa (versión preliminar)**: diseñada para aplicaciones con requisitos de latencia baja de E/S y un impacto mínimo de operaciones de mantenimiento subyacentes en la carga de trabajo.

Ambos niveles de servicio garantizan una disponibilidad del 99,99 % y le permiten seleccionar el tamaño de almacenamiento y la capacidad de proceso de forma independiente. Para obtener más información acerca de la arquitectura de alta disponibilidad de Azure SQL Database, consulte [Alta disponibilidad y Azure SQL Database](sql-database-high-availability.md).

> [!IMPORTANT]
> En la versión preliminar pública no se admite el cambio de nivel de servicio de uso General a Crítico para la empresa o viceversa. Si desea migrar bases de datos a una instancia en un nivel de servicio diferente, puede crear la nueva instancia, restaurar las bases de datos con la restauración a un momento dado de la instancia original y después quitar la instancia original si ya no es necesaria. Sin embargo, puede escalar horizontal o verticalmente el número de núcleos virtuales y el almacenamiento dentro de un nivel de servicio sin tiempo de inactividad.

### <a name="general-purpose-service-tier"></a>Nivel de servicio de uso general

La siguiente lista describe las principales características del nivel de servicio de uso general:

- Diseño de la mayoría de las aplicaciones empresariales con requisitos típicos de alto rendimiento
- Premium Storage de alto rendimiento (8 TB)
- [Alta disponibilidad](sql-database-high-availability.md#basic-standard-and-general-purpose-service-tier-availability) integrada basada en los servicios de alta confiabilidad Azure Premium Storage y [Azure Service Fabric](../service-fabric/service-fabric-overview.md)

Para obtener más información, consulte [Storage layer in General purpose tier](https://medium.com/azure-sqldb-managed-instance/file-layout-in-general-purpose-azure-sql-managed-instance-cf21fff9c76c) (Capa de almacenamiento en el nivel de uso general) y [Storage performance best practices and considerations for Azure SQL DB Managed Instance (General Purpose)](https://blogs.msdn.microsoft.com/sqlcat/2018/07/20/storage-performance-best-practices-and-considerations-for-azure-sql-db-managed-instance-general-purpose/) [Procedimientos recomendados y consideraciones de rendimiento de almacenamiento de Instancia administrada de Azure SQL DB (uso general)].

Encuentre más información sobre la diferencia entre los niveles de servicio en el artículo sobre los [límites de recursos de Instancia administrada](sql-database-managed-instance-resource-limits.md#service-tier-characteristics).

### <a name="business-critical-service-tier-preview"></a>Nivel de servicio Crítico para la empresa (versión preliminar)

El nivel de servicio Crítico para la empresa se ha creado para las aplicaciones con elevados requisitos de E/S. Ofrece la máxima resistencia a errores mediante varias réplicas aisladas.

La siguiente lista describe las principales características del nivel de servicio Crítico para la empresa:

- Diseñado para aplicaciones empresariales con mayor rendimiento y requisitos de alta disponibilidad
- Incluye almacenamiento SSD extremadamente rápido (hasta 1 TB en Gen 4 y hasta 4 TB en Gen 5)
- [Alta disponibilidad](sql-database-high-availability.md#premium-and-business-critical-service-tier-availability) integrada basada en los [Grupos de disponibilidad AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server) y [Azure Service Fabric](../service-fabric/service-fabric-overview.md).
- [Réplica de base de datos de solo lectura](sql-database-read-scale-out.md) adicional integrada que se puede usar para informes y otras cargas de trabajo de solo lectura.
- [OLTP en memoria](sql-database-in-memory.md), que se puede usar para la carga de trabajo con requisitos de alto rendimiento.  

> [!IMPORTANT]
> El nivel de servicio **Crítico para la empresa** está en versión preliminar.

Encuentre más información sobre la diferencia entre los niveles de servicio en el artículo sobre los [límites de recursos de Instancia administrada](sql-database-managed-instance-resource-limits.md#service-tier-characteristics).

## <a name="advanced-security-and-compliance"></a>Conformidad y seguridad avanzada

La Instancia administrada de Azure SQL Database combina características de seguridad avanzadas que proporciona Azure Cloud y el motor de base de datos de SQL Server.

### <a name="managed-instance-security-isolation-in-azure-cloud"></a>Aislamiento de seguridad de la Instancia administrada en Azure Cloud

Instancia administrada proporciona un aislamiento de seguridad adicional a partir de otros inquilinos en la nube de Azure. El aislamiento de seguridad incluye:

- [Implementación nativa de redes virtuales](sql-database-managed-instance-vnet-configuration.md) y conectividad al entorno local mediante Azure ExpressRoute o VPN Gateway
- El punto de conexión de SQL se expone solo mediante una dirección IP privada, lo que permite una conectividad segura desde una nube privada de Azure o redes híbridas
- Inquilino único con infraestructura subyacente dedicada (proceso, almacenamiento)

El diagrama siguiente describe distintas opciones de conectividad para las aplicaciones:

![alta disponibilidad](./media/sql-database-managed-instance/application-deployment-topologies.png)  

Para obtener más información sobre la integración con redes virtuales y las exigencias de la directiva de redes en el nivel de subred, consulte [Configurar una red virtual para Instancia administrada de Azure SQL Database](sql-database-managed-instance-vnet-configuration.md) y [Conectar la aplicación a Instancia administrada de Azure SQL Database](sql-database-managed-instance-connect-app.md).

> [!IMPORTANT]
> Coloque varias instancias administradas en la misma subred, dondequiera que lo permitan sus requisitos de seguridad, ya que le proporcionará ventajas adicionales. Colocar instancias en la misma subred simplificará significativamente el mantenimiento de la infraestructura de red y reducirá el tiempo de aprovisionamiento de instancia, ya que el aprovisionamiento de larga duración está asociado con el costo de implementar la primera instancia administrada en una subred.

### <a name="azure-sql-database-security-features"></a>Características de seguridad de Azure SQL Database

Azure SQL Database proporciona un conjunto de características de seguridad avanzadas que se pueden usar para proteger sus datos.

- La [auditoría de Instancia administrada](sql-database-managed-instance-auditing.md) realiza un seguimiento de los eventos de bases de datos y los escribe en un archivo de registro de auditoría de su cuenta de Azure Storage. La auditoría puede ayudarle a mantener el cumplimiento de normativas, comprender la actividad de las bases de datos y conocer las discrepancias y anomalías que pueden indicar problemas en el negocio o infracciones de seguridad sospechosas.
- Cifrado de datos en movimiento: la Instancia administrada protege los datos gracias al cifrado de datos en movimiento mediante la Seguridad de la capa de transporte. Además de la seguridad de la capa de transporte, Instancia administrada de SQL Database ofrece la protección de la información confidencial en tránsito, en reposo y durante el procesamiento de consultas con [Always Encrypted](/sql/relational-databases/security/encryption/always-encrypted-database-engine). Always Encrypted es una tecnología totalmente novedosa en el sector que ofrece una seguridad de datos sin parangón frente a las infracciones que implican el robo de datos críticos. Por ejemplo, con Always Encrypted, los números de las tarjetas de crédito siempre se almacenan cifrados en la base de datos, incluso durante el procesamiento de las consultas, lo que permite que solo los descifren personal autorizado o las aplicaciones que los necesitan para procesar los datos en el lugar en que se van a usar.
- La [detección de amenazas](sql-database-managed-instance-threat-detection.md) complementa la [auditoría de Instancia administrada](sql-database-managed-instance-auditing.md), ya que proporciona una capa adicional de inteligencia de seguridad integrada en el servicio que detecta intentos inusuales y potencialmente dañinos para obtener acceso a las bases de datos o vulnerarlas. Recibirá alertas de actividades sospechosas, vulnerabilidades potenciales y ataques por inyección de código SQL, así como patrones anómalos de acceso a bases de datos. Las alertas de Detección de amenazas pueden verse en [Azure Security Center](https://azure.microsoft.com/services/security-center/) y proporcionar detalles de actividad sospechosa y la acción recomendada sobre cómo investigar y mitigar la amenaza.  
- El [enmascaramiento dinámico de datos](/sql/relational-databases/security/dynamic-data-masking) limita la exposición de información confidencial mediante su enmascaramiento a los usuarios sin privilegios. El enmascaramiento dinámico de datos ayuda a impedir el acceso no autorizado a datos confidenciales permitiéndole designar la cantidad de los datos confidenciales que se revelarán con un impacto mínimo en el nivel de aplicación. Es una característica de seguridad basada en directivas que oculta los datos confidenciales en el conjunto de resultados de una consulta sobre los campos designados de la base de datos, aunque que los datos de la base de datos no cambian.
- La [seguridad de nivel de fila](/sql/relational-databases/security/row-level-security) le permite controlar el acceso a las filas de una tabla de base de datos en función de las características del usuario que ejecuta una consulta (por ejemplo, la pertenencia a un grupo o el contexto de ejecución). La seguridad de nivel de fila (RLS) simplifica el diseño y la codificación de la seguridad de la aplicación. RLS permite implementar restricciones de acceso a filas de datos. Por ejemplo, garantiza que los empleados únicamente puedan acceder a aquellas filas de datos necesarios para su departamento o restringe el acceso solo a los datos relevantes.
- El [Cifrado de datos transparente (TDE)](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) cifra los archivos de datos de la Instancia administrada de Azure SQL Database, lo que se conoce como cifrado de datos en reposo. TDE realiza el cifrado y descifrado de E/S en tiempo real de los archivos de datos y de registro. El cifrado usa una clave de cifrado de base de datos (DEK), que se almacena en el registro de arranque de la base de datos de disponibilidad durante la recuperación. Puede proteger todas las bases de datos en una instancia administrada con cifrado de datos transparente. TDE es la probada tecnología de cifrado en reposo de SQL que requieren muchos estándares de cumplimiento normativo para proteger contra el robo de soportes de almacenamiento. Durante la versión preliminar pública, se admite el modelo de administración automática de claves (realizada por la plataforma PaaS).

Se admite la migración de una base de datos cifrada a Instancia administrada de SQL a través de Azure Database Migration Service (DMS) o la restauración nativa. Si va a migrar la base de datos cifrada mediante la restauración nativa, la migración del certificado TDE existente desde la VM con SQL Server o el SQL Server local a Instancia administrada es un paso necesario. Para obtener más información sobre los diversos métodos de migración, vea [Migración de una instancia de SQL Server a Instancia administrada de Azure SQL Database](sql-database-managed-instance-migrate.md).

## <a name="azure-active-directory-integration"></a>Integración de Azure Active Directory

La Instancia administrada de Azure SQL Database admite los inicios de sesión tradicionales del motor de base de datos de SQL Server, y los inicios de sesión integrados con Azure Active Directory (AAD). Los inicios de sesión de AAD son una versión en la nube de Azure de los inicios de sesión de la base de datos de Windows que suele usar en su entorno local.

### <a name="azure-active-directory-integration-and-multi-factor-authentication"></a>Integración de Azure Active Directory y autenticación multifactor

La Instancia administrada le permite administrar de forma centralizada las identidades del usuario de base de datos y otros servicios de Microsoft con la [integración de Azure Active Directory](sql-database-aad-authentication.md). Esta funcionalidad simplifica la administración de permisos y mejora la seguridad. Azure Active Directory admite la [autenticación multifactor](sql-database-ssms-mfa-authentication-configure.md) (MFA) para aumentar la seguridad tanto de los datos como de las aplicaciones y admite un proceso de inicio de sesión único.

### <a name="authentication"></a>Autenticación

La autenticación de la Instancia administrada le indica la forma en que los usuarios prueban su identidad al conectarse a la base de datos. SQL Database admite dos tipos de autenticación:  

- **Autenticación de SQL**:

  Este método de autenticación usa un nombre de usuario y una contraseña.
- **Autenticación con Azure Active Directory**:

  Este método de autenticación usa las identidades administradas por Azure Active Directory y es compatible con dominios administrados e integrados. Use la autenticación de Active Directory (seguridad integrada) [siempre que sea posible](https://docs.microsoft.com/sql/relational-databases/security/choose-an-authentication-mode).

### <a name="authorization"></a>Autorización

La autorización indica las acciones que pueden llevar a cabo los usuarios en Azure SQL Database, algo que controlan los permisos de nivel de objeto y las pertenencias a roles de bases de datos de la cuenta de usuario. Instancia administrada tiene las mismas funcionalidades de autorización que SQL Server 2017.

## <a name="database-migration"></a>Migración de bases de datos

Instancia administrada está diseñado para escenarios de usuario con migración masiva de bases de datos desde implementaciones locales o de base de datos de IaaS. Instancia administrada admite varias opciones de migración de base de datos:

### <a name="backup-and-restore"></a>Copia de seguridad y restauración  

El enfoque de migración aprovecha las copias de seguridad de SQL en Azure Blob Storage. Las copias de seguridad almacenadas en Azure Storage Blob se pueden restaurar directamente en la Instancia administrada mediante el [comando T-SQL RESTORE](https://docs.microsoft.com/sql/t-sql/statements/restore-statements-transact-sql?view=azuresqldb-mi-current).

- Para ver una guía rápida que muestra cómo restaurar el archivo de copia de seguridad de base de datos de Wide World Importers - Standard, consulte [Restore a backup file to a Managed Instance](sql-database-managed-instance-get-started-restore.md) (Restauración de un archivo de copia de seguridad a una instancia administrada). En esta guía rápida se muestra cómo cargar un archivo de copia de seguridad en el almacenamiento de blobs de Azure y protegerlo mediante una clave de Firma de acceso compartido (SAS).
- Para obtener información sobre la restauración desde una URL, vea [Restauración de copias de seguridad nativas desde la dirección URL](sql-database-managed-instance-migrate.md#native-restore-from-url).

> [!IMPORTANT]
> Solo se pueden restaurar las copias de seguridad de una instancia administrada en otra instancia administrada. No puede restaurarse en una instancia local de SQL Server o en una base de datos única o agrupada del servidor lógico de Azure SQL Server.

### <a name="data-migration-service"></a>Data Migration Service

Azure Database Migration Service es un servicio totalmente administrado diseñado para permitir migraciones completas desde varios orígenes de base de datos hasta las plataformas de datos de Azure con un tiempo de inactividad mínimo. Este servicio simplifica las tareas necesarias para mover las bases de datos existentes de SQL Server y de terceros a Azure. Las opciones de implementación incluyen Azure SQL Database, Instancia administrada y SQL Server en una máquina virtual de Azure en versión preliminar pública. Consulte cómo [migrar su base de datos local a Instancia administrada mediante DMS](https://aka.ms/migratetoMIusingDMS).

## <a name="sql-features-supported"></a>Características de SQL admitidas

El objetivo de Instancia administrada es proporcionar una compatibilidad de área expuesta cercana al 100% con un entorno local de SQL Server, con una incorporación gradual hasta alcanzar la disponibilidad general del servicio. Para obtener una lista de características y comparaciones, consulte [Comparación de SQL Database](sql-database-features.md) y, para obtener una lista de diferencias de T-SQL comparando las Instancias administradas y SQL Server, consulte [Diferencias de T-SQL entre LAS Instancias administradas y SQL Server](sql-database-managed-instance-transact-sql-information.md).

Instancia administrada admite versiones anteriores hasta bases de datos de SQL 2008. Se admite la migración directa desde los servidores de bases de datos de SQL 2005, el nivel de compatibilidad para las bases de datos de SQL 2005 migradas se ha actualizado a SQL 2008.
  
El siguiente diagrama describe la compatibilidad de área expuesta en Instancia administrada:  

![migración](./media/sql-database-managed-instance/migration.png)

### <a name="key-differences-between-sql-server-on-premises-and-managed-instance"></a>Diferencias principales entre SQL Server local e Instancia administrada

Instancia administrada tiene la ventaja de estar siempre actualizada en la nube, lo cual significa que algunas características de la instancia local de SQL Server pueden ser obsoletas, estar retiradas o presentar alternativas. Hay casos específicos en los que las herramientas deben reconocer que una característica determinada funciona de forma ligeramente diferente o que el servicio no se está ejecutando en un entorno que no controla totalmente:

- La alta disponibilidad está integrada y preconfigurada con tecnología similar a [Grupos de disponibilidad AlwaysOn](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/always-on-availability-groups-sql-server).
- Copias de seguridad automatizadas y restauración a un momento dado. El cliente puede iniciar copias de seguridad `copy-only` que no interfieren con la cadena de copias de seguridad automáticas.
- Instancia administrada no permite especificar las rutas de acceso físicas completas por lo que todos los escenarios correspondientes se admiten de manera diferente: RESTORE DB no admite WITH MOVE, CREATE DB no permite rutas de acceso físicas, BULK INSERT funciona solo con Blobs de Azure, etc.
- Instancia administrada admite la [autenticación de Azure AD](sql-database-aad-authentication.md) como alternativa en la nube a la autenticación de Windows.
- Instancia administrada administra automáticamente el grupo de archivos XTP y los archivos de bases de datos que contienen objetos de OLTP en memoria
- Instancia administrada es compatible con SQL Server Integration Services (SSIS) y puede hospedar el catálogo de SSIS (SSISDB) que almacena paquetes SSIS, pero que se ejecuta en una instancia de Integration Runtime para la integración de SSIS en Azure en Azure Data Factory; consulte [Creación de una instancia de Integration Runtime de SSIS de Azure en Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime). Para comparar las características de SSIS en SQL Database y la Instancia administrada, consulte [Compare SQL Database logical server and Managed Instance](../data-factory/create-azure-ssis-integration-runtime.md#compare-sql-database-logical-server-and-sql-database-managed-instance) (Comparación entre el servidor lógico de SQL Database y la Instancia administrada).

### <a name="managed-instance-administration-features"></a>Características de administración de Instancia administrada

Instancia administrada permite al administrador del sistema centrarse en lo más importante para el negocio. Muchas actividades del administrador del sistema o DBA no son necesarias, o son sencillas. Por ejemplo, la instalación y aplicación de revisiones de SO o RDBMS, las opciones dinámicas de cambio de tamaño y configuración de instancias, las copias de seguridad, la [replicación de bases de datos](replication-with-sql-database-managed-instance.md) (incluidas las bases de datos del sistema), la configuración de la alta disponibilidad y los flujos de datos de configuración del estado y [supervisión del rendimiento](../log-analytics/log-analytics-azure-sql.md).

> [!IMPORTANT]
> Para obtener una lista de las características compatibles, parcialmente compatibles o no compatibles, consulte [Características de SQL Database](sql-database-features.md). Para obtener una lista de diferencias de T-SQL en Instancia administrada en comparación con SQL Server, consulte [Managed Instance T-SQL Differences from SQL Server](sql-database-managed-instance-transact-sql-information.md) (Diferencias de T-SQL entre Instancia administrada y SQL Server)

### <a name="how-to-programmatically-identify-a-managed-instance"></a>Cómo identificar mediante programación una instancia administrada

En la tabla siguiente se muestran varias propiedades, accesibles mediante Transact SQL, que puede utilizar para detectar si la aplicación funciona con Instancia administrada y recuperar propiedades importantes.

|Propiedad|Valor|Comentario|
|---|---|---|
|`@@VERSION`|Microsoft SQL Azure (RTM) - 12.0.2000.8 2018-03-07 Copyright (C) 2018 Microsoft Corporation.|Este valor es igual que en SQL Database.|
|`SERVERPROPERTY ('Edition')`|SQL Azure|Este valor es igual que en SQL Database.|
|`SERVERPROPERTY('EngineEdition')`|8|Este valor identifica la instancia administrada de forma única.|
|`@@SERVERNAME`, `SERVERPROPERTY ('ServerName')`|Nombre DNS completo de la instancia en el siguiente formato:`<instanceName>`.`<dnsPrefix>`.database.windows.net, donde `<instanceName>` es el nombre que proporciona el cliente, mientras que `<dnsPrefix>` es la parte que se genera automáticamente del nombre, lo cual permite garantizar la exclusividad del nombre DNS global (por ejemplo, "wcus17662feb9ce98").|Por ejemplo: my-managed-instance.wcus17662feb9ce98.database.windows.net|

## <a name="next-steps"></a>Pasos siguientes

- Para obtener información sobre cómo crear su primera instancia administrada, consulte la [guía de inicio rápido](sql-database-managed-instance-get-started.md).
- Para obtener una lista de características y una comparación, consulte [Características comunes de SQL](sql-database-features.md).
- Para más información acerca de la configuración de redes virtuales, consulte [Configuración de una red virtual de instancia administrada](sql-database-managed-instance-vnet-configuration.md).
- Para ver una guía rápida en la que se crea una instancia administrada y se restaura una base de datos desde un archivo de copia de seguridad, consulte [Creación de una instancia administrada](sql-database-managed-instance-get-started.md).
- Para consultar un tutorial con Azure Database Migration Service (DMS) para la migración, consulte [Migración a Instancia administrada con DMS](../dms/tutorial-sql-server-to-managed-instance.md).
- Para una supervisión avanzada del rendimiento de la base de datos de Instancia administrada con inteligencia de solución de problemas integrada, consulte [Supervisión de instancias de Azure SQL Database con Azure SQL Analytics ](../log-analytics/log-analytics-azure-sql.md) 
- Para obtener información de precios, vea [Precios de Instancia administrada de SQL Database](https://azure.microsoft.com/pricing/details/sql-database/managed/).
