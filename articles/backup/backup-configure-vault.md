---
title: Copia de seguridad de máquinas Windows con el agente de MARS
description: Use el agente de Microsoft Recovery Services (MARS) de Azure Backup para realizar copias de seguridad de máquinas Windows.
ms.topic: conceptual
ms.date: 06/04/2019
ms.openlocfilehash: 990929cc95d1c25117873ca39415d33370456b91
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025544"
---
# <a name="back-up-windows-machines-with-the-azure-backup-mars-agent"></a>Copia de seguridad de máquinas Windows con el agente de MARS de Azure Backup

En este artículo se explica cómo realizar copias de seguridad de máquinas Windows mediante el servicio [Azure Backup](backup-overview.md) y el agente de Microsoft Azure Recovery Services (MARS), también conocido como agente de Azure Backup.

En este artículo, aprenderá a:

> [!div class="checklist"]
>
> * Comprobar los requisitos previos y crear un almacén de Recovery Services
> * Descargar y configurar el agente de MARS
> * Crear una directiva y programación de copia de seguridad
> * Realizar una copia de seguridad a petición

## <a name="about-the-mars-agent"></a>Acerca del agente de MARS

Azure Backup utiliza el agente de MARS para realizar copias de seguridad de archivos, carpetas y el estado del sistema de máquinas locales y máquinas virtuales de Azure en un almacén de Recovery Services de copia de seguridad en Azure. Puede ejecutar el agente de la forma siguiente:

* Ejecute el agente directamente en máquinas Windows locales, para que puedan realizar la copia de seguridad directamente en un almacén de Recovery Services de copia de seguridad en Azure.
* Ejecute el agente en máquinas virtuales de Azure con Windows (en paralelo con la extensión de copia de seguridad de máquina virtual de Azure) para realizar copias de seguridad de archivos y carpetas específicos de la máquina virtual.
* Ejecute el agente en un servidor de Microsoft Azure Backup Server (MABS) o en un servidor de System Center Data Protection Manager (DPM). En este escenario, las copias de seguridad de las máquinas y de las cargas de trabajo se realizan en MABS/DPM y, después, las copias de seguridad de MABS/DPM se realizan en un almacén de Azure con el agente de MARS.
Aquello de lo que puede realizar una copia de seguridad depende de dónde está instalado el agente.

> [!NOTE]
> El método principal para hacer copias de seguridad de máquinas virtuales de Azure es mediante una extensión de Azure Backup en la máquina virtual. De esta manera, se realiza la copia de seguridad de toda la máquina virtual. Es posible que desee instalar y usar el agente de MARS junto con la extensión si desea realizar una copia de seguridad de archivos y carpetas específicos en la máquina virtual. [Más información](backup-architecture.md#architecture-built-in-azure-vm-backup).

![Pasos del proceso de copia de seguridad](./media/backup-configure-vault/initial-backup-process.png)

## <a name="before-you-start"></a>Antes de comenzar

* [Obtenga información sobre cómo](backup-architecture.md#architecture-direct-backup-of-on-premises-windows-server-machines-or-azure-vm-files-or-folders) Azure Backup realiza una copia de seguridad de máquinas Windows con el agente de MARS.
* [Obtenga información sobre](backup-architecture.md#architecture-back-up-to-dpmmabs) la arquitectura de copia de seguridad que ejecuta el agente de MARS en un servidor MABS o DPM secundario.
* [Revise](backup-support-matrix-mars-agent.md) qué se admite y de qué se puede hacer una copia de seguridad con el agente de MARS.
* Compruebe el acceso a Internet en los equipos de los que desea realizar copias de seguridad.
* Si desea crear una copia de seguridad de un servidor o cliente en Azure, necesita una cuenta de Azure. En caso de no tener ninguna, puede crear una [cuenta gratuita](https://azure.microsoft.com/free/) en tan solo unos minutos.

### <a name="verify-internet-access"></a>Comprobación del acceso a Internet

Si la máquina tiene limitado el acceso a Internet, asegúrese de que su configuración de firewall en la máquina o el proxy está establecida para permitir estas direcciones URL e IP:

#### <a name="urls"></a>URLs

* www\.msftncsi.com
* *.Microsoft.com
* *.WindowsAzure.com
* *.microsoftonline.com
* \* .windows.net

#### <a name="ip-addresses"></a>Direcciones IP

* 20.190.128.0/18
* 40.126.0.0/18

El acceso a todas las direcciones URL y direcciones IP enumeradas anteriormente usa el protocolo HTTPS en el puerto 443.

## <a name="create-a-recovery-services-vault"></a>Creación de un almacén de Recovery Services

Un almacén de Recovery Services almacena todas las copias de seguridad y los puntos de recuperación que se crean con el tiempo y contiene la directiva de copia de seguridad aplicada a las máquinas de las que se ha realizado una copia de seguridad. Cree un almacén como se indica a continuación:

1. Inicie sesión en [Azure Portal](https://portal.azure.com/) mediante la suscripción de Azure.

2. Busque **Almacenes de Recovery Services** y selecciónelo.

    ![Creación del almacén de Recovery Services, paso 1](./media/backup-configure-vault/open-recovery-services-vaults.png)

3. En el menú **Almacenes de Recovery Services**, haga clic en **+Agregar**.

    ![Creación del almacén de Recovery Services, paso 2](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

4. En **Nombre**, escriba un nombre descriptivo que identifique el almacén.

   * El nombre debe ser único para la suscripción de Azure.
   * Puede contener entre 2 y 50 caracteres.
   * Debe comenzar por una letra y solo puede contener letras, números y guiones.

5. Seleccione la suscripción de Azure, el grupo de recursos y la región geográfica en la que se debe crear el almacén. Los datos de copia de seguridad se envían al almacén. A continuación, haga clic en **Crear**.

    ![Creación del almacén de Recovery Services, paso 3](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

   * La creación del almacén puede tardar un tiempo.
   * Supervise las notificaciones del estado en la parte superior derecha del portal. Si no ve el almacén pasados unos minutos, haga clic en **Actualizar**.

     ![Clic en el botón Refresh (Actualizar)](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)

### <a name="set-storage-redundancy"></a>Establecimiento de la redundancia de almacenamiento

Azure Backup administra automáticamente el almacenamiento para el almacén. Debe especificar cómo se replica ese almacenamiento.

1. En la hoja **Almacenes de Recovery Services**, haga clic en el almacén nuevo. En la sección **Configuración**, haga clic en **Propiedades**.
2. En **Propiedades**, en **Configuración de copia de seguridad**, haga clic en **Actualizar**.

3. Seleccione el tipo de replicación de almacenamiento y haga clic en **Guardar**.

      ![Establecimiento de la configuración de almacenamiento del nuevo almacén](./media/backup-try-azure-backup-in-10-mins/recovery-services-vault-backup-configuration.png)

* Se recomienda que, si usa Azure como punto de conexión del almacenamiento de copia de seguridad principal, siga utilizando la configuración **con redundancia geográfica** predeterminada.
* Si no utiliza Azure como punto de conexión de almacenamiento de copia de seguridad principal, elija **Redundancia local** para reducir los costes de almacenamiento de Azure.
* Obtenga más información sobre la redundancia [geográfica](../storage/common/storage-redundancy-grs.md) y [local](../storage/common/storage-redundancy-lrs.md).

## <a name="download-the-mars-agent"></a>Descarga del agente de MARS

Descargue el agente de MARS para instalarlo en las máquinas de las que desee realizar copias de seguridad.

* Si ya ha instalado el agente en alguna máquina, asegúrese de que está ejecutando la versión más reciente.
* La versión más reciente está disponible en el portal o mediante [descarga directa](https://aka.ms/azurebackup_agent).

1. En el almacén, en **Introducción**, haga clic en **Copia de seguridad**.

    ![Abrir hoja de objetivo de copia de seguridad](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

2. En **¿Dónde se ejecuta su carga de trabajo?** , seleccione **Local**. Debe seleccionar esta opción incluso si desea instalar el agente de MARS en la máquina virtual de Azure.
3. En el menú **¿De qué desea hacer una copia de seguridad?** , seleccione **Archivos y carpetas** o **Estado del sistema**. Hay una serie de opciones disponibles, pero solo se admiten si ejecuta un servidor de copia de seguridad secundario. Haga clic en **Preparar infraestructura**.

      ![Configuración de archivos y carpetas](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

4. En **Preparar infraestructura**, en **Instalar el agente de Recovery Services**, descargue el agente de MARS.

    ![Prepare infrastructure](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

5. Haga clic en **Guardar** en el menú emergente de descarga. De forma predeterminada, se guarda el archivo **MARSagentinstaller.exe** en la carpeta de descargas.

6. Ahora, marque **Ya se ha descargado o se usa el agente de Recovery Services más reciente** y, a continuación, descargue las credenciales del almacén.

    ![descargar las credenciales de almacén](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

7. Haga clic en **Save**(Guardar). El archivo se descargará en la carpeta Descargas. El archivo de credenciales del almacén no se puede abrir.

## <a name="install-and-register-the-agent"></a>Instalación y registro del agente

1. Ejecute el archivo **MARSagentinstaller. exe** en las máquinas de las que desea realizar una copia de seguridad.
2. En el Asistente para la instalación del agente de MARS > **Configuración de la instalación**, especifique dónde desea instalar el agente y una ubicación para la memoria caché. A continuación, haga clic en **Siguiente**.
   * Azure Backup usa la memoria caché para almacenar instantáneas de datos antes de enviarlas a Azure.
   * La ubicación de la memoria caché debe tener un espacio libre igual a al menos el 5 % del tamaño de los datos de los que se va a realizar la copia de seguridad.

    ![Configuración de instalación del asistente de MARS](./media/backup-configure-vault/mars1.png)

3. En **Configuración del proxy**, especifique cómo se conectará el agente que se ejecuta en la máquina Windows a Internet. A continuación, haga clic en **Siguiente**.

   * Si utiliza un proxy personalizado, especifique la configuración del proxy y las credenciales si es necesario.
   * Recuerde que el agente necesita acceso a [estas direcciones URL](#verify-internet-access).

     ![Acceso a Internet del asistente de MARS](./media/backup-configure-vault/mars2.png)

4. En **Instalación**, compruebe los requisitos previos y haga clic en **Instalar**.
5. Una vez instalado el agente, haga clic en **Proceder al registro**.
6. En el **Asistente para registrar servidor** > **Identificación del almacén**, busque y seleccione el archivo de credenciales descargado. A continuación, haga clic en **Siguiente**.

    ![Registro: credenciales de almacén](./media/backup-configure-vault/register1.png)

7. En **Configuración de cifrado**, especifique una frase de contraseña que se usará para cifrar y descifrar copias de seguridad de la máquina.

    * Guarde la frase de contraseña en una ubicación segura.
    * Si pierde u olvida la frase de contraseña, Microsoft no puede ayudarle a recuperar los datos de copia de seguridad. Guarde el archivo en una ubicación segura. Es necesario para restaurar una copia de seguridad.

8. Haga clic en **Finalizar** Ahora está instalado el agente y el equipo está registrado en el almacén. Está listo para configurar y programar la copia de seguridad.

## <a name="create-a-backup-policy"></a>Crear una directiva de copia de seguridad

La directiva de copia de seguridad especifica cuándo tomar instantáneas de los datos para crear puntos de recuperación y cuánto tiempo se conservan los puntos de recuperación.

* Una directiva de copia de seguridad se configura mediante el agente de MARS.
* Azure Backup no tiene en cuenta automáticamente el horario de verano. Esto podría producir una discrepancia entre la hora real y la hora programada para la copia de seguridad.

Cree una directiva de la siguiente manera:
1. Después de descargar y registrar el agente de MARS, inicie la consola del agente. Para encontrarlo, busque **Microsoft Azure Backup**en la máquina.  
2. En **Acciones**, haga clic en **Programar copia de seguridad**.

    ![Programar una copia de seguridad de Windows Server](./media/backup-configure-vault/schedule-first-backup.png)
3. En el Asistente para programar copias de seguridad, en **Introducción**, haga clic en **Siguiente**.
4. En la página **Seleccionar elementos de los que realizar copia de seguridad**, haga clic en **Agregar elementos**.

    ![Seleccionar elementos de los que realizar copia de seguridad](./media/backup-azure-manage-mars/select-item-to-backup.png)

5. En **Seleccionar elementos**, seleccione lo que desee incluir en la copia de seguridad y haga clic en **Aceptar**.

    ![Elementos seleccionados para realizar una copia de seguridad](./media/backup-azure-manage-mars/selected-items-to-backup.png)

6. En la página **Seleccionar elementos de los que realizar copia de seguridad**, haga clic en **Siguiente**.
7. En la página **Especifique la programación de copia de seguridad**, especifique cuándo desea realizar copias de seguridad diarias o semanales. A continuación, haga clic en **Siguiente**.

    - Cuando se realiza una copia de seguridad, se crea un punto de recuperación.
    - El número de puntos de recuperación creados en el entorno depende de la programación de copia de seguridad.


8. Puede programar copias de seguridad diarias, hasta tres veces al día. Por ejemplo, en la captura de pantalla se muestran dos copias de seguridad diarias, una a medianoche y otra a las 6:00 p. m.

    ![Programación diaria](./media/backup-configure-vault/day-schedule.png)


9. También puede ejecutar copias de seguridad semanales. Por ejemplo, en la captura de pantalla se muestran copias de seguridad tomadas cada domingo y miércoles alternos, a las 9:30 a. m. y a la 1:00 a. m.

    ![Programación semanal](./media/backup-configure-vault/week-schedule.png)


10. En la página **Seleccionar directiva de retención**, especifique cómo desea almacenar las copias históricas de los datos. A continuación, haga clic en **Siguiente**.

    - La configuración de retención especifica qué puntos de recuperación deben almacenarse y durante cuánto tiempo.
    - Por ejemplo, cuando se establece una configuración de retención diaria, se indica que, en el momento especificado para la retención diaria, se conservará el punto de recuperación más reciente durante el número de días especificado. También puede especificar una directiva de retención mensual para indicar que el punto de recuperación creado el 30 de cada mes debe almacenarse durante 12 meses.
    - La retención diaria y semanal del punto de recuperación normalmente coincide con la programación de copia de seguridad. Es decir, cuando se desencadena la copia de seguridad de acuerdo con la programación, el punto de recuperación creado por la copia de seguridad se almacena durante el tiempo indicado en la directiva de retención diaria o semanal.
    - En la siguiente captura de pantalla se muestra un ejemplo:

        -   Las copias de seguridad diarias a medianoche ya la 6:00 p. m. se conservan durante siete días.
        -   Las copias de seguridad realizadas un sábado a medianoche y a las 6:00 p. m. se conservan durante cuatro semanas.
        -   Las copias de seguridad tomadas el sábado de la última semana del mes a medianoche y a las 6:00 p. m. se conservan durante 12 meses.
        -   Las copias de seguridad tomadas el sábado de la última semana de marzo se conservan durante 10 años.

        ![Ejemplo de retención](./media/backup-configure-vault/retention-example.png)


11. En **Elija el tipo de copia de seguridad inicial** decida si desea realizar la copia de seguridad inicial a través de la red o si usar la copia de seguridad sin conexión (para más información sobre la copia de seguridad sin conexión, consulte este [artículo](offline-backup-azure-data-box.md)). Para realizar la copia de seguridad inicial a través de la red, seleccione **Automáticamente por encima de la red** y haga clic en **Siguiente**.

    ![Tipo de copia de seguridad inicial](./media/backup-azure-manage-mars/choose-initial-backup-type.png)


12. En **Confirmación**, revise la información y, luego, haga clic en **Finalizar**.

    ![Confirmación del tipo de copia de seguridad](./media/backup-azure-manage-mars/confirm-backup-type.png)


13. Cuando el asistente termine de crear la programación de copia de seguridad, haga clic en **Cerrar**.

    ![Confirmación de modificación del proceso de copia de seguridad](./media/backup-azure-manage-mars/confirm-modify-backup-process.png)

Debe crear una directiva en cada máquina en la que esté instalado el agente.

### <a name="perform-the-initial-backup-offline"></a>Realización de la copia de seguridad inicial sin conexión

Puede ejecutar una copia de seguridad inicial automáticamente a través de la red o sin conexión. La propagación sin conexión de una copia de seguridad inicial resulta útil si tiene grandes cantidades de datos que requerirán una gran cantidad de ancho de banda de red para transferirse. Una transferencia sin conexión se realiza de la siguiente manera:

1. Escriba los datos de copia de seguridad en una ubicación de almacenamiento provisional.
2. Use la herramienta AzureOfflineBackupDiskPrep para copiar los datos de la ubicación de almacenamiento provisional en uno o más discos SATA.
3. La herramienta crea un trabajo de importación de Azure. Obtenga más información sobre la [importación y exportación de Azure DNS](https://docs.microsoft.com/azure/storage/common/storage-import-export-service).
4. Envíe los discos SATA a un centro de datos de Azure.
5. En el centro de datos, los datos de los discos se copian en una cuenta de almacenamiento de Azure.
6. Azure Backup copia los datos de la cuenta de almacenamiento en el almacén y se programan copias de seguridad incrementales.

[Más información](offline-backup-azure-data-box.md) sobre la propagación sin conexión.

### <a name="enable-network-throttling"></a>Habilitación de la limitación de la red

Puede habilitar un límite de ancho de banda de red para controlar el uso que hace el agente de MARS del ancho de banda. El límite es útil si necesita realizar una copia de seguridad de los datos durante las horas de trabajo pero desea controlar la cantidad de ancho de banda que se usa para la actividad de copia de seguridad y restauración.

* El límite de ancho de banda de red de Azure Backup utiliza [Calidad de servicio (QoS)](https://docs.microsoft.com/windows-server/networking/technologies/qos/qos-policy-top) en el sistema operativo local.
* El límite de ancho de banda de red para la copia de seguridad está disponible en Windows Server 2012 y Windows 8 (y en las versiones posteriores de ambos sistemas). Los sistemas operativos deben ejecutar los Service Pack más recientes.

Habilite el límite de ancho de banda de red de la siguiente manera:

1. En el agente de MARS, haga clic en **Cambiar propiedades**.
2. En la pestaña **Límite**, seleccione la opción **Habilitar límite de uso del ancho de banda de Internet para las operaciones de copia de seguridad**.

    ![Limitación de la red](./media/backup-configure-vault/throttling-dialog.png)
3. Especifique el ancho de banda permitido durante el horario laboral y fuera de las horas de trabajo. Los valores de ancho de banda comienzan en 512 Kbps y llegan hasta 1023 MBps. A continuación, haga clic en **Aceptar**.

## <a name="run-an-on-demand-backup"></a>Ejecución de una copia de seguridad a petición

1. En el agente de MARS, haga clic en **Hacer copia de seguridad ahora**.

    ![Realizar copia de seguridad de Windows Server ahora](./media/backup-configure-vault/backup-now.png)

2. Si la versión del agente de MARS es la 2.0.9169.0 o posterior, se puede establecer una retención personalizada. En la sección **Conservar la copia de seguridad hasta**, elija una fecha en el calendario mostrado:

   ![Calendario Duración de retención](./media/backup-configure-vault/mars-ondemand.png)

3. En **Confirmación**, revise la configuración y haga clic en **Hacer copia de seguridad**.
4. Haga clic en **Cerrar** para cerrar el asistente. Si lo hace antes de que finalice el proceso de copia de seguridad, el asistente se seguirá ejecutando en segundo plano.
5. Una vez que finalice la copia de seguridad inicial, el estado **Trabajo completado** se refleja en la consola de Copia de seguridad.

## <a name="on-demand-backup-policy-retention-behavior"></a>Comportamiento de retención de la directiva de copia de seguridad a petición

>[!NOTE]
>Solo aplicable a las versiones del agente de MARS anteriores a la 2.0.9169.0.
>

* Para más información, consulte el paso 8 de [Crear una directiva de copia de seguridad](backup-configure-vault.md#create-a-backup-policy).

| Opción Programación de copia de seguridad | ¿Cuánto tiempo se conservarán los datos de la copia de seguridad?
| -- | --
| Programar una copia de seguridad cada: *Día | **Retención predeterminada**: Equivalente a "retención en días para copias de seguridad diarias" <br/><br/> **Excepción**: Si se produce un error en una copia de seguridad programada diariamente establecida para retención a largo plazo (semanas, meses o años), se desencadena una copia de seguridad a petición justo después de que se considere esta copia de seguridad programada para retención a largo plazo. De lo contrario, se considerará la siguiente copia de seguridad programada para la retención a largo plazo.<br/><br/> **Ejemplo**: Si se produce un error en una copia de seguridad programada realizada, por ejemplo, el jueves a las 8:00 a. m. y la misma copia de seguridad se ha considerado para retención semanal, mensual o anual, la primera copia de seguridad a petición desencadenada antes de la siguiente copia de seguridad programada (por ejemplo, el viernes a las 8:00 a. m.) se etiquetaría automáticamente para la retención semanal, mensual o anual, como corresponde a la copia de seguridad del jueves a las 8:00 a. m.
| Programar una copia de seguridad cada: *Semana | **Retención predeterminada**: 1 día <br/> Las copias de seguridad a petición realizadas para un origen de datos con la directiva de copia de seguridad semanal se eliminan el día siguiente, incluso si son las copias de seguridad más recientes para el origen de datos. <br/><br/> **Excepción**: Si se produce un error en una copia de seguridad programada semanalmente establecida para retención a largo plazo (semanas, meses o años), se desencadena una copia de seguridad a petición justo después de que se considere esta copia de seguridad programada para retención a largo plazo. De lo contrario, se considerará la siguiente copia de seguridad programada para la retención a largo plazo. <br/><br/> **Ejemplo**: Si se produce un error en una copia de seguridad programada realizada, por ejemplo, el jueves a las 8:00 a. m. y la misma copia de seguridad se ha considerado para retención mensual o anual, la primera copia de seguridad a petición desencadenada antes de la siguiente copia de seguridad programada (por ejemplo, el jueves a las 8:00 a. m.) se etiquetaría automáticamente para la retención mensual o anual, como corresponde a la copia de seguridad del jueves a las 8:00 a. m.

## <a name="next-steps"></a>Pasos siguientes

[Aprenda a restaurar archivos](backup-azure-restore-windows-server.md).
