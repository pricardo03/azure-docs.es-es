---
title: Transferencia de datos con Azure Data Box Edge | Microsoft Docs
description: Aprenda a agregar recursos compartidos en el dispositivo de Data Box Edge y a conectarse a ellos.
services: databox-edge-gateway
documentationcenter: NA
author: alkohli
manager: twooley
editor: ''
ms.assetid: ''
ms.service: databox-edge-gateway
ms.devlang: NA
ms.topic: tutorial
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/08/2018
ms.author: alkohli
ms.custom: ''
Customer intent: As an IT admin, I need to understand how to add and connect to shares on Data Box Edge so I can use it to transfer data to Azure.
ms.openlocfilehash: b5c63a255547bae03acbec771593eac97d474003
ms.sourcegitcommit: 26cc9a1feb03a00d92da6f022d34940192ef2c42
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/06/2018
ms.locfileid: "48833101"
---
# <a name="tutorial-transfer-data-with-azure-data-box-edge-preview"></a>Tutorial: Transferencia de datos con Azure Data Box Edge (versión preliminar)

En este artículo se describe cómo agregar recursos compartidos a Data Box Edge y cómo conectarse a ellos. Una vez agregados los recursos compartidos, el dispositivo de Data Box Edge puede transferir datos a Azure.

Este procedimiento tarda aproximadamente 10 minutos en completarse. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Agregar un recurso compartido
> * Conexión a un recurso compartido

> [!IMPORTANT]
> Data Box Edge se encuentra en versión preliminar. Antes de solicitar e implementar esta solución revise los [términos del servicio de Azure para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 
 
## <a name="prerequisites"></a>Requisitos previos

Antes de agregar recursos compartidos a Data Box Edge, asegúrese de que:

* Ha instalado el dispositivo físico tal como se detalla en [Instalación de un dispositivo de Data Box Edge](data-box-edge-deploy-install.md). 

    El dispositivo físico está activado como se indica en [Connect and activate your Azure Data Box Edge](data-box-edge-deploy-connect-setup-activate.md) (Conexión y activación de Azure Data Box Edge). El dispositivo está listo para crear recursos compartidos y transferir datos.


## <a name="add-a-share"></a>Agregar un recurso compartido

Siga estos pasos en [Azure Portal](https://portal.azure.com/) para crear un recurso compartido.

1. Vuelva a Azure Portal. Vaya a **Todos los recursos** y busque el recurso de Data Box Edge.
    
2. En la lista filtrada de recursos, seleccione el recurso Data Box Edge y vaya a **Información general**. Haga clic en **+ Agregar recurso compartido** en la barra de comandos del dispositivo.
   
   ![Agregar un recurso compartido](./media/data-box-edge-deploy-add-shares/click-add-share.png)

3. En **Agregar recurso compartido**, especifique la configuración del recurso compartido. Proporcione un nombre exclusivo para el recurso compartido. 

   Los nombres de recursos compartidos solo pueden contener letras minúsculas, números y guiones. El nombre del recurso compartido debe tener entre 3 y 63 caracteres y empezar por una letra o un número. Antes y después de cada guion debe ir un carácter que no sea otro guión.
    
    1. Seleccione un **tipo** de recurso compartido. El tipo puede ser SMB o NFS, siendo SMB el valor predeterminado. SMB es el estándar para los clientes de Windows y se usa NFS para los clientes de Linux. 

    2. Dependiendo de si elige recursos compartidos SMB o NFS, las opciones que se presentan son ligeramente diferentes. 

    3. Debe proporcionar una cuenta de almacenamiento donde residirá el recurso compartido. Se crea un contenedor en la cuenta de almacenamiento con el nombre del recurso compartido si el contenedor no existe previamente. Si el contenedor ya existe, se usará este. 
    
    4. Elija el **servicio de almacenamiento** entre blob en bloques, blobs en páginas o archivos. El tipo de servicio elegido depende de en qué formato desea que los datos residan en Azure. Por ejemplo, en este caso, queremos que los datos residan como blobs en bloque en Azure y, por tanto, seleccionamos esa opción. Si elige blob en páginas, asegúrese de que los datos tienen una alineación de 512 bytes. Por ejemplo, un VHDX siempre tiene una alineación de 512 bytes.
   
    5. Este paso depende de si está creando un recurso compartido SMB o NFS. 
     
        - **Si crea un recurso compartido SMB**: en el campo Todos los privilegios del usuario local, elija entre **Crear nuevo** o **Usar existente**. Si crea un nuevo usuario local, proporcione el **nombre de usuario**, la **contraseña** y, a continuación, **confirme la contraseña**. Esto asigna los permisos al usuario local. Después de haber asignado los permisos aquí, puede utilizar el Explorador de archivos para modificarlos.

            Si selecciona **Permitir operaciones de solo lectura** para este recurso compartido de datos, tendrá la opción de especificar usuarios de solo lectura.

            ![Incorporación de recurso compartido de SMB](./media/data-box-edge-deploy-add-shares/add-share-smb-1.png)
   
        - **Si crea un recurso compartido NFS**, debe proporcionar las direcciones IP de los clientes autorizados que pueden acceder al recurso compartido.

            ![Incorporación de un recurso compartido NFS](./media/data-box-edge-deploy-add-shares/add-share-nfs-1.png)
   
4. Haga clic en **Crear** para crear el recurso compartido. 
    
    Se le notifica que la creación del recurso compartido está en curso. Una vez creado el recurso compartido con la configuración especificada, la hoja **Recursos compartidos** se actualiza para reflejar el nuevo recurso compartido. 
    
    ![Lista de recursos compartidos actualizada](./media/data-box-edge-deploy-add-shares/updated-list-of-shares.png) 

## <a name="connect-to-the-share"></a>Conexión al recurso compartido

Ahora, puede conectarse a uno o varios recursos compartidos que creó en el paso anterior. Dependiendo de si tiene un recurso compartido SMB o NFS, los pasos pueden ser diferentes. 

### <a name="connect-to-an-smb-share"></a>Conexión a un recurso compartido SMB

Realice estos pasos en el cliente de Windows Server conectado a Data Box Edge para conectarse a los recursos compartidos.


1. Abra el símbolo del sistema. En el símbolo del sistema, escriba:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    Cuando se le solicite, escriba la contraseña del recurso compartido. A continuación se presenta un ejemplo de salida de este comando.

    ```powershell
    Microsoft Windows [Version 10.0.16299.192) 
    (c) 2017 microsoft Corporation. All rights reserved . 
    
    C: \Users\DataBoxEdgeUser>net use \\10.10.10.60\newtestuser /u:Tota11yNewUser 
    Enter the password for 'TotallyNewUser' to connect to '10.10.10.60': 
    The command completed successfully. 
    
    C: \Users\DataBoxEdgeUser>
    ```   


2. Presione Windows + R. En la ventana **Ejecutar**, escriba `\\<device IP address>`. Haga clic en **OK**. Se abre el Explorador de archivos. Ahora podrá ver los recursos compartidos que creó como carpetas. Seleccione y haga doble clic en un recurso compartido (carpeta) para ver el contenido.
 
    ![Conexión a un recurso compartido SMB](./media/data-box-edge-deploy-add-shares/connect-to-share2.png)

    Los datos se escriben en estos recursos compartidos según se generan y el dispositivo inserta los datos en la nube.

### <a name="connect-to-an-nfs-share"></a>Conexión a un recurso compartido NFS

Realice estos pasos en el cliente de Linux conectado a Data Box Edge.

1. Asegúrese de que el cliente tiene instalado el cliente NFSv4. Para instalarlo, use el comando siguiente:

   `sudo apt-get install nfs-common`

    Para más información, vaya a [Instalación de cliente NFSv4](https://help.ubuntu.com/community/SettingUpNFSHowTo#NFSv4_client).

2. Una vez instalado el cliente NFS, use el siguiente comando para montar el recurso compartido NFS que creó en el dispositivo Data Box Edge:

   `sudo mount <device IP>:/<NFS share on device> /home/username/<Folder on local Linux computer>`

    Antes de montar los recursos compartidos, asegúrese de que los directorios que actuarán como puntos de montaje en la máquina local ya se han creado. Estos directorios no deben contener ningún archivo ni subcarpeta.

    En el ejemplo siguiente se muestra cómo conectarse mediante NFS a un recurso compartido de Data Box Edge. La dirección IP del dispositivo es `10.10.10.60`. El recurso compartido `mylinuxshare2` está montado en la máquina ubuntuVM. El punto de montaje del recurso compartido es `/home/databoxubuntuhost/edge`.

    `sudo mount -t nfs 10.10.10.60:/mylinuxshare2 /home/databoxubuntuhost/Edge`

> [!NOTE] 
> Las siguientes advertencias son aplicables a la versión preliminar:
> - Una vez creado un archivo en los recursos compartidos, no se puede cambiar el nombre del archivo. 
> - La eliminación de un archivo de un recurso compartido no elimina la entrada en la cuenta de almacenamiento.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Data Box Edge, como:

> [!div class="checklist"]
> * Agregar un recurso compartido
> * Conexión a un recurso compartido


Pase al siguiente tutorial para aprender a transformar datos con Data Box Edge.

> [!div class="nextstepaction"]
> [Transformación de datos con Data Box Edge](./data-box-edge-deploy-configure-compute.md)


