---
title: Configurar la recuperación ante desastres para máquinas virtuales de VMware locales en Azure con Azure Site Recovery | Microsoft Docs
description: Aprenda a configurar la recuperación ante desastres para máquinas virtuales de VMware locales en Azure con Azure Site Recovery.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: tutorial
ms.date: 07/06/2018
ms.author: raynew
ms.custom: MVC
ms.openlocfilehash: 93626e6a8f199841b285fb8a6e302e6c3054db0d
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/09/2018
ms.locfileid: "37918040"
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-vmware-vms"></a>Configurar la recuperación ante desastres para máquinas virtuales de VMware locales en Azure

[Azure Site Recovery](site-recovery-overview.md) contribuye a la estrategia de recuperación ante desastres y continuidad empresarial (BCDR) al mantener sus aplicaciones empresariales al día y disponibles durante interrupciones planeadas y no planeadas. Azure Site Recovery administra y coordina la recuperación ante desastres de máquinas locales y máquinas virtuales de Azure, lo que incluye la replicación, la conmutación por error y la recuperación.


En este tutorial, le mostramos cómo configurar y habilitar la replicación de una máquina virtual de VMware en Azure, mediante Azure Site Recovery. Los tutoriales están diseñados para mostrarle cómo implementar Site Recovery con la configuración básica. Utilizan la ruta de acceso más simple y no muestran todas las opciones. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Especificar el origen y destino de la replicación.
> * Configurar el entorno de replicación de origen, incluidos los componentes locales de Azure Site Recovery y el entorno de replicación de destino.
> * Crear una directiva de replicación.
> * Habilitar la replicación para una máquina virtual.

## <a name="before-you-start"></a>Antes de comenzar

Antes de empezar, es útil:

- [Revisar la arquitectura](vmware-azure-architecture.md) de este escenario de recuperación ante desastres.
- Si desea obtener más información sobre cómo configurar la recuperación ante desastres para las máquinas virtuales de VMware con más detalle, revise y utilice los recursos siguientes:
    - [Lea las preguntas frecuentes](vmware-azure-common-questions.md) acerca de la recuperación ante desastres para VMware.
    - [Aprenda](vmware-physical-azure-support-matrix.md) qué se admite y se necesita para VMware.
-  Lea nuestras **guías de procedimiento** para obtener instrucciones detalladas que cubren todas las opciones de implementación de VMware:
    - Configurar el [origen de la replicación](vmware-azure-set-up-source.md) y el [servidor de configuración](vmware-azure-deploy-configuration-server.md).
    - Configurar el [destino de la replicación](vmware-azure-set-up-target.md).
    - Configuración de una [directiva de replicación](vmware-azure-set-up-replication.md) y [habilitación de la replicación](vmware-azure-enable-replication.md)


## <a name="select-a-protection-goal"></a>Selección de un objetivo de protección

1. En **Almacenes de Recovery Services**, seleccione el nombre del almacén. En este escenario se usa **ContosoVMVault**.
2. En **Introducción**, seleccione Site Recovery. Luego, seleccione **Preparar infraestructura**.
3. En **Objetivo de protección** > **¿Dónde están ubicadas las máquinas?**, seleccione **Local**.
4. En **¿A dónde quiere replicar las máquinas?**, seleccione **En Azure**.
5. En **¿Las máquinas están virtualizadas?**, seleccione **Yes, with VMware vSphere Hypervisor** (Sí, con VMware vSphere Hypervisor). Después seleccione **Aceptar**.


## <a name="plan-your-deployment"></a>Planeamiento de la implementación

En este tutorial le mostramos cómo replicar una única máquina virtual y en **Planeamiento de implementación**, seleccionaremos **Sí, ya lo hice**. Si va a implementar varias máquinas virtuales, le recomendamos que no omita este paso. Proporcionamos para ayudarle la [herramienta Deployment Planner](https://aka.ms/asr-deployment-planner). [Más información](site-recovery-deployment-planner.md) sobre esta herramienta.

## <a name="set-up-the-source-environment"></a>Configuración del entorno de origen

Como primer paso de la implementación, va a configurar el entorno de origen. Es necesaria una máquina local, única y de alta disponibilidad para hospedar los componentes de Site Recovery locales. Los componentes son el servidor de configuración, el servidor de procesos y el servidor de destino maestro:

- El servidor de configuración coordina la comunicación entre el entorno local y Azure, además de administrar la replicación de datos.
- El servidor de procesos actúa como puerta de enlace de replicación. Recibe datos de replicación, los optimiza con almacenamiento en caché, compresión y cifrado, y los envía a Azure Storage. El servidor de procesos también instala Mobility Service en las máquinas virtuales que quiere replicar, además de realizar la detección automática de las máquinas virtuales de VMware locales.
- El servidor de destino maestro controla los datos de replicación durante la conmutación por recuperación desde Azure.

Para configurar el servidor de configuración como máquina virtual de VMware de alta disponibilidad, descargue una plantilla de Open Virtualization Application (OVA) preparada e impórtela a VMware para crear la máquina virtual. Después de configurar el servidor de configuración, regístrelo en el almacén. Tras el registro, Site Recovery detecta las máquinas virtuales locales de VMware.

> [!TIP]
> En este tutorial se usa una plantilla de OVA para crear la máquina virtual de VMware del servidor de configuración. Si no puede hacerlo, puede [configurar el servidor de configuración manualmente](physical-manage-configuration-server.md).

> [!TIP]
> En este tutorial, Site Recovery descarga e instala MySQL en el servidor de configuración. Si no desea que Site Recovery lo haga, puede configurarlo manualmente. [Más información](vmware-azure-deploy-configuration-server.md#configure-settings).


### <a name="download-the-vm-template"></a>Descarga de la plantilla de máquina virtual

1. En el almacén, vaya a **Preparar infraestructura** > **Origen**.
2. En **Preparar origen**, seleccione **+Servidor de configuración**.
3. En **Agregar servidor**, compruebe que aparezca **Servidor de configuración para VMware** en **Tipo de servidor**.
4. Descargue la plantilla OVF para el servidor de configuración.

 > [!TIP]
 >Puede descargar la versión más reciente de la plantilla del servidor de configuración directamente desde el [Centro de descarga de Microsoft](https://aka.ms/asrconfigurationserver).

>[!NOTE]
La licencia proporcionada con la plantilla de OVF es una licencia de evaluación válida durante 180 días. El cliente debe activar Windows con una licencia adquirida.

## <a name="import-the-template-in-vmware"></a>Importación de la plantilla en VMware

1. Inicie sesión en el servidor VMware vCenter o el host vSphere ESXi con el cliente de VMWare vSphere.
2. En el menú **File** (Archivo), seleccione **Deploy OVF Template** (Implementar plantilla OVF) para iniciar el **Asistente para implementar plantillas OVF**. 

     ![Plantilla de OVF](./media/vmware-azure-tutorial/vcenter-wizard.png)

3. En **Select source** (Seleccionar origen), especifique la ubicación de la plantilla OVF descargada.
4. En **Review details** (Revisar detalles), seleccione **Next** (Siguiente).
5. En **Select name and folder** (Seleccionar el nombre y la carpeta) y **Select configuration** (Seleccionar la configuración), acepte la configuración predeterminada.
6. En **Select storage** (Seleccionar almacenamiento), seleccione **Thick Provision Eager Zeroed** (Aprovisionamiento grueso diligente con ceros) en **Select virtual disk format** (Seleccionar formato de disco virtual) para obtener un rendimiento mejor.
7. En el resto de las páginas del asistente, acepte la configuración predeterminada.
8. En **Ready to Complete** (Listo para completarse), para configurar la máquina virtual con la configuración predeterminada, seleccione **Power on after deployment** > **Finish** (Encender después de la implementación > Finalizar).

    > [!TIP]
  Si desea agregar una NIC adicional, desactive **Power on after deployment** > **Finish** (Encender después de la implementación > Finalizar). De forma predeterminada, la plantilla contiene una sola NIC. Puede agregar NIC adicionales después de la implementación.

## <a name="add-an-additional-adapter"></a>Incorporación de un adaptador adicional

Si desea agregar una NIC adicional al servidor de configuración, hágalo antes de registrar el servidor en el almacén. No se admiten adaptadores adicionales después del registro.

1. En el inventario de cliente de vSphere, haga clic con el botón derecho en la máquina virtual y seleccione **Editar configuración**.
2. En **Hardware**, seleccione **Agregar** > **Adaptador Ethernet**. Luego, seleccione **Siguiente**.
3. Seleccione el tipo de adaptador y la red. 
4. Para conectar la NIC virtual al encender la máquina virtual, seleccione **Connect at power on** (Conectar al encender). Seleccione **Siguiente** > **Finalizar**. Después seleccione **Aceptar**.


## <a name="register-the-configuration-server"></a>Registro del servidor de configuración 

1. Desde la consola de cliente de VMWare vSphere, encienda la máquina virtual.
2. La máquina virtual se inicia en una experiencia de instalación de Windows Server 2016. Acepte el contrato de licencia y especifique una contraseña de administrador.
3. Una vez finalizada la instalación, inicie sesión en la máquina virtual como administrador.
4. La primera vez que inicie sesión, se inicia la herramienta de configuración de Azure Site Recovery en unos segundos.
5. Especifique el nombre que se usará para registrar el servidor de configuración en Site Recovery. Luego, seleccione **Siguiente**.
6. La herramienta comprueba que la máquina virtual pueda conectarse a Azure. Una vez establecida la conexión, haga clic en **Iniciar sesión** para iniciar sesión en la suscripción de Azure. Las credenciales deben tener acceso al almacén donde desea registrar el servidor de configuración.
7. La herramienta realiza algunas tareas de configuración y, a continuación, se reinicia.
8. Inicie sesión de nuevo en la máquina. En pocos segundos, el Asistente para administración del servidor de configuración se inicia automáticamente.

### <a name="configure-settings-and-add-the-vmware-server"></a>Configuración de los valores y adición del servidor de VMware

1. En el asistente para la administración del servidor de configuración, seleccione **Configurar conectividad** y, a continuación, seleccione la NIC que el servidor de procesos usará para recibir el tráfico de replicación de las VM. Después, seleccione **Guardar**. Una vez configurada, esta opción no se puede cambiar.
2. En **Seleccionar almacén de Recovery Services**, seleccione la suscripción de Azure y el grupo de recursos y almacén correspondientes.
3. En **Instalar software de terceros**, acepte el contrato de licencia. Seleccione **Descargar e instalar** para instalar MySQL Server. Si ha colocado MySQL en la ruta de acceso, este paso se omitirá.
4. Seleccione **Install VMware PowerCLI** (Instalar VMware PowerCLI). Asegúrese de que todas las ventanas del explorador están cerradas antes. Después, seleccione **Continuar**.
5. En **Validate appliance configuration** (Comprobar configuración del dispositivo), se comprueban los requisitos previos antes de continuar.
6. En **Configure vCenter Server/vSphere ESXi server** (Configurar vCenter Server/vSphere ESXi Server), especifique la dirección IP o FQDN del servidor vCenter o host de vSphere en los que se encuentran las máquinas virtuales donde desea realizar la replicación. Especifique el puerto en el que escucha el servidor. Escriba un nombre descriptivo que se usará para el servidor de VMware en el almacén.
7. Especifique las credenciales que usará el servidor de configuración para conectarse al servidor de VMware. Site Recovery usa estas credenciales para detectar automáticamente las máquinas virtuales de VMware disponibles para la replicación. Seleccione **Agregar** y, luego, **Continuar**.
8. En **Configure virtual machine credentials** (Configurar las credenciales de la máquina virtual), especifique el nombre de usuario y la contraseña que se usarán para instalar automáticamente Mobility Service en las máquinas, cuando la replicación esté habilitada.
    - Para las máquinas Windows, la cuenta necesita privilegios de administrador local en las máquinas que se vayan a replicar.
    - Para Linux, proporcione los detalles de la cuenta raíz.
9. Seleccione **Finalize configuration** (Terminar configuración) para completar el registro.
10. Cuando finalice el registro, en Azure Portal, verifique que el servidor de configuración y el servidor de VMware aparecen en la página **Origen** del almacén. Haga clic en **Aceptar** para configurar el destino.


Site Recovery se conecta a los servidores de VMware con la configuración especificada y detecta las máquinas virtuales.

> [!NOTE]
> El nombre de la cuenta podría tardar 15 minutos o más en aparecer en el portal. Para que se actualice inmediatamente, seleccione **Servidores de configuración** > ***nombre de servidor*** > **Actualizar servidor**.

## <a name="set-up-the-target-environment"></a>Configuración del entorno de destino

Seleccione y compruebe los recursos de destino.

1. Seleccione **Preparar infraestructura** > **Destino**. Seleccione la suscripción de Azure que quiere usar. Estamos usando un modelo de Resource Manager.
2. Site Recovery comprueba que tiene una o más redes y cuentas de Azure Storage compatibles. Debería tenerlos cuando configure los componentes de Azure en el [primer tutorial](tutorial-prepare-azure.md) de esta serie de tutoriales.

   ![Pestaña Destino](./media/vmware-azure-tutorial/storage-network.png)

## <a name="create-a-replication-policy"></a>Creación de una directiva de replicación

1. Abra [Azure Portal](https://portal.azure.com) y seleccione **Todos los recursos**.
2. Seleccione el almacén de Recovery Services (**ContosoVMVault** en este tutorial).
3. Para crear una directiva de replicación, seleccione **Site Recovery Infrastructure (Infraestructura de Site Recovery)** > **Directivas de replicación** > **+Directiva de replicación**.
4. En **Crear la directiva de replicación**, especifique el nombre de directiva. Usamos **VMwareRepPolicy**.
5. En **Umbral de RPO**, use el valor predeterminado de 60 minutos. Este valor define la frecuencia de creación de puntos de recuperación. Se genera una alerta cuando la replicación continua supera este límite.
6. En **Retención de punto de recuperación**, especifique cuánto tiempo se conserva cada punto de recuperación. En este tutorial se selecciona 72 horas. Las máquinas virtuales replicadas se pueden recuperar a cualquier momento de una ventana de retención.
7. En **Frecuencia de instantánea coherente con la aplicación**, especifique la frecuencia (en minutos) con la que se crearán puntos de recuperación que contengan las instantáneas coherentes con la aplicación. Usamos el valor predeterminado de 60 minutos. Seleccione **Aceptar** para crear la directiva.

   ![Creación de la directiva de replicación](./media/vmware-azure-tutorial/replication-policy.png)

- La directiva se asocia automáticamente al servidor de configuración.
- De forma predeterminada, se crea automáticamente una directiva correspondiente para la conmutación por recuperación. Por ejemplo, si la directiva de replicación es **rep-policy**, la directiva de conmutación por recuperación será **rep-policy-failback**. Esta directiva no se usa hasta que se inicie una conmutación por recuperación desde Azure.

## <a name="enable-replication"></a>Habilitar replicación

La habilitación de la replicación puede realizarse como sigue:

1. Seleccione **Replicar la aplicación** > **Origen**.
2. En **Origen**, seleccione **Local** y seleccione el servidor de configuración en **Ubicación de origen**.
3. En **Tipo de máquina**, seleccione **Máquinas virtuales**.
4. En **vCenter/vSphere Hypervisor**, seleccione el host de vSphere o servidor vCenter que administra el host.
5. Seleccione el servidor de proceso (que se instala de forma predeterminada en la máquina virtual del servidor de configuración). Después seleccione **Aceptar**.
6. En **Destino**, seleccione la suscripción y el grupo de recursos donde desee crear las máquinas virtuales conmutadas por error. Usamos el modelo de implementación de Resource Manager. 
7. Seleccione cuenta de almacenamiento que desea usar para replicar datos y la red y subred de Azure a la que se conectarán las máquinas virtuales de Azure cuando se creen después de la conmutación por error.
8. Seleccione **Configurar ahora para las máquinas seleccionadas** para aplicar la configuración de red a todas las máquinas habilitadas para replicación. Seleccione **Configurar más tarde** para seleccionar la red de Azure por máquina.
9. En **Máquinas virtuales** > **Seleccionar máquinas virtuales**, seleccione cada máquina que desea replicar. Solo puede seleccionar aquellas máquinas en las que se pueda habilitar la replicación. Después seleccione **Aceptar**.
10. En **Propiedades** > **Configurar propiedades**, seleccione la cuenta que usará el servidor de procesos para instalar automáticamente Mobility Service en la máquina.
11. En **Configuración de la replicación** > **Establecer configuración de replicación**, compruebe que se haya seleccionado la directiva de replicación correcta.
12. Seleccione **Habilitar replicación**. Site Recovery instala Mobility Service cuando se habilita la replicación para una máquina virtual.
13. Puede hacer un seguimiento del progreso del trabajo **Habilitar protección** en **Configuración** > **Trabajos** > **Trabajos de Site Recovery**. La máquina estará preparada para la conmutación por error después de que finalice el trabajo **Finalizar la protección**.
- Los cambios pueden tardar 15 minutos o más en aplicarse y aparecer en el portal.
- Para supervisar las máquinas virtuales que agregue, compruebe la última hora de detección de máquinas virtuales en **Servidores de configuración** > **Último contacto a las**. Para agregar máquinas virtuales sin esperar a la detección programada, resalte el servidor de configuración (no haga clic en él) y haga clic en **Actualizar**.

## <a name="next-steps"></a>Pasos siguientes

> [!div class="nextstepaction"]
> [Exploración de la recuperación ante desastres](site-recovery-test-failover-to-azure.md)
