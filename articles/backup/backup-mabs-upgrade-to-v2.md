---
title: Instalar Azure Backup Server v2
description: Azure Backup Server v2 proporciona funciones mejoradas de copia de seguridad para proteger máquinas virtuales, archivos, carpetas, cargas de trabajo, etc. Obtenga información sobre cómo instalar Azure Backup Server v2 o actualizar a esta versión.
services: backup
author: markgalioto
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 05/15/2017
ms.author: adigan
ms.openlocfilehash: a458a46f3775a593f369d5acb967fc90d61efde8
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/08/2018
ms.locfileid: "39628348"
---
# <a name="install-azure-backup-server-v2"></a>Instalar Azure Backup Server v2

Azure Backup Server ayuda a proteger máquinas virtuales (VM), cargas de trabajo, archivos, carpetas, etc. Azure Backup Server v2 se basa en Azure Backup Server v1 y proporciona características nuevas que no están disponibles en la versión v1. Para obtener una comparación de las características de v1 y v2, consulte la [Matriz de protección de Azure Backup Server](backup-mabs-protection-matrix.md). 

Las características adicionales de Backup Server v2 son una actualización de Backup Server v1. A pesar de ello, no es un requisito previo disponer de Backup Server v1 para instalar Backup Server v2. Si quiere actualizar de Backup Server v1 a Backup Server v2, instale Backup Server v2 en el servidor de protección de Backup Server. La configuración existente de Backup Server permanecerá intacta.

Puede instalar Backup Server v2 en Windows Server 2016 o Windows Server 2012 R2. Para aprovechar características nuevas como Modern Backup Storage de System Center 2016 Data Protection Manager, debe instalar Backup Server v2 en Windows Server 2016. Antes de instalar Backup Server v2 o actualizar a esta versión, lea este artículo sobre los [requisitos previos de instalación](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites).

> [!NOTE]
> Azure Backup Server tiene la misma base de código que System Center Data Protection Manager. Backup Server v1 es equivalente a Data Protection Manager 2012 R2. Backup Server v2 es equivalente a Data Protection Manager 2016. En ocasiones, en este artículo se hace referencia a la documentación de Data Protection Manager.
>
>

## <a name="upgrade-backup-server-to-v2"></a>Actualizar Backup Server a la versión v2
Para actualizar de Backup Server v1 a Backup Server v2, asegúrese de que la instalación tiene las actualizaciones necesarias:

- [Actualice los agentes de protección](backup-mabs-upgrade-to-v2.md#update-the-data-protection-manager-protection-agent) de los servidores protegidos.
- Actualice Windows Server 2012 R2 a Windows Server 2016.
- Actualice Remote Administrator de Azure Backup Server en todos los servidores de producción.
- Asegúrese de que las copias de seguridad están establecidas para continuar sin necesidad de reiniciar el servidor de producción.


### <a name="upgrade-steps-for-backup-server-v2"></a>Pasos para actualizar a Backup Server v2

1. En el Centro de descarga, [descargue el instalador de actualización](https://go.microsoft.com/fwlink/?LinkId=626082).

2. Después de extraer el Asistente para instalación, asegúrese de que está seleccionado **Execute setup.exe** (Ejecutar setup.exe) y, después, seleccione **Finalizar**.

  ![Instalador: ejecutar el programa de instalación](./media/backup-mabs-upgrade-to-v2/run-setup.png)

3. En el Asistente para Microsoft Azure Backup Server, en **Instalar**, seleccione **Microsoft Azure Backup Server**.

  ![Instalador: seleccionar la instalación](./media/backup-mabs-upgrade-to-v2/mabs-installer-s1.png)

4. En la **página principal**, revise las advertencias y seleccione **Siguiente**.

  ![Instalador: página principal](./media/backup-mabs-upgrade-to-v2/mabs-installer-s2.png)

5. El Asistente para instalación comprueba una serie de requisitos previos para asegurarse de que se puede actualizar el entorno. En la página **Comprobaciones de requisitos previos**, seleccione **Comprobar**.

  ![Instalador: página Comprobaciones de requisitos previos](./media/backup-mabs-upgrade-to-v2/mabs-installer-s3-perform-checks.png)

6. El entorno debe superar las comprobaciones de requisitos previos. Si no las supera, tome nota de los problemas y corríjalos. Después, seleccione **Comprobar de nuevo**. Una vez que haya superado las comprobaciones de requisitos previos, seleccione **Siguiente**.

  ![Instalador: botón Comprobar de nuevo](./media/backup-mabs-upgrade-to-v2/mabs-installer-s4-pass-checks.png)

7. En la página **Configuración de SQL**, seleccione la opción correspondiente a su instalación de SQL y, después, seleccione **Comprobar e instalar**.

  ![Instalador: página Configuración de SQL](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5-sql-settings.png)

  Estas comprobaciones pueden tardar unos minutos. Cuando las comprobaciones hayan terminado, seleccione **Siguiente**.

  ![Instalador: botón Comprobar e instalar en Configuración de SQL](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5a-check-and-fix-settings.png)

8. En la página **Configuración de la instalación**, cambie la ubicación donde se instala Backup Server o la ubicación temporal. Seleccione **Next** (Siguiente).

  ![Instalador: página Configuración de la instalación](./media/backup-mabs-upgrade-to-v2/mabs-installer-s6-installation-settings.png)

9. Para finalizar el Asistente para instalación, seleccione **Finalizar**.

  ![Instalador: finalizar](./media/backup-mabs-upgrade-to-v2/run-setup.png)

## <a name="add-storage-for-modern-backup-storage"></a>Agregar almacenamiento para Modern Backup Storage

Para mejorar la eficacia del almacenamiento de copias de seguridad, Backup Server v2 es compatible con volúmenes. Backup Server v1 y Backup Server v2 admiten discos.

### <a name="add-volumes-and-disks"></a>Agregar volúmenes y discos

Si ejecuta Backup Server v2 en Windows Server 2016, puede usar volúmenes para almacenar datos de copia de seguridad. Los volúmenes permiten ahorrar almacenamiento y realizar copias de seguridad más rápido. Dado que los volúmenes son una novedad de Backup Server, debe agregarlos. 

Al agregar un volumen a Backup Server, puede asignarle un nombre descriptivo. Seleccione la columna **Nombre descriptivo** del volumen al que quiere asignarle un nombre. Puede cambiar el nombre posteriormente, si es necesario. También puede usar PowerShell para agregar nombres descriptivos a los volúmenes o cambiarlos.

Para agregar un volumen en la consola de administrador:

1. En la consola de administrador de servidor de Azure Backup, seleccione **Administración** > **Almacenamiento en disco** > **Agregar**.

  ![Abrir el Asistente para agregar almacenamiento en disco](./media//backup-mabs-upgrade-to-v2/open-add-disk-storage-wizard.png)

  El Asistente para agregar almacenamiento en disco se abre.

2. En la página **Agregar almacenamiento en disco**, en el cuadro **Volúmenes disponibles**, seleccione un volumen y haga clic en **Agregar**.

3. En el cuadro **Volúmenes seleccionados**, escriba un nombre descriptivo para el volumen y seleccione **Aceptar**.

  ![Asistente para agregar almacenamiento en disco: agregar volumen](./media/backup-mabs-upgrade-to-v2/add-volume.png)

  Si quiere agregar un disco, debe pertenecer a un grupo de protección que tenga almacenamiento heredado. Solo puede usar estos discos para estos grupos de protección. Si Backup Server no tiene orígenes con protección heredada, el disco no aparecerá.

  Para más información sobre cómo agregar discos, consulte [Incorporación de discos para aumentar el almacenamiento heredado](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage). No se puede asignar un nombre descriptivo a un disco.


### <a name="assign-workloads-to-volumes"></a>Asignar cargas de trabajo a volúmenes

En Backup Server, puede especificar qué cargas de trabajo se asignan a cada volumen. Por ejemplo, puede establecer volúmenes costosos que admiten un gran número de operaciones de entrada/salida por segundo (IOPS) para almacenar solo las cargas de trabajo que requieren copias de seguridad frecuentes y de gran volumen. Un ejemplo es SQL Server con registros de transacciones.

#### <a name="update-dpmdiskstorage"></a>Update-DPMDiskStorage

Para actualizar las propiedades de un volumen en el grupo de almacenamiento de Backup Server, use el cmdlet de PowerShell **Update-DPMDiskStorage**.

Sintaxis:

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [<CommonParameters>]
```

Todos los cambios que realice mediante PowerShell se reflejarán en la interfaz de usuario.


## <a name="protect-data-sources"></a>Proteger orígenes de datos
Para empezar a proteger los orígenes de datos, cree un grupo de protección. En los pasos siguientes se muestran cambios o adiciones en el Asistente para nuevo grupo de protección.

Para crear un grupo de protección:

1. En la consola de administrador de Backup Server, seleccione **Protección**.

2. En la cinta de herramientas, seleccione **Nuevo**.

  Se abre el asistente para crear nuevo grupo de protección.

  ![Asistente para crear nuevo grupo de protección](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-1.png)

3. En la página **principal**, seleccione **Siguiente**.

4. En la página **Seleccionar tipo de grupo de protección**, elija el tipo de grupo de protección que quiere crear y haga clic en **Siguiente**.

  ![Página Seleccionar tipo de grupo de protección](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-2.png)

5. En la página **Seleccionar miembros del grupo**, en el cuadro **Miembros disponibles**, se muestran los miembros con agentes de protección. Para este ejemplo, seleccione los volúmenes D:\ y E:\,, y agréguelos a **Miembros seleccionados**. Seleccione **Next** (Siguiente).

  ![Página Seleccionar miembros del grupo](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-3.png)

6. En la página **Seleccionar método de protección de datos**, escriba el **Nombre del grupo de protección**, seleccione el método de protección y haga clic en **Siguiente**. Si quiere protección a corto plazo, seleccione el método de copia de seguridad **Disco**.

  ![Página Seleccionar método de protección de datos](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-4.png)

7. En la página **Especificar objetivos a corto plazo**, seleccione los detalles de **Duración de retención** y **Frecuencia de sincronización**. Después, seleccione **Siguiente**. Opcionalmente, para cambiar la programación en la que se toman los puntos de recuperación, seleccione **Modificar**.

  ![Página Especificar objetivos a corto plazo](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-5.png)

8. En la página **Revisar la asignación del almacenamiento en disco**, revise los detalles de los orígenes de datos que ha seleccionado, su tamaño y los valores del espacio que se aprovisionará y del volumen de almacenamiento de destino.

  ![Página Revisar la asignación del almacenamiento en disco](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-6.png)

  Los volúmenes de almacenamiento se basan en la asignación de volumen de carga de trabajo (que se establece mediante PowerShell) y el almacenamiento disponible. Para cambiar los volúmenes de almacenamiento, seleccione otros volúmenes en el menú desplegable. Si cambia el valor de **Almacenamiento de destino**, el valor de **Almacenamiento en disco disponible** cambiará dinámicamente para reflejar los valores de **Espacio disponible** y **Underprovisioned Space** (Espacio insuficiente).

  Si los orígenes de datos aumentan según lo previsto, el valor de la columna **Underprovisioned Space** (Espacio insuficiente) de **Almacenamiento en disco disponible** refleja la cantidad de almacenamiento adicional que se necesita. Use este valor como ayuda a la hora de planear las necesidades de almacenamiento para realizar copias de seguridad sin problemas. Si el valor es cero, no se prevén problemas relacionados con el almacenamiento en un futuro inmediato. Si el valor es distinto de cero, no tiene suficiente espacio de almacenamiento asignado (según la directiva de protección y el tamaño de los datos de los miembros protegidos).

  ![Almacenamiento en disco subasignado](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-7.png)

  Para acabar de crear el grupo de protección, complete el asistente.

## <a name="migrate-legacy-storage-to-modern-backup-storage"></a>Migrar el almacenamiento heredado a Modern Backup Storage
Después de instalar Backup Server v2 o actualizar a esta versión, y a continuación actualizar el sistema operativo a Windows Server 2016, actualice los grupos de protección para que usen Modern Backup Storage. De forma predeterminada, los grupos de protección no se cambian. Siguen funcionando tal como se han configurado inicialmente. 

La actualización de los grupos de protección para que usen Modern Backup Storage es opcional. Para actualizar el grupo de protección, detenga la protección de todos los orígenes de datos mediante la opción de conservar datos. Después, agregue los orígenes de datos a un nuevo grupo de protección.

1. En la Consola de administrador de System Center 2016 DPM, seleccione la característica **Protección**. En la lista **Miembro del grupo de protección**, haga clic con el botón derecho en el miembro y seleccione **Detener protección de miembro**.

  ![Detener protección de miembro](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. En el cuadro de diálogo **Quitar del grupo**, revise el espacio en disco usado y el espacio disponible para el grupo de almacenamiento. El valor predeterminado es dejar los puntos de recuperación en el disco y permitirles expirar según su directiva de retención asociada. Seleccione **Aceptar**.

  Si quiere devolver de inmediato el espacio en disco usado al grupo de almacenamiento libre, active la casilla **Eliminar réplica en disco** para eliminar los datos de copia de seguridad (y los puntos de recuperación) asociados a ese miembro.

  ![Cuadro de diálogo Quitar del grupo](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. Cree un grupo de protección que use Modern Backup Storage. Incluya los orígenes de datos no protegidos.


## <a name="add-disks-to-increase-legacy-storage"></a>Agregar discos para aumentar el almacenamiento heredado

Si quiere usar el almacenamiento heredado con Backup Server, es posible que tenga que agregar discos para aumentar el almacenamiento heredado. 

Para agregar almacenamiento en disco:

1. En la Consola de administrador de System Center 2016 DPM, seleccione **Administración** > **Disk Storage** > **Agregar**.

  ![Cuadro de diálogo Agregar almacenamiento en disco](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

2. En el cuadro de diálogo **Agregar almacenamiento en disco**, seleccione **Agregar discos**.

3. En la lista de discos disponibles, seleccione los discos que quiera agregar. Seleccione **Agregar** y luego **Aceptar**.

## <a name="update-the-data-protection-manager-protection-agent"></a>Actualizar el agente de protección de Data Protection Manager

Backup Server usa el agente de protección de System Center Data Protection Manager para las actualizaciones. Si va a actualizar un agente de protección que no está conectado a la red, no puede usar la Consola de administrador de Data Protection Manager para completar una actualización del agente conectado. Debe actualizar el agente de protección en un entorno de dominio que no esté activo. Mientras el equipo cliente no se conecte a la red, la actualización del agente de protección aparecerá como pendiente en la consola de administrador de Data Protection Manager.

En las secciones siguientes se describe cómo se actualizan los agentes de protección de equipos cliente conectados y no conectados.

### <a name="update-a-protection-agent-for-a-connected-client-computer"></a>Actualizar un agente de protección de un equipo cliente conectado

1. En la consola de administrador de Backup Server, seleccione **Administración** > **Agentes**.

2. En el panel de información, seleccione los equipos cliente cuyo agente de protección quiere actualizar.

  > [!NOTE]
  > En la columna **Actualizaciones del agente** se indica cuándo está disponible una actualización del agente de protección para cada equipo protegido. En el panel **Acciones**, la acción **Actualización** solo está disponible cuando se selecciona un equipo protegido y las actualizaciones están disponibles.

3. Para instalar agentes de protección actualizados en los equipos seleccionados, en el panel **Acciones**, seleccione **Actualizar**.

### <a name="update-a-protection-agent-on-a-client-computer-that-is-not-connected"></a>Actualizar un agente de protección en un equipo cliente que no está conectado

1. En la consola de administrador de Backup Server, seleccione **Administración** > **Agentes**.

2. En el panel de información, seleccione los equipos cliente cuyo agente de protección quiere actualizar.

  > [!NOTE]
  > En la columna **Actualizaciones del agente** se indica cuándo está disponible una actualización del agente de protección para cada equipo protegido. En el panel **Acciones**, la acción **Actualización** no está disponible cuando se selecciona un equipo protegido, a menos que haya actualizaciones disponibles.

3. Para instalar agentes de protección actualizados en los equipos seleccionados, haga clic en **Actualizar**.

4. Si un equipo cliente no está conectado a la red, aparecerá el estado **Actualización pendiente** en la columna **Estado del agente** mientras el equipo no se conecte a la red.

  Una vez que el equipo cliente se conecte a la red, aparecerá el estado **Actualizando** en la columna **Actualizaciones del agente** para el equipo cliente.
  
### <a name="move-legacy-protection-groups-from-the-old-version-and-sync-the-new-version-with-azure"></a>Paso de los grupos de protección heredados de una versión antigua y sincronización de la nueva versión con Azure

Una vez que Azure Backup Server y el sistema operativo están ambos actualizados, está preparado para proteger los nuevos orígenes de datos con Modern Backup Storage. Los orígenes de datos que ya están protegidos siguen estándolo tal como estaban en Azure Backup Server (antiguo). Todos los grupos de protección nuevos usan Modern Backup Storage.

Para migrar los orígenes de datos del modo antiguo de protección a Modern Backup Storage:

1.  Agregue los volúmenes nuevos al bloque de almacenamiento de Data Protection Manager. También puede asignar un nombre descriptivo y seleccionar las etiquetas del origen de datos.

2. Para cada origen de datos que se encuentre en el modo antiguo, detenga la protección de los orígenes de datos. A continuación, seleccione **Conservar datos protegidos**.  Así permitirá la recuperación de los puntos de recuperación antiguos después de la migración.

3. Cree un nuevo grupo de protección. A continuación, seleccione los orígenes de datos que desee almacenar mediante el nuevo formato.

  Data Protection Manager almacena localmente una copia de la réplica del almacenamiento de copia de seguridad antiguo en el volumen de Modern Backup Storage.
    > [!NOTE] 
    > La creación de la copia aparece como un trabajo de operación posterior a la recuperación.

  Todos los nuevos puntos de recuperación y sincronización se almacenan entonces en Modern Backup Storage.

  Los puntos de recuperación antiguos se eliminan cuando expiran. A medida que los puntos de recuperación antiguos se eliminan, se libera espacio en disco.

  Cuando se eliminan todos los volúmenes heredados del almacenamiento antiguo, puede quitar el disco de Azure Backup. A continuación, puede quitar el disco del sistema.

4. Realice una copia de seguridad de la base de datos de Data Protection Manager.

  > [!IMPORTANT]
  > - El nombre nuevo del servidor debe ser el mismo que el de la instancia original de Azure Backup Server. No puede cambiar el nombre de la nueva instancia de Azure Backup Server si desea usar el anterior bloque de almacenamiento y base de datos de Data Protection Manager para conservar los puntos de recuperación.
  > - Debe realizar una copia de seguridad de la base de datos de Data Protection Manager. Tendrá que restaurar la base de datos.

5. Cierre la instancia original de Azure Backup Server o desconecte el servidor.

6. Restablezca la cuenta de la máquina en Active Directory.

7. Instale Windows Server 2016 en una máquina nueva. Como nuevo nombre del servidor, use el mismo que el de la instancia original de Azure Backup Server.

8. Únase al dominio.

9. Instale Azure Backup Server v2. (Quite los discos del bloque de almacenamiento de Data Protection Manager del servidor anterior e impórtelos en el nuevo servidor).

10. Restaure la base de datos de Data Protection Manager que creó en el paso 4.

11. Conecte el almacenamiento del servidor de copia de seguridad original al nuevo servidor.

12. En SQL Server, restaure la base de datos de Data Protection Manager.

13. En la línea de comandos de administrador en el nuevo servidor, utilice `cd` para ir a la carpeta de almacén y a la ubicación de instalación de Microsoft Azure Backup.  

  Ejemplo:  
  C:\windows\system32>cd "c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\ para Azure Backup

14. Ejecute `DPMSYNC -SYNC`.
  
  > [!NOTE]
  > Si agregó discos *nuevos* al bloque de almacenamiento de Data Protection Manager en lugar de mover los antiguos, ejecute `DPMSYNC -Reallocatereplica`.

## <a name="new-powershell-cmdlets-in-backup-server-v2"></a>Nuevos cmdlets de PowerShell en Backup Server v2

Cuando se instala Azure Backup Server v2, hay dos nuevos cmdlets disponibles: 
* [Mount-DPMRecoveryPoint](https://technet.microsoft.com/library/mt787159.aspx)
* [Dismount-DPMRecoveryPoint](https://technet.microsoft.com/library/mt787158.aspx)

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre cómo preparar el servidor o empezar a proteger la carga de trabajo:
- [Preparar cargas de trabajo de Backup Server](backup-azure-microsoft-azure-backup.md)
- [Usar Backup Server para hacer una copia de seguridad de un servidor de VMware](backup-azure-backup-server-vmware.md)
- [Usar Backup Server para hacer una copia de seguridad de SQL Server](backup-azure-sql-mabs.md)
- [Usar Modern Backup Storage con Backup Server](backup-mabs-add-storage.md)

