---
title: Realizar una copia de seguridad de máquinas de Windows con el agente de MARS de copia de seguridad de Azure
description: Use el agente de copia de seguridad de Azure de Microsoft Recovery Services (MARS) para realizar una copia de seguridad de máquinas de Windows.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: raynew
ms.openlocfilehash: 4aff7c486762d9ea7bd38ae152d169968432307e
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/18/2019
ms.locfileid: "57898965"
---
# <a name="back-up-windows-machines-with-the-azure-backup-mars-agent"></a>Realizar una copia de seguridad de máquinas de Windows con el agente de MARS de copia de seguridad de Azure

En este artículo se explica cómo realizar una copia de seguridad de máquinas de Windows mediante el [Azure Backup](backup-overview.md) servicio y el agente de Microsoft Azure Recovery Services (MARS), también conocido como el agente de copia de seguridad de Azure.

En este artículo, aprenderá a:

> [!div class="checklist"]
> * Compruebe los requisitos previos y crear un almacén de Recovery Services.
> * Descargar y configurar el agente de MARS
> * Crear una directiva de copia de seguridad y programación.
> * Copia de seguridad, realizar un ad-hoc.

## <a name="about-the-mars-agent"></a>Acerca del agente de MARS

El agente de MARS se utiliza Azure backup para realizar copias de seguridad del estado del sistema, carpetas y archivos de máquinas locales y máquinas virtuales de Azure en un almacén de Recovery Services copia de seguridad de Azure. Puede ejecutar al agente como sigue:

- Ejecute al agente directamente en las máquinas de Windows local para que puede realizar copias de seguridad directamente en un almacén de Recovery Services copia de seguridad en Azure.
- Ejecute las máquinas virtuales de Azure de agente ejecuta Windows (side-by-side con la extensión de copia de seguridad de máquina virtual de Azure) para realizar una copia de seguridad de determinados archivos y carpetas en la máquina virtual.
- Ejecute el agente en un servidor de Microsoft Azure Backup Server (MABS) o en un servidor de System Center Data Protection Manager (DPM). En este escenario, las máquinas y cargas de trabajo de copia de seguridad en DPM/MABS y, a continuación, MABS/DPM realiza copias de seguridad en un almacén de Azure mediante el agente de MARS.
Aquello de lo que puede realizar una copia de seguridad depende de dónde está instalado el agente.

> [!NOTE]
> El método principal para la copia de seguridad de máquinas virtuales de Azure es mediante una extensión de copia de seguridad de Azure en la máquina virtual. De esta manera, se realiza la copia de seguridad de toda la máquina virtual. Es posible que desee instalar y usar al agente de MARS junto con la extensión si desea realizar una copia de seguridad de determinados archivos y carpetas en la máquina virtual. [Más información](backup-architecture.md#architecture-direct-backup-of-azure-vms).

![Pasos del proceso de copia de seguridad](./media/backup-configure-vault/initial-backup-process.png)

## <a name="before-you-start"></a>Antes de comenzar

- [Obtenga información sobre cómo](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders) Azure Backup realiza copias de seguridad de máquinas de Windows con el agente de MARS.
- [Obtenga información sobre](backup-architecture.md#architecture-back-up-to-dpmmabs) la arquitectura de copia de seguridad que se ejecuta el agente de MARS en un servidor MABS o DPM secundario.
- [Revisión](backup-support-matrix-mars-agent.md) ¿qué es compatible y qué copia de seguridad con el agente de MARS.
- Compruebe el acceso a internet en las máquinas que desee realizar copias de seguridad.
- Si desea crear una copia de seguridad de un servidor o cliente en Azure, necesita una cuenta de Azure. En caso de no tener ninguna, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/) en tan solo unos minutos.

### <a name="verify-internet-access"></a>Comprobación del acceso a Internet

Si el equipo ha limitado el acceso a internet, asegúrese de que la configuración de firewall en la máquina o proxy permite estas direcciones URL:

- www\.msftncsi.com
- *.Microsoft.com
- *.WindowsAzure.com
- *.microsoftonline.com
- * .windows.net

## <a name="create-a-recovery-services-vault"></a>Creación de un almacén de Recovery Services

Un almacén de Recovery Services almacena todas las copias de seguridad y los puntos de recuperación creados con el tiempo y contiene la directiva de copia de seguridad que aplica a equipos de copiados de seguridad. Cree un almacén como se indica a continuación:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) mediante la suscripción de Azure.
2. En la búsqueda, escriba **Recovery Services** y haga clic en **almacenes de Recovery Services**.

    ![Creación del almacén de Recovery Services, paso 1](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png)

3. En el **almacenes de Recovery Services** menú, haga clic en **+ agregar**.

    ![Creación del almacén de Recovery Services, paso 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

4. En **Nombre**, escriba un nombre descriptivo que identifique el almacén.

   - El nombre debe ser único para la suscripción de Azure.
   - Puede contener entre 2 y 50 caracteres.
   - Debe comenzar por una letra y solo puede contener letras, números y guiones.

5. Seleccione la suscripción de Azure, el grupo de recursos y la región geográfica en la que se debe crear el almacén. Datos de copia de seguridad se envían en el almacén. A continuación, haga clic en **Crear**.

    ![Creación del almacén de Recovery Services, paso 3](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

   - La creación del almacén puede tardar un tiempo.
   - Supervise las notificaciones del estado en la parte superior derecha del portal. Si no ve el almacén después de unos minutos, haga clic en **actualizar**.

     ![Clic en el botón Refresh (Actualizar)](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)

### <a name="set-storage-redundancy"></a>Establecimiento de la redundancia de almacenamiento

Copia de seguridad de Azure controla automáticamente el almacenamiento para el almacén. Debe especificar cómo se replica los que el almacenamiento.

1. En la hoja **Almacenes de Recovery Services**, haga clic en el almacén nuevo. En el **configuración** sección, haga clic en **propiedades**.
2. En **propiedades**, en **configuración de copia de seguridad**, haga clic en **actualización**.

3. Seleccione el tipo de replicación de almacenamiento y haga clic en **guardar**.

      ![Establecimiento de la configuración de almacenamiento del nuevo almacén](./media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

- Se recomienda que si usa Azure como un punto de conexión de almacenamiento de copia de seguridad principal, seguir usando el valor predeterminado **con redundancia geográfica** configuración.
- Si no utiliza Azure como punto de conexión de almacenamiento de copia de seguridad principal, elija **Redundancia local** para reducir los costes de almacenamiento de Azure.
- Obtenga más información sobre [geográfica](../storage/common/storage-redundancy-grs.md) y [local](../storage/common/storage-redundancy-lrs.md) redundancia.

## <a name="download-the-mars-agent"></a>Descargue al agente de MARS

Descargue al agente de MARS para su instalación en máquinas que desea realizar copias de seguridad.

- Si ya ha instalado al agente en todos los equipos, asegúrese de que está ejecutando la versión más reciente.
- La versión más reciente está disponible en el portal o mediante un [descarga directa](https://aka.ms/azurebackup_agent)

1. En el almacén, en **Introducción**, haga clic en **copia de seguridad**.

    ![Abrir hoja de objetivo de copia de seguridad](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

2. En **donde se ejecuta la carga de trabajo?**, seleccione **local**. Debe seleccionar esta opción, incluso si desea instalar al agente de MARS en una máquina virtual de Azure.
3. En **¿en qué se desea hacer una copia de seguridad?**, seleccione **archivos y carpetas** o **del estado del sistema**. Hay una serie de otras opciones disponibles, pero solo se admiten si ejecuta un servidor secundario copia de seguridad. Haga clic en **preparar infraestructura**.

      ![Configuración de archivos y carpetas](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

4. En el **preparar infraestructura**, en **agente de instalación de Recovery Services**, descargue el agente de MARS.

    ![Prepare infrastructure](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

5. Haga clic en **Guardar** en el menú emergente de descarga. De forma predeterminada, se guarda el archivo **MARSagentinstaller.exe** en la carpeta de descargas.

6. Ahora, compruebe **ya descarga o mediante el agente de servicios de recuperación más reciente**y, a continuación, descargue las credenciales del almacén.

    ![descargar las credenciales de almacén](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

7. Haga clic en **Save**(Guardar). El archivo se descarga en la carpeta de descarga. No se puede abrir el archivo de credenciales de almacén.

## <a name="install-and-register-the-agent"></a>Instalación y registro del agente

1. Ejecute el **MARSagentinstaller.exe** archivos en máquinas que desee realizar copias de seguridad.
2. En el Asistente para la instalación del agente de MARS > **configuración de la instalación**, especifique dónde desea instalar el agente y una ubicación que se usará para la memoria caché. A continuación, haga clic en **Siguiente**.
   - Azure Backup utiliza la memoria caché para almacenar las instantáneas de datos antes de enviarlos a Azure.
   - La ubicación de caché debe tener espacio libre al menos el 5% del tamaño de los datos que se hará la copia de seguridad.

     ![Configuración de instalación del Asistente de MARS](./media/backup-configure-vault/mars1.png)

2. En **configuración de Proxy**, especifique cómo el agente que se ejecuta en el equipo de Windows se conectará a internet. A continuación, haga clic en **Siguiente**.

   - Si usas un personalizado proxy especificar la configuración de proxy y las credenciales si es necesario.
   - Recuerde que el agente necesita acceso a [estas direcciones URL](#verify-internet-access).

     ![Acceso a internet de Asistente de MARS](./media/backup-configure-vault/mars2.png)

3. En **instalación** revisar la comprobación de requisitos previos y haga clic en **instalar**.
4. Después de instalar el agente, haga clic en **proceder al registro**.
5. En el **Asistente para registrar servidor** > **identificación del almacén**, busque y seleccione el archivo de credenciales que descargó. A continuación, haga clic en **Siguiente**.

    ![Registro - las credenciales de almacén](./media/backup-configure-vault/register1.png)

6. En **Encryption Setting**, especifique una frase de contraseña que se usará para cifrar y descifrar las copias de seguridad de la máquina.

    - Guarde la frase de contraseña en una ubicación segura.
    - Si pierde u olvida la frase de contraseña, Microsoft no puede ayudar a recuperar los datos de copia de seguridad. Guarde el archivo en una ubicación segura. Lo necesitará para restaurar una copia de seguridad.

7. Haga clic en **finalizar**. Ahora está instalado el agente y el equipo está registrado en el almacén. Está listo para configurar y programar la copia de seguridad.

## <a name="create-a-backup-policy"></a>Creación de una directiva de copia de seguridad

La directiva de copia de seguridad especifica cuándo se debe tomar instantáneas de los datos para crear puntos de recuperación y cuánto tiempo desea conservar puntos de recuperación.

- Configurar una directiva de copia de seguridad mediante el agente de MARS.
- Copia de seguridad de Azure no tiene automáticamente horario de verano (DST) en cuenta. Esto podría producir discrepancias entre la hora real y la hora de copia de seguridad programada.

Cree una directiva como sigue:

1. En cada máquina, abra al agente de MARS. Para encontrarlo, busque **Microsoft Azure Backup**en la máquina.
2. En **acciones**, haga clic en **programar copia de seguridad**.

    ![Programar una copia de seguridad de Windows Server](./media/backup-configure-vault/schedule-first-backup.png)

3. En el Asistente para copia de seguridad de programación > **Introducción**, haga clic en **siguiente**.
4. En **seleccionar elementos de copia de seguridad**, haga clic en **agregar elementos**.
5. En **seleccionar elementos**, seleccione qué desea realizar copias de seguridad. A continuación, haga clic en **Aceptar**.
6. En **seleccionar elementos de copia de seguridad** página, haga clic en **siguiente**.
7. En **especificar una programación de copia de seguridad** página, especifique cuándo desea realizar copias de seguridad diarias o semanales. A continuación, haga clic en **Siguiente**.

    - Cuando se realiza una copia de seguridad, se crea un punto de recuperación.
    - El número de puntos de recuperación creados en su entorno depende de la programación de copia de seguridad.

1. Puede programar copias de seguridad diarias, hasta tres veces al día. Por ejemplo, la captura de pantalla muestra dos copias de seguridad diarias, uno a medianoche y otro a las 6 p.m.

    ![Programación diaria](./media/backup-configure-vault/day-schedule.png)

9. Puede ejecutar copias de seguridad semanales demasiado. Por ejemplo, la captura de pantalla muestra las copias de seguridad que se realizan cada el domingo y el miércoles alternativas a las 9:30 A.M. y 1:00 AM.

    ![Programación semanal](./media/backup-configure-vault/week-schedule.png)

8. En el **Seleccionar directiva de retención** , especifique cómo almacenar copias históricas de los datos. A continuación, haga clic en **Siguiente**.

   - Configuración de retención especifica qué puntos de recuperación se deben almacenar y cuánto tiempo deben almacenarse para.
   - Por ejemplo, al establecer una configuración de retención diaria, indican que a la hora especificada para la retención diaria, se conservará el último punto de recuperación para el número de días especificado. O bien, como otro ejemplo, puede especificar una directiva de retención mensual para indicar que se debe almacenar el punto de recuperación creado en el 30 de cada mes durante 12 meses.
   - Retención de punto de recuperación diarios y semanales normalmente coincide con la programación de copia de seguridad. Lo que significa que cuando se desencadena la copia de seguridad según la programación, se almacena el punto de recuperación creado por la copia de seguridad para la duración que se indica en la directiva de retención diaria o semanal.
   - Por ejemplo, en la siguiente captura de pantalla:
     - Copias de seguridad diarias a medianoche y 6 p. M. se conservan durante siete días.
     - Copias de seguridad realizadas en sábado a medianoche y 6 p. M. se conservan durante 4 semanas.
     - Copias de seguridad realizadas el sábado en la última semana del mes a medianoche y 6 p. M. se conservan durante 12 meses. -Copias de seguridad realizadas en sábado en la última semana de marzo se conservan durante 10 años.

   ![Ejemplo de retención](./media/backup-configure-vault/retention-example.png)

11. En **elegir un tipo de copia de seguridad inicial** especificar cómo realizar copia de seguridad, la inicial a través de la red o sin conexión. A continuación, haga clic en **Siguiente**.

10. En **confirmación**, revise la información y, a continuación, haga clic en **finalizar**.
11. Cuando el asistente termine de crear la programación de copia de seguridad, haga clic en **Cerrar**.

### <a name="perform-the-initial-backup-offline"></a>Realizar la copia de seguridad inicial sin conexión

Puede ejecutar automáticamente un inicial copia de seguridad a través de la red o sin conexión. Es útil si tiene grandes cantidades de datos que requieren una gran cantidad de ancho de banda de red para transferir la propagación sin conexión para una copia de seguridad inicial. Realice a una transferencia sin conexión como se indica a continuación:

1. Escribir los datos de copia de seguridad en una ubicación de ensayo.
2. Utilice la herramienta AzureOfflineBackupDiskPrep para copiar los datos de la ubicación de ensayo a uno o más discos SATA.
3. La herramienta crea un trabajo de importación de Azure. [Obtenga más información](https://docs.microsoft.com/azure/storage/common/storage-import-export-service) sobre Azure importación y exportación.
4. Enviar los discos SATA a un centro de datos de Azure.
5. En el centro de datos, los datos del disco se copian a una cuenta de almacenamiento de Azure.
6. Azure Backup copia los datos de la cuenta de almacenamiento en el almacén, y se programan copias de seguridad incrementales.

[Obtenga más información](backup-azure-backup-import-export.md) sobre propagación sin conexión.

### <a name="enable-network-throttling"></a>Habilitación de la limitación de la red

Puede controlar cómo se utiliza el ancho de banda de red mediante el agente de MARS habilitando la limitación de la red. Limitación es útil si necesita realizar una copia de seguridad de los datos durante las horas de trabajo, pero desea controlar cuánto ancho de banda que se utiliza para la copia de seguridad y restaurar la actividad.

- Red de copia de seguridad de Azure limitación usa [calidad de servicio (QoS)](https://docs.microsoft.com/windows-server/networking/technologies/qos/qos-policy-top) en el sistema operativo local.
- Limitación de copia de seguridad de red está disponible en Windows Server 2008 R2 y versiones posteriores y Windows 7 y versiones posteriores. Los sistemas operativos deben ejecutar los service packs más recientes.

Habilitar el límite de red como sigue:

1. En el agente de MARS, haga clic en **cambiar las propiedades**.
2. En el **limitación** pestaña **Habilitar límite para las operaciones de copia de seguridad de uso de ancho de banda de internet**.

    ![Limitación de la red](./media/backup-configure-vault/throttling-dialog.png)
3. Especifique el ancho de banda permitido durante el trabajo y fuera del horario de trabajo. Los valores de ancho de banda comienzan en 512 Kbps y hasta 1023 MBps. A continuación, haga clic en **Aceptar**.

## <a name="run-an-ad-hoc-backup"></a>Ejecutar una copia de seguridad ad hoc

1. En el agente de MARS, haga clic en **Back Up Now**. Esto inicia la replicación inicial a través de la red.

    ![Realizar copia de seguridad de Windows Server ahora](./media/backup-configure-vault/backup-now.png)

2. En **confirmación**, revise la configuración y haga clic en **copia de seguridad**.
3. Haga clic en **Cerrar** para cerrar el asistente. Si lo hace antes de que finalice la copia de seguridad, el asistente continúa ejecutándose en segundo plano.

Una vez que finalice la copia de seguridad inicial, el estado **Trabajo completado** se refleja en la consola de Copia de seguridad.

## <a name="next-steps"></a>Pasos siguientes

[Obtenga información sobre cómo](backup-azure-restore-windows-server.md) restaurar archivos.
