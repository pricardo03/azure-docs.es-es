---
title: Rehospedar una aplicación de Contoso en Azure mediante la migración a una VM de Azure y una Instancia administrada de Azure SQL | Microsoft Docs
description: Más información sobre cómo rehospeda Contoso una aplicación local en VM de Azure y una Instancia administrada de Azure SQL
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/13/2018
ms.author: raynew
ms.openlocfilehash: 99eda135161a228fde139458de30f5120af55153
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/11/2018
ms.locfileid: "38723617"
---
# <a name="contoso-migration-rehost-an-on-premises-app-to-azure-vms-and-azure-sql-managed-instance"></a>Migración de Contoso: rehospedar una aplicación local en VM de Azure y una Instancia administrada de Azure SQL

En este artículo se muestra cómo migra Contoso su VM front-end de la aplicación SmartHotel a VM de Azure mediante el servicio Azure Site Recovery, y la base de datos de la aplicación a una Instancia administrada de Azure SQL.

> [!NOTE]
> La Instancia administrada de Azure SQL es, actualmente, una versión preliminar.

Este documento es el cuarto de una serie de artículos que documentan cómo la compañía ficticia Contoso migra sus recursos locales a la nube de Microsoft Azure. La serie incluye información general y una serie de escenarios que ilustran cómo configurar una infraestructura de migración y ejecutar los diferentes tipos de migraciones. Los escenarios aumentan de complejidad e iremos agregando artículos adicionales a medida con el tiempo.


**Artículo** | **Detalles** | **Estado**
--- | --- | ---
[Artículo 1: Introducción](contoso-migration-overview.md) | Se proporciona una introducción a la estrategia de migración de Contoso, la serie de artículos y las aplicaciones de ejemplo que usamos. | Disponible
[Artículo 2: Deploy an Azure infrastructure](contoso-migration-infrastructure.md) (Implementación de una infraestructura de Azure) | Se describe cómo Contoso prepara su infraestructura local y de Azure para la migración. Se usa la misma infraestructura para todos los escenarios de migración de Contoso. | Disponible
[Artículo 3: Assess on-premises resources](contoso-migration-assessment.md) (Evaluación de los recursos locales)  | Se muestra cómo Contoso realiza una valoración de su aplicación de dos niveles local SmartHotel que se ejecuta en VMware. Se evalúan las VM de la aplicación mediante el servicio [Azure Migrate](migrate-overview.md) y la base de datos de SQL Server de aplicaciones con [Azure Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponible
Artículo 4: Rehospedar en VM de Azure y una Instancia administrada de SQL (este artículo) | Muestra cómo Contoso migra la aplicación SmartHotel a Azure. La VM de front-end de la aplicación se migra mediante [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) y la base de datos de aplicaciones mediante el servicio [Azure Database Migration](https://docs.microsoft.com/azure/dms/dms-overview) para migrar a una instancia administrada de SQL. | Disponible
[Artículo 5: Volver a hospedar máquinas virtuales de Azure](contoso-migration-rehost-vm.md) | Muestra cómo Contoso migra las VM de la aplicación SmartHotel solo con Site Recovery.
[Artículo 6: Rehospedar en VM de Azure y grupos de disponibilidad de SQL Server](contoso-migration-rehost-vm-sql-ag.md) | Muestra cómo migra Contoso la aplicación SmartHotel. Usa Site Recovery para migrar las VM de la aplicación y el servicio Database Migration para migrar la base de datos de aplicaciones a un grupo de disponibilidad de SQL Server. | Disponible
[Artículo 7: Rehospedaje de una aplicación Linux en VM de Azure](contoso-migration-rehost-linux-vm.md) | Se muestra cómo Contoso migra la aplicación osTicket de Linux a VM de Azure mediante Site Recovery. | Disponible
[Artículo 8: Rehospedaje de una aplicación de Linux en VM de Azure y en Azure MySQL Server](contoso-migration-rehost-linux-vm-mysql.md) | Muestra cómo Contoso migra la aplicación Linux osTicket a VM de Azure mediante Site Recovery, y a una instancia de Azure MySQL Server mediante MySQL Workbench. | Disponible

Si quiere usar la aplicación SmartHotel de ejemplo que se utiliza en este artículo, puede descargarla desde [github](https://github.com/Microsoft/SmartHotel360).

## <a name="on-premises-architecture"></a>Arquitectura local

Aquí se muestra un diagrama de la infraestructura local de Contoso en la actualidad.

![Arquitectura de Contoso](./media/contoso-migration-rehost-vm-sql-managed-instance/contoso-architecture.png)  

- Contoso tiene un centro de datos principal situado en la ciudad de Nueva York, al este de los Estados Unidos.
- Así mismo, tienen tres sucursales locales más en los Estados Unidos.
- El centro de datos principal está conectado a Internet con una conexión Metro Ethernet de fibra (500 Mbps).
- Cada una de las sucursales está conectada localmente a Internet con conexiones de clase empresarial, y con túneles VPN con IPSec hacia el centro de datos principal. Esto permite que toda la red esté conectada de forma permanente, así como optimizar la conectividad a Internet.
- El centro de datos principal está completamente virtualizado con VMware. Tiene dos hosts de virtualización de ESXi 6.5, que administra vCenter Server 6.5.
- Contoso usa Active Directory para la administración de identidades y servidores DNS en la red interna.
- Los controladores de dominio del centro de datos se ejecutan en VM de VMware. Los controladores de dominio de las sucursales locales se ejecutan en servidores físicos.



## <a name="business-drivers"></a>Impulsores de negocio

El equipo directivo de TI ha trabajado estrechamente con sus asociados comerciales para comprender lo que quieren lograr las empresas con esta migración:

- **Responder al crecimiento del negocio**: Contoso está creciendo y, como resultado, sus sistemas locales e infraestructura está bajo presión.
- **Aumentar la eficacia**: Contoso debe quitar procedimientos innecesarios y optimizar los procesos para sus desarrolladores y usuarios.  La empresa necesita que el departamento de TI sea rápido y no malgaste tiempo ni dinero a fin de satisfacer más rápidamente los requisitos del cliente.
- **Aumentar la agilidad**: el equipo de TI de Contoso necesita más capacidad de respuesta a las necesidades de la empresa. Debe poder reaccionar con más rapidez que los cambios del mercado para facilitar el éxito en una economía global.  No se debe interponer en el camino ni bloquear el negocio.
- **Escala**: a medida que el negocio crece satisfactoriamente, el equipo de TI de Contoso debe proporcionar sistemas que puedan crecer al mismo ritmo.

## <a name="migration-goals"></a>Objetivos de la migración

El equipo de la nube de Contoso ha marcado los objetivos de esta migración. Estos objetivos se utilizan para determinar el mejor método de migración:

- Tras la migración, la aplicación de Azure debe tener las mismas funcionalidades de rendimiento que tiene actualmente en el entorno local de VMWare.  Pasar a la nube no significa que el rendimiento de la aplicación sea menos crítico.
- Contoso no quiere invertir en esta aplicación.  Es crítico e importante para la empresa, pero, simplemente quiere moverla a la nube en su forma actual.
- Después de migrar la aplicación, las tareas administrativas de la base de datos deberían minimizarse.
- Contoso no quiere usar Azure SQL Database para esta aplicación y está buscando alternativas.

## <a name="proposed-architecture"></a>Arquitectura propuesta

En este escenario:

- Contoso quiere migrar su aplicación de viajes local de dos niveles.
- La aplicación se organiza en niveles en dos VM (WEBVM y SQLVM) y se encuentra en el host de VMware ESXi **contosohost1.contoso.com** (versión 6.5).
- El entorno de VMware lo administra vCenter Server 6.5 (**vcenter.contoso.com**), que se ejecuta en una VM.
- Se migra la base de datos de la aplicación (SmartHotelDB) a una Instancia administrada de Azure SQL.
- Se migran las VM de VMware locales a una VM de Azure.
- Contoso tiene un centro de datos local (contoso-datacenter), con un controlador de dominio local (**contosodc1**).
- Las VM locales del centro de datos de Contoso se retirarán después de realizar la migración.

![Arquitectura del escenario](media/contoso-migration-rehost-vm-sql-managed-instance/architecture.png) 

### <a name="azure-services"></a>Servicios de Azure

**Servicio** | **Descripción** | **Costee**
--- | --- | ---
[Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) | DMS facilita las migraciones sin problemas desde varios orígenes de base de datos a plataformas de datos de Azure, con un tiempo de inactividad mínimo. | Obtenga información acerca de las [regiones compatibles](https://docs.microsoft.com/azure/dms/dms-overview#regional-availability) con DMS y obtenga [detalles de los precios](https://azure.microsoft.com/pricing/details/database-migration/).
[Instancia administrada de Azure SQL](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) | Instancia administrada es un servicio de base de datos administrada que representa una instancia de SQL Server completamente administrada en la nube de Azure. Comparte el mismo código con la versión más reciente del motor de base de datos de SQL Server y tiene las características, mejoras de rendimiento y revisiones de seguridad más recientes. | El uso de instancias administradas de Azure SQL Database que se ejecutan en Azure incurrirá en gastos basados en la capacidad. [Más información](https://azure.microsoft.com/pricing/details/sql-database/managed/). 
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | El servicio orquesta y administra la migración y la recuperación ante desastres de las máquinas virtuales de Azure, así como las máquinas virtuales y servidores físicos locales.  | Durante la replicación en Azure, se incurre en gastos de Azure Storage.  Las máquinas virtuales de Azure se crean, e incurren en gastos, cuando se produce una conmutación por error. [Más información](https://azure.microsoft.com/pricing/details/site-recovery/) sobre cargos y precios.

 

## <a name="migration-process"></a>Proceso de migración

Contoso migrará las capas de datos y web de la aplicación SmartHotel a Azure.

1. Contoso ya ha establecido su infraestructura de Azure, por lo que solo necesita agregar un par de componentes de Azure específicos para este escenario.
2. La capa de datos se migrará con Database Migration Service (DMS).  DMS se conecta a la VM de SQL Server local a través de una conexión de VPN de sitio a sitio entre el centro de datos de Contoso y Azure y, a continuación, migra la base de datos.
3. La capa web se migrará con una migración mediante lift-and-shift y Azure Site Recovery. Esto supondrá la preparación del entorno de VMware local, la configuración y habilitación de la replicación y la migración de VM mediante conmutación por error a Azure.

     ![Arquitectura de migración](media/contoso-migration-rehost-vm-sql-managed-instance/migration-architecture.png) 


## <a name="prerequisites"></a>requisitos previos

Esto es lo que Contoso (y usted) necesitan para este escenario.

**Requisitos** | **Detalles**
--- | ---
**Inscribirse en la versión preliminar** | Es necesario estar inscrito en la versión preliminar pública limitada de la instancia administrada de SQL. También es necesaria una suscripción a Azure para [registrarse](https://portal.azure.com#create/Microsoft.SQLManagedInstance). El registro puede tardar unos días en completarse; asegúrese de que lo tiene todo listo antes de empezar a implementar este escenario.
**Suscripción de Azure** | Ya debería haber creado una suscripción cuando realizó la evaluación en el primer artículo de esta serie. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Si crea una cuenta gratuita, será el administrador de su suscripción y podrá realizar todas las acciones.<br/><br/> Si usa una suscripción existente y no es el administrador, tendrá que solicitar al administrador que le asigne permisos de propietario o colaborador.<br/><br/> Si necesita permisos más específicos, consulte [este artículo](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Recuperación del sitio (local)** | Su instancia local de vCenter Server debe ejecutarse en las versiones 5.5, 6.0 o 6.5.<br/><br/> Un host ESXi que ejecute la versión 5.5, 6.0 o 6.5<br/><br/> Una o más máquinas virtuales VMware que se ejecuten en el host ESXi.<br/><br/> Las máquinas virtuales tienen que cumplir los [requisitos de Azure](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Configuración de [red](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) y [almacenamiento](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) compatible.
**DMS** | Para DMS necesita un [dispositivo VPN local compatible](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> Tiene que poder configurar el dispositivo VPN local. Tiene que tener una dirección IPv4 pública externa, y la dirección no puede encontrarse detrás de un dispositivo NAT.<br/><br/> Asegúrese de que tiene acceso a la base de datos local de SQL Server.<br/><br/> El Firewall de Windows debe poder obtener acceso al motor de base de datos de origen. [Más información](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Si hay un firewall delante de la máquina de la base de datos, agregue reglas para permitir el acceso a la base de datos y a los archivos a través del puerto 445 de SMB.<br/><br/> Las credenciales usadas para conectar con la instancia de SQL Server de origen y la instancia administrada de destino tienen que ser miembros del rol de servidor sysadmin.<br/><br/> Necesita un recurso compartido de red en la base de datos local que GMS pueda usar para hacer copia de seguridad de la base de datos de origen.<br/><br/> Asegúrese de que la cuenta de servicio que ejecuta la instancia de SQL Server de origen tenga privilegios de escritura sobre el recurso compartido de red.<br/><br/> Anote un usuario de Windows (y una contraseña) que tenga privilegio de control total sobre el recurso compartido de red. Azure Database Migration Service suplanta estas credenciales de usuario para cargar los archivos de copia de seguridad en el contenedor de Azure Storage.<br/><br/> El proceso de instalación de SQL Server Express establece el protocolo TCP/IP en **deshabilitado** de forma predeterminada. Asegúrese de que esté habilitado.


## <a name="scenario-steps"></a>Pasos del escenario

A continuación se indica cómo configurará la implementación Contoso:

> [!div class="checklist"]
> * **Paso 1: Configurar una Instancia administrada de SQL Azure**: se necesita una instancia administrada creada previamente a la que se migrará la base de datos de SQL Server local.
> * **Paso 2: Preparar DMS**: se debe registrar el proveedor de Database Migration, crear una instancia y, a continuación, crear un proyecto DMS. También se debe configurar el URI de SA para DMS. Un identificador uniforme de recursos (URI) de firma de acceso compartido (SAS) proporciona acceso delegado a los recursos en la cuenta de almacenamiento, por lo que puede conceder permisos limitados a los objetos del almacenamiento. Se configura un URI de SAS para que DMS pueda acceder al contenedor de la cuenta de almacenamiento en que el servicio carga los archivos de copia de seguridad de SQL Server.
> * **Paso 3: Preparar Azure para Site Recovery**. Para Site Recovery, se crea una cuenta de almacenamiento de Azure que contenga los datos replicados, así como un almacén de Recovery Services.
> * **Paso 4: Preparar VMware local para Site Recovery**. Contoso prepara las cuentas para la detección de VM y la instalación del agente, y se prepara para conectar a las VM de Azure tras una conmutación por error.
> * **Paso 5: Replicar VM**. Para configurar la replicación, se configura el entorno de origen y de destino de Site Recovery, se configura una directiva de replicación y se empiezan a replicar las VM en Azure Storage.
> * **Paso 6: migración de la base de datos con DMS**. Ya se puede migrar la base de datos.
> * **Paso 7: Migrar las máquinas virtuales mediante Site Recovery**. Se ejecuta una conmutación por error de prueba para comprobar que todo funciona y, luego, se ejecuta una conmutación por error completa para migrar las VM a Azure.


## <a name="step-1-prepare-an-azure-sql-managed-instance"></a>Paso 1: Preparar una Instancia administrada de Azure SQL

Para configurar una Instancia administrada de Azure SQL, Contoso requiere una subred:

- La subred debe estar dedicada. Debe estar vacía y no puede contener ningún otro servicio en la nube. Tampoco puede ser una subred de puerta de enlace.
- Una vez creada la instancia administrada, no puede agregarle recursos.
- La subred no puede tener un NSG asociado a ella.
- La subred tiene que tener una tabla de rutas de usuario (UDR) con Internet de próximo salto 0.0.0.0/0 como única ruta asignada. 
- DNS personalizado opcional: si se especifica un DNS personalizado en la red virtual, es necesario agregar la dirección IP de las resoluciones recursivas de Azure (por ejemplo, 168.63.129.16) a la lista. [Más información](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).
- La subred no puede tener un punto de conexión de servicio (Storage o SQL) asociado a ella. Los puntos de conexión de servicio se deben deshabilitar en la red virtual.
- La subred tiene que tener como mínimo 16 direcciones IP. [Más información](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#determine-the-size-of-subnet-for-managed-instances) acerca de cómo ajustar el tamaño de la subred de la instancia administrada.
- En su entorno híbrido, se necesita configuración de DNS personalizada. Contoso configurará las opciones de DNS para usar uno o varios de sus servidores de Azure DNS. [Más información](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns) acerca de la personalización de DNS.

### <a name="set-up-a-virtual-network-for-the-managed-instance"></a>Configurar una red virtual para la instancia administrada

Contoso configura la red virtual como se indica a continuación: 

1. Contoso crea una nueva red virtual (VNET-SQLMI-EU2) en la región Este de EE. UU. 2 principal, en el grupo de recursos ContosoNetworkingRG.
2. Contoso asigna un espacio de direcciones 10.235.0.0/24 y se asegura de que el intervalo no se superponga con ninguna otra red de la empresa Contoso.
2. Se agregan dos subredes a la red:
    - SQLMI-DS-EUS2 (10.235.0.0.25)
    - SQLMI-SAW-EUS2 (10.235.0.128/29). Esta subred se utilizará para conectar el directorio a la instancia administrada (SQLMI).

    ![Red de la instancia administrada](media/contoso-migration-rehost-vm-sql-managed-instance/mi-vnet.png)

6. Después de implementar la red virtual y las subredes, Contoso empareja las redes como se indica a continuación:

    - Empareja VNET-SQLMI-EUS2 con VNET-HUB-EUS2 (red virtual central para Este de EE. UU. 2).
    - Empareja VNET-SQLMI-EUS2 con VNET-PROD-EUS2 (red de producción).

    ![Emparejamiento de redes](media/contoso-migration-rehost-vm-sql-managed-instance/mi-peering.png)

7. Contoso establece la configuración de DNS personalizada. DNS señalará, en primer lugar, a sus DC de Azure. Azure DNS será secundario. La ubicación de los DC de Azure de Contoso es la siguiente:

    - Ubicado en la subred PROD-DC-EUS2, en la red de producción de Este de EE. UU. 2 (VNET-PROD-EUS2).
    - Dirección de CONTOSODC3: 10.245.42.4
    - Dirección de CONTOSODC4: 10.245.42.5
    - Resolución de Azure DNS: 168.63.129.16

     ![DNS de red](media/contoso-migration-rehost-vm-sql-managed-instance/mi-dns.png)

**¿Necesita más ayuda?**

- [Obtenga una introducción](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) a las Instancias administradas de Azure SQL.
- [Más información acerca de](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#create-a-new-virtual-network-for-managed-instances) la creación de una red virtual para la Instancia administrada de SQL.
- [Más información](https://docs.microsoft.com/azure/virtual-network/virtual-network-manage-peering) sobre la configuración del emparejamiento.
- [Más información](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started-dns) acerca de la actualización de la configuración del DNS de Azure AD.



### <a name="set-up-routing"></a>Configuración del enrutamiento

La Instancia administrada se coloca en una red virtual privada, por lo que Contoso necesita una tabla de redirección para comunicarse con el servicio de administración de Azure. Si no se puede comunicar con el servicio que lo administra, deja de estar accesible.

- La tabla de redirección contiene un conjunto de reglas (rutas) que especifica cómo se deben redirigir los paquetes enviados desde la Instancia administrada en la red virtual.
- La tabla de redirección se asocia con subredes en las que se implementan Instancias administradas. Cada paquete que sale de una subred se controla en función de la tabla de redirección asociada.
- Una subred solo puede estar asociada con una tabla de redirección.
- No hay ningún cargo adicional por la creación de tablas de redirección en Microsoft Azure.

1. Contoso crea una tabla de redirección definida por el usuario. Se crea la tabla de redirección en el grupo de recursos ContosoNetworkingRG.

    ![Tabla de rutas](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table.png)

2. Para cumplir con los requisitos de la Instancia administrada, una vez implementada la tabla de enrutamiento (MIRouteTable), Contoso agrega una ruta con un prefijo de dirección de 0.0.0.0/0 y **Tipo del próximo salto** definido como **Internet**.

    ![Prefijo de tabla de enrutamiento](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-prefix.png)
    
3. Contoso asocia la tabla de enrutamiento a la subred SQLMI-DB-EUS2 (en la red VNET-SQLMI-EUS2). 

    ![Subred de tabla de enrutamiento](media/contoso-migration-rehost-vm-sql-managed-instance/mi-route-table-subnet.png)
    
**¿Necesita más ayuda?**

[Más información](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal#create-new-route-table-and-a-route) acerca de la configuración de rutas para una instancia administrada.

### <a name="create-a-managed-instance"></a>Creación de una instancia administrada

Ahora, Contoso puede aprovisionar una Instancia administrada de SQL Database.

1. Dado que la Instancia administrada sirve a una aplicación empresarial, Contoso la implementa en su región Este de EE. UU. 2 principal, en el grupo de recursos ContosoRG. 
2. Se selecciona un plan de tarifa, proceso de tamaño y almacenamiento para la instancia. [Más información](https://azure.microsoft.com/pricing/details/sql-database/managed/) sobre precios.

    ![Instancia administrada](media/contoso-migration-rehost-vm-sql-managed-instance/mi-create.png)

3. Una vez implementada la Instancia administrada, dos nuevos recursos aparecen en el grupo de recursos ContosoRG:

    - Un clúster virtual en caso de que tenga varias instancias administradas,
    - La Instancia administrada de SQL Server. 

    ![Instancia administrada](media/contoso-migration-rehost-vm-sql-managed-instance/mi-resources.png)

**¿Necesita más ayuda?**

[Más información](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-create-tutorial-portal) acerca del aprovisionamiento de una Instancia administrada.

## <a name="step-2-prepare-dms"></a>Paso 2: Preparar DMS

Para preparar DMS, Contoso debe hacer un par de cosas:

- Registrar el proveedor de DMS en Azure
- Proporcionar a DMS acceso al almacenamiento de Azure para cargar los archivos de copia de seguridad utilizados para migrar una base de datos. Para hacerlo, se crea un contenedor de almacenamiento de blobs y se genera un URI de Firma de acceso compartido (SAS) para él. 
- Cree un proyecto de DMS.


Siga los pasos que se describen a continuación:

1. Contoso registra el proveedor de migración de base de datos en su suscripción.
    ![Registro de DMS](media/contoso-migration-rehost-vm-sql-managed-instance/dms-subscription.png)

2. Se crea un contenedor de blobs de almacenamiento y se genera un URI de SAS para que DMS puede tener acceso a él.

    ![URI de SAS](media/contoso-migration-rehost-vm-sql-managed-instance/dms-sas.png)

3. Por último, crea una instancia de DMS. 

    ![Instancia de DMS](media/contoso-migration-rehost-vm-sql-managed-instance/dms-instance.png)

4. Contoso coloca la instancia de DMS en la subred PROD-DC-EUS2 de la red virtual VNET-PROD-DC-EUS2.
    - Se coloca ahí porque debe estar en una red virtual que pueda tener acceso a la VM local de SQL Server a través de una puerta de enlace de VPN.
    - VNET-PROD-EUS2 está emparejada con VNET-HUB-EUS2 y puede usar puertas de enlace remotas.  Esto garantiza que DMS se pueda comunicar según sea necesario.

        ![Red de DMS](media/contoso-migration-rehost-vm-sql-managed-instance/dms-network.png)

**¿Necesita más ayuda?**
- [Más información](https://docs.microsoft.com/azure/dms/quickstart-create-data-migration-service-portal) sobre la configuración de DMS.
- [Más información](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2) sobre cómo crear y usar SAS.


## <a name="step-3-prepare-azure-for-the-site-recovery-service"></a>Paso 3: Preparar Azure para el servicio de Site Recovery

Hay una serie de elementos de Azure que Contoso necesita para configurar Site Recovery para la migración de la VM de capa web (WEBMV)

- Una red virtual para colocar los recursos tras la conmutación por error.
- Una cuenta de almacenamiento de Azure para almacenar los datos replicados. 
- Un almacén de Recovery Services en Azure.

Se configura Site Recovery como se indica a continuación:

1. Puesto que la VM es un front-end web para la aplicación SmartHotel, se conmuta por error la VM en la red de producción existente (VNET-PROD-EUS2) y la subred (PROD-FE-EUS2) en la región Este de EE. UU. 2 principal. Se configura esta red cuando se [implementa la infraestructura de Azure](contoso-migration-infrastructure.md)
2. Se crea una cuenta de almacenamiento de Azure (contosovmsacc20180528). Se usa una cuenta de uso general, con almacenamiento estándar y replicación LRS.

    ![Almacenamiento de Site Recovery](media/contoso-migration-rehost-vm-sql-managed-instance/asr-storage.png)

3. Con la cuenta de almacenamiento y de red en su lugar, Contoso puede crear un almacén (ContosoMigrationVault) y colocarlo en el grupo de recursos ContosoFailoverRG, en la región Este de EE. UU. 2 principal.

    ![Almacén de Recovery Services](media/contoso-migration-rehost-vm-sql-managed-instance/asr-vault.png)

**¿Necesita más ayuda?**

[Más información](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) sobre cómo configurar Azure para Site Recovery.


## <a name="step-4-prepare-on-premises-vmware-for-site-recovery"></a>Paso 4: Preparar VMware local para Site Recovery

Para preparar VMware para Site Recovery, Contoso debe:

- Preparar una cuenta en el servidor vCenter o en el host ESXi de vSphere para automatizar la detección de VM.
- Preparar una cuenta que permita la instalación automática de Mobility Service en cada VM de VMware que quiera replicar.
- Preparar VM locales para que puedan conectarse a las VM de Azure cuando se creen tras la conmutación por error.


### <a name="prepare-an-account-for-automatic-discovery"></a>Preparación de una cuenta de detección automática

Site Recovery necesita acceso a los servidores de VMware para:

- Detectar automáticamente las máquinas virtuales. Se requiere al menos una cuenta de solo lectura.
- Coordinar la replicación, la conmutación por error y la conmutación por recuperación. Necesita una cuenta que pueda ejecutar operaciones como la creación y eliminación de discos, así como la activación de máquinas virtuales.

Contoso configura la cuenta como se indica a continuación:

1. Crea un rol en el nivel de vCenter.
2. Se asignan a ese rol los permisos necesarios.

**¿Necesita más ayuda?**

[Más información](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) sobre cómo crear y asignar un rol para la detección automática.

### <a name="prepare-an-account-for-mobility-service-installation"></a>Preparación de una cuenta para la instalación de Mobility Service

Mobility Service debe instalarse en la máquina que quiera replicar.

- Site Recovery puede hacer una instalación de inserción automática de este componente cuando se habilita la replicación para la máquina virtual.
- Para la instalación de inserción automática, tiene que preparar una cuenta que Site Recovery vaya a usar para obtener acceso a la máquina virtual.
- Deberá especificar esta cuenta al configurar la replicación en la consola de Azure.
- Necesita un dominio o cuenta local con permisos para instalar en la máquina virtual.

**¿Necesita más ayuda?**

[Más información](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) sobre cómo crear una cuenta para la instalación de inserción de Mobility Service.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Preparación para la conexión a las máquinas virtuales de Azure después de la conmutación por error

Después de la conmutación por error en Azure, Contoso quiere poder conectarse a las VM replicadas de Azure. Para ello, hay un par de cosas que se deben hacer en la VM local antes de ejecutar la migración: 

1. Para obtener acceso a través de Internet, habilite RDP en la VM local antes de la conmutación por error y asegúrese de agregar reglas de TCP y UDP para el perfil **Público**, así como de que se permita RDP en **Firewall de Windows** > **Aplicaciones permitidas** para todos los perfiles.
2. Para obtener acceso a través de su VPN de sitio a sitio, se habilita el protocolo RDP en la máquina local y se permite RDP en **Firewall de Windows** -> **Aplicaciones y características permitidas** para redes **de dominio y privadas**.
3. Se establece la directiva de SAN del sistema operativo en la VM local en **OnlineAll**.

Además, cuando ejecuta una conmutación por error, debe comprobar lo siguiente:

- No debe haber actualizaciones de Windows pendientes en la VM cuando se desencadene una conmutación por error. Si las hay, no se podrá iniciar sesión en la máquina virtual hasta que se complete la actualización.
- Después de la conmutación por error, compruebe los **Diagnósticos de arranque** para ver una captura de pantalla de la VM. Si no funciona, se debe comprobar que se está ejecutando la VM, así como revisar estas [sugerencias de solución de problemas](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


## <a name="step-5-replicate-the-on-premises-vms-to-azure-with-site-recovery"></a>Paso 5: Replicación de las VM locales en Azure con Site Recovery

Antes de ejecutar una migración a Azure, Contoso debe configurar la replicación y habilitarla para su VM local.

### <a name="set-a-replication-goal"></a>Establecer un objetivo de replicación

1. En el almacén, en el nombre de este (ContosoVMVault), se establece un objetivo de replicación (**Introducción** > **Site Recovery** > **Preparar infraestructura**.
2. Especifica que las máquinas se encuentran en el entorno local, que son VM de VMware y que quieren replicarse en Azure.

    ![Objetivo de replicación](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Confirmación del planeamiento de la implementación

Para continuar, debe confirmar que ha completado la planificación de implementación seleccionando la opción **Sí, ya lo hice**. En esta implementación, Contoso solo va a migrar una única VM, por lo que no es necesario planear la implementación.

### <a name="set-up-the-source-environment"></a>Configuración del entorno de origen

Ahora se debe configurar el entorno de origen. Para ello, descarga una plantilla OVF y la usa para implementar el servidor de configuración y sus componentes asociados como una VM local de VMware altamente disponible. Entre los componentes del servidor se encuentran los siguientes:

- El servidor de configuración que coordina las comunicaciones entre el entorno local y Azure, además de administrar la replicación de datos.
- El servidor de procesos que actúa como puerta de enlace de replicación. Recibe datos de replicación, los optimiza con almacenamiento en caché, compresión y cifrado, y los envía a Azure Storage.
- El servidor de procesos también instala Mobility Service en las máquinas virtuales que quiere replicar, además de realizar la detección automática de las máquinas virtuales de VMware locales.
- Después de crear e iniciar la VM del servidor de configuración, Contoso puede registrarlo en el almacén.


Contoso realiza estos pasos como se indica a continuación:

1. Se descarga la plantilla OVF de Azure Portal (**Preparar infraestructura** > **Origen** > **Servidor de configuración**).
    
    ![Descarga de OVF](./media/contoso-migration-rehost-vm-sql-managed-instance/add-cs.png)

2. Importa la plantilla en VMware para crear e implementar la VM.

    ![Plantilla de OVF](./media/contoso-migration-rehost-vm-sql-managed-instance/vcenter-wizard.png)

3.  Al activar la VM por primera vez, se inicia en una experiencia de instalación de Windows Server 2016. Acepta el contrato de licencia e introduce una contraseña de administrador.
4. Una vez finalizada la instalación, inicia sesión en la VM como administrador. La primera vez que inicia sesión, se ejecuta la herramienta de configuración de Azure Site Recovery de manera predeterminada.
5. En la herramienta, Contoso especifica un nombre que se usará al registrar el servidor de configuración en el almacén.
6. La herramienta comprueba que la máquina virtual pueda conectarse a Azure. Una vez establecida la conexión, hace clic en **Iniciar sesión** para iniciar sesión en la suscripción a Azure. Las credenciales deben tener acceso al almacén donde se registrará el servidor de configuración. 

    [Registrar servidor de configuración](./media/contoso-migration-rehost-vm-sql-managed-instance/config-server-register2.png)

7. La herramienta realiza algunas tareas de configuración y, a continuación, se reinicia. Vuelve a iniciar sesión en la máquina y el Asistente para administración del servidor de configuración se inicia automáticamente.
8. En el asistente, selecciona la NIC para recibir tráfico de replicación. Una vez configurada, esta opción no se puede cambiar.
9. Selecciona la suscripción, el grupo de recursos y el almacén en el que se va a registrar el servidor de configuración.
        ![almacén](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz1.png) 

10. A continuación, descarga e instala MySQL Server y VMWare PowerCLI. A continuación, se valida la configuración del servidor.
11. Después de la validación, especifica la dirección IP o el FQDN del host de vSphere o vCenter Server. Deja el puerto predeterminado y especifica un nombre descriptivo para vCenter Server en Azure.
12. Necesita especificar la cuenta que creó anteriormente para que Site Recovery pueda detectar automáticamente VM de VMware disponibles para la replicación. 
13. Especifica las credenciales para instalar automáticamente Mobility Service cuando está habilitada la replicación. Para las máquinas de Windows, la cuenta necesita privilegios de administrador local en las VM. 

    ![vCenter](./media/contoso-migration-rehost-vm-sql-managed-instance/cswiz2.png)

7. Cuando finaliza el registro, en Azure Portal, Contoso verifica si el servidor de configuración y VMware Server aparecen en la página **Origen** del almacén. La detección puede tardar 15 minutos o más. 
8. Luego, Site Recovery se conecta a los servidores de VMware con la configuración especificada y detecta VM.

### <a name="set-up-the-target"></a>Configuración del destino

Ahora, Contoso debe configurar el entorno de replicación de destino.

1. En **Preparar infraestructura** > **Destino**, selecciona la configuración de destino.
2. Site Recovery comprueba si existe una red y una cuenta de almacenamiento de Azure en el destino especificado.

### <a name="create-a-replication-policy"></a>Creación de una directiva de replicación

Después de configurar el origen y el destino, Contoso está listo para crear una directiva de replicación y asociarla con el servidor de configuración.

1. En **Preparar infraestructura** > **Configuración de la replicación** > **Directiva de replicación** >  **Crear y asociar**, crea una directiva **ContosoMigrationPolicy**.
2. Usa la configuración predeterminada:
    - **Umbral de RPO**: valor predeterminado de 60 minutos. Este valor define la frecuencia de creación de puntos de recuperación. Se genera una alerta cuando la replicación continua supera este límite.
    - **Retención de punto de recuperación**. Valor predeterminado de 24 horas. Este valor especifica cuánto tiempo dura el período de retención para cada punto de recuperación. Las máquinas virtuales replicadas se pueden recuperar a cualquier momento de un período.
    - **Frecuencia de las instantáneas coherentes con la aplicación**. Valor predeterminado de una hora. Este valor especifica la frecuencia con la que se crean instantáneas coherentes con la aplicación.
 
        ![Creación de la directiva de replicación](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy.png)

5. La directiva se asocia automáticamente al servidor de configuración. 

    ![Asociación de la directiva de replicación](./media/contoso-migration-rehost-vm-sql-managed-instance/replication-policy2.png)


**¿Necesita más ayuda?**

- Puede leer un tutorial completo de todos estos pasos en [Configurar la recuperación ante desastres para VM de VMware locales](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Puede encontrar instrucciones detalladas que le ayudarán a [configurar el entorno de origen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [implementar el servidor de configuración](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server) y [establecer la configuración de replicación](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).

### <a name="enable-replication"></a>Habilitar replicación

Contoso ya puede empezar a replicar WebVM.

1. En **Replicar aplicación** > **Origen** > **+Replicar**, selecciona la configuración de origen.
2. Indica que quiere habilitar las máquinas virtuales y selecciona vCenter Server y el servidor de configuración.

 ![Habilitar replicación](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication1.png)
 
3. Ahora, especifica la configuración de destino, incluidos el grupo de recursos y una red virtual en la que la VM de Azure se ubicará después de la conmutación por error, así como una cuenta de almacenamiento en la que se almacenarán los datos replicados.

     ![Habilitar replicación](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication2.png)

4. Contoso selecciona WebVM para la replicación. Site Recovery instala Mobility Service en cada máquina virtual cuando se habilita la replicación para esta. 

    ![Habilitar replicación](./media/contoso-migration-rehost-vm-sql-managed-instance/enable-replication3.png)

5. Contoso comprueba que la directiva de replicación correcta está seleccionada y habilita la replicación para WEBVM. Realiza un seguimiento del progreso de la replicación en **Trabajos**. La máquina estará preparada para la conmutación por error después de que finalice el trabajo **Finalizar la protección**.
6. En **Essentials** en Azure Portal, Contoso puede ver la estructura para las VM que se replican en Azure.

    ![Vista de la infraestructura](./media/contoso-migration-rehost-vm-sql-managed-instance/essentials.png)


**¿Necesita más ayuda?**

Puede leer un tutorial completo de todos estos pasos en [Habilitar replicación](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).

## <a name="step-6-migrate-the-database-with-dms"></a>Paso 6: Migrar la base de datos con DMS

Contoso debe crear un proyecto de DMS y migrar la base de datos.

### <a name="create-a-dms-project"></a>Crear un proyecto de DMS
1. Crea un proyecto de DMS. Especifica el tipo de servidor de origen como SQL Server y el destino como Instancia administrada de Azure SQL Database.

     ![Proyecto DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-project.png)

2. Después de crear el proyecto, se abre Migration Wizard.

### <a name="migrate-the-database"></a>Migración de la base de datos 

1. En Migration Wizard, Contoso especifica la VM de origen en que se encuentra la base de datos local y las credenciales para acceder a ella.

    ![Origen de DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-source.png)

2. Selecciona la base de datos que se migrará (SmartHotel.Registration).

    ![Base de datos de origen de DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-wizard-sourcedb.png)

3. Como destino, especifica el nombre de la Instancia administrada de Azure y las credenciales de acceso.

    ![Destino de DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-target-details.png)

4. A continuación, en **+Nueva actividad** > **Ejecutar migración**, especifica la configuración para ejecutar la migración:
    - Credenciales de origen y destino.
    - Base de datos que se migrará.
    - Recurso compartido de red creado en la VM local. DMS lleva las copias de seguridad de origen a este recurso compartido.
        - La cuenta de servicio que ejecuta la instancia de SQL Server de origen tiene que tener privilegios de escritura en este recurso compartido.
        - Especifique la ruta de acceso FQDN para el recurso compartido.
    - El URI de SAS que proporciona a DMS acceso al contenedor de la cuenta de almacenamiento en que el servicio carga los archivos de copia de seguridad para la migración.

        ![Configuración de DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-migration-settings.png)

5. Guarda la migración y la ejecuta.
6. En **Introducción**, supervisa el estado de la migración.

    ![Monitor de DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor1.png)

7. Una vez completada la migración, comprueba que las bases de datos de destino existen en la instancia administrada.

    ![Monitor de DMS](./media/contoso-migration-rehost-vm-sql-managed-instance/dms-monitor2.png)

## <a name="step-7-migrate-the-vm-with-site-recovery"></a>Paso 7: Migrar la VM con Site Recovery

Contoso ejecuta una conmutación por error rápida de prueba y, a continuación, migra la VM.

### <a name="run-a-test-failover"></a>Ejecución de una conmutación por error de prueba

Antes de migrar WEBVM, una conmutación por error de prueba ayuda a asegurarse de que todo funciona según lo esperado. 

1. Ejecuta una conmutación por error de prueba en el punto más reciente en el tiempo disponible (**Procesado más recientemente**).
2. Selecciona **Apague la máquina antes de comenzar con la conmutación por error**, por lo que Site Recovery intenta apagar la VM de origen antes de desencadenar la conmutación por error. La conmutación por error continúa aunque se produzca un error de cierre. 
3. Se ejecuta la conmutación por error de prueba: 

    - Se ejecuta una comprobación de requisitos previos para garantizar que todas las condiciones necesarias para la conmutación por error están correctamente establecidas.
    - La conmutación por error procesa los datos, de modo que se pueda crear una máquina virtual de Azure. Si se selecciona el último punto de recuperación, se crea un punto de recuperación a partir de los datos.
    - Se crea una VM de Azure mediante los datos procesados en el paso anterior.
3. Una vez finalizada la conmutación por error, la VM de Azure de réplica aparece en Azure Portal. Comprueba que todo funciona correctamente. La VM tiene el tamaño adecuado, está conectada a la red correspondiente y se está ejecutando. 
4. Después de comprobar la conmutación por error de prueba, Contoso limpia la conmutación por error, y registra y guarda las observaciones. 

### <a name="migrate-the-vm"></a>Migración de la VM

1. Después de comprobar que la conmutación por error de prueba funcionó según lo previsto, Contoso crea un plan de recuperación para la migración. Agregan WEBVM al plan.

     ![Plan de recuperación](./media/contoso-migration-rehost-vm-sql-managed-instance/recovery-plan.png)

2. A continuación, ejecuta una conmutación por error en el plan. Selecciona el punto de recuperación más reciente y especifica que Site Recovery debe intentar apagar la VM local antes de desencadenar la conmutación por error.

    ![Conmutación por error](./media/contoso-migration-rehost-vm-sql-managed-instance/failover1.png)

3. Después de la conmutación por error, Contoso verifica si la VM de Azure aparece según lo esperado en Azure Portal.

   ![Plan de recuperación](./media/contoso-migration-rehost-vm-sql-managed-instance/failover2.png)

4. Después de verificar la VM en Azure, completa la migración para finalizar el proceso de migración, detiene la replicación para la VM y detiene la facturación de Site Recovery de la VM.

    ![Conmutación por error](./media/contoso-migration-rehost-vm-sql-managed-instance/failover3.png)

### <a name="update-the-connection-string"></a>Actualización de la cadena de conexión

Como último paso del proceso de migración, Contoso actualiza la cadena de conexión de la aplicación para que apunte a la base de datos migrada que se ejecuta en su MI de SQL.

1. En Azure Portal, busca la cadena de conexión haciendo clic en **Configuración** > **Cadenas de conexión**.

    ![Conmutación por error](./media/contoso-migration-rehost-vm-sql-managed-instance/failover4.png)  

2. Actualiza la cadena con el nombre de usuario y la contraseña de la Instancia administrada de SQL.
3. Una vez configurada la cadena, reemplaza la cadena de conexión actual en el archivo web.config de su aplicación.
4. Después de actualizar el archivo y guardarlo, reinicia IIS en WEBVM. Para ello, usa **IISRESET /RESTART** desde un símbolo del sistema.
5. Después de reiniciar IIS, la aplicación usará ahora la base de datos que se ejecuta en la Instancia administrada de SQL.
6. En este momento, Contoso puede cerrar la máquina SQLVM local y se completa la migración.

**¿Necesita más ayuda?**

- [Más información](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) sobre la ejecución de una conmutación por error de prueba. 
- [Más información](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) sobre cómo crear un plan de recuperación.
- [Más información](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) sobre cómo crear una conmutación por error en Azure.

## <a name="clean-up-after-migration"></a>Limpiar después de la migración

Una vez completada la migración, la aplicación SmartHotel se ejecuta en una VM de Azure y la base de datos de SmartHotel está disponible en la Instancia administrada de Azure SQL.  

Ahora, Contoso debe hacer una limpieza:  

- Quitar la máquina WEBVM del inventario de vCenter.
- Quitar la máquina SQLVM del inventario de vCenter.
- Quitar WEBVM y SQLVM de los trabajos de copia de seguridad locales.
- Actualizar la documentación interna para que muestre la nueva ubicación y dirección IP para WEBVM.
- Quitar SQLVM de la documentación. Como alternativa, se puede marcar para que se muestre como eliminado y ya no aparezca en el inventario de la VM.
- Revisar los recursos que interactúan con las VM retiradas y actualizar los valores de configuración pertinentes o la documentación para reflejar la nueva configuración.

## <a name="review-the-deployment"></a>Revisar la implementación

Con los recursos migrados de Azure, Contoso debe proteger la infraestructura nueva y ponerla totalmente en marcha.

### <a name="security"></a>Seguridad

El equipo de seguridad de Contoso revisa las VM de Azure y la Instancia administrada de SQL para determinar cualquier problema de seguridad con su implementación.

- Se revisan los grupos de seguridad de red (NSG) de la VM con el fin de controlar el acceso. Los grupos de seguridad de red (NSG) se usan para asegurarse de que solo el tráfico permitido pueda pasar a la aplicación.
- También se considera la posibilidad de proteger los datos en los discos mediante Azure Disk Encryption y Azure Key Vault.
- Se habilita la detección de amenazas en la Instancia administrada de SQL (SQLMI). Se envían alertas a su equipo de seguridad o sistema de servicio para abrir una incidencia si se detecta una amenaza. [Más información](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-threat-detection).

     ![Seguridad de la Instancia administrada](./media/contoso-migration-rehost-vm-sql-managed-instance/mi-security.png)  

Obtenga [más información](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) sobre los procedimientos de seguridad recomendados de las VM.

### <a name="backups"></a>Copias de seguridad
Contoso realizará una copia de seguridad de los datos de WEBVM mediante el servicio Azure Backup. [Más información](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Optimización de los costos y licencias

1. Actualmente, Contoso tiene licencia para WEBVM y aprovechará la Ventaja híbrida de Azure.  Convertirá la VM de Azure existente para aprovechar las ventajas que ofrecen estos precios.
2. Contoso habilitará Azure Cost Management bajo licencia de Cloudyn, una subsidiaria de Microsoft. Se trata de una solución de administración de costos en varias nubes que le permitirá utilizar y administrar Azure, y otros recursos en la nube.  [Más información](https://docs.microsoft.com/azure/cost-management/overview) sobre Azure Cost Management. 


## <a name="conclusion"></a>Conclusión

En este artículo, Contoso rehospedó la aplicación SmartHotel en Azure mediante la migración de la VM de front-end de la aplicación a Azure con el servicio Site Recovery. Realizó la migración de la base de datos local a una Instancia administrada de Azure SQL mediante DMS.

## <a name="next-steps"></a>Pasos siguientes

En el siguiente artículo de la serie, le mostraremos cómo Contoso rehospeda la aplicación SmartHotel en VM de Azure solo mediante el servicio Azure Site Recovery.

