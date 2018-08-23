---
title: 'Azure Security and Compliance Blueprint: hospedaje de aplicaciones web de PaaS para cargas de trabajo de la clasificación OFFICIAL (Reino Unido)'
description: 'Azure Security and Compliance Blueprint: hospedaje de aplicaciones web de PaaS para cargas de trabajo de la clasificación OFFICIAL (Reino Unido)'
services: security
author: jomolesk
ms.assetid: 446105ad-a863-44f5-a964-6ead1dac4787
ms.service: security
ms.topic: article
ms.date: 07/13/2018
ms.author: jomolesk
ms.openlocfilehash: f465a6e6cb0642f64670a8d2727c939f91bad134
ms.sourcegitcommit: a62cbb539c056fe9fcd5108d0b63487bd149d5c3
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "42617256"
---
# <a name="azure-security-and-compliance-blueprint-paas-web-application-hosting-for-uk-official-workloads"></a>Azure Security and Compliance Blueprint: hospedaje de aplicaciones web de PaaS para cargas de trabajo de la clasificación OFFICIAL (Reino Unido)

## <a name="azure-security-and-compliance-blueprints"></a>Planos de seguridad y cumplimiento de Azure

Los planos técnicos de Azure constan de documentos de guía y plantillas de automatización que implementan arquitecturas basadas en la nube para ofrecer soluciones para escenarios que tienen requisitos de acreditación o de cumplimiento. Los planos técnicos de Azure son un conjunto de guías y plantillas de automatización que permiten a los clientes de Microsoft Azure aprovisionar una arquitectura de base que puede ampliarse para cumplir cualquier requisito adicional y acelerar así la entrega de sus objetivos empresariales.

## <a name="overview"></a>Información general

Azure Security and Compliance Blueprint proporciona orientación y scripts de automatización para ofrecer una arquitectura de aplicación web hospedada de [plataforma como servicio (PaaS)](https://azure.microsoft.com/overview/what-is-paas/) de Microsoft Azure adecuada para controlar cargas de trabajo clasificadas como [OFFICIAL (Reino Unido)](https://assets.publishing.service.gov.uk/government/uploads/system/uploads/attachment_data/file/715778/May-2018_Government-Security-Classifications-2.pdf). Esta clasificación de seguridad abarca la mayoría de la información creada o procesada por el sector público. Esto incluye las operaciones y servicios empresariales de rutina, los cuales, en caso de pérdida, robo o publicación en los medios podrían tener consecuencias perjudiciales. El perfil de amenaza típica para la clasificación OFFICIAL es igual a la de una empresa privada que ofrece servicios e información valiosa. La clasificación OFFICIAL (Reino Unido) prevé la necesidad de defender los datos o servicios del gobierno de Reino Unido frente a las amenazas o riesgos causados por atacantes con capacidades y recursos limitados, lo que incluye, pero no se limita a, ciberactivistas, grupos de presión sobre una materia, periodistas de investigación, piratas informáticos individuales competentes y la mayoría de individuos y grupos criminales.

Este plano técnico ha sido revisado por el Centro de Ciberseguridad Nacional (NCSC) de Reino Unido y cumple con los 14 Principios de seguridad en la nube del NCSC.

La arquitectura utiliza componentes de [plataforma como servicio](https://azure.microsoft.com/overview/what-is-paas/) de Azure para entregar un entorno que permite a los clientes evitar los gastos y complejidad de la compra de licencias de software y de la administración de la infraestructura subyacente de la aplicación, middleware, herramientas de desarrollo y otros recursos. Los clientes administran las aplicaciones y los servicios que desarrollan, lo que les permite centrarse en proporcionar valor empresarial, mientras que Microsoft Azure administra los demás recursos de Azure como máquinas virtuales, almacenamiento y redes, apoyándose en la [división de responsabilidad](https://docs.microsoft.com/azure/security/security-paas-deployments#division-of-responsibility) de la administración de la infraestructura en la plataforma Azure. [Azure App Services](https://azure.microsoft.com/services/app-service/) ofrece escalado automático y alta disponibilidad, es compatible con Windows y Linux y permite implementaciones automatizadas desde GitHub, Visual Studio Team Services o cualquier repositorio de Git como servicios predeterminados. Mediante el uso de App Services, los desarrolladores pueden concentrarse en proporcionar valor empresarial sin la sobrecarga de la administración de la infraestructura. Es posible crear nuevas aplicaciones web de Java, PHP, Node.js, Python, HTML o C# o también migrar las aplicaciones web existentes en la nube y en el entorno local a Azure App Service (aunque se requiere diligencia y pruebas exhaustivas para confirmar el rendimiento).

Este plano técnico se centra en el aprovisionamiento de una interfaz basada en web de [plataforma como servicio](https://azure.microsoft.com/overview/what-is-paas/) de base segura para los usuarios públicos y de la organización. El escenario de diseño de este plano técnico considera el uso de servicios basados en web hospedados en Azure, donde un usuario público puede enviar, ver y administrar datos confidenciales de forma segura. También considera que un operador del gobierno o de administración pueda procesar de forma segura la información confidencial que ha enviado el usuario público. Algunos casos de uso para este escenario podrían incluir:

- Un usuario que envía una devolución de impuestos, con un operador del gobierno que procesa el envío.
- Un usuario que solicita un servicio a través de una aplicación basada en web, con un usuario administrativo que valida y ofrece el servicio.
- Un usuario que busca y visualiza información de ayuda de dominio público relativa a un servicio del gobierno.

Mediante el uso de plantillas de [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) y scripts de la CLI de Azure, este plano técnico implementa un entorno que cumple con los 14 [principios de seguridad en la nube](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) del Centro de Ciberseguridad Nacional de Reino Unido (NCSC) y los [controles de seguridad críticos](https://www.cisecurity.org/critical-controls.cfm) del Centro para la Seguridad en Internet (CIS). El NCSC recomienda que los clientes usen sus Principios de seguridad en la nube para evaluar las propiedades de seguridad del servicio y para comprender mejor la división de responsabilidades entre el cliente y el proveedor. Microsoft proporciona información sobre cada uno de estos principios para ayudarle a comprender mejor la división de responsabilidades. Esta arquitectura y las plantillas de Azure Resource Manager correspondientes son compatibles con las notas de producto de Microsoft [14 Cloud Security Controls for UK cloud Using Microsoft Azure](https://gallery.technet.microsoft.com/14-Cloud-Security-Controls-670292c1) (14 controles de seguridad en la nube para la nube de Reino Unido con Microsoft Azure). Esta arquitectura ha sido revisada por el NCSC y cumple con los 14 Principios de seguridad en la nube del NCSC de Reino Unido, lo que permite que las organizaciones del sector público realicen un seguimiento rápido de su capacidad de cumplir con sus obligaciones de cumplimiento mediante servicios basados en la nube de forma global y en el Reino Unido en la nube de Microsoft Azure. Esta plantilla implementa la infraestructura para la carga de trabajo. El código de la aplicación y el software de la capa de datos y la capa de negocio deben ser instalados y configurados por los clientes. Puede encontrar instrucciones detalladas acerca de la implementación [aquí](https://aka.ms/ukofficial-paaswa-repo/).

Este plano técnico es una arquitectura de base. Nuestros clientes pueden usar este plano técnico como base para sus cargas de trabajo basadas en web de clasificación OFFICIAL y expandir las plantillas y los recursos con sus propios requisitos. Este plano técnico se basa en los principios del [plano técnico para aplicaciones web de IaaS de tres niveles de la clasificación OFFICIAL (Reino Unido)](https://aka.ms/ukofficial-iaaswa) para ofrecer a nuestros clientes opciones de implementación de [infraestructura como servicio (IaaS)](https://azure.microsoft.com/overview/what-is-iaas/) y de PaaS para hospedar cargas de trabajo basadas en web.

Para implementar este plano técnico, se requiere una suscripción de Azure. Si no tiene una suscripción de Azure, puede registrarse de forma rápida, fácil y sin cargos: Comience a usar Azure. Haga clic [aquí](https://aka.ms/ukofficial-paaswa-repo/) para ver las instrucciones de implementación.

## <a name="architecture-and-components"></a>Arquitectura y componentes

Este plano técnico ofrece una solución de hospedaje de aplicaciones web en un entorno en la nube de Azure que admite cargas de trabajo de la clasificación OFFICIAL (Reino Unido). La arquitectura proporciona un entorno seguro que aprovecha las funcionalidades de plataforma como servicio de Azure. En el entorno se implementan dos aplicaciones web de App Service (una para los usuarios públicos y otra para los usuarios administrativos) con una capa de aplicación de API para proporcionar los servicios empresariales para el front-end web. Se implementa una instancia de Azure SQL Database como un almacén de datos relacional administrado para la aplicación. La conectividad con estos componentes desde fuera de la plataforma y entre todos estos componentes se cifra mediante TLS 1.2 para asegurar la privacidad de los datos en transporte, con acceso autenticado por Azure Active Directory.

![Diagrama de la arquitectura de referencia del hospedaje de aplicaciones web de PaaS para cargas de trabajo de clasificación OFFICIAL (Reino Unido)](images/ukofficial-paaswa-architecture.png?raw=true "Diagrama de la arquitectura de referencia del hospedaje de aplicaciones web de PaaS para cargas de trabajo de clasificación OFFICIAL (Reino Unido)")

Como parte de la arquitectura de implementación, se implementan también el aprovisionamiento de almacenamiento seguro, supervisión y registro, administración de seguridad unificada, protección contra amenazas avanzada y funcionalidades de administración para garantizar que los clientes tengan todas las herramientas necesarias para proteger y supervisar su entorno para esta solución.

Esta solución usa los siguientes servicios de Azure. Los detalles de la arquitectura de implementación se encuentran en la sección [Arquitectura de implementación](#deployment-architecture).

- Azure Active Directory
- Identidad de servicio administrada
- App Service
- Aplicación web
- Aplicación de API
- Azure DNS
- Key Vault
- Azure Monitor
- Application Insights
- Log Analytics
- Azure Resource Manager
- Azure Security Center
- Azure SQL Database
- Azure Storage

## <a name="deployment-architecture"></a>Arquitectura de implementación

En la siguiente sección se detallan los elementos de desarrollo e implementación.

### <a name="security"></a>Seguridad

#### <a name="identity-and-authentication"></a>Identidad y autenticación

Este plano técnico garantiza que el acceso a los recursos está protegido mediante servicios de administración de identidades y de directorios. Esta arquitectura hace uso completo de la [identidad como perímetro de seguridad](https://docs.microsoft.com/azure/security/security-paas-deployments#identity-as-the-primary-security-perimeter). 

Las siguientes tecnologías proporcionan funcionalidades de administración de identidad en el entorno de Azure:

- [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/) es el directorio multiinquilino basado en la nube y el servicio de administración de identidades de Microsoft. Todos los usuarios de la solución se crearon en Azure Active Directory, incluidos los usuarios que acceden a SQL Database.
- La autenticación para la aplicación web orientada a los operadores y el acceso para la administración de los recursos de Azure se realiza con Azure AD. Para obtener más información, consulte [Integración de aplicaciones con Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).
- El cifrado de las columnas de la base de datos usa Azure AD para autenticar la aplicación en Azure SQL Database. Para obtener más información, consulte [Always Encrypted: protección de datos confidenciales en SQL Database](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).
- La aplicación web orientada a los ciudadanos está configurada para acceso público. Para permitir la creación de cuentas y la autenticación mediante Active Directory o proveedores de identidades de redes sociales, es posible integrar [Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) si es necesario.
- [Azure Active Directory Identity Protection](https://docs.microsoft.com/azure/active-directory/active-directory-identityprotection) detecta posibles vulnerabilidades y cuentas en riesgo, proporciona recomendaciones para mejorar la posición de seguridad de las identidades de la organización, configura respuestas automatizadas si surgen acciones sospechosas relacionadas con esas identidades, investiga incidentes sospechosos y toma las medidas oportunas para resolverlos.
- El [control de acceso basado en rol de Azure (RBAC)](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal) permite administrar al detalle el control de acceso de Azure. El acceso a la suscripción está limitado al administrador de la suscripción y el acceso a Azure Key Vault está restringido únicamente a los usuarios que necesitan acceso de administración de claves.
- Aprovechando las ventajas del [acceso condicional de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal), los clientes pueden aplicar controles de seguridad adicionales sobre el acceso a las aplicaciones o los usuarios del entorno según condiciones específicas, como la ubicación, el dispositivo, el estado y el riesgo de inicio de sesión.
- [Azure DDoS Protection](https://docs.microsoft.com/azure/security/security-paas-deployments#security-advantages-of-a-paas-cloud-service-model) combinado con los procedimientos recomendados de diseño de aplicaciones, proporciona una defensa contra los ataques DDoS, con supervisión continua del tráfico y mitigación en tiempo real de los ataques de nivel de red comunes. Con una arquitectura de PaaS, la protección frente a ataques DDoS en el nivel de plataforma es transparente para el cliente y está integrado en la plataforma, pero es importante tener en cuenta que la responsabilidad del diseño de seguridad de aplicación recae en el cliente.

#### <a name="data-in-transit"></a>Datos en tránsito

Los datos en tránsito desde el exterior y entre los componentes de Azure están protegidos mediante [Transport Layer Security/Secure Sockets Layer (TLS/SSL)](https://www.microsoft.com/TrustCenter/Security/Encryption), que usa criptografía simétrica basada en un secreto compartido para cifrar las comunicaciones cuando viajan por la red. De forma predeterminada, el tráfico de red está protegido con TLS 1.2.

#### <a name="security-and-malware-protection"></a>Seguridad y protección contra malware

[Azure Security Center](https://azure.microsoft.com/services/security-center/) proporciona una visión centralizada del estado de seguridad de todos los recursos de Azure. Podrá comprobar de un solo vistazo si los controles de seguridad adecuados se han implementado y configurado correctamente, y podrá identificar rápidamente cualquier recurso que requiera su atención.

[Azure Advisor](https://docs.microsoft.com/azure/advisor/advisor-overview) es un consultor en la nube personalizado que le ayudará a seguir los procedimientos recomendados para optimizar las implementaciones de Azure. Analiza la configuración de recursos y la telemetría de uso, y recomienda soluciones que pueden ayudar a mejoran la rentabilidad, el rendimiento, la alta disponibilidad y la seguridad de los recursos de Azure.

[Microsoft Antimalware](https://docs.microsoft.com/azure/security/azure-security-antimalware) es una funcionalidad de protección en tiempo real que ayuda a identificar y eliminar virus, spyware y otro software malintencionado. Se instala en la infraestructura subyacente de máquinas virtuales de PaaS de forma predeterminada y se administra mediante el tejido de Azure de forma transparente para el cliente, asegurando los

### <a name="paas-services-in-this-blueprint"></a>Servicios de PaaS de este plano técnico

#### <a name="azure-app-service"></a>Azure App Service

Azure Web Apps proporciona un entorno de hospedaje web completamente administrado para aplicaciones web desarrolladas en Java, PHP, Node.js, Python, HTML y C# sin necesidad de administrar la infraestructura. Ofrece escalado automático y alta disponibilidad, es compatible con Windows y Linux y permite implementaciones automatizadas desde [Visual Studio Team Services](https://azure.microsoft.com/services/visual-studio-team-services/) o cualquier repositorio basado en Git.

App Service es [conforme a las normas ISO, SOC y PCI](https://www.microsoft.com/TrustCenter/) y puede autenticar a los usuarios con [Azure Active Directory](https://docs.microsoft.com/azure/app-service/app-service-mobile-how-to-configure-active-directory-authentication) o con un inicio de sesión social ([Google](https://docs.microsoft.com/azure/app-service/app-service-mobile-how-to-configure-google-authentication), [Facebook](https://docs.microsoft.com/azure/app-service/app-service-mobile-how-to-configure-facebook-authentication), [Twitter](https://docs.microsoft.com/azure/app-service/app-service-mobile-how-to-configure-twitter-authentication) y la [autenticación de Microsoft](https://docs.microsoft.com/azure/app-service/app-service-mobile-how-to-configure-microsoft-authentication).

Los planes Básico, Estándar y Premium están pensados para cargas de trabajo de producción y se ejecutan en instancias de máquinas virtuales dedicadas. Cada instancia puede admitir varias aplicaciones y dominios. App Service también admite [restricciones de direcciones IP](https://docs.microsoft.com/azure/app-service/app-service-ip-restrictions) para proteger el tráfico a direcciones IP de confianza si es necesario y [Managed Service Identity](https://docs.microsoft.com/azure/app-service/app-service-managed-service-identity) para una conexión segura a otros servicios de PaaS como [Key Vault](https://azure.microsoft.com/services/key-vault/) y [Azure SQL Database](https://azure.microsoft.com/services/sql-database/). Cuando se requiere seguridad adicional, el plan Aislado permite hospedar las aplicaciones en un entorno privado y dedicado de Azure y resulta ideal para aquellas aplicaciones que requieren conexiones seguras con la red local o rendimiento y escalabilidad adicional.

Esta plantilla implementa las siguientes características de App Service:

- Nivel del plan de App Service [Estándar](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)
- Varias [ranuras de implementación](https://docs.microsoft.com/azure/app-service/web-sites-staged-publishing) de aplicaciones web: desarrollo, versión preliminar, control de calidad, pruebas de aceptación de usuario y, por supuesto, producción (ranura predeterminada).
- [Managed Service Identity](https://docs.microsoft.com/azure/app-service/app-service-managed-service-identity) para conectarse a [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) (también se puede usar para proporcionar acceso a [Azure SQL Database](https://azure.microsoft.com/services/sql-database/) 
- Integración con [Azure Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-azure-web-apps) para supervisar el rendimiento
- [Registros de diagnóstico](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs) 
- [Alertas](https://docs.microsoft.com/azure/application-insights/app-insights-alerts) de métricas 
- [Azure API Apps](https://azure.microsoft.com/services/app-service/api/) 

#### <a name="azure-sql-database"></a>Azure SQL Database

SQL Database es un servicio administrado de base de datos relacional de uso general de Microsoft Azure que admite estructuras como datos relacionales, JSON, espacial y XML. SQL Database ofrece bases de datos SQL individuales administradas, bases de datos SQL administradas en un [grupo elástico](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool) e [instancias administradas](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) de SQL (en la versión preliminar pública). Proporciona un [rendimiento escalable dinámicamente])https://docs.microsoft.com/azure/sql-database/sql-database-service-tiers) y opciones como [índices de almacén de columnas](https://docs.microsoft.com/sql/relational-databases/indexes/columnstore-indexes-overview) para realizar un análisis analítico extremo y generar informes y [OLTP en memoria](https://docs.microsoft.com/azure/sql-database/sql-database-in-memory) para el procesamiento extremo de transacciones. Microsoft controla perfectamente toda la aplicación de revisiones y de actualizaciones del código base de SQL y desaparece toda la administración de la infraestructura subyacente.

Azure SQL Database en este plano técnico

La instancia de Azure SQL Database usa las siguientes medidas de seguridad de base de datos:

- [Reglas de firewall en el nivel de servidor y de base de datos](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure) o mediante [puntos de conexión de servicio de red virtual](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) con [reglas de red virtual](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview).
- El [Cifrado de datos transparente](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption-azure-sql) ayuda a proteger Azure SQL Database y Azure Data Warehouse frente a la amenaza de actividades malintencionadas. También realiza cifrado y descifrado de la base de datos en tiempo real, copias de seguridad asociadas y archivos de registro de transacciones en reposo sin necesidad de efectuar cambios en la aplicación.
- Con la [autenticación de Azure AD](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication) puede administrar centralmente las identidades de los usuarios de la base de datos y otros servicios de Microsoft en una ubicación central. La administración de identificadores central ofrece una ubicación única para administrar usuarios de base de datos y simplifica la administración de permisos.
- Uso de Azure Active Directory para la administración de bases de datos
- [Registros de auditoría](https://docs.microsoft.com/azure/sql-database/sql-database-auditing) en cuentas de almacenamiento
- [Alertas](https://docs.microsoft.com/azure/application-insights/app-insights-alerts) de métricas para errores en las conexiones de base de datos
- [Detección de amenazas de SQL](https://docs.microsoft.com/azure/sql-database/sql-database-threat-detection).
- [Columnas de Always Encrypted](https://docs.microsoft.com/azure/sql-database/sql-database-always-encrypted-azure-key-vault).

### <a name="azure-storage"></a>Azure Storage

Microsoft [Azure Storage](https://azure.microsoft.com/services/storage/) es un servicio en la nube administrado por Microsoft que proporciona almacenamiento altamente disponible, seguro, duradero, escalable y redundante. Azure Storage consta de Blob Storage, File Storage y Queue Storage.

#### <a name="azure-storage-in-this-blueprint"></a>Azure Storage en este plano técnico

Esta plantilla usa los siguientes componentes de Azure Storage:

- [Cifrado del servicio de almacenamiento](https://docs.microsoft.com/azure/storage/common/storage-service-encryption) 
- Solo se permiten conexiones HTTPS

#### <a name="data-at-rest"></a>Datos en reposo

Con [Storage Service Encryption](https://docs.microsoft.com/azure/storage/common/storage-service-encryption), todos los datos escritos en Azure Storage se cifran mediante cifrado AES de 256 bits, uno de los cifrados de bloques más fuertes disponibles. Puede usar claves de cifrado administradas por Microsoft con SSE o puede utilizar [sus propias claves de cifrado](https://docs.microsoft.com/azure/storage/common/storage-service-encryption-customer-managed-keys).

Las cuentas de almacenamiento se pueden proteger mediante [puntos de conexión de servicio de red virtual](https://docs.microsoft.com/azure/virtual-network/virtual-network-service-endpoints-overview) con [reglas de red virtual](https://docs.microsoft.com/azure/storage/common/storage-network-security).

Encontrará información detallada acerca de cómo proteger Azure Storage en la [Guía de seguridad](https://docs.microsoft.com/azure/storage/common/storage-security-guide).


### <a name="secrets-management"></a>Administración de secretos

#### <a name="azure-key-vault"></a>Azure Key Vault

[Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) se usa para proteger las claves y los secretos de aplicación para asegurarse de que no son accesibles por terceros. Key Vault no está pensado para usarse como almacén para las contraseñas de usuario. Le permite crear múltiples contenedores seguros llamados almacenes. Estos almacenes están respaldados por módulos de seguridad de hardware (HSM). Los almacenes ayudan a reducir las posibilidades de que se produzca una pérdida accidental de información de seguridad centralizando el almacenamiento de los secretos de aplicación. Key Vault también permite controlar y registrar el acceso a todo lo que está almacenado en ellos. Azure Key Vault puede administrar la solicitud y renovación de los certificados de Seguridad de la capa de transporte (TLS), proporcionando las características necesarias para conseguir una eficaz solución de administración del ciclo de vida de los certificados.

#### <a name="azure-key-vault-in-this-blueprint"></a>Azure Key Vault en este plano técnico

- Contiene la clave de acceso de almacenamiento, con acceso de lectura otorgado a la instancia de [Managed Service Identity](https://docs.microsoft.com/azure/app-service/app-service-managed-service-identity) de la aplicación de web orientada al cliente
- Contiene la contraseña de DBA de SQL Server (en un almacén independiente)
- Registro de diagnóstico

### <a name="monitoring-logging-and-audit"></a>Supervisión, registro y auditoría

#### <a name="log-analytics"></a>Log Analytics

[Log Analytics](https://azure.microsoft.com/services/log-analytics/) es un servicio de Azure que le ayuda a recopilar y analizar los datos generados por los recursos en los entornos locales o en la nube.

#### <a name="log-analytics-in-this-blueprint"></a>Log Analytics en este plano técnico

- Evaluación de SQL
- Diagnósticos de Key Vault
- Conexión a Application Insights
- Azure Activity Log

#### <a name="application-insights"></a>Application Insights

[Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) es un servicio de Application Performance Management (APM) extensible para desarrolladores web en varias plataformas. Utilizado para supervisar las aplicaciones web en directo y detectar automáticamente anomalías de rendimiento, analizar el rendimiento, diagnosticar problemas y comprender cómo interactúan los usuarios con la aplicación. Application Insights se puede implementar en plataformas como .NET, Node.js y J2EE, hospedadas en local o en la nube. Se integra con el proceso de DevOps y tiene puntos de conexión a numerosas herramientas de desarrollo.

#### <a name="application-insights-in-this-blueprint"></a>Application Insights en este plano técnico

Esta plantilla usa los siguientes componentes de Application Insights:

- Panel de Application Insights por sitio (operador, cliente y API)

#### <a name="azure-activity-logs"></a>Registros de actividad de Azure

[Azure Activity Log](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs#what-you-can-do-with-the-activity-log) audita los eventos en el plano de control de las suscripciones. Con el Registro de actividades, se pueden determinar los interrogantes “qué, quién y cuándo” de las operaciones de escritura (PUT, POST, DELETE) en los recursos de la suscripción. También puede conocer el estado de la operación y otras propiedades relevantes.

#### <a name="azure-monitor"></a>Azure Monitor

[Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-azure-monitor) habilita la supervisión básica del servicio de Azure al permitir la recopilación de métricas, registros de actividad y registros de diagnóstico. Azure Monitor ofrece registros y métricas de infraestructuras a nivel básico para la mayoría de los servicios de Microsoft Azure.

## <a name="threat-model"></a>Modelo de amenazas

El diagrama de flujo de datos de esta arquitectura de referencia está disponible para su [descarga](https://aka.ms/ukofficial-paaswa-tm) y se encuentra a continuación. El modelo puede ayudar a los clientes a comprender los puntos de riesgo potencial de la infraestructura del sistema al realizar modificaciones.

![Modelo de amenazas del hospedaje de aplicaciones web de PaaS para cargas de trabajo de clasificación OFFICIAL (Reino Unido)](images/ukofficial-paaswa-threat-model.png?raw=true "Modelo de amenazas del hospedaje de aplicaciones web de PaaS para cargas de trabajo de clasificación OFFICIAL (Reino Unido)")

## <a name="ncsc-cloud-security-principles-compliance-documentation"></a>Documentación de cumplimiento de los Principios de seguridad en la nube del NCSC

Crown Commercial Service (una agencia que trabaja para mejorar la actividad comercial y de adquisición del gobierno) ha renovado la clasificación de los servicios en la nube de Microsoft en el ámbito empresarial a G-Cloud v6, que abarca todas sus ofertas en el nivel OFFICIAL. Se pueden encontrar detalles de Azure y G-Cloud en el [resumen de la evaluación de seguridad de G-Cloud de Reino Unido de Azure](https://www.microsoft.com/trustcenter/compliance/uk-g-cloud).

Este plano técnico cumple con los 14 principios de seguridad en la nube que se documentan en los [Principios de seguridad en la nube](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) del NCSC para ayudar a garantizar un entorno que admita cargas de trabajo clasificadas como OFFICIAL (Reino Unido).

La [matriz de responsabilidades del cliente de Azure Security and Compliance Blueprint - OFFICIAL (Reino Unido)](https://aka.ms/ukofficial-crm) (libro de Excel) enumera los 14 principios de seguridad en la nube e indica, para cada principio (o subsección de principio), si la implementación del principio es responsabilidad de Microsoft, del cliente o compartida entre los dos.

La [matriz de implementación de principios de Azure Security and Compliance Blueprint - Aplicaciones web de PaaS para cargas de trabajo OFFICIAL (Reino Unido)](https://aka.ms/ukofficial-paaswa-pim) (libro de Excel) enumera los 14 principios de seguridad en la nube e indica, para cada principio (o subsección de principio), que se designa una responsabilidad del cliente en la matriz de responsabilidades del cliente, 1) si Blueprint implementa el principio y 2) una descripción de cómo cumple la implementación con los requisitos del principio.  

Además, Cloud Security Alliance (CSA) ha publicado la Matriz de control en la nube para ayudar a los clientes en la evaluación de los proveedores en la nube y para identificar las preguntas que deben responder antes de pasar a servicios en la nube. En respuesta, Microsoft Azure ha contestado el cuestionario CSA Consensus Assessment Initiative Questionnaire ([CSA CAIQ](https://www.microsoft.com/TrustCenter/Compliance/CSA)), que describe cómo aborda Microsoft los principios sugeridos.

## <a name="third-party-assessment"></a>Evaluación de terceros

Este plano técnico ha sido revisado por el Centro de Ciberseguridad Nacional (NCSC) de Reino Unido y cumple los 14 Principios de seguridad en la nube del NCSC

Las plantillas de automatización han sido probadas por el equipo de arquitectura de soluciones en la nube de Azure de la unidad de clientes de Reino Unido y nuestro socio de Microsoft, [Ampliphae](http://www.ampliphae.com/).


## <a name="deploy-the-solution"></a>Implementación de la solución

Esta instancia de Azure Security and Compliance Blueprint Automation se compone de archivos de configuración de JSON y scripts de PowerShell que se controlan mediante el servicio de API de Azure Resource Manager para implementar recursos en Azure. Puede encontrar instrucciones detalladas acerca de la implementación [aquí](https://aka.ms/ukofficial-paaswa-repo).

Se han proporcionado tres enfoques para la implementación. Un enfoque con la [CLI de Azure 2](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) "rápido" adecuado para crear rápidamente un entorno de prueba, un enfoque con la [CLI de Azure 2](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) parametrizado que proporciona mayor configuración para entornos de carga de trabajo y una implementación basada en Azure Portal donde el operador puede especificar los parámetros de implementación a través de Azure Portal. 

1.  Clone o descargue [este](https://aka.ms/ukofficial-paaswa-repo) repositorio de GitHub en su estación de trabajo local.
2.  Revise el [Método 1: CLI de Azure 2 (versión rápida)](https://aka.ms/ukofficial-paaswa-repo/#method-1-azure-cli-2-express-version) y ejecute los comandos proporcionados.
3.  Revise el [Método 1a: CLI de Azure 2 (Configuración de la implementación a través de los argumentos del script)](https://aka.ms/ukofficial-paaswa-repo/#method-1a-azure-cli-2-configuring-the-deployment-via-script-arguments) y ejecute los comandos proporcionados
4.  Revise el [Método 2: proceso de implementación de Azure Portal](https://aka.ms/ukofficial-paaswa-repo/#method-2-azure-portal-deployment-process) y ejecute los comandos enumerados

## <a name="guidance-and-recommendations"></a>Instrucciones y recomendaciones

### <a name="api-management"></a>API Management

[Azure API Management](https://azure.microsoft.com/services/api-management/) podría usarse delante de la API de App Service para proporcionar capas adicionales de seguridad, limitación y controles de exposición, proxy y protección de las API.

### <a name="azure-b2c"></a>Azure B2C

[Azure Active Directory B2C](https://azure.microsoft.com/services/active-directory-b2c/) se puede implementar como un control para permitir a los usuarios registrarse, crear una identidad y habilitar la autorización y el control de acceso para la aplicación web pública.

## <a name="disclaimer"></a>Renuncia de responsabilidades

- Este documento es meramente informativo. MICROSOFT NO EFECTÚA NINGÚN TIPO DE GARANTÍA, YA SEA EXPRESA, IMPLÍCITA O ESTATUTARIA, SOBRE LA INFORMACIÓN INCLUIDA EN EL PRESENTE DOCUMENTO. Este documento se proporciona "tal cual". La información y las opiniones expresadas en este documento, como las direcciones URL y otras referencias a sitios web de Internet, pueden cambiar sin previo aviso. Los clientes que lean este documento se atienen a las consecuencias de usarlo.
- Este documento no proporciona a los clientes ningún derecho legal sobre ninguna propiedad intelectual de ningún producto o solución de Microsoft.
- Los clientes pueden copiar y usar este documento con fines internos y de referencia.
- Algunas de las recomendaciones de este documento pueden provocar un aumento del uso de datos, de la red o de los recursos de procesos en Azure, lo que podría incrementar los costos de las licencias o las suscripciones de Azure.
- Esta arquitectura está diseñada para servir como base para que los clientes puedan ajustarse a sus requisitos específicos y no debe usarse tal cual en un entorno de producción.
- Este documento se ha desarrollado como referencia y no debe usarse para definir todos los medios por los que un cliente puede cumplir normas y requisitos de cumplimiento específicos. Los clientes deben buscar apoyo legal de su organización sobre las implementaciones de cliente aprobadas.
