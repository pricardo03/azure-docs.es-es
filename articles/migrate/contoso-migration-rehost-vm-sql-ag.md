---
title: Rehospedaje de una aplicación de Contoso mediante su migración a VM de Azure y al grupo de disponibilidad Always On de SQL Server | Microsoft Docs
description: Averigüe cómo Contoso rehospeda una aplicación local migrándola a las VM de Azure y al grupo de disponibilidad Always On de SQL Server.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: raynew
ms.openlocfilehash: 33ab56003f2d9428816ea0f32cfd6381ea857df3
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38611295"
---
# <a name="contoso-migration-rehost-an-on-premises-app-to-azure-vms-and-sql-server-alwayson-availability-group"></a>Migración de Contoso: rehospedaje de una aplicación local a las VM de Azure y al grupo de disponibilidad Always On de SQL Server.

En este artículo se muestra cómo Contoso rehospeda su aplicación SmartHotel en Azure. Migra la VM de front-end de la aplicación a una VM de Azure, y la base de datos de la aplicación a una VM de Azure SQL Server, que se ejecuta en el clúster de conmutación por error de Windows Server con Grupos de disponibilidad Always On de SQL Server.

Este documento es el primero de una serie de artículos que muestran cómo la compañía ficticia Contoso migra sus recursos locales a la nube de Microsoft Azure. En la serie se incluye información de fondo y escenarios en los que se muestra la configuración de una infraestructura de migración, la valoración de los recursos locales para la migración y la ejecución de distintos tipos de migraciones. La complejidad de los escenarios aumenta e iremos agregando otros artículos con el tiempo.

**Artículo** | **Detalles** | **Estado**
--- | --- | ---
[Artículo 1: Introducción](contoso-migration-overview.md) | Se proporciona una introducción a la estrategia de migración de Contoso, la serie de artículos y las aplicaciones de ejemplo que usamos. | Disponible
[Artículo 2: Deploy an Azure infrastructure](contoso-migration-infrastructure.md) (Implementación de una infraestructura de Azure) | Describe cómo Contoso prepara la infraestructura local y de Azure para la migración. Se utiliza la misma infraestructura para todos los artículos de migración. | Disponible
[Artículo 3: Assess on-premises resources](contoso-migration-assessment.md) (Evaluación de los recursos locales)  | Muestra cómo Contoso evalúa una aplicación local de SmartHotel de dos niveles que se ejecuta en VMware. Contoso evalúa las VM de la aplicación con el servicio [Azure Migrate](migrate-overview.md), y la base de datos de SQL Server de la aplicación con [Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponible
[Artículo 4: Rehost an app to Azure VMs and a SQL Managed Instance](contoso-migration-rehost-vm-sql-managed-instance.md) (Rehospedaje de una aplicación a las VM de Azure o a una instancia administrada de SQL) | Muestra cómo Contoso ejecuta una migración mediante lift-and-shift a Azure para la aplicación SmartHotel. Contoso migra la VM de front-end de la aplicación con [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview), y la base de datos de la aplicación a una instancia administrada de SQL con [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Disponible
[Artículo 5: Rehost an app to Azure VMs](contoso-migration-rehost-vm.md) (Rehospedaje de una aplicación a las VM de Azure) | Muestra cómo Contoso migra las VM de la aplicación SmartHotel solo con Site Recovery.
Artículo 6: Rehospedaje de una aplicación a VM de Azure y el grupo de disponibilidad Always On de SQL Server (este artículo) | Muestra cómo Contoso migra la aplicación SmartHotel. Contoso usa Site Recovery para migrar las VM de la aplicación, y Database Migration Service para migrar la base de datos de la aplicación a un clúster de SQL Server protegido por un grupo de disponibilidad Always On. | Disponible
[Artículo 7: Rehospedaje de una aplicación Linux en VM de Azure](contoso-migration-rehost-linux-vm.md) | Muestra cómo Contoso hace una migración mediante lift-and-shift de la aplicación de osTicket de Linux para VM de Azure, con Site Recovery. | Planeado
[Artículo 8: Rehospedaje de una aplicación de Linux en VM de Azure y en Azure MySQL Server](contoso-migration-rehost-linux-vm-mysql.md) | Se muestra cómo Contoso migra la aplicación osTicket de Linux a VM de Azure mediante Site Recovery y cómo migra la base de datos de aplicaciones a una instancia de Azure MySQL Server mediante MySQL Workbench. | Disponible



En este artículo, Contoso migra a Azure la aplicación SmartHotel de .NET de Windows de dos niveles que se ejecuta en las VM de VMware. Esta aplicación está disponible en código abierto y, si quiere usarla, puede descargarla en [GitHub](https://github.com/Microsoft/SmartHotel360).

## <a name="business-drivers"></a>Impulsores del negocio

El equipo directivo de TI ha trabajado estrechamente con sus asociados comerciales para comprender lo que quieren lograr con esta migración:

- **Abordaje del crecimiento empresarial**: Contoso está creciendo y, como resultado, existe una presión en su infraestructura y sus sistemas locales.
- **Aumento de la eficacia**: Contoso debe eliminar los procedimientos innecesarios y optimizar los procesos para los desarrolladores y usuarios.  La empresa necesita que el equipo de TI sea rápido y no malgaste tiempo ni dinero, por lo que debe satisfacer más rápidamente los requisitos del cliente.
- **Aumentar la agilidad**: el equipo de TI de Contoso necesita más capacidad de respuesta a las necesidades de la empresa. Debe poder reaccionar con más rapidez que los cambios del mercado para facilitar el éxito en una economía global.  No se debe interponer en el camino ni bloquear el negocio.
- **Escala**: a medida que el negocio crece satisfactoriamente, el equipo de TI de Contoso debe proporcionar sistemas que puedan crecer al mismo ritmo.

## <a name="migration-goals"></a>Objetivos de la migración

El equipo de la nube de Contoso ha establecido los objetivos de esta migración. Estos objetivos se usaron para determinar el mejor método de migración:

- Tras la migración, la aplicación de Azure debería tener las mismas funcionalidades de rendimiento que tiene actualmente en VMWare.  La aplicación seguirá siendo tan imprescindible en la nube como lo es en el entorno local.
- Contoso no quiere hacer inversiones en esta aplicación.  Aunque es importante para la empresa, simplemente quiere moverla a la nube con seguridad sin cambiarla.
- La base de datos local de la aplicación ha tenido problemas de disponibilidad. El equipo querría verla implementada en Azure como un clúster de alta disponibilidad con funcionalidades de conmutación por error.
- Contoso quiere actualizar su plataforma actual de SQL Server 2008 R2 a SQL Server 2017.
- Sin embargo, no quiere usar Azure SQL Database para esta aplicación y está buscando alternativas.

## <a name="proposed-architecture"></a>Arquitectura propuesta

En este escenario:

- La aplicación está interconectada entre dos VM (WEBVM y SQLVM).
- Las VM están ubicadas en el host de VMware ESXi **contosohost1.contoso.com** (versión 6.5).
- El entorno de VMware lo administra vCenter Server 6.5 (**vcenter.contoso.com**), que se ejecuta en una VM.
- Contoso tiene un centro de datos local (contoso-datacenter), con un controlador de dominio local (**contosodc1**).
- Las VM locales del centro de datos de Contoso se retirarán después de realizar la migración.

![Arquitectura del escenario](media/contoso-migration-rehost-vm-sql-ag/architecture.png) 

### <a name="azure-services"></a>Servicios de Azure

**Servicio** | **Descripción** | **Costee**
--- | --- | ---
[Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) | Contoso migrará el nivel de datos de la aplicación mediante DMS. DMS se conectará a la máquina SQLVM local a través de una VPN de sitio a sitio. La migración con el servicio DMS permite realizar migraciones sin problemas desde varios orígenes de bases de datos a las plataformas de datos de Azure, con un tiempo de inactividad mínimo. | Obtenga información acerca de las [regiones compatibles](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability) con DMS y obtenga [detalles de los precios](https://azure.microsoft.com/pricing/details/database-migration/).
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | Contoso usará Site Recovery para migrar mediante lift-and-shift una VM de front-end de la aplicación. Site Recovery orquesta y administra la migración y la recuperación ante desastres de las VM de Azure, así como las VM y los servidores físicos locales.  | Durante la replicación en Azure, se incurre en gastos de Azure Storage.  Las máquinas virtuales de Azure se crean, e incurren en gastos, cuando se produce una conmutación por error. [Más información](https://azure.microsoft.com/pricing/details/site-recovery/) sobre cargos y precios.

 

## <a name="migration-process"></a>Proceso de migración

- Contoso migrará las VM de la aplicación a Azure.
- Migrará la VM de front-end a la VM de Azure mediante Site Recovery:
    - Primero, preparará y configurará los componentes de Azure y preparará la infraestructura local de VMware.
    - Con todo preparado, podrá empezar a replicar la VM.
    - Una vez que se haya habilitado la replicación y esté en funcionamiento, para migrar la VM, hará que conmute por error en Azure.
- Migrará la base de datos a un clúster de SQL Server en Azure con Database Migration Service (DMS).
    - Para empezar, deberá aprovisionar las VM de SQL Server en Azure, configurar el clúster y un equilibrador de carga interno y establecer Grupos de disponibilidad Always On.
    - Cuando complete todo esto, podrá migrar la base de datos.
- Después de la migración, deberá habilitar la protección de Always On para la base de datos.

![Proceso de migración](media/contoso-migration-rehost-vm-sql-ag/migration-process.png) 
 
## <a name="prerequisites"></a>requisitos previos

Esto es lo que usted (y Contoso) necesita para ejecutar este escenario.

**Requisitos** | **Detalles**
--- | ---
**Suscripción de Azure** | Ya debería haber creado una suscripción cuando realizó la evaluación en el primer artículo de esta serie. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Si crea una cuenta gratuita, será el administrador de su suscripción y podrá realizar todas las acciones.<br/><br/> Si usa una suscripción existente y no es el administrador, tendrá que solicitar al administrador que le asigne permisos de propietario o colaborador.<br/><br/> Si necesita permisos más específicos, consulte [este artículo](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Infraestructura de Azure** | [Vea](contoso-migration-infrastructure.md) cómo Contoso configuró una infraestructura de Azure.<br/><br/> Obtenga más información sobre los requisitos específicos de la [red](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) y el [almacenamiento](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) para Site Recovery.
**Recuperación del sitio (local)** | Su instancia local de vCenter Server debe ejecutarse en las versiones 5.5, 6.0 o 6.5.<br/><br/> Un host ESXi que ejecute la versión 5.5, 6.0 o 6.5<br/><br/> Una o más máquinas virtuales VMware que se ejecuten en el host ESXi.<br/><br/> Las máquinas virtuales tienen que cumplir los [requisitos de Azure](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Configuración de [red](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) y [almacenamiento](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) compatible.<br/><br/> Las VM que quiere replicar deben cumplir con los [requisitos de Azure](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).
**DMS** | Para DMS necesita un [dispositivo VPN local compatible](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> Tiene que poder configurar el dispositivo VPN local. Tiene que tener una dirección IPv4 pública externa, y la dirección no puede encontrarse detrás de un dispositivo NAT.<br/><br/> Asegúrese de que tiene acceso a la base de datos local de SQL Server.<br/><br/> El Firewall de Windows debe poder obtener acceso al motor de base de datos de origen. [Más información](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Si hay un firewall delante de la máquina de la base de datos, agregue reglas para permitir el acceso a la base de datos y a los archivos a través del puerto 445 de SMB.<br/><br/> Las credenciales usadas para conectar con la instancia de SQL Server de origen y la instancia administrada de destino tienen que ser miembros del rol de servidor sysadmin.<br/><br/> Necesita un recurso compartido de red en la base de datos local que GMS pueda usar para hacer copia de seguridad de la base de datos de origen.<br/><br/> Asegúrese de que la cuenta de servicio que ejecuta la instancia de SQL Server de origen tenga privilegios de escritura sobre el recurso compartido de red.<br/><br/> Anote un usuario de Windows (y una contraseña) que tenga privilegio de control total sobre el recurso compartido de red. Azure Database Migration Service suplanta estas credenciales de usuario para cargar los archivos de copia de seguridad en el contenedor de Azure Storage.<br/><br/> El proceso de instalación de SQL Server Express establece el protocolo TCP/IP en **deshabilitado** de forma predeterminada. Asegúrese de que esté habilitado.


## <a name="scenario-steps"></a>Pasos del escenario

Contoso ejecutará la migración de la forma siguiente:

> [!div class="checklist"]
> * **Paso 1: creación de VM con SQL Server en Azure**: para lograr alta disponibilidad, Contoso quiere implementar una base de datos en clúster en Azure. Implementa dos VM con SQL Server y un equilibrador de carga interno de Azure.
> * **Paso 2: implementación del clúster**: después de implementar las VM de SQL Server, prepara un clúster de servidores de Azure SQL.  Migrará su base de datos en este clúster creado previamente.
> * **Paso 3: preparación de DMS**: para preparar DMS, registra el proveedor de migración de base de datos, crea una instancia de DMS y un proyecto. Configura una firma de acceso compartido (SAS) para su identificador uniforme de recursos (URI). DMS usa el URI de SA para acceder al contenedor de la cuenta de almacenamiento a la que el servicio carga los archivos de copia de seguridad de SQL Server.
> * **Paso 4: preparación de Azure para Site Recovery**: crea una cuenta de Azure Storage que contiene los datos replicados, así como un almacén de Recovery Services.
> * **Paso 5: preparación de VMware local para Site Recovery**: prepara las cuentas para la detección de VM y la instalación del agente, así como las VM locales para que puedan conectarse a las VM de Azure tras una conmutación por error.
> * **Paso 6: replicación de VM**: establece la configuración de replicación y habilita la replicación de la VM.
> * **Paso 7: migración de la base de datos con DMS**: migra la base de datos.
> * **Paso 8: migración de las VM con Site Recovery**: ejecuta una conmutación por error de prueba para garantizar que todo funciona y, a continuación, se realiza una conmutación por error completa para migrar la VM.


## <a name="step-1-prepare-a-sql-server-alwayson-availability-group-cluster"></a>Paso 1: preparación de un clúster del grupo de disponibilidad Always On de SQL Server

Contoso quiere planearlo con antelación mediante la implementación de la base de datos en un clúster de Azure. Posteriormente, podrá utilizar este clúster para otras bases de datos. 

- Las VM de SQL Server se implementarán en el grupo de recursos de ContosoRG (utilizado para los recursos de producción).
- A parte de los nombres únicos, ambas máquinas virtuales utilizan la misma configuración.
- El equilibrador de carga interno se implementará en ContosoNetworkingRG (utilizado para los recursos de red).
- Las VM ejecutarán Windows Server 2016 con SQL Server 2017 Enterprise Edition. Contoso no tiene licencias para este sistema operativo, por lo que usará una imagen en Azure Marketplace que proporciona la licencia como un cargo a su compromiso con Azure para Contrato Enterprise.

Contoso configura el clúster de la siguiente manera:

1. Crea dos VM de SQL Server mediante la selección de la imagen de Windows Server 2016 de SQL Server 2017 Enterprise en Azure Marketplace. 

    ![SKU de la VM de SQL](media/contoso-migration-rehost-vm-sql-ag/sql-vm-sku.png)

2. En el **Asistente para crear máquinas virtuales** > **Conceptos básicos**, configura:

    - Nombres para las máquinas virtuales: **SQLAOG1** y **SQLAOG2**.
    - Dado que las máquinas son críticas para la empresa, habilita SSD para el tipo de disco de VM.
    - Especifica las credenciales de la máquina.
    - Implementa las VM en la región Este de EE. UU. 2 principal, en el grupo de recursos de ContosoRG.

3. En la opción **Tamaño**, comienza con la SKU D2s_V3 para ambas VM. Posteriormente, se escalarán, puesto que lo necesitan.
4. En **Configuración**, Contoso hace lo siguiente:

    - Dado que estas VM son bases de datos críticas para la aplicación, usa discos administrados.
    - Coloca las máquinas en la red de producción de la región principal de Este de EE. UU. 2 (**VNET-PROD-EUS2**) de la subred de la base de datos (**PROD-DB-EUS2**).
    - Crea un nuevo conjunto de disponibilidad, denominado **SQLAOGAVSET**, con dos dominios de error y cinco dominios de actualización.

    ![VM con SQL](media/contoso-migration-rehost-vm-sql-ag/sql-vm-settings.png)

4. En **Configuración de SQL Server**, limita la conectividad de SQL a la red virtual (privada) del puerto 1433 predeterminado. Para la autenticación, usa las mismas credenciales que utiliza in situ (**contosoadmin**).

    ![VM con SQL](media/contoso-migration-rehost-vm-sql-ag/sql-vm-db.png)

**¿Necesita más ayuda?**

- [Obtenga ayuda](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-server-provision#1-configure-basic-settings) para aprovisionar una VM con SQL Server.
- [Averigüe](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-prereq#create-sql-server-vms) cómo puede configurar VM para distintas SKU de SQL Server.

## <a name="step-2-deploy-the-failover-cluster"></a>Paso 2: implementación del clúster de conmutación por error

Contoso configura el clúster de la siguiente manera:

1. Configura una cuenta de Azure Storage para que actúe como testigo en la nube.
2. Agrega las VM con SQL Server en el dominio de Active Directory del centro de datos local de Contoso.
3. Crea el clúster en Azure.
4. Configura al testigo de la nube.
5. Finalmente, habilita Grupos de disponibilidad Always On de SQL.

### <a name="set-up-a-storage-account-as-cloud-witness"></a>Configuración de una cuenta de almacenamiento como testigo en la nube

Para configurar un testigo en la nube, Contoso necesita una cuenta de Azure Storage que contendrá el archivo de blob que se usa para el arbitraje de clúster. La misma cuenta de almacenamiento puede utilizarse para configurar el testigo en la nube para varios clústeres. 

Contoso crea una cuenta de almacenamiento de la manera siguiente:

1. Especifica un nombre reconocible para la cuenta (**contosocloudwitness**).
2. Implementa una cuenta general para todos los fines con LRS.
3. Coloca la cuenta en una región de terceros: Centro-sur de EE. UU. La coloca fuera de la región principal y secundaria para que permanezca disponible si se produce un error regional.
4. La coloca en el grupo de recursos que contiene los recursos de infraestructura: **ContosoInfraRG**.

    ![Testigo en la nube](media/contoso-migration-rehost-vm-sql-ag/witness-storage.png)

5. Al crear la cuenta de almacenamiento, se generan las claves de acceso principal y secundaria. La clave de acceso principal es necesaria para crear al testigo en la nube. La clave aparece bajo el nombre de la cuenta de almacenamiento > **Claves de acceso**.

    ![Clave de acceso](media/contoso-migration-rehost-vm-sql-ag/access-key.png)

### <a name="add-sql-server-vms-to-contoso-domain"></a>Adición de las VM con SQL Server al dominio de Contoso

1. Contoso agrega SQLAOG1 y SQLAOG2 al dominio contoso.com.
2. A continuación, en cada VM instala la característica de clúster de conmutación por error y las herramientas de Windows.

## <a name="set-up-the-cluster"></a>Configuración del clúster

Antes de configurar el clúster, Contoso toma una instantánea de disco del sistema operativo en cada máquina.

![instantánea](media/contoso-migration-rehost-vm-sql-ag/snapshot.png)

2. A continuación, ejecuta un script que ha preparado para crear el clúster de conmutación por error de Windows.

    ![Crear clúster](media/contoso-migration-rehost-vm-sql-ag/create-cluster1.png)

3. Después de crear el clúster, verifica que las VM aparecen como nodos de clúster.

     ![Crear clúster](media/contoso-migration-rehost-vm-sql-ag/create-cluster2.png)

## <a name="configure-the-cloud-witness"></a>Configuración del testigo en la nube

1. Contoso configura el testigo en la nube mediante **Quorum Configuration Wizard** (Asistente de configuración de cuórum) en el Administrador de clústeres de conmutación por error.
2. En el asistente, selecciona la opción para crear a un testigo en la nube con la cuenta de almacenamiento.
3. Después de configurar el testigo en la nube, aparece el complemento Administrador de clústeres de conmutación por error.

    ![Testigo en la nube](media/contoso-migration-rehost-vm-sql-ag/cloud-witness.png)
            

## <a name="enable-sql-server-always-on-availability-groups"></a>Habilitación de Grupos de disponibilidad Always On de SQL Server

Contoso ahora puede habilitar Always On:

1. En el Administrador de configuración de SQL Server, habilita **Grupos de disponibilidad Always On** para el servicio **SQL Server (MSSQLSERVER)**.

    ![Habilitación de Always On](media/contoso-migration-rehost-vm-sql-ag/enable-alwayson.png)

2. Reinicia el servicio para que los cambios surtan efecto.

Con Always On habilitado, Contoso puede configurar el grupo de disponibilidad Always On que protegerá la base de datos de SmartHotel.

**¿Necesita más ayuda?**

- [Obtenga información sobre](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness) el testigo en la nube y sobre cómo configurar una cuenta de almacenamiento para este.
- [Obtenga instrucciones](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial) para configurar un clúster y crear un grupo de disponibilidad.

## <a name="step-3-deploy-the-azure-load-balancer"></a>Paso 3: obtención de Azure Load Balancer

Ahora, Contoso quiere implementar un equilibrador de carga interno que se ubique delante de los nodos del clúster. El equilibrador de carga escucha el tráfico y lo dirige al nodo adecuado.

![Equilibrio de carga](media/contoso-migration-rehost-vm-sql-ag/architecture-lb.png)

Crea un equilibrador de carga de la manera siguiente:

1. En Azure Portal > **Redes** > **Load Balancer**, configura un equilibrador de carga interno nuevo: **ILB-PROD-DB-EUS2-SQLAOG**.
2. Coloca el equilibrador de carga en la red de producción (**VNET-PROD-EUS2**) de la subred de la base de datos (**PROD-DB-EUS2**).
3. Le asigna una dirección IP estática: 10.245.40.100.
4. Como elemento de la red, implementa el equilibrador de carga en el grupo de recursos de red **ContosoNetworkingRG**.

    ![Equilibrio de carga](media/contoso-migration-rehost-vm-sql-ag/lb-create.png)

Una vez implementado el equilibrador de carga interno, Contoso debe configurarlo. Crea un grupo de direcciones de back-end, configura un sondeo de estado y establece una regla de equilibrio de carga.

### <a name="add-a-backend-pool"></a>Adición de un grupo de back-end

Para distribuir el tráfico a las VM del clúster, Contoso configura un grupo de direcciones de back-end que contiene las direcciones IP de la NIC para las VM que van a recibir tráfico de red del equilibrador de carga.

1. En la configuración del equilibrador de carga del portal, Contoso agrega un grupo de back-end: **ILB-PROD-DB-EUS-SQLAOG-BEPOOL**.
2. Asocia el grupo con el conjunto de disponibilidad SQLAOGAVSET. Las VM del conjunto (**SQLAOG1** y **SQLAOG2**) se agregan al grupo.

    ![Grupo back-end](media/contoso-migration-rehost-vm-sql-ag/backend-pool.png)

### <a name="create-a-health-probe"></a>Creación de un sondeo de estado

Para permitir que el equilibrador de carga supervise el estado de la aplicación, Contoso crea un sondeo de estado. El sondeo agrega o quita de forma dinámica las VM de la rotación del equilibrador de carga en función de cómo responden a las comprobaciones de estado.

El sondeo se crea como se muestra a continuación: 

1. En la configuración del equilibrador de carga del portal, Contoso crea un sondeo de estado denominado **SQLAlwaysOnEndPointProbe**.
2. Lo establece para supervisar las VM en el puerto TCP 59999.
3. Establece un intervalo de 5 segundos entre sondeos y un umbral de 2. Si se produce un error en los dos sondeos, se considerará que el estado de la VM es incorrecto.

    ![Sondeo](media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

### <a name="configure-the-load-balancer-to-receive-traffic"></a>Configuración del equilibrador de carga para recibir tráfico


Ahora, Contoso necesita una regla del equilibrador de carga que defina cómo se distribuye el tráfico a las VM.

- La dirección IP de front-end controla el tráfico entrante.
- El grupo de IP de back-end recibe el tráfico.

Crea el sondeo como se muestra a continuación:

1. En la configuración del equilibrador de carga del portal, agrega una nueva regla de equilibrio de carga denominada **SQLAlwaysOnEndPointListener**.
2. Contoso establece un agente de escucha de front-end para que reciba el tráfico entrante del cliente de SQL en TCP 1433.
3. Especifica el grupo de back-end al que se enrutará el tráfico y el puerto en el que las VM escuchan el tráfico.
4. Contoso habilita la IP flotante (Direct Server Return). Esto siempre es necesario para AlwaysOn de SQL.

    ![Sondeo](media/contoso-migration-rehost-vm-sql-ag/nlb-probe.png)

**¿Necesita más ayuda?**

- [Obtenga información general](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview)sobre Azure Load Balancer.
- [Averigüe](https://docs.microsoft.com/azure/load-balancer/tutorial-load-balancer-basic-internal-portal) cómo crear un equilibrador de carga.



## <a name="step-4-prepare-azure-for-the-site-recovery-service"></a>Paso 4: preparación de Azure para el servicio de Site Recovery

Estos son los componentes de Azure que Contoso necesita para implementar Site Recovery:

- Una red virtual en la que ubicar las máquinas virtuales cuando se creen durante la conmutación por error.
- Una cuenta de Azure Storage para almacenar los datos replicados. 
- Un almacén de Recovery Services en Azure.

Se deben configurar como se muestra a continuación:

1.  Contoso ya creó una red o una subred que puede utilizar para Site Recovery cuando [implementó la infraestructura de Azure](contoso-migration-rehost-vm-sql-ag.md).

    - La aplicación de SmartHotel es una aplicación de producción y WEBVM se migrará a la red de producción de Azure (VNET-PROD-EUS2) de la región principal de Este de EE. UU. 2.
    - WEBVM se colocará en el grupo de recursos de ContosoRG, que se utiliza para los recursos de producción, y en la subred de producción (PROD-FE-EUS2).

2. Contoso crea una cuenta de Azure Storage (contosovmsacc20180528) en la región principal.

    - Usa una cuenta de uso general con almacenamiento estándar y la replicación de LRS.
    - La cuenta debe estar en la misma región que el almacén.

    ![Almacenamiento de Site Recovery](media/contoso-migration-rehost-vm-sql-ag/asr-storage.png)

3. Con la cuenta de almacenamiento y la red implementados, Contoso crea un almacén de Recovery Services (**ContosoMigrationVault**) y lo coloca en el grupo de recursos **ContosoFailoverRG** de la región principal del Este de EE. UU. 2.

    ![Almacén de Recovery Services](media/contoso-migration-rehost-vm-sql-ag/asr-vault.png)

**¿Necesita más ayuda?**

[Más información](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) sobre cómo configurar Azure para Site Recovery.


## <a name="step-4-prepare-on-premises-vmware-for-site-recovery"></a>Paso 4: Preparación de VMware local para Site Recovery

Esto es lo que Contoso prepara en el entorno local:

- Una cuenta en el host de vCenter Server o vSphere ESXi para automatizar la detección de VM.
- Una cuenta que permita la instalación automática de Mobility Service en cada VM de VMware que quiera replicar.
- Configuración de la VM local, para que Contoso puede conectarse a la VM de Azure replicada después de la conmutación por error.


### <a name="prepare-an-account-for-automatic-discovery"></a>Preparación de una cuenta de detección automática

Site Recovery necesita acceso a los servidores de VMware para:

- Detectar automáticamente las máquinas virtuales. 
- Coordinar la replicación, la conmutación por error y la conmutación por recuperación.
- Se requiere al menos una cuenta de solo lectura. Necesita una cuenta que pueda ejecutar operaciones como la creación y eliminación de discos, así como la activación de máquinas virtuales.

Contoso configura la cuenta como se indica a continuación:

1. Crea un rol en el nivel de vCenter.
2. A continuación, asigna los permisos necesarios a ese rol.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Preparación de una cuenta para la instalación de Mobility Service

Mobility Service debe instalarse en cada VM.

- Site Recovery puede hacer una instalación de inserción automática de este componente cuando se habilita la replicación para la VM.
- Para la instalación de inserción, necesita una cuenta que Site Recovery pueda utilizar para acceder a la VM. Deberá especificar esta cuenta al configurar la replicación en la consola de Azure.
- La cuenta puede ser de dominio o local con permisos para hacer instalaciones en la VM.




### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Preparación para la conexión a las máquinas virtuales de Azure después de la conmutación por error

Tras la conmutación por error, Contoso quiere conectarse a las VM de Azure. Para ello, hace lo siguiente antes de la migración:

1. Para el acceso a través de Internet:

 - Habilitar RDP en la VM local antes de la conmutación por error
 - Se asegura de que se agregan las reglas TCP y UDP para el perfil **público**.
 - Comprueba que se permite RDP en **Firewall de Windows** > **Aplicaciones permitidas** para todos los perfiles.
 
2. Para el acceso a través de VPN de sitio a sitio:

 - Habilita RDP en la máquina local.
 - Permite RDP en **Firewall de Windows** -> **Aplicaciones y características permitidas** para las redes de **dominio y privadas**.
 - Establece la directiva SAN del sistema operativo de la VM local en **OnlineAll**.

Además, cuando ejecuta una conmutación por error, Contoso debe comprobar lo siguiente:

- No debe haber actualizaciones de Windows pendientes en la VM cuando se desencadene una conmutación por error. Si las hay, no podrá iniciar sesión en la VM hasta que se completen las actualizaciones.
- Después de la conmutación por error, puede comprobar los **diagnósticos de arranque** para ver una captura de pantalla de la VM. Si no funciona, debe comprobar que la VM está en ejecución, así como revisar estas [sugerencias de solución de problemas](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**¿Necesita más ayuda?**

- [Averigüe](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) cómo puede crear y asignar un rol para la detección automática.
- [Averigüe](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) cómo puede crear una cuenta para la instalación de inserción de Mobility Service.


## <a name="step-5-replicate-the-on-premises-vms-to-azure-with-site-recovery"></a>Paso 5: Replicación de las VM locales a Azure con Site Recovery

Para poder ejecutar una migración a Azure, Contoso debe configurar y habilitar la replicación.

### <a name="set-a-replication-goal"></a>Establecimiento de un objetivo de replicación

1. En el almacén, en el nombre del almacén (ContosoVMVault), selecciona un objetivo de replicación (**Introducción** > **Site Recovery** > **Preparar infraestructura**.
2. Especifica que sus máquinas estén ubicadas en el entorno local, se ejecuten en VMware y se repliquen en Azure.

    ![Objetivo de replicación](./media/contoso-migration-rehost-vm-sql-ag/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Confirmación del planeamiento de la implementación

Para continuar, debe confirmar que ha completado la planificación de implementación seleccionando la opción **Sí, ya lo hice**. En este caso, Contoso solo va a migrar una VM, por lo que no es necesario planear la implementación.

### <a name="set-up-the-source-environment"></a>Configuración del entorno de origen

Contoso debe configurar el entorno de origen. Para ello, descarga una plantilla OVF y la usa para implementar el servidor de configuración de Site Recovery como una VM local de VMware altamente disponible. Una vez que el servidor de configuración se encuentra en funcionamiento, lo registra en el almacén.

El servidor de configuración ejecuta una serie de componentes:

- El componente del servidor de configuración que coordina las comunicaciones entre el entorno local y Azure y administra la replicación de datos.
- El servidor de procesos que actúa como puerta de enlace de replicación. Recibe datos de replicación, los optimiza con almacenamiento en caché, compresión y cifrado, y los envía a Azure Storage.
- El servidor de procesos también instala Mobility Service en las máquinas virtuales que quiere replicar, además de realizar la detección automática de las máquinas virtuales de VMware locales.

Contoso realiza estos pasos como se indica a continuación:


1. En el almacén, descarga la plantilla OVF desde **Preparar infraestructura** > **Origen** > **Servidor de configuración**.
    
    ![Descarga de OVF](./media/contoso-migration-rehost-vm-sql-ag/add-cs.png)

2. Importa la plantilla en VMware para crear e implementar la VM.

    ![Plantilla de OVF](./media/contoso-migration-rehost-vm-sql-ag/vcenter-wizard.png)

3. Al activar la VM por primera vez, se inicia en una experiencia de instalación de Windows Server 2016. Acepta el contrato de licencia e introduce una contraseña de administrador.
4. Una vez finalizada la instalación, inicia sesión en la VM como administrador. Cuando se inicia sesión por primera vez, se ejecuta la herramienta de configuración de Azure Site Recovery de manera predeterminada.
5. En la herramienta, especifica un nombre que se usará para registrar el servidor de configuración en el almacén.
6. La herramienta comprueba que la máquina virtual pueda conectarse a Azure. Una vez establecida la conexión, inicia sesión en la suscripción a Azure. Las credenciales deben tener acceso al almacén donde desea registrar el servidor de configuración.

    ![Registrar servidor de configuración](./media/contoso-migration-rehost-vm-sql-ag/config-server-register2.png)

7. La herramienta realiza algunas tareas de configuración y, a continuación, se reinicia.
8. Vuelve a iniciar sesión en la máquina y el Asistente para administración del servidor de configuración se inicia automáticamente.
9. En el asistente, selecciona la NIC para recibir tráfico de replicación. Una vez configurada, esta opción no se puede cambiar.
10. Selecciona la suscripción, el grupo de recursos y el almacén en el que se va a registrar el servidor de configuración.
        ![almacén](./media/contoso-migration-rehost-vm-sql-ag/cswiz1.png) 

10. A continuación, descarga e instala MySQL Server y VMWare PowerCLI. 
11. Después de la validación, especifica la dirección IP o el FQDN del host de vSphere o vCenter Server. Deja el puerto predeterminado y especifica un nombre descriptivo para vCenter Server.
12. Especifica la cuenta que se ha creado para la detección automática, así como las credenciales que se utilizarán para instalar automáticamente Mobility Service. Para las máquinas de Windows, la cuenta necesita privilegios de administrador local en las VM.

    ![vCenter](./media/contoso-migration-rehost-vm-sql-ag/cswiz2.png)

7. Cuando finaliza el registro, en Azure Portal, Contoso verifica si el servidor de configuración y VMware Server aparecen en la página **Origen** del almacén. La detección puede tardar 15 minutos o más. 
8. Luego, Site Recovery se conecta a los servidores de VMware con la configuración especificada y detecta las VM.

### <a name="set-up-the-target"></a>Configuración del destino

Ahora, Contoso especifica la configuración de replicación de destino.

1. En **Preparar infraestructura** > **Destino**, selecciona la configuración de destino.
2. Site Recovery comprueba si existe una red y una cuenta de almacenamiento de Azure en el destino especificado.

### <a name="create-a-replication-policy"></a>Creación de una directiva de replicación

No, Contoso puede crear una directiva de replicación.

1. En **Preparar infraestructura** > **Configuración de la replicación** > **Directiva de replicación** >  **Crear y asociar**, crea la directiva **ContosoMigrationPolicy**.
2. Usa la configuración predeterminada:
    - **Umbral de RPO**: valor predeterminado de 60 minutos. Este valor define la frecuencia de creación de puntos de recuperación. Se genera una alerta cuando la replicación continua supera este límite.
    - **Retención de punto de recuperación**. Valor predeterminado de 24 horas. Este valor especifica cuánto tiempo dura el período de retención para cada punto de recuperación. Las máquinas virtuales replicadas se pueden recuperar a cualquier momento de un período.
    - **Frecuencia de las instantáneas coherentes con la aplicación**. Valor predeterminado de una hora. Este valor especifica la frecuencia con la que se crean instantáneas coherentes con la aplicación.
 
        ![Creación de la directiva de replicación](./media/contoso-migration-rehost-vm-sql-ag/replication-policy.png)

5. La directiva se asocia automáticamente al servidor de configuración. 

    ![Asociación de la directiva de replicación](./media/contoso-migration-rehost-vm-sql-ag/replication-policy2.png)



### <a name="enable-replication"></a>Habilitar replicación

Contoso ya puede empezar a replicar WebVM.

1. En **Replicar aplicación** > **Origen** > **+Replicar**, selecciona la configuración de origen.
2. Indica que quiere habilitar las VM, seleccionar el servidor de vCenter y selecciona el servidor de configuración.

    ![Habilitar replicación](./media/contoso-migration-rehost-vm-sql-ag/enable-replication1.png)

3. Luego, especifica la configuración de destino, incluidos el grupo de recursos y una red virtual, así como una cuenta de almacenamiento en la que se almacenarán los datos replicados.

     ![Habilitar replicación](./media/contoso-migration-rehost-vm-sql-ag/enable-replication2.png)

3. Contoso selecciona WebVM para la replicación, comprueba la directiva de replicación y habilita la replicación. Site Recovery instala Mobility Service en la VM cuando se habilita la replicación.
 
    ![Habilitar replicación](./media/contoso-migration-rehost-vm-sql-ag/enable-replication3.png)

4. Realiza un seguimiento del progreso de la replicación en **Trabajos**. La máquina estará preparada para la conmutación por error después de que finalice el trabajo **Finalizar la protección**.
5. En **Essentials** en Azure Portal, Contoso puede ver la estructura para las VM que se replican en Azure.

    ![Vista de la infraestructura](./media/contoso-migration-rehost-vm-sql-ag/essentials.png)


**¿Necesita más ayuda?**

- Puede leer un tutorial completo de todos estos pasos en [Configurar la recuperación ante desastres para VM de VMware locales](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Puede encontrar instrucciones detalladas que le ayudarán a [configurar el entorno de origen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [implementar el servidor de configuración](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server) y [establecer la configuración de replicación](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- Puede obtener más información sobre la [habilitación de la replicación](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-5-install-the-database-migration-assistant-dma"></a>Paso 5: instalación de Database Migration Assistant (DMA)

Contoso migrará la base de datos de SmartHotel a la VM de Azure **SQLAOG1** mediante DMA. Para ello, configura DMA como se muestra a continuación:

1. Descarga la herramienta desde el [Centro de descarga de Microsoft](https://www.microsoft.com/download/details.aspx?id=53595) en la VM local de SQL Server (**SQLVM**).
2. Ejecuta el programa de instalación (DownloadMigrationAssistant.msi) en la VM.
3. En la página **Finalizar**, selecciona **Launch Microsoft Data Migration Assistant** (Iniciar Microsoft Data Migration Assistant) antes de finalizar el asistente.

## <a name="step-6-migrate-the-database-with-dma"></a>Paso 6: migración de la base de datos con DMA

1. En DMA, Contoso ejecuta una nueva migración: **SmartHotel**.
2. Para la opción **Target server type** (Tipo de servidor de destino), selecciona **SQL Server on Azure Virtual Machines** (SQL Server en máquinas virtuales de Azure). 

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-1.png)

3. En los detalles de la migración, agrega **SQLVM** como servidor de origen, y **SQLAOG1** como el de destino. Especifica las credenciales para cada máquina.

     ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-2.png)

4. Crea un recurso compartido local para la información de la base de datos y la configuración. Debe estar accesible mediante el acceso de escritura con la cuenta de servicio de SQL en SQLVM y SQLAOG1.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-3.png)

5. Contoso selecciona los inicios de sesión que se deben migrar e inicia la migración. Una vez finalizada, DMA muestra que la migración se realizó correctamente.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-4.png)

6. Verifica que la base de datos se ejecuta en **SQLAOG1**.

    ![DMA](media/contoso-migration-rehost-vm-sql-ag/dma-5.png)

DMS se conecta a la VM de SQL Server local a través de una conexión de VPN de sitio a sitio entre el centro de datos de Contoso y Azure y, a continuación, migra la base de datos.

## <a name="step-7-protect-the-database"></a>Paso 7: protección de la base de datos

Una vez que la base de datos de la aplicación se ejecuta en **SQLAOG1**, Contoso ahora puede usar Grupos de disponibilidad AlwaysOn para protegerla. Configura Always On con SQL Management Studio y, a continuación, asigna un agente de escucha mediante la agrupación en clústeres de Windows. 

### <a name="create-an-alwayson-availability-group"></a>Creación de un grupo de disponibilidad Always On

1. En SQL Management Studio, hace doble clic en **Alta disponibilidad de Always On** para iniciar **New Availability Group Wizard** (Asistente para nuevo grupo de disponibilidad).
2. En **Especificar opciones**, asigna el nombre **SHAOG** al grupo de disponibilidad. En **Seleccionar bases de datos**, selecciona la base de datos de SmartHotel.

    ![Grupo de disponibilidad Always On](media/contoso-migration-rehost-vm-sql-ag/aog-1.png)

3. En **Especificar réplicas**, agrega los dos nodos de SQL como réplicas de disponibilidad y los configura para proporcionar la conmutación por error automática con confirmación sincrónica.

     ![Grupo de disponibilidad Always On](media/contoso-migration-rehost-vm-sql-ag/aog-2.png)

4. Configura un agente de escucha para el grupo (**SHAOG**) y el puerto. La dirección IP del equilibrador de carga interno se agrega como una dirección IP estática (10.245.40.100).

    ![Grupo de disponibilidad Always On](media/contoso-migration-rehost-vm-sql-ag/aog-3.png)

5. En **Seleccionar sincronización de datos**, permite la inicialización automática. Con esta opción, SQL Server crea automáticamente las réplicas secundarias para cada base de datos del grupo, por lo que Contoso no debe realizar manualmente la copia de seguridad ni restaurarla. Después de la validación, se crea el grupo de disponibilidad.

    ![Grupo de disponibilidad Always On](media/contoso-migration-rehost-vm-sql-ag/aog-4.png)

6. Contoso tuvo un problema al crear el grupo. Dado que no usa la seguridad integrada de Windows de Active Directory, debe conceder permisos para el inicio de sesión de SQL a fin de crear los roles de clúster de conmutación por error de Windows.

    ![Grupo de disponibilidad Always On](media/contoso-migration-rehost-vm-sql-ag/aog-5.png)

6. Una vez creado el grupo, Contoso puede verlo en SQL Management Studio.

### <a name="configure-a-listener-on-the-cluster"></a>Configuración de un agente de escucha en el clúster

Como último paso de la configuración de implementación de SQL, Contoso configura el equilibrador de carga interno como el agente de escucha del clúster y conecta el agente de escucha. Para ello, usa un script.

![Agente de escucha del clúster](media/contoso-migration-rehost-vm-sql-ag/cluster-listener.png)


### <a name="verify-the-configuration"></a>Comprobar la configuración

Con todo configurado, Contoso ahora tiene un grupo de disponibilidad funcional en Azure que utiliza la base de datos migrada. Para comprobarlo, puede conectarse al equilibrador de carga interno de SQL Management Studio.

![Conexión de ILB](media/contoso-migration-rehost-vm-sql-ag/ilb-connect.png)

**¿Necesita más ayuda?**
- Obtenga información acerca de cómo crear un [grupo de disponibilidad](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#create-the-availability-group) y un [agente de escucha](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-availability-group-tutorial#configure-listener).
- [Configure el clúster manualmente para que utilice la dirección IP del equilibrador de carga](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-portal-sql-alwayson-int-listener#configure-the-cluster-to-use-the-load-balancer-ip-address).
- [Obtenga más información](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2) sobre cómo crear y usar SAS.


## <a name="step-8-migrate-the-vm-with-site-recovery"></a>Paso 8: migración de la VM con Site Recovery

Contoso ejecuta una conmutación por error rápida de prueba y, a continuación, migra la VM.

### <a name="run-a-test-failover"></a>Ejecución de una conmutación por error de prueba

La ejecución de una conmutación por error de prueba ayuda a garantizar que todo funciona según lo esperado antes de la migración. 

1. Contoso ejecuta una conmutación por error de prueba en el punto más reciente en el tiempo disponible (**procesado más recientemente**).
2. Selecciona **Apague la máquina antes de comenzar con la conmutación por error**, por lo que Site Recovery intenta apagar la VM de origen antes de desencadenar la conmutación por error. La conmutación por error continúa aunque se produzca un error de cierre. 
3. Se ejecuta la conmutación por error de prueba: 

    - Se ejecuta una comprobación de requisitos previos para garantizar que todas las condiciones necesarias para la conmutación por error están correctamente establecidas.
    - La conmutación por error procesa los datos, de modo que se pueda crear una máquina virtual de Azure. Si se selecciona el último punto de recuperación, se crea un punto de recuperación a partir de los datos.
    - Se crea una VM de Azure mediante los datos procesados en el paso anterior.

3. Una vez finalizada la conmutación por error, la VM de Azure de réplica aparece en Azure Portal. Contoso verifica que la VM tiene el tamaño adecuado, que está conectada a la red correspondiente y que se está ejecutando. 
4. Después, limpia la conmutación por error y registra y guarda las observaciones. 

### <a name="run-a-failover"></a>Ejecución de la conmutación por error

1. Después de verificar que la conmutación por error de prueba ha funcionado según lo esperado, Contoso crea un plan de recuperación para la migración y agrega WEBVM al plan.

     ![Plan de recuperación](./media/contoso-migration-rehost-vm-sql-ag/recovery-plan.png)

2. Ejecuta una conmutación por error en el plan. Selecciona el punto de recuperación más reciente y especifica que Site Recovery debe intentar apagar la VM local antes de desencadenar la conmutación por error.

    ![Conmutación por error](./media/contoso-migration-rehost-vm-sql-ag/failover1.png)

3. Después de la conmutación por error, Contoso comprueba si la VM de Azure aparece según lo esperado en Azure Portal.

    ![Plan de recuperación](./media/contoso-migration-rehost-vm-sql-ag/failover2.png)

6. Después de verificar la VM en Azure, completa la migración para finalizar el proceso de migración, detiene la replicación para la VM y detiene la facturación de Site Recovery de la VM.

    ![Conmutación por error](./media/contoso-migration-rehost-vm-sql-ag/failover3.png)

### <a name="update-the-connection-string"></a>Actualización de la cadena de conexión

Como último paso del proceso de migración, Contoso actualiza la cadena de conexión de la aplicación para que apunte a la base de datos migrada que se ejecuta en el agente de escucha SHAOG. Esta configuración se cambiará en WEBVM, que ahora se ejecuta en Azure.  Esta configuración se encuentra en el archivo web.config de la aplicación ASP. 

1. Busca el archivo en C:\inetpub\SmartHotelWeb\web.config.  Cambia el nombre del servidor para reflejar el FQDN de AOG: shaog.contoso.com.

    ![Conmutación por error](./media/contoso-migration-rehost-vm-sql-ag/failover4.png)  

2. Después de actualizar el archivo y guardarlo, reinicia IIS en WEBVM. Para ello, usa IISRESET /RESTART desde un símbolo del sistema.
2. Después de reiniciar IIS, la aplicación usará ahora la base de datos que se ejecuta en la Instancia administrada de SQL.


**¿Necesita más ayuda?**

- [Más información](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) sobre la ejecución de una conmutación por error de prueba. 
- [Más información](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) sobre cómo crear un plan de recuperación.
- [Más información](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) sobre cómo crear una conmutación por error en Azure.

## <a name="clean-up-after-migration"></a>Limpieza después de la migración

Tras la migración, la aplicación SmartHotel se ejecuta en una VM de Azure y la base de datos de SmartHotel se ubica en el clúster de Azure SQL.

Ahora, Contoso debe completar estos pasos de limpieza:  

- Quitar las VM locales del inventario de vCenter.
- Quitar las VM de los trabajos de copia de seguridad locales.
- Actualizar la documentación interna para que muestre las nuevas ubicaciones y las direcciones IP de las VM.
- Revisar los recursos que interactúan con las VM retiradas y actualizar los valores de configuración pertinentes o la documentación para reflejar la nueva configuración.
- Las dos máquinas virtuales nuevas (SQLAOG1 y SQLAOG2) deben agregarse a los sistemas de supervisión de producción.

## <a name="review-the-deployment"></a>Revisión de la implementación

Con los recursos migrados de Azure, Contoso debe proteger la infraestructura nueva y ponerla totalmente en marcha.

### <a name="security"></a>Seguridad

El equipo de seguridad de Contoso revisa las VM de Azure WEBVM, SQLAOG1 y SQLAOG2 para determinar si hay problemas de seguridad. 

- Revisa los grupos de seguridad de red (NSG) de la VM con el fin de controlar el acceso. Los NSG se usan para garantizar que solo el tráfico permitido puede pasar a la aplicación.
- Contoso también considera la posibilidad de proteger los datos de los discos mediante Azure Disk Encryption y KeyVault.
- Debe evaluar el Cifrado de datos transparente (TDE) y, a continuación, habilitarlo en la base de datos de SmartHotel que se ejecuta en el nuevo AOG de SQL. [Más información](https://docs.microsoft.com/sql/relational-databases/security/encryption/transparent-data-encryption?view=sql-server-2017).

Obtenga [más información](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) sobre los procedimientos de seguridad recomendados de las VM.

### <a name="backups"></a>Copias de seguridad

Contoso va a realizar una copia de seguridad de los datos de WEBVM, SQLAOG1 y SQLAOG2 mediante el servicio Azure Backup. [Más información](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Optimización de los costos y licencias

1. Actualmente, Contoso tiene licencia para WEBVM y aprovechará la Ventaja híbrida de Azure.  Convertirá las VM de Azure existentes para sacar provecho de los precios.
2. Contoso habilitará Azure Cost Management bajo licencia de Cloudyn, una subsidiaria de Microsoft. Es una solución de administración de costos en varias nubes que le permitirá utilizar y administrar Azure y otros recursos en la nube.  [Más información](https://docs.microsoft.com/azure/cost-management/overview) sobre Azure Cost Management. 

## <a name="conclusion"></a>Conclusión

En este artículo, Contoso rehospedó la aplicación SmartHotel en Azure mediante la migración de la VM de front-end de la aplicación a Azure con el servicio Site Recovery. Migró la base de datos de la aplicación a un clúster de SQL Server aprovisionado en Azure y la protegió en un grupo de disponibilidad Always On de SQL Server.

## <a name="next-steps"></a>Pasos siguientes

En el siguiente artículo de la serie, mostraremos cómo Contoso rehospeda su aplicación osTicket de servicio de asistencia que se ejecuta en Linux y la reimplementa con una base de datos MySQL.


