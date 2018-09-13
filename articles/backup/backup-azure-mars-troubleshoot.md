---
title: Solución de problemas del agente de Azure Backup
description: Solución de problemas de instalación y registro del Agente de Azure Backup
services: backup
author: saurabhsensharma
manager: shreeshd
ms.service: backup
ms.topic: conceptual
ms.date: 7/25/2018
ms.author: saurse
ms.openlocfilehash: 2c8978cfba8fc56d4dbc565cb3a91c75d9d54679
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/05/2018
ms.locfileid: "43700202"
---
# <a name="troubleshoot-microsoft-azure-recovery-services-mars-agent-issues"></a>Solución de problemas del agente de Microsoft Azure Recovery Services (MARS)
## <a name="recommended-steps"></a>Pasos recomendados
Consulte este artículo para resolver errores durante la configuración, el registro, la copia de seguridad y la restauración con el agente de MARS.

## <a name="invalid-vault-credentials-provided-the-file-is-either-corrupted-or-does-not-have-the-latest-credentials-associated-with-recovery-service"></a>Se han proporcionado credenciales de almacén no válidas. El archivo está dañado o no tiene asociadas las credenciales más recientes para el servicio de recuperación.
| Detalles del error | Causas posibles | Acciones recomendadas |
| ---     | ---     | ---    |
| **Error** </br> *Se han proporcionado credenciales de almacén no válidas. El archivo está dañado o no tiene asociadas las credenciales más recientes para el servicio de recuperación. (Id.: 34513)* | <ul><li> Las credenciales del almacén no son válidas (es decir, se descargaron más de 48 horas antes del registro).<li>El agente de MARS no puede descargar archivos en el directorio TEMP de Windows. <li>Las credenciales del almacén están en una ubicación de red. <li>TLS 1.0 está deshabilitado<li> Hay un servidor proxy configurado que bloquea la conexión. <br> |  <ul><li>Descargue las credenciales de almacén nuevas.<li>Vaya a **Opciones de Internet** > **Seguridad** > **Internet**. A continuación, seleccione **Nivel personalizado** y desplácese hasta que vea la sección de descarga de archivos. Seleccione **Habilitar**.<li>Es posible que también tenga que agregar el sitio a los [sitios de confianza](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Cambie la configuración para usar un servidor proxy. A continuación, proporcione los detalles del servidor proxy. <li> Haga coincidir la fecha y hora con las de la máquina.<li>Vaya a C:/Windows/Temp para comprobar si hay más de 60 000 o 65 000 archivos con la extensión .tmp. Si es el caso, elimínelos.<li>Puede verificar esto con la ejecución del paquete SDP en el servidor. Si recibe un error que indica que no se permiten las descargas de archivos, es probable que haya un gran número de archivos en el directorio C:/Windows/Temp.<li>Asegúrese de que tiene instalado .NET Framework 4.6.2. <li>Si deshabilitó TLS 1.0 a causa del cumplimiento de PCI, consulte esta [página de solución de problemas](https://support.microsoft.com/help/4022913). <li>Si tiene instalado un software antivirus instalado en el servidor, excluya los archivos siguientes del examen: <ul><li>CBengine.exe<li>CSC.exe, relacionado con .NET Framework. Hay un archivo CSC.exe por cada versión de .NET instalada en el servidor. Excluya todos los archivos CSC.exe vinculados a todas las versiones de .NET Framework en el servidor afectado. <li>Ubicación de caché o carpeta temporal. <br>*La ubicación predeterminada de la carpeta temporal o la ruta de acceso a la ubicación de caché es C:\Archivos de programa\Microsoft Azure Recovery Services Agent\Scratch*.

## <a name="the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>El Agente de Microsoft Azure Recovery Service no se pudo conectar a Microsoft Azure Backup

| Detalles del error | Causas posibles | Acciones recomendadas |
| ---     | ---     | ---    |
| **Error** </br><ol><li>*El Agente de Microsoft Azure Recovery Services no se pudo conectar a Microsoft Azure Backup. (Id.: 100050) Compruebe la configuración de red y asegúrese de que tiene conexión a Internet*<li>*(407) Se requiere autenticación del proxy* |El proxy bloquea la conexión. |  <ul><li>Vaya a **Opciones de Internet** > **Seguridad** > **Internet**. A continuación, seleccione **Nivel personalizado** y desplácese hasta que vea la sección de descarga de archivos. Seleccione **Habilitar**.<li>Es posible que también tenga que agregar el sitio a los [sitios de confianza](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Cambie la configuración para usar un servidor proxy. A continuación, proporcione los detalles del servidor proxy. <li>Si tiene instalado un software antivirus instalado en el servidor, excluya los archivos siguientes del examen. <ul><li>CBEngine.exe (en lugar de dpmra.exe).<li>CSC.exe (relacionado con .NET Framework). Hay un archivo CSC.exe por cada versión de .NET instalada en el servidor. Excluya todos los archivos CSC.exe vinculados a todas las versiones de .NET Framework en el servidor afectado. <li>Ubicación de caché o carpeta temporal. <br>*La ubicación predeterminada de la carpeta temporal o la ruta de acceso a la ubicación de caché es C:\Archivos de programa\Microsoft Azure Recovery Services Agent\Scratch*.

## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>No se pudo establecer la clave de cifrado para proteger las copias de seguridad

| Detalles del error | Causas posibles | Acciones recomendadas |
| ---     | ---     | ---    |      
| **Error** </br>*No se pudo establecer la clave de cifrado para las copias de seguridad protegidas. La activación no se realizó completamente, pero la frase de contraseña de cifrado se guardó en el siguiente archivo*. |<li>El servidor ya está registrado con otro almacén.<li>Durante la configuración, se ha dañado la frase de contraseña.| Anule el registro del servidor del almacén y vuelva a registrarlo con una nueva frase de contraseña.

## <a name="the-activation-did-not-complete-successfully-the-current-operation-failed-due-to-an-internal-service-error-0x1fc07"></a>La activación no se completó correctamente. No se pudo realizar la operación actual debido a un error de servicio interno [0x1FC07]

| Detalles del error | Causas posibles | Acciones recomendadas |
|---------|---------|---------|
|**Error** </br><ol>*La activación no se completó correctamente. No se pudo realizar la operación actual debido a un error de servicio interno [0x1FC07]. Vuelva a intentar la operación más tarde. Si el problema persiste, póngase en contacto con el servicio de soporte técnico de Microsoft*     | <li> La carpeta temporal se encuentra en un volumen sin espacio suficiente. <li> La carpeta temporal se movió de manera incorrecta a otra ubicación. <li> Falta el archivo OnlineBackup.KEK.         | <li>Actualice a la [versión más reciente](http://aka.ms/azurebackup_agent) del agente de MARS.<li>Mueva la carpeta temporal o la ubicación de caché a un volumen con espacio libre equivalente al 5 % o 10 % del tamaño total de los datos de copia de seguridad. Para mover correctamente la ubicación de caché, consulte los pasos de las [preguntas sobre el Agente de Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> Asegúrese de que exista el archivo OnlineBackup.KEK. <br>*La ubicación predeterminada de la carpeta temporal o la ruta de acceso a la ubicación de caché es C:\Archivos de programa\Microsoft Azure Recovery Services Agent\Scratch*.        |

## <a name="error-34506-the-encryption-passphrase-stored-on-this-computer-is-not-correctly-configured"></a>Error 34506. La frase de contraseña de cifrado almacenada en este equipo no está configurada correctamente.

| Detalles del error | Causas posibles | Acciones recomendadas |
|---------|---------|---------|
|**Error** </br><ol>*Error 34506. La frase de contraseña de cifrado almacenada en este equipo no está configurada correctamente*.    | <li> La carpeta temporal se encuentra en un volumen sin espacio suficiente. <li> La carpeta temporal se movió de manera incorrecta a otra ubicación. <li> Falta el archivo OnlineBackup.KEK.        | <li>Actualice a la [versión más reciente](http://aka.ms/azurebackup_agent) del agente de MARS.<li>Mueva la carpeta temporal o la ubicación de caché a un volumen con espacio libre equivalente al 5 % o 10 % del tamaño total de los datos de copia de seguridad. Para mover correctamente la ubicación de caché, consulte los pasos de las [preguntas sobre el Agente de Azure Backup](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> Asegúrese de que exista el archivo OnlineBackup.KEK. <br>*La ubicación predeterminada de la carpeta temporal o la ruta de acceso a la ubicación de caché es C:\Archivos de programa\Microsoft Azure Recovery Services Agent\Scratch*.         |

## <a name="backups-do-not-run-as-per-schedule"></a>Las copias de seguridad no se ejecutan según la programación
Realice estos pasos si las copias de seguridad programadas no se desencadenan automáticamente, mientras que las copias de seguridad manuales funcionan sin problemas. 
 
<li>Asegúrese de que PowerShell 3.0 o posterior esté instalado en el servidor. Para comprobar la versión de PowerShell, ejecute el siguiente comando y verifique que el número de versión *principal* es igual o mayor que 3.

`$PSVersionTable.PSVersion`
<li>Asegúrese de que la ruta de acceso siguiente forma parte de la variable de entorno *PSMODULEPATH*.

`<MARS agent installation path>\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup`
<li>Si la directiva de ejecución de PowerShell para *LocalMachine* está establecida como restringida, el cmdlet de PowerShell que desencadena la tarea de copia de seguridad puede producir errores. Ejecute los siguientes comandos en modo elevado para comprobar y establecer la directiva de ejecución en *Unrestricted* o *RemoteSigned*.

`PS C:\WINDOWS\system32> Get-ExecutionPolicy -List`

`PS C:\WINDOWS\system32> Set-ExecutionPolicy Unrestricted`
<li>Vaya a Panel de control -> Herramientas administrativas -> Programador de tareas -> expanda Microsoft -> seleccione Online Backup.
<li>Haga doble clic en la tarea "Microsoft-OnlineBackup" y vaya a la pestaña "Desencadenadores".
<li>Asegúrese de que el "Estado" de la tarea se establece en "Habilitado". Si no es así, haga clic en "Editar" y seleccione la casilla "Habilitado".
<li>Vaya a la sección *Opciones de seguridad* de la pestaña *General*.
<li>Asegúrese de que la cuenta de usuario que está seleccionada para la ejecución de la tarea es la de *sistema* o del grupo Administradores locales en el servidor

> [!TIP]
> Se recomienda reiniciar el servidor después de realizar los pasos anteriores para garantizar que los cambios realizados se aplican de forma coherente


## <a name="troubleshooting-restore-issues"></a>Solución de problemas de restauración

### <a name="failure-to-mount-the-recovery-volume-during-recovery-of-files-and-folders"></a>Error al montar el volumen de recuperación durante la recuperación de archivos y carpetas
Si Azure Backup no monta correctamente el volumen de recuperación incluso después de varios minutos de hacer clic en **Montar** o no se puede montar el volumen de recuperación con uno o varios errores, siga los pasos siguientes para empezar a realizar la recuperación normalmente.

1.  Cancele el proceso de montaje en curso en caso de que se haya estado ejecutando durante varios minutos.

2.  Asegúrese de que se encuentra en la versión más reciente del agente de Azure Backup. Para averiguar la información de versión del agente de Azure Backup, haga clic en **Acerca del agente de Microsoft Azure Recovery Services** en el panel **Acciones** de la consola de Microsoft Azure Backup y asegúrese de que el número de la **versión** es igual o mayor al de la versión mencionada en [este artículo](https://go.microsoft.com/fwlink/?linkid=229525). Puede descargar la versión más reciente [aquí](https://go.microsoft.com/fwLink/?LinkID=288905).

3.  Vaya al **Administrador de dispositivos** -> **Controladores de almacenamiento** y asegúrese de que puede encontrar el **iniciador iSCSI de Microsoft**. Si es así, vaya directamente al paso 7 a continuación. 

4.  Si no encuentra el servicio del iniciador iSCSI de Microsoft como se mencionó en el paso 3, compruebe si puede encontrar una entrada en **Administrador de dispositivos** -> **Controladores de almacenamiento** denominada **Dispositivo desconocido** con el identificador de hardware **ROOT\ISCSIPRT**.

5.  Haga clic con el botón derecho en **Dispositivo desconocido** y seleccione **Actualizar software de controlador**.

6.  Actualice el controlador. Para ello, seleccione la opción **Buscar software de controlador actualizado automáticamente**. Al finalizar la actualización **Dispositivo desconocido** debería pasar a **Iniciador iSCSI de Microsoft** tal y como se muestra a continuación. 

    ![Cifrado](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  Vaya a **Administrador de tareas** -> **Servicios (local)** -> **Servicio del iniciador iSCSI de Microsoft**. 

    ![Cifrado](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)
    
8.  Reinicie el servicio del iniciador iSCSI de Microsoft haciendo clic con el botón derecho en el servicio, después en Detener y, finalmente, haga clic con el botón derecho de nuevo y seleccione **Iniciar**.

9.  Vuelva a intentar la recuperación mediante la restauración instantánea. 

Si la recuperación sigue sin funcionar, reinicie el cliente y el servidor. Si no es recomendable reiniciar el equipo o la recuperación sigue sin funcionar incluso después de reiniciar el servidor, intente recuperar desde una máquina alternativa siguiendo los pasos descritos en [este artículo](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine).

## <a name="need-help-contact-support"></a>¿Necesita ayuda? Ponerse en contacto con soporte técnico
Si sigue necesitando ayuda, [póngase en contacto con el servicio de soporte técnico](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) para resolver el problema rápidamente.

## <a name="next-steps"></a>Pasos siguientes
* Más detalles sobre la [creación de copias de seguridad de Windows Server con el Agente de Azure Backup](tutorial-backup-windows-server-to-azure.md).
* Si necesita restaurar una copia de seguridad, use este artículo: [Restaurar archivos en una máquina de Windows Server o del Cliente de Windows](backup-azure-restore-windows-server.md).
