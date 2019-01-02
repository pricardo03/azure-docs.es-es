---
title: Copia de seguridad a petición en Azure Service Fabric | Microsoft Docs
description: Use la característica de copia de seguridad y restauración de Service Fabric para realizar la copia de seguridad de sus datos de aplicación cuando lo necesite.
services: service-fabric
documentationcenter: .net
author: aagup
manager: timlt
editor: aagup
ms.assetid: 02DA262A-EEF6-4F90-842E-FFC4A09003E5
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: aagup
ms.openlocfilehash: 35d97f1da6b5d1c75073264c70e1cd1607d5fe0d
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2018
ms.locfileid: "52730706"
---
# <a name="on-demand-backup-in-azure-service-fabric"></a>Copia de seguridad a petición en Azure Service Fabric

Se puede hacer una copia de seguridad de los servicios de confianza sin estado y de Reliable Actors para abordar escenarios de desastres o pérdida de datos.

Service Fabric incluye características para la [copia de seguridad periódica de los datos](service-fabric-backuprestoreservice-quickstart-azurecluster.md) y la copia de seguridad de los datos cuando es necesario. La copia de seguridad a petición es útil, ya que protege frente a _pérdida de datos_/_daños en los datos_ debido a los cambios planeados en el servicio subyacente o su entorno.

La característica de copia de seguridad a petición es útil para capturar el estado de los servicios, antes de cualquier operación desencadenada manualmente relacionada con el servicio o el entorno del servicio. También ayuda a cambiar los binarios del servicio, es decir, a actualizar o degradar el servicio, dado que los datos estarán protegidos de los daños ocasionados por errores en el código de la aplicación.

## <a name="triggering-on-demand-backup"></a>Desencadenamiento de la copia de seguridad a petición

La copia de seguridad a petición requiere detalles de almacenamiento para cargar los archivos de copia de seguridad. Tiene lugar en el almacenamiento especificado en la directiva de copia de seguridad periódica o en el almacenamiento especificado en la solicitud de copia de seguridad a petición.

### <a name="on-demand-backup-to-storage-specified-by-periodic-backup-policy"></a>Copia de seguridad a petición en el almacenamiento especificado por la directiva de copia de seguridad periódica

La partición de un servicio de confianza con estado o de Reliable Actors se puede solicitar para una copia de seguridad adicional necesaria en el almacenamiento especificado en la directiva de copia de seguridad periódica. 

El siguiente caso es la continuación del ejemplo mencionado en [Habilitación de la copia de seguridad periódica del servicio de confianza con estado y Reliable Actors](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors), donde la partición tiene una directiva de copia de seguridad habilitada y toma la copia de seguridad con una frecuencia deseada en Azure Storage.

La copia de seguridad a petición para el identificador de partición `974bd92a-b395-4631-8a7f-53bd4ae9cf22` se puede desencadenar mediante la API [BackupPartition] (https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition). 

```powershell
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Backup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

El [progreso de la copia de seguridad a petición](service-fabric-backup-restore-service-ondemand-backup.md#tracking-on-demand-backup-progress) se puede seguir mediante la API [GetBackupProgress](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupprogress).

### <a name="on-demand-backup-to-specified-storage"></a>Copia de seguridad a petición en el almacenamiento especificado

La copia de seguridad a petición se puede solicitar para una partición de un servicio de confianza con estado o de Reliable Actors junto con la información de almacenamiento. La información de almacenamiento se debe proporcionar como parte de la solicitud de copia de seguridad a petición.

La copia de seguridad a petición para el identificador de partición `974bd92a-b395-4631-8a7f-53bd4ae9cf22` se puede desencadenar mediante la API [BackupPartition] (https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition) con la información de almacenamiento de Azure, como se muestra a continuación.

```powershell
$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$OnDemandBackupRequest = @{
    BackupStorage = $StorageInfo
}

$body = (ConvertTo-Json $OnDemandBackupRequest)
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Backup?api-version=6.4"

Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

El [progreso de la copia de seguridad a petición](service-fabric-backup-restore-service-ondemand-backup.md#tracking-on-demand-backup-progress) se puede seguir mediante la API [GetBackupProgress](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionbackupprogress).


## <a name="tracking-on-demand-backup-progress"></a>Seguimiento del progreso de copia de seguridad a petición

Una partición de un servicio de confianza con estado o de Reliable Actors acepta solo una solicitud de copia de seguridad a petición a la vez. Solo puede aceptarse otra solicitud cuando se haya completado la solicitud de copia de seguridad a petición actual. 

Varias solicitudes de copia de seguridad a petición se pueden desencadenar en distintas particiones al mismo tiempo.

```powershell
$url = "https://mysfcluster-backup.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/GetBackupProgress?api-version=6.4" 
 
$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3' 
$backupResponse = (ConvertFrom-Json $response.Content) 
$backupResponse
```

El progreso de la solicitud de copia de seguridad a petición puede tener uno de los estados siguientes:

* **Accepted** (Aceptado): la copia de seguridad se ha iniciado en la partición y está en curso.
    ```
    BackupState             : Accepted
    TimeStampUtc            : 0001-01-01T00:00:00Z
    BackupId                : 00000000-0000-0000-0000-000000000000
    BackupLocation          : 
    EpochOfLastBackupRecord : 
    LsnOfLastBackupRecord   : 0
    FailureError            : 
    ```
    
* **Success/ Failure/ Timeout** (Correcto/Incorrecto/Tiempo de espera): una copia de seguridad a petición solicitada se puede completar en cualquiera de los siguientes estados. Cada estado tiene los siguientes detalles de importancia y respuesta.

    * **Success** (Correcto): el estado de copia de seguridad _Success_ (Correcto) indica que el estado de la partición tiene una copia de seguridad correcta. La respuesta proporciona __BackupEpoch__ y __BackupLSN__ para la partición junto con la hora en UTC.
        ```
        BackupState             : Success
        TimeStampUtc            : 2018-11-21T20:00:01Z
        BackupId                : 5d64b697-6acd-45a4-adbd-3d75e0078081
        BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-11-21 20.00.01.zip
        EpochOfLastBackupRecord : @{DataLossNumber=131873018908156893; ConfigurationNumber=8589934592}
        LsnOfLastBackupRecord   : 36
        FailureError            : 
        ```
        
    * **Failure** (Error): el estado de copia de seguridad _Failure_ (Error) indica el error que se ha producido durante la copia de seguridad del estado de la partición. La causa del error se indica en la respuesta.
        ```
        BackupState             : Failure
        TimeStampUtc            : 0001-01-01T00:00:00Z
        BackupId                : 00000000-0000-0000-0000-000000000000
        BackupLocation          : 
        EpochOfLastBackupRecord : 
        LsnOfLastBackupRecord   : 0
        FailureError            : @{Code=FABRIC_E_BACKUPCOPIER_UNEXPECTED_ERROR; Message=An error occurred during this operation.  Please check the trace logs for more details.}
        ```
       
    * **Timeout** (Tiempo de espera): el estado de copia de seguridad _Timeout_ (Tiempo de espera) indica que la copia de seguridad del estado de partición no se pudo crear en la franja horaria especificada; el valor de tiempo de espera predeterminado es 10 minutos. En este escenario se recomienda iniciar una nueva solicitud de copia de seguridad con un valor de [BackupTimeout](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition#backuptimeout) mayor en la solicitud de copia de seguridad a petición.

        ```
        BackupState             : Timeout
        TimeStampUtc            : 0001-01-01T00:00:00Z
        BackupId                : 00000000-0000-0000-0000-000000000000
        BackupLocation          : 
        EpochOfLastBackupRecord : 
        LsnOfLastBackupRecord   : 0
        FailureError            : @{Code=FABRIC_E_TIMEOUT; Message=The request of backup has timed out.}
        ```

## <a name="next-steps"></a>Pasos siguientes
- [Información sobre la configuración de la copia de seguridad periódica](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [Referencia de la API REST de restauración de copias de seguridad](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)

