---
title: Solución de problemas de copia de seguridad del estado del sistema con Azure Backup
description: Solución de problemas de copia de seguridad de estado del sistema.
services: backup
author: srinathvasireddy
manager: sivan
keywords: cómo la copia de seguridad; estado del sistema de copia de seguridad
ms.service: backup
ms.topic: conceptual
ms.date: 05/09/2019
ms.author: srinathvasireddy
ms.openlocfilehash: 53b9f8fb58a6e70a4bd2cd02adb9ce824466d7de
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481600"
---
# <a name="troubleshoot-system-state-backup"></a>Solución de problemas de copia de seguridad del estado del sistema

Este artículo describe soluciones a problemas que pueden surgir al usar la copia de seguridad de estado del sistema.

## <a name="pre-requisite"></a>Requisito previo

Antes de solucionar problemas de copia de seguridad del sistema de estado con Azure Backup, asegúrese de que realizar los siguientes requisitos previos, consulte.  

### <a name="verify-windows-server-backup-is-installed"></a>Comprobar que copia de seguridad de Windows Server está instalado

Asegúrese de copia de seguridad de Windows Server está instalado y habilitado en el servidor. Para comprobar el estado de instalación, ejecute el siguiente comando de PowerShell:

 ```
 PS C:\> Get-WindowsFeature Windows-Server-Backup
 ```
Si la salida muestra el **estado de instalación** como **disponibles**, significa que característica de copia de seguridad de Windows Server está disponible para la instalación pero no está instalada en el servidor. Sin embargo si la copia de seguridad de Windows Server no está instalado, a continuación, utilice uno de los siguientes métodos para instalarlo.

**Método 1: Instale copias de seguridad de Windows Server mediante PowerShell**

Para instalar copias de seguridad de Windows Server con PowerShell, ejecute el comando siguiente:

  ```
  PS C:\> Install-WindowsFeature -Name Windows-Server-Backup
  ```

**Método 2: Instale copias de seguridad de Windows Server mediante el administrador del servidor**

Para instalar copias de seguridad de Windows Server mediante el administrador del servidor, realice los siguientes:

1. En el **Server Manager** y haga clic en **agregar roles y características**. El **agregar roles y características Asistente** aparece.

    ![panel](./media/backup-azure-system-state-troubleshoot/server_management.jpg)

2. Seleccione **tipo de instalación** y haga clic en **siguiente**.

    ![Tipo de instalación](./media/backup-azure-system-state-troubleshoot/install_type.jpg)

3. Seleccione un servidor del grupo de servidores y haga clic en **siguiente**. En el rol de servidor, deje la selección predeterminada y haga clic en **siguiente**.
4. Seleccione **copias de seguridad de Windows Server** en **características** ficha y haga clic en **siguiente**.

    ![features](./media/backup-azure-system-state-troubleshoot/features.png)

5. En el **confirmación** , haga clic **instalar** para iniciar el proceso de instalación.
6. En el **resultados** pestaña, se mostrará la copia de seguridad de Windows Server característica se instala correctamente en el servidor de Windows.

    ![result](./media/backup-azure-system-state-troubleshoot/results.jpg)


### <a name="system-volume-information-permission"></a>Permiso de información de volumen del sistema

Asegúrese de que el sistema Local tiene control total **System Volume Information** carpeta ubicada en el volumen donde está instalado windows. Esta suele ser **C:\System Volume Information**. Copia de seguridad de Windows Server puede producir un error si los permisos anteriores no están establecidos correctamente

### <a name="dependent-services"></a>Servicios dependientes

Asegúrese de los siguientes servicios están en estado de ejecución:

**Nombre del servicio** | **Tipo de inicio**
--- | ---
Call(RPC) de procedimiento remoto | Automático
Eventos COM + System(EventSystem) | Automático
Service(SENS) de notificación de eventos del sistema | Automático
Copy(VSS) de instantáneas de volumen | Manual
Provider(SWPRV) de copia de instantáneas de Software de Microsoft | Manual

### <a name="validate-windows-server-backup-status"></a>Validar el estado de copia de seguridad de Windows Server

Para validar el estado de copia de seguridad de Windows Server, realice los siguientes:

  * Asegúrese de que WSB PowerShell se está ejecutando

    -   Ejecute `Get-WBJob` desde un PowerShell de con privilegios elevados y asegúrese de que no devuelve el error siguiente:

    > [!WARNING]
    > Get-WBJob: El término 'Get-WBJob' no se reconoce como nombre de un cmdlet, función, archivo de script o programa ejecutable. Compruebe si escribió correctamente el nombre o, si incluyó una ruta de acceso, compruebe que dicha ruta es correcta e inténtelo de nuevo.

    -   Si se produce un error con este error, a continuación, vuelva a instalar la característica de copia de seguridad de Windows Server en el equipo del servidor como se mencionó en el paso 1, los requisitos previos.

  * Asegúrese de copia de seguridad WSB funciona correctamente, ejecutando el siguiente comando desde el símbolo del sistema con privilegios elevados:

      ` wbadmin start systemstatebackup -backuptarget:X: -quiet `

      > [!NOTE]
      >Reemplace X por la letra de unidad del volumen donde desea almacenar el estado del sistema realizar copias de seguridad de la imagen.

    - Compruebe periódicamente el estado del trabajo mediante la ejecución de `Get-WBJob` comando de PowerShell con privilegios elevados        
    - Cuando finalice el trabajo de copia de seguridad Compruebe el estado del trabajo final ejecutando `Get-WBJob -Previous 1` comando

Si se produce un error en el trabajo, indica un problema WSB, lo que daría lugar a un error de copias de seguridad de estado del sistema del agente de MARS.

## <a name="common-errors"></a>Errores comunes

### <a name="vss-writer-timeout-error"></a>Error de tiempo de espera del escritor de VSS

| Síntoma | Causa | Resolución
| -- | -- | --
| -Se produce un error en el agente de MARS mensaje de error: "Error del trabajo WSB con errores VSS. Compruebe los registros de eventos VSS para resolver el error"<br/><br/> -Siguiente error registro está presente en los registros de eventos de aplicación de VSS: "Un escritor de VSS ha rechazado un evento con error 0x800423f2, ha expirado el tiempo de espera del escritor entre los eventos inmovilizar y descongelar".| Escritor VSS no puede completar en el tiempo debido a la falta de recursos de CPU y memoria en el equipo <br/><br/> Otro software de copia de seguridad ya está utilizando el escritor de VSS, como resultado no se pudo completar la operación de instantánea para esta copia de seguridad | Espere de CPU/memoria ser liberados automáticamente en el sistema o anular tardando demasiada memoria y CPU de procesos y vuelva a intentarlo <br/><br/>  Espere a que la copia de seguridad en curso completar e intente la operación en un momento posterior, cuando no se ejecutan copias de seguridad en la máquina


### <a name="insufficient-disk-space-to-grow-shadow-copies"></a>Espacio en disco suficiente para crecer instantáneas

| Síntoma | Resolución
| -- | --
| -Se produce un error en el agente de MARS mensaje de error: Error de copia de seguridad como el volumen de instantáneas no se completó debido a espacio en disco insuficiente en volúmenes que contienen los archivos del sistema <br/><br/> -Está presente en los registros de eventos del sistema de volsnap después de registro de errores y advertencias: "Hubo espacio en disco insuficiente en el volumen C: para aumentar el almacenamiento de instantáneas de instantáneas de C: debido a este error todas las instantáneas de volumen C: corren el riesgo de que se va a eliminar" | -Liberar espacio en el volumen resaltado en el registro de eventos para que no haya suficiente espacio para instantáneas aumentando mientras la copia de seguridad está en curso <br/><br/> -Al configurar el espacio de la instantánea se puede restringir la cantidad de espacio usado por instantánea, para obtener más información vea esto [artículo](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788050(v=ws.11)#syntax)


### <a name="efi-partition-locked"></a>Partición EFI bloqueado

| Síntoma | Resolución
| -- | --
| Agente de MARS se produce un error con el mensaje de error: "Vuelva del estado del sistema hasta error como la partición del sistema EFI está bloqueada. Puede ser debido al acceso a la partición del sistema por una seguridad de terceros o realizar copias de seguridad de software" | -Si el problema es debido a un software de seguridad de otros fabricantes, a continuación, deberá ponerse en contacto con el proveedor de antivirus para que puede permitir que al agente de MARS <br/><br/> -Si se está ejecutando un software de copia de seguridad de terceros, a continuación, espere que termine y vuelva a intentar la copia de


## <a name="next-steps"></a>Pasos siguientes

- Para obtener más información sobre el estado del sistema de Windows en la implementación de Resource Manager, consulte [copia de seguridad de estado del sistema de Windows Server](backup-azure-system-state.md)
