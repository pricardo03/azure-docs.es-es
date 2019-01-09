---
title: Copia de datos a un dispositivo Microsoft Azure Data Box mediante NFS | Microsoft Docs
description: Use este tutorial para saber cómo copiar datos a un dispositivo Azure Data Box
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 11/20/2018
ms.author: alkohli
ms.openlocfilehash: 7ba6bc2cf3cf5286719bc6da519aabb364302af3
ms.sourcegitcommit: 71ee622bdba6e24db4d7ce92107b1ef1a4fa2600
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/17/2018
ms.locfileid: "53550474"
---
# <a name="tutorial-copy-data-to-azure-data-box-via-nfs"></a>Tutorial: Copia de datos a Azure Data Box Disk mediante NFS 

En este tutorial se describe cómo conectarse al equipo host y copiar datos desde él mediante la interfaz de usuario web local y, a continuación, preparar el envío del dispositivo Data Box.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Conexión a un dispositivo Data Box
> * Copia de datos a un dispositivo Data Box
> * Preparación del envío del dispositivo Data Box

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, asegúrese de que:

1. Ha completado el [Tutorial: Instalación de un dispositivo Azure Data Box](data-box-deploy-set-up.md).
2. Ha recibido su dispositivo Data Box y el estado del pedido en el portal se actualiza a **Delivered** (Entregado).
3. Tiene un equipo host con los datos que desea copiar en su dispositivo Data Box. El equipo host debe:
    - Ejecutar un [sistema operativo admitido](data-box-system-requirements.md).
    - Estar conectado a una red de alta velocidad. Es muy recomendable tener una conexión de 10 GbE como mínimo. Si no hay disponible una conexión 10 GbE, se puede usar un vínculo de datos de 1 GbE, pero las velocidades de copia se verán afectadas. 

## <a name="connect-to-data-box"></a>Conexión a un dispositivo Data Box

En función de la cuenta de almacenamiento seleccionada, Data Box crea hasta:
- Tres recursos compartidos para cada cuenta de almacenamiento asociada (GPv1 y GPv2).
- Un recurso compartido para las cuentas Premium Storage o Blob Storage. 

En los recursos compartidos de blob en bloques y en páginas, las entidades de primer nivel son contenedores y las entidades de segundo nivel son blobs. En los recursos compartidos de Azure Files, las entidades de primer nivel son los recursos compartidos y las entidades de segundo nivel son los archivos.

Considere el ejemplo siguiente. 

- Cuenta de almacenamiento: *Mystoracct*
- Recurso compartido para el blob en bloques: *Mystoracct_BlockBlob/my-container/blob*
- Recurso compartido para el blob en páginas: *Mystoracct_PageBlob/my-container/blob*
- Recurso compartido para archivos: *Mystoracct_AzFile/my-share*

Si usa un equipo host Linux, realice los pasos siguientes para configurar un dispositivo Data Box para que pueda acceder a los clientes NFS.

1. Proporcione las direcciones IP de los clientes autorizados que pueden acceder al recurso compartido. En la interfaz de usuario web local, vaya a la página **Connect and copy** (Conectar y copiar). En **NFS settings** (Configuración de NFS), haga clic en **NFS client access** (Acceso de cliente NFS). 

    ![Configuración del acceso de cliente NFS 1](media/data-box-deploy-copy-data/nfs-client-access.png)

2. Proporcione la dirección IP del cliente NFS y haga clic en **Add** (Agregar). Para configurar el acceso para varios clientes NFS, repita este paso. Haga clic en **OK**.

    ![Configuración del acceso de cliente NFS 2](media/data-box-deploy-copy-data/nfs-client-access2.png)

2. Asegúrese de que el equipo host de Linux tiene instalada una [versión admitida](data-box-system-requirements.md) del cliente NFS. Use la versión específica para su distribución de Linux. 

3. Una vez instalado el cliente NFS, use el siguiente comando para montar el recurso compartido NFS en el dispositivo Data Box:

    `sudo mount <Data Box device IP>:/<NFS share on Data Box device> <Path to the folder on local Linux computer>`

    En el ejemplo siguiente se muestra cómo conectarse mediante NFS a un recurso compartido de Data Box. La dirección IP del dispositivo Data Box es `10.161.23.130`; el recurso compartido `Mystoracct_Blob` se monta en la máquina virtual ubuntuVM y el punto de montaje es `/home/databoxubuntuhost/databox`.

    `sudo mount -t nfs 10.161.23.130:/Mystoracct_Blob /home/databoxubuntuhost/databox`

## <a name="copy-data-to-data-box"></a>Copia de datos a un dispositivo Data Box

Una vez que esté conectado a los recursos compartidos de Data Box, el siguiente paso es copiar los datos. Antes de copiar los datos, no olvide revisar las consideraciones siguientes:

- Es responsabilidad suya asegurarse de que copia los datos en los recursos compartidos que se corresponden con el formato de datos adecuado. Por ejemplo, copie los datos de blobs en bloques en la carpeta para blobs en bloques. Si el formato de los datos no coincide con el recurso compartido correspondiente, la carga de datos en Azure producirá un error más adelante.
-  Al copiar los datos, asegúrese de que el tamaño de los datos se ajusta a los límites descritos en los [límites de almacenamiento de Azure y de Data Box](data-box-limits.md). 
- Si los datos que va a cargar el dispositivo Data Box los están cargando a la vez otras aplicaciones fuera del dispositivo Data Box, podría provocar errores en el trabajo de carga y daños en los datos.
- Se recomienda no usar SMB y NFS simultáneamente ni copiar los mismos datos al mismo destino final en Azure. En estos casos, no se puede determinar el resultado final.

Si su equipo es un host Linux, use una utilidad de copia similar a Robocopy. Algunas de las alternativas disponibles en Linux son [rsync](https://rsync.samba.org/), [FreeFileSync](https://www.freefilesync.org/), [Unison](https://www.cis.upenn.edu/~bcpierce/unison/) o [Ultracopier](https://ultracopier.first-world.info/).  

El comando `cp` es una de las mejores opciones para copiar un directorio. Para más información sobre cómo usarlo, consulte las [páginas sobre cp](http://man7.org/linux/man-pages/man1/cp.1.html).

Si usa la opción rsync para una copia multiproceso, siga estas directrices:

 - Instale el paquete **CIFS Utils** o **NFS Utils** según el sistema de archivos que use el cliente Linux.

    `sudo apt-get install cifs-utils`

    `sudo apt-get install nfs-utils`

 -  Instale **Rsync** y **Parallel** (varía según la versión de la distribución de Linux).

    `sudo apt-get install rsync`
   
    `sudo apt-get install parallel` 

 - Cree un punto de montaje.

    `sudo mkdir /mnt/databox`

 - Monte el volumen.

    `sudo mount -t NFS4  //Databox IP Address/share_name /mnt/databox` 

 - Refleje la estructura de directorios de carpetas.  

    `rsync -za --include='*/' --exclude='*' /local_path/ /mnt/databox`

 - Copie los archivos. 

    `cd /local_path/; find -L . -type f | parallel -j X rsync -za {} /mnt/databox/{}`

     donde j especifica el número de la paralelización, X = número de copias en paralelo

     Se recomienda empezar con 16 copias en paralelo y aumentar el número de subprocesos según los recursos disponibles.

## <a name="prepare-to-ship"></a>Preparación para el envío

[!INCLUDE [data-box-prepare-to-ship](../../includes/data-box-prepare-to-ship.md)]

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Azure Data Box, como:

> [!div class="checklist"]
> * Conexión a un dispositivo Data Box
> * Copia de datos a un dispositivo Data Box
> * Preparación del envío a Data Box

En el siguiente tutorial aprenderá a enviar su dispositivo Data Box a Microsoft.

> [!div class="nextstepaction"]
> [Envío de un dispositivo Data Box a Microsoft](./data-box-deploy-picked-up.md)

