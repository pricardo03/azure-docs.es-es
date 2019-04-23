---
title: 'Prepare y personalice una imagen de disco duro virtual principal: Azure'
description: Cómo preparar, personalizar y cargar una imagen maestra de vista previa de Escritorio Virtual de Windows en Azure.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 04/03/2019
ms.author: helohr
ms.openlocfilehash: aff96931f95442c67d08521e72952dd79dad44e2
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/22/2019
ms.locfileid: "59999882"
---
# <a name="prepare-and-customize-a-master-vhd-image"></a>Preparación y personalización de una imagen de disco duro virtual maestro

Este artículo explica cómo preparar una imagen de disco duro virtual (VHD) maestro para cargar en Azure, incluida la forma de crear máquinas virtuales (VM) e instalar y configurar software en ellos. Estas instrucciones son para una configuración de Escritorio Virtual de Windows específicos de la versión preliminar que puede usarse con los procesos existentes de su organización.

## <a name="create-a-vm"></a>Crear una VM

Varias sesiones de Windows 10 Enterprise está disponible en la Galería de imágenes de Azure. Hay dos opciones para personalizar esta imagen.

La primera opción es aprovisionar una máquina virtual (VM) de Azure siguiendo las instrucciones de [crear una máquina virtual desde una imagen administrada](https://docs.microsoft.com/azure/virtual-machines/windows/create-vm-generalized-managed)y, a continuación, vaya directamente a [Software preparación e instalación](set-up-customize-master-image.md#software-preparation-and-installation).

La segunda opción es crear la imagen localmente mediante la descarga de la imagen, el aprovisionamiento de una máquina virtual de Hyper-V y personalizarla para satisfacer sus necesidades, que abordaremos en la sección siguiente.

### <a name="local-image-creation"></a>Creación de imágenes locales

Una vez que haya descargado la imagen en una ubicación local, abra **Administrador de Hyper-V** para crear una máquina virtual con el disco duro virtual que acaba de copiar. La siguiente es la versión simple, pero puede encontrar instrucciones más detalladas en [crear una máquina virtual de Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v).

Para crear una máquina virtual con el VHD copiado:

1. Abra el **Asistente para nueva máquina Virtual**.

2. En la página Especificar generación, seleccione **generación 1**.

    ![Captura de pantalla de la página Especificar generación. La opción "Generación 1" está seleccionada.](media/a41174fd41302a181e46385e1e701975.png)

3. En el tipo de punto de comprobación, deshabilitar los puntos de control para ello, desactive la casilla de verificación.

    ![Captura de pantalla de la sección tipo de punto de comprobación de la página de los puntos de control.](media/20c6dda51d7cafef33251188ae1c0c6a.png)

También puede ejecutar el siguiente cmdlet de PowerShell para deshabilitar los puntos de control.

```powershell
Set-VM -Name <VMNAME> -CheckpointType Disabled
```

### <a name="fixed-disk"></a>Disco fijo

Si crea una máquina virtual desde un disco duro virtual existente, crea un disco dinámico de forma predeterminada. Se puede cambiar a un disco fijo seleccionando **editar disco...**  tal como se muestra en la siguiente imagen. Para obtener instrucciones detalladas, consulte [preparar un VHD de Windows o un VHDX para cargar en Azure](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image).

![Captura de pantalla de la opción Editar disco.](media/35772414b5a0f81f06f54065561d1414.png)

También puede ejecutar el siguiente cmdlet de PowerShell para cambiar el disco en un disco fijo.

```powershell
Convert-VHD –Path c:\\test\\MY-VM.vhdx –DestinationPath c:\\test\\MY-NEW-VM.vhd -VHDType Fixed
```

## <a name="software-preparation-and-installation"></a>La instalación y preparación de software

En esta sección se explica cómo preparar e instalar Office 365 ProPlus, OneDrive, FSLogix, Windows Defender y otras aplicaciones comunes. Si los usuarios necesitan tener acceso a determinadas aplicaciones LOB, se recomienda que instalarlos después de completar las instrucciones de esta sección.

Esta sección se supone que elevó acceso en la máquina virtual, si se aprovisiona en Azure o Hyper-V Manager.

### <a name="install-office-in-shared-computer-activation-mode"></a>Instalar Office en modo de activación de equipo compartido

Use la [herramienta de implementación de Office](https://www.microsoft.com/download/details.aspx?id=49117) para instalar Office. Varias sesiones de Windows 10 Enterprise solo es compatible con Office 365 ProPlus, no perpetuo de Office 2019.

La herramienta de implementación de Office requiere un archivo XML de configuración. Para personalizar el ejemplo siguiente, vea el [las opciones de configuración de la herramienta de implementación de Office](https://docs.microsoft.com/deployoffice/configuration-options-for-the-office-2016-deployment-tool).

Este ejemplo de configuración XML que hemos proporcionado hará lo siguiente:

- Instalar Office desde el canal de Insider y entregar actualizaciones desde el canal Insider cuando sean ejecutados.
- Use el x64 arquitectura.
- Deshabilitar las actualizaciones automáticas.
- Instalación de Visio y Project.
- Quite las instalaciones existentes de Office y migrar su configuración.
- Habilitar licencias para la operación en un entorno de servidor de terminal server de equipos compartidos.

Esto es lo que no hacer esta configuración XML de ejemplo:

- Instalación de Skype para la empresa
- Instale OneDrive en modo por el usuario. Para obtener más información, consulte [OneDrive instalar en modo por equipo](#install-onedrive-in-per-machine-mode).

>[!NOTE]
>Licencias de equipo compartido puede configurarse a través de objetos de directiva de grupo (GPO) o la configuración del registro. Se encuentra en el GPO **configuración del equipo\\directivas\\plantillas administrativas\\(equipo) de Microsoft Office 2016\\la configuración de licencias**

La herramienta de implementación de Office contiene setup.exe. Para instalar Office, ejecute el siguiente comando en una línea de comandos:

```batch
Setup.exe /configure configuration.xml
```

#### <a name="sample-configurationxml"></a>Ejemplo configuration.xml

El siguiente ejemplo de XML instalará la versión Insider, también conocido como Insider rápido o principal de Insider.

```xml
<Configuration>
    <Add OfficeClientEdition="64" SourcePath="http://officecdn.microsoft.com/pr/5440fd1f-7ecb-4221-8110-145efaa6372f">
        <Product ID="O365ProPlusRetail">
            <Language ID="en-US" />
            <Language ID="MatchOS" Fallback = "en-US"/>
            <Language ID="MatchPreviousMSI" />
            <ExcludeApp ID="Groove" />
            <ExcludeApp ID="Lync" />
            <ExcludeApp ID="OneDrive" />
            <ExcludeApp ID="Teams" />
        </Product>
        <Product ID="VisioProRetail">
            <Language ID="en-US" />
            <Language ID="MatchOS" Fallback = "en-US"/>
            <Language ID="MatchPreviousMSI" />
            <ExcludeApp ID="Teams" /> 
        </Product>
        <Product ID="ProjectProRetail">
            <Language ID="en-US" />
            <Language ID="MatchOS" Fallback = "en-US"/>
            <Language ID="MatchPreviousMSI" />
            <ExcludeApp ID="Teams" />
        </Product>
    </Add>
    <RemoveMSI All="True" />
    <Updates Enabled="FALSE" UpdatePath="http://officecdn.microsoft.com/pr/5440fd1f-7ecb-4221-8110-145efaa6372f" />
    <Display Level="None" AcceptEULA="TRUE" />
    <Logging Level="Verbose" Path="%temp%\WVDOfficeInstall" />
    <Property Value="TRUE" Name="FORCEAPPSHUTDOWN"/>
    <Property Value="1" Name="SharedComputerLicensing"/>
    <Property Value="TRUE" Name="PinIconsToTaskbar"/>
</Configuration>
```

>[!NOTE]
>El equipo de Office recomienda el uso de la instalación de 64 bits para el **OfficeClientEdition** parámetro.

Después de instalar Office, puede actualizar el comportamiento de Office de forma predeterminada. Ejecute los comandos siguientes individualmente o en un archivo por lotes para el comportamiento de la actualización.

```batch
rem Mount the default user registry hive
reg load HKU\TempDefault C:\Users\Default\NTUSER.DAT
rem Must be executed with default registry hive mounted.
reg add HKU\TempDefault\SOFTWARE\Policies\Microsoft\office\16.0\common /v InsiderSlabBehavior /t REG_DWORD /d 2 /f
rem Set Outlook's Cached Exchange Mode behavior
rem Must be executed with default registry hive mounted.
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v enable /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v syncwindowsetting /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v CalendarSyncWindowSetting /t REG_DWORD /d 1 /f
reg add "HKU\TempDefault\software\policies\microsoft\office\16.0\outlook\cached mode" /v CalendarSyncWindowSettingMonths  /t REG_DWORD /d 1 /f
rem Unmount the default user registry hive
reg unload HKU\TempDefault

rem Set the Office Update UI behavior.
reg add HKLM\SOFTWARE\Policies\Microsoft\office\16.0\common\officeupdate /v hideupdatenotifications /t REG_DWORD /d 1 /f
reg add HKLM\SOFTWARE\Policies\Microsoft\office\16.0\common\officeupdate /v hideenabledisableupdates /t REG_DWORD /d 1 /f
```

### <a name="disable-automatic-updates"></a>Deshabilitar las actualizaciones automáticas

Para deshabilitar las actualizaciones automáticas mediante Directiva de grupo local:

1. Abra **Editor de directivas de grupo Local\\plantillas administrativas\\componentes de Windows\\Windows Update**.
2. Haga clic en **Configurar actualización automática** y establézcalo en **deshabilitado**.

También puede ejecutar el siguiente comando en un símbolo del sistema para deshabilitar las actualizaciones automáticas.

```batch
reg add HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU /v NoAutoUpdate /t REG_DWORD /d 1 /f
```

### <a name="specify-start-layout-for-windows-10-pcs-optional"></a>Especificar el diseño de inicio para equipos Windows 10 (opcional)

Ejecute este comando para especificar un diseño de inicio para equipos Windows 10.

```batch
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer" /v SpecialRoamingOverrideAllowed /t REG_DWORD /d 1 /f
```

### <a name="install-onedrive-in-per-machine-mode"></a>Instalar OneDrive en el modo por equipo

OneDrive es normalmente instalados por el usuario. En este entorno, debe ser instalado por equipo.

Aquí le mostramos cómo instalar OneDrive en el modo por equipo:

1. En primer lugar, cree una ubicación para almacenar provisionalmente al instalador de OneDrive. Una carpeta del disco local o [\\\\unc] ubicación (file://unc) es correcto.

2. Descargue OneDriveSetup.exe en la ubicación de ensayo con este vínculo: <https://aka.ms/OneDriveWVD-Installer>

3. Si ha instalado office con OneDrive omitiendo  **\<ExcludeApp ID = "OneDrive" /\>**, desinstalar cualquier las instalaciones existentes por el usuario de OneDrive desde un símbolo del sistema con privilegios elevados, ejecutando el siguiente comando:
    
    ```batch
    "[staged location]\OneDriveSetup.exe" /uninstall
    ```

4. Ejecute este comando desde un símbolo del sistema con privilegios elevados para establecer el **AllUsersInstall** valor del registro:

    ```batch
    REG ADD "HKLM\Software\Microsoft\OneDrive" /v "AllUsersInstall" /t REG_DWORD /d 1 /reg:64
    ```

5. Ejecute este comando para instalar OneDrive en el modo por equipo:

    ```batch
    Run "[staged location]\OneDriveSetup.exe" /allusers
    ```

6. Ejecute este comando para configurar OneDrive para iniciar al inicio de sesión para todos los usuarios:

    ```batch
    REG ADD "HKLM\Software\Microsoft\Windows\CurrentVersion\Run" /v OneDrive /t REG_SZ /d "C:\\Program Files (x86)\Microsoft OneDrive\OneDrive.exe /background" /f
    ```

7. Habilitar **Configurar cuenta de usuario de forma silenciosa** , ejecute el comando siguiente.

    ```batch
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\OneDrive" /v "SilentAccountConfig" /t REG_DWORD /d 1 /f
    ```

8. Redirigir y mover conocida las carpetas en OneDrive, ejecute el siguiente comando de Windows.

    ```batch
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\OneDrive" /v "KFMSilentOptIn" /t REG_SZ /d "<your-AzureAdTenantId>" /f
    ```

### <a name="teams-and-skype"></a>Los equipos y Skype

Escritorio Virtual de Windows no admite oficialmente Skype para la empresa y los equipos.

### <a name="set-up-user-profile-container-fslogix"></a>Configurar el contenedor de perfil de usuario (FSLogix)

Para incluir el contenedor FSLogix como parte de la imagen, siga las instrucciones de [configura un recurso compartido de perfil de usuario para un grupo host](create-host-pools-user-profile.md#configure-the-fslogix-profile-container). Puede probar la funcionalidad del contenedor con FSLogix [este inicio rápido](https://docs.fslogix.com/display/20170529/Profile+Containers+-+Quick+Start).

### <a name="configure-windows-defender"></a>Configurar Windows Defender

Si Windows Defender está configurado en la máquina virtual, asegúrese de que configuró para no analizar todo el contenido de archivos VHD y VHDX durante los datos adjuntos del mismo.

Esta configuración sólo quita el análisis de los archivos VHD y VHDX durante los datos adjuntos, pero no afectará al análisis en tiempo real.

Para obtener más instrucciones sobre cómo configurar Windows Defender en Windows Server, consulte [exclusiones configurar Antivirus de Windows Defender en Windows Server](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus).

Para obtener más información sobre cómo configurar Windows Defender para excluir determinados archivos del análisis, vea [configurar y validar exclusiones en función de la ubicación del archivo de extensión y carpeta](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-antivirus/configure-extension-file-exclusions-windows-defender-antivirus).

### <a name="configure-session-timeout-policies"></a>Configurar las directivas de tiempo de espera de sesión

Directivas de sesión remota se pueden aplicar en el nivel de directiva de grupo, puesto que todas las máquinas virtuales en un grupo host forman parte del mismo grupo de seguridad.

Para configurar las directivas de sesión remoto:

1. Vaya a **plantillas administrativas** > **componentes de Windows** > **servicios de escritorio remoto**  >  **Host de sesión de escritorio remoto** > **los límites de tiempo de la sesión**.
2. En el panel del lado derecho, seleccione el **establecer el límite de tiempo para sesiones de servicios de escritorio remoto activos, pero en inactividad** directiva.
3. Cuando aparezca la ventana modal, cambie la opción de directiva de **no configurado** a **habilitado** para activar la directiva.
4. En el menú desplegable situado debajo de la opción de directiva, establezca la cantidad de tiempo para **4 horas**.

También puede configurar las directivas de sesión remoto manualmente ejecutando los comandos siguientes:

```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v RemoteAppLogoffTimeLimit /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fResetBroken /t REG_DWORD /d 1 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxConnectionTime /t REG_DWORD /d 10800000 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v RemoteAppLogoffTimeLimit /t REG_DWORD /d 0 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxDisconnectionTime /t REG_DWORD /d 5000 /f
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v MaxIdleTime /t REG_DWORD /d 7200000 /f
```

### <a name="set-up-time-zone-redirection"></a>Configurar la redirección de zona horaria

Redirección de zona horaria se puede aplicar en el nivel de directiva de grupo, puesto que todas las máquinas virtuales en un grupo host forman parte del mismo grupo de seguridad.

Para redirigir las zonas horarias:

1. En el servidor de Active Directory, abra el **Group Policy Management Console**.
2. Expanda el dominio y objetos de directiva de grupo.
3. Haga clic en el **objeto de directiva de grupo** que creó para la configuración de directiva de grupo y seleccione **editar**.
4. En el **Editor de administración de directivas de grupo**, vaya a **configuración del equipo** > **directivas** > **administrativo Plantillas de** > **componentes de Windows** > **servicios de escritorio remoto** > **Host de sesión de escritorio remoto**   >  **Redireccionar los recursos y dispositivos**.
5. Habilitar la **permitir redirección de zona horaria** configuración.

También puede ejecutar este comando en la imagen maestra para redirigir las zonas horarias:

```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fEnableTimeZoneRedirection /t REG_DWORD /d 1 /f
```

### <a name="disable-storage-sense"></a>Deshabilitar la detección de almacenamiento

Para el host de sesión de Escritorio Virtual de Windows que usan Windows 10 Enterprise o varias sesiones de Windows 10 Enterprise, se recomienda deshabilitar el sensor de almacenamiento. Puede deshabilitar el sensor de almacenamiento en el menú de configuración en **almacenamiento**, tal y como se muestra en la captura de pantalla siguiente:

![Captura de pantalla del menú en la configuración de almacenamiento. La opción de "Sentido de almacenamiento" se ha desactivado.](media/storagesense.png)

También puede cambiar la configuración con el registro, ejecute el comando siguiente:

```batch
reg add HKCU\Software\Microsoft\Windows\CurrentVersion\StorageSense\Parameters\StoragePolicy /v 01 /t REG_DWORD /d 0 /f
```

### <a name="include-additional-language-support"></a>Incluye compatibilidad con idiomas adicionales

Este artículo no trata la configuración de compatibilidad regional e idioma. Para más información, consulte los siguientes artículos.

- [Agregar idiomas a imágenes de Windows](https://docs.microsoft.com/windows-hardware/manufacture/desktop/add-language-packs-to-windows)
- [Características a petición](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities)
- [Características de idioma y región bajo demanda (du)](https://docs.microsoft.com/windows-hardware/manufacture/desktop/features-on-demand-language-fod)

### <a name="other-applications-and-registry-configuration"></a>Otras aplicaciones y la configuración del registro

Esta sección trata la configuración de aplicación y sistema operativo. Toda la configuración de esta sección se realiza a través de las entradas del registro que se pueden ejecutar mediante la línea de comandos y herramientas de regedit.

>[!NOTE]
>Puede implementar prácticas recomendadas en configuración General de los objetos de directiva (GPO) o las importaciones del registro. El administrador puede elegir cualquiera de las opciones en función de los requisitos de su organización.

Colección de centro de comentarios de los datos de telemetría en varias sesiones de Windows 10 Enterprise, ejecute este comando:

```batch
reg add HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\DataCollection "AllowTelemetry"=dword:00000003
reg add HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows\DataCollection /v AllowTelemetry /d 3
```

Ejecute el comando siguiente para corregir bloqueos de Watson:

```batch
remove CorporateWerServer* from Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting
```

Escriba los siguientes comandos en el editor del registro para corregir la compatibilidad con la resolución de 5 k. Debe ejecutar los comandos antes de poder habilitar la pila en paralelo.

```batch
[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp]
"MaxMonitors"=dword:00000004
"MaxXResolution"=dword:00001400
"MaxYResolution"=dword:00000b40

[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\rdp-sxs]
"MaxMonitors"=dword:00000004
"MaxXResolution"=dword:00001400
"MaxYResolution"=dword:00000b40
```

## <a name="prepare-the-image-for-upload-to-azure"></a>Preparar la imagen para cargarlos en Azure

Una vez haya terminado la configuración y todas las aplicaciones instaladas, siga las instrucciones de [preparar un VHD de Windows o un VHDX para cargar en Azure](https://docs.microsoft.com/azure/virtual-machines/windows/prepare-for-upload-vhd-image) para preparar la imagen.

Después de preparar la imagen para la carga, asegúrese de que la máquina virtual permanece en estado desactivado o desasignado.

## <a name="upload-master-image-to-a-storage-account-in-azure"></a>Cargar imagen maestra en una cuenta de almacenamiento de Azure

En esta sección solo se aplica cuando se creó localmente la imagen maestra.

Las siguientes instrucciones le indicará cómo cargar su imagen maestra en una cuenta de almacenamiento de Azure. Si aún no tiene una cuenta de almacenamiento de Azure, siga las instrucciones de [en este artículo](https://code.visualstudio.com/tutorials/static-website/create-storage) para crear uno.

1. Si no lo ha hecho ya, convertir la imagen de máquina virtual (VHD) en Fixed. Si no convierte la imagen en Fixed, no se puede crear correctamente la imagen.

2. Cargue el VHD en un contenedor de blobs en su cuenta de almacenamiento. Puede cargar rápidamente con la [herramienta Explorador de Storage](https://azure.microsoft.com/features/storage-explorer/). Para más información acerca de la herramienta Explorador de Storage, consulte [en este artículo](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows).

    ![Captura de pantalla de ventana de búsqueda de Microsoft Azure Storage Explorer de la herramienta. Se selecciona la casilla de verificación "Cargar archivos .vhd o vhdx como blobs en páginas (recomendados)".](media/897aa9a9b6acc0aa775c31e7fd82df02.png)

3. A continuación, vaya al portal de Azure en su explorador y busque "Imágenes". La búsqueda debe provocar el **creación de una imagen** página, como se muestra en la captura de pantalla siguiente:

    ![Captura de pantalla de la página de crear imágenes de Azure portal, se rellena con los valores de la imagen de ejemplo.](media/d3c840fe3e2430c8b9b1f44b27d2bf4f.png)

4. Una vez que haya creado la imagen, verá una notificación como la de la captura de pantalla siguiente:

    ![Captura de pantalla de la notificación de "imagen se creó correctamente".](media/1f41b7192824a2950718a2b7bb9e9d69.png)

## <a name="next-steps"></a>Pasos siguientes

Ahora que tiene una imagen, puede crear o actualizar los grupos host. Para obtener más información sobre cómo crear y actualizar los grupos host, consulte los artículos siguientes:

- [Crear un grupo host con una plantilla de Azure Resource Manager](create-host-pools-arm-template.md)
- [Tutorial: Crear un grupo host con Azure Marketplace](create-host-pools-azure-marketplace.md)
- [Creación de un grupo host con PowerShell](create-host-pools-powershell.md)
- [Configure un recurso compartido de perfil de usuario para un grupo host](create-host-pools-user-profile.md)
- [Configurar el método de equilibrio de carga de Escritorio Virtual de Windows](configure-host-pool-load-balancing.md)
