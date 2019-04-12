---
title: Solución de problemas de Azure Migrate | Microsoft Docs
description: Se proporciona información general sobre los problemas conocidos del servicio Azure Migrate, además de sugerencias para solucionar los errores comunes.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: raynew
ms.openlocfilehash: dff3c96cf3ac8eea7c1160ee1834cc70390c0333
ms.sourcegitcommit: 22ad896b84d2eef878f95963f6dc0910ee098913
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58652644"
---
# <a name="troubleshoot-azure-migrate"></a>Solución de problemas de Azure Migrate

## <a name="troubleshoot-common-errors"></a>Solución de errores comunes

[Azure Migrate](migrate-overview.md) evalúa las cargas de trabajo locales para su migración a Azure. Use este artículo para solucionar problemas al implementar y usar Azure Migrate.

### <a name="i-am-using-the-ova-that-continuously-discovers-my-on-premises-environment-but-the-vms-that-are-deleted-in-my-on-premises-environment-are-still-being-shown-in-the-portal"></a>Utilizo el archivo OVA de detección continua en el entorno local, pero las máquinas virtuales que se eliminan de mi entorno local se siguen mostrando en el portal.

La aplicación de detección continua solo recopila datos de rendimiento de forma continua, no detecta ningún cambio de configuración en el entorno local (por ejemplo, la incorporación de máquinas virtuales, la eliminación de discos o su incorporación). Si se produce un cambio de configuración en el entorno local, puede hacer lo siguiente para reflejar los cambios en el portal:

- Adición de elementos (máquinas virtuales, discos, núcleos, etc.): para reflejar estos cambios en Azure Portal, puede detener la detección desde la aplicación y después iniciarla de nuevo. Así se asegurará de que los cambios se actualizan en el proyecto de Azure Migrate.

   ![Detención de la detección](./media/troubleshooting-general/stop-discovery.png)

- Eliminación de máquinas virtuales: debido a la forma en que está diseñada la aplicación, la eliminación de las máquinas virtuales no se refleja aunque detenga e inicie la detección. Esto se debe a que los datos de las detecciones posteriores se agregan a las detecciones más antiguas y no se reemplazan. En este caso, puede simplemente omitir la máquina virtual en el portal quitándola del grupo y recalculando la valoración.

### <a name="deletion-of-azure-migrate-projects-and-associated-log-analytics-workspace"></a>Eliminación de proyectos de Azure Migrate y el área de trabajo de Log Analytics asociada

Cuando se elimina un proyecto de Azure Migrate, elimina el proyecto de migración junto con todos los grupos y valoraciones. Sin embargo, si ha vinculado un área de trabajo de Log Analytics al proyecto, no elimina automáticamente el área de trabajo de Log Analytics. Esto es porque la misma área de trabajo de Log Analytics se puede usar para varios casos de uso. Si también quiere eliminar el área de trabajo de Log Analytics, deberá hacerlo manualmente.

1. Vaya al área de trabajo de Log Analytics vinculada al proyecto.
    a. Si aún no ha eliminado el proyecto de migración, puede encontrar el vínculo al área de trabajo desde la página de información general del proyecto en la sección Essentials.

   ![Área de trabajo de LA](./media/troubleshooting-general/LA-workspace.png)

   b. Si ya ha eliminado el proyecto de migración, haga clic en **Grupos de recursos** en el panel izquierdo en Azure Portal y vaya al grupo de recursos en el que se creó el área de trabajo y, luego, desplácese hasta ella.
2. Siga las instrucciones de [este artículo](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace) para eliminar el área de trabajo.

### <a name="migration-project-creation-failed-with-error-requests-must-contain-user-identity-headers"></a>Error al crear el proyecto de migración, que indica que *las solicitudes deben contener encabezados de identidad del usuario*

Pueden experimentar este problema aquellos usuarios que no tienen acceso al inquilino de Azure Active Directory (Azure AD) de la organización. Cuando un usuario se agrega a un inquilino de Azure AD por primera vez, recibe una invitación por correo electrónico para unirse al inquilino. Los usuarios deben ir al correo electrónico y aceptar la invitación para agregarse correctamente al inquilino. Si no puede ver el correo electrónico, póngase con un usuario que ya tenga acceso al inquilino y pídale que vuelva a enviarle la invitación mediante los pasos que se indican [aquí](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator#resend-invitations-to-guest-users).

Cuando reciba el correo electrónico de invitación, debe abrirlo y hacer clic en el vínculo que contiene para aceptar la invitación. Una vez hecho esto, debe cerrar la sesión de Azure Portal e iniciarla de nuevo. La actualización del explorador no funcionará. A continuación, puede intentar crear el proyecto de migración.

### <a name="i-am-unable-to-export-the-assessment-report"></a>No puedo exportar el informe de evaluación

Si no puede exportar el informe de evaluación del portal, pruebe a usar la API REST a continuación para obtener una dirección URL de descarga de este.

1. Instale *armclient* en el equipo (si no lo tiene ya instalado):

    a. En una ventana del símbolo del sistema del administrador, ejecute el siguiente comando: ```@powershell -NoProfile -ExecutionPolicy Bypass -Command "iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"```

   b. En una ventana de Windows PowerShell del administrador, ejecute el siguiente comando: ```choco install armclient```

2. Obtenga la dirección URL para el informe de evaluación mediante la API REST de Azure Migrate.

    a.    En una ventana de Windows PowerShell del administrador, ejecute el siguiente comando: ```armclient login```

        This opens the Azure login pop-up where you need to sign in to Azure.

   b.    En la misma ventana de PowerShell, ejecute el siguiente comando para obtener la dirección URL de descarga del informe de evaluación (reemplace los parámetros URI por los valores adecuados; a continuación, un ejemplo de API de la solicitud)

       ```armclient POST https://management.azure.com/subscriptions/{subscriptionID}/resourceGroups/{resourceGroupName}/providers/Microsoft.Migrate/projects/{projectName}/groups/{groupName}/assessments/{assessmentName}/downloadUrl?api-version=2018-02-02```

      Ejemplo de solicitud y de salida:

      ```PS C:\WINDOWS\system32> armclient POST https://management.azure.com/subscriptions/8c3c936a-c09b-4de3-830b-3f5f244d72e9/r
   esourceGroups/ContosoDemo/providers/Microsoft.Migrate/projects/Demo/groups/contosopayroll/assessments/assessment_11_16_2
   018_12_16_21/downloadUrl?api-version=2018-02-02
   {
   "assessmentReportUrl": "https://migsvcstoragewcus.blob.core.windows.net/4f7dddac-f33b-4368-8e6a-45afcbd9d4df/contosopayrollassessment_11_16_2018_12_16_21?sv=2016-05-31&sr=b&sig=litQmHuwi88WV%2FR%2BDZX0%2BIttlmPMzfVMS7r7dULK7Oc%3D&st=2018-11-20T16%3A09%3A30Z&se=2018-11-20T16%3A19%3A30Z&sp=r",
   "expirationTime": "2018-11-20T22:09:30.5681954+05:30"```

3. Copie la dirección URL de la respuesta y ábrala en un explorador para descargar el informe de evaluación.

4. Una vez descargado el informe, puede utilizar Excel para buscar la carpeta descargada y abrir el archivo en Excel para verlo.

### <a name="performance-data-for-cpu-memory-and-disks-is-showing-up-as-zeroes"></a>Datos de rendimiento de CPU, memoria y discos aparecen como ceros

Azure Migrate perfiles continuamente el entorno local para recopilar datos de rendimiento de las máquinas virtuales de un entorno local. Si acaba de iniciar la detección de su entorno, deberá esperar al menos un día para la recopilación de datos de rendimiento a realizarse. Si se crea una evaluación sin tener que esperar durante un día, las métricas de rendimiento se mostrarán como ceros. Después de esperar durante un día, puede crear una nueva evaluación o actualizar la evaluación existente mediante la opción "Recalcular" en el informe de evaluación.

### <a name="i-specified-an-azure-geography-while-creating-a-migration-project-how-do-i-find-out-the-exact-azure-region-where-the-discovered-metadata-would-be-stored"></a>Especifiqué una ubicación geográfica de Azure al crear un proyecto de migración, ¿cómo puedo saber en qué región exacta de Azure se almacenarían los metadatos detectados?

Puede ir a la sección **Essentials** en la página **Introducción** del proyecto para identificar la ubicación exacta donde se almacenan los metadatos. Azure Migrate selecciona aleatoriamente la ubicación dentro de la zona geográfica y no es posible modificarla. Si quiere crear un proyecto solo en una región específica, puede usar las API de REST para crear el proyecto de migración y pasar la región que quiera.

   ![Ubicación del proyecto](./media/troubleshooting-general/geography-location.png)

## <a name="collector-issues"></a>Problemas del recopilador

### <a name="deployment-of-azure-migrate-collector-failed-with-the-error-the-provided-manifest-file-is-invalid-invalid-ovf-manifest-entry"></a>La implementación de Azure Migrate Collector produjo el siguiente error: The provided manifest file is invalid: Invalid OVF manifest entry (El archivo de manifiesto proporcionado no es válido: entrada de manifiesto OVF no válida).

1. Compruebe si el archivo OVA de Azure Migrate Collector se descarga correctamente mediante la comprobación del valor hash. Consulte este [artículo](https://docs.microsoft.com/azure/migrate/tutorial-assessment-vmware#verify-the-collector-appliance) para comprobar el valor hash. Si el valor de hash no coincide, vuelva a descargar el archivo OVA e intente de nuevo la implementación.
2. Si sigue sin funcionar y utiliza el cliente de VMware vSphere para implementar el OVF, intente implementarlo mediante el cliente web de vSphere. Si sigue sin funcionar, intente usar otro explorador web.
3. Si está usando el cliente web de vSphere e intenta implementarlo en vCenter Server 6.5 o 6.7, pruebe a implementar el archivo OVA directamente en el host ESXi siguiendo estos pasos:
   - Conéctese directamente al host ESXi (en lugar de vCenter Server) mediante el cliente web (https://<*dirección IP del host*>/ui)
   - Vaya a Inicio > Inventario
   - Haga clic en Archivo > Deploy OVF template (Implementar plantilla OVF) > Busque el archivo OVA y complete la implementación.
4. Si la implementación sigue sin funcionar, póngase en contacto con el soporte técnico de Azure Migrate.

### <a name="unable-to-select-the-azure-cloud-in-the-appliance-fails-with-error-azure-cloud-selection-failed"></a>No se pudieron seleccionar Azure en la nube en el dispositivo, se produce un error con el error "Error al seleccionar la nube de Azure"

Se trata de un problema conocido y una corrección está disponible para el problema. Descargue el [última actualización bits](https://docs.microsoft.com/azure/migrate/concepts-collector-upgrade#continuous-discovery-upgrade-versions) para el dispositivo y el dispositivo para aplicar la corrección de actualización.

### <a name="collector-is-not-able-to-connect-to-the-internet"></a>El recopilador no es capaz de conectarse a Internet

Esto puede ocurrir cuando la máquina que está utilizando está detrás de un proxy. Asegúrese de proporcionar las credenciales de autorización en caso de que el proxy las necesite.
Si va a usar un proxy de firewall basado en direcciones URL para controlar la conectividad saliente, asegúrese de incluir en la lista de permitidos estas direcciones URL necesarias:

**URL** | **Propósito**  
--- | ---
*.portal.azure.com | Se requiere comprobar la conectividad con el servicio de Azure y validar los problemas de sincronización horaria.
*.oneget.org | Se requiere para descargar el módulo PowerCLI de vCenter basado en PowerShell.

**El recopilador no se puede conectar a Internet debido a un error de validación del certificado**

Esto puede ocurrir si usa un proxy de interceptación para conectarse a Internet, y si no ha importado el certificado de proxy en la máquina virtual del recopilador. Puede importar el certificado de proxy mediante los pasos detallados [aquí](https://docs.microsoft.com/azure/migrate/concepts-collector).

**El recopilador no se puede conectar al proyecto con el identificador de proyecto y la clave que copié del portal**

Asegúrese de haber copiado y pegado la información correcta. Para solucionar problemas, instale Microsoft Monitoring Agent (MMA) y compruebe si el MMA puede conectarse al proyecto como se indica a continuación:

1. Descargue [MMA](https://go.microsoft.com/fwlink/?LinkId=828603) en la máquina virtual del recopilador.
2. Para iniciar la instalación, haga doble clic en el archivo descargado.
3. En la **principal** que aparece al iniciar la instalación, haga clic en **Siguiente**. En la página **Términos de licencia**, haga clic en **Acepto** para aceptar la licencia.
4. En **Carpeta de destino**, mantenga o modifique la carpeta de instalación predeterminada y después haga clic en **Siguiente**.
5. En **Opciones de instalación del agente**, seleccione **Azure Log Analytics** > **Siguiente**.
6. Haga clic en **Agregar** para agregar un área de trabajo de Log Analytics nueva. Pegue el identificador de proyecto y la clave que copió. A continuación, haga clic en **Siguiente**.
7. Verifique que el agente puede conectarse al proyecto. Si no puede, verifique la configuración. Si el agente puede conectarse, pero el recopilador no, póngase en contacto con el servicio de soporte técnico.


### <a name="error-802-date-and-time-synchronization-error"></a>Error 802: error de sincronización de fecha y hora

El reloj del servidor podría estar desincronizado con la hora actual en más de cinco minutos. Cambie la hora del reloj de la máquina virtual del recopilador para que coincida con la hora actual; para ello, siga estos pasos:

1. Abra un símbolo del sistema de administrador en la máquina virtual.
2. Para comprobar la zona horaria, ejecute w32tm /tz.
3. Para sincronizar la hora, ejecute w32tm /resync.

### <a name="vmware-powercli-installation-failed"></a>Error en la instalación de VMware PowerCLI

El recopilador de Azure Migrate descarga PowerCLI y lo instala en el dispositivo. Un error en la instalación de PowerCLI podría deberse a puntos de conexión inaccesibles para el repositorio de PowerCLI. Para solucionar problemas, intente instalar PowerCLI manualmente en la máquina virtual del recopilador con el paso siguiente:

1. Abra Windows PowerShell en modo de administrador.
2. Vaya al directorio C:\ProgramFiles\ProfilerService\VMWare\Scripts\
3. Ejecute el script InstallPowerCLI.ps1.

### <a name="error-unhandledexception-internal-error-occurred-systemiofilenotfoundexception"></a>Se produjo un error interno UnhandledException: System.IO.FileNotFoundException

Este problema podría producirse debido a un problema con la instalación de VMware PowerCLI. Para solucionar el problema, siga estos pasos:

1. Si no tiene la versión más reciente del dispositivo del recopilador, [actualice el recopilador a la versión más reciente](https://aka.ms/migrate/col/checkforupdates) y compruebe si el problema se resuelve.
2. Si ya tiene la versión más reciente del recopilador, siga los pasos siguientes para realizar una instalación limpia de PowerCLI:

    a. Cierre el explorador web en el dispositivo.

   b. Detenga el servicio "Azure Migrate Collector" yendo al Administrador de servicios de Windows (abra "Ejecutar" y escriba services.msc para abrir el Administrador de servicios de Windows). Haga clic con el botón derecho en el servicio Azure Migrate Collector y haga clic en Detener.

   c. Elimine todas las carpetas a partir de "VMware" de las ubicaciones siguientes: C:\Archivos de programa\WindowsPowerShell\Modules  
        C:\Archivos de programa (x86)\WindowsPowerShell\Modules

   d. Reinicie el servicio "Azure Migrate Collector" en el Administrador de servicios de Windows (abra "Ejecutar" y escriba services.msc para abrir el Administrador de servicios de Windows). Haga clic con el botón derecho en el servicio Azure Migrate Collector y haga clic en Iniciar.

   e. Haga doble clic en el acceso directo del escritorio "Ejecutar recopilador" para iniciar la aplicación del recopilador. La aplicación del recopilador automáticamente debe descargar e instalar la versión necesaria de PowerCLI.

3. Si los pasos anteriores no resuelven el problema, siga los pasos de una a c anterior y, a continuación, instale manualmente PowerCLI en el dispositivo siguiendo estos pasos:

    a. Limpiar PowerCLI incompleta todos los archivos de instalación siguiendo los pasos #a #c en el paso #2 anterior.

   b. Vaya a Inicio > Ejecutar > PowerShell(x86) abiertos de Windows en modo de administrador

   c. Ejecute el comando:  Install-Module "VMWare.VimAutomation.Core" - RequiredVersion "6.5.2.6234650" (tipo "A" cuando pide confirmación)

   d. Reinicie el servicio "Azure Migrate Collector" en el Administrador de servicios de Windows (abra "Ejecutar" y escriba services.msc para abrir el Administrador de servicios de Windows). Haga clic con el botón derecho en el servicio Azure Migrate Collector y haga clic en Iniciar.

   e. Haga doble clic en el acceso directo del escritorio "Ejecutar recopilador" para iniciar la aplicación del recopilador. La aplicación del recopilador automáticamente debe descargar e instalar la versión necesaria de PowerCLI.

4. Si no puede descargar el módulo en el dispositivo debido a problemas de firewall, descargue e instale el módulo en un equipo que tenga acceso a internet mediante los pasos siguientes:

     a. Limpiar PowerCLI incompleta todos los archivos de instalación siguiendo los pasos #a #c en el paso #2 anterior.

    b. Vaya a Inicio > Ejecutar > PowerShell(x86) abiertos de Windows en modo de administrador

    c. Ejecute el comando:  Install-Module "VMWare.VimAutomation.Core" - RequiredVersion "6.5.2.6234650" (tipo "A" cuando pide confirmación)

    d. Copie todos los módulos, comenzando con "VMware" desde "C:\Program Files (x86) \WindowsPowerShell\Modules" en la misma ubicación en la máquina virtual del recopilador.

    e. Reinicie el servicio "Azure Migrate Collector" en el Administrador de servicios de Windows (abra "Ejecutar" y escriba services.msc para abrir el Administrador de servicios de Windows). Haga clic con el botón derecho en el servicio Azure Migrate Collector y haga clic en Iniciar.

    f. Haga doble clic en el acceso directo del escritorio "Ejecutar recopilador" para iniciar la aplicación del recopilador. La aplicación del recopilador automáticamente debe descargar e instalar la versión necesaria de PowerCLI.

### <a name="error-unabletoconnecttoserver"></a>Error UnableToConnectToServer

No se puede conectar con vCenter Server "Servername.com:9443" por el siguiente error: There was no endpoint listening at https://Servername.com:9443/sdk that could accept the message (No había ningún punto de conexión escuchando en https://Servername.com:9443/sdk que pudiera aceptar el mensaje).

Compruebe si está ejecutando la versión más reciente del dispositivo recopilador; de lo contrario, actualice el dispositivo a la [última versión](https://docs.microsoft.com/azure/migrate/concepts-collector).

Si el problema persiste con la última versión, es posible que el equipo recopilador no pueda resolver el nombre de vCenter Server especificado o que el puerto especificado sea incorrecto. De forma predeterminada, si no se especifica el puerto, el recopilador intentará conectarse el puerto número 443.

1. Intente hacer ping a Servername.com desde el equipo recopilador.
2. Si se produce un error en el paso 1, intente conectarse al servidor de vCenter a través de la dirección IP.
3. Identifique el número de puerto correcto para conectarse a vCenter.
4. Por último, compruebe si el servidor de vCenter está en funcionamiento.

### <a name="antivirus-exclusions"></a>Exclusiones del antivirus

Para reforzar el dispositivo Azure Migrate, deberá excluir el análisis antivirus de las carpetas siguientes en el dispositivo:

- La carpeta que contiene los archivos binarios del servicio Azure Migrate. Excluya todas las subcarpetas.
  %ProgramFiles%\ProfilerService  
- La aplicación web de Azure Migrate. Excluya todas las subcarpetas.
  %SystemDrive%\inetpub\wwwroot
- La caché local de la base de datos y los archivos de registro. El servicio Azure Migrate necesita acceso RW a esta carpeta.
  %SystemDrive%\Profiler

## <a name="dependency-visualization-issues"></a>Problemas de visualización de dependencia

### <a name="i-am-unable-to-find-the-dependency-visualization-functionality-for-azure-government-projects"></a>No puedo encontrar la funcionalidad de visualización de dependencia para proyectos de Azure Government.

Azure Migrate depende de Service Map para la funcionalidad de visualización de dependencia y puesto que Service Map no está disponible actualmente en Azure Government, esta funcionalidad no está disponible en Azure Government.

### <a name="i-installed-the-microsoft-monitoring-agent-mma-and-the-dependency-agent-on-my-on-premises-vms-but-the-dependencies-are-now-showing-up-in-the-azure-migrate-portal"></a>He instalado Microsoft Monitoring Agent (MMA) y el agente de dependencias en las máquinas virtuales locales, pero las dependencias no se muestran en el portal de Azure Migrate.

Después de instalar los agentes, Azure Migrate suele tardar entre 15 y 30 minutos en mostrar las dependencias en el portal. Si ha esperado durante más de 30 minutos, asegúrese de que el agente de MMA es capaz de comunicarse con el área de trabajo de OMS. Para ello, siga estos pasos:

Para máquinas virtuales con Windows 10:
1. En el **Panel de Control**, abra **Microsoft Monitoring Agent**.
2. Vaya a la pestaña **Azure Log Analytics (OMS)** en la ventana emergente de propiedades de MMA.
3. Asegúrese de que el **Estado** del área de trabajo está en verde.
4. Si el estado no está en verde, intente quitar el área de trabajo y vuelva a agregarla al MMA.
        ![Estado del MMA](./media/troubleshooting-general/mma-status.png)

Para máquinas virtuales con Linux, asegúrese de que se hayan completado correctamente los comandos de instalación de MMA y el agente de dependencias.

### <a name="what-are-the-operating-systems-supported-by-mma"></a>¿Cuáles son los sistemas operativos compatibles con MMA?

[Aquí](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems) tiene una lista de los sistemas operativos Windows compatibles con MMA.
La lista de los sistemas operativos de Linux compatibles con MMA la encontrará [aquí](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="what-are-the-operating-systems-supported-by-dependency-agent"></a>¿Cuáles son los sistemas operativos compatibles con el agente de dependencias?

[Aquí](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems) tiene una lista de los sistemas operativos Windows compatibles con el agente de dependencias.
La lista de los sistemas operativos de Linux compatibles con el agente de dependencias la encontrará [aquí](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems).

### <a name="i-am-unable-to-visualize-dependencies-in-azure-migrate-for-more-than-one-hour-duration"></a>No puedo visualizar las dependencias de Azure Migrate durante más de una hora.
Azure Migrate le permite visualizar las dependencias durante un máximo de una hora. Aunque Azure Migrate le permite volver atrás a una fecha concreta en el historial de hasta el último mes, el tiempo máximo durante el que se pueden visualizar las dependencias es de una hora. Por ejemplo, puede usar la característica de duración de tiempo en el mapa de dependencias para ver las dependencias de ayer, pero solo puede verlas durante un período de una hora. Sin embargo, puede usar los registros de Azure Monitor para [consultar los datos de dependencia](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) durante un período más largo.

### <a name="i-am-unable-to-visualize-dependencies-for-groups-with-more-than-10-vms"></a>No puedo visualizar las dependencias para grupos con más de 10 máquinas virtuales.
Puede [visualizar las dependencias de grupos](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) que tengan un máximo de diez máquinas virtuales. Si tiene un grupo con más de diez, recomendamos dividirlo en grupos más pequeños y visualizar las dependencias.

### <a name="i-installed-agents-and-used-the-dependency-visualization-to-create-groups-now-post-failover-the-machines-show-install-agent-action-instead-of-view-dependencies"></a>He instalado agentes y he usado la visualización de dependencias para crear grupos. Ahora, después de la conmutación por error, los equipos muestran la acción "Instalar agente" en lugar de "Ver dependencias"
* Tras una conmutación por error planeada o no planeada, los equipos locales se desactivan y los equipos equivalentes se activan en Azure. Estos equipos adquieren una dirección MAC diferente. Pueden adquirir una dirección IP distinta en función de si el usuario elige conservar o no la dirección IP local. Si las direcciones IP y MAC difieren, Azure Migrate no asocia los equipos locales con ningún dato de dependencia de Service Map y solicita al usuario que instale los agentes en lugar de visualizar las dependencias.
* Después de la conmutación por error de prueba, los equipos locales permanecen encendidos según lo previsto. Los equipos equivalentes que se activan en Azure adquieren una dirección MAC distinta y pueden adquirir una dirección IP diferente. A menos que los bloques de usuario saliente Azure Monitor registra el tráfico procedente de estos equipos, Azure Migrate no asocia las máquinas locales con ningún dato de dependencia de Service Map y solicita al usuario que instale a los agentes en lugar de visualizar las dependencias.

## <a name="troubleshoot-azure-readiness-issues"></a>Solución de problemas de preparación de Azure

**Problema** | **Revisión**
--- | ---
Tipo de arranque no compatible | Azure no admite máquinas virtuales con el tipo de arranque EFI. Se recomienda convertir el tipo de arranque a BIOS antes de ejecutar una migración. <br/><br/>Puede usar [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/tutorial-migrate-on-premises-to-azure) para realizar la migración de las máquinas virtuales de este tipo, dado que durante la migración el tipo de arranque de la máquina virtual se convertirá a BIOS.
Sistema operativo Windows admitido con condiciones | El sistema operativo alcanzó la fecha de finalización del soporte técnico y necesita un contrato de soporte técnico personalizado (CSA) para [recibir soporte técnico en Azure](https://aka.ms/WSosstatement). Considere la posibilidad de actualizar el sistema operativo antes de migrar a Azure.
Sistema operativo Windows no admitido | Azure solo admite [algunas versiones del sistema operativo Windows](https://aka.ms/WSosstatement). Considere la posibilidad de actualizar el sistema operativo de la máquina antes de migrar a Azure.
Sistema operativo Linux aprobado con condiciones | Azure solo aprueba [algunas versiones del sistema operativo Linux](../virtual-machines/linux/endorsed-distros.md). Considere la posibilidad de actualizar el sistema operativo de la máquina antes de migrar a Azure.
Sistema operativo Linux no aprobado | La máquina se puede arrancar en Azure, pero Azure no proporciona compatibilidad con ningún sistema operativo. Considere la posibilidad de actualizar el sistema operativo a una [versión Linux aprobada](../virtual-machines/linux/endorsed-distros.md) antes de migrar a Azure
Sistema operativo desconocido | El sistema operativo de la máquina virtual se especificó como "Otro" en vCenter Server, debido a que Azure Migrate no puede identificar la preparación para Azure de la máquina virtual. Asegúrese de que el sistema operativo que se ejecuta dentro de la máquina se [admite](https://aka.ms/azureoslist) en Azure antes de migrar la máquina.
Valor de bits de sistema operativo no aprobado | Las máquinas virtuales con sistemas operativos de 32 bits pueden arrancar en Azure; sin embargo, se recomienda actualizar el sistema operativo de la máquina virtual a 64 bits antes de migrar a Azure.
Requiere una suscripción a Visual Studio | Las máquinas tienen un sistema operativo cliente de Windows que se ejecuta en su interior, que solo se admite en suscripciones de Visual Studio.
No se encontró ninguna máquina virtual para el rendimiento de almacenamiento requerido | El rendimiento de almacenamiento (IOPS/rendimiento) requerido para la máquina excede el soporte técnico de máquina virtual de Azure. Reduzca los requisitos de almacenamiento de la máquina antes de realizar la migración.
No se encontró ninguna máquina virtual para el rendimiento de red requerido | El rendimiento de red (entrada/salida) requerido para la máquina excede el soporte técnico de máquina virtual de Azure. Reduzca los requisitos de red de la máquina.
No se encontró la máquina virtual en la ubicación especificada | Utilice una ubicación de destino diferente antes de la migración.
Uno o varios discos no son adecuados | Uno o varios discos conectados a la VM no cumplen los requisitos de Azure. Para cada disco conectado a la VM, compruebe que el tamaño del disco sea inferior a 4 TB. De lo contrario, redúzcalo antes de migrar a Azure. Asegúrese de que el rendimiento (IOPS/rendimiento) necesario para cada disco sea compatible con los [discos de máquinas virtuales administradas](https://docs.microsoft.com/azure/azure-subscription-service-limits#storage-limits) de Azure.   
Uno o varios adaptadores de red no son adecuados | Quite los adaptadores de red no utilizados de la máquina antes de realizar la migración.
El recuento del disco supera el límite | Quite los discos no utilizados del equipo antes de la migración.
El tamaño del disco supera el límite | Azure admite discos con un tamaño de hasta 4 TB. Reduzca los discos a un tamaño inferior a 4 TB antes de la migración.
El disco no está disponible en la ubicación especificada | Asegúrese de que el disco está en la ubicación de destino antes de realizar la migración.
El disco no está disponible para la redundancia especificada | El disco debe usar el tipo de almacenamiento de redundancia definido en la configuración de evaluación (LRS de forma predeterminada).
No se pudo determinar la idoneidad del disco debido a un error interno | Intente crear una evaluación para el grupo.
No se encontró ninguna máquina virtual con los núcleos y memoria requeridos | Azure no pudo ajustar un tipo de máquina virtual adecuado. Reduzca la memoria y el número de núcleos de la máquina local antes de realizar la migración.
No se pudo determinar la idoneidad de la máquina virtual debido a un error interno | Intente crear una evaluación para el grupo.
No se pudo determinar la idoneidad de uno o varios discos debido a un error interno | Intente crear una evaluación para el grupo.
No se pudo determinar la idoneidad de uno o varios adaptadores de red debido a un error interno | Intente crear una evaluación para el grupo.


## <a name="collect-logs"></a>Recopilación de registros

**¿Cómo puedo recopilar registros en la máquina virtual del recopilador?**

El registro está habilitado de manera predeterminada. La ubicación de los registros es la siguiente:

- C:\Profiler\ProfilerEngineDB.sqlite
- C:\Profiler\Service.log
- C:\Profiler\WebApp.log

Para recopilar el Seguimiento de eventos para Windows, haga lo siguiente:

1. En la máquina virtual del recopilador, abra una ventana de comandos de PowerShell.
2. Ejecute **Get-EventLog -LogName Application | export-csv eventlog.csv**.

**¿Cómo puedo recopilar registros de tráfico de red del portal?**

1. Abra el explorador, vaya al [portal](https://portal.azure.com) e inicie sesión en él.
2. Presione F12 para iniciar las Herramientas de desarrollo. Si es necesario, desactive la configuración **Clear entries on navigation** (Borrar entradas de navegación).
3. Haga clic en la pestaña **Network** (Red) para empezar a capturar el tráfico de red:
   - En Chrome, seleccione **Preserve log** (Conservar registro). La grabación debe iniciarse automáticamente. Un círculo rojo indica que la captura del tráfico está en curso. Si no aparece, haga clic en el círculo negro para iniciar la captura.
   - En Microsoft Edge/IE, la grabación debe iniciarse automáticamente. Si no es así, haga clic en el botón de reproducción de color verde.
4. Pruebe a reproducir el error.
5. Una vez haya encontrado el error durante la grabación, detenga la grabación y guarde una copia de la actividad grabada:
   - En Chrome, haga clic con el botón derecho y haga clic en **Save as HAR with content** (Guardar como HAR con contenido). Con esta acción, los registros se comprimen y exportan como un archivo .har.
   - En Microsoft Edge/IE, haga clic en el icono **Exportar el tráfico capturado**. Con esta acción, el registro se comprime y exporta.
6. Navegue a la pestaña **Console** (Consola) para ver si hay alguna advertencia o error. Para guardar el registro de la consola:
   - En Chrome, haga clic con el botón derecho en cualquier lugar en el registro de la consola. Seleccione **Save as** (Guardar como) para exportar y comprimir el registro.
   - En Microsoft Edge/IE, haga clic con el botón derecho sobre los errores y seleccione **Copy all** (Copiar todo).
7. Cierre las Herramientas de desarrollo.

## <a name="collector-error-codes-and-recommended-actions"></a>Códigos de error del recopilador y acciones recomendadas

| Código de error | Nombre del error   | Message   | Causas posibles | Acción recomendada  |
| --- | --- | --- | --- | --- |
| 601       | CollectorExpired               | El recopilador expiró.                                                        | Recopilador expirado.                                                                                    | Descargue una versión más reciente del recopilador y vuelva a intentarlo.                                                                                      |
| 751       | UnableToConnectToServer        | No se puede conectar con vCenter Server "%Name;" debido al siguiente error: %ErrorMessage;.     | Para obtener más detalles, consulte el mensaje de error.                                                             | Resuelva el problema y vuelva a intentarlo.                                                                                                           |
| 752       | InvalidvCenterEndpoint         | El servidor "%Name;" no es ninguna instancia de vCenter Server.                                  | Proporcione los detalles de vCenter Server.                                                                       | Vuelva a intentar la operación con los detalles correctos de vCenter Server.                                                                                   |
| 753       | InvalidLoginCredentials        | No se puede conectar con vCenter Server "%Name;" debido al siguiente error: %ErrorMessage;. | Se produjo un error de conexión a vCenter Server porque las credenciales de inicio de sesión no son válidas.                             | Asegúrese de que las credenciales de inicio de sesión proporcionadas sean correctas.                                                                                    |
| 754       | NoPerfDataAvailable           | Los datos de rendimiento no están disponibles.                                               | Compruebe el nivel de estadísticas en vCenter Server. Se debe establecer en 3 para que los datos de rendimiento estén disponibles. | Cambie el nivel de estadísticas a 3 (para una duración de 5 minutos, 30 minutos y 2 horas) y vuelva a intentarlo después de esperar, al menos, un día.                   |
| 756       | NullInstanceUUID               | Se encontró una máquina con un valor de InstanceUUID null                                  | Puede que vCenter Server tenga un objeto inadecuado.                                                      | Resuelva el problema y vuelva a intentarlo.                                                                                                           |
| 757       | VMNotFound                     | No se encuentra la máquina virtual                                                  | La máquina virtual se puede eliminar: %VMID;                                                                | Asegúrese de que las máquinas virtuales seleccionadas al definir el ámbito del inventario de vCenter existan durante la detección.                                      |
| 758       | GetPerfDataTimeout             | Se agotó el tiempo de espera de la solicitud de VCenter. Mensaje %Message;                                  | Las credenciales de vCenter Server no son correctas.                                                              | Compruebe las credenciales de vCenter Server y asegúrese de que vCenter Server esté accesible. Vuelva a intentar la operación. Si el problema persiste, póngase en contacto con el soporte técnico. |
| 759       | VmwareDllNotFound              | No se encontró el archivo DLL VMWare.Vim.                                                     | PowerCLI no está instalado correctamente.                                                                   | Compruebe si PowerCLI está instalado correctamente. Vuelva a intentar la operación. Si el problema persiste, póngase en contacto con el soporte técnico.                               |
| 800       | ServiceError                   | El servicio Azure Migrate Collector no se está ejecutando.                               | El servicio Azure Migrate Collector no se está ejecutando.                                                       | Use services.msc para iniciar el servicio y vuelva a intentar la operación.                                                                             |
| 801       | PowerCLIError                  | Error en la instalación de PowerCLI de VMware.                                          | Error en la instalación de PowerCLI de VMware.                                                                  | Vuelva a intentar la operación. Si el problema persiste, instálelo manualmente y vuelva a intentar la operación.                                                   |
| 802       | TimeSyncError                  | La hora no está sincronizada con la del servidor horario de Internet.                            | La hora no está sincronizada con la del servidor horario de Internet.                                                    | Asegúrese de que la hora del reloj esté bien definida para la zona horaria de la máquina y vuelva a intentar la operación.                                 |
| 702       | OMSInvalidProjectKey           | Se especificó una clave de proyecto no válida.                                                | Se especificó una clave de proyecto no válida.                                                                        | Reintente la operación con la clave de proyecto correcta.                                                                                              |
| 703       | OMSHttpRequestException        | Error durante el envío de la solicitud. Mensaje %Message;                                | Compruebe el id. y la clave del proyecto para asegurarse de que se puede acceder al punto de conexión.                                       | Vuelva a intentar la operación. Si el problema persiste, póngase en contacto con el Soporte técnico de Microsoft.                                                                     |
| 704       | OMSHttpRequestTimeoutException | Se agotó el tiempo de espera de la solicitud HTTP. Mensaje %Message;                                     | Compruebe el id. y la clave del proyecto para asegurarse de que se puede acceder al punto de conexión.                                       | Vuelva a intentar la operación. Si el problema persiste, póngase en contacto con el Soporte técnico de Microsoft.                                                                     |
