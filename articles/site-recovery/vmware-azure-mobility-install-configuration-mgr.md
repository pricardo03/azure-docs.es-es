---
title: Automatización del servicio Mobility para la recuperación ante desastres de la instalación en Azure Site Recovery
description: Cómo instalar automáticamente el servicio Mobility para la recuperación ante desastres de VMware/servidor físico con Azure Site Recovery.
author: Rajeswari-Mamilla
ms.topic: how-to
ms.date: 12/22/2019
ms.author: ramamill
ms.openlocfilehash: 50f8b5b4412e02692bf2b5d57b7f0dee27c2a25a
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/29/2020
ms.locfileid: "76842706"
---
# <a name="automate-mobility-service-installation"></a>Automatización de la instalación del servicio Mobility

En este artículo se describe cómo automatizar la instalación y las actualizaciones del agente del servicio Mobility en [Azure Site Recovery](site-recovery-overview.md).

Al implementar Site Recovery para la recuperación ante desastres de máquinas virtuales de VMware locales y servidores físicos en Azure, instala el agente del servicio Mobility en cada máquina que quiere replicar. El servicio Mobility captura las escrituras de datos en la máquina y las reenvía al servidor de procesos de Site Recovery para su replicación. Puede implementar el servicio Mobility de varias maneras:

- **Instalación de inserción**: Deje que Site Recovery instale el agente del servicio Mobility cuando habilite la replicación para una máquina en Azure Portal.
- **Instalación manual**: Instale el servicio Mobility manualmente en cada máquina. [Obtenga más información](vmware-physical-mobility-service-overview.md) acerca de la instalación de inserción y manual.
- **Implementación automatizada**: Automatice la instalación con herramientas de implementación de software como Microsoft Endpoint Configuration Manager.

La instalación y actualización automatizadas proporcionan una solución si:

- Su organización no permite la instalación de inserción en servidores protegidos.
- La directiva de la empresa requiere que las contraseñas se cambien periódicamente. Debe especificar una contraseña para la instalación de inserción.
- Su directiva de seguridad no permite agregar excepciones de firewall para máquinas específicas.
- Actúa como proveedor de servicios de hosting y no quiere proporcionar las credenciales de las máquinas de los clientes que se necesitan para la instalación de inserción con Site Recovery.
- Necesita escalar la instalación del agente a muchos servidores simultáneamente.
- Quiere programar las instalaciones y las actualizaciones durante las ventanas de mantenimiento planeado.



## <a name="prerequisites"></a>Prerequisites

Para la instalación automatizada, necesita lo siguiente:

- Una solución de instalación de software implementada como [Configuration Manager](https://docs.microsoft.com/configmgr/) o [JetPatch](https://jetpatch.com/microsoft-azure-site-recovery/). 
-  Los requisitos previos de implementación en [Azure](tutorial-prepare-azure.md) y [locales](vmware-azure-tutorial-prepare-on-premises.md) para la recuperación ante desastres de VMware, o para la recuperación ante desastres de [servidores físicos](physical-azure-disaster-recovery.md). También debe revisar los [requisitos de soporte](vmware-physical-azure-support-matrix.md) para la recuperación ante desastres.

## <a name="prepare-for-automated-deployment"></a>Preparación para la implementación automatizada

En la tabla siguiente se resumen las herramientas y los procesos para automatizar la implementación del servicio Mobility.

**Herramienta** | **Detalles** | **Instrucciones**
--- | --- | ---
**Administrador de configuración** | 1. Compruebe que cumple los [requisitos previos](#prerequisites) enumerados anteriormente. <br/><br/>2. Implemente la recuperación ante desastres mediante la configuración del entorno de origen, incluyendo la descarga de un archivo OVA para implementar el servidor de configuración de Site Recovery como una máquina virtual de VMware mediante una plantilla OVF.<br/><br/> 2. Registre el servidor de configuración con el servicio Site Recovery, configure el entorno de Azure de destino y configure una directiva de replicación.<br/><br/> 3. Para la implementación automatizada del servicio Mobility, cree un recurso compartido de red que contenga la frase de contraseña del servidor de configuración y los archivos de instalación del servicio Mobility.<br/><br/> 4. Cree un paquete de Configuration Manager que contenga la instalación o las actualizaciones y prepárese para la implementación del servicio Mobility.<br/><br/> 5. Entonces, puede habilitar la replicación en Azure para las máquinas que tienen instalado el servicio Mobility. | [Automatizar con Configuration Manager](#automate-with-configuration-manager).
**JetPatch** | 1. Compruebe que cumple los [requisitos previos](#prerequisites) enumerados anteriormente. <br/><br/> 2. Implemente la recuperación ante desastres mediante la configuración del entorno de origen, incluyendo la descarga e implementación de Agent Manager de JetPatch para Azure Site Recovery en su entorno de Site Recovery, mediante una plantilla OVF.<br/><br/> 2. Registre el servidor de configuración con Site Recovery, configure el entorno de Azure de destino y configure una directiva de replicación.<br/><br/> 3. Para la implementación automatizada, inicialice y complete la configuración de Agent Manager de JetPatch.<br/><br/> 4. En JetPatch, puede crear una directiva de Site Recovery para automatizar la implementación y la actualización del agente del servicio Mobility. <br/><br/> 5. Entonces, puede habilitar la replicación en Azure para las máquinas que tienen instalado el servicio Mobility. | [Automatizar con Agent Manager de JetPatch](https://jetpatch.com/microsoft-azure-site-recovery-deployment-guide/).<br/><br/> [Solución de problemas de errores de instalación del agente](https://kc.jetpatch.com/hc/articles/360035981812) en JetPatch.

## <a name="automate-with-configuration-manager"></a>Automatizar con Configuration Manager

### <a name="prepare-the-installation-files"></a>Preparar los archivos de instalación

1. Asegúrese de que cumple los requisitos previos.
2. Cree un recurso compartido de archivos de red seguro (recurso compartido de SMB) al que pueda acceder mediante la máquina que ejecuta el servidor de configuración.
3. En Configuration Manager, [clasifique los servidores](https://docs.microsoft.com/sccm/core/clients/manage/collections/automatically-categorize-devices-into-collections) en los que quiere instalar o actualizar el servicio Mobility. Una colección debe contener todos los servidores de Windows, y otra todos los servidores de Linux. 
5. En el recurso compartido de red, cree una carpeta:

    - Para la instalación en máquinas con Windows, cree una carpeta **MobSvcWindows**.
    - Para la instalación en máquinas con Linux, cree una carpeta **MobSvcLinux**.

6. Inicie sesión en la máquina del servidor de configuración.
7. Abra un símbolo del sistema de administrador en la máquina.
8. Ejecute este comando para generar el archivo de frase de contraseña:

    ```
    cd %ProgramData%\ASR\home\svsystems\bin
    genpassphrase.exe -v > MobSvc.passphrase
    ```
9. Copie el archivo MobSvc.passphrase a la carpeta de Windows y a la carpeta de Linux.
10. Ejecute este comando para navegar hasta la carpeta que contiene los archivos de instalación:

    ```
    cd %ProgramData%\ASR\home\svsystems\pushinstallsvc\repository
    ```

11. Copie estos archivos de instalación en el recurso compartido de red:

    - A **MobSvcWindows**, copie **Microsoft-ASR_UA_version_Windows_GA_date_Release.exe**
    - A **MobSvcLinux**, copie:
        - Microsoft-ASR_UA*RHEL6-64*release.tar.gz
        - Microsoft-ASR_UA*RHEL7-64*release.tar.gz
        - Microsoft-ASR_UA*SLES11-SP3-64*release.tar.gz
        - Microsoft-ASR_UA*SLES11-SP4-64*release.tar.gz
        - Microsoft-ASR_UA*OL6-64*release.tar.gz
        - Microsoft-ASR_UA*UBUNTU-14.04-64*release.tar.gz
      
12. Copie el código en las carpetas de Windows o Linux, tal y como se describe en los procedimientos siguientes. Damos por hecho que:
    - La dirección IP el servidor de configuración es 192.168.3.121.
    - El recurso compartido de archivos de red seguro es **\\\ContosoSecureFS\MobilityServiceInstallers**.

### <a name="copy-code-to-the-windows-folder"></a>Copiar código en la carpeta de Windows

Copie el siguiente código:

- Guárdelo como **install.bat** en la carpeta **MobSvcWindows**.
- Reemplace los marcadores de posición [CSIP] de este script por los valores reales de la dirección IP del servidor de configuración.
- El script es compatible con las nuevas instalaciones de los agentes del servicio Mobility y las actualizaciones de agentes ya instalados.

```DOS
Time /t >> C:\Temp\logfile.log
REM ==================================================
REM ==== Clean up the folders ========================
RMDIR /S /q %temp%\MobSvc
MKDIR %Temp%\MobSvc
MKDIR C:\Temp
REM ==================================================

REM ==== Copy new files ==============================
COPY M*.* %Temp%\MobSvc
CD %Temp%\MobSvc
REN Micro*.exe MobSvcInstaller.exe
REM ==================================================

REM ==== Extract the installer =======================
MobSvcInstaller.exe /q /x:%Temp%\MobSvc\Extracted
REM ==== Wait 10s for extraction to complete =========
TIMEOUT /t 10
REM =================================================

REM ==== Perform installation =======================
REM =================================================

CD %Temp%\MobSvc\Extracted
whoami >> C:\Temp\logfile.log
SET PRODKEY=HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall
REG QUERY %PRODKEY%\{275197FC-14FD-4560-A5EB-38217F80CBD1}
IF NOT %ERRORLEVEL% EQU 0 (
    echo "Product is not installed. Goto INSTALL." >> C:\Temp\logfile.log
    GOTO :INSTALL
) ELSE (
    echo "Product is installed." >> C:\Temp\logfile.log

    echo "Checking for Post-install action status." >> C:\Temp\logfile.log
    GOTO :POSTINSTALLCHECK
)

:POSTINSTALLCHECK
    REG QUERY "HKLM\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5" /v "PostInstallActions" | Find "Succeeded"
    If %ERRORLEVEL% EQU 0 (
        echo "Post-install actions succeeded. Checking for Configuration status." >> C:\Temp\logfile.log
        GOTO :CONFIGURATIONCHECK
    ) ELSE (
        echo "Post-install actions didn't succeed. Goto INSTALL." >> C:\Temp\logfile.log
        GOTO :INSTALL
    )

:CONFIGURATIONCHECK
    REG QUERY "HKLM\SOFTWARE\Wow6432Node\InMage Systems\Installed Products\5" /v "AgentConfigurationStatus" | Find "Succeeded"
    If %ERRORLEVEL% EQU 0 (
        echo "Configuration has succeeded. Goto UPGRADE." >> C:\Temp\logfile.log
        GOTO :UPGRADE
    ) ELSE (
        echo "Configuration didn't succeed. Goto CONFIGURE." >> C:\Temp\logfile.log
        GOTO :CONFIGURE
    )


:INSTALL
    echo "Perform installation." >> C:\Temp\logfile.log
    UnifiedAgent.exe /Role MS /InstallLocation "C:\Program Files (x86)\Microsoft Azure Site Recovery" /Platform "VmWare" /Silent
    IF %ERRORLEVEL% EQU 0 (
        echo "Installation has succeeded." >> C:\Temp\logfile.log
        (GOTO :CONFIGURE)
    ) ELSE (
        echo "Installation has failed." >> C:\Temp\logfile.log
        GOTO :ENDSCRIPT
    )

:CONFIGURE
    echo "Perform configuration." >> C:\Temp\logfile.log
    cd "C:\Program Files (x86)\Microsoft Azure Site Recovery\agent"
    UnifiedAgentConfigurator.exe  /CSEndPoint "[CSIP]" /PassphraseFilePath %Temp%\MobSvc\MobSvc.passphrase
    IF %ERRORLEVEL% EQU 0 (
        echo "Configuration has succeeded." >> C:\Temp\logfile.log
    ) ELSE (
        echo "Configuration has failed." >> C:\Temp\logfile.log
    )
    GOTO :ENDSCRIPT

:UPGRADE
    echo "Perform upgrade." >> C:\Temp\logfile.log
    UnifiedAgent.exe /Platform "VmWare" /Silent
    IF %ERRORLEVEL% EQU 0 (
        echo "Upgrade has succeeded." >> C:\Temp\logfile.log
    ) ELSE (
        echo "Upgrade has failed." >> C:\Temp\logfile.log
    )
    GOTO :ENDSCRIPT

:ENDSCRIPT
    echo "End of script." >> C:\Temp\logfile.log


```
### <a name="copy-code-to-the-linux-folder"></a>Copiar código en la carpeta de Linux

Copie el siguiente código:

- Guárdelo como **install_linux.sh** en la carpeta **MobSvcLinux**.
- Reemplace los marcadores de posición [CSIP] de este script por los valores reales de la dirección IP del servidor de configuración.
- El script es compatible con las nuevas instalaciones de los agentes del servicio Mobility y las actualizaciones de agentes ya instalados.

```Bash
#!/usr/bin/env bash

rm -rf /tmp/MobSvc
mkdir -p /tmp/MobSvc
INSTALL_DIR='/usr/local/ASR'
VX_VERSION_FILE='/usr/local/.vx_version'

echo "=============================" >> /tmp/MobSvc/sccm.log
echo `date` >> /tmp/MobSvc/sccm.log
echo "=============================" >> /tmp/MobSvc/sccm.log

if [ -f /etc/oracle-release ] && [ -f /etc/redhat-release ]; then
    if grep -q 'Oracle Linux Server release 6.*' /etc/oracle-release; then
        if uname -a | grep -q x86_64; then
            OS="OL6-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *OL6*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/redhat-release ]; then
    if grep -q 'Red Hat Enterprise Linux Server release 6.* (Santiago)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 6.* (Final)' /etc/redhat-release || \
        grep -q 'CentOS release 6.* (Final)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL6-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *RHEL6*.tar.gz /tmp/MobSvc
        fi
    elif grep -q 'Red Hat Enterprise Linux Server release 7.* (Maipo)' /etc/redhat-release || \
        grep -q 'CentOS Linux release 7.* (Core)' /etc/redhat-release; then
        if uname -a | grep -q x86_64; then
            OS="RHEL7-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *RHEL7*.tar.gz /tmp/MobSvc
                fi
    fi
elif [ -f /etc/SuSE-release ] && grep -q 'VERSION = 11' /etc/SuSE-release; then
    if grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 3' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP3-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *SLES11-SP3*.tar.gz /tmp/MobSvc
        fi
    elif grep -q "SUSE Linux Enterprise Server 11" /etc/SuSE-release && grep -q 'PATCHLEVEL = 4' /etc/SuSE-release; then
        if uname -a | grep -q x86_64; then
            OS="SLES11-SP4-64"
            echo $OS >> /tmp/MobSvc/sccm.log
            cp *SLES11-SP4*.tar.gz /tmp/MobSvc
        fi
    fi
elif [ -f /etc/lsb-release ] ; then
    if grep -q 'DISTRIB_RELEASE=14.04' /etc/lsb-release ; then
       if uname -a | grep -q x86_64; then
           OS="UBUNTU-14.04-64"
           echo $OS >> /tmp/MobSvc/sccm.log
           cp *UBUNTU-14*.tar.gz /tmp/MobSvc
       fi
    fi
else
    exit 1
fi

if [ -z "$OS" ]; then
    exit 1
fi

Install()
{
    echo "Perform Installation." >> /tmp/MobSvc/sccm.log
    ./install -q -d ${INSTALL_DIR} -r MS -v VmWare
    RET_VAL=$?
    echo "Installation Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Installation has succeeded. Proceed to configuration." >> /tmp/MobSvc/sccm.log
        Configure
    else
        echo "Installation has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

Configure()
{
    echo "Perform configuration." >> /tmp/MobSvc/sccm.log
    ${INSTALL_DIR}/Vx/bin/UnifiedAgentConfigurator.sh -i [CSIP] -P MobSvc.passphrase
    RET_VAL=$?
    echo "Configuration Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Configuration has succeeded." >> /tmp/MobSvc/sccm.log
    else
        echo "Configuration has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

Upgrade()
{
    echo "Perform Upgrade." >> /tmp/MobSvc/sccm.log
    ./install -q -v VmWare
    RET_VAL=$?
    echo "Upgrade Returncode: $RET_VAL" >> /tmp/MobSvc/sccm.log
    if [ $RET_VAL -eq 0 ]; then
        echo "Upgrade has succeeded." >> /tmp/MobSvc/sccm.log
    else
        echo "Upgrade has failed." >> /tmp/MobSvc/sccm.log
        exit $RET_VAL
    fi
}

cp MobSvc.passphrase /tmp/MobSvc
cd /tmp/MobSvc

tar -zxvf *.tar.gz

if [ -e ${VX_VERSION_FILE} ]; then
    echo "${VX_VERSION_FILE} exists. Checking for configuration status." >> /tmp/MobSvc/sccm.log
    agent_configuration=$(grep ^AGENT_CONFIGURATION_STATUS "${VX_VERSION_FILE}" | cut -d"=" -f2 | tr -d " ")
    echo "agent_configuration=$agent_configuration" >> /tmp/MobSvc/sccm.log
     if [ "$agent_configuration" == "Succeeded" ]; then
        echo "Agent is already configured. Proceed to Upgrade." >> /tmp/MobSvc/sccm.log
        Upgrade
    else
        echo "Agent is not configured. Proceed to Configure." >> /tmp/MobSvc/sccm.log
        Configure
    fi
else
    Install
fi

cd /tmp

```


### <a name="create-a-package"></a>Creación de un paquete

1. Inicie sesión en la consola de Configuration Manager > **Biblioteca de software** > **Administración de aplicaciones** > **Paquetes**.
2. Haga clic con el botón derecho en **Paquetes** > **Crear paquete**.
3. Proporcione los detalles del paquete, incluyendo el nombre, descripción, fabricante, lenguaje y versión.
4. Seleccione **Este paquete contiene archivos de origen**.
5. Haga clic en **examinar**y seleccione el recurso compartido de red y la carpeta que contiene el instalador correspondiente (MobSvcWindows o MobSvcLinux) y, a continuación, haga clic en **Siguiente**.

   ![Captura de pantalla del Asistente para crear paquetes y programas](./media/vmware-azure-mobility-install-configuration-mgr/create_sccm_package.png)

7. En la página **Elija el tipo de programa que desea crear**, seleccione **Programa estándar** > **Siguiente**.

   ![Captura de pantalla del Asistente para crear paquetes y programas](./media/vmware-azure-mobility-install-configuration-mgr/sccm-standard-program.png)

8. En la página **Especifique la información sobre este programa estándar**, especifique los valores siguientes:

    **Parámetro** | **Valor de Windows** | **Valor de Linux**
    --- | --- | ---
    **Nombre** | Instalación del Servicio de movilidad de Microsoft Azure (Windows) | Instalación del Servicio de movilidad de Microsoft Azure (Linux).
    **Línea de comandos** | install.bat | ./install_linux.sh
    **El programa se puede ejecutar** | Tanto si un usuario inició sesión como si no | Tanto si un usuario inició sesión como si no
    **Otros parámetros** | Usar el valor predeterminado | Usar el valor predeterminado

   ![Captura de pantalla del Asistente para crear paquetes y programas](./media/vmware-azure-mobility-install-configuration-mgr/sccm-program-properties.png)

9. En **Especifique los requisitos para este programa estándar**, haga lo siguiente:

    - Para máquinas con Windows, seleccione **Este programa solo puede ejecutarse en las plataformas especificadas**. A continuación seleccione los [sistemas operativos Windows compatibles](vmware-physical-azure-support-matrix.md#replicated-machines). A continuación, haga clic en **Siguiente**.
    - Para máquinas con Linux, seleccione **Este programa puede ejecutarse en cualquier plataforma**. A continuación, haga clic en **Siguiente**.
   
10. Finalice al asistente.



### <a name="deploy-the-package"></a>Implementación del paquete

1. En la consola de Configuration Manager, haga clic con el botón derecho en el paquete > **Distribuir contenido**.
   ![Captura de pantalla de la consola de Configuration Manager](./media/vmware-azure-mobility-install-configuration-mgr/sccm_distribute.png)
2. Seleccione los puntos de distribución en los que se deben copiar los paquetes. [Más información](https://docs.microsoft.com/sccm/core/servers/deploy/configure/install-and-configure-distribution-points).
3. Finalice el asistente. El paquete comienza a replicarse en los puntos de distribución especificados.
4. Cuando la distribución del paquete finalice, haga clic con el botón derecho en el paquete > **Implementar**.
   ![Captura de pantalla de la consola de Configuration Manager](./media/vmware-azure-mobility-install-configuration-mgr/sccm_deploy.png)
5. Seleccione la colección de dispositivos Windows o Linux que creó anteriormente.
6. En la página **Especifique el destino del contenido**, seleccione **Puntos de distribución**.
7. En la página **Especificar configuración para controlar cómo se implementará este software**, defina **Propósito** como **Requerido**.

   ![Captura de pantalla del Asistente para implementar software](./media/vmware-azure-mobility-install-configuration-mgr/sccm-deploy-select-purpose.png)

8. En **Especifique la programación de esta implementación**, configure una programación. [Más información](https://docs.microsoft.com/sccm/apps/deploy-use/deploy-applications#bkmk_deploy-sched).

    - El servicio Mobility se instala de acuerdo con la programación que especifique. 
    - Para evitar los reinicios innecesarios, programe la instalación del paquete durante la ventana de mantenimiento mensual o la ventana de actualizaciones de software.
9. En la página **Puntos de distribución**, configure las opciones y finalice el asistente.
10. Puede supervisar el progreso de la implementación en la consola de Configuration Manager. Vaya a **Supervisión** > **Implementaciones** >  *[nombre del paquete]* .





### <a name="uninstall-the-mobility-service"></a>Desinstalación de Mobility Service
Como en el caso de la instalación, puede crear paquetes de Configuration Manager para desinstalar Mobility Service. Para ello, use el script siguiente:

```
Time /t >> C:\logfile.log
REM ==================================================
REM ==== Check if Mob Svc is already installed =======
REM ==== If not installed no operation required ========
REM ==== Else run uninstall command =====================
REM ==== {275197FC-14FD-4560-A5EB-38217F80CBD1} is ====
REM ==== guid for Mob Svc Installer ====================
whoami >> C:\logfile.log
NET START | FIND "InMage Scout Application Service"
IF  %ERRORLEVEL% EQU 1 (GOTO :INSTALL) ELSE GOTO :UNINSTALL
:NOOPERATION
                echo "No Operation Required." >> c:\logfile.log
                GOTO :ENDSCRIPT
:UNINSTALL
                echo "Uninstall" >> C:\logfile.log
                MsiExec.exe /qn /x {275197FC-14FD-4560-A5EB-38217F80CBD1} /L+*V "C:\ProgramData\ASRSetupLogs\UnifiedAgentMSIUninstall.log"
:ENDSCRIPT

```

## <a name="next-steps"></a>Pasos siguientes
Ahora [habilite la protección](vmware-azure-enable-replication.md) de las máquinas virtuales.
