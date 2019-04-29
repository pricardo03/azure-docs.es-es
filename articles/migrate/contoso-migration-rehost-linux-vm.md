---
title: 'Rehospedaje: migración y rehospedaje de una aplicación de Linux local en VM de Azure | Microsoft Docs'
description: Obtenga información sobre cómo Contoso vuelve a hospedar una aplicación de Linux local mediante la migración de VM de Azure.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 10/10/2018
ms.author: raynew
ms.openlocfilehash: 200f9c5df0d4165341e38ca9d4dd85ad75c8403c
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/23/2019
ms.locfileid: "60675968"
---
# <a name="contoso-migration-rehost-an-on-premises-linux-app-to-azure-vms"></a>Migración de Contoso: Rehospedaje de una aplicación local Linux en Azure Virtual Machines

En este artículo se muestra cómo Contoso vuelve a hospedar una aplicación de consola de servicio basada en Linux local (**osTicket**) en las VM IaaS de Azure.

Este documento es el primero de una serie de artículos que documentan cómo la compañía ficticia Contoso migra sus recursos locales a la nube de Microsoft Azure. La serie incluye información general y un conjunto de escenarios que ilustran cómo configurar una infraestructura de migración y ejecutar los diferentes tipos de migraciones. La complejidad de los escenarios va en aumento. Se agregarán más artículos con el tiempo.

**Artículo** | **Detalles** | **Estado**
--- | --- | ---
[Artículo 1: Introducción](contoso-migration-overview.md) | Información general de la serie de artículos, la estrategia de migración de Contoso y las aplicaciones de ejemplo que se usan en esta serie. | Disponible
[Artículo 2: implementar una infraestructura de Azure](contoso-migration-infrastructure.md) | Contoso prepara la infraestructura local y la infraestructura de Azure para la migración. Se usa la misma infraestructura en todos los artículos de la serie sobre migración. | Disponible
[Artículo 3: valorar las cargas de trabajo locales para la migración a Azure](contoso-migration-assessment.md)  | Contoso ejecuta una valoración de su aplicación local SmartHotel360 que se ejecuta en VMware. Contoso evalúa las máquinas virtuales de la aplicación mediante el servicio Azure Migrate, y la base de datos SQL Server de la aplicación con Data Migration Assistant. | Disponible
[Artículo 4: rehospedaje de una aplicación en una máquina virtual de Azure e Instancia administrada de Azure SQL Database](contoso-migration-rehost-vm-sql-managed-instance.md) | Contoso ejecuta una migración mediante lift-and-shift a Azure para su aplicación SmartHotel360 local. Contoso migra la máquina virtual de front-end de la aplicación mediante [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview). Contoso migra la base de datos de la aplicación a una instancia administrada de Azure SQL Database mediante [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview). | Disponible   
[Artículo 5: Rehospedaje de una aplicación en máquinas virtuales de Azure](contoso-migration-rehost-vm.md) | Contoso migra sus máquinas virtuales de la aplicación SmartHotel360 a máquinas virtuales de Azure mediante el servicio Site Recovery. | Disponible
[Artículo 6: rehospedaje de una aplicación local en las VM de Azure y en el grupo de disponibilidad AlwayOn de SQL Server](contoso-migration-rehost-vm-sql-ag.md) | Contoso migra la aplicación SmartHotel360. Contoso usa Site Recovery para migrar las máquinas virtuales de la aplicación. Usa Database Migration Service para migrar la base de datos de la aplicación a un clúster de SQL Server que está protegido por un grupo de disponibilidad Always On. | Disponible 
Artículo 7: rehospedaje de una aplicación Linux en máquinas virtuales de Azure | Contoso realiza una migración mediante lift-and-shift de la aplicación osTicket de Linux a máquinas virtuales de Azure, mediante Azure Site Recovery | Este artículo
[Artículo 8: rehospedaje de una aplicación Linux en máquinas virtuales de Azure y en Azure MySQL](contoso-migration-rehost-linux-vm-mysql.md) | Contoso migra la aplicación osTicket de Linux a máquinas virtuales de Azure mediante Azure Site Recovery y la base de datos de la aplicación a una instancia de Azure MySQL Server con MySQL Workbench. | Disponible
[Artículo 9: refactorizar una aplicación de Azure Web Apps y Azure SQL Database](contoso-migration-refactor-web-app-sql.md) | Contoso migra la aplicación SmartHotel360 a una aplicación web de Azure y la base de datos de la aplicación a una instancia de Azure SQL Server con Database Migration Assistant | Disponible
[Artículo 10: refactorizar una aplicación Linux en Azure Web Apps y Azure MySQL](contoso-migration-refactor-linux-app-service-mysql.md) | Contoso migra su aplicación osTicket de Linux a una aplicación web de Azure en varias regiones de Azure con Azure Traffic Manager, integrado con GitHub para la entrega continua. Contoso migra la base de datos de la aplicación a una instancia de Azure Database for MySQL. | Disponible 
[Artículo 11: refactorización de TFS en Azure DevOps Services](contoso-migration-tfs-vsts.md) | Contoso migra su implementación local de Team Foundation Server a Azure DevOps Services en Azure. | Disponible
[Artículo 12: rediseño de una aplicación en Azure Containers y Azure SQL Database](contoso-migration-rearchitect-container-sql.md) | Contoso migra su aplicación SmartHotel a Azure. A continuación, rediseña el nivel de aplicación web como un contenedor de Windows que se ejecuta en Azure Service Fabric, y la base de datos con Azure SQL Database. | Disponible
[Artículo 13: Recompilación de una aplicación en Azure](contoso-migration-rebuild.md) | Contoso recompila su aplicación SmartHotel mediante una serie de funcionalidades y servicios de Azure, como Azure App Service, Azure Kubernetes Service (AKS), Azure Functions, Azure Cognitive Services y Azure Cosmos DB. | Disponible
[Artículo 14: Escalado de una migración en Azure](contoso-migration-scale.md) | Después de probar combinaciones de migración, Contoso se prepara para escalar una migración completa a Azure. | Disponible



En este artículo, Contoso migrará a Azure la aplicación **osTicket** de dos niveles, que se ejecuta en Linux Apache MySQL PHP (LAMP). Las VM de la aplicación se migrarán con el servicio de Azure Site Recovery. Si quiere usar esta aplicación de código abierto, puede descargarla desde [GitHub](https://github.com/osTicket/osTicket).

## <a name="business-drivers"></a>Impulsores del negocio

El equipo directivo de TI ha trabajado estrechamente con sus socios comerciales para comprender lo quieren lograr con esta migración:

- **Abordar el crecimiento del negocio**: Contoso está creciendo y, como resultado, sus sistemas locales e infraestructura están bajo presión.
- **Limitar el riesgo**: la aplicación de consola de servicio es fundamental para el negocio de Contoso. Contoso quiere moverla a Azure sin ningún riesgo.
- **Extensión**:  Contoso no quiere cambiar ahora mismo la aplicación. Solo quiere asegurarse de que la aplicación es estable.


## <a name="migration-goals"></a>Objetivos de la migración

El equipo de la nube de Contoso ha definido los objetivos de esta migración con el fin de determinar el mejor método para llevarla a cabo:

- Tras la migración, la aplicación de Azure debe tener las mismas funcionalidades de rendimiento que las que tiene actualmente en su entorno de VMWare local.  La aplicación seguirá siendo tan imprescindible en la nube como lo es en el entorno local. 
- Contoso no quiere hacer inversiones en esta aplicación.  Es importante para la empresa, pero, en su formato actual, Contoso solo quiere moverla a la nube de modo seguro.
- Contoso no quiere cambiar el modelo de operaciones de esta aplicación. Quiere interactuar con la aplicación en la nube de la misma manera que lo hace ahora.
- Contoso no quiere cambiar la funcionalidad de la aplicación. Solo cambiará su ubicación.
- Una vez completadas un par de migraciones de aplicaciones de Windows, Contoso quiere aprender a usar una infraestructura basada en Linux en Azure.


## <a name="solution-design"></a>Diseño de la solución

Después de fijar sus objetivos y requisitos, Contoso diseña y revisa una solución de implementación e identifica el proceso de migración, incluidos los servicios de Azure que Contoso usará para ello.

### <a name="current-app"></a>Aplicación actual

- La aplicación OSTicket se divide en capas entre dos máquinas virtuales (**OSTICKETWEB** y **OSTICKETMYSQL**).
- Las VM se encuentran en el host de VMware ESXi **contosohost1.contoso.com** (versión 6.5).
- El entorno de VMware lo administra vCenter Server 6.5 (**vcenter.contoso.com**), que se ejecuta en una VM.
- Contoso tiene un centro de datos local (**contoso-datacenter**), con un controlador de dominio local (**contosodc1**).

### <a name="proposed-architecture"></a>Arquitectura propuesta

- Dado que la aplicación es una carga de trabajo de producción, las máquinas virtuales de Azure residirán en el grupo de recursos de producción **ContosoRG**.
- Las VM migrarán a la región principal (Este de EE. UU. 2) y se colocarán en la red de producción (VNET-PROD-EUS2):
    - La VM web residirá en la subred de front-end (PROD-FE-EUS2).
    - La VM de la base de datos residirá en la subred de la base de datos (PROD-DB-EUS2).
- Las VM locales del centro de datos de Contoso se retirarán después de realizar la migración.

![Arquitectura del escenario](./media/contoso-migration-rehost-linux-vm/architecture.png) 

### <a name="solution-review"></a>Revisión de la solución

Contoso evalúa el diseño propuesto y crea una lista de ventajas y desventajas.

**Consideración** | **Detalles**
--- | ---
**Ventajas** | Las dos máquinas virtuales de la aplicación se moverán a Azure sin cambios, de forma que se simplifica la migración.<br/><br/> Dado que Contoso usa lift-and-shift con ambas máquinas virtuales de la aplicación, no se necesitan configuraciones ni herramientas de migración especiales para la base de datos de la aplicación.<br/><br/> Contoso conservará el control total de las máquinas virtuales de la aplicación en Azure. </br>/br > la aplicación de las máquinas virtuales que ejecutan Ubuntu 16.04-TLS, que es una distribución de Linux aprobada. [Más información](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros).
**Desventajas** | La capa de datos y la capa web de la aplicación seguirán siendo un único punto de conmutación por error. <br/><br/> Contoso deberá seguir admitiendo la aplicación en las máquinas virtuales de Azure en lugar de moverla a un servicio administrado como Azure App Service y Azure Database for MySQL.<br/><br/> Contoso sabe que al simplificar las cosas con una migración de máquinas virtuales mediante lift-and-shift, no están aprovechando al máximo las características proporcionadas por [Azure Database for MySQL](https://docs.microsoft.com/azure/mysql/overview) (alta disponibilidad integrada, rendimiento predecible, escalado sencillo, copias de seguridad automáticas y seguridad integrada).

### <a name="migration-process"></a>Proceso de migración

Contoso realizará la migración como se indica a continuación:

1. Como primer paso, Contoso configura la infraestructura local y de Azure necesaria para implementar Site Recovery.
2. Después de preparar los componentes locales y de Azure, Contoso configura y habilita la replicación de las máquinas virtuales.
3. Una vez que la replicación funciona, Contoso migra las máquinas virtuales mediante la conmutación por error a Azure.

![Proceso de migración](./media/contoso-migration-rehost-linux-vm/migration-process.png)

### <a name="azure-services"></a>Servicios de Azure

**Servicio** | **Descripción** | **Costee**
--- | --- | ---
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | El servicio orquesta y administra la migración y la recuperación ante desastres de las máquinas virtuales de Azure, así como las máquinas virtuales y servidores físicos locales.  | Durante la replicación en Azure, se incurre en gastos de Azure Storage.  Las máquinas virtuales de Azure se crean, e incurren en gastos, cuando se produce una conmutación por error. [Más información](https://azure.microsoft.com/pricing/details/site-recovery/) sobre cargos y precios.

 
## <a name="prerequisites"></a>Requisitos previos

Esto es lo que Contoso necesita para este escenario.

**Requisitos** | **Detalles**
--- | ---
**Suscripción de Azure** | En un artículo anterior de esta serie, Contoso creó suscripciones. Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/pricing/free-trial/).<br/><br/> Si crea una cuenta gratuita, será el administrador de su suscripción y podrá realizar todas las acciones.<br/><br/> Si usa una suscripción existente y no es el administrador, tendrá que solicitar al administrador que le asigne permisos de propietario o colaborador.<br/><br/> Si necesita permisos más específicos, consulte [este artículo](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Infraestructura de Azure** | Contoso configura la infraestructura de Azure según se describe en [Infraestructura de Azure para la migración](contoso-migration-infrastructure.md).<br/><br/> Obtenga más información sobre los requisitos específicos de la [red](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) y el [almacenamiento](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) para Site Recovery.
**Servidores locales** | La instancia local de vCenter Server debe ejecutarse en las versiones 5.5, 6.0 o 6.5.<br/><br/> Un host ESXi que ejecute la versión 5.5, 6.0 o 6.5<br/><br/> Una o más máquinas virtuales VMware que se ejecuten en el host ESXi.
**Máquinas virtuales locales** | [Revise las máquinas Linux](https://docs.microsoft.com//azure/site-recovery/vmware-physical-azure-support-matrix#replicated-machines) que se admiten para la migración con Site Recovery.<br/><br/> Verifique los [sistemas de almacenamiento y archivos de Linux](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#linux-file-systemsguest-storage) que se admiten.<br/><br/> Las máquinas virtuales tienen que cumplir los [requisitos de Azure](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).


## <a name="scenario-steps"></a>Pasos del escenario

Así es como Azure realizará la migración:

> [!div class="checklist"]
> * **Paso 1: Preparación de Azure Site Recovery**: Contoso crea una cuenta de Azure Storage que contiene los datos replicados, así como un almacén de Recovery Services.
> * **Paso 2: Preparación de VMware local para Site Recovery**: Contoso prepara las cuentas para la detección de máquinas virtuales y la instalación del agente, y se prepara para conectarse a las máquinas virtuales de Azure tras la conmutación por error.
> * **Paso 3: Replicación de máquinas virtuales**: Contoso configura el entorno de migración de origen y de destino, crea una directiva de replicación y empieza a replicar las máquinas virtuales en Azure Storage.
> * **Paso 4: Migración de las máquinas virtuales mediante Site Recovery**: Contoso ejecuta una conmutación por error de prueba para asegurarse de que todo funciona y, luego, ejecuta una conmutación por error completa para migrar las máquinas virtuales a Azure.


## <a name="step-1-prepare-azure-for-the-site-recovery-service"></a>Paso 1: Preparación de Azure para el servicio Site Recovery

Contoso necesita un par de componentes de Azure para Site Recovery:

- Una cuenta de Azure Storage para almacenar los datos replicados. 
- Un almacén de Recovery Services en Azure.
- Una red virtual para colocar los recursos tras la conmutación por error. Contoso ya ha creado la red virtual durante la [implementación de la infraestructura de Azure](contoso-migration-infrastructure.md), por lo que solo debe crear una cuenta de almacenamiento y el almacén.

1. Los administradores de Contoso crean una cuenta de Azure Storage (contosovmsacc20180528) en la región Este de EE. UU. 2.

    - La cuenta de almacenamiento debe estar en la misma región que el almacén de Recovery Services.
    - Se usa una cuenta de uso general, con almacenamiento estándar y replicación de LRS.

      ![Almacenamiento de Site Recovery](./media/contoso-migration-rehost-linux-vm/asr-storage.png)

2. Con la red y la cuenta de almacenamiento implementadas, crean un almacén (ContosoMigrationVault) y lo colocan en el grupo de recursos **ContosoFailoverRG**, en la región primaria Este de EE. UU. 2.

    ![Almacén de Recovery Services](./media/contoso-migration-rehost-linux-vm/asr-vault.png)


**¿Necesita más ayuda?**

[Más información](https://docs.microsoft.com/azure/site-recovery/tutorial-prepare-azure) sobre cómo configurar Azure para Site Recovery.


## <a name="step-2-prepare-on-premises-vmware-for-site-recovery"></a>Paso 2: Preparación de VMware local para Site Recovery

Los administradores de Contoso preparan la infraestructura de VMware local de la manera siguiente:

- Para automatizar la detección de máquinas virtuales crean una cuenta en el host de vCenter Server o vSphere ESXi.
- Crean una cuenta que permite la instalación automática de Mobility Service en las máquinas virtuales de VMware que quiere replicar.
- Preparan las máquinas virtuales locales para que puedan conectarse a las máquinas virtuales de Azure cuando se creen tras la migración.


### <a name="prepare-an-account-for-automatic-discovery"></a>Preparación de una cuenta de detección automática

Site Recovery necesita acceso a los servidores de VMware para:

- Detectar automáticamente las máquinas virtuales. Se requiere al menos una cuenta de solo lectura.
- Coordinar la replicación, la conmutación por error y la conmutación por recuperación. Necesita una cuenta que pueda ejecutar operaciones como la creación y eliminación de discos, así como la activación de máquinas virtuales.

Los administradores de Contoso configuran la cuenta de la manera siguiente:

1. Se crea un rol en el nivel de vCenter.
2. Se asignan a ese rol los permisos necesarios.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Preparación de una cuenta para la instalación de Mobility Service

Mobility Service debe estar instalado en todas las máquinas virtuales Linux que se migrarán.

- Site Recovery puede hacer una instalación de inserción automática de este componente cuando se habilite la replicación para las máquinas virtuales.
- Para la instalación de inserción automática, deben preparar una cuenta que Site Recovery usará para acceder a las máquinas virtuales.
- Los detalles de las cuentas se especifican durante la configuración de la replicación. 
- La cuenta puede ser una cuenta de dominio o local con permisos para hacer instalaciones en VM.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Preparación para la conexión a las máquinas virtuales de Azure después de la conmutación por error

Después de la conmutación por error en Azure, Contoso quiere poder conectarse a las VM replicadas de Azure. Para ello, hay un par de cosas que los administradores de Contoso deben hacer:

- Para acceder a las máquinas virtuales de Azure a través de Internet, habilitan SSH en la máquina virtual Linux local antes de la migración.  En el caso de Ubuntu, puede realizarse mediante el siguiente comando: **Sudo apt-get ssh install -y**.
- Después de ejecutar la migración (conmutación por error), pueden comprobar los **diagnósticos de arranque** para ver una captura de pantalla de la máquina virtual.
- Si esto no funciona, deberán comprobar que la máquina virtual está en ejecución y revisar estas [sugerencias de solución de problemas](https://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).


**¿Necesita más ayuda?**

- [Averigüe](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-automatic-discovery) cómo puede crear y asignar un rol para la detección automática.
- [Más información](https://docs.microsoft.com/azure/site-recovery/vmware-azure-tutorial-prepare-on-premises#prepare-an-account-for-mobility-service-installation) sobre cómo crear una cuenta para la instalación de inserción de Mobility Service.


## <a name="step-3-replicate-the-on-premises-vms"></a>Paso 3: Replicar máquinas virtuales locales

Para poder migrar la máquina virtual web a Azure, los administradores de Contoso configuran y habilitan la replicación.

### <a name="set-a-protection-goal"></a>Establecimiento de un objetivo de protección

1. En el nombre del almacén (ContosoVMVault), establece un objetivo de replicación (**Introducción** > **Site Recovery** > **Preparar infraestructura**.
2. Especifica que las máquinas se encuentran en el entorno local, que son VM de VMware y que quieren replicarse en Azure.
    ![Objetivo de replicación](./media/contoso-migration-rehost-linux-vm/replication-goal.png)

### <a name="confirm-deployment-planning"></a>Confirmación del planeamiento de la implementación

Para continuar, confirma que se ha completado la planificación de implementación mediante la selección de la opción **Sí, ya lo hice**. Contoso solo va a migrar una única VM en este escenario, por lo que no es necesario planear la implementación.

### <a name="set-up-the-source-environment"></a>Configuración del entorno de origen

Los administradores de Contoso ahora deben configurar el entorno de origen. Para ello, descarga una plantilla OVF y la usa para implementar el servidor de configuración de Site Recovery como una VM local de VMware altamente disponible. Una vez que el servidor de configuración se encuentra en funcionamiento, lo registra en el almacén.

El servidor de configuración ejecuta una serie de componentes:

- El componente del servidor de configuración que coordina las comunicaciones entre el entorno local y Azure y administra la replicación de datos.
- El servidor de procesos que actúa como puerta de enlace de replicación. Recibe datos de replicación, los optimiza con almacenamiento en caché, compresión y cifrado, y los envía a Azure Storage.
- El servidor de procesos también instala Mobility Service en las máquinas virtuales que quiere replicar, además de realizar la detección automática de las máquinas virtuales de VMware locales.

Los administradores de Contoso realizan estos pasos como se indica a continuación:

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

14. Cuando finaliza el registro, en Azure Portal, comprueban que el servidor de configuración y el servidor VMware aparecen en la página **Origen** del almacén. La detección puede tardar 15 minutos o más. 
15. A continuación, Site Recovery se conecta a los servidores de VMware y detecta VM.

### <a name="set-up-the-target"></a>Configuración del destino

Ahora, los administradores de Contoso configuran la replicación de destino.

1. En **Preparar infraestructura** > **Destino**, selecciona la configuración de destino.
2. Site Recovery comprueba si existe una red y una cuenta de almacenamiento de Azure en el destino especificado.

### <a name="create-a-replication-policy"></a>Creación de una directiva de replicación

Después de configurar el origen y el destino, están preparados para crear una directiva de replicación.

1. En **Preparar infraestructura** > **Configuración de la replicación** > **Directiva de replicación** >  **Crear y asociar**, crea una directiva denominada **ContosoMigrationPolicy**.
2. Usa la configuración predeterminada:
    - **Umbral de RPO**: el valor predeterminado es de 60 minutos. Este valor define la frecuencia de creación de puntos de recuperación. Se genera una alerta cuando la replicación continua supera este límite.
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

Los administradores de Contoso ya pueden empezar a replicar la máquina virtual **OSTICKETWEB**.

1. En **Replicar aplicación** > **Origen** > **+Replicar**, selecciona la configuración de origen.
2. Selecciona la opción para habilitar las máquinas virtuales y, a continuación, la configuración de origen, incluidos vCenter Server y el servidor de configuración.

    ![Habilitar replicación](./media/contoso-migration-rehost-linux-vm/enable-replication-source.png)

3. Especifica la configuración de destino, incluidos el grupo de recursos y una red virtual en la que la VM de Azure se ubicará después de la conmutación por error, así como una cuenta de almacenamiento en la que se almacenarán los datos replicados.

     ![Habilitar replicación](./media/contoso-migration-rehost-linux-vm/enable-replication2.png)

3. Seleccionan la máquina virtual **OSTICKETWEB** para la replicación. 

   - En esta fase, solo seleccionan **OSTICKETWEB**, dado que se deben seleccionar la red virtual y la subred, y las máquinas virtuales no están en la misma subred.
   - Site Recovery instala Mobility Service automáticamente cuando se habilita la replicación para la VM.

     ![Habilitar replicación](./media/contoso-migration-rehost-linux-vm/enable-replication3.png)

4. En las propiedades de la máquina virtual, seleccionan la cuenta que usará el servidor de procesos para instalar automáticamente Mobility Service en la máquina.

     ![Mobility Service](./media/contoso-migration-rehost-linux-vm/linux-mobility.png)

5. En **Configuración de la replicación** > **Establecer configuración de replicación**, comprueba que se haya aplicado la directiva de replicación correcta y selecciona **Habilitar replicación**.
6. Realiza un seguimiento del progreso de la replicación en **Trabajos**. La máquina estará preparada para la conmutación por error después de que finalice el trabajo **Finalizar la protección**.



### <a name="enable-replication-for-osticketmysql"></a>Habilitar la replicación para OSTICKETMYSQL

Ahora, los administradores de Contoso pueden empezar a replicar **OSTICKETMYSQL**.

1. En **Replicar aplicación** > **Origen** > **+Replicar**, se seleccionan las configuraciones de origen y de destino.
2. Seleccionan la máquina virtual **OSTICKETMYSQL** para la replicación y, a continuación, seleccionan la cuenta que se usará para la instalación de Mobility Service.

    ![Habilitar replicación](./media/contoso-migration-rehost-linux-vm/mysql-enable.png)

3. Aplican la misma directiva de replicación que se usó para OSTICKETWEB y habilitan la replicación.  

**¿Necesita más ayuda?**

Puede leer un tutorial completo de todos estos pasos en [Habilitar replicación](https://docs.microsoft.com/azure/site-recovery/vmware-azure-enable-replication).


## <a name="step-4-migrate-the-vms"></a>Paso 4: Migrar las VM 

Los administradores de Contoso ejecutan una conmutación por error rápida de prueba y, a continuación, migran las máquinas virtuales.

### <a name="run-a-test-failover"></a>Ejecución de una conmutación por error de prueba

La ejecución de una conmutación por error de prueba ayuda a garantizar que todo funciona según lo esperado antes de la migración. 

1. Ejecutan una conmutación por error de prueba en el punto más reciente en el tiempo disponible (**Procesado más recientemente**).
2. Selecciona **Apague la máquina antes de comenzar con la conmutación por error**, por lo que Site Recovery intenta apagar la VM de origen antes de desencadenar la conmutación por error. La conmutación por error continúa aunque se produzca un error de cierre. 
3. Se ejecuta la conmutación por error de prueba: 
    - Se ejecuta una comprobación de requisitos previos para garantizar que todas las condiciones necesarias para la conmutación por error están correctamente establecidas.
    - La conmutación por error procesa los datos, de modo que se pueda crear una máquina virtual de Azure. Si se selecciona el último punto de recuperación, se crea un punto de recuperación a partir de los datos.
    - Se crea una VM de Azure mediante los datos procesados en el paso anterior.
3. Una vez finalizada la conmutación por error, la VM de Azure de réplica aparece en Azure Portal. Contoso comprueba si la VM tiene el tamaño adecuado, si está conectada a la red correspondiente y si se está ejecutando. 
4. Después, limpia la conmutación por error y registra y guarda las observaciones.

### <a name="create-and-customize-a-recovery-plan"></a>Crear y personalizar un plan de recuperación

 Después de comprobar que la conmutación por error de prueba ha funcionado según lo esperado, los administradores de Contoso crean un plan de recuperación para la migración. 

- Un plan de recuperación especifica el orden en que se produce la conmutación por error y cómo aparecerán las VM de Azure en Azure.
- Dado que se va a migrar una aplicación de dos niveles, se personalizará el plan de recuperación para que la VM de datos (SQLVM) se inicie antes del front-end (WEBVM).


1. En **Planes de recuperación (Site Recovery)** > **+Plan de recuperación**, crea un plan y se le agregan las VM.

    ![Plan de recuperación](./media/contoso-migration-rehost-linux-vm/recovery-plan.png)

2. Después de crear el plan, lo selecciona para personalizarlo (**Planes de recuperación** > **OsTicketMigrationPlan** > **Personalizar**.
3.  Se quita **OSTICKETWEB** de **Grupo 1: Iniciar**.  Esto garantiza que la primera acción de inicio solo afecte a **OSTICKETMYSQL**.

    ![Grupo de recuperación](./media/contoso-migration-rehost-linux-vm/recovery-group1.png)

4.  En **+Grupo** > **Agregar elementos protegidos**, se agrega **OSTICKETWEB** a **Grupo 2: Iniciar**.  Los administradores necesitan tener estas máquinas virtuales en dos grupos diferentes.

    ![Grupo de recuperación](./media/contoso-migration-rehost-linux-vm/recovery-group2.png)


### <a name="migrate-the-vms"></a>Migrar las VM


Los administradores de Contoso están listos para ejecutar una conmutación por error en el plan de recuperación con el fin de migrar las máquinas virtuales.

1. Selecciona el plan > **Conmutación por error**.
2.  Selecciona la conmutación por error hasta el punto de recuperación más reciente y especifica que Site Recovery debe intentar apagar la VM local antes de desencadenar la conmutación por error. Puede seguir el progreso de la conmutación por error en la página **Trabajos**.

    ![Conmutación por error](./media/contoso-migration-rehost-vm/failover1.png)

3. Durante la conmutación por error, vCenter Server emite comandos para detener las dos VM que se ejecutan en el host ESXi.

    ![Conmutación por error](./media/contoso-migration-rehost-linux-vm/vcenter-failover.png)

3. Después de la conmutación por error, Contoso comprueba si la VM de Azure aparece según lo esperado en Azure Portal.

    ![Conmutación por error](./media/contoso-migration-rehost-linux-vm/failover2.png)  

3. Después de verificar la VM en Azure, completa la migración para finalizar el proceso de migración de cada VM. Con esta acción se detienen la replicación de la VM y la facturación de Site Recovery para la VM.

    ![Conmutación por error](./media/contoso-migration-rehost-linux-vm/failover3.png)


### <a name="connect-the-vm-to-the-database"></a>Conexión de la VM a la base de datos

Como último paso del proceso de migración, los administradores de Contoso actualización la cadena de conexión de la aplicación para que apunte a la base de datos de aplicación que se ejecuta en el **OSTICKETMYSQL** máquina virtual. 

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

Ahora, Contoso debe realizar la siguiente limpieza: 

- Quitar las VM locales del inventario de vCenter.
- Quitar las VM locales de los trabajos de copia de seguridad locales.
- Actualizar la documentación interna para mostrar la nueva ubicación, así como las direcciones IP de OSTICKETWEB y OSTICKETMYSQL.
- Revisar los recursos que interactúan con las VM y actualizar las opciones de configuración pertinentes o la documentación para reflejar la nueva configuración.
- Contoso usaba el servicio de Azure Migrate con la asignación de dependencia para obtener acceso a las VM para la migración. Los administradores deben quitar de la máquina virtual los componentes Microsoft Monitoring Agent y Dependency Agent que se instalaron con este fin.

## <a name="review-the-deployment"></a>Revisión de la implementación

Con la aplicación en ejecución, Contoso debe proteger y poner totalmente en marcha la infraestructura nueva.

### <a name="security"></a>Seguridad

El equipo de seguridad de Contoso revisa las VM OSTICKETWEB y OSTICKETMYSQL para determinar cualquier problema de seguridad.

- El equipo revisa los grupos de seguridad de red (NSG) de las máquinas virtuales para controlar el acceso. Los NSG se usan para garantizar que solo el tráfico permitido puede pasar a la aplicación.
- El equipo también considera la posibilidad de proteger los datos de los discos de la máquina virtual mediante el cifrado de discos y Azure Key Vault.

Obtenga [más información](https://docs.microsoft.com/azure/security/azure-security-best-practices-vms) sobre los procedimientos de seguridad recomendados de las VM.

### <a name="bcdr"></a>BCDR

Para la continuidad empresarial y la recuperación ante desastres, Contoso realiza las siguientes acciones:

- **Mantener seguros los datos**: Contoso realiza la copia de seguridad de los datos de las VM mediante el servicio Azure Backup. [Más información](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
- **Mantener las aplicaciones en funcionamiento**: Contoso replica las máquinas virtuales de la aplicación de Azure en una región secundaria mediante Site Recovery. [Más información](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-quickstart).

### <a name="licensing-and-cost-optimization"></a>Optimización de los costos y licencias

- Después de implementar los recursos, Contoso asigna etiquetas de Azure según lo definido durante la [implementación de la infraestructura de Azure](contoso-migration-infrastructure.md#set-up-tagging).
- Contoso no tiene problemas relacionados con las licencias con sus servidores de Ubuntu.
- Contoso habilitará Azure Cost Management bajo licencia de Cloudyn, una subsidiaria de Microsoft. Es una solución de administración de costos en varias nubes que le permitirá utilizar y administrar Azure y otros recursos en la nube.  [Más información](https://docs.microsoft.com/azure/cost-management/overview) sobre Azure Cost Management. 


## <a name="next-steps"></a>Pasos siguientes

En este artículo, hemos mostrado cómo Contoso ha migrado una aplicación de consola de servicio local que se divide en capas entre dos VM Linux a VM IaaS de Azure mediante Azure Site Recovery.

En el siguiente artículo de la serie, le mostraremos cómo Contoso migra la misma aplicación de consola de servicio a Azure. Esta vez, Contoso usa Site Recovery para migrar la máquina virtual front-end de la aplicación y, a continuación, la base de datos de la aplicación mediante la funcionalidad de copia de seguridad y restauración a Azure Database for MySQL con la herramienta MySQL Workbench. [Introducción](contoso-migration-rehost-linux-vm-mysql.md).
