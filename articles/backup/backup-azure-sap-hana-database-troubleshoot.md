---
title: Solución de problemas al realizar copias de seguridad de bases de datos de SAP HANA con Azure Backup | Microsoft Docs
description: En este artículo se describe cómo se solucionan los errores comunes que pueden producirse al usar Azure Backup para realizar copias de seguridad de bases de datos de SAP HANA.
ms.reviewer: pullabhk
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 08/03/2019
ms.author: dacurwin
ms.openlocfilehash: 00e37030417da97d2c57b0fb5872422e7048a2bc
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/12/2019
ms.locfileid: "68954455"
---
# <a name="troubleshoot-backup-of-sap-hana-databases-on-azure"></a>Solución de problemas al realizar copias de seguridad de bases de datos de SAP HANA en Azure

En este artículo se proporciona información para la solución de problemas al realizar copias de seguridad de bases de datos de SAP HANA en Azure Virtual Machines. En la sección siguiente se explican datos conceptuales importantes que son necesarios para diagnosticar errores comunes en las copias de seguridad de SAP HANA.

## <a name="prerequisites"></a>Requisitos previos

Como parte de los [requisitos previos](backup-azure-sap-hana-database.md#prerequisites), asegúrese de que se ha ejecutado el script de registro previo en la máquina virtual en la que se ha instalado HANA.

### <a name="setting-up-permissions"></a>Configuración de permisos

Qué hace el script de registro previo:

1. Crea AZUREWLBACKUPHANAUSER en el sistema HANA y agrega estos roles y permisos requeridos:
    - DATABASE ADMIN: para crear nuevas bases de datos durante la restauración.
    - CATALOG READ: para leer el catálogo de copia de seguridad.
    - SAP_INTERNAL_HANA_SUPPORT: para acceder a algunas tablas privadas.
2. Agrega una clave a Hdbuserstore para que el complemento de HANA controle todas las operaciones (consultas de base de datos, operaciones de restauración, configuración y ejecución de copias de seguridad).
   
   Para confirmar la creación de la clave, ejecute el comando HDBSQL en la máquina HANA con credenciales SIDADM:

    ``` hdbsql
    hdbuserstore list
    ```
    
    La salida de comandos debe mostrar la tecla {SID}{DBNAME}, con el usuario como AZUREWLBACKUPHANAUSER.

> [!NOTE]
> Asegúrese de que tiene un conjunto único de archivos SSFS en **/usr/sap/{SID}/home/.hdb/** . Solo debería haber una carpeta en esta ruta de acceso.

### <a name="setting-up-backint-parameters"></a>Configuración de los parámetros BackInt

Una vez que se elige una base de datos para la copia de seguridad, el servicio Azure Backup configurará los parámetros backInt en el nivel de la base de datos:

- [catalog_backup_using_backint:true]
- [enable_accumulated_catalog_backup:false]
- [parallel_data_backup_backint_channels:1]
- [log_backup_timeout_s:900)]
- [backint_response_timeout:7200]

> [!NOTE]
> Asegúrese de que estos parámetros *no* estén presentes en el nivel de host. Los parámetros en el nivel de host invalidarán estos parámetros y pueden causar un comportamiento inesperado.

## <a name="restore-checks"></a>Comprobaciones de restauración

### <a name="single-container-database-sdc-restore"></a>Restauración de una base de datos de contenedor único (SDC)

Ocúpese de las entradas mientras se realiza la restauración de una base de datos de contenedor único (SDC) para HANA a otra máquina de SDC. El nombre de la base de datos debe estar en minúscula con "sdc" anexado entre corchetes. La instancia de HANA se mostrará en mayúsculas.

Supongamos que hay una copia de seguridad de una instancia "H21" de HANA de SDC. En la página de elementos de copia de seguridad aparecerá el nombre del elemento de copia de seguridad como **"h21(sdc)"** . Si intenta restaurar esta base de datos en otra SDC de destino, por ejemplo, H11, es necesario proporcionar las entradas siguientes.

![Entradas de restauración de SDC](media/backup-azure-sap-hana-database/hana-sdc-restore.png)

Observe lo siguiente
- De manera predeterminada, el nombre de la base de datos restaurada se rellenará con el nombre del elemento de copia de seguridad, es decir, h21(sdc).
- Si selecciona el destino como H11, el nombre de la base de datos restaurada NO CAMBIARÁ de manera automática. **Se debe editar para cambiarlo a h11(sdc)** . En el caso de SDC, el nombre de la base de datos restaurada será el identificador de la instancia de destino en minúsculas más "sdc" entre corchetes.
- Como SDC solo puede tener una base de datos única, también debe hacer clic en la casilla para permitir reemplazar los datos existentes de la base de datos por los datos del punto de recuperación.
- Linux distingue mayúsculas de minúsculas, por lo que debe tener cuidado de mantener el formato.

### <a name="multiple-container-database-mdc-restore"></a>Restauración de base de datos de varios contenedores (MDC)

En las bases de datos de varios contenedores para HANA, la configuración estándar es SYSTEMDB + 1 o más bases de datos de inquilino. Restaurar toda una instancia de SAP HANA significa restaurar tanto SYSTEMDB como las bases de datos de inquilino. Primero se restaura SYSTEMDB y, luego, la base de datos de inquilino. Básicamente, restaurar la base de datos del sistema significa reemplazar la información del sistema del destino seleccionado. Esto también reemplaza la información relacionada con BackInt en la instancia de destino. Por lo tanto, una vez que la base de datos del sistema se restaura en una instancia de destino, es necesario volver a ejecutar el script de registro previo. Solo entonces las restauraciones de bases de datos de inquilino subsiguientes se realizarán correctamente.

## <a name="common-user-errors"></a>Errores de usuario comunes

### <a name="usererrorinopeninghanaodbcconnection"></a>UserErrorInOpeningHanaOdbcConnection

data| Mensaje de error | Causas posibles | Acción recomendada |
|---|---|---|
| No se pudo conectar con el sistema HANA. Compruebe que el sistema está en funcionamiento.| El servicio Azure Backup no se puede conectar con HANA porque la base de datos de HANA está inactiva. O bien HANA se está ejecutando, pero no permite que el servicio Azure Backup se conecte. | Compruebe si la base de datos o el servicio HANA están inactivos. Si el servicio o la base de datos de HANA está en funcionamiento, compruebe si [se han establecido todos los permisos](#setting-up-permissions). Si falta la clave, vuelva a ejecutar el script de registro previo para crear una clave. |

### <a name="usererrorinvalidbackintconfiguration"></a>UserErrorInvalidBackintConfiguration

| Mensaje de error | Causas posibles | Acción recomendada |
|---|---|---|
| Configuración de Backint no válida detectada. Detenga la protección y vuelva a configurar la base de datos.| Los parámetros de backInt están incorrectamente especificados para Azure Backup. | Compruebe si [se han establecido los parámetros](#setting-up-backint-parameters). Si los parámetros basados en backInt están presentes en el host, quítelos. Si los parámetros no están presentes en el nivel del host, pero se han modificado manualmente en el nivel de la base de datos, reviértalos a los valores apropiados como se describió anteriormente. O bien, ejecute **Detener la protección y conservar los datos de copia de seguridad** en Azure Portal y, después, seleccione **Reanudar copia de seguridad**.|