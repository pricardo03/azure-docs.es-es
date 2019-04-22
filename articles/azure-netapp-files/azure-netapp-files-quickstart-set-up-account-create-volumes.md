---
title: Configuración de Azure NetApp Files y creación de un volumen | Microsoft Docs
description: Describe cómo configurar Azure NetApp Files y crear un volumen rápidamente.
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
ms.date: 2/20/2019
ms.author: b-juche
ms.openlocfilehash: 634f23cf3161fff09f21c79fd8300cb269dcc5b7
ms.sourcegitcommit: 031e4165a1767c00bb5365ce9b2a189c8b69d4c0
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 04/13/2019
ms.locfileid: "59546590"
---
# <a name="set-up-azure-netapp-files-and-create-a-volume"></a>Configuración de Azure NetApp Files y creación de un volumen 

En este artículo se muestra cómo configurar Azure NetApp Files y crear un volumen rápidamente. 

## <a name="before-you-begin"></a>Antes de empezar 

Tiene que formar parte del programa de la versión preliminar pública y figurar en la lista blanca para acceder al proveedor de recursos de Microsoft.NetApp. Para conocer más detalles sobre cómo unirse al programa de versión preliminar pública, visite la [página de registro en la versión preliminar pública de Azure NetApp Files](https://aka.ms/nfspublicpreview). 

## <a name="register-for-azure-netapp-files-and-netapp-resource-provider"></a>Registro de Azure NetApp Files y del proveedor de recursos de NetApp

1. En Azure Portal, haga clic en el icono de Azure Cloud Shell en la esquina superior derecha.

      ![Icono de Azure Cloud Shell](../media/azure-netapp-files/azure-netapp-files-azure-cloud-shell.png)

2. Especifique la suscripción que se ha incluido en la lista blanca de Azure NetApp Files:
    
        az account set --subscription <subscriptionId>

3. Registre el proveedor de recursos de Azure: 
    
        az provider register --namespace Microsoft.NetApp --wait  

    El proceso de registro puede tardar algún tiempo en completarse.

## <a name="create-a-netapp-account"></a>Creación de una cuenta de NetApp

1. En el cuadro de búsqueda de Azure Portal, escriba **Azure NetApp Files** y seleccione **Azure NetApp Files (versión preliminar)** en la lista que aparece.

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

## <a name="create-a-volume-for-azure-netapp-files"></a>Creación de un volumen de Azure NetApp Files

1. En la hoja de administración de Azure NetApp Files de la cuenta de NetApp, haga clic en **Volúmenes**.

    ![Clic en Volúmenes](../media/azure-netapp-files/azure-netapp-files-click-volumes.png)  

2. Haga clic en **+ Agregar volumen**.

    ![Clic en Agregar volúmenes](../media/azure-netapp-files/azure-netapp-files-click-add-volumes.png)  

3. En la ventana Crear un volumen, especifique la información del volumen: 
   1. Escriba **myvol1** como nombre del volumen. 
   2. Escriba **myfilepath1** como la ruta de acceso de archivo que se usará para crear la ruta de acceso de exportación del volumen.
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

4. Haga clic en **Revisar + crear**.

    ![Ventana Revisar y crear](../media/azure-netapp-files/azure-netapp-files-review-and-create-window.png)  

5. Examine la información del volumen y haga clic en **Crear**.  
    El volumen creado aparece en la hoja Volúmenes.

    ![Volumen creado](../media/azure-netapp-files/azure-netapp-files-create-volume-created.png)  

## <a name="next-steps"></a>Pasos siguientes  

* [Información sobre la jerarquía del almacenamiento de Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Administración de volúmenes mediante Azure NetApp Files](azure-netapp-files-manage-volumes.md) 
