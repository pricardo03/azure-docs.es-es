---
title: Configuración del entorno de origen y el servidor de configuración para la recuperación ante desastres de máquinas virtuales de VMware en Azure con Azure Site Recovery | Microsoft Docs
description: En este artículo se describe cómo configurar el entorno local y el servidor de configuración para la recuperación ante desastres de máquinas virtuales de VMware en Azure con Azure Site Recovery.
services: site-recovery
author: Rajeswari-Mamilla
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 10/29/2018
ms.author: ramamill
ms.openlocfilehash: e09e93ef85449c51e35b8da7ad93ee7088a0e7b4
ms.sourcegitcommit: 6b7c8b44361e87d18dba8af2da306666c41b9396
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/12/2018
ms.locfileid: "51566319"
---
# <a name="set-up-the-source-environment-and-configuration-server"></a>Configuración del entorno de origen y el servidor de configuración

En este artículo se describe cómo configurar el entorno local de origen para la recuperación ante desastres de máquinas virtuales de VMware en Azure con [Azure Site Recovery](site-recovery-overview.md). Incluye los pasos para configurar una máquina local como el servidor de configuración de Site Recovery y detectar automáticamente las máquinas virtuales locales. 

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, asegúrese de que ha hecho lo siguiente

- Planear la implementación de la recuperación ante desastres con [Azure Site Recovery Deployment Planner](site-recovery-deployment-planner.md). La herramienta de planeación ayuda a garantizar que haya suficientes recursos y ancho de banda para la recuperación ante desastres y el objetivo de punto de recuperación (RPO) de la organización.
- [Configurar los recursos de Azure](tutorial-prepare-azure.md) en [Azure Portal](http://portal.azure.com).
- [Configurar los recursos locales de VMware](vmware-azure-tutorial-prepare-on-premises.md), incluida una cuenta dedicada para la detección automática de máquinas virtuales de VMware locales.
- Puede [revisar las preguntas más frecuentes](vmware-azure-common-questions.md#configuration-server) acerca de la implementación y administración del servidor de configuración.


## <a name="choose-protection-goals"></a>Elección de los objetivos de protección

1. En **Almacenes de Recovery Services**, seleccione el nombre del almacén. 
2. En **Introducción**, seleccione Site Recovery. Luego, seleccione **Preparar infraestructura**.
3. En **Objetivo de protección** > **¿Dónde están ubicadas las máquinas?**, seleccione **Local**.
4. En **¿A dónde quiere replicar las máquinas?**, seleccione **En Azure**.
5. En **¿Las máquinas están virtualizadas?**, seleccione **Yes, with VMware vSphere Hypervisor** (Sí, con VMware vSphere Hypervisor). Después seleccione **Aceptar**.

## <a name="about-the-configuration-server"></a>Acerca del servidor de configuración

Cuando se configura la recuperación ante desastres de máquinas virtuales de VMware en Azure, se implementa un servidor de configuración local.

- El servidor de configuración coordina la comunicación entre Azure y VMware local. También administra la replicación de datos.
- El servidor de configuración se implementa como una máquina virtual de VMWare.
- Site Recovery proporciona una plantilla OVA que puede descargar desde Azure Portal y, después, importar en vCenter Server para configurar la máquina virtual del servidor de configuración.
- [Más información](vmware-azure-architecture.md) acerca de los componentes y procesos del servidor de configuración.


>[!NOTE]
No utilice estas instrucciones si va a implementar el servidor de configuración para la recuperación ante desastres de máquinas físicas locales en Azure. En este escenario, siga [este artículo](physical-azure-set-up-source.md).


## <a name="before-you-deploy-the-configuration-server"></a>Antes de implementar el servidor de configuración

Si instala al servidor de configuración como una máquina virtual de VMware mediante la plantilla OVA, la máquina virtual se instalará con todos los requisitos previos. Si desea revisar los requisitos previos, use los siguientes artículos.

- [Obtenga información sobre](vmware-azure-configuration-server-requirements.md) el hardware, el software y los requisitos de capacidad del servidor de configuración.
- Si va a replicar varias máquinas virtuales de VMware, consulte las [consideraciones de planeamiento de capacidad](site-recovery-plan-capacity-vmware.md) y ejecute la herramienta [Azure Site Recovery Deployment Planner](site-recovery-deployment-planner.md) para la replicación de VMWare.
- La licencia de Windows de la plantilla OVA es una licencia de evaluación válida durante 180 días. Transcurrido ese tiempo, deberá activar Windows con una licencia válida. 
- La plantilla OVA proporciona una manera sencilla de configurar el servidor de configuración como una máquina virtual de VMware. Si por algún motivo tiene que configurar la máquina virtual de VMware sin la plantilla, revise los requisitos previos y siga las instrucciones manuales de [este artículo](physical-azure-set-up-source.md).
- La cuenta de Azure necesita permisos para crear aplicaciones de Azure AD.


>[!IMPORTANT]
El servidor de configuración se debe implementar como se describe en este artículo. No se admite la copia o clonación de un servidor de configuración existente.

### <a name="set-up-azure-account-permissions"></a>Configuración de los permisos de la cuenta de Azure

Un administrador de inquilinos o global puede asignar a la cuenta permisos para crear aplicaciones de Azure AD o bien asignar el rol de desarrollador de aplicaciones (que tiene los permisos) a la cuenta.


El administrador de inquilinos o global puede conceder permisos para la cuenta como se indica:

1. En Azure AD, el administrador de inquilinos o global debe ir a **Azure Active Directory** > **Usuarios** > **Configuración de usuario**.
2. El administrador debe establecer **Registros de aplicaciones** en **Sí**.

 > [!NOTE]
 > Se trata de una configuración predeterminada que no es confidencial. [Más información](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added#who-has-permission-to-add-applications-to-my-azure-ad-instance).

O bien, el administrador de inquilinos o global tiene permisos para asignar el rol a la cuenta. [Más información](https://docs.microsoft.comazure/active-directory/fundamentals/active-directory-users-assign-role-azure-portal).

 


## <a name="download-the-ova-template"></a>Descarga de la plantilla OVA

1. En el almacén, vaya a **Preparar infraestructura** > **Origen**.
2. En **Preparar origen**, seleccione **+Servidor de configuración**.
3. En **Agregar servidor**, compruebe que aparezca **Servidor de configuración para VMware** en **Tipo de servidor**.
4. Descargue la plantilla de Open Virtualization Application (OVA) para el servidor de configuración.

  > [!TIP]
>También puede descargar la versión más reciente de la plantilla del servidor de configuración desde el [Centro de descarga de Microsoft](https://aka.ms/asrconfigurationserver).


## <a name="import-the-ova-template-in-vmware"></a>Importación de la plantilla OVA en VMware

1. Inicie sesión en el servidor VMware vCenter o el host vSphere ESXi con el cliente de VMWare vSphere.
2. En el menú **File** (Archivo), seleccione **Deploy OVF Template** (Implementar plantilla OVF) para iniciar el Asistente para implementar plantillas OVF.

     ![Plantilla de OVF](./media/vmware-azure-deploy-configuration-server/vcenter-wizard.png)

3. En **Select source** (Seleccionar origen), especifique la ubicación de la plantilla OVF descargada.
4. En **Review details** (Revisar detalles), seleccione **Next** (Siguiente).
5. En **Select name and folder** (Seleccionar nombre y la carpeta) y **Select configuration** (Seleccionar configuración), acepte la configuración predeterminada.
6. En **Select storage** (Seleccionar almacenamiento), seleccione **Thick Provision Eager Zeroed** (Aprovisionamiento grueso diligente con ceros) en **Select virtual disk format** (Seleccionar formato de disco virtual) para el mejor rendimiento.
7. En el resto de las páginas del asistente, acepte la configuración predeterminada.
8. En **Ready to Complete** (Listo para completarse), para configurar la máquina virtual con la configuración predeterminada, seleccione **Power on after deployment** > **Finish** (Encender después de la implementación > Finalizar).
9. De forma predeterminada, la máquina virtual se implementa con una sola NIC. Si desea agregar una NIC adicional, desactive **Power on after deployment** (Encender después de la implementación) y haga clic en **Finish** (Finalizar). A continuación, siga el procedimiento siguiente.

## <a name="add-an-adapter-to-the-configuration-server"></a>Adición de un adaptador al servidor de configuración

Si desea agregar una NIC adicional al servidor de configuración, hágalo antes de registrar el servidor en el almacén. No se admiten adaptadores adicionales después del registro.

1. En el inventario de cliente de vSphere, haga clic con el botón derecho en la máquina virtual y seleccione **Editar configuración**.
2. En **Hardware**, seleccione **Agregar** > **Adaptador Ethernet**. Luego, seleccione **Siguiente**.
3. Seleccione el tipo de adaptador y la red.
4. Para conectar la NIC virtual al encender la máquina virtual, seleccione **Connect at power on** (Conectar al encender). Después, seleccione **Siguiente** > **Finalizar** > **Aceptar**.

## <a name="register-the-configuration-server"></a>Registro del servidor de configuración 
Encienda la máquina virtual y registre el servidor de configuración en el almacén de Site Recovery.

1. Desde la consola de cliente de VMWare vSphere, encienda la máquina virtual.
2. La máquina virtual se inicia en la experiencia de instalación de Windows Server 2016. Acepte el contrato de licencia y especifique una contraseña de administrador.
3. Una vez finalizada la instalación, inicie sesión en la máquina virtual como administrador.



## <a name="set-up-the-configuration-server"></a>Configuración del servidor de configuración

Como parte de la implementación, deberá instalar MySQL en la máquina virtual del servidor de configuración. Puede hacerlo de dos maneras:

- Permita que Site Recovery descargue e instale MySQL. Si desea utilizar esta opción, no es necesario hacer nada antes de iniciar la configuración del servidor de configuración.
- Descargue e instale MySQL manualmente: antes de configurar el servidor de configuración, descargue el instalador de MySQL y colóquelo la carpeta **C:\Temp\ASRSetup**. A continuación, instale MySQL. 
- Realice una descarga manual y permita que Site Recovery lo instale. Antes de configurar el servidor de configuración, descargue el instalador de MySQL y colóquelo la carpeta **C:\Temp\ASRSetup**.


1. La primera vez que inicie sesión en la máquina virtual, se inicia la herramienta de configuración de Azure Site Recovery.
2. Especifique el nombre utilizado para registrar el servidor de configuración en el almacén de Site Recovery. Luego, seleccione **Siguiente**.
3. La herramienta comprueba que la máquina virtual pueda conectarse a Azure. Una vez establecida la conexión, haga clic en **Iniciar sesión** para iniciar sesión en la suscripción de Azure. Tenga en cuenta que la cuenta debe tener acceso al almacén en el que desea registrar el servidor de configuración.
4. La herramienta realiza algunas tareas de configuración y, a continuación, se reinicia.
5. Inicie sesión de nuevo en la máquina. En pocos segundos, el Asistente para administración del servidor de configuración se inicia automáticamente.
6. En el asistente, seleccione **Configurar conectividad**.
7. Seleccione la NIC que usa el servidor de procesos (se ejecuta de manera predeterminada en el servidor de configuración) para recibir el tráfico de replicación de las máquinas virtuales.
8. Después, seleccione **Guardar**. Tenga en cuenta que no se puede cambiar la configuración del almacén después de registrar el servidor de configuración. 
9. En **Seleccionar almacén de Recovery Services**, inicie sesión en Microsoft Azure, seleccione la suscripción de Azure y el grupo de recursos y almacén correspondientes. 
10. En **Instalar software de terceros**, instale MySQL:
     - Si Site Recovery se ocupa de la descarga y la instalación de MySQL, haga clic en **Descargar e instalar**. Espere a que la instalación finalice y, a continuación, continúe con el asistente.
     - Si descargó MySQL y Site Recovery va a realizar la instalación, haga clic en **Descargar e instalar**. Espere a que la instalación finalice y continúe con el asistente.
     - Si descargó e instaló MySQL de forma manual, haga clic en **Descargar e instalar**. La aplicación se muestra como **Ya instalado**. Continúe con el asistente.
11. En **Validate appliance configuration** (Comprobar configuración del dispositivo), la instalación comprueba los requisitos previos antes de continuar. 
12. En **Configure vCenter Server/vSphere ESXi server** (Configurar vCenter Server/vSphere ESXi Server), escriba la dirección IP o FQDN del servidor vCenter o host de vSphere en los que se encuentran las máquinas virtuales que desea replicar. Especifique el puerto de escucha del servidor y un nombre descriptivo para el servidor de VMware en el almacén.
13. Escriba las credenciales que usará el servidor de configuración para conectarse al servidor de VMware y detectar automáticamente las máquinas virtuales de VMware que están disponibles para la replicación. Seleccione **Add** >  **Continue** (Agregar > Continuar). Las credenciales se guardan localmente.
14. En **Configure virtual machine credentials** (Configurar las credenciales de la máquina virtual), especifique las credenciales que usará Site Recovery para instalar automáticamente el Servicio de Movilidad cuando se habilite la replicación en las máquinas.
    - Para las máquinas Windows, la cuenta necesita privilegios de administrador local en las máquinas que se vayan a replicar.
    - Para Linux, proporcione los detalles de la cuenta raíz.
15. Seleccione **Finalize configuration** (Terminar configuración) para completar el registro.
16. Una vez concluido el registro, abra Azure Portal y compruebe que el servidor de configuración y el servidor de VMware se muestran en **Almacén de Recovery Services** > **Administrar** > **Infraestructura de Site Recovery** > **Servidores de configuración**.


## <a name="exclude-antivirus-on-the-configuration-server"></a>Exclusión del antivirus en el servidor de configuración

Si está ejecutando software antivirus en la máquina virtual del servidor de configuración, asegúrese de que las siguientes carpetas se excluyen de las operaciones antivirus. Esto garantiza que la replicación y la conectividad funcionan según lo esperado: 

- C:\Archivos de programa\Microsoft Azure Recovery Services Agent
- C:\Archivos de programa\Microsoft Azure Site Recovery Provider
- C:\Archivos de programa\Microsoft Azure Site Recovery Configuration Manager
- C:\Archivos de programa\Microsoft Azure Site Recovery Error Collection Tool
- C:\thirdparty
- C:\Temp
- C:\strawberry
- C:\ProgramData\MySQL
- C:\Archivos de programa (x86)\MySQL
- C:\ProgramData\ASR
- C:\ProgramData\Microsoft Azure Site Recovery
- C:\ProgramData\ASRLogs
- C:\ProgramData\ASRSetupLogs
- C:\ProgramData\LogUploadServiceLogs
- C:\inetpub
- Directorio de instalación de Site Recovery. Por ejemplo: E:\Archivos de programa (x86)\Microsoft Azure Site Recovery


## <a name="next-steps"></a>Pasos siguientes
- Si experimenta dificultades, revise las [Preguntas más frecuentes](vmware-azure-common-questions.md#configuration server) y la [Solución de problemas](vmware-azure-troubleshoot-configuration-server.md) del servidor de configuración.
- Seguidamente, [configure el entorno de destino](./vmware-azure-set-up-target.md) en Azure.
