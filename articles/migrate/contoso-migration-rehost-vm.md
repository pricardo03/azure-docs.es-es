---
title: Rehospedar una aplicación de Contoso con migración a VM de Azure mediante Azure Site Recovery | Microsoft Docs
description: Aprenda cómo rehospedar una aplicación local con la migración mediante lift-and-shift en máquinas locales a Azure mediante el servicio Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/11/2018
ms.author: raynew
ms.openlocfilehash: 004ce399437ef581b319980c83f00cb1427b934b
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/11/2018
ms.locfileid: "49115845"
---
# <a name="contoso-migration-rehost-an-on-premises-app-to-azure-vms"></a>Migración de Contoso: rehospedar una aplicación local a VM de Azure


En este artículo se muestra cómo Contoso rehospeda la aplicación SmartHotel360 local en Azure al migrar las VM de la aplicación a VM de Azure.


Este documento es el primero de una serie de artículos que muestran cómo la compañía ficticia Contoso migra recursos locales a la nube de Microsoft Azure. En la serie se incluye información de fondo y escenarios en los que se muestra la configuración de una infraestructura de migración, la valoración de los recursos locales para la migración y la ejecución de distintos tipos de migraciones. La complejidad de los escenarios va en aumento. Se agregarán más artículos con el tiempo.

**Artículo** | **Detalles** | **Estado**
--- | --- | ---
[Artículo 1: Introducción](contoso-migration-overview.md) | Información general de la serie de artículos, la estrategia de migración de Contoso y las aplicaciones de ejemplo que se usan en esta serie. | Disponible
[Artículo 2: Implementación de la infraestructura de Azure](contoso-migration-infrastructure.md) | Contoso prepara la infraestructura local y la infraestructura de Azure para la migración. Se usa la misma infraestructura en todos los artículos de la serie sobre migración. | Disponible
[Artículo 3: Evaluación de los recursos locales para la migración a Azure](contoso-migration-assessment.md)  | Contoso ejecuta una valoración de su aplicación local SmartHotel360 que se ejecuta en VMware. Contoso evalúa las máquinas virtuales de la aplicación mediante el servicio Azure Migrate, y la base de datos SQL Server de la aplicación con Data Migration Assistant. | Disponible
[Artículo 4: Rehospedaje de una aplicación en una máquina virtual de Azure e Instancia administrada de Azure SQL Database](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso ejecuta una migración mediante lift-and-shift a Azure para su aplicación SmartHotel360 local. Contoso migra la máquina virtual de front-end de la aplicación mediante [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso migra la base de datos de la aplicación a una instancia administrada de Azure SQL Database mediante [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Disponible   
Artículo 5: Rehospedaje de una aplicación en VM de Azure | Contoso migra sus máquinas virtuales de la aplicación SmartHotel360 a máquinas virtuales de Azure mediante el servicio Site Recovery. | Este artículo
[Artículo 6: Rehospedaje de una aplicación en máquinas virtuales de Azure y en un grupo de disponibilidad Always On de SQL Server](contoso-migration-rehost-vm-sql-ag.md) | Contoso migra la aplicación SmartHotel360. Contoso usa Site Recovery para migrar las máquinas virtuales de la aplicación. Usa Database Migration Service para migrar la base de datos de la aplicación a un clúster de SQL Server que está protegido por un grupo de disponibilidad Always On. | Disponible 
[Artículo 7: Rehospedaje de una aplicación Linux en VM de Azure](contoso-migration-rehost-linux-vm.md) | Contoso realiza una migración mediante lift-and-shift de la aplicación osTicket de Linux a máquinas virtuales de Azure, mediante Azure Site Recovery | Disponible
[Artículo 8: Rehospedaje de una aplicación Linux en VM de Azure y en Azure MySQL](contoso-migration-rehost-linux-vm-mysql.md) | Contoso migra la aplicación osTicket de Linux a máquinas virtuales de Azure mediante Azure Site Recovery y la base de datos de la aplicación a una instancia de Azure MySQL Server con MySQL Workbench. | Disponible
[Artículo 9: Refactorizar una aplicación en Azure Web Apps y Azure SQL Database](contoso-migration-refactor-web-app-sql.md) | Contoso migra la aplicación SmartHotel360 a una aplicación web de Azure y la base de datos de la aplicación a una instancia de Azure SQL Server con Database Migration Assistant | Disponible
[Artículo 10: Refactorizar una aplicación Linux en Azure Web Apps y Azure MySQL](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso migra su aplicación osTicket de Linux a una aplicación web de Azure en varias regiones de Azure con Azure Traffic Manager, integrado con GitHub para la entrega continua. Contoso migra la base de datos de la aplicación a una instancia de Azure Database for MySQL. | Disponible 
[Artículo 11: Refactorización de TFS en Azure DevOps Services](contoso-migration-tfs-vsts.md) | Contoso migra su implementación local de Team Foundation Server a Azure DevOps Services en Azure. | Disponible
[Artículo 12: Rediseño de la arquitectura de una aplicación en Azure Containers y Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | Contoso migra su aplicación SmartHotel a Azure. A continuación, rediseña el nivel de aplicación web como un contenedor de Windows que se ejecuta en Azure Service Fabric, y la base de datos con Azure SQL Database. | Disponible
[Artículo 13: Volver a compilar una aplicación en Azure](contoso-migration-rebuild.md) | Contoso recompila su aplicación SmartHotel mediante una serie de funcionalidades y servicios de Azure, como Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services y Azure Cosmos DB. | Disponible
[Artículo 14: Escalado de una migración en Azure](contoso-migration-scale.md) | Después de probar combinaciones de migración, Contoso se prepara para escalar una migración completa a Azure. | Disponible


En este artículo, Contoso migra a Azure la aplicación SmartHotel 360 .NET de Windows de dos niveles que se ejecuta en las VM de VMware. Si quiere usar esta aplicación, se proporciona como código abierto y se puede descargar desde [github](https://github.com/Microsoft/SmartHotel360).



## <a name="business-drivers"></a>Impulsores de negocio

El equipo directivo de TI ha trabajado estrechamente con sus socios comerciales para comprender lo quieren lograr con esta migración:

- **Responder al crecimiento del negocio**: Contoso está creciendo y, como resultado, sus sistemas locales e infraestructura están bajo presión.
- **Limitar el riesgo**: la aplicación SmartHotel360 es fundamental para el negocio de Contoso. Por ello quiere mover la aplicación a Azure sin ningún riesgo.
- **Ampliar**: Contoso no quiere modificar la aplicación, pero desea asegurarse de que sea estable.


## <a name="migration-goals"></a>Objetivos de la migración

El equipo de la nube de Contoso ha marcado los objetivos de esta migración. Estos objetivos se usan para determinar el mejor método de migración:

- Después de la migración, la aplicación de Azure debería tener las mismas funcionalidades de rendimiento que tiene actualmente en VMware.  La aplicación seguirá siendo tan imprescindible en la nube como lo es en el entorno local. 
- Contoso no quiere hacer inversiones en esta aplicación.  Es importante para la empresa, pero, en su formato actual, Contoso solo quiere moverla a la nube de modo seguro.
- Contoso no quiere cambiar el modelo de operaciones de esta aplicación. Lo que quiere es interactuar con ella en la nube de la misma manera que lo hace ahora.
- Contoso no quiere cambiar la funcionalidad de la aplicación. Solo cambiará su ubicación.


## <a name="solution-design"></a>Diseño de la solución

Después de fijar sus objetivos y requisitos, Contoso diseña y revisa una solución de implementación e identifica el proceso de migración, incluidos los servicios de Azure que Contoso usará para ello.

### <a name="current-app"></a>Aplicación actual

- La aplicación se divide en niveles entre dos VM (**WEBVM** y **SQLVM**).
- Las VM se encuentran en el host de VMware ESXi **contosohost1.contoso.com** (versión 6.5).
- El entorno de VMware lo administra vCenter Server 6.5 (**vcenter.contoso.com**), que se ejecuta en una VM.
- Contoso tiene un centro de datos local (contoso-datacenter), con un controlador de dominio local (**contosodc1**).

### <a name="proposed-architecture"></a>Arquitectura propuesta

- Dado que la aplicación es una carga de trabajo de producción, las máquinas virtuales de la aplicación en Azure residirán en el grupo de recursos de producción ContosoRG.
- Las máquinas virtuales de la aplicación se migrarán a la región primaria de Azure (Este de EE. UU. 2) y se colocarán en la red de producción (VNET-PROD-EUS2).
- La máquina virtual front-end web residirá en la subred front-end (PROD-FE-EUS2), en la red de producción.
- La máquina virtual de base de datos residirá en la subred de base de datos (PROD-DB-EUS2), en la red de producción.
- Las VM locales del centro de datos de Contoso se retirarán después de realizar la migración.

![Arquitectura del escenario](./media/contoso-migration-rehost-vm/architecture.png) 

### <a name="database-considerations"></a>Consideraciones sobre la base de datos

Como parte del proceso de diseño de la solución, Contoso hizo una comparación de características entre Azure SQL Database y SQL Server. Las siguientes consideraciones les ayudaron a decidirse por una máquina virtual IaaS de Azure que ejecuta SQL Server: 

- El uso de una máquina virtual de Azure que ejecuta SQL Server parece ser una solución óptima si Contoso necesita personalizar el sistema operativo o el servidor de bases de datos, o si quisiera ubicar conjuntamente y ejecutar aplicaciones de terceros en la misma máquina virtual.
- Con Software Assurance, Contoso puede en el futuro intercambiar sus licencias existentes para obtener descuentos en una Instancia administrada de SQL Database utilizando la Ventaja híbrida de Azure para SQL Server. Esto puede suponer hasta un 30 % de ahorro en Instancia administrada.



### <a name="solution-review"></a>Revisión de la solución

Contoso evalúa el diseño propuesto y crea una lista de ventajas y desventajas.

**Consideración** | **Detalles**
--- | ---
**Ventajas** | Las dos máquinas virtuales de la aplicación se moverán a Azure sin cambios, de forma que se simplifica la migración.<br/><br/> Dado que Contoso usa lift-and-shift con ambas máquinas virtuales de la aplicación, no se necesitan configuraciones ni herramientas de migración especiales para la base de datos de la aplicación.<br/><br/> Contoso puede aprovechar su inversión en Software Assurance, con la Ventaja híbrida de Azure.<br/><br/> Contoso conservará el control total de las máquinas virtuales de la aplicación en Azure. 
**Desventajas** | WEBVM y SQLVM ejecutan Windows Server 2008 R2. El sistema operativo se admite en Azure para roles específicos (julio de 2018). [Más información](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).<br/><br/> Las capas de datos y web de la aplicación seguirán siendo un único punto de conmutación por error.</br><br/> SQLVM se está ejecutando en SQL Server 2008 R2 que no se encuentra en el soporte técnico. Sin embargo se admite para máquinas virtuales de Azure (julio de 2018). [Más información](https://support.microsoft.com/en-us/help/956893).<br/><br/> Contoso tendrá que seguir admitiendo la aplicación en las máquinas virtuales de Azure en lugar de moverla a un servicio administrado como Azure App Service y Azure SQL Database.



### <a name="migration-process"></a>Proceso de migración

Contoso migrará las máquinas virtuales de la base de datos y el front-end de aplicación a máquinas virtuales de Azure con Site Recovery:

- Primero, Contoso prepara y configura los componentes de Azure para Site Recovery y prepara la infraestructura local de VMware.
- Ya ha establecido la [infraestructura de Azure](contoso-migration-infrastructure.md), por lo que Contoso solo tiene que agregar un par de componentes de Azure específicamente para Site Recovery.
- Con todo preparado, Contoso puede comenzar a replicar las máquinas virtuales.
- Una vez que se haya habilitado la replicación y esta se encuentre en funcionamiento, Contoso migrará la máquina virtual, haciendo que conmute por error en Azure.

![Proceso de migración](./media/contoso-migration-rehost-vm/migraton-process.png) 



### <a name="azure-services"></a>Servicios de Azure

**Servicio** | **Descripción** | **Costee**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | El servicio orquesta y administra la migración y la recuperación ante desastres de las máquinas virtuales de Azure, así como las máquinas virtuales y servidores físicos locales.  | Durante la replicación en Azure, se incurre en gastos de Azure Storage.  Las máquinas virtuales de Azure se crean, e incurren en gastos, cuando se produce una conmutación por error. [Más información](https://azure.microsoft.com/pricing/details/site-recovery/) sobre cargos y precios.


## <a name="prerequisites"></a>Requisitos previos

Esto es lo que tiene hacer Contoso para ejecutar este escenario.

**Requisitos** | **Detalles**
--- | ---
**Suscripción de Azure** | En un artículo anterior de esta serie, Contoso creó suscripciones. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Si crea una cuenta gratuita, será el administrador de su suscripción y podrá realizar todas las acciones.<br/><br/> Si usa una suscripción existente y no es el administrador, tendrá que solicitar al administrador que le asigne permisos de propietario o colaborador.<br/><br/> Si necesita permisos más específicos, consulte [este artículo](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Infraestructura de Azure** | [Vea](contoso-migration-infrastructure.md) cómo Contoso configuró una infraestructura de Azure.<br/><br/> Obtenga más información sobre los requisitos específicos de la [red](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) y el [almacenamiento](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) para Site Recovery.
**Servidores locales** | Los servidores vCenter locales deberían ejecutar las versiones 5.5, 6.0 o 6.5.<br/><br/> Los hosts ESXi deben ejecutar la versión 5.5, 6.0 o 6.5.<br/><br/> Una o más VM VMware se deben ejecutar en el host ESXi.
**Máquinas virtuales locales** | Las máquinas virtuales tienen que cumplir los [requisitos de Azure](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Pasos del escenario

Los administradores de Contoso ejecutarán la migración de la forma siguiente:

> [!div class="checklist"]
> * **Paso 1: Preparar Azure para Site Recovery**. Se crea una cuenta de Azure Storage para que contenga los datos replicados, así como un almacén de Recovery Services.
> * **Paso 2: Preparar VMware local para Site Recovery**. Se preparan las cuentas para la detección de máquinas virtuales y la instalación del agente, y se prepara para conectar a las máquinas virtuales de Azure tras la conmutación por error.
> * **Paso 3: Replicar las máquinas virtuales**. Se configura la replicación y se comienzan a replicar las máquinas virtuales en Azure Storage.
> * **Paso 4: Migrar las máquinas virtuales mediante Site Recovery**. Se ejecuta una conmutación por error de prueba para asegurar que todo funciona y, luego, se ejecuta una conmutación por error completa para migrar las VM a Azure.




## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>Paso 1: Preparar Azure para el servicio de Site Recovery

Estos son los componentes de Azure que Contoso necesita para migrar las VM a Azure:

- Una red virtual en la que se encontrarán las VM de Azure cuando se creen durante la conmutación por error.
- Una cuenta de Azure Storage para almacenar los datos replicados. 
- Un almacén de Recovery Services en Azure.

Se deben configurar como se muestra a continuación:

1. Configuración de una red: Contoso ya configuró una red que se puede usar para Site Recovery cuando [implementó la infraestructura de Azure](contoso-migration-infrastructure.md)

    - La aplicación SmartHotel360 es una aplicación de producción, y las VM se migrarán a la red de producción de Azure (VNET-PROD-EUS2) en la región principal Este de EE. UU. 2.
    - Ambas VM se colocarán en el grupo de recursos ContosoRG, que se usa para los recursos de producción.
    - La VM de la aplicación (WEBVM) se migrará a la subred front-end (PROD-FE-EUS2), en la red de producción.
    - La base de datos de la aplicación (SQLVM) se migrará a la subred de base de datos (PROD-DB-EUS2), en la red de producción.

2. Configuración de una cuenta de almacenamiento: Contoso crea una cuenta de Azure Storage (contosovmsacc20180528) en la región primaria.
    - La cuenta de almacenamiento debe estar en la misma región que el almacén de Recovery Services.
    - Usa una cuenta de uso general con almacenamiento estándar y la replicación de LRS. 

    ![Almacenamiento de Site Recovery](./media/contoso-migration-rehost-vm/asr-storage.png)

3. Creación de un almacén: con la cuenta de almacenamiento y de red implementadas, Contoso ahora crea un almacén de Recovery Services (ContosoMigrationVault) y lo coloca en el grupo de recursos ContosoFailoverRG de la región primaria Este de EE. UU. 2.

    ![Almacén de Recovery Services](./media/contoso-migration-rehost-vm/asr-vault.png)

**¿Necesita más ayuda?**

Obtenga [más información](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) sobre cómo configurar Azure para Site Recovery.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>Paso 2: Preparar VMware local para Site Recovery

Esto es lo que Contoso prepara de manera local:

- Una cuenta en el servidor vCenter o en el host ESXi de vSphere para automatizar la detección de VM.
- Una cuenta que permite la instalación automática de Mobility Service en las VM de VMware. 
- Configuración de VM locales para que Contoso pueda conectarse a las VM de Azure replicadas después de la conmutación por error.


### <a name="prepare-an-account-for-automatic-discovery"></a>Preparación de una cuenta de detección automática

Site Recovery necesita acceso a los servidores de VMware para:

- Detectar automáticamente las máquinas virtuales. 
- Coordinar la replicación, la conmutación por error y la conmutación por recuperación de las VM.
- Se requiere al menos una cuenta de solo lectura. La cuenta debe poder de ejecutar operaciones como, por ejemplo, la creación y eliminación de discos, y la activación de VM.

Los administradores de Contoso configuran la cuenta de la manera siguiente:

1. Se crea un rol en el nivel de vCenter.
2. Se asignan a ese rol los permisos necesarios.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Preparación de una cuenta para la instalación de Mobility Service

Mobility Service debe instalarse en cada VM.

- Site Recovery puede hacer una instalación de inserción automática de Mobility Service cuando está habilitada la replicación de VM.
- Se necesita una cuenta para que Site Recovery pueda acceder a las VM para la instalación de inserción. Esta cuenta se especifica al configurar la replicación.
- La cuenta puede ser una cuenta de dominio o una cuenta local, con permisos para instalar en las VM.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Preparación para la conexión a las máquinas virtuales de Azure después de la conmutación por error

Después de la conmutación por error, Contoso quiere conectarse a las VM de Azure. Para ello, los administradores de Contoso realizan lo siguiente antes de la migración:

1. Para el acceso a través de Internet:

 - Habilita RDP en la máquina virtual local antes de la conmutación por error.
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
- Obtenga [más información](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) sobre cómo crear una cuenta para la instalación de inserción de Mobility Service.


## <a name="step-3-replicate-the-on-premises-vms"></a>Paso 3: Replicar VM locales

Para que los administradores de Contoso puedan ejecutar una migración a Azure, tienen que configurar y habilitar la replicación.

### <a name="set-a-replication-goal"></a>Establecer un objetivo de replicación

1. En el almacén, en el nombre del almacén (ContosoVMVault), selecciona un objetivo de replicación (**Introducción** > **Site Recovery** > **Preparar infraestructura**.
2. Especifica que sus máquinas estén ubicadas en el entorno local, se ejecuten en VMware y se repliquen en Azure.

    ![Objetivo de replicación](./media/contoso-migration-rehost-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Confirmación del planeamiento de la implementación

Para continuar, confirma que se completó la planificación de la implementación al seleccionar **Sí, ya lo hice**. En este escenario, Contoso está migrando solo dos VM y no necesita planificar la implementación.


### <a name="set-up-the-source-environment"></a>Configuración del entorno de origen

Los administradores de Contoso tienen que configurar el entorno de origen. Para ello, descarga una plantilla OVF y la usa para implementar el servidor de configuración de Site Recovery como una VM local de VMware altamente disponible. Una vez que el servidor de configuración se encuentra en funcionamiento, lo registra en el almacén.

El servidor de configuración ejecuta una serie de componentes:

- El componente del servidor de configuración que coordina las comunicaciones entre el entorno local y Azure y administra la replicación de datos.
- El servidor de procesos que actúa como puerta de enlace de replicación. Recibe datos de replicación, los optimiza con almacenamiento en caché, compresión y cifrado, y los envía a Azure Storage.
- El servidor de procesos también instala Mobility Service en las máquinas virtuales que quiere replicar, además de realizar la detección automática de las máquinas virtuales de VMware locales.



Los administradores de Contoso realizan estos pasos como se indica a continuación:

1. En el almacén, descarga la plantilla OVF desde **Preparar infraestructura** > **Origen** > **Servidor de configuración**.
    
    ![Descarga de OVF](./media/contoso-migration-rehost-vm/add-cs.png)

2. Importa la plantilla en VMware para crear e implementar la VM.

    ![Plantilla de OVF](./media/contoso-migration-rehost-vm/vcenter-wizard.png)

3.  Al activar la VM por primera vez, se inicia en una experiencia de instalación de Windows Server 2016. Acepta el contrato de licencia e introduce una contraseña de administrador.
4. Una vez finalizada la instalación, inicia sesión en la VM como administrador. La primera vez que inicia sesión, se ejecuta la herramienta de configuración de Azure Site Recovery de manera predeterminada.
5. En la herramienta, especifica un nombre que se usará para registrar el servidor de configuración en el almacén.
6. La herramienta comprueba que la máquina virtual pueda conectarse a Azure. Una vez establecida la conexión, inicia sesión en la suscripción a Azure. Las credenciales deben tener acceso al almacén donde registrará el servidor de configuración.

    ![Registrar servidor de configuración](./media/contoso-migration-rehost-vm/config-server-register2.png)

7. La herramienta realiza algunas tareas de configuración y, a continuación, se reinicia.
8. Vuelve a iniciar sesión en la máquina y el Asistente para administración del servidor de configuración se inicia automáticamente.
9. En el asistente, selecciona la NIC para recibir tráfico de replicación. Una vez configurada, esta opción no se puede cambiar.
10. Selecciona la suscripción, el grupo de recursos y el almacén en el que se va a registrar el servidor de configuración.
        ![almacén](./media/contoso-migration-rehost-vm/cswiz1.png) 

10. Descarga e instala MySQL Server y VMware PowerCLI. 
11. Después de la validación, especifica la dirección IP o el FQDN del host de vSphere o vCenter Server. Deja el puerto predeterminado y especifica un nombre descriptivo para el servidor en Azure.
12. Especifica la cuenta que se creó para la detección automática, así como las credenciales que se usarán para instalar automáticamente Mobility Service. Para las máquinas de Windows, la cuenta necesita privilegios de administrador local en las VM.

    ![vCenter](./media/contoso-migration-rehost-vm/cswiz2.png)

7. Cuando finaliza el registro, en Azure Portal, comprueban que el servidor de configuración y el servidor VMware aparecen en la página **Origen** del almacén. La detección puede tardar 15 minutos o más. 
8. Luego, Site Recovery se conecta a los servidores de VMware con la configuración especificada y detecta las VM.

### <a name="set-up-the-target"></a>Configuración del destino

Ahora, los administradores de Contoso especifican la configuración de replicación de destino.

1. En **Preparar infraestructura** > **Destino**, selecciona la configuración de destino.
2. Site Recovery comprueba si existe una red y una cuenta de Azure Storage en la ubicación de destino especificada.

### <a name="create-a-replication-policy"></a>Creación de una directiva de replicación

Ahora, los administradores de Contoso pueden crear una directiva de replicación.

1. En **Preparar infraestructura** > **Configuración de la replicación** > **Directiva de replicación** >  **Crear y asociar**, crea una directiva denominada **ContosoMigrationPolicy**.
2. Usa la configuración predeterminada:
    - **Umbral de RPO**: valor predeterminado de 60 minutos. Este valor define la frecuencia de creación de puntos de recuperación. Se genera una alerta cuando la replicación continua supera este límite.
    - **Retención de punto de recuperación**. Valor predeterminado de 24 horas. Este valor especifica cuánto tiempo dura el período de retención para cada punto de recuperación. Las máquinas virtuales replicadas se pueden recuperar a cualquier momento de un período.
    - **Frecuencia de las instantáneas coherentes con la aplicación**. Valor predeterminado de una hora. Este valor especifica la frecuencia con la que se crean instantáneas coherentes con la aplicación.

        ![Creación de la directiva de replicación](./media/contoso-migration-rehost-vm/replication-policy.png)

5. La directiva se asocia automáticamente al servidor de configuración. 

    ![Asociación de la directiva de replicación](./media/contoso-migration-rehost-vm/replication-policy2.png)

### <a name="enable-replication-for-webvm"></a>Habilitar la replicación para WEBVM

Con todo establecido, los administradores de Contoso pueden habilitar la replicación para las máquinas virtuales. Comienza por WebVM.

1. En **Replicar aplicación** > **Origen** > **+Replicar**, selecciona la configuración de origen.
2. Indica que quiere habilitar las VM, seleccionar el servidor de vCenter y selecciona el servidor de configuración.

    ![Habilitar replicación](./media/contoso-migration-rehost-vm/enable-replication1.png)

3. Selecciona la configuración de destino, como el grupo de recursos y la red de Azure, así como la cuenta de almacenamiento.

     ![Habilitar replicación](./media/contoso-migration-rehost-vm/enable-replication2.png)

4. Los administradores de Contoso seleccionan **WebVM** para la replicación, comprueban la directiva de replicación y habilitan la replicación.

    - En esta fase, solo seleccionan WEBVM porque hay que seleccionar la red virtual y la subred, y las máquinas virtuales de la aplicación se colocarán en subredes diferentes.
    - Site Recovery instala Mobility Service automáticamente en la VM cuando se habilita la replicación.

    ![Habilitar replicación](./media/contoso-migration-rehost-vm/enable-replication3.png)

5. Realiza un seguimiento del progreso de la replicación en **Trabajos**. La máquina estará preparada para la conmutación por error después de que finalice el trabajo **Finalizar la protección**.
6. En **Información esencial** en Azure Portal, los administradores de Contoso pueden ver la estructura de las máquinas virtuales que se replican en Azure.


### <a name="enable-replication-for-sqlvm"></a>Habilitar la replicación para SQLVM

Ahora, los administradores de Contoso pueden comenzar a replicar la máquina SQLVM mediante el mismo proceso que se describió anteriormente.

1. Selecciona la configuración de origen.

    ![Habilitar replicación](./media/contoso-migration-rehost-vm/enable-replication1.png)

2. Luego, especifica la configuración de destino.

     ![Habilitar replicación](./media/contoso-migration-rehost-vm/enable-replication2-sqlvm.png)

3. Selecciona SQLVM para la replicación. 

    ![Habilitar replicación](./media/contoso-migration-rehost-vm/enable-replication3-sqlvm.png)

4. Aplica la misma directiva de replicación que usó para WEBVM y habilita la replicación.

    ![Vista de la infraestructura](./media/contoso-migration-rehost-vm/essentials.png)

**¿Necesita más ayuda?**

- Puede leer un tutorial completo de todos estos pasos en [Configurar la recuperación ante desastres para VM de VMware locales](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Puede encontrar instrucciones detalladas que le ayudarán a [configurar el entorno de origen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [implementar el servidor de configuración](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server) y [establecer la configuración de replicación](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- Puede obtener más información sobre cómo [habilitar la replicación](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-4-migrate-the-vms"></a>Paso 4: Migrar las VM 

Los administradores de Contoso ejecutan una conmutación por error de prueba rápida y, luego, una conmutación por error completa para migrar las máquinas virtuales.

### <a name="run-a-test-failover"></a>Ejecución de una conmutación por error de prueba

Una conmutación por error de prueba ayuda a garantizar que todo funciona según lo previsto. 

1. Ejecutan una conmutación por error de prueba en el punto más reciente en el tiempo disponible (**Procesado más recientemente**).
2. Selecciona **Apague la máquina antes de comenzar con la conmutación por error**, por lo que Site Recovery intenta apagar la VM de origen antes de desencadenar la conmutación por error. La conmutación por error continúa aunque se produzca un error de cierre. 
3. Se ejecuta la conmutación por error de prueba: 

    - Se ejecuta una comprobación de requisitos previos para garantizar que todas las condiciones necesarias para la conmutación por error están correctamente establecidas.
    - La conmutación por error procesa los datos, de modo que se pueda crear una máquina virtual de Azure. Si se selecciona el último punto de recuperación, se crea un punto de recuperación a partir de los datos.
    - Se crea una VM de Azure mediante los datos procesados en el paso anterior.
    
3. Una vez finalizada la conmutación por error, la VM de Azure de réplica aparece en Azure Portal. Los administradores de Contoso comprueban que la máquina virtual tiene el tamaño adecuado, está conectada a la red adecuada y está en ejecución. 
4. Después de comprobar la conmutación por error de prueba, limpia la conmutación por error y registra y guarda las observaciones. 

### <a name="create-and-customize-a-recovery-plan"></a>Crear y personalizar un plan de recuperación

 Después de comprobar que la conmutación por error de prueba ha funcionado según lo esperado, los administradores de Contoso crean un plan de recuperación para la migración. 

- Un plan de recuperación especifica el orden en que se produce la conmutación por error e indica cómo las VM de Azure se pondrán en línea en Azure.
- Puesto que la aplicación tiene dos niveles, personaliza el plan de recuperación para que la VM de datos (SQLVM) se inicie antes del front-end (WEBVM).

1. En **Planes de recuperación (Site Recovery)** > **+Plan de recuperación**, crea un plan y se le agregan las VM.

    ![Plan de recuperación](./media/contoso-migration-rehost-vm/recovery-plan.png)

2. Después de crear el plan, lo personaliza (**Planes de recuperación** > **SmartHotelMigrationPlan** > **Personalizar**).
2.  También quita WEBVM de **Grupo 1: iniciar**.  De este modo, se garantiza que la primera acción de inicio afecta solo a SQLVM.
3.  En **+Grupo** > **Agregar elementos protegidos**, agrega WEBVM a Grupo 2: iniciar.  Las VM deben estar en dos grupos distintos.


### <a name="migrate-the-vms"></a>Migrar las VM


Ahora, los administradores de Contoso ejecutan una conmutación por error completa para completar la migración.

1. Selecciona el plan de recuperación > **Conmutación por error**.
2. Selecciona la conmutación por error hasta el punto de recuperación más reciente y especifica que Site Recovery debe intentar apagar la VM local antes de desencadenar la conmutación por error. Puede seguir el progreso de la conmutación por error en la página **Trabajos**.

    ![Conmutación por error](./media/contoso-migration-rehost-vm/failover1.png)


3. Después de la conmutación por error, Contoso comprueba si la VM de Azure aparece según lo esperado en Azure Portal.

    ![Conmutación por error](./media/contoso-migration-rehost-vm/failover2.png)  

3. Después de la comprobación, completa la migración para cada VM. De este modo, se detiene la replicación de la VM y la facturación de Site Recovery correspondiente.

    ![Conmutación por error](./media/contoso-migration-rehost-vm/failover3.png)

**¿Necesita más ayuda?**

- [Más información](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) sobre la ejecución de una conmutación por error de prueba. 
- [Más información](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) sobre cómo crear un plan de recuperación.
- [Más información](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) sobre cómo crear una conmutación por error en Azure.

## <a name="clean-up-after-migration"></a>Limpiar después de la migración

Al finalizar la migración, los niveles de la aplicación SmartHotel360 ahora se ejecutan en las VM de Azure.

Ahora, Contoso debe completar estos pasos de limpieza:  

- Quitar la máquina WEBVM del inventario de vCenter.
- Quitar la máquina SQLVM del inventario de vCenter.
- Quitar WEBVM y SQLVM de los trabajos de copia de seguridad locales.
- Actualizar la documentación interna para mostrar la nueva ubicación y las direcciones IP de las VM.
- Revisar los recursos que interactúan con las VM y actualizar las opciones de configuración pertinentes o la documentación para reflejar la nueva configuración.

## <a name="review-the-deployment"></a>Revisar la implementación

Con la aplicación en ejecución, Contoso ahora debe ponerla totalmente en funcionamiento y protegerla en Azure.

### <a name="security"></a>Seguridad

El equipo de seguridad de Contoso revisa las VM de Azure para determinar si existe algún problema de seguridad.

- Para controlar el acceso, el equipo revisa los grupos de seguridad de red (NSG) de las máquinas virtuales. Los grupos de seguridad de red (NSG) se usan para asegurarse de que solo el tráfico permitido pueda pasar a la aplicación.
- El equipo también tiene en cuenta la protección de los datos en el disco usando Azure Disk Encryption y KeyVault.

Obtenga [más información](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) sobre los procedimientos de seguridad recomendados de las VM.

## <a name="bcdr"></a>BCDR

Para la continuidad empresarial y la recuperación ante desastres (BCDR), Contoso realiza las siguientes acciones:

- Mantener los datos seguros: Contoso realiza la copia de seguridad de los datos de las máquinas virtuales mediante el servicio Azure Backup. [Más información](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- Mantener las aplicaciones en funcionamiento: Contoso replica las máquinas virtuales de la aplicación en Azure en una región secundaria mediante Site Recovery. [Más información](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart).



### <a name="licensing-and-cost-optimization"></a>Optimización de los costos y licencias

1. Contoso tiene licencias existentes para sus VM y aprovechará la Ventaja híbrida de Azure.  Contoso convertirá las máquinas virtuales de Azure existentes para aprovechar las ventajas que ofrecen estos precios.
2. Contoso habilitará Azure Cost Management bajo licencia de Cloudyn, una subsidiaria de Microsoft. Se trata de una solución de administración de costos en varias nubes que le permitirá utilizar y administrar Azure y otros recursos en la nube. [Más información](https://docs.microsoft.com/azure/cost-management/overview) sobre Azure Cost Management. 

## <a name="conclusion"></a>Conclusión

En este artículo, Contoso rehospedó la aplicación SmartHotel360 en Azure al migrar las VM de la aplicación a VM de Azure mediante el servicio Site Recovery. 


## <a name="next-steps"></a>Pasos siguientes

En el [siguiente artículo](contoso-migration-rehost-vm-sql-ag.md) de la serie, le mostraremos cómo Contoso rehospeda la VM front-end de la aplicación SmartHotel360 en VM de Azure y cómo migra la base de datos a un grupo de disponibilidad AlwaysOn de SQL Server en Azure.

