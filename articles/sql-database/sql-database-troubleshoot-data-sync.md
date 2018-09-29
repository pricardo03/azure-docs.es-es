---
title: Solución de problemas de Azure SQL Data Sync | Microsoft Docs
description: Aprenda a solucionar problemas comunes de Azure SQL Data Sync.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: data sync
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: douglasl
manager: craigg
ms.date: 07/16/2018
ms.openlocfilehash: 0f836a857d6f9748416fda1526a1957af4fc51e4
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47163603"
---
# <a name="troubleshoot-issues-with-sql-data-sync"></a>Solución de problemas de SQL Data Sync

En este artículo se describe cómo solucionar problemas conocidos de Azure SQL Data Sync. Si hay una solución para un problema, la encontrará aquí.

Para obtener información general acerca de SQL Data Sync, consulte [Sincronización de datos entre varias bases de datos locales y de la nube con Azure SQL Data Sync](sql-database-sync-data.md).

## <a name="sync-issues"></a>Problemas de sincronización

- [Se produce un error de sincronización en la interfaz de usuario del portal para las bases de datos locales asociadas al agente cliente](#sync-fails)

- [Mi grupo de sincronización está atascado en el estado de procesamiento](#sync-stuck)

- [Veo datos erróneos en las tablas](#sync-baddata)

- [Tras una sincronización correcta, veo datos clave principales incoherentes](#sync-pkdata)

- [Veo una degradación significativa del rendimiento](#sync-perf)

- [He recibido el mensaje "No se puede insertar el valor NULL en la columna <column>. La columna no admite valores NULL." ¿Qué significa esto y cómo puedo corregir este error?](#sync-nulls)

- [¿Cómo trata Data Sync las referencias circulares? En otras palabras, ¿cuándo se sincronizan los mismos datos en varios grupos de sincronización y siguen cambiando como resultado?](#sync-circ)

### <a name="sync-fails"></a> Se produce un error de sincronización en la interfaz de usuario del portal para las bases de datos locales asociadas al agente cliente

Se produce un error de SQL Data Sync en la interfaz de usuario del portal para las bases de datos locales asociadas al agente cliente. En el registro de eventos del equipo local que ejecuta el agente, verá los errores System.IO.IOException. Estos errores indican que el disco no tiene espacio suficiente.

- **Causa**. La unidad no tiene espacio suficiente.

- **Resolución**. Cree más espacio en la unidad en la que se encuentra el directorio %TEMP%.

### <a name="sync-stuck"></a> Mi grupo de sincronización está atascado en el estado de procesamiento

Un grupo de sincronización de SQL Data Sync lleva mucho tiempo en estado de procesamiento. No responde al comando **stop** y los registros no muestran ninguna entrada nueva.

Cualquiera de las condiciones siguientes puede hacer que un grupo de sincronización se quede atascado en el estado de procesamiento:

- **Causa**. El agente cliente no tiene conexión

- **Resolución**. Compruebe que el agente cliente está en línea y vuelva a intentarlo.

- **Causa**. El agente cliente se ha desinstalado o no está presente.

- **Resolución**. Si el agente cliente se ha desinstalado o no está presente, haga lo siguiente:

    1. Si el archivo XML del agente existe, quítelo de la carpeta de instalación de SQL Data Sync.
    1. Instale el agente en un equipo local (puede ser el mismo o un equipo diferente). A continuación, envíe la clave del agente que se genera en el portal para el agente que se muestra como sin conexión.

- **Causa**. El servicio SQL Data Sync se detiene.

- **Resolución**. Reinicie el servicio SQL Data Sync.

    1. En el menú **Inicio**, busque **Servicios**.
    1. En los resultados de la búsqueda, haga clic en **Servicios**.
    1. Busque el servicio **SQL Data Sync**.
    1. Si el estado del servicio es **Detenido**, haga clic con el botón derecho en el nombre del servicio y, después, seleccione **Iniciar**.

> [!NOTE]
> Si la información anterior no hace que el grupo de sincronización salga del estado de procesamiento, el Soporte técnico de Microsoft puede restablecer el estado del grupo de sincronización. Para restablecer el estado del grupo de sincronización, publique un mensaje en el [foro de Azure SQL Database](https://social.msdn.microsoft.com/Forums/azure/home?forum=ssdsgetstarted). En este mensaje, incluya el identificador de suscripción y el identificador del grupo de sincronización que es necesario reiniciar. Un ingeniero de Soporte técnico de Microsoft responderá a su mensaje y le informará cuando se haya restablecido el estado.

### <a name="sync-baddata"></a> Veo datos erróneos en las tablas

Si se incluyen en una sincronización tablas que tienen el mismo nombre pero que son de esquemas de base de datos diferentes, verá datos erróneos en las tablas después de la sincronización.

- **Causa**. El proceso de aprovisionamiento de SQL Data Sync usa las mismas tablas de seguimiento para las tablas que tienen el mismo nombre, pero que están en esquemas distintos. Por este motivo, los cambios de ambas tablas se reflejan en la misma tabla de seguimiento. Esto provoca cambios de datos erróneos durante la sincronización.

- **Resolución**. Asegúrese de que los nombres de las tablas implicadas en una sincronización sean diferentes, aunque las tablas pertenezcan a esquemas de base de datos distintos.

### <a name="sync-pkdata"></a> Tras una sincronización correcta, veo datos clave principales incoherentes

Una sincronización se notifica como correcta, y en su registro no se muestran filas con errores ni omitidas, pero observa que los datos clave principales son incoherentes entre las bases de datos del grupo de sincronización.

- **Causa**. Este resultado es así por naturaleza. Cualquier cambio realizado en una clave principal tiene como resultado datos incoherentes en las filas en las que se ha cambiado la clave principal.

- **Resolución**. Para evitar este problema, asegúrese de que no se modifica ningún dato en la columna de clave principal. Para corregir este problema después de que se haya producido, elimine la fila que tiene datos incoherentes de todos los puntos de conexión del grupo de sincronización. A continuación, vuelva a insertar la fila.

### <a name="sync-perf"></a> Veo una degradación significativa del rendimiento

El rendimiento disminuye considerablemente, posiblemente hasta tal punto que ni siquiera puede abrir la interfaz de usuario de Data Sync.

- **Causa**. La causa más probable es que exista un bucle de sincronización. Un bucle de sincronización se produce cuando una sincronización del grupo de sincronización A desencadena una sincronización del grupo de sincronización B, y esto desencadena una sincronización del grupo de sincronización A. La situación real puede ser más compleja e implicar a más de dos grupos de sincronización del bucle. El problema es que hay un desencadenamiento circular de sincronización que se debe a que los grupos de sincronización se superponen entre sí.

- **Resolución**. La mejor corrección es la prevención. Asegúrese de que no haya referencias circulares en los grupos de sincronización. Cualquier fila sincronizada por un grupo de sincronización no puede ser sincronizada por otro grupo de sincronización.

### <a name="sync-nulls"></a> He recibido el mensaje "No se puede insertar el valor NULL en la columna <column>. La columna no admite valores NULL." ¿Qué significa esto y cómo puedo corregir este error? 
Este mensaje de error indica que se ha producido uno de los dos problemas siguientes:
-  Una tabla no tiene ninguna clave principal. Para corregir este problema, agregue una clave principal a todas las tablas que va a sincronizar.
-  Hay una cláusula WHERE en la instrucción CREATE INDEX. Data Sync no controla esta condición. Para solucionar este problema, quite la cláusula WHERE o realice los cambios manualmente en todas las bases de datos. 
 
### <a name="sync-circ"></a> ¿Cómo trata Data Sync las referencias circulares? En otras palabras, ¿cuándo se sincronizan los mismos datos en varios grupos de sincronización y sigue cambiando como resultado?
Data Sync no controla las referencias circulares, así que asegúrese de no usarlas. 

## <a name="client-agent-issues"></a>Problemas del agente cliente

- [La instalación, desinstalación o reparación del agente cliente produce un error](#agent-install)

- [El agente cliente no funciona cuando cancelo la desinstalación](#agent-uninstall)

- [Mi base de datos no aparece en la lista del agente](#agent-list)

- [El agente cliente no se inicia (error 1069)](#agent-start)

- [No puedo enviar la clave del agente](#agent-key)

- [El agente cliente no se puede eliminar del portal si no se puede acceder a su base de datos local](#agent-delete)

- [La aplicación local del agente de sincronización no se puede conectar al servicio de sincronización local](#agent-connect)

### <a name="agent-install"></a> La instalación, desinstalación o reparación del agente cliente produce un error

- **Causa**. Muchos escenarios pueden provocar este error. Para determinar cuál es la causa concreta, debe examinar los registros.

- **Resolución**. Para encontrar la causa específica del error, debe generar y examinar los registros de Windows Installer. Puede activar el registro desde el símbolo del sistema. Por ejemplo, si el archivo AgentServiceSetup.msi descargado es LocalAgentHost.msi, genere y examine los archivos de registro mediante el uso de las siguientes líneas de comandos:

    -   Para las instalaciones: `msiexec.exe /i SQLDataSyncAgent-Preview-ENU.msi /l\*v LocalAgentSetup.InstallLog`
    -   Para las desinstalaciones: `msiexec.exe /x SQLDataSyncAgent-se-ENU.msi /l\*v LocalAgentSetup.InstallLog`

    También puede activar el registro para todas las instalaciones realizadas por Windows Installer. En el artículo de Microsoft Knowledge Base [Cómo habilitar el registro de Windows Installer](https://support.microsoft.com/help/223300/how-to-enable-windows-installer-logging) se proporciona una solución de un solo clic para activar el registro en Windows Installer. También indica la ubicación de los registros.

### <a name="agent-uninstall"></a> El agente cliente no funciona cuando cancelo la desinstalación

El agente cliente no funciona aunque cancele su desinstalación.

- **Causa**. Este problema se produce porque el agente cliente de SQL Data Sync no almacena credenciales.

- **Resolución**. Puede probar estas dos soluciones:

    -   Use services.msc para volver a introducir las credenciales para el agente cliente.
    -   Desinstale este agente cliente y, después, instale uno nuevo. Descargue e instale el agente cliente más reciente del [Centro de descarga](http://go.microsoft.com/fwlink/?linkid=221479).

### <a name="agent-list"></a> Mi base de datos no aparece en la lista del agente

Al intentar agregar una base de datos existente de SQL Server a un grupo de sincronización, la base de datos no aparece en la lista de agentes.

Estos escenarios pueden provocar este error:

- **Causa**. El agente cliente y el grupo de sincronización se encuentran en distintos centros de datos.

- **Resolución**. El agente cliente y el grupo de sincronización deben estar en el mismo centro de datos. Para configurar esto, tiene dos opciones:

    -   Cree un nuevo agente en el centro de datos donde se encuentra el grupo de sincronización. Registre la base de datos con ese agente.
    -   Elimine el grupo de sincronización actual. A continuación, vuelva a crear el grupo de sincronización en el centro de datos donde se encuentra el agente.

- **Causa**. La lista de agentes cliente de la base de datos no está actualizada.

- **Resolución**. Detenga y reinicie el servicio de agente cliente.

    El agente local descarga la lista de bases de datos asociadas solo la primera vez que se envía la clave del agente. No descarga la lista de bases de datos asociadas en los envíos posteriores de la clave del agente. Las bases de datos que se registran durante un traslado del agente no aparecen en la instancia original del agente.

### <a name="agent-start"></a> El agente cliente no se inicia (error 1069)

El agente no se está ejecutando en un equipo que hospeda SQL Server. Al intentar iniciar manualmente el agente, ve un cuadro de diálogo con el mensaje "Error 1069: el servicio no se inició debido a un error de inicio de sesión".

![Cuadro de diálogo del error 1069 de Data Sync](media/sql-database-troubleshoot-data-sync/sync-error-1069.png)

- **Causa**. Una causa probable de este error es que la contraseña del servidor local ha cambiado desde que creó el agente y la contraseña del agente.

- **Resolución**. Actualice la contraseña del agente con su contraseña actual del servidor:

  1. Busque el servicio del agente de cliente de SQL Data Sync.  
    a. Seleccione **Iniciar**.  
    b. En el cuadro de búsqueda, escriba **services.msc**.  
    c. En los resultados de la búsqueda, haga clic en **Servicios**.  
    d. En la ventana **Servicios**, desplácese hasta la entrada **Agente de SQL Data Sync**.  
  1. Haga clic con el botón derecho en **Agente de SQL Data Sync** y seleccione **Detener**.
  1. Haga clic con el botón derecho en **Agente de SQL Data Sync** y seleccione **Propiedades**.
  1. En **Propiedades del agente de SQL Data Sync**, seleccione la pestaña **Inicio de sesión**.
  1. En el cuadro de texto **Contraseña**, escriba su contraseña.
  1. En el cuadro de texto **Confirmar contraseña**, vuelva a escribir su contraseña.
  1. Seleccione **Aplicar** y luego **Aceptar**.
  1. En la ventana **Servicios**, haga clic con el botón derecho en el servicio **Agente de SQL Data Sync** y, después, haga clic en **Iniciar**.
  1. Cierre la ventana **Servicios**.

### <a name="agent-key"></a> No puedo enviar la clave del agente

Después de crear o volver a crear una clave para un agente, intenta enviar esa clave a través de la aplicación SqlAzureDataSyncAgent y no se puede completar el envío.

![Cuadro de diálogo de error de sincronización: no se puede enviar la clave del agente](media/sql-database-troubleshoot-data-sync/sync-error-cant-submit-agent-key.png)

- **Requisitos previos**. Antes de continuar, compruebe los siguientes requisitos previos:

  - El servicio de Windows SQL Data Sync se está ejecutando.

  - La cuenta del servicio de Windows SQL Data Sync tiene acceso a la red.

  - El puerto 1433 saliente está abierto en la regla de firewall local.

  - Se agrega la dirección IP local al servidor o la regla de firewall de base de datos para la base de datos de metadatos de sincronización.

- **Causa**. La clave del agente identifica de forma única cada agente local. La clave debe cumplir dos condiciones:

  -   La clave del agente cliente del servidor de SQL Data Sync y del equipo local deben ser idénticas.
  -   La clave del agente cliente solo se puede usar una vez.

- **Resolución**. Si el agente no funciona, es porque una de estas condiciones o ninguna de las dos se cumplen. Para que el agente vuelva a funcionar:

  1. Genere una nueva clave.
  1. Aplique la nueva clave al agente.

  Para aplicar la nueva clave al agente:

  1. En el Explorador de archivos, vaya al directorio de instalación del agente. El directorio de instalación predeterminado es C:\\Archivos de programa (x86)\\Microsoft SQL Data Sync.
  1. Haga doble clic en el subdirectorio bin.
  1. Abra la aplicación SqlAzureDataSyncAgent.
  1. Seleccione **Enviar clave del agente**.
  1. Pegue la clave del portapapeles en el espacio proporcionado.
  1. Seleccione **Aceptar**.
  1. Cierre el programa.

### <a name="agent-delete"></a> El agente cliente no se puede eliminar del portal si no se puede acceder a su base de datos local

Si no se puede acceder a un punto de conexión local (es decir, a una base de datos) que está registrado en un agente cliente de SQL Data Sync (versión preliminar), dicho agente no se puede eliminar.

- **Causa**. El agente local no se puede eliminar porque la base de datos a la que no se puede acceder sigue registrada con el agente. Al intentar eliminar el agente, el proceso de eliminación intenta acceder a la base de datos, en la que se produce un error.

- **Resolución**. Use "Forzar eliminación" para eliminar la base de datos a la que no se puede acceder.

> [!NOTE]
> Si tras una eliminación forzada, las tablas de metadatos de sincronización permanecen, use `deprovisioningutil.exe` para limpiarlas.

### <a name="agent-connect"></a> La aplicación local del agente de sincronización no se puede conectar al servicio de sincronización local

- **Resolución**. Realice estos pasos:

  1. Salga de la aplicación.  
  1. Abra el panel Servicios de componentes.  
    a. En el cuadro de búsqueda de la barra de tareas, escriba **services.msc**.  
    b. En los resultados de la búsqueda, haga doble clic en **Servicios**.  
  1. Detenga el servicio **SQL Data Sync**.
  1. Reinicie el servicio **SQL Data Sync**.  
  1. Vuelva a abrir la aplicación.

## <a name="setup-and-maintenance-issues"></a>Problemas de configuración y mantenimiento

- [Aparece un mensaje en el que se indica "espacio insuficiente en disco"](#setup-space)

- [No puedo eliminar mi grupo de sincronización](#setup-delete)

- [No puedo anular el registro de una base de datos de SQL Server local](#setup-unreg)

- [No tengo suficientes privilegios para iniciar los servicios del sistema](#setup-perms)

- [Una base de datos tiene como estado "Obsoleto"](#setup-date)

- [Un grupo de sincronización tiene como estado "Obsoleto"](#setup-date2)

- [Un grupo de sincronización no se puede eliminar durante los tres minutos posteriores a la desinstalación o detención del agente](#setup-delete2)

- [¿Qué ocurre cuando se restaura una base de datos perdida o dañada?](#setup-restore)

### <a name="setup-space"></a> Aparece un mensaje en el que se indica "espacio insuficiente en disco"

- **Causa**. El mensaje "espacio insuficiente en disco" puede aparecer si es necesario eliminar archivos sobrantes. Esto puede deberse a un software antivirus o a archivos abiertos cuando se intentan realizar operaciones de eliminación.

- **Resolución**. Elimine manualmente los archivos de sincronización que se encuentran en la carpeta %temp% (`del \*sync\* /s`). A continuación, elimine los subdirectorios de la carpeta %temp%.

> [!IMPORTANT]
> No elimine los archivos mientras la sincronización esté en curso.

### <a name="setup-delete"></a> No puedo eliminar mi grupo de sincronización

Se produce un error al intentar eliminar un grupo de sincronización. El error al eliminar un grupo de sincronización se puede producir en cualquier de los siguientes escenarios:

- **Causa**. El agente cliente no tiene conexión.

- **Resolución**. Asegúrese de que el agente cliente está en línea y vuelva a intentarlo.

- **Causa**. El agente cliente se ha desinstalado o no está presente.

- **Resolución**. Si el agente cliente se ha desinstalado o no está presente, haga lo siguiente:  
    a. Si el archivo XML del agente existe, quítelo de la carpeta de instalación de SQL Data Sync.  
    b. Instale el agente en un equipo local (puede ser el mismo o un equipo diferente). A continuación, envíe la clave del agente que se genera en el portal para el agente que se muestra como sin conexión.

- **Causa**. Una base de datos está sin conexión.

- **Resolución**. Asegúrese de que las bases de datos de SQL Database y SQL Server están todas en línea.

- **Causa**. El grupo de sincronización se está aprovisionando o sincronizando.

- **Resolución**. Espere a que finalice el proceso de aprovisionamiento o sincronización y, después, vuelva a intentar eliminar el grupo de sincronización.

### <a name="setup-unreg"></a> No puedo anular el registro de una base de datos de SQL Server local

- **Causa**. Es muy probable que esté intentando anular el registro de una base de datos que ya se ha eliminado.

- **Resolución**. Para anular el registro de una base de datos de SQL Server local, seleccione la base de datos y, después, haga clic en **Forzar eliminación**.

  Si con esta operación no se puede quitar la base de datos del grupo de sincronización:

  1. Detenga y reinicie el servicio de host del agente cliente:  
    a. Seleccione el menú **Inicio**.  
    b. En el cuadro de búsqueda, escriba **services.msc**.  
    c. En la sección **Programas** del panel de resultados, haga doble clic en **Servicios**.  
    d. Haga clic con el botón derecho en el servicio **SQL Data Sync**.  
    e. Si el servicio se está ejecutando, deténgalo.  
    f. Haga clic con el botón derecho en el servicio y, después, seleccione **Iniciar**.  
    g. Compruebe si la base de datos sigue registrada. Si ya no lo está, habrá terminado. En caso contrario, continúe con el paso siguiente.
  1. Abra la aplicación de agente cliente (SqlAzureDataSyncAgent).
  1. Seleccione **Editar credenciales** y, después, escriba las credenciales de la base de datos.
  1. Continúe con la anulación del registro.

### <a name="setup-perms"></a> No tengo suficientes privilegios para iniciar los servicios del sistema

- **Causa**. Este error se produce en dos situaciones:
  -   El nombre de usuario o la contraseña son incorrectos.
  -   La cuenta de usuario especificada no tiene suficientes privilegios para iniciar sesión como servicio.

- **Resolución**. Conceda a la cuenta de usuario credenciales para iniciar sesión como servicio:

  1. Vaya a **Inicio** > **Panel de control** > **Herramientas administrativas** > **Directiva de seguridad local** > **Directiva local** > **User Rights Management** (Administración de los permisos del usuario).
  1. Seleccione **Iniciar sesión como servicio**.
  1. En el cuadro de diálogo **Propiedades**, agregue la cuenta de usuario.
  1. Seleccione **Aplicar** y luego **Aceptar**.
  1. Cierre todas las ventanas.

### <a name="setup-date"></a> Una base de datos tiene como estado "Obsoleto"

- **Causa**. SQL Data Sync quita del servicio las bases de datos que han estado sin conexión durante un mínimo de 45 días (contados a partir del momento en que la base de datos quedó sin conexión). Si una base de datos está sin conexión durante 45 días o más y después vuelve a conectarse, su estado se establece en **Obsoleto**.

- **Resolución**. Para evitar un estado **Obsoleto**, asegúrese de que ninguna de las bases de datos esté sin conexión durante 45 días o más.

  Si el estado de una base de datos es **Obsoleto**:

  1. Quite la base de datos con un estado **Obsoleto** del grupo de sincronización.
  1. Vuelva a agregar la base de datos al grupo de sincronización.

  > [!WARNING]
  > Perderá todos los cambios que haya realizado en la base de datos mientras estaba sin conexión.

### <a name="setup-date2"></a> Un grupo de sincronización tiene como estado "Obsoleto"

- **Causa**. Un grupo de sincronización puede quedar obsoleto si uno o más cambios no se pueden aplicar durante todo el período de retención de 45 días.

- **Resolución**. Para evitar el estado **Obsoleto** de un grupo de sincronización, examine de forma regular los resultados de los trabajos de sincronización en el visor del historial. Averigüe qué cambios no se pudieron aplicar y resuélvalos.

  Si el estado de un grupo de sincronización es **Obsoleto**, tiene que eliminar el grupo de sincronización y volver a crearlo.

### <a name="setup-delete2"></a> Un grupo de sincronización no se puede eliminar durante los tres minutos posteriores a la desinstalación o detención del agente

No puede eliminar un grupo de sincronización durante los tres minutos posteriores a la desinstalación o detención del agente cliente de SQL Data Sync asociado.

- **Resolución**.

  1. Quite un grupo de sincronización mientras los agentes de sincronización asociados estén en línea (opción recomendada).
  1. Si el agente está sin conexión pero instalado, póngalo en línea en el equipo local. Espere a que el estado del agente aparezca como **En línea** en el portal de SQL Data Sync. A continuación, quite el grupo de sincronización.
  1. Si el agente está sin conexión porque se ha desinstalado:  
    a.  Si el archivo XML del agente existe, quítelo de la carpeta de instalación de SQL Data Sync.  
    b.  Instale el agente en un equipo local (puede ser el mismo o un equipo diferente). A continuación, envíe la clave del agente que se genera en el portal para el agente que se muestra como sin conexión.  
    c. Intente eliminar el grupo de sincronización.

### <a name="setup-restore"></a> ¿Qué ocurre cuando se restaura una base de datos perdida o dañada?

Si restaura una base de datos perdida o dañada desde una copia de seguridad, puede que los datos de los grupos de sincronización a los que pertenece la base de datos no coincidan.

## <a name="next-steps"></a>Pasos siguientes
Para más información acerca de SQL Data Sync, consulte:

-   [Sincronización de datos entre varias bases de datos locales y de la nube con SQL Data Sync](sql-database-sync-data.md)  
-   [Configuración de Azure SQL Data Sync](sql-database-get-started-sql-data-sync.md)  
-   [Procedimientos recomendados para SQL Data Sync de Azure](sql-database-best-practices-data-sync.md)  
-   [Supervisión de Azure SQL Data Sync con Log Analytics](sql-database-sync-monitor-oms.md)  
-   Para obtener ejemplos completos de PowerShell que muestren cómo configurar SQL Data Sync:  
    -   [Uso de PowerShell para sincronizar entre varias bases de datos SQL de Azure.](scripts/sql-database-sync-data-between-sql-databases.md)  
    -   [Uso de PowerShell para realizar la sincronización entre Azure SQL Database y una base de datos de SQL Server local](scripts/sql-database-sync-data-between-azure-onprem.md)  

Para obtener más información sobre SQL Database, vea:

-   [Información general de SQL Database](sql-database-technical-overview.md)
-   [Administración del ciclo de vida de las aplicaciones](https://msdn.microsoft.com/library/jj907294.aspx)
