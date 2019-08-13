---
title: Configuración, optimización y solución de problemas de AzCopy con Azure Storage | Microsoft Docs
description: Configure, optimice y solucione problemas de AzCopy.
services: storage
author: normesta
ms.service: storage
ms.topic: article
ms.date: 07/25/2019
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 3773f9a8464dc94436d6d2503b173d4674033ab1
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/26/2019
ms.locfileid: "68565049"
---
# <a name="configure-optimize-and-troubleshoot-azcopy"></a>Configuración, optimización y solución de problemas de AzCopy

AzCopy es una utilidad de línea de comandos que puede usar para copiar blobs o archivos a o desde una cuenta de almacenamiento. Este artículo le ayuda a realizar tareas de configuración avanzadas y a solucionar los problemas que puedan surgir al usar AzCopy.

> [!NOTE]
> Si busca contenido que le ayude a empezar a trabajar con AzCopy, vea cualquiera de los artículos siguientes:
> - [Introducción a AzCopy](storage-use-azcopy-v10.md)
> - [Transferencia de datos con AzCopy y Blob Storage](storage-use-azcopy-blobs.md)
> - [Transferencia de datos con AzCopy y File Storage](storage-use-azcopy-files.md)
> - [Transferencia de datos con AzCopy y cubos de Amazon S3](storage-use-azcopy-s3.md)

## <a name="configure-proxy-settings"></a>Configuración de los valores de proxy

Para configurar las opciones de proxy para AzCopy, establezca la variable de entorno `https_proxy`. Si ejecuta AzCopy en Windows, AzCopy detecta automáticamente la configuración de proxy, por lo que no tiene que usar esta opción en Windows. Si decide usar esta opción en Windows, invalidará la detección automática.

| Sistema operativo | Get-Help  |
|--------|-----------|
| **Windows** | En un símbolo del sistema, use: `set https_proxy=<proxy IP>:<proxy port>`<br> En PowerShell, use: `$env:https_proxy="<proxy IP>:<proxy port>"`|
| **Linux** | `export https_proxy=<proxy IP>:<proxy port>` |
| **macOS** | `export https_proxy=<proxy IP>:<proxy port>` |

En la actualidad, AzCopy no admite servidores proxy que requieren autenticación con NTLM o Kerberos.

## <a name="optimize-throughput"></a>Optimización del rendimiento

Puede usar la marca `cap-mbps` para colocar un límite superior en la velocidad de datos de rendimiento. Por ejemplo, el siguiente comando limita el rendimiento a `10` megabits (MB) por segundo.

```azcopy
azcopy cap-mbps 10
```

El rendimiento puede disminuir al transferir archivos pequeños. Puede aumentar el rendimiento si establece la variable de entorno `AZCOPY_CONCURRENCY_VALUE`. Esta variable especifica el número de solicitudes simultáneas que pueden producirse.  Si el equipo tiene menos de cinco CPU, el valor de esta variable se establece en `32`. En caso contrario, el valor predeterminado es igual a 16 multiplicado por el número de CPU. El valor máximo predeterminado de esta variable es `300`, pero puede establecerlo manualmente en un valor superior o inferior.

| Sistema operativo | Get-Help  |
|--------|-----------|
| **Windows** | `set AZCOPY_CONCURRENCY_VALUE=<value>` |
| **Linux** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |
| **macOS** | `export AZCOPY_CONCURRENCY_VALUE=<value>` |

Use `azcopy env` para comprobar el valor actual de esta variable.  Si el valor está en blanco, la variable `AZCOPY_CONCURRENCY_VALUE` se establece en el valor predeterminado de `300`.

## <a name="change-the-location-of-the-log-files"></a>Cambie la ubicación del archivo de registro.

De forma predeterminada, los archivos de registro se encuentran en el directorio `%USERPROFILE\\.azcopy` de Windows o en el directorio `$HOME\\.azcopy` en Mac y Linux. Si es necesario, puede cambiar esta ubicación mediante los comandos siguientes.

| Sistema operativo | Get-Help  |
|--------|-----------|
| **Windows** | `set AZCOPY_LOG_LOCATION=<value>` |
| **Linux** | `export AZCOPY_LOG_LOCATION=<value>` |
| **macOS** | `export AZCOPY_LOG_LOCATION=<value>` |

Use `azcopy env` para comprobar el valor actual de esta variable. Si el valor está en blanco, los registros se escriben en la ubicación predeterminada.

## <a name="change-the-default-log-level"></a>Cambiar el nivel de registro predeterminado

De forma predeterminada, el nivel de registro de AzCopy se establece en `INFO`. Si quiere reducir el nivel de detalle del registro para ahorrar espacio en disco, sobrescriba este valor mediante la opción ``--log-level``. 

Los niveles de registro disponibles son: `DEBUG`, `INFO`, `WARNING`, `ERROR`, `PANIC` y `FATAL`.

## <a name="troubleshoot-issues"></a>Solución de problemas

AzCopy crea archivos de registro y de plan para cada trabajo. Puede usar los registros para investigar y solucionar los problemas potenciales. 

Los registros contendrán el estado de error (`UPLOADFAILED`, `COPYFAILED` y `DOWNLOADFAILED`), la ruta de acceso completa y el motivo del error.

De forma predeterminada, los archivos de registro y de plan se encuentran en el directorio `%USERPROFILE\\.azcopy` de Windows o en el directorio `$HOME\\.azcopy` en Mac y Linux.

> [!IMPORTANT]
> Al enviar una solicitud a Soporte técnico de Microsoft (o al solucionar el problema con la participación de terceros), comparta la versión censurada del comando que quiere ejecutar. Esto garantiza que la SAS no se comparta de forma accidental con nadie. Puede encontrar la versión censurada al principio del archivo de registro.

### <a name="review-the-logs-for-errors"></a>Revisión de los registros en busca errores

El comando siguiente obtiene todos los errores con el estado `UPLOADFAILED` del registro `04dc9ca9-158f-7945-5933-564021086c79`:

**Windows (PowerShell)**

```
Select-String UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

**Linux**

```
grep UPLOADFAILED .\04dc9ca9-158f-7945-5933-564021086c79.log
```

### <a name="view-and-resume-jobs"></a>Visualización y reanudación de trabajos

Cada operación de transferencia creará un trabajo de AzCopy. Use el comando siguiente para ver el historial de trabajos:

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

Use el comando siguiente para reanudar un trabajo con error o cancelado. Este comando usa su identificador, junto con el token de SAS, ya que no es persistente por motivos de seguridad:

```
azcopy jobs resume <job-id> --source-sas="<sas-token>"
azcopy jobs resume <job-id> --destination-sas="<sas-token>"
```

Al reanudar un trabajo, AzCopy examina el archivo de plan de trabajo. En el archivo de plan se enumeran todos los archivos que se han identificado para el procesamiento al crear el trabajo por primera vez. Al reanudar un trabajo, AzCopy intentará transferir todos los archivos que aparecen en el archivo de plan que no se han transferido todavía.