---
title: Copia de datos a un dispositivo Microsoft Azure Data Box mediante SMB | Microsoft Docs
description: Aprenda a copiar datos a un dispositivo Azure Data Box mediante SMB
services: databox
author: alkohli
ms.service: databox
ms.subservice: pod
ms.topic: tutorial
ms.date: 01/16/2019
ms.author: alkohli
ms.openlocfilehash: 2b6db4977b585b50168c2fa523db9210ca031ff3
ms.sourcegitcommit: a408b0e5551893e485fa78cd7aa91956197b5018
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/17/2019
ms.locfileid: "54359296"
---
# <a name="tutorial-copy-data-to-azure-data-box-via-smb"></a>Tutorial: Copia de datos a Azure Data Box Disk mediante SMB

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
    - Estar conectado a una red de alta velocidad. Es muy recomendable tener una conexión de 10 GbE como mínimo. Si no hay disponible una conexión 10 GbE, use un vínculo de datos de 1 GbE, pero las velocidades de copia se verán afectadas. 

## <a name="connect-to-data-box"></a>Conexión a un dispositivo Data Box

En función de la cuenta de almacenamiento seleccionada, Data Box crea hasta:
- Tres recursos compartidos para cada cuenta de almacenamiento asociada (GPv1 y GPv2).
- Un recurso compartido para las cuentas Premium Storage o Blob Storage.

En los recursos compartidos de blob en bloques y en páginas, las entidades de primer nivel son contenedores y las entidades de segundo nivel son blobs. En los recursos compartidos de Azure Files, las entidades de primer nivel son los recursos compartidos y las entidades de segundo nivel son los archivos.

En la tabla siguiente se muestra la ruta de acceso UNC a los recursos compartidos en la dirección URL de la ruta de acceso de Data Box y Azure Storage donde se cargan los datos. La dirección URL final de la ruta de acceso de Azure Storage se puede derivar a partir de la ruta de acceso UNC al recurso compartido.
 
|                   |                                                            |
|-------------------|--------------------------------------------------------------------------------|
| Blobs en bloques de Azure | <li>Ruta de acceso UNC a recursos compartidos: `\\<DeviceIPAddress>\<StorageAccountName_BlockBlob>\<ContainerName>\files\a.txt`</li><li>Dirección URL de Azure Storage: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li> |  
| Blobs en páginas de Azure  | <li>Ruta de acceso UNC a recursos compartidos: `\\<DeviceIPAddres>\<StorageAccountName_PageBlob>\<ContainerName>\files\a.txt`</li><li>Dirección URL de Azure Storage: `https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/files/a.txt`</li>   |  
| Archivos de Azure       |<li>Ruta de acceso UNC a recursos compartidos: `\\<DeviceIPAddres>\<StorageAccountName_AzFile>\<ShareName>\files\a.txt`</li><li>Dirección URL de Azure Storage: `https://<StorageAccountName>.file.core.windows.net/<ShareName>/files/a.txt`</li>        |      

Si usa un equipo host Windows Server, realice los pasos siguientes para conectarse a su dispositivo Data Box.

1. El primer paso es autenticarse e iniciar sesión. Vaya a **Connect and copy** (Conectar y copiar). Haga clic en **Get credentials** (Obtener credenciales) para obtener las credenciales de acceso para los recursos compartidos asociados con la cuenta de almacenamiento. 

    ![Obtención de las credenciales del recurso compartido 1](media/data-box-deploy-copy-data/get-share-credentials1.png)

2. En el cuadro de diálogo Access share and copy data (Acceder al recurso compartido y copiar datos), copie los valores de **Username** (Nombre de usuario) y **Password** (Contraseña) del recurso compartido. Haga clic en **OK**.
    
    ![Obtención de las credenciales del recurso compartido 1](media/data-box-deploy-copy-data/get-share-credentials2.png)

3. Para acceder a los recursos compartidos asociados con la cuenta de almacenamiento (*devicemanagertest1* en el ejemplo siguiente) desde el equipo host, abra una ventana Comandos. En el símbolo del sistema, escriba:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    Dependiendo del formato de los datos, las rutas de acceso de los recursos compartidos son las siguientes:
    - Blob en bloques de Azure: `\\10.126.76.172\devicemanagertest1_BlockBlob`
    - Blob en páginas de Azure: `\\10.126.76.172\devicemanagertest1_PageBlob`
    - Azure Files: `\\10.126.76.172\devicemanagertest1_AzFile`
    
4. Cuando se le solicite, escriba la contraseña del recurso compartido. En el ejemplo siguiente se muestra la conexión a un recurso compartido con el comando anterior.

    ```
    C:\Users\Databoxuser>net use \\10.126.76.172\devicemanagertest1_BlockBlob /u:devicemanagertest1
    Enter the password for 'devicemanagertest1' to connect to '10.126.76.172':
    The command completed successfully.
    ```

4. Presione Windows + R. En la ventana **Ejecutar**, escriba `\\<device IP address>`. Haga clic en **Aceptar** para abrir el Explorador de archivos.
    
    ![Conexión al recurso compartido mediante el Explorador de archivos 2](media/data-box-deploy-copy-data/connect-shares-file-explorer1.png)

    Ahora debería ver los recursos compartidos como carpetas.
    
    ![Conexión al recurso compartido mediante el Explorador de archivos 2](media/data-box-deploy-copy-data/connect-shares-file-explorer2.png)    

    **Cree siempre una carpeta para los archivos que se va a copiar en el recurso compartido y, después, copie los archivos en ella**. La carpeta que se creó en los recursos compartidos de blob en bloques y blob en páginas representa un contenedor en el que los datos se cargan como blobs. No se pueden copiar los archivos directamente en la carpeta *$root* de la cuenta de almacenamiento.
    
     

## <a name="copy-data-to-data-box"></a>Copia de datos a un dispositivo Data Box

Una vez que esté conectado a los recursos compartidos de Data Box, el siguiente paso es copiar los datos. Antes de comenzar la copia de datos, revise las consideraciones siguientes:

- Asegúrese de que copia los datos en los recursos compartidos que se corresponden con el formato de datos adecuado. Por ejemplo, copie los datos de blobs en bloques en la carpeta para blobs en bloques. Si el formato de los datos no coincide con el recurso compartido correspondiente, la carga de datos en Azure producirá un error más adelante.
-  Al copiar los datos, asegúrese de que el tamaño de los datos se ajusta a los límites descritos en los [límites de almacenamiento de Azure y de Data Box](data-box-limits.md).
- Si los datos que va a cargar el dispositivo Data Box los están cargando a la vez otras aplicaciones fuera del dispositivo Data Box, podría provocar errores en el trabajo de carga y daños en los datos.
- Es recomendable que:
    - No use SMB y NFS al mismo tiempo.
    - Copie los mismos datos en el mismo destino final en Azure. 
     
  En estos casos, no se puede determinar el resultado final.
- Cree siempre una carpeta para los archivos que se van a copiar en el recurso compartido y, después, copie los archivos en ella. La carpeta que se creó en los recursos compartidos de blob en bloques y blob en páginas representa un contenedor en el que los datos se cargan como blobs. No se pueden copiar los archivos directamente en la carpeta *$root* de la cuenta de almacenamiento.

Después de haberse conectado al recurso compartido SMB, inicie una copia de datos. Puede usar cualquier herramienta de copia de archivos compatible con SMB, como Robocopy, para copiar los datos. Con Robocopy se pueden iniciar varios trabajos de copia. Use el comando siguiente:
    
    robocopy <Source> <Target> * /e /r:3 /w:60 /is /nfl /ndl /np /MT:32 or 64 /fft /Log+:<LogFile> 
  
 Los atributos se describen en la tabla siguiente.
    
|Atributo  |DESCRIPCIÓN  |
|---------|---------|
|/e     |Copia los subdirectorios incluyendo los directorios vacíos.         |
|/r:     |Especifica el número de reintentos en las copias con errores.         |
|/w:     |Especifica el tiempo de espera entre reintentos, en segundos.         |
|/is     |Incluye los mismos archivos.         |
|/nfl     |Especifica que los nombres de archivo no se han registrado.         |
|/ndl    |Especifica que los nombres de directorio no se han registrado.        |
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

