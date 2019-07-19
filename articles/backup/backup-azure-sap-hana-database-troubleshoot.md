---
title: Solución de problemas al realizar copias de seguridad de bases de datos de SAP HANA con Azure Backup | Microsoft Docs
description: En esta guía se explica cómo solucionar errores comunes al intentar realizar copias de seguridad de bases de datos de SAP HANA con Azure Backup.
services: backup
author: pvrk
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: pullabhk
ms.openlocfilehash: 33f1b4674aad35d55ab014c45cd73753533931cb
ms.sourcegitcommit: 6cb4dd784dd5a6c72edaff56cf6bcdcd8c579ee7
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/02/2019
ms.locfileid: "67514170"
---
# <a name="troubleshoot-back-up-of-sap-hana-server-on-azure"></a>Solución de problemas de copias de seguridad del servidor de SAP HANA en Azure

En este artículo se proporciona información para la solución de problemas para proteger las bases de datos de SAP HANA en Azure Virtual Machines. Antes de proceder a la solución de problemas, entendamos algunos puntos clave acerca de los permisos y la configuración.

## <a name="understanding-pre-requisites"></a>Descripción de los requisitos previos

Como parte de los [requisitos previos](backup-azure-sap-hana-database.md#prerequisites), el script de registro previo debe ejecutarse en la máquina virtual en la que está instalada HANA para configurar los permisos adecuados.

### <a name="setting-up-permissions"></a>Configuración de permisos

Qué hace el script de registro previo:

1. Crea AZUREWLBACKUPHANAUSER en el sistema HANA y agrega los roles y permisos requeridos como se indica a continuación:
    - DATABASE ADMIN: para crear nuevas bases de datos durante la restauración
    - CATALOG READ: para leer el catálogo de copia de seguridad
    - SAP_INTERNAL_HANA_SUPPORT: para tener acceso a algunas tablas privadas
2. Agrega una clave a Hdbuserstore para que el complemento de HANA realice todas las operaciones (consulta de la base de datos, configuración de la copia de seguridad, realización de la copia de seguridad, realización de la restauración).
   
   - Para confirmar la creación de la clave, ejecute el comando HDBSQL en la máquina HANA con credenciales SIDADM:

    ``` hdbsql
    hdbuserstore list
    ```
    
    La salida de comandos debe mostrar la tecla {SID}{DBNAME} con el usuario como "AZUREWLBACKUPHANAUSER".

> [!NOTE]
> Asegúrese de que tiene un conjunto único de archivos SSFS bajo la ruta "/usr/sap/{SID}/home/.hdb/". Solo debería haber una carpeta bajo esta ruta de acceso.

### <a name="setting-up-backint-parameters"></a>Configuración de los parámetros BackInt

Una vez que se elige una base de datos para la copia de seguridad, el servicio Azure Backup configurará los parámetros backInt a nivel de la base de datos.

- [catalog_backup_using_backint:true]
- [enable_accumulated_catalog_backup:false]
- [parallel_data_backup_backint_channels:1]
- [log_backup_timeout_s:900)]
- [backint_response_timeout:7200]

> [!NOTE]
> Asegúrese de que estos parámetros no estén presentes en el nivel de host. Los parámetros a nivel de host invalidarán estos parámetros y pueden causar un comportamiento diferente al esperado.

## <a name="understanding-common-user-errors"></a>Descripción de los errores comunes del usuario

### <a name="usererrorinopeninghanaodbcconnection"></a>UserErrorInOpeningHanaOdbcConnection

| Mensaje de error | Causas posibles | Acción recomendada |
|---|---|---|
| No se pudo conectar al sistema HANA. Compruebe que su sistema está funcionando.| El servicio Azure Backup no puede conectarse a HANA porque la base de datos HANA está inactiva. O bien HANA se está ejecutando, pero no permite que el servicio Azure Backup se conecte. | Compruebe si la base de datos o el servicio de HANA están inactivos. Si la base de datos o el servicio de HANA están en funcionamiento, compruebe si todos los permisos están configurados como se menciona [aquí](#setting-up-permissions). Si falta la clave, vuelva a ejecutar el script de registro previo para crear una nueva clave. |

### <a name="usererrorinvalidbackintconfiguration"></a>UserErrorInvalidBackintConfiguration

| Mensaje de error | Causas posibles | Acción recomendada |
|---|---|---|
| Configuración de Backint no válida detectada. Detenga la protección y vuelva a configurar la base de datos.| Los parámetros de backInt están incorrectamente especificados para Azure Backup. | Compruebe que los parámetros son los mencionados [aquí](#setting-up-backint-parameters). Si los parámetros basados en backInt están presentes en el host, quítelos. Si los parámetros no están presentes en el host pero se han modificado manualmente a nivel de base de datos, reviértalos a los valores apropiados como se mencionó anteriormente. O bien, detenga la protección con datos de retención desde Azure Portal y reanude la copia de seguridad una vez más.|
