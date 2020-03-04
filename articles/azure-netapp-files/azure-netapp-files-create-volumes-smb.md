---
title: Creación de un volumen SMB para Azure NetApp Files | Microsoft Docs
description: Describe cómo crear un volumen SMB para Azure NetApp Files.
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
ms.date: 02/05/2020
ms.author: b-juche
ms.openlocfilehash: 7affd408ce2471f34a8362ba32101b639aafc514
ms.sourcegitcommit: 7f929a025ba0b26bf64a367eb6b1ada4042e72ed
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/25/2020
ms.locfileid: "77586619"
---
# <a name="create-an-smb-volume-for-azure-netapp-files"></a>Creación de un volumen de SMB para Azure NetApp Files

Azure NetApp Files admite volúmenes NFS y SMBv3. El consumo de la capacidad de un volumen se descuenta de la capacidad aprovisionada de su grupo. En este artículo se muestra cómo crear un volumen SMBv3. Si desea crear un volumen NFS, consulte [Creación de un volumen de Azure NetApp Files](azure-netapp-files-create-volumes.md). 

## <a name="before-you-begin"></a>Antes de empezar 
Debe haber configurado un grupo de capacidad.   
[Configuración de un grupo de capacidad](azure-netapp-files-set-up-capacity-pool.md)   
Debe haber una subred delegada en Azure NetApp Files.  
[Delegación de una subred en Azure NetApp Files](azure-netapp-files-delegate-subnet.md)

## <a name="requirements-for-active-directory-connections"></a>Requisitos para las conexiones de Active Directory

 Debe crear las conexiones de Active Directory antes de crear un volumen SMB. Los requisitos para las conexiones de Active Directory son los siguientes: 

* La cuenta de administrador que use debe ser capaz de crear cuentas de máquina en la ruta de acceso de la unidad organizativa (OU) que se especifique.  

* Los puertos adecuados deben estar abiertos en el servidor de Windows Active Directory (AD) correspondiente.  
    Los puertos necesarios son los siguientes: 

    |     Servicio           |     Port     |     Protocolo     |
    |-----------------------|--------------|------------------|
    |    Servicios web de AD    |    9389      |    TCP           |
    |    DNS                |    53        |    TCP           |
    |    DNS                |    53        |    UDP           |
    |    ICMPv4             |    N/D       |    Echo Reply    |
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
    |    w32time            |    123       |    UDP           |

* La topología del sitio para la instancia de Active Directory Domain Services de destino debe cumplir los procedimientos recomendados, en particular, la red virtual de Azure donde se implementa Azure NetApp Files.  

    El espacio de direcciones de la red virtual donde se implementa Azure NetApp Files debe agregarse a un sitio de Active Directory nuevo o existente (donde resida un controlador de dominio al que Azure NetApp Files pueda acceder). 

* Los servidores DNS especificados deben ser accesibles desde la [subred delegada](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-delegate-subnet) de Azure NetApp Files.  

    Consulte [Directrices para el planeamiento de red de Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-network-topologies) para ver topologías de red admitidas.

    Los grupos de seguridad de red (NSG) y los firewalls deben tener reglas configuradas correctamente para permitir solicitudes de Active Directory y de tráfico DNS. 

* La subred delegada en Azure NetApp Files debe poder tener acceso a todos los controladores de dominio Active Directory Domain Services (ADDS) del dominio, incluidos todos los controladores de dominio locales y remotos. De lo contrario, puede producirse una interrupción del servicio.  

    Si tiene controladores de dominio a los que no se puede acceder mediante la subred delegada de Azure NetApp Files, puede especificar un sitio de Active Directory durante la creación de la conexión a Active Directory.  Azure NetApp Files solo debe comunicarse con los controladores de dominio del sitio en el que reside el espacio de direcciones de la subred delegada en Azure NetApp Files.

    Consulte [Diseño de la topología de sitio](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/designing-the-site-topology) para obtener información acerca de los sitios y servicios de AD. 
    
Consulte las [Preguntas más frecuentes de SMB](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-faqs#smb-faqs) de Azure NetApp Files para obtener información adicional de AD. 

## <a name="create-an-active-directory-connection"></a>Creación de una conexión de Active Directory

1. En la cuenta de NetApp, haga clic en **Conexiones de Active Directory** y, a continuación, haga clic en **Unir**.  

    ![Conexiones de Active Directory](../media/azure-netapp-files/azure-netapp-files-active-directory-connections.png)

2. En la ventana Unir Active Directory, especifique la siguiente información:

    * **DNS principal**  
        Se trata del DNS que se requiere para las operaciones de autenticación de SMB y la unión a un dominio de Active Directory. 
    * **DNS secundario**   
        Este es el servidor DNS secundario para garantizar los servicios de nombre redundantes. 
    * **Nombre de dominio DNS de AD**  
        Se trata del nombre de dominio de Active Directory Domain Services al que desea unirse.
    * **Nombre de sitio de AD**  
        Este es el nombre del sitio al que se limitará la detección de controladores de dominio.
    * **Prefijo SMB (cuenta de equipo)**  
        Este es el prefijo de nomenclatura para la cuenta de máquina en Active Directory que Azure NetApp Files va a usar para la creación de nuevas cuentas.

        Por ejemplo, si el estándar de nomenclatura que su organización usa para los servidores de archivos es NAS-01, NAS-02...,NAS-045, escribiría "NAS" para el prefijo. 

        El servicio creará cuentas de máquina adicionales en Active Directory según sea necesario.

    * **Ruta de acceso de unidad organizativa**  
        Se trata de la ruta de acceso LDAP para la unidad organizativa (UO) donde se crearán las cuentas de máquina del servidor SMB. Es decir, UO=segundo nivel, UO=primer nivel. 

        Si usa Azure NetApp Files con Azure Active Directory Domain Services, la ruta de acceso de la unidad organizativa es `OU=AADDC Computers` cuando configura Active Directory para su cuenta de NetApp.
        
    * Las credenciales, incluidos el **nombre de usuario** y la **contraseña**

    ![Unir Active Directory](../media/azure-netapp-files/azure-netapp-files-join-active-directory.png)

3. Haga clic en **Unir**.  

    Aparece la conexión de Active Directory que creó.

    ![Conexiones de Active Directory](../media/azure-netapp-files/azure-netapp-files-active-directory-connections-created.png)

> [!NOTE] 
> Puede editar los campos de nombre de usuario y contraseña después de guardar la conexión de Active Directory. No se puede editar ningún otro valor después de guardar la conexión. Si necesita cambiar cualquier otro valor, primero debe eliminar todos los volúmenes SMB implementados y, a continuación, eliminar y volver a crear la conexión de Active Directory.

## <a name="add-an-smb-volume"></a>Incorporación de un volumen SMB

1. Haga clic en la hoja **Volúmenes** desde la hoja Grupos de capacidad. 

    ![Vaya a Volúmenes](../media/azure-netapp-files/azure-netapp-files-navigate-to-volumes.png)

2. Haga clic en **+ Agregar volumen** para crear un volumen.  
    Aparece la ventana Crear un volumen.

3. En la ventana Crear un volumen, haga clic en **Crear** y proporcione la información para los campos siguientes:   
    * **Nombre del volumen**      
        Especifique el nombre para el volumen que va a crear.   

        Un nombre de volumen debe ser único dentro de cada grupo de capacidad. Debe tener tres caracteres de longitud, como mínimo. Puede usar cualquier carácter alfanumérico.   

        No se puede usar `default` como nombre del volumen.

    * **Grupo de capacidad**  
        Especifique el grupo de capacidad en el que desee que el volumen se cree.

    * **Cuota**  
        Especifique la cantidad de almacenamiento lógico que se asigna al volumen.  

        El campo **Cuota disponible** muestra la cantidad de espacio no utilizado en el grupo de capacidad elegido que puede usar para crear un nuevo volumen. El tamaño del volumen nuevo no debe superar la cuota disponible.  

    * **Red virtual**  
        Especifique la red virtual de Azure desde la que desea tener acceso al volumen.  

        La red virtual que especifique debe tener una subred delegada en Azure NetApp Files. Solo puede tener acceso al servicio Azure NetApp Files desde la misma red virtual o desde una red virtual que se encuentre en la misma ubicación que el volumen mediante el emparejamiento de redes virtuales. También puede acceder al volumen desde la red local mediante ExpressRoute.   

    * **Subred**  
        Especifique la subred que desea usar para el volumen.  
        La red virtual que especifique debe estar delegada en Azure NetApp Files. 
        
        Si no ha delegado una subred, haga clic en **Crear nuevo** en el volumen de creación de un volumen. A continuación, en la página de creación de la subred, especifique la información de la subred y seleccione **Microsoft.NetApp/volumes** para delegar la subred para Azure NetApp Files. En cada red virtual, solo puede delegarse una subred a Azure NetApp Files.   
 
        ![Crear un volumen](../media/azure-netapp-files/azure-netapp-files-new-volume.png)
    
        ![Creación de una subred](../media/azure-netapp-files/azure-netapp-files-create-subnet.png)

4. Haga clic en **Protocolo** y complete la siguiente información:  
    * Seleccione **SMB** como tipo de protocolo para el volumen. 
    * Seleccione la conexión de **Active Directory** en la lista desplegable.
    * Especifique el nombre del volumen compartido en **Nombre del recurso compartido**.

    ![Especifique el protocolo SMB](../media/azure-netapp-files/azure-netapp-files-protocol-smb.png)

5. Haga clic en **Revisar y crear** para revisar los detalles del volumen.  Haga clic en **Crear** para crear el volumen SMB.

    El volumen que creó aparece en la hoja Volúmenes. 
 
    Un volumen hereda los atributos de ubicación, la suscripción y el grupo de recursos de su grupo de capacidad. Para supervisar el estado de la implementación del volumen, puede usar la pestaña Notificaciones.

## <a name="next-steps"></a>Pasos siguientes  

* [Montaje o desmontaje de un volumen para máquinas virtuales Windows o Linux](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Límites de recursos para Azure NetApp Files](azure-netapp-files-resource-limits.md)
* [Preguntas más frecuentes de SMB](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-faqs#smb-faqs)
* [Obtenga información sobre la integración de redes virtuales para los servicios de Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-for-azure-services)
* [Install a new Active Directory forest using Azure CLI](https://docs.microsoft.com/windows-server/identity/ad-ds/deploy/virtual-dc/adds-on-azure-vm) (Instalación de un nuevo bosque de Active Directory en la CLI de Azure)
