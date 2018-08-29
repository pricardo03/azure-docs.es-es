---
title: Rediseñar la arquitectura de una aplicación de Contoso en un contenedor de Azure y Azure SQL Database | Microsoft Docs
description: Información de cómo Contoso rediseña la arquitectura de una aplicación en contenedores Windows de Azure y Azure SQL Database.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 08/13/2018
ms.author: raynew
ms.openlocfilehash: 733a93d0fc80d86d28f13a9e1d32108b58893bf0
ms.sourcegitcommit: a2ae233e20e670e2f9e6b75e83253bd301f5067c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2018
ms.locfileid: "42143046"
---
# <a name="contoso-migration-rearchitect-an-on-premises-app-to-an-azure-container-and-azure-sql-database"></a>Migración de Contoso: rediseñar la arquitectura de una aplicación local en un contenedor de Azure y Azure SQL Database

En este artículo se muestra cómo Contoso migra y rediseña la arquitectura de su aplicación SmartHotel en Azure. Migran una VM del front-end de la aplicación a un contenedor Windows de Azure y la base de datos de la aplicación a una instancia de Azure SQL Database.

Este documento es el primero de una serie de artículos que muestran cómo la compañía ficticia Contoso migra sus recursos locales a la nube de Microsoft Azure. En la serie se incluye información de fondo y escenarios en los que se muestra la configuración de una infraestructura de migración, la valoración de los recursos locales para la migración y la ejecución de distintos tipos de migraciones. La complejidad de los escenarios aumenta e iremos agregando otros artículos con el tiempo.

**Artículo** | **Detalles** | **Estado**
--- | --- | ---
[Artículo 1: Introducción](contoso-migration-overview.md) | Se proporciona una introducción a la estrategia de migración de Contoso, la serie de artículos y las aplicaciones de ejemplo que usamos. | Disponible
[Artículo 2: Deploy an Azure infrastructure](contoso-migration-infrastructure.md) (Implementación de una infraestructura de Azure) | Describe cómo Contoso prepara la infraestructura local y de Azure para la migración. Se utiliza la misma infraestructura para todos los artículos de migración. | Disponible
[Artículo 3: Assess on-premises resources](contoso-migration-assessment.md) (Evaluación de los recursos locales)  | Muestra cómo Contoso evalúa una aplicación local de SmartHotel de dos niveles que se ejecuta en VMware. Contoso evalúa las VM de la aplicación con el servicio [Azure Migrate](migrate-overview.md), y la base de datos de SQL Server de la aplicación con [Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponible
[Artículo 4: Rehost an app to Azure VMs and a SQL Managed Instance](contoso-migration-rehost-vm-sql-managed-instance.md) (Rehospedaje de una aplicación a las VM de Azure o a una instancia administrada de SQL) | Muestra cómo Contoso ejecuta una migración mediante lift-and-shift a Azure para la aplicación SmartHotel. Contoso migra la VM de front-end de la aplicación con [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), y la base de datos de la aplicación a una instancia administrada de SQL con [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Disponible
[Artículo 5: Rehost an app to Azure VMs](contoso-migration-rehost-vm.md) (Rehospedaje de una aplicación a las VM de Azure) | Muestra cómo Contoso migra las VM de la aplicación SmartHotel solo con Site Recovery. | Disponible
[Artículo 6: Rehospedaje de una aplicación en VM de Azure y el grupo de disponibilidad Always On de SQL Server](contoso-migration-rehost-vm-sql-ag.md) | Muestra cómo Contoso migra la aplicación SmartHotel. Contoso usa Site Recovery para migrar las VM de la aplicación, y Database Migration Service para migrar la base de datos de la aplicación a un clúster de SQL Server protegido por un grupo de disponibilidad Always On. | Disponible
[Artículo 7: Rehospedaje de una aplicación Linux en VM de Azure](contoso-migration-rehost-linux-vm.md) | Muestra cómo Contoso hace una migración mediante lift-and-shift de la aplicación de osTicket de Linux para VM de Azure, con Site Recovery. | Disponible
[Artículo 8: Rehospedaje de una aplicación de Linux en VM de Azure y en Azure MySQL Server](contoso-migration-rehost-linux-vm-mysql.md) | Se muestra cómo Contoso migra la aplicación osTicket de Linux a VM de Azure mediante Site Recovery y cómo migra la base de datos de aplicaciones a una instancia de Azure MySQL Server mediante MySQL Workbench. | Disponible
[Artículo 9: Refactorizar una aplicación a Azure Web Apps y Azure SQL Database](contoso-migration-refactor-web-app-sql.md) | Muestra cómo Contoso migra la aplicación de SmartHotel a una aplicación web de Azure y migra la base de datos de la aplicación a la instancia de Azure SQL Server | Disponible
[Artículo 10: Refactorizar una aplicación Linux a Azure Web Apps y Azure MySQL](contoso-migration-refactor-linux-app-service-mysql.md) | Muestra cómo Contoso migra la aplicación Linux osTicket a Azure Web Apps en varios sitios integrados con GitHub para ofrecer una entrega continua. Se migra la base de datos de la aplicación a una instancia de Azure MySQL. | Disponible
[Artículo 11: Refactorizar TFS en VSTS](contoso-migration-tfs-vsts.md) | Muestra cómo Contoso migra su implementación de Team Foundation Server (TFS) local mediante la migración a Visual Studio Team Services (VSTS) en Azure.
Artículo 12: Rediseño de la arquitectura de una aplicación en Azure Containers y Azure SQL Database | Muestra cómo Contoso migra la aplicación de SmartHotel a Azure y rediseña su arquitectura. Rediseñan la arquitectura del nivel web de la aplicación como contenedor de Windows, y la base de datos de la aplicación en una instancia de Azure SQL Database. | Este artículo
[Artículo 13: Volver a compilar una aplicación en Azure](contoso-migration-rebuild.md) | Muestra cómo Contoso vuelve a compilar su aplicación de SmartHotel con una gama de funcionalidades y servicios de Azure, como App Services, Azure Kubernetes, Azure Functions, Cognitive Services y Cosmos DB. | Disponible

En este artículo, Contoso migra el sistema operativo Windows de dos niveles. de .NET de Windows de dos niveles que se ejecuta en las VM de VMware. Esta aplicación está disponible en código abierto y, si quiere usarla, puede descargarla en [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>Impulsores del negocio

El equipo directivo de TI ha trabajado estrechamente con sus asociados comerciales para comprender lo que quieren lograr con esta migración:

- **Abordaje del crecimiento empresarial**: Contoso está creciendo y, como resultado, existe una presión en su infraestructura y sus sistemas locales.
- **Aumento de la eficacia**: Contoso debe eliminar los procedimientos innecesarios y optimizar los procesos para los desarrolladores y usuarios.  La empresa necesita que el equipo de TI sea rápido y no malgaste tiempo ni dinero, por lo que debe satisfacer más rápidamente los requisitos del cliente.
- **Aumentar la agilidad**: el equipo de TI de Contoso necesita más capacidad de respuesta a las necesidades de la empresa. Debe poder reaccionar con más rapidez que los cambios del mercado para facilitar el éxito en una economía global.  No se debe interponer en el camino ni bloquear el negocio.
- **Escalar**: a medida que el negocio crece satisfactoriamente, el equipo de TI de Contoso debe proporcionar sistemas que puedan crecer al mismo ritmo.
- **Costos**: Contoso quiere minimizar los costos de licencia.

## <a name="migration-goals"></a>Objetivos de la migración

El equipo de la nube de Contoso ha establecido los objetivos de esta migración. Estos objetivos se usaron para determinar el mejor método de migración.

**Objetivos** | **Detalles**
--- | --- 
**Solicitudes de aplicación** | La aplicación de Azure seguirá siendo tan importante como lo es hoy en día.<br/><br/> Debe tener las mismas capacidades de rendimiento que las actuales en VMWare.<br/><br/> Quieren detener la compatibilidad con Windows Server 2008 R2, donde se ejecuta actualmente la aplicación, para invertir en la aplicación.<br/><br/> Quieren abandonar SQL Server 2008 R2 y pasar a una plataforma de base de datos PaaS moderna, lo que minimizará la necesidad de administración.<br/><br/> Contoso desea aprovechar su inversión en licencias de SQL Server y Software Assurance, siempre que sea posible.<br/><br/> Quieren poder escalar verticalmente el nivel de aplicación web.
**Limitaciones** | La aplicación está formada por una aplicación de ASP.NET y un servicio WCF que se ejecutan en la misma máquina virtual. Desean dividir esto en dos aplicaciones web con Azure App Service. 
**Solicitudes de Azure** | Quieren mover la aplicación a Azure y ejecutarla en un contenedor para prolongar la duración de la aplicación. No desean empezar a implementar la aplicación en Azure completamente desde cero. 

## <a name="solution-design"></a>Diseño de la solución

Después de fijar sus objetivos y requisitos, Contoso diseña y revisa una solución de implementación e identifica el proceso de migración, incluidos los servicios de Azure que utilizará para la migración.

### <a name="current-app"></a>Aplicación actual

- La aplicación local de SmartHotel se divide en niveles entre dos VM (WEBVM y SQLVM).
- Las VM están ubicadas en el host de VMware ESXi **contosohost1.contoso.com** (versión 6.5).
- El entorno de VMware lo administra vCenter Server 6.5 (**vcenter.contoso.com**), que se ejecuta en una VM.
- Contoso tiene un centro de datos local (contoso-datacenter), con un controlador de dominio local (**contosodc1**).
- Las VM locales del centro de datos de Contoso se retirarán después de realizar la migración.


### <a name="proposed-architecture"></a>Arquitectura propuesta

- Para el nivel de base de datos de la aplicación, Contoso comparó Azure SQL Database con SQL Server con [este artículo](https://docs.microsoft.com/azure/sql-database/sql-database-features). Se decantó por Azure SQL Database por diversas razones:
    - Azure SQL Database es un servicio administrado de base de datos relacional. Ofrece un rendimiento predecible en los diferentes niveles del servicio, con administración casi inexistente. Las ventajas incluyen escalabilidad dinámica sin tiempo de inactividad, optimización inteligente integrada y disponibilidad y escalabilidad global.
    - Se puede aprovechar la herramienta ligera Data Migration Assistant (DMA) para evaluar y migrar la base de datos local a Azure SQL.
    - Con Software Assurance, el equipo de Contoso puede intercambiar sus licencias existentes para obtener descuentos en una instancia de SQL Database mediante el uso de la Ventaja híbrida de Azure para SQL Server. Esto puede proporcionar un ahorro de hasta un 30 %.
    - SQL Database proporciona diferentes características de seguridad, incluidos el enmascaramiento dinámico de datos siempre cifrados y la detección de amenazas/seguridad de nivel de fila.
- Para el nivel de aplicación web, decidieron convertirlo en el contenedor de Windows con Visual Studio.
    - Implementarán la aplicación con Azure Service Fabric y extraerán la imagen del contenedor de Windows desde Azure Container Registry (ACR).
    - Implementarán un prototipo para ampliar la aplicación con la inclusión del análisis de sentimiento, como otro servicio en Service Fabric, conectado a Cosmos DB.  Esto leerá la información de Tweets y la mostrará en la aplicación.

    ![Arquitectura del escenario](./media/contoso-migration-rearchitect-container-sql/architecture.png) 

  
### <a name="solution-review"></a>Revisión de la solución
Contoso evalúa el diseño propuesto creando una lista de ventajas y desventajas.

**Consideración** | **Detalles**
--- | ---
**Ventajas** | No será necesario modificar el código de la aplicación SmartHotel para la migración a Azure Service Fabric. Sin embargo, el esfuerzo es mínimo, si se usa SDK Tools de Service Fabric para los cambios.<br/><br/> Con el movimiento a Service Fabric, pueden empezar a desarrollar microservicios para agregarlos a la aplicación con rapidez en el tiempo, sin riesgo para el código base original.<br/><br/> Los contenedores de Windows ofrecen las mismas ventajas que los contenedores en general. Mejoran la agilidad, la portabilidad y el control.<br/><br/> Pueden aprovechar la inversión en Software Assurance con la Ventaja híbrida de Azure para SQL Server y Windows Server.<br/><br/> Después de la migración ya no necesitarán la compatibilidad con Windows Server 2008 R2. [Más información](https://support.microsoft.com/lifecycle).<br/><br/> Puede configurar el nivel web de la aplicación con varias instancias, para que no haya un único punto de error.<br/><br/> Ya no se dependerá de la antigüedad de SQL Server 2008 R2.<br/><br/> SQL Database es compatible con los requisitos técnicos de Contoso. Contoso evaluó la base de datos local con Data Migration Assistant y averiguó que es compatible.<br/><br/> SQL Database cuenta con tolerancia a errores integrada que no es necesario que Contoso configure. Esto garantiza que la capa de datos ya no sea un único punto de conmutación por error.
**Desventajas** | Los contenedores son más complejos que otras opciones de migración. La curva de aprendizaje de los contenedores podría ser un problema para Contoso.  Introducen un nuevo nivel de complejidad que proporciona un gran valor a pesar de la curva.<br/><br/> El equipo de operaciones de Contoso deberá esforzarse para comprender y ofrecer soporte técnico de Azure, los contenedores y los microservicios de la aplicación.<br/><br/> Si usan Data Migration Assistant en lugar de Data Migration Service para migrar la base de datos, la infraestructura de Contoso no estará preparada para migrar bases de datos a escala.



### <a name="migration-process"></a>Proceso de migración

1. Contoso aprovisiona el clúster de Azure Service Fabric para Windows.
2. Aprovisionan una instancia de SQL Azure y migran la base de datos de SmartHotel a dicha instancia.
3. Convierten la máquina virtual de nivel web en un contenedor Docker con SDK Tools de Service Fabric.
4. Conectan el clúster de Service Fabric y el ACR, e implementan la aplicación con Azure Service Fabric.

    ![Proceso de migración](./media/contoso-migration-rearchitect-container-sql/migration-process.png) 

### <a name="azure-services"></a>Servicios de Azure

**Servicio** | **Descripción** | **Costee**
--- | --- | ---
[Database Migration Assistant (DMA)](https://docs.microsoft.com/sql/dma/dma-overview?view=ssdt-18vs2017) | Usará DMA para valorar y detectar problemas de compatibilidad que podrían afectar a la funcionalidad de su base de datos en Azure. DMA evalúa una paridad de características entre orígenes y destinos de SQL, y recomienda mejoras de rendimiento y confiabilidad. | Es una herramienta que se puede descargar de forma gratuita.
[Azure SQL Database](https://azure.microsoft.com/services/sql-database/) | Servicio de base de datos relacional inteligente y completamente administrado en la nube. | Costo basado en características, rendimiento y tamaño. [Más información](https://azure.microsoft.com/pricing/details/sql-database/managed/).
[Azure Container Registry](https://azure.microsoft.com/services/container-registry/) | Almacene imágenes para todos los tipos de implementaciones de contenedor. | Costo basado en características, almacenamiento y duración de la utilización. [Más información](https://azure.microsoft.com/pricing/details/container-registry/).
[Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | Compile y utilice aplicaciones escalables y distribuidas que estén siempre disponibles. | Costo basado en tamaño, ubicación y duración de los nodos de proceso. [Más información](https://azure.microsoft.com/pricing/details/service-fabric/).

## <a name="prerequisites"></a>Requisitos previos

Esto es lo que usted (y Contoso) necesita para ejecutar este escenario.

**Requisitos** | **Detalles**
--- | ---
**Suscripción de Azure** | Ya debería haber creado una suscripción cuando realizó la evaluación en el primer artículo de esta serie. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Si crea una cuenta gratuita, será el administrador de su suscripción y podrá realizar todas las acciones.<br/><br/> Si usa una suscripción existente y no es el administrador, tendrá que solicitar al administrador que le asigne permisos de propietario o colaborador.
**Infraestructura de Azure** | [Vea](contoso-migration-infrastructure.md) cómo Contoso configuró una infraestructura de Azure.
**Requisitos previos para desarrolladores** | Contoso necesita las siguientes herramientas en una estación de trabajo de desarrollador:<br/><br/> - [Visual Studio 2017 Community Edition: versión 15.5](https://www.visualstudio.com/)<br/><br/> - Carga de trabajo. NET habilitada.<br/><br/> - [Git](https://git-scm.com/)<br/><br/> - [SDK de Service Fabric v 3.0 o posterior](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started)<br/><br/> - [Docker CE (Windows 10) o Docker EE (Windows Server)](https://docs.docker.com/docker-for-windows/install/) configurado para usar contenedores de Windows.



## <a name="scenario-steps"></a>Pasos del escenario

Contoso ejecutará la migración de la forma siguiente:

> [!div class="checklist"]
> * **Paso 1: Aprovisionar una instancia de SQL Database en Azure**: Contoso aprovisiona una instancia de SQL en Azure. Después de migrar la máquina virtual web de front-end a un contenedor de Azure, la instancia de contenedor con el front-end web de la aplicación apuntará a esta base de datos.
> * **Paso 2: Aprovisionar Azure Service Fabric**: aprovisionan un clúster de Service Fabric.
> * **Paso 4: Migrar la base de datos con DMA**: migran la base de datos de la aplicación con Database Migration Assistant.
> * **Paso 5: Convertir la aplicación en un contenedor**: convierten la aplicación en un contenedor con Visual Studio y SDK Tools.
> * **Paso 6: Publicar la aplicación**: publican la aplicación en el ACR y un clúster de Service Fabric.
> * **Paso 7: Extender la aplicación**: después de publicar la aplicación, la amplían para aprovechar las funcionalidades de Azure y la vuelven a publicar en Azure.



## <a name="step-1-provision-an-azure-sql-database"></a>Paso 1: Aprovisionar una instancia de Azure SQL Database

1. Selecciona crear una instancia de SQL Database en Azure. 

    ![Aprovisionar SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql1.png)

2. Especifica un nombre de base de datos para que coincida con la base de datos que se ejecuta en la máquina virtual local (**SmartHotel.Registration**). Coloca la base de datos en el grupo de recursos ContosoRG. Este es el grupo de recursos que usa para los recursos de producción en Azure.

    ![Aprovisionar SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql2.png)

3. Configura una nueva instancia de SQL Server (**sql-smarthotel-eus2**) en la región primaria.

    ![Aprovisionar SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql3.png)

4. Establece el plan de tarifa que mejor se adapta a las necesidades de su servidor y base de datos. Así mismo, selecciona esta opción para ahorrar dinero con la Ventaja híbrida de Azure, porque ya tiene una licencia de SQL Server.
5. Para ajustar el tamaño, realiza compras basadas en el núcleo virtual y establece los límites para los requisitos esperados.

    ![Aprovisionar SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql4.png)

6. A continuación, Contoso crea la instancia de la base de datos.

    ![Aprovisionar SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql5.png)

7. Una vez creada la instancia, abre la base de datos y anota los detalles que necesitará cuando use Database Migration Assistance para la migración.

    ![Aprovisionar SQL](./media/contoso-migration-rearchitect-container-sql/provision-sql6.png)


**¿Necesita más ayuda?**

- [Obtenga ayuda](https://docs.microsoft.com/azure/sql-database/sql-database-get-started-portal) para aprovisionar una instancia de SQL Database.
- [Obtenga información](https://docs.microsoft.com/azure/sql-database/sql-database-vcore-resource-limits-elastic-pools) acerca de los límites de recursos de núcleo virtual.



## <a name="step-2-create-an-acr-and-provision-an-azure-container"></a>Paso 2: Crear un ACR y aprovisionar un contenedor de Azure

El contenedor de Azure se crea con los archivos exportados de la máquina virtual web. El contenedor se aloja en Azure Container Registry (ACR).


1. Contoso crea una instancia de Container Registry en Azure Portal.

     ![Container Registry](./media/contoso-migration-rearchitect-container-sql/container-registry1.png)

2. Proporcionan un nombre para el registro (**contosoacreus2**) y lo colocan en la región primaria, en el grupo de recursos que usan para sus recursos de infraestructura. Habilitan el acceso de usuarios administradores y lo establecen como SKU premium para que puedan utilizar la replicación geográfica.

    ![Container Registry](./media/contoso-migration-rearchitect-container-sql/container-registry2.png)  


## <a name="step-3-provision-azure-service-fabric"></a>Paso 3: Aprovisionar Azure Service Fabric

El contenedor SmartHotel se ejecutará en el clúster de Azure Service Fabric. Contoso crea el clúster de Service Fabric como sigue:

1. Cree un recurso de Service Fabric en Azure Marketplace.

     ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric1.png)

2. En **Básico**, proporcionan un nombre único de DS para el clúster y las credenciales para obtener acceso a la máquina virtual local. Colocan el recurso en el grupo de recursos de producción (**ContosoRG**) en la región primaria Este de EE.UU. 2.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric2.png) 

3. En **Configuración del tipo de nodo**, especifican un nombre de tipo de nodo, la configuración de durabilidad, el tamaño de máquina virtual y los puntos de conexión de la aplicación.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric3.png) 


4. En **Crear almacén de claves**, crean un nuevo almacén de claves en su grupo de recursos de infraestructura, para alojar el certificado.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric4.png) 


5. En **Directivas de acceso**, activan el acceso a las máquinas virtuales para implementar el almacén de claves.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric5.png) 

6. Especifican un nombre para el certificado.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric6.png) 

7. En la página de resumen, copian el vínculo que se usa para descargar el certificado. Lo necesitan para conectarse al clúster de Service Fabric.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric7.png) 

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric8.png) 

8. Después de la validación correcta, aprovisionan el clúster.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric9.png) 

9. En el Asistente para importar certificados, importan el certificado descargado a equipos de desarrollo. El certificado se usa para autenticarse en el clúster.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric10.png) 

10. Después de aprovisionar el clúster, se conectan al explorador de clúster de Service Fabric.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric11.png) 

11. Deben seleccionar el certificado correcto.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric12.png) 

12. Se carga la instancia de Service Fabric Explorer y el administrador de Contoso puede administrar el clúster.

    ![Service Fabric](./media/contoso-migration-rearchitect-container-sql/service-fabric13.png) 


## <a name="step-3-migrate-the-database-with-dma"></a>Paso 3: Migrar la base de datos con DMA

Contoso migrará la base de datos de SmartHotel con DMA.

### <a name="install-dma"></a>Instalar DMA

1. Descarga la herramienta desde el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=53595) en la VM local de SQL Server (**SQLVM**).
2. Ejecuta el programa de instalación (DownloadMigrationAssistant.msi) en la VM.
3. En la página **Finalizar**, selecciona **Launch Microsoft Data Migration Assistant** (Iniciar Microsoft Data Migration Assistant) antes de finalizar el asistente.

### <a name="configure-the-firewall"></a>Configuración del firewall

Para conectarse a SQL Azure Database, se necesita una regla de firewall.

1. En las propiedades de **Firewall y redes virtuales** para la base de datos, permiten el acceso a servicios de Azure y agregan una regla para la dirección IP del cliente de la máquina virtual de SQL Server local.
2. Se crea una regla de firewall de nivel de servidor.

    ![Firewall](./media/contoso-migration-rearchitect-container-sql/sql-firewall.png)

¿Necesita más ayuda?

[Obtenga información sobre](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure#creating-and-managing-firewall-rules) la creación y la administración de reglas de firewall para Azure SQL Database.

### <a name="migrate"></a>Migrar

1. En DMA, crea un nuevo proyecto (**SmartHotelDB**) y selecciona **Migración** 
2. Contoso selecciona el tipo de servidor de origen como **SQL Server** y el destino como instancia de **Azure SQL Database**. 

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-1.png)

3. En los detalles de la migración, agrega **SQLVM** como servidor de origen y la base de datos **SmartHotel.Registration**. 

     ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-2.png)

4. Recibe un error que parece que se asociará con la autenticación. Sin embargo, después de investigar, el problema es el punto (.) en el nombre de la base de datos. Como alternativa, decide aprovisionar una nueva base de datos SQL con el nombre **SmartHotel-Registration**, para resolver el problema. Cuando vuelve a ejecutar DMA, puede seleccionar **SmartHotel-Registration** y continuar con el asistente.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-3.png)

5. En **Seleccionar objetos**, selecciona las tablas de la base de datos y genera un script de SQL.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-4.png)

6. Una vez que DMS crea el script, hace clic en **Implementar esquema**.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-5.png)

7. DMA confirma que la implementación se realizó correctamente.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-6.png)

8. Luego, se inicia la migración.

    ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-7.png)

9. Una vez finalizada la migración, Contoso puede comprobar que la base de datos se ejecuta en la instancia de SQL Azure.

     ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-8.png)

10. Elimina la base de datos SQL adicional **SmartHotel.Registration** de Azure Portal.

     ![DMA](./media/contoso-migration-rearchitect-container-sql/dma-9.png)



## <a name="step-4-convert-the-app-to-a-container"></a>Paso 4: Convertir la aplicación en un contenedor

La aplicación local es una aplicación tradicional de tres niveles:

- Contiene WebForms y un Servicio WCF que conecta a SQL Server.
- Utiliza Entity Framework para integrarse con los datos de la base de datos SQL y los expone a través de un Servicio WCF.
- La aplicación WebForms interactúa con el Servicio WCF.

Contoso convertirá la aplicación en un contenedor con Visual Studio y SDK Tools, de la manera siguiente:

1. Clonan el repositorio localmente en una máquina de desarrollador:

    **clonación de GIThttps://github.com/Microsoft/SmartHotel360-internal-booking-apps.git**

    ![Contenedor](./media/contoso-migration-rearchitect-container-sql/container1.png)

2. Con Visual Studio, abren el archivo de solución (SmartHotel.Registration.sln) en el directorio **SmartHotel360-interno-reserva-apps\src\Registration** del repositorio local.  Se muestran dos aplicaciones. El front-end web SmartHotel.Registration.Web y la aplicación de Servicio WCF SmartHotel.Registration.WCF.

    ![Contenedor](./media/contoso-migration-rearchitect-container-sql/container2.png)


3. Hacen clic con el botón derecho en la aplicación web > **Agregar** > **Container Orchestrator Support** (Compatibilidad con el orquestador de contenedores).
4. En **Add Container Orchestra Support** (Agregar compatibilidad con el orquestador de contenedores), seleccionan **Service Fabric**.

    ![Contenedor](./media/contoso-migration-rearchitect-container-sql/container3.png)

5. Contoso repite el proceso para la aplicación SmartHotel.Registration.WCF.
6. Ahora, Contoso comprueba cómo ha cambiado la solución.

    - La nueva aplicación es **SmartHotel.RegistrationApplication/**
    - Contiene dos servicios: **SmartHotel.Registration.WCF** y **SmartHotel.Registration.Web**.

    ![Contenedor](./media/contoso-migration-rearchitect-container-sql/container4.png)

7. Visual Studio creó el archivo de Docker y extrajo localmente las imágenes necesarias en el equipo del desarrollador.

    ![Contenedor](./media/contoso-migration-rearchitect-container-sql/container5.png)

8. Visual Studio crea y abre un archivo de manifiesto (**ServiceManifest.xml**). Este archivo indica a Service Fabric cómo configurar el contenedor cuando se implementa en Azure.

    ![Contenedor](./media/contoso-migration-rearchitect-container-sql/container6.png)

9. Otro archivo de manifiesto (**ApplicationManifest.xml) contiene las aplicaciones de configuración para los contenedores.

    ![Contenedor](./media/contoso-migration-rearchitect-container-sql/container7.png)

## <a name="step-5-publish-the-app"></a>Paso 5: Publicar la aplicación


Por último, Contoso puede publicar la aplicación en el ACR y el clúster de Service Fabric.

> [!NOTE]
> Se realizaron algunos cambios relacionados con el clúster de Service Fabric en la aplicación SmartHotel. Puede descargar el código de aplicación modernizada y original de [GitHub](https://github.com/Microsoft/SmartHotel360-internal-booking-apps). El archivo modificado es **AppliationModern/ApplicationParameters/Cloud.xml**.


1. En Visual Studio, actualice la cadena de conexión para conectar la aplicación a Azure SQL Database. La cadena de conexión puede encontrarse en la base de datos en Azure Portal.

    ![Publicar](./media/contoso-migration-rearchitect-container-sql/publish1.png)

2. Contoso publica la aplicación en Service Fabric mediante Visual Studio. Haga clic con el botón derecho en la aplicación de Service Fabric > **Publicar**.

    ![Publicar](./media/contoso-migration-rearchitect-container-sql/publish2.png)

3. Seleccionan la suscripción, el punto de conexión y el ACR. A continuación, haga clic en **Publicar**.

    ![Publicar](./media/contoso-migration-rearchitect-container-sql/publish3.png)

4. Una vez finalizada la implementación, SmartHotel ejecutará Service Fabric.

    ![Publicar](./media/contoso-migration-rearchitect-container-sql/publish4.png)

5. Para conectarse a la aplicación, Contoso dirige el tráfico a la dirección IP pública del equilibrador de carga de Azure delante de sus nodos de Service Fabric.

    ![Publicar](./media/contoso-migration-rearchitect-container-sql/publish5.png)

## <a name="step-6-extend-the-app-and-republish"></a>Paso 6: Ampliar la aplicación y volverla a publicar

Después de que la aplicación SmartHotel y la base de datos estén en ejecución en Azure, Contoso quiere ampliar la aplicación.

- Los desarrolladores de Contoso crean el prototipo de una nueva aplicación de .NET Core que se ejecutará en el clúster de Service Fabric.
- La aplicación se usará para extraer datos de opiniones de CosmosDB.
- Estos datos estarán en formato de tweets que se procesan mediante una función sin servidor de Azure y Cognitive Services Text Analysis API.

### <a name="provision-azure-cosmos-db"></a>Aprovisionar Azure Cosmos DB

En primer lugar, Contoso aprovisiona una base de datos de Azure Cosmos.

1. Crean un recurso de Azure Cosmos DB en Azure Marketplace.

    ![Extend](./media/contoso-migration-rearchitect-container-sql/extend1.png)

2. Proporcionan un nombre de base de datos (**contososmarthotel**), seleccionan la API de SQL y colocan el recurso en el grupo de recursos de producción, en la región primaria Este de EE.UU. 2.

    ![Extend](./media/contoso-migration-rearchitect-container-sql/extend2.png)

3. En **Introducción**, seleccionan **Explorador de datos** y agregan una nueva colección.
4. En **Agregar colección** proporcionan identificadores y establecen la capacidad de almacenamiento y rendimiento.

    ![Extend](./media/contoso-migration-rearchitect-container-sql/extend3.png)

5. En el portal, abren la nueva base de datos > **Colección** > **Documentos** y hacen clic en **Nuevo documento**.
6. Pegan el siguiente código JSON en la ventana de documento. Se trata de datos de ejemplo en forma de un tweet único.

    ```
    {
            "id": "2ed5e734-8034-bf3a-ac85-705b7713d911",
            "tweetId": 927750234331580911,
            "tweetUrl": "https://twitter.com/status/927750237331580911",
            "userName": "CoreySandersWA",
            "userAlias": "@CoreySandersWA",
            "userPictureUrl": "",
            "text": "This is a tweet about #SmartHotel",
            "language": "en",
            "sentiment": 0.5,
            "retweet_count": 1,
            "followers": 500, 
            "hashtags": [
                ""
            ]
    }
    ```

    ![Extend](./media/contoso-migration-rearchitect-container-sql/extend4.png)

7. Localizan el punto de conexión de Cosmos DB y la clave de autenticación. Se usan en la aplicación para conectarse a la colección. En la base de datos, hacen clic en **Claves** y copian el URI y la clave principal en el Bloc de notas.

    ![Extend](./media/contoso-migration-rearchitect-container-sql/extend5.png)

### <a name="update-the-sentiment-app"></a>Actualizar la aplicación de opiniones

Después de aprovisionar Cosmos DB, Contoso puede configurar la aplicación para su conexión.

1. En Visual Studio, abren el archivo ApplicationModern\ApplicationParameters\cloud.xml en el Explorador de soluciones.

    ![Aplicación de opiniones](./media/contoso-migration-rearchitect-container-sql/sentiment1.png)

2. Rellenan los dos parámetros siguientes:

   ```
   <Parameter Name="SentimentIntegration.CosmosDBEndpoint" Value="[URI]" />
   ```
   
   ```
   <Parameter Name="SentimentIntegration.CosmosDBAuthKey" Value="[Key]" />
   ```

    ![Aplicación de opiniones](./media/contoso-migration-rearchitect-container-sql/sentiment2.png)

### <a name="republish-the-app"></a>Volver a publicar la aplicación

Después de ampliar la aplicación, Contoso la vuelve a publicar en Azure.

1. En el portal, hacen clic con el botón derecho en la aplicación de Service Fabric > **Publicar**.

    ![Volver a publicar](./media/contoso-migration-rearchitect-container-sql/republish1.png)

2. Seleccionan la suscripción, el punto de conexión y el ACR. A continuación, haga clic en **Publicar**.

    ![Volver a publicar](./media/contoso-migration-rearchitect-container-sql/republish2.png)

4. Una vez finalizada la implementación, SmartHotel ejecutará Service Fabric. La consola de administración de Service Fabric muestra ahora tres servicios.

    ![Volver a publicar](./media/contoso-migration-rearchitect-container-sql/republish3.png)

5. Contoso puede hacer clic a través de los servicios para ver si la aplicación SentimentIntegration está en funcionamiento.

    ![Volver a publicar](./media/contoso-migration-rearchitect-container-sql/republish4.png)

## <a name="clean-up-after-migration"></a>Limpiar después de la migración

Después de la migración, Contoso debe completar estos pasos de limpieza:  

- Quitar las VM locales del inventario de vCenter.
- Quitar las VM de los trabajos de copia de seguridad locales.
- Actualizar la documentación interna para que muestre las nuevas ubicaciones para la aplicación de SmartHotel. Mostrar la base de datos en ejecución en Azure SQL Database y el front-end en ejecución en Service Fabric.
- Revisar los recursos que interactúan con las VM retiradas y actualizar los valores de configuración pertinentes o la documentación para reflejar la nueva configuración.


## <a name="review-the-deployment"></a>Revisar la implementación

Con los recursos migrados de Azure, Contoso debe proteger la infraestructura nueva y ponerla totalmente en marcha.

### <a name="security"></a>Seguridad

- Contoso debe asegurarse de que su nueva base de datos **SmartHotel-Registration** es segura. [Más información](https://docs.microsoft.com/azure/sql-database/sql-database-security-overview).
- En concreto, deben actualizar el contenedor para usar SSL con certificados.
- Debería considerarse el uso de KeyVault para proteger los secretos de las aplicaciones de Service Fabric. [Más información](https://docs.microsoft.com/azure/service-fabric/service-fabric-application-secret-management).

### <a name="backups"></a>Copias de seguridad

- Contoso necesita revisar los requisitos de copia de seguridad para la instancia de Azure SQL Database. [Más información](https://docs.microsoft.com/azure/sql-database/sql-database-automated-backups).
- Debe considerar la implementación de grupos de conmutación por error para proporcionar conmutación por error regional para la base de datos. [Más información](https://docs.microsoft.com/azure/sql-database/sql-database-geo-replication-overview).
- Puede aprovechar la replicación geográfica para el SKU premium de ACR. [Más información](https://docs.microsoft.com/azure/container-registry/container-registry-geo-replication).
- Contoso debe considerar la implementación de Web App en las regiones principales Este de EE. UU. 2 y Centro de EE. UU. cuando Web App for Containers esté disponible. Podría configurar Traffic Manager para garantizar la conmutación por error en caso de que se produzcan interrupciones regionales.

### <a name="licensing-and-cost-optimization"></a>Optimización de los costos y licencias

- Una vez implementados todos los recursos, Contoso debe asignar etiquetas de Azure según la [planificación de su infraestructura](contoso-migration-infrastructure.md#set-up-tagging).
- Todas las licencias se integran en el costo de los servicios de PaaS que consume Contoso. Esto se deducirá del contrato Enterprise.
1. Contoso habilitará Azure Cost Management bajo licencia de Cloudyn, una subsidiaria de Microsoft. Es una solución de administración de costos en varias nubes que le permitirá utilizar y administrar Azure y otros recursos en la nube.  [Más información](https://docs.microsoft.com/azure/cost-management/overview) sobre Azure Cost Management. 

## <a name="conclusion"></a>Conclusión

En este artículo, Contoso refactorizó la aplicación SmartHotel en Azure mediante la migración de la VM de front-end de la aplicación a Service Fabric. Migraron la base de datos de la aplicación a Azure SQL Database.





