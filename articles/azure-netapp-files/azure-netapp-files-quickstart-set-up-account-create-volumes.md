---
title: Configurar Azure Files de NetApp y crea un volumen | Microsoft Docs
description: Describe cómo configurar los archivos de NetApp Azure rápidamente y crear un volumen.
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
ms.topic: quickstarts
ms.date: 2/20/2019
ms.author: b-juche
ms.openlocfilehash: e2b9b3cdcb712fcf6c415f574dc687e80ae9ee3b
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/29/2019
ms.locfileid: "58660518"
---
# <a name="set-up-azure-netapp-files-and-create-a-volume"></a>Configuración de Azure NetApp Files y creación de un volumen 

Este artículo muestra cómo configurar los archivos de NetApp Azure rápidamente y crear un volumen. 

## <a name="before-you-begin"></a>Antes de empezar 

Tiene que formar parte del programa de la versión preliminar pública y figurar en la lista blanca para acceder al proveedor de recursos de Microsoft.NetApp. Para conocer más detalles sobre cómo unirse al programa de versión preliminar pública, visite la [página de registro en la versión preliminar pública de Azure NetApp Files](https://aka.ms/nfspublicpreview). 

## <a name="register-for-azure-netapp-files-and-netapp-resource-provider"></a>Regístrese para obtener los archivos de Azure de NetApp y proveedor de recursos de NetApp

1. En el portal de Azure, haga clic en el icono de Azure Cloud Shell en la esquina superior derecha.

      ![Icono de Azure Cloud Shell](../media/azure-netapp-files/azure-netapp-files-azure-cloud-shell.png)

2. Especifique la suscripción que ha estado en la lista blanca de NetApp de Azure Files:
    
        az account set --subscription <subscriptionId>

3. Registre el proveedor de recursos de Azure: 
    
        az provider register --namespace Microsoft.NetApp --wait  

    El proceso de registro puede tardar algún tiempo en completarse.

## <a name="create-a-netapp-account"></a>Creación de una cuenta de NetApp

1. En el cuadro de búsqueda del portal de Azure, escriba **Azure Files de NetApp** y, a continuación, seleccione **NetApp de Azure Files (versión preliminar)** en la lista que aparece.

      ![Seleccione los archivos de Azure de NetApp](../media/azure-netapp-files/azure-netapp-files-select-azure-netapp-files.png)

2. Haga clic en **+ Agregar** para crear una nueva cuenta de NetApp.

     ![Crear nueva cuenta de NetApp](../media/azure-netapp-files/azure-netapp-files-create-new-netapp-account.png)

3. En la ventana de la nueva cuenta de NetApp, proporcione la siguiente información: 
   1. Escriba **myaccount1** del nombre de cuenta. 
   2. Seleccione su suscripción.
   3. Seleccione **crear nuevo** para crear nuevo grupo de recursos. Escriba **myRG1** para el nombre del grupo de recursos. Haga clic en **OK**. 
   4. Seleccione la ubicación de la cuenta.  

      ![Nueva ventana de la cuenta de NetApp](../media/azure-netapp-files/azure-netapp-files-new-account-window.png)  

      ![Ventana de grupo de recursos](../media/azure-netapp-files/azure-netapp-files-resource-group-window.png)

4. Haga clic en **crear** para crear la nueva cuenta de NetApp.

## <a name="set-up-a-capacity-pool"></a>Configuración de un grupo de capacidad

1. En la hoja de administración de Azure Files de NetApp, seleccione la cuenta de NetApp (**myaccount1**).

    ![Seleccione la cuenta de NetApp](../media/azure-netapp-files/azure-netapp-files-select-netapp-account.png)  

2. En la hoja de administración de Azure Files de NetApp de la cuenta de NetApp, haga clic en **grupos de capacidad**.

    ![Haga clic en grupos de capacidad](../media/azure-netapp-files/azure-netapp-files-click-capacity-pools.png)  

3. Haga clic en **+ agregar grupos**. 

    ![Haga clic en Agregar grupos](../media/azure-netapp-files/azure-netapp-files-click-add-pools.png)  

4. Proporciona información para el grupo de capacidad: 
    1. Escriba **mypool1** como el nombre del grupo.
    2. Seleccione **Premium** para el nivel de servicio. 
    3. Especificar **4 (TiB)** como el tamaño del grupo. 

5. Haga clic en **OK**.

## <a name="create-a-volume-for-azure-netapp-files"></a>Creación de un volumen de Azure NetApp Files

1. En la hoja de administración de Azure Files de NetApp de la cuenta de NetApp, haga clic en **volúmenes**.

    ![Haga clic en los volúmenes](../media/azure-netapp-files/azure-netapp-files-click-volumes.png)  

2. Haga clic en **+ Agregar volumen**.

    ![Haga clic en Agregar volúmenes](../media/azure-netapp-files/azure-netapp-files-click-add-volumes.png)  

3. En la creación de una ventana de volumen, proporcione información para el volumen: 
   1. Escriba **myvol1** como el nombre del volumen. 
   2. Escriba **myfilepath1** como la ruta de acceso de archivo que se usará para crear la ruta de acceso de exportación para el volumen.
   3. Seleccione el grupo de capacidades (**mypool1**).
   4. Use el valor predeterminado para la cuota. 
   5. En la red virtual, haga clic en **crear nuevo** para crear una nueva red virtual de Azure (Vnet).  A continuación, rellene la información siguiente:
       * Escriba **myvnet1** como el nombre de red virtual.
       * Especifique un espacio de direcciones para la configuración, por ejemplo, 10.7.0.0/16
       * Escriba **myANFsubnet** como el nombre de subred.
       * Especifique el intervalo de direcciones de subred, por ejemplo, 10.7.0.0/24. Tenga en cuenta que no se puede compartir con otros recursos de la subred dedicada.
       * Seleccione **Microsoft.NetApp/volumes** para la delegación de la subred.
       * Haga clic en **Aceptar** para crear la red virtual.
   6. En la subred, seleccione la red virtual recién creada (**myvnet1**) como la subred de delegado.

      ![Crear una ventana de volumen](../media/azure-netapp-files/azure-netapp-files-create-volume-window.png)  

      ![Crear ventana de la red virtual](../media/azure-netapp-files/azure-netapp-files-create-virtual-network-window.png)  

4. Haga clic en **Revisar + crear**.

    ![Revisar y crear la ventana](../media/azure-netapp-files/azure-netapp-files-review-and-create-window.png)  

5. Revise la información para el volumen y, después, haga clic en **crear**.  
    El volumen creado aparece en la hoja volúmenes.

    ![Volumen creado](../media/azure-netapp-files/azure-netapp-files-create-volume-created.png)  

## <a name="next-steps"></a>Pasos siguientes  

* [Información sobre la jerarquía del almacenamiento de Azure NetApp Files](azure-netapp-files-understand-storage-hierarchy.md)
* [Administrar volúmenes mediante el uso de Azure Files de NetApp](azure-netapp-files-manage-volumes.md) 
