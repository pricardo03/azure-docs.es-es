---
title: Copia o transferencia de datos a Azure Storage con AzCopy v10 (versión preliminar) | Microsoft Docs
description: Use la utilidad AzCopy v10 (versión preliminar) para mover o copiar datos hacia o desde contenido de archivos, blobs y Data Lake. Copie datos a Azure Storage desde archivos locales o copie datos en o entre cuentas de almacenamiento. Migre fácilmente sus datos a Azure Storage.
services: storage
author: artemuwka
ms.service: storage
ms.topic: article
ms.date: 10/09/2018
ms.author: artemuwka
ms.component: common
ms.openlocfilehash: 2d0f3292a12505249ebc8594c58234a0c6e81a8a
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2019
ms.locfileid: "54212505"
---
# <a name="transfer-data-with-the-azcopy-v10-preview"></a>Transferencia de datos con AzCopy v10 (versión preliminar)

AzCopy v10 (versión preliminar) es la utilidad de línea de comandos de última generación para copiar datos desde y hacia Microsoft Azure Blob y File Storage, y ofrece una interfaz de línea de comandos rediseñada y una nueva arquitectura para transferencias confiables de datos de alto rendimiento. Con AzCopy, puede copiar datos entre un sistema de archivos y una cuenta de almacenamiento o entre cuentas de almacenamiento.

## <a name="whats-new-in-azcopy-v10"></a>Novedades de AzCopy v10

- Sincronización de un sistema de archivos en Azure Blob o viceversa. Mediante `azcopy sync <source> <destination>`. Ideal para escenarios de copia incremental.
- Admite las API de Azure Data Lake Storage Gen2. Use `myaccount.dfs.core.windows.net` como URI para llamar a las API de ADLS Gen2.
- Admite la copia de una cuenta completa (solo el Blob service) a otra cuenta.
- Ahora la copia de cuenta a cuenta usa las nuevas API [Put from URL](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url). No se necesita ninguna transferencia de datos al cliente, lo que acelera la transferencia.
- Enumeración y eliminación de archivos y blobs en una ruta de acceso dada.
- Admite patrones de caracteres comodín en una ruta de acceso, así como marcas --include y --exclude.
- Resistencia mejorada: cada instancia de AzCopy creará un pedido de trabajo y un archivo de registro relacionado. Puede ver y reiniciar los trabajos anteriores y reanudar los trabajos con errores. AzCopy también reintenta automáticamente una transferencia tras un error.
- Mejoras en el rendimiento general.

## <a name="download-and-install-azcopy"></a>Descarga e instalación de AzCopy

### <a name="latest-preview-version-v10"></a>Versión preliminar más reciente (v10)

Descargue la versión preliminar más reciente de AzCopy:
- [Windows](https://aka.ms/downloadazcopy-v10-windows)
- [Linux](https://aka.ms/downloadazcopy-v10-linux)
- [macOS](https://aka.ms/downloadazcopy-v10-mac)

### <a name="latest-production-version-v81"></a>Versión de producción más reciente (v8.1)

Descargue la [versión de producción más reciente de AzCopy para Windows](https://aka.ms/downloadazcopy).

### <a name="azcopy-supporting-table-storage-service-v73"></a>AzCopy admite el servicio Table Storage (v7.3)

Descargue [v7.3 AzCopy que admite la copia de datos desde y hacia el servicio Table Storage de Microsoft Azure](https://aka.ms/downloadazcopynet).

## <a name="post-installation-steps"></a>Pasos posteriores a la instalación

AzCopy v10 no requiere instalación. Abra una aplicación de línea de comandos preferida y vaya a la carpeta donde se encuentra el ejecutable `azcopy.exe`. Si lo desea, puede agregar la ubicación de la carpeta de AzCopy a la ruta de acceso del sistema.

## <a name="authentication-options"></a>Opciones de autenticación

AzCopy v10 permite usar las siguientes opciones al autenticarse en Azure Storage:
- **Azure Active Directory [Tipos admitidos en blob y ADLS Gen2]**. Use ```.\azcopy login``` para iniciar sesión con Azure Active Directory.  El usuario debe tener [asignado el rol "Colaborador de datos de blobs de almacenamiento"](https://docs.microsoft.com/azure/storage/common/storage-auth-aad-rbac) para escribir en el almacenamiento de blobs mediante la autenticación de Azure Active Directory.
- **Tokens de SAS [admitidos en Blob service y File service]**. Anexe el token de SAS a la ruta de acceso de blob en la línea de comandos para usarlo. Puede generar el token de SAS mediante Azure Portal, el [Explorador de Azure Storage](https://blogs.msdn.microsoft.com/jpsanders/2017/10/12/easily-create-a-sas-to-download-a-file-from-azure-storage-using-azure-storage-explorer/), [PowerShell](https://docs.microsoft.com/powershell/module/az.storage/new-azstorageblobsastoken) u otras herramientas que prefiera. Para más información, consulte los [ejemplos](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-shared-access-signature-part-2).

## <a name="getting-started"></a>Introducción

AzCopy v10 tiene una sintaxis simple autodocumentada. La sintaxis general tiene el siguiente aspecto cuando se ha iniciado sesión en Azure Active Directory:

```azcopy
.\azcopy <command> <arguments> --<flag-name>=<flag-value>
# Examples if you have logged into the Azure Active Directory:
.\azcopy copy <source path> <destination path> --<flag-name>=<flag-value>
.\azcopy cp "C:\local\path" "https://account.blob.core.windows.net/container" --recursive=true
.\azcopy cp "C:\local\path\myfile" "https://account.blob.core.windows.net/container/myfile"
.\azcopy cp "C:\local\path\*" "https://account.blob.core.windows.net/container"

# Examples if you are using SAS tokens to authenticate:
.\azcopy cp "C:\local\path" "https://account.blob.core.windows.net/container?sastoken" --recursive=true
.\azcopy cp "C:\local\path\myfile" "https://account.blob.core.windows.net/container/myfile?sastoken"
```

Así puede obtener una lista de los comandos disponibles:

```azcopy
.\azcopy -help
# Using the alias instead
.\azcopy -h
```

Para ver la página de ayuda y los ejemplos de un comando específico, ejecute el siguiente comando:

```azcopy
.\azcopy <cmd> -help
# Example:
.\azcopy cp -h
```

## <a name="create-a-blob-container-or-file-share"></a>Creación de un contenedor de blobs o recurso compartido de archivos 

**Creación de un contenedor de blobs**

```azcopy
.\azcopy make "https://account.blob.core.windows.net/container-name"
```

**Creación de un recurso compartido de archivos**

```azcopy
.\azcopy make "https://account.file.core.windows.net/share-name"
```

**Creación de un contenedor de blobs con ADLS Gen2**

Si ha habilitado espacios de nombres jerárquicos en la cuenta de almacenamiento de blobs, puede usar el comando siguiente para crear un nuevo sistema de archivos (contenedor de blobs) para poder cargar y descargar archivos en él.

```azcopy
.\azcopy make "https://account.dfs.core.windows.net/top-level-resource-name"
```

## <a name="copy-data-to-azure-storage"></a>Copia de datos a Azure Storage

Utilice el comando de copia para transferir datos desde el origen al destino. El origen o el destino pueden ser:
- El sistema de archivos local
- Blob/directorio virtual/URI de contenedor de Azure
- Archivo/directorio/URI de recurso compartido de archivos de Azure
- Directorio/URI de archivo/sistema de archivos de Azure Data Lake Storage Gen2

```azcopy
.\azcopy copy <source path> <destination path> --<flag-name>=<flag-value>
# Using alias instead
.\azcopy cp <source path> <destination path> --<flag-name>=<flag-value>
```

El siguiente comando carga todos los archivos en la carpeta `C:\local\path` de forma recursiva en el contenedor `mycontainer1` creando el directorio `path` en el contenedor:

```azcopy
.\azcopy cp "C:\local\path" "https://account.blob.core.windows.net/mycontainer1<sastoken>" --recursive=true
```

El siguiente comando carga todos los archivos en la carpeta `C:\local\path` (sin recurrencia en los subdirectorios) en el contenedor `mycontainer1`:

```azcopy
.\azcopy cp "C:\local\path\*" "https://account.blob.core.windows.net/mycontainer1<sastoken>"
```

Para obtener más ejemplos, use el siguiente comando:

```azcopy
.\azcopy cp -h
```

## <a name="copy-data-between-two-storage-accounts"></a>Copia de datos entre dos cuentas de almacenamiento

La copia de datos entre dos cuentas de almacenamiento usa la API [Put Block From URL](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) y no utiliza el ancho de banda de red del equipo cliente. Los datos se copian entre dos servidores de Azure Storage directamente, mientras que AzCopy simplemente orquesta la operación de copia. Actualmente esta opción solo está disponible para Blob Storage.

Para copiar los datos entre dos cuentas de almacenamiento, use el siguiente comando:
```azcopy
.\azcopy cp "https://myaccount.blob.core.windows.net/<sastoken>" "https://myotheraccount.blob.core.windows.net/<sastoken>" --recursive=true
```

> [!NOTE]
> El comando enumerará todos los contenedores de blobs y los copiará en la cuenta de destino. En este momento, AzCopy v10 admite copiar solo los blobs en bloques entre dos cuentas de almacenamiento. Se omitirán todos los demás objetos de la cuenta de almacenamiento (anexar blobs, blobs en páginas, archivos, tablas y colas).

## <a name="copy-a-vhd-image-to-a-storage-account"></a>Copia de una imagen VHD en una cuenta de almacenamiento

De forma predeterminada, AzCopy v10 carga datos en blobs en bloques. Sin embargo, si un archivo de origen tiene la extensión vhd, AzCopy v10 lo cargará de manera predeterminada en un blob en páginas. Este comportamiento no se puede configurar en este momento.

## <a name="sync-incremental-copy-and-delete-blob-storage-only"></a>Sync: copia y eliminación incrementales (solo Blob Storage)

> [!NOTE]
> Comando Sync sincroniza el contenido del origen en el destino y esto incluye la ELIMINACIÓN de archivos de destino si no existen en el origen. Asegúrese de usar el destino que pretende sincronizar.

Para sincronizar su sistema de archivos local en una cuenta de almacenamiento, use el siguiente comando:

```azcopy
.\azcopy sync "C:\local\path" "https://account.blob.core.windows.net/mycontainer1<sastoken>" --recursive=true
```

Del mismo modo, puede sincronizar un contenedor de blobs en un sistema de archivos local:

```azcopy
# If you're using Azure Active Directory authentication the sastoken is not required
.\azcopy sync "https://account.blob.core.windows.net/mycontainer1" "C:\local\path" --recursive=true
```

El comando le permite sincronizar de forma incremental el origen en el destino según las últimas marcas de tiempo modificadas. Si agrega o elimina un archivo en el origen, AzCopy v10 hará lo mismo en el destino. Antes de la eliminación, AzCopy le pedirá que confirme la eliminación de los archivos.

## <a name="advanced-configuration"></a>Configuración avanzada

### <a name="configure-proxy-settings"></a>Configuración de los valores de proxy

Para establecer la configuración de proxy para AzCopy v10, establezca la variable de entorno https_proxy mediante el comando siguiente:

```cmd
# For Windows:
set https_proxy=<proxy IP>:<proxy port>
# For Linux:
export https_proxy=<proxy IP>:<proxy port>
# For MacOS
export https_proxy=<proxy IP>:<proxy port>
```

### <a name="optimize-throughput"></a>Optimización del rendimiento

Establezca la variable de entorno AZCOPY_CONCURRENCY_VALUE para configurar el número de solicitudes simultáneas y controlar la capacidad de rendimiento y el consumo de recursos. De forma predeterminada, el valor se establece en 300. Si se reduce el valor, limitará el ancho de banda y el valor CPU usados por AzCopy v10.

```cmd
# For Windows:
set AZCOPY_CONCURRENCY_VALUE=<value>
# For Linux:
export AZCOPY_CONCURRENCY_VALUE=<value>
# For MacOS
export AZCOPY_CONCURRENCY_VALUE=<value>
```

## <a name="troubleshooting"></a>solución de problemas

AzCopy v10 crea archivos de registro y archivos de plan para todos los trabajos. Puede usar los registros para investigar y solucionar los problemas potenciales. Los registros contendrá el estado de error (UPLOADFAILED, COPYFAILED y DOWNLOADFAILED), la ruta de acceso completa y el motivo del error. Los registros de trabajos y los archivos de plan se encuentran en la carpeta %USERPROFILE\\.azcopy.

### <a name="review-the-logs-for-errors"></a>Revisión de los registros en busca errores

El siguiente comando obtiene todos los errores con el estado UPLOADFAILED desde el registro 04dc9ca9-158f-7945-5933-564021086c79:

```azcopy
cat 04dc9ca9-158f-7945-5933-564021086c79.log | grep -i UPLOADFAILED
```

### <a name="view-and-resume-jobs"></a>Visualización y reanudación de trabajos

Cada operación de transferencia creará un trabajo de AzCopy. Puede ver el historial de trabajos con el comando siguiente:

```azcopy
.\azcopy jobs list
```

Para ver las estadísticas del trabajo, use el siguiente comando:

```azcopy
.\azcopy jobs show <job-id>
```

Para filtrar a las transferencias por estado, use el siguiente comando:

```azcopy
.\azcopy jobs show <job-id> --with-status=Failed
```

Puede reanudar un trabajo erróneo o cancelado mediante su identificador, junto con el token de SAS (que no es persistente por motivos de seguridad):

```azcopy
.\azcopy jobs resume <jobid> --sourcesastokenhere --destinationsastokenhere
```

### <a name="change-the-default-log-level"></a>Cambiar el nivel de registro predeterminado

De forma predeterminada, el nivel de registro de AzCopy se establece en INFO. Si desea reducir el nivel de detalle del registro para ahorrar espacio en disco, sobrescriba el valor utilizando la opción ``--log-level``. Los niveles de registro disponibles son: DEBUG, INFO, WARNING, ERROR, PANIC y FATAL

## <a name="next-steps"></a>Pasos siguientes

Agradecemos sus comentarios en todo momento. Si tiene alguna pregunta, problemas o comentarios generales, envíelos a https://github.com/Azure/azure-storage-azcopy. Gracias.