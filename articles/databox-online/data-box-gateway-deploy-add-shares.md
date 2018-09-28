---
title: Transferencia de datos con Azure Data Box Gateway | Microsoft Docs
description: Aprenda a agregar recursos compartidos en el dispositivo de Data Box Gateway y a conectarse a ellos.
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
ms.date: 09/24/2018
ms.author: alkohli
ms.custom: ''
ms.openlocfilehash: 44fa19191f08f0e35c06f0b7ea5a3e5e41611de3
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2018
ms.locfileid: "46979124"
---
# <a name="tutorial-transfer-data-with-azure-data-box-gateway-preview"></a>Tutorial: Transferencia de datos con Azure Data Box Gateway (versión preliminar)


## <a name="introduction"></a>Introducción

En este artículo se describe cómo agregar recursos compartidos a Data Box Gateway y cómo conectarse a ellos. Una vez agregados los recursos compartidos, el dispositivo Data Box Gateway puede transferir datos a Azure.

Este procedimiento tarda aproximadamente 10 minutos en completarse. 

En este tutorial, aprenderá a:

> [!div class="checklist"]
> * Agregar un recurso compartido
> * Conexión a un recurso compartido

> [!IMPORTANT]
> - Data Box Gateway está en versión preliminar. Antes de solicitar e implementar esta solución revise los [términos del servicio de Azure para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). 
 
## <a name="prerequisites"></a>Requisitos previos

Antes de agregar recursos compartidos a Data Box Gateway, asegúrese de que:

* Ha aprovisionado un dispositivo virtual y se ha conectado al mismo según se detalla en [Aprovisionamiento de Data Box Gateway en Hyper-V](data-box-gateway-deploy-provision-hyperv.md) o en [Aprovisionamiento de Data Box Gateway en VMware](data-box-gateway-deploy-provision-vmware.md). 

    El dispositivo virtual se activa como se detalla en [Conexión y activación de Azure Data Box Gateway](data-box-gateway-deploy-connect-setup-activate.md) y ya está listo para crear recursos compartidos y transferir datos.


## <a name="add-a-share"></a>Agregar un recurso compartido

Siga estos pasos en [Azure Portal](https://portal.azure.com/) para crear un recurso compartido.

1. Vuelva a Azure Portal. Vaya a **Todos los recursos** y busque el recurso de Data Box Gateway.
    
2. En la lista filtrada de recursos, seleccione el recurso de Data Box Gateway y, a continuación, vaya a **Información general**. Haga clic en **+ Agregar recurso compartido** en la barra de comandos del dispositivo.
   
   ![Agregar un recurso compartido](./media/data-box-gateway-deploy-add-shares/click-add-share.png)

4. En **Agregar recurso compartido**, especifique la configuración del recurso compartido. Proporcione un nombre exclusivo para el recurso compartido. 

   Los nombres de recursos compartidos solo pueden contener letras minúsculas, números y guiones. El nombre del recurso compartido debe tener entre 3 y 63 caracteres y empezar por una letra o un número. Antes y después de cada guion debe ir un carácter que no sea otro guión.
    
5. Seleccione un **tipo** de recurso compartido. El tipo puede ser SMB o NFS, siendo SMB el valor predeterminado. SMB es el estándar para los clientes de Windows y se usa NFS para los clientes de Linux. Dependiendo de si elige recursos compartidos SMB o NFS, las opciones que se presentan son ligeramente diferentes. 

6. Debe proporcionar una cuenta de almacenamiento donde residirá el recurso compartido. Se crea un contenedor en la cuenta de almacenamiento con el nombre del recurso compartido si el contenedor no existía previamente. Si el contenedor ya existe, se usará este. 
    
7. Elija el **servicio de almacenamiento** entre blob en bloques, blobs en páginas o archivos. El tipo de servicio elegido depende de en qué formato desea que los datos residan en Azure. Por ejemplo, en este caso, queremos que los datos residan como blobs en bloque en Azure y, por tanto, seleccionamos esa opción. Si elige blob en páginas, debe asegurarse de que los datos tienen 512 bytes alineados. Tenga en cuenta que VHDX siempre tiene 512 bytes alineados.
   
8. Este paso depende de si está creando un recurso compartido SMB o NFS. 
     
    - **Si crea un recurso compartido SMB**: en el campo Todos los privilegios del usuario local, elija entre **Crear nuevo** o **Usar existente**. Si crea un nuevo usuario local, proporcione el **nombre de usuario**, la **contraseña** y, a continuación, **confirme la contraseña**. Esto asigna los permisos al usuario local. Después de haber asignado los permisos aquí, puede utilizar el Explorador de archivos para modificarlos.
    
        ![Incorporación de recurso compartido de SMB](./media/data-box-gateway-deploy-add-shares/add-share-smb-1.png)
        
        Si selecciona **Permitir operaciones de solo lectura** para este recurso compartido de datos, tendrá la opción de especificar usuarios de solo lectura.
        
    - **Si crea un recurso compartido NFS**, debe proporcionar las direcciones IP de los clientes autorizados que pueden acceder al recurso compartido.

        ![Incorporación de un recurso compartido NFS](./media/data-box-gateway-deploy-add-shares/add-share-nfs-1.png)
   
9. Haga clic en **Crear** para crear el recurso compartido. 
    
    Se le notifica que la creación del recurso compartido está en curso. Una vez creado el recurso compartido con la configuración especificada, la hoja **Recursos compartidos** se actualiza para reflejar el nuevo recurso compartido. 
    
    ![Lista de recursos compartidos actualizada](./media/data-box-gateway-deploy-add-shares/updated-list-of-shares.png) 

## <a name="connect-to-the-share"></a>Conexión al recurso compartido

Realice estos pasos en el cliente de Windows Server conectado a Data Box Gateway para conectarse a los recursos compartidos.


1. Abra el símbolo del sistema. En el símbolo del sistema, escriba:

    `net use \\<IP address of the device>\<share name>  /u:<user name for the share>`

    Cuando se le solicite, escriba la contraseña del recurso compartido. A continuación se presenta un ejemplo de salida de este comando.

    ```powershell
    Microsoft Windows [Version 18.8.16299.192) 
    (c) 2817 microsoft Corporation. All rights reserved . 
    
    C: \Users\GatewayUser>net use \\10.10.10.60\newtestuser /u:Tota11yNewUser 
    Enter the password for 'TotallyNewUser' to connect to '10.10.10.60' • 
    The command completed successfully. 
    
    C: \Users\GatewayUser>
    ```   


2. Presione Windows + R. En la ventana **Ejecutar**, escriba `\\<device IP address>`. Haga clic en **OK**. Se abre el Explorador de archivos. Ahora podrá ver los recursos compartidos que creó como carpetas. Seleccione y haga doble clic en un recurso compartido (carpeta) para ver el contenido.
 
    ![Conexión a un recurso compartido SMB](./media/data-box-gateway-deploy-add-shares/connect-to-share2.png)-->

    Los datos se escriben en estos recursos compartidos según se generan y el dispositivo inserta los datos en la nube.

### <a name="connect-to-an-nfs-share"></a>Conexión a un recurso compartido NFS

Realice estos pasos en el cliente de Linux conectado a Data Box Edge.

1. Asegúrese de que el cliente tiene instalado el cliente NFSv4. Para instalarlo, use el comando siguiente:

   `sudo apt-get install nfs-common`

    Para más información, vaya a [Instalación de cliente NFSv4](https://help.ubuntu.com/community/SettingUpNFSHowTo#NFSv4_client).

2. Una vez instalado el cliente NFS, use el siguiente comando para montar el recurso compartido NFS que creó en el dispositivo Data Box Gateway:

   `sudo mount <device IP>:/<NFS share on device> /home/username/<Folder on local Linux computer>`

    Antes de configurar los montajes, asegúrese de que los directorios que actuarán como puntos de montaje en la máquina local ya se han creado y no contienen archivos ni subcarpetas.

    En el ejemplo siguiente se muestra cómo conectarse mediante NFS a un recurso compartido de Data Box Gateway. La dirección IP del dispositivo virtual es `10.10.10.60`; el recurso compartido `mylinuxshare2` se monta en la máquina virtual ubuntuVM y el punto de montaje es `/home/databoxubuntuhost/gateway`.

    `sudo mount -t nfs 10.10.10.60:/mylinuxshare2 /home/databoxubuntuhost/gateway`

> [!NOTE] 
> Las siguientes advertencias son aplicables a la versión preliminar:
> - Una vez creado un archivo en los recursos compartidos, no se puede cambiar el nombre del archivo. 
> - La eliminación de un archivo de un recurso compartido no elimina la entrada en la cuenta de almacenamiento.

## <a name="next-steps"></a>Pasos siguientes

En este tutorial, ha obtenido información acerca de varios temas relacionados con Data Box Gateway, como:

> [!div class="checklist"]
> * Agregar un recurso compartido
> * Conexión a un recurso compartido


Pase al siguiente tutorial para aprender a administrar Data Box Gateway.

> [!div class="nextstepaction"]
> [Uso de la interfaz de usuario web local para administrar una instancia de Data Box Gateway](http://aka.ms/dbg-docs)


