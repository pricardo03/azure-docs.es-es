---
title: Solución de problemas al realizar copias de seguridad de bases de datos de SAP HANA con Azure Backup | Microsoft Docs
description: En este artículo se describe cómo se solucionan los errores comunes que pueden producirse al usar Azure Backup para realizar copias de seguridad de bases de datos de SAP HANA.
services: backup
author: pvrk
manager: vijayts
ms.service: backup
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: pullabhk
ms.openlocfilehash: 32e814ea83f30b48af5ce507ce250f37a34390da
ms.sourcegitcommit: a6873b710ca07eb956d45596d4ec2c1d5dc57353
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/16/2019
ms.locfileid: "68249489"
---
# <a name="troubleshoot-backup-of-sap-hana-databases-on-azure"></a>Solución de problemas al realizar copias de seguridad de bases de datos de SAP HANA en Azure

En este artículo se proporciona información para la solución de problemas al realizar copias de seguridad de bases de datos de SAP HANA en Azure Virtual Machines.

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

## <a name="common-user-errors"></a>Errores de usuario comunes

### <a name="usererrorinopeninghanaodbcconnection"></a>UserErrorInOpeningHanaOdbcConnection

data| Mensaje de error | Causas posibles | Acción recomendada |
|---|---|---|
| No se pudo conectar con el sistema HANA. Compruebe que el sistema está en funcionamiento.| El servicio Azure Backup no se puede conectar con HANA porque la base de datos de HANA está inactiva. O bien HANA se está ejecutando, pero no permite que el servicio Azure Backup se conecte. | Compruebe si la base de datos o el servicio HANA están inactivos. Si el servicio o la base de datos de HANA está en funcionamiento, compruebe si [se han establecido todos los permisos](#setting-up-permissions). Si falta la clave, vuelva a ejecutar el script de registro previo para crear una clave. |

### <a name="usererrorinvalidbackintconfiguration"></a>UserErrorInvalidBackintConfiguration

| Mensaje de error | Causas posibles | Acción recomendada |
|---|---|---|
| Configuración de Backint no válida detectada. Detenga la protección y vuelva a configurar la base de datos.| Los parámetros de backInt están incorrectamente especificados para Azure Backup. | Compruebe si [se han establecido los parámetros](#setting-up-backint-parameters). Si los parámetros basados en backInt están presentes en el host, quítelos. Si los parámetros no están presentes en el nivel del host, pero se han modificado manualmente en el nivel de la base de datos, reviértalos a los valores apropiados como se describió anteriormente. O bien, ejecute **Detener la protección y conservar los datos de copia de seguridad** en Azure Portal y, después, seleccione **Reanudar copia de seguridad**.|
