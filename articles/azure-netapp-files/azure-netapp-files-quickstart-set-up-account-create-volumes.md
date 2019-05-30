---
title: 'Inicio rápido: Configuración de Azure NetApp Files y creación de un volumen de NFS | Microsoft Docs'
description: En este inicio rápido se describe cómo configurar Azure NetApp Files y crear un volumen rápidamente.
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
ms.topic: quickstart
ms.date: 4/16/2019
ms.author: b-juche
ms.openlocfilehash: 4ea511bec75557bc6f7d37b1724b4b0db65ba9cc
ms.sourcegitcommit: 8c49df11910a8ed8259f377217a9ffcd892ae0ae
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/29/2019
ms.locfileid: "66299415"
---
# <a name="quickstart-set-up-azure-netapp-files-and-create-an-nfs-volume"></a>Inicio rápido: Configuración de Azure NetApp Files y creación de un volumen de NFS 

En este artículo se muestra cómo configurar Azure NetApp Files y crear un volumen rápidamente. 

En este inicio rápido, configurará los elementos siguientes:

- Registro de Azure NetApp Files y proveedor de recursos de NetApp
- Cuenta de NetApp
- Grupo de capacidad
- Volumen de NFS para Azure NetApp Files

Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar.

## <a name="before-you-begin"></a>Antes de empezar 

> [!IMPORTANT] 
> Se le debe conceder acceso al servicio Azure Files de NetApp.  Para solicitar acceso al servicio, consulte la [página de envío de la lista de espera de Azure Files de NetApp](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR8cq17Xv9yVBtRCSlcD_gdVUNUpUWEpLNERIM1NOVzA5MzczQ0dQR1ZTSS4u).  Antes de continuar, debe esperar un correo electrónico de confirmación oficial del equipo de Azure NetApp Files. 

## <a name="register-for-azure-netapp-files-and-netapp-resource-provider"></a>Registro de Azure NetApp Files y del proveedor de recursos de NetApp

1. En Azure Portal, haga clic en el icono de Azure Cloud Shell en la esquina superior derecha.

    ![Icono de Azure Cloud Shell](../media/azure-netapp-files/azure-netapp-files-azure-cloud-shell-window.png)

2. Especifique la suscripción que se ha incluido en la lista blanca de Azure NetApp Files:
    
        az account set --subscription <subscriptionId>

3. Registre el proveedor de recursos de Azure: 
    
        az provider register --namespace Microsoft.NetApp --wait  

    El proceso de registro puede tardar algún tiempo en completarse.

## <a name="create-a-netapp-account"></a>Creación de una cuenta de NetApp

1. En el cuadro de búsqueda de Azure Portal, escriba **Azure NetApp Files** y seleccione **Azure NetApp Files** en la lista que aparece.

      ![Seleccionar Azure NetApp Files](../media/azure-netapp-files/azure-netapp-files-select-azure-netapp-files.png)

2. Haga clic en **+ Agregar** para crear una nueva cuenta de NetApp.

     ![Crear una cuenta de NetApp](../media/azure-netapp-files/azure-netapp-files-create-new-netapp-account.png)

3. En la ventana Nueva cuenta de NetApp, especifique la siguiente información: 
   1. Escriba **myaccount1** como nombre de la cuenta. 
   2. Seleccione su suscripción.
   3. Selección **Crear nuevo** para crear un grupo de recursos. Escriba **myRG1** como nombre del grupo de recursos. Haga clic en **OK**. 
   4. Seleccione la ubicación de la cuenta.  

      ![Ventana Nueva cuenta de NetApp](../media/azure-netapp-files/azure-netapp-files-new-account-window.png)  

      ![Ventana de grupo de recursos](../media/azure-netapp-files/azure-netapp-files-resource-group-window.png)

4. Haga clic en **Crear** para crear una cuenta de NetApp.

## <a name="set-up-a-capacity-pool"></a>Configuración de un grupo de capacidad

1. En la hoja de administración de Azure NetApp Files, seleccione la cuenta de NetApp (**myaccount1**).

    ![Seleccionar cuenta de NetApp](../media/azure-netapp-files/azure-netapp-files-select-netapp-account.png)  

2. En la hoja de administración de Azure NetApp Files de la cuenta de NetApp, haga clic en **Grupos de capacidad**.

    ![Clic en Grupos de capacidad](../media/azure-netapp-files/azure-netapp-files-click-capacity-pools.png)  

3. Haga clic en **+ Agregar grupos**. 

    ![Clic en Agregar grupos](../media/azure-netapp-files/azure-netapp-files-click-add-pools.png)  

4. Especifique la información del grupo de capacidad: 
    1. Escriba **mypool1** como nombre del grupo.
    2. Seleccione **Premium** como nivel de servicio. 
    3. Especifique **4 (TiB)** como tamaño del grupo. 

5. Haga clic en **OK**.

## <a name="create-an-nfs-volume-for-azure-netapp-files"></a>Creación de un volumen de NFS para Azure NetApp Files

1. En la hoja de administración de Azure NetApp Files de la cuenta de NetApp, haga clic en **Volúmenes**.

    ![Clic en Volúmenes](../media/azure-netapp-files/azure-netapp-files-click-volumes.png)  

2. Haga clic en **+ Agregar volumen**.

    ![Clic en Agregar volúmenes](../media/azure-netapp-files/azure-netapp-files-click-add-volumes.png)  

3. En la ventana Crear un volumen, especifique la información del volumen: 
   1. Escriba **myvol1** como nombre del volumen. 
   3. Seleccione el grupo de capacidad (**mypool1**).
   4. Use el valor predeterminado de la cuota. 
   5. En la red virtual, haga clic en **Crear nueva** para crear una red virtual de Azure.  Luego, rellene la siguiente información:
       * Escriba **myvnet1** como nombre de la red virtual.
       * Especifique un espacio de direcciones para el valor, por ejemplo, 10.7.0.0/16
       * Escriba **myANFsubnet** como nombre de subred.
       * Especifique el intervalo de direcciones de la subred, por ejemplo, 10.7.0.0/24. Tenga en cuenta que la subred dedicada no se puede compartir con otros recursos.
       * Seleccione **Microsoft.NetApp/volumes** como delegación de la subred.
       * Haga clic en **Aceptar** para crear la red virtual.
   6. En la subred, seleccione la red virtual recién creada (**myvnet1**) como subred de delegado.

      ![Ventana Crear un volumen](../media/azure-netapp-files/azure-netapp-files-create-volume-window.png)  

      ![Ventar Crear una red virtual](../media/azure-netapp-files/azure-netapp-files-create-virtual-network-window.png)  

4. Haga clic en **Protocolo** y seleccione **NFS** como el tipo de protocolo del volumen.   

    Escriba **myfilepath1** como la ruta de acceso de archivo que se usará para crear la ruta de acceso de exportación del volumen. 

    ![Especificación del protocolo de NFS para el inicio rápido](../media/azure-netapp-files/azure-netapp-files-quickstart-protocol-nfs.png)

5. Haga clic en **Revisar + crear**.

    ![Ventana Revisar y crear](../media/azure-netapp-files/azure-netapp-files-review-and-create-window.png)  

5. Examine la información del volumen y haga clic en **Crear**.  
    El volumen creado aparece en la hoja Volúmenes.

    ![Volumen creado](../media/azure-netapp-files/azure-netapp-files-create-volume-created.png)  

## <a name="clean-up-resources"></a>Limpieza de recursos

Cuando haya terminado, y si lo desea, puede eliminar el grupo de recursos. La acción de eliminar un grupo de recursos es irreversible.  

> [!IMPORTANT]
> Todos los recursos dentro de los grupos de recursos se eliminarán de forma permanente y esta acción no se puede deshacer. 

1. En el cuadro de búsqueda de Azure Portal, escriba **Azure NetApp Files** y seleccione **Azure NetApp Files** en la lista que aparece.

2. En la lista de suscripciones, haga clic en el grupo de recursos (myRG1) que quiere eliminar. 

    ![Navegación a los grupos de recursos](../media/azure-netapp-files/azure-netapp-files-azure-navigate-to-resource-groups.png)


3. En la página del grupo de recursos, seleccione **Eliminar grupo de recursos**.

    ![Eliminación de un grupo de recursos](../media/azure-netapp-files/azure-netapp-files-azure-delete-resource-group.png) 

    Se abre una ventana con una advertencia acerca de los recursos que se eliminarán con el grupo de recursos.

4. Escriba el nombre del grupo de recursos (myRG1) para confirmar que quiere eliminar de forma permanente el grupo de recursos y todos los recursos que contiene y, luego, haga clic en **Eliminar**.

    ![Eliminación de un grupo de recursos](../media/azure-netapp-files/azure-netapp-files-azure-confirm-resource-group-deletion.png ) 

## <a name="next-steps"></a>Pasos siguientes  

> [!div class="nextstepaction"]
> [Administración de volúmenes mediante Azure NetApp Files](azure-netapp-files-manage-volumes.md)  
