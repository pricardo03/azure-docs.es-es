---
title: Creación de un volumen de Azure NetApp Files | Microsoft Docs
description: Describe cómo crear un volumen de Azure NetApp Files.
services: azure-netapp-files
documentationcenter: ''
author: b-juche
manager: ''
editor: ''
ms.assetid: ''
ms.service: azure-netapp-files
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 4/23/2019
ms.author: b-juche
ms.openlocfilehash: 53b2742cf92f3a3df346ba3557c718b8d7a11a4e
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/28/2019
ms.locfileid: "64719442"
---
# <a name="create-a-volume-for-azure-netapp-files"></a>Creación de un volumen de Azure NetApp Files

Cada grupo de capacidad puede tener un máximo de 500 volúmenes. El consumo de la capacidad de un volumen se descuenta de la capacidad aprovisionada de su grupo. Azure Files de NetApp admite volúmenes NFS y SMBv3. 

## <a name="before-you-begin"></a>Antes de empezar 
Debe haber configurado un grupo de capacidad.   
[Configuración de un grupo de capacidad](azure-netapp-files-set-up-capacity-pool.md)   
Debe haber una subred delegada en Azure NetApp Files.  
[Delegación de una subred en Azure NetApp Files](azure-netapp-files-delegate-subnet.md)

## <a name="create-an-nfs-volume"></a>Crear un volumen NFS

1.  Haga clic en el **volúmenes** hoja en la hoja de grupos de capacidad. 

    ![Vaya a volúmenes](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2.  Haga clic en **+ Agregar volumen** para crear un volumen.  
    La creación, aparece una ventana de volumen.

3.  En la creación de una ventana de volumen, haga clic en **crear** y proporcione información para los campos siguientes:   
    * **Nombre del volumen**      
        Especifique el nombre para el volumen que va a crear.   

        Un nombre de volumen debe ser único dentro de cada grupo de capacidades. Debe tener tres caracteres de longitud, como mínimo. Puede usar cualquier carácter alfanumérico.

    * **Grupo de capacidades**  
        Especifique el grupo de capacidad en la que desea que el volumen que se va a crear.

    * **Cuota**  
        Especifique la cantidad de almacenamiento lógico que se asigna al volumen.  

        El campo **Cuota disponible** muestra la cantidad de espacio no utilizado en el grupo de capacidad elegido que puede usar para crear un nuevo volumen. El tamaño del volumen nuevo no debe superar la cuota disponible.  

    * **Red virtual**  
        Especifique la red virtual de Azure (Vnet) desde la que desea tener acceso al volumen.  

        La red virtual que especifique debe tener una subred delegada en Azure NetApp Files. Solo puede tener acceso al servicio Azure NetApp Files desde la misma red virtual o desde una red virtual que se encuentre en la misma ubicación que el volumen mediante el emparejamiento de redes virtuales. También puede tener acceso al volumen de la red local a través de Express Route.   

    * **Subred**  
        Especifique la subred que desea usar para el volumen.  
        La red virtual que especifique debe estar delegada en Azure NetApp Files. 
        
        Si no ha delegado una subred, haga clic en **Crear nuevo** en el volumen de creación de un volumen. A continuación, en la página de creación de la subred, especifique la información de la subred y seleccione **Microsoft.NetApp/volumes** para delegar la subred para Azure NetApp Files. En cada red virtual, solo una subred puede delegarse a Azure Files de NetApp.   
 
        ![Crear un volumen](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Creación de una subred](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

4. Haga clic en **protocolo**, a continuación, seleccione **NFS** como el tipo de protocolo para el volumen.   
    * Especifique el **ruta de acceso del archivo** que se usará para crear la ruta de acceso de exportación para el nuevo volumen. La ruta de acceso de exportación se usa para montar el volumen y tener acceso a él.

        El nombre de la ruta de acceso de archivo solo puede contener letras, números y guiones ("-"). El nombre debe tener entre 16 y 40 caracteres. 

        La ruta de acceso del archivo debe ser único dentro de cada suscripción y cada región. 

    * Si lo desea, [configurar Directiva de exportación para el volumen NFS](azure-netapp-files-configure-export-policy.md)

    ![Especifique el protocolo NFS](../media/azure-netapp-files/azure-netapp-files-protocol-nfs.png)

5. Haga clic en **revisar y crear** para revisar los detalles de volumen.  A continuación, haga clic en **crear** para crear el volumen NFS.

    El volumen que ha creado aparece en la página de volúmenes. 
 
    Un volumen hereda los atributos de ubicación, la suscripción y el grupo de recursos de su grupo de capacidad. Para supervisar el estado de la implementación del volumen, puede usar la pestaña Notificaciones.

## <a name="create-an-smb-volume"></a>Crear un volumen de SMB

Azure Files de NetApp admite volúmenes SMBv3. Deberá crear las conexiones de Active Directory antes de agregar un volumen de SMB. 

### <a name="create-an-active-directory-connection"></a>Crear una conexión de Active Directory

1. Asegúrese de que cumplen el requiements siguientes: 

    * La cuenta de administrador que use debe ser capaz de crear cuentas de equipo en la ruta de acceso de unidad organizativa (OU) que se especifican.
    * Los puertos adecuados deben estar abiertos en el servidor de Windows Active Directory (AD) correspondiente.  
        Los puertos necesarios son los siguientes: 

        |     Servicio           |     Port     |     Protocol     |
        |-----------------------|--------------|------------------|
        |    Servicios Web de AD    |    9389      |    TCP           |
        |    DNS                |    53        |    TCP           |
        |    DNS                |    53        |    UDP           |
        |    ICMPv4             |    N/D       |    Respuesta de eco    |
        |    Kerberos           |    464       |    TCP           |
        |    Kerberos           |    464       |    UDP           |
        |    Kerberos           |    88        |    TCP           |
        |    Kerberos           |    88        |    UDP           |
        |    LDAP               |    389       |    TCP           |
        |    LDAP               |    389       |    UDP           |
        |    LDAP               |    3268      |    TCP           |
        |    Nombre de NetBIOS       |    138       |    UDP           |
        |    SAM/LSA            |    445       |    TCP           |
        |    SAM/LSA            |    445       |    UDP           |
        |    LDAP seguro        |    636       |    TCP           |
        |    LDAP seguro        |    3269      |    TCP           |
        |    W32Time            |    123       |    UDP           |


1. En la cuenta de NetApp, haga clic en **conexiones de Active Directory**, a continuación, haga clic en **unir**.  

    ![Conexiones de Active Directory](../media/azure-netapp-files/azure-netapp-files-active-directory-connections.png)

2. En la ventana de unirse a Active Directory, proporcione la siguiente información:

    * **El servidor DNS principal**   
        Se trata de la dirección IP de controlador de dominio para el preferido Active Directory Domain Services para su uso con Azure Files de NetApp. 
    * **DNS secundario**  
        Se trata de la dirección IP de controlador de dominio para el secundario Active Directory Domain Services para su uso con Azure Files de NetApp. 
    * **Dominio**  
        Este es el nombre de dominio de Active Directory Domain Services que desea unir.
    * **Prefijo SMB (cuenta de equipo)**  
        Este es el prefijo de nomenclatura para la cuenta de equipo en Active Directory que va a usar para la creación de nuevas cuentas de Azure Files de NetApp.

        Por ejemplo, si el estándar de nomenclatura que su organización usa servidores de archivos es NAS-01, 02 NAS...,-045 NAS, a continuación, se escribiría "NAS" para el prefijo. 

        El servicio creará cuentas de equipo adicionales en Active Directory según sea necesario.

    * **Ruta de acceso de unidad organizativa**  
        Se trata de la ruta de acceso LDAP para la unidad organizativa (UO) donde se crearán las cuentas de equipo del servidor SMB. Es decir, OU = segundo nivel, OU = de primer nivel. 
    * Las credenciales, incluidos los **username** y **contraseña**

    ![Unir Active Directory](../media/azure-netapp-files/azure-netapp-files-join-active-directory.png)

3. Haga clic en **Unir**.  

    Aparece la conexión de Active Directory que creó.

    ![Conexiones de Active Directory](../media/azure-netapp-files/azure-netapp-files-active-directory-connections-created.png)

### <a name="add-an-smb-volume"></a>Agregar un volumen de SMB

1. Haga clic en el **volúmenes** hoja en la hoja de grupos de capacidad. 

    ![Vaya a volúmenes](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2. Haga clic en **+ Agregar volumen** para crear un volumen.  
    La creación, aparece una ventana de volumen.

3. En la creación de una ventana de volumen, haga clic en **crear** y proporcione información para los campos siguientes:   
    * **Nombre del volumen**      
        Especifique el nombre para el volumen que va a crear.   

        Un nombre de volumen debe ser único dentro de cada grupo de capacidades. Debe tener tres caracteres de longitud, como mínimo. Puede usar cualquier carácter alfanumérico.

    * **Grupo de capacidades**  
        Especifique el grupo de capacidad en la que desea que el volumen que se va a crear.

    * **Cuota**  
        Especifique la cantidad de almacenamiento lógico que se asigna al volumen.  

        El campo **Cuota disponible** muestra la cantidad de espacio no utilizado en el grupo de capacidad elegido que puede usar para crear un nuevo volumen. El tamaño del volumen nuevo no debe superar la cuota disponible.  

    * **Red virtual**  
        Especifique la red virtual de Azure (Vnet) desde la que desea tener acceso al volumen.  

        La red virtual que especifique debe tener una subred delegada en Azure NetApp Files. Solo puede tener acceso al servicio Azure NetApp Files desde la misma red virtual o desde una red virtual que se encuentre en la misma ubicación que el volumen mediante el emparejamiento de redes virtuales. También puede tener acceso al volumen de la red local a través de Express Route.   

    * **Subred**  
        Especifique la subred que desea usar para el volumen.  
        La red virtual que especifique debe estar delegada en Azure NetApp Files. 
        
        Si no ha delegado una subred, haga clic en **Crear nuevo** en el volumen de creación de un volumen. A continuación, en la página de creación de la subred, especifique la información de la subred y seleccione **Microsoft.NetApp/volumes** para delegar la subred para Azure NetApp Files. En cada red virtual, solo una subred puede delegarse a Azure Files de NetApp.   
 
        ![Crear un volumen](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Creación de una subred](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

4. Haga clic en **protocolo** y complete la siguiente información:  
    * Seleccione **SMB** como el tipo de protocolo para el volumen. 
    * Seleccione su **Active Directory** conexión en la lista desplegable.
    * Especifique el nombre del volumen compartido en **nombre del recurso compartido**.

    ![Especifique el protocolo SMB](../media/azure-netapp-files/azure-netapp-files-protocol-smb.png)

5. Haga clic en **revisar y crear** para revisar los detalles de volumen.  A continuación, haga clic en **crear** para crear el volumen SMB.

    El volumen que ha creado aparece en la página de volúmenes. 
 
    Un volumen hereda los atributos de ubicación, la suscripción y el grupo de recursos de su grupo de capacidad. Para supervisar el estado de la implementación del volumen, puede usar la pestaña Notificaciones.

## <a name="next-steps"></a>Pasos siguientes  

* [Montar o desmontar un volumen para máquinas virtuales Windows o Linux](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Configurar la directiva de exportación para un volumen NFS](azure-netapp-files-configure-export-policy.md)
* [Obtenga información sobre la integración de redes virtuales para los servicios de Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)
