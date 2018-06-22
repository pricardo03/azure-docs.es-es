---
title: Rehospedaje de la aplicación de consola de servicio de Linux de Contoso en Azure y Azure MySQL | Microsoft Docs
description: Obtenga información sobre cómo Contoso vuelve a hospedar una aplicación de Linux local mediante la migración de VM de Azure y Azure MySQL.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: 15a429c033cfd1598dd01b5c8cd2743c397dacdb
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/19/2018
ms.locfileid: "36225524"
---
# <a name="contoso-migration-rehost-an-on-premises-linux-app-to-azure-vms-and-azure-mysql"></a>Migración de Contoso: Rehospedaje de la aplicación de Linux local en VM de Azure y Azure MySQL

En este artículo se muestra cómo Contoso vuelve a hospedar su aplicación de consola de servicio de Linux local de dos niveles (osTicket) mediante la migración a Azure y Azure MySQL.

Este documento es el octavo de una serie de artículos que muestran cómo la compañía ficticia Contoso migra sus recursos locales a la nube de Microsoft Azure. La serie incluye información general y escenarios que ilustran cómo configurar una infraestructura de migración y ejecutar los diferentes tipos de migraciones. Los escenarios aumentan de complejidad e iremos agregando artículos adicionales a medida con el tiempo.

**Artículo** | **Detalles** | **Estado**
--- | --- | ---
[Artículo 1: Overview](contoso-migration-overview.md) (Introducción) | Proporciona información general acerca de la estrategia de migración de Contoso, la serie de artículos y las aplicaciones de ejemplo que utilizamos. | Disponible
[Artículo 2: Deploy an Azure infrastructure](contoso-migration-infrastructure.md) (Implementación de una infraestructura de Azure) | Describe cómo Contoso prepara la infraestructura local y de Azure para la migración. Se utiliza la misma infraestructura para todos los escenarios de migración de Contoso. | Disponible
[Artículo 3: Assess on-premises resources](contoso-migration-assessment.md) (Evaluación de los recursos locales)  | Muestra cómo Contoso ejecuta una evaluación de la aplicación local SmartHotel de dos niveles que se ejecuta en VMware. Evalúa las VM de la aplicación con el servicio [Azure Migrate](migrate-overview.md) y la base de datos de SQL Server de la aplicación con [Azure Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponible
[Artículo 4: Volver a hospedar máquinas virtuales de Azure y una instancia administrada de SQL](contoso-migration-rehost-vm-sql-managed-instance.md) | Muestra cómo Contoso migra la aplicación SmartHotel a Azure. La VM de la aplicación web de la aplicación se migra con [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) y la base de datos de la aplicación con [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview), para migrar a una instancia administrada de SQL. | Disponible
[Artículo 5: Volver a hospedar máquinas virtuales de Azure](contoso-migration-rehost-vm.md) | Muestra cómo Contoso migra SmartHotel a VM IaaS de Azure mediante el servicio Site Recovery.
[Article 6: Rehost to Azure VMs and SQL Server Availability Groups](contoso-migration-rehost-vm-sql-ag.md) (Artículo 6: Rehospedaje de VM de Azure y grupos de disponibilidad de SQL Server) | Muestra cómo migra Contoso la aplicación SmartHotel. Usa Site Recovery para migrar las VM de la aplicación y Database Migration Service para migrar la base de datos de la aplicación a un grupo de disponibilidad de SQL Server. | Disponible
[Artículo 7: Rehospedaje de una aplicación Linux en VM de Azure](contoso-migration-rehost-linux-vm.md) | Muestra cómo Contoso migra la aplicación osTicket de Linux a VM IaaS de Azure mediante Azure Site Recovery.
Artículo 8: Rehospedaje de una aplicación de Linux en VM de Azure y Azure MySQL Server (este artículo) | Muestra cómo Contoso migra la aplicación osTicket de Linux. Usa Site Recovery para la migración de VM y MySQL Workbench para migrar a una instancia de Azure MySQL Server. | Disponible

En este artículo, Contoso migra una aplicación de consola de servicio de Linux Apache MySQL PHP (LAMP) de dos niveles (osTicket) a Azure. Si quiere usar esta aplicación de código abierto, puede descargarla desde [GitHub](https://github.com/osTicket/osTicket).



## <a name="business-drivers"></a>Impulsores de negocio

El equipo directivo de TI ha trabajado estrechamente con sus asociados comerciales para comprender sus objetivos:

- **Responder al crecimiento empresarial**: Contoso está creciendo y, como resultado, existe una presión en la infraestructura y en los sistemas locales.
- **Limitar el riesgo**: la aplicación de consola de servicio es fundamental para el negocio de Contoso. Quiere moverla a Azure sin ningún riesgo.
- **Extensión**: Contoso no quiere cambiar la aplicación en este momento. Simplemente quiere mantenerla estable.


## <a name="migration-goals"></a>Objetivos de la migración

El equipo de la nube de Contoso ha definido los objetivos de esta migración con el fin de determinar el mejor método para llevarla a cabo:

- Tras la migración, la aplicación de Azure debe tener las mismas funcionalidades de rendimiento que las que tiene actualmente en su entorno de VMWare local.  La aplicación seguirá siendo tan imprescindible en la nube como lo es en el entorno local. 
- Contoso no quiere hacer inversiones en esta aplicación.  Aunque es importante para la empresa, simplemente quiere moverla a la nube con seguridad sin cambiarla.
- Una vez completadas un par de migraciones de aplicaciones de Windows, Contoso quiere aprender a usar una infraestructura basada en Linux en Azure.
- Quiere minimizar las tareas de administración de la base de datos tras mover la aplicación a la nube.

## <a name="proposed-architecture"></a>Arquitectura propuesta

En este escenario:

- La aplicación se divide en capas entre dos VM (OSTICKETWEB y OSTICKETMYSQL).
- Las VM están ubicadas en el host de VMware ESXi **contosohost1.contoso.com** (versión 6.5).
- vCenter Server 6.5 (**vcenter.contoso.com**), que se ejecuta en una VM, administra el entorno de VMware.
- Contoso tiene un centro de datos local (contoso-datacenter), con un controlador de dominio local (**contosodc1**).
- La aplicación de nivel de web en OSTICKETWEB se migrará a una VM IaaS de Azure.
- La base de datos de la aplicación se migrará al servicio PaaS de Azure Database for MySQL.
- Dado que va a migrar una carga de trabajo de producción, los recursos residirán en el grupo de recursos de producción **ContosoRG**.
- Los recursos se replicarán en la región principal (Este de EE. UU. 2) y se colocarán en la red de producción (VNET-PROD-EUS2):
    - La VM web residirá en la subred de front-end (PROD-FE-EUS2).
    - La instancia de la base de datos residirá en la subred de la base de datos (PROD-DB-EUS2).
- La base de datos de la aplicación se migrará a Azure MySQL con herramientas de MySQL.
- Las VM locales del centro de datos de Contoso se retirarán después de realizar la migración.


![Arquitectura del escenario](./media/contoso-migration-rehost-linux-vm-mysql/architecture.png) 


## <a name="migration-process"></a>Proceso de migración

Contoso completará el proceso de migración como se indica a continuación:

Para migrar la VM web:

1. Como primer paso, Contoso configura la infraestructura local y de Azure necesaria para implementar Site Recovery.
2. Después de preparar los componentes locales y de Azure, configura y habilita la replicación para la VM web.
3. Después de que la replicación esté en ejecución, migra la VM mediante la conmutación por error en Azure.

Migración de la base de datos:

1. Contoso proporciona una instancia de MySQL en Azure.
2. Configura MySQL Workbench y hace una copia de seguridad de la base de datos localmente.
3. A continuación, restaura la base de datos desde la copia de seguridad local en Azure.

![Proceso de migración](./media/contoso-migration-rehost-linux-vm-mysql/migration-process.png) 


### <a name="azure-services"></a>Servicios de Azure

**Servicio** | **Descripción** | **Costee**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | El servicio orquesta y administra la migración y la recuperación ante desastres de las máquinas virtuales de Azure, así como las máquinas virtuales y servidores físicos locales.  | Durante la replicación en Azure, se incurre en gastos de Azure Storage.  Las máquinas virtuales de Azure se crean, e incurren en gastos, cuando se produce una conmutación por error. [Más información](https://azure.microsoft.com/pricing/details/site-recovery/) sobre cargos y precios.
[Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/) | La base de datos se basa en el motor de MySQL Server de código abierto. Proporciona una base de datos MySQL de comunidad completamente administrada y lista para la empresa como un servicio para el desarrollo e implementación de aplicaciones. 

 
## <a name="prerequisites"></a>requisitos previos

Si usted (y Contoso) quiere ejecutar este escenario, esto es lo que debe tener.

**Requisitos** | **Detalles**
--- | ---
**Suscripción de Azure** | Ya debería haber creado una suscripción durante los primeros artículos de esta serie. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Si crea una cuenta gratuita, será el administrador de su suscripción y podrá realizar todas las acciones.<br/><br/> Si usa una suscripción existente y no es el administrador, tendrá que solicitar al administrador que le asigne permisos de propietario o colaborador.<br/><br/> Si necesita permisos más específicos, consulte [este artículo](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Infraestructura de Azure** | Contoso configura la infraestructura de Azure según se describe en [Infraestructura de Azure para la migración](contoso-migration-infrastructure.md).<br/><br/> Obtenga más información sobre los requisitos específicos de la [red](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) y el [almacenamiento](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) para Site Recovery.
**Servidores locales** | Su instancia local de vCenter Server debe ejecutarse en las versiones 5.5, 6.0 o 6.5.<br/><br/> Un host ESXi que ejecute la versión 5.5, 6.0 o 6.5<br/><br/> Una o más máquinas virtuales VMware que se ejecuten en el host ESXi.
**Máquinas virtuales locales** | [Revise los requisitos de VM Linux](https://docs.microsoft.com//azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines) que se admiten para la migración con Site Recovery.<br/><br/> Verifique los [sistemas de almacenamiento y archivos de Linux](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#linux-file-systemsguest-storage) que se admiten.<br/><br/> Las máquinas virtuales tienen que cumplir los [requisitos de Azure](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Pasos del escenario

A continuación, le mostramos cómo Azure completará la migración:

> [!div class="checklist"]
> * **Paso 1: Preparar Azure para Site Recovery**: cree una cuenta de Azure Storage que contenga los datos replicados y cree un almacén de Recovery Services.
> * **Paso 2: Preparar VMware local para Site Recovery**: prepare las cuentas para la detección de VM y la instalación del agente, y prepárese para conectarse a las VM de Azure tras una conmutación por error.
 * **Paso 3: Aprovisionar la base de datos]**: en Azure, aprovisione una instancia de base de datos Azure MySQL.
> * **Paso 4: Replicar VM**: configure el entorno de origen y de destino de Site Recovery, configure una directiva de replicación y empiece a replicar las VM en Azure Storage.
> * **Paso 5: Migrar la base de datos**: configure la migración con las herramientas de MySQL.
> * **Paso 6: Migrar las VM con Site Recovery**: ejecute una conmutación por error de prueba para asegurarse de que todo funciona y, a continuación, ejecute una conmutación por error completa para migrar las VM a Azure.




## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>Paso 1: Preparar Azure para el servicio de Site Recovery

Contoso necesita un par de componentes de Azure para Site Recovery:

- Una red virtual en la que se encuentren los recursos de conmutación por error (Contoso usará la red virtual de producción ya implementada).
- Una cuenta de Azure Storage para almacenar los datos replicados. 
- Un almacén de Recovery Services en Azure.

Contoso ya ha creado la red virtual durante la [implementación de la infraestructura de Azure](contoso-migration-infrastructure.md), por lo que solo debe crear una cuenta de almacenamiento y el almacén.


1. Contoso crea una cuenta de Azure Storage (**contosovmsacc20180528**) en la región Este de EE. UU. 2.

    - La cuenta de almacenamiento debe estar en la misma región que el almacén de Recovery Services.
    - Contoso usa una cuenta de uso general, con almacenamiento estándar, y la replicación de LRS.

    ![Almacenamiento de Site Recovery](./media/contoso-migration-rehost-linux-vm-mysql/asr-storage.png)

3. Con la cuenta de almacenamiento y la red implementadas, Contoso crea un almacén (ContosoMigrationVault) y lo coloca en el grupo de recursos **ContosoFailoverRG**, en la región Este de EE. UU. 2 principal.

    ![Almacén de Recovery Services](./media/contoso-migration-rehost-linux-vm-mysql/asr-vault.png)

**¿Necesita más ayuda?**

[Más información](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) sobre cómo configurar Azure para Site Recovery.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>Paso 2: Preparar VMware local para Site Recovery

Contoso prepara la infraestructura local de VMware como se indica a continuación:

- Crea una cuenta en vCenter Server para automatizar la detección de VM.
- Crea una cuenta que permite la instalación automática de Mobility Service en cada VM de VMware que quiere replicar.
- Prepara VM locales para que puedan conectarse a las VM de Azure cuando se creen tras la migración.


### <a name="prepare-an-account-for-automatic-discovery"></a>Preparación de una cuenta de detección automática

Site Recovery necesita acceso a los servidores de VMware para:

- Detectar automáticamente las máquinas virtuales. Se requiere al menos una cuenta de solo lectura.
- Coordinar la replicación, la conmutación por error y la conmutación por recuperación. Necesita una cuenta que pueda ejecutar operaciones como la creación y eliminación de discos, así como la activación de máquinas virtuales.

Contoso configura la cuenta como se indica a continuación:

1. Crea un rol en el nivel de vCenter.
2. A continuación, asigna los permisos necesarios a ese rol.


### <a name="prepare-an-account-for-mobility-service-installation"></a>Preparación de una cuenta para la instalación de Mobility Service

Mobility Service debe instalarse en todas las VM que Contoso quiera migrar.

- Site Recovery puede hacer una instalación de inserción automática de este componente cuando habilita la replicación para las VM.
- Para la instalación automática. Site Recovery necesita una cuenta con permisos para tener acceso a la VM. 
- Los detalles de la cuenta se especifican durante la configuración de la replicación. 
- La cuenta puede ser una cuenta de dominio o local, siempre y cuando tenga permisos de instalación.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Preparación para la conexión a las máquinas virtuales de Azure después de la conmutación por error

Después de la conmutación por error en Azure, Contoso quiere poder conectarse a las VM de Azure. Para ello, es necesario hacer un par de cosas: 

- Para obtener acceso a través de Internet, habilita SSH en la VM Linux local antes de la migración.  En el caso de Ubuntu, puede realizarse mediante el siguiente comando: **Sudo apt-get ssh install -y**.
- Después de la conmutación por error, comprueba los **Diagnósticos de arranque** para ver una captura de pantalla de la VM.
- Si no funciona, debe comprobar que se está ejecutando la VM, así como revisar estas [sugerencias de solución de problemas](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

**¿Necesita más ayuda?**

- [Averigüe](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) cómo puede crear y asignar un rol para la detección automática.
- [Más información](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) sobre cómo crear una cuenta para la instalación de inserción de Mobility Service.


## <a name="step-3-provision-azure-database-for-mysql"></a>Paso 3: Aprovisionar Azure Database for MySQL

Contoso aprovisiona una instancia de base de datos MySQL en la región Este de EE. UU. 2 principal.

1. En Azure Portal, crea un recurso de Azure Database for MySQL. 

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-1.png)

2. Agrega el nombre **contosoosticket** para la base de datos de Azure. Agrega la base de datos al grupo de recursos de producción **ContosoRG** y especifica las credenciales.
3. La base de datos MySQL local es de la versión 5.7, por lo que selecciona esta versión por cuestiones de compatibilidad. Utiliza los tamaños predeterminados, que coinciden con los requisitos de la base de datos.

     ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-2.png)

4. Para las **opciones de redundancia de copia de seguridad**, Contoso selecciona el uso de la **redundancia geográfica**. Esta opción permite restaurar la base de datos en la región secundaria del Centro de EE. UU. si se produce una interrupción. Solo puede configurar esta opción al aprovisionar la base de datos.

     ![Redundancia](./media/contoso-migration-rehost-linux-vm-mysql/db-redundancy.png)

4. En la red **VNET-PROD-EUS2** > **Puntos de conexión de servicio**, agrega un punto de conexión de servicio (una subred de la base de datos) para el servicio SQL.

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-3.png)

5. Tras agregar la subred, crea una regla de red virtual que permite obtener acceso desde la subred de la base de datos de la red de producción.

    ![MySQL](./media/contoso-migration-rehost-linux-vm-mysql/mysql-4.png)


## <a name="step-4-replicate-the-on-premises-vms"></a>Paso 4: Replicar VM locales

Para poder migrar la VM web a Azure, Contoso configura y habilita la replicación.

### <a name="set-a-protection-goal"></a>Establecimiento de un objetivo de protección

1. En el nombre del almacén (ContosoVMVault), establece un objetivo de replicación (**Introducción** > **Site Recovery** > **Preparar infraestructura**.
2. Especifica que las máquinas se encuentran en el entorno local, que son VM de VMware y que quieren replicarse en Azure.

    ![Objetivo de replicación](./media/contoso-migration-rehost-linux-vm-mysql/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Confirmación del planeamiento de la implementación

Para continuar, confirma que se ha completado la planificación de implementación mediante la selección de la opción **Sí, ya lo hice**. Contoso solo va a migrar una única VM en este escenario, por lo que no es necesario planear la implementación.

### <a name="set-up-the-source-environment"></a>Configuración del entorno de origen

Contoso debe configurar el entorno de origen. Para ello, mediante una plantilla OVF, implementa un servidor de configuración de Site Recovery como una VM local de VMware de alta disponibilidad. Una vez que el servidor de configuración se encuentra en funcionamiento, lo registra en el almacén.

El servidor de configuración ejecuta una serie de componentes:

- El componente del servidor de configuración que coordina las comunicaciones entre el entorno local y Azure y administra la replicación de datos.
- El servidor de procesos que actúa como puerta de enlace de replicación. Recibe datos de replicación, los optimiza con almacenamiento en caché, compresión y cifrado, y los envía a Azure Storage.
- El servidor de procesos también instala Mobility Service en las máquinas virtuales que quiere replicar, además de realizar la detección automática de las máquinas virtuales de VMware locales.

Contoso realiza estos pasos como se indica a continuación:


1. Descarga la plantilla OVF desde **Preparar infraestructura** > **Origen** > **Servidor de configuración**.
    
    ![Descarga de OVF](./media/contoso-migration-rehost-linux-vm-mysql/add-cs.png)

2. Importa la plantilla en VMware para crear la VM e implementa la VM.

    ![Plantilla de OVF](./media/contoso-migration-rehost-linux-vm-mysql/vcenter-wizard.png)

3. Al activar la VM por primera vez, inicia en una experiencia de instalación de Windows Server 2016. Acepta el contrato de licencia e introduce una contraseña de administrador.
4. Una vez finalizada la instalación, inicia sesión en la VM como administrador. Cuando se inicia sesión por primera vez, se ejecuta la herramienta de configuración de Azure Site Recovery de manera predeterminada.
5. En la herramienta, especifica un nombre que se usará para registrar el servidor de configuración en el almacén.
6. La herramienta comprueba que la máquina virtual pueda conectarse a Azure.
7. Una vez establecida la conexión, inicia sesión en la suscripción a Azure. Las credenciales deben tener acceso al almacén donde se registrará el servidor de configuración.

    ![Registrar servidor de configuración](./media/contoso-migration-rehost-linux-vm-mysql/config-server-register2.png)

8. La herramienta realiza algunas tareas de configuración y, a continuación, se reinicia.
9. Vuelve a iniciar sesión en la máquina y el Asistente para administración del servidor de configuración se inicia automáticamente.
10. En el asistente, selecciona la NIC para recibir tráfico de replicación. Una vez configurada, esta opción no se puede cambiar.
11. Selecciona la suscripción, el grupo de recursos y el almacén en el que se va a registrar el servidor de configuración.

    ![almacén](./media/contoso-migration-rehost-linux-vm-mysql/cswiz1.png) 

12. Ahora, descarga e instala MySQL Server y VMWare PowerCLI. 
13. Después de la validación, especifica la dirección IP o el FQDN del host de vSphere o vCenter Server. Deja el puerto predeterminado y especifica un nombre descriptivo para vCenter Server.
14. Introduce la cuenta que ha creado para la detección automática, así como las credenciales que utilizará Site Recovery para instalar automáticamente Mobility Service. 

    ![vCenter](./media/contoso-migration-rehost-linux-vm-mysql/cswiz2.png)

14. Cuando finaliza el registro, en Azure Portal, Contoso verifica si el servidor de configuración y VMware Server aparecen en la página **Origen** del almacén. La detección puede tardar 15 minutos o más. 
15. Cuando esté todo listo, Site Recovery se conecta a los servidores de VMware y detecta VM.

### <a name="set-up-the-target"></a>Configuración del destino

Ahora, Contoso introduce la configuración de replicación de destino.

1. En **Preparar infraestructura** > **Destino**, selecciona la configuración de destino.
2. Site Recovery comprueba si existe una red y una cuenta de almacenamiento de Azure en el destino especificado.


### <a name="create-a-replication-policy"></a>Creación de una directiva de replicación

Con el origen y destino configurados, Contoso está listo para crear una directiva de replicación.

1. En **Preparar infraestructura** > **Configuración de la replicación** > **Directiva de replicación** >  **Crear y asociar**, crea una directiva denominada **ContosoMigrationPolicy**.
2. Usa la configuración predeterminada:
    - **Umbral de RPO**: valor predeterminado de 60 minutos. Este valor define la frecuencia de creación de puntos de recuperación. Se genera una alerta cuando la replicación continua supera este límite.
    - **Retención de punto de recuperación**. Valor predeterminado de 24 horas. Este valor especifica cuánto tiempo dura el período de retención para cada punto de recuperación. Las máquinas virtuales replicadas se pueden recuperar a cualquier momento de un período.
    - **Frecuencia de las instantáneas coherentes con la aplicación**. Valor predeterminado de una hora. Este valor especifica la frecuencia con la que se crean instantáneas coherentes con la aplicación.
 
        ![Creación de la directiva de replicación](./media/contoso-migration-rehost-linux-vm-mysql/replication-policy.png)

5. La directiva se asocia automáticamente al servidor de configuración. 

    ![Asociación de la directiva de replicación](./media/contoso-migration-rehost-linux-vm-mysql/replication-policy2.png)


**¿Necesita más ayuda?**

- Puede leer un tutorial completo de todos estos pasos en [Configurar la recuperación ante desastres para VM de VMware locales](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Puede encontrar instrucciones detalladas que le ayudarán a [configurar el entorno de origen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [implementar el servidor de configuración](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server) y [establecer la configuración de replicación](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- [Más información](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) sobre el agente invitado de Azure para Linux.

### <a name="enable-replication-for-the-web-vm"></a>Habilitación de la replicación de la VM web

Contoso ya puede empezar a replicar la VM **OSTICKETWEB**.

1. En **Replicar aplicación** > **Origen** > **+Replicar**, selecciona la configuración de origen.
2. Indica que quiere habilitar las máquinas virtuales y selecciona la configuración de origen, incluidos vCenter Server y el servidor de configuración.

    ![Habilitar replicación](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication-source.png)

3. Ahora, especifica las opciones de configuración de destino. Entre otras, se incluye el grupo de recursos y una red en la que la VM de Azure se ubicará después de la conmutación por error, así como una cuenta de almacenamiento en la que se almacenarán los datos replicados. 

     ![Habilitar replicación](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication2.png)

3. Contoso selecciona **OSTICKETWEB** para la replicación. 

    ![Habilitar replicación](./media/contoso-migration-rehost-linux-vm-mysql/enable-replication3.png)

4. En las propiedades de la VM, selecciona la cuenta que se debe usar para instalar automáticamente Mobility Service en la VM.

     ![Mobility Service](./media/contoso-migration-rehost-linux-vm-mysql/linux-mobility.png)

5. En **Configuración de la replicación** > **Establecer configuración de replicación**, comprueba que se haya aplicado la directiva de replicación correcta y selecciona **Habilitar replicación**. Mobility Service se instalará automáticamente.
6.  Realiza un seguimiento del progreso de la replicación en **Trabajos**. La máquina estará preparada para la conmutación por error después de que finalice el trabajo **Finalizar la protección**.


**¿Necesita más ayuda?**

Puede leer un tutorial completo de todos estos pasos en [Habilitar replicación](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-5-migrate-the-database"></a>Paso 5: Migrar la base de datos

Contoso migrará la base de datos mediante la copia de seguridad y la restauración con herramientas de MySQL. Instala MySQL Workbench, hace una copia de seguridad de la base de datos de OSTICKETMYSQL y, a continuación, la restaura al servidor de Azure Database for MySQL.

### <a name="install-mysql-workbench"></a>Instalación de MySQL Workbench

1. Contoso comprueba los [requisitos previos y descarga MySQL Workbench](https://dev.mysql.com/downloads/workbench/?utm_source=tuicool).
2. Instala MySQL Workbench para Windows de acuerdo con las [instrucciones de instalación](https://dev.mysql.com/doc/workbench/en/wb-installing.html).
3. En MySQL Workbench, crea una conexión de MySQL a OSTICKETMYSQL. 

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench1.png)

4. Exporta la base de datos como **osticket** a un archivo independiente local.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench2.png)

5. Después de realizar la copia de seguridad de la base de datos localmente, crea una conexión a la instancia de Azure Database for MySQL.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench3.png)

6. Ahora, Contoso puede importar (restaurar) la base de datos de la instancia de Azure MySQL desde el archivo independiente. Se crea un nuevo esquema (osticket) para la instancia.

    ![MySQL Workbench](./media/contoso-migration-rehost-linux-vm-mysql/workbench4.png)

## <a name="step-6-migrate-the-vms-with-site-recovery"></a>Paso 6: Migrar las VM con Site Recovery

Contoso ejecuta una conmutación por error rápida de prueba y, a continuación, migra la VM.

### <a name="run-a-test-failover"></a>Ejecución de una conmutación por error de prueba

La ejecución de una conmutación por error de prueba ayuda a comprobar que todo funciona según lo previsto antes de la migración. 

1. Contoso ejecuta una conmutación por error de prueba en el punto más reciente en el tiempo disponible (**procesado más recientemente**).
2. Selecciona **Shut down machine before beginning failover** (Apagar la máquina antes de comenzar con la conmutación por error), para que Site Recovery intente apagar la VM de origen antes de desencadenar la conmutación por error. La conmutación por error continúa aunque se produzca un error de cierre. 
3. Se ejecuta la conmutación por error de prueba: 

    - Se ejecuta una comprobación de requisitos previos para garantizar que todas las condiciones necesarias para la conmutación por error están correctamente establecidas.
    - La conmutación por error procesa los datos, de modo que se pueda crear una máquina virtual de Azure. Si se selecciona el último punto de recuperación, se crea un punto de recuperación a partir de los datos.
    - Se crea una VM de Azure mediante los datos procesados en el paso anterior.

3. Una vez finalizada la conmutación por error, la VM de Azure de réplica aparece en Azure Portal. Contoso comprueba que la VM tiene el tamaño adecuado, que está conectada a la red correspondiente y que se está ejecutando. 
4. Después, limpia la conmutación por error y registra y guarda las observaciones.

### <a name="migrate-the-vm"></a>Migración de la VM

Para migrar la VM, Contoso crea un plan de recuperación que incluye la VM y conmuta el plan por error en Azure.

1. Contoso crea un plan y le agrega **OSTICKETWEB**.

    ![Plan de recuperación](./media/contoso-migration-rehost-linux-vm-mysql/recovery-plan.png)

2. Ejecuta una conmutación por error en el plan. Selecciona el punto de recuperación más reciente y especifica que Site Recovery debe intentar apagar la VM local antes de desencadenar la conmutación por error. Puede seguir el progreso de la conmutación por error en la página **Trabajos**.

    ![Conmutación por error](./media/contoso-migration-rehost-linux-vm-mysql/failover1.png)

3. Durante la conmutación por error, vCenter Server emite comandos para detener las dos VM que se ejecutan en el host ESXi.

    ![Conmutación por error](./media/contoso-migration-rehost-linux-vm-mysql/vcenter-failover.png)

4. Después de la conmutación por error, Contoso comprueba si la VM de Azure aparece según lo esperado en Azure Portal.

    ![Conmutación por error](./media/contoso-migration-rehost-linux-vm-mysql/failover2.png)  

5. Después de comprobar la VM, completa la migración. Con esta acción se detienen la replicación de la VM y la facturación de Site Recovery para la VM.

    ![Conmutación por error](./media/contoso-migration-rehost-linux-vm-mysql/failover3.png)

**¿Necesita más ayuda?**

- [Más información](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) sobre la ejecución de una conmutación por error de prueba. 
- [Más información](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) sobre cómo crear un plan de recuperación.
- [Más información](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) sobre cómo crear una conmutación por error en Azure.


### <a name="connect-the-vm-to-the-database"></a>Conexión de la VM a la base de datos

Como último paso del proceso de migración, Contoso actualiza la cadena de conexión de la aplicación para que apunte a Azure Database for MySQL. 

1. Realiza una conexión SSH a la VM OSTICKETWEB mediante Putty u otro cliente SSH. La VM es privada, por lo que se conecta mediante la dirección IP privada.

    ![Conexión a la base de datos](./media/contoso-migration-rehost-linux-vm-mysql/db-connect.png)  

    ![Conexión a la base de datos](./media/contoso-migration-rehost-linux-vm-mysql/db-connect2.png)  

2. Actualiza la configuración para que la VM **OSTICKETWEB** pueda comunicarse con la base de datos **OSTICKETMYSQL**. Actualmente, la configuración está codificada de forma rígida con la dirección IP local 172.16.0.43.

    **Antes de la actualización**
    
    ![Actualización de la IP](./media/contoso-migration-rehost-linux-vm-mysql/update-ip1.png)  

    **Después de la actualización**
    
    ![Actualización de la IP](./media/contoso-migration-rehost-linux-vm-mysql/update-ip2.png) 
    
    ![Actualización de la IP](./media/contoso-migration-rehost-linux-vm-mysql/update-ip3.png) 

3. Reinicia el servicio con **systemctl restart apache2**.

    ![Reinicio](./media/contoso-migration-rehost-linux-vm-mysql/restart.png) 

4. Por último, actualiza los registros DNS de **OSTICKETWEB** en uno de los controladores de dominio de Contoso.

    ![Actualización de DNS](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 


##  <a name="clean-up-after-migration"></a>Limpiar después de la migración

Al finalizar la migración, los niveles de la aplicación osTicket se ejecutan en VM de Azure.

Ahora, Contoso debe hacer lo siguiente: 
- Quitar las VM de VMware del inventario de vCenter.
- Quitar las VM locales de los trabajos de copia de seguridad locales.
- Actualizar la documentación interna para que muestre las nuevas ubicaciones y las direcciones IP. 
- Revisar los recursos que interactúan con las VM locales y actualizar los valores de configuración pertinentes o la documentación para reflejar la nueva configuración.
- Contoso usaba el servicio de Azure Migrate con la asignación de dependencia para evaluar la VM **OSTICKETWEB** para la migración. Ahora, debe quitar de la VM los agentes (Microsoft Monitoring Agent o Dependency Agent) que se instalaron con este propósito.

## <a name="review-the-deployment"></a>Revisión de la implementación

Con la aplicación en ejecución, Contoso debe proteger y poner totalmente operativa la infraestructura nueva.

### <a name="security"></a>Seguridad

El equipo de seguridad de Contoso revisa la VM y la base de datos para determinar cualquier problema de seguridad.

- Revisa los grupos de seguridad de red (NSG) de la VM con el fin de controlar el acceso. Los NSG se usan para garantizar que solo el tráfico permitido puede pasar a la aplicación.
- Considera la posibilidad de proteger los datos en los discos de la VM mediante el cifrado de discos y Azure Key Vault.
- La comunicación entre la instancia de la base de datos y la VM no está configurada para SSL. Deberá realizar esta acción para garantizar que el tráfico de la base de datos no pueda piratearse.

[Más información](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) sobre los procedimientos de seguridad recomendados para VM.

### <a name="backups"></a>Copias de seguridad

- Contoso realizará la copia de seguridad de los datos de la VM mediante el servicio de Azure Backup. [Más información](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- No deberá configurar la copia de seguridad de la base de datos. Azure Database for MySQL crea automáticamente copias de seguridad del servidor y las almacena. Contoso optó por utilizar la redundancia geográfica para la base de datos para que sea resistente y esté lista para la producción.

### <a name="licensing-and-cost-optimization"></a>Optimización de los costos y licencias

- Después de implementar los recursos, Contoso asigna etiquetas de Azure de acuerdo con las decisiones que se tomaron durante la implementación de la [infraestructura de Azure](contoso-migration-infrastructure.md#set-up-tagging).
- No hay ningún problema relacionado con las licencias para los servidores de Ubuntu de Contoso.
- Contoso habilitará Azure Cost Management bajo licencia de Cloudyn, una subsidiaria de Microsoft. Es una solución de administración de costos en varias nubes que le permitirá utilizar y administrar Azure y otros recursos en la nube.  [Más información](https://docs.microsoft.com/azure/cost-management/overview) sobre Azure Cost Management.


## <a name="next-steps"></a>Pasos siguientes

En este artículo, se mostró el escenario de rehospedaje final que Contoso probó. Se migró la VM de front-end de la aplicación osTicket de Linux local a una VM de Azure, así como la base de datos de la aplicación a una instancia de Azure MySQL.

En el siguiente conjunto de tutoriales de la serie de migración, le mostraremos cómo Contoso realiza un conjunto de migraciones más complejo, que implica la refactorización de la aplicación, en lugar de las migraciones simples mediante lift-and-shift.
