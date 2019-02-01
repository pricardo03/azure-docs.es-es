---
title: 'Tutorial: Migración de datos locales a Azure Storage mediante AzCopy | Microsoft Docs'
description: En este tutorial, va a utilizar AzCopy para migrar datos al contenido de archivos, blobs y tablas o copiarlos. Realice la migración de los datos fácilmente desde el almacenamiento local a Azure Storage.
services: storage
author: roygara
ms.service: storage
ms.topic: tutorial
ms.date: 12/14/2017
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: 25a0867d9a2060989ce7206279b4acabe39b4ff3
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/31/2019
ms.locfileid: "55478277"
---
#  <a name="tutorial-migrate-on-premises-data-to-cloud-storage-by-using-azcopy"></a>Tutorial: Migración de datos locales al almacenamiento en la nube mediante AzCopy

AzCopy es una herramienta de línea de comandos para copiar datos de Azure Blob Storage, Azure Files y Azure Table Storage (o en ellos) mediante comandos simples. Los comandos están diseñados para optimizar el rendimiento. Con AzCopy, puede copiar datos entre un sistema de archivos y una cuenta de almacenamiento o entre cuentas de almacenamiento. AzCopy se puede usar para copiar datos desde los datos locales a una cuenta de almacenamiento.

Descargue la versión de AzCopy adecuada para su sistema operativo:

* [AzCopy en Linux](storage-use-azcopy-linux.md) se compila con .NET Core Framework. Tiene como destino plataformas Linux, ya que ofrece opciones de línea de comandos de estilo POSIX. 
* [AzCopy en Windows](storage-use-azcopy.md) se compila con .NET Framework. Ofrece opciones de línea de comandos de estilo Windows. 
 
En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Cree una cuenta de almacenamiento. 
> * Use AzCopy para cargar todos los datos.
> * Modifique los datos para fines de prueba.
> * Cree una tarea programada o trabajo cron para identificar los nuevos archivos que cargar.

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="prerequisites"></a>Requisitos previos

Para completar este tutorial, descargue la versión más reciente de AzCopy en [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#download-and-install-azcopy) o [Windows](https://aka.ms/downloadazcopy).

Si tiene Windows, necesitará [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx), ya que este tutorial lo utiliza para programar una tarea. Los usuarios de Linux utilizarán en su lugar el comando crontab.

[!INCLUDE [storage-create-account-portal-include](../../../includes/storage-create-account-portal-include.md)]

## <a name="create-a-container"></a>Crear un contenedor

El primer paso es crear un contenedor, ya que los blobs siempre se deben cargar en un contenedor. Los contenedores se utilizan como un método para organizar grupos de blobs de la misma forma que los archivos en el equipo, en carpetas. 

Siga estos pasos para crear un contenedor:

1. Seleccione el botón **Cuentas de almacenamiento** de la página principal y la cuenta de almacenamiento que ha creado.
2. Seleccione **Blobs** en **Servicios** y **Contenedor**. 

   ![Crear un contenedor](media/storage-azcopy-migrate-on-premises-data/CreateContainer.png)
 
El nombre de contenedor debe empezar por una letra o un número. Solo puede contener letras, números y el carácter de guion (-). Para conocer más reglas sobre la nomenclatura de los contenedores de almacenamiento, consulte [Asignación de nombres y referencias a contenedores, blobs y metadatos](/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata).

## <a name="upload-contents-of-a-folder-to-blob-storage"></a>Carga del contenido en una carpeta en Blob Storage

Puede usar AzCopy para cargar todos los archivos en una carpeta en Blob Storage en [Windows](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#upload-blobs-to-blob-storage) o [Linux](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy-linux#blob-download). Para cargar todos los blobs en una carpeta, escriba el siguiente comando de AzCopy:

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    azcopy \
        --source /mnt/myfolder \
        --destination https://myaccount.blob.core.windows.net/mycontainer \
        --dest-key <key> \
        --recursive

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:<key> /S
---

Reemplace `<key>` y `key` por el la clave de cuenta. En Azure Portal, la clave de cuenta se recupera al seleccionar **Claves de acceso** en **Configuración** en la cuenta de almacenamiento. Seleccione una clave y péguela en el comando de AzCopy. Si el contenedor de destino especificado no existe, AzCopy lo crea y carga el archivo en él. Actualice la ruta de acceso de origen al directorio de datos y reemplace **myaccount** en la dirección URL de destino con el nombre de la cuenta de almacenamiento.

Para cargar el contenido del directorio especificado repetidamente en Blob Storage, especifique la opción `--recursive` (Linux) o `/S` (Windows). Al ejecutar AzCopy con una de estas opciones, se cargan también todas las subcarpetas y sus archivos.

## <a name="upload-modified-files-to-blob-storage"></a>Carga de archivos modificados en Blob Storage

Puede utilizar AzCopy para [cargar archivos](https://docs.microsoft.com/azure/storage/common/storage-use-azcopy#other-azcopy-features) según la hora de la última modificación. Para probar esto, modifique o cree archivos en el directorio de origen para fines de prueba. Para cargar solo los archivos nuevos o actualizados, agregue el parámetro `--exclude-older` (Linux) o `/XO` (Windows) al comando AzCopy.

Si solo desea copiar los recursos de origen que no existen en el destino, especifique ambos parámetros `--exclude-older` y `--exclude-newer` (Linux) o `/XO` y `/XN` (Windows) en el comando de AzCopy. AzCopy carga solo los datos actualizados, en función de su marca de tiempo.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    azcopy \
    --source /mnt/myfolder \
    --destination https://myaccount.blob.core.windows.net/mycontainer \
    --dest-key <key> \
    --recursive \
    --exclude-older

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    AzCopy /Source:C:\myfolder /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:<key> /S /XO
---

## <a name="create-a-scheduled-task"></a>Creación de una tarea programada

Puede crear una tarea programada o un trabajo cron que ejecute un script de AzCopy. El script identifica y carga los datos locales nuevos en el almacenamiento en la nube en un intervalo de tiempo específico.

Copie el comando de AzCopy en un editor de texto. Actualice los valores de parámetro del comando de AzCopy con los valores adecuados. Guarde el archivo como `script.sh` (Linux) o `script.bat` (Windows) para AzCopy.

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

    azcopy --source /mnt/myfiles --destination https://myaccount.blob.core.windows.net/mycontainer --dest-key <key> --recursive --exclude-older --exclude-newer --verbose >> Path/to/logfolder/`date +\%Y\%m\%d\%H\%M\%S`-cron.log

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

    cd C:\Program Files (x86)\Microsoft SDKs\Azure\AzCopy
    AzCopy /Source: C:\myfolder  /Dest:https://myaccount.blob.core.windows.net/mycontainer /DestKey:<key> /V /XO /XN >C:\Path\to\logfolder\azcopy%date:~-4,4%%date:~-7,2%%date:~-10,2%%time:~-11,2%%time:~-8,2%%time:~-5,2%.log
---

AzCopy se ejecuta con la opción detallada `--verbose` (Linux) o `/V` (Windows). La salida se redirige a un archivo de registro.

En este tutorial, se utiliza [Schtasks](https://msdn.microsoft.com/library/windows/desktop/bb736357(v=vs.85).aspx) para crear una tarea programada en Windows. El comando [Crontab](http://crontab.org/) se utiliza para crear un trabajo cron en Linux.
 **Schtasks** permite a un administrador crear, eliminar, consultar, cambiar, ejecutar y finalizar tareas programadas en un equipo local o remoto. **Cron** permite a los usuarios de Unix y Linux ejecutar comandos o scripts en una fecha y hora especificadas mediante [expresiones cron](https://en.wikipedia.org/wiki/Cron#CRON_expression).

# <a name="linuxtablinux"></a>[Linux](#tab/linux)

Para crear un trabajo cron en Linux, escriba el siguiente comando en un terminal:

```bash
crontab -e
*/5 * * * * sh /path/to/script.sh
```

Especificar la expresión cron `*/5 * * * * ` en el comando indica que el script de shell `script.sh` debe ejecutarse cada cinco minutos. Puede programar que el script se ejecute en un momento determinado todos los días, meses o años. Para más información sobre cómo establecer la fecha y la hora de ejecución de un trabajo, consulte las [expresiones cron](https://en.wikipedia.org/wiki/Cron#CRON_expression).

# <a name="windowstabwindows"></a>[Windows](#tab/windows)

Para crear una tarea programada en Windows, escriba el siguiente comando en un símbolo del sistema o en PowerShell:

```cmd
schtasks /CREATE /SC minute /MO 5 /TN "AzCopy Script" /TR C:\Users\username\Documents\script.bat
```

El comando utiliza:
- El parámetro `/SC` para especificar una programación por minuto.
- El parámetro `/MO` para especificar un intervalo de cinco minutos.
- El parámetro `/TN` para especificar el nombre de la tarea.
- El parámetro `/TR` para especificar la ruta de acceso al archivo `script.bat`.

Para más información sobre cómo crear una tarea programada en Windows, consulte [Schtasks](https://technet.microsoft.com/library/cc772785(v=ws.10).aspx#BKMK_minutes).

---

Para asegurarse de que la tarea programada o el trabajo cron se ejecutan correctamente, cree nuevos archivos en el directorio `myfolder`. Espere cinco minutos para confirmar que los nuevos archivos se han cargado en la cuenta de almacenamiento. Vaya al directorio de registro para ver los registros de salida de la tarea programada o el trabajo cron.

## <a name="next-steps"></a>Pasos siguientes

Para más información sobre las formas de mover datos locales a Azure Storage y viceversa, siga este vínculo:

> [!div class="nextstepaction"]
> [Movimiento de datos hacia Azure Storage y desde este](https://docs.microsoft.com/azure/storage/common/storage-moving-data?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)  

Para más información sobre AzCopy explícitamente, elija el artículo apropiado a su sistema operativo:

> [!div class="nextstepaction"]
> [Transferencia de datos con AzCopy en Windows](storage-use-azcopy.md)
> [Transferencia de datos con AzCopy en Linux](storage-use-azcopy-linux.md)
