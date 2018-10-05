---
title: Copia de datos a Microsoft Azure Data Box Disk | Microsoft Docs
description: Use este tutorial para saber cómo copiar datos a Azure Data Box Disk
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
ms.date: 09/28/2018
ms.author: alkohli
Customer intent: As an IT admin, I need to be able to order Data Box Disk to upload on-premises data from my server onto Azure.
ms.openlocfilehash: 776f70b6b24288006d52cb0e91797d1074180160
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452622"
---
# <a name="tutorial-copy-data-to-azure-data-box-disk-and-verify"></a>Tutorial: Copia de datos a Azure Data Box Disk y comprobación de los mismos

Este tutorial describe cómo copiar datos desde el equipo host y, a continuación, generar sumas de comprobación para comprobar su integridad.

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Copia de datos a Data Box Disk
> * Comprobación de datos

## <a name="prerequisites"></a>Requisitos previos

Antes de comenzar, asegúrese de que:
- Ha completado el [Tutorial: Instalación y configuración de Azure Data Box Disk](data-box-disk-deploy-set-up.md).
- Los discos se desbloquean y se conectan a un equipo cliente.
- El equipo cliente que se utiliza para copiar datos en los discos debe ejecutar un [sistema operativo compatible](data-box-disk-system-requirements.md).


## <a name="copy-data-to-disks"></a>Copia de datos a discos

Realice los pasos siguientes para conectarse y copiar datos desde el equipo a Data Box Disk.

1. Vea el contenido de la unidad desbloqueada. 

    ![Visualización del contenido de la unidad](media/data-box-disk-deploy-copy-data/data-box-disk-content.png)
 
2. Copie los datos que deben importarse como blobs en bloques en la carpeta BlockBlob. De forma similar, puede copiar datos como VHD/VHDX a la carpeta PageBlob. 

    Se crea un contenedor en la cuenta de almacenamiento de Azure para cada subcarpeta bajo las carpetas BlockBlob y PageBlob. Todos los archivos bajo las carpetas BlockBlob y PageBlob se copian en un contenedor predeterminado `$root` bajo la cuenta de Azure Storage. Todos los archivos del contenedor `$root` siempre se cargan como blobs en bloques.

    Si existen archivos y carpetas en el directorio raíz, debe pasarlos a una carpeta diferente antes de comenzar la copia de los datos.

    Siga los requisitos de nomenclatura de Azure para los nombres de blobs y de contenedores.

    #### <a name="azure-naming-conventions-for-container-and-blob-names"></a>Convenciones de nomenclatura de Azure para los nombres de contenedores y de blobs
    |Entidad   |Convenciones  |
    |---------|---------|
    |Nombres de contenedor, de blob en bloques y de blob en páginas     |Deben comenzar con una letra o un número, y solo pueden contener letras en minúsculas, números y guiones. Cada carácter de guión (-) debe estar inmediatamente precedido y seguido por una letra o un número. En los nombres, no se permiten guiones consecutivos. <br>Debe ser un nombre DNS válido, cuya longitud tenga entre 3 y 63 caracteres.          |
    |Nombres de blob para blob en bloques y blob en páginas    |Los nombres de blob distinguen mayúsculas de minúsculas y pueden contener cualquier combinación de caracteres. <br>Un nombre de blob debe tener entre 1 y 1024 caracteres.<br>Los caracteres de URL reservadas deben convertirse correspondientemente.<br>El número de segmentos de ruta de acceso que componen el nombre del blob no puede superar los 254. Un segmento de ruta de acceso es la cadena entre caracteres delimitadores consecutivos (por ejemplo, la barra diagonal '/') que se corresponden con el nombre de un directorio virtual.         |

    > [!IMPORTANT] 
    > Todos los contenedores y blobs deben adecuarse a las [convenciones de nomenclatura de Azure](data-box-disk-limits.md#azure-block-blob-and-page-blob-naming-conventions). Si no se siguen estas reglas, se producirá un error en la carga de datos en Azure.

3. Al copiar archivos, asegúrese de que estos no superen ~4.7 TiB para blobs en bloques y ~ 8 TiB para blobs en páginas. 
4. Puede usar arrastrar y colocar con el Explorador de archivos para copiar los datos. También puede usar cualquier herramienta de copia de archivos compatible con SMB, como Robocopy, para copiar los datos. Con el siguiente comando de Robocopy, se pueden iniciar varios trabajos de copia:

    `Robocopy <source> <destination>  * /MT:64 /E /R:1 /W:1 /NFL /NDL /FFT /Log:c:\RobocopyLog.txt` 
    
    Los parámetros y las opciones para el comando se tabulan como sigue:
    
    |Parámetros/opciones  |DESCRIPCIÓN |
    |--------------------|------------|
    |Origen            | Especifica la ruta de acceso del directorio de origen.        |
    |Destino       | Especifica la ruta de acceso del directorio de destino.        |
    |/E                  | Copia los subdirectorios incluyendo los directorios vacíos. |
    |/MT[:N]             | Crea copias multiproceso con N subprocesos, donde N es un entero entre 1 y 128. <br>El valor predeterminado de N es 8.        |
    |/R: <N>             | Especifica el número de reintentos en las copias con errores. El valor predeterminado de N es 1 000 000 (un millón de reintentos).        |
    |/W: <N>             | Especifica el tiempo de espera entre reintentos, en segundos. El valor predeterminado de N es 30 (30 segundos de tiempo de espera).        |
    |/NFL                | Especifica que los nombres de archivo no se van a registrar.        |
    |/NDL                | Especifica que los nombres de directorio no se van a registrar.        |
    |/FFT                | Se da por supuesto que se trata de tiempos de archivos FAT (precisión de dos segundos).        |
    |/Log:<Log File>     | Escribe la salida del estado en el archivo de registro (sobrescribe el archivo de registro existente).         |

    Se pueden usar varios discos en paralelo con varios trabajos que se ejecutan en cada disco. 

6. Se comprueba el estado de la copia cuando el trabajo está en curso. El ejemplo siguiente muestra la salida del comando robocopy para copiar archivos en Data Box Disk.

    ```
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
    
    C:\Users>Robocopy C:\Git\azure-docs-pr\contributor-guide \\10.126.76.172\devicemanagertest1_AzFile\templates /MT:64 /E /R:1 /W:1 /FFT 
    -------------------------------------------------------------------------------
       ROBOCOPY     ::     Robust File Copy for Windows
    -------------------------------------------------------------------------------
    
      Started : Thursday, March 8, 2018 2:34:58 PM
       Source : C:\Git\azure-docs-pr\contributor-guide\
         Dest : \\10.126.76.172\devicemanagertest1_AzFile\templates\
    
        Files : *.*
    
      Options : *.* /DCOPY:DA /COPY:DAT /MT:8 /R:1000000 /W:30
    
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
       Times :   0:00:05   0:00:00                       0:00:00   0:00:00
        
       Speed :                5620 Bytes/sec.
       Speed :               0.321 MegaBytes/min.
       Ended : Thursday, March 8, 2018 2:34:59 PM
        
    C:\Users>
    ```
 
    
7. Abra la carpeta de destino para ver y comprobar los archivos copiados. Si se produce algún error durante el proceso de copia, descargue los archivos de registro para solucionar problemas. Los archivos de registro se ubican según lo especificado en el comando robocopy.
 


> [!IMPORTANT]
> - Es responsabilidad suya asegurarse de que copia los datos en las carpetas que se corresponden con el formato de datos adecuado. Por ejemplo, copie los datos de blobs en bloques en la carpeta para blobs en bloques. Si el formato de los datos no coincide con la carpeta correspondiente (tipo de almacenamiento), en un paso posterior, la carga de datos en Azure produce un error.
> -  Al copiar los datos, asegúrese de que su tamaño se ajusta a los límites descritos en los [límites de almacenamiento de Azure y Data Box Disk](data-box-disk-limits.md). 
> - Si los datos que carga Data Box Disk están siendo cargados a la vez por otras aplicaciones fuera de Data Box Disk, podría provocar errores en el trabajo de carga y daños en los datos.

## <a name="verify-data"></a>Comprobación de datos 

Para comprobar los datos, realice los pasos siguientes.

1. Ejecute `DataBoxDiskValidation.cmd` para la validación de la suma de comprobación en la carpeta *AzureImportExport* de la unidad. 
    
    ![Salida de la herramienta de validación Data Box Disk](media/data-box-disk-deploy-copy-data/data-box-disk-validation-tool-output.png)

2. Elija la opción más adecuada. **Se recomienda validar siempre los archivos y generar sumas de comprobación mediante la selección de la opción 2**. Según el tamaño de los datos, este paso puede tardar un rato. Una vez completado el script, salga de la ventana de comandos. Si se produce algún error durante la validación y la generación de la suma de comprobación, se le notificará y también se le proporcionará un vínculo a los registros de errores.

    ![Salida de la suma de comprobación](media/data-box-disk-deploy-copy-data/data-box-disk-checksum-output.png)

    > [!TIP]
    > - Restablezca la herramienta entre dos ejecuciones.
    > - Utilice la opción 1 para validar los archivos que solo tratan con conjuntos de datos grandes que contienen archivos pequeños (~KB). En estos casos, la generación de la suma de comprobación puede llevar mucho tiempo y el rendimiento puede ser muy lento.

3. Si usa varios discos, ejecute el comando para cada disco.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha aprendido sobre temas relacionados con Azure Data Box Disk; por ejemplo:

> [!div class="checklist"]
> * Copia de datos a Data Box Disk
> * Comprobar la integridad de los datos

Pase al siguiente tutorial para obtener información sobre cómo devolver Data Box Disk y comprobar la carga de datos en Azure.

> [!div class="nextstepaction"]
> [Devolución de Azure Data Box Disk a Microsoft](./data-box-disk-deploy-picked-up.md)

