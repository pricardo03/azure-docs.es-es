---
title: Restauración de una copia de seguridad en Azure Service Fabric | Microsoft Docs
description: Use la característica de copia de seguridad periódica y restauración de Service Fabric para restaurar datos de copia de seguridad de los datos de aplicación.
services: service-fabric
documentationcenter: .net
author: aagup
manager: timlt
editor: aagup
ms.assetid: 802F55B6-6575-4AE1-8A8E-C9B03512FF88
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/30/2018
ms.author: aagup
ms.openlocfilehash: 69604decab354368f336b85bfa1497671f0c3101
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/01/2018
ms.locfileid: "52730725"
---
#  <a name="restoring-backup-in-azure-service-fabric"></a>Restauración de una copia de seguridad en Azure Service Fabric


Los servicios de confianza con estado y Reliable Actors de Service Fabric pueden mantener un estado mutable y autoritativo más allá de la solicitud y respuesta o una transacción completa. Si un servicio con estado deja de funcionar durante mucho tiempo o pierde información debido a un desastre, es posible que tenga que restaurarse a la última copia de seguridad aceptable de su estado con el fin de seguir proporcionando servicio una vez que vuelva a activarse.

Por ejemplo, es posible que un servicio quiera realizar copias de seguridad de los datos como medida de protección en los escenarios siguientes:

- En caso de que se produzca una pérdida permanente de todo un clúster de Service Fabric. **(Caso de recuperación ante desastres [DR])**
- Pérdida permanente de la mayoría de las réplicas de una partición del servicio. **(Caso de pérdida de datos)**
- Errores administrativos por los cuales el estado se elimina o daña accidentalmente. Por ejemplo, un administrador con suficientes privilegios elimina el servicio por error. **(Caso de pérdida de datos)**
- Errores en el servicio que causan daños en los datos. Por ejemplo, los daños en los datos pueden producirse cuando una actualización del código de servicio empieza a escribir datos defectuosos en una colección confiable. En tal caso, es posible que el código y los datos se tengan que revertir a un estado anterior. **(Caso de datos dañados)**


## <a name="prerequisites"></a>Requisitos previos
* Para desencadenar la restauración, el _servicio de análisis de errores (FAS)_ debe estar habilitado para el clúster.
* El _servicio de restauración de copia de seguridad (BRS)_ debe haber tomado la copia de seguridad que se va a restaurar.
* La restauración solo se puede desencadenar en una partición.

## <a name="triggering-restore"></a>Desencadenamiento de la restauración

La restauración puede darse en cualquiera de los escenarios siguientes: 
* Restauración de datos en caso de _recuperación ante desastres_ (DR)
* Restauración de datos en caso de _datos dañados y pérdida de datos_



### <a name="data-restore-in-the-event-of-disaster-recovery-dr"></a>Restauración de datos en caso de _recuperación ante desastres_ (DR)
En caso de que se pierda un clúster de Service Fabric completo, los datos de las particiones del servicio de confianza con estado y de Reliable Actors se pueden restaurar en un clúster alternativo. La copia de seguridad deseada se puede seleccionar de la enumeración de los [detalles de GetBackupAPI con almacenamiento de copia de seguridad](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getbackupsfrombackuplocation). La enumeración de copia de seguridad puede ser para una aplicación, un servicio o una partición.

Supongamos que el clúster perdido era el clúster mencionado en [Habilitación de la copia de seguridad periódica del servicio de confianza con estado y Reliable Actors](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors), que tenía `SampleApp` implementado, donde la partición tenía habilitada la directiva de copia de seguridad y las copias de seguridad tenían lugar en Azure Storage. 


Ejecute el siguiente script de PowerShell para invocar la API REST a fin de enumerar las copias de seguridad creadas para todas las particiones dentro de la aplicación `SampleApp` en el clúster de Service Fabric perdido. La API de enumeración requiere información de almacenamiento (dónde se almacenan las copias de seguridad de una aplicación) para enumerar las copias de seguridad disponibles. 

```powershell
$StorageInfo = @{
    ConnectionString = 'DefaultEndpointsProtocol=https;AccountName=<account-name>;AccountKey=<account-key>;EndpointSuffix=core.windows.net'
    ContainerName = 'backup-container'
    StorageKind = 'AzureBlobStore'
}

$BackupEntity = @{
    EntityKind = 'Application'
    ApplicationName='fabric:/SampleApp'
}

$BackupLocationAndEntityInfo = @{
    Storage = $StorageInfo
    BackupEntity = $BackupEntity
}

$body = (ConvertTo-Json $BackupLocationAndEntityInfo)
$url = "https://myalternatesfcluster.southcentralus.cloudapp.azure.com:19080/BackupRestore/$/GetBackups?api-version=6.4"

$response = Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
$BackupPoints = (ConvertFrom-Json $response.Content)
$BackupPoints.Items
```
Salida de ejemplo de la ejecución anterior:

```
BackupId                : b9577400-1131-4f88-b309-2bb1e943322c
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 20.55.16.zip
BackupType              : Full
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3334
CreationTimeUtc         : 2018-04-06T20:55:16Z
FailureError            : 
*
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            : 
*
BackupId                : 69436834-c810-4163-9386-a7a800f78359
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.25.36.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3764
CreationTimeUtc         : 2018-04-06T21:25:36Z
FailureError            : 
```



Para desencadenar la restauración, es necesario elegir la copia de seguridad deseada. Permitir que la copia de seguridad deseada para la recuperación ante desastres (DR) actual sea la siguiente copia de seguridad

```
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            : 
```

Para la API de restauración, es necesario proporcionar los detalles __BackupId__ y __BackupLocation__. La partición del clúster alternativo se debe elegir según el [esquema de partición](service-fabric-concepts-partitioning.md#get-started-with-partitioning). Es responsabilidad del usuario elegir la partición de destino para restaurar la copia de seguridad del clúster alternativo, según el esquema de partición del clúster perdido original.

Suponga que el identificador de partición del clúster alternativo es `1c42c47f-439e-4e09-98b9-88b8f60800c6`, que se asigna al identificador de partición del clúster original `974bd92a-b395-4631-8a7f-53bd4ae9cf22` mediante la comparación de la clave superior e inferior para la _creación de particiones por rangos (UniformInt64Partition)_.

Para la _creación de particiones con nombre_, el valor de nombre se compara para identificar la partición de destino en el clúster alternativo.

La restauración se solicita en la partición del clúster de copia de seguridad mediante la siguiente [API de restauración](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-restorepartition).

```powershell 
$RestorePartitionReference = @{ 
    BackupId = 'b0035075-b327-41a5-a58f-3ea94b68faa4'
    BackupLocation = 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip' 
} 
 
$body = (ConvertTo-Json $RestorePartitionReference) 
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/1c42c47f-439e-4e09-98b9-88b8f60800c6/$/Restore?api-version=6.4" 
 
Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
``` 
El progreso de la restauración puede ser [TrackRestoreProgress](service-fabric-backup-restore-service-trigger-restore.md#tracking-restore-progress).

### <a name="data-restore-in-the-event-of-data-corruption--data-loss"></a>Restauración de datos en caso de _datos dañados y pérdida de datos_

En el caso de _pérdida de datos_ o _datos dañados_, los datos de las particiones del servicio de confianza con estado y de Reliable Actors se pueden restaurar a cualquiera de las copias de seguridad elegidas. El siguiente caso es la continuación del ejemplo mencionado en [Habilitación de la copia de seguridad periódica del servicio de confianza con estado y Reliable Actors](service-fabric-backuprestoreservice-quickstart-azurecluster.md#enabling-periodic-backup-for-reliable-stateful-service-and-reliable-actors), donde la partición tiene una directiva de copia de seguridad habilitada y toma la copia de seguridad con una frecuencia deseada en Azure Storage. 

La copia de seguridad deseada se selecciona de la salida de [GetBackupAPI](service-fabric-backuprestoreservice-quickstart-azurecluster.md#list-backups). En este escenario, la copia de seguridad se genera a partir del mismo clúster del pasado.
Para desencadenar la restauración, es necesario elegir la copia de seguridad deseada de la lista. Vamos a dejar que nuestra copia de seguridad deseada para los casos actuales de _pérdida de datos_ / _datos dañados_ sea la siguiente copia de seguridad:

```
BackupId                : b0035075-b327-41a5-a58f-3ea94b68faa4
BackupChainId           : b9577400-1131-4f88-b309-2bb1e943322c
ApplicationName         : fabric:/SampleApp
ServiceName             : fabric:/SampleApp/MyStatefulService
PartitionInformation    : @{LowKey=-9223372036854775808; HighKey=9223372036854775807; ServicePartitionKind=Int64Range; Id=974bd92a-b395-4631-8a7f-53bd4ae9cf22}
BackupLocation          : SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip
BackupType              : Incremental
EpochOfLastBackupRecord : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
LsnOfLastBackupRecord   : 3552
CreationTimeUtc         : 2018-04-06T21:10:27Z
FailureError            : 
```

Para la API de restauración, es necesario proporcionar los detalles __BackupId__ y __BackupLocation__. Dado que el clúster tiene la copia de seguridad habilitada, el _servicio de restauración de copia de seguridad (BRS)_ de Service Fabric identifica la ubicación de almacenamiento correcta de la directiva de copia de seguridad asociada.

```powershell
$RestorePartitionReference = @{ 
    BackupId = 'b0035075-b327-41a5-a58f-3ea94b68faa4', 
    BackupLocation = 'SampleApp\MyStatefulService\974bd92a-b395-4631-8a7f-53bd4ae9cf22\2018-04-06 21.10.27.zip' 
} 
 
$body = (ConvertTo-Json $RestorePartitionReference) 
$url = "https://mysfcluster.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/Restore?api-version=6.4" 
 
Invoke-WebRequest -Uri $url -Method Post -Body $body -ContentType 'application/json' -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3'
```

El progreso de la restauración puede ser [TrackRestoreProgress](service-fabric-backup-restore-service-trigger-restore.md#tracking-restore-progress).


## <a name="tracking-restore-progress"></a>Seguimiento del progreso de la restauración

Una partición de un servicio de confianza con estado o de Reliable Actors acepta solo una solicitud de restauración a la vez. Solo puede aceptarse otra solicitud cuando se haya completado la solicitud de restauración actual. Varias solicitudes de restauración se pueden desencadenar en distintas particiones al mismo tiempo.

```powershell
$url = "https://mysfcluster-backup.southcentralus.cloudapp.azure.com:19080/Partitions/974bd92a-b395-4631-8a7f-53bd4ae9cf22/$/GetRestoreProgress?api-version=6.4" 
 
$response = Invoke-WebRequest -Uri $url -Method Get -CertificateThumbprint '1b7ebe2174649c45474a4819dafae956712c31d3' 
 
$restoreResponse = (ConvertFrom-Json $response.Content) 
$restoreResponse | Format-List
```

La solicitud de restauración progresa en el orden siguiente:

1. __Accepted__ (Aceptado): el estado de restauración _Accepted_ (Aceptado) indica que la solicitud se ha desencadenado con los parámetros de solicitud correctos.
    ```
    RestoreState  : Accepted
    TimeStampUtc  : 0001-01-01T00:00:00Z
    RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
    RestoredLsn   : 3552
    ```
    
2. __InProgress__ (EnCurso): el estado de restauración _InProgress_ (EnCurso) indica que la partición se someterá a una restauración con la copia de seguridad mencionada en la solicitud. La partición notificará el estado _dataloss_ (pérdida de datos).
    ```
    RestoreState  : RestoreInProgress
    TimeStampUtc  : 0001-01-01T00:00:00Z
    RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
    RestoredLsn   : 3552
    ```
    
3. __Success__/ __Failure__/ __Timeout__ (Correcto/Incorrecto/Tiempo de espera): una restauración solicitada se puede realizar en cualquiera de los siguientes estados. Cada estado tiene los siguientes detalles de importancia y respuesta.
       
    * __Success__ (Correcto): el estado de restauración _Success_ (Correcto) indica que el estado de la partición se ha recuperado. La respuesta proporcionará RestoreEpoch y RestordLSN para la partición, junto con la hora en UTC. 
    
        ```
        RestoreState  : Success
        TimeStampUtc  : 2018-11-22T11:22:33Z
        RestoredEpoch : @{DataLossNumber=131675205859825409; ConfigurationNumber=8589934592}
        RestoredLsn   : 3552
        ```
        
    *. __Failure__ (Error): el estado de restauración _Failure_ (Error) indica el error de la solicitud de restauración. En la solicitud se indica la causa del error.
        ```
        RestoreState  : Failure
        TimeStampUtc  : 0001-01-01T00:00:00Z
        RestoredEpoch : 
        RestoredLsn   : 0
        ```
    *. __Timeout__ (Tiempo de espera): el estado de restauración _Timeout_ (Tiempo de espera) indica que la solicitud tiene tiempo de espera. Se recomienda la nueva solicitud de restauración con mayor [RestoreTimeout](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-backuppartition#backuptimeout); el tiempo de espera predeterminado es 10 minutos. Antes de volver a solicitar la restauración, se recomienda asegurarse de que la partición esté fuera del estado de pérdida de datos.
     
        ```
        RestoreState  : Timeout
        TimeStampUtc  : 0001-01-01T00:00:00Z
        RestoredEpoch : 
        RestoredLsn   : 0
        ```

## <a name="auto-restore"></a>Restauración automática

Las particiones del servicio de confianza con estado y de Reliable Actors en el clúster de Service Fabric se pueden configurar para la _restauración automática_. Al crear la directiva de copia de seguridad, la directiva puede tener `AutoRestore` establecido en _true_.  Al habilitar la _restauración automática_ para una partición, se restauran los datos de la última copia de seguridad si se notifica la pérdida de datos.
 
 [Habilitación de la restauración automática en la directiva de copia de seguridad](service-fabric-backuprestoreservice-configure-periodic-backup.md#auto-restore-on-data-loss)


[Referencia de API RestorePartition](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-restorepartition)
[Referencia de API GetPartitionRestoreProgress](https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-getpartitionrestoreprogress)

## <a name="next-steps"></a>Pasos siguientes
- [Información sobre la configuración de la copia de seguridad periódica](./service-fabric-backuprestoreservice-configure-periodic-backup.md)
- [Referencia de la API REST de restauración de copias de seguridad](https://docs.microsoft.com/rest/api/servicefabric/sfclient-index-backuprestore)
