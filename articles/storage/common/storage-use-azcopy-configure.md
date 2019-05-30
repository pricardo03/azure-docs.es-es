---
title: Configurar, optimizar y solucionar problemas de AzCopy con el almacenamiento de Azure | Microsoft Docs
description: Configurar, optimizar y solucionar problemas de AzCopy.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: a160591ef0a47eed097ce8db373878f32965de9b
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/27/2019
ms.locfileid: "66247130"
---
# <a name="configure-optimize-and-troubleshoot-azcopy"></a>Configurar, optimizar y solucionar problemas de AzCopy

AzCopy es una utilidad de línea de comandos que puede usar para copiar los blobs o archivos a o desde una cuenta de almacenamiento. En este artículo le ayuda a realizar tareas de configuración avanzada y le ayuda a solucionar los problemas que pueden surgir al utilizar AzCopy.

> [!NOTE]
> Si está buscando el contenido que le ayudarán a empezar a trabajar con AzCopy, consulte cualquiera de los siguientes artículos:
> - [Empezar a trabajar con AzCopy](storage-use-azcopy-v10.md)
> - [Transferencia de datos con AzCopy y blob storage](storage-use-azcopy-blobs.md)
> - [Transferencia de datos con AzCopy y file storage](storage-use-azcopy-files.md)
> - [Transferencia de datos con AzCopy y Amazon S3 depósitos](storage-use-azcopy-s3.md)

## <a name="configure-proxy-settings"></a>Configuración de los valores de proxy

Para configurar la configuración de proxy para AzCopy, establezca el `https_proxy` variable de entorno.

| Sistema operativo | Get-Help  |
|--------|-----------|
| **Windows** | `set https_proxy=<proxy IP>:<proxy port>` |
| **Linux** | `export https_proxy=<proxy IP>:<proxy port>` |
| **macOS** | `export https_proxy=<proxy IP>:<proxy port>` |

Actualmente, AzCopy no admite a servidores proxy que requieren autenticación con NTLM o Kerberos.

## <a name="optimize-throughput"></a>Optimización del rendimiento

Establecer el `AZCOPY_CONCURRENCY_VALUE` variable de entorno para configurar el número de solicitudes simultáneas y para controlar el consumo de recursos y el rendimiento de rendimiento. Si el equipo tiene menos de 5 CPU, entonces el valor de esta variable se establece en `32`. En caso contrario, el valor predeterminado es igual a 16 multiplicado por el número de CPU. El valor máximo predeterminado de esta variable es `300`, pero puede establecer manualmente este valor superior o inferior.

| Sistema operativo | Get-Help  |
|--------|-----------|
| **Windows** | `set AZCOPY_CONCURRENCY_VALUE=<value>` |
| **Linux** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |
| **macOS** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |

Use el `azcopy env` para comprobar el valor actual de esta variable.  Si el valor está en blanco, el `AZCOPY_CONCURRENCY_VALUE` variable se establece en el valor predeterminado de `300`.

## <a name="change-the-location-of-the-log-files"></a>Cambie la ubicación del archivo de registro.

De forma predeterminada, los archivos de registro se encuentran en el `%USERPROFILE\\.azcopy` carpeta de Windows o en el `$HOME\\.azcopy` carpeta en Mac y Linux. Puede cambiar esta ubicación si necesita mediante los siguientes comandos.

| Sistema operativo | Get-Help  |
|--------|-----------|
| **Windows** | `set AZCOPY_LOG_LOCATION=<value>` |
| **Linux** | `export AZCOPY_LOG_LOCATION=<value>` |
| **macOS** | `export AZCOPY_LOG_LOCATION=<value>` |

Use el `azcopy env` para comprobar el valor actual de esta variable. Si el valor está en blanco, los registros se escriben en la ubicación predeterminada.

## <a name="change-the-default-log-level"></a>Cambiar el nivel de registro predeterminado

De forma predeterminada, el nivel de registro de AzCopy se establece en `INFO`. Si desea reducir el nivel de detalle de registro para ahorrar espacio en disco, sobrescribir esta configuración mediante el ``--log-level`` opción. 

Los niveles de registro disponibles son: `DEBUG`, `INFO`, `WARNING`, `ERROR`, `PANIC`, y `FATAL`.

## <a name="troubleshoot-issues"></a>Solución de problemas

AzCopy crea archivos de registro y el plan para cada trabajo. Puede usar los registros para investigar y solucionar los problemas potenciales. 

Los registros contendrá el estado de error (`UPLOADFAILED`, `COPYFAILED`, y `DOWNLOADFAILED`), la ruta de acceso completa y el motivo del error.

De forma predeterminada, los archivos de registro y el plan se encuentran en el `%USERPROFILE\\.azcopy` carpeta de Windows o `$HOME\\.azcopy` carpeta en Mac y Linux.

> [!IMPORTANT]
> Al enviar una solicitud a Microsoft Support (o para solucionar el problema de que impliquen a ningún tercero), compartir la versión censurada del comando que desea ejecutar. Esto garantiza que la SAS accidentalmente no se comparte con nadie. Puede encontrar la versión censurada al principio del archivo de registro.

### <a name="review-the-logs-for-errors"></a>Revisión de los registros en busca errores

El siguiente comando obtiene todos los errores con `UPLOADFAILED` estado desde el `04dc9ca9-158f-7945-5933-564021086c79` registro:

**Windows**

```
Select-String UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

**Linux**

```
grep UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

### <a name="view-and-resume-jobs"></a>Visualización y reanudación de trabajos

Cada operación de transferencia creará un trabajo de AzCopy. Use el siguiente comando para ver el historial de trabajos:

```
azcopy jobs list
```

Para ver las estadísticas del trabajo, use el siguiente comando:

```
azcopy jobs show <job-id>
```

Para filtrar a las transferencias por estado, use el siguiente comando:

```
azcopy jobs show <job-id> --with-status=Failed
```

Use el comando siguiente para reanudar un trabajo de error/cancelado. Este comando usa su identificador, junto con el token de SAS, ya que no es persistente por motivos de seguridad:

```
azcopy jobs resume <job-id> --source-sas="<sas-token>"
azcopy jobs resume <job-id> --destination-sas="<sas-token>"
```

Al reanudar un trabajo, AzCopy examina el archivo de plan de trabajo. El archivo de plan enumera todos los archivos que se identificaron para el procesamiento cuando se creó el trabajo por primera vez. Al reanudar un trabajo, AzCopy intentará transferir todos los archivos que aparecen en el archivo de plan que ya no se ha transferido.