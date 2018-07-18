---
title: Implementación del servidor de configuración para realizar la recuperación ante desastres de VMware con Azure Site Recovery | Microsoft Docs
description: En este artículo se describe cómo implementar un servidor de configuración para realizar la recuperación ante desastres de VMware con Azure Site Recovery
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 05/06/2018
ms.author: raynew
ms.openlocfilehash: 841176d8c5f215d18edf25b1f191792b37555fa9
ms.sourcegitcommit: 65b399eb756acde21e4da85862d92d98bf9eba86
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/22/2018
ms.locfileid: "36318126"
---
# <a name="deploy-a-configuration-server"></a>Implementar un servidor de configuración

Cuando se usa [Azure Site Recovery](site-recovery-overview.md) para realizar la recuperación ante desastres de servidores físicos y máquinas virtuales de VMware en Azure, se implementa un servidor de configuración local. El servidor de configuración coordina la comunicación entre Azure y VMware local. También administra la replicación de datos. Este artículo lo guiará por los pasos necesarios para implementar el servidor de configuración a la hora de replicar máquinas virtuales VMware en Azure. [Siga este artículo](physical-azure-set-up-source.md) si necesita configurar un servidor de configuración para la replicación del servidor físico.

>[!TIP]
Puede obtener información acerca del papel del servidor de configuración como parte de la arquitectura de Azure Site Recovery [aquí](vmware-azure-architecture.md).

## <a name="deployment-of-configuration-server-through-ova-template"></a>Implementación del servidor de configuración a través de una plantilla de OVA

El servidor de configuración debe configurarse como una máquina virtual de VMware de alta disponibilidad con ciertos requisitos mínimos de hardware y tamaño. Para una implementación cómoda y fácil, Site Recovery proporciona una plantilla de OVA (Open Virtualization Application) descargable para configurar el servidor de configuración que cumple con todos los requisitos obligatorios que se enumeran a continuación.

## <a name="prerequisites"></a>requisitos previos

En la tabla siguiente se resumen los requisitos mínimos de hardware necesarios para el servidor de configuración.

[!INCLUDE [site-recovery-configuration-server-requirements](../../includes/site-recovery-configuration-and-scaleout-process-server-requirements.md)]

## <a name="capacity-planning"></a>Planificación de capacidad

Los requisitos de tamaño para el servidor de configuración dependen de la tasa de cambio de datos potencial. Use esta tabla como guía.

| **CPU** | **Memoria** | **Tamaño del disco de caché** | **Frecuencia de cambio de datos** | **Máquinas protegidas** |
| --- | --- | --- | --- | --- |
| 8 vCPU (2 sockets * 4 núcleos @ 2,5 GHz) |16 GB |< 300 GB |500 GB o menos |Replicar menos de 100 máquinas. |
| 12 vCPUs (2 sockets * 6 núcleos @ 2,5 GHz) |18 GB |600 GB |500 GB a 1 TB |Replicar 100-150 máquinas. |
| 16 vCPUs (2 sockets * 8 núcleos @ 2,5 GHz) |32 GB |1 TB |1 TB a 2 TB |Replicar 150-200 máquinas. |

Si replica más de una máquina virtual de VMware, consulte las [consideraciones de planeación de capacidad](/site-recovery-plan-capacity-vmware.md). Ejecute la [herramienta Deployment Planner](site-recovery-deployment-planner.md) para la replicación de VMware.

## <a name="download-the-template"></a>Descarga de la plantilla

1. En el almacén, vaya a **Preparar infraestructura** > **Origen**.
2. En **Preparar origen**, seleccione **+Servidor de configuración**.
3. En **Agregar servidor**, compruebe que aparezca **Servidor de configuración para VMware** en **Tipo de servidor**.
4. Descargue la plantilla de Open Virtualization Application (OVA) para el servidor de configuración.

  > [!TIP]
>También puede descargar la versión más reciente de la plantilla del servidor de configuración directamente desde el [Centro de descarga de Microsoft](https://aka.ms/asrconfigurationserver).

>[!NOTE]
La licencia proporcionada con la plantilla de OVA es una licencia de evaluación válida durante 180 días. Tras este periodo, el cliente debe activar Windows con una licencia adquirida.

## <a name="import-the-template-in-vmware"></a>Importación de la plantilla en VMware

1. Inicie sesión en el servidor VMware vCenter o el host vSphere ESXi con el cliente de VMWare vSphere.
2. En el menú **File** (Archivo), seleccione **Deploy OVF Template** (Implementar plantilla OVF) para iniciar el Asistente para implementar plantillas OVF.

     ![Plantilla de OVF](./media/vmware-azure-deploy-configuration-server/vcenter-wizard.png)

3. En **Select source** (Seleccionar origen), especifique la ubicación de la plantilla OVF descargada.
4. En **Review details** (Revisar detalles), seleccione **Next** (Siguiente).
5. En **Select name and folder** (Seleccionar nombre y la carpeta) y **Select configuration** (Seleccionar configuración), acepte la configuración predeterminada.
6. En **Select storage** (Seleccionar almacenamiento), seleccione **Thick Provision Eager Zeroed** (Aprovisionamiento grueso diligente con ceros) en **Select virtual disk format** (Seleccionar formato de disco virtual) para el mejor rendimiento.
7. En el resto de las páginas del asistente, acepte la configuración predeterminada.
8. En **Ready to complete** (Listo para completar):

    * Para configurar la máquina virtual con la configuración predeterminada, seleccione **Power on after deployment** > **Finish** (Encender después de la implementación > Finalizar).

    * Para agregar una interfaz de red adicional, desactive **Power on after deployment** (Encender después de la implementación) y seleccione **Finish** (Finalizar). De forma predeterminada, la plantilla del servidor de configuración se implementa con una sola NIC. Puede agregar NIC adicionales después de la implementación.

## <a name="add-an-additional-adapter"></a>Incorporación de un adaptador adicional

Si desea agregar una NIC adicional al servidor de configuración, hágalo antes de registrar el servidor en el almacén. No se admiten adaptadores adicionales después del registro.

1. En el inventario de cliente de vSphere, haga clic con el botón derecho en la máquina virtual y seleccione **Editar configuración**.
2. En **Hardware**, seleccione **Agregar** > **Adaptador Ethernet**. Luego, seleccione **Siguiente**.
3. Seleccione el tipo de adaptador y la red. 
4. Para conectar la NIC virtual al encender la máquina virtual, seleccione **Connect at power on** (Conectar al encender). Después, seleccione **Siguiente** > **Finalizar** > **Aceptar**.

## <a name="register-the-configuration-server-with-azure-site-recovery-services"></a>Registrar el servidor de configuración con los servicios de Azure Site Recovery

1. Desde la consola de cliente de VMWare vSphere, encienda la máquina virtual.
2. La máquina virtual se inicia en una experiencia de instalación de Windows Server 2016. Acepte el contrato de licencia y especifique una contraseña de administrador.
3. Una vez finalizada la instalación, inicie sesión en la máquina virtual como administrador.
4. La primera vez que inicie sesión, se inicia la herramienta de configuración de Azure Site Recovery en unos segundos.
5. Especifique el nombre que se usará para registrar el servidor de configuración en Site Recovery. Luego, seleccione **Siguiente**.
6. La herramienta comprueba que la máquina virtual pueda conectarse a Azure. Una vez establecida la conexión, haga clic en **Iniciar sesión** para iniciar sesión en la suscripción de Azure. Las credenciales deben tener acceso al almacén donde desea registrar el servidor de configuración.
7. La herramienta realiza algunas tareas de configuración y, a continuación, se reinicia.
8. Inicie sesión de nuevo en la máquina. En pocos segundos, el Asistente para administración del servidor de configuración se inicia **automáticamente**.

### <a name="configure-settings"></a>Definición de la configuración

1. En el asistente para la administración del servidor de configuración, seleccione **Configurar conectividad** y, a continuación, seleccione la NIC que el servidor de procesos usará para recibir el tráfico de replicación de las VM. Después, seleccione **Guardar**. Una vez configurada, esta opción no se puede cambiar.
2. En **Seleccionar almacén de Recovery Services**, inicie sesión en Microsoft Azure, seleccione la suscripción de Azure y el grupo de recursos y almacén correspondientes.
    >[!NOTE]
    > Una vez registrado, no hay ninguna flexibilidad para cambiar el almacén de Recovery Services.
3. En **Instalar software de terceros**,

    |Escenario   |Pasos a seguir  |
    |---------|---------|
    |¿Puedo descargar e instalar MySQL manualmente?     |  Sí. Descargue la aplicación MySQL y colóquela en la carpeta **C:\Temp\ASRSetup** y, después, instálela manualmente. Cuando acepte los términos > haga clic en **Descargar e instalar**, el portal indicará que *ya está instalado*. Siga con el paso siguiente.       |
    |¿Se puede evitar la descarga de MySQL en línea?     |   Sí. Coloque la aplicación del instalador de MySQL en la carpeta **C:\Temp\ASRSetup**. Acepte los términos > haga clic en **Descargar e instalar**, el portal usará el instalador que agregó e instalará la aplicación. Puede continuar con el siguiente paso después de la instalación.    |
    |Me gustaría descargar e instalar MySQL a través de Azure Site Recovery     |  Acepte el Contrato de licencia y haga clic en **Descargar e instalar**. A continuación, puede continuar con el siguiente paso después de la instalación.       |
4. En **Validate appliance configuration** (Comprobar configuración del dispositivo), se comprueban los requisitos previos antes de continuar.
5. En **Configure vCenter Server/vSphere ESXi server** (Configurar vCenter Server/vSphere ESXi Server), especifique la dirección IP o FQDN del servidor vCenter o host de vSphere en los que se encuentran las máquinas virtuales donde desea realizar la replicación. Especifique el puerto en el que escucha el servidor. Escriba un nombre descriptivo que se usará para el servidor de VMware en el almacén.
6. Especifique las credenciales que usará el servidor de configuración para conectarse al servidor de VMware. Site Recovery usa estas credenciales para detectar automáticamente las máquinas virtuales de VMware disponibles para la replicación. Seleccione **Agregar** y, luego, **Continuar**. Las credenciales especificadas se guardan localmente.
7. En **Configure virtual machine credentials** (Configurar las credenciales de la máquina virtual), especifique el nombre de usuario y la contraseña de las máquinas virtuales para instalar automáticamente Mobility Service durante la replicación. Para las máquinas **Windows**, la cuenta necesita privilegios de administrador local en las máquinas que se vayan a replicar. Para **Linux**, proporcione los detalles de la cuenta raíz.
8. Seleccione **Finalize configuration** (Terminar configuración) para completar el registro.
9. Una vez concluido el registro, abra Azure Portal, compruebe que el servidor de configuración y el servidor de VMware se muestran en **Almacén de Recovery Services** > **Administrar** > **Infraestructura de Site Recovery** > **Servidores de configuración**.

## <a name="faq"></a>Preguntas más frecuentes

1. ¿Puedo usar la máquina virtual donde está instalado el servidor de configuración para propósitos diferentes? **No**, el servidor de configuración debe ser un servidor de propósito único y no está permitido usarlo como servidor compartido.
2. ¿Puedo cambiar el almacén ya registrado en el servidor de configuración por un almacén recién creado? **No**, una vez que un almacén está registrado con el servidor de configuración, no se puede cambiar.
3. ¿Puedo usar el mismo servidor de configuración para proteger las máquinas físicas y virtuales? **Sí**, puede usarse el mismo servidor de configuración para la replicación de máquinas físicas y virtuales. Sin embargo, no se admite la conmutación por recuperación a una máquina física.
4. ¿Dónde se usará el servidor de configuración? Consulte la arquitectura de Azure Site Recovery [aquí](vmware-azure-architecture.md) para obtener más información acerca del servidor de configuración y sus funcionalidades.
5. ¿Dónde puedo encontrar la versión más reciente del servidor de configuración? Puede descargarla directamente del [Centro de descarga de Microsoft](https://aka.ms/asrconfigurationserver). Consulte el artículo que incluye los pasos para actualizar el servidor de configuración [aquí](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server).

## <a name="upgrade-the-configuration-server"></a>Actualización del servidor de configuración

Para actualizar el servidor de configuración a la versión más reciente, consulte los pasos indicados [aquí](vmware-azure-manage-configuration-server.md#upgrade-the-configuration-server)

## <a name="troubleshoot-deployment-issues"></a>Solución de problemas de implementación

[!INCLUDE [site-recovery-vmware-to-azure-install-register-issues](../../includes/site-recovery-vmware-to-azure-install-register-issues.md)]



## <a name="next-steps"></a>Pasos siguientes

Configure la recuperación ante desastres en Azure para [máquinas virtuales VMware](vmware-azure-tutorial.md).
