---
title: 'Rehospedaje: migración y rehospedaje de una aplicación de Linux local en VM de Azure | Microsoft Docs'
description: Obtenga información sobre cómo Contoso vuelve a hospedar una aplicación de Linux local mediante la migración de VM de Azure.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 07/12/2018
ms.author: raynew
ms.openlocfilehash: 6c96beee347a7a36a3dc04ecf8cd994484fd6bb7
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/13/2018
ms.locfileid: "39007258"
---
# <a name="contoso-migration-rehost-an-on-premises-linux-app-to-azure-vms"></a>Migración de Contoso: rehospedaje de una aplicación de Linux local en VM de Azure

En este artículo se muestra cómo Contoso vuelve a hospedar una aplicación de consola de servicio basada en Linux local (**osTicket**) en las VM IaaS de Azure.

Este documento es el primero de una serie de artículos que documentan cómo la compañía ficticia Contoso migra sus recursos locales a la nube de Microsoft Azure. La serie incluye información general y un conjunto de escenarios que ilustran cómo configurar una infraestructura de migración y ejecutar los diferentes tipos de migraciones. Los escenarios aumentan de complejidad e iremos agregando artículos adicionales a medida con el tiempo.

**Artículo** | **Detalles** | **Estado**
--- | --- | ---
[Artículo 1: Introducción](contoso-migration-overview.md) | Se proporciona una introducción a la estrategia de migración de Contoso, la serie de artículos y las aplicaciones de ejemplo que usamos. | Disponible
[Artículo 2: Deploy an Azure infrastructure](contoso-migration-infrastructure.md) (Implementación de una infraestructura de Azure) | Se describe cómo Contoso prepara su infraestructura local y de Azure para la migración. Se usa la misma infraestructura para todos los escenarios de migración de Contoso. | Disponible
[Artículo 3: Assess on-premises resources](contoso-migration-assessment.md) (Evaluación de los recursos locales)  | Se muestra cómo Contoso realiza una valoración de su aplicación de dos niveles local SmartHotel que se ejecuta en VMware. Se evalúan las VM de la aplicación mediante el servicio [Azure Migrate](migrate-overview.md) y la base de datos de SQL Server de aplicaciones con [Azure Database Migration Assistant](https://docs.microsoft.com/sql/dma/dma-overview?view=sql-server-2017). | Disponible
[Artículo 4: Rehospedar en máquinas virtuales de Azure y una instancia administrada de SQL](contoso-migration-rehost-vm-sql-managed-instance.md) | Muestra cómo Contoso migra la aplicación SmartHotel a Azure. La VM de front-end de la aplicación se migra mediante [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) y la base de datos de aplicaciones mediante el servicio [Azure Database Migration](https://docs.microsoft.com/azure/dms/dms-overview) para migrar a una instancia administrada de SQL. | Disponible
[Artículo 5: Volver a hospedar máquinas virtuales de Azure](contoso-migration-rehost-vm.md) | Se muestra cómo Contoso migra sus VM de la aplicación de SmartHotel a las VM de Azure. | Disponible
[Artículo 6: Rehospedar en VM de Azure y grupos de disponibilidad de SQL Server](contoso-migration-rehost-vm-sql-ag.md) | Muestra cómo migra Contoso la aplicación SmartHotel. Usa Site Recovery para migrar las VM de la aplicación y el servicio Database Migration para migrar la base de datos de aplicaciones a un grupo de disponibilidad de SQL Server. | Disponible
Artículo 7: Rehospedar una aplicación de Linux en VM de Azure | Muestra cómo Contoso migra la aplicación osService de Linux mediante Azure Site Recovery. | Este artículo.
[Artículo 8: Rehospedaje de una aplicación de Linux en VM de Azure y en Azure MySQL Server](contoso-migration-rehost-linux-vm-mysql.md) | Muestra cómo Contoso migra la aplicación osService de Linux mediante el uso de Site Recovery para la migración de VM, y MySQL Workbench para migrar a una instancia de Azure MySQL Server. | Disponible
[Artículo 9: Refactorizar una aplicación en Azure Web Apps y Azure SQL Database](contoso-migration-refactor-web-app-sql.md) | Muestra cómo Contoso migra la aplicación de SmartHotel a una aplicación web de Azure y migra la base de datos de la aplicación a la instancia de Azure SQL Server | Disponible
[Artículo 10: Refactorizar una aplicación Linux en Azure Web Apps y Azure MySQL](contoso-migration-refactor-linux-app-service-mysql.md) | Muestra cómo Contoso migra la aplicación Linux osTicket a Azure Web Apps en varios sitios integrados con GitHub para la entrega continua. Migran la base de datos de la aplicación a una instancia de Azure MySQL. | Disponible
[Artículo 11: Refactorizar TFS en VSTS](contoso-migration-tfs-vsts.md) | Muestra cómo Contoso migra su implementación de Team Foundation Server (TFS) local mediante la migración a Visual Studio Team Services (VSTS) en Azure. | Disponible
[Artículo 12: Rediseño de la arquitectura de una aplicación en Azure Containers y Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | Muestra cómo Contoso migra la aplicación de SmartHotel a Azure y rediseña su arquitectura. Se rediseña la arquitectura del nivel web de la aplicación como contenedor de Windows, y la base de datos de la aplicación en una instancia de Azure SQL Database. | Disponible
[Artículo 13: Volver a compilar una aplicación en Azure](contoso-migration-rebuild.md) | Muestra cómo Contoso vuelve a compilar su aplicación de SmartHotel con una gama de funcionalidades y servicios de Azure, como App Services, Azure Kubernetes, Azure Functions, Cognitive Services y Cosmos DB. | Disponible

En este artículo, Contoso migrará a Azure la aplicación **osTicket** de dos niveles, que se ejecuta en Linux Apache MySQL PHP (LAMP). Las VM de la aplicación se migrarán con el servicio de Azure Site Recovery. Si quiere usar esta aplicación de código abierto, puede descargarla desde [GitHub](https://github.com/osTicket/osTicket).

## <a name="business-drivers"></a>Impulsores de negocio

El equipo directivo de TI ha trabajado estrechamente con sus asociados comerciales para comprender lo que quieren lograr con esta migración:

- **Responder al crecimiento empresarial**: Contoso está creciendo y, como resultado, existe una presión en la infraestructura y en los sistemas locales.
- **Limitar el riesgo**: la aplicación de consola de servicio es fundamental para el negocio de Contoso. Quiere moverla a Azure sin ningún riesgo.
- **Extensión**: no quiere cambiar la aplicación en este momento. Simplemente quiere asegurarse de que es estable.


## <a name="migration-goals"></a>Objetivos de la migración

El equipo de la nube de Contoso ha definido los objetivos de esta migración con el fin de determinar el mejor método para llevarla a cabo:

- Tras la migración, la aplicación de Azure debe tener las mismas funcionalidades de rendimiento que las que tiene actualmente en su entorno de VMWare local.  La aplicación seguirá siendo tan imprescindible en la nube como lo es en el entorno local. 
- Contoso no quiere hacer inversiones en esta aplicación.  Es importante para la empresa, pero, simplemente, quiere moverla a la nube de modo seguro en su forma actual.
- Contoso no quiere cambiar el modelo de operaciones de esta aplicación. Quiere interactuar con ella en la nube de la misma manera que lo hace ahora.
- Contoso no quiere cambiar la funcionalidad de la aplicación. Solo cambiará su ubicación.
- Una vez completadas un par de migraciones de aplicaciones de Windows, Contoso quiere aprender a usar una infraestructura basada en Linux en Azure.

## <a name="proposed-architecture"></a>Arquitectura propuesta

En este escenario:

- La aplicación se divide en capas entre dos VM (OSTICKETWEB y OSTICKETMYSQL).
- Las VM están ubicadas en el host de VMware ESXi **contosohost1.contoso.com** (versión 6.5).
- vCenter Server 6.5 (**vcenter.contoso.com**), que se ejecuta en una VM, administra el entorno de VMware.
- Contoso tiene un centro de datos local (**contoso-datacenter**), con un controlador de dominio local (**contosodc1**).
- Dado que la aplicación es una carga de trabajo de producción, las VM de Azure residirán en el grupo de recursos de producción **ContosoRG**.
- Las VM migrarán a la región principal (Este de EE. UU. 2) y se colocarán en la red de producción (VNET-PROD-EUS2):
    - La VM web residirá en la subred de front-end (PROD-FE-EUS2).
    - La VM de la base de datos residirá en la subred de la base de datos (PROD-DB-EUS2).
- Las VM locales del centro de datos de Contoso se retirarán después de realizar la migración.

![Arquitectura del escenario](./media/contoso-migration-rehost-linux-vm/architecture.png) 

## <a name="migration-process"></a>Proceso de migración

Contoso realizará la migración como se indica a continuación:

1. Como primer paso, Contoso configura la infraestructura local y de Azure necesaria para implementar Site Recovery.
2. Después de preparar los componentes locales y de Azure, configura y habilita la replicación para las VM.
3. Una vez que la replicación está en ejecución, migra las VM mediante la conmutación por error en Azure.

![Proceso de migración](./media/contoso-migration-rehost-linux-vm/migration-process.png)

### <a name="azure-services"></a>Servicios de Azure

**Servicio** | **Descripción** | **Costee**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | El servicio orquesta y administra la migración y la recuperación ante desastres de las máquinas virtuales de Azure, así como las máquinas virtuales y servidores físicos locales.  | Durante la replicación en Azure, se incurre en gastos de Azure Storage.  Las máquinas virtuales de Azure se crean, e incurren en gastos, cuando se produce una conmutación por error. [Más información](https://azure.microsoft.com/pricing/details/site-recovery/) sobre cargos y precios.

 
## <a name="prerequisites"></a>Requisitos previos

Esto es lo que usted (y Contoso) necesitan para este escenario.

**Requisitos** | **Detalles**
--- | ---
**Suscripción de Azure** | Ya debería haber creado una suscripción durante los primeros artículos de esta serie. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Si crea una cuenta gratuita, será el administrador de su suscripción y podrá realizar todas las acciones.<br/><br/> Si usa una suscripción existente y no es el administrador, tendrá que solicitar al administrador que le asigne permisos de propietario o colaborador.<br/><br/> Si necesita permisos más específicos, consulte [este artículo](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Infraestructura de Azure** | Contoso configura la infraestructura de Azure según se describe en [Infraestructura de Azure para la migración](contoso-migration-infrastructure.md).<br/><br/> Obtenga más información sobre los requisitos específicos de la [red](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) y el [almacenamiento](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) para Site Recovery.
**Servidores locales** | Su instancia local de vCenter Server debe ejecutarse en las versiones 5.5, 6.0 o 6.5.<br/><br/> Un host ESXi que ejecute la versión 5.5, 6.0 o 6.5<br/><br/> Una o más máquinas virtuales VMware que se ejecuten en el host ESXi.
**Máquinas virtuales locales** | [Revise las máquinas Linux](https://docs.microsoft.com//azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines) que se admiten para la migración con Site Recovery.<br/><br/> Verifique los [sistemas de almacenamiento y archivos de Linux](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#linux-file-systemsguest-storage) que se admiten.<br/><br/> Las máquinas virtuales tienen que cumplir los [requisitos de Azure](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Pasos del escenario

A continuación, le mostramos cómo Azure completará la migración:

> [!div class="checklist"]
> * **Paso 1: Preparar Azure para Site Recovery**: crea una cuenta de Azure Storage que contiene los datos replicados, así como un almacén de Recovery Services.
> * **Paso 2: Preparar VMware local para Site Recovery**: prepara las cuentas para la detección de VM y la instalación del agente, así como para conectarse a las VM de Azure tras una conmutación por error.
> * **Paso 3: Replicar VM**: configura el entorno de migración de origen y de destino, crea una directiva de replicación y empieza a replicar las VM en Azure Storage.
> * **Paso 4: Migrar las VM con Site Recovery**: ejecuta una conmutación por error de prueba para garantizar que todo funciona y, a continuación, ejecuta una conmutación por error completa para migrar las VM a Azure.


## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>Paso 1: Preparar Azure para el servicio de Site Recovery

Contoso necesita un par de componentes de Azure para Site Recovery:

- Una red virtual en la que se encuentren los recursos de conmutación por error (Contoso usará la red virtual de producción ya implementada).
- Una cuenta de Azure Storage para almacenar los datos replicados. 
- Un almacén de Recovery Services en Azure.

Contoso ya ha creado la red virtual durante la [implementación de la infraestructura de Azure](contoso-migration-infrastructure.md), por lo que solo debe crear una cuenta de almacenamiento y el almacén.

1. Contoso crea una cuenta de Azure Storage (contosovmsacc20180528) en la región Este de EE. UU. 2.

    - La cuenta de almacenamiento debe estar en la misma región que el almacén de Recovery Services.
    - Usa una cuenta de uso general, con almacenamiento estándar, y la replicación de LRS.

    ![Almacenamiento de Site Recovery](./media/contoso-migration-rehost-linux-vm/asr-storage.png)

2. Con la cuenta de almacenamiento y la red implementados, Contoso crea un almacén (ContosoMigrationVault) y lo coloca en el grupo de recursos **ContosoFailoverRG**, en la región Este de EE. UU. 2 principal.

    ![Almacén de Recovery Services](./media/contoso-migration-rehost-linux-vm/asr-vault.png)


**¿Necesita más ayuda?**

Obtenga [más información](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) sobre cómo configurar Azure para Site Recovery.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>Paso 2: Preparar VMware local para Site Recovery

Contoso prepara la infraestructura local de VMware como se indica a continuación:

- Crea una cuenta en el host de vCenter Server o vSphere ESXi para automatizar la detección de VM.
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

Mobility Service debe instalarse en todas las VM Linux que Contoso va a migrar:

- Site Recovery puede hacer una instalación de inserción automática de este componente cuando habilita la replicación para las VM.
- Para la instalación de inserción automática, tiene que preparar una cuenta que Site Recovery vaya a usar para obtener acceso a las VM.
- Los detalles de las cuentas se especifican durante la configuración de la replicación. 
- La cuenta puede ser una cuenta de dominio o local con permisos para hacer instalaciones en VM.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Preparación para la conexión a las máquinas virtuales de Azure después de la conmutación por error

Después de la conmutación por error en Azure, Contoso quiere poder conectarse a las VM replicadas de Azure. Para ello, es necesario hacer un par de cosas:

- Para obtener acceso a través de Internet, habilita SSH en la VM Linux local antes de la migración.  En el caso de Ubuntu, puede realizarse mediante el siguiente comando: **Sudo apt-get ssh install -y**.
- Después de ejecutar la migración (conmutación por error), debe comprobar los **Diagnósticos de arranque** para ver una captura de pantalla de la VM.
- Si no funciona, debe comprobar que se está ejecutando la VM, así como revisar estas [sugerencias de solución de problemas](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**¿Necesita más ayuda?**

- [Averigüe](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) cómo puede crear y asignar un rol para la detección automática.
- Obtenga [más información](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) sobre cómo crear una cuenta para la instalación de inserción de Mobility Service.


## <a name="step-3-replicate-the-on-premises-vms"></a>Paso 3: Replicar VM locales

Para poder migrar la VM web a Azure, Contoso configura y habilita la replicación.

### <a name="set-a-protection-goal"></a>Establecimiento de un objetivo de protección

1. En el nombre del almacén (ContosoVMVault), establece un objetivo de replicación (**Introducción** > **Site Recovery** > **Preparar infraestructura**.
2. Especifica que las máquinas se encuentran en el entorno local, que son VM de VMware y que quieren replicarse en Azure.
    ![Objetivo de replicación](./media/contoso-migration-rehost-linux-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Confirmación del planeamiento de la implementación

Para continuar, confirma que se ha completado la planificación de implementación mediante la selección de la opción **Sí, ya lo hice**. Contoso solo va a migrar una única VM en este escenario, por lo que no es necesario planear la implementación.

### <a name="set-up-the-source-environment"></a>Configuración del entorno de origen

Contoso debe configurar el entorno de origen. Para ello, descarga una plantilla OVF y la usa para implementar el servidor de configuración de Site Recovery como una VM local de VMware altamente disponible. Una vez que el servidor de configuración se encuentra en funcionamiento, lo registra en el almacén.

El servidor de configuración ejecuta una serie de componentes:

- El componente del servidor de configuración que coordina las comunicaciones entre el entorno local y Azure y administra la replicación de datos.
- El servidor de procesos que actúa como puerta de enlace de replicación. Recibe datos de replicación, los optimiza con almacenamiento en caché, compresión y cifrado, y los envía a Azure Storage.
- El servidor de procesos también instala Mobility Service en las máquinas virtuales que quiere replicar, además de realizar la detección automática de las máquinas virtuales de VMware locales.

Contoso realiza estos pasos como se indica a continuación:

1. Descarga la plantilla OVF desde **Preparar infraestructura** > **Origen** > **Servidor de configuración**.
    
    ![Descarga de OVF](./media/contoso-migration-rehost-linux-vm/add-cs.png)

2. Importa la plantilla en VMware para crear la VM e implementa la VM.

    ![Plantilla de OVF](./media/contoso-migration-rehost-linux-vm/vcenter-wizard.png)

3. Al activar la VM por primera vez, se inicia en una experiencia de instalación de Windows Server 2016. Acepta el contrato de licencia e introduce una contraseña de administrador.
4. Una vez finalizada la instalación, inicia sesión en la VM como administrador. Cuando se inicia sesión por primera vez, se ejecuta la herramienta de configuración de Azure Site Recovery de manera predeterminada.
5. En la herramienta, especifica un nombre que se usará para registrar el servidor de configuración en el almacén.
6. La herramienta comprueba que la máquina virtual pueda conectarse a Azure. Una vez establecida la conexión, inicia sesión en la suscripción a Azure. Las credenciales deben tener acceso al almacén donde desea registrar el servidor de configuración.

    ![Registrar servidor de configuración](./media/contoso-migration-rehost-linux-vm/config-server-register2.png)

7. La herramienta realiza algunas tareas de configuración y, a continuación, se reinicia.
8. Vuelve a iniciar sesión en la máquina y el Asistente para administración del servidor de configuración se inicia automáticamente.
9. En el asistente, selecciona la NIC para recibir tráfico de replicación. Una vez configurada, esta opción no se puede cambiar.
10. Selecciona la suscripción, el grupo de recursos y el almacén en el que se va a registrar el servidor de configuración.

    ![almacén](./media/contoso-migration-rehost-linux-vm/cswiz1.png) 

11. A continuación, descarga e instala MySQL Server y VMWare PowerCLI. 
12. Después de la validación, especifica la dirección IP o el FQDN del host de vSphere o vCenter Server. Deja el puerto predeterminado y especifica un nombre descriptivo para vCenter Server.
13. Especifica la cuenta que se ha creado para la detección automática, así como las credenciales que se deben usar para instalar automáticamente Mobility Service.

    ![vCenter](./media/contoso-migration-rehost-linux-vm/cswiz2.png)

14. Cuando finaliza el registro, en Azure Portal, Contoso verifica si el servidor de configuración y VMware Server aparecen en la página **Origen** del almacén. La detección puede tardar 15 minutos o más. 
15. A continuación, Site Recovery se conecta a los servidores de VMware y detecta VM.

### <a name="set-up-the-target"></a>Configuración del destino

Ahora, Contoso establece la configuración de la replicación de destino.

1. En **Preparar infraestructura** > **Destino**, selecciona la configuración de destino.
2. Site Recovery comprueba si existe una red y una cuenta de almacenamiento de Azure en el destino especificado.

### <a name="create-a-replication-policy"></a>Creación de una directiva de replicación

Después de configurar el origen y el destino, Contoso está listo para crear una directiva de replicación.

1. En **Preparar infraestructura** > **Configuración de la replicación** > **Directiva de replicación** >  **Crear y asociar**, crea una directiva denominada **ContosoMigrationPolicy**.
2. Usa la configuración predeterminada:
    - **Umbral de RPO**: valor predeterminado de 60 minutos. Este valor define la frecuencia de creación de puntos de recuperación. Se genera una alerta cuando la replicación continua supera este límite.
    - **Retención de punto de recuperación**. Valor predeterminado de 24 horas. Este valor especifica cuánto tiempo dura el período de retención para cada punto de recuperación. Las máquinas virtuales replicadas se pueden recuperar a cualquier momento de un período.
    - **Frecuencia de las instantáneas coherentes con la aplicación**. Valor predeterminado de una hora. Este valor especifica la frecuencia con la que se crean instantáneas coherentes con la aplicación.
 
        ![Creación de la directiva de replicación](./media/contoso-migration-rehost-linux-vm/replication-policy.png)

5. La directiva se asocia automáticamente al servidor de configuración. 

    ![Asociación de la directiva de replicación](./media/contoso-migration-rehost-linux-vm/replication-policy2.png)

**¿Necesita más ayuda?**

- Puede leer un tutorial completo de todos estos pasos en [Configurar la recuperación ante desastres para VM de VMware locales](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Puede encontrar instrucciones detalladas que le ayudarán a [configurar el entorno de origen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [implementar el servidor de configuración](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server) y [establecer la configuración de replicación](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- [Más información](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) sobre el agente invitado de Azure para Linux.

**¿Necesita más ayuda?**

- Puede leer un tutorial completo de todos estos pasos en [Configurar la recuperación ante desastres para VM de VMware locales](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial).
- Puede encontrar instrucciones detalladas que le ayudarán a [configurar el entorno de origen](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-source), [implementar el servidor de configuración](https://docs.microsoft.com/azure/site-recovery/vmware-azure-deploy-configuration-server) y [establecer la configuración de replicación](https://docs.microsoft.com/azure/site-recovery/vmware-azure-set-up-replication).
- [Más información](https://docs.microsoft.com/azure/virtual-machines/extensions/agent-linux) sobre el agente invitado de Azure para Linux.



### <a name="enable-replication-for-osticketweb"></a>Habilitar la replicación para OSTICKETWEB

Contoso ya puede empezar a replicar la VM **OSTICKETWEB**.

1. En **Replicar aplicación** > **Origen** > **+Replicar**, selecciona la configuración de origen.
2. Selecciona la opción para habilitar las máquinas virtuales y, a continuación, la configuración de origen, incluidos vCenter Server y el servidor de configuración.

    ![Habilitar replicación](./media/contoso-migration-rehost-linux-vm/enable-replication-source.png)

3. Especifica la configuración de destino, incluidos el grupo de recursos y una red virtual en la que la VM de Azure se ubicará después de la conmutación por error, así como una cuenta de almacenamiento en la que se almacenarán los datos replicados.

     ![Habilitar replicación](./media/contoso-migration-rehost-linux-vm/enable-replication2.png)

3. Contoso selecciona **OSTICKETWEB** para la replicación. 

    - En esta fase, Contoso solo selecciona **OSTICKETWEB**, dado que se deben seleccionar la red virtual y la subred, pero las VM no están en la misma subred.
    - Site Recovery instala Mobility Service automáticamente cuando se habilita la replicación para la VM.

    ![Habilitar replicación](./media/contoso-migration-rehost-linux-vm/enable-replication3.png)

4. En las propiedades de la VM, Contoso selecciona la cuenta que usará el servidor de procesos para instalar automáticamente Mobility Service en la máquina.

     ![Mobility Service](./media/contoso-migration-rehost-linux-vm/linux-mobility.png)

5. En **Configuración de la replicación** > **Establecer configuración de replicación**, comprueba que se haya aplicado la directiva de replicación correcta y selecciona **Habilitar replicación**.
6.  Realiza un seguimiento del progreso de la replicación en **Trabajos**. La máquina estará preparada para la conmutación por error después de que finalice el trabajo **Finalizar la protección**.



### <a name="enable-replication-for-osticketmysql"></a>Habilitar la replicación para OSTICKETMYSQL

Contoso ya puede empezar a replicar **OSTICKETMYSQL**.

1. En **Replicar aplicación** > **Origen** > **+Replicar**, se seleccionan las configuraciones de origen y de destino.
2. Contoso selecciona **OSTICKETMYSQL** para la replicación y, a continuación, selecciona la cuenta que se utilizará para la instalación del Mobility Service.

    ![Habilitar replicación](./media/contoso-migration-rehost-linux-vm/mysql-enable.png)

3. Contoso aplica la misma directiva de replicación que se usó para OSTICKETWEB y habilita la replicación.  

**¿Necesita más ayuda?**

Puede leer un tutorial completo de todos estos pasos en [Habilitar replicación](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-4-migrate-the-vms"></a>Paso 4: Migrar las VM 

Contoso ejecuta una conmutación por error rápida de prueba y, a continuación, migra las VM.

### <a name="run-a-test-failover"></a>Ejecución de una conmutación por error de prueba

La ejecución de una conmutación por error de prueba ayuda a garantizar que todo funciona según lo esperado antes de la migración. 

1. Contoso ejecuta una conmutación por error de prueba en el punto más reciente en el tiempo disponible (**procesado más recientemente**).
2. Selecciona **Apague la máquina antes de comenzar con la conmutación por error**, por lo que Site Recovery intenta apagar la VM de origen antes de desencadenar la conmutación por error. La conmutación por error continúa aunque se produzca un error de cierre. 
3. Se ejecuta la conmutación por error de prueba: 
    - Se ejecuta una comprobación de requisitos previos para garantizar que todas las condiciones necesarias para la conmutación por error están correctamente establecidas.
    - La conmutación por error procesa los datos, de modo que se pueda crear una máquina virtual de Azure. Si se selecciona el último punto de recuperación, se crea un punto de recuperación a partir de los datos.
    - Se crea una VM de Azure mediante los datos procesados en el paso anterior.
3. Una vez finalizada la conmutación por error, la VM de Azure de réplica aparece en Azure Portal. Contoso comprueba si la VM tiene el tamaño adecuado, si está conectada a la red correspondiente y si se está ejecutando. 
4. Después, limpia la conmutación por error y registra y guarda las observaciones.

### <a name="create-and-customize-a-recovery-plan"></a>Creación y personalización de un plan de recuperación

 Después de comprobar que la conmutación por error de prueba ha funcionado según lo esperado, Contoso crea un plan de recuperación para la migración. 

- Un plan de recuperación especifica el orden en que se produce la conmutación por error y cómo aparecerán las VM de Azure en Azure.
- Dado que se va a migrar una aplicación de dos niveles, se personalizará el plan de recuperación para que la VM de datos (SQLVM) se inicie antes del front-end (WEBVM).


1. En **Planes de recuperación (Site Recovery)** > **+Plan de recuperación**, crea un plan y se le agregan las VM.

    ![Plan de recuperación](./media/contoso-migration-rehost-linux-vm/recovery-plan.png)

2. Después de crear el plan, lo selecciona para personalizarlo (**Planes de recuperación** > **OsTicketMigrationPlan** > **Personalizar**.
3.  Quita **OSTICKETWEB** de **Grupo 1: inicio**.  Esto garantiza que la primera acción de inicio solo afecte a **OSTICKETMYSQL**.

    ![Grupo de recuperación](./media/contoso-migration-rehost-linux-vm/recovery-group1.png)

4.  En **+Grupo** > **Agregar elementos protegidos**, se agrega **OSTICKETWEB** a **Grupo 2: inicio**.  Contoso necesita estas opciones en dos grupos distintos.

    ![Grupo de recuperación](./media/contoso-migration-rehost-linux-vm/recovery-group2.png)


### <a name="migrate-the-vms"></a>Migrar las VM


Contoso está listo para ejecutar una conmutación por error en el plan de recuperación con el fin de migrar las VM.

1. Selecciona el plan > **Conmutación por error**.
2.  Selecciona la conmutación por error hasta el punto de recuperación más reciente y especifica que Site Recovery debe intentar apagar la VM local antes de desencadenar la conmutación por error. Puede seguir el progreso de la conmutación por error en la página **Trabajos**.

    ![Conmutación por error](./media/contoso-migration-rehost-vm/failover1.png)

3. Durante la conmutación por error, vCenter Server emite comandos para detener las dos VM que se ejecutan en el host ESXi.

    ![Conmutación por error](./media/contoso-migration-rehost-linux-vm/vcenter-failover.png)

3. Después de la conmutación por error, Contoso verifica si la VM de Azure aparece según lo esperado en Azure Portal.

    ![Conmutación por error](./media/contoso-migration-rehost-linux-vm/failover2.png)  

3. Después de verificar la VM en Azure, completa la migración para finalizar el proceso de migración de cada VM. Con esta acción se detienen la replicación de la VM y la facturación de Site Recovery para la VM.

    ![Conmutación por error](./media/contoso-migration-rehost-linux-vm/failover3.png)


### <a name="connect-the-vm-to-the-database"></a>Conexión de la VM a la base de datos

Como último paso del proceso de migración, Contoso actualiza la cadena de conexión de la aplicación para que apunte a la base de datos de la aplicación que se ejecuta en la VM **OSTICKETMYSQL**. 

1. Realiza una conexión SSH a la VM **OSTICKETWEB** mediante Putty u otro cliente SSH. La VM es privada, por lo que se conecta mediante la dirección IP privada.

    ![Conexión a la base de datos](./media/contoso-migration-rehost-linux-vm/db-connect.png)  

    ![Conexión a la base de datos](./media/contoso-migration-rehost-linux-vm/db-connect2.png)  

2. Contoso debe asegurarse de que la VM **OSTICKETWEB** puede comunicarse con la base de datos **OSTICKETMYSQL**. Actualmente, la configuración está codificada con la dirección IP local 172.16.0.43.

    **Antes de la actualización**
    
    ![Actualización de la IP](./media/contoso-migration-rehost-linux-vm/update-ip1.png)  

    **Después de la actualización**
    
    ![Actualización de la IP](./media/contoso-migration-rehost-linux-vm/update-ip2.png) 
    
3. Reinicia el servicio con **systemctl restart apache2**.

    ![Reinicio](./media/contoso-migration-rehost-linux-vm/restart.png) 

4. Por último, actualiza los registros DNS de **OSTICKETWEB** y **OSTICKETMYSQL** en uno de los controladores de dominio de Contoso.

    ![Actualización de DNS](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 

    ![Actualización de DNS](./media/contoso-migration-rehost-linux-vm-mysql/update-dns.png) 

**¿Necesita más ayuda?**

- [Más información](https://docs.microsoft.com/azure/site-recovery/tutorial-dr-drill-azure) sobre la ejecución de una conmutación por error de prueba. 
- [Más información](https://docs.microsoft.com/azure/site-recovery/site-recovery-create-recovery-plans) sobre cómo crear un plan de recuperación.
- [Más información](https://docs.microsoft.com/azure/site-recovery/site-recovery-failover) sobre cómo crear una conmutación por error en Azure.

## <a name="clean-up-after-migration"></a>Limpieza después de la migración

Al finalizar la migración, los niveles de la aplicación osTicket se ejecutan en VM de Azure.

Ahora, Contoso debe hacer una limpieza:  

- Quita las VM locales del inventario de vCenter.
- Quita las VM locales de los trabajos de copia de seguridad locales.
- Actualiza la documentación interna para mostrar la nueva ubicación, así como las direcciones IP para OSTICKETWEB y OSTICKETMYSQL.
- Revisa los recursos que interactúan con las VM y actualiza los valores de configuración pertinentes o la documentación para reflejar la nueva configuración.
- Contoso usaba el servicio de Azure Migrate con la asignación de dependencia para obtener acceso a las VM para la migración. Debe quitar de la VM los agentes Microsoft Monitoring Agent y Dependency Agent que se instalaron con este propósito.

## <a name="review-the-deployment"></a>Revisión de la implementación.

Con la aplicación en ejecución, Contoso debe proteger y poner totalmente en marcha la infraestructura nueva.

### <a name="security"></a>Seguridad

El equipo de seguridad de Contoso revisa las VM OSTICKETWEB y OSTICKETMYSQL para determinar cualquier problema de seguridad.

- Revisa los grupos de seguridad de red (NSG) de las VM con el fin de controlar el acceso. Los NSG se usan para garantizar que solo el tráfico permitido puede pasar a la aplicación.
- También considera la posibilidad de proteger los datos en los discos de la VM mediante el cifrado de discos y Azure Key Vault.

Obtenga [más información](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms#vm-authentication-and-access-control) sobre los procedimientos de seguridad recomendados de las VM.

### <a name="backups"></a>Copias de seguridad

Contoso realiza la copia de seguridad de los datos de las VM mediante el servicio de Azure Backup. [Más información](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

### <a name="licensing-and-cost-optimization"></a>Optimización de los costos y licencias

- Después de implementar los recursos, Contoso asigna etiquetas de Azure según lo definido durante la [implementación de la infraestructura de Azure](contoso-migration-infrastructure.md#set-up-tagging).
- Contoso no tiene problemas relacionados con las licencias con sus servidores de Ubuntu.
- Contoso habilitará Azure Cost Management bajo licencia de Cloudyn, una subsidiaria de Microsoft. Es una solución de administración de costos en varias nubes que le permitirá utilizar y administrar Azure y otros recursos en la nube.  [Más información](https://docs.microsoft.com/azure/cost-management/overview) sobre Azure Cost Management. 


## <a name="next-steps"></a>Pasos siguientes

En este artículo, hemos mostrado cómo Contoso ha migrado una aplicación de consola de servicio local que se divide en capas entre dos VM Linux a VM IaaS de Azure mediante Azure Site Recovery.

En el siguiente artículo de la serie, le mostraremos cómo Contoso migra la misma aplicación de consola de servicio a Azure. Esta vez, Contoso usa Site Recovery para migrar la VM de front-end de la aplicación y, a continuación, la base de datos de la aplicación mediante la copia de seguridad y la restauración en Azure Database for MySQL con la herramienta MySQL Workbench. [Introducción](contoso-migration-rehost-linux-vm-mysql.md).
