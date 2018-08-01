---
title: Migrar servidores de Windows Server 2008 locales a Azure con Azure Site Recovery | Microsoft Docs
description: En este artículo se describe cómo migrar máquinas de Windows Server 2008 locales a Azure mediante Azure Site Recovery.
services: site-recovery
documentationcenter: ''
author: bsiva
manager: abhemraj
editor: raynew
ms.assetid: ''
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/23/2018
ms.author: bsiva
ms.openlocfilehash: 552a0d131f630db7b3a73293d330377ee350d2a9
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/23/2018
ms.locfileid: "39214625"
---
# <a name="migrate-servers-running-windows-server-2008-2008-r2-to-azure"></a>Migrar los servidores que ejecutan Windows Server 2008, 2008 R2 en Azure

En este tutorial se muestra cómo migrar los servidores locales que ejecutan Windows Server 2008 o 2008 R2 a Azure con Azure Site Recovery. En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Preparar los servidores locales para la migración
> * Configuración del entorno de destino
> * Configurar una directiva de replicación
> * Habilitar replicación
> * Ejecute una migración de prueba para asegurarse de que todo funciona de la forma esperada
> * Conmutación por error a Azure y completar la migración

Las sección de limitaciones y problemas conocidos enumera algunas de las limitaciones y soluciones alternativas para problemas conocidos que puede encontrar mientras realiza la migración de máquinas de Windows Server 2008 a Azure. 


## <a name="supported-operating-systems-and-environments"></a>Sistemas operativos y entornos compatibles


|Sistema operativo  | Entorno local  |
|---------|---------|
|Windows Server 2008 SP2: 32 y 64 bits (IA-32 y x86-64)</br>- Standard</br>- Enterprise</br>- Datacenter   |     Máquinas virtuales de VMware, máquinas virtuales de Hyper-V y servidores físicos    |
|Windows Server 2008 R2 SP1: 64 bits</br>- Standard</br>- Enterprise</br>- Datacenter     |     Máquinas virtuales de VMware, máquinas virtuales de Hyper-V y servidores físicos|

> [!WARNING]
> - No se admite la migración de servidores que ejecutan Server Core.
> - Asegúrese de que tiene el último Service Pack y las actualizaciones de Windows instalados antes de migrar.


## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, es útil revisar la arquitectura de Azure Site Recovery para [la migración de servidores de VMware y físicos](vmware-azure-architecture.md) o [la migración de máquinas virtuales de Hyper-V](hyper-v-azure-architecture.md) 

Para migrar máquinas virtuales de Hyper-V que ejecutan Windows Server 2008 o Windows Server 2008 R2, siga los pasos descritos en el tutorial [migrar máquinas locales a Azure](migrate-tutorial-on-premises-azure.md).

El resto de este tutorial muestra cómo puede migrar máquinas virtuales de VMware locales y servidores físicos que ejecutan Windows Server 2008 o 2008 R2.


## <a name="limitations-and-known-issues"></a>Limitaciones y problemas conocidos

- El servidor de configuración, los servidores de procesos adicionales y el servicio de movilidad que se usa para migrar los servidores de Windows Server 2008 SP2 deberían usar la versión 9.18.0.1 del software de Azure Site Recovery. El programa de instalación unificado para la versión 9.18.0.1 del servidor de configuración y el servidor de procesos puede descargarse desde [https://aka.ms/asr-w2k8-migration-setup](https://aka.ms/asr-w2k8-migration-setup).

- Un servidor de configuración existente o de procesos existente no se puede usar para migrar servidores que ejecutan Windows Server 2008 SP2. Se debe aprovisionar un nuevo servidor de configuración con la versión 9.18.0.1 del software de Azure Site Recovery. Este servidor de configuración solo debe usarse para la migración de servidores de Windows en Azure.

- No se admiten puntos de recuperación coherentes con la aplicación y la característica de coherencia de varias máquinas virtuales para la replicación de servidores que ejecutan Windows Server 2008 SP2. Los servidores de Windows Server 2008 SP2 deben migrarse a un punto de recuperación coherente para el bloqueo. Los puntos de recuperación coherentes para el bloqueo se generan cada 5 minutos de forma predeterminada. Mediante una directiva de replicación con una frecuencia de instantánea coherente con la aplicación configurada hará que el estado de replicación se vuelva crítico debido a la falta de puntos de recuperación coherentes con la aplicación. Para evitar falsos positivos, establezca la frecuencia de instantáneas coherentes con la aplicación en la directiva de replicación en "Desactivado".

- Los servidores que se están migrando deben tener .NET Framework 3.5 Service Pack 1 para que el servicio de movilidad funcione.


- Si el servidor tiene discos dinámicos, verá que en determinadas configuraciones estos discos en el servidor con conmutación por error se marcan como desconectados o se muestran como discos externos. También es posible que vea que el estado del conjunto reflejado para los volúmenes reflejados en discos dinámicos se marca como "Error de redundancia". Puede solucionar este problema desde diskmgmt.msc importando manualmente estos discos y reactivándolos.

- Los servidores que se están migrando deben tener el controlador vmstorfl.sys. La conmutación por error puede producir un error si el controlador no está presente en el servidor que se está migrando. 
  > [!TIP]
  >Compruebe si el controlador se encuentra en "C:\Windows\system32\drivers\vmstorfl.sys". Si no se encuentra el controlador, puede solucionar el problema mediante la creación de un archivo ficticio en su lugar. 
  >
  > Abra un símbolo (Ejecutar > Cmd) y ejecute lo siguiente: "copy nul c:\Windows\system32\drivers\vmstorfl.sys"

- Es posible que no pueda usar el protocolo RDP a los servidores de Windows Server 2008 SP2 que ejecutan el sistema operativo de 32 bits inmediatamente después de que se conmuten por error o se conmuten por error de prueba a Azure. Reinicie la máquina virtual conmutada por error desde Azure Portal e intente conectarse de nuevo. Si todavía no puede conectarse, compruebe si el servidor está configurado para permitir conexiones a escritorio remotas y asegúrese de que no hay ninguna regla de firewall ni grupos de seguridad de red bloqueando la conexión. 
  > [!TIP]
  > Se recomienda una conmutación por error de prueba antes de migrar los servidores. Asegúrese de que ha realizado al menos una conmutación por error correcta en cada servidor que va a migrar. Como parte de la prueba de conmutación por error, conéctese a la máquina de la conmutación por error de prueba y asegúrese que las cosas funcionan según lo previsto.
  >
  >La operación de conmutación por error de prueba no es disruptiva y le ayuda a probar las migraciones mediante la creación de máquinas virtuales en una red aislada de su elección. A diferencia de la operación de conmutación por error, durante la operación de conmutación por error de prueba, la replicación de datos continúa realizándose. Puede realizar tantas conmutaciones por error de prueba como sea necesario antes de tener todo listo para migrar. 
  >
  >


## <a name="getting-started"></a>Introducción

Realice las siguientes tareas para preparar el entorno de VMware y físico locales y la suscripción de Azure:

1. [Preparación de Azure](tutorial-prepare-azure.md)
2. Preparación de [VMware](vmware-azure-tutorial-prepare-on-premises.md) local


## <a name="create-a-recovery-services-vault"></a>Creación de un almacén de Recovery Services

1. Inicie sesión en [Azure Portal](https://portal.azure.com) > **Recovery Services**.
2. Haga clic en **Crear un recurso** > **Supervisión y administración** > **Backup y Site Recovery**.
3. En el apartado **Nombre**, especifique el nombre descriptivo **W2K8-migration**. Si tiene más de una suscripción, seleccione la apropiada.
4. Cree un grupo de recursos **w2k8migrate**.
5. Especifique una región de Azure. Para comprobar las regiones admitidas, consulte la disponibilidad geográfica en [Detalles de precios de Azure Site Recovery](https://azure.microsoft.com/pricing/details/site-recovery/).
6. Para acceder rápidamente al almacén desde el panel, haga clic en **Anclar al panel** y, después, en **Crear**.

   ![Almacén nuevo](media/migrate-tutorial-windows-server-2008/migrate-windows-server-2008-vault.png)

El nuevo almacén se agrega al **Panel**, en **Todos los recursos**, y en la página principal **Almacenes de Recovery Services**.


## <a name="prepare-your-on-premises-environment-for-migration"></a>Preparar los servidores locales para la migración

- Descargue el instalador del servidor de configuración (Configuración unificada) desde [https://aka.ms/asr-w2k8-migration-setup](https://aka.ms/asr-w2k8-migration-setup)
- Siga los pasos descritos a continuación para configurar el entorno de origen mediante el archivo del instalador descargado en el paso anterior.

> [!IMPORTANT]
> - Asegúrese de que usa el archivo de instalación descargado en el primer paso para instalar y registrar el servidor de configuración. No descargue el archivo del programa de instalación del agente desde Azure Portal. El archivo de instalación disponible en [https://aka.ms/asr-w2k8-migration-setup](https://aka.ms/asr-w2k8-migration-setup) es la única versión que admite la migración de Windows Server 2008.
>
> - No se puede usar un servidor de configuración existente para migrar las máquinas que ejecutan Windows Server 2008. Deberá configurar un nuevo servidor de configuración mediante el vínculo proporcionado anteriormente.
>
> - Siga los pasos indicados a continuación para instalar al servidor de configuración. No intente ejecutar el programa de instalación unificada directamente para usar el procedimiento de instalación basada en la GUI. Esto producirá un error de instalación que indica que no hay ninguna conexión a Internet.

 
1) Descargue el archivo de credenciales del almacén desde el portal: en Azure Portal, seleccione el almacén de Recovery Services creado en el paso anterior. En el menú de la página del almacén, seleccione **Site Recovery Infrastructure** > **Configuration Servers** (Infraestructura de Site Recovery > Servidores de configuración). Haga clic en **+Server** (+Servidor). Seleccione *Configuration Server for Physical* (Servidor de configuración para físico) en el menú desplegable de la página que se abre. Haga clic en el botón de descarga del paso 4 para descargar el archivo de credenciales del almacén.

 ![Descargar la clave de registro del almacén](media/migrate-tutorial-windows-server-2008/download-vault-credentials.png) 

2) Copie el archivo de credenciales del almacén descargado en el paso anterior y el archivo de instalación unificada descargado anteriormente en el escritorio de la máquina del servidor de configuración (la máquina Windows Server 2012 R2 o Windows Server 2016 en la que va a instalar el software del servidor de configuración).

3) Asegúrese de que el servidor de configuración tiene conexión a Internet y que el reloj del sistema y la zona horaria del equipo están configurados correctamente. Descargue el instalador de [MySQL 5.7](https://dev.mysql.com/get/Downloads/MySQLInstaller/mysql-installer-community-5.7.20.0.msi) y colóquelo en *C:\Temp\ASRSetup* (cree el directorio si no existe). 

4) Cree un archivo de credenciales de MySQL con las siguientes líneas y colóquelo en el escritorio en **C:\Users\Administrator\MySQLCreds.txt**. Reemplace "Password~1" a continuación con una contraseña segura y adecuada:

```
[MySQLCredentials]
MySQLRootPassword = "Password~1"
MySQLUserPassword = "Password~1"
```

5) Ejecute el comando siguiente para extraer el contenido del archivo de instalación unificada descargado en el escritorio:

```
cd C:\Users\Administrator\Desktop

MicrosoftAzureSiteRecoveryUnifiedSetup.exe /q /x:C:\Users\Administrator\Desktop\9.18
```
  
6) Ejecute los comandos siguientes para instalar el software del servidor de configuración con el contenido extraído:

```
cd C:\Users\Administrator\Desktop\9.18.1

UnifiedSetup.exe /AcceptThirdpartyEULA /ServerMode CS /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /MySQLCredsFilePath "C:\Users\Administrator\Desktop\MySQLCreds.txt" /VaultCredsFilePath <vault credentials file path> /EnvType VMWare /SkipSpaceCheck
```

## <a name="set-up-the-target-environment"></a>Configuración del entorno de destino

Seleccione y compruebe los recursos de destino.

1. Haga clic en **Preparar infraestructura** > **Destino** y seleccione la suscripción de Azure que quiere usar.
2. Especifique el modelo de implementación de Resource Manager.
3. Site Recovery comprueba que tiene una o más redes y cuentas de Azure Storage compatibles.


## <a name="set-up-a-replication-policy"></a>Configurar una directiva de replicación

1. Para crear una directiva de replicación, haga clic en **Site Recovery Infrastructure (Infraestructura de Site Recovery)** > **Directivas de replicación** > **+Directiva de replicación**.
2. En **Crear directiva de replicación**, especifique un nombre de directiva.
3. En **Umbral de RPO**, especifique el límite del objetivo de punto de recuperación (RPO). Se genera una alerta si el RPO de replicación supera este límite.
4. En **Retención de punto de recuperación**, especifique la duración (en horas) del período de retención de cada punto de recuperación. Las máquinas virtuales replicadas se pueden recuperar a cualquier momento de un período. Se admite una retención de hasta 24 horas para máquinas replicadas en Premium Storage y 72 horas para almacenamiento estándar.
5. En **Frecuencia de las instantáneas coherentes con la aplicación**, especifique **Desactivado**. Haga clic en **Aceptar** para crear la directiva.

La directiva se asocia automáticamente al servidor de configuración.

> [!WARNING]
> Asegúrese de especificar **DESACTIVADO** en la configuración Frecuencia de las instantáneas coherentes con la aplicación de la directiva de replicación. Solo se admiten los puntos de recuperación coherentes con el bloqueo al replicar los servidores que ejecutan Windows Server 2008. Especificar cualquier otro valor para la Frecuencia de las instantáneas coherentes con la aplicación producirá alertas falsas volviendo el estado de replicación del servidor crítico debido a la falta de puntos de recuperación coherentes con la aplicación.

   ![Creación de la directiva de replicación](media/migrate-tutorial-windows-server-2008/create-policy.png)

## <a name="enable-replication"></a>Habilitar replicación

[Habilite la replicación](physical-azure-disaster-recovery.md#enable-replication) para el servidor de Windows Server 2008 SP2 o Windows Server 2008 R2 SP1 que se va a migrar.
   
   ![Agregue un servidor físico](media/migrate-tutorial-windows-server-2008/Add-physical-server.png)

   ![Habilitar replicación](media/migrate-tutorial-windows-server-2008/Enable-replication.png)

## <a name="run-a-test-migration"></a>Ejecutar una migración de prueba

Puede realizar una prueba de conmutación por error de replicación de servidores después de que se complete la replicación inicial y el estado del servidor pase a **Protegido**.

Ejecute una [conmutación por error de prueba](tutorial-dr-drill-azure.md) en Azure para asegurarse de que todo funciona de la forma esperada.

   ![Conmutación por error de prueba](media/migrate-tutorial-windows-server-2008/testfailover.png)


## <a name="migrate-to-azure"></a>Migración a Azure

Ejecute una conmutación por error para las máquinas que desea migrar.

1. En **Configuración** > **Elementos replicados**, haga clic en la máquina > **Conmutación por error**.
2. En **Conmutación por error**, seleccione un **punto de recuperación** en el que realizar la conmutación por error. Seleccione el punto de recuperación más reciente.
3. Seleccione **Apague la máquina antes de comenzar con la conmutación por error**. Después, Site Recovery intentará apagar el servidor antes de desencadenar la conmutación por error. La conmutación por error continúa aunque se produzca un error de cierre. Puede seguir el progreso de la conmutación por error en la página **Trabajos**.
4. Compruebe que la máquina virtual de Azure aparece en Azure según lo previsto.
5. En **Elementos replicados**, haga clic con el botón derecho en la máquina virtual > **Completar migración**. Con esta acción se completa el proceso de migración, y se detienen la replicación de la máquina virtual y la facturación de Site Recovery para la VM.

   ![Completar migración](media/migrate-tutorial-windows-server-2008/complete-migration.png)


> [!WARNING]
> **No cancele una conmutación por error en curso**: antes de iniciar la conmutación por error, se detiene la replicación de la máquina virtual. Si se cancela una conmutación por error en curso, la conmutación por error se detiene, pero no se replica la máquina virtual de nuevo.
