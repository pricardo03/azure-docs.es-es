---
title: Copia de datos a un dispositivo Microsoft Azure Data Box | Microsoft Docs
description: Use este tutorial para saber cómo copiar datos a un dispositivo Azure Data Box
services: databox
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 09/25/2018
ms.author: alkohli
ms.openlocfilehash: 9bc84a9b08c4cfbdf7f24416c923e0dbd7076556
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/25/2018
ms.locfileid: "47161937"
---
# <a name="tutorial-copy-data-to-azure-data-box"></a>Tutorial: Copia de datos a un dispositivo Azure Data Box 

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
- Recurso compartido para blobs en bloques: *Mystoracct_BlockBlob/my-container/blob*
- Recurso compartido para blobs en páginas: *Mystoracct_PageBlob/my-container/blob*
- Recurso compartido de archivos: *Mystoracct_AzFile/my-share*

Dependiendo de si su dispositivo Data Box está conectado a un equipo host Windows Server o a un host Linux, los pasos necesarios para conectar y copiar pueden ser diferentes.

### <a name="connect-via-smb"></a>Conexión mediante SMB 

Si usa un equipo host Windows Server, realice los pasos siguientes para conectarse a su dispositivo Data Box.

1. El primer paso es autenticarse e iniciar sesión. Vaya a **Connect and copy** (Conectar y copiar). Haga clic en **Get credentials** (Obtener credenciales) para obtener las credenciales de acceso para los recursos compartidos asociados con la cuenta de almacenamiento. 

    ![Obtención de las credenciales del recurso compartido 1](media/data-box-deploy-copy-data/get-share-credentials1.png)

2. En el cuadro de diálogo Access share and copy data (Acceder al recurso compartido y copiar datos), copie los valores de **Username** (Nombre de usuario) y **Password** (Contraseña) del recurso compartido. Haga clic en **OK**.
    
    ![Obtención de las credenciales del recurso compartido 1](media/data-box-deploy-copy-data/get-share-credentials2.png)

3. Acceda a los recursos compartidos asociados con su cuenta de almacenamiento (Mystoracct en el ejemplo siguiente). Use la ruta de acceso `\\<IP of the device>\ShareName` para acceder a los recursos compartidos. Dependiendo del formato de sus datos, conéctese a los recursos compartidos (use el nombre del recurso compartido) en la siguiente dirección: 
    - *\\<IP address of the device>\Mystoracct_Blob*
    - *\\<IP address of the device>\Mystoracct_Page*
    - *\\<IP address of the device>\Mystoracct_AzFile*
    
    Para conectarse a los recursos compartidos desde el equipo host, abra una ventana de comandos. En el símbolo del sistema, escriba:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    Cuando se le solicite, escriba la contraseña del recurso compartido. En el ejemplo siguiente se muestra la conexión a un recurso compartido con el comando anterior.

    ```
    C:\Users\Databoxuser>net use \\10.126.76.172\devicemanagertest1_BlockBlob /u:devicemanagertest1
    Enter the password for 'devicemanagertest1' to connect to '10.126.76.172':
    The command completed successfully.
    ```

4. Presione Windows + R. En la ventana **Ejecutar**, escriba `\\<device IP address>`. Haga clic en **OK**. Se abre el Explorador de archivos.
    
    ![Conexión al recurso compartido mediante el Explorador de archivos 2](media/data-box-deploy-copy-data/connect-shares-file-explorer1.png)

5. Ahora podrá ver los recursos compartidos como carpetas. Cree una carpeta para los archivos que se van a copiar (en este caso, plantillas). En algunas ocasiones, las carpetas pueden mostrar una cruz gris. La cruz no indica que haya un error. La aplicación marca las carpetas para realizar un seguimiento del estado.
    
    ![Conexión al recurso compartido mediante el Explorador de archivos 2](media/data-box-deploy-copy-data/connect-shares-file-explorer2.png) ![Conexión al recurso compartido mediante el Explorador de archivos 2](media/data-box-deploy-copy-data/connect-shares-file-explorer2.png) 

### <a name="connect-via-nfs"></a>Conexión mediante NFS 

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

### <a name="copy-data-via-smb"></a>Copia de datos mediante SMB

Después de haberse conectado al recurso compartido SMB, inicie una copia de datos. 

Puede usar cualquier herramienta de copia de archivos compatible con SMB, como Robocopy, para copiar los datos. Con Robocopy se pueden iniciar varios trabajos de copia. Use el comando siguiente:
    
    robocopy <Source> <Target> * /e /r:3 /w:60 /is /nfl /ndl /np /MT:32 or 64 /fft /Log+:<LogFile> 
  
 Los atributos se describen en la tabla siguiente.
    
|Atributo  |DESCRIPCIÓN  |
|---------|---------|
|/e     |Copia los subdirectorios incluyendo los directorios vacíos.         |
|/r:     |Especifica el número de reintentos en las copias con errores.         |
|/w:     |Especifica el tiempo de espera entre reintentos, en segundos.         |
|/is     |Incluye los mismos archivos.         |
|/nfl     |Especifica que los nombres de archivo no se van a registrar.         |
|/ndl    |Especifica que los nombres de directorio no se van a registrar.        |
|/np     |Especifica que no se mostrará el progreso de la operación de copia (el número de archivos o directorios copiados hasta el momento). Mostrar el progreso reduce significativamente el rendimiento.         |
|/MT     | Especifica que se utilice subprocesamiento múltiple; se recomiendan 64 o 32 subprocesos. Esta opción que no se utiliza con los archivos cifrados. Es posible que deba separar archivos cifrados y sin cifrar. Sin embargo, copiar con un solo subproceso disminuye de forma significativa el rendimiento.           |
|/fft     | Utilice esta opción para reducir la granularidad de la marca de tiempo para cualquier sistema de archivos.        |
|/b     | Copia los archivos en modo de copia de seguridad.        |
|/z    | Copia los archivos en modo de reinicio; use esta opción si el entorno es inestable. Esta opción reduce el rendimiento porque realiza un registro adicional.      |
| /zb     | Usa el modo de reinicio. Si se deniega el acceso, esta opción utiliza el modo de copia de seguridad. Esta opción reduce el rendimiento porque utiliza puntos de control.         |
|/efsraw     | Copia todos los archivos cifrados en el modo sin procesar de EFS. Usar solo con los archivos cifrados.         |
|log+:<LogFile>| Anexa la salida al archivo de registro existente.|    
 
El ejemplo siguiente muestra la salida del comando robocopy para copiar archivos en el dispositivo Data Box.
    
    C:\Users>robocopy
        -------------------------------------------------------------------------------
        ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
    
        Started : Thursday, March 8, 2018 2:34:53 PM
            Simple Usage :: ROBOCOPY source destination /MIR
    
                    source :: Source Directory (drive:\path or \\server\share\path).
            destination :: Destination Dir  (drive:\path or \\server\share\path).
                    /MIR :: Mirror a complete directory tree.
    
        For more usage information run ROBOCOPY /?    
    
    ****  /MIR can DELETE files as well as copy them !
    
    C:\Users>Robocopy C:\Git\azure-docs-pr\contributor-guide \\10.126.76.172\devicemanagertest1_AzFile\templates /MT:32
    -------------------------------------------------------------------------------
        ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
    
        Started : Thursday, March 8, 2018 2:34:58 PM
        Source : C:\Git\azure-docs-pr\contributor-guide\
            Dest : \\10.126.76.172\devicemanagertest1_AzFile\templates\
    
        Files : *.*
    
        Options : *.* /DCOPY:DA /COPY:DAT /MT:32 /R:5 /W:60
    
    ------------------------------------------------------------------------------
    
    100%        New File                 206        C:\Git\azure-docs-pr\contributor-guide\article-metadata.md
    100%        New File                 209        C:\Git\azure-docs-pr\contributor-guide\content-channel-guidance.md
    100%        New File                 732        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-index.md
    100%        New File                 199        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pr-criteria.md
                New File                 178        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pull-request-co100%  .md
                New File                 250        C:\Git\azure-docs-pr\contributor-guide\contributor-guide-pull-request-et100%  e.md
    100%        New File                 174        C:\Git\azure-docs-pr\contributor-guide\create-images-markdown.md
    100%        New File                 197        C:\Git\azure-docs-pr\contributor-guide\create-links-markdown.md
    100%        New File                 184        C:\Git\azure-docs-pr\contributor-guide\create-tables-markdown.md
    100%        New File                 208        C:\Git\azure-docs-pr\contributor-guide\custom-markdown-extensions.md
    100%        New File                 210        C:\Git\azure-docs-pr\contributor-guide\file-names-and-locations.md
    100%        New File                 234        C:\Git\azure-docs-pr\contributor-guide\git-commands-for-master.md
    100%        New File                 186        C:\Git\azure-docs-pr\contributor-guide\release-branches.md
    100%        New File                 240        C:\Git\azure-docs-pr\contributor-guide\retire-or-rename-an-article.md
    100%        New File                 215        C:\Git\azure-docs-pr\contributor-guide\style-and-voice.md
    100%        New File                 212        C:\Git\azure-docs-pr\contributor-guide\syntax-highlighting-markdown.md
    100%        New File                 207        C:\Git\azure-docs-pr\contributor-guide\tools-and-setup.md
    ------------------------------------------------------------------------------
    
                    Total    Copied   Skipped  Mismatch    FAILED    Extras
        Dirs :         1         1         1         0         0         0
        Files :        17        17         0         0         0         0
        Bytes :     3.9 k     3.9 k         0         0         0         0          
    C:\Users>
       

Para optimizar el rendimiento, use los siguientes parámetros de robocopy al copiar los datos.

|    Plataforma    |    Archivos pequeños principalmente < 512 KB                           |    Archivos medianos principalmente, de 512 KB a 1 MB                      |    Archivos grandes principalmente > 1 MB                             |   
|----------------|--------------------------------------------------------|--------------------------------------------------------|--------------------------------------------------------|---|
|    Data Box         |    2 sesiones de Robocopy <br> 16 subprocesos por sesión    |    3 sesiones de Robocopy <br> 16 subprocesos por sesión    |    2 sesiones de Robocopy <br> 24 subprocesos por sesión    |  |


Para más información sobre el comando Robocopy, consulte [Robocopy and a few examples](https://social.technet.microsoft.com/wiki/contents/articles/1073.robocopy-and-a-few-examples.aspx) (Robocopy y algunos ejemplos).

Abra la carpeta de destino para ver y comprobar los archivos copiados. Si se produce algún error durante el proceso de copia, descargue los archivos de error para solucionar problemas.

Para garantizar la integridad de los datos, la suma de comprobación se calcula a medida que los datos se copian. Una vez completada la copia, compruebe el espacio utilizado y el espacio disponible en el dispositivo.
    
   ![Comprobación del espacio libre y utilizado en el panel](media/data-box-deploy-copy-data/verify-used-space-dashboard.png)

### <a name="copy-data-via-nfs"></a>Copia de datos mediante NFS

Si su equipo es un host Linux, use una utilidad de copia similar a Robocopy. Algunas de las alternativas disponibles en Linux son [rsync](https://rsync.samba.org/), [FreeFileSync](https://www.freefilesync.org/), [Unison](https://www.cis.upenn.edu/~bcpierce/unison/) o [Ultracopier](https://ultracopier.first-world.info/).  

El comando `cp` es una de las mejores opciones para copiar un directorio. Para más información sobre cómo usarlo, consulte las [páginas sobre cp](http://man7.org/linux/man-pages/man1/cp.1.html).

Si usa la opción rsync para una copia multiproceso, siga estas directrices:

 - Instale el paquete **CIFS Utils** o **NFS Utils** según el sistema de archivos que use el cliente Linux.

    `sudo apt-get install cifs-utils``sudo apt-get install nfs-utils`

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

El paso final consiste en preparar el dispositivo para el envío. En este paso, se desconectan todos los recursos compartidos de dispositivo. Una vez que se empiece a preparar el dispositivo para su envío, no se podrá acceder a los recursos compartidos.
1. Vaya a **Prepare to ship** (Preparación para el envío) y haga clic en **Start preparation** (Iniciar preparación). 
   
    ![Preparación para el envío 1](media/data-box-deploy-copy-data/prepare-to-ship1.png)

2. De forma predeterminada, las sumas de comprobación se procesan alineadas durante la acción de preparar para enviar. El proceso de la suma de comprobación puede tardar un tiempo en función del tamaño de los datos. Haga clic en **Start preparation** (Iniciar preparación).
    1. Los recursos compartidos de dispositivo se desconectan y el dispositivo se bloquea cuando se prepara para el envío.
        
        ![Preparación para el envío 1](media/data-box-deploy-copy-data/prepare-to-ship2.png) 
   
    2. Una vez completada la preparación del dispositivo, su estado se actualiza a *Ready to ship* (Listo para enviar). 
        
        ![Preparación para el envío 1](media/data-box-deploy-copy-data/prepare-to-ship3.png)

    3. Descargue la lista de archivos (manifiesto) que se copiaron en este proceso. Más adelante, puede usar esta lista para comprobar los archivos se cargaron en Azure.
        
        ![Preparación para el envío 1](media/data-box-deploy-copy-data/prepare-to-ship4.png)

3. Apague el dispositivo. Vaya a la página **Shut down or restart** (Apagar o reiniciar) y haga clic en **Shut down** (Apagar). Cuando se le pida confirmación, haga clic en **OK** (Aceptar) para continuar.
4. Retire los cables. El siguiente paso es enviar el dispositivo a Microsoft.

 
<!--## Appendix - robocopy parameters

This section describes the robocopy parameters used when copying the data to optimize the performance.

|    Platform    |    Mostly small files < 512 KB                           |    Mostly medium  files 512 KB-1 MB                      |    Mostly large files > 1 MB                             |   
|----------------|--------------------------------------------------------|--------------------------------------------------------|--------------------------------------------------------|---|
|    Data Box         |    2 Robocopy sessions <br> 16 threads per sessions    |    3 Robocopy sessions <br> 16 threads per sessions    |    2 Robocopy sessions <br> 24 threads per sessions    |  |
|    Data Box Heavy     |    6 Robocopy sessions <br> 24 threads per sessions    |    6 Robocopy sessions <br> 16 threads per sessions    |    6 Robocopy sessions <br> 16 threads per sessions    |   
|    Data Box Disk         |    4 Robocopy sessions <br> 16 threads per sessions             |    2 Robocopy sessions <br> 16 threads per sessions    |    2 Robocopy sessions <br> 16 threads per sessions    |   
-->

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Azure Data Box, como:

> [!div class="checklist"]
> * Conexión a un dispositivo Data Box
> * Copia de datos a un dispositivo Data Box
> * Preparación del envío a Data Box

Pase al siguiente tutorial para aprender a configurar y copiar datos en su dispositivo Data Box.

> [!div class="nextstepaction"]
> [Envío de un dispositivo Data Box a Microsoft](./data-box-deploy-picked-up.md)

